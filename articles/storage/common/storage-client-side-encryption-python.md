---
title: Python ile istemci tarafı şifreleme
titleSuffix: Azure Storage
description: Python için Azure Depolama İstemci Kitaplığı, Azure Depolama uygulamalarınız için maksimum güvenlik için istemci tarafındaki şifrelemeyi destekler.
services: storage
author: tamram
ms.service: storage
ms.devlang: python
ms.topic: how-to
ms.date: 12/04/2019
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: 16e66cd762b86b27dc6703542ca7261b2300a33b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74895380"
---
# <a name="client-side-encryption-with-python"></a>Python ile istemci tarafı şifreleme

[!INCLUDE [storage-selector-client-side-encryption-include](../../../includes/storage-selector-client-side-encryption-include.md)]

## <a name="overview"></a>Genel Bakış
[Python için Azure Depolama İstemci Kitaplığı,](https://pypi.python.org/pypi/azure-storage) Azure Depolama'ya yüklemeden önce istemci uygulamaları içindeki verileri şifrelemeyi ve istemciye indirirken verilerin şifresini çözmeyi destekler.

> [!NOTE]
> Azure Depolama Python kitaplığı önizlemede.
> 
> 

## <a name="encryption-and-decryption-via-the-envelope-technique"></a>Zarf tekniği ile şifreleme ve şifre çözme
Şifreleme ve şifre çözme işlemleri zarf tekniğini izler.

### <a name="encryption-via-the-envelope-technique"></a>Zarf tekniği ile şifreleme
Zarf tekniği ile şifreleme aşağıdaki şekilde çalışır:

1. Azure depolama istemcisi kitaplığı, tek kullanımlık simetrik anahtar olan bir içerik şifreleme anahtarı (CEK) oluşturur.
2. Kullanıcı verileri bu CEK kullanılarak şifrelenir.
3. CEK daha sonra anahtar şifreleme anahtarı (KEK) kullanılarak sarılır (şifrelenir). KEK anahtar tanımlayıcısı ile tanımlanır ve asimetrik bir anahtar çifti veya yerel olarak yönetilen simetrik bir anahtar olabilir.
   Depolama istemcisi kitaplığı kek erişimi asla. Kitaplık KEK tarafından sağlanan anahtar sarma algoritmasını çağırır. Kullanıcılar, istenirse anahtar kaydırma/açma için özel sağlayıcılar kullanmayı seçebilirler.
4. Şifrelenmiş veriler daha sonra Azure Depolama hizmetine yüklenir. Bazı ek şifreleme meta verileriyle birlikte sarılmış anahtar meta veri olarak depolanır (blob üzerinde) veya şifrelenmiş verilerle (sıra iletileri ve tablo varlıkları) enterpolasyonlanır.

### <a name="decryption-via-the-envelope-technique"></a>Zarf tekniği ile şifre çözme
Zarf tekniği ile şifre çözme aşağıdaki şekilde çalışır:

1. İstemci kitaplığı, kullanıcının anahtar şifreleme anahtarını (KEK) yerel olarak yönettiğini varsayar. Kullanıcının şifreleme için kullanılan belirli anahtarı bilmesi gerekmez. Bunun yerine, anahtarlara farklı anahtar tanımlayıcıları çözen bir anahtar çözücü ayarlanabilir ve kullanılabilir.
2. İstemci kitaplığı, serviste depolanan şifreleme materyalleriyle birlikte şifrelenmiş verileri karşıdan yüklenir.
3. Sarılmış içerik şifreleme anahtarı (CEK), anahtar şifreleme anahtarı (KEK) kullanılarak unwrapped (şifresi çözülür). Burada yine, istemci kitaplığı KEK erişimi yok. Yalnızca özel sağlayıcının açma algoritmasını çağırır.
4. İçerik şifreleme anahtarı (CEK) daha sonra şifrelenmiş kullanıcı verilerinin şifresini çözmek için kullanılır.

## <a name="encryption-mechanism"></a>Şifreleme Mekanizması
Depolama istemcisi kitaplığı, kullanıcı verilerini şifrelemek için [AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard) kullanır. Özellikle, AES ile [Şifre Blok Zincirleme (CBC)](https://en.wikipedia.org/wiki/Block_cipher_mode_of_operation#Cipher-block_chaining_.28CBC.29) modu. Her hizmet biraz farklı çalışır, bu yüzden burada her biri tartışacağız.

### <a name="blobs"></a>Bloblar
İstemci kitaplığı şu anda yalnızca tüm lekelerin şifrelemesini destekler. Özellikle, şifreleme kullanıcılar **oluşturma*** yöntemleri ni kullandığında desteklenir. İndirmeler için hem tam hem de aralıklı indirmeler desteklenir ve hem yükleme hem de karşıdan yükleme paralelleştirme kullanılabilir.

Şifreleme sırasında, istemci kitaplığı, 32 baytlık rastgele içerik şifreleme anahtarı (CEK) ile birlikte 16 baytlık rastgele bir Başlatma Vektörü (IV) oluşturur ve bu bilgileri kullanarak blob verilerinin zarf şifrelemesini gerçekleştirir. Sarılmış CEK ve bazı ek şifreleme meta verileri daha sonra hizmetteki şifreli blob ile birlikte blob meta veri olarak depolanır.

> [!WARNING]
> Blob için kendi meta verilerinizi düzenliyor veya yüklüyorsanız, bu meta verilerin korunduğundan emin olmanız gerekir. Bu meta veriler olmadan yeni meta veriler yüklerseniz, sarılmış CEK, IV ve diğer meta veriler kaybolur ve blob içeriği bir daha asla alınamaz.
> 
> 

Şifreli bir blob indirme **get*** kolaylık yöntemlerini kullanarak tüm blob içeriğini alma içerir. Sarılmış CEK unwrapped ve iv ile birlikte kullanılır (bu durumda blob meta veri olarak saklanır) kullanıcılara şifresi çözülmüş verileri döndürmek için.

Şifreli blob'da rasgele bir aralık **(get*** methods in passedile) indirmek, istenen aralığın şifresini başarılı bir şekilde çözmek için kullanılabilecek az miktarda ek veri elde etmek için kullanıcılar tarafından sağlanan aralığı ayarlamayı içerir.

Blok lekeleri ve sayfa lekeleri yalnızca bu düzen kullanılarak şifrelenebilir/şifresi çözülebilir. Şu anda ek lekeleri şifrelemek için destek yoktur.

### <a name="queues"></a>Kuyruklar
Sıra iletileri herhangi bir biçimde olabileceğinden, istemci kitaplığı ileti metninde Başlatma Vektörü (IV) ve şifrelenmiş içerik şifreleme anahtarını (CEK) içeren özel bir biçim tanımlar.

Şifreleme sırasında, istemci kitaplığı rastgele 16 baytlık bir RASGELE IV 32 bayt rastgele CEK oluşturur ve bu bilgileri kullanarak sıra iletisi metninin zarf şifrelemegerçekleştirir. Sarılmış CEK ve bazı ek şifreleme meta verileri daha sonra şifrelenmiş sıra iletisine eklenir. Bu değiştirilmiş ileti (aşağıda gösterilmiştir) hizmette depolanır.

```
<MessageText>{"EncryptedMessageContents":"6kOu8Rq1C3+M1QO4alKLmWthWXSmHV3mEfxBAgP9QGTU++MKn2uPq3t2UjF1DO6w","EncryptionData":{…}}</MessageText>
```

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
3. Sarılmış CEK ve bazı ek şifreleme meta verileri daha sonra iki ek ayrılmış özellik olarak depolanır. İlk ayrılmış özellik\_(ClientEncryptionMetadata1), IV, sürüm ve sarılmış anahtar hakkındaki bilgileri tutan bir dize özelliğidir. İkinci ayrılmış özellik\_(ClientEncryptionMetadata2), şifrelenmiş özellikler hakkındaki bilgileri tutan ikili bir özelliktir. Bu ikinci özellikteki\_bilgiler (ClientEncryptionMetadata2) kendisi şifrelenir.
4. Şifreleme için gerekli olan bu ek ayrılmış özellikler nedeniyle, kullanıcılar artık 252 yerine yalnızca 250 özel özelliklere sahip olabilir. Varlığın toplam boyutu 1MB'den az olmalıdır.

   Yalnızca dize özelliklerinin şifrelenebileceğini unutmayın. Diğer özellik türleri şifrelenecekse, dizeleri dönüştürmeleri gerekir. Şifrelenmiş dizeleri ikili özellikleri olarak hizmette depolanır ve deşifre edildikten sonra dizeleri (ham dizeleri değil, EdmType.STRING türü ile EntityProperties) dönüştürülür.

   Tablolar için, şifreleme ilkesine ek olarak, kullanıcıların şifrelenecek özellikleri belirtmesi gerekir. Bu, bu özellikleri EdmType.STRING türüne ayarlanmış TabloVarlık nesnelerinde depolayarak ve doğru ayarlanmış şifreleme kümesi yle veya tablo hizmeti nesnesinde encryption_resolver_function ayarlayarak yapılabilir. Şifreleme çözümleyicisi, bölüm anahtarı, satır anahtarı ve özellik adı alan ve bu özelliğin şifrelenip şifrelenmemesi gerektiğini belirten bir boolean döndüren bir işlevdir. Şifreleme sırasında, istemci kitaplığı, bir özelliğin kabloya yazarken şifrelenip şifrelenmemesi gerektiğine karar vermek için bu bilgileri kullanır. Temsilci ayrıca özelliklerin nasıl şifrelendirilenleri hakkında mantık olasılığını da sağlar. (Örneğin, X ise, a özelliğini şifreleyin; aksi takdirde A ve B özelliklerini şifreleyin.) Varlıkları okurken veya sorgularken bu bilgileri sağlamanın gerekli olmadığını unutmayın.

### <a name="batch-operations"></a>Toplu İşlemler
Bir şifreleme ilkesi toplu işteki tüm satırlar için geçerlidir. İstemci kitaplığı dahili olarak toplu iş başına yeni bir rasgele IV ve rasgele CEK oluşturur. Kullanıcılar, şifreleme çözümleyicisinde bu davranışı tanımlayarak toplu işteki her işlem için farklı özellikleri şifrelemeyi de seçebilir.
Tablo hizmeti toplu iş() yöntemi yle bağlam yöneticisi olarak bir toplu iş oluşturulursa, tablo hizmetinin şifreleme ilkesi toplu iş için otomatik olarak uygulanır. Bir toplu iş oluşturucu çağırılarak açıkça oluşturulursa, şifreleme ilkesi bir parametre olarak geçirilmeli ve toplu iş ömrü boyunca değiştirilmemiş bırakılmalıdır.
Varlıkların toplu iş için şifreleme ilkesini kullanarak toplu iş aktIçine eklenirken şifrelendiklerini unutmayın (varlıklar, tablo hizmetinin şifreleme ilkesini kullanarak toplu işlenme sırasında şifrelenmez).

### <a name="queries"></a>Sorgular
> [!NOTE]
> Varlıklar şifrelenmiş olduğundan, şifrelenmiş bir özellik üzerinde filtre olan sorguları çalıştıramazsınız.  Denerseniz, hizmet şifrelenmiş verileri şifrelenmemiş verilerle karşılaştırmaya çalıştığından, sonuçlar yanlış olacaktır.
> 
> 
> Sorgu işlemlerini gerçekleştirmek için, sonuç kümesindeki tüm anahtarları çözebilecek bir anahtar çözümleyicisi belirtmeniz gerekir. Sorgu sonucu bulunan bir varlık bir sağlayıcıya çözülemez, istemci kitaplık bir hata atar. Sunucu yan projeksiyonları gerçekleştiren tüm sorgular için istemci kitaplığı varsayılan\_olarak seçili \_sütunlara özel şifreleme meta veri özelliklerini (ClientEncryptionMetadata1 ve ClientEncryptionMetadata2) ekler.
> 
> [!IMPORTANT]
> İstemci tarafı şifrelemekullanırken bu önemli noktalara dikkat edin:
> 
> * Şifrelenmiş bir blob'a okurken veya yazarken, tüm blob yükleme komutlarını ve aralık/bütün blob indirme komutlarını kullanın. Blok La, Blok Listesi Koy, Sayfalar Yaz veya Sayfaları Temizle gibi protokol işlemlerini kullanarak şifreli bir blob'a yazmaktan kaçının; aksi takdirde şifrelenmiş blob bozuk ve okunamayan hale getirebilirsiniz.
> * Tablolar için benzer bir kısıtlama vardır. Şifreleme meta verilerini güncelleştirmeden şifrelenmiş özellikleri güncelleştirmemeye dikkat edin.
> * Meta verileri şifrelenmiş blob'a ayarlarsanız, meta verileri ayarlamak katkı maddesi olmadığından, şifre çözme için gereken şifrelemeyle ilgili meta verilerin üzerine yazabilirsiniz. Bu anlık görüntüler için de geçerlidir; şifrelenmiş bir lekenin anlık görüntüsünü oluştururken meta verileri belirtmekten kaçının. Meta verilerin ayarlandığı gerekiyorsa, geçerli şifreleme meta verilerini almak için önce **get_blob_metadata** yöntemini aradığından emin olun ve meta veriler ayarlanırken eşzamanlı yazmalardan kaçının.
> * Yalnızca şifrelenmiş verilerle çalışması gereken kullanıcılar için hizmet nesnesi üzerindeki **require_encryption** bayrağını etkinleştirin. Daha fazla bilgi için aşağıya bakın.

Depolama istemcisi kitaplığı sağlanan KEK ve anahtar çözümleyiciaşağıdaki arabirimi uygulamak için bekliyor. Python KEK yönetimi için [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) desteği beklemededir ve tamamlandığında bu kitaplığa entegre edilecektir.

## <a name="client-api--interface"></a>İstemci API / Arayüz
Bir depolama hizmeti nesnesi (yani blockblobservice) oluşturulduktan sonra, kullanıcı şifreleme ilkesi oluşturan alanlara değerler atayabilir: key_encryption_key, key_resolver_function ve require_encryption. Kullanıcılar yalnızca bir KEK, yalnızca bir çözümleyici veya her ikisini birden sağlayabilir. key_encryption_key anahtar tanımlayıcıkullanılarak tanımlanan ve sarma/açma mantığını sağlayan temel anahtar türüdür. key_resolver_function şifre çözme işlemi sırasında bir anahtarı çözmek için kullanılır. Anahtar tanımlayıcısı verilen geçerli bir KEK döndürür. Bu, kullanıcılara birden çok konumda yönetilen birden çok anahtar arasında seçim yapma olanağı sağlar.

KEK, verileri başarıyla şifrelemek için aşağıdaki yöntemleri uygulamalıdır:

* wrap_key(cek): Kullanıcının seçtiği algoritmayı kullanarak belirtilen CEK'yi (bayt) sarar. Sarılmış anahtarı döndürür.
* get_key_wrap_algorithm(): Anahtarları sarmak için kullanılan algoritmayı döndürür.
* get_kid(): Bu KEK için dize anahtar kimliğini verir.
  KEK, verilerin şifresini başarılı bir şekilde çözmek için aşağıdaki yöntemleri uygulamalıdır:
* unwrap_key(cek, algoritma): Dize belirtilen algoritmayı kullanarak belirtilen CEK'nin açılmamış biçimini verir.
* get_kid(): Bu KEK için bir dize anahtar kimliği verir.

Anahtar çözücü en azından, bir anahtar kimliği verilen, yukarıdaki arayüzü uygulayan ilgili KEK döndürür bir yöntem uygulamak gerekir. Yalnızca bu yöntem, hizmet nesnesi üzerinde key_resolver_function özelliğiatanacak.

* Şifreleme için anahtar her zaman kullanılır ve anahtarın yokluğu bir hataya neden olur.
* Şifre çözme için:

  * Anahtarı almak için belirtilirse anahtar çözücü çağrılır. Çözümleyici belirtilir, ancak anahtar tanımlayıcısı için eşleme yoksa, bir hata atılır.
  * Çözümleyici belirtilmemişse, ancak bir anahtar belirtilmişse, tanımlayıcısı gerekli anahtar tanımlayıcısıile eşleşiyorsa anahtar kullanılır. Tanımlayıcı eşleşmiyorsa, bir hata atılır.

    azure.storage.samples'deki şifreleme örnekleri, lekeler, kuyruklar ve tablolar için daha ayrıntılı bir uçtan uca senaryo gösterir.
      KEK ve anahtar çözücüörnek uygulamaları sırasıyla KeyWrapper ve KeyResolver olarak örnek dosyalarda sağlanır.

### <a name="requireencryption-mode"></a>Şifreleme modu gerektirir
Kullanıcılar isteğe bağlı olarak tüm yüklemelerin ve indirmelerin şifrelenmesi gereken bir çalışma moduna olanak sağlayabilir. Bu modda, şifreleme ilkesi olmadan veri yükleme veya hizmette şifrelenmemiş verileri indirme girişimleri istemcide başarısız olur. Hizmet nesnesindeki **require_encryption** bayrağı bu davranışı denetler.

### <a name="blob-service-encryption"></a>Blob hizmet şifrelemesi
Blockblobservice nesnesindeki şifreleme ilkesi alanlarını ayarlayın. Diğer her şey istemci kitaplığı tarafından dahili olarak ele alınacaktır.

```python
# Create the KEK used for encryption.
# KeyWrapper is the provided sample implementation, but the user may use their own object as long as it implements the interface above.
kek = KeyWrapper('local:key1')  # Key identifier

# Create the key resolver used for decryption.
# KeyResolver is the provided sample implementation, but the user may use whatever implementation they choose so long as the function set on the service object behaves appropriately.
key_resolver = KeyResolver()
key_resolver.put_key(kek)

# Set the KEK and key resolver on the service object.
my_block_blob_service.key_encryption_key = kek
my_block_blob_service.key_resolver_funcion = key_resolver.resolve_key

# Upload the encrypted contents to the blob.
my_block_blob_service.create_blob_from_stream(
    container_name, blob_name, stream)

# Download and decrypt the encrypted contents from the blob.
blob = my_block_blob_service.get_blob_to_bytes(container_name, blob_name)
```

### <a name="queue-service-encryption"></a>Sıra hizmeti şifrelemesi
Sıra hizmeti nesnesindeki şifreleme ilkesi alanlarını ayarlayın. Diğer her şey istemci kitaplığı tarafından dahili olarak ele alınacaktır.

```python
# Create the KEK used for encryption.
# KeyWrapper is the provided sample implementation, but the user may use their own object as long as it implements the interface above.
kek = KeyWrapper('local:key1')  # Key identifier

# Create the key resolver used for decryption.
# KeyResolver is the provided sample implementation, but the user may use whatever implementation they choose so long as the function set on the service object behaves appropriately.
key_resolver = KeyResolver()
key_resolver.put_key(kek)

# Set the KEK and key resolver on the service object.
my_queue_service.key_encryption_key = kek
my_queue_service.key_resolver_funcion = key_resolver.resolve_key

# Add message
my_queue_service.put_message(queue_name, content)

# Retrieve message
retrieved_message_list = my_queue_service.get_messages(queue_name)
```

### <a name="table-service-encryption"></a>Tablo hizmeti şifrelemesi
Bir şifreleme ilkesi oluşturmanın ve istek seçeneklerine ayarlanın yanı sıra, **tablo hizmetinde**bir **encryption_resolver_function** belirtmeniz veya EntityProperty'deki şifreleme özniteliğini ayarlamanız gerekir.

### <a name="using-the-resolver"></a>Çözümleyiciyi kullanma

```python
# Create the KEK used for encryption.
# KeyWrapper is the provided sample implementation, but the user may use their own object as long as it implements the interface above.
kek = KeyWrapper('local:key1')  # Key identifier

# Create the key resolver used for decryption.
# KeyResolver is the provided sample implementation, but the user may use whatever implementation they choose so long as the function set on the service object behaves appropriately.
key_resolver = KeyResolver()
key_resolver.put_key(kek)

# Define the encryption resolver_function.


def my_encryption_resolver(pk, rk, property_name):
    if property_name == 'foo':
        return True
    return False


# Set the KEK and key resolver on the service object.
my_table_service.key_encryption_key = kek
my_table_service.key_resolver_funcion = key_resolver.resolve_key
my_table_service.encryption_resolver_function = my_encryption_resolver

# Insert Entity
my_table_service.insert_entity(table_name, entity)

# Retrieve Entity
# Note: No need to specify an encryption resolver for retrieve, but it is harmless to leave the property set.
my_table_service.get_entity(
    table_name, entity['PartitionKey'], entity['RowKey'])
```

### <a name="using-attributes"></a>Öznitelikleri kullanma
Yukarıda belirtildiği gibi, bir özellik bir EntityProperty nesnesinde depolayarak ve şifreleme alanını ayarlayarak şifreleme için işaretlenmiş olabilir.

```python
encrypted_property_1 = EntityProperty(EdmType.STRING, value, encrypt=True)
```

## <a name="encryption-and-performance"></a>Şifreleme ve performans
Depolama verilerinizi şifrelemenin ek performans yüküyle sonuçladığını unutmayın. İçerik anahtarı ve IV oluşturulmalı, içeriğin kendisi şifrelenmeli ve ek meta veriler biçimlendirilmeli ve yüklenmelidir. Bu ek yükü, şifrelenen veri miktarına bağlı olarak değişir. Müşterilerin geliştirme sırasında uygulamalarını her zaman performans için test etmelerine tavsiye ediyoruz.

## <a name="next-steps"></a>Sonraki adımlar
* Java [PyPi paketi için Azure Depolama İstemci Kitaplığını](https://pypi.python.org/pypi/azure-storage) indirin
* [GitHub'dan Python Kaynak Kodu için Azure Depolama İstemci Kitaplığını Indirin](https://github.com/Azure/azure-storage-python)
