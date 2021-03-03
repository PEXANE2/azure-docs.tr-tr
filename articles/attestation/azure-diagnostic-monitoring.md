---
title: Azure Tanılama izleme-Azure kanıtlama
description: Azure kanıtlama için Azure Tanılama izleme
services: attestation
author: msmbaldwin
ms.service: attestation
ms.topic: overview
ms.date: 08/31/2020
ms.author: mbaldwin
ms.openlocfilehash: d01e7817906927295591353b710afe2899aacdf1
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2021
ms.locfileid: "101726487"
---
# <a name="setting-up-diagnostics-with-trusted-platform-module-tpm-endpoint-of-azure-attestation"></a>Azure kanıtlama Güvenilir Platform Modülü (TPM) uç noktası ile tanılamayı ayarlama

Azure etkinlik günlüğü ve kaynak günlükleri dahil olmak üzere Azure 'daki [Platform günlükleri](../azure-monitor/essentials/platform-logs-overview.md) , Azure kaynakları ve bağımlı oldukları Azure platformu için ayrıntılı tanılama ve denetim bilgileri sağlar. [Platform ölçümleri](../azure-monitor/essentials/data-platform-metrics.md) varsayılan olarak toplanır ve genellikle Azure izleyici ölçümleri veritabanında depolanır. Bu makalede, farklı hedeflere platform ölçümleri ve platform günlükleri göndermek için tanılama ayarlarını oluşturma ve yapılandırma hakkında ayrıntılı bilgi verilmektedir. 

TPM uç nokta hizmeti, tanılama ayarıyla etkinleştirildi ve etkinliği izlemek için kullanılabilir. PowerShell 'i kullanarak TPM hizmeti uç noktası için [Azure izlemeyi](../azure-monitor/overview.md) ayarlamak için aşağıdaki adımları izleyin. 

Azure kanıtlama hizmetini kurun. 

[Azure kanıtlama 'nı Azure PowerShell ile ayarlama](./quickstart-powershell.md)

```powershell

 Connect-AzAccount 

 Set-AzContext -Subscription <Subscription id> 

 $attestationProviderName=<Name of the attestation provider> 

 $attestationResourceGroup=<Name of the resource Group> 

 $attestationProvider=Get-AzAttestation -Name $attestationProviderName -ResourceGroupName $attestationResourceGroup 

 $storageAccount=New-AzStorageAccount -ResourceGroupName $attestationProvider.ResourceGroupName -Name <Storage Account Name> -SkuName Standard_LRS -Location <Location> 

 Set-AzDiagnosticSetting -ResourceId $ attestationProvider.Id -StorageAccountId $ storageAccount.Id -Enabled $true 

```
Etkinlik günlükleri, depolama hesabının kapsayıcılar bölümünde bulunabilir. Ayrıntılı bilgi, [bir Azure kaynağından kaynak günlüklerini toplama ve Azure izleyici ile analiz etme-Azure izleyici ile](../azure-monitor/essentials/tutorial-resource-logs.md) bulunabilir.
