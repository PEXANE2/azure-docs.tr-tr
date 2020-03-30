---
title: Windows Update ayarlarını Azure Update Yönetimi ile çalışacak şekilde yapılandırın
description: Bu makalede, Azure Update Yönetimi ile çalışmak üzere yapılandırdığınız Windows Update ayarları açıklanmaktadır.
services: automation
ms.subservice: update-management
ms.date: 03/02/2020
ms.topic: conceptual
ms.openlocfilehash: 7f226c4d297d25644b2650d085655f70d8326927
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79279032"
---
# <a name="configure-windows-update-settings-for-update-management"></a>Güncelleştirme Yönetimi için Windows Update ayarlarını yapılandırma

Azure Güncelleştirme Yönetimi, Windows güncelleştirmelerini indirmek ve yüklemek için [Windows Update istemcisine](https://docs.microsoft.com//windows/deployment/update/windows-update-overview) güvenir. Windows Server Update Services (WSUS) veya Windows Update'e bağlanırken Windows Update istemcisi tarafından kullanılan belirli ayarlar vardır. Bu ayarların çoğu şu şekilde yönetilebilir:

- Yerel Grup İlkesi Düzenleyicisi
- Grup İlkesi
- PowerShell
- Kayıt Defterini doğrudan düzenleme

Güncelleştirme Yönetimi, Windows Update istemcisini denetlemek için belirtilen ayarların çoğuna saygı duyar. Windows dışı güncelleştirmeleri etkinleştirmek için ayarları kullanırsanız, Güncelleştirme Yönetimi de bu güncelleştirmeleri yönetir. Güncelleştirme dağıtımı gerçekleşmeden önce güncelleştirmelerin karşıdan yükleştirilmesini etkinleştirmek istiyorsanız, güncelleştirme dağıtımı daha hızlı, daha verimli ve bakım penceresini aşma olasılığı daha düşük olabilir.

## <a name="pre-download-updates"></a>Ön yükleme güncelleştirmeleri

Güncelleştirmelerin otomatik olarak karşıdan yüklenmesini ancak otomatik olarak yüklemeyin, [Yapılandırma Otomatik Güncelleştirmeler ayarını](/windows-server/administration/windows-server-update-services/deploy/4-configure-group-policy-settings-for-automatic-updates##configure-automatic-updates) **3**olarak ayarlamak için Grup İlkesi'ni kullanabilirsiniz. Bu ayar, arka planda gerekli güncelleştirmelerin karşıdan yüklenmesini sağlar ve güncelleştirmelerin yüklenmeye hazır olduğunu size haber verir. Bu şekilde, Güncelleştirme Yönetimi zamanlamaları denetlemeye devam eder, ancak güncelleştirmeler Güncelleştirme Yönetimi bakım penceresinin dışına indirilebilir. Bu davranış, Güncelleştirme **Yönetimi'ndeki Bakım penceresinin aşıldığı** hataları önler.

Aşağıdaki komutu çalıştırarak PowerShell kullanarak bu ayar ayarını etkinleştirebilirsiniz:

```powershell
$WUSettings = (New-Object -com "Microsoft.Update.AutoUpdate").Settings
$WUSettings.NotificationLevel = 3
$WUSettings.Save()
```

## <a name="configure-reboot-settings"></a>Yeniden başlatma ayarlarını yapılandırma

[Yeniden başlatmayı yönetmek için kullanılan](/windows/deployment/update/waas-restart#registry-keys-used-to-manage-restart) kayıt defteri ve Kayıt Defteri anahtarlarını [düzenleyerek Otomatik Güncelleştirmeleri Yapılandırma'da](/windows/deployment/update/waas-wu-settings#configuring-automatic-updates-by-editing-the-registry) listelenen kayıt defteri anahtarları, **Dağıtım'ı Güncelleştirme** ayarlarında Asla Yeniden **Başlatma'yı** belirtmeseniz bile makinelerinizin yeniden başlatılmasına neden olabilir. Bu kayıt defteri anahtarlarını ortamınıza en iyi uyacak şekilde yapılandırın.

## <a name="enable-updates-for-other-microsoft-products"></a>Diğer Microsoft ürünleri için güncelleştirmeleri etkinleştirme

Varsayılan olarak, Windows Update istemcisi yalnızca Windows için güncelleştirmeler sağlamak üzere yapılandırılır. Windows ayarını **güncellediğimde diğer Microsoft ürünleri için ver güncelleştirmelerini** etkinleştirirken, Microsoft SQL Server ve diğer Microsoft yazılımları için güvenlik düzeltme emaları da dahil olmak üzere diğer ürünler için güncelleştirmeler alırsınız. Windows 2016 ve üzeri için kullanılabilen en son [Yönetim şablonu dosyalarını](https://support.microsoft.com/help/3087759/how-to-create-and-manage-the-central-store-for-group-policy-administra) indirip kopyaladıysanız, bu seçenek yapılandırılabilir.

Windows Server 2012 R2 çalıştırıyorsanız, bu ayar Grup İlkesi tarafından yapılandırılamaz. Bu makinelerde aşağıdaki PowerShell komutunu çalıştırın. Güncelleştirme Yönetimi bu ayara uygundur.

```powershell
$ServiceManager = (New-Object -com "Microsoft.Update.ServiceManager")
$ServiceManager.Services
$ServiceID = "7971f918-a847-4430-9279-4a52d1efe18d"
$ServiceManager.AddService2($ServiceId,7,"")
```

## <a name="wsus-configuration-settings"></a>WSUS yapılandırma ayarları

Update Management WSUS ayarlarını destekler. Update Management ile çalışmak için yapılandırabileceğiniz WSUS ayarları aşağıda listelenmiştir.

### <a name="intranet-microsoft-update-service-location"></a>Intranet Microsoft güncelleştirme hizmeti konumu

[Intranet Microsoft Update hizmet konumunu belirt in](/windows/deployment/update/waas-wu-settings#specify-intranet-microsoft-update-service-location)altında güncelleştirmeleri tarama ve indirme için kaynakları belirtebilirsiniz. Varsayılan olarak, Windows Update istemcisi Windows Update güncelleştirmeleri indirmek için yapılandırılmıştır. Makineleriniz için kaynak olarak bir WSUS sunucusu belirttiğinizde, güncelleştirmeler WSUS'ta onaylanmamışsa, güncelleştirme dağıtımı başarısız olur. 

Makineleri yalnızca bu dahili güncelleştirme hizmetiyle sınırlamak için, herhangi [bir Windows Update Internet konumuna bağlanmayın'ı](https://docs.microsoft.com/windows-server/administration/windows-server-update-services/deploy/4-configure-group-policy-settings-for-automatic-updates#do-not-connect-to-any-windows-update-internet-locations)yapılandırın. 

## <a name="next-steps"></a>Sonraki adımlar

Windows Update ayarlarını yapılandırdıktan sonra, [Azure VM'leriniz için güncelleştirmeleri ve düzeltme](automation-tutorial-update-management.md)emleketleri'ndeki yönergeleri izleyerek bir güncelleştirme dağıtımı zamanlayabilirsiniz.
