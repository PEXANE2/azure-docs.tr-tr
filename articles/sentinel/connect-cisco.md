---
title: Cisco verilerini Azure Sentinel'e bağlayın| Microsoft Dokümanlar
description: Cisco verilerini Azure Sentinel'e nasıl bağlayabilirsiniz öğrenin.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.assetid: 62029b5c-29d3-4336-8a22-a9db8214eb7e
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/30/2019
ms.author: yelevin
ms.openlocfilehash: 264dab4fabc890ffe03d911f8ee3c08ef51375d8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77588408"
---
# <a name="connect-cisco-asa-to-azure-sentinel"></a>Cisco ASA'yı Azure Sentinel'e bağlayın



Bu makalede, Cisco ASA cihazınızı Azure Sentinel'e nasıl bağlayabilirsiniz. Cisco ASA veri konektörü, Cisco ASA günlüklerinizi Azure Sentinel ile kolayca bağlamanızı, panoları görüntülemenizi, özel uyarılar oluşturmanızı ve araştırmayı geliştirmenizi sağlar. Azure Sentinel'de Cisco ASA'yı kullanmak, kuruluşunuzun Internet kullanımı hakkında daha fazla bilgi sağlar ve güvenlik işlemi yeteneklerini geliştirir. 



## <a name="forward-cisco-asa-logs-to-the-syslog-agent"></a>Cisco ASA günlüklerini Syslog temsilcisine iletme

Cisco ASA CEF'i desteklemez, bu nedenle günlükler Syslog olarak gönderilir ve Azure Sentinel aracısı bunları CEF günlükleri gibi nasıl ayrışdıracaklarını bilir. Syslog aracısı aracılığıyla Syslog iletilerini Azure çalışma alanınıza iletecek şekilde Cisco ASA'yı yapılandırın:

1. Harici [bir Syslog sunucusuna Syslog iletileri gönder'e](https://aka.ms/asi-syslog-cisco-forwarding)gidin ve bağlantıyı kurmak için yönergeleri izleyin. İstendiğinde bu parametreleri kullanın:
    - **Bağlantı noktasını** 514 olarak ayarlayın veya aracıda ayarladığınız bağlantı noktası.
    - **syslog_ip** aracının IP adresine ayarlayın.

1. Cisco etkinlikleri için Log Analytics'teki ilgili şemayı `CommonSecurityLog`kullanmak için .

1. ADIM 3'e devam [et: Bağlantıyı doğrulayın.](connect-cef-verify.md)




## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, Cisco ASA cihazlarını Azure Sentinel'e nasıl bağlayabileceğinizi öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Verilerinize ve olası tehditlere](quickstart-get-visibility.md)nasıl görünürlük elde edebilirsiniz öğrenin.
- Azure [Sentinel ile tehditleri algılamaya](tutorial-detect-threats-built-in.md)başlayın.
- Verilerinizi izlemek için [çalışma kitaplarını kullanın.](tutorial-monitor-your-data.md)


