---
title: dosya dahil etme
description: dosya dahil etme
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: include
ms.date: 03/15/2019
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: b20c97bf919073b01f564b59b3a55eccb7614005
ms.sourcegitcommit: 419cf179f9597936378ed5098ef77437dbf16295
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89043018"
---
<!-- This is the note explaining about the avro and json formats when routing to blob storage. -->
> [!NOTE]
> Veriler, varsayılan ya da JSON (Önizleme) olan [Apache avro](https://avro.apache.org/) biçiminde blob depolamaya yazılabilir. 
>    
> JSON biçimini kodlama özelliği, Doğu ABD, Batı ABD ve Batı Avrupa dışında IoT Hub kullanılabildiği tüm bölgelerde önizleme aşamasındadır. Kodlama biçimi yalnızca BLOB depolama uç noktasının yapılandırıldığı zamanda ayarlanabilir. Zaten ayarlanmış olan bir uç nokta için biçim değiştirilemez. JSON kodlaması kullanılırken, ' yi JSON olarak ve Contentenencoding ' i ileti sistemi özelliklerinde UTF-8 olarak ayarlamanız gerekir. 
>
> BLOB depolama uç noktası kullanma hakkında daha ayrıntılı bilgi için lütfen [depolama alanına yönlendirmeye yönelik kılavuza](../articles/iot-hub/iot-hub-devguide-messages-d2c.md#azure-storage-as-a-routing-endpoint)bakın.
>