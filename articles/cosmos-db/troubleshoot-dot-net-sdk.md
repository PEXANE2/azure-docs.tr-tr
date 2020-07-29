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
ms.openlocfilehash: 1dd6bdc66146eb7dfe155e7d1091eee5cca450a0
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87290917"
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
[Portal ölçümlerinin](monitor-accounts.md) denetlenmesi, istemci tarafı bir sorun olup olmadığını veya hizmette bir sorun olup olmadığını belirlemenize yardımcı olur. Örneğin, ölçümler yüksek bir hız sınırlı istek (HTTP durum kodu 429) içeriyorsa, isteğin azaltıldı, [istek hızı çok büyük](troubleshoot-request-rate-too-large.md) bölümünü kontrol edin. 

## <a name="common-error-status-codes"></a>Ortak hata durum kodları<a id="error-codes"></a>

| Durum Kodu | Açıklama | 
|----------|-------------|
| 400 | Hatalı istek (hata iletisine bağlıdır)| 
| 401 | [Yetkilendirilmemiş](troubleshoot-unauthorized.md) | 
| 404 | [Kaynak bulunamadı](troubleshoot-not-found.md) |
| 408 | [İstek zaman aşımına uğradı](troubleshoot-dot-net-sdk-request-timeout.md) |
| 409 | Çakışma hatası, yazma işlemindeki bir kaynak için belirtilen KIMLIğIN mevcut bir kaynak tarafından alındığı zaman. Bu sorunu çözmek için kaynak için başka bir KIMLIK kullanın, çünkü KIMLIK aynı bölüm anahtarı değerine sahip tüm belgeler içinde benzersiz olmalıdır. |
| 410 | Özel durumlar (SLA 'yı ihlal etmemesi gereken geçici hata) |
| 412 | Önkoşul hatası, işlemin sunucuda bulunan sürümden farklı bir eTag 'i belirttiği yerdir. Bu, iyimser eşzamanlılık hatasıdır. Kaynağın en son sürümünü okuyup istekteki eTag’i güncelleştirdikten sonra isteği yeniden deneyin.
| 413 | [İstek varlığı çok büyük](concepts-limits.md#per-item-limits) |
| 429 | [Çok fazla istek](troubleshoot-request-rate-too-large.md) |
| 449 | Yalnızca yazma işlemlerinde oluşan ve yeniden denenme güvenli olan geçici hata |
| 500 | İşlem beklenmeyen bir hizmet hatası nedeniyle başarısız oldu. Desteğe başvurun. Bkz. [Azure destek sorununu](https://aka.ms/azure-support)oluşturma. |
| 503 | [Hizmet kullanılamıyor](troubleshoot-service-unavailable.md) | 

### <a name="azure-snat-pat-port-exhaustion"></a><a name="snat"></a>Azure SNAT (PAT) bağlantı noktası tükenmesi

Uygulamanız [Azure sanal makinelerinde genel IP adresi olmadan](../load-balancer/load-balancer-outbound-connections.md)dağıtılmışsa, varsayılan olarak [Azure SNAT bağlantı noktaları](../load-balancer/load-balancer-outbound-connections.md#preallocatedports) , sanal makinenizin dışındaki herhangi bir uç noktaya bağlantı kurar. VM 'den Azure Cosmos DB uç noktasına izin verilen bağlantı sayısı [Azure SNAT yapılandırması](../load-balancer/load-balancer-outbound-connections.md#preallocatedports)ile sınırlıdır. Bu durum bağlantının azaltılmasına, bağlantı kapanışına veya yukarıda belirtilen [istek zaman aşımlarına](troubleshoot-dot-net-sdk-request-timeout.md)yol açabilir.

 Azure SNAT bağlantı noktaları, yalnızca sanal makinenizin özel bir IP adresi genel bir IP adresine bağlandığında kullanılır. Azure SNAT sınırlamasından kaçınmak için iki geçici çözüm vardır (önceden uygulamanın tamamında tek bir istemci örneği kullanıyor olmanız gerekir):

* Azure Cosmos DB hizmeti uç noktanızı Azure sanal makineler sanal ağınızın alt ağına ekleyin. Daha fazla bilgi için bkz. [Azure sanal ağ hizmeti uç noktaları](../virtual-network/virtual-network-service-endpoints-overview.md). 

    Hizmet uç noktası etkinleştirildiğinde, istekler artık genel bir IP Azure Cosmos DB ' e gönderilmez. Bunun yerine, sanal ağ ve alt ağ kimliği gönderilir. Yalnızca genel IP 'Lere izin veriliyorsa bu değişiklik güvenlik duvarı düşceye neden olabilir. Bir güvenlik duvarı kullanıyorsanız, hizmet uç noktasını etkinleştirdiğinizde, [sanal ağ ACL 'lerini](../virtual-network/virtual-networks-acl.md)kullanarak güvenlik duvarına bir alt ağ ekleyin.
* [Azure sanal makinenize genel IP](../load-balancer/troubleshoot-outbound-connection.md#assignilpip)atayın.

### <a name="high-network-latency"></a><a name="high-network-latency"></a>Yüksek ağ gecikmesi
Yüksek ağ gecikmesi, v2 SDK 'sindeki [Tanılama dizesi](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.resourceresponsebase.requestdiagnosticsstring?view=azure-dotnet) veya v3 SDK 'daki [Tanılamalar](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmos.responsemessage.diagnostics?view=azure-dotnet#Microsoft_Azure_Cosmos_ResponseMessage_Diagnostics) kullanılarak belirlenebilir.

Bir [zaman aşımı](troubleshoot-dot-net-sdk-request-timeout.md) yoksa ve tanılama, yüksek gecikme süresinin ve arasındaki farka göre `ResponseTime` `RequestStartTime` (Bu örnekte >300 milisaniyelik) açık olduğu tek istekleri gösterir.

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

### <a name="slow-query-performance"></a>Yavaş sorgu performansı
Sorgu [ölçümleri](sql-api-query-metrics.md) , sorgunun en fazla zaman harcamanın nerede olduğunu belirlemenize yardımcı olur. Sorgu ölçümlerinde, istemcinin arka ucunda ne kadarının harcandığını görebilirsiniz.
* Arka uç sorgusu hızlı bir şekilde döndürürse ve istemci üzerinde büyük bir zaman harcadıysanız makinedeki yükü kontrol edin. Yeterli miktarda kaynak olmaması ve SDK 'nın yanıtı işlemek için kaynakların kullanılabilir olmasını bekliyor olması olasıdır.
* Arka uç sorgusu yavaşsa, [sorguyu iyileştirmeyi](optimize-cost-queries.md) ve geçerli [Dizin oluşturma ilkesine](index-overview.md) bakmaya çalışın 

## <a name="next-steps"></a>Sonraki adımlar

* [.Net v3](performance-tips-dotnet-sdk-v3-sql.md) ve [.net v2](performance-tips.md) için performans yönergeleri hakkında bilgi edinin
* [Reaktör tabanlı Java SDK 'ları](https://github.com/Azure-Samples/azure-cosmos-java-sql-api-samples/blob/master/reactor-pattern-guide.md) hakkında bilgi edinin

 <!--Anchors-->
[Common issues and workarounds]: #common-issues-workarounds
[Enable client SDK logging]: #logging
[Azure SNAT (PAT) port exhaustion]: #snat
[Production check list]: #production-check-list
