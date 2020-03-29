---
title: Azure Güvenlik Merkezi'nde konteyner önerileri | Microsoft Dokümanlar
description: Bu belge, kapsayıcılarınızın korunmasına nasıl yardımcı olunailgili Azure Güvenlik Merkezi önerilerini açıklar.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 2e76c7f7-a3dd-4d9f-add9-7e0e10e9324d
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/20/2018
ms.author: memildin
ms.openlocfilehash: b1e1402f58b103570d6a98a5f9a01c8168abf749
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77912371"
---
# <a name="understand-azure-security-center-container-recommendations"></a>Azure Güvenlik Merkezi'ndeki kapsayıcı önerilerini anlama

Üretimde görev açısından kritik, kapsayıcı bulut yerel uygulamaları çalıştırmak için monolith uygulamalarınızı geçirerken, kolay ve hızlı dağıtım ve güncelleştirme dahil olmak üzere kapsayıcıların özelliklerinden yararlanabilirsiniz. Dağıtılan konteyner lerin sayısı artmaya devam ettikçe, konteynerlerinizin güvenlik durumuna görünürlük sağlamak ve onları tehditlerden korumaya yardımcı olmak için güvenlik çözümlerinin yerinde olması gerekir.

Azure Güvenlik Merkezi, kapsayıcılarınızı korumanıza yardımcı olmak için aşağıdaki özellikleri sağlar:

- **IaaS Linux makinelerinde barındırılan konteynerlere görünürlük**<br>Azure Güvenlik Merkezi'nde Kapsayıcılar sekmesi, Docker ile dağıtılan tüm sanal makineleri görüntüler. Güvenlik Merkezi, sanal bir makinedeki güvenlik sorunlarını incelerken, docker sürümü ve ana bilgisayarda çalışan görüntü sayısı gibi makinedeki kapsayıcılarla ilgili ek bilgiler sağlar.

    ![konteyner sekmesi](./media/security-center-container-recommendations/docker-recommendation.png)


- **Docker için BDT kıyaslama dayalı güvenlik önerileri**<br>Güvenlik Merkezi, Docker yapılandırmalarınızı tarar ve değerlendirilen başarısız kuralların listesini sunarak hatalı yapılandırmalar konusunda görünürlük sağlar. Güvenlik Merkezi, bu sorunları hızlı bir şekilde çözmenize ve zamandan tasarruf etmenizi sağlamak için yönergeler sağlar. Güvenlik Merkezi, Docker yapılandırmalarını sürekli değerlendirir ve son durumlar hakkında bilgi verir.

    ![konteyner sekmesi](./media/security-center-container-recommendations/container-cis-benchmark.png)

- **Gerçek zamanlı konteyner tehdit koruması**<br> Security Center, AuditD bileşeni ne sahip Linux makinelerindeki konteynerleriniz için gerçek zamanlı tehdit koruması sağlar. Uyarılar, ana bilgisayarda ayrıcalıklı bir kapsayıcı oluşturulması, Docker konteynerinin içinde çalışan Secure Shell (SSH) sunucusunun bir göstergesi veya kripto madencilerin kullanımı gibi birkaç şüpheli Docker faaliyetini tanımlar. Bu bilgileri kullanarak güvenlik sorunlarını hızlı bir şekilde çözebilir ve kapsayıcılarınızın güvenlik düzeyini artırabilirsiniz.

    ![konteyner sekmesi](./media/security-center-container-recommendations/docker-threat-detection.png)

## <a name="recommendations"></a>Öneriler
IaaS Linux makinelerinde barındırılan kullanılabilir kapsayıcıları ve Docker yapılandırmalarının güvenlik değerlendirmesini anlamanıza yardımcı olmak için aşağıdaki tabloları referans olarak kullanın.

| Öneri | Açıklama | Düzeltme |
| --- | --- | --- |
|Konteyner güvenlik yapılandırmalarında düzeltici güvenlik açıkları |Yapılandırma en iyi uygulamaları temel alan kapsayıcı güvenlik yapılandırmalarında güvenlik açıklarını düzeltin.| Kapsayıcı güvenlik yapılandırmalarında güvenlik açıklarını düzeltmek için:<br>1. Başarısız kuralların listesini gözden geçirin.<br>2. Belirtilen talimatlara göre her kuralı düzeltin.|


## <a name="next-steps"></a>Sonraki adımlar
Diğer Azure kaynak türleri için geçerli olan öneriler hakkında daha fazla bilgi edinmek için aşağıdakilere bakın:

* [Azure Güvenlik Merkezi'nde kimliği ve erişimi izleme](security-center-identity-access.md)
* [Azure Güvenlik Merkezi'nde ağınızı koruma](security-center-network-recommendations.md)
* [Azure SQL hizmetinizi Azure Güvenlik Merkezi'nde koruma](security-center-sql-service-recommendations.md)

Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi'nde makinelerinizi ve uygulamalarınızı koruma](security-center-virtual-machine-protection.md)
* [Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama](tutorial-security-policy.md) -- Azure abonelikleriniz ve kaynak gruplarınız için güvenlik ilkelerini yapılandırma hakkında bilgi edinin.
* [Azure Güvenlik Merkezi'ndeki güvenlik uyarılarını yönetme ve yanıtla](security-center-managing-and-responding-alerts.md) -- Güvenlik uyarılarını nasıl yönetip yanıtlaştak yapılacağını öğrenin.