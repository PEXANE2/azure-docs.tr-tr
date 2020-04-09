---
title: Azure Cosmos DB ve Azure Fonksiyonları ile sunucusuz veritabanı hesaplama
description: Azure Cosmos DB ve Azure Fonksiyonlarının olay odaklı sunucusuz bilgi işlem uygulamaları oluşturmak için birlikte nasıl kullanılabileceğini öğrenin.
author: SnehaGunda
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 07/17/2019
ms.author: sngun
ms.openlocfilehash: 079c246f87bb8294f3c7ad6dea3391f5c67ba0ad
ms.sourcegitcommit: 7d8158fcdcc25107dfda98a355bf4ee6343c0f5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2020
ms.locfileid: "80985261"
---
# <a name="serverless-database-computing-using-azure-cosmos-db-and-azure-functions"></a>Azure Cosmos DB ve Azure Fonksiyonlarını kullanarak sunucusuz veritabanı hesaplama

Sunucusuz bilgi işlem, tek tek tekrarlanabilir ve durumsuz olan mantık parçalarına odaklanma yeteneğiyle ilgilidir. Bu parçalar altyapı yönetimi gerektirmez ve kaynakları yalnızca çalıştıkları saniyeler veya milisaniyeler için tüketirler. Sunucusuz bilgi işlem hareketinin özünde, [Azure İşlevleri](https://azure.microsoft.com/services/functions)tarafından Azure ekosisteminde kullanıma sunulan işlevler vardır. Azure'daki diğer sunucusuz yürütme ortamları hakkında bilgi edinmek için Azure sayfasında [sunucusuz](https://azure.microsoft.com/solutions/serverless/) görün. 

[Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db) ve Azure İşlevler arasındaki yerel tümleştirmeyle, doğrudan Azure Cosmos DB hesabınızdan veritabanı tetikleyicileri, giriş bağlamaları ve çıktı bağlamaları oluşturabilirsiniz. Azure İşlevlerini ve Azure Cosmos DB'yi kullanarak, küresel bir kullanıcı tabanı için zengin verilere düşük gecikme li uygulamalar oluşturup dağıtabilirsiniz.

## <a name="overview"></a>Genel Bakış

Azure Cosmos DB ve Azure Fonksiyonları, veritabanlarınızı ve sunucusuz uygulamalarınızı aşağıdaki yollarla entegre etmenizi sağlar:

* Cosmos DB için olay odaklı **Azure İşlevleri tetikleyicisi**oluşturun. Bu tetikleyici, Azure Cosmos kapsayıcınızı değişiklikler için izlemek için [özet akışı](change-feed.md) akışlarını değiştirmeye dayanır. Bir kapsayıcıda herhangi bir değişiklik yapıldığında, değişiklik akışı tetikleyiciye gönderilir ve bu da Azure İşi'ni çağırır.
* Alternatif olarak, bir Azure İşlevi'ni giriş **bağlama**kullanarak Bir Azure Cosmos kapsayıcısına bağlayın. Giriş bağlamaları, bir işlev yürütüldüğünde bir kapsayıcıdan gelen verileri okur.
* **Bir çıktı bağlama**kullanarak işlevi Azure Cosmos kapsayıcısına bağlayın. Çıktı bağlamaları, bir işlev tamamlandığında verileri kapsayıcıya yazar.

> [!NOTE]
> Şu anda, Cosmos DB için Azure İşlevler tetikleyici, giriş bağlamaları ve çıktı bağlamaları yalnızca SQL API ile kullanılmak üzere desteklenir. Diğer tüm Azure Cosmos DB API'leri için, API'niz için statik istemciyi kullanarak işlevinizden veritabanına erişmelisiniz.


Aşağıdaki diyagram, bu üç tümleştirmenin her birini göstermektedir: 

![Azure Cosmos DB ve Azure Fonksiyonları nasıl tümleşir?](./media/serverless-computing-database/cosmos-db-azure-functions-integration.png)

Azure Cosmos DB için Azure İşlevleri tetikleyicisi, giriş bağlama ve çıktı bağlama aşağıdaki kombinasyonlarda kullanılabilir:

* Cosmos DB için bir Azure İşlevler tetikleyicisi, farklı bir Azure Cosmos kapsayıcısına bağlanan bir çıktıyla kullanılabilir. Bir işlev, değişiklik akışındaki bir öğe üzerinde bir eylem gerçekleştirdikten sonra başka bir kapsayıcıya yazabilirsiniz (geldiği aynı kapsayıcıya yazmak etkili bir özyinelemeli döngü oluşturur). Veya, bir çıktı bağlama kullanarak, değiştirilen tüm öğeleri bir kapsayıcıdan farklı bir kapsayıcıya etkili bir şekilde geçirmek için Cosmos DB için bir Azure İşlevler tetikleyicisi kullanabilirsiniz.
* Azure Cosmos DB için giriş bağlamaları ve çıktı bağlamaları aynı Azure İşlevi'nde kullanılabilir. Bu, giriş bağlama ile belirli verileri bulmak, Azure İşlevi'nde değiştirmek ve değişiklikten sonra aynı kapsayıcıya veya farklı bir kapsayıcıya kaydetmek istediğiniz durumlarda iyi çalışır.
* Bir Azure Cosmos kapsayıcısına bağlanan bir giriş, Cosmos DB için Azure İşlevleri tetikleyicisiyle aynı işlevde kullanılabilir ve çıktı bağlamalı veya olmadan da kullanılabilir. Bu kombinasyonu, alışveriş sepeti hizmetinizdeki yeni siparişlerin değişim akışına güncel döviz bilgilerini (bir döviz konteynerine bağlanan bir girişle birlikte çekilmiş) uygulamak için kullanabilirsiniz. Geçerli para birimi dönüştürme uygulandığı güncelleştirilmiş alışveriş sepeti toplamı, çıktı bağlama kullanılarak üçüncü bir kapsayıcıya yazılabilir.

## <a name="use-cases"></a>Uygulama alanları

Aşağıdaki kullanım örnekleri, verilerinizi olay odaklı Azure Fonksiyonları'na bağlayarak Azure Cosmos DB verilerinizden en iyi şekilde yararlanmanın birkaç yolunu gösterir.

### <a name="iot-use-case---azure-functions-trigger-and-output-binding-for-cosmos-db"></a>IoT kullanım örneği - Cosmos DB için Azure Fonksiyonları tetikleme ve çıktı bağlama

IoT uygulamalarında, kontrol motoru ışığı bağlı bir arabada görüntülendiğinde bir işlev çağırabilirsiniz.

**Uygulama:** Cosmos DB için Azure Fonksiyonları tetikleyicisi ve çıktı bağlama kullanma

1. **Cosmos DB için bir Azure İşlevler** tetikleyicisi, bağlı bir araçta yanmakta olan kontrol motoru ışığı gibi araç uyarılarıyla ilgili olayları tetiklemek için kullanılır.
2. Kontrol motoru ışığı geldiğinde, sensör verileri Azure Cosmos DB'ye gönderilir.
3. Azure Cosmos DB yeni sensör veri belgeleri oluşturur veya günceller, ardından bu değişiklikler Cosmos DB için Azure Fonksiyonları tetikleyicisine aktarılır.
4. Tüm değişiklikler değişiklik akışı üzerinden akış olarak, sensör veri koleksiyonundaki her veri değişikliğinde tetikleyici çağrılır.
5. Sensör verilerini garanti bölümüne göndermek için işlevde bir eşik koşulu kullanılır.
6. Sıcaklık da belirli bir değerin üzerindeyse, sahibine de bir uyarı gönderilir.
7. İşlev **üzerindeki çıktı bağlayıcısı,** kontrol motoru olayı yla ilgili bilgileri depolamak için başka bir Azure Cosmos konteynerindeki araç kaydını güncelleştirir.

Aşağıdaki resimde, bu tetikleyici için Azure portalında yazılan kod gösterilmektedir.

![Azure portalında Cosmos DB için Azure İşlevleri tetikleyicisi oluşturma](./media/serverless-computing-database/cosmos-db-trigger-portal.png)

### <a name="financial-use-case---timer-trigger-and-input-binding"></a>Finansal kullanım örneği - Zamanlayıcı tetikleyici setetik ve giriş bağlama

Finansal uygulamalarda, banka hesap bakiyesi belirli bir tutarın altına düştüğünde bir işlev çağırabilirsiniz.

**Uygulama:** Azure Cosmos DB giriş bağlamalı bir zamanlayıcı tetikleyicisi

1. [Zamanlayıcı tetikleyicisini](../azure-functions/functions-bindings-timer.md)kullanarak, bir Azure Cosmos kapsayıcısında depolanan banka hesap bakiyesi bilgilerini giriş **bağlamayı**kullanarak zaman aralıklarında alabilirsiniz.
2. Bakiye kullanıcı tarafından belirlenen düşük bakiye eşiğinin altındaysa, Azure İşlevi'nden bir eylemle izleyin.
3. Çıktı bağlama, bir hizmet hesabından düşük bakiyeli hesapların her biri için tanımlanan e-posta adreslerine e-posta gönderen bir [SendGrid tümleştirmesi](../azure-functions/functions-bindings-sendgrid.md) olabilir.

Aşağıdaki resimler, bu senaryo için Azure portalındaki kodu gösterir.

![Bir finansal senaryo için timer tetikleyicisi için Index.js dosyası](./media/serverless-computing-database/cosmos-db-functions-financial-trigger.png)

![Mali senaryo için Timer tetikleyicisi için Run.csx dosyası](./media/serverless-computing-database/azure-function-cosmos-db-trigger-run.png)

### <a name="gaming-use-case---azure-functions-trigger-and-output-binding-for-cosmos-db"></a>Oyun kullanım örneği - Cosmos DB için Azure Fonksiyonları tetikleme ve çıktı bağlama 

Oyun oynarken, yeni bir kullanıcı oluşturulduğunda [Azure Cosmos DB Gremlin API'sini](graph-introduction.md)kullanarak onları tanıyabilecek diğer kullanıcıları arayabilirsiniz. Daha sonra kolay alma için sonuçları bir [Azure Cosmos DB SQL veritabanına] yazabilirsiniz.

**Uygulama:** Cosmos DB için Azure Fonksiyonları tetikleyicisi ve çıktı bağlama kullanma

1. Tüm kullanıcıları depolamak için bir Azure Cosmos DB [grafik veritabanı](graph-introduction.md) nı kullanarak Cosmos DB için Azure İşlevleri tetikleyicisi içeren yeni bir işlev oluşturabilirsiniz. 
2. Yeni bir kullanıcı takıldığında, işlev çağrılır ve sonuç bir **çıktı bağlama**kullanılarak depolanır.
3. İşlev, grafik veritabanını yeni kullanıcıyla doğrudan ilişkili tüm kullanıcıları aramak için sorgular ve bu veri kümesini işleve döndürür.
4. Bu veriler daha sonra yeni kullanıcıya bağlı arkadaşlarını gösteren herhangi bir ön uç uygulaması tarafından kolayca alınabilecek bir Azure Cosmos DB'de depolanır.

### <a name="retail-use-case---multiple-functions"></a>Perakende kullanım örneği - Birden çok fonksiyon

Perakende uygulamalarında, bir kullanıcı sepetine bir öğe eklediğinde, artık isteğe bağlı iş ardışık iş hattı bileşenleri için işlevler oluşturma ve çağırma esnekliğine sahip olabilirsiniz.

**Uygulama:** Cosmos DB'nin tek bir kapsayıcıyı dinlemesi için birden çok Azure Işlevi tetikler

1. Cosmos DB için Azure İşleme tetikleyicileri ekleyerek birden çok Azure İşi oluşturabilirsiniz ve bunların hepsi alışveriş sepeti verilerinin aynı değişiklik akışını dinler. Birden çok işlev aynı değişiklik akışını dinlediğinde, her işlev için yeni bir kira koleksiyonu gerektiğini unutmayın. Kira koleksiyonları hakkında daha fazla bilgi için [Bkz. Özet Akışı İşlemci kitaplığını Değiştir'i anlama.](change-feed-processor.md)
2. Bir kullanıcı alışveriş sepetine yeni bir öğe eklendiğinde, her işlev alışveriş sepeti konteynerindeki değişiklik akışı tarafından bağımsız olarak çağrılır.
   * Bir işlev, kullanıcının ilgilenebileceği diğer öğelerin ekranını değiştirmek için geçerli sepetin içeriğini kullanabilir.
   * Başka bir işlev stok toplamlarını güncelleyebilir.
   * Başka bir işlev, belirli ürünler için müşteri bilgilerini pazarlama departmanına gönderebilir ve bu da onlara bir promosyon postası gönderebilir. 

     Herhangi bir departman, değişiklik akışını dinleyerek Cosmos DB için bir Azure İşlevleri oluşturabilir ve işlemdeki kritik sipariş işleme olaylarını geciktirmeyeceklerinden emin olabilir.

Tüm bu kullanım durumlarında, işlev uygulamanın kendisini ayırdığı için, her zaman yeni uygulama örnekleri döndürmeniz gerekmez. Bunun yerine, Azure İşlevler gerektiğinde ayrı işlemleri tamamlamak için tek tek işlevleri döndürür.

## <a name="tooling"></a>Araçlar

Azure Cosmos DB ve Azure Fonksiyonları arasındaki yerel tümleştirme, Azure portalında ve Visual Studio 2019'da kullanılabilir.

* Azure İşlevler portalında bir tetikleyici oluşturabilirsiniz. Hızlı başlatma yönergeleri için [bkz.](../azure-functions/functions-create-cosmos-db-triggered-function.md)
* Azure Cosmos DB portalında, aynı kaynak grubundaki mevcut bir Azure İşlevi uygulamasına Cosmos DB için Azure İşleme tetikleyicisi ekleyebilirsiniz.
* Visual Studio 2019'da, [Azure İşlevaraçları araçlarını](../azure-functions/functions-develop-vs.md)kullanarak tetikleyiciyi oluşturabilirsiniz:

    >[!VIDEO https://www.youtube.com/embed/iprndNsUeeg]

## <a name="why-choose-azure-functions-integration-for-serverless-computing"></a>Sunucusuz bilgi işlem için neden Azure İşlevler tümleştirmesini seçmelisiniz?

Azure İşlevler, altyapı sağlama veya yönetmeden, isteğe bağlı olarak çalıştırılabilen ölçeklenebilir çalışma birimleri veya kısa mantık parçaları oluşturma olanağı sağlar. Azure İşlevlerini kullanarak, Azure Cosmos veritabanınızdaki değişikliklere yanıt vermek için tam gelişmiş bir uygulama oluşturmanız gerekmez, belirli görevler için yeniden kullanılabilir küçük işlevler oluşturabilirsiniz. Ayrıca, HTTP istekleri veya zamanlanmış tetikleyici gibi olaya yanıt olarak Azure Cosmos DB verilerini azure işlevine giriş veya çıktı olarak da kullanabilirsiniz.

Azure Cosmos DB, sunucusuz bilgi işlem yapınız için aşağıdaki nedenlerden dolayı önerilen veritabanıdır:

* **Tüm verilerinize anında erişim**: Azure Cosmos DB varsayılan olarak tüm verileri [otomatik olarak dizine eklediği](index-policy.md) ve bu dizinleri anında kullanılabilir hale getiren her değere parçalı erişime sahipsiniz. Bu, veritabanınıza sürekli olarak sorgulayabildiğiniz, güncelleyebileceğiniz ve yeni öğeler ekleyebileceğiniz ve Azure İşlevleri üzerinden anında erişebildiğiniz anlamına gelir.

* **Şemasız.** Azure Cosmos DB şemasızdır , bu nedenle bir Azure İşlevi'nden elde edilen tüm veri çıktılarını benzersiz bir şekilde işleyebilir. Bu "her şeyi işleme" yaklaşımı, azure cosmos DB'ye tüm çıktıları içeren çeşitli Işlevler oluşturmayı kolaylaştırır.

* **Ölçeklenebilir iş.** Azure Cosmos DB'de iş ortalığı anında yukarı ve aşağı ölçeklendirilebilir. Aynı kapsayıcıya sorgulayan ve yazan yüzlerce veya binlerce Fonksiyon'unuzun varsa, yükü işlemek için [RU/s'lerinizi](request-units.md) ölçeklendirebilirsiniz. Tüm işlevler, tahsis edilen RU/s'leri kullanarak paralel olarak çalışabilir ve verilerinizin [tutarlı](consistency-levels.md)olacağı garanti edilir.

* **Genel çoğaltma**. Gecikme süresini azaltmak ve verilerinizin kullanıcılarınızın bulunduğu yere en yakın coğrafi yerini bulmak için Azure Cosmos DB verilerini [dünya çapında](distribute-data-globally.md) çoğaltabilirsiniz. Tüm Azure Cosmos DB sorgularında olduğu gibi, olay odaklı tetikleyicilerden elde edilen veriler kullanıcıya en yakın Azure Cosmos DB'den alınan verileri okur.

Verileri depolamak için Azure Fonksiyonları ile tümleştirmek istiyorsanız ve derin dizin oluşturmanız gerekmiyorsa veya ekleri ve ortam dosyalarını depolamanız gerekiyorsa, [Azure Blob Depolama tetikleyicisi](../azure-functions/functions-bindings-storage-blob.md) daha iyi bir seçenek olabilir.

Azure Fonksiyonlarının Avantajları: 

* **Olay odaklı**. Azure Fonksiyonları olay odaklıdır ve Azure Cosmos DB'den bir değişiklik akışını dinleyebilir. Bu, dinleme mantığı oluşturmanız gerekolmadığı, sadece dinlediğiniz değişikliklere dikkat ettiğiniz anlamına gelir. 

* **Sınır yok.** İşlevler paralel olarak yürütülür ve hizmet istediğiniz kadar döner. Parametreleri sen ayarla.

* **Hızlı görevler için iyi.** Hizmet, bir olay çıktığında yeni işlev örneklerini döndürür ve işlev tamamlanır tamamlanmaz onları kapatır. Yalnızca işlevlerinizin çalıştığı süre için ödeme yapıyorsunuz.

Akış, Mantık Uygulamaları, Azure İşlerİ veya Web İşlerinin uygulamanız için en iyi olup olmadığından emin değilseniz, [bkz.](../azure-functions/functions-compare-logic-apps-ms-flow-webjobs.md)

## <a name="next-steps"></a>Sonraki adımlar

Şimdi Azure Cosmos DB ve Azure Fonksiyonlarını gerçekten bağlayalım: 

* [Azure portalında Cosmos DB için Azure İşlevleri tetikleyicisi oluşturma](../azure-functions/functions-create-cosmos-db-triggered-function.md)
* [Azure Cosmos DB giriş bağlama işlemiyle Azure İşlevleri HTTP Tetikleyicisi oluşturma](../azure-functions/functions-bindings-cosmosdb.md?tabs=csharp)
* [Azure Cosmos DB bağlamaları ve tetikleyicileri](../azure-functions/functions-bindings-cosmosdb-v2.md)