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
### cluster_name

| Attribute      | Value                                                      |
|----------------|------------------------------------------------------------|
| Category       | Reporting and Logging / Process Title |
| Description    | Sets the name of the cluster, which is included in the process title. |
| Data type      | string    |
| Default value  |               |
| Allowed values |                |
| Parameter type | read-only      |
| Documentation  |                                                                                                                 |


[!INCLUDE [server-parameters-azure-notes-void](./server-parameters-azure-notes-void.md)]



### update_process_title

| Attribute      | Value                                                      |
|----------------|------------------------------------------------------------|
| Category       | Reporting and Logging / Process Title |
| Description    | Updates the process title to show the active SQL command.             |
| Data type      | boolean   |
| Default value  | `on`          |
| Allowed values | `on`           |
| Parameter type | read-only      |
| Documentation  | [update_process_title](https://www.postgresql.org/docs/14/runtime-config-logging.html#GUC-UPDATE-PROCESS-TITLE) |


[!INCLUDE [server-parameters-azure-notes-void](./server-parameters-azure-notes-void.md)]



