---
title: Güvenlik
titleSuffix: Azure Cognitive Services
description: Bilişsel hizmetler kullanımıyla ilgili çeşitli güvenlik konuları hakkında bilgi edinin.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: conceptual
ms.date: 05/11/2020
ms.author: dapine
ms.openlocfilehash: fa0ad8c7f75a977e1a39ff6ffd6fee08d977f57a
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2020
ms.locfileid: "83202005"
---
# <a name="azure-cognitive-services-security"></a>Azure bilişsel hizmetler güvenliği

Güvenlik, tüm uygulamalar geliştirilirken en iyi öncelik olarak düşünülmelidir. Yapay zeka etkin uygulamalar içeren başladıkları noktada ile güvenlik daha da önemlidir. Bu makalede, Aktarım Katmanı Güvenliği, kimlik doğrulama, hassas verileri güvenli şekilde yapılandırma ve müşteri verileri erişimi için Müşteri Kasası gibi çeşitli Azure bilişsel hizmetler güvenliği konuları özetlenmektedir.

## <a name="transport-layer-security-tls"></a>Aktarım Katmanı Güvenliği (TLS)

HTTP üzerinden sunulan bilişsel hizmetler uç noktaları, TLS 1,2 ' i zorlar. Zorunlu bir güvenlik protokolüyle, bilişsel hizmetler uç noktasını çağırmaya çalışan tüketiciler şu yönergelere uymalıdır:

* İstemci Işletim sisteminin (OS) TLS 1,2 ' i desteklemesi gerekir
* HTTP çağrısının isteğin bir parçası olarak TLS 1,2 belirtmesini sağlamak için kullanılan dil (ve platform)
  * Dile ve platforma bağlı olarak, TLS belirtmek örtük olarak veya açık olarak yapılır

.NET kullanıcıları için, <a href="https://docs.microsoft.com/dotnet/framework/network-programming/tls" target="_blank">Aktarım Katmanı Güvenliği en iyi yöntemlerini <span class="docon docon-navigate-external x-hidden-focus"></span> </a>göz önünde bulundurun.

## <a name="authentication"></a>Kimlik Doğrulaması

Kimlik doğrulaması tartışırken, yaygın olarak karşılaşılan birkaç yanlış rastlamalar vardır. Kimlik doğrulama ve yetkilendirme genellikle bir diğeri için karıştırılır. Kimlik ayrıca güvenlik ' de bir ana bileşendir. Kimlik, bir <a href="https://en.wikipedia.org/wiki/Principal_(computer_security)" target="_blank">sorumlu <span class="docon docon-navigate-external x-hidden-focus"></span> </a>hakkındaki bilgilerin koleksiyonudur. Kimlik sağlayıcıları (IDP) kimlik doğrulama hizmetlerine kimlikler sağlar. Kimlik doğrulaması, bir kullanıcının kimliğini doğrulama işlemidir. Yetkilendirme, belirli bir kimlik için kaynaklara yönelik erişim haklarının ve ayrıcalıkların belirtiledir. Bilişsel hizmetler tekliflerinden bazıları rol tabanlı erişim denetimi (RBAC) içerir. RBAC, sorumluları el ile yönetme ile ilgili bazı sertifika basitleştirecek şekilde kullanılabilir. Daha fazla ayrıntı için bkz. [Azure kaynakları için rol tabanlı erişim denetimi](../role-based-access-control/overview.md).

Abonelik anahtarları, erişim belirteçleri ve Azure Active Directory (AAD) ile kimlik doğrulaması hakkında daha fazla bilgi için bkz. Azure bilişsel <a href="https://docs.microsoft.com/azure/cognitive-services/authentication" target="_blank">Hizmetler <span class="docon docon-navigate-external x-hidden-focus"></span> 'e yönelik kimlik doğrulama istekleri</a>.

## <a name="environment-variables-and-application-configuration"></a>Ortam değişkenleri ve uygulama yapılandırması

Ortam değişkenleri, belirli bir ortam içinde depolanan ad-değer çiftleridir. Hassas veriler için sabit kodlanmış değerler kullanmanın daha güvenli bir alternatifi, ortam değişkenlerini kullanmaktır. Sabit kodlanmış değerler güvenli değildir ve kaçınılmalıdır.

> [!CAUTION]
> Hassas veriler için sabit kodlanmış **değerler kullanmayın,** bunun yapılması önemli bir güvenlik açığıdır.

> [!NOTE]
> Ortam değişkenleri düz metin olarak depolanırken, ortamları bir ortama yalıtılmıştır. Bir ortamın güvenliği tehlikeye girerse, ortam ile değişkenler de vardır.

### <a name="set-environment-variable"></a>Ortam değişkenini ayarla

Ortam değişkenlerini ayarlamak için aşağıdaki komutlardan birini kullanın; burada, `ENVIRONMENT_VARIABLE_KEY` adlandırılmış anahtardır ve `value` ortam değişkeninde depolanan değerdir.

# <a name="command-line"></a>[Komut satırı](#tab/command-line)

Değer verilen kalıcı ortam değişkeni oluşturun ve atayın.

```CMD
:: Assigns the env var to the value
setx ENVIRONMENT_VARIABLE_KEY="value"
```

**Komut isteminin**yeni bir örneğinde, ortam değişkenini okuyun.

```CMD
:: Prints the env var value
echo %ENVIRONMENT_VARIABLE_KEY%
```

# <a name="powershell"></a>[PowerShell](#tab/powershell)

Değer verilen kalıcı ortam değişkeni oluşturun ve atayın.

```powershell
# Assigns the env var to the value
[System.Environment]::SetEnvironmentVariable('ENVIRONMENT_VARIABLE_KEY', 'value', 'User')
```

**Windows PowerShell**'in yeni bir örneğinde, ortam değişkenini okuyun.

```powershell
# Prints the env var value
[System.Environment]::GetEnvironmentVariable('ENVIRONMENT_VARIABLE_KEY')
```

# <a name="bash"></a>[Bash](#tab/bash)

Değer verilen kalıcı ortam değişkeni oluşturun ve atayın.

```Bash
# Assigns the env var to the value
echo export ENVIRONMENT_VARIABLE_KEY="value" >> /etc/environment && source /etc/environment
```

**Bash**'in yeni bir örneğinde, ortam değişkenini okuyun.

```Bash
# Prints the env var value
echo "${ENVIRONMENT_VARIABLE_KEY}"

# Or use printenv:
# printenv ENVIRONMENT_VARIABLE_KEY
```

---

> [!TIP]
> Bir ortam değişkenini ayarladıktan sonra, yeni eklenen ortam değişkenlerinin kullanılabilir olduğundan emin olmak için tümleşik geliştirme ortamınızı (IDE) yeniden başlatın.

### <a name="get-environment-variable"></a>Ortam değişkenini al

Bir ortam değişkenini almak için, belleğin belleğine okunmalıdır. Kullanmakta olduğunuz dile bağlı olarak, aşağıdaki kod parçacıklarını göz önünde bulundurun. Bu kod parçacıkları, `ENVIRONMENT_VARIABLE_KEY` adlı bir değişkene öğesine verilen ve atayan ortam değişkeninin nasıl alınacağını gösterir `value` .

# <a name="c"></a>[, #](#tab/csharp)

Daha fazla bilgi için bkz <a href="https://docs.microsoft.com/dotnet/api/system.environment.getenvironmentvariable" target="_blank"> `Environment.GetEnvironmentVariable` <span class="docon docon-navigate-external x-hidden-focus"></span> </a>..

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

Daha fazla bilgi için bkz <a href="https://docs.microsoft.com/cpp/c-runtime-library/reference/getenv-wgetenv" target="_blank"> `getenv` <span class="docon docon-navigate-external x-hidden-focus"></span> </a>..

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

Daha fazla bilgi için bkz <a href="https://docs.oracle.com/javase/7/docs/api/java/lang/System.html#getenv(java.lang.String)" target="_blank"> `System.getenv` <span class="docon docon-navigate-external x-hidden-focus"></span> </a>..

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

Daha fazla bilgi için bkz <a href="https://nodejs.org/api/process.html#process_process_env" target="_blank"> `process.env` <span class="docon docon-navigate-external x-hidden-focus"></span> </a>..

```javascript
// Get the named env var, and assign it to the value variable
const value =
    process.env.ENVIRONMENT_VARIABLE_KEY;
```

# <a name="python"></a>[Python](#tab/python)

Daha fazla bilgi için bkz <a href="https://docs.python.org/2/library/os.html#os.environ" target="_blank"> `os.environ` <span class="docon docon-navigate-external x-hidden-focus"></span> </a>..

```python
import os

# Get the named env var, and assign it to the value variable
value = os.environ['ENVIRONMENT_VARIABLE_KEY']
```

# <a name="objective-c"></a>[Objective-C](#tab/objective-c)

Daha fazla bilgi için bkz <a href="https://developer.apple.com/documentation/foundation/nsprocessinfo/1417911-environment?language=objc" target="_blank"> `environment` <span class="docon docon-navigate-external x-hidden-focus"></span> </a>..

```objectivec
// Get the named env var, and assign it to the value variable
NSString* value =
    [[[NSProcessInfo processInfo]environment]objectForKey:@"ENVIRONMENT_VARIABLE_KEY"];
```

---

## <a name="customer-lockbox"></a>Müşteri Kasası

[Microsoft Azure için müşteri kasası](../security/fundamentals/customer-lockbox-overview.md) , müşterilerin müşteri verileri erişim isteklerini gözden geçirmesi ve onaylaması için bir arabirim sağlar. Bir Microsoft mühendisinin bir destek isteği sırasında müşteri verilerine erişmesi gereken durumlarda kullanılır. Müşteri Kasası isteklerinin nasıl başlatıldığı, izlendiği ve daha sonraki incelemeler ve denetimleri için depolandığı hakkında daha fazla bilgi için bkz. [müşteri kasası](../security/fundamentals/customer-lockbox-overview.md). 

Bu bilişsel hizmet için Müşteri Kasası kullanılabilir:

* Çevirmen

Language Understanding için, Microsoft mühendisleri E0 SKU 'sunda hiçbir müşteri verilerine erişemez. E0 SKU 'YU kullanma yeteneği istemek için, [Lusıs hizmet Isteği formunu](https://aka.ms/cogsvc-cmk)doldurun ve bu hizmeti gönderebilirsiniz. İsteğinizin durumunu öğrenmek yaklaşık 3-5 iş günü sürer. Talebe bağlı olarak, bir kuyruğa yerleştirilmiş ve alan kullanılabilir olduğunda onaylanmış olabilir. E0 SKU 'SU LUIN ile kullanılması onaylandıktan sonra, Azure portal Language Understanding yeni bir kaynak oluşturmanız ve fiyatlandırma katmanı olarak E0 ' ı seçmeniz gerekir. Kullanıcılar F0 'den yeni E0 SKU 'suna yükseltemez.

Konuşma hizmeti şu anda Müşteri Kasası desteklememektedir. Ancak, müşteri verileri KCG kullanılarak depolanabilir ve [müşteri kasası](../security/fundamentals/customer-lockbox-overview.md)için benzer veri denetimleri elde etmenizi sağlar. Konuşma hizmeti verilerinin, konuşma kaynağının oluşturulduğu bölgede kaldığından ve işlendiğini aklınızda bulundurun. Bu, bekleyen veriler ve iletim verileri için geçerlidir. Özel Konuşma Tanıma ve özel ses gibi özelleştirme özellikleri kullanılırken, tüm müşteri verileri, KCG (kullanılıyorsa) ve konuşma hizmeti kaynağının bulunduğu bölgede aktarılır, depolanır ve işlenir.

> [!IMPORTANT]
> Microsoft, konuşma modellerini geliştirmek için müşteri **verilerini kullanmaz.** Ayrıca, uç nokta günlüğü devre dışıysa ve hiçbir özelleştirme kullanılmazsa, hiçbir müşteri verisi depolanmaz. 

## <a name="next-steps"></a>Sonraki adımlar

* Çeşitli bilişsel [Hizmetleri](welcome.md) inceleyin
* Bilişsel [Hizmetler sanal ağları](cognitive-services-virtual-networks.md) hakkında daha fazla bilgi edinin
