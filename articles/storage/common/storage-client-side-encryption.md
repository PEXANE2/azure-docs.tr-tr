---
title: Microsoft Azure Depolama için .NET ile İstemci Tarafı Şifrelemesi | Microsoft Dokümanlar
description: .NET için Azure Depolama İstemci Kitaplığı, Azure Depolama uygulamalarınız için maksimum güvenlik için istemci tarafı şifrelemeyi ve Azure Anahtar Kasası ile tümleştirmeyi destekler.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 10/20/2017
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: 6cf19292c3675382789ca25af7f9b7f69e9066fe
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79255424"
---
# <a name="client-side-encryption-and-azure-key-vault-for-microsoft-azure-storage"></a>Microsoft Azure Depolama için İstemci Tarafı Şifreleme ve Azure Anahtar Kasası
[!INCLUDE [storage-selector-client-side-encryption-include](../../../includes/storage-selector-client-side-encryption-include.md)]

## <a name="overview"></a>Genel Bakış
[.NET için Azure Depolama İstemci Kitaplığı,](/dotnet/api/overview/azure/storage?view=azure-dotnet) Azure Depolama'ya yüklemeden önce istemci uygulamaları içindeki verileri şifrelemeyi ve istemciye indirirken verilerin şifresini çözmeyi destekler. Kitaplık, depolama hesabı anahtar yönetimi için [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) ile tümleştirmeyi de destekler.

İstemci tarafı şifreleme ve Azure Key Vault kullanarak blobs şifreleme sürecinde size yol açan bir adım adım öğretici için, Azure Key Vault kullanarak Microsoft Azure Depolama şifreve [şifre lekeleri](../blobs/storage-encrypt-decrypt-blobs-key-vault.md)bakın.

Java ile istemci tarafı şifreleme için, [Microsoft Azure Depolama için Java ile İstemci Tarafı Şifreleme'ye](storage-client-side-encryption-java.md)bakın.

## <a name="encryption-and-decryption-via-the-envelope-technique"></a>Zarf tekniği ile şifreleme ve şifre çözme
Şifreleme ve şifre çözme işlemleri zarf tekniğini izler.

### <a name="encryption-via-the-envelope-technique"></a>Zarf tekniği ile şifreleme
Zarf tekniği ile şifreleme aşağıdaki şekilde çalışır:

1. Azure depolama istemcisi kitaplığı, tek kullanımlık simetrik anahtar olan bir içerik şifreleme anahtarı (CEK) oluşturur.
2. Kullanıcı verileri bu CEK kullanılarak şifrelenir.
3. CEK daha sonra anahtar şifreleme anahtarı (KEK) kullanılarak sarılır (şifrelenir). KEK, önemli bir tanımlayıcı tarafından tanımlanır ve asimetrik anahtar çifti veya simetrik anahtar olabilir ve yerel olarak yönetilebilir veya Azure Anahtar Kasaları'nda depolanabilir.
   
    Depolama istemcisi kitaplığı kek erişimi asla. Kitaplık, Key Vault tarafından sağlanan anahtar sarma algoritmasını çağırır. Kullanıcılar, istenirse anahtar kaydırma/açma için özel sağlayıcılar kullanmayı seçebilirler.

4. Şifrelenmiş veriler daha sonra Azure Depolama hizmetine yüklenir. Bazı ek şifreleme meta verileriyle birlikte sarılmış anahtar meta veri olarak depolanır (blob üzerinde) veya şifrelenmiş verilerle (sıra iletileri ve tablo varlıkları) enterpolasyonlanır.

### <a name="decryption-via-the-envelope-technique"></a>Zarf tekniği ile şifre çözme
Zarf tekniği ile şifre çözme aşağıdaki şekilde çalışır:

1. İstemci kitaplığı, kullanıcının anahtar şifreleme anahtarını (KEK) yerel olarak veya Azure Key Vaults'ta yönettiğini varsayar. Kullanıcının şifreleme için kullanılan belirli anahtarı bilmesi gerekmez. Bunun yerine, anahtarlara farklı anahtar tanımlayıcıları çözen bir anahtar çözücü ayarlanabilir ve kullanılabilir.
2. İstemci kitaplığı, serviste depolanan şifreleme materyalleriyle birlikte şifrelenmiş verileri karşıdan yüklenir.
3. Sarılmış içerik şifreleme anahtarı (CEK), anahtar şifreleme anahtarı (KEK) kullanılarak unwrapped (şifresi çözülür). Burada yine, istemci kitaplığı KEK erişimi yok. Yalnızca özel veya Key Vault sağlayıcısının açma algoritmasını çağırır.
4. İçerik şifreleme anahtarı (CEK) daha sonra şifrelenmiş kullanıcı verilerinin şifresini çözmek için kullanılır.

## <a name="encryption-mechanism"></a>Şifreleme Mekanizması
Depolama istemcisi kitaplığı, kullanıcı verilerini şifrelemek için [AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard) kullanır. Özellikle, AES ile [Şifre Blok Zincirleme (CBC)](https://en.wikipedia.org/wiki/Block_cipher_mode_of_operation#Cipher-block_chaining_.28CBC.29) modu. Her hizmet biraz farklı çalışır, bu yüzden burada her biri tartışacağız.

### <a name="blobs"></a>Bloblar
İstemci kitaplığı şu anda yalnızca tüm lekelerin şifrelemesini destekler. Özellikle, kullanıcılar **UploadFrom** yöntemlerini veya **OpenWrite** yöntemini kullandığında şifreleme desteklenir. İndirmeler için hem tam hem de aralıklı indirmeler desteklenir.

Şifreleme sırasında, istemci kitaplığı, 32 baytlık rastgele içerik şifreleme anahtarı (CEK) ile birlikte 16 baytlık rastgele bir Başlatma Vektörü (IV) oluşturur ve bu bilgileri kullanarak blob verilerinin zarf şifrelemesini gerçekleştirir. Sarılmış CEK ve bazı ek şifreleme meta verileri daha sonra hizmetteki şifreli blob ile birlikte blob meta veri olarak depolanır.

> [!WARNING]
> Blob için kendi meta verilerinizi düzenliyor veya yüklüyorsanız, bu meta verilerin korunduğundan emin olmanız gerekir. Bu meta veriler olmadan yeni meta veriler yüklerseniz, sarılmış CEK, IV ve diğer meta veriler kaybolur ve blob içeriği bir daha asla alınamaz.
> 
> 

Şifreli bir blob indirme **DownloadTo**/**BlobReadStream** kolaylık yöntemlerini kullanarak tüm blob içeriğini alma içerir. Sarılmış CEK unwrapped ve iv ile birlikte kullanılır (bu durumda blob meta veri olarak saklanır) kullanıcılara şifresi çözülmüş verileri döndürmek için.

Şifreli blob rasgele bir aralık **(DownloadRange** yöntemleri) indirme başarıyla istenen aralığın şifresini çözmek için kullanılabilecek ek veri küçük bir miktar almak için kullanıcılar tarafından sağlanan aralığı ayarlama içerir.

Tüm blob türleri (blok lekeleri, sayfa lekeleri ve ek blobs) bu şema kullanılarak şifrelenebilir/şifresi çözülebilir.

### <a name="queues"></a>Kuyruklar
Sıra iletileri herhangi bir biçimde olabileceğinden, istemci kitaplığı ileti metninde Başlatma Vektörü (IV) ve şifrelenmiş içerik şifreleme anahtarını (CEK) içeren özel bir biçim tanımlar.

Şifreleme sırasında, istemci kitaplığı rastgele 16 baytlık bir RASGELE IV 32 bayt rastgele CEK oluşturur ve bu bilgileri kullanarak sıra iletisi metninin zarf şifrelemegerçekleştirir. Sarılmış CEK ve bazı ek şifreleme meta verileri daha sonra şifrelenmiş sıra iletisine eklenir. Bu değiştirilmiş ileti (aşağıda gösterilmiştir) hizmette depolanır.

    <MessageText>{"EncryptedMessageContents":"6kOu8Rq1C3+M1QO4alKLmWthWXSmHV3mEfxBAgP9QGTU++MKn2uPq3t2UjF1DO6w","EncryptionData":{…}}</MessageText>

Şifre çözme sırasında, sarılmış anahtar sıra iletisinden çıkarılır ve paketlenmemiş olur. IV ayrıca sıra iletisinden ayıklanır ve sıra iletisi verilerinin şifresini çözmek için paketlenmemiş anahtarla birlikte kullanılır. Şifreleme meta verilerinin küçük (500 baytın altında) olduğunu, bu nedenle bir sıra iletisi için 64KB sınırına doğru sayılmasına bağlı olsa da, etkinin yönetilebilir olması gerektiğini unutmayın.

### <a name="tables"></a>Tablolar
İstemci kitaplığı, ekleme ve değiştirme işlemleri için varlık özelliklerinin şifrelemesini destekler.

> [!NOTE]
> Birleştirme şu anda desteklenmiyor. Özelliklerin bir alt kümesi daha önce farklı bir anahtar kullanılarak şifrelenmiş olabileceğinden, yeni özellikleri birleştirmek ve meta verileri güncelleştirmek veri kaybına neden olur. Birleştirme, hizmetten önceden varolan varlığı okumak için ek hizmet çağrıları yapmayı veya her ikisi de performans nedenleriyle uygun olmayan özellik başına yeni bir anahtar kullanmayı gerektirir.
> 
> 

Tablo veri şifreleme aşağıdaki gibi çalışır:  

1. Kullanıcılar şifrelenecek özellikleri belirtir.
2. İstemci kitaplığı, her varlık için 32 baytlık rasgele içerik şifreleme anahtarı (CEK) ile birlikte 16 baytlık rastgele bir Başlatma Vektörü (IV) oluşturur ve özellik başına yeni bir IV türeterek şifrelenecek tek tek özellikler üzerinde zarf şifrelemesi gerçekleştirir. Şifrelenmiş özellik ikili veri olarak depolanır.
3. Sarılmış CEK ve bazı ek şifreleme meta verileri daha sonra iki ek ayrılmış özellik olarak depolanır. İlk ayrılmış özellik (_ClientEncryptionMetadata1) IV, sürüm ve sarılmış anahtar hakkında bilgi tutan bir dize özelliğidir. İkinci ayrılmış özellik (_ClientEncryptionMetadata2), şifrelenmiş özellikler hakkındaki bilgileri tutan ikili bir özelliktir. Bu ikinci özellik (_ClientEncryptionMetadata2) bilgileri kendisi şifrelenir.
4. Şifreleme için gerekli olan bu ek ayrılmış özellikler nedeniyle, kullanıcılar artık 252 yerine yalnızca 250 özel özelliklere sahip olabilir. Varlığın toplam boyutu 1 MB'den az olmalıdır.

Yalnızca dize özelliklerinin şifrelenebileceğini unutmayın. Diğer özellik türleri şifrelenecekse, dizeleri dönüştürmeleri gerekir. Şifrelenmiş dizeleri ikili özellikleri olarak hizmette depolanır ve şifre çözme sonra dizeleri geri dönüştürülür.

Tablolar için, şifreleme ilkesine ek olarak, kullanıcıların şifrelenecek özellikleri belirtmesi gerekir. Bu, bir [EncryptProperty] özniteliği (TableEntity'den türeyen POCO varlıkları için) veya istek seçeneklerindeki bir şifreleme çözümleyicisi belirterek yapılabilir. Şifreleme çözümleyicisi, bölüm anahtarı, satır anahtarı ve özellik adı alan ve bu özelliğin şifrelenip şifrelenmemesi gerektiğini belirten bir Boolean döndüren bir temsilcidir. Şifreleme sırasında, istemci kitaplığı, bir özelliğin kabloya yazarken şifrelenip şifrelenmemesi gerektiğine karar vermek için bu bilgileri kullanır. Temsilci ayrıca özelliklerin nasıl şifrelendirilenleri hakkında mantık olasılığını da sağlar. (Örneğin, X ise, a özelliğini şifreleyin; aksi takdirde A ve B özelliklerini şifreleyin.) Varlıkları okurken veya sorgularken bu bilgileri sağlamanın gerekli olmadığını unutmayın.

### <a name="batch-operations"></a>Toplu İşlemler
Toplu işlemişlemlerinde, istemci kitaplığı toplu işlem başına yalnızca bir seçenek nesnesine (ve dolayısıyla bir ilke/KEK) izin verdiğinden, bu toplu işlemdeki tüm satırlarda aynı KEK kullanılır. Ancak, istemci kitaplığı dahili olarak toplu iş satır başına yeni bir rasgele IV ve rasgele CEK oluşturur. Kullanıcılar, şifreleme çözümleyicisinde bu davranışı tanımlayarak toplu işteki her işlem için farklı özellikleri şifrelemeyi de seçebilir.

### <a name="queries"></a>Sorgular
> [!NOTE]
> Varlıklar şifrelenmiş olduğundan, şifrelenmiş bir özellik üzerinde filtre olan sorguları çalıştıramazsınız.  Denerseniz, hizmet şifrelenmiş verileri şifrelenmemiş verilerle karşılaştırmaya çalıştığından, sonuçlar yanlış olacaktır.
> 
> 
> Sorgu işlemlerini gerçekleştirmek için, sonuç kümesindeki tüm anahtarları çözebilecek bir anahtar çözümleyicisi belirtmeniz gerekir. Sorgu sonucu bulunan bir varlık bir sağlayıcıya çözülemez, istemci kitaplık bir hata atar. Sunucu tarafı projeksiyonları gerçekleştiren tüm sorgular için istemci kitaplığı varsayılan olarak seçili sütunlara özel şifreleme meta veri özelliklerini (_ClientEncryptionMetadata1 ve _ClientEncryptionMetadata2) ekler.

## <a name="azure-key-vault"></a>Azure Key Vault
Azure Anahtar Kasası, bulut uygulamaları ve hizmetleri tarafından kullanılan şifreleme anahtarlarının ve gizli anahtarların korunmasına yardımcı olur. Kullanıcılar Azure Key Vault'u kullanarak anahtarları ve sırları (kimlik doğrulama anahtarları, depolama hesabı anahtarları, veri şifreleme anahtarları gibi) şifreleyebilir. PFX dosyaları ve parolaları) donanım güvenlik modülleri (HSM) tarafından korunan anahtarları kullanarak. Daha fazla bilgi için bkz. [Azure Key Vault nedir?](../../key-vault/key-vault-overview.md)

Depolama istemcisi kitaplığı, anahtarları yönetmek için Azure genelinde ortak bir çerçeve sağlamak için Anahtar Kasa çekirdek kitaplığını kullanır. Kullanıcılar ayrıca Key Vault uzantıları kitaplığını kullanmanın ek avantajını da elde eder. Uzantılar kitaplığı, basit ve sorunsuz Simetrik/RSA yerel ve bulut anahtar sağlayıcılarının yanı sıra toplama ve önbelleğe alma konusunda yararlı işlevler sağlar.

### <a name="interface-and-dependencies"></a>Arayüz ve bağımlılıklar
Üç Key Vault paketi vardır:

* Microsoft.Azure.KeyVault.Core IKey ve IKeyResolver içerir. Bağımlılıkları olmayan küçük bir pakettir. .NET için depolama istemcisi kitaplığı bir bağımlılık olarak tanımlar.
* Microsoft.Azure.KeyVault, Key Vault REST istemcisini içerir.
* Microsoft.Azure.KeyVault.Extensions, şifreleme algoritmaları ve RSAKey ve SymmetricKey uygulamalarını içeren uzantı kodu içerir. Core ve KeyVault ad alanlarına bağlıdır ve bir toplu çözümleyici (kullanıcılar birden çok anahtar sağlayıcısı kullanmak istediğinizde) ve önbelleğe alma anahtarı çözümleyicisini tanımlamak için işlevsellik sağlar. Depolama istemcisi kitaplığı doğrudan bu pakete bağlı olmasa da, kullanıcılar anahtarlarını depolamak için Azure Key Vault'u kullanmak veya yerel ve bulut şifreleme sağlayıcılarını kullanmak için Key Vault uzantılarını kullanmak istiyorlarsa, bu pakete gereksinim duyarlar.

Key Vault yüksek değerli ana anahtarlar için tasarlanmıştır ve Key Vault başına azaltma limitleri bu göz önünde bulundurularak tasarlanmıştır. Key Vault ile istemci tarafı şifreleme gerçekleştirirken, tercih edilen model, Key Vault'ta sır olarak depolanan ve yerel olarak önbelleğe alınmış simetrik ana anahtarları kullanmaktır. Kullanıcılar aşağıdakileri yapmalıdır:

1. Gizli bir çevrimdışı oluşturun ve Key Vault'a yükleyin.
2. Şifreleme için sırrın geçerli sürümünü çözmek ve bu bilgileri yerel olarak önbelleğe almak için gizlinin temel tanımlayıcısını parametre olarak kullanın. Önbelleğe alma için CachingKeyResolver kullanın; kullanıcıların kendi önbelleğe alma mantığını uygulamaları beklenmez.
3. Şifreleme ilkesini oluştururken önbelleğe alma çözümleyicisini giriş olarak kullanın.

Anahtar Kasa sı kullanımı ile ilgili daha fazla bilgiyi [şifreleme kodu örneklerinde](https://github.com/Azure/azure-storage-net/tree/master/Samples/GettingStarted/EncryptionSamples)bulabilirsiniz.

## <a name="best-practices"></a>En iyi uygulamalar
Şifreleme desteği yalnızca .NET için depolama istemcisi kitaplığında kullanılabilir. Windows Phone ve Windows Runtime şu anda şifrelemeyi desteklemiyor.

> [!IMPORTANT]
> İstemci tarafı şifrelemekullanırken bu önemli noktalara dikkat edin:
> 
> * Şifrelenmiş bir blob'a okurken veya yazarken, tüm blob yükleme komutlarını ve aralık/bütün blob indirme komutlarını kullanın. Blok La, Blok Listesi Koy, Sayfalar Yaz, Sayfaları Temizle veya Ek Engelleme gibi protokol işlemlerini kullanarak şifreli bir blob'a yazmaktan kaçının; aksi takdirde şifrelenmiş blob bozuk ve okunamayan hale getirebilirsiniz.
> * Tablolar için benzer bir kısıtlama vardır. Şifreleme meta verilerini güncelleştirmeden şifrelenmiş özellikleri güncelleştirmemeye dikkat edin.
> * Meta verileri şifrelenmiş blob'a ayarlarsanız, meta verileri ayarlamak katkı maddesi olmadığından, şifre çözme için gereken şifrelemeyle ilgili meta verilerin üzerine yazabilirsiniz. Bu anlık görüntüler için de geçerlidir; şifrelenmiş bir lekenin anlık görüntüsünü oluştururken meta verileri belirtmekten kaçının. Meta verilerin ayarlandığı gerekiyorsa, geçerli şifreleme meta verilerini almak için önce **FetchAttributes** yöntemini aradığından emin olun ve meta veriler ayarlanırken eşzamanlı yazmalardan kaçının.
> * Yalnızca şifrelenmiş verilerle çalışması gereken kullanıcılar için varsayılan istek seçeneklerinde **Şifreleme Gerektirme** özelliğini etkinleştirin. Daha fazla bilgi için aşağıya bakın.
> 
> 

## <a name="client-api--interface"></a>İstemci API / Arayüz
Bir EncryptionPolicy nesnesi oluştururken, kullanıcılar yalnızca bir Anahtar (IKey uygulama), yalnızca bir çözümleyici (IKeyResolver uygulama) veya her ikisini de sağlayabilir. IKey, anahtar tanımlayıcısı kullanılarak tanımlanan ve sarma/açma mantığını sağlayan temel anahtar türüdür. IKeyResolver şifre çözme işlemi sırasında bir anahtarı çözmek için kullanılır. Anahtar tanımlayıcısı verilen bir IKey döndüren bir ResolveKey yöntemi tanımlar. Bu, kullanıcılara birden çok konumda yönetilen birden çok anahtar arasında seçim yapma olanağı sağlar.

* Şifreleme için anahtar her zaman kullanılır ve anahtarın yokluğu bir hataya neden olur.
* Şifre çözme için:
  * Anahtarı almak için belirtilirse anahtar çözücü çağrılır. Çözümleyici belirtilir, ancak anahtar tanımlayıcısı için eşleme yoksa, bir hata atılır.
  * Çözümleyici belirtilmemişse, ancak bir anahtar belirtilmişse, tanımlayıcısı gerekli anahtar tanımlayıcısıile eşleşiyorsa anahtar kullanılır. Tanımlayıcı eşleşmiyorsa, bir hata atılır.

Bu makaledeki kod örnekleri, şifreleme ilkesinin ayarını ve şifrelenmiş verilerle çalıştığını gösterir, ancak Azure Key Vault ile çalıştığını göstermez. GitHub'daki [şifreleme örnekleri,](https://github.com/Azure/azure-storage-net/tree/master/Samples/GettingStarted/EncryptionSamples) Key Vault tümleştirmesiyle birlikte blobs, kuyruklar ve tablolar için daha ayrıntılı bir uçtan uca senaryo gösterir.

### <a name="requireencryption-mode"></a>Şifreleme modu gerektirir
Kullanıcılar isteğe bağlı olarak tüm yüklemelerin ve indirmelerin şifrelenmesi gereken bir çalışma moduna olanak sağlayabilir. Bu modda, şifreleme ilkesi olmadan veri yükleme veya hizmette şifrelenmemiş verileri indirme girişimleri istemcide başarısız olur. İstek seçenekleri nesnesinin **Denetimgerektiren Şifreleme** özelliği bu davranışı denetler. Uygulamanız Azure Depolama'da depolanan tüm nesneleri şifreleyecekse, Hizmet istemcisi nesnesi için varsayılan istek seçeneklerinde **Şifreleme Özelliğini** Ayarlayabilirsiniz. Örneğin, **CloudBlobClient.DefaultRequestOptions.RequireEncryption'i** bu istemci nesnesi üzerinden gerçekleştirilen tüm blob işlemleri için şifreleme gerektirecek şekilde **doğru** olarak ayarlayın.


### <a name="blob-service-encryption"></a>Blob hizmet şifrelemesi
**BlobEncryptionPolicy** nesnesi oluşturun ve istek seçeneklerinde ayarlayın **(Varsayılan İstek Seçenekleri'ni**kullanarak API başına veya istemci düzeyinde). Diğer her şey istemci kitaplığı tarafından dahili olarak ele alınacaktır.

```csharp
// Create the IKey used for encryption.
 RsaKey key = new RsaKey("private:key1" /* key identifier */);

 // Create the encryption policy to be used for upload and download.
 BlobEncryptionPolicy policy = new BlobEncryptionPolicy(key, null);

 // Set the encryption policy on the request options.
 BlobRequestOptions options = new BlobRequestOptions() { EncryptionPolicy = policy };

 // Upload the encrypted contents to the blob.
 blob.UploadFromStream(stream, size, null, options, null);

 // Download and decrypt the encrypted contents from the blob.
 MemoryStream outputStream = new MemoryStream();
 blob.DownloadToStream(outputStream, null, options, null);
```

### <a name="queue-service-encryption"></a>Sıra hizmeti şifrelemesi
Bir **QueueEncryptionPolicy** nesnesi oluşturun ve istek seçeneklerinde ayarlayın **(Varsayılan İstek Seçenekleri'ni**kullanarak API başına veya istemci düzeyinde). Diğer her şey istemci kitaplığı tarafından dahili olarak ele alınacaktır.

```csharp
// Create the IKey used for encryption.
 RsaKey key = new RsaKey("private:key1" /* key identifier */);

 // Create the encryption policy to be used for upload and download.
 QueueEncryptionPolicy policy = new QueueEncryptionPolicy(key, null);

 // Add message
 QueueRequestOptions options = new QueueRequestOptions() { EncryptionPolicy = policy };
 queue.AddMessage(message, null, null, options, null);

 // Retrieve message
 CloudQueueMessage retrMessage = queue.GetMessage(null, options, null);
```

### <a name="table-service-encryption"></a>Tablo hizmeti şifrelemesi
Bir şifreleme ilkesi oluşturmanın ve istek seçeneklerine ayarlanın yanı sıra, **TableRequestOptions'da**bir **Şifreleme Çözümleyicisi** belirtmeniz veya varlık üzerinde [EncryptProperty] özniteliğini ayarlamanız gerekir.

#### <a name="using-the-resolver"></a>Çözümleyiciyi kullanma

```csharp
// Create the IKey used for encryption.
 RsaKey key = new RsaKey("private:key1" /* key identifier */);

 // Create the encryption policy to be used for upload and download.
 TableEncryptionPolicy policy = new TableEncryptionPolicy(key, null);

 TableRequestOptions options = new TableRequestOptions()
 {
    EncryptionResolver = (pk, rk, propName) =>
     {
        if (propName == "foo")
         {
            return true;
         }
         return false;
     },
     EncryptionPolicy = policy
 };

 // Insert Entity
 currentTable.Execute(TableOperation.Insert(ent), options, null);

 // Retrieve Entity
 // No need to specify an encryption resolver for retrieve
 TableRequestOptions retrieveOptions = new TableRequestOptions()
 {
    EncryptionPolicy = policy
 };

 TableOperation operation = TableOperation.Retrieve(ent.PartitionKey, ent.RowKey);
 TableResult result = currentTable.Execute(operation, retrieveOptions, null);
```

#### <a name="using-attributes"></a>Öznitelikleri kullanma
Yukarıda belirtildiği gibi, varlık TableEntity uygularsa, o zaman özellikleri **Şifreleme Çözücü**belirterek yerine [EncryptProperty] özniteliği ile dekore edilebilir.

```csharp
[EncryptProperty]
 public string EncryptedProperty1 { get; set; }
```

## <a name="encryption-and-performance"></a>Şifreleme ve performans
Depolama verilerinizi şifrelemenin ek performans yüküyle sonuçladığını unutmayın. İçerik anahtarı ve IV oluşturulmalı, içeriğin kendisi şifrelenmeli ve ek meta veriler biçimlendirilmeli ve yüklenmelidir. Bu ek yükü, şifrelenen veri miktarına bağlı olarak değişir. Müşterilerin geliştirme sırasında uygulamalarını her zaman performans için test etmelerine tavsiye ediyoruz.

## <a name="next-steps"></a>Sonraki adımlar
* [Öğretici: Azure Anahtar Kasası'nı kullanarak Microsoft Azure Depolama'daki lekeleri şifreleme ve çözme](../blobs/storage-encrypt-decrypt-blobs-key-vault.md)
* [.NET NuGet paketi için Azure Depolama İstemci Kitaplığını](https://www.nuget.org/packages/WindowsAzure.Storage) indirin
* Azure Key Vault NuGet [Core](https://www.nuget.org/packages/Microsoft.Azure.KeyVault.Core/), [İstemci](https://www.nuget.org/packages/Microsoft.Azure.KeyVault/)ve [Uzantılı](https://www.nuget.org/packages/Microsoft.Azure.KeyVault.Extensions/) paketleri indirin  
* Azure [Anahtar Kasası Belgelerini](../../key-vault/key-vault-overview.md) ziyaret edin
