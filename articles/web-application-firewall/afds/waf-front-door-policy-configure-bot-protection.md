---
title: Azure Ön Kapı ile Web Uygulaması Güvenlik Duvarı için bot koruma yapılandırma (Önizleme)
description: Web Uygulaması Güvenlik Duvarı (WAF) öğrenin.
author: vhorne
ms.service: web-application-firewall
ms.topic: article
services: web-application-firewall
ms.date: 08/21/2019
ms.author: victorh
ms.openlocfilehash: f48b683044bc727cda461fb64a743c055188962a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76934669"
---
# <a name="configure-bot-protection-for-web-application-firewall-preview"></a>Web Uygulaması Güvenlik Duvarı için bot koruma yapılandırma (Önizleme)
Bu makalede, Azure portalı kullanarak Ön Kapı için Azure Web Uygulama Güvenlik Duvarı'nda (WAF) bot koruma kuralını nasıl yapılandırabileceğiniz gösterilmektedir. Bot koruma kuralı, CLI, Azure PowerShell veya Azure Kaynak Yöneticisi şablonu kullanılarak da yapılandırılabilir.

> [!IMPORTANT]
> Bot koruma kuralı kümesi şu anda genel önizlemededir ve bir önizleme hizmeti düzeyi sözleşmesiyle sağlanır. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.  Ayrıntılar için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Ön koşullar

[Azure portalını kullanarak Azure Ön Kapı için WAF ilkesi oluştur'da](waf-front-door-create-portal.md)açıklanan yönergeleri izleyerek Ön Kapı için temel bir WAF ilkesi oluşturun.

## <a name="enable-bot-protection-rule-set"></a>Bot koruma kuralı kümesini etkinleştirme

Yönetilen **Kurallar** sayfasında Bir Web Uygulaması Güvenlik Duvarı ilkesi oluştururken, önce **Yönetilen kural kümesi** bölümünü bulun, açılan menüden **Microsoft_BotManager_1.0** kuralının önündeki onay kutusunu seçin ve ardından **Gözden Geçir + Oluştur'u**seçin.

   ![Bot koruma kuralı](.././media/waf-front-door-configure-bot-protection/botmanager112019.png)

## <a name="next-steps"></a>Sonraki adımlar

- [WAF'ı](waf-front-door-monitor.md)nasıl izleyeceğizi öğrenin.
