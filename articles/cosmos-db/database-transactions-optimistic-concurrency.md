---
title: Azure Cosmos DB 'de veritabanı işlemleri ve iyimser eşzamanlılık denetimi
description: Bu makalede, Azure Cosmos DB veritabanı işlemleri ve iyimser eşzamanlılık denetimi açıklanmaktadır
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 12/04/2019
ms.reviewer: sngun
ms.openlocfilehash: d453bb4071c4a6972e01b8f7e90375181caf6d01
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "74806533"
---
# <a name="transactions-and-optimistic-concurrency-control"></a>İşlemler ve iyimser eşzamanlılık denetimi

Veritabanı işlemleri, verilerdeki eşzamanlı değişikliklerle başa çıkmak için güvenli ve öngörülebilir bir programlama modeli sağlar. SQL Server gibi geleneksel ilişkisel veritabanları, depolanan yordamları ve/veya Tetikleyicileri kullanarak iş mantığını yazmanızı sağlar, bu dosyayı doğrudan veritabanı altyapısının içinde yürütülmek üzere sunucuya gönderir. Geleneksel ilişkisel veritabanlarında, JavaScript, Python, C#, Java, vb. gibi iki farklı programlama dili ve veritabanı tarafından yerel olarak yürütülen işlem programlama dili (T-SQL) ile uğraşmanız gerekir.

Azure Cosmos DB veritabanı altyapısı, anlık görüntü yalıtımıyla tam ACID (Atomicity, tutarlılık, yalıtım, dayanıklılık) uyumlu işlemleri destekler. Bir kapsayıcının [mantıksal bölümünün](partition-data.md) kapsamındaki tüm veritabanı işlemleri, Bölüm çoğaltması tarafından barındırılan veritabanı altyapısında işlem içinde yürütülür. Bu işlemler hem yazma (mantıksal bölümün içindeki bir veya daha fazla öğeyi güncelleştirme) hem de okuma işlemlerini içerir. Aşağıdaki tabloda farklı işlemler ve işlem türleri gösterilmektedir:

| **İşlem**  | **İşlem türü** | **Tek veya çoklu öğe Işlemi** |
|---------|---------|---------|
| Ekle (ön/son tetikleyici olmadan) | Yazma | Tek öğe işlemi |
| Insert (bir ön/son tetikleyici ile) | Yazma ve okuma | Çoklu öğe işlemi |
| Değiştir (ön/son tetikleyici olmadan) | Yazma | Tek öğe işlemi |
| Değiştir (bir ön/son tetikleyici ile) | Yazma ve okuma | Çoklu öğe işlemi |
| Upsert (ön/son tetikleyici olmadan) | Yazma | Tek öğe işlemi |
| Upsert (bir ön/son tetikleyici ile) | Yazma ve okuma | Çoklu öğe işlemi |
| Sil (ön/son tetikleyici olmadan) | Yazma | Tek öğe işlemi |
| Sil (bir ön/son tetikleyici ile) | Yazma ve okuma | Çoklu öğe işlemi |
| Saklı yordamı Yürüt | Yazma ve okuma | Çoklu öğe işlemi |
| Sistemin bir birleştirme yordamının yürütülmesi başlatıldı | Yazma | Çoklu öğe işlemi |
| Bir öğenin süre sonu (TTL) temelinde öğelerin silinmesinin sistem tarafından başlatılması | Yazma | Çoklu öğe işlemi |
| Okuma | Okuma | Tek öğe işlem |
| Değişiklik Akışı | Okuma | Çoklu öğe işlemi |
| Sayfalandırılmış okuma | Okuma | Çoklu öğe işlemi |
| Sayfalandırılmış sorgu | Okuma | Çoklu öğe işlemi |
| Sayfalandırılmış sorgunun bir parçası olarak UDF yürütün | Okuma | Çoklu öğe işlemi |

## <a name="multi-item-transactions"></a>Çoklu öğe işlemleri

Azure Cosmos DB, JavaScript 'te [saklı yordamlar, ön ve son Tetikleyiciler, Kullanıcı tanımlı işlevler (UDF 'ler)](stored-procedures-triggers-udfs.md) ve birleştirme yordamları yazmanızı sağlar. Azure Cosmos DB, veritabanı altyapısının içinde JavaScript yürütmesini yerel olarak destekler. Saklı yordamları, ön ve son Tetikleyicileri, Kullanıcı tanımlı işlevleri (UDF 'ler) ve birleştirme yordamlarını bir kapsayıcı üzerinde kaydedebilir ve daha sonra bunları Azure Cosmos veritabanı altyapısı içinde işlem temelli olarak yürütebilirsiniz. JavaScript 'e uygulama mantığı yazmak, denetim akışının doğal ifadesine, değişken kapsamı, atamaya ve veritabanı işlemlerinde doğrudan JavaScript dilinde özel durum işleme temelleri tümleştirilmesine olanak tanır.

JavaScript tabanlı saklı yordamlar, Tetikleyiciler, UDF 'ler ve birleştirme yordamları, mantıksal bölümün içindeki tüm öğelerde anlık görüntü yalıtımıyla bir çevresel ACID işlemi içinde sarmalanır. Yürütülmesi sırasında, JavaScript programı bir özel durum oluşturursa tüm işlem durdurulur ve geri alınır. Ortaya çıkan programlama modeli basit ancak güçlü bir işlemdir. JavaScript geliştiricileri, bildiğiniz dil yapılarını ve kitaplık temel öğelerini kullanmaya devam ederken dayanıklı bir programlama modeli alırlar.

Doğrudan veritabanı altyapısının içinde JavaScript yürütme özelliği, bir kapsayıcının öğelerine karşı veritabanı işlemlerinin performans ve işlem yürütme işlemlerini sağlar. Ayrıca, Azure Cosmos veritabanı altyapısı JSON ve JavaScript 'i yerel olarak desteklediğinden, bir uygulamanın tür sistemleri ve veritabanı arasında kesin bir uyumsuzluk yoktur.

## <a name="optimistic-concurrency-control"></a>İyimser eşzamanlılık denetimi

İyimser eşzamanlılık denetimi, kayıp güncelleştirmeleri ve silmeleri engellemenizi sağlar. Eşzamanlı, çakışan işlemler, öğenin sahibi olan mantıksal bölüm tarafından barındırılan veritabanı altyapısının normal kötümser kilitlemesinde tabi. İki eşzamanlı işlem, bir mantıksal bölüm içindeki bir öğenin en son sürümünü güncelleştirmeye çalıştığında, bunlardan biri kazanıyacaktır ve diğeri başarısız olur. Bununla birlikte, aynı öğeyi aynı anda güncelleştirmeye çalışan bir veya iki işlem daha önce öğenin daha eski bir değerini okuseydi, veritabanı daha önce veya her iki çakışan işlemin öğenin en son değeri olan veya her ikisi tarafından daha önce okunan değerin ne olduğunu bilmez. Neyse ki, bu durum **Iyimser eşzamanlılık denetimi (OCC)** ile birlikte algılanarak iki işlemin veritabanı altyapısının içinde işlem sınırını girmelerini önlenebilir. OCC, verilerinizi yanlışlıkla başkalarının yaptığı değişikliklerin üzerine yazmalarını önler. Ayrıca başkalarının kendi değişikliklerinizi yanlışlıkla üzerine yazmasını engeller.

Bir öğenin eşzamanlı güncelleştirmeleri, Azure Cosmos DB 'in iletişim protokolü katmanına göre OCC ile tabi. Azure Cosmos veritabanı, güncelleştirdiğiniz (veya sildiğiniz) öğenin istemci tarafı sürümünün Azure Cosmos kapsayıcısındaki öğenin sürümüyle aynı olmasını sağlar. Bu, yazma işlemlerinin yanlışlıkla başkalarının üzerine yazılmasına karşı korunmasını sağlar ve tam tersi de geçerlidir. Çok kullanıcılı bir ortamda, iyimser eşzamanlılık denetimi bir öğenin yanlış sürümünü yanlışlıkla silmenizi veya güncelleştirmenizi önler. Bu nedenle, öğeler "kayıp güncelleştirme" veya "kayıp silme" sorunlarına karşı korunur.

Azure Cosmos kapsayıcısında depolanan her öğenin sistem tarafından tanımlanan bir özelliği vardır `_etag` . Öğesinin değeri, `_etag` öğe her güncelleştirildiği zaman otomatik olarak oluşturulur ve sunucu tarafından güncelleştirilir. `_etag``if-match`, sunucunun bir öğenin koşullu olarak güncelleştirilip güncelleştirilmediğini istemediğinize karar vermesini sağlamak için istemci tarafından sağlanan istek üst bilgisi ile birlikte kullanılabilir. Üstbilginin değeri, `if-match` sunucudaki değeri ile eşleşir `_etag` , öğe daha sonra güncelleştirilir. `if-match`İstek üstbilgisinin değeri artık geçerli değilse, sunucu işlemi "HTTP 412 Önkoşul hatası" Yanıt iletisiyle reddeder. İstemci daha sonra öğenin geçerli sürümünü elde etmek için öğeyi yeniden alabilir veya öğe için kendi değeri ile sunucudaki öğenin sürümünü geçersiz kılabilir `_etag` . Ayrıca, `_etag` `if-none-match` bir kaynağın tekrar al gerekip gerekmediğini öğrenmek için üst bilgiyle birlikte kullanılabilir.

Öğenin değeri, `_etag` öğe her güncellenmesinde değişir. Öğe değiştirme işlemleri için, `if-match` istek seçeneklerinin bir parçası olarak açıkça ifade edilmesi gerekir. Örnek için [GitHub](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/Usage/ItemManagement/Program.cs#L578-L674)'daki örnek koda bakın. `_etag`değerler, saklı yordamın dokunmasıyla tüm yazılan öğeler için örtülü olarak denetlenir. Herhangi bir çakışma algılanırsa, saklı yordam işlemi geri alacak ve bir özel durum oluşturur. Bu yöntemle, saklı yordam içinde tüm veya yazma işlemleri otomatik olarak uygulanır. Bu, güncelleştirmeleri yeniden uygulamak ve özgün istemci isteğini yeniden denemek için uygulamaya yönelik bir sinyaldir.

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makalelerde veritabanı işlemleri ve iyimser eşzamanlılık denetimi hakkında daha fazla bilgi edinin:

- [Azure Cosmos veritabanları, kapsayıcılar ve öğeleriyle çalışma](databases-containers-items.md)
- [Tutarlılık düzeyleri](consistency-levels.md)
- [Çakışma türleri ve çözme ilkeleri](conflict-resolution-policies.md)
- [Saklı yordamlar, Tetikleyiciler ve Kullanıcı tanımlı işlevler](stored-procedures-triggers-udfs.md)
