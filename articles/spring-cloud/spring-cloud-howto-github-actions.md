---
title: GitHub Eylemleri ile Azure İlkbahar Bulut CI/CD
description: GitHub Eylemleri ile Azure Bahar Bulutu için CI/CD iş akışı oluşturma
author: MikeDodaro
ms.author: barbkess
ms.service: spring-cloud
ms.topic: how-to
ms.date: 01/15/2019
ms.openlocfilehash: 559c894a2212466761de820de7486ae203337802
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77538473"
---
# <a name="azure-spring-cloud-cicd-with-github-actions"></a>GitHub Eylemleri ile Azure İlkbahar Bulut CI/CD

GitHub Eylemleri, otomatik yazılım geliştirme yaşam döngüsü iş akışını destekler. Azure Bahar Bulutu için GitHub Eylemleri ile Azure'u oluşturmak, test etmek, paketlemek, serbest bırakmak ve Azure'a dağıtmak için deponuzda iş akışları oluşturabilirsiniz. 

## <a name="prerequisites"></a>Ön koşullar
Bu örnek, [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)gerektirir.

## <a name="set-up-github-repository-and-authenticate"></a>GitHub deposunu ayarlama ve kimlik doğrulaması
Azure oturum açma eylemini yetkilendirmek için bir Azure hizmet ilkesi kimlik bilgisine ihtiyacınız var. Azure kimlik bilgisi almak için yerel makinenizde aşağıdaki komutları uygulayın:
```
az login
az ad sp create-for-rbac --role contributor --scopes /subscriptions/<SUBSCRIPTION_ID> --sdk-auth 
```
Belirli bir kaynak grubuna erişmek için kapsamı azaltabilirsiniz:
```
az ad sp create-for-rbac --role contributor --scopes /subscriptions/<SUBSCRIPTION_ID>/resourceGroups/<RESOURCE_GROUP> --sdk-auth
```
Komut bir JSON nesnesi çıktı olmalıdır:
```JSON
{
    "clientId": "<GUID>",
    "clientSecret": "<GUID>",
    "subscriptionId": "<GUID>",
    "tenantId": "<GUID>",
    ...
}
```

Bu örnek, GitHub'daki [Piggy Metrics](https://github.com/Azure-Samples/piggymetrics) örneğini kullanır.  Çatal örneği, GitHub deposu sayfasını açın ve **Ayarlar** sekmesini **Secrets** tıklatın. **Add a new secret**

 ![Yeni sır ekleme](./media/github-actions/actions1.png)

Gizli adı `AZURE_CREDENTIALS` ve değerini *GitHub deponuzu ayarlayın ve kimlik doğrulamasını ayarlayın*başlığı altında bulduğunuz JSON dizesine ayarlayın.

 ![Gizli verileri ayarlama](./media/github-actions/actions2.png)

Ayrıca, [GitHub Eylemlerinde Key Vault ile Azure Baharını Authenticate Azure Spring'de](./spring-cloud-github-actions-key-vault.md)açıklandığı gibi, GitHub'daki Key Vault eylemlerinden De Vesa'dan Azure giriş kimlik belgesini de alabilirsiniz.

## <a name="provision-service-instance"></a>Sağlama hizmeti örneği
Azure İlkbahar Bulutu hizmet örneğinizi sağlamak için Azure CLI'yi kullanarak aşağıdaki komutları çalıştırın.
```
az extension add --name spring-cloud
az group create --location eastus --name <resource group name>
az spring-cloud create -n <service instance name> -g <resource group name>
az spring-cloud config-server git set -n <service instance name> --uri https://github.com/xxx/piggymetrics --label config
```
## <a name="build-the-workflow"></a>İş akışını oluşturma
İş akışı aşağıdaki seçenekler kullanılarak tanımlanır.

### <a name="prepare-for-deployment-with-azure-cli"></a>Azure CLI ile dağıtıma hazırlanın
Komut `az spring-cloud app create` şu anda idempotent değil.  Bu iş akışını mevcut Azure İlkbahar Bulutu uygulamalarında ve örneklerinde öneririz.

Hazırlık için aşağıdaki Azure CLI komutlarını kullanın:
```
az configure --defaults group=<service group name>
az configure --defaults spring-cloud=<service instance name>
az spring-cloud app create --name gateway
az spring-cloud app create --name auth-service
az spring-cloud app create --name account-service
```

### <a name="deploy-with-azure-cli-directly"></a>Azure CLI ile doğrudan dağıtma
Dosyayı `.github/workflow/main.yml` depoda oluşturun:

```
name: AzureSpringCloud
on: push

env:
  GROUP: <resource group name>
  SERVICE_NAME: <service instance name>

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
    
    - uses: actions/checkout@master
    
    - name: Set up JDK 1.8
      uses: actions/setup-java@v1
      with:
        java-version: 1.8
    
    - name: maven build, clean
      run: |
        mvn clean package -D skipTests
    
    - name: Azure Login
      uses: azure/login@v1
      with:
        creds: ${{ secrets.AZURE_CREDENTIALS }}
      
    - name: Install ASC AZ extension
      run: az extension add --name spring-cloud
   
    - name: Deploy with AZ CLI commands
      run: |
        az configure --defaults group=$GROUP
        az configure --defaults spring-cloud=$SERVICE_NAME
        az spring-cloud app deploy -n gateway --jar-path ${{ github.workspace }}/gateway/target/gateway.jar
        az spring-cloud app deploy -n account-service --jar-path ${{ github.workspace }}/account-service/target/account-service.jar
        az spring-cloud app deploy -n auth-service --jar-path ${{ github.workspace }}/auth-service/target/auth-service.jar
```
### <a name="deploy-with-azure-cli-action"></a>Azure CLI eylemiyle dağıtma
Az `run` komutu Azure CLI'nin en son sürümünü kullanır. Kesme değişiklikleri varsa, Azure CLI'nin azure/CLI `action`ile belirli bir sürümünü de kullanabilirsiniz. 

> [!Note] 
> Bu komut yeni bir kapsayıcıda `env` çalışır, bu nedenle çalışmaz ve çapraz eylem dosyası erişimi nde ek kısıtlamalar olabilir.

Depoda .github/iş akışı/main.yml dosyasını oluşturun:
```
name: AzureSpringCloud
on: push

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
    
    - uses: actions/checkout@master
    
    - name: Set up JDK 1.8
      uses: actions/setup-java@v1
      with:
        java-version: 1.8
    
    - name: maven build, clean
      run: |
        mvn clean package -D skipTests
        
    - name: Azure Login
      uses: azure/login@v1
      with:
        creds: ${{ secrets.AZURE_CREDENTIALS }}
              
    - name: Azure CLI script
      uses: azure/CLI@v1
      with:
        azcliversion: 2.0.75
        inlineScript: |
          az extension add --name spring-cloud
          az configure --defaults group=<service group name>
          az configure --defaults spring-cloud=<service instance name>
          az spring-cloud app deploy -n gateway --jar-path $GITHUB_WORKSPACE/gateway/target/gateway.jar
          az spring-cloud app deploy -n account-service --jar-path $GITHUB_WORKSPACE/account-service/target/account-service.jar
          az spring-cloud app deploy -n auth-service --jar-path $GITHUB_WORKSPACE/auth-service/target/auth-service.jar
```

## <a name="deploy-with-maven-plugin"></a>Maven Plugin ile dağıtın
Başka bir seçenek, Kavanoz'u dağıtmak ve Uygulama ayarlarını güncellemek için [Maven](https://docs.microsoft.com/azure/spring-cloud/spring-cloud-quickstart-launch-app-maven) Eklentisi'ni kullanmaktır. Komut `mvn azure-spring-cloud:deploy` idempotent ve gerekirse otomatik olarak Apps oluşturur. İlgili uygulamaları önceden oluşturmanız gerekmez.

```
name: AzureSpringCloud
on: push

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
    
    - uses: actions/checkout@master
    
    - name: Set up JDK 1.8
      uses: actions/setup-java@v1
      with:
        java-version: 1.8
    
    - name: maven build, clean
      run: |
        mvn clean package -D skipTests
        
    # Maven plugin can cosume this authentication method automatically
    - name: Azure Login
      uses: azure/login@v1
      with:
        creds: ${{ secrets.AZURE_CREDENTIALS }}
    
    # Maven deploy, make sure you have correct configurations in your pom.xml
    - name: deploy to Azure Spring Cloud using Maven
      run: |
        mvn azure-spring-cloud:deploy
```

## <a name="run-the-workflow"></a>İş akışını çalıştırma
GitHub **Eylemleri,** GitHub'a `.github/workflow/main.yml` ittikten sonra otomatik olarak etkinleştirilmelidir. Yeni bir commit ittiğiniz zaman eylem tetiklenir. Bu dosyayı tarayıcıda oluşturursanız, eyleminiz zaten çalışır.

Eylemin etkinleştirildiğini doğrulamak için GitHub deposu **sayfasındaki Eylemler** sekmesini tıklatın:

 ![Eylemi etkinleştirme özelliğini doğrula](./media/github-actions/actions3.png)

Örneğin, eyleminiz hatalı çalışıyorsa, Örneğin Azure kimlik belgesini ayarlamadıysanız, hatayı düzelttikten sonra denetimleri yeniden çalıştırabilirsiniz. GitHub deposu **sayfasında, Eylemler'i**tıklatın, belirli iş akışı görevini seçin ve sonra denetimleri yeniden çalıştırmak için **Yeniden Çalıştır denetimleri** düğmesini tıklatın:

 ![Yeniden çalıştırma denetimleri](./media/github-actions/actions4.png)

## <a name="next-steps"></a>Sonraki adımlar
* [Spring Cloud GitHub eylemleri için Anahtar Vault](./spring-cloud-github-actions-key-vault.md)
* [Azure Active Directory hizmet ilkeleri](https://docs.microsoft.com/cli/azure/ad/sp?view=azure-cli-latest#az-ad-sp-create-for-rbac)
* [Azure için GitHub Eylemleri](https://github.com/Azure/actions/)
