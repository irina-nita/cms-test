# [`cms`](https://github.com/cms-dev/cms) dockerized

## Setting up
-------------------
services:
`db` and `cms_dev`.

Build and run:
```shellsession
foo@cms-test$ cd cms-dev/
foo@cms-dev$ docker compose -f docker-compose.yml up
```
_(This should show the logs for the database)_

In another terminal, run:
```shellsession
foo@cms-dev$ docker compose exec cms_dev bash
```
Now this should start an interactive shell for cms.

Optional:
```shellsession
cmsuser@~/cms$ sudo apt install tmux -y
cmsuser@~/cms$ sudo apt install vim -y
cmsuser@~/cms$ tmux
```

Copy `config/cms.conf.docker.sample` to `/usr/local/etc/cms.conf` to set the right host and port to connect to the database (`db:5432`). The user is set to cmsuser:cmsuser.

```shellsession
cmsuser@~/cms$ cat ~/cms/config/cms.conf.docker.sample > /usr/local/etc/cms.conf
```

You can either connect to postgres:
```shellsession
cmsuser@~/cms$ psql -h db -p 5432 -U postgres
postgres#
```
Or run as cmsuser using psql.
_______
Run the following commands to create a new user & database:

_(if you don't want to modify cms.conf, let password be cmsuser)_
```shellsession
cmsuser@~/cms$ createuser -h db -p 5432 --username=postgres --pwprompt cmsuser
createdb -h db -p 5432 --username=postgres --owner=cmsuser cmsdb
```
Grant permissions to user:
```shellsession
cmsuser@~/cms$ psql -h db -p 5432 --username=postgres --dbname=cmsdb --command='ALTER SCHEMA public OWNER TO cmsuser'
cmsuser@~/cms$ psql  -h db -p 5432 --username=postgres --dbname=cmsdb --command='GRANT SELECT ON pg_largeobject TO cmsuser'
```
______

```shellsession
postgres# logout
```
Create the database schema for CMS:
```shellsession
cmsuser@~/cms$ cmsInitDB
```
Create your admin account:
```shellsession
cmsuser@~/cms$ cmsAddAdmin your_username -p your_password
```
Finally, start web server:
```shellsession
cmsuser@~/cms$ cmsLogService
```
________

In your web browser, navigate to [localhost:8889]() and enter your admin credentials.

### Done! 🥳
