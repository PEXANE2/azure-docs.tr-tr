---
title: Java 'da ilk güvenilir hizmetinizi oluşturma
description: Durum bilgisiz ve durum bilgisi olan hizmetlerle Microsoft Azure Service Fabric uygulama oluşturmaya giriş.
author: suhuruli
ms.topic: conceptual
ms.date: 11/02/2017
ms.author: suhuruli
ms.openlocfilehash: 7855b92c90a9ccd208a25080c260437e6808d1b7
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82184155"
---
# <a name="get-started-with-reliable-services-in-java"></a>Java 'da Reliable Services kullanmaya başlama
> [!div class="op_single_selector"]
> * [Windows üzerinde C#](service-fabric-reliable-services-quick-start.md)
> * [Linux üzerinde Java](service-fabric-reliable-services-quick-start-java.md)
>
>

Bu makalede, Azure Service Fabric Reliable Services temel bilgileri açıklanmakta ve Java 'da yazılmış basit bir güvenilir hizmet uygulaması oluşturma ve dağıtma işlemi adım adım açıklanmaktadır. 

## <a name="installation-and-setup"></a>Yükleme ve kurulum
Başlamadan önce, makinenizde Service Fabric geliştirme ortamının ayarlanmış olduğundan emin olun.
Ayarlamanız gerekirse [Mac](service-fabric-get-started-mac.md) 'e Başlarken veya [Linux üzerinde çalışmaya](service-fabric-get-started-linux.md)başlama bölümüne gidin.

## <a name="basic-concepts"></a>Temel kavramlar
Reliable Services kullanmaya başlamak için yalnızca birkaç temel kavramları anlamanız gerekir:

* **Hizmet türü**: Bu hizmet uygulamanız. Bu, yazdığınız sınıf tarafından, bir ad ve sürüm `StatelessService` numarası ile birlikte kullanılan diğer kod veya bağımlılıkları genişletir.
* **Adlandırılmış hizmet örneği**: hizmetinizi çalıştırmak için, bir sınıf türünün nesne örneklerini oluştururken olduğu gibi, hizmet türünden adlandırılmış örnekler oluşturursunuz. Hizmet örnekleri, yazdığınız hizmet sınıfınızın olgu nesne örneklemelerinden oluşur.
* **Hizmet ana bilgisayarı**: oluşturduğunuz adlandırılmış hizmet örneklerinin bir konak içinde çalıştırılması gerekir. Hizmet ana bilgisayarı yalnızca hizmetinizin örneklerinin çalıştırılabileceği bir işlemdir.
* **Hizmet kaydı**: kayıt her şeyi bir araya getirir. Service Fabric çalıştırmak için hizmet türü, bir hizmet ana bilgisayarındaki Service Fabric çalışma zamanına kaydedilmelidir.  

## <a name="create-a-stateless-service"></a>Durum bilgisi olmayan hizmet oluşturma
Service Fabric bir uygulama oluşturarak başlayın. Linux için Service Fabric SDK 'Sı, durum bilgisi olmayan bir hizmetle Service Fabric bir uygulama için yapı iskelesi sağlamak üzere Yeumman Oluşturucu içerir. Aşağıdaki Yeumman komutunu çalıştırarak başlayın:

```bash
$ yo azuresfjava
```

**Güvenilir bir durum bilgisiz hizmeti**oluşturmak için yönergeleri izleyin. Bu öğreticide, "Merhaba Worldapplication" adlı uygulamayı ve "HelloWorld" hizmetini adlandırın. Sonuç, `HelloWorldApplication` ve `HelloWorld`için dizinleri içerir.

```bash
HelloWorldApplication/
├── build.gradle
├── HelloWorld
│   ├── build.gradle
│   └── src
│       └── statelessservice
│           ├── HelloWorldServiceHost.java
│           └── HelloWorldService.java
├── HelloWorldApplication
│   ├── ApplicationManifest.xml
│   └── HelloWorldPkg
│       ├── Code
│       │   ├── entryPoint.sh
│       │   └── _readme.txt
│       ├── Config
│       │   └── _readme.txt
│       ├── Data
│       │   └── _readme.txt
│       └── ServiceManifest.xml
├── install.sh
├── settings.gradle
└── uninstall.sh
```
### <a name="service-registration"></a>Hizmet kaydı
Hizmet türlerinin Service Fabric çalışma zamanına kayıtlı olması gerekir. Hizmet türü, `ServiceManifest.xml` ve ' i uygulayan `StatelessService`hizmet sınıfınız içinde tanımlanır. Hizmet kaydı, işlem ana giriş noktasında gerçekleştirilir. Bu örnekte, işlem ana giriş noktası şu şekilde olur `HelloWorldServiceHost.java`:

```java
public static void main(String[] args) throws Exception {
    try {
        ServiceRuntime.registerStatelessServiceAsync("HelloWorldType", (context) -> new HelloWorldService(), Duration.ofSeconds(10));
        logger.log(Level.INFO, "Registered stateless service type HelloWorldType.");
        Thread.sleep(Long.MAX_VALUE);
    }
    catch (Exception ex) {
        logger.log(Level.SEVERE, "Exception in registration:", ex);
        throw ex;
    }
}
```

## <a name="implement-the-service"></a>Hizmeti uygulama

**Merhaba Worldapplication/HelloWorld/src/statelessservice/HelloWorldService. Java**' yı açın. Bu sınıf, hizmet türünü tanımlar ve herhangi bir kodu çalıştırabilir. Service API 'SI, kodunuz için iki giriş noktası sağlar:

* Uzun süre çalışan işlem iş yükleri dahil olmak üzere `runAsync()`herhangi bir iş yükünü yürütmeye başlayabileceğiniz açık uçlu bir giriş noktası yöntemi.

```java
@Override
protected CompletableFuture<?> runAsync(CancellationToken cancellationToken) {
    ...
}
```

* Seçtiğiniz iletişim yığınınızı ekleyebileceğiniz bir iletişim giriş noktası. Bu, kullanıcılardan ve diğer hizmetlerden istek almaya başlayabileceğiniz yerdir.

```java
@Override
protected List<ServiceInstanceListener> createServiceInstanceListeners() {
    ...
}
```

Bu öğretici, `runAsync()` giriş noktası yöntemine odaklanır. Bu, kodunuzun hemen çalıştırılmasına başlayabileceğiniz yerdir.

### <a name="runasync"></a>RunAsync
Bir hizmetin bir örneği yerleştirildiğinde ve yürütülmeye hazırlanıyorsa, platform bu yöntemi çağırır. Durum bilgisi olmayan bir hizmet için, hizmet örneğinin açıldığı anlamına gelir. Hizmet örneğinizin kapatılması gerektiğinde koordine etmek için bir iptal belirteci sağlanır. Service Fabric, bir hizmet örneğinin bu açık/kapalı döngüsü, hizmetin kullanım ömrü boyunca bir bütün olarak çok fazla zaman alabilir. Bu, aşağıdakiler dahil çeşitli nedenlerden kaynaklanabilir:

* Sistem, hizmet örneklerinizi kaynak Dengeleme için taşıdıkça.
* Hatalar kodunuzda oluşur.
* Uygulama veya sistem yükseltilir.
* Temel alınan donanım bir kesinti yaşıyorsa.

Bu düzenleme, hizmetinizi yüksek oranda kullanılabilir ve düzgün şekilde dengeli tutmak için Service Fabric tarafından yönetilir.

`runAsync()`zaman uyumlu olarak engellenmemelidir. RunAsync uygulamanız, çalışma zamanının devam etmesine izin vermek için bir tamamlayıcı Tablefuture döndürmelidir. İş yükünüzün, bir uzun süre çalışan bir görevi, tamamlanmalı ve bu görevin tamamlanması gerekir.

#### <a name="cancellation"></a>İptal
İş yükünüzün iptali, belirtilen iptal belirteci tarafından düzenlenen bir işbirliği çabadır. Sistem, başlamadan önce görevin bitmesini bekler (başarılı tamamlama, iptal veya hata ile). İptal belirtecini dikkate almak, tüm işleri tamamlamak ve sistem iptali istediğinde mümkün olduğunca hızlı `runAsync()` bir şekilde çıkmak önemlidir. Aşağıdaki örnek, bir iptal olayının nasıl işleneceğini göstermektedir:

```java
@Override
protected CompletableFuture<?> runAsync(CancellationToken cancellationToken) {

    // TODO: Replace the following sample code with your own logic
    // or remove this runAsync override if it's not needed in your service.

    return CompletableFuture.runAsync(() -> {
        long iterations = 0;
        while(true)
        {
        cancellationToken.throwIfCancellationRequested();
        logger.log(Level.INFO, "Working-{0}", ++iterations);

        try {
            Thread.sleep(1000);
        } catch (InterruptedException ex){}
        }
    });
}
```

Bu durum bilgisi olmayan hizmet örneğinde, sayı yerel bir değişkende depolanır. Ancak bu durum bilgisiz olmayan bir hizmet olduğundan, depolanan değer yalnızca hizmet örneğinin geçerli yaşam döngüsü için mevcut olur. Hizmet taşınsa veya yeniden başlatıldığında, değer kaybedilir.

## <a name="create-a-stateful-service"></a>Durum bilgisi olan hizmet oluşturma
Service Fabric, durum bilgisi olan yeni bir hizmet türü sunar. Durum bilgisi olan bir hizmet, hizmetin kendisi içinde güvenilir bir şekilde durumunu koruyabilir ve onu kullanan kodla birlikte bulunur. Durum, bir dış depoya durumunun kalıcı hale getirilmesi gerekmeden Service Fabric tarafından yüksek oranda kullanılabilir hale getirilir.

Bir sayaç değerini durum bilgisiz durumundan yüksek oranda kullanılabilir ve kalıcı olarak dönüştürmek için, hizmet taşınsa veya yeniden başlatıldığında bile, durum bilgisi olan bir hizmete ihtiyacınız vardır.

HelloWorld uygulamasıyla aynı dizinde `yo azuresfjava:AddService` komutunu çalıştırarak yeni bir hizmet ekleyebilirsiniz. Çerçeveinizde "güvenilir durum bilgisi olan hizmet" seçeneğini belirleyin ve hizmeti "Merhaba Dünya durum bilgisi" olarak adlandırın. 

Uygulamanızın Şu anda iki hizmeti olmalıdır: durum bilgisi olmayan hizmet HelloWorld ve durum bilgisi olan hizmet Merhaba Dünya durum bilgisi.

Durum bilgisi olan bir hizmet, durum bilgisi olmayan bir hizmetle aynı giriş noktalarına sahiptir. Temel fark, durumu güvenilir bir şekilde depolayabilen bir durum sağlayıcısının kullanılabilirliğinden oluşur. Service Fabric, güvenilir koleksiyonlar adlı bir durum sağlayıcısı uygulamasıyla birlikte gelir, bu da güvenilir durum Yöneticisi aracılığıyla çoğaltılan veri yapıları oluşturmanızı sağlar. Durum bilgisi olan güvenilir bir hizmet, varsayılan olarak bu durum sağlayıcısını kullanır.

Aşağıdaki RunAsync metodunu içeren **Merhaba Dünya durum bilgisi olan > src**Içinde helloworldstateful. Java açın:

```java
@Override
protected CompletableFuture<?> runAsync(CancellationToken cancellationToken) {
    Transaction tx = stateManager.createTransaction();
    return this.stateManager.<String, Long>getOrAddReliableHashMapAsync("myHashMap").thenCompose((map) -> {
        return map.computeAsync(tx, "counter", (k, v) -> {
            if (v == null)
                return 1L;
            else
                return ++v;
            }, Duration.ofSeconds(4), cancellationToken)
                .thenCompose((r) -> tx.commitAsync())
                .whenComplete((r, e) -> {
            try {
                tx.close();
            } catch (Exception e) {
                logger.log(Level.SEVERE, e.getMessage());
            }
        });
    });
}
```

### <a name="runasync"></a>RunAsync
`RunAsync()`durum bilgisiz ve durum bilgisi içermeyen hizmetlerde benzer şekilde çalışır. Ancak, durum bilgisi olan bir hizmette, platform yürütmeden `RunAsync()`önce sizin adınıza ek iş gerçekleştirir. Bu iş, güvenilir durum Yöneticisi ve güvenilir koleksiyonların kullanıma hazırlandığından emin olabilir.

### <a name="reliable-collections-and-the-reliable-state-manager"></a>Güvenilir koleksiyonlar ve güvenilir durum Yöneticisi
```java
ReliableHashMap<String,Long> map = this.stateManager.<String, Long>getOrAddReliableHashMapAsync("myHashMap")
```

[Reliablehashmap](https://docs.microsoft.com/java/api/microsoft.servicefabric.data.collections.reliablehashmap) , durumu hizmette güvenilir bir şekilde depolamak için kullanabileceğiniz bir sözlük uygulamasıdır. Service Fabric ve güvenilir diyez eşlemeleriyle, verileri bir dış kalıcı depoya gerek duymadan doğrudan hizmetinize kaydedebilirsiniz. Güvenilir HashMaps, verilerinizi yüksek oranda kullanılabilir hale getirir. Service Fabric, hizmetinizin birden fazla *çoğaltmasını* oluşturup yöneterek bunu gerçekleştirir. Ayrıca, bu çoğaltmaları ve bunların durum geçişlerini yönetmenin karmaşıklıklarını soyutlayan bir API sağlar.

Güvenilir koleksiyonlar, özel türleriniz dahil olmak üzere herhangi bir Java türünü, birkaç uyarılarla saklayabilir:

* Service Fabric, durumları düğümler arasında *çoğaltarak* ve güvenilir HashMap verilerinizi her çoğaltmada yerel diske depolar. Bu, güvenilir diyez eşlemelerinde depolanan her şeyin *seri hale getirilebilir*olması gerektiği anlamına gelir. 
* İşlemleri güvenilir diyez eşlemelerde gerçekleştirdiğinizde, nesneler yüksek kullanılabilirlik için çoğaltılır. Güvenilir Karmaeşlemlerde depolanan nesneler, hizmetinizdeki yerel bellekte tutulur. Bu, nesnesine yerel bir başvurunuz olduğu anlamına gelir.
  
   Bir işlemdeki güvenilir koleksiyonda bir güncelleştirme işlemi gerçekleştirmeden, bu nesnelerin yerel örneklerini muanmamak önemlidir. Bunun nedeni, yerel nesne örneklerine yapılan değişikliklerin otomatik olarak çoğaltılmaması olabilir. Nesneyi sözlüğe yeniden eklemeniz veya sözlükteki *güncelleştirme* yöntemlerinden birini kullanmanız gerekir.

Güvenilir durum Yöneticisi, güvenilir diyez haritalarını sizin için yönetir. Güvenilir durum Yöneticisi 'Ne dilediğiniz zaman ve hizmetinize dilediğiniz zaman güvenilir bir koleksiyon adı verebilirsiniz. Güvenilir durum Yöneticisi bir başvuru geri almanızı sağlar. Sınıf üyesi değişkenlerinde veya özelliklerde güvenilir koleksiyon örneklerine başvuruları kaydetmenizi önermiyoruz. Başvurunun hizmet yaşam döngüsünün her zaman bir örneğe ayarlandığından emin olmak için özel dikkatli olunmalıdır. Güvenilir durum Yöneticisi bu çalışmayı sizin için işler ve yineleme ziyaretleri için en iyi duruma getirilmiştir.


### <a name="transactional-and-asynchronous-operations"></a>İşlem ve zaman uyumsuz işlemler
```java
return map.computeAsync(tx, "counter", (k, v) -> {
    if (v == null)
        return 1L;
    else
        return ++v;
    }, Duration.ofSeconds(4), cancellationToken)
        .thenCompose((r) -> tx.commitAsync())
        .whenComplete((r, e) -> {
    try {
        tx.close();
    } catch (Exception e) {
        logger.log(Level.SEVERE, e.getMessage());
    }
});
```

Güvenilir HashMaps üzerinde işlemler zaman uyumsuzdur. Bunun nedeni, güvenilir Koleksiyonlar içeren yazma işlemlerinin verileri diske çoğaltmak ve diskte kalıcı hale getirmek için g/ç işlemleri gerçekleştirmesini sağlar.

Güvenilir HashMap işlemleri *işlem*halinde olduğundan, durumu birden çok güvenilir hashmaps ve işlem arasında tutarlı tutabilmenizi sağlayabilirsiniz. Örneğin, bir güvenilir sözlükten bir iş öğesi alabilir, üzerinde bir işlem gerçekleştirebilir ve sonucu tek bir işlem içinde başka bir güvenilir diyez eşlemesiyle kaydedebilirsiniz. Bu bir atomik işlem olarak değerlendirilir ve tüm işlemin başarılı olacağını veya işlemin tamamının geri alınmayacağını garanti eder. Öğeyi sıradan çıktıktan sonra bir hata oluşursa, ancak sonucu kaydetmeden önce, tüm işlem geri alınır ve öğe işlenmek üzere kuyrukta kalır.


## <a name="build-the-application"></a>Uygulama oluşturma

Yeumman scafkatlaması, uygulamayı dağıtmak ve kaldırmak üzere uygulamayı ve Bash betiklerini derlemek için bir Gradle betiği içerir. Uygulamayı çalıştırmak için önce Gradle ile uygulamayı derleyin:

```bash
$ gradle
```

Bu, Service Fabric CLı kullanılarak dağıtılabilecek bir Service Fabric uygulama paketi oluşturur.

## <a name="deploy-the-application"></a>Uygulamayı dağıtma

Uygulama oluşturulduktan sonra uygulamayı yerel kümeye dağıtabilirsiniz.

1. Yerel Service Fabric kümesine bağlanın.

    ```bash
    sfctl cluster select --endpoint http://localhost:19080
    ```

2. Uygulama paketini kümenin görüntü deposuna kopyalamak, uygulama türünü kaydetmek ve uygulamanın bir örneğini oluşturmak için şablonda verilen yükleme betiğini çalıştırın.

    ```bash
    ./install.sh
    ```

Oluşturulan uygulamayı dağıtma işlemi, diğer tüm Service Fabric uygulamalarında olduğu gibidir. Ayrıntılı yönergeler için [Service Fabric uygulamasını Service Fabric CLI ile yönetme](service-fabric-application-lifecycle-sfctl.md) ile ilgili belgelere bakın.

Bu komutların parametreleri, uygulama paketi içinde oluşturulmuş bildirimlerde bulunabilir.

Uygulama dağıtıldığında bir tarayıcı açın ve `http://localhost:19080/Explorer` konumundaki [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md)'a gidin. Ardından, **Uygulamalar** düğümünü genişletin ve geçerli olarak uygulamanızın türü için bir giriş ve bu türün ilk örneği için başka bir giriş olduğuna dikkat edin.

> [!IMPORTANT]
> Uygulamayı Azure 'da güvenli bir Linux kümesine dağıtmak için, Service Fabric çalışma zamanı ile uygulamanızı doğrulamak üzere bir sertifika yapılandırmanız gerekir. Bunun yapılması, Reliable Services hizmetlerinizin temel alınan Service Fabric çalışma zamanı API 'Leriyle iletişim kurmasını sağlar. Daha fazla bilgi edinmek için bkz. [Reliable Services uygulamasını Linux kümelerinde çalışacak şekilde yapılandırma](./service-fabric-configure-certificates-linux.md#configure-a-reliable-services-app-to-run-on-linux-clusters).  
>

## <a name="next-steps"></a>Sonraki adımlar

* [Service Fabric CLI ile çalışmaya başlama](service-fabric-cli.md)
