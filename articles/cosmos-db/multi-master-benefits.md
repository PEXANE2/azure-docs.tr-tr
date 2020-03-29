---
title: Azure Cosmos DB çoklu master avantajları
description: Azure Cosmos DB'de çoklu ustanın avantajlarını, tek ve birden çok yazma konumlarında gecikme ve SLA gereksinimlerini nkarşılayabildiklerini anlayın.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 12/02/2019
ms.author: mjbrown
ms.openlocfilehash: b21b6ba82ba1ada0103501b8beeca270df86abd9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74872018"
---
# <a name="understand-multi-master-benefits-in-azure-cosmos-db"></a>Azure Cosmos DB'de çok yönlü avantajları anlama

Cosmos DB hesap operatörleri, uygulamaları için gecikme, kullanılabilirlik ve RTO gereksinimlerini sağlamak için uygun küresel dağıtım yapılandırmasını seçmelidir. Birden çok yazma konumuyla yapılandırılan Azure Cosmos hesapları, %99,999 yazma kullanılabilirliği SLA, <10 ms 99 yüzdelik bir oranda gecikme sla'sı ve bölgesel bir felakette RTO = 0 dahil olmak üzere tek yazma konumuna sahip hesaplar adedine göre önemli avantajlar sunar.

## <a name="comparison-of-features"></a>Özelliklerin karşılaştırılması

|Uygulama Gereksinimi|Birden Çok Yazma Konumu|Tek Yazma Konumu|Not|
|---|---|---|---|
|P99'da <10ms gecikme SLA yazın|**Evet**|Hayır|Tek Yazma Konumu olan hesaplar, her yazı için ek bölgeler arası ağ gecikmesi sağlar.|
|P99 <10ms gecikme SLA okuyun|**Evet**|Evet| |
|99.999% sla yazın|**Evet**|Hayır|Tek Yazma Konumu garanti SLA ile hesaplar 99,99%|
|RTO = 0|**Evet**|Hayır|Bölgesel felaketler durumunda yazmak için sıfır aşağı zaman. Tek yazma konumu olan hesaplar 15 dk RTO'ya sahiptir.|

## <a name="next-steps"></a>Sonraki Adımlar

Azure Cosmos hesabınızda EnableMultipleWriteLocations'ı yine de devre dışı atmak istiyorsanız, lütfen [bir destek bileti açın.](https://azure.microsoft.com/support/create-ticket/)
