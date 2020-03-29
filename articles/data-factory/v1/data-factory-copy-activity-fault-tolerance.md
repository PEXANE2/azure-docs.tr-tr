---
title: Uyumsuz satırları atlayarak Azure Veri Fabrikası Kopyalama Etkinliği'nde hata toleransı ekleme
description: Kopyalama sırasında uyumsuz satırları atlayarak Azure Veri Fabrikası Kopyalama Etkinliği'nde hata toleransı eklemeyi öğrenin
services: data-factory
documentationcenter: ''
author: linda33wj
manager: shwang
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 03/27/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 8ffaee75154fd5fe025bdb683c89f16799d6e86b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74926144"
---
# <a name="add-fault-tolerance-in-copy-activity-by-skipping-incompatible-rows"></a>Uyumsuz satırları atlayarak Kopyalama Etkinliğinde hata toleransı ekleme

> [!div class="op_single_selector" title1="Kullandığınız Veri Fabrikası hizmetisürümünü seçin:"]
> * [Sürüm 1](data-factory-copy-activity-fault-tolerance.md)
> * [Sürüm 2 (geçerli sürüm)](../copy-activity-fault-tolerance.md)

> [!NOTE]
> Bu makale, Data Factory’nin 1. sürümü için geçerlidir. Veri Fabrikası hizmetinin geçerli sürümünü kullanıyorsanız, [Veri Fabrikası'nın kopyalama etkinliğinde hata toleransı](../copy-activity-fault-tolerance.md)bakın.

Azure Veri Fabrikası [Kopyalama Etkinliği,](data-factory-data-movement-activities.md) kaynak ve lavabo veri depoları arasında veri kopyalarken uyumsuz satırları işlemeniz için iki yol sunar:

- Uyumsuz verilerle (varsayılan davranış) karşılaşıldığında kopyalama etkinliğini iptal edebilir ve başarısız olabilirsiniz.
- Hata toleransı ekleyerek ve uyumsuz veri satırlarını atlayarak tüm verileri kopyalamaya devam edebilirsiniz. Ayrıca, uyumsuz satırları Azure Blob depolama alanında da günlüğe kaydedebilirsiniz. Daha sonra hatanın nedenini öğrenmek, veri kaynağındaki verileri düzeltmek ve kopyalama etkinliğini yeniden denemek için günlüğü inceleyebilirsiniz.

## <a name="supported-scenarios"></a>Desteklenen senaryolar
Kopyalama Etkinliği, uyumsuz verileri algılama, atlama ve günlüğe kaydetme için üç senaryoyu destekler:

- **Kaynak veri türü ile havuz yerel türü arasında uyumsuzluk**

    Örneğin: Blob depolamasındaki bir CSV dosyasındaki verileri üç **INT** türü sütun içeren şema tanımına sahip bir SQL veritabanına kopyalayın. Sayısal veri içeren CSV dosya satırları, örneğin `123,456,789` lavabo deposuna başarıyla kopyalanır. Ancak, gibi `123,456,abc` sayısal olmayan değerler içeren satırlar uyumsuz olarak algılanır ve atlanır.

- **Kaynak ile havuz arasında sütun sayısı uyuşmazlığı**

    Örneğin: Blob depolamasındaki bir CSV dosyasındaki verileri altı sütun içeren şema tanımına sahip bir SQL veritabanına kopyalayın. Altı sütun içeren CSV dosya satırları lavabo deposuna başarıyla kopyalanır. Altısütundan daha fazla veya daha az sütun içeren CSV dosya satırları uyumsuz olarak algılanır ve atlanır.

- **SQL Server/Azure SQL Veritabanı/Azure Cosmos DB’ye yazarken birincil anahtar ihlali**

    Örneğin: Verileri BIR SQL sunucusundan SQL veritabanına kopyalayın. Birincil anahtar lavabo SQL veritabanında tanımlanır, ancak kaynak SQL sunucusunda böyle bir birincil anahtar tanımlanmamıştır. Kaynakta bulunan yinelenen satırlar lavaboya kopyalanamaz. Etkinlik kopyalarını yalnızca kaynak verilerin ilk satırını lavaboya kopyalar. Yinelenen birincil anahtar değerini içeren sonraki kaynak satırları uyumsuz olarak algılanır ve atlanır.

>[!NOTE]
>Bu özellik, kopyalama etkinliği [Azure SQL Veri Ambarı PolyBase](data-factory-azure-sql-data-warehouse-connector.md#use-polybase-to-load-data-into-azure-sql-data-warehouse) veya [Amazon Redshift Unload](data-factory-amazon-redshift-connector.md#use-unload-to-copy-data-from-amazon-redshift)dahil olmak üzere dış veri yükleme mekanizmasını çağırmak üzere yapılandırıldığında geçerli değildir. PolyBase kullanarak SQL Veri Ambarı'na veri yüklemek için kopyalama etkinliğinde "[polyBaseSettings](data-factory-azure-sql-data-warehouse-connector.md#sqldwsink)" belirterek PolyBase'in yerel hata toleransı desteğini kullanın.

## <a name="configuration"></a>Yapılandırma
Aşağıdaki örnek, Kopyalama Etkinliği'ndeki uyumsuz satırları atlayarak yapılandırmak için bir JSON tanımı sağlar:

```json
"typeProperties": {
    "source": {
        "type": "BlobSource"
    },
    "sink": {
        "type": "SqlSink",
    },
    "enableSkipIncompatibleRow": true,
    "redirectIncompatibleRowSettings": {
        "linkedServiceName": "BlobStorage",
        "path": "redirectcontainer/erroroutput"
    }
}
```

| Özellik | Açıklama | İzin verilen değerler | Gerekli |
| --- | --- | --- | --- |
| **etkinleştirinSkipIncompatibleRow** | Kopya lama sırasında uyumsuz satırları atlamayı etkinleştirin. | True<br/>False (varsayılan) | Hayır |
| **yeniden YönlendirmeUyumsuzSatır Ayarları** | Uyumsuz satırları günlüğe kaydetmek istediğinizde belirtilebilen özellikler grubu. | &nbsp; | Hayır |
| **linkedServiceName** | Atlanan satırları içeren günlüğü depolamak için Azure Depolama'nın bağlantılı hizmeti. | Günlük dosyasını depolamak için kullanmak istediğiniz depolama örneğine atıfta bulunan [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service) veya [AzureStorageSas](data-factory-azure-blob-connector.md#azure-storage-sas-linked-service) bağlantılı bir hizmetin adı. | Hayır |
| **Yolu** | Atlanan satırları içeren günlük dosyasının yolu. | Uyumsuz verileri günlüğe kaydetmek için kullanmak istediğiniz Blob depolama yolunu belirtin. Bir yol sağlamazsanız, hizmet sizin için bir kapsayıcı oluşturur. | Hayır |

## <a name="monitoring"></a>İzleme
Kopyalama etkinliği tamamlandıktan sonra, izleme bölümünde atlanan satır sayısını görebilirsiniz:

![Bağdaşmayan satırları atlayan izleme](./media/data-factory-copy-activity-fault-tolerance/skip-incompatible-rows-monitoring.png)

Uyumsuz satırları günlüğe kaydetmek için yapılandırAcak şekilde yapılandırarak, `https://[your-blob-account].blob.core.windows.net/[path-if-configured]/[copy-activity-run-id]/[auto-generated-GUID].csv` günlük dosyasını bu yolda bulabilirsiniz: Günlük dosyasında, atlanan satırları ve uyumsuzluğun temel nedenini görebilirsiniz.

Hem özgün veriler hem de karşılık gelen hata dosyada günlüğe kaydedilir. Günlük dosyası içeriğinin bir örneği aşağıdaki gibidir:
```
data1, data2, data3, UserErrorInvalidDataValue,Column 'Prop_2' contains an invalid value 'data3'. Cannot convert 'data3' to type 'DateTime'.,
data4, data5, data6, Violation of PRIMARY KEY constraint 'PK_tblintstrdatetimewithpk'. Cannot insert duplicate key in object 'dbo.tblintstrdatetimewithpk'. The duplicate key value is (data4).
```

## <a name="next-steps"></a>Sonraki adımlar
Azure Veri Fabrikası Kopyalama Etkinliği hakkında daha fazla bilgi edinmek için [Kopyalama Etkinliği'ni kullanarak verileri taşı](data-factory-data-movement-activities.md)'ya bakın.
