---
title: Azure 'da giden bağlantılar (klasik)
titlesuffix: Azure Load Balancer
description: Bu makalede, Azure 'un bulut hizmetleri 'nin genel İnternet hizmetleriyle iletişim kurmasına nasıl izin aldığı açıklanmaktadır.
services: load-balancer
documentationcenter: na
author: asudbring
ms.service: load-balancer
ms.custom: seodec18
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/13/2018
ms.author: allensu
ms.openlocfilehash: 10af3b4838aae1565bac1d996997c117a74cedbc
ms.sourcegitcommit: 9a699d7408023d3736961745c753ca3cec708f23
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/16/2019
ms.locfileid: "68274659"
---
# <a name="outbound-connections-classic"></a>Giden bağlantılar (klasik)

Azure birçok farklı mekanizma aracılığıyla müşteri dağıtımları için giden bağlantı sağlar. Bu makalede senaryoların ne olduğu, ne zaman uygulandığı, nasıl çalıştığı ve nasıl yönetileceği açıklanmaktadır.

>[!NOTE]
>Bu makalede yalnızca klasik dağıtımlar ele alınmaktadır.  Azure 'daki tüm Kaynak Yöneticisi dağıtım senaryoları için [giden bağlantıları](load-balancer-outbound-connections.md) gözden geçirin.

Azure 'daki bir dağıtım, genel IP adresi alanında Azure dışındaki uç noktalarla iletişim kurabilir. Bir örnek, genel IP adresi alanındaki bir hedefe giden akışı başlattığında, Azure özel IP adresini dinamik olarak genel bir IP adresiyle eşleştirir. Bu eşleme oluşturulduktan sonra, bu çıkış kaynaklı akış için geri dönüş trafiği akışın kaynaklandığı özel IP adresine de ulaşabilir.

Azure, bu işlevi gerçekleştirmek için kaynak ağ adresi çevirisini (SNAT) kullanır. Birden çok özel IP adresi tek bir genel IP adresinin arkasında olduğunda, Azure özel IP adreslerine maske eklemek için [bağlantı noktası adres çevirisini (Pat)](#pat) kullanır. Kısa ömürlü bağlantı noktaları, PAT için kullanılır ve havuz boyutuna bağlı olarak [önceden ayrılır](#preallocatedports) .

Birden çok [giden senaryo](#scenarios)vardır. Bu senaryoları gerektiğinde birleştirebilirsiniz. Özellikleri, kısıtlamaları ve desenleri, dağıtım modeliniz ve Uygulama senaryonuz için uygun olarak anlamak üzere dikkatlice gözden geçirin. [Bu senaryoları yönetme](#snatexhaust)kılavuzunu gözden geçirin.

## <a name="scenarios"></a>Senaryoya genel bakış

Azure, giden bağlantıların klasik dağıtımlarını elde etmek için üç farklı yöntem sağlar.  Klasik dağıtımların tümünde kullanabileceğiniz üç senaryo vardır:

| Senaryo | Yöntem | IP protokolleri | Açıklama | Web çalışanı rolü | IaaS | 
| --- | --- | --- | --- | --- | --- |
| [1. Örnek düzeyi genel IP adresine sahip VM](#ilpip) | SNAT, bağlantı noktası aşağı olarak kullanılmıyor | TCP, UDP, ICMP, ESP | Azure, genel IP atanmış sanal makineyi kullanır. Örnekte, tüm kısa ömürlü bağlantı noktaları kullanılabilir. | Hayır | Evet |
| [2. genel yük dengeli uç nokta](#publiclbendpoint) | Genel uç noktaya bağlantı noktası (PAT) ile SNAT | TCP, UDP | Azure, genel IP adresi genel uç noktasını birden çok özel uç noktayla paylaşır. Azure, PAT için genel uç noktanın kısa ömürlü bağlantı noktalarını kullanır. | Evet | Evet |
| [3. Tek başına VM](#defaultsnat) | Bağlantı noktası geçici olarak SNAT (PAT) | TCP, UDP | Azure, SNAT için bir genel IP adresi otomatik olarak atar, bu genel IP adresini dağıtımın tamamına paylaşır ve PAT için genel uç nokta IP adresinin kısa ömürlü bağlantı noktalarını kullanır. Bu, önceki senaryolar için bir geri dönüş senaryosudur. Görünürlük ve denetime ihtiyacınız varsa bunu önermiyoruz. | Evet | Evet |

Bu, Azure 'da Kaynak Yöneticisi dağıtımlar için kullanılabilen giden bağlantı işlevselliğinin bir alt kümesidir.  

Klasik sürümündeki farklı dağıtımlar farklı işlevlere sahiptir:

| Klasik dağıtım | Kullanılabilir işlevler | 
| --- | --- |
| Sanal makine | Senaryo [1](#ilpip), [2](#publiclbendpoint)veya [3](#defaultsnat) |
| Web çalışanı rolü | yalnızca senaryo [2](#publiclbendpoint), [3](#defaultsnat) | 

[Risk azaltma stratejileri](#snatexhaust) de aynı farklılıklara sahiptir.

Klasik dağıtımlar için PAT için kısa ömürlü bağlantı noktaları ön tahsisi için kullanılan algoritma Azure Resource Manager kaynak dağıtımları ile aynıdır.

### <a name="ilpip"></a>Senaryo 1: Örnek düzeyi genel IP adresine sahip VM

Bu senaryoda, VM 'ye atanan bir örnek düzeyi genel IP (ıLPıP) vardır. Giden bağlantılar söz konusu olduğunda, VM 'nin yük dengeli bir uç noktaya sahip olup olmadığı önemi yoktur. Bu senaryo diğerlerine göre önceliklidir. ILPıP kullanıldığında, VM tüm giden akışlar için ıLPı kullanır.  

Bir VM 'ye atanan genel IP, 1:1 ilişkidir (1: çok) ve durum bilgisiz 1:1 NAT olarak uygulanır.  Bağlantı noktası (PAT) kullanılmaz ve VM 'nin kullanılabilir tüm kısa ömürlü bağlantı noktaları kullanılabilir.

Uygulamanız birçok giden akışı başlatırsa ve SNAT bağlantı noktası tükenmesi ile karşılaşırsanız, [SNAT kısıtlamalarını azaltmak için bir ılpıp](#assignilpip)atamasını düşünün. [SNAT tükenmesi yönetimini](#snatexhaust) tamamen inceleyin.

### <a name="publiclbendpoint"></a>Senaryo 2: Ortak yük dengeli uç nokta

Bu senaryoda, VM veya Web çalışanı rolü yük dengeli uç nokta aracılığıyla genel bir IP adresi ile ilişkilendirilir. SANAL makineye atanmış bir genel IP adresi yok. 

Yük dengeli VM bir giden akış oluşturduğunda, Azure giden akışın özel kaynak IP adresini ortak yük dengeli uç noktanın genel IP adresine çevirir. Azure, bu işlevi gerçekleştirmek için SNAT 'yi kullanır. Azure, genel bir IP adresinin arkasında birden çok özel IP adresini geçici olarak çözmek için [Pat](#pat) 'yi de kullanır. 

Yük dengeleyicinin genel IP adresi ön ucu için kısa ömürlü bağlantı noktaları, VM 'nin kaynaklandığı bireysel akışları ayırt etmek için kullanılır. SNAT, giden akışlar oluşturulduğunda, [önceden ayrılmış kısa ömürlü bağlantı noktalarını](#preallocatedports) dinamik olarak kullanır. Bu bağlamda, SNAT için kullanılan kısa ömürlü bağlantı noktaları SNAT bağlantı noktaları olarak adlandırılır.

SNAT bağlantı noktaları, [SNAT ve Pat 'ı anlama](#snat) bölümünde açıklandığı gibi önceden ayrılır. Bu, tükenebilir sınırlı bir kaynaktır. Bunların nasıl kullanıldığını anlamak [önemlidir.](#pat) Bu tüketim için nasıl tasarlanacağını ve gerekirse etkisini anlamak için, [SNAT tükenmesi yönetimini](#snatexhaust)gözden geçirin.

[Birden çok ortak yük dengeli uç nokta](load-balancer-multivip.md) olduğunda, bu genel IP adreslerinden herhangi biri giden akışlar için aday olur ve rastgele bir seçilir.  

### <a name="defaultsnat"></a>Senaryo 3: İlişkili genel IP adresi yok

Bu senaryoda, VM veya Web çalışanı rolü ortak yük dengeli bir uç noktanın parçası değildir.  VM 'nin söz konusu olduğunda, kendisine atanmış bir ıLPıP adresi yoktur. VM bir giden akış oluşturduğunda, Azure giden akışın özel kaynak IP adresini ortak kaynak IP adresine çevirir. Bu giden akış için kullanılan genel IP adresi yapılandırılamaz ve aboneliğin genel IP kaynak sınırına göre sayılmaz.  Azure bu adresi otomatik olarak ayırır.

Azure, bu işlevi gerçekleştirmek için bağlantı noktası geçici ([Pat](#pat)) ile SNAT 'yi kullanır. Bu senaryo, kullanılan IP adresi üzerinde denetim olmaması dışında Senaryo 2 ' ye benzer. Bu senaryo 1 ve 2 ' nin bulunmadığı durumlarda için bir geri dönüş senaryosudur. Giden adres üzerinde denetim istiyorsanız bu senaryoyu önermiyoruz. Giden bağlantılar uygulamanızın önemli bir parçasıysa, başka bir senaryo seçmeniz gerekir.

SNAT bağlantı noktaları, [SNAT ve Pat 'ı anlama](#snat) bölümünde açıklandığı gibi önceden ayrılır.  Genel IP adresini paylaşan VM 'lerin veya Web çalışanı rollerinin sayısı, önceden ayrılan kısa ömürlü bağlantı noktalarının sayısını belirler.   Bunların nasıl kullanıldığını anlamak [önemlidir.](#pat) Bu tüketim için nasıl tasarlanacağını ve gerekirse etkisini anlamak için, [SNAT tükenmesi yönetimini](#snatexhaust)gözden geçirin.

## <a name="snat"></a>SNAT ve PAT 'yi anlama

### <a name="pat"></a>Bağlantı noktası, SNAT (PAT)

Dağıtım giden bir bağlantı yaptığında, her giden bağlantı kaynağı yeniden yazılır. Kaynak, özel IP adres alanından dağıtımla ilişkili genel IP 'ye (yukarıda açıklanan senaryolara göre) yeniden yazılır. Genel IP adresi alanında, akışın 5 demet (kaynak IP adresi, kaynak bağlantı noktası, IP Aktarım Protokolü, hedef IP adresi, hedef bağlantı noktası) benzersiz olmalıdır.  

Çok sayıda akış tek bir genel IP adresinden kaynaklandığından, bu, özel kaynak IP adresini yeniden yazdıktan sonra bunu gerçekleştirmek için kısa ömürlü bağlantı noktaları (SNAT bağlantı noktaları) kullanılır. 

Tek bir hedef IP adresi, bağlantı noktası ve protokole akış başına bir adet SNAT bağlantı noktası kullanılır. Aynı hedef IP adresi, bağlantı noktası ve protokolüne birden çok akış için, her akış tek bir SNAT bağlantı noktası kullanır. Bu, akışların aynı ortak IP adresinden kaynaklandıklarında benzersiz olmasını sağlar ve aynı hedef IP adresine, bağlantı noktasına ve protokolüne gider. 

Her biri farklı bir hedef IP adresi, bağlantı noktası ve protokole birden çok akış, tek bir SNAT bağlantı noktasını paylaşır. Hedef IP adresi, bağlantı noktası ve protokol, genel IP adres alanındaki akışları ayırt etmek için ek kaynak bağlantı noktalarına gerek olmadan akış yapar.

SNAT bağlantı noktası kaynakları tükendiğinde, mevcut akışlar SNAT bağlantı noktalarını yayınlana kadar giden akışlar başarısız olur. Flow kapandığında geri kazanır SNAT bağlantı noktalarını Load Balancer ve boştaki akışlardan geri kazanma SNAT bağlantı noktaları için [4 dakikalık bir boşta kalma zaman aşımı süresi](#idletimeout) kullanır.

Genellikle SNAT bağlantı noktası tükenmesi ' ne yol açabilecek koşulları azaltmak için, SNAT 'yi [yönetme](#snatexhaust) bölümünü gözden geçirin.

### <a name="preallocatedports"></a>Bağlantı noktası için geçici bağlantı noktası önayırması (PAT)

Azure, bağlantı noktası geçici olarak ([Pat](#pat)) kullanılırken arka uç havuzunun boyutuna bağlı olarak kullanılabilir önceden ayrılmış SNAT bağlantı noktalarının sayısını belirlemede bir algoritma kullanır. SNAT bağlantı noktaları, belirli bir genel IP kaynak adresi için kullanılabilir kısa ömürlü bağlantı noktalarıdır.

Azure, belirli bir genel IP adresini kaç tane VM veya Web çalışanı rol örneği paylaştığına göre dağıtıldığında bir örnek dağıtıldığında, SNAT bağlantı noktalarını önceden ayırır.  Giden akışlar oluşturulduğunda, [Pat](#pat) dinamik olarak (önceden ayrılan sınıra kadar) kullanır ve akış kapandığında veya boşta kalma zaman aşımları olduğunda bu bağlantı noktalarını yayınlar.

Aşağıdaki tabloda, arka uç havuz boyutlarının katmanları için SNAT bağlantı noktası ön ayırmaları gösterilmektedir:

| Örnekler | Örnek başına önceden ayrılan SNAT bağlantı noktaları |
| --- | --- |
| 1-50 | 1,024 |
| 51-100 | 512 |
| 101-200 | 256 |
| 201-400 | 128 |

Kullanılabilir SNAT bağlantı noktası sayısının, akış sayısına doğrudan çevrilemez olduğunu unutmayın. Tek bir SNAT bağlantı noktası, birden çok benzersiz hedef için yeniden kullanılabilir. Bağlantı noktaları yalnızca akışları benzersiz hale getirmek için gerekliyse kullanılır. Tasarım ve azaltma Kılavuzu için, [Bu tüketilme kaynağını yönetme](#snatexhaust) ve [Pat](#pat)'yi açıklayan bölüm hakkında bölümüne bakın.

Dağıtımınızın boyutunu değiştirmek, sağlanan akışlarınızdan bazılarını etkileyebilir. Arka uç havuzu boyutu bir sonraki katmana artıyorsa ve geçiş yaptığında, önceden ayrılan SNAT bağlantı noktalarınızın yarısı sonraki daha büyük arka uç havuz katmanına geçiş sırasında geri kazanılır. Geri kazanılan bir SNAT bağlantı noktasıyla ilişkili akışlar zaman aşımına uğrar ve yeniden oluşturulmalıdır. Yeni bir akış deneniyorsa, önceden ayrılan bağlantı noktaları kullanılabildiği sürece akış hemen başarılı olur.

Dağıtım boyutu azalır ve daha düşük bir katmana geçerse, kullanılabilir SNAT bağlantı noktalarının sayısı artar. Bu durumda, ayrılmış olan SNAT bağlantı noktaları ve ilgili akışları etkilenmez.

Bir bulut hizmeti yeniden dağıtılırsa ya da değiştirilmişse, altyapı arka uç havuzunu geçici olarak en fazla iki katına çıkabilir ve Azure, bir örnek başına beklenenden daha az SNAT bağlantı noktası ayırır.  Bu, SNAT bağlantı noktası tükenmesi olasılığını geçici olarak artırabilir. Sonuç olarak, havuz boyutu gerçek boyuta geçer ve Azure önceden ayrılmış SNAT bağlantı noktalarını, yukarıdaki tabloya göre beklenen sayıya otomatik olarak arttırır.  Bu davranış tasarım ve yapılandırılamaz.

SNAT bağlantı noktaları ayırmaları, IP aktarım protokolüne özgüdür (TCP ve UDP ayrı tutulur) ve aşağıdaki koşullarda yayımlanır:

### <a name="tcp-snat-port-release"></a>TCP SNAT bağlantı noktası sürümü

- Sunucu/istemci FIN/ACK gönderiyorsa, SNAT bağlantı noktası 240 saniye sonra serbest bırakılır.
- Bir RST görülüyorsa, SNAT bağlantı noktası 15 saniye sonra serbest bırakılır.
- boşta kalma zaman aşımına ulaşıldı

### <a name="udp-snat-port-release"></a>UDP SNAT bağlantı noktası sürümü

- boşta kalma zaman aşımına ulaşıldı

## <a name="problemsolving"></a>Sorun çözme 

Bu bölüm, Azure 'da giden bağlantılarla oluşabilecek SNAT tükenmesi ve diğer senaryolara karşı hafifletmek için tasarlanmıştır.

### <a name="snatexhaust"></a>SNAT (PAT) bağlantı noktası tükenmesi yönetimi
[Pat](#pat) Için kullanılan [kısa ömürlü bağlantı noktaları](#preallocatedports) , [genel IP ilişkili](#defaultsnat) ve [genel yük dengeli uç nokta](#publiclbendpoint)olmadığında açıklandığı gibi, tüketilmeyen bir kaynaktır.

Aynı hedef IP adresine ve bağlantı noktasına giden çok sayıda giden TCP veya UDP bağlantısı başlattığın ve başarısız olmuş bağlantıları gözlemlerseniz veya SNAT bağlantı noktalarını (önceden ayrılan [kısa ömürlü bağlantı noktaları](#preallocatedports) ) tahmin ettiğiniz destekle karşılaşdığınızı bilirsiniz [Pat](#pat)tarafından kullanılan), çeşitli genel risk azaltma seçenekleriniz vardır. Bu seçenekleri gözden geçirin ve senaryonuz için nelerin kullanılabilir ve en iyisi olduğuna karar verin. Bir veya daha fazla bu senaryonun yönetilmesine yardımcı olabilir.

Giden bağlantı davranışını anlamakta sorun yaşıyorsanız, IP yığın istatistiklerini (netstat) kullanabilirsiniz. Ya da paket yakalamaları kullanılarak bağlantı davranışlarını gözlemlemek faydalı olabilir.

#### <a name="connectionreuse"></a>Bağlantıyı yeniden kullanmak için uygulamayı değiştirme 
Uygulamanızdaki bağlantıları yeniden kullandığınızda, SNAT için kullanılan kısa ömürlü bağlantı noktaları için talebi azaltabilirsiniz. Bu, özellikle HTTP/1.1 gibi protokoller için geçerlidir; burada bağlantı yeniden kullanım varsayılandır. Ve taşıma olarak HTTP kullanan diğer protokoller (örneğin, REST) sırasıyla faydalanabilir. 

Her istek için bireysel, atomik TCP bağlantılarından yeniden kullanım her zaman daha iyidir. Sonuçları daha performanslı, çok verimli bir TCP işlemi ile yeniden kullanın.

#### <a name="connection pooling"></a>Uygulamayı bağlantı havuzunu kullanacak şekilde değiştirme
Uygulamanızda, isteklerin dahili bir bağlantı kümesi arasında dahili olarak dağıtıldığı (mümkün olduğunda yeniden kullanıldığı) bir bağlantı havuzu oluşturabilirsiniz. Bu düzen, kullanımda olan kısa ömürlü bağlantı noktalarının sayısını kısıtlar ve daha öngörülebilir bir ortam oluşturur. Bu düzen, bir işlem yanıtında tek bir bağlantı yapıldığında birden çok eş zamanlı işleme izin vererek isteklerin verimini de artırabilir.  

Bağlantı havuzu, uygulamanızı geliştirmek için kullandığınız çerçeve içinde veya uygulamanızın yapılandırma ayarlarını zaten içerebilir. Bağlantı havuzunu bağlantı yeniden kullanımı ile birleştirebilirsiniz. Birden çok istekleriniz daha sonra aynı hedef IP adresi ve bağlantı noktası için sabit, öngörülebilir bir bağlantı noktası sayısı kullanır. İstekler, gecikme ve kaynak kullanımını azaltan TCP işlemlerinin verimli kullanımından de yararlanır. UDP işlemleri de yararlı olabilir, çünkü UDP akışlarının sayısını yönetmek, koşulları tüketmekten kaçınmak ve SNAT bağlantı noktası kullanımını yönetmektir.

#### <a name="retry logic"></a>Uygulamayı daha az agresif yeniden deneme mantığını kullanacak şekilde değiştirin
[Pat](#pat) için kullanılan [kısa ömürlü bağlantı noktaları](#preallocatedports) tükendiğinde ya da uygulama hatalarından oluşması durumunda, Decay ve geri dönüş mantığı olmadan, agresif veya deneme yanılma denemesi kesintileri meydana gelir veya kalıcı hale getirme. Daha az bir agresif yeniden deneme mantığı kullanarak, kısa ömürlü bağlantı noktaları için talebi azaltabilirsiniz. 

Kısa ömürlü bağlantı noktalarında 4 dakikalık boşta kalma zaman aşımı (ayarlanamaz) vardır. Yeniden denemeler çok ısrarlı ise, tükenmenin kendi kendine temizleme olanağı yoktur. Bu nedenle, uygulamanızın yeniden deneme işlemleri, tasarımın kritik bir parçasıdır.

#### <a name="assignilpip"></a>Her VM 'ye bir örnek düzeyi genel IP atayın
ILZAR atama, senaryonuzu [BIR VM 'ye örnek düzeyi genel IP](#ilpip)'ye dönüştürür. Her VM için kullanılan genel IP 'nin tüm kısa ömürlü bağlantı noktaları, sanal makine için kullanılabilir. (Genel bir IP 'nin kısa ömürlü bağlantı noktaları, ilgili dağıtımla ilişkili tüm VM 'Ler ile paylaşıldığından senaryolar aksine.) Çok sayıda ayrı IP adresini daha beyaz listeleyen olası etkileri gibi göz önünde bulundurmanız gereken bir denge vardır.

>[!NOTE] 
>Bu seçenek Web çalışanı rolleri için kullanılamaz.

### <a name="idletimeout"></a>Giden boşta kalma zaman aşımını sıfırlamak için keepcanlı tutmayı kullanın

Giden bağlantılarda 4 dakikalık bir boşta kalma zaman aşımı vardır. Bu zaman aşımı ayarlanamaz. Bununla birlikte, boş bir akışı yenilemek ve gerekirse bu boşta kalma zaman aşımını sıfırlamak için taşıma (örneğin, TCP keepcanlı tutma) veya uygulama katmanı keepcanlı tutma kullanabilirsiniz.  Bu yazılımın desteklenip desteklenmediğini veya nasıl etkinleştirileceğini öğrenmek için paketlenmiş yazılımların tedarikçisine danışın.  Boşta kalma zaman aşımını sıfırlamak için genellikle tek bir tarafın keepcanlı tutma oluşturması gerekir. 

## <a name="discoveroutbound"></a>Bir VM 'nin kullandığı genel IP 'yi keşfetme
Giden bir bağlantının genel kaynak IP adresini belirlemenin birçok yolu vardır. OpenDNS, sanal makinenizin genel IP adresini gösterebilmeniz için bir hizmet sağlar. 

Nslookup komutunu kullanarak, myip.opendns.com adı için OpenDNS çözümleyiciye bir DNS sorgusu gönderebilirsiniz. Hizmet, sorguyu göndermek için kullanılan kaynak IP adresini döndürür. Aşağıdaki sorguyu sanal makinenizde çalıştırdığınızda, yanıt o VM için kullanılan genel IP 'dir:

    nslookup myip.opendns.com resolver1.opendns.com


## <a name="next-steps"></a>Sonraki adımlar

- Kaynak Yöneticisi dağıtımlarında kullanılan [Load Balancer](load-balancer-overview.md) hakkında daha fazla bilgi edinin.
- Kaynak Yöneticisi dağıtımlarında sunulan [giden bağlantı](load-balancer-outbound-connections.md) senaryolarına yönelik mod hakkında bilgi edinin.
