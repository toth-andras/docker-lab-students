# Задание 2. Вход по ключу без пароля

**Что делаем:** генерируем SSH-ключ на хосте и добавляем публичную часть
в контейнер.

**Зачем:** так устроен доступ к любому боевому серверу: пароль отключён,
вход только по ключу.

## Шаг 1. Сгенерируйте ключ на хосте

```bash
ssh-keygen -t ed25519 -f ~/.ssh/docker_lab_key -N ""
```

(`-N ""` — пустая passphrase, для лабораторной допустимо.)

## Шаг 2. Скопируйте публичный ключ в контейнер

```bash
cat ~/.ssh/docker_lab_key.pub
```

Скопируйте вывод (одна длинная строка).

## Шаг 3. Добавьте ключ в контейнер

```bash
docker exec -it ubuntu-ssh bash
```

Внутри (вместо `ВСТАВЬ_КЛЮЧ` — строка из шага 2):

```bash
mkdir -p /root/.ssh
chmod 700 /root/.ssh
echo "ВСТАВЬ_КЛЮЧ" >> /root/.ssh/authorized_keys
chmod 600 /root/.ssh/authorized_keys
exit
```

## Шаг 4. Войдите по ключу

```bash
ssh -i ~/.ssh/docker_lab_key -p 2222 root@localhost
```

<img width="1301" height="526" alt="image" src="https://github.com/user-attachments/assets/fcad6357-2bd6-487a-96ab-9d66ecdbd626" />


Должно впустить **без пароля**. Внутри сразу выйдите: `exit`.

## Результат

**В отчёт:** доказательство входа без пароля (например, вывод
`ssh -i ~/.ssh/docker_lab_key -p 2222 root@localhost hostname`).
