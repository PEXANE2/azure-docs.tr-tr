---
title: Azure Blob depolama içeriğini arama
titleSuffix: Azure Cognitive Search
description: Azure Bloblarından metin ayıklama ve Azure Bilişsel Arama dizininde tam metin araması yapma hakkında bilgi edinin.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 09/23/2020
ms.openlocfilehash: f61bf635cc61a2153a7bb016ef4b4711d7ba7391
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "91355304"
---
# <a name="search-over-azure-blob-storage-content"></a>Azure Blob depolama içeriğini arama

Azure Blob depolamada depolanan çeşitli içerik türleri arasında arama yapmak zor bir sorun olabilir. Bu makalede, bloblardan içerik ve meta verileri ayıklamak ve Azure Bilişsel Arama bir arama dizinine göndermek için temel iş akışını gözden geçirin. Elde edilen dizin, tam metin araması kullanılarak sorgulanabilir.

> [!NOTE]
> İş akışı ve birleşim zaten tanıdık mı? [Bir blob Dizin oluşturucuyu yapılandırma bir](search-howto-indexing-azure-blob-storage.md) sonraki adımınız.

## <a name="what-it-means-to-add-full-text-search-to-blob-data"></a>Blob verilerine tam metin araması eklemenin anlamı

Azure Bilişsel Arama, bulutta barındırılan uzaktan aranabilir içeriğinizi içeren Kullanıcı tanımlı dizinlerde Dizin oluşturmayı ve sorgu iş yüklerini destekleyen bir arama hizmetidir. Sorgu altyapısı ile aranabilir içeriğinizi birlikte bulmak, performans için gereklidir ve bu da sonuçları, arama sorgularının beklediği bir hızda geri döndürür.

Bilişsel Arama, dizin oluşturma katmanında Azure Blob depolama ile tümleşir ve bu, serbest biçimli metin sorgularını ve Filtre ifadelerini destekleyen, *ters dizinler* ve diğer sorgu yapılarına dizin oluşturulan arama belgeleri olarak içeri aktarın. Blob içeriğiniz bir arama dizininde oluşturulduğundan, blob içerikinizdeki bilgileri bulmak için Azure Bilişsel Arama 'deki sorgu özelliklerinin tam aralığını kullanabilirsiniz.

Azure Blob depolama alanında, tek bir kapsayıcıda bulunan bloblarınızın girdileri vardır. Blob 'lar neredeyse her türlü metin verisi olabilir. Bloblarınız görüntü içeriyorsa, görüntülerden metin oluşturup çıkarmak için bir [BLOB dizin oluşturmaya AI zenginleştirme](search-blob-ai-integration.md) ekleyebilirsiniz.

Çıktı, istemci uygulamalarında hızlı metin arama, alma ve araştırma için kullanılan her zaman bir Azure Bilişsel Arama dizinidir. Between, dizin oluşturma ardışık düzen mimarisidir. İşlem hattı, bu makalede daha fazla açıklanılan *Dizin Oluşturucu* özelliğine dayanır.

Dizin oluşturulduktan ve doldurulduktan sonra, blob kapsayıcınızda bağımsız olarak bulunur, ancak dizin oluşturma işlemlerini, dizini değiştirilen belgelere göre yenilemek için tekrar çalıştırabilirsiniz. Tek bloblarda zaman damgası bilgileri değişiklik algılama için kullanılır. Yenileme mekanizması olarak zamanlanmış yürütme ya da isteğe bağlı dizin oluşturma için kabul edebilirsiniz.

## <a name="required-resources"></a>Gerekli kaynaklar

Azure Bilişsel Arama ve Azure Blob depolama alanı gerekir. BLOB depolama alanında, kaynak içerik sağlayan bir kapsayıcıya ihtiyacınız vardır.

Doğrudan depolama hesabı portalı sayfanızda başlayabilirsiniz. Sol Gezinti sayfasında, **BLOB hizmeti** altında **Azure bilişsel arama Ekle** ' ye tıklayarak yeni bir hizmet oluşturun veya var olan bir hizmeti seçin. 

Depolama hesabınıza Azure Bilişsel Arama eklediğinizde, blob verilerini indekslemek için standart işlemi izleyebilirsiniz. Kolay bir başlangıç girişi için Azure Bilişsel Arama **veri alma** Sihirbazı 'nı öneririz veya Postman gibi bir araç kullanarak REST API 'leri çağırabilirsiniz. Bu öğretici, Postman 'da REST API çağırma adımlarında size yol gösterir: [Azure bilişsel arama 'de yarı yapılandırılmış verileri (JSON blob 'ları) dizin ve arama](search-semi-structured-data.md). 

## <a name="use-a-blob-indexer"></a>Blob Dizin Oluşturucu kullanma

Bir *Dizin Oluşturucu* , verileri örnekleme, meta veri verileri okuma, verileri alma ve yerel biçimlerden verileri, sonraki içeri aktarma için JSON belgelerine serileştirmede çalışan, bilişsel arama veri kaynağı kullanan bir alt hizmettir. 

Azure depolama 'daki Bloblar, [azure bilişsel arama blob Storage Indexer](search-howto-indexing-azure-blob-storage.md)kullanılarak dizine alınır. **Veri alma** Sihirbazı 'nı, bir REST API veya .NET SDK 'sını kullanarak bu dizin oluşturucuyu çağırabilirsiniz. Kod içinde, bu dizin oluşturucuyu türü ayarlayarak ve bir blob kapsayıcısı ile birlikte bir Azure depolama hesabı içeren bağlantı bilgilerini sağlayarak kullanırsınız. Daha sonra bir parametre olarak geçirebilen veya bir dosya türü uzantısı üzerinde filtreleyerek sanal bir dizin oluşturarak bloblarınızı alt kümelayabilirsiniz.

Bir Dizin Oluşturucu, içeriği incelemek için bir blob açan "belgeyi çözme" yapmaz. Veri kaynağına bağlandıktan sonra, işlem hattının ilk adımı vardır. Blob verileri için PDF, Office belgeleri ve diğer içerik türlerinin algılandığı yerdir. Metin ayıklama ile belge çözme ücretsizdir. Bloblarınız görüntü içeriği içeriyorsa, [AI zenginleştirme](search-blob-ai-integration.md)eklemediğiniz takdirde görüntüler yok sayılır. Standart dizin oluşturma yalnızca metin içeriği için geçerlidir.

Blob Indexer yapılandırma parametreleriyle birlikte gelir ve temel alınan veriler yeterli bilgi sağlıyorsa değişiklik izlemeyi destekler. [Azure bilişsel arama blob Storage Indexer](search-howto-indexing-azure-blob-storage.md)'ın temel işlevleri hakkında daha fazla bilgi edinebilirsiniz.

### <a name="supported-content-types"></a>Desteklenen içerik türleri

Bir kapsayıcı üzerinde bir blob Indexer çalıştırarak, aşağıdaki içerik türlerinden metin ve meta verileri tek bir sorgu ile ayıklayabilirsiniz:

[!INCLUDE [search-blob-data-sources](../../includes/search-blob-data-sources.md)]

### <a name="indexing-blob-metadata"></a>Blob meta verileri dizinleniyor

Herhangi bir içerik türünün Blobları arasında sıralama yapmayı kolaylaştıran yaygın bir senaryo, her blob için hem özel meta verileri hem de sistem özelliklerini dizinlemek. Bu şekilde, tüm Bloblar için bilgiler, arama hizmetinizde bir dizinde depolanan belge türünden bağımsız olarak dizinlenir. Yeni dizininizi kullanarak, tüm blob depolama içeriklerine göre sıralama, filtreleme ve model işleme devam edebilirsiniz.

> [!NOTE]
> Blob dizin etiketleri, BLOB depolama hizmeti tarafından yerel olarak dizinlenir ve sorgulama için sunulur. Bloblarınızın anahtar/değer öznitelikleri dizin oluşturma ve filtreleme özellikleri gerektiriyorsa, blob dizini etiketlerinin meta veriler yerine yararlanılabilir olması gerekir.
>
> Blob dizini hakkında daha fazla bilgi edinmek için bkz. [BLOB diziniyle Azure Blob depolama üzerinde verileri yönetme ve bulma](../storage/blobs/storage-manage-find-blobs.md).

### <a name="indexing-json-blobs"></a>JSON bloblarını dizine alma

Dizin oluşturucular, JSON içeren bloblarda bulunan yapılandırılmış içerikleri ayıklamak üzere yapılandırılabilir. Bir Dizin Oluşturucu, JSON bloblarını okuyabilir ve yapılandırılmış içeriği bir arama belgesinin uygun alanlarıyla ayrıştırır. Dizin oluşturucular Ayrıca, bir dizi JSON nesnesi içeren Bloblar alabilir ve her öğeyi ayrı bir arama belgesiyle eşler. Dizin Oluşturucu tarafından oluşturulan JSON nesnesinin türünü etkilemek için bir ayrıştırma modu ayarlayabilirsiniz.

## <a name="search-blob-content-in-a-search-index"></a>Arama dizininde blob içeriğini ara 

Bir dizin oluşturucunun çıktısı, bir istemci uygulamasında ücretsiz metin ve filtrelenmiş sorgular kullanan etkileşimli araştırma için kullanılan bir arama dizinidir. İçeriğin ilk araştırılması ve doğrulanması için, belge yapısını incelemek üzere portalda [Arama Gezgini](search-explorer.md) ile başlamasını öneririz. Arama Gezgini 'nde [basit sorgu söz dizimi](query-simple-syntax.md), [tam sorgu söz](query-lucene-syntax.md)dizimi ve [filtre ifadesi sözdizimini](query-odata-filter-orderby-syntax.md) kullanabilirsiniz.

Daha kalıcı bir çözüm, sorgu girişleri toplamaktır ve yanıtı bir istemci uygulamasında arama sonuçları olarak sunar. Aşağıdaki C# öğreticisi, bir arama uygulamasının nasıl oluşturulduğunu açıklar: [Azure bilişsel arama ilk uygulamanızı oluşturma](tutorial-csharp-create-first-app.md).

## <a name="next-steps"></a>Sonraki adımlar

+ [Azure portal (Azure Blob depolama) ile Blobları karşıya yükleme, indirme ve listeleme](../storage/blobs/storage-quickstart-blobs-portal.md)
+ [Blob Dizin oluşturucuyu ayarlama (Azure Bilişsel Arama)](search-howto-indexing-azure-blob-storage.md)