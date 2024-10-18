Данный репозиторий является форком репозитория https://gitlab.iaaras.ru/iaaras/gostdown.git

12332

# Назначение

Это набор шаблонов и скриптов для автоматической вёрстки электронной
документации по ГОСТ 19.xxx (ЕСПД) и ГОСТ 7.32 (отчёт о научно-исследовательской
работе) в форматах docx. Исходными данными при создании docx являются текстовые
файлы формата `.md` (Markdown) и `.bib` (BibTeX). Также из docx производятся
PDF-файлы.

Основное назначение этого сооружения — снабдить пользователей
средством совместной работы над большими документами, при этом соблюдая
частые требования государственных заказчиков: формат docx и отечественные ГОСТы. 

[Пример сгенерированного документа (PDF)](https://gitlab.iaaras.ru/iaaras/gostdown/-/jobs/artifacts/master/file/demo-report.pdf?job=build_docx).

Использование Markdown и BibTeX открывает возможности к использованию
целого ряда удобств *одновременно*:

1. Автоматическая нумерация разделов, таблиц, формул, рисунков;
   ссылки на всё это из текста.
2. Написание формул в привычном многим формате TeX/LaTeX.
3. Формирование библиографии в привычном многим формате BibTeX.
3. Автоматическое оформление списка литературы и нумерация ссылок в тексте.
4. Автоматическая вставка в заданное место количества страниц, таблиц, рисунков,
   использованных источников, приложений.
5. Простые и понятные процедуры контроля внесённых изменений, слияния изменений,
   сделанных разными пользователями, и разрешения конфликтов изменений.

Для следующих частей документа процедура слияния не
работает по причине бинарности соответствующих файлов:

- Лист утверждения (при наличии), титульный лист и лист регистрации изменений.
  При большом желании можно преодолеть это ограничение, воспользовавшись тем
  фактом, что docx-файл — это запакованный текст в формате XML, который можно
  распаковать и обращаться с ним как с текстом; но в данном репозитории это
  не сделано.
- Иллюстрации (PNG, JPEG, EMF)

# Перечень файлов

## Исходные файлы

- `demo-espd-beginning.md`, `demo-main.md`, `demo-espd-end.md` — файлы,
  составляющие текстовую часть документа, оформленного по ГОСТ ЕСПД.  При
  преобразовании в `.docx` три файла последовательно склеиваются в один.
  Разбиение на три части необязательно и сделано для удобства.
  
- `demo-report-beginning.md`, `demo-report-end.md` — замена для первого
  и третьего файлов из тройки для преобразования в `.docx` документа,
  оформленного по ГОСТ 7.32.

- `demo-template-espd.docx` — файл-шаблон для документа, оформленного по
  ГОСТ 19, содержащий:
    - лист утверждения
    - титульный лист
    - последнюю страницу (лист регистрации изменений)
    - стили, используемые для форматирования текста
    
- `demo-template-report.docx` — аналогичный файл-шаблон для документа,
  оформленного по ГОСТ 7.32. В нём нет листа утверждения и листа регистрации
  изменений, и титульный лист выглядит иначе, чем в шаблоне для ГОСТ 19.
  
- `demo.bib` — библиографические ссылки в формате BibTeX.

- `gost-r-7-0-5-2008-numeric-iaa.csl` — стилевой файл библиографического списка.

- `drracket-screenshot.png`, `lunokhod-expo.jpg`, `iaa-logo.emf`, `oc-plot.emf` —
  иллюстрации для документа.

## Скрипты

- `build.ps1` — PowerShell-скрипт, который делает следующее:
    - Вызывает Pandoc для конвертации трёх (или любого другого количества)
      `.md`-файлов в формат docx с применением стилей из шаблона в формате docx.
    - С помощью Word и COM делает постобработку полученного docx: вставляет
      первые и последние страницы из шаблона, поправляет стили списков, формул,
      таблиц
    - Сохраняет результат в форматах docx и PDF.
- `build-demo-report.bat` — файл для быстрого запуска `build.ps1` со всеми
  необходимыми параметрами для сборки отчёта по ГОСТ 7.32.
- `build-demo-espd.bat` — аналогичный файл для сборки документа по ГОСТ 19.
- `pandoc-demo-report.bat` — файл для преобразования отчёта по ГОСТ 7.32
  в формат docx без постобработки (используется при необходимости отладки
  ошибок).
- `pandoc-demo-espd.bat` — аналогичный файл для преобразования документа по
  ГОСТ 19 без постобработки.
- `.gitlab-ci.yml` — настройки удалённого запуска сборки по коммиту в GitLab
  (см. ниже)

## Результаты сборки (отсутствуют в репозитории)

- `demo-espd.docx`, `demo-espd.pdf` — результаты сборки документа по ГОСТ ЕСПД.
- `demo-report.docx`, `demo-report.pdf` — результаты сборки документа по ГОСТ 19.
- `demo-espd-raw.docx`, `demo-report-raw.docx` — результаты преобразования
   в docx без постобрабоки (используются для отладки ошибок)

Результаты сборки (в терминологии GitLab — артефакты) доступны по ссылкам
из раздела «[Pipelines](https://gitlab.iaaras.ru/iaaras/gostdown/pipelines)».
Есть и более прямые ссылки:

- [Перечень файлов наиболее свежей сборки](https://gitlab.iaaras.ru/iaaras/gostdown/-/jobs/artifacts/master/browse?job=build_docx).
- [Отдельный файл из наиболее свежей сборки](https://gitlab.iaaras.ru/iaaras/gostdown/-/jobs/artifacts/master/file/demo-report.pdf?job=build_docx) (с предпросмотром).
- [Отдельный файл из наиболее свежей сборки](https://gitlab.iaaras.ru/iaaras/gostdown/-/jobs/artifacts/master/raw/demo-report.pdf?job=build_docx) (прямое скачивание).


# Инсталляция

## Системные требования

1. Microsoft Word версии 2010 или выше, с сопутствующими ему COM-объектами
в операционной системе. Проверялась русская локализация Word,
но и английская тоже должна работать.
2. [Pandoc](http://pandoc.org/) версии 2.1.1 или выше.
[Дистрибутив Pandoc](https://github.com/jgm/pandoc/releases) включает также
фильтр pandoc-citeproc, необходимый в нашем случае.
**Внимание:** для работы скриптов требуется, чтобы директория,
в которой находится исполняемый файл `pandoc.exe` (как правило,
это директория `C:\Program Files (x86)\Pandoc`) была указана
в переменной окружения `PATH`. 
3. pandoc-crossref — ещё один необходимый фильтр. Дистрибутив можно
скачать с [этой страницы](https://github.com/lierdakil/pandoc-crossref/releases).
**Внимание:** исполняемый файл `pandoc-crossref.exe` требуется поместить
в одну из директорий, указанных в `PATH`, например в вышеупомянутую
`C:\Program Files (x86)\Pandoc`.
4. Microsoft PowerShell. Входит в Windows начиная с 7-й версии.
5. Шрифты компании «Паратайп»:
   [PT Serif, PT Sans и PT Mono](https://www.paratype.ru/public/).
   Они бесплатны для скачивания, использования и распространения.
   Сгенерированные `.docx` файлы содержат эти шрифты внутри себя,
   что позволяет открывать эти файлы на системах, где эти шрифты
   не установлены. При отсутствии необходимости внедрения шрифтов
   отключите опцию `-embedfonts` в команде запуска `build.ps1`.
   (Внедрение шрифтов «утяжеляет» `.docx` файлы на несколько МБ.)
6. Система контроля версий [Git](https://git-scm.com/). Строго говоря, не
   является обязательной, но её отсутствие сведёт на нет существенную часть
   преимуществ данного подхода по сравнению с простым редактированием документа
   в Word. Допустимы (но не рассмотрены здесь) и другие системы контроля версий.

Необходима ОС Windows, преимущественно по причине использования COM. (Остальные
вещи в той или иной степени существуют и на Linux и Mac OS X.) Тем не менее,
Windows необходима лишь для автоматической вёрстки (в т.ч. удалённой, см. ниже),
а *писать* документы можно в текстовом редакторе в любой операционной системе,
обмениваясь файлами с другими пользователями через Git.

## Системные настройки

1. Для выполнения PowerShell скриптов из файла требуется запустить PowerShell
   с правами администратора и выполнить следующую команду:
   
    ```
    set-executionpolicy remotesigned 
    ```
    
2. Для увеличения лимита памяти при запуске PowerShell в удалённом режиме
   требуется установить следующий ключ в регистре:
   `Computer\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\WSMAN\ClientMaxMemoryPerShellMB`
   в значение 512 или больше.
   
    Этот ключ требуется лишь при удалённом запуске, и лишь при версии Powershell
    меньше 3.0.

# Использование

1. Склонируйте данный репозиторий на свою машину с Windows.

2. Запустите `build-demo-report.bat` и `build-demo-espd.bat`.

3. При успешном выполнении этих скриптов приступайте к модификации шаблона
   (впишите реальное название организации, работы, руководителей),
   затем редактируйте `.md`-файлы. Количество и названия файлов можно
   произвольно менять, соответствующим образом правя параметры запуска
   в `build-demo-espd.bat`/`build-demo-report.bat`.
   Эти команды работают не очень быстро (минуты) и с увеличением размера
   документа будут работать всё медленнее.
   
    Файлы, не имеющие отношения к вашему документу, можно вовсе удалить (например,
    иллюстрации). Из двух видов документов (ЕСПД или отчёт) вам, вероятно, нужен
    лишь один; второй можно удалить.
    
    Файл `.gitlab-ci.yml` можно удалить, если вам не требуется удалённый запуск
    (см. раздел «Удалённый запуск через GitLab CI»).
    
4. Полученные `.docx` файлы не требуют, по замыслу, никакой дальнейшей ручной
   или автоматической обработки. Их можно отправлять заказчику; при желании
   их можно редактировать так же, как обычные документы, созданные вручную,
   но это не рекомендуется.

## При необходимости сравнения `.docx`

Если кто-то всё же внёс изменения непосредственно в `.docx` файл, и вы желаете
просмотреть эти изменения и перенести их в `.md` файл, выполните следующие
команды:
 
```
pandoc original.docx -o original.md
pandoc modified.docx -o modified.md
<программа для сравнения текстовых файлов> original.md modified.md
```

Таким образом, изменения в `.docx` (по крайней мере, в текстовой части) не
пройдут незамеченными.

Программ для сравнения текстовых файлов доступно множество; можно отметить
[Meld](http://meldmerge.org/) для Linux или [WinMerge](http://winmerge.org/) для
Windows.

## Работа над более чем одним документом

Работать над двумя и более документами можно как в одном репозитории, так и в
разных. Если два и более документа относятся к одному проекту и используют
единый `.bib` файл, имеет смысл держать их в одном репозитории, что, однако,
немного усложнит скрипты сборки.

## Удалённый запуск через GitLab CI

Следующий сценарий позволяет «простым пользователям» работать над документом, не
пользуясь ни Windows, ни Pandoc, вообще ничем, кроме Git и текстового редактора
(и средства создания иллюстраций, если нужны иллюстрации).

Некоторые начальные действия (однократно) всё же придётся сделать вручную
на машине с Windows.

1. Инсталлируйте необходимые программы и задайте системные настройки (см. выше)
2. Отредактируйте первые страницы шаблона (см. выше)
3. Инсталлируйте [Gitlab runner](https://docs.gitlab.com/runner/) и настройте его
   связи с вашим репозиторием (инструкции, как это сделать, см. по ссылке)
4. После инсталляции раннера запустите `services.msc` с правами администратора
   и поставьте в свойствах сервиса `gitlab-runner` галочку «Allow service to
   interact with desktop» (в некоторых случаях это облегчит диагностику ошибок)
   
   
Следующие действия можно выполнять с любой клиентской машины.

1. В случае, если вы удаляли или переименовывали исходные файлы или скрипты,
   отредактируйте файл `.gitlab-ci.yml`, приведя его в соответствие.

2. С этого момента каждая отдача файлов в репозиторий будет приводить к запуску
   сборки документа на машине раннером на Windows. Результаты сборки можно
   скачать из веб-интерфейса GitLab со страницы «Pipelines» (все разом в архиве или
   пофайлово). Там же можно ознакомиться с сообщениями об ошибках, если сборка
   оказалась неудачной.
   
    Наличие PDF-файла наряду с docx позволит вам взглянуть на документ
    при отсутствии Word.

GitLab CI не является исключительным для осуществления этого процесса.
Аналогичное средство есть, например, в GitHub (Travis CI).

# Авторы и права на использование

Авторы: Дмитрий Павлов (ИПА РАН) и Алёна Водолагина (ИПА РАН),
при участии Даниила Аксима (ИПА РАН).

Файл `gost-r-7-0-5-2008-numeric-iaa.csl` распространяется на условиях
«Creative Commons Attribution-ShareAlike 3.0 License».
                                                    
Прочие файлы, содержащиеся в данном репозитории, являются общественным достоянием
и могут использоваться, модифицироваться и распространяться без ограничений.

# Связь

Замечания и предложения можно присылать на адрес <dpavlov@iaaras.ru>.

