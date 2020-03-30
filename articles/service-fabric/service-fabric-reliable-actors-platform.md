---
title: Servis Kumaşında Güvenilir Aktörler
description: Güvenilir Aktörlerin Güvenilir Hizmetler'de nasıl katmanlandırılanları açıklar ve Service Fabric platformunun özelliklerini kullanır.
author: vturecek
ms.topic: conceptual
ms.date: 3/9/2018
ms.author: vturecek
ms.openlocfilehash: 92c717fa2c82dd147acd3c28333e37ccf8dd2e89
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79282308"
---
# <a name="how-reliable-actors-use-the-service-fabric-platform"></a>Güvenilir Aktörler Service Fabric platformını nasıl kullanır?
Bu makalede, Güvenilir Aktörler'in Azure Hizmet Kumaşı platformunda nasıl çalıştığı açıklanmaktadır. Güvenilir Aktörler *aktör hizmeti*olarak adlandırılan bir devlet güvenilir hizmet in bir uygulamada barındırılan bir çerçevede çalıştırın. Aktör hizmeti, oyuncularınız için yaşam döngüsünü ve ileti göndermeyi yönetmek için gereken tüm bileşenleri içerir:

* Aktör Runtime yaşam döngüsünü, çöp toplamayı yönetir ve tek iş parçacığı erişimi zorlar.
* Bir aktör hizmeti remoting dinleyici aktörlere uzaktan erişim çağrıları kabul eder ve uygun aktör örneğine yönlendirmek için bir gönderici gönderir.
* Aktör Devlet Sağlayıcısı, devlet sağlayıcılarını (Güvenilir Koleksiyonlar devlet sağlayıcısı gibi) sarar ve aktör devlet yönetimi için bir bağdaştırıcı sağlar.

Bu bileşenler birlikte Güvenilir Aktör çerçevesini oluşturur.

## <a name="service-layering"></a>Hizmet katmanı
Aktör hizmetinin kendisi güvenilir bir hizmet olduğundan, tüm [uygulama modeli,](service-fabric-application-model.md)yaşam döngüsü, [paketleme,](service-fabric-package-apps.md) [dağıtım,](service-fabric-deploy-remove-applications.md)yükseltme ve Güvenilir Hizmetler ölçekleme kavramları aktör hizmetlerine aynı şekilde uygulanır.

![Aktör hizmet katmanı][1]

Önceki diyagram, Hizmet Dokusu uygulama çerçeveleri ile kullanıcı kodu arasındaki ilişkiyi gösterir. Mavi öğeler Güvenilir Hizmetler uygulama çerçevesini, turuncu güvenilir aktör çerçevesini ve yeşil kullanıcı kodunu temsil eder.

Güvenilir Hizmetlerde, hizmetiniz `StatefulService` sınıfı devralır. Bu sınıfın kendisi `StatefulServiceBase` (veya `StatelessService` devletsiz hizmetler için) türetilmiştir. Güvenilir Aktörler'de aktör hizmetini kullanırsınız. Aktör hizmeti, aktörlerinizin `StatefulServiceBase` çalıştığı aktör deseni uygulayan sınıfın farklı bir uygulamasıdır. Aktör hizmetinin kendisi sadece bir `StatefulServiceBase`uygulama olduğundan, örneğin, devralırken `ActorService` `StatefulService`olduğu gibi hizmet düzeyi özelliklerinden türetilmiş ve hizmet düzeyi özelliklerini uygulayan kendi hizmetinizi yazabilirsiniz:

* Servis yedekleme ve geri yükleme.
* Tüm aktörler için paylaşılan işlevsellik, örneğin, bir devre kesici.
* Uzaktan yordam aktör hizmeti kendisi ve her aktör çağırır.

Daha fazla bilgi için bkz: [Aktör hizmetinizde hizmet düzeyi özellikleri uygulama.](service-fabric-reliable-actors-using.md)

## <a name="application-model"></a>Uygulama modeli
Aktör hizmetleri Güvenilir Hizmetlerdir, bu nedenle uygulama modeli aynıdır. Ancak, aktör çerçeve oluşturma araçları sizin için bazı uygulama modeli dosyaları oluşturur.

### <a name="service-manifest"></a>Hizmet bildirimi
Aktör çerçeve oluşturma araçları otomatik olarak aktör hizmetServiceManifest.xml dosyasının içeriğini oluşturur. Bu dosya şunları içerir:

* Aktör hizmet türü. Tür adı, aktörünüzün proje adına göre oluşturulur. Aktörünüzdeki kalıcılık özelliğine bağlı olarak, HasPersistedState bayrağı da buna göre ayarlanır.
* Kod paketi.
* Config paketi.
* Kaynaklar ve uç noktalar.

### <a name="application-manifest"></a>Uygulama bildirimi
Aktör çerçeve oluşturma araçları otomatik olarak aktör hizmet için varsayılan bir hizmet tanımı oluşturur. Yapı araçları varsayılan hizmet özelliklerini dolduruyor:

* Yineleme kümesi sayısı, aktörünüzdeki kalıcılık özniteliğine göre belirlenir. Aktörünüzün kalıcılık özniteliği her değiştirilince, varsayılan hizmet tanımındaki yineleme kümesi sayısı buna göre sıfırlanır.
* Bölüm düzeni ve aralığı tam Int64 anahtar aralığı ile Uniform Int64 olarak ayarlanır.

## <a name="service-fabric-partition-concepts-for-actors"></a>Aktörler için Hizmet Kumaş bölümkavramları
Aktör hizmetleri, bölümlenmiş devlet hizmetleridir. Bir aktör hizmetinin her bölümü bir dizi aktör içerir. Hizmet bölümleri, Hizmet Kumaşı'ndaki birden çok düğüme otomatik olarak dağıtılır. Sonuç olarak aktör örnekleri dağıtılır.

![Aktör bölümleme ve dağıtım][5]

Güvenilir Hizmetler, farklı bölüm şemaları ve bölüm anahtar aralıkları ile oluşturulabilir. Aktör hizmeti, aktörleri bölümlere eşlemek için tam Int64 tuş aralığıyla Int64 bölümleme düzenini kullanır.

### <a name="actor-id"></a>Aktör Kimliği
Hizmette oluşturulan her aktörün, `ActorId` sınıf tarafından temsil edilen benzersiz bir kimliği vardır. `ActorId`rasgele kimlikler oluşturarak aktörlerin hizmet bölümleri arasında tek tip dağıtımı için kullanılabilecek opak bir kimlik değeridir:

```csharp
ActorProxy.Create<IMyActor>(ActorId.CreateRandom());
```
```Java
ActorProxyBase.create<MyActor>(MyActor.class, ActorId.newId());
```


Her `ActorId` bir Int64 için haşlanır. Bu nedenle aktör hizmeti, tam Int64 anahtar aralığına sahip bir Int64 bölümleme düzenini kullanmalıdır. Ancak, GUID'ler/Kullanıcı `ActorID`Araları, dizeleri ve Int64'ler dahil olmak üzere bir , özel kimlik değerleri için kullanılabilir.

```csharp
ActorProxy.Create<IMyActor>(new ActorId(Guid.NewGuid()));
ActorProxy.Create<IMyActor>(new ActorId("myActorId"));
ActorProxy.Create<IMyActor>(new ActorId(1234));
```
```Java
ActorProxyBase.create(MyActor.class, new ActorId(UUID.randomUUID()));
ActorProxyBase.create(MyActor.class, new ActorId("myActorId"));
ActorProxyBase.create(MyActor.class, new ActorId(1234));
```

GUID'ler/UUID'ler ve dizeleri kullanırken, değerler bir Int64'e işlenir. Ancak, bir Int64'e açıkça bir Int64 `ActorId`sağladığınızda, Int64 daha fazla karma yapmadan doğrudan bir bölümle eşlenecektir. Aktörlerin hangi bölüme yerleştirildiğini denetlemek için bu tekniği kullanabilirsiniz.


## <a name="next-steps"></a>Sonraki adımlar
* [Aktör devlet yönetimi](service-fabric-reliable-actors-state-management.md)
* [Aktör yaşam döngüsü ve çöp toplama](service-fabric-reliable-actors-lifecycle.md)
* [Aktörler API başvuru belgeleri](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.actors?redirectedfrom=MSDN&view=azure-dotnet)
* [.NET örnek kodu](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [Java örnek kodu](https://github.com/Azure-Samples/service-fabric-java-getting-started)

<!--Image references-->
[1]: ./media/service-fabric-reliable-actors-platform/actor-service.png
[2]: ./media/service-fabric-reliable-actors-platform/app-deployment-scripts.png
[3]: ./media/service-fabric-reliable-actors-platform/actor-partition-info.png
[4]: ./media/service-fabric-reliable-actors-platform/actor-replica-role.png
[5]: ./media/service-fabric-reliable-actors-introduction/distribution.png
