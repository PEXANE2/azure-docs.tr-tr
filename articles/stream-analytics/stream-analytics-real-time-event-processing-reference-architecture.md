---
title: Azure Stream Analytics kullanarak gerçek zamanlı olay işleme
description: Bu makalede, Azure Stream Analytics kullanarak gerçek zamanlı olay işleme ve analizine ulaşmak için başvuru mimarisi açıklanmaktadır.
author: jseb225
ms.author: jeanb
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 01/24/2017
ms.openlocfilehash: 23c7112639a64097d95df29bde16636702837f9e
ms.sourcegitcommit: 0b80a5802343ea769a91f91a8cdbdf1b67a932d3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/25/2020
ms.locfileid: "83832988"
---
# <a name="reference-architecture-real-time-event-processing-with-microsoft-azure-stream-analytics"></a>Başvuru Mimarisi: Microsoft Azure Stream Analytics ile gerçek zamanlı olay işleme
Azure Stream Analytics ile gerçek zamanlı olay işleme için başvuru mimarisi, Microsoft Azure ile gerçek zamanlı bir hizmet olarak platform (PaaS) akış işleme çözümü dağıtmaya yönelik bir genel şema sağlamaktır.

## <a name="summary"></a>Özet
Geleneksel olarak, analiz çözümleri ETL (Ayıkla, Dönüştür, yük) ve veri depolama gibi, verilerin analizden önce depolanacağı özellikleri temel alır. Daha hızlı ulaşan veriler de dahil olmak üzere değişiklik değişikliği, bu mevcut modeli sınıra itliyor. Depolama 'dan önce taşınan akışlardaki verileri çözümleme özelliği bir çözümdür ve yeni bir özellik olmasa da yaklaşım tüm sektör verilerlerinde yaygın olarak benimsenmez. 

Microsoft Azure, farklı çözüm senaryoları ve gereksinimlerinin bir dizisini destekleyebilen bir dizi analiz teknolojisinden oluşan kapsamlı bir katalog sağlar. Uçtan uca bir çözüm için hangi Azure hizmetlerinin dağıtılacağını seçmek, tekliflerin kapsamını belirlemek için bir zorluk olabilir. Bu sayfa, bir olay akışı çözümünü destekleyen çeşitli Azure hizmetlerinin yeteneklerini ve birlikte çalışmasını tanımlamaya yönelik olarak tasarlanmıştır. Ayrıca, müşterilerin bu tür bir yaklaşımdan faydalanabilir bazı senaryolar da açıklanmaktadır.

## <a name="contents"></a>İçindekiler
* Yönetici Özeti
* Gerçek zamanlı Analize Giriş
* Azure 'daki gerçek zamanlı verilerin değer teklifi
* Gerçek zamanlı analiz için yaygın senaryolar
* Mimari ve bileşenler
  * Veri Kaynakları
  * Veri tümleştirme katmanı
  * Gerçek zamanlı analiz katmanı
  * Veri depolama katmanı
  * Sunum/tüketim katmanı
* Sonuç

**Yazar:** Charles Feddersen, çözüm mimarı, üstün veri öngörüleri Merkezi, Microsoft Corporation

**Yayımlanma tarihi:** Ocak 2015

**Düzeltme:** 1,0

**İndirin:** [Microsoft Azure Stream Analytics Ile gerçek zamanlı olay işleme](https://download.microsoft.com/download/6/2/3/623924DE-B083-4561-9624-C1AB62B5F82B/real-time-event-processing-with-microsoft-azure-stream-analytics.pdf)

## <a name="get-help"></a>Yardım alın
Daha fazla yardım için [Microsoft Q&soru sayfasını deneyin Azure Stream Analytics](https://docs.microsoft.com/answers/topics/azure-stream-analytics.html)

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Stream Analytics giriş](stream-analytics-introduction.md)
* [Azure Akış Analizi'ni kullanmaya başlama](stream-analytics-real-time-fraud-detection.md)
* [Azure Akış Analizi işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Azure Akış Analizi Sorgu Dili Başvurusu](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference)
* [Azure Akış Analizi Yönetimi REST API'si Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)

