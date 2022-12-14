## Running The WDB Docker Container

Run the container with the following docker compose file.

```yml
version: '3.4'

services:
  wunderdb:
    image: ghcr.io/tanmoysg/wunderdb:v1.0.2-test
    container_name: wunderdb
    ports:
      - 5002:5002
    environment:
      - PORT=123
      - SERVER=smtp.server.com
      - PASSWORD=password
      - SENDER=sender
    volumes:
      - ./clusters:/app/clusters
    networks:
      - wundernetwork
```

For Secrets, setup a `server-config.json` file in `secrets` directory with following format
```json
{
    "port": "123",
    "mail-server": "mail.server.com",
    "password": "password@123",
    "sender": "email@server.com"
}
```

Then run the docker compose file.
```
docker-compose up
```

### Creating User Account

To Create Account run thr following curl command
```sh
curl --location --request POST 'http://localhost:5002/register/authenticate' \
--header 'Content-Type: application/json' \
--data-raw '{
    "username":"you-email@here.com"
}'
```

You'll recieve an email with the OTP on your email ID. Using the OTP run the following Curl Command
```sh
curl --location --request POST 'http://localhost:5002/register/verify' \
--header 'Content-Type: application/json' \
--data-raw '{
    "username": "your-email@here.com",
    "name": "User",
    "password": "password@123",
    "otp": "OTP123"
}'
```

## Running with WunderDash

WunderDash is the official client of WunderDB. To run wDash refer to the [wDash documentation](https://github.com/TanmoySG/wunderDash). Also, you can run wDB and wDash using docker-compose in this repo.

```shell
docker-compose up
```

### Using WDB

For usage use the wdb [documentation](./documentation.md).