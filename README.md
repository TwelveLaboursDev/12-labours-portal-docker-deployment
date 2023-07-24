# 12 Labours Portal Docker Deployment

## Docker image
Click image name to see more information for docker images.

`Web Portal` - [12labours/app-dev:latest](https://hub.docker.com/r/12labours/app-dev)

`Query Server` - [12labours/api-dev:latest](https://hub.docker.com/r/12labours/api-dev)

`Login System`- [12labours/nodejsapi-dev:latest](https://hub.docker.com/r/12labours/nodejsapi-dev)

`pgAdmin4` - [dpage/pgadmin4](https://hub.docker.com/r/dpage/pgadmin4)

`PostgresDB` - [12labours/postgres-dev:latest](https://hub.docker.com/r/12labours/postgres-dev)

You can also create your own docker image by forking 12 labours portal related repositories. To find out related repositories, please check [Auckland Bioengineering Institute Software](https://github.com/ABI-Software) GitHub.

## Environment variables
Create a *`.env`* file, copy and paste with following environment variables.
```bash
PORTAL_URL = "http://localhost:3000"
QUERY_API_URL = "http://localhost:8000"
LOGIN_API_URL = "http://localhost:8080"

# api_key and secret_key should be set to complex strings
LOGIN_API_KEY =
LOGIN_SECRET_KEY =

GRAPHCMS_ENDPOINT =
GOOGLE_CLIENT_ID =
TWELVE_LABOURS_XML =
// GOOGLE_ANALYTICS_GA4 =
FLATMAP_API =


JWT_SECURE_KEY =

GEN3_ENDPOINT_URL = "https://gen3.abi-ctt-ctp.cloud.edu.au"
GEN3_API_KEY =
GEN3_KEY_ID =
GEN3_PUBLIC_ACCESS =

IRODS_ENDPOINT_URL = "/tempZone/home/rods/12L/datasets"
IRODS_HOST = "130.216.216.128"
IRODS_PORT = "1247"
IRODS_USER =
IRODS_PASSWORD =
IRODS_ZONE = "tempZone"


LOGIN_API_PORT = "8080"

USER_VERIFY_URL = "http://localhost:3000/verify"
GOOGLE_REDIRECT_URI = "http://localhost:3000/login/callback"
PASSWORD_RESET_URL = "http://localhost:3000/password-reset"

# db_user and db_name better to be the same, will easier to setup web version pgadmin4
# here is an example of db information
DB_HOST = "postgresdb"
DB_PORT = "5432"
DB_USER =
DB_PASSWORD =
DB_NAME =

GOOGLE_CLIENT_ID =
GOOGLE_CLIENT_SECRET =
SENDGRID_API_KEY =
SENDGRID_VERIFIED_SENDER =

# use your own admin email and password
PGADMIN_DEFAULT_EMAIL =
PGADMIN_DEFAULT_PASSWORD =
```

## Start the docker-compose file
Clone the repo and follow above steps to create env file.

Use following command in the `Terminal` to start all docker images.
```bash
$ docker compose up
```
Or right click the *`docker-compose.yml`* file, choose **`Compose Up (- Select Services)`** command to start docker images.


## Setup pgAdmin4 web version
1. Point your browser to [http://localhost:5050](http://localhost:5050)
2. Login with the email and account you used in `PGADMIN_DEFAULT_EMAIL` and `PGADMIN_DEFAULT_PASSWORD`.
3. Once logged in, click **`Add New Server`** under Quick Links.
4. Fill the server name.
5. Go to Connection:

    - Host: `Container IPAddress` (Run `docker ps` in terminal, then run `docker inspect <postgres container id>`, you will find **`IPAddress`**)
    - Username: `DB_NAME`
    - Password: `DB_PASSWORD`
6. Now you can find all user data through `Databases` > `12labours` > `Schemas` > `public` > `Tables` or use sql to query data.


## Setting update when pgAdmin4 container restarted
If you restart the pgAdmin4 docker container, you cannot open the pgAdmin4 web version directly. When you enter the password, you will get a `connection timeout expired` error. This is because the container IPAddress is changed every time it restarted. Please follow the steps to fix the setting:
1. Right click you database server name.
2. Choose `Properties`.
3. Go to `Connection`.
4. Repeat `Step 5 - Host` in `Setup pgAdmin4 web version` to get current `IPAddress`.
5. Replace the IPAddress.
6. Now you should be able to access your database through pgAdmin4 web again.
