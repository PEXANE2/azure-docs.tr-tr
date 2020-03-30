---
title: Öğretici - Azure AD Etki Alanı Hizmetlerinde orman güveni oluşturun | Microsoft Dokümanlar
description: Azure AD Etki Alanı Hizmetleri için Azure portalında şirket içi AD DS etki alanına tek yönlü giden bir orman oluşturmayı öğrenin
services: active-directory-ds
author: iainfoulds
manager: daveba
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: conceptual
ms.date: 11/19/2019
ms.author: iainfou
ms.openlocfilehash: 5620d1cdc7dc71bdac17057b9a13a74150b12d5c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77612524"
---
# <a name="tutorial-create-an-outbound-forest-trust-to-an-on-premises-domain-in-azure-active-directory-domain-services-preview"></a>Öğretici: Azure Etkin Dizin Etki Alanı Hizmetleri'nde şirket içi etki alanına giden orman güveni oluşturma (önizleme)

Parola hatalarını eşitleyebileceğiniz veya parolalarını bilmemeleri için yalnızca akıllı kartlar kullanarak oturum açan kullanıcılarınız olduğu ortamlarda, Azure Active Directory Domain Services 'da (AD DS) bir kaynak ormanı kullanabilirsiniz. Kaynak ormanı, Azure AD DS'den bir veya daha fazla şirket içi AD DS ortamına tek yönlü giden güven kullanır. Bu güven ilişkisi, kullanıcıların, uygulamaların ve bilgisayarların Azure AD DS yönetilen etki alanından şirket içi etki alanına karşı kimlik doğrulaması yapmanızı sağlar. Azure AD DS kaynak ormanları şu anda önizlemede.

![Azure AD DS'den şirket içi AD DS'ye orman güveni diyagramı](./media/concepts-resource-forest/resource-forest-trust-relationship.png)

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Azure AD DS bağlantısını desteklemek için DNS'yi şirket içinde AD DS ortamında yapılandırın
> * Şirket içi AD DS ortamında tek yönlü gelen orman güveni oluşturun
> * Azure AD DS'de tek yönlü giden orman güveni oluşturma
> * Kimlik doğrulama ve kaynak erişimi için güven ilişkisini test edin ve doğrulayın

Azure aboneliğiniz yoksa, başlamadan önce [bir hesap oluşturun.](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdaki kaynaklara ve ayrıcalıklara ihtiyacınız vardır:

* Etkin bir Azure aboneliği.
    * Azure aboneliğiniz yoksa [bir hesap oluşturun.](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
* Aboneliğinizle ilişkili bir Azure Etkin Dizin kiracısı, şirket içi bir dizini veya yalnızca bulut dizininizle eşitlenir.
    * Gerekirse, [bir Azure Etkin Dizin kiracısı oluşturun][create-azure-ad-tenant] veya [bir Azure aboneliğini hesabınızla ilişkilendirin.][associate-azure-ad-tenant]
* Bir Kaynak ormanı kullanılarak oluşturulan ve Azure AD kiracınızda yapılandırılan bir Azure Etkin Dizin Etki Alanı Hizmetleri etki alanı yönetildi.
    * Gerekirse, [bir Azure Etkin Dizin Etki Alanı Hizmetleri örneği oluşturun ve yapılandırın.][create-azure-ad-ds-instance-advanced]
    
    > [!IMPORTANT]
    > *Kaynak* ormanı kullanarak Azure AD DS yönetilen bir etki alanı oluşturduğunuzdan emin olun. Varsayılan seçenek bir *kullanıcı* ormanı oluşturur. Yalnızca kaynak ormanları, ön hazırlık AD DS ortamlarına güven oluşturabilir. Ayrıca yönetilen etki alanınız için en az *enterprise* SKU kullanmanız gerekir. Gerekirse, [Azure AD DS yönetilen etki alanı için SKU'yu değiştirin.][howto-change-sku]

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

Bu öğreticide, Azure portalını kullanarak Azure AD DS'den giden orman güvenini oluşturur ve yapılandırırsınız. Başlamak için önce [Azure portalında](https://portal.azure.com)oturum açın.

## <a name="networking-considerations"></a>Ağ konusunda dikkat edilmesi gerekenler

Azure AD DS kaynak ormanı barındıran sanal ağ, şirket içi Active Directory'nize ağ bağlantısı na ihtiyaç duyar. Uygulamalar ve hizmetler, Azure AD DS kaynak ormanını barındıran sanal ağa ağ bağlantısı na da ihtiyaç duyar. Azure AD DS kaynak ormanına ağ bağlantısı her zaman üzerinde ve kararlı olmalıdır aksi takdirde kullanıcılar kaynakları doğrulamak veya kaynaklara erişemeyebilir.

Azure AD DS'de bir orman güvenini yapılandırmadan önce, Azure ve şirket içi ortamı arasındaki ağlarınızın aşağıdaki gereksinimleri karşıladığından emin olun:

* Özel IP adreslerini kullanın. Dinamik IP adresi ataması ile DHCP'ye güvenmeyin.
* Sanal ağ izleme ve yönlendirmenin Azure ile şirket içinde başarılı bir şekilde iletişim kurmasına olanak sağlamak için çakışan IP adresi boşluklarından kaçının.
* Azure sanal ağının siteden siteye (S2S) VPN veya ExpressRoute bağlantısını yapılandırmak için ağ geçidi alt ağına ihtiyacı vardır
* Senaryonuzu destekleyecek yeterli IP adresine sahip alt ağlar oluşturun.
* Azure AD DS'nin kendi alt ağı olduğundan emin olun, bu sanal ağ alt netini uygulama VM'leri ve hizmetleriyle paylaşmayın.
* Bakılmış sanal ağlar geçişli DeğİlDir.
    * Azure sanal ağ eşlemeleri, azure AD DS kaynak ormanı güvenini şirket içi AD DS ortamına kullanmak istediğiniz tüm sanal ağlar arasında oluşturulmalıdır.
* Şirket içi Active Directory ormanınıza sürekli ağ bağlantısı sağlayın. İsteğe bağlı bağlantılar kullanmayın.
* Azure AD DS kaynak orman adınız ile şirket içi Active Directory orman adınız arasında sürekli ad çözümlemesi (DNS) olduğundan emin olun.

## <a name="configure-dns-in-the-on-premises-domain"></a>Şirket içi etki alanında DNS'yi yapılandırma

Azure AD DS yönetilen etki alanını şirket içi ortamdan doğru şekilde çözmek için, varolan DNS sunucularına iletme eklemeniz gerekebilir. Azure AD DS yönetilen etki alanıyla iletişim kurmak için şirket içi ortamı yapılandırmadıysanız, şirket içi AD DS etki alanı için bir yönetim iş istasyonundan aşağıdaki adımları tamamlayın:

1. **Başlangıç'ı seçin | İdari Araçlar | DNS**
1. *myAD01*gibi sağ seçilen DNS sunucusu, **Özellikleri** seçin
1. **İletmeciler'i**seçin, ardından ek iletmeler eklemek için **Edit.**
1. *10.0.1.4* ve *10.0.1.5*gibi Azure AD DS yönetilen etki alanının IP adreslerini ekleyin.

## <a name="create-inbound-forest-trust-in-the-on-premises-domain"></a>Şirket içi etki alanında gelen orman güveni oluşturma

Şirket içi AD DS etki alanı, Azure AD DS yönetilen etki alanı için gelen bir orman güvenine ihtiyaç duyar. Bu güven şirket içi AD DS etki alanında el ile oluşturulmalıdır, ancak Azure portalından oluşturulamaz.

Şirket içi AD DS etki alanında gelen güveni yapılandırmak için, şirket içi AD DS etki alanı için bir yönetim iş istasyonundan aşağıdaki adımları tamamlayın:

1. **Başlangıç'ı seçin | İdari Araçlar | Etkin Dizin Etki Alanları ve Güvenler**
1. *onprem.contoso.com*gibi sağ seçili etki alanı , **Özellikler'i** seçin
1. **Güvenler** sekmesini seçin, ardından **Yeni Güven**

   > [!NOTE]
   > **Güvenler** menüsü seçeneğini görmüyorsanız, *Orman türü*için **Özellikler'in** altından bakın. Yalnızca *kaynak* ormanları güven oluşturabilir. Orman türü *Kullanıcı*ise, güven oluşturamazsınız. Şu anda Azure AD DS yönetilen etki alanının orman türünü değiştirmenin bir yolu yoktur. Yönetilen etki alanını kaynak ormanı olarak silmeniz ve yeniden oluşturmanız gerekir.

1. *aaddscontoso.com*gibi Azure AD DS etki alanı adına ad girin , **ardından İleri'yi** seçin
1. **Bir Orman güveni**oluşturma seçeneğini seçin, ardından tek bir yol **oluşturun: gelen** güven.
1. Yalnızca Bu etki **alanı**için güven oluşturmayı seçin. Bir sonraki adımda, Azure AD DS yönetilen etki alanı için Azure portalında güven oluşturursunuz.
1. **Orman genelinde kimlik doğrulamayı**kullanmayı seçin, ardından güven parolasını girin ve onaylayın. Aynı parola bir sonraki bölümde Azure portalına da girilir.
1. Varsayılan seçenekleri ile sonraki birkaç pencereden adım, sonra Hayır seçeneğini **seçin, giden güven onaylamayın.**
1. **Son**’u seçin

## <a name="create-outbound-forest-trust-in-azure-ad-ds"></a>Azure AD DS'de giden orman güveni oluşturma

Azure AD DS yönetilen etki alanını ve oluşturulan gelen orman güvenini gidermek üzere yapılandırılan şirket içi AD DS etki alanıyla, artık giden orman güveni oluşturuldu. Bu giden orman güveni, şirket içi AD DS etki alanı ile Azure AD DS yönetilen etki alanı arasındaki güven ilişkisini tamamlar.

Azure portalında Azure AD DS yönetilen etki alanı için giden güven oluşturmak için aşağıdaki adımları tamamlayın:

1. Azure portalında, **Azure AD Etki Alanı Hizmetlerini**arayın ve seçin, ardından *yönetilen* etki alanınızı seçin aaddscontoso.com
1. Azure AD DS yönetilen etki alanının sol tarafındaki menüden **Güvenler'i**seçin ve ardından + Güven **Ekle'yi** seçin.
1. Güveninizi tanımlayan bir görüntü adı girin, ardından şirket içinde güvenilen orman DNS adı *(örneğin, onprem.contoso.com*
1. Önceki bölümde şirket içi AD DS etki alanı için gelen orman güvenini yapılandırırken kullanılan aynı güven parolasını sağlayın.
1. Şirket içi AD DS etki alanı için *10.0.2.4* ve *10.0.2.5* gibi en az iki DNS sunucusu sağlayın
1. Hazır olduğunuzda, giden orman güvenini **kaydedin**

    [Azure portalında giden orman güveni oluşturma](./media/create-forest-trust/portal-create-outbound-trust.png)

## <a name="validate-resource-authentication"></a>Kaynak kimlik doğrulamasını doğrulama

Aşağıdaki yaygın senaryolar, orman güveninin kullanıcıları ve kaynaklara erişimi doğru bir şekilde doğru bir şekilde doğruladığını doğrulamanızı sağlar:

* [Azure AD DS kaynak ormanından şirket içi kullanıcı kimlik doğrulaması](#on-premises-user-authentication-from-the-azure-ad-ds-resource-forest)
* [Şirket içi kullanıcıyı kullanarak Azure AD DS kaynak ormanındaki kaynaklara erişin](#access-resources-in-the-azure-ad-ds-resource-forest-using-on-premises-user)
    * [Dosya ve yazıcı paylaşımını etkinleştirme](#enable-file-and-printer-sharing)
    * [Güvenlik grubu oluşturma ve üye ekleme](#create-a-security-group-and-add-members)
    * [Ormanlar arası erişim için dosya paylaşımı oluşturma](#create-a-file-share-for-cross-forest-access)
    * [Bir kaynağa ormanlar arası kimlik doğrulamasını doğrulama](#validate-cross-forest-authentication-to-a-resource)

### <a name="on-premises-user-authentication-from-the-azure-ad-ds-resource-forest"></a>Azure AD DS kaynak ormanından şirket içi kullanıcı kimlik doğrulaması

Windows Server sanal makinesinin Azure AD DS kaynak etki alanına katılması gerekir. Şirket içi kullanıcınızın sanal bir makinede kimlik doğrulamasını test etmek için bu sanal makineyi kullanın.

1. Uzak Masaüstü ve Azure AD DS yönetici kimlik bilgilerinizi kullanarak Azure AD DS kaynak ormanına katılan Windows Server VM'ye bağlanın. Ağ Düzeyinde Kimlik Doğrulama (NLA) hatası alırsanız, kullandığınız kullanıcı hesabının etki alanı kullanıcı hesabı olmadığını kontrol edin.

    > [!NOTE]
    > Azure AD Etki Alanı Hizmetlerine katılan VM'lerinize güvenli bir şekilde bağlanmak için desteklenen Azure bölgelerinde [Azure Bastion Host Hizmetini](https://docs.microsoft.com/azure/bastion/bastion-overview) kullanabilirsiniz.

1. Bir komut istemi açın `whoami` ve şu anda kimlik doğrulaması yapılan kullanıcının ayırt edici adını göstermek için komutu kullanın:

    ```console
    whoami /fqdn
    ```

1. Şirket `runas` içi etki alanından kullanıcı olarak kimlik doğrulaması yapmak için komutu kullanın. Aşağıdaki komutta, `userUpn@trusteddomain.com` güvenilir şirket içi etki alanından bir kullanıcının UPN'i ile değiştirin. Komut, kullanıcının parolasını ister:

    ```console
    Runas /u:userUpn@trusteddomain.com cmd.exe
    ```

1. Kimlik doğrulama başarılı ysa, yeni bir komut istemi açılır. Yeni komut isteminin başlığı. `running as userUpn@trusteddomain.com`
1. Kimlik `whoami /fqdn` doğrulaması yapılan kullanıcının ayırt edici adını şirket içi Etkin Dizinden görüntülemek için yeni komut isteminde kullanın.

### <a name="access-resources-in-the-azure-ad-ds-resource-forest-using-on-premises-user"></a>Şirket içi kullanıcıyı kullanarak Azure AD DS kaynak ormanındaki kaynaklara erişin

Azure AD DS kaynak ormanına katılan Windows Server VM'yi kullanarak, kullanıcıların şirket içi etki alanında bulunan kullanıcılarla şirket içi etki alanında bilgisayarlardan kimlik doğrularken kaynak ormanında barındırılan kaynaklara erişebilecekleri senaryoyu test edebilirsiniz. Aşağıdaki örnekler, çeşitli ortak senaryoları nasıl oluşturup sınayabileceğinizi gösterir.

#### <a name="enable-file-and-printer-sharing"></a>Dosya ve yazıcı paylaşımını etkinleştirme

1. Uzak Masaüstü ve Azure AD DS yönetici kimlik bilgilerinizi kullanarak Azure AD DS kaynak ormanına katılan Windows Server VM'ye bağlanın. Ağ Düzeyinde Kimlik Doğrulama (NLA) hatası alırsanız, kullandığınız kullanıcı hesabının etki alanı kullanıcı hesabı olmadığını kontrol edin.

    > [!NOTE]
    > Azure AD Etki Alanı Hizmetlerine katılan VM'lerinize güvenli bir şekilde bağlanmak için desteklenen Azure bölgelerinde [Azure Bastion Host Hizmetini](https://docs.microsoft.com/azure/bastion/bastion-overview) kullanabilirsiniz.

1. **Windows Ayarlarını**açın, ardından **Ağ ve Paylaşım Merkezi'ni**arayın ve seçin.
1. **Gelişmiş paylaşım** ayarlarını değiştir seçeneğini belirleyin.
1. Etki **Alanı Profili**altında, **dosyayı ve yazıcı paylaşımını aç'ı ve** ardından değişiklikleri **kaydet'i**seçin.
1. **Ağı ve Paylaşım Merkezini**Kapat.

#### <a name="create-a-security-group-and-add-members"></a>Güvenlik grubu oluşturma ve üye ekleme

1. **Active Directory Kullanıcıları ve Bilgisayarları**'nı açın.
1. Alan adını sağ seçin, **Yeni'yi**seçin ve ardından **Kuruluş Birimi'ni**seçin.
1. Ad kutusuna, *Yerel Nesneler*yazın, ardından **Tamam'ı**seçin.
1. Gezinti bölmesinde **LocalObjects'i** seçin ve sağ tıklatın. **Yeni'yi** seçin ve sonra **Gruplayın.**
1. **Grup adı** kutusuna *FileServerAccess* yazın. Grup **Kapsamı**için Etki **Alanı yerel'i**seçin ve **ardından Tamam'ı**seçin.
1. İçerik bölmesinde **FileServerAccess'e**çift tıklayın. **Üye**seçin, **Ekle'yi**seçin, ardından **Konumlar'ı**seçin.
1. **Konum** görünümünden şirket içi Etkin Dizini seçin ve **ardından Tamam'ı**seçin.
1. Kutuyu seçmek **için nesne adlarını girin'e** *Etki Alanı Kullanıcıları* yazın. **Denetim Adlarını**seçin, şirket içi Etkin Dizini için kimlik bilgileri sağlayın, ardından **Tamam'ı**seçin.

    > [!NOTE]
    > Güven ilişkisi yalnızca tek bir yol olduğundan kimlik bilgileri sağlamanız gerekir. Bu, Azure AD DS'deki kullanıcıların kaynaklara erişilemeyecekleri veya güvenilen (şirket içi) etki alanında kullanıcıları veya grupları arayamamak anlamına gelir.

1. Şirket içi Active Dizininizdeki **Etki Alanı Kullanıcıları** grubu **FileServerAccess** grubunun bir üyesi olmalıdır. Grubu kaydetmek ve pencereyi kapatmak için **Tamam'ı** seçin.

#### <a name="create-a-file-share-for-cross-forest-access"></a>Ormanlar arası erişim için dosya paylaşımı oluşturma

1. Windows Server'da VM, Azure AD DS kaynak ormanına katıldı, bir klasör oluşturun ve *CrossForestShare*gibi ad sağlayın.
1. Klasörü sağ seçin ve **Özellikler'i**seçin.
1. **Güvenlik** sekmesini seçin ve ardından **Edit'i**seçin.
1. *CrossForestShare* iletişim kutusu için İzinler'de **Ekle'yi**seçin.
1. Seçmek **için nesne adlarını girin'de** *FileServerAccess* yazın, ardından **Tamam'ı**seçin.
1. **Gruplardan veya kullanıcı adları** listesinden *FileServerAccess'i* seçin. **FileServerAccess İzinleri** listesinde, **Değiştir** ve **Yaz** izinleri için *İzin Ver'i* seçin ve ardından **Tamam'ı**seçin.
1. **Paylaşım** sekmesini seçin, ardından **Gelişmiş Paylaşım'ı seçin...**
1. **Bu klasörü paylaş'ı**seçin, ardından *CrossForestShare*gibi **Share adına** dosya paylaşımı için unutulmaz bir ad girin.
1. **İzinleri**seçin. Herkes **için İzinler** listesinde, **Değişiklik** İzni için İzin Ver'i'ni seçin. **Allow**
1. **Tamam'ı** iki kez seçin ve ardından **Kapat'ı**seçin.

#### <a name="validate-cross-forest-authentication-to-a-resource"></a>Bir kaynağa ormanlar arası kimlik doğrulamasını doğrulama

1. Şirket içi Active Directory'nizden bir kullanıcı hesabı kullanarak şirket içi Active Directory'nize katılan bir Windows bilgisayarında oturum açın.
1. **Windows Gezgini'ni**kullanarak, tam nitelikli ana bilgisayar adını ve `\\fs1.aaddscontoso.com\CrossforestShare`' i gibi paylaşımı kullanarak oluşturduğunuz paylaşıma bağlayın.
1. Yazma iznini doğrulamak için, klasörde sağ seçseçeneğini seçin, **Ardından Metin** **Belgesi'ni**seçin. Varsayılan adı **Yeni Metin Belgesi**kullanın.

    Yazma izinleri doğru ayarlanırsa, yeni bir metin belgesi oluşturulur. Aşağıdaki adımlar daha sonra dosyayı uygun şekilde açar, düzenler ve siler.
1. Okuma iznini doğrulamak için **Yeni Metin Belgesi'ni**açın.
1. Değişiklik iznini doğrulamak için dosyaya metin ekleyin ve **Not Defteri'ni**kapatın. Değişiklikleri kaydetmek istendiğinde **Kaydet'i**seçin.
1. Silme iznini doğrulamak için **Yeni Metin Belgesi'ni** sağ seçin ve **Sil'i**seçin. Dosya silme işlemini onaylamak için **Evet'i** seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Azure AD DS bağlantısını desteklemek için DNS'yi şirket içinde AD DS ortamında yapılandırın
> * Şirket içi AD DS ortamında tek yönlü gelen orman güveni oluşturun
> * Azure AD DS'de tek yönlü giden orman güveni oluşturma
> * Kimlik doğrulama ve kaynak erişimi için güven ilişkisini test edin ve doğrulayın

Azure AD DS'deki orman türleri hakkında daha fazla kavramsal [How do forest trusts work in Azure AD DS?][concepts-trust] bilgi [için][concepts-forest] bkz.

<!-- INTERNAL LINKS -->
[concepts-forest]: concepts-resource-forest.md
[concepts-trust]: concepts-forest-trust.md
[create-azure-ad-tenant]: ../active-directory/fundamentals/sign-up-organization.md
[associate-azure-ad-tenant]: ../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md
[create-azure-ad-ds-instance-advanced]: tutorial-create-instance-advanced.md
[howto-change-sku]: change-sku.md
