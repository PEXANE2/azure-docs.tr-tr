---
title: GPO kullanarak IE için Azure Erişim Paneli Uzantısını Dağıtma | Microsoft Dokümanlar
description: Uygulamalarım portalı için Internet Explorer eklentisini dağıtmak için grup ilkesini kullanma.
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/08/2018
ms.author: mimart
ms.reviewer: asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: 71c342ede77349b3f6c22093e5877ad5f5ce6549
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67807677"
---
# <a name="how-to-deploy-the-access-panel-extension-for-internet-explorer-using-group-policy"></a>Nasıl yapılır: Grup ilkesini kullanarak Internet Explorer için Erişim Masası uzantısını dağıtma

Bu öğretici, Internet Explorer için Erişim Masası uzantısını kullanıcılarınızın makinelerine uzaktan yüklemek için grup ilkesinin nasıl kullanılacağını gösterir. Bu uzantı, [parola tabanlı tek oturum](what-is-single-sign-on.md#password-based-sso)açma kullanılarak yapılandırılan uygulamalarda oturum açmanız gereken Internet Explorer kullanıcıları için gereklidir.

Yöneticilerin bu uzantının dağıtımını otomatikleştirmesi önerilir. Aksi takdirde, kullanıcıların kullanıcı hatasına yatkın olan ve yönetici izinleri gerektiren uzantıyı kendileri karşıdan yüklemeleri ve yüklemeleri gerekir. Bu öğretici, grup ilkesini kullanarak yazılım dağıtımlarını otomatikleştirmek için kullanılan bir yöntemi kapsar. [Grup ilkesi hakkında daha fazla bilgi edinin.](https://technet.microsoft.com/windowsserver/bb310732.aspx)

Erişim Masası [uzantısı, yüklemek](https://go.microsoft.com/fwLink/?LinkID=311859) için yönetici izni gerektirmeyen Chrome ve [Firefox](https://go.microsoft.com/fwLink/?LinkID=626998)için de kullanılabilir.

## <a name="prerequisites"></a>Ön koşullar

* [Active Directory Domain Services'ı](https://msdn.microsoft.com/library/aa362244%28v=vs.85%29.aspx)kurdunuz ve kullanıcılarınızın makinelerini etki alanınıza katıldınız.
* Grup İlkesi Nesnesini (GPO) yeniden atamak için "Ayarları düzelt" iznine sahip olmalısınız. Varsayılan olarak, aşağıdaki güvenlik gruplarının üyeleri şu izne sahiptir: Etki Alanı Yöneticileri, Kurumsal Yöneticiler ve Grup İlkesi Oluşturucusu Sahipleri. [Daha fazla bilgi edinin.](https://technet.microsoft.com/library/cc781991%28v=ws.10%29.aspx)

## <a name="step-1-create-the-distribution-point"></a>Adım 1: Dağıtım noktasını oluşturma

İlk olarak, yükleyici paketini uzantıyı uzaktan yüklemek istediğiniz makineler tarafından erişilebilen bir ağ konumuna yerleştirmeniz gerekir. Bunu yapmak için şu adımları uygulayın:

1. Sunucuda yönetici olarak oturum açın.
1. Sunucu **Yöneticisi** penceresinde, **Dosyalar ve Depolama Hizmetleri'ne**gidin.

    ![Dosyaları ve Depolama Hizmetlerini Aç](./media/deploy-access-panel-browser-extension/files-services.png)

1. **Paylaşımlar** sekmesine gidin. Ardından **Görevler** > **Yeni Paylaşım'ı tıklatın...**

    ![Ekran görüntüsü, Görevler ekranından Yeni Paylaşım'ın nerede bulunacağını gösterir](./media/deploy-access-panel-browser-extension/shares.png)

1. Yeni **Paylaşım Sihirbazı'nı** tamamlayın ve kullanıcılarınızın makinelerinden erişilebilmesini sağlamak için izinler ayarlayın. [Paylaşımlar hakkında daha fazla bilgi edinin.](https://technet.microsoft.com/library/cc753175.aspx)
1. Aşağıdaki Microsoft Windows Installer paketini indirin (.msi dosyası): [Access Panel Extension.msi](https://account.activedirectory.windowsazure.com/Applications/Installers/x64/Access%20Panel%20Extension.msi)
1. Yükleyici paketini paylaşımda istenilen konuma kopyalayın.

    ![.msi dosyasını paylaşıma kopyalama](./media/deploy-access-panel-browser-extension/copy-package.png)

1. İstemci makinelerinizin yükleyici paketine paylaşımdan erişebildiğini doğrulayın.

## <a name="step-2-create-the-group-policy-object"></a>Adım 2: Grup ilkesi nesnesini oluşturma

1. Active Directory Domain Services (AD DS) yüklemenizi barındıran sunucuda oturum açın.
1. Sunucu Yöneticisi'nde, **Tools** > **Group İlke Yönetimi'ne**gidin.

    ![Grup Politika Yönetimi > Araçlara Git](./media/deploy-access-panel-browser-extension/tools-gpm.png)

1. **Grup İlkesi Yönetimi** penceresinin sol bölmesinde, Kuruluş Birimi (OU) hiyerarşinizi görüntüleyin ve grup ilkesini hangi kapsamda uygulamak istediğinizi belirleyin. Örneğin, sınama için birkaç kullanıcıya dağıtmak için küçük bir OU seçmeye karar verebilir veya tüm kuruluşunuza dağıtmak için üst düzey bir OU seçebilirsiniz.

   > [!NOTE]
   > Kuruluş Birimlerinizi (OS) oluşturmak veya değiştirmek istiyorsanız, Server Manager'a geri dön ve **Tools** > **Active Directory Users and Computers'a**gidin.

1. Ou seçtikten sonra, sağ tıklayın ve **bu etki alanında bir GPO oluştur'u seçin ve buraya bağla...**

    ![Ekran görüntüsü yeni bir GPO Oluştur seçeneğini gösterir](./media/deploy-access-panel-browser-extension/create-gpo.png)

1. Yeni **GPO** isteminde, yeni Grup İlkesi Nesnesi için bir ad yazın.
1. Oluşturduğunuz Grup İlkesi Nesnesi'ni sağ tıklatın ve **Edit'i**seçin.

## <a name="step-3-assign-the-installation-package"></a>Adım 3: Yükleme paketini atama

1. Uzantıyı **Bilgisayar Yapılandırmasına** veya Kullanıcı **Yapılandırmasına**göre dağıtmak isteyip istemeyeceğiniz belirlendir. Bilgisayar [yapılandırması](https://technet.microsoft.com/library/cc736413%28v=ws.10%29.aspx)kullanırken, uzantı, hangi kullanıcının oturum açmasından bağımsız olarak bilgisayara yüklenir. [Kullanıcı yapılandırması](https://technet.microsoft.com/library/cc781953%28v=ws.10%29.aspx)ile, kullanıcılar hangi bilgisayarlarda oturum açtıklarına bakılmaksızın uzantıyı kendileri için yüklü dür.
1. **Grup İlkesi Yönetimi Düzenleyicisi** penceresinin sol bölmesinde, seçtiğiniz yapılandırma türüne bağlı olarak aşağıdaki klasör yollarından hangisine gidin:

   * `Computer Configuration/Policies/Software Settings/`
   * `User Configuration/Policies/Software Settings/`

1. Sağ tıklayın **Yazılım yükleme,** sonra **Yeni** > Paket seçin **...**
1. Adım 1'den yükleyici paketini içeren paylaşılan klasöre [gidin: Dağıtım Noktası nı oluşturun,](#step-1-create-the-distribution-point).msi dosyasını seçin ve **Aç'ı**tıklatın.

   > [!IMPORTANT]
   > Paylaşım aynı sunucuda bulunuyorsa, .msi'ye yerel dosya yolu yerine ağ dosyası yolu üzerinden eriştinettiğinizi doğrulayın.

    ![Paylaşılan klasörden yükleme paketini seçin](./media/deploy-access-panel-browser-extension/select-package.png)

1. Yazılım **Dağıt** komut isteminde, dağıtım yönteminiz için **Atanmış'ı** seçin. Ardından **Tamam**'a tıklayın.

Uzantı artık seçtiğiniz OU'ya dağıtılır. [Grup İlkesi Yazılım Yükleme si hakkında daha fazla bilgi edinin.](https://technet.microsoft.com/library/cc738858%28v=ws.10%29.aspx)

## <a name="step-4-auto-enable-the-extension-for-internet-explorer"></a>Adım 4: Internet Explorer için uzantıyı otomatik olarak etkinleştirin

Yükleyiciyi çalıştırmanın yanı sıra, Internet Explorer için her uzantı nın kullanılabilmesi için açıkça etkinleştirilmesi gerekir. Grup ilkesini kullanarak Access Panel Uzantısını etkinleştirmek için aşağıdaki adımları izleyin:

1. Grup **İlkesi Yönetimi Düzenleyicisi** penceresinde, Adım 3'te hangi yapılandırma türünü seçtiğinize bağlı olarak aşağıdaki yollardan [hangisine gidin: Yükleme Paketini atama:](#step-3-assign-the-installation-package)

   * `Computer Configuration/Policies/Administrative Templates/Windows Components/Internet Explorer/Security Features/Add-on Management`
   * `User Configuration/Policies/Administrative Templates/Windows Components/Internet Explorer/Security Features/Add-on Management`

1. **Sağ tıkla Ekle Listesi**ve **Edit'i**seçin.

    ![Sağ tıklayın "Eklenti Listesi" ve "Edit" seçin](./media/deploy-access-panel-browser-extension/edit-add-on-list.png)

1. Eklenti **Listesi** penceresinde **Etkin'i**seçin. Ardından, **Seçenekler** bölümünün altında **Göster......** seçeneğini tıklatın.

    ![Etkinleştir'i tıklatın, ardından Göster'i tıklatın...](./media/deploy-access-panel-browser-extension/edit-add-on-list-window.png)

1. İçeriği **Göster** penceresinde aşağıdaki adımları gerçekleştirin:

   1. İlk sütun **(Değer Adı** alanı) için aşağıdaki Sınıf Kimliğini kopyalayın ve yapıştırın:`{030E9A3F-7B18-4122-9A60-B87235E4F59E}`
   1. İkinci sütun **(Değer** alanı) için aşağıdaki değeri yazın:`1`
   1. **İçeriği Göster** penceresini kapatmak için **Tamam'ı** tıklatın.

      ![Önceki adımda belirtilen değerleri doldurun](./media/deploy-access-panel-browser-extension/show-contents.png)

1. Değişikliklerinizi uygulamak ve Eklenti Listesi penceresini kapatmak için **Tamam'ı** tıklatın. **OK**

Uzantı şimdi seçili OU'daki makineler için etkinleştirilmelidir. [Internet Explorer eklentilerini etkinleştirmek veya devre dışı katmak için grup ilkesini kullanma hakkında daha fazla bilgi edinin.](https://technet.microsoft.com/library/dn454941.aspx)

## <a name="step-5-optional-disable-remember-password-prompt"></a>Adım 5 (İsteğe bağlı): "Parolayı Hatırla" istemini devre dışı

Kullanıcılar Access Panel Uzantısı'nı kullanarak web sitelerinde oturum açtıklarında, Internet Explorer aşağıdaki istem 'Parolanızı saklamak ister misiniz?" diye soran bir istem gösterebilir.

!["Parolanızı saklamak ister misiniz..." Istemi](./media/deploy-access-panel-browser-extension/remember-password-prompt.png)

Kullanıcılarınızın bu istemi görmesini engellemek istiyorsanız, otomatik olarak parolaları hatırlamaktan korunmak için aşağıdaki adımları izleyin:

1. Grup **İlkesi Yönetimi Düzenleyicisi** penceresinde, aşağıda listelenen yola gidin. Bu yapılandırma ayarı yalnızca **Kullanıcı Yapılandırması**altında kullanılabilir.

   * `User Configuration/Policies/Administrative Templates/Windows Components/Internet Explorer/`
1. **Formlarda kullanıcı adları ve parolalar için otomatik tamamlama özelliğini aç**adlı ayarı bulun.

   > [!NOTE]
   > Active Directory önceki sürümlerinde ad ile bu ayarı listeleyebilir **otomatik şifreleri kaydetmek için otomatik olarak tam izin vermez.** Bu ayarın yapılandırması, bu öğreticide açıklanan ayardan farklıdır.

    ![Kullanıcı Ayarları altında bunu aramayı unutmayın](./media/deploy-access-panel-browser-extension/disable-auto-complete.png)

1. Yukarıdaki ayarı sağ tıklatın ve **Edit'i**seçin.
1. **Formlar üzerindeki kullanıcı adları ve parolalar için otomatik tamamlama özelliğini aç**başlıklı pencerede **Devre Dışı' yı**seçin.

    ![Otomatik tamamlama özelliğini açmak için "Devre Dışı" seçeneğini belirleyin](./media/deploy-access-panel-browser-extension/disable-passwords.png)

1. Bu değişiklikleri uygulamak ve pencereyi kapatmak için **Tamam'ı** tıklatın.

Kullanıcılar artık kimlik bilgilerini depolayamayacak veya daha önce depolanan kimlik bilgilerine erişmek için otomatik tamamlama yı kullanamayacak. Ancak, bu ilke kullanıcıların arama alanları gibi diğer form alanları türleri için otomatik tamamlama kullanmaya devam etmesine olanak sağlar.

> [!WARNING]
> Kullanıcılar bazı kimlik bilgilerini depolamayı seçtikten sonra bu ilke etkinleştirilirse, bu ilke zaten depolanmış olan kimlik bilgilerini *temizlemez.*

## <a name="step-6-testing-the-deployment"></a>Adım 6: Dağıtımı test etme

Uzantı dağıtımının başarılı olup olmadığını doğrulamak için aşağıdaki adımları izleyin:

1. **Bilgisayar Yapılandırması'nı**kullanarak dağıttıysanız, Adım 2'de seçtiğiniz OU'ya ait bir istemci makinesinde oturum [açın: Grup İlkesi Nesnesini Oluşturun.](#step-2-create-the-group-policy-object) **Kullanıcı Yapılandırması'nı**kullanarak dağıttıysanız, o OU'ya ait bir kullanıcı olarak oturum açtığınızdan emin olun.
1. Grup ilkesi değişikliklerinin bu makineyle tam olarak güncelleştirilen birkaç oturum açma alması gerekebilir. Güncelleştirmeyi zorlamak için Komut **İstemi** penceresini açın ve aşağıdaki komutu çalıştırın:`gpupdate /force`
1. Yüklemenin gerçekleşmesi için makineyi yeniden başlatmanız gerekir. Önyükleme, uzantı yüklenirken normalden çok daha fazla zaman alabilir.
1. Yeniden başladıktan sonra **Internet Explorer'ı**açın. Pencerenin sağ üst köşesinde **Araçlar'ı** (vites simgesi) tıklatın ve ardından **eklentileri yönet'i**seçin.
1. **Eklentileri Yönet** penceresinde, Access Panel **Uzantısı'nın** yüklendiğini ve **Durumunun** **Etkin**olarak ayarlandığını doğrulayın.

   ![Access Panel Uzantısı'nın yüklü ve etkin olduğunu doğrulayın](./media/deploy-access-panel-browser-extension/verify-install.png)

## <a name="learn-more"></a>Daha fazla bilgi edinin

* [Azure Active Directory ile uygulama erişimi ve tek oturum açma](what-is-single-sign-on.md)
* [Internet Explorer için Erişim Masası Uzantısısorun Giderme](manage-access-panel-browser-extension.md)
