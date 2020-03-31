---
title: CEF verilerini Azure Sentinel Preview'a bağlamak için aracıyı dağıtın| Microsoft Dokümanlar
description: CEF verilerini Azure Sentinel'e bağlamak için aracıyı nasıl dağıtılayabilirsiniz öğrenin.
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
ms.date: 11/26/2019
ms.author: yelevin
ms.openlocfilehash: b0c9335357cb793ea76e1dbe68575f716a50372a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77588476"
---
# <a name="step-1-deploy-the-agent"></a>Adım 1: Aracıyı dağıtın


Bu adımda, Azure Sentinel ile güvenlik çözümünüz arasında proxy olarak hareket edecek Linux makinesini seçmeniz gerekir. Proxy makinesinde bir komut dosyası çalıştırmak zorunda kalacak:
- Log Analytics aracısını yükler ve Syslog iletilerini dinlemek için gerektiği gibi yapılandırır.
- Syslog daemon'u, TCP portu 514'ü kullanarak Syslog iletilerini dinleyecek şekilde yapılandırır ve ardından yalnızca CEF iletilerini TCP portu 25226'yı kullanarak Log Analytics temsilcisine ileter.
- Syslog aracısını verileri toplayacak ve güvenli bir şekilde ayrıştırıldığı ve zenginleştiği Azure Sentinel'e gönderecek şekilde ayarlar.
 
## <a name="deploy-the-agent"></a>Aracıyı dağıtın
 
1. Azure Sentinel portalında **Veri bağlayıcılarını** tıklatın ve **Ortak Etkinlik Biçimi'ni (CEF)** seçin ve ardından **bağlayıcı sayfasını açın.** 

1. **Syslog aracısını yükle ve yapılandırma**altında, Azure, diğer bulut veya şirket içi makine türünüzü seçin. 
   > [!NOTE]
   > Bir sonraki adımdaki komut dosyası Log Analytics aracısını yüklediğinden ve makineyi Azure Sentinel çalışma alanınıza bağladığı için, bu makinenin başka bir çalışma alanına bağlı olmadığından emin olun.
1. Makinenizde yüksek izinler (sudo) olmalıdır. Aşağıdaki komutu kullanarak makinenizde Python olduğundan emin olun:`python –version`

1. Proxy makinenizde aşağıdaki komut dosyasını çalıştırın.
   `sudo wget https://raw.githubusercontent.com/Azure/Azure-Sentinel/master/DataConnectors/CEF/cef_installer.py&&sudo python cef_installer.py [WorkspaceID] [Workspace Primary Key]`
1. Komut dosyası çalışırken, herhangi bir hata veya uyarı iletisi almadığınızdan emin olun.

ADIM 2'ye devam [et: Güvenlik çözümünüzü CEF iletilerini iletmek için yapılandırın.](connect-cef-solution-config.md)


## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, CEF cihazlarını Azure Sentinel'e nasıl bağlayabileceğinizi öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Verilerinize ve olası tehditlere](quickstart-get-visibility.md)nasıl görünürlük elde edebilirsiniz öğrenin.
- Azure [Sentinel ile tehditleri algılamaya](tutorial-detect-threats.md)başlayın.

