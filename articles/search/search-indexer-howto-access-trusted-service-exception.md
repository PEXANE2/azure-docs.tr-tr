---
title: Güvenilen hizmet özel durumu aracılığıyla depolamaya erişime izin ver
titleSuffix: Azure Cognitive Search
description: Depolama hesaplarından güvenli bir şekilde veri erişimi için güvenilen hizmet özel durumunun nasıl ayarlanacağını anlatan yönergeler.
manager: nitinme
author: arv100kri
ms.author: arjagann
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 09/07/2020
ms.openlocfilehash: f901833caf0623de643e0372c53658fa7da8c8be
ms.sourcegitcommit: 4a7a4af09f881f38fcb4875d89881e4b808b369b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/04/2020
ms.locfileid: "89463910"
---
# <a name="accessing-data-in-storage-accounts-securely-via-trusted-service-exception"></a>Güvenilen hizmet özel durumu aracılığıyla depolama hesaplarındaki verilere güvenli bir şekilde erişme

Depolama hesaplarında verilere erişen Dizin oluşturucular, verilere güvenli bir şekilde erişmek için [güvenilir hizmet özel durum](https://docs.microsoft.com/azure/storage/common/storage-network-security#exceptions) özelliğinden daha fazla kullanabilir. Bu mekanizma, [IP güvenlik duvarı kuralları aracılığıyla Dizin Oluşturucu erişimi](search-indexer-howto-access-ip-restricted.md) izni veren müşterileri, depolama hesaplarındaki verilere erişim için basit, güvenli ve ücretsiz bir alternatif sağlar.

> [!NOTE]
> Bir arama hizmetinin sistem tarafından atanan kimliği üzerinden depolama hesabına erişmek hala önizlemededir. Bu önizleme özelliği, bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez.

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
