---
title: Azure Data Factory'de bağlı hizmetler
description: Veri Fabrikası'nda bağlantılı hizmetler hakkında bilgi edinin. Bağlantılı hizmetler bilgi işlem/veri depolarını veri fabrikasına bağlar.
services: data-factory
documentationcenter: ''
author: djpmsft
ms.author: daperlov
manager: jroth
ms.reviewer: maghan
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 04/25/2019
ms.openlocfilehash: 90e51e8b56bd3fb63d56c630d47770e97f439796
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75563556"
---
# <a name="linked-services-in-azure-data-factory"></a>Azure Data Factory'de bağlı hizmetler
> [!div class="op_single_selector" title1="Kullandığınız Veri Fabrikası hizmetisürümünü seçin:"]
> * [Sürüm 1](v1/data-factory-create-datasets.md)
> * [Geçerli sürüm](concepts-linked-services.md)

Bu makalede, bağlantılı hizmetlerin ne olduğu, JSON biçiminde nasıl tanımlandığı ve Azure Veri Fabrikası ardışık hatlarında nasıl kullanıldığı açıklanmaktadır.

Veri Fabrikası'nda yeniyseniz, genel bakış için [Azure Veri Fabrikası'na Giriş'e](introduction.md) bakın.

## <a name="overview"></a>Genel Bakış
Bir veri fabrikasında bir veya daha fazla işlem hattı olabilir. **Ardışık işlem,** bir görevi birlikte gerçekleştiren **etkinliklerin** mantıksal bir gruplandırmasIdır. Bir işlem hattındaki etkinlikler, verilerinizde gerçekleştirilecek eylemleri tanımlar. Örneğin, şirket içi bir SQL Server'dan Azure Blob depolama alanına veri kopyalamak için bir kopyalama etkinliği kullanabilirsiniz. Ardından, çıktı verileri üretmek için Blob depolamadan gelen verileri işlemek için Azure HDInsight kümesinde Hive komut dosyası çalıştıran bir Kovan etkinliği kullanabilirsiniz. Son olarak, çıktı verilerini Azure SQL Veri Ambarı'na kopyalamak için ikinci bir kopyalama etkinliği kullanabilirsiniz ve bunun üzerine iş zekası (BI) raporlama çözümleri oluşturulur. Boru hatları ve etkinlikler hakkında daha fazla bilgi için Azure Veri Fabrikası'ndaki [Boru Hatları ve etkinliklere](concepts-pipelines-activities.md) bakın.

Şimdi, **veri kümesi,** **etkinliklerinizde** kullanmak istediğiniz verileri giriş ve çıktı olarak yalnızca işaretleyen veya başvuran verilerin adlandırılmış görünümüdür.

Bir veri kümesi oluşturmadan önce, veri deponuzu veri fabrikasına bağlamak için bağlantılı bir **hizmet** oluşturmanız gerekir. Bağlı hizmetler, dış kaynaklara bağlanmak için Data Factory’ye gereken bağlantı bilgilerini tanımlayan bağlantı dizelerine çok benzer. Şöyle düşünün; veri kümesi, bağlı veri depoları içindeki verilerin yapısını temsil eder ve bağlantılı hizmet veri kaynağına olan bağlantıyı tanımlar. Örneğin, Azure Depolama bağlantılı bir hizmet, bir depolama hesabını veri fabrikasına bağlar. Azure Blob veri kümesi, işlenecek giriş bloblarını içeren bu Azure depolama hesabındaki blob kapsayıcısını ve klasörünü temsil eder.

İşte örnek bir senaryo. Blob depolamasundaki verileri bir SQL veritabanına kopyalamak için iki bağlantılı hizmet oluşturursunuz: Azure Depolama ve Azure SQL Veritabanı. Ardından, iki veri kümesi oluşturun: Azure Blob veri kümesi (Azure Depolama bağlantılı hizmete başvurur) ve Azure SQL Table veri kümesi (Azure SQL Veritabanı bağlantılı hizmete başvurur). Azure Depolama ve Azure SQL Veritabanı bağlantılı hizmetler, Veri Fabrikası'nın sırasıyla Azure Depolama ve Azure SQL Veritabanınıza bağlanmak için çalışma zamanında kullandığı bağlantı dizeleri içerir. Azure Blob veri kümesi, Blob depolama alanınızdaki giriş bloblarını içeren blob kapsayıcısını ve blob klasörünü belirtir. Azure SQL Tablo veri kümesi, verilerin kopyalanış olacağı SQL veritabanınızdaki SQL tablosunu belirtir.

Aşağıdaki diyagram, Veri Fabrikası'nda ardışık hatlar, etkinlik, veri kümesi ve bağlantılı hizmet arasındaki ilişkileri gösterir:

![Boru hattı, etkinlik, veri kümesi, bağlantılı hizmetler arasındaki ilişki](media/concepts-datasets-linked-services/relationship-between-data-factory-entities.png)

## <a name="linked-service-json"></a>Bağlantılı hizmet JSON
Veri Fabrikası'ndaki bağlantılı bir hizmet JSON formatında aşağıdaki gibi tanımlanır:

```json
{
    "name": "<Name of the linked service>",
    "properties": {
        "type": "<Type of the linked service>",
        "typeProperties": {
              "<data store or compute-specific type properties>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

Aşağıdaki tabloda yukarıdaki JSON özellikleri açıklanır:

Özellik | Açıklama | Gerekli |
-------- | ----------- | -------- |
ad | Bağlantılı hizmetin adı. Bkz. [Azure Veri Fabrikası - Adlandırma kuralları.](naming-rules.md) |  Evet |
type | Bağlantılı hizmettürü. Örneğin: AzureStorage (veri deposu) veya AzureBatch (bilgi işlem). typeProperties için açıklamaya bakın. | Evet |
typeProperties | Tür özellikleri her veri deposu veya bilgi işlem için farklıdır. <br/><br/> Desteklenen veri deposu türleri ve bunların tür özellikleri için bu makaledeki [veri kümesi türü](concepts-datasets-linked-services.md#dataset-type) tablosuna bakın. Veri deposuna özgü tür özellikleri hakkında bilgi edinmek için veri deposu bağlayıcısı makalesine gidin. <br/><br/> Desteklenen bilgi işlem türleri ve bunların türü özellikleri için [Bkz. Bilgi İşlem bağlantılı hizmetler.](compute-linked-services.md) | Evet |
connectVia | Veri deposuna bağlanmak için kullanılacak [Tümleştirme Çalışma Süresi.](concepts-integration-runtime.md) Azure Tümleştirme Çalışma Süresi'ni veya Kendi kendine barındırılan Tümleştirme Çalışma Süresini (veri deponuz özel bir ağda bulunuyorsa) kullanabilirsiniz. Belirtilmemişse, varsayılan Azure Tümleştirme Çalışma Süresini kullanır. | Hayır

## <a name="linked-service-example"></a>Bağlantılı hizmet örneği
Aşağıdaki bağlantılı hizmet, Azure Depolama bağlantılı bir hizmettir. Türün AzureStorage olarak ayarlanmasına dikkat edin. Azure Depolama bağlantılı hizmetin tür özellikleri arasında bir bağlantı dizesi yer alıyor. Veri Fabrikası hizmeti, çalışma zamanında veri deposuna bağlanmak için bu bağlantı dizesini kullanır.

```json
{
    "name": "AzureStorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="create-linked-services"></a>Bağlı hizmetler oluşturma

Bu araçlardan veya SDK'lardan birini kullanarak bağlantılı hizmetler oluşturabilirsiniz: [.NET API](quickstart-create-data-factory-dot-net.md), [PowerShell](quickstart-create-data-factory-powershell.md), [REST API,](quickstart-create-data-factory-rest-api.md)Azure Kaynak Yöneticisi Şablonu ve Azure portalı

## <a name="data-store-linked-services"></a>Veri deposu bağlantılı hizmetler
[Bağlayıcıya genel bakış](copy-activity-overview.md#supported-data-stores-and-formats) makalesinden Veri Fabrikası tarafından desteklenen veri depolarının listesini bulabilirsiniz. Desteklenen bağlantı özelliklerini öğrenmek için bir veri deposunu tıklatın.

## <a name="compute-linked-services"></a>İşlem bağlantılı hizmetler
Veri fabrikanızdan bağlanabileceğiniz farklı bilgi işlem [ortamları](compute-linked-services.md) ve farklı yapılandırmalar hakkında ayrıntılar için desteklenen başvuru işlem ortamları.

## <a name="next-steps"></a>Sonraki adımlar
Bu araçlardan veya SDK'lardan birini kullanarak ardışık hatlar ve veri kümeleri oluşturmak için adım adım talimatlar için aşağıdaki öğreticiye bakın.

- [Hızlı başlangıç: .NET kullanarak veri fabrikası oluşturma](quickstart-create-data-factory-dot-net.md)
- [Quickstart: PowerShell kullanarak bir veri fabrikası oluşturun](quickstart-create-data-factory-powershell.md)
- [Quickstart: REST API kullanarak bir veri fabrikası oluşturmak](quickstart-create-data-factory-rest-api.md)
- [Quickstart: Azure portalLarını kullanarak bir veri fabrikası oluşturun](quickstart-create-data-factory-portal.md)
