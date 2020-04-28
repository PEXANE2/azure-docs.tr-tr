---
title: Azure dev Space ile çalışırken gizli dizileri yönetme
services: azure-dev-spaces
ms.date: 12/03/2019
ms.topic: conceptual
description: Azure Dev Spaces ile uygulama geliştirirken Kubernetes gizliliklerini çalışma veya oluşturma zamanında nasıl kullanacağınızı öğrenin
keywords: Docker, Kubernetes, Azure, AKS, Azure Container Service, kapsayıcılar
ms.openlocfilehash: d9dd0de348612bbb3baf5fb351c1c9af1c228c1f
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "75438473"
---
# <a name="how-to-manage-secrets-when-working-with-an-azure-dev-space"></a>Azure dev Space ile çalışırken gizli dizileri yönetme

Hizmetleriniz, veritabanları veya diğer güvenli Azure hizmetleri gibi belirli parolalar, bağlantı dizeleri ve diğer gizli dizileri gerektirebilir. Bu gizli dizi değerlerini yapılandırma dosyalarında ayarlayarak kodunuzda ortam değişkenleri olarak kullanılabilir hale getirebilirsiniz.  Bu yapılandırma dosyaları, gizli dizi güvenliğinin tehlikeye uğramaması için dikkatli bir şekilde işlenmelidir.

## <a name="storing-and-using-runtime-secrets"></a>Çalışma zamanı gizli dizilerini depolama ve kullanma

Azure Dev Spaces, Azure Dev Spaces istemci araçları tarafından oluşturulan Helu grafiklerde gizli dizileri depolamak için önerilen iki ve kolay seçenek sunar: `values.dev.yaml` dosyada, satır içinde doğrudan içinde. `azds.yaml` Gizli dizileri ' de `values.yaml`depolamak önerilmez.

> [!NOTE]
> Aşağıdaki yaklaşımlar, istemci araçları tarafından oluşturulan Held grafikleri için gizli dizileri nasıl depolayıp kullanacağınızı göstermektedir. Kendi hele grafiğinizi oluşturursanız, gizli dizileri yönetmek ve depolamak için HELI grafiğini doğrudan kullanabilirsiniz.

### <a name="using-valuesdevyaml"></a>Values. dev. YAML kullanma

Azure Dev Spaces önceden hazırladığınız bir projede, gizli anahtar ve değerlerinizi tanımlamak `values.dev.yaml` `azds.yaml` için aynı klasörde bir dosya oluşturun. Örneğin:

```yaml
secrets:
  redis:
    port: "6380"
    host: "contosodevredis.redis.cache.windows.net"
    key: "secretkeyhere"
```

Kullanarak `azds.yaml` dosya başvurularını `values.dev.yaml` isteğe bağlı olarak doğrulayın. `?` Örneğin:

```yaml
install:
  values:
  - values.dev.yaml?
  - secrets.dev.yaml?
```

Ek gizli dosyalarınız varsa, bunları buraya da ekleyebilirsiniz.

Hizmet bilgilerinizi, ortam değişkenleri olarak gizli dizilerinizdeki başvuruya göre güncelleştirin veya doğrulayın. Örneğin:

```javascript
var redisPort = process.env.REDIS_PORT
var host = process.env.REDIS_HOST
var theKey = process.env.REDIS_KEY
```
    
Kullanarak `azds up`güncelleştirilmiş hizmetlerinizi çalıştırın.

```console
azds up
```
 
Gizli `kubectl` dizilerinizi oluşturulmuş olduklarını doğrulamak için kullanın.

```console
kubectl get secret --namespace default -o yaml 
```

> [!IMPORTANT]
> Gizli dizileri kaynak denetiminde depolamak önerilmez. Git kullanıyorsanız, kaynak denetiminde `values.dev.yaml` gizli dizileri `.gitignore` yürütmeyi önlemek için dosyaya ekleyin.

### <a name="using-azdsyaml"></a>Azds. YAML kullanma

Azure Dev Spaces önceden hazırladığınız bir projede, *Konfigürasyonlar. Install. Install. set* `azds.yaml`altında *$PlaceHolder* söz dizimini kullanarak gizli anahtar ve değer ekleyin. Örneğin:

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
> İçinde `azds.yaml` *$PlaceHolder* sözdizimini kullanmadan doğrudan gizli değerleri girebilirsiniz. Ancak, bu yaklaşım kaynak denetiminde depolandığından `azds.yaml` önerilmez.
     
$PLACEHOLDER değerlerinizi tanımlamak `.env` `azds.yaml` için aynı klasörde bir dosya oluşturun. *$PLACEHOLDER* Örneğin:

```
REDIS_PORT=3333
REDIS_HOST=myredishost
REDIS_KEY=myrediskey
```

> [!IMPORTANT]
> Gizli dizileri kaynak denetiminde depolamak önerilmez. Git kullanıyorsanız, kaynak denetiminde `.env` gizli dizileri `.gitignore` yürütmeyi önlemek için dosyaya ekleyin.

Hizmet bilgilerinizi, ortam değişkenleri olarak gizli dizilerinizdeki başvuruya göre güncelleştirin veya doğrulayın. Örneğin:

```javascript
var redisPort = process.env.REDIS_PORT
var host = process.env.REDIS_HOST
var theKey = process.env.REDIS_KEY
```
    
Kullanarak `azds up`güncelleştirilmiş hizmetlerinizi çalıştırın.

```console
azds up
```
 
Gizli `kubectl` dizilerinizi oluşturulmuş olduklarını doğrulamak için kullanın.

```console
kubectl get secret --namespace default -o yaml 
```

## <a name="using-secrets-as-build-arguments"></a>Derleme bağımsız değişkenleri olarak gizli dizileri kullanma

Önceki bölümde, kapsayıcının çalışma zamanında kullanılmak üzere parolaların nasıl depolandığı ve kullanılacağı gösterildi. Ayrıca, kullanarak `azds.yaml`özel bir NuGet için parola gibi kapsayıcı oluşturma zamanında herhangi bir gizli dizi da kullanabilirsiniz.

İçinde `azds.yaml`, yapılandırmada derleme zamanı gizli dizilerini ayarlayın *. Build. Build. args* `<variable name>: ${secret.<secret name>.<secret key>}` sözdizimini kullanarak. Örneğin:

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

Yukarıdaki örnekte, *mynugetsecret* var olan bir sır ve *pattoken* var olan bir anahtardır.

>[!NOTE]
> Gizli adlar ve anahtarlar `.` karakteri içerebilir. Gizli `\` dizileri yapı `.` bağımsız değişkenleri olarak geçirirken kaçış için kullanın. Örneğin, *belirteç*anahtarıyla `MYTOKEN: ${secret.foo\.bar.token}` *foo. Bar* adlı bir gizli anahtar geçirmek için:. Ayrıca, gizlilikler önek ve sonek metniyle değerlendirilebilir. Örneğin, `MYURL: eus-${secret.foo\.bar.token}-version1`. Ayrıca, üst ve alt üst alanlarda bulunan gizlilikler yapı bağımsız değişkenleri olarak geçirilebilir.

Dockerfile 'da, parolayı kullanmak için *arg* yönergesini kullanın, daha sonra dockerfile içinde bu değişkeni kullanın. Örneğin:

```dockerfile
...
ARG MYTOKEN
...
ARG NUGET_EXTERNAL_FEED_ENDPOINTS="{'endpointCredentials': [{'endpoint':'PRIVATE_NUGET_ENDPOINT', 'password':'${MYTOKEN}'}]}"
...
```

Kümenizde çalışan hizmetleri bu değişikliklerle güncelleştirin. Komut satırında komutunu çalıştırın:

```
azds up
```

## <a name="next-steps"></a>Sonraki adımlar

Bu yöntemlerle, artık bir veritabanına güvenli bir şekilde bağlanabilir, redin için bir Azure önbelleği veya güvenli Azure hizmetlerine erişebilirsiniz.
 
