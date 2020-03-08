---
title: Azure sanal ağ NAT bağlantısı sorunlarını giderme
titleSuffix: Azure Virtual Network NAT troubleshooting
description: Sanal ağ NAT ile ilgili sorunları giderin.
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
ms.date: 03/05/2020
ms.author: allensu
ms.openlocfilehash: c629b3425cd095a6ac9d305b5cd6de58ed9d572a
ms.sourcegitcommit: bc792d0525d83f00d2329bea054ac45b2495315d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78674323"
---
# <a name="troubleshoot-azure-virtual-network-nat-connectivity-problems"></a>Azure sanal ağ NAT bağlantısı sorunlarını giderme

Bu makale, yöneticilerin sanal ağ NAT kullanırken bağlantı sorunlarını tanılayıp çözümlemesine yardımcı olur.

>[!NOTE] 
>Sanal ağ NAT Şu anda genel önizleme olarak kullanılabilir. Şu anda yalnızca sınırlı sayıda [bölgede](nat-overview.md#region-availability)kullanılabilir. Bu önizleme, bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Ayrıntılar için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms).

## <a name="problems"></a>Sorunlar

* [SNAT tükenmesi](#snat-exhaustion)
* [ICMP ping işlemi başarısız oluyor](#icmp-ping-is-failing)
* [Bağlantı sorunları](#connectivity-failures)
* [IPv6 birlikte bulunma](#ipv6-coexistence)

Bu sorunları çözmek için aşağıdaki bölümdeki adımları izleyin.

## <a name="resolution"></a>Çözüm

### <a name="snat-exhaustion"></a>SNAT tükenmesi

Tek bir [NAT ağ geçidi kaynağı](nat-gateway-resource.md) 64.000 ' den 1.000.000 ' e kadar eşzamanlı akışı destekler.  Her IP adresi, kullanılabilir stoğa 64.000 SNAT bağlantı noktası sağlar. Her NAT ağ geçidi kaynağı için en fazla 16 IP adresi kullanabilirsiniz.  SNAT mekanizması daha ayrıntılı olarak [açıklanmıştır.](nat-gateway-resource.md#source-network-address-translation)

SNAT tükenmesi 'nın temel nedenlerinden biri, giden bağlantının kurulma ve yönetilme şekli için bir kenar modeldir.  Bu bölümü dikkatli bir şekilde inceleyin.

#### <a name="steps"></a>Adımlar

1. Uygulamanızın giden bağlantı oluşturma şeklini araştırın (örneğin, kod incelemesi veya paket yakalama). 
2. Bu etkinliğin beklenen davranış olup olmadığını veya uygulamanın hatalı çalışıp çalışmadığını belirleme.  Bulgularınızı eklemek için Azure Izleyici 'de [ölçümleri](nat-metrics.md) kullanın. SNAT bağlantıları ölçümü için "başarısız" kategorisini kullanın.
3. Uygun desenlerin izlendikten sonra değerlendirin.
4. SNAT bağlantı noktası tükenmesi 'nin NAT ağ geçidi kaynağına atanan ek IP adresleriyle azaltılıp azaltılmasının gerekip gerekmediğini değerlendirin.

#### <a name="design-patterns"></a>Tasarım desenleri

Mümkün olduğunda her zaman bağlantı yeniden kullanımı ve bağlantı havuzlarından yararlanın.  Bu desenler, kaynak tükenmesi sorunlarını doğrudan önler ve öngörülebilir, güvenilir ve ölçeklenebilir davranışa neden olur. Bu desenlerin temelleri birçok geliştirme kitaplığı ve çerçeve içinde bulunabilir.

_**Çözüm:**_ Uygun desenleri kullan

- Diğer işlemlere yönelik bağlantı kaynaklarını boşaltmak için uzun süre çalışan işlemler için [zaman uyumsuz yoklama düzenlerini](https://docs.microsoft.com/azure/architecture/patterns/async-request-reply) düşünün.
- Uzun süreli akışlar (örneğin, yeniden kullanılan TCP bağlantıları), ara sistemlerin zaman aşımına uğramaması için TCP keepcanlı veya uygulama katmanı keepcanlı olarak kullanılmalıdır.
- Geçici hata veya hata kurtarma sırasında agresif yeniden denemeler/bursts önlemek için düzgün [yeniden deneme desenleri](https://docs.microsoft.com/azure/architecture/patterns/retry) kullanılmalıdır.
Her HTTP işlemi için yeni TCP bağlantısı oluşturma ("atomik bağlantılar" olarak da bilinir), bir anti-örünmedir.  Atomik bağlantılar, uygulamanızın iyi ve çöp kaynaklarını ölçeklendirmasını engeller.  Her zaman ardışık düzen birden çok işlem aynı bağlantıya sahiptir.  Uygulamanız işlem hızı ve kaynak maliyetlerinde yarar olacaktır.  Uygulamanız aktarım katmanı şifrelemesini (örneğin, TLS) kullandığında, yeni bağlantıların işlenmesiyle ilişkili önemli bir maliyet vardır.  Ek en iyi yöntem desenleri için [Azure bulut tasarım modellerini](https://docs.microsoft.com/azure/architecture/patterns/) gözden geçirin.

#### <a name="possible-mitigations"></a>Olası azaltmalar

_**Çözüm:**_ Giden bağlantıyı aşağıdaki şekilde ölçeklendirin:

| Senaryo | Elde |Risk azaltma |
|---|---|---|
| SNAT bağlantı noktaları ve SNAT bağlantı noktası tükenmesi için yüksek kullanım dönemlerinde çekişme yaşıyor olursunuz. | Azure Izleyici 'de SNAT bağlantıları [ölçümü](nat-metrics.md) için "başarısız" kategorisi, zaman ve yüksek bağlantı hacminde geçici veya kalıcı sorunları gösterir.  | Ek genel IP adresi kaynakları veya genel IP öneki kaynakları ekleyip ekleyemediğine göre belirlenir. Bu ekleme, NAT ağ geçidiniz için toplamda en fazla 16 IP adresi sağlar. Bu ek, kullanılabilir SNAT bağlantı noktaları (IP adresi başına 64.000) için daha fazla envanter sağlar ve senaryonuzu daha da ölçeklendirmenize olanak tanır.|
| Zaten 16 IP adresi vermiş ve hala SNAT bağlantı noktası tükenmesi yaşıyor. | Ek IP adresi ekleme girişimi başarısız olur. Genel IP adresi kaynaklarından veya genel IP öneki kaynaklarından alınan toplam IP adresi sayısı toplam 16 değerini aşıyor. | Uygulama ortamınızı birden çok alt ağ arasında dağıtın ve her alt ağ için bir NAT ağ geçidi kaynağı sağlayın.  Önceki [kılavuza](#design-patterns)göre iyileştirmek için tasarım modellerinizi yeniden değerlendirin. |

>[!NOTE]
>SNAT tükenmesinin neden oluştuğunu anlamak önemlidir. Ölçeklenebilir ve güvenilir senaryolar için doğru desenleri kullandığınızdan emin olun.  İstek nedenini bilmeden bir senaryoya daha fazla SNAT bağlantı noktası eklenmesi, son çare olmalıdır. Senaryolarınızın SNAT bağlantı noktası envanterine neden olduğunu anladıysanız daha fazla IP adresi ekleyerek stoğa daha fazla SNAT bağlantı noktası eklenmesi, uygulamanız ölçeklenirken yalnızca aynı tükenme başarısızlığını geciktirecek.  Diğer verimsizlikleri ve anti-desenlerinde maskeleme yapabilirsiniz.

### <a name="icmp-ping-is-failing"></a>ICMP ping işlemi başarısız oluyor

[Sanal ağ NAT](nat-overview.md) , IPv4 UDP ve TCP protokollerini destekler. ICMP desteklenmez ve başarısız olması beklenir.  

_**Çözüm:**_ Bunun yerine, uçtan uca bağlantıyı doğrulamak için TCP bağlantı testlerini (örneğin, "TCP ping") ve UDP 'ye özgü uygulama katmanı testlerini kullanın.

Aşağıdaki tabloda, testleri başlatmak için kullanılacak araçlar için bir başlangıç noktası kullanılabilir.

| İşletim sistemi | Genel TCP bağlantısı sınaması | TCP uygulama katmanı testi | UDP |
|---|---|---|---|
| Linux | NC (genel bağlantı testi) | kıvrımlı (TCP uygulama katmanı testi) | uygulamaya özgü |
| Windows | [PsPing](https://docs.microsoft.com/sysinternals/downloads/psping) | PowerShell [Invoke-WebRequest](https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/invoke-webrequest) | uygulamaya özgü |

### <a name="connectivity-failures"></a>Bağlantı sorunları

[Sanal ağ NAT](nat-overview.md) ile ilgili bağlantı sorunları çeşitli farklı sorunlardan kaynaklanıyor olabilir:

* NAT ağ geçidinin geçici veya kalıcı [SNAT tükenmesi](#snat-exhaustion) ,
* Azure altyapısında geçici başarısızlıklar, 
* Azure ile genel Internet hedefi arasındaki yoldaki geçici sorunlar 
* genel Internet hedefindeki geçici veya kalıcı arızalar.

Doğrulama bağlantısı için aşağıdaki gibi araçları kullanın. [ICMP pingi desteklenmez](#icmp-ping-is-failing).

| İşletim sistemi | Genel TCP bağlantısı sınaması | TCP uygulama katmanı testi | UDP |
|---|---|---|---|
| Linux | NC (genel bağlantı testi) | kıvrımlı (TCP uygulama katmanı testi) | uygulamaya özgü |
| Windows | [PsPing](https://docs.microsoft.com/sysinternals/downloads/psping) | PowerShell [Invoke-WebRequest](https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/invoke-webrequest) | uygulamaya özgü |

#### <a name="snat-exhaustion"></a>SNAT tükenmesi

Bu makaledeki [SNAT tükenmesi](#snat-exhaustion) bölümüne bakın.

#### <a name="azure-infrastructure"></a>Azure altyapısı

Azure, altyapısını harika bir şekilde izleyip işletse bile, iletimlerin kayıpsız olduğunun garantisi olmadığı için geçici arızalar ortaya çıkabilir.  TCP uygulamaları için SYN yeniden iletimlerine izin veren tasarım düzenlerini kullanın. Bir kayıp SYN paketinin neden olduğu geçici etkileri azaltmak için TCP SYN yeniden aktarım için yeterince büyük olan bağlantı zaman aşımlarını kullanın.

_**Çözümden**_

* [SNAT tükenmesi](#snat-exhaustion)olup olmadığını denetleyin.
* SYN yeniden aktarım davranışını denetleyen bir TCP yığınındaki yapılandırma parametresi RTO ([yeniden Iletim zaman aşımı](https://tools.ietf.org/html/rfc793)) olarak adlandırılır. RTO değeri ayarlanabilir ancak varsayılan olarak üstel geri ile 1 saniye veya daha yüksektir.  Uygulamanızın bağlantı zaman aşımı çok kısaysa (örneğin 1 saniye), tek biçimli bağlantı zaman aşımları görebilirsiniz.  Uygulama bağlantısı zaman aşımını artırın.
* Daha uzun bir süre gözlemlerseniz, varsayılan uygulama davranışlarıyla birlikte beklenmeyen zaman aşımları varsa, daha fazla sorun giderme için bir destek

TCP bağlantı zaman aşımını azaltma veya RTO parametresini ayarlama yapay önerilmez.

#### <a name="public-internet-transit"></a>genel Internet geçişi

Geçici hataların olasılığı, hedef ve daha fazla ara sistem için daha uzun bir yol ile artar. Geçici hataların [Azure altyapısına](#azure-infrastructure)göre sıklığından artması beklenmektedir. 

Yukarıdaki [Azure altyapısı](#azure-infrastructure) bölümüyle aynı yönergeleri izleyin.

#### <a name="internet-endpoint"></a>Internet uç noktası

Yukarıdaki bölümler, iletişimin kurulduğu Internet uç noktasıyla ilgili önemli noktalara ek olarak uygulanır. Bağlantı başarısını etkileyebilecek diğer faktörler şunlardır:

* hedef tarafta trafik yönetimi, örneğin
- Hedef tarafı tarafından uygulanan API hız sınırlaması
- Volumetric DDoS azaltmaları veya Aktarım katmanı trafiği şekillendirme
* güvenlik duvarı veya hedefteki diğer bileşenler 

Genellikle, kaynak üzerinde paket yakalar ve hedefin ne olduğunu belirlemek için hedef (varsa) gereklidir.

_**Çözümden**_

* [SNAT tükenmesi](#snat-exhaustion)olup olmadığını denetleyin. 
* Aynı bölgedeki bir uç nokta ile veya başka bir yerde karşılaştırma için bağlantıyı doğrulayın.  
* Yüksek hacimli veya işlem oranı testi oluşturuyorsanız, oranın azaltılmasının, hataların oluşma riskini azalttığını araştırın.
* Değiştirme oranı başarısızlık oranını etkilerken, API Hız sınırlarına veya hedef taraftaki diğer kısıtlamalara ulaşılmadığını denetleyin.
* Araştırmanızın giderilmesi sonuçlandırıldıysanız, daha fazla sorun giderme için bir destek talebi açın.

#### <a name="tcp-resets-received"></a>TCP sıfırlama alındı

Kaynak VM 'de alınan TCP sıfırlamaları (TCP RST paketleri) gözlemlerseniz, bunlar, devam ediyor olarak tanınmayan akışlar için özel tarafta NAT ağ geçidi tarafından oluşturulabilir.  Olası bir nedenden dolayı TCP bağlantısının boşta kalma süresi doldu.  Boşta kalma zaman aşımını 4 dakika ila 120 dakikaya kadar ayarlayabilirsiniz.

TCP sıfırlamaları, NAT ağ geçidi kaynaklarının genel tarafında oluşturulmaz. Hedef tarafta TCP sıfırlamaları alırsanız, bunlar, NAT ağ geçidi kaynağı değil, kaynak VM 'nin yığını tarafından oluşturulur.

_**Çözümden**_

* [Tasarım desenleri](#design-patterns) önerilerini gözden geçirin.  
* Gerekirse daha fazla sorun giderme için bir destek talebi açın.

### <a name="ipv6-coexistence"></a>IPv6 birlikte bulunma

[Sanal ağ NAT](nat-overview.md) , IPv4 UDP ve TCP protokollerini destekler ve [IPv6 ön ekine sahip bir alt ağda dağıtım desteklenmez](nat-overview.md#limitations).

_**Çözüm:**_ NAT ağ geçidini IPv6 öneki olmayan bir alt ağda dağıtın.

[Sanal ağ NAT UserVoice](https://aka.ms/natuservoice)aracılığıyla ek özelliklerde ilgi gösterebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [Sanal ağ NAT](nat-overview.md) hakkında bilgi edinin
* [NAT ağ geçidi kaynağı](nat-gateway-resource.md) hakkında bilgi edinin
* [NAT ağ geçidi kaynakları için ölçümler ve uyarılar](nat-metrics.md)hakkında bilgi edinin.
* [UserVoice 'Ta sanal ağ NAT için bir sonraki derleme yapmanız gerektiğini bize söyleyin](https://aka.ms/natuservoice).

