# Snapcraft Documentation Website

The is the source code for the Snapcraft Documentation Website (<https://docs.snapcraft.io>) built with [Jekyll](https://jekyllrb.com/).

## Development

Requirements:
- Docker should be installed
  - [directions](https://docs.docker.com/install/)
- (Linux only) user should be part of the `docker` group
  - `sudo adduser <user> docker`

The docs website can be built locally by using the `./run` script available within this repository:

```
./run serve
```

Once the environment is created, you can view it at <http://localhost:8202/> in your browser.
