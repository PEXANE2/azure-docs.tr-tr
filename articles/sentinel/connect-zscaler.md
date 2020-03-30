---
title: Zscaler verilerini Azure Sentinel'e bağlayın| Microsoft Dokümanlar
description: Zscaler verilerini Azure Sentinel'e nasıl bağlayabilirsiniz öğrenin.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/30/2019
ms.author: yelevin
ms.openlocfilehash: cc784afe5db64ccc4aad13fae7a2fa748e4befa3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77588000"
---
# <a name="connect-zscaler-internet-access-to-azure-sentinel"></a>Zscaler Internet Erişimini Azure Sentinel'e Bağlayın

> [!IMPORTANT]
> Azure Sentinel'deki Zscaler veri bağlayıcısı şu anda genel önizlemededir.
> Bu özellik bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için Microsoft [Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.

Bu makalede, Zscaler Internet Access cihazınızı Azure Sentinel'e nasıl bağlayabilirsiniz. Zscaler veri bağlayıcısı, Zscaler Internet Access (ZIA) günlüklerinizi Azure Sentinel ile kolayca bağlamanızı, panoları görüntülemenizi, özel uyarılar oluşturmanızı ve araştırmayı geliştirmenizi sağlar. Azure Sentinel'de Zscaler'i kullanmak, kuruluşunuzun Internet kullanımı hakkında daha fazla bilgi sağlar ve güvenlik işlemi yeteneklerini geliştirir. 


## <a name="configure-your-zscaler-to-send-cef-messages"></a>Zscaler'inizi CEF iletileri gönderecek şekilde yapılandırın

1. Zscaler cihazında, cihazın gerekli formattaki günlükleri Log Analytics aracısını temel alan Azure Sentinel Syslog aracısına göndermesi için bu değerleri ayarlamanız gerekir. Azure Sentinel aracısındaki Syslog daemon'da da değiştirdiğiniz sürece, bu parametreleri cihazınızda değiştirebilirsiniz.
    - Protokol = TCP
    - Bağlantı Noktası = 514
    - Biçim = CEF
    - IP adresi - CEF mesajlarını bu amaçla adadığınız sanal makinenin IP adresine gönderdiğinden emin olun.
 Daha fazla bilgi için [Zscaler ve Azure Sentinel Dağıtım Kılavuzu'na](https://aka.ms/ZscalerCEFInstructions)bakın.
 
   > [!NOTE]
   > Bu çözüm Syslog RFC 3164 veya RFC 5424'u destekler.


1. CEF etkinlikleri için Log Analytics'teki ilgili şemayı `CommonSecurityLog`kullanmak için .
1. ADIM 3'e devam [et: Bağlantıyı doğrulayın.](connect-cef-verify.md)


## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, Zscaler Internet Access'i Azure Sentinel'e nasıl bağlayabileceğinizi öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Verilerinize ve olası tehditlere](quickstart-get-visibility.md)nasıl görünürlük elde edebilirsiniz öğrenin.
- Azure [Sentinel ile tehditleri algılamaya](tutorial-detect-threats.md)başlayın.
- Verilerinizi izlemek için [çalışma kitaplarını kullanın.](tutorial-monitor-your-data.md)


