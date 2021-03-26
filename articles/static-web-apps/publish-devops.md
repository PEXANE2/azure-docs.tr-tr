---
title: 'Öğretici: Azure DevOps ile Azure statik Web Apps yayımlama'
description: Azure statik Web Apps yayımlamak için Azure DevOps kullanmayı öğrenin.
services: static-web-apps
author: scubaninja
ms.service: static-web-apps
ms.topic: tutorial
ms.date: 03/23/2021
ms.author: apedward
ms.openlocfilehash: af359734ff5bfe90dedbb7f8389aecdc6e056654
ms.sourcegitcommit: 44edde1ae2ff6c157432eee85829e28740c6950d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/25/2021
ms.locfileid: "105543576"
---
# <a name="tutorial-publish-azure-static-web-apps-with-azure-devops"></a>Öğretici: Azure DevOps ile Azure statik Web Apps yayımlama

Bu makalede [Azure DevOps](https://dev.azure.com/)kullanılarak [Azure statik Web Apps](./overview.md) dağıtma işlemi gösterilmektedir.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

- Azure statik Web Apps sitesini ayarlama
- Statik bir Web uygulaması derlemek ve yayımlamak için bir Azure DevOps işlem hattı oluşturma

## <a name="prerequisites"></a>Önkoşullar

- **Etkin Azure hesabı:** Bir hesabınız yoksa, [ücretsiz olarak bir hesap oluşturabilirsiniz](https://azure.microsoft.com/free/).
- **Azure DevOps projesi:** Bir tane yoksa, [ücretsiz olarak bir proje oluşturabilirsiniz](https://azure.microsoft.com/pricing/details/devops/azure-devops-services/).
- **Azure DevOps Işlem hattı:** Başlamak için yardıma ihtiyacınız varsa, bkz. [ilk işlem hattınızı oluşturma](https://docs.microsoft.com/azure/devops/pipelines/create-first-pipeline?view=azure-devops&preserve-view=true).

## <a name="create-a-static-web-app-in-an-azure-devops-repository"></a>Azure DevOps deposunda statik bir Web uygulaması oluşturma

  > [!NOTE]
  > Deponuzda mevcut bir uygulamanız varsa, sonraki bölüme atlayabilirsiniz.

1. Azure DevOps deponuza gidin.

1. Mevcut bir depoyu kullanın veya _bir depoyu_ aşağıda gösterildiği gibi içeri aktarın.
  
    :::image type="content" source="media/publish-devops/devops-repo.png" alt-text="DevOps deposu":::

1. Ön uç Web uygulamanız için yeni bir dosya oluşturun.

1. Aşağıdaki HTML işaretlemesini kopyalayıp yeni dosyanıza yapıştırın:

    ```html
    <!DOCTYPE html>
    <html lang="en">
  
    <head>
      <meta charset="UTF-8">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <link rel="stylesheet" href="styles.css">
      <title>Hello World!</title>
    </head>
  
    <body>
      <main>
        <h1>Hello World!</h1>
      </main>
    </body>
  
    </html>
    ```

1. Dosyayı kaydedin.

## <a name="create-a-static-web-app"></a>Statik web uygulaması oluşturma

1. [Azure Portal](https://portal.azure.com)gidin.

1. **Kaynak Oluştur**’u seçin.

1. **Statik Web Apps** arayın.

1. **Statik Web Apps seçin (Önizleme)**.

1. **Oluştur**’u seçin.

1. _Dağıtım ayrıntıları_ ' nın altında **diğer**' i seçtiğinizden emin olun. Bu, Azure DevOps deponuzdaki kodu kullanmanıza olanak sağlar.

    > [!NOTE]
    > _Diğer_ ' i seçme işlevselliği Şu anda kullanıma sunulmuştur ve henüz tüm Azure aboneliklerinde kullanılamıyor olabilir.

    :::image type="content" source="media/publish-devops/create-resource.png" alt-text="Dağıtım ayrıntıları-diğer":::

1. Dağıtım başarılı olduktan sonra **dağıtım belirtecini Yönet**' i seçin.

1. **Dağıtım belirtecini** kopyalayın ve başka bir ekranda kullanmak üzere bir metin düzenleyicisine yapıştırın.

    > [!NOTE]
    > Bu değer, önümüzdeki adımlarda daha fazla değer kopyalayacağından, bu değerin Şu anda ayrılmış olarak ayarlanır.

    :::image type="content" source="media/publish-devops/deployment-token.png" alt-text="Dağıtım belirteci":::

## <a name="create-the-pipeline-task-in-azure-devops"></a>Azure DevOps 'da işlem hattı görevi oluşturma

1. Daha önce oluşturulan Azure DevOps projesine gidin.

2. Yeni bir **derleme Işlem hattı** oluşturun ve **derlemeyi ayarla**' yı seçin.

    :::image type="content" source="media/publish-devops/azdo-build.png" alt-text="Derleme işlem hattı":::

3. Aşağıdaki YAML 'yi kopyalayıp işlem hattınızla yapıştırın.

    > [!NOTE]
    > _App_location_,_api_location_ ve _output_location_ için girilen değerlerin uygulamanız için değiştirilmesi gerekir.  

    ```yaml
    trigger:
      - main
    
    pool:
      vmImage: ubuntu-latest
    
    steps:
      - task: AzureStaticWebApp@0
        inputs:
          app_location: frontend 
          api_location: api
          output_location: build
        env:
          azure_static_web_apps_api_token: $(deployment_token)
    ```

    Azure statik Web uygulaması girişlerini uygulamanızın klasör yapısına göre yapılandırın.

    [!INCLUDE [static-web-apps-folder-structure](../../includes/static-web-apps-folder-structure.md)]

    `azure_static_web_apps_api_token`Değer kendi kendine yönetilir ve el ile yapılandırılır.

4. **Değişkenler**' i seçin.

5. Yeni bir değişken oluşturun.

6. Değişkeni **deployment_token** (iş akışındaki ad ile eşleşen) adlandırın.

7. Daha önce bir metin düzenleyicisine yapıştırdığınız dağıtım belirtecini kopyalayın.

8. _Değer_ kutusuna dağıtım belirtecine yapıştırın.

    :::image type="content" source="media/publish-devops/variable-token.png" alt-text="Değişken belirteci":::

9. **Tamam**’ı seçin.

10. **Kaydet '** i seçin ve işlem hattını çalıştırın.

    :::image type="content" source="media/publish-devops/save-and-run.png" alt-text="İşlem Hattı":::

11. Dağıtım başarılı olduktan sonra dağıtım yapılandırmasına bağlantılar içeren Azure statik Web Apps **genel bakışa** gidin.

12. Yeni dağıtılan Web sitenizi görmek için **URL 'yi** seçin. _Kaynak_ bağlantının artık Azure DevOps deposunun dalını ve konumunu nasıl işaret ettiğini unutmayın.

    :::image type="content" source="media/publish-devops/deployment-location.png" alt-text="Dağıtım konumu":::

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure statik Web Apps yapılandırma](./configuration.md)
