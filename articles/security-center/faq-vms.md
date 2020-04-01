---
title: Azure Güvenlik Merkezi SSS - sanal makineler hakkında sorular
description: Tehditleri önlemenize, algılamanıza ve yanıt vermenize yardımcı olan bir ürün olan Azure Güvenlik Merkezi'ndeki sanal makinelerle ilgili sık sorulan sorular
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
ms.openlocfilehash: 0b7817539a26b7ebfa24efd086edf8b0ef82eae3
ms.sourcegitcommit: ced98c83ed25ad2062cc95bab3a666b99b92db58
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80436000"
---
# <a name="faq---questions-about-virtual-machines"></a>SSS - Sanal makineler le ilgili sorular


## <a name="what-types-of-virtual-machines-are-supported"></a>Ne tür sanal makineler desteklenir?

Hem klasik hem de [Kaynak Yöneticisi dağıtım modelleri](../azure-classic-rm.md)kullanılarak oluşturulan sanal makineler (VM'ler) için izleme ve öneriler mevcuttur.

Desteklenen platformların listesi için [Azure Güvenlik Merkezi'ndeki Desteklenen platformlara](security-center-os-coverage.md) bakın.


## <a name="why-doesnt-azure-security-center-recognize-the-antimalware-solution-running-on-my-azure-vm"></a>Azure Güvenlik Merkezi, Azure VM'mde çalışan kötü amaçlı yazılımdan koruma çözümlerini neden tanımıyor?

Azure Güvenlik Merkezi, Azure uzantıları aracılığıyla yüklenen kötü amaçlı yazılımdan koruma yla ilgili görünürlüğe sahiptir. Örneğin, Güvenlik Merkezi, sağladığınız bir görüntüye önceden yüklenmiş olan kötü amaçlı yazılımdan korumayı algılayamaz veya kendi işlemlerinizi (yapılandırma yönetim sistemleri gibi) kullanarak sanal makinelerinize kötü amaçlı yazılımdan koruma yüklediyseniz.


## <a name="why-do-i-get-the-message-missing-scan-data-for-my-vm"></a>VM'im için neden "Eksik Tarama Verileri" iletisini alıyorum?

Bu ileti, VM için tarama verisi olmadığında görüntülenir. Veri Toplama, Azure Güvenlik Merkezi'nde etkinleştirildikten sonra tarama verilerinin toplanması zaman (bir saatten az) alabilir. Tarama verileri ilk kez alındıktan sonra hiç tarama verisi veya güncel tarama verisi olmadığından bu iletiyle karşılaşabilirsiniz. Durdurulmuş VM'lerle ilgili değerler taramaya eklenmez. Bu ileti, tarama verileri son zamanlarda doldurulmsa da görünebilir (varsayılan değeri 30 gün olan Windows aracısının bekletme ilkesine uygun olarak).


## <a name="how-often-does-security-center-scan-for-operating-system-vulnerabilities-system-updates-and-endpoint-protection-issues"></a>Güvenlik Merkezi işletim sistemi güvenlik açıklarını, sistem güncelleştirmelerini ve uç nokta koruma sorunlarını ne sıklıkta tarar?

Güvenlik Merkezi güvenlik açıklarını, güncelleştirmeleri ve sorunları tarar için gecikme süreleri aşağıda verilmiştir:

- İşletim sistemi güvenlik yapılandırmaları – veriler 48 saat içinde güncelleştirilir
- Sistem güncellemeleri – veriler 24 saat içinde güncellenir
- Endpoint Protection sorunları – veriler 8 saat içinde güncelleştirilir

Güvenlik Merkezi genellikle her saat başı yeni verileri tarar ve önerileri buna göre yeniler. 

> [!NOTE]
> Güvenlik Merkezi, veri toplamak ve depolamak için Log Analytics aracısını kullanır. Daha fazla bilgi için Azure [Güvenlik Merkezi Platformu Geçişi'ne](security-center-platform-migration.md)bakın.


## <a name="why-do-i-get-the-message-vm-agent-is-missing"></a>Neden "VM Agent Eksik?" mesajını alıyorum?

Veri Toplama'yı etkinleştirmek için VM Aracısı VM'lere yüklenmelidir. VM Aracısı, Azure Marketi’nden dağıtılan VM’ler için varsayılan olarak yüklüdür. VM Aracısının diğer VM'lere nasıl yüklenmeye ne kadar açık olduğu hakkında daha fazla bilgi için, [VM Agent ve Uzantıları](https://azure.microsoft.com/blog/vm-agent-and-extensions-part-2/)adlı blog gönderisine bakın.