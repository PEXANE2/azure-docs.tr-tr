---
title: Eşzamanlılığı yönetme
titleSuffix: Azure Storage
description: Blob, kuyruk, tablo ve dosya hizmetleri için Azure Storage 'da eşzamanlılık yönetimi hakkında bilgi edinin. Kullanılan üç ana veri eşzamanlılık stratejilerini anlayın.
services: storage
author: tamram
ms.service: storage
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 12/20/2019
ms.author: tamram
ms.subservice: common
ms.custom: devx-track-csharp
ms.openlocfilehash: ac54282135759f14f17ed16b9779013f849bd8d7
ms.sourcegitcommit: 3bcce2e26935f523226ea269f034e0d75aa6693a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2020
ms.locfileid: "92488682"
---
# <a name="managing-concurrency-in-microsoft-azure-storage"></a>Microsoft Azure Depolamada Eşzamanlılığı Yönetme

Modern Internet tabanlı uygulamalarda, verileri eşzamanlı olarak görüntüleyen ve güncelleştiren birden fazla kullanıcı vardır. Uygulama geliştiricilerinin, özellikle birden çok kullanıcının aynı verileri güncelleştirebileceği, öngörülebilir bir kullanıcı deneyimi sağlama konusunda dikkatli olması gerekir. Geliştiricilerin genellikle göz önünde bulundurulması gereken üç ana veri eşzamanlılık stratejisi vardır:

1. İyimser eşzamanlılık – güncelleştirme yapan bir uygulama, güncelleştirmesinin bir parçası olarak, uygulamanın o verileri en son okuduğundan bu yana verilerin değişip değişmediğini doğrular. Örneğin, bir wiki sayfasını görüntüleyen iki kullanıcı aynı sayfada bir güncelleştirme yapmazsa, wiki platformu ikinci güncelleştirmenin ilk güncelleştirmenin üzerine yazmadığından ve her iki kullanıcının da güncelleştirmesinin başarılı olup olmadığını anlamalarına emin olmalıdır. Bu strateji çoğu zaman Web uygulamalarında kullanılır.
2. Kötümser eşzamanlılık: bir güncelleştirmeyi yapmak isteyen bir uygulama, nesne üzerinde bir kilit alır ve bu, kilit yayımlanıncaya kadar diğer kullanıcıların verileri güncelleştirmesini önler.
3. Son yazıcı WINS – tüm güncelleştirme işlemlerinin, uygulamanın verileri ilk okumasından bu yana verileri güncelleştirmiş olup olmadığını doğrulamadan devam etmesine izin veren bir yaklaşım. Bu strateji genellikle veri bölümlenmiş olduğunda kullanılır, bu nedenle birden çok kullanıcının aynı verilere erişmesi olasılığı yoktur. Kısa süreli veri akışlarının işlendiği durumlar da yararlı olabilir.

Bu makalede, Azure depolama 'nın bu eşzamanlılık stratejilerinin hepsini destekleyerek geliştirme konusunda nasıl basitleşmesine ilişkin bir genel bakış sunulmaktadır.

## <a name="azure-storage-simplifies-cloud-development"></a>Azure depolama, bulut geliştirmeyi basitleştirir

Azure depolama, tüm üç stratejiyi destekler, ancak iyimser ve Kötümser eşzamanlılık için tam destek sağlama yeteneğine de sahiptir. Azure depolama, güçlü bir tutarlılık modelini bir araya getirirken bir veri ekleme veya güncelleştirme işlemi tamamlandığında bu verilere daha fazla erişim en son güncelleştirmeyi görecek şekilde tasarlanmıştır. Son tutarlılık modelini kullanan depolama platformları, bir kullanıcı tarafından bir yazma işlemi gerçekleştirildiğinde ve güncelleştirilmiş veriler diğer kullanıcılar tarafından görülebileceği zaman arasında bir gecikme olur.

Geliştiriciler ayrıca, depolama platformunun bir değişikliği nasıl yalıtdığının farkında olmalıdır. özellikle de işlemler arasında aynı nesneye yapılan değişiklikler. Azure depolama hizmeti, okuma işlemlerinin tek bir bölüm içindeki yazma işlemleriyle aynı anda gerçekleşmesini sağlamak için anlık görüntü yalıtımını kullanır. Diğer yalıtım düzeylerinden farklı olarak, anlık görüntü yalıtımı tüm okumaların, güncelleştirmeler gerçekleşirken bile verilerin tutarlı bir anlık görüntüsünü (aslında bir güncelleştirme işlemi işlenirken, son kaydedilen değerleri döndürerek) görmesini güvence altına alır.

## <a name="managing-concurrency-in-blob-storage"></a>Blob depolamada eşzamanlılık yönetimi

Blob hizmetindeki bloblara ve kapsayıcılara erişimi yönetmek için iyimser veya Kötümser eşzamanlılık modellerini kullanmayı tercih edebilirsiniz. Açık olarak bir strateji belirtmezseniz, en son WINS yazma varsayılandır.

### <a name="optimistic-concurrency-for-blobs-and-containers"></a>Bloblar ve kapsayıcılar için iyimser eşzamanlılık

Depolama hizmeti, depolanan her nesneye bir tanımlayıcı atar. Bu tanımlayıcı, bir nesne üzerinde her güncelleştirme işlemi yapıldığında güncellenir. Tanımlayıcı, HTTP protokolünde tanımlanmış ETag (varlık etiketi) üst bilgisini kullanan HTTP GET yanıtının bir parçası olarak istemciye döndürülür. Bu tür bir nesne üzerinde bir güncelleştirme yapan bir Kullanıcı, bir güncelleştirmenin yalnızca belirli bir koşul karşılanırsa meydana geldiğinden emin olmak için bir koşullu üstbilgiyle birlikte özgün ETag 'e gönderebilir. Bu durumda koşul, güncelleştirme isteğinde belirtilen ETag değerinin depolama hizmetinde depolanmaya aynı olduğundan emin olmak için depolama hizmetini gerektiren bir "IF-Match" üst bilgisi olur.

Bu işlemin ana hattı aşağıdaki gibidir:

1. Depolama hizmetinden bir blob alma, yanıt, depolama hizmetindeki nesnenin geçerli sürümünü tanımlayan bir HTTP ETag üst bilgisi değeri içerir.
2. Blobu güncelleştirdiğinizde, hizmete göndereceğiniz isteğin **IF-Match** koşullu üst bilgisinde adım 1 ' de aldığınız ETag değerini ekleyin.
3. Hizmet, istekteki ETag değerini Blobun geçerli ETag değeriyle karşılaştırır.
4. Blob 'un geçerli ETag değeri istekteki **IF-Match** koşullu üstbilgisindeki ETag 'ten farklı bir sürümse, hizmet istemciye 412 hatası döndürür. Bu hata, istemcinin aldığı bu yana başka bir işlemin blobu güncelleştirdiğini istemciye bildirir.
5. Blob 'un geçerli ETag değeri, istekteki **IF-Match** koşullu üstbilgisindeki ETag ile aynı sürümse, hizmet istenen işlemi gerçekleştirir ve yeni bir sürüm oluşturduğunu göstermek için Blobun geçerli ETag değerini güncelleştirir.

Aşağıdaki C# kod parçacığı (Istemci depolama kitaplığı 4.2.0 kullanarak), daha önce alınmış veya eklenen bir Blobun özelliklerinden erişilen ETag değerine dayalı bir **IF-Match AccessCondition** öğesinin nasıl oluşturulacağı hakkında basit bir örnek gösterir. Ardından, blobu güncelleştirdiğinde **accesscondition** nesnesini kullanır: **accesscondition** nesnesi isteğe **IF-Match** üst bilgisini ekler. Başka bir işlem blobu güncelleştirdiyse, blob hizmeti bir HTTP 412 (Önkoşul başarısız) durum iletisi döndürür. Tam örneği buradan indirebilirsiniz: [Azure Storage kullanarak eşzamanlılık yönetimi](https://code.msdn.microsoft.com/Managing-Concurrency-using-56018114).

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

Azure depolama Ayrıca, **If-Modified-Since**, **If-Modified-Since**, **If-None-Match**ve bu üstbilgilerin birleşimleri gibi koşullu üstbilgiler için destek içerir. Daha fazla bilgi için bkz. [BLOB hizmeti işlemleri Için koşullu üstbilgiler belirtme](https://msdn.microsoft.com/library/azure/dd179371.aspx).

Aşağıdaki tablo, istekte **IF-Match** gibi koşullu üstbilgileri kabul eden ve yanıtta bir ETag değeri döndüren kapsayıcı işlemlerini özetler.

| İşlem | Kapsayıcı ETag değerini döndürür | Koşullu üstbilgileri kabul eder |
|:--- |:--- |:--- |
| Kapsayıcı Oluşturma |Yes |Hayır |
| Kapsayıcı özelliklerini al |Yes |Hayır |
| Kapsayıcı meta verilerini al |Yes |Hayır |
| Kapsayıcı meta verilerini ayarla |Yes |Yes |
| Kapsayıcı ACL 'sini al |Yes |Hayır |
| Kapsayıcı ACL 'sini ayarla |Yes |Evet (*) |
| Kapsayıcıyı Silme |Hayır |Yes |
| Kira kapsayıcısı |Yes |Yes |
| Blobları Listele |Hayır |Hayır |

(*) SetContainerACL tarafından tanımlanan izinler önbelleğe alınır ve bu izinlere yönelik güncelleştirmeler, zaman içindeki güncelleştirmelerin tutarlı olması garanti edilmediği için 30 saniye sürer.

Aşağıdaki tablo, istekte **IF-Match** gibi koşullu üstbilgileri kabul eden ve yanıtta bir ETag değeri döndüren blob işlemlerini özetler.

| İşlem | ETag değerini döndürür | Koşullu üstbilgileri kabul eder |
|:--- |:--- |:--- |
| İkili Büyük Nesne Koyma |Yes |Yes |
| Blob al |Yes |Yes |
| BLOB özelliklerini Al |Yes |Yes |
| Blob özelliklerini ayarla |Yes |Yes |
| Blob meta verilerini al |Yes |Yes |
| Blob meta verilerini ayarla |Yes |Yes |
| Kira Blobu (*) |Yes |Yes |
| İkili Büyük Nesne Anlık Görüntüsünü Alma |Yes |Yes |
| İkili Büyük Nesneyi Kopyalama |Yes |Evet (kaynak ve hedef blobu için) |
| Kopyalama blobu durdur |Hayır |Hayır |
| İkili Büyük Nesneyi Silme |Hayır |Yes |
| Yerleştirme bloğu |Hayır |Hayır |
| Öbek listesini yerleştirme |Yes |Yes |
| Engelleme listesini al |Yes |Hayır |
| Yerleştirme sayfası |Yes |Yes |
| Sayfa aralıklarını al |Yes |Yes |

(*) Kira blobu blob üzerinde ETag öğesini değiştirmez.

### <a name="pessimistic-concurrency-for-blobs"></a>Blob 'lar için Kötümser eşzamanlılık

Bir blobu dışlamalı kullanım için kilitlemek için bir [kira](https://msdn.microsoft.com/library/azure/ee691972.aspx) elde edin. Kira aldığınızda kira için bir zaman aralığı belirtirsiniz. Zaman aralığı, 15 ile 60 saniyeye veya sonsuz, özel bir kilit için olan değer olarak değişir. Sınırlı bir kirayı genişletmek için yenileyin. İşiniz bittiğinde bir kira yayınlayın. BLOB depolama süreleri dolduğunda sınırlı kiraları otomatik olarak serbest bırakır.

Kiralamalar, farklı eşitleme stratejilerinin desteklenme imkanı sağlar. Stratejiler arasında *özel yazma/paylaşılan okuma*, *dışlamalı yazma/özel okuma*ve *paylaşılan yazma/dışlamalı*okuma sayılabilir. Kira mevcut olduğunda, Azure Storage özel yazma işlemleri uygular (put, set ve DELETE işlemleri), ancak okuma işlemlerine yönelik denetim sağlamak, geliştiricinin tüm istemci uygulamalarının bir kira KIMLIĞI kullandığından ve aynı anda yalnızca bir istemcinin geçerli bir kira KIMLIĞI içerdiğinden emin olmasını gerektirir. Paylaşılan okumaların kira KIMLIĞI sonucunu içermeyen okuma işlemleri.

Aşağıdaki C# kod parçacığında bir blob üzerinde 30 saniye boyunca özel kiralama alma, Blobun içeriğini güncelleştirme ve ardından kirayı serbest bırakma örneği gösterilmektedir. Yeni bir kira almaya çalıştığınızda blob üzerinde zaten geçerli bir kira varsa, blob hizmeti "HTTP (409) çakışması" durum sonucunu döndürür. Aşağıdaki kod parçacığı, depolama hizmetindeki blobu güncellemek için bir istek yaptığında kira bilgilerini kapsüllemek için bir **Accesscondition** nesnesi kullanır.  Tam örneği buradan indirebilirsiniz: [Azure Storage kullanarak eşzamanlılık yönetimi](https://code.msdn.microsoft.com/Managing-Concurrency-using-56018114).

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

Kira KIMLIĞINI geçirmeden kiralanan bir blob üzerinde yazma işlemine çalışırsanız, istek 412 hatasıyla başarısız olur. , **Uploadtext** yöntemini çağırmadan önce kira süresi dolarsa ancak kira kimliğini hala geçirirseniz, istek da **412** hatasıyla başarısız olur. Kira süre sonu zamanlarını ve kira kimliklerini yönetme hakkında daha fazla bilgi için bkz. [kira blobu](https://msdn.microsoft.com/library/azure/ee691972.aspx) Rest belgeleri.

Aşağıdaki blob işlemleri, Kötümser eşzamanlılık 'yi yönetmek için kiraları kullanabilir:

* İkili Büyük Nesne Koyma
* Blob al
* BLOB özelliklerini Al
* Blob özelliklerini ayarla
* Blob meta verilerini al
* Blob meta verilerini ayarla
* İkili Büyük Nesneyi Silme
* Yerleştirme bloğu
* Öbek listesini yerleştirme
* Engelleme listesini al
* Yerleştirme sayfası
* Sayfa aralıklarını al
* Anlık görüntü blobu-Kiralama KIMLIĞI bir kira varsa isteğe bağlıdır
* Kopyalama blobu-hedef blobu üzerinde kira varsa, kira KIMLIĞI gereklidir
* Kopyalama blobu durdur-hedef blobu üzerinde sonsuz bir kira varsa, kira KIMLIĞI gereklidir
* Blob Kiralama

### <a name="pessimistic-concurrency-for-containers"></a>Kapsayıcılar için Kötümser eşzamanlılık

Kapsayıcılardaki kiralamalar aynı eşitleme stratejilerinin bloblarda (*özel yazma/paylaşılan okuma*, *dışlamalı yazma/özel okuma*ve *paylaşılan yazma/özel okuma*) aynı şekilde desteklendiğinden, ancak Blobların aksine depolama hizmeti yalnızca silme işlemlerinde denetim uygulanmasını sağlar. Etkin bir kiralamaya sahip bir kapsayıcıyı silmek için, istemci, silme isteğiyle birlikte etkin Kiralama KIMLIĞINI içermelidir. Diğer tüm kapsayıcı işlemleri, kira KIMLIĞINI dahil etmeden bir kiralanan kapsayıcıda başarılı olur ve bu durumda paylaşılan işlemler olur. Denetim of Update (put veya set) veya Read işlemleri gerekliyse, geliştiriciler tüm istemcilerin bir kira KIMLIĞI kullandığından ve aynı anda yalnızca bir istemcinin geçerli bir kira KIMLIĞI olduğundan emin olmalıdır.

Aşağıdaki kapsayıcı işlemleri, Kötümser eşzamanlılık 'yi yönetmek için kiraları kullanabilir:

* Kapsayıcıyı Silme
* Kapsayıcı özelliklerini al
* Kapsayıcı meta verilerini al
* Kapsayıcı meta verilerini ayarla
* Kapsayıcı ACL 'sini al
* Kapsayıcı ACL 'sini ayarla
* Kira kapsayıcısı

Daha fazla bilgi için bkz.

* [Blob Hizmeti İşlemlerinde Koşullu Üst Bilgiler Belirtme](https://msdn.microsoft.com/library/azure/dd179371.aspx)
* [Kira kapsayıcısı](https://msdn.microsoft.com/library/azure/jj159103.aspx)
* [Blob Kiralama](https://msdn.microsoft.com/library/azure/ee691972.aspx)

## <a name="managing-concurrency-in-table-storage"></a>Tablo depolamada eşzamanlılık yönetimi

Tablo hizmeti, iyimser eşzamanlılık denetimleri yapmak için açıkça seçmeniz gereken blob hizmetinden farklı olarak, varlıklarla çalışırken varsayılan davranış olarak iyimser eşzamanlılık denetimleri kullanır. Tablo ve BLOB Hizmetleri arasındaki diğer fark, yalnızca varlıkların eşzamanlılık davranışını yönetebilir, ancak blob hizmetiyle hem kapsayıcıların hem de Blobların Eşzamanlılığını yönetebilirsiniz.

İyimser eşzamanlılığı kullanmak ve tablo depolama hizmetinden aldıktan sonra başka bir işlemin bir varlığı değiştirse denetlemek için, tablo hizmeti bir varlık döndürdüğünde aldığınız ETag değerini kullanabilirsiniz. Bu işlemin ana hattı aşağıdaki gibidir:

1. Tablo depolama hizmetinden bir varlık alma, yanıt, depolama hizmetindeki bu varlıkla ilişkili geçerli tanımlayıcıyı tanımlayan bir ETag değeri içerir.
2. Varlığı güncelleştirdiğinizde, hizmete göndereceğiniz isteğin zorunlu **IF-Match** üst bilgisinde 1. adım ' da aldığınız ETag değerini ekleyin.
3. Hizmet, istekteki ETag değerini varlığın geçerli ETag değeri ile karşılaştırır.
4. Varlığın geçerli ETag değeri, istekteki zorunlu **IF-Match** üst bilgisinde ETag değerinden farklıysa, hizmet istemciye 412 hatası döndürür. Bu, istemcinin aldığı bu yana başka bir işlemin varlığı güncelleştirdiğini gösterir.
5. Varlığın geçerli ETag değeri, istekteki zorunlu **IF-Match** üst bilgisindeki ETag ile aynı veya **IF-Match** üst bilgisi joker karakteri (*) içeriyorsa, hizmet istenen işlemi yapar ve varlığın güncel ETag değerini güncelleştirir.

Tablo hizmeti, istemcinin güncelleştirme isteklerinde bir **IF-Match** üst bilgisi içermesini gerektirir. Ancak, istemci istekte **IF-Match** üst bilgisini (*) joker karakteri (*) olarak ayarladıysanız, koşulsuz bir güncelleştirme (son yazıcı WINS stratejisi) zorlamak ve eşzamanlılık denetimlerini atlamak mümkündür.

Aşağıdaki C# kod parçacığında, daha önce oluşturulmuş veya e-posta adresi güncelleştirildikten sonra alınan bir müşteri varlığı gösterilmektedir. İlk ekleme veya alma işlemi, ETag değerini Customer nesnesinde depolar ve örnek değiştirme işlemini yürütürken aynı nesne örneğini kullandığından, otomatik olarak ETag değerini tablo hizmetine geri gönderir ve hizmetin eşzamanlılık ihlallerini denetlemesini sağlar. Başka bir işlem, varlığı tablo depolamadaki güncelleştirdiyseniz, hizmet bir HTTP 412 (Önkoşul başarısız) durum iletisi döndürür.  Tam örneği buradan indirebilirsiniz: [Azure Storage kullanarak eşzamanlılık yönetimi](https://code.msdn.microsoft.com/Managing-Concurrency-using-56018114).

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

Eşzamanlılık denetimini açıkça devre dışı bırakmak için, Replace işlemini yürütmeden önce **Employee** nesnesinin **ETag** özelliğini "*" olarak ayarlamanız gerekir.

```csharp
customer.ETag = "*";
```

Aşağıdaki tablo, tablo varlığı işlemlerinin ETag değerlerini nasıl kullandığını özetler:

| İşlem | ETag değerini döndürür | If-Match istek üst bilgisi gerektiriyor |
|:--- |:--- |:--- |
| Sorgu varlıkları |Yes |Hayır |
| Varlık Ekle |Yes |Hayır |
| Varlığı Güncelleştir |Yes |Yes |
| Birleştirme varlığı |Yes |Yes |
| Varlığı Sil |Hayır |Yes |
| Varlık Ekle veya Değiştir |Yes |Hayır |
| Varlık ekleme veya birleştirme |Yes |Hayır |

**Varlık ekleme veya değiştirme** ve **varlık ekleme veya birleştirme** işlemlerinin, tablo hizmetine bir ETag değeri göndermediğinden hiçbir eşzamanlılık *denetimi yapmayın olmadığını* unutmayın.

Genel olarak, tabloları kullanan geliştiriciler iyimser eşzamanlılık kullanır. Tablolara erişirken Kötümser kilitleme gerekiyorsa, her tablo için seçili bir blob atayın ve tablo üzerinde çalışmadan önce bloba bir kira almaya çalışın. Bu yaklaşım, tüm veri erişim yollarının tablo üzerinde çalışmadan önce kirayı almasını sağlamak için uygulamayı gerektirir. Ayrıca, minimum kira süresinin 15 saniye olduğunu ve bu da ölçeklenebilirlik için dikkatli bir dikkat gerektiren bir değer görmeniz gerekir.

Daha fazla bilgi için bkz.

* [Varlıklar üzerinde işlemler](https://msdn.microsoft.com/library/azure/dd179375.aspx)

## <a name="managing-concurrency-in-the-queue-service"></a>Kuyruk hizmetinde eşzamanlılık yönetimi

Sıraya alma hizmetinde eşzamanlılık olan bir senaryo, birden çok istemcinin bir kuyruktan ileti aldığı yerdir. Kuyruktan bir ileti alındığında, yanıt iletiyi silmek için gereken iletiyi ve bir pop alındı değerini içerir. İleti kuyruktan otomatik olarak silinmez, ancak alındıktan sonra, görünürlük zaman aşımı parametresi tarafından belirtilen zaman aralığı için diğer istemcilere görünür değildir. İletiyi alan istemcinin, işleme alındıktan sonra iletiyi silmesi beklenir ve yanıt, görünürlük zaman aşımı parametresinin değeri temel alınarak hesaplanan, yanıtın TimeNextVisible öğesi tarafından belirtilen süreden önce. Görünürlük zaman aşımı değeri, iletinin alındığı zaman, TimeNextVisible değerini belirleyen zamana eklenir.

Sıra hizmeti, iyimser veya Kötümser eşzamanlılık desteğine sahip değildir ve bu nedenle, bir kuyruktan alınan iletilerin işlenmesi, iletilerin bir ıdempotent şekilde işlenmesini sağlamaktır. Son yazıcı WINS stratejisi, SetQueueServiceProperties, SetQueueMetaData, SetQueueACL ve UpdateMessage gibi güncelleştirme işlemleri için kullanılır.

Daha fazla bilgi için bkz.

* [Kuyruk Hizmeti REST API'si](https://msdn.microsoft.com/library/azure/dd179363.aspx)
* [Iletileri al](https://msdn.microsoft.com/library/azure/dd179474.aspx)

## <a name="managing-concurrency-in-azure-files"></a>Azure dosyalarında eşzamanlılık yönetimi

Dosya hizmetine iki farklı protokol uç noktası, SMB ve REST kullanılarak erişilebilir. REST hizmeti, iyimser kilitleme veya Kötümser kilitleme desteğine sahip değildir ve tüm güncelleştirmeler son yazıcı WINS stratejisini takip eder. Dosya paylaşımlarını bağlayan SMB istemcileri, Kötümser kilitleme gerçekleştirme özelliği de dahil olmak üzere paylaşılan dosyalara erişimi yönetmek için dosya sistemi kilitleme mekanizmalarını kullanabilir. Bir SMB istemcisi bir dosyayı açtığında, hem dosya erişimi hem de paylaşma modunu belirtir. "Write" veya "Read/Write" adlı bir dosya erişimi seçeneğinin ve "none" dosya paylaşma moduyla birlikte ayarlanması, dosya kapatılıncaya kadar dosyanın bir SMB istemcisi tarafından kilitlenme oluşmasına neden olur. Bir SMB istemcisinin dosya kilitli olduğu bir dosya üzerinde REST işlemi deneniyorsa REST hizmeti, 409 (çakışma) durum kodunu bir hata kodu Sharingihlayle döndürür.

Bir SMB istemcisi silme için bir dosyayı açtığında, bu dosyadaki diğer tüm SMB istemci açık tanıtıcıları kapanana kadar dosyayı bekleyen silme olarak işaretler. Bir dosya bekleyen silme olarak işaretlendiğinden, bu dosyadaki tüm REST işlemleri SMBDeletePending hata kodu ile 409 (çakışma) durum kodunu döndürür. SMB istemcisinin dosyayı kapatmadan önce bekleyen silme bayrağını kaldırması mümkün olduğundan, durum kodu 404 (bulunamadı) döndürülmedi. Diğer bir deyişle, 404 (bulunamadı) durum kodu yalnızca dosya kaldırıldığında beklenir. Bir dosya SMB bekleyen silme durumundaysa, liste dosyaları sonuçlarına dahil edilmez. Ayrıca, REST silme dosyası ve REST Delete dizin işlemlerinin, kararlılık ve bekleyen silme durumuyla sonuçlanmadığını unutmayın.

Daha fazla bilgi için bkz.

* [Dosya kilitlerini yönetme](https://msdn.microsoft.com/library/azure/dn194265.aspx)

## <a name="next-steps"></a>Sonraki adımlar

Bu blogda başvurulan tüm örnek uygulamalar için:

* [Azure depolama kullanarak eşzamanlılık yönetme-örnek uygulama](https://code.msdn.microsoft.com/Managing-Concurrency-using-56018114)

Azure depolama hakkında daha fazla bilgi için bkz.

* [Microsoft Azure Depolama giriş sayfası](https://azure.microsoft.com/services/storage/)
* [Azure Depolama’ya giriş](storage-introduction.md)
* [BLOB](../blobs/storage-dotnet-how-to-use-blobs.md), [tablo](../../cosmos-db/table-storage-how-to-use-dotnet.md), [kuyruk](../storage-dotnet-how-to-use-queues.md)ve [dosyalar](../storage-dotnet-how-to-use-files.md) için depolama kullanmaya başlama
* Depolama mimarisi – [Azure depolama: güçlü tutarlılığı olan yüksek oranda kullanılabilir bir bulut depolama hizmetidir](/archive/blogs/windowsazurestorage/sosp-paper-windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency)
