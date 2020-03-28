---
title: "Öğretici: GitHub'dan Jenkins ile Azure Uygulama Hizmetine dağıtın"
description: Jenkins'i GitHub'dan sürekli tümleştirme (CI) ve Java web uygulamaları için Azure Uygulama Hizmeti'ne sürekli dağıtım (CD) için ayarlama
ms.topic: tutorial
ms.date: 10/23/2019
ms.custom: seo-java-july2019, seo-java-august2019, seo-java-september2019
ms.openlocfilehash: 9fcf178b71ac1f07bfb58cd2502701ae5392b472
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "74158391"
---
# <a name="tutorial-deploy-from-github-to-azure-app-service-with-jenkins-continuous-integration-and-deployment"></a>Öğretici: Jenkins sürekli tümleştirme ve dağıtım ile GitHub'dan Azure Uygulama Hizmeti'ne dağıtın

Bu öğretici, Jenkins'te sürekli tümleştirme (CI) ve sürekli dağıtım (CD) kurarak GitHub'dan [Linux'taki Azure Uygulama Hizmeti'ne](/azure/app-service/containers/app-service-linux-intro) örnek bir Java web uygulaması dağıtır. Uygulamayı GitHub'a iterek güncellediğinizde Jenkins, uygulamanızı otomatik olarak oluşturur ve Azure Uygulama Hizmeti'ne yeniden yayınlar. Bu öğreticideki örnek uygulama [Bahar Önyükleme](https://projects.spring.io/spring-boot/) çerçevesi kullanılarak geliştirilmiştir. 

![GitHub'dan Azure Uygulama Hizmeti dağıtımına genel bakış](media/tutorial-jenkins-deploy-web-app-azure-app-service/azure-continuous-integration-deployment-overview.png)

Bu öğreticide, şu görevleri tamamlayasınız:

> [!div class="checklist"]
> * GitHub'dan oluşturabilmeniz, Azure Uygulama Hizmeti'ne dağıtabilmeniz ve diğer ilgili görevleri yapabilmek için Jenkins eklentilerini yükleyin.
> * Çatal örnek GitHub repo böylece çalışan bir kopyasını var.
> * Jenkins'i GitHub'a bağlayın.
> * Jenkins'in kimlik bilgilerinizi kullanmadan Azure'a erişebilmeleri için bir Azure hizmet ilkesi oluşturun.
> * Hizmet müdürünü Jenkins'e ekle.
> * Uygulamayı GitHub'da her güncellediğinizde örnek uygulamayı oluşturan ve dağıtan Jenkins ardışık hattını oluşturun.
> * Jenkins ardınız için oluşturma ve dağıtım dosyaları oluşturun.
> * Jenkins boru hattını yapı ve dağıtım komut dosyasına doğrult.
> * El ile oluşturma yı çalıştırarak örnek uygulamanızı Azure'a dağıtın.
> * GitHub'da Jenkins'in Azure'u oluşturmasına ve yeniden dağıtmasına neden olan bir uygulama güncelleştirmesi taşıyın.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için şu öğelere ihtiyacınız vardır:

* Azure Linux VM'de yüklü Java Geliştirme Kiti (JDK) ve Maven araçlarına sahip bir [Jenkins](https://jenkins.io/) sunucusu

  Jenkins sunucunuz yoksa, Azure portalında şu adımları tamamlayın: [Azure Linux VM'de Jenkins sunucusu oluşturun](/azure/jenkins/install-jenkins-solution-template)

* Bir [GitHub](https://github.com) hesabı böylece örnek Java web uygulaması için çalışan bir kopyasını (çatal) alabilirsiniz. 

* Yerel komut satırınızdan veya [Azure Bulut BulutU'nuzdan](/azure/cloud-shell/overview) çalıştırabileceğiniz [Azure CLI](/cli/azure/install-azure-cli)

## <a name="install-jenkins-plug-ins"></a>Jenkins eklentilerini yükleme

1. Jenkins web konsolunuzda bu konumda oturum açın:

   `https://<Jenkins-server-name>.<Azure-region>.cloudapp.azure.com`

1. Jenkins ana sayfasında, **Jenkins'i** > **Yönet Eklentilerini yönet'i**seçin.

   ![Jenkins eklentilerini yönetme](media/tutorial-jenkins-deploy-web-app-azure-app-service/manage-jenkins-plugins.png)

1. **Kullanılabilir** sekmesinde aşağıdaki eklentileri seçin:

   - [Azure Uygulama Hizmeti](https://plugins.jenkins.io/azure-app-service)
   - [GitHub Şube Kaynağı](https://plugins.jenkins.io/github-branch-source)
   - Jenkins [Çevre Enjektör Plug-in](https://plugins.jenkins.io/envinject)
   - [Azure Kimlik Bilgileri](https://plugins.jenkins.io/azure-credentials)

   Bu eklentiler görünmüyorsa, **Yüklü** sekmesini denetleyerek zaten yüklü olmadıklarından emin olun.

1. Seçili eklentilerinizi yüklemek için **şimdi indir'i seçin ve yeniden başlattıktan sonra yükleyin.**

1. İşiniz bittikten sonra Jenkins menüsünde **Jenkins'i Yönet'i** seçin, böylece ilerideki adımlar için Jenkins yönetim sayfasına dönün.

## <a name="fork-sample-github-repo"></a>Çatal örnek GitHub repo

1. [Bahar Önyükleme örnek uygulaması için GitHub repo'da oturum açın.](https://github.com/spring-guides/gs-spring-boot) 

1. GitHub'ın sağ üst köşesinde **Çatal'ı**seçin.

   ![GitHub'dan çatal örnek repo](media/tutorial-jenkins-deploy-web-app-azure-app-service/fork-github-repo.png)

1. GitHub hesabınızı seçmek ve forking'i bitirmek için istemleri izleyin.

Ardından, GitHub kimlik bilgilerinizle Jenkins'i ayarlayın.

## <a name="connect-jenkins-to-github"></a>Jenkins'i GitHub'a bağlayın

Jenkins'in GitHub'ı izlemesini ve github çatalınızdaki web uygulamanıza yeni taahhütler geldiğinde yanıt vermesi için Jenkins'teki [GitHub webhook'larını](https://developer.github.com/webhooks/) etkinleştirin.

> [!NOTE]
> 
> Bu adımlar, Jenkins'in GitHub kullanıcı adınızı ve parolanızı kullanarak GitHub ile çalışması için kişisel erişim belirteç kimlik bilgileri oluşturur. 
> Ancak, GitHub hesabınız iki faktörlü kimlik doğrulama kullanıyorsa, belirtecinizi GitHub'da oluşturun ve Jenkins'in bu belirteci kullanmasını ayarlayın. 
> Daha fazla bilgi için [Jenkins GitHub eklenti](https://wiki.jenkins.io/display/JENKINS/GitHub+Plugin) belgelerine bakın.

1. **Jenkins'i Yönet** sayfasından **Yapıl'ı**seçin. 

   ![Jenkins'te sistemi yapılandırma](media/tutorial-jenkins-deploy-web-app-azure-app-service/manage-jenkins-configure-system.png)

1. GitHub bölümünde, **GitHub** sunucunuz için ayrıntılı bilgi sağlayın. **GitHub Sunucu Ekle** listesinden **GitHub Server'ı**seçin. 

   ![Jenkins'te GitHub sunucusu ekle](media/tutorial-jenkins-deploy-web-app-azure-app-service/add-GitHub-server.png)

1. Yönet **kancaları** özelliği seçili değilse, bu özelliği seçin. Diğer ayarları belirtebilmek için **Gelişmiş'i** seçin. 

   ![GitHub Server için gelişmiş Jenkins ayarlarını belirtin](media/tutorial-jenkins-deploy-web-app-azure-app-service/advanced-GitHub-settings.png)

1. Ek **GitHub eylemlerini yönet** listesinden **giriş ve parolayı dönüştür'ü ve belirteç'i**seçin.

   ![Giriş ve parolayı GitHub için belirteç olarak dönüştürme](media/tutorial-jenkins-deploy-web-app-azure-app-service/manage-additional-actions.png)

1. GitHub kullanıcı adınızı ve şifrenizi girebilmeniz için **giriş ve paroladan** seçin. İşi bittiğinde, [GitHub kişisel erişim belirteci (PAT)](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/)oluşturan **belirteç kimlik bilgilerini oluştur'u**seçin.   

   ![Oturum açma adı ve paroladan GitHub PAT oluşturma](media/tutorial-jenkins-deploy-web-app-azure-app-service/create-github-token-credentials.png)

1. **GitHub Server** bölümünde, **Kimlik Bilgileri** listesinden yeni belirteçinizi seçin. Test **bağlantısını**seçerek kimlik doğrulamanın çalışıp çalışmadığını denetleyin.

   ![Yeni PAT ile GitHub sunucusuna bağlantıyı kontrol edin](media/tutorial-jenkins-deploy-web-app-azure-app-service/check-github-connection.png)

Ardından, Jenkins'in Azure kaynaklarına kimlik doğrulamak ve erişmek için kullandığı Azure hizmet ilkesini oluşturun.

## <a name="create-service-principal"></a>Hizmet sorumlusu oluşturma

Daha sonraki bir bölümde, uygulamanızı GitHub'dan oluşturan ve uygulamanızı Azure Uygulama Hizmeti'ne dağıtan bir Jenkins boru hattı işi oluşturursunuz. Jenkins'in kimlik bilgilerinizi girmeden Azure'a erişmelerini sağlamak için, Jenkins için Azure Active Directory'de bir [hizmet yöneticisi](https://docs.microsoft.com/azure/active-directory/develop/app-objects-and-service-principals) oluşturun. Hizmet sorumlusu, Jenkins'in Azure kaynaklarına erişimin kimliğini doğrulamak için kullanabileceği ayrı bir kimliktir. Bu hizmet ilkesini oluşturmak için, [**`az ad sp create-for-rbac`**](https://docs.microsoft.com/cli/azure/create-an-azure-service-principal-azure-cli?view=azure-cli-latest)örneğin yerel komut satırınızdan veya Azure Bulut Bulutu'ndan Azure CLI komutunu çalıştırın: 

```azurecli-interactive
az ad sp create-for-rbac --name "yourAzureServicePrincipalName" --password yourSecurePassword
```

Hizmet ana adının etrafında tırnak işaretleri kullandığınızdan emin olun. Ayrıca, [Azure Etkin Dizin parola kuralları ve kısıtlamalarına](/azure/active-directory/active-directory-passwords-policy)dayalı güçlü bir parola oluşturun. Parola sağlamazsanız, Azure CLI sizin için bir parola oluşturur. 

Komut tarafından oluşturulan çıktı **`create-for-rbac`** aşağıda veda edinilir: 

```json
{
   "appId": "yourAzureServicePrincipal-ID", // A GUID such as AAAAAAAA-AAAA-AAAA-AAAA-AAAAAAAAAAAA
   "displayName": "yourAzureServicePrincipalName", // A user-friendly name for your Azure service principal
   "name": "http://yourAzureServicePrincipalName",
   "password": "yourSecurePassword",
   "tenant": "yourAzureActiveDirectoryTenant-ID" // A GUID such as BBBBBBBB-BBBB-BBBB-BBBB-BBBBBBBBBBBB
}
```

> [!TIP]
> 
> Zaten bir hizmet müdürünüz varsa, bunun yerine bu kimliği yeniden kullanabilirsiniz.
> Kimlik doğrulama için hizmet temel değerlerini `appId`sağlarken, "ve `password` `tenant` özellik değerlerini" kullanın. 
> Varolan bir hizmet anabilim `displayName` daararken, özellik değerini kullanın.

## <a name="add-service-principal-to-jenkins"></a>Jenkins'e hizmet sorumlusu ekleme

1. Jenkins ana sayfasında Kimlik **Bilgileri** > **Sistemi'ni**seçin. 

1. **Sistem** sayfasında, **Etki Alanı**altında, Genel kimlik bilgilerini **(sınırsız)** seçin.

1. Sol menüden **Kimlik Bilgileri Ekle'yi**seçin.

1. **Tür** listesinden **Azure Hizmet Sorumlusu'nu**seçin.

1. Bu adımda tablotarafından açıklanan özelliklerde hizmet sorumlusunuz ve Azure aboneliğiniz için bilgileri sağlayın:

   ![Azure hizmet temel kimlik bilgileri ekleme](media/tutorial-jenkins-deploy-web-app-azure-app-service/add-service-principal-credentials.png)

   | Özellik | Değer | Açıklama | 
   |----------|-------|-------------| 
   | **Abonelik Kimliği** | <*yourAzureSubscription-ID*> | Azure aboneliğiniz için GUID değeri <p>**İpucu**: Azure abonelik kimliğinizi bilmiyorsanız, bu Azure CLI komutunu komut satırından veya Bulut `id` Kabuğu'ndan çalıştırın ve ardından GUID değerini kullanın: <p>`az account list` | 
   | **İstemci Kimliği** | <*yourAzureServicePrincipal-ID*> | Azure `appId` hizmet müdürünüz için daha önce oluşturulan GUID değeri | 
   | **Müşteri Sırrı** | <*yourSecurePassword*> | Azure `password` hizmet müdürünüz için sağladığınız değer veya "gizli" | 
   | **Kiracı Kimliği** | <*yourAzureActiveDirectoryTenant-ID*> | Azure `tenant` Etkin Dizin kiracınız için GUID değeri | 
   | **Kimliği** | <*yourAzureServicePrincipalName*> | Azure `displayName` hizmet müdürünüz için değer | 

1. Hizmet müdürünün çalıştığını doğrulamak için **Hizmet Anabilim Dali'ni doğrula'yı**seçin. İşiniz bittiğinde **Tamam**’ı seçin.

Ardından, uygulamanızı oluşturan ve dağıtan Jenkins ardışık hattını oluşturun.

## <a name="create-jenkins-pipeline"></a>Jenkins işlem hattı oluşturma

Jenkins'te, uygulamanızı oluşturmak ve dağıtmak için boru hattı işini oluşturun.

1. Jenkins giriş sayfanıza dönün ve **Yeni Öğe'yi**seçin. 

   ![Jenkins işlem hattı oluşturma](media/tutorial-jenkins-deploy-web-app-azure-app-service/jenkins-select-new-item.png)

1. Örneğin, "My-Java-Web-App" adlı boru hattı işiniz için bir ad sağlayın ve **Pipeline'ı**seçin. Altta **Tamam'ı**seçin.  

   ![Jenkins boru hattı işini adlandırın](media/tutorial-jenkins-deploy-web-app-azure-app-service/jenkins-select-pipeline.png)

1. Jenkins'i hizmet müdürünüzle ayarla, böylece Jenkins kendi kimlik bilgilerinizi kullanmadan Azure'a dağıtabilebile.

   1. **Genel** sekmesinde, **çalışma için ortam hazırla'yı**seçin. 

   1. Görünen **Özellikler İçeriği** kutusuna, bu ortam değişkenlerini ve değerlerini ekleyin. 

      ```ini
      AZURE_CRED_ID=yourAzureServicePrincipalName
      RES_GROUP=yourWebAppAzureResourceGroupName
      WEB_APP=yourWebAppName
      ```

      ![Çalışma için bir ortam hazırlayın ve ortam değişkenlerini ayarlayın](media/tutorial-jenkins-deploy-web-app-azure-app-service/prepare-environment-for-jenkins-run.png)

1. İşiniz bittiğinde **Kaydet**'i seçin.

Ardından, Jenkins için yapı ve dağıtım komut dosyaları oluşturun.

## <a name="create-build-and-deployment-files"></a>Oluşturma ve dağıtım dosyaları oluşturma

Şimdi Jenkins'in uygulamanızı oluşturmak ve dağıtmak için kullandığı dosyaları oluşturun.

1. GitHub fork'unuzun `src/main/resources/` klasöründe, bu XML'i içeren `web.config`ancak `$(JAR_FILE_NAME)` aşağıdakilerle `gs-spring-boot-0.1.0.jar`değiştirilen bu uygulama yapılandırma dosyanı oluşturun:

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <configuration>
      <system.webServer>
         <handlers>
            <add name="httpPlatformHandler" path="*" verb="*" modules="httpPlatformHandler" resourceType="Unspecified" />
         </handlers>
         <httpPlatform processPath="%JAVA_HOME%\bin\java.exe" arguments="-Djava.net.preferIPv4Stack=true -Dserver.port=%HTTP_PLATFORM_PORT% -jar &quot;%HOME%\site\wwwroot\${JAR_FILE_NAME}&quot;"></httpPlatform>
      </system.webServer>
   </configuration>
   ```

1. GitHub çatalınızın kök klasöründe, bu metni içeren `Jenkinsfile`bu yapı ve dağıtım komut dosyalarını[(burada GitHub'daki kaynak)](https://github.com/Microsoft/todo-app-java-on-azure/blob/master/doc/resources/jenkins/Jenkinsfile-webapp-se)oluşturun:

   ```groovy
   node {
      stage('init') {
         checkout scm
      }
      stage('build') {
         sh '''
            mvn clean package
            cd target
            cp ../src/main/resources/web.config web.config
            cp todo-app-java-on-azure-1.0-SNAPSHOT.jar app.jar 
            zip todo.zip app.jar web.config
         '''
      }
      stage('deploy') {
         azureWebAppPublish azureCredentialsId: env.AZURE_CRED_ID,
         resourceGroup: env.RES_GROUP, appName: env.WEB_APP, filePath: "**/todo.zip"
      }
   }
   ```

1. Hem `web.config` de `Jenkinsfile` dosyaları GitHub çatalınıza kaydedin ve değişikliklerinizi zorla.

## <a name="point-pipeline-at-script"></a>Komut dosyasında nokta ardışık

Şimdi Jenkins'in kullanmasını istediğiniz yapı ve dağıtım komut dosyasını belirtin.

1. Jenkins'te, daha önce oluşturulmuş boru hattı işinizi seçin. 

   ![Web uygulamanız için Jenkins boru hattı işini seçin](media/tutorial-jenkins-deploy-web-app-azure-app-service/select-pipeline-job.png)

1. Sol menüde **Yapıla'yı**seçin.

1. **Sözcük Hattı** sekmesinde, **Tanım** **listesinden, SCM'den Pipeline komut dosyası'nı**seçin.

   1. Görünen **SCM** kutusunda kaynak denetiminiz olarak **Git'i** seçin. 

   1. **Depolar** bölümünde, **Depo URL'si**için GitHub çatalınızın URL'sini girin, örneğin: 

      `https://github.com/<your-GitHub-username>/gs-spring-boot`

   1. **Kimlik Bilgileri**için, önceden oluşturulmuş GitHub kişisel erişim jetonu seçin.

   1. Komut **Dosyası Yolu** kutusunda, "Jenkinsfile" komut dosyasına yolu ekleyin.

   İşinizi bitirdiğinizde, boru hattı tanımınız aşağıdaki örnek gibi görünür: 

   ![Jenkins boru hattını senaryoya doğrult.](media/tutorial-jenkins-deploy-web-app-azure-app-service/set-up-jenkins-github.png)

1. İşiniz bittiğinde **Kaydet**'i seçin.

Ardından, uygulamanızı Azure Uygulama Hizmeti'ne oluşturun ve dağıtın. 

## <a name="build-and-deploy-to-azure"></a>Azure'da oluşturma ve dağıtma

1. Komut satırından veya Azure Cloud Shell'den Azure CLI ile, Jenkins'in bir yapıyı bitirdikten sonra web uygulamanızı dağıttığı [Linux'ta](/azure/app-service/containers/app-service-linux-intro) bir Azure Uygulama Hizmeti web uygulaması oluşturun. Web uygulamanızın benzersiz bir ada sahip olduğundan emin olun.

   ```azurecli-interactive
   az group create --name yourWebAppAzureResourceGroupName --location yourAzureRegion
   az appservice plan create --name yourLinuxAppServicePlanName --resource-group yourWebAppAzureResourceGroupName --is-linux
   az webapp create --name yourWebAppName --resource-group yourWebAppAzureResourceGroupName --plan yourLinuxAppServicePlanName --runtime "java|1.8|Tomcat|8.5"
   ```

   Bu Azure CLI komutları hakkında daha fazla bilgi için şu sayfalara bakın:

   * [**`az group create`**](https://docs.microsoft.com/cli/azure/group?view=azure-cli-latest#az-group-create)

   * [**`az appservice plan create`**](https://docs.microsoft.com/cli/azure/appservice/plan?view=azure-cli-latest#az-appservice-plan-create)

   * [**`az webapp create`**](https://docs.microsoft.com/cli/azure/webapp?view=azure-cli-latest#az-webapp-create)

1. Jenkins'te, boru hattı işinizi seçin ve **Şimdi Oluştur'u**seçin.

   Yapı bittikten sonra Jenkins, şu anda Yayın URL'sinde Azure'da canlı olan uygulamanızı dağıtır: 

   `http://<your-Java-web-app>.azurewebsites.net`

   ![Dağıtılan uygulamanızı Azure’da görüntüleme](media/tutorial-jenkins-deploy-web-app-azure-app-service/greetings-unedited.png)

## <a name="push-changes-and-redeploy"></a>Değişiklikleri gönderme ve yeniden dağıtma

1. Web tarayıcınızda, web uygulamanızın GitHub çatalında bu konuma gidin:

   `complete/src/main/java/Hello/Application.java`
   
1. GitHub'ın sağ üst köşesinden **bu dosyayı düzelt'i**seçin.

1. Bu değişikliği yöntemde `commandLineRunner()` yapın ve değişikliği repo'nun `master` dalına ada. `master` Bu şubede taahhüt Jenkins bir yapı başlar. 
   
   ```java
   System.out.println("Let's inspect the beans provided by Spring Boot on Azure");
   ```

1. Yapı bittikten ve Jenkins Azure'a yeniden dağıtıldıktan sonra, güncellemenizi şimdi gösteren uygulamanızı yenileyin.

   ![Dağıtılan uygulamanızı Azure’da görüntüleme](media/tutorial-jenkins-deploy-web-app-azure-app-service/greetings-edited.png)

## <a name="troubleshooting-the-jenkins-plug-in"></a>Jenkins eklentisinin sorun giderme

Jenkins eklentileri ile herhangi bir hata karşılaşırsanız, belirli bir bileşen için [Jenkins JIRA](https://issues.jenkins-ci.org/) bir sorun dosya.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure VM’lerini derleme aracısı olarak kullanma](/azure/jenkins/jenkins-azure-vm-agents)
