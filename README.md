# СУБД с точки зрение CAP теоремы

_Теорема Брюера_ (_CAP теорема_) утверждает, что распределенное хранилище данных не может одновременно обеспечить более двух из трех свойств:

- _Согласованность_ (__C__ - consistency)<br/>
Каждый процесс чтения получает последнюю запись или ошибку, соответственно, когда в системе запущено несколько параллельных процессов записи и чтения, то каждое чтение всегда возвращает последнюю запись, сделанную в системе.
- _Доступность_ (__A__ - Availability)<br/>
Принцип доступности в распределенной системе гарантирует, что система всегда остается работоспособной. Каждый запрос получает ответ без ошибок, независимо от индивидуального состояния узла. Но не гарантируется, что ответ содержит самую последнюю запись.
- _Устойчивость к разделению_ (P - Partition Tolerance)<br/>
Система продолжает работу, даже когда отдельный узел не отвечает. Вышедший из строя узел подкрепляется вторичным узлом, поэтому вторичный узел заменяет первичный во время сбоев, а система становится отказоустойчивой.

## MongoDB

В рамках _CAP теоремы_ _MongoDB_ представляет из себя __CP-хранилище данных__.

_MongoDB_ допускает один первичный узел, который получает все операции записи. Когда первичный узел вне доступа, то вторичный узел выбирается в качестве нового первичного узла. Поскольку клиенты не отправляют запросы на запись в течение интервала смены ролей, данные остаются согласованными по всей сети. Таким образом _MongoDB_ предоставляет согласованность данных (__C__) и устойчива к разделению сети (__P__).

## Cassandra

_Cassandra_ использует схему репликации без мастер узла, что обеспечивает _устойчивость к разрыву_ (__P__) и доступность (__A__) кластера, т.к. при недоступности одного узла его функции возьмет на себя другой узел кластера. _Cassandra_ имеет настройки согласованности данных для уменьшения задержек, однако, по-умолчанию Cassandra предоставляет согласованноть в конечном счете (_Eventually Consistent_).<br/>
Как результат _Cassandra_ принято считать _AP-хранилищем данных_ с точки зрения _CAP теоремы_.

## MS SQL

Как и все релляционные базы данных _MS SQL Server_ предоставляет _доступность_ (__A__) и _согласованность_ (__P__) данных во всех узлах кластера, но не устойчив к разделению.<br/>
Таким образом все реплики данных распределенные по разным узлам не противоречат друг другу и любой запрос завершается корректно.<br/>
_MS SQL_ является примером _CA-хранилища данных_
