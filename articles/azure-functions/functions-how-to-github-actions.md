---
title: Azure Işlevlerinde kod güncelleştirmeleri yapmak için GitHub eylemlerini kullanma
description: GitHub 'da Azure Işlevleri projelerini derlemek ve dağıtmak için bir iş akışı tanımlamak üzere GitHub eylemlerini nasıl kullanacağınızı öğrenin.
author: craigshoemaker
ms.topic: conceptual
ms.date: 10/07/2020
ms.author: cshoe
ms.custom: devx-track-csharp, devx-track-python, github-actions-azure
ms.openlocfilehash: a2d5234b3c80456a98fde4547b9665ca1b0a83dd
ms.sourcegitcommit: d76108b476259fe3f5f20a91ed2c237c1577df14
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/29/2020
ms.locfileid: "92913555"
---
# <a name="continuous-delivery-by-using-github-action"></a>GitHub eylemini kullanarak sürekli teslim

Azure işlev uygulamanıza otomatik olarak kod derlemek ve dağıtmak için bir iş akışı tanımlamak üzere [GitHub eylemlerini](https://github.com/features/actions) kullanın. 

GitHub eylemlerinde, bir [iş akışı](https://help.github.com/articles/about-github-actions#workflow) GitHub deponuzda tanımladığınız otomatikleştirilmiş bir işlemdir. Bu süreç, GitHub 'da işlevler uygulama projenizi nasıl oluşturup dağıtacağınızı gösterir. 

Bir iş akışı, deponuzdaki yoldaki bir YAML (. yıml) dosyası tarafından tanımlanır `/.github/workflows/` . Bu tanım, iş akışını oluşturan çeşitli adımları ve parametreleri içerir. 

Azure Işlevleri iş akışı için, dosyanın üç bölümü vardır: 

| Section | Görevler |
| ------- | ----- |
| **Kimlik Doğrulaması** | Bir yayımlama profili indirin.<br/>GitHub gizli dizisi oluşturun.|
| **Derleme** | Ortamı ayarlayın.<br/>İşlev uygulamasını oluşturun.|
| **Dağıtma** | İşlev uygulamasını dağıtın.|

## <a name="prerequisites"></a>Önkoşullar

- Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- GitHub hesabı. Bir hesabınız yoksa [ücretsiz](https://github.com/join)kaydolun.  
- GitHub deposu ile Azure üzerinde barındırılan bir çalışan işlev uygulaması.   
    - [Hızlı Başlangıç: Visual Studio Code’u kullanarak Azure’da işlev oluşturma](functions-create-first-function-vs-code.md)

## <a name="generate-deployment-credentials"></a>Dağıtım kimlik bilgileri oluştur

GitHub eylemleri için Azure Işlevleri ile kimlik doğrulamak için önerilen yol, bir yayımlama profili kullanmaktır. Hizmet sorumlusu ile de kimlik doğrulaması yapabilirsiniz. Daha fazla bilgi edinmek için [Bu GitHub eylemleri deposuna](https://github.com/Azure/functions-action)bakın. 

Yayımlama profili kimlik bilgilerinizi [GitHub gizli anahtarı](https://docs.github.com/en/actions/reference/encrypted-secrets)olarak kaydettikten sonra Azure ile kimlik doğrulamak için bu gizli anahtarı iş akışınız dahilinde kullanacaksınız. 

#### <a name="download-your-publish-profile"></a>Yayımlama profilinizi indirin

İşlev uygulamanızın yayımlama profilini indirmek için:

1. İşlev uygulamasının **genel bakış** sayfasını seçin ve ardından **Yayımlama profili al** ' ı seçin.

   :::image type="content" source="media/functions-how-to-github-actions/get-publish-profile.png" alt-text="Yayımlama profilini indir":::

1. Dosyanın içeriğini kaydedin ve kopyalayın.


### <a name="add-the-github-secret"></a>GitHub gizliliğini ekleyin

1. [GitHub](https://github.com)'da deponuza gidin, **Ayarlar**  >  **gizli** dizileri  >  **Yeni bir parola ekleyin** ' i seçin.

   :::image type="content" source="media/functions-how-to-github-actions/add-secret.png" alt-text="Yayımlama profilini indir":::

1. `AZURE_FUNCTIONAPP_PUBLISH_PROFILE` **Ad** , yayımlama profili dosyasının içeriği için kullanarak yeni bir gizli dizi ekleyin ve sonra gizli **Value** dizi **Ekle** ' yi seçin.

GitHub artık Azure 'daki işlev uygulamanıza kimlik doğrulaması yapabilir.

## <a name="create-the-environment"></a>Ortamı oluşturma 

Ortamı ayarlamak, dile özgü bir yayımlama kurulum eylemi kullanılarak yapılır.

# <a name="net"></a>[.NET](#tab/dotnet)

.NET (ASP.NET dahil) eylemi kullanır `actions/setup-dotnet` .  
Aşağıdaki örnek, ortamı ayarlayan iş akışının parçasını gösterir:

```yaml
    - name: Setup DotNet 2.2.402 Environment
      uses: actions/setup-dotnet@v1
      with:
        dotnet-version: 2.2.402
```

# <a name="java"></a>[Java](#tab/java)

Java bu  `actions/setup-java` eylemi kullanır.  
Aşağıdaki örnek, ortamı ayarlayan iş akışının parçasını gösterir:

```yaml
    - name: Setup Java 1.8.x
      uses: actions/setup-java@v1
      with:
        # If your pom.xml <maven.compiler.source> version is not in 1.8.x
        # Please change the Java version to match the version in pom.xml <maven.compiler.source>
        java-version: '1.8.x'
```

# <a name="javascript"></a>[JavaScript](#tab/javascript)

JavaScript (Node.js) eylemi kullanır `actions/setup-node` .  
Aşağıdaki örnek, ortamı ayarlayan iş akışının parçasını gösterir:

```yaml

    - name: Setup Node 12.x Environment
      uses: actions/setup-node@v1
      with:
        node-version: 12.x
```

# <a name="python"></a>[Python](#tab/python)

Python eylemi kullanır `actions/setup-python` .  
Aşağıdaki örnek, ortamı ayarlayan iş akışının parçasını gösterir:

```yaml
    - name: Setup Python 3.7 Environment
      uses: actions/setup-python@v1
      with:
        python-version: 3.7
```
---

## <a name="build-the-function-app"></a>İşlev uygulaması oluşturma

Bu, dile ve Azure Işlevleri tarafından desteklenen dillere bağlı olarak, bu bölümün her dilin standart derleme adımları olması gerekir.

Aşağıdaki örnek, dile özgü olan işlev uygulamasını oluşturan iş akışının parçasını gösterir:

# <a name="net"></a>[.NET](#tab/dotnet)

```yaml
    env:
      AZURE_FUNCTIONAPP_PACKAGE_PATH: '.' # set this to the path to your web app project, defaults to the repository root

    - name: 'Resolve Project Dependencies Using Dotnet'
      shell: bash
      run: |
        pushd './${{ env.AZURE_FUNCTIONAPP_PACKAGE_PATH }}'
        dotnet build --configuration Release --output ./output
        popd
```

# <a name="java"></a>[Java](#tab/java)

```yaml
    env:
      POM_XML_DIRECTORY: '.'  # set this to the directory which contains pom.xml file

    - name: 'Restore Project Dependencies Using Mvn'
      shell: bash
      run: |
        pushd './${{ env.POM_XML_DIRECTORY }}'
        mvn clean package
        mvn azure-functions:package
        popd
```

# <a name="javascript"></a>[JavaScript](#tab/javascript)

```yaml
    env:
      AZURE_FUNCTIONAPP_PACKAGE_PATH: '.'  # set this to the path to your web app project, defaults to the repository root

    - name: 'Resolve Project Dependencies Using Npm'
      shell: bash
      run: |
        pushd './${{ env.AZURE_FUNCTIONAPP_PACKAGE_PATH }}'
        npm install
        npm run build --if-present
        npm run test --if-present
        popd
```

# <a name="python"></a>[Python](#tab/python)

```yaml
    env:
      AZURE_FUNCTIONAPP_PACKAGE_PATH: '.' # set this to the path to your web app project, defaults to the repository root

    - name: 'Resolve Project Dependencies Using Pip'
      shell: bash
      run: |
        pushd './${{ env.AZURE_FUNCTIONAPP_PACKAGE_PATH }}'
        python -m pip install --upgrade pip
        pip install -r requirements.txt --target=".python_packages/lib/site-packages"
        popd
```
---

## <a name="deploy-the-function-app"></a>İşlev uygulamasını dağıtma
`Azure/functions-action`Kodunuzu bir işlev uygulamasına dağıtmak için eylemini kullanın. Bu eylemin üç parametresi vardır:

|Parametre |Açıklama  |
|---------|---------|
|_**uygulama adı**_ | Girilmesi İşlev uygulamanızın adı. |
|_**yuva adı**_ | Seçim Dağıtmak istediğiniz [dağıtım yuvasının](functions-deployment-slots.md) adı. Yuva, işlev uygulamanızda zaten tanımlanmış olmalıdır. |
|_**Yayımlama profili**_ | Seçim Yayımlama profiliniz için GitHub parolasının adı. |

Aşağıdaki örnek, `functions-action` `publish profile` kimlik doğrulaması için ve için olan 1 sürümünü kullanır 

# <a name="net"></a>[.NET](#tab/dotnet)

Yayımlama profili kullanan bir .NET Linux iş akışı ayarlayın.

```yaml
name: Deploy DotNet project to Azure function app with a Linux environment

on:
  [push]

env:
  AZURE_FUNCTIONAPP_NAME: your-app-name  # set this to your application's name
  AZURE_FUNCTIONAPP_PACKAGE_PATH: '.'    # set this to the path to your web app project, defaults to the repository root
  DOTNET_VERSION: '2.2.402'              # set this to the dotnet version to use

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
    - name: 'Checkout GitHub Action'
      uses: actions/checkout@master

    - name: Setup DotNet ${{ env.DOTNET_VERSION }} Environment
      uses: actions/setup-dotnet@v1
      with:
        dotnet-version: ${{ env.DOTNET_VERSION }}

    - name: 'Resolve Project Dependencies Using Dotnet'
      shell: bash
      run: |
        pushd './${{ env.AZURE_FUNCTIONAPP_PACKAGE_PATH }}'
        dotnet build --configuration Release --output ./output
        popd
    - name: 'Run Azure Functions Action'
      uses: Azure/functions-action@v1
      id: fa
      with:
        app-name: ${{ env.AZURE_FUNCTIONAPP_NAME }}
        package: '${{ env.AZURE_FUNCTIONAPP_PACKAGE_PATH }}/output'
        publish-profile: ${{ secrets.AZURE_FUNCTIONAPP_PUBLISH_PROFILE }}
```
Yayımlama profili kullanan bir .NET Windows iş akışı ayarlayın.

```yaml
name: Deploy DotNet project to Azure function app with a Windows environment

on:
  [push]

env:
  AZURE_FUNCTIONAPP_NAME: your-app-name  # set this to your application's name
  AZURE_FUNCTIONAPP_PACKAGE_PATH: '.'    # set this to the path to your web app project, defaults to the repository root
  DOTNET_VERSION: '2.2.402'              # set this to the dotnet version to use

jobs:
  build-and-deploy:
    runs-on: windows-latest
    steps:
    - name: 'Checkout GitHub Action'
      uses: actions/checkout@master

    - name: Setup DotNet ${{ env.DOTNET_VERSION }} Environment
      uses: actions/setup-dotnet@v1
      with:
        dotnet-version: ${{ env.DOTNET_VERSION }}

    - name: 'Resolve Project Dependencies Using Dotnet'
      shell: pwsh
      run: |
        pushd './${{ env.AZURE_FUNCTIONAPP_PACKAGE_PATH }}'
        dotnet build --configuration Release --output ./output
        popd
    - name: 'Run Azure Functions Action'
      uses: Azure/functions-action@v1
      id: fa
      with:
        app-name: ${{ env.AZURE_FUNCTIONAPP_NAME }}
        package: '${{ env.AZURE_FUNCTIONAPP_PACKAGE_PATH }}/output'
        publish-profile: ${{ secrets.AZURE_FUNCTIONAPP_PUBLISH_PROFILE }}
```

# <a name="java"></a>[Java](#tab/java)

Yayımlama profili kullanan bir Java Linux iş akışı ayarlayın.

```yaml
name: Deploy Java project to Azure Function App

on:
  [push]

env:
  AZURE_FUNCTIONAPP_NAME: your-app-name      # set this to your function app name on Azure
  POM_XML_DIRECTORY: '.'                     # set this to the directory which contains pom.xml file
  POM_FUNCTIONAPP_NAME: your-app-name        # set this to the function app name in your local development environment
  JAVA_VERSION: '1.8.x'                      # set this to the dotnet version to use

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
    - name: 'Checkout GitHub Action'
      uses: actions/checkout@master

    - name: Setup Java Sdk ${{ env.JAVA_VERSION }}
      uses: actions/setup-java@v1
      with:
        java-version: ${{ env.JAVA_VERSION }}

    - name: 'Restore Project Dependencies Using Mvn'
      shell: bash
      run: |
        pushd './${{ env.POM_XML_DIRECTORY }}'
        mvn clean package
        mvn azure-functions:package
        popd
    - name: 'Run Azure Functions Action'
      uses: Azure/functions-action@v1
      id: fa
      with:
        app-name: ${{ env.AZURE_FUNCTIONAPP_NAME }}
        package: './${{ env.POM_XML_DIRECTORY }}/target/azure-functions/${{ env.POM_FUNCTIONAPP_NAME }}'
        publish-profile: ${{ secrets.AZURE_FUNCTIONAPP_PUBLISH_PROFILE }}
```

Yayımlama profili kullanan bir Java Windows iş akışı ayarlayın.

```yaml
name: Deploy Java project to Azure Function App

on:
  [push]

env:
  AZURE_FUNCTIONAPP_NAME: your-app-name      # set this to your function app name on Azure
  POM_XML_DIRECTORY: '.'                     # set this to the directory which contains pom.xml file
  POM_FUNCTIONAPP_NAME: your-app-name        # set this to the function app name in your local development environment
  JAVA_VERSION: '1.8.x'                      # set this to the java version to use

jobs:
  build-and-deploy:
    runs-on: windows-latest
    steps:
    - name: 'Checkout GitHub Action'
      uses: actions/checkout@master

    - name: Setup Java Sdk ${{ env.JAVA_VERSION }}
      uses: actions/setup-java@v1
      with:
        java-version: ${{ env.JAVA_VERSION }}

    - name: 'Restore Project Dependencies Using Mvn'
      shell: pwsh
      run: |
        pushd './${{ env.POM_XML_DIRECTORY }}'
        mvn clean package
        mvn azure-functions:package
        popd
    - name: 'Run Azure Functions Action'
      uses: Azure/functions-action@v1
      id: fa
      with:
        app-name: ${{ env.AZURE_FUNCTIONAPP_NAME }}
        package: './${{ env.POM_XML_DIRECTORY }}/target/azure-functions/${{ env.POM_FUNCTIONAPP_NAME }}'
        publish-profile: ${{ secrets.AZURE_FUNCTIONAPP_PUBLISH_PROFILE }}
```

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Yayımlama profili kullanan bir Node.JS Linux iş akışı ayarlayın.

```yaml
name: Deploy Node.js project to Azure Function App

on:
  [push]

env:
  AZURE_FUNCTIONAPP_NAME: your-app-name    # set this to your application's name
  AZURE_FUNCTIONAPP_PACKAGE_PATH: '.'      # set this to the path to your web app project, defaults to the repository root
  NODE_VERSION: '12.x'                     # set this to the node version to use (supports 8.x, 10.x, 12.x)

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
    - name: 'Checkout GitHub Action'
      uses: actions/checkout@master

    - name: Setup Node ${{ env.NODE_VERSION }} Environment
      uses: actions/setup-node@v1
      with:
        node-version: ${{ env.NODE_VERSION }}

    - name: 'Resolve Project Dependencies Using Npm'
      shell: bash
      run: |
        pushd './${{ env.AZURE_FUNCTIONAPP_PACKAGE_PATH }}'
        npm install
        npm run build --if-present
        npm run test --if-present
        popd
    - name: 'Run Azure Functions Action'
      uses: Azure/functions-action@v1
      id: fa
      with:
        app-name: ${{ env.AZURE_FUNCTIONAPP_NAME }}
        package: ${{ env.AZURE_FUNCTIONAPP_PACKAGE_PATH }}
        publish-profile: ${{ secrets.AZURE_FUNCTIONAPP_PUBLISH_PROFILE }}
```

Yayımlama profili kullanan Node.JS bir Windows iş akışı ayarlayın.

```yaml
name: Deploy Node.js project to Azure Function App

on:
  [push]

env:
  AZURE_FUNCTIONAPP_NAME: your-app-name    # set this to your application's name
  AZURE_FUNCTIONAPP_PACKAGE_PATH: '.'      # set this to the path to your web app project, defaults to the repository root
  NODE_VERSION: '10.x'                     # set this to the node version to use (supports 8.x, 10.x, 12.x)

jobs:
  build-and-deploy:
    runs-on: windows-latest
    steps:
    - name: 'Checkout GitHub Action'
      uses: actions/checkout@master

    - name: Setup Node ${{ env.NODE_VERSION }} Environment
      uses: actions/setup-node@v1
      with:
        node-version: ${{ env.NODE_VERSION }}

    - name: 'Resolve Project Dependencies Using Npm'
      shell: pwsh
      run: |
        pushd './${{ env.AZURE_FUNCTIONAPP_PACKAGE_PATH }}'
        npm install
        npm run build --if-present
        npm run test --if-present
        popd
    - name: 'Run Azure Functions Action'
      uses: Azure/functions-action@v1
      id: fa
      with:
        app-name: ${{ env.AZURE_FUNCTIONAPP_NAME }}
        package: ${{ env.AZURE_FUNCTIONAPP_PACKAGE_PATH }}
        publish-profile: ${{ secrets.AZURE_FUNCTIONAPP_PUBLISH_PROFILE }}

```
# <a name="python"></a>[Python](#tab/python)

Yayımlama profili kullanan bir Python Linux iş akışı ayarlayın.

```yaml
name: Deploy Python project to Azure Function App

on:
  [push]

env:
  AZURE_FUNCTIONAPP_NAME: your-app-name # set this to your application's name
  AZURE_FUNCTIONAPP_PACKAGE_PATH: '.'   # set this to the path to your web app project, defaults to the repository root
  PYTHON_VERSION: '3.7'                 # set this to the python version to use (supports 3.6, 3.7, 3.8)

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
    - name: 'Checkout GitHub Action'
      uses: actions/checkout@master

    - name: Setup Python ${{ env.PYTHON_VERSION }} Environment
      uses: actions/setup-python@v1
      with:
        python-version: ${{ env.PYTHON_VERSION }}

    - name: 'Resolve Project Dependencies Using Pip'
      shell: bash
      run: |
        pushd './${{ env.AZURE_FUNCTIONAPP_PACKAGE_PATH }}'
        python -m pip install --upgrade pip
        pip install -r requirements.txt --target=".python_packages/lib/site-packages"
        popd
    - name: 'Run Azure Functions Action'
      uses: Azure/functions-action@v1
      id: fa
      with:
        app-name: ${{ env.AZURE_FUNCTIONAPP_NAME }}
        package: ${{ env.AZURE_FUNCTIONAPP_PACKAGE_PATH }}
        publish-profile: ${{ secrets.AZURE_FUNCTIONAPP_PUBLISH_PROFILE }}
```

---

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure ve GitHub tümleştirmesi hakkında daha fazla bilgi edinin](/azure/developer/github/)
