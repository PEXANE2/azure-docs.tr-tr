---
title: Azure Container Registry görevleri başvurusu-YAML
description: Görev özellikleri, adım türleri, adım özellikleri ve yerleşik değişkenler de dahil olmak üzere ACR görevleri için YAML görevlerini tanımlamaya yönelik başvuru.
services: container-registry
author: dlepow
manager: gwallace
ms.service: container-registry
ms.topic: article
ms.date: 03/28/2019
ms.author: danlep
ms.openlocfilehash: 588c4c267c16c72a7673c09a4c5726058302fccb
ms.sourcegitcommit: f5075cffb60128360a9e2e0a538a29652b409af9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2019
ms.locfileid: "68310504"
---
# <a name="acr-tasks-reference-yaml"></a>ACR görevleri başvurusu: YAML

ACR görevlerinde çok adımlı görev tanımı, kapsayıcıları oluşturmaya, teste ve düzeltme eki uygulamaya odaklanan kapsayıcı merkezli bir işlem temel alanı sağlar. Bu makalede, çok adımlı görevlerinizi tanımlayan YAML dosyalarının komutları, parametreleri, özellikleri ve söz dizimi ele alınmaktadır.

Bu makale, ACR görevleri için çok adımlı görev YAML dosyaları oluşturmak için başvuru içerir. ACR görevlerine giriş istiyorsanız [ACR görevlerine genel bakış ' a](container-registry-tasks-overview.md)bakın.

## <a name="acr-taskyaml-file-format"></a>ACR-Task. YAML dosya biçimi

ACR görevleri standart YAML sözdiziminde çok adımlı görev bildirimini destekler. Bir YAML dosyasında bir görevin adımlarını tanımlarsınız. Sonra dosyayı bir git işlemesinde veya temel görüntü güncelleştirmesinde otomatik olarak tetiklenen [az ACR çalıştırmasına][az-acr-run] command. Or, use the file to create a task with [az acr task create][az-acr-task-create] geçirerek görevi el ile çalıştırabilirsiniz. Bu makale, adımları içeren `acr-task.yaml` dosya olarak ifade edilse de, ACR görevleri [desteklenen bir uzantıya](#supported-task-filename-extensions)sahip tüm geçerli dosya adını destekler.

`acr-task.yaml` En üst düzey temel elemanlar **görev özellikleri**, **adım türleri**ve **adım özelliklerdir**:

* Görev [özellikleri](#task-properties) , görev yürütme boyunca tüm adımlara uygulanır. Aşağıdakiler dahil olmak üzere birkaç genel görev özelliği vardır:
  * `version`
  * `stepTimeout`
  * `workingDirectory`
* [Görev adımı türleri](#task-step-types) , bir görevde gerçekleştirilebilecek eylem türlerini temsil eder. Üç adım türü vardır:
  * `build`
  * `push`
  * `cmd`
* [Görev adımı özellikleri](#task-step-properties) , tek bir adıma uygulanan parametrelerdir. Aşağıdakiler dahil olmak üzere birkaç adım özelliği vardır:
  * `startDelay`
  * `timeout`
  * `when`
  * ... ve çok daha fazlası.

Bazı yaygın adım özellikleri de `acr-task.yaml` dahil olmak üzere bir dosyanın temel biçimi aşağıdaki gibidir. Tüm kullanılabilir adım özelliklerinin veya adım türü kullanımının ayrıntılı bir gösterimi olmasa da temel dosya biçimine hızlı bir genel bakış sağlar.

```yml
version: # acr-task.yaml format version.
stepTimeout: # Seconds each step may take.
steps: # A collection of image or container actions.
  - build: # Equivalent to "docker build," but in a multi-tenant environment
  - push: # Push a newly built or retagged image to a registry.
    when: # Step property that defines either parallel or dependent step execution.
  - cmd: # Executes a container, supports specifying an [ENTRYPOINT] and parameters.
    startDelay: # Step property that specifies the number of seconds to wait before starting execution.
```

### <a name="supported-task-filename-extensions"></a>Desteklenen görev dosya adı uzantıları

ACR görevleri, `.yaml`bir görev dosyası olarak işlem yaptığı gibi birkaç dosya adı uzantısını ayırmıştır. Aşağıdaki listede *olmayan* herhangi bir uzantı, ACR görevlerinin bir Dockerfile olarak değerlendirilir:. YAML,. yml,. TOML,. JSON,. sh,. Bash,. ZSH,. ps1,. PS,. cmd,. bat,. TS,. js,. php,.,. RB,. lua

YAML, şu anda ACR görevleri tarafından desteklenen tek dosya biçimidir. Diğer dosya adı uzantıları olası gelecekteki destek için ayrılmıştır.

## <a name="run-the-sample-tasks"></a>Örnek görevleri çalıştırma

Bu makalenin aşağıdaki bölümlerinde başvurulan birkaç örnek görev dosyası vardır. Örnek görevler ortak bir GitHub deposunda, [Azure-Samples/ACR-Tasks][acr-tasks]. You can run them with the Azure CLI command [az acr run][az-acr-run]' dadır. Örnek komutlar şuna benzerdir:

```azurecli
az acr run -f build-push-hello-world.yaml https://github.com/Azure-Samples/acr-tasks.git
```

Örnek komutların biçimlendirmesi, Azure CLI 'de varsayılan bir kayıt defteri yapılandırdığınız ve bu nedenle `--registry` parametreyi attıkları varsayılır. Varsayılan bir kayıt defteri yapılandırmak için, bir `acr=REGISTRY_NAME` değeri kabul eden `--defaults` parametresiyle [az configure][az-configure] komutunu kullanın.

Örneğin, Azure CLı 'yı "myregistry" adlı varsayılan bir kayıt defteriyle yapılandırmak için:

```azurecli
az configure --defaults acr=myregistry
```

## <a name="task-properties"></a>Görev özellikleri

Görev özellikleri genellikle bir `acr-task.yaml` dosyanın en üstünde görünür ve görev adımlarının tam yürütülmesi boyunca uygulanan genel özelliklerdir. Bu genel özelliklerden bazıları, tek bir adım içinde geçersiz kılınabilir.

| Özellik | Type | İsteğe Bağlı | Açıklama | Geçersiz kılma destekleniyor | Varsayılan değer |
| -------- | ---- | -------- | ----------- | ------------------ | ------------- |
| `version` | dize | Evet | ACR görevler hizmeti `acr-task.yaml` tarafından ayrıştırılabilen dosyanın sürümü. ACR görevleri geriye dönük uyumluluğu sürdürmeye devam ederken, bu değer ACR görevlerinin tanımlı bir sürüm içinde uyumluluğu korumasına olanak tanır. Belirtilmemişse, varsayılan olarak en son sürümü alır. | Hayır | Yok. |
| `stepTimeout` | int (saniye) | Evet | Bir adımın çalıştırılacağı en fazla saniye sayısı. Özellik bir görevde belirtilmişse, tüm adımların varsayılan `timeout` özelliğini ayarlar. `timeout` Özellik bir adımda belirtilmişse, görev tarafından sunulan özelliği geçersiz kılar. | Evet | 600 (10 dakika) |
| `workingDirectory` | dize | Evet | Çalışma zamanı sırasında kapsayıcının çalışma dizini. Özellik bir görevde belirtilmişse, tüm adımların varsayılan `workingDirectory` özelliğini ayarlar. Bir adımda belirtilmişse, görev tarafından sunulan özelliği geçersiz kılar. | Evet | `$HOME` |
| `env` | [dize, dize,...] | Evet |  Görevin ortam değişkenlerini tanımlayan `key=value` biçimdeki dizeler dizisi. Özellik bir görevde belirtilmişse, tüm adımların varsayılan `env` özelliğini ayarlar. Bir adımda belirtilmişse, görevden devralınan tüm ortam değişkenlerini geçersiz kılar. | None |
| `secrets` | [gizli, gizli,...] | Evet | [Gizli](#secret) nesneler dizisi. | Yok. |
| `networks` | [Ağ, ağ,...] | Evet | [Ağ](#network) nesneleri dizisi. | Yok. |

### <a name="secret"></a>secret

Gizli nesne aşağıdaki özelliklere sahiptir.

| Özellik | Type | İsteğe Bağlı | Açıklama | Varsayılan değer |
| -------- | ---- | -------- | ----------- | ------- |
| `id` | dize | Hayır | Gizli dizi tanımlayıcısı. | Yok. |
| `keyvault` | dize | Evet | Azure Key Vault gizli URL 'SI. | Yok. |
| `clientID` | dize | Evet | Azure kaynakları için Kullanıcı tarafından atanan yönetilen kimliğin istemci KIMLIĞI. | Yok. |

### <a name="network"></a>Network

Ağ nesnesi aşağıdaki özelliklere sahiptir.

| Özellik | Type | İsteğe Bağlı | Açıklama | Varsayılan değer |
| -------- | ---- | -------- | ----------- | ------- | 
| `name` | dize | Hayır | Ağın adı. | None |
| `driver` | dize | Evet | Ağı yönetmek için sürücü. | Yok. |
| `ipv6` | bool | Evet | IPv6 ağ oluşturma özelliğinin etkin olup olmadığı. | `false` |
| `skipCreation` | bool | Evet | Ağ oluşturma atlanıp atlanmayacağı. | `false` |
| `isDefault` | bool | Evet | Ağın Azure Container Registry ile belirtilen bir varsayılan ağ olup olmadığı | `false` |

## <a name="task-step-types"></a>Görev adımı türleri

ACR görevleri üç adım türünü destekler. Her adım türü, her adım türü için bölümünde ayrıntılı olarak açıklanan birkaç özelliği destekler.

| Adım türü | Açıklama |
| --------- | ----------- |
| [`build`](#build) | Tanıdık `docker build` sözdizimi kullanarak bir kapsayıcı görüntüsü oluşturur. |
| [`push`](#push) | Yeni oluşturulmuş `docker push` veya retagged görüntülerinin bir kapsayıcı kayıt defterine yürütülür. Azure Container Registry, diğer özel kayıt defterleri ve genel Docker Hub desteklenir. |
| [`cmd`](#cmd) | Kapsayıcıyı kapsayıcının `[ENTRYPOINT]`kendisine geçirilmiş parametreler ile bir komut olarak çalıştırır. Adım türü, `env` `docker run` ve diğer tanıdık komut seçenekleri gibi parametreleri destekler, eşzamanlı kapsayıcı yürütmesi ile birim ve işlevsel testi etkinleştirir. `detach` `cmd` |

## <a name="build"></a>derlemeyi

Kapsayıcı görüntüsü oluşturun. Adım türü, bulutta ilk sınıf temel olarak çalıştırmanın `docker build` çok kiracılı, güvenli bir şekilde temsil eder. `build`

### <a name="syntax-build"></a>Sözdizimi: derleme

```yml
version: v1.0.0
steps:
  - [build]: -t [imageName]:[tag] -f [Dockerfile] [context]
    [property]: [value]
```

`build` Adım türü aşağıdaki tablodaki parametreleri destekler. Adım türü Ayrıca, oluşturma zamanı değişkenlerini ayarlamak `--build-arg` için gibi [Docker Build](https://docs.docker.com/engine/reference/commandline/build/) komutunun tüm derleme seçeneklerini de destekler. `build`

| Parametre | Açıklama | İsteğe Bağlı |
| --------- | ----------- | :-------: |
| `-t` &#124; `--image` | Oluşturulan görüntünün tam nitelikli `image:tag` olduğunu tanımlar.<br /><br />Görüntü, işlevsel testler gibi iç görev doğrulamaları için kullanılabilir, ancak tüm görüntülerin bir kayıt defterine ihtiyacı `push` yoktur. Ancak, bir görev yürütme içindeki bir görüntüyü örnek olarak, görüntünün başvuru için bir adı olması gerekir.<br /><br />Farklı `az acr build`olarak, ACR görevlerinin çalıştırılması varsayılan gönderme davranışı sağlamaz. ACR görevleri ile, varsayılan senaryo bir görüntüyü oluşturma, doğrulama ve gönderme özelliğini kabul eder. İsteğe bağlı olarak oluşturulan görüntüleri gönderme hakkında bilgi için bkz. [gönderim](#push) . | Evet |
| `-f` &#124; `--file` | Geçirilen Dockerfile dosyasını belirtir `docker build`. Belirtilmemişse, bağlam kökündeki varsayılan Dockerfile varsayılır. Bir Dockerfile belirtmek için, dosya adını bağlamın köküne göre geçirin. | Evet |
| `context` | Kök dizin geçildi `docker build`. Her görevin kök dizini paylaşılan bir [WorkingDirectory](#task-step-properties)olarak ayarlanır ve ilişkili git klonlanmış dizinin kökünü içerir. | Hayır |

### <a name="properties-build"></a>Özellikler: derleme

`build` Adım türü aşağıdaki özellikleri destekler. Bu makalenin [görev adımı özellikleri](#task-step-properties) bölümünde bu özelliklerin ayrıntılarını bulun.

| | | |
| -------- | ---- | -------- |
| `detach` | bool | İsteğe Bağlı |
| `disableWorkingDirectoryOverride` | bool | İsteğe Bağlı |
| `entryPoint` | dize | İsteğe Bağlı |
| `env` | [dize, dize,...] | İsteğe Bağlı |
| `expose` | [dize, dize,...] | İsteğe Bağlı |
| `id` | dize | İsteğe Bağlı |
| `ignoreErrors` | bool | İsteğe Bağlı |
| `isolation` | dize | İsteğe Bağlı |
| `keep` | bool | İsteğe Bağlı |
| `network` | object | İsteğe Bağlı |
| `ports` | [dize, dize,...] | İsteğe Bağlı |
| `pull` | bool | İsteğe Bağlı |
| `repeat` | int | İsteğe Bağlı |
| `retries` | int | İsteğe Bağlı |
| `retryDelay` | int (saniye) | İsteğe Bağlı |
| `secret` | object | İsteğe Bağlı |
| `startDelay` | int (saniye) | İsteğe Bağlı |
| `timeout` | int (saniye) | İsteğe Bağlı |
| `when` | [dize, dize,...] | İsteğe Bağlı |
| `workingDirectory` | dize | İsteğe Bağlı |

### <a name="examples-build"></a>Örnekler: derleme

#### <a name="build-image---context-in-root"></a>Derleme görüntüsü-kökte bağlam

```azurecli
az acr run -f build-hello-world.yaml https://github.com/AzureCR/acr-tasks-sample.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/build-hello-world.yaml -->
[!code-yml[task](~/acr-tasks/build-hello-world.yaml)]

#### <a name="build-image---context-in-subdirectory"></a>Alt dizinde derleme görüntüsü-bağlam

```yml
version: v1.0.0
steps:
  - build: -t {{.Run.Registry}}/hello-world -f hello-world.dockerfile ./subDirectory
```

## <a name="push"></a>hareketle

Bir veya daha fazla oluşturulmuş veya retagged görüntüsünü bir kapsayıcı kayıt defterine gönderin. Azure Container Registry veya genel Docker Hub 'ına gibi özel kayıt defterlerine göndermeyi destekler.

### <a name="syntax-push"></a>Sözdizimi: Push

`push` Adım türü bir görüntü koleksiyonunu destekler. YAML koleksiyon sözdizimi, satır içi ve iç içe biçimleri destekler. Tek bir görüntünün gönderilmesi genellikle satır içi sözdizimi kullanılarak temsil edilir:

```yml
version: v1.0.0
steps:
  # Inline YAML collection syntax
  - push: ["{{.Run.Registry}}/hello-world:{{.Run.ID}}"]
```

Daha fazla okunabilirlik için, birden çok görüntü gönderirken iç içe geçmiş sözdizimini kullanın:

```yml
version: v1.0.0
steps:
  # Nested YAML collection syntax
  - push:
    - {{.Run.Registry}}/hello-world:{{.Run.ID}}
    - {{.Run.Registry}}/hello-world:latest
```

### <a name="properties-push"></a>Özellikler: gönderim

`push` Adım türü aşağıdaki özellikleri destekler. Bu makalenin [görev adımı özellikleri](#task-step-properties) bölümünde bu özelliklerin ayrıntılarını bulun.

| | | |
| -------- | ---- | -------- |
| `env` | [dize, dize,...] | İsteğe Bağlı |
| `id` | dize | İsteğe Bağlı |
| `ignoreErrors` | bool | İsteğe Bağlı |
| `startDelay` | int (saniye) | İsteğe Bağlı |
| `timeout` | int (saniye) | İsteğe Bağlı |
| `when` | [dize, dize,...] | İsteğe Bağlı |

### <a name="examples-push"></a>Örnekler: Push

#### <a name="push-multiple-images"></a>Birden çok görüntü gönderme

```azurecli
az acr run -f build-push-hello-world.yaml https://github.com/Azure-Samples/acr-tasks.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/build-push-hello-world.yaml -->
[!code-yml[task](~/acr-tasks/build-push-hello-world.yaml)]

#### <a name="build-push-and-run"></a>Oluşturma, gönderme ve çalıştırma

```azurecli
az acr run -f build-run-hello-world.yaml https://github.com/Azure-Samples/acr-tasks.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/build-run-hello-world.yaml -->
[!code-yml[task](~/acr-tasks/build-run-hello-world.yaml)]

## <a name="cmd"></a>cmd

`cmd` Adım türü bir kapsayıcı çalıştırır.

### <a name="syntax-cmd"></a>Sözdizimi: cmd

```yml
version: v1.0.0
steps:
  - [cmd]: [containerImage]:[tag (optional)] [cmdParameters to the image]
```

### <a name="properties-cmd"></a>Özellikler: cmd

`cmd` Adım türü aşağıdaki özellikleri destekler:

| | | |
| -------- | ---- | -------- |
| `detach` | bool | İsteğe Bağlı |
| `disableWorkingDirectoryOverride` | bool | İsteğe Bağlı |
| `entryPoint` | dize | İsteğe Bağlı |
| `env` | [dize, dize,...] | İsteğe Bağlı |
| `expose` | [dize, dize,...] | İsteğe Bağlı |
| `id` | dize | İsteğe Bağlı |
| `ignoreErrors` | bool | İsteğe Bağlı |
| `isolation` | dize | İsteğe Bağlı |
| `keep` | bool | İsteğe Bağlı |
| `network` | object | İsteğe Bağlı |
| `ports` | [dize, dize,...] | İsteğe Bağlı |
| `pull` | bool | İsteğe Bağlı |
| `repeat` | int | İsteğe Bağlı |
| `retries` | int | İsteğe Bağlı |
| `retryDelay` | int (saniye) | İsteğe Bağlı |
| `secret` | object | İsteğe Bağlı |
| `startDelay` | int (saniye) | İsteğe Bağlı |
| `timeout` | int (saniye) | İsteğe Bağlı |
| `when` | [dize, dize,...] | İsteğe Bağlı |
| `workingDirectory` | dize | İsteğe Bağlı |

Bu özelliklerin ayrıntılarını bu makalenin [görev adımı özellikleri](#task-step-properties) bölümünde bulabilirsiniz.

### <a name="examples-cmd"></a>Örnekler: cmd

#### <a name="run-hello-world-image"></a>Merhaba-Dünya görüntüsünü Çalıştır

Bu komut, Docker Hub 'daki `hello-world.yaml` [Merhaba-Dünya](https://hub.docker.com/_/hello-world/) görüntüsüne başvuran görev dosyasını yürütür.

```azurecli
az acr run -f hello-world.yaml https://github.com/Azure-Samples/acr-tasks.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/hello-world.yaml -->
[!code-yml[task](~/acr-tasks/hello-world.yaml)]

#### <a name="run-bash-image-and-echo-hello-world"></a>Bash görüntüsünü ve "Hello World" yankısını çalıştırın

Bu komut, Docker Hub 'da `bash-echo.yaml` [Bash](https://hub.docker.com/_/bash/) görüntüsüne başvuran görev dosyasını yürütür.

```azurecli
az acr run -f bash-echo.yaml https://github.com/Azure-Samples/acr-tasks.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/bash-echo.yaml -->
[!code-yml[task](~/acr-tasks/bash-echo.yaml)]

#### <a name="run-specific-bash-image-tag"></a>Belirli Bash Image etiketini Çalıştır

Belirli bir görüntü sürümünü çalıştırmak için içinde `cmd`etiketini belirtin.

Bu komut, Docker Hub 'da `bash-echo-3.yaml` [bash: 3.0](https://hub.docker.com/_/bash/) görüntüsüne başvuran görev dosyasını yürütür.

```azurecli
az acr run -f bash-echo-3.yaml https://github.com/Azure-Samples/acr-tasks.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/bash-echo-3.yaml -->
[!code-yml[task](~/acr-tasks/bash-echo-3.yaml)]

#### <a name="run-custom-images"></a>Özel görüntüleri Çalıştır

Adım `cmd` türü, standart `docker run` biçimi kullanarak görüntülere başvurur. Bir kayıt defteriyle önceden kullanıma hazır olmayan görüntülerin docker.io 'tan kaynaklandığından varsayılır. Önceki örnek eşit olarak temsil edilebilir:

```yml
version: v1.0.0
steps:
  - cmd: docker.io/bash:3.0 echo hello world
```

Standart `docker run` görüntü başvuru kuralını kullanarak, `cmd` herhangi bir özel kayıt defterinden veya ortak Docker Hub 'ından görüntü çalıştırabilir. ACR görevinin yürütüldüğü aynı kayıt defterindeki görüntülere başvuruyorsanız, kayıt defteri kimlik bilgilerini belirtmeniz gerekmez.

* Azure Container Registry 'den bir görüntü çalıştırma

    Kayıt `[myregistry]` defterinizin adıyla değiştirin:

    ```yml
    version: v1.0.0
    steps:
        - cmd: [myregistry].azurecr.io/bash:3.0 echo hello world
    ```

* Bir çalıştırma değişkeniyle kayıt defteri başvurusunu genelleştirin

    Kayıt defteri adınızı bir `acr-task.yaml` dosyada sabit kodlamak yerine bir [çalıştırma değişkeni](#run-variables)kullanarak daha taşınabilir hale getirebilirsiniz. `Run.Registry` Değişkeni, çalışma zamanında görevin yürütüldüğü kayıt defterinin adına genişletilir.

    Önceki görevi herhangi bir Azure Container Registry 'de çalışacak şekilde genelleştirmek için görüntü adında [Run. Registry](#runregistry) değişkenine başvurun:

    ```yml
    version: v1.0.0
    steps:
      - cmd: {{.Run.Registry}}/bash:3.0 echo hello world
    ```

## <a name="task-step-properties"></a>Görev adımı özellikleri

Her adım türü, türü için uygun olan birkaç özelliği destekler. Aşağıdaki tabloda, tüm kullanılabilir adım özellikleri tanımlanmaktadır. Tüm adım türleri tüm özellikleri desteklemez. Her adım türü için bu özelliklerden hangilerinin kullanılabildiğini görmek için, [cmd](#cmd), [Build](#build)ve [Push](#push) Step Type başvuru bölümlerine bakın.

| Özellik | Type | İsteğe Bağlı | Açıklama | Varsayılan değer |
| -------- | ---- | -------- | ----------- | ------- |
| `detach` | bool | Evet | Çalışma sırasında kapsayıcının ayrılmayacağı. | `false` |
| `disableWorkingDirectoryOverride` | bool | Evet | Geçersiz kılma işlevselliğinin `workingDirectory` devre dışı bırakılıp başlatılmayacağını belirtir. Kapsayıcının çalışma dizini üzerinde tamamen `workingDirectory` denetim sağlamak için bunu ile birlikte kullanın. | `false` |
| `entryPoint` | dize | Evet | `[ENTRYPOINT]` Bir adımın kapsayıcısını geçersiz kılar. | None |
| `env` | [dize, dize,...] | Evet | Adımın ortam değişkenlerini tanımlayan `key=value` biçimdeki dizeler dizisi. | None |
| `expose` | [dize, dize,...] | Evet | Kapsayıcıdan sunulan bağlantı noktası dizisi. |  Yok. |
| [`id`](#example-id) | dize | Evet | Görevi içindeki adımı benzersiz bir şekilde tanımlar. Görevdeki diğer adımlarda `id`, ile `when`bağımlılık denetimi gibi bir adım başvuru yapılabilir.<br /><br />, `id` Aynı zamanda çalışan kapsayıcının adıdır. Görevdeki diğer kapsayıcılar üzerinde çalışan süreçler, `id` DNS ana bilgisayar adı olarak veya Docker logs [ID] ile erişim için öğesine başvurabilir. | `acb_step_%d``%d` , YAML dosyasında adımın en üst düzey 0 tabanlı dizinidir |
| `ignoreErrors` | bool | Evet | Kapsayıcının yürütülmesi sırasında bir hata oluşup oluşmadığını ne olursa olsun adımın başarıyla işaretleneceğini belirtir. | `false` |
| `isolation` | dize | Evet | Kapsayıcının yalıtım düzeyi. | `default` |
| `keep` | bool | Evet | Adım kapsayıcısının yürütmeden sonra tutulup tutulmayacağını belirtir. | `false` |
| `network` | object | Evet | Kapsayıcının çalıştığı bir ağı tanımlar. | Yok. |
| `ports` | [dize, dize,...] | Evet | Kapsayıcıdan konağa yayınlanan bağlantı noktaları dizisi. |  None |
| `pull` | bool | Evet | Herhangi bir önbelleğe alma davranışına engel olmak için yürütmeden önce kapsayıcının yapılıp yapılmayacağını belirtir. | `false` |
| `privileged` | bool | Evet | Kapsayıcının ayrıcalıklı modda çalıştırılıp çalıştırılmayacağı. | `false` |
| `repeat` | int | Evet | Kapsayıcının yürütülmesini yinelemek için yeniden deneme sayısı. | 0 |
| `retries` | int | Evet | Bir kapsayıcının yürütülmesinin başarısız olması durumunda denenecek yeniden deneme sayısı. Yeniden deneme yalnızca bir kapsayıcının çıkış kodu sıfır değilse denenir. | 0 |
| `retryDelay` | int (saniye) | Evet | Bir kapsayıcının çalışmasının yeniden denemeleri arasındaki gecikme süresi (saniye cinsinden). | 0 |
| `secret` | object | Evet | Azure kaynakları için Azure Key Vault gizli dizi veya yönetilen kimlik tanımlar. | Yok. |
| `startDelay` | int (saniye) | Evet | Kapsayıcının yürütülmesinin geciktirileceği saniye sayısı. | 0 |
| `timeout` | int (saniye) | Evet | Bir adımın sonlandırılmadan önce yürütebilmesi için gereken en fazla saniye sayısı. | 600 |
| [`when`](#example-when) | [dize, dize,...] | Evet | Görevin içindeki bir veya daha fazla adım için bir adımın bağımlılığını yapılandırır. | Yok. |
| `user` | dize | Evet | Bir kapsayıcının Kullanıcı adı veya UID 'SI | Yok. |
| `workingDirectory` | dize | Evet | Bir adım için çalışma dizinini ayarlar. Varsayılan olarak ACR görevleri, çalışma dizini olarak bir kök dizin oluşturur. Ancak, derlemeniz birkaç adım içeriyorsa, önceki adımlar aynı çalışma dizinini belirterek yapıtları sonraki adımlarla paylaşabilir. | `$HOME` |

### <a name="examples-task-step-properties"></a>Örnekler: Görev adımı özellikleri

#### <a name="example-id"></a>Örnek: kimlik

İki görüntü oluşturun ve işlevsel test görüntüsünü örnekleyebilirsiniz. Her adım, `when` özelliğindeki görev başvurusunda bulunan `id` diğer adımların benzersiz olarak tanımlanır.

```azurecli
az acr run -f when-parallel-dependent.yaml https://github.com/Azure-Samples/acr-tasks.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/when-parallel-dependent.yaml -->
[!code-yml[task](~/acr-tasks/when-parallel-dependent.yaml)]

#### <a name="example-when"></a>Örnek: ne zaman

Özelliği `when` , bir adımın görev içindeki diğer adımlara bağımlılığı belirtir. İki parametre değerini destekler:

* `when: ["-"]`-Diğer adımlara bağımlılığı olmadığını gösterir. Öğesini belirten `when: ["-"]` bir adım, yürütme işlemini hemen başlatır ve eşzamanlı adım yürütülmesine olanak sağlayacaktır.
* `when: ["id1", "id2"]`-Adımın "ID1" ve `id` `id` "ID2" ile adımlara bağlı olduğunu gösterir. "ID1" ve "ID2" adımları tamamlanana kadar bu adım yürütülmez.

Bir adımda belirtilmemişse, bu adım `acr-task.yaml` dosyadaki önceki adımın tamamlanmasına bağlıdır. `when`

Ardışık adım yürütme `when`:

```azurecli
az acr run -f when-sequential-default.yaml https://github.com/Azure-Samples/acr-tasks.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/when-sequential-default.yaml -->
[!code-yml[task](~/acr-tasks/when-sequential-default.yaml)]

İle `when`sıralı adım yürütme:

```azurecli
az acr run -f when-sequential-id.yaml https://github.com/Azure-Samples/acr-tasks.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/when-sequential-id.yaml -->
[!code-yml[task](~/acr-tasks/when-sequential-id.yaml)]

Paralel görüntü oluşturma:

```azurecli
az acr run -f when-parallel.yaml https://github.com/Azure-Samples/acr-tasks.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/when-parallel.yaml -->
[!code-yml[task](~/acr-tasks/when-parallel.yaml)]

Paralel görüntü oluşturma ve bağımlı test:

```azurecli
az acr run -f when-parallel-dependent.yaml https://github.com/Azure-Samples/acr-tasks.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/when-parallel-dependent.yaml -->
[!code-yml[task](~/acr-tasks/when-parallel-dependent.yaml)]

## <a name="run-variables"></a>Değişkenleri Çalıştır

ACR görevleri, yürütdiklerinde görev adımları için kullanılabilen varsayılan bir değişken kümesi içerir. Bu değişkenlere, aşağıdakilerden biri `{{.Run.VariableName}}` `VariableName` olan biçimi kullanılarak erişilebilir:

* `Run.ID`
* `Run.Registry`
* `Run.Date`
* `Run.Commit`
* `Run.Branch`

### <a name="runid"></a>Run.ID

`az acr run` Kullanılarak`az acr task create` oluşturulan görevlerin her çalıştırma, çalışma veya tetikleme tabanlı yürütmesi benzersiz bir kimliğe sahiptir. KIMLIK Şu anda yürütülmekte olan çalışmayı temsil eder.

Genellikle bir görüntüyü benzersiz bir şekilde etiketlemek için kullanılır:

```yml
version: v1.0.0
steps:
    - build: -t {{.Run.Registry}}/hello-world:{{.Run.ID}} .
```

### <a name="runregistry"></a>Çalıştır. kayıt defteri

Kayıt defterinin tam sunucu adı. Genellikle görevin çalıştırıldığı kayıt defterine genel olarak başvurmak için kullanılır.

```yml
version: v1.0.0
steps:
  - build: -t {{.Run.Registry}}/hello-world:{{.Run.ID}} .
```

### <a name="rundate"></a>Run. Date

Çalıştırmanın başladığı geçerli UTC saati.

### <a name="runcommit"></a>Run. COMMIT

GitHub deposuna yapılan bir kayıt tarafından tetiklenen bir görev için, COMMIT tanımlayıcısı.

### <a name="runbranch"></a>Run. Branch

GitHub deposuna yapılan bir kayıt tarafından tetiklenen bir görev için, dal adı.

## <a name="next-steps"></a>Sonraki adımlar

Çok adımlı görevlere genel bakış için [ACR görevlerinde multi-step Build, test ve Patch görevlerini çalıştırma](container-registry-tasks-multi-step.md)sayfasına bakın.

Tek adımlı derlemeler için bkz. [ACR görevlerine genel bakış](container-registry-tasks-overview.md).

<!-- IMAGES -->

<!-- LINKS - External -->
[acr-tasks]: https://github.com/Azure-Samples/acr-tasks

<!-- LINKS - Internal -->
[az-acr-run]: /cli/azure/acr#az-acr-run
[az-acr-task-create]: /cli/azure/acr/task#az-acr-task-create
[az-configure]: /cli/azure/reference-index#az-configure
