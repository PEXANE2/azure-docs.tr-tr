---
title: Azure Uygulama Öngörülerinden Power BI'ye Dışa Aktarma | Microsoft Dokümanlar
description: Analitik sorguları Power BI'de görüntülenebilir.
ms.topic: conceptual
ms.date: 08/10/2018
ms.openlocfilehash: 0e17ca6e07ec76f0a7a1cb04f7aa13619fb9970c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77664006"
---
# <a name="feed-power-bi-from-application-insights"></a>Uygulama Öngörülerinden Besleme Gücü BI
[Power BI,](https://www.powerbi.com/) verileri analiz elemenize ve öngörüleri paylaşmanıza yardımcı olan bir iş araçları paketidir. Her cihazda zengin panolar mevcuttur. [Azure Uygulama Öngörüleri'nin](../../azure-monitor/app/app-insights-overview.md)Analytics sorguları da dahil olmak üzere birçok kaynaktan gelen verileri birleştirebilirsiniz.

Power BI'ye Uygulama Öngörüleri verilerini aktarmanın üç yöntemi vardır:

* [**İhracat Analytics sorguları**](#export-analytics-queries). Bu tercih edilen yöntemdir. İstediğinherhangi bir sorgu yazın ve Power BI'ye dışa aktarın. Bu sorgu, diğer verilerle birlikte bir panoya yerleştirebilirsiniz.
* [**Sürekli dışa aktarma ve Azure Akışı Analitiği.**](../../azure-monitor/app/export-stream-analytics.md) Verilerinizi uzun süre depolamak istiyorsanız, bu yöntem yararlıdır. Genişletilmiş bir veri saklama gereksiniminuz yoksa, dışa aktarma analizi sorgu yöntemini kullanın. Sürekli dışa aktarma ve Akış Analizi, kurulum ve ek depolama ek yükü için daha fazla çalışma içerir.
* **Güç BI adaptörü**. Grafik kümesi önceden tanımlanmıştır, ancak diğer kaynaklardan kendi sorgularınızı ekleyebilirsiniz.

> [!NOTE]
> Power BI bağdaştırıcısı artık **amortismana hazır.** Bu çözüm için önceden tanımlanmış grafikler statik uneditable sorguları tarafından doldurulur. Bu sorguları yeniden yapma olanağınız yoktur ve verilerinizin belirli özelliklerine bağlı olarak Power BI bağlantısının başarılı olması mümkündür, ancak hiçbir veri doldurulur. Bunun nedeni, kodlanmış sorgu içinde ayarlanan dışlama ölçütleridir. Bu çözüm bazı müşteriler için hala işe yarayabilir, bağdaştırıcının esnekliği olmadığı için önerilen çözüm [**dışa aktarma Analytics sorgu**](#export-analytics-queries) işlevini kullanmaktır.

## <a name="export-analytics-queries"></a>İhracat Analitiği sorguları
Bu rota, istediğiniz Herhangi bir Analytics sorgusu yazmanızı veya Kullanım Hunilerinden dışa aktarıp bunu bir Power BI panosuna aktarmanızı sağlar. (Bağdaştırıcı tarafından oluşturulan panoya ekleyebilirsiniz.)

### <a name="one-time-install-power-bi-desktop"></a>Bir kez: Power BI Masaüstü yüklemek
Application Insights sorgunuzu almak için Power BI'nin masaüstü sürümünü kullanırsınız. Ardından web'de veya Power BI bulut çalışma alanınızda yayınlayabilirsiniz. 

[Yük Güç BI Masaüstü](https://powerbi.microsoft.com/en-us/desktop/).

### <a name="export-an-analytics-query"></a>Analytics sorgusuna dışa aktarma
1. [Analytics'i açın ve sorgunuzu yazın.](../../azure-monitor/log-query/get-started-portal.md)
2. Sonuçlardan memnun olana kadar sorguyu test edin ve hassaslaştırın. Dışa aktarmadan önce sorgunun Analytics'te doğru çalıştığından emin olun.
3. **Dışa Aktarma** menüsünde **Power BI (M)** seçeneğini belirleyin. Metin dosyasını kaydedin.
   
    ![Analytics ekran görüntüsü, Export menüsü vurgulanır](./media/export-power-bi/analytics-export-power-bi.png)
4. Power BI Desktop'da Veri > Boş**Sorgu** **al'ı**seçin. Daha sonra, sorgu düzenleyicisinde, **Görünüm**altında **Gelişmiş Düzenleyici'yi**seçin.

    Dışa aktarılan M Dili komut dosyasını Gelişmiş Editöre yapıştırın.

    ![Advanced Editor vurgulanmış Power BI Masaüstü ekran görüntüsü](./media/export-power-bi/power-bi-import-analytics-query.png)

5. Power BI'nin Azure'a erişmesine izin vermek için kimlik bilgileri sağlamanız gerekebilir. Microsoft **hesabınızda** oturum açabilmek için Kuruluş hesabını kullanın.
   
    ![Power BI Sorgu Ayarları iletişim kutusunun ekran görüntüsü](./media/export-power-bi/power-bi-import-sign-in.png)

    Kimlik bilgilerini doğrulamanız gerekiyorsa, sorgu düzenleyicisindeki **Veri Kaynağı Ayarları** menüsü komutunu kullanın. Power BI için kimlik bilgilerinizden farklı olabilecek Azure için kullandığınız kimlik bilgilerini belirttiğinizden emin olun.
6. Sorgunuz için bir görselleştirme seçin ve x ekseni, y ekseni ve boyutlandırma alanları seçin.
   
    ![Power BI Masaüstü görselleştirme seçeneklerinin ekran görüntüsü](./media/export-power-bi/power-bi-analytics-visualize.png)
7. Raporunuzu Power BI bulut çalışma alanınızda yayınlayın. Buradan, senkronize edilmiş bir sürümü diğer web sayfalarına katıştırabilirsiniz.
   
    ![Power BI Desktop ekran görüntüsü, Yayın düğmesi vurgulanmış](./media/export-power-bi/publish-power-bi.png)
8. Raporu aralıklarla el ile yenileyin veya seçenekler sayfasında zamanlanmış bir yenileme ayarlayın.

### <a name="export-a-funnel"></a>Huni Verme
1. [Huni nizi yapın.](../../azure-monitor/app/usage-funnels.md)
2. **Power BI'yi**seçin.

   ![Power BI düğmesinin ekran görüntüsü](./media/export-power-bi/button.png)

3. Power BI Desktop'da Veri > Boş**Sorgu** **al'ı**seçin. Daha sonra, sorgu düzenleyicisinde, **Görünüm**altında **Gelişmiş Düzenleyici'yi**seçin.

   ![Boş Sorgu düğmesi vurgulanmış Power BI Masaüstü ekran görüntüsü](./media/export-power-bi/blankquery.png)

   Dışa aktarılan M Dili komut dosyasını Gelişmiş Editöre yapıştırın. 

   ![Advanced Editor vurgulanmış Power BI Masaüstü ekran görüntüsü](./media/export-power-bi/advancedquery.png)

4. Sorgudaki öğeleri seçin ve bir Huni görselleştirmesi seçin.

   ![Power BI Masaüstü görselleştirme seçeneklerinin ekran görüntüsü](./media/export-power-bi/selectsequence.png)

5. Anlamlı hale getirmek için başlığı değiştirin ve raporunuzu Power BI bulut çalışma alanınızda yayınlayın. 

   ![Başlık değişikliği vurgulanmış Power BI Masaüstü ekran görüntüsü](./media/export-power-bi/changetitle.png)

## <a name="troubleshooting"></a>Sorun giderme

Kimlik bilgileriyle veya veri kümesinin boyutuyla ilgili hatalarla karşılaşabilirsiniz. Aşağıda, bu hatalar hakkında ne yapılacılarının yapıla

### <a name="unauthorized-401-or-403"></a>Yetkisiz (401 veya 403)
Yenileme belirteciniz güncelleştirilmezse bu durum olabilir. Erişiminizi devam ettirebilmek için şu adımları deneyin:

1. Azure portalında oturum açın ve kaynağa erişebildiğinizden emin olun.
2. Pano için kimlik bilgilerini yenilemeye çalışın.
3. PowerBI Masaüstünüzdeki önbelleği temizlemeye çalışın.


   Kimlik bilgilerine erişiminiz varsa ve kimlik bilgilerini yenilemek işe yaramazsa, lütfen bir destek bileti açın.

### <a name="bad-gateway-502"></a>Kötü Ağ Geçidi (502)
Bunun nedeni genellikle çok fazla veri döndüren bir Analytics sorgusudur. Sorgu için daha küçük bir zaman aralığı kullanmayı deneyin. 

Analytics sorgusundan gelen veri kümesini azaltmak gereksinimlerinizi karşılamıyorsa, daha büyük bir veri kümesi çekmek için [API'yi](https://dev.applicationinsights.io/documentation/overview) kullanmayı düşünün. API'yi kullanmak için M-Query dışa aktarmayı şu şekilde dönüştürebilirsiniz.

1. Bir [API anahtarı](https://dev.applicationinsights.io/documentation/Authorization/API-key-and-App-ID)oluşturun.
2. Azure Kaynak Yöneticisi URL'sini Application Insights API ile değiştirerek Analytics'ten dışa aktardığınız Power BI M komut dosyasını güncelleştirin.
   * **https değiştirin:\//management.azure.com/subscriptions/...**
   * ile, **https:\//api.applicationinsights.io/beta/apps/...**
3. Son olarak, kimlik bilgilerini temel olarak güncelleştirin ve API anahtarınızı kullanın.

**Varolan komut dosyası**
 ```
 Source = Json.Document(Web.Contents("https://management.azure.com/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups//providers/microsoft.insights/components//api/query?api-version=2014-12-01-preview",[Query=[#"csl"="requests",#"x-ms-app"="AAPBI"],Timeout=#duration(0,0,4,0)]))
 ```
**Güncelleştirilmiş komut dosyası**
 ```
 Source = Json.Document(Web.Contents("https://api.applicationinsights.io/beta/apps/<APPLICATION_ID>/query?api-version=2014-12-01-preview",[Query=[#"csl"="requests",#"x-ms-app"="AAPBI"],Timeout=#duration(0,0,4,0)]))
 ```

## <a name="about-sampling"></a>Örnekleme hakkında
Uygulamanız tarafından gönderilen veri miktarına bağlı olarak, telemetrinizin yalnızca bir yüzdesini gönderen uyarlanabilir örnekleme özelliğini kullanmak isteyebilirsiniz. SDK'da veya yutma işleminde örneklemeyi el ile ayarladıysanız da aynı durum geçerlidir. [Örnekleme hakkında daha fazla bilgi edinin.](../../azure-monitor/app/sampling.md)

## <a name="power-bi-adapter-deprecated"></a>Güç BI bağdaştırıcısı (amortismana uğradı)
Bu yöntem sizin için tam bir telemetri panosu oluşturur. İlk veri kümesi önceden tanımlanmıştır, ancak daha fazla veri ekleyebilirsiniz.

### <a name="get-the-adapter"></a>Adaptörü alın
1. [Power BI](https://app.powerbi.com/)'da oturum açın.
2. Sol alt köşede ![](./media/export-power-bi/001.png)GetData Simgesi veri ekran görüntüsü **alın,** **Hizmetler**.

    ![Application Insights veri kaynağından alın ekran görüntüleri](./media/export-power-bi/002.png)

3. Uygulama Öngörüleri altında **şimdi alın'ı** seçin.

   ![Application Insights veri kaynağından alın ekran görüntüleri](./media/export-power-bi/003.png)
4. Uygulama Öngörüleri kaynağınızın ayrıntılarını sağlayın ve ardından **oturum açın.**

    ![Uygulama Öngörüleri veri kaynağından alın ekran görüntüsü](./media/export-power-bi/005.png)

     Bu bilgiler, Uygulama Öngörüleri Genel Bakış bölmesinde bulunabilir:

     ![Uygulama Öngörüleri veri kaynağından alın ekran görüntüsü](./media/export-power-bi/004.png)

5. Yeni oluşturulan Application Insights Power BI Uygulamasını açın.

6. Verilerin içe aktarılması için bir veya iki dakika bekleyin.

    ![Power BI adaptörü ekran görüntüsü](./media/export-power-bi/010.png)

Uygulama Öngörüleri grafiklerini diğer kaynaklarla ve Analytics sorgularıyla birleştirerek panonu edinebilirsiniz. Görselleştirme galerisinden daha fazla grafik alabilirsiniz ve her grafiğin ayarlayabileceğiniz parametreleri vardır.

İlk alma işleminden sonra pano ve raporlar günlük olarak güncelleştirilmektedir. Veri kümesinde yenileme zamanlamasını denetleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
* [Güç BI - Öğrenin](https://www.powerbi.com/learning/)
* [Analitik eğitimi](../../azure-monitor/log-query/get-started-portal.md)

