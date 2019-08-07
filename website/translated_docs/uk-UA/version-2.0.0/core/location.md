---
title: Місцерозташування
id: version-2.0.0-location
original_id: місцерозташування
---

## Огляд

Оновлення місцерозташування надсилаються з вашого пристрою до домашнього помічника в ряді ситуацій:

* Коли ви входите або виходите з [зони](https://www.home-assistant.io/components/zone/), визначеної в домашньому помічнику.
* Коли iBeacon виявлено або втрачено (див. [нижче](#ibeacons)).
* Коли додаток відкрито і він не був відкритий у фоновому режимі.
* Через автоматичний фоновий вибір.
* Коли запит на оновлення здійснюється за допомогою [спеціального повідомлення](notifications/location.md)
* Коли відкрито посилання [URL Handler](integrations/url-handler.md).
* Коли програма викликається за допомогою [X-Callback-URL](integrations/x-callback-url.md).
* Коли ваші пристрої виявляють [ *значущі зміни місцерозташування*](#location-tracking-when-outside-a-home-assistant-zone).
* Вручну, коли програма оновлюється (проведіть пальцем вниз у верхній частині сторінки) або з контекстного меню, відкритого в 3D, торкаючись піктограми програми.

Ви можете перевірити причину останнього оновлення місцерозташування, перевіривши значення `sensor.last_update_trigger`

Залежно від налаштувань, дані про місцерозташування надсилаються безпосередньо з вашого телефону на примірники Home Assistant або через Home Assistant Cloud Service. Це буде залежати від URL-адреси, зазначених у розділі "З'єднання" в меню "Конфігурація програми". Дані про місцерозташування не надсилаються через інші сервери або організації. Звичайно, якщо ви вирішили не надати дозвіл на розташування Home Assistant Companion або ви згодом видалили дозволи на місцерозташування (через налаштування iOS> конфіденційність> служби визначення місцезнаходження), дані про місцерозташування не надсилатимуться з вашого пристрою до домашнього помічника. **Перевірте чі це так для оновлення сповіщень**

## Початок роботи

Після першого встановлення та відкриття програми Home Assistant Companion буде створено новий об'єкт `device_tracker.`. За замовчуванням об'єкт буде мати назву форми `device_tracker.<device_ID>` де `<device_ID>` це ім'я, яке ви вказали в iOS (назва: Налаштування> Загальні> Про). Ви можете перевірити ім'я об'єкта в Home Assistant, відвідавши розділ "Інтеграція" на сторінці конфігурації з бічної панелі (проведіть праворуч, якщо ви використовуєте додаток), а потім клацнувши або торкнувшись інтеграції мобільного додатка для вашого пристрою і прокрутивши список суб'єктів. Якщо потрібно, ви можете редагувати атрибут `name` об'єкта.

Нижче наведено основний приклад для включення світла, коли ви вводите *home* зону після темряви.

```yaml
automation:
  - alias: 'Turn door light on when getting home'
    trigger:
      platform: state
      entity_id: device_tracker.<device_ID>
      to: 'home'
    condition:
      condition: sun
      after: sunset
    action:
      - service: light.turn_on
        data:
          entity_id: light.frontdoor
```

## Відстеження місця розташування за межами зони Home Assistant

Додаток Home Assistant отримує *значні оновлення місцерозсташування* з iOS. При отриманні оновлення він надсилається до Home Assistant. Приблизно оновлюється щоразу, коли ваш пристрій переходить на нову стільникову вежу, пройшов значний час (як правило, кілька годин) або змінюється стан з'єднання, і система помічає, що ваше місце розташування нещодавно змінилося.

Apple опріділює [defines](https://developer.apple.com/library/content/documentation/UserExperience/Conceptual/LocationAwarenessPG/CoreLocation/CoreLocation.html#//apple_ref/doc/uid/TP40009497-CH2-SW9) значні оновлення місцезнаходження значних змін як:

> Служба визначення місцезнаходження із значними змінами надає оновлення лише тоді, коли відбулося значна зміна розташування пристрою, наприклад 500 метрів або більше.

Вони також говорять у [Energy Efficiency Guide](https://developer.apple.com/library/content/documentation/Performance/Conceptual/EnergyGuide-iOS/LocationBestPractices.html#//apple_ref/doc/uid/TP40015243-CH24-SW4):

> Оновлення місцеположень із значними змінами розбуджують систему та вашу програму раз на 15 хвилин, принаймні, навіть якщо не відбулися зміни місцезнаходження.

Нарешті, я думаю, що ця відповідь на [переповнення стеків](http://stackoverflow.com/a/13331625/486182) говорить найкраще:

> Значне зміна розташування є найменш точним з усіх типів моніторингу місцезнаходження. Він оновлюється лише тоді, коли відбувається перехід або зміна стільникової вежі. Це може означати різний рівень точності та оновлення в залежності від того, де знаходиться користувач. Міська зона, більше оновлень з великою кількістю веж. За межами міста, міждержавних, менше веж і змін.

У чому реальна історія щодо значних змін оновлення місцезнаходження? Хто знає, тому що Apple зберігає її приватною.

## Відстеження місцезнаходження в зонах Home Assistant

При запуску Home Assistant для iOS встановлює геозони для всіх зон у вашій конфігурації Home Assistant. Повідомлення про вхід і вихід надсилаються до головного помічника.

### Конфігурація

Додайте `track_ios: false` до своїх налаштувань зони, щоб вимкнути відстеження розташування зон для всіх підключених додатків iOS.

### iBeacons

Додаток має базову підтримку для використання iBeacons для запуску оновлень входу/ виходу. Щоб налаштувати їх, додайте відомості про iBeacon до своєї зони так:

```yaml
zone.home:
  beacon:
    uuid: B9407F30-F5F8-466E-AFF9-25556B57FE6D
    major: 60042
    minor: 43814
```

Перезапустіть Home Assistant, а потім додаток iOS. Потім він почне використовувати iBeacons *замість вашого місцерозсташування* для введення тригерів навколо ваших зон. Щоб додати iBeacon до `zone.home`, додайте вищезгадане під `customize`.