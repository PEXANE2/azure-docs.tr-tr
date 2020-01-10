---
title: PowerShell 'deki bir kümeden uygulamayı kaldırma
description: Azure PowerShell betiği örneği-bir uygulamayı Service Fabric kümeden kaldırın.
services: service-fabric
documentationcenter: ''
author: athinanthny
manager: chackdan
editor: ''
tags: azure-service-management
ms.assetid: ''
ms.service: service-fabric
ms.workload: multiple
ms.topic: sample
ms.date: 01/18/2018
ms.author: atsenthi
ms.custom: mvc
ms.openlocfilehash: 9ac4c23468c74b5a2c6874de2cb6b8d0e6b9e7dd
ms.sourcegitcommit: 003e73f8eea1e3e9df248d55c65348779c79b1d6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/02/2020
ms.locfileid: "75610293"
---
# <a name="remove-an-application-from-a-service-fabric-cluster-using-powershell"></a>PowerShell kullanarak bir uygulamayı Service Fabric kümeden kaldırma

Bu örnek betik, çalışan bir Service Fabric uygulama örneğini siler ve kümeden bir uygulama türü ve sürümü siler.  Uygulama örneğinin silinmesi, bu uygulamayla ilişkili çalışan tüm hizmet örneklerini de siler. Parametreleri gereken şekilde özelleştirin. 

Gerekirse, [Service Fabric SDK’sı](../service-fabric-get-started.md) ile Service Fabric PowerShell modülünü yükleyin. 

## <a name="sample-script"></a>Örnek betik

[!code-powershell[main](../../../powershell_scripts/service-fabric/remove-application/remove-application.ps1 "Remove an application from a cluster")]

## <a name="script-explanation"></a>Betik açıklaması

Bu betik aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [Remove-ServiceFabricApplication](/powershell/module/servicefabric/remove-servicefabricapplication?view=azureservicefabricps) | Çalışan bir Service Fabric uygulama örneğini kümeden kaldırır.  |
| [Unregister-ServiceFabricApplicationType](/powershell/module/servicefabric/unregister-servicefabricapplicationtype?view=azureservicefabricps) | Kümeden bir Service Fabric uygulama türü ve sürümü kaydını siler. |

## <a name="next-steps"></a>Sonraki adımlar

Service Fabric PowerShell modülü hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/service-fabric/?view=azureservicefabricps).

Azure Service Fabric için ek Powershell örneklerine [Azure PowerShell örnekleri](../service-fabric-powershell-samples.md) içinden erişilebilir.
