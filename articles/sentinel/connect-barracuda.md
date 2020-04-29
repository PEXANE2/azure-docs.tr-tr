---
title: Barçcuda verilerini Azure Sentinel 'e bağlama | Microsoft Docs
description: Barbcuda verilerini Azure Sentinel 'e bağlamayı öğrenin.
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
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "77588493"
---
# <a name="connect-your-barracuda-appliance"></a>Barvacuda gerecinizi bağlama 



Barbcuda Web uygulaması güvenlik duvarı (WAF) Bağlayıcısı, Bariscuda günlüklerinizi Azure Sentinel ile kolayca bağlamanıza, panoları görüntüleyebilir, özel uyarılar oluşturabilir ve araştırmayı iyileştirmenize olanak tanır. Bu, kuruluşunuzun ağı hakkında daha ayrıntılı bilgi verir ve güvenlik işlemi yeteneklerini geliştirir. Azure Sentinel, sorunsuz tümleştirme sağlamak için **barnacuda** ve Log Analytics Aracısı arasındaki yerel tümleştirmeden yararlanır. 


> [!NOTE]
> Veriler, Azure Sentinel çalıştırdığınız çalışma alanının coğrafi konumunda depolanır.

## <a name="configure-and-connect-barracuda-waf"></a>Barçcuda WAF 'yi yapılandırma ve bağlama
Barbcuda Web uygulaması güvenlik duvarı, Log Analytics Aracısı aracılığıyla günlükleri doğrudan Azure Sentinel 'e tümleştirebilir ve dışarı aktarabilir.
1. [Barbcuda WAF yapılandırma akışına](https://campus.barracuda.com/product/webapplicationfirewall/doc/73696965/configure-the-barracuda-web-application-firewall-to-integrate-with-the-oms-server-and-export-logs/)gidin ve aşağıdaki parametreleri kullanarak bağlantıyı ayarlamak için yönergeleri izleyin:
    - **Çalışma alanı kimliği**: çalışma alanı kimliğinizin değerini Azure Sentinel Barraycuda bağlayıcı sayfasından kopyalayın.
    - **Birincil anahtar**: Azure Sentinel Barkıcuda bağlayıcı sayfasından birincil anahtarınızın değerini kopyalayın.
1. Barbcuda olayları için Log Analytics ilgili şemayı kullanmak için, **Commonsecuritylog** ve **barracuda_CL**aratın.


## <a name="validate-connectivity"></a>Bağlantıyı doğrula

Günlüklerinizin Log Analytics görünene kadar 20 dakikadan bu kadar bir zaman çıkabilir. 



## <a name="next-steps"></a>Sonraki adımlar
Bu belgede Barbcuda gereçlerini Azure Sentinel 'e bağlamayı öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Verilerinize nasıl görünürlük alabileceğinizi ve olası tehditleri](quickstart-get-visibility.md)öğrenin.
- [Azure Sentinel ile tehditleri algılamaya](tutorial-detect-threats-built-in.md)başlayın.
- Verilerinizi izlemek için [çalışma kitaplarını kullanın](tutorial-monitor-your-data.md) .


