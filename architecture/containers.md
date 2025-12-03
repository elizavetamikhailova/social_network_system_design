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
   Container(posts, "Posts", "Go", "Создание и просмотр постов", $tags = "microService")      
}

System_Boundary(f, "Feed") {
   Container(feed, "Feed", "Go", "Лента постов", $tags = "microService")
}          

System_Boundary(p, "Photos") {
   Container(photos, "Photos", "Go", "Загрузка и получение фото", $tags = "microService") 
}     
   
System_Boundary(r, "Reactions") {
   Container(reactions, "Reactions", "Go", "Создание оценок и комментариев", $tags = "microService")      
}

System_Boundary(u, "Users") {   
   Container(subscriptions, "Subscriptions", "Go", "Подписки", $tags = "microService")      
}
}

Rel(user, app_android, "", "")
Rel(user, app_ios, "", "")
Rel(user, web, "", "")

Rel(app_android, load_balancer, "", "")
Rel(app_ios, load_balancer, "", "")
Rel(web, load_balancer, "", "")

Rel(load_balancer, posts, "Сохранение и получение постов", "HTTPS")

Rel(load_balancer, feed, "Получение ленты постов", "HTTPS")

Rel(load_balancer, photos, "Загрузка фото", "HTTPS")
Rel(posts, photos, "События о загрузке постов", "")

Rel(posts, feed, "События о загрузке постов", "")
Rel(reactions, feed, "События о новых реакциях", "gRPS")
Rel(subscriptions, feed, "События о подписках и отписках", "")


Rel(load_balancer, reactions, "Отправка и получение реакций и комментариев", "HTTPS")

Rel(load_balancer, subscriptions, "Подписки и отписки", "HTTPS")


SHOW_LEGEND()
@enduml
