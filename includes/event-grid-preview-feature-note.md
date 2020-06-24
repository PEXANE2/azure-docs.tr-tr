---
title: dosya dahil etme
description: dosya dahil etme
services: event-grid
author: tfitzmac
ms.service: event-grid
ms.topic: include
ms.date: 11/06/2018
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: d32beb2d799a60cb9c5be061c39e4ec834da8dcf
ms.sourcegitcommit: be32c9a3f6ff48d909aabdae9a53bd8e0582f955
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/26/2020
ms.locfileid: "66814842"
---
Bu özellik önizlemede. Bunu kullanmak için bir önizleme uzantısı veya modülü yüklemelisiniz.

### <a name="install-extension-for-azure-cli"></a>Azure CLı için uzantıyı yükler

Azure CLı için [Event Grid uzantıya](/cli/azure/azure-cli-extensions-list)ihtiyacınız vardır.

[Cloudshell](/azure/cloud-shell/quickstart)içinde:

* Uzantıyı daha önce yüklediyseniz, güncelleştirme`az extension update -n eventgrid`
* Uzantıyı daha önce yüklemediyseniz, yükleme`az extension add -n eventgrid`

Yerel yükleme için:

1. [Azure CLI 'Yı yükler](/cli/azure/install-azure-cli). İle kontrol ederek en son sürüme sahip olduğunuzdan emin olun `az --version` .
1. Uzantının önceki sürümlerini Kaldır`az extension remove -n eventgrid`
1. `eventgrid`Uzantıyı ile birlikte yükler`az extension add -n eventgrid`

### <a name="install-module-for-powershell"></a>PowerShell için modülü yükler

PowerShell için [Azurerd. EventGrid modülüne](https://www.powershellgallery.com/packages/AzureRM.EventGrid/0.4.1-preview)ihtiyacınız vardır.

[Cloudshell](/azure/cloud-shell/quickstart-powershell)içinde:

* Modülü yükler`Install-Module -Name AzureRM.EventGrid -AllowPrerelease -Force -Repository PSGallery`

Yerel yükleme için:

1. PowerShell konsolunu yönetici olarak aç
1. Modülü yükler`Install-Module -Name AzureRM.EventGrid -AllowPrerelease -Force -Repository PSGallery`

`-AllowPrerelease`Parametre kullanılamıyorsa, aşağıdaki adımları kullanın:

1. `Install-Module PowerShellGet -Force` öğesini çalıştırın
1. `Update-Module PowerShellGet` öğesini çalıştırın
1. PowerShell konsolunu kapatma
1. PowerShell 'i yönetici olarak yeniden Başlat
1. Modülü yükler`Install-Module -Name AzureRM.EventGrid -AllowPrerelease -Force -Repository PSGallery`
