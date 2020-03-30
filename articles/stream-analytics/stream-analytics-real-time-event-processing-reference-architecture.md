---
title: Azure Akış Analizi'ni kullanarak gerçek zamanlı etkinlik işleme
description: Bu makalede, Azure Akış Analitiği kullanarak gerçek zamanlı olay işleme ve analitik elde etmek için başvuru mimarisi açıklanmaktadır.
author: jseb225
ms.author: jeanb
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 01/24/2017
ms.openlocfilehash: d219b3fcb27b23527c0a651bc8e842a9e036bfc2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75431475"
---
# <a name="reference-architecture-real-time-event-processing-with-microsoft-azure-stream-analytics"></a>Başvuru mimarisi: Microsoft Azure Akış Analizi ile gerçek zamanlı etkinlik işleme
Azure Akış Analytics ile gerçek zamanlı olay işleme için başvuru mimarisi, Microsoft Azure ile gerçek zamanlı bir platformu hizmet (PaaS) akış işleme çözümü olarak dağıtmak için genel bir plan sağlamayı amaçlamaktadır.

## <a name="summary"></a>Özet
Geleneksel olarak, analitik çözümler, verilerin analizden önce depolandığı ETL (ayıklama, dönüştürme, yükleme) ve veri depolama gibi özelliklere dayanmaktadır. Daha hızlı gelen veriler de dahil olmak üzere değişen gereksinimler, bu varolan modeli sınıra itiyor. Depolamadan önce hareketli akışlar içindeki verileri çözümleme yeteneği bir çözümdür ve yeni bir yetenek olmasa da, yaklaşım tüm endüstri dikeylerinde yaygın olarak benimsenmemiştir. 

Microsoft Azure, bir dizi farklı çözüm senaryosunu ve gereksinimini destekleyebilecek kapsamlı bir analiz teknolojileri kataloğu sağlar. Uçtan uca bir çözüm için hangi Azure hizmetlerinin dağıtılanın, tekliflerin genişliği göz önüne alındığında, hangi Azure hizmetlerinin dağıtılanabileceğini seçmek zor olabilir. Bu makale, etkinlik akışı çözümlerini destekleyen çeşitli Azure hizmetlerinin yeteneklerini ve birlikte çalışmasını açıklamak üzere tasarlanmıştır. Ayrıca, müşterilerin bu tür bir yaklaşımdan yararlanabileceği bazı senaryoları da açıklar.

## <a name="contents"></a>İçindekiler
* Yönetici Özeti
* Gerçek Zamanlı Analitik'e Giriş
* Azure'da Gerçek Zamanlı Verilerin Değer Teklifi
* Gerçek Zamanlı Analitik için Yaygın Senaryolar
* Mimari ve Bileşenler
  * Veri Kaynakları
  * Veri Tümleştirme Katmanı
  * Gerçek zamanlı Analitik Katmanı
  * Veri Depolama Katmanı
  * Sunum / Tüketim Katmanı
* Sonuç

**Yazar:** Charles Feddersen, Çözüm Mimarı, Veri Öngörüleri Mükemmellik Merkezi, Microsoft Corporation

**Yayımlandığı Zaman:** Ocak 2015

**Revizyon:** 1.0

**Indirin:** [Microsoft Azure Akış Analizi ile Gerçek Zamanlı Etkinlik İşleme](https://download.microsoft.com/download/6/2/3/623924DE-B083-4561-9624-C1AB62B5F82B/real-time-event-processing-with-microsoft-azure-stream-analytics.pdf)

## <a name="get-help"></a>Yardım alın
Daha fazla yardım için [Azure Akışı Analizi forumuna](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics) katılın

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Akış Analizine Giriş](stream-analytics-introduction.md)
* [Azure Akış Analizi'ni kullanmaya başlama](stream-analytics-real-time-fraud-detection.md)
* [Azure Akış Analizi işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Azure Akış Analizi Sorgu Dili Başvurusu](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference)
* [Azure Akış Analizi Yönetimi REST API'si Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)

