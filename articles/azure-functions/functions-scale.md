---
title: Azure İşlevleri’ni ölçeklendirme ve barındırma
description: Azure İşlevler Tüketim planı ile Premium planı arasında nasıl seçim yapabileceğinizi öğrenin.
ms.assetid: 5b63649c-ec7f-4564-b168-e0a74cb7e0f3
ms.topic: conceptual
ms.date: 03/27/2019
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 92ac0417e9d8adca168dd68e1721a1c9c890de1c
ms.sourcegitcommit: 62c5557ff3b2247dafc8bb482256fef58ab41c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80656954"
---
# <a name="azure-functions-scale-and-hosting"></a>Azure İşlevleri’ni ölçeklendirme ve barındırma

Azure'da bir işlev uygulaması oluşturduğunuzda, uygulamanız için bir barındırma planı seçmeniz gerekir. Azure İşlevleri için üç barındırma planı mevcuttur: [Tüketim planı,](#consumption-plan) [Premium plan](#premium-plan)ve [Özel (App Service) planı.](#app-service-plan)

Seçtiğiniz barındırma planı aşağıdaki davranışları belirler:

* İşlev uygulamanız nasıl ölçeklendirilir.
* Her işlev uygulaması örneği için kullanılabilir kaynaklar.
* Azure Sanal Ağ bağlantısı gibi gelişmiş özellikler için destek.

Hem Tüketim hem de Premium planları, kodunuz çalışırken otomatik olarak işlem gücü ekler. Uygulamanız, yüklemeyi işlemek için gerektiğinde ölçeklendirilir ve kod çalışmayı durdurduğunda ölçeklenir. Tüketim planı için, boştaki VM'ler veya rezerv kapasitesi için önceden ödeme yapmak zorunda değilsiniz.  

Premium plan, premium bilgi işlem örnekleri, örnekleri sonsuza kadar sıcak tutma ve VNet bağlantısı gibi ek özellikler sağlar.

Uygulama Hizmeti planı, yönettiğiniz özel altyapıdan yararlanmanızı sağlar. İşlev uygulamanız olaylara göre ölçeklendirmez, bu da hiçbir zaman sıfıra ölçeklendirmeanlamına gelmez. (Her [Zaman etkinleştirilmesi](#always-on) gerekir.)

## <a name="hosting-plan-support"></a>Hosting planı desteği

Özellik desteği aşağıdaki iki kategoriye ayrılır:

* _Genel olarak kullanılabilir (GA)_: tam olarak desteklenen ve üretim kullanımı için onaylı.
* _Önizleme_: henüz tam olarak desteklenmemiş veya üretim kullanımı için onaylanmadı.

Aşağıdaki tablo, Windows veya Linux üzerinde çalışırken üç barındırma planı için geçerli destek düzeyini gösterir:

| | Tüketim planı | Premium planı | Özel plan |
|-|:----------------:|:------------:|:----------------:|
| Windows | GA | GA | GA |
| Linux | GA | GA | GA |

## <a name="consumption-plan"></a>Tüketim planı

Tüketim planını kullanırken, Azure İşlevler ana bilgisayarının örnekleri gelen olay sayısına bağlı olarak dinamik olarak eklenir ve kaldırılır. Bu sunucusuz plan otomatik olarak ölçeklenir ve yalnızca işlevleriniz çalıştırıldığında işlem kaynakları için ücretlendirilirsiniz. Tüketim planında, yapılandırılabilir bir sürenin sonunda işlev yürütme zaman aşımına uğrar.

Fatura oluşturulurken yürütme sayısı, yürütme süresi ve kullanılan bellek temel alınır. Faturada, işlev uygulamasındaki tüm işlevler toplanır. Daha fazla bilgi için [Azure İşlevleri fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/functions/)bakın.

Tüketim planı varsayılan barındırma planıdır ve aşağıdaki avantajları sunar:

* Yalnızca işlevleriniz çalışırken ödeme
* Yüksek yük dönemlerinde bile otomatik olarak ölçeklendirin

Aynı bölgedeki işlev uygulamaları aynı Tüketim planına atanabilir. Aynı Tüketim planında birden fazla uygulamanın çalıştırılabması hiçbir dezavantajı veya etkisi yoktur. Aynı Tüketim planına birden fazla uygulama atamanın her uygulamanın esnekliği, ölçeklenebilirliği veya güvenilirliği üzerinde hiçbir etkisi yoktur.

Bir Tüketim planında çalışırken maliyetleri nasıl tahmin edebilirsiniz hakkında daha fazla bilgi edinmek için [bkz.](functions-consumption-costs.md)

## <a name="premium-plan"></a><a name="premium-plan"></a>Premium planı

Premium planı kullanırken, Azure İşlevler ana bilgisayarının örnekleri, Tüketim planı gibi gelen olayların sayısına bağlı olarak eklenir ve kaldırılır.  Premium plan aşağıdaki özellikleri destekler:

* Herhangi bir soğuk başlangıç önlemek için sürekli sıcak örnekleri
* VNet bağlantısı
* Sınırsız yürütme süresi (60 dakika garantili)
* Premium örnek boyutları (bir çekirdek, iki çekirdek ve dört temel örnek)
* Daha öngörülebilir fiyatlandırma
* Birden çok işlevli uygulama içeren planlar için yüksek yoğunluklu uygulama tahsisi

Bu seçenekleri nasıl yapılandırabileceğinize ilişkin bilgileri [Azure İşlevler Premium planı belgesinde](functions-premium-plan.md)bulabilirsiniz.

Premium plan için faturalandırma, yürütme başına faturalandırma ve tüketilen bellek yerine, gerekli ve önceden ısıtılmış örneklerde kullanılan temel saniye ve bellek sayısına bağlıdır. En az bir örnek her zaman plan başına sıcak olmalıdır. Bu, yürütme sayısına bakılmaksızın etkin plan başına en az aylık maliyet olduğu anlamına gelir. Premium plandaki tüm işlev uygulamalarının önceden ısıtılmış ve etkin örnekleri paylaştığını unutmayın.

Azure İşlevler Premium planını aşağıdaki durumlarda göz önünde bulundurun:

* İşlev uygulamalarınız sürekli veya neredeyse sürekli çalışır.
* Çok sayıda küçük yürütmeniz var ve yüksek bir yürütme faturanız var, ancak Tüketim planında düşük GB ikinci faturanız var.
* Tüketim planı tarafından sağlanandan daha fazla CPU veya bellek seçeneğine ihtiyacınız vardır.
* Kodunuzun Tüketim planında [izin verilen maksimum yürütme süresinden](#timeout) daha uzun çalışması gerekir.
* Sanal ağ bağlantısı gibi yalnızca Premium planda kullanılabilen özelliklere gereksinim duyarsınız.

Premium planda JavaScript işlevlerini çalıştırırken, daha az vCPUs'u olan bir örnek seçmelisiniz. Daha fazla bilgi için [tek çekirdekli Premium planları seçin'e](functions-reference-node.md#considerations-for-javascript-functions)bakın.  

## <a name="dedicated-app-service-plan"></a><a name="app-service-plan"></a>Özel (App Service) planı

İşlev uygulamalarınız, diğer Uygulama Hizmeti uygulamalarıyla (Temel, Standart, Premium ve İzole STU' lar) aynı özel VM'lerde de çalışabilir.

Aşağıdaki durumlarda bir Uygulama Hizmeti planı düşünün:

* Diğer Uygulama Hizmeti örneklerini zaten çalıştıran varolan, yeterince kullanılmayan VM'lere sahipsiniz.
* İşlevlerinizi çalıştırabileceğiniz özel bir görüntü sağlamak istiyorsunuz.

Aynı ücreti, web uygulamaları gibi diğer Uygulama Hizmeti kaynaklarında olduğu gibi, Bir Uygulama Hizmet Planı'ndaki işlev uygulamaları için de ödersiniz. Uygulama Hizmeti planının nasıl çalıştığı hakkında ayrıntılı bilgi için [Azure Uygulama Hizmeti planlarına ayrıntılı bir bakış](../app-service/overview-hosting-plans.md)bakın.

Bir Uygulama Hizmeti planıyla, daha fazla VM örneği ekleyerek el ile ölçeklendirebilirsiniz. Otomatik ölçeklendirmeyi de etkinleştirebilirsiniz. Daha fazla bilgi için bkz: [Ölçek örneği sayısı el ile veya otomatik olarak.](../azure-monitor/platform/autoscale-get-started.md?toc=%2fazure%2fapp-service%2ftoc.json) Ayrıca farklı bir Uygulama Hizmeti planı seçerek ölçeklendirebilirsiniz. Daha fazla bilgi için [bkz.](../app-service/manage-scale-up.md) 

Bir Uygulama Hizmeti planında JavaScript işlevlerini çalıştırırken, daha az vCPUs'u olan bir plan seçmeniz gerekir. Daha fazla bilgi için bkz: [Tek çekirdekli Uygulama Hizmeti planlarını seçin.](functions-reference-node.md#choose-single-vcpu-app-service-plans) 
<!-- Note: the portal links to this section via fwlink https://go.microsoft.com/fwlink/?linkid=830855 --> 

### <a name="always-on"></a><a name="always-on"></a>Her Zaman Anın

Bir Uygulama Hizmeti planı yla çalışıyorsanız, işlev uygulamanızın doğru çalışması için **Her Zaman** ayarı etkinleştirmelisiniz. Bir Uygulama Hizmeti planında, çalışma süresi birkaç dakika hareketsizkten sonra boşta kalır, böylece yalnızca HTTP tetikleyicileri işlevlerinizi "uyandırır". Her zaman açık yalnızca bir Uygulama Hizmeti planında kullanılabilir. Tüketim planında platform işlev uygulamalarını otomatik olarak etkinleştirir.

[!INCLUDE [Timeout Duration section](../../includes/functions-timeout-duration.md)]


Always On etkin olsa bile, tek tek işlevler `functionTimeout` için yürütme zaman ayarı [host.json](functions-host-json.md#functiontimeout) proje dosyasındaki ayar tarafından denetlenir.

## <a name="determine-the-hosting-plan-of-an-existing-application"></a>Varolan bir uygulamanın barındırma planını belirleme

İşlev uygulamanız tarafından kullanılan barındırma planını belirlemek için, [Azure portalındaki](https://portal.azure.com)işlev uygulamasıiçin **Genel Bakış** sekmesinde Uygulama **Hizmeti planına** bakın. Fiyatlandırma katmanını görmek için Uygulama **Hizmet Planı'nın**adını seçin ve ardından sol bölmeden **Özellikler'i** seçin.

![Portaldaki ölçekleme planını görüntüleme](./media/functions-scale/function-app-overview-portal.png)

Planı belirlemek için Azure CLI'yi aşağıdaki gibi de kullanabilirsiniz:

```azurecli-interactive
appServicePlanId=$(az functionapp show --name <my_function_app_name> --resource-group <my_resource_group> --query appServicePlanId --output tsv)
az appservice plan list --query "[?id=='$appServicePlanId'].sku.tier" --output tsv
```  

Bu komuttan çıktı `dynamic`olduğunda, işlev uygulamanız Tüketim planında dır. Bu komuttan çıktı `ElasticPremium`olduğunda, işlev uygulamanız Premium planında yer alır. Diğer tüm değerler, Bir Uygulama Hizmeti planının farklı katmanlarını gösterir.

## <a name="storage-account-requirements"></a>Depolama hesabı gereksinimleri

Herhangi bir planda, bir işlev uygulaması Azure Blob, Kuyruk, Dosyalar ve Tablo depolama destekleyen genel bir Azure Depolama hesabı gerektirir. Bunun nedeni, Azure İşlevlerinin tetikleyicileri yönetme ve işlev yürütmelerini günlüğe kaydetme gibi işlemler için Azure Depolama'ya dayanmasa da, bazı depolama hesapları kuyrukları ve tabloları desteklemez. Yalnızca blob depolama hesaplarını (premium depolama dahil) ve bölge yedekli depolama çoğaltma içeren genel amaçlı depolama hesaplarını içeren bu hesaplar, bir işlev uygulaması oluşturduğunuzda mevcut **Depolama Hesabı** seçimlerinizden filtrelenir.

İşlev uygulamanız tarafından kullanılan aynı depolama hesabı, uygulama verilerinizi depolamak için tetikleyicileriniz ve bağlamalarınız tarafından da kullanılabilir. Ancak, depolama yoğun işlemler için ayrı bir depolama hesabı kullanmanız gerekir.  

Birden çok işlevli uygulamanın herhangi bir sorun olmadan aynı depolama hesabını paylaşması kesinlikle mümkündür. (Bunun iyi bir örneği, tek bir depolama hesabı gibi davranan Azure Depolama Emülatörü'ni kullanarak yerel ortamınızda birden fazla uygulama geliştirmenizdir.) 

<!-- JH: Does using a Premium Storage account improve perf? -->

Depolama hesabı türleri hakkında daha fazla bilgi edinmek için Azure [Depolama hizmetlerini tanıtma 'ya](../storage/common/storage-introduction.md#azure-storage-services)bakın.

## <a name="how-the-consumption-and-premium-plans-work"></a>Tüketim ve Premium planları nasıl çalışır?

Tüketim ve Premium planlarında, Azure İşlevler altyapısı, işlevlerinin tetiklediği olay sayısına bağlı olarak, Işlevler ana bilgisayarınek ek örnekleri ekleyerek CPU ve bellek kaynaklarını ölçeklendirin. Tüketim planındaki İşlevler ana bilgisayarınher örneği 1,5 GB bellek ve bir CPU ile sınırlıdır.  Ana bilgisayar, bir işlev uygulaması içindeki tüm işlevlerin aynı anda bir örnek ve ölçek içindeki tüm işlevlerin olması anlamına gelen işlev uygulamasının bir örneğidir. Aynı Tüketim planını paylaşan işlev uygulamaları bağımsız olarak ölçeklendirilir.  Premium planda, plan boyutunuz, bu durumda bu plandaki tüm uygulamalar için kullanılabilir belleği ve CPU'yu belirler.  

İşlev kodu dosyaları, işlevin ana depolama hesabındaki Azure Dosyaları paylaşımlarında depolanır. İşlev uygulamasının ana depolama hesabını sildiğinizde, işlev kodu dosyaları silinir ve kurtarılamaz.

### <a name="runtime-scaling"></a>Çalışma zamanı ölçekleme

Azure İşlevleri, olayların hızını izlemek ve ölçeklendirmek mi yoksa ölçeklendirmek mi gerektiğini belirlemek için *ölçek denetleyicisi* adı verilen bir bileşen kullanır. Ölçek denetleyicisi her tetikleyici türü için sezgisel kullanır. Örneğin, bir Azure Kuyruğu depolama tetikleyicisi kullanırken, sıra uzunluğuna ve en eski sıra iletisinin yaşına göre ölçeklendirilir.

Azure İşlevler için ölçek birimi işlev uygulamasıdır. İşlev uygulaması ölçeklendirildiğinde, Azure İşlevleri ana bilgisayarının birden çok örneğini çalıştırmak için ek kaynaklar ayrılır. Tersine, işlem talebi azaldıkça, ölçek denetleyicisi işlev ana bilgisayar örneklerini kaldırır. Bir işlev uygulamasında hiçbir işlev çalışmadığında, örnek sayısı sonunda sıfıra *ölçeklendirilir.*

![Ölçek denetleyicisi olayları izleme ve örnekler oluşturma](./media/functions-scale/central-listener.png)

### <a name="understanding-scaling-behaviors"></a>Ölçekleme davranışlarını anlama

Ölçeklendirme bir dizi etkene göre değişebilir ve tetikleyiciye ve seçilen dile göre farklı ölçeklenebilir. Dikkat edilmesi gereken ölçekleme davranışlarının birkaç incelikleri vardır:

* Tek bir işlev uygulaması yalnızca en fazla 200 örnekle ölçeklendirin. Tek bir örnek olsa da aynı anda birden fazla ileti veya istek işleyebilir, bu nedenle eşzamanlı yürütme sayısı üzerinde ayarlanmış bir sınır yoktur.
* HTTP tetikleyicileri için, saniyede en fazla bir kez yeni örnekler ayrılır.
* HTTP olmayan tetikleyiciler için, en fazla 30 saniyede bir yeni örnekler ayrılır. [Premium planda](#premium-plan)çalışırken ölçekleme daha hızlıdır.
* Servis Veri Servisi tetikleyicileri için, en verimli ölçekleme için kaynaklardaki _hakları yönet'i_ kullanın. _Dinleme_ haklarında, ölçekleme sayısı o kadar doğru değildir, çünkü kuyruk uzunluğu ölçekleme kararlarını bildirmek için kullanılamaz. Hizmet Veri Yolu erişim ilkelerinde hakları ayarlama hakkında daha fazla bilgi edinmek için [Paylaşılan Erişim Yetkilendirme İlkesi'ne](../service-bus-messaging/service-bus-sas.md#shared-access-authorization-policies)bakın.
* Olay Hub tetikleyicileri için, başvuru makalesinde [ölçekleme kılavuzuna](functions-bindings-event-hubs-trigger.md#scaling) bakın. 

### <a name="best-practices-and-patterns-for-scalable-apps"></a>Ölçeklenebilir uygulamalar için en iyi uygulamalar ve desenler

Bir işlev uygulamasının ana bilgisayar yapılandırması, çalışma zamanı ayak izi ve kaynak verimliliği dahil olmak üzere ne kadar iyi ölçeklendireceğini etkileyecek birçok yönü vardır.  Daha fazla bilgi için, [performans hususları makalesinin ölçeklenebilirlik bölümüne](functions-best-practices.md#scalability-best-practices)bakın. Ayrıca, işlevleriniz uygulama ölçeklendikçe bağlantıların nasıl hissettiğini de bilmelisiniz. Daha fazla bilgi için Azure [İşlevlerinde bağlantıları nasıl yönetebilirsiniz'e](manage-connections.md)bakın.

Python ve Node.js'de ölçekleme hakkında daha fazla bilgi için Azure [İşleme Python geliştirici kılavuzuna bakın - Ölçekleme ve eşzamanlılık](functions-reference-python.md#scaling-and-concurrency) ve [Azure İşleçlik Düğümü geliştirici kılavuzu - Ölçekleme ve eşzamanlılık.](functions-reference-node.md#scaling-and-concurrency)

### <a name="billing-model"></a>Faturalama modeli

Farklı planların faturalandırması [Azure İşlevleri fiyatlandırma sayfasında](https://azure.microsoft.com/pricing/details/functions/)ayrıntılı olarak açıklanmıştır. Kullanım, işlev uygulaması düzeyinde toplanır ve yalnızca işlev kodunun yürütüldettiği zamanı sayar. Faturalandırma birimleri şunlardır:

* **Gigabayt saniye (GB-s) cinsinden kaynak tüketimi.** Bir işlev uygulaması içindeki tüm işlevler için bellek boyutu ve yürütme süresinin bir kombinasyonu olarak hesaplanır. 
* **İdamlar**. Olay tetikleyicisine yanıt olarak her işlev yürütüldünde sayılır.

Yararlı sorgular ve tüketim faturası anlamak için nasıl bilgi [fatura SSS](https://github.com/Azure/Azure-Functions/wiki/Consumption-Plan-Cost-Billing-FAQ)bulunabilir.

[Azure Functions pricing page]: https://azure.microsoft.com/pricing/details/functions

## <a name="service-limits"></a>Hizmet sınırlamaları

Aşağıdaki tablo, çeşitli barındırma planlarında çalışırken işlev uygulamaları için geçerli olan sınırları gösterir:

[!INCLUDE [functions-limits](../../includes/functions-limits.md)]
