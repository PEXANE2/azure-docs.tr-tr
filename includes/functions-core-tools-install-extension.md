---
title: include dosyası
description: include dosyası
services: functions
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 05/25/2019
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: 94cac0932da5880e5e7b8a8fac3870b5bc464af9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "75564808"
---
## <a name="register-extensions"></a>Kayıt uzantıları

HTTP ve zamanlayıcı tetikleyicileri dışında, çalışma zamanı sürüm 2.x ve daha yüksek işlevler bağlamaları uzantı paketleri olarak uygulanır. Sürüm 2.x'te ve Azure İşlevleri çalışma zamanının ötesinde, işlevlerinizde kullanılan bağlama türleri için uzantıları açıkça kaydetmeniz gerekir. Bunun istisnaları, uzantı gerektirmeyen HTTP bağlamaları ve zamanlayıcı tetikleyicileridir.

Bağlama uzantılarını tek tek yüklemeyi seçebilir veya ana bilgisayar.json proje dosyasına bir uzantı paketi başvurusu ekleyebilirsiniz. Uzantı paketleri, birden çok bağlama türü kullanırken paket uyumluluğu sorunları olma olasılığını ortadan kaldırır. Bağlayıcı uzantıları kaydetmek için önerilen yaklaşımdır. Uzantı paketleri de .NET Core 2.x SDK yükleme gereksinimini kaldırır. 

### <a name="extension-bundles"></a>Uzatma paketleri

[!INCLUDE [Register extensions](functions-extension-bundles.md)]

Daha fazla bilgi için Azure [İşlevlerini kaydedin uzantıları](../articles/azure-functions/functions-bindings-register.md#extension-bundles). function.json dosyasına bağlama eklemeden önce host.json'a uzantı demetleri eklemeniz gerekir.

### <a name="register-individual-extensions"></a>Tek tek uzantıları kaydedin

Pakette olmayan uzantıları yüklemeniz gerekiyorsa, belirli bağlamalar için tek tek uzantı paketlerini el ile kaydedebilirsiniz. 

> [!NOTE]
> Uzantıları `func extensions install`kullanarak el ile kaydetmek için .NET Core 2.x SDK yüklü olmalıdır.

*function.json* dosyanızı işlevinizin ihtiyaç duyduğu tüm bağlamaları içerecek şekilde güncelledikten sonra, aşağıdaki komutu proje klasöründe çalıştırın.

```bash
func extensions install
```

Komut, gereksinim duyduğunuz paketleri görmek için *function.json* dosyasını okur, yükler ve uzantılar projesini yeniden çalışır. Geçerli sürüme yeni bağlamalar ekler, ancak varolan bağlamaları güncelleştirmez. Yenilerini `--force` yüklerken varolan bağlamaları en son sürüme güncelleştirme seçeneğini kullanın.
