---
title: Azure Sentinel 'e bağlantıyı doğrulama | Microsoft Docs
description: CEF iletilerinin Azure Sentinel 'e iletildiğinden emin olmak için güvenlik çözümünüzün bağlantısını doğrulayın.
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
ms.openlocfilehash: e192f5db212faae9a8a93fbf3991f05b0adef0f8
ms.sourcegitcommit: 003e73f8eea1e3e9df248d55c65348779c79b1d6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/02/2020
ms.locfileid: "75615352"
---
# <a name="step-3-validate-connectivity"></a>3\. Adım: bağlantıyı doğrulama



Aracıyı dağıttıktan ve CEF iletilerini iletmek için Güvenlik çözümünüzü yapılandırdıktan sonra, Azure Sentinel ve güvenlik çözümünüz arasındaki bağlantıyı doğrulamayı öğrenmek için bu makaleyi kullanın. 

## <a name="how-to-validate-connectivity"></a>Bağlantıyı doğrulama

1. Günlüklerin CommonSecurityLog şeması kullanılarak alındığından emin olmak için Log Analytics açın.<br> Günlüklerinizin Log Analytics görünene kadar 20 dakikadan bu kadar bir zaman çıkabilir. 

1. Betiği çalıştırmadan önce, yapılandırdığınız Syslog proxy makinesine iletilmesini sağlamak için güvenlik çözümünüzden iletiler göndermenizi öneririz. 
1. Makinenizde yükseltilmiş izinleriniz (sudo) olmalıdır. Aşağıdaki komutu kullanarak makinenizde Python olduğundan emin olun: `python –version`
1. Aracı, Azure Sentinel ve güvenlik çözümünüz arasındaki bağlantıyı denetlemek için aşağıdaki betiği çalıştırın. Daemon iletme işleminin düzgün şekilde yapılandırıldığını, doğru bağlantı noktalarını dinlediğini ve hiçbir şeyin daemon ile Log Analytics Aracısı arasındaki iletişimi engellemediğini denetler. Betik Ayrıca, uçtan uca bağlantıyı denetlemek için ' TestCommonEventFormat ' adlı sahte iletiler gönderir. <br>
 `sudo wget https://raw.githubusercontent.com/Azure/Azure-Sentinel/master/DataConnectors/CEF/cef_troubleshoot.py&&sudo python cef_troubleshoot.py [WorkspaceID]`


## <a name="next-steps"></a>Sonraki adımlar
Bu belgede CEF gereçlerini Azure Sentinel 'e bağlamayı öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Verilerinize nasıl görünürlük alabileceğinizi ve olası tehditleri](quickstart-get-visibility.md)öğrenin.
- [Azure Sentinel ile tehditleri algılamaya](tutorial-detect-threats.md)başlayın.
- Verilerinizi izlemek için [çalışma kitaplarını kullanın](tutorial-monitor-your-data.md) .

