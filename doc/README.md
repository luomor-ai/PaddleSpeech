```shell
cd docker/ubuntu18-cpu

git clone --depth 1 git@github.com:PaddlePaddle/PaddleSpeech.git

sudo docker build -t yiluxiangbei/paddlespeech:v1.0 -f Dockerfile1 .

du -h -d 1 .
```