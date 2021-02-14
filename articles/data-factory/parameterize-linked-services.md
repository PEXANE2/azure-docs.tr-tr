---
title: Azure Data Factory bağlı hizmetleri Parametreleştir
description: Azure Data Factory bağlı hizmetleri Parametreleştirme ve çalışma zamanında dinamik değerleri geçirme hakkında bilgi edinin.
ms.service: data-factory
ms.topic: conceptual
ms.date: 01/15/2021
author: dcstwh
ms.author: weetok
ms.openlocfilehash: fbb7cb6fd0041ab77e649b14712907d916eaa449
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2021
ms.locfileid: "100372935"
---
# <a name="parameterize-linked-services-in-azure-data-factory"></a>Azure Data Factory bağlı hizmetleri Parametreleştir

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Artık bağlı bir hizmeti parametreleştirebilirsiniz ve çalışma zamanında dinamik değerler geçirebilirsiniz. Örneğin, aynı mantıksal SQL Server üzerindeki farklı veritabanlarına bağlanmak istiyorsanız, artık bağlı hizmet tanımındaki veritabanı adını parametreleştirebilirsiniz. Bu, mantıksal SQL Server üzerindeki her bir veritabanı için bağlı bir hizmet oluşturmanızı önler. Bağlı hizmet tanımındaki diğer özellikleri de parametreleştirebilirsiniz; Örneğin, *Kullanıcı adı.*

Bağlı hizmetleri parametreleştirmek için Azure portal veya bir programlama arabirimindeki Data Factory Kullanıcı arabirimini kullanabilirsiniz.

> [!TIP]
> Parola veya gizli dizileri parametreetmememiz önerilir. Tüm bağlantı dizelerini Azure Key Vault ' de depolayın ve *gizli adı* parametreleştirin.

> [!Note]
> Parametre adlarında "-" kullanmak için açık hata var, hata çözümlenene kadar adları "-" olmadan kullanmanız önerilir.

Bu özelliğin yedi dakikalık bir girişi ve gösterimi için aşağıdaki videoyu izleyin:

> [!VIDEO https://channel9.msdn.com/shows/azure-friday/Parameterize-connections-to-your-data-stores-in-Azure-Data-Factory/player]

## <a name="supported-linked-service-types"></a>Desteklenen bağlı hizmet türleri

Herhangi bir bağlı hizmet türünü parametreleştirebilirsiniz.
Kullanıcı arabiriminde bağlı hizmet yazarken Data Factory, aşağıdaki bağlantılı hizmet türleri için yerleşik Parametreleştirme deneyimi sağlar. Bağlı hizmet oluşturma/düzenleme dikey penceresinde, yeni parametrelere seçenekler bulabilir ve dinamik içerik ekleyebilirsiniz.

- Amazon Redshift
- Amazon S3
- Azure Cosmos DB (SQL API)
- MySQL için Azure Veritabanı
- Azure Databricks
- Azure Key Vault
- Azure SQL Veritabanı
- Azure SQL Yönetilen Örnek
- Azure Synapse Analytics 
- MySQL
- Oracle
- SQL Server
- Genel HTTP
- Genel REST

Yukarıdaki listede yer alan diğer bağlı hizmet türleri için, Kullanıcı arabirimindeki JSON 'u düzenleyerek bağlı hizmeti parametreleştirebilirsiniz:

- Bağlı hizmet oluşturma/düzenleme dikey penceresinde-> alt kısımdaki "Gelişmiş" öğesini genişletin > "JSON biçiminde dinamik içerik belirt" onay kutusunu işaretleyin > bağlantılı hizmet JSON yükünü belirtin. 
- Ya da, Parametreleştirme olmadan bağlı bir hizmet oluşturduktan sonra, [Yönetim Merkezi](author-visually.md#management-hub) -> bağlı hizmetler ' de > belirli bağlı hizmeti bulun-> JSON 'u düzenlemek Için "kod" (düğme " {} ") seçeneğine tıklayın. 

[](#json) ` parameters` Parametreleri tanımlamak ve kullanarak parametreye başvurmak için bölüm eklemek üzere JSON örneğine bakın ` @{linkedService().paraName} ` .

## <a name="data-factory-ui"></a>Data Factory Kullanıcı Arabirimi (UI)

![Bağlı hizmet tanımına dinamik içerik ekleme](media/parameterize-linked-services/parameterize-linked-services-image1.png)

![Yeni parametre oluşturma](media/parameterize-linked-services/parameterize-linked-services-image2.png)

## <a name="json"></a>JSON

```json
{
    "name": "AzureSqlDatabase",
    "properties": {
        "type": "AzureSqlDatabase",
        "typeProperties": {
            "connectionString": "Server=tcp:myserver.database.windows.net,1433;Database=@{linkedService().DBName};User ID=user;Password=fake;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
        },
        "connectVia": null,
        "parameters": {
            "DBName": {
                "type": "String"
            }
        }
    }
}
```
