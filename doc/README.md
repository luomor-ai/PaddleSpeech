```shell
cd docker/ubuntu18-cpu

git clone --depth 1 git@github.com:PaddlePaddle/PaddleSpeech.git

sudo docker build -t yiluxiangbei/paddlespeech:v1.0 -f Dockerfile1 .
sudo docker push yiluxiangbei/paddlespeech:v1.0

cd ../..
sudo docker run -ti --volume="$(pwd)":/app --rm yiluxiangbei/paddlespeech:v1.0 bash

wget -c https://paddlespeech.bj.bcebos.com/PaddleAudio/zh.wav
paddlespeech_client asr_online --server_ip 49.232.6.131 --port 8101 --input ./zh.wav

cd /app
paddlespeech_server start --config_file ./paddlespeech/server/conf/application.yaml

pip install paddlespeech_ctcdecoders -i https://pypi.tuna.tsinghua.edu.cn/simple

sudo docker commit 36033335bd46 yiluxiangbei/paddlespeech:v1.1
sudo docker push yiluxiangbei/paddlespeech:v1.1

sudo docker build -t yiluxiangbei/paddlespeech:v1.2 -f Dockerfile2 .
sudo docker push yiluxiangbei/paddlespeech:v1.2

sudo docker run -it --name paddlespeech -p 8101:8090 -d yiluxiangbei/paddlespeech:v1.2

sudo docker exec -it paddlespeech bash 

sudo docker stop paddlespeech
sudo docker start paddlespeech
sudo docker rm paddlespeech

sudo docker logs -f paddlespeech

sudo docker build -t yiluxiangbei/paddlespeech:v1.2.1 -f Dockerfile2 .
sudo docker push yiluxiangbei/paddlespeech:v1.2.1

sudo docker run -ti --volume="$(pwd)":/app --rm yiluxiangbei/paddlespeech:v1.2.1 bash

wget -c https://paddlespeech.bj.bcebos.com/PaddleAudio/zh.wav
paddlespeech_client asr_online --server_ip 49.232.6.131 --port 8101 --input ./zh.wav

apt-get autoremove --purge protobuf-compiler
apt-get autoremove --purge libprotobuf-dev

du -h -d 1 .
wget -c https://paddlespeech.bj.bcebos.com/PaddleAudio/zh.wav
https://github.com/PaddlePaddle/PaddleSpeech/blob/develop/demos/streaming_asr_server/README.md
```

```shell
apt-get install protoc
ubuntu protoc

git clone git@github.com:google/benchmark.git
git clone git@github.com:google/googletest.git

git clone git@github.com:protocolbuffers/protobuf.git
apt-get install autoconf automake libtool curl make g++ unzip
cd protobuf/
git submodule update --init --recursive
./autogen.sh   #生成配置脚本
./configure    #生成Makefile文件，为下一步的编译做准备，可以加上安装路径：--prefix=path ，默认路径为/usr/local/
make           #从Makefile读取指令，然后编译
make check     #可能会报错，但是不影响,对于安装流程没有实质性用处，可以跳过该步
make install 
ldconfig       #更新共享库缓存
which protoc        #查看软件的安装位置
protoc --version    #检查是否安装成功

[submodule "third_party/benchmark"]
        path = third_party/benchmark
        url = https://github.com/google/benchmark.git
        ignore = dirty
[submodule "third_party/googletest"]
        path = third_party/googletest
        url = https://github.com/google/googletest.git
        ignore = dirty
```