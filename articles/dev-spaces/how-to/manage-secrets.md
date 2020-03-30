---
title: Azure Geliştirme Alanı ile çalışırken sırları yönetme
services: azure-dev-spaces
ms.date: 12/03/2019
ms.topic: conceptual
description: Azure Geliştirme Alanları ile uygulama geliştirirken Kubernetes sırlarını çalışma sırasında veya oluşturma zamanında nasıl kullanacağınızı öğrenin
keywords: Docker, Kubernetes, Azure, AKS, Azure Konteyner Hizmeti, konteynerler
ms.openlocfilehash: d9dd0de348612bbb3baf5fb351c1c9af1c228c1f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75438473"
---
# <a name="how-to-manage-secrets-when-working-with-an-azure-dev-space"></a>Azure Geliştirme Alanı ile çalışırken sırları yönetme

Hizmetleriniz, veritabanları veya diğer güvenli Azure hizmetleri gibi belirli parolalar, bağlantı dizeleri ve diğer sırlar gerektirebilir. Yapılandırma dosyalarında bu sırların değerlerini ayarlayarak, bunları kodunuzda ortam değişkenleri olarak kullanılabilir hale getirebilirsiniz.  Bu yapılandırma dosyaları, sırların güvenliğini tehlikeye atmamak için özenle ele alınmalıdır.

## <a name="storing-and-using-runtime-secrets"></a>Çalışma zamanı sırlarını depolama ve kullanma

Azure Dev Spaces, Azure Dev Spaces istemci aracı tarafından oluşturulan Miğfer grafiklerinde sırları `values.dev.yaml` depolamak için önerilen `azds.yaml`ve kolaylaştırılmış iki seçenek sunar: dosyada ve doğrudan satır da. Sırları saklamak tavsiye `values.yaml`edilmez.

> [!NOTE]
> Aşağıdaki yaklaşımlar, istemci aracı tarafından oluşturulan Miğfer grafikleri için sırları nasıl depolayıp kullanacağınızı gösterir. Kendi Miğfer grafiğinizi oluşturursanız, sırları yönetmek ve depolamak için doğrudan Helm grafiğini kullanabilirsiniz.

### <a name="using-valuesdevyaml"></a>values.dev.yaml kullanma

Azure Geliştirme Alanları ile önceden hazırladığınız bir `values.dev.yaml` projede, gizli `azds.yaml` anahtarlarınızı ve değerlerinizi tanımlamak için aynı klasörde bir dosya oluşturun. Örnek:

```yaml
secrets:
  redis:
    port: "6380"
    host: "contosodevredis.redis.cache.windows.net"
    key: "secretkeyhere"
```

Dosya `azds.yaml` başvurularını `values.dev.yaml` isteğe `?`bağlı olarak doğrulayın. Örnek:

```yaml
install:
  values:
  - values.dev.yaml?
  - secrets.dev.yaml?
```

Ek gizli dosyalarınız varsa, bunları buraya da ekleyebilirsiniz.

Hizmetinizin sırlarınızı çevre değişkeni olarak güncelleştirin veya doğrulayın. Örnek:

```javascript
var redisPort = process.env.REDIS_PORT
var host = process.env.REDIS_HOST
var theKey = process.env.REDIS_KEY
```
    
Güncelleştirilmiş hizmetlerinizi `azds up`kullanarak çalıştırın.

```console
azds up
```
 
Sırlarınızın oluşturulduğunu doğrulamak için kullanın. `kubectl`

```console
kubectl get secret --namespace default -o yaml 
```

> [!IMPORTANT]
> Sırları kaynak kontrolünde depolamak önerilmez. Git kullanıyorsanız, `values.dev.yaml` kaynak `.gitignore` denetiminde sırlar işlemekten kaçınmak için dosyaya ekleyin.

### <a name="using-azdsyaml"></a>azds.yaml kullanma

Azure Dev Spaces ile önceden hazırladığınız bir projede `azds.yaml` *configurations.develop.install.set* altında *$PLACEHOLDER* sözdizimini kullanarak gizli anahtarlar ve değer ekleyin. Örnek:

```yaml
configurations:
  develop:
    ...
    install:
      set:
        secrets:
          redis:
            port: "$REDIS_PORT"
            host: "$REDIS_HOST"
            key: "$REDIS_KEY"
```

> [!NOTE]
> Gizli değerleri *$PLACEHOLDER* sözdizimini kullanmadan `azds.yaml`doğrudan girebilirsiniz. Ancak, kaynak denetiminde `azds.yaml` depolanan bu yana bu yaklaşım önerilmez.
     
$PLACEHOLDER `.env` değerlerinizi tanımlamak `azds.yaml` için aynı *$PLACEHOLDER* klasörde bir dosya oluşturun. Örnek:

```
REDIS_PORT=3333
REDIS_HOST=myredishost
REDIS_KEY=myrediskey
```

> [!IMPORTANT]
> Sırları kaynak kontrolünde depolamak önerilmez. Git kullanıyorsanız, `.env` kaynak `.gitignore` denetiminde sırlar işlemekten kaçınmak için dosyaya ekleyin.

Hizmetinizin sırlarınızı çevre değişkeni olarak güncelleştirin veya doğrulayın. Örnek:

```javascript
var redisPort = process.env.REDIS_PORT
var host = process.env.REDIS_HOST
var theKey = process.env.REDIS_KEY
```
    
Güncelleştirilmiş hizmetlerinizi `azds up`kullanarak çalıştırın.

```console
azds up
```
 
Sırlarınızın oluşturulduğunu doğrulamak için kullanın. `kubectl`

```console
kubectl get secret --namespace default -o yaml 
```

## <a name="using-secrets-as-build-arguments"></a>Sırları bağımsız değişken oluşturma olarak kullanma

Önceki bölümde, kapsayıcı çalışma zamanında kullanmak üzere sırların nasıl depolanır ve kullanılacağı gösterilmektedir. Ayrıca, özel bir NuGet için bir şifre gibi kapsayıcı oluşturma `azds.yaml`zamanında herhangi bir sırrı kullanabilirsiniz.

In `azds.yaml`, yapılandırmaları yapı zaman sırlarını *ayarlayın.develop.build.args* `<variable name>: ${secret.<secret name>.<secret key>}` sözdizimi kullanarak. Örnek:

```yaml
configurations:
  develop:
    build:
      dockerfile: Dockerfile.develop
      useGitIgnore: true
      args:
        BUILD_CONFIGURATION: ${BUILD_CONFIGURATION:-Debug}
        MYTOKEN: ${secret.mynugetsecret.pattoken}
```

Yukarıdaki örnekte, *mynugetsecret* mevcut bir sır ve *pattoken* varolan bir anahtardır.

>[!NOTE]
> Gizli adlar ve `.` anahtarlar karakteri içerebilir. Argümanlar `\` oluşturmak `.` gibi sırları geçerken kaçmak için kullanın. Örneğin, *jeton*tuşu ile *foo.bar* adlı bir `MYTOKEN: ${secret.foo\.bar.token}`sırrı geçirmek için: . Buna ek olarak, sırlar önek ve postfix metni ile değerlendirilebilir. Örneğin, `MYURL: eus-${secret.foo\.bar.token}-version1`. Ayrıca, ebeveyn ve büyükbaba alanlarda mevcut sırları inşa argümanlar olarak geçirilebilir.

Dockerfile'nizde, sırrı kullanmak için *ARG* yönergesini kullanın ve daha sonra Dockerfile'da aynı değişkeni kullanın. Örnek:

```dockerfile
...
ARG MYTOKEN
...
ARG NUGET_EXTERNAL_FEED_ENDPOINTS="{'endpointCredentials': [{'endpoint':'PRIVATE_NUGET_ENDPOINT', 'password':'${MYTOKEN}'}]}"
...
```

Kümenizde çalışan hizmetleri bu değişikliklerle güncelleştirin. Komut satırında, komutu çalıştırın:

```
azds up
```

## <a name="next-steps"></a>Sonraki adımlar

Bu yöntemlerle artık bir veritabanına, Redis için bir Azure Önbelleğine güvenli bir şekilde bağlanabilir veya güvenli Azure hizmetlerine erişebilirsiniz.
 
