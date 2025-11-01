@startuml
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Container.puml

AddElementTag("microService", $shape=EightSidedShape(), $bgColor="CornflowerBlue", $fontColor="white", $legendText="microservice")
AddElementTag("storage", $shape=RoundedBoxShape(), $bgColor="lightSkyBlue", $fontColor="white")

Person(user, "Путешественник", "Пользователь приложения")

System_Boundary(c, "Adventures") {
   Container(appAndroid, "Мобильное приложение", "Android", "Adventure App")
   Container(appIOS, "Мобильное приложение", "IOS", "Adventure App")
   Container(web, "Сайт", "Web", "adventure.com")
   Container(loadBalancer, "Load balancer", "LB", "Балансировщик нагрузки")

   Container(posts, "Posts Service", "Go", "Создание и просмотр постов", $tags = "microService")      
   ContainerDb(posts_db, "Posts Database", "PostgreSQL", "База данных для постов", $tags = "storage")      

   Container(photos, "Photos Service", "Go", "Загрузка и получение фото", $tags = "microService")      
   ContainerDb(photos_db, "Photos Database", "PostgreSQL", "База данных для мета данных фото", $tags = "storage")
   ContainerDb(photos_storage, "Photos S3", "S3", "Хранилище фото", $tags = "storage")      
   
   Container(reactions, "Reactions Service", "Go", "Создание оценок и комментариев", $tags = "microService")      
   ContainerDb(reactions_db, "Reactions Database", "PostgreSQL", "База данных для оценок и комментариев", $tags = "storage") 
   
   Container(subscriptions, "Subscriptions Service", "Go", "Подписки", $tags = "microService")      
   ContainerDb(users_db, "Users Database", "PostgreSQL", "База данных для пользователей и подписок", $tags = "storage")
}

Rel(appAndroid, loadBalancer, "", "")
Rel(appIOS, loadBalancer, "", "")
Rel(web, loadBalancer, "", "")

Rel(loadBalancer, posts, "", "HTTPS")
Rel(posts, posts_db, "", "")

Rel(loadBalancer, photos, "", "HTTPS")
Rel(photos, photos_db, "", "")
Rel(photos, photos_storage, "", "")

Rel(loadBalancer, reactions, "", "HTTPS")
Rel(reactions, reactions_db, "", "")

Rel(loadBalancer, subscriptions, "", "HTTPS")
Rel(subscriptions, users_db, "", "")


SHOW_LEGEND()
@enduml
