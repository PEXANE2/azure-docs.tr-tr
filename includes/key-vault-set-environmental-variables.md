---
author: msmbaldwin
ms.service: key-vault
ms.topic: include
ms.date: 07/20/2020
ms.author: msmbaldwin
ms.openlocfilehash: 6dfc96df3623e44a4ba513c238f21482c2a2989d
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87013135"
---
Uygulamamızda DefaultAzureCredential yöntemi üç çevresel değişkene dayanır: `AZURE_CLIENT_ID` , `AZURE_CLIENT_SECRET` ve `AZURE_TENANT_ID` . Bu değişkenleri biçim kullanılarak "hizmet sorumlusu oluşturma" adımında döndürülen ClientID, clientSecret ve Tenantıd değerlerine ayarlayın `export VARNAME=VALUE` . (Bu yöntem yalnızca geçerli kabuğunuzun ve kabuktan oluşturulan süreçlerin değişkenlerini ayarlar; bu değişkenleri ortamınıza kalıcı olarak eklemek için `/etc/environment ` dosyanızı düzenleyin.) 

Ayrıca, Anahtar Kasası adınızı adlı bir ortam değişkeni olarak kaydetmeniz gerekir `KEY_VAULT_NAME` .

```console
export AZURE_CLIENT_ID=<your-clientID>

export AZURE_CLIENT_SECRET=<your-clientSecret>

export AZURE_TENANT_ID=<your-tenantId>

export KEY_VAULT_NAME=<your-key-vault-name>
````
