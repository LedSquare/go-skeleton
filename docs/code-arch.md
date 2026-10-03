# Структура кода
- cmd/ - Точка входа (http, queue, schedule и так далее)
	- http/
		- main.go - Воркер приложеия
	- queue/
		- main.go - Воркер очереди
	- migration/
		- main.go - Воркер запуска миграций
- docs/ - Документация. Произвольно
- internal/ - private
	- app/ 
		- app.go - Структура App, New, Close, геттеры
		- infra.go - NewLogger, NewDB, NewRedis, NewTracer
		- server.go - RunHTTP, graceful shutdown
		- middleware.go -  Общие middleware (recover, request-id, logging)
		- router.go - Каркас роутера, health-check, регистрация
	- config/
		- config.go
	- di/
		- di.go - Контейнер для загрузки зависимостей доменов
	- domains/ - Бизнес домены
		- user/ - Какой либо домен
			- handler/
			- usecase/
			- repository/
			- model/
				- user.go - Основная сущность
				- order_provider.go - Интерфейс для запроса данных из другого микросервиса или домена.
			- config.go - Конфигурация домена
	- config/
		- config.go - Структуры
		- load.go - Загрузка конфига
		- validate.go
- pgk - Публичный код / собственные библиотеки
- database/
	- migrations/
		- user/


# Правила взаимодействия между доменами
1. Интерфейс определяется в модели
Контракт:
- domain/user/model/
	- user.go - Сама сущность
	- order_provider.go - Интерфейс для запроса данных из домена заказов
Реализация:
- domain/user/client/
	- order/
		- order_inprocess.go - Запращивает данные из соседнего модуля(если монолит)
		- order_http.go - Делает запрос в другой сервис по http
		- order_grpc.go - Делает запрос в другой сервис по gRPC

