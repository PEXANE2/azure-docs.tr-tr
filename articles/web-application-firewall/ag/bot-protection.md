---
title: Azure Web Uygulama Güvenlik Duvarı (WAF) için bot koruma yapılandırma
description: Azure Uygulama Ağ Geçidi'nde Web Uygulaması Güvenlik Duvarı (WAF) için bot korumasının nasıl yapılandırılabildiğini öğrenin.
services: web-application-firewall
ms.topic: article
author: vhorne
ms.service: web-application-firewall
ms.date: 10/31/2019
ms.author: victorh
ms.openlocfilehash: 89c863e85d9eab27a47bc1bf7b98cd1c8d89e900
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73516870"
---
# <a name="configure-bot-protection-for-web-application-firewall-on-azure-application-gateway-preview"></a>Azure Uygulama Ağ Geçidi'nde Web Uygulaması Güvenlik Duvarı için bot koruma yapılandırma (Önizleme)

Bu makalede, Azure portalını kullanarak Uygulama Ağ Geçidi için Azure Web Uygulama Güvenlik Duvarı'nda (WAF) bot koruma kuralını nasıl yapılandırabileceğiniz gösterilmektedir. 

WAF'Nizin bilinen kötü amaçlı IP adreslerinden gelen istekleri engellemek veya günlüğe kaydetmek için yönetilen bir bot koruma kuralını etkinleştirebilirsiniz. IP adresleri Microsoft Threat Intelligence akışından kaynaklanmaktadır. Akıllı Güvenlik Grafiği Microsoft tehdit zekasına güç sağlar ve Azure Güvenlik Merkezi de dahil olmak üzere birden çok hizmet tarafından kullanılır.

> [!NOTE]
> Bot koruma kuralı kümesi şu anda genel önizlemededir ve bir önizleme hizmeti düzeyi sözleşmesiyle sağlanır. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Ayrıntılar [için Microsoft Azure Önizlemeleri](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) için Ek Kullanım Koşulları'na bakın.

## <a name="prerequisites"></a>Ön koşullar

Uygulama Ağ Geçidi için [Web Uygulaması Güvenlik Duvarı ilkeleri oluşturma'nda](create-waf-policy-ag.md)açıklanan yönergeleri izleyerek Uygulama Ağ Geçidi için temel bir WAF ilkesi oluşturun.

## <a name="enable-bot-protection-rule-set"></a>Bot koruma kuralı kümesini etkinleştirme

1. Daha önce oluşturduğunuz **Temel** ilke sayfasında Ayarlar **altında** **Kurallar'ı**seçin.  

2. Ayrıntılar sayfasında, kuralları **yönet** bölümünde, açılır menüden, bot Koruması kuralı için onay kutusunu seçin ve sonra **Kaydet'i**seçin.

> [!div class="mx-imgBorder"]
> ![Bot koruması](../media/bot-protection/bot-protection.png)

## <a name="next-steps"></a>Sonraki adımlar

Özel kurallar hakkında daha fazla bilgi için Azure [Uygulama Ağ Geçidi'nde Web Uygulaması Güvenlik Duvarı v2 için Özel kurallara](custom-waf-rules-overview.md)bakın.