# PowerDNS Docker Container
Based on [psitrax/powerdns](https://hub.docker.com/r/psitrax/powerdns/).

* Small Alpine based image
* MySQL/MariaDB backend included
* DNSSEC support optional
* Automatic MySQL database initialization
* Latest PowerDNS version (if not pls file an issue)
* Guardian process enabled
* Graceful shutdown using `pdns_control`

## Supported tags

* Exact: i.e. `4.4.1`: PowerDNS Version 4.4.1
* `4.0`: PowerDNS Version 4.0.x, latest image build
* `4`: PowerDNS Version 4.x.x, latest image build

## Usage

```shell
# Start a MySQL Container
$ docker run -d \
  --name pdns-mysql \
  -e MYSQL_ROOT_PASSWORD=supersecret \
  -v $PWD/mysql-data:/var/lib/mysql \
  mariadb:10.5.12

$ docker run --name pdns \
  --link pdns-mysql:mysql \
  -p 53:53 \
  -p 53:53/udp \
  -e MYSQL_USER=root \
  -e MYSQL_PASS=supersecret \
  -e MYSQL_PORT=3306 \
  docker.io/freinet/powerdns \
    --cache-ttl=120 \
    --allow-axfr-ips=127.0.0.1,123.1.2.3
```

## Configuration

**Environment Configuration:**

* MySQL connection settings
  * `MYSQL_HOST=mysql`
  * `MYSQL_USER=root`
  * `MYSQL_PASS=root`
  * `MYSQL_DB=pdns`
  * `MYSQL_DNSSEC=no`
* To support docker secrets, use same variables as above with suffix `_FILE`.
* Want to disable mysql initialization? Use `MYSQL_AUTOCONF=false`
* DNSSEC is disabled by default, to enable use `MYSQL_DNSSEC=yes`
* Want to use own config files? Mount a Volume to `/etc/pdns/conf.d` or simply overwrite `/etc/pdns/pdns.conf`
* To run additional startup scripts from entrypoint.sh before starting PowerDNS, mount them into `/entrypoint.d`

**PowerDNS Configuration:**

Append the PowerDNS setting to the command as shown in the example above.
See `docker run --rm freinet/powerdns --help`

## License

[GNU General Public License v2.0](https://github.com/PowerDNS/pdns/blob/master/COPYING) applyies to PowerDNS and all files in this repository.


## Maintainers

* Sebastian Pitsch <pitsch@freinet.de>
* Dominic Zöller <zoeller@freinet.de>

### Credits

* Christoph Wiechert <wio@psitrax.de>: Original Project which we forked from
* Mathias Kaufmann <me@stei.gr>: Reduced image size

