---
title: Windows Sanal Masaüstü için tanılama aracını dağıtma - Azure
description: Windows Sanal Masaüstü için tanılama UX aracı nasıl dağıtılır.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 03/20/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 4eb63fe4bd8f8a8b0961aa6a7fccb8de9b7c2f16
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80123398"
---
# <a name="deploy-the-diagnostics-tool"></a>Tanılama aracını dağıtma

>[!IMPORTANT]
>16 Mart 2020 itibarıyla, hizmete artan talep nedeniyle kullanıcı deneyimini etkileyen tanılama sorgularını geçici olarak devre dışı bıraktık. Bu, bu sorguların çalışmasına dayandığı için aracın çalışmayı durdurmasına neden olur. Tanılama sorguları yeniden kullanılabilir olduğunda bu makaleyi güncelleştireceğiz.
>
>O zamana kadar, sürekli izleme için [Log Analytics'i kullanmanızı](diagnostics-log-analytics.md) şiddetle öneririz.

Windows Sanal Masaüstü için tanılama aracının sizin için yapabilecekleri aşağıda açıklanabilir:

- Bir haftalık bir süre içinde tek bir kullanıcı için tanılama etkinliklerine (yönetim, bağlantı veya özet akışı) bakın.
- Log Analytics çalışma alanınızdan bağlantı etkinlikleri için oturum ana bilgisayar bilgilerini toplayın.
- Belirli bir ana bilgisayar için sanal makine (VM) performans ayrıntılarını gözden geçirin.
- Oturum ana bilgisayarda hangi kullanıcıların oturumda oturum alabilen kullanıcılar olduğunu görün.
- Belirli bir oturum ana bilgisayarındaki etkin kullanıcılara ileti gönderin.
- Kullanıcıları oturum ana bilgisayardan dağıtın.

## <a name="prerequisites"></a>Ön koşullar

Araç için Azure Kaynak Yöneticisi şablonu dağıtabilmek için önce bir Azure Active Directory App Kaydı ve Log Analytics çalışma alanı oluşturmanız gerekir. Sizin veya yöneticinin bunu yapmak için bu izinlere ihtiyacı vardır:

- Azure aboneliğinin sahibi
- Azure aboneliğinizde kaynak oluşturma izni
- Azure AD uygulaması oluşturma izni
- RDS Sahibi veya Katılımcı hakları

Ayrıca başlamadan önce bu iki PowerShell modülleri yüklemeniz gerekir:

- [Azure PowerShell modülü](/powershell/azure/install-az-ps?view=azps-2.4.0/)
- [Azure AD modülü](/powershell/azure/active-directory/install-adv2?view=azureadps-2.0/)

Oturum açken Abonelik Kimliğinizin hazır olduğundan emin olun.

Her şeyi düzene savurduktan sonra Azure AD uygulama kaydını oluşturabilirsiniz.

## <a name="create-an-azure-active-directory-app-registration"></a>Azure Active Directory uygulama kaydı oluşturma

Bu bölümde, powershell'i bir hizmet sorumlusuyla Azure Active Directory uygulamasını oluşturmak ve bunun için API izinleri almak için nasıl kullanacağınızı gösterecektir.

>[!NOTE]
>API izinleri Windows Sanal Masaüstü, Log Analytics ve Microsoft Graph API izinleri Azure Active Directory Uygulamasına eklenir.

1. Yönetici olarak PowerShell’i açın.
2. Tanılama aracı için kullanmak istediğiniz Azure aboneliğinde Sahibi veya Katılımcı izinlerine sahip bir hesapla Azure'da oturum açın:
   ```powershell
   Login-AzAccount
   ```
3. Aynı hesapla Azure AD'de oturum açın:
   ```powershell
   Connect-AzureAD
   ```
4. [RDS-Templates GitHub repo'ya](https://github.com/Azure/RDS-Templates/tree/master/wvd-templates/diagnostics-sample/deploy/scripts) gidin ve PowerShell'de **CreateADAppRegistrationforDiagnostics.ps1** komut dosyasını çalıştırın.
5.  Komut dosyası uygulamanızın adını sizden istediğinde, benzersiz bir uygulama adı girin.


Komut dosyası başarıyla çalıştırıladıktan sonra, çıktısında aşağıdaki şeyleri göstermelidir:

-  Uygulamanızın artık bir hizmet temel rol ataması olduğunu onaylayan bir ileti.
-  Tanılama aracını dağıtırken gereksinim duyduğunuz İstemci Kimliğiniz ve İstemci Gizli Anahtarınız.

Artık uygulamanızı kaydettiğinize göre, Log Analytics çalışma alanınızı yapılandırma nın zamanı geldiğinden.

## <a name="configure-your-log-analytics-workspace"></a>Günlük Analizi çalışma alanınızı yapılandırın

Mümkün olan en iyi deneyim için, Log Analytics çalışma alanınızı, kullanıcı deneyiminin uzak bir oturumda ifadelerini elde etmenize olanak tanıyan aşağıdaki performans sayaçlarıyla yapılandırmanızı öneririz. Önerilen eşiklere sahip önerilen sayaçların listesi için [Windows performans sayacı eşiklerine](deploy-diagnostics.md#windows-performance-counter-thresholds)bakın.

### <a name="create-an-azure-log-analytics-workspace-using-powershell"></a>PowerShell'i kullanarak Azure Günlük Analizi çalışma alanı oluşturun

Log Analytics çalışma alanı oluşturmak ve kullanıcı deneyimini ve uygulama performansını izlemek için önerilen Windows performans sayaçlarını yapılandırmak için bir PowerShell komut dosyası çalıştırabilirsiniz.

>[!NOTE]
>Kullanmak istediğiniz PowerShell komut dosyası olmadan yaptığınız varolan bir Log Analytics çalışma alanınız varsa, [Azure portalındaki komut dosyası sonuçlarını doğrulamak](#validate-the-script-results-in-the-azure-portal)için ileri ye atlayın.

PowerShell komut dosyasını çalıştırmak için:

1.  PowerShell'i yönetici olarak açın.
2.  [RDS-Templates GitHub repo'ya](https://github.com/Azure/RDS-Templates/tree/master/wvd-templates/diagnostics-sample/deploy/scripts) gidin ve PowerShell'de **CreateLogAnalyticsWorkspaceforDiagnostics.ps1** komut dosyasını çalıştırın.
3. Parametreler için aşağıdaki değerleri girin:

    - **ResourceGroupName**için kaynak grubunun adını girin.
    - **LogAnalyticsWorkspaceName**için Log Analytics çalışma alanınız için benzersiz bir ad girin.
    - **Konum**için, kullanmakta olduğunuz Azure bölgesini girin.
    - Azure portalında **Abonelikler**altında bulabileceğiniz **Azure Abonelik Kimliği'ni**girin.

4. Temsilci yönetici erişimine sahip bir kullanıcının kimlik bilgilerini girin.
5. Aynı kullanıcının kimlik bilgileriyle Azure portalında oturum açın.
6. LogAnalyticsWorkspace kimliğini daha sonra için yazın veya ezberleyin.
7. PowerShell komut dosyasıyla Log Analytics çalışma alanını ayarlarsanız, performans sayaçlarınız zaten yapılandırılmış olmalı ve [Azure portalındaki komut dosyası sonuçlarını doğrulamak](#validate-the-script-results-in-the-azure-portal)için ileri ye geçebilirsiniz. Aksi takdirde, sonraki bölüme devam edin.

### <a name="configure-windows-performance-counters-in-your-existing-log-analytics-workspace"></a>Mevcut Log Analytics çalışma alanınızda Windows performans sayaçlarını yapılandırın

Bu bölüm, önceki bölümde PowerShell komut dosyası olmadan oluşturulan varolan bir Azure Log Analytics çalışma alanını kullanmak isteyen kullanıcılar içindir. Komut dosyasını kullanmadıysanız, önerilen Windows performans sayaçlarını el ile yapılandırmanız gerekir.

Önerilen performans sayaçlarını el ile nasıl yapılandırabilirsiniz:

1. İnternet tarayıcınızı açın ve yönetim hesabınızla [Azure portalında](https://portal.azure.com/) oturum açın.
2. Ardından, yapılandırılan Windows Performans Sayaçlarını incelemek için **Günlük Analizi çalışma alanlarına** gidin.
3. **Ayarlar** bölümünde Gelişmiş **ayarlar'ı**seçin.
4. Bundan sonra, **Veri** > **Windows Performans Sayaçları'na** gidin ve aşağıdaki sayaçları ekleyin:

    -   LogicalDisk(\*\\) %Boş Alan
    -   LogicalDisk(C:)\\Avg. Disk Sıra Uzunluğu
    -   Bellek(\*\\) Kullanılabilir Mbytes
    -   İşlemci\*Bilgileri( )\\İşlemci Süresi
    -   Oturum Başına Kullanıcı Girişi\*\\Gecikmesi( ) Max Giriş Gecikmesi

Azure Monitor'daki Windows [ve Linux performans veri kaynaklarındaki](/azure/azure-monitor/platform/data-sources-performance-counters)performans sayaçları hakkında daha fazla bilgi edinin.

>[!NOTE]
>Yapılandırmayaptığınız ek sayaçlar tanılama aracının kendisinde gösterilmez. Tanılama aracında görünmesini sağlamak için, aracın config dosyasını yapılandırmanız gerekir. Gelişmiş yönetim ile bunu yapmak için talimatlar daha sonraki bir tarihte GitHub mevcut olacaktır.

## <a name="validate-the-script-results-in-the-azure-portal"></a>Azure portalında komut dosyası sonuçlarını doğrulama

Tanılama aracını dağıtmaya devam etmeden önce, Azure Active Directory uygulamanızın API izinlerine sahip olduğunu ve Log Analytics çalışma alanınızın önceden yapılandırılmış Windows performans sayaçlarına sahip olduğunu doğrulamanızı öneririz.

### <a name="review-your-app-registration"></a>Uygulama kaydınızı inceleyin

Uygulama kaydınızın API izinlerine sahip olduğundan emin olmak için:

1. Bir tarayıcı açın ve yönetim hesabınızla [Azure portalına](https://portal.azure.com/) bağlanın.
2. Azure **Etkin Dizini'ne**gidin.
3. Uygulama **kayıtlarına** gidin ve **Tüm Uygulamaları**seçin.
4. [Azure Active Directory uygulama kaydı oluşturma](deploy-diagnostics.md#create-an-azure-active-directory-app-registration)adım 5'te girdiğiniz uygulama adıyla Azure AD uygulama kaydınızı arayın.

### <a name="review-your-log-analytics-workspace"></a>Günlük Analizi çalışma alanınızı gözden geçirin

Log Analytics çalışma alanınızın önceden yapılandırılmış Windows performans sayaçlarına sahip olduğundan emin olmak için:

1. Azure [portalında,](https://portal.azure.com/)yapılandırılan Windows Performans Sayaçlarını incelemek için **Log Analytics çalışma alanlarına** gidin.
2. **Ayarlar**altında Gelişmiş **ayarları**seçin.
3. Bundan sonra, **Veri** > **Windows Performans Sayaçları**gidin.
4. Aşağıdaki sayaçların önceden yapılandırıldıklarından emin olun:

   - LogicalDisk(\*\\) %Boş Alan: Diskteki toplam kullanılabilir alanın boş alan miktarını yüzde olarak görüntüler.
   - LogicalDisk(C:)\\Avg. Disk Sıra Uzunluğu: C sürücünüz için disk aktarım isteğinin uzunluğu. Değer kısa bir süre için 2'yi geçmemelidir.
   - \*Bellek()\\Kullanılabilir Mbytes: Megabayt sistemi için kullanılabilir bellek.
   - İşlemci\*Bilgileri( )\\İşlemci Süresi: İşlemcinin Boşta olmayan bir iş parçacığı yürütmek için harcadığı geçen süre yüzdesi.
   - Oturum Başına Kullanıcı Girişi\*\\Gecikmesi( ) Max Giriş Gecikmesi

### <a name="connect-to-vms-in-your-log-analytics-workspace"></a>Log Analytics çalışma alanınızda VM'lere bağlanın

VM'lerin durumunu görebilmek için Log Analytics bağlantısını etkinleştirmeniz gerekir. VM'lerinizi bağlamak için aşağıdaki adımları izleyin:

1. Bir tarayıcı açın ve yönetim hesabınızla [Azure portalında](https://portal.azure.com/) oturum açın.
2. Log Analytics Çalışma Alanı'na gidin.
3. Sol panelde, Çalışma Alanı Veri Kaynakları **altında, sanal makineleri**seçin.
4. Bağlanmak istediğiniz VM'nin adını seçin.
5. **Bağlan**’ı seçin.

## <a name="deploy-the-diagnostics-tool"></a>Tanılama aracını dağıtma

Tanılama aracı için Azure Kaynak Yönetimi şablonuna dağıtmak için:

1.   [GitHub Azure RDS-Şablonlar sayfasına](https://github.com/Azure/RDS-Templates/tree/master/wvd-templates/diagnostics-sample/deploy)gidin.
2.  Şablonu Azure'a dağıtın ve şablondaki yönergeleri izleyin. Aşağıdaki bilgilere sahip olduğundan emin olun:

    -   Müşteri Kimliği
    -   Müşteri-Gizli
    -   Log Analytics çalışma alanı kimliği

3.  Giriş parametreleri sağlandıktan sonra, şart ve koşulları kabul edin ve ardından **Satın Al'ı**seçin.

Dağıtım 2-3 dakika sürer. Başarılı dağıtımdan sonra kaynak grubuna gidin ve web uygulaması ve uygulama hizmeti planı kaynaklarının orada olduğundan emin olun.

Bundan sonra, Redirect URI ayarlamak gerekir.

### <a name="set-the-redirect-uri"></a>Uri'yi Yönlendirmeyi Ayarlama

Uri'yi Yönlendirme'yi ayarlamak için:

1.  Azure [portalında,](https://portal.azure.com/) **Uygulama Hizmetleri'ne** gidin ve oluşturduğunuz uygulamayı bulun.
2.  Genel bakış sayfasına gidin ve orada bulduğunuz URL'yi kopyalayın.
3.  Uygulama **kayıtlarına** gidin ve dağıtmak istediğiniz uygulamayı seçin.
4.  Sol panelde, Yönet bölümünde Kimlik **Doğrulama'yı**seçin.
5.  Uri'yi Yeniden Yönlendirme metin kutusuna istediğiniz Yönlendirme **URI'yi** girin ve ardından menünün sol üst köşesinde **Kaydet'i** seçin.
6. Açılan menüde Type'ın altındaki **Web'i** seçin.
7. Uygulamaya genel bakış sayfasından URL'yi girin ve sonuna **/security/signin-callback** ekleyin. Örneğin: `https://<yourappname>.azurewebsites.net/security/signin-callback`.

   ![URI sayfasını yeniden yönlendirme](media/redirect-uri-page.png)

8. Şimdi, Azure kaynaklarınıza gidin, şablonda sağladığınız adla Azure Uygulama Hizmetleri kaynağını seçin ve onunla ilişkili URL'ye gidin. (Örneğin, şablonda kullandığınız uygulama adı `contosoapp45`ise, ilişkili URL'niz dir). <https://contosoapp45.azurewebsites.net>
9. Uygun Azure Active Directory kullanıcı hesabını kullanarak oturum açın.
10.   **Kabul Et**’i seçin.

## <a name="distribute-the-diagnostics-tool"></a>Tanılama aracını dağıtın

Tanılama aracını kullanıcılarınızın kullanımına sunmadan önce, aşağıdaki izinlere sahip olduğundan emin olun:

- Kullanıcıların günlük analitiği için okuma erişimine ihtiyacı vardır. Daha fazla bilgi için Azure [Monitor ile roller, izinler ve güvenlik le başlayın'a](/azure/azure-monitor/platform/roles-permissions-security)bakın.
-  Kullanıcılar ayrıca Windows Sanal Masaüstü kiracı (RDS Reader rolü) için okuma erişimi gerekir. Daha fazla bilgi için [Windows Sanal Masaüstü'nde Temsilci erişimine](delegated-access-virtual-desktop.md)bakın.

Ayrıca kullanıcılarınıza aşağıdaki bilgileri vermeniz gerekir:

- Uygulamanın URL'si
- Erişebilecekleri kiracı grubu bireysel kiracının adları.

## <a name="use-the-diagnostics-tool"></a>Tanılama aracını kullanma

Kuruluşunuzdan aldığınız bilgileri kullanarak hesabınızda oturum açtıktan sonra, etkinlikleri sorgulamak istediğiniz kullanıcı için UPN'yi hazır bulundurun. Bir arama, geçen hafta içinde gerçekleşen belirtilen etkinlik türü altında tüm etkinlikleri size verecektir.

### <a name="how-to-read-activity-search-results"></a>Etkinlik arama sonuçları nasıl okunur?

Etkinlikler zaman damgası ile sıralanır ve önce en son etkinlik yapılır. Sonuçlar bir hata döndürecekse, önce servis hatası olup olmadığını denetleyin. Hizmet hataları için, sorunu hata ayıklamamıza yardımcı olacak etkinlik bilgilerini içeren bir destek bileti oluşturun. Diğer tüm hata türleri genellikle kullanıcı veya yönetici tarafından çözülebilir. En yaygın hata senaryolarının listesi ve bunları nasıl çözeceğiniz için [bkz.](diagnostics-role-service.md#common-error-scenarios)

>[!NOTE]
>Hizmet hataları, bağlı belgelerde "dış hatalar" olarak adlandırılır. PowerShell başvurusunu güncellediğimiz de bu durum değişecektir.

Bağlantı etkinlikleri birden fazla hata olabilir. Kullanıcının karşılaştığı diğer hataları görmek için etkinlik türünü genişletebilirsiniz. Hakkında daha fazla bilgi görmek için bir iletişim kutusu açmak için hata kodunun adını seçin.

### <a name="investigate-the-session-host"></a>Oturum ana bilgisayarını araştırın 

Arama sonuçlarında, hakkında bilgi almak istediğiniz oturum ana bilgisayarını bulun ve seçin.

Oturum ana bilgisayar durumunu analiz edebilirsiniz:

- Önceden tanımlanmış bir eşiğe bağlı olarak, Oturum ana bilgisayar sistem bilgilerini, Log Analytics sorgularını alabilirsiniz.
- Etkinlik olmadığında veya oturum ana bilgisayar Log Analytics'e bağlı değilse, bilgiler kullanılamaz.

Oturum ana bilgisayardaki kullanıcılarla da etkileşimde de olabilirsiniz:

- Oturum açabilir veya oturum açmış kullanıcılara ileti gönderebilirsiniz.
- Başlangıçta aradığınız kullanıcı varsayılan olarak seçilir, ancak aynı anda ileti göndermek veya birden çok kullanıcıyı oturum alabilmek için ek kullanıcılar da seçebilirsiniz.

### <a name="windows-performance-counter-thresholds"></a>Windows performans sayacı eşikleri

- LogicalDisk(\*\\) %Boş Alan:

    - Boş olan mantıksal diskte kullanılabilir toplam alanın yüzdesini görüntüler.
    - Eşik: Az% 20 sağlıksız olarak işaretlenir.

- LogicalDisk(C:)\\Avg. Disk Sıra Uzunluğu:

    - Depolama sistemi koşullarını temsil eder.
    - Eşik: 5'ten yüksek sağlıksız olarak işaretlenir.

- Bellek(\*\\) Kullanılabilir Mbytes:

    - Sistem için kullanılabilir bellek.
    - Eşik: 500 megabayttan az sağlıksız olarak işaretlenmiştir.

- İşlemci\*Bilgileri( )\\İşlemci Süresi:

    - Eşik: %80'den yüksek sağlıksız olarak işaretlenir.

- [Oturum Başına Kullanıcı Girişi\*\\Gecikmesi ( ) Max Giriş Gecikmesi](/windows-server/remote/remote-desktop-services/rds-rdsh-performance-counters/):

    - Eşik: 2000 ms'den yüksek sağlıksız olarak işaretlenir.

## <a name="next-steps"></a>Sonraki adımlar

- [Log Analytics ile Kullanım tanılama'da](diagnostics-log-analytics.md)etkinlik günlüklerini nasıl izleyeceğinizi öğrenin.
- Sık karşılaşılan hata senaryoları ve [bunları Tanımla ve tanılama sorunları](diagnostics-role-service.md)hakkında bilgi edinin.
