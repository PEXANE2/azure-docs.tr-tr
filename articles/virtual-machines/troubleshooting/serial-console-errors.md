---
title: Azure seri konsol hataları | Microsoft Docs
description: Azure seri konsolu 'nda sık karşılaşılan hatalar
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
ms.openlocfilehash: 7bd9fe4044dace4061285c016cb08562b556b98e
ms.sourcegitcommit: 8cf199fbb3d7f36478a54700740eb2e9edb823e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/25/2019
ms.locfileid: "74483576"
---
# <a name="common-errors-within-the-azure-serial-console"></a>Azure seri konsolu 'nda sık karşılaşılan hatalar
Azure seri konsolu 'nda bilinen hatalar kümesi vardır. Bu, bunlar için bu hataların ve risk azaltma adımlarının bir listesidir.

## <a name="common-errors"></a>Sık karşılaşılan hatalar

Hata                             |   Risk azaltma
:---------------------------------|:--------------------------------------------|
"Azure seri konsolu önyükleme tanılaması 'nın etkinleştirilmesini gerektirir. Sanal makineniz için önyükleme tanılamayı yapılandırmak üzere buraya tıklayın. " ![Önyükleme tanılaması hatası](./media/virtual-machines-serial-console/virtual-machines-serial-console-boot-diagnostics-error.png) | VM veya sanal makine ölçek kümesinin [önyükleme tanılaması](boot-diagnostics.md) 'nın etkinleştirildiğinden emin olun. Bir sanal makine ölçek kümesi örneğinde seri konsol kullanıyorsanız, örneğinizin en son modele sahip olduğundan emin olun.
"Azure seri konsolu 'Nun çalışıyor olması için bir sanal makine gerekir. Sanal makinenizi başlatmak için yukarıdaki Başlat düğmesini kullanın. " ![Serbest bırakıldı hatası](./media/virtual-machines-serial-console/virtual-machines-serial-console-deallocating-error.png) | Seri konsoluna erişmek için VM veya sanal makine ölçek kümesi örneğinin başlatılmış bir durumda olması gerekir (sanal makineniz durdurulmamalıdır veya serbest bırakılmamalıdır). VM veya sanal makine ölçek kümesi örneğinizin çalıştığından emin olun ve yeniden deneyin.
"Bu abonelik için Azure seri konsolu etkin değil, etkinleştirmek için abonelik yöneticinizle iletişime geçin." ![Abonelik devre dışı hatası](./media/virtual-machines-serial-console/virtual-machines-serial-console-subscription-disabled-error.png) | Azure seri konsolu bir abonelik düzeyinde devre dışı bırakılabilir. Abonelik yöneticisiyseniz, [Azure seri konsolunu etkinleştirebilir ve devre dışı bırakabilirsiniz](./serial-console-enable-disable.md). Abonelik yöneticisi değilseniz, sonraki adımlar için abonelik yöneticinize ulaşmalısınız.
Bu sanal makinenin önyükleme tanılaması depolama hesabı erişirken "Yasak" yanıt karşılaşıldı. ![Depolama hesabı güvenlik duvarı hatası](./media/virtual-machines-serial-console/virtual-machines-serial-console-firewall-error.png)| Önyükleme tanılamalarının bir hesap güvenlik duvarı olmadığından emin olun. İşleve seri konsol için bir erişilebilir önyükleme tanılaması depolama hesabı gereklidir. Seri konsol tasarıma göre, önyükleme tanılama depolama hesabında etkin olan depolama hesabı güvenlik duvarları ile birlikte çalışılamıyor.
Bu VM 'yi seri konsol ile kullanmak için gerekli izinlere sahip değilsiniz. En az olduğundan emin olun sanal makine Katılımcısı rolü izinleri.| Seri konsol erişimi, VM 'niz veya sanal makine ölçek kümesi üzerinde katkıda bulunan düzeyinde erişime sahip olmanızı gerektirir. Daha fazla bilgi için [genel bakış sayfasına](serial-console-overview.md)bakın.
Bu VM 'de önyükleme tanılaması için kullanılan ' ' depolama hesabı bulunamadı. Bu VM için önyükleme tanılamaları 'nın etkinleştirildiğini, bu depolama hesabının silinmediğini ve bu depolama hesabına erişiminizin olduğunu doğrulayın. | VM 'niz veya sanal makine ölçek kümesi için önyükleme tanılama depolama hesabını silmediğinden emin olun
Bu VM için sağlama henüz başarılı değil. Lütfen VM 'nin tam olarak dağıtıldığından emin olun ve seri konsol bağlantısını yeniden deneyin. | VM 'niz veya sanal makine ölçek kümesi hala sağlanıyor olabilir. Bir süre bekleyip yeniden deneyin.
Bu VM için önyükleme tanılama depolama hesabına yazmak için gerekli izinlere sahip değilsiniz. Lütfen ' ' üzerinde en az sanal makine katılımcısı izinlerinizin olduğundan emin olun. | Seri konsol erişim, ön yükleme tanılama depolama hesabında katkıda bulunan düzeyinde erişim gerektirir. Daha fazla bilgi için [genel bakış sayfasına](serial-console-overview.md)bakın.
*STORAGEACCOUNTNAME&gt;&lt;* önyükleme tanılama depolama hesabı için kaynak grubu belirlenemiyor. Bu VM için önyükleme tanılaması etkin ve bu depolama hesabına erişiminiz olduğunu doğrulayın. | Seri konsol erişim, ön yükleme tanılama depolama hesabında katkıda bulunan düzeyinde erişim gerektirir. Daha fazla bilgi için [genel bakış sayfasına](serial-console-overview.md)bakın.
Web yuvası kapalı veya açılamadı. | `*.console.azure.com`için güvenlik duvarı erişimi eklemeniz gerekebilir. Daha ayrıntılı ancak daha uzun bir yaklaşım, çok düzenli olarak değişen [Microsoft Azure veri MERKEZI IP aralıklarına](https://www.microsoft.com/download/details.aspx?id=41653)güvenlik duvarı erişimine izin vermedir.
Seri konsol hiyerarşik ad alanları ile Azure Data Lake Storage 2. kullanılarak depolama hesabıyla birlikte çalışmaz. | Bu, hiyerarşik ad alanları ile ilgili bilinen bir sorundur. Azaltmak için, sanal makinenizin önyükleme tanılama depolama hesabının Azure Data Lake Storage 2. kullanılarak oluşturulmadığından emin olun. Bu seçenek, yalnızca depolama hesabı oluşturma sırasında ayarlanabilir. Bu sorunu azaltmak için Azure Data Lake Storage 2. etkin olmadan ayrı bir önyükleme tanılama depolama hesabı oluşturmanız gerekebilir.


## <a name="next-steps"></a>Sonraki adımlar
* [Linux VM 'leri Için Azure seri konsolu](./serial-console-linux.md) hakkında daha fazla bilgi edinin
* [Windows VM 'leri Için Azure seri konsolu](./serial-console-windows.md) hakkında daha fazla bilgi edinin