# OpenAPI aggregator by Tags

[![Build project](https://github.com/Romanow/openapi-aggregator-by-tags/actions/workflows/build.yml/badge.svg)](https://github.com/Romanow/openapi-aggregator-by-tags/actions/workflows/build.yml)
[![pre-commit](https://img.shields.io/badge/pre--commit-enabled-brightgreen?logo=pre-commit)](https://github.com/pre-commit/pre-commit)

Генерация агрегированного OpenAPI по нескольким файлам:

1. Если ничего не задано, то результатом будет конкатенированный OpenAPI.
2. Если задан параметр `include`, то в результирующий OpenAPI добавляются только те path, для которых заданы _все_
   переданные `tags`. Параметр `exclude` не учитывается.
3. Если задан параметр `exclude`, то из всего множества `tags` удаляются те, что переданы в `exclude`.

При фильтрации результата используются следующий допущения:

* Целевые `tags` строятся на базе блоке `tags` в верхнем уровне описания OpenAPI и к ним применяется
  фильтрация `include`, `exclude`.
* Метод добавляется в результирующий OpenAPI, если всего `tags` содержатся в целевых `tags`.
* Если метод добавляется в результирующий OpenAPI, то его имена объектов request и response сохраняются и при
  копировании схемы копируются только те объекты, на которые ссылались целевые методы.
* Фильтрация выполняется только в блоке `schemas`, т.к. [springdoc](https://springdoc.org/) при генерации добавляет все
  объекты туда, но не заполняет `headers`, `parameters`, `requestBodies`, `responses`.

> ⚠️ **Важное замечание**
>
> При генерации OpenAPI с правилами фильтрации _требуется перечитывать_ декларации OpenAPI из файла, т.к. при удалении
> метода он помечается `null`, а это влияет на `apis: Map<String, OpenAPI>`, который создается только один раз при
> инициализации приложения.

## TODO

* [ ] Запустить `openapi-aggregator-by-tags` в кластере.
* [ ] Опубликовать OpenAPI как ConfigMap.
