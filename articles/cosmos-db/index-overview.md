---
title: Azure Cosmos DB’de dizin oluşturma
description: Azure Cosmos DB'de dizin oluşturmanın nasıl çalıştığını, Aralık, Uzamsal, bileşik dizinler gibi farklı türde dizinlerin desteklenmesini anlayın.
author: ThomasWeiss
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/11/2019
ms.author: thweiss
ms.openlocfilehash: 65186262095560d7ae54d32b218d1c01f1fb921d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74873633"
---
# <a name="indexing-in-azure-cosmos-db---overview"></a>Azure Cosmos DB’de dizin oluşturma - Genel bakış

Azure Cosmos DB, şema veya dizin yönetimiyle uğraşmak zorunda kalmadan uygulamanızı yinelenebilmenizi sağlayan şema-agnostik bir veritabanıdır. Varsayılan olarak, Azure Cosmos DB, herhangi bir şema tanımlamak veya ikincil dizinleri yapılandırmak zorunda kalmadan [kapsayıcınızdaki](databases-containers-items.md#azure-cosmos-containers) tüm öğeleriçin her özelliği otomatik olarak dizine dizine alır.

Bu makalenin amacı Azure Cosmos DB'nin verileri nasıl dizine aldığını ve sorgu performansını geliştirmek için dizinleri nasıl kullandığını açıklamaktır. [Dizin oluşturma ilkelerini](index-policy.md)nasıl özelleştireceklerini keşfetmeden önce bu bölümden geçmen önerilir.

## <a name="from-items-to-trees"></a>Öğelerden ağaçlara

Bir öğe her kapsayıcıda depolansa, içeriği JSON belgesi olarak yansıtılır ve ardından ağaç gösterimine dönüştürülür. Bunun anlamı, o öğenin her özelliğinin bir ağaçta düğüm olarak temsil edilmesidir. Sözde kök düğümü, öğenin tüm birinci düzey özelliklerine üst öğe olarak oluşturulur. Yaprak düğümleri, bir öğe tarafından taşınan gerçek skaler değerleri içerir.

Örnek olarak, bu öğeyi göz önünde bulundurun:

```json
    {
        "locations": [
            { "country": "Germany", "city": "Berlin" },
            { "country": "France", "city": "Paris" }
        ],
        "headquarters": { "country": "Belgium", "employees": 250 },
        "exports": [
            { "city": "Moscow" },
            { "city": "Athens" }
        ]
    }
```

Aşağıdaki ağaç tarafından temsil edilecektir:

![Ağaç olarak temsil edilen önceki öğe](./media/index-overview/item-as-tree.png)

Dizilerin ağaçta nasıl kodlandırılmalarına dikkat edin: Bir dizideki her giriş, dizi içindeki bu girişin dizisini (0, 1 vb.) içeren bir ara düğüm alır.

## <a name="from-trees-to-property-paths"></a>Ağaçlardan mülkiyet yollarına

Azure Cosmos DB'nin öğeleri ağaca dönüştürmesinin nedeni, özelliklerin bu ağaçlardaki yolları tarafından başvurulmasına izin vermesidir. Bir özelliğin yolunu almak için, ağacı kök düğümden bu özelliğe geçiş yapabilir ve her çapraz düğümün etiketlerini birleştiririz.

Aşağıda, yukarıda açıklanan örnek öğedeki her özellik için yollar verilmiştir:

    /locations/0/country: "Germany"
    /locations/0/city: "Berlin"
    /locations/1/country: "France"
    /locations/1/city: "Paris"
    /headquarters/country: "Belgium"
    /headquarters/employees: 250
    /exports/0/city: "Moscow"
    /exports/1/city: "Athens"

Bir öğe yazıldığında, Azure Cosmos DB her özelliğin yolunu ve karşılık gelen değerini etkili bir şekilde dizine dizine eder.

## <a name="index-kinds"></a>Dizin türleri

Azure Cosmos DB şu anda üç tür dizin destekler.

### <a name="range-index"></a>Aralık Endeksi

**Aralık** dizini sıralı ağaç benzeri bir yapıyı temel alıyor. Aralık dizini türü için kullanılır:

- Eşitlik sorguları:

    ```sql
   SELECT * FROM container c WHERE c.property = 'value'
   ```

   ```sql
   SELECT * FROM c WHERE c.property IN ("value1", "value2", "value3")
   ```

   Bir dizi öğesinde eşitlik eşleşmesi
   ```sql
    SELECT * FROM c WHERE ARRAY_CONTAINS(c.tags, "tag1”)
    ```

- Aralık sorguları:

   ```sql
   SELECT * FROM container c WHERE c.property > 'value'
   ```
  (, `>`, `<` `>=`, `<=` `!=`, ) için çalışır

- Bir özelliğin varlığını denetleme:

   ```sql
   SELECT * FROM c WHERE IS_DEFINED(c.property)
   ```

- String öneki eşleşmeleri (İçERİğİ anahtar kelime aralık dizini kaldıraç olmaz):

   ```sql
   SELECT * FROM c WHERE STARTSWITH(c.property, "value")
   ```

- `ORDER BY`Sorgu:

   ```sql
   SELECT * FROM container c ORDER BY c.property
   ```

- `JOIN`Sorgu:

   ```sql
   SELECT child FROM container c JOIN child IN c.properties WHERE child = 'value'
   ```

Aralık dizinleri skaler değerler (dize veya sayı) üzerinde kullanılabilir.

### <a name="spatial-index"></a>Uzamsal indeks

**Uzamsal** endeksler, noktalar, çizgiler, çokgenler ve çokgenler gibi jeouzamsal nesneler üzerinde etkin sorgular sağlar. Bu sorgular ST_DISTANCE, ST_WITHIN ST_INTERSECTS anahtar kelimeler kullanır. Mekansal dizin türünü kullanan bazı örnekler şunlardır:

- Jeouzamsal mesafe sorguları:

   ```sql
   SELECT * FROM container c WHERE ST_DISTANCE(c.property, { "type": "Point", "coordinates": [0.0, 10.0] }) < 40
   ```

- Sorgular içinde coğrafi:

   ```sql
   SELECT * FROM container c WHERE ST_WITHIN(c.property, {"type": "Point", "coordinates": [0.0, 10.0] } })
   ```

- Jeouzamsal kesişme sorguları:

   ```sql
   SELECT * FROM c WHERE ST_INTERSECTS(c.property, { 'type':'Polygon', 'coordinates': [[ [31.8, -5], [32, -5], [31.8, -5] ]]  })  
   ```

Uzamsal dizinler doğru biçimlendirilmiş [GeoJSON](geospatial.md) nesnelerinde kullanılabilir. Noktalar, LineString'ler, Çokgenler ve Çokgenler şu anda desteklenir.

### <a name="composite-indexes"></a>Bileşik dizinler

**Bileşik** endeksler, birden çok alanda işlem gerçekleştirirken verimliliği artırır. Bileşik dizin türü için kullanılır:

- `ORDER BY`birden çok özellik üzerinde sorgular:

```sql
 SELECT * FROM container c ORDER BY c.property1, c.property2
```

- Filtreli sorgular `ORDER BY`ve . Filtre özelliği `ORDER BY` yan tümceye eklenirse, bu sorgular bileşik dizin kullanabilir.

```sql
 SELECT * FROM container c WHERE c.property1 = 'value' ORDER BY c.property1, c.property2
```

- En az bir özelliğin eşitlik filtresi olduğu iki veya daha fazla özellik üzerinde filtre içeren sorgular

```sql
 SELECT * FROM container c WHERE c.property1 = 'value' AND c.property2 > 'value'
```

Bir filtre yüklemi dizin türünde kullandığı sürece, sorgu motoru geri kalanını taramadan önce bunu ilk olarak değerlendirir. Örneğin, gibi bir SQL sorgusu varsa`SELECT * FROM c WHERE c.firstName = "Andrew" and CONTAINS(c.lastName, "Liu")`

* Yukarıdaki sorgu ilk indeks kullanarak firstName = "Andrew" girişleri için filtre olacaktır. Daha sonra, Contains filtre yüklemini değerlendirmek için sonraki bir ardışık boru hattından tüm firstName = "Andrew" girişlerini geçirir.

* Dizin kullanan ek filtre yüklemleri ekleyerek sorguları hızlandırabilir ve dizin kullanmayan işlevleri (örn. İçERİk) kullanırken tam kapsayıcı taramalarından kaçınabilirsiniz. Filtre yan tümcelerinin sırası önemli değildir. Sorgu motoru hangi yüklemlerin daha seçici olduğunu çözecek ve sorguyu buna göre çalıştıracaktır.


## <a name="querying-with-indexes"></a>Dizinlerle sorgulama

Verileri dizine alırken çıkarılan yollar, sorguyu işlerken dizini aramayı kolaylaştırır. Sorgu `WHERE` nun yan tümcesini dizine eklenmiş yolların listesiyle eşleştirerek, sorgu yüklemiyle eşleşen öğeleri çok hızlı bir şekilde tanımlamak mümkündür.

Örneğin, aşağıdaki sorguyu `SELECT location FROM location IN company.locations WHERE location.country = 'France'`göz önünde bulundurun: . Sorgu yüklemi (herhangi bir konumun ülkesi olarak "Fransa" olduğu öğelerüzerinde filtreleme) aşağıdaki kırmızı renkle vurgulanan yolla eşleşir:

![Ağaç içinde belirli bir yolu eşleştirme](./media/index-overview/matching-path.png)

> [!NOTE]
> Tek `ORDER BY` bir özellik tarafından sipariş veren bir yan tümce *her zaman* bir aralık dizini gerekir ve başvurur yolu yoksa başarısız olur. Benzer şekilde, `ORDER BY` birden çok özellik tarafından sipariş edilen bir sorgu *her zaman* bileşik dizin gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makalelerde dizin oluşturma hakkında daha fazla bilgi edinin:

- [Dizin oluşturma ilkesi](index-policy.md)
- [Dizin oluşturma ilkesi nasıl yönetilir?](how-to-manage-indexing-policy.md)
