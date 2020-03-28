---
title: include dosyası
description: include dosyası
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: include
ms.date: 03/15/2019
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: 27a254b664e2035cc66109f7a473a3e9ef4d730e
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "73612195"
---
<!-- This is the note explaining about the avro and json formats when routing to blob storage. -->
> [!NOTE]
> Veriler, varsayılan olan [Apache Avro](https://avro.apache.org/) biçiminde blob depolamasına veya JSON'a (önizleme) yazılabilir. 
>    
> JSON biçimini kodlama özelliği, Doğu ABD, Batı ABD ve Batı Avrupa dışında IoT Hub'ın kullanılabildiği tüm bölgelerde önizlemede dir. Kodlama biçimi yalnızca blob depolama bitiş noktası yapılandırıldığı anda ayarlanabilir. Biçim, önceden ayarlanmış bir bitiş noktası için değiştirilemez. JSON kodlama kullanırken, içerikType JSON ve içerikEncoding için UTF-8 ileti sistemi özellikleri ayarlamanız gerekir. 
>
> Blob depolama bitiş noktası kullanma hakkında daha ayrıntılı bilgi için lütfen [depolamaalanına yönlendirme kılavuzuna](../articles/iot-hub/iot-hub-devguide-messages-d2c.md#azure-storage)bakın.
>