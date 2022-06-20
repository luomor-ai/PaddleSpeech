```shell
cd docker/ubuntu18-cpu

git clone --depth 1 git@github.com:PaddlePaddle/PaddleSpeech.git

sudo docker build -t yiluxiangbei/paddlespeech:v1.0 -f Dockerfile1 .
sudo docker push yiluxiangbei/paddlespeech:v1.0

cd ../..
sudo docker run -ti --volume="$(pwd)":/app --rm yiluxiangbei/paddlespeech:v1.0 bash

cd /app
paddlespeech_server start --config_file ./paddlespeech/server/conf/application.yaml

apt-get autoremove --purge protobuf-compiler
apt-get autoremove --purge libprotobuf-dev

du -h -d 1 .
```

```shell
apt-get install protoc
ubuntu protoc
```