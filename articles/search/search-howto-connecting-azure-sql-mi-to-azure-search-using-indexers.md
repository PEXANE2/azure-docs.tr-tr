---
title: Arama dizin oluşturma için Azure SQL Yönetilen Örnek bağlantısı
titleSuffix: Azure Cognitive Search
description: Azure Bilişsel Arama'daki bir dizin leyiciden SQL Yönetilen Örnekler'e bağlantılara izin vermek için ortak bitiş noktasını etkinleştirin.
manager: nitinme
author: vl8163264128
ms.author: victliu
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 65e483fd772e20daa73b465ea17dfa6ecde42233
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76964898"
---
# <a name="configure-a-connection-from-an-azure-cognitive-search-indexer-to-sql-managed-instance"></a>Bir Azure Bilişsel Arama dizinleyicisinden SQL Yönetilen Örnek'e bağlantı yapılandırma

[Azure SQL Veritabanı'nı dizin oluşturarak Azure Bilişsel Arama'ya bağlamada](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md#faq)belirtildiği gibi, SQL Yönetilen **Örneklere** karşı dizin oluşturucu oluşturmak, genel bitiş noktası üzerinden Azure Bilişsel Arama tarafından desteklenir.

## <a name="create-azure-sql-managed-instance-with-public-endpoint"></a>Ortak bitiş noktasıyla Azure SQL Yönetilen Örnek oluşturma
**Ortak bitiş noktası seçeneğini etkinleştir** seçeneği seçili olan bir SQL Yönetilen Örnek oluşturun.

   ![Genel bitiş noktasını etkinleştirme](media/search-howto-connecting-azure-sql-mi-to-azure-search-using-indexers/enable-public-endpoint.png "Genel bitiş noktasını etkinleştirme")

## <a name="enable-azure-sql-managed-instance-public-endpoint"></a>Azure SQL Yönetilen Örnek genel bitiş noktasını etkinleştirme
Ayrıca, **Security** > **Virtual network** > **Public endpoint** > **Enable**altında varolan bir SQL Yönetilen Örnek'te ortak bitiş noktasını da etkinleştirebilirsiniz.

   ![Genel bitiş noktasını etkinleştirme](media/search-howto-connecting-azure-sql-mi-to-azure-search-using-indexers/mi-vnet.png "Genel bitiş noktasını etkinleştirme")

## <a name="verify-nsg-rules"></a>NSG kurallarını doğrulayın
Ağ Güvenlik Grubu'nun Azure hizmetlerinden bağlantılara izin veren doğru **Gelen güvenlik kurallarına** sahip olup olmadığını denetleyin.

   ![NSG Gelen güvenlik kuralı](media/search-howto-connecting-azure-sql-mi-to-azure-search-using-indexers/nsg-rule.png "NSG Gelen güvenlik kuralı")

> [!NOTE]
> Dizinleyiciler, verileri okumak için SQL Yönetilen Örnek'in ortak bir bitiş noktasıyla yapılandırılmasını gerektirir.
> Ancak, geçerli kuralı (`public_endpoint_inbound`) aşağıdaki 2 kuralla değiştirerek gelen erişimi bu ortak bitiş noktasına sınırlamayı seçebilirsiniz:
>
> * `AzureCognitiveSearch` [Hizmet etiketinden](https://docs.microsoft.com/azure/virtual-network/service-tags-overview#available-service-tags) gelen erişime izin verme `AzureCognitiveSearch`("KAYNAK" `cognitive_search_inbound`= , "AD" = )
>
> * Arama hizmetinin IP adresinden gelen erişime izin vermek, tam nitelikli alan adını pingleyerek `<your-search-service-name>.search.windows.net`elde edilebilir (örn. ("KAYNAK" `IP address`= , "AD" = `search_service_inbound`)
>
> Bu 2 kuralın her biri için `3342`"PORT" `TCP`= , "PROTOKOL" = " "DESTINATION" = `Any`, "ACTION" =`Allow`

## <a name="get-public-endpoint-connection-string"></a>Genel uç nokta bağlantı dizesi alma
**Ortak bitiş noktası** (bağlantı noktası 3342, bağlantı noktası 1433 değil) için bağlantı dizesini kullandığınızdan emin olun.

   ![Ortak uç nokta bağlantı dizesi](media/search-howto-connecting-azure-sql-mi-to-azure-search-using-indexers/mi-connection-string.png "Ortak uç nokta bağlantı dizesi")

## <a name="next-steps"></a>Sonraki adımlar
Yapılandırma nın önüne geçen yapılandırmayla, artık portalı veya REST API'yi kullanarak Azure Bilişsel Arama dizinleyicisinin veri kaynağı olarak bir SQL Yönetilen Örnek belirtebilirsiniz. Daha fazla bilgi için [dizin oluşturmayı kullanarak Azure SQL Veritabanını Azure Bilişsel Arama'ya bağlama](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md) ya da birleştirme bilgisine bakın.
