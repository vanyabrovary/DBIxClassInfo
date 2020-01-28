### INSTALL nginx, php, postgresql, certbot

```
apt update && apt upgrade && add-apt-repository ppa:certbot/certbot 
apt install postgresql-10 nginx-extras python-certbot-nginx php7.2 php7.2-pgsql
```

### INSTALL postgrest, adminer, swagger

*It is recommended to create a new temporary directory for the installation, so you can easily delete the downloaded files after the installation. This should do the trick:*

```
mkdir ~/src && cd ~/src
wget https://github.com/PostgREST/postgrest/releases/download/v6.0.2/postgrest-v6.0.2-linux-x64-static.tar.xz && tar -xzf postgrest-v6.0.2-linux-x64-static.tar.xz && cp postgrest /var/www/postgrest/
mkdir -p /var/www/postgrest/db && wget https://ssh.in.ua/adminer.php.tar.gz -O /var/www/postgrest/db/adminer.php
wget https://ssh.in.ua/api.php.tar.gz && tar -xzf api.php.tar.gz && cp api/ /var/www/postgrest/
```

### CREATE POSTGRESQL DB

```
sudo -u postgres createuser agrill --pwprompt
sudo -u postgres createdb agrill -O agrill
```

### CREATE POSTGRESQL agrill table ivr_log

*Connect to DB*
```
sudo -u postgres psql --host=127.0.0.1 --dbname=agrill --username=agrill --password=agrill
```

*Connect to DB agrill*
```
\c agrill
```

*Create function for updated_at*
```
CREATE FUNCTION public.updated_proce() RETURNS trigger LANGUAGE plpgsql AS $$ BEGIN NEW.updated_at := CURRENT_TIMESTAMP; RETURN NEW; END $$; 
```

*Create ivr_log table*
```
CREATE TABLE public.ivr_log ("UID" character varying(255) NOT NULL,"PhoneNumber" character varying(32) NOT NULL,"Name" character varying(255) NOT NULL,"Email" character varying(255) NOT NULL, created_at timestamp without time zone DEFAULT now() NOT NULL, updated_at timestamp without time zone DEFAULT now() NOT NULL, "OpenNumber" integer DEFAULT 0);
CREATE VIEW public.ivr_log_group_by_id AS SELECT ivr_log."UID", array_to_string(array_agg(ivr_log."Name"), ','::text) AS "Names", count(ivr_log."OpenNumber") AS count, array_to_string(array_agg(ivr_log."OpenNumber"), '+'::text) AS "Numbers", sum(ivr_log."OpenNumber") AS sum  FROM public.ivr_log GROUP BY ivr_log."UID";
```

*Create view ivr_log_group_phone_number*
```
CREATE VIEW public.ivr_log_group_phone_number AS SELECT ivr_log."PhoneNumber", array_to_string(array_agg(((ivr_log."UID")::text || (ivr_log."Name")::text)), ','::text) AS "UIDS", count(ivr_log."OpenNumber") AS count, array_to_string(array_agg(ivr_log."OpenNumber"), '+'::text) AS "Numbers", sum(ivr_log."OpenNumber") AS sum FROM public.ivr_log GROUP BY ivr_log."PhoneNumber";
```

*Create trigger for updated_at*
```
CREATE TRIGGER ivr_log_update BEFORE UPDATE ON public.ivr_log FOR EACH ROW EXECUTE PROCEDURE public.updated_proce();
```

*Ownre permissions*
```
ALTER FUNCTION public.updated_proce() OWNER TO agrill;
ALTER TABLE public.ivr_log OWNER TO agrill;
ALTER TABLE public.ivr_log_group_by_id OWNER TO agrill;
ALTER TABLE public.ivr_log_group_phone_number OWNER TO agrill;
```

*Exit from PostgreSQL*
```
\q
```

### CREATE POSTGREST CONFIG /var/www/postgrest/postgrest.conf

```
db-uri           = "postgres://agrill:agrill@127.0.0.1:5432/agrill"
db-schema        = "public"
db-anon-role     = "agrill"
server-host      = "!4"
server-port      = "8054"
server-proxy-uri = "http://a.argentinagrill.rest/v2/"
```


### CREATE NGINX CONFIG /etc/nginx/sites-enabled/a.argentinagrill.rest.conf

```
server {
    server_name a.argentinagrill.rest;
    listen 443 ssl http2;
    ssl_certificate     /etc/letsencrypt/live/a.argentinagrill.rest/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/a.argentinagrill.rest/privkey.pem;
    include             /etc/letsencrypt/options-ssl-nginx.conf;
    ssl_dhparam         /etc/letsencrypt/ssl-dhparams.pem;
    root                /var/www/postgrest;
    location / {
        index index.html;
	alias /var/www/postgrest/api/;
    }
    location /web/ {
        index index.html;
    }
    location /db/ {
        index index.php;
        try_files $uri $uri/ /index.php?$query_string;
    }
    location ~ ^/v2/(.+) {
        proxy_pass http://127.0.0.1:8054/$1;
    }
    location ~* \.php$ {
        try_files 		$uri = 404;
        fastcgi_split_path_info ^(.+\.php)(/.+)$;
        fastcgi_pass 		unix:/run/php/php7.2-fpm.sock;
        fastcgi_index 		index.php;
        fastcgi_param 		SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include 		fastcgi_params;
        access_log 		/var/log/nginx/a.argentinagrill.rest.access.log;
        error_log 		/var/log/nginx/a.argentinagrill.error.access.log;
    }
}
```

### START nginx

```
/etc/init.d/nginx restart
```

### START POSTGREST

```
nohup /var/www/postgrest/postgrest /var/www/postgrest/postgrest.conf > /var/log/postgrest.log &
```

### STOP POSTGREST

```
ps aux |grep postgrest |grep -v 'grep' |awk '{print $2;}' |xargs kill -9
```

### REFRESH SCHEMA

```
killall -SIGUSR1 postgrest
```

### TESTING

```
curl -X POST --header 'Content-Type: application/json' --header 'Accept: application/json' -d '{ "UID": "2UID", "PhoneNumber": "+380935255566", "UserName": "I", "Email": "v@gmail.com", "openNumber": "2"}' 'https://a.argentinagrill.rest/v2/ivr_log'
```

```
curl -X GET --header 'Content-Type: application/json' --header 'Accept: application/json' 'https://a.argentinagrill.rest/v2/ivr_log'
```
