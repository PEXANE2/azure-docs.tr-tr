---
title: Powershell'deki bir kümeden uygulamayı kaldırma
description: Azure PowerShell Komut Dosyası Örneği - Bir Hizmeti Kumaş kümesinden uygulamayı kaldırın.
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
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "75610293"
---
# <a name="remove-an-application-from-a-service-fabric-cluster-using-powershell"></a>Powershell kullanarak bir Hizmeti Kumaş kümesinden uygulama kaldırma

Bu örnek komut dosyası, çalışan bir Hizmet Kumaşı uygulama örneğini siler ve bir uygulama türünü ve sürümünü kümeden siler.  Uygulama örneğinin silinmesi, bu uygulamayla ilişkili çalışan tüm hizmet örneklerini de siler. Parametreleri gereken şekilde özelleştirin. 

Gerekirse, [Service Fabric SDK’sı](../service-fabric-get-started.md) ile Service Fabric PowerShell modülünü yükleyin. 

## <a name="sample-script"></a>Örnek betik

[!code-powershell[main](../../../powershell_scripts/service-fabric/remove-application/remove-application.ps1 "Remove an application from a cluster")]

## <a name="script-explanation"></a>Betik açıklaması

Bu betik aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [Remove-ServiceFabricApplication](/powershell/module/servicefabric/remove-servicefabricapplication?view=azureservicefabricps) | Çalışan Hizmet Kumaşı uygulama örneğini kümeden kaldırır.  |
| [Kayıt Dışı-HizmetKumaşUygulamaTürü](/powershell/module/servicefabric/unregister-servicefabricapplicationtype?view=azureservicefabricps) | Bir Hizmet Kumaşı uygulama türünü ve sürümünü kümeden kaydeder. |

## <a name="next-steps"></a>Sonraki adımlar

Service Fabric PowerShell modülü hakkında daha fazla bilgi için [Azure PowerShell belgelerine](/powershell/azure/service-fabric/?view=azureservicefabricps)bakın.

Azure Service Fabric için ek Powershell örneklerine [Azure PowerShell örnekleri](../service-fabric-powershell-samples.md) içinden erişilebilir.
