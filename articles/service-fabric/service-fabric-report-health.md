---
title: Özel Hizmet Kumaşı sağlık raporları ekleme
description: Azure Hizmet Dokusu sistem durumu kuruluşlarına özel sistem durumu raporlarının nasıl gönderilebildiğini açıklar. Kaliteli sağlık raporlarının tasarlanması ve uygulanması için öneriler de verir.
author: oanapl
ms.topic: conceptual
ms.date: 2/28/2018
ms.author: oanapl
ms.openlocfilehash: d00f740085b15bdb5fe698a069d97f168507f31f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75451598"
---
# <a name="add-custom-service-fabric-health-reports"></a>Özel Hizmet Kumaşı sağlık raporları ekleme
Azure Hizmet Kumaşı, belirli varlıklardaki sağlıksız küme ve uygulama koşullarını işaretlemek üzere tasarlanmış bir [sistem durumu modeli](service-fabric-health-introduction.md) sunar. Sağlık modeli **sağlık muhabirleri** (sistem bileşenleri ve watchdogs) kullanır. Amaç kolay ve hızlı tanı ve onarım. Hizmet yazarları sağlık hakkında önceden düşünmek gerekir. Özellikle köke yakın bayrak sorunlarına yardımcı olabilir, sağlık etkileyebilir herhangi bir durum rapor edilmelidir. Sağlık bilgileri hata ayıklama ve soruşturma zaman ve çaba kaydedebilirsiniz. Hizmet bulutta (özel veya Azure) ölçekte çalışır durumda olduğunda kullanışlılık özellikle açıktır.

Service Fabric muhabirleri belirlenen ilgi koşullarını izlerler. Yerel görüşlerine göre bu koşulları rapor ediyorlar. [Sistem durumu deposu,](service-fabric-health-introduction.md#health-store) varlıkların genel olarak sağlıklı olup olmadığını belirlemek için tüm muhabirler tarafından gönderilen sağlık verilerini toplar. Modelzengin, esnek ve kullanımı kolay olması amaçlanmıştır. Sistem durumu raporlarının kalitesi kümenin sistem durumu görünümünün doğruluğunu belirler. Sağlıksız sorunları yanlış gösteren yanlış pozitif sonuçlar, yükseltmeleri veya sistem durumu verilerini kullanan diğer hizmetleri olumsuz etkileyebilir. Bu tür hizmetlere örnek olarak onarım hizmetleri ve uyarı mekanizmaları verilebilir. Bu nedenle, bazı düşünce mümkün olan en iyi şekilde ilgi koşulları yakalamak raporlar sağlamak için gereklidir.

Sağlık raporlamasını tasarlamak ve uygulamak için izleme köpekleri ve sistem bileşenleri şunları yapmalı:

* İlgilendikleri durumu, izlenme şeklini ve küme veya uygulama işlevselliği üzerindeki etkisini tanımlayın. Bu bilgilere dayanarak, sağlık raporu özelliği ve sağlık durumu karar verin.
* Raporun geçerli olduğu [varlığı](service-fabric-health-introduction.md#health-entities-and-hierarchy) belirleyin.
* Raporlamanın nerede, hizmet içinden veya dahili veya harici bir izleme örgütünden yapıldığını belirleyin.
* Muhabiri tanımlamak için kullanılan bir kaynak tanımlayın.
* Periyodik olarak veya geçişlerde bir raporlama stratejisi seçin. Önerilen yol, daha basit kod gerektirdiğinden ve hatalara daha az eğilimli olduğundan, düzenli aralıklarla yapılır.
* Sağlıksız koşullara yönelik raporun sağlık deposunda ne kadar kalması ve nasıl temizlenmesi gerektiğini belirleyin. Bu bilgileri kullanarak, raporun yaşam süresine ve son kullanma tarihini kaldırma davranışına karar verin.

Belirtildiği gibi, raporlama yapılabilir:

* İzlenen Service Fabric servis çoğaltma.
* Service Fabric hizmeti olarak dağıtılan dahili izleme örgütleri (örneğin, koşulları izleyen ve rapor veren bir Service Fabric stateless hizmeti). Watchdogs bir tüm düğümleri dağıtılabilir veya izlenen hizmetaffinitized olabilir.
* Service Fabric düğümleri üzerinde çalışan ancak Service Fabric hizmetleri olarak *uygulanmayan* dahili watchdogs.
* Hizmeti Kumaş kümesi *dışından* kaynak probe dış watchdogs (örneğin, Gomez gibi izleme hizmeti).

> [!NOTE]
> Kutunun dışında, küme sistem bileşenleri tarafından gönderilen sistem raporlarıyla doldurulur. [Sorun giderme için sistem sistem sistem raporlarını kullanma](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)hakkında daha fazla bilgi edinin. Kullanıcı raporları, sistem tarafından zaten oluşturulmuş [olan sistem varlıklarına](service-fabric-health-introduction.md#health-entities-and-hierarchy) gönderilmelidir.
> 
> 

Sağlık raporlama tasarımı netleştikten sonra, sağlık raporları kolayca gönderilebilir. Küme [güvenli](service-fabric-cluster-security.md) değilse veya kumaş istemcisinin yönetici ayrıcalıkları varsa, durumu bildirmek için [FabricClient'ı](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient) kullanabilirsiniz. Raporlama, [FabricClient.HealthManager.ReportHealth](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.reporthealth)kullanılarak, PowerShell üzerinden veya REST aracılığıyla API üzerinden yapılabilir. Yapılandırma, gelişmiş performans için toplu iş raporları nın düğümlerini sağlar.

> [!NOTE]
> Rapor durumu eşzamanlıdır ve yalnızca istemci tarafındaki doğrulama çalışmasını temsil eder. Raporun sağlık istemcisi veya `Partition` `CodePackageActivationContext` nesneleri tarafından kabul edilmiş olması, raporun mağazada uygulandığı anlamına gelmez. Eşzamanlı olarak gönderilir ve muhtemelen diğer raporlarla toplu olarak paketlenir. Sunucudaki işlemler hala başarısız olabilir: sıra numarası eski olabilir, raporun uygulanması gereken varlık silinmiş olabilir, vb.
> 
> 

## <a name="health-client"></a>Sağlık istemcisi
Sağlık raporları, kumaş istemcisi içinde yaşayan bir sağlık müşterisi aracılığıyla sağlık müdürüne gönderilir. Sistem durumu yöneticisi raporları sağlık deposunda kaydeder. Sistem durumu istemcisi aşağıdaki ayarlarla yapılandırılabilir:

* **HealthReportSendInterval**: Raporun istemciye eklenme tarihi ile sağlık yöneticisine gönderildiği saat arasındaki gecikme. Her rapor için bir ileti göndermek yerine raporları tek bir iletiye toplu olarak ayırmak için kullanılır. Toplu iş performansı artırır. Varsayılan: 30 saniye.
* **HealthReportRetrySendInterval**: Sağlık istemcisinin birikmiş sağlık raporlarını sağlık yöneticisine yeniden gönderme aralığı. Varsayılan: 30 saniye, en az: 1 saniye.
* **HealthOperationTimeout**: Sağlık yöneticisine gönderilen rapor iletisinin zaman ödeme süresi. İleti nin zaman ları dışarı çıkarsa, sağlık yöneticisi raporun işlendiğini onaylayana kadar sistem durumu istemcisi bu iletiyi yeniden dener. Varsayılan: iki dakika.

> [!NOTE]
> Raporlar toplu olarak paketlendiğinde, gönderilenlerden emin olmak için en azından HealthReportSendInterval için kumaş istemcisinin canlı tutulması gerekir. İleti kaybolursa veya sağlık yöneticisi geçici hatalar nedeniyle bunları uygulayamıyorsa, kumaş istemcisinin yeniden deneme şansı vermesi için daha uzun süre hayatta kalması gerekir.
> 
> 

İstemci üzerinde arabelleğe alma, raporların benzersizliğini dikkate alır. Örneğin, belirli bir kötü muhabir aynı varlığın aynı özelliği hakkında saniyede 100 rapor bildiriyorsa, raporlar son sürümle değiştirilir. İstemci kuyruğunda en fazla bir rapor bulunur. Toplu işlem yapılandırılırsa, sistem durumu yöneticisine gönderilen rapor sayısı gönderme aralığı başına yalnızca bir olur. Bu rapor, varlığın en güncel durumunu yansıtan son eklenen rapordur.
Sistem durumuyla `FabricClient` ilgili girişler için istenen değerlerle [FabricClientSettings](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclientsettings) geçirilerek oluşturulduğunda yapılandırma parametrelerini belirtin.

Aşağıdaki örnek bir kumaş istemcisi oluşturur ve raporların eklendiğinde gönderilmesi gerektiğini belirtir. Yeniden denenebilen zaman adablarında ve hatalarda, yeniden denemeler her 40 saniyede bir gerçekleşir.

```csharp
var clientSettings = new FabricClientSettings()
{
    HealthOperationTimeout = TimeSpan.FromSeconds(120),
    HealthReportSendInterval = TimeSpan.FromSeconds(0),
    HealthReportRetrySendInterval = TimeSpan.FromSeconds(40),
};
var fabricClient = new FabricClient(clientSettings);
```

30 saniye olarak ayarlanmış `HealthReportSendInterval` varsayılan kumaş istemci ayarlarını tutmanızı öneririz. Bu ayar, toplu iş nedeniyle en iyi performansı sağlar. En kısa sürede gönderilmesi gereken kritik raporlar `HealthReportSendOptions` için, Hemen `true` [FabricClient.HealthClient.ReportHealth](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.reporthealth) API ile kullanın. Hemen raporlar toplu iş aralığını atlar. Bu bayrağı dikkatli kullanın; mümkün olduğunca sağlık istemcisi toplu işyararlanmak istiyorum. Kumaş istemcisi kapanırken anında gönderme de yararlıdır (örneğin, işlem geçersiz durumu belirlemiştir ve yan etkileri önlemek için kapatılması gerekir). Birikmiş raporların en iyi şekilde gönderilmesini sağlar. Bir rapor Hemen bayrakla eklendiğinde, sağlık istemcisi son gönderimden bu yana birikmiş tüm raporları toplu olarak alır.

PowerShell üzerinden bir kümeye bağlantı oluşturulduğunda aynı parametreler belirtilebilir. Aşağıdaki örnek, yerel bir kümeye bağlantı başlatır:

```powershell
PS C:\> Connect-ServiceFabricCluster -HealthOperationTimeoutInSec 120 -HealthReportSendIntervalInSec 0 -HealthReportRetrySendIntervalInSec 40
True

ConnectionEndpoint   :
FabricClientSettings : {
                       ClientFriendlyName                   : PowerShell-1944858a-4c6d-465f-89c7-9021c12ac0bb
                       PartitionLocationCacheLimit          : 100000
                       PartitionLocationCacheBucketCount    : 1024
                       ServiceChangePollInterval            : 00:02:00
                       ConnectionInitializationTimeout      : 00:00:02
                       KeepAliveInterval                    : 00:00:20
                       HealthOperationTimeout               : 00:02:00
                       HealthReportSendInterval             : 00:00:00
                       HealthReportRetrySendInterval        : 00:00:40
                       NotificationGatewayConnectionTimeout : 00:00:00
                       NotificationCacheUpdateTimeout       : 00:00:00
                       }
GatewayInformation   : {
                       NodeAddress                          : localhost:19000
                       NodeId                               : 1880ec88a3187766a6da323399721f53
                       NodeInstanceId                       : 130729063464981219
                       NodeName                             : Node.1
                       }
```

API'ye benzer şekilde, `-Immediate` `HealthReportSendInterval` raporlar değeri ne olursa olsun hemen gönderilmek üzere anahtar kullanılarak gönderilebilir.

REST için raporlar, dahili kumaş istemcisi olan Service Fabric ağ geçidine gönderilir. Varsayılan olarak, bu istemci her 30 saniyede bir toplu raporlar gönderecek şekilde yapılandırılır. Küme yapılandırma ayarı `HttpGatewayHealthReportSendInterval` ile `HttpGateway`toplu iş aralığını değiştirebilirsiniz. Belirtildiği gibi, daha iyi bir seçenek `Immediate` doğru raporları göndermektir. 

> [!NOTE]
> Yetkisiz hizmetlerin kümedeki varlıklara karşı sistem durumunu bildirememesini sağlamak için sunucuyu yalnızca güvenli istemcilerden gelen istekleri kabul layacak şekilde yapılandırın. Raporlama `FabricClient` için kullanılan kümeyle iletişim kurabilmek için güvenlik etkinleştirilmiş olmalıdır (örneğin, Kerberos veya sertifika kimlik doğrulaması ile). [Küme güvenliği](service-fabric-cluster-security.md)hakkında daha fazla bilgi edinin.
> 
> 

## <a name="report-from-within-low-privilege-services"></a>Düşük ayrıcalıklı hizmetler içinden rapor
Service Fabric hizmetlerinin kümeye yönetici erişimi yoksa, geçerli bağlam `Partition` üzerinden veya `CodePackageActivationContext`'den varlıklar üzerinde sistem durumunu bildirebilirsiniz.

* Durum damayan hizmetler için, geçerli hizmet örneğini bildirmek için [IStatelessServicePartition.ReportInstanceHealth'i](https://docs.microsoft.com/dotnet/api/system.fabric.istatelessservicepartition.reportinstancehealth) kullanın.
* Durum lu hizmetler için, geçerli yinelemeyi bildirmek için [IStatefulServicePartition.ReportReplicaHealth'i](https://docs.microsoft.com/dotnet/api/system.fabric.istatefulservicepartition.reportreplicahealth) kullanın.
* Geçerli bölüm varlığı hakkında rapor vermek için [IServicePartition.ReportPartitionHealth'i](https://docs.microsoft.com/dotnet/api/system.fabric.iservicepartition.reportpartitionhealth) kullanın.
* Geçerli uygulamayı raporlamak için [CodePackageActivationContext.ReportApplicationHealth'i](https://docs.microsoft.com/dotnet/api/system.fabric.codepackageactivationcontext.reportapplicationhealth) kullanın.
* Geçerli düğümüzerinde dağıtılan geçerli uygulamayı bildirmek için [CodePackageActivationContext.ReportDeployedApplicationHealth'i](https://docs.microsoft.com/dotnet/api/system.fabric.codepackageactivationcontext.reportdeployedapplicationhealth) kullanın.
* Geçerli düğümüzerinde dağıtılan uygulama için bir hizmet paketi rapor etmek için [CodePackageActivationContext.ReportDeployedServicePackageHealth](https://docs.microsoft.com/dotnet/api/system.fabric.codepackageactivationcontext.reportdeployedservicepackagehealth) kullanın.

> [!NOTE]
> Dahili olarak, `Partition` `CodePackageActivationContext` ve varsayılan ayarları ile yapılandırılan bir sistem durumu istemcisi tutun. [Sağlık istemcisi](service-fabric-report-health.md#health-client)için açıklandığı gibi, raporlar toplu ve bir zamanlayıcı üzerinde gönderilir. Raporu göndermek için nesneler canlı tutulmalıdır.
> 
> 

Rapor gönderirken `HealthReportSendOptions` `Partition` ve `CodePackageActivationContext` sistem durumu API'leri ile belirtebilirsiniz. En kısa sürede gönderilmesi gereken kritik raporlarınız `HealthReportSendOptions` varsa, `true`Hemen ile birlikte kullanın. Anında raporlar, iç sağlık istemcisinin toplu işleme aralığını atlar. Daha önce de belirtildiği gibi, özenle bu bayrağı kullanın; mümkün olduğunca sağlık istemcisi toplu işyararlanmak istiyorum.

## <a name="design-health-reporting"></a>Tasarım sağlık raporlaması
Yüksek kaliteli raporlar oluşturmanın ilk adımı, hizmetin sağlığını etkilenebilen koşulları belirlemektir. Hizmet veya kümedeki sorunları başlatıldığında veya daha da iyisi, bir sorun olmadan önce işaretlemeye yardımcı olabilecek herhangi bir koşul, milyarlarca dolar tasarruf edebilir. Avantajlar arasında daha az çalışma süresi, sorunları araştırmak ve onarmak için harcanan gece saatleri sayısı nın azalması ve müşteri memnuniyetinin artması yer alıyor.

Koşullar belirlendikten sonra, izleme örgütü yazarlarının genel merkez ve kullanışlılık arasındaki denge için onları izlemenin en iyi yolunu bulmaları gerekir. Örneğin, bir paylaşımda bazı geçici dosyalar kullanan karmaşık hesaplamalar yapan bir hizmet düşünün. Bir izleme örgütü yeterli alanın kullanılabilir olduğundan emin olmak için payı izleyebilir. Dosya bildirimleri veya dizin değişiklikleri dinleyebilir. Bir ön eşiğe ulaşılırsa bir uyarı bildirebilir ve pay doluysa bir hata bildirebilir. Bir uyarı üzerine, bir onarım sistemi paylaşımdaki eski dosyaları temizlemeye başlayabilir. Bir hatada, bir onarım sistemi hizmet yinelemesini başka bir düğüme taşıyabilir. Durum durumlarının sağlık açısından nasıl açıklandığına dikkat edin: sağlıklı (tamam) veya sağlıksız (uyarı veya hata) olarak kabul edilebilen durumun durumu.

İzleme ayrıntıları belirlendikten sonra, bir izleme örgütü yazarının izleme örgütünün nasıl uygulanacağını çözmesi gerekiyor. Koşullar hizmet içinden belirlenebiliyorsa, izleme örgütü izlenen hizmetin bir parçası olabilir. Örneğin, hizmet kodu paylaşım kullanımını denetleyebilir ve her dosya yazmaya çalıştığında rapor layabilir. Bu yaklaşımın avantajı raporlama nın basit olmasıdır. İzleme izleme hatalarının hizmet işlevselliğini etkilemesini önlemeye özen gerekir.

İzlenen hizmet içinden raporlama her zaman bir seçenek değildir. Hizmet içindeki bir izleme örgütü koşulları algılayamayabilir. Bu belirleme yapmak için mantık veya veri olmayabilir. Koşulları izleme yükü yüksek olabilir. Koşullar bir hizmete özgü olmayabilir, ancak bunun yerine hizmetler arasındaki etkileşimleri etkiler. Başka bir seçenek ayrı işlemler olarak kümede watchdogs olmasıdır. İzleme örgütleri, ana hizmetleri hiçbir şekilde etkilemeden koşulları ve raporu izliyorlar. Örneğin, bu izleme izleme örgütleri aynı uygulamada, tüm düğümlere veya hizmetle aynı düğümlerde dağıtılan, devletsiz hizmetler olarak uygulanabilir.

Bazen, kümede çalışan bir izleme örgütü de bir seçenek değildir. İzlenen koşul, kullanıcıların gördüğü hizmetin kullanılabilirliği veya işlevselliğiyse, izleme izleme köpeklerinin kullanıcı istemcileri ile aynı yerde olması en iyisidir. Burada, işlemleri kullanıcıların adlandırdığı şekilde test edebilirler. Örneğin, kümenin dışında yaşayan, hizmetisteklerini veren ve sonucun gecikmesini ve doğruluğunu denetleyen bir izleme izleme köpeğiniz olabilir. (Örneğin, bir hesap makinesi hizmeti için 2+2 4'ü makul bir süre içinde döndürer mi?)

İzleme ekibi ayrıntıları kesinleştikten sonra, onu benzersiz olarak tanımlayan bir kaynak kimliğine karar vermelisiniz. Kümede aynı türde birden çok izleme ekibi yaşıyorsa, farklı varlıklar hakkında rapor vermeleri veya aynı varlık üzerinde rapor verirlerse farklı kaynak kimliği veya özellik kullanmaları gerekir. Bu şekilde, raporları bir arada bulunabilir. Sağlık raporunun özelliği izlenen durumu yakalamalıdır. (Yukarıdaki örnekiçin, özellik **ShareSize**olabilir.) Aynı koşula birden çok rapor uygulanıyorsa, özellik raporların bir arada bulunmasına izin veren bazı dinamik bilgiler içermelidir. Örneğin, birden çok paylaşımın izlenmesi gerekiyorsa, özellik adı **ShareSize-sharename**olabilir.

> [!NOTE]
> Durum bilgilerini saklamak için sistem durumu deposunu *kullanmayın.* Bu bilgiler bir varlığın sağlık değerlendirmesini etkilediğinden, yalnızca sağlıkla ilgili bilgiler sağlık olarak rapor edilmelidir. Sağlık mağazası genel amaçlı bir mağaza olarak tasarlanmadı. Tüm verileri sistem durumu durumuna toplamak için sistem durumu değerlendirme mantığını kullanır. Sağlıkla ilgisi olmayan bilgilerin gönderilmesi (tamam durumu yla raporlama durumu gibi) toplu sağlık durumunu etkilemez, ancak sistem durumu deposunun performansını olumsuz etkileyebilir.
> 
> 

Bir sonraki karar noktası hangi varlık hakkında rapor etmektir. Çoğu zaman, durum varlığı açıkça tanımlar. Mümkün olan en iyi parçalılığa sahip varlığı seçin. Bir koşul bir bölümdeki tüm yinelemeleri etkilerse, hizmette değil, bölüm hakkında rapor verin. Yine de daha fazla düşünülmesine ihtiyaç duyulan köşe davaları var. Koşul yineleme gibi bir varlığı etkiler, ancak arzu yineleme ömrü nden daha uzun süre için işaretlenmiş olması, o zaman bölüm bildirilmelidir. Aksi takdirde, yineleme silindiğinde, sistem durumu deposu tüm raporlarını temizler. Watchdog yazarlar varlığın yaşam ları ve rapor hakkında düşünmek gerekir. Bir raporun bir mağazadan ne zaman temizlenmesi gerektiği açık olmalıdır (örneğin, bir varlık üzerinde bildirilen bir hata artık geçerli olmadığında).

Anlattığım noktaları bir araya getiren bir örneğe bakalım. Tüm düğümlerde (her görev türü için bir ikincil hizmet türü) dağıtılan eski bir durumlu kalıcı hizmet ve ikincil stateless hizmetlerinden oluşan bir Hizmet Kumaşı uygulamasını düşünün. Asıl, ikinciler tarafından yürütülecek komutları içeren bir işleme sırası vardır. İkinciler gelen istekleri yürütür ve onay sinyallerini geri gönderirler. İzlenebilir bir koşul ana işlem sırasının uzunluğudur. Ana sıra uzunluğu bir eşiğe ulaşırsa, bir uyarı bildirilir. Uyarı, ikincilerin yükü kaldıramalarını gösterir. Sıra en yüksek uzunluğa ulaşır ve komutlar bırakılırsa, hizmet kurtarılamayacağından bir hata bildirilir. Raporlar **özellik QueueStatus**üzerinde olabilir. Bekçi, servisin içinde yaşıyor ve ana birincil kopyaya periyodik olarak gönderiliyor. Yaşama süresi iki dakikadır ve her 30 saniyede bir periyodik olarak gönderilir. Birincil gider, rapor mağazadan otomatik olarak temizlenir. Hizmet yinelemesi dolduysa, ancak kilitlendiyse veya başka sorunlar varsa, raporun sistem durumu deposunda süresi doluyor. Bu durumda, varlık hata olarak değerlendirilir.

İzlenebilir başka bir koşul görev yürütme süresidir. Asıl, görevleri görev türüne göre ikincilere dağıtır. Tasarıma bağlı olarak, asıl görev durumu için ikincileri yoklayabilir. Ayrıca, ikincillerin onaylandıklarında onay sinyallerini geri göndermelerini de bekleyebilir. İkinci durumda, ikincilerin öldüğü veya mesajların kaybolduğu durumları tespit etmek için dikkatli olunmalıdır. Bir seçenek, ana kişinin durumunu geri gönderen aynı ikincil isteğe ping isteği göndermesidir. Durum alınmazsa, asıl hata olarak kabul eder ve görevi yeniden zamanlar. Bu davranış, görevlerin iktidarlı olduğunu varsayar.

Görev belirli bir süre içinde yapılmazsa izlenen durum uyarı olarak çevrilebilir **(t1**, örneğin 10 dakika). Görev zamanında tamamlanmazsa **(t2**, örneğin 20 dakika), izlenen koşul Hata olarak çevrilebilir. Bu raporlama birden çok şekilde yapılabilir:

* Ana birincil yineleme periyodik olarak kendisi hakkında raporlar. Kuyruktaki bekleyen tüm görevler için bir özelliğiniz olabilir. En az bir görev daha uzun sürüyorsa, **özellik Bekleme Görevleri'ndeki** rapor durumu uygun olduğu şekilde bir uyarı veya hatadır. Bekleyen görevler yoksa veya tüm görevler yürütmeye başladıysa, rapor durumu tamamdır. Görevler kalıcıdır. Birincil aşağı giderse, yeni tanıtılan birincil düzgün rapor devam edebilirsiniz.
* Başka bir izleme işlemi (bulutveya dışta) tamamlanıp tamamlanmadıklarını görmek için görevleri (istenilen görev sonucuna göre dışarıdan) denetler. Eşiklere uymazlarsa, ana hizmete bir rapor gönderilir. **Ayrıca, BeklemeGörev+taskId**gibi görev tanımlayıcısını içeren her göreve bir rapor gönderilir. Raporlar yalnızca sağlıksız durumlara gönderilmelidir. Zamanı birkaç dakikaya ayarlayın ve temizleme sağlamak için süresi dolduğunda kaldırılacak raporları işaretleyin.
* Bir görevin çalıştırılaması beklenenden daha uzun sürdüğünde görev raporlarını yürüten ikincil. Bu özellik **BekleyenGörevler**üzerinde hizmet örneği raporlar. Rapor, sorunları olan hizmet örneğini tam olarak saptar, ancak örneğin öldüğü durumu yakalamaz. Raporlar o zaman temizlenir. İkincil servis hakkında rapor verebilir. İkincil görevi tamamlarsa, ikincil örnek raporu mağazadan temizler. Rapor, bildirim iletisinin kaybolduğu ve görevin ustanın bakış açısından tamamlanmadığı durumu yakalamaz.

Ancak yukarıda açıklanan durumlarda raporlama yapılır, sağlık değerlendirildiğinde raporlar uygulama sağlığı nda yakalanır.

## <a name="report-periodically-vs-on-transition"></a>Geçiş le ilgili periyodik olarak rapor
İzleme örgütleri, sistem durumu raporlama modelini kullanarak düzenli olarak veya geçişler hakkında rapor gönderebilir. Kod çok daha basit ve hatalara daha az eğilimli olduğundan, izleme örgütü raporlaması için önerilen yol düzenli aralıklarla dır. İzleme örgütleri, yanlış raporları tetikleyen hataları önlemek için mümkün olduğunca basit olmaya gayret etmelidir. Yanlış *sağlıksız* raporlar, yükseltmeler de dahil olmak üzere sağlık durumuna dayalı sağlık değerlendirmelerini ve senaryoları etkiler. Yanlış *sağlıklı* raporlar kümedeki, istenmeyen sorunları gizler.

Periyodik raporlama için, watchdog bir zamanlayıcı ile uygulanabilir. Zamanlayıcı geri aramasında, izleme örgütü durumu kontrol edebilir ve geçerli duruma göre bir rapor gönderebilir. Hangi raporun daha önce gönderildiğini görmenize veya mesajlaşma açısından herhangi bir optimizasyon yapmanıza gerek yoktur. Sistem durumu istemcisi, performansa yardımcı olacak toplu iş leme mantığına sahiptir. Sağlık istemcisi canlı tutulurken, rapor sağlık deposu tarafından kabul edilene veya izleme örgütü aynı varlık, özellik ve kaynakla daha yeni bir rapor oluşturana kadar dahili olarak yeniden çalışır.

Geçişlerle ilgili raporlama, devletin dikkatli bir şekilde ele alınmasını gerektirir. İzleme örgütü bazı koşulları izler ve yalnızca koşullar değiştiğinde raporlar. Bu yaklaşımın iyi tarafı, daha az rapor gerekli olmasıdır. Dezavantajı bekçi mantığı karmaşık olmasıdır. İzleme örgütü, devlet değişikliklerini belirlemek için denetlenebilmeleri için koşulları veya raporları korumalıdır. Başarısız olunması üzerine, ekraporlar la bakım alınmalıdır, ancak henüz sağlık deposuna gönderilmemelidir. Sıra numarası sürekli artıyor olmalı. Değilse, raporlar bayat olarak reddedilir. Veri kaybının yaşandığı nadir durumlarda, muhabirin durumu ile sağlık deposunun durumu arasında senkronizasyon gerekebilir.

Geçişler hakkında raporlama, hizmetlerin kendileri aracılığıyla `Partition` `CodePackageActivationContext`veya . Yerel nesne (çoğaltma veya dağıtılan hizmet paketi / dağıtılmış uygulama) kaldırıldığında, tüm raporları da kaldırılır. Bu otomatik temizleme muhabiri ve sağlık deposu arasında senkronizasyon ihtiyacını rahatlatır. Rapor üst bölüm veya üst uygulama içinise, sağlık deposunda eski raporları önlemek için başarısız lığa dikkat edilmelidir. Doğru durumu korumak ve artık gerekmediğinde raporu depodan temizlemek için mantık eklenmelidir.

## <a name="implement-health-reporting"></a>Sağlık raporlaması uygulayın
Varlık ve rapor ayrıntıları netleştikten sonra, sağlık raporlarının gönderilmesi API, PowerShell veya REST üzerinden yapılabilir.

### <a name="api"></a>API
API üzerinden rapor vermek için, rapor vermek istedikleri varlık türüne özgü bir sistem durumu raporu oluşturmanız gerekir. Raporu bir sağlık istemcisine verin. Alternatif olarak, bir sistem durumu bilgisi oluşturun `Partition` ve `CodePackageActivationContext` raporlama yöntemlerini düzeltmek veya geçerli varlıklar üzerinde rapor vermek için aktarın.

Aşağıdaki örnek, küme içindeki bir izleme örgütünden gelen periyodik raporlamayı gösterir. İzleme örgütü, bir dış kaynağa düğüm içinden erişilip erişilemeyeceğini denetler. Kaynak, uygulama içinde bir hizmet bildirimi tarafından gereklidir. Kaynak kullanılamıyorsa, uygulama içindeki diğer hizmetler düzgün çalışmaya devam edebilir. Bu nedenle, rapor dağıtılan hizmet paketi varlığıüzerinde her 30 saniyede bir gönderilir.

```csharp
private static Uri ApplicationName = new Uri("fabric:/WordCount");
private static string ServiceManifestName = "WordCount.Service";
private static string NodeName = FabricRuntime.GetNodeContext().NodeName;
private static Timer ReportTimer = new Timer(new TimerCallback(SendReport), null, 30 * 1000, 30 * 1000);
private static FabricClient Client = new FabricClient(new FabricClientSettings() { HealthReportSendInterval = TimeSpan.FromSeconds(0) });

public static void SendReport(object obj)
{
    // Test whether the resource can be accessed from the node
    HealthState healthState = this.TestConnectivityToExternalResource();

    // Send report on deployed service package, as the connectivity is needed by the specific service manifest
    // and can be different on different nodes
    var deployedServicePackageHealthReport = new DeployedServicePackageHealthReport(
        ApplicationName,
        ServiceManifestName,
        NodeName,
        new HealthInformation("ExternalSourceWatcher", "Connectivity", healthState));

    // TODO: handle exception. Code omitted for snippet brevity.
    // Possible exceptions: FabricException with error codes
    // FabricHealthStaleReport (non-retryable, the report is already queued on the health client),
    // FabricHealthMaxReportsReached (retryable; user should retry with exponential delay until the report is accepted).
    Client.HealthManager.ReportHealth(deployedServicePackageHealthReport);
}
```

### <a name="powershell"></a>PowerShell
Sağlık raporlarını **Gönder-ServiceFabric*EntityType*HealthReport**ile gönderin.

Aşağıdaki örnek, bir düğümüzerindeki CPU değerleri üzerinde periyodik raporlamayı gösterir. Raporlar her 30 saniyede bir gönderilmeli ve iki dakika yaşamak için zamanları var. Sona ererse, muhabirin sorunları vardır, bu nedenle düğüm hata yla değerlendirilir. CPU bir eşiğin üzerinde olduğunda, raporun sistem durumu bir uyarı durumu vardır. CPU, yapılandırılan süreden daha uzun süre bir eşiğin üzerinde kaldığında, bu bir hata olarak bildirilir. Aksi takdirde, muhabir tamam bir sağlık durumu gönderir.

```powershell
PS C:\> Send-ServiceFabricNodeHealthReport -NodeName Node.1 -HealthState Warning -SourceId PowershellWatcher -HealthProperty CPU -Description "CPU is above 80% threshold" -TimeToLiveSec 120

PS C:\> Get-ServiceFabricNodeHealth -NodeName Node.1
NodeName              : Node.1
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='PowershellWatcher', Property='CPU', HealthState='Warning', ConsiderWarningAsError=false.

HealthEvents          :
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 5
                        SentAt                : 4/21/2015 8:01:17 AM
                        ReceivedAt            : 4/21/2015 8:02:12 AM
                        TTL                   : Infinite
                        Description           : Fabric node is up.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/21/2015 8:02:12 AM

                        SourceId              : PowershellWatcher
                        Property              : CPU
                        HealthState           : Warning
                        SequenceNumber        : 130741236814913394
                        SentAt                : 4/21/2015 9:01:21 PM
                        ReceivedAt            : 4/21/2015 9:01:21 PM
                        TTL                   : 00:02:00
                        Description           : CPU is above 80% threshold
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Warning = 4/21/2015 9:01:21 PM
```

Aşağıdaki örnek, bir yineleme üzerinde geçici bir uyarı bildirir. Önce bölüm kimliğini, ardından ilgilendiği hizmetin çoğaltma kimliğini alır. Daha sonra **powershellWatcher** gelen özellik **ResourceDependency**bir rapor gönderir. Rapor yalnızca iki dakika lığına ilgi çekicidir ve otomatik olarak mağazadan kaldırılır.

```powershell
PS C:\> $partitionId = (Get-ServiceFabricPartition -ServiceName fabric:/WordCount/WordCount.Service).PartitionId

PS C:\> $replicaId = (Get-ServiceFabricReplica -PartitionId $partitionId | where {$_.ReplicaRole -eq "Primary"}).ReplicaId

PS C:\> Send-ServiceFabricReplicaHealthReport -PartitionId $partitionId -ReplicaId $replicaId -HealthState Warning -SourceId PowershellWatcher -HealthProperty ResourceDependency -Description "The external resource that the primary is using has been rebooted at 4/21/2015 9:01:21 PM. Expect processing delays for a few minutes." -TimeToLiveSec 120 -RemoveWhenExpired

PS C:\> Get-ServiceFabricReplicaHealth  -PartitionId $partitionId -ReplicaOrInstanceId $replicaId


PartitionId           : 8f82daff-eb68-4fd9-b631-7a37629e08c0
ReplicaId             : 130740415594605869
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='PowershellWatcher', Property='ResourceDependency', HealthState='Warning', ConsiderWarningAsError=false.

HealthEvents          :
                        SourceId              : System.RA
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 130740768777734943
                        SentAt                : 4/21/2015 8:01:17 AM
                        ReceivedAt            : 4/21/2015 8:02:12 AM
                        TTL                   : Infinite
                        Description           : Replica has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : ->Ok = 4/21/2015 8:02:12 AM

                        SourceId              : PowershellWatcher
                        Property              : ResourceDependency
                        HealthState           : Warning
                        SequenceNumber        : 130741243777723555
                        SentAt                : 4/21/2015 9:12:57 PM
                        ReceivedAt            : 4/21/2015 9:12:57 PM
                        TTL                   : 00:02:00
                        Description           : The external resource that the primary is using has been rebooted at 4/21/2015 9:01:21 PM. Expect processing delays for a few minutes.
                        RemoveWhenExpired     : True
                        IsExpired             : False
                        Transitions           : ->Warning = 4/21/2015 9:12:32 PM
```

### <a name="rest"></a>REST
İstenilen varlığa giden ve vücutta sağlık raporu açıklaması bulunan POST istekleriyle REST kullanarak sağlık raporları gönderin. Örneğin, REST [kümesi sağlık raporlarının](https://docs.microsoft.com/rest/api/servicefabric/report-the-health-of-a-cluster) veya hizmet sağlık [raporlarının](https://docs.microsoft.com/rest/api/servicefabric/report-the-health-of-a-service)nasıl gönderilebildiğini görün. Tüm varlıklar desteklenir.

## <a name="next-steps"></a>Sonraki adımlar
Sistem durumu verilerine dayanarak, hizmet yazarları ve küme/uygulama yöneticileri bilgileri tüketmenin yollarını düşünebilir. Örneğin, kesintileri kışkırtmadan önce ciddi sorunları yakalamak için sağlık durumuna göre uyarılar ayarlayabilirler. Yöneticiler, sorunları otomatik olarak gidermek için onarım sistemleri de ayarlayabilir.

[Hizmet Kumaş Sağlık İzleme Giriş](service-fabric-health-introduction.md)

[Hizmet Kumaşı sağlık raporlarını görüntüleyin](service-fabric-view-entities-aggregated-health.md)

[Hizmet durumunu bildirme ve kontrol etme](service-fabric-diagnostics-how-to-report-and-check-service-health.md)

[Sorun giderme için sistem sistem sistem raporlarıkullanma](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)

[Hizmetleri yerel olarak izleme ve tanılama](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[Hizmet Kumaş uygulama yükseltme](service-fabric-application-upgrade.md)

