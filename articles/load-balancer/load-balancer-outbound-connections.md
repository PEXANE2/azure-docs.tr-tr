---
title: Azure’da giden bağlantılar
titleSuffix: Azure Load Balancer
description: Bu makalede, Azure'un VM'lerin genel internet hizmetleriyle iletişim kurmasını nasıl sağladığı açıklanmaktadır.
services: load-balancer
documentationcenter: na
author: asudbring
ms.service: load-balancer
ms.custom: seodec18
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/07/2019
ms.author: allensu
ms.openlocfilehash: acf49c4247c8084a3afd3c2046003ee1b20d2f67
ms.sourcegitcommit: d6e4eebf663df8adf8efe07deabdc3586616d1e4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/15/2020
ms.locfileid: "81393100"
---
# <a name="outbound-connections-in-azure"></a>Azure’da giden bağlantılar

Azure, birkaç farklı mekanizma aracılığıyla müşteri dağıtımları için giden bağlantı sağlar. Bu makalede, senaryoların ne olduğu, ne zaman uygulanacağı, nasıl çalıştıkları ve bunların nasıl yönetilenleri açıklanmaktadır.

>[!NOTE] 
>Bu makalede, yalnızca Kaynak Yöneticisi dağıtımları kapsar. Azure'daki tüm Klasik dağıtım senaryoları için [Giden bağlantıları (Klasik)](load-balancer-outbound-connections-classic.md) gözden geçirin.

Azure'daki bir dağıtım, ortak IP adresi alanında Azure dışındaki uç noktalarla iletişim kurabilir. Bir örnek ortak IP adresi alanında bir hedefe giden bir akış başlattığında, Azure özel IP adresini ortak bir IP adresiyle dinamik olarak eşler. Bu eşleme oluşturulduktan sonra, bu giden kaynaklı akış için dönüş trafiği de akışın kaynaklandığı özel IP adresine ulaşabilir.

Azure, bu işlevi gerçekleştirmek için kaynak ağ adresi çevirisi (SNAT) kullanır. Birden çok özel IP adresi tek bir genel IP adresinin arkasına kılık aldığında, Azure özel IP adreslerini maskelemek için [bağlantı noktası adresi çevirisini (PAT)](#pat) kullanır. Kısa ömürlü bağlantı noktaları PAT için kullanılır ve havuz büyüklüğüne göre [önceden tahsis edilir.](#preallocatedports)

Birden çok [giden senaryo](#scenarios)vardır. Gerektiğinde bu senaryoları birleştirebilirsiniz. Dağıtım modelinize ve uygulama senaryonuza uygulanan özellikleri, kısıtlamaları ve desenleri anlamak için bunları dikkatle gözden geçirin. Bu senaryoları yönetmek için kılavuzu gözden [geçirin.](#snatexhaust)

>[!IMPORTANT] 
>Standart Yük Dengeleyicisi ve Standart Genel IP, giden bağlantıya yeni yetenekler ve farklı davranışlar sunar.  Temel SNU'larla aynı değildirler.  Standart SKUs'larla çalışırken giden bağlantı istiyorsanız, standart genel IP adresleri veya Standart genel yük dengeleyicisi ile açıkça tanımlamanız gerekir.  Bu, dahili bir Standart Yük Dengeleyicisi kullanırken giden bağlantı oluşturmayı içerir.  Giden kuralları her zaman Standart genel Yük Dengeleyicisi'nde kullanmanızı öneririz.  [Senaryo 3](#defaultsnat) Standart SKU ile kullanılamaz.  Bu, dahili bir Standart Yük Dengeleyici kullanıldığında, giden bağlantı isteniyorsa arka uç havuzundaki VM'ler için giden bağlantı oluşturmak için adımlar atmanız gerektiği anlamına gelir.  Giden bağlantı bağlamında, tek bir bağımsız VM, tüm VM'ler kullanılabilirlik kümesinde, VMSS'deki tüm örnekler grup olarak yer alır. Bu, bir Kullanılabilirlik Kümesi'ndeki tek bir VM Standart SKU ile ilişkiliyse, bu Kullanılabilirlik Kümesi'ndeki tüm VM örnekleri, tek bir örnek doğrudan ilişkili olmasa bile, Standart SKU ile ilişkili gibi aynı kurallara göre ilişki içinde olur. Bu davranış, yük dengeleyicisine bağlı birden çok ağ arabirimi kartı olan bağımsız bir VM durumunda da gözlenir. Bir NIC tek başına olarak eklenirse, aynı davranışa sahip olacaktır. Genel kavramları anlamak için bu belgenin tamamını dikkatle gözden geçirin, SNU'lar arasındaki farklar için [Standart Yük Dengeleyicisini](load-balancer-standard-overview.md) gözden geçirin ve [giden kuralları](load-balancer-outbound-rules-overview.md)gözden geçirin.  Giden kuralları kullanmak, giden bağlantının tüm yönleri üzerinde ince taneli denetim sağlar.

## <a name="scenario-overview"></a><a name="scenarios"></a>Senaryoya genel bakış

Azure Yük Bakiyesi ve ilgili kaynaklar, [Azure Kaynak Yöneticisi'ni](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview)kullanırken açıkça tanımlanır.  Azure şu anda Azure Kaynak Yöneticisi kaynakları için giden bağlantı sağlamak için üç farklı yöntem sağlar. 

| SKU'lar | Senaryo | Yöntem | IP protokolleri | Açıklama |
| --- | --- | --- | --- | --- |
| Standart, Temel | [1. Örnek Düzeyinde Genel IP adresi ne olacak (Yük Dengeleyicili veya Yük Bakiyesi olmayan) VM](#ilpip) | SNAT, kullanılmayan maskeleme bağlantı noktası | TCP, UDP, ICMP, ESP | Azure, örneğin NIC'sinin IP yapılandırmasına atanan genel IP'yi kullanır. Örnekte tüm geçici bağlantı noktaları vardır. Standart Yük Dengeleyicisi kullanırken, Sanal Makine'ye genel bir IP atanmışsa [giden kurallar](load-balancer-outbound-rules-overview.md) desteklenmez. |
| Standart, Temel | [2. VM ile ilişkili Genel Yük Dengeleyicisi (örnekte Genel IP adresi yok)](#lb) | Yük Dengeleyici ön uçlarını kullanarak bağlantı noktası maskesi (PAT) ile SNAT | TCP, UDP |Azure, ortak Yük Dengeleyicisi ön uçlarının genel IP adresini birden çok özel IP adresiyle paylaşır. Azure, PAT'in ön uçlarının geçici bağlantı noktalarını kullanır. Giden bağlantının açık olarak tanımlanması için [giden kuralları](load-balancer-outbound-rules-overview.md) kullanmalısınız. |
| yok veya Temel | [3. Bağımsız VM (Yük Dengeleyicisi yok, Genel IP adresi yok)](#defaultsnat) | Port maskeli SNAT (PAT) | TCP, UDP | Azure, SNAT için genel bir IP adresi otomatik olarak belirler, bu genel IP adresini kullanılabilirlik kümesinin birden çok özel IP adresiyle paylaşır ve bu genel IP adresinin geçici bağlantı noktalarını kullanır. Bu senaryo, önceki senaryolar için bir geri dönüş. Görünürlüğe ve kontrole ihtiyacınız varsa bunu önermiyoruz. |

Bir VM'nin ortak IP adresi alanında Azure dışındaki uç noktalarla iletişim kurmasını istemiyorsanız, gerektiğinde erişimi engellemek için ağ güvenlik gruplarını (NSGs) kullanabilirsiniz. Giden [bağlantının önlenmesi](#preventoutbound) bölümü NSG'leri daha ayrıntılı olarak tartışır. Herhangi bir giden erişim olmadan bir sanal ağ tasarlama, uygulama ve yönetme kılavuzu bu makalenin kapsamı dışındadır.

### <a name="scenario-1-vm-with-public-ip-address"></a><a name="ilpip"></a>Senaryo 1: Genel IP adresine sahip VM

Bu senaryoda, VM'nin atanmış bir Genel IP'si vardır. Giden bağlantılar söz konusu olduğunda, VM'nin yük dengeli olup olmadığı önemli değildir. Bu senaryo diğerlerinden önce gelir. Genel IP adresi kullanıldığında, VM tüm giden akışlar için Genel IP adresini kullanır.  

VM'ye atanan genel bir IP, 1:1 ilişkisidir (1 yerine: çok) ve devletsiz 1:1 NAT olarak uygulanır.  Bağlantı noktası maskeleme (PAT) kullanılmaz ve VM'de kullanımiçin tüm geçici bağlantı noktaları bulunur.

Uygulamanız çok sayıda giden akış başlatır ve SNAT bağlantı noktası tükenmesi yaşarsanız, [SNAT kısıtlamalarını azaltmak için](#assignilpip)bir Genel IP adresi atamayı düşünün. [Gözden Yönetimi SNAT yorgunluk](#snatexhaust) bütünüyle.

### <a name="scenario-2-load-balanced-vm-without-a-public-ip-address"></a><a name="lb"></a>Senaryo 2: Genel IP adresi olmayan yük dengeli VM

Bu senaryoda, VM ortak Yük Dengeleyici arka uç havuzunun bir parçasıdır. VM'nin atanmış genel bir IP adresi yoktur. Yük Dengeleyici sağlık gütücü başındaki ortak IP frontend arasında bir bağlantı oluşturmak için bir yük dengeleyici kuralı ile yıkılaştırıl

Bu kural yapılandırmasını tamamlamazsanız, [davranış, Ortak IP'si olmayan Bağımsız VM](#defaultsnat)senaryosunda açıklandığı gibi. Bu kural için arka uç havuzunda çalışan bir dinleyici için sağlık sondası başarılı olması için gerekli değildir.

Yük dengeli VM giden bir akış oluşturduğunda, Azure giden akışın özel kaynak IP adresini ortak Yük Dengeleyici ön ucunun genel IP adresine çevirir. Azure, bu işlevi gerçekleştirmek için SNAT kullanır. Azure, ortak bir IP adresinin arkasında birden fazla özel IP adresini maskelemek için [PAT'i](#pat) de kullanır. 

Yük bakiyesi ortak IP adresi ön ucunun geçici bağlantı noktaları, VM'den kaynaklanan tek tek akışları ayırt etmek için kullanılır. SNAT, giden akışlar oluşturulduğunda [önceden ayrılmış geçici bağlantı noktalarını](#preallocatedports) dinamik olarak kullanır. Bu bağlamda, SNAT için kullanılan geçici bağlantı noktaları Nasat bağlantı noktaları olarak adlandırılır.

SNAT bağlantı [noktaları, Anlayış SNAT ve PAT](#snat) bölümünde açıklandığı şekilde önceden tahsis edilir. Onlar tüketilebilen sınırlı bir kaynak. Nasıl [tükettiklerini](#pat)anlamak önemlidir. Bu tüketim için nasıl tasarlayacağımı anlamak ve gerektiğinde azaltmak için [SNAT yorgunluğunu yönetmeyi](#snatexhaust)gözden geçirin.

[Birden çok genel IP adresi Load Balancer Basic ile ilişkilendirildiğinde,](load-balancer-multivip-overview.md)bu ortak IP adreslerinden herhangi biri giden akışlar için bir adaydır ve biri rastgele seçilir.  

Load Balancer Basic ile giden bağlantıların durumunu izlemek [için Yük Dengeleyicisi için Azure Monitor günlüklerini](load-balancer-monitor-log.md) kullanabilir ve SNAT bağlantı noktası tükenme iletilerini izlemek için [olay günlüklerini uyarabilirsiniz.](load-balancer-monitor-log.md#alert-event-log)

### <a name="scenario-3-standalone-vm-without-a-public-ip-address"></a><a name="defaultsnat"></a>Senaryo 3: Genel IP adresi olmayan bağımsız VM

Bu senaryoda, VM ortak yük dengeleyici havuzunun bir parçası değildir (ve dahili bir Standart Yük Dengeleyici havuzunun bir parçası değildir) ve ona atanmış bir Genel IP adresi yoktur. VM giden bir akış oluşturduğunda, Azure giden akışın özel kaynak IP adresini ortak kaynak IP adresine çevirir. Bu giden akış için kullanılan genel IP adresi yapılandırılamaz ve aboneliğin genel IP kaynak sınırına dahil edilmez. Bu genel IP adresi size ait değildir ve rezerve edilemez. VM veya Kullanılabilirlik Kümesini veya sanal makine ölçeği kümesini yeniden dağıtırsanız, bu genel IP adresi serbest bırakılır ve yeni bir genel IP adresi istenir. Bu senaryoyu IP adreslerini beyaz listelemek için kullanmayın. Bunun yerine, giden senaryoyu ve giden bağlantı için kullanılacak genel IP adresini açıkça beyan ettiğiniz diğer iki senaryodan birini kullanın.

>[!IMPORTANT] 
>Bu senaryo, __yalnızca__ bir dahili Temel Yük Dengeleyicisi takıldığında da geçerlidir. Bir VM'ye dahili Standart Yük Dengeleyici takıldığında Senaryo 3 __kullanılamaz.__  Dahili bir Standart Yük Dengeleyicisi kullanmanın yanı sıra [açıkça senaryo 1](#ilpip) veya senaryo [2](#lb) oluşturmanız gerekir.

Azure, bu işlevi gerçekleştirmek için bağlantı noktası maskesi[(PAT)](#pat)içeren SNAT kullanır. Bu senaryo [senaryo 2'ye](#lb)benzer, ancak kullanılan IP adresi üzerinde denetim yoktur. Bu, 1 ve 2 senaryoları olmadığında bir geri dönüş senaryosudur. Giden adres üzerinde denetim istiyorsanız bu senaryoyu önermiyoruz. Giden bağlantılar uygulamanızın önemli bir parçasıysa, başka bir senaryo seçmeniz gerekir.

SNAT bağlantı [noktaları, Anlayış SNAT ve PAT](#snat) bölümünde açıklandığı şekilde önceden tahsis edilmiştir.  Kullanılabilirlik Kümesini paylaşan VM sayısı, hangi ön ayırma katmanının geçerli olduğunu belirler.  Kullanılabilirlik Kümesi olmayan bağımsız bir VM, preallocation 'ı (1024 SNAT bağlantı noktası) belirlemek amacıyla etkin bir şekilde 1'den oluşan bir havuzdur. SNAT bağlantı noktaları tüketilebilen sınırlı bir kaynaktır. Nasıl [tükettiklerini](#pat)anlamak önemlidir. Bu tüketim için nasıl tasarlayacağımı anlamak ve gerektiğinde azaltmak için [SNAT yorgunluğunu yönetmeyi](#snatexhaust)gözden geçirin.

### <a name="multiple-combined-scenarios"></a><a name="combinations"></a>Birden çok, birleştirilmiş senaryolar

Belirli bir sonuca ulaşmak için önceki bölümlerde açıklanan senaryoları birleştirebilirsiniz. Birden çok senaryo varsa, bir öncelik sırası uygulanır: [senaryo 1,](#ilpip) [senaryo 2](#lb) ve [3'ten](#defaultsnat)önce gelir. [Senaryo 2](#lb) [senaryo 3](#defaultsnat)geçersiz kılar.

Buna örnek olarak, uygulamanın büyük ölçüde sınırlı sayıda hedefe giden bağlantılara dayandığı ve aynı zamanda Yük Bakiyesi ön ucundan gelen akışları aldığı bir Azure Kaynak Yöneticisi dağıtımıdır. Bu durumda, kabartma için senaryo1 ve 2'yi birleştirebilirsiniz. Ek desenler için, [SNAT yorgunluğunu yönetmeyi](#snatexhaust)gözden geçirin.

### <a name="multiple-frontends-for-outbound-flows"></a><a name="multife"></a>Giden akışlar için birden çok ön uç

#### <a name="standard-load-balancer"></a>Standart Yük Dengeleyici

Standart Yük Dengeleyici, [birden fazla (ortak) IP ön uç](load-balancer-multivip-overview.md) bulunduğunda, giden akışlar için tüm adayları aynı anda kullanır. Giden bağlantılar için bir yük dengeleme kuralı etkinse, her ön uç, kullanılabilir önceden ayrılmış SNAT bağlantı noktası sayısını çarpar.

Yeni bir yük dengeleme kuralı seçeneğiyle giden bağlantılar için kullanılan bir ön uç IP adresini bastırmayı seçebilirsiniz:

```json    
      "loadBalancingRules": [
        {
          "disableOutboundSnat": false
        }
      ]
```

Normalde, `disableOutboundSnat` seçenek _varsayılan olarak false'a_ işaret eder ve bu kuralın, yük dengeleme kuralının arka uç havuzunda ilişkili VM'ler için giden SNAT'yi programladığını belirtir. `disableOutboundSnat` Yük Dengeleyici'nin bu yük dengeleme kuralının arka uç havuzundaki VM'ler için giden bağlantılar için ilişkili ön uç IP adresini kullanmasını önlemek için _doğru_ olarak değiştirilebilir.  Ayrıca, [Birden Çok birleşik senaryoda](#combinations) da açıklandığı gibi giden akışlar için belirli bir IP adresi belirleyebilirsiniz.

#### <a name="load-balancer-basic"></a>Yük Dengeleyici Temel

Load Balancer Basic, [birden fazla (genel) IP ön uç](load-balancer-multivip-overview.md) giden akışlar için aday olduğunda giden akışlar için kullanılacak tek bir ön uç seçer. Bu seçim yapılandırılabilir değildir ve seçim algoritmasını rasgele olarak düşünmelisiniz. [Birden Çok, birleşik senaryolarda](#combinations)açıklandığı gibi giden akışlar için belirli bir IP adresi atayabilirsiniz.

### <a name="availability-zones"></a><a name="az"></a>Kullanılabilirlik Bölgeleri

[Kullanılabilirlik Bölgeleri ile Standart Yük Dengeleyici](load-balancer-standard-availability-zones.md)serken, bölge yedekli frontends bölge yedekli giden SNAT bağlantıları sağlayabilir ve SNAT programlama bölge hatası hayatta.  Bölge cepheleri kullanıldığında, giden SNAT bağlantıları kaderi ait oldukları bölgeyle paylaşır.

## <a name="understanding-snat-and-pat"></a><a name="snat"></a>SNAT ve PAT'i Anlamak

### <a name="port-masquerading-snat-pat"></a><a name="pat"></a>SNAT maskesi (PAT)

Ortak Yük Dengeleyici kaynağı VM örnekleriyle ilişkilendirildiğinde, her giden bağlantı kaynağı yeniden yazılır. Kaynak, sanal ağ özel IP adresi alanından yük dengeleyicisinin ön uç Public IP adresine yeniden yazılır. Ortak IP adres alanında, akışın 5-tuple (kaynak IP adresi, kaynak bağlantı noktası, IP taşıma protokolü, hedef IP adresi, hedef bağlantı noktası) benzersiz olmalıdır.  SNAT maskesi ni takma noktası TCP veya UDP IP protokolleri ile kullanılabilir.

Kısa ömürlü bağlantı noktaları (SNAT bağlantı noktaları), tek bir ortak IP adresinden birden çok akış kaynağı olduğundan, özel kaynak IP adresini yeniden yazdıktan sonra bunu başarmak için kullanılır. SNAT algoritması kılığına giren bağlantı noktası, SNAT bağlantı noktalarını UDP ile TCP için farklı şekilde ayırır.

#### <a name="tcp-snat-ports"></a><a name="tcp"></a>TCP SNAT Bağlantı Noktaları

Tek bir hedef IP adresine, bağlantı noktasına akış başına bir SNAT bağlantı noktası tüketilir. Aynı hedef IP adresi, bağlantı noktası ve protokole birden çok TCP akışı için, her TCP akışı tek bir SNAT bağlantı noktası tüketir. Bu, akışların aynı genel IP adresinden geldiklerinde ve aynı hedef IP adresine, bağlantı noktasına ve protokole gittiklerinde benzersiz olmasını sağlar. 

Her biri farklı bir hedef IP adresine, bağlantı noktasına ve protokole birden çok akış, tek bir SNAT bağlantı noktasını paylaşır. Hedef IP adresi, bağlantı noktası ve protokol, ortak IP adresi alanında akışları ayırt etmek için ek kaynak bağlantı noktalarına gerek kalmadan akışları benzersiz hale getirin.

#### <a name="udp-snat-ports"></a><a name="udp"></a>UDP SNAT Bağlantı Noktaları

UDP SNAT bağlantı noktaları, TCP SNAT bağlantı noktalarından farklı bir algoritma yla yönetilir.  Yük Dengeleyici udp için "bağlantı noktası kısıtlı koni NAT" olarak bilinen bir algoritma kullanır.  Hedef IP adresine, bağlantı noktasına bakılmaksızın her akış için bir SNAT bağlantı noktası tüketilir.

#### <a name="snat-port-reuse"></a>SNAT bağlantı noktası yeniden

Bir bağlantı noktası serbest bırakıldıktan sonra, bağlantı noktası gerektiği gibi yeniden kullanılabilir.  SNAT bağlantı noktalarını belirli bir senaryo için en düşükten en yükseğe doğru bir dizi olarak düşünebilirsiniz ve yeni bağlantılar için kullanılabilir ilk SNAT bağlantı noktası kullanılır. 
 
#### <a name="exhaustion"></a>Tükenme

SNAT bağlantı noktası kaynakları tükendiğinde, varolan akışlar SNAT bağlantı noktalarını serbest bırakana kadar giden akışlar başarısız olursa. Yük Dengeleyici, akış kapandığında SNAT bağlantı noktalarını geri alır ve SNAT bağlantı noktalarını boşta akışlardan geri almak için [4 dakikalık boş zaman ayarı](#idletimeout) kullanır.

UDP SNAT bağlantı noktaları genellikle kullanılan algoritma farkı nedeniyle TCP SNAT bağlantı noktalarından çok daha hızlı egzoz. Bu farkı göz önünde bulundurarak test tasarlamalı ve ölçeklendirmelisiniz.

Genellikle SNAT bağlantı noktası tükenmesine yol açan koşulları azaltmak için desenler [Için, Yönetici SNAT](#snatexhaust) bölümünü gözden geçirin.

### <a name="ephemeral-port-preallocation-for-port-masquerading-snat-pat"></a><a name="preallocatedports"></a>SNAT (PAT) maskesi ne ştanlamalı bağlantı noktası için geçici bağlantı noktası preallocation

Azure, snat[(PAT)](#pat)maskesi kullanarak arka uç havuzunun boyutuna bağlı olarak kullanılabilir önceden tahsis edilmiş SNAT bağlantı noktası sayısını belirlemek için bir algoritma kullanır. SNAT bağlantı noktaları, belirli bir genel IP kaynak adresi için geçici bağlantı noktalarıdır.

UdP ve TCP için sırasıyla aynı sayıda SNAT bağlantı noktası önceden tahsis edilir ve IP taşıma protokolü başına bağımsız olarak tüketilir.  Ancak, SNAT bağlantı noktası kullanımı, akışın UDP veya TCP olup olmadığına bağlı olarak farklıdır.

>[!IMPORTANT]
>Standart SKU SNAT programlama, IP taşıma protokolüne göredir ve yük dengeleme kuralından türetilmiştir.  Yalnızca bir TCP yük dengeleme kuralı varsa, SNAT yalnızca TCP için kullanılabilir. Yalnızca bir TCP yük dengeleme kuralınız varsa ve UDP için giden SNAT'ye ihtiyacınız varsa, aynı ön uçtan aynı arka uç havuzuna udp yük dengeleme kuralı oluşturun.  Bu UDP için SNAT programlama tetikler.  Bir çalışma kuralı veya sağlık sondası gerekli değildir.  Temel SKU SNAT, yük dengeleme kuralında belirtilen taşıma protokolüne bakılmaksızın, her iki IP taşıma protokolü için her zaman SNAT programları sunar.

Azure, SNAT bağlantı noktalarını her VM'nin NIC yapılandırmasına önceden ayırır. Havuza bir IP yapılandırması eklendiğinde, SNAT bağlantı noktaları arka uç havuzu boyutuna bağlı olarak bu IP yapılandırması için önceden tahsis edilir. Giden akışlar oluşturulduğunda, [PAT](#pat) dinamik olarak tüketir (önceden ayrılan sınıra kadar) ve akış kapandığında veya [boşta zaman ayırmalar](#idletimeout) olduğunda bu bağlantı noktalarını serbest bırakır.

Aşağıdaki tablo, arka uç havuz boyutları katmanları için SNAT bağlantı noktası preallocations gösterir:

| Havuz boyutu (VM örnekleri) | IP yapılandırması başına önceden tahsis edilmiş SNAT bağlantı noktaları|
| --- | --- |
| 1-50 | 1,024 |
| 51-100 | 512 |
| 101-200 | 256 |
| 201-400 | 128 |
| 401-800 | 64 |
| 801-1,000 | 32 |

>[!NOTE]
> [Birden fazla ön uça](load-balancer-multivip-overview.md)sahip Standart Yük Dengeleyicisi kullanırken, her ön uç IP adresi önceki tablodaki kullanılabilir SNAT bağlantı noktası sayısını çarpar. Örneğin, her biri ayrı bir ön uç IP adresine sahip 2 yük dengeleme kuralına sahip 50 VM'lik bir arka uç havuzu, kural başına 2048 (2x 1024) SNAT bağlantı noktası kullanır. Birden çok ön uç için [ayrıntılara](#multife)bakın.

Kullanılabilir SNAT bağlantı noktası sayısının doğrudan akış sayısına çevrilmediğini unutmayın. Tek bir SNAT bağlantı noktası, birden çok benzersiz hedef için yeniden kullanılabilir. Bağlantı noktaları yalnızca akışları benzersiz hale getirmek için gerekli olduğunda tüketilir. Tasarım ve azaltma kılavuzu için, bu [tükenmez kaynağın nasıl yönetilenhakkında](#snatexhaust) bölüme ve [PAT'i](#pat)açıklayan bölüme bakın.

Arka uç havuzunuzun boyutunu değiştirmek, yerleşik akışlarınızın bazılarını etkileyebilir. Arka uç havuzu boyutu artar ve bir sonraki katmana geçiş lerse, önceden tahsis edilmiş SNAT bağlantı noktalarınızın yarısı sonraki büyük arka uç havuz katmanına geçiş sırasında geri alınır. Geri alınan bir SNAT bağlantı noktasıyla ilişkili akışlar zaman dolacak ve yeniden kurulması gerekir. Yeni bir akış denenirse, önceden tahsis edilmiş bağlantı noktaları kullanılabilir olduğu sürece akış hemen başarılı olur.

Arka uç havuzu boyutu azalır ve daha düşük bir katmana geçiş yaparsa, kullanılabilir SNAT bağlantı noktası sayısı artar. Bu durumda, varolan ayrılan SNAT bağlantı noktaları ve ilgili akışları etkilenmez.

SNAT bağlantı noktaları ayırmaları IP taşıma protokolüne özgüdür (TCP ve UDP ayrı olarak korunur) ve aşağıdaki koşullar altında serbest bırakılır:

### <a name="tcp-snat-port-release"></a>TCP SNAT bağlantı noktası sürümü

- Sunucu/istemcilerden biri FINACK gönderirse, SNAT bağlantı noktası 240 saniye sonra serbest bırakılır.
- Bir RST görülürse, SNAT bağlantı noktası 15 saniye sonra serbest bırakılır.
- Boşta zaman adedine ulaşıldıysa, bağlantı noktası serbest bırakılır.

### <a name="udp-snat-port-release"></a>UDP SNAT bağlantı noktası sürümü

- Boşta zaman adedine ulaşıldıysa, bağlantı noktası serbest bırakılır.

## <a name="problem-solving"></a><a name="problemsolving"></a>Problem çözme 

Bu bölüm, SNAT yorgunluğunu azaltmaya yardımcı olmak için tasarlanmıştır ve Azure'daki giden bağlantılarda oluşabilir.

### <a name="managing-snat-pat-port-exhaustion"></a><a name="snatexhaust"></a>SNAT (PAT) bağlantı noktası tükenmesi yönetme
[PAT](#pat) için kullanılan [geçici bağlantı noktaları,](#preallocatedports) [Genel IP adresi olmayan Bağımsız VM'de](#defaultsnat) ve Ortak IP adresi olmayan Yük dengeli VM'de açıklandığı gibi tükenmez [bir kaynaktır.](#lb) Bu [kılavuzu](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-diagnostics#how-do-i-check-my-snat-port-usage-and-allocation) kullanarak SNAT eksünü riskini belirlemek veya onaylamak için geçici bağlantı noktaları kullanımınızı izleyebilir ve geçerli tahsisatınızla karşılaştırabilirsiniz.

Aynı hedef IP adresine ve bağlantı noktasına giden birçok TCP veya UDP bağlantısı başlattığınızı biliyorsanız ve giden bağlantıların başarısız olduğunu gözlemliyorsanız veya destek tarafından SNAT bağlantı noktalarını [(PAT](#pat)tarafından kullanılan önceden ayrılmış [geçici bağlantı noktaları)](#preallocatedports) tükettiğiniz idika tavsiye ediliyorsa, birkaç genel azaltma seçeneğiniz vardır. Bu seçenekleri gözden geçirin ve senaryonuz için neyin uygun ve en iyi olduğuna karar verin. Bir veya daha fazla kişi bu senaryoyu yönetmenize yardımcı olabilir.

Giden bağlantı davranışını anlamakta güçlük yaşıyorsanız, IP yığını istatistiklerini (netstat) kullanabilirsiniz. Veya paket yakalamaları kullanarak bağlantı davranışlarını gözlemlemek yararlı olabilir. Bu paket yakalamalarını örneğinizin konuk işletim sistemi içinde gerçekleştirebilir veya [paket yakalama için Ağ İzleyicisi'ni](../network-watcher/network-watcher-packet-capture-manage-portal.md)kullanabilirsiniz. 

#### <a name="modify-the-application-to-reuse-connections"></a><a name="connectionreuse"></a>Bağlantıları yeniden kullanmak için uygulamayı değiştirme 
Uygulamanızdaki bağlantıları yeniden kullanarak SNAT için kullanılan geçici bağlantı noktalarına olan talebi azaltabilirsiniz. Bu, özellikle bağlantı nın yeniden kullanılmasının varsayılan olduğu HTTP/1.1 gibi protokoller için geçerlidir. Ve kendi aktarım (örneğin, REST) olarak HTTP kullanan diğer protokoller sırayla yararlanabilir. 

Yeniden kullanım her istek için her zaman bireysel, atomik TCP bağlantılarından daha iyidir. Sonuçları daha performant, çok verimli TCP işlemleriyle yeniden kullanın.

#### <a name="modify-the-application-to-use-connection-pooling"></a><a name="connection pooling"></a>Bağlantı havuzunu kullanmak için uygulamayı değiştirme
Uygulamanızda, isteklerin sabit bir bağlantı kümesine dahili olarak dağıtıldığı (her biri mümkünse yeniden kullanma) bir bağlantı birleştirme şeması kullanabilirsiniz. Bu şema, kullanımdaki geçici bağlantı noktası sayısını kısıtlar ve daha öngörülebilir bir ortam oluşturur. Bu şema, tek bir bağlantı bir işlemin yanıtını engelliyorsa, birden çok eşzamanlı işlem eve izin vererek isteklerin iş bilgisini de artırabilir.  

Bağlantı havuzu, uygulamanızı veya uygulamanızın yapılandırma ayarlarını geliştirmek için kullandığınız çerçeve içinde zaten var olabilir. Bağlantı havuzu ile bağlantı yeniden birleştirme yi birleştirebilirsiniz. Birden çok isteğiniz daha sonra aynı hedef IP adresi ve bağlantı noktasına sabit ve öngörülebilir sayıda bağlantı noktası tüketir. İstekler, gecikme süresini ve kaynak kullanımını azaltan TCP işlemlerinin verimli kullanımından da yararlanır. UDP akışlarının sayısını yönetmek egzoz koşullarını önleyebildiği ve SNAT bağlantı noktası kullanımını yönetebileceğinden UDP işlemleri de yararlanabilir.

#### <a name="modify-the-application-to-use-less-aggressive-retry-logic"></a><a name="retry logic"></a>Daha az agresif yeniden deneme mantığını kullanmak için uygulamayı değiştirin
[PAT](#pat) için kullanılan [önceden tahsis edilmiş geçici bağlantı noktaları](#preallocatedports) tükendiğinde veya uygulama hataları oluştuğunda, çürüme ve geri tepme mantığı olmadan agresif veya kaba kuvvet yeniden denemeleri tükenmenin oluşmasına veya devam ına neden olur. Daha az agresif yeniden deneme mantığı kullanarak geçici bağlantı noktalarına olan talebi azaltabilirsiniz. 

Geçici bağlantı noktaları4 dakikalık boşta zaman ayarı vardır (ayarlanabilir değil). Eğer tekrar denemeler çok agresifse, yorgunluk kendi kendine açıklığa kavuşturulmak için bir fırsat alamazsa. Bu nedenle, uygulamanızın hareketleri nasıl ve ne sıklıkta yeniden denedir, tasarımın önemli bir parçasıdır.

#### <a name="assign-a-public-ip-to-each-vm"></a><a name="assignilpip"></a>Her VM'ye genel IP atama
Genel IP adresi atamak, senaryonuzu Genel IP ile [VM](#ilpip)olarak değiştirir. Her VM için kullanılan genel IP'nin tüm geçici bağlantı noktaları VM'de kullanılabilir. (Genel BIR IP'nin geçici bağlantı noktalarının ilgili arka uç havuzuyla ilişkili tüm VM'lerle paylaşıldığı senaryoların aksine.) Genel IP adreslerinin ek maliyeti ve çok sayıda bireysel IP adresini beyaz listeye almanın olası etkisi gibi göz önünde bulundurulması gereken dengeler vardır.

>[!NOTE] 
>Bu seçenek web çalışanı rolleri için kullanılamaz.

#### <a name="use-multiple-frontends"></a><a name="multifesnat"></a>Birden çok frontends kullanma

Ortak Standart Yük Dengeleyicisini kullanırken, [giden bağlantılar için birden çok frontend IP adresi](#multife) atar ve kullanılabilir [SNAT bağlantı noktası sayısını çarparsınız.](#preallocatedports)  SNAT programlamasını ön uçtaki genel IP'ye tetiklemek için bir ön uç IP yapılandırması, kuralı ve arka uç havuzu oluşturun.  Kuralın işlemesi gerekmez ve bir sağlık sondasının başarılı olması gerekmez.  Gelen için de birden çok ön uç kullanıyorsanız (sadece giden ler için değil), güvenilirliği sağlamak için özel sistem sondalarını iyi kullanmalısınız.

>[!NOTE]
>Çoğu durumda, SNAT bağlantı noktalarının tükenmesi kötü tasarımın bir işaretidir.  SNAT bağlantı noktaları eklemek için daha fazla ön uç kullanmadan önce bağlantı noktalarını neden tükettiğinizi anladığınızdan emin olun.  Daha sonra başarısızlığa yol açabilecek bir sorunu gontluyor olabilirsiniz.

#### <a name="scale-out"></a><a name="scaleout"></a>Ölçeği genişletme

[Önceden tahsis edilmiş bağlantı noktaları](#preallocatedports) arka uç havuz boyutuna göre atanır ve bazı bağlantı noktalarının bir sonraki büyük arka uç havuz boyutu katmanını karşılamak için yeniden tahsis edilmesi gerektiğinde kesintiyi en aza indirmek için katmanlar halinde gruplandırılır.  Arka uç havuzunuzu belirli bir katman için maksimum boyuta ölçeklendirerek belirli bir ön uç için SNAT bağlantı noktası kullanımının yoğunluğunu artırma seçeneğiniz olabilir.  Bu, uygulamanın verimli bir şekilde ölçeklendirmesini gerektirir.

Örneğin, arka uç havuzundaki iki sanal makinede IP yapılandırması başına 1024 SNAT bağlantı noktası kullanılabilir ve dağıtım için toplam 2048 SNAT bağlantı noktası vardır.  Dağıtım 50 sanal makineye çıkarılacakolsaydı, önceden tahsis edilmiş bağlantı noktası sayısı sanal makine başına sabit kalsa bile, dağıtım tarafından toplam 51.200 (50 x 1024) SNAT bağlantı noktası kullanılabilir.  Dağıtımınızı ölçeklendirmek istiyorsanız, ölçeğinizi ilgili katman için maksimum adede göre şekillendirdiğinizden emin olmak için katman başına [önceden tahsis edilmiş bağlantı noktası](#preallocatedports) sayısını kontrol edin.  Önceki örnekte, 50 örnek yerine 51'e ölçeklendirmeyi seçmiş olsaydınız, bir sonraki katmana ilerleyecek ve Toplam da VM başına daha az SNAT bağlantı noktası yla sonuçlanır.

Bir sonraki büyük arka uç havuz boyutu katmanına ölçeklendirirseniz, ayrılan bağlantı noktalarının yeniden tahsis edilmesi gerekiyorsa, giden bağlantılarınızın bazılarının zaman sonu için potansiyel vardır.  Yalnızca Bazı SNAT bağlantı noktalarınızı kullanıyorsanız, bir sonraki büyük arka uç havuzu boyutu boyunca ölçekleme önemsizdir.  Bir sonraki arka uç havuz katmanına her geçişinizde varolan bağlantı noktalarının yarısı yeniden tahsis edilir.  Bunun gerçekleşmesini istemiyorsanız, dağıtımınızı katman boyutuna göre şekillendirmeniz gerekir.  Veya uygulamanızın gerektiğinde algılayıp yeniden denediğinden emin olun.  TCP keepalives, Yeniden tahsis edildiği için SNAT bağlantı noktalarının artık çalışmadığını algılamaya yardımcı olabilir.

### <a name="use-keepalives-to-reset-the-outbound-idle-timeout"></a><a name="idletimeout"></a>Giden boşta zaman azamanı sıfırlamak için keepalives kullanın

Giden bağlantıların 4 dakikalık boş zaman ayarı vardır. Bu zaman ayarı [Giden kurallar](../load-balancer/load-balancer-outbound-rules-overview.md#idletimeout)la ayarlanabilir. Ayrıca, boşta kalan akışı yenilemek ve gerekirse bu boşta zaman ayarı sıfırlamak için aktarım (örneğin, TCP keepalives) veya uygulama katmanı keepalives kullanabilirsiniz.  

TCP keepalives kullanırken, bağlantının bir tarafında bunları etkinleştirmek için yeterlidir. Örneğin, bunları sunucu tarafında yalnızca akışın boşta zamanlayıcısını sıfırlamak için etkinleştirmek yeterlidir ve her iki tarafın da TCP keepalives'ı başlatması gerekmez.  Veritabanı istemci-sunucu yapılandırmaları da dahil olmak üzere uygulama katmanı için benzer kavramlar vardır.  Uygulamaya özel keepalives için hangi seçeneklerin var olduğunu sunucu tarafını denetleyin.

## <a name="discovering-the-public-ip-that-a-vm-uses"></a><a name="discoveroutbound"></a>Bir VM'nin kullandığı genel IP'yi keşfetme
Giden bir bağlantının ortak kaynak IP adresini belirlemenin birçok yolu vardır. OpenDNS, VM'nizin genel IP adresini gösterebilecek bir hizmet sağlar. 

nslookup komutunu kullanarak, OpenDNS çözümleyicisine myip.opendns.com ad için bir DNS sorgusu gönderebilirsiniz. Hizmet, sorguyu göndermek için kullanılan kaynak IP adresini döndürür. VM'nizden aşağıdaki sorguyu çalıştırdığınızda, yanıt, bu VM için kullanılan genel IP'dir:

    nslookup myip.opendns.com resolver1.opendns.com

## <a name="preventing-outbound-connectivity"></a><a name="preventoutbound"></a>Giden bağlantıyı önleme
Bazen bir VM'nin giden bir akış oluşturmasına izin verilmesi istenmeyen bir şey dir. Veya giden akışlarla hangi hedeflere ulaşılabileceğini veya hangi hedeflerin gelen akışlara başlayabileceğini yönetme zorunluluğu olabilir. Bu durumda, VM'nin ulaşabileceği hedefleri yönetmek için [ağ güvenlik gruplarını](../virtual-network/security-overview.md) kullanabilirsiniz. Ayrıca, hangi ortak hedefin gelen akışları başlatabileceğini yönetmek için NSG'leri de kullanabilirsiniz.

Yük dengeli vm'ye NSG uyguladığınız zaman, [hizmet etiketlerine](../virtual-network/security-overview.md#service-tags) ve [varsayılan güvenlik kurallarına](../virtual-network/security-overview.md#default-security-rules)dikkat edin. VM'nin Azure Yük Dengeleyicisi'nden sistem durumu sondası isteklerini alabileceğinden emin olmalısınız. 

Bir NSG, AZURE_LOADBALANCER varsayılan etiketinden sistem durumu sondası isteklerini engellerse, VM sistem durumu sondanız başarısız olur ve VM işaretlenir. Yük Dengeleyicisi bu VM'ye yeni akışlar göndermeyi durdurur.

## <a name="limitations"></a>Sınırlamalar
- VNet ve diğer Microsoft platform hizmetleri olmayan Web İşçisi Rolleri'ne, VNet öncesi hizmetlerin ve diğer platform hizmetlerinin nasıl çalıştığına bağlı olarak yalnızca dahili bir Standart Yük Dengeleyicisi kullanıldığında erişilebilir. İlgili hizmetin kendisi veya altta yatan platform önceden haber vermeden değişebileceğinden bu yan etkiye güvenmeyin. Yalnızca dahili bir Standart Yük Dengeleyicisi kullanırken istenirse, her zaman giden bağlantı oluşturmanız gerektiğini varsaymalısınız. Bu makalede açıklanan [varsayılan SNAT](#defaultsnat) senaryo 3 kullanılamıyor.

## <a name="next-steps"></a>Sonraki adımlar

- [Standart Yük Dengeleyici](load-balancer-standard-overview.md) hakkında daha fazla bilgi edinin.
- Standart genel Yük Dengeleyicisi için [giden kurallar](load-balancer-outbound-rules-overview.md) hakkında daha fazla bilgi edinin.
- [Yük Dengeleyicisi](load-balancer-overview.md)hakkında daha fazla bilgi edinin.
- [Ağ güvenlik grupları](../virtual-network/security-overview.md)hakkında daha fazla bilgi edinin.
- Azure'daki diğer önemli [ağ özelliklerinden](../networking/networking-overview.md) bazıları hakkında bilgi edinin.
