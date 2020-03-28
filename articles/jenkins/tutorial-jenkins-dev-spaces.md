---
title: Azure Kubernetes Hizmeti ile Jenkins için Azure Dev Spaces Eklentisini Kullanma
description: Azure Geliştirme Alanları eklentisini sürekli bir tümleştirme ardışık alanında nasıl kullanacağınızı öğrenin.
ms.topic: tutorial
ms.date: 10/23/2019
ms.openlocfilehash: 281565cec5ee947781ab8ee9f62a00e01f9ababb
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79037032"
---
# <a name="tutorial-using-the-azure-dev-spaces-plug-in-for-jenkins-with-azure-kubernetes-service"></a>Öğretici: Azure Kubernetes Hizmeti ile Jenkins için Azure Dev Spaces Eklentisini Kullanma 

Azure Geliştirme Alanları, azure Kubernetes Hizmeti 'nde (AKS) çalışan mikro hizmet uygulamanızı, bağımlılıkları çoğaltmaveya alay etmeye gerek kalmadan test etmenizi ve yinelemeli olarak geliştirmenize olanak tanır. Jenkins için Azure Geliştirme Spaces eklentisi, sürekli tümleştirme ve teslimat (CI/CD) ardışık ardışık alanınızda Dev Spaces'i kullanmanıza yardımcı olur.

Bu öğretici de Azure Kapsayıcı Kayıt (ACR) kullanır. ACR görüntüleri depolar ve Bir ACR Görev Docker ve Helm eserler oluşturur. Yapı oluşturma için ACR ve ACR Görevi'ni kullanmak, Jenkins sunucunuza Docker gibi ek yazılımyükleme gereksinimini ortadan kaldırır. 

Bu öğreticide, şu görevleri tamamlayasınız:

> [!div class="checklist"]
> * Azure Dev Spaces etkin leştirilmiş AKS kümesi oluşturma
> * AKS'ye çok hizmetli bir uygulama dağıtma
> * Jenkins sunucunuzu hazırlayın
> * Kod değişikliklerini projeye dönüştürmeden önce önizlemesini yapmak için Azure Dev Spaces eklentisini Jenkins ardışık bir ardışık olarak kullanma

Bu öğretici, temel Azure hizmetleri, AKS, ACR, Azure Dev Spaces, Jenkins boru hatları ve [eklentileri](https://jenkins.io/doc/book/pipeline/) ve GitHub gibi ara bilgileri varsayar. Kubectl ve Helm gibi destekleyici araçlara temel aşinalık yararlıdır.

## <a name="prerequisites"></a>Ön koşullar

* Bir Azure hesabı. Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

* GitHub hesabı. GitHub hesabınız yoksa, başlamadan önce ücretsiz bir [hesap](https://github.com/) oluşturun.

* [Azure Dev Spaces](https://marketplace.visualstudio.com/items?itemName=azuredevspaces.azds) uzantısı yüklü Görsel Stüdyo [Kodu.](https://code.visualstudio.com/download)

* [Azure CLI yüklendi,](/cli/azure/install-azure-cli?view=azure-cli-latest)sürüm 2.0.43 veya üstü.

* Bir Jenkins ana sunucusu. Zaten bir Jenkins master'ınuz yoksa, bu [hızlı başlangıçtaki](https://docs.microsoft.com/azure/jenkins/install-jenkins-solution-template)adımları izleyerek [Jenkins'i](https://aka.ms/jenkins-on-azure) Azure'da dağıtın. 

* Jenkins sunucusu, bu öğreticide daha sonra açıklandığı gibi, hem Helm hem de kubectl yüklü ve Jenkins hesabına açık olmalıdır.

* VS Kodu, VS Kodu Terminali veya WSL ve Bash. 


## <a name="create-azure-resources"></a>Azure kaynakları oluşturma

Bu bölümde, Azure kaynakları oluşturursunuz:

* Bu öğretici için tüm Azure kaynaklarını içeren bir kaynak grubu.
* [Azure Kubernetes Hizmeti](https://docs.microsoft.com/azure/aks/) (AKS) kümesi.
* Bir [Azure kapsayıcı kayıt defteri](https://docs.microsoft.com/azure/container-registry/) (ACR), (ACR Görevlerini kullanarak) oluşturmak ve Docker görüntülerini depolamak için.

1. Bir kaynak grubu oluşturun.

    ```azurecli
    az group create --name MyResourceGroup --location westus2
    ```

2. Bir AKS kümesi oluşturun. [Dev Spaces'i destekleyen](../dev-spaces/about.md#supported-regions-and-configurations)bir bölgede AKS kümesini oluşturun.

    ```azurecli
    az aks create --resource-group MyResourceGroup --name MyAKS --location westus2 --kubernetes-version 1.11.9 --enable-addons http_application_routing --generate-ssh-keys --node-count 1 --node-vm-size Standard_D1_v2
    ```

3. AKS'yi Dev Spaces'i kullanacak şekilde yapılandırın.

    ```azurecli
    az aks use-dev-spaces --resource-group MyResourceGroup --name MyAKS
    ```
    Bu adım CLI uzantısı yükler. `azds`

4. Bir kapsayıcı kayıt defteri oluşturun.

    ```azurecli
    az acr create -n MyACR -g MyResourceGroup --sku Basic --admin-enabled true
    ```

## <a name="deploy-sample-apps-to-the-aks-cluster"></a>Örnek uygulamaları AKS kümesine dağıtma

Bu bölümde, bir dev alanı ayarlayın ve son bölümde oluşturduğunuz AKS kümesine bir örnek uygulama dağıtırsınız. Uygulama iki bölümden oluşur, *webfrontend* ve *mywebapi.* Her iki bileşen de bir dev alanında dağıtılır. Daha sonra bu öğretici, Jenkins CI boru hattı tetiklemek için mywebapi karşı bir çekme isteği göndereceğiz.

Azure Dev Spaces'i kullanma ve Azure Geliştirme Alanları ile çoklu hizmet geliştirme hakkında daha fazla bilgi için, [Java ile Azure Dev Spaces'e ve](https://docs.microsoft.com/azure/dev-spaces/get-started-java)Azure Dev Spaces ile Çoklu hizmet [geliştirmeye](https://docs.microsoft.com/azure/dev-spaces/multi-service-java)başlayın'a bakın. Bu öğreticiler burada yer almayan ek arka plan bilgileri sağlar.

1. Repo'yu GitHub'dan indirin. https://github.com/Azure/dev-spaces

2. VS `samples/java/getting-started/webfrontend` Kodu'nda klasörü açın. (Hata ayıklama varlıkları eklemek veya projeyi geri yüklemek için tüm varsayılan istemleri yoksayabilirsiniz.)

3. Aşağıdaki `/src/main/java/com/ms/sample/webfrontend/Application.java` gibi görünmesi için güncelleme:

    ```java
    package com.ms.sample.webfrontend;

    import java.io.*;
    import java.net.*;
    import org.springframework.boot.SpringApplication;
    import org.springframework.boot.autoconfigure.SpringBootApplication;
    import org.springframework.web.bind.annotation.*;

    @SpringBootApplication
    @RestController
    public class Application {
        public static void main(String[] args) {
            SpringApplication.run(Application.class, args);
        }

        @RequestMapping(value = "/greeting", produces = "text/plain")
        public String greeting(@RequestHeader(value = "azds-route-as", required = false) String azdsRouteAs) throws Exception {
            URLConnection conn = new URL("http://mywebapi/").openConnection();
            conn.setRequestProperty("azds-route-as", azdsRouteAs); // propagate dev space routing header
            try (BufferedReader reader = new BufferedReader(new InputStreamReader(conn.getInputStream())))
            {
                return "Hello from webfrontend and " + reader.lines().reduce("\n", String::concat);
            }
        }
    }
    ```

4. VS Kodu'nda Entegre Terminal'i açmak için Önce **Görüntüle'yi** tıklatın. **View**

5. Bir `azds prep` dev alanda çalıştırmak için uygulama hazırlamak için komutu çalıştırın. Uygulamanızı doğru hazırlamak `dev-spaces/samples/java/getting-started/webfrontend` için bu komutun çalıştırılması gerekir:

    ```bash
    azds prep --public
    ```

    Dev Spaces CLI'nin `azds prep` komutu, varsayılan ayarlarla Docker ve Kubernetes varlıklarını oluşturur. Bu dosyalar projenin ömrü boyunca devam ediyor ve özelleştirilebilirler:

    * `./Dockerfile`ve `./Dockerfile.develop` uygulamanın kapsayıcı görüntüsünü ve kaynak kodunun kapsayıcı içinde nasıl oluşturulup çalıştığını açıklayın.
    * `./charts/webfrontend` altındaki [Helm grafiği](https://helm.sh/docs/topics/charts/), kapsayıcının Kubernetes'de nasıl dağıtıldığını açıklar.
    * `./azds.yaml`Azure Dev Spaces yapılandırma dosyasıdır.

    Daha fazla bilgi için [Azure Geliştirme Alanları'nın nasıl çalıştığını ve nasıl yapılandırıldığına](https://docs.microsoft.com/azure/dev-spaces/how-dev-spaces-works)bakın.

6. Uygulamayı AKS'de komutu `azds up` kullanarak oluşturun ve çalıştırın:

    ```bash
    azds up
    ```
    <a name="test_endpoint"></a>Komut tarafından oluşturulan URL hakkında bilgi için konsol `up` çıktısını tarayın. Şu biçimde olacaktır:

    ```bash
    (pending registration) Service 'webfrontend' port 'http' will be available at '<url>'
    Service 'webfrontend' port 80 (TCP) is available at 'http://localhost:<port>'
    ```
    Bu URL'yi bir tarayıcı penceresinde açın ve web uygulamasını görmeniz gerekir. Kapsayıcı yürütülürken, terminal penceresine `stdout` ve `stderr` çıkışının akışı yapılır.

8. Sonra, kurmak ve *mywebapi*dağıtmak:

    1. Dizini değiştirin`dev-spaces/samples/java/getting-started/mywebapi`

    2. Çalıştırın

        ```bash
        azds prep
        ```

    3. Çalıştırın

        ```bash
        azds up -d
        ```

## <a name="prepare-jenkins-server"></a>Jenkins sunucusuhazırlayın

Bu bölümde, örnek CI ardışık çalışmasını sağlamak için Jenkins sunucusunu hazırlayın.

* Eklentileri yükleme
* Helm ve Kubernetes CLI yükleyin
* Kimlik bilgileri ekleme

### <a name="install-plug-ins"></a>Eklentileri yükleme

1. Jenkins sunucunuzda oturum açın. **Jenkins'i yönet > Eklentileri Yönet'i**seçin.
2. **Kullanılabilir** sekmesinde aşağıdaki eklentileri seçin:
    * [Azure Dev Spaces](https://plugins.jenkins.io/azure-dev-spaces)
    * [Azure Container Registry Görevleri](https://plugins.jenkins.io/azure-container-registry-tasks)
    * [Çevre Enjektörü](https://plugins.jenkins.io/envinject)
    * [GitHub Entegrasyonu](https://plugins.jenkins.io/github-pullrequest)

    Bu eklentiler listede görünmüyorsa, yüklü olup olmadıklarını görmek için **Yüklü** sekmesini kontrol edin.

3. Eklentileri yüklemek için şimdi **İndir'i seçin ve yeniden başlattıktan sonra yükleyin.**

4. Yüklemeyi tamamlamak için Jenkins sunucunuzu yeniden başlatın.

### <a name="install-helm-and-kubectl"></a>Helm ve kubectl yükleyin

Örnek boru hattı, dev alana dağıtmak için Helm ve kubectl kullanır. Jenkins yüklendiğinde, *jenkins*adında bir yönetici hesabı oluşturur. Hem Helm hem de kubectl jenkins kullanıcı için erişilebilir olması gerekir.

1. Jenkins ustasıyla SSH bağlantısı kurun. 

2. Kullanıcıya `jenkins` geçin:
    ```bash
    sudo su jenkins
    ```

3. Miğfer CLI'yi yükleyin. Daha fazla bilgi için, [Yükleme Miğferi'ne](https://helm.sh/docs/using_helm/#installing-helm)bakın.

4. kubectl yükleyin. Daha fazla bilgi için az [**acs kubernetes install-cli'ye**](https://helm.sh/docs/using_helm/#installing-helm)bakın.

### <a name="add-credentials-to-jenkins"></a>Jenkins'e kimlik bilgileri ekleme

1. Jenkins'in Azure kaynaklarının kimliğini doğrulamak ve erişmek için bir Azure hizmet ilkesine ihtiyacı vardır. Hizmet ilkesini oluşturmak için, Azure Uygulaması Hizmeti öğreticisine [Dağıt'taki hizmet ana](https://docs.microsoft.com/azure/jenkins/tutorial-jenkins-deploy-web-app-azure-app-service#create-service-principal) ilkesini oluştur bölümüne bakın. Bir sonraki adımı tamamlamak için `create-for-rbac` bu bilgilere ihtiyacınız olduğundan çıktının bir kopyasını kaydettiğinizden emin olun. Çıkış şuna benzer olacaktır:

    ```json
    {
      "appId": "f4150da1-xxx-xxxx-xxx-xxxxxxxxxxxx",
      "displayName": "xxxxxxxjenkinssp",
      "name": "http://xxxxxxxjenkinssp",
      "password": "f6e4d839-xxx-xxxx-xxx-xxxxxxxxxxxx",
      "tenant": "72f988bf--xxx-xxxx-xxx-xxxxxxxxxxxx"
    }
    ```

2. Önceki adımdaki hizmet temel bilgilerini kullanarak Jenkins'te bir *Microsoft Azure Hizmet Müdürü* kimlik bilgisi türü ekleyin. Aşağıdaki ekran görüntüsündeki adlar `create-for-rbac`.

    **Kimlik** alanı, hizmet müdürünüz için Jenkins kimlik bilgisidir. Örnek, `displayName` (bu örnekte) değerini `xxxxxxxjenkinssp`kullanır, ancak istediğiniz metni kullanabilirsiniz. Bu kimlik bilgisi adı, bir sonraki bölümdeki AZURE_CRED_ID ortam değişkeninin değeridir.

    ![Jenkins'e hizmet temel kimlik bilgileri ekleme](media/tutorial-jenkins-dev-spaces/add-service-principal-credentials.png)

    **Açıklama** isteğe bağlıdır. Daha ayrıntılı talimatlar için, Azure Uygulama Hizmetine Dağıt uygulamasında [Jenkins'e](https://docs.microsoft.com/azure/jenkins/tutorial-jenkins-deploy-web-app-azure-app-service#add-service-principal-to-jenkins) hizmet sorumlusu ekle bölümüne bakın. 



3. ACR kimlik bilgilerinizi göstermek için şu komutu çalıştırın:

    ```azurecli
    az acr credential show -n <yourRegistryName>
    ```

    JSON çıktısının bir kopyasını yapın, bu da şuna benzer:

    ```json
    {
      "passwords": [
        {
          "name": "password",
          "value": "vGBP=zzzzzzzzzzzzzzzzzzzzzzzzzzz"
        },
        {
          "name": "password2",
          "value": "zzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzz"
        }
      ],
      "username": "acr01"
    }
    ```

4. Jenkins şifre kimlik bilgisi türü *ne bir Kullanıcı adı* ekleyin. **Kullanıcı adı,** bu örnekte `acr01`son adımdaki kullanıcı adıdır. **Parola,** bu örnekte `vGBP=zzzzzzzzzzzzzzzzzzzzzzzzzzz`ilk parolanın değeridir. Bu kimlik **kimliğinin** ACR_CRED_ID değeridir.

5. AKS kimlik bilgileri ayarlayın. Jenkins'e *Kubernetes yapılandırması (kubeconfig)* kimlik bilgisi türü ekleyin ("Doğrudan girin" seçeneğini kullanın). AKS kümenizin erişim kimlik bilgilerini almak için aşağıdaki komutu çalıştırın:

    ```azurecli
    az aks get-credentials -g MyResourceGroup -n <yourAKSName> -f -
    ```

   Bu kimlik **numarası,** bir sonraki bölümdeki KUBE_CONFIG_ID değeridir.

## <a name="create-a-pipeline"></a>İşlem hattı oluşturma

Örnek ardışık düzen için seçilen senaryo gerçek bir modele dayanır: Çekme isteği, önerilen değişiklikleri test etmek ve gözden geçirmek için Bir Azure geliştirme alanına dağıtır ve oluşturan bir CI ardışık hattını tetikler. İncelemenin sonucuna bağlı olarak, değişiklikler birleştirilir ve AKS'ye dağıtılır veya atılır. Son olarak, dev alanı kaldırılır.

Jenkins boru hattı yapılandırması ve Jenkinsfile CI ardışık alan daki aşamaları tanımlar. Bu akış şeması, Jenkinsfile tarafından tanımlanan boru hattı aşamalarını ve karar noktalarını gösterir:

![Jenkins boru hattı akışı](media/tutorial-jenkins-dev-spaces/jenkins-pipeline-flow.png)

1. *Mywebapi* projesinin değiştirilmiş bir [https://github.com/azure-devops/mywebapi](https://github.com/azure-devops/mywebapi)sürümünü indirin. Bu proje *Jenkinsfile,* *Dockerfiles*ve Helm grafik de dahil olmak üzere bir boru hattı oluşturmak için gerekli çeşitli dosyaları içerir.

2. Jenkins'e giriş yap. Soldaki menüden Öğe **Ekle'yi**seçin.

3. **Pipeline'ı**seçin ve ardından **madde adı kutusuna** bir ad girin. **Tamam'ı**seçin ve ardışık ardışık yapılandırma ekranı otomatik olarak açılır.

4. **Genel** sekmesinde, **çalışma için ortam hazırlamayı**denetleyin. 

5. **Jenkins Çevre Değişkenleri tutun** ve Jenkins Yapı **Değişkenleri tutun**.

6. Özellikler **İçeriği** kutusuna aşağıdaki ortam değişkenlerini girin:

    ```
    AZURE_CRED_ID=[your credential ID of service principal]
    RES_GROUP=[your resource group of the function app]
    ACR_RES_GROUP=[your ACR resource group]
    ACR_NAME=[your ACR name]
    ACR_REGISTRY=[your ACR registry url, without http schema]
    ACR_CRED_ID=[your credential id of your ACR account]
    AKS_RES_GROUP=[your AKS resource group]
    AKS_NAME=[your AKS name]
    IMAGE_NAME=[name of Docker image you will push to ACR, without registry prefix]
    KUBE_CONFIG_ID=[your kubeconfig id]
    PARENT_DEV_SPACE=[shared dev space name]
    TEST_ENDPOINT=[your web frontend end point for testing. Should be webfrontend.XXXXXXXXXXXXXXXXXXX.xxxxxx.aksapp.io]
    ```

    Önceki bölümlerde verilen örnek değerleri kullanarak, ortam değişkenleri listesi aşağıdaki gibi görünmelidir:

    ![Jenkins boru hattı çevre değişkenleri](media/tutorial-jenkins-dev-spaces/jenkins-pipeline-environment.png)

7. **Pipeline > Definition'da** **SCM'den Pipeline komut dosyası** seçin.
8. **SCM'de** **Git'i** seçin ve repo URL'nizi girin.
9. **Şube Belirtimi'** nde `refs/remotes/origin/${GITHUB_PR_SOURCE_BRANCH}`, girin.
10. SCM repo URL'sini ve komut dosyası yolunu "Jenkinsfile" doldurun.
11. **Hafif ödeme** kontrol edilmelidir.

## <a name="create-a-pull-request-to-trigger-the-pipeline"></a>Ardışık mayı tetiklemek için çekme isteği oluşturma

Bu bölümde adım 3 tamamlamak için, Jenkinsfile bir parçası yorum gerekir, aksi takdirde yan yana yeni ve eski sürümleri görüntülemek için çalıştığınızda bir 404 hata alırsınız. Varsayılan olarak, PR'ı birleştirmeyi seçtiğinizde, mywebapi'nin önceki paylaşılan sürümü kaldırılır ve yeni sürümle değiştirilir. Adım 1'i tamamlamadan önce Jenkinsdosyasında aşağıdaki değişikliği yapın:

```Groovy
    if (userInput == true) {
        stage('deploy') {
            // Apply the deployment to shared namespace in AKS using acsDeploy, Helm or kubectl   
        }
        
        stage('Verify') {
            // verify the staging environment is working properly
        }
        
        /* Comment the cleanup stage to allow side by side comparison with the new child dev space

        stage('cleanup') {
            devSpacesCleanup aksName: env.AKS_NAME, 
                azureCredentialsId: env.AZURE_CRED_ID, 
                devSpaceName: devSpaceNamespace, 
                kubeConfigId: env.KUBE_CONFIG_ID, 
                resourceGroupName: env.AKS_RES_GROUP,
                helmReleaseName: releaseName
        }
        */

    } else {
        // Send a notification
    }
```

1. Bir değişiklik `mywebapi/src/main/java/com/ms/sample/mywebapi/Application.java`yapın ve ardından çekme isteği oluşturun. Örnek:

    ```java
    public String index() {
        return "Hello from mywebapi in my private dev space";
    }
    ```

2. Jenkins'te oturum açın ve boru hattı adını seçin ve ardından **Şimdi Oluştur'u**seçin. 

    Jenkins boru hattını otomatik olarak tetiklemek için bir *webhook* da ayarlayabilirsiniz. Bir çekme isteği girildiğinde, GitHub Jenkins'e bir POST vererek boru hattını tetikler. Web hook kurma hakkında daha fazla bilgi için [Jenkins'i GitHub'a bağlayın' a](https://github.com/MicrosoftDocs/azure-docs/blob/master/articles/jenkins/tutorial-jenkins-deploy-web-app-azure-app-service.md#connect-jenkins-to-github)bakın.

3. Değişiklikleri geçerli paylaşılan sürümle karşılaştırın:

    1. Tarayıcınızı açın ve paylaşılan `https://webfrontend.XXXXXXXXXXXXXXXXXXX.eastus.aksapp.io`sürüme gidin. TEST_ENDPOINT URL içerir.

    2. Başka bir sekme açın ve ardından PR dev alanı URL'sini girin. Bu benzer olacaktır `https://<yourdevspacename>.s.webfrontend.XXXXXXXXXXXXXXXXXXX.eastus.aksapp.io`. Jenkins işi için **Yapı Geçmişi > <#> > Konsol Çıkışı'nda** bağlantıyı bulacaksınız. Sayfayı arayın `aksapp`, veya yalnızca önek görmek `azdsprefix`için, arama .

 

### <a name="constructing-the-url-to-the-child-dev-space"></a>URL'yi çocuk geliştirme alanına oluşturma

Bir çekme isteği dosyaladığınızda, Jenkins takımın paylaşılan geliştirme alanına göre bir alt geliştirme alanı oluşturur ve kodu o çocuk geliştirme alanında çekme isteğinizden çalıştırR. Alt dev alanının URL'si form `http://$env.azdsprefix.<test_endpoint>`alır. 

**$env.azdsprefix** **devSpacesCreate**tarafından Azure Dev Spaces eklentisi tarafından boru hattı yürütme sırasında ayarlanır:

```Groovy
stage('create dev space') {
    devSpacesCreate aksName: env.AKS_NAME,
        azureCredentialsId: env.AZURE_CRED_ID,
        kubeconfigId: env.KUBE_CONFIG_ID,
        resourceGroupName: env.AKS_RES_GROUP,
        sharedSpaceName: env.PARENT_DEV_SPACE,
        spaceName: devSpaceNamespace
}
```

Bu `test_endpoint` URL, daha önce `azds up` [aks kümesine örnek uygulamaları dağıt'ta](#test_endpoint)kullanarak dağıttığınız webfrontend uygulamasının URL'sidir, Adım 7. Değeri, `$env.TEST_ENDPOINT` boru hattı yapılandırmasında ayarlanır. 

Aşağıdaki kod `smoketest` snippet, alt dev alanı URL'sinin sahnede nasıl kullanıldığını gösterir. Kod, çocuk geliştirme alanı TEST_ENDPOINT kullanılabilir olup olmadığını denetlemek için denetler ve varsa, karşılama metnini stdout'a indirir:

```Groovy
stage('smoketest') {
    // CI testing against http://$env.azdsprefix.$env.TEST_ENDPOINT" 
    SLEEP_TIME = 30
    SITE_UP = false
    for (int i = 0; i < 10; i++) {
        sh "sleep ${SLEEP_TIME}"
        code = "0"
        try {
            code = sh returnStdout: true, script: "curl -sL -w '%{http_code}' 'http://$env.azdsprefix.$env.TEST_ENDPOINT/greeting' -o /dev/null"
        } catch (Exception e){
            // ignore
        }
        if (code == "200") {
            sh "curl http://$env.azdsprefix.$env.TEST_ENDPOINT/greeting"
            SITE_UP = true
            break
        }
    }
    if(!SITE_UP) {
        echo "The site has not been up after five minutes"
    }
}
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Örnek uygulamayı kullanmayı bitirdiğinizde, kaynak grubunu silerek Azure kaynaklarını temizleyin:

```azurecli
az group delete -y --no-wait -n MyResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure'da Jenkins ile CI/CD](jenkins-continuous-deployment.md)
