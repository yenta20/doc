# 6. general architecture

Date: 2020-09-13

## Status

Discussion

## Context

Recon backend architecture approach

## Decision

1) Реализиация ведётся на основе подхода "распределённый монолит". То есть вместо независмых микросервисов используется набор сервисов, которые могут разделять состояние через SQL/NoSQL базы данных.

2) Планируется три сервиса

- Recon API: REST api доступный клиенту (in progress)
- Dispatcher: асинхронная обработка событий, формирование фида, отправка пуш нотификаций (development starting)
- ContactMatcher: анализ контактов юзера для поиска совпадений (in desing)

3) SQL DB - Cloud Postgres

4) NoSQL DB - Cloud Datastore navive mode

4) EventBus - Pub/Sub

5) Для разработки используется monorepository что бы упростить менеджмент зависимостей

### Architecture diagram

![](img/recon_cloud_architecture.png =500x389)


## Consequences

What becomes easier or more difficult to do and any risks introduced by the change that will need to be mitigated.
