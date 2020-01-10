---
title: Jenkins ve Azure CLı kullanarak Azure Spring buluta uygulama dağıtma
description: Azure Spring Cloud Service 'e mikro hizmetler dağıtmak için sürekli tümleştirme ve dağıtım işlem hattında Azure CLı 'yi nasıl kullanacağınızı öğrenin
ms.topic: tutorial
ms.date: 01/07/2020
ms.openlocfilehash: 67ad97bb762ed302ef52c404d47c5755ea4b245b
ms.sourcegitcommit: c32050b936e0ac9db136b05d4d696e92fefdf068
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2020
ms.locfileid: "75734981"
---
# <a name="tutorial-deploy-apps-to-azure-spring-cloud-using-jenkins-and-the-azure-cli"></a>Öğretici: Jenkins ve Azure CLı kullanarak Azure Spring buluta uygulama dağıtma

[Azure Spring Cloud](https://docs.microsoft.com/azure/spring-cloud/spring-cloud-overview) , yerleşik hizmet bulma ve yapılandırma yönetimiyle tam olarak yönetilen bir mikro hizmet geliştirmesinde bulunur. Hizmet, Spring Boot tabanlı mikro hizmet uygulamalarının Azure 'a dağıtılmasını kolaylaştırır. Bu öğreticide, Azure Spring Cloud için sürekli tümleştirme ve teslimi (CI/CD) otomatik hale getirmek için Jenkins 'de Azure CLı 'yı nasıl kullanabileceğiniz gösterilmektedir.

Bu öğreticide, şu görevleri tamamlayacaksınız:

> [!div class="checklist"]
> * Bir hizmet örneği sağlayın ve bir Java Spring uygulaması başlatın
> * Jenkins sunucunuzu hazırlama
> * Mikro hizmet uygulamalarını derlemek ve dağıtmak için Jenkins ardışık düzeninde Azure CLı 'yı kullanma 

Bu öğreticide, temel Azure Hizmetleri, Azure yay bulutu, Jenkins işlem [hatları](https://jenkins.io/doc/book/pipeline/) ve GitHub ve GitHub hakkında ara bilgi varsayılmaktadır.

## <a name="prerequisites"></a>Ön koşullar

>[!Note]
> Azure yay bulutu Şu anda genel önizleme olarak sunulmaktadır. Genel Önizleme teklifleri, müşterilerin resmi sürümünden önceki yeni özelliklerle deneme yapmasına olanak tanır.  Genel Önizleme özellikleri ve Hizmetleri üretim kullanımı için tasarlanmamıştır.  Önizleme sırasında destek hakkında daha fazla bilgi için lütfen [SSS](https://azure.microsoft.com/support/faq/) veya dosya dosyası [destek isteği](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request) inceleyerek daha fazla bilgi edinebilirsiniz.

[!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../../includes/open-source-devops-prereqs-azure-subscription.md)]

* GitHub hesabı. GitHub hesabınız yoksa başlamadan önce [ücretsiz bir hesap](https://github.com/) oluşturun.

* Bir Jenkins ana sunucusu. Zaten bir Jenkins ana hesabınız yoksa, bu [hızlı](https://docs.microsoft.com/azure/jenkins/install-jenkins-solution-template)başlangıçta bulunan adımları izleyerek Azure 'Da [jenkins](https://aka.ms/jenkins-on-azure) dağıtın. Jenkins düğümünde/aracısında aşağıdakiler gereklidir (örneğin,. yapı sunucusu):

    * [Git](https://git-scm.com/)
    * [JDK 8](https://docs.microsoft.com/java/azure/jdk/?view=azure-java-stable)
    * [Maven 3,0 veya üzeri](https://maven.apache.org/download.cgi)
    * [Azure CLI yüklü](/cli/azure/install-azure-cli?view=azure-cli-latest), sürüm 2.0.67 veya üzeri

    >[!TIP]
    > Git, JDK, az CLı ve Azure plug 'ler gibi araçlar, Azure Marketi [Microsoft Jenkins](https://aka.ms/jenkins-on-azure) çözüm şablonunda varsayılan olarak eklenmiştir.
    
* [Azure aboneliğine kaydolma](https://azure.microsoft.com/free/)
 
## <a name="provision-a-service-instance-and-launch-a-java-spring-application"></a>Bir hizmet örneği sağlayın ve bir Java Spring uygulaması başlatın

Örnek Microsoft hizmet uygulaması olarak [Piggy ölçümlerini](https://github.com/Azure-Samples/piggymetrics) kullanıyoruz ve [hızlı başlangıç: Azure CLI kullanarak bir Java Spring uygulaması başlatarak](https://docs.microsoft.com/azure/spring-cloud/spring-cloud-quickstart-launch-app-cli) hizmet örneğini temin edin ve uygulamaları ayarlayın. Aynı işlemden daha önce gittiğinden sonraki bölüme atlayabilirsiniz. Aksi takdirde, Azure CLı komutları aşağıda verilmiştir. [Hızlı başlangıç: Azure CLI kullanarak bir Java Spring uygulaması başlatarak](https://docs.microsoft.com/azure/spring-cloud/spring-cloud-quickstart-launch-app-cli) daha fazla arka plan bilgisi alın bölümüne bakın.

Yerel makinenizin Jenkins yapı sunucusuyla aynı önkoşulu karşılaması gerekir. Mikro hizmet uygulamalarını derlemek ve dağıtmak için aşağıdakilerin yüklü olduğundan emin olun:
    * [Git](https://git-scm.com/)
    * [JDK 8](https://docs.microsoft.com/java/azure/jdk/?view=azure-java-stable)
    * [Maven 3,0 veya üzeri](https://maven.apache.org/download.cgi)
    * [Azure CLI yüklü](/cli/azure/install-azure-cli?view=azure-cli-latest), sürüm 2.0.67 veya üzeri

1. Azure yay bulutu uzantısını yükler:

    ```Azure CLI
        az extension add --name spring-cloud
    ```

2. Azure yay bulut hizmetinizi içerecek bir kaynak grubu oluşturun:

    ```Azure CLI
        az group create --location eastus --name <resource group name>
    ```

3. Azure Spring Cloud 'ın bir örneğini sağlayın:

    ```Azure CLI
        az spring-cloud create -n <service name> -g <resource group name>
    ```

4. [Piggy ölçümleri](https://github.com/Azure-Samples/piggymetrics) deposunu kendi GitHub hesabınıza çatalla. Yerel makinenizde `source-code`adlı bir dizinde depoyu kopyalayın:

    ```bash
        mkdir source-code
        git clone https://github.com/<your GitHub id>/piggymetrics
    ```

5. Yapılandırma sunucunuzu ayarlayın. GitHub kimliği&gt; doğru değerle &lt;değiştirdiğinizden emin olun.

    ```Azure CLI
        az spring-cloud config-server git set -n <your-service-name> --uri https://github.com/<your GitHub id>/piggymetrics --label config
    ```

6. Projeyi derleyin:

    ```bash
        cd piggymetrics
        mvn clean package -D skipTests
    ```

7. Üç mikro hizmeti oluşturun: **Gateway**, **AUTH-Service**ve **Account-Service**:

    ```Azure CLI
        az spring-cloud app create --n gateway -s <service name> -g <resource group name>
        az spring-cloud app create --n auth-service -s <service name> -g <resource group name>
        az spring-cloud app create --n account-service -s <service name> -g <resource group name>
    ```

8. Uygulamaları dağıtma: 

    ```Azure CLI
        az spring-cloud app deploy -n gateway -s <service name> -g <resource group name> --jar-path ./gateway/target/gateway.jar
        az spring-cloud app deploy -n account-service -s <service name> -g <resource group name> --jar-path ./account-service/target/account-service.jar
        az spring-cloud app deploy -n auth-service -s <service name> -g <resource group name> --jar-path ./auth-service/target/auth-service.jar
    ```

9. Ağ geçidine genel uç nokta ata:

    ```Azure CLI
        az spring-cloud app update -n gateway -s <service name> -g <resource group name> --is-public true
    ```

10. Uygulamanın çalıştığını doğrulayabilmeniz için, URL 'yi almak üzere ağ geçidi uygulamasını sorgulayın.

    ```Azure CLI
    az spring-cloud app show --name gateway | grep url
    ```
    
    Plımetrimetrik uygulamasını çalıştırmak için önceki komutun sunduğu URL 'ye gidin. 

## <a name="prepare-jenkins-server"></a>Jenkins sunucusunu hazırlama

Bu bölümde, Jenkins sunucusunu test için ince olan bir derlemeyi çalıştıracak şekilde hazırlarsınız. Ancak güvenlik nedeniyle, derlemelerinizi çalıştırmak üzere Azure 'da bir aracı çalıştırmak için bir [Azure VM Aracısı](https://plugins.jenkins.io/azure-vm-agents) veya [Azure Kapsayıcı Aracısı](https://plugins.jenkins.io/azure-container-agents) kullanmanız gerekir. Daha fazla bilgi için [ana sunucuda derleme çalıştırmanın güvenlik sonuçları](https://wiki.jenkins.io/display/JENKINS/Security+implication+of+building+on+master) konulu Jenkins makalesine bakın.

### <a name="install-plug-ins"></a>Eklentileri yükler

1. Jenkins sunucunuzda oturum açın. **Eklentileri yönet > Jenkins 'ı Yönet**' i seçin.
2. **Kullanılabilir** sekmesinde, aşağıdaki eklentileri seçin:
    * [GitHub tümleştirmesi](https://plugins.jenkins.io/github-pullrequest)
    * [Azure kimlik bilgisi](https://plugins.jenkins.io/azure-credentials)

    Bu eklentiler listede görünmezse, zaten yüklü olup olmadığını görmek için **yüklü** sekmesini işaretleyin.

3. Eklentileri yüklemek için **Şimdi İndir ' i ve yeniden başlattıktan sonra Yükle**' yi seçin.

4. Yüklemeyi gerçekleştirmek için Jenkins sunucunuzu yeniden başlatın.

### <a name="add-your-azure-service-principal-credential-in-jenkins-credential-store"></a>Azure hizmet sorumlusu kimlik bilgilerinizi Jenkins kimlik bilgileri deposuna ekleyin

1. Azure 'a dağıtmak için bir Azure hizmet sorumlusu gerekir. Daha fazla bilgi için, dağıtma Azure App Service öğreticisindeki [hizmet sorumlusu oluşturma](https://docs.microsoft.com/azure/jenkins/tutorial-jenkins-deploy-web-app-azure-app-service#create-service-principal) bölümüne bakın. `az ad sp create-for-rbac` çıkışı şuna benzer:

    ```
    {
        "appId": "xxxxxx-xxx-xxxx-xxx-xxxxxxxxxxxx",
        "displayName": "xxxxxxxjenkinssp",
        "name": "http://xxxxxxxjenkinssp",
        "password": "xxxxxx-xxx-xxxx-xxx-xxxxxxxxxxxx",
        "tenant": "xxxxxx--xxx-xxxx-xxx-xxxxxxxxxxxx"
    }
    ```

2. Jenkins panosunda **Credentials** > **System**'ı (Kimlik Bilgileri > Sistem) seçin. Ardından, **Global credentials(unrestricted)** (Genel kimlik bilgileri (sınırsız)) seçeneğini belirleyin.

3. **Kimlik bilgileri ekle**' yi seçin. 

4. **Hizmet sorumlusunu** tür olarak Microsoft Azure seçin.

5. Şu değerleri sağlayın: * abonelik KIMLIĞI: Azure abonelik KIMLIĞINIZI kullanın * Istemci KIMLIĞI: Use `appId` * Istemci gizli dizisi: Use `password` * kiracı KIMLIĞI: `tenant` * Azure ortamını kullanın: önceden ayarlanmış bir değer seçin. Örneğin, Azure için **Azure** genel * kimlik: **azure_service_principal**olarak ayarla ' yı kullanın. Bu kodu bu makalenin sonraki bir adımında kullanıyoruz * Açıklama: isteğe bağlı bir alandır. Burada anlamlı bir değer sağlanması önerilir.

### <a name="install-maven-and-az-cli-spring-cloud-extension"></a>Maven 'i ve az CLı Spring-Cloud uzantısını yükler

Örnek işlem hattı, hizmet örneğine dağıtmak için Maven 'yi kullanır ve az CLı kullanır. Jenkins yüklendiğinde, *Jenkins*adlı bir yönetici hesabı oluşturur. *Jenkins* kullanıcısının Spring-Cloud uzantısını çalıştırma izni olduğundan emin olun.

1. SSH ile Jenkins ana 'e bağlanın. 

2. Maven 'i yükler

    ```bash
        sudo apt-get install maven 
    ```

3. Azure CLI yükleyin. Daha fazla bilgi için bkz. [Azure CLI 'Yi yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest). Azure ['Da Jenkins Master](https://aka.ms/jenkins-on-azure)kullanıyorsanız, Azure CLI varsayılan olarak yüklenir.

4. `jenkins` kullanıcısına geç:

    ```bash
        sudo su jenkins
    ```

5. **Yay-bulut** uzantısını ekleyin:

    ```bash
        az extension add --name spring-cloud
    ```

## <a name="create-a-jenkinsfile"></a>Jenkinsfile oluşturma
1. Kendi deponuzda (https://github.com/&lt ; GitHub kimliğiniz&gt; /Pbir ölçümdür), kökte bir **Jenkinsfile** oluşturun.

2. Dosyayı aşağıdaki gibi güncelleştirin. **\<kaynak grubu adı >** ve **\<hizmet adı >** değerlerini değiştirdiğinizden emin olun. Jenkins 'e kimlik bilgisini eklediğinizde farklı bir değer kullanırsanız, **azure_service_principal** doğru kimlikle değiştirin. 

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

3. Değişiklikleri kaydedin ve işleyin.

## <a name="create-the-job"></a>İşi oluşturma

1. Jenkins panosunda **Yeni öğe**' ye tıklayın.

2. İş için bir ad, *Deploy-POF ölçümleri* sağlayın ve işlem **hattı**seçin. Tamam'a tıklayın.

3. Ardından, **Pipeline** (İşlem hattı) sekmesine tıklayın.

4. **Definition** (Tanım) için **Pipeline script from SCM**'yi (SCM'den işlem hattı betiği) seçin.

5. **SCM** için **Git**'i seçin.

6. Tüm deponuz için GitHub URL 'sini girin: **https://github.com/&lt ; GitHub kimliğiniz&gt; /ptuıgıt**

7. **Dal belirticisinin (' any ' için siyah)** * **/Azure** olduğundan emin olun

8. **Betik yolunu** **Jenkinsfile** olarak tut

7. **Kaydet**’e tıklayın

## <a name="validate-and-run-the-job"></a>İşi doğrulama ve çalıştırma

İşi çalıştırmadan önce, oturum açma **kimliğini girmek**için oturum açma giriş kutusundaki metni güncelleştirelim.

1. Kendi deponuzda, **/Gateway/src/Main/Resources/static/** içinde `index.html` açın

2. "Oturum açma bilgilerini girin" ifadesini arayın ve "oturum açma KIMLIĞINI gir" olarak güncelleştirin

    ```HTML
        <input class="frontforms" id="frontloginform" name="username" placeholder="enter login ID" type="text" autocomplete="off"/>
    ```

3. Değişiklikleri Yürüt

4. İşi Jenkins 'te el ile çalıştırın. Jenkins panosunda, iş *dağıtımı-Pmbölçümleri* ' ne tıklayın ve ardından **Şimdi derleyin**.

İş tamamlandıktan sonra, **ağ geçidi** UYGULAMASıNıN genel IP adresine gidin ve uygulamanızın güncelleştirildiğinden emin olun. 

![Piggy ölçümleri güncelleştirildi](media/tutorial-jenkins-deploy-cli-spring-cloud/piggymetrics.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse, bu makalede oluşturulan kaynakları silin:

```bash
az group delete -y --no-wait -n <resource group name>
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Azure yay bulutu için sürekli tümleştirme ve teslimi (CI/CD) otomatik hale getirmek üzere Jenkins 'de Azure CLı 'yi nasıl kullanacağınızı öğrendiniz.

Azure Jenkins sağlayıcısı hakkında daha fazla bilgi edinmek için bkz. Azure sitesindeki Jenkins.

> [!div class="nextstepaction"]
> [Azure üzerinde Jenkins](/azure/jenkins/)
