# localstack-docker-server

Script to build a [localstack](https://www.localstack.cloud/) environment with docker

- Debian version: `12.5` (bookworm)
- Docker version: `26.1.1, build 4cf5afa`

## Setting

Create Docker Network (Only the first time)

```
$ cd localstack-network/
$ docker compose up

$ docker compose ps -a
NAME                        IMAGE                     COMMAND   SERVICE   CREATED       STATUS    PORTS
localstack-network-fake-1   localstack-network-fake   "fake"    fake      3 weeks ago   Created
```

Create localstack server (Only the first time)

```
$ cd localstack-server/
$ docker compose up

$ docker compose ps -a
NAME                                    IMAGE                   COMMAND                  SERVICE             CREATED         STATUS                   PORTS
localstack-server-localstack-server-1   localstack/localstack   "docker-entrypoint.sh"   localstack-server   3 minutes ago   Up 3 minutes (healthy)   127.0.0.1:4510-4559->4510-4559/tcp, 127.0.0.1:4566->4566/tcp, 5678/tcp
```

add the contents of `localstack-server/aws_profile/{config,credential}` to `~/.aws/config` etc.

```
$ aws configure list
      Name                    Value             Type    Location
      ----                    -----             ----    --------
   profile               localstack              env    ['AWS_PROFILE', 'AWS_DEFAULT_PROFILE']
access_key     ****************ummy shared-credentials-file
secret_key     ****************ummy shared-credentials-file
    region                us-east-1      config-file    ~/.aws/config
```

Enjoy development :)

```
## Create My Application
$ cd app/

## Containers IpAddress
$ docker network inspect localstack-net | jq -r '.[0].Containers[] | [.Name, "-->", .IPv4Address] | @csv' | column -t -s "," | xargs -ICONTAINER echo "- CONTAINER"
- app-web-1                              -->  172.20.0.3/16
- localstack-server-localstack-server-1  -->  172.20.0.2/16
```
