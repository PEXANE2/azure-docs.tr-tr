---
title: Azure Dosya Eşitleme aracısını dağıtma
description: Azure Dosya Eşitleme Aracısı dağıtılıyor. Geçiş belgeleri arasında paylaşılan ortak bir metin bloğu.
author: fauhse
ms.service: storage
ms.topic: conceptual
ms.date: 2/20/2020
ms.author: fauhse
ms.subservice: files
ms.openlocfilehash: 694becc49667204ef2071a140bb6330285089039
ms.sourcegitcommit: 5192c04feaa3d1bd564efe957f200b7b1a93a381
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2020
ms.locfileid: "78209421"
---
Bu bölümde, Azure Dosya Eşitleme aracısını Windows Server 'a yüklersiniz.
[Dağıtım Kılavuzu](../articles/storage/files/storage-sync-files-deployment-guide.md) , **IE artırılmış güvenliği**kapatmanız gerektiğini gösterir. IE artırılmış güvenlik, Azure Dosya Eşitleme geçerli olmayan bir güvenlik ölçümüdür ve devre dışı bırakmak, herhangi bir sorun olmadan Azure 'da kimlik doğrulaması yapmanıza olanak sağlar.

PowerShell 'i açın ve gerekli PowerShell modüllerini aşağıdaki komutlarla birlikte yükler. İstendiğinde tam modülü ve NuGet sağlayıcısını yüklediğinizden emin olun:

```powershell
Install-Module -Name Az -AllowClobber
Install-Module -Name Az.StorageSync
```

Sunucunuza internet 'e ulaşan herhangi bir sorununuz varsa, bu sorunları çözmek için zaman ayırın. Azure Dosya Eşitleme, internet 'e yönelik kullanılabilir ağ bağlantılarını kullanır.
Bir proxy sunucusunun İnternet 'e ulaşmasını gerektirme de desteklenir. Şimdi makine genelinde bir proxy yapılandırabilir veya aracı yüklemesi sırasında yalnızca dosya eşitlemenin kullanacağı bir ara sunucu belirtebilirsiniz.

Bu, bu sunucu için güvenlik duvarlarını açmanız gerektiği anlamına gelir ve bu, sizin için kabul edilebilir bir yaklaşım olabilir. Sunucu yüklemesinin sonunda, tamamlanan sunucu kaydı tamamlandıktan sonra, Azure 'daki tam uç nokta URL 'Lerinin seçtiğiniz bölge için iletişim kurması gerektiğini gösteren bir ağ bağlantısı raporu olacaktır. Rapor ayrıca iletişimin neden gerekli olduğunu size bildirir. Bu raporu, bu sunucunun çevresindeki güvenlik duvarlarını belirli URL 'lere kilitlemek için kullanabilirsiniz.

Ayrıca, güvenlik duvarları genelinde açılmamanıza karşın daha yüksek bir yaklaşımı izleyebilirsiniz, bunun yerine sunucuyu daha üst düzey DNS adı alanlarıyla iletişim kuracak şekilde sınırlayabilirsiniz. [Azure dosya eşitleme proxy ve güvenlik duvarı ayarları](../articles/storage/files/storage-sync-files-firewall-and-proxy.md) makalesinde daha fazla belge ve ayrıntı mevcuttur. Kendi ağ en iyi uygulamalarınızı izleyin.

Sunucu *yükleme* sihirbazının sonunda, bir sunucu *kayıt* Sihirbazı açılır.
Sunucuyu daha önce depolama eşitleme hizmeti Azure kaynağına kaydedin.

Bu adımlar, önce yüklemeniz gereken yukarıdaki PowerShell modülleri de dahil olmak üzere dağıtım kılavuzunda daha ayrıntılı olarak açıklanmıştır: [Aracı yüklemesi Azure dosya eşitleme](../articles/storage/files/storage-sync-files-deployment-guide.md).

En son aracı kullanılmalıdır ve Microsoft Indirme merkezi 'nden indirilebilir: [Azure dosya eşitleme-aracı](https://aka.ms/AFS/agent "Aracı indirme Azure Dosya Eşitleme").

Başarılı bir yükleme ve sunucu kaydından sonra, bu adımı başarıyla tamamladığınıza bakabilirsiniz: Azure portal depolama eşitleme hizmeti kaynağına gidin ve ardından sol taraftaki menüyü "kayıtlı sunucular" a uygulayın. Sunucunuzu doğru bir şekilde listelendiğini göreceksiniz.
