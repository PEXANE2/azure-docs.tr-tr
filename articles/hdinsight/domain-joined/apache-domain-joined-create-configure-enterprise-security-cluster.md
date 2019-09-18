---
title: Azure HDInsight 'ta Kurumsal Güvenlik Paketi kümeleri oluşturma ve yapılandırma
description: Azure HDInsight 'ta Kurumsal Güvenlik Paketi kümelerini oluşturma ve yapılandırma hakkında bilgi edinin
services: hdinsight
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.topic: conceptual
ms.date: 05/09/2019
ms.openlocfilehash: 482e3e534486a226783e2377c10b074a087e67ed
ms.sourcegitcommit: 0fab4c4f2940e4c7b2ac5a93fcc52d2d5f7ff367
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/17/2019
ms.locfileid: "71037312"
---
# <a name="create-and-configure-enterprise-security-package-clusters-in-azure-hdinsight"></a>Azure HDInsight 'ta Kurumsal Güvenlik Paketi kümeleri oluşturma ve yapılandırma

Azure HDInsight için Kurumsal Güvenlik Paketi, Azure 'daki Apache Hadoop kümeleriniz için Active Directory tabanlı kimlik doğrulamasına, çok kullanıcılı desteğe ve rol tabanlı erişim denetimine erişmenizi sağlar. HDInsight ESP kümeleri, hassas verileri güvenli bir şekilde işlemek için sıkı kurumsal güvenlik ilkelerine bağlı olan kuruluşları etkinleştirir.

Bu kılavuzun amacı, şirket içi kullanıcıların bir ESP etkin HDInsight kümesinde oturum açmasını sağlamak için gerekli kaynakları doğru şekilde yapılandırmaktır. Bu makalede Kurumsal Güvenlik Paketi etkinleştirilmiş bir Azure HDInsight kümesi oluşturmak için gereken adımlar açıklanmaktadır. Adımlar, Active Directory & etki alanı adı hizmetleri (DNS) etkin bir Windows IaaS VM 'si oluşturmayı kapsar. Bu sunucu, **gerçek** şirket içi ortamınızın yerini alacak ve daha sonra kendi ortamınızda tekrarlamanız için kurulum ve yapılandırma adımlarında ilerleyebilmenizi sağlayacak. Bu kılavuz Ayrıca, Azure Active Directory ile parola karması eşitlemesini kullanarak bir karma kimlik ortamı oluşturmanıza yardımcı olur.

Bu kılavuz, [HDInsight 'ta kurumsal güvenlik paketi kullanımını](apache-domain-joined-architecture.md) tamamlamak içindir

Bu işlemi kendi ortamınızda kullanmadan önce, Active Directory ve etki alanı adı hizmetleri (DNS) kurun. Ayrıca, Azure Active Directory etkinleştirin ve şirket içi kullanıcı hesaplarını Azure Active Directory ile eşitleyin.

![Azure Active Directory mimari diyagramı](./media/apache-domain-joined-create-configure-enterprise-security-cluster/hdinsight-image-0002.png)

## <a name="create-on-premises-environment"></a>Şirket içi ortam oluşturma

Genel Bakış: Bu bölümde, yeni VM 'Ler oluşturmak, etki alanı adı hizmetleri 'ni (DNS) ve yeni bir AD ormanını yapılandırmak için bir Azure hızlı dağıtım şablonu kullanacaksınız.

1. Hızlı Dağıtım şablonunu görüntülemek için [Yeni BIR ad ormanı Ile Azure VM oluşturma](https://azure.microsoft.com/resources/templates/active-directory-new-domain/)bölümüne gidin.

1. **Azure 'A dağıt**' a tıklayın.
1. Azure aboneliğinizde oturum açın.
1. Yeni bir **ad ormanı Ile Azure VM oluşturma** ekranında, aşağıdaki adımları izleyin:
    1. Kaynakların, **abonelik** açılan menüsünden dağıtılmasını istediğiniz aboneliği seçin.
    1. **Kaynak grubu** ' nun yanındaki **Yeni oluştur** ' u seçin ve **onpremadvrg** adını girin
    1. Şablon alanlarının geri kalanı için aşağıdaki ayrıntıları girin:

        * **Konum**: Orta ABD
        * **Yönetici Kullanıcı adı**: HDIFabrikamAdmin
        * **Yönetici parolası**: < YOUR_PASSWORD >
        * **Etki alanı**: HDIFabrikam.com
        * **DNS ön eki**: hdıfabrikam

        ![Şablon Azure VM ve AD Ormanı oluşturma](./media/apache-domain-joined-create-configure-enterprise-security-cluster/create-azure-vm-ad-forest.png)

    1. **Satın al** 'a tıklayın
    1. Dağıtımı izleyin ve tamamlanmasını bekleyin.
    1. Kaynakların doğru kaynak grubu `OnPremADVRG`altında oluşturulduğunu onaylayın.

## <a name="configure-users-and-groups-for-cluster-access"></a>Küme erişimi için kullanıcıları ve grupları yapılandırma

Genel Bakış: Bu bölümde, bu kılavuzun sonuna kadar HDInsight kümesine erişimi olacak kullanıcıları oluşturacaksınız.

1. Uzak Masaüstü kullanarak etki alanı denetleyicisine bağlanın.
    1. Başında bahsedilen şablonu kullandıysanız, etki alanı denetleyicisi `OnPremADVRG` kaynak grubunda **advm** adlı bir sanal makine olur.
    1. **Onpremadvrg** >  > **advm** > **Connect**Azure Portal > **kaynak gruplarına**gidin.
    1. **RDP** sekmesine tıklayın ve ardından **RDP dosyasını indir**' e tıklayın.
    1. Dosyayı bilgisayarınıza kaydedin ve açın.
    1. Kimlik bilgileri istendiğinde Kullanıcı adı olarak `HDIFabrikam\HDIFabrikamAdmin` kullanın ve ardından yönetici hesabı için seçtiğiniz parolayı girin.

1. Etki alanı denetleyicisi VM 'sinde Uzak Masaüstü oturumunuz açıldıktan sonra, **Sunucu Yöneticisi** panosundan **Active Directory Kullanıcıları ve bilgisayarları** başlatın. Sağ üstteki **Araçlar** ' a tıklayın ve ardından açılır listeden **kullanıcılar ve bilgisayarlar Active Directory** .

    ![Sunucu Yöneticisi Active Directory yönetimini aç](./media/apache-domain-joined-create-configure-enterprise-security-cluster/server-manager-active-directory-screen.png)

1. **Hdiadmin**, **hdiuser**olmak üzere iki yeni kullanıcı oluşturun. Bu iki Kullanıcı, HDInsight kümelerinde oturum açmak için kullanılacaktır.

    1. **Active Directory Kullanıcıları ve bilgisayarları** ekranında,**Yeni** > **Kullanıcı** **eylem** > ' e tıklayın.

        ![Yeni Active Directory kullanıcı oluştur](./media/apache-domain-joined-create-configure-enterprise-security-cluster/create-active-directory-user.png)

    1. **Yeni nesne-Kullanıcı** ekranında `HDIUser` **Kullanıcı oturum açma adı** olarak girin ve **İleri**' ye tıklayın.

        ![İlk yönetici kullanıcı nesnesi oluştur](./media/apache-domain-joined-create-configure-enterprise-security-cluster/hdinsight-image-0020.png)

    1. Görüntülenen açılan pencerede, yeni hesap için istenen parolayı girin. **Parolanın hiç zaman aşımına**uğramadığını belirten kutuyu işaretleyin. HDIClick **Tamam**.
    1. Yeni hesabı oluşturmak için **son** ' a tıklayın.
    1. Başka bir Kullanıcı `HDIAdmin`oluşturun.

        ![İkinci Yönetici Kullanıcı nesnesi oluştur](./media/apache-domain-joined-create-configure-enterprise-security-cluster/hdinsight-image-0024.png)

1. **Active Directory Kullanıcıları ve bilgisayarları** ekranında **eylem** > **Yeni** > **Grup**' a tıklayın. Yeni `HDIUserGroup` bir grup olarak oluşturun.

    ![Yeni Active Directory grubu oluştur](./media/apache-domain-joined-create-configure-enterprise-security-cluster/create-active-directory-group.png)

    ![Yeni nesne grubu oluştur iletişim kutusu](./media/apache-domain-joined-create-configure-enterprise-security-cluster/hdinsight-image-0028.png)

1. Önceki adımda oluşturulan **Hdiuser** öğesini üye olarak **HDIUserGroup** ekleyin.

    1. **HDIUserGroup** öğesine sağ tıklayın ve **Özellikler**' e tıklayın.
    1. **Üyeler** sekmesine gidin ve **Ekle**' ye tıklayın.
    1. `HDIUser` **Seçilecek nesne adlarını girin** etiketli kutuya girin ve **Tamam**' a tıklayın.
    1. Diğer hesap için önceki adımları yineleyin`HDIAdmin`

        ![HDIUserGroup grubuna üye HDIUser Ekle](./media/apache-domain-joined-create-configure-enterprise-security-cluster/active-directory-add-users-to-group.png)

Active Directory ortamınızı, HDInsight kümesine erişmek için iki Kullanıcı ve bir kullanıcı grubuyla birlikte oluşturdunuz.

Bu kullanıcılar, Azure AD ile eşitlenecek.

### <a name="create-a-new-azure-active-directory"></a>Yeni bir Azure Active Directory oluştur

1. Azure Portal’da oturum açın.
1. **Kaynak oluştur** ve **Dizin**yaz ' a tıklayın. **Azure Active Directory** > **Oluştur**' u seçin.
1. **Kuruluş adı**altında **hdıfabrikam** girin.
1. **İlk etki alanı adı**bölümüne **HDIFabrikamoutlook** girin.
1. **Oluştur**'a tıklayın.
1. Azure portal sol tarafta **Azure Active Directory**' a tıklayın.
1. Gerekirse, **HDIFabrikamoutlook**oluşturduğunuz yeni dizine geçmek için **dizini** Değiştir ' e tıklayın.
1. **Yönet** altında **özel etki alanı adları** > **özel etki alanı Ekle**' ye tıklayın.
1. **Özel etki alanı adı** bölümüne **HDIFabrikam.com** girin ve **etki alanı Ekle**' ye tıklayın.

![Yeni bir Azure Active Directory oluşturun](./media/apache-domain-joined-create-configure-enterprise-security-cluster/create-new-directory.png)

![Azure AD, yeni bir özel etki alanı oluşturun](./media/apache-domain-joined-create-configure-enterprise-security-cluster/create-custom-domain.png)

## <a name="configure-your-azure-ad-tenant"></a>Azure AD kiracınızı yapılandırma

Genel Bakış: Artık Azure AD kiracınızı, şirket içi AD 'den buluta Kullanıcı ve grupları eşitleyebilmeniz için yapılandıracaksınız.

1. Bir AD kiracı yöneticisi oluşturun.
    1. Azure portal oturum açın ve Azure AD kiracınızı seçin **Hdifabrikam**
    1. **Yönet** altında **Kullanıcılar** ' ı ve sonra **Yeni Kullanıcı**' yı seçin.
    1. Yeni Kullanıcı için aşağıdaki ayrıntıları girin:

        * Ad: fabrikamazureadmin
        * Kullanıcı adı:fabrikamazureadmin@hdifabrikam.com
        * Parola: tercih ettiğiniz güvenli bir parola

    1. **Gruplar** bölümüne tıklayın, **AAD DC yöneticileri**Için arama yapın ve **Seç**' e tıklayın.

        ![Azure Active Directory grupları iletişim kutusu](./media/apache-domain-joined-create-configure-enterprise-security-cluster/hdinsight-image-0038.png)

    1. **Dizin rolü** bölümüne tıklayın ve sağ taraftaki **genel yönetici** ' yi seçin. **Tamam**’a tıklayın.

        ![Azure Active Directory rolü iletişim kutusu](./media/apache-domain-joined-create-configure-enterprise-security-cluster/hdinsight-image-0040.png)

    1. Kullanıcı için bir parola girin. **Oluştur**'a tıklayın.

1. Yeni oluşturulan kullanıcının <fabrikamazureadmin@hdifabrikam.com>parolasını değiştirmek istiyorsanız. Kimliği kullanarak Azure portal oturum açın ve parolayı değiştirmeniz istenir.

## <a name="sync-on-premises-users-to-azure-ad"></a>Şirket içi kullanıcıları Azure AD ile eşitleme

### <a name="download-and-install-microsoft-azure-active-directory-connect"></a>Microsoft Azure Active Directory Bağlan ve yükle

1. [Azure AD Connect indirin](https://www.microsoft.com/download/details.aspx?id=47594).

1. Etki alanı denetleyicisine Install Microsoft Azure Active Directory Connect.

    1. Önceki adımda indirdiğiniz yürütülebilir dosyayı açın ve lisans koşullarını kabul edin. **Devam**'a tıklayın.

        ![Azure AD Connect iletişim kutusuna hoş geldiniz](./media/apache-domain-joined-create-configure-enterprise-security-cluster/hdinsight-image-0052.png)

    1. **Hızlı ayarları kullan** ' a tıklayın ve yüklemeyi doldurun.

        ![Azure AD Connect Express ayarları](./media/apache-domain-joined-create-configure-enterprise-security-cluster/hdinsight-image-0054.png)

### <a name="configure-sync-with-on-premises-domain-controller"></a>Şirket içi etki alanı denetleyicisi ile eşitlemeyi yapılandırma

1. **Azure AD 'ye Bağlan** EKRANıNDA Azure AD için genel yöneticinin kullanıcı adını ve parolasını girin.  **İleri**' ye tıklayın. Bu, AD kiracınızı yapılandırırken oluşturduğunuz kullanıcı adıdır `fabrikamazureadmin@hdifabrikam.com` .

    ![Azure Active Directory'e ](./media/apache-domain-joined-create-configure-enterprise-security-cluster/hdinsight-image-0058.png)

1. **Active Directory Domain Services Bağlan** ekranında, bir kurumsal yönetici hesabının kullanıcı adını ve parolasını girin.  **İleri**' ye tıklayın. Bu, daha önce `HDIFabrikam\HDIFabrikamAdmin` oluşturduğunuz Kullanıcı adı ve eşleşen paroladır.

   ![Active Directory Domain Services Bağlan](./media/apache-domain-joined-create-configure-enterprise-security-cluster/hdinsight-image-0060.png)
1. **Azure AD oturum açma yapılandırması** sayfasında **İleri**' ye tıklayın.
    ![Azure AD oturum açma yapılandırması](./media/apache-domain-joined-create-configure-enterprise-security-cluster/hdinsight-image-0062.png)

1. Yapılandırmaya hazırlanma ekranında, **yüklensin**' e tıklayın.

    ![Azure AD Connect yapılandırmaya hazırlanma iletişim kutusu](./media/apache-domain-joined-create-configure-enterprise-security-cluster/hdinsight-image-0064.png)

1. **Yapılandırma tamamlanma** ekranı görüntülendiğinde **Çıkış**' a tıklayın.
    ![Azure AD Yapılandırması Tamam](./media/apache-domain-joined-create-configure-enterprise-security-cluster/hdinsight-image-0078.png)

1. Eşitleme tamamlandıktan sonra, ıAAS Active Directory üzerinde oluşturduğunuz kullanıcıların Azure Active Directory ile eşitlendiğinden emin olun.
    1. Azure Portal’da oturum açın.
    1. **Azure Active Directory** > **hdifabrikam** > **kullanıcıları**' nı seçin.

### <a name="create-an-user-assigned-managed-identity"></a>Kullanıcı tarafından atanan yönetilen kimlik oluşturma

Azure Active Directory Domain Services (Azure AD DS) yapılandırmak için kullanılacak, Kullanıcı tarafından atanan bir yönetilen kimlik oluşturun. Kullanıcı tarafından atanan yönetilen kimlik oluşturma hakkında daha fazla bilgi için, bkz. [Azure Portal kullanarak Kullanıcı tarafından atanan yönetilen kimlik için rol oluşturma, listeleme, silme veya atama](../../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md).

1. Azure Portal’da oturum açın.
1. **Kaynak oluştur ' a** tıklayın ve **yönetilen kimlik**yazın. **Kullanıcı tarafından atanan yönetilen kimlik** > **Oluştur**' u seçin.
1. **Kaynak adı**olarak **HDIFabrikamManagedIdentity** girin.
1. Aboneliğinizi seçin.
1. **Kaynak grubu** altında **Yeni oluştur** ' a tıklayın ve **Hdifabrikam-merkezileşus**girin.
1. **Konum**altında **Orta ABD** seçin.
1. **Oluştur**'a tıklayın.

![Kullanıcı tarafından atanan yeni bir yönetilen kimlik oluşturma](./media/apache-domain-joined-create-configure-enterprise-security-cluster/hdinsight-image-0082.png)

### <a name="enable-azure-active-directory-domain-services"></a>Azure Active Directory Domain Services’ı etkinleştirme

Daha fazla bilgi için bkz. [Azure Portal kullanarak Azure Active Directory Domain Services etkinleştirme](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-getting-started).

1. Azure AD-DS 'yi barındıracak sanal ağı oluşturun. Aşağıdaki PowerShell kodunu çalıştırın.

    ```powershell
    Connect-AzAccount
    Get-AzSubscription
    Set-AzContext -Subscription 'SUBSCRIPTION_ID'
    $virtualNetwork = New-AzVirtualNetwork -ResourceGroupName 'HDIFabrikam-CentralUS' -Location 'Central US' -Name 'HDIFabrikam-AADDSVNET' -AddressPrefix 10.1.0.0/16
    $subnetConfig = Add-AzVirtualNetworkSubnetConfig -Name 'AADDS-subnet' -AddressPrefix 10.1.0.0/24 -VirtualNetwork $virtualNetwork
    $virtualNetwork | Set-AzVirtualNetwork
    ```

1. Azure Portal’da oturum açın.
1. **Kaynak oluştur**' a tıklayın, **etki alanı hizmetleri** girin ve **Azure AD Domain Services**seçin.
1. **Temel bilgiler** ekranında, aşağıdaki adımları izleyin:
    1. **Dizin adı** altında, bu makale için oluşturulan Azure Active Directory seçin, **hdıfabrikam**.
    1. **HDIFabrikam.com**bir **DNS etki alanı adı** girin.
    1. Aboneliğinizi seçin.
    1. **Hdifabrikam-merkezileştirme** kaynak grubunu ve **Orta ABD** **konumunu** belirtin.

        ![Azure AD Domain Services temel Ayrıntılar](./media/apache-domain-joined-create-configure-enterprise-security-cluster/hdinsight-image-0084.png)

1. **Ağ** ekranı tamamlandıktan sonra, önceki PowerShell betiği ile oluşturduğunuz ağı (**HDıFABRIKAM-VNET**) ve alt ağı (**aeklemesine-subnet**) seçin. Ya da şimdi bir sanal ağ oluşturmak için **Yeni oluştur** seçeneğini kullanabilirsiniz.

    ![Sanal ağ oluşturma Azure AD Domain Services](./media/apache-domain-joined-create-configure-enterprise-security-cluster/hdinsight-image-0086.png)

1. **Yönetici grubu** ekranında, **AAD DC yöneticileri** adlı bir grubun bu grubu yönetmek için zaten oluşturulduğunu belirten bir bildirim görmeniz gerekir. İsteğe bağlı olarak bu grubun üyeliğini değiştirebilirsiniz, ancak bu makalenin adımları için gerekli değildir. **Tamam**'ı tıklatın.

    ![Azure AD yönetici grubunu görüntüle](./media/apache-domain-joined-create-configure-enterprise-security-cluster/hdinsight-image-0088.png)

1. **Eşitleme** ekranında, **tümünü seçerek tüm** eşitlemeyi etkinleştirin ve ardından **Tamam**' a tıklayın.

    ![Azure AD Domain Services eşitlemeyi etkinleştir](./media/apache-domain-joined-create-configure-enterprise-security-cluster/hdinsight-image-0090.png)

1. **Özet** EKRANıNDA Azure AD-DS 'nin ayrıntılarını doğrulayıp **Tamam**' a tıklayın.

    ![Azure AD Domain Services özetini etkinleştir](./media/apache-domain-joined-create-configure-enterprise-security-cluster/hdinsight-image-0092.png)

1. Azure AD-DS 'yi etkinleştirdikten sonra, AD sanal makinelerinde (VM 'Ler) bir yerel etki alanı ad hizmeti (DNS) sunucusu çalışır.

### <a name="configure-your-azure-ad-ds-virtual-network"></a>Azure AD-DS Sanal ağınızı yapılandırma

Bu bölümdeki adımlar, Azure AD-DS Sanal ağınızı (**Hdıfabrikam-AADDSVNET**) özel DNS sunucularınızı kullanacak şekilde yapılandırmanıza yardımcı olur.

1. Özel DNS sunucularınızın IP adreslerini bulun. **HDIFabrikam.com** AD-DS kaynağına tıklayın, **Yönet**   ' in altındaki **Özellikler** ' e tıklayın ve **sanal ağdaki IP adresi**altında listelenen IP adreslerine bakın.

    ![Azure AD-DS için özel DNS IP adreslerini bulma](./media/apache-domain-joined-create-configure-enterprise-security-cluster/hdinsight-image-0096.png)

1. **Hdifabrikam-aaddsvnet** ' i özel IP `10.0.0.4` 'ler `10.0.0.5`ile yapılandırın.

    1.   **Ayarlar**kategorisialtındaDNSsunucuları'nı seçin. ardından, **özel**' in yanındaki radyo düğmesine tıklayın, aşağıdaki metin kutusuna Ilk IP adresini (10.0.0.4) girin ve **Kaydet**' e tıklayın.
    1. Aynı adımları kullanarak ek IP adresleri (10.0.0.5) ekleyin.

1. Senaryolarımızda Azure AD-DS, 10.0.0.4 ve 10.0.0.5 IP adreslerini kullanacak şekilde yapılandırılmıştır. Bu, AFS üzerinde aynı IP adresini aşağıdaki görüntüde göster olarak ayarlar.

    ![özel DNS sunucularını görüntüle iletişim kutusu](./media/apache-domain-joined-create-configure-enterprise-security-cluster/hdinsight-image-0098.png)

## <a name="securing-ldap-traffic"></a>LDAP trafiğinin güvenliğini sağlama

Active Directory okumak ve yazmak için hafif Dizin Erişim Protokolü (LDAP) kullanılır. LDAP trafiğini Güvenli Yuva Katmanı (SSL)/Aktarım Katmanı Güvenliği (TLS) teknolojisini kullanarak gizli ve güvenli hale getirebilirsiniz. Düzgün şekilde biçimlendirilen bir sertifika yükleyerek SSL üzerinden LDAP 'yi (LDAPS) etkinleştirebilirsiniz.

Güvenli LDAP hakkında daha fazla bilgi için, [Azure AD Domain Services yönetilen bir etki alanı için GÜVENLI LDAP (LDAPS) yapılandırma](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap)konusuna bakın.

Bu bölümde, kendinden imzalı bir sertifika oluşturur, sertifikayı indirebilir ve **hdifabrikam** Azure AD-DS tarafından yönetilen etki alanı IÇIN Güvenli LDAP (LDAPS) yapılandırabilirsiniz.

Aşağıdaki betik, hdıfabrikam için bir sertifika oluşturur. Sertifika, "LocalMachine" yolu altına kaydedilir.

> [!Note] 
> SSL sertifika isteği oluşturmak için geçerli bir PKCS \#10 isteği oluşturan herhangi bir yardımcı program veya uygulama kullanılabilir.

```powershell
$lifetime = Get-Date
New-SelfSignedCertificate -Subject hdifabrikam.com `
-NotAfter $lifetime.AddDays(365) -KeyUsage DigitalSignature, KeyEncipherment `
-Type SSLServerAuthentication -DnsName *.hdifabrikam.com, hdifabrikam.com
```

Sertifikanın, bilgisayar\'s Kişisel deposunda yüklü olduğunu doğrulayın. Aşağıdaki adımları tamamlayın:

1. Microsoft Yönetim Konsolu 'Nu (MMC) başlatın.
1. Yerel bilgisayardaki sertifikaları yöneten Sertifikalar ek bileşenini ekleyin.
1.  **Sertifikalar (yerel bilgisayar)** ' ı genişletin, **Kişisel**' i genişletin ve **Sertifikalar**' ı genişletin. Kişisel depoda yeni bir sertifika bulunmalıdır. Bu sertifika, tam ana bilgisayar adına verilir.

    ![Yerel sertifika oluşturmayı doğrulama](./media/apache-domain-joined-create-configure-enterprise-security-cluster/hdinsight-image-0102.png)

1. Sağ bölmede, önceki adımda oluşturduğunuz sertifikaya sağ tıklayın, **Tüm görevler**' in üzerine gelin ve ardından **dışarı aktar**' a tıklayın.

1.   **Özel anahtarı dışarı aktar**sayfasında **Evet, özel, anahtarı dışarı aktar**' a tıklayın. Şifrelenmiş iletilerin, anahtarın içeri aktarılacağı bilgisayardan okunması için özel anahtar gereklidir.

    ![Sertifika Sihirbazı, özel anahtarı dışarı aktar sayfası](./media/apache-domain-joined-create-configure-enterprise-security-cluster/hdinsight-image-0103.png)

1.  **Dışarı aktarma dosyası biçimi** sayfasında, varsayılan ayarları bırakın ve ardından **İleri**' ye tıklayın. 
1.  **Parola** sayfasında, özel anahtar için bir parola yazın, şifreleme için TripleDES-SHA1 ' ı seçin ve ileri ' ye tıklayın. 
1.  **Dışarı aktarılacak** dosya sayfasında, dışarı aktarılmış sertifika dosyasının yolunu ve adını yazın ve ardından **İleri**' ye tıklayın.
1. Dosya adının. pfx uzantısı olması gerekir, bu dosya güvenli bağlantı kurmak için Azure portal ' de yapılandırılır.
1. Azure AD Domain Services yönetilen bir etki alanı için Güvenli LDAP (LDAPS) özelliğini etkinleştirin.
    1. Azure portal etki alanı **HDIFabrikam.com** seçin.
    1. **Yönet**altında **Güvenli LDAP** ' ye tıklayın.
    1. **Güvenli LDAP** ekranında, **Güvenli LDAP**altında **Etkinleştir** ' e tıklayın.
    1. Bilgisayarınıza verdiğiniz. pfx sertifika dosyasına gözatamazsınız.
    1. Sertifika parolasını girin.

    ![Azure AD-DS, Güvenli LDAP 'yi etkinleştirme](./media/apache-domain-joined-create-configure-enterprise-security-cluster/hdinsight-image-0113.png)

1. Artık Güvenli LDAP etkinleştirdiğinizden, bağlantı noktası 636 ' i etkinleştirerek erişilebilir olduğundan emin olun.
    1. **Hdifabrikam-merkezde ABD** kaynak grubundaki ağ güvenlik grubuna **aeklemesine-HDIFabrikam.com-NSG** ' ye tıklayın.
    1. **Ayarlar** altında **gelen güvenlik kuralları** > **Ekle**' ye tıklayın.
    1. **Gelen güvenlik kuralı ekle** ekranında, aşağıdaki özellikleri girin ve **Ekle**' ye tıklayın:

        | Özellik | Value |
        |---|---|
        | Source | Any |
        | Source port ranges | * |
        | Destination | Any |
        | Destination port range | 636 |
        | Protocol | Any |
        | Action | Allow |
        | Priority | \<Desired Number\> |
        | Name | Port_LDAP_636 |

    ![Gelen güvenlik kuralı Ekle iletişim kutusu](./media/apache-domain-joined-create-configure-enterprise-security-cluster/add-inbound-security-rule.png)

1. `HDIFabrikamManagedIdentity`Kullanıcı tarafından atanan yönetilen kimlik, HDInsight etki alanı Hizmetleri katılımcısı rolü, bu kimliğin etki alanı Hizmetleri işlemlerini okumasına, oluşturmasına, değiştirmesine ve silmesine imkan tanıyan yönetilen kimlik için etkinleştirilmiştir.

    ![Kullanıcı tarafından atanan yönetilen kimlik oluştur](./media/apache-domain-joined-create-configure-enterprise-security-cluster/hdinsight-image-0117.png)

## <a name="creating-enterprise-security-package-enabled-hdinsight-cluster"></a>Kurumsal Güvenlik Paketi etkin HDInsight kümesi oluşturuluyor

Bu adım aşağıdaki önkoşulları gerektirir:

1. `HDIFabrikam-WestUS` Konumda`West US`yeni bir kaynak grubu oluşturun.
1. ESP etkin HDInsight kümesini barındıracak bir sanal ağ oluşturun.

    ```powershell
    $virtualNetwork = New-AzVirtualNetwork -ResourceGroupName 'HDIFabrikam-WestUS' -Location 'West US' -Name 'HDIFabrikam-HDIVNet' -AddressPrefix 10.1.0.0/16
    $subnetConfig = Add-AzVirtualNetworkSubnetConfig -Name 'SparkSubnet' -AddressPrefix 10.1.0.0/24 -VirtualNetwork $virtualNetwork
    $virtualNetwork | Set-AzVirtualNetwork
    ```

1. Aeklemeleri (`HDIFabrikam-AADDSVNET`) ve ESP etkin HDInsight kümesini barındıracak sanal ağı (`HDIFabrikam-HDIVNet`) barındıran sanal ağ arasında bir eşdüzey ilişki oluşturun. Bu iki sanal ağı eşler için aşağıdaki PowerShell kodunu kullanın.

    ```powershell
    Add-AzVirtualNetworkPeering -Name 'HDIVNet-AADDSVNet' -RemoteVirtualNetworkId (Get-AzVirtualNetwork -ResourceGroupName 'HDIFabrikam-CentralUS').Id -VirtualNetwork (Get-AzVirtualNetwork -ResourceGroupName 'HDIFabrikam-WestUS')

    Add-AzVirtualNetworkPeering -Name 'AADDSVNet-HDIVNet' -RemoteVirtualNetworkId (Get-AzVirtualNetwork -ResourceGroupName 'HDIFabrikam-WestUS').Id -VirtualNetwork (Get-AzVirtualNetwork -ResourceGroupName 'HDIFabrikam-CentralUS')
    ```

1. Kullanıcı tarafından yönetilen kimlik **HDIFabrikamManagedIdentity**ile yapılandırılan, **Hdigen2store**adlı yeni bir Azure Data Lake Storage 2. hesabı oluşturun. Kullanıcı tarafından yönetilen kimliklerle etkinleştirilmiş Data Lake Storage 2. hesapları oluşturma hakkında daha fazla bilgi için bkz. [Azure HDInsight kümeleri ile Azure Data Lake Storage 2. kullanma](../hdinsight-hadoop-use-data-lake-storage-gen2.md).

1. **Hdifabrikam-AADDSVNET** sanal AĞıNDA özel DNS ayarlayın.
    1. **Onpremadvrg** > **hdifabrikam-aaddsvnet** > **DNS sunucuları**Azure Portal > **kaynak gruplarına** > gidin.
    1. **Özel** ' i seçin `10.0.0.4` ve `10.0.0.5`ve yazın.
    1. **Kaydet**’e tıklayın.

        ![Sanal ağ için özel DNS ayarlarını kaydet](./media/apache-domain-joined-create-configure-enterprise-security-cluster/hdinsight-image-0123.png)

1. Yeni bir ESP etkin HDInsight Spark kümesi oluşturun.
    1. **Özel (boyut, ayarlar, uygulamalar)** seçeneğine tıklayın.
    2. Bölüm 1 **temel kavramları**için istenen ayrıntıları girin. **Küme türünün** **Spark 2,3 (HDI 3,6)** ve **kaynak grubunun** **hdifabrikam-merkezde ABD** olduğundan emin olun

    1. Bölüm 2 **güvenlik + ağ**altında, aşağıdaki adımları izleyin:
        1. **Kurumsal güvenlik paketi**altında **etkin** ' e tıklayın.
        1. **Küme Yönetici kullanıcısı** ' na tıklayın ve şirket içi yönetici kullanıcı olarak daha önce oluşturduğunuz **hdıadmin** hesabını seçin. Tıklayın **seçin**.

        1. **Küme erişim grubu** ' na tıklayın ve ardından **HDIUserGroup**' ı seçin. Gelecekte bu gruba eklediğiniz tüm kullanıcılar HDInsight kümelerine erişebilecektir.

            ![küme erişim grubu HDIUserGroup seçin](./media/apache-domain-joined-create-configure-enterprise-security-cluster/hdinsight-image-0129.jpg)

    1. Küme yapılandırmasının diğer adımlarını tamamlayıp **küme özetinin**ayrıntılarını doğrulayın. **Oluştur**'a tıklayın.

1. Yönetici Kullanıcı adınızı `https://CLUSTERNAME.azurehdinsight.net` `hdiadmin@hdifabrikam.com` ve parolanızı kullanarak tarihinde yeni oluşturulan kümenin ambarı Kullanıcı arabiriminde oturum açın.

    ![Apache ambarı Kullanıcı arabirimi oturum açma penceresi](./media/apache-domain-joined-create-configure-enterprise-security-cluster/hdinsight-image-0135.jpg)

1. Küme panosundan **Roller** ' e tıklayın.
1. **Roller** sayfasında, **rol ata**altındaki **Küme Yöneticisi** rolüne atamak için **hdiusergroup** grubunu girin.

    ![Hdiusergroup 'e Küme Yöneticisi rolü atama](./media/apache-domain-joined-create-configure-enterprise-security-cluster/hdinsight-image-0137.jpg)

1. SSH istemcinizi açın ve daha önce şirket içi Active Directory oluşturduğunuz **hdiuser** kullanarak kümede oturum açın.

    ![SSH istemcisiyle küme üzerinde oturum açma](./media/apache-domain-joined-create-configure-enterprise-security-cluster/hdinsight-image-0139.jpg)

Bu hesapla oturum açabilmeniz için, ESP kümenizi şirket içi Active Directory 'niz ile eşitlenecek şekilde doğru şekilde yapılandırdınız.

## <a name="next-steps"></a>Sonraki adımlar

* [Kurumsal Güvenlik Paketi Apache Hadoop güvenlik ile ilgili bir giriş](hdinsight-security-overview.md)
