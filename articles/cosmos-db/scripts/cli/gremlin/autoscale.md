---
title: Azure Cosmos DB for Gremlin database and graph with autoscale
description: Use this Azure CLI script to create an Azure Cosmos DB for Gremlin account, database, and graph with autoscale.
author: seesharprun
ms.author: sidandrews
ms.reviewer: mjbrown
ms.service: cosmos-db
ms.subservice: apache-gremlin
ms.topic: sample
ms.date: 05/02/2022
ms.custom: kr2b-contr-experiment, devx-track-azurecli
---

# Use Azure CLI to create a API for Gremlin account, database, and graph with autoscale

[!INCLUDE[Gremlin](../../../includes/appliesto-gremlin.md)]

The script in this article creates an Azure Cosmos DB for Gremlin account, database, and graph with autoscale.

## Prerequisites

- [!INCLUDE [quickstarts-free-trial-note](~/reusable-content/ce-skilling/azure/includes/quickstarts-free-trial-note.md)]

- This script requires Azure CLI version 2.30 or later.

  - You can run the script in the Bash environment in [Azure Cloud Shell](/azure/cloud-shell/get-started). When Cloud Shell opens, make sure to select **Bash** in the environment field at the upper left of the shell window. Cloud Shell has the latest version of Azure CLI.

    :::image type="icon" source="~/reusable-content/ce-skilling/azure/media/cloud-shell/launch-cloud-shell-button.png" alt-text="Button to launch the Azure Cloud Shell." border="false" link="https://shell.azure.com":::

  - If you prefer, you can [install Azure CLI](/cli/azure/install-azure-cli) to run the script locally. Run [az version](/cli/azure/reference-index?#az-version) to find your Azure CLI version, and run [az upgrade](/cli/azure/reference-index?#az-upgrade) if you need to upgrade. Sign in to Azure by running [az login](/cli/azure/reference-index#az-login).

## Sample script

This script uses the following commands:

- [az group create](/cli/azure/group#az-group-create) creates a resource group to store all resources.
- [az cosmosdb create](/cli/azure/cosmosdb#az-cosmosdb-create) with the `--capabilities EnableGremlin` parameter creates a Gremlin-enabled Azure Cosmos DB account.
- [az cosmosdb gremlin database create](/cli/azure/cosmosdb/gremlin/database#az-cosmosdb-gremlin-database-create) creates an Azure Cosmos DB for Gremlin database.
- [az cosmosdb gremlin graph create](/cli/azure/cosmosdb/gremlin/graph#az-cosmosdb-gremlin-graph-create) with the `--max-throughput` parameter set to minimum `4000` creates an Azure Cosmos DB for Gremlin graph with autoscale.

:::code language="azurecli" source="~/azure_cli_scripts/cosmosdb/gremlin/autoscale.sh" id="FullScript":::

## Delete resources

If you don't need the resources the script created, use the [az group delete](/cli/azure/group#az-group-delete) command to delete the resource group and all resources it contains, including the Azure Cosmos DB account and database.

```azurecli
az group delete --name $resourceGroup
```

## Next steps

[Azure Cosmos DB CLI documentation](/cli/azure/cosmosdb)
