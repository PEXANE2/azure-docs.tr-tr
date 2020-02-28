---
title: Azure sanal ağ NAT nedir?
description: Sanal ağ NAT özellikleri, kaynakları, mimarisi ve uygulamaya genel bakış. Sanal ağ NAT 'ın nasıl çalıştığını ve NAT ağ geçidi kaynaklarını bulutta nasıl kullanacağınızı öğrenin.
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
ms.date: 02/24/2020
ms.author: allensu
ms.openlocfilehash: 0e19271b5c69610dd9f44c3121e195b80732279c
ms.sourcegitcommit: 747a20b40b12755faa0a69f0c373bd79349f39e3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2020
ms.locfileid: "77669548"
---
# <a name="what-is-virtual-network-nat-public-preview"></a>Sanal ağ NAT nedir (Genel Önizleme)?

Sanal ağ NAT (ağ adresi çevirisi), sanal ağlar için yalnızca giden Internet bağlantısını basitleştirir. Bir alt ağda yapılandırıldığında, tüm giden bağlantılar belirttiğiniz statik genel IP adreslerini kullanır.  Doğrudan sanal makinelere bağlı yük dengeleyici veya genel IP adresleri olmadan giden bağlantı kurulabilir. NAT tamamen yönetiliyor ve yüksek oranda dayanıklı.

<!-- 
<img src="./media/nat-overview/flow-map.svg" width="270" align="center">
![Virtual Network NAT](./media/nat-overview/flow-map.svg)
-->

<p align="center">
  <img src="./media/nat-overview/flow-map.svg" width="256" title="Sanal ağ NAT">
</p>



*Şekil: sanal ağ NAT*


>[!NOTE] 
>Sanal ağ NAT Şu anda genel önizleme olarak kullanılabilir. Şu anda yalnızca sınırlı sayıda [bölgede](#region-availability)kullanılabilir. Bu önizleme, bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Ayrıntılar için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms).

## <a name="static-ip-addresses-for-outbound-only"></a>Yalnızca giden statik IP adresleri

Her alt ağ için NAT ile giden bağlantı tanımlanabilir.  Aynı sanal ağ içindeki birden çok alt ağ farklı NAT 'ları içerebilir. Bir alt ağ, hangi [NAT ağ geçidi kaynağının](./nat-gateway-resource.md) kullanılacağını belirterek yapılandırılır. Herhangi bir sanal makine örneğinden gelen tüm UDP ve TCP giden akışları NAT kullanır. 

NAT, standart SKU [genel IP adresi kaynaklarıyla](./virtual-network-ip-addresses-overview-arm.md#standard) veya [genel IP öneki kaynaklarıyla](./public-ip-address-prefix.md) veya her ikisinin birleşimiyle uyumludur.  Genel bir IP önekini doğrudan kullanabilir veya ön ek IP adreslerini birden çok NAT ağ geçidi kaynağı arasında dağıtabilirsiniz. NAT, tüm trafiği ön ek IP adresi aralığına göre bölümlendirilecektir.  Dağıtımlarınızın IP beyaz listesi artık kolay.

Alt ağ için tüm giden trafik, hiçbir müşteri yapılandırması olmadan NAT tarafından otomatik olarak işlenir.  Kullanıcı tanımlı yollar gerekli değildir. NAT diğer [giden senaryolara](../load-balancer/load-balancer-outbound-connections.md) göre önceliklidir ve bir alt ağın varsayılan Internet hedefini değiştirir.

## <a name="on-demand-snat-with-multiple-ip-addresses-for-scale"></a>Ölçek için birden çok IP adresi ile isteğe bağlı SNAT

NAT, "bağlantı noktası ağ adresi çevirisi" (PNAT veya PAT) kullanır ve çoğu iş yükü için önerilir. Dinamik veya daha fazla iş yükleri, isteğe bağlı giden akış ayırmasına kolayca eklenebilir. Kapsamlı ön planlama, ön tahsisi ve son olarak giden kaynakların aşırı sağlanması önlenmiş olur. SNAT bağlantı noktası kaynakları, belirli bir NAT ağ geçidi kaynağı kullanan tüm alt ağlarda paylaşılır ve kullanılabilir ve gerektiğinde sağlanır.

NAT 'a bağlı genel IP adresi, UDP ve TCP için en fazla 64.000 eşzamanlı akış sağlar. Tek bir IP adresi ile başlayabilir ve 16 genel IP adresini ölçeklendirebilirsiniz.

NAT, sanal ağdan Internet 'e akış oluşturulmasına izin verir. Yalnızca etkin bir akışa yanıt olarak Internet 'ten gelen trafiğe izin verilir.

Yük dengeleyici giden SNAT 'den farklı olarak NAT, bir sanal makine örneğinin özel IP 'si giden bağlantılar oluşturmak için hiçbir kısıtlamaya sahip değildir.  İkincil IP yapılandırması, NAT ile giden Internet bağlantısı oluşturabilir.

## <a name="coexistence-of-inbound-and-outbound"></a>Gelen ve giden birlikte bulunma

NAT, aşağıdaki standart SKU kaynaklarıyla uyumludur:

- [Yük Dengeleyici](../load-balancer/load-balancer-overview.md)
- [Genel IP adresi](../virtual-network/virtual-network-ip-addresses-overview-arm.md#public-ip-addresses)
- [Genel IP öneki](../virtual-network/public-ip-address-prefix.md)

NAT ile birlikte kullanıldığında, bu kaynaklar alt ağlarınızla gelen Internet bağlantısı sağlar. NAT, alt ağlarınızın tüm giden Internet bağlantılarını sağlar.

NAT ve uyumlu standart SKU özellikleri, akışın başlatıldığı yönden haberdar değildir. Gelen ve giden senaryolar birlikte çalışabilir. Bu senaryolar, akış yönünün farkında olduğundan, bu senaryolar doğru ağ adresi çevirileri alır. 

<!-- 
<img src="./media/nat-overview/flow-direction4.svg" width="500" align="center">
![Virtual Network NAT flow direction](./media/nat-overview/flow-direction4.svg)
-->
<p align="center">
  <img src="./media/nat-overview/flow-direction4.svg" width="512" title="Sanal ağ NAT akış yönü">
</p>

*Şekil: sanal ağ NAT akış yönü*

## <a name="fully-managed-highly-resilient"></a>Tam olarak yönetilen, yüksek oranda dayanıklı

NAT, başlangıçtan tamamen ölçeklendirildi. Bir artırma veya genişletme işlemi gerekli değildir.  Azure, NAT 'nin işlemini sizin için yönetir.  NAT, her zaman birden çok hata etki alanına sahiptir ve hizmet kesintisi olmadan birden çok hata olabilir.

## <a name="tcp-reset-for-unrecognized-flows"></a>Tanınmayan akışlar için TCP sıfırlaması

NAT 'nin özel tarafı, mevcut olmayan bir TCP bağlantısıyla iletişim kurmaya yönelik TCP sıfırlama paketleri gönderir. Bir örnek, boşta kalma zaman aşımına ulaşmış bağlantılardır. Alınan bir sonraki paket, bağlantı kapatmaya zorlamak ve özel IP adresine TCP sıfırlaması döndürecek.

NAT 'nin Genel tarafı TCP sıfırlama paketleri veya başka herhangi bir trafik oluşturmaz.  Yalnızca müşterinin sanal ağı tarafından üretilen trafik yayınlanır.

## <a name="configurable-idle-timeout"></a>Yapılandırılabilir boşta zaman aşımı

4 dakikalık varsayılan bir boşta kalma süresi kullanılır ve 120 dakikaya kadar artırılabilir. Akıştaki her etkinlik, TCP keepcanlı tutma dahil boşta zamanlayıcısını de sıfırlayabilir.

## <a name="regional-or-zone-isolation-with-availability-zones"></a>Kullanılabilirlik alanları ile bölgesel veya bölge yalıtımı

NAT varsayılan olarak bölgesel bir bölgedir. [Kullanılabilirlik alanları](../availability-zones/az-overview.md) senaryoları oluştururken, NAT belirli bir bölgede (zonal dağıtım) yalıtılabilir.

<!-- 
<img src="./media/nat-overview/az-directions.svg" width="500" align="center">
![Virtual Network NAT with availability zones](./media/nat-overview/az-directions.svg)
-->

<p align="center">
  <img src="./media/nat-overview/az-directions.svg" width="512" title="Kullanılabilirlik alanları ile sanal ağ NAT">
</p>

*Şekil: kullanılabilirlik alanları ile sanal ağ NAT*

## <a name="multi-dimensional-metrics-for-observability"></a>Observability için çok boyutlu ölçümler

Azure Izleyici 'de kullanıma sunulan çok boyutlu ölçümler aracılığıyla NAT 'nizin işlemini izleyebilirsiniz. Bu ölçümler, kullanımı gözlemlemek ve sorunlarını gidermek için kullanılabilir.  NAT ağ geçidi kaynakları aşağıdaki ölçümleri sunar:
- Bayt
- Paketleri
- Bırakılan paketler
- Toplam SNAT bağlantısı
- Her Aralık için SNAT bağlantı durumu geçişleri.

<!-- "ADD when PM is done" Learn more about [NAT gateway metrics](./nat-gateway-metrics.md) -->

## <a name="sla"></a>SLA

Genel kullanılabilirlik aşamasında NAT veri yolu en az% 99,9 kullanılabilir.

## <a name = "region-availability"></a>Bölge kullanılabilirliği

NAT Şu anda şu bölgelerde kullanılabilir:

- Batı Avrupa
- Doğu Japonya
- ABD Doğu 2
- ABD Batı
- ABD Batı 2
- ABD Orta Batı

## <a name = "enable-preview"></a>Genel Önizleme katılımı

Aboneliklerin, genel önizlemeye katılımını sağlamak için kayıtlı olması gerekir.  Katılım için iki adımlı bir işlem gerekir ve aşağıdaki Azure CLı ve Azure PowerShell yönergeleri sunulmaktadır.  Etkinleştirmenin tamamlanması birkaç dakika sürebilir.

### <a name="azure-cli"></a>Azure CLI

1. aboneliği genel önizleme için Kaydet

    ```azurecli-interactive
      az feature register --namespace Microsoft.Network --name AllowNatGateway
    ```

2. kaydı etkinleştir

    ```azurecli-interactive
      az provider register --namespace Microsoft.Network
    ```

### <a name="azure-powershell"></a>Azure PowerShell

1. aboneliği genel önizleme için Kaydet

    ```azurepowershell-interactive
      Register-AzProviderFeature -ProviderNamespace Microsoft.Network -FeatureName AllowNatGateway
    ```

2. kaydı etkinleştir

    ```azurepowershell-interactive
      Register-AzResourceProvider -ProviderNamespace Microsoft.Network
    ```

## <a name="pricing"></a>Fiyatlandırma

NAT Gateway iki ayrı ölçü ile faturalandırılır:

| Ölçüm | Fiyat |
| --- | --- |
| Kaynak saatleri | $0.045/saat |
| İşlenen veri | $0.045/GB |

Bir NAT ağ geçidi kaynağının bulunduğu süre için kaynak saatleri hesapları.
Bir NAT ağ geçidi kaynağı tarafından işlenen tüm trafik için veri işleme hesapları.

Genel Önizleme sırasında fiyatlandırma %50 ' de indirimli.

## <a name="support"></a>Destek

NAT, normal destek kanalları aracılığıyla desteklenir.

## <a name="feedback"></a>Geri Bildirim

Hizmeti nasıl geliştirebileceğimizi öğrenmek istiyoruz. [Görüşlerinizi genel önizlemede](https://aka.ms/natfeedback) bizimle paylaşabilirsiniz.  Ve [NAT Için UserVoice](https://aka.ms/natuservoice)'ta daha sonra derleme yapmanız gerekenleri de önerebilir.

## <a name="limitations"></a>Sınırlamalar

- NAT, standart SKU genel IP 'si, genel IP öneki ve yük dengeleyici kaynaklarıyla uyumludur.   Temel kaynaklar (örneğin, temel yük dengeleyici) ve bunlardan türetilmiş tüm ürünler NAT ile uyumlu değildir.  Temel kaynakların NAT ile yapılandırılmamış bir alt ağa yerleştirilmesi gerekir.
- IPv4 adres ailesi destekleniyor.  NAT, IPv6 adres ailesi ile etkileşime girmez.
- NSG akış günlüğü, NAT kullanılırken desteklenmez.
- NAT birden çok sanal ağa yayılamaz.

## <a name="next-steps"></a>Sonraki adımlar

- [NAT ağ geçidi kaynağı](./nat-gateway-resource.md)hakkında bilgi edinin.
- [UserVoice 'ta daha sonra neleri derleyeceğinizi bize söyleyin](https://aka.ms/natuservoice).
- [Genel önizleme hakkında geri bildirim sağlayın](https://aka.ms/natfeedback).
