---
title: Ekleme Azure Cloud Shell | Microsoft Docs
description: Azure Cloud Shell eklemeyi öğrenin.
services: cloud-shell
documentationcenter: ''
author: maertendMSFT
manager: timlt
tags: azure-resource-manager
ms.assetid: ''
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 12/11/2017
ms.author: damaerte
ms.openlocfilehash: bbf6c1f3049265961559ea34c8b748b2b4d263b6
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "60614302"
---
# <a name="embed-azure-cloud-shell"></a>Azure Cloud Shell Ekle

Ekleme Cloud Shell, geliştiricilerin ve içerik yazıcılarının Cloud Shell özel bir URL 'den doğrudan açmasını sağlar, [Shell.Azure.com](https://shell.azure.com). Bu, Cloud Shell kimlik doğrulaması, alet kullanımı ve güncel Azure CLı/Azure PowerShell araçlarının tam gücünü kullanıcılarınıza taşır.

Normal boyutlu düğme

[![Normal başlatma](https://shell.azure.com/images/launchcloudshell.png "Azure Cloud Shell'i başlatma")](https://shell.azure.com)

Büyük boyutlu düğme

[![Büyük başlatma](https://shell.azure.com/images/launchcloudshell@2x.png "Azure Cloud Shell'i başlatma")](https://shell.azure.com)

## <a name="how-to"></a>Nasıl yapılır

Cloud Shell başlatma düğmesini aşağıdaki kopyalayarak markaşağı dosyalara tümleştirin:

```markdown
[![Launch Cloud Shell](https://shell.azure.com/images/launchcloudshell.png "Launch Cloud Shell")](https://shell.azure.com)
```

Bir açılır Cloud Shell eklemek için HTML aşağıda verilmiştir:
```html
<a style="cursor:pointer" onclick='javascript:window.open("https://shell.azure.com", "_blank", "toolbar=no,scrollbars=yes,resizable=yes,menubar=no,location=no,status=no")'><img alt="Launch Azure Cloud Shell" src="https://shell.azure.com/images/launchcloudshell.png" /></a>
```

## <a name="customize-experience"></a>Deneyimi özelleştirme

URL 'nizi genişleterek belirli bir kabuk deneyimi ayarlayın.

|Deneyim   |URL'si   |
|---|---|
|En son kullanılan kabuk   |[shell.azure.com](https://shell.azure.com)           |
|Bash                       |[shell.azure.com/bash](https://shell.azure.com/bash)       |
|PowerShell                 |[shell.azure.com/powershell](https://shell.azure.com/powershell) |

## <a name="next-steps"></a>Sonraki adımlar
[Bash Cloud Shell hızlı başlangıç](quickstart.md)<br>
[PowerShell Cloud Shell hızlı başlangıç](quickstart-powershell.md)
