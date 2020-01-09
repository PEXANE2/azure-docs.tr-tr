---
title: Yerel Azure Service Fabric kümesi kurulumlarınızın sorunlarını giderme
description: Bu makalede, yerel geliştirme kümenizin sorunlarını gidermeye yönelik bir dizi öneri ele alınmaktadır
author: mikkelhegn
ms.topic: conceptual
ms.date: 02/23/2018
ms.author: mikhegn
ms.openlocfilehash: ea313adb43f8d91ec9e57dd1d0b8d3447a8075f2
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75465512"
---
# <a name="troubleshoot-your-local-development-cluster-setup"></a>Yerel geliştirme kümesi kurulumlarınızın sorunlarını giderme
Yerel Azure Service Fabric geliştirme kümeniz ile etkileşim kurarken bir sorunla karşılaşırsanız, olası çözümler için aşağıdaki önerileri gözden geçirin.

## <a name="cluster-setup-failures"></a>Küme kurulum sorunları
### <a name="cannot-clean-up-service-fabric-logs"></a>Service Fabric günlükleri temizlenemiyor
#### <a name="problem"></a>Sorun
DevClusterSetup betiğini çalıştırırken şu hatayı görürsünüz:

    Cannot clean up C:\SfDevCluster\Log fully as references are likely being held to items in it. Please remove those and run this script again.
    At line:1 char:1 + .\DevClusterSetup.ps1
    + ~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo : NotSpecified: (:) [Write-Error], WriteErrorException
    + FullyQualifiedErrorId : Microsoft.PowerShell.Commands.WriteErrorException,DevClusterSetup.ps1


#### <a name="solution"></a>Çözüm
Geçerli PowerShell penceresini kapatın ve yönetici olarak yeni bir PowerShell penceresi açın. Artık betiği başarıyla çalıştırabilirsiniz.

## <a name="cluster-connection-failures"></a>Küme bağlantısı sorunları

### <a name="type-initialization-exception"></a>Tür başlatma özel durumu
#### <a name="problem"></a>Sorun
PowerShell 'de kümeye bağlanırken, System. Fabric. Common. AppTrace için hata TypeInitializationException görürsünüz.

#### <a name="solution"></a>Çözüm
Yol değişkeniniz yükleme sırasında doğru şekilde ayarlanmadı. Windows oturumunu kapatın ve yeniden oturum açın. Bu, yolunuza yenilenir.

### <a name="cluster-connection-fails-with-object-is-closed"></a>Küme bağlantısı "nesne kapalı" ile başarısız oluyor
#### <a name="problem"></a>Sorun
Connect-ServiceFabricCluster çağrısı aşağıdakine benzer bir hata ile başarısız olur:

    Connect-ServiceFabricCluster : The object is closed.
    At line:1 char:1
    + Connect-ServiceFabricCluster
    + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo : InvalidOperation: (:) [Connect-ServiceFabricCluster], FabricObjectClosedException
    + FullyQualifiedErrorId : CreateClusterConnectionErrorId,Microsoft.ServiceFabric.Powershell.ConnectCluster

#### <a name="solution"></a>Çözüm
Geçerli PowerShell penceresini kapatın ve yönetici olarak yeni bir PowerShell penceresi açın.

### <a name="fabric-connection-denied-exception"></a>Fabric bağlantısı reddedildi özel durumu
#### <a name="problem"></a>Sorun
Visual Studio 'da hata ayıklarken, bir FabricConnectionDeniedException hatası alırsınız.

#### <a name="solution"></a>Çözüm
Bu hata genellikle bir hizmet ana bilgisayarı işlemini el ile başlatmaya çalıştığınızda oluşur.

Çözümünüzde başlangıç projeleri olarak ayarlanmış bir hizmet projesi olmadığından emin olun. Yalnızca Service Fabric uygulama projelerinin başlangıç projeleri olarak ayarlanması gerekir.

> [!TIP]
> Kurulum sonrasında, yerel kümeniz anormal davranmaya başlarsa, Yerel Küme Yöneticisi sistem tepsisi uygulamasını kullanarak sıfırlayabilirsiniz. Bu, var olan kümeyi kaldırır ve yenisini ayarlar. Dağıtılan tüm uygulamaların ve ilişkili verilerin kaldırıldığını unutmayın.
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
* [Sistem durumu raporları ile kümenizi anlayın ve sorun giderin](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)
* [Service Fabric Explorer ile kümenizi görselleştirme](service-fabric-visualizing-your-cluster.md)

