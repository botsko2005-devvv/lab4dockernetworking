# Lab 4: Networking in Docker

Лабораторна робота з дисципліни «Розгортання інформаційно-комунікаційних систем» (група ІК-41).

## Що виконано
- Переглянуто мережі за замовчуванням (`bridge`, `host`, `none`) та детальну інформацію про `bridge` (`172.17.0.0/16`, шлюз `172.17.0.1`).
- Створено користувацьку мережу `myapp-net` (драйвер `bridge`).
- Запущено контейнери `mynginxapp` (`my-nginx:1.0`) та `db` (`mysql:8`) у мережі `myapp-net` і перевірено зв'язок між ними за іменем (`ping db`).
- Протестовано мережі `host` (`flaskapp-host`) та `none` (`isolated`), а також прокидання портів / NAT (`-p 5000:80`, `flaskinternal` без `-p`, правила `DNAT` у `iptables`).

## Основні команди
```bash
docker network ls
docker network inspect bridge
docker network create myapp-net
docker run -d --name mynginxapp --network=myapp-net -p 8080:80 my-nginx:1.0
docker run -d --name db --network=myapp-net -e MYSQL_ROOT_PASSWORD=1234 mysql:8
docker network inspect myapp-net
docker exec -it mynginxapp ping db
docker run -d --name flaskapp-host --network=host my-nginx:1.0
docker run -dit --name isolated --network=none alpine sh
docker run -d --name flasknat -p 5000:80 my-nginx:1.0
sudo iptables -t nat -L -n
docker run -d --name flaskinternal my-nginx:1.0
