---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 11/12/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 1643b20c6c157c43e93967cef364e703dbf4478e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "74829114"
---
|**VPN<br><br>Ağ Geçidi Üretimi** |**Sku**   | **S2S/VNet-VNet<br>Tünelleri** | **P2S<br> SSTP Bağlantıları** | **P2S<br> IKEv2/OpenVPN Bağlantıları** | **Toplam<br>Aktarım Hızı Kıyaslaması** | **BGP** | **Bölge yedekli** |
|---            |---         | ---        | ---       | ---            | ---       | ---       | ---|
|**Nesil1**|**Temel**   | En çok, 10    | En çok, 128  | Desteklenmiyor  | 100 Mbps  | Desteklenmiyor| Hayır |
|**Nesil1**|**VpnGw1**  | En çok, 30*   | En çok, 128  | En çok, 250       | 650 Mbps  | Destekleniyor | Hayır |
|**Nesil1**|**VpnGw2**  | En çok, 30*   | En çok, 128  | En çok, 500       | 1 Gbps    | Destekleniyor | Hayır |
|**Nesil1**|**VpnGw3**  | En çok, 30*   | En çok, 128  | En çok, 1000      | 1,25 Gb/sn | Destekleniyor | Hayır |
|**Nesil1**|**VpnGw1AZ**| En çok, 30*   | En çok, 128  | En çok, 250       | 650 Mbps  | Destekleniyor | Evet |
|**Nesil1**|**VpnGw2AZ**| En çok, 30*   | En çok, 128  | En çok, 500       | 1 Gbps    | Destekleniyor | Evet |
|**Nesil1**|**VpnGw3AZ**| En çok, 30*   | En çok, 128  | En çok, 1000      | 1,25 Gb/sn | Destekleniyor | Evet |
|        |            |            |           |                |           |           |     |
|**Nesil2**|**VpnGw2**  | En çok, 30*   | En çok, 128  | En çok, 500       | 1,25 Gb/sn | Destekleniyor | Hayır |
|**Nesil2**|**VpnGw3**  | En çok, 30*   | En çok, 128  | En çok, 1000      | 2,5 Gbps  | Destekleniyor | Hayır |
|**Nesil2**|**Vpngw4**  | En çok, 30*   | En çok, 128  | En çok, 5000      | 5 Gbps    | Destekleniyor | Hayır |
|**Nesil2**|**Vpngw5**  | En çok, 30*   | En çok, 128  | En çok, 10000      | 10 Gbps   | Destekleniyor | Hayır |
|**Nesil2**|**VpnGw2AZ**| En çok, 30*   | En çok, 128  | En çok, 500       | 1,25 Gb/sn | Destekleniyor | Evet |
|**Nesil2**|**VpnGw3AZ**| En çok, 30*   | En çok, 128  | En çok, 1000      | 2,5 Gbps  | Destekleniyor | Evet |
|**Nesil2**|**VpnGw4AZ**| En çok, 30*   | En çok, 128  | En çok, 5000      | 5 Gbps    | Destekleniyor | Evet |
|**Nesil2**|**VpnGw5AZ**| En çok, 30*   | En çok, 128  | En çok, 10000      | 10 Gbps   | Destekleniyor | Evet |

(*) 30'dan fazla S2S VPN tüneline ihtiyacınız varsa [Sanal WAN](../articles/virtual-wan/virtual-wan-about.md) kullanın.

* VpnGw SKU yeniden boyutlandırma temel SKU yeniden boyutlandırma dışında, aynı nesil içinde izin verilir. Temel SKU eski bir SKU ve özellik sınırlamaları vardır. Basic'ten başka bir VpnGw SKU'ya geçmek için Temel SKU VPN ağ geçidini silmeniz ve istediğiniz Nesil ve SKU boyut kombinasyonuyla yeni bir ağ geçidi oluşturmanız gerekir.

* Bu bağlantı sınırları ayrıdır. Örneğin, bir VpnGw1 SKU’sunda 128 SSTP bağlantısına ek olarak 250 IKEv2 bağlantısına sahip olabilirsiniz.

* Fiyatlandırma bilgileri [Fiyatlandırma](https://azure.microsoft.com/pricing/details/vpn-gateway) sayfasında bulunabilir.

* SLA (Hizmet Düzeyi Sözleşmesi) bilgilerine [SLA](https://azure.microsoft.com/support/legal/sla/vpn-gateway/) sayfasından ulaşılabilir.

* Tek bir tünelde en fazla 1 Gbps verim elde edilebilir. Yukarıdaki tablodaki Toplam İş İbzülLe, tek bir ağ geçidinden toplanan birden çok tünelin ölçümlerine dayanmaktadır. Bir VPN Gateway’e yönelik Toplam Aktarım Hızı Karşılaştırması S2S ve P2S’nin birleşimidir. **Çok sayıda P2S bağlantısına sahipseniz S2S bağlantınız aktarım hızı sınırlamalarından dolayı olumsuz etkilenebilir.** Toplam İş İbzülleme, Internet trafik koşulları ve uygulama davranışlarınız nedeniyle garantili bir iş artışı değildir.

Müşterilerimizin farklı algoritmalar kullanarak SNU'ların göreceli performansını anlamalarına yardımcı olmak için, performansları ölçmek için herkese açık iPerf ve CTSTraffic araçlarını kullandık. Aşağıdaki tabloda Nesil 1, VpnGw SKUs için performans testlerinin sonuçları listelanmaktadır. Gördüğünüz gibi, hem IPsec Şifreleme ve Bütünlük için GCMAES256 algoritması kullanıldığında en iyi performans elde edilir. IPsec Encryption için AES256 ve Bütünlük için SHA256 kullanırken ortalama performans elde ettik. Bütünlük için IPsec Şifreleme ve SHA256 için DES3'u kullandığımızda en düşük performansı yakaladık.

|**Nesil**|**Sku**   | **Kullanılan<br>algoritmalar** | **Elde<br>elde edilmesi gözlendi** | **Gözlenen saniyede<br>paketler** |
|---           |---       | ---                 | ---            | ---                    |
|**Nesil1**|**VpnGw1**| GCMAES256<br>AES256 & SHA256<br>DES3 & SHA256| 650 Mbps<br>500 Mbps<br>120 Mbps   | 58,000<br>50.000<br>50.000|
|**Nesil1**|**VpnGw2**| GCMAES256<br>AES256 & SHA256<br>DES3 & SHA256| 1 Gbps<br>500 Mbps<br>120 Mbps | 90,000<br>80,000<br>55,000|
|**Nesil1**|**VpnGw3**| GCMAES256<br>AES256 & SHA256<br>DES3 & SHA256| 1,25 Gb/sn<br>550 Mbps<br>120 Mbps | 105,000<br>90,000<br>60.000|
|**Nesil1**|**VpnGw1AZ**| GCMAES256<br>AES256 & SHA256<br>DES3 & SHA256| 650 Mbps<br>500 Mbps<br>120 Mbps   | 58,000<br>50.000<br>50.000|
|**Nesil1**|**VpnGw2AZ**| GCMAES256<br>AES256 & SHA256<br>DES3 & SHA256| 1 Gbps<br>500 Mbps<br>120 Mbps | 90,000<br>80,000<br>55,000|
|**Nesil1**|**VpnGw3AZ**| GCMAES256<br>AES256 & SHA256<br>DES3 & SHA256| 1,25 Gb/sn<br>550 Mbps<br>120 Mbps | 105,000<br>90,000<br>60.000|
