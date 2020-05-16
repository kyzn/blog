---
title: "Log In to E-Government with Id Card"
date: 2020-05-16T15:30:00+03:00
tags:
  - ubuntu
  - smart-card
---

![](/images/edevlet-menu.png)

Turkish E-Government is a great web application with quite a number of features. There's many ways to login, one of which involves using a card reader. I have a mini card reader at home that I got from Estonian E-Residency.

Things I used:
- Ubuntu 19.10
- Estonia E-Residency USB card reader
- Turkish ID card
- and its PIN number

# Step 1

See if the card reader shows up in `lsusb`. Run `lsusb` before plugging the card reader in. And then re-run it after plugging in. You should notice an extra item in the list.

    Advanced Card Systems, Ltd ACR38 SmartCard Reader

Another fun way to do this: Run `watch lsusb` and watch the screen as you plug/unplug your card reader.

# Step 2

You need to download the E-ID application from https://cdn.e-devlet.gov.tr/downloads/e-kimlik/edevlet-ekimlik.jnlp. That's going to be a JNLP file, and you will need `icedtea-netx` to open it in Ubuntu. Get it with:

    $ sudo apt-get install icedtea-netx


# Step 3

`lsusb` recognizing the card reader doesn't mean the E-ID application can read from it. So we need some more libraries.

    $ sudo apt-get install pcscd pcsc-tools

This brings in a useful command called `pcsc_scan`. You can use it to see if your card reader actually works. So go ahead, remove your card reader if connected and run the following. If you don't want to play around, **feel free to [skip to Step 4](#step-4).**

    $ pcsc_scan

The app will keep running until you send a `^C`. First you should see something like this:

    Scanning present readers...
    Waiting for the first reader...

When you plug your card reader in, it should update the last line as:

    Waiting for the first reader...found one

If it had no cards in the reader, you should see:

    Card state: Card removed,

And when you insert a card,

    Card state: Card inserted,
    ATR: 01 23 45 67 89 AB CD EF .. .. .. ..

I actually got an error when I inserted my card.

    Can't locate Chipcard/PCSC/Card.pm in @INC

And that happened because I was using `perlbrew` instead of system Perl. So `libpcsc-perl` was installed, but my Perl installation was not pointing to it. So all I had to do was to run `perlbrew switch-off` and restart `pcsc_scan`.

    $ which perl
    /home/kivanc/perl5/perlbrew/perls/perl-5.30.2/bin/perl
    $ perlbrew switch-off
    perlbrew is switched off.
    $ which perl
    /usr/bin/perl

Don't forget to `perlbrew switch` back on once you are done.

# Step 4

Go to https://giris.turkiye.gov.tr/Giris/T-C-Kimlik-Karti and enter your ID number. This will print a transaction code. Copy the code and keep the browser tab open.

![](/images/edevlet-giris.png)
<center><h6>Left: Input ID number and click continue. Right: You will see a transaction code.</h6></center>

# Step 5

Plug in the card and card reader, and launch the JNLP file. You can do so by double clicking the file, or running this:

    $ javaws edevlet-ekimlik.jnlp

If it's the first time you are running this app, it may ask you about permissions and shortcuts. I chose not to create a desktop shortcut.

# Step 6

Enter the transaction code from Step 4 into the app. Click "login with standard card reader".

![](/images/edevlet-jnlp-menu.png)

# Step 7

Now it will ask you to enter the PIN code of your ID card. Remember the envelope you got when you applied for the card? This number should be in there.

![](/images/edevlet-jnlp-pin.png)

# Step 8

You will see a message saying it's done. Check your browser to see if it actually worked. You can close the app and unplug the card reader.

![](/images/edevlet-jnlp-done.png)
