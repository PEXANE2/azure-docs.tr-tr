---
title: Zscaler verilerini Azure Sentinel 'e bağlama | Microsoft Docs
description: Zscaler verilerini Azure Sentinel 'e bağlamayı öğrenin.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/30/2019
ms.author: rkarlin
ms.openlocfilehash: d64ec43c255813b99d7ceefc1e526884c99f1873
ms.sourcegitcommit: 003e73f8eea1e3e9df248d55c65348779c79b1d6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/02/2020
ms.locfileid: "75610395"
---
# <a name="connect-zscaler-internet-access-to-azure-sentinel"></a>Zscaler Internet erişimini Azure Sentinel 'e bağlama

> [!IMPORTANT]
> Azure Sentinel 'deki Zscaler veri Bağlayıcısı Şu anda genel önizlemededir.
> Bu özellik, bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Bu makalede, Zscaler Internet erişim gerecinizi Azure Sentinel 'e nasıl bağlayabileceğiniz açıklanır. Zscaler veri Bağlayıcısı, Azure Sentinel ile Zscaler Internet erişimi (ZIA) günlüklerinizi kolayca bağlamanıza, panoları görüntülemenize, özel uyarılar oluşturmaya ve araştırmaya olanak tanır. Azure Sentinel 'de Zscaler 'ın kullanılması, kuruluşunuzun Internet kullanımı hakkında daha fazla öngörü sağlar ve güvenlik işlemi yeteneklerini geliştirir. 


## <a name="configure-your-zscaler-to-send-cef-messages"></a>Zscaler 'nizi CEF iletileri gönderecek şekilde yapılandırma

1. Zscaler aracısında, gerecin gerekli günlükleri Log Analytics aracısına bağlı olarak Azure Sentinel Syslog aracısına göndermesi için bu değerleri ayarlamanız gerekir. Bu parametreleri Azure Sentinel aracısında Syslog arka plan programı 'nda da değiştirdiğiniz sürece, aracısında değiştirebilirsiniz.
    - Protokol = TCP
    - Bağlantı noktası = 514
    - Biçim = CEF
    - IP adresi-CEF iletilerini bu amaçla ayrıldığınızdan sanal makinenin IP adresine gönderdiğinizden emin olun.
 Daha fazla bilgi için bkz. [Zscaler ve Azure Sentinel dağıtım kılavuzu](https://aka.ms/ZscalerCEFInstructions).
 
   > [!NOTE]
   > Bu çözüm Syslog RFC 3164 veya RFC 5424 ' ü destekler.


1. CEF olayları için Log Analytics ilgili şemayı kullanmak için, `CommonSecurityLog`aratın.
1. 3\. [Adım: bağlantıyı doğrulama adımına](connect-cef-verify.md)geçin.


## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, Zscaler Internet erişimini Azure Sentinel 'e bağlamayı öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Verilerinize nasıl görünürlük alabileceğinizi ve olası tehditleri](quickstart-get-visibility.md)öğrenin.
- [Azure Sentinel ile tehditleri algılamaya](tutorial-detect-threats.md)başlayın.
- Verilerinizi izlemek için [çalışma kitaplarını kullanın](tutorial-monitor-your-data.md) .


