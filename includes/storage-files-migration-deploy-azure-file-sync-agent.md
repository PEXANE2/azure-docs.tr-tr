---
title: Azure Dosya Eşitleme aracısını dağıtma
description: Azure Dosya Eşitleme aracısını dağıtın. Geçiş belgeleri arasında paylaşılan ortak bir metin bloğu.
author: fauhse
ms.service: storage
ms.topic: conceptual
ms.date: 2/20/2020
ms.author: fauhse
ms.subservice: files
ms.openlocfilehash: f038392f03b94aa2c2450531c9da4a11d9900295
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "93043143"
---
Bu bölümde, Windows Server örneğinize Azure Dosya Eşitleme aracısını yüklersiniz.

[Dağıtım Kılavuzu](../articles/storage/files/storage-sync-files-deployment-guide.md) , **Internet Explorer Artırılmış güvenlik yapılandırmasını** kapatmanız gerektiğini gösterir. Bu güvenlik ölçüsü Azure Dosya Eşitleme için geçerli değildir. Devre dışı bırakmak, herhangi bir sorun olmadan Azure 'da kimlik doğrulaması yapmanıza olanak sağlar.

PowerShell 'i açın ve aşağıdaki komutları kullanarak gerekli PowerShell modüllerini yükler. İstendiğinde tam modülü ve NuGet sağlayıcısını yüklediğinizden emin olun.

```powershell
Install-Module -Name Az -AllowClobber
Install-Module -Name Az.StorageSync
```

Sunucunuza internet 'e ulaşan herhangi bir sorununuz varsa, bu sorunları çözmek için zaman ayırın. Azure Dosya Eşitleme, internet 'e yönelik kullanılabilir ağ bağlantılarını kullanır. Bir proxy sunucusunun İnternet 'e ulaşmasını gerektirme de desteklenir. Yalnızca makine genelinde bir ara sunucu yapılandırabilir veya aracı yüklemesi sırasında yalnızca Azure Dosya Eşitleme kullanacağı bir ara sunucu belirtebilirsiniz.

Bir proxy yapılandırmak, bu sunucu için güvenlik duvarlarını açmanız gerektiği anlamına gelir, bu yaklaşım sizin için kabul edilebilir durumda olabilir. Sunucu yüklemesinin sonunda, sunucu kaydını tamamladıktan sonra, bir ağ bağlantısı raporu, seçtiğiniz bölge için iletişim kurması gereken Azure Dosya Eşitleme Azure 'da tam uç nokta URL 'Lerini gösterir. Rapor ayrıca iletişimin neden gerekli olduğunu size bildirir. Bu sunucunun etrafındaki güvenlik duvarlarını belirli URL 'Ler ile kilitlemek için bu raporu kullanabilirsiniz.

Ayrıca güvenlik duvarları genelinde açılmayın daha koruyucu bir yaklaşımını de izleyebilirsiniz, bunun yerine sunucuyu daha üst düzey DNS ad alanlarıyla iletişim kuracak şekilde sınırlandırın. Daha fazla bilgi için bkz. [proxy ve güvenlik duvarı ayarları Azure dosya eşitleme](../articles/storage/files/storage-sync-files-firewall-and-proxy.md). Kendi ağ en iyi uygulamalarınızı izleyin.

Sunucu yükleme sihirbazının sonunda, bir sunucu kayıt Sihirbazı açılır. Sunucuyu daha önce depolama eşitleme hizmetinizin Azure kaynağına kaydedin.

Bu adımlar, önce yüklemeniz gereken PowerShell modüllerini içeren dağıtım kılavuzunda daha ayrıntılı olarak açıklanmıştır: [Aracı yüklemesi Azure dosya eşitleme](../articles/storage/files/storage-sync-files-deployment-guide.md).

En son aracıyı kullanın. Bunu Microsoft Indirme merkezi 'nden indirebilirsiniz: [Azure dosya eşitleme Aracısı](https://aka.ms/AFS/agent "Aracı indirme Azure Dosya Eşitleme").

Başarılı bir yükleme ve sunucu kaydından sonra, bu adımı başarıyla tamamlayıp tamamlamadığınızı kontrol edebilirsiniz. Azure portal depolama eşitleme hizmeti kaynağına gidin. Sol taraftaki menüde **kayıtlı sunucular**' a gidin. Sunucunuzu orada listelendiğini görürsünüz.
