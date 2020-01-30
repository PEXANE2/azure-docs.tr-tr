---
title: GitHub eylemleri ile Azure yay bulut CI/CD
description: GitHub eylemleriyle Azure Spring bulutu için CI/CD iş akışını oluşturma
author: MikeDodaro
ms.author: barbkess
ms.service: spring-cloud
ms.topic: how-to
ms.date: 01/15/2019
ms.openlocfilehash: 303f24ef6d934c0382bd8917833e3ec545f2a540
ms.sourcegitcommit: 984c5b53851be35c7c3148dcd4dfd2a93cebe49f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/28/2020
ms.locfileid: "76776488"
---
# <a name="azure-spring-cloud-cicd-with-github-actions"></a>GitHub eylemleri ile Azure yay bulut CI/CD

GitHub eylemleri otomatik yazılım geliştirme yaşam döngüsü iş akışını destekler. Azure Spring Cloud için GitHub eylemleri sayesinde deponuzda oluşturmak, test etmek, paketlemek, yayınlamak ve Azure 'a dağıtmak için iş akışları oluşturabilirsiniz. 

## <a name="prerequisites"></a>Ön koşullar
Bu örnek, [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)gerektirir.

## <a name="set-up-github-repository-and-authenticate"></a>GitHub deposunu ayarlama ve kimlik doğrulama
Azure oturum açma eylemini yetkilendirmek için bir Azure hizmet asıl kimlik bilgisine ihtiyacınız vardır. Azure kimlik bilgilerini almak için yerel makinenizde aşağıdaki komutları yürütün:
```
az login
az ad sp create-for-rbac --role contributor --scopes /subscriptions/<SUBSCRIPTION_ID> --sdk-auth 
```
Belirli bir kaynak grubuna erişmek için kapsamı azaltabilirsiniz:
```
az ad sp create-for-rbac --role contributor --scopes /subscriptions/<SUBSCRIPTION_ID>/resourceGroups/<RESOURCE_GROUP> --sdk-auth
```
Komut bir JSON nesnesinin çıktısını almalıdır:
```JSON
{
    "clientId": "<GUID>",
    "clientSecret": "<GUID>",
    "subscriptionId": "<GUID>",
    "tenantId": "<GUID>",
    ...
}
```

Bu örnek GitHub 'da [Piggy ölçümler](https://github.com/Azure-Samples/piggymetrics) örneğini kullanır.  Örnek, GitHub deposu sayfasını açın ve **Ayarlar** sekmesi ' ne tıklayın. **gizlilikler** menüsünü açın ve **Yeni bir parola Ekle**' ye tıklayın:

 ![Yeni gizli dizi Ekle](./media/github-actions/actions1.png)

Gizli adını `AZURE_CREDENTIALS` ve değerini, *GitHub deponuzu ayarlama ve kimlik doğrulama*altında bulduğunuz JSON dizesine ayarlayın.

 ![Gizli verileri ayarlama](./media/github-actions/actions2.png)

GitHub [eylemlerinde Key Vault Ile Azure Spring kimlik doğrulama konusunda](./spring-cloud-github-actions-key-vault.md)açıklandığı gibi, Azure oturum açma kimlik bilgilerini GitHub eylemleriyle Key Vault de alabilirsiniz.

## <a name="provision-service-instance"></a>Hizmet örneği sağlama
Azure Spring Cloud Service örneğinizi sağlamak için Azure CLı 'yı kullanarak aşağıdaki komutları çalıştırın.
```
az extension add --name spring-cloud
az group create --location eastus --name <resource group name>
az spring-cloud create -n <service instance name> -g <resource group name>
az spring-cloud config-server git set -n <service instance name> --uri https://github.com/xxx/piggymetrics --label config
```
## <a name="build-the-workflow"></a>İş akışını oluşturma
İş akışı aşağıdaki seçenekler kullanılarak tanımlanır.

### <a name="prepare-for-deployment-with-azure-cli"></a>Azure CLı ile dağıtıma hazırlanma
Komut `az spring-cloud app create` Şu anda ıdempotent değil.  Bu iş akışını mevcut Azure Spring Cloud Apps ve örnekleri üzerinde öneririz.

Hazırlama için aşağıdaki Azure CLı komutlarını kullanın:
```
az configure --defaults group=<service group name>
az configure --defaults spring-cloud=<service instance name>
az spring-cloud app create --name gateway
az spring-cloud app create --name auth-service
az spring-cloud app create --name account-service
```

### <a name="deploy-with-azure-cli-directly"></a>Doğrudan Azure CLı ile dağıtma
Depoda `.github/workflow/main.yml` dosyasını oluşturun:

```
name: AzureSpringCloud

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
### <a name="deploy-with-azure-cli-action"></a>Azure CLı eylemiyle Dağıtma eylemi
Az `run` komutu, en son Azure CLı sürümünü kullanacaktır. Büyük değişiklikler varsa Azure CLı 'nin belirli bir sürümünü Azure/CLı `action`de kullanabilirsiniz. 

> [!Note] 
> Bu komut yeni bir kapsayıcıda çalışacak, bu nedenle `env` çalışmayacak ve çapraz işlem dosya erişimi ek kısıtlamalara sahip olabilir.

Depoda. GitHub/Workflow/Main. yml dosyasını oluşturun:
```
name: AzureSpringCloud

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

## <a name="deploy-with-maven-plugin"></a>Maven eklentisi ile dağıtma
Diğer bir seçenek de Jar 'yi dağıtmak ve uygulama ayarlarını güncelleştirmek için [Maven eklentisini](https://docs.microsoft.com/azure/spring-cloud/spring-cloud-quickstart-launch-app-maven) kullanmaktır. Komut `mvn azure-spring-cloud:deploy` ıdempotent ve gerekirse otomatik olarak uygulamalar oluşturur. İlgili uygulamaları önceden oluşturmanız gerekmez.

```
name: AzureSpringCloud

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
GitHub 'a `.github/workflow/main.yml` itilduktan sonra GitHub **eylemleri** otomatik olarak etkinleştirilmelidir. Yeni bir işleme gönderdiğinizde bu eylem tetiklenir. Bu dosyayı tarayıcıda oluşturursanız, eyleminiz zaten çalıştırılmış olmalıdır.

Eylemin etkinleştirildiğini doğrulamak için GitHub Deposu sayfasında **Eylemler** sekmesi ' ne tıklayın:

 ![Etkin doğrulama eylemi](./media/github-actions/actions3.png)

Eylemleriniz hatada çalışıyorsa, örneğin, Azure kimlik bilgisini ayarlamadıysanız, hatayı düzelttikten sonra denetimleri yeniden çalıştırabilirsiniz. GitHub Deposu sayfasında, **Eylemler**' e tıklayın, belirli bir iş akışı görevini seçin ve **sonra denetimleri yeniden çalıştır düğmesine tıklayarak** denetimleri yeniden çalıştırın:

 ![Denetimleri yeniden çalıştır](./media/github-actions/actions4.png)

## <a name="next-steps"></a>Sonraki adımlar
* [Spring Cloud GitHub eylemleri için Key Vault](./spring-cloud-github-actions-key-vault.md)
* [Hizmet sorumlularını Azure Active Directory](https://docs.microsoft.com/cli/azure/ad/sp?view=azure-cli-latest#az-ad-sp-create-for-rbac)
* [Azure için GitHub eylemleri](https://github.com/Azure/actions/)
