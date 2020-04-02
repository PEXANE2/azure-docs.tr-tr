---
title: Azure Sanal Ağ NAT nedir?
titlesuffix: Azure Virtual Network
description: Sanal Ağ NAT özelliklerine, kaynaklarına, mimarisine ve uygulamasına genel bakış. Sanal Ağ NAT'nin nasıl çalıştığını ve buluttaki NAT ağ geçidi kaynaklarını nasıl kullanacağınızı öğrenin.
services: virtual-network
documentationcenter: na
author: asudbring
manager: KumudD
ms.service: virtual-network
Customer intent: As an IT administrator, I want to learn more about Virtual Network NAT, its NAT gateway resources, and what I can use them for.
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/14/2020
ms.author: allensu
ms.openlocfilehash: 50fc8b9cefe88a80f3f954ce363139b6a4a38589
ms.sourcegitcommit: 980c3d827cc0f25b94b1eb93fd3d9041f3593036
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80548381"
---
# <a name="what-is-virtual-network-nat"></a>Sanal Ağ NAT nedir?

Sanal Ağ NAT (ağ adresi çevirisi) sanal ağlar için giden yalnızca Internet bağlantısını basitleştirir. Bir alt ağ üzerinde yapılandırıldığında, tüm giden bağlantı belirtilen statik genel IP adreslerini kullanır.  Giden bağlantı, yük dengeleyicisi veya sanal makinelere doğrudan bağlı genel IP adresleri olmadan mümkündür. NAT tamamen yönetilen ve son derece esnektir.

<!-- 
<img src="./media/nat-overview/flow-map.svg" width="270" align="center">
![Virtual Network NAT](./media/nat-overview/flow-map.svg)
-->

<p align="center">
  <img src="./media/nat-overview/flow-map.svg" width="256" title="Sanal Ağ NAT">
</p>



*Şekil: Sanal Ağ NAT*

## <a name="static-ip-addresses-for-outbound-only"></a>Yalnızca giden ler için statik IP adresleri

NAT'li her alt ağ için giden bağlantı tanımlanabilir.  Aynı sanal ağ daki birden çok alt ağ farklı N'lere sahip olabilir. Bir alt ağ, hangi NAT ağ geçidi kaynağının kullanılacağı belirtilerek yapılandırılır. Herhangi bir sanal makine örneğinden tüm UDP ve TCP giden akışları NAT kullanır. 

NAT, standart SKU genel IP adresi kaynakları veya genel IP öneki kaynakları veya her ikisinin bir kombinasyonu ile uyumludur.  Genel BIR IP önekini doğrudan kullanabilir veya önekin genel IP adreslerini birden çok NAT ağ geçidi kaynağına dağıtabilirsiniz. NAT, önekin IP adresleri aralığına kadar olan tüm trafiği tımar eder.  Dağıtımlarınızın ip beyaz listesi artık çok kolay.

Alt ağ için tüm giden trafik nat tarafından herhangi bir müşteri yapılandırması olmadan otomatik olarak işlenir.  Kullanıcı tanımlı rotalar gerekli değildir. NAT, diğer giden senaryolardan önce gelir ve bir alt ağın varsayılan Internet hedefinin yerini alır.

## <a name="on-demand-snat-with-multiple-ip-addresses-for-scale"></a>Ölçek için birden fazla IP adresine sahip isteğe bağlı SNAT

NAT "bağlantı noktası ağ adresi çevirisi" (PNAT veya PAT) kullanır ve çoğu iş yükü için önerilir. Dinamik veya farklı iş yükleri, isteğe bağlı giden akış tahsisi ile kolayca karşılanabilir. Kapsamlı ön planlama, ön tahsis ve sonuçta giden kaynakların aşırı sağlanması önlenir. SNAT bağlantı noktası kaynakları, belirli bir NAT ağ geçidi kaynağı kullanılarak tüm alt ağlarda paylaşılır ve kullanılabilir ve gerektiğinde sağlanır.

NAT'ye bağlı genel bir IP adresi, UDP ve TCP için 64.000'e kadar eşzamanlı akış sağlar. Tek bir IP adresiyle başlayabilir ve en fazla 16 genel IP adresi ölçeklendirebilirsiniz.

NAT, sanal ağdan Internet'e akışların oluşturulmasına izin verir. Yalnızca etkin bir akışa yanıt olarak Internet'ten gelen geri dönüş trafiğine izin verilir.

Yük dengeleyici giden SNAT aksine, NAT bir sanal makine örneğinin özel IP giden bağlantılar yapabilirsiniz üzerinde hiçbir kısıtlama vardır.  İkincil IP yapılandırmaları NAT ile giden Internet bağlantısı oluşturabilir.

## <a name="coexistence-of-inbound-and-outbound"></a>Gelen ve gidenin birlikte liği

NAT aşağıdaki standart SKU kaynakları ile uyumludur:

- Yük dengeleyici
- Genel IP adresi
- Genel IP ön eki

NAT ile birlikte kullanıldığında, bu kaynaklar alt ağınıza gelen Internet bağlantısı sağlar. NAT, alt ağınızdan tüm giden Internet bağlantısını sağlar.

NAT ve uyumlu Standart SKU özellikleri, akışın başlatıldıklarının farkındadır. Gelen ve giden senaryolar bir arada bulunabilir. Bu özellikler akış yönünün farkında olduğundan, bu senaryolar doğru ağ adresi çevirilerini alır. 

<!-- 
<img src="./media/nat-overview/flow-direction4.svg" width="500" align="center">
![Virtual Network NAT flow direction](./media/nat-overview/flow-direction4.svg)
-->
<p align="center">
  <img src="./media/nat-overview/flow-direction4.svg" width="512" title="Sanal Ağ NAT akış yönü">
</p>

*Şekil: Sanal Ağ NAT akış yönü*

## <a name="fully-managed-highly-resilient"></a>Tamamen yönetilen, son derece esnek

NAT baştan tamamen ölçeklendirilir. Herhangi bir rampa ya da ölçeklendirme işlemi gerekli değildir.  Azure, NAT'nin çalışmasını sizin için yönetir.  NAT her zaman birden çok hata etki alanı vardır ve hizmet kesintisi olmadan birden çok hatayı sürdürebilir.

## <a name="tcp-reset-for-unrecognized-flows"></a>Tanınmayan akışlar için TCP Sıfırlama

NAT'nin özel tarafı, var olmayan bir TCP bağlantısında iletişim kurma girişimleri için TCP Sıfırlama paketleri gönderir. Bir örnek, boşta zaman alabilen bağlantılardır. Alınan bir sonraki paket, sinyal vermek ve bağlantı kapatmayı zorlamak için özel IP adresine bir TCP Sıfırlama döndürecektir.

NAT'nin genel tarafı TCP Sıfırlama paketleri veya başka bir trafik oluşturmaz.  Yalnızca müşterinin sanal ağı tarafından üretilen trafik yayılır.

## <a name="configurable-tcp-idle-timeout"></a>Yapılandırılabilir TCP boşta zaman acısı

Varsayılan 4 dakikalık bir TCP boşta zaman aşımı kullanılır ve 120 dakikaya kadar artırılabilir. Akıştaki herhangi bir etkinlik, TCP keepalives da dahil olmak üzere boşta zamanlayıcıyı da sıfırlayabilir.

## <a name="regional-or-zone-isolation-with-availability-zones"></a>Kullanılabilirlik bölgeleri ile bölgesel veya bölge yalıtımı

NAT varsayılan olarak bölgeseldir. [Kullanılabilirlik bölgeleri](../availability-zones/az-overview.md) senaryoları oluştururken, NAT belirli bir bölgede (bölge dağıtımı) yalıtılabilir.

<!-- 
<img src="./media/nat-overview/az-directions.svg" width="500" align="center">
![Virtual Network NAT with availability zones](./media/nat-overview/az-directions.svg)
-->

<p align="center">
  <img src="./media/nat-overview/az-directions.svg" width="512" title="Kullanılabilirlik bölgeleri ile Sanal Ağ NAT">
</p>

*Şekil: Kullanılabilirlik bölgeleri ile Sanal Ağ NAT*

## <a name="multi-dimensional-metrics-for-observability"></a>Gözlemlenebilirlik için çok boyutlu ölçümler

Azure Monitor'da açığa çıkan çok boyutlu ölçümler aracılığıyla NAT'nizin çalışmasını izleyebilirsiniz. Bu ölçümler, kullanımı gözlemlemek ve sorun giderme için kullanılabilir.  NAT ağ geçidi kaynakları aşağıdaki ölçümleri ortaya çıkarır:
- Bayt
- Paket
- Bırakılan Paketler
- Toplam SNAT bağlantısı
- SNAT bağlantı durumu aralık başına geçişler.

<!-- "ADD when PM is done" Learn more about [NAT gateway metrics](./nat-gateway-metrics.md) -->

## <a name="sla"></a>SLA

Genel kullanılabilirlik olduğunda, NAT veri yolu en az %99,9 kullanılabilir.


## <a name="pricing"></a>Fiyatlandırma

NAT ağ geçidi iki ayrı metre ile faturalandırılır:

| Ölçüm | Fiyat |
| --- | --- |
| Kaynak saatleri | $0.045/saat |
| İşlenen veriler | $0.045/TR |

Kaynak saatleri, NAT ağ geçidi kaynağının bulunduğu süreyi hesaplar.
NAT ağ geçidi kaynağı tarafından işlenen tüm trafik için işlenen veriler.

## <a name="availability"></a>Kullanılabilirlik

Sanal Ağ NAT ve NAT ağ geçidi kaynağı tüm Azure genel bulut [bölgelerinde](https://azure.microsoft.com/global-infrastructure/regions/)kullanılabilir.

## <a name="support"></a>Destek

NAT normal destek kanalları aracılığıyla desteklenir.

## <a name="feedback"></a>Geri Bildirim

Hizmeti nasıl geliştirebileceğimizi bilmek istiyoruz. Önerin ve [NAT için UserVoice](https://aka.ms/natuservoice)sonraki inşa etmelidir ne oy.


## <a name="limitations"></a>Sınırlamalar

* NAT standart SKU genel IP, genel IP öneki ve yük dengeleyici kaynakları ile uyumludur. Temel yük dengeleyicisi gibi temel kaynaklar ve bunlardan türetilen ürünler NAT ile uyumlu değildir.  Temel kaynaklar NAT ile yapılandırılan bir alt ağa yerleştirilmelidir.
* IPv4 adres ailesi desteklenir.  NAT, IPv6 adres ailesiyle etkileşime girmez.  NAT, IPv6 öneki olan bir alt ağ üzerinde dağıtılamaz.
* NAT kullanırken NSG akış günlüğü desteklenmez.
* NAT birden çok sanal ağa yayAmaz.

## <a name="next-steps"></a>Sonraki adımlar

* [NAT ağ geçidi kaynağı](./nat-gateway-resource.md)hakkında bilgi edinin.
* [UserVoice Sanal Ağ NAT için sonraki oluşturmak için ne söyle.](https://aka.ms/natuservoice)

