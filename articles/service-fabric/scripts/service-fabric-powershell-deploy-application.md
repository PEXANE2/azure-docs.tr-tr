---
title: Powershell'de bir kümeye uygulama dağıtma
description: Azure PowerShell Komut Dosyası Örneği - Bir uygulamayı Hizmet Kumaşı kümesine dağıtın.
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
ms.openlocfilehash: 207f2a4e8173aa1e5009435665532973045d9198
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "75610310"
---
# <a name="deploy-an-application-to-a-service-fabric-cluster"></a>Service Fabric kümesine uygulama dağıtma

Bu örnek komut dosyası, bir uygulama paketini küme görüntü deposuna kopyalar, uygulama türünü kümeye kaydeder, gereksiz uygulama paketini kaldırır ve uygulama türünden bir uygulama örneği oluşturur.  Hedef uygulama türünün uygulama bildiriminde varsayılan hizmetler tanımlanmışsa, bu hizmetler şu anda oluşturulur. Parametreleri gereken şekilde özelleştirin. 

Gerekirse, [Service Fabric SDK’sı](../service-fabric-get-started.md) ile Service Fabric PowerShell modülünü yükleyin. 

## <a name="sample-script"></a>Örnek betik

[!code-powershell[main](../../../powershell_scripts/service-fabric/deploy-application/deploy-application.ps1 "Deploy an application to a cluster")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme 

Komut dosyası örneği çalıştırıldıktan sonra, uygulama örneğini [kaldırmak,](service-fabric-powershell-remove-application.md) uygulama türünü kaldırmak ve uygulama paketini görüntü deposundan silmek için uygulamayı Kaldır'daki komut dosyası kullanılabilir.

## <a name="script-explanation"></a>Betik açıklaması

Bu betik aşağıdaki komutları kullanır. Tablodaki her komut, komuta özgü belgelere yönlendirir.

| Komut | Notlar |
|---|---|
|[Connect-ServiceFabricCluster](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps)| Service Fabric kümesine bağlantı oluşturur. |
|[Kopya-ServisKumaşUygulama Paketi](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) | Bir uygulama paketini küme görüntü deposuna kopyalar.  |
|[Kayıt-ServisKumaşUygulama Türü](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps)| Kümedeki bir uygulama türünü ve sürümünü kaydeder. |
|[Yeni HizmetKumaş Uygulaması](/powershell/module/servicefabric/new-servicefabricapplication?view=azureservicefabricps)| Kayıtlı bir uygulama türünden bir uygulama oluşturur. |
| [Remove-ServiceFabricApplicationPackage](/powershell/module/servicefabric/remove-servicefabricapplicationpackage?view=azureservicefabricps) | Hizmet Kumaşı uygulama paketini görüntü deposundan kaldırır.|

## <a name="next-steps"></a>Sonraki adımlar

Service Fabric PowerShell modülü hakkında daha fazla bilgi için [Azure PowerShell belgelerine](/powershell/azure/service-fabric/?view=azureservicefabricps)bakın.

Azure Service Fabric için ek Powershell örneklerine [Azure PowerShell örnekleri](../service-fabric-powershell-samples.md) içinden erişilebilir.
