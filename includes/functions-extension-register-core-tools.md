---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 08/07/2020
ms.author: glenga
ms.openlocfilehash: 2936d22eacef73daef4433b3fd296dd4757fa410
ms.sourcegitcommit: bfeae16fa5db56c1ec1fe75e0597d8194522b396
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/10/2020
ms.locfileid: "88031141"
---
Uzantı paketlerinizi kullanmıyorsanız, projeniz için gereken belirli uzantı paketlerini yüklemek için Azure Functions Core Tools yerel olarak kullanabilirsiniz. 

> [!NOTE]
> Temel araçları kullanarak uzantıları el ile yüklemek için .NET Core 2. x SDK 'sının yüklü olması gerekir.

Uzantıları açıkça yüklediğinizde, projenizin köküne Extensions. csproj adlı bir .NET proje dosyası eklenir. Bu dosya, işlevleriniz için gereken NuGet paketleri kümesini tanımlar. Bu dosyadaki [NuGet paket başvurularıyla](/nuget/consume-packages/package-references-in-project-files) çalışabilmenize karşın, çekirdek araçları dosyayı el ile düzenlemek zorunda kalmadan uzantıları yüklemenize olanak sağlar.

Gerekli uzantıları Yerel projenize yüklemek için temel araçları kullanmanın birkaç yolu vardır. 

#### <a name="install-all-extensions"></a>Tüm uzantıları yükler 

Yerel projenizde bağlamalar tarafından kullanılan tüm uzantı paketlerini otomatik olarak eklemek için aşağıdaki komutu kullanın:

```dotnetcli
func extensions install
```
Komut, hangi paketlere ihtiyacınız olduğunu görmek için dosyadaki *function.js* okur, bunları yüklerse ve uzantılar projesini (Extensions. csproj) yeniden oluşturur. Geçerli sürümde yeni bağlamalar ekler ancak mevcut bağlamaları güncelleştirmez. `--force`Yeni yenilerini yüklerken mevcut bağlamaları en son sürüme güncelleştirmek için seçeneğini kullanın.

#### <a name="install-a-specific-extension"></a>Belirli bir uzantıyı yükler

Belirli bir sürüme belirli bir uzantı paketini yüklemek için aşağıdaki komutu kullanın, bu durumda depolama uzantısı:

```dotnetcli
func extensions install --package Microsoft.Azure.WebJobs.Extensions.Storage --version 4.0.2
```