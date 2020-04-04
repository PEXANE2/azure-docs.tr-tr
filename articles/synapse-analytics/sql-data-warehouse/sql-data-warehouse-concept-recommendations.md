---
title: Synapse SQL önerileri
description: Synapse SQL önerileri ve bunların nasıl oluşturulduğu hakkında bilgi edinin
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
ms.sourcegitcommit: d597800237783fc384875123ba47aab5671ceb88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80633676"
---
# <a name="synapse-sql-recommendations"></a>Synapse SQL önerileri

Bu makalede, Azure Danışmanı aracılığıyla sunulan Synapse SQL önerileri açıklanmaktadır.  

SQL Analytics, veri ambarı iş yükünüzün performans için sürekli olarak optimize edilmesini sağlamak için öneriler sağlar. Öneriler, doğrudan [Azure portalında](https://aka.ms/Azureadvisor)en iyi uygulamaları sağlamak için [Azure Advisor](../../advisor/advisor-performance-recommendations.md?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) ile sıkı bir şekilde entegre edilmiştir. SQL Analytics, aktif iş yükünüz için günlük iş yükünüz için telemetri ve yüzey önerileri toplar. Desteklenen öneri senaryoları, önerilen eylemlerin nasıl uygulanacağıyla birlikte aşağıda özetlenmiştir.

[Önerilerinizi](https://aka.ms/Azureadvisor) hemen kontrol edebilirsiniz! Şu anda bu özellik yalnızca Gen2 veri ambarları için geçerlidir.

## <a name="data-skew"></a>Veri eğriltme

Veri eğriliği, iş yükünüzü çalıştırırken ek veri hareketine veya kaynak darboğazlarına neden olabilir. Aşağıdaki belgeler, verileri eğriltme tanımlamak ve en iyi dağıtım anahtarı nı seçerek bunun olmasını önlemek için gösteriyi açıklar.

- [Eğriliyi tanımlayın ve kaldırın](sql-data-warehouse-tables-distribute.md#how-to-tell-if-your-distribution-column-is-a-good-choice)

## <a name="no-or-outdated-statistics"></a>Hayır veya güncel olmayan istatistikler

Sql sorgu optimize edicinin alt optimal sorgu planları oluşturmasına neden olabileceğinden, en uygun olmayan istatistiklere sahip olmak sorgu performansını önemli ölçüde etkileyebilir. Aşağıdaki belgeler, istatistik oluşturma ve güncelleştirme yle ilgili en iyi uygulamaları açıklar:

- [Tablo istatistikleri oluşturma ve güncelleştirme](sql-data-warehouse-tables-statistics.md)

Bu önerilerden etkilenen tabloların listesini görmek için aşağıdaki [T-SQL komut dosyasını](https://github.com/Microsoft/sql-data-warehouse-samples/blob/master/samples/sqlops/MonitoringScripts/ImpactedTables)çalıştırın. Danışman, bu önerileri oluşturmak için sürekli olarak aynı T-SQL komut dosyasını çalıştırın.

## <a name="replicate-tables"></a>Tabloları çoğaltma

Yinelenen tablo önerileri için Danışman tablo adaylarını aşağıdaki fiziksel özelliklere göre algılar:

- Çoğaltılan tablo boyutu
- Sütun sayısı
- Tablo dağılım türü
- Bölüm sayısı

Danışman, yüksek kaliteli önerilerin oluşturulduğundan emin olmak için tablo erişim sıklığı, ortalama olarak döndürülen satırlar ve veri ambarı boyutu ve etkinliği eşikleri gibi iş yükü tabanlı buluşsal lardan sürekli olarak yararlanır.

Aşağıda, çoğaltılan her tablo önerisi için Azure portalında bulabileceğiniz iş yükü tabanlı buluşsal uygulamalar açıklanmaktadır:

- Son yedi gün içinde her tablo erişimi için tablodan döndürülen satırların ortalama yüzdesi
- Sık okuma, güncelleme yok - erişim etkinliğini gösterirken tablonun son yedi gün içinde güncelleştirilemediğini gösterir
- Okuma/güncelleme oranı - tablonun son yedi gün içinde güncelleştirildiğindene göre ne sıklıkta erişildioranı
- Etkinlik - erişim etkinliğine göre kullanımı ölçer. Bu, tablo erişim etkinliğini, son yedi gün içinde veri ambarı genelindeki ortalama tablo erişim etkinliğiyle karşılaştırır.

Şu anda Danışman, en yüksek etkinliği önceliklendiren kümelenmiş sütun deposu dizinleriyle aynı anda yalnızca dört çoğaltılan tablo adayını gösterir.

> [!IMPORTANT]
> Çoğaltılan tablo önerisi tam kanıt değildir ve veri hareketi işlemlerini hesaba katmaz. Bunu sezgisel olarak eklemeye çalışıyoruz, ancak bu arada, öneriyi uyguladıktan sonra her zaman iş yükünüzü doğrulamanız gerekir. İş sqldwadvisor@service.microsoft.com yükünüzün gerilemesine neden olan çoğaltılmış tablo önerileri bulduğunuzda lütfen iletişim kurun. Çoğaltılan tablolar hakkında daha fazla bilgi edinmek için aşağıdaki [belgeleri](design-guidance-for-replicated-tables.md#what-is-a-replicated-table)ziyaret edin.
