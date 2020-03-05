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
ms.date: 03/02/2020
ms.author: allensu
ms.openlocfilehash: 185556e3045cb1a879bc256f4f2932549de71cd8
ms.sourcegitcommit: d45fd299815ee29ce65fd68fd5e0ecf774546a47
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2020
ms.locfileid: "78274945"
---
# <a name="troubleshoot-azure-virtual-network-nat-connectivity-problems"></a>Azure sanal ağ NAT bağlantısı sorunlarını giderme

Bu makale, yöneticilerin sanal ağ NAT kullanırken bağlantı sorunlarını tanılayıp çözümlemesine yardımcı olur.

>[!NOTE] 
>Sanal ağ NAT Şu anda genel önizleme olarak kullanılabilir. Şu anda yalnızca sınırlı sayıda [bölgede](nat-overview.md#region-availability)kullanılabilir. Bu önizleme, bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Ayrıntılar için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms).

## <a name="problems"></a>Sorunlar

- [SNAT tükenmesi](#snat-exhaustion).
- [ICMP ping işlemi başarısız oluyor](#icmp-ping-is-failing).

Bu sorunları çözmek için aşağıdaki bölümdeki adımları izleyin.

## <a name="resolution"></a>Çözüm

### <a name="snat-exhaustion"></a>SNAT tükenmesi

Tek bir [NAT ağ geçidi kaynağı](nat-gateway-resource.md) 64.000 ' den 1.000.000 ' e kadar eşzamanlı akışı destekler.  Her IP adresi, kullanılabilir stoğa 64.000 SNAT bağlantı noktası sağlar. Her NAT ağ geçidi kaynağı için en fazla 16 IP adresi kullanabilirsiniz.  SNAT mekanizması daha ayrıntılı olarak [açıklanmıştır.](nat-gateway-resource.md#source-network-address-translation)

#### <a name="steps"></a>Adımlar:

1. Uygulamanızın giden bağlantı oluşturma şeklini araştırın (örneğin, kod incelemesi veya paket yakalama). 
2. Bu etkinliğin beklenen davranış olup olmadığını veya uygulamanın hatalı çalışıp çalışmadığını belirleme.  Bulgularınızı eklemek için Azure Izleyici 'de ölçümleri kullanın.
3. Uygun desenlerin izlendikten sonra değerlendirin.
4. SNAT bağlantı noktası tükenmesi 'nin NAT ağ geçidi kaynağına atanan ek IP adresleriyle azaltılıp azaltılmasının gerekip gerekmediğini değerlendirin.

#### <a name="design-pattern"></a>Tasarım stili:

Mümkün olduğunda her zaman bağlantı yeniden kullanımı ve bağlantı havuzlarından yararlanın.  Bu model kaynak tükenmesi sorunlarını ortadan kaldırmak ve öngörülebilir davranışa neden olur. Bu desenlerin temelleri birçok geliştirme kitaplığı ve çerçeve içinde bulunabilir.
- Diğer işlemlere yönelik bağlantı kaynaklarını boşaltmak için uzun süre çalışan işlemler için [zaman uyumsuz yoklama düzenlerini](https://docs.microsoft.com/azure/architecture/patterns/async-request-reply) düşünün.
- Uzun süreli akışlar (örneğin, yeniden kullanılan TCP bağlantıları), ara sistemlerin zaman aşımına uğramaması için TCP keepcanlı veya uygulama katmanı keepcanlı olarak kullanılmalıdır.
- Geçici hata veya hata kurtarma sırasında agresif yeniden denemeler/bursts önlemek için düzgün [yeniden deneme desenleri](https://docs.microsoft.com/azure/architecture/patterns/retry) kullanılmalıdır.
Her HTTP işlemi için yeni TCP bağlantısı oluşturma ("atomik bağlantılar" olarak da bilinir), bir anti-örünmedir.  Atomik bağlantılar, uygulamanızın iyi ve çöp kaynaklarını ölçeklendirmasını engeller.  Her zaman ardışık düzen birden çok işlem aynı bağlantıya sahiptir.  Uygulamanız işlem hızı ve kaynak maliyetlerinde yarar olacaktır.  Uygulamanız aktarım katmanı şifrelemesini (örneğin, TLS) kullandığında, yeni bağlantıların işlenmesiyle ilişkili önemli bir maliyet vardır.  Ek en iyi yöntem desenleri için [Azure bulut tasarım modellerini](https://docs.microsoft.com/azure/architecture/patterns/) gözden geçirin.

#### <a name="mitigations"></a>Risk Azaltıcı Etkenler

Giden bağlantıyı şu şekilde ölçeklendirebilirsiniz:

| Senaryo | Risk azaltma |
|---|---|
| SNAT bağlantı noktaları ve SNAT bağlantı noktası tükenmesi için yüksek kullanım dönemlerinde çekişme yaşıyor olursunuz. | Ek genel IP adresi kaynakları veya genel IP öneki kaynakları ekleyip ekleyemediğine göre belirlenir. Bu ekleme, NAT ağ geçidiniz için toplamda en fazla 16 IP adresi sağlar. Bu ek, kullanılabilir SNAT bağlantı noktaları (IP adresi başına 64.000) için daha fazla envanter sağlar ve senaryonuzu daha da ölçeklendirmenize olanak tanır.|
| Zaten 16 IP adresi vermiş ve hala SNAT bağlantı noktası tükenmesi yaşıyor. | Uygulama ortamınızı birden çok alt ağ arasında dağıtın ve her alt ağ için bir NAT ağ geçidi kaynağı sağlayın. |

>[!NOTE]
>SNAT tükenmesinin neden oluştuğunu anlamak önemlidir. Ölçeklenebilir ve güvenilir senaryolar için doğru desenleri kullandığınızdan emin olun.  İstek nedenini bilmeden bir senaryoya daha fazla SNAT bağlantı noktası eklenmesi, son çare olmalıdır. Senaryolarınızın SNAT bağlantı noktası envanterine neden olduğunu anladıysanız daha fazla IP adresi ekleyerek stoğa daha fazla SNAT bağlantı noktası eklenmesi, uygulamanız ölçeklenirken yalnızca aynı tükenme başarısızlığını geciktirecek.  Diğer verimsizlikleri ve anti-desenlerinde maskeleme yapabilirsiniz.

### <a name="icmp-ping-is-failing"></a>ICMP ping işlemi başarısız oluyor

[Sanal ağ NAT](nat-overview.md) , IPv4 UDP ve TCP protokollerini destekler. ICMP desteklenmez ve başarısız olması beklenir.  Bunun yerine, uçtan uca bağlantıyı doğrulamak için TCP bağlantı testlerini (örneğin, "TCP ping") ve UDP 'ye özgü uygulama katmanı testlerini kullanın.

Aşağıdaki tabloda, testleri başlatmak için kullanılacak araçlar için bir başlangıç noktası kullanılabilir.

| İşletim sistemi | Genel TCP bağlantısı sınaması | TCP uygulama katmanı testi | UDP |
|---|---|---|---|
| Linux | NC (genel bağlantı testi) | kıvrımlı (TCP uygulama katmanı testi) | uygulamaya özgü |
| Windows | [PsPing](https://docs.microsoft.com/sysinternals/downloads/psping) | PowerShell [Invoke-WebRequest](https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/invoke-webrequest) | uygulamaya özgü |

## <a name="next-steps"></a>Sonraki adımlar

- [Sanal ağ NAT](nat-overview.md) hakkında bilgi edinin
- [NAT ağ geçidi kaynağı](nat-gateway-resource.md) hakkında bilgi edinin
