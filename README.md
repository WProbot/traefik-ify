# Traefik-ify

> Easily Traefik-ify your roots/bedrock WordPress installation. It's great! It's easy to do! Nothing to lose (except some port conflicts!)

And it's basically non-destructive for an existing Trellis setup. Dank.

## Try it out

In ./traefik/:

1. `docker network create traefikpress`
2. `docker-compose up -d`

In ./example.com/:

1. `composer install`
2. Rename `env.example` to `.env`.
3. `docker-compose up -d`

Lastly, add example.local to your `/etc/hosts`.

WordPress installation screen should be available at `example.local`.

You can monitor the traefik web GUI at `localhost`.

## Add to existing install

These are the files you'll want to add (root is bedrock):

1. `./config/docker/` (make sure to update the nginx server name)
2. `./Dockerfile`
3. `./docker-compose.yml`
4. Add `database` to your `.gitignore` 🙀
5. Add something like the following to your `./.env`:

``` env
# Docker local
DEV_WP=example.local
DEV_WP_HOME=http://${DEV_WP}
DEV_WP_SITEURL=${DEV_WP_HOME}/wp
```

6. Add something like this to `./config/environments/development.php`:

```
// Dockerize
Config::define('WP_HOME', env('DEV_WP_HOME'));
Config::define('WP_SITEURL', env('DEV_WP_SITEURL'));
```

Just comment that last bit out if you wanna flip back over to Ansible for a spell. This is the only part of the config that overlaps.

## Party!

You should be good to  `docker-compose up` from your traefik and existing bedrock. You'll need to port the db.