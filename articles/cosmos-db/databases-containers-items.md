---
title: Azure Cosmos DB veritabanları, kapsayıcılar ve öğelerle çalışma
description: Bu makalede, Azure Cosmos DB veritabanlarının, kapsayıcıların ve öğelerin nasıl oluşturulacağı ve kullanılacağı açıklanmaktadır.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/01/2019
ms.reviewer: sngun
ms.openlocfilehash: 43a842c3b6d6d421eca4196c7f3facc7876318cd
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79246792"
---
# <a name="work-with-databases-containers-and-items-in-azure-cosmos-db"></a>Azure Cosmos DB veritabanları, kapsayıcılar ve öğelerle çalışma

Azure aboneliğinizde bir [Azure Cosmos DB hesabı](account-overview.md) oluşturduktan sonra veritabanları, kapsayıcılar ve öğeler oluşturarak hesabınızdaki verileri yönetebilirsiniz. Bu makalede bu varlıkların her biri açıklanır. 

Aşağıdaki görüntüde Azure Cosmos DB hesabındaki farklı varlıkların hiyerarşisi gösterilmektedir:

![Azure Cosmos hesabı varlıklar](./media/databases-containers-items/cosmos-entities.png)

## <a name="azure-cosmos-databases"></a>Azure Cosmos veritabanları

Hesabınızda bir veya daha fazla Azure Cosmos veritabanı oluşturabilirsiniz. Bir veritabanı bir ad alanına benzer. Veritabanı, bir dizi Azure Cosmos kapsayıcısı için yönetim birimidir. Aşağıdaki tablo, bir Azure Cosmos veritabanı çeşitli API özel varlıklara nasıl eşleştiğini gösterir:

| Azure Cosmos varlığı | SQL API’si | Cassandra API’si | MongoDB için Azure Cosmos DB API | Gremlin API | Tablo API’si |
| --- | --- | --- | --- | --- | --- |
|Azure Cosmos veritabanı | Database | Keyspace | Database | Database | NA |

> [!NOTE]
> Tablo API'si hesapları ile ilk tablonuzu oluşturduğunuzda, Azure Cosmos hesabınızda otomatik olarak varsayılan bir veritabanı oluşturulur.

### <a name="operations-on-an-azure-cosmos-database"></a>Bir Azure Cosmos veritabanı üzerinde işlemler

Aşağıdaki tabloda açıklandığı gibi Azure Cosmos API 'si ile Azure Cosmos veritabanıyla etkileşime geçebilirsiniz:

| İşlem | Azure CLI | SQL API’si | Cassandra API’si | MongoDB için Azure Cosmos DB API | Gremlin API | Tablo API’si |
| --- | --- | --- | --- | --- | --- | --- |
|Tüm veritabanları listeleme| Yes | Yes | Evet (veritabanı için bir anahtar alanı eşlendi) | Yes | NA | NA |
|Okuma veritabanı| Yes | Yes | Evet (veritabanı için bir anahtar alanı eşlendi) | Yes | NA | NA |
|Yeni veritabanı oluştur| Yes | Yes | Evet (veritabanı için bir anahtar alanı eşlendi) | Yes | NA | NA |
|Veritabanını Güncelleştir| Yes | Yes | Evet (veritabanı için bir anahtar alanı eşlendi) | Yes | NA | NA |


## <a name="azure-cosmos-containers"></a>Azure Cosmos kapsayıcıları

Azure Cosmos kapsayıcısı, sağlanan aktarım hızı ve depolama için ölçeklenebilirlik birimidir. Bir kapsayıcı yatay olarak bölümlenir ve ardından birden çok bölgeye çoğaltılır. Kapsayıcıya eklediğiniz öğeler ve üzerinde sağladığınız aktarım hızı, Bölüm anahtarını temel alan bir mantıksal bölümler kümesi üzerinden otomatik olarak dağıtılır. Bölümlendirme ve bölüm anahtarları hakkında daha fazla bilgi için bkz. [bölüm verileri](partition-data.md). 

Bir Azure Cosmos kapsayıcısı oluşturduğunuzda, aşağıdaki modlardan birinde üretilen işi yapılandırın:

* **Adanmış sağlanan aktarım hızı modu**: bir kapsayıcıda sağlanan aktarım hızı, bu kapsayıcı için özel olarak ayrılmıştır ve SLA 'lar tarafından desteklenir. Daha fazla bilgi edinmek için bkz. [Azure Cosmos kapsayıcısında üretilen iş sağlama](how-to-provision-container-throughput.md).

* **Paylaşılan sağlanan aktarım hızı modu**: Bu kapsayıcılar, sağlanan aktarım hızını aynı veritabanındaki diğer kapsayıcılarla paylaşır (adanmış sağlanan aktarım hızı ile yapılandırılmış kapsayıcılar hariç). Diğer bir deyişle, veritabanında sağlanan aktarım hızı "paylaşılan verimlilik" kapsayıcıları arasında paylaşılır. Daha fazla bilgi edinmek için bkz. [Azure Cosmos veritabanında üretilen iş sağlama](how-to-provision-database-throughput.md).

> [!NOTE]
> Yalnızca veritabanı ve kapsayıcı oluştururken paylaşılan ve ayrılmış aktarım hızını yapılandırabilirsiniz. Kapsayıcı oluşturulduktan sonra ayrılmış aktarım hızı modundan paylaşılan aktarım hızı moduna (ve tam tersine) geçiş yapmak için yeni bir kapsayıcı oluşturup verileri yeni kapsayıcıya geçirmeniz gerekir. Azure Cosmos DB akışı Değiştir özelliğini kullanarak verileri geçirebilirsiniz.

Bir Azure Cosmos kapsayıcısı, özel veya paylaşılan sağlanmış verimlilik modlarını kullanarak kapsayıcılar oluşturup esnek ölçeklendirebilirler.

Bir Azure Cosmos kapsayıcı öğeleri şemadan kapsayıcıdır. Bir kapsayıcıdaki öğelerin rastgele şemaları olabilir. Örneğin, bir kişiyi temsil eden bir öğe ve bir otomobil öğesini temsil eden bir öğe *aynı kapsayıcıya*yerleştirilebilir. Varsayılan olarak, bir kapsayıcıya eklediğiniz tüm öğeler açık dizin veya şema yönetimine gerek kalmadan otomatik olarak dizinlenir. Bir kapsayıcıda [Dizin oluşturma ilkesini](index-overview.md) yapılandırarak dizin oluşturma davranışını özelleştirebilirsiniz. 

Bir Azure Cosmos kapsayıcısındaki seçili öğelerde [yaşam süresi (TTL)](time-to-live.md) ayarlayabilir veya tüm kapsayıcının bu öğeleri sistemden düzgün bir şekilde temizlemesini sağlayabilirsiniz. Azure Cosmos DB süreleri dolduğunda öğeleri otomatik olarak siler. Ayrıca, kapsayıcıda gerçekleştirilen bir sorgunun, süresi geçen öğelerin sabit bir bağlı olarak döndürülmeyeceğini güvence altına alır. Daha fazla bilgi için bkz. [KAPSAYıCıDA TTL yapılandırma](how-to-time-to-live.md).

[Değişiklik akışını](change-feed.md) , kapsayıcının her bir mantıksal bölümü için yönetilen işlemler günlüğüne abone olmak için kullanabilirsiniz. Değişiklik akışı, kapsayıcıda gerçekleştirilen tüm güncelleştirmelerin günlüğünü, öğelerin önceki ve sonraki görüntülerinden birlikte sağlar. Daha fazla bilgi için bkz. [değişiklik akışını kullanarak reaktif uygulamalar oluşturma](serverless-computing-database.md). Değişiklik akışı için saklama süresini, kapsayıcıda değişiklik akışı ilkesini kullanarak da yapılandırabilirsiniz. 

Azure Cosmos Kapsayıcınız için [saklı yordamları, Tetikleyicileri, Kullanıcı tanımlı işlevleri (UDF 'ler)](stored-procedures-triggers-udfs.md)ve [birleştirme yordamlarını](how-to-manage-conflicts.md) kaydedebilirsiniz. 

Azure Cosmos kapsayıcıda [benzersiz bir anahtar kısıtlaması](unique-keys.md) belirtebilirsiniz. Benzersiz anahtar bir ilke oluşturarak, mantıksal bölüm anahtarı başına bir veya daha fazla değerlerin benzersiz olmasını sağlamak. Benzersiz anahtar ilkesi kullanarak bir kapsayıcı oluşturursanız, benzersiz anahtar kısıtlaması tarafından belirtilen değerleri yineleyen yeni veya güncelleştirilmiş bir öğe oluşturulabilir. Daha fazla bilgi için bkz. [benzersiz anahtar kısıtlamaları](unique-keys.md).

Aşağıdaki tabloda gösterildiği gibi, bir Azure Cosmos kapsayıcısı, API 'ye özgü varlıklar için özelleştirilmiştir:

| Azure Cosmos varlığı | SQL API’si | Cassandra API’si | MongoDB için Azure Cosmos DB API | Gremlin API | Tablo API’si |
| --- | --- | --- | --- | --- | --- |
|Azure Cosmos kapsayıcı | Kapsayıcı | Tablo | Koleksiyon | Graf | Tablo |

### <a name="properties-of-an-azure-cosmos-container"></a>Bir Azure Cosmos kapsayıcı özellikleri

Bir Azure Cosmos kapsayıcısının sistem tarafından tanımlanan özellikler kümesi vardır. Kullandığınız API 'ye bağlı olarak, bazı özellikler doğrudan sunulmayabilir. Aşağıdaki tabloda sistem tarafından tanımlanan özelliklerin listesi açıklanmaktadır:

| Sistem tanımlı özellik | Sistem tarafından oluşturulan veya Kullanıcı tarafından yapılandırılabilen | Amaç | SQL API’si | Cassandra API’si | MongoDB için Azure Cosmos DB API | Gremlin API | Tablo API’si |
| --- | --- | --- | --- | --- | --- | --- | --- |
|\_RID | Sistem tarafından oluşturulan | Kapsayıcı benzersiz tanıtıcısı | Yes | Hayır | Hayır | Hayır | Hayır |
|\_ETag | Sistem tarafından oluşturulan | İyimser eşzamanlılık denetimi için kullanılan varlık etiketi | Yes | Hayır | Hayır | Hayır | Hayır |
|\_TS | Sistem tarafından oluşturulan | Kapsayıcının son güncelleştirilen zaman damgası | Yes | Hayır | Hayır | Hayır | Hayır |
|\_kendine | Sistem tarafından oluşturulan | Kapsayıcının adreslenebilir URI'si | Yes | Hayır | Hayır | Hayır | Hayır |
|id | Kullanıcı tarafından yapılandırılabilir | Kullanıcı tanımlı kapsayıcının benzersiz adı | Yes | Yes | Yes | Yes | Yes |
|indexingPolicy | Kullanıcı tarafından yapılandırılabilir | Dizin yolunu, dizin türünü ve Dizin modunu değiştirme olanağı sağlar | Yes | Hayır | Hayır | Hayır | Yes |
|TimeToLive | Kullanıcı tarafından yapılandırılabilir | Bir zaman aralığı sonrasında bir kapsayıcıdan otomatik olarak öğe silme yeteneği sağlar. Ayrıntılar için bkz. [yaşam süresi](time-to-live.md). | Yes | Hayır | Hayır | Hayır | Yes |
|changeFeedPolicy | Kullanıcı tarafından yapılandırılabilir | Bir kapsayıcı içindeki öğelerde yapılan değişiklikleri okumak için kullanılır. Ayrıntılar için bkz. [akışı değiştirme](change-feed.md). | Yes | Hayır | Hayır | Hayır | Yes |
|uniqueKeyPolicy | Kullanıcı tarafından yapılandırılabilir | Mantıksal bir bölümdeki bir veya daha fazla değerin benzersizliğini sağlamak için kullanılır. Daha fazla bilgi için bkz. [benzersiz anahtar kısıtlamaları](unique-keys.md). | Yes | Hayır | Hayır | Hayır | Yes |

### <a name="operations-on-an-azure-cosmos-container"></a>Bir Azure Cosmos kapsayıcısı üzerinde işlemler

Azure Cosmos kapsayıcısı, Azure Cosmos API 'Lerinden herhangi birini kullandığınızda aşağıdaki işlemleri destekler:

| İşlem | Azure CLI | SQL API’si | Cassandra API’si | MongoDB için Azure Cosmos DB API | Gremlin API | Tablo API’si |
| --- | --- | --- | --- | --- | --- | --- |
| Bir veritabanı kapsayıcılarda listeleme | Yes | Yes | Yes | Yes | NA | NA |
| Bir kapsayıcı okuyun | Yes | Yes | Yes | Yes | NA | NA |
| Yeni kapsayıcı oluştur | Yes | Yes | Yes | Yes | NA | NA |
| Kapsayıcıyı güncelleştirme | Yes | Yes | Yes | Yes | NA | NA |
| Kapsayıcı silme | Yes | Yes | Yes | Yes | NA | NA |

## <a name="azure-cosmos-items"></a>Azure Cosmos öğeleri

Kullandığınız API 'ye bağlı olarak, bir Azure Cosmos öğesi koleksiyondaki bir belgeyi, tablodaki bir satırı veya bir grafikteki bir düğümü ya da kenarı temsil edebilir. Aşağıdaki tabloda, bir Azure Cosmos öğesine yönelik API 'ye özgü varlıkların eşleştirmesi gösterilmektedir:

| Cosmos varlığı | SQL API’si | Cassandra API’si | MongoDB için Azure Cosmos DB API | Gremlin API | Tablo API’si |
| --- | --- | --- | --- | --- | --- |
|Azure Cosmos öğesi | Belge | Satır | Belge | Düğüm veya kenar | Öğe |

### <a name="properties-of-an-item"></a>Bir öğenin özellikleri

Her Azure Cosmos öğesi, sistem tarafından tanımlanan aşağıdaki özelliklere sahiptir. Kullandığınız API 'ye bağlı olarak, bunlardan bazıları doğrudan sunulmayabilir.

| Sistem tanımlı özellik | Sistem tarafından oluşturulan veya Kullanıcı tarafından yapılandırılabilen| Amaç | SQL API’si | Cassandra API’si | MongoDB için Azure Cosmos DB API | Gremlin API | Tablo API’si |
| --- | --- | --- | --- | --- | --- | --- | --- |
|\_RID | Sistem tarafından oluşturulan | Öğenin benzersiz tanımlayıcısı | Yes | Hayır | Hayır | Hayır | Hayır |
|\_ETag | Sistem tarafından oluşturulan | İyimser eşzamanlılık denetimi için kullanılan varlık etiketi | Yes | Hayır | Hayır | Hayır | Hayır |
|\_TS | Sistem tarafından oluşturulan | Öğenin son güncelleştirme zaman damgası | Yes | Hayır | Hayır | Hayır | Hayır |
|\_kendine | Sistem tarafından oluşturulan | Öğenin adreslenebilir URI'si | Yes | Hayır | Hayır | Hayır | Hayır |
|id | Ya da | Mantıksal bölümde Kullanıcı tanımlı benzersiz ad. | Yes | Yes | Yes | Yes | Yes |
|Kullanıcı tanımlı isteğe bağlı özellikler | Kullanıcı tanımlı | API yerel gösteriminde temsil edilen Kullanıcı tanımlı Özellikler (JSON, BSON ve CQL dahil) | Yes | Yes | Yes | Yes | Yes |

> [!NOTE]
> `id` özelliğinin benzersizliği yalnızca her mantıksal bölüm içinde zorlanır. Birden çok belge, farklı bölüm anahtarı değerleriyle aynı `id` özelliğine sahip olabilir.

### <a name="operations-on-items"></a>Öğeleri üzerinde işlemler

Azure Cosmos öğeleri aşağıdaki işlemleri destekler. İşlemleri gerçekleştirmek için Azure Cosmos API 'Lerinden herhangi birini kullanabilirsiniz.

| İşlem | Azure CLI | SQL API’si | Cassandra API’si | MongoDB için Azure Cosmos DB API | Gremlin API | Tablo API’si |
| --- | --- | --- | --- | --- | --- | --- |
| Ekleme, değiştirme, silme, Upsert, okuma | Hayır | Yes | Yes | Yes | Yes | Yes |

## <a name="next-steps"></a>Sonraki adımlar

Bu görevler ve kavramlar hakkında bilgi edinin:

* [Azure Cosmos veritabanında üretilen iş sağlama](how-to-provision-database-throughput.md)
* [Azure Cosmos kapsayıcısında üretilen iş sağlama](how-to-provision-container-throughput.md)
* [Mantıksal bölümlerle çalışma](partition-data.md)
* [Azure Cosmos kapsayıcısında TTL yapılandırma](how-to-time-to-live.md)
* [Değişiklik akışını kullanarak reaktif uygulamalar oluşturun](change-feed.md)
* [Azure Cosmos kapsayıcıda benzersiz bir anahtar kısıtlaması yapılandırma](unique-keys.md)
