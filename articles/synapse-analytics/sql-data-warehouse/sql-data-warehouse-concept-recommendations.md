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
ms.openlocfilehash: c4bbd98cc28b242be5310fab76521a80de8fcb7c
ms.sourcegitcommit: 3c318f6c2a46e0d062a725d88cc8eb2d3fa2f96a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80584117"
---
# <a name="synapse-sql-recommendations"></a>Synapse SQL önerileri

Bu makalede, Azure Danışmanı aracılığıyla sunulan Synapse SQL önerileri açıklanmaktadır.  

Synapse SQL, veri ambarı iş yükünüzün performans için sürekli olarak optimize edilmesini sağlamak için öneriler sağlar. Öneriler, doğrudan [Azure portalında](https://aka.ms/Azureadvisor)en iyi uygulamaları sağlamak için [Azure Advisor](https://docs.microsoft.com/azure/advisor/advisor-performance-recommendations) ile sıkı bir şekilde entegre edilmiştir. Synapse SQL, günlük iş yükünüz için telemetri ve yüzey önerileri toplar. Desteklenen öneri senaryoları, önerilen eylemlerin nasıl uygulanacağıyla birlikte aşağıda özetlenmiştir.

[Önerilerinizi](https://aka.ms/Azureadvisor) hemen kontrol edebilirsiniz! Şu anda bu özellik yalnızca Gen2 veri ambarları için geçerlidir. 

## <a name="data-skew"></a>Veri eğriltme

Veri eğriliği, iş yükünüzü çalıştırırken ek veri hareketine veya kaynak darboğazlarına neden olabilir. Aşağıdaki belgeler, verileri eğriltme tanımlamak ve en iyi dağıtım anahtarı nı seçerek bunun olmasını önlemek için gösteriyi açıklar.

- [Eğriliyi tanımlayın ve kaldırın](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-tables-distribute#how-to-tell-if-your-distribution-column-is-a-good-choice) 

## <a name="no-or-outdated-statistics"></a>Hayır veya güncel olmayan istatistikler

Sql sorgu optimize edicinin alt optimal sorgu planları oluşturmasına neden olabileceğinden, en uygun olmayan istatistiklere sahip olmak sorgu performansını önemli ölçüde etkileyebilir. Aşağıdaki belgeler, istatistik oluşturma ve güncelleştirme yle ilgili en iyi uygulamaları açıklar:

- [Tablo istatistikleri oluşturma ve güncelleştirme](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-tables-statistics)

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
> Çoğaltılan tablo önerisi tam kanıt değildir ve veri hareketi işlemlerini hesaba katmaz. Bunu sezgisel olarak eklemeye çalışıyoruz, ancak bu arada, öneriyi uyguladıktan sonra her zaman iş yükünüzü doğrulamanız gerekir. İş sqldwadvisor@service.microsoft.com yükünüzün gerilemesine neden olan çoğaltılmış tablo önerileri bulduğunuzda lütfen iletişim kurun. Çoğaltılan tablolar hakkında daha fazla bilgi edinmek için aşağıdaki [belgeleri](https://docs.microsoft.com/azure/sql-data-warehouse/design-guidance-for-replicated-tables#what-is-a-replicated-table)ziyaret edin.
