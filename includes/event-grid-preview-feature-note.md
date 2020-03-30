---
title: include dosyası
description: include dosyası
services: event-grid
author: tfitzmac
ms.service: event-grid
ms.topic: include
ms.date: 11/06/2018
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: d32beb2d799a60cb9c5be061c39e4ec834da8dcf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "66814842"
---
Bu özellik önizlemede. Kullanmak için bir önizleme uzantısı veya modül yüklemeniz gerekir.

### <a name="install-extension-for-azure-cli"></a>Azure CLI için yükleme uzantısı

Azure CLI için [Olay Izgara uzantısına](/cli/azure/azure-cli-extensions-list)ihtiyacınız var.

[Bulut Kabuk](/azure/cloud-shell/quickstart)olarak:

* Uzantıyı daha önce yüklediyseniz, güncelleştirme`az extension update -n eventgrid`
* Uzantıyı daha önce yüklemediyseniz,`az extension add -n eventgrid`

Yerel yükleme için:

1. [Azure CLI'yi yükleyin.](/cli/azure/install-azure-cli) Kontrol ederek en son sürüme sahip `az --version`olduğundan emin olun.
1. Uzantının önceki sürümlerini kaldırma`az extension remove -n eventgrid`
1. Uzantıyı `eventgrid``az extension add -n eventgrid`

### <a name="install-module-for-powershell"></a>PowerShell için modül yükleme

PowerShell için [AzureRM.EventGrid modülüne](https://www.powershellgallery.com/packages/AzureRM.EventGrid/0.4.1-preview)ihtiyacınız vardır.

[Bulut Kabuk](/azure/cloud-shell/quickstart-powershell)olarak:

* Modülü yükleyin`Install-Module -Name AzureRM.EventGrid -AllowPrerelease -Force -Repository PSGallery`

Yerel yükleme için:

1. PowerShell konsolu yönetici olarak aç
1. Modülü yükleyin`Install-Module -Name AzureRM.EventGrid -AllowPrerelease -Force -Repository PSGallery`

`-AllowPrerelease` Parametre kullanılamıyorsa, aşağıdaki adımları kullanın:

1. `Install-Module PowerShellGet -Force` öğesini çalıştırın
1. `Update-Module PowerShellGet` öğesini çalıştırın
1. PowerShell konsolu kapatın
1. PowerShell'i yönetici olarak yeniden başlatın
1. Modülü yükleyin`Install-Module -Name AzureRM.EventGrid -AllowPrerelease -Force -Repository PSGallery`
