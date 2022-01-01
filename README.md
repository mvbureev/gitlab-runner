# gitlab-runner

После выполнения docker-compose.yml:

Авторизуйтесь как root:
```shell
sudo su
```
- Отредактируйте конфиг gitlab runner:
```shell
nano /var/lib/docker/volumes/gitlab-runner-config/_data/config.toml
```
- Вы должны поправить 2 строчки:
```toml
     privileged = true
     volumes = ["/cache", "/var/run/docker.sock:/var/run/docker.sock"]
```
- Чтобы конфиг выглядел примерно так:
```toml
concurrent = 1
check_interval = 0

[session_server]
   session_timeout = 1800

[[runners]]
   name = "mvbureev.ru"
   url = "https://gitlab.com/"
   token = "{{YOUR_TOKEN}}"
   executor = "docker"
   [runners.custom_build_dir]
   [runners.cache]
     [runners.cache.s3]
     [runners.cache.gcs]
     [runners.cache.azure]
   [runners.docker]
     tls_verify = false
     image = "docker:stable"
     privileged = true
     disable_entrypoint_overwrite = false
     oom_kill_disable = false
     disable_cache = false
     volumes = ["/cache", "/var/run/docker.sock:/var/run/docker.sock"]
     shm_size = 0
```
