---
title: Azure dev Space ile çalışırken gizli dizileri yönetme
services: azure-dev-spaces
ms.date: 05/11/2018
ms.topic: conceptual
description: Azure’da kapsayıcılar ve mikro hizmetlerle hızlı Kubernetes geliştirme
keywords: Docker, Kubernetes, Azure, AKS, Azure Container Service, kapsayıcılar
ms.openlocfilehash: 49f53683b2499e790414d139dcb0bc0833005647
ms.sourcegitcommit: 653e9f61b24940561061bd65b2486e232e41ead4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2019
ms.locfileid: "74280001"
---
# <a name="how-to-manage-secrets-when-working-with-an-azure-dev-space"></a>Azure dev Space ile çalışırken gizli dizileri yönetme

Hizmetleriniz, veritabanları veya diğer güvenli Azure hizmetleri gibi belirli parolalar, bağlantı dizeleri ve diğer gizli dizileri gerektirebilir. Bu gizli dizi değerlerini yapılandırma dosyalarında ayarlayarak kodunuzda ortam değişkenleri olarak kullanılabilir hale getirebilirsiniz.  Bunların gizli dizi güvenliğinin tehlikeye uğramaması için dikkatli bir şekilde işlenmelidir.

Azure Dev Spaces, Azure Dev Spaces istemci araçları tarafından oluşturulan Helu grafiklerde gizli dizileri depolamak için iki önerilen ve kolaylaştırılmış seçenek sunar: values. dev. YAML dosyası ve satır içi doğrudan azds. YAML. Gizli dizileri, values. YAML biçiminde depolamak önerilmez. Bu makalede tanımlanan istemci araçları tarafından oluşturulan Helu grafikleri için iki yaklaşımdan daha fazla, kendi hele grafiğinizi oluşturursanız, gizli dizileri yönetmek ve depolamak için HELI grafiğini doğrudan kullanabilirsiniz.

## <a name="method-1-valuesdevyaml"></a>Yöntem 1: values. dev. YAML
1. Azure Dev Spaces için etkinleştirilen VS Code açın.
2. Var olan _azds. YAML_ ile aynı klasöre _values. dev. YAML_ adlı bir dosya ekleyin ve gizli anahtarınızı ve değerlerini aşağıdaki örnekte olduğu gibi tanımlayın:

    ```yaml
    secrets:
      redis:
        port: "6380"
        host: "contosodevredis.redis.cache.windows.net"
        key: "secretkeyhere"
    ```
     
3. _azds. YAML_ zaten varsa _. dev. YAML_ dosyasına başvuruyor. Farklı bir dosya adı tercih ederseniz Install. Values bölümünü güncelleştirin:

    ```yaml
    install:
      values:
      - values.dev.yaml?
      - secrets.dev.yaml?
    ```
 
4. Aşağıdaki örnekte olduğu gibi, hizmet kodunuzu bu gizli dizileri ortam değişkenleri olarak başvuracak şekilde değiştirin:

    ```
    var redisPort = process.env.REDIS_PORT
    var host = process.env.REDIS_HOST
    var theKey = process.env.REDIS_KEY
    ```
    
5. Kümenizde çalışan hizmetleri bu değişikliklerle güncelleştirin. Komut satırında komutunu çalıştırın:

    ```
    azds up
    ```
 
6. Seçim Komut satırından, bu gizli dizileri oluşturulmuş olduğunu kontrol edin:

      ```
      kubectl get secret --namespace default -o yaml 
      ```

7. Kaynak denetiminde gizli dizileri yürütmeyi önlemek için. _dev. YAML değerlerini_ _. gitignore_ dosyasına eklediğinizden emin olun.
 
 
## <a name="method-2-inline-directly-in-azdsyaml"></a>Yöntem 2: doğrudan AZD. YAML 'de satır Içi
1.  _AZD. YAML_'de, YAML bölümü yapılandırması/geliştirme/yüklemesi altındaki gizli dizileri ayarlayın. Gizli değerleri doğrudan girebilseniz de, _azds. YAML_ kaynak denetimine işaretlenmiş olduğundan bu önerilmez. Bunun yerine, "$PLACEHOLDER" sözdizimini kullanarak yer tutucuları ekleyin.

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
     
2.  _AZD. YAML_ile aynı klasörde bir _. env_ dosyası oluşturun. Standart anahtar = değer gösterimini kullanarak gizli dizileri girin. _. Env_ dosyasını kaynak denetimine teslim etmeyin. (Git tabanlı sürüm denetim sistemlerindeki kaynak denetiminden atlamak için, _. gitignore_ dosyasına ekleyin.) Aşağıdaki örnekte bir _. env_ dosyası gösterilmektedir:

    ```
    REDIS_PORT=3333
    REDIS_HOST=myredishost
    REDIS_KEY=myrediskey
    ```
2.  Aşağıdaki örnekte olduğu gibi, kodda bu gizli dizileri başvuracak şekilde hizmet kaynak kodunuzu değiştirin:

    ```
    var redisPort = process.env.REDIS_PORT
    var host = process.env.REDIS_HOST
    var theKey = process.env.REDIS_KEY
    ```
 
3.  Kümenizde çalışan hizmetleri bu değişikliklerle güncelleştirin. Komut satırında komutunu çalıştırın:

    ```
    azds up
    ```

4.  seçim Kubectl 'deki gizli dizileri görüntüle:

    ```
    kubectl get secret --namespace default -o yaml
    ```

## <a name="next-steps"></a>Sonraki adımlar

Bu yöntemlerle, artık bir veritabanına güvenli bir şekilde bağlanabilir, redin için bir Azure önbelleği veya güvenli Azure hizmetlerine erişebilirsiniz.
 
