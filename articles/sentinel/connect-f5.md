---
title: F5 verilerini Azure Sentinel 'e bağlama | Microsoft Docs
description: F5 verilerini Azure Sentinel 'e bağlamayı öğrenin.
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
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "77588272"
---
# <a name="connect-f5-to-azure-sentinel"></a>F5 'i Azure Sentinel 'e bağlama

Bu makalede F5 gerecinizi Azure Sentinel 'e nasıl bağlayabileceğiniz açıklanır. F5 Data Connector, panoları görüntülemek, özel uyarılar oluşturmak ve araştırmayı geliştirmek için F5 günlüklerinizi Azure Sentinel ile kolayca bağlamanıza olanak tanır. Azure Sentinel 'de F5 'in kullanılması, kuruluşunuzun Internet kullanımı hakkında daha fazla öngörü sağlar ve güvenlik işlemi yeteneklerini geliştirir. 

## <a name="configure-your-f5-to-send-cef-messages"></a>F5 'nizi CEF iletileri gönderecek şekilde yapılandırma

1. F5 ' e giderek [uygulama güvenliği olay günlüğünü yapılandırma](https://techdocs.f5.com/kb/en-us/products/big-ip_asm/manuals/product/asm-implementations-11-5-0/12.html)' ya gidin ve aşağıdaki yönergeleri kullanarak uzaktan günlüğe kaydetmeyi ayarlamak için yönergeleri izleyin:
   - **Uzak depolama türünü** **CEF**olarak ayarlayın.
   - **Protokolü** **TCP**olarak ayarlayın.
   - **IP adresini** Syslog sunucusu IP adresi olarak ayarlayın.
   - **Bağlantı noktası numarasını** **514**olarak veya aracıyı kullanmak için ayarladığınız bağlantı noktasını ayarlayın.
   - **En büyük sorgu dizesi boyutunu** , aracısında ayarladığınız boyuta ayarlayabilirsiniz.

1. CEF olayları için Log Analytics ilgili şemayı kullanmak için, araması yapın `CommonSecurityLog`.

1. 3. [Adım: bağlantıyı doğrulama adımına](connect-cef-verify.md)geçin.


## <a name="next-steps"></a>Sonraki adımlar
Bu belgede F5 'i Azure Sentinel 'e bağlamayı öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Verilerinize nasıl görünürlük alabileceğinizi ve olası tehditleri](quickstart-get-visibility.md)öğrenin.
- [Azure Sentinel ile tehditleri algılamaya](tutorial-detect-threats.md)başlayın.
- Verilerinizi izlemek için [çalışma kitaplarını kullanın](tutorial-monitor-your-data.md) .

