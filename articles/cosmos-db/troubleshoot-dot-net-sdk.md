---
title: Azure Cosmos DB .NET SDK'sını kullanırken karşılaşılan sorunları tanılama ve giderme
description: .NET SDK kullanırken Azure Cosmos DB sorunlarını tanımlamak, tanılamak ve sorun gidermek için istemci tarafı günlüğü ve diğer üçüncü taraf araçları gibi özellikleri kullanın.
author: j82w
ms.service: cosmos-db
ms.date: 03/11/2020
ms.author: jawilley
ms.subservice: cosmosdb-sql
ms.topic: troubleshooting
ms.reviewer: sngun
ms.openlocfilehash: 5f92d98630c6fb875babeb907f92732b0c24bb52
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79137963"
---
# <a name="diagnose-and-troubleshoot-issues-when-using-azure-cosmos-db-net-sdk"></a>Azure Cosmos DB .NET SDK'sını kullanırken karşılaşılan sorunları tanılama ve giderme
Bu makalede, Azure Cosmos DB SQL API hesaplarıyla [.NET SDK'yı](sql-api-sdk-dotnet.md) kullandığınızda sık karşılaşılan sorunlar, geçici çözüm adımları ve araçlar ele alılmıştır.
.NET SDK, Azure Cosmos DB SQL API'sine erişmek için istemci tarafı mantıksal gösterimi sağlar. Bu makalede, sorunla karşılaştığınızda size yardımcı olacak araçlar ve yaklaşımlar açıklanır.

## <a name="checklist-for-troubleshooting-issues"></a>Sorun giderme sorunları için denetim listesi:
Uygulamanızı üretime taşımadan önce aşağıdaki denetim listesini göz önünde bulundurun. Denetim listesini kullanmak, görebileceğiniz birkaç yaygın sorunu önler. Bir sorun oluştuğunda da hızlı bir şekilde tanılayabilirsiniz:

*    En son [SDK'yı](sql-api-sdk-dotnet-standard.md)kullanın. Önizleme SDK'ları üretim için kullanılmamalıdır. Bu, zaten düzeltilen bilinen sorunları vurmayı önler.
*    Performans [ipuçlarını](performance-tips.md)gözden geçirin ve önerilen uygulamaları izleyin. Bu, ölçekleme, gecikme ve diğer performans sorunlarını önlemeye yardımcı olur.
*    Sorunları gidermenize yardımcı olması için SDK günlüğe kaydetmeyi etkinleştirin. Günlüğe kaydetmeyi etkinleştirmek performansı etkileyebilir, bu nedenle yalnızca sorun giderme sorunları olduğunda etkinleştirmek en iyisidir. Aşağıdaki günlükleri etkinleştirebilirsiniz:
    *    Azure portalını kullanarak [ölçümleri günlüğe](monitor-accounts.md) kaydedin. Portal ölçümleri, sorunun Azure Cosmos DB'ye karşılık olup olmadığını veya istemci tarafından mı olduğunu belirlemek için yararlı olan Azure Cosmos DB telemetrisini gösterir.
    *    Nokta operasyon yanıtlarından V2 SDK'daki [tanılama dizesini](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.resourceresponsebase.requestdiagnosticsstring) veya V3 SDK'daki [tanılamayı](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmos.responsemessage.diagnostics) kaydedin.
    *    Tüm sorgu yanıtlarından [SQL Sorgu Ölçümlerini](sql-api-query-metrics.md) günlüğe kaydetme 
    *    [SDK günlüğe kaydetme]( https://github.com/Azure/azure-cosmos-dotnet-v2/blob/master/docs/documentdb-sdk_capture_etl.md) kurulumlarını takip edin

Bu makaledeki [Ortak sorunlar ve geçici çözüm ler](#common-issues-workarounds) bölümüne bir göz atın.

Etkin olarak izlenen [GitHub sorunları bölümünü](https://github.com/Azure/azure-cosmos-dotnet-v2/issues) kontrol edin. Geçici çözümle ilgili benzer bir sorunun dosyalanmış olup olmadığını denetleyin. Bir çözüm bulamadıysanız, github sorunu dosyalayın. Acil sorunlar için bir destek işareti açabilirsiniz.


## <a name="common-issues-and-workarounds"></a><a name="common-issues-workarounds"></a>Yaygın sorunlar ve geçici çözümler

### <a name="general-suggestions"></a>Genel öneriler
* Uygulamanızı mümkün olduğunca Azure Cosmos DB hesabınızla aynı Azure bölgesinde çalıştırın. 
* İstemci makinenizde kaynak yetersizliğinden dolayı bağlantı/kullanılabilirlik sorunlarıyla karşılaşabilirsiniz. CPU kullanımınızı Azure Cosmos DB istemcisini çalıştıran düğümlerde izlemenizi ve yüksek yükte çalışıyorlarsa ölçeklendirmenizi/çıkarmanızı öneririz.

### <a name="check-the-portal-metrics"></a>Portal ölçümlerini kontrol edin
Portal [ölçümlerinin](monitor-accounts.md) denetlenen, istemci tarafı bir sorun olup olmadığını veya hizmetle ilgili bir sorun olup olmadığını belirlemede yardımcı olur. Örneğin, ölçümler yüksek oranda oran sınırlı istek içeriyorsa (HTTP durum kodu 429) bu da isteğin daraldığı anlamına geliyorsa, [Istek oranını çok büyük] bir bölümdenetleyin. 

### <a name="requests-timeouts"></a><a name="request-timeouts"></a>Zaman zaman larını istekleri
RequestTimeout genellikle Doğrudan/TCP kullanırken olur, ancak Ağ Geçidi modunda gerçekleşebilir. Bu hatalar, bilinen yaygın nedenler ve sorunun nasıl düzeltilenle ilgili önerilerdir.

* CPU kullanımı yüksektir, bu da gecikmeye ve/veya istek zaman adabına neden olur. Müşteri daha fazla kaynak sağlamak için ana makineyi ölçeklendirebilir veya yük daha fazla makineye dağıtılabilir.
* Soket / Bağlantı noktası kullanılabilirliği düşük olabilir. Azure'da çalışırken,.NET SDK'yı kullanan istemciler Azure SNAT (PAT) bağlantı noktası tükenmesini engelleyebilir. Bu soruna çarpma olasılığını azaltmak için .NET SDK'nın 2.x veya 3.x en son sürümünü kullanın. Bu, her zaman en son SDK sürümünü çalıştırmak için tavsiye edilir neden bir örnektir.
* Birden çok DocumentClient örneği oluşturmak bağlantı çekişmesine ve zaman adabına neden olabilir. Performans [ipuçlarını](performance-tips.md)izleyin ve tüm işlem boyunca tek bir DocumentClient örneğini kullanın.
* Kullanıcılar bazen yüksek gecikme süresi veya istek zaman zaman ları görür, çünkü koleksiyonları yetersiz olarak karşılanır, arka uç isteklerini daraltır ve istemci dahili olarak yeniden çalışır. Portal [ölçümlerini](monitor-accounts.md)kontrol edin.
* Azure Cosmos DB, sağlanan genel iş ortasını fiziksel bölümler arasında eşit olarak dağıtır. İş yükünün sıcak bir [bölüm anahtarıyla](partition-data.md)karşılaşıyor mu diye portal ölçümlerini denetleyin. Bu, tüketilen toplam iş buzun (RU/s) sağlanan RUs'lar altında görünmesine neden olur, ancak tüketilen tek bir bölüm iş girdisi (RU/s) sağlanan iş buzunu aşacaktır. 
* Ayrıca, 2.0 SDK doğrudan/TCP bağlantılarına kanal semantikleri ekler. Aynı anda birden çok istek için bir TCP bağlantısı kullanılır. Bu, belirli durumlarda iki soruna yol açabilir:
    * Yüksek miktarda eşzamanlılık kanalda çekişmeye yol açabilir.
    * Büyük istekler veya yanıtlar, kanalda satır başı engellemesine yol açabilir ve nispeten düşük bir eşzamanlılık derecesine rağmen çekişmeyi şiddetlendirebilir.
    * Bu iki kategoriden herhangi birinde durum düşüyorsa (veya yüksek CPU kullanımından şüpheleniliyorsa), bunlar olası azaltıcı etkenlerdir:
        * Uygulamayı yukarı/dışarı ölçeklendirmeye çalışın.
        * Ayrıca, SDK günlükleri daha fazla bilgi almak için [Trace Listener](https://github.com/Azure/azure-cosmosdb-dotnet/blob/master/docs/documentdb-sdk_capture_etl.md) aracılığıyla yakalanabilir.

### <a name="high-network-latency"></a><a name="high-network-latency"></a>Yüksek ağ gecikmesi
Yüksek ağ gecikmesi V2 SDK'daki [tanılama dizesi](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.resourceresponsebase.requestdiagnosticsstring?view=azure-dotnet) veya V3 SDK'daki [tanılama](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmos.responsemessage.diagnostics?view=azure-dotnet#Microsoft_Azure_Cosmos_ResponseMessage_Diagnostics) kullanılarak tanımlanabilir.

Zaman [sonu](#request-timeouts) yoksa ve tanılama, yüksek gecikme süresinin, aşağıdaki gibi (bu `ResponseTime` `RequestStartTime`örnekte 300 milisaniye >) arasındaki farkta belirgin olduğu tek istekleri gösterir:

```bash
RequestStartTime: 2020-03-09T22:44:49.5373624Z, RequestEndTime: 2020-03-09T22:44:49.9279906Z,  Number of regions attempted:1
ResponseTime: 2020-03-09T22:44:49.9279906Z, StoreResult: StorePhysicalAddress: rntbd://..., ...
```

Bu gecikmenin birden çok nedeni olabilir:

* Uygulamanız Azure Cosmos DB hesabınızla aynı bölgede çalışmıyor.
* [PreferredLocations](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.connectionpolicy.preferredlocations) veya [ApplicationRegion](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmos.cosmosclientoptions.applicationregion) yapılandırmanız yanlıştır ve uygulamanızın şu anda çalışmakta olduğu farklı bir bölgeye bağlanmaya çalışır.
* Yüksek trafik nedeniyle Ağ arabiriminde bir darboğaz olabilir. Uygulama Azure Sanal Makineler'de çalışıyorsa, olası geçici geçici çözüm vardır:
    * [Hızlandırılmış Ağ etkin](../virtual-network/create-vm-accelerated-networking-powershell.md)leştirilmiş bir Sanal Makine kullanmayı düşünün.
    * [Varolan bir Sanal Makinede Hızlandırılmış Ağ'ı](../virtual-network/create-vm-accelerated-networking-powershell.md#enable-accelerated-networking-on-existing-vms)etkinleştirin.
    * Daha yüksek bir [uç Sanal Makine](../virtual-machines/windows/sizes.md)kullanmayı düşünün.

### <a name="azure-snat-pat-port-exhaustion"></a><a name="snat"></a>Azure SNAT (PAT) bağlantı noktası tükenmesi

Uygulamanız herkese açık [bir IP adresi olmayan Azure Sanal Makinelerde](../load-balancer/load-balancer-outbound-connections.md#defaultsnat)dağıtılıyorsa, varsayılan olarak [Azure SNAT bağlantı noktaları](../load-balancer/load-balancer-outbound-connections.md#preallocatedports) VM'nizin dışındaki herhangi bir uç noktaya bağlantılar kurar. VM'den Azure Cosmos DB bitiş noktasına izin verilen bağlantı sayısı [Azure SNAT yapılandırması](../load-balancer/load-balancer-outbound-connections.md#preallocatedports)ile sınırlıdır. Bu durum bağlantı azaltma, bağlantı kapatma veya yukarıda belirtilen [İstek zaman zaman zaman ları](#request-timeouts)yol açabilir.

 Azure SNAT bağlantı noktaları yalnızca VM'nizin özel bir IP adresi ortak bir IP adresine bağlandığında kullanılır. Azure SNAT sınırlamasını önlemek için iki geçici çözüm vardır (uygulamanın tamamında zaten tek bir istemci örneği kullanıyorsanız):

* Azure Cosmos DB hizmet bitiş noktanızı Azure Sanal Makineler sanal ağınızın alt ağına ekleyin. Daha fazla bilgi için Azure [Sanal Ağ hizmeti bitiş noktalarına](../virtual-network/virtual-network-service-endpoints-overview.md)bakın. 

    Hizmet bitiş noktası etkinleştirildiğinde, istekler artık ortak bir IP'den Azure Cosmos DB'ye gönderilmez. Bunun yerine, sanal ağ ve alt ağ kimliği gönderilir. Bu değişiklik, yalnızca genel IP'lere izin verilirse güvenlik duvarının düşmesine neden olabilir. Bir güvenlik duvarı kullanıyorsanız, hizmet bitiş noktasını etkinleştirdiğinizde, [Sanal Ağ ALA'larını](../virtual-network/virtual-networks-acl.md)kullanarak güvenlik duvarına bir alt ağ ekleyin.
* [Azure VM'nize ortak](../load-balancer/load-balancer-outbound-connections.md#assignilpip)bir IP atayın.

### <a name="http-proxy"></a>HTTP proxy
BIR HTTP proxy kullanıyorsanız, SDK'da `ConnectionPolicy`yapılandırılan bağlantı sayısını desteklediğinden emin olun.
Aksi takdirde, bağlantı sorunlarıyla karşı karşıya kaldığınız da olur.

### <a name="request-rate-too-large"></a><a name="request-rate-too-large"></a>İstek oranı çok büyük
'İstek oranı çok büyük' veya hata kodu 429, tüketilen iş batışı (RU/s) [sağlanan iş bürünmüş](set-throughput.md)olduğundan isteklerinizin azaltıldığını gösterir. SDK, belirtilen [yeniden deneme ilkesine](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.connectionpolicy.retryoptions?view=azure-dotnet)göre istekleri otomatik olarak yeniden deneecektir. Bu hatayı sık sık alırsanız, koleksiyondaki iş bilgili liği artırmayı düşünün. 429 hata alıyorsanız görmek için [portalın ölçümlerini](use-metrics.md) kontrol edin. Depolama ve talep hacminin eşit bir şekilde dağıtılmasıyla sonuçlanmasını sağlamak için [bölüm anahtarınızı](partitioning-overview.md#choose-partitionkey) gözden geçirin. 

### <a name="slow-query-performance"></a>Yavaş sorgu performansı
[Sorgu ölçümleri,](sql-api-query-metrics.md) sorgunun çoğu zaman nerede harcadığını belirlemede yardımcı olur. Sorgu ölçümlerinden, ne kadarının istemciye karşı arka uçta harcandığını görebilirsiniz.
* Arka uç sorgusu hızlı bir şekilde dönerse ve istemci üzerinde büyük bir zaman harcarsa makinedeki yükü kontrol edin. Yeterli kaynak olmaması ve SDK'nın yanıtı işlemek için kaynakların kullanılabilir olmasını beklemesi olasıdır.
* Arka uç sorgusu [yavaşsa, sorguyu en iyi duruma getirmeye](optimize-cost-queries.md) ve geçerli [dizin oluşturma ilkesine](index-overview.md) bakmayı deneyin 

 <!--Anchors-->
[Common issues and workarounds]: #common-issues-workarounds
[Enable client SDK logging]: #logging
[İstek oranı çok büyük]: #request-rate-too-large
[Request Timeouts]: #request-timeouts
[Azure SNAT (PAT) port exhaustion]: #snat
[Production check list]: #production-check-list


