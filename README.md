This is hugo repository for [kivanc's blog](https://kyzn.org/). Generated content (published on the blog) can be found in `/docs` folder.

## Quick notes

- Testing locally

      hugo server -p 3000

- Generate content into `docs/` folder

      hugo -d docs/

- Add a new post

      hugo new posts/my-new-blog-post.md

- Add a new page

      hugo new my-new-page.md


## Installing hugo

- Install linuxbrew

      sh -c "$(curl -fsSL https://raw.githubusercontent.com/Linuxbrew/install/master/install.sh)"

- Make sure it's in `$PATH`

- Install hugo

      brew install hugo

- Verify hugo version

      hugo version
