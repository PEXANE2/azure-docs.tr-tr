---
title: Şifreleme anahtarı bilgilerini al
titleSuffix: Azure Cognitive Search
description: Azure Key Vault anahtarı yönetebilmeniz için bir dizin veya eş anlamlı eşlemede kullanılan şifreleme anahtarı adını ve sürümünü alın.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 08/01/2020
ms.openlocfilehash: 37ff94608e9756142f70a4f3c64d0a6f7eeea685
ms.sourcegitcommit: 62e1884457b64fd798da8ada59dbf623ef27fe97
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88932908"
---
# <a name="get-customer-managed-key-information-from-indexes-and-synonym-maps"></a>Dizinlerden ve eş anlamlı eşlemlerden müşteri tarafından yönetilen anahtar bilgilerini alın

Azure Bilişsel Arama 'de, müşteri tarafından yönetilen şifreleme anahtarları Azure Key Vault oluşturulur, depolanır ve yönetilir. Bir nesnenin şifrelenip şifrelenmediğini veya hangi anahtar adının veya sürümünün kullanıldığını belirlemeniz gerekiyorsa, bir dizin veya eş anlamlı eşleme tanımından **encryptionKey** özelliğini almak için REST API veya SDK 'yı kullanın. 

Anahtar kullanımını izleyebilmek için Key Vault [günlüğü etkinleştirmenizi](../key-vault/general/logging.md) öneririz.

## <a name="get-the-admin-api-key"></a>Yönetici API anahtarını al

Bir arama hizmetinden nesne tanımları almak için yönetici haklarıyla kimlik doğrulaması yapmanız gerekir. Yönetici API anahtarını almanın en kolay yolu portalı kullanmaktır.

1. [Azure Portal](https://portal.azure.com/) oturum açın ve arama hizmeti genel bakış sayfasını açın.

1. Sol tarafta **anahtarlar** ' a tıklayın ve bir yönetici API 'sini kopyalayın. Dizin ve eş anlamlı eşleme alımı için bir yönetici anahtarı gereklidir.

Kalan adımlar için PowerShell 'e ve REST API geçin. Portal, eş anlamlı haritaları veya dizinlerin şifreleme anahtarı özelliklerini göstermez.

## <a name="use-powershell-and-rest"></a>PowerShell ve REST kullanma

Değişkenleri ayarlamak ve nesne tanımlarını almak için aşağıdaki komutları çalıştırın.

```powershell
<# Connect to Azure #>
$Connect-AzAccount

<# Provide the admin API key used for search service authentication  #>
$headers = @{
'api-key' = '<YOUR-ADMIN-API-KEY>'
'Content-Type' = 'application/json'
'Accept' = 'application/json' }

<# List all existing synonym maps #>
$uri= 'https://<YOUR-SEARCH-SERVICE>.search.windows.net/synonyms?api-version=2020-06-30&$select=name'
Invoke-RestMethod -Uri $uri -Headers $headers | ConvertTo-Json

<# List all existing indexes #>
$uri= 'https://<YOUR-SEARCH-SERVICE>.search.windows.net/indexes?api-version=2020-06-30&$select=name'
Invoke-RestMethod -Uri $uri -Headers $headers | ConvertTo-Json

<# Return a specific synonym map definition. The encryptionKey property is at the end #>
$uri= 'https://<YOUR-SEARCH-SERVICE>.search.windows.net/synonyms/<YOUR-SYNONYM-MAP-NAME>?api-version=2020-06-30'
Invoke-RestMethod -Uri $uri -Headers $headers | ConvertTo-Json

<# Return a specific index definition. The encryptionKey property is at the end #>
$uri= 'https://<YOUR-SEARCH-SERVICE>.search.windows.net/indexes/<YOUR-INDEX-NAME>?api-version=2020-06-30'
Invoke-RestMethod -Uri $uri -Headers $headers | ConvertTo-Json
```

## <a name="next-steps"></a>Sonraki adımlar

Hangi şifreleme anahtarı ve sürümünün kullanıldığını bildiğinize göre, Azure Key Vault anahtarı yönetebilir veya diğer yapılandırma ayarlarını kontrol edebilirsiniz.

+ [Hızlı başlangıç: PowerShell kullanarak Azure Key Vault'tan gizli dizi ayarlama ve alma](../key-vault/secrets/quick-create-powershell.md)

+ [Azure Bilişsel Arama veri şifrelemesi için müşteri tarafından yönetilen anahtarları yapılandırma](search-security-manage-encryption-keys.md)