---
title: Azure Güvenlik Merkezi 'nde işletim sistemi sürümünü güncelleştirme | Microsoft Docs
description: Bu makalede, Azure Güvenlik Merkezi önerisi **güncelleştirme işletim sistemi sürümünün**nasıl uygulanacağı gösterilmektedir.
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: aa372492-ecdb-4368-8fdd-d8ed31e216ee
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/28/2018
ms.author: rkarlin
ms.openlocfilehash: a17bc3cf8c8f1ffbb2a91e17944bfc1c203f439d
ms.sourcegitcommit: 0f54f1b067f588d50f787fbfac50854a3a64fff7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/12/2019
ms.locfileid: "60905865"
---
# <a name="update-os-version-in-azure-security-center"></a>Azure Güvenlik Merkezi 'nde işletim sistemi sürümünü güncelleştirme
Bulut hizmetlerindeki sanal makineler (VM) için, Azure Güvenlik Merkezi, daha yeni bir sürüm varsa işletim sisteminin (OS) güncelleştirilmesini önerir.  Yalnızca bulut Hizmetleri Web ve üretim yuvalarında çalışan çalışan rolleri izlenir.

> [!NOTE]
> Bu belge, örnek bir dağıtım kullanarak hizmeti tanıtır.  Bu, adım adım ilerleyen bir kılavuz değildir.
> 
> 

## <a name="implement-the-recommendation"></a>Öneriyi uygulayın
1. **Öneriler** dikey penceresinde, **Işletim sistemi sürümünü Güncelleştir**' i seçin.
   ![İşletim sistemi sürümünü güncelleştirme][1]
2. Bu, dikey pencere **güncelleştirme işletim sistemi sürümünü**açar. İşletim sistemi sürümünü güncelleştirmek için bu dikey penceredeki adımları izleyin.

## <a name="see-also"></a>Ayrıca bkz.
Bu makalede, "işletim sistemi sürümünü güncelleştirme" Güvenlik Merkezi önerisi nasıl uygulanır. Bulut hizmetleri hakkında daha fazla bilgi edinmek ve bir bulut hizmeti için işletim sistemi sürümünü güncelleştirmek için bkz.:

* [Cloud Services’e genel bakış](../cloud-services/cloud-services-choose-me.md)
* [Bulut hizmetini güncelleştirme](../cloud-services/cloud-services-update-azure-service.md)
* [Cloud Services’ı Yapılandırma](../cloud-services/cloud-services-how-to-configure-portal.md)

Güvenlik Merkezi hakkında daha fazla bilgi edinmek için şunlara bakın:

* [Azure Güvenlik Merkezi'nde güvenlik ilkelerini ayarlama](tutorial-security-policy.md) -- Azure abonelikleriniz ve kaynak gruplarınız için güvenlik ilkelerini yapılandırma hakkında bilgi edinin.
* [Azure Güvenlik Merkezi 'nde güvenlik önerilerini yönetme](security-center-recommendations.md) -önerilerin Azure kaynaklarınızı korumanıza nasıl yardımcı olduğunu öğrenin.
* [Azure Güvenlik Merkezi 'Nde güvenlik durumu izleme](security-center-monitoring.md) -Azure kaynaklarınızın sistem durumunu izlemeyi öğrenin.
* [Azure Güvenlik Merkezi'nde güvenlik uyarılarını yönetme ve yanıtlama](security-center-managing-and-responding-alerts.md) -- Güvenlik uyarılarını yönetme ve yanıtlama hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile iş ortağı çözümlerini izleme](security-center-partner-solutions.md) - İş ortağı çözümlerinizin sistem durumunu nasıl izleyeceğiniz hakkında bilgi edinin.
* [Azure Güvenlik Merkezi ile ilgili SSS](security-center-faq.md) -- Hizmeti kullanımı ile ilgili sık sorulan soruları bulabilirsiniz.
* [Azure Güvenlik blogu](https://blogs.msdn.com/b/azuresecurity/) --en son Azure Güvenlik haberlerini ve bilgilerini alın.

<!--Image references-->
[1]: ./media/security-center-update-os-version/update-os-version.png
