# Задание 1. SSH-сервер в контейнере

**Что делаем:** превращаем контейнер с Ubuntu в мини-сервер с SSH-доступом.

**Зачем:** учимся заходить в работающий контейнер «по-настоящему» —
так же, как на удалённый сервер.

## Шаг 1. Запустите контейнер с пробросом порта

```bash
docker run -d -p 2222:22 --name ubuntu-ssh ubuntu sleep infinity
```

(`sleep infinity` держит контейнер живым; в вебинаре был `bash`,
он завершается — нам нужен живой контейнер.)

## Шаг 2. Установите SSH-сервер внутри

Зайдите в контейнер:

```bash
docker exec -it ubuntu-ssh bash
```

Внутри выполните:

```bash
apt-get update
apt-get install -y openssh-server
mkdir -p /run/sshd
```

## Шаг 3. Разрешите вход по ключу

Откройте `/etc/ssh/sshd_config` (редактора нет — используйте sed):

```bash
sed -i 's/^#\?PubkeyAuthentication.*/PubkeyAuthentication yes/' /etc/ssh/sshd_config
sed -i 's/^#\?PermitRootLogin.*/PermitRootLogin prohibit-password/' /etc/ssh/sshd_config
```

## Шаг 4. Запустите sshd

```bash
/usr/sbin/sshd
```

Выйдите: `exit`.

## Шаг 5. Проверьте с хоста

```bash
ssh -p 2222 root@localhost
```

Спросит пароль — его мы не задавали, вход по паролю закроем,
поэтому просто прервите (Ctrl+C). Главное — SSH **отвечает**,
значит сервер работает. Вход без пароля настроим в следующем задании.

## Результат

<img width="1119" height="241" alt="image" src="https://github.com/user-attachments/assets/4e765053-cab9-4c2a-931e-9dfd169074ca" />


**В отчёт:** строку баннера SSH из шага 5 (что сервер ответил).
