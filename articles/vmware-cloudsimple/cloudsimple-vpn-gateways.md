---
title: Azure VMware çözümleri (AVS)-VPN ağ geçitleri
description: AVS siteden siteye VPN ve Noktadan siteye VPN kavramları hakkında bilgi edinin
author: sharaths-cs
ms.author: dikamath
ms.date: 08/20/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 73171e2c46bdf6c934db5777efe36ba51153a686
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2020
ms.locfileid: "77024866"
---
# <a name="vpn-gateways-overview"></a>VPN ağ geçitlerine genel bakış

VPN ağ geçidi, bir AVS bölgesi ağı ile şirket içi konumdaki bir bilgisayar veya genel İnternet üzerinden bir bilgisayar arasında şifrelenmiş trafik göndermek için kullanılır. Her bölge, birden çok bağlantıyı destekleyebilen bir VPN ağ geçidine sahip olabilir. Aynı VPN ağ geçidiyle birden fazla bağlantı oluşturduğunuzda, tüm VPN tünelleri kullanılabilir ağ geçidi bant genişliğini paylaşır.

AVS iki tür VPN ağ geçidi sağlar:

* Siteden siteye VPN ağ geçidi
* Noktadan siteye VPN ağ geçidi

## <a name="site-to-site-vpn-gateway"></a>Siteden siteye VPN ağ geçidi

Bir AVS bölgesi ağı ile şirket içi veri merkezi arasında şifrelenmiş trafik göndermek için siteden siteye VPN ağ geçidi kullanılır. Şirket içi ağınız ve AVS bölgesi ağı arasındaki ağ trafiği için alt ağları/CıDR aralığını tanımlamak üzere bu bağlantıyı kullanın.

VPN ağ geçidi, AVS özel bulutunuzda şirket içi hizmetleri ve şirket içi ağdan AVS özel bulutunuzda Hizmetleri kullanmanıza olanak sağlar. AVS, şirket içi ağınızdan bağlantı kurmak için ilke tabanlı bir VPN sunucusu sağlar.

Siteden siteye VPN için kullanım örnekleri:

* Şirket içi ağınızdaki herhangi bir iş istasyonundan AVS özel bulut vCenter ' nin erişilebilirliği.
* Şirket içi Active Directory vCenter Identity kaynağı olarak kullanın.
* VM şablonlarının, ISOs 'nin ve diğer dosyaların şirket içi kaynaklarınızdan AVS özel bulut vCenter 'larınıza uygun şekilde aktarılması.
* Şirket içi ağınızdan, AVS özel bulutunuzda çalışan iş yüklerinin erişilebilirliği.

![Siteden siteye VPN bağlantı topolojisi](media/cloudsimple-site-to-site-vpn-connection.png)

### <a name="cryptographic-parameters"></a>Şifreleme parametreleri

Siteden siteye VPN bağlantısı, güvenli bir bağlantı kurmak için aşağıdaki varsayılan şifreleme parametrelerini kullanır. Şirket içi VPN cihazınızdan bir bağlantı oluşturduğunuzda, şirket içi VPN ağ geçidiniz tarafından desteklenen aşağıdaki parametrelerden birini kullanın.

#### <a name="phase-1-proposals"></a>1\. aşama teklifleri

| Parametre | Teklif 1 | Teklif 2 | Teklif 3 |
|-----------|------------|------------|------------|
| IKE Sürümü | IKEv1 | IKEv1 | IKEv1 |
| Şifreleme | AES 128 | AES 256 | AES 256 |
| Karma algoritması| SHA 256 | SHA 256 | SHA 1 |
| Diffie Hellman grubu (DH grubu) | 2 | 2 | 2 |
| Yaşam süresi | 28.800 saniye | 28.800 saniye | 28.800 saniye |
| Veri boyutu | 4 GB | 4 GB | 4 GB |

#### <a name="phase-2-proposals"></a>2\. aşama teklifleri

| Parametre | Teklif 1 | Teklif 2 | Teklif 3 |
|-----------|------------|------------|------------|
| Şifreleme | AES 128 | AES 256 | AES 256 |
| Karma algoritması| SHA 256 | SHA 256 | SHA 1 |
| Kusursuz Iletme gizliliği grubu (PFS Grubu) | Hiçbiri | Hiçbiri | Hiçbiri |
| Yaşam süresi | 1\.800 saniye | 1\.800 saniye | 1\.800 saniye |
| Veri boyutu | 4 GB | 4 GB | 4 GB |


> [!IMPORTANT]
> VPN cihazınızda 1200 ' de TCP ve üst sınırı ayarlayın. Ya da VPN cihazlarınız, sahip olma özelliğini desteklemiyorsa, bunun yerine tünel arabirimindeki MTU değerini 1240 bayta ayarlayabilirsiniz.

## <a name="point-to-site-vpn-gateway"></a>Noktadan siteye VPN ağ geçidi

Bir AVS bölgesi ağı ile istemci bilgisayar arasında şifrelenmiş trafik göndermek için Noktadan siteye VPN kullanılır. Uçtan Siteye VPN, AVS özel bulut vCenter ve iş yükü VM 'leriniz de dahil olmak üzere AVS özel bulut ağınıza erişmenin en kolay yoludur. AVS özel bulutuna uzaktan bağlanıyorsanız Noktadan siteye VPN bağlantısı kullanın.

## <a name="next-steps"></a>Sonraki adımlar

* [VPN ağ geçidini ayarlama](vpn-gateway.md)
