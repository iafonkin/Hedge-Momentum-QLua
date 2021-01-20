# Hedge-Momentum-QLua
Hedge Momentum QLua
Этот скрипт позволяет автоматизировать хеджирование длинной позиции в акциях на ММВБ,
на случай их просадки.
Скрипт поможет при сохранить Ваши финансовые вложения от просадок.

 * Установка скрипта
 * Настройка первичная
 * Что делает активный скрипт ?
 * Управление скриптом

## Установка скрипта.
Вопрос самый простой. Скопируйте файлы 
`hedge.lua`, `ini.txt` и `log.txt` на Ваш комп с установленным терминалом Quik одну директорию, к примеру */hedge* 

Запуск скрипта производится в Quik терминале : **Сервисы --> Lua скрипты --> Добавить -->** *выбираете в вашей папке, куда скопировали файлы* **hedge.lua** и нажимаете **Запустить**. Первый запуск скрипта **ОБЯЗАТЕЛЬНО !** делать только после **Настройки первичной**



## Настройка первичная 

Откройте файл ini.txt и установите значение в соответствующих строках:

###table
| Строка | Содержимое      |Пример |
|------|-----------------------------------------------------------------------|--------------|
| 1 | Торговый счет / код клиента. | A901TSU |
| 2 | Торговый счет / код клиенta | A901TSU |
| 3 | Класс фьючерсов |SPBFUT |

4..............Класс хеджируемой акции......................................................................TQBR

5..............Код хеджируемой акции...........................................................................HYDR

6..............- служебная информация = 0..................................................................0

7..............Код фьючерсного контракта (ближайший)......................................HYH1

8.............- служебная информация = 0....................................................................0

9.............- служебная информация = 0....................................................................0

10............Максимальный спред для расчета рекомендуемой

...............доли открытой позиции /(рекомендуется 0,15)................................0.15

11............Статус скрипта - в начале всегда "OFF"...............................................OFF

12............- служебная информация = 0....................................................................0

13............- служебная информация = 0....................................................................0


## Что делает активный скрипт ?
Теперь рассмотрим что может делать скрипт за Вас

При начале работы скрипт проверяет на актуальность указанный в ini.txt код фьючерса. И если у указанного фьючерса до экспирации остается менее 11 дней, то предлагает, соответствующим сообщением, заменить на следующий. Код нового контракта определяется скриптом самостоятельно (в данной версии, пока замена происходит только вручную !!! )

К сожалению, не все фьючерсы на ММВБ имеют большую мгновенную ликвидность. С целью возможности работы с такими фьючерсами, скрипт анализирует стакан котировок фьючерса. И в зависимости от размера длинной позиции в акции рекомендует долю, которую можно захеджировать, не сильно продавив котировки, а следовательно не потерять при входе в хедж.

Если у скрипта статус = ON (см. управление скриптом), то отслеживание открытия короткой позиции в фьючерсе на выбранный Вами объем происходит по моменту локального экстремума 200 часовой средней. В этот момент скрипт выставляет заявку на продажу фьючерсов по цене определяемой как последняя цена сделки плюс рекомендуемый процент (см. Настройка первоначальная).

Если у скрипта статус - HEDGE , то отслеживается обратная ситуация. При этом скрипт будет игнорировать незначительное изменение средней при локальном минимальном экстремуме, что позволяет уменьшить количество ложных выходов из позиции. При срабатывании, скрипт выставляет заявку на покупку (закрытие) открытой короткой позиции во фьючерсе.


## Управление скриптом

![](https://github.com/iafonkin/Hedge-Momentum-QLua/blob/main/window_1.png)


рассмотрим каждую строку таблицы

строки 1 и 2 указывается данные: код и класс хеджируемой бумаги и соответствующего фьючерса (см. Настройка первоначальная)

В строку 4 указывается размер вашего портфеля хеджируемой акции в штуках (по данным терминала)

В строке 5 скрипт показывает рекомендуемую часть портфеля акции в штуках и %% (расчет ведется на основании анализа стакана заявок и выбранного размера страхового спреда (см. Настройка первоначальная)

В строке 6 Вы выбираете на основании рекомендаций строки 5 хеджируемую часть путем нажатия на ячейку <-> или <+> , при этом изменяется выбираемый Вами размер хеджа с шагом размера минимального лота по фьючерсу (данные терминала и ММВБ). По умолчанию стоит весь портфель, т.е. 100%. Что бы быстро указать рекомендуемый скриптом размер- достаточно кликнуть на соответствующую ячейку в Строке 5.

В строке 7 указывается размер контанго или бэквордации в %% годовых (расчет идет при запуске) Информация очень ВАЖНА !!! Т.к. нужно быть осторожным при приближении дивидендной отсечки, о чём скажет состояние бэквордации. # Не рекомендуется хеджировать в таком состоянии.

В строке 8 указывается действующее на момент запуска размер ГО (гарантийное обеспечение) на 1 лот фьючерса и на весь объем планируемой к хеджированию части портфеля.

Строка 10 показывает статус скрипта, т.е. какие задачи сейчас стоят перед ним. Статус имеет три варианта: OFF, ON и HEDGE. Запуск (переход из OFF в ON и назад производиться по клику на "кнопке" в этой же строке, которая имеет соответствующие моменту надписи START или STOP
