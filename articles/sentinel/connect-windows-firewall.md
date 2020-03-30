---
title: Windows güvenlik duvarı verilerini Azure Sentinel'e bağlayın| Microsoft Dokümanlar
description: Windows güvenlik duvarı verilerini Azure Sentinel'e nasıl bağlayabilirsiniz öğrenin.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.assetid: 0e41f896-8521-49b8-a244-71c78d469bc3
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/23/2019
ms.author: yelevin
ms.openlocfilehash: 5d2f68261143c3fc5bbcda0b739af17251eeee63
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77588068"
---
# <a name="connect-windows-firewall"></a>Windows güvenlik duvarını bağlama



Windows güvenlik duvarı konektörü, Azure Sentinel çalışma alanınıza bağlıysa Windows güvenlik duvarı günlüklerinizi kolayca bağlamanızı sağlar. Bu bağlantı, panoları görüntülemenize, özel uyarılar oluşturmanıza ve araştırmayı geliştirmenize olanak tanır. Bu, kuruluşunuzun ağı hakkında daha fazla bilgi verir ve güvenlik işlem yeteneklerinizi geliştirir. Çözüm, Bir Log Analytics aracısının yüklü olduğu Windows makinelerinden Windows güvenlik duvarı olaylarını toplar. 


> [!NOTE]
> - Veriler, Azure Sentinel çalıştırdığınız çalışma alanının coğrafi konumunda depolanır.
> - Azure Sentinel ve Azure Güvenlik Merkezi aynı çalışma alanında toplanıyorsa, bu bağlayıcı aracılığıyla Windows Güvenlik Duvarı çözümünü etkinleştirmeye gerek yoktur. Yine de etkinleştirdiyseniz, yinelenen verilere neden olmaz. 

## <a name="enable-the-connector"></a>Konektörü etkinleştirme 

1. Azure Sentinel portalında **Veri bağlayıcılarını** seçin ve ardından **Windows güvenlik duvarı** döşemesini tıklatın. 
1.  Windows makineleriniz Azure'daysa:
    1. **Azure Windows sanal makinesinde Yükle aracısı'nı**tıklatın.
    1. Sanal **makineler** listesinde, Azure Sentinel'e aktarmak istediğiniz Windows makinesini seçin. Bunun bir Windows VM olduğundan emin olun.
    1. Bu VM için açılan pencerede **Bağlan'ı**tıklatın.  
    1. Windows güvenlik **duvarı bağlayıcısı** penceresinde **Etkinleştir'i** tıklatın. 

2. Windows makineniz Azure VM değilse:
    1. **Azure olmayan makinelerde Yükle aracıyı**tıklatın.
    1. Doğrudan **aracı** penceresinde, **Windows aracısını indir (64 bit)** veya **Windows aracısını (32 bit) indir'i**seçin.
    1. Aracıyı Windows makinenize yükleyin. Çalışma **Alanı Kimliği,** **Birincil anahtar**ve **İkincil anahtarı** kopyalayın ve yükleme sırasında istendiğinde bunları kullanın.

4. Hangi veri türlerini aktarmak istediğinizi seçin.
5. **Yükle çözüme**tıklayın.
6. Windows güvenlik duvarı için Log Analytics'teki ilgili şemayı kullanmak için **SecurityEvent'i**arayın.

## <a name="validate-connectivity"></a>Bağlantıyı doğrulama

Günlüklerinizin Log Analytics'te görünmeye başlaması 20 dakikadan fazla sürebilir. 



## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, Windows güvenlik duvarını Azure Sentinel' e nasıl bağlayırıla Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Verilerinize ve olası tehditlere](quickstart-get-visibility.md)nasıl görünürlük elde edebilirsiniz öğrenin.
- Azure [Sentinel ile tehditleri algılamaya](tutorial-detect-threats-built-in.md)başlayın.

