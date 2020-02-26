---
title: Azure Güvenlik Merkezi hakkında SSS-Sanal makineler hakkında sorular
description: Azure Güvenlik Merkezi 'ndeki sanal makinelerle ilgili sık sorulan sorular, tehditleri önlemenize, algılamanıza ve yanıtlamanıza yardımcı olan bir ürün
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: be2ab6d5-72a8-411f-878e-98dac21bc5cb
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/25/2020
ms.author: memildin
ms.openlocfilehash: d84085115816a8fe1cba65e191ea391dd91a4aed
ms.sourcegitcommit: 99ac4a0150898ce9d3c6905cbd8b3a5537dd097e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/25/2020
ms.locfileid: "77599372"
---
# <a name="faq---questions-about-virtual-machines"></a>SSS-Sanal makineler hakkında sorular


## <a name="what-types-of-virtual-machines-are-supported"></a>Sanal makinelerin hangi türleri desteklenir?

İzleme ve öneriler, hem klasik hem de [Kaynak Yöneticisi dağıtım modelleri](../azure-classic-rm.md)kullanılarak oluşturulan sanal makineler (VM) için kullanılabilir.

Desteklenen platformların listesi için bkz. [Azure Güvenlik Merkezi 'Nde desteklenen platformlar](security-center-os-coverage.md) .


## <a name="why-doesnt-azure-security-center-recognize-the-antimalware-solution-running-on-my-azure-vm"></a>Neden Azure Güvenlik Merkezi, Azure VM'deki uygulamalarımdan birine çalışan kötü amaçlı yazılımdan koruma çözümü tanımıyor?

Azure Güvenlik Merkezi, Azure uzantıları yüklü kötü amaçlı yazılımdan koruma görünürlük sahiptir. Örneğin, Güvenlik Merkezi, sağlanan bir görüntüyü veya üzerinde kendi işlemleri (örneğin, yapılandırma yönetim sistemleri) kullanarak sanal makinelerinizde kötü amaçlı yazılımdan koruma yüklediyseniz önceden yüklenmiş kötü amaçlı yazılımdan koruma algılamasını mümkün değil.


## <a name="why-do-i-get-the-message-missing-scan-data-for-my-vm"></a>Neden ileti "Eksik tarama verileri" sanal Makinem için alıyorum?

Bu ileti, VM için tarama verisi olmadığında görüntülenir. Veri Toplama, Azure Güvenlik Merkezi'nde etkinleştirildikten sonra tarama verilerinin toplanması zaman (bir saatten az) alabilir. Tarama verileri ilk kez alındıktan sonra hiç tarama verisi veya güncel tarama verisi olmadığından bu iletiyle karşılaşabilirsiniz. Durdurulmuş VM'lerle ilgili değerler taramaya eklenmez. Tarama verileri kısa bir süre önce (uygun olarak 30 günlük varsayılan değer olan Windows aracısına yönelik, bekletme ilkesi) doldurduğu değil, bu iletiyi de görüntülenebilir.


## <a name="how-often-does-security-center-scan-for-operating-system-vulnerabilities-system-updates-and-endpoint-protection-issues"></a>Güvenlik Merkezi, işletim sistemi güvenlik açıkları, sistem güncelleştirmeleri ve endpoint protection sorunları ne sıklıkta tarama?

Güvenlik açıkları, güncelleştirmeler ve sorunlar için Güvenlik Merkezi taramalarının gecikme süreleri aşağıda verilmiştir:

- İşletim sistemi güvenlik yapılandırmaları – veri 48 saat içinde güncelleştirilir
- Sistem güncelleştirmeleri – veri 24 saat içinde güncelleştirilir
- Uç nokta koruma sorunları – 8 saat içinde verileri güncelleştirildi.

Güvenlik Merkezi genellikle her saat yeni verileri tarar ve buna göre öneriler yeniler. 

> [!NOTE]
> Güvenlik Merkezi, veri toplamak ve depolamak için Microsoft Monitoring Agent kullanır. Daha fazla bilgi için bkz. [Azure Güvenlik Merkezi platform geçişi](security-center-platform-migration.md).


## <a name="why-do-i-get-the-message-vm-agent-is-missing"></a>"VM Aracısı eksik?" iletiyi neden alıyorum

Veri toplama özelliğini etkinleştirmeyi Vm'lerinde VM aracısı yüklü olmalıdır. VM Aracısı, Azure Marketi’nden dağıtılan VM’ler için varsayılan olarak yüklüdür. VM aracısının diğer VM 'Lere nasıl yükleneceği hakkında bilgi için bkz. Blog Post [VM Aracısı ve uzantıları](https://azure.microsoft.com/blog/vm-agent-and-extensions-part-2/).