---
title: Tüm Azure Güvenlik Merkezi önerileri için başvuru tablosu
description: Bu makalede, Azure Güvenlik Merkezi 'nin kaynaklarınızı korumanıza yardımcı olan güvenlik önerileri listelenmektedir.
author: memildin
ms.service: security-center
ms.topic: reference
ms.date: 01/12/2021
ms.author: memildin
ms.custom: generated
ms.openlocfilehash: 11d4830908b4e86da12cd5e40cc26b1c1b1aecbd
ms.sourcegitcommit: 431bf5709b433bb12ab1f2e591f1f61f6d87f66c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/12/2021
ms.locfileid: "98133056"
---
# <a name="security-recommendations---a-reference-guide"></a>Güvenlik önerileri - başvuru kılavuzu

Bu makalede, Azure Güvenlik Merkezi 'nde görebileceğiniz öneriler listelenir. Ortamınızda gösterilen öneriler, koruduğunuz kaynaklara ve özelleştirilmiş yapılandırmanıza bağlıdır.

Güvenlik Merkezi'nin önerileri en iyi yöntemlere dayanır. Bazıları genel uyumluluk çerçevelerine göre güvenlik ve uyumluluk için en iyi uygulamalara yönelik, Microsoft tarafından yazılmış ve Azure'a özgü yönergeler olan **Azure Güvenlik Karşılaştırması**'na uygundur.
[Azure Güvenlik Karşılaştırması hakkında daha fazla bilgi edinin](../security/benchmarks/introduction.md).

Bu önerilere yanıt verme hakkında bilgi edinmek için bkz. [Azure Güvenlik Merkezi 'nde önerileri düzeltme](security-center-remediate-recommendations.md).

Güvenli puanınız, tamamladığınız Güvenlik Merkezi önerilerinin sayısını temel alır. İlk olarak hangi önerilerin çözümlenmeyeceğine karar vermek için, her birinin önem derecesine ve güvenli puanınızın olası etkilerine göz atın.

> [!TIP]
> Bir önerinin açıklaması "ilişkili ilke yok" ifadesini içeriyorsa, bu, genellikle Bu önerilerin farklı bir öneriye _ve ilkesine bağlı_ olması nedeniyle oluşur. Örneğin, "Endpoint Protection sistem durumu arızaları düzeltildi..." önerisi, bir uç nokta koruma çözümünün da _yüklü_ olup olmadığını denetleyen öneriye dayanır ("Endpoint Protection çözümü yüklenmelidir..."). Temel _alınan önerinin_ bir ilkesi vardır.
> İlkeleri yalnızca temel öneriyle sınırlamak, ilke yönetimini basitleştirir.

## <a name="compute-recommendations"></a><a name='recs-compute'></a>İşlem önerileri

[!INCLUDE [asc-recs-compute](../../includes/asc-recs-compute.md)]

## <a name="data-recommendations"></a><a name='recs-data'></a>Veri önerileri

[!INCLUDE [asc-recs-data](../../includes/asc-recs-data.md)]

## <a name="identityandaccess-recommendations"></a><a name='recs-identityandaccess'></a>Identityandaccess önerileri

[!INCLUDE [asc-recs-identityandaccess](../../includes/asc-recs-identityandaccess.md)]

## <a name="networking-recommendations"></a><a name='recs-networking'></a>Ağ önerileri

[!INCLUDE [asc-recs-networking](../../includes/asc-recs-networking.md)]

## <a name="deprecated-recommendations"></a>Kullanım dışı öneriler

|Öneri|Açıklama & ilgili ilke|Önem derecesi|Hızlı düzelme etkin mi? ([Daha fazla bilgi](security-center-remediate-recommendations.md#quick-fix-remediation))|Kaynak türü|
|----|----|----|----|----|
|**Uygulama Hizmetleri 'ne erişim kısıtlı olmalıdır**|Ağ yapılandırmasını değiştirerek, çok geniş aralıklardan gelen trafiği reddedecek şekilde uygulama hizmetlerinize erişimi kısıtlayın.<br>(İlgili ilke: [Önizleme]: uygulama hizmetlerine erişim kısıtlı olmalıdır)|Yüksek|H|App service|
|**IaaS NSG 'lerdeki Web uygulamalarına yönelik kurallar sağlamlaştırılmış olmalıdır**|Web uygulamaları çalıştıran sanal makinelerinizin ağ güvenlik grubu (NSG), Web uygulaması bağlantı noktalarıyla ilgili aşırı izin veren NSG kuralları ile Harden.<br>(İlgili ilke: IaaS üzerinde Web uygulamalarına yönelik NSG kuralları sağlamlaştırılmış olmalıdır)|Yüksek|H|Sanal makine|
|**Gereksiz uygulama ayrıcalıklarını kaldırarak saldırı vektörünü azaltmak için pod güvenlik Ilkeleri tanımlanmalıdır (Önizleme)**|Gereksiz uygulama ayrıcalıklarını kaldırarak saldırı vektörünü azaltmak için pod güvenlik Ilkeleri tanımlayın. Yalnızca erişim izni verilen kaynaklara erişebilmeleri için pod güvenlik ilkelerinin yapılandırılması önerilir.<br>(İlgili ilke: [Önizleme]: Pod güvenlik Ilkeleri Kubernetes hizmetlerinde tanımlanmalıdır)|Orta|N|İşlem kaynakları (kapsayıcılar)|
|**IoT cihazlarınıza daha fazla görünürlük sağlamak için IoT güvenlik modülü için Azure Güvenlik Merkezi 'ni yükler**|IoT cihazlarınıza daha fazla görünürlük sağlamak için IoT güvenlik modülü için Azure Güvenlik Merkezi 'ni yükler.|Düşük|N|IoT cihazı|

## <a name="next-steps"></a>Sonraki adımlar

Öneriler hakkında daha fazla bilgi edinmek için aşağıdakilere bakın:

- [Azure Güvenlik Merkezi'nde güvenlik önerileri](security-center-recommendations.md)
- [Azure Güvenlik Merkezi'nde ağınızı koruma](security-center-network-recommendations.md)
