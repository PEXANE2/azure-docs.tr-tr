---
title: Azure Application Insights Funlar
description: Müşterilerin uygulamanızla nasıl etkileşime alınacağını öğrenmek için funnels 'yi nasıl kullanabileceğinizi öğrenin.
ms.topic: conceptual
author: NumberByColors
ms.author: daviste
ms.date: 07/17/2017
ms.reviewer: mbullwin
ms.openlocfilehash: 89440a6385bab5b917a866b686e8d2ba828c92e5
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "77671061"
---
# <a name="discover-how-customers-are-using-your-application-with-application-insights-funnels"></a>Application Insights Funlarla müşterilerin uygulamanızı nasıl kullandığını öğrenin

Müşteri deneyimini anlamak, işletmeniz için en önemli öneme sahiptir. Uygulamanız birden çok aşama içeriyorsa, çoğu müşterinin işlemin tamamında ilerlediğini veya işlemi bir noktada sonlandırdığını bilmeniz gerekir. Web uygulamasındaki bir dizi adımdan ilerleme, *huni*olarak bilinir. Kullanıcılarınıza Öngörüler elde etmek ve adım adım dönüştürme oranlarını izlemek için Azure Application Insights funnels 'yi kullanabilirsiniz. 

## <a name="create-your-funnel"></a>Huni oluşturma
Huni oluşturmadan önce yanıtlamak istediğiniz soruya karar verin. Örneğin, giriş sayfasını görüntüleme, müşteri profilini görüntüleme ve bilet oluşturma işlemlerinin kaç Kullanıcı tarafından yapıldığını öğrenmek isteyebilirsiniz. Bu örnekte, Fabrikam Fiber şirketinin sahipleri, başarıyla bir müşteri bileti oluşturan müşterilerin yüzdesini bilmesini istiyor.

Huni oluşturmak için gereken adımlar aşağıda verilmiştir.

1. Application Insights funnels aracında **Yeni**' yi seçin.
1. **Zaman aralığı** açılan menüsünde **son 90 gün**' yı seçin. **Komik** veya **paylaşılan komik**seçeneklerinden birini belirleyin.
1. **Adım 1** aşağı açılan listesinden **Dizin**' i seçin. 
1. 2. **adım** listesinde **Müşteri**' yi seçin.
1. 3. **adım** listesinden **Oluştur**' u seçin.
1. Huni için bir ad ekleyin ve **Kaydet**' i seçin.

Aşağıdaki ekran görüntüsünde, funnels aracının oluşturduğu veri türünün bir örneği gösterilmektedir. Fabrikam sahipleri, son 90 gün içinde, giriş sayfasını ziyaret eden müşterilerinin yüzde 54,3 ' inin bir müşteri bileti oluşturduğunu görebilirler. Ayrıca, kendi müşterilerinin 2.700, giriş sayfasındaki dizine geldiğini de görebilir. Bu, yenileme sorununa işaret edebilir.


![Verilerle funnels aracının ekran görüntüsü](media/usage-funnels/funnel1.png)

### <a name="funnels-features"></a>Funnels özellikleri
Önceki ekran görüntüsünde beş Vurgulanan alan bulunur. Bunlar, Nednels özellikleridir. Aşağıdaki listede, ekran görüntüsünde karşılık gelen her alan hakkında daha fazla bilgi verilmektedir:
1. Uygulamanız örneklenir ise, bir örnekleme başlığı görürsünüz. Başlığın seçilmesi, örnekleme 'nın nasıl açılacağını açıklayan bir bağlam bölmesi açar. 
2. [Power BI](../../azure-monitor/app/export-power-bi.md ), huni 'nizi dışarı aktarabilirsiniz.
3. Sağ tarafta daha fazla ayrıntı görmek için bir adım seçin. 
4. Geçmiş dönüştürme grafiğinde, son 90 güne ait dönüştürme ücretleri gösterilmektedir. 
5. Kullanıcılar aracına erişerek kullanıcılarınızı daha iyi anlayın. Her adımda filtre kullanabilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar
  * [Kullanıma genel bakış](usage-overview.md)
  * [Kullanıcılar, Oturumlar ve Etkinlikler](usage-segmentation.md)
  * [Bekletme](usage-retention.md)
  * [Çalışma Kitapları](../../azure-monitor/app/usage-workbooks.md)
  * [Kullanıcı bağlamı Ekle](usage-send-user-context.md)
  * [Power BI’a aktarma](../../azure-monitor/app/export-power-bi.md )

