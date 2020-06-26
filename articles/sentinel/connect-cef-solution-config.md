---
title: CEF verilerini Azure Sentinel önizlemesine bağlamak için Güvenlik çözümünüzü yapılandırın | Microsoft Docs
description: Güvenlik çözümünüzü, CEF verilerini Azure Sentinel 'e bağlamak üzere nasıl yapılandıracağınızı öğrenin.
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
ms.openlocfilehash: 1c25e48bd46f0d37330f693cb4d6538e7bc29c4b
ms.sourcegitcommit: bf8c447dada2b4c8af017ba7ca8bfd80f943d508
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2020
ms.locfileid: "85367249"
---
# <a name="step-2-configure-your-security-solution-to-send-cef-messages"></a>2. Adım: Güvenlik çözümünüzü CEF iletileri gönderecek şekilde yapılandırma

Bu adımda, CEF aracısına Günlükler göndermek için güvenlik çözümünüz üzerinde gerekli yapılandırma değişikliklerini gerçekleştirirsiniz.

## <a name="configure-a-solution-with-a-connector"></a>Bağlayıcı ile çözüm yapılandırma

Güvenlik çözümünüz zaten mevcut bir bağlayıcınız varsa, bağlayıcıya özgü yönergeleri aşağıdaki gibi kullanın:

- [AI Vektöri algılama](connect-ai-vectra-detect.md)
- [Denetim Noktası](connect-checkpoint.md)
- [Cisco](connect-cisco.md)
- [ExtraHop Reveal(x)](connect-extrahop.md)
- [F5 ASM](connect-f5.md)  
- [Fortinet](connect-fortinet.md)
- [One Identity Safeguard](connect-one-identity.md)
- [Palo Alto Networks](connect-paloalto.md)
- [Trend Micro Deep Security](connect-trend-micro.md)
- [Zscaler](connect-zscaler.md)   

## <a name="configure-any-other-solution"></a>Diğer herhangi bir çözümü yapılandırma

Belirli güvenlik çözümünüz için bir bağlayıcı yoksa, günlükleri CEF aracısına iletmek için aşağıdaki genel yönergeleri kullanın.

1. Çözümünüzü CEF iletileri gönderecek şekilde yapılandırma adımları için ilgili yapılandırma makalesine gidin. Çözümünüz listede yoksa, Gereç bu değerleri, Log Analytics aracısına bağlı olarak, gerecin gerekli biçimdeki Azure Sentinel Syslog aracısına göndermesi için ayarlamanız gerekir. Bu parametreleri Azure Sentinel aracısında Syslog arka plan programı 'nda da değiştirdiğiniz sürece, aracısında değiştirebilirsiniz.
    - Protokol = TCP
    - Bağlantı noktası = 514
    - Biçim = CEF
    - IP adresi-CEF iletilerini bu amaçla ayrıldığınızdan sanal makinenin IP adresine gönderdiğinizden emin olun.

   > [!NOTE]
   > Bu çözüm Syslog RFC 3164 veya RFC 5424 ' ü destekler.

1. CEF olayları için Log Analytics ilgili şemayı kullanmak için, araması yapın `CommonSecurityLog` .

1. 3. Adım: [bağlantıyı doğrulama](connect-cef-verify.md)adımına geçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu belgede CEF gereçlerini Azure Sentinel 'e bağlamayı öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Verilerinize nasıl görünürlük alabileceğinizi ve olası tehditleri](quickstart-get-visibility.md)öğrenin.
- [Azure Sentinel ile tehditleri algılamaya](tutorial-detect-threats.md)başlayın.
