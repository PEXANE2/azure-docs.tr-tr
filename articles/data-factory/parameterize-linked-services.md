---
title: Azure Veri Fabrikası'nda bağlantılı hizmetleri parametrenize
description: Azure Veri Fabrikası'nda bağlantılı hizmetleri nasıl parametrenize geçeceğinizi ve dinamik değerleri çalışma zamanında nasıl geçeceğinizi öğrenin.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 12/18/2018
author: djpmsft
ms.author: daperlov
manager: anandsub
ms.openlocfilehash: acc7284eb607d20ca1d62b478d802be56048bc6c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75440106"
---
# <a name="parameterize-linked-services-in-azure-data-factory"></a>Azure Veri Fabrikası'nda bağlantılı hizmetleri parametrenize

Artık bağlı bir hizmeti parametreye aktarabilir ve dinamik değerleri çalışma zamanında geçirebilirsiniz. Örneğin, aynı Azure SQL Veritabanı sunucusundaki farklı veritabanlarına bağlanmak istiyorsanız, artık bağlı hizmet tanımındaki veritabanı adını parametrenize alabilirsiniz. Bu, Azure SQL veritabanı sunucusundaki her veritabanı için bağlantılı bir hizmet oluşturmanızı önler. Bağlı hizmet tanımındaki diğer özellikleri de parametrenize alabilirsiniz - örneğin, *Kullanıcı adı.*

Bağlantılı hizmetleri parametrelendirmek için Azure portalındaki Veri Fabrikası UI'sini veya programlama arabirimini kullanabilirsiniz.

> [!TIP]
> Parolaları veya sırları parametrelememenizi öneririz. Bunun yerine tüm bağlantı dizeleri'ni Azure Anahtar Kasası'nda saklayın ve *Gizli Adı*parametreleştirin.

Yedi dakikalık bir tanıtım ve bu özelliğin tanıtımı için aşağıdaki videoyu izleyin:

> [!VIDEO https://channel9.msdn.com/shows/azure-friday/Parameterize-connections-to-your-data-stores-in-Azure-Data-Factory/player]

## <a name="supported-data-stores"></a>Desteklenen veri depoları

Şu anda, bağlantılı hizmet parametrelendirmesi aşağıdaki veri depoları için Azure portalındaki Veri Fabrikası UI'sinde desteklenir. Diğer tüm veri depoları için, **Bağlantılar** sekmesindeki **Kod** simgesini seçerek ve JSON düzenleyicisini kullanarak bağlantılı hizmeti parametrenize edebilirsiniz.
- Azure SQL Veritabanı
- Azure SQL Veri Ambarı
- SQL Server
- Oracle
- Cosmos DB
- Amazon Redshift
- MySQL
- MySQL için Azure Veritabanı

## <a name="data-factory-ui"></a>Data Factory Kullanıcı Arabirimi (UI)

![Bağlantılı Hizmet tanımına dinamik içerik ekleme](media/parameterize-linked-services/parameterize-linked-services-image1.png)

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
