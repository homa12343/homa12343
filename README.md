+ # QiyanasTEMPMAIL
+ QIYANAS TEMPMAIL
 эхо  выключено
echo Преобразование Python скрипта в исполняемый файл...
pyinstaller --onefile --windowed --icon=qiyanka.ico .\qiyana.py
echo Готово! Исполняемый файл создан. Погладь Киану.
Пауза
@ эхо  выключено
echo Установка зависимостей...
pip install -r требования.txt
echo Готово! Все зависимости установлены. Погладь Киану.
Пауза
айоhttp==3.8.1
PyQt5==5.15.4
 пример  =  TempMailApp ()
    бывший . setWindowTitle ( 'QIYANAS TEMPMAIL' )
    ex.show()
    сис . выход ( app . exec_ ())
    сис . выход ( app . exec_ ())
импортировать  систему
импортировать  aiohttp
импортировать  асинхронный код
из  PyQt5 . QtWidgets  импортирует  QApplication , QWidget , QVBoxLayout , QPushButton , QLabel , QLineEdit , QTextBrowser
из  PyQt5 . Импорт QtGui  QIcon 

класс  EmailManager :
    защита  __init__ ( сам ):
        сам . список_адресов электронной почты  = []

    async  defgenerate_emails  ( self , count ) :
        асинхронно  с  aiohttp . ClientSession () как  сеанс :
            задачи  = []
            для  _  в  диапазоне ( количество ):
                задача  =  я . генерировать_single_email ( сеанс )
                задачи . добавить ( задача )
            сам . email_list  =  ожидайте  асинхронно . собрать ( * задачи )

    async  def  ignore_single_email ( self , session ):
        api_url  =  "https://www.1secmail.com/api/v1/?action=genRandomMailbox"
        асинхронно  с  сеансом . получите ( api_url ) в качестве  ответа :
            если  ответ . статус  ==  200 :
                данные  =  ожидайте  ответа . json ()
                вернуть  данные [ 0 ]
            еще :
                print ( f"Ошибка: { ответ . статус } - { ожидание  ответа . текст () } " )
                возврат  Нет

    асинхронная  защита  check_mail ( self , email ):
        логин , домен  =  электронная почта . разделить ( «@» )
        api_url  =  f"https://www.1secmail.com/api/v1/?action=getMessages&login= { логин } &domain= { домен } "
        асинхронно  с  aiohttp . ClientSession () как  сеанс :
            асинхронно  с  сеансом . получите ( api_url ) в качестве  ответа :
                если  ответ . статус  ==  200 :
                    сообщения  =  ожидайте  ответа . json ()
                    ответные  сообщения
                еще :
                    print ( f"Ошибка: { ответ . статус } - { ожидание  ответа . текст () } " )
                    возврат  Нет

    асинхронная  защита  delete_mail ( self , email ):
        логин , домен  =  электронная почта . разделить ( «@» )
        api_url  =  f"https://www.1secmail.com/api/v1/?action=deleteMailbox&login= { логин } &domain= { домен } "
        асинхронно  с  aiohttp . ClientSession () как  сеанс :
            асинхронно  с  сеансом . получите ( api_url ) в качестве  ответа :
                если  ответ . статус  ==  200 :
                    вернуть  истину
                еще :
                    print ( f"Ошибка: { ответ . статус } - { ожидание  ответа . текст () } " )
                    вернуть  ложь

    асинхронный  запуск def  ( self , num_emails ):
        жду  себя . генерировать_электронные письма ( число_электронных писем )
        если  сам . адрес электронной почты_список :
            для  электронной почты  самостоятельно  . адрес электронной почты_список :
                распечатать ( электронная почта )

класс  TempMailApp ( QWidget ):
    защита  __init__ ( сам ):
        супер (). __инит__ ()

        сам . email_manager  =  Менеджер электронной почты ()

        сам . инициализирующий интерфейс ()

    определение  initUI ( self ):
        макет  =  QVBoxLayout ()

        сам . setWindowIcon ( QIcon ( 'qiyanka.ico' ))
        сам . setFixedSize ( 400 , 300 )

        self.label = QLabel('Адрес временной почты:')
        сам . email_input  =  QLineEdit ( самостоятельно )
        макет . addWidget ( собственная метка ) _
        макет . addWidget ( self . email_input )

        self.get_email_button = QPushButton('Получить временную почту', self)
        сам . get_email_button . щелкнул . подключиться ( self . get_temp_email )
        макет . addWidget ( self . get_email_button )

        self.check_mail_button = QPushButton('Проверить почту', self)
        сам . check_mail_button . щелкнул . подключиться ( self.check_mail ) _ _
        макет . addWidget ( self . check_mail_button )

        сам . result_browser  =  QTextBrowser ( самостоятельно )
        макет . addWidget ( self . result_browser )

        сам . author_button  =  QPushButton ( 'АВТОР' , self )
        сам . автор_кнопка . щелкнул . подключиться ( self . open_author_page )
        макет . addWidget ( self.author_button ) _ _

        self.delete_mail_button = QPushButton('Удалить почту', self)
        сам . delete_mail_button . щелкнул . подключиться ( self . delete_mail )
        макет . addWidget ( self . delete_mail_button )

        сам . setLayout ( макет )

    защита  get_temp_email ( сам ):
        асинцио . запустить ( self . email_manager.generate_emails ( 1 ) ) _
        если  сам . email_manager . адрес электронной почты_список :
            сам . электронная почта_вход . setText ( self . email_manager . email_list [ 0 ])
            сам . результат_браузер . ясно ()
            self.result_browser.append(f"Сгенерированная почта: {self.email_manager.email_list[0]}")

    защита  check_mail ( сам ):
        электронная почта  =  я . электронная почта_вход . текст (). полоска ()
        если  электронная почта :
            сообщения  =  асинхронно . запустить ( self . email_manager . check_mail ( электронная почта ))
            если  сообщения :
                сам . результат_браузер . ясно ()
                self.result_browser.append(f"Сообщения для {email}:")
                для  сообщения  в  сообщениях :
                    сам . результат_браузер . добавить ( ул ( сообщение ))
            еще :
                сам . результат_браузер . ясно ()
                self.result_browser.append(f"Сообщений нет для {email}")
        еще :
            сам . результат_браузер . ясно ()
            self.result_browser.append("Введите адрес электронной почты для проверки.")

    защита  open_author_page ( я ):
        импортировать  веб-браузер
        веб-браузер . открыть ( 'https://zelenka.guru/sataraitsme/' )

    защита  delete_mail ( сам ):
        электронная почта  =  я . электронная почта_вход . текст (). полоска ()
        если  электронная почта :
            успех  =  асинхронно . запустить ( self . email_manager . delete_mail ( электронная почта ))
            если  успех :
                сам . результат_браузер . ясно ()
                self.result_browser.append(f"Почта {email} успешно удалена.")
            еще :
                сам . результат_браузер . ясно ()
                self.result_browser.append(f"Не удалось удалить почту {email}. Попробуйте позже.")
        еще :
            сам . результат_браузер . ясно ()
            self.result_browser.append("Введите адрес электронной почты для удаления.")

если  __name__  ==  '__main__' :
    приложение  =  QApplication ( sys . argv )
    пример  =  TempMailApp ()
    бывший . setWindowTitle ( 'QIYANAS TEMPMAIL' )
    ex.show()
    сис . выход ( app . exec_ ())
    @ эхо  выключено
echo Преобразование Python скрипта в исполняемый файл...
pyinstaller --onefile --windowed --icon=qiyanka.ico .\qiyana.py
echo Готово! Исполняемый файл создан. Погладь Киану.
Пауза
@ эхо  выключено
echo Установка зависимостей...
pip install -r требования.txt
echo Готово! Все зависимости установлены. Погладь Киану.
Пауза
айоhttp==3.8.1
PyQt5==5.15.4
@ эхо  выключено
echo Преобразование Python скрипта в исполняемый файл...
pyinstaller --onefile --windowed --icon=qiyanka.ico .\qiyana.py
echo Готово! Исполняемый файл создан. Погладь Киану.
Пауза
@ эхо  выключено
echo Установка зависимостей...
pip install -r требования.txt
echo Готово! Все зависимости установлены. Погладь Киану.
Пауза
айоhttp==3.8.1
PyQt5==5.15.4
