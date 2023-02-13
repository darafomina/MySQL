**Создание таблиц**

CREATE TABLE movie (\
  id_movie INT NOT NULL AUTO_INCREMENT,\
  name_movie VARCHAR(45),\
  rating INT,\
  year_release INT,\
  id_studio INT,\
  country VARCHAR(45),\
  id_genre INT,\
  PRIMARY KEY (id_movie));
  
INSERT movie(name_movie, rating, year_release, id_studio, country, id_genre)\
VALUES\
  ('Снежная королева: Разморозка', 9, 2022, 1, 'Россия', 1), \
  ('Зелёная миля', 9, 1999, 2, 'США', 2),\
  ('Список Шиндлера', 9, 1993, 3, 'США', 3);


CREATE TABLE  distribution  
(\
    id_studio INT NOT NULL AUTO_INCREMENT,\
    name_studio VARCHAR(100),\
	PRIMARY KEY(id_studio));

INSERT distribution ( name_studio)\
VALUES \
    ('Анимационная студия Воронеж'), \
	('Castle Rock Entertainment'),\
    ('Universal Pictures');\


CREATE TABLE  genre \
(\
    id_genre INT NOT NULL AUTO_INCREMENT,\
    name_genre VARCHAR(45),\
	PRIMARY KEY(id_genre));

INSERT genre ( name_genre)\
VALUES \
    ('мультфильм'), \
    ('драма'),\
    ('драма');
    

**Продемонстрировать работу с операторами переименования таблиц\полей;**

RENAME TABLE movie TO films; -- для таблицы


ALTER TABLE films CHANGE name_movie name_film varchar(45); -- для поля

**операторами добавления и удаления таблиц\полей;**

ALTER TABLE movie
ADD actor VARCHAR(45) NULL; -- добавление поля


DELETE FROM films
WHERE year_release = 2022; -- удаление для полей

DELETE FROM genre; -- удаление для таблиц

**добавления ключей(внешних);**\
-- DROP TABLE time;\
CREATE TABLE time\
(\
    Id INT PRIMARY KEY AUTO_INCREMENT,\
    timing INT,\
    CONSTRAINT orders_movie_fk \
    FOREIGN KEY (timing)  REFERENCES movie (id_movie)\
);

**оператором псевдонима;**

SELECT timing AS 'ВСЕГО' FROM time;

**оператором CASE;**\
SELECT name_movie, year_release,\
CASE\
	WHEN year_release = 2022 THEN 'новый'\
    WHEN year_release = 1999 THEN 'старый'\
    ELSE 'древний'\
    END AS age\
    FROM movie;

**оператором IF;**

SELECT name_movie, year_release,\
	IF (year_release > 2000, 'Фильм XXI века', 'Фильм XX века')\
AS century FROM movie;

**Подумать и описать проблемы своего проектирования базы данных;**

Не удалось найти информацию, как поместить в одну ячейку несколько ключей. Пример: если у одного фильма несколько киностудий или жанров. 
Нашла такой комментарий 'Сохранение нескольких значений в одно поле является нарушением первой нормальной формы реляционной модели. 
Реляционные СУБД, чтобы называться реляционными должны не позволять ее нарушать.
Так что прямой ответ - нет, нельзя.'

**Оформить работу так, чтобы результатами было пользоваться удобно не только лично вам.**\
-- Старалась сделать понятнее (сама запуталась).