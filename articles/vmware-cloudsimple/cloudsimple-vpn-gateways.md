---
title: CloudSimple tarafından Azure VMware Çözümü - VPN ağ geçitleri
description: CloudSimple Siteden Siteye VPN ve Site'ye Noktadan KullanıcıYA VPN kavramları hakkında bilgi edinin
author: sharaths-cs
ms.author: dikamath
ms.date: 08/20/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 662fa342b3a18f726b418c496ff3fda937445301
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77024866"
---
# <a name="vpn-gateways-overview"></a>VPN ağ geçitlerine genel bakış

VPN ağ geçidi, bir CloudSimple bölge ağı arasında şirket içi bir konumda veya genel internet üzerinden bir bilgisayar arasında şifreli trafik göndermek için kullanılır.  Her bölgede birden çok bağlantıyı desteklenebilen bir VPN ağ geçidi olabilir. Aynı VPN ağ geçidiyle birden fazla bağlantı oluşturduğunuzda, tüm VPN tünelleri kullanılabilir ağ geçidi bant genişliğini paylaşır.

CloudSimple iki tür VPN ağ geçidi sağlar:

* Siteden Siteye VPN ağ geçidi
* Noktadan Siteye VPN ağ geçidi

## <a name="site-to-site-vpn-gateway"></a>Siteden Siteye VPN ağ geçidi

Siteden Siteye VPN ağ geçidi, CloudSimple bölge ağı ile şirket içi veri merkezi arasında şifreli trafik göndermek için kullanılır. Şirket içi ağınızla CloudSimple bölge ağı arasındaki ağ trafiği için alt ağlar/CIDR aralığını tanımlamak için bu bağlantıyı kullanın.

VPN ağ geçidi, Özel Bulut'unuzda şirket içi hizmetler ve şirket içi ağdan Özel Bulut'unuzdaki hizmetleri tüketmenize olanak tanır.  CloudSimple, şirket içi ağınızdan bağlantıyı kurmak için ilke tabanlı bir VPN sunucusu sağlar.

Siteden Siteye VPN için kılıfları kullanma:

* Özel Bulut vCenter'ınızın şirket içi ağınızdaki herhangi bir iş istasyonundan erişilebilirliği.
* Şirket içi Active Directory'nizi vCenter kimlik kaynağı olarak kullanın.
* Şirket içi kaynaklarınızdan Özel Bulut vCenter'ınıza VM şablonlarının, ISO'larının ve diğer dosyaların rahat çalarından aktarılması.
* Özel Bulut'unuzda çalışan iş yüklerinin şirket içi ağınızdan erişilebilirliği.

![Siteden Siteye VPN bağlantı topolojisi](media/cloudsimple-site-to-site-vpn-connection.png)

### <a name="cryptographic-parameters"></a>Şifreleme parametreleri

Siteden Siteye VPN bağlantısı, güvenli bir bağlantı kurmak için aşağıdaki varsayılan şifreleme parametrelerini kullanır.  Şirket içi VPN aygıtınızdan bir bağlantı oluşturduğunuzda, şirket içi VPN ağ geçidiniz tarafından desteklenen aşağıdaki parametrelerden herhangi birini kullanın.

#### <a name="phase-1-proposals"></a>Faz 1 önerileri

| Parametre | Teklif 1 | Teklif 2 | Teklif 3 |
|-----------|------------|------------|------------|
| IKE Sürümü | IKEv1 | IKEv1 | IKEv1 |
| Şifreleme | AES 128 | AES 256 | AES 256 |
| Karma Algoritma| ŞAH 256 | ŞAH 256 | SHA 1 |
| Diffie Hellman Grubu (DH Grubu) | 2 | 2 | 2 |
| Yaşam Süresi | 28.800 saniye | 28.800 saniye | 28.800 saniye |
| Veri Boyutu | 4 GB | 4 GB | 4 GB |

#### <a name="phase-2-proposals"></a>Aşama 2 önerileri

| Parametre | Teklif 1 | Teklif 2 | Teklif 3 |
|-----------|------------|------------|------------|
| Şifreleme | AES 128 | AES 256 | AES 256 |
| Karma Algoritma| ŞAH 256 | ŞAH 256 | SHA 1 |
| Perfect Forward Gizlilik Grubu (PFS Group) | None | None | None |
| Yaşam Süresi | 1800 saniye | 1800 saniye | 1800 saniye |
| Veri Boyutu | 4 GB | 4 GB | 4 GB |


> [!IMPORTANT]
> VPN cihazınızda TCP MSS Bağlamayı 1200 olarak ayarlayın. Veya VPN aygıtlarınız MSS bağlamayı desteklemiyorsa, tünel arabirimindeki MTU'yu alternatif olarak 1240 bayt olarak ayarlayabilirsiniz.

## <a name="point-to-site-vpn-gateway"></a>Noktadan Siteye VPN ağ geçidi

Bir CloudSimple bölge ağı ile istemci bilgisayar arasında şifreli trafik göndermek için Site'ye Nokta VPN kullanılır.  Özel Bulut vCenter'ınız ve iş yükü VM'leriniz de dahil olmak üzere Özel Bulut ağınıza erişmenin en kolay yoludur.  Özel Bulut'a uzaktan bağlanıyorsanız, Site'ye Noktalı VPN bağlantısını kullanın.

## <a name="next-steps"></a>Sonraki adımlar

* [VPN ağ geçidini ayarlama](vpn-gateway.md)
