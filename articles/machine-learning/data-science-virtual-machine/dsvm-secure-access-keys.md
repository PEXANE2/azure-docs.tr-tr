---
title: Erişim kimlik bilgilerini güvenli bir şekilde depolama
titleSuffix: Azure Data Science Virtual Machine
description: Veri Bilimi Sanal Makinesi'nde erişim kimlik bilgilerini güvenli bir şekilde nasıl depolayarak depolaymayı öğrenin. Erişim kimlik bilgilerini depolamak için yönetilen hizmet kimliklerini ve Azure Anahtar Kasası'nı nasıl kullanacağınızı öğreneceksiniz.
keywords: derin öğrenme, Yapay Bilgi, veri bilimi araçları, veri bilimi sanal makine, jeouzamsal analitik, ekip veri bilimi süreci
services: machine-learning
ms.service: machine-learning
ms.subservice: data-science-vm
author: vijetajo
ms.author: vijetaj
ms.topic: conceptual
ms.date: 05/08/2018
ms.openlocfilehash: 1cb0c5094d49eac5a1c8f63406a28d2927d8fa94
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79477332"
---
# <a name="store-access-credentials-securely-on-an-azure-data-science-virtual-machine"></a>Erişim kimlik bilgilerini Azure Veri Bilimi Sanal Makinesi'nde güvenli bir şekilde depolama

Bulut uygulamalarındaki kodun bulut hizmetlerine kimlik doğrulama için kimlik bilgileri içermesi yaygındır. Bu kimlik bilgilerinin nasıl yönetilen ve güvenli hale getirmek, bulut uygulamaları oluşturmada iyi bilinen bir sorundur. İdeal olarak, kimlik bilgileri hiçbir zaman geliştirici iş istasyonlarında görünmemeli veya kaynak denetimi için iade edilmeli.

[Azure kaynakları için yönetilen kimlikler](https://docs.microsoft.com/azure/active-directory/managed-service-identity/overview) özelliği, Azure hizmetlerine Azure Etkin Dizin'de (Azure AD) otomatik olarak yönetilen bir kimlik vererek bu sorunu çözmeyi kolaylaştırır. Bu kimliği kullanarak, Azure AD kimlik doğrulamasını destekleyen tüm hizmetlerde kodunuzda kimlik bilgileri bulunmasına gerek kalmadan kimlik doğrulaması yapabilirsiniz.

Kimlik bilgilerini güvence altına almanın bir yolu, sırları ve şifreleme anahtarlarını güvenli bir şekilde depolamak için yönetilen bir Azure hizmeti olan [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/)ile birlikte Windows Installer 'ı (MSI) kullanmaktır. Yönetilen kimliği kullanarak bir anahtar kasasına erişebilir ve ardından yetkili sırları ve şifreleme anahtarlarını anahtar kasasından alabilirsiniz.

Azure kaynakları ve Key Vault için yönetilen kimliklerle ilgili belgeler, bu hizmetler hakkında ayrıntılı bilgi için kapsamlı bir kaynak içerir. Bu makalenin geri kalanı, Azure kaynaklarına erişmek için Veri Bilimi Sanal Makinesi'ndeki (DSVM) MSI ve Key Vault'un temel kullanımından geçer. 

## <a name="create-a-managed-identity-on-the-dsvm"></a>DSVM'de yönetilen bir kimlik oluşturma

```azurecli-interactive
# Prerequisite: You have already created a Data Science VM in the usual way.

# Create an identity principal for the VM.
az vm assign-identity -g <Resource Group Name> -n <Name of the VM>
# Get the principal ID of the DSVM.
az resource list -n <Name of the VM> --query [*].identity.principalId --out tsv
```

## <a name="assign-key-vault-access-permissions-to-a-vm-principal"></a>Anahtar Kasa erişim izinlerini bir VM ilkesine atama

```azurecli-interactive
# Prerequisite: You have already created an empty Key Vault resource on Azure by using the Azure portal or Azure CLI.

# Assign only get and set permissions but not the capability to list the keys.
az keyvault set-policy --object-id <Principal ID of the DSVM from previous step> --name <Key Vault Name> -g <Resource Group of Key Vault>  --secret-permissions get set
```

## <a name="access-a-secret-in-the-key-vault-from-the-dsvm"></a>DSVM'den anahtar kasasında ki bir gizliye erişin

```bash
# Get the access token for the VM.
x=`curl http://localhost:50342/oauth2/token --data "resource=https://vault.azure.net" -H Metadata:true`
token=`echo $x | python -c "import sys, json; print(json.load(sys.stdin)['access_token'])"`

# Access the key vault by using the access token.
curl https://<Vault Name>.vault.azure.net/secrets/SQLPasswd?api-version=2016-10-01 -H "Authorization: Bearer $token"
```

## <a name="access-storage-keys-from-the-dsvm"></a>Depolama anahtarlarına DSVM'den erişin

```bash
# Prerequisite: You have granted your VMs MSI access to use storage account access keys based on instructions at https://docs.microsoft.com/azure/active-directory/managed-service-identity/tutorial-linux-vm-access-storage. This article describes the process in more detail.

y=`curl http://localhost:50342/oauth2/token --data "resource=https://management.azure.com/" -H Metadata:true`
ytoken=`echo $y | python -c "import sys, json; print(json.load(sys.stdin)['access_token'])"`
curl https://management.azure.com/subscriptions/<SubscriptionID>/resourceGroups/<ResourceGroup of Storage account>/providers/Microsoft.Storage/storageAccounts/<Storage Account Name>/listKeys?api-version=2016-12-01 --request POST -d "" -H "Authorization: Bearer $ytoken"

# Now you can access the data in the storage account from the retrieved storage account keys.
```

## <a name="access-the-key-vault-from-python"></a>Python'dan anahtar kasasına erişin

```python
from azure.keyvault import KeyVaultClient
from msrestazure.azure_active_directory import MSIAuthentication

"""MSI Authentication example."""

# Get credentials.
credentials = MSIAuthentication(
    resource='https://vault.azure.net'
)

# Create a Key Vault client.
key_vault_client = KeyVaultClient(
    credentials
)

key_vault_uri = "https://<key Vault Name>.vault.azure.net/"

secret = key_vault_client.get_secret(
    key_vault_uri,  # Your key vault URL.
    # The name of your secret that already exists in the key vault.
    "SQLPasswd",
    ""              # The version of the secret; empty string for latest.
)
print("My secret value is {}".format(secret.value))
```

## <a name="access-the-key-vault-from-azure-cli"></a>Azure CLI'den anahtar kasasına erişin

```azurecli-interactive
# With managed identities for Azure resources set up on the DSVM, users on the DSVM can use Azure CLI to perform the authorized functions. The following commands enable access to the key vault from Azure CLI without requiring login to an Azure account.
# Prerequisites: MSI is already set up on the DSVM as indicated earlier. Specific permissions, like accessing storage account keys, reading specific secrets, and writing new secrets, are provided to the MSI.

# Authenticate to Azure CLI without requiring an Azure account. 
az login --msi

# Retrieve a secret from the key vault. 
az keyvault secret show --vault-name <Vault Name> --name SQLPasswd

# Create a new secret in the key vault.
az keyvault secret set --name MySecret --vault-name <Vault Name> --value "Helloworld"

# List access keys for the storage account.
az storage account keys list -g <Storage Account Resource Group> -n <Storage Account Name>
```
