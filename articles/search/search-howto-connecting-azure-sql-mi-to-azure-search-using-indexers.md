---
title: Arama dizini oluşturma için Azure SQL yönetilen örnek bağlantısı-Azure Search
description: Azure Search üzerindeki bir dizin oluşturucudan SQL yönetilen örneklere bağlantılara izin vermek için ortak uç noktayı etkinleştirin.
author: vl8163264128
manager: briansmi
services: search
ms.service: search
ms.topic: conceptual
ms.date: 07/01/2019
ms.author: victliu
ms.openlocfilehash: ae947e85f600b3bc380898ad5820239aa823794d
ms.sourcegitcommit: 920ad23613a9504212aac2bfbd24a7c3de15d549
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/15/2019
ms.locfileid: "68229134"
---
# <a name="configure-a-connection-from-an-azure-search-indexer-to-sql-managed-instance"></a>Azure Search Dizin oluşturucudan SQL yönetilen örneği 'ne bağlantı yapılandırma
[Azure SQL veritabanı 'nı Dizinleyicileri kullanarak Azure Search bağlama](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md#faq)bölümünde belirtildiği gibi, **SQL tarafından yönetilen örneklere** karşı dizin oluşturucular oluşturmak, Azure Search tarafından genel uç nokta aracılığıyla desteklenir.

## <a name="create-azure-sql-managed-instance-with-public-endpoint"></a>Ortak uç nokta ile Azure SQL yönetilen örneği oluşturma
**Ortak uç noktayı etkinleştir** seçeneği belirlenmiş olarak bir SQL yönetilen örneği oluşturun.

   ![Ortak uç noktayı etkinleştir](media/search-howto-connecting-azure-sql-mi-to-azure-search-using-indexers/enable-public-endpoint.png "Ortak uç noktayı etkinleştir")

## <a name="enable-azure-sql-managed-instance-public-endpoint"></a>Azure SQL yönetilen örneği genel uç noktasını etkinleştir
Ayrıca, mevcut bir SQL yönetilen örneği üzerinde **güvenlik** > **sanal ağı** > **genel uç nokta** > **Etkinleştir**altında ortak uç noktayı etkinleştirebilirsiniz.

   ![Ortak uç noktayı etkinleştir](media/search-howto-connecting-azure-sql-mi-to-azure-search-using-indexers/mi-vnet.png "Ortak uç noktayı etkinleştir")

## <a name="verify-nsg-rules"></a>NSG kurallarını doğrulama
Ağ güvenlik grubunun Azure hizmetlerinden gelen bağlantılara izin veren doğru **gelen güvenlik kurallarına** sahip olup olmadığını denetleyin.

   ![NSG gelen güvenlik kuralı](media/search-howto-connecting-azure-sql-mi-to-azure-search-using-indexers/nsg-rule.png "NSG gelen güvenlik kuralı")

## <a name="get-public-endpoint-connection-string"></a>Genel uç nokta bağlantı dizesi al
**Genel uç nokta** için bağlantı dizesini kullandığınızdan emin olun (bağlantı noktası 1433 değil bağlantı noktası 3342).

   ![Genel uç nokta bağlantı dizesi](media/search-howto-connecting-azure-sql-mi-to-azure-search-using-indexers/mi-connection-string.png "Genel uç nokta bağlantı dizesi")

## <a name="next-steps"></a>Sonraki adımlar
Yapılandırma sayesinde, artık Portal veya REST API kullanarak bir Azure Search Indexer için veri kaynağı olarak bir SQL yönetilen örneği belirtebilirsiniz. Daha fazla bilgi için bkz. [Dizin oluşturucular kullanarak Azure Search Azure SQL veritabanı bağlama](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md) .
