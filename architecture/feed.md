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
}

System_Boundary(f, "Feed") {
   Container(feed, "Feed Service", "Go", "Лента постов", $tags = "microService")
   ContainerDb(feed_cache, "Feed Cache", "Redis", "Преподготовленная лента постов", $tags = "storage")
   ContainerQueue(feed_queue, "Feed Queue", "Очередь, куда сервисы пишут события о загрузке фото") 
}            
   
System_Boundary(r, "Reactions") {
   Container(reactions, "Reactions Service", "Go", "Создание оценок и комментариев", $tags = "microService")      
}

System_Boundary(u, "Users") {   
   Container(subscriptions, "Subscriptions Service", "Go", "Подписки", $tags = "microService")      
}
}

Rel(user, app_android, "", "")
Rel(user, app_ios, "", "")
Rel(user, web, "", "")

Rel(app_android, load_balancer, "", "")
Rel(app_ios, load_balancer, "", "")
Rel(web, load_balancer, "", "")

Rel(load_balancer, feed, "Получение ленты постов", "HTTPS")
Rel(feed, feed_cache, "Получение и сборка ленты постов", "")
Rel(feed, feed_queue, "Получение событий о постах", "")

Rel(posts, feed_queue, "События о загрузке постов", "")
Rel(reactions, feed_queue, "События о новых реакциях", "gRPS")
Rel(subscriptions, feed_queue, "События о подписках и отписках", "")


SHOW_LEGEND()
@enduml
