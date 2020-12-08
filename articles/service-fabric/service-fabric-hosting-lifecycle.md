---
title: Azure Service Fabric etkinleştirme ve devre dışı bırakma yaşam döngüsü
description: Bir düğümdeki uygulama ve ServicePackage yaşam döngüsünü açıklar
author: tugup
ms.topic: conceptual
ms.date: 05/1/2020
ms.author: tugup
ms.openlocfilehash: f049b19703d37412d1ee1961aee6cb327efabe7c
ms.sourcegitcommit: 8b4b4e060c109a97d58e8f8df6f5d759f1ef12cf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/07/2020
ms.locfileid: "96779609"
---
# <a name="azure-service-fabric-hosting-lifecycle"></a>Azure Service Fabric barındırma yaşam döngüsü

Bu makalede, bir düğümde uygulama etkinleştirildiğinde Azure Service Fabric gerçekleşen olaylara genel bir bakış ve davranışı denetlemek için kullanılan çeşitli küme yapılandırmaları sunulmaktadır.

Devam etmeden önce, [Service Fabric bir uygulama modelleme][a1]bölümünde açıklanan çeşitli kavramları ve ilişkileri anladığınızdan emin olun. 

> [!NOTE]
> Bu makalede, açıkça başka bir şeyin anlamı olarak belirtilmedikçe:
>
> - *Çoğaltma* , durum bilgisi olan bir hizmetin bir çoğaltmasını ve durum bilgisi olmayan bir hizmetin örneğini ifade eder.
> - *CodePackage* , bir *serviceType* kayıt yapan bir *ServiceHost* Işlemine eşdeğer olarak değerlendirilir ve bu *serviceType* hizmet çoğaltmalarını barındırır.
>

## <a name="activation-of-applicationservicepackage"></a>Uygulama/hizmet paketini etkinleştirme

Etkinleştirme için işlem hattı aşağıdaki gibidir:

1. ApplicationPackage 'i indirin. Örneğin: ApplicationManifest.xml vb.
2. EX için uygulama için ortam ayarlama: Kullanıcı oluşturma vb.
3. Uygulamayı devre dışı bırakma için izlemeye başlayın.
4. ServicePackage 'i indirin. Örneğin: ServiceManifest.xml, kod, config, veri paketleri vb.
5. EX için hizmet paketi için ortam ayarla: Kurulum güvenlik duvarı, uç noktalar için bağlantı noktası ayırır.
6. Devre dışı bırakma için hizmet paketini izlemeye başla.
7. CodePackages SetupEntryPoint başlatın ve tamamlanmasını bekleyin.
8. Kod paketlerinin MainEntryPoint öğesini başlatın.

### <a name="servicetype-blocklisting"></a>ServiceType blok listeleme
**Servicetypedisablefailurethreshold** , serviceType blok listeleme için zamanlandıktan sonra başarısız olan başarısızlık sayısını (etkinleştirme, Indirme, CodePackage hatası) belirler. İlk etkinleştirme hatası, indirme hatası veya CodePackage kilitlenmesi, ServiceType blok listesini zamanlar. **ServiceTypeDisableGraceInterval** yapılandırması, serviceType bu düğümde blok olarak işaretlenme süresini belirler. Tüm bu işlemleri yaparken, etkinleştirme, indirme ve CodePackage yeniden başlatması paralel olarak yeniden denenir. Yeniden deneniyor, örneğin, bu CodePackage, kilitlenme sonrasında tekrar başlamak üzere zamanlanacaktır veya Service Fabric paketleri yeniden indirmeyi dener.

ServiceType bir blok listelendiğinde, ' ServiceTypeRegistration: ServiceType ' özelliği için "' System. Hosting ' hatası bildiren bir sistem durumu hatası görürsünüz. ServiceType düğüm üzerinde devre dışı bırakıldı. "

Aşağıdaki işlemlerden herhangi biri gerçekleştiğinde, ServiceType düğümde yeniden etkinleştirilecek:
- Etkinleştirme başarılı oldu veya hata sonrasında **Activationmaxfailurecount** yeniden denemeye ulaştı.
- İndirme başarılı veya hata sonrasında **Deploymentmaxfailurecount** yeniden denemeleri ulaştı.
- Kilitlenen bir CodePackage başlatılır ve ServiceType başarıyla kaydedilir.

**Activationmaxfailurecount** ve **deploymentmaxfailurecount** , bir Service Fabric düğümdeki uygulamayı etkinleştirmek/indirmek için Service Fabric izin verilen en fazla deneme sayısıdır. Bu, hizmete etkinleştirme için başka bir fırsat vermek, bu da sorunun otomatik olarak iyileştirilmesi anlamına gelir. Bir çoğaltmanın yerleştirilmesi ve etkinleştirilmesi tarafından tetiklenen yeni bir etkinleştirme/indirme işlemi, ServiceType blok listesini yeniden tetikleyebilir veya başarılı olabilir.

> [!NOTE]
> Bir ServiceType kayıt yaptırmayan CodePackage kilitlenmezse, ServiceType ' yı etkilemez. Yalnızca bir çoğaltma kilitlenmesinin barındırıldığı CodePackage, ServiceType ' yı etkileyecektir.
>

### <a name="codepackage-crash"></a>CodePackage kilitlenmesi
Bir CodePackage kilitlenirse, Service Fabric yeniden başlatmak için bir geri alma kullanır. Geri alma, kod paketinin Service Fabric çalışma zamanına sahip bir türü kaydettirip kaydetmediği bağımsızdır.

Geri alma değeri min (RetryTime, **Activationmaxretryınterval**) ve bu geri alma değeri, **Activationretrybackoffüs ationbase** yapılandırma ayarına bağlı olarak sabit, doğrusal veya üstel tutarlarda artırılır.

- Sabit: **Activationretrybackoffüs Ationbase** = = 0 Ise, RetryTime = **Activationretrybackoffınterval**;
- Doğrusal:  **Activationretrybackoffüs Ationbase** = = 0 Ise, RetryTime = ContinuousFailureCount * **Activationretrybackoffınterval** ; burada ContinousFailureCount, bir CodePackage 'in kaç kez çöktüğü veya etkinleştiremediği sayısıdır.
- Üstel: RetryTime = (saniye cinsinden **Activationretrybackoffınterval** ) * (**Activationretrybackoffüs ationbase** ^ ContinuousFailureCount);
    
Değerleri değiştirerek davranışı kontrol edebilirsiniz. Örneğin, birkaç hızlı yeniden başlatma girişimi isterseniz, **Activationretrybackoffüs Ationbase** değerini 0 olarak ayarlayıp **Activationretrybackoffınterval** değerini 10 olarak ayarlayarak doğrusal kullanabilirsiniz. Bu ayarlarla, bir CodePackage kilitlenirse başlangıç aralığı 10 saniye sonra olur. Paket kilitlenmeye devam ederseniz, CodePackage etkinleştirmesi başarılı olana veya kod paketi devre dışı bırakılıncaya kadar geri alma işlemi, 20, 30, 40 saniye vb. olarak değişir. 
    
Bir hatadan sonra, bir hatadan sonra geçen en uzun süre Service Fabric (bekler), **Activationmaxretryınterval** öğesine tabidir.
    
CodePackage çöktüyse ve geri geliyorsa, Service Fabric sağlıklı olarak düşüneceği için **Codepackagecontinuousexitfailureresetınterval** için bir sorun olması gerekir. bu noktada, önceki hata sağlık raporunun üzerine yazılacak ve ContinousFailureCount sıfırlanır.

### <a name="codepackage-not-registering-servicetype"></a>CodePackage, ServiceType kayıt değil
Bir CodePackage canlı kalırsa ve bir ServiceType 'yi bizimle kaydetmesi bekleniyorsa, Service Fabric, ServiceType ilgili bir süre içinde kayıtlı olmadığını söyleyen **Servicetyperegistrationtimeout** öğesinden sonra bir uyarı HealthReport oluşturacaktır.

### <a name="activation-failure"></a>Etkinleştirme hatası
Service Fabric, etkinleştirme sırasında hata bulduğunda her zaman doğrusal bir geri (CodePackage kilitlenmesiyle aynı) kullanır. Bu, etkinleştirme işleminin daha sonra (0 + 10 + 20 + 30 + 40) = 100 sn (ilk yeniden deneme anında) olarak verilecek anlamına gelir. Bu etkinleştirme yeniden denenmez.
    
En fazla etkinleştirme geri alma işlemi **Activationmaxretryınterval** olabilir ve **Activationmaxfailurecount** yeniden denenmelidir.

### <a name="download-failure"></a>İndirme hatası
Service Fabric, indirme sırasında hatayla karşılaştığında her zaman doğrusal bir arka kapatma kullanır. Bu, etkinleştirme işleminin daha sonra (0 + 10 + 20 + 30 + 40) = 100 sn (ilk yeniden deneme anında) olarak verilecek anlamına gelir. Bundan sonra indirme yeniden denenmez. İndirme için doğrusal geri alma, ContinuousFailureCount **_Deploymentretrybackoffınterval_* değerine eşittir ve **deploymentmaxretryınterval** için maksimum geri kapatılabilir. Etkinleştirmeler gibi, indirme işlemi **Activationmaxfailurecount** için yeniden deneyebilir.

> [!NOTE]
> Bu ayarları değiştirmeden önce göz önünde bulundurmanız gereken birkaç örnek aşağıda verilmiştir.

* CodePackage kilitlenme ve geri alma işlemlerini devam ederse, ServiceType devre dışı bırakılır. Ancak, etkinleştirme yapılandırması hızlı yeniden başlatmaya sahipse, ServiceType aslında silinmeden önce CodePackage birkaç kez kullanılabilir. For Ex: CodePackage 'in geldiğini varsayın, ServiceType Service Fabric kaydeder ve sonra çöker. Bu durumda, barındırma bir tür kaydı aldığında **ServiceTypeDisableGraceInterval** dönemi iptal edilir. Bu, CodePackage  **ServiceTypeDisableGraceInterval** ' den büyük bir değere ulaşana kadar ve bu durumda serviceType, düğüm üzerinde bir blok olarak değiştirilene kadar yinelenir. Bu, maytake ' den daha uzun bir süre içinde yer alabilir.

* Etkinleştirmeler söz konusu olduğunda, Service Fabric sisteminin bir düğüme bir çoğaltma eklemesi gerektiğinde RA (ReconfigurationAgent), uygulamayı etkinleştirmek ve etkinleştirme isteğini 15 saniyede bir ( **Rapmessageretryıınterval** yapılandırma ayarı ile yönetilir) yeniden dener. ServiceType 'in bir blok olarak listelendiğini Service Fabric için, barındırma içindeki etkinleştirme işleminin yeniden deneme aralığı ve **ServiceTypeDisableGraceInterval** kıyasla daha uzun bir süre için canlı olması gerekir. Örneğin: kümede **Activationmaxfailurecount** değerinin 5 olarak ayarlandığını ve **Activationretrybackoffınterval** değerinin 1 saniye olarak ayarlandığını varsayalım. Bu, etkinleştirme işleminin daha sonra (0 + 1 + 2 + 3 + 4) = 10 saniye (ilk yeniden deneme işleminin hemen olduğunu geri çek) ve bu barındırma yeniden denenmeden sonra verdiği anlamına gelir. Bu durumda, etkinleştirme işlemi tamamlanır ve 15 saniye sonra yeniden denenmeyecektir. Bunun nedeni, Service Fabric 15 saniye içinde tüm izin verilen yeniden denemeler tükenmesidir. Bu nedenle, ReconfigurationAgent 'tan her yeniden deneme, barındırma alt sisteminde yeni bir etkinleştirme işlemi oluşturur ve bu model yinelenmeye devam eder. Sonuç olarak, ServiceType hiçbir şekilde düğümde listelenmez. ServiceType düğüm üzerinde blok olmadığından, çoğaltma farklı bir düğümde taşınmaz ve denenmez.
> 

## <a name="deactivation"></a>Bırakmayı

Bir düğümde bir ServicePackage etkinleştirildiğinde, devre dışı bırakma için izlenir. 

Devre dışı bırakma işlemi iki şekilde geçerlidir:

- Düzenli olarak: her **Deactivationscanınterval**'DA, hiçbir zaman bir çoğaltma barındırılmayan hizmet paketlerini denetler ve devre dışı bırakma adayları olarak işaretler.
- ReplicaClose: bir çoğaltma kapalıysa, Etkinleştirici bir azaltma sayısı azaltır. Sayı 0 ' a gittiğinde, ServicePackage herhangi bir çoğaltmayı barındırmadığından ve bu nedenle devre dışı bırakma için bir aday olduğu anlamına gelir.

 [Özel/paylaşılan][a2]etkinleştirme moduna bağlı olarak, devre dışı bırakma adayları **DeactivationGraceInterval** for Sharedmode ve exclusivemode için **ExclusiveModeDeactivationGraceInterval** sonrasında zamanlanır. Bu süre içinde yeni bir çoğaltma yerleştirmesi gelirse, devre dışı bırakma işlemi iptal edilir.

### <a name="periodically"></a>Yokla
Düzenli olarak devre dışı bırakma örnekleri hakkında tartışalım:

Örnek 1: Etkinleştirici, zaman T (**Deactivationscanınterval**) zamanında tarama Işlemi yapalım. Sonraki tarama 2T konumunda olacak. T + 1 ' de bir ServicePackage etkinleştirmenin gerçekleştiğini varsayalım. Bu ServicePackage bir çoğaltma barındırmıyor ve bu nedenle devre dışı bırakılması gerekiyor. ServicePackage 'in devre dışı bırakma için bir aday olması için, en az T süresi için çoğaltma olmaması durumunda olması gerekir. Yani, 2T + 1 ' de devre dışı bırakma için uygun olacaktır. Bu nedenle, 2T 'de tarama, devre dışı bırakma için bir aday olarak bu ServicePackage 'i bulmaz. Sonraki devre dışı bırakma işlemi, bu hizmet paketini devre dışı bırakma için zamanlanır, çünkü artık zaman T için çoğaltma durumunda.  

Örnek 2: bir ServicePackage 'in T-1 zamanında etkinleştirildiğini ve Etkinleştirici 'in T 'de tarama gerçekleştirmesini söyleyin. ServicePackage bir çoğaltma barındırmıyor. Sonra, sonraki taramada bu ServicePackage devre dışı bırakma için bir aday olarak bulunur ve bu nedenle devre dışı bırakma için zamanlanır.  

Örnek 3: bir ServicePackage 'in T – 1 ' de etkinleştirildiğini ve Etkinleştirici tarafından bir tarama olduğunu varsayalım. ServicePackage henüz bir çoğaltmayı barındırmıyor. Şimdi T + 1 ' de bir çoğaltma yerleştirilir, yani Barındırma bir ıncrementusagecount alır, bu da bir çoğaltmanın oluşturulduğu anlamına gelir. Şimdi 2T 'de bu ServicePackage devre dışı bırakma için zamanlanmayacak. Bir çoğaltma içerdiğinden, devre dışı bırakma işlemi aşağıda açıklanan ReplicaClose mantığına taşınır.

Örnek 4: ServicePackage 'in büyük olduğunu, 10 GB olduğunu ve düğüm üzerine indirmek için biraz zaman süreyelim. Uygulama etkinleştirildikten sonra, Etkinleştirici, yaşam döngüsünü izler. Artık, **Deactivationscanınterval** yapılandırmasını küçük bir değere ayarladıysanız, tüm süre indirilmeye çalıştığı Için, servicepackage 'in düğümde etkinleştirilecek zaman olmadığı sorunlar yaşayabilirsiniz. Sorunu aşmak için, [düğümde ServicePackage 'i önceden indirebilir][p1] veya **Deactivationscanınterval** değerini artırabilirsiniz. 

> [!NOTE]
> Bir servicepackage, (**deactivationscanınterval** ile 2 **_deactivationscanınterval_*) + **DeactivationGraceInterval** /** ExclusiveModeDeactivationGraceInterval * * arasında her yerde devre dışı bırakılabilir. 
>

### <a name="replica-close"></a>Çoğaltma kapalı:
Devre dışı bırakma, bir ServicePackage 'in tuttuğu çoğaltma sayısını tutar. Bir ServicePackage bir çoğaltmayı tutuyor ve çoğaltma kapatılabilir/kapalıysa, barındırma bir azaltma sayısı alır. Bir çoğaltma açıldığında, barındırma bir ıncrementusagecount alır. Azalma, servicepackage 'in artık daha az bir çoğaltma barındırdığı ve sayı 0 ' a düştüğünde, servicepackage devre dışı bırakma için zamanlanır ve sonrasında devre dışı bırakılacak süre **DeactivationGraceInterval** / **ExclusiveModeDeactivationGraceInterval** olur. 

For Ex: T ve servicepackage 'in, 2T + X (**DeactivationGraceInterval** / **ExclusiveModeDeactivationGraceInterval**) ' de devre dışı bırakılacak şekilde zamanlandığını varsayalım. Bu süre boyunca barındırma bir ıncrementusage alırsa bir çoğaltmanın oluşturulduğu anlamına gelir, devre dışı bırakma işlemi iptal edilir.

> [!NOTE]
> Bu yapılandırma ayarları ne anlama geliyor?
**DeactivationGraceInterval** / **ExclusiveModeDeactivationGraceInterval**: bir hizmet paketine, herhangi bir çoğaltmayı barındırdıktan sonra yeniden barındırmak için verilen süre. 
**Deactivationscanınterval**: hiçbir çoğaltma Barındırılmadıysa servicepackage 'e bir çoğaltmayı barındırmak için verilen en kısa süre ( kullanılmazsa.
>

### <a name="ctrlc"></a>Ctrl+C
Bir servicepackage **DeactivationGraceInterval** / **ExclusiveModeDeactivationGraceInterval** ' i geçtiğinde ve yine de bir çoğaltma barındırmadığında, devre dışı bırakma işlemi iptal edilemez. CodePackage 'e, artık devre dışı bırakma işlem hattının işlemi aşağı getirmek için geçmesi gerektiği anlamına gelen bir CTRL + C işleyicisi verilir. Bu süre boyunca, aynı ServicePackage için yeni bir çoğaltma, devre dışı bırakmayı etkinleştirmeye geçiş yapamadığı için başarısız olur.

## <a name="cluster-configs"></a>Küme yapılandırması

Yapılandırma, etkinleştirme/decasyon 'yi etkileyen varsayılanlarla birlikte yapılandırma.

### <a name="servicetype"></a>ServiceType
**Servicetypedisablefailurethreshold**: varsayılan 1. Bu düğümdeki hizmet türünü devre dışı bırakmak ve yerleştirme için farklı bir düğüm denemek üzere FM (FailoverManager) sonrasında oluşan hata sayısı eşiği.
**ServiceTypeDisableGraceInterval**: varsayılan 30 sn. Hizmet türünün devre dışı bırakılabileceği zaman aralığı.
**Servicetyperyumuristrationtimeout**: varsayılan 300 sn. ServiceType Service Fabric kayıt için zaman aşımı.

### <a name="activation"></a>Etkinleştirme
**Activationretrybackoffınterval**: varsayılan 10 sn. Her etkinleştirme hatasında geri alma aralığı.
**Activationmaxfailurecount**: varsayılan 20. Sistemin başarısız olarak etkinleştirme işlemini gerçekleştirmeden önce yeniden denemesi gereken en fazla sayı. 
**Activationretrybackoffüs Ationbase**: varsayılan 1,5.
**Activationmaxretryınterval**: varsayılan 3600 sn. Hatalarda etkinleştirme için en fazla geri dönüş.
**Codepackagecontinuousexitfailureresetınterval**: varsayılan 300 sn. CodePackage için sürekli çıkış hatası sayısını sıfırlama zaman aşımı.

### <a name="download"></a>İndir
**Deploymentretrybackoffınterval**: varsayılan 10. Dağıtım hatası için geri dönüş aralığı.
**Deploymentmaxretryınterval**: varsayılan 3600 sn. Hatalarda dağıtım için en fazla geri dönüş.
**Deploymentmaxfailurecount**: varsayılan 20. Uygulama dağıtımı, bu uygulamanın düğümde dağıtılması başarısız olmadan önce DeploymentMaxFailureCount süreleri için yeniden denenecek.

### <a name="deactivation"></a>Bırakmayı
**Deactivationscanınterval**: Varsayılan 600 sn. Hiçbir çoğaltma barındırılmadıysa, bir çoğaltmayı barındırmak için ServicePackage 'e verilen en kısa süre ( kullanılmazsa.
**DeactivationGraceInterval**: varsayılan 60 sn. Bir ServicePackage 'e, **paylaşılan** işlem modeli durumunda herhangi bir çoğaltma barındırıldıktan sonra yeniden barındırmak için verilen süre.
**ExclusiveModeDeactivationGraceInterval**: varsayılan 1 sn. Bir ServicePackage 'e, **özel** işlem modeli durumunda herhangi bir çoğaltma barındırıldıktan sonra yeniden barındırmak için verilen süre.

## <a name="next-steps"></a>Sonraki adımlar
[Bir uygulamayı paketleyin][a3] ve dağıtıma başlamaya hazırlanın.

[Uygulamaları dağıtın ve kaldırın][a4]. Bu makalede, uygulama örneklerini yönetmek için PowerShell 'in nasıl kullanılacağı açıklanır.

<!--Link references--In actual articles, you only need a single period before the slash-->
[a1]: service-fabric-application-model.md
[a2]: service-fabric-hosting-model.md
[a3]: service-fabric-package-apps.md
[a4]: service-fabric-deploy-remove-applications.md

[p1]: /powershell/module/servicefabric/copy-servicefabricservicepackagetonode
