---
title: Azure Time Series Insights ortamınızda saklama yapılandırma | Microsoft Docs
description: Bu makalede Azure Time Series Insights ortamınızda bekletmenin nasıl yapılandırılacağı açıklanır.
ms.service: time-series-insights
services: time-series-insights
author: ashannon7
ms.author: dpalled
manager: cshankar
ms.reviewer: jasonh, kfile
ms.workload: big-data
ms.topic: conceptual
ms.date: 08/09/2019
ms.custom: seodec18
ms.openlocfilehash: e5cc1489af1dce3a9a57b96a3240c63e0395c33a
ms.sourcegitcommit: 124c3112b94c951535e0be20a751150b79289594
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/10/2019
ms.locfileid: "68947237"
---
# <a name="configuring-retention-in-time-series-insights"></a>Time Series Insights bekletme yapılandırma

Bu makalede, Azure Time Series Insights ' de **veri saklama süresi** ve **depolama sınırı aşıldı davranışının** nasıl yapılandırılacağı açıklanır.

## <a name="summary"></a>Özet

Her Time Series Insights (TSI) ortamının **veri saklama süresini**yapılandırma ayarı vardır. Değer 1 ile 400 gün arasında yayılır. Veriler ortam depolama kapasitesi veya Bekletme süresine (1-400) göre silinir, hangisi önce gelir.

Her bir TSI ortamının ek ayar **depolama sınırı aşıldı davranışı**vardır. Bu ayar, bir ortamın maksimum kapasitesine ulaşıldığında giriş ve temizleme davranışını denetler. Aralarından seçim yapabileceğiniz iki davranış vardır:

- **Eski verileri temizle** varsayılanını
- **Girişi Duraklat**

Bu ayarları daha iyi anlamak için ayrıntılı bilgiler için [Time Series Insights ' de saklama](time-series-insights-concepts-retention.md)süresini gözden geçirin.  

## <a name="configure-data-retention"></a>Veri saklamayı yapılandırma

1. [Azure Portal](https://portal.azure.com) oturum açın.

1. Mevcut Time Series Insights ortamınızı bulun. Azure portal sol tarafındaki menüden **tüm kaynaklar** ' ı seçin. Zaman Serisi Görüşleri ortamınızı seçin.

1. **Ayarlar** başlığı altında **Yapılandır**' ı seçin.

1. Kaydırma çubuğunu kullanarak bekletme 'yi yapılandırmak için **veri saklama süresini** seçin veya metin kutusuna bir sayı yazın.

1. Bu yapılandırma, verileri depolamak için maksimum veri olayı miktarını ve toplam depolama kapasitesini etkilediğinden **Kapasite** ayarını dikkate alın.

1. **Depolama sınırı aşıldı davranış** ayarını değiştirin. **Eski verileri temizle** veya giriş davranışını **Duraklat** ' ı seçin.

1. Değişiklikleri yapılandırmak için **Kaydet** ' i seçin.

## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi için [Time Series Insights ' de saklama süresini anlama](time-series-insights-concepts-retention.md)konusunu gözden geçirin.