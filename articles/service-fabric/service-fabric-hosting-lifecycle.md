---
title: Azure Service Fabric etkinleştirme ve devre dışı bırakma yaşam döngüsü
description: Bir düğümdeki uygulama ve ServicePackage yaşam döngüsünü açıklar
author: tugup
ms.topic: conceptual
ms.date: 05/1/2020
ms.author: tugup
ms.openlocfilehash: b106061805ea5485893df292c40974d3ee9bcadb
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2020
ms.locfileid: "86258821"
---
# <a name="azure-service-fabric-hosting-lifecycle"></a>Azure Service Fabric barındırma yaşam döngüsü
Bu makalede, bir düğümde uygulama etkinleştirildiğinde ve davranışı denetlemek için kullanılan çeşitli küme yapılandırmaları hakkında bir genel bakış sunulmaktadır.

Devam etmeden önce, [Service Fabric bir uygulama modelleme][a1]bölümünde açıklanan çeşitli kavramları ve ilişkileri anladığınızdan emin olun. 

> [!NOTE]
> Bu makalede, açıkça başka bir şeyin anlamı olarak belirtilmedikçe:
>
> - *Çoğaltma* , durum bilgisi olan bir hizmetin bir çoğaltmasını ve durum bilgisi olmayan bir hizmetin örneğini ifade eder.
> - *CodePackage* , bir *serviceType*kayıt yapan bir *ServiceHost* Işlemine eşdeğer olarak değerlendirilir ve bu *serviceType*hizmet çoğaltmalarını barındırır.
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
**Servicetypedisablefailurethreshold** , serviceType blok listeleme için zamanlandıktan sonra başarısız olan başarısızlık sayısını (etkinleştirme, Indirme, CodePackage hatası) belirler. Bu nedenle, ilk etkinleştirme/indirme hatası ya da CodePackage kilitlenmesi, ServiceType blok listesinin zamanlamasını tetiklemelidir. **ServiceTypeDisableGraceInterval** config, bu, son kullanılan serviceType, bu düğümde blok olarak işaretlenmiş olan yetkisiz kullanım aralığını belirler. Tüm bu işlemler için etkinleştirme/indir/CodePackage yeniden başlatma işlemi, hala tekrar tekrar deneme modunda ve barındırma alt sistemi tarafından izlenmelidir. Yeniden deneniyor, örneğin: CodePackage kilitlenmeden sonra başlayacak şekilde zamanlanır veya Service Fabric paketleri yeniden indirmeyi dener.
Blocklistelendikten sonra, ' ServiceTypeRegistration: ServiceType ' özelliği için "' System. Hosting ' bildiren bir hata olduğunu görmeniz gerekir. ServiceType düğüm üzerinde devre dışı bırakıldı. "

ServiceType düğüm üzerinde yeniden etkinleştirilecek 
- Etkinleştirme işlemi başarılı olursa veya hata oluştuğunda **Activationmaxfailurecount** yeniden denemeye ulaşırsa.
- İndirme işlemi başarılı olursa veya hata sonrasında **Deploymentmaxfailurecount** yeniden denemelerine ulaşırsa.
- Kilitlenen bir CodePackage yeniden başlatılır ve ServiceType başarıyla kaydedilir.

**Iıpı 'nin activationmaxfailurecount** / **deploymentmaxfailurecount** yeniden denemeler sonrasında yeniden etkinleştirilme nedeni, bir düğümdeki uygulamayı etkinleştirmek/indirmek için gerçekleştirilecek Service Fabric en yüksek girişimlerdir. Başarılı olmazsa, geçerli işlem yeniden denenmez ve Service Fabric bu işlemin başarılı olması için, soruna otomatik olarak hata vererek, etkinleştirme/indirme işleminin yaşam döngüsüne bağlıdır. Bir çoğaltmanın yerleştirilmesi tarafından tetiklenen yeni bir etkinleştirme/indirme işlemi, ServiceType blok listesini yeniden tetikleyebilir veya başarılı olabilir.

> [!NOTE]
> Bir ServiceType kayıt yaptırmayan CodePackage kilitlenmezse, ServiceType ' yı etkilemez. Yalnızca bir çoğaltma kilitlenmesinin barındırıldığı CodePackage, ServiceType ' yı etkileyecektir.
>

### <a name="codepackage-crash"></a>CodePackage kilitlenmesi
Bir CodePackage kilitlenirse, Service Fabric yeniden başlatmak için bir geri dönüş kullanır ve arka kapatma, kod paketinin bizimle birlikte bir tür kaydettirip kaydolmadığını bağımsız olur.

Geri alma değeri her zaman min (RetryTime, **Activationmaxretryınterval**) ve bu değer, **Activationretrybackoffüs ationbase** yapılandırmasına bağlı olarak sabit, doğrusal veya üstel olabilir.

- Sabit: **Activationretrybackoffüs Ationbase** = = 0 Ise, RetryTime = **Activationretrybackoffınterval**;
- Doğrusal: **Activationretrybackoffüs Ationbase** = = 0 Ise, RetryTime = ContinuousFailureCount * **Activationretrybackoffınterval** ; burada ContinousFailureCount, bir CodePackage 'in kaç kez çöktüğü veya etkinleştiremediği sayısıdır.
- Üstel: RetryTime = (saniye cinsinden**Activationretrybackoffınterval** ) * (**Activationretrybackoffüs ationbase** ^ ContinuousFailureCount);
    
Davranışı hızlı yeniden başlatmalar gibi istediğiniz şekilde denetleyebilirsiniz. Doğrusal olarak bahsedelim. Bu, bir CodePackage kilitlenirse, CodePackage devre dışı bırakılıncaya kadar başlangıç aralığının 10, 20, 30 40 saniye sonra olacağını gösterir. 
    
Bir hatadan sonra geçen en uzun süre Service Fabric (bekler), **Activationmaxretryınterval** öğesine tabidir
    
CodePackage çöktüğü ve geri dönerse, Service Fabric için **Codepackagecontinuousexitfailureresetınterval** için bir sorun olması gerekir, bu durumda, sistem durumu raporunun üzerine yazılacak ve ContinousFailureCount ' ı sıfırlacaksınız.

### <a name="codepackage-not-registering-servicetype"></a>CodePackage, ServiceType kayıt değil
Bir CodePackage canlı kalırsa ve bir ServiceType 'yi bizimle kaydetmesi bekleniyorsa, ancak hiçbir zaman yapmadıysa, bu durumda Service Fabric, ServiceType zaman aşımı içinde yapılandırılmamasını söyleyen **Servicetyperyumuristrationtimeout** sonrasında bir uyarı HealthReport oluşturacaktır.

### <a name="activation-failure"></a>Etkinleştirme hatası
Service Fabric, etkinleştirme sırasında hata bulduğunda her zaman doğrusal bir geri (CodePackage kilitlenmesiyle aynı) kullanır. Bu, etkinleştirme işleminin daha sonra (0 + 10 + 20 + 30 + 40) = 100 sn (ilk yeniden deneme anında) olarak verilecek anlamına gelir. Bu etkinleştirme yeniden denenmez.
    
En fazla etkinleştirme geri alma işlemi **Activationmaxretryınterval** olabilir ve **Activationmaxfailurecount**yeniden denenmelidir.

### <a name="download-failure"></a>İndirme hatası
Service Fabric, indirme sırasında hatayla karşılaştığında her zaman doğrusal bir arka kapatma kullanır. Bu, etkinleştirme işleminin daha sonra (0 + 10 + 20 + 30 + 40) = 100 sn (ilk yeniden deneme anında) olarak verilecek anlamına gelir. Bu indirme işlemi yeniden denenmez. İndirme için doğrusal geri alma, ContinuousFailureCount ***Deploymentretrybackoffınterval** değerine eşittir ve **deploymentmaxretryınterval**için maksimum geri kapatılabilir. Etkinleştirmeler gibi, indirme işlemi **Activationmaxfailurecount**için yeniden deneyebilir.

> [!NOTE]
> Yapılandırmaları değiştirmeden önce göz önünde bulundurmanız gereken birkaç örnek aşağıda verilmiştir.

* CodePackage kilitlenme ve geri alma işlemlerini devam ederse, ServiceType devre dışı bırakılır. Ancak etkinleştirmeler yapılandırması hızlı bir yeniden başlatmaya sahipse CodePackage 'in, ServiceType 'nın devre dışı bırakılacağını görebilmek için birkaç kez gelebilmesi mümkündür. For Ex: CodePackage 'in geldiğini varsayın, ServiceType Service Fabric kaydeder ve sonra çöker. Bu durumda, barındırma bir tür kaydı aldığında **ServiceTypeDisableGraceInterval** dönemi iptal edilir. Bu, CodePackage **ServiceTypeDisableGraceInterval** ' den büyük bir değere sahip olana kadar yineleyebilir ve ardından düğümde serviceType devre dışı bırakılır. Bu nedenle, bu bir süre içinde, ServiceType devre dışı bırakılmadan önce bir işlem olabilir.

* Etkinleştirmeler söz konusu olduğunda, Service Fabric sisteminin bir düğüme bir çoğaltma eklemesi gerektiğinde RA (ReconfigurationAgent), uygulamayı etkinleştirmek ve etkinleştirme isteğini 15 saniyede bir (**Rapmessageretryıınterval**) yeniden denemeye yönelik alt sistemi ister. Service Fabric sistem 'in ServiceType devre dışı bırakıldığını bilmesini sağlamak için, barındırma içindeki etkinleştirme işleminin yeniden deneme aralığı ve **ServiceTypeDisableGraceInterval**kıyasla daha uzun bir süre için canlı olması gerekir. Örneğin: kümede yapılandırmalarını **activationmaxfailurecount** değerinin 5 olarak ayarlanmış olması ve **activationretrybackoffınterval** değerinin 1 sn olarak ayarlanmış olmasına izin verin. Bu, etkinleştirme işleminin daha sonra (0 + 1 + 2 + 3 + 4) = 10 sn (ilk yeniden deneme anında) ve bu barındırma yeniden denemeye başladıktan sonra verdiği anlamına gelir. Bu durumda, etkinleştirme işlemi tamamlanır ve 15 saniye sonra yeniden denenmeyecektir. Service Fabric, tüm yeniden denemeler 15 saniye içinde tükendiğinden gerçekleşti. Bu nedenle, ReconfigurationAgent 'tan her yeniden deneme, barındırma alt sisteminde yeni bir etkinleştirme işlemi oluşturur ve bu model yinelenen ve ServiceType, düğüm üzerinde hiçbir durumda devre dışı bırakılmayacaktır. ServiceType, bu düğüm üzerinde devre dışı bırakılmayacak ve SF sisteminin bileşen FM (FailoverManager), çoğaltmayı farklı bir düğüme taşımayacak.
> 

## <a name="deactivation"></a>Bırakmayı

Bir düğümde bir ServicePackage etkinleştirildiğinde, devre dışı bırakma için izlenir. Etkinleştirici, izlemeyi tutan varlıktır.
Etkinleştirici, iki şekilde çalışmaktadır:

1.  Düzenli olarak: her **Deactivationscanınterval**'DA, hiçbir zaman bir çoğaltma barındırılmayan ve bunları devre dışı bırakma adayları olarak Işaretleyen servicepackages denetimi gerçekleştirir.
2.  ReplicaClose: bir çoğaltma kapalıysa, Etkinleştirici bir azaltma sayısı azaltır. Sayı 0 ' a gittiğinde, ServicePackage herhangi bir çoğaltma barındırmaz ve bu nedenle devre dışı bırakma için bir adaydır.

 [Özel/paylaşılan][a2]etkinleştirme moduna bağlı olarak, **DeactivationGraceInterval** sonrasında devre dışı bırakma adayları, exclusivemode için **ExclusiveModeDeactivationGraceInterval** Bu süre içinde yeni bir çoğaltma yerleştirmesi, devre dışı bırakma işlemi iptal edilir.

### <a name="periodically"></a>Yokla
Örnek 1: Etkinleştirici, zaman T (**Deactivationscanınterval**) zamanında tarama Işlemi yapalım. Sonraki tarama 2T konumunda olacak. T + 1 ' de bir ServicePackage etkinleştirmenin gerçekleştiğini varsayalım. Bu ServicePackage bir çoğaltma barındırmıyor ve bu nedenle devre dışı bırakılması gerekiyor. ServicePackage 'in devre dışı bırakma için bir aday olması için, en az T süresi için çoğaltma olmaması durumunda olması gerekir. Yani, 2T + 1 ' de devre dışı bırakma için uygun olacaktır. Bu nedenle, 2T 'de tarama, devre dışı bırakma için bir aday olarak bu ServicePackage 'i bulmaz. Sonraki devre dışı bırakma işlemi, bu hizmet paketini devre dışı bırakma için zamanlanır, çünkü artık zaman T için çoğaltma durumunda.  

Örnek 2: bir ServicePackage 'in T-1 zamanında etkinleştirildiğini ve Etkinleştirici 'in T 'de tarama gerçekleştirmesini söyleyin. ServicePackage bir çoğaltma barındırmıyor. Sonra, sonraki taramada bu ServicePackage devre dışı bırakma için bir aday olarak bulunur ve bu nedenle devre dışı bırakma için zamanlanır.  

Örnek 3: bir ServicePackage 'in T – 1 ' de etkinleştirildiğini ve Etkinleştirici tarafından bir tarama olduğunu varsayalım. ServicePackage henüz bir çoğaltmayı barındırmıyor. Şimdi T + 1 ' de bir çoğaltma yerleştirilir, yani Barındırma bir ıncrementusagecount alır, bu da bir çoğaltmanın oluşturulduğu anlamına gelir. Şimdi 2T 'de bu ServicePackage devre dışı bırakma için zamanlanmayacak. Artık devre dışı bırakma işlemi, aşağıda açıklanan ReplicaClose mantığına geçer.

Örnek 4: ServicePackage 'in, 10 GB gibi büyük olduğunu varsayalım, bu durumda düğümde indirmek biraz zaman alabilir. Uygulama etkinleştirildikten sonra, Etkinleştirici, yaşam döngüsünü izler. Artık, **Deactivationscanınterval** config ' i küçükse, tüm süre indirilmeye çalıştığı Için servicepackage 'in düğümde etkinleştirilecek zaman olmadığı sorunlar yaşayabilirsiniz. Sorunu aşmak için, [düğümde ServicePackage 'i önceden indirebilirsiniz][p1]. 

> [!NOTE]
> Bu nedenle bir servicepackage, (**deactivationscanınterval** ila 2 ***deactivationscanınterval**) + **DeactivationGraceInterval** / **ExclusiveModeDeactivationGraceInterval**arasında herhangi bir yerde devre dışı bırakılabilir. 
>

### <a name="replica-close"></a>Çoğaltma kapalı:
Deetkinleştirici, bir ServicePackage 'in tuttuğu çoğaltma sayısını tutar. Bir ServicePackage bir çoğaltmayı tutuyor ve çoğaltma kapatılabilir/kapalıysa, barındırma bir azaltma sayısı alır. Bir çoğaltma açıldığında, barındırma bir ıncrementusagecount alır. Azalma, servicepackage 'in artık daha az bir çoğaltma barındırdığı ve sayı 0 ' a düştüğünde, servicepackage devre dışı bırakma için zamanlanır ve sonrasında devre dışı bırakılacak süre **DeactivationGraceInterval** / **ExclusiveModeDeactivationGraceInterval**olur. 

For Ex: T ve servicepackage 'in, 2T + X (**DeactivationGraceInterval** / **ExclusiveModeDeactivationGraceInterval**) ' de devre dışı bırakılacak şekilde zamanlandığını varsayalım. Bu süre boyunca barındırma bir ıncrementusage alırsa bir çoğaltmanın oluşturulduğu anlamına gelir, devre dışı bırakma işlemi iptal edilir.

> [!NOTE]
>Bu nedenle bu yapılandırma temelde ne anlama gelir: **DeactivationGraceInterval** / **ExclusiveModeDeactivationGraceInterval**: bir servicepackage 'e, herhangi bir çoğaltmayı barındırdıktan sonra yeniden barındırmak için verilen süre. 
**Deactivationscanınterval**: hiçbir çoğaltma Barındırılmadıysa servicepackage 'e bir çoğaltmayı barındırmak için verilen en kısa süre ( kullanılmazsa.
>

### <a name="ctrlc"></a>Ctrl+C
Bir servicepackage **DeactivationGraceInterval** / **ExclusiveModeDeactivationGraceInterval** ' i geçtiğinde ve yine de bir çoğaltma barındırmadığında, devre dışı bırakma işlemi iptal edilemez. CodePackage 'e, artık devre dışı bırakma işlem hattının işlemi aşağı getirmek için geçmesi gerektiği anlamına gelen bir CTRL + C işleyicisi verilir. Bu süre boyunca, aynı ServicePackage için yeni bir çoğaltma, devre dışı bırakmayı etkinleştirmeye geçiş yapamadığı için başarısız olur.

## <a name="cluster-configs"></a>Küme yapılandırması

Yapılandırma, etkinleştirme/decasyon 'yi etkileyen varsayılanlarla birlikte yapılandırma.

### <a name="servicetype"></a>ServiceType
**Servicetypedisablefailurethreshold**: varsayılan 1. Bu düğümdeki hizmet türünü devre dışı bırakmak ve yerleştirme için farklı bir düğüm denemek üzere FM (FailoverManager) sonrasında oluşan hata sayısı eşiği.
**ServiceTypeDisableGraceInterval**: varsayılan 30 sn. hizmet türünün devre dışı bırakılabileceği zaman aralığı.
**Servicetyperyumuristrationtimeout**: varsayılan 300 sn. ServiceType Service Fabric kayıt için zaman aşımı.

### <a name="activation"></a>Etkinleştirme
**Activationretrybackoffınterval**: varsayılan 10 sn. her etkinleştirme hatasında geri alma aralığı.
**Activationmaxfailurecount**: varsayılan 20. Sistemin başarısız olarak etkinleştirme işlemini gerçekleştirmeden önce yeniden denemesi gereken en fazla sayı. 
**Activationretrybackoffüs Ationbase**: varsayılan 1,5.
**Activationmaxretryınterval**: hatalarda etkinleştirme için varsayılan 3600 sn. Max geri.
**Codepackagecontinuousexitfailureresetınterval**: varsayılan 300 sn. CodePackage için sürekli çıkış hatası sayısını sıfırlama zaman aşımı.

### <a name="download"></a>İndirme
**Deploymentretrybackoffınterval**: varsayılan 10. Dağıtım hatası için geri dönüş aralığı.
**Deploymentmaxretryınterval**: hatalarda dağıtım için varsayılan 3600 sn. Max geri dönüş.
**Deploymentmaxfailurecount**: varsayılan 20. Uygulama dağıtımı, bu uygulamanın düğümde dağıtılması başarısız olmadan önce DeploymentMaxFailureCount süreleri için yeniden denenecek.

### <a name="deactivation"></a>Bırakmayı
**Deactivationscanınterval**: Varsayılan 600 sn. hiçbir çoğaltma barındırılmadıysa, bir çoğaltmayı barındırmak Için servicepackage 'e verilen en kısa süre ( kullanılmazsa.
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
