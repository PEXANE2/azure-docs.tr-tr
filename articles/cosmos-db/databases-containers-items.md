---
title: Azure Cosmos DB veritabanları, kapsayıcılar ve öğelerle çalışma
description: Bu makalede, Azure Cosmos DB veritabanlarının, kapsayıcıların ve öğelerin nasıl oluşturulacağı ve kullanılacağı açıklanmaktadır.
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/01/2019
ms.author: rimman
ms.reviewer: sngun
ms.openlocfilehash: de9b0a372d04b40a24b6dc0a8952722129f4a55f
ms.sourcegitcommit: 6794fb51b58d2a7eb6475c9456d55eb1267f8d40
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70241205"
---
# <a name="work-with-databases-containers-and-items-in-azure-cosmos-db"></a>Azure Cosmos DB veritabanları, kapsayıcılar ve öğelerle çalışma

Oluşturduktan sonra bir [Azure Cosmos DB hesabı](account-overview.md) Azure aboneliğiniz kapsamındaki verileri hesabınızdaki veritabanları, kapsayıcılar ve öğeleri oluşturarak yönetebilirsiniz. Bu makalede bu varlıkların her biri açıklanmaktadır. 

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

| Çalışma | Azure CLI | SQL API’si | Cassandra API’si | MongoDB için Azure Cosmos DB API | Gremlin API | Tablo API’si |
| --- | --- | --- | --- | --- | --- | --- |
|Tüm veritabanları listeleme| Evet | Evet | Evet (veritabanı için bir anahtar alanı eşlendi) | Evet | NA | NA |
|Okuma veritabanı| Evet | Evet | Evet (veritabanı için bir anahtar alanı eşlendi) | Evet | NA | NA |
|Yeni veritabanı oluştur| Evet | Evet | Evet (veritabanı için bir anahtar alanı eşlendi) | Evet | NA | NA |
|Veritabanını Güncelleştir| Evet | Evet | Evet (veritabanı için bir anahtar alanı eşlendi) | Evet | NA | NA |


## <a name="azure-cosmos-containers"></a>Azure Cosmos kapsayıcıları

Azure Cosmos kapsayıcısı, sağlanan aktarım hızı ve depolama için ölçeklenebilirlik birimidir. Bir kapsayıcı yatay olarak bölümlenir ve ardından birden çok bölgeye çoğaltılır. Kapsayıcıya eklediğiniz öğeler ve üzerinde sağladığınız aktarım hızı, Bölüm anahtarını temel alan bir mantıksal bölümler kümesi üzerinden otomatik olarak dağıtılır. Bölümlendirme ve bölüm anahtarları hakkında daha fazla bilgi için bkz. [bölüm verileri](partition-data.md). 

Bir Azure Cosmos kapsayıcısı oluşturduğunuzda, aşağıdaki modlardan birinde üretilen işi yapılandırın:

* **Adanmış sağlanan aktarım hızı modu**: Bir kapsayıcıda sağlanan aktarım hızı, bu kapsayıcı için özel olarak ayrılmıştır ve SLA 'Lar tarafından desteklenir. Daha fazla bilgi edinmek için bkz. [Azure Cosmos kapsayıcısında üretilen iş sağlama](how-to-provision-container-throughput.md).

* **Paylaşılan sağlanan aktarım hızı modu**: Bu kapsayıcılar, sağlanan aktarım hızını aynı veritabanındaki diğer kapsayıcılarla paylaşır (adanmış sağlanan aktarım hızı ile yapılandırılmış kapsayıcılar hariç). Diğer bir deyişle, veritabanında sağlanan aktarım hızı "paylaşılan verimlilik" kapsayıcıları arasında paylaşılır. Daha fazla bilgi edinmek için bkz. [Azure Cosmos veritabanında üretilen iş sağlama](how-to-provision-database-throughput.md).

> [!NOTE]
> Yalnızca veritabanı ve kapsayıcı oluştururken paylaşılan ve ayrılmış aktarım hızını yapılandırabilirsiniz. Kapsayıcı oluşturulduktan sonra adanmış aktarım hızı modundan paylaşılan verimlilik moduna (ve tam tersi) geçiş yapmak için yeni bir kapsayıcı oluşturmanız ve verileri yeni kapsayıcıya geçirmeniz gerekir. Azure Cosmos DB akışı Değiştir özelliğini kullanarak verileri geçirebilirsiniz.

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
|\_numarasını | Sistem tarafından oluşturulan | Kapsayıcı benzersiz tanıtıcısı | Evet | Hayır | Hayır | Hayır | Hayır |
|\_özelliği | Sistem tarafından oluşturulan | İyimser eşzamanlılık denetimi için kullanılan varlık etiketi | Evet | Hayır | Hayır | Hayır | Hayır |
|\_Talar | Sistem tarafından oluşturulan | Kapsayıcının son güncelleştirilen zaman damgası | Evet | Hayır | Hayır | Hayır | Hayır |
|\_Self | Sistem tarafından oluşturulan | Kapsayıcının adreslenebilir URI'si | Evet | Hayır | Hayır | Hayır | Hayır |
|id | Kullanıcı tarafından yapılandırılabilir | Kullanıcı tanımlı kapsayıcının benzersiz adı | Evet | Evet | Evet | Evet | Evet |
|indexingPolicy | Kullanıcı tarafından yapılandırılabilir | Dizin yolunu, dizin türünü ve Dizin modunu değiştirme olanağı sağlar | Evet | Hayır | Hayır | Hayır | Evet |
|timeToLive | Kullanıcı tarafından yapılandırılabilir | Bir zaman aralığı sonrasında bir kapsayıcıdan otomatik olarak öğe silme yeteneği sağlar. Ayrıntılar için bkz. [yaşam süresi](time-to-live.md). | Evet | Hayır | Hayır | Hayır | Evet |
|changeFeedPolicy | Kullanıcı tarafından yapılandırılabilir | Bir kapsayıcı içindeki öğelerde yapılan değişiklikleri okumak için kullanılır. Ayrıntılar için bkz. [akışı değiştirme](change-feed.md). | Evet | Hayır | Hayır | Hayır | Evet |
|uniqueKeyPolicy | Kullanıcı tarafından yapılandırılabilir | Mantıksal bir bölümdeki bir veya daha fazla değerin benzersizliğini sağlamak için kullanılır. Daha fazla bilgi için bkz. [benzersiz anahtar kısıtlamaları](unique-keys.md). | Evet | Hayır | Hayır | Hayır | Evet |

### <a name="operations-on-an-azure-cosmos-container"></a>Bir Azure Cosmos kapsayıcısı üzerinde işlemler

Azure Cosmos kapsayıcısı, Azure Cosmos API 'Lerinden herhangi birini kullandığınızda aşağıdaki işlemleri destekler:

| Çalışma | Azure CLI | SQL API’si | Cassandra API’si | MongoDB için Azure Cosmos DB API | Gremlin API | Tablo API’si |
| --- | --- | --- | --- | --- | --- | --- |
| Bir veritabanı kapsayıcılarda listeleme | Evet | Evet | Evet | Evet | NA | NA |
| Bir kapsayıcı okuyun | Evet | Evet | Evet | Evet | NA | NA |
| Yeni kapsayıcı oluştur | Evet | Evet | Evet | Evet | NA | NA |
| Kapsayıcıyı güncelleştirme | Evet | Evet | Evet | Evet | NA | NA |
| Kapsayıcı silme | Evet | Evet | Evet | Evet | NA | NA |

## <a name="azure-cosmos-items"></a>Azure Cosmos öğeleri

Kullandığınız API 'ye bağlı olarak, bir Azure Cosmos öğesi koleksiyondaki bir belgeyi, tablodaki bir satırı veya bir grafikteki bir düğümü ya da kenarı temsil edebilir. Aşağıdaki tabloda, bir Azure Cosmos öğesine yönelik API 'ye özgü varlıkların eşleştirmesi gösterilmektedir:

| Cosmos varlığı | SQL API’si | Cassandra API’si | MongoDB için Azure Cosmos DB API | Gremlin API | Tablo API’si |
| --- | --- | --- | --- | --- | --- |
|Azure Cosmos öğesi | Belge | Satır | Belge | Düğüm veya kenar | Öğe |

### <a name="properties-of-an-item"></a>Bir öğenin özellikleri

Her Azure Cosmos öğesi, sistem tarafından tanımlanan aşağıdaki özelliklere sahiptir. Kullandığınız API 'ye bağlı olarak, bunlardan bazıları doğrudan sunulmayabilir.

| Sistem tanımlı özellik | Sistem tarafından oluşturulan veya Kullanıcı tarafından yapılandırılabilen| Amaç | SQL API’si | Cassandra API’si | MongoDB için Azure Cosmos DB API | Gremlin API | Tablo API’si |
| --- | --- | --- | --- | --- | --- | --- | --- |
|\_numarasını | Sistem tarafından oluşturulan | Öğenin benzersiz tanımlayıcısı | Evet | Hayır | Hayır | Hayır | Hayır |
|\_özelliği | Sistem tarafından oluşturulan | İyimser eşzamanlılık denetimi için kullanılan varlık etiketi | Evet | Hayır | Hayır | Hayır | Hayır |
|\_Talar | Sistem tarafından oluşturulan | Öğenin son güncelleştirme zaman damgası | Evet | Hayır | Hayır | Hayır | Hayır |
|\_Self | Sistem tarafından oluşturulan | Öğenin adreslenebilir URI'si | Evet | Hayır | Hayır | Hayır | Hayır |
|id | Ya da | Mantıksal bölümde Kullanıcı tanımlı benzersiz ad. Kullanıcı KIMLIĞI belirtmezse, sistem otomatik olarak bir tane oluşturur. | Evet | Evet | Evet | Evet | Evet |
|Kullanıcı tanımlı isteğe bağlı özellikler | Kullanıcı tanımlı | API yerel gösteriminde temsil edilen Kullanıcı tanımlı Özellikler (JSON, BSON ve CQL dahil) | Evet | Evet | Evet | Evet | Evet |

> [!NOTE]
> `id` Özelliğin benzersizliği yalnızca her mantıksal bölüm içinde zorlanır. Birden çok belge, farklı bölüm `id` anahtarı değerleriyle aynı özelliğe sahip olabilir.

### <a name="operations-on-items"></a>Öğeleri üzerinde işlemler

Azure Cosmos öğeleri aşağıdaki işlemleri destekler. İşlemleri gerçekleştirmek için Azure Cosmos API 'Lerinden herhangi birini kullanabilirsiniz.

| Çalışma | Azure CLI | SQL API’si | Cassandra API’si | MongoDB için Azure Cosmos DB API | Gremlin API | Tablo API’si |
| --- | --- | --- | --- | --- | --- | --- |
| Ekleme, değiştirme, silme, Upsert, okuma | Hayır | Evet | Evet | Evet | Evet | Evet |

## <a name="next-steps"></a>Sonraki adımlar

Bu görevler ve kavramlar hakkında bilgi edinin:

* [Azure Cosmos veritabanında üretilen iş sağlama](how-to-provision-database-throughput.md)
* [Azure Cosmos kapsayıcısında üretilen iş sağlama](how-to-provision-container-throughput.md)
* [Mantıksal bölümlerle çalışma](partition-data.md)
* [Azure Cosmos kapsayıcısında TTL yapılandırma](how-to-time-to-live.md)
* [Değişiklik akışını kullanarak reaktif uygulamalar oluşturun](change-feed.md)
* [Azure Cosmos kapsayıcıda benzersiz bir anahtar kısıtlaması yapılandırma](unique-keys.md)
