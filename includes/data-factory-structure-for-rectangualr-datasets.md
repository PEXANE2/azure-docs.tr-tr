---
author: linda33wj
ms.service: data-factory
ms.topic: include
ms.date: 11/09/2018
ms.author: jingwang
ms.openlocfilehash: 1ab404b838af65dcb75395dfeee1ca0553e497a1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "67189041"
---
## <a name="specifying-structure-definition-for-rectangular-datasets"></a>Dikdörtgen veri kümeleri için yapı tanımını belirtme
JSON veri kümelerinin yapı bölümü dikdörtgen tablolar (satırlar & sütunlar ile) için **isteğe bağlı** bir bölümdür ve tablo için sütunkoleksiyonu içerir. Tür dönüşümleri için tür bilgileri sağlamak veya sütun eşlemeleri yapmak için yapı bölümünü kullanırsınız. Aşağıdaki bölümlerde bu özellikler ayrıntılı olarak açıklayınız. 

Her sütun aşağıdaki özellikleri içerir:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| ad |Sütunun adı. |Evet |
| type |Sütunun veri türü. Tür bilgilerini ne zaman belirtmeniz gerektiğine ilişkin daha fazla bilgi için aşağıdaki tür dönüşümleri bölümüne bakın |Hayır |
| kültür |.NET tabanlı kültür türü belirtildiğinde kullanılacak ve .NET türü Datetime veya Datetimeoffset olduğunu. Varsayılan "en-us". |Hayır |
| biçim |Tür belirtildiğinde ve .NET türü Datetime veya Datetimeoffset olduğunda kullanılacak biçim dizesi. |Hayır |

Aşağıdaki örnek, üç sütun userid, ad ve lastlogindate olan bir tablo için yapı bölümü JSON gösterir.

```json
"structure": 
[
    { "name": "userid"},
    { "name": "name"},
    { "name": "lastlogindate"}
],
```

Lütfen "yapı" bilgilerinin ne zaman eklendiğinde ve **yapı** bölümüne ne eklensiniz için aşağıdaki yönergeleri kullanın.

* Veri şemasını ve tür bilgilerini verilerin kendisiyle birlikte depolayan **yapılandırılmış veri kaynakları** için (SQL Server, Oracle, Azure tablosu vb. kaynaklar gibi kaynaklar), "yapı" bölümünü yalnızca belirli kaynak sütunların lavabodaki belirli sütunlara sütun eşlemesi yapmak istiyorsanız belirtmeniz gerekir ve adları aynı değildir (aşağıdaki sütun eşleme bölümündeki ayrıntılara bakın). 
  
    Yukarıda belirtildiği gibi, tür bilgileri "yapı" bölümünde isteğe bağlıdır. Yapılandırılmış kaynaklar için, tür bilgileri veri deposunda veri kümesi tanımının bir parçası olarak zaten kullanılabilir, bu nedenle "yapı" bölümünü eklediğinizde tür bilgilerini eklememelisiniz.
* **Okuma veri kaynakları (özellikle Azure blob) üzerinde şema için,** verilerle herhangi bir şema depolamadan veya bilgi yazmadan verileri depolamayı seçebilirsiniz. Bu veri kaynakları türleri için aşağıdaki 2 duruma "yapı" eklemeniz gerekir:
  * Sütun eşleme yapmak istiyorsunuz.
  * Veri kümesi Bir Kopya etkinliğinde bir kaynak olduğunda, "yapı"da tür bilgileri sağlayabilirsiniz ve veri fabrikası bu tür bilgilerini lavabo için yerel türlere dönüştürme için kullanır. Daha fazla bilgi [için verileri Azure Blob makalesine taşıyın.](../articles/data-factory/v1/data-factory-azure-blob-connector.md)

### <a name="supported-net-based-types"></a>Desteklenen. NET tabanlı türleri
Veri fabrikası, Azure blob gibi okuma veri kaynaklarında şema için "yapı" türü bilgisi sağlamak için aşağıdaki CLS uyumlu .NET tabanlı tür değerlerini destekler.

* Int16
* Int32 
* Int64
* Tek
* Çift
* Ondalık
* Bayt[]
* Bool
* Dize 
* Guid
* Tarih saat
* Datetimeoffset
* Timespan 

Datetime & Datetimeoffset için, özel Datetime dizenizin ayrıştırmasını kolaylaştırmak için isteğe bağlı olarak "kültür" & "biçim" dizesini de belirtebilirsiniz. Aşağıdaki tür dönüştürme örneği bakın.

