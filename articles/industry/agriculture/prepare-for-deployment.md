---
title: Azure Farmtts dağıtma
description: Bu makalede, Azure Farmtts 'nin nasıl dağıtılacağı açıklanır.
author: uhabiba04
ms.topic: article
ms.date: 11/04/2019
ms.author: v-umha
ms.openlocfilehash: 27bf62cb328273db1f7bdd44117853b00feca9ae
ms.sourcegitcommit: b5ff5abd7a82eaf3a1df883c4247e11cdfe38c19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/09/2019
ms.locfileid: "74941596"
---
# <a name="deploy-azure-farmbeats"></a>Azure Farmtts dağıtma

Bu makalede, Azure Farmtts 'nin nasıl ayarlanacağı açıklanır.

Azure Farm, Azure bulutuna, telemetri veri toplamaya ve toplamaya sorunsuz sağlama ve algılayıcı cihaz bağlantısı sağlayan, verilere dayalı farkliğe yönelik sektöre özgü, genişletilebilir bir çözümdür. Azure Farmırts; kameralar, Drones ve SOIL sensörleri gibi çeşitli sensörler içerir. Azure Farmalerts sayesinde cihazları buluttan yönetebilirsiniz. Bu, IoT 'e yönelik cihazlar için Azure 'da altyapı ve hizmetlerin yönetilmesini ve görselleştirme, uyarılar ve Öngörüler sağlamak için genişletilebilir bir Web ve mobil uygulama kullanmanızı içerir.

> [!NOTE]
> Azure Farmtts yalnızca genel bulut ortamlarında desteklenir. Daha fazla bilgi için bkz. [genel bulut nedir?](https://azure.microsoft.com/overview/what-is-a-public-cloud/).

Azure Farmtts aşağıdaki iki bileşene sahiptir:

- Veri hub 'ı: mevcut veya yeni veri işlem hatlarından Öngörüler oluşturmanıza, depolamanıza, işlemenize ve bunların içgörüler çizmenize olanak tanıyan Azure **Farmretts**'nin platform katmanı. Bu platform katmanı, tarım veri işlem hatlarınızı ve modellerini çalıştırmak ve oluşturmak için kullanışlıdır.

- **Hızlandırıcı**: Azure farmtts 'nin çözüm katmanı, önceden oluşturulmuş agricler modellerini kullanarak Azure Farm, yeteneklerini gösteren yerleşik bir uygulamaya sahiptir. Bu çözüm katmanı, Grup sınırları oluşturmanıza ve grup sınırının bağlamı içindeki agricbir veri kaynağından Öngörüler çizmenize imkan tanır.

Azure Farmtts 'nin hızlı dağıtımı bir saatten kısa sürer. Veri hub 'ı ve hızlandırıcının maliyetleri, kullanıma bağlı olarak farklılık gösterir.

## <a name="deployed-resources"></a>Dağıtılan kaynaklar

Azure Farmtts dağıtımı, aboneliğiniz dahilinde aşağıdaki kaynakları oluşturur:

| Ardışık. no.  | Kaynak adı  | Farmtempts bileşeni  |
|---------|---------|---------|
|1  |       Azure Cosmos DB   |  Veri hub 'ı       |
|2  |    Application Insights      |     Veri merkezi/Hızlandırıcı     |
|3  |Redis için Azure Cache   |Veri hub 'ı   |
|4  |       Azure Key Vault    |  Veri merkezi/Hızlandırıcı        |
|5  |    Azure Time Series Insights       |     Veri hub 'ı      |
|6 |      Azure Olay Hub 'ı ad alanı    |  Veri hub 'ı       |
|7  |    Azure Data Factory V2       |     Veri merkezi/Hızlandırıcı      |
|8  |Batch hesabı    |Veri hub 'ı   |
|9  |       Depolama hesabı     |  Veri merkezi/Hızlandırıcı        |
|10  |    Mantıksal uygulama        |     Veri hub 'ı      |
|11  |    API bağlantısı        |     Veri hub 'ı      |
|12|      Azure App Service      |  Veri merkezi/Hızlandırıcı       |
|13 |    App service planı        |     Veri merkezi/Hızlandırıcı      |
|14 |Azure haritalar hesabı     |Hızlandır    |

Azure Farmfor, doğrudan Azure portal erişebileceğiniz Azure Marketi 'nde indirilebilir.  

## <a name="create-an-azure-farmbeats-offer-in-azure-marketplace"></a>Azure Marketi 'nde Azure Farmırts teklifi oluşturma

Azure Marketi 'nde Azure Farmırts teklifi oluşturmak için aşağıdakileri yapın:

1. Azure portal oturum açın, sağ üst köşedeki hesabınızı seçin ve ardından Azure Farmtts 'yi dağıtmak istediğiniz Azure Active Directory (Azure AD) kiracısına geçiş yapın.
1. Azure Farmırts, Azure Marketi 'nde kullanılabilir. **Azure Marketi** sayfasında **Şimdi al**' ı seçin.
1. **Oluştur**'u seçin.

> [!NOTE]
> Azure Marketi 'nde teklifin tamamlanması yalnızca kurulum 'un bir parçasıdır. Azure aboneliğinizdeki Azure Farmınts dağıtımını gerçekleştirmek için sonraki bölümlerdeki yönergeleri izleyin.

## <a name="prepare"></a>Hazırlanın

Azure Farmtts 'yi dağıtabilmeniz için önce aşağıdaki izinlere sahip olmanız gerekir:

- **Kiracı**: okuma erişimi
- **Abonelik**: katkıda bulunan veya sahip
- **Kaynak grubu**: sahip

## <a name="before-you-begin"></a>Başlamadan önce

Dağıtıma başlamadan önce, aşağıdaki önkoşulların yerinde olduğundan emin olun:

- Bir Sentinel hesabı
- Azure AD uygulama kaydı

## <a name="create-a-sentinel-account"></a>Sentinel hesabı oluşturma    

Sentinel 'e sahip bir hesap, resmi web sitesinden cihazınızdan, Sentinel uydu canlandırın 'yi yüklemenize yardımcı olur. Ücretsiz bir hesap oluşturmak için aşağıdakileri yapın:

1. [Sentinel hesap kaydı sayfasına](https://scihub.copernicus.eu/dhus/#/self-registration)gidin.
1. Kayıt formunda, ad, soyadı, Kullanıcı adı, parola ve e-posta adresinizi belirtin.

Onay için kayıtlı e-posta adresine bir doğrulama e-postası gönderilir. E-posta adresinizi onaylamak için bağlantıyı seçin. Kayıt işleminiz tamamlanmıştır.

## <a name="create-an-azure-ad-app-registration"></a>Azure AD uygulama kaydı oluşturma

Azure Farm, kimlik doğrulaması ve yetkilendirme için bir Azure AD uygulama kaydınız olmalıdır. Bunu iki şekilde oluşturabilirsiniz:

* **Seçenek 1**: yükleyici, gerekli kiracı, abonelik ve kaynak grubu erişim izinlerine sahip olmanız kaydıyla kaydı otomatik olarak oluşturabilir. Bu izinler mevcutsa, ["Input. json dosyasını hazırlama"](#prepare-the-inputjson-file) bölümüne geçin.

* **2. seçenek**: Azure Farmttları dağıtmadan önce kaydı el ile oluşturup yapılandırabilirsiniz. Aboneliğiniz dahilinde bir Azure AD uygulama kaydı oluşturmak ve yapılandırmak için gerekli izinlere sahip olmadığınız durumlarda bu yöntemi öneririz. Yöneticinizden, BT yöneticisinin Azure portal Azure AD uygulama kaydını otomatik olarak oluşturup yapılandırmasına yardımcı olacak [özel betiği](https://aka.ms/FarmBeatsAADScript)kullanmasını isteyin. PowerShell ortamını kullanarak bu özel betiği çalıştırmaya yönelik bir çıktı olarak BT yöneticisinin sizinle bir Azure AD uygulama Istemci KIMLIĞI ve parola gizli anahtarı paylaşması gerekir. Bu değerleri bir yere unutmayın.

    Azure AD uygulama kayıt betiğini çalıştırmak için şunları yapın:

    1. [Betiği](https://aka.ms/FarmBeatsAADScript)indirin.
    1. Azure portal oturum açın ve aboneliğinizi ve Azure AD kiracınızı seçin.
    1. Azure portal en üstündeki araç çubuğundan Cloud Shell başlatın.

        ![Proje Farmtları](./media/prepare-for-deployment/navigation-bar-1.png)

    1. İlk kez Kullanıcı kullanıyorsanız, bir depolama hesabı ve Microsoft Azure dosya paylaşımının oluşturulması için bir abonelik seçmeniz istenir. **Depolama oluştur**'u seçin.
    1. Kullanıcılardan ilk kez, kabuk deneyimleri, bash veya PowerShell seçenekleri de sunulur. PowerShell ' i seçin.
    1. Betiği (adım 1 ' den Cloud Shell) karşıya yükleyin ve karşıya yüklenen dosyanın konumunu aklınızda edin.

        > [!NOTE]
        > Varsayılan olarak, dosya giriş dizininize yüklenir.

        Aşağıdaki betiği kullanın:

        ```azurepowershell-interactive
        ./create_aad_script.ps1
        ```
    1. Azure AD uygulama KIMLIĞI ve istemci gizli anahtarı 'nı, Azure Farmıd 'leri dağıtmaya çalışan kişiyle paylaşmak üzere bir yere unutmayın.

### <a name="prepare-the-inputjson-file"></a>Input. json dosyasını hazırlama

Yükleme kapsamında, burada gösterildiği gibi bir *input. JSON* dosyası oluşturun:

```json
{  
    "sku":"both",
    "subscriptionId":"da9xxxec-dxxf-4xxc-xxx21-xxx3ee7xxxxx",
    "datahubResourceGroup":"dummy-test-dh1",
    "location":"westus2",
    "datahubWebsiteName":"dummy-test-dh1",
    "acceleratorResourceGroup":" dummy-test-acc1",
    "acceleratorWebsiteName":" dummy-test-acc1",
    "sentinelUsername":"dummy-dev",
    "notificationEmailAddress":"dummy@yourorg.com",
    "updateIfExists":true
}
```

Bu dosya Azure Cloud Shell için giriş dosyanız. Yükleme sırasında değerleri kullanılan parametreleri içerir. JSON dosyasındaki tüm parametrelerin uygun değerlerle değiştirilmeleri veya kaldırılması gerekir. Bunlar kaldırıldığında, yükleyici yükleme sırasında sizden istemde yer alacak.

Dosyayı hazırlanmadan önce, aşağıdaki tablodaki parametreleri gözden geçirin:

|Komut | Açıklama|
|--- | ---|
|sku  | Azure Farmtts bileşenlerinden birini ya da her ikisini de indirmek için bir seçenek sağlar. Hangi bileşenlerin indirileceği belirler. Yalnızca Datahub 'ı yüklemek için "onlydatabhub" kullanın. Datahub ve Hızlandırıcı 'yı yüklemek için "both" kullanın.|
|subscriptionId | Azure Farmtts yükleme aboneliğini belirtir.|
|datahubResourceGroup| Veri hub kaynaklarınızın kaynak grubu adı.|
|location |Kaynakları oluşturmak istediğiniz konum.|
|Ivatorwebsitename |Datahub örneğinizi adlandırmak için benzersiz URL ön eki.|
|Ivatorresourcegroup  | Hızlandırıcı Web sitenizi adlandırmak için benzersiz URL ön eki.|
|datahubWebsiteName  | Datahub Web sitenizi adlandırmak için benzersiz URL ön eki. |
|sentinelUsername | Oturum açmak için Kullanıcı adı (örneğin, `https://scihub.copernicus.eu/dhus/#/self-registration`).|
|Notificationemapostaadresi  | Veri hub örneğiniz dahilinde yapılandırdığınız tüm uyarılar için bildirimleri alacak e-posta adresi.|
|updateIfExists| Seçim Yalnızca var olan bir Azure Farmtts örneğini yükseltmek istiyorsanız *input. JSON* dosyasına dahil edilecek bir parametre. Yükseltme için, kaynak grubu adları ve konumları gibi diğer ayrıntıların aynı olması gerekir.|
|Aadappclientıd | Seçim Yalnızca Azure AD uygulaması zaten varsa *input. JSON* dosyasına dahil edilecek bir parametre.  |
|aadAppClientSecret  | Seçim Yalnızca Azure AD uygulaması zaten varsa *input. JSON* dosyasına dahil edilecek bir parametre.|

## <a name="deploy-the-app-within-cloud-shell"></a>Uygulamayı Cloud Shell içinde dağıtma

Daha önce tartışılan Market iş akışının bir parçası olarak, bir kaynak grubu oluşturmuş ve gerçek dağıtımın bir parçası olarak yeniden gözden geçirebilmeniz için "Lisans Koşulları" sözleşmesini imzalamalısınız. Bash ortamını kullanarak uygulamayı Cloud Shell tarayıcı tabanlı komut satırı arabirimi aracılığıyla dağıtabilirsiniz. Cloud Shell aracılığıyla dağıtmak için sonraki bölümlere devam edin.

> [!NOTE]
> Etkin olmayan Cloud Shell oturumlarının süresi 20 dakika sonra dolacak. Bu süre içinde dağıtımı tamamlamayı deneyin.

1. Azure portal oturum açın ve kullanmak istediğiniz aboneliği ve Azure kiracıyı seçin.
1. Azure portal en üstündeki araç çubuğundan Cloud Shell başlatın.
1. İlk kez Cloud Shell kullanıyorsanız, bir depolama hesabı ve bir Azure dosya paylaşımının oluşturulması için bir abonelik seçmeniz istenir.
1. **Depolama oluştur**' u seçin.  
1. Ortam için **Bash** (PowerShell değil) seçeneğini belirleyin.

## <a name="deployment-scenario-1"></a>Dağıtım senaryosu 1

"Seçenek 1" içinde daha önce açıklanan bu senaryoda, yükleyici Azure AD uygulama kaydını otomatik olarak oluşturur. Aşağıdaki işlemleri gerçekleştirerek Farmtempts 'yi dağıtabilirsiniz:

1. Aşağıdaki örnek JSON şablonunu kopyalayın ve *input. JSON*olarak kaydedin. Yerel bilgisayarınızda dosya yolunu bir yere göz önünde olduğunuzdan emin olun.

    ```json
    {  
       "sku":"both", 
       "subscriptionId":"da9xxxec-dxxf-4xxc-xxx21-xxx3ee7xxxxx", 
       "datahubResourceGroup":"dummy-test-dh1", 
       "location":"eastus2", 
       "datahubWebsiteName":"dummy-test-dh1", 
       "acceleratorResourceGroup":" dummy-test-acc1",   
       "acceleratorWebsiteName":" dummy-test-acc1", 
       "sentinelUsername":"dummy-dev", 
       "notificationEmailAddress":" dummy@microsoft.com", 
       "updateIfExists":true 
    }
    ```

1. Azure Cloud Shell açın. Başarılı kimlik doğrulamasından sonra **karşıya yükle** düğmesini (aşağıdaki görüntüde vurgulanan) seçin ve ardından *Input. JSON* dosyasını Cloud Shell depolama alanına yükleyin.  

    ![Proje Farmtları](./media/prepare-for-deployment/bash-2-1.png)

1. Cloud Shell giriş dizinine gidin. Varsayılan olarak, dizin adı */Home/\<username >* olur.
1. Cloud Shell ' de, aşağıdaki komutu girin. *Input. JSON* dosyasının yolunu değiştirdiğinizden emin olun ve Enter tuşunu seçin.

   ```bash
      wget -O farmbeats-installer.sh https://aka.ms/AzureFarmbeatsInstallerScript && bash farmbeats-installer.sh /home/<username>/input.json
    ```
     Yükleyici tüm bağımlılıkları otomatik olarak indirir ve dağıtıcıyı oluşturur. Azure Farmınts lisans koşullarını kabul etmeniz istenir.

     - Kabul ediyorsanız, sonraki adıma geçmek için **Y** girin.
     - Kabul etmiyorsanız, **N**girin ve dağıtım sonlandırılır.

1. İstemde, dağıtım için bir erişim belirteci girin. Oluşturulan kodu kopyalayın ve Azure kimlik bilgilerinizle [cihaz oturum açma sayfasında](https://microsoft.com/devicelogin) oturum açın.

    > [!NOTE]
    > Belirtecin süresi 60 dakika sonra dolar. Süresi dolarsa, dağıtım komutunu yeniden yazarak yeniden başlatabilirsiniz.

1. İstemde, Sentinel hesap parolanızı girin.

   Yükleyici, dağıtımı doğrular ve başlatır. Bu işlem 20 dakika sürebilir.

   Dağıtım başarıyla tamamlandıktan sonra aşağıdaki çıkış bağlantılarını alırsınız:

    - **Datahub URL 'si**: Azure Farmtts API 'lerini denemek için Swagger bağlantısı.
    - **Hızlandırıcı URL 'si**: Azure Farm, akıllı grup hızlandırıcıyı keşfetmeye yönelik kullanıcı arabirimi.
    - **Deployer günlük dosyası**: dağıtım sırasında oluşturulan günlük dosyası. Gerekirse, sorun giderme için kullanabilirsiniz.

## <a name="deployment-scenario-2"></a>Dağıtım senaryosu 2

Daha önce "2. seçenek" bölümünde açıklanan bu senaryoda, mevcut Azure Active Directory Uygulama kaydınızı kullanarak, aşağıdakileri yaparak aşağıdaki işlemleri gerçekleştirebilirsiniz:

1. Azure Uygulama Istemci KIMLIĞINI ve parolayı *input. JSON* dosyasına dahil eden aşağıdaki json dosyasını kopyalayın ve kaydedin. Yerel bilgisayarınızda dosya yolunu bir yere göz önünde olduğunuzdan emin olun.

    ```json
   {

   "sku":"both",
   "subscriptionId":"daxx9xxx-d18f-4xxc-9c21-5xx3exxxxx45",
   "datahubResourceGroup":"dummy-test-dh1",   
   "location":"westus2",   
   "datahubWebsiteName":"dummy-test-dh1",   
   "acceleratorResourceGroup":"dummy-test-acc1",   
   "acceleratorWebsiteName":"dummy-test-acc1",   
   "sentinelUsername":"dummy-dev",
   "notificationEmailAddress":"dummyuser@org1.com",
   "updateIfExists": true,
   "aadAppClientId": "lmtlemlemylmylkmerkywmkm823",
   "aadAppClientSecret": "Kxxxqxxxxu*kxcxxx3Yxxu5xx/db[xxx"

   }
   ```

1. Azure Cloud Shell yeniden gidin ve kimlik doğrulaması başarıyla yapılır.
1. **Karşıya yükle** düğmesini (aşağıdaki görüntüde vurgulanan) seçin ve ardından *Input. JSON* dosyasını Cloud Shell depolama alanına yükleyin.

    ![Proje Farmtları](./media/prepare-for-deployment/bash-2-1.png)

1. Cloud Shell giriş dizinine gidin. Varsayılan olarak, dizin adı */Home/\<username >* olur.
1. Cloud Shell ' de, aşağıdaki komutu girin. *Input. JSON* dosyasının yolunu değiştirdiğinizden emin olun ve Enter tuşunu seçin.

    ```bash
    wget -O farmbeats-installer.sh https://aka.ms/AzureFarmbeatsInstallerScript && bash farmbeats-installer.sh /home/<username>/input.json
    ```

     Yükleyici tüm bağımlılıkları otomatik olarak indirir ve dağıtıcıyı oluşturur. Azure Farmınts lisans koşullarını kabul etmeniz istenir.

     - Kabul ediyorsanız, sonraki adıma geçmek için **Y** girin.
     - Kabul etmiyorsanız, **N**girin ve dağıtım sonlandırılır.

1. İstemde, dağıtım için bir erişim belirteci girin. Oluşturulan kodu kopyalayın ve Azure kimlik bilgilerinizle [cihaz oturum açma sayfasında](https://microsoft.com/devicelogin) oturum açın.

   Yükleyici, dağıtımı doğrular ve başlatır. Bu işlem 20 dakika sürebilir.

   Dağıtım başarıyla tamamlandıktan sonra aşağıdaki çıkış bağlantılarını alırsınız:

    - **Datahub URL 'si**: Azure Farmtts API 'lerini denemek için Swagger bağlantısı.
    - **Hızlandırıcı URL 'si**: Azure Farm, akıllı grup hızlandırıcıyı keşfetmeye yönelik kullanıcı arabirimi.
    - **Deployer günlük dosyası**: dağıtım sırasında oluşturulan günlük dosyası. Gerekirse, sorun giderme için kullanabilirsiniz.

Herhangi bir sorunla karşılaşırsanız [sorun giderme](troubleshoot-project-farmbeats.md)' yi gözden geçirin.


## <a name="validate-the-deployment"></a>Dağıtımı doğrulama

### <a name="datahub"></a>Veri hub 'ı

Datahub yüklemesi tamamlandıktan sonra, https://biçimindeki Swagger arabirimi aracılığıyla Azure Farmınts API 'Lerine erişmek için URL 'YI alacaksınız\<yourdatahub-website-name >. azurewebsites. net/Swagger.

1. Swagger aracılığıyla oturum açmak için URL 'YI kopyalayıp tarayıcınıza yapıştırın.
2. Azure portal kimlik bilgilerinizle oturum açın.
3. Swagger 'yi görebilir ve Azure Farmtts API 'Lerinde tüm REST işlemlerini gerçekleştirebilirsiniz. Bu, Azure Farmtts 'nin başarılı dağıtımını gösterir.

### <a name="accelerator"></a>Hızlandır

Hızlandırıcı yüklemesi tamamlandıktan sonra, https://\<Hızlandırıcı-Web sitesi-adı >. azurewebsites. net biçiminde Azure Farmtts Kullanıcı arabirimine erişmek için URL 'YI alacaksınız.

1. Hızlandırıcının oturumunu açmak için, URL 'YI kopyalayıp tarayıcınıza yapıştırın.
1. Azure portal kimlik bilgilerinizle oturum açın.

## <a name="upgrade"></a>Yükseltin

Yükseltme yönergeleri, ilk kez yükleme için olanlarla benzerdir. Şunları yapın:

1. Azure portal oturum açın ve istediğiniz aboneliği ve Azure kiracınızı seçin.
1. Azure portal üst gezintiden Cloud Shell açın.

   ![Proje Farmtları](./media/prepare-for-deployment/navigation-bar-1.png)

1. Soldaki **ortam** açılan listesinde **Bash**' i seçin.
1. Gerekirse, *input. JSON* dosyanızda değişiklik yapıp Cloud Shell ' a yükleyin. Örneğin, e-posta adresinizi almak istediğiniz bildirim için güncelleştirebilirsiniz.
1. Aşağıdaki iki komutu Cloud Shell yazın veya yapıştırın. *İnput. JSON* dosya yolunu değiştirdiğinizden emin olun ve Enter tuşunu seçin.

    ```bash
    wget -O farmbeats-installer.sh https://aka.ms/AzureFarmbeatsInstallerScript && bash farmbeats-installer.sh /home/<username>/input.json
    ```

   Yükleyici, çalışma zamanında gerekli girdileri otomatik olarak ister.

1. Dağıtım için bir erişim belirteci girin. Oluşturulan kodu kopyalayın ve Azure kimlik bilgilerinizle [cihaz oturum açma sayfasında](https://microsoft.com/devicelogin) oturum açın.
1. Sentinel parolanızı girin.

   Yükleyici artık 20 dakikadan uzun sürebilen ve kaynakları oluşturmayı doğrular ve başlatır.

   Dağıtım başarıyla tamamlandıktan sonra aşağıdaki çıkış bağlantılarını alırsınız:
    - **Datahub URL 'si**: Azure Farmtts API 'lerini denemek için Swagger bağlantısı.
    - **Hızlandırıcı URL 'si**: Azure Farm, akıllı grup hızlandırıcıyı keşfetmeye yönelik kullanıcı arabirimi.
    - **Deployer günlük dosyası**: dağıtım sırasında oluşturulan günlük dosyası. Gerekirse, sorun giderme için kullanabilirsiniz.

> [!NOTE]
> Önceki değerleri daha sonra kullanmak üzere unutmayın.


## <a name="uninstall"></a>Kaldırma

Şu anda, yükleyiciyi kullanarak Azure Farmtörler 'in otomatik olarak kaldırılmasını desteklemiyoruz. Veri hub 'ını veya hızlandırıcıyı kaldırmak için Azure portal, bu bileşenlerin yüklendiği kaynak grubunu silin veya kaynakları el ile silin.

Örneğin, veri hub 'ı ve hızlandırıcıyı iki farklı kaynak grubunda dağıttıysanız, bu kaynak gruplarını aşağıdaki gibi silebilirsiniz:

1. Azure Portal’da oturum açın.
1. Sağ üst taraftaki hesabınızı seçin ve Azure Farmtts 'yi dağıtmak istediğiniz Azure AD kiracısına geçiş yapın.

   > [!NOTE]
   > Hızlandırıcının düzgün çalışması için veri hub 'ı gereklidir. Hızlandırıcı 'Yı kaldırmadan veri hub 'ını kaldırmayı önermiyoruz.

1. **Kaynak grupları**' nı seçin ve ardından silmek Istediğiniz veri hub 'ının veya hızlandırıcı kaynak grubunun adını girin.
1. Kaynak grubu adını seçin. Onaylamak için adı yeniden yazın ve ardından **Sil** ' i seçerek kaynak grubunu ve onun altındaki tüm kaynaklarını kaldırın.
   Alternatif olarak, önerdiğimiz bir yöntemi, her kaynağı el ile de silebilirsiniz.
1. Veri hub 'ını silmek veya kaldırmak için doğrudan Azure 'da kaynak grubuna gidin ve kaynak grubunu buradan silin.

## <a name="next-steps"></a>Sonraki adımlar

Azure Farmtts 'yi dağıtmayı öğrendiniz. Daha sonra, [Farmtts grupları oluşturmayı](manage-farms.md#create-farms)öğrenin.
