---
title: 'Azure Cosmos DB tasarım deseni: Sosyal medya uygulamaları'
description: Azure Cosmos DB ve diğer Azure hizmetlerinin depolama esnekliğinden yararlanarak Sosyal Ağlar için tasarım deseni hakkında bilgi edinin.
author: ealsur
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/28/2019
ms.author: maquaran
ms.openlocfilehash: 8428e417f5f86edca77edae6ca4b7ef84e5ff425
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73827290"
---
# <a name="going-social-with-azure-cosmos-db"></a>Azure Cosmos DB ile sosyalleşme

Birbirine bağlı bir toplumda yaşamak, hayatın bir noktasında **bir sosyal ağın**parçası olduğunuz anlamına gelir. Sosyal ağları arkadaşlarınızla, iş arkadaşlarınızla, ailenizle iletişimde kalmak veya bazen ortak ilgi alanlarına sahip kişilerle tutkunuzu paylaşmak için kullanırsınız.

Mühendisler veya geliştiriciler olarak, bu ağların verilerinizi nasıl depolayıp birbirine bağladığını merak etmiş olabilirsiniz. Ya da belirli bir niş pazar için yeni bir sosyal ağ oluşturmak veya mimar görevli olabilir. İşte o zaman önemli bir soru ortaya çıkar: Tüm bu veriler nasıl saklanır?

Kullanıcılarınızın resimler, videolar ve hatta müzik gibi ilgili medyayla makaleler gönderebileceği yeni ve parlak bir sosyal ağ oluşturduğunuzu varsayalım. Kullanıcılar gönderiler hakkında yorum yapabilir ve derecelendirmeler için puan verebilir. Kullanıcıların ana web sitesi açılış sayfasında göreceği ve etkileşimde bulunacak gönderiler akışı olacaktır. Bu yöntem ilk başta karmaşık gelmiyor, ama basitlik uğruna, orada duralım. (İlişkilerden etkilenen özel kullanıcı akışlarını araştırabilirsiniz, ancak bu makalenin amacının ötesine geçer.)

Peki, bu verileri nasıl ve nerede saklarsınız?

SQL veritabanlarında deneyim sahibi olabilir veya [verilerin ilişkisel modellemesi kavramına](https://en.wikipedia.org/wiki/Relational_model)sahip olabilirsiniz. Aşağıdaki gibi bir şey çizmeye başlayabilirsiniz:

![Göreceli ilişkisel modeli gösteren diyagram](./media/social-media-apps/social-media-apps-sql.png)

Mükemmel normalleştirilmiş ve güzel bir veri yapısı... Bu ölçeklendirmiyor.

Beni yanlış anlama, ben hayatım boyunca SQL veritabanları ile çalıştım. Harikalar, ama her desen, uygulama ve yazılım platformu gibi, her senaryo için mükemmel değildir.

Bu senaryoda SQL neden en iyi seçim değil? Tek bir yazının yapısına bakalım. Ben bir web sitesi veya uygulamada sonrası göstermek istedim, ben bir sorgu yapmak gerekir ... sadece tek bir yazı göstermek için sekiz tablolar (!) katılarak. Şimdi dinamik olarak yüklenen ve ekranda görünen bir gönderi akışı hayal edin ve nereye gittiğimi görebilirsiniz.

İçeriğinize hizmet etmek için binlerce sorguyla binlerce sorguyı çözmek için yeterli güce sahip muazzam bir SQL örneği kullanabilirsiniz. Ama daha basit bir çözüm varken neden yapasın ki?

## <a name="the-nosql-road"></a>NoSQL yolu

Bu makale, azure'un NoSQL veritabanı [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) ile sosyal platformunuzun verilerini modellemeniz için sizi yönlendirir. Ayrıca [Gremlin API](../cosmos-db/graph-introduction.md)gibi diğer Azure Cosmos DB özelliklerini nasıl kullanacağınızı da söyler. Bir [NoSQL](https://en.wikipedia.org/wiki/NoSQL) yaklaşımı kullanarak, JSON formatında veri depolama ve [denormalization](https://en.wikipedia.org/wiki/Denormalization)uygulayarak, daha önce karmaşık sonrası tek bir [Belge](https://en.wikipedia.org/wiki/Document-oriented_database)dönüştürülebilir:

    {
        "id":"ew12-res2-234e-544f",
        "title":"post title",
        "date":"2016-01-01",
        "body":"this is an awesome post stored on NoSQL",
        "createdBy":User,
        "images":["https://myfirstimage.png","https://mysecondimage.png"],
        "videos":[
            {"url":"https://myfirstvideo.mp4", "title":"The first video"},
            {"url":"https://mysecondvideo.mp4", "title":"The second video"}
        ],
        "audios":[
            {"url":"https://myfirstaudio.mp3", "title":"The first audio"},
            {"url":"https://mysecondaudio.mp3", "title":"The second audio"}
        ]
    }

Ve tek bir sorgu ile elde edilebilir, ve hiçbir birleştirmeleri ile. Bu sorgu çok basit ve basittir ve bütçe açısından daha iyi bir sonuç elde etmek için daha az kaynak gerektirir.

Azure Cosmos DB, otomatik dizin oluşturma yla tüm özelliklerin dizinlenmiş olmasını sağlar. Otomatik dizin oluşturma bile [özelleştirilebilir.](index-policy.md) Şemasız yaklaşım, belgeleri farklı ve dinamik yapılara sahip depolamamızı sağlar. Belki yarın gönderilerin onlarla ilişkili kategorilerin veya hashtag'lerin bir listesi olmasını istersiniz? Cosmos DB, yeni Belgeleri ek özelliklerle, bizim için gerekli olan ekstra çalışma olmadan ele alacaktır.

Gönderideki yorumlar, bir üst öğeye sahip diğer gönderiler olarak kabul edilebilir. (Bu uygulama nesne eşleme basitleştirir.)

    {
        "id":"1234-asd3-54ts-199a",
        "title":"Awesome post!",
        "date":"2016-01-02",
        "createdBy":User2,
        "parent":"ew12-res2-234e-544f"
    }

    {
        "id":"asd2-fee4-23gc-jh67",
        "title":"Ditto!",
        "date":"2016-01-03",
        "createdBy":User3,
        "parent":"ew12-res2-234e-544f"
    }

Ve tüm sosyal etkileşimler sayaç olarak ayrı bir nesne üzerinde saklanabilir:

    {
        "id":"dfe3-thf5-232s-dse4",
        "post":"ew12-res2-234e-544f",
        "comments":2,
        "likes":10,
        "points":200
    }

Özet akışları oluşturma, belirli bir alaka düzeyi sırasına sahip posta i'lerinin listesini tutabilen belgeler oluşturma meselesidir:

    [
        {"relevance":9, "post":"ew12-res2-234e-544f"},
        {"relevance":8, "post":"fer7-mnb6-fgh9-2344"},
        {"relevance":7, "post":"w34r-qeg6-ref6-8565"}
    ]

Oluşturma tarihine göre sıralanmış gönderiler içeren "en son" bir akış sağlayabilirsiniz. Ya da son 24 saat içinde daha fazla beğeni ile bu mesajları ile bir "sıcak" akışı olabilir. Hatta takipçileri ve ilgi alanları gibi mantık dayalı her kullanıcı için özel bir akış uygulayabilirsiniz. Yine de mesajların bir listesi olacaktır. Bu listelerin nasıl oluşturulaöğretilen bir konudur, ancak okuma performansı engellenmez. Bu listelerden birini elde ettikten sonra, aynı anda gönderilerin sayfalarını almak için IN anahtar sözcük anahtar [sözcüklerini](sql-query-keywords.md#in) kullanarak Cosmos DB'ye tek bir sorgu gönderirsiniz.

Özet akışı [akışları, Azure Uygulama Hizmetleri'nin](https://azure.microsoft.com/services/app-service/) arka plan işlemleri kullanılarak oluşturulabilir: [Webişleri.](../app-service/webjobs-create.md) Bir gönderi oluşturulduktan sonra, azure web [işleri SDK](https://github.com/Azure/azure-webjobs-sdk/wiki)kullanılarak tetiklenen [Azure Depolama](https://azure.microsoft.com/services/storage/) [Kuyrukları](../storage/queues/storage-dotnet-how-to-use-queues.md) ve Web işleri kullanılarak arka plan işleme tetiklenebilir ve kendi özel mantığınızı temel alarak akışlar içinde gönderi yayılımı uygulayabilir.

Bir gönderi üzerindeki puanlar ve beğenmeler, sonunda tutarlı bir ortam oluşturmak için aynı teknik kullanılarak ertelenmiş bir şekilde işlenebilir.

Takipçiler daha aldatıcıdır. Cosmos DB'nin belge boyutu sınırı vardır ve büyük belgeleri okumak/yazmak uygulamanızın ölçeklenebilirliğini etkileyebilir. Bu nedenle takipçileri bu yapıya sahip bir belge olarak depolamayı düşünebilirsiniz:

    {
        "id":"234d-sd23-rrf2-552d",
        "followersOf": "dse4-qwe2-ert4-aad2",
        "followers":[
            "ewr5-232d-tyrg-iuo2",
            "qejh-2345-sdf1-ytg5",
            //...
            "uie0-4tyg-3456-rwjh"
        ]
    }

Bu yapı, birkaç bin takipçisi olan bir kullanıcı için çalışabilir. Bazı ünlü saflarına katılırsa, ancak, bu yaklaşım büyük bir belge boyutuna yol açacaktır ve sonunda belge boyutu kapağına çarpabilir.

Bu sorunu çözmek için karışık bir yaklaşım kullanabilirsiniz. Kullanıcı İstatistikleri belgesinin bir parçası olarak takipçi sayısını depolayabilirsiniz:

    {
        "id":"234d-sd23-rrf2-552d",
        "user": "dse4-qwe2-ert4-aad2",
        "followers":55230,
        "totalPosts":452,
        "totalPoints":11342
    }

"A-follows-B" ilişkilerini koruyan her kullanıcı ve [kenarlar](http://mathworld.wolfram.com/GraphEdge.html) için [tepe noktası](http://mathworld.wolfram.com/GraphVertex.html) oluşturmak için Azure Cosmos DB [Gremlin API'yi](../cosmos-db/graph-introduction.md) kullanarak takipçilerin gerçek grafiğini depolayabilirsiniz. Gremlin API ile, belirli bir kullanıcının takipçileri ni alabilir ve ortak kişileri önermek için daha karmaşık sorgular oluşturabilirsiniz. Grafiğe insanların beğendiklerine veya beğendiklerinden hoşlandığı İçerik Kategorileri'ni eklerseniz, akıllı içerik bulma, takip ettiğiniz içerikleri öneren veya çok ortak noktalarınız olabilecek kişileri bulan deneyimler örmeye başlayabilirsiniz.

Kullanıcı İstatistikleri belgesi, Kullanıcı Arabirimi'nde veya hızlı profil önizlemelerinde kart oluşturmak için kullanılabilir.

## <a name="the-ladder-pattern-and-data-duplication"></a>"Merdiven" deseni ve veri çoğaltma

Bir gönderiye başvuran JSON belgesinde fark etmiş olabileceğiniz gibi, bir kullanıcının birçok olayı vardır. Ve doğru tahmin olurdu, bu yinelemeler, bu normalleştirme göz önüne alındığında, bir kullanıcıyı tanımlayan bilgilerin birden fazla yerde bulunabileceği anlamına gelir.

Daha hızlı sorgular için izin vermek için, veri çoğaltma tabi. Bu yan etki ile ilgili sorun, bazı eylem, bir kullanıcının veri değişiklikleri, kullanıcının şimdiye kadar yaptığı tüm faaliyetleri bulmak ve hepsini güncellemek gerekir. Kulağa pek mantıklı gelmiyor, değil mi?

Her etkinlik için uygulamanızda gösterdiğiniz bir kullanıcının temel özniteliklerini tanımlayarak bunu çözesiniz. Uygulamanızda görsel olarak bir gönderi gösteriyorsanız ve yalnızca oluşturucunun adını ve resmini gösteriyorsanız, neden tüm kullanıcı verilerini "createdBy" özniteliğinde saklıyorsunuz? Her yorum için yalnızca kullanıcının resmini gösterirseniz, kullanıcının diğer bilgilerine gerçekten ihtiyacınız olmaz. "Merdiven deseni" dediğim bir şey burada devreye girer.

Kullanıcı bilgilerini örnek alalım:

    {
        "id":"dse4-qwe2-ert4-aad2",
        "name":"John",
        "surname":"Doe",
        "address":"742 Evergreen Terrace",
        "birthday":"1983-05-07",
        "email":"john@doe.com",
        "twitterHandle":"\@john",
        "username":"johndoe",
        "password":"some_encrypted_phrase",
        "totalPoints":100,
        "totalPosts":24
    }

Bu bilgilere bakarak, hangikritik bilginin kritik olup olmadığını hızlı bir şekilde algılayabilir, böylece bir "Merdiven" oluşturabilirsiniz:

![Merdiven deseni diyagramı](./media/social-media-apps/social-media-apps-ladder.png)

En küçük adım, kullanıcıyı tanımlayan ve veri çoğaltma için kullanılan en küçük bilgi parçası olan UserChunk olarak adlandırılır. Yinelenen veri boyutunu yalnızca "göstereceğiniz" bilgilere indirgeyerek, büyük güncelleştirme olasılığını azaltırsınız.

Orta adım kullanıcı olarak adlandırılır. En çok erişilen ve kritik olan Cosmos DB'deki performansa bağlı sorguların çoğunda kullanılacak tam veridir. UserChunk tarafından temsil edilen bilgileri içerir.

En büyüğü Genişletilmiş Kullanıcıdır. Oturum açma işlemi gibi hızlı bir şekilde okunması gerekmeyen veya nihai kullanımı olan kritik kullanıcı bilgilerini ve diğer verileri içerir. Bu veriler Cosmos DB dışında, Azure SQL Veritabanı'nda veya Azure Depolama Tabloları'nda depolanabilir.

Neden kullanıcıyı bölüp bu bilgileri farklı yerlerde depolayasınız ki? Çünkü performans açısından bakıldığında, belgeler ne kadar büyükse, sorgular o kadar pahalıdır. Sosyal ağınız için tüm performansa bağlı sorgularınızı yapmak için doğru bilgilerle belgeleri ince tutun. Tam profil edinimi, oturum açma ve veri madenciliği gibi nihai senaryolar için diğer ek bilgileri kullanım analitiği ve Büyük Veri girişimleri için depolayın. Azure SQL Veritabanı'nda çalıştığı için veri madenciliği için veri toplamanın daha yavaş olması gerçekten umurunda değil. Kullanıcılarınızın hızlı ve ince bir deneyime sahip olmasına rağmen endişevar. Cosmos DB'de depolanan bir kullanıcı şu koda benzer:

    {
        "id":"dse4-qwe2-ert4-aad2",
        "name":"John",
        "surname":"Doe",
        "username":"johndoe"
        "email":"john@doe.com",
        "twitterHandle":"\@john"
    }

Ve bir Post gibi görünür:

    {
        "id":"1234-asd3-54ts-199a",
        "title":"Awesome post!",
        "date":"2016-01-02",
        "createdBy":{
            "id":"dse4-qwe2-ert4-aad2",
            "username":"johndoe"
        }
    }

Bir yığın özniteliğinin etkilendiği bir edit ortaya çıktığında, etkilenen belgeleri kolayca bulabilirsiniz. Dizinlenmiş öznitelikleri işaret eden sorguları `SELECT * FROM posts p WHERE p.createdBy.id == "edited_user_id"`kullanın, örneğin , ve sonra parçaları güncelleştirin.

## <a name="the-search-box"></a>Arama kutusu

Kullanıcılar, neyse ki, çok içerik üretecek. Ve doğrudan içerik akışlarında olmayan içerikleri arama ve bulma olanağı sağlayabilmelisiniz, belki de yaratıcıları takip etmediğiniz için, ya da belki de altı ay önce yaptığınız eski gönderiyi bulmaya çalışıyorsunuzdur.

Azure Cosmos DB kullandığınızdan, arama işlemi ve UI dışında herhangi bir kod yazmadan [Azure Bilişsel Arama'yı](https://azure.microsoft.com/services/search/) kullanarak bir arama motorunu birkaç dakika içinde kolayca uygulayabilirsiniz.

Bu süreç neden bu kadar kolay?

Azure Bilişsel Arama, [Dizin leyiciler](https://msdn.microsoft.com/library/azure/dn946891.aspx)olarak adlandırdıkları, veri depolarınıza bağlanan ve dizinlere nesnelerinizi otomatik olarak ekleyen, güncelleyen veya kaldıran arka plan işlemlerini uygular. Onlar bir [Azure SQL Veritabanı dizinleyicileri](https://blogs.msdn.microsoft.com/kaevans/2015/03/06/indexing-azure-sql-database-with-azure-search/), Azure [Blobs dizinleyiciler](../search/search-howto-indexing-azure-blob-storage.md) ve neyse ki, [Azure Cosmos DB dizinleyiciler](../search/search-howto-index-documentdb.md)destekler. Cosmos DB'den Azure Bilişsel Arama'ya bilgi geçişi basittir. Her iki teknoloji de bilgileri JSON biçiminde depolar, bu nedenle [dizininizi oluşturmanız](../search/search-create-index-portal.md) ve Belgelerinizin dizinlenmiş özelliklerini eşlebilmeniz yeterlidir. Bu kadar! Verilerinizin boyutuna bağlı olarak, bulut altyapısındaki en iyi Hizmet Olarak Arama çözümü tarafından tüm içeriğiniz dakikalar içinde aranabilecektir.

Azure Bilişsel Arama hakkında daha fazla bilgi için [Otostopçunun Arama Kılavuzu'nu](https://blogs.msdn.microsoft.com/mvpawardprogram/2016/02/02/a-hitchhikers-guide-to-search/)ziyaret edebilirsiniz.

## <a name="the-underlying-knowledge"></a>Altta yatan bilgi

Her gün büyüyen ve büyüyen tüm bu içeriği depoladıktan sonra şöyle düşünebilirsiniz: Kullanıcılarımdan gelen tüm bu bilgi akışıyla ne yapabilirim?

Cevap basittir: Çalışmak ve ondan öğrenmek koyun.

Ama ne öğrenebilirsin ki? Birkaç kolay örnek arasında [duyarlılık analizi,](https://en.wikipedia.org/wiki/Sentiment_analysis)kullanıcının tercihlerine dayalı içerik önerileri ve hatta sosyal ağınız tarafından yayınlanan içeriğin aile için güvenli olduğundan emin olan otomatik bir içerik moderatörü yer almaktadır.

Şimdi seni bağımlı duruma düşürdüm, muhtemelen bu kalıpları ve bilgileri basit veritabanlarından ve dosyalardan çıkarmak için matematik bilimi doktorasına ihtiyacın olduğunu düşüneceksin, ama yanılıyorsun.

[Cortana Intelligence Suite'in](https://social.technet.microsoft.com/wiki/contents/articles/36688.introduction-to-cortana-intelligence-suite.aspx)bir parçası olan [Azure Machine Learning,](https://azure.microsoft.com/services/machine-learning/)basit bir sürükle ve bırak arabiriminde algoritmalar kullanarak iş akışları oluşturmanıza, [R'de](https://en.wikipedia.org/wiki/R_\(programming_language\))kendi algoritmalarınızı kodlamanıza veya önceden oluşturulmuş ve kullanıma hazır olan API'lerden bazılarını kullanmanıza olanak tanıyan tam olarak yönetilen bir bulut hizmetidir: [Metin Analizi](https://gallery.cortanaanalytics.com/MachineLearningAPI/Text-Analytics-2), [İçerik Moderatörü veya [Öneriler.](https://gallery.azure.ai/Solution/Recommendations-Solution)

Bu Machine Learning senaryolarından herhangi birini elde etmek için, farklı kaynaklardan bilgi almak için [Azure Veri Gölü'nü](https://azure.microsoft.com/services/data-lake-store/) kullanabilirsiniz. Bilgileri işlemek ve Azure Machine Learning tarafından işlenebilecek bir çıktı oluşturmak için [U-SQL'i](https://azure.microsoft.com/documentation/videos/data-lake-u-sql-query-execution/) de kullanabilirsiniz.

Kullanılabilen bir diğer seçenek de, kullanıcılarınızın içeriğini analiz etmek için [Azure Bilişsel Hizmetleri'ni](https://www.microsoft.com/cognitive-services) kullanmaktır; sadece onları daha iyi anlayabilirsiniz [(Text Analytics API](https://www.microsoft.com/cognitive-services/en-us/text-analytics-api)ile yazdıklarını analiz ederek), aynı zamanda istenmeyen veya olgun içeriği algılayabilir ve [Computer Vision API](https://www.microsoft.com/cognitive-services/en-us/computer-vision-api)ile buna göre hareket edebilirsiniz. Bilişsel Hizmetler, kullanmak için makine öğrenimi bilgisi nin her türlüsünün gerektirmeyen birçok kullanıma açık çözüm içerir.

## <a name="a-planet-scale-social-experience"></a>Gezegen ölçeğinde bir sosyal deneyim

Ben ele almalıdır son, ama en az değil, önemli bir makale var: **ölçeklenebilirlik**. Bir mimari tasarlarken, her bileşen kendi kendine ölçeklendirilmelidir. Sonunda daha fazla veri işlemek gerekir, ya da daha büyük bir coğrafi kapsama sahip olmak isteyeceksiniz. Neyse ki, her iki görevi de başarmak Cosmos DB ile **anahtar teslimi** bir deneyimdir.

Cosmos DB, kutudan dinamik bölümleme yi destekler. Belgelerinizde öznitelik olarak tanımlanan belirli bir **bölüm anahtarına**dayalı bölümler oluşturur. Doğru bölme anahtarının tanımlanması tasarım zamanında yapılmalıdır. Daha fazla bilgi için Azure [Cosmos DB'de Bölümleme bölümüne](partitioning-overview.md)bakın.

Sosyal bir deneyim için, bölümleme stratejinizi sorgulama ve yazma biçiminizle uyumlu hale vermelisiniz. (Örneğin, aynı bölüm içinde okur arzu edilir ve birden çok bölüm üzerinde yazıyor yayarak "sıcak noktalar" kaçının.) Bazı seçenekler şunlardır: zamansal anahtara (gün/ay/hafta), içerik kategorisine, coğrafi bölgeye göre veya kullanıcıya göre dayalı bölümler. Her şey gerçekten verileri nasıl sorgulayacağınızdan ve sosyal deneyiminizdeki verileri nasıl göstereceğinize bağlıdır.

Cosmos DB sorgularınızı [(toplamlar](https://azure.microsoft.com/blog/planet-scale-aggregates-with-azure-documentdb/)dahil) tüm bölümlerinizde saydam olarak çalıştırır, böylece verileriniz büyüdükçe herhangi bir mantık eklemenize gerek yoktur.

Zamanla, sonunda trafikte büyüyecek ve kaynak tüketiminiz [(RUs](request-units.md)cinsinden veya İstek Birimleri cinsinden ölçülür) artacaktır. Kullanıcı tabanınız büyüdükçe daha sık okuyup yazabilirsiniz. Kullanıcı tabanı daha fazla içerik oluşturmaya ve okumaya başlar. Yani iş **inizi ölçekleme** yeteneği çok önemlidir. RUs'unuzu artırmak kolaydır. Bunu Azure portalına birkaç tıklamayla veya [API üzerinden komut lar vererek](https://docs.microsoft.com/rest/api/cosmos-db/replace-an-offer)yapabilirsiniz.

![Bir bölüm anahtarını ölçekleme ve tanımlama](./media/social-media-apps/social-media-apps-scaling.png)

İşler yoluna devam ederse ne olur? Başka bir bölgeden, ülkeden veya kıtadan kullanıcıların platformunuzu fark edip kullanmaya başladığını varsayalım. Ne büyük bir sürpriz!

Ama bekleyin! Kısa sürede platform ile deneyimlerinin en iyi olmadığını fark edersiniz. Operasyon bölgenizden o kadar uzaktalar ki gecikme çok kötü. Belli ki bırakmalarını istemiyorsun. Keşke **küresel erişiminizi genişletmenin**kolay bir yolu olsaydı? Var!

Cosmos DB, [verilerinizi](../cosmos-db/tutorial-global-distribution-sql-api.md) birkaç tıklamayla küresel ve şeffaf bir şekilde çoğaltmanızı ve [istemci kodunuzdan](../cosmos-db/tutorial-global-distribution-sql-api.md)kullanılabilir bölgeler arasında otomatik olarak seçim yapmanızı sağlar. Bu işlem aynı zamanda [birden çok başarısız bölgeniz](high-availability.md)olabileceği anlamına da gelir.

Verilerinizi genel olarak çoğaltırken, müşterilerinizin bu verilerden yararlanabilmesini sağlamanız gerekir. Bir web ön ucu kullanıyorsanız veya mobil istemcilerden API'lere erişiyorsanız, genişletilmiş küresel kapsamınızı desteklemek için bir performans yapılandırması kullanarak [Azure Trafik Yöneticisi'ni](https://azure.microsoft.com/services/traffic-manager/) dağıtabilir ve Azure Uygulama Hizmetinizi istenen tüm bölgelerde klonlayabilirsiniz. Müşterileriniz ön yüz veya API'larınıza eriştiklerinde, en yakın Uygulama Hizmetine yönlendirilirler ve bu da yerel Cosmos DB yinelemesine bağlanır.

![Sosyal platformunuza küresel kapsama alanı ekleme](./media/social-media-apps/social-media-apps-global-replicate.png)

## <a name="conclusion"></a>Sonuç

Bu makale, düşük maliyetli hizmetlerle Azure'da tamamen sosyal ağlar oluşturma alternatiflerine ışık tutmaktadır. çok katmanlı depolama çözümü ve "Merdiven" adı verilen veri dağıtımının kullanımını teşvik ederek sonuçlar sunar.

![Sosyal ağ için Azure hizmetleri arasındaki etkileşim diyagramı](./media/social-media-apps/social-media-apps-azure-solution.png)

Gerçek şu ki, bu tür senaryolar için gümüş kurşun yok. Harika deneyimler oluşturmamıza olanak tanıyan harika hizmetlerin birleşimi tarafından yaratılan sinerjidir: Azure Cosmos DB'nin büyük bir sosyal uygulama sağlama hızı ve özgürlüğü, Azure Bilişsel Arama gibi birinci sınıf bir arama çözümünün arkasındaki zeka, Azure Uygulama Hizmetleri'nin dil-agnostik uygulamalara değil, güçlü arka plan süreçlerine ve büyük miktarda veriyi ve Azure Machine Learning'in analitik gücünü depolamak için genişletilebilir Azure Depolama ve Azure SQL Veritabanı'nı barındırma esnekliği süreçlerinize geri bildirim sağlayabilecek ve doğru içeriği doğru kullanıcılara sunmamıza yardımcı olabilecek bilgi ve zeka oluşturun.

## <a name="next-steps"></a>Sonraki adımlar

Cosmos DB kullanım örnekleri hakkında daha fazla bilgi edinmek için [Common Cosmos DB kullanım örneklerine](use-cases.md)bakın.
