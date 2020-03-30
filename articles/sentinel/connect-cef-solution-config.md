---
title: CEF verilerini Azure Sentinel Preview'a bağlamak için güvenlik çözümünüzü yapılandırın| Microsoft Dokümanlar
description: CEF verilerini Azure Sentinel'e bağlamak için güvenlik çözümünüzü nasıl yapılandıracaklarınızı öğrenin.
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
ms.openlocfilehash: bdb76954b1db8135d8a36d6658bb7fff274ac126
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77588459"
---
# <a name="step-2-configure-your-security-solution-to-send-cef-messages"></a>ADIM 2: CEF iletileri göndermek için güvenlik çözümünüzü yapılandırın

Bu adımda, CEF aracısına günlük göndermek için güvenlik çözümünüzüzde gerekli yapılandırma değişikliklerini gerçekleştireceksiniz.

## <a name="configure-a-solution-with-a-connector"></a>Bir çözümü konektörle yapılandırma

Güvenlik çözümünüzzaten varolan bir bağlayıcıya sahipse, bağlayıcıya özgü yönergeleri aşağıdaki gibi kullanın:

- [Denetim Noktası](connect-checkpoint.md)
- [Cisco](connect-cisco.md)
- [ExtraHop Reveal(x)](connect-extrahop.md)
- [F5](connect-f5.md)  
- [Fortinet](connect-fortinet.md)
- [One Identity Safeguard](connect-one-identity.md)
- [Palo Alto Networks](connect-paloalto.md)
- [Trend Micro Deep Security](connect-trend-micro.md)
- [Zscaler](connect-zscaler.md)   

## <a name="configure-any-other-solution"></a>Diğer çözümleri yapılandırma
Özel güvenlik çözümünüz için bir bağlayıcı yoksa, günlükleri CEF aracısına iletmek için aşağıdaki genel yönergeleri kullanın.

1. CEF iletileri göndermek için çözümünüzü yapılandırma ya da yapılandırma adımlarını almak için belirli yapılandırma makalesine gidin. Çözümünüz listelenmemişse, cihazda bu değerleri ayarlamalısınız, böylece cihaz Gerekli günlükleri Log Analytics aracısını temel alan Azure Sentinel Syslog aracısına gerekli formatta gönderir. Azure Sentinel aracısındaki Syslog daemon'da da değiştirdiğiniz sürece, bu parametreleri cihazınızda değiştirebilirsiniz.
    - Protokol = TCP
    - Bağlantı Noktası = 514
    - Biçim = CEF
    - IP adresi - CEF mesajlarını bu amaçla adadığınız sanal makinenin IP adresine gönderdiğinden emin olun.

   > [!NOTE]
   > Bu çözüm Syslog RFC 3164 veya RFC 5424'u destekler.


1. CEF etkinlikleri için Log Analytics'teki ilgili şemayı `CommonSecurityLog`kullanmak için .

1. ADIM 3'e devam [edin: bağlantıyı doğrulayın.](connect-cef-verify.md)

## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, CEF cihazlarını Azure Sentinel'e nasıl bağlayabileceğinizi öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Verilerinize ve olası tehditlere](quickstart-get-visibility.md)nasıl görünürlük elde edebilirsiniz öğrenin.
- Azure [Sentinel ile tehditleri algılamaya](tutorial-detect-threats.md)başlayın.

