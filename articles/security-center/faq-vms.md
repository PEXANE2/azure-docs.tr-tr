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
ms.openlocfilehash: 0b7817539a26b7ebfa24efd086edf8b0ef82eae3
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "80436000"
---
# <a name="faq---questions-about-virtual-machines"></a>SSS-Sanal makineler hakkında sorular


## <a name="what-types-of-virtual-machines-are-supported"></a>Hangi tür sanal makineler destekleniyor?

İzleme ve öneriler, hem klasik hem de [Kaynak Yöneticisi dağıtım modelleri](../azure-classic-rm.md)kullanılarak oluşturulan sanal makineler (VM) için kullanılabilir.

Desteklenen platformların listesi için bkz. [Azure Güvenlik Merkezi 'Nde desteklenen platformlar](security-center-os-coverage.md) .


## <a name="why-doesnt-azure-security-center-recognize-the-antimalware-solution-running-on-my-azure-vm"></a>Azure Güvenlik Merkezi, Azure VM 'imde çalışan kötü amaçlı yazılımdan koruma çözümünü neden tanımıyor?

Azure Güvenlik Merkezi 'nin, Azure uzantıları aracılığıyla yüklenen kötü amaçlı yazılımdan bir görünürlüğü vardır. Örneğin, Güvenlik Merkezi, belirttiğiniz bir görüntüde önceden yüklenmiş olan kötü amaçlı yazılımdan koruma veya kendi işlemlerinizi (örneğin, yapılandırma yönetimi sistemleri) kullanarak sanal makinelerinize yükleiyor.


## <a name="why-do-i-get-the-message-missing-scan-data-for-my-vm"></a>Neden Sanal makinem için "tarama verilerini eksik" iletisini alıyorum?

Bu ileti, VM için tarama verisi olmadığında görüntülenir. Veri Toplama, Azure Güvenlik Merkezi'nde etkinleştirildikten sonra tarama verilerinin toplanması zaman (bir saatten az) alabilir. Tarama verileri ilk kez alındıktan sonra hiç tarama verisi veya güncel tarama verisi olmadığından bu iletiyle karşılaşabilirsiniz. Durdurulmuş VM'lerle ilgili değerler taramaya eklenmez. Bu ileti, tarama verileri yakın zamanda doldurulmamışsa de görünebilir (varsayılan değeri 30 gün olan Windows aracısının bekletme ilkesine uygun olarak).


## <a name="how-often-does-security-center-scan-for-operating-system-vulnerabilities-system-updates-and-endpoint-protection-issues"></a>Güvenlik Merkezi, işletim sistemi güvenlik açıkları, sistem güncelleştirmeleri ve uç nokta koruma sorunlarını ne sıklıkta tarar?

Güvenlik açıkları, güncelleştirmeler ve sorunlar için Güvenlik Merkezi taramalarının gecikme süreleri aşağıda verilmiştir:

- İşletim sistemi güvenlik yapılandırması – veriler 48 saat içinde güncelleştirilir
- Sistem güncelleştirmeleri – veriler 24 saat içinde güncelleştirilir
- Endpoint Protection sorunları – veriler 8 saat içinde güncelleştirilir

Güvenlik Merkezi genellikle her saat yeni verileri tarar ve önerileri buna göre yeniler. 

> [!NOTE]
> Güvenlik Merkezi, veri toplamak ve depolamak için Log Analytics aracısını kullanır. Daha fazla bilgi için bkz. [Azure Güvenlik Merkezi platform geçişi](security-center-platform-migration.md).


## <a name="why-do-i-get-the-message-vm-agent-is-missing"></a>"VM Aracısı eksik mı?" iletisini neden alıyorum?

Veri toplamayı etkinleştirmek için VM 'Lere sanal makine aracısının yüklü olması gerekir. VM Aracısı, Azure Marketi’nden dağıtılan VM’ler için varsayılan olarak yüklüdür. VM aracısının diğer VM 'Lere nasıl yükleneceği hakkında bilgi için bkz. Blog Post [VM Aracısı ve uzantıları](https://azure.microsoft.com/blog/vm-agent-and-extensions-part-2/).