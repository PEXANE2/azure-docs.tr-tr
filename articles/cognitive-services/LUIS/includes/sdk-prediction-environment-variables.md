---
title: include dosyası
description: include dosyası
ms.topic: include
ms.date: 01/23/2020
ms.openlocfilehash: a7f4e0b47b3aaf9924f8c82eb6a9fa37decc0b2a
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76772395"
---
### <a name="create-an-environment-variable"></a>Bir ortam değişkeni oluşturma

Çalışma zamanı anahtarınızı ve çalışma zamanı bitiş noktasını kullanarak kimlik doğrulama ve erişim için ortam değişkenleri oluşturun:

* `LUIS_RUNTIME_KEY`- İsteklerinizi doğrulamak için çalışma zamanı kaynak anahtarı.
* `LUIS_RUNTIME_ENDPOINT`- Anahtarınızla ilişkili çalışma süresi bitiş noktası.
* `LUIS_APP_ID`- Kamu LUIS IoT `df67dcdb-c37d-46af-88e1-8b97951ca1c2`uygulama kimliği .
* `LUIS_APP_SLOT_NAME` - `production`Veya`staging`

Kendi uygulamanıza erişmek için bu hızlı başlatmayı kullanmayı planlıyorsanız, ek adımlar atmanız gerekir:
* Uygulamayı oluşturun ve uygulama kimliğini alın
* LUIS portalındaki uygulamanın çalışma saati anahtarını atama
* Uygulamaya erişebileceğiniz URL'yi tarayıcıyla test edin

İşletim sisteminiziçin yönergeleri kullanın.

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

Ortam değişkenlerini ekledikten sonra, değişiklikleri etkili kılmak için konsol pencerenizden çalıştırın. `source ~/.bashrc`

#### <a name="macos"></a>[Macos](#tab/unix)

'nizi `.bash_profile`ve ortam değişkenini ekleyin:

```bash
export LUIS_RUNTIME_KEY= <replace-with-your-resource-key>
export LUIS_RUNTIME_ENDPOINT= <replace-with-your-resource-endpoint>
export LUIS_APP_ID= <replace-with-your-app-id>
export LUIS_APP_SLOT_NAME= <replace-with-production-or-staging>
```

Ortam değişkenlerini ekledikten sonra, değişiklikleri etkili kılmak için konsol pencerenizden çalıştırın. `source .bash_profile`

***
