---
title: Restrict user access to data operations only with Azure Cosmos DB
description: Learn how to restrict access to data operations only with Azure Cosmos DB
author: seesharprun
ms.service: cosmos-db
ms.topic: how-to
ms.date: 12/9/2019
ms.author: sidandrews
ms.reviewer: mjbrown
ms.custom: devx-track-azurepowershell
---

# Restrict user access to data operations in Azure Cosmos DB
[!INCLUDE[NoSQL, MongoDB, Cassandra, Gremlin, Table](includes/appliesto-nosql-mongodb-cassandra-gremlin-table.md)]

In Azure Cosmos DB, there are two ways to authenticate your interactions with the database service:

- using your Microsoft Entra identity when interacting with the Azure portal,
- using Azure Cosmos DB [keys](database-security.md#primary-keys) or [resource tokens](secure-access-to-data.md#resource-tokens) when issuing calls from APIs and SDKs.

Each authentication method gives access to different sets of operations, with some overlap:

:::image type="content" source="./media/how-to-restrict-user-data/operations.png" alt-text="Split of operations per authentication type" border="false":::

> [!NOTE]
> [Microsoft Entra ID identities data operations are supported by NoSQL.](how-to-setup-rbac.md)

In some scenarios, you may want to restrict some users of your organization to perform data operations (that is CRUD requests and queries) only. This is typically the case for developers who don't need to create or delete resources, or change the provisioned throughput of the containers they are working on.

You can restrict the access by applying the following steps:
1. Creating a custom Microsoft Entra role for the users whom you want to restrict access. The custom Active Directory role should have fine-grained access level to operations using Azure Cosmos DB's [granular actions](/azure/role-based-access-control/resource-provider-operations#microsoftdocumentdb).
1. Disallowing the execution of non-data operations with keys. You can achieve this by restricting these operations to Azure Resource Manager calls only.

The next sections of this article show how to perform these steps.

> [!NOTE]
> In order to execute the commands in the next sections, you need to install Azure PowerShell Module 3.0.0 or later, as well as the [Azure Owner Role](/azure/role-based-access-control/built-in-roles#owner) on the subscription that you are trying to modify.

In the PowerShell scripts in the next sections, substitute the following placeholders with values specific to your environment:
- `$MySubscriptionId` - The subscription ID that contains the Azure Cosmos DB account where you want to limit the permissions. For example: `e5c8766a-eeb0-40e8-af56-0eb142ebf78e`.
- `$MyResourceGroupName` - The resource group containing the Azure Cosmos DB account. For example: `myresourcegroup`.
- `$MyAzureCosmosDBAccountName` - The name of your Azure Cosmos DB account. For example: `mycosmosdbsaccount`.
- `$MyUserName` - The login (username@domain) of the user for whom you want to limit access. For example: `cosmosdbuser@contoso.com`.

## Select your Azure subscription

Azure PowerShell commands require you to login and select the subscription to execute the commands:

```azurepowershell
Login-AzAccount
Select-AzSubscription $MySubscriptionId
```

<a name='create-the-custom-azure-active-directory-role'></a>

## Create the custom Microsoft Entra role

The following script creates a Microsoft Entra role assignment with "Key Only" access for Azure Cosmos DB accounts. The role is based on [Azure custom roles](/azure/role-based-access-control/custom-roles) and [Granular actions for Azure Cosmos DB](/azure/role-based-access-control/resource-provider-operations#microsoftdocumentdb). These roles and actions are part of the `Microsoft.DocumentDB` Microsoft Entra namespace.

1. First, create a JSON document named `AzureCosmosKeyOnlyAccess.json` with the following content:

    ```
    {
        "Name": "Azure Cosmos DB Key Only Access Custom Role",
        "Id": "00000000-0000-0000-0000-0000000000",
        "IsCustom": true,
        "Description": "This role restricts the user to read the account keys only.",
        "Actions":
        [
            "Microsoft.DocumentDB/databaseAccounts/listKeys/action"
        ],
        "NotActions": [],
        "DataActions": [],
        "NotDataActions": [],
        "AssignableScopes":
        [
            "/subscriptions/$MySubscriptionId"
        ]
    }
    ```

1. Run the following commands to create the Role assignment and assign it to the user:

    ```azurepowershell
    New-AzRoleDefinition -InputFile "AzureCosmosKeyOnlyAccess.json"
    New-AzRoleAssignment -SignInName $MyUserName -RoleDefinitionName "Azure Cosmos DB Key Only Access Custom Role" -ResourceGroupName $MyResourceGroupName -ResourceName $MyAzureCosmosDBAccountName -ResourceType "Microsoft.DocumentDb/databaseAccounts"
    ```

## Disallow the execution of non-data operations

The following commands remove the ability to use keys to:
- create, modify or delete resources
- update container settings (including indexing policies, throughput etc.).

```azurepowershell
$cdba = Get-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" -ApiVersion "2015-04-08" -ResourceGroupName $MyResourceGroupName -ResourceName $MyAzureCosmosDBAccountName
$cdba.Properties.disableKeyBasedMetadataWriteAccess="True"
$cdba | Set-AzResource -Force
```

## Next steps

- Learn more about [Azure Cosmos DB's role-based access control](role-based-access-control.md)
- Get an overview of [secure access to data in Azure Cosmos DB](secure-access-to-data.md)
