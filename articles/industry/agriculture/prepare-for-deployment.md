---
title: Azure Farmtts dağıtma
description: Farmtts 'nin nasıl dağıtılacağını açıklar
author: uhabiba04
ms.topic: article
ms.date: 11/04/2019
ms.author: v-umha
ms.openlocfilehash: c609285b727414b4849c9ef6654406a035005bb1
ms.sourcegitcommit: 018e3b40e212915ed7a77258ac2a8e3a660aaef8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2019
ms.locfileid: "73797731"
---
# <a name="deploy-farmbeats"></a>Farmtts dağıtma

Bu makalede, Azure Farmtts 'nin nasıl ayarlanacağı açıklanır.

Azure Farm, Azure bulutuna, telemetri veri toplamaya ve toplamaya yönelik sorunsuz sağlama ve cihaz bağlantısı sağlayan, verilere dayalı farkliğe yönelik sektöre özgü, genişletilebilir bir çözümdür. Azure Farmırts; kameralar, Drones, SOIL sensörleri ve buluttan cihazların yönetimi gibi çeşitli sensörler içerir. bu sayede, IoT-Ready (Nesnelerin İnterneti) cihazlar için Azure 'daki altyapı ve hizmetler, bir genişletilmiş Web ve mobil uygulamaya sağlanacak görselleştirme, uyarılar ve Öngörüler.

> [!NOTE]
> Azure Farmtts yalnızca genel bulut ortamlarında desteklenir. Bulut ortamı hakkında daha fazla bilgi için bkz. [Azure](https://azure.microsoft.com/overview/what-is-a-public-cloud/).

Azure Farmtts aşağıdaki iki bileşene sahiptir:

- **Veri merkezi** -veri hub 'ı, mevcut veya yeni veri işlem hatlarından veri oluşturmanıza, depolamanıza, işlemenize ve Öngörüler sağlamanıza olanak tanıyan Azure Farmtts 'nin platform katmanıdır. Bu platform katmanı, tarım veri işlem hatlarınızı ve modellerini çalıştırmak ve oluşturmak için kullanışlıdır.

- **Hızlandırıcı** -Hızlandırıcı, önceden oluşturulmuş agricdefinitions modellerini kullanarak Azure farmtilerinin yeteneklerini göstermek üzere yerleşik bir uygulamaya sahip Azure farmtts 'nin çözüm katmanıdır. Bu çözüm katmanı, Grup sınırları oluşturmanıza ve grup sınırının bağlamı içindeki agricbir veri kaynağından Öngörüler çizmenize imkan tanır.

Azure Farmtts 'nin hızlı dağıtımı bir saatten kısa sürer. Veri merkezi ve hızlandırıcının maliyetleri, kullanıma göre farklılık gösterir.

## <a name="deployed-resources"></a>Dağıtılan kaynaklar

Azure Farmtts dağıtımı, aboneliğiniz dahilinde aşağıdaki listelenen kaynakları oluşturur:

|S.No  |Kaynak Adı  |Azure Farmtempts bileşeni  |
|---------|---------|---------|
|1  |       Azure Cosmos DB   |  Veri Merkezi       |
|2  |    Application Insights      |     Veri merkezi/Hızlandırıcı     |
|3  |Redis için Azure Önbelleği   |Veri Merkezi   |
|4  |       Azure Keykasası    |  Veri merkezi/Hızlandırıcı        |
|5  |    Zaman Serisi Öngörüleri       |     Veri Merkezi      |
|6 |      EventHub ad alanı    |  Veri Merkezi       |
|7  |    Azure Data Factory V2       |     Veri merkezi/Hızlandırıcı      |
|8  |Batch hesabı    |Veri Merkezi   |
|9  |       Depolama hesabı     |  Veri merkezi/Hızlandırıcı        |
|10  |    Mantıksal uygulama        |     Veri Merkezi      |
|11  |    API bağlantısı        |     Veri Merkezi      |
|12|      App Service      |  Veri merkezi/Hızlandırıcı       |
|13 |    App Service planı        |     Veri merkezi/Hızlandırıcı      |
|14 |Azure haritalar hesabı     |Hızlandır    |
|15 |       Zaman Serisi Öngörüleri      |  Veri Merkezi     |

Azure Farmırts, Azure Marketi 'nden indirebilirsiniz. Doğrudan Azure portal erişebilirsiniz.  

## <a name="prepare"></a>Hazırlama

Azure Farmduytaları dağıtmak için aşağıdaki izinlere ihtiyacınız vardır:

- Kiracı: okuma erişimi
- Abonelik: Sahibe katkıda bulunan
- Kaynak grubu: sahip

## <a name="before-you-begin"></a>Başlamadan önce

Dağıtımı başlatmadan önce aşağıdakilerden emin olun:

- Sentinel hesabı
- Azure Active Directory (uygulama kaydı)
- Azure Farmno 'Lar

## <a name="create-a-sentinel-account"></a>Sentinel hesabı oluşturma    

Sentinel 'e sahip bir hesap, resmi web sitesinden cihazınızdan, Sentinel uydu canlandırın 'yi yüklemenize yardımcı olur. Ücretsiz bir hesap oluşturmak için aşağıdaki adımları izleyin:

1. https://scihub.copernicus.eu/dhus/#/self-registration kısmına gidin. Kayıt sayfasında, bir ad, soyadı, Kullanıcı adı, parola ve e-posta sağlayın.
2. **Etki alanı seç** açılan menüsünde, **Land**seçeneğini belirleyin.
3. **Kullanımı Seç** açılan menüsünde **eğitim**seçeneğini belirleyin.
4. **Ülke Seç** açılan menüsünde ülkeniz ' ı seçin.
5. Kayıt işlemini gerçekleştirmek için **Kaydet** ' i seçin.

Onay için kayıtlı e-posta adresine bir doğrulama e-postası gönderilir. Bağlantıyı seçin ve onaylayın. Kayıt işleminiz tamamlanmıştır.

## <a name="create-azure-ad-app-registration"></a>Azure AD uygulama kaydı oluşturma

Azure Farm, kimlik doğrulaması ve yetkilendirme için, aşağıdakileri içeren bir Azure Active Directory uygulaması kaydınız olmalıdır:

- Durum 1: yükleyici otomatik olarak oluşturulabilir (gerekli kiracı, abonelik ve kaynak grubu erişim izinlerine sahip olmanız şartıyla).
- 2\. durum: Azure Farmtts 'yi dağıtmaya başlamadan önce oluşturabilir ve yapılandırabilirsiniz (el ile adımlar gerekir).

**Durum 1**: yükleyici, istenen abonelik Içinde bir Azure Active Directory uygulama kaydı oluşturma haklarınızın olduğunu varsayar. Kaydolmak, portalda oturum açmak için **Azure Active directory** > **uygulama kaydı** > **Yeni kayıt**' a gidin.

Zaten bir aboneliğiniz varsa, doğrudan bir sonraki yordama taşıyabilirsiniz.

**Durum 2**: aboneliğiniz dahilinde BIR Azure AD uygulama kaydı oluşturmak ve yapılandırmak için yeterli haklara sahip olmadığınız durumlarda bu yöntem tercih edilen adımdır. Yöneticinizin, BT yöneticisinin Azure portal Azure AD uygulama kaydını otomatik olarak oluşturup yapılandırmasına yardımcı olacak [özel betiği](https://aka.ms/FarmBeatsAADScript)kullanmasını isteyin. PowerShell ortamını kullanarak bu özel betiği çalıştırmanın bir çıkışı olarak BT yöneticisinin sizinle bir Azure Active Directory Uygulama Istemci KIMLIĞI ve parola gizli anahtarı paylaşması gerekir. Bu değerleri bir yere unutmayın.

Azure AD uygulaması kayıt betiğini çalıştırmak için aşağıdaki adımları kullanın:

1. Kayıt [betiğini](https://aka.ms/FarmBeatsAADScript)alın.
2. Azure portal oturum açın ve aboneliğinizi ve AD kiracınızı seçin.
3. Cloud Shell’i Azure portalında en üst gezinti bölmesinden başlatın.

    ![Proje grubu ları](./media/prepare-for-deployment/navigation-bar-1.png)


4. İlk kullanıcıdan, bir depolama hesabı ve Microsoft Azure dosya paylaşımının oluşturulması için bir abonelik seçmesi istenir. **Depolama oluştur**'u seçin.
5. Kullanıcılardan ilk kez tercih edilen kabuk deneyimi (bash veya PowerShell seçimi) istenir. PowerShell ' i seçin.
6. Cloud Shell, komut dosyasını çalıştırmak için aşağıdaki komutları girin.

    ```powershell
    ./create_aad_script.ps1
    ```
7. Azure Farmıd 'leri dağıtan kişilerle paylaşmak için Azure AD uygulama KIMLIĞI ve istemci gizli anahtarı ' nı bir yere göz önünde yapın.

## <a name="create-azure-farmbeats-offer-on-marketplace"></a>Market 'te Azure Farmırts teklifi oluşturma

Market 'te bir Azure Farmırts teklifi oluşturmak için aşağıdaki adımları kullanın:

1. **Azure Portal** oturum açın ve sağ üst köşedeki hesabınızı seçin ve Microsoft Azure Farmtts dağıtmak ISTEDIĞINIZ Azure AD kiracısına geçiş yapın.
2. **Arama/Market** ' i veya **kaynak oluştur**' u seçin. Teklif ayrıntılarını görüntülemek için **Farmtempts** yazın. Sonraki adımlara geçmeden önce bu sayfada aka.ms köprüleri aracılığıyla kullanılabilen kılavuzlarını indirin.
3. **Oluştur** ' u seçin ve aşağıdaki bilgileri girin:

   - Abonelik adını girin.
   - Mevcut bir kaynak grubu adı girin (yalnızca boş kaynak grubu) veya Azure Farmtts dağıtmak için yeni bir kaynak grubu oluşturun. Daha sonraki bir aşamada input. JSON dosyası içine girmek için bu kaynak grubunu unutmayın.
   - Azure Farmtts 'yi yüklemek istediğiniz bölgeyi girin. Şu anda şu bölgelerde Farmtempts yüklü olabilir: Orta ABD, Batı Avrupa, Doğu ABD 2, Kuzey Avrupa, Batı ABD, Güneydoğu Asya, Doğu ABD, Avustralya Doğu, Batı ABD 2.
4. Kullanım koşulları sayfasına yönlendiren **Tamam**' ı seçin. Standart Market şartlarını gözden geçirin veya kullanım koşullarını gözden geçirmek için köprüyü seçin.
5. **Kapat**' ı, sonra "kabul ediyorum" onay kutusunu seçin ve ardından **Oluştur**' u seçin.
6. Azure Farmtts 'nin Son Kullanıcı Lisans Sözleşmesi 'ni (EULA) Market 'te başarıyla kapattınız. Dağıtıma devam etmek için bu kılavuzdaki sonraki adımları izleyin.

### <a name="prepare-inputjson-file"></a>Input. json dosyasını hazırlama

Bu dosya Azure Cloud Shell ve bu dosya içinde değerleri, dağıtım sırasında istenmeden karşıya yüklemeden önce belirtilen parametrelere ve bu nedenle bir süre kaydedilmeden, bu dosya için girdi dosyanız.  

> [!NOTE]
> Bu dosya Azure Cloud Shell için değerleri girdi.  Zaman kazanmak için, dağıtım sırasında bu dosyaya eklediğiniz parametreler istenmez. Eksik parametreler istenir.

Dosyayı hazırlamadan önce parametreleri gözden geçirin.

|Komut | Açıklama|
|--- | ---|
|isteyin  | Azure Farmtts bileşenlerinin birini ya da her ikisini de indirmek için bir seçenek sağlar. Hangi bileşenlerin indirileceği belirtir. Yalnızca veri merkezini yüklemek için "onlydatabhub" kullanın. Veri merkezini ve hızlandırıcıyı yüklemek için "both" kullanın.|
|SubscriptionID  | Farmtts yükleme aboneliğini belirtir|
|"datahubResourceGroup"  | Veri merkezi kaynakları için kaynak grubu adı.|
|"datahubLocation" | Veri merkezi kaynaklarını depolamak istediğiniz konum.|
|"Ivatorwebsitename"  |Veri merkezinizi adlandırmak için benzersiz URL öneki
Swagger Web sitesi. Varsayılan değer veri merkezi kaynak grubu adıdır. Varsayılan değere devam etmek için ENTER tuşuna basın.|
|"Ivatorresourcegroup"  | Veri merkezi kaynakları için kaynak grubu adı. |
|"Ivatorlocation"  | Veri merkezi kaynaklarının depolanacağı konum
"Ivatorwebsitename"  | Hızlandırıcı Web sitenizi adlandırmak için benzersiz URL ön eki. Varsayılan değer hızlandırıcıdır. Varsayılan değere devam etmek için ENTER tuşuna basın.|
|''sentinelUsername'' | oturum açmak için Kullanıcı adı: https://scihub.copernicus.eu/dhus/#/self-registration.|
|"sentinelPassword"  | Oturum açmak için parola: https://scihub.copernicus.eu/dhus/#/self-registration.|
|"Farmbeatsappıd"  | Ekip Azure Farmtts tarafından paylaşılacak değerler.  |
|"farmbeatsPassword"  | Ekip Azure Farmtts tarafından paylaşılacak değerler.|
|"Notificationemapostaadresi"  | Veri merkezi içinde yapılandırdığınız tüm uyarılar için bildirimleri alacak e-posta adresi.|
|"upda" Aadappclientıd ""  |[**Isteğe bağlı**] Yalnızca Azure AD uygulaması zaten varsa Input. JSON içine dahil edilecek parametre.  -True/false. İlk kez yükleme için false ve yükseltme senaryosu için true.|
|"Aadappclientıd"  | [**Isteğe bağlı**] Yalnızca Azure AD uygulaması zaten varsa Input. JSON içine dahil edilecek parametre.  |
|"aadAppClientSecret"   | [**Isteğe bağlı**] Yalnızca Azure AD uygulaması zaten varsa Input. JSON içine dahil edilecek parametre.|


Örnek JSON girişi:

```json
{  
   "sku":"both", 
   "subscriptionId": "da9xxxec-dxxf-4xxc-xxx21-xxx3ee7xxxxx", 
   "datahubResourceGroup": "dummy-test-dh1", 
   "datahubLocation": "westus2", 
   "datahubWebsiteName": "dummy-test-dh1", 
   "acceleratorResourceGroup": "dummy-test-acc1", 
   "acceleratorLocation": "westus2", 
   "acceleratorWebsiteName": "dummy-test-acc1", 
   "sentinelUsername": "dummy-dev", 
   "farmbeatsAppId": "c3cb3xxx-27xx-4xxb-8xx6-3xxx2xxdxxx5c", 
   "notificationEmailAddress": "dummy@microsoft.com", 
   "updateIfExists": true
}
```
## <a name="deploy-within-cloud-shell-browser-based-command-line"></a>Cloud Shell tarayıcı tabanlı komut satırı içinde dağıtın

Market iş akışının bir parçası olarak, bir kaynak grubu oluşturdunuz ve Son Kullanıcı Lisans sözleşmesini imzalamışsanız, bu, gerçek dağıtımın bir parçası olarak bir kez daha incelenebilir. Dağıtım, Bash ortamı kullanılarak Azure Cloud Shell (tarayıcı tabanlı komut satırı) aracılığıyla yapılır.  

> [!NOTE]
> Etkin olmayan Cloud Shell oturumlarının süresi 20 dakika sonra dolacak. Bu süre içinde dağıtımı tamamlamayı deneyin.

1. **Azure** portalında oturum açın ve istediğiniz ABONELIĞI ve ad kiracıyı seçin.
2. **Azure** portalının üst gezintiden **Cloud Shell** başlatın.

   ![Proje grubu ları](./media/prepare-for-deployment/navigation-bar-1.png)

3. Depolama hesabı ve Microsoft Azure dosyaları paylaşma oluşturmak için bir abonelik seçin.
4. **Depolama oluştur**'u seçin.
5. Kabuk penceresinin (Bash) sol tarafındaki ortam açılan penceresini seçin.

   ![Proje grubu ları](./media/prepare-for-deployment/bash-1-1.png)

### <a name="deployment-scenario-1"></a>Dağıtım senaryosu 1

Yükleyici, Azure AD 'yi (AD kiracı okuma izinleriniz vardır) oluşturur.  

1. [İnput. JSON](https://aka.ms/PPInputJsonTemplate) şablonunu indirin. İnput. JSON dosyasına Azure uygulama istemci KIMLIĞINI ve parolasını ekleyin ve kaydedin.
2. İndirilen dosyayı bir not defteri içinde açın ve değerleri girerek dosyayı doldurun.

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
    "updateIfExists":true
    }

    ```

    > [!NOTE]
    > Bu adımı atlayabilirsiniz ve girişler çalışma zamanında istenir.

3. Dosyayı kaydedin ve yolu (yerel bilgisayarınızda) bir yere göz önünde yapın.  
4. Azure Cloud Shell gidin ve başarılı kimlik doğrulamasından sonra karşıya yükle ' yi seçin (aşağıdaki görüntüde vurgulanan simgeye bakın) ve input. json dosyasını Cloud Shell depolama alanına yükleyin. Azure AD parametresini JSON içinde iletmeniz gerekir, çünkü yükleyici Azure AD uygulama kaydını sizin için oluşturup yapılandırır.

   ![Proje grubu ları](./media/prepare-for-deployment/bash-2-1.png)

5. "Dağıtım komutu" nu Cloud Shell yazın veya yapıştırın. Girişin yolunu değiştirdiğinizden emin olun. JSON dosyası ve ENTER tuşuna basın.  

    ```
    wget -N -O farmbeats-installer.sh https://aka.ms/FB_1.2.0 && bash farmbeats-installer.sh> /home/dummyuser/input.json

    ```
   Komut dosyası tüm bağımlılıkları otomatik olarak indirir ve dağıtıcı oluşturur. Ardından, Azure Farmınts Son Kullanıcı Lisans Sözleşmesi 'ni (EULA) kabul etmeniz istenir.

   - Kabul ediyorsanız ' Y ' yazın ve sonraki adıma ilerlemeniz gerekir.
   - Koşulları kabul etmiyorsanız ve dağıtım sonlanacak olursa ' N ' girin.

   Ardından dağıtım için bir erişim belirteci girmeniz istenir. Oluşturulan kodu kopyalayın ve Azure kimlik bilgilerinizle https://microsoft.com/devicelogin oturum açın.

   > [!NOTE]
   > Kodun süresi 60 dakika sonra dolar. Süresi sona erdiğinde, dağıtım komutunu yeniden yazarak yeniden başlatabilirsiniz.

6. Sonraki istem üzerinde Sentinel hesap parolası girin.
7. Yükleyici şimdi doğrular ve dağıtımı başlatır. Bu işlem 20 dakika sürebilir.
8. Dağıtım başarılı olduktan sonra aşağıdaki çıkış bağlantılarını alacaksınız:
    - **Veri merkezi URL 'si**: Azure Farmtts API 'lerini denemek için Swagger bağlantısı.
    - **HıZLANDıRıCı URL**: Azure Farmtts akıllı grup hızlandırıcıyı keşfetmeye yönelik kullanıcı arabirimi.
    - **Deployer günlük dosyası**-dağıtım sırasında oluşturulan günlük dosyası. Sorun giderme için kullanılabilir.

    - Kabul ediyorsanız ' Y ' yazın ve sonraki adımla devam edersiniz.
    - Koşulları kabul etmiyorsanız ve dağıtım sonlanacak olursa ' N ' girin.

   Ardından dağıtım için bir erişim belirteci girmeniz istenir. Oluşturulan kodu kopyalayın ve Azure kimlik bilgilerinizle https://microsoft.com/devicelogin oturum açın.

   > [!NOTE]
    > Bunlara dikkat edin ve dağıtım günlük dosyası yolunu daha sonra kullanmak üzere kullanışlı tutun.


### <a name="deployment-scenario-2"></a>Dağıtım senaryosu 2

Mevcut Azure Active Directory uygulama kaydı dağıtmak için kullanılır.

1. [JSON girişi](https://aka.ms/PPInputJsonTemplate) ' ni indirin input. json ' da Azure uygulama istemci kimliğini ve parolasını ekleyin ve kaydedin.

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

     Kalan adımları izleyin:

2. İnput. JSON dosyanızın yolunu (yerel bilgisayarınızda) bir yere unutmayın.
3. Azure Cloud Shell yeniden bir kez daha gidin ve başarıyla kimliğiniz doğrulandıktan sonra karşıya yükle düğmesini (aşağıdaki görüntüde vurgulanan simgeye bakın) seçin ve input. json dosyasını Cloud Shell depolama alanına yükleyin.

    ![Proje grubu ları](./media/prepare-for-deployment/bash-2-1.png)

4. *Dağıtım komutunu* Cloud Shell yazın veya yapıştırın. Girişin yolunu değiştirdiğinizden emin olun. JSON dosyası ve ENTER tuşuna basın.

    ```
    wget -N -O farmbeats-installer.sh https://aka.ms/FB_1.2.0 && bash farmbeats-installer.sh> /home/dummyuser/input.json

    ```
   Kalan adımları izleyin:

5. Komut dosyası tüm bağımlılıkları otomatik olarak indirir ve dağıtıcı oluşturur.
6. Azure Farmınts Son Kullanıcı Lisans Sözleşmesi 'ni (EULA) okuyup kabul etmeniz istenir.

   - Kabul ediyorsanız ' Y ' yazın ve sonraki adımla devam edersiniz.
   - Koşulları kabul etmiyorsanız ve dağıtım sonlanacak olursa ' N ' girin.

7. Dağıtım için bir erişim belirteci girmeniz istenir. Oluşturulan kodu kopyalayın ve Azure kimlik bilgilerinizle https://microsoft.com/devicelogin oturum açın.
8. Yükleyici artık 20 dakika sürebilecek kaynakları doğrular ve oluşturmaya başlar. Bu süre boyunca Cloud Shell oturumun etkin kalmasını sağlayın.
9. Dağıtım başarılı olduktan sonra aşağıdaki çıkış bağlantılarını alacaksınız:
   - **Veri merkezi URL 'si**: Farmtts API 'lerini denemek için Swagger bağlantısı.
   - **HıZLANDıRıCı URL**: Farmtts akıllı grup hızlandırıcıyı keşfetmeye yönelik kullanıcı arabirimi.
   - **Deployer günlük dosyası**: dağıtım sırasında günlükleri kaydeder ve sorun giderme için kullanılabilir.

Herhangi bir sorunla karşılaşırsanız [sorun giderme](troubleshoot-project-farmbeats.md)' yi gözden geçirin.


## <a name="validate-deployment"></a>Dağıtımı doğrula

### <a name="data-hub"></a>Veri Merkezi

Veri merkezi yüklemesi tamamlandıktan sonra, Şu biçimdeki Swagger arabirimi aracılığıyla Azure Farmınts API 'Lerine erişmek için URL 'YI alacaksınız: https://\<yourdatahub-website-name >. azurewebsites. net

1. Swagger aracılığıyla oturum açmak için URL 'YI kopyalayıp tarayıcıya yapıştırın.
2. Azure portal kimlik bilgileriyle oturum açın.
3. Sanity testi (Isteğe bağlı)

     - Başarılı bir dağıtıma çıkış olarak aldığınız veri merkezi bağlantısını kullanarak Swagger portalında başarıyla oturum açabiliyor.
     - Genişletilmiş türler API al-"deneyin/kapat" seçeneğini belirleyin
     - Sunucu yanıt kodu 200 ' i almalısınız ve 403 "yetkisiz Kullanıcı" gibi bir özel durum değil.

### <a name="accelerator"></a>Hızlandır

Hızlandırıcı yüklemesi tamamlandıktan sonra, Şu biçimdeki Farmtts Kullanıcı arabirimine erişmek için URL 'YI alacaksınız: https://\<Hızlandırıcısı-Web sitesi-adı >. azurewebsites. net

1. Hızlandırıcının oturumunu açmak için URL 'YI kopyalayıp tarayıcıya yapıştırın.
2. Azure portal kimlik bilgileriyle oturum açın.
3. İsteğe bağlı bir sağlamlık testi çalıştırın.

    - Başarılı bir dağıtıma çıkış olarak aldığınız Hızlandırıcı bağlantısını kullanarak Hızlandırıcı portalında başarıyla oturum açabiliyor olup olmadığınızı denetleyin.
    - **Grup Oluştur**' u seçin.
    - "?" Simgesinin altında, **kullanmaya başlama** düğmesini kullanarak Farmtts kılavuzlarını açın.

## <a name="upgrade"></a>Yükseltme

Yükseltme adımları, ilk kez yüklemeye benzerdir. Şu adımları uygulayın:

1. Azure portal için oturum açın ve istediğiniz abonelik ve AD kiracınızı seçin.
2. Cloud Shell’i Azure portalında en üst gezinti bölmesinden başlatın.

   ![Proje grubu ları](./media/prepare-for-deployment/navigation-bar-1.png)

3. Bir depolama hesabı ve bir Azure dosyaları dosya paylaşma oluşturmak için bir abonelik seçin.
4. **Depolama oluştur**'u seçin.
5. Kabuğun solundaki (Bash) açılır listesinden ortamı seçin.
6. Yalnızca gerektiğinde input. JSON dosyanızda değişiklik yapın ve Azure Cloud Shell yükleyin. Örneğin, e-posta adresinizi almak istediğiniz bildirim için güncelleştirebilirsiniz.
7. İnput. JSON dosya yolunu (yerel bilgisayarınızda) unutmayın.
8. Azure Cloud Shell gidin. Kimliği başarıyla doğrulandıktan sonra karşıya yükle düğmesini seçin (aşağıdaki görüntüde vurgulanan simgeye bakın) ve input. json dosyasını Cloud Shell depoya yükleyin.

   ![Proje grubu ları](./media/prepare-for-deployment/bash-2-1.png)

9. **Dağıtım komutunu** Cloud Shell yazın veya yapıştırın. Girişin yolunu değiştirdiğinizden emin olun. JSON dosyası ve ENTER tuşuna basın.

    ```
    wget -N -O farmbeats-installer.sh https://aka.ms/FB_1.2.0 && bash farmbeats-installer.sh> /home/dummyuser/input.json

    ```
    Kalan adımları izleyin:

10. Yükleyici, çalışma zamanında gerekli girişleri otomatik olarak sorar:

    - Dağıtım için bir erişim belirteci girin. Oluşturulan kodu kopyalayın ve Azure kimlik bilgilerinizle https://microsoft.com/devicelogin oturum açın.

     > [!NOTE]
     > Kodun süresi 60 dakika sonra dolar. Süresi sona erdiğinde, dağıtım komutunu yeniden yazarak yeniden başlatabilirsiniz.

     - Sentinel parolası

11. Yükleyici artık 20 dakikadan uzun sürebilen ve kaynakları oluşturmayı doğrular ve başlatır.
12. Dağıtım başarılı olduktan sonra aşağıdaki çıkış bağlantılarını alacaksınız:

    - **Veri merkezi URL 'si**: Farmtts API 'lerini denemek için Swagger bağlantısı.
    - **HıZLANDıRıCı URL**: Farmtts akıllı grup hızlandırıcıyı keşfetmeye yönelik kullanıcı arabirimi.
    - **Deployer günlüğü dosyası**: dağıtım sırasında günlükleri kaydeder. Sorun giderme için kullanılabilir.

    > [!NOTE]
    > Yukarıdaki bağlantıları aklınızda bulundurun ve ileride kullanılmak üzere dağıtım günlük dosyası yolunu koruyun.

## <a name="uninstall"></a>Kaldırma

Şu anda, yükleyiciyi kullanarak Farmof 'ları otomatik olarak kaldırmayı desteklemiyoruz. Veri hub 'ını veya hızlandırıcıyı kaldırmak için Azure portal, bu bileşenlerin yüklendiği kaynak grubunu silin veya kaynakları el ile silin.

Örneğin, veri hub 'ı ve hızlandırıcıyı iki farklı kaynak grubunda dağıttıysanız, bu kaynak gruplarını aşağıdaki şekilde silersiniz:

1. Azure portal oturum açın.
2. Sağ üst köşede hesabınızı seçin ve Microsoft Farmtts 'yi dağıtmak istediğiniz Azure AD kiracısına geçiş yapın.

   > [!NOTE]
   > Hızlandırıcının düzgün çalışması için veri hub 'ı gerekir. Hızlandırıcı 'Yı kaldırmadan veri merkezini kaldırmayı önermiyoruz.

3. Kaynak grupları ' nı seçin ve silmek istediğiniz veri hub 'ının veya hızlandırıcı kaynak grubunun adını yazın.
4. Kaynak grubu adını seçin. Yeniden denetlemek için adı bir kez daha yazın ve ardından Sil ' e tıklayarak kaynak grubunu ve onun altındaki tüm kaynaklarını kaldırın.
5. Alternatif olarak, her kaynağı el ile silebilirsiniz, bu önerilmez.
7. Veri merkezini silmek/kaldırmak için doğrudan Azure 'da kaynak grubuna gidin ve kaynak grubunu buradan silin.

## <a name="next-steps"></a>Sonraki adımlar

Azure Farmtts 'yi dağıttıysanız. Şimdi [gruplar oluşturmayı](manage-farms.md#create-farms)öğrenin.
