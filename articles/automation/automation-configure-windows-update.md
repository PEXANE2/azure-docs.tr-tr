---
title: Azure Otomasyonu Güncelleştirme Yönetimi Windows Update ayarlarını yapılandırma
description: Bu makalede Windows Update ayarlarının Azure Otomasyonu Güncelleştirme Yönetimi ile çalışması için nasıl yapılandırılacağı açıklanır.
services: automation
ms.subservice: update-management
ms.date: 05/04/2020
ms.topic: conceptual
ms.openlocfilehash: e30d1606a3928f421d2155e2d1abac0c8a6872aa
ms.sourcegitcommit: ec682dcc0a67eabe4bfe242fce4a7019f0a8c405
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2020
ms.locfileid: "86186648"
---
# <a name="configure-windows-update-settings-for-azure-automation-update-management"></a>Azure Otomasyonu Güncelleştirme Yönetimi Windows Update ayarlarını yapılandırma

Azure Otomasyonu Güncelleştirme Yönetimi, Windows güncelleştirmelerini indirmek ve yüklemek için [Windows Update istemcisine](/windows/deployment/update/windows-update-overview) bağımlıdır. Windows Server Update Services (WSUS) veya Windows Update bağlanırken Windows Update istemcisi tarafından kullanılan belirli ayarlar vardır. Bu ayarların birçoğu, ile yönetilebilir:

- Yerel Grup İlkesi Düzenleyicisi
- Grup İlkesi
- PowerShell
- Kayıt defterini doğrudan Düzenle

Güncelleştirme Yönetimi, Windows Update istemcisini denetlemek için belirtilen ayarların birçoğunu sağlar. Windows dışı güncelleştirmeleri etkinleştirmek için ayarları kullanıyorsanız, Güncelleştirme Yönetimi bu güncelleştirmeleri de yönetecektir. Güncelleştirme dağıtımı gerçekleşmeden önce güncelleştirmelerin indirilmesini etkinleştirmek istiyorsanız, güncelleştirme dağıtımı daha hızlı, daha verimli ve bakım penceresini aşmaya daha az olabilir.

Azure aboneliğinizde WSUS ayarlama ve Windows sanal makinelerinizi güvenli bir şekilde güncel tutmanız konusunda ek öneriler için, [WSUS kullanarak Azure 'Da Windows sanal makinelerini güncelleştirme dağıtımınızı planlayın](/azure/architecture/example-scenario/wsus/).

## <a name="pre-download-updates"></a>Güncelleştirmeleri indirme öncesi

Güncelleştirmelerin otomatik olarak indirilmesini otomatik olarak yüklemeden yapılandırmak için, [Otomatik Güncelleştirmeler ayarını](/windows-server/administration/windows-server-update-services/deploy/4-configure-group-policy-settings-for-automatic-updates##configure-automatic-updates) 3 olarak yapılandırmak üzere Grup İlkesi kullanabilirsiniz. Bu ayar, arka planda gerekli güncelleştirmelerin indirmelerini sağlar ve güncelleştirmelerin yüklenmeye hazırolduğunu bildirir. Bu şekilde Güncelleştirme Yönetimi zamanlamaların denetiminde kalır, ancak güncelleştirmelerin Güncelleştirme Yönetimi bakım penceresi dışında indirilmesine izin verir. Bu davranış `Maintenance window exceeded` güncelleştirme yönetimi hataları engeller.

PowerShell 'de bu ayarı etkinleştirebilirsiniz:

```powershell
$WUSettings = (New-Object -com "Microsoft.Update.AutoUpdate").Settings
$WUSettings.NotificationLevel = 3
$WUSettings.Save()
```

## <a name="configure-reboot-settings"></a>Yeniden başlatma ayarlarını yapılandırma

[Yeniden başlatmayı yönetmek için kullanılan](/windows/deployment/update/waas-restart#registry-keys-used-to-manage-restart) kayıt defteri ve kayıt defteri anahtarlarını [düzenleyerek otomatik güncelleştirmeleri yapılandırma](/windows/deployment/update/waas-wu-settings#configuring-automatic-updates-by-editing-the-registry) bölümünde listelenen kayıt defteri anahtarları, **güncelleştirme dağıtım** ayarlarında **hiçbir şekilde yeniden başlatma** belirtseniz bile makinelerinizin yeniden başlatılmasına neden olabilir. Bu kayıt defteri anahtarlarını ortamınıza en iyi uyacak şekilde yapılandırın.

## <a name="enable-updates-for-other-microsoft-products"></a>Diğer Microsoft ürünleri için güncelleştirmeleri etkinleştir

Varsayılan olarak, Windows Update istemcisi yalnızca Windows için güncelleştirmeler sağlamak üzere yapılandırılmıştır. **Windows 'u güncelleştirdiğimde diğer Microsoft ürünlerine yönelik güncelleştirmeleri ver** ' i etkinleştirirseniz, Microsoft SQL Server ve diğer Microsoft yazılımları için güvenlik düzeltme ekleri de dahil olmak üzere diğer ürünler için de güncelleştirmeler alırsınız. Windows 2016 ve üzeri için kullanılabilen en son [yönetim şablonu dosyalarını](https://support.microsoft.com/help/3087759/how-to-create-and-manage-the-central-store-for-group-policy-administra) indirip kopyaladığınız takdirde bu seçeneği yapılandırabilirsiniz.

Windows Server 2012 R2 çalıştıran makineleriniz varsa, bu ayarı grup ilkesi aracılığıyla yapılandıramazsınız. Şu makinelerde PowerShell komutunu çalıştırın:

```powershell
$ServiceManager = (New-Object -com "Microsoft.Update.ServiceManager")
$ServiceManager.Services
$ServiceID = "7971f918-a847-4430-9279-4a52d1efe18d"
$ServiceManager.AddService2($ServiceId,7,"")
```

## <a name="make-wsus-configuration-settings"></a>WSUS yapılandırma ayarlarını yap

Güncelleştirme Yönetimi WSUS ayarlarını destekler. Güncelleştirme, [intranet Microsoft Update hizmeti konumunu belirtme](/windows/deployment/update/waas-wu-settings#specify-intranet-microsoft-update-service-location)' deki yönergeleri kullanarak tarama ve indirme kaynakları belirtebilirsiniz. Windows Update istemcisi varsayılan olarak güncelleştirmeleri Windows Update indirmek üzere yapılandırılmıştır. Makineleriniz için kaynak olarak bir WSUS sunucusu belirttiğinizde, güncelleştirmeler WSUS 'de onaylanmamışsa güncelleştirme dağıtımı başarısız olur. 

Makineleri iç güncelleştirme hizmetine kısıtlamak için [Windows Update Internet konumlarına bağlanma](/windows-server/administration/windows-server-update-services/deploy/4-configure-group-policy-settings-for-automatic-updates#do-not-connect-to-any-windows-update-internet-locations)seçeneğini belirleyin. 

## <a name="next-steps"></a>Sonraki adımlar

[Azure VM 'leriniz için güncelleştirmeleri ve düzeltme eklerini yönetme](automation-tutorial-update-management.md)konusundaki yönergeleri uygulayarak bir güncelleştirme dağıtımı zamanlayın.
