---
title: "On Parsing Time Zones"
date: 2017-02-08T23:24:53-07:00
tags:
  - social-networks
  - eksisozluk
  - perl
  - time-zone
---

_I posted this to [GitHub](https://github.com/kyzn/WWW-Eksi/commit/667882c7) first, later edited and moved here._

## Summary

This post is about a CPAN module I built, called [WWW::Eksi](https://metacpan.org/pod/WWW::Eksi). Module parses posts in a Turkish social network called [Eksisozluk](https://eksisozluk.com). All posts come with an attached "creation time", and some also include "last update time". I realized [DateTime](https://metacpan.org/pod/DateTime) was throwing some errors for some of the posts. It turned out these posts had an attached time that never existed in Turkish time zone.

- This social network do not reveal time zone information to its users.
- Their daylight saving time (DST) changes were out of sync with reality of the country.
- In some cases, we can use some workarounds to figure out the correct time zone.
- But this is not true for all cases.
- This is why I stopped including time zone information in my module's output.

## Background

Turkey had daylight saving time since the 1940s. This means we had a "spring forward" day in March, and a "fall back" day in October.

Spring forward day is 23 hours. Clocks go from `00:59:59` to `02:00:00`. There is no `01:30:00` on that day. This means if you find a post in this social network that has a timestamp of `01:30:00`, then you are in trouble. I would like to call this nonexisting hour "NEVER-ZONE".

Fall back day is 25 hours. Clocks go from `01:59:59` to `01:00:00`. There are two `01:30:00`s on that day. If you find a post with this time, you need to figure out whether it's `01:30:00 (+3)` or `01:30:00 (+2)`. Let's call this interval "AMBIGUOUS-ZONE".

Let's look at what we can do to fix this problem.

## Fixing Spring Forward with Try-Catch

If daylight saving rules were in sync, we wouldn't have any problem figuring out time zone.

| Post ID | Turkey Time   | Eksisozluk Time |      Comment       |
| ------- | ------------- | --------------- | ------------------ |
|   ...   |     ...       |      ...        |                    |
|   101   | 00:59:59 (+2) |  00:59:59 (+2)  | Both change TZ     |
|   102   | 02:00:00 (+3) |  02:00:00 (+3)  | at the same time   |
|   ...   |     ...       |      ...        |                    |
|   111   | 02:59:59 (+3) |  02:59:59 (+3)  | They are in        |
|   112   | 03:00:00 (+3) |  03:00:00 (+3)  | sync all the time  |
|   ...   |     ...       |      ...        |                    |

But we know that these rules were out of sync for 2001 through 2006. Turkey had observed daylight saving at `01:00:00`, whereas Eksisozluk had it at `02:00:00`.

| Post ID | Turkey Time   | Eksisozluk Time |       Comment      |
| ------- | ------------- | --------------- | ------------------ |
|   ...   |     ...       |      ...        |                    |
|   201   | 00:59:59 (+2) |  00:59:59 (+2)  | In sync up to here |
|   202   | 02:00:00 (+3) |  01:00:00 (+2)  |   **NEVER-ZONE**   |
|   ...   |     ...       |      ...        |   **NEVER-ZONE**   |
|   211   | 02:59:59 (+3) |  01:59:59 (+2)  |   **NEVER-ZONE**   |
|   212   | 03:00:00 (+3) |  03:00:00 (+3)  |    In sync again   |
|   ...   |     ...       |      ...        |                    |

If you try to create a `DateTime` object in Never-Zone, it will throw an error.

    $ reply
    0> use DateTime;
    1> my $dt = DateTime->new(year=>2003, month=>3, day=>30, hour=>1, minute=>30, time_zone=>'Europe/Istanbul');
    Invalid local time for date in time zone: Europe/Istanbul
    2>

Here's the solution: We can `catch` that error, increment `hour` by one, and create `DateTime` object again. By doing so we will have the correct time zone data, even though that was not what Eksisozluk displayed to us.

I mentioned some posts have two times: One for creation, and another one for update. This solution works for both, so we are good here.

## Fixing Fall Back with Post IDs

Even if daylight saving rules were in sync, we still end up with an Ambiguous-Zone. This is because Eksisozluk does not show us time zone alongside with time.

| Post ID | Turkey Time   | Eksisozluk Time |      Comment       |
| ------- | ------------- | --------------- | ------------------ |
|   ...   |     ...       |      ...        |                    |
|   301   | 00:59:59 (+3) |  00:59:59 (+3)  |                    |
|   302   | 01:00:00 (+3) |  01:00:00 (+3)  | **AMBIGUOUS-ZONE** |
|   ...   |     ...       |      ...        | **AMBIGUOUS-ZONE** |
|   311   | 01:59:59 (+3) |  01:59:59 (+3)  | **AMBIGUOUS-ZONE** |
|   312   | 01:00:00 (+2) |  01:00:00 (+2)  | **AMBIGUOUS-ZONE** |
|   ...   |     ...       |      ...        | **AMBIGUOUS-ZONE** |
|   321   | 01:59:59 (+2) |  01:59:59 (+2)  | **AMBIGUOUS-ZONE** |
|   322   | 02:00:00 (+2) |  02:00:00 (+2)  |                    |
|   ...   |     ...       |      ...        |                    |

If you try to create a `DateTime` object in Ambiguous-Zone, it will use latest possible time zone.

    $ reply
    0> use DateTime;
    1> my $dt = DateTime->new(year=>2003, month=>10, day=>26, hour=>1, minute=>30, time_zone=>'Europe/Istanbul'); 1
    $res[0] = 1

    2> $dt->offset / 3600 # offset is in seconds, divide to get hours (+2)
    $res[1] = '2'

    3>

If you parse post #302 and post #312, both of them will have the same time zone, +2. But we do know posts up to #311 are in +3. We can use these IDs to decide time zone of the post. Problem solved for creation time, but this doesn't work for update time. If a post has an update time in Ambiguous-Zone, there is no way for us to know its time zone.

But we know daylight saving rules were not in sync.


| Post ID | Turkey Time   | Eksisozluk Time |      Comment       |
| ------- | ------------- | --------------- | ------------------ |
|   ...   |     ...       |      ...        |                    |
|   401   | 00:59:59 (+3) |  00:59:59 (+3)  |                    |
|   402   | 01:00:00 (+3) |  01:00:00 (+3)  |                    |
|   ...   |     ...       |      ...        |                    |
|   411   | 01:59:59 (+3) |  01:59:59 (+3)  |                    |
|   412   | 01:00:00 (+2) |  02:00:00 (+3)  | **AMBIGUOUS-ZONE** |
|   ...   |     ...       |      ...        | **AMBIGUOUS-ZONE** |
|   421   | 01:59:59 (+2) |  02:59:59 (+3)  | **AMBIGUOUS-ZONE** |
|   422   | 02:00:00 (+2) |  02:00:00 (+2)  | **AMBIGUOUS-ZONE** |
|   ...   |     ...       |      ...        | **AMBIGUOUS-ZONE** |
|   431   | 02:59:59 (+2) |  02:59:59 (+2)  | **AMBIGUOUS-ZONE** |
|   432   | 03:00:00 (+2) |  03:00:00 (+2)  |                    |
|   ...   |     ...       |      ...        |                    |

Turkey time has two `01:30:00`s, and Eksisozluk time has two `02:30:00`s. Since our goal is to parse Eksisozluk, I've marked Eksisozluk's Ambiguous-Zone in table.

If we are seeing `01:30:00` in a post, this can only mean +3. DateTime will assume it's +2, but we can subtract one hour to fix. This works both for creation time and update time.

If we are seeing `02:30:00` in a post, this is the Ambiguous-Zone of Eksisozluk time. DateTime will assume it's +2, because there is only one `02:30:00` in Turkey time. We can get rid of ambiguity by looking at post IDs. If it's between #412 and #421, we know it is `01:30:00 (+2)`, even though that is not the time displayed on Eksisozluk. Otherwise, we can leave it as `02:30:00 (+2)`. This solves the problem for creation time, but not for update time.

## Conclusion

- We can solve spring forward problem with try-catch. This works for both creation and update time.
- We can solve part of fall back problem by adding one hour. This also works for both creation and update time.
- We can solve another part of fall back problem by hardcoding some post IDs into module. This works only for creation time. There is no way to fix this for update time.
- This problem applies only to a very small subset of posts. This gave me three options.
  - Hardcode post IDs into the module to fix creation time.
  - Drop time zone support for the affected time intervals.
  - Drop time zone support completely.

I picked the last one.
