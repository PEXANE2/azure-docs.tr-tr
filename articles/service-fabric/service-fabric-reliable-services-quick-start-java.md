---
title: Java'da ilk güvenilir hizmetinizi oluşturun
description: Durum disları ve durum ları kullanım durumu yla bir Microsoft Azure Hizmet Kumaşı uygulaması oluşturmaya giriş.
author: suhuruli
ms.topic: conceptual
ms.date: 11/02/2017
ms.author: suhuruli
ms.openlocfilehash: c3b301a7a9039f1fe8095950f0a5a4e23eb52a9b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75614222"
---
# <a name="get-started-with-reliable-services-in-java"></a>Java'da Güvenilir Hizmetlerle başlayın
> [!div class="op_single_selector"]
> * [Windows üzerinde C#](service-fabric-reliable-services-quick-start.md)
> * [Linux üzerinde Java](service-fabric-reliable-services-quick-start-java.md)
>
>

Bu makalede, Azure Hizmet Kumaşı Güvenilir Hizmetler'in temelleri açıklanmaktadır ve Java'da yazılmış basit bir Güvenilir Hizmet uygulaması oluşturma ve dağıtma konusunda size yol açmaktadır. 

## <a name="installation-and-setup"></a>Kurulum ve kurulum
Başlamadan önce, makinenizde Servis Kumaşı geliştirme ortamının ayarlandığınızdan emin olun.
Eğer kurmak gerekiyorsa, [Mac'te başlamak](service-fabric-get-started-mac.md) veya [Linux'ta başlamak](service-fabric-get-started-linux.md)için gidin.

## <a name="basic-concepts"></a>Temel kavramlar
Güvenilir Hizmetler'e başlamak için yalnızca birkaç temel kavramı anlamanız gerekir:

* **Hizmet türü**: Bu sizin hizmet uygulamanızdır. Yazdığınız sınıf tarafından tanımlanır `StatelessService` ve burada kullanılan diğer kod lar veya bağımlılıklar, bir ad ve sürüm numarası ile birlikte.
* **Adlandırılmış hizmet örneği**: Hizmetinizi çalıştırmak için, tıpkı bir sınıf türünün nesne örneklerini oluşturduğunuz gibi hizmet türünün adlandırılmış örneklerini oluşturursunuz. Hizmet örnekleri aslında yazdığınız hizmet sınıfının nesne anlık açıklamalarıdır.
* **Servis ana bilgisayarı**: Oluşturduğunuz adlandırılmış hizmet örneklerinin bir ana bilgisayar içinde çalışması gerekir. Hizmet ana bilgisayar, hizmet inizin örneklerinin çalıştırılabildiği bir işlemdir.
* **Hizmet kaydı**: Kayıt her şeyi bir araya getirir. Hizmet dokusunun çalışmasına izin vermek için hizmet türünün servis ana bilgisayarda Hizmet Kumaşı'na kaydedilmesi gerekir.  

## <a name="create-a-stateless-service"></a>Devletsiz hizmet oluşturma
Service Fabric uygulaması oluşturarak başlayın. Linux için Hizmet Kumaş SDK bir Yeoman jeneratör bir Hizmet Kumaş uygulaması için bir hizmet ile iskele sağlamak için içerir. Aşağıdaki Yeoman komutunu çalıştırarak başlayın:

```bash
$ yo azuresfjava
```

**Güvenilir Bir Stateless Hizmeti**oluşturmak için yönergeleri izleyin. Bu eğitim için, uygulama "HelloWorldApplication" ve hizmet "HelloWorld" adını. Sonuç için dizinler `HelloWorldApplication` `HelloWorld`içerir ve .

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
### <a name="service-registration"></a>Servis kaydı
Servis türleri Servis Kumaşı çalışma süresine kaydedilmelidir. Hizmet türü, uygulayan `ServiceManifest.xml` hizmet sınıfınızda `StatelessService`tanımlanır. Hizmet kaydı işlemi ana giriş noktasında gerçekleştirilir. Bu örnekte, işlem ana `HelloWorldServiceHost.java`giriş noktası:

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

## <a name="implement-the-service"></a>Hizmeti uygulayın

Açık **HelloWorldApplication/HelloWorld/src/statelessservice/HelloWorldService.java**. Bu sınıf hizmet türünü tanımlar ve herhangi bir kodu çalıştırabilir. Hizmet API'si kodunuz için iki giriş noktası sağlar:

* Uzun süren işlem iş yükleri de dahil olmak üzere tüm iş yüklerini yürütmeye başlayabileceğiniz açık uçlu giriş noktası yöntemi. `runAsync()`

```java
@Override
protected CompletableFuture<?> runAsync(CancellationToken cancellationToken) {
    ...
}
```

* Seçtiğiniz iletişim yığınını takabileceğiniz bir iletişim giriş noktası. Bu, kullanıcılardan ve diğer hizmetlerden istek almaya başlabildiğiniz yerdir.

```java
@Override
protected List<ServiceInstanceListener> createServiceInstanceListeners() {
    ...
}
```

Bu öğretici, giriş `runAsync()` noktası yöntemine odaklanır. Bu, kodunuzu hemen çalıştırmaya başlabildiğiniz yerdir.

### <a name="runasync"></a>RunAsync
Platform, bir hizmet örneği yerleştirildiğinde ve yürütülmeye hazır olduğunda bu yöntemi çağırır. Devletsiz bir hizmet için, hizmet örneği açıldığında anlamına gelir. Hizmet örneğinizin kapatılması gerektiğinde koordine etmek için bir iptal belirteci sağlanır. Hizmet Kumaşı'nda, bir hizmet örneğinin bu açma/kapama döngüsü, hizmetin bir bütün olarak kullanım ömrü boyunca birçok kez oluşabilir. Bu, aşağıdakiler de dahil olmak üzere çeşitli nedenlerle gerçekleşebilir:

* Sistem, kaynak dengeleme için hizmet örneklerinizi taşır.
* Kodunuzda hatalar oluşur.
* Uygulama veya sistem yükseltilir.
* Altta yatan donanım bir kesinti yle karşılar.

Bu orkestrasyon Service Fabric tarafından, hizmetinizi yüksek oranda kullanılabilir ve dengeli tutmak için yönetilir.

`runAsync()`senkronize olarak engellememelidir. runAsync uygulamanız, çalışma süresinin devam etmesine izin vermek için bir CompletableFuture döndürmelidir. İş yükünüzün, CompletableFuture içinde yapılması gereken uzun süren bir görevi uygulaması gerekiyorsa.

#### <a name="cancellation"></a>İptal
İş yükünüzün iptali, sağlanan iptal belirteci tarafından yönetilen bir işbirliği çabasıdır. Sistem, devam etmeden önce görevin bitmesini bekler (başarılı bir şekilde tamamlayarak, iptal ederek veya hata yaparak). İptal jetonuna uymak, herhangi bir işi bitirmek `runAsync()` ve sistem iptal talebinde de diğinde mümkün olan en kısa sürede çıkmak önemlidir. Aşağıdaki örnek, bir iptal olayının nasıl işleyeceğini gösterir:

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

Bu durumsuz hizmet örneğinde, sayım yerel bir değişkende depolanır. Ancak bu durum devletsiz bir hizmet olduğundan, depolanan değer yalnızca hizmet örneğinin geçerli yaşam döngüsü için vardır. Hizmet hareket ettiğinde veya yeniden başlatıldığında, değer kaybolur.

## <a name="create-a-stateful-service"></a>Devlet hizmeti oluşturma
Service Fabric, devlete ait yeni bir hizmet türünü sunar. Devlet hizmeti, hizmeti kullanan kodla birlikte bulunan, hizmetin kendi içinde durumu güvenilir bir şekilde koruyabilir. Durum, harici bir mağazaya durumu devam etmeye gerek kalmadan Service Fabric tarafından yüksek oranda kullanılabilir hale getirilir.

Hizmet hareket etse veya yeniden başlatıldığında bile, karşı değeri durum suzdan yüksek kullanılabilir ve kalıcı ya da kalıcı hale getirmek için, devlet hizmetine ihtiyacınız vardır.

HelloWorld uygulamasıyla aynı dizinde, komutu `yo azuresfjava:AddService` çalıştırarak yeni bir hizmet ekleyebilirsiniz. Çerçeveniz için "Güvenilir Devlet Hizmeti"ni seçin ve hizmete "HelloWorldStateful" adını verdi. 

Uygulamanız şimdi iki hizmet olmalıdır: devletsiz hizmet HelloWorld ve devlet hizmeti HelloWorldStateful.

Devlet hizmeti, devletsiz bir hizmetle aynı giriş noktalarına sahiptir. Temel fark, durumu güvenilir bir şekilde depolayabilen bir devlet sağlayıcısının kullanılabilirliğidir. Service Fabric, Güvenilir Durum Yöneticisi aracılığıyla çoğaltılan veri yapıları oluşturmanıza olanak tanıyan Güvenilir Koleksiyonlar adlı bir devlet sağlayıcı uygulamasıyla birlikte gelir. Duruma durumuna geçen Güvenilir Hizmet varsayılan olarak bu durum sağlayıcısını kullanır.

Open **HelloWorldStateful.java HelloWorldStateful -> src**, aşağıdaki RunAsync yöntemi ni içerir:

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
`RunAsync()`aynı şekilde devletli ve devletsiz hizmetlerde de çalışır. Ancak, devlet hizmetinde, platform yürütülmeden önce sizin `RunAsync()`adınıza ek iş gerçekleştirir. Bu çalışma, Güvenilir Devlet Yöneticisi ve Güvenilir Koleksiyonların kullanıma hazır olmasını sağlamayı içerebilir.

### <a name="reliable-collections-and-the-reliable-state-manager"></a>Güvenilir Koleksiyonlar ve Güvenilir Devlet Müdürü
```java
ReliableHashMap<String,Long> map = this.stateManager.<String, Long>getOrAddReliableHashMapAsync("myHashMap")
```

[ReliableHashMap,](https://docs.microsoft.com/java/api/microsoft.servicefabric.data.collections.reliablehashmap) hizmeti güvenilir bir şekilde depolamak için kullanabileceğiniz bir sözlük uygulamasıdır. Service Fabric ve Reliable HashMaps ile, harici kalıcı bir mağazaya gerek kalmadan verileri doğrudan hizmetinizde depolayabilirsiniz. Güvenilir HashMaps verilerinizi son derece kullanılabilir hale getirin. Service Fabric bunu, hizmetinizin birden çok *kopyasını* sizin için oluşturarak ve yöneterek gerçekleştirir. Ayrıca, bu yinelemeleri ve durum geçişlerini yönetme karmaşıklığını özetleyen bir API sağlar.

Güvenilir Koleksiyonlar, özel türleriniz de dahil olmak üzere herhangi bir Java türünü birkaç uyarıyla depolayabilir:

* Service Fabric, düğümler arasında durumu *çoğaltarak* durumunuzu yüksek oranda kullanılabilir hale getirir ve Güvenilir HashMap verilerinizi her yinelemede yerel diske saklar. Bu, Güvenilir HashMaps depolanan her şeyin *serileştirilebilir*olması gerektiği anlamına gelir. 
* Güvenilir HashMaps'te işlem yaptığınızda nesneler yüksek kullanılabilirlik için çoğaltılır. Güvenilir HashMaps'te depolanan nesneler hizmetinizde yerel bellekte tutulur. Bu, nesneye yerel bir başvurunuz olduğu anlamına gelir.
  
   Bir işlemdeki güvenilir koleksiyonda güncelleştirme işlemi gerçekleştirmeden bu nesnelerin yerel örneklerini mutasyona uğratmamanız önemlidir. Bunun nedeni, nesnelerin yerel örneklerindeki değişikliklerin otomatik olarak çoğaltılamamasıdır. Nesneyi sözlüğe yeniden eklemeniz veya sözlükteki *güncelleştirme* yöntemlerinden birini kullanmanız gerekir.

Güvenilir Devlet Yöneticisi sizin için Güvenilir HashMaps yönetir. Güvenilir Devlet Müdürü'nden, hizmetinizde istediğiniz zaman ve istediğiniz yerde ada göre güvenilir bir koleksiyon isteyebilirsiniz. Güvenilir Durum Yöneticisi, bir başvuruyu geri almanızı sağlar. Sınıf üye değişkenlerinde veya özelliklerinde güvenilir toplama örneklerine başvuruları kaydetmenizi önermiyoruz. Başvurunun hizmet yaşam döngüsünde her zaman bir örneğe ayarlandığından emin olmak için özel özen gerekir. Güvenilir Devlet Yöneticisi bu işi sizin için işler ve tekrar ziyaretler için optimize edicisidir.


### <a name="transactional-and-asynchronous-operations"></a>İşlemsel ve eşzamanlı işlemler
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

Güvenilir HashMaps operasyonları asynchronous vardır. Bunun nedeni, Güvenilir Koleksiyonlar ile yazma işlemleri çoğaltmak ve diske veri kalıcı gindirimi işlemleri gerçekleştirmek olmasıdır.

Güvenilir HashMap işlemleri *işlemseldir,* böylece birden çok Güvenilir HashMaps ve işlem arasında durumu tutarlı tutabilirsiniz. Örneğin, bir Güvenilir Sözlük'ten bir iş öğesi alabilir, üzerinde bir işlem gerçekleştirebilir ve sonucu tek bir işlem içinde başka bir Güvenilir Karma Harita'da kaydedebilirsiniz. Bu bir atomik işlem olarak kabul edilir ve ya tüm işlemin başarılı olacağını ya da tüm işlemin geri döneceğini garanti eder. Maddenin kuyruğunu çıkardıktan sonra ancak sonucu kaydetmeden önce bir hata oluşursa, tüm işlem geri alınır ve öğe işleme için sırada kalır.


## <a name="build-the-application"></a>Uygulama oluşturma

Yeoman iskeledağıtmak ve uygulamayı kaldırmak için uygulama ve bash komut oluşturmak için bir gradle komut içerir. Uygulamayı çalıştırmak için, önce uygulamayı gradle birlikte oluşturun:

```bash
$ gradle
```

Bu, Service Fabric CLI kullanılarak dağıtılabilen bir Service Fabric uygulama paketi üretir.

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

Uygulama dağıtıldığında bir tarayıcı açın ve [http://localhost:19080/Explorer](http://localhost:19080/Explorer) konumundaki [Service Fabric Explorer](service-fabric-visualizing-your-cluster.md)'a gidin. Ardından, **Uygulamalar** düğümünü genişletin ve geçerli olarak uygulamanızın türü için bir giriş ve bu türün ilk örneği için başka bir giriş olduğuna dikkat edin.

> [!IMPORTANT]
> Uygulamayı Azure'da güvenli bir Linux kümesine dağıtmak için, uygulamanızı Hizmet Kumaşı çalışma süresiyle doğrulamak için bir sertifika yapılandırmanız gerekir. Bunu yapmak, Güvenilir Hizmetler hizmetlerinizin temel Hizmet Dokusu çalışma zamanı API'leri ile iletişim kurmasını sağlar. Daha fazla bilgi için bkz: [Linux kümelerinde çalışacak güvenilir hizmetler uygulamasını yapılandırın.](./service-fabric-configure-certificates-linux.md#configure-a-reliable-services-app-to-run-on-linux-clusters)  
>

## <a name="next-steps"></a>Sonraki adımlar

* [Service Fabric CLI ile çalışmaya başlama](service-fabric-cli.md)
