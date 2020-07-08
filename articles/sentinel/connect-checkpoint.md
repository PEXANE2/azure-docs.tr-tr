---
title: Denetim noktası verilerini Azure Sentinel 'e bağlama | Microsoft Docs
description: Syslog Aracısı aracılığıyla CEF biçimindeki syslog iletilerini Azure Sentinel çalışma alanınıza iletmek için denetim noktası gerecinizi yapılandırın.
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
ms.openlocfilehash: 32cbdabef4d89c7fabb47a52ebf0589690b7ab33
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85566056"
---
# <a name="connect-check-point-to-azure-sentinel"></a>Denetim noktasını Azure Sentinel 'e bağlama



Bu makalede, denetim noktası gerecinizi Azure Sentinel 'e bağlama açıklanmaktadır. Check Point Data Connector, denetim noktası günlüklerinizi kolayca Azure Sentinel 'e bağlamanıza, panoları görüntülemenize, özel uyarılar oluşturmanıza ve araştırmaya olanak tanır. Azure Sentinel 'de Check Point 'in kullanılması, kuruluşunuzun Internet kullanımı hakkında daha fazla öngörü sağlar ve güvenlik işlemi yeteneklerini geliştirir. 

## <a name="forward-check-point-logs-to-the-syslog-agent"></a>Denetim noktası günlüklerini Syslog aracısına ilet

Syslog Aracısı aracılığıyla CEF biçimindeki syslog iletilerini Azure çalışma alanınıza iletmek için denetim noktası gerecinizi yapılandırın.

1. [Check Point günlük dışarı aktarmaya](https://aka.ms/asi-syslog-checkpoint-forwarding)gidin.
1. **Temel dağıtıma** aşağı kaydırın ve aşağıdaki yönergeleri kullanarak bağlantıyı kurmak için yönergeleri izleyin:
   - **Syslog bağlantı noktasını** **514** olarak veya aracıda ayarladığınız bağlantı noktasını ayarlayın.
     - CLı 'daki **Name** ve **target-Server IP adresini** Syslog Aracısı adı ve IP adresi ile değiştirin.
     - Biçimini **CEF**olarak ayarlayın.
1. R 77.30 veya R 80.10 sürümünü kullanıyorsanız, **yüklemelere** kadar ilerleyin ve sürümünüz Için bir günlük dışarı aktarıcı yükleme yönergelerini izleyin.
1. 3. [Adım: bağlantıyı doğrulama adımına](connect-cef-verify.md)geçin.
 

## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, Check Point gereçlerini Azure Sentinel 'e bağlamayı öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Bağlantıyı doğrulayın](connect-cef-verify.md).
- [Azure Sentinel ile tehditleri algılamaya](tutorial-detect-threats-built-in.md)başlayın.
- Verilerinizi izlemek için [çalışma kitaplarını kullanın](tutorial-monitor-your-data.md) .


