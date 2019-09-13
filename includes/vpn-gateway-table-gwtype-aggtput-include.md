---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 05/22/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 65c1011e6e005c190d1ae5d51fdd009f66a20956
ms.sourcegitcommit: 083aa7cc8fc958fc75365462aed542f1b5409623
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2019
ms.locfileid: "70919755"
---
|**SKU**   | **S2S/VNet-VNet<br>Tünelleri** | **P2S<br> SSTP Bağlantıları** | **P2S<br> Ikev2/OpenVPN bağlantıları** | **Toplam<br>Aktarım Hızı Kıyaslaması** | **BGP** | **Bölge yedekli** |
|---       | ---        | ---       | ---            | ---       | --- | --- |
|**Temel** | En çok, 10    | En çok, 128  | Desteklenmiyor  | 100 Mbps  | Desteklenmiyor| Hayır |
|**VpnGw1**| En çok, 30*   | En çok, 128  | En çok, 250       | 650 Mbps  | Desteklenen | Hayır |
|**VpnGw2**| En çok, 30*   | En çok, 128  | En çok, 500       | 1 Gbps    | Desteklenen | Hayır |
|**VpnGw3**| En çok, 30*   | En çok, 128  | En çok, 1000      | 1,25 Gb/sn | Desteklenen | Hayır |
|**VpnGw1AZ**| En çok, 30*   | En çok, 128  | En çok, 250       | 650 Mbps  | Desteklenen | Evet |
|**VpnGw2AZ**| En çok, 30*   | En çok, 128  | En çok, 500       | 1 Gbps    | Desteklenen | Evet |
|**VpnGw3AZ**| En çok, 30*   | En çok, 128  | En çok, 1000      | 1,25 Gb/sn | Desteklenen | Evet |

(*) 30'dan fazla S2S VPN tüneline ihtiyacınız varsa [Sanal WAN](../articles/virtual-wan/virtual-wan-about.md) kullanın.

* Bu bağlantı sınırları ayrıdır. Örneğin, bir VpnGw1 SKU’sunda 128 SSTP bağlantısına ek olarak 250 IKEv2 bağlantısına sahip olabilirsiniz.

* Fiyatlandırma bilgileri [Fiyatlandırma](https://azure.microsoft.com/pricing/details/vpn-gateway) sayfasında bulunabilir.

* SLA (Hizmet Düzeyi Sözleşmesi) bilgilerine [SLA](https://azure.microsoft.com/support/legal/sla/vpn-gateway/) sayfasından ulaşılabilir.

* VpnGw1, VpnGw2, VpnGw3, VpnGw1AZ, VpnGw2AZ ve VpnGw3AZ yalnızca Kaynak Yöneticisi dağıtım modelini kullanan VPN ağ geçitleri için desteklenir.

* Temel SKU, eski SKU olarak kabul edilir. Temel SKU 'nun belirli özellik sınırlamaları vardır. Temel SKU kullanan bir ağ geçidini yeni ağ geçidi SKU 'larından birine yeniden boyutlandıramazsınız, bunun yerine VPN ağ geçidinizi silmeyi ve yeniden oluşturmayı içeren yeni bir SKU 'ya geçiş yapmanız gerekir. Temel SKU 'YU kullanmadan önce ihtiyacınız olan özelliğin desteklendiğinden emin olun.

* Toplam Aktarım Hızı Kıyaslaması, tek bir ağ geçidi üzerinde yer alan birden fazla tünelin ölçümlerine bağlıdır. Bir VPN Gateway’e yönelik Toplam Aktarım Hızı Karşılaştırması S2S ve P2S’nin birleşimidir. **Çok sayıda P2S bağlantısına sahipseniz S2S bağlantınız aktarım hızı sınırlamalarından dolayı olumsuz etkilenebilir.** Toplam Verimlilik kıyaslaması, Internet trafiği koşulları ve uygulama davranışlarınız nedeniyle garantili bir aktarım hızı değildir.

* Müşterilerin VpnGw SKU 'Larının göreli performansını anlamalarına yardımcı olmak için, performansı ölçmek üzere genel kullanıma açık Iperf ve CTSTraffic araçları kullandık. Aşağıdaki tabloda, farklı algoritmalar kullanılarak performans testlerinin sonuçları listelenmiştir. Gördüğünüz gibi, her iki IPSec şifrelemesi ve bütünlüğü için GCMAES256 algoritması kullandığımızda en iyi performans elde edilir. IPSec şifrelemesi için AES256 ve bütünlüğü için SHA256 kullanılırken ortalama performans aldık. IPSec şifrelemesi için DES3 kullandığımızda ve bütünlüğü için SHA256 en düşük performansa sahip olduğumuz zaman.

|**SKU**   | **Kullanılan<br>algoritmalar** | **Gözlenen<br>üretilen iş** | **Saniyedeki<br>paket sayısı gözlemlendi** |
|---       | ---                 | ---            | ---                    |
|**VpnGw1**| GCMAES256<br>AES256 & SHA256<br>DES3 & SHA256| 650 Mbps<br>500 Mbps<br>120 Mbps   | 58.000<br>50,000<br>50,000|
|**VpnGw2**| GCMAES256<br>AES256 & SHA256<br>DES3 & SHA256| 1 Gbps<br>500 Mbps<br>120 Mbps | 90,000<br>80,000<br>55.000|
|**VpnGw3**| GCMAES256<br>AES256 & SHA256<br>DES3 & SHA256| 1,25 Gb/sn<br>550 Mbps<br>120 Mbps | 105.000<br>90,000<br>60,000|
|**VpnGw1AZ**| GCMAES256<br>AES256 & SHA256<br>DES3 & SHA256| 650 Mbps<br>500 Mbps<br>120 Mbps   | 58.000<br>50,000<br>50,000|
|**VpnGw2AZ**| GCMAES256<br>AES256 & SHA256<br>DES3 & SHA256| 1 Gbps<br>500 Mbps<br>120 Mbps | 90,000<br>80,000<br>55.000|
|**VpnGw3AZ**| GCMAES256<br>AES256 & SHA256<br>DES3 & SHA256| 1,25 Gb/sn<br>550 Mbps<br>120 Mbps | 105.000<br>90,000<br>60,000|
