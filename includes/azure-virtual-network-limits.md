---
title: include dosyası
description: include dosyası
services: networking
author: anavinahar
ms.service: networking
ms.topic: include
ms.date: 01/14/2020
ms.author: anavin
ms.custom: include file
ms.openlocfilehash: 8752585e731f905636f57d31741e2be67f7140b3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80335033"
---
### <a name="networking-limits---azure-resource-manager"></a><a name="azure-resource-manager-virtual-networking-limits"></a>Ağ sınırları - Azure Kaynak Yöneticisi
Aşağıdaki sınırlar yalnızca abonelik başına bölge başına **Azure Kaynak Yöneticisi** aracılığıyla yönetilen ağ kaynakları için geçerlidir. [Geçerli kaynak kullanımınızı abonelik sınırlarınıza göre](../articles/networking/check-usage-against-limits.md)nasıl görüntüleyebilirsiniz öğrenin.

> [!NOTE]
> Yakın zamanda tüm varsayılan limitleri maksimum limitlerine yükselttik. Maksimum sınır sütunu yoksa, kaynağın ayarlanabilir sınırları yoktur. Geçmişte destek le bu limitleri artırdıysanız ve aşağıdaki tablolarda güncelleştirilmiş sınırlar görmüyorsanız, [ücretsiz olarak çevrimiçi müşteri destek isteği açın](../articles/azure-resource-manager/templates/error-resource-quota.md)

| Kaynak | Sınır | 
| --- | --- |
| Sanal ağlar |1000 |
| Sanal ağ başına alt ağ sayısı |3,000 |
| Sanal ağ başına sanal ağ eşlemesi |500 |
| [Sanal ağ başına sanal ağ ağ geçitleri (VPN ağ geçitleri)](../articles/vpn-gateway/vpn-gateway-about-vpngateways.md#gwsku) |1 |
| [Sanal ağ başına sanal ağ ağ geçitleri (ExpressRoute ağ geçitleri)](../articles/expressroute/expressroute-about-virtual-network-gateways.md#gwsku) |1 |
| Sanal ağ başına DNS sunucuları |20 |
| Sanal ağ başına özel IP adresi |65.536 |
| Ağ arabirimi başına özel IP adresleri |256 |
| Sanal makine başına özel IP adresleri |256 |
| Ağ arabirimi başına genel IP adresleri |256 |
| Sanal makine başına genel IP adresleri |256 |
| [Sanal bir makinenin veya rol örneğinin NIC başına eşzamanlı TCP veya UDP akışları](../articles/virtual-network/virtual-machine-network-throughput.md#flow-limits-and-recommendations) |500.000 |
| Ağ arabirim kartları |65.536 |
| Ağ Güvenlik Grupları |5.000 |
| NSG başına NSG kuralları |1000 |
| Bir güvenlik grubunda kaynak veya hedef için belirtilen IP adresleri ve aralıkları |4.000 |
| Uygulama güvenliği grupları |3,000 |
| IP yapılandırması başına uygulama güvenlik grupları, NIC başına |20 |
| Uygulama güvenlik grubuna göre IP yapılandırmaları |4.000 |
| Ağ güvenlik grubunun tüm güvenlik kuralları içinde belirtilebilen uygulama güvenlik grupları |100 |
| Kullanıcı tanımlı rota tabloları |200 |
| Rota tablosu başına kullanıcı tanımlı rotalar |400 |
| Azure VPN Ağ Geçidi'ne göre siteden siteye kök sertifikalar |20 |
| Sanal ağ TAPs |100 |
| Sanal ağ TAP başına ağ arabirimi TAP yapılandırmaları |100 |

#### <a name="public-ip-address-limits"></a><a name="publicip-address"></a>Genel IP adresi sınırları
| Kaynak | Varsayılan limit | Üst sınır |
| --- | --- | --- |
| Genel IP adresleri<sup>1</sup> | Basic için 10. | Desteğe başvurun. |
| Statik Genel IP adresleri<sup>1</sup> | Basic için 10. | Desteğe başvurun. |
| Standart Genel IP adresleri<sup>1</sup> | 10 | Desteğe başvurun. |
| Genel IP Ön Ekleri | bir abonelikteki Standart Genel IP sayısıyla sınırlıdır | Desteğe başvurun. |
| Genel IP öneki uzunluğu | /28 | Desteğe başvurun. |

<sup>1.1.2</sup> Genel IP adresleri için varsayılan sınırlar, Ücretsiz Deneme, Ödeme-As-You-Go, CSP gibi teklif kategori türüne göre değişir. Örneğin, Kurumsal Sözleşme abonelikleri için varsayılan değer 1000'dir.

#### <a name="load-balancer-limits"></a><a name="load-balancer"></a>Yük dengeleyici limitleri
Aşağıdaki sınırlar yalnızca abonelik başına bölgeye göre Azure Resource Manager ile yönetilen ağ kaynakları için geçerlidir. [Geçerli kaynak kullanımınızı abonelik sınırlarınıza göre](../articles/networking/check-usage-against-limits.md)nasıl görüntüleyebilirsiniz öğrenin.

**Standart Yük Dengeleyici**

| Kaynak                                | Sınır         |
|-----------------------------------------|-------------------------------|
| Yük dengeleyiciler                          | 1000                         |
| Kaynak başına kurallar                      | 1.500                         |
| NIC başına kurallar (NIC'deki tüm IP'ler arasında) | 300                           |
| Frontend IP yapılandırmaları              | 600                           |
| Arka uç havuz boyutu                       | 1.000 IP yapılandırması, tek sanal ağ |
| Yüksek kullanılabilirlik bağlantı noktaları                 | Dahili ön uç başına 1       |
| Yük Dengeleyicibaşına giden kurallar        | 20                            |
| [TCP boşta zaman adabı](https://docs.microsoft.com/azure/load-balancer/load-balancer-tcp-idle-timeout#tcp-idle-timeout) | 4 dakika/30 dakika          |


**Temel Yük Dengeleyici**

| Kaynak                                | Sınır        |
|-----------------------------------------|------------------------------|
| Yük dengeleyiciler                          | 1000                        |
| Kaynak başına kurallar                      | 250                          |
| NIC başına kurallar (NIC'deki tüm IP'ler arasında) | 300                          |
| Frontend IP yapılandırmaları              | 200                          |
| Arka uç havuz boyutu                       | 300 IP yapılandırması, tek kullanılabilirlik seti |
| Yük Dengeleyicisi başına kullanılabilirlik kümeleri     | 150                          |

<a name="virtual-networking-limits-classic"></a>Aşağıdaki sınırlar yalnızca abonelik başına **klasik** dağıtım modeli aracılığıyla yönetilen ağ kaynakları için geçerlidir. [Geçerli kaynak kullanımınızı abonelik sınırlarınıza göre](../articles/networking/check-usage-against-limits.md)nasıl görüntüleyebilirsiniz öğrenin.

| Kaynak | Varsayılan limit | Üst sınır |
| --- | --- | --- |
| Sanal ağlar |100 |100 |
| Yerel ağ siteleri |20 |50 |
| Sanal ağ başına DNS sunucuları |20 |20 |
| Sanal ağ başına özel IP adresi |4,096 |4,096 |
| Sanal bir makinenin veya rol örneğinin NIC başına eşzamanlı TCP veya UDP akışları |500,000, iki veya daha fazla NIC için 1.000.000'e kadar. |500,000, iki veya daha fazla NIC için 1.000.000'e kadar. |
| Ağ Güvenlik Grupları (NSGs) |200 |200 |
| NSG başına NSG kuralları |1000 |1000 |
| Kullanıcı tanımlı rota tabloları |200 |200 |
| Rota tablosu başına kullanıcı tanımlı rotalar |400 |400 |
| Genel IP adresleri (dinamik) |500 |500 |
| Ayrılmış genel IP adresleri |500 |500 |
| Dağıtım başına genel VIP |5 |Desteğe başvurun |
| Dağıtım başına özel VIP (dahili yük dengeleme) |1 |1 |
| Uç nokta erişim denetim listeleri (AÇS) |50 |50 |
