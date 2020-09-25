---
title: Azure ön kapısına sahip Web uygulaması güvenlik duvarı için bot korumasını yapılandırma (Önizleme)
description: Azure portal kullanarak, ön kapıya yönelik Azure Web uygulaması güvenlik duvarı 'nda (WAF) bot koruma kuralını nasıl yapılandıracağınızı öğrenin.
author: vhorne
ms.service: web-application-firewall
ms.topic: article
services: web-application-firewall
ms.date: 08/21/2019
ms.author: victorh
ms.openlocfilehash: 2357c51f47bcb9bd8bbc6c408cb6d8edbab4d10e
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91267015"
---
# <a name="configure-bot-protection-for-web-application-firewall-preview"></a>Web uygulaması güvenlik duvarı için bot korumasını yapılandırma (Önizleme)
Bu makalede, Azure portal kullanarak, ön kapıya yönelik Azure Web uygulaması güvenlik duvarı 'nda (WAF) bot koruma kuralını yapılandırma işlemi gösterilmektedir. Bot koruma kuralı, CLı, Azure PowerShell veya Azure Resource Manager şablonu kullanılarak da yapılandırılabilir.

> [!IMPORTANT]
> Bot koruma kuralı kümesi şu anda genel önizleme aşamasındadır ve bir önizleme hizmet düzeyi sözleşmesi ile sunulmaktadır. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.  Ayrıntılar için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Önkoşullar

[Azure Portal kullanarak Azure ön kapısına yönelik BIR WAF Ilkesi oluşturma](waf-front-door-create-portal.md)bölümünde açıklanan yönergeleri Izleyerek ön kapıya yönelik temel bir WAF ilkesi oluşturun.

## <a name="enable-bot-protection-rule-set"></a>Bot koruma kuralı kümesini etkinleştir

**Yönetilen kurallar** sayfasında, bir Web uygulaması güvenlik duvarı ilkesi oluştururken, önce **yönetilen kural kümesi** ' ni bulun, açılan menüden **0 Microsoft_BotManager_1** kuralın önündeki onay kutusunu seçin ve ardından **gözden geçir + oluştur**' u seçin.

   ![Bot koruma kuralı](.././media/waf-front-door-configure-bot-protection/botmanager112019.png)

## <a name="next-steps"></a>Sonraki adımlar

- [WAF 'yi nasıl izleyeceğinizi](waf-front-door-monitor.md)öğrenin.
