---
title: Servis Kumaş Küme Kaynak Yöneticisi - Affinity
description: Azure Hizmet Kumaşı hizmetleri için hizmet yakınlığına genel bakış ve hizmet yakınlığı yapılandırması ile ilgili kılavuz.
services: service-fabric
documentationcenter: .net
author: masnider
ms.topic: conceptual
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 7bfd261802fbf891b8f45079255783cb1e8ac7d4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75551752"
---
# <a name="configuring-and-using-service-affinity-in-service-fabric"></a>Hizmet Kumaşında hizmet afiyetinin yapılandırılması ve kullanılması
Affinity, daha büyük monolitik uygulamaların bulut ve mikro hizmetler dünyasına geçişini kolaylaştırmak için esas olarak sağlanan bir kontroldür. Aynı zamanda hizmetlerin performansını artırmak için bir optimizasyon olarak kullanılır, bunu yaparken yan etkileri olabilir rağmen.

Service Fabric'e (veya dağıtılmış herhangi bir ortama) daha büyük bir uygulama veya mikro hizmetler düşünülerek tasarlanılmamış bir uygulama getirdiğinizi varsayalım. Bu geçiş türü yaygındır. Tüm uygulamayı çevreye kaldırıp paketleyerek ve sorunsuz çalışmasını sağlayarak işe başlarsınız. Sonra da birbirinibirbiriyle konuşan farklı küçük hizmetlere bölmeye başlarsın.

Sonunda uygulama bazı sorunlar yaşıyor bulabilirsiniz. Sorunlar genellikle aşağıdaki kategorilerden birine girer:

1. Yekpare uygulamadaki bazı bileşen X'in Y bileşenine belgesiz bir bağımlılığı vardı ve siz bu bileşenleri ayrı hizmetlere dönüştürdüniz. Bu hizmetler artık kümedeki farklı düğümlerde çalıştığından, bozuklar.
2. Bu bileşenler (yerel adlandırılmış borular | diskteki paylaşılan bellek | dosyaları) üzerinden iletişim kurar ve gerçekten şu anda performans nedenleriyle paylaşılan bir yerel kaynağa yazabilmeleri gerekir. Bu sert bağımlılık daha sonra ortadan kalkar, belki.
3. Her şey yolunda, ama bu iki bileşen aslında geveze / performans duyarlı olduğu ortaya çıkıyor. Bunları ayrı hizmetlere taşıdıklarında genel uygulama performansı artmış veya gecikme süresi artmıştır. Sonuç olarak, genel uygulama beklentileri karşılamıyor.

Bu gibi durumlarda, yeniden düzenleme işimizi kaybetmek istemiyoruz ve monolite geri dönmek istemiyoruz. Son durum bile düz bir optimizasyon olarak arzu edilebilir. Ancak, bileşenleri hizmet olarak doğal olarak çalışacak şekilde yeniden tasarlayane kadar (veya performans beklentilerini başka bir şekilde çözene kadar) yerellik duygusuna ihtiyacımız olacak.

Ne yapmalı? Yakınlığı ortaya çıkarmaya çalışabilirsin.

## <a name="how-to-configure-affinity"></a>Afinite nasıl yapılandırılır?
Yakınlık ayarlamak için, iki farklı hizmet arasında bir yakınlık ilişkisi tanımlarsınız. Yakınlığı, bir hizmeti diğerine "işaret etmek" ve "Bu hizmet yalnızca bu hizmetin çalıştığı yerde çalıştırılabilir" olarak düşünebilirsiniz. Bazen yakınlık bir ebeveyn / alt ilişki olarak bakın (nerede ebeveyn çocuk işaret). Affinity, bir hizmetin yinelemelerinin veya örneklerinin başka bir hizmetin yinelemeleri ile aynı düğümlere yerleştirilmesini sağlar.

```csharp
ServiceCorrelationDescription affinityDescription = new ServiceCorrelationDescription();
affinityDescription.Scheme = ServiceCorrelationScheme.Affinity;
affinityDescription.ServiceName = new Uri("fabric:/otherApplication/parentService");
serviceDescription.Correlations.Add(affinityDescription);
await fabricClient.ServiceManager.CreateServiceAsync(serviceDescription);
```

> [!NOTE]
> Bir çocuk hizmeti yalnızca tek bir yakınlık ilişkisine katılabilir. Çocuğun aynı anda iki ana hizmete bağdaşmasını istiyorsanız, birkaç seçeneğiniz vardır:
> - İlişkileri tersine çevirin (geçerli alt hizmette parentService1 ve parentService2 noktası var) veya
> - Ebeveynlerden birini sözleşmeye göre bir hub olarak belirleyin ve tüm hizmet noktasını bu hizmete sahip olun. 
>
> Kümede ortaya çıkan yerleşim davranışı aynı olmalıdır.
>

## <a name="different-affinity-options"></a>Farklı yakınlık seçenekleri
Affinity birkaç korelasyon şemaları biri ile temsil edilir ve iki farklı modu vardır. Yakınlık en yaygın modu NonAlignedAffinity dediğimiz şeydir. AlignedAffinity'de, farklı hizmetlerin kopyaları veya örnekleri aynı düğümlere yerleştirilir. Diğer mod AlignedAffinity olduğunu. Hizalanmış Yakınlık yalnızca devlet hizmetleri için yararlıdır. İki durumlu hizmetin uyumlu bir afiniteye sahip olarak yapılandırılmak, bu hizmetlerin önceliklerinin birbiriyle aynı düğümlere yerleştirilmesini sağlar. Ayrıca, bu hizmetlerin aynı düğümlere yerleştirilmesi için her bir ikincilik çiftinin olmasına neden olur. Ayrıca , (daha az yaygın olsa da) devlet hizmetleri için NonAlignedAffinity yapılandırmak mümkündür. NonAlignedAffinity için, iki devlet hizmetinin farklı kopyaları aynı düğümler üzerinde çalışır, ancak bunların öncelikler farklı düğümler üzerinde sona erebilir.

<center>

![Yakınlık Modları ve Etkileri][Image1]
</center>

### <a name="best-effort-desired-state"></a>En iyi çaba istenilen devlet
Yakınlık ilişkisi en iyi çabadır. Aynı yürütülebilir işlemde çalışan aynı harmanlama veya güvenilirlik garantileri sağlamaz. Yakınlık ilişkisindeki hizmetler, başarısız olan ve bağımsız olarak taşınabilen temelde farklı varlıklardır. Bu molalar geçici olsa da, yakınlık ilişkisi de bozulabilir. Örneğin, kapasite sınırlamaları, yakınlık ilişkisindeki hizmet nesnelerinden yalnızca bazılarının belirli bir düğüme sığabileceği anlamına gelebilir. Bu gibi durumlarda, bir yakınlık ilişkisi olsa bile, diğer kısıtlamalar nedeniyle uygulanamaz. Bunu yapmak mümkünse, ihlal daha sonra otomatik olarak düzeltilir.

### <a name="chains-vs-stars"></a>Zincirler yıldızlara karşı
Bugün Küme Kaynak Yöneticisi yakınlık ilişkileri zincirlerini modelleemez. Bunun anlamı, bir yakınlık ilişkisinde bir çocuk olan bir hizmetin başka bir yakınlık ilişkisinde ebeveyn olamayacağıdır. Bu tür bir ilişkiyi modellemek istiyorsanız, onu bir zincir yerine bir yıldız olarak modellemeniz gerekir. Bir zincirden bir yıldıza geçmek için, en alttaki çocuk ilk çocuğun ebeveynine ebeveynlik edilir. Hizmetlerinizin düzenlenmesine bağlı olarak, bunu birden çok kez yapmanız gerekebilir. Doğal bir ebeveyn hizmeti yoksa, yer tutucu olarak hizmet veren bir hizmet oluşturmanız gerekebilir. Gereksinimlerinize bağlı olarak, [Uygulama Gruplarına](service-fabric-cluster-resource-manager-application-groups.md)da bakmak isteyebilirsiniz.

<center>

![Yakınlık İlişkileri Bağlamında Zincirler vs. Yıldızlar][Image2]
</center>

Bugün yakınlık ilişkileri hakkında dikkat edilmesi gereken bir diğer şey de varsayılan olarak yön seli olmalarıdır. Bu, yakınlık kuralının yalnızca çocuğun ebeveyne yerleştirdiği anlamına gelir. Ebeveynin çocukla birlikte bulunmasını sağlamaz. Bu nedenle, bir yakınlık ihlali varsa ve herhangi bir nedenle ihlali düzeltmek için çocuğun ebeveynin düğümüne taşınması mümkün değildir, o zaman - ebeveyni çocuğun düğümüne taşımak ihlali düzeltmiş olsa bile - ebeveyn çocuk düğümü. Config [MoveParentToFixAffinityViolation'nin](service-fabric-cluster-fabric-settings.md) doğru ya yıllığına ayarlanması yön öğesini ortadan kaldırır. Ayrıca, farklı hizmetler farklı yaşam döngüleri ile var ve başarısız olabilir ve bağımsız hareket beri yakınlık ilişkisi mükemmel veya anında zorlanabilir dikkat etmek önemlidir. Örneğin, ebeveynin çöktüğu için aniden başka bir düğüme geçemediğini varsayalım. Hizmetleri sürekli, tutarlı ve kullanılabilir tutmak öncelik olduğundan, Küme Kaynak Yöneticisi ve Failover Yöneticisi önce başarısızlığı ele alar. Failover tamamlandıktan sonra yakınlık ilişkisi bozulur, ancak Küme Kaynak Yöneticisi, çocuğun ebeveynle birlikte olmadığını fark edene kadar her şeyin yolunda olduğunu düşünür. Bu tür denetimler periyodik olarak gerçekleştirilir. Küme Kaynak Yöneticisi'nin kısıtlamaları nasıl değerlendirdiği hakkında daha fazla bilgi [bu makalede](service-fabric-cluster-resource-manager-management-integration.md#constraint-types)mevcuttur ve [bu](service-fabric-cluster-resource-manager-balancing.md) makalede, bu kısıtlamaların değerlendirildiği cadence'in nasıl yapılandırılacaklığı nasıl yapılandırılabilmek hakkında daha fazla bilgi vardır.   


### <a name="partitioning-support"></a>Bölümleme desteği
Yakınlık hakkında dikkat edilmesi gereken son şey, yakınlık ilişkilerinin ebeveynin bölümlendiği yerde desteklenmediğidir. Bölümlenmiş üst hizmetler sonunda desteklenebilir, ancak bugün buna izin verilmez.

## <a name="next-steps"></a>Sonraki adımlar
- Hizmetleri yapılandırma hakkında daha fazla bilgi [için, Hizmetleri yapılandırma hakkında bilgi edinin](service-fabric-cluster-resource-manager-configure-services.md)
- Hizmetleri küçük bir makine grubuyla sınırlamak veya hizmet yükünü bir araya getirmek için [Uygulama Grupları'nı](service-fabric-cluster-resource-manager-application-groups.md) kullanın

[Image1]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-affinity/cluster-resrouce-manager-affinity-modes.png
[Image2]:./media/service-fabric-cluster-resource-manager-advanced-placement-rules-affinity/cluster-resource-manager-chains-vs-stars.png