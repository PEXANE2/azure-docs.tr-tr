---
title: Azure Cosmos DB'de depolanan yordamlar, tetikleyiciler ve UDF'lerle çalışma
description: Bu makalede, Azure Cosmos DB'de depolanan yordamlar, tetikleyiciler ve kullanıcı tanımlı işlevler gibi kavramlar tanıtıştır.
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 04/09/2020
ms.author: tisande
ms.reviewer: sngun
ms.openlocfilehash: 13256377b8a8aaebf59196df57eef67d3b960cb8
ms.sourcegitcommit: ae3d707f1fe68ba5d7d206be1ca82958f12751e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/10/2020
ms.locfileid: "81010554"
---
# <a name="stored-procedures-triggers-and-user-defined-functions"></a>Depolanan yordamlar, tetikleyiciler ve kullanıcı tanımlı işlevler

Azure Cosmos DB JavaScript'in dil ile tümleşik, işlem tabanlı yürütmesini sağlar. Azure Cosmos DB'de SQL API'sini kullanırken, JavaScript dilinde **depolanan yordamları,** **tetikleyicileri**ve **kullanıcı tanımlı işlevleri (UDF'ler)** yazabilirsiniz. Veritabanı altyapısı içinde yürütülen mantığınızı JavaScript'te yazabilirsiniz. [Azure portalını,](https://portal.azure.com/) [Azure Cosmos DB'deki JavaScript dili tümleşik sorgu API'sini](javascript-query-api.md) veya [Cosmos DB SQL API istemci SDK'larını](how-to-use-stored-procedures-triggers-udfs.md)kullanarak tetikleyiciler, depolanmış yordamlar ve UDF'ler oluşturabilir ve yürütebilirsiniz.

## <a name="benefits-of-using-server-side-programming"></a>Sunucu tarafı programlama kullanmanın yararları

JavaScript'te depolanmış yordamları, tetikleyicileri ve kullanıcı tanımlı işlevleri (UDF'ler) yazmak zengin uygulamalar oluşturmanıza olanak tanır ve aşağıdaki avantajlara sahiptir:

* **Usul mantığı:** JavaScript iş mantığı ifade etmek için zengin ve tanıdık arayüzü sağlayan bir üst düzey programlama dili olarak. Veriler üzerinde karmaşık işlemler dizisi gerçekleştirebilirsiniz.

* **Atomik işlemler:** Azure Cosmos DB, tek bir depolanan yordam veya tetikleyici içinde gerçekleştirilen veritabanı işlemlerinin atomik olduğunu garanti eder. Bu atomik işlevsellik, bir uygulamanın ilgili işlemleri tek bir toplu iş halinde birleştirmesine olanak tanır, böylece tüm işlemler başarılı olur veya bunların hiçbiri başarılı olmaz.

* **Performans:** JSON verileri, JavaScript dil türü sistemine eşlenir. Bu eşleme, arabellek havuzunda JSON belgelerinin tembel olarak maddeleştirilmesi ve bunları yürütme koduna isteğe bağlı olarak kullanılabilir hale getirme gibi bir dizi optimizasyona olanak tanır. Veritabanına iş mantığı gönderimi ile ilişkili diğer performans avantajları da vardır:

   * *Toplu İşlem:* Ekler gibi işlemleri gruplayabilir ve toplu olarak gönderebilirsiniz. Ağ trafiği gecikme maliyetleri ve ayrı hareketler oluşturmak için mağaza genel giderleri önemli ölçüde azalır.

   * *Ön derleme:* Depolanan yordamlar, tetikleyiciler ve UDF'ler, her komut dosyası çağırma sırasında derleme maliyetini önlemek için bayt kodu biçiminde örtülü olarak önceden derlenir. Ön derleme nedeniyle, depolanan yordamların çağrılması hızlıdır ve düşük bir ayak izine sahiptir.

   * *Sıralama:* Bazen işlemler, verilerde bir veya ek güncelleştirme gerçekleştirebilecek bir tetikleyici mekanizma gerekir. Atomicity ek olarak, sunucu tarafında yürütüldüğünde performans yararları da vardır.

* **Kapsülleme:** Depolanan yordamlar, mantığı tek bir yerde gruplandırmak için kullanılabilir. Kapsülleme, verilerin üzerine, uygulamalarınızı verilerden bağımsız olarak geliştirmenize olanak tanıyan bir soyutlama katmanı ekler. Bu soyutlama katmanı, veriler şema olmadığında ve doğrudan uygulamanıza ek mantık eklemeyi yönetmeniz gerektiğinde yararlıdır. Soyutlama, komut dosyalarından erişimi kolaylaştırarak verileri güvende tutmanızı sağlar.

> [!TIP]
> Depolanan yordamlar, yazma ağırlıklı ve bölüm anahtarı değeri arasında bir işlem gerektiren işlemler için en uygun olanlardır. Depolanan yordamları kullanıp kullanmamaya karar verirken, mümkün olan en fazla yazma miktarını kapsülleme konusunda optimize edin. Genel olarak konuşursak, depolanan yordamlar okuma veya sorgu işlemleri çok sayıda yapmak için en verimli araç değildir, bu nedenle istemciye dönmek için okuma çok sayıda toplu için depolanan yordamları kullanarak istenen yararı sağlamaz. En iyi performans için, bu okuma ağır işlemleri Cosmos SDK kullanılarak istemci tarafında yapılmalıdır. 

## <a name="transactions"></a>İşlemler

Tipik bir veritabanındaki işlem, tek bir mantıksal çalışma birimi olarak gerçekleştirilen işlem dizisi olarak tanımlanabilir. Her işlem **ACID özellik garantileri**sağlar. ASİt anlamına gelen iyi bilinen bir kısaltmadır: **Bir**tomicity, **C**onsistency, **I**solation, ve **D**urability. 

* Atomiklik, bir işlem içinde yapılan tüm işlemlerin tek bir birim olarak kabul edilir ve bunların tümü taahhüt edilir ya da hiçbiri değildir garanti eder. 

* Tutarlılık, verilerin hareketler arasında her zaman geçerli bir durumda olmasını sağlar. 

* Yalıtım, iki işlemin birbirini etkilemediğini garanti eder – birçok ticari sistem, uygulama gereksinimlerine göre kullanılabilecek birden çok yalıtım düzeyi sağlar. 

* Dayanıklılık, veritabanında yapılan herhangi bir değişikliğin her zaman mevcut olmasını sağlar.

Azure Cosmos DB'de, JavaScript çalışma süresi veritabanı altyapısının içinde barındırılır. Bu nedenle, depolanan yordamlar ve tetikleyiciler içinde yapılan istekler veritabanı oturumu ile aynı kapsamda yürütülür. Bu özellik, Azure Cosmos DB'nin depolanan yordamın veya tetikleyicinin parçası olan tüm işlemler için ACID özelliklerini garanti etmesini sağlar. Örnekler için, [hareketler makalesinin nasıl uygulanacağını](how-to-write-stored-procedures-triggers-udfs.md#transactions) görün.

### <a name="scope-of-a-transaction"></a>İşlemin kapsamı

Depolanan yordamlar bir Azure Cosmos kapsayıcısıyla ilişkilidir ve depolanan yordam yürütmemantıksal bir bölüm anahtarına göre kapsamlıdır. Depolanan yordamlar, yürütme sırasında hareketin kapsamı için mantıksal bölümü tanımlayan mantıksal bir bölüm anahtarı değeri içermelidir. Daha fazla bilgi için Azure [Cosmos DB bölümleme](partition-data.md) makalesine bakın.

### <a name="commit-and-rollback"></a>Commit ve rollback

İşlemler azure cosmos DB JavaScript programlama modeline yerel olarak entegre edilmiştir. JavaScript işlevi içinde, tüm işlemler otomatik olarak tek bir işlem altında sarılır. Depolanan yordamdaki JavaScript mantığı istisnasız tamamlanırsa, işlem deki tüm işlemler veritabanına adamıştır. Azure `BEGIN TRANSACTION` Cosmos DB'deki gibi ve `COMMIT TRANSACTION` (ilişkisel veritabanlarına tanıdık) gibi ifadeler örtülüdür. Komut dosyasında herhangi bir özel durum varsa, Azure Cosmos DB JavaScript çalışma süresi tüm işlemi geri alır. Bu nedenle, özel durum atma, `ROLLBACK TRANSACTION` Azure Cosmos DB'deki bir özel durumla etkin bir şekilde eşdeğerdir.

### <a name="data-consistency"></a>Veri tutarlılığı

Depolanan yordamlar ve tetikleyiciler her zaman bir Azure Cosmos kapsayıcısının birincil yinelemesi üzerinde yürütülür. Bu özellik, depolanan yordamlardan alınan okumaların [güçlü bir tutarlılık](consistency-levels-tradeoffs.md)sunmasını sağlar. Kullanıcı tanımlı işlevleri kullanan sorgular birincil veya ikincil yinelemede yürütülebilir. Depolanan yordamlar ve tetikleyiciler işlem yazmalarını desteklemek için tasarlanmıştır – bu arada salt okunur mantığı en iyi uygulama tarafı mantığı olarak uygulanır ve [Azure Cosmos DB SQL API SDK'larını](sql-api-dotnet-samples.md)kullanarak sorgular veritabanı veri tabanı verime doygunluk sağlar. 

## <a name="bounded-execution"></a>Sınırlanmış yürütme

Tüm Azure Cosmos DB işlemleri belirtilen zaman aşımı süresi içinde tamamlanmalıdır. Bu kısıtlama JavaScript işlevleri için geçerlidir - depolanan yordamlar, tetikleyiciler ve kullanıcı tanımlı işlevler. Bir işlem bu süre içinde tamamlanmazsa, işlem geri alınır.

JavaScript işlevlerinizin zaman sınırı içinde bitmesini sağlayabilir veya toplu yürütme/devam yürütme için devam tabanlı bir model uygulayabilirsiniz. Zaman sınırlarını işlemek için depolanan yordamların ve tetikleyicilerin geliştirilmesini kolaylaştırmak için, Azure Cosmos kapsayıcısının altındaki tüm işlevler (örneğin, öğelerin oluşturulması, okunması, güncelleştirilmesi ve silinmesi) işlemin tamamlanıp tamamlanmayacağını gösteren bir boolean değeri döndürür. Bu değer yanlışsa, komut dosyası yapılandırılan değerden daha fazla zaman veya sağlanan iş tükettiği için yordamın yürütmeyi tamamlaması gerektiğinin bir göstergesidir. İlk kabul edilmeyen mağaza işleminden önce sıraya girilen işlemler, depolanan yordam zamanında tamamlanırsa ve daha fazla istek sıraya girmezse tamamlanagaranti edilir. Bu nedenle, komut dosyasının denetim akışını yönetmek için JavaScript'in geri arama kuralını kullanarak işlemler birer birer sıralanmalıdır. Komut dosyaları sunucu tarafında yürütüldolduğundan, bunlar kesinlikle yönetilir. Yürütme sınırlarını sürekli olarak ihlal eden komut dosyaları etkin olmayabilir ve yürütülemez ve yürütme sınırlarını onurlandırmak için yeniden oluşturulmalıdır.

JavaScript işlevleri de [sağlanan iş verme kapasitesine](request-units.md)tabidir. JavaScript işlevleri kısa bir süre içinde çok sayıda istek birimi kullanabilir ve sağlanan iş birimi kapasitesi sınırına ulaşılırsa hız sınırı sınırlı olabilir. Bu veritabanı işlemleri istemciden aynı işlemleri yürütme biraz daha az pahalı olmasına rağmen komut dosyaları veritabanı işlemleri yürütme harcanan iş sonuna ek olarak ek iş elde etme kullanır unutmayın.

## <a name="triggers"></a>Tetikleyiciler

Azure Cosmos DB iki tür tetikleyiciyi destekler:

### <a name="pre-triggers"></a>Ön tetikleyiciler

Azure Cosmos DB, bir Azure Cosmos öğesinde işlem gerçekleştirerek çağrılabilen tetikleyiciler sağlar. Örneğin, öğe oluştururken bir ön tetikleyici belirtebilirsiniz. Bu durumda öğe oluşturulmadan önce ön tetikleyici çalıştırılır. Ön tetikleyicilerin hiçbir giriş parametresi olamaz. Gerekirse özgün istekten belge gövdesini güncelleştirmek için istek nesnesi kullanılabilir. Tetikleyiciler kaydedilirken kullanıcılar birlikte çalıştırılabileceği işlemleri belirtebilir. Bir tetikleyici ile `TriggerOperation.Create`oluşturulduysa, bu bir değiştirme işleminde tetikleyicinin kullanılmasına izin verilmeyeceğini anlamına gelir. Örnekler için, tetikleyiciler makalesi [nasıl yazılır'](how-to-write-stored-procedures-triggers-udfs.md#triggers) a bakın.

### <a name="post-triggers"></a>Son tetikleyiciler

Ön tetikleyicilere benzer şekilde, tetikleyici sonrası tetikleyiciler de Bir Azure Cosmos öğesindeki bir işlemle ilişkilidir ve herhangi bir giriş parametresi gerektirmezler. İşlem *tamamlandıktan sonra* çalışır lar ve istemciye gönderilen yanıt iletisine erişebilirler. Örnekler için, tetikleyiciler makalesi [nasıl yazılır'](how-to-write-stored-procedures-triggers-udfs.md#triggers) a bakın.

> [!NOTE]
> Kayıtlı tetikleyiciler, ilgili işlemleri (oluşturma / silme / değiştirme / güncelleme) gerçekleştiğinde otomatik olarak çalışmaz. Bu işlemler yürütürken tetikleyicilerin açıkça çağrılması gerekir. Daha fazla bilgi edinmek [için tetikleyiciler makalesinin nasıl çalıştırılacak işletilecesine](how-to-use-stored-procedures-triggers-udfs.md#pre-triggers) bakın.

## <a name="user-defined-functions"></a><a id="udfs"></a>Kullanıcı tanımlı işlevler

SQL API sorgu dili sözdizimini genişletmek ve özel iş mantığını kolayca uygulamak için [kullanıcı tanımlı işlevler](sql-query-udfs.md) (UDF'ler) kullanılır. Bunlar yalnızca sorgular içinde çağrılabilir. UDF'lerin bağlam nesnesine erişimi yoktur ve yalnızca JavaScript'i hesaplamak için kullanılır. Bu nedenle, UDF'ler ikincil yinelemeler üzerinde çalıştırılabilir. Örnekler için, [kullanıcı tanımlı işlevler](how-to-write-stored-procedures-triggers-udfs.md#udfs) makalesi nasıl yazılır makalesine bakın.

## <a name="javascript-language-integrated-query-api"></a><a id="jsqueryapi"></a>JavaScript dil tümleşik sorgu API

SQL API sorgu sözdizimini kullanarak sorgu lar vermenin yanı sıra, [sunucu tarafındaki SDK,](https://azure.github.io/azure-cosmosdb-js-server) SQL hakkında hiçbir bilgi olmadan javascript arabirimi kullanarak sorguları gerçekleştirmenize olanak tanır. JavaScript sorgu API'si, yüklem işlevlerini işlev çağrıları dizisine geçirerek sorguları programlı bir şekilde oluşturmanıza olanak tanır. Sorgular JavaScript çalışma süresi yle ayrıştırılır ve Azure Cosmos DB içinde verimli bir şekilde yürütülür. JavaScript sorgu API desteği hakkında bilgi edinmek için [JavaScript dili tümleşik sorgu API makalesiile çalışma](javascript-query-api.md) başlıklı bakın. Örnekler için, [Javascript Query API makalesini kullanarak depolanan yordamları ve tetikleyicileri nasıl yazılanın.](how-to-write-javascript-query-api.md)

## <a name="next-steps"></a>Sonraki adımlar

Azure Cosmos DB'de depolanmış yordamları, tetikleyicileri ve kullanıcı tanımlı işlevleri aşağıdaki makalelerle nasıl yazarak kullanacağınızı ve kullanacağınızı öğrenin:

* [Depolanan yordamlar, tetikleyiciler ve kullanıcı tanımlı işlevler nasıl yazılır?](how-to-write-stored-procedures-triggers-udfs.md)

* [Depolanan yordamlar, tetikleyiciler ve kullanıcı tanımlı işlevler nasıl kullanılır?](how-to-use-stored-procedures-triggers-udfs.md)

* [JavaScript dili tümleşik sorgu API ile çalışma](javascript-query-api.md)
