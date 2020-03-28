---
title: Jenkins Azure İşlevlerini kullanarak Azure İşlevlerini Dağıtma eklentisini
description: Jenkins Azure Fonksiyonları eklentisini kullanarak Azure İşlevlerini nasıl dağıttığınızı öğrenin
keywords: jenkins, azure, devops, java, azure fonksiyonları
ms.topic: tutorial
ms.date: 10/23/2019
ms.openlocfilehash: 731bac13a596bbeaf970b3f6ce976a582d1f11ae
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "78250916"
---
# <a name="deploy-to-azure-functions-using-the-jenkins-azure-functions-plug-in"></a>Jenkins Azure İşlevlerini kullanarak Azure İşlevlerini Dağıtma eklentisini

[Azure İşlevler](/azure/azure-functions/) sunucusuz bir bilgi işlem hizmetidir. Azure İşlevlerini kullanarak, altyapı sağlamadan veya yönetmeden isteğe bağlı kod çalıştırabilirsiniz. Bu öğretici, Azure İşlevleri eklentisini kullanarak Bir Java işleviniAzure İşlevlerine nasıl dağıtılanın gösterir.

## <a name="prerequisites"></a>Ön koşullar

- **Azure aboneliği**: Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) oluşturun.
- **Jenkins sunucusu**: Jenkins sunucunuz yüklü değilse, makaleye bakın, [Azure'da bir Jenkins sunucusu oluşturun.](./install-jenkins-solution-template.md)

  > [!TIP]
  > Bu öğretici için kullanılan kaynak kodu [Visual Studio Çin GitHub repo](https://github.com/VSChina/odd-or-even-function/blob/master/src/main/java/com/microsoft/azure/Function.java)bulunur.

## <a name="create-a-java-function"></a>Java işlevi oluşturma

Java çalışma zamanı yığınıyla bir Java işlevi oluşturmak için [Azure portalını](https://portal.azure.com) veya [Azure CLI'yi](/cli/azure/?view=azure-cli-latest)kullanın.

Aşağıdaki adımlar, Azure CLI'yi kullanarak java işlevinin nasıl oluşturulurunu gösterir:

1. ** &lt;resource_group>** yer tutucuyu kaynak grup adınız ile değiştirerek bir kaynak grubu oluşturun.

    ```azurecli
    az group create --name <resource_group> --location eastus
    ```

1. Yer tutucuları uygun değerlerle değiştirerek bir Azure depolama hesabı oluşturun.
 
    ```azurecli
    az storage account create --name <storage_account> --location eastus --resource-group <resource_group> --sku Standard_LRS    
    ```

1. Yer tutucuları uygun değerlerle değiştirerek test işlevi uygulamasını oluşturun.

    ```azurecli
    az functionapp create --resource-group <resource_group> --consumption-plan-location eastus --name <function_app> --storage-account <storage_account>
    ```

## <a name="prepare-jenkins-server"></a>Jenkins sunucusuhazırlayın

Aşağıdaki adımlar Jenkins sunucusunun nasıl hazırlanacağını açıklar:

1. Azure'da bir [Jenkins sunucusu](https://aka.ms/jenkins-on-azure) dağıtın. Jenkins sunucusunun yüklü bir örneğini zaten yüklemediyseniz, [Azure'da jenkins sunucusu oluşturma](./install-jenkins-solution-template.md) makalesi işlem boyunca size yol göstericidir.

1. SSH ile Jenkins örneğine oturum açın.

1. Jenkins örneğinde, aşağıdaki komutu kullanarak maven'i yükleyin:

    ```terminal
    sudo apt install -y maven
    ```

1. Jenkins örneğinde, terminal isteminde aşağıdaki komutları vererek [Azure İşlevler Temel Araçları'nı](/azure/azure-functions/functions-run-local) yükleyin:

    ```terminal
    wget -q https://packages.microsoft.com/config/ubuntu/16.04/packages-microsoft-prod.deb
    sudo dpkg -i packages-microsoft-prod.deb
    sudo apt-get update
    sudo apt-get install azure-functions-core-tools
    ```

1. Jenkins panosunda aşağıdaki eklentileri yükleyin:

    - Azure İşlevler Eklentisi
    - EnvInject Eklentisi

1. Jenkins'in Azure kaynaklarına kimlik doğrulamak ve erişmek için bir Azure hizmet ilkesine ihtiyacı vardır. Adım adım talimatlar için [Azure Uygulama Hizmetine Dağıt'a](./tutorial-jenkins-deploy-web-app-azure-app-service.md) bakın.

1. Azure hizmet ilkesini kullanarak Jenkins'e "Microsoft Azure Hizmet Sorumlusu" kimlik bilgisi türü ekleyin. Azure Uygulama Hizmeti ne kadar dağıt ı [öğreticisine](./tutorial-jenkins-deploy-web-app-azure-app-service.md#add-service-principal-to-jenkins) bakın.

## <a name="fork-the-sample-github-repo"></a>Çatal örnek GitHub repo

1. [Tek veya örnek uygulama için GitHub repo'da oturum açın.](https://github.com/VSChina/odd-or-even-function.git)

1. GitHub'ın sağ üst köşesinde **Fork'u**seçin.

1. GitHub hesabınızı seçmek ve forking'i bitirmek için istemleri izleyin.

## <a name="create-a-jenkins-pipeline"></a>Jenkins Boru Hattı Oluştur

Bu bölümde, Jenkins [Boru Hattı](https://jenkins.io/doc/book/pipeline/)oluşturun.

1. Jenkins panosunda bir Boru Hattı oluşturun.

1. Çalışma **için ortam hazırlamayı etkinleştirin.**

1. Yer tutucuları ortamınız için uygun değerlerle değiştirerek **Özellikler İçeriği'ne**aşağıdaki ortam değişkenlerini ekleyin:

    ```
    AZURE_CRED_ID=<service_principal_credential_id>
    RES_GROUP=<resource_group>
    FUNCTION_NAME=<function_name>
    ```
    
1. **Pipeline->Definition** bölümünde, **SCM'den Pipeline komut dosyası'nı**seçin.

1. [JenkinsFile örneğinde](https://github.com/VSChina/odd-or-even-function/blob/master/doc/resources/jenkins/JenkinsFile)kullanmak üzere GitHub çatalınızın URL'sini ve komut dosyası yolunu ("doc/resources/jenkins/JenkinsFile") girin.

   ```
   node {
    stage('Init') {
        checkout scm
        }

    stage('Build') {
        sh 'mvn clean package'
        }

    stage('Publish') {
        azureFunctionAppPublish appName: env.FUNCTION_NAME, 
                                azureCredentialsId: env.AZURE_CRED_ID, 
                                filePath: '**/*.json,**/*.jar,bin/*,HttpTrigger-Java/*', 
                                resourceGroup: env.RES_GROUP, 
                                sourceDirectory: 'target/azure-functions/odd-or-even-function-sample'
        }
    }
    ```

## <a name="build-and-deploy"></a>Oluşturma ve dağıtma

Şimdi Jenkins işini yürütme zamanı.

1. İlk olarak, [Azure İşlevleri HTTP tetikleyicileri ve bağlamaları](/azure/azure-functions/functions-bindings-http-webhook-trigger#authorization-keys) makalesindeki yönergeler aracılığıyla yetkilendirme anahtarını edinin.

1. Tarayıcınıza uygulamanın URL'sini girin. Yer tutucuları uygun değerlerle değiştirin ve Java işlevi için giriş olarak ** &lt;input_number>** için sayısal bir değer belirtin.

    ```
    https://<function_app>.azurewebsites.net/api/HttpTrigger-Java?code=<authorization_key>&number=<input_number>
    ```
1. Aşağıdaki örnek çıktıya benzer sonuçlar görürsünüz (tek bir sayı - 365 - test olarak kullanılmıştır):

    ```output
    The number 365 is Odd.
    ```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu uygulamayı kullanmaya devam etmeyecekseniz, oluşturduğunuz kaynakları aşağıdaki adımla silin:

```azurecli
az group delete -y --no-wait -n <resource_group>
```

## <a name="next-steps"></a>Sonraki adımlar

Azure İşlevler hakkında daha fazla bilgi edinmek için aşağıdaki kaynağa bakın:
> [!div class="nextstepaction"]
> [Azure Fonksiyonları belgeleri](/azure/azure-functions/)
