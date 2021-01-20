---
title: Azure Tanılama izleme-Azure kanıtlama
description: Azure kanıtlama için Azure Tanılama izleme
services: attestation
author: msmbaldwin
ms.service: attestation
ms.topic: overview
ms.date: 08/31/2020
ms.author: mbaldwin
ms.openlocfilehash: 40bc76f839cf6757b8f874112504249e611c3e1a
ms.sourcegitcommit: fc401c220eaa40f6b3c8344db84b801aa9ff7185
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/20/2021
ms.locfileid: "98606154"
---
# <a name="setting-up-diagnostics-with-trusted-platform-module-tpm-endpoint-of-azure-attestation"></a>Azure kanıtlama Güvenilir Platform Modülü (TPM) uç noktası ile tanılamayı ayarlama

Azure etkinlik günlüğü ve kaynak günlükleri dahil olmak üzere Azure 'daki [Platform günlükleri](/azure/azure-monitor/platform/platform-logs-overview) , Azure kaynakları ve bağımlı oldukları Azure platformu için ayrıntılı tanılama ve denetim bilgileri sağlar. [Platform ölçümleri](/azure/azure-monitor/platform/data-platform-metrics) varsayılan olarak toplanır ve genellikle Azure izleyici ölçümleri veritabanında depolanır. Bu makalede, farklı hedeflere platform ölçümleri ve platform günlükleri göndermek için tanılama ayarlarını oluşturma ve yapılandırma hakkında ayrıntılı bilgi verilmektedir. 

TPM uç nokta hizmeti, tanılama ayarıyla etkinleştirildi ve etkinliği izlemek için kullanılabilir. PowerShell 'i kullanarak TPM hizmeti uç noktası için [Azure izlemeyi](/azure/azure-monitor/overview) ayarlamak için aşağıdaki adımları izleyin. 

Azure kanıtlama hizmetini kurun. 

[Azure kanıtlama 'nı Azure PowerShell ile ayarlama](/azure/attestation/quickstart-powershell#:~:text=%20Quickstart%3A%20Set%20up%20Azure%20Attestation%20with%20Azure,Register%20Microsoft.Attestation%20resource%20provider.%20Register%20the...%20More%20)

```powershell

 Connect-AzAccount 

 Set-AzContext -Subscription <Subscription id> 

 $attestationProviderName=<Name of the attestation provider> 

 $attestationResourceGroup=<Name of the resource Group> 

 $attestationProvider=Get-AzAttestation -Name $attestationProviderName -ResourceGroupName $attestationResourceGroup 

 $storageAccount=New-AzStorageAccount -ResourceGroupName $attestationProvider.ResourceGroupName -Name <Storage Account Name> -SkuName Standard_LRS -Location <Location> 

 Set-AzDiagnosticSetting -ResourceId $ attestationProvider.Id -StorageAccountId $ storageAccount.Id -Enabled $true 

```
Etkinlik günlükleri, depolama hesabının kapsayıcılar bölümünde bulunabilir. Ayrıntılı bilgi, [bir Azure kaynağından kaynak günlüklerini toplama ve Azure izleyici ile analiz etme-Azure izleyici ile](/azure/azure-monitor/learn/tutorial-resource-logs) bulunabilir.
