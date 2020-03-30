---
title: Azure Sentinel'e bağlantı doğrulama| Microsoft Dokümanlar
description: CEF iletilerinin Azure Sentinel'e iletildiğinden emin olmak için güvenlik çözümünüzün bağlantılarını doğrulayın.
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
ms.openlocfilehash: e224f6d5cfd82dfc6cb1ce107d111ee0e031247b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77588442"
---
# <a name="step-3-validate-connectivity"></a>ADIM 3: Bağlantıyı doğrulama



Aracıyı dağıttıktan ve güvenlik çözümünüzü CEF iletilerini iletmek üzere yapılandırdıktan sonra, Azure Sentinel ile güvenlik çözümünüz arasındaki bağlantıyı nasıl doğrulayacağınızı anlamak için bu makaleyi kullanın. 

## <a name="how-to-validate-connectivity"></a>Bağlantı nasıl doğrulanır?

1. Günlüklerin CommonSecurityLog şeması kullanılarak alındığından emin olmak için Günlük Analizi'ni açın.<br> Günlüklerinizin Log Analytics'te görünmeye başlaması 20 dakikadan fazla sürebilir. 

1. Komut dosyasını çalıştırmadan önce, yapılandırılan Syslog proxy makinesine iletildiklerinden emin olmak için güvenlik çözümünüzden ileti göndermenizi öneririz. 
1. Makinenizde yüksek izinler (sudo) olmalıdır. Aşağıdaki komutu kullanarak makinenizde Python olduğundan emin olun:`python –version`
1. Aracı, Azure Sentinel ve güvenlik çözümünüz arasındaki bağlantıyı denetlemek için aşağıdaki komut dosyasını çalıştırın. Daemon yönlendirmenin düzgün bir şekilde yapılandırıldığından, doğru bağlantı noktalarını dinlediğini ve daemon ile Log Analytics aracısı arasındaki iletişimi hiçbir şeyin engellemediğini denetler. Komut dosyası ayrıca uçtan uca bağlantıyı denetlemek için sahte mesajlar 'TestCommonEventFormat' gönderir. <br>
 `sudo wget https://raw.githubusercontent.com/Azure/Azure-Sentinel/master/DataConnectors/CEF/cef_troubleshoot.py&&sudo python cef_troubleshoot.py [WorkspaceID]`


## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, CEF cihazlarını Azure Sentinel'e nasıl bağlayabileceğinizi öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Verilerinize ve olası tehditlere](quickstart-get-visibility.md)nasıl görünürlük elde edebilirsiniz öğrenin.
- Azure [Sentinel ile tehditleri algılamaya](tutorial-detect-threats.md)başlayın.
- Verilerinizi izlemek için [çalışma kitaplarını kullanın.](tutorial-monitor-your-data.md)

