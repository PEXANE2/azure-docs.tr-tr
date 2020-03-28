---
title: Powershell'de Hizmet Kumaşı uygulamasını yükseltme
description: Azure PowerShell Script Örneği - Powershell'i kullanarak bir Azure Hizmet Kumaşı uygulamasını yükseltin ve izleyin.
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
ms.openlocfilehash: 3a4ef9fad8567eb145d51c6fef61773cc3a00b11
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "75614749"
---
# <a name="upgrade-a-service-fabric-application"></a>Hizmet Kumaşı uygulamasını yükseltme

Bu örnek komut dosyası, çalışan bir Hizmet Kumaşı uygulama örneğini sürüm 1.3.0'a yükseltir. Komut dosyası, yeni uygulama paketini küme görüntü deposuna kopyalar, uygulama türünü kaydeder ve gereksiz uygulama paketini kaldırır.  Komut dosyası izlenen bir yükseltme yi başlatır ve yükseltme tamamlanana veya geri dönene kadar yükseltme durumunu sürekli olarak denetler. Parametreleri gereken şekilde özelleştirin. 

Gerekirse, [Service Fabric SDK’sı](../service-fabric-get-started.md) ile Service Fabric PowerShell modülünü yükleyin. 

## <a name="sample-script"></a>Örnek betik

[!code-powershell[main](../../../powershell_scripts/service-fabric/upgrade-application/upgrade-application.ps1 "Upgrade an application")]

## <a name="script-explanation"></a>Betik açıklaması

Bu betik aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
| [Al-ServiceFabricApplication](/powershell/module/servicefabric/get-servicefabricapplication?view=azureservicefabricps) | Service Fabric kümesindeki veya belirli bir uygulamadaki tüm uygulamaları alır.  |
| [Get-ServiceFabricApplicationYükseltme](/powershell/module/servicefabric/get-servicefabricapplicationupgrade?view=azureservicefabricps) | Service Fabric uygulama yükseltmesi durumunu alır. |
| [Get-ServiceFabricApplicationType](/powershell/module/servicefabric/get-servicefabricapplicationtype?view=azureservicefabricps) | Service Fabric uygulama türlerini Servis Kumaşı kümesine kaydettirin. |
| [Kayıt Dışı-HizmetKumaşUygulamaTürü](/powershell/module/servicefabric/unregister-servicefabricapplicationtype?view=azureservicefabricps) | Hizmet Kumaşı uygulama türünü kaydeder.  |
| [Kopya-ServisKumaşUygulama Paketi](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) | Hizmet Kumaşı uygulama paketini görüntü deposuna kopyalar.  |
| [Kayıt-ServisKumaşUygulama Türü](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) | Service Fabric uygulama türünü kaydeder. |
| [Başlangıç-ServisKumaş Uygulama Yükseltme](/powershell/module/servicefabric/start-servicefabricapplicationupgrade?view=azureservicefabricps) | Service Fabric uygulamasını belirtilen uygulama türü sürümüne yükseltir. |
| [Remove-ServiceFabricApplicationPackage](/powershell/module/servicefabric/remove-servicefabricapplicationpackage?view=azureservicefabricps) | Hizmet Kumaşı uygulama paketini görüntü deposundan kaldırır.|


## <a name="next-steps"></a>Sonraki adımlar

Service Fabric PowerShell modülü hakkında daha fazla bilgi için [Azure PowerShell belgelerine](/powershell/azure/service-fabric/?view=azureservicefabricps)bakın.

Azure Service Fabric için ek Powershell örneklerine [Azure PowerShell örnekleri](../service-fabric-powershell-samples.md) içinden erişilebilir.
