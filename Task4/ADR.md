### <a name="_b7urdng99y53"></a>**Название задачи:** MVP передачи ставок в кол-центр
### <a name="_hjk0fkfyohdk"></a>**Автор:** Руслан
### <a name="_uanumrh8zrui"></a>**Дата:** 26.07.2025
### <a name="_3bfxc9a45514"></a>**Функциональные требования**

|**№**|**Действующие лица или системы**|**Use Case**|**Описание**|
| :-: | :- | :- | :- |
|UC1|Сотрудник бэк-офиса, сотрудник кол-центра|Передача ставки в кол-центр (текущее решение)|1. Выделенный сотрудник бэк-офиса отправляет ежедневную рассылку с файловым вложением актуальной процентной ставки всем сотрудникам кол-центра<br>2. Сотрудник кол-центра получает письмо с утренней рассылкой|
|UC2|Сервис ставок, Сервис ЦБ РФ, Сервис уведомлений, сотрудник кол-центра|Передача ставки в кол-центр MVP|1. Сервис ставок отправляет запрос к API центрального банка РФ<br>2. ЦБ РФ присылает актуальную процентную ставку<br>3. Сервис ставок выполняет дополнительные вычисления ставки с учетом коэффициентов банка по определенным бизнес-правилам<br>4. Сервис ставок отправляет сообщение на отправку рассылки актуальной ставки в очередь<br>5. Сервис уведомлений получает сообщения из очереди и выполняет рассылку кол-центрам в виде файлов<br>6. Сотрудник кол-центра получает письмо с утренней рассылкой|

### <a name="_u8xz25hbrgql"></a>**Нефункциональные требования**

Взято из задания 3.

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

[![](https://img.plantuml.biz/plantuml/svg/jLPDJnDH5DttLpoxmiJIZIjNQCF2H8NGk2R3z24JfZEJwQl23bhyIjK6DoR6e1ZZnaqjRHXRMX9-mNt_o7FVj0CTMy0OqsJwtkkTSyyzzzm72nLbXQfQTZCPvIXN2lf8Gnhe6dMfftVnFQ2MRWhgK4GjEj8xU9xWqnBq4yi1zR0y-q672ELj1-4xkdRM1w0V2EMBDLyflonAJYXjvVXU9dF3yMotMfBLq1KRIWMLE_byQ3sRV-AeZUfQjI93s_UKzDIyxPVpAuvjXIN_aIC3wUOBjtFBhkMfnmyVvCjMHSaGHwi5lnpu7hyHc5gMPLZnlJdRTN1sIsHHLHy5TBcoR6eli305exO1dhNS5PLbRuxnronxSsbztR7bOZbm_Mqf6VkGXhg6q3fU6G2y1wdqM-hbSBv7aTwbbk5mW6EM4zaGtw0srbtcSyoIOzkZIE2HC4LdskV97KWKUfPRh0Q17weh6BG1KET3Xk8wwOY6Z3waVaw_Y1CmdIkfqIb-6i8RZKl8oQs_90VuL_-HLd4RLYdF5PNbbU2cU1kpYOpDkzVnTFQIDv07dKNBz7E11RkfRPeT4nWnM5tSKdDAkXBENWfaQ2a_PD2lWElpp27y14QXKuFGuWBqZidxFX4febyY7IAmXN2GrWZHClRf2h3zH8U902Rzk3S5otKBabqq1JxTWKaAkeax0RLTL6BK6Mvk0xzDT25M1c4H4pTDrA-D4oCu7SUJ36sm2YttdkLj5cNu5FCL2v-8oat-Jlk2lj0tPdPt-JuV96wXldu3y67iflsagOXdYu0uC-aWh6c4mwAkNuu92SJt05I_00H9OeED51aTclfZiRc6p8fqarldJxCT8BW7ArrmRWTmJE0OYMj0O80frqeAwpgNXwuBlhodfqZXfHqPKnqR2IfD7JE3Fk4upcaSbuDuRUJji9-DiuslHjdJwKQApSWw6r0tnvgdjRxaSloSj7ka65__Upn3R6FqDzBl1DicrM_TC9ETzBk5xVbzCLrNCUiU_8yq38zPUQVCFeyrwyeauYdWnIN3RFu3YAONF6Loi_JXt3R_C5eBqYlXZ_jl)](https://editor.plantuml.com/uml/jLPDJnDH5DttLpoxmiJIZIjNQCF2H8NGk2R3z24JfZEJwQl23bhyIjK6DoR6e1ZZnaqjRHXRMX9-mNt_o7FVj0CTMy0OqsJwtkkTSyyzzzm72nLbXQfQTZCPvIXN2lf8Gnhe6dMfftVnFQ2MRWhgK4GjEj8xU9xWqnBq4yi1zR0y-q672ELj1-4xkdRM1w0V2EMBDLyflonAJYXjvVXU9dF3yMotMfBLq1KRIWMLE_byQ3sRV-AeZUfQjI93s_UKzDIyxPVpAuvjXIN_aIC3wUOBjtFBhkMfnmyVvCjMHSaGHwi5lnpu7hyHc5gMPLZnlJdRTN1sIsHHLHy5TBcoR6eli305exO1dhNS5PLbRuxnronxSsbztR7bOZbm_Mqf6VkGXhg6q3fU6G2y1wdqM-hbSBv7aTwbbk5mW6EM4zaGtw0srbtcSyoIOzkZIE2HC4LdskV97KWKUfPRh0Q17weh6BG1KET3Xk8wwOY6Z3waVaw_Y1CmdIkfqIb-6i8RZKl8oQs_90VuL_-HLd4RLYdF5PNbbU2cU1kpYOpDkzVnTFQIDv07dKNBz7E11RkfRPeT4nWnM5tSKdDAkXBENWfaQ2a_PD2lWElpp27y14QXKuFGuWBqZidxFX4febyY7IAmXN2GrWZHClRf2h3zH8U902Rzk3S5otKBabqq1JxTWKaAkeax0RLTL6BK6Mvk0xzDT25M1c4H4pTDrA-D4oCu7SUJ36sm2YttdkLj5cNu5FCL2v-8oat-Jlk2lj0tPdPt-JuV96wXldu3y67iflsagOXdYu0uC-aWh6c4mwAkNuu92SJt05I_00H9OeED51aTclfZiRc6p8fqarldJxCT8BW7ArrmRWTmJE0OYMj0O80frqeAwpgNXwuBlhodfqZXfHqPKnqR2IfD7JE3Fk4upcaSbuDuRUJji9-DiuslHjdJwKQApSWw6r0tnvgdjRxaSloSj7ka65__Upn3R6FqDzBl1DicrM_TC9ETzBk5xVbzCLrNCUiU_8yq38zPUQVCFeyrwyeauYdWnIN3RFu3YAONF6Loi_JXt3R_C5eBqYlXZ_jl)

[![](https://img.plantuml.biz/plantuml/svg/nLbFJnjN4B_xKxpwH2GCbwg7dW86jKa3k9YsnsXPlu3BshjQhvEWgbA041gHX19LQbMrILEbwQ4Nny6niS58-GHllr5dPlzuz-rR6sfG5IMnrszdVZFpcpyxUxtcQBPJhnWPf-mOd8cVH4Sqn3lt0Vnx0dyQJ7H5Nvowsw8bZiMfQC5FVT5auYsSE8KV6-p3F-8L_8vVZqJ3VU1kV-YnYPMbtEpyjKp6CItcc6pLT1opmafbc-jEsQncCX-LgxfHB_6wRR1rnx5gdqvFszgzgRMoirvVhTUuhPjLXrUTATsiJ2-NTSqkcOjbRd5ZElznhc1eLUUhXLlJ5QtcS1iktSwR5SkiuXqMcfGfSBjcLYTqempN9bbM_2fwO5yBpSbAtnby82l1FhHoEpV2mkorGFwgfcyitRbJrlbynJBCJSvHzYlHTxVXw0xSSWh2k-0ozwdevk3wGz5njqI3CBo0o-Xjq0RdzqKJFhSGJnik7C3NhkXW44AawFw1yZAuoAvgHh5kMQRjZ43G104xU155ePtYdUXx8Uxbt4UU0eGp4feuGoAuM-x-473y_WN1WVWVhX1M9W-CqifLRj-UDUlLacPlJXGT3J-KYfj0eue73Q93X2V-Zw31a3j0AOGPukVkClY0r6fIv0aD-ox3M0XeuWO5aZkpMdLZCYj-IM5Z5dvxIV4x8ZiHJbFqW2daWeHgqOgfTlV13L6rCtL8IytX9THRhgxvRiYA7q42-kq4lGnosd0xfJvfE4Lo9jNXHVWGySPmlEYBE0smxZ3t8Lm0kH3w3bOVkDXpdy3_9C5zmiGPygCV4W64mAsj60MyCYMH819EoH5vpJ3o78CGka0AgugEsVpoGYvkyIIZ69_XQG3T1bFeElhh22rdXB0JY6swkt3qo3rm7nDJm8Oc6TtrMHj5MQpKFeDetTCs2TzlubKECgRdFXDjHW1wy1TlF84w0nWmy1Y_lWzt67Vauj0Hnv6g24KBEe2x5Hg1FZyZ__iroeUp0zz90vZN8r4W29ClQiS2hzMqDJvhcnlS1bY7nAqkAY9IK6mVXtTFPcSiJL_dx0ljpeP6EJSi2OOA2mre4biw01FSxna7aZ1D3XJf6iU_346e5Rbz5we-HI8ypK8yVShMgSNv5VR9Y3HGy9rHvBRSFJpXKRU3pDa11lUyJA7ZSB4btfDWB9mOn7raaPS5FbdzYr5eoKpmc-VSQiowEStHLhKQMda8DxIHiaTU3K2P68k2MNFMR5xyyXP5vYyY1VWQOn4UnUoahFLI1GdP0UFydFPi6uwOO2vp8-xy5An_e_gWCNJymk1Lm47387083N43_EcLV86qn6AzQ3fbwEuQZYqf8LTpVlpW8zeUPPSYAUIeAN1UUl1wWE6TLwEZ2AFHKRbhEBHnOlMzigd7cYdBuYrXmnquB7hg79vQAB9pUkaCNDAWSloUV4NnypCPlYSY5mF2BRDMTYuL3rOtx4Tjtz4dSZcB0lY6h-QzYJahdiEDRtFH9egbk5WWNiD1DbEumH1cYPbL5h1zQ7N6Ew8jf0kjPHyBWDSVeq2JxSztZZeB5NV_hs0BscO5t9W2DigHbjVYBWtgcJ-fjGZZgGTNLYZDWReEoPC2Djg6hjIdCKN4KQmeKjI7PyjuXH_LIN5BgHBZwv5SVWLwV4Ngx4xaqdCGq8v8XK2Xk9l5fSNfprTM2iKGkLfWWk_Z2anmyZm2rGqXvJ57QWrXCgpMvdXjmp6j6Sio_Bu5-d-dbhVjJ_Omkb25fM4UU-1xlpL35B9ptB3MooDI8JfbmX0HT0WM6TqVUIChbl5e2o4gl8LRTgaTz_ndHHW74yq5ZHdLJB9BjgOR70UL4Qqa-O1v8i45snBznUTQmJPr6FpH_-gYgBHRqRu6azNv_PjuNEoSNv_iqpHxHZiwQDeZ7Qruk9NH9diUBkqwWyHNypXEYF7hM6fmp-6bAzH-UMQgIqfYOP1IJ0RdHavMorpJdQblQwCUS1KFvLGW1dnf4JcYsn2YXcefbeerLlker6PlQdSrLhHiS3ERDKtd0gq-efWAHny9lESNr4viQuJBEqMoZjhj9B9qOCtel8m-1etCJSclRB3iyHPauIRcU92BAXgak_i2lJDLYDByCVF0e_hj-VlEXCCDhfkL9OlRccFQo8G_XowWY2pFmoKlrLlfI53xOgkbbhzQ0WGHzS5kUM2bGhoyiej3S0yIwyf83W4BCOTatdQyFFBv6JoEOzEZoXj1g0CgMpDmuWXg4elrE-Jv6t78n1_YDIAZT7apM2-6KmPjmm_3FJISnrpRyhO_mSnF5AGP8fZu6p1RRxix4K77YIsBbuT1udQzxe6q2jwQI8_IuaKmiaVf4kb1Q4X0K9IZU3kIkdCU_Ix4tM6u8P5rUgeYor6Pd1xkW5DX8PNU693a6FbGtz2zffoJY4DMhWT5NuaXNRnx0BMek50CGX8BHMmJ_9hsQIUueiTryyLCmxekcw2U7AvEPz8-UKEiybrYAGQbeeagzoZ9m9tkaq3JpDVBIxvngIE3h3DQB6LTc60TqOvfW-o9Q_DeARrX6lBAxcMOn8r1ft_eEzgRhfynsZ0DsvNYQdWtljzLuKXxEtU5C0X7cbv5HVVcf2Cl0FxpqT0I7Kx02eCLzJWhVXvGTio26TjA_rVsmbSrZ9Z69ALLIzVnsHmqbyGs6cybOCr1AVHdqAVU140Th4_laVh-R-uUTbavGwxpQgbUCVu5)](https://editor.plantuml.com/uml/nLbFJnjN4B_xKxpwH2GCbwg7dW86jKa3k9YsnsXPlu3BshjQhvEWgbA041gHX19LQbMrILEbwQ4Nny6niS58-GHllr5dPlzuz-rR6sfG5IMnrszdVZFpcpyxUxtcQBPJhnWPf-mOd8cVH4Sqn3lt0Vnx0dyQJ7H5Nvowsw8bZiMfQC5FVT5auYsSE8KV6-p3F-8L_8vVZqJ3VU1kV-YnYPMbtEpyjKp6CItcc6pLT1opmafbc-jEsQncCX-LgxfHB_6wRR1rnx5gdqvFszgzgRMoirvVhTUuhPjLXrUTATsiJ2-NTSqkcOjbRd5ZElznhc1eLUUhXLlJ5QtcS1iktSwR5SkiuXqMcfGfSBjcLYTqempN9bbM_2fwO5yBpSbAtnby82l1FhHoEpV2mkorGFwgfcyitRbJrlbynJBCJSvHzYlHTxVXw0xSSWh2k-0ozwdevk3wGz5njqI3CBo0o-Xjq0RdzqKJFhSGJnik7C3NhkXW44AawFw1yZAuoAvgHh5kMQRjZ43G104xU155ePtYdUXx8Uxbt4UU0eGp4feuGoAuM-x-473y_WN1WVWVhX1M9W-CqifLRj-UDUlLacPlJXGT3J-KYfj0eue73Q93X2V-Zw31a3j0AOGPukVkClY0r6fIv0aD-ox3M0XeuWO5aZkpMdLZCYj-IM5Z5dvxIV4x8ZiHJbFqW2daWeHgqOgfTlV13L6rCtL8IytX9THRhgxvRiYA7q42-kq4lGnosd0xfJvfE4Lo9jNXHVWGySPmlEYBE0smxZ3t8Lm0kH3w3bOVkDXpdy3_9C5zmiGPygCV4W64mAsj60MyCYMH819EoH5vpJ3o78CGka0AgugEsVpoGYvkyIIZ69_XQG3T1bFeElhh22rdXB0JY6swkt3qo3rm7nDJm8Oc6TtrMHj5MQpKFeDetTCs2TzlubKECgRdFXDjHW1wy1TlF84w0nWmy1Y_lWzt67Vauj0Hnv6g24KBEe2x5Hg1FZyZ__iroeUp0zz90vZN8r4W29ClQiS2hzMqDJvhcnlS1bY7nAqkAY9IK6mVXtTFPcSiJL_dx0ljpeP6EJSi2OOA2mre4biw01FSxna7aZ1D3XJf6iU_346e5Rbz5we-HI8ypK8yVShMgSNv5VR9Y3HGy9rHvBRSFJpXKRU3pDa11lUyJA7ZSB4btfDWB9mOn7raaPS5FbdzYr5eoKpmc-VSQiowEStHLhKQMda8DxIHiaTU3K2P68k2MNFMR5xyyXP5vYyY1VWQOn4UnUoahFLI1GdP0UFydFPi6uwOO2vp8-xy5An_e_gWCNJymk1Lm47387083N43_EcLV86qn6AzQ3fbwEuQZYqf8LTpVlpW8zeUPPSYAUIeAN1UUl1wWE6TLwEZ2AFHKRbhEBHnOlMzigd7cYdBuYrXmnquB7hg79vQAB9pUkaCNDAWSloUV4NnypCPlYSY5mF2BRDMTYuL3rOtx4Tjtz4dSZcB0lY6h-QzYJahdiEDRtFH9egbk5WWNiD1DbEumH1cYPbL5h1zQ7N6Ew8jf0kjPHyBWDSVeq2JxSztZZeB5NV_hs0BscO5t9W2DigHbjVYBWtgcJ-fjGZZgGTNLYZDWReEoPC2Djg6hjIdCKN4KQmeKjI7PyjuXH_LIN5BgHBZwv5SVWLwV4Ngx4xaqdCGq8v8XK2Xk9l5fSNfprTM2iKGkLfWWk_Z2anmyZm2rGqXvJ57QWrXCgpMvdXjmp6j6Sio_Bu5-d-dbhVjJ_Omkb25fM4UU-1xlpL35B9ptB3MooDI8JfbmX0HT0WM6TqVUIChbl5e2o4gl8LRTgaTz_ndHHW74yq5ZHdLJB9BjgOR70UL4Qqa-O1v8i45snBznUTQmJPr6FpH_-gYgBHRqRu6azNv_PjuNEoSNv_iqpHxHZiwQDeZ7Qruk9NH9diUBkqwWyHNypXEYF7hM6fmp-6bAzH-UMQgIqfYOP1IJ0RdHavMorpJdQblQwCUS1KFvLGW1dnf4JcYsn2YXcefbeerLlker6PlQdSrLhHiS3ERDKtd0gq-efWAHny9lESNr4viQuJBEqMoZjhj9B9qOCtel8m-1etCJSclRB3iyHPauIRcU92BAXgak_i2lJDLYDByCVF0e_hj-VlEXCCDhfkL9OlRccFQo8G_XowWY2pFmoKlrLlfI53xOgkbbhzQ0WGHzS5kUM2bGhoyiej3S0yIwyf83W4BCOTatdQyFFBv6JoEOzEZoXj1g0CgMpDmuWXg4elrE-Jv6t78n1_YDIAZT7apM2-6KmPjmm_3FJISnrpRyhO_mSnF5AGP8fZu6p1RRxix4K77YIsBbuT1udQzxe6q2jwQI8_IuaKmiaVf4kb1Q4X0K9IZU3kIkdCU_Ix4tM6u8P5rUgeYor6Pd1xkW5DX8PNU693a6FbGtz2zffoJY4DMhWT5NuaXNRnx0BMek50CGX8BHMmJ_9hsQIUueiTryyLCmxekcw2U7AvEPz8-UKEiybrYAGQbeeagzoZ9m9tkaq3JpDVBIxvngIE3h3DQB6LTc60TqOvfW-o9Q_DeARrX6lBAxcMOn8r1ft_eEzgRhfynsZ0DsvNYQdWtljzLuKXxEtU5C0X7cbv5HVVcf2Cl0FxpqT0I7Kx02eCLzJWhVXvGTio26TjA_rVsmbSrZ9Z69ALLIzVnsHmqbyGs6cybOCr1AVHdqAVU140Th4_laVh-R-uUTbavGwxpQgbUCVu5)

[![](https://img.plantuml.biz/plantuml/svg/ZPJFJi904CRlVOfv0IAqjDmCU3Dm8D4RwM432nORAjsj2JVHn7Wp9Xuy6cCy8zXG-UShJD_8ATYKXDJifTcPdVRxzVisMnMI-RB71b9gsfNlzNbRYZPpEGm3sk2-EzAQkpPiHmN82mub8S7hGZ-WGRsecreBNt62y_Y6df-uYMznWHE8nnXIQueHhPAI-XCgDxYmWGbedTVEwYKRV3uC79yBC8hGOIEXfHkEWBRdUZxlW3E01hlcWmxtZnfExqAUk04dtHTSqrT3d2NQyJr9FVwdImNKcgU07_W41FsUWUXx82Lp3qHN0TaH5ux_tiXn19S4CSE8YWYe7-4SrmGxBk3FTfIOSPdZb947QAW2wYcWOMkAcTtHCiGl8baHV4Yq4NvdJEMyzCeAJ2hnRN2QP5PdtKkMoPfvigQg5KPgXMcdm5c3mDwIL0Wj8X9Y88-IFmyp12iFmL95C5D5QwludAHDVgyIbm-LWSemVZYCe99tB2gg85OAKY6GAetiBeHV)](https://editor.plantuml.com/uml/ZPJFJi904CRlVOfv0IAqjDmCU3Dm8D4RwM432nORAjsj2JVHn7Wp9Xuy6cCy8zXG-UShJD_8ATYKXDJifTcPdVRxzVisMnMI-RB71b9gsfNlzNbRYZPpEGm3sk2-EzAQkpPiHmN82mub8S7hGZ-WGRsecreBNt62y_Y6df-uYMznWHE8nnXIQueHhPAI-XCgDxYmWGbedTVEwYKRV3uC79yBC8hGOIEXfHkEWBRdUZxlW3E01hlcWmxtZnfExqAUk04dtHTSqrT3d2NQyJr9FVwdImNKcgU07_W41FsUWUXx82Lp3qHN0TaH5ux_tiXn19S4CSE8YWYe7-4SrmGxBk3FTfIOSPdZb947QAW2wYcWOMkAcTtHCiGl8baHV4Yq4NvdJEMyzCeAJ2hnRN2QP5PdtKkMoPfvigQg5KPgXMcdm5c3mDwIL0Wj8X9Y88-IFmyp12iFmL95C5D5QwludAHDVgyIbm-LWSemVZYCe99tB2gg85OAKY6GAetiBeHV)

#### Обоснование

##### Временное решение

В качестве временного решения можно использовать ручную рассылку сообщений по актуальным процентным ставкам банкам со стороны ответственного лица (сотрудника бэк-офиса). Если в компании используется корпоративная почта и есть интеграция с LDAP, то у сотудников кол-центра должна быть выделенная группа ("Сотрудники кол-центра"). Сотрудник бэк-офиса может просто на ежедневной основе выполнять рассылку группе пользователей.

##### Полноценное решение MVP

Автоматизированное решение предполагает, что появляются новые сервисы:
1. Сервис ставок: отвечает за ежедневный расчет актуальной процентной ставки по бизнес-правилам банка (например: банк может вычитать из ставки банка N процентов или вычислять процент на основе формул и данных банковской системы). После расчета публиковать событие для дальнейшего "подхватывания" другим сервисом.
Сервис имеет свою БД для хранения истории процентных ставок, а также реализации паттерна Transaction Outbox.
2. Сервис уведомлений: отвечает за чтение событий из очереди и их дальнейшую отправку выделенной группе лиц. Также имеет свою БД для реализации Transaction Outbox.

### <a name="_bjrr7veeh80c"></a>**Альтернативы**

1. Ручная рассылка выделенным сотрудником (как сейчас)
В качестве временно решения хороший вариант, закрывающий потребности бизнеса.

2. Доработка АБС
АБС - критичная система и написана на устаревших технологиях. Ее изменения лучше минимизировать.

3. Прямой доступ кол-центров к выделенной веб-странице на сайте банка с ежедневным обновлением
Не до конца понятно разрешен ли такой вариант. Возможно, стоит уточнить у стейкхолдеров.
Если просто публиковать файлы по прямым ссылкам или сделать веб-страницу для операторов и клиентов - это будет намного удобнее.

**Недостатки, ограничения, риски**

Недостатки:
1. Большая сложность для MVP (два новых сервиса)
2. Возможные задержки из-за Kafka (но их можно минимизировать)

Ограничения:
1. Партнерский кол-центр работает только с файлами

Риски:
1. Риск не успеть за полгода
2. Дополнительные доработки если ставки придется менять много раз в день

