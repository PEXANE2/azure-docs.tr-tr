---
title: Azure Hizmet Kumaşı ile sağlık durumunu bildirin ve kontrol edin
description: Azure Hizmet Dokusu'nun sağladığı sistem durumu izleme araçlarını kullanarak hizmet kodunuzdan sistem durumu raporlarını nasıl göndereceğinizi ve hizmetinizin durumunu nasıl denetleyebilirsiniz.
author: srrengar
ms.topic: conceptual
ms.date: 02/25/2019
ms.author: srrengar
ms.openlocfilehash: 2b7a9c44a84e3ce15eaec22c8f57bb48f79dae05
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75464631"
---
# <a name="report-and-check-service-health"></a>Hizmet durumunu raporlama ve denetleme
Hizmetleriniz sorunlarla karşılaştığında, olaylara ve kesintilere yanıt verme ve düzeltme yeteneğiniz, sorunları hızlı bir şekilde algılama yeteneğinize bağlıdır. Hizmet kodunuzdan Azure Hizmet Dokusu sistem durumu yöneticisine sorun ve hata bildiriyorsanız, Hizmet Dokusu'nun sistem durumunu denetlemek için sağladığı standart sistem durumu izleme araçlarını kullanabilirsiniz.

Hizmetten sağlık durumunu bildirmenin üç yolu vardır:

* [Bölüm](https://docs.microsoft.com/dotnet/api/system.fabric.istatefulservicepartition) veya [CodePackageActivationContext](https://docs.microsoft.com/dotnet/api/system.fabric.codepackageactivationcontext) nesnelerini kullanın.  
  Geçerli bağlamın `Partition` `CodePackageActivationContext` bir parçası olan öğelerin sistem durumunu bildirmek için nesneleri ve nesneleri kullanabilirsiniz. Örneğin, yinelemenin bir parçası olarak çalışan kod, yalnızca bu yinelemede, ait olduğu bölüme ve parçası olduğu uygulamada sistem durumu bildirebilir.
* `FabricClient` adresini kullanın.   
  Küme `FabricClient` [güvenli](service-fabric-cluster-security.md) değilse veya hizmet yönetici ayrıcalıklarıyla çalışıyorsa, hizmet kodundan sistem durumunu bildirmek için kullanabilirsiniz. Gerçek dünya senaryolarının çoğu güvenli olmayan kümeler kullanmaz veya yönetici ayrıcalıkları sağlamaz. `FabricClient`, kümenin bir parçası olan herhangi bir varlık üzerinde sistem durumunu bildirebilirsiniz. İdeal olarak, ancak, hizmet kodu yalnızca kendi durumuyla ilgili raporlar göndermelidir.
* KÜME, uygulama, dağıtılan uygulama, hizmet, hizmet paketi, bölümleme, çoğaltma veya düğüm düzeylerinde REST API'lerini kullanın. Bu, bir kapsayıcının içinden durumu bildirmek için kullanılabilir.

Bu makale, hizmet kodundan sağlık raporları bir örnek size yol. Örnek, Service Fabric tarafından sağlanan araçların sistem durumu durumunu denetlemek için nasıl kullanılabileceğini de gösterir. Bu makale, Hizmet Kumaşı'nın sistem durumu izleme yeteneklerine hızlı bir giriş olması amaçlanmıştır. Daha ayrıntılı bilgi için, bu makalenin sonundaki bağlantıyla başlayan sağlık la ilgili derinlemesine makale serisini okuyabilirsiniz.

## <a name="prerequisites"></a>Ön koşullar
Aşağıdakileri yüklemiş olmalısınız:

* Visual Studio 2015 veya Visual Studio 2019
* Servis Kumaş ı SDK

## <a name="to-create-a-local-secure-dev-cluster"></a>Yerel bir güvenli dev kümesi oluşturmak için
* PowerShell'i yönetici ayrıcalıklarıyla açın ve aşağıdaki komutları çalıştırın:

![Güvenli bir dev kümenin nasıl oluşturulabildiğini gösteren komutlar](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/create-secure-dev-cluster.png)

## <a name="to-deploy-an-application-and-check-its-health"></a>Bir uygulamayı dağıtmak ve sistem durumunu kontrol etmek için
1. Yönetici olarak Visual Studio'u açın.
1. **Durum Hizmeti** şablonu kullanarak bir proje oluşturun.
   
    ![Stateful Service ile Hizmet Kumaşı uygulaması oluşturma](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/create-stateful-service-application-dialog.png)
1. Uygulamayı hata ayıklama modunda çalıştırmak için **F5** tuşuna basın. Uygulama yerel kümeye dağıtılır.
1. Uygulama çalışmaya devam ettikten sonra, bildirim alanındaki Yerel Küme Yöneticisi simgesine sağ tıklayın ve Hizmet Kumaş Gezgini'ni açmak için kısayol menüsünden **Yerel Kümeyi Yönet'i** seçin.
   
    ![Bildirim alanından Hizmet Kumaş Explorer'ı aç](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/LaunchSFX.png)
1. Uygulama durumu bu resimde olduğu gibi görüntülenmelidir. Şu anda, uygulama hiçbir hata ile sağlıklı olmalıdır.
   
    ![Hizmet Kumaş Explorer sağlıklı uygulama](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/sfx-healthy-app.png)
1. PowerShell'i kullanarak da sağlığınızı kontrol edebilirsiniz. Bir uygulamanın durumunu denetlemek için kullanabilirsiniz ```Get-ServiceFabricApplicationHealth``` ve ```Get-ServiceFabricServiceHealth``` bir hizmetin durumunu denetlemek için kullanabilirsiniz. PowerShell'de aynı uygulama için sağlık raporu bu resimde.
   
    ![PowerShell'de sağlıklı uygulama](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/ps-healthy-app-report.png)

## <a name="to-add-custom-health-events-to-your-service-code"></a>Hizmet kodunuza özel sağlık olayları eklemek için
Visual Studio'daki Service Fabric proje şablonları örnek kod içerir. Aşağıdaki adımlar, servis kodunuzdan özel sağlık olaylarını nasıl bildirebileceğinizi gösterir. Bu tür raporlar, Service Fabric Explorer, Azure portal sistem durumu görünümü ve PowerShell gibi Hizmet Dokusu'nun sağladığı sistem durumu izleme için standart araçlarda otomatik olarak ortaya çıkar.

1. Visual Studio'da daha önce oluşturduğunuz uygulamayı yeniden açın veya **Stateful Service** Visual Studio şablonuna bakarak yeni bir uygulama oluşturun.
1. Stateful1.cs dosyasını açın ve `myDictionary.TryGetValueAsync` aramayı `RunAsync` yöntemde bulun. Bu uygulamadaki anahtar mantık `result` bir sayıyı çalışır durumda tutmak olduğundan, bu yöntemin sayacın geçerli değerini tutan bir yöntem döndürür olduğunu görebilirsiniz. Bu uygulama gerçek bir uygulamaysa ve sonuç eksikliği bir hata yıtemsil ettiyse, bu olayı işaretlemek isteyebilirsiniz.
1. Sonuç eksikliği bir hata temsil ettiğinde bir sistem durumu olayını bildirmek için aşağıdaki adımları ekleyin.
   
    a. `System.Fabric.Health` Stateful1.cs dosyasına ad alanını ekleyin.
   
    ```csharp
    using System.Fabric.Health;
    ```
   
    b. `myDictionary.TryGetValueAsync` Aramadan sonra aşağıdaki kodu ekleyin
   
    ```csharp
    if (!result.HasValue)
    {
        HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
        this.Partition.ReportReplicaHealth(healthInformation);
    }
    ```
    Yineleme durumunu bildiriyoruz çünkü devlet hizmetinden bildiriliyor. Parametre, `HealthInformation` bildirilen sağlık sorunu yla ilgili bilgileri depolar.
   
    Devletsiz bir hizmet oluşturduysanız, aşağıdaki kodu kullanın
   
    ```csharp
    if (!result.HasValue)
    {
        HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
        this.Partition.ReportInstanceHealth(healthInformation);
    }
    ```
1. Hizmetiniz yönetici ayrıcalıklarıyla çalışıyorsa veya küme [güvenli](service-fabric-cluster-security.md)değilse, `FabricClient` aşağıdaki adımlarda gösterildiği gibi durumu bildirmek için de kullanabilirsiniz.  
   
    a. Bildirimden `FabricClient` `var myDictionary` sonra örneği oluşturun.
   
    ```csharp
    var fabricClient = new FabricClient(new FabricClientSettings() { HealthReportSendInterval = TimeSpan.FromSeconds(0) });
    ```
   
    b. `myDictionary.TryGetValueAsync` Aramadan sonra aşağıdaki kodu ekleyin.
   
    ```csharp
    if (!result.HasValue)
    {
       var replicaHealthReport = new StatefulServiceReplicaHealthReport(
            this.Context.PartitionId,
            this.Context.ReplicaId,
            new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error));
        fabricClient.HealthManager.ReportHealth(replicaHealthReport);
    }
    ```
1. Bu başarısızlığı simüle edelim ve sistem durumu izleme araçlarında görünelim. Hatayı simüle etmek için, daha önce eklediğiniz sistem durumu raporlama kodundaki ilk satırı belirtin. İlk satırı açıklamayaptıktan sonra, kod aşağıdaki örnek gibi görünür.
   
    ```csharp
    //if(!result.HasValue)
    {
        HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
        this.Partition.ReportReplicaHealth(healthInformation);
    }
    ```
   Bu kod, her çalıştırışta `RunAsync` sağlık raporunu çalıştırıyor. Değişikliği yaptıktan sonra, uygulamayı çalıştırmak için **F5** tuşuna basın.
1. Uygulama çalıştırıladıktan sonra, uygulamanın durumunu kontrol etmek için Service Fabric Explorer'ı açın. Bu kez, Service Fabric Explorer uygulamanın sağlıksız olduğunu gösterir. Uygulama sağlıksız olarak gösterir, çünkü daha önce eklediğimiz koddan bildirilen hata.
   
    ![Hizmet Kumaş Explorer sağlıksız uygulama](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/sfx-unhealthy-app.png)
1. Hizmet Kumaş Explorer'ın ağaç görünümünde birincil yinelemeyi seçerseniz, **Sistem Durumu'nun** da bir hata gösterdiğini görürsünüz. Service Fabric Explorer, koddaki `HealthInformation` parametreye eklenen sistem durumu raporu ayrıntılarını da görüntüler. Aynı sistem durumu raporlarını PowerShell ve Azure portalında da görebilirsiniz.
   
    ![Hizmet Kumaş Explorer çoğaltma sağlık](./media/service-fabric-diagnostics-how-to-report-and-check-service-health/replica-health-error-report-sfx.png)

Bu rapor, başka bir raporla değiştirilene veya bu yineleme silinene kadar sistem durumu yöneticisinde kalır. Bu sağlık raporunu `TimeToLive` nesnede ayarlamadığımız için, raporun süresi hiç dolmaz. `HealthInformation`

Sağlık durumunun en ayrıntılı düzeyde rapor edilmesini öneririz, bu durumda yineleme dir. Ayrıca sağlık durumunu `Partition`da bildirebilirsiniz.

```csharp
HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
this.Partition.ReportPartitionHealth(healthInformation);
```

Sağlık durumunu `Application`bildirmek `DeployedApplication`için `DeployedServicePackage`, `CodePackageActivationContext`ve , .

```csharp
HealthInformation healthInformation = new HealthInformation("ServiceCode", "StateDictionary", HealthState.Error);
var activationContext = FabricRuntime.GetActivationContext();
activationContext.ReportApplicationHealth(healthInformation);
```

## <a name="next-steps"></a>Sonraki adımlar
* [Hizmet Kumaş sağlığı derin dalış](service-fabric-health-introduction.md)
* [Hizmet sağlığını raporlamak için REST API](https://docs.microsoft.com/rest/api/servicefabric/report-the-health-of-a-service)
* [Uygulama durumunu bildirmek için REST API](https://docs.microsoft.com/rest/api/servicefabric/report-the-health-of-an-application)

