---
title: SYNAPSE SQL önerileri
description: SYNAPSE SQL önerileri ve bunların nasıl üretildiği hakkında bilgi edinin
services: synapse-analytics
author: kevinvngo
manager: craigg-msft
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 02/05/2020
ms.author: kevin
ms.reviewer: igorstan
ms.custom: azure-synapse
ms.openlocfilehash: 17877a1ef5d949fbbee080b6157844ac5b516fe7
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80633676"
---
# <a name="synapse-sql-recommendations"></a>SYNAPSE SQL önerileri

Bu makalede, Azure Advisor aracılığıyla sunulan SYNAPSE SQL önerileri açıklanır.  

SQL Analytics veri ambarı iş yükünüzün performans için tutarlı bir şekilde iyileştirildiğinden emin olmak için öneriler sağlar. Öneriler, doğrudan [Azure Portal](https://aka.ms/Azureadvisor)içinde en iyi uygulamaları sunmak Için [Azure Danışmanı](../../advisor/advisor-performance-recommendations.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) ile sıkı bir şekilde tümleşiktir. SQL Analytics, günlük temposunda etkin iş yükünüz için telemetri ve yüzey önerilerini toplar. Desteklenen öneri senaryoları, önerilen eylemlerin nasıl uygulanacağı ile birlikte aşağıda özetlenmiştir.

Önerilerinizi bugün [kontrol](https://aka.ms/Azureadvisor) edebilirsiniz! Şu anda bu özellik yalnızca Gen2 veri ambarlarında geçerlidir.

## <a name="data-skew"></a>Veri eğriltme

Veri eğriltme, iş yükünüzü çalıştırırken ek veri hareketine veya kaynak performans sorunlarına neden olabilir. Aşağıdaki belgelerde, veri eğriliğini belirlemek ve en iyi bir dağıtım anahtarı seçerek oluşmasını engellemek için gösteri açıklanır.

- [Eğriliği tanımla ve Kaldır](sql-data-warehouse-tables-distribute.md#how-to-tell-if-your-distribution-column-is-a-good-choice)

## <a name="no-or-outdated-statistics"></a>Hiç veya güncel olmayan istatistik

En iyi performans istatistikleri, SQL sorgu iyileştiricinin alt sorgu planları oluşturmasına neden olabileceği için sorgu performansını ciddi bir şekilde etkileyebilir. Aşağıdaki belgelerde, istatistikleri oluşturma ve güncelleştirme konusunda en iyi uygulamalar açıklanmaktadır:

- [Tablo istatistikleri oluşturma ve güncelleştirme](sql-data-warehouse-tables-statistics.md)

Bu önerilerin etkilenen tablolarının listesini görmek için aşağıdaki [T-SQL betiğini](https://github.com/Microsoft/sql-data-warehouse-samples/blob/master/samples/sqlops/MonitoringScripts/ImpactedTables)çalıştırın. Danışman, bu önerileri oluşturmak için sürekli olarak aynı T-SQL betiğini çalıştırır.

## <a name="replicate-tables"></a>Tabloları Çoğalt

Çoğaltılan tablo önerileri için, Advisor aşağıdaki fiziksel özelliklere göre tablo adaylarını algılar:

- Çoğaltılan tablo boyutu
- Sütun sayısı
- Tablo dağıtım türü
- Bölüm sayısı

Danışman, tablo erişim sıklığı, ortalama olarak döndürülen satırlar ve veri ambarı boyutunun ve etkinliğinin etrafındaki eşiklerini, yüksek kaliteli önerilerin oluşturulmasını sağlamak için sürekli olarak iş yükü tabanlı buluşsal yöntemler kullanır.

Aşağıda, her bir çoğaltılan tablo önerisi için Azure portal bulabileceğiniz iş yükü tabanlı buluşsal yöntemler açıklanmaktadır:

- Ortalama tarama-son yedi gün içinde her tablo erişimi için tablodan döndürülen satırların ortalama yüzdesi
- Sık okunan, güncelleştirme yok-tablonun, erişim etkinliğini gösterirken son yedi gün içinde güncelleştirilmemiş olduğunu belirtir
- Okuma/güncelleştirme oranı-tablonun son yedi gün içinde güncelleştirildiği zamana göre ne sıklıkta erişildiğine ilişkin oran
- Etkinlik-kullanımı, erişim etkinliğine göre ölçer. Bu, son yedi gün içinde veri ambarı genelinde ortalama tablo erişim etkinliğine göre tablo erişim etkinliğini karşılaştırır.

Şu anda Danışman, en yüksek etkinliğin önceliğini belirleme kümelenmiş columnstore dizinleri ile aynı anda en fazla dört çoğaltılan tablo adayını gösterir.

> [!IMPORTANT]
> Çoğaltılan tablo önerisi, tam kanıt değildir ve hesaba veri taşıma işlemlerini almaz. Bunu bir buluşsal yöntem olarak eklemekle çalışıyoruz ancak bu arada, öneriyi uyguladıktan sonra iş yükünüzü her zaman doğrulamanız gerekir. İş yükünüzün çalışmasına neden olan çoğaltılmış tablo önerilerini buldıysanız lütfen iletişim kurun sqldwadvisor@service.microsoft.com . Çoğaltılan tablolar hakkında daha fazla bilgi edinmek için aşağıdaki [belgeleri](design-guidance-for-replicated-tables.md#what-is-a-replicated-table)ziyaret edin.
