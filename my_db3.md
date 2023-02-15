1. Написать скрипт, возвращающий список имен (только firstname) пользователей без повторений в алфавитном порядке. [ORDER BY]

SELECT DISTINCT firstname \
AS 'Unique name' \
FROM users  \
ORDER BY firstname;

-- -------------------------------------------------------------
2. Выведите количество мужчин старше 35 лет [COUNT].

SELECT COUNT(*) \
AS 'men over 35' FROM profiles \
WHERE gender = 'm' \
AND (YEAR(CURRENT_DATE)-YEAR(birthday))-(DATE_FORMAT(CURRENT_DATE, '%m%d')  
<  DATE_FORMAT(birthday, '%m%d')) > 35;

-- -------------------------------------------------------------
3. Сколько заявок в друзья в каждом статусе? (таблица friend_requests) [GROUP BY]

SELECT status, COUNT(*) \
AS 'Status Count' \
FROM friend_requests \
GROUP BY status;

-- -------------------------------------------------------------
4. Выведите номер пользователя, который отправил больше всех заявок в друзья (таблица friend_requests) [LIMIT].

SELECT COUNT(*) AS number_of_application\
FROM friend_requests \
GROUP BY initiator_user_id \
ORDER BY number_of_application DESC LIMIT 1;

-- -------------------------------------------------------------
5. Выведите названия и номера групп, имена которых состоят из 5 символов [LIKE].

SELECT name\
FROM communities \
WHERE name LIKE '_____';
-- -------------------------------------------------------------