---
title: Yönetilen bir etki alanına Windows Server VM'ye katılma | Microsoft Dokümanlar
description: Bu eğitimde, Windows Server sanal makinesine Azure Active Directory Etki Alanı Hizmetleri yönetilen bir etki alanına nasıl katılacağınızı öğrenin.
author: iainfoulds
manager: daveba
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: tutorial
ms.date: 03/30/2020
ms.author: iainfou
ms.openlocfilehash: 1ac508fc9fee07482e475c46e1db262c8bfa1a12
ms.sourcegitcommit: efefce53f1b75e5d90e27d3fd3719e146983a780
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80476213"
---
# <a name="tutorial-join-a-windows-server-virtual-machine-to-a-managed-domain"></a>Öğretici: Yönetilen bir etki alanına Windows Server sanal makineye katılın

Azure Active Directory Etki Alanı Hizmetleri (Azure AD DS), Windows Server Active Directory ile tam uyumlu etki alanı birleştirme, grup ilkesi, LDAP, Kerberos/NTLM kimlik doğrulaması gibi yönetilen etki alanı hizmetleri sağlar. Azure AD DS yönetilen etki alanı yla, Azure'daki sanal makinelere (VM) etki alanı birleştirme özellikleri ve yönetimi sağlayabilirsiniz. Bu öğretici, windows server VM'yi nasıl oluşturabileceğinizi ve azure AD DS yönetilen bir etki alanına nasıl katılacağınızı gösterir.

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Windows Server VM oluşturma
> * Windows Server VM'yi Azure sanal ağına bağlayın
> * Azure AD DS yönetilen etki alanına VM'ye katılın

Azure aboneliğiniz yoksa, başlamadan önce [bir hesap oluşturun.](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdaki kaynaklara ihtiyacınız var:

* Etkin bir Azure aboneliği.
    * Azure aboneliğiniz yoksa [bir hesap oluşturun.](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
* Aboneliğinizle ilişkili bir Azure Etkin Dizin kiracısı, şirket içi bir dizini veya yalnızca bulut dizininizle eşitlenir.
    * Gerekirse, [bir Azure Etkin Dizin kiracısı oluşturun][create-azure-ad-tenant] veya [bir Azure aboneliğini hesabınızla ilişkilendirin.][associate-azure-ad-tenant]
* Azure Etkin Dizin Etki Alanı Hizmetleri, Azure AD kiracınızda etkin leştirilmiş ve yapılandırılan bir etki alanı yönetildi.
    * Gerekirse, [bir Azure Etkin Dizin Etki Alanı Hizmetleri örneği oluşturun ve yapılandırın.][create-azure-ad-ds-instance]
* Azure AD DS yönetilen etki alanının bir parçası olan bir kullanıcı hesabı.
    * Hesabın Azure AD DS yönetilen etki alanında oturum açabilmesi için Azure AD Connect parola karma eşitleme veya self servis parola sıfırlama gerçekleştirildiğinden emin olun.
* Azure AD DS sanal ağınızda dağıtılan bir Azure Bastion ana bilgisayarı.
    * Gerekirse, [bir Azure Bastion ana bilgisayarı oluşturun.][azure-bastion]

Zaten etki alanına katılmak istediğiniz bir VM'invarsa, VM'ye katılmak için [Azure AD DS yönetilen etki alanına katılın.](#join-the-vm-to-the-azure-ad-ds-managed-domain)

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

Bu eğitimde, Azure portalını kullanarak Azure AD DS yönetilen etki alanınıza katılmak için bir Windows Server VM oluşturursunuz. Başlamak için önce [Azure portalında](https://portal.azure.com)oturum açın.

## <a name="create-a-windows-server-virtual-machine"></a>Windows Server sanal makine oluşturma

Bir bilgisayara Azure AD DS yönetilen etki alanına nasıl katılacağını görmek için bir Windows Server VM oluşturalım. Bu VM, Azure AD DS yönetilen etki alanına bağlantı sağlayan bir Azure sanal ağına bağlıdır. Azure AD DS yönetilen etki alanına katılma işlemi, düzenli bir şirket içi Active Directory Domain Services etki alanına katılmakla aynıdır.

Zaten etki alanına katılmak istediğiniz bir VM'invarsa, VM'ye katılmak için [Azure AD DS yönetilen etki alanına katılın.](#join-the-vm-to-the-azure-ad-ds-managed-domain)

1. Azure portalı menüsünden veya **Ana** sayfadan **kaynak oluştur'u**seçin.

1. **Başlat'tan** **Windows Server 2016 Datacenter'ı**seçin.

    ![Azure portalında bir Windows Server 2016 Datacenter VM oluşturmayı seçin](./media/join-windows-vm/select-vm-image.png)

1. Temel **Bilgiler** penceresinde, sanal makinenin çekirdek ayarlarını yapılandırın. *Kullanılabilirlik seçenekleri,* *Resim*ve *Boyut*için varsayılanları bırakın.

    | Parametre            | Önerilen değer   |
    |----------------------|-------------------|
    | Kaynak grubu       | *myResourceGroup* gibi bir kaynak grubu seçme veya oluşturma |
    | Sanal makine adı | *myVM* gibi VM için bir ad girin |
    | Bölge               | *Doğu ABD* gibi VM'nizi oluşturmak için bölgeyi seçin |
    | Kullanıcı adı             | *Azureuser* gibi VM'de oluşturmak üzere yerel yönetici hesabı için bir kullanıcı adı girin |
    | Parola             | Yerel yöneticinin VM'de oluşturması için güvenli bir parola girin ve sonra onaylayın. Etki alanı kullanıcı hesabının kimlik bilgilerini belirtmeyin. |

1. Varsayılan olarak, Azure'da oluşturulan VM'lere RDP kullanarak Internet'ten erişilebilir. RDP etkinleştirildiğinde, otomatik oturum açma saldırıları nın gerçekleşmesi olasıdır ve bu da denemelerde birden çok başarısız art arda oturum açma nedeniyle *yönetici* veya *yönetici* gibi ortak adlara sahip hesapları devre dışı bırakır.

    RDP yalnızca gerektiğinde etkinleştirilmeli ve bir dizi yetkili IP aralığıyla sınırlandırılmalıdır. Bu yapılandırma VM'nin güvenliğini artırmaya yardımcı olur ve olası saldırı alanını azaltır. Veya yalnızca TLS üzerinden Azure portalı üzerinden erişime izin veren bir Azure Bastion ana bilgisayarı oluşturun ve kullanın. Bu öğreticinin bir sonraki adımında, VM'ye güvenli bir şekilde bağlanmak için bir Azure Bastion ana bilgisayarı kullanırsınız.

    **Genel gelen bağlantı noktaları**altında, *Yok'u*seçin.

1. Bittiğinde, **Sonraki: Diskler'i**seçin.
1. **OS disk türü**için açılan menüden Standart *SSD'yi*seçin, ardından **İleri: Ağ'** ı seçin.
1. VM'niz, Azure AD DS yönetilen etki alanınızın dağıtılan alt ağıyla iletişim kurabilen bir Azure sanal ağ alt ağına bağlanmalıdır. Azure AD DS yönetilen bir etki alanının kendi özel alt ağına dağıtılmasını öneririz. VM'nizi Azure AD DS yönetilen etki alanadınız ile aynı alt ağda dağıtmayın.

    VM'nizi dağıtmanın ve uygun bir sanal ağ alt ağına bağlanmanın iki ana yolu vardır:
    
    * Azure AD DS yönetilen etki alanınızın dağıtıldığı sanal ağda bir alt ağ oluşturun veya varolan bir alt ağ seçin.
    * Azure sanal ağ [eşlemi][vnet-peering]kullanarak bir Azure sanal ağında bir alt ağ seçin.
    
    Azure AD DS örneğiniz için alt ağa bağlı olmayan bir sanal ağ alt ağı seçerseniz, Yönetilen etki alanına VM'ye katamazsınız. Bu öğretici için Azure sanal ağında yeni bir alt ağ oluşturalım.

    **Ağ** bölmesinde, Azure AD DS tarafından yönetilen etki alanınızın dağıtıldığı *aaads-vnet* gibi sanal ağı seçin
1. Bu örnekte, varolan *aaads-subnet* Azure AD DS yönetilen etki alanına bağlı olduğu gösterilir. VM'nizi bu alt ağa bağlamayın. VM için bir alt ağ oluşturmak için **alt ağ yapılandırmasını yönet'i**seçin.

    ![Azure portalında alt ağ yapılandırmasını yönetmeyi seçin](./media/join-windows-vm/manage-subnet.png)

1. Sanal ağ penceresinin sol menüsünde **Adres alanı'nı**seçin. Sanal ağ, varsayılan alt ağ tarafından kullanılan *10.0.2.0/24*tek bir adres alanıyla oluşturulur. *İş yükleri* veya Azure Kalesi gibi diğer alt ağlar da zaten var olabilir.

    Sanal ağa ek bir IP adresi aralığı ekleyin. Bu adres aralığının boyutu ve kullanılacak gerçek IP adresi aralığı, zaten dağıtılan diğer ağ kaynaklarına bağlıdır. IP adres aralığı, Azure veya şirket içi ortamınızdaki varolan adres aralıklarıyla örtüşmemelidir. IP adresi aralığını alt ağa dağıtmayı beklediğiniz VM sayısına göre yeterince büyük boyutlandırdığınızdan emin olun.

    Aşağıdaki örnekte, *10.0.5.0/24* ek bir IP adresi aralığı eklenir. Hazır olduğunda **Kaydet'i**seçin.

    ![Azure portalına ek bir sanal ağ IP adresi aralığı ekleme](./media/join-windows-vm/add-vnet-address-range.png)

1. Ardından, sanal ağ penceresinin sol menüsünde **Alt Ağlar'ı**seçin ve alt ağ eklemek için **+ Subnet'i** seçin.

1. **+ Subnet'i**seçin, ardından *yönetim*gibi alt ağ için bir ad girin. *10.0.5.0/24*gibi bir **Adres aralığı (CIDR bloğu)** sağlayın. Bu IP adresi aralığının varolan diğer Azure veya şirket içi adres aralıklarıyla örtüşmediğinden emin olun. Diğer seçenekleri varsayılan değerleri olarak bırakın ve **ardından Tamam'ı**seçin.

    ![Azure portalında bir alt ağ yapılandırması oluşturma](./media/join-windows-vm/create-subnet.png)

1. Alt ağı oluşturmak birkaç saniye sürer. Oluşturulduktan sonra, alt ağ penceresini kapatmak için *X'i* seçin.
1. VM oluşturmak için **Ağ** bölmesinde, *yönetim*gibi açılır menüden oluşturduğunuz alt ağı seçin. Yine, doğru alt ağı seçtiğinizden ve VM'nizi Azure AD DS yönetilen etki alanınızın aynı alt ağına dağıtmadığınızdan emin olun.
1. **Genel IP**için, yönetime bağlanmak için Azure Bastion'u kullandığınız ve atanmış ortak bir IP adresine gerek duymadığınız için açılır menüden *Yok'u* seçin.
1. Diğer seçenekleri varsayılan değerleri olarak bırakın ve ardından **Yönetim'i**seçin.
1. **Önyükleme tanılamalarını** *Kapalı*olarak ayarlayın. Diğer seçenekleri varsayılan değerleri olarak bırakın, ardından **Gözden Geçir + oluştur'u**seçin.
1. VM ayarlarını gözden geçirin ve ardından **Oluştur'u**seçin.

VM'nin oluşturulması birkaç dakika sürer. Azure portalı dağıtımDurumunu gösterir. VM hazır olduğunda **kaynağa git'i**seçin.

![Başarıyla oluşturulduktan sonra Azure portalındaki VM kaynağına gidin](./media/join-windows-vm/vm-created.png)

## <a name="connect-to-the-windows-server-vm"></a>Windows Server VM'ye bağlanın

VM'lerinize güvenli bir şekilde bağlanmak için bir Azure Bastion ana bilgisayarı kullanın. Azure Bastion ile yönetilen bir ana bilgisayar sanal ağınıza dağıtılır ve VM'lere web tabanlı RDP veya SSH bağlantıları sağlar. VM'ler için ortak IP adresi gerekmez ve harici uzak trafik için ağ güvenlik grubu kurallarını açmanız gerekmez. Web tarayıcınızdan Azure portalını kullanarak VM'lere bağlanırsınız.

VM'nize bağlanmak için bir Bastion ana bilgisayar kullanmak için aşağıdaki adımları tamamlayın:

1. VM'niz için **Genel Bakış** bölmesinde **Connect**ve ardından **Bastion'u**seçin.

    ![Azure portalında Bastion'u kullanarak Windows sanal makinesine bağlanma](./media/join-windows-vm/connect-to-vm.png)

1. Önceki bölümde belirttiğiniz VM'nizin kimlik bilgilerini girin ve ardından **Bağlan'ı**seçin.

   ![Azure portalındaki Bastion ana bilgisayar aracılığıyla bağlantı kurun](./media/join-windows-vm/connect-to-bastion.png)

Gerekirse, web tarayıcınızın Bastion bağlantısının görüntülenmesi için açılır pencereler açmasına izin verin. VM'inize bağlantı sağlamak birkaç saniye sürer.

## <a name="join-the-vm-to-the-azure-ad-ds-managed-domain"></a>Azure AD DS yönetilen etki alanına VM'ye katılın

Oluşturulan VM ve Azure Bastion kullanılarak kurulan web tabanlı RDP bağlantısıyla, artık Windows Server sanal makinesini Azure AD DS yönetilen etki alanına katılalım. Bu işlem, düzenli bir şirket içi Active Directory Etki Alanı Hizmetleri etki alanına bağlanan bir bilgisayarla aynıdır.

1. VM'de oturum açtığınızda **Sunucu Yöneticisi** varsayılan olarak açılmıyorsa, **Başlat** menüsünü seçin ve ardından **Server Manager'ı**seçin.
1. **Server Manager** penceresinin sol bölmesinde **Yerel Sunucu'yu**seçin. Sağ bölmedeki **Özellikler** altında **Çalışma Grubu'nu**seçin.

    ![VM'de Sunucu Yöneticisi'ni açın ve çalışma grubu özelliğini düzenleme](./media/join-windows-vm/server-manager.png)

1. Sistem **Özellikleri** penceresinde, Azure AD DS yönetilen etki alanına katılmak için **Değiştir'i** seçin.

    ![Çalışma grubu veya etki alanı özelliklerini değiştirmeyi seçin](./media/join-windows-vm/change-domain.png)

1. Etki **Alanı** kutusunda, Azure AD DS yönetilen etki alanınızın adını belirtin(aaddscontoso.com gibi, **tamam'ı**seçin. *aaddscontoso.com*

    ![Katılmak için Azure AD DS yönetilen etki alanını belirtin](./media/join-windows-vm/join-domain.png)

1. Etki alanına katılmak için etki alanı kimlik bilgilerini girin. Azure AD DS yönetilen etki alanının bir parçası olan bir kullanıcının kimlik bilgilerini kullanın. Hesap, Azure AD DS yönetilen etki alanının veya Azure AD kiracısının bir parçası olmalıdır - Azure AD kiracınızla ilişkili dış dizinlerden gelen hesaplar etki alanı birleştirme işlemi sırasında doğru şekilde doğru şekilde doğru şekilde doğru şekilde doğru şekilde doğru lanamaz. Hesap kimlik bilgileri aşağıdaki yollardan biriyle belirtilebilir:

    * **UPN formatı** (önerilir) - Azure AD'de yapılandırıldığı şekilde kullanıcı hesabı için kullanıcı anaadı (UPN) soneki girin. Örneğin, kullanıcı *contosoadmin* UPN soneki olacaktır. `contosoadmin@aaddscontoso.onmicrosoft.com` UPN biçiminin *SAMAccountName* biçimi yerine etki alanında oturum açmak için güvenilir bir şekilde kullanılabildiği birkaç yaygın kullanım noktası vardır:
        * Bir kullanıcının UPN öneki *deehasareallylongname*gibi uzunsa, *SAMAccountName* otomatik olarak oluşturulabilir.
        * Birden çok kullanıcı, Azure AD kiracınızda dee gibi aynı UPN önekine *sahipse,* *SAMAccountName* biçimleri otomatik olarak oluşturulabilir.
    * **SAMAccountName formatı** - HESAP adını *SAMAccountName* biçiminde girin. Örneğin, kullanıcı *contosoadmin* `AADDSCONTOSO\contosoadmin` *SAMAccountName* olacaktır.

1. Azure AD DS yönetilen etki alanına katılmak birkaç saniye sürer. Tamamlandığında, aşağıdaki ileti sizi etki alanına kabul eder:

    ![Etki alanına hoş geldiniz](./media/join-windows-vm/join-domain-successful.png)

    Devam etmek için **Tamam**'ı seçin.

1. Azure AD DS yönetilen etki alanına katılma işlemini tamamlamak için VM'yi yeniden başlatın.

> [!TIP]
> PowerShell'i [Kullanarak Bilgisayar Ekle][add-computer] cmdlet ile bir VM'ye etki alanına katılabilirsiniz. Aşağıdaki *örnek, AADDSCONTOSO* etki alanına katılır ve vm'yi yeniden başlatır. İstendiğinde, Azure AD DS yönetilen etki alanının bir parçası olan bir kullanıcının kimlik bilgilerini girin:
>
> `Add-Computer -DomainName AADDSCONTOSO -Restart`
>
> Bir VM'ye bağlanmadan ve bağlantıyı el ile yapılandırmadan etki alanına katılmak için [Set-AzVmAdDomainExtension][set-azvmaddomainextension] Azure PowerShell cmdlet'ini kullanabilirsiniz.

Windows Server VM yeniden başlatıldıktan sonra, Azure AD DS yönetilen etki alanında uygulanan tüm ilkeler VM'ye itilir. Artık uygun etki alanı kimlik bilgilerini kullanarak Windows Server VM'de oturum açabilirsiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Sonraki öğreticide, Azure AD DS yönetilen etki alanını yönetmenize izin veren yönetim araçlarını yüklemek için bu Windows Server VM'yi kullanırsınız. Bu öğretici seriye devam etmek istemiyorsanız, [VM'yi silmek](#delete-the-vm)için aşağıdaki temizleme adımlarını gözden geçirin. Aksi takdirde, [sonraki öğretici devam edin.](#next-steps)

### <a name="un-join-the-vm-from-azure-ad-ds-managed-domain"></a>Azure AD DS yönetilen etki alanından VM'ye katılma

VM'yi Azure AD DS yönetilen etki alanından kaldırmak için, [VM'yi bir etki alanına katılmak](#join-the-vm-to-the-azure-ad-ds-managed-domain)için adımları yeniden izleyin. Azure AD DS yönetilen etki alanına katılmak yerine, varsayılan *WORKGROUP*gibi bir çalışma grubuna katılmayı seçin. VM yeniden başlatıldıktan sonra, bilgisayar nesnesi Azure AD DS yönetilen etki alanından kaldırılır.

VM'yi etki alanından katılmadan [silerseniz,](#delete-the-vm) artık bir bilgisayar nesnesi Azure AD DS'de kalır.

### <a name="delete-the-vm"></a>VM’yi silin

Bu Windows Server VM'yi kullanmıyorsanız, aşağıdaki adımları kullanarak VM'yi silin:

1. Sol menüden **Kaynak gruplarını** seçin
1. *myResourceGroup*gibi kaynak grubunuzu seçin.
1. *myVM*gibi VM'nizi seçin, ardından **Sil'i**seçin. Kaynak silme işlemini onaylamak için **Evet'i** seçin. VM'nin silinması birkaç dakika sürer.
1. VM silindiğinde, işletim sistemi diskini, ağ arabirim kartını ve *myVM* önekine sahip diğer kaynakları seçin ve silin.

## <a name="troubleshoot-domain-join-issues"></a>Sorun giderme etki alanı birleştirme sorunları

Windows Server VM, düzenli bir şirket içi bilgisayarın Active Directory Domain Services etki alanına katılması gibi Azure AD DS yönetilen etki alanına başarıyla katılmalıdır. Windows Server VM, Azure AD DS yönetilen etki alanına katılamazsa, bu bir bağlantı veya kimlik bilgileriyle ilgili bir sorun olduğunu gösterir. Yönetilen etki alanına başarılı bir şekilde katılmak için aşağıdaki sorun giderme bölümlerini gözden geçirin.

### <a name="connectivity-issues"></a>Bağlantı sorunları

Etki alanına katılmak için kimlik bilgilerini isteyen bir istem almazsanız, bir bağlantı sorunu vardır. VM, sanal ağdaki Azure AD DS yönetilen etki alanına erişemez.

Bu sorun giderme adımlarının her birini denedikten sonra, yönetilen etki alanına Windows Server VM'ye katılmayı deneyin.

* VM'nin Azure AD DS'nin etkinleştirdiği sanal ağa bağlı olduğunu veya eşlenmiş bir ağ bağlantısı olduğunu doğrulayın.
* Yönetilen etki alanının DNS etki alanı adını `ping aaddscontoso.com`ping deneyin, gibi.
    * Ping isteği başarısız olursa, yönetilen etki alanının IP adreslerini `ping 10.0.0.4`pinglemeyi deneyin, örneğin. Azure kaynakları listenizden Azure AD DS yönetilen etki alanını seçtiğinizde ortamınızın IP adresi *Özellikler* sayfasında görüntülenir.
    * Ip adresini ping ancak etki alanı ping, DNS yanlış yapılandırılmış olabilir. Yönetilen etki alanının IP adreslerinin sanal ağ için DNS sunucusu olarak yapılandırıldığından onaylayın.
* Komutu kullanarak sanal makinedeki DNS çözümleyici `ipconfig /flushdns` önbelleğini temizlemeye çalışın.

### <a name="credentials-related-issues"></a>Kimlik bilgileriyle ilgili sorunlar

Etki alanına katılmak için kimlik bilgilerini isteyen bir istem alırsanız, ancak bu kimlik bilgilerini girdikten sonra bir hata alırsanız, VM Azure AD DS yönetilen etki alanına bağlanabilir. Sağladığınız kimlik bilgileri, VM'nin Azure AD DS yönetilen etki alanına katılmasına izin vermez.

Bu sorun giderme adımlarının her birini denedikten sonra, yönetilen etki alanına Windows Server VM'ye katılmayı deneyin.

* Belirttiğiniz kullanıcı hesabının Azure AD DS yönetilen etki alanına ait olduğundan emin olun.
* Hesabın Azure AD DS yönetilen etki alanının veya Azure AD kiracısının bir parçası olduğunu doğrulayın. Azure AD kiracınızla ilişkili dış dizinlerden gelen hesaplar, etki alanı birleştirme işlemi sırasında doğru şekilde doğru şekilde doğru şekilde doğru şekilde doğru şekilde doğru şekilde doğru şekilde doğru şekilde doğru şekilde doğru şekilde doğru yapamaz.
* Gibi kimlik bilgilerini belirtmek için UPN `contosoadmin@aaddscontoso.onmicrosoft.com`biçimini kullanmayı deneyin. Kiracınızda aynı UPN önekine sahip çok sayıda kullanıcı varsa veya UPN önekiniz aşırı uzunsa, hesabınızdaki *SAMAccountName* otomatik olarak oluşturulabilir. Bu gibi durumlarda, hesabınızın *SAMAccountName* biçimi, şirket içi etki alanınızda beklediğiniz veya kullandığınızdan farklı olabilir.
* Yönetilen etki alanınıza [parola eşitlemeyi etkinleştirdiğinizi][password-sync] denetleyin. Bu yapılandırma adımı olmadan, oturum açma girişiminizde doğru bir şekilde doğrulamak için azure AD DS yönetilen etki alanında gerekli parola hibirleri bulunmaz.
* Parola eşitlemesinin tamamlanmasını bekleyin. Bir kullanıcı hesabının parolası değiştirildiğinde, Azure AD'den otomatik arka plan eşitlemesi parolayı Azure AD DS'de güncelleştirir. Parolanın etki alanına katılma kullanımı için kullanılabilir olması biraz zaman alır.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Windows Server VM oluşturma
> * Windows Server VM'ye Azure sanal ağına bağlanma
> * Azure AD DS yönetilen etki alanına VM'ye katılın

Azure AD DS yönetilen etki alanınızı yönetmek için Active Directory Administrative Center'ı (ADAC) kullanarak bir yönetim VM'si yapılandırın.

> [!div class="nextstepaction"]
> [Yönetim VM'ine yönetim araçları yükleme](tutorial-create-management-vm.md)

<!-- INTERNAL LINKS -->
[create-azure-ad-tenant]: ../active-directory/fundamentals/sign-up-organization.md
[associate-azure-ad-tenant]: ../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md
[create-azure-ad-ds-instance]: tutorial-create-instance.md
[vnet-peering]: ../virtual-network/virtual-network-peering-overview.md
[password-sync]: active-directory-ds-getting-started-password-sync.md
[add-computer]: /powershell/module/microsoft.powershell.management/add-computer
[azure-bastion]: ../bastion/bastion-create-host-portal.md
[set-azvmaddomainextension]: /powershell/module/az.compute/set-azvmaddomainextension
