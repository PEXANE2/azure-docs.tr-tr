---
title: Azure Data Factory bağlı hizmetleri Parametreleştir
description: Azure Data Factory bağlı hizmetleri Parametreleştirme ve çalışma zamanında dinamik değerleri geçirme hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 10/16/2020
author: djpmsft
ms.author: daperlov
manager: anandsub
ms.openlocfilehash: 1bb523c1b5aa595a714930962bbbe4e7e04de517
ms.sourcegitcommit: 33368ca1684106cb0e215e3280b828b54f7e73e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2020
ms.locfileid: "92131403"
---
# <a name="parameterize-linked-services-in-azure-data-factory"></a>Azure Data Factory bağlı hizmetleri Parametreleştir

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Artık bağlı bir hizmeti parametreleştirebilirsiniz ve çalışma zamanında dinamik değerler geçirebilirsiniz. Örneğin, aynı mantıksal SQL Server üzerindeki farklı veritabanlarına bağlanmak istiyorsanız, artık bağlı hizmet tanımındaki veritabanı adını parametreleştirebilirsiniz. Bu, mantıksal SQL Server üzerindeki her bir veritabanı için bağlı bir hizmet oluşturmanızı önler. Bağlı hizmet tanımındaki diğer özellikleri de parametreleştirebilirsiniz; Örneğin, *Kullanıcı adı.*

Bağlı hizmetleri parametreleştirmek için Azure portal veya bir programlama arabirimindeki Data Factory Kullanıcı arabirimini kullanabilirsiniz.

> [!TIP]
> Parola veya gizli dizileri parametreetmememiz önerilir. Tüm bağlantı dizelerini Azure Key Vault ' de depolayın ve *gizli adı*parametreleştirin.

Bu özelliğin yedi dakikalık bir girişi ve gösterimi için aşağıdaki videoyu izleyin:

> [!VIDEO https://channel9.msdn.com/shows/azure-friday/Parameterize-connections-to-your-data-stores-in-Azure-Data-Factory/player]

## <a name="supported-data-stores"></a>Desteklenen veri depoları

Herhangi bir bağlı hizmet türünü parametreleştirebilirsiniz.
Kullanıcı arabiriminde bağlantılı hizmet yazarken, Data Factory aşağıdaki bağlayıcı türleri için yerleşik Parametreleştirme deneyimi sağlar. Bağlı hizmet oluşturma/düzenleme dikey penceresinde, yeni parametrelere seçenekler bulabilir ve dinamik içerik ekleyebilirsiniz.

- Amazon Redshift
- Amazon S3
- Azure Cosmos DB (SQL API)
- MySQL için Azure Veritabanı
- Azure SQL Veritabanı
- Azure Synapse Analytics (eski adı SQL DW)
- MySQL
- Oracle
- SQL Server
- Genel HTTP
- Genel REST

Diğer türler için, Kullanıcı arabirimindeki JSON 'u düzenleyerek bağlı hizmeti parametreleştirebilirsiniz:

- Bağlı hizmet oluşturma/düzenleme dikey penceresinde-> alt kısımdaki "Gelişmiş" öğesini genişletin > "JSON biçiminde dinamik içerik belirt" onay kutusunu işaretleyin > bağlantılı hizmet JSON yükünü belirtin. 
- Ya da, Parametreleştirme olmadan bağlı bir hizmet oluşturduktan sonra, [Yönetim Merkezi](author-visually.md#management-hub) -> bağlı hizmetler ' de > belirli bağlı hizmeti bulun-> JSON 'u düzenlemek Için "kod" (düğme " {} ") seçeneğine tıklayın. 

[JSON sample](#json) ` parameters` Parametreleri tanımlamak ve kullanarak parametreye başvurmak için bölüm eklemek üzere JSON örneğine bakın ` @{linkedService().paraName} ` .

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
