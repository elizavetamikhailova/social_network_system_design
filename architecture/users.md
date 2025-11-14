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

System_Boundary(f, "Feed") {
   ContainerQueue(feed_queue, "Feed Queue", "Очередь, куда сервисы пишут события о загрузке фото") 
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

Rel(subscriptions, feed_queue, "События о подписках и отписках", "")

Rel(load_balancer, subscriptions, "Подписки и отписки", "HTTPS")
Rel(subscriptions, users_db, "", "")


SHOW_LEGEND()
@enduml
