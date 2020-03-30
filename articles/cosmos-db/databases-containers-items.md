---
title: Azure Cosmos DB'deki veritabanları, kapsayıcılar ve öğelerle çalışın
description: Bu makalede, Azure Cosmos DB'de veritabanları, kapsayıcılar ve öğelerin nasıl oluşturulup kullanılacağı açıklanmaktadır.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/01/2019
ms.reviewer: sngun
ms.openlocfilehash: 43a842c3b6d6d421eca4196c7f3facc7876318cd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79246792"
---
# <a name="work-with-databases-containers-and-items-in-azure-cosmos-db"></a>Azure Cosmos DB'deki veritabanları, kapsayıcılar ve öğelerle çalışın

Azure aboneliğiniz altında bir [Azure Cosmos DB hesabı](account-overview.md) oluşturduktan sonra, veritabanları, kapsayıcılar ve öğeler oluşturarak hesabınızdaki verileri yönetebilirsiniz. Bu makalede bu varlıkların her biri açıklanır. 

Aşağıdaki resim, bir Azure Cosmos DB hesabındafarklı varlıkların hiyerarşisini gösterir:

![Azure Cosmos hesap varlıkları](./media/databases-containers-items/cosmos-entities.png)

## <a name="azure-cosmos-databases"></a>Azure Cosmos veritabanları

Hesabınız altında bir veya birden çok Azure Cosmos veritabanı oluşturabilirsiniz. Veritabanı, ad alanına benzer. Veritabanı, bir dizi Azure Cosmos kapsayıcısının yönetim birimidir. Aşağıdaki tablo, Azure Cosmos veritabanının API'ye özgü çeşitli varlıklarla nasıl eşlenerek eşlendirildigini gösterir:

| Azure Cosmos varlığı | SQL API’si | Cassandra API’si | MongoDB için Azure Cosmos DB API | Gremlin API | Tablo API’si |
| --- | --- | --- | --- | --- | --- |
|Azure Cosmos veritabanı | Database | Anahtar Alanı | Database | Database | NA |

> [!NOTE]
> Tablo API hesapları ile, ilk tablonuzu oluşturduğunuzda, Azure Cosmos hesabınızda otomatik olarak varsayılan bir veritabanı oluşturulur.

### <a name="operations-on-an-azure-cosmos-database"></a>Azure Cosmos veritabanındaki işlemler

Aşağıdaki tabloda açıklandığı gibi Azure Cosmos API'leri ile Azure Cosmos veritabanıyla etkileşim kurabilirsiniz:

| İşlem | Azure CLI | SQL API’si | Cassandra API’si | MongoDB için Azure Cosmos DB API | Gremlin API | Tablo API’si |
| --- | --- | --- | --- | --- | --- | --- |
|Tüm veritabanlarını sayısala| Evet | Evet | Evet (veritabanı bir anahtar alanına eşlenir) | Evet | NA | NA |
|Veritabanını okuma| Evet | Evet | Evet (veritabanı bir anahtar alanına eşlenir) | Evet | NA | NA |
|Yeni veritabanı oluşturma| Evet | Evet | Evet (veritabanı bir anahtar alanına eşlenir) | Evet | NA | NA |
|Veritabanını güncelleştirme| Evet | Evet | Evet (veritabanı bir anahtar alanına eşlenir) | Evet | NA | NA |


## <a name="azure-cosmos-containers"></a>Azure Cosmos kapsayıcıları

Azure Cosmos kapsayıcısı, hem sağlanan iş ortası hem de depolama için ölçeklenebilirlik birimidir. Kapsayıcı yatay olarak bölümlenir ve birden çok bölge arasında çoğaltılır. Kapsayıcıya eklediğiniz öğeler ve üzerinde sağlama yaptığınız iş bölümü, bölüm anahtarına dayalı bir dizi mantıksal bölüme otomatik olarak dağıtılır. Bölümleme ve bölüm tuşları hakkında daha fazla bilgi edinmek için [Bölüm verilerine](partition-data.md)bakın. 

Bir Azure Cosmos kapsayıcısı oluşturduğunuzda, iş ortasını aşağıdaki modlardan birinde yapılandırırsınız:

* **Özel tedarik edilen iş kolu modu**: Bir konteyner üzerinde sağlanan iş kolu münhasıran o kap için ayrılmıştır ve SLA'lar tarafından yedeklenir. Daha fazla bilgi edinmek için azure [cosmos kapsayıcısına nasıl iş ortası sağlayacağınızı](how-to-provision-container-throughput.md)öğrenin.

* **Paylaşılan tedarik edilen iş lenme modu**: Bu kapsayıcılar, sağlanan iş artışını aynı veritabanındaki diğer kapsayıcılarla paylaşır (özel tedarik edilen iş girdisi ile yapılandırılan kapsayıcılar hariç). Başka bir deyişle, veritabanında sağlanan iş paylaşımı tüm "paylaşılan iş" kapsayıcıları arasında paylaşılır. Daha fazla bilgi edinmek için azure [cosmos veritabanında iş ortası sağlama konusuna](how-to-provision-database-throughput.md)bakın.

> [!NOTE]
> Paylaşılan ve özel iş verisini yalnızca veritabanı ve kapsayıcı oluştururken yapılandırabilirsiniz. Kapsayıcı oluşturulduktan sonra ayrılmış aktarım hızı modundan paylaşılan aktarım hızı moduna (ve tam tersine) geçiş yapmak için yeni bir kapsayıcı oluşturup verileri yeni kapsayıcıya geçirmeniz gerekir. Azure Cosmos DB değiştirme akışı özelliğini kullanarak verileri geçirebilirsiniz.

Azure Cosmos kapsayıcısı, ister özel veya paylaşılan sağlanan iş ortası modları kullanarak kapsayıcılar oluştursanız da elastik olarak ölçeklenebilir.

Azure Cosmos kapsayıcısı, öğelerin şema-agnostik bir kapsayıcısIdur. Kapsayıcıdaki öğelerin keyfi şemaları olabilir. Örneğin, bir kişiyi temsil eden bir öğe ve otomobili temsil eden bir öğe *aynı kapsayıcıya*yerleştirilebilir. Varsayılan olarak, bir kapsayıcıya eklediğiniz tüm öğeler açık dizin veya şema yönetimi gerektirmeden otomatik olarak dizine eklenir. [Dizin oluşturma ilkesini](index-overview.md) bir kapsayıcıüzerinde yapılandırarak dizin oluşturma davranışını özelleştirebilirsiniz. 

Bir Azure Cosmos kapsayıcısındaki seçili öğelerde veya tüm kapsayıcı için bu öğeleri sistemden zarif bir şekilde temizlemek için [Time to Live (TTL)](time-to-live.md) ayarlayabilirsiniz. Azure Cosmos DB, süresi dolduğunda öğeleri otomatik olarak siler. Ayrıca, kapsayıcıda gerçekleştirilen bir sorgunun süresi dolan öğeleri sabit bir sınır içinde döndürmediğini de garanti eder. Daha fazla bilgi [için, bkz.](how-to-time-to-live.md)

Kapsayıcınızın her mantıksal bölümü için yönetilen işlem günlüğüne abone olmak için [değişiklik akışını](change-feed.md) kullanabilirsiniz. Değişiklik akışı, öğelerin önceki ve sonraki görüntüleriyle birlikte kapsayıcıda gerçekleştirilen tüm güncelleştirmelerin günlüğünü sağlar. Daha fazla bilgi için [bkz.](serverless-computing-database.md) Ayrıca, kapsayıcıdaki değişiklik akışı ilkesini kullanarak değişiklik akışı için bekletme süresini de yapılandırabilirsiniz. 

[Depolanan yordamları, tetikleyicileri, kullanıcı tanımlı işlevleri (UDF'leri)](stored-procedures-triggers-udfs.md)kaydedebilir ve Azure Cosmos kapsayıcınız için [birleştirme yordamlarını birleştirebilirsiniz.](how-to-manage-conflicts.md) 

Azure Cosmos kapsayıcınızda benzersiz bir [anahtar kısıtlaması](unique-keys.md) belirtebilirsiniz. Benzersiz bir anahtar ilkesi oluşturarak, mantıksal bölüm anahtarı başına bir veya daha fazla değerin benzersizliğini sağlarsınız. Benzersiz bir anahtar ilkesi kullanarak bir kapsayıcı oluşturursanız, benzersiz anahtar kısıtlaması tarafından belirtilen değerleri yinelenen değerlere sahip yeni veya güncelleştirilmiş öğeler oluşturulamaz. Daha fazla bilgi edinmek için [Benzersiz anahtar kısıtlamalarına](unique-keys.md)bakın.

Azure Cosmos kapsayıcısı, aşağıdaki tabloda gösterildiği gibi API'ye özgü varlıklarda özelleştirilmiştir:

| Azure Cosmos varlığı | SQL API’si | Cassandra API’si | MongoDB için Azure Cosmos DB API | Gremlin API | Tablo API’si |
| --- | --- | --- | --- | --- | --- |
|Azure Cosmos konteyneri | Kapsayıcı | Tablo | Koleksiyon | Graf | Tablo |

### <a name="properties-of-an-azure-cosmos-container"></a>Azure Cosmos kapsayıcısının özellikleri

Azure Cosmos kapsayıcısı sistem tanımlı bir dizi özelliye sahiptir. Kullandığınız API'ye bağlı olarak, bazı özellikler doğrudan açıkta olmayabilir. Aşağıdaki tabloda sistem tanımlı özelliklerin listesi açıklanmaktadır:

| Sistem tanımlı özellik | Sistem tarafından oluşturulan veya kullanıcı tarafından yapılandırılabilir | Amaç | SQL API’si | Cassandra API’si | MongoDB için Azure Cosmos DB API | Gremlin API | Tablo API’si |
| --- | --- | --- | --- | --- | --- | --- | --- |
|\_rid | Sistem tarafından oluşturulan | Konteynerin benzersiz tanımlayıcısı | Evet | Hayır | Hayır | Hayır | Hayır |
|\_Etag | Sistem tarafından oluşturulan | İyimser eşzamanlılık denetimi için kullanılan varlık etiketi | Evet | Hayır | Hayır | Hayır | Hayır |
|\_Ts | Sistem tarafından oluşturulan | Kapsayıcının son güncellenen zaman damgası | Evet | Hayır | Hayır | Hayır | Hayır |
|\_Kendini | Sistem tarafından oluşturulan | Konteynerin adreslenebilir URI | Evet | Hayır | Hayır | Hayır | Hayır |
|id | Kullanıcı tarafından yapılandırılabilir | Kapsayıcının kullanıcı tanımlı benzersiz adı | Evet | Evet | Evet | Evet | Evet |
|indexingPolitikası | Kullanıcı tarafından yapılandırılabilir | Dizin yolunu, dizin türünü ve dizin modunu değiştirme olanağı sağlar | Evet | Hayır | Hayır | Hayır | Evet |
|TimeToLive | Kullanıcı tarafından yapılandırılabilir | Belirli bir süre sonra öğeleri bir kapsayıcıdan otomatik olarak silme olanağı sağlar. Ayrıntılar için, [Yaşam Zamanı'na](time-to-live.md)bakın. | Evet | Hayır | Hayır | Hayır | Evet |
|changeFeedPolicy | Kullanıcı tarafından yapılandırılabilir | Bir kapsayıcıdaki öğelerde yapılan değişiklikleri okumak için kullanılır. Ayrıntılar için [bkz.](change-feed.md) | Evet | Hayır | Hayır | Hayır | Evet |
|uniqueKeyPolicy | Kullanıcı tarafından yapılandırılabilir | Mantıksal bir bölümdeki bir veya daha fazla değerin benzersizliğini sağlamak için kullanılır. Daha fazla bilgi için [Benzersiz anahtar kısıtlamalarına](unique-keys.md)bakın. | Evet | Hayır | Hayır | Hayır | Evet |

### <a name="operations-on-an-azure-cosmos-container"></a>Azure Cosmos kapsayıcısı üzerindeki işlemler

Azure Cosmos kapsayıcısı, Azure Cosmos API'lerinden herhangi birini kullandığınızda aşağıdaki işlemleri destekler:

| İşlem | Azure CLI | SQL API’si | Cassandra API’si | MongoDB için Azure Cosmos DB API | Gremlin API | Tablo API’si |
| --- | --- | --- | --- | --- | --- | --- |
| Veritabanındaki kapsayıcıları sayısallandırma | Evet | Evet | Evet | Evet | NA | NA |
| Bir kapsayıcıyı okuma | Evet | Evet | Evet | Evet | NA | NA |
| Yeni bir kapsayıcı oluşturma | Evet | Evet | Evet | Evet | NA | NA |
| Kapsayıcıyı güncelleştirme | Evet | Evet | Evet | Evet | NA | NA |
| Kapsayıcı silme | Evet | Evet | Evet | Evet | NA | NA |

## <a name="azure-cosmos-items"></a>Azure Cosmos öğeleri

Hangi API'yi kullandığınıza bağlı olarak, bir Azure Cosmos öğesi koleksiyondaki bir belgeyi, tablodaki bir satırı veya grafikteki düğümü veya kenarı temsil edebilir. Aşağıdaki tablo, API'ye özgü varlıkların bir Azure Cosmos öğesiyle eşlenemesini gösterir:

| Kozmos varlığı | SQL API’si | Cassandra API’si | MongoDB için Azure Cosmos DB API | Gremlin API | Tablo API’si |
| --- | --- | --- | --- | --- | --- |
|Azure Cosmos öğesi | Belge | Satır | Belge | Düğüm veya kenar | Öğe |

### <a name="properties-of-an-item"></a>Bir öğenin özellikleri

Her Azure Cosmos öğesi aşağıdaki sistem tanımlı özelliklere sahiptir. Kullandığınız API'ye bağlı olarak, bazıları doğrudan açıkta olmayabilir.

| Sistem tanımlı özellik | Sistem tarafından oluşturulan veya kullanıcı tarafından yapılandırılabilir| Amaç | SQL API’si | Cassandra API’si | MongoDB için Azure Cosmos DB API | Gremlin API | Tablo API’si |
| --- | --- | --- | --- | --- | --- | --- | --- |
|\_rid | Sistem tarafından oluşturulan | Öğenin benzersiz tanımlayıcısı | Evet | Hayır | Hayır | Hayır | Hayır |
|\_Etag | Sistem tarafından oluşturulan | İyimser eşzamanlılık denetimi için kullanılan varlık etiketi | Evet | Hayır | Hayır | Hayır | Hayır |
|\_Ts | Sistem tarafından oluşturulan | Öğenin son güncelleştirmesinin zaman damgası | Evet | Hayır | Hayır | Hayır | Hayır |
|\_Kendini | Sistem tarafından oluşturulan | Öğenin adreslenebilir URI | Evet | Hayır | Hayır | Hayır | Hayır |
|id | Ya | Mantıksal bir bölümde kullanıcı tanımlı benzersiz ad. | Evet | Evet | Evet | Evet | Evet |
|Rasgele kullanıcı tanımlı özellikler | Kullanıcı tanımlı | API yerel gösteriminde temsil edilen kullanıcı tanımlı özellikler (JSON, BSON ve CQL dahil) | Evet | Evet | Evet | Evet | Evet |

> [!NOTE]
> Özelliğin `id` benzersizliği yalnızca her mantıksal bölüm içinde zorlanır. Birden çok belge, `id` farklı bölme anahtar değerleriile aynı özelliğe sahip olabilir.

### <a name="operations-on-items"></a>Öğeler üzerindeki işlemler

Azure Cosmos öğeleri aşağıdaki işlemleri destekler. İşlemleri gerçekleştirmek için Azure Cosmos API'lerinden herhangi birini kullanabilirsiniz.

| İşlem | Azure CLI | SQL API’si | Cassandra API’si | MongoDB için Azure Cosmos DB API | Gremlin API | Tablo API’si |
| --- | --- | --- | --- | --- | --- | --- |
| Ekle, Değiştir, Sil, Yukarı, Oku | Hayır | Evet | Evet | Evet | Evet | Evet |

## <a name="next-steps"></a>Sonraki adımlar

Bu görevler ve kavramlar hakkında bilgi edinin:

* [Azure Cosmos veritabanında sağlama iş ortası](how-to-provision-database-throughput.md)
* [Azure Cosmos kapsayıcısı üzerinde sağlama iş ortası](how-to-provision-container-throughput.md)
* [Mantıksal bölümlerle çalışma](partition-data.md)
* [Azure Cosmos kapsayıcıda TTL yapılandırma](how-to-time-to-live.md)
* [Değişiklik akışı kullanarak reaktif uygulamalar oluşturun](change-feed.md)
* [Azure Cosmos kapsayıcınızda benzersiz bir anahtar kısıtlaması yapılandırın](unique-keys.md)
