---
title: Azure ön kapısına sahip Web uygulaması güvenlik duvarı için bot korumasını yapılandırma (Önizleme)
description: Web uygulaması güvenlik duvarını (WAF) öğrenin.
services: frontdoor
author: KumudD
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/31/2019
ms.author: kumud
ms.reviewer: tyao
ms.openlocfilehash: 0f36ea33badad52c55cd11491874db955df5408b
ms.sourcegitcommit: fa45c2bcd1b32bc8dd54a5dc8bc206d2fe23d5fb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/12/2019
ms.locfileid: "67846351"
---
# <a name="configure-bot-protection-for-web-application-firewall-preview"></a>Web uygulaması güvenlik duvarı için bot korumasını yapılandırma (Önizleme)
Bu makalede, Azure CLı, Azure PowerShell veya Azure Resource Manager şablonu kullanarak ön kapıya yönelik Azure Web uygulaması güvenlik duvarı 'nda (WAF) bot koruma kuralını yapılandırma işlemi gösterilmektedir.

WAF 'nizin bilinen kötü amaçlı IP adreslerinden gelen isteklere özel eylemler kazanması için yönetilen bir bot koruma kuralı kümesi etkinleştirilebilir. IP adresleri Microsoft Threat Intelligence akışından kaynaklıdır. [Intelligent Security Graph](https://www.microsoft.com/security/operations/intelligence) , Microsoft Threat Intelligence 'ı güçlendirir ve Azure Güvenlik Merkezi dahil birden çok hizmet tarafından kullanılır.

> [!IMPORTANT]
> Bot koruma kuralı kümesi şu anda genel önizleme aşamasındadır ve bir önizleme hizmet düzeyi sözleşmesi ile sunulmaktadır. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.  Ayrıntılar için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Önkoşullar

[Azure Portal kullanarak Azure ön kapısına yönelik BIR WAF Ilkesi oluşturma](waf-front-door-create-portal.md)bölümünde açıklanan yönergeleri Izleyerek ön kapıya yönelik temel bir WAF ilkesi oluşturun.

## <a name="enable-bot-protection-rule-set"></a>Bot koruma kuralı kümesini etkinleştir

1. Önceki bölümde oluşturduğunuz temel ilke sayfasında, **Ayarlar**' ın altında **kurallar**' a tıklayın.
2. Ayrıntılar sayfasında, **Kuralları Yönet** bölümünün altında, aşağı açılan menüden **botprotection-Preview-0,1**kuralının önündeki onay kutusunu seçin ve ardından yukarıya **Kaydet** ' i seçin.
    
   ![Bot koruma kuralı](./media/waf-front-door-configure-bot-protection/botprotect2.png)

## <a name="next-steps"></a>Sonraki adımlar

- [WAF](waf-front-door-monitor.md)'yi nasıl izleyeceğinizi öğrenin.
