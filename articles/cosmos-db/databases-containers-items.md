---
title: Azure Cosmos DB veritabanları, kapsayıcılar ve öğelerle çalışma
description: Bu makalede, Azure Cosmos DB veritabanlarının, kapsayıcıların ve öğelerin nasıl oluşturulacağı ve kullanılacağı açıklanmaktadır.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 04/24/2020
ms.reviewer: sngun
ms.openlocfilehash: 257d7a2e374867f6ff14aeaa633d95521b7ca39e
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85114767"
---
# <a name="work-with-databases-containers-and-items-in-azure-cosmos-db"></a>Azure Cosmos DB veritabanları, kapsayıcılar ve öğelerle çalışma

Azure aboneliğinizde bir [Azure Cosmos DB hesabı](account-overview.md) oluşturduktan sonra veritabanları, kapsayıcılar ve öğeler oluşturarak hesabınızdaki verileri yönetebilirsiniz. Bu makalede bu varlıkların her biri açıklanır. 

Aşağıdaki görüntüde Azure Cosmos DB hesabındaki farklı varlıkların hiyerarşisi gösterilmektedir:

:::image type="content" source="./media/databases-containers-items/cosmos-entities.png" alt-text="Azure Cosmos hesabı varlıkları" border="false":::

## <a name="azure-cosmos-databases"></a>Azure Cosmos veritabanları

Hesabınızda bir veya daha fazla Azure Cosmos veritabanı oluşturabilirsiniz. Bir veritabanı bir ad alanına benzer. Veritabanı, bir dizi Azure Cosmos kapsayıcısı için yönetim birimidir. Aşağıdaki tabloda, bir Azure Cosmos veritabanının, API 'ye özgü çeşitli varlıklarla nasıl eşlendiği gösterilmektedir:

| Azure Cosmos varlığı | SQL API’si | Cassandra API’si | MongoDB için Azure Cosmos DB API | Gremlin API | Tablo API’si |
| --- | --- | --- | --- | --- | --- |
|Azure Cosmos veritabanı | Veritabanı | Anahtar alanı | Veritabanı | Veritabanı | NA |

> [!NOTE]
> Tablo API'si hesapları ile ilk tablonuzu oluşturduğunuzda, Azure Cosmos hesabınızda otomatik olarak varsayılan bir veritabanı oluşturulur.

### <a name="operations-on-an-azure-cosmos-database"></a>Azure Cosmos veritabanında işlemler

Aşağıdaki tabloda açıklandığı gibi Azure Cosmos API 'si ile Azure Cosmos veritabanıyla etkileşime geçebilirsiniz:

| Çalışma | Azure CLI | SQL API’si | Cassandra API’si | MongoDB için Azure Cosmos DB API | Gremlin API | Tablo API’si |
| --- | --- | --- | --- | --- | --- | --- |
|Tüm veritabanlarını listeleme| Evet | Evet | Evet (veritabanı bir keyspace ile eşlendi) | Evet | NA | NA |
|Veritabanını oku| Evet | Evet | Evet (veritabanı bir keyspace ile eşlendi) | Evet | NA | NA |
|Yeni veritabanı oluştur| Evet | Evet | Evet (veritabanı bir keyspace ile eşlendi) | Evet | NA | NA |
|Veritabanını güncelleştir| Evet | Evet | Evet (veritabanı bir keyspace ile eşlendi) | Evet | NA | NA |


## <a name="azure-cosmos-containers"></a>Azure Cosmos kapsayıcıları

Azure Cosmos kapsayıcısı, sağlanan aktarım hızı ve depolama için ölçeklenebilirlik birimidir. Bir kapsayıcı yatay olarak bölümlenmiştir ve daha sonra birden çok bölgede çoğaltılır. Kapsayıcıya eklediğiniz öğeler ve üzerinde sağladığınız aktarım hızı, Bölüm anahtarını temel alan bir mantıksal bölümler kümesi üzerinden otomatik olarak dağıtılır. Bölümlendirme ve bölüm anahtarları hakkında daha fazla bilgi için bkz. [bölüm verileri](partition-data.md). 

Bir Azure Cosmos kapsayıcısı oluşturduğunuzda, aşağıdaki modlardan birinde üretilen işi yapılandırın:

* **Adanmış sağlanan aktarım hızı modu**: bir kapsayıcıda sağlanan aktarım hızı, bu kapsayıcı için özel olarak ayrılmıştır ve SLA 'lar tarafından desteklenir. Daha fazla bilgi edinmek için bkz. [Azure Cosmos kapsayıcısında üretilen iş sağlama](how-to-provision-container-throughput.md).

* **Paylaşılan sağlanan aktarım hızı modu**: Bu kapsayıcılar, sağlanan aktarım hızını aynı veritabanındaki diğer kapsayıcılarla paylaşır (adanmış sağlanan aktarım hızı ile yapılandırılmış kapsayıcılar hariç). Diğer bir deyişle, veritabanında sağlanan aktarım hızı "paylaşılan verimlilik" kapsayıcıları arasında paylaşılır. Daha fazla bilgi edinmek için bkz. [Azure Cosmos veritabanında üretilen iş sağlama](how-to-provision-database-throughput.md).

> [!NOTE]
> Yalnızca veritabanı ve kapsayıcı oluştururken paylaşılan ve ayrılmış aktarım hızını yapılandırabilirsiniz. Kapsayıcı oluşturulduktan sonra ayrılmış aktarım hızı modundan paylaşılan aktarım hızı moduna (ve tam tersine) geçiş yapmak için yeni bir kapsayıcı oluşturup verileri yeni kapsayıcıya geçirmeniz gerekir. Azure Cosmos DB akışı Değiştir özelliğini kullanarak verileri geçirebilirsiniz.

Bir Azure Cosmos kapsayıcısı, özel veya paylaşılan sağlanmış verimlilik modlarını kullanarak kapsayıcılar oluşturup esnek ölçeklendirebilirler.

Azure Cosmos kapsayıcısı, öğelerin şema belirsiz bir kapsayıcısıdır. Bir kapsayıcıdaki öğelerin rastgele şemaları olabilir. Örneğin, bir kişiyi temsil eden bir öğe ve bir otomobil öğesini temsil eden bir öğe *aynı kapsayıcıya*yerleştirilebilir. Varsayılan olarak, bir kapsayıcıya eklediğiniz tüm öğeler açık dizin veya şema yönetimine gerek kalmadan otomatik olarak dizinlenir. Bir kapsayıcıda [Dizin oluşturma ilkesini](index-overview.md) yapılandırarak dizin oluşturma davranışını özelleştirebilirsiniz. 

Bir Azure Cosmos kapsayıcısındaki seçili öğelerde [yaşam süresi (TTL)](time-to-live.md) ayarlayabilir veya tüm kapsayıcının bu öğeleri sistemden düzgün bir şekilde temizlemesini sağlayabilirsiniz. Azure Cosmos DB süreleri dolduğunda öğeleri otomatik olarak siler. Ayrıca, kapsayıcıda gerçekleştirilen bir sorgunun, süresi geçen öğelerin sabit bir bağlı olarak döndürülmeyeceğini güvence altına alır. Daha fazla bilgi için bkz. [KAPSAYıCıDA TTL yapılandırma](how-to-time-to-live.md).

[Değişiklik akışını](change-feed.md) , kapsayıcının her bir mantıksal bölümü için yönetilen işlemler günlüğüne abone olmak için kullanabilirsiniz. Değişiklik akışı, kapsayıcıda gerçekleştirilen tüm güncelleştirmelerin günlüğünü, öğelerin önceki ve sonraki görüntülerinden birlikte sağlar. Daha fazla bilgi için bkz. [değişiklik akışını kullanarak reaktif uygulamalar oluşturma](serverless-computing-database.md). Değişiklik akışı için saklama süresini, kapsayıcıda değişiklik akışı ilkesini kullanarak da yapılandırabilirsiniz.

Azure Cosmos Kapsayıcınız için [saklı yordamları, Tetikleyicileri, Kullanıcı tanımlı işlevleri (UDF 'ler)](stored-procedures-triggers-udfs.md)ve [birleştirme yordamlarını](how-to-manage-conflicts.md) kaydedebilirsiniz.

Azure Cosmos kapsayıcıda [benzersiz bir anahtar kısıtlaması](unique-keys.md) belirtebilirsiniz. Benzersiz bir anahtar ilkesi oluşturarak mantıksal bölüm anahtarı başına bir veya daha fazla değerin benzersizliği olduğundan emin olursunuz. Benzersiz anahtar ilkesi kullanarak bir kapsayıcı oluşturursanız, benzersiz anahtar kısıtlaması tarafından belirtilen değerleri yineleyen yeni veya güncelleştirilmiş bir öğe oluşturulabilir. Daha fazla bilgi için bkz. [benzersiz anahtar kısıtlamaları](unique-keys.md).

Aşağıdaki tabloda gösterildiği gibi, bir Azure Cosmos kapsayıcısı, API 'ye özgü varlıklar için özelleştirilmiştir:

| Azure Cosmos varlığı | SQL API’si | Cassandra API’si | MongoDB için Azure Cosmos DB API | Gremlin API | Tablo API’si |
| --- | --- | --- | --- | --- | --- |
|Azure Cosmos kapsayıcısı | Kapsayıcı | Tablo | Koleksiyon | Graf | Tablo |

> [!NOTE]
> Kapsayıcılar oluştururken, aynı ada ancak büyük küçük harflere sahip iki kapsayıcı oluşturmadığınızdan emin olun. Bunun nedeni, Azure platformunun bazı bölümlerinin büyük/küçük harfe duyarlı olmaması ve bu nedenle, bu tür adlara sahip kapsayıcılar ve telemetri ve eylemlerin karışmasına ve çakışmasına neden olabilir.

### <a name="properties-of-an-azure-cosmos-container"></a>Azure Cosmos kapsayıcısının özellikleri

Bir Azure Cosmos kapsayıcısının sistem tarafından tanımlanan özellikler kümesi vardır. Kullandığınız API 'ye bağlı olarak, bazı özellikler doğrudan sunulmayabilir. Aşağıdaki tabloda sistem tarafından tanımlanan özelliklerin listesi açıklanmaktadır:

| Sistem tanımlı özellik | Sistem tarafından oluşturulan veya Kullanıcı tarafından yapılandırılabilen | Amaç | SQL API’si | Cassandra API’si | MongoDB için Azure Cosmos DB API | Gremlin API | Tablo API’si |
| --- | --- | --- | --- | --- | --- | --- | --- |
|\_rid | Sistem tarafından oluşturulan | Kapsayıcının benzersiz tanımlayıcısı | Evet | Hayır | Hayır | Hayır | Hayır |
|\_özelliği | Sistem tarafından oluşturulan | İyimser eşzamanlılık denetimi için kullanılan varlık etiketi | Evet | Hayır | Hayır | Hayır | Hayır |
|\_Talar | Sistem tarafından oluşturulan | Kapsayıcının son güncelleştirilme zaman damgası | Evet | Hayır | Hayır | Hayır | Hayır |
|\_Self | Sistem tarafından oluşturulan | Kapsayıcının adreslenebilir URI 'SI | Evet | Hayır | Hayır | Hayır | Hayır |
|kimlik | Kullanıcı tarafından yapılandırılabilir | Kapsayıcının Kullanıcı tanımlı benzersiz adı | Yes | Yes | Yes | Yes | Yes |
|ındexingpolicy | Kullanıcı tarafından yapılandırılabilir | Dizin yolunu, dizin türünü ve Dizin modunu değiştirme olanağı sağlar | Evet | Hayır | Hayır | Hayır | Evet |
|TimeToLive | Kullanıcı tarafından yapılandırılabilir | Bir zaman aralığı sonrasında bir kapsayıcıdan otomatik olarak öğe silme yeteneği sağlar. Ayrıntılar için bkz. [yaşam süresi](time-to-live.md). | Evet | Hayır | Hayır | Hayır | Evet |
|changeFeedPolicy | Kullanıcı tarafından yapılandırılabilir | Bir kapsayıcıdaki öğelerde yapılan değişiklikleri okumak için kullanılır. Ayrıntılar için bkz. [akışı değiştirme](change-feed.md). | Evet | Hayır | Hayır | Hayır | Evet |
|uniqueKeyPolicy | Kullanıcı tarafından yapılandırılabilir | Mantıksal bir bölümdeki bir veya daha fazla değerin benzersizliğini sağlamak için kullanılır. Daha fazla bilgi için bkz. [benzersiz anahtar kısıtlamaları](unique-keys.md). | Evet | Hayır | Hayır | Hayır | Evet |

### <a name="operations-on-an-azure-cosmos-container"></a>Azure Cosmos kapsayıcısındaki işlemler

Azure Cosmos kapsayıcısı, Azure Cosmos API 'Lerinden herhangi birini kullandığınızda aşağıdaki işlemleri destekler:

| Çalışma | Azure CLI | SQL API’si | Cassandra API’si | MongoDB için Azure Cosmos DB API | Gremlin API | Tablo API’si |
| --- | --- | --- | --- | --- | --- | --- |
| Bir veritabanındaki kapsayıcıları listeleme | Yes | Yes | Yes | Evet | NA | NA |
| Kapsayıcıyı oku | Yes | Yes | Yes | Evet | NA | NA |
| Yeni kapsayıcı oluştur | Yes | Yes | Yes | Evet | NA | NA |
| Kapsayıcıyı güncelleştirme | Yes | Yes | Yes | Evet | NA | NA |
| Kapsayıcı silme | Yes | Yes | Yes | Evet | NA | NA |

## <a name="azure-cosmos-items"></a>Azure Cosmos öğeleri

Kullandığınız API 'ye bağlı olarak, bir Azure Cosmos öğesi koleksiyondaki bir belgeyi, tablodaki bir satırı veya bir grafikteki bir düğümü ya da kenarı temsil edebilir. Aşağıdaki tabloda, bir Azure Cosmos öğesine yönelik API 'ye özgü varlıkların eşleştirmesi gösterilmektedir:

| Cosmos varlığı | SQL API’si | Cassandra API’si | MongoDB için Azure Cosmos DB API | Gremlin API | Tablo API’si |
| --- | --- | --- | --- | --- | --- |
|Azure Cosmos öğesi | Belge | Satır | Belge | Düğüm veya kenar | Öğe |

### <a name="properties-of-an-item"></a>Bir öğenin özellikleri

Her Azure Cosmos öğesi, sistem tarafından tanımlanan aşağıdaki özelliklere sahiptir. Kullandığınız API 'ye bağlı olarak, bunlardan bazıları doğrudan sunulmayabilir.

| Sistem tanımlı özellik | Sistem tarafından oluşturulan veya Kullanıcı tarafından yapılandırılabilen| Amaç | SQL API’si | Cassandra API’si | MongoDB için Azure Cosmos DB API | Gremlin API | Tablo API’si |
| --- | --- | --- | --- | --- | --- | --- | --- |
|\_rid | Sistem tarafından oluşturulan | Öğenin benzersiz tanımlayıcısı | Evet | Hayır | Hayır | Hayır | Hayır |
|\_özelliği | Sistem tarafından oluşturulan | İyimser eşzamanlılık denetimi için kullanılan varlık etiketi | Evet | Hayır | Hayır | Hayır | Hayır |
|\_Talar | Sistem tarafından oluşturulan | Öğenin son güncelleştirme zaman damgası | Evet | Hayır | Hayır | Hayır | Hayır |
|\_Self | Sistem tarafından oluşturulan | Öğenin adreslenebilir URI 'SI | Evet | Hayır | Hayır | Hayır | Hayır |
|kimlik | Kullanabilir | Mantıksal bölümde Kullanıcı tanımlı benzersiz ad. | Evet | Yes | Yes | Yes | Yes |
|Rastgele Kullanıcı tanımlı özellikler | Kullanıcı tanımlı | API yerel gösteriminde temsil edilen Kullanıcı tanımlı Özellikler (JSON, BSON ve CQL dahil) | Evet | Yes | Yes | Yes | Yes |

> [!NOTE]
> Özelliğin benzersizliği `id` yalnızca her mantıksal bölüm içinde zorlanır. Birden çok belge, `id` farklı bölüm anahtarı değerleriyle aynı özelliğe sahip olabilir.

### <a name="operations-on-items"></a>Öğeler üzerinde işlemler

Azure Cosmos öğeleri aşağıdaki işlemleri destekler. İşlemleri gerçekleştirmek için Azure Cosmos API 'Lerinden herhangi birini kullanabilirsiniz.

| Çalışma | Azure CLI | SQL API’si | Cassandra API’si | MongoDB için Azure Cosmos DB API | Gremlin API | Tablo API’si |
| --- | --- | --- | --- | --- | --- | --- |
| INSERT, Replace, DELETE, upsert, Read | Hayır | Evet | Yes | Yes | Yes | Yes |

## <a name="next-steps"></a>Sonraki adımlar

Bu görevler ve kavramlar hakkında bilgi edinin:

* [Azure Cosmos veritabanında üretilen iş sağlama](how-to-provision-database-throughput.md)
* [Azure Cosmos kapsayıcısında üretilen iş sağlama](how-to-provision-container-throughput.md)
* [Mantıksal bölümlerle çalışma](partition-data.md)
* [Azure Cosmos kapsayıcısında TTL yapılandırma](how-to-time-to-live.md)
* [Değişiklik akışını kullanarak reaktif uygulamalar oluşturun](change-feed.md)
* [Azure Cosmos kapsayıcıda benzersiz bir anahtar kısıtlaması yapılandırma](unique-keys.md)
