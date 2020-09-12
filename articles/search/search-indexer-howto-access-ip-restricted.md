---
title: Dizin Oluşturucu IP aralıklarına erişime izin ver
titleSuffix: Azure Cognitive Search
description: Dizin oluşturucularının erişim sahibi olması için IP Güvenlik Duvarı kurallarının nasıl ayarlanacağını anlatan yönergeler.
manager: nitinme
author: arv100kri
ms.author: arjagann
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 09/07/2020
ms.openlocfilehash: c80462707d3dccbb8fccff244017053c25ad46e8
ms.sourcegitcommit: 4a7a4af09f881f38fcb4875d89881e4b808b369b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/04/2020
ms.locfileid: "89463920"
---
# <a name="setting-up-ip-firewall-rules-to-enable-indexer-access"></a>Dizin Oluşturucu erişimini etkinleştirmek için IP güvenlik duvarı kuralları ayarlama

Azure kaynaklarında, depolama hesapları, Cosmos DB hesapları ve Azure SQL Server 'lar gibi IP güvenlik duvarı kuralları, yalnızca belirli IP aralıklarından gelen trafiğe verilere erişim izni verir.

Bu makalede, Azure Bilişsel Arama dizin oluşturucularının verilere güvenli bir şekilde erişebilmesi için bir depolama hesabı için Azure portal aracılığıyla IP kurallarının nasıl yapılandırılacağı açıklanır. Depolama alanına özel olarak, bu kılavuz, verilere erişimi güvenli hale getirmek için IP güvenlik duvarı kuralları sunan diğer Azure kaynaklarına doğrudan çevrilebilir.

> [!NOTE]
> Depolama hesabı için IP güvenlik duvarı kuralları yalnızca depolama hesabı ve arama hizmeti farklı bölgelerde olduğunda geçerlidir. Kurulumlarınız buna izin veriyorsa, [güvenilir hizmet özel durum seçeneğini](search-indexer-howto-access-trusted-service-exception.md)kullanmanızı öneririz.

## <a name="get-the-ip-address-of-the-search-service"></a>Arama hizmetinin IP adresini al

Arama hizmetinizin tam etki alanı adını (FQDN) alın. Bu, şöyle görünecektir `<search-service-name>.search.windows.net` . Azure portal arama hizmetinize arayarak FQDN 'yi bulabilirsiniz.

   ![Hizmet FQDN 'SI al](media\search-indexer-howto-secure-access\search-service-portal.png "Hizmet FQDN 'SI al")

Arama hizmetinin IP adresi, `nslookup` FQDN 'nin bir (veya a) gerçekleştirerek elde edilebilir `ping` . Bu, güvenlik duvarı kurallarına eklenecek IP adreslerinden biri olacaktır.

```azurepowershell

nslookup contoso.search.windows.net
Server:  server.example.org
Address:  10.50.10.50

Non-authoritative answer:
Name:    <name>
Address:  150.0.0.1
Aliases:  contoso.search.windows.net
```

## <a name="get-the-ip-address-ranges-for-azurecognitivesearch-service-tag"></a>"Azurecognivesearch" hizmet etiketi için IP adresi aralıklarını al

Hizmet etiketi için IP adresi aralıkları `AzureCognitiveSearch` [bulma API 'si (Önizleme)](../virtual-network/service-tags-overview.md#use-the-service-tag-discovery-api-public-preview) veya [indirilebilir JSON dosyası](../virtual-network/service-tags-overview.md#discover-service-tags-by-using-downloadable-json-files)aracılığıyla elde edilebilir.

Bu kılavuzda, arama hizmetinin Azure genel bulutu olduğu varsayıldığında, [Azure genel JSON dosyası](https://www.microsoft.com/download/details.aspx?id=56519) indirilmelidir.

   ![JSON dosyasını indir](media\search-indexer-howto-secure-access\service-tag.png "JSON dosyasını indir")

Arama hizmetinin Orta Batı ABD olduğu kabul edildiğinde JSON dosyasından, çok kiracılı Dizin Oluşturucu yürütme ortamı için IP adreslerinin listesi aşağıda listelenmiştir.

```json
    {
      "name": "AzureCognitiveSearch.WestCentralUS",
      "id": "AzureCognitiveSearch.WestCentralUS",
      "properties": {
        "changeNumber": 1,
        "region": "westcentralus",
        "platform": "Azure",
        "systemService": "AzureCognitiveSearch",
        "addressPrefixes": [
          "52.150.139.0/26",
          "52.253.133.74/32"
        ]
      }
    }
```

/32 IP adresleri için, "/32" (52.253.133.74/32-> 52.253.133.74) öğesini bırakın, diğerleri de tam olarak kullanılabilir.

## <a name="add-the-ip-address-ranges-to-ip-firewall-rules"></a>IP adresi aralıklarını IP güvenlik duvarı kurallarına ekleme

Bir depolama hesabının güvenlik duvarı kuralına IP adres aralıklarını eklemenin en kolay yolu Azure portal aracılığıyla yapılır. Portalda depolama hesabını bulun ve "**güvenlik duvarları ve sanal ağlar**" sekmesine gidin.

   ![Güvenlik Duvarı ve sanal ağlar](media\search-indexer-howto-secure-access\storage-firewall.png "Güvenlik Duvarı ve sanal ağlar")

Daha önce alınan üç IP adresini (1. arama hizmeti IP 'si için 1, `AzureCognitiveSearch` hizmet etiketi için 2) adres aralığında ekleyin ve "**Kaydet**" e tıklayın

   ![Güvenlik Duvarı IP kuralları](media\search-indexer-howto-secure-access\storage-firewall-ip.png "Güvenlik Duvarı IP kuralları")

Güvenlik duvarı kuralları, depolama hesabındaki verilere erişebilecek dizin oluşturucularının güncelleştirilmesini sağlamak için 5-10 dakika sürer.

## <a name="next-steps"></a>Sonraki Adımlar

Dizinlerin erişimine izin vermek için iki IP adresi kümesini nasıl alacağınız bilineceğimize göre, bazı ortak veri kaynakları için IP güvenlik duvarı kurallarını güncelleştirmek üzere aşağıdaki bağlantıları kullanın.

- [Azure Storage güvenlik duvarlarını yapılandırma](https://docs.microsoft.com/azure/storage/common/storage-network-security)
- [CosmosDB için IP güvenlik duvarını yapılandırma](https://docs.microsoft.com/azure/cosmos-db/firewall-support)
- [Azure SQL Server için IP güvenlik duvarını yapılandırma](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure)