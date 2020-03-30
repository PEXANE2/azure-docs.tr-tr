---
title: Azure sanal ağında ki alt ağ delegasyonu nedir?
description: Azure sanal ağında ki alt ağ delegasyonu hakkında bilgi edinin
services: virtual-network
documentationcenter: na
author: KumudD
manager: mtillman
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/20/2019
ms.author: kumud
ms.openlocfilehash: b33ff808b802b6848e2d5debaf515a73bf21a1bc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74281343"
---
# <a name="what-is-subnet-delegation"></a>Subnet delegasyonu nedir?

Alt ağ delegasyonu, seçtiğiniz bir Azure PaaS hizmeti için sanal ağınıza enjekte edilmesi gereken belirli bir alt ağ belirlemenize olanak tanır. Subnet delegasyonu, Azure hizmetlerinin sanal ağlarına entegrasyonunu yönetme konusunda müşteriye tam denetim sağlar.

Bir alt ağı Bir Azure hizmetine devrettiğinizde, bu hizmetin bu alt ağ için Azure hizmetinin örneklerini kararlı bir şekilde çalışmasına yardımcı olan bazı temel ağ yapılandırma kuralları oluşturmasına izin verirsiniz. Sonuç olarak, Azure hizmeti aşağıdakiler gibi dağıtım öncesi veya sonrası bazı koşullar belirleyebilir:
- hizmeti paylaşılan ve özel bir alt ağda dağıtın.
- hizmete, hizmetin düzgün çalışması için gereken bir dizi Ağ Niyeti İlkeleri dağıtım sonrası ekleyin.

##  <a name="advantages-of-subnet-delegation"></a>Subnet delegasyonunun avantajları

Bir alt ağı belirli hizmetlere vermek aşağıdaki avantajları sağlar:

- bir veya daha fazla Azure hizmeti için bir alt ağ belirlemeye ve alt ağdaki örnekleri gereksinimlere göre yönetmeye yardımcı olur. Örneğin, sanal ağ sahibi, kaynakları ve erişimi daha iyi yönetmek için devralınan bir alt ağ için aşağıdakileri tanımlayabilir:
    - ağ güvenlik gruplarıile trafik ilkelerini filtreleme.
    - politikaları kullanıcı tanımlı rotalarla yönlendirme.
    - hizmet bitiş noktaları yapılandırmaları ile hizmet entegrasyonu.
- Enjekte edilen hizmetlerin, dağıtımların ön koşullarını Ağ Amaçlı İlkeleri biçiminde tanımlayarak sanal ağla daha iyi bütünleştirmelerine yardımcı olur. Bu, enjekte edilen hizmetin işleyişini etkileyebilecek tüm eylemlerin PUT'ta engellenebilmesini sağlar.


## <a name="who-can-delegate"></a>Kim temsilci lik yapabilir?
Alt ağ delegasyonu, sanal ağ sahiplerinin belirli bir Azure Hizmetinin alt ağlarından birini belirlemek için gerçekleştirmeleri gereken bir alıştırmadır. Azure Hizmeti de örnekleri müşteri iş yüklerine göre tüketim için bu alt ağa dağıtıyor.

## <a name="impact-of-subnet-delegation-on-your-subnet"></a>Alt ağ delegasyonunun alt ağınıza etkisi
Her Azure hizmeti, enjeksiyon amacıyla devredilen bir alt ağda hangi özellikleri yapabileceklerini veya desteklemediklerini tanımlayacak ları kendi dağıtım modelini tanımlar:
- aynı alt ağda ayarlanan diğer Azure Hizmetleri veya VM / sanal makine ölçeğiyle paylaşılan alt ağ veya yalnızca bu hizmetin yalnızca örneklerinin yer aldığı özel bir alt ağı destekler.
- temsilci alt ağı ile NSG ilişkilendirme destekler.
- devredilen alt ağla ilişkili NSG'yi destekler, başka bir alt ağla da ilişkilendirilebilir.
- temsilci alt ağıyla rota tablosu ilişkilendirmesine izin verir.
- devredilen alt ağla ilişkili rota tablosunun başka bir alt ağla ilişkilendirilmesine izin verir.
- devredilen alt ağdaki en az IP Adresi sayısını belirler.
- görevlendirilen alt ağdaki IP Adresi alanını Özel IP Adresi alanından (10.0.0.0/8, 192.168.0.0/16, 172.16.0.0/12) belirtir.
- özel DNS yapılandırmasının bir Azure DNS girişi olduğunu belirtir.

Enjekte edilen hizmetler de aşağıdaki gibi kendi ilkelerini ekleyebilirsiniz:
- **Güvenlik ilkeleri**: Belirli bir hizmetin çalışması için gereken güvenlik kurallarının toplanması.
- **Rota ilkeleri**: Belirli bir hizmetin çalışması için gerekli yolların toplanması.

## <a name="what-subnet-delegation-does-not-do"></a>Alt ağ delegasyonu ne yapmaz

Devredilen bir alt ağa enjekte edilen Azure hizmetleri, hala devralınmayan alt ağlar için kullanılabilen temel özelliklere sahiptir:
-  Azure hizmetleri müşteri alt ağlarına örnekler enjekte edebilir, ancak varolan iş yüklerini etkileyemez.
-  Bu hizmetlerin uyguladığı ilkeler veya rotalar esnektir ve müşteri tarafından geçersiz kılınabilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Bir alt ağı temsilciolarak](manage-subnet-delegation.md)
