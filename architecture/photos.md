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
   ContainerDb(posts_db, "Posts Database", "PostgreSQL", "База данных для постов", $tags = "storage")
}
       

System_Boundary(p, "Photos") {
   Container(photos, "Photos Service", "Go", "Загрузка и получение фото", $tags = "microService") 
   ContainerDb(photos_db, "Photos Database", "PostgreSQL", "База данных для мета данных фото", $tags = "storage")
   ContainerDb(photos_storage, "Photos Blob Storage", "Ceph", "Хранилище фото", $tags = "storage")
   Container(posts_change_data_capture, "Photos CDC", "CDC для БД постов") 
}     
   
}

Rel(user, app_android, "", "")
Rel(user, app_ios, "", "")
Rel(user, web, "", "")

Rel(app_android, load_balancer, "", "")
Rel(app_ios, load_balancer, "", "")
Rel(web, load_balancer, "", "")

Rel(load_balancer, photos, "Загрузка фото", "HTTPS")
Rel(photos, photos_db, "", "")
Rel(photos, photos_storage, "", "")
Rel(posts_change_data_capture, photos, "События о загрузке постов", "")
Rel(posts_db, posts_change_data_capture, "События об изменениях в БД постов", "")


SHOW_LEGEND()
@enduml
