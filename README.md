# nginx-basic-config

This repo contains an incredibly minimalist `nginx.conf` file that serves a very specific purpose:
it puts all resources for a website under a common host (domain:port), using the path to determine
which server to pull from. There are four different resources being proxied:

* NextJS REST API, located at `localhost:3000/api`
* NextJS web pages, located at `localhost:3000` (everything except /api/)
* NextJS websocket, located at `localhost:3000/_next`
* New API, located at `localhost:3001/api2`

There are multiple reasons to put all of these resources behind a reverse proxy:

* I want to be able to set cookies in the new API server, which isn't possible if it's on a different host
* I don't want to have to set all sorts of CORS-related headers to get the client-server communication working
* The production environment will have everything under an application load balancer (ALB) that will handle
all the proxying (minus the websocket), and I want my dev and prod environments to be as close as possible

# Installation

N.B. This is tested on a Mac. If you're using this on something different, you will have to change these
commands to match.

Use brew to install `nginx`:

```
brew update
brew install nginx
```

That's it.

# Running nginx

Normally, `nginx` wants to run off files located in `/usr/local/etc`, but we want to run with our local file.
There are all sorts of ways in which you can get this to start up automatically, etc., etc., but here we just
want to get this running locally.

```
sudo nginx -c $PWD/nginx.conf
```

Again, that's all it takes.

# Tweaking your config

If you want to make changes to your config file and reload, do this:

```
sudo nginx -s reload
```

# Shutting nginx down

```
sudo nginx -s quit
```
