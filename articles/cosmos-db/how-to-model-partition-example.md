---
title: Azure Cosmos DB'deki verileri gerçek dünya örneğiyle modelleme ve bölümleme
description: Azure Cosmos DB Core API'yi kullanarak gerçek dünya örneğini nasıl modelleyip bölümlendireceklerini öğrenin
author: ThomasWeiss
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/23/2019
ms.author: thweiss
ms.openlocfilehash: 10f8ffd90215a21ca03e112aea463d444c623d06
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75445386"
---
# <a name="how-to-model-and-partition-data-on-azure-cosmos-db-using-a-real-world-example"></a>Gerçek dünyadan bir örnek kullanarak Azure Cosmos DB'de verileri modelleme ve bölümleme

Bu makalede, gerçek dünya veri tasarımı [alıştırması](modeling-data.md)ile nasıl başa çıta olunan veri modelleme, [bölümleme](partitioning-overview.md)ve [sağlanan iş gücü](request-units.md) gibi çeşitli Azure Cosmos DB kavramları temel alınız.

Genellikle ilişkisel veritabanları ile çalışıyorsanız, büyük olasılıkla bir veri modeli tasarlamak için nasıl alışkanlıkları ve sezgileri inşa var. Azure Cosmos DB'nin benzersiz güçlü yanları ve belirli kısıtlamaları nedeniyle, bu en iyi uygulamaların çoğu iyi bir şekilde çevrilemez ve sizi en uygun olmayan çözümlere sürükleyebilir. Bu makalenin amacı, Azure Cosmos DB'de gerçek bir kullanım örneğini modelleme sürecinde, öğe modellemeden varlık birlikteliği ve kapsayıcı bölümlemesine kadar tüm süreç boyunca size rehberlik etmektir.

## <a name="the-scenario"></a>Senaryo

Bu egzersiz için, *kullanıcıların* *mesaj*oluşturabilirsiniz bir bloglama platformuetki dikkate gidiyoruz. Kullanıcılar bu gönderileri *beğenebilir* ve bu gönderilere *yorum* ekleyebilir.

> [!TIP]
> Biz *italik*bazı kelimeler vurgulanır; bu kelimeler modelimizin manipüle etmek zorunda kalacakları "şeyleri" tanımlar.

Belirtimimize daha fazla gereksinim ekleme:

- Ön sayfa, son oluşturulan gönderilerin akışını görüntüler,
- Biz bir kullanıcı için tüm mesajları getirebilir, bir yazı için tüm yorumlar ve bir yazı için tüm seviyor,
- Gönderiler, yazarlarının kullanıcı adı ve kaç yorum ve beğeniye sahip oldukları yla birlikte döndürülür,
- Yorumlar ve beğenmeler, bunları oluşturan kullanıcıların kullanıcı adı ile birlikte iade edilir,
- Listeler olarak görüntülendiğinde, gönderilerin yalnızca içeriklerinin kesilmiş bir özetini sunması gerekir.

## <a name="identify-the-main-access-patterns"></a>Ana erişim modellerini belirleme

Başlamak için, çözümümüzün erişim modellerini belirleyerek ilk belirtimimize bir miktar yapı veririz. Azure Cosmos DB için bir veri modeli tasarlarken, modelimizin bu isteklere verimli bir şekilde sunmasını sağlamak için hangi isteklerin hizmet etmesi gerekeceğini anlamak önemlidir.

Genel işlemin takibini kolaylaştırmak için, bu farklı istekleri komut lar veya sorgular olarak kategorilere ayırıyoruz, komutların yazma istekleriolduğu [CQRS'den](https://en.wikipedia.org/wiki/Command%E2%80%93query_separation#Command_query_responsibility_segregation) bazı kelimeler ödünç alıyoruz (diğer bir süre, sistemi güncelleştirme niyetleri) ve sorgular salt okunur isteklerdir.

Platformumuzun ortaya çıkarmak zorunda kalacak taleplerin listesi aşağıdadır:

- **[C1]** Kullanıcı oluşturma/değiştirme
- **[Q1]** Bir kullanıcı yı alma
- **[C2]** Gönderi oluşturma/değiştirme
- **[Q2]** Gönderi alma
- **[Q3]** Kullanıcının gönderilerini kısa formda listelama
- **[C3]** Yorum oluşturma
- **[Q4]** Gönderinin yorumlarını listele
- **[C4]** Bir gönderi gibi
- **[Q5]** Gönderinin beğenmelerini listele
- **[Q6]** Kısa formda oluşturulan *x* en son gönderileri listeleyin (besleme)

Bu aşamada, her varlığın (kullanıcı, gönderi vb.) ne içereceği yle ilgili ayrıntıları düşünmedik. Bu adım genellikle ilişkisel bir mağazaya karşı tasarlanırken ele alınacak ilk adımlardır, çünkü bu varlıkların tablolar, sütunlar, yabancı anahtarlar vb. açısından nasıl tercüme edeceğini çözmemiz gerekir. Bu, yazmada herhangi bir şema zorlamayan bir belge veritabanı ile ilgili çok daha az bir sorundur.

Erişim kalıplarımızı en başından beri tanımlamanın önemli olmasının temel nedeni, bu istek listesinin test paketimiz olmasıdır. Veri modelimizi her tekrarladığımızda, her bir istek üzerinden geçeceğiz ve performansını ve ölçeklenebilirliğini kontrol edeceğiz.

## <a name="v1-a-first-version"></a>V1: İlk sürüm

İki konteynerle başlıyoruz: `users` ve. `posts`

### <a name="users-container"></a>Kullanıcılar konteyner

Bu kapsayıcı yalnızca kullanıcı öğelerini depolar:

    {
      "id": "<user-id>",
      "username": "<username>"
    }

Bu `id`kapsayıcıyı, bu kapsayıcının içindeki her mantıksal bölümün yalnızca bir öğe içereceği anlamına gelir.

### <a name="posts-container"></a>Posta konteyner

Bu kapsayıcı gönderileri, yorumları ve beğenilerini barındırıyor:

    {
      "id": "<post-id>",
      "type": "post",
      "postId": "<post-id>",
      "userId": "<post-author-id>",
      "title": "<post-title>",
      "content": "<post-content>",
      "creationDate": "<post-creation-date>"
    }

    {
      "id": "<comment-id>",
      "type": "comment",
      "postId": "<post-id>",
      "userId": "<comment-author-id>",
      "content": "<comment-content>",
      "creationDate": "<comment-creation-date>"
    }

    {
      "id": "<like-id>",
      "type": "like",
      "postId": "<post-id>",
      "userId": "<liker-id>",
      "creationDate": "<like-creation-date>"
    }

Biz bu kapsayıcı `postId`bölüm , bu kapsayıcı içinde her mantıksal bölüm bir yazı, bu yazı için tüm yorumlar ve bu yazı için tüm seviyor içerecektir anlamına gelir.

Bu kapsayıcıda depolanan `type` öğelerde, bu kapsayıcının barındırdığı üç varlık türünü ayırt etmek için bir özellik getirdiğimizi unutmayın.

Ayrıca, ilgili verileri katıştırmak yerine referans etmeyi seçtik (bu kavramlarla ilgili ayrıntılar için [bu bölümü](modeling-data.md) kontrol edin) çünkü:

- bir kullanıcının oluşturabileceği gönderilerin üst sınırı yoktur,
- mesajlar keyfi uzun olabilir,
- bir gönderinin kaç yorum ve beğeniye sahip olabileceğine dair bir üst sınır yoktur,
- gönderinin kendisini güncellemek zorunda kalmadan bir gönderiye yorum veya beğenme eklemek istiyoruz.

## <a name="how-well-does-our-model-perform"></a>Modelimiz ne kadar iyi performans sergiliyor?

Şimdi ilk sürümümüzün performansını ve ölçeklenebilirliğini değerlendirme zamanı. Daha önce tanımlanan isteklerin her biri için gecikme sinin ve kaç istek biriminin tükettiği ölçültü. Bu ölçüm, kullanıcı başına 5 ila 50 mesaj içeren 100.000 kullanıcı ve gönderi başına en fazla 25 yorum ve 100 beğeni içeren sahte bir veri kümesine karşı yapılır.

### <a name="c1-createedit-a-user"></a>[C1] Kullanıcı oluşturma/değiştirme

Bu isteği, `users` kapsayıcıda bir öğe oluştururken veya güncelleştirirken uygulamak kolaydır. İstekler, bölüm tuşu sayesinde tüm `id` bölümlere güzelce yayılacaktır.

![Kullanıcı kapsayıcısına tek bir öğe yazma](./media/how-to-model-partition-example/V1-C1.png)

| **Gecikme süresi** | **RU şarj** | **Performans** |
| --- | --- | --- |
| 7 ms | 5.71 RU | ✅ |

### <a name="q1-retrieve-a-user"></a>[Q1] Bir kullanıcı yı alma

Kullanıcının `users` alınması, ilgili öğeyi kapsayıcıdan okuyarak yapılır.

![Tek bir öğeyi kullanıcı kapsayıcısından alma](./media/how-to-model-partition-example/V1-Q1.png)

| **Gecikme süresi** | **RU şarj** | **Performans** |
| --- | --- | --- |
| 2 ms | 1 RU | ✅ |

### <a name="c2-createedit-a-post"></a>[C2] Gönderi oluşturma/değiştirme

Benzer şekilde **[C1]**, biz sadece `posts` konteyner yazmak zorunda.

![Gönderi kapsayıcısına tek bir öğe yazma](./media/how-to-model-partition-example/V1-C2.png)

| **Gecikme süresi** | **RU şarj** | **Performans** |
| --- | --- | --- |
| 9 ms | 8.76 RU | ✅ |

### <a name="q2-retrieve-a-post"></a>[Q2] Gönderi alma

İlgili belgeyi `posts` konteynerden alarak başlıyoruz. Ama bu yeterli değil, bizim belirtim uyarınca biz de gönderinin yazarın kullanıcı adı ve kaç yorum ve kaç bu yazı, 3 ek SQL sorguları verilmesi gereken sayısı toplamak zorunda.

![Gönderi alma ve ek veri toplama](./media/how-to-model-partition-example/V1-Q2.png)

Ek sorguların her biri, performansı ve ölçeklenebilirliği en üst düzeye çıkarmak istediğimiz şey olan ilgili kapsayıcının bölüm anahtarındaki filtrelerden biridir. Ama sonunda tek bir yazı dönmek için dört işlem gerçekleştirmek zorunda, bu yüzden bir sonraki yineleme de bunu artıracağız.

| **Gecikme süresi** | **RU şarj** | **Performans** |
| --- | --- | --- |
| 9 ms | 19,54 RU | ⚠ |

### <a name="q3-list-a-users-posts-in-short-form"></a>[Q3] Kullanıcının gönderilerini kısa formda listelama

İlk olarak, belirli bir kullanıcıya karşılık gelen gönderileri getiren bir SQL sorgusu ile istenen gönderileri almamız gerekir. Ancak yazarın kullanıcı adını ve yorum ve beğeni sayılarını toplamak için ek sorgular da düzenlememiz gerekir.

![Bir kullanıcı için tüm gönderileri alma ve ek verilerini toplama](./media/how-to-model-partition-example/V1-Q3.png)

Bu uygulama birçok dezavantajları sunar:

- yorum ve beğeni sayılarını bir araya alan sorguların, ilk sorgu tarafından döndürülen her gönderi için düzenlenmesi,
- ana `posts` sorgu, kapsayıcının bölüm tuşuna filtre uygulamaz ve bu da bir fan-out ve kapsayıcı boyunca bir bölüm tadına yol açmaz.

| **Gecikme süresi** | **RU şarj** | **Performans** |
| --- | --- | --- |
| 130 ms | 619,41 RU | ⚠ |

### <a name="c3-create-a-comment"></a>[C3] Yorum oluşturma

Bir açıklama `posts` kapsayıcıda ilgili öğe yazarak oluşturulur.

![Gönderi kapsayıcısına tek bir öğe yazma](./media/how-to-model-partition-example/V1-C2.png)

| **Gecikme süresi** | **RU şarj** | **Performans** |
| --- | --- | --- |
| 7 ms | 8.57 RU | ✅ |

### <a name="q4-list-a-posts-comments"></a>[Q4] Gönderinin yorumlarını listele

Bu gönderi için tüm yorumları getiren bir sorguyla başlarız ve bir kez daha, her yorum için ayrı ayrı kullanıcı adlarını toplamamız gerekir.

![Bir gönderi için tüm yorumları alma ve ek verilerini toplama](./media/how-to-model-partition-example/V1-Q4.png)

Ana sorgu kapsayıcının bölüm anahtarına filtre uygulamasa da, kullanıcı adlarını ayırmak genel performansı ayrı ayrı cezalandırır. Bunu daha sonra geliştireceğiz.

| **Gecikme süresi** | **RU şarj** | **Performans** |
| --- | --- | --- |
| 23 ms | 27.72 RU | ⚠ |

### <a name="c4-like-a-post"></a>[C4] Bir gönderi gibi

**[C3]** gibi, kapsayıcıda karşılık gelen `posts` öğeyi oluştururuz.

![Gönderi kapsayıcısına tek bir öğe yazma](./media/how-to-model-partition-example/V1-C2.png)

| **Gecikme süresi** | **RU şarj** | **Performans** |
| --- | --- | --- |
| 6 ms | 7.05 RU | ✅ |

### <a name="q5-list-a-posts-likes"></a>[Q5] Gönderinin beğenmelerini listele

**[Q4]** gibi, biz bu yazı için seviyor sorgulayın, sonra kullanıcı adlarını toplamak.

![Bir gönderi için tüm beğenileri alma ve ek verilerini toplama](./media/how-to-model-partition-example/V1-Q5.png)

| **Gecikme süresi** | **RU şarj** | **Performans** |
| --- | --- | --- |
| 59 ms | 58,92 RU | ⚠ |

### <a name="q6-list-the-x-most-recent-posts-created-in-short-form-feed"></a>[Q6] Kısa formda oluşturulan x en son gönderileri listeleyin (besleme)

Azalan oluşturma tarihine göre sıralanan kapsayıcıyı `posts` sorgulayarak en son gönderileri getiririz, ardından her gönderi için kullanıcı adlarını ve yorum ve beğenileri toplarız.

![En son gönderileri alma ve ek verileri toplama](./media/how-to-model-partition-example/V1-Q6.png)

Bir kez daha, ilk sorgumuz `posts` kapsayıcının bölüm tuşuna filtre uygulamaz ve bu da pahalı bir fan-out'u tetikler. Biz çok daha büyük bir sonuç seti hedef ve istek `ORDER BY` birimleri açısından daha pahalı kılan bir yan tümce ile sonuçları sıralamak gibi bu daha da kötüdür.

| **Gecikme süresi** | **RU şarj** | **Performans** |
| --- | --- | --- |
| 306 ms | 2063,54 RU | ⚠ |

## <a name="reflecting-on-the-performance-of-v1"></a>V1'in performansını yansıtma

Bir önceki bölümde karşılaştığımız performans sorunlarına baktığımızda, iki ana sorun sınıfını tanımlayabiliriz:

- bazı istekler, döndürmek için gereken tüm verileri toplamak için birden çok sorgu yapılmasını gerektirir,
- bazı sorgular, hedefledikleri kapsayıcıların bölüm anahtarına filtre uygulamaz ve ölçeklenebilirliğimizi engelleyen bir fan-out'a yol açan bir

İlkinden başlayarak bu sorunların her birini çözelim.

## <a name="v2-introducing-denormalization-to-optimize-read-queries"></a>V2: Okuma sorgularını optimize etmek için normalleştirme yi tanıtma

Bazı durumlarda ek isteklerde bulunmamızın nedeni, ilk isteğin sonuçlarının döndürmemiz gereken tüm verileri içermemesidir. Azure Cosmos DB gibi ilişkisel olmayan bir veri deposuyla çalışırken, bu tür bir sorun genellikle veri kümemizdeki verileri normalden arındırarak çözülür.

Örneğimizde, gönderi yazarının kullanıcı adını, yorum sayısını ve beğenme sayısını eklemek için gönderi öğelerini değiştiririz:

    {
      "id": "<post-id>",
      "type": "post",
      "postId": "<post-id>",
      "userId": "<post-author-id>",
      "userUsername": "<post-author-username>",
      "title": "<post-title>",
      "content": "<post-content>",
      "commentCount": <count-of-comments>,
      "likeCount": <count-of-likes>,
      "creationDate": "<post-creation-date>"
    }

Ayrıca, bunları oluşturan kullanıcının kullanıcı adını eklemek için yorum ve benzeri öğeleri de değiştiririz:

    {
      "id": "<comment-id>",
      "type": "comment",
      "postId": "<post-id>",
      "userId": "<comment-author-id>",
      "userUsername": "<comment-author-username>",
      "content": "<comment-content>",
      "creationDate": "<comment-creation-date>"
    }

    {
      "id": "<like-id>",
      "type": "like",
      "postId": "<post-id>",
      "userId": "<liker-id>",
      "userUsername": "<liker-username>",
      "creationDate": "<like-creation-date>"
    }

### <a name="denormalizing-comment-and-like-counts"></a>Yorumu ve benzeri sayıları normalleştirme

Ne elde etmek istiyorum biz bir yorum veya benzeri eklemek her zaman, biz de artış `commentCount` veya `likeCount` ilgili yazı. Bizim `posts` kapsayıcı tarafından `postId`bölümlenmiş olarak, yeni öğe (yorum veya benzer) ve ilgili sonrası aynı mantıksal bölüm içinde oturur. Sonuç olarak, bu işlemi gerçekleştirmek için [depolanmış](stored-procedures-triggers-udfs.md) bir yordam kullanabiliriz.

Şimdi bir yorum oluştururken **([C3]**), sadece bu `posts` kapsayıcı üzerinde aşağıdaki depolanmış yordam diyoruz kapsayıcıya yeni bir öğe eklemek yerine:

```javascript
function createComment(postId, comment) {
  var collection = getContext().getCollection();

  collection.readDocument(
    `${collection.getAltLink()}/docs/${postId}`,
    function (err, post) {
      if (err) throw err;

      post.commentCount++;
      collection.replaceDocument(
        post._self,
        post,
        function (err) {
          if (err) throw err;

          comment.postId = postId;
          collection.createDocument(
            collection.getSelfLink(),
            comment
          );
        }
      );
    })
}
```

Bu depolanan yordam, gönderinin kimliğini ve yeni yorumun gövdesini parametre olarak alır ve sonra:

- gönderiyi alır
- artışlar`commentCount`
- postanın yerini alır
- yeni yorum ekler

Saklanan yordamlar atomik hareketler olarak yürütüldükçe, yorum `commentCount` değerinin ve gerçek sayısının her zaman eşit kalacağı garanti edilir.

Biz açıkçaartış için yeni beğeniler eklerken benzer bir `likeCount`saklı yordam diyoruz.

### <a name="denormalizing-usernames"></a>Kullanıcı adlarını normalden arındıran

Kullanıcılar yalnızca farklı bölümlerde değil, aynı zamanda farklı bir kapsayıcıda da yer almaktadır. Verileri bölümler ve kapsayıcılar arasında normalden arındırmak zorunda kaldığımız zaman, kaynak kapsayıcının [değişiklik beslemesini](change-feed.md)kullanabiliriz.

Örneğimizde, kullanıcılar kullanıcı adlarını `users` güncelleştirse tepki vermek için kapsayıcının değişiklik akışını kullanırız. Bu durumda, `posts` kapsayıcıda başka bir depolanmış yordam çağırarak değişikliği yayıyoruz:

![Kullanıcı adlarını gönderi kapsayıcısına normalleştirme](./media/how-to-model-partition-example/denormalization-1.png)

```javascript
function updateUsernames(userId, username) {
  var collection = getContext().getCollection();
  
  collection.queryDocuments(
    collection.getSelfLink(),
    `SELECT * FROM p WHERE p.userId = '${userId}'`,
    function (err, results) {
      if (err) throw err;

      for (var i in results) {
        var doc = results[i];
        doc.userUsername = username;

        collection.upsertDocument(
          collection.getSelfLink(),
          doc);
      }
    });
}
```

Bu depolanan yordam, kullanıcının kimliğini ve kullanıcının yeni kullanıcı adını parametre olarak alır ve sonra:

- eşleşen tüm öğeleri `userId` getirir (gönderiler, yorumlar veya beğenmeler olabilir)
- bu öğelerin her biri için
  - yerini`userUsername`
  - öğenin yerini alır

> [!IMPORTANT]
> Bu işlem, depolanan yordamın `posts` kapsayıcının her bölümüne yürütülmesini gerektirdiğinden maliyetlidir. Çoğu kullanıcının kaydolma sırasında uygun bir kullanıcı adı seçtiğini ve bu kullanıcıyı hiç değiştirmediğini varsayıyoruz, bu nedenle bu güncelleştirme çok nadiren yayınlanacaktır.

## <a name="what-are-the-performance-gains-of-v2"></a>V2'nin performans kazanımları nelerdir?

### <a name="q2-retrieve-a-post"></a>[Q2] Gönderi alma

Artık normali şu raydan çıktığına göre, bu isteği yerine getirmek için tek bir öğe getirmemiz gerekiyor.

![Gönderi ler kapsayıcısından tek bir öğe alma](./media/how-to-model-partition-example/V2-Q2.png)

| **Gecikme süresi** | **RU şarj** | **Performans** |
| --- | --- | --- |
| 2 ms | 1 RU | ✅ |

### <a name="q4-list-a-posts-comments"></a>[Q4] Gönderinin yorumlarını listele

Burada yine, kullanıcı adlarını getiren ek istekleri yedekleyebilir ve bölüm anahtarına filtre leyen tek bir sorguyla son bulabiliriz.

![Bir gönderi için tüm yorumları alma](./media/how-to-model-partition-example/V2-Q4.png)

| **Gecikme süresi** | **RU şarj** | **Performans** |
| --- | --- | --- |
| 4 ms | 7.72 RU | ✅ |

### <a name="q5-list-a-posts-likes"></a>[Q5] Gönderinin beğenmelerini listele

Beğenileri listelerken de aynı durum.

![Bir gönderi için tüm beğenileri alma](./media/how-to-model-partition-example/V2-Q5.png)

| **Gecikme süresi** | **RU şarj** | **Performans** |
| --- | --- | --- |
| 4 ms | 8.92 RU | ✅ |

## <a name="v3-making-sure-all-requests-are-scalable"></a>V3: Tüm isteklerin ölçeklenebilir olduğundan emin olunması

Genel performans iyileştirmelerimize baktığımızda, hala tam olarak optimize etmediğimiz iki istek vardır: **[Q3]** ve **[Q6]**. Bunlar, hedefledikleri kapsayıcıların bölüm anahtarına filtre uygulamayan sorguları içeren isteklerdir.

### <a name="q3-list-a-users-posts-in-short-form"></a>[Q3] Kullanıcının gönderilerini kısa formda listelama

Bu istek, v2'de sunulan ve ek sorguları yedekleyen geliştirmelerden zaten yararlanır.

![Bir kullanıcı için tüm gönderileri alma](./media/how-to-model-partition-example/V2-Q3.png)

Ancak kalan sorgu hala `posts` kapsayıcının bölüm anahtarı üzerinde filtreleme değildir.

Bu durum hakkında düşünmek için yol aslında basittir:

1. Belirli *has* bir kullanıcı için `userId` tüm gönderileri getirmek istediğimiz için bu isteğin filtrele olması gerekiyor
1. Tarafından bölümlenmiyor `posts` kapsayıcı, karşı yürütülür, çünkü iyi performans yok`userId`
1. Bariz belirterek, biz *bölümlenmiş* bir kapsayıcı karşı bu isteği yürüterek performans sorunu çözmek istiyorsunuz`userId`
1. Zaten böyle bir konteyner var çıkıyor: `users` konteyner!

Bu yüzden `users` konteyner için tüm mesajları çoğaltarak denormalizasyon ikinci bir düzey tanıtmak. Bunu yaparak, etkili bizim mesaj bir kopyasını almak, sadece farklı boyutlarda bölümlenmiş, onları yol `userId`daha verimli onların tarafından almak için yapım .

Kapsayıcı `users` şimdi 2 tür öğe içerir:

    {
      "id": "<user-id>",
      "type": "user",
      "userId": "<user-id>",
      "username": "<username>"
    }

    {
      "id": "<post-id>",
      "type": "post",
      "postId": "<post-id>",
      "userId": "<post-author-id>",
      "userUsername": "<post-author-username>",
      "title": "<post-title>",
      "content": "<post-content>",
      "commentCount": <count-of-comments>,
      "likeCount": <count-of-likes>,
      "creationDate": "<post-creation-date>"
    }

Şunlara dikkat edin:

- kullanıcıları gönderilerden `type` ayırmak için kullanıcı öğesinde bir alan tanıttık,
- biz de `userId` `id` alan ile gereksiz kullanıcı öğesi, bir alan ekledik ama `users` kapsayıcı şimdi tarafından `userId` bölümlenmiş olarak `id` gereklidir (ve daha önce olduğu gibi değil)

Bu denormalizasyon elde etmek için, bir kez daha değişim beslemesi kullanın. Bu kez, kapsayıcıya yeni veya `posts` güncelleştirilmiş bir gönderi göndermek `users` için kapsayıcının değişiklik akışına tepki veririz. Gönderileri listeleme işlemlerinin tam içeriğini iade etmesi gerekmediği için, bu sırada gönderileri son işlemde son kullanabiliriz.

![Gönderileri kullanıcı kapsayıcısına normalleştirme](./media/how-to-model-partition-example/denormalization-2.png)

Artık sorgumuzu kapsayıcının `users` bölme anahtarına filtre uygulayarak yönlendirebiliriz.

![Bir kullanıcı için tüm gönderileri alma](./media/how-to-model-partition-example/V3-Q3.png)

| **Gecikme süresi** | **RU şarj** | **Performans** |
| --- | --- | --- |
| 4 ms | 6.46 RU | ✅ |

### <a name="q6-list-the-x-most-recent-posts-created-in-short-form-feed"></a>[Q6] Kısa formda oluşturulan x en son gönderileri listeleyin (besleme)

Burada da benzer bir durumla uğraşmak zorundayız: V2'de tanıtılan normalleştirmenin gereksiz bıraktığı ek sorguları sevedikten sonra bile, kalan sorgu kapsayıcının bölüm anahtarına filtre uygulamaz:

![En son gönderileri alma](./media/how-to-model-partition-example/V2-Q6.png)

Aynı yaklaşımı izleyerek, bu isteğin performansını ve ölçeklenebilirliğini en üst düzeye çıkarmak için yalnızca bir bölüme çarpması nı gerektirir. Bu akla yatkındır, çünkü yalnızca sınırlı sayıda öğe döndürmemiz gerekir; amacıyla bizim bloglama platformu ana sayfa doldurmak için, biz sadece tüm veri seti ile paginate gerek kalmadan, 100 en son mesaj almak gerekir.

Bu son isteği optimize etmek için, tasarımımıza tamamen bu isteği sunmaya adanmış üçüncü bir konteyner itanıştırıyoruz. Biz bu yeni `feed` konteyner bizim mesaj denormalize:

    {
      "id": "<post-id>",
      "type": "post",
      "postId": "<post-id>",
      "userId": "<post-author-id>",
      "userUsername": "<post-author-username>",
      "title": "<post-title>",
      "content": "<post-content>",
      "commentCount": <count-of-comments>,
      "likeCount": <count-of-likes>,
      "creationDate": "<post-creation-date>"
    }

Bu kapsayıcı tarafından `type`bölümlenmiş , `post` her zaman bizim öğeleri olacak. Bunu yapmak, bu kapsayıcıdaki tüm öğelerin aynı bölümde kalmasını sağlar.

Normalleşmeyi sağlamak için, daha önce bu yeni konteyner için mesaj göndermek için tanıttı değişim besleme boru hattı kanca var. Akılda tutulması gereken önemli bir şey biz sadece 100 en son mesaj saklamak emin olmak gerekir; aksi takdirde, kapsayıcının içeriği bir bölümün maksimum boyutunun ötesine geçebilir. Bu, kapsayıcıya her belge ekleninde [tetikleyici sonrası](stored-procedures-triggers-udfs.md#triggers) çağırılarak yapılır:

![Gönderileri besleme kabına normalleştirme](./media/how-to-model-partition-example/denormalization-3.png)

İşte koleksiyonu niçin truncates post-tetik gövdesi:

```javascript
function truncateFeed() {
  const maxDocs = 100;
  var context = getContext();
  var collection = context.getCollection();

  collection.queryDocuments(
    collection.getSelfLink(),
    "SELECT VALUE COUNT(1) FROM f",
    function (err, results) {
      if (err) throw err;

      processCountResults(results);
    });

  function processCountResults(results) {
    // + 1 because the query didn't count the newly inserted doc
    if ((results[0] + 1) > maxDocs) {
      var docsToRemove = results[0] + 1 - maxDocs;
      collection.queryDocuments(
        collection.getSelfLink(),
        `SELECT TOP ${docsToRemove} * FROM f ORDER BY f.creationDate`,
        function (err, results) {
          if (err) throw err;

          processDocsToRemove(results, 0);
        });
    }
  }

  function processDocsToRemove(results, index) {
    var doc = results[index];
    if (doc) {
      collection.deleteDocument(
        doc._self,
        function (err) {
          if (err) throw err;

          processDocsToRemove(results, index + 1);
        });
    }
  }
}
```

Son adım, sorgumuzu yeni `feed` kapsayıcımıza yönlendirmektir:

![En son gönderileri alma](./media/how-to-model-partition-example/V3-Q6.png)

| **Gecikme süresi** | **RU şarj** | **Performans** |
| --- | --- | --- |
| 9 ms | 16.97 RU | ✅ |

## <a name="conclusion"></a>Sonuç

Tasarımımızın farklı sürümleri nde sunduğumuz genel performans ve ölçeklenebilirlik iyileştirmelerine bir göz atalım.

| | V1 | V2 | V3 |
| --- | --- | --- | --- |
| **[C1]** | 7 ms / 5.71 RU | 7 ms / 5.71 RU | 7 ms / 5.71 RU |
| **[Q1]** | 2 ms / 1 RU | 2 ms / 1 RU | 2 ms / 1 RU |
| **[C2]** | 9 ms / 8.76 RU | 9 ms / 8.76 RU | 9 ms / 8.76 RU |
| **[Q2]** | 9 ms / 19,54 RU | 2 ms / 1 RU | 2 ms / 1 RU |
| **[Q3]** | 130 ms / 619,41 RU | 28 ms / 201.54 RU | 4 ms / 6.46 RU |
| **[C3]** | 7 ms / 8.57 RU | 7 ms / 15.27 RU | 7 ms / 15.27 RU |
| **[Q4]** | 23 ms / 27.72 RU | 4 ms / 7.72 RU | 4 ms / 7.72 RU |
| **[C4]** | 6 ms / 7.05 RU | 7 ms / 14.67 RU | 7 ms / 14.67 RU |
| **[Q5]** | 59 ms / 58,92 RU | 4 ms / 8.92 RU | 4 ms / 8.92 RU |
| **[Q6]** | 306 ms / 2063.54 RU | 83 ms / 532,33 RU | 9 ms / 16.97 RU |

### <a name="we-have-optimized-a-read-heavy-scenario"></a>Okuma ağırlıklı bir senaryoyu optimize ettik

Yazma istekleri (komutları) pahasına okuma isteklerinin (sorguların) performansını artırmaya yönelik çabalarımızı yoğunlaştırdığımızı fark etmişsinizdir. Çoğu durumda, yazma işlemleri artık değişim akışları aracılığıyla sonraki denormalleştirmetetik, bu da onları hesaplama açısından daha pahalı ve daha uzun hale getirir.

Bu aslında bir bloglama platformu (çoğu sosyal uygulamalar gibi) okuma-ağır, hangi hizmet etmek zorunda okuma isteklerinin miktarı genellikle büyüklük siparişleri yazma isteklerinin miktarından daha yüksek olduğu anlamına gelir haklı. Bu nedenle, okuma isteklerinin daha ucuz ve daha iyi performans lı olmasını sağlamak için yazma isteklerini yürütmek için daha pahalı hale getirmek mantıklıdır.

Yaptığımız en aşırı optimizasyona bakacak olursak, **[Q6]** 2000+ RUs'tan sadece 17 RUs'a çıktı; biz öğe başına yaklaşık 10 RUs bir maliyetle mesajları denormalizing tarafından elde ettik. Biz oluşturma veya mesaj güncellemeleri daha çok daha fazla besleme istekleri hizmet edeceğini gibi, bu denormalization maliyeti genel tasarruf dikkate alınarak ihmal edilebilir.

### <a name="denormalization-can-be-applied-incrementally"></a>Denormalizasyon kademeli olarak uygulanabilir

Bu makalede incelediğimiz ölçeklenebilirlik geliştirmeleri, veri kümesi ndeki verilerin normalden arındırılması nı ve çoğaltılmasını içerir. Bu optimizasyonların ilk gün yerine konması gerekmemelidir. Bölüm tuşlarına filtre uygulayan sorgular ölçekte daha iyi performans gösterir, ancak bölümler arası sorgular nadiren veya sınırlı bir veri kümesine karşı çağrıldıklarında tamamen kabul edilebilir. Sadece bir prototip oluşturuyorsanız veya küçük ve kontrollü bir kullanıcı tabanına sahip bir ürünü piyasaya sürüyorsanız, bu geliştirmeleri daha sonraiçin yedekleyebilirsiniz; O zaman önemli olan, modelinizin performansını [izlemektir,](use-metrics.md) böylece onları getirme nin zamanının gelip olmadığına karar verebilirsiniz.

Güncelleştirmeleri diğer kapsayıcılara dağıtmak için kullandığımız değişiklik akışı, tüm bu güncelleştirmeleri kalıcı olarak depolar. Bu, sisteminizde zaten çok fazla veri olsa bile, kapsayıcı ve bootstrap denormalize görünümlerin tek seferlik bir yakalama işlemi olarak oluşturulmasından bu yana tüm güncelleştirmeleri talep etmeyi mümkün kılar.

## <a name="next-steps"></a>Sonraki adımlar

Pratik veri modelleme ve bölümleme bu girişten sonra, biz ele almış kavramları gözden geçirmek için aşağıdaki makaleleri kontrol etmek isteyebilirsiniz:

- [Veritabanları, kapsayıcılar ve öğelerle çalışma](databases-containers-items.md)
- [Azure Cosmos DB'de bölümleme](partitioning-overview.md)
- [Azure Cosmos DB'deki değişiklik akışı](change-feed.md)
