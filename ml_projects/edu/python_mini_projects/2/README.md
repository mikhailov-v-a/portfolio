## В следующих заданиях мы будем работать с базой данных, в которой имеется три таблицы:

1. Таблица с данными о пользователях (user):
    - id - уникальный идентификатор пользователя (primary key)
    - gender - пол
    - age - возраст
    - country - страна
    - city - город
    - exp_group - экспериментальная группа
    - os - операционная система
    - source - источник трафика
2. Таблица с данными о постах (post):
    - id - уникальный идентификатор поста (primary key)
    - text - текст поста
    - topic - тема поста
3. Таблица с данными о действиях пользователей (feed_action):
    - user_id (——>) user (id)- идентификатор пользователя     
    - post_id (——>) post (id)- идентификатор поста     
    - action - совершенное в сети действие     
    - time- время действия