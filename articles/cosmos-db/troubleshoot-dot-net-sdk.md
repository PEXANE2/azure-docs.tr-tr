---
title: Azure Cosmos DB .NET SDK'sını kullanırken karşılaşılan sorunları tanılama ve giderme
description: .NET SDK kullanırken Azure Cosmos DB sorunları tanımlamak, tanılamak ve sorunlarını gidermek için istemci tarafı günlüğe kaydetme gibi özellikleri ve diğer üçüncü taraf araçları kullanın.
author: anfeldma-ms
ms.service: cosmos-db
ms.date: 06/16/2020
ms.author: anfeldma
ms.subservice: cosmosdb-sql
ms.topic: troubleshooting
ms.reviewer: sngun
ms.openlocfilehash: b24c0b045bc7d894496a59eda00f0e8835ea6a8d
ms.sourcegitcommit: e3c28affcee2423dc94f3f8daceb7d54f8ac36fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2020
ms.locfileid: "84887376"
---
# <a name="diagnose-and-troubleshoot-issues-when-using-azure-cosmos-db-net-sdk"></a>Azure Cosmos DB .NET SDK'sını kullanırken karşılaşılan sorunları tanılama ve giderme

> [!div class="op_single_selector"]
> * [Java SDK’sı v4](troubleshoot-java-sdk-v4-sql.md)
> * [Zaman uyumsuz Java SDK v2](troubleshoot-java-async-sdk.md)
> * [.NET](troubleshoot-dot-net-sdk.md)
> 

Bu makalede, Azure Cosmos DB SQL API hesaplarıyla [.NET SDK](sql-api-sdk-dotnet.md) kullandığınızda yaygın sorunlar, geçici çözümler, Tanılama adımları ve araçlar ele alınmaktadır.
.NET SDK, Azure Cosmos DB SQL API 'sine erişmek için istemci tarafı mantıksal temsili sağlar. Bu makalede, sorunla karşılaştığınızda size yardımcı olacak araçlar ve yaklaşımlar açıklanır.

## <a name="checklist-for-troubleshooting-issues"></a>Sorun giderme sorunları için denetim listesi
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

Bir [zaman aşımı](#request-timeouts) yoksa ve tanılama, yüksek gecikme süresinin ve arasındaki farka göre `ResponseTime` `RequestStartTime` (Bu örnekte >300 milisaniyelik) açık olduğu tek istekleri gösterir.

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
* [Azure sanal makinenize genel IP](../load-balancer/troubleshoot-outbound-connection.md#assignilpip)atayın.

### <a name="http-proxy"></a>HTTP proxy 'si
Bir HTTP proxy kullanıyorsanız, SDK 'da yapılandırılan bağlantı sayısını destekleyediğinden emin olun `ConnectionPolicy` .
Aksi halde bağlantı sorunlarıyla karşılaşın.

### <a name="request-rate-too-large"></a><a name="request-rate-too-large"></a>İstek hızı çok büyük
' İstek hızı çok büyük ' veya 429 hata kodu hatası, tüketilen üretilen iş (RU/s) [sağlanan aktarım hızını](set-throughput.md)aştığından isteklerin kısıtlandığını gösterir. SDK, belirtilen [yeniden deneme ilkesine](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.connectionpolicy.retryoptions?view=azure-dotnet)göre istekleri otomatik olarak yeniden dener. Bu hatayla sık sık karşılaşırsanız, koleksiyondaki aktarım hızını artırmayı düşünün. 429 hatası olup olmadığınızı görmek için [portalın ölçümlerini](use-metrics.md) denetleyin. Depolama ve istek hacminin eşit bir şekilde dağıtımına neden olduğundan emin olmak için [bölüm anahtarınızı](partitioning-overview.md#choose-partitionkey) gözden geçirin. 

### <a name="slow-query-performance"></a>Yavaş sorgu performansı
Sorgu [ölçümleri](sql-api-query-metrics.md) , sorgunun en fazla zaman harcamanın nerede olduğunu belirlemenize yardımcı olur. Sorgu ölçümlerinde, istemcinin arka ucunda ne kadarının harcandığını görebilirsiniz.
* Arka uç sorgusu hızlı bir şekilde döndürürse ve istemci üzerinde büyük bir zaman harcadıysanız makinedeki yükü kontrol edin. Yeterli miktarda kaynak olmaması ve SDK 'nın yanıtı işlemek için kaynakların kullanılabilir olmasını bekliyor olması olasıdır.
* Arka uç sorgusu yavaşsa, [sorguyu iyileştirmeyi](optimize-cost-queries.md) ve geçerli [Dizin oluşturma ilkesine](index-overview.md) bakmaya çalışın 

### <a name="http-401-the-mac-signature-found-in-the-http-request-is-not-the-same-as-the-computed-signature"></a>HTTP 401: HTTP isteğinde bulunan MAC imzası, hesaplanan imzayla aynı değil
Aşağıdaki 401 hata iletisini aldıysanız: "HTTP isteğinde bulunan MAC imzası, hesaplanan imzayla aynı değil." Bu, aşağıdaki senaryolardan kaynaklanıyor olabilir.

1. Anahtar döndürüldü ve [en iyi uygulamaları](secure-access-to-data.md#key-rotation)izmedi. Bu genellikle durumdur. Cosmos DB hesap anahtarı döndürme, Cosmos DB hesap boyutuna bağlı olarak birkaç saniye ile muhtemelen gün arasında bir süre sürebilir.
   1. 401 MAC imzası, anahtar dönüşünün kısa bir süre sonra görülür ve sonuç olarak herhangi bir değişiklik yapılmadan durduruluyor. 
1. Anahtar, uygulamada, anahtarın hesap ile eşleşmemesi için yanlış yapılandırılmış.
   1. 401 MAC imzası sorunu tutarlı olacak ve tüm çağrılar için gerçekleşir
1. Uygulama, yazma işlemleri için [salt okunurdur anahtarlarını](secure-access-to-data.md#master-keys) kullanıyor.
   1. 401 MAC imzası sorunu yalnızca uygulama yazma istekleri gerçekleştirirken olur, ancak okuma istekleri başarılı olur.
1. Kapsayıcı oluşturma ile bir yarış durumu var. Bir uygulama örneği, kapsayıcı oluşturma işlemi tamamlanmadan önce kapsayıcıya erişmeye çalışıyor. Uygulama çalışıyorsa bu için en yaygın senaryo ve uygulama çalışırken kapsayıcı silinip aynı adla yeniden oluşturulur. SDK yeni kapsayıcıyı kullanmayı deneyecek, ancak kapsayıcı oluşturma hala devam ediyor, bu yüzden anahtarlara sahip değil.
   1. 401 MAC imzası sorunu bir kapsayıcı oluşturulduktan sonra kısa bir süre sonra görülür ve yalnızca kapsayıcı oluşturma tamamlanana kadar oluşur.
 
 ### <a name="http-error-400-the-size-of-the-request-headers-is-too-long"></a>HTTP hatası 400. İstek üst bilgilerinin boyutu çok uzun.
 Üstbilginin boyutu büyük ve izin verilen en büyük boyutu aşıyor. Her zaman en son SDK 'Yı kullanmanız önerilir. Özel durum iletisine üst bilgi boyutu izlemeyi ekleyen en az [3. x](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/changelog.md) veya [2. x](https://github.com/Azure/azure-cosmos-dotnet-v2/blob/master/changelog.md)sürümünü kullandığınızdan emin olun.

Mesine
 1. Oturum belirteci çok büyük. Kapsayıcıda bulunan bölüm sayısı arttıkça oturum belirteci de artar.
 2. Devamlılık belirteci büyüdü. Farklı sorguların farklı devamlılık belirteci boyutları olacaktır.
 3. Bunun nedeni, oturum belirtecinin ve devamlılık belirtecinin bir birleşimidir.

Çözüm:
   1. [Performans ipuçlarını](performance-tips.md) izleyin ve uygulamayı doğrudan + TCP bağlantı moduna dönüştürün. Doğrudan + TCP, bu sorunu önleyen HTTP gibi üst bilgi boyutu kısıtlamasına sahip değildir.
   2. Oturum belirteci neden olursa, geçici bir risk azaltma uygulamayı yeniden başlatmayı sağlar. Uygulama örneğinin yeniden başlatılması oturum belirtecini sıfırlayacaktır. Yeniden başlatmanın ardından özel durumlar durduktan sonra, oturum belirtecinin neden olduğunu onaylar. Sonuç olarak, özel duruma neden olacak boyuta geri dönüş yapılır.
   3. Uygulama doğrudan + TCP 'ye dönüştürülemiyorsa ve oturum belirteci neden ise, istemci [tutarlılığı düzeyi](consistency-levels.md)değiştirilerek risk azaltma yapılabilir. Oturum belirteci yalnızca Cosmos DB için varsayılan olan oturum tutarlılığı için kullanılır. Diğer tutarlılık düzeyi, oturum belirtecini kullanmaz. 
   4. Uygulama doğrudan + TCP 'ye dönüştürülemiyorsa ve devamlılık belirteci neden olursa, Responsecontinuationtokenlimitınkb seçeneğini ayarlamayı deneyin. Bu seçenek, v2 için Feedoseçenekleri veya v3 içindeki QueryRequestOptions içinde bulunabilir.

 <!--Anchors-->
[Common issues and workarounds]: #common-issues-workarounds
[Enable client SDK logging]: #logging
[İstek hızı çok büyük]: #request-rate-too-large
[Request Timeouts]: #request-timeouts
[Azure SNAT (PAT) port exhaustion]: #snat
[Production check list]: #production-check-list
