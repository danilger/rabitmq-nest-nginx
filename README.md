# rabitmq-nest-nginx
rabitmq


```markdown
## Настройка RabbitMQ для Nest.js на Ubuntu
```

## 1. Установка RabbitMQ:

```bash
sudo apt update
sudo apt install rabbitmq-server
```

## 2. Запуск и добавление в автозагрузку:

```bash
sudo systemctl start rabbitmq-server
sudo systemctl enable rabbitmq-server
```

## 3. Создание пользователя и виртуальный хост:

```bash
sudo rabbitmqctl add_user ваше_имя_пользователя ваш_пароль
sudo rabbitmqctl set_user_tags ваше_имя_пользователя administrator
sudo rabbitmqctl add_vhost ваш_виртуальный_хост
sudo rabbitmqctl set_permissions -p ваш_виртуальный_хост ваше_имя_пользователя ".*" ".*" ".*"
```

## 4. Включение управления RabbitMQ:

```bash
sudo rabbitmq-plugins enable rabbitmq_management
```

## 5. Откройте порты:

```bash
sudo ufw allow 5672
sudo ufw allow 15672
```

если ufw не установлен то 

```bash
sudo apt install ufw
```

## 6. Проверка:

Откройте веб-интерфейс RabbitMQ в браузере по адресу `http://ваш_сервер:15672/` и войдите, используя созданные вами учетные данные.

## 7. Измените `rabbitmq.config` (по желанию):

Создайте файл, если его нет:

```bash
sudo nano /etc/rabbitmq/rabbitmq.config
```

Добавьте следующие строки:

```plaintext
[
  {rabbit, [
    {loopback_users, []}
  ]}
].
```
В этой конфигурации пустой список, который должен разрешать подключение дефолтного пользователя "guest" не только с локальной машины, но и с удаленных.

Пожалуйста, убедитесь, что после внесения изменений в файл конфигурации, вы перезапустили RabbitMQ, чтобы изменения вступили в силу:

```bash
sudo systemctl restart rabbitmq-server
```

## 8. Обновление файла конфигурации Nginx (если используется):

Если вы используете Nginx на сервере с RabbitMQ, убедитесь, что конфигурация прокси соответствует вашим потребностям и перезапустите nginx:

```bash
service nginx reload
```

## 9. Ваш Nest.js код:

В вашем коде Nest.js для подключения к RabbitMQ используйте следующие параметры:

```typescript
const app = await NestFactory.createMicroservice<MicroserviceOptions>(
  AppModule,
  {
    transport: Transport.RMQ,
    options: {
      urls: ['amqp://ваш_имя_пользователя:ваш_пароль@ваш_сервер/ваш_виртуальный_хост'],
      queue: 'main_queue',
      queueOptions: {
        durable: false,
      },
    },
  },
);
```

Замените `ваш_имя_пользователя`, `ваш_пароль`, `ваш_сервер` и `ваш_виртуальный_хост` на соответствующие значения.
```

Этот текст можно вставить в ваш файл `README.md` на GitHub.
