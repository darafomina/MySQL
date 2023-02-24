*1. Создайте представление с произвольным SELECT-запросом из прошлых уроков [CREATE VIEW]\
Из 4 домашнего задания (Подсчитать количество пользователей в каждом сообществе)*

CREATE VIEW usersName\
AS SELECT COUNT(*), name\
FROM users_communities AS uc\
JOIN communities AS c\
ON c.id = uc.community_id\
GROUP BY c.id

*2. Выведите данные, используя написанное представление [SELECT]*


SELECT * FROM usersName;

*3. Удалите представление [DROP VIEW]*

DROP VIEW usersName;

*4.  Сколько новостей (записей в таблице media) у каждого пользователя? Вывести поля: news_count (количество новостей),
user_id (номер пользователя), user_email (email пользователя). Попробовать решить с помощью CTE или с помощью обычного JOIN.*

SELECT COUNT(*) AS news_count,\
user_id, email AS user_email\
FROM media as m\
JOIN users as u on u.id = m.user_id \
GROUP BY user_id;