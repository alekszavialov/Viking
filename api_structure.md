# API Структура для компонентов и страниц

## Header (шапка)

### 1. apiRoute: /api/getCurrentCustomer
- **Метод:** GET
- **Описание:** Получение полной информации о текущем пользователе
- **Поля ответа:**
  - `id`: string
  - `name`: string
  - `surname`: string
  - `patronymic`: string
  - `phone`: string
  - `birthDate`: string
  - `gender`: string
  - `language`: string
  - `contacts`: объект
    - `phone`: string
    - `email`: string
  - `recipient`: объект или массив (основной получатель)
    - `name`: string
    - `phone`: string
  - `deliveryAddress`: [] address array
  - `hobbies`: массив строк ("Туризм", "Музыка", "Тренаж", "Рыбалка", "Садоводство", "Рыбки")
  - `pets`: массив строк ("Собака", "Кот", "Рыбы")
  - `additionalInfo`: объект
    - `isLegalEntity`: boolean (аккаунт юридического лица)
    - `noCall`: boolean (не звонить для онлайн-заказов)
  - `avatarUrl`: string
  - `bonusBalance`: number

### 2. apiRoute: /api/getCart
- **Метод:** GET
- **Описание:** Получение информации о корзине пользователя
- **Поля ответа:**
  - `count`: number (количество товаров в корзине)
  - `totalPrice`: number
  - `items`: массив объектов как в getProducts

### 4. apiRoute: /api/getCatalogCategories
- **Метод:** GET
- **Описание:** Получение списка категорий каталога вложенный список
- **Поля ответа:**
  - `categories`: массив объектов
    - `id`: string
    - `label`: string
    - `icon`: string (путь к иконке)
    - `children`: массив (подкатегории)

### 5. apiRoute: /api/getNotifications
- **Метод:** GET
- **Описание:** Получение уведомлений пользователя
- **Поля ответа:**
  - `notifications`: массив объектов
    - `id`: string
    - `type`: string (например, 'cart', 'order', 'promo')
    - `message`: string
    - `read`: boolean

### 6. apiRoute: /api/getPopularSearch
- **Метод:** GET
- **Описание:** Получение популярных поисковых запросов
- **Поля ответа:**
  - `searchPopular`: массив строк

### 7. apiRoute: /api/search
- **Метод:** GET
- **Описание:** Поиск по сайту (товары, категории, бренды)
- **Параметры запроса:**
  - `q`: string (поисковый запрос)
- **Поля ответа:**
  - `products`: массив объектов (результаты по товарам)
    - `id`: string
    - `name`: string
    - `image`: string
    - `price`: number
  - `categories`: массив объектов (результаты по категориям)
    - `id`: string
    - `label`: string
    - `icon`: string
  - `brands`: массив объектов (результаты по брендам)
    - `id`: string
    - `label`: string
    - `logo`: string

### 8. apiRoute: /api/getMetatags
- **Метод:** GET
- **Описание:** Получение SEO-метатегов для страницы
- **Параметры запроса:**
  - `page`: string (уникальный идентификатор или путь страницы, например, '/', '/category/123', '/product/456')
- **Поля ответа:**
  - `title`: string
  - `description`: string
  - `keywords`: string
  - `ogImage`: string (url для Open Graph)
  - `canonical`: string (канонический url)

## Аутентификация и логин

### 9. apiRoute: /api/auth/phone/start
- **Метод:** POST
- **Описание:** Запросить отправку SMS-кода на телефон
- **Параметры запроса:**
  - `phone`: string
- **Поля ответа:**
  - `success`: boolean
  - `sessionId`: string (идентификатор сессии для последующей проверки кода)
  - `error`: string (если есть)

### 10. apiRoute: /api/auth/phone/verify
- **Метод:** POST
- **Описание:** Проверить SMS-код и войти
- **Параметры запроса:**
  - `sessionId`: string
  - `code`: string
- **Поля ответа:**
  - `success`: boolean
  - `token`: string (JWT или иной токен авторизации)
  - `user`: объект (id, name, email, avatarUrl, bonusBalance)
  - `error`: string (если есть)

### 11. apiRoute: /api/auth/email
- **Метод:** POST
- **Описание:** Вход по email и паролю
- **Параметры запроса:**
  - `email`: string
  - `password`: string
- **Поля ответа:**
  - `success`: boolean
  - `token`: string
  - `user`: объект (id, name, email, avatarUrl, bonusBalance)
  - `error`: string (если есть)

### 12. apiRoute: /api/auth/social
- **Метод:** POST
- **Описание:** Вход через соцсети (Google, Apple, Facebook)
- **Параметры запроса:**
  - `provider`: string ('google' | 'apple' | 'facebook')
  - `accessToken`: string (токен соцсети)
- **Поля ответа:**
  - `success`: boolean
  - `token`: string
  - `user`: объект (id, name, email, avatarUrl, bonusBalance)
  - `error`: string (если есть)

### 13. apiRoute: /api/getBreadcrumbs
- **Метод:** GET
- **Описание:** Получение структуры хлебных крошек для текущей страницы
- **Параметры запроса:**
  - `path`: string (URL или идентификатор страницы)
- **Поля ответа:**
  - `breadcrumbs`: массив объектов
    - `label`: string
    - `url`: string

### 14. apiRoute: /api/getTags
- **Метод:** GET
- **Описание:** Получение списка тегов/популярных поисковых запросов
- **Параметры запроса:**
  - `path`: string (опционально) home/categories...
- **Поля ответа:**
  - `tags`: массив объектов
    - `id`: string
    - `label`: string

### 15. apiRoute: /api/getBanner
- **Метод:** GET
- **Описание:** Получение данных для баннера
- **Параметры запроса:**
  - `path`: string (опционально) home/categories...
- **Поля ответа:**
  - `banner`: объект
    - `id`: string
    - `image`: string (путь к картинке)
    - `title`: string
    - `subtitle`: string
    - `link`: string (url перехода)
    - `backgroundColor`: string (опционально)

### 16. apiRoute: /api/getProducts
- **Метод:** GET
- **Описание:** Получение списка товаров по фильтрам, категориям, поиску
- **Параметры запроса:**
  - `categoryId`: string (опционально)
  - `search`: string (опционально)
  - `tags`: массив строк (опционально)
  - `sort`: string (опционально, например, 'price', 'popularity')
  - `page`: number (опционально)
  - `limit`: number (опционально)
  - `minPrice`: number (опционально)
  - `maxPrice`: number (опционально)
  - `brands`: массив строк (опционально)
  - `sellers`: массив строк (опционально)
  - `attributes`: объект (ключ — id характеристики, значение — массив выбранных значений)
- **Поля ответа:**
  - `products`: массив объектов
    - `id`: string
    - `name`: string
    - `image`: string
    - `price`: number
    - `oldPrice`: number (опционально)
    - `rating`: number (опционально)
    - `reviewsCount`: number (опционально)
    - `labels`: массив строк (опционально)
    - `inStock`: boolean
  - `total`: number (общее количество товаров)
  - `filters`: объект (доступные фильтры для данной выборки)
    - `minPrice`: number
    - `maxPrice`: number
    - `brands`: массив объектов
      - `id`: string
      - `name`: string
    - `sellers`: массив объектов
      - `id`: string
      - `name`: string
    - `attributes`: массив объектов
      - `id`: string
      - `name`: string
      - `values`: массив строк

### 17. apiRoute: /api/getAllCategories
- **Метод:** GET
- **Описание:** Получение всех категорий с картинкой, id и названием
- **Поля ответа:**
  - `categories`: массив объектов
    - `id`: string
    - `name`: string
    - `image`: string (путь к картинке)

### 18. apiRoute: /api/getMainCategories
- **Метод:** GET
- **Описание:** Получение основных (топовых) категорий для страницы категории
- **Поля ответа:**
  - `mainCategories`: массив объектов
    - `id`: string
    - `name`: string
    - `image`: string (путь к картинке)
    - `description`: string (опционально)
    - `subcategories`: массив объектов (опционально)
      - `id`: string
      - `name`: string
      - `image`: string (путь к картинке, опционально)

### 19. apiRoute: /api/getPageDescription
- **Метод:** GET
- **Описание:** Получение текстового описания для страницы (например, описание категории, главной, FAQ и т.д.)
- **Параметры запроса:**
  - `path`: string (уникальный идентификатор или путь страницы)
- **Поля ответа:**
  - `description`: string

### 20. apiRoute: /api/addToCart
- **Метод:** POST
- **Описание:** Добавление товара или массива товаров в корзину
- **Параметры запроса:**
  - `productId`: string (если добавляется один товар)
  - `quantity`: number (по умолчанию 1, если добавляется один товар)
  - `products`: массив объектов (bulk-режим, опционально)
    - `productId`: string
    - `quantity`: number (по умолчанию 1)
- **Примечание:** Если передан массив `products`, то игнорируются параметры `productId` и `quantity` и добавляются все товары из массива.
- **Поля ответа:**
  - `success`: boolean
  - `cart`: объект (актуальное состояние корзины)
    - `count`: number
    - `totalPrice`: number
    - `items`: массив объектов
      - `id`: string
      - `name`: string
      - `quantity`: number
      - `price`: number
      - `image`: string
  - `error`: string (если есть)

### 21. apiRoute: /api/addToFavourites
- **Метод:** POST
- **Описание:** Добавление товара в избранное
- **Параметры запроса:**
  - `productId`: string
- **Поля ответа:**
  - `success`: boolean
  - `favourites`: массив объектов (актуальный список избранного)
    - `id`: string
    - `name`: string
    - `image`: string
  - `error`: string (если есть)

### 22. apiRoute: /api/getFavouritesLists
- **Метод:** GET
- **Описание:** Получение всех списков избранного пользователя
- **Параметры запроса:**
  - (авторизация по токену пользователя)
- **Поля ответа:**
  - `lists`: массив объектов
    - `id`: string
    - `name`: string
    - `isDefault`: boolean (основной список)
    - `productsCount`: number (количество товаров)
    - `products`: массив объектов (до 10 товаров для превью)
      - `id`: string
      - `name`: string
      - `image`: string

### 23. apiRoute: /api/createFavouritesList
- **Метод:** POST
- **Описание:** Создание нового списка избранного
- **Параметры запроса:**
  - `name`: string (название списка)
  - `isDefault`: boolean (сделать основным, опционально)
- **Поля ответа:**
  - `success`: boolean
  - `list`: объект созданного списка
    - `id`: string
    - `name`: string
    - `isDefault`: boolean
    - `productsCount`: number
    - `products`: массив объектов (обычно пустой)
  - `error`: string (если есть)

### 24. apiRoute: /api/removeFromFavourites
- **Метод:** POST
- **Описание:** Удаление товара из избранного
- **Параметры запроса:**
  - `productId`: string
- **Поля ответа:**
  - `success`: boolean
  - `favourites`: массив объектов (актуальный список избранного)
    - `id`: string
    - `name`: string
    - `image`: string
  - `error`: string (если есть)

### 25. apiRoute: /api/getBundleSlider
- **Метод:** GET
- **Описание:** Получение данных для слайдера "Вместе дешевле" (товары, которые можно купить комплектом дешевле)
- **Параметры запроса:**
  - `productId`: string (основной товар)
- **Поля ответа:**
  - `bundles`: массив объектов
    - `id`: string (id комплекта)
    - `products`: массив объектов
      - `id`: string
      - `name`: string
      - `image`: string
      - `price`: number
    - `bundlePrice`: number (цена за комплект)
    - `oldPrice`: number (общая цена без скидки)
    - `economy`: number (сумма экономии)

### 26. apiRoute: /api/getSubcategoryFilters
- **Метод:** GET
- **Описание:** Получение доступных фильтров для страницы сабкатегории
- **Параметры запроса:**
  - `subcategoryId`: string
- **Поля ответа:**
  - `minPrice`: number
  - `maxPrice`: number
  - `brands`: массив объектов
    - `id`: string
    - `name`: string
  - `sellers`: массив объектов
    - `id`: string
    - `name`: string
  - `attributes`: массив объектов (характеристики)
    - `id`: string
    - `name`: string
    - `values`: массив строк

### 27. apiRoute: /api/getPopularReviews
- **Метод:** GET
- **Описание:** Получение популярных отзывов в категории
- **Параметры запроса:**
  - `categoryId`: string (идентификатор категории)
  - `limit`: number (опционально)
- **Поля ответа:**
  - `reviews`: массив объектов
    - `id`: string
    - `product`: объект
      - `id`: string
      - `name`: string
      - `image`: string
      - `rating`: number
      - `reviewsCount`: number
    - `user`: объект
      - `name`: string
      - `avatar`: string (опционально)
    - `date`: string (ISO)
    - `text`: string
    - `rating`: number
    - `likes`: number (опционально)
    - `moreUrl`: string (ссылка на все отзывы о товаре)

### 28. apiRoute: /api/getProductDetails
- **Метод:** GET
- **Описание:** Получение полной информации о товаре
- **Параметры запроса:**
  - `productId`: string
- **Поля ответа:**
  - `id`: string
  - `name`: string
  - `code`: string (артикул)
  - `images`: массив строк (галерея)
  - `mainImage`: string
  - `price`: number
  - `oldPrice`: number (опционально)
  - `inStock`: boolean
  - `labels`: массив строк (например, "Новинка", "Хит")
  - `rating`: number
  - `reviewsCount`: number
  - `shortDescription`: string
  - `description`: string
  - `characteristics`: массив объектов
    - `name`: string
    - `value`: string
  - `sellers`: массив объектов
    - `id`: string
    - `name`: string
    - `price`: number
    - `delivery`: string
    - `isMain`: boolean
  - `services`: массив объектов (дополнительные услуги)
    - `id`: string
    - `name`: string
    - `price`: number
  - `faq`: массив объектов (вопросы и ответы по товару)
    - `question`: string
    - `answer`: string
  - `reviews`: массив объектов (короткие отзывы, для блока на странице)
    - `id`: string
    - `user`: объект
      - `name`: string
      - `avatar`: string (опционально)
    - `date`: string (ISO)
    - `text`: string
    - `rating`: number
    - `likes`: number (опционально)
    - `dislikes`: number (опционально)
    - `images`: массив строк (опционально)
    - `pros`: массив строк (опционально)
    - `cons`: массив строк (опционально)
    - `repliesCount`: number (опционально)
  - `video`: массив объектов (опционально)
    - `id`: string
    - `url`: string
    - `title`: string

### 29. apiRoute: /api/getStores
- **Метод:** GET
- **Описание:** Получение списка магазинов для страницы Контакты
- **Параметры запроса:**
  - `city`: string (опционально)
- **Поля ответа:**
  - `stores`: массив объектов
    - `id`: string
    - `name`: string
    - `address`: string
    - `lat`: number
    - `lng`: number
    - `workTime`: string
    - `phone`: string (опционально)
    - `isService`: boolean (есть ли сервисная зона)
    - `isPickup`: boolean (точка самовывоза)

### 30. apiRoute: /api/searchStores
- **Метод:** GET
- **Описание:** Поиск магазинов по адресу или улице
- **Параметры запроса:**
  - `query`: string (адрес или часть адреса)
- **Поля ответа:**
  - `stores`: массив объектов (см. структуру /api/getStores)

### 31. apiRoute: /api/getOrders
- **Метод:** GET
- **Описание:** Получение списка заказов пользователя
- **Параметры запроса:**
  - `search`: string (опционально, поиск по номеру заказа или товару)
  - `period`: string (опционально, фильтр по периоду: 'all', 'year', 'month', 'week')
  - `page`: number (опционально)
  - `limit`: number (опционально)
- **Поля ответа:**
  - `orders`: массив объектов
    - `id`: string (номер заказа)
    - `date`: string (ISO)
    - `status`: string (например, 'Виконано', 'В обработке', 'Отменено')
    - `seller`: объект
      - `id`: string
      - `name`: string
      - `phone`: string
    - `delivery`: объект
      - `type`: string (например, 'Самовывоз', 'Курьер')
      - `service`: string (например, 'Новая Почта')
      - `ttn`: string (номер ТТН, если есть)
      - `address`: string
      - `workTime`: string
    - `recipient`: объект
      - `name`: string
      - `phone`: string
      - `email`: string
    - `payment`: объект
      - `type`: string (например, 'Оплата при получении')
      - `commission`: number (опционально)
    - `items`: массив объектов
      - `id`: string
      - `name`: string
      - `image`: string
      - `quantity`: number
      - `price`: number
      - `oldPrice`: number (опционально)
    - `total`: number
    - `detailsUrl`: string (ссылка на детали заказа)
  - `total`: number (общее количество заказов)

### 32. apiRoute: /api/getOrderGuarantees
- **Метод:** GET
- **Описание:** Получение информации о гарантиях и возвратах по заказам пользователя
- **Параметры запроса:**
  - `orderId`: string (опционально, если нужно по конкретному заказу)
- **Поля ответа:**
  - `guarantees`: массив объектов
    - `id`: string
    - `orderId`: string
    - `productId`: string
    - `productName`: string
    - `status`: string (например, 'Активна', 'Истекла', 'В процессе возврата')
    - `startDate`: string (ISO)
    - `endDate`: string (ISO)
    - `detailsUrl`: string (ссылка на детали гарантии/возврата)

### 33. apiRoute: /api/startCheckout
- **Метод:** POST
- **Описание:** Начало процесса оформления заказа, проверка товаров на наличие (чекаут) с возможностью запроса печати документов
- **Параметры запроса:**
  - `printDocuments`: boolean (опционально, требуется ли печать документов)
- **Поля ответа:**
  - `success`: boolean
  - `checkoutId`: string (идентификатор чекаута)
  - `error`: string (если есть)

### 34. apiRoute: /api/getFavouritesDetails
- **Метод:** GET
- **Описание:** Получение детальной информации о продуктах, добавленных в избранное (по id списка или основному списку)
- **Параметры запроса:**
  - `listId`: string (опционально, если не передан — основной список)
  - (авторизация по токену пользователя)
- **Поля ответа:**
  - `products`: массив объектов (структура как в getProducts)
    - `id`: string
    - `name`: string
    - `images`: массив строк (пути к картинкам)
    - `price`: number
    - `oldPrice`: number (если есть)
    - `inStock`: boolean
    - `rating`: number
    - `reviewsCount`: number
    - `tags`: массив строк
    - `description`: string
    - `characteristics`: массив объектов
      - `name`: string
      - `value`: string
    - ... (дополнительные поля из getProducts)
  - `listId`: string
  - `listName`: string
  - `isDefault`: boolean
  - `productsCount`: number
  - `error`: string (если есть)

### 35. apiRoute: /api/getParcels
- **Метод:** GET
- **Описание:** Получение списка всех посылок пользователя
- **Параметры запроса:**
  - (авторизация по токену пользователя)
- **Поля ответа:**
  - `parcels`: массив объектов
    - `id`: string (номер посылки)
    - `createdAt`: string (дата создания)
    - `status`: string (статус: "выдано", "в пути", и т.д.)
    - `orderNumber`: string
    - `recipient`: объект
      - `name`: string
      - `phone`: string
      - `address`: string
    - `sender`: объект
      - `name`: string
      - `phone`: string
      - `address`: string
    - `deliveryDate`: string (плановая дата доставки)
    - `description`: string (описание)
    - `params`: string (параметры: размеры, вес)

### 36. apiRoute: /api/findParcel
- **Метод:** GET
- **Описание:** Поиск и получение информации о посылке по номеру
- **Параметры запроса:**
  - `parcelId`: string (номер посылки)
- **Поля ответа:**
  - `parcel`: объект (структура как в getParcels)
  - `error`: string (если есть)

### 39. apiRoute: /api/getBonusAccount
- **Метод:** GET
- **Описание:** Получение текущего состояния бонусного счета пользователя
- **Параметры запроса:**
  - (авторизация по токену пользователя)
- **Поля ответа:**
  - `total`: number (всего бонусов)
  - `available`: number (доступно для использования)
  - `pending`: number (ожидают активации)
  - `giftBonuses`: number (подарочные бонусы)
  - `actions`: массив объектов (акции/ограничения)
    - `title`: string
    - `expireAt`: string (дата сгорания)
    - `amount`: number

### 40. apiRoute: /api/getBonusHistory
- **Метод:** GET
- **Описание:** Получение истории начисления и списания бонусов
- **Параметры запроса:**
  - `offset`: number (опционально, для пагинации)
  - `limit`: number (опционально, по умолчанию 10)
  - (авторизация по токену пользователя)
- **Поля ответа:**
  - `history`: массив объектов
    - `date`: string (дата операции)
    - `type`: string ("начисление" или "списание")
    - `title`: string (описание)
    - `amountAdded`: number (начислено)
    - `amountUsed`: number (списано)
    - `activatedAt`: string (дата активации)
    - `expireAt`: string (дата сгорания)
  - `hasMore`: boolean (есть ли еще записи)

### 41. apiRoute: /api/getRecommendedProducts
- **Метод:** GET
- **Описание:** Получение рекомендованных товаров для пользователя или страницы
- **Параметры запроса:**
  - `limit`: number (опционально, по умолчанию 10)
  - `categoryId`: string (опционально, для рекомендаций по категории)
  - (авторизация по токену пользователя, если требуется персонализация)
- **Поля ответа:**
  - `products`: массив объектов (структура как in getProducts)
    - `id`: string
    - `name`: string
    - `image`: string
    - `price`: number
    - `oldPrice`: number (опционально)
    - `rating`: number (опционально)
    - `reviewsCount`: number (опционально)
    - `labels`: массив строк (опционально)
    - `inStock`: boolean
  - `total`: number (общее количество товаров)

### 42. apiRoute: /api/getCheckoutData
- **Метод:** GET
- **Описание:** Получение данных чекаута по checkoutId
- **Параметры запроса:**
  - `checkoutId`: string
  - (авторизация по токену пользователя)
- **Поля ответа:**
  - `products`: массив объектов (структура как в getProducts)
    - `id`: string
    - `name`: string
    - `image`: string
    - `price`: number
    - `oldPrice`: number (опционально)
    - `rating`: number (опционально)
    - `reviewsCount`: number (опционально)
    - `labels`: массив строк (опционально)
    - `inStock`: boolean
  - `address`: string (сохранённый адрес доставки)
  - `deliveryType`: string (тип доставки)
  - `paymentType`: string (тип оплаты)
  - `branchNumber`: string (номер отделения, если есть)
  - `recipient`: объект
    - `name`: string
    - `phone`: string
    - `email`: string

### 43. apiRoute: /api/getDeliveryAndPaymentTypes
- **Метод:** GET
- **Описание:** Получение возможных типов доставки и оплаты
- **Параметры запроса:**
  - (авторизация по токену пользователя, если требуется)
- **Поля ответа:**
  - `deliveryTypes`: массив объектов
    - `id`: string
    - `label`: string
    - `description`: string (опционально)
  - `paymentTypes`: массив объектов
    - `id`: string
    - `label`: string
    - `description`: string (опционально)

### 44. apiRoute: /api/submitCheckout
- **Метод:** POST
- **Описание:** Оформление заказа (сабмит чекаута)
- **Параметры запроса:**
  - `checkoutId`: string
  - `address`: string
  - `deliveryType`: string
  - `paymentType`: string
  - `branchNumber`: string (опционально)
  - `recipient`: объект
    - `name`: string
    - `phone`: string
    - `email`: string
- **Поля ответа:**
  - `success`: boolean
  - `orderId`: string (номер созданного заказа)
  - `error`: string (если есть)

### 45. apiRoute: /api/editCurrentCustomer
- **Метод:** POST
- **Описание:** Редактирование личных данных пользователя
- **Параметры запроса:**
  - Любое из полей, возвращаемых в getCurrentCustomer (см. выше)
- **Поля ответа:**
  - `success`: boolean
  - `customer`: объект (актуальные данные пользователя)
  - `error`: string (если есть)

