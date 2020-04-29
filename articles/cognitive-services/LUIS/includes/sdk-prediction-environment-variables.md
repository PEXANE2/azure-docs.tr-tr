---
title: include dosyası
description: include dosyası
ms.topic: include
ms.date: 01/23/2020
ms.openlocfilehash: a7f4e0b47b3aaf9924f8c82eb6a9fa37decc0b2a
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "76772395"
---
### <a name="create-an-environment-variable"></a>Ortam değişkeni oluşturma

Çalışma zamanı anahtarınızı ve çalışma zamanı uç noktasını kullanarak, kimlik doğrulama ve erişim için ortam değişkenleri oluşturun:

* `LUIS_RUNTIME_KEY`-İsteklerinizin kimliğini doğrulamak için çalışma zamanı kaynak anahtarı.
* `LUIS_RUNTIME_ENDPOINT`-Anahtarınızla ilişkili çalışma zamanı uç noktası.
* `LUIS_APP_ID`-Genel LUSıS IoT uygulama KIMLIĞI `df67dcdb-c37d-46af-88e1-8b97951ca1c2`.
* `LUIS_APP_SLOT_NAME` - `production`veya`staging`

Bu hızlı başlangıcı kendi uygulamanıza erişmek için kullanmayı düşünüyorsanız, ek adımlar gerçekleştirmeniz gerekir:
* Uygulamayı oluşturma ve uygulama KIMLIĞINI edinme
* Çalışma zamanı anahtarını LUSıS portalındaki uygulamaya atama
* Uygulamaya erişebileceğiniz URL 'YI tarayıcıyla test edin

İşletim sisteminiz için yönergeleri kullanın.

#### <a name="windows"></a>[Windows](#tab/windows)

```console
setx LUIS_RUNTIME_KEY <replace-with-your-resource-key>
setx LUIS_RUNTIME_ENDPOINT <replace-with-your-resource-endpoint>
setx LUIS_APP_ID <replace-with-your-app-id>
setx LUIS_APP_SLOT_NAME <replace-with-production-or-staging>
```

Ortam değişkenlerini ekledikten sonra konsol penceresini yeniden başlatın.

#### <a name="linux"></a>[Linux](#tab/linux)

```bash
export LUIS_RUNTIME_KEY= <replace-with-your-resource-key>
export LUIS_RUNTIME_ENDPOINT= <replace-with-your-resource-endpoint>
export LUIS_APP_ID= <replace-with-your-app-id>
export LUIS_APP_SLOT_NAME= <replace-with-production-or-staging>
```

Ortam değişkenlerini ekledikten sonra, değişikliklerin etkili olması `source ~/.bashrc` için konsol penceresinizden çalıştırın.

#### <a name="macos"></a>[macOS](#tab/unix)

Hesabınızı `.bash_profile`düzenleyin ve ortam değişkenini ekleyin:

```bash
export LUIS_RUNTIME_KEY= <replace-with-your-resource-key>
export LUIS_RUNTIME_ENDPOINT= <replace-with-your-resource-endpoint>
export LUIS_APP_ID= <replace-with-your-app-id>
export LUIS_APP_SLOT_NAME= <replace-with-production-or-staging>
```

Ortam değişkenlerini ekledikten sonra, değişikliklerin etkili olması `source .bash_profile` için konsol penceresinizden çalıştırın.

***
