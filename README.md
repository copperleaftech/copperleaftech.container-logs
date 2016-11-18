# copperleaftech.container-logs
Enables container logging for a compose service via tail > file.log

The advantages writing the logs out to file over simply using gelf from the compose file are that with only UDP support we might loose logs, we cant view the logs on the server retrospectively and we lose multiline support which we could get from filebeat tailing the log.

### What does it do?
Could also interact with a filebeat role to install filebeat and place a config file for the designated log

### Usage
```
  roles:
  - {
     role: copperleaftech.container-logs,
     services: [
       {  name: 'myservice1',
          path: '/data/<myservicepath>',
          log:  '/var/log/containerlogs.log'
       },
       {
         name: 'awesomeness.com',
         path: '/data/awesomeness.com',
         log:  '/var/log/containerlogs.log'
       }
     ]
    }
```

### systemd
1. creates the service file in /etc/systemd
2. set it to startup
```
[Unit]
Description=Docker Compose Logger for {{ service.name }}
After=docker.service
Requires=docker.service

[Service]
RestartSec=3s
Type=simple
ExecStart=/bin/sh -c '/usr/local/bin/docker-compose logs -f >> /var/log/{{ service.log }}.log'
Restart=always
WorkingDirectory={{ service.path }}

[Install]
WantedBy=multi-user.target
```

### sysinitv
TODO

### upstart
TODO
