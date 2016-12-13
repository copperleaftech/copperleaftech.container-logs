# copperleaftech.container-logs
Enables container logging for a compose service via the log tail function

The advantage of writing the logs out to file over simply using gelf from the compose file are that with only UDP support we might lose logs and we can't view the logs on the server retrospectively or in realtime, plus we lose multiline support which we could get from filebeat tailing the log.

Another significant advantage is the encrypted channel support which filebeat offers...!

Please contibute for other OS startup managers. Only systemd is currently supported. 

### What does it do?
Installs to the local service manager to run the command docker-compose tail -f >> file.log for a particular compose service.

### Feature Roadmap
Could also interact with a filebeat role to install filebeat and place a config file for the designated service/log

If used with the container-startup role, dependencies on services could be build in.

### Reliability Note
This is currently still in testing / development.

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

### sysvinit
TODO

### upstart
TODO
