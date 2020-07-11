---
title: Azure Data Factory bağlayıcıya genel bakış
description: Data Factory içinde desteklenen bağlayıcıları öğrenin.
services: data-factory
author: linda33wj
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 07/08/2020
ms.author: jingwang
ms.reviewer: craigg
ms.openlocfilehash: f22b4bec17abe95be2a17d3e0e3c41fab3483fe0
ms.sourcegitcommit: f844603f2f7900a64291c2253f79b6d65fcbbb0c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2020
ms.locfileid: "86224833"
---
# <a name="azure-data-factory-connector-overview"></a>Azure Data Factory bağlayıcıya genel bakış

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Azure Data Factory, kopyalama, veri akışı, arama, meta veri al ve silme etkinlikleri aracılığıyla aşağıdaki veri depolarını ve biçimlerini destekler. Desteklenen özellikleri ve Ayrıntılar ' daki ilgili konfigürasyonları öğrenmek için her bir veri deposuna tıklayın.

## <a name="supported-data-stores"></a>Desteklenen veri depoları

[!INCLUDE [Connector overview](../../includes/data-factory-v2-connector-overview.md)]

## <a name="supported-file-formats"></a>Desteklenen dosya biçimleri

Azure Data Factory aşağıdaki dosya biçimlerini destekler. Biçim tabanlı ayarlar için her makaleye başvurun.

- [Avro biçimi](format-avro.md)
- [İkili biçim](format-binary.md)
- [Common Data Model biçimi](format-common-data-model.md)
- [Sınırlandırılmış metin biçimi](format-delimited-text.md)
- [Delta biçimi](format-delta.md)
- [Excel biçimi](format-excel.md)
- [JSON biçimi](format-json.md)
- [ORC biçimi](format-orc.md)
- [Parquet biçimi](format-parquet.md)

## <a name="next-steps"></a>Sonraki adımlar

- [Kopyalama etkinliği](copy-activity-overview.md)
- [Eşleme Veri Akışı](concepts-data-flow-overview.md)
- [Arama Etkinliği](control-flow-lookup-activity.md)
- [Meta Veri Alma Etkinliği](control-flow-get-metadata-activity.md)
- [Etkinliği sil](delete-activity.md)
