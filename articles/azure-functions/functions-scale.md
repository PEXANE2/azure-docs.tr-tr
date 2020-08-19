---
title: Azure İşlevleri’ni ölçeklendirme ve barındırma
description: Azure Işlevleri tüketim planı ve Premium plan arasında seçim yapma hakkında bilgi edinin.
ms.assetid: 5b63649c-ec7f-4564-b168-e0a74cb7e0f3
ms.topic: conceptual
ms.date: 08/17/2020
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 80bb59527f416afd78b992fb12a4ef72956f91b7
ms.sourcegitcommit: 02ca0f340a44b7e18acca1351c8e81f3cca4a370
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/19/2020
ms.locfileid: "88587234"
---
# <a name="azure-functions-scale-and-hosting"></a>Azure İşlevleri’ni ölçeklendirme ve barındırma

Azure 'da bir işlev uygulaması oluşturduğunuzda, uygulamanız için bir barındırma planı seçmeniz gerekir. Azure Işlevleri için kullanılabilen üç temel barındırma planı vardır: [Tüketim planı](#consumption-plan), [Premium plan](#premium-plan)ve [adanmış (App Service) plan](#app-service-plan). Tüm barındırma planları, hem Linux hem de Windows sanal makinelerinde genel kullanıma sunulmuştur (GA).

Seçtiğiniz barındırma planı aşağıdaki davranışları belirler:

* İşlev uygulamanız nasıl ölçeklendirilir.
* Her işlev uygulaması örneği için kullanılabilir kaynaklar.
* Azure sanal ağ bağlantısı gibi gelişmiş özellikler için destek.

Hem tüketim hem de Premium planlar, kodunuz çalışırken otomatik olarak işlem gücü ekler. Uygulamanız, yükü işlemek için gerektiğinde ölçeklendirilir ve kod çalışmayı durdurduktan sonra ölçeklendirilir. Tüketim planı için, boş VM 'Ler için ödeme yapmak veya kapasiteyi önceden ayırmak zorunda kalmazsınız.  

Premium plan, Premium işlem örnekleri gibi ek özellikler sağlar, örneklerin sonsuza kadar sıcak kalmasına ve VNet bağlantısına sahip olmasını sağlar.

App Service planı, yönettiğiniz özel altyapıdan yararlanmanızı sağlar. İşlev uygulamanız olaylara göre ölçeklendirilmez, yani hiçbir şekilde sıfıra ölçeklenmez. ( [Her zaman açık](#always-on) özelliğinin etkinleştirilmesini gerektirir.)

Çeşitli barındırma planları (Kubernetes tabanlı barındırma dahil) arasında ayrıntılı bir karşılaştırma için [barındırma planları karşılaştırma bölümüne](#hosting-plans-comparison)bakın.

## <a name="consumption-plan"></a>Tüketim planı

Tüketim planını kullanırken, Azure Işlevleri ana bilgisayarının örnekleri, gelen olayların sayısına göre dinamik olarak eklenir ve kaldırılır. Bu sunucusuz plan otomatik olarak ölçeklenir ve yalnızca işlevleriniz çalıştırıldığında işlem kaynakları için ücretlendirilirsiniz. Tüketim planında, yapılandırılabilir bir sürenin sonunda işlev yürütme zaman aşımına uğrar.

Fatura oluşturulurken yürütme sayısı, yürütme süresi ve kullanılan bellek temel alınır. Faturada, işlev uygulamasındaki tüm işlevler toplanır. Daha fazla bilgi için bkz. [Azure işlevleri fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/functions/).

Tüketim planı varsayılan barındırma plandır ve aşağıdaki avantajları sunar:

* Yalnızca işlevleriniz çalışırken ödeyin
* Yüksek yük dönemlerinde bile otomatik olarak ölçeği genişletme

Aynı bölgedeki işlev uygulamaları aynı tüketim planına atanabilir. Aynı tüketim planında çalışan birden çok uygulamayı kullanmanın bir kısmı veya etkisi yoktur. Aynı tüketim planına birden fazla uygulamanın atanması, her uygulamanın esnekliği, ölçeklenebilirlik veya güvenilirliğini etkilemez.

Tüketim planında çalışırken maliyetleri tahmin etme hakkında daha fazla bilgi edinmek için bkz. [Tüketim planı maliyetlerini anlama](functions-consumption-costs.md).

## <a name="premium-plan"></a><a name="premium-plan"></a>Premium planı

Premium planı kullanırken, Azure Işlevleri ana bilgisayarının örnekleri, tüketim planı gibi gelen olayların sayısına göre eklenir ve kaldırılır.  Premium plan aşağıdaki özellikleri destekler:

* Soğuk başlangıçtan kaçınmak için adet sürekli sıcak örnekler
* VNet bağlantısı
* Sınırsız yürütme süresi (60 dakika garanti edilir)
* Premium örnek boyutları (bir çekirdek, iki çekirdek ve dört temel örnek)
* Daha öngörülebilir fiyatlandırma
* Birden çok işlev uygulaması olan planlar için yüksek yoğunluklu uygulama ayırma

Premium planda bir işlev uygulaması oluşturma hakkında bilgi edinmek için bkz. [Azure Işlevleri Premium plan](functions-premium-plan.md).

Yürütme başına faturalandırılması ve tüketilen bellek yerine, Premium plan için faturalandırma, gerekli ve önceden çarpımış örneklerde kullanılan çekirdek saniyelik ve belleğin sayısını temel alır. Plan başına en az bir örnek her zaman sıcak olmalıdır. Bu, yürütmelerin sayısı ne olursa olsun, etkin plan başına en az aylık maliyet olduğu anlamına gelir. Premium planındaki tüm işlev uygulamalarının, önceden çarpımış ve etkin örnekleri paylaştığı göz önünde bulundurun.

Aşağıdaki durumlarda Azure Işlevleri Premium planını göz önünde bulundurun:

* İşlev uygulamalarınız sürekli veya neredeyse sürekli çalışır.
* Çok sayıda küçük yürütmeler ve yüksek bir yürütme faturanız, ancak tüketim planında düşük GB ikinci faturanız vardır.
* Tüketim planı tarafından sağlandıkından daha fazla CPU veya bellek seçeneği gerekir.
* Kodunuzun, tüketim planında [izin verilen maksimum yürütme süresinden](#timeout) daha uzun çalışması gerekir.
* Yalnızca bir Premium planda kullanılabilir olan Özellikler (örneğin, sanal ağ bağlantısı) gereklidir. 

## <a name="dedicated-app-service-plan"></a><a name="app-service-plan"></a>Adanmış (App Service) plan

İşlev uygulamalarınız aynı zamanda diğer App Service uygulamalarla aynı ayrılmış VM 'lerde da çalıştırılabilir (temel, standart, Premium ve yalıtılmış SKU 'Lar).

Aşağıdaki durumlarda bir App Service planı düşünün:

* Zaten başka App Service örnekleri çalıştıran, az önce kullanılan VM 'Ler var.
* İşlevlerinizin çalıştırılacağı özel bir görüntü sağlamak istiyorsunuz.

Aynı şekilde, Web Apps gibi diğer App Service kaynaklarda yaptığınız gibi App Service planındaki işlev uygulamaları için de aynı ödeme yaparsınız. App Service planının nasıl çalıştığı hakkında daha fazla bilgi için bkz. [ayrıntılı genel bakış Azure App Service planları](../app-service/overview-hosting-plans.md).

App Service planıyla, daha fazla VM örneği ekleyerek ölçeği el ile değiştirebilirsiniz. Otomatik ölçeklendirme özelliğini de etkinleştirebilirsiniz. Daha fazla bilgi için bkz. [örnek sayısını el ile veya otomatik olarak ölçeklendirme](../azure-monitor/platform/autoscale-get-started.md?toc=%2fazure%2fapp-service%2ftoc.json). Ayrıca, farklı bir App Service planı seçerek ölçeği genişletebilirsiniz. Daha fazla bilgi için bkz. [Azure 'da bir uygulamayı ölçeklendirme](../app-service/manage-scale-up.md). 

JavaScript işlevlerini bir App Service planında çalıştırırken, daha az vCPU içeren bir plan seçmelisiniz. Daha fazla bilgi için bkz. [tek çekirdekli App Service planlarını seçme](functions-reference-node.md#choose-single-vcpu-app-service-plans). 
<!-- Note: the portal links to this section via fwlink https://go.microsoft.com/fwlink/?linkid=830855 --> 

Bir [App Service ortamı](../app-service/environment/intro.md) (ASE) çalıştırmak, işlevlerinizi tamamen yalıtmanıza ve yüksek ölçeğe karşı yararlanmanızı sağlar.

### <a name="always-on"></a><a name="always-on"></a> Her zaman açık

App Service bir planda çalıştırırsanız, işlev uygulamanızın doğru çalışması için **Always on** ayarını etkinleştirmeniz gerekir. Bir App Service planında, işlevler çalışma zamanı birkaç dakikadan sonra boş kalır, bu nedenle yalnızca HTTP Tetikleyicileri "uyandırır" olur. Her zaman açık, yalnızca bir App Service planında kullanılabilir. Tüketim planında, platform işlev uygulamalarını otomatik olarak etkinleştirir.

[!INCLUDE [Timeout Duration section](../../includes/functions-timeout-duration.md)]


Her zaman etkinleştirilmiş olsa bile, tek tek işlevlerde yürütme zaman aşımı, `functionTimeout` Proje dosyasındaki [host.js](functions-host-json.md#functiontimeout) ayarıyla denetlenir.

## <a name="determine-the-hosting-plan-of-an-existing-application"></a>Mevcut bir uygulamanın barındırma planını belirleme

İşlev uygulamanız tarafından kullanılan barındırma planını öğrenmek için, [Azure Portal](https://portal.azure.com)işlev uygulaması Için **genel bakış** sekmesinde **App Service plan** bölümüne bakın. Fiyatlandırma katmanını görmek için **App Service planının**adını seçin ve ardından sol bölmedeki **Özellikler** ' i seçin.

![Portalda ölçeklendirme planını görüntüleme](./media/functions-scale/function-app-overview-portal.png)

Planı aşağıdaki gibi öğrenmek için Azure CLı de kullanabilirsiniz:

```azurecli-interactive
appServicePlanId=$(az functionapp show --name <my_function_app_name> --resource-group <my_resource_group> --query appServicePlanId --output tsv)
az appservice plan list --query "[?id=='$appServicePlanId'].sku.tier" --output tsv
```  

Bu komutun çıktısı olduğunda `dynamic` , işlev uygulamanız tüketim planınızdan olur. Bu komutun çıktısı olduğunda `ElasticPremium` , işlev uygulamanız Premium plandır. Diğer tüm değerler App Service planının farklı katmanlarını gösterir.

## <a name="storage-account-requirements"></a>Depolama hesabı gereksinimleri

Herhangi bir planda, bir işlev uygulaması Azure blob, kuyruk, dosyalar ve tablo depolamayı destekleyen genel bir Azure depolama hesabı gerektirir. Bunun nedeni, Azure Işlevlerinin Tetikleyicileri yönetme ve işlev yürütmelerini yönetme gibi işlemler için Azure Storage 'ı kullandığından, bazı depolama hesaplarının kuyrukları ve tabloları desteklememesinden kaynaklanır. Yalnızca BLOB depolama hesapları (Premium Depolama dahil) ve bölgesel olarak yedekli depolama çoğaltması olan genel amaçlı depolama hesapları dahil olmak üzere bu hesaplar, bir işlev uygulaması oluşturduğunuzda mevcut **depolama hesabı** Seçimlerinizden filtrelenmiştir.

İşlev uygulamanız tarafından kullanılan depolama hesabı, Tetikleyiciniz ve bağlamalarınız tarafından, uygulama verilerinizi depolamak için de kullanılabilir. Ancak, depolama yoğun işlemler için ayrı bir depolama hesabı kullanmanız gerekir.  

Birden çok işlevli uygulamanın herhangi bir sorun olmadan aynı depolama hesabını paylaşması mümkündür. (Bunun iyi bir örneği, bir depolama hesabı gibi davranan Azure Storage öykünücüsü kullanarak yerel ortamınızda birden çok uygulama geliştirmektir.) 

<!-- JH: Does using a Premium Storage account improve perf? -->

Depolama hesabı türleri hakkında daha fazla bilgi edinmek için bkz. [Azure Storage Services 'A giriş](../storage/common/storage-introduction.md#core-storage-services).

## <a name="how-the-consumption-and-premium-plans-work"></a>Tüketim ve Premium plan türleri nasıl çalışır?

Tüketim ve Premium planlarında, Azure Işlevleri altyapısı, işlevlerinin tetiklendiği olay sayısına göre Işlevler ana bilgisayarının ek örneklerini ekleyerek CPU ve bellek kaynaklarını ölçeklendirir. Tüketim planında Işlevlerin ana bilgisayarının her örneği 1,5 GB bellek ve bir CPU ile sınırlıdır.  Ana bilgisayarın bir örneği, bir işlev uygulamasının tüm işlevleri bir örnek içinde kaynak paylaşır ve aynı anda ölçeklendirin. Aynı tüketim planını paylaşan işlev uygulamaları bağımsız olarak ölçeklendirilir.  Premium planda, plan boyutunuz söz konusu örnekteki plandaki tüm uygulamalar için kullanılabilir bellek ve CPU 'YU tespit edecektir.  

İşlev kodu dosyaları, işlevin ana depolama hesabındaki Azure dosya paylaşımlarında depolanır. İşlev uygulamasının ana depolama hesabını sildiğinizde, işlev kodu dosyaları silinir ve kurtarılamaz.

### <a name="runtime-scaling"></a>Çalışma zamanı ölçeklendirme

Azure Işlevleri, olayların oranını izlemek ve ölçeğini genişletmek veya ölçeklendirmek için *Ölçek denetleyicisi* adlı bir bileşen kullanır. Ölçek denetleyicisi her tetikleyici türü için buluşsal yöntemler kullanır. Örneğin, bir Azure kuyruk depolama tetikleyicisi kullanırken, sıra uzunluğuna ve en eski sıra iletisinin yaşa göre ölçeklendirilir.

Azure Işlevleri için ölçek birimi, işlev uygulamasıdır. İşlev uygulaması ölçeklenirse, Azure Işlevleri ana bilgisayarının birden çok örneğini çalıştırmak için ek kaynaklar ayrılır. Buna karşılık, işlem talebi azaltıldı, ölçek denetleyicisi işlev ana bilgisayar örneklerini kaldırır. İşlev uygulaması içinde hiçbir işlev çalışmadığı zaman örneklerin sayısı sonunda sıfıra *ölçeklendirilir* .

![Denetleyici izleme olaylarını ölçeklendirme ve örnek oluşturma](./media/functions-scale/central-listener.png)

### <a name="cold-start"></a>Soğuk başlangıç

İşlev uygulamanız birkaç dakika boyunca boşta kaldıktan sonra, Platform, uygulamanızın çalıştırıldığı örneklerin sayısını sıfıra ölçeklendirebilir. Sonraki istek, sıfırdan diğerine ölçeklendirmeye yönelik ek gecikme süresine sahiptir. Bu gecikme süresi _soğuk başlangıç_olarak adlandırılır. İşlev uygulamanız tarafından yüklenmesi gereken bağımlılıkların sayısı, soğuk başlangıç süresini etkileyebilir. Soğuk başlatma, yanıt döndürmesi gereken HTTP Tetikleyicileri gibi zaman uyumlu işlemler için bir sorundur. Soğuk başlıyorsa işlevlerinizi etkilese, her zaman etkin olan bir Premium planda veya adanmış bir planda çalışmayı düşünün.   

### <a name="understanding-scaling-behaviors"></a>Ölçeklendirme davranışlarını anlama

Ölçeklendirme, bir dizi etkene göre farklılık gösterebilir ve seçilen tetikleyici ve dile göre farklı şekilde ölçeklendirebilir. ' Nin farkında olması için ölçeklendirmenin bazı çok karmaşık özellikleri vardır:

* Tek bir işlev uygulaması yalnızca en fazla 200 örneğe ölçeklendirir. Tek bir örnek aynı anda birden fazla ileti veya isteği işleyebilir, bu nedenle eşzamanlı yürütmeler sayısında bir küme sınırı yoktur.  Ölçeği azaltmak için [daha düşük bir değer belirtebilirsiniz](#limit-scale-out) .
* HTTP Tetikleyicileri için, saniyede en çok bir kez yeni örnekler ayrılır.
* HTTP olmayan Tetikleyiciler için, en çok, her 30 saniyede bir yeni örnek ayrılır. Bir [Premium planda](#premium-plan)çalışırken ölçekleme daha hızlıdır.
* Service Bus Tetikleyiciler için, en verimli ölçekleme için kaynaklardaki hakları _Yönet_ ' i kullanın. _Dinleme_ haklarıyla, ölçek kararlarını bilgilendirmek için sıra uzunluğu kullanılamadığından ölçekleme doğru değildir. Service Bus erişim ilkelerinde hakları ayarlama hakkında daha fazla bilgi edinmek için bkz. [paylaşılan erişim yetkilendirme ilkesi](../service-bus-messaging/service-bus-sas.md#shared-access-authorization-policies).
* Olay Hub 'ı Tetikleyicileri için başvuru makalesindeki [ölçeklendirme Kılavuzu](functions-bindings-event-hubs-trigger.md#scaling) ' na bakın. 

### <a name="limit-scale-out"></a>Ölçeği genişletme sınırı

Bir uygulamanın ölçeklendirilen örnek sayısını kısıtlamak isteyebilirsiniz.  Bu, bir veritabanı gibi bir aşağı akış bileşeninin aktarım hızını sınırlı olduğu durumlarda yaygın olarak kullanılır.  Varsayılan olarak, tüketim planı işlevleri en fazla 200 örneğe kadar ölçeklendirecektir ve Premium plan işlevleri, en fazla 100 örneğe kadar ölçeklendirecektir.  Değeri değiştirerek belirli bir uygulama için daha düşük bir en yüksek değer belirtebilirsiniz `functionAppScaleLimit` .  `functionAppScaleLimit`Sınırsız için 0 veya null ya da 1 ile uygulama maksimum değeri arasında geçerli bir değer ayarlanabilir.

```azurecli
az resource update --resource-type Microsoft.Web/sites -g <resource_group> -n <function_app_name>/config/web --set properties.functionAppScaleLimit=<scale_limit>
```

### <a name="best-practices-and-patterns-for-scalable-apps"></a>Ölçeklenebilir uygulamalar için en iyi uygulamalar ve desenler

Konak yapılandırması, çalışma zamanı kaplama ve kaynak verimliliği dahil, ne kadar iyi ölçeklendirilemeyeceğini etkileyecek bir işlev uygulamasının birçok yönü vardır.  Daha fazla bilgi için [performans konuları makalesinin ölçeklenebilirlik bölümüne](functions-best-practices.md#scalability-best-practices)bakın. Ayrıca, işlev uygulamanız ölçeklenirken bağlantıların nasıl davranacağını de bilmelisiniz. Daha fazla bilgi için bkz. [Azure işlevlerinde bağlantıları yönetme](manage-connections.md).

Python ve Node.js ölçeklendirme hakkında daha fazla bilgi için bkz. [Azure Işlevleri Python Geliştirici Kılavuzu-ölçeklendirme ve eşzamanlılık](functions-reference-python.md#scaling-and-concurrency) ve [Azure işlevleri Node.js Geliştirici Kılavuzu-ölçeklendirme ve eşzamanlılık](functions-reference-node.md#scaling-and-concurrency).

### <a name="billing-model"></a>Faturalama modeli

Farklı planların [faturalandırılması, Azure işlevleri fiyatlandırma sayfasında](https://azure.microsoft.com/pricing/details/functions/)ayrıntılı olarak açıklanmıştır. Kullanım işlevi uygulama düzeyinde toplanır ve yalnızca işlev kodunun yürütüldüğü süreyi sayar. Faturalandırma için birimler aşağıda verilmiştir:

* **Gigabayt-saniye cinsinden kaynak tüketimi (GB-s)**. Bir işlev uygulaması içindeki tüm işlevler için bellek boyutu ve yürütme süresinin birleşimi olarak hesaplanır. 
* **Yürütmeler**. Bir olay tetikleyicisine yanıt olarak bir işlev yürütüldüğünde her seferinde sayılır.

Tüketim faturanızı nasıl anlayacağınızı öğrenmek için faydalı sorgular ve bilgiler [faturalandırma hakkında SSS bölümünde](https://github.com/Azure/Azure-Functions/wiki/Consumption-Plan-Cost-Billing-FAQ)bulunabilir.

[Azure Functions pricing page]: https://azure.microsoft.com/pricing/details/functions

## <a name="hosting-plans-comparison"></a>Barındırma planlarını karşılaştırma

Aşağıdaki karşılaştırma tablosu, Azure Işlevleri uygulama barındırma planı seçimi kararına yardımcı olan tüm önemli yönleri gösterir:

### <a name="plan-summary"></a>Plan Özeti
| | |
| --- | --- |  
|**[Tüketim planı](#consumption-plan)**| Otomatik olarak ölçeklendirin ve işlevleriniz çalışırken yalnızca işlem kaynakları için ödeme yapın. Tüketim planında, Işlevler ana bilgisayarının örnekleri, gelen olayların sayısına göre dinamik olarak eklenir ve kaldırılır.<br/> ✔ Varsayılan barındırma planı.<br/>✔ Yalnızca işlevleriniz çalışırken ödeyin.<br/>yüksek yük dönemlerinde bile otomatik olarak genişleme ✔.|  
|**[Premium planı](#premium-plan)**|Talebe göre otomatik olarak ölçeklendirirken, boşta kaldıktan sonra gecikme olmadan uygulamaları çalıştırmak için önceden çarpımış çalışanları kullanın, daha güçlü örnekler üzerinde çalışır ve sanal ağlara bağlanın. App Service planının tüm özelliklerine ek olarak aşağıdaki durumlarda Azure Işlevleri Premium planını göz önünde bulundurun: <br/>İşlev uygulamalarınızın sürekli veya neredeyse sürekli çalışmasını ✔.<br/>✔ Yüksek sayıda küçük yürütmeler ve yüksek bir yürütme faturanız, ancak tüketim planında düşük GB ikinci faturanız vardır.<br/>✔ Tüketim planı tarafından sağlandıkından daha fazla CPU veya bellek seçeneği gerekir.<br/>Kodunuzun, tüketim planında izin verilen en uzun yürütme süresinden daha uzun süre çalışmasını ✔.<br/>✔ Yalnızca, sanal ağ bağlantısı gibi Premium bir planda kullanılabilir olan özellikler gerektirir.|  
|**[Adanmış plan](#app-service-plan)**<sup>1</sup>|İşlevlerinizi düzenli App Service plan ücretlerindeki bir App Service planı içinde çalıştırın. Uzun süre çalışan işlemlere uygun, ayrıca daha tahmine dayalı ölçekleme ve maliyetlerin gerekli olduğu durumlarda iyi şekilde uyum sağlar. Aşağıdaki durumlarda bir App Service planı düşünün:<br/>Daha önce başka App Service örnekleri çalıştıran mevcut, az kullanılan VM 'Ler ✔.<br/>İşlevlerinizin çalıştırılacağı özel bir görüntü sağlamak ✔.|  
|**[Ao](#app-service-plan)**<sup>1</sup>|App Service Ortamı (ASE), yüksek ölçekte App Service uygulamaları güvenli bir şekilde çalıştırmak için tam olarak yalıtılmış ve ayrılmış bir ortam sağlayan bir App Service özelliğidir. ASEs, gereken uygulama iş yükleri için uygundur: <br/>Çok yüksek ölçekli ✔.<br/>Yalıtım ve güvenli ağ erişimi ✔.<br/>✔ Yüksek bellek kullanımı.|  
| **[Kubernetes](functions-kubernetes-keda.md)** | Kubernetes, Kubernetes platformunun üstünde çalışan tam yalıtılmış ve ayrılmış bir ortam sağlar.  Kubernetes şunları gerektiren uygulama iş yükleri için uygundur: <br/>Özel donanım gereksinimlerini ✔.<br/>Yalıtım ve güvenli ağ erişimi ✔.<br/>Karma veya çok bulut ortamında çalıştırmak ✔.<br/>✔, Mevcut Kubernetes uygulamaları ve hizmetleriyle birlikte çalışır.|  

<sup>1</sup> çeşitli App Service planı seçeneklerine yönelik belirli sınırlar için, [App Service plan sınırlarına](../azure-resource-manager/management/azure-subscription-service-limits.md#app-service-limits)bakın.

### <a name="operating-systemruntime"></a>İşletim sistemi/çalışma zamanı

| | Linux<sup>1</sup><br/>Yalnızca kod | Windows<sup>2</sup><br/>Yalnızca kod | Linux<sup>1, 3</sup><br/>Docker kapsayıcısı |
| --- | --- | --- | --- |
| **[Tüketim planı](#consumption-plan)** | .NET Core<br/>Node.js<br/>Java<br/>Python | .NET Core<br/>Node.js<br/>Java<br/>PowerShell Core | Destek yok  |
| **[Premium planı](#premium-plan)** | .NET Core<br/>Node.js<br/>Java<br/>Python|.NET Core<br/>Node.js<br/>Java<br/>PowerShell Core |.NET Core<br/>Node.js<br/>Java<br/>PowerShell Core<br/>Python  | 
| **[Adanmış plan](#app-service-plan)**<sup>4</sup> | .NET Core<br/>Node.js<br/>Java<br/>Python|.NET Core<br/>Node.js<br/>Java<br/>PowerShell Core |.NET Core<br/>Node.js<br/>Java<br/>PowerShell Core<br/>Python |
| **[Ao](#app-service-plan)**<sup>4</sup> | .NET Core<br/>Node.js<br/>Java<br/>Python |.NET Core<br/>Node.js<br/>Java<br/>PowerShell Core  |.NET Core<br/>Node.js<br/>Java<br/>PowerShell Core<br/>Python | 
| **[Kubernetes](functions-kubernetes-keda.md)** | yok | yok |.NET Core<br/>Node.js<br/>Java<br/>PowerShell Core<br/>Python |

<sup>1</sup> Linux, Python çalışma zamanı yığını için desteklenen tek işletim sistemidir.  
<sup>2</sup> Windows, PowerShell çalışma zamanı yığını için desteklenen tek işletim sistemidir.   
<sup>3</sup> Linux, Docker kapsayıcıları için desteklenen tek işletim sistemidir.
<sup>4</sup> çeşitli App Service planı seçeneklerine yönelik belirli sınırlar için, [App Service plan sınırlarına](../azure-resource-manager/management/azure-subscription-service-limits.md#app-service-limits)bakın.

### <a name="scale"></a>Ölçek

| | Ölçeği genişletme | En fazla # örnek |
| --- | --- | --- |
| **[Tüketim planı](#consumption-plan)** | Olay odaklı. Yüksek yük dönemlerinde bile otomatik olarak ölçeklendirin. Azure Işlevleri altyapısı, işlevlerinin tetiklendiği olay sayısına göre Işlevler ana bilgisayarının ek örneklerini ekleyerek CPU ve bellek kaynaklarını ölçeklendirir. | 200 |
| **[Premium planı](#premium-plan)** | Olay odaklı. Yüksek yük dönemlerinde bile otomatik olarak ölçeklendirin. Azure Işlevleri altyapısı, işlevlerinin tetiklendiği olay sayısına göre Işlevler ana bilgisayarının ek örneklerini ekleyerek CPU ve bellek kaynaklarını ölçeklendirir. |100|
| **[Adanmış plan](#app-service-plan)**<sup>1</sup> | El ile/otomatik ölçeklendirme |10-20|
| **[Ao](#app-service-plan)**<sup>1</sup> | El ile/otomatik ölçeklendirme |100 |
| **[Kubernetes](functions-kubernetes-keda.md)**  | [Keda](https://keda.sh)kullanarak Kubernetes kümeleri için olay odaklı otomatik ölçeklendirme. | &nbsp;Kümeye göre değişir &nbsp; .&nbsp;&nbsp;|

<sup>1</sup> çeşitli App Service planı seçeneklerine yönelik belirli sınırlar için, [App Service plan sınırlarına](../azure-resource-manager/management/azure-subscription-service-limits.md#app-service-limits)bakın.

### <a name="cold-start-behavior"></a>Soğuk başlangıç davranışı

|    |    | 
| -- | -- |
| **[Tüketim &nbsp; planı](#consumption-plan)** | Uygulamalar belirli bir süre boşta kalırsa sıfıra ölçeklendirebilir, bazı isteklerin başlangıçta ek gecikme süresine sahip olabileceğini belirtir.  Tüketim planı, işlev Konağı ve dil işlemlerini çalıştıran önceden çarpımış yer tutucu işlevlerden çekme dahil olmak üzere soğuk başlangıç süresini azaltmanıza yardımcı olmak için bazı iyileştirmeler içerir. |
| **[Premium planı](#premium-plan)** | Tüm soğuk başlarını önlemek için sıcak örnekler adet sürekli. |
| **[Adanmış plan](#app-service-plan)**<sup>1</sup> | Adanmış bir planda çalışırken, Işlevler ana bilgisayarı sürekli çalışabilir ve bu da soğuk başlatması aslında bir sorun değildir. |
| **[Ao](#app-service-plan)**<sup>1</sup> | Adanmış bir planda çalışırken, Işlevler ana bilgisayarı sürekli çalışabilir ve bu da soğuk başlatması aslında bir sorun değildir. |
| **[Kubernetes](functions-kubernetes-keda.md)**  | KEDA yapılandırmasına bağımlıdır. Uygulamalar her zaman çalışacak şekilde yapılandırılabilir ve soğuk başlatmaya hiçbir zaman sahip olmaz ya da sıfıra ölçeklenebilen şekilde yapılandırılmıştır. Bu, yeni olaylarda soğuk başlar. 

<sup>1</sup> çeşitli App Service planı seçeneklerine yönelik belirli sınırlar için, [App Service plan sınırlarına](../azure-resource-manager/management/azure-subscription-service-limits.md#app-service-limits)bakın.

### <a name="service-limits"></a>Hizmet sınırlamaları

[!INCLUDE [functions-limits](../../includes/functions-limits.md)]

### <a name="networking-features"></a>Ağ özellikleri

[!INCLUDE [functions-networking-features](../../includes/functions-networking-features.md)]

### <a name="billing"></a>Faturalandırma

| | | 
| --- | --- |
| **[Tüketim planı](#consumption-plan)** | Yalnızca işlevlerinizin çalıştığı süre için ödeme yapın. Fatura oluşturulurken yürütme sayısı, yürütme süresi ve kullanılan bellek temel alınır. |
| **[Premium planı](#premium-plan)** | Premium plan, gerekli ve önceden çarpımış örnekler genelinde kullanılan çekirdek saniye ve bellek sayısını temel alır. Plan başına en az bir örnek her zaman sıcak tutulmalıdır. Bu plan, daha öngörülebilir fiyatlandırma sağlar. |
| **[Adanmış plan](#app-service-plan)**<sup>1</sup> | Aynı şekilde, Web Apps gibi diğer App Service kaynaklarda yaptığınız gibi App Service planındaki işlev uygulamaları için de aynı ödeme yaparsınız.|
| **[Ao](#app-service-plan)**<sup>1</sup> | altyapı için ödeme yapan ve asa 'nın boyutuyla değişmeyen bir asa için sabit bir aylık ücret vardır. Ayrıca, App Service plan vCPU 'Su başına bir maliyet vardır. ASE'de barındırılan tüm uygulamalar, Yalıtılmış fiyatlandırma SKU’su içindedir. |
| **[Kubernetes](functions-kubernetes-keda.md)**| Yalnızca Kubernetes kümenizin maliyetlerini ödeyin; Işlevler için ek faturalandırma yok. İşlev uygulamanız, kümenizin üstünde, normal bir uygulama gibi bir uygulama iş yükü olarak çalışır. |

<sup>1</sup> çeşitli App Service planı seçeneklerine yönelik belirli sınırlar için, [App Service plan sınırlarına](../azure-resource-manager/management/azure-subscription-service-limits.md#app-service-limits)bakın.

## <a name="next-steps"></a>Sonraki adımlar

+ [Hızlı başlangıç: Visual Studio Code kullanarak bir Azure Işlevleri projesi oluşturma](functions-create-first-function-vs-code.md)
+ [Azure Işlevlerinde dağıtım teknolojileri](functions-deployment-technologies.md) 
+ [Azure İşlevleri geliştirici kılavuzu](functions-reference.md)
