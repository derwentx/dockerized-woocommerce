# Dockerized - Wordpress

This dockerization allows to easily build Wordpress development environments.
It comes with a helper script that can used like so:

```
./manage start
./manage stop
./manage destroy
```

To customize the Wordpress environment, copy the `env/overrides.env.sample` file
into `env/overrides.env` and tweak the values appropriately.

The Wordpress Dockerfile was forked to match our needs based on a repository found
[here](https://github.com/tristanpenman/docker-wordpress).

It allows to add post-install steps into scripts that you can place inside the
`data/scripts/post-install.d` folder. This repository shows how to directly install
Wordpress core programatically (See the `10-install-core` script)
as well as how to install and activate a plugin (see `20-install-woocommerce`).
