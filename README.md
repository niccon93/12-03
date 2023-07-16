# Домашнее задание к занятию «SQL. Часть 1»

### Инструкция по выполнению домашнего задания

1. Сделайте fork [репозитория c шаблоном решения](https://github.com/netology-code/sys-pattern-homework) к себе в Github и переименуйте его по названию или номеру занятия, например, https://github.com/имя-вашего-репозитория/gitlab-hw или https://github.com/имя-вашего-репозитория/8-03-hw).
2. Выполните клонирование этого репозитория к себе на ПК с помощью команды `git clone`.
3. Выполните домашнее задание и заполните у себя локально этот файл README.md:
   - впишите вверху название занятия и ваши фамилию и имя;
   - в каждом задании добавьте решение в требуемом виде: текст/код/скриншоты/ссылка;
   - для корректного добавления скриншотов воспользуйтесь инструкцией [«Как вставить скриншот в шаблон с решением»](https://github.com/netology-code/sys-pattern-homework/blob/main/screen-instruction.md);
   - при оформлении используйте возможности языка разметки md. Коротко об этом можно посмотреть в [инструкции по MarkDown](https://github.com/netology-code/sys-pattern-homework/blob/main/md-instruction.md).
4. После завершения работы над домашним заданием сделайте коммит (`git commit -m "comment"`) и отправьте его на Github (`git push origin`).
5. Для проверки домашнего задания преподавателем в личном кабинете прикрепите и отправьте ссылку на решение в виде md-файла в вашем Github.
6. Любые вопросы задавайте в чате учебной группы и/или в разделе «Вопросы по заданию» в личном кабинете.

Желаем успехов в выполнении домашнего задания.

---

Задание можно выполнить как в любом IDE, так и в командной строке.

### Задание 1

Получите уникальные названия районов из таблицы с адресами, которые начинаются на “K” и заканчиваются на “a” и не содержат пробелов.

```
SELECT DISTINCT district as Районы
FROM address
WHERE ((district LIKE 'K%a') AND (district NOT LIKE '% %'));
```

![img](img/1-1.PNG)


### Задание 2

Получите из таблицы платежей за прокат фильмов информацию по платежам, которые выполнялись в промежуток с 15 июня 2005 года по 18 июня 2005 года **включительно** и стоимость которых превышает 10.00.

```
select * from payment
where date(payment_date) >= '2005-06-15' and date(payment_date) <= '2005-06-18 23:59:59' and amount > 10.00
order by payment_date asc;
```

![img](img/2.PNG)

### Задание 3

Получите последние пять аренд фильмов.

```
select r.rental_date as Дата, f.title as Фильм
from rental r
join inventory i on i.inventory_id = r.inventory_id
join film f on i.film_id = f.film_id
order by r.rental_date desc limit 5;
```

![img](img/3.PNG)

### Задание 4

Одним запросом получите активных покупателей, имена которых Kelly или Willie. 

Сформируйте вывод в результат таким образом:
- все буквы в фамилии и имени из верхнего регистра переведите в нижний регистр,
- замените буквы 'll' в именах на 'pp'.

```
SELECT 
    LOWER(c.first_name) AS имя,
    LOWER(c.last_name) AS фамилия,
    REPLACE(LOWER(c.first_name), 'll', 'pp') AS имя_замена
FROM 
    customer c
WHERE 
    c.active = 1 
    AND (TRIM(LOWER(c.first_name)) LIKE 'kelly' OR TRIM(LOWER(c.first_name)) LIKE 'willie')
ORDER BY 
    LOWER(c.last_name) ASC;
```

![img](img/4-4.PNG)

## Дополнительные задания (со звёздочкой*)
Эти задания дополнительные, то есть не обязательные к выполнению, и никак не повлияют на получение вами зачёта по этому домашнему заданию. Вы можете их выполнить, если хотите глубже шире разобраться в материале.

### Задание 5*

Выведите Email каждого покупателя, разделив значение Email на две отдельных колонки: в первой колонке должно быть значение, указанное до @, во второй — значение, указанное после @.

```
select lower(c.email) as Почтовый_адрес, lower(substring_index(c.email, '@', 1)) as Почтовый_адрес_до, right(c.email, length(c.email)-position('@' in c.email)) as Почтовый_адрес_после
from customer c
order by c.email asc;
```

![img](img/5.PNG)

### Задание 6*

Доработайте запрос из предыдущего задания, скорректируйте значения в новых колонках: первая буква должна быть заглавной, остальные — строчными.

```
SELECT 
    LOWER(c.email) AS Почтовый_адрес,
    CONCAT(
        UPPER(LEFT(SUBSTRING_INDEX(c.email, '@', 1), 1)),
        LOWER(SUBSTRING(SUBSTRING_INDEX(c.email, '@', 1), 2))
    ) AS Почтовый_адрес_до,
    CONCAT(
        UPPER(LEFT(SUBSTRING_INDEX(c.email, '@', 1), 1)),
        LOWER(RIGHT(c.email, LENGTH(c.email) - POSITION('@' IN c.email)))
    ) AS Почтовый_адрес_после
FROM customer c
ORDER BY c.email ASC;
```

![img](img/6.PNG)
