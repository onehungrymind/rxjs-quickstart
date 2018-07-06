# RxJS Quickstart
![image](https://cloud.githubusercontent.com/assets/1544557/23685335/bb612184-0360-11e7-9ce8-f541afa04ee5.png)

This repo contains some introductory examples of pairing Angular with RxJS.

It covers many of the core scenarios that Angular developers would encounter in the wild.

## Development
This site is built with Jekyll, so you will need a couple items to get started.

### Prerequisites
- Install Ruby via `brew install ruby` (on Mac).
- Install the `jekyll` and `bundler` gems via `gem install jekyll bundler`.

### Getting started
```
git clone https://github.com/onehungrymind/rxjs-quickstart.git
cd rxjs-quickstart
gem install
bundle exec jekyll serve
```

The site will now be running on `localhost:4000`.

---
## Deploying
We use [surge.sh](http://surge.sh/) to host this site.

### Prerequisites
You will need to have NodeJS and NPM installed.

We recommend installing it via [NVM](https://github.com/creationix/nvm), but you can also install it via `brew` (on Mac).

Once Node and NPM are installed, install surge.sh by running `npm i -g surge`.

### Deploying to Surge
We have created a script in this directory called `deploy.sh` that deploys to surge.
The script uses the value in the `CNAME` file in this directory to automatically detect which domain is pushed to.
To run the script, simply execute `./deploy.sh` in this directory.
If you want to change the deploy destination, run the following:
```
bundle exec jekyll build # generate the necessary static assets
surge _site <your-domain.surge.sh> # upload assets to surge
```