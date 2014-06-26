# fail2web

fail2web is a [fail2ban](http://www.fail2ban.org) GUI that communicates with a fail2ban instance via [fail2rest](https://github.com/Sean-Der/fail2rest)

fail2ban allows you to administer the following

* **Failregex** - Delete and add new failregexes
* **Banned IPs** - Ban and Unban IP address
* **Per Jail Config** - Configure find time, max retry and usedns per jail, and view the filelist per jail

with the following features planned in the future

* **Reporting** - Expose the time that an IP address was banned, and show trends via visualizations
* **Alerting** - Desktop notification when an IP address is banned
* **Regex Testing** - Testing ignore+fail regexes on your current logs to quickly build and debug regexes
* **More Jail Controls** - Create new jails and expose more settings for current jails

## Installing
###Development

* **Install build requirements**
    * nodejs and npm for browserify (not a runtime requirement) [Installing Node.js](https://github.com/joyent/node/wiki/Installing-Node.js-via-package-manager)
    * Git
* **Install libraries**
    * execute `npm install` in the root of the fail2web repository
* **Building**
    * When writing code run `npm run watch` this will rebuild web/bundle.js on every change
    * When deploying run `npm run build` this will build once and exit

###Production
There is production release of fail2web, but I plan to have one soon.

##Deploying and Configuration
To run fail2web you should host it via a HTTP server, the following is an example nginx config to do so

    server {
        listen       80;
        server_name  YOUR_SERVER_NAME;

        access_log  off;
        error_log off;
        auth_basic "Restricted";
        auth_basic_user_file YOUR_HTPASSWD_FILE;

        location / {
            root FAIL2WEB_WEB_FOLDER;
        }
        location /api/ {
            proxy_pass         http://127.0.0.1:5000/;
            proxy_redirect     off;
        }
    }

Fail2web has only one configuration option available via config.json in the root of the web folder.
This config option allows you to specify the path to your fail2rest handler. Currently the config.json uses /api/
which is what the above nginx config is also set to do.
