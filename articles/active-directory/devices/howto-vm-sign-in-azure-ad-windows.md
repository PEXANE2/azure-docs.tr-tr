---
title: Azure Active Directory 'i kullanarak Azure'da Windows sanal makinesinde oturum açın (Önizleme)
description: Azure AD,Windows çalıştıran bir Azure VM'de oturum açma
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: conceptual
ms.date: 10/29/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sandeo
ms.collection: M365-identity-device-management
ms.openlocfilehash: 88ae3c45126403161e35ec46e5ccc2666c3edb55
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80050072"
---
# <a name="sign-in-to-windows-virtual-machine-in-azure-using-azure-active-directory-authentication-preview"></a>Azure Active Directory kimlik doğrulamasını kullanarak Azure'da Windows sanal makinesinde oturum açma (Önizleme)

Kuruluşlar artık **Windows Server 2019 Datacenter sürümü** veya Windows **10 1809** ve sonrası çalıştıran Azure sanal makineleri (VM'ler) için Azure Active Directory (AD) kimlik doğrulamasını kullanabilir. Azure AD'yi VM'lere kimlik doğrulamak için kullanmak, ilkeleri merkezi olarak denetlemeniz ve uygulamanız için bir yol sağlar. Azure Role Tabanlı Erişim Denetimi (RBAC) ve Azure AD Koşullu Erişim gibi araçlar, VM'ye kimlerin erişebileceğini denetlemenize olanak tanır. Bu makalede, Azure AD kimlik doğrulamasını kullanmak için bir Windows Server 2019 VM'i nasıl oluşturup yapılandırabileceğiniz gösterilmektedir.

|     |
| --- |
| Azure Windows VM'leri için Azure AD oturum açma özelliği, Azure Etkin Dizin'in genel önizleme özelliğidir. Önizlemeler hakkında daha fazla bilgi için Microsoft [Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) bakın|
|     |

Azure'da Windows VM'lerinde oturum açmak için Azure AD kimlik doğrulamasını kullanmanın birçok avantajı vardır:

- Normalde kullandığınız federe veya yönetilen Azure AD kimlik bilgilerini kullanın.
- Artık yerel yönetici hesaplarını yönetmek zorunda değil.
- Azure RBAC, vm'lere ihtiyaca göre uygun erişimi vermenize ve artık ihtiyaç duyulmadığında kaldırmanıza olanak tanır.
- Bir VM'ye erişime izin vermeden önce, Azure AD Koşullu Erişim gibi ek gereksinimleri zorlayabilir: 
   - Multi-factor authentication
   - Oturum açma risk kontrolü
- VDI dağıtımlarınızın parçası olan Azure Windows VM'lerinin Azure AD birleştirmesini otomatikleştirin ve ölçeklendirin.

> [!NOTE]
> Bu özelliği etkinleştirdikten sonra Azure'daki Windows VM'leriniz Azure AD'ye katılır. Şirket içi AD veya Azure AD DS gibi diğer etki alanına katılamazsınız. Bunu yapmanız gerekiyorsa, uzantıyı katarak VM'nin Azure REKLAM kiracınızdan bağlantısını kesmeniz gerekir.

## <a name="requirements"></a>Gereksinimler

### <a name="supported-azure-regions-and-windows-distributions"></a>Desteklenen Azure bölgeleri ve Windows dağıtımları

Aşağıdaki Windows dağıtımları şu anda bu özelliğin önizlemesi sırasında desteklenir:

- Windows Server 2019 Datacenter
- Windows 10 1809 ve sonrası

> [!IMPORTANT]
> Azure AD'ye katılan VM'lere uzaktan bağlantı yalnızca Azure AD'sı olan Windows 10 bilgisayarlarından veya karma Azure AD'ın VM ile **aynı** dizine katılmasına izin verilir. 

Aşağıdaki Azure bölgeleri şu anda bu özelliğin önizlemesi sırasında desteklenir:

- Tüm Azure global bölgeleri

> [!IMPORTANT]
> Bu önizleme özelliğini kullanmak için yalnızca desteklenen bir Windows dağıtımına ve desteklenen bir Azure bölgesinde dağıtın. Özellik şu anda Azure Genel'de veya egemen bulutlarda desteklenmez.

### <a name="network-requirements"></a>Ağ gereksinimleri

Azure'daki Windows VM'leriniz için Azure AD kimlik doğrulamasını etkinleştirmek için, VM ağ yapılandırmanızın TCP bağlantı noktası 443 üzerinden aşağıdaki uç noktalara giden erişime izin verdiğinden emin olmanız gerekir:

- https:\//enterpriseregistration.windows.net
- https:\//login.microsoftonline.com
- https:\//device.login.microsoftonline.com
- https:\//pas.windows.net

## <a name="enabling-azure-ad-login-in-for-windows-vm-in-azure"></a>Azure'da Windows VM için Azure AD oturum açmayı etkinleştirme

Azure'da Windows VM için Azure AD oturum açma'yı kullanmak için önce Windows VM'niz için Azure AD oturum açma seçeneğini etkinleştirmeniz ve ardından VM'ye giriş yapma yetkisi olan kullanıcılar için RBAC rol atamaları yapılandırmanız gerekir.
Windows VM'niz için Azure AD oturum açmanızı etkinleştirmenin birden çok yolu vardır:

- Windows VM oluştururken Azure portalı deneyimini kullanma
- Windows VM oluştururken **veya varolan bir Windows VM için** Azure Bulut Kabuğu deneyimini kullanma

### <a name="using-azure-portal-create-vm-experience-to-enable-azure-ad-login"></a>Azure portalının kullanılması, Azure AD oturum açmanızı etkinleştirmek için VM deneyimi oluşturur

Windows Server 2019 Datacenter veya Windows 10 1809 ve daha sonraki VM görüntüleri için Azure AD oturum açma'yı etkinleştirebilirsiniz. 

Azure'da Azure'da Bir Windows Server 2019 Datacenter VM oluşturmak için: 

1. VM'ler oluşturmak için erişimi olan bir hesapla [Azure portalında](https://portal.azure.com)oturum açın ve **+ Kaynak Oluştur'u**seçin.
1. Market arama çubuğunda **Windows Server** yazın.
   1. **Windows Server'ı** tıklatın ve bir yazılım planı açılır listesini seçin'den Windows **Server 2019 Datacenter'ı** seçin.
   1. **Oluştur**'a tıklayın.
1. "Yönetim" sekmesinde, Kapalı'dan **Açık'a**Azure Etkin Dizin bölümü altında **AAD kimlik bilgileriyle (Önizleme) Oturum** Açma seçeneğini etkinleştirin.
1. Kimlik bölümü altında **yönetilen kimlik atanan Sistemin** **A.B.** olarak ayarlandıklarına emin olun. Azure AD kimlik bilgileriyle Oturum Açma'yı etkinleştirdikten sonra bu eylem otomatik olarak gerçekleştirilmelidir.
1. Sanal bir makine oluşturma deneyiminin geri kalanını gözden geçirin. Bu önizleme sırasında, VM için bir yönetici kullanıcı adı ve parola oluşturmanız gerekir.

![Azure AD kimlik bilgileriyle giriş bir VM oluşturma](./media/howto-vm-sign-in-azure-ad-windows/azure-portal-login-with-azure-ad.png)

> [!NOTE]
> Azure AD kimlik bilgilerinizi kullanarak VM'de oturum açmak için öncelikle VM için rol atamalarını aşağıdaki bölümlerden birinde açıklandığı şekilde yapılandırmanız gerekir.

### <a name="using-the-azure-cloud-shell-experience-to-enable-azure-ad-login"></a>Azure AD oturum açmanızı etkinleştirmek için Azure Bulut Kabuğu deneyimini kullanma

Azure Cloud Shell, bu makaledeki adımları çalıştırmak için kullanabileceğiniz ücretsiz bir etkileşimli kabuktur. Yaygın kullanılan Azure araçları hesabınızla kullanmanız için Cloud Shell'de önceden yüklenir ve yapılandırılır. Kopyala’ya tıklayarak kodu kopyalamanız, Cloud Shell’e yapıştırmanız ve Enter tuşuna basarak çalıştırmanız yeterlidir. Cloud Shell’i açmanın birkaç yolu vardır:

Kod bloğunun sağ üst köşesindeki Deneyin’i seçin.
Cloud Shell’i tarayıcınızda açın.
[Azure portalının](https://portal.azure.com) sağ üst köşesindeki menüde yer alan Cloud Shell düğmesini seçin.

CLI'yi yerel olarak yüklemeyi ve kullanmayı seçerseniz, bu makalede Azure CLI sürümü 2.0.31 veya sonraki sürümlerini çalıştırdığınız gerekir. Sürümü bulmak için az --version komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekiyorsa, [Azure CLI yükle](/cli/azure/install-azure-cli)makalesine bakın.

1. [az group create](/cli/azure/group#az-group-create) ile bir kaynak grubu oluşturun. 
1. Desteklenen bir bölgede desteklenen bir dağıtım kullanarak [az vm oluşturmak](/cli/azure/vm#az-vm-create) ile bir VM oluşturun. 
1. Azure AD oturum açma VM uzantısını yükleyin. 

Aşağıdaki örnek, win2019Datacenter kullanan myVM adlı bir VM'yi southcentralus bölgesindeki myResourceGroup adlı bir kaynak grubuna dağıtır. Aşağıdaki örneklerde, gerektiğinde kendi kaynak grubunuzu ve VM adlarınızı sağlayabilirsiniz.

```AzureCLI
az group create --name myResourceGroup --location southcentralus

az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image Win2019Datacenter \
    --assign-identity \
    --admin-username azureuser \
    --admin-password yourpassword
```

> [!NOTE]
> Azure AD oturum açma VM uzantısını yüklemeden önce Sistem atanmış yönetilen kimliği sanal makinenizde etkinleştirmeniz gerekir.

VM’yi ve destekleyici kaynakları oluşturmak birkaç dakika sürer.

Son olarak, Windows VM için Azure AD oturum açma'yı etkinleştirmek için Azure AD oturum açma VM uzantısını yükleyin. VM uzantıları, Azure sanal makinelerinde dağıtım sonrası yapılandırma ve otomasyon görevleri sağlayan küçük uygulamalardır. MyResourceGroup kaynak grubuna myVM adlı VM'ye AADLoginForWindows uzantısını yüklemek için [az vm uzantısı](/cli/azure/vm/extension#az-vm-extension-set) kümesini kullanın:

> [!NOTE]
> AADLoginForWindows uzantısını varolan bir Windows Server 2019 veya Windows 10 1809 ve daha sonra VM'ye yükleyip Azure AD kimlik doğrulaması için etkinleştirebilirsiniz. AZ CLI bir örnek aşağıda gösterilmiştir.

```AzureCLI
az vm extension set \
    --publisher Microsoft.Azure.ActiveDirectory \
    --name AADLoginForWindows \
    --resource-group myResourceGroup \
    --vm-name myVM
```

Uzantısı `provisioningState` `Succeeded` VM'ye yüklendikten sonra, bu gösteri gösterilir.

## <a name="configure-role-assignments-for-the-vm"></a>VM için rol atamalarını yapılandırma

VM'yi oluşturduğunuza göre, VM'de kimlerin oturum açabileceğini belirlemek için Azure RBAC ilkesini yapılandırmanız gerekir. VM oturum açma yetkisi vermek için iki RBAC rolü kullanılır:

- **Sanal Makine Yöneticisi Girişi**: Bu role sahip kullanıcılar yönetici ayrıcalıklarına sahip bir Azure sanal makinesinde oturum açabilir.
- **Sanal Makine Kullanıcı Girişi**: Bu role sahip kullanıcılar, düzenli kullanıcı ayrıcalıklarına sahip bir Azure sanal makinesinde oturum açabilir.

> [!NOTE]
> Bir kullanıcının RDP üzerinden VM'de oturum açmasına izin vermek için Sanal Makine Yöneticisi Girişi veya Sanal Makine Kullanıcı Girişi rolünü atamanız gerekir. VM için atanan Sahip veya Katılımcı rollerine sahip bir Azure kullanıcısının, RDP üzerinden VM'de oturum açma ayrıcalığı otomatik olarak gerekmez. Bu, sanal makineleri kontrol eden kişiler ile sanal makinelere erişebilen kişiler kümesi arasında denetlenmiş ayrım sağlamaktır.

VM için rol atamalarını yapılandırmanın birden çok yolu vardır:

- Azure AD Portalı deneyimini kullanma
- Azure Bulut BulutU deneyimini kullanma

### <a name="using-azure-ad-portal-experience"></a>Azure AD Portalı deneyimini kullanma

Azure AD etkin Windows Server 2019 Datacenter VM'leriniz için rol atamaları yapılandırmak için:

1. Belirli sanal makineye genel bakış sayfasına gidin
1. Menü seçeneklerinden **Erişim denetimi (IAM)** seçeneğini belirleyin
1. Rol Atama ekle bölmesini açmak için **Ekle**, **Rol Atama** ekle'yi seçin.
1. **Rol** açılır listesinde, Sanal Makine Yöneticisi **Girişi** veya Sanal **Makine Kullanıcı Girişi**gibi bir rol seçin.
1. **Select** alanında bir kullanıcı, grup, hizmet sorumlusu veya yönetilen kimlik seçin. Listede güvenlik sorumlusunu görmüyorsanız **Seç** kutusuna giriş yaparak dizinde görünen ad, e-posta adresi ve nesne tanımlayıcısı arayabilirsiniz.
1. Rolü atamak için **Kaydet'i**seçin.

Birkaç dakika sonra, güvenlik ilkesiseçili kapsamda rol atanır.

![VM'ye erişecek kullanıcılara rol atama](./media/howto-vm-sign-in-azure-ad-windows/azure-portal-access-control-assign-role.png)

### <a name="using-the-azure-cloud-shell-experience"></a>Azure Bulut BulutU deneyimini kullanma

Aşağıdaki örnekte, sanal makine yöneticisi giriş rolünü geçerli Azure kullanıcınız için VM'ye atamak için [az rol ataması oluşturma](/cli/azure/role/assignment#az-role-assignment-create) kullanır. Etkin Azure hesabınızın kullanıcı adı [az hesap göster](/cli/azure/account#az-account-show)ile elde edilir ve kapsam [az vm show](/cli/azure/vm#az-vm-show)ile önceki adımda oluşturulan VM olarak ayarlanır. Kapsam bir kaynak grubu veya abonelik düzeyinde de atanabilir ve normal RBAC devralma izinleri geçerlidir. Daha fazla bilgi için [Bkz. Rol Tabanlı Erişim Denetimleri.](../../virtual-machines/linux/login-using-aad.md)

```   AzureCLI
username=$(az account show --query user.name --output tsv)
vm=$(az vm show --resource-group myResourceGroup --name myVM --query id -o tsv)

az role assignment create \
    --role "Virtual Machine Administrator Login" \
    --assignee $username \
    --scope $vm
```

> [!NOTE]
> AAD etki alanınız ve oturum açma kullanıcı adı alan adınız eşleşmiyorsa, kullanıcı hesabınızın `--assignee`nesne kimliğini yalnızca kullanıcı adı ile `--assignee-object-id`değil, [Az reklam kullanıcı listesi](/cli/azure/ad/user#az-ad-user-list)ile kullanıcı hesabınız için nesne kimliği elde edebilirsiniz.

Azure abonelik kaynaklarınıza erişimi yönetmek için RBAC'ı nasıl kullanacağınız hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

- [RBAC ve Azure CLI'yi kullanarak Azure kaynaklarına erişimi yönetme](/azure/role-based-access-control/role-assignments-cli)
- [RBAC ve Azure portalını kullanarak Azure kaynaklarına erişimi yönetme](/azure/role-based-access-control/role-assignments-portal)
- [RBAC ve Azure PowerShell'i kullanarak Azure kaynaklarına erişimi yönetin.](/azure/role-based-access-control/role-assignments-powershell)

## <a name="using-conditional-access"></a>Koşullu Erişimi Kullanma

Azure'da Azure'da Oturum Açma özelliğiyle etkinleştirilen Windows VM'lerine erişimi yetkilendirmeden önce çok faktörlü kimlik doğrulama veya kullanıcı oturum açma riski denetimi gibi Koşullu Erişim ilkelerini uygulayabilirsiniz. Koşullu Erişim ilkesini uygulamak için bulut uygulamaları veya eylemleri atama seçeneğinden "Azure Windows VM Oturum Açma" uygulamasını seçmeniz ve ardından oturum açma riskini koşul olarak kullanmanız ve/veya hibe erişim denetimi olarak çok faktörlü kimlik doğrulaması gerektirmeniz gerekir. 

> [!NOTE]
> "Azure Windows VM Oturum Açma" uygulamasına erişmek için hibe erişim denetimi olarak "Çok faktörlü kimlik doğrulama gerektirir" sözcüğünün kullanılmasını kullanıyorsanız, hedef Windows'a RDP oturumunu başlatan istemcinin bir parçası olarak çok faktörlü kimlik doğrulama talebi sağlamanız gerekir Azure'da VM. Bunu bir Windows 10 istemcisi üzerinde başarmanın tek yolu, Windows Hello for Business PIN veya rdp istemcisi ile biyometrik autennication kullanmaktır. Windows 10 sürüm 1809'daki RDP istemcisine biyometrik kimlik doğrulama desteği eklendi. Windows Hello for Business kimlik doğrulaması kullanan uzak masaüstü yalnızca cert trust modelini kullanan ve şu anda anahtar güven modeli için kullanılamayan dağıtımlar için kullanılabilir.

## <a name="log-in-using-azure-ad-credentials-to-a-windows-vm"></a>Windows VM'de Azure AD kimlik bilgilerini kullanarak oturum açma

> [!IMPORTANT]
> Azure AD'ye katılan VM'lere uzaktan bağlantı yalnızca Azure AD'sı olan Windows 10 bilgisayarlarından veya karma Azure AD'ın VM ile **aynı** dizine katılmasına izin verilir. Ayrıca, Azure AD kimlik bilgilerini kullanarak RDP'ye göre, kullanıcı iki RBAC rolünden birine, Sanal Makine Yöneticisi Girişi'ne veya Sanal Makine Kullanıcı Girişi'ne ait olmalıdır. Şu anda Azure Bastion, AADLoginForWindows uzantısı ile Azure Active Directory kimlik doğrulamasını kullanarak oturum açmak için kullanılamaz. Yalnızca doğrudan RDP desteklenir.

Azure AD'yi kullanarak Windows Server 2019 sanal makinenize giriş yapmak için: 

1. Azure AD oturumu ile etkinleştirilen sanal makinenin genel bakış sayfasına gidin.
1. Connect to virtual machine blade'i açmak için **Bağlan'ı** seçin.
1. **RDP Dosyasını İndir'i**seçin.
1. Uzak Masaüstü Bağlantısı istemcisini başlatmak için **Aç'ı** seçin.
1. Windows oturum açma iletişim kutusunu başlatmak için **Bağlan'ı** seçin.
1. Azure REKLAM kimlik bilgilerinizi kullanarak oturum açın.

Artık Windows Server 2019 Azure sanal makinesinde VM User veya VM Administrator gibi atanmış rol izinleriyle oturum açtınız. 

> [!NOTE]
> Kaydedebilirsiniz. RDP, Azure portalındaki sanal makineye genel bakış sayfasına gitmek ve bağlantı seçeneğini kullanmak yerine gelecekteki uzak masaüstü bağlantılarını sanal makinenize başlatmak için bilgisayarınızda yerel olarak dosya kurun.

## <a name="troubleshoot"></a>Sorun giderme

### <a name="troubleshoot-deployment-issues"></a>Dağıtım sorunlarını giderme

VM'nin Azure AD join işlemini tamamlaması için AADLoginForWindows uzantısı başarıyla yüklenmelidir. VM uzantısı düzgün yüklenmezse aşağıdaki adımları gerçekleştirin.

1. RDP yerel yönetici hesabını kullanarak VM ve CommandExecuti'n.log altında incelemek  
   
   C:\WindowsAzure\Günlükler\Eklentiler\Microsoft.Azure.ActiveDirectory.AADLoginForWindows\0.3.1.0. 

   > [!NOTE]
   > Uzantı ilk hatadan sonra yeniden başlatılırsa, dağıtım hatası olan günlük CommandExecution_YYYYMMDDHHMMSSSSS.log olarak kaydedilir. "
1. VM'de bir komut istemi açın ve bu sorguları Azure ana bilgisayar da çalışan Örnek Meta Veri Hizmeti (IMDS) Bitiş Noktası'na karşı doğrulayın:

   | Çalıştırmak için komut | Beklenen çıkış |
   | --- | --- |
   | `curl -H @{"Metadata"="true"} "http://169.254.169.254/metadata/instance?api-version=2017-08-01"` | Azure VM hakkında doğru bilgi |
   | `curl -H @{"Metadata"="true"} "http://169.254.169.254/metadata/identity/info?api-version=2018-02-01"` | Azure Aboneliği ile ilişkili geçerli Kiracı Kimliği |
   | `curl -H @{"Metadata"="true"} "http://169.254.169.254/metadata/identity/oauth2/token?resource=urn:ms-drs:enterpriseregistration.windows.net&api-version=2018-02-01"` | Bu VM'ye atanan yönetilen kimlik için Azure Active Directory tarafından verilen geçerli erişim belirteci |

   > [!NOTE]
   > Erişim belirteci gibi [http://calebb.net/](http://calebb.net/)bir araç kullanılarak çözülebilir. Erişim belirtecindeki "appid"in VM'ye atanan yönetilen kimlikle eşleştiğini doğrulayın.

1. Komut satırını kullanarak VM'den gerekli uç noktalara erişilebildiğinden emin olun:
   
   - curl https:\//login.microsoftonline.com/ -D –
   - curl https:\/`<TenantID>`/login.microsoftonline.com/ / -D –

   > [!NOTE]
   > Azure `<TenantID>` aboneliğiyle ilişkili Azure AD Kiracı Kimliği ile değiştirin.

   - curl https:\//enterpriseregistration.windows.net/ -D -
   - curl https:\//device.login.microsoftonline.com/ -D -
   - curl https:\//pas.windows.net/ -D -

1. Aygıt Durumu çalıştırılarak `dsregcmd /status`görüntülenebilir. Amaç, Aygıt Durumu'nun `AzureAdJoined : YES`'' olarak göstermesidir.

   > [!NOTE]
   > Azure AD join etkinliği, Kullanıcı Aygıtı Kaydı\Yönetici günlüğü altında Olay görüntüleyicisinde yakalanır.

AADLoginForWindows uzantısı belirli hata koduyla başarısız olursa, aşağıdaki adımları gerçekleştirebilirsiniz:

#### <a name="issue-1-aadloginforwindows-extension-fails-to-install-with-terminal-error-code-1007-and-exit-code--2145648574"></a>Sorun 1: AADLoginForWindows uzantısı terminal hata kodu '1007' ve Çıkış kodu ile yüklemek için başarısız: -2145648574.

Uzantı Azure AD Kiracı bilgilerini sorgulayamadığıiçin bu çıkış kodu DSREG_E_MSI_TENANTID_UNAVAILABLE çevirir.

1. Azure VM'nin Kiracı Kimliğini Örnek Meta veri Hizmeti'nden alabileceğini doğrulayın.

   - Yerel bir yönetici olarak VM'ye RDP ve bu komutu VM'deki yüksek bir komut satırından çalıştırarak bitiş noktasının geçerli Kiracı Kimliğini döndürtuğudoğrulayın:
      
      - curl -H Metadata:truehttp://169.254.169.254/metadata/identity/info?api-version=2018-02-01

1. VM yöneticisi AADLoginForWindows uzantısını yüklemeye çalışır, ancak yönetilen kimlik atanan bir sistem önce VM'yi etkinleştirmemiştir. VM'nin kimlik bıçağına gidin. Sistem atanan sekmesinden, Durum'un A'ya geçiş olduğunu doğrulayın.

#### <a name="issue-2-aadloginforwindows-extension-fails-to-install-with-exit-code--2145648607"></a>Sorun 2: AADLoginForWindows uzantısı Çıkış kodu ile yüklemek için başarısız: -2145648607

Bu Çıkış kodu, uzantı `https://enterpriseregistration.windows.net` bitiş noktasına ulaşamadığı için DSREG_AUTOJOIN_DISC_FAILED çevirir.

1. Komut satırını kullanarak VM'den gerekli uç noktaların erişilebildiğinden doğrulayın:

   - curl https:\//login.microsoftonline.com/ -D –
   - curl https:\/`<TenantID>`/login.microsoftonline.com/ / -D –
   
   > [!NOTE]
   > Azure `<TenantID>` aboneliğiyle ilişkili Azure AD Kiracı Kimliği ile değiştirin. Kiracı kimliğini bulmanız gerekiyorsa, dizin /kiracı kimliğini almak için hesap adınızın üzerine gelebilir veya Azure portalında Azure Active Directory > Properties > Directory ID'yi seçebilirsiniz.

   - curl https:\//enterpriseregistration.windows.net/ -D -
   - curl https:\//device.login.microsoftonline.com/ -D -
   - curl https:\//pas.windows.net/ -D -

1. Komutlardan herhangi biri "Ana bilgisayarı `<URL>`çözemedi" ile başarısız olursa, VM tarafından kullanılan DNS sunucusunu belirlemek için bu komutu çalıştırmayı deneyin.
   
   `nslookup <URL>`

   > [!NOTE] 
   > "login.microsoftonline.com" gibi uç noktalartarafından kullanılan tam nitelikli alan adlarıyla değiştirin. `<URL>`

1. Ardından, ortak bir DNS sunucusu belirtmekomutun başarılı olmasını izin veriyor mu bakın:

   `nslookup <URL> 208.67.222.222`

1. Gerekirse, Azure VM'nin ait olduğu ağ güvenlik grubuna atanan DNS sunucusunu değiştirin.

#### <a name="issue-3-aadloginforwindows-extension-fails-to-install-with-exit-code-51"></a>Sorun 3: AADLoginForWindows uzantısı Çıkış kodu ile yüklemek için başarısız olur: 51

Çıkış kodu 51 "Bu uzantı VM işletim sisteminde desteklenmez" anlamına gelir.

Genel Önizleme'de, AADLoginForWindows uzantısı yalnızca Windows Server 2019 veya Windows 10'a (Derleme 1809 veya sonraki) yüklenmesi amaçlanmıştır. Windows sürümünün desteklendirildiğından emin olun. Windows'un yapısı desteklenmiyorsa, VM Uzantısını kaldırın.

### <a name="troubleshoot-sign-in-issues"></a>Oturum açma sorunları

Azure AD kimlik bilgileriyle RDP'ye çalıştığınızda yapılan bazı sık karşılaşılan hatalar arasında Atanmış RBAC rolleri, yetkisiz istemci veya 2FA oturum açma yöntemi gerekmez. Bu sorunları düzeltmek için aşağıdaki bilgileri kullanın.

Aygıt ve SSO Durumu çalıştırılarak `dsregcmd /status`görüntülenebilir. Amaç, Aygıt Durumu olarak `AzureAdJoined : YES` göstermek `SSO State` ve `AzureAdPrt : YES`göstermek için .

Ayrıca, Azure AD hesaplarını kullanan RDP Oturum Açma, AAD\Operational olay günlükleri altında Olay görüntüleyicisinde yakalanır.

#### <a name="rbac-role-not-assigned"></a>RBAC rolü atanmamış

VM'inize uzak bir masaüstü bağlantısı başlattığınızda aşağıdaki hata iletisini görürseniz: 

- Hesabınız bu aygıtı kullanmanızı engelleyecek şekilde yapılandırıldı. Daha fazla bilgi için sistem yöneticinize başvurun

![Hesabınız bu aygıtı kullanmanızı engelleyecek şekilde yapılandırıldı.](./media/howto-vm-sign-in-azure-ad-windows/rbac-role-not-assigned.png)

Kullanıcıya Sanal Makine Yöneticisi Girişi veya Sanal Makine Kullanıcı Girişi rolü veren VM için [RBAC ilkelerini yapılandırdığınızı](../../virtual-machines/linux/login-using-aad.md) doğrulayın:
 
#### <a name="unauthorized-client"></a>Yetkisiz istemci

VM'inize uzak bir masaüstü bağlantısı başlattığınızda aşağıdaki hata iletisini görürseniz: 

- Kimlik bilgileriniz çalışmadı

![Kimlik bilgileriniz çalışmadı](./media/howto-vm-sign-in-azure-ad-windows/your-credentials-did-not-work.png)

Uzak masaüstü bağlantısını başlatmak için kullandığınız Windows 10 bilgisayarının Azure AD'ye katılan veya karma Azure AD'sinin VM'nizin birleştiği aynı Azure REKLAM dizinine katıldığını doğrulayın. Aygıt kimliği hakkında daha fazla bilgi için, aygıt [kimliği nedir](/azure/active-directory/devices/overview)makalesine bakın.

> [!NOTE]
> Windows 10 20H1, VM'inize uzak masaüstü bağlantısı başlatmak için Azure AD Kayıtlı PC'ye destek ekler. Bunu denemek ve Windows 10'un yeni özelliklerini keşfetmek için Windows Insider Programına katılın.

Ayrıca, Azure AD birleştirme tamamlandıktan sonra AADLoginForWindows uzantısının kaldırılmadığını doğrulayın.
 
#### <a name="mfa-sign-in-method-required"></a>MFA oturum açma yöntemi gerekli

VM'inize uzak bir masaüstü bağlantısı başlattığınızda aşağıdaki hata iletisini görürseniz: 

- Kullanmaya çalıştığınız oturum açma yöntemine izin verilmez. Farklı bir oturum açma yöntemi deneyin veya sistem yöneticinize başvurun.

![Kullanmaya çalıştığınız oturum açma yöntemine izin verilmez.](./media/howto-vm-sign-in-azure-ad-windows/mfa-sign-in-method-required.png)

Kaynağa erişebilmeniz için çok faktörlü kimlik doğrulama (MFA) gerektiren bir Koşullu Erişim ilkesi ni yapılandırmışsanız, Windows 10 bilgisayarının VM işaretlerinize uzak masaüstü bağlantısını güçlü bir Windows Hello gibi kimlik doğrulama yöntemi. Uzak masaüstü bağlantınız için güçlü bir kimlik doğrulama yöntemi kullanmıyorsanız, önceki hatayı görürsünüz.

Windows Hello for Business'ı dağıtmadıysanız ve bu şimdilik bir seçenek değilse, MFA gerektiren bulut uygulamaları listesinden "Azure Windows VM Oturum Açma" uygulamasını hariç tutan Koşullu Erişim ilkesini yapılandırarak MFA gereksinimini hariç tutabilirsiniz. İşletmeler için Windows Hello hakkında daha fazla bilgi edinmek [için, İşletmelere Genel Bakış için Windows Hello'ya](/windows/security/identity-protection/hello-for-business/hello-identity-verification)bakın.

> [!NOTE]
> RDP ile Windows Hello for Business PIN kimlik doğrulaması çeşitli sürümler için Windows 10 tarafından desteklenmiştir, ancak RDP ile Biyometrik kimlik doğrulama desteği Windows 10 sürüm 1809'a eklenmiştir. RDP sırasında Windows Hello for Business auth'u kullanmak yalnızca cert trust modelini kullanan ve şu anda anahtar güven modeli için kullanılamayan dağıtımlar için kullanılabilir.
 
## <a name="preview-feedback"></a>Önizleme geri bildirimi

Bu önizleme özelliği hakkındaki görüşlerinizi paylaşın veya [Azure AD geri bildirim forumunda](https://feedback.azure.com/forums/169401-azure-active-directory?category_id=166032)kullanarak sorunları bildirin.

## <a name="next-steps"></a>Sonraki adımlar

Azure Etkin Dizin hakkında daha fazla bilgi için Azure [Etkin Dizin nedir](/azure/active-directory/fundamentals/active-directory-whatis)
