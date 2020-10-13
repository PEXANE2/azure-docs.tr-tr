---
title: Güvenilen hizmet özel durumu aracılığıyla depolamaya erişime izin ver
titleSuffix: Azure Cognitive Search
description: Depolama hesaplarından güvenli bir şekilde veri erişimi için güvenilen hizmet özel durumunun nasıl ayarlanacağını anlatan yönergeler.
manager: nitinme
author: arv100kri
ms.author: arjagann
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 09/22/2020
ms.openlocfilehash: 4fbffaa7bc68bb32bd07b657f4b769e5af4302bf
ms.sourcegitcommit: a2d8acc1b0bf4fba90bfed9241b299dc35753ee6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/12/2020
ms.locfileid: "91950033"
---
# <a name="accessing-data-in-storage-accounts-securely-via-trusted-service-exception"></a>Güvenilen hizmet özel durumu aracılığıyla depolama hesaplarındaki verilere güvenli bir şekilde erişme

Depolama hesaplarında verilere erişen Dizin oluşturucular, verilere güvenli bir şekilde erişmek için [güvenilir hizmet özel durum](../storage/common/storage-network-security.md#exceptions) özelliğinden daha fazla kullanabilir. Bu mekanizma, [IP güvenlik duvarı kuralları aracılığıyla Dizin Oluşturucu erişimi](search-indexer-howto-access-ip-restricted.md) izni veren müşterileri, depolama hesaplarındaki verilere erişim için basit, güvenli ve ücretsiz bir alternatif sağlar.

> [!NOTE]
> Güvenilen bir hizmet özel durumu aracılığıyla depolama hesaplarındaki verilere erişim desteği, Azure Blob depolama ve Azure Data Lake Gen2 depolama ile sınırlıdır. Azure Tablo Depolaması desteklenmiyor.

## <a name="step-1-configure-connection-to-the-storage-account-via-identity"></a>1. Adım: depolama hesabı ile bağlantıyı kimlik aracılığıyla yapılandırma

Arama hizmetinin yönetilen kimliği aracılığıyla depolama hesaplarına erişmek üzere dizin oluşturucuyu yapılandırmak için [yönetilen kimlik erişimi kılavuzunda](search-howto-managed-identities-storage.md) özetlenen ayrıntıları izleyin.

## <a name="step-2-allow-trusted-microsoft-services-to-access-the-storage-account"></a>2. Adım: güvenilen Microsoft hizmetlerinin depolama hesabına erişmesine Izin verin

Azure portal, depolama hesabının "güvenlik duvarları ve sanal ağlar" sekmesine gidin. "Güvenilen Microsoft hizmetlerinin bu depolama hesabına erişmesine Izin ver" seçeneğinin işaretli olduğundan emin olun. Bu seçenek, belirli bir arama hizmeti örneğine, IP güvenlik duvarı kuralları tarafından güvenli hale getirilse bile depolama hesabındaki verilere erişmek için depolama hesabına uygun rol tabanlı erişime (güçlü kimlik doğrulaması) izin verir.

![Güvenilen hizmet özel durumu](media\search-indexer-howto-secure-access\exception.png "Güvenilen hizmet özel durumu")

Hesap, IP güvenlik duvarı kuralları aracılığıyla güvenli hale getirilse bile, Dizin oluşturucular artık depolama hesabındaki verilere erişebilecektir.

## <a name="next-steps"></a>Sonraki adımlar

Azure depolama Dizin oluşturucular hakkında daha fazla bilgi edinin:

- [Azure Blob Dizin Oluşturucu](search-howto-indexing-azure-blob-storage.md)
- [Azure Data Lake Storage 2. Dizin Oluşturucu](search-howto-index-azure-data-lake-storage.md)
- [Azure Tablo Dizin Oluşturucu](search-howto-indexing-azure-tables.md)