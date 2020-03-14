---
title: Azure Service Bus coğrafi olağanüstü durum kurtarma | Microsoft Docs
description: Azure Service Bus için coğrafi bölgeleri kullanarak yük devretme ve olağanüstü durum kurtarma gerçekleştirme
services: service-bus-messaging
author: axisc
manager: timlt
editor: spelluru
ms.service: service-bus-messaging
ms.topic: article
ms.date: 01/23/2019
ms.author: aschhab
ms.openlocfilehash: 24d6658733ea38c15f0673d10db3c0ff5ef51c23
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79259584"
---
# <a name="azure-service-bus-geo-disaster-recovery"></a>Azure Service Bus coğrafi olağanüstü durum kurtarma

Azure bölgelerinin veya veri merkezlerinin (hiçbir [kullanılabilirlik](../availability-zones/az-overview.md) alanı kullanılmıyorsa) çalışma süresi kapalı kalma süresi, veri işlemenin farklı bir bölgede veya veri merkezinde çalışmaya devam edebilmesi için kritik öneme sahiptir. Bu nedenle, *coğrafi olağanüstü durum kurtarma* , tüm kuruluşlar için önemli bir özelliktir. Azure Service Bus, ad alanı düzeyinde coğrafi olağanüstü durum kurtarmayı destekler.

Coğrafi olağanüstü durum kurtarma özelliği Service Bus Premium SKU için genel kullanıma sunulmuştur. 

>[!NOTE]
> Coğrafi olağanüstü durum kurtarma şu anda yalnızca meta verilerin (kuyruklar, konular, abonelikler, filtreler) birincil ad alanından eşleştirildiği ikincil ad alanına kopyalanmasını sağlar.

## <a name="outages-and-disasters"></a>Kesintileri ve olağanüstü durumları yönetme

"Kesintiler" ve "olağanüstü durumlar" arasındaki fark dikkate almak önemlidir 

*Kesinti* , Azure Service Bus geçici olarak kullanılamaz ve hizmetin bir mesajlaşma deposu gibi bazı bileşenlerini veya hatta tüm veri merkezini etkileyebilir. Ancak, sorun giderildikten sonra Service Bus yeniden kullanılabilir hale gelir. Genellikle, bir kesinti iletileri veya diğer veri kaybına neden olmaz. Böyle bir kesinti örneği, veri merkezinde güç kesintisi olabilir. Bazı kesintiler yalnızca kısa bağlantı kayıpları geçici ya da ağ sorunları nedeniyle olduğundan. 

*Olağanüstü durum* , Service Bus kümesi, Azure bölgesi veya veri merkezi 'nin kalıcı veya uzun süreli kaybı olarak tanımlanır. Bölge veya veri merkezi olabilir veya yeniden kullanılabilir olmaktan çıkabilir veya saatler veya günler için aşağı sonuna olmayabilir. Ateş, taşmasını veya deprem gibi olağanüstü örnekleridir. Kalıcı hale olağanüstü bir durum, bazı iletileri, olayları ya da diğer veri kaybına neden olabilir. Ancak, çoğu durumda olması gerekir veri kaybı olmadan ve yedekleme veri merkezi olduktan sonra iletileri kurtarılabilir.

Azure Service Bus coğrafi olağanüstü durum kurtarma özelliği bir olağanüstü durum kurtarma çözümüdür. Bu makalede açıklanan iş akışı ve kavramları olağanüstü durum senaryoları ve geçici ve geçici kesintiler için geçerlidir. Microsoft Azure ' de olağanüstü durum kurtarma hakkında ayrıntılı bir tartışma için [Bu makaleye](/azure/architecture/resiliency/disaster-recovery-azure-applications)bakın.   

## <a name="basic-concepts-and-terms"></a>Temel kavramlar ve terimler

Olağanüstü Durum Kurtarma özelliği meta verileri olağanüstü durum kurtarma uygular ve birincil ve ikincil olağanüstü durum kurtarma ad alanlarında kullanır. Coğrafi olağanüstü durum kurtarma özelliğinin yalnızca [PREMIUM SKU](service-bus-premium-messaging.md) için kullanılabilir olduğunu unutmayın. Bir diğer ad üzerinden bağlantı kurulmadığı herhangi bir bağlantı dizesi değişiklik yapılması gerekmez.

Bu makalede aşağıdaki terimler kullanılır:

-  *Diğer ad*: ayarladığınız bir olağanüstü durum kurtarma yapılandırması adı. Diğer ad, tek bir kararlı tam etki alanı adı (FQDN) bağlantı dizesi sağlar. Uygulamaları, bir ad alanına bağlanmak için bu diğer ad bağlantı dizesi kullanır. Diğer ad kullanılması, yük devretme tetiklendiğinde bağlantı dizesinin değişmeden olmasını sağlar.

-  *Birincil/ikincil ad alanı*: diğer ada karşılık gelen ad alanları. Birincil ad "etkin" ve iletileri (Bu, mevcut veya yeni bir ad olabilir) alır. İkincil ad alanı, "pasif" ve iletileri almaz. Her ikisi arasındaki bir meta veri eşitlenmiş olarak olduğundan her ikisi de sorunsuz bir şekilde uygulama kodu veya bağlantı dizesi değişiklik yapmadan iletileri kabul edebilir. Etkin ad alanı iletileri aldığından emin olmak için diğer adı kullanmanız gerekir. 

-  *Meta veriler*: kuyruklar, konu başlıkları ve abonelikler gibi varlıklar; ve ad alanıyla ilişkili hizmetin özellikleri. Varlıklar ve ayarları otomatik olarak çoğaltılır unutmayın. İletiler çoğaltılmaz.

-  *Yük devretme*: ikincil ad alanını etkinleştirme işlemi.

## <a name="setup"></a>Kurulum

Aşağıdaki bölüm, ad alanları arasında eşleştirmeye kurulum için bir genel bakıştır.

![1][]

Kurulum işlemi aşağıdaki gibidir-

1. ***Birincil*** Service Bus Premium ad alanı sağlayın.

2. *Birincil ad alanının sağlandığı farklı*bir bölgede ***Ikincil*** Service Bus Premium ad alanı sağlayın. Bu, farklı veri merkezi bölgelerinde hata yalıtımına izin verir.

3. ***Diğer adı***almak için birincil ad alanı ve ikincil ad alanı arasında eşleştirme oluşturun.

    >[!NOTE] 
    > [Azure Service Bus standart ad alanınızı Azure Service Bus Premium 'a geçirdiyseniz](service-bus-migrate-standard-premium.md), **PS/CLI** veya **REST API**aracılığıyla olağanüstü durum kurtarma yapılandırması oluşturmak için önceden mevcut diğer adı (yani Service Bus standart ad alanı bağlantı dizeniz) kullanmanız gerekir.
    >
    >
    > Bunun nedeni, geçiş sırasında Azure Service Bus standart ad alanı bağlantı dizesinin/DNS adınızın, Azure Service Bus Premium ad alanınız için bir diğer ad haline geldiği bir addır.
    >
    > İstemci uygulamalarınızın, olağanüstü durum kurtarma eşinin ayarlandığı Premium ad alanına bağlanmak için bu diğer adı (örneğin, Azure Service Bus standart ad alanı bağlantı dizesi) kullanmanız gerekir.
    >
    > Olağanüstü durum kurtarma yapılandırmasını kurmak için portalını kullanıyorsanız, Portal bu desteklenmediği uyarısıyla sizin için Özet olur.


4. İstemci uygulamalarınızı coğrafi DR özellikli birincil ad alanına bağlamak için adım 3 ' te elde edilen ***diğer adı*** kullanın. Başlangıçta, diğer ad birincil ad alanını işaret eder.

5. Seçim Yük devretme gerekli olup olmadığını algılamak için bazı izleme ekleyin.

## <a name="failover-flow"></a>Yük devretme akışı

Yük devretme, müşteri tarafından el ile tetiklenir (bir komutla veya komutu tetikleyen istemci iş mantığı aracılığıyla veya hiçbir şekilde Azure tarafından). Bu, müşteriye tam sahiplik ve Azure 'ın omurga üzerinde kesinti çözümlemesi için görünürlük sağlar.

![4][]

Yük devretme tetiklendikten sonra-

1. ***Diğer ad*** bağlantı dizesi, ikincil Premium ad alanını işaret etmek üzere güncelleştirilir.

2. İstemciler (Gönderenler ve alıcılar) Ikincil ad alanına otomatik olarak bağlanır.

3. Birincil ve Ikincil Premium ad alanı arasındaki mevcut eşleştirme bozuk.

Yük devretme başlatıldıktan sonra-

1. Başka bir kesinti oluşursa, yeniden yük devredebilmek istersiniz. Bu nedenle, başka bir pasif ad alanı ayarlama ve eşleştirmesini güncelleştirin. 

2. Yeniden kullanılabilir olduğunda eski birincil ad alanındaki iletileri çekin. Bundan sonra bu ad alanı normal coğrafi kurtarma kurulumunuzu dışında Mesajlaşma için kullanın veya eski birincil ad alanı silin.

> [!NOTE]
> Yalnızca hata iletme semantiği desteklenir. Bu senaryoda, yük devretme ve ardından yeni bir ad alanı ile yeniden eşleştirin. İlk duruma döndürmeden desteklenmiyor; Örneğin SQL kümesi. 

Yük devretme ile izleme sistemlerinden veya özel olarak geliştirilmiş izleme çözümleri ile otomatik hale getirebilirsiniz. Ancak, bu tür Otomasyon ek planlama ve bu makalenin kapsamı dışında olan iş alır.

![2][]

## <a name="management"></a>Yönetim

Bir hata yaptıysanız; Örneğin, ilk kurulum sırasında yanlış bölgede eşleştirilmiş, iki ad alanlarını herhangi bir zamanda eşleştirme bozabilir. Eşleştirilen ad alanları normal bir ad kullanmak istiyorsanız, diğer silin.

## <a name="use-existing-namespace-as-alias"></a>Mevcut ad alanını diğer ad olarak kullan

Üreticileri ve tüketicilerinin bağlantılarını değiştirebir senaryonuz varsa, ad alanı adınızı diğer ad olarak yeniden kullanabilirsiniz. [GitHub 'daki örnek koda buradan](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/GeoDR/SBGeoDR2/SBGeoDR_existing_namespace_name)bakın.

## <a name="samples"></a>Örnekler

[GitHub 'daki örneklerde](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/GeoDR/SBGeoDR2/) yük devretmeyi ayarlama ve başlatma işlemi gösterilmektedir. Bu örnekler aşağıdaki kavramları göstermektedir:

- Coğrafi olağanüstü durum kurtarma 'yı ayarlamak ve etkinleştirmek için Service Bus Azure Resource Manager kullanmak üzere Azure Active Directory için gereken bir .NET örneği ve ayarları.
- Örnek kod yürütmek için gerekli adımlar.
- Mevcut bir ad alanını diğer ad olarak kullanma.
- Alternatif olarak PowerShell veya CLı ile coğrafi olağanüstü durum kurtarmayı etkinleştirme adımları.
- Diğer adı kullanarak geçerli birincil veya ikincil ad alanından [gönderin ve alın](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/GeoDR/TestGeoDR/ConsoleApp1) .

## <a name="considerations"></a>Dikkat edilmesi gerekenler

Bu sürümle birlikte göz önünde tutmak için aşağıdaki konuları göz önünde bulundurun:

1. Planınızı yük devretme saat faktörü düşünmelisiniz. Örneğin, 15-20 dakikadan fazla bağlantısını kaybederseniz, yük devretmeyi başlatmak karar verebilirsiniz.

2. Hiçbir veri çoğaltılır şu anda etkin oturumları değil çoğaltıldığından emin anlamına gelir. Ayrıca, yinelenen algılama ve zamanlanmış iletileri çalışmayabilir. Yeni oturumlar, yeni zamanlanmış iletiler ve yeni yinelemeler çalışacaktır. 

3. Karmaşık bir dağıtılmış altyapının yük devretmesi en az bir kez [prova](/azure/architecture/reliability/disaster-recovery#disaster-recovery-plan) edilmelidir.

4. Varlık eşitleme dakika başına yaklaşık 50-100 varlık biraz zaman alabilir. Abonelikler ve kurallar da varlık olarak sayılır.

## <a name="availability-zones"></a>Kullanılabilirlik Alanları

Service Bus Premium SKU, bir Azure bölgesi içinde hataya yalıtılmış konumlar sağlayarak [kullanılabilirlik alanları](../availability-zones/az-overview.md)de destekler.

> [!NOTE]
> Azure Service Bus Premium için Kullanılabilirlik Alanları desteği yalnızca kullanılabilirlik alanlarının bulunduğu [Azure bölgelerinde](../availability-zones/az-overview.md#services-support-by-region) kullanılabilir.

Kullanılabilirlik alanları, yeni ad alanları üzerinde yalnızca, Azure portalını kullanarak etkinleştirebilirsiniz. Service Bus var olan ad alanlarının geçişini desteklemez. Bölge artıklığı ad alanınızı etkinleştirildikten sonra devre dışı bırakılamıyor.

![3][]

## <a name="next-steps"></a>Sonraki adımlar

- Coğrafi olağanüstü durum kurtarma [REST API başvurusunu buraya](/rest/api/servicebus/disasterrecoveryconfigs)bakın.
- [GitHub 'Da](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/GeoDR/SBGeoDR2/SBGeoDR2)coğrafi olağanüstü durum kurtarma örneğini çalıştırın.
- [Bir diğer ada ileti gönderen](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/Microsoft.ServiceBus.Messaging/GeoDR/TestGeoDR/ConsoleApp1)coğrafi olağanüstü durum kurtarma örneğine bakın.

Service Bus mesajlaşma hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

* [Service Bus kuyrukları, konu başlıkları ve abonelikleri](service-bus-queues-topics-subscriptions.md)
* [Service Bus kuyrukları ile çalışmaya başlama](service-bus-dotnet-get-started-with-queues.md)
* [Service Bus konu başlıklarını ve aboneliklerini kullanma](service-bus-dotnet-how-to-use-topics-subscriptions.md)
* [REST API 'SI](/rest/api/servicebus/) 

[1]: ./media/service-bus-geo-dr/geodr_setup_pairing.png
[2]: ./media/service-bus-geo-dr/geo2.png
[3]: ./media/service-bus-geo-dr/az.png
[4]: ./media/service-bus-geo-dr/geodr_failover_alias_update.png
