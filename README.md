# Домашнее задание к занятию "13.Системы мониторинга
1. Минимальный набор метрик для мониторинга

Для платформы вычислений, которая загружает ЦПУ и работает по протоколу HTTP, минимальный набор метрик для мониторинга может включать:

    Загрузка ЦПУ (CPU Load): Чтобы отслеживать, насколько сильно нагружен процессор, и чтобы предотвратить перегрузку системы.
    Занятая/доступная память (Memory Usage): Чтобы понимать использование RAM и избегать ситуации, когда система начинает использовать своп, что может замедлить производительность.
    Время отклика запросов (Response Time): Чтобы контролировать задержки при взаимодействии с платформой и обеспечивать качество обслуживания.
    Количество запросов в секунду (Requests per Second): Чтобы отслеживать нагруженность системы и предсказывать рост нагрузки.
    Статистика кодов ответов (Response Codes): Для мониторинга успешных и неуспешных запросов (например, 2xx, чтобы следить за общим успехом взаимодействия).

2. Объяснение метрик и улучшение информативности

Для лучшего понимания метрик, менеджеру продукта можно объяснить их следующим образом:

    RAM: Оперативная память, используемая приложением. Важно знать, сколько памяти занятой и свободной, так как это влияет на производительность.
    inodes: Это структура данных, которая хранит информацию о файлах и директориях на файловой системе. Нужно следить за количеством используемых inodes, чтобы избежать ситуации, когда не будет места для создания новых файлов.
    CPU Load: Средняя загрузка процессора, показывающая, насколько загружен сервер. Важно следить за этой метрикой, чтобы предотвратить перегрузку и задержки в обработке запросов.

Для оценки выполнения обязательств перед клиентами и качества обслуживания можно предложить следующие метрики:

    Uptime: Доля времени, когда платформа доступна для клиентов.
    Response Time Percentiles (например, 95-й и 99-й процентиль): Чтобы понять, насколько быстро обрабатываются запросы, включая "испорченные" по времени отклика.
    Время до первого байта (Time To First Byte, TTFB): Чтобы понять, сколько времени проходит с момента запроса до первого байта ответа.
    Время обработки запросов: Чтобы показать клиента, сколько времени уходит на выполнение задач на сервере.

3. Работа с ошибками без выделенного финансирования

Если финансирование на систему сбора логов не выделено, можно предложить следующее решение:

    Использовать встроенные возможности логирования приложений: Разработчики могут настроить вывод критически важной информации об ошибках в стандартный вывод (stdout) или стандартный поток ошибок (stderr). На уровне контейнера или платформы можно настроить сбор этих логов, отправляя их, например, в систему мониторинга (если такая имеется) или в файл, который будет доступен для анализа.

4. Ошибка в расчетах SLA

Если SLA вычисляется как summ_2xx_requests/summ_all_requests и значение не поднимается выше 70% без кодов 4xx и 5xx, это может указывать на то, что:

    Существует значительное количество запросов, которые возвращают коды не 2xx. Это могут быть коды 3xx (перенаправления), которые могут не учитываться или неправильно обрабатываться вашей метрикой.
    Необходима детальная проверка всех кодов ответов, чтобы выяснить, что происходит с запросами. Возможно, есть протоколирование или освещение других кодов, которые теперь нужно включить в анализ.

5. Плюсы и минусы pull и push систем мониторинга

Push система:

    Плюсы:
        Более гибкая и позволяет отправлять данные по мере их возникновения.
        Может уменьшить нагрузку на серверы мониторинга.
    Минусы:
        Проблемы с сетью могут привести к потере данных.
        Сложно в управлении и настройке, т.к. каждая установка должна сама отправлять данные.

Pull система:

    Плюсы:
        Легче управлять и настраивать.
        Возможность опроса и получения данных непосредственно в момент запроса.
    Минусы:
        Может создавать нагрузки на мониторинг при частых опросах.
        Задержка данных – все данные будут доступны только по запросу.

6. Классификация систем мониторинга

Push модель:

    TICK (включает Telegraf, который используется для отправки данных)
    VictoriaMetrics (может работать как push-решение)

Pull модель:

    Prometheus
    Zabbix (может действовать как pull система, хотя есть возможность push через Zabbix Sender)
    Nagios (в основном pull, но тоже может использоваться push подход)

Гибридные модели:

    Zabbix (имеет возможности как pull, так и push)
    TICK (может также подключаться к инструментам, которые работают в pull режиме)

Эта классификация может варьироваться в зависимости от конкретной конфигурации систем и использования.

