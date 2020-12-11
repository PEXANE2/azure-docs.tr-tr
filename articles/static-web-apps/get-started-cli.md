---
title: 'Hızlı başlangıç: CLı kullanarak ilk statik sitenizi Azure statik Web Apps oluşturma'
description: Azure CLı ile statik bir siteyi Azure statik Web Apps dağıtmayı öğrenin.
services: static-web-apps
author: craigshoemaker
ms.service: static-web-apps
ms.topic: quickstart
ms.date: 08/13/2020
ms.author: cshoe
ms.openlocfilehash: ca05194c0d8e63290a9cab8d9f7903daa192069b
ms.sourcegitcommit: 3ea45bbda81be0a869274353e7f6a99e4b83afe2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2020
ms.locfileid: "97030357"
---
# <a name="quickstart-building-your-first-static-site-using-the-azure-cli"></a>Hızlı başlangıç: Azure CLı kullanarak ilk statik sitenizi oluşturma

Azure statik Web Apps bir GitHub deposundan uygulama oluşturarak bir Web sitesini üretim ortamına yayınlar. Bu hızlı başlangıçta, Azure CLı kullanarak Azure statik Web uygulamalarına bir Web uygulaması dağıtırsınız.

Azure aboneliğiniz yoksa [ücretsiz bir deneme hesabı oluşturun](https://azure.microsoft.com/free).

## <a name="prerequisites"></a>Önkoşullar

- [GitHub](https://github.com) hesabı
- [GitHub kişisel erişim belirteci](https://docs.github.com/github/authenticating-to-github/creating-a-personal-access-token)
- [Azure](https://portal.azure.com) hesabı
- [Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest) yüklü (sürüm 2.8.0 ve üzeri)

[!INCLUDE [create repository from template](../../includes/static-web-apps-get-started-create-repo.md)]

[!INCLUDE [clone the repository](../../includes/static-web-apps-get-started-clone-repo.md)]

Ardından, aşağıdaki komutu kullanarak yeni klasöre geçiş yapın.

```bash
cd my-first-static-web-app
```

## <a name="create-a-static-web-app"></a>Statik web uygulaması oluşturma

Artık depo oluşturulduktan sonra, Azure CLı 'den statik bir Web uygulaması oluşturabilirsiniz.

> [!IMPORTANT]
> Terminalinizdeki _My-static-Web-App_ klasöründe olduğunuzdan emin olun.

1. Aşağıdaki komutu kullanarak Azure CLı 'da oturum açın.

    ```bash
    az login
    ```

1. Deponuzdan yeni bir statik Web uygulaması oluşturun.

    # <a name="no-framework"></a>[Çerçeve yok](#tab/vanilla-javascript)

    ```bash
    az staticwebapp create \
        -n my-first-static-web-app \
        -g <RESOURCE_GROUP_NAME> \
        -s https://github.com/<YOUR_GITHUB_ACCOUNT_NAME>/my-first-static-web-app \
        -l <LOCATION> \
        -b master \
        --token <YOUR_GITHUB_PERSONAL_ACCESS_TOKEN>
    ```

    # <a name="angular"></a>[Angular](#tab/angular)

    ```bash
    az staticwebapp create \
        -n my-first-static-web-app \
        -g <RESOURCE_GROUP_NAME> \
        -s https://github.com/<YOUR_GITHUB_ACCOUNT_NAME>/my-first-static-web-app \
        -l <LOCATION> \
        -b master \
        --app-artifact-location "dist/angular-basic" \
        --token <YOUR_GITHUB_PERSONAL_ACCESS_TOKEN>
    ```

    # <a name="react"></a>[React](#tab/react)

    ```bash
    az staticwebapp create \
        -n my-first-static-web-app \
        -g <RESOURCE_GROUP_NAME> \
        -s https://github.com/<YOUR_GITHUB_ACCOUNT_NAME>/my-first-static-web-app \
        -l <LOCATION> \
        -b master \
        --app-artifact-location "build" \
        --token <YOUR_GITHUB_PERSONAL_ACCESS_TOKEN>
    ```

    # <a name="vue"></a>[Vue](#tab/vue)

    ```bash
    az staticwebapp create \
        -n my-first-static-web-app \
        -g <RESOURCE_GROUP_NAME> \
        -s https://github.com/<YOUR_GITHUB_ACCOUNT_NAME>/my-first-static-web-app \
        -l <LOCATION> \
        -b master \
        --app-artifact-location "dist" \
        --token <YOUR_GITHUB_PERSONAL_ACCESS_TOKEN>
    ```

    ---

    - `<RESOURCE_GROUP_NAME>`: Bu değeri mevcut bir Azure Kaynak grubu adıyla değiştirin.

    - `<YOUR_GITHUB_ACCOUNT_NAME>`: Bu değeri GitHub Kullanıcı adınızla değiştirin.

    - `<LOCATION>`: Bu değeri en yakın konum ile değiştirin. Seçenekler şunlardır: _merkezde US_, _eastaya_, _EastUS2_, _westeurope_ ve _WestUS2_.

    - `<YOUR_GITHUB_PERSONAL_ACCESS_TOKEN>`: Bu değeri, daha önce oluşturmuş olduğunuz [GitHub kişisel erişim belirteci](https://docs.github.com/github/authenticating-to-github/creating-a-personal-access-token) ile değiştirin.

    Artık oluşturulan uygulamayı Azure 'da görüntüleyebilirsiniz.

1. [Azure portalını](https://portal.azure.com) açın.

1. En üst arama çubuğundan **My-First-Web-static-App** ' i arayın.

1. **My-First-Web-static-App**' i seçin.

[!INCLUDE [view website](../../includes/static-web-apps-get-started-view-website.md)]

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu uygulamayı kullanmaya devam edemeyecekecekseniz, aşağıdaki komutu çalıştırarak Azure statik Web Apps örneğini silebilirsiniz:

```bash
az staticwebapp delete \
    --name my-first-static-web-app
    --resource-group my-first-static-web-app
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [API ekleme](add-api.md)
