---
title: Azure ön kapısına sahip WAF için bot korumasını yapılandırma (Önizleme)
description: Web uygulaması güvenlik duvarını (WAF) öğrenin.
author: vhorne
ms.service: web-application-firewall
ms.topic: article
services: web-application-firewall
ms.date: 08/21/2019
ms.author: victorh
ms.openlocfilehash: 95660f764d28172ecb55a4952b785fea5f2aa4bb
ms.sourcegitcommit: dbde4aed5a3188d6b4244ff7220f2f75fce65ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/19/2019
ms.locfileid: "74186707"
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
    
   ![Bot koruma kuralı](.././media/waf-front-door-configure-bot-protection/botprotect2.png)

## <a name="next-steps"></a>Sonraki adımlar

- [WAF 'yi nasıl izleyeceğinizi](waf-front-door-monitor.md)öğrenin.
