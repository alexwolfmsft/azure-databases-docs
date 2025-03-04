---
author: AlicjaKucharczyk
author: akashraokm
ms.author: akashrao
ms.reviewer: maghan
ms.date: 06/18/2024
ms.service: azure-database-postgresql
ms.subservice: flexible-server
ms.topic: include
---
### stats_temp_directory

| Attribute      | Value                                                      |
|----------------|------------------------------------------------------------|
| Category       | Statistics / Query and Index Statistics Collector |
| Description    | Writes temporary statistics files to the specified directory. |
| Data type      | string    |
| Default value  | `pg_stat_tmp` |
| Allowed values | `pg_stat_tmp`  |
| Parameter type | read-only      |
| Documentation  | [stats_temp_directory](https://www.postgresql.org/docs/11/runtime-config-statistics.html#GUC-STATS-TEMP-DIRECTORY) |


[!INCLUDE [server-parameters-azure-notes-void](./server-parameters-azure-notes-void.md)]



