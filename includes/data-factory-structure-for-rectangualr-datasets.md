---
author: linda33wj
ms.service: data-factory
ms.topic: include
ms.date: 11/09/2018
ms.author: jingwang
ms.openlocfilehash: 1ab404b838af65dcb75395dfeee1ca0553e497a1
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "67189041"
---
## <a name="specifying-structure-definition-for-rectangular-datasets"></a>Dikdörtgen veri kümeleri için yapı tanımı belirtme
JSON veri kümelerinde yapı bölümü, dikdörtgen tablolar (satırlar & sütunları olan) için **isteğe bağlı** bir bölümdür ve tablo için bir sütun koleksiyonu içerir. Tür dönüştürmeleri için tür bilgilerini sağlamak ya da sütun eşlemeleri yapmak için yapı bölümünü kullanacaksınız. Aşağıdaki bölümlerde bu özellikler ayrıntılı olarak açıklanır. 

Her sütun aşağıdaki özellikleri içerir:

| Özellik | Açıklama | Gerekli |
| --- | --- | --- |
| name |Sütunun adı. |Evet |
| tür |Sütunun veri türü. Tür bilgilerini belirtmeniz gerektiğinde ilgili daha fazla ayrıntı için aşağıdaki tür dönüştürmeleri bölümüne bakın |Hayır |
| kültür |Tür belirtildiğinde ve .NET türü DateTime veya DateTimeOffset olduğunda kullanılacak .NET tabanlı kültür. Varsayılan değer "en-US" dir. |Hayır |
| biçim |Tür belirtildiğinde kullanılacak dize ve .NET türü DateTime veya DateTimeOffset olduğunda kullanılacak biçim dizesi. |Hayır |

Aşağıdaki örnek, UserID, Name ve LastLoginDate adlı üç sütunlu bir tablo için yapı bölümü JSON ' ı gösterir.

```json
"structure": 
[
    { "name": "userid"},
    { "name": "name"},
    { "name": "lastlogindate"}
],
```

"Structure" bilgilerinin ne zaman ekleneceğini ve **Yapı** bölümüne nelerin ekleneceğini görmek için lütfen aşağıdaki yönergeleri kullanın.

* Veri şemasını depolayan **yapılandırılmış veri kaynakları** ve verilerin kendisiyle birlikte (SQL Server, Oracle, Azure tablosu vb. gibi), yalnızca belirli kaynak sütunlarının havuz içindeki belirli sütunlara eşlenmesini istiyorsanız ve adları aynı değilse, "yapı" bölümünü belirtmeniz gerekir (Aşağıdaki sütun eşleme bölümünde ayrıntılara bakın). 
  
    Yukarıda belirtildiği gibi, tür bilgileri "yapı" bölümünde isteğe bağlıdır. Yapılandırılmış kaynaklar için, tür bilgileri veri deposundaki veri kümesi tanımının bir parçası olarak zaten mevcut olduğundan, "yapı" bölümünü dahil ettiğinizde tür bilgilerini içermemelidir.
* **Okuma veri kaynakları üzerindeki şema için (özellikle Azure blob)** verileri herhangi bir şema veya tür bilgilerini depolamadan depolamak zorunda kalmadan depolamayı tercih edebilirsiniz. Bu veri kaynağı türleri için aşağıdaki 2 durumda "Structure" eklemeniz gerekir:
  * Sütun eşlemesi yapmak istiyorsunuz.
  * Veri kümesi bir kopyalama etkinliğinde kaynak olduğunda, "Structure" içinde tür bilgilerini sağlayabilir ve Data Factory, bu tür bilgilerini havuzun yerel türlerine dönüştürmek için kullanır. Daha fazla bilgi için bkz. [Azure Blob 'a veri taşıma](../articles/data-factory/v1/data-factory-azure-blob-connector.md) makalesi.

### <a name="supported-net-based-types"></a>Destek. NET tabanlı türler
Data Factory, Azure Blob gibi okuma veri kaynaklarında şema için "Structure" içinde tür bilgilerini sağlamak için aşağıdaki CLS uyumlu .NET tabanlı tür değerlerini destekler.

* Int16
* Int32 
* Int64
* Tek
* Çift
* Ondalık
* Byte []
* Bool
* Dize 
* Guid
* Tarih saat
* Türünde
* Timespan 

Tarih saat & DateTimeOffset için, isteğe bağlı olarak, özel tarih/saat dizenizi ayrıştırmayı kolaylaştırmak için "kültür" & "biçim" dizesini de belirtebilirsiniz. Aşağıdaki tür dönüştürmesi için örneğe bakın.

