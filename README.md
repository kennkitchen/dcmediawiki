# Docker-Compose MediaWiki
## with MariaDB and Memcached

Get a development version of MediaWiki running in Docker with files uploads, custom logo, data persistency, and Memcached.

## How to Use

1. Download or clone this repository.
2. Edit the .env files and change any values as desired.
3. From within the local project root, run:

`docker-compose up -d`

Note: the output from the above command will look something like this:

```
Creating network "mediawiki_default" with the default driver
Creating mediawiki_memcached_1 ... done
Creating mediawiki_database_1  ... done
Creating mediawiki_mediawiki_1 ... done
```

You'll need these names later!

4. Visit `http://localhost:8080` in your browser and go through the normal online MediaWiki setup process. Use the values you specified in the .env file where requested.

Note: for the database, instead of the defaul ("localhost"), put in the name that came from running `docker-compose up -d` followed by the port (e.g. "mediawiki_database_1:3306").

5. When MediaWiki's setup is complete, it will prompt you to download the "LocalSettings.php" file that it generated. Do so, and save it to `datadir/config`.
6. Stop your local installation with the following command:

`docker-compose down`

7. Edit your `docker-compose.yml` file to **uncomment** the following line:

`- ./datadir/config/LocalSettings.php:/var/www/html/LocalSettings.php`

8. Restart your local instance with:

`docker-compose up -d`

9. Now re-visit `http://localhost:8080` in your browser to see your fresh copy of MediaWiki.

## Additional Configuration

### Custom Logo

A `logo.png` file is included in the root of this repository of the proper dimensions (135x135) with transparent background. Edit this file and save it to the `datadir/images` folder.

In your `datadir/config/LocalSettings.php` file, find the `$wgLogo` definition and replace it with:

`$wgLogo = $wgScriptPath . "/images/logo.png";`

### Memcached

To enable Memcached use, find the section of `datadir/config/LocalSettings.php` that begins with `## Shared memory settings` and put in your own values for the following:

```
$wgMainCacheType = CACHE_MEMCACHED;
$wgMemCachedServers = [ "mediawiki_memcached_1:11211" ];
```
