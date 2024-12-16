# Задание № 1

Разделить проект Mesto на несколько микрофронтендов. 

## Функциональность Mesto:

- создать/редактировать профиль
- загрузить фото
- удалить фото
- собирать и учитывать лайки

### Компоненты

- Пользователи
- Профили (имя, инфо, аватар)
- Places( name, link)
- Фотографии (cards)
- Оценки (лайки)

### Маршрутизация

- ProtectedRoute
- signup
- signin

### Потоки 

Пользователи:

- Регистрируются (onRegister, onLogin)
- Ведут профиль (onEditProfile, onEditAvatar)
- Загружают, удаляют, оценивают фото (onCardClick, onAddPlace, onCardDelete, onCardLike)

### Логические блоки

 - аутентификация и авторизация
 - профиль пользователя
 - место (фото, наименование, ссылка)

### Выбор подхода Single SPA(SSPA) или Webpack Module Federation (WMF)

SSPA позволяет работать с разными фреймворками. 
WMF - заводить общие модули, которые микрофронты могут использовать совместно.
У нас все исходники на React, есть общие модули, поэтому более уместен выбор WMF.

### Выбор стратегии управления состоянием

	Нужна независимая связь с бэкендом.
	У приложения простые потребности в общении, они сводятся к получению данных или отправке форм.
	Важны производительность и независимая масштабируемость.

Поэтому используем коммуникации через API

## Обоснование принятого решения

Выбранный подход позволяет использовать микрофронтенды как независимые приложения, которые монтируются в общую разметку (корневое приложение, host).
Кроме того, остается возможность использовать общие модули исходного приложения и не менять фреймворк.
Структура исходного приложения имела одну главную страницу, которую удобно использовать в качестве корневой для объединения 
микрофронтендов профиля, места и авторизации.

## Исходная структура проекта

Содержимое папки architecture-sprint-1\frontend\src

  blocks            - css-файлы проекта, разбитые по частям страницы и частично по сущностям предметной области:
    auth-form
    card
    content
    footer
    header
    login
    page
    places
    popup
    profile

  components        - js-код в такой же разбивке
    App.js               - обработчики событий приложения
    AddPlacePopup.js     - окно добавления места
    Card.js              - снятие, установка лайков на фото, удаление фото
    EditAvatarPopup.js   - редактирование фото для профиля пользователя
    EditProfilePopup.js  - редактирование профиля пользователя
    Footer.js            - отрисовка подвала главной страницы
    Header.js            - отрисовка шапки главной страницы 
    ImagePopup.js        - окно описания фото
    InfoTooltip.js       - окно результата регистрации
    Login.js             - форма входа
    Main.js              - каркас главной страницы
    PopupWithForm.js     - окно подтверждения действия "Сохранить"
    ProtectedRoute.js    - аутентификация
    Register.js          - регистрация пользователя

  contexts
		CurrentUserContext.js - js-код объекта контекста CurrentUserContext экспортируется из отдельного файла директории contexts
  images            - иконки для оформления в svg, 3 фото и аватар (?)
  utils
    api.js          - getCardList(), addCard(), removeCard(), getUserInfo(), setUserInfo(), setUserAvatar(), changeLikeCardStatus()
    auth.js         - register(email, password), login, checkToken
  vendor            - css-файлы шрифтов и общий для проекта normalize.css

## Предлагаемая структура проекта 

В папке architecture-sprint-1\frontend\microfrontend созданы папки:
	\auth
		\node_modules                 // Зависимости
		\src
			\components
				Login.js                  // Компонент входа пользователя
				Register.js               // Компонент регистрации пользователя
			\styles
				auth-form                 // Стили окна авторизации
				login                     // Стили для компонента входа
			\utils
				App.js                    // Обработчики событий микрофронтенда
				auth.js                   // register(email, password), login, checkToken
        InfoTooltip.js            // Окно результата регистрации
			index.js                    // Точка входа микрофронтенда
		package.json                  // Зависимости и скрипты микрофронтенда
		webpack.config.js             // Настройка микрофронтенда
  \host
		\node_modules                 // Зависимости
		\src
			\components
				Footer.js                 // отрисовка подвала главной страницы
				Header.js                 // отрисовка шапки главной страницы 
        Main.js                   // каркас главной страницы
				ProtectedRoute.js         // аутентификация
				CurrentUserContext.js     // контекст пользователя
			\images                     // изображения дизайна элементов страницы
      \styles                     // стили компонентов микрофронтенда
				\content
				\footer 
				\header 
        \page
			\utils
				api.js                    // Обработчики событий приложения
				PopupWithForm.js          // окно подтверждения действия "Сохранить"
			App.jsx                     // Загрузчик микрофронтенда
			index.css                   // Стили главной страницы
			index.html                  // Главная страница
			index.js                    // Точка входа микрофронтенда
      Main.js                     // Компоновцик главной страницы
		package.json                  // Зависимости и скрипты микрофронтенда
		webpack.config.js             // Настройка загрузки микрофронтендов
  \place
    \node_modules                 // Зависимости
		\src
      \components
        AddPlacePopup.js          // окно добавления места
		    Card.js                   // снятие, установка лайков на фото, удаление фото
		    ImagePopup.js             // окно описания фото
      \styles                     // стили компонентов микрофронтенда
				\card
        \places
        \vendor
			\utils
				PopupWithForm.js          // окно подтверждения действия "Сохранить"
		  index.js                    // Точка входа микрофронтенда
		package.json                  // Зависимости и скрипты микрофронтенда
    webpack.config.js             // Настройка микрофронтенда
  \profile
		\node_modules                 // Зависимости
		\src
      \components
				EditAvatarPopup.js        // редактирование фото для профиля пользователя
				EditProfilePopup.js       // редактирование профиля пользователя
			\styles                     // стили компонентов микрофронтенда
				\popup
				\profile
			\utils
				api.js                    // Обработчики событий микрофронтенда
				PopupWithForm.js          // окно подтверждения действия "Сохранить"
		  index.js                    // Точка входа микрофронтенда
    package.json                  // Зависимости и скрипты микрофронтенда
    webpack.config.js             // Настройка микрофронтенда
