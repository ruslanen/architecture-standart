### <a name="_b7urdng99y53"></a>**Название задачи:** MVP открытия депозитов в интернет-банке "Стандарт"
### <a name="_hjk0fkfyohdk"></a>**Автор:** Руслан
### <a name="_uanumrh8zrui"></a>**Дата:** 19.07.2025
### <a name="_3bfxc9a45514"></a>**Функциональные требования**

|**№**|**Действующие лица или системы**|**Use Case**|**Описание**|
| :-: | :- | :- | :- |
|UC1|Клиент, Интернет-банк, Сотрудник бэк-офиса, АБС|Открытие депозита MVP|1. Клиент открывает интернет-банк и переходит в личный кабинет<br>2. Клиент переходит в раздел депозитов<br>3. Интернет-банк отображает персональные предложения по депозитам<br>4. Клиент выбирает предложение, оформляет заявку и отправляет запрос<br>5. Сотрудник бэк-офиса в режиме реального времени получает заявку, проверяет данные и согласовывает  ее<br>6. АБС отправляет СМС-уведомление клиенту о статусе заявки<br>7. Клиент зачисляет денежные средства на депозит|

Будем считать, что регистрация и вход с СМС-подтверждением уже реализованы в интернет-банке. Поэтому детализацией данных шагов на схемах можем пренебречь.

### <a name="_u8xz25hbrgql"></a>**Нефункциональные требования**

|**№**|**Требование**|
| :-: | :- |
| R   | Надёжность (Reliability)           |
| R1    | Все сервисы должны работать 24/7 и быть доступны в 99,9% случаев |              
| R1    | В случае сбоев в ЦОД необходимо, чтобы сервисы интернет-банка были доступны и выдерживали требуемую нагрузку |              
| P   | Производительность (Performance)   |
| P1    | Отклик по всем операциям должен быть максимально быстрым и занимать миллисекунды | 
| P2    | Нужно предусмотреть равномерное горизонтальное масштабирование и распределение запросов между серверами, приложениями и ЦОД |
| S   | Поддерживаемость (Supportability)  |              
| S1    | Нужно предусмотреть разработку документации для дальнейшего расширения системы                                |              
| +R  | + Ограничения (Restrictions)       |   
| +R1    | Поддержка шифрования данных при работе с персональными данными |    
| +R2    | Функционал работы с СМС следует доработать силой команды разработки банка в ядре системы, вместо доработок со стороны подрядчика |              
| +R3    | АБС может масштабироваться только вертикально из-за своей базы данных |              
| +R4    | Необходимо избежать прямой работы интернет-банка с API АБС в новом процессе |
| +R5    | Необходимо использовать технологии, которые уже есть в банке (MS SQL, Oracle, PostgreSQL, ASP.NET MVC 4.5, .NET Framework 4.5, Delphi, Java Spring Boot, PHP, React.js.) | 
| +R6    | Если нужны очереди сообщений, то лучше использовать Kafka на перспективу. Стоит учитывать, что текущая версия платформы интернет-банка несовместима с ней. Возможно, стоит подумать о переводе интернет-банка на микросервисную архитектуру, но пока только в рамках задачи открытия депозитов |     

### <a name="_qmphm5d6rvi3"></a>**Решение**

[![](https://img.plantuml.biz/plantuml/svg/ZLF1Qjj04BthAxO-EL17bvvw2eJScYOaz1naQTE8hhHYlQBDpN5QA8GIwB6KW7ljHN4jgiYH0_c2sJ_gsxKTMZLfWs4zCyo-D-_DQcEW0wNpH7Wwre8p-a9pQio8IghD2VuRAimveobLLD0FCyHvZL_1w19XGpN2s-yqHgjDfsWVcj7jpFEqp9YMh2-rbWcBOiL37SlKyvx4QIZoYETAi2Ejiy5ptrV1s_NNiJxA-_c0gr2ccgTwFPI9lnU7WOhaovXdNFYRptgx8aZrg-qNVX8CD5Se7MpA99EflP5PATukLmEPTaCHe_QKTQ1g1W6bLTPfvGo14mnj3SvHxp_AEcih_7uGljavF8n3lfLaGXvpRh77D3SZj9xYIWFm6bQPJrJtK7zFbJcXmirmWRAr4speFwC9ujBEyzDQZhIcYk6ucnIRcO1y_nYoa-w-3j88pYpI1N7bLbO-PZ-TVzm03wO4lVd045Xd9cxmnG3B2Gwstkckve9ZIpiPLu6Moe9-0zpClX-qNqCqtJY4mWemGyVxsJdzqTbpnTej3EcSMHVfnZbahIWPflrAOyk_nyNTRl-suGBtQeZypT4fnW4cdAtm0DOSm3tBQs4zPaNcbDbFv7wA5bxRgyMeAuap0AwmNN6ErPmjqV_mmd8wBtfuOnFmJgtJpdDdw9AzJRTqx07MXm7DEG0tU1hXq_e5)](https://editor.plantuml.com/uml/ZLF1Qjj04BthAxO-EL17bvvw2eJScYOaz1naQTE8hhHYlQBDpN5QA8GIwB6KW7ljHN4jgiYH0_c2sJ_gsxKTMZLfWs4zCyo-D-_DQcEW0wNpH7Wwre8p-a9pQio8IghD2VuRAimveobLLD0FCyHvZL_1w19XGpN2s-yqHgjDfsWVcj7jpFEqp9YMh2-rbWcBOiL37SlKyvx4QIZoYETAi2Ejiy5ptrV1s_NNiJxA-_c0gr2ccgTwFPI9lnU7WOhaovXdNFYRptgx8aZrg-qNVX8CD5Se7MpA99EflP5PATukLmEPTaCHe_QKTQ1g1W6bLTPfvGo14mnj3SvHxp_AEcih_7uGljavF8n3lfLaGXvpRh77D3SZj9xYIWFm6bQPJrJtK7zFbJcXmirmWRAr4speFwC9ujBEyzDQZhIcYk6ucnIRcO1y_nYoa-w-3j88pYpI1N7bLbO-PZ-TVzm03wO4lVd045Xd9cxmnG3B2Gwstkckve9ZIpiPLu6Moe9-0zpClX-qNqCqtJY4mWemGyVxsJdzqTbpnTej3EcSMHVfnZbahIWPflrAOyk_nyNTRl-suGBtQeZypT4fnW4cdAtm0DOSm3tBQs4zPaNcbDbFv7wA5bxRgyMeAuap0AwmNN6ErPmjqV_mmd8wBtfuOnFmJgtJpdDdw9AzJRTqx07MXm7DEG0tU1hXq_e5)

[![](https://img.plantuml.biz/plantuml/svg/nLXBRnjL5DxxLrncGoh5yGQnODN4YQ2bQKmSO5cDnxV9aF7CQ6RS4g5AJTBqeHG25Gk4eAW8n8ADwyRrr4uSgR_WpZ_Yd3CFpzEJAerGrTRpEkVxllEwzrwr7TbokXsjKd5KH-DC_2nSqHEllFlmxmdywJ4n4XDnwks9WNWZJiK07ar4ducNyCOfFEonj_-A5_0SBuz5pxllxRqTixcDjUhIoXKmR9ZCCLZBS1oZmzggnHL7DVHAvGDLLxHkctSjZMqvZcb_KgjPyjs5JTNPwhQwDhSKGtUuxYmeHgUshYgorJPkgjpaMgt-KRMXoRhpvUgDMaUs7Mx1hLjresCQEdvX4gS6jsn3drCq5UxDCqdyAiR0Ru1qfDGrWn_44lWXoxrg2KFfIcY_9IlRQxTlgmfVwPYQiSCvsduX9juUlBeFdvo2yH58vdqlHbMu_q2utgxe4ORdS1lL1c_m_g7em-y1uXd2ZIEu70aNWn0XHVd1UQKEwiYgpgrRIqPNRylMpbpJaV57kxa3adHy5E29WaUnBA4qumjm2f01YtV0u0VAr2SMX8PzLs4i0ZHt3U9cwTnPalNjUKdyKg2i1C_-WBi3W30XfpqmEWRM9436jHWaN7k784FSxM8NKansU1lzglfc88CaVWGBgDi9QWnsXl0vfJ5vE4MXi-xm9lo8gJ4RBsgHZ3ZM4FCUm0smwps4MnDq1byyXV_9WlUKYJCnOV0aJ0Cm0fyEA0746UBqLSuaGIoSAI7gigRLEGOXaY0LLgGvhW8cieQUWLnzVRMQP3pFACPd-3Q07W8Lkezw7IDpHWZTq6pVEu1NZxqZxmbb2d3e4-bHaBLnbCsE_Ib4wwwyG_X-4o-gtcCm-eCOCW8mXh_uuGdK3630m6Fy9W7SMRdJ1rhxH6n04NR5crY5Gm52D_Ds8nAe-Hdf7zHR06SVhia3q1kJAJ24nSVOXpg_cw2pocrRtkHBbh7DBK3t4wNO2FrHRb28dqH6vgL5KrQsEFjSlhqjK-dDgeMPnY8UVKeQ5z1257oEO0chvIYdQYltShj8edgPXD2Y2WyQx78hyTQoxCWjsUR86lmFCHME_Tho3hq3jB3QPCqlRX3Dlqbe08x4ejSmuwaIpa3f7iN_DHKGX0DOF48Ty3f8eW8Wt3HirNclU517B01i6Y62lFZ7SNpDMtL_waZY6Np_iXelNenzix5mSsM3mOjiiN6D8TesCtH6TpykIGVy8fww8qZfG-niVbt6yMRBBj2eoQqxCBAezAAyoFlwVmNRa7Sw86C1s7Yg3FoQUczGpu89CI2CfpxSjCFK_54KgA42i54ENxQc2KUKgkjSIqTzTj7y4wGwjHutc4J-o9skJwXIi4uGAzHIRQFU847GNIfk1StYudvIab-2dy1HVdLdQkaP61Z6h4AWqDprvjhDscSR6urcX3pVO2RVBsemavFdCPW_5mgMLwa94HL3orxczZOi_nTDKuFLEwxwmV_lK42xb9t1CeWQBIqYnd3nEfXf40LfcMlcbbfI2l6nnc3c1uE2nPOC3_qPYTqyFaae5Lx29mVY5QtA_3MgAzpCZ44oPJD5MhDaHUEuC2cPADc5xKaABd0BnaocRpSiGu6L1kgVtnHpUUTCiNdfNAqceMzccUgUtrzQqo8-vKA7ixjKw9n5THfjTkRXnjUT5dv-7YSJuk9zh32uvr3f4hs_FvhvBIMpKIbe9jFtIbTMwrnMd8LlxB8LTSvcW1h4D5y6b1pnNHYbHjvcl11helTHgvMkotTarZGja9aj6ORp3gC-vfWQnmGJ-57GKDt4RjKxp2tMiZrbfagdRChhic7OpgR5prURALtPT9FfRyoZ7U2RQItcfK6ntEymEmkDvDA_O1tugNvhvLjdpk4QLupEciajsJ4ip8G_PsvyALcUqJdAut3lbtebx7s9BUqWsDAYbaWuF1DBxVN4yt0n6ISIwuxeFvGExUISVHMUmfIVT7dx30z8qZvzLx7ZWhHiFkwGRLJYiHJ3SY3B2LdGtyoTVZfR44VQkHAsermCnUQz8wYIvcep34aY58cpaR_eoEC43wtm0ERTg677ILF8dscNvpFBBtrOagTTvfm3ahYFBXzHkeEStjFGq-9Nwsi1kS9XblOPB-EqB-oshXWclC6g8EcjSfNhxMv7-my0)](https://editor.plantuml.com/uml/nLXBRnjL5DxxLrncGoh5yGQnODN4YQ2bQKmSO5cDnxV9aF7CQ6RS4g5AJTBqeHG25Gk4eAW8n8ADwyRrr4uSgR_WpZ_Yd3CFpzEJAerGrTRpEkVxllEwzrwr7TbokXsjKd5KH-DC_2nSqHEllFlmxmdywJ4n4XDnwks9WNWZJiK07ar4ducNyCOfFEonj_-A5_0SBuz5pxllxRqTixcDjUhIoXKmR9ZCCLZBS1oZmzggnHL7DVHAvGDLLxHkctSjZMqvZcb_KgjPyjs5JTNPwhQwDhSKGtUuxYmeHgUshYgorJPkgjpaMgt-KRMXoRhpvUgDMaUs7Mx1hLjresCQEdvX4gS6jsn3drCq5UxDCqdyAiR0Ru1qfDGrWn_44lWXoxrg2KFfIcY_9IlRQxTlgmfVwPYQiSCvsduX9juUlBeFdvo2yH58vdqlHbMu_q2utgxe4ORdS1lL1c_m_g7em-y1uXd2ZIEu70aNWn0XHVd1UQKEwiYgpgrRIqPNRylMpbpJaV57kxa3adHy5E29WaUnBA4qumjm2f01YtV0u0VAr2SMX8PzLs4i0ZHt3U9cwTnPalNjUKdyKg2i1C_-WBi3W30XfpqmEWRM9436jHWaN7k784FSxM8NKansU1lzglfc88CaVWGBgDi9QWnsXl0vfJ5vE4MXi-xm9lo8gJ4RBsgHZ3ZM4FCUm0smwps4MnDq1byyXV_9WlUKYJCnOV0aJ0Cm0fyEA0746UBqLSuaGIoSAI7gigRLEGOXaY0LLgGvhW8cieQUWLnzVRMQP3pFACPd-3Q07W8Lkezw7IDpHWZTq6pVEu1NZxqZxmbb2d3e4-bHaBLnbCsE_Ib4wwwyG_X-4o-gtcCm-eCOCW8mXh_uuGdK3630m6Fy9W7SMRdJ1rhxH6n04NR5crY5Gm52D_Ds8nAe-Hdf7zHR06SVhia3q1kJAJ24nSVOXpg_cw2pocrRtkHBbh7DBK3t4wNO2FrHRb28dqH6vgL5KrQsEFjSlhqjK-dDgeMPnY8UVKeQ5z1257oEO0chvIYdQYltShj8edgPXD2Y2WyQx78hyTQoxCWjsUR86lmFCHME_Tho3hq3jB3QPCqlRX3Dlqbe08x4ejSmuwaIpa3f7iN_DHKGX0DOF48Ty3f8eW8Wt3HirNclU517B01i6Y62lFZ7SNpDMtL_waZY6Np_iXelNenzix5mSsM3mOjiiN6D8TesCtH6TpykIGVy8fww8qZfG-niVbt6yMRBBj2eoQqxCBAezAAyoFlwVmNRa7Sw86C1s7Yg3FoQUczGpu89CI2CfpxSjCFK_54KgA42i54ENxQc2KUKgkjSIqTzTj7y4wGwjHutc4J-o9skJwXIi4uGAzHIRQFU847GNIfk1StYudvIab-2dy1HVdLdQkaP61Z6h4AWqDprvjhDscSR6urcX3pVO2RVBsemavFdCPW_5mgMLwa94HL3orxczZOi_nTDKuFLEwxwmV_lK42xb9t1CeWQBIqYnd3nEfXf40LfcMlcbbfI2l6nnc3c1uE2nPOC3_qPYTqyFaae5Lx29mVY5QtA_3MgAzpCZ44oPJD5MhDaHUEuC2cPADc5xKaABd0BnaocRpSiGu6L1kgVtnHpUUTCiNdfNAqceMzccUgUtrzQqo8-vKA7ixjKw9n5THfjTkRXnjUT5dv-7YSJuk9zh32uvr3f4hs_FvhvBIMpKIbe9jFtIbTMwrnMd8LlxB8LTSvcW1h4D5y6b1pnNHYbHjvcl11helTHgvMkotTarZGja9aj6ORp3gC-vfWQnmGJ-57GKDt4RjKxp2tMiZrbfagdRChhic7OpgR5prURALtPT9FfRyoZ7U2RQItcfK6ntEymEmkDvDA_O1tugNvhvLjdpk4QLupEciajsJ4ip8G_PsvyALcUqJdAut3lbtebx7s9BUqWsDAYbaWuF1DBxVN4yt0n6ISIwuxeFvGExUISVHMUmfIVT7dx30z8qZvzLx7ZWhHiFkwGRLJYiHJ3SY3B2LdGtyoTVZfR44VQkHAsermCnUQz8wYIvcep34aY58cpaR_eoEC43wtm0ERTg677ILF8dscNvpFBBtrOagTTvfm3ahYFBXzHkeEStjFGq-9Nwsi1kS9XblOPB-EqB-oshXWclC6g8EcjSfNhxMv7-my0)

#### Обоснование

1. Минимизация изменений в АБС

АБС - критичная система, её переработка рискованна. 

Решение: Добавлен Deposit Processor как отдельный сервис в АБС, подписанный на Kafka. Это позволяет:
- Избежать прямого доступа интернет-банка к API АБС (+R4).
- Сохранить вертикальное масштабирование АБС (+R3).

2. Гарантированная доставка событий

Требуется надёжность (R1) и отсутствие потерь данных.

Решение:
- Transaction Outbox в интернет-банке (локальное хранилище событий перед отправкой в Kafka).
- Apache Kafka как брокер (поддержка репликации, восстановление после сбоев).

Альтернатива (RabbitMQ) не рассматривалась, так как Kafka лучше подходит для масштабируемости (+R6), плюс в требованиях от заказчика было это условие.

3. Скорость отклика (P1, P2)

Интернет-банк должен быстро отвечать, даже при высокой нагрузке.

Решение:  
- Микросервис Deposit Service (отделен от монолита).
- Горизонтальное масштабирование (можно добавлять инстансы под нагрузкой).
- Дополнительно можно добавить API Gateway, который балансирует запросы между ЦОД (R1, P2).

4. Использование текущего стека технологий (+R5)

Интернет-банк:
- Оставлен ASP.NET MVC 4.5 (минимальные изменения).
- Новый микросервис (Deposit Service) - на .NET Core (совместим с текущей инфраструктурой).

АБС:
- Delphi + Oracle (Deposit Processor работает через PL/SQL).
- Kafka развёртывается в инфраструктуре банка (не требует изменений в АБС).

5. Безопасность (+R1, +R2)

- Все данные шифруются (TLS, шифрование в Kafka).
- СМС-шлюз дорабатывается внутренней командой (+R2).

6. Поддерживаемость (S1)

Документируется:
- Схема взаимодействия сервисов.
- Протокол событий Kafka.
- API Deposit Service.

### <a name="_bjrr7veeh80c"></a>**Альтернативы**

1. Прямая интеграция интернет-банка с АБС через API

Интернет-банк напрямую вызывает API АБС для открытия депозитов без использования брокера сообщений.

Плюсы:
- Простота реализации (не нужна Kafka, Transaction Outbox).
- Меньше задержек (синхронный вызов).

Минусы:
- Нарушает +R4 (запрет прямого доступа к АБС).
- Нет отказоустойчивости – при падении АБС интернет-банк тоже ломается (R1).
- Сложнее масштабировать (P2) (АБС поддерживает только вертикальное масштабирование).

Вывод: Не подходит из-за требований безопасности и надёжности.

2. Использование RabbitMQ вместо Kafka

RabbitMQ как брокер сообщений для асинхронной обработки заявок.

Плюсы:
- Проще в настройке, чем Kafka.
- Поддерживает транзакционность.

Минусы:
- Менее отказоустойчив при массовых сбоях (R1).
- Сложнее масштабировать для будущих задач (+R6).
- Нарушает требование заказчика.

Вывод: Kafka предпочтительнее из-за требований к масштабируемости и надёжности.

3. Полный отказ от микросервисов (доработка монолита)

Оставить интернет-банк как монолит и добавить функционал депозитов в него.

Плюсы:
- Быстрее внедрить (не нужно разбивать систему).
- Меньше операционных затрат (одна codebase).

Минусы:
- Замедлит работу интернет-банка (P1).
- Усложнит дальнейшее развитие (S1) (например, добавление кредитов).
- Риск нарушить работу существующего функционала.

Вывод: Strangler Fig (постепенный переход на микросервисы) безопаснее.


**Недостатки, ограничения, риски**

1. Сложность внедрения Kafka

Текущая версия интернет-банка несовместима с Kafka (+R6).

2. Задержки при обработке событий

Асинхронность Kafka может привести к задержкам (не мгновенное открытие депозита).

3. Риски при распиле монолита

Ошибки при выделении Deposit Service могут нарушить работу интернет-банка.

