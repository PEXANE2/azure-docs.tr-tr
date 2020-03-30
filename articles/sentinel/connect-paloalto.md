---
title: Palo Alto Networks verilerini Azure Sentinel'e bağlayın| Microsoft Dokümanlar
description: Palo Alto Networks verilerini Azure Sentinel'e nasıl bağlayabilirsiniz öğrenin.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.assetid: a4b21d67-1a72-40ec-bfce-d79a8707b4e1
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/30/2019
ms.author: yelevin
ms.openlocfilehash: a79b7a1448e1decb377aa0072261df068c366567
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77588136"
---
# <a name="connect-palo-alto-networks-to-azure-sentinel"></a>Palo Alto Ağlarını Azure Sentinel'e bağlayın



Bu makalede, Palo Alto Networks cihazınızı Azure Sentinel'e nasıl bağlayabilirsiniz. Palo Alto Networks veri bağlayıcısı, Azure Sentinel ile Palo Alto Networks günlüklerinizi kolayca bağlamanızı, panoları görüntülemenizi, özel uyarılar oluşturmanızı ve araştırmayı geliştirmenizi sağlar. Azure Sentinel'de Palo Alto Ağlarını kullanmak, kuruluşunuzun Internet kullanımı hakkında daha fazla bilgi sağlar ve güvenlik işlemi yeteneklerini geliştirir. 


## <a name="forward-palo-alto-networks-logs-to-the-syslog-agent"></a>Forward Palo Alto Networks, Syslog temsilcisine günlük ler

Syslog aracısı aracılığıyla CEF formatında Syslog iletilerini Azure çalışma alanınıza iletmek için Palo Alto Ağlarını yapılandırın:
1.  Ortak [Etkinlik Formatı (CEF) Yapılandırma Kılavuzları'na](https://docs.paloaltonetworks.com/resources/cef) gidin ve cihaz tipiniz için pdf'yi indirin. CEF etkinliklerini toplamak için Palo Alto Networks cihazınızı kurmak için kılavuzdaki tüm talimatları izleyin. 

1.  [Syslog izlemeyi Yapılandırma'ya](https://aka.ms/asi-syslog-paloalto-forwarding) gidin ve Palo Alto Networks cihazınızdan Azure Sentinel'e CEF olayını yapılandırmak için 2 ve 3 adımlarını izleyin.

    1. **Syslog sunucu biçimini** **BSD**olarak ayarladıklarından emin olun.

       > [!NOTE]
       > PDF'deki kopyalama/yapıştır işlemleri metni değiştirebilir ve rastgele karakterler ekleyebilir. Bunu önlemek için metni bir düzenleyiciye kopyalayın ve bu örnekte de görebileceğiniz gibi, yapıştırmadan önce günlük biçimini kırabilecek karakterleri kaldırın.
 
        ![CEF metin kopyalama sorunu](./media/connect-cef/paloalto-text-prob1.png)

1. Palo Alto Networks etkinlikleri için Log Analytics'teki ilgili şemayı kullanmak için **CommonSecurityLog'u**arayın.

1. ADIM 3'e devam [et: Bağlantıyı doğrulayın.](connect-cef-verify.md)




## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, Palo Alto Networks cihazlarını Azure Sentinel'e nasıl bağlayabileceğinizi öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Verilerinize ve olası tehditlere](quickstart-get-visibility.md)nasıl görünürlük elde edebilirsiniz öğrenin.
- Azure [Sentinel ile tehditleri algılamaya](tutorial-detect-threats-built-in.md)başlayın.
- Verilerinizi izlemek için [çalışma kitaplarını kullanın.](tutorial-monitor-your-data.md)


