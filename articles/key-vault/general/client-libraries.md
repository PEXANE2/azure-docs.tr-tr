---
title: Azure Key Vault için istemci kitaplıkları | Microsoft Docs
description: Azure Key Vault için istemci kitaplıkları
services: key-vault
author: msmbaldwin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: general
ms.topic: tutorial
ms.date: 08/14/2020
ms.author: mbaldwin
ms.openlocfilehash: 9cc8c1e9406f6b92a445cd52a4ad26e71d83ef5a
ms.sourcegitcommit: 64ad2c8effa70506591b88abaa8836d64621e166
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88264094"
---
# <a name="client-libraries-for-azure-key-vault"></a>Azure Key Vault için istemci kitaplıkları

Azure Key Vault için istemci kitaplıkları, .NET, Python, Java ve JavaScript gibi çeşitli dillerden Key Vault işlevlere programlı erişim sağlar.

## <a name="client-libraries-per-language-and-object"></a>Dil ve nesne başına istemci kitaplıkları

Her SDK 'nın aşağıdaki tabloya göre gizli dizi, anahtar ve sertifika için ayrı istemci kitaplıkları vardır.

| Dil | Gizli Diziler | Anahtarlar | Sertifikalar |
|--|--|--|--|
| .NET | - [API başvurusu](/dotnet/api/azure.security.keyvault.secrets?view=azure-dotnet)<br>- [NuGet paketi](https://www.nuget.org/packages/Azure.Security.KeyVault.Secrets/)<br>- [Kitaplık kaynak kodu](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/keyvault/Azure.Security.KeyVault.Secrets)<br>- [Hızlı başlangıç](../secrets/quick-create-net.md) | - [API başvurusu](/dotnet/api/azure.security.keyvault.keys?view=azure-dotnet)<br>- [NuGet paketi](https://www.nuget.org/packages/Azure.Security.KeyVault.Keys/)<br>- [Kitaplık kaynak kodu](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/keyvault/Azure.Security.KeyVault.Keys) | - [API başvurusu](/dotnet/api/azure.security.keyvault.certificates?view=azure-dotnet)<br>- [NuGet paketi](https://www.nuget.org/packages/Azure.Security.KeyVault.Certificates/)<br>- [Kitaplık kaynak kodu](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/keyvault/Azure.Security.KeyVault.Certificates) |
| Python| - [API başvurusu](/python/api/overview/azure/keyvault-secrets-readme?view=azure-python)<br>- [Pypı paketi](https://pypi.org/project/azure-keyvault-secrets/)<br>- [Kitaplık kaynak kodu](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/keyvault/azure-keyvault-secrets)<br>- [Hızlı başlangıç](../secrets/quick-create-python.md) |- [API başvurusu](/python/api/overview/azure/keyvault-keys-readme?view=azure-python)<br>- [Pypı paketi](https://pypi.org/project/azure-keyvault-keys/)<br>- [Kitaplık kaynak kodu](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/keyvault/azure-keyvault-keys)<br>- [Hızlı başlangıç](../keys/quick-create-python.md) | - [API başvurusu](/python/api/overview/azure/keyvault-certificates-readme?view=azure-python)<br>- [Pypı paketi](https://pypi.org/project/azure-keyvault-certificates/)<br>- [Kitaplık kaynak kodu](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/keyvault/azure-keyvault-certificates)<br>- [Hızlı başlangıç](../certificates/quick-create-python.md) |
| Java | - [API başvurusu](https://azuresdkdocs.blob.core.windows.net/$web/java/azure-security-keyvault-secrets/4.2.0/index.html)<br>- [Kitaplık kaynak kodu](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/keyvault/azure-security-keyvault-secrets)<br>- [Hızlı başlangıç](../secrets/quick-create-java.md) |- [API başvurusu](https://azuresdkdocs.blob.core.windows.net/$web/java/azure-security-keyvault-keys/4.2.0/index.html)<br>- [Kitaplık kaynak kodu](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/keyvault/azure-security-keyvault-keys) | - [API başvurusu](https://azuresdkdocs.blob.core.windows.net/$web/java/azure-security-keyvault-certificates/4.1.0/index.html)<br>- [Kitaplık kaynak kodu](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/keyvault/azure-security-keyvault-certificates) |
| Node.js | - [API başvurusu](/javascript/api/@azure/keyvault-secrets/?view=azure-node-latest)<br>- [NPM paketi](https://www.npmjs.com/package/@azure/keyvault-secrest)<br>- [Kitaplık kaynak kodu](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/keyvault/keyvault-secrets)<br>- [Hızlı başlangıç](../secrets/quick-create-node.md) |- [API başvurusu](/javascript/api/@azure/keyvault-keys/?view=azure-node-latest)<br>- [NPM paketi](https://www.npmjs.com/package/@azure/keyvault-keys)<br>- [Kitaplık kaynak kodu](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/keyvault/keyvault-keys)| - [API başvurusu](/javascript/api/@azure/keyvault-certificates/?view=azure-node-latest)<br>- [NPM paketi](https://www.npmjs.com/package/@azure/keyvault-certificates)<br>- [Kitaplık kaynak kodu](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/keyvault/keyvault-certificates) |

## <a name="next-steps"></a>Sonraki adımlar

- Bkz. [Azure Key Vault Geliştirici Kılavuzu](developers-guide.md)
- [Azure Key Vault yönetilen kimlik](managed-identity.md) hakkında daha fazla bilgi edinin