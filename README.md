## runc

`runc` is a CLI tool for spawning and running containers according to the OCF specification.

## State of the project

Currently `runc` is an implementation of the OCF specification.  We are currently sprinting
to have a v1 of the spec out within a quick timeframe of a few weeks, ~July 2015,
so the `runc` config format will be constantly changing until
the spec is finalized.  However, we encourage you to try out the tool and give feedback.

### OCF

How does `runc` integrate with the Open Container Format?  `runc` depends on the types
specified in the [specs](https://github.com/opencontainers/specs) repository.  Whenever
the specification is updated and ready to be versioned `runc` will update it's dependency
on the specs repository and support the update spec.

### Building:

At the time of writing, runc only builds on the Linux platform.

```bash
# create a 'github.com/opencontainers' in your GOPATH/src
cd github.com/opencontainers
git clone https://github.com/opencontainers/runc
cd runc
make
sudo make install
```

### Using:

To run a container, execute `runc start` in the bundle's root directory:
```bash
runc start
/ $ ps
PID   USER     COMMAND
1     daemon   sh
5     daemon   sh
/ $
```

Or you can specify the path to a JSON configuration file:
```bash
runc start config.json
/ $ ps
PID   USER     COMMAND
1     daemon   sh
5     daemon   sh
/ $
```
Note: the use of the `start` command is required when specifying a 
configuration file.

### OCF Container JSON Format:

Below is a sample `config.json` configuration file. It assumes that
the file-system is found in a directory called `rootfs` and there is a
user with uid and gid of `0` defined within that file-system.

```json
{
    "version": "pre-draft",
    "platform": {
        "os": "linux",
        "arch": "amd64"
    },
    "process": {
        "terminal": true,
        "user": {
            "uid": 0,
            "gid": 0,
            "additionalGids": null
        },
        "args": [
            "sh"
        ],
        "env": [
            "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
            "TERM=xterm"
        ],
        "cwd": ""
    },
    "root": {
        "path": "rootfs",
        "readonly": true
    },
    "hostname": "shell",
    "mounts": [
        {
            "type": "proc",
            "source": "proc",
            "destination": "/proc",
            "options": ""
        },
        {
            "type": "tmpfs",
            "source": "tmpfs",
            "destination": "/dev",
            "options": "nosuid,strictatime,mode=755,size=65536k"
        },
        {
            "type": "devpts",
            "source": "devpts",
            "destination": "/dev/pts",
            "options": "nosuid,noexec,newinstance,ptmxmode=0666,mode=0620,gid=5"
        },
        {
            "type": "tmpfs",
            "source": "shm",
            "destination": "/dev/shm",
            "options": "nosuid,noexec,nodev,mode=1777,size=65536k"
        },
        {
            "type": "mqueue",
            "source": "mqueue",
            "destination": "/dev/mqueue",
            "options": "nosuid,noexec,nodev"
        },
        {
            "type": "sysfs",
            "source": "sysfs",
            "destination": "/sys",
            "options": "nosuid,noexec,nodev"
        },
        {
            "type": "cgroup",
            "source": "cgroup",
            "destination": "/sys/fs/cgroup",
            "options": "nosuid,noexec,nodev,relatime,ro"
        }
    ],
    "linux": {
        "uidMapping": null,
        "gidMapping": null,
        "rlimits": [
           {
                "type": 7,
                "hard": 1024,
                "soft": 1024
           }
        ],
        "systemProperties": null,
        "resources": {
            "disableOOMKiller": false,
            "memory": {
                "limit": 0,
                "reservation": 0,
                "swap": 0,
                "kernel": 0,
                "swappiness": -1
            },
            "cpu": {
                "shares": 0,
                "quota": 0,
                "period": 0,
                "realtimeRuntime": 0,
                "realtimePeriod": 0,
                "cpus": "",
                "mems": ""
            },
            "blockIO": {
                "blkioWeight": 0,
                "blkioWeightDevice": "",
                "blkioThrottleReadBpsDevice": "",
                "blkioThrottleWriteBpsDevice": "",
                "blkioThrottleReadIopsDevice": "",
                "blkioThrottleWriteIopsDevice": ""
            },
            "hugepageLimits": null,
            "network": {
                "classId": "",
                "priorities": null
            }
        },
        "namespaces": [
            {
                "type": "pid",
                "path": ""
            },
            {
                "type": "network",
                "path": ""
            },
            {
                "type": "ipc",
                "path": ""
            },
            {
                "type": "uts",
                "path": ""
            },
            {
                "type": "mount",
                "path": ""
            }
        ],
        "capabilities": [
            "AUDIT_WRITE",
            "KILL",
            "NET_BIND_SERVICE"
        ],
        "devices": [
                {
                        "type": 99,
                        "path": "/dev/null",
                        "major": 1,
                        "minor": 3,
                        "permissions": "rwm",
                        "fileMode": 438,
                        "uid": 0,
                        "gid": 0
                },
                {
                        "type": 99,
                        "path": "/dev/random",
                        "major": 1,
                        "minor": 8,
                        "permissions": "rwm",
                        "fileMode": 438,
                        "uid": 0,
                        "gid": 0
                },
                {
                        "type": 99,
                        "path": "/dev/full",
                        "major": 1,
                        "minor": 7,
                        "permissions": "rwm",
                        "fileMode": 438,
                        "uid": 0,
                        "gid": 0
                },
                {
                        "type": 99,
                        "path": "/dev/tty",
                        "major": 5,
                        "minor": 0,
                        "permissions": "rwm",
                        "fileMode": 438,
                        "uid": 0,
                        "gid": 0
                },
                {
                        "type": 99,
                        "path": "/dev/zero",
                        "major": 1,
                        "minor": 5,
                        "permissions": "rwm",
                        "fileMode": 438,
                        "uid": 0,
                        "gid": 0
                },
                {
                        "type": 99,
                        "path": "/dev/urandom",
                        "major": 1,
                        "minor": 9,
                        "permissions": "rwm",
                        "fileMode": 438,
                        "uid": 0,
                        "gid": 0
                }
        ]
    }
}
```

### Examples:

#### Using a Docker image (requires version 1.3 or later)

To test using Docker's `busybox` image follow these steps:
* Install `docker` and download the `busybox` image: `docker pull busybox`
* Create a container from that image and export its contents to a tar file:
`docker export $(docker create busybox) > busybox.tar`
* Untar the contents to create your filesystem directory:
```
mkdir rootfs
tar -C rootfs -xf busybox.tar
```
* Create a file called `config.json` using the example from above.  You can also
generate a spec using `runc spec`, redirecting the output into `config.json`
* Execute `runc start` and you should be placed into a shell where you can run `ps`:
```
$ runc start
/ # ps
PID   USER     COMMAND
    1 root     sh
    9 root     ps
```

#### Using runc with systemd

```service
[Unit]
Description=Minecraft Build Server
Documentation=http://minecraft.net
After=network.target

[Service]
CPUQuota=200%
MemoryLimit=1536M
ExecStart=/usr/local/bin/runc
Restart=on-failure
WorkingDirectory=/containers/minecraftbuild

[Install]
WantedBy=multi-user.target
```
