---
title: Mevcut bir yürütülebilir yazılımı Azure Hizmet Kumaşı'na paketle
description: Varolan bir uygulamayı, hizmet kumaşı kümesine dağıtılabilmek için konuk çalıştırılabilir olarak paketleme hakkında bilgi edinin.
ms.topic: conceptual
ms.date: 03/15/2018
ms.openlocfilehash: 3d7aab28a32effa2caf7b04b830d72e5e3dfda56
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75457824"
---
# <a name="deploy-an-existing-executable-to-service-fabric"></a>Hizmet Kumaşı'na mevcut bir çalıştırılabilir dağıtma
Hizmet olarak Azure Hizmet Kumaşı'nda Node.js, Java veya C++ gibi her türlü kodu çalıştırabilirsiniz. Service Fabric, bu tür hizmetleri konuk yürütülebilir olarak ifade eder.

Konuk icra edilebilir ler Service Fabric tarafından vatansız hizmetler gibi ele alınır. Sonuç olarak, kullanılabilirlik ve diğer ölçümlere bağlı olarak kümedeki düğümlere yerleştirilirler. Bu makalede, Visual Studio veya komut satırı yardımcı programını kullanarak, bir Hizmet Kumaşı kümesine çalıştırılabilen bir konuğun nasıl paketleyip dağıtılacağa kadar açıklanmaktadır.

## <a name="benefits-of-running-a-guest-executable-in-service-fabric"></a>Hizmet Kumaşı'nda çalıştırılabilen bir misafiri çalıştırmanın faydaları
Hizmet Kumaşı kümesinde çalıştırılabilen bir konuk çalıştırmanın birkaç avantajı vardır:

* Yüksek kullanılabilirlik. Service Fabric'te çalışan uygulamalar son derece kullanılabilir hale getirilir. Service Fabric, uygulama örneklerinin çalışmasını sağlar.
* Sağlık takibi. Service Fabric sistem durumu izleme, bir uygulamanın çalışıyorsa algılar ve bir hata varsa tanılama bilgileri sağlar.   
* Uygulama yaşam döngüsü yönetimi. Hiçbir kesinti ile yükseltmeleri sağlamanın yanı sıra, Servis Kumaş bir yükseltme sırasında bildirilen kötü bir sağlık olayı varsa önceki sürüme otomatik geri alma sağlar.    
* Yoğun -luğu. Bir kümede birden çok uygulama çalıştırabilirsiniz, bu da her uygulamanın kendi donanımında çalışması gereksinimini ortadan kaldırır.
* Keşfedilebilirlik: REST'i kullanarak kümedeki diğer hizmetleri bulmak için Servis Kumaşı Adlandırma hizmetini arayabilirsiniz. 

## <a name="samples"></a>Örnekler
* [Uygulanabilir bir konuğu paketlemek ve dağıtmak için örnek](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [REST kullanarak Adlandırma hizmeti aracılığıyla iletişim kuran iki konuk çalıştırılabilir (C# ve nodejs) örneği](https://github.com/Azure-Samples/service-fabric-dotnet-containers)

## <a name="overview-of-application-and-service-manifest-files"></a>Uygulama ve hizmet bildirimi dosyalarına genel bakış
Bir konuk çalıştırılabilir dağıtmanın bir parçası olarak, [uygulama modelinde](service-fabric-application-model.md)açıklandığı gibi Hizmet Kumaş ambalaj ve dağıtım modeli anlamak yararlıdır. Service Fabric ambalaj modeli iki XML dosyasına dayanır: uygulama ve hizmet bildirimleri. ApplicationManifest.xml ve ServiceManifest.xml dosyaları için şema tanımı *C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*içine Hizmet Kumaş SDK ile yüklenir.

* **Başvuru bildirimi** Uygulama bildirimi, uygulamayı tanımlamak için kullanılır. Bu, onu oluşturan hizmetleri ve örnek sayısı gibi bir veya daha fazla hizmetin nasıl dağıtılmasını tanımlamak için kullanılan diğer parametreleri listeler.

  Hizmet Kumaşı'nda uygulama dağıtım ve yükseltme birimidir. Bir uygulama, olası hataların ve olası geri almaların yönetildiği tek bir birim olarak yükseltilebilir. Service Fabric, yükseltme işleminin başarılı olduğunu veya yükseltme başarısız olursa uygulamayı bilinmeyen veya kararsız durumda bırakmadığını garanti eder.
* **Hizmet bildirimi** Hizmet bildirimi, bir hizmetin bileşenlerini açıklar. Hizmetin adı ve türü, kodu ve yapılandırması gibi verileri içerir. Hizmet bildirimi, dağıtıldıktan sonra hizmeti yapılandırmak için kullanılabilecek bazı ek parametreler de içerir.

## <a name="application-package-file-structure"></a>Uygulama paketi dosya yapısı
Bir uygulamayı Hizmet Kumaşı'na dağıtmak için, uygulama önceden tanımlanmış bir dizin yapısını izlemelidir. Aşağıda bu yapının bir örneği verilmiştir.

```
|-- ApplicationPackageRoot
    |-- GuestService1Pkg
        |-- Code
            |-- existingapp.exe
        |-- Config
            |-- Settings.xml
        |-- Data
        |-- ServiceManifest.xml
    |-- ApplicationManifest.xml
```

ApplicationPackageRoot, uygulamayı tanımlayan ApplicationManifest.xml dosyasını içerir. Uygulamada yer alan her hizmet için bir alt dizini, hizmetin gerektirdiği tüm yapıları içermek için kullanılır. Bu alt dizinler ServiceManifest.xml ve genellikle aşağıdaki gibidir:

* *Kod*. Bu dizin hizmet kodunu içerir.
* *Config*. Bu dizin, hizmetin belirli yapılandırma ayarlarını almak için çalışma zamanında erişebileceği bir Settings.xml dosyası (ve gerekirse diğer dosyalar) içerir.
* *Veri*. Bu, hizmetin ihtiyaç duyabileceği ek yerel verileri depolamak için ek bir dizinidir. Veriler yalnızca geçici verileri depolamak için kullanılmalıdır. Hizmet Dokusu, hizmetin taşınması gerekiyorsa (örneğin, başarısız olma sırasında) veri dizinindeki değişiklikleri kopyalamaz veya çoğaltmaz.

> [!NOTE]
> İhtiyacınız yoksa `config` dizinleri `data` oluşturmak zorunda değilsiniz.
>
>

## <a name="next-steps"></a>Sonraki adımlar
İlgili bilgi ve görevler için aşağıdaki makalelere bakın.
* [Konuk tarafından yürütülebilir bir uygulama dağıtma](service-fabric-deploy-existing-app.md)
* [Konuk tarafından yürütülebilir birden çok uygulama dağıtma](service-fabric-deploy-multiple-apps.md)
* [Visual Studio'yu kullanarak ilk konuk çalıştırılabilir uygulamanızı oluşturun](quickstart-guest-app.md)
* Paketleme aracının ön salıverilme bağlantısı da dahil olmak üzere, uygulanabilir bir konuk paketleme [ve dağıtma için örnek](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [REST kullanarak Adlandırma hizmeti aracılığıyla iletişim kuran iki konuk çalıştırılabilir (C# ve nodejs) örneği](https://github.com/Azure-Samples/service-fabric-containers)

