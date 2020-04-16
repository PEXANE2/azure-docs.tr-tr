---
title: Azure Veri Fabrikası'nda yinelemeli geliştirme ve hata ayıklama
description: Azure portalında veri fabrikası boru hatlarını yinelemeli olarak nasıl geliştirip hata ayıklamayı öğrenin.
ms.date: 09/26/2018
ms.topic: conceptual
ms.service: data-factory
services: data-factory
documentationcenter: ''
ms.workload: data-services
author: djpmsft
ms.author: daperlov
manager: anandsub
ms.openlocfilehash: 49e406f0730189439bb1c2529b23d8ad6d760a2f
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81411523"
---
# <a name="iterative-development-and-debugging-with-azure-data-factory"></a>Azure Data Factory ile yinelemeli geliştirme ve hata ayıklama
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Azure Veri Fabrikası, Veri Fabrikası ardışık hatlarını yinelemeli olarak geliştirmenize ve hata ayıklamanıza olanak tanır.

Bu özelliğin sekiz dakikalık tanıtımı ve gösterimi için aşağıdaki videoyu izleyin:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Iterative-development-and-debugging-with-Azure-Data-Factory/player]

## <a name="iterative-debugging-features"></a>Yinelemeli hata ayıklama özellikleri
Tek bir kod satırı yazmadan ardışık hatlar oluşturun ve boru hattı tuvalindeki **Hata Ayıklama** özelliğini kullanarak test çalıştırmaları yapın.

![Boru hattı tuvalinde hata ayıklama özelliği](media/iterative-development-debugging/iterative-development-image1.png)

Test çalışmalarınızın sonuçlarını boru hattı tuvalinin **Çıktı** penceresinde görüntüleyin.

![Boru hattı tuvalinin çıkış penceresi](media/iterative-development-debugging/iterative-development-image2.png)

Bir test çalışması başarılı olduktan sonra, ardınıza daha fazla etkinlik ekleyin ve yinelemeli bir şekilde hata ayıklama devam edin. Devam ederken bir test çalışmasını da **iptal** edebilirsiniz.

![Test çalışmasını iptal etme](media/iterative-development-debugging/iterative-development-image3.png)

Test çalışmaları yaptığınızda, Hata Ayıklama'yı seçmeden önce değişikliklerinizi **Debug**veri fabrikasında yayımlamanız gerekmez. Bu özellik, veri fabrikası iş akışını güncelleştirmeden önce değişikliklerin beklendiği gibi çalıştığından emin olmak istediğiniz senaryolarda yararlıdır.

> [!IMPORTANT]
> Hata **Ayıklama'yı** seçmek aslında ardışık bölümü çalıştırıyor. Örneğin, ardışık hatlar kopyalama etkinliği içeriyorsa, test çalışması verileri kaynaktan hedefe kopyalar. Sonuç olarak, hata ayıklama yaparken kopyalama etkinliklerinizde ve diğer etkinliklerde test klasörleri kullanmanızı öneririz. Ardışık işlemi debugged sonra, normal işlemlerde kullanmak istediğiniz gerçek klasörlere geçin.

## <a name="visualizing-debug-runs"></a>Hata ayıklama çalıştırmalarını görselleştirme

Veri fabrikanız için devam eden tüm hata ayıklama çalıştırmalarını tek bir yerde görselleştirebilirsiniz. Sayfanın sağ üst köşesinde **hata ayıklama çalışır'ı seçin.** Bu özellik, alt aksama akdesi yardımcı hatları için yardımcı ardışık ardışık hatlar var ve tüm etkin hata ayıklama çalışır görmek için tek bir görünüm istediğiniz senaryolarda yararlıdır.

![Etkin hata ayıklama çalışır simgesini görüntüle'yi seçin](media/iterative-development-debugging/view-debug-runs-image1.png)

![Etkin hata ayıklama çalıştırmalarının örnek listesi](media/iterative-development-debugging/view-debug-runs-image2.png)

Etkin Veri Akışı hata ayıklama oturumları varsa, bu oturumlar etkin hata ayıklama penceresinin alt kısmında görünür. Etkin bir veri akışı oturumu seçebilir ve ilgili kümeyi durdurabilirsiniz.

![Etkin veri akışı hata ayıklama çalıştırmalarının örnek listesi](media/data-flow/dfsessions.png)

## <a name="monitoring-debug-runs"></a>Hata ayıklama çalıştırmalarını izleme

**Hata Ayıklama** özelliğiyle başlatılan test çalıştırmaları, **Monitör** sekmesindeki listede kullanılamaz. Yalnızca **Tetikle Şimdi**, **Zamanlama**veya **Yuvarlanan Pencere** tetikleyicileri ile tetiklenen çalıştırmaları **Monitör** sekmesinde görebilirsiniz. Hata **Ayıklama** özelliğiyle başlatılan son test çalışmasını, ardışık hatun tuvalinin **Çıktı** penceresinde görebilirsiniz.

## <a name="setting-breakpoints-for-debugging"></a>Hata ayıklama için kesme noktaları ayarlama

Veri Fabrikası, boru hattı tuvalinde belirli bir faaliyete ulaşana kadar hata ayıklamanızı da sağlar. Yalnızca, test etmek istediğinize kadar aktiviteye bir kesme noktası koyun ve **Hata Ayıklama'yı**seçin. Veri Fabrikası, testin yalnızca boru hattı tuvalindeki kesme noktası etkinliğine kadar çalıştırılmasını sağlar. Bu *Hata Ayıklama Özelliği,* tüm ardışık işlem hattını sınamak istemediğiniz, ancak ardışık işlem hattının içindeki bir alt kümesini test etmek istediğinizde yararlıdır.

![Boru hattı tuvalindeki kesme noktaları](media/iterative-development-debugging/iterative-development-image4.png)

Kesme noktası ayarlamak için, ardışık hatlar tuvalinde bir öğe seçin. Hata *Ayıklama Kadar* seçeneği öğenin sağ üst köşesinde boş bir kırmızı daire olarak görünür.

![Seçili öğeüzerinde bir kesme noktası ayarlamadan önce](media/iterative-development-debugging/iterative-development-image5.png)

*Hata Ayıklama Ta'yı* seçtikten sonra, kesme noktasının etkin olduğunu belirtmek için dolu kırmızı daireye dönüşür.

![Seçili öğeüzerinde bir kesme noktası ayarladıktan sonra](media/iterative-development-debugging/iterative-development-image6.png)

## <a name="next-steps"></a>Sonraki adımlar
[Azure Veri Fabrikası'nda sürekli tümleştirme ve dağıtım](continuous-integration-deployment.md)
