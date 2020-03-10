---
title: Windows Update ayarlarını Azure Güncelleştirme Yönetimi ile çalışacak şekilde yapılandırma
description: Bu makalede, Azure Güncelleştirme Yönetimi ile çalışmak üzere yapılandırdığınız Windows Update ayarları açıklanmaktadır.
services: automation
ms.subservice: update-management
ms.date: 03/02/2020
ms.topic: conceptual
ms.openlocfilehash: 7f226c4d297d25644b2650d085655f70d8326927
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78373256"
---
# <a name="configure-windows-update-settings-for-update-management"></a>Güncelleştirme Yönetimi için Windows Update ayarlarını yapılandırma

Azure Güncelleştirme Yönetimi, Windows güncelleştirmelerini indirmek ve yüklemek için [Windows Update istemcisine](https://docs.microsoft.com//windows/deployment/update/windows-update-overview) bağımlıdır. Windows Server Update Services (WSUS) veya Windows Update bağlanırken Windows Update istemcisi tarafından kullanılan belirli ayarlar vardır. Bu ayarların birçoğu, ile yönetilebilir:

- Yerel Grup İlkesi Düzenleyicisi
- Grup İlkesi
- PowerShell
- Kayıt defterini doğrudan Düzenle

Güncelleştirme Yönetimi, Windows Update istemcisini denetlemek için belirtilen ayarların birçoğunu sağlar. Windows dışı güncelleştirmeleri etkinleştirmek için ayarları kullanıyorsanız, Güncelleştirme Yönetimi bu güncelleştirmeleri de yönetecektir. Güncelleştirme dağıtımı gerçekleşmeden önce güncelleştirmelerin indirilmesini etkinleştirmek istiyorsanız, güncelleştirme dağıtımı daha hızlı, daha verimli ve bakım penceresini aşmaya daha az olabilir.

## <a name="pre-download-updates"></a>Güncelleştirmeleri indirme öncesi

Güncelleştirmelerin otomatik olarak indirilmesini yapılandırmak ancak otomatik olarak yüklemek için grup ilkesi kullanarak [otomatik güncelleştirmeleri Yapılandır ayarını](/windows-server/administration/windows-server-update-services/deploy/4-configure-group-policy-settings-for-automatic-updates##configure-automatic-updates) **3**olarak ayarlayabilirsiniz. Bu ayar, arka planda gerekli güncelleştirmelerin indirmelerini sağlar ve güncelleştirmelerin yüklenmeye hazırolduğunu bildirir. Bu şekilde Güncelleştirme Yönetimi zamanlamaların denetiminde kalır, ancak güncelleştirmeler Güncelleştirme Yönetimi bakım penceresinin dışında indirilebilir. Bu davranış, **bakım penceresinin** güncelleştirme yönetimi hata ile aşılmasına engel olur.

Aşağıdaki komutu çalıştırarak PowerShell kullanarak bu ayar ayarını etkinleştirebilirsiniz:

```powershell
$WUSettings = (New-Object -com "Microsoft.Update.AutoUpdate").Settings
$WUSettings.NotificationLevel = 3
$WUSettings.Save()
```

## <a name="configure-reboot-settings"></a>Yeniden başlatma ayarlarını yapılandırma

[Yeniden başlatmayı yönetmek için kullanılan](/windows/deployment/update/waas-restart#registry-keys-used-to-manage-restart) kayıt defteri ve kayıt defteri anahtarlarını [düzenleyerek otomatik güncelleştirmeleri yapılandırma](/windows/deployment/update/waas-wu-settings#configuring-automatic-updates-by-editing-the-registry) bölümünde listelenen kayıt defteri anahtarları, **güncelleştirme dağıtım** ayarlarında **hiçbir şekilde yeniden başlatma** belirtseniz bile makinelerinizin yeniden başlatılmasına neden olabilir. Bu kayıt defteri anahtarlarını ortamınıza en iyi uyacak şekilde yapılandırın.

## <a name="enable-updates-for-other-microsoft-products"></a>Diğer Microsoft ürünleri için güncelleştirmeleri etkinleştir

Varsayılan olarak, Windows Update istemcisi yalnızca Windows için güncelleştirmeler sağlamak üzere yapılandırılmıştır. **Windows 'u güncelleştirdiğimde diğer Microsoft ürünlerine yönelik güncelleştirmeleri ver** ' i etkinleştirirseniz, Microsoft SQL Server ve diğer Microsoft yazılımları için güvenlik düzeltme ekleri de dahil olmak üzere diğer ürünler için de güncelleştirmeler alırsınız. Windows 2016 ve üzeri için kullanılabilen en son [yönetim şablonu dosyalarını](https://support.microsoft.com/help/3087759/how-to-create-and-manage-the-central-store-for-group-policy-administra) indirip kopyaladığınız takdirde bu seçenek yapılandırılabilir.

Windows Server 2012 R2 çalıştırıyorsanız, bu ayar grup ilkesi tarafından yapılandırılamaz. Bu makinelerde aşağıdaki PowerShell komutunu çalıştırın. Güncelleştirme Yönetimi Bu ayarla uyumludur.

```powershell
$ServiceManager = (New-Object -com "Microsoft.Update.ServiceManager")
$ServiceManager.Services
$ServiceID = "7971f918-a847-4430-9279-4a52d1efe18d"
$ServiceManager.AddService2($ServiceId,7,"")
```

## <a name="wsus-configuration-settings"></a>WSUS Yapılandırma ayarları

Güncelleştirme Yönetimi WSUS ayarlarını destekler. Güncelleştirme Yönetimi ile çalışmak için yapılandırabileceğiniz WSUS ayarları aşağıda listelenmiştir.

### <a name="intranet-microsoft-update-service-location"></a>Intranet Microsoft güncelleştirme hizmeti konumu

Güncelleştirmeleri taramak ve indirmek için, [intranet Microsoft Update hizmet konumunu belirtin](/windows/deployment/update/waas-wu-settings#specify-intranet-microsoft-update-service-location)altında kaynakları belirtebilirsiniz. Varsayılan olarak, Windows Update istemci Windows Update güncelleştirmeleri indirmek üzere yapılandırılmıştır. Makineleriniz için kaynak olarak bir WSUS sunucusu belirttiğinizde, güncelleştirmeler WSUS 'de onaylanmamışsa güncelleştirme dağıtımı başarısız olur. 

Makineleri yalnızca bu iç güncelleştirme hizmetiyle kısıtlamak için, yapılandırma [Windows Update Internet konumlarına bağlanmayın](https://docs.microsoft.com/windows-server/administration/windows-server-update-services/deploy/4-configure-group-policy-settings-for-automatic-updates#do-not-connect-to-any-windows-update-internet-locations). 

## <a name="next-steps"></a>Sonraki adımlar

Windows Update ayarlarını yapılandırdıktan sonra, [Azure VM 'leriniz için güncelleştirmeleri ve düzeltme eklerini yönetme](automation-tutorial-update-management.md)bölümündeki yönergeleri izleyerek bir güncelleştirme dağıtımı zamanlayabilirsiniz.
