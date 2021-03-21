---
title: Azure Cosmos DB kaynak modeli
description: Bu makalede, Azure Cosmos hesabını, veritabanını, kapsayıcısını ve öğelerini içeren Azure Cosmos DB kaynak modeli açıklanmaktadır. Ayrıca, bir Azure Cosmos hesabındaki bu öğelerin hiyerarşisini de ele alır.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/12/2020
ms.reviewer: sngun
ms.openlocfilehash: 4ed881b74f240946d98d9868344c898d3e9a9dad
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "99627357"
---
# <a name="azure-cosmos-db-resource-model"></a>Azure Cosmos DB kaynak modeli
[!INCLUDE[appliesto-all-apis](includes/appliesto-all-apis.md)]

Azure Cosmos DB, tam olarak yönetilen bir hizmet olarak platform (PaaS). Azure Cosmos DB kullanmaya başlamak için başlangıçta Azure aboneliğinizde ve veritabanları, kapsayıcılarınızda ve içindeki öğelerde bir Azure Cosmos hesabı oluşturmanız gerekir. Bu makalede kaynak modeli hiyerarşisindeki Azure Cosmos DB kaynak modeli ve farklı varlıklar açıklanmaktadır.

Azure Cosmos hesabı, genel dağıtım ve yüksek kullanılabilirliğin temel birimidir. Azure Cosmos hesabınız benzersiz bir DNS adı içerir ve Azure portal veya Azure CLı kullanarak ya da dile özgü farklı SDK 'Ları kullanarak bir hesabı yönetebilirsiniz. Daha fazla bilgi için bkz. [Azure Cosmos hesabınızı yönetme](how-to-manage-database-account.md). Verilerinizi ve aktarım hızını birden çok Azure bölgesinde küresel olarak dağıtmak için, istediğiniz zaman hesabınıza Azure bölgelerini ekleyebilir ve kaldırabilirsiniz. Hesabınızı, tek bir bölge veya birden çok yazma bölgesi olacak şekilde yapılandırabilirsiniz. Daha fazla bilgi için bkz. [hesabınıza Azure bölgelerini ekleme ve kaldırma](how-to-manage-database-account.md). Bir hesapta [varsayılan tutarlılık](consistency-levels.md) düzeyini yapılandırabilirsiniz.

## <a name="elements-in-an-azure-cosmos-account"></a>Azure Cosmos hesabındaki öğeler

Azure Cosmos kapsayıcısı, temel ölçeklenebilirlik birimidir. Kapsayıcıda sınırsız bir sağlanan üretilen iş (RU/s) ve depolama alanı oluşturabilirsiniz. Azure Cosmos DB, sağlanan aktarım hızını ve depolamayı esnek ölçeklendirmek için belirttiğiniz mantıksal bölüm anahtarını kullanarak kapsayıcınızı saydam olarak bölümlendirir.

Şu anda, bir Azure aboneliği altında en fazla 50 Azure Cosmos hesabı oluşturabilirsiniz (Bu, destek isteği aracılığıyla artırılabilir ve bu bir sınır olabilir). Tek bir Azure Cosmos hesabı, sınırsız miktarda veriyi ve sağlanan aktarım hızını neredeyse yönetebilir. Verilerinizi ve sağlanan aktarım hızını yönetmek için, hesabınız altında ve bu veritabanı içinde bir veya daha fazla Azure Cosmos veritabanı oluşturabilir, bir veya daha fazla kapsayıcı oluşturabilirsiniz. Aşağıdaki görüntüde, bir Azure Cosmos hesabındaki öğelerin hiyerarşisi gösterilmektedir:

:::image type="content" source="./media/account-databases-containers-items/hierarchy.png" alt-text="Azure Cosmos hesabının hiyerarşisi" border="false":::

Azure aboneliğiniz kapsamında bir hesap oluşturduktan sonra veritabanları, kapsayıcılar ve öğeler oluşturarak hesabınızdaki verileri yönetebilirsiniz. 

Aşağıdaki görüntüde Azure Cosmos DB hesabındaki farklı varlıkların hiyerarşisi gösterilmektedir:

:::image type="content" source="./media/account-databases-containers-items/cosmos-entities.png" alt-text="Azure Cosmos hesabı varlıkları" border="false":::

## <a name="azure-cosmos-databases"></a>Azure Cosmos veritabanları

Hesabınızda bir veya daha fazla Azure Cosmos veritabanı oluşturabilirsiniz. Bir veritabanı bir ad alanına benzer. Veritabanı, bir dizi Azure Cosmos kapsayıcısı için yönetim birimidir. Aşağıdaki tabloda, bir veritabanının, API 'ye özgü çeşitli varlıklarla nasıl eşlendiği gösterilmektedir:

| Azure Cosmos varlığı | SQL API | Cassandra API’si | MongoDB için Azure Cosmos DB API | Gremlin API | Tablo API’si |
| --- | --- | --- | --- | --- | --- |
|Azure Cosmos veritabanı | Veritabanı | Anahtar alanı | Veritabanı | Veritabanı | NA |

> [!NOTE]
> Tablo API'si hesapları ile ilk tablonuzu oluşturduğunuzda, Azure Cosmos hesabınızda otomatik olarak varsayılan bir veritabanı oluşturulur.

### <a name="operations-on-an-azure-cosmos-database"></a>Azure Cosmos veritabanında işlemler

Aşağıdaki tabloda açıklandığı gibi Azure Cosmos API 'si ile Azure Cosmos veritabanıyla etkileşime geçebilirsiniz:

| İşlem | Azure CLI’si | SQL API | Cassandra API’si | MongoDB için Azure Cosmos DB API | Gremlin API | Tablo API’si |
| --- | --- | --- | --- | --- | --- | --- |
|Tüm veritabanlarını listeleme| Yes | Yes | Evet (veritabanı bir keyspace ile eşlendi) | Yes | NA | NA |
|Veritabanını oku| Yes | Yes | Evet (veritabanı bir keyspace ile eşlendi) | Yes | NA | NA |
|Yeni veritabanı oluştur| Yes | Yes | Evet (veritabanı bir keyspace ile eşlendi) | Yes | NA | NA |
|Veritabanını güncelleştir| Yes | Yes | Evet (veritabanı bir keyspace ile eşlendi) | Yes | NA | NA |

## <a name="azure-cosmos-containers"></a>Azure Cosmos kapsayıcıları

Azure Cosmos kapsayıcısı, sağlanan aktarım hızı ve depolama için ölçeklenebilirlik birimidir. Bir kapsayıcı yatay olarak bölümlenmiştir ve daha sonra birden çok bölgede çoğaltılır. Kapsayıcıya eklediğiniz öğeler, bölüm anahtarına göre fiziksel bölümler arasında dağıtılan mantıksal bölümlerle otomatik olarak gruplandırılır. Bir kapsayıcıdaki aktarım hızı fiziksel bölümler arasında eşit olarak dağıtılır. Bölümlendirme ve bölüm anahtarları hakkında daha fazla bilgi için bkz. [bölüm verileri](partitioning-overview.md). 

Bir kapsayıcı oluşturduğunuzda, aşağıdaki modlardan birinde üretilen işi yapılandırırsınız:

* **Adanmış sağlanan aktarım hızı modu**: bir kapsayıcıda sağlanan aktarım hızı, bu kapsayıcı için özel olarak ayrılmıştır ve SLA 'lar tarafından desteklenir. Daha fazla bilgi için bkz. [bir kapsayıcıda üretilen iş sağlama](how-to-provision-container-throughput.md).

* **Paylaşılan sağlanan aktarım hızı modu**: Bu kapsayıcılar, sağlanan aktarım hızını aynı veritabanındaki diğer kapsayıcılarla paylaşır (adanmış sağlanan aktarım hızı ile yapılandırılmış kapsayıcılar hariç). Diğer bir deyişle, veritabanında sağlanan aktarım hızı "paylaşılan verimlilik" kapsayıcıları arasında paylaşılır. Daha fazla bilgi için bkz. [bir veritabanında üretilen iş sağlama](how-to-provision-database-throughput.md).

> [!NOTE]
> Yalnızca veritabanı ve kapsayıcı oluştururken paylaşılan ve ayrılmış aktarım hızını yapılandırabilirsiniz. Kapsayıcı oluşturulduktan sonra ayrılmış aktarım hızı modundan paylaşılan aktarım hızı moduna (ve tam tersine) geçiş yapmak için yeni bir kapsayıcı oluşturup verileri yeni kapsayıcıya geçirmeniz gerekir. Azure Cosmos DB akışı Değiştir özelliğini kullanarak verileri geçirebilirsiniz.

Bir Azure Cosmos kapsayıcısı, özel veya paylaşılan sağlanmış verimlilik modlarını kullanarak kapsayıcılar oluşturup esnek ölçeklendirebilirler.

Kapsayıcı, öğelerin şema belirsiz kapsayıcısıdır. Bir kapsayıcıdaki öğelerin rastgele şemaları olabilir. Örneğin, bir kişiyi temsil eden bir öğe ve bir otomobil öğesini temsil eden bir öğe *aynı kapsayıcıya* yerleştirilebilir. Varsayılan olarak, bir kapsayıcıya eklediğiniz tüm öğeler açık dizin veya şema yönetimine gerek kalmadan otomatik olarak dizinlenir. Bir kapsayıcıda [Dizin oluşturma ilkesini](index-overview.md) yapılandırarak dizin oluşturma davranışını özelleştirebilirsiniz. 

Bir kapsayıcıdaki seçili öğeler üzerinde [yaşam süresi (TTL)](time-to-live.md) ayarlayabilir veya tüm kapsayıcının bu öğeleri sistemden düzgün bir şekilde temizlemesini sağlayabilirsiniz. Azure Cosmos DB süreleri dolduğunda öğeleri otomatik olarak siler. Ayrıca, kapsayıcıda gerçekleştirilen bir sorgunun, süresi geçen öğelerin sabit bir bağlı olarak döndürülmeyeceğini güvence altına alır. Daha fazla bilgi için bkz. [KAPSAYıCıDA TTL yapılandırma](how-to-time-to-live.md).

[Değişiklik akışını](change-feed.md) , kapsayıcının her bir mantıksal bölümü için yönetilen işlemler günlüğüne abone olmak için kullanabilirsiniz. Değişiklik akışı, kapsayıcıda gerçekleştirilen tüm güncelleştirmelerin günlüğünü, öğelerin önceki ve sonraki görüntülerinden birlikte sağlar. Daha fazla bilgi için bkz. [değişiklik akışını kullanarak reaktif uygulamalar oluşturma](serverless-computing-database.md). Değişiklik akışı için saklama süresini, kapsayıcıda değişiklik akışı ilkesini kullanarak da yapılandırabilirsiniz.

Kapsayıcıda [saklı yordamları, Tetikleyicileri, Kullanıcı tanımlı işlevleri (UDF 'ler)](stored-procedures-triggers-udfs.md)ve [birleştirme yordamlarını](how-to-manage-conflicts.md) kaydedebilirsiniz.

Azure Cosmos kapsayıcıda [benzersiz bir anahtar kısıtlaması](unique-keys.md) belirtebilirsiniz. Benzersiz bir anahtar ilkesi oluşturarak mantıksal bölüm anahtarı başına bir veya daha fazla değerin benzersizliği olduğundan emin olursunuz. Benzersiz anahtar ilkesi kullanarak bir kapsayıcı oluşturursanız, benzersiz anahtar kısıtlaması tarafından belirtilen değerleri yineleyen yeni veya güncelleştirilmiş bir öğe oluşturulabilir. Daha fazla bilgi için bkz. [benzersiz anahtar kısıtlamaları](unique-keys.md).

Bir kapsayıcı, aşağıdaki tabloda gösterildiği gibi, API 'ye özgü varlıklara özgüdür:

| Azure Cosmos varlığı | SQL API | Cassandra API’si | MongoDB için Azure Cosmos DB API | Gremlin API | Tablo API’si |
| --- | --- | --- | --- | --- | --- |
|Azure Cosmos kapsayıcısı | Kapsayıcı | Tablo | Koleksiyon | Graf | Tablo |

> [!NOTE]
> Kapsayıcılar oluştururken, aynı ada ancak büyük küçük harflere sahip iki kapsayıcı oluşturmadığınızdan emin olun. Bunun nedeni, Azure platformunun bazı bölümlerinin büyük/küçük harfe duyarlı olmaması ve bu nedenle, bu tür adlara sahip kapsayıcılar ve telemetri ve eylemlerin karışmasına ve çakışmasına neden olabilir.

### <a name="properties-of-an-azure-cosmos-container"></a>Azure Cosmos kapsayıcısının özellikleri

Bir Azure Cosmos kapsayıcısının sistem tarafından tanımlanan özellikler kümesi vardır. Kullandığınız API 'ye bağlı olarak, bazı özellikler doğrudan sunulmayabilir. Aşağıdaki tabloda sistem tarafından tanımlanan özelliklerin listesi açıklanmaktadır:

| Sistem tanımlı özellik | Sistem tarafından oluşturulan veya Kullanıcı tarafından yapılandırılabilen | Amaç | SQL API | Cassandra API’si | MongoDB için Azure Cosmos DB API | Gremlin API | Tablo API’si |
| --- | --- | --- | --- | --- | --- | --- | --- |
|\_rid | Sistem tarafından oluşturulan | Kapsayıcının benzersiz tanımlayıcısı | Yes | Hayır | Hayır | Hayır | Hayır |
|\_özelliği | Sistem tarafından oluşturulan | İyimser eşzamanlılık denetimi için kullanılan varlık etiketi | Yes | Hayır | Hayır | Hayır | Hayır |
|\_Talar | Sistem tarafından oluşturulan | Kapsayıcının son güncelleştirilme zaman damgası | Yes | Hayır | Hayır | Hayır | Hayır |
|\_Self | Sistem tarafından oluşturulan | Kapsayıcının adreslenebilir URI 'SI | Yes | Hayır | Hayır | Hayır | Hayır |
|kimlik | Kullanıcı tarafından yapılandırılabilir | Kapsayıcının Kullanıcı tanımlı benzersiz adı | Yes | Yes | Yes | Yes | Yes |
|ındexingpolicy | Kullanıcı tarafından yapılandırılabilir | Dizin yolunu, dizin türünü ve Dizin modunu değiştirme olanağı sağlar | Yes | Hayır | Hayır | Hayır | Yes |
|TimeToLive | Kullanıcı tarafından yapılandırılabilir | Bir zaman aralığı sonrasında bir kapsayıcıdan otomatik olarak öğe silme yeteneği sağlar. Ayrıntılar için bkz. [yaşam süresi](time-to-live.md). | Yes | Hayır | Hayır | Hayır | Yes |
|changeFeedPolicy | Kullanıcı tarafından yapılandırılabilir | Bir kapsayıcıdaki öğelerde yapılan değişiklikleri okumak için kullanılır. Ayrıntılar için bkz. [akışı değiştirme](change-feed.md). | Yes | Hayır | Hayır | Hayır | Yes |
|uniqueKeyPolicy | Kullanıcı tarafından yapılandırılabilir | Mantıksal bir bölümdeki bir veya daha fazla değerin benzersizliğini sağlamak için kullanılır. Daha fazla bilgi için bkz. [benzersiz anahtar kısıtlamaları](unique-keys.md). | Yes | Hayır | Hayır | Hayır | Yes |

### <a name="operations-on-an-azure-cosmos-container"></a>Azure Cosmos kapsayıcısındaki işlemler

Azure Cosmos kapsayıcısı, Azure Cosmos API 'Lerinden herhangi birini kullandığınızda aşağıdaki işlemleri destekler:

| İşlem | Azure CLI’si | SQL API | Cassandra API’si | MongoDB için Azure Cosmos DB API | Gremlin API | Tablo API’si |
| --- | --- | --- | --- | --- | --- | --- |
| Bir veritabanındaki kapsayıcıları listeleme | Yes | Yes | Yes | Yes | NA | NA |
| Kapsayıcıyı oku | Yes | Yes | Yes | Yes | NA | NA |
| Yeni kapsayıcı oluştur | Yes | Yes | Yes | Yes | NA | NA |
| Kapsayıcıyı güncelleştirme | Yes | Yes | Yes | Yes | NA | NA |
| Kapsayıcı silme | Yes | Yes | Yes | Yes | NA | NA |

## <a name="azure-cosmos-items"></a>Azure Cosmos öğeleri

Kullandığınız API 'ye bağlı olarak, bir Azure Cosmos öğesi koleksiyondaki bir belgeyi, tablodaki bir satırı veya bir grafikteki bir düğümü ya da kenarı temsil edebilir. Aşağıdaki tabloda, bir Azure Cosmos öğesine yönelik API 'ye özgü varlıkların eşleştirmesi gösterilmektedir:

| Cosmos varlığı | SQL API | Cassandra API’si | MongoDB için Azure Cosmos DB API | Gremlin API | Tablo API’si |
| --- | --- | --- | --- | --- | --- |
|Azure Cosmos öğesi | Öğe | Satır | Belge | Düğüm veya kenar | Öğe |

### <a name="properties-of-an-item"></a>Bir öğenin özellikleri

Her Azure Cosmos öğesi, sistem tarafından tanımlanan aşağıdaki özelliklere sahiptir. Kullandığınız API 'ye bağlı olarak, bunlardan bazıları doğrudan sunulmayabilir.

| Sistem tanımlı özellik | Sistem tarafından oluşturulan veya Kullanıcı tarafından yapılandırılabilen| Amaç | SQL API | Cassandra API’si | MongoDB için Azure Cosmos DB API | Gremlin API | Tablo API’si |
| --- | --- | --- | --- | --- | --- | --- | --- |
|\_rid | Sistem tarafından oluşturulan | Öğenin benzersiz tanımlayıcısı | Yes | Hayır | Hayır | Hayır | Hayır |
|\_özelliği | Sistem tarafından oluşturulan | İyimser eşzamanlılık denetimi için kullanılan varlık etiketi | Yes | Hayır | Hayır | Hayır | Hayır |
|\_Talar | Sistem tarafından oluşturulan | Öğenin son güncelleştirme zaman damgası | Yes | Hayır | Hayır | Hayır | Hayır |
|\_Self | Sistem tarafından oluşturulan | Öğenin adreslenebilir URI 'SI | Yes | Hayır | Hayır | Hayır | Hayır |
|kimlik | Herhangi biri | Mantıksal bölümde Kullanıcı tanımlı benzersiz ad. | Yes | Yes | Yes | Yes | Yes |
|Rastgele Kullanıcı tanımlı özellikler | Kullanıcı tanımlı | API yerel gösteriminde temsil edilen Kullanıcı tanımlı Özellikler (JSON, BSON ve CQL dahil) | Yes | Yes | Yes | Yes | Yes |

> [!NOTE]
> Özelliğin benzersizliği `id` yalnızca her mantıksal bölüm içinde zorlanır. Birden çok belge, `id` farklı bölüm anahtarı değerleriyle aynı özelliğe sahip olabilir.

### <a name="operations-on-items"></a>Öğeler üzerinde işlemler

Azure Cosmos öğeleri aşağıdaki işlemleri destekler. İşlemleri gerçekleştirmek için Azure Cosmos API 'Lerinden herhangi birini kullanabilirsiniz.

| İşlem | Azure CLI’si | SQL API | Cassandra API’si | MongoDB için Azure Cosmos DB API | Gremlin API | Tablo API’si |
| --- | --- | --- | --- | --- | --- | --- |
| INSERT, Replace, DELETE, upsert, Read | Hayır | Yes | Yes | Yes | Yes | Evet |

## <a name="next-steps"></a>Sonraki adımlar

Azure Cosmos hesabınızı ve diğer kavramları yönetmeyi öğrenin:

* [Azure Cosmos hesabınızı yönetme](how-to-manage-database-account.md)
* [Genel dağıtım](distribute-data-globally.md)
* [Tutarlılık düzeyleri](consistency-levels.md)
* [Azure Cosmos hesabınız için VNET hizmeti uç noktası](how-to-configure-vnet-service-endpoint.md)
* [Azure Cosmos hesabınız için IP-güvenlik duvarı](how-to-configure-firewall.md)
* [Azure Cosmos hesabınıza Azure bölgelerini ekleme ve kaldırma](how-to-manage-database-account.md)
* [Azure Cosmos DB SLA 'Lar](https://azure.microsoft.com/support/legal/sla/cosmos-db/v1_2/)
