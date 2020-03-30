---
title: F5 verilerini Azure Sentinel'e bağlayın| Microsoft Dokümanlar
description: F5 verilerini Azure Sentinel'e nasıl bağlayabilirsiniz öğrenin.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.assetid: 0001cad6-699c-4ca9-b66c-80c194e439a5
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/30/2019
ms.author: yelevin
ms.openlocfilehash: 6f33cecca1c67f91d0f2be64ab156f45ee500521
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77588272"
---
# <a name="connect-f5-to-azure-sentinel"></a>F5'i Azure Sentinel'e bağlayın

Bu makalede, F5 cihazınızı Azure Sentinel'e nasıl bağlayabilirsiniz. F5 veri bağlayıcısı, F5 günlüklerinizi Azure Sentinel ile kolayca bağlamanızı, panoları görüntülemenizi, özel uyarılar oluşturmanızı ve araştırmayı geliştirmenizi sağlar. Azure Sentinel'de F5'i kullanmak, kuruluşunuzun Internet kullanımı hakkında daha fazla bilgi sağlar ve güvenlik işlemi yeteneklerini geliştirir. 

## <a name="configure-your-f5-to-send-cef-messages"></a>CEF iletileri göndermek için F5'inizi yapılandırın

1. [F5 Yapılandırma Uygulama Güvenliği Olay Günlüğe](https://techdocs.f5.com/kb/en-us/products/big-ip_asm/manuals/product/asm-implementations-11-5-0/12.html)gidin ve aşağıdaki yönergeleri kullanarak uzaktan günlüğe kaydetmeyi ayarlamak için yönergeleri izleyin:
   - Uzak **depolama türünü** **CEF**olarak ayarlayın.
   - **Protokolü** **TCP**olarak ayarlayın.
   - IP **adresini** Syslog sunucu IP adresine ayarlayın.
   - Bağlantı **noktası numarasını** **514**olarak veya aracınızı kullanmak üzere ayarladığınız bağlantı noktası olarak ayarlayın.
   - Maksimum Sorgu **Dizesi Boyutunu** aracınızda ayarladığınız boyuta ayarlayabilirsiniz.

1. CEF etkinlikleri için Log Analytics'teki ilgili şemayı `CommonSecurityLog`kullanmak için .

1. ADIM 3'e devam [et: Bağlantıyı doğrulayın.](connect-cef-verify.md)


## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, F5'i Azure Sentinel'e nasıl bağlayabileceğinizi öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Verilerinize ve olası tehditlere](quickstart-get-visibility.md)nasıl görünürlük elde edebilirsiniz öğrenin.
- Azure [Sentinel ile tehditleri algılamaya](tutorial-detect-threats.md)başlayın.
- Verilerinizi izlemek için [çalışma kitaplarını kullanın.](tutorial-monitor-your-data.md)

