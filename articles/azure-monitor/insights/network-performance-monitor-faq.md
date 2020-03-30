---
title: SSS ' ler - Azure'da Ağ Performans Monitörü çözümü | Microsoft Dokümanlar
description: Bu makalede, Azure'da Ağ Performans İzleyicisi hakkında sık sorulan sorular ele geçirilebis. Ağ Performans İzleyicisi (NPM), ağlarınızın performansını neredeyse gerçek zamanlı olarak izlemenize ve ağ performansı sorunlarını algılamanıza ve bulmanıza yardımcı olur.
ms.subservice: logs
ms.topic: conceptual
author: vinynigam
ms.author: vinigam
ms.date: 10/12/2018
ms.openlocfilehash: 443e4b44633e949dd9bd55df1ec7d18ca93d6e04
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79096215"
---
# <a name="network-performance-monitor-solution-faq"></a>Ağ Performans Monitörü çözümü SSS

![Ağ Performans Monitörü simgesi](media/network-performance-monitor-faq/npm-symbol.png)

Bu makale, Azure'da Ağ Performans İzleyicisi (NPM) hakkında sık sorulan soruları (SSSS) yakalar

[Ağ Performans İzleyicisi,](/azure/networking/network-monitoring-overview) ağ altyapınızdaki çeşitli noktalar arasındaki ağ performansını izlemenize yardımcı olan bulut tabanlı [karma ağ izleme](../../azure-monitor/insights/network-performance-monitor-performance-monitor.md) çözümüdür. Ayrıca, hizmet ve uygulama [uç noktalarına](../../azure-monitor/insights/network-performance-monitor-service-connectivity.md) ağ bağlantısını izlemenize ve [Azure ExpressRoute'un performansını izlemenize](../../azure-monitor/insights/network-performance-monitor-expressroute.md)de yardımcı olur. 

Ağ Performans İzleyicisi, trafik karaollama, yönlendirme hataları ve geleneksel ağ izleme yöntemlerinin algılanamayan sorunları gibi ağ sorunlarını algılar. Bir ağ bağlantısı için bir eşik ihlal edildiğinde, çözüm uyarılar oluşturur ve size bildirir. Ayrıca ağ performans sorunlarının zamanında tespit edilmesini sağlar ve sorunun kaynağını belirli bir ağ kesimine veya cihazına yerelleştirir. 

[Network Performance Monitor](https://docs.microsoft.com/azure/networking/network-monitoring-overview) tarafından desteklenen çeşitli yetenekler hakkında daha fazla bilgi çevrimiçi olarak mevcuttur.

## <a name="set-up-and-configure-agents"></a>Aracıları ayarlama ve yapılandırma

### <a name="what-are-the-platform-requirements-for-the-nodes-to-be-used-for-monitoring-by-npm"></a>NPM tarafından izlenmesi için kullanılacak düğümler için platform gereksinimleri nelerdir?
Aşağıda NPM'nin çeşitli yetenekleri için platform gereksinimleri listelenmiştir:

- NPM'nin Performans İzleyicisi ve Hizmet Bağlantısı İzleme özellikleri hem Windows sunucusuhem de Windows masaüstü/istemci işletim sistemlerini destekler. Desteklenen Windows server OS sürümleri 2008 SP1 veya daha sonraki sürümlerdir. Desteklenen Windows masaüstü/istemci sürümleri Windows 10, Windows 8.1, Windows 8 ve Windows 7'dir. 
- NPM'nin ExpressRoute Monitor özelliği yalnızca Windows sunucusu (2008 SP1 veya sonraki) işletim sistemini destekler.

### <a name="can-i-use-linux-machines-as-monitoring-nodes-in-npm"></a>Linux makinelerini NPM'de izleme düğümleri olarak kullanabilir miyim?
Linux tabanlı düğümleri kullanarak ağları izleme özelliği şu anda önizlemededir. Daha fazla şey öğrenmek için Hesap Yöneticinize ulaşın. Linux aracıları yalnızca NPM'nin Performans İzleme özelliği için izleme yeteneği sağlar ve Service Connectivity Monitor ve ExpressRoute Monitor yetenekleri için kullanılamaz

### <a name="what-are-the-size-requirements-of-the-nodes-to-be-used-for-monitoring-by-npm"></a>NPM tarafından izlenmesi için kullanılacak düğümlerin boyut gereksinimleri nelerdir?
Ağları izlemek için düğüm VM'lerde NPM çözümünü çalıştırmak için düğümlerin en az 500 MB bellek ve bir çekirdek olması gerekir. NPM çalıştırmak için ayrı düğümler kullanmanız gerekmez. Çözüm, üzerinde çalışan başka iş yükleri olan düğümler üzerinde çalıştırılabilir. Çözüm, %5'ten fazla CPU kullanıyorsa izleme işlemini durdurma özelliğine sahiptir.

### <a name="to-use-npm-should-i-connect-my-nodes-as-direct-agent-or-through-system-center-operations-manager"></a>NPM'yi kullanmak için düğümlerimi Doğrudan temsilci olarak mı yoksa Sistem Merkezi İşlemleri Yöneticisi aracılığıyla mı bağlamalıyım?
Hem Performans İzleyicisi hem de Hizmet Bağlantısı İzleyicisi [yetenekleri, Doğrudan Aracı olarak bağlanan](../../azure-monitor/platform/agent-windows.md) ve [Operasyon Yöneticisi aracılığıyla bağlanan](../../azure-monitor/platform/om-agents.md)düğümleri destekler.

ExpressRoute Monitor özelliği için Azure düğümleri yalnızca Doğrudan Aracılar olarak bağlanmalıdır. Operations Manager aracılığıyla bağlanan Azure düğümleri desteklenmez. Şirket içi düğümler için, Doğrudan Aracılar ve Operations Manager aracılığıyla bağlanan düğümler, expressroute devresini izlemek için desteklenir.

### <a name="which-protocol-among-tcp-and-icmp-should-be-chosen-for-monitoring"></a>İzleme için TCP ve ICMP arasında hangi protokol seçilmelidir?
Ağınızı Windows sunucu tabanlı düğümleri kullanarak izliyorsanız, daha iyi doğruluk sağladığından TCP'yi izleme protokolü olarak kullanmanızı öneririz. 

ICMP, Windows masaüstü/istemci işletim sistemi tabanlı düğümler için önerilir. Bu platform, NPM'nin ağ topolojisini keşfetmek için kullandığı ham soketler üzerinden TCP verilerinin gönderilmesine izin vermez.

[Burada](../../azure-monitor/insights/network-performance-monitor-performance-monitor.md#choose-the-protocol)her protokolün göreli avantajları hakkında daha fazla bilgi alabilirsiniz.

### <a name="how-can-i-configure-a-node-to-support-monitoring-using-tcp-protocol"></a>TCP protokolünü kullanarak izlemeyi destekleyecek bir düğümü nasıl yapılandırabilirim?
Düğümün TCP protokolü kullanılarak izlemeyi desteklemesi için: 
* Düğüm platformunun Windows Server (2008 SP1 veya sonraki) olduğundan emin olun.
* Düğümüzerinde [EnableRules.ps1](https://aka.ms/npmpowershellscript) Powershell komut dosyası çalıştırın. Daha fazla ayrıntı için [talimatlara](../../azure-monitor/insights/network-performance-monitor.md#configure-log-analytics-agents-for-monitoring) bakın.


### <a name="how-can-i-change-the-tcp-port-being-used-by-npm-for-monitoring"></a>NPM tarafından izleme için kullanılan TCP bağlantı noktasını nasıl değiştirebilirim?
[EnableRules.ps1](https://aka.ms/npmpowershellscript) komut dosyasını çalıştırarak NPM tarafından izleme için kullanılan TCP bağlantı noktasını değiştirebilirsiniz. Parametre olarak kullanmak istediğiniz bağlantı noktası numarasını girmeniz gerekir. Örneğin, 8060 bağlantı noktasında TCP'yi etkinleştirmek için çalıştırın. `EnableRules.ps1 8060` İzleme için kullanılan tüm düğümlerde aynı TCP bağlantı noktasını kullandığınızdan emin olun.

Komut dosyası yalnızca Windows Güvenlik Duvarı'nı yerel olarak yapılandırır. Ağ güvenlik duvarınız veya Ağ Güvenlik Grubu (NSG) kurallarınız varsa, Bunların NPM tarafından kullanılan TCP bağlantı noktasına yönelik trafiğe izin verdiğinden emin olun.

### <a name="how-many-agents-should-i-use"></a>Kaç ajan kullanmalıyım?
İzlemek istediğiniz her alt ağ için en az bir aracı kullanmalısınız.

### <a name="what-is-the-maximum-number-of-agents-i-can-use-or-i-see-error--youve-reached-your-configuration-limit"></a>Kullanabileceğim veya hata görebildiğim maksimum aracı sayısı nedir ".... Yapılandırma sınırınıza ulaştınız"?
NPM, IP sayısını çalışma alanı başına 5000 IP ile sınırlar. Bir düğümün hem IPv4 hem de IPv6 adresleri varsa, bu düğüm için 2 IP olarak sayılır. Bu nedenle, 5000 IP'lik bu sınır, aracı sayısının üst sınırına karar verecekti. Etkin olmayan aracıları NPM >> Configure'deki Düğümler sekmesinden silebilirsiniz. NPM ayrıca, aracıyı barındıran VM'ye atanmış tüm IP'lerin geçmişini de korur ve her biri 5000 IP'lik üst sınıra katkıda bulunan ayrı IP olarak sayılır. Çalışma alanınız için IP'leri serbest bırakmanız için, kullanılmayan IP'leri silmek için Düğümler sayfasını kullanabilirsiniz.

## <a name="monitoring"></a>İzleme

### <a name="how-are-loss-and-latency-calculated"></a>Kayıp ve gecikme nasıl hesaplanır?
Kaynak aracılar, kaynak hedef IP arasındaki tüm yolların olmasını sağlamak için düzenli aralıklarla IP'yi hedef almaya ya TCP SYN isteklerini (Izleme protokolü olarak TCP seçilirse) veya ICMP ECHO isteklerini (izleme protokolü olarak icmp seçilirse) gönderir kombinasyonu kapsanmaktadır. Alınan paketlerin yüzdesi ve paket gidiş-dönüş süresi, her yolun kaybını ve gecikme süresini hesaplamak için ölçülür. Bu veriler, belirli yoklama aralığı için IP birleşimi için kayıp ve gecikme toplam değerlerini almak için yoklama aralığı ve tüm yollar üzerinde toplanır.

### <a name="with-what-frequency-does-the-source-agent-send-packets-to-the-destination-for-monitoring"></a>Kaynak aracı hangi sıklıkta izleme için hedefe paket gönderir?
Performans İzleme ve ExpressRoute Monitor yetenekleri için kaynak her 5 saniyede bir paket gönderir ve ağ ölçümlerini kaydeder. Bu veriler, kayıp ve gecikme ortalamave tepe değerlerini hesaplamak için 3 dakikalık bir yoklama aralığı nda toplanır. Servis Bağlantısı İzleme özelliği için, ağ ölçümü için paketleri gönderme sıklığı, testi yapılandırırken kullanıcı tarafından belirli bir test için girilen sıklık tarafından belirlenir.

### <a name="how-many-packets-are-sent-for-monitoring"></a>İzleme için kaç paket gönderilir?
Bir yoklamada kaynak aracı tarafından hedefe gönderilen paket sayısı uyarlanabilir ve farklı ağ topolojileri için farklı olabilecek özel algoritmamız tarafından belirlenir. Kaynak hedef IP birleşimi arasındaki ağ yollarının sayısı daha fazla, gönderilen paket sayısı dır. Sistem, kaynak hedef IP kombinasyonu arasındaki tüm yolların kapalı olmasını sağlar.

### <a name="how-does-npm-discover-network-topology-between-source-and-destination"></a>NPM kaynak ve hedef arasındaki ağ topolojisi nasıl keşfeder?
NPM, kaynak ve hedef arasındaki tüm yolları ve atlamaları keşfetmek için Traceroute'u temel alan özel bir algoritma kullanır.

### <a name="does-npm-provide-routing-and-switching-level-info"></a>NPM yönlendirme ve anahtarlama düzeyi bilgileri sağlar mı 
NPM kaynak aracı ile hedef arasındaki tüm olası yolları algılayabilse de, belirli iş yükleri tarafından gönderilen paketler tarafından hangi rotanın alındığına görünürlük sağlamaz. Çözüm, beklediğinden daha fazla gecikme gecikmesi ekleyen yolları ve altta yatan ağ atlamalarını belirlemenize yardımcı olabilir.

### <a name="why-are-some-of-the-paths-unhealthy"></a>Neden bazı yollar sağlıksız?
Kaynak ve hedef IP'ler arasında farklı ağ yolları bulunabilir ve her yol farklı bir kayıp ve gecikme değerine sahip olabilir. NPM bu yolları, kayıp ve/veya gecikme değerlerinin izleme yapılandırmasında ayarlanan ilgili eşikten daha büyük olduğu sağlıksız (kırmızı renkle gösterilir) olarak işaretler.

### <a name="what-does-a-hop-in-red-color-signify-in-the-network-topology-map"></a>Ağ topolojisi haritasında kırmızı renkli bir zıplatme ne anlama geliyor?
Atlama kırmızıysa, en az bir sağlıksız yolun parçası olduğunu belirtir. NPM yalnızca sağlıksız olarak yolları işaretler, her yolun sağlık durumunu ayırmaz. Zahmetli atlamaları tanımlamak için hop-by-hop gecikmesini görüntüleyebilir ve beklenenden fazla gecikme sonları ekleyerek ayırabilirsiniz.

### <a name="how-does-fault-localization-in-performance-monitor-work"></a>Performans İzleyicisi'nde hata yerelleştirme nasıl çalışır?
NPM, her ağ yoluna, ağ kesimine ve kurucu ağ atlamalarına parçası oldukları sağlıksız yolların sayısına bağlı olarak hata olasılıkları atamak için olasılıksal bir mekanizma kullanır. Ağ bölümleri ve atlamaları daha fazla sağlıksız yolun parçası haline geldikçe, bunlarla ilişkili hata olasılığı artar. Bu algoritma, hata olasılıklarını hesaplamak için veri noktalarını artırdığı için birbirine bağlı NPM aracısı olan birçok düğümolduğunda en iyi şekilde çalışır.

### <a name="how-can-i-create-alerts-in-npm"></a>NPM'de nasıl uyarı oluşturabilirim?
Adım adım talimatlar için [belgelerdeki uyarılar bölümüne](https://docs.microsoft.com/azure/log-analytics/log-analytics-network-performance-monitor#alerts) bakın.

### <a name="what-are-the-default-log-analytics-queries-for-alerts"></a>Uyarılar için varsayılan Log Analytics sorguları nelerdir
Performans izleme sorgusu

    NetworkMonitoring 
     | where (SubType == "SubNetwork" or SubType == "NetworkPath") 
     | where (LossHealthState == "Unhealthy" or LatencyHealthState == "Unhealthy") and RuleName == "<<your rule name>>"
    
Hizmet bağlantısı izleme sorgusu

    NetworkMonitoring                 
     | where (SubType == "EndpointHealth" or SubType == "EndpointPath")
     | where (LossHealthState == "Unhealthy" or LatencyHealthState == "Unhealthy" or ServiceResponseHealthState == "Unhealthy" or LatencyHealthState == "Unhealthy") and TestName == "<<your test name>>"
    
ExpressRoute monitör sorguları: Devreler sorgusu

    NetworkMonitoring
    | where (SubType == "ERCircuitTotalUtilization") and (UtilizationHealthState == "Unhealthy") and CircuitResourceId == "<<your circuit resource ID>>"

Özel eşleme

    NetworkMonitoring 
     | where (SubType == "ExpressRoutePeering" or SubType == "ERVNetConnectionUtilization" or SubType == "ExpressRoutePath")   
    | where (LossHealthState == "Unhealthy" or LatencyHealthState == "Unhealthy" or UtilizationHealthState == "Unhealthy") and CircuitName == "<<your circuit name>>" and VirtualNetwork == "<<vnet name>>"

Microsoft eşlemesi

    NetworkMonitoring 
     | where (SubType == "ExpressRoutePeering" or SubType == "ERMSPeeringUtilization" or SubType == "ExpressRoutePath")
    | where (LossHealthState == "Unhealthy" or LatencyHealthState == "Unhealthy" or UtilizationHealthState == "Unhealthy") and CircuitName == ""<<your circuit name>>" and PeeringType == "MicrosoftPeering"

Ortak sorgu   

    NetworkMonitoring
    | where (SubType == "ExpressRoutePeering" or SubType == "ERVNetConnectionUtilization" or SubType == "ERMSPeeringUtilization" or SubType == "ExpressRoutePath")
    | where (LossHealthState == "Unhealthy" or LatencyHealthState == "Unhealthy" or UtilizationHealthState == "Unhealthy") 

### <a name="can-npm-monitor-routers-and-servers-as-individual-devices"></a>NPM yönlendiricileri ve sunucuları tek tek aygıtlar olarak izleyebilir mi?
NPM yalnızca kaynak ve hedef IP'ler arasında temel ağ atlamalarının (anahtarlar, yönlendiriciler, sunucular, vb.) IP ve ana bilgisayar adını tanımlar. Ayrıca, bu tanımlanan atlamalar arasındaki gecikmeyi de tanımlar. Bu temel atlamaları tek tek izlemez.

### <a name="can-npm-be-used-to-monitor-network-connectivity-between-azure-and-aws"></a>NPM, Azure ve AWS arasındaki ağ bağlantısını izlemek için kullanılabilir mi?
Evet. Ayrıntılar için [NPM'yi kullanan Azure, AWS ve şirket içi ağları izleyin](https://blogs.technet.microsoft.com/msoms/2016/08/30/monitor-on-premises-cloud-iaas-and-hybrid-networks-using-oms-network-performance-monitor/) makalesine bakın.

### <a name="is-the-expressroute-bandwidth-usage-incoming-or-outgoing"></a>ExpressRoute bant genişliği kullanımı geliyor mu yoksa giden mi?
Bant genişliği kullanımı gelen ve giden bant genişliğinin toplamıdır. Bit/sn cinsinden ifade edilir.

### <a name="can-we-get-incoming-and-outgoing-bandwidth-information-for-the-expressroute"></a>ExpressRoute için gelen ve giden bant genişliği bilgilerini alabilir miyiz?
Hem Birincil hem de İkincil bant genişliği için gelen ve giden değerler yakalanabilir.

MS peering düzey bilgileri için, Log Search'te aşağıdaki sorguyu kullanın

    NetworkMonitoring 
     | where SubType == "ERMSPeeringUtilization"
     | project  CircuitName,PeeringName,PrimaryBytesInPerSecond,PrimaryBytesOutPerSecond,SecondaryBytesInPerSecond,SecondaryBytesOutPerSecond
    
Özel akran düzeyi bilgileri için, Log Search'te aşağıdaki sorguyu kullanın

    NetworkMonitoring 
     | where SubType == "ERVNetConnectionUtilization"
     | project  CircuitName,PeeringName,PrimaryBytesInPerSecond,PrimaryBytesOutPerSecond,SecondaryBytesInPerSecond,SecondaryBytesOutPerSecond
  
Devre düzeyi bilgileri için, Log Search'te aşağıdaki sorguyu kullanın

    NetworkMonitoring 
        | where SubType == "ERCircuitTotalUtilization"
        | project CircuitName, PrimaryBytesInPerSecond, PrimaryBytesOutPerSecond,SecondaryBytesInPerSecond,SecondaryBytesOutPerSecond

### <a name="which-regions-are-supported-for-npms-performance-monitor"></a>NPM'nin Performans Monitörü için hangi bölgeler destekleniyor?
NPM, [desteklenen bölgelerden](../../azure-monitor/insights/network-performance-monitor.md#supported-regions) birinde barındırılan bir çalışma alanından, dünyanın herhangi bir yerindeki ağlar arasındaki bağlantıyı izleyebilir

### <a name="which-regions-are-supported-for-npms-service-connectivity-monitor"></a>NPM'nin Hizmet Bağlantısı İzleyicisi için hangi bölgeler desteklenir?
NPM, [desteklenen bölgelerden](../../azure-monitor/insights/network-performance-monitor.md#supported-regions) birinde barındırılan bir çalışma alanından, dünyanın herhangi bir yerindeki hizmetlere bağlantı sağlayabilir

### <a name="which-regions-are-supported-for-npms-expressroute-monitor"></a>NPM'nin ExpressRoute Monitor'u için hangi bölgeler destekleniyor?
NPM, herhangi bir Azure bölgesinde bulunan ExpressRoute devrelerinizi izleyebilir. NPM'ye dahil olmak için, [desteklenen bölgelerden](/azure/expressroute/how-to-npm) birinde barındırılması gereken bir Log Analytics çalışma alanına ihtiyacınız olacak

## <a name="troubleshoot"></a>Sorun giderme

### <a name="why-are-some-of-the-hops-marked-as-unidentified-in-the-network-topology-view"></a>Neden bazı atlamalar ağ topolojisi görünümünde tanımlanmamış olarak işaretlenir?
NPM, kaynak maddeden hedefe giden topolojiyi keşfetmek için izleme yolunun değiştirilmiş bir sürümünü kullanır. Tanımlanamayan bir atlama, ağ atlamasının kaynak aracının izleme yolu isteğine yanıt vermediğini gösterir. Art arda üç ağ atlamaaracının izleme rotasına yanıt vermezse, çözüm yanıt vermeyen atlamaları tanımlanmamış olarak işaretler ve daha fazla atlama bulmaya çalışmaz.

Atlama, aşağıdaki senaryolardan birinde veya daha fazlasında bir izleme rotasına yanıt vermeyebilir:

* Yönlendiriciler kimliklerini açıklamamak için yapılandırılmıştır.
* Ağ aygıtları ICMP_TTL_EXCEEDED trafiğine izin vermiyor.
* Bir güvenlik duvarı, ağ aygıtından gelen ICMP_TTL_EXCEEDED yanıtı engelliyor.

Bitiş noktalarından biri Azure'da yattığında, Azure Altyapısı izleme rotasına kimlik göstermediği için izleme rotası tanımlanamayan atlamalarda gösterir. 

### <a name="i-get-alerts-for-unhealthy-tests-but-i-do-not-see-the-high-values-in-npms-loss-and-latency-graph-how-do-i-check-what-is-unhealthy"></a>Ben sağlıksız testler için uyarılar almak ama NPM kaybı ve gecikme grafiğinde yüksek değerleri görmüyorum. Neyin sağlıksız olduğunu nasıl kontrol edebilirim?
Kaynak ve hedef arasındaki bitiş gecikmesinin sona ermesi, aralarındaki herhangi bir yol için eşiği geçerse NPM bir uyarı yükseltir. Bazı ağların aynı kaynağı ve hedefi bağlayan birden çok yolu vardır. NPM herhangi bir yol sağlıksız olduğunu bir uyarı yükseltir. Grafiklerde görülen kayıp ve gecikme, tüm yolların ortalama değeridir, bu nedenle tek bir yolun tam değerini göstermeyebilir. Eşiğin nerede ihlal edildiğini anlamak için uyarıdaki "Alt Tip" sütununa bakın. Sorun bir yoldan kaynaklanıyorsa, Alt Tür değeri NetworkPath (Performans İzleyicisi testleri için), EndpointPath (Hizmet Bağlantısı İzleme Testleri için) ve ExpressRoutePath (ExpressRotue Monitor testleri için) olacaktır. 

Bulmak için Örnek Sorgu yolu sağlıksız:

    NetworkMonitoring 
    | where ( SubType == "ExpressRoutePath")
    | where (LossHealthState == "Unhealthy" or LatencyHealthState == "Unhealthy" or UtilizationHealthState == "Unhealthy") and          CircuitResourceID =="<your ER circuit ID>" and ConnectionResourceId == "<your ER connection resource id>"
    | project SubType, LossHealthState, LatencyHealthState, MedianLatency 

### <a name="why-does-my-test-show-unhealthy-but-the-topology-does-not"></a>Neden benim test sağlıksız gösteriyor ama topoloji yok 
NPM, uçlardan uca kaybı, gecikme süresini ve topolojiyi farklı aralıklarla izler. Kayıp ve gecikme her 5 saniyede bir ölçülür ve her üç dakikada bir (Performans Monitörü ve Ekspres Rota Monitörü için) toplanırken topoloji her 10 dakikada bir izleme yolu kullanılarak hesaplanır. Örneğin, 3:44 ile 04:04 arasında topoloji üç kez güncellenebilir (3:44, 3:54, 4:04), ancak kayıp ve gecikme sayısı yaklaşık yedi kez güncellenir (3:44, 3:47, 3:50, 3:53, 3:56, 3:59, 4:02). 3:54'te oluşturulan topoloji, 3:56, 3:59 ve 4:02'de hesaplanan kayıp ve gecikme için işlenecektir. Acil'in devrenin 3:59'da sağlıksız olduğuna dair bir uyarı aldığınızı varsayalım. NPM'de oturum açın ve topoloji süresini 3:59 olarak ayarlamayı deneyin. NPM 3:54'te oluşturulan topolojiyi işleyecek. Ağınızın bilinen son topolojisini anlamak için TimeProcessed (kayıp ve gecikme süresinin hesaplandığı zaman) ve TracerouteCompletedTime (topolojinin hesaplandığı zaman) alanlarını karşılaştırın. 

### <a name="what-is-the-difference-between-the-fields-e2emedianlatency-and-avghoplatencylist-in-the-networkmonitoring-table"></a>NetworkMonitoring tablosundaki E2EMedianLatency ve AvgHopLatencyList alanları arasındaki fark nedir?
E2EMedianLatency, tcp ping testlerinin sonuçlarını bir araya geldikten sonra her üç dakikada bir güncellenen gecikme, AvgHopLatencyList ise izleme yoluna göre her 10 dakikada bir güncellenir. E2EMedianLatency'nin tam olarak ne zaman hesaplandığını anlamak için TimeProcessed alanını kullanın. Traceroute'un tam olarak ne zaman tamamlandığını ve Güncellendiği AvgHopLatencyList alanını anlamak için TracerouteCompletedTime alanını kullanın

### <a name="why-does-hop-by-hop-latency-numbers-differ-from-hoplatencyvalues"></a>Hop-by-hop gecikme numaraları Neden HopLatencyValues'den farklıdır? 
HopLatencyValues uç noktaya kaynak vardır.
Örneğin: Hops - A,B,C. AvgHopLatency - 10,15,20. Bu, A gecikmesi = 10 kaynağı, Kaynak B gecikmesi = 15 ve C gecikme kaynağı 20 anlamına gelir. UI, topolojide A-B atlama gecikmesini 5 olarak hesaplar

### <a name="the-solution-shows-100-loss-but-there-is-connectivity-between-the-source-and-destination"></a>Çözüm % 100 kayıp gösterir, ancak kaynak ve hedef arasında bağlantı vardır
Bu durum, ana bilgisayar güvenlik duvarı veya ara güvenlik duvarı (ağ güvenlik duvarı veya Azure NSG) kaynak aracı ile Bağlantı Noktası üzerindeki hedef arasındaki iletişimi Engelliyorsa(varsayılan olarak bağlantı noktası 8084'se, müşteri bu değişti).

* Ana bilgisayar güvenlik duvarının gerekli bağlantı noktasındaki iletişimi engellemediğini doğrulamak için kaynak ve hedef düğümlerinin sistem durumu durumunu aşağıdaki görünümden görüntüleyin: Ağ Performans İzleyicisi -> Yapılandırma -> Düğümleri. 
  Sağlıksızlarsa, yönergeleri görüntüleyin ve düzeltici eylemde olun. Düğümler sağlıklıysa, b adımına geçin. öğrenin.
* Bir ara ağ güvenlik duvarının veya Azure NSG'nin gerekli bağlantı noktasındaki iletişimi engellemediğini doğrulamak için aşağıdaki yönergeleri kullanarak üçüncü taraf PsPing yardımcı programını kullanın:
  * psping programı [buradan](https://technet.microsoft.com/sysinternals/psping.aspx) indirilebilir 
  * Kaynak düğümünden aşağıdaki komutu çalıştırın.
    * psping -n \<15 hedef\>düğüm IPAddress :portNumber Varsayılan NPM tarafından 8084 bağlantı noktası kullanır. EnableRules.ps1 komut dosyasını kullanarak bunu açıkça değiştirdiyseniz, kullanmakta olduğunuz özel bağlantı noktası numarasını girin). Bu, Azure makinesinden şirket içi bir ping
* Ping'lerin başarılı olup olmadığını kontrol edin. Değilse, bir ara ağ güvenlik duvarı veya Azure NSG bu bağlantı noktasındaki trafiği engellediğini gösterir.
* Şimdi, hedef düğümden kaynak düğümü IP komutunu çalıştırın.


### <a name="there-is-loss-from-node-a-to-b-but-not-from-node-b-to-a-why"></a>A düğümünden B'ye kayıp vardır, ancak B düğümünden A'ya kadar değil. Neden?
A'dan B'ye giden ağ yolları B ile A arasındaki ağ yollarından farklı olabileceğinden, kayıp ve gecikme için farklı değerler gözlemlenebilir.

### <a name="why-are-all-my-expressroute-circuits-and-peering-connections-not-being-discovered"></a>Neden tüm ExpressRoute devrelerim ve bakan bağlantılarım keşfedilmedi?
NPM artık kullanıcının erişesahip olduğu tüm aboneliklerde ExpressRoute devrelerini ve bakan bağlantıları keşfeder. Ekspres Rota kaynaklarınızın bağlı olduğu tüm abonelikleri seçin ve keşfedilen her kaynak için izlemeyi etkinleştirin. NPM, özel bir görünüm keşfederken bağlantı nesnelerini arar, bu nedenle lütfen bir VNET'in sizin bakışınla ilişkili olup olmadığını kontrol edin.

### <a name="the-er-monitor-capability-has-a-diagnostic-message-traffic-is-not-passing-through-any-circuit-what-does-that-mean"></a>ER Monitor özelliğinin "Trafik ANY devresinden geçmiyor" bir tanılama iletisi vardır. Bu ne anlama gelir?

Şirket içi ve Azure düğümleri arasında sağlıklı bir bağlantı olduğu, ancak trafiğin NPM tarafından izlenecek şekilde yapılandırılan ExpressRoute devresi üzerinden gitmiyor sayılabileceği bir senaryo olabilir. 

Bu hata şu durumlarda oluşabilir:

* Er devresi devre sayılsın.
* Rota filtreleri, amaçlanan ExpressRoute devresi üzerinden diğer rotalara (VPN bağlantısı veya başka bir ExpressRoute devresi gibi) öncelik verecek şekilde yapılandırılır. 
* İzleme yapılandırmasında ExpressRoute devresini izlemek için seçilen şirket içi ve Azure düğümleri, amaçlanan ExpressRoute devresi üzerinden birbirleriyle bağlantı sahibi olmaz. İzlemeyi istediğiniz ExpressRoute devresi üzerinden birbirine bağlanan doğru düğümleri seçtiğinizden emin olun.

### <a name="while-configuring-monitoring-of-my-expressroute-circuit-the-azure-nodes-are-not-being-detected"></a>ExpressRoute devremin izlemeyi yapılandırırken, Azure düğümleri algılanmıyor.
Azure düğümleri Operations Manager aracılığıyla bağlanırsa bu durum olabilir. ExpressRoute Monitor özelliği yalnızca Doğrudan Aracıolarak bağlanan Azure düğümlerini destekler.

### <a name="i-cannot-discover-by-expressroute-circuits-in-the-oms-portal"></a>OMS portalında ExpressRoute devreleri ile keşfedemiyorum
NPM hem Azure portalından hem de OMS portalından kullanılabilse de, ExpressRoute Monitor özelliğindeki devre keşfi yalnızca Azure portalı üzerinden çalışır. Devreler Azure portalı üzerinden keşfedildikten sonra, bu özelliği iki portaldan birinde kullanabilirsiniz. 

### <a name="in-the-service-connectivity-monitor-capability-the-service-response-time-network-loss-as-well-as-latency-are-shown-as-na"></a>Service Connectivity Monitor özelliğinde, hizmet yanıt süresi, ağ kaybı ve gecikme süresi NA olarak gösterilir
Bir veya daha fazla doğruysa bu durum olabilir:

* Servis düştü.
* Hizmete ağ bağlantısını denetlemek için kullanılan düğüm indi.
* Test yapılandırmasına girilen hedef yanlıştır.
* Düğümde ağ bağlantısı yok.

### <a name="in-the-service-connectivity-monitor-capability-a-valid-service-response-time-is-shown-but-network-loss-as-well-as-latency-are-shown-as-na"></a>Service Connectivity Monitor özelliğinde geçerli bir hizmet yanıt süresi gösterilir, ancak ağ kaybı nın yanı sıra gecikme süresi NA olarak gösterilir
 Bir veya daha fazla doğruysa bu durum olabilir:

* Hizmete ağ bağlantısını denetlemek için kullanılan düğüm bir Windows istemci salı makinesiyse, hedef hizmet ICMP isteklerini engelliyor veya bir ağ güvenlik duvarı düğümden kaynaklanan ICMP isteklerini engelliyor.
* Ağ ölçülerini gerçekleştir onay kutusu test yapılandırmasında boştur.

### <a name="in-the-service-connectivity-monitor-capability-the-service-response-time-is-na-but-network-loss-as-well-as-latency-are-valid"></a>Service Connectivity Monitor özelliğinde, hizmet yanıt süresi NA'dır, ancak ağ kaybı nın yanı sıra gecikme süresi de geçerlidir
Hedef hizmet bir web uygulaması değilse, ancak test Web testi olarak yapılandırılırsa bu durum olabilir. Test yapılandırmasını düzenleme ve Web yerine Ağ olarak test türünü seçin.

## <a name="miscellaneous"></a>Çeşitli

### <a name="is-there-a-performance-impact-on-the-node-being-used-for-monitoring"></a>İzleme için kullanılan düğüm üzerinde bir performans etkisi var mı?
NPM işlemi, ana bilgisayar CPU kaynaklarının %5'inden fazlasını kullanıyorsa duracak şekilde yapılandırılır. Bu, düğümleri performansı etkilemeden her zamanki iş yükleri için kullanmaya devam edebilmenizi sağlamak içindir.

### <a name="does-npm-edit-firewall-rules-for-monitoring"></a>NPM izleme için güvenlik duvarı kurallarını düzenliyor mu?
NPM yalnızca enablerules.ps1 Powershell komut dosyasının çalıştırıldığı düğümlerde, aracıların belirtilen bağlantı noktasında birbirleriyle TCP bağlantıları oluşturmasına izin vermek için yerel bir Windows Güvenlik Duvarı kuralı oluşturur. Çözüm, herhangi bir ağ güvenlik duvarı veya Ağ Güvenlik Grubu (NSG) kurallarını değiştirmez.

### <a name="how-can-i-check-the-health-of-the-nodes-being-used-for-monitoring"></a>İzleme için kullanılan düğümlerin durumunu nasıl kontrol edebilirim?
Aşağıdaki görünümden izlemek için kullanılan düğümlerin sistem durumu durumunu görüntüleyebilirsiniz: Ağ Performans İzleyicisi -> Yapılandırma -> Düğümleri. Düğüm sağlıksızsa, hata ayrıntılarını görüntüleyebilir ve önerilen eylemi yapabilirsiniz.

### <a name="can-npm-report-latency-numbers-in-microseconds"></a>NPM gecikme numaralarını mikrosaniye cinsinden bildirebilir mi?
NPM, UI'deki gecikme sayılarını milisaniye cinsinden yuvarlar. Aynı veriler daha yüksek bir tanecikli (bazen en fazla dört ondalık basamak) depolanır.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure'daki Ağ Performans İzleme çözümüne](../../azure-monitor/insights/network-performance-monitor.md)atıfta bulunarak Ağ Performansı İzleyicisi hakkında daha fazla bilgi edinin.
