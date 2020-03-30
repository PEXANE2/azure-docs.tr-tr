---
title: Azure Cosmos DB'de veritabanı hareketleri ve iyimser eşzamanlılık denetimi
description: Bu makalede, Azure Cosmos DB'deki veritabanı hareketleri ve iyimser eşzamanlılık denetimi açıklanmaktadır
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 12/04/2019
ms.reviewer: sngun
ms.openlocfilehash: d453bb4071c4a6972e01b8f7e90375181caf6d01
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74806533"
---
# <a name="transactions-and-optimistic-concurrency-control"></a>İşlemler ve iyimser eşzamanlılık denetimi

Veritabanı işlemleri, verilerdeki eşzamanlı değişikliklerle başa çıkmak için güvenli ve öngörülebilir bir programlama modeli sağlar. SQL Server gibi geleneksel ilişkisel veritabanları, depolanmış yordamlar ve/veya tetikleyicileri kullanarak iş mantığını yazmanızı ve doğrudan veritabanı altyapısı içinde yürütülmesi için sunucuya göndermenize olanak sağlar. Geleneksel ilişkisel veritabanları ile, javascript, Python, C#, Java, vb. gibi iki farklı programlama dili (işlem dışı) uygulama programlama dili ve veritabanı tarafından yerel olarak yürütülen işlem programlama dili (T-SQL gibi) ile ilgilenmeniz gerekir.

Azure Cosmos DB'deki veritabanı altyapısı, anlık görüntü yalıtımıyla tam ACID (Atomiklik, Tutarlılık, Yalıtım, Dayanıklılık) uyumlu işlemleri destekler. Bir kapsayıcının [mantıksal bölümü](partition-data.md) kapsamındaki tüm veritabanı işlemleri, bölümün yinelemesi tarafından barındırılan veritabanı altyapısı içinde işlemsel olarak yürütülür. Bu işlemler hem yazma (mantıksal bölüm içinde bir veya daha fazla öğegüncelleştirme) ve okuma işlemleri içerir. Aşağıdaki tabloda farklı işlemler ve işlem türleri gösterilebilir:

| **İşlem**  | **İşlem Türü** | **Tek veya Çok Maddeli İşlem** |
|---------|---------|---------|
| Ekleme (ön/sonrası tetikleyicisi olmadan) | Yazma | Tek madde hareketi |
| Ekleme (ön/sonrası tetikleyiciyle) | Yazma ve Okuma | Çok maddeli hareket |
| Değiştir (ön/sonrası tetikleyici olmadan) | Yazma | Tek madde hareketi |
| Değiştir (ön/sonrası tetikleyiciyle) | Yazma ve Okuma | Çok maddeli hareket |
| Yükseltme (ön/sonrası tetikleyiciolmadan) | Yazma | Tek madde hareketi |
| Yukarı sertle (ön/sonrası tetikleyiciyle) | Yazma ve Okuma | Çok maddeli hareket |
| Silme (ön/posta tetikleyicisi olmadan) | Yazma | Tek madde hareketi |
| Silme (ön/posta tetikleyicisi ile) | Yazma ve Okuma | Çok maddeli hareket |
| Depolanan yordamı çalıştırma | Yazma ve Okuma | Çok maddeli hareket |
| Sistem birleştirme yordamının yürütülmesini başlattı | Yazma | Çok maddeli hareket |
| Bir maddenin son kullanma süresine (TTL) göre maddelerin silme işlemini başlatan sistem | Yazma | Çok maddeli hareket |
| Okuma | Okuma | Tek maddeli hareket |
| Değişiklik Akışı | Okuma | Çok maddeli hareket |
| Paginated Okuma | Okuma | Çok maddeli hareket |
| Paginated Sorgu | Okuma | Çok maddeli hareket |
| Paginated sorgusunun bir parçası olarak UDF'yi çalıştırma | Okuma | Çok maddeli hareket |

## <a name="multi-item-transactions"></a>Çok maddeli hareketler

Azure Cosmos DB, [JavaScript'te depolanmış yordamlar, ön/sonrası tetikleyiciler, kullanıcı tanımlı işlevler (UDF' ler)](stored-procedures-triggers-udfs.md) ve birleştirme yordamları yazmanıza olanak tanır. Azure Cosmos DB, veritabanı altyapısı içinde JavaScript yürütmesini doğal olarak destekler. Depolanan yordamları, ön/posta tetikleyicilerini, kullanıcı tanımlı işlevleri (UDF'leri) kaydedebilir ve yordamları bir kapsayıcıda birleştirebilir ve daha sonra Bunları Azure Cosmos veritabanı altyapısında işlem seli olarak yürütebilirsiniz. JavaScript'te uygulama mantığının yazılması, denetim akışının, değişken kapsamlandırmanın, atamanın ve istisna işleme nin veritabanı işlemleri içindeki ilkel işlemlerinin doğrudan JavaScript dilinde tümleştirilmesine olanak tanır.

JavaScript tabanlı depolanan yordamlar, tetikleyiciler, UDF'ler ve birleştirme yordamları, mantıksal bölüm içindeki tüm öğeler arasında anlık görüntü yalıtımı ile ortam ASİt hareketi içinde sarılır. Yürütme sırasında, JavaScript programı bir özel durum atarsa, tüm işlem iptal edilir ve geri alınır. Ortaya çıkan programlama modeli basit ama güçlü. JavaScript geliştiricileri hala tanıdık dil yapıları ve kütüphane ilkel kullanırken dayanıklı bir programlama modeli olsun.

JavaScript'i doğrudan veritabanı altyapısı içinde yürütebilme özelliği, veritabanı işlemlerinin bir kapsayıcının öğelerine karşı performans ve işlem yürütmesini sağlar. Ayrıca, Azure Cosmos veritabanı altyapısı JSON ve JavaScript'i doğal olarak desteklediğinden, uygulamanın türü sistemleri yle veritabanı arasında empedans uyuşmazlığı yoktur.

## <a name="optimistic-concurrency-control"></a>İyimser eşzamanlılık kontrolü

İyimser eşzamanlılık denetimi, kayıp güncelleştirmeleri ve silmeleri önlemenize olanak tanır. Eşzamanlı, çakışan işlemler, öğenin sahibi olan mantıksal bölüm tarafından barındırılan veritabanı altyapısının düzenli kötümser kilitlemetabi tutulur. İki eşzamanlı işlem mantıksal bir bölüm içinde bir öğenin en son sürümünü güncelleştirmeye çalıştığınızda, bunlardan biri kazanır ve diğeri başarısız olur. Ancak, aynı öğeyi aynı anda güncelleştirmeye çalışan bir veya iki işlem öğenin daha önce eski bir değerini okumuşsa, veritabanı daha önce çakışan işlemlertarafından veya her ikisi tarafından daha önce okunan değerin gerçekten öğenin en son değeri olup olmadığını bilmez. Neyse ki, bu durum, iki işlemin veritabanı altyapısının içindeki işlem sınırına girmesine izin vermeden önce **İyimser Eşzamanlılık Denetimi (OCC)** ile algılanabilir. OCC, verilerinizi başkaları tarafından yapılan değişiklikleri yanlışlıkla üzerine yazmaktan korur. Ayrıca, başkalarının yanlışlıkla kendi değişikliklerinizi üzerine yazmalarını da engeller.

Bir öğenin eşzamanlı güncelleştirmeleri, Azure Cosmos DB'nin iletişim protokolü katmanı tarafından OCC'ye tabi tutulur. Azure Cosmos veritabanı, güncellediğiniz (veya silediğiniz) öğenin istemci tarafındaki sürümünün Azure Cosmos kapsayıcısındaki öğenin sürümüyle aynı olmasını sağlar. Bu, yazmalarınızın başkalarının yazıları tarafından yanlışlıkla üzerine yazılmasından korunduğunu garanti eder ve bunun tersi de önemlidir. Çok kullanıcılı bir ortamda, iyimser eşzamanlılık denetimi bir öğenin yanlışlıkla yanlış sürümünü silmesinden veya güncellenmesinden sizi korur. Bu nedenle, öğeler rezil "kayıp güncelleştirme" veya "kayıp silme" sorunlarına karşı korunur.

Azure Cosmos kapsayıcısında depolanan her öğenin sistem tanımlı `_etag` bir özelliği vardır. Öğe her `_etag` güncelleştirildiğinde, öğenin değeri sunucu tarafından otomatik olarak oluşturulur ve güncelleştirilir. `_etag`sunucunun bir öğenin `if-match` koşullu olarak güncelleştirilip güncelleştirilemeyeceğine karar vermesine izin vermek için istemci tarafından sağlanan istek üstbilgisiyle kullanılabilir. `if-match` Üstbilginin değeri `_etag` sunucudaki değeriyle eşleşirse, öğe güncelleştirilir. `if-match` İstek üstbilgisinin değeri artık geçerli değilse, sunucu işlemi "HTTP 412 Ön Koşul hatası" yanıt iletisiyle reddeder. İstemci daha sonra öğeyi sunucudaki geçerli sürümünü elde etmek için öğeyi yeniden alabilir veya `_etag` öğe için kendi değeri olan sunucudaki öğesürümünü geçersiz kılabilir. Buna ek `_etag` olarak, bir `if-none-match` kaynağın yeniden alınması gerekip gerekmediğini belirlemek için üstbilgi ile kullanılabilir.

Öğe her `_etag` güncelleştiride maddenin değeri değişir. Madde yi değiştirme `if-match` işlemleri için, istek seçeneklerinin bir parçası olarak açıkça ifade edilmelidir. Örneğin, [GitHub'daki](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/ItemManagement/Program.cs#L578-L674)örnek kodu görün. `_etag`değerler, depolanan yordamtarafından dokunulan tüm yazılı öğeler için örtülü olarak denetlenir. Herhangi bir çakışma algılanırsa, depolanan yordam hareketi geri alır ve bir özel durum atar. Bu yöntemle, depolanan yordam içinde tüm veya hiç yazmaları atomik olarak uygulanır. Bu, güncelleştirmeleri yeniden uygulamak ve özgün istemci isteğini yeniden denemek için uygulamaya bir sinyaldir.

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makalelerde veritabanı hareketleri ve iyimser eşzamanlılık denetimi hakkında daha fazla bilgi edinin:

- [Azure Cosmos veritabanları, kapsayıcılar ve öğelerle çalışma](databases-containers-items.md)
- [Tutarlılık düzeyleri](consistency-levels.md)
- [Çakışma türleri ve çözme ilkeleri](conflict-resolution-policies.md)
- [Depolanan yordamlar, tetikleyiciler ve kullanıcı tanımlı işlevler](stored-procedures-triggers-udfs.md)
