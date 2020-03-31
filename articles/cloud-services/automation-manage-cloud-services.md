---
title: Azure Otomasyonu kullanarak Azure Bulut Hizmetlerini Yönetme | Microsoft Dokümanlar
description: Azure Bulut hizmetlerini ölçekte yönetmek için Azure Otomasyon hizmetinin nasıl kullanAbileceği hakkında bilgi edinin.
services: cloud-services, automation
author: jodoglevy
manager: timlt
editor: ''
ms.assetid: 3789810a-2892-4eef-bf29-c781c1b5af48
ms.service: cloud-services
ms.topic: article
ms.date: 06/20/2016
ms.author: timlt
ms.openlocfilehash: 482fcf7d100a90d9527f510382c5dafb4f67adfa
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72439081"
---
# <a name="managing-azure-cloud-services-using-azure-automation"></a>Azure Otomasyonu kullanarak Azure Bulut Hizmetlerini Yönetme
Bu kılavuz, Azure Otomasyon hizmeti ve Azure bulut hizmetlerinizin yönetimini kolaylaştırmak için nasıl kullanılabileceğini size tanıtacaktır.

## <a name="what-is-azure-automation"></a>Azure Otomasyonu Nedir?
[Azure Otomasyon,](https://azure.microsoft.com/services/automation/) süreç otomasyonu yoluyla bulut yönetimini basitleştirmek için bir Azure hizmetidir. Azure Otomasyonu'nu kullanarak, uzun süreli, manuel, hataya açık ve sık sık tekrarlanan görevler, kuruluşunuz için güvenilirliği, verimliliği ve değeri artırma süresini artırmak için otomatikhale getirilebilir.

Azure Otomasyonu, kuruluşunuz büyüdükçe ihtiyaçlarınızı karşılayacak şekilde ölçeklendirilebilen son derece güvenilir ve yüksek kullanılabilirlikte bir iş akışı yürütme motoru sağlar. Azure Otomasyonu'nda işlemler el ile, üçüncü taraf sistemler tarafından veya zamanlanmış aralıklarla çalıştırılabilir, böylece görevler tam olarak gerektiğinde gerçekleşir.

Azure Otomasyonu tarafından otomatik olarak çalıştırılacak bulut yönetimi görevlerinizi taşıyarak iş değeri katan işlere odaklanmaları için BT / DevOps personelini daha da serbest leştirin.

## <a name="how-can-azure-automation-help-manage-azure-cloud-services"></a>Azure Otomasyonu Azure bulut hizmetlerinin yönetilmesine nasıl yardımcı olabilir?
Azure bulut hizmetleri, [Azure PowerShell araçlarında](/powershell/)bulunan PowerShell cmdlet'leri kullanılarak Azure Otomasyonu'nda yönetilebilir. Azure Automation, tüm bulut hizmeti yönetimi görevlerinizi hizmet dahilinde gerçekleştirebilmeniz için kutunun dışında kullanılabilen bulut hizmeti PowerShell cmdlet'lere sahiptir. Azure hizmetleri ve üçüncü taraf sistemlerindeki karmaşık görevleri otomatikleştirmek için bu cmdlet'leri Azure Otomasyonu'ndaki diğer Azure hizmetleri için cmdletlerle eşleştirebilirsiniz.

Azure Bulut Hizmetlerini yönetmek için Azure Otomasyonu'nun bazı kullanımları şunlardır:

* [Azure Blob depolama alanında cscfg veya cspkg güncelleştirildiğinde Bulut Hizmeti'nin sürekli dağıtımı](https://gallery.technet.microsoft.com/scriptcenter/Continuous-Deployment-of-A-eeebf3a6)
* [Bulut Hizmeti örneklerini paralel olarak yeniden başlatma, aynı anda bir yükseltme etki alanı](https://gallery.technet.microsoft.com/scriptcenter/Reboot-Cloud-Service-PaaS-b337a06d)

## <a name="next-steps"></a>Sonraki Adımlar
Azure Otomasyonunun temellerini ve Azure bulut hizmetlerini yönetmek için nasıl kullanılabileceğini öğrendiğiniz için, Azure Otomasyonu hakkında daha fazla bilgi edinmek için bu bağlantıları izleyin.

* [Azure Otomasyonuna Genel Bakış](../automation/automation-intro.md)
* [İlk runbook’um](../automation/automation-first-runbook-graphical.md)
