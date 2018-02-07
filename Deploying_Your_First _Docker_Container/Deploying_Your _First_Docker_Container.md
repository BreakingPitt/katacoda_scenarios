# Deploying Your First Docker Container.

## Step 1 - Running A Container.

### Docker Search Command.

```
$ docker search --filter stars=4500 redis
NAME                DESCRIPTION                                     STARS               OFFICIAL            AUTOMATED
redis               Redis is an open source key-value store th...   4757                [OK]
```

### Docker Run Command.
```
$ docker run -d --name redisMaster redis:latest
9a76064d1278a794ef7eab899d53b5811ea76c5ccbfc7d2854b029ac109b8177
```


## Step 2 - Finding Running Containers

### Docker Ps Command.

```
$ docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS    NAMES
9a76064d1278        redis               "docker-entrypoint..."   4 minutes ago       Up 4 minutes        6379/tcp    dreamy_edison
```

### Docker Inspect Command.

```
$ docker inspect 9a76064d1278a794ef7eab899d53b5811ea76c5ccbfc7d2854b029ac109b8177
[
    {
        "Id": "9a76064d1278a794ef7eab899d53b5811ea76c5ccbfc7d2854b029ac109b8177",
        "Created": "2018-02-07T08:36:35.171758711Z",
        "Path": "docker-entrypoint.sh",
        "Args": [
            "redis-server"
        ],
        "State": {
            "Status": "running",
            "Running": true,
            "Paused": false,
            "Restarting": false,
            "OOMKilled": false,
            "Dead": false,
            "Pid": 469,
            "ExitCode": 0,
            "Error": "",
            "StartedAt": "2018-02-07T08:36:35.440371746Z",
            "FinishedAt": "0001-01-01T00:00:00Z"
        },
        "Image": "sha256:b6dddb991dfa5f8e49b00d2d4ff1e46e6593101ace99cca0febf287cadc4febf",
        "ResolvConfPath": "/var/lib/docker/containers/9a76064d1278a794ef7eab899d53b5811ea76c5ccbfc7d2854b029ac109b8177/resolv.conf",
        "HostnamePath": "/var/lib/docker/containers/9a76064d1278a794ef7eab899d53b5811ea76c5ccbfc7d2854b029ac109b8177/hostname",
        "HostsPath": "/var/lib/docker/containers/9a76064d1278a794ef7eab899d53b5811ea76c5ccbfc7d2854b029ac109b8177/hosts",
        "LogPath": "/var/lib/docker/containers/9a76064d1278a794ef7eab899d53b5811ea76c5ccbfc7d2854b029ac109b8177/9a76064d1278a794ef7eab899d53b5811ea76c5ccbfc7d2854b029ac109b8177-json.log",
        "Name": "/dreamy_edison",
        "RestartCount": 0,
        "Driver": "overlay",
        "Platform": "linux",
        "MountLabel": "",
        "ProcessLabel": "",
        "AppArmorProfile": "",
        "ExecIDs": null,
        "HostConfig": {
            "Binds": null,
            "ContainerIDFile": "",
            "LogConfig": {
                "Type": "json-file",
                "Config": {}
            },
            "NetworkMode": "default",
            "PortBindings": {},
            "RestartPolicy": {
                "Name": "no",
                "MaximumRetryCount": 0
            },
            "AutoRemove": false,
            "VolumeDriver": "",
            "VolumesFrom": null,
            "CapAdd": null,
            "CapDrop": null,
            "Dns": [],
            "DnsOptions": [],
            "DnsSearch": [],
            "ExtraHosts": null,
            "GroupAdd": null,
            "IpcMode": "shareable",
            "Cgroup": "",
            "Links": null,
            "OomScoreAdj": 0,
            "PidMode": "",
            "Privileged": false,
            "PublishAllPorts": false,
            "ReadonlyRootfs": false,
            "SecurityOpt": null,
            "UTSMode": "",
            "UsernsMode": "",
            "ShmSize": 67108864,
            "Runtime": "runc",
            "ConsoleSize": [
                0,
                0
            ],
            "Isolation": "",
            "CpuShares": 0,
            "Memory": 0,
            "NanoCpus": 0,
            "CgroupParent": "",
            "BlkioWeight": 0,
            "BlkioWeightDevice": [],
            "BlkioDeviceReadBps": null,
            "BlkioDeviceWriteBps": null,
            "BlkioDeviceReadIOps": null,
            "BlkioDeviceWriteIOps": null,
            "CpuPeriod": 0,
            "CpuQuota": 0,
            "CpuRealtimePeriod": 0,
            "CpuRealtimeRuntime": 0,
            "CpusetCpus": "",
            "CpusetMems": "",
            "Devices": [],
            "DeviceCgroupRules": null,
            "DiskQuota": 0,
            "KernelMemory": 0,
            "MemoryReservation": 0,
            "MemorySwap": 0,
            "MemorySwappiness": null,
            "OomKillDisable": false,
            "PidsLimit": 0,
            "Ulimits": null,
            "CpuCount": 0,
            "CpuPercent": 0,
            "IOMaximumIOps": 0,
            "IOMaximumBandwidth": 0
        },
        "GraphDriver": {
            "Data": {
                "LowerDir": "/var/lib/docker/overlay/bfd1a3fc8d00e71189ea3a44c1f9e55ad9f3fd4ebb1815bbac5defb8d6b85478/root",
                "MergedDir": "/var/lib/docker/overlay/1fa03d27d9e38e025da9c7c4ac30889ee198e691f0ed55c7848aba95e0eaf152/merged",
                "UpperDir": "/var/lib/docker/overlay/1fa03d27d9e38e025da9c7c4ac30889ee198e691f0ed55c7848aba95e0eaf152/upper",
                "WorkDir": "/var/lib/docker/overlay/1fa03d27d9e38e025da9c7c4ac30889ee198e691f0ed55c7848aba95e0eaf152/work"
            },
            "Name": "overlay"
        },
        "Mounts": [
            {
                "Type": "volume",
                "Name": "a8e12dfa1861c054ff901877f39b576c52f6e4d12403683bee8a99200597c162",
                "Source": "/var/lib/docker/volumes/a8e12dfa1861c054ff901877f39b576c52f6e4d12403683bee8a99200597c162/_data",
                "Destination": "/data",
                "Driver": "local",
                "Mode": "",
                "RW": true,
                "Propagation": ""
            }
        ],
        "Config": {
            "Hostname": "9a76064d1278",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "ExposedPorts": {
                "6379/tcp": {}
            },
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
                "GOSU_VERSION=1.10",
                "REDIS_VERSION=4.0.2",
                "REDIS_DOWNLOAD_URL=http://download.redis.io/releases/redis-4.0.2.tar.gz",
                "REDIS_DOWNLOAD_SHA=b1a0915dbc91b979d06df1977fe594c3fa9b189f1f3d38743a2948c9f7634813"
            ],
            "Cmd": [
                "redis-server"
            ],
            "ArgsEscaped": true,
            "Image": "redis",
            "Volumes": {
                "/data": {}
            },
            "WorkingDir": "/data",
            "Entrypoint": [
                "docker-entrypoint.sh"
            ],
            "OnBuild": null,
            "Labels": {}
        },
        "NetworkSettings": {
            "Bridge": "",
            "SandboxID": "a58fedb113dcad6825c27a0c1dec83ed207d1509349b64f6132ba92c934e98ff",
            "HairpinMode": false,
            "LinkLocalIPv6Address": "",
            "LinkLocalIPv6PrefixLen": 0,
            "Ports": {
                "6379/tcp": null
            },
            "SandboxKey": "/var/run/docker/netns/a58fedb113dc",
            "SecondaryIPAddresses": null,
            "SecondaryIPv6Addresses": null,
            "EndpointID": "f87470165fbe35051406c21a1bd6919189330c639c423bfd99d7f5e9d705a8e5",
            "Gateway": "172.18.0.1",
            "GlobalIPv6Address": "",
            "GlobalIPv6PrefixLen": 0,
            "IPAddress": "172.18.0.2",
            "IPPrefixLen": 24,
            "IPv6Gateway": "",
            "MacAddress": "02:42:ac:12:00:02",
            "Networks": {
                "bridge": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": null,
                    "NetworkID": "3aef00a70139b24658230d02e2df29bad859ca837c26742cb1a03d8ad0ec5e59",
                    "EndpointID": "f87470165fbe35051406c21a1bd6919189330c639c423bfd99d7f5e9d705a8e5",
                    "Gateway": "172.18.0.1",
                    "IPAddress": "172.18.0.2",
                    "IPPrefixLen": 24,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": "02:42:ac:12:00:02",
                    "DriverOpts": null
                }
            }
        }
    }
]
```
### Docker Logs Command.

```
$ docker logs 9a76064d1278a794ef7eab899d53b5811ea76c5ccbfc7d2854b029ac109b8177
1:C 07 Feb 08:36:35.449 # oO0OoO0OoO0Oo Redis is starting oO0OoO0OoO0Oo
1:C 07 Feb 08:36:35.449 # Redis version=4.0.2, bits=64, commit=00000000, modified=0, pid=1, just started
1:C 07 Feb 08:36:35.449 # Warning: no config file specified, using the default config. In order to specify a config fileuse redis-server /path/to/redis.conf
1:M 07 Feb 08:36:35.452 * Running mode=standalone, port=6379.
1:M 07 Feb 08:36:35.452 # WARNING: The TCP backlog setting of 511 cannot be enforced because /proc/sys/net/core/somaxconn is set to the lower value of 128.
1:M 07 Feb 08:36:35.452 # Server initialized
1:M 07 Feb 08:36:35.452 # WARNING overcommit_memory is set to 0! Background save may fail under low memory condition. Tofix this issue add 'vm.overcommit_memory = 1' to /etc/sysctl.conf and then reboot or run the command 'sysctl vm.overcommit_memory=1' for this to take effect.
1:M 07 Feb 08:36:35.452 # WARNING you have Transparent Huge Pages (THP) support enabled in your kernel. This will createlatency and memory usage issues with Redis. To fix this issue run the command 'echo never > /sys/kernel/mm/transparent_hugepage/enabled' as root, and add it to your /etc/rc.local in order to retain the setting after a reboot. Redis must be restarted after THP is disabled.
1:M 07 Feb 08:36:35.452 * Ready to accept connections
```
