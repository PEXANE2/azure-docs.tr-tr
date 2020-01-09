---
title: Azure Application Insights performans ve yük testi | Microsoft Docs
description: Azure Application Insights performans ve yük testlerini ayarlama
ms.service: azure-monitor
ms.subservice: application-insights
ms.topic: conceptual
author: mrbullwinkle
ms.author: mbullwin
ms.date: 06/19/2019
ms.reviewer: sdash
ms.openlocfilehash: db23fae6bb15e851d22e54b323428c061f55b34f
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75406562"
---
# <a name="performance-testing"></a>Performansı test etme

> [!NOTE]
> Bulut tabanlı yük testi hizmeti kullanımdan kaldırılmıştır. Kullanımdan kaldırma, hizmet kullanılabilirliği ve alternatif hizmetler hakkında daha fazla bilgi [bulabilirsiniz.](https://docs.microsoft.com/azure/devops/test/load-test/overview?view=azure-devops)

Application Insights, Web siteleriniz için yük testleri oluşturmanıza olanak sağlar. [Kullanılabilirlik testleri](monitor-web-app-availability.md)gibi, dünyanın dört bir yanındaki Azure test aracılarından temel istekleri veya [çok adımlı istekleri](availability-multistep.md) gönderebilirsiniz. Performans testleri, en fazla 20.000 dakikaya kadar eşzamanlı 60 kullanıcının benzetimini yapmanıza olanak sağlar.

## <a name="create-an-application-insights-resource"></a>Application Insights kaynağı oluşturma

Bir performans testi oluşturmak için önce bir Application Insights kaynağı oluşturmanız gerekir. Zaten bir kaynak oluşturduysanız bir sonraki bölüme ilerleyin.

Azure portal > **Geliştirici Araçları** > **kaynak oluştur** ' u seçin **ve Application Insights bir** kaynak oluşturun.

## <a name="configure-performance-testing"></a>Performans testini yapılandırma

İlk kez performans testi oluşturuyorsanız, **kuruluş ayarla** ' yı seçin ve performans testleriniz için kaynak olacak bir Azure DevOps organizasyonu seçin.

**Yapılandır**altında **performans testi** ' ne gidin ve **Yeni** ' ye tıklayarak bir test oluşturun.

![En azından web sitenizin URL'sini doldurma](./media/performance-testing/new-performance-test.png)

Temel bir performans testi oluşturmak için, **El Ile test** bir test türü seçin ve testiniz için istenen ayarları doldurun.

|Ayar| En yüksek değer
|----------|------------|
| Kullanıcı Yükü | 20.000 |
| Süre (dakika)  | 60 |  

Testiniz oluşturulduktan sonra **Test Çalıştır**' a tıklayın.

Test tamamlandıktan sonra aşağıdaki sonuçlara benzer sonuçlar görürsünüz:

![Test Sonuçları](./media/performance-testing/test-results.png)

## <a name="configure-visual-studio-web-test"></a>Visual Studio Web testini yapılandırma

Application Insights gelişmiş performans testi özellikleri, Visual Studio performans ve yük testi projelerinin üzerine kurulmuştur.

![Visual Studio ](./media/performance-testing/visual-studio-test.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Çok adımlı Web testleri](availability-multistep.md)
* [URL ping testleri](monitor-web-app-availability.md)
