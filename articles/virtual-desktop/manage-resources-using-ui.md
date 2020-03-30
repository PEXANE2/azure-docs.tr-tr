---
title: Azure Kaynak Yöneticisi şablonuyla yönetim aracıdağıtma - Azure
description: Windows Sanal Masaüstü kaynaklarını yönetmek için Azure Kaynak Yöneticisi şablonu olan bir kullanıcı arabirimi aracı nasıl yüklenir?
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 01/10/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: dac17da4aa3d3d73ccbbc101cba4326a17fec9b1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79127775"
---
# <a name="deploy-a-management-tool-with-an-azure-resource-manager-template"></a>Azure Kaynak Yöneticisi şablonuyla bir yönetim aracı dağıtma

Bu makaledeki yönergeler, bir Azure Kaynak Yöneticisi şablonu kullanarak UI'yi nasıl dağıtacağınızı size gösterir.

## <a name="important-considerations"></a>Önemli noktalar

Uygulama Windows Sanal Masaüstü ile etkileşim kurmak için izin gerektirdiğinden, bu araç İşletmelerarası (B2B) senaryolarını desteklemez. Her Azure Etkin Dizin (AAD) kiracısının aboneliğinin yönetim aracının kendi ayrı dağıtımına ihtiyacı vardır.

Bu yönetim aracı bir örnektir. Microsoft önemli güvenlik ve kalite güncelleştirmeleri sağlayacaktır. [Kaynak kodu GitHub'da kullanılabilir.](https://github.com/Azure/RDS-Templates/tree/master/wvd-templates/wvd-management-ux/deploy) Müşteriler ve iş ortakları, aracı iş gereksinimlerine uyacak şekilde özelleştirmeye teşvik edilir.

Aşağıdaki tarayıcılar yönetim aracı ile uyumludur:
- Google Chrome 68 veya sonrası
- Microsoft Edge 40.15063 veya sonrası
- Mozilla Firefox 52.0 veya sonrası
- Safari 10 veya sonraki (yalnızca macOS)

## <a name="what-you-need-to-deploy-the-management-tool"></a>Yönetim aracını dağıtmak için gerekenler

Yönetim aracını dağıtmadan önce, bir uygulama kaydı oluşturmak ve yönetim kullanıcısını dağıtmak için bir Azure Etkin Dizin (Azure AD) kullanıcısına ihtiyacınız vardır. Bu kullanıcı şunları yapmalı:

- Azure Çok Faktörlü Kimlik Doğrulama (MFA) devre dışı
- Azure aboneliğinizde kaynak oluşturma iznine sahip olun
- Azure AD uygulaması oluşturmak için izniniz var. Kullanıcınızın [Gerekli Izinler'deki](../active-directory/develop/howto-create-service-principal-portal.md#required-permissions)yönergeleri izleyerek gerekli izinlere sahip olup olmadığını kontrol etmek için aşağıdaki adımları izleyin.

Yönetim aracını dağıttıktan ve yapılandırdıktan sonra, her şeyin çalıştığından emin olmak için bir kullanıcıdan yönetim kullanıcıara'sını başlatmasını istemenizi öneririz. Yönetim Kullanıcı Yı'sını başlatan kullanıcının, Windows Sanal Masaüstü kiracısını görüntülemelerine veya yönetmelerine olanak tanıyan bir rol ataması olmalıdır.

## <a name="deploy-the-management-tool"></a>Yönetim aracını dağıtma

Başlamadan önce, sunucu ve istemci uygulamalarının temsil edilen Azure Etkin Dizini için [Windows Sanal Masaüstü Onay Sayfasını](https://rdweb.wvd.microsoft.com) (AAD) ziyaret ederek onay verdiğinden emin olun.

Azure Kaynak Yönetimi şablonuna dağıtmak için aşağıdaki yönergeleri izleyin:

1. [GitHub Azure RDS-Şablonlar sayfasına](https://github.com/Azure/RDS-Templates/tree/master/wvd-templates/wvd-management-ux/deploy)gidin.
2. Şablonu Azure'a dağıtın.
    - Kurumsal abonelikte dağıtım yapmaktaysanız, aşağı kaydırın ve **Azure'a Dağıt'ı**seçin. 
    - Bulut Çözüm Sağlayıcısı aboneliğinde dağıtım yapıyorsunuz, Azure'a dağıtmak için aşağıdaki yönergeleri izleyin:
        1. Aşağı kaydırın ve **Azure'a Dağıt'ı**sağ tıklatın, ardından **Bağlantı Konumunu Kopyala'yı**seçin.
        2. Not Defteri gibi bir metin düzenleyicisi açın ve bağlantıyı buraya yapıştırın.
        3. Hashtag'ten (#) hemen sonra <https://portal.azure.com/> ve öncesinde, kiracı alan adı ardından bir at işareti (@) girin. Biçime bir örnek aşağıda <https://portal.azure.com/@Contoso.onmicrosoft.com#create/>verilmiştir: .
        4. Bulut Çözüm Sağlayıcısı aboneliğine Yönetici/Katılımcı izni olan bir kullanıcı olarak Azure portalında oturum açın.
        5. Metin düzenleyicisine kopyaladığınız bağlantıyı adres çubuğuna yapıştırın.
3. Parametreleri girerken aşağıdakileri yapın:
    - **isServicePrincipal** parametresi için **yanlış'ı**seçin.
    - Kimlik bilgileri için, çok faktörlü kimlik doğrulama devre dışı bırakılmış Azure REKLAM kimlik bilgilerinizi girin. Bu kimlik bilgileri, Azure AD uygulamasını ve Azure kaynaklarını oluşturmak için kullanılır. Daha fazla bilgi edinmek [için yönetim aracını dağıtmak için gerekenlere](#what-you-need-to-deploy-the-management-tool)bakın.
    - **applicationName**için, Uygulamanız için Azure Active Dizininize kayıtlı olacak benzersiz bir ad kullanın. Bu ad, web uygulaması URL'si için de kullanılır. Örneğin, "Apr3UX" gibi bir ad kullanabilirsiniz.
4. Parametreleri sağladıktan sonra, şart ve koşulları kabul edin ve **Satın Al'ı**seçin.

## <a name="provide-consent-for-the-management-tool"></a>Yönetim aracı için onay sağlama

GitHub Azure Kaynak Yöneticisi şablonu tamamlandıktan sonra, Azure portalında bir uygulama hizmet planıyla birlikte iki uygulama hizmeti içeren bir kaynak grubu bulacaksınız.

Oturum açmadan ve yönetim aracını kullanmadan önce, yönetim aracıyla ilişkili yeni Azure AD uygulaması için onay sağlamanız gerekir. İzin sağlamak, yönetim aracının, araçta oturum açmış olan kullanıcı adına Windows Sanal Masaüstü yönetim çağrıları yapmasına olanak tanır.

![UI yönetim aracını kabul ettiğinizde verilen izinleri gösteren bir ekran görüntüsü.](media/management-ui-delegated-permissions.png)

Araçta oturum açmada hangi kullanıcıyı kullanabileceğinizi belirlemek için [Azure Active Directory kullanıcı ayarları sayfanıza](https://portal.azure.com/#blade/Microsoft_AAD_IAM/StartboardApplicationsMenuBlade/UserSettings/menuId/) gidin ve Kullanıcılar için değeri dikkate alın ve **şirket verilerine kendi adlarına erişen uygulamalara izin verebilir.**

![Kullanıcıların yalnızca kendi kullanıcıları için uygulamalara izin verilip verilemediğini gösteren bir ekran görüntüsü.](media/management-ui-user-consent-allowed.png)

- Değer **Evet**olarak ayarlanmışsa, Azure Etkin Dizini'ndeki herhangi bir kullanıcı hesabıyla oturum açabilir ve yalnızca bu kullanıcı için onay verebilirsiniz. Ancak, yönetim aracında daha sonra farklı bir kullanıcıyla oturum açtığınızda, aynı onayı yeniden gerçekleştirmeniz gerekir.
- Değer **Hayır**olarak ayarlanmışsa, Azure Etkin Dizininde Global Administrator olarak oturum açmanız ve dizindeki tüm kullanıcılar için yönetici onayı sağlamanız gerekir. Başka hiçbir kullanıcı bir onay istemi karşı karşıya gelecektir.


İzin vermek için hangi kullanıcıyı kullanacağınıza karar verdikten sonra, araca onay vermek için aşağıdaki yönergeleri izleyin:

1. Azure kaynaklarınıza gidin, şablonda sağladığınız adla Azure Uygulama Hizmetleri kaynağını seçin (örneğin, Apr3UX) ve onunla ilişkili URL'ye gidin; örneğin, <https://rdmimgmtweb-210520190304.azurewebsites.net>.
2. Uygun Azure Active Directory kullanıcı hesabını kullanarak oturum açın.
3. Bir Genel Yönetici ile kimlik doğrulaması verdiyseniz, artık **kuruluşunuz adına Onay için**onay kutusunu seçebilirsiniz. İzin vermek için **Kabul Et'i** seçin.
   
   ![Kullanıcının veya yöneticinin göreceği tam onay sayfasını gösteren bir ekran görüntüsü.](media/management-ui-consent-page.png)

Bu şimdi yönetim aracına götürecektir.

## <a name="use-the-management-tool"></a>Yönetim aracını kullanma

Kuruluş veya belirli bir kullanıcı için onay sağladıktan sonra, yönetim aracına istediğiniz zaman erişebilirsiniz.

Aracı başlatmak için aşağıdaki yönergeleri izleyin:

1. Şablonda sağladığınız adla (örneğin, Apr3UX) Azure Uygulama Hizmetleri kaynağını seçin ve bu kaynakla ilişkili URL'ye gidin; örneğin, <https://rdmimgmtweb-210520190304.azurewebsites.net>.
2. Windows Sanal Masaüstü kimlik bilgilerinizi kullanarak oturum açın.
3. Bir Kiracı Grubu seçilmesi istendiğinde, açılan listedevarsayılan **Kiracı Grubu'nu** seçin.
4. **Varsayılan Kiracı Grubu'nu**seçtiğinizde, pencerenizin sol tarafında bir menü görünmelidir. Bu menüde, kiracı grubunuzun adını bulun ve seçin.
  
  > [!NOTE]
  > Özel bir kiracı grubunuz varsa, açılan listeden seçim yapmak yerine adı el ile girin.

## <a name="report-issues"></a>Sorun bildirme

Yönetim aracı veya diğer Windows Sanal Masaüstü araçlarıyla ilgili herhangi bir sorunla karşılaşırsanız, Bunları GitHub'da bildirmek [için Uzak Masaüstü Hizmetleri için Azure Kaynak Yöneticisi şablonlarındaki](https://github.com/Azure/RDS-Templates/blob/master/README.md) yönergeleri izleyin.

## <a name="next-steps"></a>Sonraki adımlar

Yönetim aracını dağıtmayı ve bunlara bağlanmayı öğrendiğiniz için, hizmet sorunlarını ve sistem durumu danışma belgelerini izlemek için Azure Hizmeti'ni nasıl kullanacağınızı öğrenebilirsiniz. Daha fazla bilgi edinmek için, [servis uyarıları hazırla öğreticimize](./set-up-service-alerts.md)bakın.