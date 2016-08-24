Caesium (Цезий)
===============

Клиент для ii, написанный на python3 с интерфейсом на ncurses.


Конфигурационный файл
---------------------

Файл caesium.cfg очень прост по своей структуре и содержит всего несколько параметров:

  * nodename - условное название ноды
  * editor - команда вызова текстового редактора
  * theme - имя цветовой схемы из директории themes/ без расширения
  * nosplash - указание этой опции отключает splash screen при старте клиента
  * oldquote - включает старый формат цитирования (не рекомендуется для использования)
  * fetch - команда запуска фетчера (ключевые слова для подстановки %node, %echoareas и %to)
  * clone - команда запуска фетчера для клонирования конференций (%node, %echoareas, %to и %clone)
  * db - формат базы сообщений (txt или aio см. aio_readme.txt)
  * to - имя, по которому клиент будет определять сообщения для помещения в карбонку; имён может быть несколько, указывать их необходимо через запятую без пробела после неё
  * node - адрес для работы с нодой
  * auth - строка авторизации для отправки сообщений на ноду
  * echo - название эхоконференции и описание
  * stat - название эхоконференции и описание (эха не попадает в архив, но и не синхронизируется с нодой)
  * archive - название и описание эхоконференции в архиве (архивные эхоконференции доступны только для чтения и не синхронизируются с нодой).
  * browser - команда запуска веб-браузера для открытия ссылок в сообщениях.

Клиент умеет работать с произвольным количеством нод. Описание каждой ноды в конфиге начинается с параметра nodename. Параметр editor можно указывать в произвольном месте конфигурационного файла.

Пример caesium.cfg:

```
editor nano
nosplash
db aio

fetch ./fetcher_aio.py -w -n %node -e %echoareas -to %to
clone ./fetcher_aio.py -w -n %node -e %echoareas -c %clone -to %to
send ./sender.py -w -n %node -m %nodename -a %auth

nodename nodeN1
node http://127.0.0.1:62220/
auth authkey1
to anonymous

echo im.15
echo linux.15
archive linux.14

nodename nodeN2
node http://some.node.example/
auth authkey2
to John Doe

echo pol.15
echo humor.15
archive pol.15
archive humor.14
```

В этом примере, клиент настроен на работу с двумя нодами.


Клавиши управления
------------------

Экран выбора эхоконференции:

  * Курсор вверх/Курсор вниз - выбор эхоконференции
  * Page Up - перевести курсор на экран вверх
  * Page Down - перевести курсор на экран вниз
  * Home - перевести курсор в начало списка
  * End - перевести курсор в конец списка
  * Enter - перейти к чтению выделенной эхоконференции
  * O - просмотр исходящих сообщений
  * Tab - переключение между архивным и основным списком эхоконференций
  * . - переключение на работу со следующей нодой
  * , - переключение на работу с предыдущей нодой
  * C - пометить/снять поментку эху для клонирования (см. ниже)
  * G - получить новые сообщения с ноды
  * S - отправить сообщений
  * E - редактировать файл конфигурации
  * F10 - выход из клиента

Экран чтения эхоконференции:

  * Курсор влево/Курсор вправо - переход между сообщениями
  * - - вернуться на одно сообщение назад в цепочке ответов
  * = - вернуться на одно сообщение в цепочке
  * Курсор вверх/Курсор вниз - прокрутка сообщения
  * Page Up - прокрутка сообщения на экран вверх
  * Page Down - прокрутка сообщения на экран вниз
  * Home - прокрутка в начало сообщения
  * End - прокрутка в конец сообщения
  * < - перейти в начало эхоконференции
  * > - перейти в конец эхоконференции
  * Esc - выход из режима чтения в режим выбора эхоконференции
  * F10 - выход из клиента
  * F - пометить сообщение как избранное
  * W - сохранить сообщение в файл
  * S - показать тему сообщения в messagebox (полезно, если тема не входит в ширину экрана)
  * M - показать id сообщения и адрес станции, с которой оно пришло
  * I, Ins - написать новое сообщение в текущую эхоконференцию
  * Q - ответить на текущее сообщение с цитированием
  * V - открыть ссылку или вызвать меню выбора ссылки
  * Del - удалить сообщение (работает только при просмотре избранных сообщений)

Экран чтения исходящих сообщений:

  * E - Редактирование неотправленное сообщение


Если вы случайно вызвали функцию нового сообщения, то можно просто удалить весь текст, включая заголовок и сохранить файл либо просто закрыть текстовый редактор без сохранения файла. Тогда оно не попадёт в директорию out/.

Тоссинг происходит непосредственно перед отправкой сообщения. Так что можно предотвратить отправку нежелательного сообщения, удалив из директории out/ соответствующий .out файл.

На экране выбора эхоконференции перед названием может отображаться знак "+". Он указывает на то, что после последнего сообщения, которое читал пользователь есть ещё сообщения в эхоконференции.

По умолчанию клиент получает только последние сообщения в эхе (либо только 200 последних, если эхоконференции ещё нет в базе). В случае, если есть желание получить полную копию эхоконференции, можно пометить её на клонирование. В этом случае возле названия эхи появится символ "*" и при последующем получении почты клиент получит полную копию эхоконференции.