---
title: FabricClient ile Azure Servis Kumaşı dağıtımı
description: Service Fabric'deki uygulamaları dağıtmak ve kaldırmak için FabricClient API'lerini kullanın.
ms.topic: conceptual
ms.date: 01/19/2018
ms.openlocfilehash: 25b874d1be8ab50d8076ff8fe9423c8cc0187512
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75376979"
---
# <a name="deploy-and-remove-applications-using-fabricclient"></a>FabricClient kullanarak uygulamaları dağıtma ve kaldırma
> [!div class="op_single_selector"]
> * [Kaynak Yöneticisi](service-fabric-application-arm-resource.md)
> * [Powershell](service-fabric-deploy-remove-applications.md)
> * [Service Fabric CLI](service-fabric-application-lifecycle-sfctl.md)
> * [FabricClient API’leri](service-fabric-deploy-remove-applications-fabricclient.md)
> 
> 

<br/>

Bir [uygulama türü paketlendikten][10]sonra, azure hizmet kumaşı kümesine dağıtıma hazırdır. Dağıtım aşağıdaki üç adımı içerir:

1. Uygulama paketini resim mağazasına yükleme
2. Başvuru türünü kaydedin
3. Uygulama paketini görüntü deposundan kaldırma
4. Uygulama örneğini oluşturma

Bir uygulamayı dağıttıktan ve kümede bir örnek çalıştırdıktan sonra, uygulama örneğini ve uygulama türünü silebilirsiniz. Aşağıdaki adımları izleyerek bir uygulamayı kümeden tamamen kaldırın:

1. Çalışan uygulama örneğini kaldırma (veya silme)
2. Artık ihtiyacınız yoksa uygulama türünü kayıt dışı

Visual Studio'yu yerel geliştirme kümenizde uygulamaları dağıtmak ve hata ayıklamak için kullanıyorsanız, önceki tüm adımlar bir PowerShell komut dosyası aracılığıyla otomatik olarak işlenir.  Bu komut dosyası, uygulama projesinin *Komut Dosyaları* klasöründe bulunur. Bu makalede, Visual Studio dışında aynı işlemleri yapabilmeniz için bu komut dosyasının ne yaptığına ilişkin arka plan sağlandığı nızı sağlar. 
 
## <a name="connect-to-the-cluster"></a>Kümeye bağlanma
Bu makaledeki kod örneklerinden herhangi birini çalıştırmadan önce bir [FabricClient](/dotnet/api/system.fabric.fabricclient) örneği oluşturarak kümeye bağlanın. Azure Etkin Dizini, X509 sertifikaları veya Windows Etkin Dizin kullanılarak güvenli bir yerel geliştirme kümesine veya uzak bir kümeye veya [kümeye](service-fabric-connect-to-secure-cluster.md#connect-to-a-cluster-using-the-fabricclient-apis)bağlanma örnekleri için bkz. Yerel geliştirme kümesine bağlanmak için aşağıdaki örneği çalıştırın:

```csharp
// Connect to the local cluster.
FabricClient fabricClient = new FabricClient();
```

## <a name="upload-the-application-package"></a>Uygulama paketini yükleyin
Visual Studio'da *MyApplication* adında bir uygulama oluşturduğunuzu ve paketlediğinizi varsayalım. Varsayılan olarak, ApplicationManifest.xml listelenen uygulama türü adı "MyApplicationType" dır.  Gerekli başvuru bildirimini, hizmet bildirimlerini ve kod/config/veri paketlerini içeren uygulama paketi *C:\Users\&&gt;lt;kullanıcı adı \Belgeler\Visual Studio 2019\Projeler\MyApplication\MyApplication\pkg\Hata Ayıklama*' nda yer almaktadır.

Uygulama paketinin yüklenmesi, paketi dahili Service Fabric bileşenleri tarafından erişilebilen bir konuma koyar. Service Fabric, başvuru paketinin kaydı sırasında başvuru paketini doğrular. Ancak, başvuru paketini yerel olarak doğrulamak istiyorsanız (yani yüklemeden önce), [Test-ServiceFabricApplicationPackage](/powershell/module/servicefabric/test-servicefabricapplicationpackage?view=azureservicefabricps) cmdlet'i kullanın.

[CopyApplicationPackage](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.copyapplicationpackage) API, uygulama paketini küme görüntü deposuna yükler. 

Uygulama paketi büyükse ve/veya çok sayıda dosyavarsa, [sıkıştırabilir](service-fabric-package-apps.md#compress-a-package) ve PowerShell'i kullanarak görüntü deposuna kopyalayabilirsiniz. Sıkıştırma, dosyaların boyutunu ve sayısını azaltır.

Bkz. Görüntü deposu ve görüntü deposu bağlantı dizesi hakkında ek bilgiler için [görüntü deposu bağlantı dizesini anlayın.](service-fabric-image-store-connection-string.md)

## <a name="register-the-application-package"></a>Başvuru paketini kaydedin
Uygulama bildiriminde beyan edilen uygulama türü ve sürümü, başvuru paketi kaydedildiğinde kullanıma hazır hale gelir. Sistem, önceki adımda yüklenen paketi okur, paketi doğrular, paket içeriğini işler ve işlenen paketi dahili bir sistem konumuna kopyalar.  

[ProvisionApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync) API, uygulama türünü kümeye kaydeder ve dağıtım için kullanılabilir hale getirmektedir.

[GetApplicationTypeListAsync](/dotnet/api/system.fabric.fabricclient.queryclient.getapplicationtypelistasync) API tüm başarıyla kayıtlı uygulama türleri hakkında bilgi sağlar. Bu API'yi, kaydın ne zaman yapılacağını belirlemek için kullanabilirsiniz.

## <a name="remove-an-application-package-from-the-image-store"></a>Uygulama paketini görüntü deposundan kaldırma
Uygulama başarıyla kaydedildikten sonra uygulama paketini kaldırmanız önerilir.  Uygulama paketlerini görüntü deposundan silerse sistem kaynaklarını boşaltAr.  Kullanılmayan uygulama paketlerinin tutulması disk depolamayı tüketir ve uygulama performansı sorunlarına yol açar. Uygulama Paketini [Kaldır](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.removeapplicationpackage) API'sini kullanarak uygulama paketini görüntü deposundan silin.

## <a name="create-an-application-instance"></a>Uygulama örneği oluşturma
[CreateApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.createapplicationasync) API'sini kullanarak başarıyla kaydedilmiş herhangi bir uygulama türünden bir uygulamayı anında alabilirsiniz. Her uygulamanın adı *"kumaş:"* şemasıyla başlamalı ve her uygulama örneği için (küme içinde) benzersiz olmalıdır. Hedef uygulama türünün uygulama bildiriminde tanımlanan varsayılan hizmetler de oluşturulur.

Kayıtlı bir uygulama türünün belirli bir sürümü için birden çok uygulama örneği oluşturulabilir. Her uygulama örneği, kendi çalışma dizini ve işlem kümesiyle, yalıtımiçinde çalışır.

Kümede hangi adlandırılmış uygulamaların ve hizmetlerin çalıştığını görmek için [GetApplicationListAsync](/dotnet/api/system.fabric.fabricclient.queryclient.getapplicationlistasync) ve [GetServiceListAsync](/dotnet/api/system.fabric.fabricclient.queryclient.getservicelistasync) API'lerini çalıştırın.

## <a name="create-a-service-instance"></a>Hizmet örneği oluşturma
[CreateServiceAsync](/dotnet/api/system.fabric.fabricclient.servicemanagementclient.createserviceasync) API'sini kullanarak bir hizmet türünden bir hizmeti anında alabilirsiniz.  Hizmet uygulama bildiriminde varsayılan hizmet olarak bildirilirse, uygulama anında yapıldığında hizmet anında gerçekleşir.  [CreateServiceAsync](/dotnet/api/system.fabric.fabricclient.servicemanagementclient.createserviceasync) API'yi zaten anında alınmış bir hizmet için çağırmak FabricException türünden bir özel durum döndürür. Özel durum FabricErrorCode.ServiceAlreadyExists değeri olan bir hata kodu içerir.

## <a name="remove-a-service-instance"></a>Hizmet örneğini kaldırma
Bir hizmet örneğine artık gerek kalmadığında, [DeleteServiceAsync](/dotnet/api/system.fabric.fabricclient.servicemanagementclient.deleteserviceasync) API'yi arayarak onu çalışan uygulama örneğinden kaldırabilirsiniz.  

> [!WARNING]
> Bu işlem geri alınamaz ve hizmet durumu kurtarılamaz.

## <a name="remove-an-application-instance"></a>Uygulama örneğini kaldırma
Bir uygulama örneğine artık gerek olmadığında, [DeleteApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.deleteapplicationasync) API'sını kullanarak uygulamayı kalıcı olarak adıyla kaldırabilirsiniz. [DeleteApplicationAsync,](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.deleteapplicationasync) uygulamaya ait tüm hizmetleri de otomatik olarak kaldırır ve tüm hizmet durumunu kalıcı olarak kaldırır.

> [!WARNING]
> Bu işlem geri alınamaz ve uygulama durumu kurtarılamaz.

## <a name="unregister-an-application-type"></a>Uygulama türünü kayıt dışı
Bir uygulama türünün belirli bir sürümüne artık gerek olmadığında, [Kayıt Dışı-HizmetKumaşUygulama Türü](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.unprovisionapplicationasync) API'sini kullanarak uygulama türünün belirli bir sürümünün kaydını silmeniz gerekir. Uygulama türlerinin kullanılmayan sürümlerinin kaydedilemi, görüntü deposu tarafından kullanılan depolama alanı bırakır. Uygulama türünün bu sürümüne karşı hiçbir uygulama anında yapılmadığı sürece, uygulama türünün bir sürümü kayıt dışı olabilir. Ayrıca, uygulama türü bekleyen uygulama yükseltmeleri olabilir uygulama türü bu sürümü başvuruyor.

## <a name="troubleshooting"></a>Sorun giderme
### <a name="copy-servicefabricapplicationpackage-asks-for-an-imagestoreconnectionstring"></a>Copy-ServiceFabricApplicationPackage bir ImageStoreConnectionString ister
Hizmet Kumaşı SDK ortamı zaten doğru varsayılanları ayarlamış olmalıdır. Ancak gerekirse, tüm komutlar için ImageStoreConnectionString, Service Fabric kümesinin kullandığı değerle eşleşmelidir. ImageStoreConnectionString'i küme manifestosunda, [Get-ServiceFabricClusterManifest](/powershell/module/servicefabric/get-servicefabricclustermanifest?view=azureservicefabricps) ve Get-ImageStoreStringFromClusterManifest komutları kullanılarak alınan aşağıdakileri bulabilirsiniz:

```powershell
PS C:\> Get-ImageStoreConnectionStringFromClusterManifest(Get-ServiceFabricClusterManifest)
```

Service Fabric SDK PowerShell modülünün bir parçası olan **Get-ImageStoreConnectionStringFromClusterManifest** cmdlet, görüntü deposu bağlantı dizesini almak için kullanılır.  SDK modüllerini almak için çalıştırın:

```powershell
Import-Module "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\Tools\PSModule\ServiceFabricSDK\ServiceFabricSDK.psm1"
```


ImageStoreConnectionString küme manifestosunda bulunur:

```xml
<ClusterManifest xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" Name="Server-Default-SingleNode" Version="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">

    [...]

    <Section Name="Management">
      <Parameter Name="ImageStoreConnectionString" Value="file:D:\ServiceFabric\Data\ImageStore" />
    </Section>

    [...]
```

Bkz. Görüntü deposu ve görüntü deposu bağlantı dizesi hakkında ek bilgiler için [görüntü deposu bağlantı dizesini anlayın.](service-fabric-image-store-connection-string.md)

### <a name="deploy-large-application-package"></a>Büyük uygulama paketini dağıtma
Sorun: [CopyApplicationPackage](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.copyapplicationpackage) API büyük bir uygulama paketi (GB sırası) için zaman ları dışarı.
Deneyin:
- [Parametreli CopyApplicationPackage](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.copyapplicationpackage) yöntemi için `timeout` daha büyük bir zaman ara sını belirtin. Varsayılan olarak, zaman ası 30 dakikadır.
- Kaynak makineniz ve kümeniz arasındaki ağ bağlantısını denetleyin. Bağlantı yavaşsa, daha iyi ağ bağlantısına sahip bir makine kullanmayı düşünün.
İstemci makine kümeden başka bir bölgedeyse, istemci makinesini kümeyle daha yakın veya aynı bölgede kullanmayı düşünün.
- Harici azaltma isabet olup olmadığını kontrol edin. Örneğin, görüntü deposu azure depolamayı kullanacak şekilde yapılandırıldığında, yükleme azaltılabilir.

Sorun: Yükleme paketi başarıyla tamamlandı, ancak [ProvisionApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync) API kez dışarı. Deneyin:
- Resim [deposuna kopyalamadan](service-fabric-package-apps.md#compress-a-package) önce paketi sıkıştırın.
Sıkıştırma, dosya boyutunu ve sayısını azaltır ve bu da Hizmet Kumaşı'nın gerçekleştirmesi gereken trafik ve çalışma miktarını azaltır. Yükleme işlemi daha yavaş olabilir (özellikle sıkıştırma süresini de eklerseniz), ancak uygulama türünü kaydedip not alın, daha hızlıdır.
- Parametreli [ProvisionApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync) API için `timeout` daha büyük bir zaman arası belirtin.

### <a name="deploy-application-package-with-many-files"></a>Birçok dosyaiçeren uygulama paketini dağıtma
Sorun: [ProvisionApplicationBirçok](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync) dosya (binlerce sırayla) ile bir uygulama paketi için zaman ları dışarı.
Deneyin:
- Resim [deposuna kopyalamadan](service-fabric-package-apps.md#compress-a-package) önce paketi sıkıştırın. Sıkıştırma dosya sayısını azaltır.
- [Parametreli ProvisionApplicationAsync](/dotnet/api/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync) için `timeout` daha büyük bir zaman arası belirtin.

## <a name="code-example"></a>Kod örneği
Aşağıdaki örnek, bir uygulama paketini görüntü deposuna kopyalar ve uygulama türünü karşılar. Ardından, örnek bir uygulama örneği oluşturur ve bir hizmet örneği oluşturur. Son olarak, örnek uygulama örneğini kaldırır, uygulama türünü unprovisions ve resim deposundan uygulama paketini siler.

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Reflection;
using System.Threading.Tasks;

using System.Fabric;
using System.Fabric.Description;
using System.Threading;

namespace ServiceFabricAppLifecycle
{
class Program
{
static void Main(string[] args)
{

    string clusterConnection = "localhost:19000";
    string appName = "fabric:/MyApplication";
    string appType = "MyApplicationType";
    string appVersion = "1.0.0";
    string serviceName = "fabric:/MyApplication/Stateless1";
    string imageStoreConnectionString = "file:C:\\SfDevCluster\\Data\\ImageStoreShare";
    string packagePathInImageStore = "MyApplication";
    string packagePath = "C:\\Users\\username\\Documents\\Visual Studio 2019\\Projects\\MyApplication\\MyApplication\\pkg\\Debug";
    string serviceType = "Stateless1Type";

    // Connect to the cluster.
    FabricClient fabricClient = new FabricClient(clusterConnection);

    // Copy the application package to a location in the image store
    try
    {
        fabricClient.ApplicationManager.CopyApplicationPackage(imageStoreConnectionString, packagePath, packagePathInImageStore);
        Console.WriteLine("Application package copied to {0}", packagePathInImageStore);
    }
    catch (AggregateException ae)
    {
        Console.WriteLine("Application package copy to Image Store failed: ");
        foreach (Exception ex in ae.InnerExceptions)
        {
            Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
        }
    }

    // Provision the application.  "MyApplicationV1" is the folder in the image store where the application package is located. 
    // The application type with name "MyApplicationType" and version "1.0.0" (both are found in the application manifest) 
    // is now registered in the cluster.            
    try
    {
        fabricClient.ApplicationManager.ProvisionApplicationAsync(packagePathInImageStore).Wait();

        Console.WriteLine("Provisioned application type {0}", packagePathInImageStore);
    }
    catch (AggregateException ae)
    {
        Console.WriteLine("Provision Application Type failed:");

        foreach (Exception ex in ae.InnerExceptions)
        {
            Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
        }
    }

    // Delete the application package from a location in the image store.
    try
    {
        fabricClient.ApplicationManager.RemoveApplicationPackage(imageStoreConnectionString, packagePathInImageStore);
        Console.WriteLine("Application package removed from {0}", packagePathInImageStore);
    }
    catch (AggregateException ae)
    {
        Console.WriteLine("Application package removal from Image Store failed: ");
        foreach (Exception ex in ae.InnerExceptions)
        {
            Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
        }
    }

    //  Create the application instance.
    try
    {
        ApplicationDescription appDesc = new ApplicationDescription(new Uri(appName), appType, appVersion);
        fabricClient.ApplicationManager.CreateApplicationAsync(appDesc).Wait();
        Console.WriteLine("Created application instance of type {0}, version {1}", appType, appVersion);
    }
    catch (AggregateException ae)
    {
        Console.WriteLine("CreateApplication failed.");
        foreach (Exception ex in ae.InnerExceptions)
        {
            Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
        }
    }

    // Create the stateless service description.  For stateful services, use a StatefulServiceDescription object.
    StatelessServiceDescription serviceDescription = new StatelessServiceDescription();
    serviceDescription.ApplicationName = new Uri(appName);
    serviceDescription.InstanceCount = 1;
    serviceDescription.PartitionSchemeDescription = new SingletonPartitionSchemeDescription();
    serviceDescription.ServiceName = new Uri(serviceName);
    serviceDescription.ServiceTypeName = serviceType;

    // Create the service instance.  If the service is declared as a default service in the ApplicationManifest.xml,
    // the service instance is already running and this call will fail.
    try
    {
        fabricClient.ServiceManager.CreateServiceAsync(serviceDescription).Wait();
        Console.WriteLine("Created service instance {0}", serviceName);
    }
    catch (AggregateException ae)
    {
        Console.WriteLine("CreateService failed.");
        foreach (Exception ex in ae.InnerExceptions)
        {
            Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
        }
    }

    // Delete a service instance.
    try
    {
        DeleteServiceDescription deleteServiceDescription = new DeleteServiceDescription(new Uri(serviceName));

        fabricClient.ServiceManager.DeleteServiceAsync(deleteServiceDescription);
        Console.WriteLine("Deleted service instance {0}", serviceName);
    }
    catch (AggregateException ae)
    {
        Console.WriteLine("DeleteService failed.");
        foreach (Exception ex in ae.InnerExceptions)
        {
            Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
        }
    }

    // Delete an application instance from the application type.
    try
    {
        DeleteApplicationDescription deleteApplicationDescription = new DeleteApplicationDescription(new Uri(appName));

        fabricClient.ApplicationManager.DeleteApplicationAsync(deleteApplicationDescription).Wait();
        Console.WriteLine("Deleted application instance {0}", appName);
    }
    catch (AggregateException ae)
    {
        Console.WriteLine("DeleteApplication failed.");
        foreach (Exception ex in ae.InnerExceptions)
        {
            Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
        }
    }

    // Un-provision the application type.
    try
    {
        fabricClient.ApplicationManager.UnprovisionApplicationAsync(appType, appVersion).Wait();
        Console.WriteLine("Un-provisioned application type {0}, version {1}", appType, appVersion);
    }
    catch (AggregateException ae)
    {
        Console.WriteLine("Un-provision application type failed: ");
        foreach (Exception ex in ae.InnerExceptions)
        {
            Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
        }
    }

    Console.WriteLine("Hit enter...");
    Console.Read();
}        
}
}

```

## <a name="next-steps"></a>Sonraki adımlar
[Hizmet Kumaş uygulama yükseltme](service-fabric-application-upgrade.md)

[Hizmet Kumaş sağlık giriş](service-fabric-health-introduction.md)

[Bir Service Fabric hizmetini tanılama ve sorun giderme](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[Hizmet Kumaşı'nda bir uygulama modeli](service-fabric-application-model.md)

<!--Link references--In actual articles, you only need a single period before the slash-->
[10]: service-fabric-package-apps.md
[11]: service-fabric-application-upgrade.md
