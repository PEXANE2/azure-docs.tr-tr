---
title: Azure Application Insights ile performans ve yük testi | Microsoft Dokümanlar
description: Azure Application Insights ile performans ve yükleme testleri ayarlama
ms.topic: conceptual
ms.date: 06/19/2019
ms.reviewer: sdash
ms.openlocfilehash: 0bfb60b876add70be6cbcd3d726fb3317a8dfcf9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77669871"
---
# <a name="performance-testing"></a>Performansı test etme

> [!NOTE]
> Bulut tabanlı yük testi hizmeti amortismana kaldırıldı. Amortisman, hizmet kullanılabilirliği ve alternatif hizmetler hakkında daha fazla bilgiyi [burada](https://docs.microsoft.com/azure/devops/test/load-test/overview?view=azure-devops)bulabilirsiniz.

Application Insights, web siteleriniz için yük testleri oluşturmanıza olanak tanır. [Kullanılabilirlik testleri](monitor-web-app-availability.md)gibi, dünyanın dört bir yanındaki Azure test aracılarından temel istekler veya [çok adımlı istekler](availability-multistep.md) gönderebilirsiniz. Performans testleri, 20.000'e kadar eşzamanlı kullanıcıyı 60 dakikaya kadar simüle etmenizi sağlar.

## <a name="create-an-application-insights-resource"></a>Application Insights kaynağı oluşturma

Performans testi oluşturmak için öncelikle bir Uygulama Öngörüleri kaynağı oluşturmanız gerekir. Zaten bir kaynak oluşturduysanız sonraki bölüme devam edin.

Azure portalından, **bir kaynak** > **Geliştirici Araçları** > **Uygulama Öngörüleri** Oluştur'u seçin ve bir Uygulama Öngörüleri kaynağı oluşturun.

## <a name="configure-performance-testing"></a>Performans testini yapılandırma

Performans testi ni ilk kez **oluşturuyorsanız, Organizasyon Ayarla'yı** seçin ve performans testlerinizin kaynağı olması için bir Azure DevOps kuruluşu seçin.

**Yapılandırma**altında, **Performans Testi'ne** gidin ve test oluşturmak için **Yeni'yi** tıklatın.

![En azından web sitenizin URL'sini doldurma](./media/performance-testing/new-performance-test.png)

Temel bir performans testi oluşturmak için, bir test türü **Manuel Test** seçin ve testiniz için istenen ayarları doldurun.

|Ayar| En yüksek değer
|----------|------------|
| Kullanıcı Yükü | 20.000 |
| Süre (Dakika)  | 60 |  

Testiniz oluşturulduktan sonra **Testi Çalıştır'ı**tıklatın.

Test tamamlandıktan sonra, aşağıdaki sonuçlara benzer sonuçlar görürsünüz:

![Test Sonuçları](./media/performance-testing/test-results.png)

## <a name="configure-visual-studio-web-test"></a>Visual Studio web testini yapılandır

Application Insights gelişmiş performans testi yetenekleri Visual Studio performans ve yük testi projelerinin üzerine inşa edilmiştir.

![Visual Studio ](./media/performance-testing/visual-studio-test.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Çok adımlı web testleri](availability-multistep.md)
* [Url ping testleri](monitor-web-app-availability.md)
