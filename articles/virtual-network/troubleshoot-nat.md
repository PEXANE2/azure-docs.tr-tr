---
title: Azure Sanal Ağ NAT bağlantısıyla sorun giderme
titleSuffix: Azure Virtual Network
description: Sanal Ağ NAT ile sorunları giderin.
services: virtual-network
documentationcenter: na
author: asudbring
manager: KumudD
ms.service: virtual-network
Customer intent: As an IT administrator, I want to troubleshoot Virtual Network NAT.
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/30/2020
ms.author: allensu
ms.openlocfilehash: c012a8d83761b88cc59b62d11fd3d5542ca7f7a1
ms.sourcegitcommit: 632e7ed5449f85ca502ad216be8ec5dd7cd093cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2020
ms.locfileid: "80396082"
---
# <a name="troubleshoot-azure-virtual-network-nat-connectivity"></a>Azure Sanal Ağ NAT bağlantısıyla sorun giderme

Bu makale, yöneticilerin Sanal Ağ NAT'ı kullanırken bağlantı sorunlarını tanılamanıza ve çözmenize yardımcı olur.

## <a name="problems"></a>Sorun

* [SNAT tükenmesi](#snat-exhaustion)
* [ICMP ping başarısız oluyor](#icmp-ping-is-failing)
* [Bağlantı hataları](#connectivity-failures)
* [IPv6 birlikte yaşama](#ipv6-coexistence)

Bu sorunları gidermek için aşağıdaki bölümdeki adımları izleyin.

## <a name="resolution"></a>Çözüm

### <a name="snat-exhaustion"></a>SNAT tükenmesi

Tek bir [NAT ağ geçidi kaynağı](nat-gateway-resource.md) 64.000'den 1 milyon eşzamanlı akışı destekler.  Her IP adresi, mevcut envantere 64.000 SNAT bağlantı noktası sağlar. NAT ağ geçidi kaynağı başına en fazla 16 IP adresi kullanabilirsiniz.  SNAT mekanizması [burada](nat-gateway-resource.md#source-network-address-translation) daha ayrıntılı olarak açıklanmıştır.

Sık sık SNAT tükenmesinin temel nedeni, giden bağlantının varsayılan değerlerinden nasıl oluşturulduk, yönetildiği veya yapılandırılabilir zamanlayıcılar tarafından değiştirildiğine ilişkin bir anti-desendir.  Bu bölümü dikkatli bir şekilde inceleyin.

#### <a name="steps"></a>Adımlar

1. Varsayılan boşta zaman dilimini 4 dakikadan daha yüksek bir değerle değiştirip değiştirip değiştirmedin.
2. Uygulamanızın giden bağlantı yı nasıl oluşturduğunu (örneğin, kod incelemesi veya paket yakalama) araştırın. 
3. Bu etkinliğin beklenen davranış olup olmadığını veya uygulamanın kötü davranış tanınıp davranmadığını belirleyin.  Bulgularınızı doğrulamak için Azure Monitor'daki [ölçümleri](nat-metrics.md) kullanın. SNAT Connections ölçümü için "Başarısız" kategorilerini kullanın.
4. Uygun desenlerin izilip uyulmamasa değerlendirin.
5. SNAT bağlantı noktası tükenmesi NAT ağ geçidi kaynağına atanan ek IP adresleri ile azaltılmalıdır değerlendirin.

#### <a name="design-patterns"></a>Tasarım desenleri

Her zaman mümkün olduğunca bağlantı yeniden kullanımı ve bağlantı havuzu avantajlarından yararlanın.  Bu desenler kaynak tükenmesi sorunlarını önler ve öngörülebilir davranışlara neden olur. Bu desenler için ilkel birçok geliştirme kütüphaneleri ve çerçeveleri bulunabilir.

_**Çözüm:**_ Uygun desenleri ve en iyi uygulamaları kullanma

- NAT ağ geçidi kaynaklarının varsayılan TCP boşta zaman dilimi 4 dakikadır.  Bu ayar daha yüksek bir değere değiştirilirse, NAT akışları daha uzun süre tutar ve [SNAT bağlantı noktası envanteri üzerinde gereksiz baskıya](nat-gateway-resource.md#timers)neden olabilir.
- Atomik istekler (bağlantı başına bir istek) kötü bir tasarım seçimidir. Bu tür anti-desen ölçek sınırlar, performansı azaltır ve güvenilirliği azaltır. Bunun yerine, bağlantı ve ilişkili SNAT bağlantı noktalarının sayısını azaltmak için HTTP/S bağlantılarını yeniden kullanın. TLS kullanırken düşük el sıkışma, genel merkez ve kriptografik işlem maliyeti nedeniyle uygulama ölçeği artacak ve performans artacaktır.
- DNS, istemci DNS çözümleyicileri sonucunu önbelleğe almadığında, birim olarak birçok ayrı akış sağlayabilir. Önbelleğe alma kullanın.
- UDP akışları (örneğin DNS aramaları) SNAT bağlantı noktalarını boşta zaman aşımı süresince ayırır. Boşta zaman aşımı ne kadar uzun sayılsa, SNAT bağlantı noktaları üzerindeki basınç da o kadar yüksek olur. Kısa boşta zaman alakart kullanın (örneğin 4 dakika).
- Bağlantı hacminizi şekillendirmek için bağlantı havuzlarını kullanın.
- TCP akışını asla sessizce terk etmeyin ve akışı temizlemek için TCP zamanlayıcılarına güvenin. TCP'nin bağlantıyı açıkça kapatmasına izin vermezseniz, durum ara sistemlerde ve uç noktalarda ayrılmış kalır ve SNAT bağlantı noktalarını diğer bağlantılar için kullanılamaz hale getirir. Bu uygulama hataları ve SNAT yorgunluğu tetikleyebilir. 
- Etki konusunda uzman bilgisi olmadan işletim sistemi düzeyindeKi TCP ilgili zamanlayıcı değerlerini kapatmayın. TCP yığını toparlanacak olsa da, bir bağlantının bitiş noktaları beklentileri uyuşmadığında uygulama performansınız olumsuz etkilenebilir. Zamanlayıcıları değiştirme isteği genellikle altta yatan bir tasarım sorununun işaretidir. Aşağıdaki önerileri gözden geçirin.

Çoğu kez SNAT tükenmesi de altta yatan uygulamada diğer anti-desenler ile güçlendirilmiş olabilir. Hizmetinizin ölçeğini ve güvenilirliğini geliştirmek için bu ek desenleri ve en iyi uygulamaları gözden geçirin.

- SNAT bağlantı noktası envanterini daha erken boşaltmak için varsayılan boşta kalma süresi 4 dakika da dahil olmak üzere değerleri düşürmek için [TCP boşta zaman amı](nat-gateway-resource.md#timers) azaltmanın etkisini keşfedin.
- Diğer işlemler için bağlantı kaynaklarını serbest hale getirmek için uzun süren işlemler için [eşzamanlı yoklama desenleri](https://docs.microsoft.com/azure/architecture/patterns/async-request-reply) düşünün.
- Uzun ömürlü akışlar (örneğin yeniden kullanılan TCP bağlantıları) ara sistemlerin zamanlamasını önlemek için TCP keepalives veya uygulama katmanı keepalives kullanmalıdır. Boşta kalma süresini artırmak son çaredir ve temel nedeni çözmeyebilir. Uzun bir zaman aşımı, zaman aşımı sona erdiğinde düşük oranlı hatalara neden olabilir ve gecikme ve gereksiz hatalara neden olabilir.
- Geçici hata veya hata kurtarma sırasında agresif yeniden deneme/patlamaları önlemek için zarif [yeniden deneme desenleri](https://docs.microsoft.com/azure/architecture/patterns/retry) kullanılmalıdır.
Her HTTP işlemi için yeni bir TCP bağlantısı oluşturmak ("atomik bağlantılar" olarak da bilinir) bir anti-desendir.  Atomik bağlantılar, uygulamanızın iyi ölçeklemesini ve kaynakları boşa harcamasını önleyecektir.  Her zaman aynı bağlantıya birden fazla işlem boru hattı.  Uygulamanız işlem hızı ve kaynak maliyetlerinden yararlanacaktır.  Uygulamanız aktarım katmanı şifrelemesi (örneğin TLS) kullandığında, yeni bağlantıların işlenmesiyle ilişkili önemli bir maliyet vardır.  Ek en iyi uygulama [desenleri](https://docs.microsoft.com/azure/architecture/patterns/) için Azure Bulut Tasarım Desenleri'ni gözden geçirin.

#### <a name="additional-possible-mitigations"></a>Ek olası azaltıcı etkenler

_**Çözüm:**_ Giden bağlantıyı aşağıdaki gibi ölçeklendirin:

| Senaryo | Kanıt |Risk azaltma |
|---|---|---|
| Yüksek kullanım dönemlerinde SNAT bağlantı noktaları ve SNAT bağlantı noktası tükenmesi için çekişme yaşıyorsunuz. | Azure Monitor'da SNAT Connections [ölçümü](nat-metrics.md) için "başarısız" kategorisi, zaman ve yüksek bağlantı hacminde geçici veya kalıcı hatalar gösterir.  | Ek genel IP adresi kaynakları veya genel IP öneki kaynakları ekleyip ekleyebileceğinizi belirleyin. Bu ek, NAT ağ geçidinize toplam 16 IP adresine kadar izin verir. Bu ek, kullanılabilir SNAT bağlantı noktaları için daha fazla stok sağlar (IP adresi başına 64.000) ve senaryonuzu daha da ölçeklendirmenize olanak sağlar.|
| Zaten 16 IP adresi verdiniz ve hala SNAT bağlantı noktası tükenmesi yaşıyorsunuz. | Ek IP adresi ekleme girişimi başarısız olur. Ortak IP adresi kaynaklarından veya genel IP öneki kaynaklarından toplam IP adresi sayısı toplam 16'yı aşıyor. | Uygulama ortamınızı birden çok alt ağ arasında dağıtın ve her alt ağ için bir NAT ağ geçidi kaynağı sağlayın.  Önceki [kılavuza](#design-patterns)göre en iyi duruma getirmek için tasarım deseni(ler)'i yeniden değerlendirin. |

>[!NOTE]
>SNAT yorgunluğunun neden oluştuğunu anlamak önemlidir. Ölçeklenebilir ve güvenilir senaryolar için doğru desenleri kullandığınızdan emin olun.  Talebin nedenini anlamadan bir senaryoya daha fazla SNAT bağlantı noktası eklemek son çare olmalıdır. Senaryonuzun Neden SNAT bağlantı noktası envanteri üzerinde baskı uyguladığını anlamıyorsanız, daha fazla IP adresi ekleyerek envantere daha fazla SNAT bağlantı noktası eklemek yalnızca uygulamanız ölçeklendirilirken aynı tükenme hatasını geciktirir.  Diğer verimsizlikleri ve anti-desenleri gontluyor olabilirsiniz.

### <a name="icmp-ping-is-failing"></a>ICMP ping başarısız oluyor

[Sanal Ağ NAT,](nat-overview.md) IPv4 UDP ve TCP protokollerini destekler. ICMP desteklenmez ve başarısız olması beklenir.  

_**Çözüm:**_ Bunun yerine, uçuç bağlantısını doğrulamak için TCP bağlantı testlerini (örneğin "TCP ping") ve UDP'ye özgü uygulama katmanı testlerini kullanın.

Aşağıdaki tablo, testleri başlatmak için hangi araçların kullanılacağı bir başlangıç noktası olarak kullanılabilir.

| İşletim sistemi | Genel TCP bağlantı testi | TCP uygulama katmanı testi | UDP |
|---|---|---|---|
| Linux | nc (genel bağlantı testi) | curl (TCP uygulama katmanı testi) | uygulamaya özgü |
| Windows | [Psping](https://docs.microsoft.com/sysinternals/downloads/psping) | PowerShell [Invoke-WebRequest](https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/invoke-webrequest) | uygulamaya özgü |

### <a name="connectivity-failures"></a>Bağlantı hataları

Virtual Network [NAT](nat-overview.md) ile bağlantı sorunları birkaç farklı sorunlardan kaynaklanabilir:

* NAT ağ geçidinin geçici veya kalıcı [SNAT](#snat-exhaustion) tükenmesi,
* Azure altyapısındaki geçici hatalar, 
* Azure ile ortak Internet hedefi arasındaki yolda geçici hatalar, 
* genel Internet hedefgeçici veya kalıcı hatalar.

Doğrulama bağlantısı için aşağıdaki gibi araçları kullanın. [ICMP ping desteklenmiyor.](#icmp-ping-is-failing)

| İşletim sistemi | Genel TCP bağlantı testi | TCP uygulama katmanı testi | UDP |
|---|---|---|---|
| Linux | nc (genel bağlantı testi) | curl (TCP uygulama katmanı testi) | uygulamaya özgü |
| Windows | [Psping](https://docs.microsoft.com/sysinternals/downloads/psping) | PowerShell [Invoke-WebRequest](https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/invoke-webrequest) | uygulamaya özgü |

#### <a name="snat-exhaustion"></a>SNAT tükenmesi

Bu makalede [SNAT yorgunluk](#snat-exhaustion) inceleme bölümü.

#### <a name="azure-infrastructure"></a>Azure altyapısı

Azure, altyapısını büyük bir özenle izler ve çalışır. Geçici arızalar oluşabilir, iletimlerin kayıpsız olduğunun garantisi yoktur.  TCP uygulamaları için SYN yeniden iletimine izin veren tasarım desenleri kullanın. Kayıp bir SYN paketinin neden olduğu geçici etkileri azaltmak için TCP SYN yeniden iletimine izin verecek kadar büyük bağlantı zaman zaman larını kullanın.

_**Çözüm:**_

* [SNAT yorgunluk](#snat-exhaustion)için kontrol edin.
* SYN yeniden iletim davranışını kontrol eden bir TCP yığınındaki yapılandırma parametresine RTO[(Yeniden iletim Zaman Dışarısı)](https://tools.ietf.org/html/rfc793)denir. RTO değeri ayarlanabilir, ancak genellikle 1 saniye veya daha yüksek varsayılan olarak üstel geri tepme ile.  Uygulamanızın bağlantı zaman ayarı çok kısaysa (örneğin 1 saniye), düzensiz bağlantı zaman zaman ları görebilirsiniz.  Uygulama bağlantısı zaman dışarısını artırın.
* Varsayılan uygulama davranışlarıyla daha uzun, beklenmeyen zaman aşımı gözlemlerseniz, daha fazla sorun giderme için bir destek örneği açın.

TCP bağlantı zaman süresini yapay olarak azaltmanızı veya RTO parametresini aparatlamanızı önermiyoruz.

#### <a name="public-internet-transit"></a>Toplu İnternet geçişi

Hedefe giden daha uzun bir yol ve daha fazla ara sistem le geçici hata olasılığı artar. Geçici hataların [Azure altyapısı](#azure-infrastructure)üzerinde sıklığının artması beklenmektedir. 

Önceki [Azure altyapısı](#azure-infrastructure) bölümüyle aynı kılavuzu izleyin.

#### <a name="internet-endpoint"></a>Internet bitiş noktası

Önceki bölümler, iletişimin kurulduğu Internet bitiş noktasıyla birlikte geçerlidir. Bağlantı başarısını etkilenebilen diğer faktörler şunlardır:

* dahil olmak üzere hedef tarafında trafik yönetimi,
- Hedef tarafın uyguladığı API oranı sınırlayıcı
- Hacimsel DDoS azaltımları veya taşıma katmanı trafiği şekillendirme
* varış yerinde güvenlik duvarı veya diğer bileşenler 

Genellikle paket yakalamalar kaynak ve hedef (varsa) ne yer alıyor belirlemek için gereklidir.

_**Çözüm:**_

* [SNAT yorgunluk](#snat-exhaustion)için kontrol edin. 
* Karşılaştırma için aynı bölgedeki veya başka bir yerde bir bitiş noktasına bağlantı doğrulayın.  
* Yüksek hacimli veya işlem hızı testi oluşturuyorsanız, oranı azaltmanın hata oluşumunu azaltıp azaltmadığınızı keşfedin.
* Oran değiştirme hata oranını etkiliyorsa, API oranı sınırlarına veya hedef taraftaki diğer kısıtlamalara ulaşılıp ulaşılamaydığını kontrol edin.
* Soruşturmanız sonuçsuz kaldıysa, daha fazla sorun giderme için bir destek örneği açın.

#### <a name="tcp-resets-received"></a>TCP Sıfırlamaları alındı

NAT ağ geçidi, devam eden olarak tanınmayan trafik için kaynak VM'de TCP sıfırlamaları oluşturur.

Olası nedenlerden biri, TCP bağlantısının zamansız zamanlanmış olmasıdır.  Boşta kalma süresini 4 dakikadan 120 dakikaya kadar ayarlayabilirsiniz.

TCP Sıfırlamaları, NAT ağ geçidi kaynaklarının genel tarafında oluşturulmadı. Hedef tarafındaki TCP sıfırlamaları NAT ağ geçidi kaynağı tarafından değil, kaynak VM tarafından oluşturulur.

_**Çözüm:**_

* Tasarım desen önerilerini gözden [geçirin.](#design-patterns)  
* Gerekirse daha fazla sorun giderme için bir destek örneği açın.

### <a name="ipv6-coexistence"></a>IPv6 birlikte yaşama

[Sanal Ağ NAT](nat-overview.md) IPv4 UDP ve TCP protokolleri destekler ve [bir IPv6 öneki ile](nat-overview.md#limitations)bir alt net üzerinde dağıtım desteklenmez.

_**Çözüm:**_ IPv6 öneki olmayan bir alt net üzerinde NAT ağ geçidi dağıtın.

[Sanal Ağ NAT UserVoice](https://aka.ms/natuservoice)üzerinden ek özellikler ilgi gösterebilir.

## <a name="next-steps"></a>Sonraki adımlar

* Sanal [Ağ NAT](nat-overview.md) hakkında bilgi edinin
* [NAT ağ geçidi kaynağı](nat-gateway-resource.md) hakkında bilgi edinin
* [NAT ağ geçidi kaynakları için ölçümler ve uyarılar](nat-metrics.md)hakkında bilgi edinin.
* [UserVoice Sanal Ağ NAT için sonraki oluşturmak için ne söyle.](https://aka.ms/natuservoice)

