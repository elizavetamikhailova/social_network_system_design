@startuml
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Container.puml

AddElementTag("microService", $shape=EightSidedShape(), $bgColor="CornflowerBlue", $fontColor="white", $legendText="microservice")
AddElementTag("storage", $shape=RoundedBoxShape(), $bgColor="lightSkyBlue", $fontColor="white")

Person(user, "Путешественник", "Пользователь приложения")

System_Boundary(a, "Adventures") {
   Container(appAndroid, "Мобильное приложение", "Android", "Adventure App")
   Container(appIOS, "Мобильное приложение", "IOS", "Adventure App")
   Container(web, "Сайт", "Web", "adventure.com")
   Container(loadBalancer, "Load balancer", "LB", "Балансировщик нагрузки")

   Container(posts, "Posts Service", "Go", "Создание и просмотр постов", $tags = "microService")      
   ContainerDb(posts_db, "Posts Database", "PostgreSQL", "База данных для постов", $tags = "storage")

System_Boundary(f, "Feed") {
   Container(feed, "Feed Service", "Go", "Лента постов", $tags = "microService")      
   ContainerDb(feed_cache, "Feed Cache", "Redis", "Преподготовленная лента постов", $tags = "storage")
   ContainerQueue(feed_queue, "Feed Queue", "Очередь, куда сервисы пишут события о загрузке фото") 
}          

System_Boundary(p, "Photos") {
   Container(photos, "Photos Service", "Go", "Загрузка и получение фото", $tags = "microService") 
   Container(photos_worker, "Photos Worker", "Go", "Периодическое удаление фото, которые не были привязаны к посту", $tags = "microService")           
   ContainerDb(photos_db, "Photos Database", "PostgreSQL", "База данных для мета данных фото", $tags = "storage")
   ContainerDb(photos_storage, "Photos S3", "S3", "Хранилище фото", $tags = "storage")
   ContainerQueue(photos_queue, "Photos Queue", "Очередь, куда сервис постов пишет события о загрузке фото") 
}     
   
   Container(reactions, "Reactions Service", "Go", "Создание оценок и комментариев", $tags = "microService")      
   ContainerDb(reactions_db, "Reactions Database", "PostgreSQL", "База данных для оценок и комментариев", $tags = "storage") 
   
   Container(subscriptions, "Subscriptions Service", "Go", "Подписки", $tags = "microService")      
   ContainerDb(users_db, "Users Database", "PostgreSQL", "База данных для пользователей и подписок", $tags = "storage")
}

Rel(user, appAndroid, "", "")
Rel(user, appIOS, "", "")
Rel(user, web, "", "")

Rel(appAndroid, loadBalancer, "", "")
Rel(appIOS, loadBalancer, "", "")
Rel(web, loadBalancer, "", "")

Rel(loadBalancer, posts, "Сохранение и получение постов", "HTTPS")
Rel(posts, posts_db, "Сохранение и получение постов", "")

Rel(loadBalancer, feed, "Получение ленты постов", "HTTPS")
Rel(feed, feed_cache, "Получение и сборка ленты постов", "")

Rel(loadBalancer, photos, "", "HTTPS")
Rel(photos, photos_db, "", "")
Rel(photos, photos_storage, "", "")
Rel(photos_worker, photos_db, "", "")
Rel(photos_worker, photos_storage, "", "")
Rel(photos_queue, photos, "События о загрузке постов", "")
Rel(posts, photos_queue, "События о загрузке постов", "")

Rel(posts, feed_queue, "События о загрузке постов", "")
Rel(feed_queue, feed, "События о загрузке постов", "")
Rel(feed, reactions, "Получение реакций", "gRPS")
Rel(subscriptions, feed_queue, "События о подписках и отписках", "")


Rel(loadBalancer, reactions, "", "HTTPS")
Rel(reactions, reactions_db, "", "")

Rel(loadBalancer, subscriptions, "", "HTTPS")
Rel(subscriptions, users_db, "", "")


SHOW_LEGEND()
@enduml
