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
ms.openlocfilehash: f4ca76f4be9d00e185f8774fc33296d1af1aeece
ms.sourcegitcommit: 3c318f6c2a46e0d062a725d88cc8eb2d3fa2f96a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80585493"
---
# <a name="remediate-non-compliant-dsc-servers"></a>Uyumlu olmayan DSC sunucularını düzeltme

Sunucular Azure Otomasyon Durumu Yapılandırması'na kaydedildiğinde, 'Yapılandırma Modu' ApplyOnly, ApplyandMonitor veya ApplyAndAutoCorrect olarak ayarlanır.
Mod Otomatik Düzeltme olarak ayarlanmazsa, herhangi bir nedenle uyumlu bir durumdan sürüklenen sunucular, el ile düzeltilene kadar uyumlu olmaz.

Azure bilgi işlem, müşterilerin sanal makinelerde komut ları çalıştırmasına olanak tanıyan Çalıştır Komutu adlı bir özellik sunar.
Bu belge, yapılandırma sürüklenme el ile düzeltirken bu özellik için örnek komut dosyaları sağlar.

## <a name="correct-drift-of-windows-virtual-machines-using-powershell"></a>PowerShell kullanarak Windows sanal makinelerin doğru sürüklenme

Windows sanal makinelerde Çalıştır Komutu özelliğini kullanarak adım adım talimatlar için, [Windows VM'nizde Run Komutu ile PowerShell komutdosyalarını çalıştır'A](/azure/virtual-machines/windows/run-command)bakın.

Azure Otomasyon Durumu Yapılandırma düğümlerini en son yapılandırmayı indirmeye `Update-DscConfiguration` ve uygulamak için cmdlet'i kullanın.
Ayrıntılar için cmdlet belgeleri [Güncelleştirme-DscConfiguration'a](/powershell/module/psdesiredstateconfiguration/update-dscconfiguration)bakın.

```powershell
Update-DscConfiguration -Wait -Verbose
```

## <a name="correct-drift-of-linux-virtual-machines"></a>Linux sanal makinelerin doğru sürüklenme

Benzer işlevler şu anda Linux sunucuları için kullanılamıyor.
Tek seçenek kayıt işlemini tekrarlamaktır.
Azure düğümleri için, sürüklenme düzeltme portalından veya Az Automation cmdlets kullanılarak yapılabilir.
Bu işlemle ilgili ayrıntılar [Azure Otomasyon Durumu Yapılandırması tarafından yönetilmeleri için Onboarding makineleri](/azure/automation/automation-dsc-onboarding#onboard-a-vm-using-azure-portal)sayfasında belgelenmiştir.
Hibrit düğümler için, drift düzeltme dahil Python komut dosyaları kullanılarak yapılabilir.
[Linux repo için PowerShell DSC dokümantasyon](https://github.com/Microsoft/PowerShell-DSC-for-Linux#performing-dsc-operations-from-the-linux-computer)bakın.

## <a name="next-steps"></a>Sonraki adımlar

- PowerShell cmdlet başvurusu için Azure [Otomasyon Durumu Yapılandırma cmdlet'ine](/powershell/module/azurerm.automation/#automation) bakın
- Sürekli dağıtım ardışık bir şekilde Azure Otomasyon Durumu Yapılandırmasını kullanma örneğini görmek için [bkz.](automation-dsc-cd-chocolatey.md)
