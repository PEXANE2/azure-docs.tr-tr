---
title: Azure Bulut Kabuklarını Gömme | Microsoft Dokümanlar
description: Azure Bulut Su Larını yerleştirmeyi öğrenin.
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60614302"
---
# <a name="embed-azure-cloud-shell"></a>Azure Bulut KabuğuNu Gömme

Bulut Kabuğunu katıştırma, geliştiricilerin ve içerik yazarlarının Cloud Shell'i shell.azure.com özel bir URL'den doğrudan açmalarını [sağlar.](https://shell.azure.com) Bu, Cloud Shell'in kimlik doğrulama, araç ve güncel Azure CLI/Azure PowerShell araçlarının tüm gücünü kullanıcılarınıza hemen getirir.

Normal boyutlu düğme

[![Düzenli fırlatma](https://shell.azure.com/images/launchcloudshell.png "Azure Cloud Shell'i başlatma")](https://shell.azure.com)

Büyük boyutlu düğme

[![Büyük fırlatma](https://shell.azure.com/images/launchcloudshell@2x.png "Azure Cloud Shell'i başlatma")](https://shell.azure.com)

## <a name="how-to"></a>Nasıl yapılır

Aşağıdakileri kopyalayarak Cloud Shell'in başlatma düğmesini işaretleme dosyalarına entegre edin:

```markdown
[![Launch Cloud Shell](https://shell.azure.com/images/launchcloudshell.png "Launch Cloud Shell")](https://shell.azure.com)
```

Açılır Bulut Kabuğu'nu yerleştirmek için HTML aşağıdadır:
```html
<a style="cursor:pointer" onclick='javascript:window.open("https://shell.azure.com", "_blank", "toolbar=no,scrollbars=yes,resizable=yes,menubar=no,location=no,status=no")'><img alt="Launch Azure Cloud Shell" src="https://shell.azure.com/images/launchcloudshell.png" /></a>
```

## <a name="customize-experience"></a>Deneyimi özelleştirin

URL'nizi artırarak belirli bir kabuk deneyimi ayarlayın.

|Deneyim   |URL'si   |
|---|---|
|En son kullanılan kabuk   |[shell.azure.com](https://shell.azure.com)           |
|Bash                       |[shell.azure.com/bash](https://shell.azure.com/bash)       |
|PowerShell                 |[shell.azure.com/powershell](https://shell.azure.com/powershell) |

## <a name="next-steps"></a>Sonraki adımlar
[Bulut Kabuğunda Bash hızlı başlat](quickstart.md)<br>
[PowerShell Bulut Shell hızlı başlat](quickstart-powershell.md)
