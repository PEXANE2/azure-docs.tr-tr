---
title: YAML referans - ACR Görevleri
description: Görev özellikleri, adım türleri, adım özellikleri ve yerleşik değişkenler de dahil olmak üzere ACR Görevleri için YAML'deki görevleri tanımlamak için başvuru.
ms.topic: article
ms.date: 10/23/2019
ms.openlocfilehash: 9558f698b4a9dbca46431fc02ced6ae30de29121
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79246987"
---
# <a name="acr-tasks-reference-yaml"></a>ACR Görevleri referans: YAML

ACR Görevleri'ndeki çok aşamalı görev tanımı, kapsayıcı oluşturma, test etme ve yama oluşturmaya odaklanmış konteyner merkezli bir işlem ilkelliği sağlar. Bu makalede, çok adımlı görevlerinizi tanımlayan YAML dosyalarının komutları, parametreleri, özellikleri ve sözdizimi yer alıyor.

Bu makalede, ACR Görevleri için çok adımlı görev YAML dosyaları oluşturmak için başvuru içerir. ACR Görevleri'ne giriş yapmak istiyorsanız, [ACR Görevleri'ne genel bakış'a](container-registry-tasks-overview.md)bakın.

## <a name="acr-taskyaml-file-format"></a>acr-task.yaml dosya biçimi

ACR Görevleri standart YAML sözdiziminde çok aşamalı görev bildirimini destekler. Bir GÖREVIN adımlarını YAML dosyasında tanımlarsınız. Daha sonra dosyayı [az acr çalıştır][az-acr-run] komutuna geçirerek görevi el ile çalıştırabilirsiniz. Veya, [az acr görev oluşturma][az-acr-task-create] ile bir görev oluşturmak için dosyayı kullanın otomatik olarak bir Git commit veya temel görüntü güncelleştirmesi tetiklenir. Bu makalede adımları `acr-task.yaml` içeren dosya olarak ifade edilen rağmen, ACR Görevleri desteklenen bir [uzantısı](#supported-task-filename-extensions)ile geçerli bir dosya adını destekler.

Üst `acr-task.yaml` düzey ilkel **görev özellikleri,** adım **türleri**ve adım **özellikleri**şunlardır:

* [Görev özellikleri](#task-properties) görev yürütme boyunca tüm adımlar için geçerlidir. Bunlar arasında çeşitli genel görev özellikleri vardır:
  * `version`
  * `stepTimeout`
  * `workingDirectory`
* [Görev adımı türleri,](#task-step-types) görevde yapılabilen eylem türlerini temsil e Üç adım türü vardır:
  * `build`
  * `push`
  * `cmd`
* [Görev adımı özellikleri,](#task-step-properties) tek bir adımiçin geçerli parametrelerdir. Bunlar arasında çeşitli adım özellikleri vardır:
  * `startDelay`
  * `timeout`
  * `when`
  * ... ve daha birçok.

Bazı ortak adım `acr-task.yaml` özellikleri de dahil olmak üzere bir dosyanın temel biçimi aşağıdaki gibidir. Kullanılabilir tüm adım özelliklerinin veya adım türü kullanımının kapsamlı bir gösterimi olmasa da, temel dosya biçimine hızlı bir genel bakış sağlar.

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

ACR Görevleri, görev dosyası olarak işleyecek leri dahil olmak üzere `.yaml`birkaç dosya adı uzantısı ayırdı. Aşağıdaki listede *olmayan* uzantılar ACR Tasks tarafından Dockerfile olarak kabul edilir: .yaml, .yml, .toml, .json, .sh, .bash, .zsh, .ps1, .ps, .cmd, .bat, .ts, .js, .php, .py, .rb, .lua

YAML, şu anda ACR Görevleri tarafından desteklenen tek dosya biçimidir. Diğer dosya adı uzantıları gelecekteki olası destek için ayrılmıştır.

## <a name="run-the-sample-tasks"></a>Örnek görevleri çalıştırma

Bu makalenin aşağıdaki bölümlerinde başvurulan birkaç örnek görev dosyası vardır. Örnek görevler herkese açık bir GitHub deposunda, [Azure Örnekleri/acr-görevlerdedir.][acr-tasks] Bunları Azure CLI [komutu az kısaltma çalıştırarak][az-acr-run]çalıştırabilirsiniz. Örnek komutlar aşağıdakilere benzer:

```azurecli
az acr run -f build-push-hello-world.yaml https://github.com/Azure-Samples/acr-tasks.git
```

Örnek komutların biçimlendirmesi, Azure CLI'de varsayılan bir kayıt defteri yapılandırdığınızı `--registry` varsayar, bu nedenle parametreyi atlar. Varsayılan kayıt defterini yapılandırmak [için, bir][az-configure] `acr=REGISTRY_NAME` değer `--defaults` kabul eden parametre ile az yapılandırma komutunu kullanın.

Örneğin, Azure CLI'yi "kayıt defteri" adlı varsayılan bir kayıt defteriyle yapılandırmak için:

```azurecli
az configure --defaults acr=myregistry
```

## <a name="task-properties"></a>Görev özellikleri

Görev özellikleri genellikle bir `acr-task.yaml` dosyanın üst kısmında görünür ve görev adımlarının tam yürütülmesi boyunca geçerli olan genel özelliklerdir. Bu genel özelliklerden bazıları tek bir adım içinde geçersiz kılınabilir.

| Özellik | Tür | İsteğe bağlı | Açıklama | Denetle | Varsayılan değer |
| -------- | ---- | -------- | ----------- | ------------------ | ------------- |
| `version` | string | Evet | `acr-task.yaml` ACR Görevler hizmeti tarafından ayrıştırılan dosyanın sürümü. ACR Görevleri geriye dönük uyumluluğu korumaya çalışırken, bu değer ACR Görevlerinin tanımlı bir sürümiçinde uyumluluğu korumasını sağlar. Belirtilmemişse, varsayılan olarak en son sürüme geçemez. | Hayır | None |
| `stepTimeout` | int (saniye) | Evet | Bir adımın çalıştırabileceği maksimum saniye sayısı. Özellik bir görevde belirtilirse, tüm `timeout` adımların varsayılan özelliğini ayarlar. Özellik `timeout` bir adımda belirtilirse, görev tarafından sağlanan özelliği geçersiz kılar. | Evet | 600 (10 dakika) |
| `workingDirectory` | string | Evet | Çalışma süresi sırasında kapsayıcının çalışma dizini. Özellik bir görevde belirtilirse, tüm `workingDirectory` adımların varsayılan özelliğini ayarlar. Bir adımda belirtilirse, görev tarafından sağlanan özelliği geçersiz kılar. | Evet | `/workspace` |
| `env` | [dize, dize, ...] | Evet |  Görev için ortam `key=value` değişkenlerini tanımlayan biçimdeki dizeler dizisi. Özellik bir görevde belirtilirse, tüm `env` adımların varsayılan özelliğini ayarlar. Bir adımda belirtilirse, görevden devralınan tüm ortam değişkenlerini geçersiz kılar. | None |
| `secrets` | [gizli, gizli, ...] | Evet | [Gizli](#secret) nesneler dizisi. | None |
| `networks` | [ağ, ağ, ...] | Evet | [Ağ](#network) nesneleri dizisi. | None |

### <a name="secret"></a>gizli dizi

Gizli nesne aşağıdaki özelliklere sahiptir.

| Özellik | Tür | İsteğe bağlı | Açıklama | Varsayılan değer |
| -------- | ---- | -------- | ----------- | ------- |
| `id` | string | Hayır | Sırrın tanımlayıcısı. | None |
| `keyvault` | string | Evet | Azure Key Vault Gizli URL'si. | None |
| `clientID` | string | Evet | Azure kaynakları için [kullanıcı tarafından atanan yönetilen kimliğin](container-registry-tasks-authentication-managed-identity.md) istemci kimliği. | None |

### <a name="network"></a>network

Ağ nesnesi aşağıdaki özelliklere sahiptir.

| Özellik | Tür | İsteğe bağlı | Açıklama | Varsayılan değer |
| -------- | ---- | -------- | ----------- | ------- | 
| `name` | string | Hayır | Ağın adı. | None |
| `driver` | string | Evet | Ağı yönetecek sürücü. | None |
| `ipv6` | bool | Evet | IPv6 ağ etkin olup olmadığı. | `false` |
| `skipCreation` | bool | Evet | Ağ oluşturma atlamak ister. | `false` |
| `isDefault` | bool | Evet | Ağın Azure Kapsayıcı Kayıt Defteri ile sağlanan varsayılan ağ olup olmadığı | `false` |

## <a name="task-step-types"></a>Görev adımı türleri

ACR Görevleri üç adım türünü destekler. Her adım türü, her adım türü için bölümde ayrıntılı olarak birkaç özelliği destekler.

| Adım türü | Açıklama |
| --------- | ----------- |
| [`build`](#build) | Tanıdık `docker build` sözdizimini kullanarak bir kapsayıcı görüntüsü oluşturur. |
| [`push`](#push) | Yeni oluşturulmuş `docker push` veya yeniden etiketlenmiş görüntülerden birini kapsayıcı kayıt defterine yürütür. Azure Kapsayıcı Kayıt Defteri, diğer özel kayıt defterleri ve ortak Docker Hub desteklenir. |
| [`cmd`](#cmd) | Parametrelerimle birlikte kapsayıcının. `[ENTRYPOINT]` Adım `cmd` türü, birim `env` `detach`ve eşzamanlı `docker run` kapsayıcı yürütme ile işlevsel test sağlayan, , ve diğer tanıdık komut seçenekleri gibi parametreleri destekler. |

## <a name="build"></a>derleme

Bir kapsayıcı görüntüsü oluşturun. Adım `build` türü, bulutta birinci sınıf ilkel `docker build` olarak çalışan çok kiracılı, güvenli bir aracı temsil eder.

### <a name="syntax-build"></a>Sözdizimi: yapı

```yml
version: v1.1.0
steps:
  - [build]: -t [imageName]:[tag] -f [Dockerfile] [context]
    [property]: [value]
```

Adım `build` türü aşağıdaki tablodaki parametreleri destekler. Adım `build` türü, [yapı](https://docs.docker.com/engine/reference/commandline/build/) zamanı değişkenlerini ayarlamak gibi `--build-arg` docker build komutunun tüm yapı seçeneklerini de destekler.

| Parametre | Açıklama | İsteğe bağlı |
| --------- | ----------- | :-------: |
| `-t`&#124;`--image` | Yerleşik görüntünün `image:tag` tam nitelikliliğini tanımlar.<br /><br />Görüntüler işlevsel testler gibi iç görev doğrulamaları için kullanılabilen, `push` tüm görüntülerin bir kayıt defterine gerek yoktur. Ancak, görev yürütme sidi içinde bir görüntü örneği için, görüntü başvuruiçin bir ad gerekir.<br /><br />Aksine, `az acr build`ACR Görevleri çalışan varsayılan itme davranışı sağlamaz. ACR Görevleri ile varsayılan senaryo, bir görüntüyü oluşturma, doğrulama ve itme yeteneğini varsayar. Yerleşik görüntüleri isteğe bağlı olarak nasıl iteceklerine [bakın.](#push) | Evet |
| `-f`&#124;`--file` | Dockerfile'ın '' `docker build`a geçtiğini belirtir. Belirtilmemişse, bağlamın kökündeki varsayılan Dockerfile varsayalım. Dockerfile belirtmek için dosya adını bağlamın köküne göre geçirin. | Evet |
| `context` | Kök dizini `docker build`geçti. Her görevin kök dizini paylaşılan çalışma [Dizin'ine](#task-step-properties)ayarlanır ve ilişkili Git klonlanmış dizinin kökünü içerir. | Hayır |

### <a name="properties-build"></a>Özellikleri: yapı

Adım `build` türü aşağıdaki özellikleri destekler. Bu makalenin Görev [adımı özellikleri](#task-step-properties) bölümünde bu özelliklerin ayrıntılarını bulun.

| | | |
| -------- | ---- | -------- |
| `detach` | bool | İsteğe bağlı |
| `disableWorkingDirectoryOverride` | bool | İsteğe bağlı |
| `entryPoint` | string | İsteğe bağlı |
| `env` | [dize, dize, ...] | İsteğe bağlı |
| `expose` | [dize, dize, ...] | İsteğe bağlı |
| `id` | string | İsteğe bağlı |
| `ignoreErrors` | bool | İsteğe bağlı |
| `isolation` | string | İsteğe bağlı |
| `keep` | bool | İsteğe bağlı |
| `network` | object | İsteğe bağlı |
| `ports` | [dize, dize, ...] | İsteğe bağlı |
| `pull` | bool | İsteğe bağlı |
| `repeat` | int | İsteğe bağlı |
| `retries` | int | İsteğe bağlı |
| `retryDelay` | int (saniye) | İsteğe bağlı |
| `secret` | object | İsteğe bağlı |
| `startDelay` | int (saniye) | İsteğe bağlı |
| `timeout` | int (saniye) | İsteğe bağlı |
| `when` | [dize, dize, ...] | İsteğe bağlı |
| `workingDirectory` | string | İsteğe bağlı |

### <a name="examples-build"></a>Örnekler: build

#### <a name="build-image---context-in-root"></a>Yapı görüntüsü - kökte bağlam

```azurecli
az acr run -f build-hello-world.yaml https://github.com/AzureCR/acr-tasks-sample.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/build-hello-world.yaml -->
[!code-yml[task](~/acr-tasks/build-hello-world.yaml)]

#### <a name="build-image---context-in-subdirectory"></a>Oluşturma görüntü - alt dizin içinde bağlam

```yml
version: v1.1.0
steps:
  - build: -t $Registry/hello-world -f hello-world.dockerfile ./subDirectory
```

## <a name="push"></a>push

Bir veya daha fazla yerleşik veya yeniden etiketlenmiş görüntüyü bir kapsayıcı kayıt defterine itin. Azure Konteyner Kayıt Defteri gibi özel kayıt şirketlerine veya ortak Docker Hub'ına itme yi destekler.

### <a name="syntax-push"></a>Sözdizimi: itme

Adım `push` türü bir görüntü koleksiyonunu destekler. YAML toplama sözdizimi satır içinde ve iç içe biçimleri destekler. Tek bir görüntüyü zorlamak genellikle satır içinde sözdizimi kullanılarak temsil edilir:

```yml
version: v1.1.0
steps:
  # Inline YAML collection syntax
  - push: ["$Registry/hello-world:$ID"]
```

Daha fazla okunabilirlik için, birden çok görüntüyü iterken iç içe geçen sözdizimini kullanın:

```yml
version: v1.1.0
steps:
  # Nested YAML collection syntax
  - push:
    - $Registry/hello-world:$ID
    - $Registry/hello-world:latest
```

### <a name="properties-push"></a>Özellikleri: itme

Adım `push` türü aşağıdaki özellikleri destekler. Bu makalenin Görev [adımı özellikleri](#task-step-properties) bölümünde bu özelliklerin ayrıntılarını bulun.

| | | |
| -------- | ---- | -------- |
| `env` | [dize, dize, ...] | İsteğe bağlı |
| `id` | string | İsteğe bağlı |
| `ignoreErrors` | bool | İsteğe bağlı |
| `startDelay` | int (saniye) | İsteğe bağlı |
| `timeout` | int (saniye) | İsteğe bağlı |
| `when` | [dize, dize, ...] | İsteğe bağlı |

### <a name="examples-push"></a>Örnekler: itme

#### <a name="push-multiple-images"></a>Birden çok görüntüyü itme

```azurecli
az acr run -f build-push-hello-world.yaml https://github.com/Azure-Samples/acr-tasks.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/build-push-hello-world.yaml -->
[!code-yml[task](~/acr-tasks/build-push-hello-world.yaml)]

#### <a name="build-push-and-run"></a>Oluşturma, itme ve çalıştırma

```azurecli
az acr run -f build-run-hello-world.yaml https://github.com/Azure-Samples/acr-tasks.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/build-run-hello-world.yaml -->
[!code-yml[task](~/acr-tasks/build-run-hello-world.yaml)]

## <a name="cmd"></a>cmd

Adım `cmd` türü bir kapsayıcı çalıştırın.

### <a name="syntax-cmd"></a>Sözdizimi: cmd

```yml
version: v1.1.0
steps:
  - [cmd]: [containerImage]:[tag (optional)] [cmdParameters to the image]
```

### <a name="properties-cmd"></a>Özellikleri: cmd

Adım `cmd` türü aşağıdaki özellikleri destekler:

| | | |
| -------- | ---- | -------- |
| `detach` | bool | İsteğe bağlı |
| `disableWorkingDirectoryOverride` | bool | İsteğe bağlı |
| `entryPoint` | string | İsteğe bağlı |
| `env` | [dize, dize, ...] | İsteğe bağlı |
| `expose` | [dize, dize, ...] | İsteğe bağlı |
| `id` | string | İsteğe bağlı |
| `ignoreErrors` | bool | İsteğe bağlı |
| `isolation` | string | İsteğe bağlı |
| `keep` | bool | İsteğe bağlı |
| `network` | object | İsteğe bağlı |
| `ports` | [dize, dize, ...] | İsteğe bağlı |
| `pull` | bool | İsteğe bağlı |
| `repeat` | int | İsteğe bağlı |
| `retries` | int | İsteğe bağlı |
| `retryDelay` | int (saniye) | İsteğe bağlı |
| `secret` | object | İsteğe bağlı |
| `startDelay` | int (saniye) | İsteğe bağlı |
| `timeout` | int (saniye) | İsteğe bağlı |
| `when` | [dize, dize, ...] | İsteğe bağlı |
| `workingDirectory` | string | İsteğe bağlı |

Bu özelliklerin ayrıntılarını bu makalenin [Görev adımı özellikleri](#task-step-properties) bölümünde bulabilirsiniz.

### <a name="examples-cmd"></a>Örnekler: cmd

#### <a name="run-hello-world-image"></a>Merhaba dünya imajını çalıştırın

Bu komut, `hello-world.yaml` Docker Hub'daki [merhaba dünyası](https://hub.docker.com/_/hello-world/) görüntüsüne başvuran görev dosyasını yürütür.

```azurecli
az acr run -f hello-world.yaml https://github.com/Azure-Samples/acr-tasks.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/hello-world.yaml -->
[!code-yml[task](~/acr-tasks/hello-world.yaml)]

#### <a name="run-bash-image-and-echo-hello-world"></a>Run bash görüntü ve yankı "merhaba dünya"

Bu komut, `bash-echo.yaml` Docker Hub'daki [bash](https://hub.docker.com/_/bash/) görüntüsüne başvuran görev dosyasını yürütür.

```azurecli
az acr run -f bash-echo.yaml https://github.com/Azure-Samples/acr-tasks.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/bash-echo.yaml -->
[!code-yml[task](~/acr-tasks/bash-echo.yaml)]

#### <a name="run-specific-bash-image-tag"></a>Belirli bash resim etiketini çalıştırın

Belirli bir resim sürümünü çalıştırmak için, `cmd`etiketi .

Bu komut, `bash-echo-3.yaml` Docker Hub'daki [bash:3.0](https://hub.docker.com/_/bash/) görüntüsüne başvuran görev dosyasını yürütür.

```azurecli
az acr run -f bash-echo-3.yaml https://github.com/Azure-Samples/acr-tasks.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/bash-echo-3.yaml -->
[!code-yml[task](~/acr-tasks/bash-echo-3.yaml)]

#### <a name="run-custom-images"></a>Özel görüntüler çalıştırma

Adım `cmd` türü, standart `docker run` biçimi kullanarak görüntülere başvurur. Kayıt defteriyle önceden anlaşılmayan görüntülerin docker.io kaynaklandığı varsayılır. Önceki örnek aşağıdaki gibi eşit olarak temsil edilebilir:

```yml
version: v1.1.0
steps:
  - cmd: docker.io/bash:3.0 echo hello world
```

Standart `docker run` görüntü başvuru kuralını `cmd` kullanarak, herhangi bir özel kayıt defterinden veya ortak Docker Hub'ından görüntüler çalıştırabilirsiniz. ACR Görevi'nin yürütüldüğü aynı kayıt defterindeki resimlere başvuruyorsanız, kayıt defteri kimlik bilgilerini belirtmeniz gerekmez.

* Azure kapsayıcı kayıt defterine ait bir görüntü çalıştırın. Aşağıdaki örnek, adlı `myregistry`bir kayıt defteriniz ve `myimage:mytag`özel bir resminiz olduğunu varsayar.

    ```yml
    version: v1.1.0
    steps:
        - cmd: myregistry.azurecr.io/myimage:mytag
    ```

* Kayıt defteri başvurularını Bir Çalıştır değişkeni veya diğer adla genelleştirin

    Kayıt defteri adınızı bir `acr-task.yaml` dosyada sabit kodlamak yerine, bir Çalıştır [değişkeni](#run-variables) veya diğer [ad](#aliases)kullanarak daha taşınabilir hale getirebilirsiniz. Değişken `Run.Registry` veya `$Registry` diğer ad çalışma zamanında görevin yürütüldüğü kayıt defterinin adına genişletilir.

    Örneğin, önceki görevi herhangi bir Azure kapsayıcı kayıt defterinde çalışabilmesi için genelleştirmek için görüntü adındaki $Registry değişkene başvurun:

    ```yml
    version: v1.1.0
    steps:
      - cmd: $Registry/myimage:mytag
    ```

## <a name="task-step-properties"></a>Görev adımı özellikleri

Her adım türü türü türüne uygun birkaç özelliği destekler. Aşağıdaki tablo, kullanılabilir adım özelliklerinin tümlerini tanımlar. Tüm adım türleri tüm özellikleri desteklemez. Bu özelliklerden hangisinin her adım türü için kullanılabilir olduğunu görmek için [cmd,](#cmd) [build](#build)ve [push](#push) step türü başvuru bölümlerine bakın.

| Özellik | Tür | İsteğe bağlı | Açıklama | Varsayılan değer |
| -------- | ---- | -------- | ----------- | ------- |
| `detach` | bool | Evet | Kapsayıcı çalışırken ayrılıp ayrılmaması gerektiği. | `false` |
| `disableWorkingDirectoryOverride` | bool | Evet | İşlevselliği `workingDirectory` devre dışı bırakmayıp devre dışı bırakmayı. Kapsayıcının çalışma `workingDirectory` dizini üzerinde tam denetime sahip olmak için bunu birlikte kullanın. | `false` |
| `entryPoint` | string | Evet | Adım ın `[ENTRYPOINT]` kapsayıcısını geçersiz kılar. | None |
| `env` | [dize, dize, ...] | Evet | Adım için ortam `key=value` değişkenlerini tanımlayan biçimdeki dizeler dizisi. | None |
| `expose` | [dize, dize, ...] | Evet | Kapsayıcıdan açığa çıkan bağlantı noktaları dizisi. |  None |
| [`id`](#example-id) | string | Evet | Görev içindeki adımı benzersiz bir şekilde tanımlar. Görevdeki diğer `id`adımlar, bağımlılık denetimi gibi bir adımın `when`kine başvurulabilir.<br /><br />Ayrıca `id` çalışan konteynerin adıdır. Görevdeki diğer kapsayıcılarda çalışan işlemler, `id` dns ana bilgisayar adı olarak adlandırılabilir veya örneğin docker günlükleri [id] ile erişmek için. | `acb_step_%d`, `%d` YAML dosyasında yukarıdan aşağıya adımın 0 tabanlı dizin nerede |
| `ignoreErrors` | bool | Evet | Kapsayıcı yürütme sırasında bir hata oluşup oluşmadığına bakılmaksızın adımın başarılı olarak işaretlenip işaretlenmeyeceği. | `false` |
| `isolation` | string | Evet | Kapsayıcının izolasyon seviyesi. | `default` |
| `keep` | bool | Evet | Adımın kapsayıcısının yürütmeden sonra tutulup tutulmaması gerektiği. | `false` |
| `network` | object | Evet | Kapsayıcının çalıştığı ağı tanımlar. | None |
| `ports` | [dize, dize, ...] | Evet | Kapsayıcıdan ana bilgisayara yayımlanan bağlantı noktaları dizisi. |  None |
| `pull` | bool | Evet | Herhangi bir önbelleğe alma davranışı önlemek için uygulamadan önce kapsayıcının çekme zorlamak için olsun. | `false` |
| `privileged` | bool | Evet | Kapsayıcının ayrıcalıklı modda çalıştırılıp çalıştırılmayacağı. | `false` |
| `repeat` | int | Evet | Bir kapsayıcının yürütülmesini yinelemek için yeniden deneme sayısı. | 0 |
| `retries` | int | Evet | Bir kapsayıcı yürütme başarısız olursa denemeye yeniden deneme sayısı. Yeniden deneme yalnızca bir kapsayıcının çıkış kodu sıfır değilse denenir. | 0 |
| `retryDelay` | int (saniye) | Evet | Bir kapsayıcının yürütülmesinin yeniden denemesi arasındaki saniyeler içinde gecikme. | 0 |
| `secret` | object | Evet | Azure kaynakları için azure anahtar kasası gizli veya [yönetilen bir kimlik](container-registry-tasks-authentication-managed-identity.md)tanımlar. | None |
| `startDelay` | int (saniye) | Evet | Bir konteynerin yürütülmesini geciktirmek için saniye sayısı. | 0 |
| `timeout` | int (saniye) | Evet | Bir adımın sonlandırılmadan önce yürütebileceği maksimum saniye sayısı. | 600 |
| [`when`](#example-when) | [dize, dize, ...] | Evet | Bir adımın görev içindeki bir veya daha fazla diğer adıma olan bağımlılığını yapılandırır. | None |
| `user` | string | Evet | Bir kapsayıcının kullanıcı adı veya UID'si | None |
| `workingDirectory` | string | Evet | Çalışma dizini bir adım için ayarlar. Varsayılan olarak, ACR Görevleri çalışma dizini olarak bir kök dizini oluşturur. Ancak, yapınızın birkaç adımı varsa, önceki adımlar aynı çalışma dizinini belirterek yapıları sonraki adımlarla paylaşabilir. | `/workspace` |

### <a name="examples-task-step-properties"></a>Örnekler: Görev adımı özellikleri

#### <a name="example-id"></a>Örnek: id

İşlevsel bir test görüntüsü oluşturmak için iki görüntü oluşturun. Her adım, kendi `id` `when` özelliğindeki görev başvurusundaki diğer adımlarla tanımlanır.

```azurecli
az acr run -f when-parallel-dependent.yaml https://github.com/Azure-Samples/acr-tasks.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/when-parallel-dependent.yaml -->
[!code-yml[task](~/acr-tasks/when-parallel-dependent.yaml)]

#### <a name="example-when"></a>Örnek: ne zaman

Özellik, `when` bir adımın göreviçindeki diğer adımlara olan bağımlılığını belirtir. İki parametre değerini destekler:

* `when: ["-"]`- Diğer adımlara bağımlılık olmadığını gösterir. Belirten `when: ["-"]` bir adım hemen yürütme ye başlar ve eşzamanlı adım yürütme sağlar.
* `when: ["id1", "id2"]`- Adımın "id1" `id` ve `id` "id2" ile olan adımlara bağlı olduğunu gösterir. Bu adım hem "id1" hem de "id2" adımları tamamlanana kadar yürütülmez.

Bir `when` adımda belirtilmemişse, bu adım dosyadaki önceki adımın tamamlanmasına `acr-task.yaml` bağlıdır.

Sıralı adım `when`yürütme olmadan:

```azurecli
az acr run -f when-sequential-default.yaml https://github.com/Azure-Samples/acr-tasks.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/when-sequential-default.yaml -->
[!code-yml[task](~/acr-tasks/when-sequential-default.yaml)]

Sıralı adım `when`yürütme:

```azurecli
az acr run -f when-sequential-id.yaml https://github.com/Azure-Samples/acr-tasks.git
```

<!-- SOURCE: https://github.com/Azure-Samples/acr-tasks/blob/master/when-sequential-id.yaml -->
[!code-yml[task](~/acr-tasks/when-sequential-id.yaml)]

Paralel görüntüler oluşturmak:

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

## <a name="run-variables"></a>Değişkenleri çalıştırma

ACR Görevleri, görev adımları için kullanılabilir varsayılan bir değişken kümesi içerir. Bu değişkenlere aşağıdakilerden biri olan `{{.Run.VariableName}}` `VariableName` biçim kullanılarak erişilebilir:

* `Run.ID`
* `Run.SharedVolume`
* `Run.Registry`
* `Run.RegistryName`
* `Run.Date`
* `Run.OS`
* `Run.Architecture`
* `Run.Commit`
* `Run.Branch`
* `Run.TaskName`

Değişken adları genellikle kendini açıklayıcıdır. Sık kullanılan değişkenler için ayrıntılar aşağıdaki gibidir. YAML sürümünden `v1.1.0`itibaren, çoğu çalıştırılan değişkenyerine kısaltılmış, önceden tanımlanmış bir [görev takma adını](#aliases) kullanabilirsiniz. Örneğin, `$Registry` takma ad `{{.Run.Registry}}`yerine kullanın.

### <a name="runid"></a>Run.ID

Oluşturulan görevlerin `az acr run`her Çalıştır,veya tetik `az acr task create`tabanlı yürütülmesi, benzersiz bir kimliği vardır. Kimlik, şu anda yürütülmekte olan Çalıştır'ı temsil eder.

Genellikle benzersiz bir görüntü etiketleme için kullanılır:

```yml
version: v1.1.0
steps:
    - build: -t $Registry/hello-world:$ID .
```

### <a name="runregistry"></a>Run.Registry

Kayıt defterinin tam nitelikli sunucu adı. Genellikle, görevin çalıştırıldığı kayıt defterine genel olarak başvurmak için kullanılır.

```yml
version: v1.1.0
steps:
  - build: -t $Registry/hello-world:$ID .
```

### <a name="runregistryname"></a>Run.RegistryName

Konteyner kayıt defterinin adı. Genellikle tam nitelikli bir sunucu adı gerektirmeyen görev adımlarında `cmd` kullanılır (örneğin, kayıt defterlerinde Azure CLI komutları çalıştıran adımlar).

```yml
version 1.1.0
steps:
# List repositories in registry
- cmd: az login --identity
- cmd: az acr repository list --name $RegistryName
```

### <a name="rundate"></a>Çalıştır.Tarih

Geçerli UTC zamanı çalıştırma başladı.

### <a name="runcommit"></a>Çalıştır.Commit

GitHub deposuna bağlanmak tarafından tetiklenen bir görev için, işleme tanımlayıcısı.

### <a name="runbranch"></a>Run.Branch

GitHub deposuna bağlanmanın tetiklediği bir görev için şube adı.

## <a name="aliases"></a>Diğer adlar

Şu `v1.1.0`andan itibaren, ACR Görevleri yürütüldüğünde görev adımlarının kullanabileceği diğer adları destekler. Diğer adlar, bash ve diğer bazı komut kabuklarında desteklenen diğer adlara (komut kısayolları) benzerdir. 

Takma adla, tek bir sözcük girerek herhangi bir komut veya komut grubunu (seçenekler ve dosya adları dahil) başlatabilirsiniz.

ACR Görevleri, önceden tanımlanmış birkaç diğer ad ve ayrıca oluşturduğunuz özel diğer adları destekler.

### <a name="predefined-aliases"></a>Önceden tanımlanmış takma adlar

Aşağıdaki görev diğer adları [çalıştırma değişkenleri](#run-variables)yerine kullanılabilir:

| Diğer ad | Değişkeni çalıştır |
| ----- | ------------ |
| `ID` | `Run.ID` |
| `SharedVolume` | `Run.SharedVolume` |
| `Registry` | `Run.Registry` |
| `RegistryName` | `Run.RegistryName` |
| `Date` | `Run.Date` |
| `OS` | `Run.OS` |
| `Architecture` | `Run.Architecture` |
| `Commit` | `Run.Commit` |
| `Branch` | `Run.Branch` |

Görev adımlarında, bu örnekte `$` olduğu gibi yönergeyle birlikte bir diğer addan önce gelir:

```yml
version: v1.1.0
steps:
  - build: -t $Registry/hello-world:$ID -f hello-world.dockerfile .
```

### <a name="image-aliases"></a>Resim takma adları

Aşağıdaki diğer adların her biri Microsoft Kapsayıcı Kayıt Defteri'nde (MCR) kararlı bir görüntüye işaret eder. Bir yönerge kullanmadan görev `cmd` dosyasının bölümünde bunların her birine başvurabilirsiniz.

| Diğer ad | Görüntü |
| ----- | ----- |
| `acr` | `mcr.microsoft.com/acr/acr-cli:0.1` |
| `az` | `mcr.microsoft.com/acr/azure-cli:a80af84` |
| `bash` | `mcr.microsoft.com/acr/bash:a80af84` |
| `curl` | `mcr.microsoft.com/acr/curl:a80af84` |

Aşağıdaki örnek görev, çalıştırma kayıt [purge](container-registry-auto-purge.md) defterindeki repo'da `samples/hello-world` 7 günden eski resim etiketlerini temizlemek için birkaç takma ad kullanır:

```yml
version: v1.1.0
steps:
  - cmd: acr tag list --registry $RegistryName --repository samples/hello-world
  - cmd: acr purge --registry $RegistryName --filter samples/hello-world:.* --ago 7d
```

### <a name="custom-alias"></a>Özel takma ad

YAML dosyanızda özel bir takma ad tanımlayın ve aşağıdaki örnekte gösterildiği gibi kullanın. Takma ad yalnızca alfasayısal karakterler içerebilir. Takma adı genişletmek için varsayılan `$` yönerge karakterdir.

```yml
version: v1.1.0
alias:
  values:
    repo: myrepo
steps:
  - build: -t $Registry/$repo/hello-world:$ID -f Dockerfile .
```

Özel diğer ad tanımları için uzak veya yerel bir YAML dosyasına bağlanabilirsiniz. Aşağıdaki örnek, Azure blob depolamasındaki bir YAML dosyasına bağlantılar:

```yml
version: v1.1.0
alias:
  src:  # link to local or remote custom alias files
    - 'https://link/to/blob/remoteAliases.yml?readSasToken'
[...]
```

## <a name="next-steps"></a>Sonraki adımlar

Çok aşamalı görevlere genel bakış için, [ACR Görevleri'ndeki Çok adımlı yapıyı çalıştır, sınave ve yama görevlerine](container-registry-tasks-multi-step.md)bakın.

Tek adımlı yapılar için [ACR Görevleri'ne genel bakış'a](container-registry-tasks-overview.md)bakın.



<!-- IMAGES -->

<!-- LINKS - External -->
[acr-tasks]: https://github.com/Azure-Samples/acr-tasks

<!-- LINKS - Internal -->
[az-acr-run]: /cli/azure/acr#az-acr-run
[az-acr-task-create]: /cli/azure/acr/task#az-acr-task-create
[az-configure]: /cli/azure/reference-index#az-configure
