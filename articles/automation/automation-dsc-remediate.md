---
title: Uyumsuz Azure Otomasyonu durum yapılandırma sunucularını düzelt
description: Yapılandırma durumunun düzeltebilecekler sahip olduğu sunuculara yapılandırmaları isteğe bağlı olarak yeniden uygulama
services: automation
ms.service: automation
ms.subservice: dsc
author: mgreenegit
ms.author: migreene
ms.topic: conceptual
ms.date: 07/17/2019
manager: nirb
ms.openlocfilehash: f871b406793e455c857ca14c83434c9ed3e004df
ms.sourcegitcommit: 309a9d26f94ab775673fd4c9a0ffc6caa571f598
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2020
ms.locfileid: "82993836"
---
# <a name="remediate-noncompliant-dsc-servers"></a>Uyumsuz DSC sunucularını düzelt

Sunucular Azure Otomasyonu durum yapılandırmasıyla kaydedildiğinde, yapılandırma modu `ApplyOnly`, `ApplyandMonitor`veya `ApplyAndAutoCorrect`olarak ayarlanır. Mod olarak `ApplyAndAutoCorrect`ayarlanmamışsa, her nedenden dolayı uyumlu bir durumdan alınan sunucular el ile düzeltilene kadar uyumsuz kalır.

Azure işlem, müşterilerin sanal makinelerde betikleri çalıştırmasına izin veren Run komutu adlı bir özellik sunar.
Bu belge, yapılandırma DRFT 'i el ile düzeltirken bu özellik için örnek betik sağlar.

## <a name="correct-drift-of-windows-virtual-machines-using-powershell"></a>PowerShell kullanarak Windows sanal makinelerini doğru Drın

Windows sanal makinelerinde Çalıştır komut özelliğini kullanarak adım adım yönergeler için, [Çalıştır komutuyla WINDOWS VM 'niz Içindeki PowerShell betiklerini çalıştırma](/azure/virtual-machines/windows/run-command)sayfasına bakın.

Bir Azure Otomasyonu durum yapılandırma düğümünü en son yapılandırmayı indirip uygulamayı zorlamak için [Update-DscConfiguration](/powershell/module/psdesiredstateconfiguration/update-dscconfiguration) cmdlet 'ini kullanın.

```powershell
Update-DscConfiguration -Wait -Verbose
```

## <a name="correct-drift-of-linux-virtual-machines"></a>Linux sanal makinelerini doğru DRT

Linux sunucuları için şu anda benzer işlevler bulunmamaktadır.
Tek seçenek kayıt işlemini tekrarlamalısınız.
Azure düğümleri için, Azure portal veya az Module cmdlet 'lerini kullanarak DRFT 'yi düzeltebilirsiniz. Bu işlemle ilgili ayrıntılar, [Azure Otomasyonu durum yapılandırması tarafından yönetim için makineleri ekleme](automation-dsc-onboarding.md#enable-a-vm-using-azure-portal)bölümünde belgelenmiştir.
Karma düğümler için, dahil edilen Python betiklerini kullanarak değişikliklerini 'yi düzeltebilirsiniz.
Bkz. [Linux deposu Için POWERSHELL DSC](https://github.com/Microsoft/PowerShell-DSC-for-Linux#performing-dsc-operations-from-the-linux-computer).

## <a name="next-steps"></a>Sonraki adımlar

- PowerShell cmdlet başvurusu için bkz. [az. Automation](https://docs.microsoft.com/powershell/module/az.automation/?view=azps-3.7.0#automation
).
- Azure Otomasyonu durum yapılandırması 'nı sürekli bir dağıtım ardışık düzeninde kullanmaya ilişkin bir örnek görmek için bkz. [Azure Otomasyonu durum yapılandırması ve Chocolatey kullanarak sürekli dağıtım](automation-dsc-cd-chocolatey.md).