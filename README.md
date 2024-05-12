######Условие задания
Будучи тимлидом команды разработки, вы получили от менеджера проекта задачу повысить скорость разработки. Звучит, как начало плохого анекдота, но, тем не менее, решение вам все же нужно найти. В ходе размышлений и изучений различного внешнего опыта других команд разработки вы решили попробовать инструменты геймификации. То есть применить техники и подходы игрового характера с целью повышения вовлеченности команды в решение задач.

Вами была придумана рейтинговая таблица самых активных контрибьютеров за спринт. Что это значит в теории: по окончании итерации (4 рабочие недели) выгружается список коммитов, сделанных в релизную ветку продукта, и на его основе вычисляются трое самых активных разработчиков, сделавших наибольшее количество коммитов. В зависимости от занятого места, разработчик получает определенное количество внутренней валюты вашей компании, которую он впоследствии может обменять на какие-то товары из внутреннего магазина.

На практике вы видите решение следующим образом: на следующий день после окончания спринта в 00:00 запускается автоматическая процедура, которая забирает файл с данными о коммитах в релизную ветку, сделанных в период спринта, после чего выполняется поиск 3-х самых активных контрибьютеров. Имена найденных разработчиков записываются в файл, который впоследствии отправляется вам на почту.

В рамках практической реализации данной задачи вам необходимо разработать процедуру формирование отчета “Топ-3 контрибьютера”. Данная процедура принимает на вход текстовый файл (commits.txt), содержащий данные о коммитах (построчно). Каждая строка содержит сведения о коммите в релизную ветку в формате: “<Имя пользователя> <Сокращенный хэш коммита> <Дата и время коммита>”. Например: AIvanov 25ec001 2024-04-24T13:56:39.492

К данным предъявляются следующие требования:

имя пользователя может содержать латинские символы в любом регистре, цифры (но не начинаться с них), а также символ "_";
сокращенный хэш коммита представляет из себя строку в нижнем регистре, состояющую из 7 символов: букв латинского алфавита, а также цифр;
дата и время коммита в формате YYYY-MM-ddTHH:mm:ss.
В результате работы процедура формирует новый файл (result.txt), содержащий информацию об именах 3-х самых активных пользователей по одному в каждой строке в порядке убывания места в рейтинге. Пример содержимого файла: AIvanov AKalinina CodeKiller777

Ручной ввод пути к файлу (через консоль, через правку переменной в коде и т.д.) недопустим. Необходимость любых ручных действий с файлами в процессе работы программы будут обнулять решение.


######Автор решения
Цапков Михаил Андреевич

######Описание реализации:
Было создано два модуля: contributorsTop, отвечающий за чтение файла commits.txt, и модуль checkUp, который отвечает за проверку входных данных из файла commits.txt. Хранение путей до файлов commits.txt и result.txt осуществляется в main.py файле для возможности удобного редактирования пути в случае необходимости при проверке. После определения пути до нужных файлов, файл commits.txt отправляется на проверку в функцию commitCheckUp модуля checkUp, где входным параметром является путь до файла commits.txt. Далее мы считываем данные из файла commits.txt в список commits, открываем временный файл temp_commits.txt и вызываем функцию is_valid_commit модуля checkUp, где входным параметром служит каждая строка commit списка строк commits. В данной функции задается регулярное выражение, соответвующее верным входным данным, с которым сравниваются входящие строки, если эта функция возвращает boolean-значение True, то мы записываем строку в наш временным файл, если она возвращает boolean-значение False, то мы не записываем эту строку в наш временный файл. Далее, с помощью функции copyfile модуля shutil стандартной библиотеки python мы копируем все строки с верным форматом данных в наш файл commits.txt, перезаписывая его. После этого проверенный файл commits.txt передается в функцию read_commits модуля contributorsTop.

В функции read_commits попадает проверенный файл commits.txt, каждая строка которого вытягивается и по пробелам разбивается на три элемента: Автор-Хэш коммита-Дата. Далее мы извлекаем из каждого сообщения имя автора и проверяем: если имя автора уже присутствует в словаре commits, где оно является ключом, то мы увеличиваем значение соответствующего значения на один, если имени автора в данном словаре нет, то мы создаем новую пару ключ-значение, со значение равным 1. После этого мы сортируем наш словарь commits по убыванию значений и записываем первые три результата в отсортированный словарь sorted_commits. Этот словарь и возвращает функция read_commits. 

Далее вызывается функция result  с двумя входными параметрами: путем до файла result.txt и отсортированным словарем sorted_commits из функции read_commits. Эта функция просто перезаписывает отсортированный словарь в файл result.txt.


######Инструкция по сборке и запуску решения:
Точкой входа в программу является файл main.py, находящийся по пути Task\main.py

