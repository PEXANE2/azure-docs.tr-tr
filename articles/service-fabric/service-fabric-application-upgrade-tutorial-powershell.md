---
title: PowerShell kullanarak Servis Kumaş Uygulaması yükseltmesi
description: Bu makalede, bir Service Fabric uygulaması dağıtma, kodu değiştirme ve PowerShell kullanarak bir yükseltme gönderme deneyimi üzerinden yürür.
ms.topic: conceptual
ms.date: 2/23/2018
ms.openlocfilehash: b113b5a1042518e3b0d86e53796c5fe49afed418
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75426791"
---
# <a name="service-fabric-application-upgrade-using-powershell"></a>PowerShell kullanarak Servis Kumaş ı uygulama yükseltmesi
> [!div class="op_single_selector"]
> * [Powershell](service-fabric-application-upgrade-tutorial-powershell.md)
> * [Visual Studio](service-fabric-application-upgrade-tutorial.md)
> 
> 

<br/>

En sık kullanılan ve önerilen yükseltme yaklaşımı izlenen haddeleme yükseltmesidir.  Azure Hizmet Kumaşı, bir dizi sistem durumu ilkelerine göre yükseltilen uygulamanın sistem durumunu izler. Güncelleştirme etki alanı (UD) yükseltildikten sonra, Service Fabric uygulama durumunu değerlendirir ve bir sonraki güncelleştirme etki alanına devam eder veya sistem durumu ilkelerine bağlı olarak yükseltmede başarısız olur.

İzlenen bir uygulama yükseltmesi yönetilen veya yerel API'ler, PowerShell, Azure CLI, Java veya REST kullanılarak gerçekleştirilebilir. Visual Studio kullanarak yükseltme yapmakla ilgili talimatlar için Visual [Studio'yu kullanarak uygulamanızı yükseltme](service-fabric-application-upgrade-tutorial.md)konusuna bakın.

Hizmet Kumaşı'nın izlenen haddeleme yükseltmeleri ile, uygulama yöneticisi Hizmet Dokusu'nun uygulamanın sağlıklı olup olmadığını belirlemek için kullandığı sistem durumu değerlendirme ilkesini yapılandırabilir. Buna ek olarak, yönetici sistem durumu değerlendirmesi başarısız olduğunda (örneğin, otomatik geri alma işi yle) yapılacak eylemi yapılandırabilir. Bu bölüm, PowerShell kullanan SDK örneklerinden biri için izlenen bir yükseltmeden geçer. 

## <a name="step-1-build-and-deploy-the-visual-objects-sample"></a>Adım 1: Görsel Nesneler örneğini oluşturma ve dağıtma
Uygulama projesi **VisualObjectsApplication'a** sağ tıklayarak ve **Yayımla** komutunu seçerek uygulamayı oluşturun ve yayımlayın.  Daha fazla bilgi için [Service Fabric uygulama yükseltme öğreticisi'ne](service-fabric-application-upgrade-tutorial.md)bakın.  Alternatif olarak, uygulamanızı dağıtmak için PowerShell'i kullanabilirsiniz.

> [!NOTE]
> Service Fabric komutlarından herhangi biri PowerShell'de kullanılmadan `Connect-ServiceFabricCluster` önce, önce cmdlet'i kullanarak kümeye bağlanmanız gerekir. Benzer şekilde, Küme'nin yerel makinenizde zaten kurulduğu varsayılır. Service Fabric [geliştirme ortamınızı ayarlama](service-fabric-get-started.md)hakkındaki makaleye bakın.
> 
> 

Visual Studio'da projeyi yaptıktan sonra, uygulama paketini ImageStore'a kopyalamak için PowerShell komutu [Copy-ServiceFabricApplicationPackage'ı](/powershell/module/servicefabric/copy-servicefabricapplicationpackage) kullanabilirsiniz. Uygulama paketini yerel olarak doğrulamak istiyorsanız, [Test-ServiceFabricApplicationPackage](/powershell/module/servicefabric/test-servicefabricapplicationpackage) cmdlet'i kullanın. Bir sonraki [adım, Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype) cmdlet kullanarak uygulamayı Servis Kumaşı çalışma süresine kaydetmektir. Aşağıdaki [adım, Yeni ServisKumaş Uygulaması](/powershell/module/servicefabric/new-servicefabricapplication?view=azureservicefabricps) cmdlet kullanarak uygulamanın bir örneğini başlatmaktır.  Bu üç adım Visual Studio'daki **Dağıt** menüsü öğesini kullanmaya benzer.  Sağlama tamamlandıktan sonra, tüketilen kaynakları azaltmak için kopyalanan uygulama paketini görüntü deposundan temizlemeniz gerekir.  Bir uygulama türü artık gerekli değilse, aynı nedenle kayıtsız olmalıdır. Daha fazla bilgi için [PowerShell'i kullanarak uygulamaları dağıt'a](service-fabric-application-upgrade-tutorial-powershell.md) bakın ve kaldırın.

Şimdi, küme [ve uygulamayı görüntülemek için Service Fabric Explorer'ı](service-fabric-visualizing-your-cluster.md)kullanabilirsiniz. Uygulama, adres çubuğuna yazarak [http://localhost:8081/visualobjects](http://localhost:8081/visualobjects) Internet Explorer'da gezinilebilen bir web hizmetine sahiptir.  Ekranda hareket eden bazı yüzen görsel nesneler görmeniz gerekir.  Ayrıca, uygulama durumunu kontrol etmek için [Get-ServiceFabricApplication'ı](/powershell/module/servicefabric/get-servicefabricapplication?view=azureservicefabricps) kullanabilirsiniz.

## <a name="step-2-update-the-visual-objects-sample"></a>Adım 2: Görsel Nesneler örneğini güncelleştirme
Adım 1'de dağıtılan sürümle görsel nesnelerin dönmediğini fark edebilirsiniz. Bu uygulamayı görsel nesnelerin de döndüğü bir uygulamaya yükseltelim.

VisualObjects.ActorService projesini VisualObjects çözümü nde seçin ve StatefulVisualObjectActor.cs dosyasını açın. Bu dosya içinde, yönteme `MoveObject`gidin `this.State.Move()`, yorum `this.State.Move(true)`dışarı , ve yorum yapmayın . Bu değişiklik, hizmet yükseltildikten sonra nesneleri döndürür.

Ayrıca Project **VisualObjects.ActorService** *ServiceManifest.xml* dosyasını (PackageRoot altında) güncellememiz gerekir. *CodePackage'ı* ve hizmet sürümünü 2.0'a ve *ServiceManifest.xml* dosyasındaki ilgili satırları güncelleştirin.
Bildirim dosyası değişikliklerini yapmak için çözüme sağ tıkladıktan sonra Visual Studio *Edit Manifest Files* seçeneğini kullanabilirsiniz.

Değişiklikler yapıldıktan sonra, bildirim aşağıdaki gibi görünmelidir (vurgulanan bölümler değişiklikleri gösterir):

```xml
<ServiceManifestName="VisualObjects.ActorService" Version="2.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance">

<CodePackageName="Code" Version="2.0">
```

Şimdi *ApplicationManifest.xml* dosyası **(VisualObjects** çözümü altında **VisualObjects** projesi altında bulunan) **VisualObjects.ActorService** projesinin 2.0 sürümüne güncellenir. Ayrıca, Uygulama sürümü 1.0.0.0'dan 2.0.0.0 olarak güncelleştirilir. *ApplicationManifest.xml* aşağıdaki parçacık gibi görünmelidir:

```xml
<ApplicationManifestxmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="VisualObjects" ApplicationTypeVersion="2.0.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">

 <ServiceManifestRefServiceManifestName="VisualObjects.ActorService" ServiceManifestVersion="2.0" />
```

Şimdi, projeyi yalnızca **ActorService** projesini seçerek ve ardından Visual Studio'da **Yapı** seçeneğini sağ tıklayarak ve seçerek oluşturun. **Tümünü Yeniden Oluştur'u**seçerseniz, kod değiştiğinden tüm projelerin sürümlerini güncelleştirmeniz gerekir. Ardından, ***VisualObjectsApplication'a***sağ tıklayarak, Hizmet Kumaş Menüsünü seçerek ve **Paket'i**seçerek güncelleştirilmiş uygulamayı paketleyelim. Bu eylem dağıtılabilir bir uygulama paketi oluşturur.  Güncelleştirilmiş uygulamanız dağıtılacak hazırdır.

## <a name="step-3--decide-on-health-policies-and-upgrade-parameters"></a>Adım 3: Sağlık ilkeleri ne karar ve yükseltme parametrelerine karar verin
Uygulanan çeşitli yükseltme parametreleri, zaman-out'lar ve sistem durumu ölçütü hakkında iyi bir bilgi almak için [uygulama yükseltme parametrelerini](service-fabric-application-upgrade-parameters.md) ve [yükseltme işlemini](service-fabric-application-upgrade.md) öğrenin. Bu izlenecek yol için, hizmet durumu değerlendirme ölçütü varsayılan (ve önerilen) değerlere ayarlanır, bu da yükseltmeden sonra tüm hizmetlerin ve örneklerin *sağlıklı* olması gerektiği anlamına gelir.  

Ancak, *HealthCheckStableDuration'ı* 180 saniyeye yükseltelim (böylece yükseltme bir sonraki güncelleştirme etki alanına geçmeden önce hizmetler en az 120 saniye sağlıklı olsun).  *Ayrıca UpgradeDomainTimeout'u* 1200 saniye, *UpgradeTimeout'ı* ise 3000 saniye olarak ayarlayalım.

Son olarak, *UpgradeFailureAction'ı* geri almaya da ayarlayalım. Bu seçenek, yükseltme sırasında herhangi bir sorunla karşılaşırsa, Hizmet Kumaşı'nın uygulamayı önceki sürüme geri döndürmesini gerektirir. Bu nedenle, yükseltmebaşlatılırken (Adım 4'te), aşağıdaki parametreler belirtilir:

FailureAction = Geri Alma

HealthCheckStableDurationSec = 180

UpgradeDomainTimeoutSec = 1200

UpgradeTimeout = 3000

## <a name="step-4-prepare-application-for-upgrade"></a>Adım 4: Yükseltme için uygulama hazırlayın
Şimdi uygulama inşa edilmiş ve yükseltilmeye hazır. Bir PowerShell penceresini yönetici olarak açar ve [Get-ServiceFabricApplication](/powershell/module/servicefabric/get-servicefabricapplication?view=azureservicefabricps)yazarsanız, dağıtılan **VisualObjects'in** 1.0.0.0 uygulama türü olduğunu bilmeniz gerekir.  

Uygulama paketi, Hizmet Kumaşı SDK - *Örnekler\Hizmetler\Stateful\VisualObjects\VisualObjects\obj\x64\Debug*sıkıştırılmış aşağıdaki göreli yol altında saklanır. Uygulama paketinin depolandığı bu dizinde bir "Paket" klasörü bulmanız gerekir. En son yapı olduğundan emin olmak için zaman damgalarını denetleyin (yolları da uygun şekilde değiştirmeniz gerekebilir).

Şimdi güncelleştirilmiş uygulama paketini Service Fabric ImageStore'a kopyalayalım (uygulama paketlerinin Service Fabric tarafından depolandığı yer). *Parametre ApplicationPackagePathInImageStore,* servis paketini bulabileceği Servis Kumaşı'nı bilgilendirir. Güncelleştirilmiş uygulamayı aşağıdaki komutla "VisualObjects\_V2"ye koyduk (yolları uygun şekilde yeniden değiştirmeniz gerekebilir).

```powershell
Copy-ServiceFabricApplicationPackage -ApplicationPackagePath .\Samples\Services\Stateful\VisualObjects\VisualObjects\obj\x64\Debug\Package -ApplicationPackagePathInImageStore "VisualObjects\_V2"
```

Bir sonraki adım, bu uygulamayı [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) komutu kullanılarak gerçekleştirilebilen Service Fabric ile kaydetmektir:

```powershell
Register-ServiceFabricApplicationType -ApplicationPathInImageStore "VisualObjects\_V2"
```

Önceki komut başarılı olmazsa, büyük olasılıkla tüm hizmetlerin yeniden oluşturulmasıgerekir. Adım 2'de belirtildiği gibi, WebService sürümünüzü de güncelleştirmeniz gerekebilir.

Uygulama başarıyla kaydedildikten sonra uygulama paketini kaldırmanız önerilir.  Uygulama paketlerini görüntü deposundan silerse sistem kaynaklarını boşaltAr.  Kullanılmayan uygulama paketlerinin tutulması disk depolamayı tüketir ve uygulama performansı sorunlarına yol açar.

```powershell
Remove-ServiceFabricApplicationPackage -ApplicationPackagePathInImageStore "VisualObjects\_V2" -ImageStoreConnectionString fabric:ImageStore
```

## <a name="step-5-start-the-application-upgrade"></a>Adım 5: Uygulama yükseltmesini başlatın
Şimdi, hepimiz [Başlat-ServiceFabricApplicationUpgrade](/powershell/module/servicefabric/start-servicefabricapplicationupgrade?view=azureservicefabricps) komutunu kullanarak uygulama yükseltme başlatmak için ayarlanır:

```powershell
Start-ServiceFabricApplicationUpgrade -ApplicationName fabric:/VisualObjects -ApplicationTypeVersion 2.0.0.0 -HealthCheckStableDurationSec 60 -UpgradeDomainTimeoutSec 1200 -UpgradeTimeout 3000   -FailureAction Rollback -Monitored
```


Uygulama adı *ApplicationManifest.xml* dosyasında açıklandığı ile aynıdır. Service Fabric, hangi uygulamanın yükseltilmeye devam olduğunu belirlemek için bu adı kullanır. Zaman zaman larını çok kısa olacak şekilde ayarlarsanız, sorunu belirten bir hata iletisi ile karşılaşabilirsiniz. Sorun giderme bölümüne bakın veya zaman zamanlarını artırın.

Şimdi, uygulama yükseltme devam ettikçe, Service Fabric Explorer kullanarak veya [Get-ServiceFabricApplicationUpgrade](/powershell/module/servicefabric/get-servicefabricapplicationupgrade?view=azureservicefabricps) PowerShell komutunu kullanarak izleyebilirsiniz: 

```powershell
Get-ServiceFabricApplicationUpgrade fabric:/VisualObjects
```

Birkaç dakika içinde, önceki PowerShell komutunu kullanarak aldığınız durum, tüm güncelleştirme etki alanlarının yükseltildi (tamamlandı) belirtmelidir. Ve tarayıcı pencerenizdeki görsel nesnelerin dönmeye başladığını bulmalısınız!

Sürüm 2'den sürüm 3'e veya sürüm 2'den sürüm 1'e alıştırma olarak yükseltmeyi deneyebilirsiniz. Sürüm 2'den sürüm 1'e geçiş de bir yükseltme olarak kabul edilir. Zaman zaman dilimleri ve sağlık politikaları ile kendinizi onları tanımak için oynayın. Bir Azure kümesine dağıtılırken parametrelerin uygun şekilde ayarlanması gerekir. Zaman çıkışlarını konservatif olarak ayarlamak iyidir.

## <a name="next-steps"></a>Sonraki adımlar
[Visual Studio kullanarak uygulamanızı yükseltmek,](service-fabric-application-upgrade-tutorial.md) Visual Studio'yu kullanarak bir uygulama yükseltmesi için size yol sağlar.

[Yükseltme parametrelerini](service-fabric-application-upgrade-parameters.md)kullanarak uygulamanızın nasıl yükselttikini kontrol edin.

[Veri serileştirmenin](service-fabric-application-upgrade-data-serialization.md)nasıl kullanılacağını öğrenerek uygulama yükseltmelerinizi uyumlu hale getirin.

[Gelişmiş konulara](service-fabric-application-upgrade-advanced.md)atıfta bulunarak uygulamanızı yükseltirken gelişmiş işlevselliği nasıl kullanacağınızı öğrenin.

[Sorun Giderme uygulama yükseltmeleri](service-fabric-application-upgrade-troubleshooting.md)adımlarına atıfta bulunarak uygulama yükseltmelerinde sık karşılaşılan sorunları giderin.

