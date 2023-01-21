# Заготовка проекта react + laravel(backend)
## Инструкция по развёртыванию среды разработки:
### Требования:
1. Установленные docker и docker-compose(^3).

### Подготовка проекта к запуску и запуск:  
1. Настраиваем среду и запускаем контейнеры:
```bash
sudo chgrp -R laravel/www-data laravel/storage laravel/bootstrap/cache
sudo chmod -R ug+rwx laravel/storage laravel/bootstrap/cache
cp laravel/.env.example laravel/.env
cp docker-compose-example.yml docker-compose.yml
docker-compose up
```

2. Применяем миграции
```bash
cd laravel && docker-compose exec app php artisan migrate
```

## Руководство:
### После развёртки контейнеров локальная среда разработки полностью готова.  
- React доступен по адресу: [localhost](http://localhost)
- Laravel досупен по адресу [localhost:81](http://localhost:81)
- По адресу [localhost:8080](http://localhost:8080) будет доступен админер - веб-интерфейс для ваших бд.  

Ссылки на локальные бд, везде пароль homestead:
- [MYSQL](http://localhost:8080/?server=mysql&username=homestead&db=homestead)

### Заливаем данные в бд.
Добавьте фаил дампа базы в папку docker/volumes/mysql/home и выполните импорт базы данных изнутри контейнера:
```bash
docker-compose exec mysql bash -c "zcat /home/ФАИЛ.sql.gz | mysql -u root -prootpass -h 127.0.0.1 --ssl-mode=DISABLED homestead"
```

### Бекап бд.
Выполните бекап базы данных изнутри контейнера:
```bash
docker-compose exec mysql bash -c 'mysqldump -u root -prootpass homestead | gzip > /home/backup$(date +%F).sql.gz'
```
После чего бекап появится в папке docker/volumes/mysql/home
