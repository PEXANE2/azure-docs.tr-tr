---
title: Uyumlu olmayan Azure Otomasyon Durumu Yapılandırma sunucularını düzeltin
description: Yapılandırma durumunun sürüklendiği sunuculara isteğe bağlı yapılandırmaları yeniden uygulama
services: automation
ms.service: automation
ms.subservice: dsc
author: mgreenegit
ms.author: migreene
ms.topic: conceptual
ms.date: 07/17/2019
manager: nirb
ms.openlocfilehash: cc5884e1f70bdccee4e7a113e6e3ee2d6604b50a
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81406079"
---
# <a name="remediate-noncompliant-dsc-servers"></a>Uyumlu olmayan DSC sunucularını düzeltin

Sunucular Azure Otomasyon Durumu Yapılandırması'na kaydedildiğinde, `ApplyandMonitor`yapılandırma `ApplyAndAutoCorrect`modu `ApplyOnly`, yani . Mod `ApplyAndAutoCorrect`ayarlanmıyorsa, herhangi bir nedenle uyumlu bir durumdan sürüklenen sunucular, el ile düzeltilene kadar uyumlu değildir.

Azure bilgi işlem, müşterilerin sanal makinelerde komut ları çalıştırmasına olanak tanıyan Çalıştır Komutu adlı bir özellik sunar.
Bu belge, yapılandırma sürüklenme el ile düzeltirken bu özellik için örnek komut dosyaları sağlar.

## <a name="correct-drift-of-windows-virtual-machines-using-powershell"></a>PowerShell kullanarak Windows sanal makinelerin doğru sürüklenme

Windows sanal makinelerde Çalıştır Komutu özelliğini kullanarak adım adım talimatlar için, [Windows VM'nizde Run Komutu ile PowerShell komutdosyalarını çalıştır'A](/azure/virtual-machines/windows/run-command)bakın.

Azure Otomasyon Durumu Yapılandırma düğümlerini en son yapılandırmayı indirmeye ve uygulamak için [Update-DscConfiguration](/powershell/module/psdesiredstateconfiguration/update-dscconfiguration) cmdlet'i kullanın.

```powershell
Update-DscConfiguration -Wait -Verbose
```

## <a name="correct-drift-of-linux-virtual-machines"></a>Linux sanal makinelerin doğru sürüklenme

Benzer işlevler şu anda Linux sunucuları için kullanılamıyor.
Tek seçenek kayıt işlemini tekrarlamaktır.

Azure düğümleri için, Azure portalından veya Az modül cmdletlerini kullanarak sürüklenmedüzeltmeyapabilirsiniz. Bu işlemle ilgili [ayrıntılar, Azure Otomasyon Durumu Yapılandırması tarafından yönetilmek üzere Onboarding makinelerde](automation-dsc-onboarding.md#onboard-vms-by-using-the-azure-portal)belgelenmiştir.
Karma düğümler için, dahil Python komut dosyalarını kullanarak sürüklenme düzeltebilirsiniz.
[Linux repo için PowerShell DSC](https://github.com/Microsoft/PowerShell-DSC-for-Linux#performing-dsc-operations-from-the-linux-computer)bakın.

## <a name="next-steps"></a>Sonraki adımlar

- PowerShell cmdlet referansı için [Az.Automation'a](https://docs.microsoft.com/powershell/module/az.automation/?view=azps-3.7.0#automation
)bakın.
- Sürekli dağıtım ardışık bir şekilde Azure Otomasyon Durumu Yapılandırmasını kullanma örneğini görmek için [bkz.](automation-dsc-cd-chocolatey.md)