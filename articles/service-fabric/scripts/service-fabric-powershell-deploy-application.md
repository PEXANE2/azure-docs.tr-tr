---
title: PowerShell 'de uygulamayı bir kümeye dağıtma
description: Azure PowerShell betik örneği-bir uygulamayı Service Fabric kümesine dağıtın.
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
ms.openlocfilehash: 145372fa872c481ec1a7c3de016c35fdc0f9d960
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87083812"
---
# <a name="deploy-an-application-to-a-service-fabric-cluster"></a>Service Fabric kümesine uygulama dağıtma

Bu örnek betik bir uygulama paketini bir küme görüntü deposuna kopyalar, uygulama türünü kümeye kaydeder, gereksiz uygulama paketini kaldırır ve uygulama türünden bir uygulama örneği oluşturur.  Hedef uygulama türünün uygulama bildiriminde herhangi bir varsayılan hizmet tanımlanmışsa, bu hizmetler şu anda oluşturulur. Parametreleri gereken şekilde özelleştirin. 

Gerekirse, [Service Fabric SDK’sı](../service-fabric-get-started.md) ile Service Fabric PowerShell modülünü yükleyin. 

## <a name="sample-script"></a>Örnek betik

[!code-powershell[main](../../../powershell_scripts/service-fabric/deploy-application/deploy-application.ps1 "Deploy an application to a cluster")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme 

Betik örneği çalıştırıldıktan sonra, uygulamayı [Kaldır](service-fabric-powershell-remove-application.md) içindeki betik uygulama örneğini kaldırmak, uygulama türünün kaydını silmek ve uygulama paketini görüntü deposundan silmek için kullanılabilir.

## <a name="script-explanation"></a>Betik açıklaması

Bu betik aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
|[Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps)| Service Fabric kümesine bir bağlantı oluşturur. |
|[Copy-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) | Bir uygulama paketini küme görüntü deposuna kopyalar.  |
|[Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps)| Kümeye bir uygulama türü ve sürümü kaydeder. |
|[New-ServiceFabricApplication](/powershell/module/servicefabric/new-servicefabricapplication?view=azureservicefabricps)| Kayıtlı uygulama türünden bir uygulama oluşturur. |
| [Remove-ServiceFabricApplicationPackage](/powershell/module/servicefabric/remove-servicefabricapplicationpackage?view=azureservicefabricps) | Service Fabric uygulama paketini görüntü deposundan kaldırır.|

## <a name="next-steps"></a>Sonraki adımlar

Service Fabric PowerShell modülü hakkında daha fazla bilgi için bkz. [Azure PowerShell belgeleri](/powershell/azure/service-fabric/overview?view=azureservicefabricps).

Azure Service Fabric için ek Powershell örneklerine [Azure PowerShell örnekleri](../service-fabric-powershell-samples.md) içinden erişilebilir.
