# [`cms`](https://github.com/cms-dev/cms) dockerized

## Setting up
-------------------
services:
`db` and `cms_dev`.

Build and run:
```shellsession
foo@cms-test$ cd cms-dev/
foo@cms-dev$ docker compose -f docker-compose.yml up --build
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

Config file used is `config/cms.conf.docker.sample` to set the right host and port to connect to the database (`db:5432`). The user is set to cmsuser:cmsuser.

You can connect as postgres, using:
```shellsession
cmsuser@~/cms$ psql -h db -p 5432 -U postgres
postgres#
postgres# logout
cmsuser@~/cms$
```
Or using psql:
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

Create the database schema for CMS:
```shellsession
cmsuser@~/cms$ cmsInitDB
```
Create your admin account:
```shellsession
cmsuser@~/cms$ cmsAddAdmin your_username -p your_password
```
Start web server for admin:
```shellsession
cmsuser@~/cms$ cmsAdminWebServer
```
In your web browser, navigate to [http://localhost:8889](http://localhost:8889) and enter your admin credentials.

Start all services:
```shellsession
cmsuser@~/cms$ cmsResourceService -a
```
[More info about resources](https://cms.readthedocs.io/en/latest/Running%20CMS.html)

After configuring a contest, you can manually add a user by using the web interface.

They can log in with their credentials at:
[http://localhost:8888](http://localhost:8888).

