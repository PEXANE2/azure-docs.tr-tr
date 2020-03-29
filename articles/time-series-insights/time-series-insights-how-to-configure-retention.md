---
title: Ortamınızda bekletme yapılandırma nasıl yapılandırılır - Azure Zaman Serisi Öngörüleri | Microsoft Dokümanlar
description: Azure Zaman Serisi Öngörüleri ortamınızda bekletme yi nasıl yapılandırıştırmayı öğrenin.
ms.service: time-series-insights
services: time-series-insights
author: deepakpalled
ms.author: dpalled
manager: cshankar
ms.workload: big-data
ms.topic: conceptual
ms.date: 01/17/2020
ms.custom: seodec18
ms.openlocfilehash: 3fdc007caaa4cb79f6083599a5bc176bc022fb03
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76278609"
---
# <a name="configuring-retention-in-time-series-insights"></a>Zaman Serisi Öngörülerinde bekletme yapılandırma

Bu makalede, Azure Zaman Serisi Öngörüleri'nde **Veri saklama süresi** ve Depolama sınırı nın **aşıldığı davranışların** nasıl yapılandırılabildiğini açıklanmaktadır.

## <a name="summary"></a>Özet

Her Azure Zaman Serisi Öngörüler ortamının **Veri saklama süresini**yapılandırmak için bir ayarı vardır. Değer 1 ile 400 güne kadar uzanır. Veriler, hangisi önce gerçekleşirse, ortam depolama kapasitesine veya bekletme süresine (1-400) göre silinir.

Her Zaman Serisi Öngörüler ortamıek bir ayar **Depolama sınırı aşan davranış**vardır. Bu ayar, bir ortamın maksimum kapasitesine ulaşıldığında giriş ve temizleme davranışını denetler. Aralarından seçim yapabileceğiniz iki davranış vardır:

- **Eski verileri temizleme** (varsayılan)
- **Duraklatma girişi**

Bu ayarları daha iyi anlamak için ayrıntılı bilgi [için, Zaman Serisi Öngörüleri'nde Anlama saklamayı gözden geçirin.](time-series-insights-concepts-retention.md)  

## <a name="configure-data-retention"></a>Veri saklamayı yapılandırma

1. [Azure portalında](https://portal.azure.com)oturum açın.

1. Mevcut Time Series Öngörüleri ortamınızı bulun. Azure portalının sol tarafındaki menüdeki **Tüm kaynakları** seçin. Zaman Serisi Görüşleri ortamınızı seçin.

1. **Ayarlar** başlığı altında **Depolama Yapılandırması'nı**seçin.

    [![Ayarlar altında Depolama Yapılandırması'nı seçin](media/data-retention/configure-data-retention.png)](media/data-retention/configure-data-retention.png#lightbox)

1. Kaydırıcı çubuğunu kullanarak bekletme süresini yapılandırmak veya metin kutusuna bir sayı yazmak için **Veri bekletme süresini (günler içinde)** seçin.

1. Bu yapılandırma maksimum veri olayı miktarını ve veri depolama için toplam depolama kapasitesini etkilediğiiçin **Kapasite** ayarını not edin.

1. **Depolama sınırını aşan davranış** ayarını geçiş. **Eski verileri Temizleme** veya Ara **verme** davranışını seçin.

    [![Girişi duraklatın - kabul edin ve kaydedin.](media/data-retention/pause-ingress-accept-and-save.png)](media/data-retention/pause-ingress-accept-and-save.png#lightbox)

1. Veri kaybının olası risklerini anlamak için belgeleri gözden geçirin. Değişiklikleri yapılandırmak için **Kaydet'i** seçin.

## <a name="next-steps"></a>Sonraki adımlar

- Daha fazla bilgi için, [Zaman Serisi Öngörüleri'nde anlama tutmayı gözden geçirin.](time-series-insights-concepts-retention.md)

- [Zaman Serisi Öngörüleri ortamınızı nasıl ölçeklendireceklerini](time-series-insights-how-to-scale-your-environment.md)öğrenin.

- [Çevrenizi planlama](time-series-insights-environment-planning.md)hakkında bilgi edinin.
