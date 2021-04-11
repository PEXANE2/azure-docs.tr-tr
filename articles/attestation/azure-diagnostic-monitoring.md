---
title: Azure kanıtlama için Azure Tanılama izleme
description: Azure kanıtlama için Azure Tanılama izleme
services: attestation
author: msmbaldwin
ms.service: attestation
ms.topic: overview
ms.date: 08/31/2020
ms.author: mbaldwin
ms.openlocfilehash: d2773be4bc67e125c18d5d38c951685e4f4fceaf
ms.sourcegitcommit: d23602c57d797fb89a470288fcf94c63546b1314
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2021
ms.locfileid: "106168357"
---
# <a name="set-up-diagnostics-with-a-trusted-platform-module-tpm-endpoint-of-azure-attestation"></a>Azure kanıtlama 'nın Güvenilir Platform Modülü (TPM) uç noktası ile tanılamayı ayarlama

Bu makale, farklı hedeflere platform ölçümleri ve platform günlükleri göndermek için Tanılama ayarları oluşturmanıza ve yapılandırmanıza yardımcı olur. Azure etkinlik günlüğü ve kaynak günlükleri dahil olmak üzere Azure 'daki [Platform günlükleri](/azure/azure-monitor/platform/platform-logs-overview) , Azure kaynakları ve bağımlı oldukları Azure platformu için ayrıntılı tanılama ve denetim bilgileri sağlar. [Platform ölçümleri](/azure/azure-monitor/platform/data-platform-metrics) varsayılan olarak toplanır ve Azure izleyici ölçümleri veritabanında depolanır.

Başlamadan önce [Azure PowerShell Ile Azure kanıtlama ayarladığınızdan](quickstart-powershell.md)emin olun.

Güvenilir Platform Modülü (TPM) uç nokta hizmeti Tanılama ayarlarından etkinleştirilir ve etkinliği izlemek için kullanılabilir. Aşağıdaki kodu kullanarak TPM hizmeti uç noktası için [Azure izleme](/azure/azure-monitor/overview) 'yi ayarlayın.

```powershell

 Connect-AzAccount 

 Set-AzContext -Subscription <Subscription id> 

 $attestationProviderName=<Name of the attestation provider> 

 $attestationResourceGroup=<Name of the resource Group> 

 $attestationProvider=Get-AzAttestation -Name $attestationProviderName -ResourceGroupName $attestationResourceGroup 

 $storageAccount=New-AzStorageAccount -ResourceGroupName $attestationProvider.ResourceGroupName -Name <Storage Account Name> -SkuName Standard_LRS -Location <Location> 

 Set-AzDiagnosticSetting -ResourceId $ attestationProvider.Id -StorageAccountId $ storageAccount.Id -Enabled $true 

```

Etkinlik günlükleri, depolama hesabının **kapsayıcılar** bölümünde bulunur. Daha fazla bilgi için bkz. [Azure kaynağından kaynak günlüklerini toplama ve çözümleme](/azure/azure-monitor/learn/tutorial-resource-logs).
