---
title: Fortinet verilerini Azure Sentinel'e bağlayın| Microsoft Dokümanlar
description: Fortinet verilerini Azure Sentinel'e nasıl bağlayabilirsiniz öğrenin.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.assetid: add92907-0d7c-42b8-a773-f570f2d705ff
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/30/2019
ms.author: yelevin
ms.openlocfilehash: aabc95fcb8d3b32b89bb83f4f6892c3d40b03417
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77588204"
---
# <a name="connect-fortinet-to-azure-sentinel"></a>Fortinet'i Azure Sentinel'e bağlayın



Bu makalede, Fortinet cihazınızı Azure Sentinel'e nasıl bağlayabilirsiniz. Fortinet veri bağlayıcısı, Fortinet günlüklerinizi Azure Sentinel ile kolayca bağlamanızı, panoları görüntülemenizi, özel uyarılar oluşturmanızı ve araştırmayı geliştirmenizi sağlar. Fortinet'i Azure Sentinel'de kullanmak, kuruluşunuzun Internet kullanımı hakkında daha fazla bilgi sağlar ve güvenlik işlemi yeteneklerini geliştirir. 


 
## <a name="forward-fortinet-logs-to-the-syslog-agent"></a>Fortinet günlüklerini Syslog temsilcisine iletin

Fortinet'i CEF formatında Syslog iletilerini Syslog aracısı aracılığıyla Azure çalışma alanınıza iletecek şekilde yapılandırın.

1. Fortinet cihazınızdaki CLI'yi açın ve aşağıdaki komutları çalıştırın:

        config log syslogd setting
        set format cef
        set port 514
        set server <ip_address_of_Receiver>
        set status enable
        end

    - Sunucu **ip adresini** aracının IP adresiyle değiştirin.
    - **Syslog bağlantı noktasını** **514** olarak ayarlayın veya aracıüzerinde ayarlanan bağlantı noktası.
    - Erken FortiOS sürümlerinde CEF biçimini etkinleştirmek için **csv**devre dışı bırakma komut kümesini çalıştırmanız gerekebilir.
 
   > [!NOTE] 
   > Daha fazla bilgi için [Fortinet belge kitaplığına](https://aka.ms/asi-syslog-fortinet-fortinetdocumentlibrary)gidin. Sürümünüzü seçin ve **El Kitabı** ve **Günlük İleti Başvurusu'nu**kullanın.

1. Fortinet etkinlikleri için Azure Monitor Log Analytics'teki ilgili şemayı kullanmak için `CommonSecurityLog`.

1. ADIM 3'e devam [et: Bağlantıyı doğrulayın.](connect-cef-verify.md)


## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, Fortinet cihazlarını Azure Sentinel'e nasıl bağlayabileceğinizi öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Verilerinize ve olası tehditlere](quickstart-get-visibility.md)nasıl görünürlük elde edebilirsiniz öğrenin.
- Azure [Sentinel ile tehditleri algılamaya](tutorial-detect-threats-built-in.md)başlayın.
- Verilerinizi izlemek için [çalışma kitaplarını kullanın.](tutorial-monitor-your-data.md)


