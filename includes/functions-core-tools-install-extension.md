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
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "75564808"
---
## <a name="register-extensions"></a>Uzantıları Kaydet

HTTP ve Zamanlayıcı Tetikleyicileri hariç olmak üzere, çalışma zamanı sürüm 2. x ve üzeri içindeki Işlev bağlamaları uzantı paketleri olarak uygulanır. Azure Işlevleri çalışma zamanının 2. x ve sonraki sürümlerinde, işlevleriniz içinde kullanılan bağlama türleri için uzantıları açıkça kaydetmeniz gerekir. Bunun özel durumları, uzantılar gerektirmeyen HTTP bağlamaları ve Zamanlayıcı tetikleyiclarıdır.

Bağlama uzantılarını ayrı ayrı yüklemeyi seçebilir veya Host. JSON proje dosyasına Uzantı paketi başvurusu ekleyebilirsiniz. Uzantı demeti, birden çok bağlama türü kullanırken paket uyumluluk sorunları olma olasılığını ortadan kaldırır. Bağlama uzantılarını kaydetmek için önerilen yaklaşımdır. Uzantı paketleri de .NET Core 2. x SDK yükleme gereksinimini ortadan kaldırır. 

### <a name="extension-bundles"></a>Uzantı demeti

[!INCLUDE [Register extensions](functions-extension-bundles.md)]

Daha fazla bilgi için bkz. [Azure işlevleri bağlama uzantılarını kaydetme](../articles/azure-functions/functions-bindings-register.md#extension-bundles). Function. JSON dosyasına bağlama eklemeden önce, Host. JSON öğesine uzantı paketleri eklemeniz gerekir.

### <a name="register-individual-extensions"></a>Tek tek uzantıları Kaydet

Bir paket içinde olmayan uzantıları yüklemeniz gerekiyorsa, belirli bağlamalar için bağımsız uzantı paketleri el ile kaydedebilirsiniz. 

> [!NOTE]
> Kullanarak `func extensions install`uzantıları el ile kaydetmek Için .NET Core 2. x SDK 'sının yüklü olması gerekir.

İşlevinizin ihtiyaç duyacağı tüm bağlamaları dahil etmek için *function. JSON* dosyanızı güncelleştirdikten sonra proje klasöründe aşağıdaki komutu çalıştırın.

```bash
func extensions install
```

Komut, hangi paketlere ihtiyacınız olduğunu görmek için *function. JSON* dosyasını okur, bunları yüklerse ve uzantılar projesini yeniden oluşturur. Geçerli sürümde yeni bağlamalar ekler ancak mevcut bağlamaları güncelleştirmez. Yeni yenilerini `--force` yüklerken mevcut bağlamaları en son sürüme güncelleştirmek için seçeneğini kullanın.
