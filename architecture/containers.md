@startuml
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Container.puml

AddElementTag("microService", $shape=EightSidedShape(), $bgColor="CornflowerBlue", $fontColor="white", $legendText="microservice")
AddElementTag("storage", $shape=RoundedBoxShape(), $bgColor="lightSkyBlue", $fontColor="white")

Person(user, "Путешественник", "Пользователь приложения")

System_Boundary(a, "Adventures") {
   Container(app_android, "Мобильное приложение", "Android", "Adventure App")
   Container(app_ios, "Мобильное приложение", "IOS", "Adventure App")
   Container(web, "Сайт", "Web", "adventure.com")
   Container(load_balancer, "Load balancer", "LB", "Балансировщик нагрузки")

System_Boundary(ps, "Posts") {
   Container(posts, "Posts Service", "Go", "Создание и просмотр постов", $tags = "microService")      
   ContainerDb(posts_db, "Posts Database", "PostgreSQL", "База данных для постов", $tags = "storage")
}

System_Boundary(f, "Feed") {
   Container(feed, "Feed Service", "Go", "Лента постов", $tags = "microService")
   ContainerDb(feed_cache, "Feed Cache", "Redis", "Преподготовленная лента постов", $tags = "storage")
   ContainerQueue(feed_queue, "Feed Queue", "Очередь, куда сервисы пишут события о загрузке фото") 
}          

System_Boundary(p, "Photos") {
   Container(photos, "Photos Service", "Go", "Загрузка и получение фото", $tags = "microService") 
   ContainerDb(photos_db, "Photos Database", "PostgreSQL", "База данных для мета данных фото", $tags = "storage")
   ContainerDb(photos_storage, "Photos Blob Storage", "Ceph", "Хранилище фото", $tags = "storage")
   Container(posts_change_data_capture, "Photos CDC", "CDC для БД постов") 
}     
   
System_Boundary(r, "Reactions") {
   Container(reactions, "Reactions Service", "Go", "Создание оценок и комментариев", $tags = "microService")      
   ContainerDb(reactions_db, "Reactions Database", "PostgreSQL", "База данных для оценок и комментариев", $tags = "storage") 
}

System_Boundary(u, "Users") {   
   Container(subscriptions, "Subscriptions Service", "Go", "Подписки", $tags = "microService")      
   ContainerDb(users_db, "Users Database", "PostgreSQL", "База данных для пользователей и подписок", $tags = "storage")
}
}

Rel(user, app_android, "", "")
Rel(user, app_ios, "", "")
Rel(user, web, "", "")

Rel(app_android, load_balancer, "", "")
Rel(app_ios, load_balancer, "", "")
Rel(web, load_balancer, "", "")

Rel(load_balancer, posts, "Сохранение и получение постов", "HTTPS")
Rel(posts, posts_db, "Сохранение и получение постов", "")

Rel(load_balancer, feed, "Получение ленты постов", "HTTPS")
Rel(feed, feed_cache, "Получение и сборка ленты постов", "")
Rel(feed, feed_queue, "Получение событий о постах", "")


Rel(load_balancer, photos, "Загрузка фото", "HTTPS")
Rel(photos, photos_db, "", "")
Rel(photos, photos_storage, "", "")
Rel(posts_change_data_capture, photos, "События о загрузке постов", "")
Rel(posts_db, posts_change_data_capture, "События об изменениях в БД постов", "")

Rel(posts, feed_queue, "События о загрузке постов", "")
Rel(reactions, feed_queue, "События о новых реакциях", "gRPS")
Rel(subscriptions, feed_queue, "События о подписках и отписках", "")


Rel(load_balancer, reactions, "Отправка и получение реакций и комментариев", "HTTPS")
Rel(reactions, reactions_db, "", "")

Rel(load_balancer, subscriptions, "Подписки и отписки", "HTTPS")
Rel(subscriptions, users_db, "", "")


SHOW_LEGEND()
@enduml
