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
   ContainerQueue(feed_queue, "Feed Queue", "Очередь, куда сервисы пишут события о загрузке фото") 
}          

System_Boundary(p, "Photos") {
   Container(posts_change_data_capture, "Photos CDC", "CDC для БД постов") 
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

Rel(posts_db, posts_change_data_capture, "События об изменениях в БД постов", "")

Rel(posts, feed_queue, "События о загрузке постов", "")


SHOW_LEGEND()
@enduml
