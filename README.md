# Описание утилиты RX Instance Manager
![](https://github.com/DirectumCompany/rx-instance-manager/blob/main/rx-instance-manager.png)

Утилита является GUI-обверткой вокруг компоненты Manage Applied Projects. В большинстве случаев выполнение действий в утилите приводит к выполнению тех или иных команд компоненты Manage Applied Projects. Рекомендуется изучить соответствующие команды.

С помощью утилиты можно запускать и останавливать сервисы Directum RX, переключать экземпляры RX между проектами, быстро смотреть текущие настройки экземпляра RX, быстро открывать логи и некоторые другие типовые операции, с которыми сталкивается прикладной разработчик.

Для управления экземпляром RX его нужно зарегистрировать в утилите кнопкой "Добавить". Список зарегистрированных экземпляров хранится в файле rxman.yaml.

Также есть возможность настраивать UI и поведение утилиты задавая параметры в файле rxman.config. Если этого файла нет, то создается автоматически при запуске утилиты. Подробнее см. раздел "Настройки".

### Интерфейс и возможности утилиты

Кнопки:

1. "Info" - просмотр настроек текущего проекта. Для этого выполняется команда `do map current`.
2. "Добавить" - добавление нового экземпляра Directum RX в утилиту. Потребуется выбрать каталог, в котором установлен экземпляр Directum RX.
3. "do all up"/"do all down" - кнопки запуска и остановки сервисов экземпляра Directum RX. Какая кнопка будет доступна зависит от текущего состояния сервисов.
4. "Development Studio" - запуск Directum Development Studio.
5. "Веб-клиент" - открыть в браузере веб-клиент текущего экземпляра Directum RX.
6. "Directum LogViewer" - открыть Directum LogViewer, предварительно "настроив" его на работу с логами текущего экземпляра Directum RX. Настройка производится корректировкой соответствующей записи в реестре. Кнопка видна есть заполнен параметр `logViewer` в настройках и файл, который указан в этом параметре, существует.
7. "Папка исходников" - открыть папку исходников текущего проекта. Если репозитории в confgi.yml настроены так, что есть только один репозиторий для слоя Work, то откроется папка этого репозитория. Если таких репозиториев несколько - то откроется корневая папки исходников.

Контекстное меню:

1. "Сменить проект" - изменение проекта. Попросит выбрать файл описания проекта и переключит на него текущий экземпляр Directum RX выполнив `do map set`. От значения настройки needCheckAfterSet зависит будет запускаться или проверка отклика сервисов после их перезапуска. Если значение `false` - то проверка отклика выполняться не будет, в результате команда выполнится заметно быстрее и можно будет быстрее запустить DevelopmentStudio. Но в этом случае возможна ситуация, когда сервисы RX не успели до конца загрузиться или при их загрузке произошла какая-то ошибка (например, в случае отсутствия БД).  Значение `true` приводит к более длительному переключению проектов, но при этом разработчик видит  прошло переключение корректно или нет.
2. "Создать проект" - создание нового проекта. Попросит выбрать файл описания проекта и создаст на основе него новый проект командой  `do map create_project`.  Окно командной строки при выполнении этого пункта меню не закрывается автоматически, чтобы у разработчика была возможность увидеть возможные ошибки при создании проекта. Типовые ошибки - сообщает о существовании либо БД, либо домашнего каталога, указанных в файле описания проекта.
3. "Создать копию текущего проекта" - создание копии нового проекта. Попросит выбрать файл описания проекта и создаст на основе него копию текущего проекта командой  `do map clones_project`. 
4. "Пропатчить config.yml" - внести изменения в config.yml на основе какого-то файла описания проекта. Попросит выбрать файл описания проекта и и выполнит команду  `do map update update_config`. 
5. "Проверить сервисы" - проверяет состояние сервисов RX командой `do all check`. Окно командной строки при выполнении этого пункта меню не закрывается автоматически, чтобы у разработчика была возможность увидеть результаты проверки сервисов. Может быть полезна, например, если есть сомнения в работе сервисов после переключения проекта, если используется "быстрая" версия переключения.
6. "Открыть DDS без deploy" - открытие DDS без возможности публикации с выбором конфига проекта - "Открыть DDS без deploy" в контекстном меню.  Попросит выбрать файл описания проекта  и выполнит команду `do map dds_wo_deploy`. Может быть полезна для быстрого просмотра исходников другого проекта в этом же экземпляре Directum RX.
7. "Сводка по инстансу" - показывает основную информацию по экземпляру Directum RX и текущему проекту.
8. "Запустить cmd (от администратора)" - открывает консоль командной строки в папке текущего экземпляра RX. Может быть полезна для выполнения различных do-команд.
9. "Очистить логи текущего экземпляра RX" - удаляет старые логи текущего экземпляра Directum RX командой `do map clear_log`. В результате будут удалены логи старее, чем три дня.
10. "Очистить логи всех экземпляров RX" - удаляет старые логи для всех зарегистрированных экземпляров Directum RX.
11. "Открыть config.yml" - открывает config.yml текущего экземпляра Directum RX  в приложении, ассоциированном с yml-файлами.
12. "Открыть конфиг проекта" - открывает файл описания текущего проекта в приложении, ассоциированном с yml-файлами.
13. "Конвертировать БД проектов" - выполняет команду `do db convert` для текущего проекта, "обновляя" БД до текущей версии платформы Sungero.
14. "Удалить БД и домашний каталог выбранного проекта" - удаляет БД и домашний каталог п
15. "Убрать экземпляр RX из списка" - убирает текущий экземпляр Directum RX из списка зарегистрированных в утилите. Сам экземпляр при этом не удаляется.
16. "Открыть папку экземпляра RX" - открывает папку, в которой развернут текущий экземпляр Directum RX.
17. "Открыть папку логов" - открывает папку логов текущего экземпляра Directum RX.

### Настройки

Описание параметров в файле настроек rxman.config:

* logViewer - путь к Directum LogViewer.
* needCheckAfterSet - требуется или нет выполнять проверку отклика сервисов после переключения между проектами. Допустимые значения `true` и `false`.
* contextMenu - настройки видимости пунктов контекстного меню:
    * changeProject - Сменить проект
    * createProject - Создать проект
    * cloneProject - Создать копию текущего проекта
    * updateConfig - Пропатчить config.yml
    * checkServices - Проверить сервисы
    * runDDSWithOutDeploy - Открыть DDS без deploy
    * infoContext - Сводка по инстансу
    * cmdAdminContext - Запустить cmd (от администратора)
    * clearLogContext - Очистить логи
    * clearLogAllInstancesContext - Очистить логи всех инстансов
    * configContext - Открыть config.yml
    * projectConfigContext - Открыть конфиг проекта
    * convertDBsContext - Конвертировать БД проектов
    * removeInstance - Убрать инстанс из списка
    * openRXFolder - Открыть папку инстанса
    * openLogFolder - Открыть папку инстанса
