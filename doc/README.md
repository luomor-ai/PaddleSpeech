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
wget -c https://paddlespeech.bj.bcebos.com/PaddleAudio/zh.wav
paddlespeech_client asr --server_ip 127.0.0.1 --port 8090 --input zh.wav
# streaming
paddlespeech_client asr_online --server_ip 127.0.0.1 --port 8090 --input ./zh.wav

sudo docker stop paddlespeech
sudo docker start paddlespeech
sudo docker rm paddlespeech

sudo docker logs -f paddlespeech

sudo docker build -t yiluxiangbei/paddlespeech:v1.2.1 -f Dockerfile2 .
sudo docker push yiluxiangbei/paddlespeech:v1.2.1

sudo docker run -ti --volume="$(pwd)":/app --rm yiluxiangbei/paddlespeech:v1.2.1 bash

wget -c https://paddlespeech.bj.bcebos.com/PaddleAudio/zh.wav
paddlespeech_client asr --server_ip 49.232.6.131 --port 8101 --input ./zh.wav
paddlespeech_client asr --server_ip 172.21.16.11 --port 8101 --input ./zh.wav

paddlespeech_client tts --server_ip 49.232.6.131 --port 8101 --input "您好，欢迎使用Peter的语音合成服务。" --output output.wav

apt-get autoremove --purge protobuf-compiler
apt-get autoremove --purge libprotobuf-dev

du -h -d 1 .
wget -c https://paddlespeech.bj.bcebos.com/PaddleAudio/zh.wav
https://github.com/PaddlePaddle/PaddleSpeech/blob/develop/demos/streaming_asr_server/README.md
https://github.com/PaddlePaddle/PaddleSpeech/wiki/PaddleSpeech-Server-RESTful-API
url:POST /paddlespeech/asr
请求body参数
字段	必选	类型	说明
audio	是	string	将音频文件进行 base64编码后得到的 string
audio_format	是	string	合成音频文件格式，可选：pcm、wav，默认值：wav
sample_rate	是	int	音频的采样率，值选择 [8000, 16000]，默认与模型采样率一致
lang	是	string	语种 zh_cn：中文; zh_tw: 台湾普通话； en_us：英文
punc	否	bool	是否开启标点符号添加 true：开启 false：关闭（默认值）

url:POST /paddlespeech/tts
请求body参数
字段	必选	类型	说明
text	是	string	待合成文本
spk_id	否	int	发音人id，未使用到，默认：0
speed	否	float	合成音频的语速，值范围：(0，3]，默认：1.0，windows 平台不支持变语速
volume	否	float	合成音频的音量，值范围：(0，3]，默认：1.0，值过大可能会存在截幅现象
sample_rate	否	int	合成音频的采样率，只支持下采样，值选择 [0, 8000, 16000]，默认:0，表示与模型采样率一致
save_path	否	string	通过此参数，可以在合成完成后在本地保存一个音频文件，默认值：None，表示不保存音频，保存音频格式支持wav和pcm
```

```shell
apt-get install protoc
ubuntu protoc
wav to base64
https://codebeautify.org/audio-to-base64-converter

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

```shell
https://zhuanlan.zhihu.com/p/268016844
docker pull hub.baidubce.com/paddlepaddle/serving:latest
docker run -p 9292:9292 --name test -dit hub.baidubce.com/paddlepaddle/serving:latest
docker exec -it test bash
docker run -p 9292:9292 --net host --name local_test -dit hub.baidubce.com/paddlepaddle/serving:latest
docker cp ocr local_test:opt
docker exec -it local_test bash
python3 -m pip install paddlepaddle -i https://mirror.baidu.com/pypi/simple

pip install paddle-serving-server -i https://pypi.tuna.tsinghua.edu.cn/simple

nohup python3 ocr_debugger_server.py cpu &

docker run -p 9292:9292 --name server_test_gpu --gpus 2 paddlepaddle/serving:latest-cuda10.0-cudnn7-devel
nvidia-docker pull hub.baidubce.com/paddlepaddle/serving:latest-cuda9.0-cudnn7
docker version

docker oceanbase
https://developer.aliyun.com/article/789554
https://hub.docker.com/repository/docker/obpilot/oceanbase-ce

docker pull obpilot/oceanbase-ce:latest
docker images
docker run -itd -m 10G --name oceanbase-ce  obpilot/oceanbase-ce:latest
# -v /data:/data
docker run -itd --name oceanbase-ce obpilot/oceanbase-ce:latest
docker ps
docker logs -f oceanbase-ce
docker exec -it oceanbase-ce bash
obd cluster list
obd cluster start obdemo

admin
adminPWD123

obclient -h127.1 -uroot@sys#obdemo -P2883 -prootPWD123 -c -A oceanbase
obclient -h127.1 -uroot@sys -P2883 -prootPWD123 -c -A oceanbase
alter resource unit sys_unit_config min_cpu=5;
-- 创建资源单元规格
CREATE resource unit S4C1G max_cpu=4, min_cpu=4, max_memory='1G', min_memory='1G', max_iops=10000, min_iops=1000, max_session_num=1000000, max_disk_size='1024G'; 

-- 创建资源池（分配资源）
CREATE resource pool my_pool unit = 'S4C1G', unit_num = 1;

-- 创建实例（mysql类型）
create tenant obmysql resource_pool_list=('my_pool'), primary_zone='RANDOM',comment 'mysql tenant/instance', charset='utf8' set ob_tcp_invited_nodes='%', ob_compatibility_mode='mysql';

exit;

obclient -h 127.1 -uroot@obmysql#obdemo -P2883 -p -c -A test
obclient -h 127.1 -uroot@obmysql -P2883 -p -c -A test
show databases;
source bmsql.sql
show tables;
show tablegroups;

sudo docker stop oceanbase-ce
sudo docker rm oceanbase-ce

https://github.com/oceanbase/obdeploy/tree/master/example
```