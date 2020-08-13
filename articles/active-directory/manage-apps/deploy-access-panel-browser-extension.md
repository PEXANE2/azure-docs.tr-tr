---
title: GPO kullanarak IE için Azure erişim paneli uzantısını dağıtma | Microsoft Docs
description: Uygulamalarım portalı için Internet Explorer eklentisini dağıtmak üzere Grup İlkesi 'ni kullanma.
services: active-directory
documentationcenter: ''
author: kenwith
manager: celestedg
ms.service: active-directory
ms.subservice: app-mgmt
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/08/2018
ms.author: kenwith
ms.reviewer: asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: 31235936109361f0f15a071346baee128060375a
ms.sourcegitcommit: c28fc1ec7d90f7e8b2e8775f5a250dd14a1622a6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2020
ms.locfileid: "88164997"
---
# <a name="how-to-deploy-the-access-panel-extension-for-internet-explorer-using-group-policy"></a>Nasıl yapılır: Grup İlkesi kullanarak Internet Explorer için erişim paneli uzantısını dağıtma

Bu öğreticide, Kullanıcı makinelerinizde Internet Explorer için erişim paneli uzantısını uzaktan yüklemek üzere Grup İlkesi 'nin nasıl kullanılacağı gösterilmektedir. Bu uzantı, [parola tabanlı çoklu oturum açma](sso-options.md#password-based-sso)kullanılarak yapılandırılan uygulamalarda oturum açması gereken Internet Explorer kullanıcıları için gereklidir.

Yöneticilerin bu uzantının dağıtımını otomatikleştirmesini öneririz. Aksi takdirde, kullanıcılar uzantıyı indirip yüklemelidir, bu da Kullanıcı hatasına açıktır ve yönetici izinleri gerektirir. Bu öğreticide, yazılım dağıtımlarını Grup İlkesi kullanarak otomatikleştirmeye yönelik bir yöntem ele alınmaktadır. [Grup ilkesi hakkında daha fazla bilgi edinin.](https://technet.microsoft.com/windowsserver/bb310732.aspx)

Erişim paneli uzantısı, her biri için yönetici izinleri gerektirmeyen [Chrome](https://go.microsoft.com/fwLink/?LinkID=311859) ve [Firefox](https://go.microsoft.com/fwLink/?LinkID=626998)için de kullanılabilir.

## <a name="prerequisites"></a>Ön koşullar

* [Active Directory Domain Services](https://msdn.microsoft.com/library/aa362244%28v=vs.85%29.aspx)ayarlamış ve kullanıcılarınızın makinelerinizi etki alanına katıldıysanız.
* Grup ilkesi nesnesini (GPO) düzenlemek için "Ayarları Düzenle" izninizin olması gerekir. Varsayılan olarak, aşağıdaki güvenlik gruplarının üyeleri bu izne sahiptir: etki alanı yöneticileri, kuruluş yöneticileri ve grup ilkesi Oluşturucu sahipleri. [Daha fazla bilgi edinin.](https://technet.microsoft.com/library/cc781991%28v=ws.10%29.aspx)

## <a name="step-1-create-the-distribution-point"></a>1. Adım: dağıtım noktasını oluşturma

İlk olarak, yükleyici paketini, uzantıyı uzaktan yüklemek istediğiniz makineler tarafından erişilebilen bir ağ konumuna yerleştirmeniz gerekir. Bunu yapmak için şu adımları izleyin:

1. Sunucuda yönetici olarak oturum açın.
1. **Sunucu Yöneticisi** penceresinde, **Dosyalar ve depolama hizmetleri**' ne gidin.

    ![Açık dosyalar ve depolama hizmetleri](./media/deploy-access-panel-browser-extension/files-services.png)

1. **Paylaşımlar** sekmesine gidin. **Görevler**  >  **Yeni paylaşma...** öğesine tıklayın.

    ![Ekran görüntüsü, görevler ekranından yeni paylaşımın nerede bulunacağını gösterir](./media/deploy-access-panel-browser-extension/shares.png)

1. **Yeni paylaşma sihirbazını** tamamlayıp kullanıcılarınızın makinelerinizden erişilebilmesini sağlamak için izinleri ayarlayın. [Paylaşımlar hakkında daha fazla bilgi edinin.](https://technet.microsoft.com/library/cc753175.aspx)
1. Şu Microsoft Windows Installer paketini (. msi dosyası) indirin: [erişim paneli Extension.msi](https://account.activedirectory.windowsazure.com/Applications/Installers/x64/Access%20Panel%20Extension.msi)
1. Yükleyici paketini paylaşımdaki istenen bir konuma kopyalayın.

    ![. Msi dosyasını paylaşıma kopyalayın](./media/deploy-access-panel-browser-extension/copy-package.png)

1. İstemci makinelerinizin yükleyici paketine paylaşımdan erişebildiğini doğrulayın.

## <a name="step-2-create-the-group-policy-object"></a>2. Adım: Grup İlkesi nesnesini oluşturma

1. Active Directory Domain Services (AD DS) yüklemenizi barındıran sunucuda oturum açın.
1. Sunucu Yöneticisi **Araçlar**  >  **Grup İlkesi Yönetim**' e gidin.

    ![Araçlar > grup ilkesi yönetimine git](./media/deploy-access-panel-browser-extension/tools-gpm.png)

1. **Grup İlkesi Yönetimi** penceresinin sol bölmesinde, kuruluş BIRIMI (OU) hiyerarşinizi görüntüleyin ve grup ilkesini hangi kapsamda uygulamak istediğinizi saptayın. Örneğin, test için birkaç kullanıcıya dağıtmak üzere küçük bir OU seçerek veya tüm kuruluşunuza dağıtmak için en üst düzey bir OU seçmeniz gerekebilir.

   > [!NOTE]
   > Kuruluş birimlerinizi (OU) oluşturmak veya düzenlemek istiyorsanız, Sunucu Yöneticisi geri dönün ve **Araçlar**  >  **Active Directory Kullanıcılar ve bilgisayarlar**' a gidin.

1. Bir OU seçtikten sonra, sağ tıklayın ve **Bu etki alanında GPO oluştur ve buraya bağla ' yı seçin...**

    ![Ekran görüntüsü yeni bir GPO oluştur seçeneğini gösterir](./media/deploy-access-panel-browser-extension/create-gpo.png)

1. **Yenı GPO** isteminde yeni Grup İlkesi nesnesi için bir ad yazın.
1. Oluşturduğunuz grup ilkesi nesnesine sağ tıklayın ve **Düzenle**' yi seçin.

## <a name="step-3-assign-the-installation-package"></a>3. Adım: yükleme paketini atama

1. Uzantıyı **bilgisayar yapılandırmasına** veya **Kullanıcı yapılandırmasına**göre dağıtmak isteyip istemediğinizi belirleme. [Bilgisayar yapılandırması](https://technet.microsoft.com/library/cc736413%28v=ws.10%29.aspx)kullanılırken, uzantı bilgisayara hangi kullanıcıların oturum açtığından bağımsız olarak yüklenir. [Kullanıcı Yapılandırması](https://technet.microsoft.com/library/cc781953%28v=ws.10%29.aspx)ile kullanıcıların, oturum açtığından bağımsız olarak, kullanıcılar için yüklenmiş uzantısı vardır.
1. **Grup İlkesi Yönetimi Düzenleyicisi** penceresinin sol bölmesinde, seçtiğiniz yapılandırma türüne bağlı olarak aşağıdaki klasör yollarından birine gidin:

   * `Computer Configuration/Policies/Software Settings/`
   * `User Configuration/Policies/Software Settings/`

1. **Yazılım yükleme**' ye sağ tıkladıktan sonra **Yeni**  >  **paket...** ' i seçin.
1. [1. Adım: dağıtım noktası oluşturma](#step-1-create-the-distribution-point)' dan yükleyici paketini içeren paylaşılan klasöre gidin,. msi dosyasını seçin ve **Aç**' a tıklayın.

   > [!IMPORTANT]
   > Bu aynı sunucuda yer alıyorsa,. msi dosyasına yerel dosya yolu yerine ağ dosya yolu aracılığıyla erişdiğinizi doğrulayın.

    ![Paylaşılan klasörden yükleme paketini seçin](./media/deploy-access-panel-browser-extension/select-package.png)

1. **Yazılım dağıtma** isteminde dağıtım yönteminiz için **atanan** ' ı seçin. Daha sonra, **Tamam**'a tıklayın.

Uzantı artık seçtiğiniz OU 'ya dağıtılır. [Grup İlkesi İle Yazılım Yükleme hakkında daha fazla bilgi edinin.](https://technet.microsoft.com/library/cc738858%28v=ws.10%29.aspx)

## <a name="step-4-auto-enable-the-extension-for-internet-explorer"></a>4. Adım: Internet Explorer için uzantıyı otomatik olarak etkinleştirin

Yükleyiciyi çalıştırmanın yanı sıra Internet Explorer için her uzantının kullanılmadan önce açıkça etkinleştirilmesi gerekir. Grup İlkesi kullanarak erişim paneli uzantısını etkinleştirmek için aşağıdaki adımları izleyin:

1. **Grup İlkesi Yönetimi Düzenleyicisi** penceresinde, [Adım 3: yükleme paketini atama](#step-3-assign-the-installation-package)bölümünde seçtiğiniz yapılandırma türüne bağlı olarak aşağıdaki yollardan birine gidin:

   * `Computer Configuration/Policies/Administrative Templates/Windows Components/Internet Explorer/Security Features/Add-on Management`
   * `User Configuration/Policies/Administrative Templates/Windows Components/Internet Explorer/Security Features/Add-on Management`

1. **Eklenti listesi**' ne sağ tıklayın ve **Düzenle**' yi seçin.

    !["Eklenti listesi" ne sağ tıklayın ve "Düzenle" yi seçin](./media/deploy-access-panel-browser-extension/edit-add-on-list.png)

1. **Eklenti listesi** penceresinde, **etkin**' i seçin. Ardından, **Seçenekler** bölümünün altında **göster...**' e tıklayın.

    ![Etkinleştir ' e ve ardından göster... öğesine tıklayın.](./media/deploy-access-panel-browser-extension/edit-add-on-list-window.png)

1. **Içeriği göster** penceresinde aşağıdaki adımları uygulayın:

   1. İlk sütun ( **değer adı** alanı) Için AŞAĞıDAKI sınıf kimliğini kopyalayıp yapıştırın:`{030E9A3F-7B18-4122-9A60-B87235E4F59E}`
   1. İkinci sütun ( **değer** alanı) için aşağıdaki değeri yazın:`1`
   1. **Tamam** ' a tıklayarak **içeriği göster** penceresini kapatın.

      ![Önceki adımda belirtilen değerleri doldurun](./media/deploy-access-panel-browser-extension/show-contents.png)

1. Değişikliklerinizi uygulamak için **Tamam** ' ı tıklatın ve **eklenti listesi** penceresini kapatın.

Uzantı, seçilen OU 'daki makineler için artık etkinleştirilmelidir. [Internet Explorer eklentilerini etkinleştirmek veya devre dışı bırakmak için Grup İlkesi kullanma hakkında daha fazla bilgi edinin.](https://technet.microsoft.com/library/dn454941.aspx)

## <a name="step-5-optional-disable-remember-password-prompt"></a>5. adım (Isteğe bağlı): "parolayı anımsa" uyarısını devre dışı bırak

Kullanıcılar, erişim paneli uzantısını kullanarak Web sitelerinde oturum açtığında, Internet Explorer "parolanızı depolamak Ister misiniz?" uyarısını aşağıdaki istemde gösterebilir.

!["Parolanızı depolamak istiyor musunuz..." isteme](./media/deploy-access-panel-browser-extension/remember-password-prompt.png)

Kullanıcılarınızın bu istemi görmesini engellemek isterseniz, parolaları hatırlayıp otomatik tamamlamayı engellemek için aşağıdaki adımları izleyin:

1. **Grup İlkesi Yönetimi Düzenleyicisi** penceresinde, aşağıda listelenen yola gidin. Bu yapılandırma ayarı yalnızca **Kullanıcı Yapılandırması**altında kullanılabilir.

   * `User Configuration/Policies/Administrative Templates/Windows Components/Internet Explorer/`
1. **Formlardaki kullanıcı adları ve parolalar için otomatik tamamlamayı aç**adlı ayarı bulun.

   > [!NOTE]
   > Active Directory önceki sürümleri, bu ayarın adı, **parolaların kaydedileceği otomatik tamamlamayı izin vermiyor**olarak listeleyebilir. Bu ayarın yapılandırması, bu öğreticide açıklanan ayardan farklıdır.

    ![Kullanıcı ayarları altında bunu aramaya anımsa](./media/deploy-access-panel-browser-extension/disable-auto-complete.png)

1. Yukarıdaki ayara sağ tıklayın ve **Düzenle**' yi seçin.
1. **Formlardaki kullanıcı adları ve parolalar için otomatik tamamlamayı aç**başlıklı pencerede **devre dışı**' yı seçin.

    ![Otomatik tamamlamayı aç özelliğini etkinleştirmek için "devre dışı" seçeneğini belirleyin](./media/deploy-access-panel-browser-extension/disable-passwords.png)

1. Bu değişiklikleri uygulamak ve pencereyi kapatmak için **Tamam** ' ı tıklatın.

Kullanıcılar artık kimlik bilgilerini depolayamayacak veya önceden depolanmış kimlik bilgilerine erişmek için otomatik tamamlamayı kullanamaz. Ancak, bu ilke kullanıcıların arama alanları gibi diğer form alanı türleri için otomatik tamamlamayı kullanmaya devam etmesine izin verir.

> [!WARNING]
> Kullanıcılar bazı kimlik bilgilerini depolamayı seçtikten sonra bu ilke etkinleştirildiyse, bu ilke zaten depolanmış olan kimlik bilgilerini *temizlemez* .

## <a name="step-6-testing-the-deployment"></a>6. Adım: dağıtımı test etme

Uzantı dağıtımının başarılı olup olmadığını doğrulamak için aşağıdaki adımları izleyin:

1. **Bilgisayar yapılandırması**kullanarak dağıttıysanız [2. Adım: Grup İlkesi nesnesini oluşturma](#step-2-create-the-group-policy-object)bölümünde seçtiğiniz OU 'ya ait bir istemci makinesinde oturum açın. **Kullanıcı Yapılandırması**kullanarak dağıttıysanız, bu OU 'ya ait olan bir kullanıcı olarak oturum açmayı unutmayın.
1. Grup İlkesi değişikliklerinin bu makineyle tamamen güncelleştirilmesi için birkaç oturum açma işlemleri gerekebilir. Güncelleştirmeyi zorlamak için bir **komut istemi** penceresi açın ve aşağıdaki komutu çalıştırın:`gpupdate /force`
1. Yüklemenin gerçekleşmesi için makineyi yeniden başlatmanız gerekir. Uzantı yüklenirken, önyükleme normalden çok daha fazla zaman alabilir.
1. Yeniden başlattıktan sonra **Internet Explorer**'ı açın. Pencerenin sağ üst köşesinde **Araçlar** ' a (dişli simgesi) tıklayın ve ardından Eklentileri **Yönet**' i seçin.
1. **Eklentileri Yönet** penceresinde, **erişim paneli uzantısının** yüklendiğini ve **durumunun** **etkin**olarak ayarlandığını doğrulayın.

   ![Access panel uzantısının yüklü ve etkin olduğunu doğrulama](./media/deploy-access-panel-browser-extension/verify-install.png)

## <a name="learn-more"></a>Daha fazla bilgi edinin

* [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma](what-is-single-sign-on.md)
* [Internet Explorer için erişim paneli uzantısı sorunlarını giderme](manage-access-panel-browser-extension.md)
