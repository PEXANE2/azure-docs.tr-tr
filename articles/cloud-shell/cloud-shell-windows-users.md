---
title: Windows kullanıcıları için Azure Cloud Shell | Microsoft Docs
description: Linux sistemleri hakkında bilgi sahibi olmayan kullanıcılar için kılavuz
services: azure
documentationcenter: ''
author: maertendMSFT
manager: hemantm
tags: azure-resource-manager
ms.assetid: ''
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 08/03/2018
ms.author: damaerte
ms.openlocfilehash: 4fc4f6523eb19294cabdf6b5b910dd346a877502
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "67204131"
---
# <a name="powershell-in-azure-cloud-shell-for-windows-users"></a>Windows kullanıcıları için Azure Cloud Shell'de PowerShell

Mayıs 2018 ' de, değişiklikler Azure Cloud Shell PowerShell 'e [duyurulmuştur](https://azure.microsoft.com/blog/pscloudshellrefresh/) .
Azure Cloud Shell PowerShell deneyimi artık Linux ortamında [PowerShell Core 6](https://github.com/powershell/powershell) çalıştırıyor.
Bu değişiklik ile, bir Windows PowerShell deneyiminde beklenildiği ile karşılaştırıldığında Cloud Shell PowerShell deneyiminde bazı farklılıklar olabilir.

## <a name="file-system-case-sensitivity"></a>Dosya sistemi büyük/küçük harf duyarlılığı

Dosya sistemi Windows 'da büyük/küçük harfe duyarlıdır, ancak Linux üzerinde dosya sistemi büyük/küçük harfe duyarlıdır.
Daha önce `file.txt` ve `FILE.txt` aynı dosyada olduğu kabul edildi, ancak artık farklı dosya olarak kabul edilir.
Dosya sisteminde uygun büyük/küçük harf kullanılması gerekir `tab-completing` .
`tab-completing`Cmdlet adları, parametreleri ve değerleri gibi PowerShell 'e özgü deneyimler, büyük/küçük harfe duyarlı değildir.

## <a name="windows-powershell-aliases-vs-linux-utilities"></a>Windows PowerShell diğer adları vs Linux yardımcı programları

Bazı mevcut PowerShell diğer adları,,, `cat` `ls` `sort` vb. yerleşik Linux komutlarıyla aynı ada sahiptir `sleep` . PowerShell Core 6 ' da, yerleşik Linux komutlarıyla çakışan diğer adlar kaldırılmıştır.
Aşağıda, kaldırılan ortak diğer adlar ve bunların eşdeğer komutları verilmiştir:  

|Diğer ad kaldırıldı   |Denk komut   |
|---|---|
|`cat`    | `Get-Content` |
|`curl`   | `Invoke-WebRequest` |
|`diff`   | `Compare-Object` |
|`ls`     | `dir` <br> `Get-ChildItem` |
|`mv`     | `Move-Item`   |
|`rm`     | `Remove-Item` |
|`sleep`  | `Start-Sleep` |
|`sort`   | `Sort-Object` |
|`wget`   | `Invoke-WebRequest` |

## <a name="persisting-home"></a>Kalıcı $HOME

Önceki kullanıcılar yalnızca bulut sürücüsünde betikleri ve diğer dosyaları kalıcı hale getirebileceği.
Artık Kullanıcı $HOME Dizin, oturumlarda de kalıcı hale getirilir.

## <a name="powershell-profile"></a>PowerShell profili

Varsayılan olarak, bir kullanıcının PowerShell profili oluşturulmaz.
Profilinizi oluşturmak için altında bir dizin oluşturun `PowerShell` `$HOME/.config` .

```azurepowershell-interactive
mkdir (Split-Path $profile.CurrentUserAllHosts)
```

Altında `$HOME/.config/PowerShell` , profil dosyalarınızı oluşturabilir `profile.ps1` ve/veya `Microsoft.PowerShell_profile.ps1` .

## <a name="whats-new-in-powershell-core-6"></a>PowerShell Core 6 ' daki yenilikler

PowerShell Core 6 ' daki yenilikler hakkında daha fazla bilgi için [PowerShell belgelerine](https://docs.microsoft.com/powershell/scripting/whats-new/what-s-new-in-powershell-core-60?view=powershell-6) ve [PowerShell Core ile çalışmaya](https://blogs.msdn.microsoft.com/powershell/2017/06/09/getting-started-with-powershell-core-on-windows-mac-and-linux/) başlama blog gönderisine başvurun.
