# 一个nacos的0day

## 环境准备：
```sh
git clone https://github.com/nacos-group/nacos-docker.git
cd nacos-docker

vim .env
# 将修改 NACOS_VERSION=v2.3.1 为 NACOS_VERSION=v2.3.2（20240715，若已为2.3.2则无须改动）

docker-compose -f example/standalone-derby.yaml up
```

## POC利用

- 启动一个可被攻击者（Attacker）和目标（Target）**都可访问**的 flask service
安装依赖 `pip install -r requiments.txt`  

配置 `config.py` 中的 `server_host` 和 `server_port` ，执行 `python3 service.py` 启动 flask service

如一个具有公网的配置
```ini
server_host = '0.0.0.0'
server_port = 5089
```

- RCE利用
配置 `config.py` 中的 `server_host` 和 `server_port`为 flask service 的 IP 和端口，执行 `exploit.py` ，输入目标（Target）和命令即可执行

**flask service 与 Attacker 尽量分开，若一定要使用同一个配置，一定要确保配置的 IP 要能被目标（Target）访问**
**若目标（Target）在本地docker部署的不可使用 `127.0.0.1` 作为配置，docker容器内的 `127.0.0.1` 和宿主机的 `127.0.0.1` 是不同的。**

参考：https://mp.weixin.qq.com/s/AxczsjhcrM_a_qNVyT38GA

另一个POC: https://github.com/FFR66/Nacos_Rce

# 原说明内容

```
因为某些原因，公开一个nacos的0day

环境准备： 下载nacos2.3.2或2.4.0版本，解压，使用 startup.cmd -m standalone 启动nacos 补充POC信息 POC是一个python项目，依赖requests和flask，请先使用requiments.txt安装依赖 1.配置config.py中的ip和端口，执行service.py，POC攻击需要启动一个jar包下载的地方，jar包里可以放任意代码，都可执行，我这里放了一个接收参数执行java命令的 2.执行exploit.py，输入地址和命令即可执行。
```

