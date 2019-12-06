---
title: Azure Cosmos DB çoklu yönetici avantajları
description: Azure Cosmos DB çoklu yönetici 'nin avantajlarını, gecikme ve SLA gereksinimlerinin tek ve birden çok yazma konumlarında karşılaştırılmasını anlayın.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 12/02/2019
ms.author: mjbrown
ms.openlocfilehash: b21b6ba82ba1ada0103501b8beeca270df86abd9
ms.sourcegitcommit: 9405aad7e39efbd8fef6d0a3c8988c6bf8de94eb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2019
ms.locfileid: "74872018"
---
# <a name="understand-multi-master-benefits-in-azure-cosmos-db"></a>Azure Cosmos DB 'de çok asıllı avantajları anlayın

Cosmos DB hesap işleçleri, uygulamaları için gecikme süresi, kullanılabilirlik ve RTO gereksinimlerini sağlamak üzere uygun küresel dağıtım yapılandırmasını seçmelidir. Birden çok yazma konumuyla yapılandırılan Azure Cosmos hesapları, bölgesel bir olağanüstü durumda% 99,999 yazma kullanılabilirlik SLA 'sı, 10 MS yazma gecikme süresi için 99. yüzdebirlik ve RTO = 0 <.

## <a name="comparison-of-features"></a>Özelliklerin karşılaştırması

|Uygulama gereksinimi|Birden çok yazma konumu|Tek bir yazma konumu|Not|
|---|---|---|---|
|P99 adresindeki 10ms < gecikme süresi SLA 'Sı yazın|**Evet**|Hayır|Tek bir yazma konumu olan hesaplar, her yazma için ek bölgeler arası ağ gecikme süresi doğurur.|
|P99 adresindeki 10ms < gecikme süresi SLA 'sını okuyun|**Evet**|Yes| |
|% 99,999 SLA 'Sı yaz|**Evet**|Hayır|Tek yazma konumu garanti eden hesaplar% 99,99 SLA|
|RTO = 0|**Evet**|Hayır|Bölgesel felaetler durumunda yazma işlemleri için sıfır süre. Tek bir yazma konumu olan hesaplar 15 dakikadan fazla.|

## <a name="next-steps"></a>Sonraki Adımlar

Azure Cosmos hesabınızda EnableMultipleWriteLocations 'ı hala devre dışı bırakmak istiyorsanız lütfen [bir destek bileti açın](https://azure.microsoft.com/support/create-ticket/).
