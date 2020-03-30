---
title: Depolanan Yordam etkinliğini kullanarak verileri dönüştürme
description: Veri Fabrikası ardışık kaynaklarından bir Azure SQL Veritabanı/Veri Ambarı'nda depolanan yordamı çağırmak için SQL Server Stored Yordam Etkinliği'nin nasıl kullanılacağını açıklar.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
author: nabhishek
ms.author: abnarain
manager: shwang
ms.custom: seo-lt-2019
ms.date: 11/27/2018
ms.openlocfilehash: 4a0709b4eaa8742069eecb4c39712e384645304b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74926657"
---
# <a name="transform-data-by-using-the-sql-server-stored-procedure-activity-in-azure-data-factory"></a>Azure Veri Fabrikası'nda SQL Server Stored Yordametkinliğini kullanarak verileri dönüştürme
> [!div class="op_single_selector" title1="Kullandığınız Veri Fabrikası hizmetisürümünü seçin:"]
> * [Sürüm 1](v1/data-factory-stored-proc-activity.md)
> * [Geçerli sürüm](transform-data-using-stored-procedure.md)

Ham verileri öngörülere ve öngörülere dönüştürmek ve işlemek için Veri Fabrikası [ardışık hattındaki](concepts-pipelines-activities.md) veri dönüştürme etkinliklerini kullanırsınız. Depolanan Yordam Etkinliği, Veri Fabrikası'nın desteklediği dönüşüm etkinliklerinden biridir. Bu makalede, veri dönüşümü ve Veri Fabrikası'nda desteklenen dönüşüm faaliyetlerine genel bir genel bakış sunan [dönüşüm veri](transform-data.md) makalesi üzerine bir temel oluşturur.

> [!NOTE]
> Azure Veri Fabrikası'nda yeniyseniz, [Azure Veri Fabrikası'na Giriş'i](introduction.md) okuyun ve öğreticiyi yapın: Öğretici: Bu makaleyi okumadan önce verileri [dönüştürün.](tutorial-transform-data-spark-powershell.md) 

İşletmenizdeki aşağıdaki veri depolarından birinde veya bir Azure sanal makinesinde (VM) depolanan yordamı çağırmak için Depolanan Yordam Etkinliği'ni kullanabilirsiniz: 

- Azure SQL Veritabanı
- Azure SQL Veri Ambarı
- SQL Server Veritabanı.  SQL Server kullanıyorsanız, veritabanını barındıran aynı makineye veya veritabanına erişimi olan ayrı bir makineye Kendi kendine barındırılan tümleştirme çalışma zamanını yükleyin. Kendi Kendine Barındırılan tümleştirme çalışma süresi, Azure VM'deki veri kaynaklarını bulut hizmetlerine güvenli ve yönetilen bir şekilde şirket içinde/üzerinde bağlayan bir bileşendir. Ayrıntılar için [Kendi barındırılan tümleştirme çalışma zamanı](create-self-hosted-integration-runtime.md) makalesine bakın.

> [!IMPORTANT]
> Verileri Azure SQL Veritabanı'na veya SQL Server'a kopyalarken, **sqlWriterStoredProcedureName** özelliğini kullanarak depolanan yordamı çağırmak için kopyalama **etkinliğindeSqlSink'i** yapılandırabilirsiniz. Özellik hakkında ayrıntılı bilgi için aşağıdaki bağlayıcı makalelere bakın: [Azure SQL Database](connector-azure-sql-database.md), SQL [Server](connector-sql-server.md). Bir kopyalama etkinliği kullanarak verileri Azure SQL Veri Ambarı'na kopyalarken depolanan yordamı çağırmak desteklenmez. Ancak, sql veri ambarında depolanan yordamı çağırmak için depolanan yordam etkinliğini kullanabilirsiniz. 
>
> Azure SQL Veritabanı'ndan veya SQL Server veya Azure SQL Veri Ambarı'ndan veri kopyalarken, **sqlReaderStoredProcedureName** özelliğini kullanarak kaynak veritabanından verileri okumak için depolanmış bir yordam çağırmak için kopyalama etkinliğinde **SqlSource'u** yapılandırabilirsiniz. Daha fazla bilgi için aşağıdaki bağlayıcı makalelere bakın: [Azure SQL Veritabanı](connector-azure-sql-database.md), SQL [Server](connector-sql-server.md), Azure SQL [Veri Ambarı](connector-azure-sql-data-warehouse.md)          

 

## <a name="syntax-details"></a>Sözdizimi ayrıntıları
Burada, Depolanan Yordam Etkinliği tanımlamak için JSON biçimi vereme biçimi vereme

```json
{
    "name": "Stored Procedure Activity",
    "description":"Description",
    "type": "SqlServerStoredProcedure",
    "linkedServiceName": {
        "referenceName": "AzureSqlLinkedService",
        "type": "LinkedServiceReference"
    },
    "typeProperties": {
        "storedProcedureName": "usp_sample",
        "storedProcedureParameters": {
            "identifier": { "value": "1", "type": "Int" },
            "stringData": { "value": "str1" }

        }
    }
}
```

Aşağıdaki tabloda bu JSON özellikleri açıklanmaktadır:

| Özellik                  | Açıklama                              | Gerekli |
| ------------------------- | ---------------------------------------- | -------- |
| ad                      | Etkinliğin adı                     | Evet      |
| açıklama               | Etkinliğin ne için kullanıldığını açıklayan metin | Hayır       |
| type                      | Saklı Yordam Etkinliği için etkinlik türü **SqlServerStoredProcedure** | Evet      |
| linkedServiceName         | Veri Fabrikası'nda bağlantılı hizmet olarak kayıtlı **Azure SQL Veritabanı** veya Azure SQL Veri **Ambarı** veya **SQL Server'a** başvuru. Bu bağlantılı hizmet hakkında bilgi edinmek için [Bkz. Compute bağlantılı hizmetler](compute-linked-services.md) makalesine bakın. | Evet      |
| depolanmışProcedureName       | Çağırmak için depolanan yordamın adını belirtin. | Evet      |
| depolanmışProsedürParametreleri | Depolanan yordam parametreleri için değerleri belirtin. Parametre değerlerini ve bunların veri kaynağı tarafından desteklenen türünü geçmek için kullanın. `"param1": { "value": "param1Value","type":"param1Type" }` Bir parametre için null geçmeniz `"param1": { "value": null }` gerekiyorsa, (tüm küçük harf) kullanın. | Hayır       |

## <a name="parameter-data-type-mapping"></a>Parametre veri türü eşleme
Parametre için belirttiğiniz veri türü, kullanmakta olduğunuz veri kaynağındaki veri türüyle eşleyen Azure Veri Fabrikası türüdür. Veri kaynağınızın veri türü eşlemelerini bağlayıcılar alanında bulabilirsiniz. Bazı örnekler

| veri kaynağı          | Veri Türü Eşleme |
| ---------------------|-------------------|
| Azure SQL Veri Ambarı | https://docs.microsoft.com/azure/data-factory/connector-azure-sql-data-warehouse#data-type-mapping-for-azure-sql-data-warehouse |
| Azure SQL Veritabanı   | https://docs.microsoft.com/azure/data-factory/connector-azure-sql-database#data-type-mapping-for-azure-sql-database | 
| Oracle               | https://docs.microsoft.com/azure/data-factory/connector-oracle#data-type-mapping-for-oracle |
| SQL Server           | https://docs.microsoft.com/azure/data-factory/connector-sql-server#data-type-mapping-for-sql-server |


## <a name="error-info"></a>Hata bilgileri

Depolanan yordam başarısız olduğunda ve hata ayrıntıları döndürdüğünde, hata bilgilerini doğrudan etkinlik çıktısında yakalayamadığınızda. Ancak, Veri Fabrikası tüm etkinlik çalışmasını Azure Monitor'a pompalar. Veri Fabrikası'nın Azure Monitor'a pompalayan olaylar arasında hata ayrıntılarını oraya iter. Örneğin, bu olaylardan e-posta uyarıları ayarlayabilirsiniz. Daha fazla bilgi için [Azure Monitor'u kullanarak Veri fabrikalarını uyarı ve izleme](monitor-using-azure-monitor.md)'ye bakın.

## <a name="next-steps"></a>Sonraki adımlar
Verileri başka şekillerde nasıl dönüştüreceklerini açıklayan aşağıdaki makalelere bakın: 

* [U-SQL Etkinliği](transform-data-using-data-lake-analytics.md)
* [Kovan Etkinliği](transform-data-using-hadoop-hive.md)
* [Domuz Aktivitesi](transform-data-using-hadoop-pig.md)
* [MapReduce Etkinliği](transform-data-using-hadoop-map-reduce.md)
* [Hadoop Akış Etkinliği](transform-data-using-hadoop-streaming.md)
* [Kıvılcım Etkinliği](transform-data-using-spark.md)
* [.NET özel etkinliği](transform-data-using-dotnet-custom-activity.md)
* [Makine Öğrenimi Bach Yürütme Etkinliği](transform-data-using-machine-learning.md)
* [Depolanan yordam etkinliği](transform-data-using-stored-procedure.md)
