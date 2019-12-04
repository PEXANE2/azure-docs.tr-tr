---
title: Ağ için Azure Güvenlik Merkezi önerileri
description: Bu makalede, Azure Güvenlik Merkezi 'nin ağ kaynaklarınızı korumanıza yardımcı olan güvenlik önerileri listelenmektedir.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 47fa1f76-683d-4230-b4ed-d123fef9a3e8
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/29/2019
ms.author: memildin
ms.openlocfilehash: 1b8d84286d14949c007d1ece3d089a7606464aaf
ms.sourcegitcommit: 6bb98654e97d213c549b23ebb161bda4468a1997
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74781975"
---
# <a name="network-recommendations---reference-guide"></a>Ağ önerileri-başvuru kılavuzu

Bu makalede, Azure Güvenlik Merkezi 'nde ağınızın topolojisine ve internet 'e yönelik uç noktalarınıza ilişkin olarak görebileceğiniz önerilerin tam listesini bulabilirsiniz.

Bunların nasıl bulunacağını ve nasıl çözümlendiğinin bir açıklaması için [buraya](security-center-network-recommendations.md)bakın.

## <a name="network-recommendations"></a>Ağ önerileri

|Öneri adı|Açıklama|Önem Derecesi|Güvenlik puanı|Kaynak türü|
|----|----|----|----|----|----|
|Alt ağ düzeyindeki ağ güvenlik grupları etkinleştirilmelidir|Alt ağlarınızda dağıtılan kaynakların ağ erişimini denetlemek için ağ güvenlik grupları 'nı etkinleştirin.|Yüksek/orta|30|Alt ağ|
|Sanal makineler bir ağ güvenlik grubuyla ilişkilendirilmelidir|Sanal makinelerinizin ağ erişimini denetlemek için ağ güvenlik grupları 'nı etkinleştirin.|Yüksek/orta|30|Sanal makine|
|Internet 'e yönelik VM 'lerle izin veren ağ güvenlik grupları için erişim kısıtlanması gerekir|Mevcut izin verme kurallarınızın erişimini kısıtlayarak Internet 'e yönelik sanal makinelerinizin ağ güvenlik gruplarını sağlamlaştırın.|Yüksek|20|Sanal makine|
|IaaS NSG 'lerdeki Web uygulamalarına yönelik kurallar sağlamlaştırılmış olmalıdır|Web uygulamaları çalıştıran sanal makinelerinizin ağ güvenlik grubu (NSG), Web uygulaması bağlantı noktalarıyla ilgili aşırı izin veren NSG kuralları ile Harden.|Yüksek|20|Sanal makine|
|Uygulama Hizmetleri 'ne erişim kısıtlı olmalıdır|Ağ yapılandırmasını değiştirerek, çok geniş aralıklardan gelen trafiği reddedecek şekilde uygulama hizmetlerinize erişimi kısıtlayın.|Yüksek|10|App Service|
|Yönetim bağlantı noktaları sanal makinelerinizde kapatılmalıdır|Yönetim bağlantı noktalarına erişimi kısıtlamak için sanal makinelerinizin ağ güvenlik grubunu sağlamlaştırın.|Yüksek|10|Sanal makine|
DDoS koruma standardı etkinleştirilmelidir|DDoS koruma hizmeti standardını etkinleştirerek ortak IP 'Leri olan uygulamaları içeren sanal ağları koruyun. DDoS koruması, ağ Volumetric ve protokol saldırılarına karşı hafifletme imkanı sunar.|Yüksek|10|Sanal ağ|
|Sanal makinenizde IP iletimi devre dışı bırakılmalıdır|IP iletmeyi devre dışı bırakın. Bir sanal makinenin NIC 'inde IP iletimi etkinleştirildiğinde, makine diğer hedeflere yönelik trafiği alabilir. IP iletimi nadiren gereklidir (örneğin, VM 'yi bir ağ sanal gereci olarak kullanırken) ve bu nedenle ağ güvenlik ekibi tarafından incelenmelidir.|Orta|10|Sanal makine|
|Web uygulaması yalnızca HTTPS üzerinden erişilebilir olmalıdır|Web uygulamaları için "yalnızca HTTPS" erişimini etkinleştirin. HTTPS kullanımı, sunucu/hizmet kimlik doğrulamasını sağlar ve ağ katmanı gizlice dinleme saldırılarına karşı geçiş sırasında verileri korur.|Orta|20|Web uygulaması|
|Tam zamanında ağ erişim denetimi, sanal makinelere uygulanmalıdır|Tam zamanında (JıT) sanal makine (VM) erişim denetimi uygulayarak seçili bağlantı noktalarına erişimi kalıcı olarak kilitler ve yetkili kullanıcıların bunları JıT aracılığıyla yalnızca sınırlı bir süre için açmasını sağlayın.|Yüksek|20|Sanal makine|
|İşlev uygulamalarına yalnızca HTTPS üzerinden erişilebilir olması gerekir|İşlev uygulamaları için "yalnızca HTTPS" erişimini etkinleştirin. HTTPS kullanımı, sunucu/hizmet kimlik doğrulamasını sağlar ve ağ katmanı gizlice dinleme saldırılarına karşı geçiş sırasında verileri korur.|Orta|20|İşlev uygulaması|
|Depolama hesaplarına Güvenli aktarım etkinleştirilmelidir|Depolama hesaplarına Güvenli aktarım özelliğini etkinleştirin. Güvenli aktarım, depolama hesabınızı yalnızca güvenli bağlantılardan (HTTPS) istekleri kabul edecek şekilde zorlayan bir seçenektir. HTTPS kullanımı, sunucu ile hizmet arasında kimlik doğrulaması sağlar ve ağ katmanı saldırılarından geçiş sırasında, ortadaki adam, gizlice dinleme ve oturum ele geçirme gibi verileri korur.|Yüksek|20|Depolama hesabı|


## <a name="next-steps"></a>Sonraki adımlar
Diğer Azure kaynak türlerine uygulanan öneriler hakkında daha fazla bilgi edinmek için aşağıdakilere bakın:

* [Kimliği ve erişimi izleme](security-center-identity-access.md)
* [Makinelerinizi ve uygulamalarınızı koruma](security-center-virtual-machine-protection.md)
* [Azure SQL hizmetinizi koruma](security-center-sql-service-recommendations.md)

