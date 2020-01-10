---
title: CEF verilerini Azure Sentinel Preview 'a bağlamak için aracıyı dağıtın | Microsoft Docs
description: CEF verilerini Azure Sentinel 'e bağlamak için aracıyı dağıtmayı öğrenin.
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
ms.date: 11/26/2019
ms.author: rkarlin
ms.openlocfilehash: 5451382763195172b48e93ef2ea859552b05d154
ms.sourcegitcommit: 003e73f8eea1e3e9df248d55c65348779c79b1d6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/02/2020
ms.locfileid: "75615378"
---
# <a name="step-1-deploy-the-agent"></a>1\. Adım: aracıyı dağıtma


Bu adımda, Azure Sentinel ve güvenlik çözümünüz arasında proxy görevi görecek Linux makinesini seçmeniz gerekir. Proxy makinede şu komutu çalıştırmanız gerekir:
- Log Analytics aracısını yükleyip syslog iletilerini dinlemek için gerektiğinde yapılandırır.
- , TCP bağlantı noktası 514 kullanarak syslog iletilerini dinlemek ve sonra yalnızca CEF iletilerini Log Analytics aracısına, TCP bağlantı noktası 25226 kullanarak iletmek için Syslog Daemon programını yapılandırır.
- Verileri toplamak ve Azure Sentinel 'e güvenli bir şekilde göndermek için Syslog aracısını ayarlar ve burada ayrıştırılıp zenginleştirir.
 
## <a name="deploy-the-agent"></a>Aracıyı dağıtma
 
1. Azure Sentinel portalında, **veri bağlayıcıları** ' na tıklayın ve **ortak olay biçimi (CEF)** öğesini seçin ve ardından **bağlayıcı sayfasını açın**. 

1. **Syslog aracısını yükleyip yapılandırma**altında, Azure, diğer bulut ya da şirket içi makine türünü seçin. 
   > [!NOTE]
   > Sonraki adımdaki betik Log Analytics aracısını yükleyip makineyi Azure Sentinel çalışma alanınıza bağladığından, bu makinenin başka bir çalışma alanına bağlı olmadığından emin olun.
1. Makinenizde yükseltilmiş izinleriniz (sudo) olmalıdır. Aşağıdaki komutu kullanarak makinenizde Python olduğundan emin olun: `python –version`

1. Proxy makinenizde aşağıdaki betiği çalıştırın.
   `sudo wget https://raw.githubusercontent.com/Azure/Azure-Sentinel/master/DataConnectors/CEF/cef_installer.py&&sudo python cef_installer.py [WorkspaceID] [Workspace Primary Key]`
1. Betik çalışırken, herhangi bir hata veya uyarı iletisi aldığınızdan emin olmak için kontrol edin.

2\. [Adım: CEF iletilerini iletmek için Güvenlik çözümünüzü yapılandırma](connect-cef-solution-config.md) .


## <a name="next-steps"></a>Sonraki adımlar
Bu belgede CEF gereçlerini Azure Sentinel 'e bağlamayı öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Verilerinize nasıl görünürlük alabileceğinizi ve olası tehditleri](quickstart-get-visibility.md)öğrenin.
- [Azure Sentinel ile tehditleri algılamaya](tutorial-detect-threats.md)başlayın.

