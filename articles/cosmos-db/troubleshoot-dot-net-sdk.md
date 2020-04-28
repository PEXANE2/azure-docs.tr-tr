---
title: Azure Cosmos DB .NET SDK'sını kullanırken karşılaşılan sorunları tanılama ve giderme
description: .NET SDK kullanırken Azure Cosmos DB sorunları tanımlamak, tanılamak ve sorunlarını gidermek için istemci tarafı günlüğe kaydetme gibi özellikleri ve diğer üçüncü taraf araçları kullanın.
author: j82w
ms.service: cosmos-db
ms.date: 03/11/2020
ms.author: jawilley
ms.subservice: cosmosdb-sql
ms.topic: troubleshooting
ms.reviewer: sngun
ms.openlocfilehash: 5f92d98630c6fb875babeb907f92732b0c24bb52
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79137963"
---
# <a name="diagnose-and-troubleshoot-issues-when-using-azure-cosmos-db-net-sdk"></a>Azure Cosmos DB .NET SDK'sını kullanırken karşılaşılan sorunları tanılama ve giderme
Bu makalede, Azure Cosmos DB SQL API hesaplarıyla [.NET SDK](sql-api-sdk-dotnet.md) kullandığınızda yaygın sorunlar, geçici çözümler, Tanılama adımları ve araçlar ele alınmaktadır.
.NET SDK, Azure Cosmos DB SQL API 'sine erişmek için istemci tarafı mantıksal temsili sağlar. Bu makalede, sorunla karşılaştığınızda size yardımcı olacak araçlar ve yaklaşımlar açıklanır.

## <a name="checklist-for-troubleshooting-issues"></a>Sorun giderme sorunları için denetim listesi:
Uygulamanızı üretime taşımadan önce aşağıdaki denetim listesini göz önünde bulundurun. Denetim listesinin kullanılması, görebileceğiniz bazı yaygın sorunları engeller. Ayrıca bir sorun oluştuğunda hızlı bir şekilde tanılama yapabilirsiniz:

*    En son [SDK 'yı](sql-api-sdk-dotnet-standard.md)kullanın. Önizleme SDK 'Ları üretim için kullanılmamalıdır. Bu, zaten düzeltilen bilinen sorunların vurmasını engeller.
*    [Performans ipuçlarını](performance-tips.md)gözden geçirin ve önerilen uygulamaları izleyin. Bu, ölçeklendirmeyi, gecikmeyi ve diğer performans sorunlarını önlemeye yardımcı olur.
*    Bir sorunu gidermenize yardımcı olması için SDK günlüğünü etkinleştirin. Günlüğe kaydetme işleminin etkinleştirilmesi performansı etkileyebilir, bu sayede yalnızca sorun giderme sırasında etkinleştirmek en iyisidir. Aşağıdaki günlükleri etkinleştirebilirsiniz:
    *    Azure portal kullanarak [ölçümleri günlüğe kaydedin](monitor-accounts.md) . Portal ölçümleri, sorunun Azure Cosmos DB veya istemci tarafından olup olmadığını belirlemede faydalı olan Azure Cosmos DB Telemetriyi gösterir.
    *    [Tanılama dizesini](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.resourceresponsebase.requestdiagnosticsstring) v2 SDK 'Sında veya v3 SDK 'daki [tanılamalardan](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmos.responsemessage.diagnostics) gelen işlem yanıtlarından günlüğe kaydedin.
    *    Tüm Sorgu yanıtlarından [SQL sorgu ölçümlerini](sql-api-query-metrics.md) günlüğe kaydet 
    *    [SDK günlüğü]( https://github.com/Azure/azure-cosmos-dotnet-v2/blob/master/docs/documentdb-sdk_capture_etl.md) kurulumunu izleyin

Bu makaledeki [yaygın sorunlar ve geçici çözümler](#common-issues-workarounds) bölümüne göz atın.

Etkin olarak izlenen [GitHub sorunları bölümüne](https://github.com/Azure/azure-cosmos-dotnet-v2/issues) bakın. Geçici bir çözüm ile benzer bir sorun olup olmadığını denetleyin. Bir çözüm bulamıyorsanız bir GitHub sorunu verirsiniz. Acil sorunlar için bir destek çizgisi açabilirsiniz.


## <a name="common-issues-and-workarounds"></a><a name="common-issues-workarounds"></a>Yaygın sorunlar ve geçici çözümler

### <a name="general-suggestions"></a>Genel öneriler
* Uygulamanızı, mümkün olduğunda Azure Cosmos DB hesabınızla aynı Azure bölgesinde çalıştırın. 
* İstemci makinenizde kaynak olmaması nedeniyle bağlantı/kullanılabilirlik sorunlarıyla karşılaşabilirsiniz. Azure Cosmos DB istemcisini çalıştıran düğümlerde CPU kullanımınızı izlemenizi ve yüksek yük üzerinde çalışıyorsa ölçeği büyütmeyi/dışarı izlemeyi öneririz.

### <a name="check-the-portal-metrics"></a>Portal ölçümlerini denetleyin
[Portal ölçümlerinin](monitor-accounts.md) denetlenmesi, istemci tarafı bir sorun olup olmadığını veya hizmette bir sorun olup olmadığını belirlemenize yardımcı olur. Örneğin, ölçümler yüksek bir hız sınırlı istek (HTTP durum kodu 429) içeriyorsa, isteğin azaltıldı, [istek hızı çok büyük] bölümünü kontrol edin. 

### <a name="requests-timeouts"></a><a name="request-timeouts"></a>İstek zaman aşımları
RequestTimeout genellikle doğrudan/TCP kullanılırken gerçekleşir, ancak ağ geçidi modunda gerçekleşebilir. Bu hatalar yaygın olarak bilinen nedenlerdir ve sorunun nasıl düzeltileceğini gösteren önerilerdir.

* CPU kullanımı yüksek, gecikme süresi ve/veya istek zaman aşımına neden olur. Müşteri, daha fazla kaynak vermek için konak makinenin ölçeğini değiştirebilir veya yük daha fazla makineye dağıtılabilir.
* Yuva/bağlantı noktası kullanılabilirliği düşük olabilir. Azure 'da çalışırken, .NET SDK kullanan istemciler Azure SNAT (PAT) bağlantı noktası tükenmesi ile aynı olabilir. Bu sorunu verme olasılığını azaltmak için .NET SDK 'sının en son sürüm 2. x veya 3. x sürümünü kullanın. Bu, en son SDK sürümünü her zaman çalıştırmanın nasıl önerildiğini bir örnektir.
* Birden çok DocumentClient örneği oluşturmak bağlantı çekişmesine ve zaman aşımı sorunlarına yol açabilir. [Performans ipuçlarını](performance-tips.md)izleyin ve bir işlemin tamamında tek bir documentclient örneği kullanın.
* Koleksiyonları bazen yükseltilmiş gecikme süresi veya istek zaman aşımları görebilir, çünkü koleksiyonları yeterince, arka uç kısıtlar ve istemci yeniden denemeler yapılır. [Portal ölçümlerini](monitor-accounts.md)denetleyin.
* Azure Cosmos DB, genel olarak sağlanan üretilen işi fiziksel bölümler arasında eşit olarak dağıtır. İş yükünün bir sıcak [bölüm anahtarıyla](partition-data.md)karşılaşmaya yönelik olup olmadığını görmek için Portal ölçümlerini denetleyin. Bu, toplam tüketilen üretilen iş üretiminin (RU/s), sağlanan RUs kapsamında görünmesine neden olur, ancak tek bölümlü tüketilen üretilen iş hacmi (RU/s), sağlanan aktarım hızını aşacak. 
* Ayrıca, 2,0 SDK, kanal semantiğini doğrudan/TCP bağlantılarına ekler. Aynı anda birden çok istek için bir TCP bağlantısı kullanılır. Bu, belirli durumlarda iki soruna neden olabilir:
    * Yüksek derecede eşzamanlılık, kanalda çekişmeye neden olabilir.
    * Büyük istekler veya yanıtlar, görece düşük eşzamanlılık derecesine sahip olan kanal ve exacerbate çekişmesi üzerinde satır başı engellemeye yol açabilir.
    * Örnek, bu iki kategorinin herhangi birine denk geliyorsa (veya yüksek CPU kullanımı şüpheli ise), bunlar olası azaltmalar olur:
        * Uygulamayı ölçeklendirmeye/dışarı ölçeklendirmeye çalışın.
        * Ayrıca, daha fazla ayrıntı edinmek için SDK günlükleri [Izleme dinleyicisi](https://github.com/Azure/azure-cosmosdb-dotnet/blob/master/docs/documentdb-sdk_capture_etl.md) aracılığıyla yakalanabilir.

### <a name="high-network-latency"></a><a name="high-network-latency"></a>Yüksek ağ gecikmesi
Yüksek ağ gecikmesi, v2 SDK 'sindeki [Tanılama dizesi](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.resourceresponsebase.requestdiagnosticsstring?view=azure-dotnet) veya v3 SDK 'daki [Tanılamalar](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmos.responsemessage.diagnostics?view=azure-dotnet#Microsoft_Azure_Cosmos_ResponseMessage_Diagnostics) kullanılarak belirlenebilir.

Bir [zaman aşımı](#request-timeouts) yoksa ve tanılama, yüksek gecikme süresinin ve `ResponseTime` `RequestStartTime`arasındaki farka göre (Bu örnekte >300 milisaniyelik) açık olduğu tek istekleri gösterir.

```bash
RequestStartTime: 2020-03-09T22:44:49.5373624Z, RequestEndTime: 2020-03-09T22:44:49.9279906Z,  Number of regions attempted:1
ResponseTime: 2020-03-09T22:44:49.9279906Z, StoreResult: StorePhysicalAddress: rntbd://..., ...
```

Bu gecikme süresinin birden çok nedeni olabilir:

* Uygulamanız Azure Cosmos DB hesabınızla aynı bölgede çalışmıyor.
* [Preferredlocations](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.connectionpolicy.preferredlocations) veya [ApplicationRegion](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmos.cosmosclientoptions.applicationregion) yapılandırmanız yanlıştır ve uygulamanızın çalışmakta olduğu konuma farklı bir bölgeye bağlanmaya çalışıyor.
* Yüksek trafikle ilgili olarak ağ arabiriminde bir performans sorunu olabilir. Uygulama Azure sanal makinelerinde çalışıyorsa olası geçici çözümler vardır:
    * [Hızlandırılmış ağ etkin bir sanal makine](../virtual-network/create-vm-accelerated-networking-powershell.md)kullanmayı düşünün.
    * [Mevcut bir sanal makinede hızlandırılmış ağı](../virtual-network/create-vm-accelerated-networking-powershell.md#enable-accelerated-networking-on-existing-vms)etkinleştirin.
    * [Daha yüksek bir uç sanal makine](../virtual-machines/windows/sizes.md)kullanmayı düşünün.

### <a name="azure-snat-pat-port-exhaustion"></a><a name="snat"></a>Azure SNAT (PAT) bağlantı noktası tükenmesi

Uygulamanız [Azure sanal makinelerinde genel IP adresi olmadan](../load-balancer/load-balancer-outbound-connections.md#defaultsnat)dağıtılmışsa, varsayılan olarak [Azure SNAT bağlantı noktaları](../load-balancer/load-balancer-outbound-connections.md#preallocatedports) , sanal makinenizin dışındaki herhangi bir uç noktaya bağlantı kurar. VM 'den Azure Cosmos DB uç noktasına izin verilen bağlantı sayısı [Azure SNAT yapılandırması](../load-balancer/load-balancer-outbound-connections.md#preallocatedports)ile sınırlıdır. Bu durum bağlantının azaltılmasına, bağlantı kapanışına veya yukarıda belirtilen [istek zaman aşımlarına](#request-timeouts)yol açabilir.

 Azure SNAT bağlantı noktaları, yalnızca sanal makinenizin özel bir IP adresi genel bir IP adresine bağlandığında kullanılır. Azure SNAT sınırlamasından kaçınmak için iki geçici çözüm vardır (önceden uygulamanın tamamında tek bir istemci örneği kullanıyor olmanız gerekir):

* Azure Cosmos DB hizmeti uç noktanızı Azure sanal makineler sanal ağınızın alt ağına ekleyin. Daha fazla bilgi için bkz. [Azure sanal ağ hizmeti uç noktaları](../virtual-network/virtual-network-service-endpoints-overview.md). 

    Hizmet uç noktası etkinleştirildiğinde, istekler artık genel bir IP Azure Cosmos DB ' e gönderilmez. Bunun yerine, sanal ağ ve alt ağ kimliği gönderilir. Yalnızca genel IP 'Lere izin veriliyorsa bu değişiklik güvenlik duvarı düşceye neden olabilir. Bir güvenlik duvarı kullanıyorsanız, hizmet uç noktasını etkinleştirdiğinizde, [sanal ağ ACL 'lerini](../virtual-network/virtual-networks-acl.md)kullanarak güvenlik duvarına bir alt ağ ekleyin.
* [Azure sanal makinenize genel IP](../load-balancer/load-balancer-outbound-connections.md#assignilpip)atayın.

### <a name="http-proxy"></a>HTTP proxy 'si
Bir HTTP proxy kullanıyorsanız, SDK `ConnectionPolicy`'da yapılandırılan bağlantı sayısını destekleyediğinden emin olun.
Aksi halde bağlantı sorunlarıyla karşılaşın.

### <a name="request-rate-too-large"></a><a name="request-rate-too-large"></a>İstek hızı çok büyük
' İstek hızı çok büyük ' veya 429 hata kodu hatası, tüketilen üretilen iş (RU/s) [sağlanan aktarım hızını](set-throughput.md)aştığından isteklerin kısıtlandığını gösterir. SDK, belirtilen [yeniden deneme ilkesine](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.connectionpolicy.retryoptions?view=azure-dotnet)göre istekleri otomatik olarak yeniden dener. Bu hatayla sık sık karşılaşırsanız, koleksiyondaki aktarım hızını artırmayı düşünün. 429 hatası olup olmadığınızı görmek için [portalın ölçümlerini](use-metrics.md) denetleyin. Depolama ve istek hacminin eşit bir şekilde dağıtımına neden olduğundan emin olmak için [bölüm anahtarınızı](partitioning-overview.md#choose-partitionkey) gözden geçirin. 

### <a name="slow-query-performance"></a>Yavaş sorgu performansı
Sorgu [ölçümleri](sql-api-query-metrics.md) , sorgunun en fazla zaman harcamanın nerede olduğunu belirlemenize yardımcı olur. Sorgu ölçümlerinde, istemcinin arka ucunda ne kadarının harcandığını görebilirsiniz.
* Arka uç sorgusu hızlı bir şekilde döndürürse ve istemci üzerinde büyük bir zaman harcadıysanız makinedeki yükü kontrol edin. Yeterli miktarda kaynak olmaması ve SDK 'nın yanıtı işlemek için kaynakların kullanılabilir olmasını bekliyor olması olasıdır.
* Arka uç sorgusu yavaşsa, [sorguyu iyileştirmeyi](optimize-cost-queries.md) ve geçerli [Dizin oluşturma ilkesine](index-overview.md) bakmaya çalışın 

 <!--Anchors-->
[Common issues and workarounds]: #common-issues-workarounds
[Enable client SDK logging]: #logging
[İstek hızı çok büyük]: #request-rate-too-large
[Request Timeouts]: #request-timeouts
[Azure SNAT (PAT) port exhaustion]: #snat
[Production check list]: #production-check-list


