---
title: Azure Uygulama Öngörüleri Hunileri
description: Müşterilerin uygulamanızla nasıl etkileşimde bulunduğunu keşfetmek için Hunileri nasıl kullanabileceğinizi öğrenin.
ms.topic: conceptual
author: NumberByColors
ms.author: daviste
ms.date: 07/17/2017
ms.reviewer: mbullwin
ms.openlocfilehash: 89440a6385bab5b917a866b686e8d2ba828c92e5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77671061"
---
# <a name="discover-how-customers-are-using-your-application-with-application-insights-funnels"></a>Application Insights Hunileri ile müşterilerin uygulamanızı nasıl kullandığını keşfedin

Müşteri deneyimini anlamak işletmeniz için son derece önemlidir. Uygulamanız birden çok aşama içeriyorsa, çoğu müşterinin tüm işlem boyunca ilerleme kaydedip geçirmediğini veya bir noktada işlemi sonlandırdıklarını bilmeniz gerekir. Bir web uygulamasında bir dizi adımda ilerleme *huni*olarak bilinir. Kullanıcılarınız hakkında bilgi edinmek ve adım adım dönüşüm oranlarını izlemek için Azure Uygulama Öngörüleri Hunileri'ni kullanabilirsiniz. 

## <a name="create-your-funnel"></a>Huninizi oluşturun
Huninizi oluşturmadan önce, yanıtlamak istediğiniz soruya karar verin. Örneğin, ana sayfayı kaç kullanıcının görüntülediğinizi, müşteri profilini görüntülediğinizi ve bir bilet oluşturduğunu bilmek isteyebilirsiniz. Bu örnekte, Fabrikam Fiber şirketinin sahipleri başarılı bir müşteri bileti oluşturan müşterilerin yüzdesini bilmek istiyor.

Hunilerini oluşturmak için atdıkları adımlar şunlardır.

1. Application Insights Huniler aracında **Yeni'yi**seçin.
1. Zaman **Aralığı** açılır menüsünden **Son 90 gün'ü**seçin. **Hunilerimi** veya Paylaşılan **hunilerimi**seçin.
1. Adım **1** açılır listesinden **Dizin'i**seçin. 
1. Adım **2** listesinden **Müşteri'yi**seçin.
1. Adım **3** listesinden **Oluştur'u**seçin.
1. Huniye bir ad ekleyin ve **Kaydet'i**seçin.

Aşağıdaki ekran görüntüsü, Huniler aracının oluşturduğu veri türünün bir örneğini gösterir. Fabrikam sahipleri, son 90 gün içinde ana sayfayı ziyaret eden müşterilerinin yüzde 54,3'ünün müşteri bileti oluşturduğunu görebiliyor. Ayrıca, müşterilerinin 2.700'ünün ana sayfadan dizin için geldiğini de görebiliyorlar. Bu, bir yenileme sorunu gösterebilir.


![Veri ile Huniler aracının ekran görüntüsü](media/usage-funnels/funnel1.png)

### <a name="funnels-features"></a>Huni özellikleri
Önceki ekran görüntüsü beş vurgulanan alanları içerir. Bunlar Hunilerin özellikleridir. Aşağıdaki liste, ekran görüntüsündeki her ilgili alan hakkında daha fazla açıklama dır:
1. Uygulamanız örneklenirse, bir örnekleme afişi görürsünüz. Banner'ı seçmek, örneklemenin nasıl kapatılabildiğini açıklayan bir bağlam bölmesi açar. 
2. Huninizi [Power BI'ye](../../azure-monitor/app/export-power-bi.md )dışa aktarabilirsiniz.
3. Sağdaki ayrıntıları görmek için bir adım seçin. 
4. Geçmiş dönüşüm grafiği, son 90 gündeki dönüşüm oranlarını gösterir. 
5. Kullanıcı aracına erişerek kullanıcılarınızı daha iyi anlayın. Her adımda filtrelerk kullanabilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar
  * [Kullanıma genel bakış](usage-overview.md)
  * [Kullanıcılar, Oturumlar ve Etkinlikler](usage-segmentation.md)
  * [Bekletme](usage-retention.md)
  * [Çalışma Kitapları](../../azure-monitor/app/usage-workbooks.md)
  * [Kullanıcı bağlamı ekleme](usage-send-user-context.md)
  * [Power BI’a aktarma](../../azure-monitor/app/export-power-bi.md )

