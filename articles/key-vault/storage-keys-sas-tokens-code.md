---
title: Azure Key Vault yönetilen depolama hesabı - PowerShell sürümü
description: Yönetilen depolama hesabı özelliği, Azure Anahtar Kasası ile Azure depolama hesabı arasında sorunsuz bir tümleştirme sağlar.
ms.topic: conceptual
ms.service: key-vault
ms.subservice: secrets
author: msmbaldwin
ms.author: mbaldwin
manager: rkarlin
ms.date: 09/10/2019
ms.openlocfilehash: cbd7bd034c5cbbdf5308ec660a96dc52a9a2b310
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78200711"
---
# <a name="fetch-shared-access-signature-tokens-in-code"></a>Kodda paylaşılan erişim imza belirteçlerini getirme

Anahtar kasanızdaki [paylaşılan erişim imza belirteçleri](../storage/common/storage-dotnet-shared-access-signature-part-1.md) ile depolama hesabınızı yönetebilirsiniz. Bu makalede, bir SAS belirteci getiren ve onunla işlemleri gerçekleştiren C# kodu örnekleri sağlar.  SAS belirteçlerinin nasıl oluşturulup depolanış edilebildiğim hakkında daha fazla bilgi için, [Key Vault ve Azure CLI ile depolama hesabı anahtarlarını yönet](key-vault-ovw-storage-keys.md) veya Key Vault ve Azure [PowerShell ile depolama hesabı anahtarlarını yönet'e](key-vault-overview-storage-keys-powershell.md)bakın.

## <a name="code-samples"></a>Kod örnekleri

Bu örnekte, kod anahtar kasanızdan bir SAS belirteci getirir, yeni bir depolama hesabı oluşturmak için kullanır ve yeni bir Blob hizmet istemcisi oluşturur.  

```cs
// After you get a security token, create KeyVaultClient with vault credentials.
var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(securityToken));

// Get a shared access signature token for your storage from Key Vault.
// The format for SecretUri is https://<YourKeyVaultName>.vault.azure.net/secrets/<ExamplePassword>
var sasToken = await kv.GetSecretAsync("SecretUri");

// Create new storage credentials by using the shared access signature token.
var accountSasCredential = new StorageCredentials(sasToken.Value);

// Use the storage credentials and the Blob storage endpoint to create a new Blob service client.
var accountWithSas = new CloudStorageAccount(accountSasCredential, new Uri ("https://myaccount.blob.core.windows.net/"), null, null, null);

var blobClientWithSas = accountWithSas.CreateCloudBlobClient();
```

Paylaşılan erişim imza belirtecinizin süresi dolmak üzereyse, paylaşılan erişim imza jetonunu anahtar kasanızdan alabilir ve kodu güncelleştirebilirsiniz.

```cs
// If your shared access signature token is about to expire,
// get the shared access signature token again from Key Vault and update it.
sasToken = await kv.GetSecretAsync("SecretUri");
accountSasCredential.UpdateSASToken(sasToken);
```


## <a name="next-steps"></a>Sonraki adımlar
- Key Vault ve Azure CLI veya [Azure PowerShell](key-vault-overview-storage-keys-powershell.md) [ile depolama hesabı anahtarlarını](key-vault-ovw-storage-keys.md) nasıl yönetebilirsiniz öğrenin.
- Bkz. [Yönetilen depolama hesabı anahtar örnekleri](https://github.com/Azure-Samples?utf8=%E2%9C%93&q=key+vault+storage&type=&language=)
- [Anahtarlar, sırlar ve sertifikalar hakkında](about-keys-secrets-and-certificates.md)
- [Anahtar Vault PowerShell referans](/powershell/module/az.keyvault/?view=azps-1.2.0#key_vault)
