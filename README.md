###Caesium (Цезий)

Клиент для *ii* (http://ii-net.tk/), написанный на python3 с интерфейсом на ncurses.


####Конфигурационный файл

Файл ``caesium.cfg очень прост по своей структуре и содержит всего несколько параметров:

  * nodename - условное название ноды
  * editor - команда вызова текстового редактора
  * theme - имя цветовой схемы из директории themes/ без расширения
  * to - имя, по которому клиент будет определять сообщения для помещения в карбонку
  * node - адрес для работы с нодой
  * auth - строка авторизации для отправки сообщений на ноду
  * echo - название эхоконференции и описание
  * archive - название и описание эхоконференции в архиве (архивные эхоконференции доступны только для чтения и не синхронизируются с нодой).

Клиент умеет работать с произвольным количеством нод. Описание каждой ноды в конфиге начинается с параметра nodename. Параметр editor можно указывать в произвольном месте конфигурационного файла.

####Пример caesium.cfg:
``cfg
editor nano

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

В этом примере, клиент настроен на работу с двумя нодами.


####Клавиши управления

**Экран выбора эхоконференции:**

  * Курсор вверх/Курсор вниз - выбор эхоконференции
  * Page Up - перевести курсор на экран вверх
  * Page Down - перевести курсор на экран вниз
  * Home - перевести курсор в начало списка
  * End - перевести курсор в конец списка
  * Enter - перейти к чтению выделенной эхоконференции
  * Tab - переключение между архивным и основным списком эхоконференций
  * . - переключение на работу со следующей нодой
  * , - переключение на работу с предыдущей нодой
  * C - пометить/снять поментку эху для клонирования (см. ниже)
  * G - получить новые сообщения с ноды
  * S - отправить сообщения
  * F10 - выход из клиента

**Экран чтения эхоконференции:**

  * Курсор влево/Курсор вправо - переход между сообщениями
  * Курсор вверх/Курсор вниз - прокрутка сообщения
  * Page Up - прокрутка сообщения на экран вверх
  * Page Down - прокрутка сообщения на экран вниз
  * Home - перейти в начало эхоконференции
  * End - перейти в конец эхоконференции
  * Esc - выход из режима чтения в режим выбора эхоконференции
  * F10 - выход из клиента
  * F - пометить сообщение как избранное
  * W - сохранить сообщение в файл
  * I - написать новое сообщение в текущую эхоконференцию
  * Q - ответить на текущее сообщение с цитированием
  * Del - удалить сообщение (работает только при просмотре избранных сообщений)


Если вы случайно вызвали функцию нового сообщения, то можно просто удалить весь текст, включая заголовок и сохранить файл. Тогда оно не попадёт в директорию out/.

Тоссинг происходит непосредственно перед отправкой сообщения. Так что можно предотвратить отправку нежелательного сообщения, удалив из директории out/ соответствующий .out файл.

На экране выбора эхоконференции перед названием может отображаться знак "+". Он указывает на то, что после последнего сообщения, которое читал пользователь есть ещё сообщения в эхоконференции.

По умолчанию клиент получает только последние сообщения в эхе (либо только 50 последних, если эхоконференции ещё нет в базе). В случае, если есть желание получить полную копию эхоконференции, можно пометить её на клонирование. В этом случае возле названия эхи появится символ "\*" и при последующем получении почты клиент получит полную копию эхоконференции.

В директории tools/ находятся полезные скрипты.

Сам клиент пока не умеет работать с базой сообщений в формате sqlite3, но уже есть скрипты, позволяющие экспортировать и импортировать базу сообщений в этот формат. Директория out/ с исходящими сообщениями в новую базу не попадают.

Это позволяет делать быстрые резервные копии или переносить базу с компьютера на компьютер.

**txt2sqlite.py** создаёт в корневом каталоге клиента файл ii.db, в который переносит все данные из директорий echo/ и msg/.

**sqlite2txt.py** создаёт в папке tools базу сообщений в формате txt (директории echo/ и msg/). Это сделано для того, чтобы не повредить рабочую базу в процессе отладки работы скриптов.

**rmecho.py** принимает в качестве параметра название эхоконференции и удаляет её из базы. Работает только с текстовой базой сообщений.
