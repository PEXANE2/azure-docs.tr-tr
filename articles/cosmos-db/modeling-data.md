---
title: Azure Cosmos DB'de verileri modelleme
titleSuffix: Azure Cosmos DB
description: NoSQL veritabanlarındaki veri modelleme, ilişkisel veritabanındaki verileri modelleme ve belge veritabanı arasındaki farklar hakkında bilgi edinin.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 07/23/2019
ms.openlocfilehash: 523049ea3286445117f41147f3dd12a2c911d1ae
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72755008"
---
# <a name="data-modeling-in-azure-cosmos-db"></a>Azure Cosmos DB'de veri modelleme

Azure Cosmos DB gibi şema içermeyen veritabanları, yapılandırılmamış ve yarı yapılandırılmış verileri depolamayı ve sorgulamayı süper kolay hale getirse de, performans ve ölçeklenebilirlik ve en düşük hizmet açısından hizmetten en iyi şekilde yararlanmak için veri modelinizi düşünmek için biraz zaman harcamanız gerekir Maliyet.

Veriler nasıl depolanacak? Uygulamanız verileri nasıl alıp sorgulayacak? Başvurunuz okuma-ağır mı, yoksa yazma-ağır mı?

Bu makaleyi okuduktan sonra, aşağıdaki soruları cevaplamak mümkün olacak:

* Veri modelleme nedir ve neden önemsemeliyim?
* Azure Cosmos DB'deki verileri modellemenin ilişkisel bir veritabanından farkı nedir?
* İlişkisel olmayan bir veritabanında veri ilişkilerini nasıl ifade edebilirim?
* Verileri ne zaman katıştırıyorum ve verilere ne zaman bağlanacağım?

## <a name="embedding-data"></a>Veri gömme

Azure Cosmos DB'de verileri modellemeye başladığınızda, varlıklarınızı JSON belgeleri olarak temsil edilen **bağımsız öğeler** olarak ele almaya çalışın.

Karşılaştırma için, önce ilişkisel bir veritabanındaki verileri nasıl modelleyebileceğimizi görelim. Aşağıdaki örnek, bir kişinin ilişkisel bir veritabanında nasıl depolanabileceğini gösterir.

![İlişkisel veritabanı modeli](./media/sql-api-modeling-data/relational-data-model.png)

İlişkisel veritabanları ile çalışırken, strateji tüm verilerinizi normalleştirmektir. Verilerinizi normalleştirmek genellikle bir kişi gibi bir varlığı alıp ayrı bileşenlere bölmeyi içerir. Yukarıdaki örnekte, bir kişinin birden çok ilgili kişi ayrıntı kaydının yanı sıra birden çok adres kaydı olabilir. İletişim bilgileri, bir tür gibi ortak alanları daha da ayıklayarak daha da bölünebilir. Aynı adres için de geçerlidir, her kayıt tür *Ev* veya *İş*olabilir.

Verileri normalleştirmede yol gösterici öncül, her kayıtta **gereksiz verileri depolamaktan kaçınmak** ve daha çok verilere başvurmaktır. Bu örnekte, bir kişiyi tüm iletişim bilgileri ve adresleriyle birlikte okumak için, çalışma zamanında verilerinizi etkili bir şekilde oluşturmak (veya normalden arındırmak) için JOINS'i kullanmanız gerekir.

    SELECT p.FirstName, p.LastName, a.City, cd.Detail
    FROM Person p
    JOIN ContactDetail cd ON cd.PersonId = p.Id
    JOIN ContactDetailType cdt ON cdt.Id = cd.TypeId
    JOIN Address a ON a.PersonId = p.Id

Tek bir kişinin iletişim bilgileri ve adresleriyle güncelleştirilmesi, birçok tabloda yazma işlemleri gerektirir.

Şimdi Azure Cosmos DB'de bağımsız bir varlıkla aynı verileri nasıl modelleyebileceğimize bir göz atalım.

    {
        "id": "1",
        "firstName": "Thomas",
        "lastName": "Andersen",
        "addresses": [
            {
                "line1": "100 Some Street",
                "line2": "Unit 1",
                "city": "Seattle",
                "state": "WA",
                "zip": 98012
            }
        ],
        "contactDetails": [
            {"email": "thomas@andersen.com"},
            {"phone": "+1 555 555-5555", "extension": 5555}
        ]
    }

Yukarıdaki yaklaşımı kullanarak, iletişim bilgileri ve adresleri gibi bu kişiyle ilgili tüm bilgileri tek bir *JSON* belgesine **yerleştirerek** kişi kaydını **normalden ayırdık.**
Buna ek olarak, sabit bir şemaya bağlı olmadığımız için, tamamen farklı şekillerin iletişim bilgilerine sahip olmak gibi şeyleri yapma esnekliğine sahibiz.

Veritabanından tam bir kişi kaydı alma artık tek bir kapsayıcı ya karşı ve tek bir öğe için tek bir **okuma işlemidir.** Kişi bilgileri ve adresleriyle bir kişi kaydının güncelleştirilmesi, aynı zamanda tek bir öğeye karşı tek bir **yazma işlemidir.**

Verilerin normalinden arındırılarak, uygulamanızın ortak işlemleri tamamlamak için daha az sorgu ve güncelleştirme sorunu yayınlaması gerekebilir.

### <a name="when-to-embed"></a>Ne zaman gömüler

Genel olarak, gömülü veri modellerini şu durumlarda kullanın:

* Varlıklar arasında **içeren** ilişkiler vardır.
* Varlıklar arasında **bire bir** ilişki vardır.
* **Seyrek olarak değişen**gömülü veriler vardır.
* **Bağlı olmadan**büyümeyecek gömülü veriler vardır.
* Sık sık birlikte **sorgulandık**gömülü veriler vardır.

> [!NOTE]
> Genellikle normalden arındırılmış veri modelleri daha iyi **okuma** performansı sağlar.

### <a name="when-not-to-embed"></a>Gömmemek için ne zaman

Azure Cosmos DB'deki başparmak kuralı her şeyin normalinden arındırmak ve tüm verileri tek bir öğeye yerleştirmek olsa da, bu kaçınılması gereken bazı durumlara yol açabilir.

Şu JSON parçacığı al.

    {
        "id": "1",
        "name": "What's new in the coolest Cloud",
        "summary": "A blog post by someone real famous",
        "comments": [
            {"id": 1, "author": "anon", "comment": "something useful, I'm sure"},
            {"id": 2, "author": "bob", "comment": "wisdom from the interwebs"},
            …
            {"id": 100001, "author": "jane", "comment": "and on we go ..."},
            …
            {"id": 1000000001, "author": "angry", "comment": "blah angry blah angry"},
            …
            {"id": ∞ + 1, "author": "bored", "comment": "oh man, will this ever end?"},
        ]
    }

Bu, tipik bir blog veya CMS, sistem modelleme olsaydı gömülü yorumlar ile bir yazı varlık nasıl görüneceğini olabilir. Bu örnekteki sorun, yorum dizisinin **sınırsız**olmasıdır, yani herhangi bir gönderinin sahip olabileceği yorum sayısı için (pratik) bir sınır yoktur. Öğenin boyutu sonsuz büyük büyüyebilir gibi bu bir sorun haline gelebilir.

Öğenin boyutu büyüdükçe, verileri tel üzerinden iletme ve öğeyi okuma ve güncelleme yeteneği ölçekolarak etkilenecektir.

Bu durumda, aşağıdaki veri modelini dikkate almak daha iyi olacaktır.

    Post item:
    {
        "id": "1",
        "name": "What's new in the coolest Cloud",
        "summary": "A blog post by someone real famous",
        "recentComments": [
            {"id": 1, "author": "anon", "comment": "something useful, I'm sure"},
            {"id": 2, "author": "bob", "comment": "wisdom from the interwebs"},
            {"id": 3, "author": "jane", "comment": "....."}
        ]
    }

    Comment items:
    {
        "postId": "1"
        "comments": [
            {"id": 4, "author": "anon", "comment": "more goodness"},
            {"id": 5, "author": "bob", "comment": "tails from the field"},
            ...
            {"id": 99, "author": "angry", "comment": "blah angry blah angry"}
        ]
    },
    {
        "postId": "1"
        "comments": [
            {"id": 100, "author": "anon", "comment": "yet more"},
            ...
            {"id": 199, "author": "bored", "comment": "will this ever end?"}
        ]
    }

Bu model, sabit bir öznitelik kümesine sahip bir dizi olan posta kapsayıcısında gömülü en son üç yoruma sahiptir. Diğer yorumlar 100 yorumdan oluşan gruplar halinde gruplandırılır ve ayrı öğeler olarak depolanır. Bizim hayali uygulama kullanıcı bir seferde 100 yorum yüklemek için izin verir, çünkü toplu iş boyutu 100 olarak seçildi.  

Verileri katıştırma iyi bir fikir olmadığı başka bir durum da, katıştırılmış verilerin öğeler arasında sık sık kullanıldığı ve sık sık değişeceğidir.

Şu JSON parçacığı al.

    {
        "id": "1",
        "firstName": "Thomas",
        "lastName": "Andersen",
        "holdings": [
            {
                "numberHeld": 100,
                "stock": { "symbol": "zaza", "open": 1, "high": 2, "low": 0.5 }
            },
            {
                "numberHeld": 50,
                "stock": { "symbol": "xcxc", "open": 89, "high": 93.24, "low": 88.87 }
            }
        ]
    }

Bu bir kişinin hisse senedi portföyünü temsil edebilir. Hisse senedi bilgilerini her portföy belgesine yerleştirmeyi seçtik. İlgili verilerin hisse senedi alım satım uygulaması gibi sık sık değiştiği bir ortamda, sık sık değişen verileri yerleştirmek, her hisse senedi işlem gördüğünde her portföy belgesini sürekli olarak güncellediğiniz anlamına gelir.

Hisse senedi *zazaca* bir günde yüzlerce kez işlem görebilir ve binlerce kullanıcı portföyünde *zaza* olabilir. Yukarıdaki gibi bir veri modeli ile biz portföy belgeleri binlerce her gün birçok kez iyi ölçeklendirmez bir sisteme giden güncellemek zorunda kalacaktı.

## <a name="referencing-data"></a>Verilere başvurma

Verileri katıştırma birçok durumda iyi çalışır, ancak verilerinnormalden arındırma değerinden daha fazla soruna neden olacak senaryolar vardır. Peki şimdi ne yapacağız?

İlişkisel veritabanları, varlıklar arasında ilişki oluşturabileceğiniz tek yer değildir. Belge veritabanında, bir belgede diğer belgelerdeki verilerle ilgili bilgilere sahip olabilirsiniz. Azure Cosmos DB'deki veya başka bir belge veritabanında ilişkisel bir veritabanına daha uygun sistemler oluşturmanızı önermiyoruz, ancak basit ilişkiler iyidir ve yararlı olabilir.

Aşağıdaki JSON'da daha önceki bir hisse senedi portföyü örneğini kullanmayı seçtik, ancak bu sefer onu yerleştirmek yerine portföydeki hisse senedi maddesine atıfta bulunuyoruz. Bu şekilde, stok maddesi gün boyunca sık sık değiştiğinde, güncellenmesi gereken tek belge tek stok belgesidir.

    Person document:
    {
        "id": "1",
        "firstName": "Thomas",
        "lastName": "Andersen",
        "holdings": [
            { "numberHeld":  100, "stockId": 1},
            { "numberHeld":  50, "stockId": 2}
        ]
    }

    Stock documents:
    {
        "id": "1",
        "symbol": "zaza",
        "open": 1,
        "high": 2,
        "low": 0.5,
        "vol": 11970000,
        "mkt-cap": 42000000,
        "pe": 5.89
    },
    {
        "id": "2",
        "symbol": "xcxc",
        "open": 89,
        "high": 93.24,
        "low": 88.87,
        "vol": 2970200,
        "mkt-cap": 1005000,
        "pe": 75.82
    }

Ancak bu yaklaşımın hemen bir dezavantajı, başvurunuzun bir kişinin portföyünü görüntülerken tutulan her hisse senedi hakkında bilgi göstermesi gerekip gerekmemesidir; Bu durumda, her stok belgesi için bilgileri yüklemek için veritabanına birden çok yolculuk yapmanız gerekir. Burada, gün boyunca sık sık gerçekleşen, ancak bu sistemin performansı üzerinde potansiyel olarak daha az etkiye sahip okuma işlemleri üzerinde tehlikeye giren yazma işlemlerinin verimliliğini artırmaya karar verdik.

> [!NOTE]
> Normalleştirilmiş veri modelleri sunucuya **daha fazla gidiş dönüş gerektirebilir.**

### <a name="what-about-foreign-keys"></a>Peki ya yabancı anahtarlar?

Şu anda bir kısıtlama, yabancı anahtar veya başka bir kavram olmadığından, belgelerde bulunan tüm belgeler arası ilişkiler etkili bir şekilde "zayıf bağlantılardır" ve veritabanının kendisi tarafından doğrulanmaz. Bir belgenin atıfta bulunduğu verilerin gerçekten var olduğundan emin olmak istiyorsanız, bunu uygulamanızda veya Azure Cosmos DB'de sunucu tarafındaki tetikleyiciler veya depolanmış yordamlar aracılığıyla yapmanız gerekir.

### <a name="when-to-reference"></a>Ne zaman başvurulması gerektiğini

Genel olarak, normalleştirilmiş veri modellerini şu durumlarda kullanın:

* **Bir-çok** ilişkileri temsil ediyor.
* **Çok-çok** ilişkileri temsil ediyor.
* İlgili veriler **sık sık değişir.**
* Başvurulan veriler **sınırsız**olabilir.

> [!NOTE]
> Genellikle normalleştirme daha iyi **yazma** performansı sağlar.

### <a name="where-do-i-put-the-relationship"></a>İlişkiyi nereye koyacağım?

İlişkinin büyümesi, başvurunun hangi belgede depolanacağını belirlemeye yardımcı olur.

Aşağıdaki JSON'a bakacak olursak yayıncıları ve kitapları modeller.

    Publisher document:
    {
        "id": "mspress",
        "name": "Microsoft Press",
        "books": [ 1, 2, 3, ..., 100, ..., 1000]
    }

    Book documents:
    {"id": "1", "name": "Azure Cosmos DB 101" }
    {"id": "2", "name": "Azure Cosmos DB for RDBMS Users" }
    {"id": "3", "name": "Taking over the world one JSON doc at a time" }
    ...
    {"id": "100", "name": "Learn about Azure Cosmos DB" }
    ...
    {"id": "1000", "name": "Deep Dive into Azure Cosmos DB" }

Yayımcı başına düşen kitap sayısı sınırlı büyümeyle birlikte küçükse, kitap referansını yayımcı belgesinin içinde depolamak yararlı olabilir. Ancak, yayımcı başına düşen kitap sayısı sınırsızsa, bu veri modeli yukarıdaki örnek yayımcı belgesinde olduğu gibi, çözülebilen, büyüyen dizilere yol açar.

Bir şeyleri biraz etrafında değiştirmek, hala aynı verileri temsil eden ancak şimdi bu büyük mutable koleksiyonlardan kaçınan bir modelle sonuçlanır.

    Publisher document:
    {
        "id": "mspress",
        "name": "Microsoft Press"
    }

    Book documents:
    {"id": "1","name": "Azure Cosmos DB 101", "pub-id": "mspress"}
    {"id": "2","name": "Azure Cosmos DB for RDBMS Users", "pub-id": "mspress"}
    {"id": "3","name": "Taking over the world one JSON doc at a time"}
    ...
    {"id": "100","name": "Learn about Azure Cosmos DB", "pub-id": "mspress"}
    ...
    {"id": "1000","name": "Deep Dive into Azure Cosmos DB", "pub-id": "mspress"}

Yukarıdaki örnekte, yayımcı belgesinde sınırsız koleksiyonu düşürdük. Bunun yerine sadece her kitap belgesinde yayıncıya bir referans var.

### <a name="how-do-i-model-manymany-relationships"></a>Çok: birçok ilişkiyi nasıl modellendirebilirim?

İlişkisel bir veritabanında *çok: birçok* ilişki genellikle sadece birlikte diğer tablolardan kayıtları birleştirmek birleştirme tabloları ile modellenir.

![Birleştirme tabloları](./media/sql-api-modeling-data/join-table.png)

Belgeleri kullanarak aynı şeyi çoğaltmak ve aşağıdakilere benzer bir veri modeli oluşturmak isteyebilirsiniz.

    Author documents:
    {"id": "a1", "name": "Thomas Andersen" }
    {"id": "a2", "name": "William Wakefield" }

    Book documents:
    {"id": "b1", "name": "Azure Cosmos DB 101" }
    {"id": "b2", "name": "Azure Cosmos DB for RDBMS Users" }
    {"id": "b3", "name": "Taking over the world one JSON doc at a time" }
    {"id": "b4", "name": "Learn about Azure Cosmos DB" }
    {"id": "b5", "name": "Deep Dive into Azure Cosmos DB" }

    Joining documents:
    {"authorId": "a1", "bookId": "b1" }
    {"authorId": "a2", "bookId": "b1" }
    {"authorId": "a1", "bookId": "b2" }
    {"authorId": "a1", "bookId": "b3" }

Bu işe yarar. Ancak, bir yazarı kitaplarıyla yüklemek veya bir kitabı yazarıyla birlikte yüklemek, her zaman veritabanına karşı en az iki ek sorgu gerektirir. Birleştirme belgesine bir sorgu ve sonra birleşen gerçek belgeyi almak için başka bir sorgu.

Eğer tüm bu birleştirme tablosu iki parça veriyi birbirine yapıştırıyorsa, o zaman neden tamamen düşürmedin?
Aşağıdakileri göz önünde bulundurun.

    Author documents:
    {"id": "a1", "name": "Thomas Andersen", "books": ["b1, "b2", "b3"]}
    {"id": "a2", "name": "William Wakefield", "books": ["b1", "b4"]}

    Book documents:
    {"id": "b1", "name": "Azure Cosmos DB 101", "authors": ["a1", "a2"]}
    {"id": "b2", "name": "Azure Cosmos DB for RDBMS Users", "authors": ["a1"]}
    {"id": "b3", "name": "Learn about Azure Cosmos DB", "authors": ["a1"]}
    {"id": "b4", "name": "Deep Dive into Azure Cosmos DB", "authors": ["a2"]}

Şimdi, bir yazar olsaydı, ben hemen yazdıkları kitapları biliyorum, ve tersine ben bir kitap belgesi yüklü olsaydı ben yazarın (lar) kimliklerini bilmek istiyorum. Bu, ara sorguyu birleştirme tablosuna karşı, uygulamanızın yapmak zorunda olduğu sunucu tur larını azaltarak kaydeder.

## <a name="hybrid-data-models"></a>Karma veri modelleri

Şimdi katıştırma (veya denormalizing) ve referans (veya normalleştirme) veri baktım, her biri kendi artıları var ve her gördüğümüz gibi tavizler var.

Her zaman ya da olmak zorunda değildir, şeyler biraz karıştırmak için korkmayın.

Uygulamanızın özel kullanım kalıplarına ve iş yüklerine bağlı olarak, gömülü ve başvurulan verilerin karıştırılmasının mantıklı olduğu ve iyi bir performans düzeyini korurken daha az sunucu turu gezileriyle daha basit uygulama mantığına yol açabileceği durumlar olabilir.

Aşağıdaki JSON düşünün.

    Author documents:
    {
        "id": "a1",
        "firstName": "Thomas",
        "lastName": "Andersen",
        "countOfBooks": 3,
        "books": ["b1", "b2", "b3"],
        "images": [
            {"thumbnail": "https://....png"}
            {"profile": "https://....png"}
            {"large": "https://....png"}
        ]
    },
    {
        "id": "a2",
        "firstName": "William",
        "lastName": "Wakefield",
        "countOfBooks": 1,
        "books": ["b1"],
        "images": [
            {"thumbnail": "https://....png"}
        ]
    }

    Book documents:
    {
        "id": "b1",
        "name": "Azure Cosmos DB 101",
        "authors": [
            {"id": "a1", "name": "Thomas Andersen", "thumbnailUrl": "https://....png"},
            {"id": "a2", "name": "William Wakefield", "thumbnailUrl": "https://....png"}
        ]
    },
    {
        "id": "b2",
        "name": "Azure Cosmos DB for RDBMS Users",
        "authors": [
            {"id": "a1", "name": "Thomas Andersen", "thumbnailUrl": "https://....png"},
        ]
    }

Burada (çoğunlukla) diğer varlıklardan gelen verilerin üst düzey belgeye katıştırıldığı, ancak diğer verilere başvurulduğu gömülü modeli izledik.

Kitap belgesine bakarsanız, yazar dizilimi ne zaman görebilirsiniz. Biz bir `id` yazar belge, normalleştirilmiş bir modelde standart uygulama başvurmak için kullandığınız alan dır, `name` `thumbnailUrl`ama sonra biz de var ve . "Bağlantı"yı `id` kullanarak ilgili yazar belgesinden gerekli olan ek bilgileri almak için uygulamadan ayrılabilirdik, ancak uygulamamız yazarın adını ve görüntülenen her kitapla birlikte bir küçük resim görüntülediği için, yazardan gelen **bazı** verileri normalleştirerek bir listedeki sunucu başına bir gidiş dönüş kaydedebiliriz.

Tabii, eğer yazarın adı değişti yse ya da fotoğraflarını güncellemek istiyorlarsa, yayınladıkları her kitabı güncellememiz gerekir, ancak uygulamamız için, yazarların adlarını sık sık değiştirmedikleri varsayımına dayanarak, bu kabul edilebilir bir tasarım kararıdır.  

Örnekte, okundu işleminde pahalı işleme kaydetmek için **önceden hesaplanmış toplam** değerleri vardır. Örnekte, yazar belgesine katıştırılabilen verilerin bazıları çalışma zamanında hesaplanan verilerdir. Her yeni kitap yayımlanında, bir kitap belgesi oluşturulur **ve** countOfBooks alanı, belirli bir yazar için var olan kitap belgelerinin sayısına göre hesaplanan bir değere ayarlanır. Bu optimizasyon biz okuma optimize etmek için yazma hesaplamaları yapmak gelemez ağır sistemlerde okumak iyi olurdu.

Azure Cosmos DB **çoklu belge işlemlerini**desteklediğinden, önceden hesaplanmış alanlara sahip bir modele sahip olma özelliği mümkün kılınır. Birçok NoSQL deposu belgeler arasında işlem yapamaz ve bu nedenle bu sınırlama nedeniyle "her şeyi her zaman katıştır" gibi tasarım kararlarını savunur. Azure Cosmos DB ile, bir ACID işlemi içinde kitap ekleyen ve yazarları güncelleştiren sunucu tarafındaki tetikleyicileri veya depolanmış yordamları kullanabilirsiniz. Artık verilerinizin **have** tutarlı olduğundan emin olmak için her şeyi tek bir belgeye gömmeniz gerekmez.

## <a name="distinguishing-between-different-document-types"></a>Farklı belge türleri arasında ayrım

Bazı senaryolarda, farklı belge türlerini aynı koleksiyonda karıştırmak isteyebilirsiniz; bu genellikle aynı [bölümde](partitioning-overview.md)oturup birden çok, ilgili belgeler istediğinizde durumdur. Örneğin, hem kitap hem de kitap incelemelerini aynı koleksiyona koyabilir ve 'ye göre bölümleyebilirsin. `bookId` Böyle bir durumda, genellikle bunları ayırt etmek için kendi türünü tanımlayan bir alan ile belgelerinize eklemek istiyorum.

    Book documents:
    {
        "id": "b1",
        "name": "Azure Cosmos DB 101",
        "bookId": "b1",
        "type": "book"
    }

    Review documents:
    {
        "id": "r1",
        "content": "This book is awesome",
        "bookId": "b1",
        "type": "review"
    },
    {
        "id": "r2",
        "content": "Best book ever!",
        "bookId": "b1",
        "type": "review"
    }

## <a name="next-steps"></a>Sonraki adımlar

Bu makaleden en büyük takeaways bir şema-free dünyada veri modelleme her zamanki gibi önemli olduğunu anlamak için vardır.

Bir ekranda bir veri parçasını temsil etmenin tek bir yolu olmadığı gibi, verilerinizi modellemenin tek bir yolu da yoktur. Uygulamanızı ve verileri nasıl üreteceğini, tüketeceğini ve işleyeceklerini anlamanız gerekir. Daha sonra, burada sunulan bazı yönergeleri uygulayarak, uygulamanızın acil gereksinimlerini karşılayan bir model oluşturmaya ayarlayabilirsiniz. Uygulamalarınızın değişmesi gerektiğinde, bu değişimi benimsemek ve veri modelinizi kolayca geliştirmek için şema içermeyen bir veritabanının esnekliğinden yararlanabilirsiniz.

Azure Cosmos DB hakkında daha fazla bilgi edinmek için hizmetin [dokümantasyon](https://azure.microsoft.com/documentation/services/cosmos-db/) sayfasına bakın.

Verilerinizi birden çok bölüm arasında nasıl parçalayacağınızı anlamak için [Azure Cosmos DB'deki Verileri Bölümleme bölümüne](sql-api-partition-data.md)bakın.

Azure Cosmos DB'deki verileri gerçek dünya örneğini kullanarak nasıl modelleyip bölümlendireceklerini öğrenmek için, [Gerçek Dünya Örneği olan Veri Modelleme ve Bölümleme](how-to-model-partition-example.md)bölümüne bakın.
