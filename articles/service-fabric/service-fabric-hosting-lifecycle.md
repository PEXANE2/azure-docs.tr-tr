---
title: Azure Service Fabric etkinleştirme ve devre dışı bırakma yaşam döngüsü
description: Bir uygulama ve bir düğümdeki ServicePackage yaşam döngüsü hakkında bilgi edinin.
author: tugup
ms.topic: conceptual
ms.date: 05/01/2020
ms.author: tugup
ms.openlocfilehash: d8585d0b39e4a4ef9cf77f40ea878ddb47bcb0de
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "97831831"
---
# <a name="azure-service-fabric-hosting-life-cycle"></a>Azure Service Fabric barındırma yaşam döngüsü

Bu makalede, bir uygulama bir düğümde etkinleştirildiğinde Azure Service Fabric gerçekleşen olaylara genel bir bakış sağlanır. Davranışı denetleyen çeşitli küme yapılandırmasını açıklar.

Devam etmeden önce, [Service Fabric bir uygulama modelleme][a1]bölümünde açıklanan kavramları ve ilişkileri anladığınızdan emin olun. 

> [!NOTE]
> Bu makale, bazı terminolojiyi özelleştirilmiş yollarla kullanır. Aksi belirtilmediği takdirde:
>
> - *Çoğaltma* , durum bilgisi olan bir hizmetin bir çoğaltmasını ve durum bilgisi olmayan bir hizmetin örneğini ifade eder.
> - *CodePackage* , bir serviceType kayıt yapan bir ServiceHost işlemine eşdeğer olarak değerlendirilir. Bu ServiceType hizmet çoğaltmalarını barındırır.
>

## <a name="activate-an-applicationpackage-or-servicepackage"></a>ApplicationPackage veya ServicePackage 'i etkinleştirme

Bir ApplicationPackage veya ServicePackage 'i etkinleştirmek için:

1. ApplicationPackage 'i indirin (örneğin, *ApplicationManifest.xml*).
2. Uygulama için bir ortam ayarlayın. Adımlar, örneğin, Kullanıcı oluşturma ' yı içerir.
3. Uygulamayı devre dışı bırakma için izlemeye başlayın.
4. ServicePackage 'i (örneğin, *ServiceManifest.xml*, kod, yapılandırma dosyaları ve veri paketleri) indirin.
5. ServicePackage için bir ortam ayarlayın. Adımlar, örneğin, bir güvenlik duvarı ayarlamayı ve uç noktalar için bağlantı noktaları ayırmayı içerir.
6. Devre dışı bırakma için ServicePackage 'i izlemeye başlayın.
7. CodePackages için SetupEntryPoint ' u başlatın ve bitmesini bekleyin.
8. CodePackages için MainEntryPoint başlatın.

### <a name="servicetype-blocklisting"></a>ServiceType blok listeleme
`ServiceTypeDisableFailureThreshold` izin verilen etkinleştirme, indirme ve kod paketi hatalarının sayısını belirler. Eşiğe ulaşıldığında, ServiceType blok listesi için zamanlanır. İlk etkinleştirme hatası, indirme hatası veya CodePackage kilitlenme, ServiceType blok listesini zamanlar. 

`ServiceTypeDisableGraceInterval`Yapılandırma, düğüm üzerinde serviceType silinmeden önce yetkisiz kullanım aralığını belirler. Bu işlem yürütülürken, etkinleştirme, indirme ve CodePackage yeniden başlatması paralel olarak yeniden denenir. Yeniden deneme anlamına gelir, örneğin, kod paketinin kilitlenmeden sonra tekrar başlamak üzere zamanlandığı veya Service Fabric paketleri yeniden indirmeyi deneyecek.

ServiceType bir blok listelendiğinde, bir sistem durumu hatası görürsünüz: `'System.Hosting' reported Error for property 'ServiceTypeRegistration:ServiceType'. The ServiceType was disabled on the node.`

Aşağıdaki olaylardan herhangi biri gerçekleştiğinde, bu,, düğümde yeniden etkinleştirilir:
- Etkinleştirme başarılı oldu. Ya da başarısız olursa, `ActivationMaxFailureCount` en fazla yeniden deneme sayısına ulaşır.
- İndirme başarılı oldu. Ya da başarısız olursa, `DeploymentMaxFailureCount` en fazla yeniden deneme sayısına ulaşır.
- Kilitlenen bir CodePackage başlatılır ve ServiceType başarıyla kaydedilir.

`ActivationMaxFailureCount` ve `DeploymentMaxFailureCount` bir düğümdeki uygulamayı etkinleştirmek veya indirmek için Service Fabric en fazla deneme sayısıdır. En yüksek sınıra ulaşıldığında Service Fabric, ServiceType etkinleştirme için yeniden etkinleştirilir. 

Bu eşikler, hizmete etkinleştirme için başka bir fırsat sağlar. Etkinleştirme başarılı olursa sorun otomatik olarak düzeltilmeye devam edilir. 

Yeni yerleştirilmiş veya etkinleştirilmiş bir çoğaltma, yeni bir etkinleştirme veya indirme işlemi tetikleyebilir. Bu eylem başarılı olur ya da ServiceType blok listesini yeniden tetikler.

> [!NOTE]
> Bir serviceType kayıt olmayan bir kilitlenen kod paketi serviceType ' yı etkilemez. Yalnızca bir çoğaltma barındıran bir kilitlenen kod paketi serviceType 'ı etkiler.
>

### <a name="codepackage-crash"></a>CodePackage kilitlenmesi
Bir CodePackage kilitlenirse, Service Fabric yeniden başlatmak için bir geri alma kullanır. Geri alma, CodePackage Service Fabric çalışma zamanına sahip bir tür kaydettirip kaydolmadığına bakılmaksızın uygulanır.

Geri alma değeri `Min(RetryTime, ActivationMaxRetryInterval)` . Değer, yapılandırma ayarına bağlı olarak sabit, doğrusal veya üstel tutarlarda artırılır `ActivationRetryBackoffExponentiationBase` :

- **Sabit**: ise `ActivationRetryBackoffExponentiationBase == 0` , `RetryTime = ActivationRetryBackoffInterval` .
- **Doğrusal**: ise,,  `ActivationRetryBackoffExponentiationBase == 0` `RetryTime = ContinuousFailureCount ActivationRetryBackoffInterval` `ContinuousFailureCount` bir CodePackage 'in kaç kez çöktüğü veya etkinleştiremediği.
- **Üstel**: `RetryTime = (ActivationRetryBackoffInterval in seconds) * (ActivationRetryBackoffExponentiationBase ^ ContinuousFailureCount)` .
    
Değerleri değiştirerek davranışı kontrol edebilirsiniz. Örneğin, birkaç hızlı yeniden başlatma denemesini isterseniz, olarak ayarlayarak ve ayarını yaparak doğrusal miktarları kullanabilirsiniz `ActivationRetryBackoffExponentiationBase` `0` `ActivationRetryBackoffInterval` `10` . Bu nedenle, bir CodePackage kilitlenirse başlangıç aralığı 10 saniye sonra olur. Paket kilitlenmeye devam ederse, CodePackage etkinleştirmesi başarılı olana veya CodePackage devre dışı bırakılıncaya kadar, geri alma işlemi 20 saniye, 30 saniye, 40 saniye vb. olarak değişir. 
    
Bir hatadan sonra Service Fabric yedeklediği (yani, bekleyeceği) en uzun süre `ActivationMaxRetryInterval` .
    
CodePackage kilitlenirse ve geri geliyorsa, tarafından belirtilen zaman dilimine açık kalması gerekir `CodePackageContinuousExitFailureResetInterval` . Bu aralıktan sonra, Service Fabric CodePackage sağlıklı olduğunu varsayar. Service Fabric daha sonra önceki hata sağlık raporunun üzerine yazar ve bunu sıfırlar `ContinuousFailureCount` .

### <a name="codepackage-not-registering-servicetype"></a>CodePackage, ServiceType kayıt değil
Bir CodePackage çalışır durumda kalırsa ve bir ServiceType Kaydolması bekleniyorsa, Service Fabric sonrasında bir uyarı sistem durumu raporu oluşturur `ServiceTypeRegistrationTimeout` . Rapor, ServiceType beklenen süre içinde kaydedilmemiş olduğunu gösterir.

### <a name="activation-failure"></a>Etkinleştirme hatası
Service Fabric etkinleştirme sırasında bir hata bulduğunda, kod paketi kilitlenmesiyle birlikte her zaman doğrusal bir geri alma kullanır. Etkinleştirme işlemi şu aralıklarda yeniden denemeler sağlar: (0 + 10 + 20 + 30 + 40) = 100 saniye. (İlk yeniden deneme anında gerçekleşir.) Bu sıra sonrasında, etkinleştirme yeniden denenmez.
    
En fazla etkinleştirme geri alma olabilir `ActivationMaxRetryInterval` . Yeniden deneme olabilir `ActivationMaxFailureCount` .

### <a name="download-failure"></a>İndirme hatası
Service Fabric, indirme sırasında bir hata bulduğunda her zaman doğrusal bir geri alma kullanır. Etkinleştirme işlemi şu aralıklarda yeniden denemeler sağlar: (0 + 10 + 20 + 30 + 40) = 100 saniye. (İlk yeniden deneme anında gerçekleşir.) Bu sıra sonrasında indirme yeniden denenmez. 

İndirme için doğrusal geri alma, değerine eşittir `ContinuousFailureCount`  *  `DeploymentRetryBackoffInterval` . Maksimum geri alma olabilir `DeploymentMaxRetryInterval` . Etkinleştirmeler gibi indirme işlemleri de sınır için yeniden deneyebilir `ActivationMaxFailureCount` .

> [!NOTE]
> Bu ayarları değiştirmeden önce aşağıdaki örnekleri göz önünde bulundurun:
>
>* CodePackage kilitlenme ve yedeklemeyi devre dışı bırakır, ServiceType devre dışı bırakılır. Ancak etkinleştirme yapılandırmasında hızlı bir yeniden başlatma işlemi varsa, ServiceType aslında silinmeden önce CodePackage birkaç kez kullanılabilir. 
>
>    Örneğin, CodePackage 'in geldiğini, ServiceType Service Fabric kaydettirir ve sonra kilitlenmelerini söyleyin. Bu durumda, barındırma sonrasında bir tür kaydı alırsa, `ServiceTypeDisableGraceInterval` Dönem iptal edilir. Bu işlem, CodePackage, dönemden daha büyük bir değere kapatılana kadar yinelenir `ServiceTypeDisableGraceInterval` . Ardından, bu ServiceType düğüm üzerinde bir blok silinir. ServiceType blok listesi beklediğinizden uzun sürebilir.
>
>* Etkinleştirmeler söz konusu olduğunda, bir Service Fabric sistemi bir düğüme bir çoğaltma yerleştirdiğinde, yeniden yapılandırma Aracısı uygulamayı etkinleştirmek için barındırma alt sistemini ister. Etkinleştirme isteğini 15 dakikada bir yeniden dener. (Süre, `RAPMessageRetryInterval` yapılandırma ayarına tabidir.) Service Fabric, barındırma içindeki etkinleştirme işlemi yeniden deneme aralığından ve ' den daha uzun bir süre boyunca kullanılmadığı takdirde, ServiceType 'in listelendiğinden emin olamaz `ServiceTypeDisableGraceInterval` . 
>
>    Örneğin, kümenin `ActivationMaxFailureCount` 5 olarak ayarlandığını ve `ActivationRetryBackoffInterval` 1 saniyelik olarak ayarlandığını varsayalım. Bu durumda, etkinleştirme işlemi zaman aralıklarıyla (0 + 1 + 2 + 3 + 4) = 10 saniye sürer. (İlk yeniden deneme anında gerçekleşir.) Bu sıra sonrasında barındırma, yeniden denemeyi sağlar. Etkinleştirme işlemi tamamlanır ve 15 saniye sonra yeniden denenmez. 
>
>    Service Fabric, 15 saniye içinde tüm izin verilen yeniden denemeler tükendi. Bu nedenle yeniden yapılandırma aracısından her yeniden deneme barındırma alt sisteminde yeni bir etkinleştirme işlemi oluşturur ve bu model yinelenen olarak kalır. Sonuç olarak, ServiceType düğüm üzerinde hiçbir şekilde listelenmez. ServiceType düğüm üzerinde bir blok olmadığından, çoğaltma farklı bir düğümde taşınmaz ve denenmez.
> 

## <a name="deactivation"></a>Bırakmayı

Bir düğümde bir ServicePackage etkinleştirildiğinde, devre dışı bırakma için izlenir. Devre dışı bırakma işlemi iki şekilde geçerlidir:

- **Düzenli olarak devre dışı bırakma**: `DeactivationScanInterval` sistem, her *zaman* bir çoğaltma barındırılmayan hizmet paketlerini denetler ve devre dışı bırakma için aday olarak işaretler.
- **Replicaclose devre dışı bırakma**: bir çoğaltma kapalıysa, Etkinleştirici bir alır `DecrementUsageCount` . Bir sayı, ServicePackage herhangi bir çoğaltmayı barındırmazsa 0 ' dır. bu nedenle, ServicePackage devre dışı bırakma için bir adaydır.

Etkinleştirme modu, adayların devre dışı bırakma için ne zaman zamanlandığını belirler. Paylaşılan modda, devre dışı bırakma adayları öğesinden sonra zamanlanır `DeactivationGraceInterval` . Özel modda, öğesinden sonra zamanlanır `ExclusiveModeDeactivationGraceInterval` . Bu saatler arasında yeni bir çoğaltma yerleştirmesi gelirse, devre dışı bırakma işlemi iptal edilir. 

Daha fazla bilgi için bkz. [özel mod ve paylaşılan mod][a2].

### <a name="periodic-deactivation"></a>Düzenli olarak devre dışı bırakma
Düzenli olarak devre dışı bırakma örnekleri aşağıda verilmiştir:

* **Örnek 1**: Etkinleştirici, zaman anında tarama (' a `DeactivationScanInterval` ) başlatır. Sonraki tarama 2T konumunda olacak. T + 1 ' de bir ServicePackage etkinleştirmenin gerçekleştiğini varsayalım. Bu ServicePackage bir çoğaltma barındırmıyor, bu yüzden devre dışı bırakılması gerekiyor. 

    Devre dışı bırakma için bir aday olması için, ServicePackage en az T zamanı için hiçbir çoğaltma barındırması gerekir. Devre dışı bırakma için 2T + 1 ' de uygun olacaktır. Bu nedenle, 2. tarama devre dışı bırakma için bir aday olarak bu ServicePackage 'i tanımlamaz. 

    Sonraki devre dışı bırakma çevrimi, bu hizmet paketini devre dışı bırakma için zamanlayamıyor çünkü artık paket, zaman T için çoğaltma dışı durumda.  

* **Örnek 2**: bir servicepackage 'in T-1 zamanında etkinleştirildiğini ve devre dışı Etkinleştirici, t 'de bir tarama başlattığı söylayalım. ServicePackage bir çoğaltma barındırmıyor. 2. bir sonraki taramada, ServicePackage devre dışı bırakma için bir aday olarak tanımlanır, bu nedenle devre dışı bırakma işlemi için zamanlanır.  

* **Örnek 3**: t – 1 ' de bir servicepackage etkinleştirileceğini ve devre dışı Etkinleştirici, t 'de bir tarama başlatır. ServicePackage henüz bir çoğaltmayı barındırmıyor. Şimdi T + 1 ' de bir çoğaltma yerleştirilir. Diğer bir deyişle, barındırma bir `IncrementUsageCount` , bir çoğaltmanın oluşturulduğu anlamına gelir. 

    Bu ServicePackage, 2T 'de devre dışı bırakma için zamanlanmayacaktır. Paket bir çoğaltma içerdiğinden, devre dışı bırakma işlemi, bu makaledeki sonraki bölümde anlatılan ReplicaClose mantığını izlemelidir.

* **Örnek 4**: servicepackage 'ın 10 GB olduğunu varsayalım. Paket büyükse, düğüm üzerinde indirilmek zaman alır. Bir uygulama etkinleştirildiğinde, Etkinleştirici, kendi yaşam döngüsünü izler. , `DeactivationScanInterval` Küçük bir değere ayarlanırsa, hizmet paketinizin indirilmek üzere geçen süre nedeniyle düğümde etkinleştirilecek zaman olmayabilir. Bu sorunu aşmak için, [hizmet paketini düğüm üzerinde önceden indirebilir][p1] veya artırabilirsiniz `DeactivationScanInterval` . 

> [!NOTE]
> Bir ServicePackage, ( `DeactivationScanInterval` 2 *) + arasında her yerde devre dışı bırakılabilir `DeactivationScanInterval` `DeactivationGraceInterval` / `ExclusiveModeDeactivationGraceInterval` . 
>

### <a name="replicaclose-deactivation"></a>ReplicaClose devre dışı bırakma

> [!NOTE]
> Bu bölüm aşağıdaki yapılandırma ayarlarını gösterir:
> - **DeactivationGraceInterval** / **ExclusiveModeDeactivationGraceInterval**: bir hizmet paketine, zaten herhangi bir çoğaltma barındırılıyorsa, başka bir çoğaltmayı barındırmak için verilen süre. 
> - **Deactivationscanınterval**: bir çoğaltmayı barındırmak Için bir servicepackage 'e verilen en kısa süre *, hiçbir çoğaltma barındırılmadıysa* , yani kullanılmadıysa.
>

Sistem, bir ServicePackage 'in tuttuğu çoğaltmaların sayısını tutar. Bir ServicePackage bir çoğaltmayı tutuyor ve çoğaltma kapatılabilir veya kapatılırsa, barındırma bir alır `DecrementUsageCount` . Bir çoğaltma açıldığında, barındırma bir alır `IncrementUsageCount` . 

Azalma, hizmet paketinin barındırdığı kopyaların bir çoğaltma tarafından azaldığını gösterir. Sayı 0 ' a düştüğünde, ServicePackage devre dışı bırakma için zamanlanır. Sonrasında devre dışı bırakılacak zaman `DeactivationGraceInterval` / `ExclusiveModeDeactivationGraceInterval` . 

Örneğin, T 'de bir azalış olduğunu ve ServicePackage 'in 2T + X () ' de devre dışı bırakılacak şekilde zamanlandığını varsayalım `DeactivationGraceInterval` / `ExclusiveModeDeactivationGraceInterval` . Bu süre boyunca, `IncrementUsage` bir çoğaltma oluşturulduğu için barındırma bir, devre dışı bırakma işlemi iptal edilir.

### <a name="ctrl--c"></a>Ctrl + C
Bir servicepackage bunu geçirir `DeactivationGraceInterval` / `ExclusiveModeDeactivationGraceInterval` ve yine de bir çoğaltma barındırmamışsa, devre dışı bırakma işlemi iptal edilemez. CodePackages bir CTRL + C işleyicisi alır. Bu nedenle, devre dışı bırakma işlem hattının işlemi bir daha aşağı getirmek için tamamlanmaları. 

Bu süre boyunca, aynı ServicePackage için yeni bir çoğaltma yerleştirilmeye çalışıyorsa, işlem devre dışı bırakma ' dan etkinleştirmeye geçiş yapamadığı için başarısız olur.

## <a name="cluster-configurations"></a>Küme yapılandırmaları

Bu bölümde, etkinleştirme ve devre dışı bırakmayı etkileyen varsayılanlar içeren konfigürasyonlar listelenir.

### <a name="servicetype"></a>ServiceType
- **Servicetypedisablefailurethreshold**: varsayılan: 1. Hata sayısı eşiği; Bu eşiğe ulaşıldığında, FailoverManager 'da hizmet türünü devre dışı bırakmak ve yerleştirme için farklı bir düğüm denemek üzere bir bildirim gönderilir.
- **ServiceTypeDisableGraceInterval**: varsayılan: 30 saniye. Hizmet türünün devre dışı bırakılabileceği zaman aralığı.
- **Servicetyperyumuristrationtimeout**: varsayılan: 300 saniye. ServiceType Service Fabric kayıt için zaman aşımı.

### <a name="activation"></a>Etkinleştirme
- **Activationretrybackoffınterval**: varsayılan: 10 saniye. Her etkinleştirme hatası için geri alma aralığı.
- **Activationmaxfailurecount**: varsayılan: 20. Sistemin, başarısız bir etkinleştirmeyi vermeden önce yeniden denemesini sağlayacak maksimum sayı. 
- **Activationretrybackoffüs Ationbase**: varsayılan: 1,5.
- **Activationmaxretryınterval**: varsayılan: 3.600 saniye. Hatalardan sonra etkinleştirme için maksimum geri deneme aralığı.
- **Codepackagecontinuousexitfailureresetınterval**: varsayılan: 300 saniye. CodePackage için sürekli çıkış hatası sayısını sıfırlama zaman aşımı aralığı.

### <a name="download"></a>İndir
- **Deploymentretrybackoffınterval**: varsayılan: 10. Dağıtım hatası için geri alma aralığı.
- **Deploymentmaxretryınterval**: varsayılan: 3.600 saniye. Hatalardan sonra dağıtım için maksimum geri alma aralığı.
- **Deploymentmaxfailurecount**: varsayılan: 20. Uygulama dağıtımı, `DeploymentMaxFailureCount` düğümdeki uygulamanın dağıtımı başarısız olmadan önce, zaman için yeniden denenecek.

### <a name="deactivation"></a>Bırakmayı
- **Deactivationscanınterval**: varsayılan: 600 saniye. Bir çoğaltmayı barındırmak için hizmet paketi 'ne verilen en kısa süre (kullanılmıyorsa, bu) bir çoğaltma barındırılmaz.
- **DeactivationGraceInterval**: varsayılan: 60 saniye. *Paylaşılan* bir işlem modelinde, bir hizmet paketine verilen süre, zaten bir çoğaltma barındırdıktan sonra başka bir çoğaltmayı yeniden barındırmalıdır.
- **ExclusiveModeDeactivationGraceInterval**: varsayılan: 1 saniye. *Özel* bir işlem modelinde, bir servicepackage 'e zaten bir çoğaltma barındırdıktan sonra başka bir çoğaltmayı yeniden barındırmak için verilen süre.

## <a name="next-steps"></a>Sonraki adımlar

- [Bir uygulamayı paketleyin][a3] ve dağıtıma başlamaya hazırlanın.
- PowerShell 'de [uygulamaları dağıtın ve kaldırın][a4] .

<!--Link references--In actual articles, you only need a single period before the slash-->
[a1]: service-fabric-application-model.md
[a2]: service-fabric-hosting-model.md
[a3]: service-fabric-package-apps.md
[a4]: service-fabric-deploy-remove-applications.md

[p1]: /powershell/module/servicefabric/copy-servicefabricservicepackagetonode
