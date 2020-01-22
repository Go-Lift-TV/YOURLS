<h1 align="center">
  <a href="https://yourls.org">
    <img src="images/yourls-logo.png" alt="YOURLS">
  </a>
</h1>

> Your Own URL Shortener

[![Build Status](https://api.travis-ci.org/YOURLS/YOURLS.svg?branch=master)](https://travis-ci.org/YOURLS/YOURLS) [![Packagist](https://img.shields.io/packagist/v/yourls/yourls.svg)](https://packagist.org/packages/yourls/yourls) [![OpenCollective](https://opencollective.com/yourls/backers/badge.svg)](https://opencollective.com/yourls#contributors)
[![OpenCollective](https://opencollective.com/yourls/sponsors/badge.svg)](#sponsors)

**YOURLS** is a set of PHP scripts that will allow you to run <strong>Y</strong>our <strong>O</strong>wn <strong>URL</strong> <strong>S</strong>hortener. You'll have full control over your data, detailed stats, analytics, plugins, and more. It's free and open-source.

# Go Lift TV - YOURLS

The changes in this fork allow Nginx proxy auth. I use this behind
[Organizr](https://github.com/causefx/Organizr) with Nginx proxy auth.
The changes specifically allow me (or you) to expose the `/admin` interface
to (authorized) users on one domain while creating short links on another.
My admin interface is on `golift.tv`, but the short links use `l.golift.tv`.
You may choose any domains you wish. The app uses the `/admin`, `/js`, `/css`
and `/images` paths on the _admin_ domain, so you will need to account for that.
I provided a sample solution in the nginx config below.

**If you use this code you must protect your `/admin` path!**

This is the running nginx config. YOURLS is installed at `/config/www/YOURLS/`.
```shell
# Server for public domain.
server {
  server_name        l.*;
  listen             443 ssl http2;
  include            /config/nginx/ssl.conf;
  root               /config/www/YOURLS;
  index              index.php index.html index.htm;
  location /admin {
    return 403;
  }

  location ~ \.php$ {
    fastcgi_split_path_info ^(.+\.php)(/.+)$;
    fastcgi_pass            127.0.0.1:9000;
    fastcgi_index           index.php;
    include                 /etc/nginx/fastcgi_params;
  }

  location / {
    try_files $uri $uri/ /yourls-loader.php$is_args$args;
  }
}

# YOURLS admin interface on different (or same) domain.
# Add this inside your existing server block.
# Mine lives in the letsencrypt Docker container nginx config file.ß
# Uses Organizr auth to authorize users.
location /admin {
  auth_request.    /auth-4;
  auth_request_set $user $upstream_http_x_organizr_user;
  add_header       X-WEBAUTH-USER $user;
  root             /config/www/YOURLS;
  index            index.php index.html index.htm;
  location ~ \.php$ {
    fastcgi_split_path_info ^(.+\.php)(/.+)$;
    fastcgi_pass            127.0.0.1:9000;
    fastcgi_index           index.php;
    include                 /etc/nginx/fastcgi_params;
  }

  location /admin {
    try_files $uri $uri/ /yourls-loader.php$is_args$args;
  }
}

# These blocks allow serving assets for /images, /js and /css from more than 1 location.
# These paths are not using auth, but could if you want more server load.
location /images {
  root      /config/www/;
  try_files $uri $uri/ /YOURLS$uri /YOURLS$uri/ @organizr;
}
location /css {
  root      /config/www/;
  try_files $uri $uri/ /YOURLS$uri /YOURLS$uri/ @organizr;
}
location /js {
  root      /config/www/;
  try_files $uri $uri/ /YOURLS$uri /YOURLS$uri/ @organizr;
}
```

*The rest of this document is the original readme from the forked repo.*

## Quick Start

To get started, check [yourls.org](https://yourls.org)!
Learn more tweaks in the [Wiki documentation](https://github.com/YOURLS/YOURLS/wiki/).


## Community news, tips and tricks

* Read and subscribe to the [The Official YOURLS Blog](http://blog.yourls.org)
* Follow [@yourls](https://twitter.com/yourls) on Twitter
* Subscribe to the [YOURLS User Newsletter](https://yourls.org/newsletter) (infrequent, low volume)


## Keep track of development

* Follow [@yourls_dev](https://twitter.com/yourls_dev)
* Check [commit messages](https://github.com/YOURLS/YOURLS/commits/master)
* Check the [Road map](https://github.com/orgs/YOURLS/projects/1)


## Contributing

__Before opening any issue, please search for existing issues and read the [Issue Guidelines](https://github.com/YOURLS/YOURLS/wiki/Bug-Report).__

Have a **new bug** to report? [Please open a new issue](https://github.com/YOURLS/YOURLS/issues/new?title=Issue+title+--+be+DESCRIPTIVE).


## Backers

Do you use and enjoy YOURLS? [Become a backer](https://opencollective.com/yourls#backer) and show your support to our open source project.

[![](https://opencollective.com/yourls/backer/0/avatar.svg)](https://opencollective.com/yourls/backer/0/website)
[![](https://opencollective.com/yourls/backer/1/avatar.svg)](https://opencollective.com/yourls/backer/1/website)
[![](https://opencollective.com/yourls/backer/2/avatar.svg)](https://opencollective.com/yourls/backer/2/website)
[![](https://opencollective.com/yourls/backer/3/avatar.svg)](https://opencollective.com/yourls/backer/3/website)
[![](https://opencollective.com/yourls/backer/4/avatar.svg)](https://opencollective.com/yourls/backer/4/website)
[![](https://opencollective.com/yourls/backer/5/avatar.svg)](https://opencollective.com/yourls/backer/5/website)
[![](https://opencollective.com/yourls/backer/6/avatar.svg)](https://opencollective.com/yourls/backer/6/website)
[![](https://opencollective.com/yourls/backer/7/avatar.svg)](https://opencollective.com/yourls/backer/7/website)
[![](https://opencollective.com/yourls/backer/8/avatar.svg)](https://opencollective.com/yourls/backer/8/website)
[![](https://opencollective.com/yourls/backer/9/avatar.svg)](https://opencollective.com/yourls/backer/9/website)
[![](https://opencollective.com/yourls/backer/10/avatar.svg)](https://opencollective.com/yourls/backer/10/website)
[![](https://opencollective.com/yourls/backer/11/avatar.svg)](https://opencollective.com/yourls/backer/11/website)
[![](https://opencollective.com/yourls/backer/12/avatar.svg)](https://opencollective.com/yourls/backer/12/website)
[![](https://opencollective.com/yourls/backer/13/avatar.svg)](https://opencollective.com/yourls/backer/13/website)
[![](https://opencollective.com/yourls/backer/14/avatar.svg)](https://opencollective.com/yourls/backer/14/website)
[![](https://opencollective.com/yourls/backer/15/avatar.svg)](https://opencollective.com/yourls/backer/15/website)
[![](https://opencollective.com/yourls/backer/16/avatar.svg)](https://opencollective.com/yourls/backer/16/website)
[![](https://opencollective.com/yourls/backer/17/avatar.svg)](https://opencollective.com/yourls/backer/17/website)
[![](https://opencollective.com/yourls/backer/18/avatar.svg)](https://opencollective.com/yourls/backer/18/website)
[![](https://opencollective.com/yourls/backer/19/avatar.svg)](https://opencollective.com/yourls/backer/19/website)
[![](https://opencollective.com/yourls/backer/20/avatar.svg)](https://opencollective.com/yourls/backer/20/website)
[![](https://opencollective.com/yourls/backer/21/avatar.svg)](https://opencollective.com/yourls/backer/21/website)
[![](https://opencollective.com/yourls/backer/22/avatar.svg)](https://opencollective.com/yourls/backer/22/website)
[![](https://opencollective.com/yourls/backer/23/avatar.svg)](https://opencollective.com/yourls/backer/23/website)
[![](https://opencollective.com/yourls/backer/24/avatar.svg)](https://opencollective.com/yourls/backer/24/website)
[![](https://opencollective.com/yourls/backer/25/avatar.svg)](https://opencollective.com/yourls/backer/25/website)
[![](https://opencollective.com/yourls/backer/26/avatar.svg)](https://opencollective.com/yourls/backer/26/website)
[![](https://opencollective.com/yourls/backer/27/avatar.svg)](https://opencollective.com/yourls/backer/27/website)
[![](https://opencollective.com/yourls/backer/28/avatar.svg)](https://opencollective.com/yourls/backer/28/website)
[![](https://opencollective.com/yourls/backer/29/avatar.svg)](https://opencollective.com/yourls/backer/29/website)


## Sponsors

Does your company use YOURLS? Ask your manager or marketing team if your company would be interested in supporting our project. Your company logo will show here. Help support our open-source development efforts by [becoming a sponsor](https://opencollective.com/yourls).

[![](https://opencollective.com/yourls/sponsor/0/avatar.svg)](https://opencollective.com/yourls/sponsor/0/website)
[![](https://opencollective.com/yourls/sponsor/1/avatar.svg)](https://opencollective.com/yourls/sponsor/1/website)
[![](https://opencollective.com/yourls/sponsor/2/avatar.svg)](https://opencollective.com/yourls/sponsor/2/website)
[![](https://opencollective.com/yourls/sponsor/3/avatar.svg)](https://opencollective.com/yourls/sponsor/3/website)
[![](https://opencollective.com/yourls/sponsor/4/avatar.svg)](https://opencollective.com/yourls/sponsor/4/website)
[![](https://opencollective.com/yourls/sponsor/5/avatar.svg)](https://opencollective.com/yourls/sponsor/5/website)
[![](https://opencollective.com/yourls/sponsor/6/avatar.svg)](https://opencollective.com/yourls/sponsor/6/website)
[![](https://opencollective.com/yourls/sponsor/7/avatar.svg)](https://opencollective.com/yourls/sponsor/7/website)
[![](https://opencollective.com/yourls/sponsor/8/avatar.svg)](https://opencollective.com/yourls/sponsor/8/website)
[![](https://opencollective.com/yourls/sponsor/9/avatar.svg)](https://opencollective.com/yourls/sponsor/9/website)
[![](https://opencollective.com/yourls/sponsor/10/avatar.svg)](https://opencollective.com/yourls/sponsor/10/website)
[![](https://opencollective.com/yourls/sponsor/11/avatar.svg)](https://opencollective.com/yourls/sponsor/11/website)
[![](https://opencollective.com/yourls/sponsor/12/avatar.svg)](https://opencollective.com/yourls/sponsor/12/website)
[![](https://opencollective.com/yourls/sponsor/13/avatar.svg)](https://opencollective.com/yourls/sponsor/13/website)
[![](https://opencollective.com/yourls/sponsor/14/avatar.svg)](https://opencollective.com/yourls/sponsor/14/website)
[![](https://opencollective.com/yourls/sponsor/15/avatar.svg)](https://opencollective.com/yourls/sponsor/15/website)
[![](https://opencollective.com/yourls/sponsor/16/avatar.svg)](https://opencollective.com/yourls/sponsor/16/website)
[![](https://opencollective.com/yourls/sponsor/17/avatar.svg)](https://opencollective.com/yourls/sponsor/17/website)
[![](https://opencollective.com/yourls/sponsor/18/avatar.svg)](https://opencollective.com/yourls/sponsor/18/website)
[![](https://opencollective.com/yourls/sponsor/19/avatar.svg)](https://opencollective.com/yourls/sponsor/19/website)


## License

Free software. Do whatever the hell you want with it.  
YOURLS is released under the [MIT license](LICENSE).
