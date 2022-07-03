This is hugo repository for [kivanc's blog](https://kyzn.org/). Generated content (published on the blog) can be found in `/docs` folder.

## Quick notes

- Testing locally

      hugo server

- Testing locally with posts with future date

      hugo server --buildFuture

- Testing locally with drafts

      hugo server --buildDrafts

- Then go to

      http://localhost:1313/

- Generate `docs/` and commit

      rm -rf docs/ && hugo -d docs/ && echo -n "kyzn.org" > docs/CNAME && git add docs/ && git commit -m "generate docs"

- Add a new post

      hugo new posts/my-new-blog-post.md

- Add a new page

      hugo new my-new-page.md


## Installing hugo

- Install hugo

      snap install hugo

- Verify hugo version

      hugo version
