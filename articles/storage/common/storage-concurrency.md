---
title: Eşzamanlılığı yönetme
titleSuffix: Azure Storage
description: Blob, Queue, Tablo ve Dosya hizmetleri için eşzamanlılığı nasıl yöneteceklerini öğrenin.
services: storage
author: tamram
ms.service: storage
ms.devlang: dotnet
ms.topic: article
ms.date: 12/20/2019
ms.author: tamram
ms.subservice: common
ms.openlocfilehash: 9879f98e72e22fc0745a9e91f29216cbe74ab8fe
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79255307"
---
# <a name="managing-concurrency-in-microsoft-azure-storage"></a>Microsoft Azure Depolamada Eşzamanlılığı Yönetme

Modern Internet tabanlı uygulamalar genellikle birden çok kullanıcının verileri aynı anda görüntülemesi ve güncelleştirmesi sağlar. Bu, uygulama geliştiricilerin, özellikle birden çok kullanıcının aynı verileri güncelleştirebileceği senaryolar için, son kullanıcılarına öngörülebilir bir deneyim sağlama konusunda dikkatlice düşünmelerini gerektirir. Geliştiricilerin genellikle göz önünde bulundurduğu üç ana veri eşzamanlılık stratejileri vardır:  

1. İyimser eşzamanlılık – Güncelleştirmenin bir parçası olarak bir güncelleştirme gerçekleştiren uygulama, uygulamanın bu verileri en son okuması nedeniyle verilerin değişip değişmediğini doğrular. Örneğin, bir wiki sayfasını görüntüleyen iki kullanıcı aynı sayfada bir güncelleştirme yaparsa, wiki platformu ikinci güncelleştirmenin ilk güncelleştirmenin üzerine yazmadığından ve her iki kullanıcının da güncelleştirmelerinin başarılı olup olmadığını anlamasını sağlamalıdır. Bu strateji en sık web uygulamalarında kullanılır.
2. Kötümser eşzamanlılık – Güncelleştirme gerçekleştirmek isteyen bir uygulama, kilit serbest bırakılıkadar diğer kullanıcıların verileri güncelleştirmesini engelleyen bir nesneyi kilitler. Örneğin, yalnızca asıl ın güncelleştirmeleri gerçekleştireceği bir ana/alt veri çoğaltma senaryosunda, ana bilgisayar genellikle verileri başka kimsenin güncelleştirmediğinden emin olmak için belirli bir süre boyunca özel bir kilit tutar.
3. Son yazar kazanır – Uygulama verileri ilk okuduğundan beri başka bir uygulamanın verileri güncelleştirip güncelleştirdiğini doğrulamadan herhangi bir güncelleştirme işleminin devam etmesine olanak tanıyan bir yaklaşım. Bu strateji (veya resmi bir strateji nin olmaması) genellikle verilerin birden çok kullanıcının aynı verilere erişme olasılığı olmayacak şekilde bölümlere aktarıldığı durumlarda kullanılır. Kısa ömürlü veri akışlarının işlendiği yerlerde de yararlı olabilir.  

Bu makalede, Azure Depolama platformunun bu üç eşzamanlılık stratejileri için birinci sınıf destek sağlayarak geliştirmeyi nasıl basitleştirildiğine genel bir bakış sağlanmaktadır.  

## <a name="azure-storage-simplifies-cloud-development"></a>Azure Depolama bulut geliştirmeyi kolaylaştırır

Azure depolama hizmeti, iyimser ve kötümser eşzamanlılık için tam destek sağlama yeteneğinde ayırt edici olsa da, üç stratejiyi de destekler, çünkü Depolama hizmeti, bir veri ekleme veya güncelleştirme işlemi taahhüt eder, bu verilere diğer tüm erişimler en son güncelleştirmeyi görür. Nihai tutarlılık modeli kullanan depolama platformları, bir yazmanın bir kullanıcı tarafından gerçekleştirilmesi ile güncelleştirilmiş verilerin diğer kullanıcılar tarafından görülebilmesi ile tutarsızlıkları önlemek için istemci uygulamalarının geliştirilmesini zorlaştırdığı zaman arasında bir gecikmeye sahiptir. son kullanıcıları etkilemesi.  

Uygun bir eşzamanlılık stratejisi seçmenin yanı sıra geliştiriciler, bir depolama platformunun değişiklikleri nasıl yalıtdığını da bilmelidir – özellikle de hareketler arasında aynı nesnedeki değişiklikler. Azure depolama hizmeti, okuma işlemlerinin tek bir bölüm içindeki yazma işlemleriyle aynı anda gerçekleşmesine izin vermek için anlık görüntü yalıtımı kullanır. Diğer yalıtım düzeylerinden farklı olarak, anlık görüntü yalıtımı, güncelleştirmeler gerçekleşirken bile tüm okumaların verilerin tutarlı bir anlık görüntüsünü görmesini garanti eder – esasen bir güncelleştirme işlemi işlenirken işlenen son değerleri döndürerek.  

## <a name="managing-concurrency-in-blob-storage"></a>Blob depolama biriminde eşzamanlılık yönetimi

Blob hizmetindeki blob ve konteynerlere erişimi yönetmek için iyimser veya kötümser eşzamanlılık modellerini kullanmayı tercih edebilirsiniz. Açıkça bir strateji belirtmezseniz son yazıyor kazanır varsayılandır.  

### <a name="optimistic-concurrency-for-blobs-and-containers"></a>Lekeler ve kaplar için iyimser eşzamanlılık

Depolama hizmeti depolanan her nesneye bir tanımlayıcı atar. Bu tanımlayıcı, bir nesne üzerinde her güncelleştirme işlemi gerçekleştirilinin güncelleştirilmesinde güncelleştirilir. Tanımlayıcı, HTTP protokolü içinde tanımlanan ETag (entity tag) üstbilgisini kullanarak bir HTTP GET yanıtının bir parçası olarak istemciye döndürülür. Böyle bir nesne üzerinde güncelleştirme gerçekleştiren bir kullanıcı, yalnızca belirli bir koşul yerine getirildiğinde güncelleştirmenin gerçekleşeceğinden emin olmak için koşullu bir üstbilgiyle birlikte orijinal ETag'ı gönderebilir – bu durumda durum Depolama Hizmeti gerektiren bir "If-Match" üstbilgisidir güncelleştirme isteğinde belirtilen ETag değerinin Depolama Hizmeti'nde depolananla aynı olduğundan emin olmak için.  

Bu işlemin ana hattı aşağıdaki gibidir:  

1. Depolama hizmetinden bir leke alın, yanıt depolama hizmetinde nesnenin geçerli sürümünü tanımlayan bir HTTP ETag Üstbilgi değeri içerir.
2. Blob'u güncellediğinizde, hizmete gönderdiğiniz isteğin **If-Match** koşullu üstbilgisine adım 1'de aldığınız ETag değerini ekleyin.
3. Hizmet, istekteki ETag değerini blob'un geçerli ETag değeriyle karşılaştırır.
4. Blob'un geçerli ETag değeri, istekteki **If-Match** koşullu üstbilgideki ETag'tan farklı bir sürümse, hizmet istemciye 412 hatası döndürür. Bu, istemci geri aldığından beri başka bir işlemin blob güncelleştirdiğini istemciye gösterir.
5. Blob'un geçerli ETag değeri, istekteki **If-Match** koşullu üstbilgideki ETag ile aynı sürümse, hizmet istenen işlemi gerçekleştirir ve yeni bir sürüm oluşturduğunu göstermek için blob'un geçerli ETag değerini güncelleştirir.  

Aşağıdaki C# snippet (İstemci Depolama Kitaplığı 4.2.0 kullanarak) daha önce alınan veya eklenen bir blob özelliklerinden erişilen ETag değerine dayalı bir **If-Match AccessCondition** oluşturmak için nasıl basit bir örnek gösterir. Daha sonra blob güncelleştirir **AccessCondition** nesnesi kullanır: **AccessCondition** nesnesi istek için **If-Match** üstbilgi ekler. Başka bir işlem blob güncelleştirilmişvarsa, Blob hizmeti bir HTTP 412 (ÖnKoşul Başarısız) durum iletisi döndürür. Tam örneği buradan indirebilirsiniz: [Azure Depolama'yı kullanarak Eşzamanlılığı Yönetme.](https://code.msdn.microsoft.com/Managing-Concurrency-using-56018114)  

```csharp
// Retrieve the ETag from the newly created blob
// Etag is already populated as UploadText should cause a PUT Blob call
// to storage Blob service which returns the ETag in response.
string originalETag = blockBlob.Properties.ETag;

// This code simulates an update by a third party.
string helloText = "Blob updated by a third party.";

// No ETag provided so original blob is overwritten (thus generating a new ETag)
blockBlob.UploadText(helloText);
Console.WriteLine("Blob updated. Updated ETag = {0}",
blockBlob.Properties.ETag);

// Now try to update the blob using the original ETag provided when the blob was created
try
{
    Console.WriteLine("Trying to update blob using original ETag to generate if-match access condition");
    blockBlob.UploadText(helloText,accessCondition:
    AccessCondition.GenerateIfMatchCondition(originalETag));
}
catch (StorageException ex)
{
    if (ex.RequestInformation.HttpStatusCode == (int)HttpStatusCode.PreconditionFailed)
    {
        Console.WriteLine("Precondition failure as expected. Blob's original ETag no longer matches");
        // TODO: client can decide on how it wants to handle the 3rd party updated content.
    }
    else
        throw;
}  
```

Azure Depolama, **If-Modified-Since**, **If-Unmodified-Since** ve **If-None-Match** gibi ek koşullu üstbilgileri ve bunların kombinasyonları için destek de içerir. Daha fazla bilgi için bkz: [Blob Servis İşlemleri için Koşullu Üstbilgileri Belirtme.](https://msdn.microsoft.com/library/azure/dd179371.aspx)  

Aşağıdaki tablo, istekte **If-Match** gibi koşullu üstbilgi kabul eden ve yanıtta ETag değeri döndüren kapsayıcı işlemlerini özetlemektedir.  

| İşlem | Konteyner ETag değerini döndürür | Koşullu üstbilgi kabul eder |
|:--- |:--- |:--- |
| Kapsayıcı Oluşturma |Evet |Hayır |
| Konteyner Özelliklerini Al |Evet |Hayır |
| Kapsayıcı Meta verilerini alın |Evet |Hayır |
| Kapsayıcı Meta verilerini ayarla |Evet |Evet |
| Konteyner ACL alın |Evet |Hayır |
| Konteyner ACL'yi Ayarla |Evet |Evet (*) |
| Kapsayıcıyı Silme |Hayır |Evet |
| Kira Konteyneri |Evet |Evet |
| Blobs listesi |Hayır |Hayır |

(*) SetContainerACL tarafından tanımlanan izinler önbelleğe alınır ve bu izinlerde güncelleştirmelerin, dönem güncelleştirmelerinin tutarlı olmasının garanti olmadığı süre boyunca yayılması 30 saniye sürer.  

Aşağıdaki tablo, istekte **If-Match** gibi koşullu üstbilgi kabul eden ve yanıtta Bir ETag değeri döndüren blob işlemlerini özetler.

| İşlem | ETag değerini döndürür | Koşullu üstbilgi kabul eder |
|:--- |:--- |:--- |
| İkili Büyük Nesne Koyma |Evet |Evet |
| Blob alın |Evet |Evet |
| BLOB özelliklerini Al |Evet |Evet |
| Blob Özelliklerini Ayarla |Evet |Evet |
| Blob Meta verilerini alın |Evet |Evet |
| Blob Meta verilerini ayarlama |Evet |Evet |
| Kiralık Blob (*) |Evet |Evet |
| İkili Büyük Nesne Anlık Görüntüsünü Alma |Evet |Evet |
| İkili Büyük Nesneyi Kopyalama |Evet |Evet (kaynak ve hedef blob için) |
| Abort Kopya Blob |Hayır |Hayır |
| İkili Büyük Nesneyi Silme |Hayır |Evet |
| Blok Koy |Hayır |Hayır |
| Blok Listesi Koy |Evet |Evet |
| Blok Listesini Al |Evet |Hayır |
| Sayfa Koy |Evet |Evet |
| Sayfa Aralıklarını Alın |Evet |Evet |

(*) Kiralık Blob bir blob üzerinde ETag değiştirmez.  

### <a name="pessimistic-concurrency-for-blobs"></a>Lekeler için kötümser eşzamanlılık

Özel kullanım için bir blob kilitlemek için, üzerinde bir [kira](https://msdn.microsoft.com/library/azure/ee691972.aspx) edebilirsiniz. Bir kira satın aldığınızda, kiraya ne kadar süre yle ihtiyacınız olduğunu belirtirsiniz: bu, özel bir kilit anlamına gelen 15 ila 60 saniye veya sonsuz arasında olabilir. Uzatmak için sınırlı bir kirayı yenileyebilir ve bu kirayı bitirdiğinizde herhangi bir kirayı serbest bırakabilirsiniz. Blob hizmeti, sonlu kiraların süresi dolduğunda otomatik olarak serbest bırakır.  

Kiralama, özel yazma / paylaşılan okuma, özel yazma / özel okuma ve paylaşılan yazma / özel okuma dahil olmak üzere farklı senkronizasyon stratejilerinin desteklenmesini sağlar. Bir kira varsa depolama hizmeti özel yazmaları (koyma, ayarlama ve silme işlemleri) uygular, ancak okuma işlemleri için münhasırlık sağlamak, geliştiricinin tüm istemci uygulamalarının bir kira kimliği kullanmasını ve aynı anda yalnızca bir istemcinin geçerli kira kimliği. Paylaşılan okumalarda kira kimliği sonucunu içermeyen işlemleri okuyun.  

Aşağıdaki C# snippet, bir blob üzerinde 30 saniye boyunca özel bir kira satın alma, blob içeriğini güncelleme ve sonra kira serbest bir örnek gösterir. Yeni bir kira elde etmeye çalıştığınızda blob üzerinde geçerli bir kira zaten varsa, Blob hizmeti bir "HTTP (409) Çakışma" durum sonucu döndürür. Aşağıdaki snippet, depolama hizmetinde blob güncelleştirmek için bir istekte bulununca kira bilgilerini kapsüllemek için bir **AccessCondition** nesnesi kullanır.  Tam örneği buradan indirebilirsiniz: [Azure Depolama'yı kullanarak Eşzamanlılığı Yönetme.](https://code.msdn.microsoft.com/Managing-Concurrency-using-56018114)

```csharp
// Acquire lease for 15 seconds
string lease = blockBlob.AcquireLease(TimeSpan.FromSeconds(15), null);
Console.WriteLine("Blob lease acquired. Lease = {0}", lease);

// Update blob using lease. This operation will succeed
const string helloText = "Blob updated";
var accessCondition = AccessCondition.GenerateLeaseCondition(lease);
blockBlob.UploadText(helloText, accessCondition: accessCondition);
Console.WriteLine("Blob updated using an exclusive lease");

//Simulate third party update to blob without lease
try
{
    // Below operation will fail as no valid lease provided
    Console.WriteLine("Trying to update blob without valid lease");
    blockBlob.UploadText("Update without lease, will fail");
}
catch (StorageException ex)
{
    if (ex.RequestInformation.HttpStatusCode == (int)HttpStatusCode.PreconditionFailed)
        Console.WriteLine("Precondition failure as expected. Blob's lease does not match");
    else
        throw;
}  
```

Kira kimliği olmadan kiralanan bir blob üzerinde yazma işlemi yapmaya çalışırsanız, istek 412 hatasıyla başarısız olur. **UploadText** yöntemini aramadan önce kira süresi doluyorsa ancak yine de kira kimliğini geçiyorsanız, istek **412** hatasıyla da başarısız olur. Kira süresi nin sona erme sürelerini ve kira belgelerini yönetme hakkında daha fazla bilgi için [Lease Blob](https://msdn.microsoft.com/library/azure/ee691972.aspx) REST belgelerine bakın.  

Aşağıdaki blob işlemleri kötümser eşzamanlılığı yönetmek için kiralamak kullanabilirsiniz:  

* İkili Büyük Nesne Koyma
* Blob alın
* BLOB özelliklerini Al
* Blob Özelliklerini Ayarla
* Blob Meta verilerini alın
* Blob Meta verilerini ayarlama
* İkili Büyük Nesneyi Silme
* Blok Koy
* Blok Listesi Koy
* Blok Listesini Al
* Sayfa Koy
* Sayfa Aralıklarını Alın
* Snapshot Blob - kira varsa isteğe bağlı kira kimliği
* Blob'u kopyala - varış yerinde kira varsa kira kimliği gereklidir
* Abort Copy Blob - varış noktasında sonsuz bir kira varsa kira kimliği gereklidir
* Blob Kiralama  

### <a name="pessimistic-concurrency-for-containers"></a>Konteynerler için kötümser eşzamanlılık

Kapsayıcılar üzerinde Kiralama blobs (özel yazma / paylaşılan okuma, özel yazma / özel okuma / özel okuma / özel okuma) ancak lekeler aksine depolama hizmeti sadece silme işlemleri münhasırlık uygular aynı senkronizasyon stratejileri desteklenmesini sağlar. Etkin bir kira ile bir kapsayıcı silmek için, istemci silme isteği ile etkin kira kimliği içermelidir. Diğer tüm konteyner işlemleri, kira kimliği dahil edilmeden kiralanan bir kapsayıcıda başarılı olur ve bu durumda işlemler paylaşılır. Güncelleştirme (koyma veya ayarlama) veya okuma işlemlerinin münhasırlık olması gerekiyorsa, geliştiriciler tüm istemcilerin bir kira kimliği kullandığından ve aynı anda yalnızca bir istemcinin geçerli bir kira kimliğine sahip olduğundan emin olmalıdır.  

Aşağıdaki kapsayıcı işlemleri kötümser eşzamanlılığı yönetmek için kiralamaları kullanabilir:  

* Kapsayıcıyı Silme
* Konteyner Özelliklerini Al
* Kapsayıcı Meta verilerini alın
* Kapsayıcı Meta verilerini ayarla
* Konteyner ACL alın
* Konteyner ACL'yi Ayarla
* Kira Konteyneri  

Daha fazla bilgi için bkz.  

* [Blob Hizmeti İşlemlerinde Koşullu Üst Bilgiler Belirtme](https://msdn.microsoft.com/library/azure/dd179371.aspx)
* [Kira Konteyneri](https://msdn.microsoft.com/library/azure/jj159103.aspx)
* [Blob Kiralama](https://msdn.microsoft.com/library/azure/ee691972.aspx)

## <a name="managing-concurrency-in-table-storage"></a>Tablo depolamasında eşzamanlılık yönetimi

Tablo hizmeti, açıkça iyimser eşzamanlılık denetimleri gerçekleştirmeyi seçmeniz gereken Blob hizmetinin aksine, varlıklarla çalışırken varsayılan davranış olarak iyimser eşzamanlılık denetimleri kullanır. Tablo ve Blob hizmetleri arasındaki diğer fark, yalnızca varlıkların eşzamanlılık davranışını yönetebiliyor, Blob hizmeti yle ise hem kapsayıcıların hem de blobların eşzamanlılıklarını yönetebiliyor olabilirsiniz.  

İyimser eşzamanlılık kullanmak ve tablo depolama hizmetinden aldığınızdan beri başka bir işlemin varlığı değiştirip değiştirip değiştirip değiştirip değiştirip değiştirip değiştirip değiştirip değiştirip değiştirip değiştirip kaldırmadığınızı kontrol etmek için, tablo hizmeti bir varlığı döndürdüğünde aldığınız ETag değerini kullanabilirsiniz. Bu işlemin ana hattı aşağıdaki gibidir:  

1. Tablo depolama hizmetinden bir varlık alın, yanıt depolama hizmetinde bu varlıkla ilişkili geçerli tanımlayıcıyı tanımlayan bir ETag değeri içerir.
2. Varlığı güncellediğinizde, hizmete gönderdiğiniz isteğin zorunlu **If-Match** üstbilgisine adım 1'de aldığınız ETag değerini ekleyin.
3. Hizmet, istekteki ETag değerini varlığın geçerli ETag değeriyle karşılaştırır.
4. Varlığın geçerli ETag değeri istekteki zorunlu **If-Match** üstbilgindeki ETag'tan farklıysa, hizmet istemciye 412 hatası döndürür. Bu, istemci nin varlığı geri alacağından beri başka bir işlemin güncelleştirdiğini istemciye gösterir.
5. Varlığın geçerli ETag değeri, istekteki zorunlu **If-Match** üstbilgindeki ETag ile aynıysa veya **If-Match** üstbilgisinde joker karakter (*) varsa, hizmet istenen işlemi gerçekleştirir ve varlığın geçerli ETag değerini güncelleştirdiğini göstermek için güncelleştirir.  

Blob hizmetinin aksine, tablo hizmetinin istemcinin güncelleştirme isteklerine bir **If-Match** üstbilgi eklemesini gerektirdiğini unutmayın. Ancak, istemci İstekteki **If-Match** üstbilgisini joker karaktere (*) ayarlarsa, koşulsuz güncelleştirmeyi zorlamak (son yazar strateji kazanır) ve eşzamanlılık denetimlerini atlamak mümkündür.  

Aşağıdaki C# snippet, e-posta adresleri güncelleştirildikten sonra daha önce oluşturulmuş veya alınan bir müşteri varlığını gösterir. İlk ekleme veya alma işlemi ETag değerini müşteri nesnesinde depolar ve örnek değiştirme işlemini yürütürken aynı nesne örneğini kullandığından, ETag değerini otomatik olarak tablo hizmetine geri gönderir ve hizmetin eşzamanlılık ihlalleri için kontrol edin. Başka bir işlem tablo depolama varlığı güncelleştirilmişvarsa, hizmet bir HTTP 412 (ÖnKoşul Başarısız) durum iletisi döndürür.  Tam örneği buradan indirebilirsiniz: [Azure Depolama'yı kullanarak Eşzamanlılığı Yönetme.](https://code.msdn.microsoft.com/Managing-Concurrency-using-56018114)

```csharp
try
{
    customer.Email = "updatedEmail@contoso.org";
    TableOperation replaceCustomer = TableOperation.Replace(customer);
    customerTable.Execute(replaceCustomer);
    Console.WriteLine("Replace operation succeeded.");
}
catch (StorageException ex)
{
    if (ex.RequestInformation.HttpStatusCode == 412)
        Console.WriteLine("Optimistic concurrency violation – entity has changed since it was retrieved.");
    else
        throw;
}  
```

Eşzamanlılık denetimini açıkça devre dışı bırakabilmek için, değiştirme işlemini gerçekleştirmeden önce **çalışan** nesnesinin **ETag** özelliğini "*" olarak ayarlamanız gerekir.  

```csharp
customer.ETag = "*";  
```

Aşağıdaki tablo, tablo varlık işlemlerinin ETag değerlerini nasıl kullandığını özetlemektedir:

| İşlem | ETag değerini döndürür | If-Match istek üstbilgisi gerektirir |
|:--- |:--- |:--- |
| Sorgu Varlıkları |Evet |Hayır |
| Varlık Ekle |Evet |Hayır |
| Varlığı Güncelleştir |Evet |Evet |
| Varlığı Birleştir |Evet |Evet |
| Varlığı Sil |Hayır |Evet |
| Varlık Ekleme veya Değiştirme |Evet |Hayır |
| Varlık Ekleme veya Birleştirme |Evet |Hayır |

Tablo hizmetine ETag değeri göndermedikleri için Varlık Ekle **veya Değiştir** ve Varlık Ekle **veya Birleştirme** işlemlerinin eşzamanlılık denetimleri *gerçekleştirmediğini* unutmayın.  

Genel olarak tabloları kullanan geliştiriciler ölçeklenebilir uygulamalar geliştirirken iyimser eşzamanlılık güvenmek gerekir. Kötümser kilitleme gerekiyorsa, tablolara erişirken geliştiricilerin alabilecekleri bir yaklaşım, her tablo için belirlenmiş bir blob atamak ve masada çalışmadan önce blob'u kiralamaya çalışmaktır. Bu yaklaşım, tüm veri erişim yollarının tabloüzerinde çalışmadan önce kira elde sağlamak için uygulama gerektirir. Ayrıca, minimum kira süresiölçeklenebilirlik için dikkatli dikkate gerektiren 15 saniye olduğunu unutmayın.  

Daha fazla bilgi için bkz.  

* [Varlıklar üzerindeki işlemler](https://msdn.microsoft.com/library/azure/dd179375.aspx)  

## <a name="managing-concurrency-in-the-queue-service"></a>Kuyruk Hizmetinde Eşzamanlılık Yönetimi

Eşzamanlılık sıralama hizmetinde bir sorun olduğu bir senaryo, birden çok istemcinin bir kuyruktan iletileri aldığı durumdur. Bir ileti kuyruktan alındığı zaman, yanıt iletiyi ve iletiyi silmek için gereken bir pop makbuz değerini içerir. İleti kuyruktan otomatik olarak silinmez, ancak alındıktan sonra görünürlük süresi parametresi tarafından belirtilen zaman aralığı için diğer istemciler tarafından görünmez. İletiyi alan istemcinin, ileti işlendikten sonra ve görünürlük süresi son dakika parametresinin değerine göre hesaplanan yanıtın TimeNextVisible öğesi tarafından belirtilen süreden önce silmesi beklenir. Görünürlük süresinin değeri, TimeNextVisible değerini belirlemek için iletinin alındığı zamana eklenir.  

Kuyruk hizmetinin iyimser veya kötümser eşzamanlılık için desteği yoktur ve bu nedenle, bir kuyruktan alınan iletileri işleyen istemciler iletilerin idempotent bir şekilde işlenmesini sağlamalıdır. SetQueueServiceProperties, SetQueueMetaData, SetQueueACL ve UpdateMessage gibi güncelleştirme işlemleri için son bir yazar kazanır stratejisi kullanılır.  

Daha fazla bilgi için bkz.  

* [Kuyruk Hizmeti REST API'si](https://msdn.microsoft.com/library/azure/dd179363.aspx)
* [İletileri Alın](https://msdn.microsoft.com/library/azure/dd179474.aspx)  

## <a name="managing-concurrency-in-azure-files"></a>Azure Dosyalarında eşzamanlılık yönetme

Dosya hizmetine iki farklı protokol uç noktası olan SMB ve REST kullanılarak erişilebilir. REST hizmeti ya iyimser kilitleme veya kötümser kilitleme için destek yok ve tüm güncellemeleri son bir yazar strateji kazanır takip edecektir. Dosya paylaşımlarını monte eden SMB istemcileri, paylaşılan dosyalara erişimi yönetmek için kötümser kilitleme özelliği de dahil olmak üzere dosya sistemi kilitleme mekanizmalarından yararlanabilir. Bir SMB istemcisi bir dosyayı açtığında, hem dosya erişimini hem de paylaşım modunu belirtir. "Yazma" veya "Oku/Yaz" seçeneğinin yanı sıra "Yok" Dosya Paylaşımı modunun ayarlanması, dosya kapatılana kadar dosyanın bir Kobİ istemcisi tarafından kilitlenmesiyle sonuçlanır. Bir SMB istemcisinin dosyayı kilitlediği bir dosyada REST işlemi denenirse, REST hizmeti hata kodu SharingViolation ile durum kodu 409 (Çakışma) döndürecektir.  

Bir SMB istemcisi silme için bir dosya açtığında, dosyayı bekleyen silme olarak işaretler, bu dosyadaki diğer tüm SMB istemcisi açık iş kolları kapatılana kadar. Bir dosya bekleyen silme olarak işaretlenmiş olsa da, bu dosyadaki herhangi bir REST işlemi durum kodu 409 (Çakışma) hata kodu SMBDeletePending ile döndürecek. Durum kodu 404 (Bulunamadı), SMB istemcisinin dosyayı kapatmadan önce bekleyen silme bayrağını kaldırması mümkün olduğundan döndürülmez. Başka bir deyişle, durum kodu 404 (Bulunamadı) yalnızca dosya kaldırıldığında beklenir. Bir dosya silmek için bekleyen bir SMB'de yken, Liste Dosyaları sonuçlarına dahil edilmeyeceğini unutmayın. Ayrıca, REST Delete File ve REST Delete Directory işlemlerinin atomik olarak işlendiğini ve bekleyen silme durumuna neden olmadığını unutmayın.  

Daha fazla bilgi için bkz.  

* [Dosya Kilitlerini Yönetme](https://msdn.microsoft.com/library/azure/dn194265.aspx)  

## <a name="next-steps"></a>Sonraki adımlar

Bu blogda başvurulan tam örnek uygulama için:  

* [Azure Depolama'yı kullanarak Eşzamanlılık Yönetimi - Örnek Uygulama](https://code.msdn.microsoft.com/Managing-Concurrency-using-56018114)  

Azure Depolama hakkında daha fazla bilgi için bkz:  

* [Microsoft Azure Depolama Giriş Sayfası](https://azure.microsoft.com/services/storage/)
* [Azure Depolama’ya giriş](storage-introduction.md)
* [Blob,](../blobs/storage-dotnet-how-to-use-blobs.md) [Tablo,](../../cosmos-db/table-storage-how-to-use-dotnet.md) [Kuyruklar](../storage-dotnet-how-to-use-queues.md)ve [Dosyalar](../storage-dotnet-how-to-use-files.md) için Depolama Başlarken
* Depolama Mimarisi – [Azure Depolama: Güçlü Tutarlılığa Sahip Yüksek Kullanılabilirlik](https://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx) Li Bulut Depolama Hizmeti

