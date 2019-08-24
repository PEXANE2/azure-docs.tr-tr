---
title: "Azure Active Directory Domain Services: Bir CoreOS Linux VM 'sine ekleme | Microsoft Docs"
description: Bir CoreOS Linux sanal makinesine Azure AD Domain Services ekleyin
services: active-directory-ds
documentationcenter: ''
author: iainfoulds
manager: daveba
editor: curtand
ms.assetid: 5db65f30-bf69-4ea3-9ea5-add1db83fdb8
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/20/2019
ms.author: iainfou
ms.openlocfilehash: dc76d9a0d492d8ef0e37c0c34173216ff4c75164
ms.sourcegitcommit: 007ee4ac1c64810632754d9db2277663a138f9c4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/23/2019
ms.locfileid: "69990559"
---
# <a name="join-a-coreos-linux-virtual-machine-to-a-managed-domain"></a>Bir CoreOS Linux sanal makinesini yönetilen bir etki alanına katma
Bu makalede, Azure 'da bir CoreOS Linux sanal makinesini Azure AD Domain Services yönetilen bir etki alanına nasıl katılabilmeniz gösterilmektedir.

[!INCLUDE [active-directory-ds-prerequisites.md](../../includes/active-directory-ds-prerequisites.md)]

## <a name="before-you-begin"></a>Başlamadan önce
Bu makalede listelenen görevleri gerçekleştirmek için şunlar gerekir:
1. Geçerli bir **Azure aboneliği**.
2. Bir **Azure ad dizini** -şirket içi bir dizinle veya yalnızca bulut diziniyle eşitlenir.
3. Azure AD dizini için **Azure AD Domain Services** etkinleştirilmelidir. Bunu yapmadıysanız, [Başlarken kılavuzunda](tutorial-create-instance.md)özetlenen tüm görevleri izleyin.
4. Yönetilen etki alanının IP adreslerini, sanal ağ için DNS sunucuları olarak yapılandırdığınızdan emin olun. Daha fazla bilgi için bkz [. Azure sanal ağı IÇIN DNS ayarlarını güncelleştirme](tutorial-create-instance.md#update-dns-settings-for-the-azure-virtual-network)
5. [Parolaları Azure AD Domain Services yönetilen etki alanı ile senkronize](tutorial-create-instance.md#enable-user-accounts-for-azure-ad-ds)etmek için gereken adımları uygulayın.


## <a name="provision-a-coreos-linux-virtual-machine"></a>CoreOS Linux sanal makinesi sağlama
Aşağıdaki yöntemlerden birini kullanarak Azure 'da bir CoreOS sanal makinesi sağlayın:
* [Azure portal](../virtual-machines/linux/quick-create-portal.md)
* [Azure CLI](../virtual-machines/linux/quick-create-cli.md)
* [Azure PowerShell](../virtual-machines/linux/quick-create-powershell.md)

Bu makalede Azure 'da **CoreOS Linux (Stable)** sanal makine görüntüsü kullanılmaktadır.

> [!IMPORTANT]
> * Sanal makineyi **Azure AD Domain Services etkinleştirdiğiniz aynı sanal ağa**dağıtın.
> * Azure AD Domain Services etkinleştirdiğiniz olandan **farklı bir alt ağ** seçin.
>


## <a name="connect-remotely-to-the-newly-provisioned-linux-virtual-machine"></a>Yeni sağlanan Linux sanal makinesine uzaktan Bağlan
CoreOS sanal makinesi Azure 'da sağlandı. Sonraki görev, sanal makine sağlanırken oluşturulan yerel yönetici hesabı kullanılarak sanal makineye uzaktan bağlanmamalıdır.

[Linux çalıştıran bir sanal makinede oturum açma](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)makalesindeki yönergeleri izleyin.


## <a name="configure-the-hosts-file-on-the-linux-virtual-machine"></a>Linux sanal makinesinde Hosts dosyasını yapılandırma
SSH terminalinizde/etc/hosts dosyasını düzenleyin ve makinenizin IP adresini ve ana bilgisayar adını güncelleştirin.

```console
sudo vi /etc/hosts
```

Konaklar dosyasında aşağıdaki değeri girin:

```console
127.0.0.1 contoso-coreos.contoso.com contoso-coreos
```

Burada, ' contoso.com ', yönetilen etki alanının DNS etki alanı adıdır. ' contoso-CoreOS ', yönetilen etki alanına katıldığınız CoreOS sanal makinesinin ana bilgisayar adıdır.


## <a name="configure-the-sssd-service-on-the-linux-virtual-machine"></a>Linux sanal makinesinde SSSD hizmetini yapılandırma
Sonra, SSSD yapılandırma dosyanızı ('/etc/sssd/sssd.exe ') Şu örnekle eşleşecek şekilde güncelleştirin:

```console
[sssd]
config_file_version = 2
services = nss, pam
domains = CONTOSO.COM

[domain/CONTOSO.COM]
id_provider = ad
auth_provider = ad
chpass_provider = ad

ldap_uri = ldap://contoso.com
ldap_search_base = dc=contoso,dc=com
ldap_schema = rfc2307bis
ldap_sasl_mech = GSSAPI
ldap_user_object_class = user
ldap_group_object_class = group
ldap_user_home_directory = unixHomeDirectory
ldap_user_principal = userPrincipalName
ldap_account_expire_policy = ad
ldap_force_upper_case_realm = true
fallback_homedir = /home/%d/%u

krb5_server = contoso.com
krb5_realm = CONTOSO.COM
```

' CONTOSO ' öğesini değiştirin. COM ', yönetilen etki alanınız için DNS etki alanı adı ile. Etki alanı adını conf dosyasında büyük harflerle belirttiğinizden emin olun.


## <a name="join-the-linux-virtual-machine-to-the-managed-domain"></a>Linux sanal makinesini yönetilen etki alanına ekleyin
Artık gerekli paketler Linux sanal makinesinde yüklü olduğuna göre, bir sonraki görev sanal makineyi yönetilen etki alanına katabilir.

```console
sudo adcli join -D CONTOSO.COM -U bob@CONTOSO.COM -K /etc/krb5.keytab -H contoso-coreos.contoso.com -N coreos
```


> [!NOTE]
> **Sorunu** *Adclı* , yönetilen etki alanınızı bulamazsa:
>   * Etki alanının sanal makineden ulaşılabilir olduğundan emin olun (PING komutunu deneyin).
>   * Sanal makinenin, yönetilen etki alanının kullanılabildiği aynı sanal ağa gerçekten dağıtılıp dağıtılmadığını denetleyin.
>   * Sanal ağın DNS sunucusu ayarlarını, yönetilen etki alanının etki alanı denetleyicilerini işaret etmek üzere güncelleştirmiş olup olmadığını denetleyin.

SSSD hizmetini başlatın. SSH terminalinizde aşağıdaki komutu yazın:
  
```console
sudo systemctl start sssd.service
```


## <a name="verify-domain-join"></a>Etki alanına katılımı doğrula
Makinenin yönetilen etki alanına başarıyla katılıp katılmadığını doğrulayın. Farklı bir SSH bağlantısı kullanarak etki alanına katılmış CoreOS VM 'ye bağlanın. Bir etki alanı kullanıcı hesabı kullanın ve ardından Kullanıcı hesabının doğru çözümlenmiş olup olmadığını kontrol edin.

1. SSH terminalinizde, SSH kullanarak etki alanına katılmış CoreOS sanal makinesine bağlanmak için aşağıdaki komutu yazın. Yönetilen etki alanına ait bir etki alanı hesabı kullanın (örneğin, bu durumda 'bob@CONTOSO.COM').)
    
    ```console
    ssh -l bob@CONTOSO.COM contoso-coreos.contoso.com
    ```

2. SSH terminalinizde, giriş dizininin doğru şekilde başlatılmış olup olmadığını görmek için aşağıdaki komutu yazın.
    
    ```console
    pwd
    ```

3. SSH terminalinizde, grup üyeliklerinin doğru çözümlenip çözümlenmediğini görmek için aşağıdaki komutu yazın.
   
    ```console
    id
    ```


## <a name="troubleshooting-domain-join"></a>Etki alanına katılması sorunlarını giderme
[Etki alanına ekleme sorunlarını giderme](join-windows-vm.md#troubleshoot-domain-join-issues) makalesine bakın.

## <a name="related-content"></a>İlgili İçerik
* [Azure AD Domain Services-Başlarken Kılavuzu](tutorial-create-instance.md)
* [Windows Server sanal makinesini Azure AD Domain Services yönetilen bir etki alanına katma](active-directory-ds-admin-guide-join-windows-vm.md)
* [Linux çalıştıran bir sanal makinede oturum açma](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
