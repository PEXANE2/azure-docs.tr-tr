---
title: Power BI ile Azure Analysis Services bağlanma | Microsoft Docs
description: Power BI kullanarak bir Azure Analysis Services sunucusuna nasıl bağlanacağınızı öğrenin. Bağlandıktan sonra kullanıcılar model verilerini keşfedebilir.
author: minewiskan
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 03/30/2020
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: f79ff9af1272ca908c7a5ba35fbaa1e8bb87fc50
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86506729"
---
# <a name="connect-with-power-bi"></a>Power BI ile bağlanma

Azure 'da bir sunucu oluşturup buna bir tablosal Model dağıttıktan sonra, kuruluşunuzdaki kullanıcılar bağlanmaya ve verileri keşfetmeye başlamaya hazırsınız. 

> [!TIP]
> [Power BI Desktop](https://powerbi.microsoft.com/desktop/)en son sürümünü kullandığınızdan emin olun.
> 
> 
  
## <a name="connect-in-power-bi-desktop"></a>Power BI Desktop'ta bağlanma

1. Power BI Desktop, **veri al**  >  **Azure**  >  **Azure Analysis Services veritabanı**' na tıklayın.

2. **Sunucu**' da sunucu adını girin. Tam URL 'YI eklediğinizden emin olun; Örneğin, asazure://westcentralus.asazure.windows.net/advworks.

3. **Veritabanında**, bağlamak istediğiniz tablosal model veritabanının veya perspektifinin adını biliyorsanız buraya yapıştırın. Aksi takdirde, bu alanı boş bırakıp daha sonra bir veritabanı veya perspektif seçebilirsiniz.

4. Bir bağlantı seçeneği seçin ve ardından **Bağlan**' a basın. 

    Hem **canlı Bağlan** hem de **içeri aktarma** seçenekleri desteklenir. Ancak, Içeri aktarma modunda bazı sınırlamalar olduğundan canlı bağlantılar kullanmanız önerilir; özellikle sunucu performansı, içeri aktarma sırasında etkilenebilir. Ayrıca, model Power BI hizmeti yenilenirse, **Power BI erişime Izin ver** ayarı yalnızca **canlı Bağlan**' ı seçerken geçerlidir.

5. İstenirse, oturum açma kimlik bilgilerinizi girin. 

   > [!NOTE]
   > Bir kerelik geçiş kodu (OTP) hesapları desteklenmez. 

6. **Gezgin**'de, sunucuyu genişletin, ardından bağlanmak istediğiniz modeli veya perspektifi seçin ve ardından **Bağlan**' a tıklayın. Bu görünüm için tüm nesneleri göstermek üzere bir modele veya perspektife tıklayın.

    Model, rapor görünümünde boş bir raporla Power BI Desktop açılır. Alanlar listesinde gizli olmayan tüm model nesneleri görüntülenir. Bağlantı durumu sağ alt köşede gösterilir.

## <a name="connect-in-power-bi-service"></a>Power BI (hizmet) içinde Bağlan

1. Sunucunuzdaki modelinize canlı bağlantısı olan Power BI Desktop bir dosya oluşturun.
2. [Power BI](https://powerbi.microsoft.com), **veri**  >  **dosyalarını**Al ' a tıklayın ve ardından. pbix dosyanızı bulup seçin.

## <a name="see-also"></a>Ayrıca bkz.
[Azure Analysis Services Bağlan](analysis-services-connect.md)   
[İstemci kitaplıkları](https://docs.microsoft.com/analysis-services/client-libraries?view=azure-analysis-services-current)

