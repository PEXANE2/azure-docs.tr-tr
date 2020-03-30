---
title: Azure Otomasyonu'ni kullanarak Azure API Yönetimini yönetme
description: Azure Otomasyon hizmetinin Azure API Yönetimini yönetmek için nasıl kullanılabileceğini öğrenin.
services: api-management, automation
documentationcenter: ''
author: vladvino
manager: eamono
editor: ''
ms.assetid: 2e53c9af-f738-47f8-b1b6-593050a7c51b
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 02/13/2018
ms.author: apimpm
ms.openlocfilehash: a472e00f9ecab8a5ffa6b19e4fe9a5f8b5ee5b95
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "70072053"
---
# <a name="managing-azure-api-management-using-azure-automation"></a>Azure API Management'ı Azure Otomasyonu ile Yönetme
Bu kılavuz, azure otomasyon hizmeti ve Azure API Yönetimi yönetimini kolaylaştırmak için nasıl kullanılabileceğini tanıtıyor.

## <a name="what-is-azure-automation"></a>Azure Otomasyonu Nedir?
[Azure Otomasyon,](https://azure.microsoft.com/services/automation/) süreç otomasyonu yoluyla bulut yönetimini basitleştirmek için bir Azure hizmetidir. Azure Otomasyonu'nu kullanarak, el ile, tekrarlanan, uzun süren ve hataya açık görevler, kuruluşunuz için güvenilirliği, verimliliği ve değeri artırma süresini artırmak için otomatikleştirilmiş olabilir.

Azure Otomasyonu, ihtiyaçlarınızı karşılayacak şekilde ölçeklendirilebilen son derece güvenilir ve yüksek kullanılabilirlikte bir iş akışı yürütme motoru sağlar. Azure Otomasyonu'nda işlemler el ile, üçüncü taraf sistemler tarafından veya zamanlanmış aralıklarla çalıştırılabilir, böylece görevler tam olarak gerektiğinde gerçekleşir.

Azure Otomasyonu tarafından otomatik olarak çalıştırılacak bulut yönetimi görevlerinizi taşıyarak iş değeri katan işlere odaklanmaları için operasyonel ek yükü azaltın ve BT ve DevOps personelini serbest leştirin.

## <a name="how-can-azure-automation-help-manage-azure-api-management"></a>Azure Otomasyonu Azure API Yönetimi'nin yönetilmesine nasıl yardımcı olabilir?
API Yönetimi, [Azure API Yönetimi API'si için Windows PowerShell cmdlets](https://docs.microsoft.com/powershell/module/az.apimanagement)kullanılarak Azure Otomasyonu'nda yönetilebilir. Azure Otomasyonu'nda, cmdlets'i kullanarak API Yönetimi görevlerinizi gerçekleştirmek için PowerShell iş akışı komut dosyaları yazabilirsiniz. Azure hizmetleri ve üçüncü taraf sistemlerindeki karmaşık görevleri otomatikleştirmek için bu cmdlet'leri Azure Otomasyonu'ndaki diğer Azure hizmetleri için cmdletlerle eşleştirebilirsiniz.

Powershell ile API Yönetimi'ni kullanmanın bazı örnekleri şunlardır:

* [API Management için Azure PowerShell örnekleri](https://docs.microsoft.com/azure/api-management/powershell-samples)

## <a name="next-steps"></a>Sonraki Adımlar
Azure Otomasyonunun temellerini ve Azure API Yönetimini yönetmek için nasıl kullanılabileceğini öğrendiğiniz için, daha fazla bilgi edinmek için bu bağlantıları izleyin.

* [Azure Otomasyonu'nun öğreticiye başlamasına](../automation/automation-first-runbook-graphical.md)bakın.

