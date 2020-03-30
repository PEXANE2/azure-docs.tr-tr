---
title: Barracuda verilerini Azure Sentinel'e bağlayın| Microsoft Dokümanlar
description: Barracuda verilerini Azure Sentinel'e nasıl bağlayabilirsiniz öğrenin.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.assetid: 3b33b4aa-7286-4d79-b461-8e1812edc2e1
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/30/2019
ms.author: yelevin
ms.openlocfilehash: 4e87bb57e6bdfea6307a166383da9dea187eea4f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77588493"
---
# <a name="connect-your-barracuda-appliance"></a>Barracuda cihazınızı bağlayın 



Barracuda Web Uygulaması Güvenlik Duvarı (WAF) konektörü, Barracuda günlüklerinizi Azure Sentinel'inizle kolayca bağlamanızı, panoları görüntülemenizi, özel uyarılar oluşturmanızı ve araştırmayı geliştirmenizi sağlar. Bu, kuruluşunuzun ağı hakkında daha fazla bilgi verir ve güvenlik işlem yeteneklerinizi geliştirir. Azure Sentinel, sorunsuz entegrasyon sağlamak için **Barracuda** ve Log Analytics aracısı arasındaki yerel tümleştirmeden yararlanır. 


> [!NOTE]
> Veriler, Azure Sentinel çalıştırdığınız çalışma alanının coğrafi konumunda depolanır.

## <a name="configure-and-connect-barracuda-waf"></a>Barracuda WAF'ı yapılandırın ve bağlayın
Barracuda Web Application Firewall, Log Analytics aracısı aracılığıyla günlükleri doğrudan Azure Sentinel'e entegre edebilir ve dışa aktarabilir.
1. [Barracuda WAF yapılandırma akışına](https://campus.barracuda.com/product/webapplicationfirewall/doc/73696965/configure-the-barracuda-web-application-firewall-to-integrate-with-the-oms-server-and-export-logs/)gidin ve aşağıdaki parametreleri kullanarak bağlantıyı kurmak için yönergeleri izleyin:
    - **Çalışma Alanı Kimliği**: Çalışma alanı kimliğinizin değerini Azure Sentinel Barracuda bağlayıcı sayfasından kopyalayın.
    - **Birincil anahtar:** Birincil anahtarınızın değerini Azure Sentinel Barracuda bağlayıcı sayfasından kopyalayın.
1. Barracuda etkinlikleri için Log Analytics'teki ilgili şemayı kullanmak için **CommonSecurityLog** ve **barracuda_CL**arayın.


## <a name="validate-connectivity"></a>Bağlantıyı doğrulama

Günlüklerinizin Log Analytics'te görünmeye başlaması 20 dakikadan fazla sürebilir. 



## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, Barracuda cihazlarını Azure Sentinel'e nasıl bağlayabileceğinizi öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Verilerinize ve olası tehditlere](quickstart-get-visibility.md)nasıl görünürlük elde edebilirsiniz öğrenin.
- Azure [Sentinel ile tehditleri algılamaya](tutorial-detect-threats-built-in.md)başlayın.
- Verilerinizi izlemek için [çalışma kitaplarını kullanın.](tutorial-monitor-your-data.md)


