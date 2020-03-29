---
title: Kurumsal Güvenlik Paketi kümeleri oluşturma, yapılandırma - Azure
description: Azure HDInsight'ta Kurumsal Güvenlik Paketi kümelerini nasıl oluşturup yapılandırılamayı öğrenin
services: hdinsight
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.topic: conceptual
ms.date: 12/10/2019
ms.openlocfilehash: 4edafc0c07e967acfabf7fdc5b58c481b2cfccc3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75436025"
---
# <a name="create-and-configure-enterprise-security-package-clusters-in-azure-hdinsight"></a>Azure HDInsight'ta Kurumsal Güvenlik Paketi kümeleri oluşturma ve yapılandırma

Azure HDInsight için Kurumsal Güvenlik Paketi (ESP), Azure'daki Apache Hadoop kümeleriniz için Active Directory tabanlı kimlik doğrulama, çok kullanıcılı destek ve rol tabanlı erişim denetimine erişmenizi sağlar. HDInsight ESP kümeleri, hassas verileri güvenli bir şekilde işlemek için sıkı kurumsal güvenlik ilkelerine uyan kuruluşların olmasını sağlar.

Bu kılavuz, ESP özellikli bir Azure HDInsight kümesinin nasıl oluşturulacak olduğunu gösterir. Ayrıca, Active Directory ve Domain Name System (DNS) etkin olduğu bir Windows IaaS VM nasıl oluşturulacak larını da gösterir. Şirket içi kullanıcıların ESP özellikli bir HDInsight kümesinde oturum açmalarına izin vermek için gerekli kaynakları yapılandırmak için bu kılavuzu kullanın.

Oluşturduğunuz sunucu, *gerçek* şirket içi ortamınızın yerini alacak. Kurulum ve yapılandırma adımları için kullanırsınız. Daha sonra adımları kendi ortamınızda yinelersiniz.

Bu kılavuz, Azure Etkin Dizini (Azure AD) ile parola karma eşitleme kullanarak karma kimlik ortamı oluşturmanıza da yardımcı olur. Kılavuz, [HDInsight'ta ESP kullanımını](apache-domain-joined-architecture.md)tamamlar.

Bu işlemi kendi ortamınızda kullanmadan önce:

* Active Directory ve DNS'yi ayarlayın.
* Azure AD'yi etkinleştirin.
* Şirket içi kullanıcı hesaplarını Azure AD ile eşitle.

![Azure AD mimari diyagramı](./media/apache-domain-joined-create-configure-enterprise-security-cluster/hdinsight-image-0002.png)

## <a name="create-an-on-premises-environment"></a>Şirket içinde ortam oluşturma

Bu bölümde, yeni VM'ler oluşturmak, DNS'yi yapılandırmak ve yeni bir Active Directory ormanı eklemek için bir Azure Hızlı Başlatma dağıtım şablonu kullanırsınız.

1. [Yeni Bir Active Directory ormanı olan bir Azure VM oluşturmak](https://azure.microsoft.com/resources/templates/active-directory-new-domain/)için Hızlı Başlatma dağıtım şablonuna gidin.

1. **Azure'a Dağıt'ı**seçin.
1. Azure aboneliğinizde oturum açın.
1. Yeni bir AD Forest sayfası **yla Azure VM Oluştur'da** aşağıdaki bilgileri sağlayın:

    |Özellik | Değer |
    |---|---|
    |Abonelik|Kaynakları dağıtmak istediğiniz aboneliği seçin.|
    |Kaynak grubu|**Yeni Oluştur'u**seçin ve adı girin`OnPremADVRG`|
    |Konum|Bir konum seçin.|
    |Yönetici Kullanıcı Adı|`HDIFabrikamAdmin`|
    |Yönetici Parolası|Parola girin.|
    |Etki Alanı Adı|`HDIFabrikam.com`|
    |Dns Önek|`hdifabrikam`|

    Kalan varsayılan değerleri bırakın.

    ![Yeni bir Azure AD Ormanı ile Azure VM Oluşturma Şablonu](./media/apache-domain-joined-create-configure-enterprise-security-cluster/create-azure-vm-ad-forest.png)

1. Şartlar **ve Koşulları**Gözden Geçirin ve daha sonra **yukarıda belirtilen hüküm ve koşulları kabul ediyorum**seçin.
1. **Satın Al'ı**seçin ve dağıtımı izleyin ve tamamlanmasını bekleyin. Dağıtımın tamamlanması yaklaşık 30 dakika sürer.

## <a name="configure-users-and-groups-for-cluster-access"></a>Küme erişimi için kullanıcıları ve grupları yapılandırma

Bu bölümde, bu kılavuzun sonuna kadar HDInsight kümesine erişebilecek kullanıcıları oluşturursunuz.

1. Uzak Masaüstü'nü kullanarak etki alanı denetleyicisine bağlanın.
    1. Azure portalından,**OnPremADVRG** > **adVM** > **Connect**kaynak **gruplarına** > gidin.
    1. IP **adresi** açılır listesinden genel IP adresini seçin.
    1. **RDP Dosyasını İndir'i**seçin ve ardından dosyayı açın.
    1. Kullanıcı `HDIFabrikam\HDIFabrikamAdmin` adı olarak kullanın.
    1. Yönetici hesabı için seçtiğiniz parolayı girin.
    1. **Tamam'ı**seçin.

1. Etki alanı **denetleyicisi Server Manager** panosundan, **Araçlar** > **Etkin Dizin Kullanıcıları ve Bilgisayarlarına**gidin.

    ![Sunucu Yöneticisi panosunda Active Directory Management'ı açın](./media/apache-domain-joined-create-configure-enterprise-security-cluster/server-manager-active-directory-screen.png)

1. İki yeni kullanıcı oluşturun: **HDIAdmin** ve **HDIUser**. Bu iki kullanıcı HDInsight kümelerinde oturum açacaktır.

    1. Etkin **Dizin Kullanıcıları ve Bilgisayarları** `HDIFabrikam.com`sayfasından sağ tıklayın ve ardından **Yeni** > **Kullanıcı'ya**gidin.

        ![Yeni bir Active Directory kullanıcısı oluşturma](./media/apache-domain-joined-create-configure-enterprise-security-cluster/create-active-directory-user.png)

    1. Yeni **Nesne - Kullanıcı** sayfasında, **Ad** ve **Kullanıcı oturum açma adı**için girin. `HDIUser` Diğer alanlar otomatik olarak doldurulur. Ardından **İleri'yi**seçin.

        ![İlk yönetici kullanıcı nesnesini oluşturma](./media/apache-domain-joined-create-configure-enterprise-security-cluster/hdinsight-image-0020.png)

    1. Görünen açılır pencereye yeni hesabın parolasını girin. **Parola'nın süresi nin asla dolmadığını**ve açılan iletide **Tamam'ı** seçin.
    1. Yeni hesap oluşturmak için **İleri**ve **ardından Bitir'i** seçin.
    1. Kullanıcıyı `HDIAdmin`oluşturmak için yukarıdaki adımları yineleyin.

        ![İkinci bir yönetici kullanıcı nesnesi oluşturma](./media/apache-domain-joined-create-configure-enterprise-security-cluster/hdinsight-image-0024.png)

1. Genel bir güvenlik grubu oluşturun.

    1. **Etkin Dizin Kullanıcıları ve Bilgisayar'dan**sağ tıklayın `HDIFabrikam.com`ve ardından **Yeni** > **Grup'a**gidin.

    1. Grup `HDIUserGroup` **adı** metin kutusuna girin.

    1. **Tamam'ı**seçin.

    ![Yeni bir Active Directory grubu oluşturma](./media/apache-domain-joined-create-configure-enterprise-security-cluster/create-active-directory-group.png)

    ![Yeni bir nesne oluşturma](./media/apache-domain-joined-create-configure-enterprise-security-cluster/hdinsight-image-0028.png)

1. **HDIUserGroup'a**üye ekleyin.

    1. **HDIUser'a** sağ tıklayın ve **gruba ekle'yi seçin...**.
    1. Metin kutusunu **seçmek için nesne adlarını girin'e** `HDIUserGroup`. Ardından açılan pencerede **Tamam**ve **Tamam'ı** tekrar seçin.
    1. **HDIAdmin** hesabı için önceki adımları yineleyin.

        ![ÜYE HDIUser'ı HDIUserGroup grubuna ekleyin](./media/apache-domain-joined-create-configure-enterprise-security-cluster/active-directory-add-users-to-group.png)

Şimdi Active Directory ortamınızı oluşturdunuz. HDInsight kümesine erişebilen iki kullanıcı ve bir kullanıcı grubu eklediniz.

Kullanıcılar Azure AD ile senkronize edilecektir.

### <a name="create-an-azure-ad-directory"></a>Azure AD dizini oluşturma

1. Azure Portal’da oturum açın.
1. **Kaynak Oluştur'u** `directory`seçin ve yazın. **Azure Etkin Dizin** > **Oluştur'u**seçin.
1. **Kuruluş adı**altında `HDIFabrikam`, girin .
1. **İlk etki alanı** `HDIFabrikamoutlook`adı altında , girin .
1. **Oluştur'u**seçin.

    ![Azure AD dizini oluşturma](./media/apache-domain-joined-create-configure-enterprise-security-cluster/create-new-directory.png)

### <a name="create-a-custom-domain"></a>Özel bir etki alanı oluşturma

1. Yeni **Azure Etkin Dizini'nizden** **Yönet'in**altında Özel **alan adlarını**seçin.
1. Seçin **+ Özel etki alanı ekleyin.**
1. **Özel etki alanı** `HDIFabrikam.com`adı altında , girin ve sonra **etki alanı ekle'yi**seçin.
1. Ardından [DNS bilgilerinizi etki alanı kayıt şirketine ekleyin'i](../../active-directory/fundamentals/add-custom-domain.md#add-your-dns-information-to-the-domain-registrar)tamamlayın.

![Özel bir etki alanı oluşturma](./media/apache-domain-joined-create-configure-enterprise-security-cluster/create-custom-domain.png)

### <a name="create-a-group"></a>Grup oluşturma

1. Yeni **Azure Etkin Dizini'nizden** **, Yönet**altında **Gruplar'ı**seçin.
1. + **Yeni grubu**seçin.
1. Grup **adı** metin kutusuna. `AAD DC Administrators`
1. **Oluştur'u**seçin.

## <a name="configure-your-azure-ad-tenant"></a>Azure AD kiracınızı yapılandırma

Artık Azure AD kiracınızı, kullanıcıları ve grupları şirket içi Active Directory örneğinden buluta senkronize edilebilmeniz için yapılandıracaksınız.

Etkin Dizin kiracı yöneticisi oluşturun.

1. Azure portalında oturum açın ve Azure AD kiracınızı, **HDIFabrikam'ı**seçin.

1.  > **Kullanıcıları** **Yönetmek** > için gidin**Yeni kullanıcı**.

1. Yeni kullanıcı için aşağıdaki ayrıntıları girin:

    **Kimlik**

    |Özellik |Açıklama |
    |---|---|
    |Kullanıcı adı|Metin `fabrikamazureadmin` kutusuna girin. Alan adı açılır listesinden,`hdifabrikam.com`|
    |Adı| `fabrikamazureadmin` yazın.|

    **Parola**
    1. **Şifreyi oluşturayım'ı**seçin.
    1. Seçtiğiniz güvenli bir parola girin.

    **Gruplar ve roller**
    1. **Seçili 0 grubu seçin.**
    1. **AAD DC Yöneticileri'ni**seçin ve sonra **seçin.**

    ![Azure AD Grupları iletişim kutusu](./media/apache-domain-joined-create-configure-enterprise-security-cluster/azure-ad-add-group-member.png)

    1. **Kullanıcı'yı**seçin.
    1. **Global yöneticiyi**seçin ve sonra **seçin.**

    ![Azure AD rol iletişim kutusu](./media/apache-domain-joined-create-configure-enterprise-security-cluster/azure-ad-add-role-member.png)

1. **Oluştur'u**seçin.

1. Ardından, yeni kullanıcının parolayı değiştirmesinin isteneceği Azure portalında oturum açmasını sağlar. Microsoft Azure Active Directory Connect'i yapılandırmadan önce bunu yapmanız gerekir.

## <a name="sync-on-premises-users-to-azure-ad"></a>Şirket içi kullanıcıları Azure AD ile senkronize etme

### <a name="configure-microsoft-azure-active-directory-connect"></a>Microsoft Azure Active Directory Connect'i yapılandırma

1. Etki alanı denetleyicisinden [Microsoft Azure Active Directory Connect'i indirin.](https://www.microsoft.com/download/details.aspx?id=47594)

1. İndirdiğiniz çalıştırılabilir dosyayı açın ve lisans koşullarını kabul edin. **Devam**'ı seçin.

1. **Ekspres ayarları kullan'ı**seçin.

1. Azure **AD'ye Bağlan** sayfasında, Azure AD için genel yöneticinin kullanıcı adını ve parolasını girin. Active Directory `fabrikamazureadmin@hdifabrikam.com` kiracınızı yapılandırırken oluşturduğunuz kullanıcı adını kullanın. Ardından **İleri'yi**seçin.

    !["Azure AD'ye Bağlan" sayfası](./media/apache-domain-joined-create-configure-enterprise-security-cluster/hdinsight-image-0058.png)

1. Etkin **Dizin Etki Alanı Hizmetlerine Bağlan** sayfasında, kurumsal yönetici hesabının kullanıcı adını ve parolasını girin. Daha önce `HDIFabrikam\HDIFabrikamAdmin` oluşturduğunuz kullanıcı adını ve parolasını kullanın. Ardından **İleri'yi**seçin.

   !["Azure AD'ye Bağlan" sayfası](./media/apache-domain-joined-create-configure-enterprise-security-cluster/hdinsight-image-0060.png)
1. Azure **AD oturum açma yapılandırma** sayfasında **İleri'yi**seçin.
   !["Azure AD oturum açma yapılandırması" sayfası](./media/apache-domain-joined-create-configure-enterprise-security-cluster/hdinsight-image-0062.png)

1. **Yapılandırmaya Hazır** sayfasında **Yükle'yi**seçin.

   !["Yapılandırmaya hazır" sayfası](./media/apache-domain-joined-create-configure-enterprise-security-cluster/hdinsight-image-0064.png)

1. Yapılandırma **tam** **sayfasında, Exit'i**seçin.
   !["Yapılandırma tamamlandı" sayfası](./media/apache-domain-joined-create-configure-enterprise-security-cluster/hdinsight-image-0078.png)

1. Eşitleme tamamlandıktan sonra, IaaS dizininde oluşturduğunuz kullanıcıların Azure AD ile senkronize edildiğini doğrulayın.
   1. Azure Portal’da oturum açın.
   1. **Azure Active Directory** > **HDIFabrikam** > **Kullanıcıları'nı**seçin.

### <a name="create-a-user-assigned-managed-identity"></a>Kullanıcı tarafından atanan yönetilen kimlik oluşturma

Azure AD Etki Alanı Hizmetlerini (Azure AD DS) yapılandırmak için kullanabileceğiniz kullanıcı tarafından atanmış yönetilen bir kimlik oluşturun. Daha fazla bilgi için bkz: [Azure portalını kullanarak kullanıcı tarafından atanan yönetilen bir kimliğe rol oluşturun, listeleyin, silin veya atayın.](../../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md)

1. Azure Portal’da oturum açın.
1. **Kaynak Oluştur'u** `managed identity`seçin ve yazın. **Kullanıcı Atanmış Yönetilen Kimlik** > **Oluştur'u**seçin.
1. Kaynak **Adı**için `HDIFabrikamManagedIdentity`, girin .
1. Aboneliğinizi seçin.
1. **Kaynak grubu**altında, yeni `HDIFabrikam-CentralUS` **oluştur'u** seçin ve girin.
1. **Konum**altında, **Orta ABD'yi**seçin.
1. **Oluştur'u**seçin.

![Kullanıcı tarafından atanan yeni bir yönetilen kimlik oluşturma](./media/apache-domain-joined-create-configure-enterprise-security-cluster/hdinsight-image-0082.png)

### <a name="enable-azure-ad-ds"></a>Azure AD DS’yi etkinleştirme

Azure AD DS'yi etkinleştirmek için aşağıdaki adımları izleyin. Daha fazla bilgi için Azure [portalını kullanarak Azure AD DS'yi etkinleştir'e](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-getting-started)bakın.

1. Azure AD DS'yi barındırmak için sanal bir ağ oluşturun. Aşağıdaki PowerShell kodunu çalıştırın.

    ```powershell
    # Sign in to your Azure subscription
    $sub = Get-AzSubscription -ErrorAction SilentlyContinue
    if(-not($sub))
    {
        Connect-AzAccount
    }

    # If you have multiple subscriptions, set the one to use
    # Select-AzSubscription -SubscriptionId "<SUBSCRIPTIONID>"
    
    $virtualNetwork = New-AzVirtualNetwork -ResourceGroupName 'HDIFabrikam-CentralUS' -Location 'Central US' -Name 'HDIFabrikam-AADDSVNET' -AddressPrefix 10.1.0.0/16
    $subnetConfig = Add-AzVirtualNetworkSubnetConfig -Name 'AADDS-subnet' -AddressPrefix 10.1.0.0/24 -VirtualNetwork $virtualNetwork
    $virtualNetwork | Set-AzVirtualNetwork
    ```

1. Azure Portal’da oturum açın.
1. **Kaynak Oluştur'u** `Domain services`seçin, girin ve Azure AD Etki Alanı **Hizmetleri** > **Oluştur'u**seçin.
1. Temel **Bilgiler** sayfasında:
    1. **Dizin adı altında,** oluşturduğunuz Azure REKLAM dizinini seçin: **HDIFabrikam**.
    1. **DNS etki alanı adı için,** *HDIFabrikam.com*girin.
    1. Aboneliğinizi seçin.
    1. **HDIFabrikam-CentralUS**kaynak grubunu belirtin. **Konum**için, **Orta ABD'yi**seçin.

        ![Azure AD DS temel ayrıntıları](./media/apache-domain-joined-create-configure-enterprise-security-cluster/hdinsight-image-0084.png)

1. **Ağ** sayfasında, PowerShell komut dosyasını kullanarak oluşturduğunuz ağı **(HDIFabrikam-VNET)** ve alt**ağı (AADDS-subnet)** seçin. Veya şimdi sanal ağ oluşturmak için **yeni oluştur'u** seçin.

    !["Sanal ağ oluşturma" adımı](./media/apache-domain-joined-create-configure-enterprise-security-cluster/hdinsight-image-0086.png)

1. Yönetici **grup** sayfasında, bu grubu yönetmek için **AAD DC Yöneticileri** adında bir grubun oluşturulduğuna dair bir bildirim görmeniz gerekir. İsterseniz bu grubun üyeliğini değiştirebilirsiniz, ancak bu durumda değiştirmeniz gerekmez. **Tamam'ı**seçin.

    ![Azure AD yönetici grubunu görüntüleme](./media/apache-domain-joined-create-configure-enterprise-security-cluster/hdinsight-image-0088.png)

1. **Eşitleme** sayfasında, **Tümü** > **Tamam'ı**seçerek tam eşitleme yi etkinleştirin.

    ![Azure AD DS eşitlemesini etkinleştirme](./media/apache-domain-joined-create-configure-enterprise-security-cluster/hdinsight-image-0090.png)

1. **Özet** sayfasında, Azure AD DS'nin ayrıntılarını doğrulayın ve **Tamam'ı**seçin.

    !["Azure AD Etki Alanı Hizmetlerini Etkinleştir" özeti](./media/apache-domain-joined-create-configure-enterprise-security-cluster/hdinsight-image-0092.png)

Azure AD DS'yi etkinleştirdikten sonra, azure AD VM'lerinde yerel bir DNS sunucusu çalışır.

### <a name="configure-your-azure-ad-ds-virtual-network"></a>Azure AD DS sanal ağınızı yapılandırma

Azure AD DS sanal ağınızı **(HDIFabrikam-AADDSVNET)** özel DNS sunucularınızı kullanmak üzere yapılandırmak için aşağıdaki adımları kullanın.

1. Özel DNS sunucularınızın IP adreslerini bulun.
    1. `HDIFabrikam.com` Azure AD DS kaynağını seçin.
    1. **Yönet**altında, **Özellikleri**seçin.
    1. **Sanal ağdaki IP adresi**nin altındaki IP adreslerini bulun.

    ![Azure AD DS için özel DNS IP adreslerini bulma](./media/apache-domain-joined-create-configure-enterprise-security-cluster/hdinsight-image-0096.png)

1. **HDIFabrikam-AADDSVNET'i** 10.0.0.4 ve 10.0.0.5 özel IP adreslerini kullanacak şekilde yapılandırın.

    1. **Ayarlar** **altında, DNS Sunucularını**seçin.
    1. **Özel'i**seçin.
    1. Metin kutusuna, ilk IP adresini girin (*10.0.0.4*).
    1. **Kaydet'i**seçin.
    1. Diğer IP adresini eklemek için adımları yineleyin (*10.0.0.5*).

Senaryomuzda, Azure AD DS'yi 10.0.0.4 ve 10.0.0.5 IP adreslerini kullanacak şekilde yapılandırarak Azure AD DS sanal ağında aynı IP adresini ayarladık:

![Özel DNS sunucuları sayfası](./media/apache-domain-joined-create-configure-enterprise-security-cluster/hdinsight-image-0098.png)

## <a name="securing-ldap-traffic"></a>LDAP trafiğinin güvenliği

Hafif Dizin Erişim Protokolü (LDAP), Azure Active Directory'den okumak ve yazmak için kullanılır. Secure Sockets Layer (SSL) veya Transport Layer Security (TLS) teknolojisini kullanarak LDAP trafiğini gizli ve güvenli hale getirebilirsiniz. Düzgün biçimlendirilmiş bir sertifika yükleyerek LDAP'yi SSL (LDAPS) üzerinden etkinleştirebilirsiniz.

Güvenli LDAP hakkında daha fazla bilgi için Azure [AD DS yönetilen etki alanı için LDAPS'yi Yapılandır'a](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap)bakın.

Bu bölümde, kendi imzalı bir sertifika oluşturur, sertifikayı karşıdan yükler ve **HDIFabrikam** Azure AD DS yönetilen etki alanı için LDAPS'yi yapılandırabilirsiniz.

Aşağıdaki komut dosyası **HDIFabrikam**için bir sertifika oluşturur. Sertifika *LocalMachine* yoluna kaydedilir.

```powershell
$lifetime = Get-Date
New-SelfSignedCertificate -Subject hdifabrikam.com `
-NotAfter $lifetime.AddDays(365) -KeyUsage DigitalSignature, KeyEncipherment `
-Type SSLServerAuthentication -DnsName *.hdifabrikam.com, hdifabrikam.com
```

> [!NOTE]  
> Geçerli bir Ortak Anahtar Şifreleme Standartları (PKCS) \#10 isteği oluşturan herhangi bir yardımcı program veya uygulama, SSL sertifika isteğini oluşturmak için kullanılabilir.

Sertifikanın bilgisayarın **Kişisel** deposunda yüklü olduğunu doğrulayın:

1. Microsoft Yönetim Konsolu'nun (MMC) başlatılmasını başlatın.
1. Yerel bilgisayarda sertifikaları yöneten **Sertifikaları** eklemeyi ekleyin.
1. Sertifikaları Genişlet **(Yerel Bilgisayar)** > **Kişisel** > **Sertifikaları.** **Kişisel** mağazada yeni bir sertifika bulunmalıdır. Bu sertifika tam nitelikli ana bilgisayar adına verilir.

    ![Yerel sertifika oluşturmayı doğrulama](./media/apache-domain-joined-create-configure-enterprise-security-cluster/hdinsight-image-0102.png)

1. Sağdaki bölmede, oluşturduğunuz sertifikayı sağ tıklatın. Tüm **Görevlere**Işaret Edin ve sonra **Dışa Aktar'ı**seçin.

1. Özel **Anahtarı Dışa Aktar** sayfasında **Evet'i seçin, özel anahtarı dışa aktarın.** Anahtarın alınacağı bilgisayarın, şifrelenmiş iletileri okumak için özel anahtara ihtiyacı vardır.

    ![Sertifika Dışa Aktarma Sihirbazı'nın Dışa Aktar Özel Anahtar sayfası](./media/apache-domain-joined-create-configure-enterprise-security-cluster/hdinsight-image-0103.png)

1. Dosya **Biçimini Dışa Aktar** sayfasında varsayılan ayarları bırakın ve sonra **İleri'yi**seçin.
1. **Parola** sayfasında, özel anahtar için bir parola yazın. **Şifreleme**için **TripleDES-SHA1'i**seçin. Ardından **İleri'yi**seçin.
1. **Dışa Aktar sayfasında,** dışa aktarılan sertifika dosyasının yolunu ve adını yazın ve **sonra İleri'yi**seçin. Dosya adının bir .pfx uzantısı olması gerekir. Bu dosya, güvenli bir bağlantı kurmak için Azure portalında yapılandırılır.
1. Azure AD DS yönetilen etki alanı için LDAPS'ı etkinleştirin.
    1. Azure portalından etki alanını `HDIFabrikam.com`seçin.
    1. **Yönet**altında, **Güvenli LDAP'yi**seçin.
    1. Secure **LDAP** sayfasında, **Secure LDAP**altında Etkinleştir'i **seçin.**
    1. Bilgisayarınızda dışa aktardığınız .pfx sertifika dosyasına göz atın.
    1. Sertifika parolasını girin.

    ![Güvenli LDAP'yi etkinleştirme](./media/apache-domain-joined-create-configure-enterprise-security-cluster/hdinsight-image-0113.png)

1. LDAPS'yi etkinleştirdiğinize göre, 636 bağlantı noktasını etkinleştirerek erişim emildiğinden emin olun.
    1. **HDIFabrikam-CentralUS** kaynak grubunda Ağ güvenlik grubu **AADDS-HDIFabrikam.com-NSG'yi**seçin.
    1. **Ayarlar**altında, **Gelen güvenlik kurallarını** > seçin**Ekle**.
    1. Gelen **güvenlik kuralı** ekle sayfasında aşağıdaki özellikleri girin ve **Ekle'yi**seçin:

        | Özellik | Değer |
        |---|---|
        | Kaynak | Herhangi biri |
        | Kaynak bağlantı noktası aralıkları | * |
        | Hedef | Herhangi biri |
        | Hedef bağlantı noktası aralığı | 636 |
        | Protokol | Herhangi biri |
        | Eylem | İzin Ver |
        | Öncelik | \<İstenilen sayı> |
        | Adı | Port_LDAP_636 |

    !["Gelen güvenlik kuralı ekle" iletişim kutusu](./media/apache-domain-joined-create-configure-enterprise-security-cluster/add-inbound-security-rule.png)

**HDIFabrikamManagedIdentity,** kullanıcı tarafından atanan yönetilen kimliktir. HDInsight Etki Alanı Hizmetleri Katılımcısı rolü, bu kimliğin etki alanı hizmetleri işlemlerini okumasına, oluşturmasına, değiştirmesine ve silmesine olanak tanıyan yönetilen kimlik için etkinleştirilir.

![Kullanıcı tarafından atanan yönetilen kimlik oluşturma](./media/apache-domain-joined-create-configure-enterprise-security-cluster/hdinsight-image-0117.png)

## <a name="create-an-esp-enabled-hdinsight-cluster"></a>ESP özellikli HDInsight kümesi oluşturma

Bu adım aşağıdaki ön koşulları gerektirir:

1. **Batı ABD'de**yeni bir kaynak grubu *hdiFabrikam-WestUS* oluşturun.
1. ESP etkin HDInsight kümesini barındıracak bir sanal ağ oluşturun.

    ```powershell
    $virtualNetwork = New-AzVirtualNetwork -ResourceGroupName 'HDIFabrikam-WestUS' -Location 'West US' -Name 'HDIFabrikam-HDIVNet' -AddressPrefix 10.1.0.0/16
    $subnetConfig = Add-AzVirtualNetworkSubnetConfig -Name 'SparkSubnet' -AddressPrefix 10.1.0.0/24 -VirtualNetwork $virtualNetwork
    $virtualNetwork | Set-AzVirtualNetwork
    ```

1. Azure AD DS ()`HDIFabrikam-AADDSVNET`barındıran sanal ağ ile ESP özellikli HDInsight kümesini barındıracak`HDIFabrikam-HDIVNet`sanal ağ () arasında eş ilişkisi oluşturun. İki sanal ağa eş sağlamak için aşağıdaki PowerShell kodunu kullanın.

    ```powershell
    Add-AzVirtualNetworkPeering -Name 'HDIVNet-AADDSVNet' -RemoteVirtualNetworkId (Get-AzVirtualNetwork -ResourceGroupName 'HDIFabrikam-CentralUS').Id -VirtualNetwork (Get-AzVirtualNetwork -ResourceGroupName 'HDIFabrikam-WestUS')

    Add-AzVirtualNetworkPeering -Name 'AADDSVNet-HDIVNet' -RemoteVirtualNetworkId (Get-AzVirtualNetwork -ResourceGroupName 'HDIFabrikam-WestUS').Id -VirtualNetwork (Get-AzVirtualNetwork -ResourceGroupName 'HDIFabrikam-CentralUS')
    ```

1. **Hdigen2store**adında yeni bir Azure Veri Gölü Depolama Gen2 hesabı oluşturun. Hesabı kullanıcı tarafından yönetilen kimlik **HDIFabrikamManagedIdentity**ile yapılandırın. Daha fazla bilgi için bkz: [Azure HDInsight kümeleriyle Azure Veri Gölü Depolama Gen2'yi kullan.](../hdinsight-hadoop-use-data-lake-storage-gen2.md)

1. **HDIFabrikam-AADDSVNET** sanal ağında özel DNS'leri ayarlayın.
    1. **Kaynak grupları** > > Azure portalına gidin**OnPremADVRG** > **HDIFabrikam-AADDSVNET** > **DNS sunucuları.**
    1. **Özel'i** seçin ve *10.0.0.4* ve *10.0.0.5'i*girin.
    1. **Kaydet'i**seçin.

        ![Sanal ağ için özel DNS ayarlarını kaydetme](./media/apache-domain-joined-create-configure-enterprise-security-cluster/hdinsight-image-0123.png)

1. YENI bir ESP özellikli HDInsight Spark kümesi oluşturun.
    1. **Özel (boyut, ayarlar, uygulamalar) seçeneğini belirleyin.**
    1. **Temel bilgiler** için ayrıntıları girin (bölüm 1). **Küme türünün** **Spark 2.3 (HDI 3.6)** olduğundan emin olun. **Kaynak grubunun** **HDIFabrikam-CentralUS**olduğundan emin olun.

    1. **Güvenlik + ağ** (bölüm 2) için aşağıdaki ayrıntıları doldurun:
        * **Kurumsal Güvenlik Paketi**altında, **Etkin'i**seçin.
        * **Cluster admin kullanıcısını** seçin ve şirket içi yönetici kullanıcısı olarak oluşturduğunuz **HDIAdmin** hesabını seçin. **Seç'i**tıklatın.
        * **Cluster erişim grubu** > **HDIUserGroup'u**seçin. Gelecekte bu gruba eklediğiniz herhangi bir kullanıcı HDInsight kümelerine erişebilecektir.

            ![KÜME erişim grubu HDIUserGroup'u seçin](./media/apache-domain-joined-create-configure-enterprise-security-cluster/hdinsight-image-0129.jpg)

    1. Küme yapılandırmasının diğer adımlarını tamamlayın ve **Küme özetindeki**ayrıntıları doğrulayın. **Oluştur'u**seçin.

1. Yeni oluşturulan küme için Ambari UI'de `https://CLUSTERNAME.azurehdinsight.net`oturum açın. Yönetici kullanıcı adınızı `hdiadmin@hdifabrikam.com` ve parolasını kullanın.

    ![Apache Ambari UI oturum açma penceresi](./media/apache-domain-joined-create-configure-enterprise-security-cluster/hdinsight-image-0135.jpg)

1. Küme panosundan **Roller'i**seçin.
1. **Roller** sayfasında, **bunlara rol atanın**altında , **Küme Yöneticisi** rolünün yanında, grup *hdiusergroup*girin. 

    ![Küme yöneticisi rolünü hdiusergroup'a atama](./media/apache-domain-joined-create-configure-enterprise-security-cluster/hdinsight-image-0137.jpg)

1. Güvenli Shell (SSH) istemcinizi açın ve kümede oturum açın. Şirket içi Active Directory örneğinde oluşturduğunuz **hdiuser'ı** kullanın.

    ![SSH istemcisini kullanarak kümede oturum açma](./media/apache-domain-joined-create-configure-enterprise-security-cluster/hdinsight-image-0139.jpg)

Bu hesapla oturum açabiliyorsanız, ESP kümenizi şirket içi Active Directory örneğinizle eşitlemek üzere doğru şekilde yapılandırırsınız.

## <a name="next-steps"></a>Sonraki adımlar

ESP [ile Apache Hadoop güvenlik için bir giriş](hdinsight-security-overview.md)okuyun.
