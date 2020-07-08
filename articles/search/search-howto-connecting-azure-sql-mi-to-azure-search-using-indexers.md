---
title: Arama dizini oluşturma için Azure SQL yönetilen örnek bağlantısı
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama 'deki bir dizin oluşturucudan SQL yönetilen örneklere bağlantılara izin vermek için ortak uç noktayı etkinleştirin.
manager: nitinme
author: vl8163264128
ms.author: victliu
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 65e483fd772e20daa73b465ea17dfa6ecde42233
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "76964898"
---
# <a name="configure-a-connection-from-an-azure-cognitive-search-indexer-to-sql-managed-instance"></a>Azure Bilişsel Arama Dizin oluşturucudan SQL yönetilen örneği 'ne bağlantı yapılandırma

[Dizin oluşturucular kullanarak Azure SQL veritabanı 'Nı azure bilişsel arama bağlama](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md#faq)bölümünde belirtildiği gibi, **SQL yönetilen örneklere** karşı Dizin oluşturucular oluşturmak, Azure bilişsel arama tarafından genel uç nokta üzerinden desteklenir.

## <a name="create-azure-sql-managed-instance-with-public-endpoint"></a>Ortak uç nokta ile Azure SQL yönetilen örneği oluşturma
**Ortak uç noktayı etkinleştir** seçeneği belirlenmiş olarak bir SQL yönetilen örneği oluşturun.

   ![Ortak uç noktayı etkinleştir](media/search-howto-connecting-azure-sql-mi-to-azure-search-using-indexers/enable-public-endpoint.png "Ortak uç noktayı etkinleştir")

## <a name="enable-azure-sql-managed-instance-public-endpoint"></a>Azure SQL yönetilen örneği genel uç noktasını etkinleştir
Ayrıca, mevcut bir SQL yönetilen örneği üzerinde **güvenlik**  >  **sanal ağı**  >  **genel uç nokta**  >  **Etkinleştir**altında ortak uç noktayı etkinleştirebilirsiniz.

   ![Ortak uç noktayı etkinleştir](media/search-howto-connecting-azure-sql-mi-to-azure-search-using-indexers/mi-vnet.png "Ortak uç noktayı etkinleştir")

## <a name="verify-nsg-rules"></a>NSG kurallarını doğrulama
Ağ güvenlik grubunun Azure hizmetlerinden gelen bağlantılara izin veren doğru **gelen güvenlik kurallarına** sahip olup olmadığını denetleyin.

   ![NSG gelen güvenlik kuralı](media/search-howto-connecting-azure-sql-mi-to-azure-search-using-indexers/nsg-rule.png "NSG gelen güvenlik kuralı")

> [!NOTE]
> Dizin oluşturucular hala verileri okumak için SQL yönetilen örneğinin genel bir uç nokta ile yapılandırılmasını gerektirir.
> Ancak, geçerli kuralı ( `public_endpoint_inbound` ) aşağıdaki 2 kurallarla değiştirerek bu genel uç noktaya gelen erişimi kısıtlamayı seçebilirsiniz:
>
> * `AzureCognitiveSearch` [Hizmet etiketiyle](https://docs.microsoft.com/azure/virtual-network/service-tags-overview#available-service-tags) gelen erişime izin veriliyor ("kaynak" = `AzureCognitiveSearch` , "ad" = `cognitive_search_inbound` )
>
> * Arama hizmetinin IP adresinden gelen erişime izin vererek, tam etki alanı adı (ör.) ile ping yaparak elde edilebilir `<your-search-service-name>.search.windows.net` . ("kaynak" = `IP address` , "ad" = `search_service_inbound` )
>
> Bu 2 kuralların her biri için "PORT" = `3342` , "Protocol" = `TCP` , "DESTINATION" = `Any` , "ACTION" = olarak ayarlayın.`Allow`

## <a name="get-public-endpoint-connection-string"></a>Genel uç nokta bağlantı dizesi al
**Genel uç nokta** için bağlantı dizesini kullandığınızdan emin olun (bağlantı noktası 1433 değil bağlantı noktası 3342).

   ![Genel uç nokta bağlantı dizesi](media/search-howto-connecting-azure-sql-mi-to-azure-search-using-indexers/mi-connection-string.png "Genel uç nokta bağlantı dizesi")

## <a name="next-steps"></a>Sonraki adımlar
Yapılandırma sayesinde, artık Portal veya REST API kullanarak bir Azure Bilişsel Arama Dizin Oluşturucu için veri kaynağı olarak bir SQL yönetilen örneği belirtebilirsiniz. Daha fazla bilgi için bkz. [Dizin oluşturucular kullanarak Azure SQL veritabanı 'Nı azure bilişsel arama bağlama](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md) .
