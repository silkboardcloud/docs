

```sh
sudo docker run -d   \
-e DRONE_GITHUB=true  \
-e DRONE_GITHUB_CLIENT=<github appid>  \
-e DRONE_GITHUB_SECRET=<github token>  \
-e DRONE_SECRET=secret  \
-e DRONE_OPEN=true \
-e DRONE_ADMIN=<github username> \
-e DRONE_HOST=http://drone.example.com \
-v /var/lib/drone:/var/lib/drone \
-p 80:8000 \
-p 9000:9000 \
--restart=always  \
--name=drone-server  \
drone/drone:0.8.5


sudo docker run \
-v /var/run/docker.sock:/var/run/docker.sock \
-e DRONE_SERVER=drone.example.com:9000 \
-e DRONE_DEBUG=true \
-e DRONE_SECRET=secret  \
--restart=always \
--detach=true \
--name=drone-agent drone/agent:0.8.5

```
