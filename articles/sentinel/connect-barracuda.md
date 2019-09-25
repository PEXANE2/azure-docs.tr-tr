---
title: Barçcuda verilerini Azure Sentinel 'e bağlama | Microsoft Docs
description: Barbcuda verilerini Azure Sentinel 'e bağlamayı öğrenin.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: 3b33b4aa-7286-4d79-b461-8e1812edc2e1
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/23/2019
ms.author: rkarlin
ms.openlocfilehash: b3ca93d9e70456d25d5f78b2ca1fde8e4ea24f8d
ms.sourcegitcommit: 992e070a9f10bf43333c66a608428fcf9bddc130
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2019
ms.locfileid: "71240196"
---
# <a name="connect-your-barracuda-appliance"></a>Barvacuda gerecinizi bağlama 



Barbcuda Web uygulaması güvenlik duvarı (WAF) Bağlayıcısı, Bariscuda günlüklerinizi Azure Sentinel ile kolayca bağlamanıza, panoları görüntüleyebilir, özel uyarılar oluşturabilir ve araştırmayı iyileştirmenize olanak tanır. Bu, kuruluşunuzun ağı hakkında daha ayrıntılı bilgi verir ve güvenlik işlemi yeteneklerini geliştirir. Azure Sentinel, sorunsuz tümleştirme sağlamak için **barnacuda** ve Log Analytics Aracısı arasındaki yerel tümleştirmeden yararlanır. 


> [!NOTE]
> Veriler, Azure Sentinel çalıştırdığınız çalışma alanının coğrafi konumunda depolanır.

## <a name="configure-and-connect-barracuda-waf"></a>Barracuda WAF'yi yapılandırın ve bağlayın
Barbcuda Web uygulaması güvenlik duvarı, Log Analytics Aracısı aracılığıyla günlükleri doğrudan Azure Sentinel 'e tümleştirebilir ve dışarı aktarabilir.
1. [Barbcuda WAF yapılandırma akışına](https://campus.barracuda.com/product/webapplicationfirewall/doc/73696965/configure-the-barracuda-web-application-firewall-to-integrate-with-the-oms-server-and-export-logs/)gidin ve aşağıdaki parametreleri kullanarak bağlantıyı ayarlamak için yönergeleri izleyin:
    - **Çalışma alanı kimliği**: çalışma alanı kimliğinizin değerini Azure Sentinel Barraycuda bağlayıcı sayfasından kopyalayın.
    - **Birincil anahtar**: Azure Sentinel Barkıcuda bağlayıcı sayfasından birincil anahtarınızın değerini kopyalayın.
2. Azure Sentinel portalında, Azure Sentinel 'i dağıttığınız çalışma alanına gidin ve satırın sonundaki üç nokta (...) simgesini seçin ve **Gelişmiş ayarlar**' ı seçin. 
1. **Veri** ve ardından **Syslog**öğesini seçin.
1. Barbcuda ' de ayarladığınız tesis ' ın bulunduğundan emin olun ve önem derecesi ' ni ayarlayıp **Kaydet**' e tıklayın.
6. Barbcuda olayları için Log Analytics ilgili şemayı kullanmak için, **Commonsecuritylog** ve **barracuda_CL**için arama yapın.


## <a name="validate-connectivity"></a>Bağlantıyı doğrula

Günlüklerinizin Log Analytics görünene kadar 20 dakikadan bu kadar bir zaman çıkabilir. 



## <a name="next-steps"></a>Sonraki adımlar
Bu belgede Barbcuda gereçlerini Azure Sentinel 'e bağlamayı öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Verilerinize nasıl görünürlük alabileceğinizi ve olası tehditleri](quickstart-get-visibility.md)öğrenin.
- [Azure Sentinel ile tehditleri algılamaya](tutorial-detect-threats-built-in.md)başlayın.

