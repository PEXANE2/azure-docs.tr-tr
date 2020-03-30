---
title: App Service planları
description: Uygulama Hizmeti planlarının Azure Uygulama Hizmeti'nde nasıl çalıştığını, müşteriye nasıl faturalandırıldıklarını ve ihtiyaçlarınız için bunları nasıl ölçeklendireceklerini öğrenin.
keywords: uygulama hizmeti, azure uygulama hizmeti, ölçek, ölçeklenebilir, ölçeklenebilirlik, uygulama servis planı, uygulama servis maliyeti
ms.assetid: dea3f41e-cf35-481b-a6bc-33d7fc9d01b1
ms.topic: article
ms.date: 11/09/2017
ms.custom: seodec18
ms.openlocfilehash: f1012f8c00de4b19bbf6206408ec1a806e09e54f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77482352"
---
# <a name="azure-app-service-plan-overview"></a>Azure App Service planına genel bakış

App Service'teki uygulamalar bir _App Service planında_ çalışır. App Service planı, bir web uygulamasının birlikte çalıştırılacağı işlem kaynakları kümesini tanımlar. Bu bilgi işlem kaynakları geleneksel web barındırma [_sunucu çiftliğine_](https://wikipedia.org/wiki/Server_farm) benzer. Bir veya daha fazla uygulama aynı bilgi işlem kaynaklarında (veya aynı Uygulama Hizmeti planında) çalışacak şekilde yapılandırılabilir.

Belirli bir bölgede (örneğin, Batı Avrupa) bir Uygulama Hizmeti planı oluşturduğunuzda, o bölgedeki plan için bir dizi işlem kaynağı oluşturulur. Bu Uygulama Hizmeti planına hangi uygulamaları koyarsanız koyun, Uygulama Hizmeti planınızda tanımlandığı şekilde bu hesaplama kaynaklarında çalışır. Her App Service planı şunları tanımlar:

- Bölge (Batı ABD, Doğu ABD vb.)
- VM örneklerinin sayısı
- VM örneklerinin boyutu (Küçük, Orta, Büyük)
- Fiyatlandırma katmanı (Ücretsiz, Paylaşılan, Temel, Standart, Premium, PremiumV2, İzole)

Uygulama Hizmeti planının _fiyatlandırma katmanı,_ hangi App Service özelliklerini aldığınızı ve plan için ne kadar ödeme yaptığınızı belirler. Birkaç fiyatlandırma katmanı kategorisi vardır:

- **Paylaşılan işlem**: **Ücretsiz** ve **Paylaşılan**, iki temel katman, diğer müşterilerin uygulamaları da dahil olmak üzere diğer Uygulama Hizmeti uygulamalarıyla aynı Azure VM'de bir uygulama çalıştırAr. Bu katmanlar, paylaşılan kaynaklar üzerinde çalışan her uygulamaya belirli CPU kotaları atar ve kaynaklar için ölçeklendirme yapılamaz.
- **Özel işlem**: **Temel,** **Standart,** **Premium**ve **PremiumV2** katmanları, özel Azure VM'lerinde uygulamalar çalıştırın. Yalnızca aynı App Service planındaki uygulamalar aynı işlem kaynaklarını kullanır. Katman yükseldikçe ölçeği genişletme için kullanabileceğiniz VM örneği sayısı da artar.
- **Yalıtılmış**: Bu katman, özel Azure Sanal Ağlarda özel Azure VM'leri çalıştırılır. Uygulamalarınıza işlem yalıtımı nın üstüne ağ yalıtımı sağlar. En fazla ölçek genişletme özellikleri sunan katmandır.

[!INCLUDE [app-service-dev-test-note](../../includes/app-service-dev-test-note.md)]

Her katman, App Service özelliklerinin belirli bir alt kümesini de sağlar. Bu özellikler özel etki alanları ve SSL sertifikaları, otomatik ölçekleme, dağıtım yuvaları, yedeklemeler, Trafik Yöneticisi tümleştirmeve daha fazlasını içerir. Katman ne kadar yüksekse, o kadar çok özellik kullanılabilir. Her fiyatlandırma katmanında hangi özelliklerin destekleniyi öğrenmek için [Uygulama Hizmeti planı ayrıntılarına](https://azure.microsoft.com/pricing/details/app-service/plans/)bakın.

<a name="new-pricing-tier-premiumv2"></a>

> [!NOTE]
> Yeni **PremiumV2** fiyatlandırma katmanı, [Dv2 serisi VM'lere](../virtual-machines/dv2-dsv2-series.md) **Standart** katmana kıyasla daha hızlı işlemciler, SSD depolama ve çift bellekten orada oranı sağlar. **PremiumV2,** standart planda bulunan tüm gelişmiş yetenekleri sağlarken, artan örnek sayısı yla daha yüksek ölçekleri de destekler. Mevcut **Premium** katmanında bulunan tüm özellikler **PremiumV2'ye**dahildir.
>
> Diğer özel katmanlara benzer şekilde, bu katman için üç VM boyutu mevcuttur:
>
> - Küçük (bir CPU çekirdeği, 3,5 GB bellek) 
> - Orta (iki CPU çekirdeği, 7 GB bellek) 
> - Büyük (dört CPU çekirdeği, 14 GiB bellek)  
>
> **PremiumV2** fiyatlandırma bilgileri için [Uygulama Hizmeti Fiyatlandırması'na](https://azure.microsoft.com/pricing/details/app-service/)bakın.
>
> Yeni **PremiumV2** fiyatlandırma katmanına başlamak [için Uygulama Hizmeti için PremiumV2 katmanını Yapılandır'a](app-service-configure-premium-tier.md)bakın.

## <a name="how-does-my-app-run-and-scale"></a>Uygulamam nasıl çalışır ve ölçeklendirilir?

**Özgür** ve **Paylaşılan** katmanlarda, bir uygulama paylaşılan bir VM örneğinde CPU dakikaları alır ve ölçeklendirilemez. Diğer katmanlarda, bir uygulama aşağıdaki gibi çalışır ve ölçeklendirir.

Uygulama Hizmeti'nde bir uygulama oluşturduğunuzda, uygulama bir Uygulama Hizmeti planına konur. Uygulama çalıştığında, App Service planında yapılandırılan tüm VM örneklerinde çalışır. Birden çok uygulama aynı Uygulama Hizmeti planındaysa, hepsi aynı VM örneklerini paylaşır. Bir uygulama için birden çok dağıtım yuvanız varsa, tüm dağıtım yuvaları aynı VM örneklerinde de çalışır. Tanılama günlüklerini etkinleştiriyorsanız, yedeklemeler gerçekleştiriyorsanız veya Webİşler çalıştırıyorsanız, bu VM örneklerinde CPU döngüleri ve bellek de kullanırlar.

Bu şekilde, Uygulama Hizmeti planı, App Service uygulamalarının ölçek birimidir. Plan beş VM örneğini çalıştıracak şekilde yapılandırılırsa, plandaki tüm uygulamalar beş örnekte de çalışır. Plan otomatik ölçeklendirme için yapılandırılırsa, plandaki tüm uygulamalar otomatik ölçeklendirme ayarlarına göre birlikte ölçeklendirilir.

Bir uygulamayı ölçeklendirme hakkında daha fazla bilgi için, [ölçekörneği sayımını el ile veya otomatik olarak](../monitoring-and-diagnostics/insights-how-to-scale.md)görün.

<a name="cost"></a>

## <a name="how-much-does-my-app-service-plan-cost"></a>Uygulama Hizmeti planım ne kadara mal olur?

Bu bölümde, Uygulama Hizmeti uygulamalarının nasıl faturalandırıldırıolduğu açıklanmaktadır. Ayrıntılı, bölgeye özel fiyatlandırma bilgileri için [Uygulama Hizmeti Fiyatlandırması'na](https://azure.microsoft.com/pricing/details/app-service/)bakın.

**Ücretsiz** katman dışında, Bir Uygulama Hizmeti planı kullandığı bilgi işlem kaynakları üzerinde saatlik ücret taşır.

- **Paylaşılan** katmanda, her uygulama cpu dakikakotası alır, bu nedenle _her uygulama_ CPU kotası için saatlik olarak ücretlendirilir.
- Özel işlem katmanlarında **(Temel,** **Standart,** **Premium,** **PremiumV2),** Uygulama Hizmeti planı, uygulamaların ölçeklendirildikgin VM örneklerinin sayısını tanımlar, bu nedenle Uygulama Hizmeti planındaki _her VM örneğinin_ saatlik ücreti vardır. Bu VM örnekleri, üzerinde kaç uygulama çalıştırıldığına bakılmaksızın aynı şekilde ücretlendirilir. Beklenmeyen ücretleri önlemek için [bkz.](app-service-plan-manage.md#delete)
- **Yalıtılmış** katmanda, Uygulama Hizmet Ortamı uygulamalarınızı çalıştıran yalıtılmış çalışan sayısını tanımlar ve _her çalışansaatlik_ ücretlendirilir. Ayrıca, Uygulama Hizmeti Ortamı'nı çalıştıran işletme için saatlik taban ücret de vardır.

Kullanabileceğiniz Uygulama Hizmeti özelliklerini kullandığınız için ücret lendirilmezsiniz (özel etki alanlarını, SSL sertifikalarını, dağıtım yuvalarını, yedeklemeleri yapılandırmanız vb.). İstisnalar şunlardır:

- Uygulama Hizmeti Etki Alanları - Azure'dan bir tane satın aldığınızda ve her yıl yenilediğinizde ödeme yapıyorsun.
- Uygulama Hizmet Sertifikaları - Azure'dan bir tane satın aldığınızda ve her yıl yenilediğinizde ödeme yapıyorsun.
- IP tabanlı SSL bağlantıları - Her IP tabanlı SSL bağlantısı için saatlik ücret vardır, ancak bazı **Standart** katman veya üzeri size ücretsiz bir IP tabanlı SSL bağlantısı sağlar. SNI tabanlı SSL bağlantıları ücretsizdir.

> [!NOTE]
> Uygulama Hizmetini başka bir Azure hizmetiyle tümleştirirseniz, bu diğer hizmetlerden gelen ücretleri göz önünde bulundurmanız gerekebilir. Örneğin, uygulamanızı coğrafi olarak ölçeklendirmek için Azure Trafik Yöneticisi'ni kullanıyorsanız, Azure Trafik Yöneticisi de kullanımınıza göre sizden ücret lendirin. Azure'daki çapraz hizmet maliyetinizi tahmin etmek için [Fiyatlandırma hesaplayıcısına](https://azure.microsoft.com/pricing/calculator/)bakın. 
>
>

## <a name="what-if-my-app-needs-more-capabilities-or-features"></a>Uygulamamın daha fazla yetiveya özelliğe ihtiyacı varsa ne olur?

App Service planınızı dilediğiniz zaman büyütebilir veya küçültebilirsiniz. Planın fiyatlandırma katmanını değiştirmek kadar basittir. Başlangıçta düşük fiyatlandırma katmanı seçip daha fazla App Service özelliğine ihtiyaç duyduğunuzda ölçeklendirebilirsiniz.

Örneğin, web uygulamanızı **Ücretsiz** Uygulama Hizmeti planında test etmeye başlayabilir ve hiçbir şey ödemeyin. Web uygulamasına [özel DNS adınızı](app-service-web-tutorial-custom-domain.md) eklemek istediğinizde, planınızı **Paylaşılan** katmana ölçeklendirmeniz gereken bir ölçeklendirmeniz gerekiyor. Daha [sonra, bir SSL bağlama oluşturmak](configure-ssl-bindings.md)istediğinizde, planınızı **Temel** katmana kadar ölçeklendirin. [Hazırlama ortamlarına](deploy-staging-slots.md)sahip olmak istediğinizde, **Standart** katmana kadar ölçeklendirin. Daha fazla çekirdek, bellek veya depolama ya da daha fazla sına ihtiyacınız olduğunda, aynı katmanda daha büyük bir VM boyutuna kadar ölçeklendirin.

Aynı şey tam tersi. Daha yüksek bir seviyenin özelliklerine veya özelliklerine artık ihtiyacınız olmadığını hissettiğinizde, daha düşük bir katmana ölçeklendirebilirsiniz ve bu da size para kazandırır.

Uygulama Hizmeti planını ölçeklendirme hakkında daha fazla bilgi için [azure'da bir uygulamayı ölçeklendir'e](manage-scale-up.md)bakın.

Uygulamanız diğer uygulamalarla aynı Uygulama Hizmeti planındaysa, işlem kaynaklarını yalıtarak uygulamanın performansını artırmak isteyebilirsiniz. Uygulamayı ayrı bir Uygulama Hizmeti planına taşıyarak bunu yapabilirsiniz. Daha fazla bilgi için [bkz.](app-service-plan-manage.md#move)

## <a name="should-i-put-an-app-in-a-new-plan-or-an-existing-plan"></a>Yeni bir plana veya varolan bir plana bir uygulama koymalı mıyım?

Uygulama Hizmeti planınızın ayırdığı bilgi işlem kaynakları için ödeme yaptığınızdan (bkz. [Uygulama Hizmeti planım ne kadara mal olur?](#cost)), birden fazla uygulamayı tek bir Uygulama Hizmeti planına koyarak paradan tasarruf edebilirsiniz. Plan, yükü işlemek için yeterli kaynağa sahip olduğu sürece, varolan bir plana uygulama eklemeye devam edebilirsiniz. Ancak, aynı Uygulama Hizmeti planındaki uygulamaların hepsinin aynı bilgi işlem kaynaklarını paylaştığını unutmayın. Yeni uygulamanın gerekli kaynaklara sahip olup olmadığını belirlemek için var olan App Service planının kapasitesini ve yeni uygulamanın beklenen yükünü anlamanız gerekir. Bir App Service planının aşırı yüklenmesi yeni ve var olan uygulamalarınızın kesinti yaşamasına neden olabilir.

Aşağıdaki durumlarda uygulamanızı yeni bir App Service planında yalıtabilirsiniz:

- Uygulama kaynak yoğun.
- Uygulamayı, varolan plandaki diğer uygulamalardan bağımsız olarak ölçeklendirmek istiyorsunuz.
- Uygulamanın farklı bir coğrafi bölgede kaynağa ihtiyacı vardır.

Bu şekilde uygulamanız için yeni bir kaynak kümesi ayırabilir ve uygulamalarınız üzerinde daha fazla denetim elde edebilirsiniz.

## <a name="manage-an-app-service-plan"></a>Uygulama Hizmeti planını yönetme

> [!div class="nextstepaction"]
> [Uygulama Hizmeti planını yönetme](app-service-plan-manage.md)
