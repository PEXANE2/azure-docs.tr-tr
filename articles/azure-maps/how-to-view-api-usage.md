---
title: Azure Haritalar API kullanım ölçümlerini görüntüleyin | Microsoft Azure Haritaları
description: Bu makalede, Azure portalında Microsoft Azure Haritalar API çağrılarınızın ölçümlerini nasıl görüntüleyebilirsiniz.
author: philmea
ms.author: philmea
ms.date: 08/06/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: 0eb117af712b3b1f63a3f99c96cba9775f8e3996
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80335159"
---
# <a name="view-azure-maps-api-usage-metrics"></a>Azure Haritalar API'si kullanım ölçümlerini görüntüleme

Bu makalede, Azure Haritalar hesabınız için API kullanım ölçümlerini [Azure portalında](https://portal.azure.com)nasıl görüntüleyebilirsiniz. Ölçümler, özelleştirilebilir bir süre boyunca uygun bir grafik biçiminde gösterilir.

## <a name="view-metric-snapshot"></a>Metrik anlık görüntü görüntüleme

Haritalar hesabınızın **Genel Bakış** sayfasında bazı yaygın ölçümleri görebilirsiniz. Şu anda seçilebilir bir süre boyunca *Toplam İstekler,* *Toplam Hatalar*ve *Kullanılabilirlik* gösterir.

![Azure Haritalar kullanım ölçümlerine genel bakış](media/how-to-view-api-usage/portal-overview.png)

Bu grafikleri özel çözümlemesiniz için özelleştirmeniz gerekiyorsa bir sonraki bölüme devam edin.

## <a name="view-detailed-metrics"></a>Ayrıntılı ölçümleri görüntüleme

1. [Portalda](https://portal.azure.com)Azure aboneliğinizde oturum açın.

2. Sol taraftaki **Tüm kaynaklar** menüsü öğesini tıklatın ve Azure *Haritalar Hesabınıza*gidin.

3. Haritalar hesabınız açıldıktan sonra soldaki **Ölçümler** menüsüne tıklayın.

4. **Ölçümler** bölmesine aşağıdaki seçeneklerden birini seçin:

   1. **Kullanılabilirlik** - bir süre içinde API kullanılabilirlik *ortalamasını* gösterir.
   2. **Kullanım** - hesabınız için kullanım *Sayısı'nın* nasıl olduğunu gösterir.

      ![Azure Haritalar kullanım ölçümleri bölmesi](media/how-to-view-api-usage/portal-metrics.png)

5. Ardından, **Son 24 saat (Otomatik)** seçeneğini tıklatarak *Saat aralığını* seçebilirsiniz. Varsayılan olarak, zaman aralığı 24 saat olarak ayarlanır. Tıklattıktan sonra, tüm seçilebilir zaman aralıklarını görürsünüz. Zaman parçalı *nı* seçebilir ve saati *yerel* veya *GMT* olarak aynı açılır durumda göstermeyi seçebilirsiniz. **Uygula**’ya tıklayın.

    ![Azure Haritalar ölçümleri zaman aralığı](media/how-to-view-api-usage/time-range.png)

6. Metriğinizi ekledikten sonra, bu metriğin ilgili özelliklerinden **filtre ekleyebilirsiniz.** Ardından, grafikte yansıtılan görmek istediğiniz özelliğin değerini seçin.

    ![Azure Haritalar kullanım ölçümleri Filtresi](media/how-to-view-api-usage/filter.png)

7. Ayrıca, seçtiğiniz metrik özelliğine göre metrik için **bölme uygulayabilirsiniz.** Grafiğin, o özelliğin her değeri için birden çok grafiğe bölünmesini sağlar. Aşağıdaki resimde, her grafiğin rengi grafiğin alt kısmında gösterilen özellik değerine karşılık gelir.

    ![Azure Haritalar kullanım ölçümleri bölme](media/how-to-view-api-usage/splitting.png)

8. Ayrıca, üstteki **Metrik Ekle** düğmesine tıklayarak aynı grafikte birden çok ölçüm gözlemlayabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdakiler için kullanımı izlemek istediğiniz Azure Haritalar API'ları hakkında daha fazla bilgi edinin:
> [!div class="nextstepaction"] 
> [Azure Haritalar Web SDK Nasıl Yapılsın](how-to-use-map-control.md)

> [!div class="nextstepaction"] 
> [Azure Haritalar Android SDK Nasıl-Nasıl](how-to-use-android-map-control-library.md)

> [!div class="nextstepaction"]
> [Azure Haritalar REST API belgeleri](https://docs.microsoft.com/rest/api/maps)
