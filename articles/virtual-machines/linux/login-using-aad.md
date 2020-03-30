---
title: Azure Active Directory kimlik bilgileriyle bir Linux VM'de oturum açın
description: Azure Active Directory kimlik doğrulamasını kullanarak oturum açabilmek için bir Linux VM'yi nasıl oluşturup yapılandırarak nasıl yapılandırılabildiğini öğrenin.
author: iainfoulds
ms.service: virtual-machines-linux
ms.topic: article
ms.workload: infrastructure
ms.date: 08/29/2019
ms.author: iainfou
ms.openlocfilehash: 2731693667d2129a72da72455c6bbdd74c277697
ms.sourcegitcommit: 07d62796de0d1f9c0fa14bfcc425f852fdb08fb1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80366485"
---
# <a name="preview-log-in-to-a-linux-virtual-machine-in-azure-using-azure-active-directory-authentication"></a>Önizleme: Azure Active Directory kimlik doğrulamasını kullanarak Azure'daki bir Linux sanal makinesinde oturum açın

Azure'daki Linux sanal makinelerinin (VM) güvenliğini artırmak için Azure Active Directory (AD) kimlik doğrulaması ile tümleştirebilirsiniz. Linux VM'leri için Azure AD kimlik doğrulaması kullandığınızda, VM'lere erişime izin veren veya reddeden ilkeleri merkezi olarak denetler ve uygularsınız. Bu makalede, Azure AD kimlik doğrulamasını kullanmak için bir Linux VM'si nasıl oluşturup yapılandırılabildiğiniz gösterilmektedir.


> [!IMPORTANT]
> Azure Etkin Dizin kimlik doğrulaması şu anda genel önizlemededir.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için Microsoft [Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.
> Bu özelliği, testten sonra atmayı beklediğiniz bir sanal makinede kullanın.
>


Azure'da Linux VM'lerinde oturum açmak için Azure AD kimlik doğrulamasını kullanmanın birçok avantajı vardır:

- **Geliştirilmiş güvenlik:**
  - Azure Linux VM'lerinde oturum açmak için kurumsal AD kimlik bilgilerinizi kullanabilirsiniz. Yerel yönetici hesapları oluşturmanız ve kimlik bilgisi ömrünü yönetmenize gerek yoktur.
  - Yerel yönetici hesaplarına olan güveninizi azaltarak, kimlik bilgilerinin kaybolması/çalınması, zayıf kimlik bilgilerini yapılandıran kullanıcılar vb. konusunda endişelenmenize gerek yoktur.
  - Azure AD dizininiz için yapılandırılan parola karmaşıklığı ve parola ömrü ilkeleri, Linux VM'lerinin güvenliğini sağlamaya da yardımcı olur.
  - Azure sanal makinelerine girişi daha da güvenli hale getirmek için çok faktörlü kimlik doğrulamayı yapılandırabilirsiniz.
  - Azure Active Directory ile Linux VM'lerinde oturum açma özelliği, [Federasyon Hizmetlerini](../../active-directory/hybrid/how-to-connect-fed-whatis.md)kullanan müşteriler için de çalışır.

- **Sorunsuz işbirliği:** Role-Based Access Control (RBAC) ile, belirli bir VM'de normal bir kullanıcı olarak veya yönetici ayrıcalıklarıyla kimlerin oturum açabileceğini belirtebilirsiniz. Kullanıcılar ekibinize katıldıklarında veya ekibinizden ayrıldıklarında, uygun şekilde erişim sağlamak için VM'nin RBAC ilkesini güncelleştirebilirsiniz. Bu deneyim, gereksiz SSH ortak anahtarlarını kaldırmak için VM'leri temizlemek zorunda kalmaktan çok daha kolaydır. Çalışanlar kuruluşunuzdan ayrıldıklarında ve kullanıcı hesapları devre dışı bırakıldığında veya Azure AD'den kaldırıldığında, artık kaynaklarınıza erişemez.

## <a name="supported-azure-regions-and-linux-distributions"></a>Desteklenen Azure bölgeleri ve Linux dağıtımları

Aşağıdaki Linux dağıtımları şu anda bu özelliğin önizlemesi sırasında desteklenir:

| Dağıtım | Sürüm |
| --- | --- |
| CentOS | CentOS 6, CentOS 7 |
| Debian | Debian 9 |
| openSUSE | openSUSE Sıçrama42.3 |
| Red Hat Enterprise Linux | RHEL 6, RHEL 7 | 
| SUSE Linux Enterprise Server | SLES 12 |
| Ubuntu Server | Ubuntu 14.04 LTS, Ubuntu Server 16.04 ve Ubuntu Server 18.04 |


Aşağıdaki Azure bölgeleri şu anda bu özelliğin önizlemesi sırasında desteklenir:

- Tüm genel Azure bölgeleri

>[!IMPORTANT]
> Bu önizleme özelliğini kullanmak için yalnızca desteklenen bir Linux dağıtım dağıtımına ve desteklenen bir Azure bölgesinde dağıtın. Özellik Azure Devlet'te veya egemen bulutlarda desteklenmez.
>
> Bu uzantıyı Azure Kubernetes Hizmeti (AKS) kümelerinde kullanmak desteklenmez. Daha fazla bilgi [için AKS destek ilkelerine](../../aks/support-policies.md)bakın.


CLI'yi yerel olarak yüklemeyi ve kullanmayı seçerseniz, bu öğretici, Azure CLI sürümünü 2.0.31 veya daha sonra çalıştırdığınızı gerektirir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme]( /cli/azure/install-azure-cli).

## <a name="network-requirements"></a>Ağ gereksinimleri

Azure'daki Linux VM'leriniz için Azure AD kimlik doğrulamasını etkinleştirmek için, VM ağ yapılandırmanızın TCP bağlantı noktası 443 üzerinden aşağıdaki uç noktalara giden erişime izin verdiğinden emin olmanız gerekir:

* https:\//login.microsoftonline.com
* https:\//login.windows.net
* https:\//device.login.microsoftonline.com
* https:\//pas.windows.net
* https:\//management.azure.com
* https:\//packages.microsoft.com

> [!NOTE]
> Şu anda, Azure ağ güvenlik grupları Azure AD kimlik doğrulaması ile etkinleştirilmiş VM'ler için yapılandırılamaz.

## <a name="create-a-linux-virtual-machine"></a>Linux sanal makinesi oluşturma

[Az grubu oluşturmak](/cli/azure/group#az-group-create)ile bir kaynak grubu oluşturun, sonra az vm ile desteklenen bir dağıtım kullanarak ve desteklenen bir bölgede oluşturmak bir [VM](/cli/azure/vm#az-vm-create) oluşturun. Aşağıdaki örnek, *southcentralus* bölgesinde *myResourceGroup* adlı bir kaynak grubuna *Ubuntu 16.04 LTS* kullanan *myVM* adlı bir VM dağıtır. Aşağıdaki örneklerde, gerektiğinde kendi kaynak grubunuzu ve VM adlarınızı sağlayabilirsiniz.

```azurecli-interactive
az group create --name myResourceGroup --location southcentralus

az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys
```

VM’yi ve destekleyici kaynakları oluşturmak birkaç dakika sürer.

## <a name="install-the-azure-ad-login-vm-extension"></a>Azure AD oturum açma VM uzantısını yükleme

> [!NOTE]
> Bu uzantıyı önceden oluşturulmuş bir VM'ye dağıtmak makinenin en az 1 GB bellek tahsis edilmiş olduğundan emin olursanız, uzantı yüklenemez

Azure AD kimlik bilgilerine sahip bir Linux VM'de oturum açmak için Azure Active Directory giriş VM uzantısını yükleyin. VM uzantıları, Azure sanal makinelerinde dağıtım sonrası yapılandırma ve otomasyon görevleri sağlayan küçük uygulamalardır. *MyResourceGroup* kaynak grubuna *myVM* adlı VM'de *AADLoginForLinux* uzantısını yüklemek için [az vm uzantısını](/cli/azure/vm/extension#az-vm-extension-set) kullanın:

```azurecli-interactive
az vm extension set \
    --publisher Microsoft.Azure.ActiveDirectory.LinuxSSH \
    --name AADLoginForLinux \
    --resource-group myResourceGroup \
    --vm-name myVM
```

Uzantı VM'ye başarıyla yüklendikten sonra *Başarılı* Olanın *ProvisioningState'i* gösterilir. VM uzantısı yüklemek için çalışan bir VM aracısı gerekir. Daha fazla bilgi için [VM Agent Genel Bakış'a](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-windows)bakın.

## <a name="configure-role-assignments-for-the-vm"></a>VM için rol atamalarını yapılandırma

Azure Role Tabanlı Erişim Denetimi (RBAC) ilkesi, VM'de kimlerin oturum açabileceğini belirler. VM oturum açma yetkisi vermek için iki RBAC rolü kullanılır:

- **Sanal Makine Yöneticisi Girişi**: Bu role sahip kullanıcılar Windows Administrator veya Linux root kullanıcı ayrıcalıklarına sahip bir Azure sanal makinesinde oturum açabilir.
- **Sanal Makine Kullanıcı Girişi**: Bu role sahip kullanıcılar, düzenli kullanıcı ayrıcalıklarına sahip bir Azure sanal makinesinde oturum açabilir.

> [!NOTE]
> Bir kullanıcının SSH üzerinden VM'de oturum açmasına izin vermek için *Sanal Makine Yöneticisi Girişi* veya Sanal Makine Kullanıcı *Girişi* rolünü atamanız gerekir. VM için atanan *Sahip* veya *Katılımcı* rollerine sahip bir Azure kullanıcısının, SSH üzerinden VM'de oturum açma ayrıcalığı otomatik olarak gerekmez.

Aşağıdaki örnekte, sanal makine yöneticisi *giriş* rolünü geçerli Azure kullanıcınız için VM'ye atamak için az rol [ataması oluşturma](/cli/azure/role/assignment#az-role-assignment-create) kullanır. Etkin Azure hesabınızın kullanıcı adı [az hesap göster](/cli/azure/account#az-account-show)ile elde edilir ve *kapsam* [az vm show](/cli/azure/vm#az-vm-show)ile önceki adımda oluşturulan VM olarak ayarlanır. Kapsam bir kaynak grubu veya abonelik düzeyinde de atanabilir ve normal RBAC devralma izinleri geçerlidir. Daha fazla bilgi için [Bkz. Rol Tabanlı Erişim Denetimleri](../../role-based-access-control/overview.md)

```azurecli-interactive
username=$(az account show --query user.name --output tsv)
vm=$(az vm show --resource-group myResourceGroup --name myVM --query id -o tsv)

az role assignment create \
    --role "Virtual Machine Administrator Login" \
    --assignee $username \
    --scope $vm
```

> [!NOTE]
> AAD etki alanınız ve oturum açma kullanıcı adı alan adınız eşleşmiyorsa, kullanıcı hesabınızın nesne kimliğini yalnızca kullanıcı adı *değil,-atama* *-ile*birlikte belirtmeniz gerekir. [Az reklam kullanıcı listesi](/cli/azure/ad/user#az-ad-user-list)ile kullanıcı hesabınız için nesne kimliği elde edebilirsiniz.

Azure abonelik kaynaklarınıza erişimi yönetmek için RBAC'ı nasıl kullanacağınız hakkında daha fazla bilgi için Bkz. [Azure CLI, Azure](../../role-based-access-control/role-assignments-cli.md) [portalını](../../role-based-access-control/role-assignments-portal.md)veya [Azure PowerShell'i](../../role-based-access-control/role-assignments-powershell.md)kullanın.

Azure AD'yi, belirli bir kullanıcının Linux sanal makinesinde oturum açabilmek için çok faktörlü kimlik doğrulaması gerektirecek şekilde yapılandırabilirsiniz. Daha fazla bilgi için [buluttaki Azure Çok Faktörlü Kimlik Doğrulama'ya başlayın'](../../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md)a bakın.

## <a name="log-in-to-the-linux-virtual-machine"></a>Linux sanal makinesine giriş yapın

İlk olarak, [az vm show](/cli/azure/vm#az-vm-show)ile VM genel IP adresini görüntüleyin:

```azurecli-interactive
az vm show --resource-group myResourceGroup --name myVM -d --query publicIps -o tsv
```

Azure AD kimlik bilgilerinizi kullanarak Azure Linux sanal makinesinde oturum açın. Parametre, `-l` kendi Azure AD hesap adresinizi belirtmenizi sağlar. Örnek hesabı kendi hesabınızla değiştirin. Hesap adresleri tüm küçük harfe girilmelidir. Örnek IP adresini önceki komuttan VM'nizin genel IP adresiyle değiştirin.

```console
ssh -l azureuser@contoso.onmicrosoft.com 10.11.123.456
```

Azure AD'de tek seferlik kullanım koduyla oturum [https://microsoft.com/devicelogin](https://microsoft.com/devicelogin)açmanız istenir. Tek kullanımlık kodu aygıt giriş sayfasına kopyalayıp yapıştırın.

İstendiğinde, oturum açma sayfasındaAzure AD giriş kimlik bilgilerinizi girin. 

Aşağıdaki ileti, başarılı bir şekilde kimlik doğrulaması yaptığınızda web tarayıcısında gösterilir:`You have signed in to the Microsoft Azure Linux Virtual Machine Sign-In application on your device.`

Tarayıcı penceresini kapatın, SSH istemine dönün ve **Enter** tuşuna basın. 

Artık VM User veya *VM Administrator*gibi atanmış rol izinleriyle Azure Linux sanal makinesinde oturum *açtınız.* Kullanıcı hesabınıza Sanal *Makine Yöneticisi Giriş* rolü atanmışsa, kök ayrıcalıkları gerektiren komutları çalıştırmak için kullanabilirsiniz. `sudo`

## <a name="sudo-and-aad-login"></a>Sudo ve AAD girişi

Sudo'yu ilk çalıştırdığınızda, ikinci kez kimlik doğrulamanız istenir. Sudo çalıştırmak için yeniden kimlik doğrulamak zorunda kalmak istemiyorsanız, sudoers dosyanızı `/etc/sudoers.d/aad_admins` düzenleyebilir ve bu satırı değiştirebilirsiniz:

```bash
%aad_admins ALL=(ALL) ALL
```

Bu satırile:

```bash
%aad_admins ALL=(ALL) NOPASSWD:ALL
```


## <a name="troubleshoot-sign-in-issues"></a>Oturum açma sorunları

Azure AD kimlik bilgileriyle SSH'e çalıştığınızda karşılaşılan bazı sık karşılaşılan hatalar arasında RBAC rolü atanmamış ve oturum açma istemi yer alır. Bu sorunları düzeltmek için aşağıdaki bölümleri kullanın.

### <a name="access-denied-rbac-role-not-assigned"></a>Erişim reddedildi: RBAC rolü atanmadı

SSH isteminizde aşağıdaki hatayı görürseniz, kullanıcıya *Sanal Makine Yöneticisi Girişi* veya Sanal Makine Kullanıcı *Girişi* rolü veren VM için RBAC ilkelerini yapılandırdığınızı doğrulayın:

```output
login as: azureuser@contoso.onmicrosoft.com
Using keyboard-interactive authentication.
To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code FJX327AXD to authenticate. Press ENTER when ready.
Using keyboard-interactive authentication.
Access denied:  to sign-in you be assigned a role with action 'Microsoft.Compute/virtualMachines/login/action', for example 'Virtual Machine User Login'
Access denied
```

### <a name="continued-ssh-sign-in-prompts"></a>Devam eden SSH oturum açma istemleri

Bir web tarayıcısında kimlik doğrulama adımını başarıyla tamamlarsanız, hemen yeni bir kodla yeniden oturum açmanız istenebilir. Bu hata genellikle SSH isteminde belirttiğiniz oturum açma adı ile Azure AD'de oturum açtığınız hesap arasındaki uyuşmazlıklardan kaynaklanır. Bu sorunu düzeltmek için:

- SSH komut isteminde belirttiğiniz oturum açma adının doğru olduğunu doğrulayın. Oturum açma adındaki bir yazım hatası, SSH komut isteminde belirttiğiniz oturum açma adı ile Azure AD'de oturum açtığınız hesap arasında uyumsuzluklara neden olabilir. Örneğin, *\@azureuser contoso.onmicrosoft.com*yerine *azuresuer\@contoso.onmicrosoft.com* yazdınız.
- Birden çok kullanıcı hesabınız varsa, Azure AD'de oturum açken tarayıcı penceresinde farklı bir kullanıcı hesabı sağlamadığınızdan emin olun.
- Linux büyük/küçük harf duyarlı bir işletim sistemidir. ' ' veAzureuser@contoso.onmicrosoft.com' 'azureuser@contoso.onmicrosoft.comarasında bir fark vardır, bu da uyumsuzluk yaratabilir. Upn'i SSH isteminde doğru servis duyarlılığıyla belirttiğinden emin olun.

### <a name="other-limitations"></a>Diğer sınırlamalar

İç içe geçen gruplar veya rol atamaları aracılığıyla erişim haklarını devralan kullanıcılar şu anda desteklenmez. Kullanıcıya veya gruba doğrudan [gerekli rol atamaları](#configure-role-assignments-for-the-vm)atanmalıdır. Örneğin, yönetim gruplarının veya iç içe kullanılan grup rol atamalarının kullanımı, kullanıcının oturum kurmasını sağlamak için doğru izinleri vermez.

## <a name="preview-feedback"></a>Önizleme geri bildirimi

Bu önizleme özelliği hakkındaki görüşlerinizi paylaşın veya [Azure AD geri bildirim forumunda](https://feedback.azure.com/forums/169401-azure-active-directory?category_id=166032) kullanarak sorunları raporla

## <a name="next-steps"></a>Sonraki adımlar

Azure Etkin Dizin hakkında daha fazla bilgi için Azure [Etkin Dizin nedir](../../active-directory/fundamentals/active-directory-whatis.md)
