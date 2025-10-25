# Рассчет капасити
## Посты
- capacity = traffic * 86400 * 365 = 35 kb/s * 86400 * 365 = 1 TB
- HDD: 
  - Disks_for_capacity = capacity / disk_capacity = 1
  - Disks_for_throughput = traffic_per_second / disk_throughput = 1
  - Disks_for_iops = iops / disk_iops = (RPS(создание поста) + RPS(загрузка ленты) + RPS(просмотр поста) + RPS(поиск)) / disk_iops = 6747 / 100 = 67
  - Disks = max(ceil(Disks_for_capacity), ceil(Disks_for_throughput), ceil(Disks_for_iops)) = 67
- SSD(sata):
  - Disks_for_capacity = capacity / disk_capacity = 1
  - Disks_for_throughput = traffic_per_second / disk_throughput = 1
  - Disks_for_iops = iops / disk_iops = (RPS(создание поста) + RPS(загрузка ленты) + RPS(просмотр поста) + RPS(поиск)) / disk_iops = 6747 / 1000 = 7
  - Disks = max(ceil(Disks_for_capacity), ceil(Disks_for_throughput), ceil(Disks_for_iops)) = 7
- SSD(nVME):
  - Disks_for_capacity = capacity / disk_capacity = 1
  - Disks_for_throughput = traffic_per_second / disk_throughput = 1
  - Disks_for_iops = iops / disk_iops = (RPS(создание поста) + RPS(загрузка ленты) + RPS(просмотр поста) + RPS(поиск)) / disk_iops = 6747 / 10000 = 1
  - Disks = max(ceil(Disks_for_capacity), ceil(Disks_for_throughput), ceil(Disks_for_iops)) = 1
## Фото (мета данные)
capacity = traffic * 86400 * 365 = 65 kb/s * 86400 * 365 = 2 TB
## Фото (сами файлы)
capacity = traffic * 86400 * 365 = 574 mb/s * 86400 * 365 = 17 263 TB
## Комментарии + реакции
capacity(комментарии + реакции) = traffic * 86400 * 365 = 2 mb/s * 86400 * 365 = 60 TB
## Подписки
capacity (подписки) = traffic * 86400 * 365 = 10 kb/s * 86400 * 365 = 300 GB
