---
title: Azure Blockchain Çalışma Tezgahı Önizlemesini Dağıtma
description: Azure Blockchain Workbench Preview nasıl dağıtılır?
ms.date: 01/08/2020
ms.topic: article
ms.reviewer: brendal
ms.openlocfilehash: 141bb8825e47eb2309f9f551990a2976e8f4e209
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78943196"
---
# <a name="deploy-azure-blockchain-workbench-preview"></a>Azure Blockchain Çalışma Tezgahı Önizlemesini Dağıtma

Azure Blockchain Workbench Preview, Azure Marketi'nde bir çözüm şablonu kullanılarak dağıtılır. Şablon, blockchain uygulamaları oluşturmak için gereken bileşenlerin dağıtımını kolaylaştırır. Blockchain Workbench dağıtıldıktan sonra, kullanıcıları ve blockchain uygulamalarını oluşturmak ve yönetmek için istemci uygulamalarına erişim sağlar.

Blockchain Workbench bileşenleri hakkında daha fazla bilgi için [Azure Blockchain Workbench mimarisine](architecture.md)bakın.

[!INCLUDE [Preview note](./includes/preview.md)]

## <a name="prepare-for-deployment"></a>Dağıtım için hazırlanma

Blockchain Workbench, blockchain tabanlı bir uygulama oluşturmak için en sık kullanılan bir dizi alakalı Azure hizmetiyle birlikte bir blockchain defteri dağıtmanıza olanak tanır. Blockchain Workbench'in dağıtılması, Azure aboneliğinizde bir kaynak grubunda aşağıdaki Azure hizmetlerinin sağlanmasıyla sonuçlanır.

* Uygulama Hizmet Planı (Standart)
* Application Insights
* Event Grid
* Azure Key Vault
* Service Bus
* SQL Veritabanı (Standart S0) + SQL Mantıksal Sunucu
* Azure Depolama hesabı (Standart LRS)
* 1 kapasiteli sanal makine ölçeği
* Sanal Ağ kaynak grubu (Yük Dengeleyici, Ağ Güvenlik Grubu, Genel IP Adresi, Sanal Ağ ile)
* Azure Blockchain Hizmeti. Önceki bir Blockchain Workbench dağıtımını kullanıyorsanız, Azure Blockchain Workbench'i Azure Blockchain Service'i kullanmak üzere yeniden dağıtmayı düşünün.

Aşağıda, **myblockchain** kaynak grubunda oluşturulan örnek bir dağıtım verilmiştir.

![Örnek dağıtım](media/deploy/example-deployment.png)

Blockchain Workbench'in maliyeti, temel Azure hizmetlerinin maliyetinin toplamıdır. Azure hizmetleri için fiyatlandırma bilgileri [fiyatlandırma hesaplayıcısı](https://azure.microsoft.com/pricing/calculator/)kullanılarak hesaplanabilir.

## <a name="prerequisites"></a>Ön koşullar

Azure Blockchain Workbench, Azure AD yapılandırması ve uygulama kayıtları gerektirir. Dağıtımdan önce Azure AD [yapılandırmalarını el ile](#azure-ad-configuration) yapmayı veya komut dosyası sonrası dağıtımı çalıştırmayı seçebilirsiniz. Blockchain Workbench'i yeniden dağıtıyorsanız, Azure REKLAM yapılandırmanızı doğrulamak için [Azure REKLAM yapılandırmasına](#azure-ad-configuration) bakın.

> [!IMPORTANT]
> Çalışma tezgahı, bir Azure REKLAM uygulamasını kaydetmek için kullandığınız kiracıyla aynı kiracıda dağıtılmak zorunda değildir. Çalışma tezgahı, kaynakları dağıtmak için yeterli izinlere sahip olduğunuz bir kiracıda dağıtılmalıdır. Azure AD kiracıları hakkında daha fazla bilgi için Active [Directory kiracısını nasıl edinir](../../active-directory/develop/quickstart-create-new-tenant.md) ve [uygulamaları Azure Etkin Dizini ile tümleştirin.](../../active-directory/develop/quickstart-v1-integrate-apps-with-azure-ad.md)

## <a name="deploy-blockchain-workbench"></a>Blockchain Çalışma Tezgahı'nı dağıtın

Ön koşul adımları tamamlandıktan sonra Blockchain Workbench'i dağıtmaya hazırsınız. Aşağıdaki bölümlerde çerçevenin nasıl dağıtılanılaaçıklanmıştır.

1. [Azure portalında](https://portal.azure.com)oturum açın.
1. Hesabınızı sağ üst köşede seçin ve Azure Blockchain Workbench'i dağıtmak istediğiniz istediğiniz Azure AD kiracısına geçin.
1. Azure portalının sol üst köşesinde bulunan **Kaynak oluştur** öğesini seçin.
1. **Blockchain** > **Azure Blockchain Çalışma Tezgahı'nı seçin (önizleme)**.

    ![Azure Blockchain Çalışma Tezgahı Oluşturma](media/deploy/blockchain-workbench-settings-basic.png)

    | Ayar | Açıklama  |
    |---------|--------------|
    | Kaynak öneki | Dağıtımınız için kısa benzersiz tanımlayıcı. Bu değer kaynakları adlandırmak için bir temel olarak kullanılır. |
    | VM kullanıcı adı | Kullanıcı adı tüm sanal makineler (VM) için yönetici olarak kullanılır. |
    | Kimlik doğrulaması türü | VM'lere bağlanmak için parola veya anahtar kullanmak isteyip istemediğinizi seçin. |
    | Parola | Parola, VM'lere bağlanmak için kullanılır. |
    | SSH | **Ssh-rsa** ile başlayan tek satırlı biçimde bir RSA ortak anahtarı kullanın veya çok satırlı PEM biçimini kullanın. Linux ve OS X'i kullanarak `ssh-keygen` veya Windows'da PuTTYGen kullanarak SSH tuşları oluşturabilirsiniz. SSH tuşları hakkında daha fazla bilgi için, [Windows ile Azure'da SSH tuşlarının nasıl kullanılacağına](../../virtual-machines/linux/ssh-from-windows.md)bakın. |
    | Veritabanı ve Blockchain şifresi | Dağıtımın bir parçası olarak oluşturulan veritabanına erişmek için kullanılacak parolayı belirtin. Parola aşağıdaki dört gereksinimden üçünü karşılamalıdır: uzunluk 12 & 72 karakter, 1 küçük harf karakteri, 1 büyük harf karakteri, 1 sayı ve sayı işareti olmayan 1 özel karakter(#), yüzde/), virgül(,), yıldız(*), geri teklif(),çift\`tırnak("), tek teklif('), tire(-) ve yarı sütun(;) |
    | Dağıtım bölgesi | Blockchain Workbench kaynaklarının nerede dağıtılacağını belirtin. En iyi kullanılabilirlik için, bu **Konum** ayarı eşleşmelidir. |
    | Abonelik | Dağıtımınız için kullanmak istediğiniz Azure Aboneliğini belirtin. |
    | Kaynak grupları | **Yeni Oluştur'u** seçerek yeni bir Kaynak grubu oluşturun ve benzersiz bir kaynak grubu adı belirtin. |
    | Konum | Çerçeveyi dağıtmak istediğiniz bölgeyi belirtin. |

1. Temel ayar yapılandırma bölümünü bitirmek için **Tamam'ı** seçin.

1. **Gelişmiş**Ayarlar'da, yeni bir blockchain ağı oluşturmak veya varolan yetki belgesi blockchain ağı kullanmak isteyip istemediğinizi seçin.

    **Yeni Oluştur**için :

    *Yeni oluştur* seçeneği, varsayılan temel sku ile bir Azure Blockchain Hizmet Kotası genel muhasebesi dağıtır.

    ![Yeni blockchain ağı için gelişmiş ayarlar](media/deploy/advanced-blockchain-settings-new.png)

    | Ayar | Açıklama  |
    |---------|--------------|
    | Azure Blockchain Hizmeti fiyatlandırma katmanı | Blockchain Workbench için kullanılan **Temel** veya **Standart** Azure Blockchain Hizmet katmanını seçin |
    | Azure Active Directory ayarları | **Daha Sonra Ekle'yi**seçin.</br>Not: [Azure AD'yi önceden yapılandırmayı](#azure-ad-configuration) seçtiyseniz veya yeniden dağıtıyorsanız, *Şimdi Ekle'yi*seçin. |
    | VM seçimi | Blockchain ağınız için tercih edilen depolama performansını ve VM boyutunu seçin. Azure ücretsiz katmanı gibi düşük hizmet limitlerine sahip bir abonelikteyseniz, *Standart DS1 v2* gibi daha küçük bir VM boyutu seçin. |

    **Mevcut Kullanım**İçin :

    *Mevcut seçeneğin kullanımı,* bir Ethereum Proof-of-Authority (PoA) blockchain ağı belirtmenize olanak tanır. Uç noktalar aşağıdaki gereksinimlere sahiptir.

   * Bitiş noktası bir Ethereum Proof-of-Authority (PoA) blockchain ağı olmalıdır.
   * Bitiş noktası ağ üzerinden herkese açık olmalıdır.
   * PoA blockchain ağı, gaz fiyatısıfıra ayarlanmış olacak şekilde yapılandırılmalıdır.

     > [!NOTE]
     > Blockchain Workbench hesapları finanse edilmez. Para gerekiyorsa, işlemler başarısız olur.

     ![Varolan blockchain ağı için gelişmiş ayarlar](media/deploy/advanced-blockchain-settings-existing.png)

     | Ayar | Açıklama  |
     |---------|--------------|
     | Ethereum RPC Bitiş Noktası | Varolan bir PoA blockchain ağının RPC bitiş noktasını sağlayın. Bitiş noktası https:// veya http:// ile başlar ve bir bağlantı noktası numarasıyla biter. Örneğin, `http<s>://<network-url>:<port>` |
     | Azure Active Directory ayarları | **Daha Sonra Ekle'yi**seçin.</br>Not: [Azure AD'yi önceden yapılandırmayı](#azure-ad-configuration) seçtiyseniz veya yeniden dağıtıyorsanız, *Şimdi Ekle'yi*seçin. |
     | VM seçimi | Blockchain ağınız için tercih edilen depolama performansını ve VM boyutunu seçin. Azure ücretsiz katmanı gibi düşük hizmet limitlerine sahip bir abonelikteyseniz, *Standart DS1 v2* gibi daha küçük bir VM boyutu seçin. |

1. Gelişmiş Ayarları tamamlamak için **Tamam'ı** seçin.

1. Parametrelerinizin doğru olduğunu doğrulamak için özeti gözden geçirin.

    ![Özet](media/deploy/blockchain-workbench-summary.png)

1. Şartları kabul etmek ve Azure Blockchain Workbench'inizi dağıtmak için **Oluştur'u** seçin.

Dağıtım 90 dakika kadar sürebilir. İlerlemeyi izlemek için Azure portalını kullanabilirsiniz. Yeni oluşturulan kaynak grubunda, dağıtılan yapıtların durumunu görmek için **Dağıtımlar > Genel Bakış'ı** seçin.

> [!IMPORTANT]
> Dağıtım sonrası, Active Directory ayarlarını tamamlamanız gerekir. **Daha Sonra Ekle'yi**seçtiyseniz, Azure AD yapılandırma [komut dosyasını](#azure-ad-configuration-script)çalıştırmanız gerekir.  **Şimdi Ekle'yi**seçtiyseniz, [Yanıtla URL'sini yapılandırmanız](#configuring-the-reply-url)gerekir.

## <a name="blockchain-workbench-web-url"></a>Blockchain Çalışma Tezgahı web URL'si

Blockchain Workbench dağıtımı tamamlandıktan sonra, yeni bir kaynak grubu Blockchain Workbench kaynaklarınızı içerir. Blockchain Workbench hizmetlerine bir web URL'si üzerinden erişilir. Aşağıdaki adımlar, dağıtılan çerçevenin web URL'sini nasıl alabildiğinizi gösterir.

1. [Azure portalında](https://portal.azure.com)oturum açın.
1. Sol daki gezinti bölmesinde **Kaynak gruplarını**seçin.
1. Blockchain Workbench'i dağıtırken belirttiğiniz kaynak grubu adını seçin.
1. Listeyi türe göre alfabetik olarak sıralamak için **TYPE** sütununa başlığını seçin.
1. Tip **Uygulama Hizmeti**ile iki kaynak vardır. "-api" soneki *olmadan* **Uygulama Hizmeti** türünü seçin.

    ![Uygulama hizmet listesi](media/deploy/resource-group-list.png)

1. Uygulama Hizmetine **Genel Bakış'ta,** web URL'sini dağıtılmış Blockchain Workbench'ine temsil eden **URL** değerini kopyalayın.

    ![Uygulama hizmeti gereklilikleri](media/deploy/app-service.png)

Özel bir etki alanı adını Blockchain Workbench ile ilişkilendirmek [için, Trafik Yöneticisi'ni kullanarak Azure App Hizmeti'ndeki bir web uygulaması için özel bir etki alanı adı yapılandırma](../../app-service/configure-domain-traffic-manager.md)bölümüne bakın.

## <a name="azure-ad-configuration-script"></a>Azure AD yapılandırma komut dosyası

Azure AD, Blockchain Workbench dağıtımınızı tamamlamak için yapılandırılmalıdır. Yapılandırmayı yapmak için bir PowerShell komut dosyası kullanırsınız.

1. Tarayıcıda [Blockchain Workbench Web URL'sine](#blockchain-workbench-web-url)gidin.
1. Bulut Kabuğu'nu kullanarak Azure AD'yi ayarlama yönergelerini görürsünüz. Komutu kopyalayın ve Cloud Shell'i başlatın.

    ![AAD komut dosyalarını başlat](media/deploy/launch-aad-script.png)

1. Blockchain Workbench'i dağıttığınız Azure AD kiracısını seçin.
1. Cloud Shell PowerShell ortamında, komutu yapıştırın ve çalıştırın.
1. İstendiğinde, Blockchain Workbench için kullanmak istediğiniz Azure REKLAM kiracısını girin. Bu, Blockchain Workbench kullanıcılarını içeren kiracı olacaktır.

    > [!IMPORTANT]
    > Kimliği doğrulanan kullanıcı, Azure AD uygulama kayıtları oluşturmak ve kiracıya temsilci verilen uygulama izinleri vermek için izinler gerektirir. Kiracının yöneticisinden Azure AD yapılandırma komut dosyasını çalıştırmasını veya yeni bir kiracı oluşturmasını istemeniz gerekebilir.

    ![Azure AD kiracıgirin](media/deploy/choose-tenant.png)

1. Bir tarayıcı kullanarak Azure AD kiracısına kimlik doğrulamanız istenir. Web URL'sini tarayıcıda açın, kodu girin ve kimlik doğrulaması edin.

    ![Kodla kimlik doğrulaması](media/deploy/authenticate.png)

1. Komut dosyası çeşitli durum iletileri çıktırıyor. Kiracı başarıyla sağlanmışsa, bir **SUCCESS** durum iletisi alırsınız.
1. Blockchain Workbench URL'sine gidin. Dizin için okuma izinleri vermek için izin istenir. Bu, Blockchain Workbench web uygulamasının kiracıdaki kullanıcılara erişmesine olanak tanır. Kiracı yöneticiyseniz, tüm kuruluş için onay vermeyi seçebilirsiniz. Bu seçenek, kiracıdaki tüm kullanıcılar için onay kabul eder. Aksi takdirde, her kullanıcının Blockchain Workbench web uygulamasının ilk kullanımı için onay istenir.
1. Rıza **kabul** et'i seçin.

     ![Kullanıcı profillerini okumaya izin verme](media/deploy/graph-permission-consent.png)

1. Onay aldıktan sonra Blockchain Workbench web uygulaması kullanılabilir.

Azure Blockchain Workbench dağıtımınızı tamamladınız. Dağıtımınızı kullanmaya başlamak için öneriler için [Sonraki adımlara](#next-steps) bakın.

## <a name="azure-ad-configuration"></a>Azure AD yapılandırması

Dağıtımdan önce Azure AD ayarlarını el ile yapılandırmayı veya doğrulamayı seçerseniz, bu bölümdeki tüm adımları tamamlayın. Azure AD ayarlarını otomatik olarak yapılandırmayı tercih ediyorsanız, Blockchain Workbench'i dağıttıktan sonra [Azure AD yapılandırma komut dosyasını](#azure-ad-configuration-script) kullanın.

### <a name="blockchain-workbench-api-app-registration"></a>Blockchain Workbench API uygulama kaydı

Blockchain Workbench dağıtımı için Azure AD uygulamasının kaydedilmesi gerekir. Uygulamayı kaydetmek için bir Azure Etkin Dizin (Azure AD) kiracısına ihtiyacınız vardır. Varolan bir kiracı kullanabilir veya yeni bir kiracı oluşturabilirsiniz. Varolan bir Azure AD kiracısını kullanıyorsanız, uygulamaları kaydetmek, Grafik API izinleri vermek ve Azure REKLAM kiracısı içinde konuk erişimine izin vermek için yeterli izinlere ihtiyacınız vardır. Varolan bir Azure AD kiracısında yeterli izine sahip değilseniz, yeni bir kiracı oluşturun.


1. [Azure portalında](https://portal.azure.com)oturum açın.
1. Hesabınızı sağ üst köşede seçin ve istediğiniz Azure AD kiracısına geçin. Kiracı, Azure Blockchain Workbench'in dağıtıldığı aboneliğin abonelik yöneticisi olmalıdır ve uygulamaları kaydetmek için yeterli izine sahip siniz.
1. Sol taraftaki gezinti bölmesinde **Azure Active Directory** hizmetini seçin. **Uygulama kayıtlarını** > seçin**Yeni kayıt**.

    ![Uygulama kaydı](media/deploy/app-registration.png)

1. Bir görüntü **Adı** sağlayın ve **yalnızca bu kuruluş dizininde Hesaplar'ı**seçin.

    ![Uygulama kaydı oluşturma](media/deploy/app-registration-create.png)

1. Azure AD uygulamasını kaydetmek için **Kaydol'u** seçin.

### <a name="modify-manifest"></a>Bildirimi değiştir

Ardından, Blockchain Workbench yöneticilerini belirtmek için Azure AD içindeki uygulama rollerini kullanmak için bildirimi değiştirmeniz gerekir.  Uygulama bildirimleri hakkında daha fazla bilgi için [Azure Active Directory uygulama bildirimine](../../active-directory/develop/reference-app-manifest.md)bakın.


1. Manifesto için bir GUID gereklidir. PowerShell komutunu `[guid]::NewGuid()` veya `New-GUID` cmdlet'i kullanarak bir GUID oluşturabilirsiniz. Başka bir seçenek bir GUID jeneratör web sitesi kullanmaktır.
1. Kaydettiğiniz uygulama için **Yönet** bölümünde **Kisa'yı** seçin.
1. Ardından, bildirimin **appRoles** bölümünü güncelleyin. Sağlanan `"appRoles": []` JSON ile değiştirin. **Kimlik** alanı nın değerini oluşturduğunuz GUID ile değiştirdiğinden emin olun. 

    ![Bildirimi ni dala](media/deploy/edit-manifest.png)

    ``` json
    "appRoles": [
         {
           "allowedMemberTypes": [
             "User",
             "Application"
           ],
           "displayName": "Administrator",
           "id": "<A unique GUID>",
           "isEnabled": true,
           "description": "Blockchain Workbench administrator role allows creation of applications, user to role assignments, etc.",
           "value": "Administrator"
         }
       ],
    ```

    > [!IMPORTANT]
    > Blockchain Workbench yöneticilerini tanımlamak için **değer Yöneticisi** gereklidir.

1. Manifestoda, **oauth2AllowImplicitFlow** değerini de **gerçek**olarak değiştirin.

    ``` json
    "oauth2AllowImplicitFlow": true,
    ```

1. Bildirim değişikliklerini kaydetmek için **Kaydet'i** seçin.

### <a name="add-graph-api-required-permissions"></a>Grafik API gerekli izinleri ekleme

API uygulamasının dizine erişmek için kullanıcıdan izin istemesi gerekir. API uygulaması için aşağıdaki gerekli izni ayarlayın:

1. *Blockchain API* uygulama kaydında, **API izinlerini**seçin. Varsayılan olarak, Graph API **User.Read** izni eklenir.
1. Workbench uygulaması, kullanıcıların temel profil bilgilerine okuma erişimi gerektirir. *Yapılandırılmış izinlerde,* **İzin ekle'yi**seçin. **Microsoft API'lerinde,** **Microsoft Graph'ı**seçin.
1. Workbench uygulaması kimlik doğrulaması kullanıcı kimlik bilgilerini kullandığından, **Temsilci izinlerini**seçin.
1. *Kullanıcı* kategorisinde **User.ReadBasic.All** iznini seçin.

    ![Microsoft Graph User.ReadBasic.All delegated izni eklemeyi gösteren Azure AD uygulama kaydı yapılandırması](media/deploy/add-graph-user-permission.png)

    **İzin Ekle'yi**seçin.

1. *Yapılandırılmış izinlerde,* etki alanı için **Yönetici Onayı Ver'i** seçin ve ardından doğrulama istemi için **Evet'i** seçin.

   ![İzinleri verme](media/deploy/client-app-grant-permissions.png)

   İzin vermek Blockchain Workbench'in dizindeki kullanıcılara erişmesine olanak tanır. Blockchain Workbench'te arama yapmak ve üye eklemek için okuma izni gereklidir.

### <a name="get-application-id"></a>Uygulama kimliği alın

Dağıtım için uygulama kimliği ve kiracı bilgileri gereklidir. Dağıtım sırasında kullanılmak üzere bilgileri toplayın ve depolayın.

1. Kaydettiğiniz uygulama için **Genel Bakış'ı**seçin.
1. Dağıtım sırasında daha sonra kullanmak üzere **Uygulama Kimliği** değerini kopyalayın ve depolayın.

    ![API uygulama özellikleri](media/deploy/app-properties.png)

    | Depolama ya da depolama ayarlaması  | Dağıtımda kullanım |
    |------------------|-------------------|
    | Uygulama (istemci) kimliği | Azure Active Directory kurulumu > Uygulama Kimliği |

### <a name="get-tenant-domain-name"></a>Kiracı etki alanı adı alma

Uygulamaların kayıtlı olduğu Active Directory kiracı etki alanı adını toplayın ve depolayın. 

Sol taraftaki gezinti bölmesinde **Azure Active Directory** hizmetini seçin. **Özel etki alanı adları**'nı seçin. Etki alanı adını kopyalayın ve saklayın.

![Etki alanı adı](media/deploy/domain-name.png)

### <a name="guest-user-settings"></a>Konuk kullanıcı ayarları

Azure AD kiracınızda konuk kullanıcı varsa, Blockchain Workbench kullanıcı ataması ve yönetiminin düzgün çalıştığından emin olmak için ek adımları izleyin.

1. Azure AD kiracınızı değiştirin ve **Azure Etkin Dizin > Kullanıcı ayarlarını**seçin > dış işbirliği ayarlarını yönetin.
1. **Ayar Konuk kullanıcı izinleri** **Hayır**ile sınırlıdır.
    ![Dış işbirliği ayarları](media/deploy/user-collaboration-settings.png)

## <a name="configuring-the-reply-url"></a>Yanıt URL'sini yapılandırma

Azure Blockchain Workbench dağıtıldıktan sonra, dağıtılan Blockchain Workbench web URL'sinin Azure Etkin Dizin (Azure AD) istemci uygulaması **Yanıt URL'sini** yapılandırmanız gerekir.

1. [Azure portalında](https://portal.azure.com)oturum açın.
1. Azure AD istemcisi uygulamasını kaydettiğiniz kiracıda olduğunuzu doğrulayın.
1. Sol taraftaki gezinti bölmesinde **Azure Active Directory** hizmetini seçin. **Uygulama kayıtları**'nı seçin.
1. Ön koşul bölümüne kaydettiğiniz Azure AD istemcisi uygulamasını seçin.
1. **Kimlik Doğrulaması**'nı seçin.
1. [Blockchain Workbench web URL](#blockchain-workbench-web-url) bölümünde aldığınız Azure Blockchain Workbench dağıtımının ana web URL'sini belirtin. Yanıt URL'si `https://`önceden '. Örneğin, `https://myblockchain2-7v75.azurewebsites.net`

    ![Kimlik doğrulama url'leri](media/deploy/configure-reply-url.png)

1. Gelişmiş **ayar** bölümünde, **Erişim belirteçlerini** ve **kimlik belirteçlerini**denetleyin.

    ![Kimlik doğrulama gelişmiş ayarları](media/deploy/authentication-advanced-settings.png)

1. İstemci kaydını güncelleştirmek için **Kaydet'i** seçin.

## <a name="remove-a-deployment"></a>Dağıtımı kaldırma

Dağıtıma artık gerek olmadığında, Blockchain Çalışma Tezgahı kaynak grubunu silerek dağıtımı kaldırabilirsiniz.

1. Azure portalında, sol gezinti bölmesindeki **Kaynak grubuna** gidin ve silmek istediğiniz kaynak grubunu seçin. 
1. **Kaynak grubunu sil**'i seçin. Kaynak grup adını girerek silme işlemini doğrulayın ve **Sil'i**seçin.

    ![Kaynak grubunu silme](media/deploy/delete-resource-group.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu nasıl yapılsa makalede, Azure Blockchain Workbench'i dağıttınız. Blockchain uygulamasını nasıl oluşturabilirsiniz öğrenmek için bir sonraki nasıl yapılacağını makaleye devam edin.

> [!div class="nextstepaction"]
> [Azure Blockchain Workbench'te blockchain uygulaması oluşturma](create-app.md)
