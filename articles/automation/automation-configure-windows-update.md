---
title: Windows Update ayarlarını Azure Güncelleştirme Yönetimi ile çalışacak şekilde yapılandırma
description: Bu makalede, Güncelleştirme Yönetimi birlikte çalışmak üzere yapılandırdığınız Windows Update ayarları açıklanmaktadır
services: automation
ms.service: automation
ms.subservice: update-management
author: bobbytreed
ms.author: robreed
ms.date: 10/02/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: f50ca9515f12e8c9b5943904c4d0226f2ca3353c
ms.sourcegitcommit: 0576bcb894031eb9e7ddb919e241e2e3c42f291d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2019
ms.locfileid: "72377570"
---
# <a name="configure-windows-update-settings-for-update-management"></a>Güncelleştirme Yönetimi için Windows Update ayarlarını yapılandırma

Güncelleştirme Yönetimi, Windows güncelleştirmelerini indirmek ve yüklemek için Windows Update bağımlıdır. Sonuç olarak, Windows Update tarafından kullanılan birçok ayarı sunuyoruz. Windows dışı güncelleştirmeleri etkinleştirmek için ayarları kullanıyorsanız, Güncelleştirme Yönetimi bu güncelleştirmeleri de yönetecektir. Güncelleştirme dağıtımı gerçekleşmeden önce güncelleştirme indirmeyi etkinleştirmek istiyorsanız, güncelleştirme dağıtımları daha hızlı gidip bakım penceresini aşmaya daha az olabilir.

## <a name="pre-download-updates"></a>İndirme öncesi güncelleştirmeler

Grup ilkesi güncelleştirmeleri otomatik olarak karşıdan yüklemeyi yapılandırmak için [otomatik güncelleştirmeleri Yapılandır ayarını](/windows-server/administration/windows-server-update-services/deploy/4-configure-group-policy-settings-for-automatic-updates##configure-automatic-updates) **3**olarak ayarlayabilirsiniz. Bu, arka planda gerekli olan güncelleştirmeleri indirir, ancak yüklemez. Bu, zamanlamaların denetiminde Güncelleştirme Yönetimi korur ancak güncelleştirmelerin Güncelleştirme Yönetimi bakım penceresi dışında indirilmesine izin verir. Bu, **bakım penceresinin** güncelleştirme yönetimi hatalarını aşılmasına engel olabilir.

Bunu PowerShell ile de ayarlayabilirsiniz, güncelleştirmeleri otomatik olarak indirmek istediğiniz bir sistemde aşağıdaki PowerShell 'i çalıştırın.

```powershell
$WUSettings = (New-Object -com "Microsoft.Update.AutoUpdate").Settings
$WUSettings.NotificationLevel = 3
$WUSettings.Save()
```

## <a name="disable-automatic-installation"></a>Otomatik yüklemeyi devre dışı bırak

Azure VM 'Leri varsayılan olarak etkinleştirilen güncelleştirmelerin otomatik olarak yüklenmesine sahiptir. Bu, güncelleştirmelerin Güncelleştirme Yönetimi tarafından yüklenmek üzere zamanlanmadan önce yüklenememesine neden olabilir. @No__t-0 kayıt defteri anahtarını `1` olarak ayarlayarak bu davranışı devre dışı bırakabilirsiniz. Aşağıdaki PowerShell kod parçacığı bunu yapmanın bir yolunu gösterir.

```powershell
$AutoUpdatePath = "HKLM:SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate\AU"
Set-ItemProperty -Path $AutoUpdatePath -Name NoAutoUpdate -Value 1
```

## <a name="configure-reboot-settings"></a>Yeniden başlatma ayarlarını yapılandırma

[Yeniden başlatmayı yönetmek için kullanılan](/windows/deployment/update/waas-restart#registry-keys-used-to-manage-restart) kayıt defteri ve kayıt defteri anahtarlarını [düzenleyerek otomatik güncelleştirmeleri yapılandırma](/windows/deployment/update/waas-wu-settings#configuring-automatic-updates-by-editing-the-rej7uijui7jgistry) altında listelenen kayıt defteri anahtarları, güncelleştirme dağıtım ayarlarında **hiçbir şekilde yeniden başlatma** belirtseniz bile makinelerinizin yeniden başlatılmasına neden olabilir. . Bu kayıt defteri anahtarlarını ortamınız için istendiği şekilde yapılandırmanız gerekir.

## <a name="enable-updates-for-other-microsoft-products"></a>Diğer Microsoft ürünleri için güncelleştirmeleri etkinleştir

Varsayılan olarak Windows Update yalnızca Windows için güncelleştirmeler sağlar. **Windows 'u güncelleştirdiğimde diğer Microsoft ürünleri için güncelleştirmeleri Izin ver**' i etkinleştirirseniz, SQL Server veya diğer birinci taraf yazılımlar için güvenlik düzeltme ekleri de dahil olmak üzere diğer ürünlerle ilgili güncelleştirmeler sunulur. Bu seçenek grup ilkesi tarafından yapılandırılamaz. Diğer birinci taraf düzeltme eklerini etkinleştirmek istediğiniz sistemlerde aşağıdaki PowerShell 'i çalıştırın ve Güncelleştirme Yönetimi Bu ayarı kabul eder.

```powershell
$ServiceManager = (New-Object -com "Microsoft.Update.ServiceManager")
$ServiceManager.Services
$ServiceID = "7971f918-a847-4430-9279-4a52d1efe18d"
$ServiceManager.AddService2($ServiceId,7,"")
```

## <a name="wsus-configuration-settings"></a>WSUS Yapılandırma ayarları

**GÜNCELLEŞTIRME YÖNETIMI** WSUS yapılandırma ayarlarına uyar. Güncelleştirme Yönetimi ile çalışmak için yapılandırabileceğiniz WSUS ayarlarının listesi aşağıda listelenmiştir.

### <a name="intranet-microsoft-update-service-location"></a>Intranet Microsoft güncelleştirme hizmeti konumu

Güncelleştirmeleri taramak ve indirmek için, [Intranet Microsoft Update hizmet konumu](/windows/deployment/update/waas-wu-settings#specify-intranet-microsoft-update-service-location)altında kaynakları belirtebilirsiniz.

## <a name="next-steps"></a>Sonraki Adımlar

Windows Update ayarlarını yapılandırdıktan sonra, [Azure VM 'leriniz için güncelleştirmeleri ve düzeltme eklerini yönetme](automation-tutorial-update-management.md) bölümündeki yönergeleri Izleyerek bir güncelleştirme dağıtımı zamanlayabilirsiniz.