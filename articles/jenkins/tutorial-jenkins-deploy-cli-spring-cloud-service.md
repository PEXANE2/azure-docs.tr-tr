---
title: Jenkins ve Azure CLI kullanarak uygulamaları Azure Spring Cloud’a dağıtma
description: Azure Yay Bulutu hizmetine mikro hizmetleri dağıtmak için Azure CLI'yi sürekli tümleştirme ve dağıtım boru hattında nasıl kullanacağınızı öğrenin
ms.topic: tutorial
ms.date: 01/07/2020
ms.openlocfilehash: bc48eecc9e73b95fd9112d645135409c24369e10
ms.sourcegitcommit: 98e79b359c4c6df2d8f9a47e0dbe93f3158be629
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "80810210"
---
# <a name="tutorial-deploy-apps-to-azure-spring-cloud-using-jenkins-and-the-azure-cli"></a>Öğretici: Jenkins ve Azure CLI'yi kullanarak uygulamaları Azure İlkbahar Bulutu'na dağıtın

[Azure Yay Bulutu,](https://docs.microsoft.com/azure/spring-cloud/spring-cloud-overview) yerleşik hizmet bulma ve yapılandırma yönetimi yle tamamen yönetilen bir mikro hizmet geliştirmedir. Bu hizmet, Bahar Önyükleme tabanlı mikro hizmet uygulamalarını Azure'a dağıtmayı kolaylaştırır. Bu öğretici, Azure Bahar Bulutu için sürekli tümleştirme ve teslimi (CI/CD) otomatikleştirmek için Jenkins'teki Azure CLI'yi nasıl kullanabileceğinizi gösterir.

Bu öğreticide, şu görevleri tamamlayasınız:

> [!div class="checklist"]
> * Bir hizmet örneği sağlama ve Java Spring uygulaması başlatma
> * Jenkins sunucunuzu hazırlayın
> * Mikro hizmet uygulamalarını oluşturmak ve dağıtmak için Jenkins ardışık bir boru hattında Azure CLI'yi kullanın 

Bu öğretici, temel Azure hizmetleri, Azure Bahar Bulutu, Jenkins boru hatları ve [eklentileri](https://jenkins.io/doc/book/pipeline/) ve GitHub hakkında ara bilgileri varsayar.

## <a name="prerequisites"></a>Ön koşullar

>[!Note]
> Azure İlkbahar Bulutu şu anda genel önizleme olarak sunulmaktadır. Genel önizleme teklifleri, müşterilerin resmi yayınlanmalarından önce yeni özelliklerle denemeler e-sayılsa.  Genel önizleme özellikleri ve hizmetleri üretim kullanımı için değildir.  Önizlemeler sırasında destek hakkında daha fazla bilgi için lütfen [SSS'mizi](https://azure.microsoft.com/support/faq/) gözden geçirin veya daha fazla bilgi edinmek için bir [Destek isteği](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request) dosyalayın.

- **Azure aboneliği**: Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) oluşturun.

* GitHub hesabı. GitHub hesabınız yoksa, başlamadan önce ücretsiz bir [hesap](https://github.com/) oluşturun.

* Bir Jenkins ana sunucusu. Zaten bir Jenkins master'ınuz yoksa, bu [hızlı başlangıçtaki](https://docs.microsoft.com/azure/jenkins/install-jenkins-solution-template)adımları izleyerek [Jenkins'i](https://aka.ms/jenkins-on-azure) Azure'da dağıtın. Jenkins düğümü/aracısı için aşağıdakiler gereklidir (örneğin. build server):

    * [Git](https://git-scm.com/)
    * [JDK 8](https://docs.microsoft.com/java/azure/jdk/?view=azure-java-stable)
    * [Maven 3.0 veya üzeri](https://maven.apache.org/download.cgi)
    * [Azure CLI yüklendi,](/cli/azure/install-azure-cli?view=azure-cli-latest)sürüm 2.0.67 veya üstü

    >[!TIP]
    > Git, JDK, Az CLI ve Azure eklentileri gibi araçlar varsayılan olarak Azure Marketi [Microsoft Jenkins](https://aka.ms/jenkins-on-azure) çözüm şablonuna dahildir.
    
* [Azure aboneliği için kaydolun](https://azure.microsoft.com/free/)
 
## <a name="provision-a-service-instance-and-launch-a-java-spring-application"></a>Bir hizmet örneği sağlama ve Java Spring uygulaması başlatma

[Piggy Metrics'i](https://github.com/Azure-Samples/piggymetrics) örnek Microsoft hizmet uygulaması olarak kullanıyoruz ve [Quickstart: Azure CLI'yi kullanarak](https://docs.microsoft.com/azure/spring-cloud/spring-cloud-quickstart-launch-app-cli) hizmet örneğini sağlamak ve uygulamaları ayarlamak için aynı adımları uyguluyoruz. Zaten aynı işlemden geçtiyseniz, bir sonraki bölüme atlayabilirsiniz. Aksi takdirde, aşağıdakiazure CLI komutları dahildir. Hızlı Başlangıç: Ek arka plan bilgileri almak için [Azure CLI'yi kullanarak bir Java Spring uygulaması başlatın.](https://docs.microsoft.com/azure/spring-cloud/spring-cloud-quickstart-launch-app-cli)

Yerel makinenizin Jenkins build sunucusuyla aynı ön koşulu karşılaması gerekir. Mikro hizmet uygulamalarını oluşturmak ve dağıtmak için aşağıdakilerin yüklü olduğundan emin olun:
    * [Git](https://git-scm.com/)
    * [JDK 8](https://docs.microsoft.com/java/azure/jdk/?view=azure-java-stable)
    * [Maven 3.0 veya üzeri](https://maven.apache.org/download.cgi)
    * [Azure CLI yüklendi,](/cli/azure/install-azure-cli?view=azure-cli-latest)sürüm 2.0.67 veya üstü

1. Azure İlkbahar Bulutu uzantısını yükleyin:

    ```Azure CLI
        az extension add --name spring-cloud
    ```

2. Azure İlkbahar Bulut hizmetinizi içerecek bir kaynak grubu oluşturun:

    ```Azure CLI
        az group create --location eastus --name <resource group name>
    ```

3. Azure Yay Bulutu örneğini sağlama:

    ```Azure CLI
        az spring-cloud create -n <service name> -g <resource group name>
    ```

4. Kendi GitHub hesabınıza [Piggy Metrics](https://github.com/Azure-Samples/piggymetrics) repo fork. Yerel makinenizde, reponuzu şu adlı `source-code`bir dizinde klonla:

    ```bash
        mkdir source-code
        git clone https://github.com/<your GitHub id>/piggymetrics
    ```

5. Yapılandırma sunucunuzu ayarlayın. GitHub kimliğinizi &lt;&gt; doğru değerle değiştirdiğinizden emin olun.

    ```Azure CLI
        az spring-cloud config-server git set -n <your-service-name> --uri https://github.com/<your GitHub id>/piggymetrics --label config
    ```

6. Projeyi oluşturun:

    ```bash
        cd piggymetrics
        mvn clean package -D skipTests
    ```

7. Üç mikro hizmet oluşturun: **ağ geçidi,** **auth-service**ve **hesap hizmeti:**

    ```Azure CLI
        az spring-cloud app create --n gateway -s <service name> -g <resource group name>
        az spring-cloud app create --n auth-service -s <service name> -g <resource group name>
        az spring-cloud app create --n account-service -s <service name> -g <resource group name>
    ```

8. Uygulamaları dağıtın: 

    ```Azure CLI
        az spring-cloud app deploy -n gateway -s <service name> -g <resource group name> --jar-path ./gateway/target/gateway.jar
        az spring-cloud app deploy -n account-service -s <service name> -g <resource group name> --jar-path ./account-service/target/account-service.jar
        az spring-cloud app deploy -n auth-service -s <service name> -g <resource group name> --jar-path ./auth-service/target/auth-service.jar
    ```

9. Ağ geçidine genel bitiş noktası atayın:

    ```Azure CLI
        az spring-cloud app update -n gateway -s <service name> -g <resource group name> --is-public true
    ```

10. Url'yi almak için ağ geçidi uygulamasını sorgulayın, böylece uygulamanın çalıştığını doğrulayabilirsiniz.

    ```Azure CLI
    az spring-cloud app show --name gateway | grep url
    ```
    
    PiggyMetrics uygulamasını çalıştırmak için önceki komutun sağladığı URL'ye gidin. 

## <a name="prepare-jenkins-server"></a>Jenkins sunucusuhazırlayın

Bu bölümde, Jenkins sunucusunu bir yapıyı çalıştırmak için hazırlarsınız, bu da test etmek için iyidir. Ancak, güvenlik açısından iması nedeniyle, yapılarınızı çalıştırmak için Azure'da bir aracıyı döndürmek için bir [Azure VM aracısı](https://plugins.jenkins.io/azure-vm-agents) veya [Azure Kapsayıcı aracısı](https://plugins.jenkins.io/azure-container-agents) kullanmanız gerekir. Daha fazla bilgi için [ana sunucuda derleme çalıştırmanın güvenlik sonuçları](https://wiki.jenkins.io/display/JENKINS/Security+implication+of+building+on+master) konulu Jenkins makalesine bakın.

### <a name="install-plug-ins"></a>Eklentileri yükleme

1. Jenkins sunucunuzda oturum açın. **Jenkins'i yönet > Eklentileri Yönet'i**seçin.
2. **Kullanılabilir** sekmesinde aşağıdaki eklentileri seçin:
    * [GitHub Entegrasyonu](https://plugins.jenkins.io/github-pullrequest)
    * [Azure Kimlik Bilgileri](https://plugins.jenkins.io/azure-credentials)

    Bu eklentiler listede görünmüyorsa, yüklü olup olmadıklarını görmek için **Yüklü** sekmesini kontrol edin.

3. Eklentileri yüklemek için şimdi **İndir'i seçin ve yeniden başlattıktan sonra yükleyin.**

4. Yüklemeyi tamamlamak için Jenkins sunucunuzu yeniden başlatın.

### <a name="add-your-azure-service-principal-credential-in-jenkins-credential-store"></a>Azure Hizmet Sorumlusu kimlik bilgilerinizi Jenkins kimlik bilgisi deposuna ekleyin

1. Azure'a dağıtmak için bir Azure Hizmet Sorumlusuna ihtiyacınız var. Daha fazla bilgi için Azure Uygulama Hizmetine Dağıt öğreticisinde [hizmet ana](https://docs.microsoft.com/azure/jenkins/tutorial-jenkins-deploy-web-app-azure-app-service#create-service-principal) sını oluştur bölümüne bakın. Çıktı `az ad sp create-for-rbac` şuna benzer:

    ```
    {
        "appId": "xxxxxx-xxx-xxxx-xxx-xxxxxxxxxxxx",
        "displayName": "xxxxxxxjenkinssp",
        "name": "http://xxxxxxxjenkinssp",
        "password": "xxxxxx-xxx-xxxx-xxx-xxxxxxxxxxxx",
        "tenant": "xxxxxx--xxx-xxxx-xxx-xxxxxxxxxxxx"
    }
    ```

2. Jenkins panosunda **Kimlik Bilgileri** > **Sistemi'ni**seçin. Ardından, **Global credentials(unrestricted)** (Genel kimlik bilgileri (sınırsız)) seçeneğini belirleyin.

3. **Kimlik Bilgileri Ekle'yi**seçin. 

4. Tür olarak **Microsoft Azure Hizmet Sorumlusu'nun** seçeneğini belirleyin.

5. Kaynak değerleri: * Abonelik Kimliği: Azure abonelik kimliğinizi `appId` kullanın * `password` İstemci Kimliği: kullanım * İstemci Sırrı: kullanım * Kiracı Kimliği: kullanım `tenant` * Azure Ortamı: önceden ayarlanmış bir değer seçin. Örneğin, Azure Global * ID için **Azure'u** kullanın: **azure_service_principal**olarak ayarlayın. Bu kimliği bu makalenin ilerleyen adımlarında kullanırız * Açıklama: isteğe bağlı bir alandır. Burada anlamlı bir değer sağlamanızı öneririz.

### <a name="install-maven-and-az-cli-spring-cloud-extension"></a>Maven ve Az CLI yay bulutu uzantısını yükleyin

Örnek ardışık yapı oluşturmak için Maven ve hizmet örneğine dağıtmak için Az CLI kullanır. Jenkins yüklendiğinde, *jenkins*adında bir yönetici hesabı oluşturur. Kullanıcı *jenkins'in* yay bulutu uzantısını çalıştırmak için izin verdiğinden emin olun.

1. SSH üzerinden Jenkins ustasına bağlanın. 

2. Maven'i Yükle

    ```bash
        sudo apt-get install maven 
    ```

3. Azure CLI yükleyin. Daha fazla bilgi için Azure [CLI'yi yükleme'ye](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)bakın. Azure Master'ı [Azure'da](https://aka.ms/jenkins-on-azure)kullanırsanız Azure CLI varsayılan olarak yüklenir.

4. Kullanıcıya `jenkins` geçin:

    ```bash
        sudo su jenkins
    ```

5. Yay **bulutu** uzantısını ekleyin:

    ```bash
        az extension add --name spring-cloud
    ```

## <a name="create-a-jenkinsfile"></a>Jenkinsdosyası oluşturma
1. Kendi repohttps://github.com/&lt(; github&gt;id / piggymetrics), kök bir **Jenkinsfile** oluşturun.

2. Dosyayı aşağıdaki gibi güncelleştirin. Kaynak grubu adı>ve ** \<** ** \<hizmet adı **>değerlerini değiştirdiğinden emin olun. Jenkins'teki kimlik belgesini eklediyseniz farklı bir değer kullanıyorsanız **azure_service_principal** doğru kimlikle değiştirin. 

```groovy
    node {
      stage('init') {
        checkout scm
      }
      stage('build') {
        sh 'mvn clean package'
      }
      stage('deploy') {
        withCredentials([azureServicePrincipal('azure_service_principal')]) {
          // login to Azure
          sh '''
            az login --service-principal -u $AZURE_CLIENT_ID -p $AZURE_CLIENT_SECRET -t $AZURE_TENANT_ID
            az account set -s $AZURE_SUBSCRIPTION_ID
          '''  
          // Set default resource group name and service name. Replace <resource group name> and <service name> with the right values
          sh 'az configure --defaults group=<resource group name>'
          sh 'az configure --defaults spring-cloud=<service name>'
          // Deploy applications
          sh 'az spring-cloud app deploy -n gateway --jar-path ./gateway/target/gateway.jar'
          sh 'az spring-cloud app deploy -n account-service --jar-path ./account-service/target/account-service.jar'
          sh 'az spring-cloud app deploy -n auth-service --jar-path ./auth-service/target/auth-service.jar'
          sh 'az logout'
        }
      }
    }
```

3. Değişimi kaydedin ve gerçekleştirin.

## <a name="create-the-job"></a>İşi oluşturma

1. Jenkins panosunda **Yeni Öğe'yi**tıklatın.

2. Bir ad sağlayın, iş için *Dağıtım-PiggyMetrics* ve **Pipeline**seçin. Tamam'a tıklayın.

3. Ardından, **Pipeline** (İşlem hattı) sekmesine tıklayın.

4. **Definition** (Tanım) için **Pipeline script from SCM**'yi (SCM'den işlem hattı betiği) seçin.

5. **SCM** için **Git**'i seçin.

6. Çatallı repo'nuz için GitHub URL'sini girin: ** https://github.com/&lt;GitHub&gt;kimliğiniz /piggymetrics.git**

7. Şube **Belirticinin ('any' için siyah)** ***/Azure** olduğundan emin olun

8. **Komut Dosyası yolunu** **Jenkinsfile** olarak tutun

7. **Kaydet'i** tıklatın

## <a name="validate-and-run-the-job"></a>İşi doğrulayın ve çalıştırın

İşi çalıştırmadan önce, **giriş kimliğine girmek**için giriş kutusundaki metni güncelleştirelim.

1. Kendi `index.html` repo'nuzda **,/gateway/src/main/resources/static/**

2. "Giriş inizi girin" araması yapın ve "giriş Kimliği girin" için güncelleyin

    ```HTML
        <input class="frontforms" id="frontloginform" name="username" placeholder="enter login ID" type="text" autocomplete="off"/>
    ```

3. Değişiklikleri işleme

4. Jenkins'teki işi elle yürüt. Jenkins panosunda, iş *Dağıt-PiggyMetrics'i* tıklatın ve ardından **Şimdi Oluştur'u oluşturun.**

İş tamamlandıktan sonra ağ **geçidi** uygulamasının genel IP'sine gidin ve uygulamanızın güncelleştirildiğini doğrulayın. 

![Güncellenmiş Piggy Ölçümleri](media/tutorial-jenkins-deploy-cli-spring-cloud/piggymetrics.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekmediğinde, bu makalede oluşturulan kaynakları silin:

```bash
az group delete -y --no-wait -n <resource group name>
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Azure Bahar Bulutu için sürekli tümleştirme ve teslimi (CI/CD) otomatikleştirmek için Jenkins'te Azure CLI'yi nasıl kullanacağınızı öğrendiniz.

Azure Jenkins sağlayıcısı hakkında daha fazla bilgi edinmek için Azure sitesindejenkins'e bakın.

> [!div class="nextstepaction"]
> [Azure'da Jenkins](/azure/jenkins/)
