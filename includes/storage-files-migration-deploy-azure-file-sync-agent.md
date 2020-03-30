---
title: Azure Dosya Eşitleme aracısını dağıtma
description: Azure Dosya Eşitleme aracısını dağıtma. Geçiş dokümanları arasında paylaşılan ortak bir metin bloğu.
author: fauhse
ms.service: storage
ms.topic: conceptual
ms.date: 2/20/2020
ms.author: fauhse
ms.subservice: files
ms.openlocfilehash: 694becc49667204ef2071a140bb6330285089039
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78209421"
---
Bu bölümde, Windows Server'ınıza Azure Dosya Eşitleme aracısını yüklersiniz.
[Dağıtım kılavuzu,](../articles/storage/files/storage-sync-files-deployment-guide.md) **IE gelişmiş güvenliği**kapatmanız gerektiğini göstermektedir. IE gelişmiş güvenlik, Azure Dosya Eşitlemesi ile geçerli olmayan bir güvenlik önlemidir ve bu güvenliği kapatmak herhangi bir sorun olmadan Azure'a kimlik doğrulamanızı sağlar.

PowerShell'i açın ve gerekli PowerShell modüllerini aşağıdaki komutlarla kurun. İstendiğinde modülün tamamını ve NuGet sağlayıcısını yüklediğinizden emin olun:

```powershell
Install-Module -Name Az -AllowClobber
Install-Module -Name Az.StorageSync
```

Eğer sunucudan internet ulaşan herhangi bir sorun varsa, şimdi bunları çözmek için zamanı. Azure Dosya Eşitlemi, Internet'e sunulan tüm ağ bağlantısını kullanır.
Bir proxy sunucusunun internete ulaşmasını gerektirmesi de desteklenir. Aracı yükleme sırasında makine genelinde bir proxy'yi şimdi yapılandırabilir veya yalnızca dosya eşitlemenin kullanacağı bir proxy belirtebilirsiniz.

Bu, bu sunucu için güvenlik duvarlarınızı açmanız gerektiği anlamına geliyorsa, bu sizin için kabul edilebilir bir yaklaşım olabilir. Sunucu yüklemesinin sonunda, sunucu kaydı tamamlandıktan sonra, seçtiğiniz bölge için dosya eşitlemesi için iletişim kurmanız gereken Azure'daki tam bitiş noktası URL'lerini gösteren bir ağ bağlantı raporu olacaktır. Rapor ayrıca iletişimin neden gerekli olduğunu da bildirir. Raporu, bu sunucunun etrafındaki güvenlik duvarlarını belirli URL'lere kilitlemek için kullanabilirsiniz.

Güvenlik duvarlarını geniş açmadığınız, ancak bunun yerine sunucuyu daha üst düzey DNS ad boşluklarıyla iletişim kurmak için sınırlandırdığınız daha tutucu bir yaklaşım da izleyebilirsiniz - [Azure Dosya Eşitleme proxy ve güvenlik duvarı ayarları](../articles/storage/files/storage-sync-files-firewall-and-proxy.md) makalesinde daha fazla belge ve ayrıntı mevcuttur. Kendi ağ en iyi uygulamaları izleyin.

Sunucu *yükleme* sihirbazının sonunda, bir sunucu *kayıt* sihirbazı açılır.
Sunucuyu daha önceki Depolama Eşitleme Hizmeti Azure kaynağınıza kaydedin.

Bu adımlar, önce yüklemeniz gereken yukarıdaki PowerShell modülleri de dahil olmak üzere dağıtım kılavuzunda daha ayrıntılı olarak açıklanmıştır: [Azure Dosya Eşitleme aracısı yükleme.](../articles/storage/files/storage-sync-files-deployment-guide.md)

En son aracı kullanılmalıdır ve Microsoft Download Center indirilebilir: [Azure Dosya Eşitleme - aracı](https://aka.ms/AFS/agent "Azure Dosya Eşitleme aracısı indir").

Başarılı bir yükleme ve sunucu kaydından sonra, bu adımı başarıyla tamamlayıp tamamlamadığınızı kontrol edebilirsiniz: Azure portalındaki Depolama Eşitleme Hizmeti kaynağına gidin ve sol menüyü "Kayıtlı sunucular"a kadar takip edin. Sunucunuzun hemen orada listelenmiş olduğunu göreceksiniz.
