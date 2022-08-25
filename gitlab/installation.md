```
version: '2'
services:
    gitlab:
      image: 'gitlab/gitlab-ce:15.1.5-ce.0'
      container_name: "gitlab"
      restart: always
      hostname: 'gtlab.linux.io'
      environment:
        TZ: 'Asia/Shanghai'
        GITLAB_OMNIBUS_CONFIG: |
          external_url 'http://gitlab.linux.io'
          gitlab_rails['gitlab_shell_ssh_port'] = 10080
          gitlab_rails['time_zone'] = 'Asia/Shanghai'
      ports:
        - '80:80'
        - '10080:22'
        - '443:443'
      volumes:
        - /data/application/gitlab/config:/etc/gitlab
        - /data/application/logs:/var/log/gitlab
        - /data/application/gitlab/data:/var/opt/gitlab
```
