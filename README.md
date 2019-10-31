# Протокол JWT-RTC

Тут содержится документация и описание протокола JWT-RTC.

## Для чего?

формат [JWT](https://jwt.io/introduction/) представляет собой контейнер для информации, защищенной от изменений.
В обычном использовании, мы представляем его как Base64 URL-кодированную строку, состоящию из заголовка, тела и подписи, разделенных точками.
Такой формат подходит для использования в качестве значений полей документов которыми мы обмениваемся.
Хотя, сам JWT представляет собой полноценный документ, уже включающий заголовок с мета-информацией и сигнатуру подписи, не говоря о полноценном произвольном JSON в качестве тела такого докумнта.

Тут мы предлагаем использовать несколько изменений формат JWT в качестве носителя данных в протоколе сетевого взаимодействия.
Изменения включают:

1. Произвольный	разделитель между блоками заголовка, тела и подписи документа.
2. Отказ от обязательного кодирования блоков документа в Base64.
3. Отказ от обязательного использования JSON в качестве разметки полезной информации.

Таким образом, более гибкий формат документа позволяет быть использованым совместно с уже существующими протоколами, и в то же время, наследует все особенности использования JWT.

## Как?

Далее представлен сборник документов, описывающих прикладное использование протокола:

* [JSON как простой список](json-as-plain.md) - пример отказа от использования JSON как разметки информации в пользу совместимости с форматом списка заголовков.
* [JWT как документ](jwt-as-bocument.md) - пример формирования JWT с произвольными разделителями и кодированием блоков.
* [JWT-RTC поверх HTTP](jwt-ower-http.md) - пример использвоания JWT-RTC в запросах протокола HTTP.

## Лицензия и вклад сообщества

Данная документация опубликована под [лицензией MIT](LICENSE).

Для внесения изменений в данную документацию, следуйте [принципам сообщества GitHub](https://guides.github.com/introduction/flow/).