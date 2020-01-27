---
title: Azure FarmBeats'i yükleme
description: Bu makalede, Azure aboneliğinizdeki Azure Farmtts 'nin nasıl yükleneceği açıklanır.
author: usha-rathnavel
ms.topic: article
ms.date: 12/11/2019
ms.author: usrathna
ms.openlocfilehash: d1a6bdfb38431e18eb305b223ce8ee2467804052
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75482458"
---
# <a name="install-azure-farmbeats"></a>Azure FarmBeats'i yükleme

Bu makalede, Azure aboneliğinizdeki Azure Farmtts 'nin nasıl yükleneceği açıklanır.

Azure Farm, Azure Marketi 'nde bulunan işletmeden işletmeye yönelik bir tekliftir. Sağlayıcılar genelinde verilerin toplanmasının yanı sıra eyleme dönüştürülebilir içgörüler oluşturulmasına olanak tanıyor. Azure Farm, bu, fkullanılan veri kümelerine göre yapay zeka (AI) veya makine öğrenimi (ML) modelleri oluşturmanızı sağlayarak bunu yapar. Azure Farmtts 'nin iki ana bileşeni şunlardır:

- **Datahub**: farklı sağlayıcılardaki çeşitli agriclaral veri kümelerinin toplama, normalleştirme ve şeritleştirmeyi sağlayan bir API katmanı.

- **Hızlandırıcı**: veri hub 'ının üzerine inşa edilen örnek bir Web uygulaması. Hızlı bir şekilde model geliştirme ve görselleştirmenizi başlatır. Hızlandırıcı, alınan algılayıcı verilerinin görselleştirmeyi ve model çıkışının harita olarak görselleştirmesini göstermek için Azure Farmınts API 'Lerini kullanır.

## <a name="before-you-start"></a>Başlamadan önce
### <a name="components-installed"></a>Yüklenen bileşenler

Azure Farmtts 'yi yüklediğinizde, Azure aboneliğinizde aşağıdaki kaynaklar sağlanır:

| Azure kaynakları yüklendi  | Azure Farmtempts bileşeni  |
|---------|---------|
| Application Insights   |      Datahub & Hızlandırıcısı      |
| App Service     |     Datahub & Hızlandırıcısı     |
| App Service Planı   | Datahub & Hızlandırıcısı  |
| API bağlantısı    |  Veri hub 'ı       |
| Redis için Azure Cache       | Veri hub 'ı      |
| Azure Cosmos DB   |  Veri hub 'ı       |
| Azure Data Factory V2       |     Datahub & Hızlandırıcısı      |
| Azure Batch hesabı    | Veri hub 'ı   |
| Azure Key Vault |  Datahub & Hızlandırıcısı        |
| Azure haritalar hesabı       |     Hızlandır    |
| Olay Hub 'ı ad alanı    |     Veri hub 'ı      |
| Mantıksal Uygulama      |  Veri hub 'ı       |
| Depolama Hesabı      |     Datahub & Hızlandırıcısı      |
| Zaman Serisi İçgörüleri     |    Veri hub 'ı    |

### <a name="costs-incurred"></a>Tahakkuk eden maliyetler

Azure Farmtts 'nin maliyeti, temel alınan Azure hizmetlerinin maliyetinin toplamıdır. Azure hizmetleri fiyatlandırma bilgileri, [Fiyatlandırma hesaplayıcısı](https://azure.microsoft.com/pricing/calculator)kullanılarak hesaplanabilir. Toplam yüklemenin gerçek maliyeti, kullanıma bağlı olarak değişir. İki bileşen için sabit durum maliyeti:

* Veri merkezi-günde $10 ' den az
* Hızlandırıcı-günde $2 'den az

### <a name="regions-supported"></a>Desteklenen bölgeler

Şu anda, şu bölgelerde genel bulut ortamlarında Azure Farmtempts desteklenir:
* Avustralya Doğu
* Orta ABD
* Doğu ABD
* Doğu ABD 2
* Batı ABD
* Batı ABD 2
* Kuzey Avrupa
* Batı Avrupa
* Güneydoğu Asya

### <a name="time-taken"></a>Geçen süre

Hazırlama ve yükleme dahil olmak üzere tüm Azure Farmtts kurulumu bir saatten kısa sürer.

## <a name="prerequisites"></a>Ön koşullar    

Azure Farmtts 'nin gerçek yüklemesine başlamadan önce aşağıdaki adımları gerçekleştirmeniz gerekir:

### <a name="create-sentinel-account"></a>Sentinel hesabı oluştur
Azure Farmtts kurulumu, Avrupa alan ajanlarından, grubunuz için [Sentinel-2](https://scihub.copernicus.eu/) uydu görevi 'nden ücretsiz uydu görüntüsü almanızı sağlar. Bu kurulumu yapılandırmak için bir Sentinel hesabı gerekir.

Sentinel ile ücretsiz bir hesap oluşturmak için aşağıdaki adımları izleyin:

1. Resmi [kaydolma](https://scihub.copernicus.eu/dhus/#/self-registration) sayfasına gidin.
2. Gerekli ayrıntıları (ad, soyadı, Kullanıcı adı, parola ve e-posta KIMLIĞI) sağlayın ve formu doldurun.
3. Kayıtlı e-posta KIMLIĞINE bir doğrulama bağlantısı gönderilir. E-postada sunulan bağlantıyı seçin ve doğrulamayı doldurun.

Doğrulamayı tamamladıktan sonra kayıt işleminiz tamamlanmıştır. **Sentinel Kullanıcı adı** ve **Sentinel parolanızı**bir yere göz önünde alın.

### <a name="decide-subscription-and-region"></a>Aboneliğe ve bölgeye karar verme

Azure abonelik KIMLIĞI ve Azure Farmtts 'yi yüklemek istediğiniz bölge gereklidir. [Desteklenen bölgeler](#regions-supported) bölümünde listelenen bölgelerden birini seçin.

**Azure ABONELIK kimliği** ve **Azure bölgesi**' ni bir yere unutmayın.

### <a name="verify-permissions"></a>Izinleri doğrulama

Azure 'a yönelik yeterli ayrıcalıklara ve izinlere sahip olup olmadığını doğrulamanız gerekir.

[Rol tabanlı erişim denetimi](https://docs.microsoft.com/azure/role-based-access-control/check-access) yönergelerini izleyerek Azure Portal erişim izinlerinizi doğrulayabilirsiniz

Azure Farmtts 'yi yüklemek için aşağıdaki izinlere sahip olmanız gerekir:
- Kiracı-okuma erişimi
- Abonelik-katkıda bulunan veya sahip
- Kaynak grubu-sahip.

Ayrıca, Azure Farmtts Azure Active Directory uygulama kayıtları gerektirir. Gerekli Azure AD kurulumunu tamamlamaya yönelik iki yol vardır:

**Durum 1**: yüklemekte olduğunuz Azure kiracısında **yazma** izinleriniz var. Bu durum, yüklemesi sırasında AAD uygulama kaydını dinamik olarak oluşturmak için gerekli izinlere sahip olduğunuz anlamına gelir.

Doğrudan [Market kaydolma bölümünü tamamlamaya](#complete-azure-marketplace-sign-up) devam edebilirsiniz.


**Durum 2**: Azure kiracısında **yazma** izinleriniz yok. Bu durum, şirketinizin Azure aboneliğine Azure Farmtları yüklemeye çalışırken ve **yazma** erişiminizin yalnızca sahip olduğunuz kaynak grubuyla sınırlandırıldığı durumlarda yaygındır.
Bu durumda, Azure portal Azure AD uygulama kaydını otomatik olarak oluşturup tamamlamaya yönelik aşağıdaki adımları izlemek için BT yöneticinize isteyin.

1. [AAD uygulama Oluşturucu betiğini](https://aka.ms/FarmBeatsAADScript) indirip yerel makinenize ayıklayın.
2. Azure portal oturum açın ve aboneliğinizi ve Azure AD kiracınızı seçin.
3. Azure portal en üstündeki araç çubuğundan Cloud Shell başlatın.

    ![Proje Farmtları](./media/install-azure-farmbeats/navigation-bar-1.png)

4. Tercih edilen kabuk deneyimi olarak PowerShell ' i seçin. Kullanıcılardan bir abonelik seçmesi ve bir depolama hesabı oluşturması istenir. Kurulumu belirtildiği gibi doldurun.
5. Betiği (adım 1 ' den Cloud Shell) karşıya yükleyin ve karşıya yüklenen dosyanın konumunu aklınızda edin.

    > [!NOTE]
    > Varsayılan olarak, dosya giriş dizininize yüklenir.

6. ' CD ' komutunu kullanarak giriş dizinine gidin ve aşağıdaki betiği çalıştırın:

      ```azurepowershell-interactive
            ./create_aad_script.ps1
      ```
7. **Datahub Web sitesi** adını ve **Hızlandırıcı Web sitesi** adını girin. Betiğin çıkışını bir yere ve Azure Farmtts 'yi yükleyen kişiyle paylaştığından emin olun.

BT yöneticiniz gerekli ayrıntıları size sunduktan sonra **AAD ISTEMCI kimliği, AAD Istemci parolası, Datahub Web sitesi adı & Hızlandırıcı Web**sitesi adı ' nı bir yere unutmayın.

   > [!NOTE]
   > Büyük/küçük harf 2 ' den yönergeler için, AAD istemci KIMLIĞINI & AAD istemci gizliliğini [Parametreler dosyasında](#prepare-parameters-file) ayrı parametreler olarak eklemeyi unutmayın

Şimdi bir sonraki bölüme ilerlemek için gereken tüm bilgilere sahipsiniz.

### <a name="complete-azure-marketplace-sign-up"></a>Tam Azure Market kaydı

Cloud Shell tabanlı yükleme işlemini başlatabilmeniz için önce Azure Marketi 'nde Azure Farmtts teklifini doldurmanız gerekir. Kaydolma işleminin tamamlanabilmesi için aşağıdaki adımları izleyin:

1.  Azure Portal’da oturum açın. Sağ üst köşede hesabınızı seçin ve Azure Farmtts 'yi yüklemek istediğiniz Azure AD kiracısına geçiş yapın.

2.  Portalda Azure Marketi ' ne gidin ve Market 'te **Azure Farmırts** 'yi arayın

3.  Azure Farmtts 'ye genel bakış içeren yeni bir pencere görüntülenir. **Oluştur**' u seçin.

4.  Yeni bir pencere görüntülenir. Azure Farmtts 'yi yüklemek istediğiniz doğru aboneliği, kaynak grubunu ve konumu seçerek kaydolma işlemini tamamlayabilirsiniz.

5.  Girilen Ayrıntılar doğrulandıktan sonra **Tamam**' ı seçin. Kullanım koşulları sayfası görüntülenir. Koşulları gözden geçirin ve **Oluştur** ' u seçerek kaydolma işlemini tamamlayabilirsiniz.

Bu adım, Azure Marketi 'nde kaydolma işlemini tamamlar. Artık parametreler dosya hazırlanmasını başlatmaya hazırsınız.

### <a name="prepare-parameters-file"></a>Parametre dosyasını hazırla
Önkoşullar aşamasının son adımı, Cloud Shell yüklemesi sırasında giriş olarak kullanılacak bir JSON dosyası oluşturuyor. JSON dosyasındaki parametrelerin uygun değerlerle değiştirilmeleri gerekir.

Örnek bir JSON dosyası aşağıda verilmiştir. Örneği indirin ve gerekli ayrıntıları güncelleştirin.

```json
{  
    "sku":"both",
    "subscriptionId":"da9xxxec-dxxf-4xxc-xxx21-xxx3ee7xxxxx",
    "datahubResourceGroup":"dummy-test-dh1",
    "location":"westus2",
    "datahubWebsiteName":"dummy-test-dh1",
    "acceleratorResourceGroup":"dummy-test-acc1",
    "acceleratorWebsiteName":"dummy-test-acc1",
    "sentinelUsername":"dummy-dev",
    "notificationEmailAddress":"dummy@yourorg.com",
    "updateIfExists":true
}
```

Parametrelerin her birinde daha fazlasını anlamak için aşağıdaki parametre tablosuna başvurabilirsiniz.

| Parametre | Değer|
|--- | ---|
|sku  | Kullanıcıya hem veri hub 'ını hem de hızlandırıcıyı veya yalnızca veri merkezini yüklemek için bir seçenek sağlar. Yalnızca Datahub 'ı yüklemek için "Datahub" kullanın. Datahub ve Hızlandırıcı 'yı yüklemek için "her ikisi de" kullanın|
|subscriptionId | Azure Farmtts 'yi yüklemek için Azure aboneliğini belirtir|
|datahubResourceGroup| Veri hub kaynaklarınızın kaynak grubu adını belirtir. Azure Marketi 'nde oluşturduğunuz kaynak grubu adını buradan girin|
|location |Azure Farmtts 'yi yüklemek istediğiniz konum/Azure bölgesi|
|datahubWebsiteName  | Datahub Web uygulamanız için benzersiz URL öneki |
|Ivatorresourcegroup  | Hızlandırıcı kaynaklarınız için kaynak grubu adını belirtir|
|Ivatorwebsitename |Hızlandırıcı Web uygulamanız için benzersiz URL öneki|
|sentinelUsername | Sentinel uydu Imagery 'yi almak için Kullanıcı adı|
|Notificationemapostaadresi  | Veri merkeziniz içinde yapılandırdığınız tüm uyarılar için bildirimleri alacak e-posta adresi|
|updateIfExists| Seçim Yalnızca var olan bir Azure Farmtts örneğini yükseltmek istiyorsanız, parametreler dosyasına dahil edilecek bir parametre. Yükseltme için, kaynak grubu adları ve konumları gibi diğer ayrıntıların aynı olması gerekir|
|Aadappclientıd | Seçim Yalnızca önceden oluşturulmuş bir AAD uygulaması kullanıyorsanız, parametreler dosyasına dahil edilecek bir parametre. Daha ayrıntılı bilgi için bkz. [Izinleri doğrula](#verify-permissions) bölümünde durum 2 ' ye bakın |
|aadAppClientSecret  | Seçim Seçim Yalnızca önceden oluşturulmuş bir AAD uygulaması kullanıyorsanız, parametreler dosyasına dahil edilecek bir parametre. Daha ayrıntılı bilgi için bkz. [Izinleri doğrula](#verify-permissions) bölümünde durum 2 ' ye bakın|

Yukarıdaki tabloya ve örnek JSON dosyasına bağlı olarak, parametrelerinizi JSON dosyanızı oluşturun ve yerel bilgisayarınıza kaydedin.

## <a name="install"></a>Yükleme

Azure Farmtts kaynakları 'nın gerçek yüklemesi, Bash ortamı kullanılarak Cloud Shell tarayıcı tabanlı komut satırı arabiriminde gerçekleşir. Azure Farmınts 'yi yüklemek için aşağıdaki yönergeleri izleyin:

1. Azure Portal’da oturum açın. Azure Farmtts 'yi yüklemek istediğiniz Azure aboneliğini ve kiracıyı seçin.
2. Azure portal sağ üst köşesindeki araç çubuğundan **Cloud Shell** başlatın.
3. Tercih edilen kabuk deneyimi olarak Bash ' i seçin. **Karşıya yükle** düğmesini (aşağıdaki görüntüde vurgulanan) seçin ve hazırlanan parametreler json dosyasını karşıya yükleyin.

      ![Proje Farmtları](./media/install-azure-farmbeats/bash-2-1.png)

4. Aşağıdaki komutu **kopyalayın** ve **\<Kullanıcı adı >** doğru değerle değiştirin, böylece komut karşıya yüklenen dosyanın doğru yolunu işaret eder.

    ```bash
          wget -O farmbeats-installer.sh https://aka.ms/AzureFarmbeatsInstallerScript && bash farmbeats-installer.sh /home/<username>/input.json
    ```
5. Yükleme işlemini başlatmak için değiştirilen komutunu çalıştırın. Şunları yapmanız istenir:
 - **Azure Farmınts lisans** koşullarını kabul edin. Kullanım koşulları kabul ediyorsanız sonraki adıma geçmek için "Y" yazın. Kullanım koşullarını kabul etmiyorsanız yüklemeyi sonlandırmak için "N" yazın.

 - Sonra, yükleme için bir erişim belirteci girmeniz istenir. Oluşturulan kodu kopyalayın ve **Azure kimlik bilgilerinizle** [cihaz oturum açma sayfasında](https://microsoft.com/devicelogin) oturum açın.

 - Oturum açma işlemi başarıyla tamamlandıktan sonra yükleyici, Sentinel hesap parolanızı ister. **Sentinel hesap parolanızı**girin.

6. Parametreler dosyası onaylanır ve Azure kaynakları yüklemesi başlar. Yüklemenin tamamlanabilmesi yaklaşık **25 dakika** sürer.    
> [!NOTE]
> Etkin olmayan Cloud Shell oturumlarının süresi **20 dakika**sonra dolacak. Yükleyici Azure kaynaklarını dağıttığı sırada Cloud Shell oturumunun etkin kalmasını sağlayın. Oturum zaman aşımına uğrarsa, yüklemesi işlemini yeniden başlatmanız gerekir.

Yükleme tamamlandıktan sonra aşağıdaki çıkış bağlantılarını alırsınız:
* **Datahub URL 'si**: veri hub 'ı API 'lerine erişmek için Swagger bağlantısı.
* **HıZLANDıRıCı URL**: Azure Farmtts hızlandırıcıyı keşfetmek için Web uygulaması.
* **Yükleyici günlük dosyası**: yükleme ayrıntılarını içeren günlük dosyası. Bu günlük dosyası, gerekirse yükleme sorunlarını gidermek için kullanılabilir.

Aşağıdaki denetimleri gerçekleştirerek Azure Farmtts yüklemenizin tamamlandığını doğrulayabilirsiniz:

**Veri hub 'ı**
1. Azure kimlik bilgilerinizle sunulan Hızlandırıcı URL 'sinde ( **https://biçiminde\<yourdatahub-website-name >. azurewebsites. net/Swagger**) oturum açın.
2. Farklı Farmtts API nesnelerini görebilmeniz ve API 'lerde REST işlemleri gerçekleştirmeniz gerekir.

**Hızlandır**
1. Azure kimlik bilgilerinizle, sunulan Hızlandırıcı URL 'sinde oturum açın ( **\<https://. azurewebsites. net/Swagger**).
2. Hızlandırıcı Kullanıcı arabirimini tarayıcınızda gruplar oluşturma seçeneği ile görebilmeniz gerekir.

Yukarıdaki işlemleri gerçekleştirme becerinizin başarılı bir şekilde Azure Farmtts yüklemesi olduğunu gösterir.

## <a name="upgrade"></a>Yükseltin
Genel Önizleme sürümünde, mevcut bir Azure Farmtts yüklemesini yükseltmek için, Parameters dosyasındaki "**true**" olarak ayarlanmış ek bir "**updateifexists**" parametresi ile birlikte Cloud Shell yükleme komutunu yeniden çalıştırmanız gerekir. Güncelleştirme parametresi için aşağıdaki JSON örneğinin son satırına bakın.

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
Komutu, mevcut Azure Farmın yüklemesini en son sürüme güncelleştirir ve size çıkış bağlantıları sağlar.

## <a name="uninstall"></a>Kaldırma

Azure Farmrets veri hub 'ını veya hızlandırıcıyı kaldırmak için aşağıdaki adımları izleyin:

1.  Azure portal oturum açın ve bu bileşenlerin yüklendiği **kaynak gruplarını silin** .

2.  Azure Farmtts yüklemesine bağlı **Azure AD uygulamasını silmek** & Azure Active Directory gidin. Bu işlem Azure aboneliğinizdeki Azure Farmfor yüklemesini kaldırır.

## <a name="next-steps"></a>Sonraki adımlar
Azure aboneliğinizdeki Azure Farmtts 'yi yüklemeyi öğrendiniz. Şimdi Azure Farmtts örneğinize [Kullanıcı eklemeyi](manage-users-in-azure-farmbeats.md#manage-users) öğrenin.
