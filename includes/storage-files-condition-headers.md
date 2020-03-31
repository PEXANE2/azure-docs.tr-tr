---
title: include dosyası
description: include dosyası
services: storage
author: roygara
ms.service: storage
ms.topic: include
ms.date: 09/04/2019
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 4604616cd4f2d6c75c272586df1331fc405061cb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "70737500"
---
## <a name="error-conditionheadersnotsupported-from-a-web-application-using-azure-files-from-browser"></a>Hata ConditionHeadersNotBrowser'dan Azure Dosyaları kullanarak Bir Web Uygulamasından Desteklendi

ConditionHeadersNotSupported hatası, web tarayıcısı gibi koşullu üstbilgiden yararlanan bir uygulama aracılığıyla Azure Dosyalarında barındırılan içeriğe erişirken, erişim başarısız olur. Hata, durum üstbilgilerinin desteklenmediğini belirtir.

![Azure Dosyaları koşullu üstbilgi hatası](media/storage-files-condition-headers/conditionalerror.png)

### <a name="cause"></a>Nedeni

Koşullu üstbilgi henüz desteklenmez. Bunları uygulayan uygulamaların dosyaya her erişici olduğunda tam dosyayı istemeleri gerekir.

### <a name="workaround"></a>Geçici çözüm

Yeni bir dosya yüklendiğinde, önbellek denetimi özelliği varsayılan olarak "önbellek yok" olur. Uygulamayı her seferinde dosyayı istemeye zorlamak için, dosyanın önbellek denetimi özelliğinin "önbellek yok"tan "önbellek yok, deposuz, yeniden geçersiz kılması gerekir" olarak güncelleştirilmesi gerekir. Bu, Azure [Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/)kullanılarak elde edilebilir.

![Azure Dosyaları koşullu üstbilgileri için depolama gezgini önbellek değişikliği](media/storage-files-condition-headers/storage-explorer-cache.png)