---
title: Coğrafi yedekli depolama yı kullanarak yüksek kullanılabilir uygulamalar tasarla
titleSuffix: Azure Storage
description: Kesintileri işlemek için yeterince esnek olan yüksek kullanılabilir bir uygulamayı mimar etmek için okuma erişimi coğrafi yedekli depolamayı nasıl kullanacağınızı öğrenin.
services: storage
author: tamram
ms.service: storage
ms.topic: conceptual
ms.date: 01/14/2020
ms.author: tamram
ms.reviewer: artek
ms.subservice: common
ms.openlocfilehash: 592be1710893791e80dfe4b20e1323e789b33e69
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77157101"
---
# <a name="designing-highly-available-applications-using-read-access-geo-redundant-storage"></a>Okuma erişimi coğrafi yedekli depolama yı kullanarak yüksek kullanılabilir uygulamalar tasarlama

Azure Depolama gibi bulut tabanlı altyapıların yaygın bir özelliği, uygulamaları barındırmak için son derece kullanılabilir bir platform sağlamalarıdır. Bulut tabanlı uygulamaların geliştiricileri, kullanıcılarına yüksek oranda kullanılabilir uygulamalar sunmak için bu platformdan nasıl yararlanılabileceklerini dikkatlice düşünmelidir. Bu makalede, geliştiricilerin Azure Depolama uygulamalarının yüksek oranda kullanılabilir olduğundan emin olmak için Azure'un coğrafi yedekli çoğaltma seçeneklerinden birini nasıl kullanabilecekleri üzerinde duruluyor.

Coğrafi yedekli çoğaltma için yapılandırılan depolama hesapları birincil bölgede eşzamanlı olarak çoğaltılır ve daha sonra yüzlerce mil uzaktaki ikincil bir bölgeye eşzamanlı olarak çoğaltılır. Azure Depolama iki tür coğrafi yedekli çoğaltma sunar:

* [Coğrafi bölge yedekli depolama (GZRS) (önizleme)](storage-redundancy.md) hem yüksek kullanılabilirlik hem de maksimum dayanıklılık gerektiren senaryolar için çoğaltma sağlar. Veriler, bölge yedekli depolama (ZRS) kullanılarak birincil bölgedeki üç Azure kullanılabilirlik bölgesinde eşzamanlı olarak çoğaltılır ve ardından ikincil bölgeye eşit olarak çoğaltılır. İkincibölgedeki verilere okuma erişimi için, okuma-erişim coğrafi bölge yedekli depolamayı (RA-GZRS) etkinleştirin.
* [Coğrafi yedekli depolama (GRS),](storage-redundancy.md) bölgesel kesintilere karşı korumak için bölgeler arası çoğaltma sağlar. Veriler, birincil bölgede yerel olarak yedekli depolama (LRS) kullanılarak üç kez eşzamanlı olarak çoğaltılır ve ardından ikincil bölgeye eşzamanlı olarak çoğaltılır. İkincibölgedeki verilere okuma erişimi için, okuma erişimi coğrafi yedekli depolamayı (RA-GRS) etkinleştirin.

Bu makalede, birincil bölgede bir kesintisi işlemek için uygulamatasarımı nasıl gösterilmektedir. Birincil bölge kullanılamıyorsa, uygulamanız ikincil bölgeye karşı okuma işlemleri gerçekleştirmek için uyarlanabilir. Başlamadan önce depolama hesabınızın RA-GRS veya RA-GZRS için yapılandırıldığından emin olun.

Hangi birincil bölgelerin hangi ikincil bölgelerle eşleştiği hakkında bilgi için bkz: [İş sürekliliği ve olağanüstü durum kurtarma (BCDR): Azure Eşli Bölgeler.](https://docs.microsoft.com/azure/best-practices-availability-paired-regions)

Bu makalede kod parçacıkları ve sonunda indirip çalıştırabileceğiniz tam bir örneğe bağlantı vardır.

## <a name="application-design-considerations-when-reading-from-the-secondary"></a>Uygulama tasarımı nda ikincil

Bu makalenin amacı, birincil veri merkezinde büyük bir felaket durumunda bile (sınırlı kapasitede de olsa) çalışmaya devam edecek bir uygulamanın nasıl tasarlanacağınızı göstermektir. Birincil bölgeden okumayı engelleyen bir sorun olduğunda, ikincil bölgeden okuyarak uygulamanızı geçici veya uzun süren sorunları işleyebilecek şekilde tasarlayabilirsiniz. Birincil bölge yeniden kullanılabilir olduğunda, uygulamanız birincil bölgeden okumaya geri dönebilir.

RA-GRS veya RA-GZRS için uygulamanızı tasarlarken şu önemli noktaları aklınızdan tutun:

* Azure Depolama, birincil bölgenizde depoladığınız verilerin salt okunur bir kopyasını ikincil bir bölgede tutar. Yukarıda belirtildiği gibi, depolama hizmeti ikincil bölgenin konumunu belirler.

* Salt okunur kopya [sonunda](https://en.wikipedia.org/wiki/Eventual_consistency) birincil bölgedeki verilerle tutarlıdır.

* Blobs, tablolar ve kuyruklar için, birincil den ikincil bölgeye son çoğaltma oluştu zaman belirten bir *Son Eşitleme Zamanı* değeri için ikincil bölge sorgulayabilirsiniz. (Bu, şu anda RA-GRS artıklığı olmayan Azure Dosyaları için desteklenmez.)

* Birincil veya ikincil bölgede veri okumak ve yazmak için Depolama İstemci Kitaplığını kullanabilirsiniz. Okuma isteği nin zaman dolması durumunda okuma isteklerini otomatik olarak ikincil bölgeye yönlendirebilirsiniz.

* Birincil bölge kullanılamıyorsa, bir hesap başarısızı başlatabilirsiniz. İkincil bölgeye başarısız olduğunuzda, birincil bölgeye işaret eden DNS girişleri ikincil bölgeye işaret edecek şekilde değiştirilir. Başarısızlık tamamlandıktan sonra GRS ve RA-GRS hesapları için yazma erişimi geri yüklenir. Daha fazla bilgi için Azure [Depolama'da Olağanüstü Durum kurtarma ve depolama hesabı nın başarısız lığına (önizleme)](storage-disaster-recovery-guidance.md)bakın.

> [!NOTE]
> Müşteri tarafından yönetilen hesap başarısız olması (önizleme) Henüz GZRS/RA-GZRS'yi destekleyen bölgelerde kullanılamadığından, müşteriler şu anda GZRS ve RA-GZRS hesaplarıyla hesap başarısızlık olaylarını yönetemez. Önizleme sırasında Microsoft, GZRS/RA-GZRS hesaplarını etkileyen başarısız olayları yönetir.

### <a name="using-eventually-consistent-data"></a>Sonunda tutarlı verileri kullanma

Önerilen çözüm, arama uygulamasına olası eski verileri döndürmenin kabul edilebilir olduğunu varsayar. İkincil bölgedeki veriler sonunda tutarlı olduğundan, ikincil bölgeye yapılan bir güncelleştirme çoğaltma tamamlanmadan önce birincil bölgeye erişilemez hale gelebiliyor olabilir.

Örneğin, müşterinizin bir güncelleştirmeyi başarıyla gönderdiğini, ancak güncelleştirme ikincil bölgeye yayılmadan önce birincil bölge başarısız olduğunu varsayalım. Müşteri verileri geri okumak istediğinde, güncelleştirilmiş veriler yerine ikincil bölgeden eski verileri alır. Uygulamanızı tasarlarken, bunun kabul edilebilir olup olmadığına ve eğer öyleyse müşteriye nasıl mesaj göndereceğinize karar vermeniz gerekir. 

Bu makalenin ilerleyen aylarında, ikincil verilerin güncel olup olmadığını denetlemek için Son Eşitleme Saatini nasıl denetleriz göstereceğiz.

### <a name="handling-services-separately-or-all-together"></a>Hizmetleri ayrı ayrı veya hep birlikte işleme

Olası olmasa da, diğer hizmetler hala tam olarak işlevsel iken bir hizmet in kullanılamaz hale gelmesi mümkündür. Her hizmet için yeniden denemeleri ve salt okunur modunu ayrı ayrı işleyebilir veya tüm depolama hizmetleri için genel olarak yeniden denemeleri birlikte işleyebilirsiniz.

Örneğin, uygulamanızda kuyruklar ve lekeler kullanıyorsanız, bunların her biri için yeniden denlenebilir hataları işlemek için ayrı bir kod koymaya karar verebilirsiniz. Daha sonra blob hizmetinden bir yeniden deneme alırsanız, ancak kuyruk hizmeti hala çalışıyorsa, uygulamanızın yalnızca blobları işleyen kısmı etkilenir. Tüm depolama hizmeti yeniden denemelerini genel olarak işlemeye karar verirseniz ve blob hizmetine yapılan bir çağrı yeniden denenebilir bir hata döndürür, sonra hem blob hizmetine hem de sıra hizmetine gelen istekler etkilenir.

Sonuç olarak, bu uygulamanızın karmaşıklığına bağlıdır. Hataları hizmet e göre işlememeye karar verebilirsiniz, ancak bunun yerine tüm depolama hizmetleri için okuma isteklerini ikincil bölgeye yönlendirmeye ve birincil bölgedeki herhangi bir depolama hizmetiyle ilgili bir sorun algıladığında uygulamayı salt okunur modunda çalıştırabilirsiniz.

### <a name="other-considerations"></a>Diğer konular

Bunlar, bu makalenin geri kalanında tartışacağız diğer hususlar dır.

* Devre Kesici deseni kullanarak okuma isteklerinin yeniden denemelerini işleme

* Sonunda tutarlı veri ve Son Eşitleme Süresi

* Sınama

## <a name="running-your-application-in-read-only-mode"></a>Uygulamanızı salt okunur modunda çalıştırma

Birincil bölgede bir kesintiye etkili bir şekilde hazırlanmak için, hem başarısız okuma isteklerini hem de başarısız güncelleştirme isteklerini işleyebilmelisin (bu durumda eklemeler, güncelleştirmeler ve silmeler anlamına gelen güncelleştirmeyle). Birincil bölge başarısız olursa, okuma istekleri ikincil bölgeye yönlendirilebilir. Ancak, ikincil okuma okunur olduğundan güncelleştirme istekleri ikincil yönlendirilemez. Bu nedenle, uygulamanızı salt okunur modunda çalışacak şekilde tasarlamanız gerekir.

Örneğin, herhangi bir güncelleştirme isteği Azure Depolama'ya gönderilmeden önce denetlenen bir bayrak ayarlayabilirsiniz. Güncelleştirme isteklerinden biri geldiğinde, bunu atlayabilir ve müşteriye uygun bir yanıt döndürebilirsiniz. Sorun çözülene kadar bazı özellikleri tamamen devre dışı bırakmak ve kullanıcılara bu özelliklerin geçici olarak kullanılmayan durumu bildirmek isteyebilirsiniz.

Her hizmetin hatalarını ayrı ayrı işlemeye karar verirseniz, uygulamanızı hizmete göre salt okunur modunda çalıştırma yeteneğini de işlemeniz gerekir. Örneğin, etkinleştirilebilen ve devre dışı bırakılmış her hizmet için salt okunur bayraklar olabilir. Ardından bayrağı kodunuzdaki uygun yerlerde işleyebilirsiniz.

Uygulamanızı salt okunur modda çalıştırabilmek başka bir yan yarara sahiptir – büyük bir uygulama yükseltmesi sırasında sınırlı işlevsellik sağlama nızı sağlar. Uygulamanızı salt okunur modunda çalışmasını tetikleyebilir ve yükseltme yaparken birincil bölgedeki verilere kimsenin erişmemesini sağlayarak ikincil veri merkezini işaret edebilirsiniz.

## <a name="handling-updates-when-running-in-read-only-mode"></a>Salt okunur modunda çalışırken güncelleştirmeleri işleme

Salt okunur modunda çalışırken güncelleştirme isteklerini işlemenin birçok yolu vardır. Bunu kapsamlı bir şekilde ele alamayacağız, ama genel olarak, düşündüğünüz birkaç model vardır.

1. Kullanıcınıza yanıt verebilir ve güncelleştirmeleri şu anda kabul etmediğinizi söyleyebilirsiniz. Örneğin, bir kişi yönetim sistemi müşterilerin kişi bilgilerine erişmesini sağlayabilir, ancak güncelleştirmeler yapamaz.

2. Güncelleştirmelerinizi başka bir bölgeye ekolarak sıralayabilirsiniz. Bu durumda, bekleyen güncelleştirme isteklerinizi farklı bir bölgedeki bir kuyruğa yazar ve birincil veri merkezi yeniden çevrimiçi olduktan sonra bu istekleri işlemenin bir yolunu bursunuz. Bu senaryoda, müşteriye istenen güncelleştirmenin daha sonraki işlemler için sıraya ait olduğunu bildirmelisiniz.

3. Güncelleştirmelerinizi başka bir bölgedeki bir depolama hesabına yazabilirsiniz. Daha sonra birincil veri merkezi yeniden çevrimiçi olduğunda, verilerin yapısına bağlı olarak bu güncelleştirmeleri birincil verilerle birleştirmenin bir yolunu bulabilirsiniz. Örneğin, adında tarih/saat damgası olan ayrı dosyalar oluşturuyorsanız, bu dosyaları birincil bölgeye kopyalayabilirsiniz. Bu, günlük işleme ve iOT verileri gibi bazı iş yüklerinde çalışır.

## <a name="handling-retries"></a>Yeniden denemeleri işleme

Azure Depolama istemci kitaplığı, hangi hataların yeniden denenebileceğini belirlemenize yardımcı olur. Örneğin, 404 hatası (kaynak bulunamadı) yeniden denenmez, çünkü yeniden deneme nin başarıyla sonuçlanması olası değildir. Öte yandan, bir sunucu hatası olduğu için 500 hatası yeniden denenebilir ve sorun geçici bir sorun olabilir. Daha fazla ayrıntı için ,.NET depolama istemcisi kitaplığındaki [Üstel Retry sınıfının açık kaynak koduna](https://github.com/Azure/azure-storage-net/blob/87b84b3d5ee884c7adc10e494e2c7060956515d0/Lib/Common/RetryPolicies/ExponentialRetry.cs) göz atın. (ShouldRetry yöntemini arayın.)

### <a name="read-requests"></a>İstekleri okuma

Birincil depolamayla ilgili bir sorun varsa, okuma istekleri ikincil depolama alanına yönlendirilebilir. Yukarıda da belirtildiği [gibi, Sonunda Tutarlı Verileri Kullanma,](#using-eventually-consistent-data)uygulamanızın eski verileri okuması kabul edilebilir olmalıdır. İkincil verilerden verilere erişmek için depolama istemcisi kitaplığını kullanıyorsanız, **Konum Modu** özelliği için aşağıdakilerden birine bir değer ayarlayarak okuma isteğinin yeniden deneme davranışını belirtebilirsiniz:

* **PrimaryOnly** (varsayılan)

* **İlköğretim Sonrası Ortaöğretim**

* **Sadece Ortaöğretim**

* **İkincilThenPrimary**

**LocationMode'u** **PrimaryThenSecondary**olarak ayarladığınızda, birincil bitiş noktasına ilk okuma isteği yeniden denenebilecek bir hatayla başarısız olursa, istemci otomatik olarak ikincil bitiş noktasına başka bir okuma isteği yapar. Hata bir sunucu zaman aşımı ise, istemci hizmetten yeniden denilebilir bir hata almadan önce zaman aşımı için beklemek zorunda kalacaktır.

Yeniden denilebilir bir hataya nasıl yanıt verilecenize karar verirken göz önünde bulundurmanız gereken temelde iki senaryo vardır:

* Bu yalıtılmış bir sorundur ve birincil bitiş noktasına sonraki istekler yeniden denenebilir bir hata döndürmez. Bunun olabileceği bir örnek, geçici bir ağ hatası olduğunda ortaya çıkabilir.

    Bu senaryoda, Yalnızca seyrek olarak gerçekleştiği için **LocationMode'un** **PrimaryThenSecondary** olarak ayarlamında önemli bir performans cezası yoktur.

* Bu, birincil bölgedeki depolama hizmetlerinden en az birinde bir sorundur ve birincil bölgedeki bu hizmete sonraki tüm isteklerin bir süre için yeniden denenebilir hataları döndürme olasılığı yüksektir. Buna bir örnek, birincil bölgeye tamamen erişilememiş olmasıdır.

    Bu senaryoda, tüm okuma istekleriniz önce birincil bitiş noktasını çalışacağından, zaman aşımının dolmasını bekleyeceğinden ve ardından ikincil bitiş noktasına geçeceğinden bir performans cezası vardır.

Bu senaryolar için, birincil bitiş noktasıyla ilgili devam eden bir sorun olduğunu belirlemeli ve **KonumModu** özelliğini **SecondaryOnly'e**ayarlayarak tüm okuma isteklerini doğrudan ikincil bitiş noktasına göndermelisiniz. Şu anda, uygulamayı salt okunur modunda çalışacak şekilde de değiştirmeniz gerekir. Bu yaklaşım Devre [Kesici Deseni](/azure/architecture/patterns/circuit-breaker)olarak bilinir.

### <a name="update-requests"></a>İstekleri güncelleştirme

Devre Kesici deseni, güncelleştirme isteklerine de uygulanabilir. Ancak, güncelleştirme istekleri salt okunur olan ikincil depolama alanına yönlendirilemez. Bu istekler **için, Konum Modu** özelliğini **PrimaryOnly** (varsayılan) olarak bırakmalısınız. Bu hataları işlemek için, bu isteklere (örneğin, üst üste 10 hata gibi) bir metrik uygulayabilirsiniz ve eşiğiniz karşılandığında uygulamayı salt okunur moduna dönüştürebilirsiniz. Devre Kesici deseni hakkında bir sonraki bölümde aşağıda açıklananlarla güncelleştirme moduna dönmek için aynı yöntemleri kullanabilirsiniz.

## <a name="circuit-breaker-pattern"></a>Devre Kesici düzeni

Uygulamanızdaki Devre Kesici deseni kullanmak, tekrar tekrar başarısız olma olasılığı olan bir işlemi yeniden denemesini engelleyebilir. İşlem katlanarak yeniden denenirken zaman almak yerine uygulamanın çalışmaya devam etmesini sağlar. Ayrıca, hatanın ne zaman düzeltildiğini algılar ve uygulama işlemi yeniden deneyebilir.

### <a name="how-to-implement-the-circuit-breaker-pattern"></a>Devre kesici deseni nasıl uygulanır?

Birincil bitiş noktasıyla ilgili devam eden bir sorun olduğunu belirlemek için, istemcinin yeniden denilebilir hatalarla ne sıklıkta karşılaştığını izleyebilirsiniz. Her servis talebi farklı olduğundan, ikincil bitiş noktasına geçme ve uygulamayı salt okunur modunda çalıştırma kararı için kullanmak istediğiniz eşiğe karar vermeniz gerekir. Örneğin, üst üste hiçbir başarı elde edemediği10 hata varsa, anahtarı gerçekleştirmeye karar verebilirsiniz. Başka bir örnek, 2 dakikalık bir süre içinde isteklerin %90'ı başarısız olursa geçiş yapmaktır.

İlk senaryoiçin, hataların sayısını tutabilirsiniz ve maksimuma ulaşmadan önce bir başarı varsa, sayıyı sıfıra ayarlayın. İkinci senaryo için, bunu uygulamanın bir yolu MemoryCache nesnesini (.NET'te) kullanmaktır. Her istek için önbelleğe bir Önbellek ekleyin, değeri başarı (1) veya başarısız (0) olarak ayarlayın ve son kullanma süresini bundan 2 dakika sonraya (veya zaman kısıtlamanız ne olursa olsun) ayarlayın. Bir girişin son kullanma süresine ulaşıldığında, giriş otomatik olarak kaldırılır. Bu size 2 dakikalık bir süre verecektir. Depolama hizmetine her istekte bulunduğunuzda, önce MemoryCache nesnesi arasında değerleri toplama ve sayıma göre bölme yle yüzde başarıyı hesaplamak için bir Linq sorgusu kullanırsınız. Yüzde başarı bazı eşiğin altına düştüğünde (%10 gibi), **Saltasyonel'e** okuma istekleri için **LocationMode** özelliğini ayarlayın ve devam etmeden önce uygulamayı salt okunur moduna geçin.

Geçişin ne zaman yapılacağını belirlemek için kullanılabilen hata ların eşikleri uygulamanızdaki hizmetten hizmete değişebilir, bu yılları yapılanabilir parametreler haline getirmeyi düşünüyor. Bu, her hizmetten daha önce tartışıldığı gibi yeniden denenebilir hataları ayrı ayrı veya bir olarak işlemeye karar verdiğiniz yerdir.

Başka bir göz, bir uygulamanın birden çok örneğini nasıl işleyeceğiniz ve her örnekte yeniden deneebilir hataları algıladiğinizde ne yapmanız gerektiğinin nedenidir. Örneğin, aynı uygulama yüklü 20 VM'niz çalışıyor olabilir. Her örneği ayrı ayrı mı işliyorsunuz? Bir örnekte sorun olmaya başlarsa, yanıtı sadece bir örnekle sınırlamak mı istiyorsunuz, yoksa bir örnekte sorun olduğunda tüm örneklerin aynı şekilde yanıt vermesi için mi denemek istiyorsunuz? Örnekleri ayrı ayrı işlemek, yanıtı bunlar arasında koordine etmeye çalışmaktan çok daha kolaydır, ancak bunu nasıl yapacağınız uygulamanızın mimarisine bağlıdır.

### <a name="options-for-monitoring-the-error-frequency"></a>Hata sıklığını izleme seçenekleri

İkincil bölgeye ne zaman geçeceğinizi belirlemek ve uygulamayı salt okunur modunda çalışacak şekilde değiştirmek için birincil bölgedeki yeniden denemelerin sıklığını izlemek için üç ana seçeneğiniz var.

* Depolama isteklerinize geçtiğiniz [**OperationContext**](https://docs.microsoft.com/java/api/com.microsoft.applicationinsights.extensibility.context.operationcontext) nesnesindeki [**Yeniden Deneme**](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmos.table.operationcontext.retrying) olayı için bir işleyici ekleyin – bu makalede görüntülenen ve beraberindeki örnekte kullanılan yöntemdir. Bu olaylar, istemci bir isteği yeniden çalıştığında, istemcinin birincil bitiş noktasında yeniden denilebilir hatalarla ne sıklıkta karşılaştığını izlemenize olanak sağlar.

    ```csharp
    operationContext.Retrying += (sender, arguments) =>
    {
        // Retrying in the primary region
        if (arguments.Request.Host == primaryhostname)
            ...
    };
    ```

* Özel yeniden deneme ilkesindeki [**Değerlendir**](https://docs.microsoft.com/dotnet/api/microsoft.azure.cosmos.table.iextendedretrypolicy.evaluate) yönteminde, yeniden deneme yapıldığında özel kod çalıştırabilirsiniz. Yeniden deneme yapıldığında kaydetmeye ek olarak, bu size yeniden deneme davranışınızı değiştirme fırsatı da verir.

    ```csharp 
    public RetryInfo Evaluate(RetryContext retryContext,
    OperationContext operationContext)
    {
        var statusCode = retryContext.LastRequestResult.HttpStatusCode;
        if (retryContext.CurrentRetryCount >= this.maximumAttempts
            || ((statusCode >= 300 && statusCode < 500 && statusCode != 408)
            || statusCode == 501 // Not Implemented
            || statusCode == 505 // Version Not Supported
            ))
        {
            // Do not retry
            return null;
        }

        // Monitor retries in the primary location
        ...

        // Determine RetryInterval and TargetLocation
        RetryInfo info =
            CreateRetryInfo(retryContext.CurrentRetryCount);

        return info;
    }
    ```

* Üçüncü yaklaşım, uygulamanızda, birincil depolama bitiş noktanızı sürekli olarak sahte okuma istekleriyle (küçük bir leke okuma gibi) pingleyerek, durumunu belirlemek için özel bir izleme bileşeni uygulamaktır. Bu bazı kaynaklar, ancak önemli bir miktar alacaktı. Eşiğinize ulaşan bir sorun fark edildiğinde, **Saltveli ve** salt okunur moduna geçişi gerçekleştirirsiniz.

Bir noktada, birincil bitiş noktasını kullanarak ve güncelleştirmelere izin vermeye geri dönmek isteyeceksiniz. Yukarıda listelenen ilk iki yöntemden birini kullanıyorsanız, yalnızca birincil bitiş noktasına geri dönebilir ve rasgele seçilen bir zaman veya işlem sayısı gerçekleştirildikten sonra güncelleştirme modunu etkinleştirebilirsiniz. Daha sonra yeniden deneme mantığı üzerinden geçmesine izin verebilirsiniz. Sorun giderildiyse, birincil bitiş noktasını kullanmaya ve güncelleştirmelere izin vermeye devam edecektir. Hala bir sorun varsa, belirlediğiniz ölçütleri başarısız lığa uğrattıktan sonra bir kez daha ikincil bitiş noktasına ve salt okunur moduna geri döner.

Üçüncü senaryoda, birincil depolama bitiş noktasını pingleme yeniden başarılı olduğunda, **Geçişi PrimaryOnly'e** geri tetikleyebilir ve güncelleştirmelere izin vermeye devam edebilirsiniz.

## <a name="handling-eventually-consistent-data"></a>Sonunda tutarlı verileri işleme

Coğrafi yedekli depolama, birincil bölgeden ikincil bölgeye hareketleri çoğaltarak çalışır. Bu çoğaltma işlemi, ikincil bölgedeki verilerin *sonunda tutarlı*olduğunu garanti eder. Bu, birincil bölgedeki tüm işlemlerin eninde sonunda ikincil bölgede görüneceği, ancak görünmeden önce bir gecikme olabileceği ve işlemlerin ikincil bölgeye geldikleri sırada aynı sırada geleceğinin garantisi olmadığı anlamına gelir. başlangıçta birincil bölgede uygulanmıştır. İşlemleriniz ikincil bölgeye bozuk gelirse, hizmet yakalanana kadar ikincil bölgedeki verilerinizi tutarsız bir durumda olarak *düşünebilirsiniz.*

Aşağıdaki tablo, yöneticilerin *rolünün* bir üyesi yapmak için bir çalışanın ayrıntılarını güncelleştirdiğinizde neler olabileceğine ilişkin bir örnek gösterir. Bu örneğin iyiliği için, bu **çalışan** varlık güncelleştirmek ve yönetici rol varlık toplam yönetici sayısı nın bir sayı ile bir **yönetici rol** varlığı güncelleştirmeyi gerektirir. Güncelleştirmelerin ikincil bölgede sıra dışı nasıl uygulandığına dikkat edin.

| **Zaman** | **Işlem**                                            | **Çoğaltma**                       | **Son Eşitleme Süresi** | **Sonuç** |
|----------|------------------------------------------------------------|---------------------------------------|--------------------|------------| 
| T0       | İşlem A: <br> Çalışan ekleme <br> birincil varlık |                                   |                    | İşlem Birincil ekime eklenen A,<br> henüz çoğaltılmadı. |
| T1       |                                                            | İşlem A <br> için çoğaltılır<br> Ikincil | T1 | İşlem A ikincil olarak çoğaltılır. <br>Son Eşitleme Süresi güncelleştirildi.    |
| T2       | İşlem B:<br>Güncelleştirme<br> çalışan varlık<br> birincil  |                                | T1                 | Birincil olarak yazılan Işlem B,<br> henüz çoğaltılmadı.  |
| T3       | İşlem C:<br> Güncelleştirme <br>yönetici<br>rol varlığı<br>Birincil |                    | T1                 | Birincil olarak yazılan C işlemi,<br> henüz çoğaltılmadı.  |
| *T4*     |                                                       | İşlem C <br>için çoğaltılır<br> Ikincil | T1         | İşlem C ikincil çoğaltılır.<br>LastSyncTime güncelleştirilemedi çünkü <br>işlem B henüz çoğaltılmadı.|
| *T5*     | Varlıkları okuma <br>ikincil                           |                                  | T1                 | Çalışan için eski değeri elde elabilirsiniz <br> varlık, çünkü işlem B değil <br> henüz çoğaltılır. Yeni bir değer elde<br> yönetici rol varlığı, çünkü C<br> Çoğaltılmış. Son Eşitleme Süresi hala yok<br> işlem B nedeniyle güncelleştirildi<br> çoğaltılmadı. Bunu anlayabilirsin.<br>yönetici rol varlığı tutarsız <br>çünkü varlık tarihi/saati sonra <br>Son Eşitleme Zamanı. |
| *T6*     |                                                      | İşlem B<br> için çoğaltılır<br> Ikincil | T6                 | *T6* – C üzerinden yapılan tüm işlemler, <br>çoğaltıldı, Son Eşitleme Zamanı<br> güncellenir. |

Bu örnekte, istemcinin T5'teki ikincil bölgeden okumaya geçtiğini varsayalım. Şu anda **yönetici rol** varlığını başarıyla okuyabilir, ancak varlık, şu anda ikincil bölgede yönetici olarak işaretlenmiş **çalışan** varlık sayısıyla tutarlı olmayan yönetici sayısı için bir değer içerir. Müşteriniz, tutarsız bilgi riski ile bu değeri yalnızca görüntüleyebilir. Alternatif olarak, istemci, güncelleştirmeler sıra dışı gerçekleştiğinden **yönetici rolünün** tutarsız bir durumda olduğunu belirlemeye çalışabilir ve ardından kullanıcıyı bu gerçeği bildirebilir.

Olası tutarsız verilere sahip olduğunu anlamak için istemci, bir depolama hizmetini sorgulayarak istediğiniz zaman alabileceğiniz *Son Eşitleme Süresi'nin* değerini kullanabilir. Bu, ikincil bölgedeki verilerin en son tutarlı olduğu zamanı ve hizmetin bu noktadan önce tüm hareketleri uyguladığı zamanı gösterir. Yukarıda gösterilen örnekte, hizmet ikinci bölgedeki **çalışan** varlığı ekledikten sonra, son eşitleme süresi *T1*olarak ayarlanır. Hizmet, *T6*olarak ayarlandığında ikincil bölgedeki **çalışan** varlığını güncellene kadar *T1'de* kalır. İstemci, *T5'teki*varlığı okuduğunda son eşitleme süresini alırsa, varlık üzerindeki zaman damgasıyla karşılaştırabilir. Varlık üzerindeki zaman damgası son eşitleme süresinden daha geç sayılsa, varlık tutarsız bir durumdadır ve uygulamanız için uygun eylemi gerçekleştirebilirsiniz. Bu alanı kullanmak, birincil güncelleştirmeye yapılan son güncelleştirmenin ne zaman tamamlandığını bilmenizi gerektirir.

Son eşitleme süresini nasıl denetleyeceklerini öğrenmek [için, depolama hesabı için Son Eşitleme Süresini](last-sync-time-get.md)Denetle'ye bakın.

## <a name="testing"></a>Sınama

Yeniden deneebilir hatalarla karşılaştığında uygulamanızın beklendiği gibi çalıştığını test etmek önemlidir. Örneğin, bir sorun algıladığında uygulamanın ikincil ve salt okunur moduna geçtiğini ve birincil bölge yeniden kullanılabilir olduğunda geri döndüğünü test etmeniz gerekir. Bunu yapmak için, yeniden denenebilir hataları simüle etmek ve bunların ne sıklıkta oluştuğunu denetlemek için bir yol gerekir.

Bir komut dosyasında HTTP yanıtlarını engellemek ve değiştirmek için [Fiddler'ı](https://www.telerik.com/fiddler) kullanabilirsiniz. Bu komut dosyası, birincil bitiş noktanızdan gelen yanıtları tanımlayabilir ve HTTP durum kodunu Depolama İstemci Kitaplığı'nın yeniden denenebilir bir hata olarak tanıdığı bir kodla değiştirebilir. Bu kod snippet 502 durumu döndürmek için **çalışan veri** tablosuna karşı okuma isteklerine yanıtları engelleyen bir Fiddler komut dosyasıbasit bir örnek gösterir:

```java
static function OnBeforeResponse(oSession: Session) {
    ...
    if ((oSession.hostname == "\[yourstorageaccount\].table.core.windows.net")
      && (oSession.PathAndQuery.StartsWith("/employeedata?$filter"))) {
        oSession.responseCode = 502;
    }
}
```

Bu örneği daha geniş bir istek yelpazesini engellemek için genişletebilir ve yalnızca bazılarındaki **yanıt Kodunu** değiştirerek gerçek dünya senaryosunu daha iyi simüle edebilirsiniz. Fiddler komut dosyalarını özelleştirme hakkında daha fazla bilgi için [bkz.](https://docs.telerik.com/fiddler/KnowledgeBase/FiddlerScript/ModifyRequestOrResponse)

Uygulamanızı salt okunur modda yapılandırılabilir hale getirmek için eşikleri yaptıysanız, davranışı üretim dışı işlem hacimleriyle sınamak daha kolay olacaktır.

## <a name="next-steps"></a>Sonraki Adımlar

* Son Eşitleme Zamanı özelliğinin nasıl ayarlıolduğuna ilişkin başka bir örnek de dahil olmak üzere ikincil bölgeden nasıl okunacakhakkında daha fazla bilgi için [Azure Depolama Artıkları Seçenekleri ve Access Geo-Yedekdepolama'yı okuyun'](https://blogs.msdn.microsoft.com/windowsazurestorage/2013/12/11/windows-azure-storage-redundancy-options-and-read-access-geo-redundant-storage/)a bakın.

* Birincil ve ikincil uç noktalar arasında geçişin nasıl ileri ve geri yapılacağını gösteren eksiksiz bir örnek için, [Bkz. Azure Örnekleri – RA-GRS depolama alanı ile Devre Kesici Deseni'ni kullanma](https://github.com/Azure-Samples/storage-dotnet-circuit-breaker-pattern-ha-apps-using-ra-grs).
