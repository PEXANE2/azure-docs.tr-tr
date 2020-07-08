---
title: Ortamınızda saklama Yapılandırma-Azure Time Series Insights | Microsoft Docs
description: Azure Time Series Insights ortamınızda bekletmenin nasıl yapılandırılacağını öğrenin.
ms.service: time-series-insights
services: time-series-insights
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.workload: big-data
ms.topic: conceptual
ms.date: 04/16/2020
ms.custom: seodec18
ms.openlocfilehash: 524e47e5be142b720687ad48ee9407da23284bd4
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "81605061"
---
# <a name="configuring-retention-in-time-series-insights"></a>Time Series Insights bekletme yapılandırma

Bu makalede, Azure Time Series Insights ' de **veri saklama süresi** ve **depolama sınırı aşıldı davranışının** nasıl yapılandırılacağı açıklanır.

## <a name="summary"></a>Özet

Her Azure Time Series Insights ortamının **veri saklama süresini**yapılandırma ayarı vardır. Değer 1 ile 400 gün arasında yayılır. Veriler ortam depolama kapasitesi veya Bekletme süresine (1-400) göre silinir, hangisi önce gelir.

Her Time Series Insights ortamında, ek bir **depolama sınırı aşıldı davranışı**vardır. Bu ayar, bir ortamın maksimum kapasitesine ulaşıldığında giriş ve temizleme davranışını denetler. Aralarından seçim yapabileceğiniz iki davranış vardır:

- **Eski verileri temizle** (varsayılan)
- **Girişi Duraklat**

Bu ayarları daha iyi anlamak için ayrıntılı bilgiler için [Time Series Insights ' de saklama](time-series-insights-concepts-retention.md)süresini gözden geçirin.  

## <a name="configure-data-retention"></a>Veri saklamayı yapılandırma

1. [Azure portalında](https://portal.azure.com) oturum açın.

1. Mevcut Time Series Insights ortamınızı bulun. Azure portal sol tarafındaki menüden **tüm kaynaklar** ' ı seçin. Zaman Serisi Görüşleri ortamınızı seçin.

1. **Ayarlar** başlığı altında **depolama yapılandırması**' nı seçin.

    [![Ayarlar altında depolama yapılandırması ' nı seçin.](media/data-retention/configure-data-retention.png)](media/data-retention/configure-data-retention.png#lightbox)

1. Kaydırma çubuğunu kullanarak bekletme 'yi yapılandırmak için **veri saklama süresini (gün olarak)** seçin veya metin kutusuna bir sayı yazın.

1. Bu yapılandırma, verileri depolamak için maksimum veri olayı miktarını ve toplam depolama kapasitesini etkilediğinden **Kapasite** ayarını dikkate alın.

1. **Depolama sınırı aşıldı davranış** ayarını değiştirin. **Eski verileri temizle** veya giriş davranışını **Duraklat** ' ı seçin.

    [![Girişi duraklatma-kabul etme ve kaydetme.](media/data-retention/pause-ingress-accept-and-save.png)](media/data-retention/pause-ingress-accept-and-save.png#lightbox)

1. Veri kaybından oluşan olası riskleri anlamak için belgeleri gözden geçirin. Değişiklikleri yapılandırmak için **Kaydet** ' i seçin.

## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi için [Time Series Insights ' de saklama süresini anlama](time-series-insights-concepts-retention.md)konusunu gözden geçirin.

- [Time Series Insights ortamınızı ölçeklendirmeyi](time-series-insights-how-to-scale-your-environment.md)öğrenin.

- [Ortamınızı planlama](time-series-insights-environment-planning.md)hakkında bilgi edinin.
