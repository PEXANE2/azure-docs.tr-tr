---
title: Azure Seri Konsol hataları | Microsoft Dokümanlar
description: Azure Seri Konsolu'ndaki sık karşılaşılan hatalar
services: virtual-machines
documentationcenter: ''
author: asinn826
manager: borisb
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm
ms.workload: infrastructure-services
ms.date: 8/20/2019
ms.author: alsin
ms.openlocfilehash: 61ae0ef92fe522a2a038a6076a5e0c0a10ee47b6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80060699"
---
# <a name="common-errors-within-the-azure-serial-console"></a>Azure Seri Konsolu'ndaki sık karşılaşılan hatalar
Azure Seri Konsolu'nda bilinen bir dizi hata vardır. Bu, bu hataların ve azaltma adımlarının bir listesidir.

## <a name="common-errors"></a>Sık karşılaşılan hatalar

Hata                             |   Risk azaltma
:---------------------------------|:--------------------------------------------|
"Azure Seri Konsolu etkinleştirilen önyükleme tanılama gerektirir. Sanal makineniz için önyükleme tanılamasını yapılandırmak için burayı tıklatın." | VM veya sanal makine ölçek kümesinin [önyükleme tanılama](boot-diagnostics.md) özelliğietkinleştirildiğinden emin olun. Seri konsolu sanal makine ölçeği kümesi örneğinde kullanıyorsanız, örneğinizin en son modele sahip olduğundan emin olun.
"Azure Seri Konsolu'nun çalışıyor olması için sanal bir makine gerekir. Sanal makinenizi başlatmak için yukarıdaki Başlat düğmesini kullanın."  | Seri konsoluna erişmek için VM veya sanal makine ölçeği kümesi örneği başlangıç durumunda olmalıdır (VM'niz durdurulmamalı veya ayrılmamalıdır). VM veya sanal makine ölçeği set örneğinizin çalıştığından emin olun ve yeniden deneyin.
"Azure Seri Konsolu bu abonelik için etkinleştirilemedi, etkinleştirmek için abonelik yöneticinize başvurun." | Azure Seri Konsolu abonelik düzeyinde devre dışı alınabilir. Abonelik yöneticisiyseniz, [Azure Seri Konsolu'nu etkinleştirebilir ve devre dışı kabilirsiniz.](./serial-console-enable-disable.md) Abonelik yöneticisi değilseniz, sonraki adımlar için abonelik yöneticinize ulaşmanız gerekir.
Bu VM'nin önyükleme tanılama depolama hesabına erişirken bir "Yasak" yanıtına karşılaşıldı. | Önyükleme tanılamanın bir hesap güvenlik duvarı olmadığından emin olun. Seri konsolun çalışması için erişilebilir bir önyükleme tanılama depolama hesabı gereklidir. Seri konsol tasarım tarafından önyükleme tanılama depolama hesabında etkin depolama hesabı güvenlik duvarları ile çalışamaz.
Seri konsolu ile bu VM kullanmak için gerekli izinlere sahip değil. En azından Sanal Makine Katılımcısı rol izinlerine sahip olduğundan emin olun.| Seri konsol erişimi, VM veya sanal makine ölçeği setinizde katılımcı düzeyinde erişime veya daha yüksek bir düzeye sahip olmayı gerektirir. Daha fazla bilgi için [genel bakış sayfasına](serial-console-overview.md)bakın.
Bu VM'de önyükleme tanılamaiçin kullanılan depolama hesabı '' bulunamadı. Bu VM için önyükleme tanılamanın etkin olduğunu, bu depolama hesabının silinmediğini ve bu depolama hesabına erişiminiz olduğunu doğrulayın. | VM veya sanal makine ölçek setinizin önyükleme tanılama depolama hesabını silmediğinizi çift kontrol edin
VM'ye seri konsol bağlantısı bir hatayla karşılaştı: 'Kötü İstek' (400) | Önyükleme tanılama URI yanlışsa bu olabilir. Örneğin, "https://" yerine "http://" kullanılmıştır. Önyükleme tanılama URI bu komutile düzeltilebilir:`az vm boot-diagnostics enable --name vmName --resource-group rgName --storage https://<storageAccountUri>.blob.core.windows.net/`
Bu VM için önyükleme tanılama depolama hesabına yazmak için gerekli izinlere sahip değilseniz. Lütfen en az VM Katılımcısı iznine sahip olduğundan emin olun | Seri konsol erişimi, önyükleme tanılama depolama hesabında katılımcı düzeyinde erişim gerektirir. Daha fazla bilgi için [genel bakış sayfasına](serial-console-overview.md)bakın.
Önyükleme tanılama depolama hesabı * &lt;STORAGEACCOUNTNAME&gt;* için kaynak grubu belirlenemedi. Bu VM için önyükleme tanılamanın etkin olduğunu ve bu depolama hesabına erişebildiğinizi doğrulayın. | Seri konsol erişimi, önyükleme tanılama depolama hesabında katılımcı düzeyinde erişim gerektirir. Daha fazla bilgi için [genel bakış sayfasına](serial-console-overview.md)bakın.
Bu VM için sağlama henüz başarılı olmamıştır. Lütfen VM'nin tamamen dağıtıldığından emin olun ve seri konsol bağlantısını yeniden deneyin. | VM veya sanal makine ölçek kümeniz hala kullanılabilirlik olabilir. Biraz bekleyin ve tekrar deneyin.
Web soketi kapatıldı veya açılamadı. | `*.console.azure.com`Güvenlik duvarı erişimi eklemeniz gerekebilir. Daha ayrıntılı ama daha uzun bir yaklaşım, oldukça düzenli olarak değişen [Microsoft Azure Veri Merkezi IP aralıklarına](https://www.microsoft.com/download/details.aspx?id=41653)güvenlik duvarı erişimine izin vermektir.
Seri konsol, hiyerarşik ad alanlarına sahip Azure Veri Gölü Depolama Gen2'yi kullanarak bir depolama hesabıyla çalışmaz. | Bu hiyerarşik ad alanları ile bilinen bir sorundur. Azaltmak için, VM'nizin önyükleme tanılama depolama hesabınızın Azure Veri Gölü Depolama Gen2 kullanılarak oluşturulmadığını sağlayın. Bu seçenek yalnızca depolama hesabı oluşturma üzerine ayarlanabilir. Bu sorunu azaltmak için Azure Veri Gölü Depolama Gen2 etkin olmadan ayrı bir önyükleme tanılama depolama hesabı oluşturmanız gerekebilir.
VM'ye seri konsol bağlantısı nda bir hata oluştu: 'Forbidden'(SubscriptionNotEnabled) \<- Abonelik adı tanımlanmamış, kimlik abonelik kimliği> tanımlanmamış durumda | Bu sorun, bir kullanıcının Bulut Bulut depolama hesabını oluşturduğu abonelik devre dışı bırakılmışsa oluşabilir. Bulut Shell'i azaltmak için Bulut Shell'i başlatın ve geçerli abonelikte Cloud Shell için bir destek depolama hesabı sağlamak için [gereken adımları gerçekleştirin.](https://docs.microsoft.com/azure/cloud-shell/persisting-shell-storage#unmount-clouddrive-1)

## <a name="next-steps"></a>Sonraki adımlar
* [Linux VM'leri için Azure Seri Konsolu](./serial-console-linux.md) hakkında daha fazla bilgi edinin
* [Windows VM'ler için Azure Seri Konsolu](./serial-console-windows.md) hakkında daha fazla bilgi edinin