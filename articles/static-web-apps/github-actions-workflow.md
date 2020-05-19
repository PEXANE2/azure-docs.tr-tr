---
title: Azure statik Web Apps için GitHub eylemleri iş akışları
description: Azure statik Web Apps sürekli dağıtımı ayarlamak için GitHub depolarını nasıl kullanacağınızı öğrenin.
services: static-web-apps
author: christiannwamba
ms.service: static-web-apps
ms.topic: conceptual
ms.date: 05/08/2020
ms.author: chnwamba
ms.openlocfilehash: c7180be2afa541f743d7bd81627b93f34d9bb4eb
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83597845"
---
# <a name="github-actions-workflows-for-azure-static-web-apps-preview"></a>Azure statik Web Apps önizlemesi için GitHub eylemleri iş akışları

Yeni bir Azure statik Web uygulaması kaynağı oluşturduğunuzda Azure, uygulamanın sürekli dağıtımını denetlemek için bir GitHub eylemleri iş akışı oluşturur. İş akışı bir YAML dosyası tarafından yönlendiriliyor. Bu makalede, iş akışı dosyasının yapısı ve seçenekleri ayrıntılı olarak açıklanır.

Dağıtımlar, tek tek [adımlarla](#steps)tanımlanan [işleri](#jobs) çalıştıran [Tetikleyiciler](#triggers)tarafından başlatılır.

## <a name="file-location"></a>Dosya konumu

GitHub deponuzu Azure statik Web Apps bağladığınızda depoya bir iş akışı dosyası eklenir.

Oluşturulan iş akışı dosyasını görüntülemek için bu adımları izleyin.

1. Uygulamanın deposunu GitHub üzerinde açın.
1. _Kod_ sekmesinde, `.github/workflows` klasöre tıklayın.
1. Dosyaya benzeyen bir ada tıklayın `azure-static-web-apps-<RANDOM_NAME>.yml` .

Deponuzdaki YAML dosyası aşağıdaki örneğe benzer olacaktır:

```yml
name: Azure Static Web Apps CI/CD

on:
  push:
    branches:
    - master
  pull_request:
    types: [opened, synchronize, reopened, closed]
    branches:
    - master

jobs:
  build_and_deploy_job:
    if: github.event_name == 'push' || (github.event_name == 'pull_request' && github.event.action != 'closed')
    runs-on: ubuntu-latest
    name: Build and Deploy Job
    steps:
    - uses: actions/checkout@v1
    - name: Build And Deploy
      id: builddeploy
      uses: Azure/static-web-apps-deploy@v0.0.1-preview
      with:
        azure_static_web_apps_api_token: ${{ secrets.AZURE_STATIC_WEB_APPS_API_TOKEN_MANGO_RIVER_0AFDB141E }}
        repo_token: ${{ secrets.GITHUB_TOKEN }} # Used for GitHub integrations (i.e. PR comments)
        action: 'upload'
        ###### Repository/Build Configurations - These values can be configured to match you app requirements. ######
        app_location: '/' # App source code path
        api_location: 'api' # Api source code path - optional
        app_artifact_location: 'dist' # Built app content directory - optional
        ###### End of Repository/Build Configurations ######

  close_pull_request_job:
    if: github.event_name == 'pull_request' && github.event.action == 'closed'
    runs-on: ubuntu-latest
    name: Close Pull Request Job
    steps:
    - name: Close Pull Request
      id: closepullrequest
      uses: Azure/static-web-apps-deploy@v0.0.1-preview
      with:
        azure_static_web_apps_api_token: ${{ secrets.AZURE_STATIC_WEB_APPS_API_TOKEN_MANGO_RIVER_0AFDB141E }}
        action: 'close'
```

## <a name="triggers"></a>Tetikleyiciler

GitHub eylemleri [tetikleyicisi](https://help.github.com/actions/reference/events-that-trigger-workflows) , bir GitHub eylemleri iş akışının olay tetikleyicilerini temel alarak bir işi çalıştırmasını bildirir. Tetikleyiciler, `on` iş akışı dosyasındaki özelliği kullanılarak listelenir.

```yml
on:
  push:
    branches:
    - master
  pull_request:
    types: [opened, synchronize, reopened, closed]
    branches:
    - master
```

Özelliği ile ilişkili ayarlar sayesinde `on` , hangi dalların bir işi tetikleyeceğini tanımlayabilir ve farklı çekme isteği durumları için Tetikleyicileri tetiklenecek şekilde ayarlayabilirsiniz.

Bu örnekte, _ana_ dal değiştikçe bir iş akışı başlatılır. İş akışını başlatan değişiklikler, yürütmelerin ve çekme isteklerinin seçili dala göre açılmasını içerir.

## <a name="jobs"></a>İşler

Her olay tetikleyicisi bir olay işleyicisi gerektirir. [İşler](https://help.github.com/actions/reference/workflow-syntax-for-github-actions#jobs) , bir olay tetiklendiğinde ne olacağını tanımlar.

Statik Web Apps iş akışı dosyasında, kullanılabilir iki iş vardır.

| Name  | Açıklama |
|---------|---------|
|`build_and_deploy_job` | Yürütmeler gönderdiğinizde veya özellikte listelenen dala karşı bir çekme isteği açtığınızda yürütülür `on` . |
|`close_pull_request_job` | YALNıZCA bir çekme isteğini kapattığınızda yürütülür. |

## <a name="steps"></a>Adımlar

Adımlar, bir iş için sıralı görevlerdir. Bir adım bağımlılıklar yükleme, testleri çalıştırma ve uygulamanızı üretime dağıtma gibi eylemleri gerçekleştirir.

Bir iş akışı dosyası aşağıdaki adımları tanımlar.

| İş  | Adımlar  |
|---------|---------|
| `build_and_deploy_job` |<ol><li>Işlemin ortamındaki depoyu kontrol eder.<li>Depoyu oluşturup Azure statik Web Apps dağıtır.</ol>|
| `close_pull_request_job` | <ol><li>Azure statik Web Apps bir çekme isteğinin kapatıldığını bildirir.</ol>|

## <a name="build-and-deploy"></a>Oluşturma ve dağıtma

Adlı adım, `Build and Deploy` Azure statik Web Apps örneğinize yapılar ve dağıtır. `with`Bölümünde, dağıtımınız için aşağıdaki değerleri özelleştirebilirsiniz.

```yml
with:
    azure_static_web_apps_api_token: ${{ secrets.AZURE_STATIC_WEB_APPS_API_TOKEN_MANGO_RIVER_0AFDB141E }}
    repo_token: ${{ secrets.GITHUB_TOKEN }} # Used for GitHub integrations (i.e. PR comments)
    action: 'upload'
    ###### Repository/Build Configurations - These values can be configured to match you app requirements. ######
    app_location: '/' # App source code path
    api_location: 'api' # Api source code path - optional
    app_artifact_location: 'dist' # Built app content directory - optional
    ###### End of Repository/Build Configurations ######
```

| Özellik | Açıklama | Gerekli |
|---|---|---|
| `app_location` | Uygulama kodunuzun konumu.<br><br>Örneğin, `/` uygulamanızın kaynak kodu deponun kökünde veya `/app` uygulama kodunuz adlı bir dizinde ise girin `app` . | Yes |
| `api_location` | Azure Işlevleri kodunuzun konumu.<br><br>Örneğin, `/api` uygulama kodunuz adlı bir klasörde ise yazın `api` . Klasörde hiçbir Azure Işlevleri uygulaması algılanmazsa, derleme başarısız olmaz, iş akışı bir API istemediğinizi varsayar. | Hayır |
| `app_artifact_location` | Derleme çıkış dizininin öğesine göre konumu `app_location` .<br><br>Örneğin, uygulama kaynak kodunuz adresinde bulunuyorsa `/app` ve derleme betiği dosyaları klasörüne çıktıdaysa `/app/build` `build` değer olarak ayarlanır `app_artifact_location` . | Hayır |

`repo_token`, `action` , Ve `azure_static_web_apps_api_token` değerleri Azure static tarafından sizin için ayarlanır Web Apps el ile değiştirilmemelidir.

## <a name="custom-build-commands"></a>Özel derleme komutları

Dağıtım sırasında hangi komutların çalıştığı hakkında ayrıntılı denetim sahibi olabilirsiniz. Aşağıdaki komutlar bir iş bölümünün altında tanımlanabilir `with` .

Dağıtım her zaman `npm install` özel bir komuttan önce çağırır.

| Komut            | Açıklama |
|---------------------|-------------|
| `app_build_command` | Statik içerik uygulamasının dağıtımı sırasında çalışacak özel bir komut tanımlar.<br><br>Örneğin, angular uygulaması için bir üretim yapısını yapılandırmak için, girin `ng build -prod` . Boş bırakılırsa, iş akışı `npm run build` veya komutlarını çalıştırmayı dener `npm run build:Azure` .  |
| `api_build_command` | Azure Işlevleri API uygulaması dağıtımı sırasında çalışacak özel bir komut tanımlar. |

## <a name="route-file-location"></a>Yol dosyası konumu

İş akışını, deponuzdaki herhangi bir klasörde [yönlendirmeler. JSON](routes.md) aramak için özelleştirebilirsiniz. Aşağıdaki özellik bir iş bölümünün altında tanımlanabilir `with` .

| Özellik            | Açıklama |
|---------------------|-------------|
| `routes_location` | _Routes. JSON_ dosyasının bulunduğu dizin konumunu tanımlar. Bu konum, deponun köküne göredir. |

 _Yolların_ konumuyla ilgili olarak açık olması, ön uç Framework oluşturma adımlarınızın bu dosyayı varsayılan olarak içine taşımaması durumunda özellikle önemlidir `app_artifact_location` .

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Ön üretim ortamlarında çekme isteklerini gözden geçirin](review-publish-pull-requests.md)
