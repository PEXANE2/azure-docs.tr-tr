---
title: Kodda paylaşılan erişim imza belirteçlerini getirme | Azure Key Vault
description: Yönetilen depolama hesabı özelliği, Azure Key Vault ile Azure depolama hesabı arasında sorunsuz bir tümleştirme sağlar.
ms.topic: conceptual
ms.service: key-vault
ms.subservice: secrets
author: msmbaldwin
ms.author: mbaldwin
manager: rkarlin
ms.date: 09/10/2019
ms.openlocfilehash: e429115ce2624685c413ae252229964feee70137
ms.sourcegitcommit: f7e160c820c1e2eb57dc480b2a8fd6bef7053e91
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2020
ms.locfileid: "86232602"
---
# <a name="fetch-shared-access-signature-tokens-in-code"></a>Kodda paylaşılan erişim imza belirteçlerini getirme

Depolama hesabınızı, Anahtar Kasanızda depolanan paylaşılan erişim imzası (SAS) belirteçleriyle yönetebilirsiniz. Daha fazla bilgi için bkz. [SAS kullanarak Azure depolama kaynaklarına sınırlı erişim verme](../../storage/common/storage-sas-overview.md).

Bu makalede, bir SAS belirteci getiren ve ile işlem gerçekleştiren .NET kodu örnekleri sağlanmaktadır. SAS belirteçlerini oluşturma ve depolama hakkında daha fazla bilgi için bkz. [Key Vault ve Azure CLI ile depolama hesabı anahtarlarını yönetme](overview-storage-keys.md) veya [Key Vault ve Azure PowerShell depolama hesabı anahtarlarını yönetme](overview-storage-keys-powershell.md).

## <a name="code-samples"></a>Kod örnekleri

Bu örnekte, kod anahtar kasaınızdan bir SAS belirteci getirir, bunu yeni bir depolama hesabı oluşturmak için kullanır ve yeni bir blob hizmeti istemcisi oluşturur.

```cs
// The shared access signature is stored as a secret in keyvault. 
// After you get a security token, create a new SecretClient with vault credentials and the key vault URI.
// The format for the key vault URI (kvuri) is https://<YourKeyVaultName>.vault.azure.net

var kv = new SecretClient(new Uri(kvUri), new DefaultAzureCredential());

// Now retrive your storage SAS token from Key Vault using the name of the secret (secretName).

KeyVaultSecret secret = client.GetSecret(secretName);
var sasToken = secret.Value;

// Create new storage credentials using the SAS token.
StorageCredentials accountSAS = new StorageCredentials(sasToken);

// Use these credentials and your storage account name to create a Blob service client.
CloudStorageAccount accountWithSAS = new CloudStorageAccount(accountSAS, "<storage-account>", endpointSuffix: null, useHttps: true);
CloudBlobClient blobClientWithSAS = accountWithSAS.CreateCloudBlobClient();
```

Paylaşılan erişim imza belirtecinizin kullanım süreleri dolarsa, anahtar kasaınızdan paylaşılan erişim imza belirtecini getirip kodu güncelleştirebilirsiniz.

```cs
// If your shared access signature token is about to expire,
// get the shared access signature token again from Key Vault and update it.
KeyVaultSecret secret = client.GetSecret(secretName);
var sasToken = secret.Value;
accountSAS.UpdateSASToken(sasToken);
```


## <a name="next-steps"></a>Sonraki adımlar
- [SAS kullanarak Azure depolama kaynaklarına sınırlı erişim verme](../../storage/common/storage-sas-overview.md)hakkında bilgi edinin.
- [Azure PowerShell](overview-storage-keys-powershell.md) [Key Vault ve Azure CLI veya Azure PowerShell ile depolama hesabı anahtarlarını yönetmeyi](overview-storage-keys.md) öğrenin.
- Bkz. [yönetilen depolama hesabı anahtar örnekleri](https://github.com/Azure-Samples?utf8=%E2%9C%93&q=key+vault+storage&type=&language=)
