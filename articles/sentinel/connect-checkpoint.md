---
title: Denetim Noktası verilerini Azure Sentinel'e bağlayın| Microsoft Dokümanlar
description: Check Point verilerini Azure Sentinel'e nasıl bağlayabilirsiniz öğrenin.
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
ms.openlocfilehash: 70836ec557eff1be035d92e8e7db30a882e05fc6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77588425"
---
# <a name="connect-check-point-to-azure-sentinel"></a>Denetim Noktasını Azure Sentinel'e Bağla



Bu makalede, Check Point cihazınızı Azure Sentinel'e nasıl bağlayabilirsiniz. Kontrol Noktası veri bağlayıcısı, Azure Sentinel ile Check Point günlüklerinizi kolayca bağlamanızı, panoları görüntülemenizi, özel uyarılar oluşturmanızı ve araştırmayı geliştirmenizi sağlar. Azure Sentinel'de Check Point'i kullanmak, kuruluşunuzun Internet kullanımı hakkında daha fazla bilgi sağlar ve güvenlik işlemi yeteneklerini geliştirir. 

## <a name="forward-check-point-logs-to-the-syslog-agent"></a>Syslog aracısına İleri Kontrol Noktası günlükleri

Çek Noktası cihazınızı CEF formatında Syslog iletilerini Syslog aracısı aracılığıyla Azure çalışma alanınıza iletecek şekilde yapılandırın.

1. Kontrol [Noktası Günlüğü Dışa Aktar'a](https://aka.ms/asi-syslog-checkpoint-forwarding)gidin.
1. **Temel Dağıtım'a** gidin ve aşağıdaki yönergeleri kullanarak bağlantıyı kurmak için yönergeleri izleyin:
   - **Syslog bağlantı noktasını** **514** olarak ayarlayın veya aracıüzerinde ayarladığınız bağlantı noktası.
     - CLI'deki **ad** ve **hedef sunucu IP adresini** Syslog aracısı adı ve IP adresiyle değiştirin.
     - Biçimi **CEF**olarak ayarlayın.
1. R77.30 veya R80.10 sürümünü kullanıyorsanız, **Yüklemeler'e** gidin ve sürümünüz için bir Log Exporter yükleme yönergelerini izleyin.
1. ADIM 3'e devam [et: Bağlantıyı doğrulayın.](connect-cef-verify.md)
 

## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, Check Point cihazlarını Azure Sentinel'e nasıl bağlayabileceğinizi öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Bağlantıyı doğrulayın.](connect-cef-verify.md)
- Azure [Sentinel ile tehditleri algılamaya](tutorial-detect-threats-built-in.md)başlayın.
- Verilerinizi izlemek için [çalışma kitaplarını kullanın.](tutorial-monitor-your-data.md)


