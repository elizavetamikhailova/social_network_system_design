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
IOPS = (RPS(создание поста) + RPS(загрузка ленты) + RPS(просмотр поста) + RPS(поиск)) * 5 = 33 735
traffic = 33 735 * 334 b = 10 mb/s
- HDD: 
  - Disks_for_capacity = capacity / disk_capacity = 1
  - Disks_for_throughput = traffic_per_second / disk_throughput = 1
  - Disks_for_iops = iops / disk_iops = 33 735 / 100 = 337
  - Disks = max(ceil(Disks_for_capacity), ceil(Disks_for_throughput), ceil(Disks_for_iops)) = 337
- SSD(sata):
  - Disks_for_capacity = capacity / disk_capacity = 1
  - Disks_for_throughput = traffic_per_second / disk_throughput = 1
  - Disks_for_iops = iops / disk_iops = 33 735 / 1000 = 34
  - Disks = max(ceil(Disks_for_capacity), ceil(Disks_for_throughput), ceil(Disks_for_iops)) = 34
- SSD(nVME):
  - Disks_for_capacity = capacity / disk_capacity = 1
  - Disks_for_throughput = traffic_per_second / disk_throughput = 1
  - Disks_for_iops = iops / disk_iops =  33 735 / 10000 = 4
  - Disks = max(ceil(Disks_for_capacity), ceil(Disks_for_throughput), ceil(Disks_for_iops)) = 4
## Фото (сами файлы)
capacity = traffic * 86400 * 365 = 574 mb/s * 86400 * 365 = 17 263 TB
IOPS = (RPS(создание поста) + RPS(загрузка ленты) + RPS(просмотр поста) + RPS(поиск)) * 5 = 33 735
traffic = 33 735 * 400 kb = 13 GB/s

- HDD: 
  - Disks_for_capacity = 17 263 / 32 = 539
  - Disks_for_throughput = 13 GB/s / 100 MB/s = 13 312 / 100 = 134
  - Disks_for_iops = iops / disk_iops = 33 735 / 100 = 337
  - Disks = max(ceil(Disks_for_capacity), ceil(Disks_for_throughput), ceil(Disks_for_iops)) = 539
- SSD(sata):
  - Disks_for_capacity = 17 263 / 100 = 173
  - Disks_for_throughput = 13 312 / 500 = 27
  - Disks_for_iops = iops / disk_iops = 33 735 / 1000 = 34
  - Disks = max(ceil(Disks_for_capacity), ceil(Disks_for_throughput), ceil(Disks_for_iops)) = 173
- SSD(nVME):
  - Disks_for_capacity = 17 263 / 30 = 575
  - Disks_for_throughput = 13 312 / 10 000 = 2
  - Disks_for_iops = iops / disk_iops =  33 735 / 10000 = 4
  - Disks = max(ceil(Disks_for_capacity), ceil(Disks_for_throughput), ceil(Disks_for_iops)) = 575

## Комментарии + реакции
capacity(комментарии + реакции) = traffic * 86400 * 365 = 2 mb/s * 86400 * 365 = 60 TB
IOPS = RPS(реакции read + реакции write) + RPS (комментарии read + комментари write) = 15 378
traffic = 15 378 * 2 kb = 31 mb/s

- HDD: 
  - Disks_for_capacity = 60 / 32 = 2
  - Disks_for_throughput = traffic_per_second / disk_throughput = 1
  - Disks_for_iops = iops / disk_iops = 15 378 / 100 = 154
  - Disks = max(ceil(Disks_for_capacity), ceil(Disks_for_throughput), ceil(Disks_for_iops)) = 154
- SSD(sata):
  - Disks_for_capacity = capacity / disk_capacity = 1
  - Disks_for_throughput = traffic_per_second / disk_throughput = 1
  - Disks_for_iops = iops / disk_iops = 15 378 / 1000 = 16
  - Disks = max(ceil(Disks_for_capacity), ceil(Disks_for_throughput), ceil(Disks_for_iops)) = 16
- SSD(nVME):
  - Disks_for_capacity = capacity / disk_capacity = 2
  - Disks_for_throughput = traffic_per_second / disk_throughput = 1
  - Disks_for_iops = iops / disk_iops =  15 378 / 10000 = 2
  - Disks = max(ceil(Disks_for_capacity), ceil(Disks_for_throughput), ceil(Disks_for_iops)) = 2

## Подписки
capacity (подписки) = traffic * 86400 * 365 = 10 kb/s * 86400 * 365 = 300 GB
