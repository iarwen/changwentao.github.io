title: docker学习-02-docker-run
date: 2016/08/01 16:11:25
categories:
- docker学习
tags: [docker,run]
---

docker运行镜像靠run，run命令会自动从指定的镜像运行，如果镜像不存在，会先自动pull到本地，docker的文件系统称为layer，有发生变化的layer会新生成一个layer，有一个image id。
目前我使用docker，一般是用镜像启动成一个容器，然后以后就直接stop/start这个容器
常用的组合
端口：-p 3306:3306
起名：--name mysql
磁盘映射：-v /data/mysql:/var/lib/mysql
例如
```
docker run --name mysql -p 3306:3306 -v /data/mysql:/var/lib/mysql mysql:5.6-jessie
```
<!--more-->
有时候Docker上的系统会对时间敏感，有可能由于时区的原因导致时间不正确，这个时候
就要使用环境变量设置时区，保证运行环境的正确
```
docker run --rm -it -e TZ=Asia/Shanghai sonatype/nexus date
```
贴一下docker run的二级命令，很多个子命令
```
root@ubuntu:~# docker run --help
`Usage:  docker run [OPTIONS] IMAGE [COMMAND] [ARG...]`
Run a command in a new container

  -a, --attach=[]                 Attach to STDIN, STDOUT or STDERR
  --add-host=[]                   Add a custom host-to-IP mapping (host:ip)
  --blkio-weight                  Block IO (relative weight), between 10 and 1000
  --blkio-weight-device=[]        Block IO weight (relative device weight)
  --cpu-shares                    CPU shares (relative weight)
  --cap-add=[]                    Add Linux capabilities
  --cap-drop=[]                   Drop Linux capabilities
  --cgroup-parent                 Optional parent cgroup for the container
  --cidfile                       Write the container ID to the file
  --cpu-period                    Limit CPU CFS (Completely Fair Scheduler) period
  --cpu-quota                     Limit CPU CFS (Completely Fair Scheduler) quota
  --cpuset-cpus                   CPUs in which to allow execution (0-3, 0,1)
  --cpuset-mems                   MEMs in which to allow execution (0-3, 0,1)
  -d, --detach                    Run container in background and print container ID
  --detach-keys                   Override the key sequence for detaching a container
  --device=[]                     Add a host device to the container
  --device-read-bps=[]            Limit read rate (bytes per second) from a device
  --device-read-iops=[]           Limit read rate (IO per second) from a device
  --device-write-bps=[]           Limit write rate (bytes per second) to a device
  --device-write-iops=[]          Limit write rate (IO per second) to a device
  --disable-content-trust=true    Skip image verification
  --dns=[]                        Set custom DNS servers
  --dns-opt=[]                    Set DNS options
  --dns-search=[]                 Set custom DNS search domains
  -e, --env=[]                    Set environment variables
  --entrypoint                    Overwrite the default ENTRYPOINT of the image
  --env-file=[]                   Read in a file of environment variables
  --expose=[]                     Expose a port or a range of ports
  --group-add=[]                  Add additional groups to join
  -h, --hostname                  Container host name
  --help                          Print usage
  -i, --interactive               Keep STDIN open even if not attached
  --ip                            Container IPv4 address (e.g. 172.30.100.104)
  --ip6                           Container IPv6 address (e.g. 2001:db8::33)
  --ipc                           IPC namespace to use
  --isolation                     Container isolation technology
  --kernel-memory                 Kernel memory limit
  -l, --label=[]                  Set meta data on a container
  --label-file=[]                 Read in a line delimited file of labels
  --link=[]                       Add link to another container
  --log-driver                    Logging driver for container
  --log-opt=[]                    Log driver options
  -m, --memory                    Memory limit
  --mac-address                   Container MAC address (e.g. 92:d0:c6:0a:29:33)
  --memory-reservation            Memory soft limit
  --memory-swap                   Swap limit equal to memory plus swap: '-1' to enable unlimited swap
  --memory-swappiness=-1          Tune container memory swappiness (0 to 100)
  --name                          Assign a name to the container
  --net=default                   Connect a container to a network
  --net-alias=[]                  Add network-scoped alias for the container
  --oom-kill-disable              Disable OOM Killer
  --oom-score-adj                 Tune host's OOM preferences (-1000 to 1000)
  -P, --publish-all               Publish all exposed ports to random ports
  -p, --publish=[]                Publish a container's port(s) to the host
  --pid                           PID namespace to use
  --pids-limit                    Tune container pids limit (set -1 for unlimited)
  --privileged                    Give extended privileges to this container
  --read-only                     Mount the container's root filesystem as read only
  --restart=no                    Restart policy to apply when a container exits
  --rm                            Automatically remove the container when it exits
  --security-opt=[]               Security Options
  --shm-size                      Size of /dev/shm, default value is 64MB
  --sig-proxy=true                Proxy received signals to the process
  --stop-signal=SIGTERM           Signal to stop a container, SIGTERM by default
  -t, --tty                       Allocate a pseudo-TTY
  --tmpfs=[]                      Mount a tmpfs directory
  -u, --user                      Username or UID (format: <name|uid>[:<group|gid>])
  --ulimit=[]                     Ulimit options
  --userns                        User namespace to use
  --uts                           UTS namespace to use
  -v, --volume=[]                 Bind mount a volume
  --volume-driver                 Optional volume driver for the container
  --volumes-from=[]               Mount volumes from the specified container(s)
  -w, --workdir                   Working directory inside the container
```


![](http://alwen.qiniudn.com//github/img/db6a0d5d0e95517db58a662ec5226fef.jpg)
