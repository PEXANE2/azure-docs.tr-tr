---
title: Windows kullanıcıları için Azure Bulut Kabuğu | Microsoft Dokümanlar
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67204131"
---
# <a name="powershell-in-azure-cloud-shell-for-windows-users"></a>Windows kullanıcıları için Azure Cloud Shell'de PowerShell

Mayıs 2018'de Azure Cloud Shell'de PowerShell'de değişiklikler [duyuruldu.](https://azure.microsoft.com/blog/pscloudshellrefresh/)
Azure Cloud Shell'deki PowerShell deneyimi artık [PowerShell Core 6'yı](https://github.com/powershell/powershell) Linux ortamında çalıştırıyor.
Bu değişiklikle, Cloud Shell'deki PowerShell deneyiminde, Windows PowerShell deneyiminde beklenenle karşılaştırıldığında bazı farklılıklar olabilir.

## <a name="file-system-case-sensitivity"></a>Dosya sistemi durum hassasiyeti

Dosya sistemi Windows'da büyük/küçük harf duyarsız, Linux'ta ise, dosya sistemi büyük/küçük harf duyarlıdır.
Daha `file.txt` önce `FILE.txt` ve aynı dosya olarak kabul edildi, ama şimdi farklı dosyalar olarak kabul edilir.
Dosya sistemindeyken `tab-completing` uygun kasa kullanılmalıdır.
PowerShell'e özgü `tab-completing` cmdlet adları, parametreleri ve değerleri gibi özel deneyimler büyük/küçük harf duyarlı değildir.

## <a name="windows-powershell-aliases-vs-linux-utilities"></a>Windows PowerShell takma adları vs Linux yardımcı programları

Bazı mevcut PowerShell diğer adları, yerleşik Linux komutlarıyla aynı `cat`adlara sahiptir, örneğin , ,`ls` `sort`, `sleep`, vb. PowerShell Core 6'da, yerleşik Linux komutlarıyla çarpışan diğer adlar kaldırıldı.
Aşağıda, bunların eşdeğer komutlarının yanı sıra kaldırılan ortak diğer adlar verilmiştir:  

|Kaldırılan Diğer Ad   |Eşdeğer Komut   |
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

Önceki kullanıcılar yalnızca Bulut Sürücülerinde komut dosyaları ve diğer dosyaları devam ettirebiliyordu.
Şimdi, kullanıcının $HOME dizini de oturumlar arasında devam etmektedir.

## <a name="powershell-profile"></a>PowerShell profili

Varsayılan olarak, bir kullanıcının PowerShell profili oluşturulmaz.
Profilinizi oluşturmak için `PowerShell` altında bir `$HOME/.config`dizin oluşturun.

```azurepowershell-interactive
mkdir (Split-Path $profile.CurrentUserAllHosts)
```

Altında, `$HOME/.config/PowerShell`profil dosyalarınızı oluşturabilirsiniz `profile.ps1` - `Microsoft.PowerShell_profile.ps1`ve/veya .

## <a name="whats-new-in-powershell-core-6"></a>PowerShell Core 6'daki yenilikler

PowerShell Core 6'da yeni neler olduğu hakkında daha fazla bilgi için [PowerShell dokümanlarına](https://docs.microsoft.com/powershell/scripting/whats-new/what-s-new-in-powershell-core-60?view=powershell-6) ve PowerShell Core blog [yazısıyla başlarken](https://blogs.msdn.microsoft.com/powershell/2017/06/09/getting-started-with-powershell-core-on-windows-mac-and-linux/) başvurun.
