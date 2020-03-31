---
title: Özellikleri ve kapasiteleri ölçeklendirin
description: Azure Uygulama Hizmeti'nde bir uygulamayı nasıl ölçeklendireceklerini öğrenin. Daha fazla CPU, bellek, disk alanı ve ekstra özellik elde edin.
ms.assetid: f7091b25-b2b6-48da-8d4a-dcf9b7baccab
ms.topic: article
ms.date: 08/19/2019
ms.custom: seodec18
ms.openlocfilehash: dfd9297e80836978b8a185df3fc4659676383802
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75659908"
---
# <a name="scale-up-an-app-in-azure-app-service"></a>Azure Uygulama Hizmeti'nde bir uygulamayı ölçeklendirin

Bu makalede, Azure Uygulama Hizmeti'nde uygulamanızı nasıl ölçeklendirdiğinizi gösterilmektedir. Ölçeklendirme için iki iş akışı vardır, ölçeklendirin ve ölçeklendirin ve bu makalede ölçeklendirme iş akışını açıklar.

* [Ölçeklendirme](https://en.wikipedia.org/wiki/Scalability#Horizontal_and_vertical_scaling): Daha fazla CPU, bellek, disk alanı ve özel sanal makineler (VM'ler), özel etki alanları ve sertifikalar, evreleme yuvaları, otomatik ölçeklendirme ve daha fazlası gibi ekstra özellikler alın. Uygulamanızın ait olduğu Uygulama Hizmeti planının fiyatlandırma katmanını değiştirerek ölçeklendirirsiniz.
* [Ölçeklendir](https://en.wikipedia.org/wiki/Scalability#Horizontal_and_vertical_scaling): Uygulamanızı çalıştıran VM örneklerinin sayısını artırın.
  Fiyatlandırma katmanınıza bağlı olarak 30 örneğe kadar ölçeklendirebilirsiniz. **Yalıtılmış** katmandaki [Uygulama Hizmet Ortamları,](environment/intro.md) ölçeklendirme sayınızı 100 örneğe daha da artırır. Ölçeklendirme hakkında daha fazla bilgi için, [bkz.](../monitoring-and-diagnostics/insights-how-to-scale.md) Burada, önceden tanımlanmış kurallara ve zamanlamalara göre örnek sayısını otomatik olarak ölçeklendirmek için otomatik ölçeklendirmenin nasıl kullanılacağını öğrenirsiniz.

Ölçek ayarlarının uygulanması yalnızca saniyeler sürer ve [Uygulama Hizmeti planınızdaki](../app-service/overview-hosting-plans.md)tüm uygulamaları etkiler.
Kodunuzu değiştirmenizi veya uygulamanızı yeniden dağıtmanızı gerektirmezler.

Bireysel Uygulama Hizmeti planlarının fiyatlandırması ve özellikleri hakkında bilgi için [Uygulama Hizmeti Fiyatlandırma Ayrıntıları'na](https://azure.microsoft.com/pricing/details/web-sites/)bakın.  

> [!NOTE]
> **Ücretsiz** katmandan bir Uygulama Hizmeti planını değiştirmeden önce, Azure aboneliğiniz için [öncelikle harcama sınırlarını](https://azure.microsoft.com/pricing/spending-limits/) kaldırmanız gerekir. Microsoft Azure Uygulama Hizmeti aboneliğiniz için seçenekleri görüntülemek veya değiştirmek için [Bkz. Microsoft Azure Abonelikleri.][azuresubscriptions]
> 
> 

<a name="scalingsharedorbasic"></a>
<a name="scalingstandard"></a>

## <a name="scale-up-your-pricing-tier"></a>Fiyatlandırma katmanınızı ölçeklendirin

> [!NOTE]
> **PremiumV2** katmanına kadar ölçeklendirmek [için, Uygulama Hizmeti için PremiumV2 katmanını Yapılandır'a](app-service-configure-premium-tier.md)bakın.
>

1. Tarayıcınızda [Azure portalı][portal]’nı açın.

1. Uygulama Hizmeti uygulama sayfanızda, sol menüden **Yukarı Ölçekle (App Service planı)** seçeneğini belirleyin.
   
3. Katmanınızı seçin ve sonra **Uygula'yı**seçin. Farklı kategorileri seçin (örneğin, **Üretim)** ve ayrıca daha fazla katman göstermek için **ek seçenekleri görün.**
   
    ![Azure uygulamanızı büyütmek için gidin.][ChooseWHP]

    İşlem tamamlandığında, yeşil başarı onay işareti ne reli bir bildirim açılır penceresi görürsünüz.

<a name="ScalingSQLServer"></a>

## <a name="scale-related-resources"></a>İlgili kaynakları ölçeklendir
Uygulamanız Azure SQL Veritabanı veya Azure Depolama gibi diğer hizmetlere bağlıysa, bu kaynakları ayrı ayrı ölçeklendirebilirsiniz. Bu kaynaklar App Service planı tarafından yönetilmez.

1. Uygulamanız için **Genel Bakış** sayfasında **Kaynak grubu** bağlantısını seçin.
   
    ![Azure uygulamanızın ilgili kaynaklarını ölçeklendirin](./media/web-sites-scale/RGEssentialsLink.png)

2. **Kaynak grubu** sayfasının **Özet** bölümünde, ölçeklendirmek istediğiniz bir kaynak seçin. Aşağıdaki ekran görüntüsü bir SQL Veritabanı kaynağını gösterir.
   
    ![Azure uygulamanızı ölçeklendirmek için kaynak grubu sayfasına gidin](./media/web-sites-scale/ResourceGroup.png)

    İlgili kaynağı ölçeklendirmek için, belirli kaynak türüiçin belgelere bakın. Örneğin, tek bir SQL Veritabanını ölçeklendirmek için [Azure SQL Veritabanı'ndaki tek veritabanı kaynaklarını ölçeklendir'e](../sql-database/sql-database-single-database-scale.md)bakın. MySQL kaynağı için bir Azure Veritabanı nı ölçeklendirmek için [MySQL kaynaklarını ölçeklendir'e](../mysql/concepts-pricing-tiers.md#scale-resources)bakın.

<a name="OtherFeatures"></a>
<a name="devfeatures"></a>

## <a name="compare-pricing-tiers"></a>Fiyatlandırma katmanlarını karşılaştırın

Her fiyatlandırma katmanı için VM boyutları gibi ayrıntılı bilgi için [Uygulama Hizmeti Fiyatlandırma Ayrıntıları'na](https://azure.microsoft.com/pricing/details/app-service)bakın.

Hizmet sınırları, kotalar ve kısıtlamalar tablosu ve her katmandaki desteklenen özellikler için [Uygulama Hizmeti sınırlarına](../azure-resource-manager/management/azure-subscription-service-limits.md#app-service-limits)bakın.

<a name="Next Steps"></a>

## <a name="more-resources"></a>Diğer kaynaklar

[Örnek sayısını el ile veya otomatik olarak ölçeklendirme](../monitoring-and-diagnostics/insights-how-to-scale.md)  
[App Service için PremiumV2 katmanını yapılandırın](app-service-configure-premium-tier.md)

<!-- LINKS -->
[vmsizes]:https://azure.microsoft.com/pricing/details/app-service/
[SQLaccountsbilling]:https://go.microsoft.com/fwlink/?LinkId=234930
[azuresubscriptions]:https://account.windowsazure.com/subscriptions
[portal]: https://portal.azure.com/

<!-- IMAGES -->
[ChooseWHP]: ./media/web-sites-scale/scale1ChooseWHP.png
[ResourceGroup]: ./media/web-sites-scale/scale10ResourceGroup.png
[ScaleDatabase]: ./media/web-sites-scale/scale11SQLScale.png
[GeoReplication]: ./media/web-sites-scale/scale12SQLGeoReplication.png
