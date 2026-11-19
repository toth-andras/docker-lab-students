# Задание 3. Передача файла из контейнера (scp)

**Что делаем:** создаём файл внутри контейнера и забираем его на хост
по SSH — без пароля.

## Шаг 1. Создайте файл в контейнере

```bash
ssh -i ~/.ssh/docker_lab_key -p 2222 root@localhost \
  'echo "Hello from container" > /root/testfile.txt'
```

Обратите внимание: команду можно передать прямо в ssh, не заходя
в интерактивную сессию.

## Шаг 2. Заберите файл на хост

```bash
scp -i ~/.ssh/docker_lab_key -P 2222 root@localhost:/root/testfile.txt ./
cat testfile.txt
```

Разбор:
- `scp` — копирование по SSH;
- `-P 2222` — у scp порт задаётся большой буквой (у ssh — маленькой,
  частая путаница!);
- `./` — куда класть (текущая папка).

## Шаг 3 (наоборот). Отправьте файл в контейнер

```bash
echo "Hello from host" > hostfile.txt
scp -i ~/.ssh/docker_lab_key -P 2222 hostfile.txt root@localhost:/root/
ssh -i ~/.ssh/docker_lab_key -p 2222 root@localhost 'cat /root/hostfile.txt'
```

## Результат

<img width="1681" height="473" alt="image" src="https://github.com/user-attachments/assets/aac7706e-3ed9-432b-968d-f1f9dc679487" />


**В отчёт:** вывод `cat testfile.txt` на хосте и `cat /root/hostfile.txt`
в контейнере.

> После задания контейнер можно убрать: `docker rm -f ubuntu-ssh`.
