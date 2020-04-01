---
title: Power BI ile Azure Analiz Hizmetlerine Bağlanın | Microsoft Dokümanlar
description: Power BI'yi kullanarak Azure Analiz Hizmetleri sunucusuna nasıl bağlanabileceğinizi öğrenin. Bağlandıktan sonra, kullanıcılar model verilerini keşfedebilir.
author: minewiskan
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 03/30/2020
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 6205c4189abfefc2ee9c4a273ebfd6773ea609b6
ms.sourcegitcommit: 27bbda320225c2c2a43ac370b604432679a6a7c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80411371"
---
# <a name="connect-with-power-bi"></a>Power BI ile bağlanma

Azure'da bir sunucu oluşturduktan ve ona bir tabular modeli dağıttıktan sonra, kuruluşunuzdaki kullanıcılar bağlanmaya ve verileri keşfetmeye hazırdır. 

> [!TIP]
> [Power BI Desktop'ın](https://powerbi.microsoft.com/desktop/)en son sürümünü kullandığınızdan emin olun.
> 
> 
  
## <a name="connect-in-power-bi-desktop"></a>Power BI Desktop'ta bağlanma

1. Power BI Desktop'da **Veri** > Al**Azure** > **Azure Analiz Hizmetleri veritabanını**tıklatın.

2. **Sunucu'da**sunucu adını girin. TAM URL'yi eklediğinden emin olun; örneğin, asazure://westcentralus.asazure.windows.net/advworks.

3. **Veritabanı'nda,** bağlanmak istediğiniz tabular model veritabanının veya perspektifinin adını biliyorsanız, buraya yapıştırın. Aksi takdirde, bu alanı boş bırakabilir ve daha sonra bir veritabanı veya perspektif seçebilirsiniz.

4. Bağlantı seçeneğini seçin ve ardından **Bağlan'a**basın. 

    Hem **Canlı Bağlan** hem de **İçe Aktarma** seçenekleri desteklenir. Ancak, Alma modubazı sınırlamalar var, çünkü canlı bağlantıları kullanmanızı tavsiye; en önemlisi, sunucu performansı alma sırasında etkilenebilir. Ayrıca, model Power BI hizmetinde yenilenecekse, **Power BI ayarından erişime izin** ver yalnızca Canlı **Bağlan'ı**seçerken geçerlidir.

5. İstenirse, giriş kimlik bilgilerinizi girin. 

6. **Navigator'da**sunucuyu genişletin, ardından bağlanmak istediğiniz modeli veya perspektifi seçin ve sonra **Bağlan'ı**tıklatın. Bu görünümiçin tüm nesneleri göstermek için bir model veya perspektifi tıklatın.

    Model, Power BI Desktop'da Rapor görünümünde boş bir raporla açılır. Alanlar listesinde gizli olmayan tüm model nesneleri görüntülenir. Bağlantı durumu sağ alt köşede gösterilir.

## <a name="connect-in-power-bi-service"></a>Power BI'ye bağlan (servis)

1. Sunucunuzdaki modelinize canlı bağlantısı olan bir Power BI Masaüstü dosyası oluşturun.
2. [Power BI'de](https://powerbi.microsoft.com)Veri > **Dosyalarını** **Al'ı**tıklatın ve sonra .pbix dosyanızı bulup seçin.

## <a name="see-also"></a>Ayrıca bkz.
[Azure Analiz Hizmetlerine Bağlanın](analysis-services-connect.md)   
[İstemci kitaplıkları](analysis-services-data-providers.md)

