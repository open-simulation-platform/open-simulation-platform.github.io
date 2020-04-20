# local dev

Local development of github pages using Jekyll is described here: https://help.github.com/en/github/working-with-github-pages/testing-your-github-pages-site-locally-with-jekyll

To avoid installing Ruby and Bundler, the starefossen/github-pages docker image is a good alternative.
Get docker at https://docs.docker.com/

`docker pull starefossen/github-pages`

`docker run -it --rm -v c:\path\to\open-simulation-platform.github.io\:/usr/src/app -p "4000:4000" starefossen/github-pages`

For some fields you'll need a personal access token from github. See how to get it [here](
https://github.com/jekyll/github-metadata/blob/master/docs/authentication.md).
To use it run the docker container with the following option `-e JEKYLL_GITHUB_TOKEN=****`

`docker pull jekyll/jekyll:3.8`

`docker run --rm --volume="c:\path\open-simulation-platform.github.io:/srv/jekyll" -e JEKYLL_GITHUB_TOKEN=*************** -p 4000:4000 -it jekyll/jekyll:3.8 bash -c "bundle update && bundle install && bundle exec jekyll serve -H 0.0.0.0"`