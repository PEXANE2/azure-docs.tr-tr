---
title: Azure Data Factory bağlı hizmetleri Parametreleştir
description: Azure Data Factory bağlı hizmetleri Parametreleştirme ve çalışma zamanında dinamik değerleri geçirme hakkında bilgi edinin.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 06/18/2020
author: djpmsft
ms.author: daperlov
manager: anandsub
ms.openlocfilehash: 85689661e7f0d170cd88edde8985f46285e679c6
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84987777"
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

Şu anda, aşağıdaki veri depoları için Data Factory Kullanıcı arabiriminde bağlı hizmet Parametreleştirme desteklenir. Diğer tüm veri depoları için, **Bağlantılar** sekmesindeki **kod** simgesini seçerek ve JSON düzenleyicisini kullanarak bağlı hizmeti parametreleştirebilirsiniz.

- Amazon Redshift
- Azure Cosmos DB (SQL API)
- MySQL için Azure Veritabanı
- Azure SQL Veritabanı
- Azure Synapse Analytics (eski adı SQL DW)
- MySQL
- Oracle
- SQL Server

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
