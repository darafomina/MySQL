
-- 1. Подсчитать количество групп, в которые вступил каждый пользователь.*

SELECT \
CONCAT(firstname, ' ', lastname) AS owner,\
COUNT(*)\
FROM users AS u\
JOIN users_communities AS uc ON u.id = uc.user_id\
GROUP BY u.id\
ORDER BY COUNT(*) DESC;

-- 2. Подсчитать количество пользователей в каждом сообществе.

SELECT COUNT(*), name\
FROM users_communities  AS uc\
JOIN communities AS c\
ON c.id = uc.community_id\
GROUP BY c.id;

-- 3. Пусть задан некоторый пользователь. Из всех пользователей соц. сети найдите человека, который больше всех общался с выбранным пользователем (написал ему сообщений).

SELECT from_user_id,\
 CONCAT(u.firstname, ' ', u.lastname) AS name,\
 COUNT(*) as 'messages count'\
FROM messages AS m\
JOIN users AS u ON u.id = m.from_user_id\
WHERE to_user_id = 1\
GROUP BY from_user_id\
ORDER BY count(*) DESC\
LIMIT 1;

/*Задача 4
 общее количество лайков, которые получили пользователи младше 18 лет.
 (решение с вложенными запросами) */

select count(*) -- количество лайков\
from likes\
where media_id in ( -- все медиа записи таких пользователей\
	select id \
	from media \
	where user_id in ( -- все пользователи младше 18 лет\
		select \
			user_id\
		-- 	, birthday\
		from profiles as p\
		where  YEAR(CURDATE()) - YEAR(birthday) < 18
	)
);
 
-- (решение с объединением таблиц)\
select count(*) -- количество лайков\
from likes l\
join media m on l.media_id = m.id\
join profiles p on p.user_id = m.user_id\
where  YEAR(CURDATE()) - YEAR(birthday) < 18;

-- ---------------------------------------------------------------
/* Задача 5
Определить: кто больше поставил лайков (всего) - мужчины или женщины.
(решение с вложенными запросами) */\
select 
	gender
	, count(*)\
from (
	select 
		user_id as user,
		(
			select gender \
			from vk.profiles
			where user_id = user
		) as gender\
	from likes
) as dummy
group by gender;

-- решение с объединением таблиц
SELECT  gender, COUNT(*)
from likes
join profiles on likes.user_id = profiles.user_id 
group by gender;

***********************************************************
 * Определить кто больше поставил лайков (всего): мужчины или женщины.
 * Альтернативное решение.
 ************************************************************* 
SELECT gender FROM (\
	SELECT gender, COUNT((SELECT COUNT(*) FROM likes as L where L.user_id = P.\user_id)) as gender_likes_count FROM profiles as P
	WHERE gender = 'm'\
	GROUP BY gender\
	UNION ALL\
	SELECT gender, COUNT((SELECT COUNT(*) FROM likes as L where L.user_id = P.\user_id)) FROM profiles as P
	WHERE gender = 'f'\
	GROUP BY gender\
) AS T\
GROUP BY gender\
ORDER BY MAX(gender_likes_count) DESC\
LIMIT 1;