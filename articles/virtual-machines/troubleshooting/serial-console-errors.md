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
ms.openlocfilehash: 697f7267437f750bbb751239001145cb1c8af000
ms.sourcegitcommit: 5aefc96fd34c141275af31874700edbb829436bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2019
ms.locfileid: "74806839"
---
# <a name="common-errors-within-the-azure-serial-console"></a>Azure seri konsolu 'nda sık karşılaşılan hatalar
Azure seri konsolu 'nda bilinen hatalar kümesi vardır. Bu, bunlar için bu hataların ve risk azaltma adımlarının bir listesidir.

## <a name="common-errors"></a>Sık karşılaşılan hatalar

Hata                             |   Risk azaltma
:---------------------------------|:--------------------------------------------|
"Azure seri konsolu önyükleme tanılaması 'nın etkinleştirilmesini gerektirir. Sanal makineniz için önyükleme tanılamayı yapılandırmak üzere buraya tıklayın. " | VM veya sanal makine ölçek kümesinin [önyükleme tanılaması](boot-diagnostics.md) 'nın etkinleştirildiğinden emin olun. Bir sanal makine ölçek kümesi örneğinde seri konsol kullanıyorsanız, örneğinizin en son modele sahip olduğundan emin olun.
"Azure seri konsolu 'Nun çalışıyor olması için bir sanal makine gerekir. Sanal makinenizi başlatmak için yukarıdaki Başlat düğmesini kullanın. "  | Seri konsoluna erişmek için VM veya sanal makine ölçek kümesi örneğinin başlatılmış bir durumda olması gerekir (sanal makineniz durdurulmamalıdır veya serbest bırakılmamalıdır). VM veya sanal makine ölçek kümesi örneğinizin çalıştığından emin olun ve yeniden deneyin.
"Bu abonelik için Azure seri konsolu etkin değil, etkinleştirmek için abonelik yöneticinizle iletişime geçin." | Azure seri konsolu bir abonelik düzeyinde devre dışı bırakılabilir. Abonelik yöneticisiyseniz, [Azure seri konsolunu etkinleştirebilir ve devre dışı bırakabilirsiniz](./serial-console-enable-disable.md). Abonelik yöneticisi değilseniz, sonraki adımlar için abonelik yöneticinize ulaşmalısınız.
Bu sanal makinenin önyükleme tanılaması depolama hesabı erişirken "Yasak" yanıt karşılaşıldı. | Önyükleme tanılamalarının bir hesap güvenlik duvarı olmadığından emin olun. İşleve seri konsol için bir erişilebilir önyükleme tanılaması depolama hesabı gereklidir. Seri konsol tasarıma göre, önyükleme tanılama depolama hesabında etkin olan depolama hesabı güvenlik duvarları ile birlikte çalışılamıyor.
Bu VM 'yi seri konsol ile kullanmak için gerekli izinlere sahip değilsiniz. En az olduğundan emin olun sanal makine Katılımcısı rolü izinleri.| Seri konsol erişimi, VM 'niz veya sanal makine ölçek kümesi üzerinde katkıda bulunan düzeyinde erişime sahip olmanızı gerektirir. Daha fazla bilgi için [genel bakış sayfasına](serial-console-overview.md)bakın.
Bu VM 'de önyükleme tanılaması için kullanılan ' ' depolama hesabı bulunamadı. Bu VM için önyükleme tanılamaları 'nın etkinleştirildiğini, bu depolama hesabının silinmediğini ve bu depolama hesabına erişiminizin olduğunu doğrulayın. | VM 'niz veya sanal makine ölçek kümesi için önyükleme tanılama depolama hesabını silmediğinden emin olun
VM 'ye yönelik seri konsol bağlantısı bir hata ile karşılaştı: ' hatalı Istek ' (400) | Önyükleme tanılama URI 'niz yanlışsa bu durum oluşabilir. Örneğin, "http://" yerine "https://" kullanılmıştır. Önyükleme tanılaması URI 'SI şu komutla sabitlenebilir: `az vm boot-diagnostics enable --name vmName --resource-group rgName --storage https://<storageAccountUri>.blob.core.windows.net/`
Bu VM için önyükleme tanılama depolama hesabına yazmak için gerekli izinlere sahip değilsiniz. Lütfen en az sanal makine katılımcısı izninizin olduğundan emin olun | Seri konsol erişim, ön yükleme tanılama depolama hesabında katkıda bulunan düzeyinde erişim gerektirir. Daha fazla bilgi için [genel bakış sayfasına](serial-console-overview.md)bakın.
Önyükleme tanılaması depolama hesabı için kaynak grubu belirlenemiyor  *&lt;STORAGEACCOUNTNAME&gt;* . Bu VM için önyükleme tanılaması etkin ve bu depolama hesabına erişiminiz olduğunu doğrulayın. | Seri konsol erişim, ön yükleme tanılama depolama hesabında katkıda bulunan düzeyinde erişim gerektirir. Daha fazla bilgi için [genel bakış sayfasına](serial-console-overview.md)bakın.
Bu VM için sağlama henüz başarılı değil. Lütfen VM 'nin tam olarak dağıtıldığından emin olun ve seri konsol bağlantısını yeniden deneyin. | VM 'niz veya sanal makine ölçek kümesi hala sağlanıyor olabilir. Bir süre bekleyip yeniden deneyin.
Web yuvası kapalı veya açılamadı. | `*.console.azure.com`için güvenlik duvarı erişimi eklemeniz gerekebilir. Daha ayrıntılı ancak daha uzun bir yaklaşım, çok düzenli olarak değişen [Microsoft Azure veri MERKEZI IP aralıklarına](https://www.microsoft.com/download/details.aspx?id=41653)güvenlik duvarı erişimine izin vermedir.
Seri konsol hiyerarşik ad alanları ile Azure Data Lake Storage 2. kullanılarak depolama hesabıyla birlikte çalışmaz. | Bu, hiyerarşik ad alanları ile ilgili bilinen bir sorundur. Azaltmak için, sanal makinenizin önyükleme tanılama depolama hesabının Azure Data Lake Storage 2. kullanılarak oluşturulmadığından emin olun. Bu seçenek, yalnızca depolama hesabı oluşturma sırasında ayarlanabilir. Bu sorunu azaltmak için Azure Data Lake Storage 2. etkin olmadan ayrı bir önyükleme tanılama depolama hesabı oluşturmanız gerekebilir.


## <a name="next-steps"></a>Sonraki adımlar
* [Linux VM 'leri Için Azure seri konsolu](./serial-console-linux.md) hakkında daha fazla bilgi edinin
* [Windows VM 'leri Için Azure seri konsolu](./serial-console-windows.md) hakkında daha fazla bilgi edinin