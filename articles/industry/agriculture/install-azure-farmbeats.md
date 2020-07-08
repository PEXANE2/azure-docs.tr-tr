---
title: Azure FarmBeats'i yükleme
description: Bu makalede, Azure aboneliğinizdeki Azure Farmtts 'nin nasıl yükleneceği açıklanır.
author: usha-rathnavel
ms.topic: article
ms.date: 1/17/2020
ms.author: atinb
ms.openlocfilehash: 0761db6b73c6fcfeb1ef6fda729a68c9644bbc72
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "79479568"
---
# <a name="install-azure-farmbeats"></a>Azure FarmBeats'i yükleme

Bu makalede, Azure aboneliğinizdeki Azure Farmtts 'nin nasıl yükleneceği açıklanır.

Azure Farm, Azure Marketi 'nde bulunan işletmeden işletmeye yönelik bir tekliftir. Sağlayıcılar genelinde verilerin toplanmasının yanı sıra eyleme dönüştürülebilir içgörüler oluşturulmasına olanak tanıyor. Azure Farm, bu, fkullanılan veri kümelerine göre yapay zeka (AI) veya makine öğrenimi (ML) modelleri oluşturmanızı sağlayarak bunu yapar. Azure Farmtts 'nin iki ana bileşeni şunlardır:

- **Datahub**: farklı sağlayıcılardaki çeşitli agriclaral veri kümelerinin toplama, normalleştirme ve şeritleştirmeyi sağlayan bir API katmanı.

- **Hızlandırıcı**: veri hub 'ının üzerine inşa edilen Web uygulaması. Hızlı bir şekilde model geliştirme ve görselleştirmenizi başlatır. Hızlandırıcı, alınan algılayıcı verilerinin görselleştirmeyi ve model çıkışının harita olarak görselleştirmesini göstermek için Azure Farmınts API 'Lerini kullanır.

## <a name="general-information"></a>Genel bilgiler

### <a name="components-installed"></a>Yüklenen bileşenler

Azure Farmtts 'yi yüklediğinizde, Azure aboneliğinizde aşağıdaki kaynaklar sağlanır:

| Azure kaynakları yüklendi  | Azure Farmtempts bileşeni  |
|---------|---------|
| Application Insights   |      Datahub & Hızlandırıcısı      |
| App Service     |     Datahub & Hızlandırıcısı     |
| App Service Planı   | Datahub & Hızlandırıcısı  |
| API bağlantısı    |  Veri hub 'ı       |
| Redis için Azure Önbelleği       | Veri hub 'ı      |
| Azure Cosmos DB   |  Veri hub 'ı       |
| Azure Data Factory V2       |     Datahub & Hızlandırıcısı      |
| Azure Batch hesabı    | Veri hub 'ı   |
| Azure Key Vault |  Datahub & Hızlandırıcısı        |
| Azure haritalar hesabı       |     Hızlandır    |
| Olay Hub 'ı ad alanı    |     Veri hub 'ı      |
| Logic App      |  Veri hub 'ı       |
| Depolama Hesabı      |     Datahub & Hızlandırıcısı      |
| Time Series Insights     |    Veri hub 'ı    |

### <a name="costs-incurred"></a>Tahakkuk eden maliyetler

Azure Farmtts 'nin maliyeti, temel alınan Azure hizmetlerinin maliyetinin toplamıdır. Azure hizmetleri fiyatlandırma bilgileri, [Fiyatlandırma hesaplayıcısı](https://azure.microsoft.com/pricing/calculator)kullanılarak hesaplanabilir. Toplam yüklemenin gerçek maliyeti, kullanıma bağlı olarak değişir. İki bileşen için sabit durum maliyeti:

- Veri merkezi-günde $10 ' den az
- Hızlandırıcı-günde $2 'den az

### <a name="regions-supported"></a>Desteklenen bölgeler

Şu anda, şu bölgelerde genel bulut ortamlarında Azure Farmtempts desteklenir:

- Doğu Avustralya
- Orta ABD
- Doğu ABD
- Doğu ABD 2
- Batı ABD
- Batı ABD 2
- Kuzey Avrupa
- Batı Avrupa
- Doğu Asya
- Güneydoğu Asya

### <a name="time-taken"></a>Geçen süre

Hazırlama ve yükleme dahil olmak üzere tüm Azure Farmtts kurulumu bir saatten kısa sürer.

## <a name="prerequisites"></a>Ön koşullar

Azure Farmtts 'nin gerçek yüklemesine başlamadan önce aşağıdaki adımları gerçekleştirmeniz gerekir:

### <a name="verify-permissions"></a>Izinleri doğrulama

Azure Farmtts 'yi yüklemek için Azure kiracısında aşağıdaki izinlere sahip olmanız gerekir:

- Kiracı-AAD uygulama Oluşturucu
- Abonelik-sahip
- Farmtts 'nin yüklendiği kaynak grubu-sahip

[AAD uygulama adımını oluşturmak](#create-an-aad-application) için ilk iki izin gerekir. Gerekirse, AAD uygulamasını oluşturmak için uygun izinlere sahip bir kişiye ulaşabilirsiniz.

Market 'ten yüklenen Farmtts 'leri çalıştıran kişinin, Farmtts 'nin yüklenmekte olduğu kaynak grubunun sahibi olması gerekir. Abonelik sahipleri için, kaynak grubu oluşturulduğunda bu otomatik olarak gerçekleşir. Diğerleri için lütfen kaynak grubunu önceden oluşturun ve abonelik sahibinden kaynak grubunun sahibini yapmasını isteyin.

[Rol tabanlı erişim denetimi](https://docs.microsoft.com/azure/role-based-access-control/check-access)yönergelerini izleyerek Azure Portal erişim izinlerinizi doğrulayabilirsiniz.

### <a name="decide-subscription-and-region"></a>Aboneliğe ve bölgeye karar verme

Azure abonelik KIMLIĞI ve Azure Farmtts 'yi yüklemek istediğiniz bölge gereklidir. [Desteklenen bölgeler](#regions-supported) bölümünde listelenen bölgelerden birini seçin.

**Azure ABONELIK kimliği** ve **Azure bölgesi**' ni bir yere unutmayın.

### <a name="create-an-aad-application"></a>AAD uygulaması oluşturma

Azure Farmtts, Azure Active Directory uygulama oluşturma ve kaydetme gerektirir. AAD oluşturma betiğini başarılı bir şekilde çalıştırmak için aşağıdaki izinler gereklidir:

- Kiracı-AAD uygulama Oluşturucu
- Abonelik-sahip

PowerShell ortamını kullanarak bir Cloud Shell örneğinde aşağıdaki adımları çalıştırın. Kullanıcılardan önce bir abonelik seçmesi ve bir depolama hesabı oluşturması istenir. Kurulumu belirtildiği gibi doldurun.

1. [AAD uygulama Oluşturucu betiğini](https://aka.ms/FarmBeatsAADScript) indirin

    ```azurepowershell-interactive
        wget -q https://aka.ms/FarmBeatsAADScript -O ./create_aad_script.ps1
    ```

2. Varsayılan olarak, dosya giriş dizininize indirilir. Dizinine gidin.

    ```azurepowershell-interactive
        cd
    ```

3. AAD betiğini çalıştırma

    ```azurepowershell-interactive
        ./create_aad_script.ps1
    ```

4. Betik aşağıdaki üç girişi ister:

    - **Farmtts Web sitesi adı**: Bu, farmtts Web UYGULAMANıZıN benzersiz URL önekidir. Ön ek zaten alınmış olması durumunda, betik hata vermez. Yüklendikten sonra, https://. azurewebsites.net adresinden Farmtts dağıtımına erişilebilecektir \<FarmBeats-website-name> ve Swagger API 'leri https:// \<FarmBeats-website-name> -api.azurewebsites.net olacaktır

    - **Azure oturum açma kimliği**: Farmof 'lar Yöneticisi olarak eklenmesini istediğiniz kullanıcı için Azure oturum açma kimliği sağlayın. Bu Kullanıcı daha sonra, diğer kullanıcılara Farmtempts Web uygulamasına erişim izni verebilir. Oturum açma KIMLIĞI genellikle formdan oluşur john.doe@domain.com . Azure UPN de desteklenir.

    - **ABONELIK kimliği**: Bu, Azure Farmtts 'yi yüklemek ISTEDIĞINIZ aboneliğin kimliğidir

5. AAD betiği, aynı dizindeki bir JSON dosyasına ve hem ekranda hem de değerleri çalıştırmak ve çıktısını almak için 2 dakika sürer. Betiği başka birine çalıştırdıysanız, bu çıktıyı sizinle paylaşmasını isteyin.

### <a name="create-sentinel-account"></a>Sentinel hesabı oluştur

Azure Farmtts kurulumu, Avrupa alan Ajancınızın [Sentinel-2](https://scihub.copernicus.eu/) uydu görev adresinden uydu imalatı almanızı sağlar. Bu kurulumu yapılandırmak için bir Sentinel hesabı gerekir.

Sentinel ile ücretsiz bir hesap oluşturmak için aşağıdaki adımları izleyin:

1. Resmi [kaydolma](https://aka.ms/SentinelRegistration) sayfasına gidin.
2. Gerekli ayrıntıları (ad, soyadı, Kullanıcı adı, parola ve e-posta KIMLIĞI) sağlayın ve formu doldurun.
3. Kayıtlı e-posta KIMLIĞINE bir doğrulama bağlantısı gönderilir. E-postada sunulan bağlantıyı seçin ve doğrulamayı doldurun.

Kayıt işleminiz tamamlanmıştır. Doğrulama işlemi tamamlandıktan sonra, **Sentinel Kullanıcı adı** ve **Sentinel parolanızı**bir yere göz önünde bulabilirsiniz.

## <a name="install"></a>Yükleme

Artık Farmtempts 'yi yüklemeye hazırsınız. Yüklemeyi başlatmak için aşağıdaki adımları izleyin:

1. Azure Portal’da oturum açın. Sağ üst köşede hesabınızı seçin ve Azure Farmtts 'yi yüklemek istediğiniz Azure AD kiracısına geçiş yapın.

2. Portal 'da Azure Marketi ' ne gidin ve Market 'te **Azure Farmtempts** 'yi arayın.

3. Azure Farmtts 'ye genel bakış içeren yeni bir pencere görüntülenir. **Oluştur**'u seçin.

4. Yeni bir pencere görüntülenir. Azure Farmtts 'yi yüklemek istediğiniz doğru aboneliği, kaynak grubunu ve konumu seçerek kaydolma işlemini tamamlayabilirsiniz.

5. **Farmtts hizmet uyarıları** bölümünde Azure farmalerts ile ilgili herhangi bir hizmet uyarısı alması gereken e-posta adresini belirtin. **Bağımlılıklar** sekmesine geçmek için sayfanın alt kısmındaki **İleri ' yi** seçin.

    ![Temel bilgiler sekmesi](./media/install-azure-farmbeats/create-azure-farmbeats-basics.png)

6. [AAD komut dosyasının](#create-an-aad-application) çıktısından tek tek girdileri AAD uygulaması bölümündeki girişlere kopyalayın.

7. Sentinel hesabı bölümünde [Sentinel hesabının](#create-sentinel-account) Kullanıcı adını ve parolasını girin. **Gözden geçir + oluştur** sekmesine geçmek için **İleri ' yi** seçin.

    ![Bağımlılıklar Sekmesi](./media/install-azure-farmbeats/create-azure-farmbeats-dependencies.png)

8. Girilen Ayrıntılar doğrulandıktan sonra **Tamam**' ı seçin. Kullanım koşulları sayfası görüntülenir. Koşulları gözden geçirin ve yüklemeyi başlatmak için **Oluştur** ' u seçin. Yükleme ilerlemesini izleyebileceğiniz sayfaya yönlendirilirsiniz.

Yükleme tamamlandıktan sonra, yükleme sırasında verdiğiniz web sitesi adına giderek, yüklemeyi doğrulayabilirsiniz ve Farmtempts portalını kullanmaya başlayabilirsiniz: https:// \<FarmBeats-website-name> . azurewebsites.net. Gruplar oluşturma seçeneği ile Farmtempts Kullanıcı arabirimini görmeniz gerekir.

**Datahub** , https:// \<FarmBeats-website-name> -api.azurewebsites.net/Swagger adresinde bulunabilir. Burada, farklı Farmtts API nesneleri ve API 'lerde REST işlemleri gerçekleştirmeniz gerekir.

## <a name="upgrade"></a>Yükseltme

Farmtts 'leri en son sürüme yükseltmek için aşağıdaki adımları PowerShell ortamını kullanarak bir Cloud Shell örneğinde çalıştırın. Kullanıcının, Farmtts 'nin yüklü olduğu aboneliğin sahibi olması gerekir.

Kullanıcılardan önce bir abonelik seçmesi ve bir depolama hesabı oluşturması istenir. Kurulumu belirtildiği gibi doldurun.

1. [Yükseltme betiğini](https://aka.ms/FarmBeatsUpgradeScript) indir

    ```azurepowershell-interactive
        wget –q https://aka.ms/FarmBeatsUpgradeScript -O ./upgrade-farmbeats.ps1
    ```

2. Varsayılan olarak, dosya giriş dizininize indirilir. Dizinine gidin.

    ```azurepowershell-interactive
        cd
    ```

3. Yükseltme betiğini Çalıştır

    ```azurepowershell-interactive
        ./upgrade-farmbeats.ps1 -InputFilePath [Path to input.json file]
    ```

Dosyadaki input.jsyolu isteğe bağlıdır. Belirtilmezse, betik tüm gerekli girişleri ister. Yükseltmenin yaklaşık 30 dakika içinde tamamlanmalıdır.

## <a name="uninstall"></a>Kaldırma

Azure Farmrets veri hub 'ını veya hızlandırıcıyı kaldırmak için aşağıdaki adımları izleyin:

1. Azure portal oturum açın ve bu bileşenlerin yüklendiği **kaynak gruplarını silin** .

2. Azure Farmtts yüklemesine bağlı **Azure AD uygulamasını silmek** & Azure Active Directory gidin.

## <a name="next-steps"></a>Sonraki adımlar

Azure aboneliğinizdeki Azure Farmtts 'yi yüklemeyi öğrendiniz. Şimdi Azure Farmtts örneğinize [Kullanıcı eklemeyi](manage-users-in-azure-farmbeats.md#manage-users) öğrenin.
