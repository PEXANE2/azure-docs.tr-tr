---
title: Yerel Azure Hizmet Kumaşı küme kurulumunuzu sorun giderme
description: Bu makale, yerel geliştirme kümenizi sorun giderme için bir dizi öneriyi kapsar
author: mikkelhegn
ms.topic: conceptual
ms.date: 02/23/2018
ms.author: mikhegn
ms.openlocfilehash: ea313adb43f8d91ec9e57dd1d0b8d3447a8075f2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75465512"
---
# <a name="troubleshoot-your-local-development-cluster-setup"></a>Yerel geliştirme kümesi kurulumunuzda sorun giderme
Yerel Azure Hizmet Kumaşı geliştirme kümenizle etkileşimde bulunurken bir sorunla karşılaştıysanız, olası çözümler için aşağıdaki önerileri gözden geçirin.

## <a name="cluster-setup-failures"></a>Küme kurulum hataları
### <a name="cannot-clean-up-service-fabric-logs"></a>Servis Kumaş günlükleri temizleyemez
#### <a name="problem"></a>Sorun
DevClusterSetup komut dosyasını çalıştırırken aşağıdaki hatayı görürsünüz:

    Cannot clean up C:\SfDevCluster\Log fully as references are likely being held to items in it. Please remove those and run this script again.
    At line:1 char:1 + .\DevClusterSetup.ps1
    + ~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo : NotSpecified: (:) [Write-Error], WriteErrorException
    + FullyQualifiedErrorId : Microsoft.PowerShell.Commands.WriteErrorException,DevClusterSetup.ps1


#### <a name="solution"></a>Çözüm
Geçerli PowerShell penceresini kapatın ve yönetici olarak yeni bir PowerShell penceresi açın. Artık komut dosyasını başarıyla çalıştırabilirsiniz.

## <a name="cluster-connection-failures"></a>Küme bağlantısı hataları

### <a name="type-initialization-exception"></a>Tür Başlatma özel durumu
#### <a name="problem"></a>Sorun
PowerShell'de kümeye bağlanırken System.Fabric.Common.AppTrace için TypeInitializationException hatasını görürsünüz.

#### <a name="solution"></a>Çözüm
Yol değişkeniniz yükleme sırasında doğru ayarlanmadı. Windows'dan çıkış ve tekrar oturum açın. Bu yolunuzu yeniler.

### <a name="cluster-connection-fails-with-object-is-closed"></a>Küme bağlantısı "Nesne kapalı" ile başarısız olur
#### <a name="problem"></a>Sorun
Connect-ServiceFabricCluster için bir çağrı şu gibi bir hata ile başarısız olur:

    Connect-ServiceFabricCluster : The object is closed.
    At line:1 char:1
    + Connect-ServiceFabricCluster
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo : InvalidOperation: (:) [Connect-ServiceFabricCluster], FabricObjectClosedException
    + FullyQualifiedErrorId : CreateClusterConnectionErrorId,Microsoft.ServiceFabric.Powershell.ConnectCluster

#### <a name="solution"></a>Çözüm
Geçerli PowerShell penceresini kapatın ve yönetici olarak yeni bir PowerShell penceresi açın.

### <a name="fabric-connection-denied-exception"></a>Kumaş Bağlantısı Reddedildi özel durum
#### <a name="problem"></a>Sorun
Visual Studio'dan hata ayıklarken FabricConnectionDeniedException hatası alırsınız.

#### <a name="solution"></a>Çözüm
Bu hata genellikle bir hizmet ana bilgisayar işlemini el ile başlatmaya çalıştığınızda oluşur.

Çözümünüzde başlangıç projesi olarak ayarlanmış herhangi bir hizmet projeniz olmadığından emin olun. Yalnızca Service Fabric uygulama projeleri başlangıç projeleri olarak ayarlanmalıdır.

> [!TIP]
> Kurulumu takiben, yerel kümeniz anormal şekilde çalışmaya başlarsa, yerel küme yöneticisi sistem tepsisi uygulamasını kullanarak bunu sıfırlayabilirsiniz. Bu, varolan kümeyi kaldırır ve yeni bir küme ayarlar. Dağıtılan tüm uygulamaların ve ilişkili verilerin kaldırıldığını unutmayın.
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
* [Sistem sistem durumu raporlarıyla kümenizi anlama ve sorun giderme](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)
* [Service Fabric Explorer ile kümenizi görselleştirme](service-fabric-visualizing-your-cluster.md)

