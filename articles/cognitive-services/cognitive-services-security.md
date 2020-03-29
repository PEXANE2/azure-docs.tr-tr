---
title: Güvenlik
titleSuffix: Azure Cognitive Services
description: Bilişsel Hizmetler kullanımı için çeşitli güvenlik konuları hakkında bilgi edinin.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: conceptual
ms.date: 03/23/2020
ms.author: dapine
ms.openlocfilehash: c86d806c408c2e8226e632a0b15e1e8729c987f9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80131526"
---
# <a name="azure-cognitive-services-security"></a>Azure Bilişsel Hizmetler güvenliği

Güvenlik, tüm uygulamaları geliştirirken en önemli öncelik olarak kabul edilmelidir. Yapay zeka nın etkin uygulamalarının başlangıcıyla güvenlik daha da önemlidir. Bu makalede, Azure Bilişsel Hizmetler güvenliğinin aktarım katmanı güvenliğinin kullanımı, kimlik doğrulama ve hassas verilerin güvenli bir şekilde yapılandırılması gibi çeşitli yönleri özetlenmiştir.

## <a name="transport-layer-security-tls"></a>Aktarım Katmanı Güvenliği (TLS)

HTTP üzerinden açığa çıkan Tüm Bilişsel Hizmetler uç noktaları TLS 1.2'yi uygular. Zorunlu bir güvenlik protokolü ile, Bilişsel Hizmetler bitiş noktası çağırmaya çalışan tüketicilerin bu kurallara uyması gerekir:

* İstemci İşletim Sistemi (OS) TLS 1.2 desteklemek gerekir
* HTTP araması yapmak için kullanılan dil (ve platform) isteğin bir parçası olarak TLS 1.2'yi belirtmeli
  * Dil ve platforma bağlı olarak, TLS'nin belirtilmesi örtülü veya açık bir şekilde yapılır

.NET kullanıcıları <a href="https://docs.microsoft.com/dotnet/framework/network-programming/tls" target="_blank">için, Aktarım Katmanı <span class="docon docon-navigate-external x-hidden-focus"> </span>Güvenliği en iyi uygulamaları </a>düşünün.

## <a name="authentication"></a>Kimlik doğrulaması

Kimlik doğrulama tartışılırken, birkaç yaygın yanlış anlama vardır. Kimlik doğrulama ve yetkilendirme genellikle birbirleri için karıştırılır. Kimlik de güvenlik önemli bir bileşenidir. Kimlik, <a href="https://en.wikipedia.org/wiki/Principal_(computer_security)" target="_blank">bir asıl <span class="docon docon-navigate-external x-hidden-focus"> </span> </a>hakkında bilgi topluluğudur. Kimlik sağlayıcıları (IdP) kimlik doğrulama hizmetlerine kimlik sağlar. Kimlik doğrulama, kullanıcının kimliğini doğrulama eylemidir. Yetkilendirme, belirli bir kimlik için kaynaklara erişim hakları ve ayrıcalıklarının belirtimidir. Bilişsel Hizmetler teklifleri birkaç, rol tabanlı erişim kontrolü (RBAC) içerir. RBAC, el ile yönetim ilkeleriyle ilgili bazı törenleri basitleştirmek için kullanılabilir. Daha fazla ayrıntı için [Azure kaynakları için rol tabanlı erişim denetimine](../role-based-access-control/overview.md)bakın.

Abonelik anahtarları, erişim belirteçleri ve Azure Etkin Dizini (AAD) ile kimlik doğrulama hakkında daha fazla bilgi için <a href="https://docs.microsoft.com/azure/cognitive-services/authentication" target="_blank">Azure Bilişsel<span class="docon docon-navigate-external x-hidden-focus"></span>Hizmetleri'ne yönelik kimlik doğrulama isteklerini</a>görün.

## <a name="environment-variables-and-application-configuration"></a>Ortam değişkenleri ve uygulama yapılandırması

Ortam değişkenleri, belirli bir ortamda depolanan ad değeri çiftleridir. Hassas veriler için hardcoded değerleri kullanmanın daha güvenli bir alternatifi, ortam değişkenlerini kullanmaktır. Kodlanmış değerler güvenli değildir ve kaçınılmalıdır.

> [!CAUTION]
> Hassas **not** veriler için kodlanmış değerleri kullanmayın, bunu yapmak önemli bir güvenlik açığıdır.

> [!NOTE]
> Ortam değişkenleri düz metinde depolanırken, bir ortama yalıtılır. Bir ortam tehlikeye girerse, çevre ile değişkenler de tehlikeye girer.

### <a name="set-environment-variable"></a>Ortam değişkenini ayarlama

Ortam değişkenlerini ayarlamak için aşağıdaki komutlardan birini `ENVIRONMENT_VARIABLE_KEY` kullanın - `value` burada adlandırılmış anahtar ve ortam değişkeninde depolanan değerdir.

# <a name="command-line"></a>[Komut Satırı](#tab/command-line)

Değer göz önüne alındığında, kalıcı ortam değişkeni oluşturun ve atayın.

```CMD
:: Assigns the env var to the value
setx ENVIRONMENT_VARIABLE_KEY="value"
```

**Komut Komut Istemi'nin**yeni bir örneğinde, ortam değişkenini okuyun.

```CMD
:: Prints the env var value
echo %ENVIRONMENT_VARIABLE_KEY%
```

# <a name="powershell"></a>[Powershell](#tab/powershell)

Değer göz önüne alındığında, kalıcı ortam değişkeni oluşturun ve atayın.

```powershell
# Assigns the env var to the value
[System.Environment]::SetEnvironmentVariable('ENVIRONMENT_VARIABLE_KEY', 'value', 'User')
```

**Windows PowerShell**yeni bir örnekte, çevre değişkeni okuyun.

```powershell
# Prints the env var value
[System.Environment]::GetEnvironmentVariable('ENVIRONMENT_VARIABLE_KEY')
```

# <a name="bash"></a>[Bash](#tab/bash)

Değer göz önüne alındığında, kalıcı ortam değişkeni oluşturun ve atayın.

```Bash
# Assigns the env var to the value
echo export ENVIRONMENT_VARIABLE_KEY="value" >> /etc/environment && source /etc/environment
```

**Bash**yeni bir örnekte , çevre değişkeni okuyun.

```Bash
# Prints the env var value
echo "${ENVIRONMENT_VARIABLE_KEY}"

# Or use printenv:
# printenv ENVIRONMENT_VARIABLE_KEY
```

---

> [!TIP]
> Bir ortam değişkeni ayarladıktan sonra, yeni eklenen ortam değişkenlerinin kullanılabilir olduğundan emin olmak için tümleşik geliştirme ortamınızı (IDE) yeniden başlatın.

### <a name="get-environment-variable"></a>Ortam değişkenini alın

Bir ortam değişkenini elde etmek için belleğe okunması gerekir. Kullandığınız dile bağlı olarak, aşağıdaki kod parçacıklarını göz önünde bulundurun. Bu kod parçacıkları, çevre değişkeninin `ENVIRONMENT_VARIABLE_KEY` nasıl alınıp. `value`

# <a name="c"></a>[C #](#tab/csharp)

Daha fazla bilgi <a href="https://docs.microsoft.com/dotnet/api/system.environment.getenvironmentvariable" target="_blank"> `Environment.GetEnvironmentVariable` <span class="docon docon-navigate-external x-hidden-focus"> </span> </a>için bkz.

```csharp
using static System.Environment;

class Program
{
    static void Main()
    {
        // Get the named env var, and assign it to the value variable
        var value =
            GetEnvironmentVariable(
                "ENVIRONMENT_VARIABLE_KEY");
    }
}
```

# <a name="c"></a>[C++](#tab/cpp)

Daha fazla bilgi <a href="https://docs.microsoft.com/cpp/c-runtime-library/reference/getenv-wgetenv" target="_blank"> `getenv` <span class="docon docon-navigate-external x-hidden-focus"> </span> </a>için bkz.

```cpp
#include <stdlib.h>

int main()
{
    // Get the named env var, and assign it to the value variable
    auto value =
        getenv("ENVIRONMENT_VARIABLE_KEY");
}
```

# <a name="java"></a>[Java](#tab/java)

Daha fazla bilgi <a href="https://docs.oracle.com/javase/7/docs/api/java/lang/System.html#getenv(java.lang.String)" target="_blank"> `System.getenv` <span class="docon docon-navigate-external x-hidden-focus"> </span> </a>için bkz.

```java
import java.lang.*;

public class Program {
   public static void main(String[] args) throws Exception {
    // Get the named env var, and assign it to the value variable
    String value =
        System.getenv(
            "ENVIRONMENT_VARIABLE_KEY")
   }
}
```

# <a name="nodejs"></a>[Node.js](#tab/node-js)

Daha fazla bilgi <a href="https://nodejs.org/api/process.html#process_process_env" target="_blank"> `process.env` <span class="docon docon-navigate-external x-hidden-focus"> </span> </a>için bkz.

```javascript
// Get the named env var, and assign it to the value variable
const value =
    process.env.ENVIRONMENT_VARIABLE_KEY;
```

# <a name="python"></a>[Python](#tab/python)

Daha fazla bilgi <a href="https://docs.python.org/2/library/os.html#os.environ" target="_blank"> `os.environ` <span class="docon docon-navigate-external x-hidden-focus"> </span> </a>için bkz.

```python
import os

# Get the named env var, and assign it to the value variable
value = os.environ['ENVIRONMENT_VARIABLE_KEY']
```

# <a name="objective-c"></a>[Objective-C](#tab/objective-c)

Daha fazla bilgi <a href="https://developer.apple.com/documentation/foundation/nsprocessinfo/1417911-environment?language=objc" target="_blank"> `environment` <span class="docon docon-navigate-external x-hidden-focus"> </span> </a>için bkz.

```objectivec
// Get the named env var, and assign it to the value variable
NSString* value =
    [[[NSProcessInfo processInfo]environment]objectForKey:@"ENVIRONMENT_VARIABLE_KEY"];
```

---

## <a name="next-steps"></a>Sonraki adımlar

* Çeşitli [Bilişsel Hizmetleri](welcome.md) keşfedin
* [Bilişsel Hizmetler Sanal Ağlar](cognitive-services-virtual-networks.md) hakkında daha fazla bilgi edinin
