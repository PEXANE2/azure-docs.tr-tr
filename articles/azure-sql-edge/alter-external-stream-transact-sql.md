---
title: Dış akışı DEĞIŞTIRME (Transact-SQL)-Azure SQL Edge (Önizleme)
description: Azure SQL Edge 'de ALTER EXTERNAL STREAM ifadesi hakkında bilgi edinin (Önizleme)
keywords: ''
services: sql-edge
ms.service: sql-edge
ms.topic: conceptual
author: SQLSourabh
ms.author: sourabha
ms.reviewer: sstein
ms.date: 05/19/2020
ms.openlocfilehash: 2559c4b4b875403b7c70671e27cb6222a3f1103a
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84235205"
---
# <a name="alter-external-stream-transact-sql"></a>Dış akışı DEĞIŞTIRME (Transact-SQL)

Dış akışın tanımını değiştirir. *Çalışan* bir durumda akış işi tarafından kullanılan bir dış akışın değiştirilmesine izin verilmez. 



## <a name="syntax"></a>Söz dizimi

```sql
  ALTER EXTERNAL STREAM external_stream_name 
  SET 
    [DATA_SOURCE] = <data_source_name> 
    , LOCATION = <location_name> 
    , EXTERNAL_FILE_FORMAT = <external_file_format_name> 
    , INPUT_OPTIONS = <input_options> 
    , OUTPUT_OPTIONS = <output_options> 
```

## <a name="arguments"></a>Bağımsız değişkenler

Alter External Stream komut bağımsız değişkenleri hakkında daha fazla bilgi için bkz. [dış akış oluşturma (Transact-SQL)](create-external-stream-transact-sql.md).

## <a name="return-code-values"></a>Dönüş kodu değerleri

Başarılı olursa ALTER EXTERNAL STREAM 0 döndürür. Sıfır olmayan bir dönüş değeri hata olduğunu gösterir.


## <a name="see-also"></a>Ayrıca bkz.

- [Dış AKıŞ oluşturma (Transact-SQL)](create-external-stream-transact-sql.md) 
- [Dış akışı bırak (Transact-SQL)](drop-external-stream-transact-sql.md) 
