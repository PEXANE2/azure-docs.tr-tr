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
ms.openlocfilehash: 78a6c5262cd6668712beac1e041fa4f25c05a724
ms.sourcegitcommit: b2db98f55785ff920140f117bfc01f1177c7f7e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/16/2019
ms.locfileid: "68234078"
---
# <a name="join-a-coreos-linux-virtual-machine-to-a-managed-domain"></a>Bir CoreOS Linux sanal makinesini yönetilen bir etki alanına katma
Bu makalede, Azure 'da bir CoreOS Linux sanal makinesini Azure AD Domain Services yönetilen bir etki alanına nasıl katılabilmeniz gösterilmektedir.

[!INCLUDE [active-directory-ds-prerequisites.md](../../includes/active-directory-ds-prerequisites.md)]

## <a name="before-you-begin"></a>Başlamadan önce
Bu makalede listelenen görevleri gerçekleştirmek için şunlar gerekir:
1. Geçerli bir **Azure aboneliği**.
2. Bir **Azure ad dizini** -şirket içi bir dizinle veya yalnızca bulut diziniyle eşitlenir.
3. Azure AD dizini için **Azure AD Domain Services** etkinleştirilmelidir. Bunu yapmadıysanız, [Başlarken kılavuzunda](create-instance.md)özetlenen tüm görevleri izleyin.
4. Yönetilen etki alanının IP adreslerini, sanal ağ için DNS sunucuları olarak yapılandırdığınızdan emin olun. Daha fazla bilgi için bkz [. Azure sanal ağı IÇIN DNS ayarlarını güncelleştirme](active-directory-ds-getting-started-dns.md)
5. [Parolaları Azure AD Domain Services yönetilen etki alanı ile senkronize](active-directory-ds-getting-started-password-sync.md)etmek için gereken adımları uygulayın.


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
127.0.0.1 contoso-coreos.contoso100.com contoso-coreos
```

Burada, ' contoso100.com ', yönetilen etki alanının DNS etki alanı adıdır. ' contoso-CoreOS ', yönetilen etki alanına katıldığınız CoreOS sanal makinesinin ana bilgisayar adıdır.


## <a name="configure-the-sssd-service-on-the-linux-virtual-machine"></a>Linux sanal makinesinde SSSD hizmetini yapılandırma
Sonra, SSSD yapılandırma dosyanızı ('/etc/sssd/sssd.exe ') Şu örnekle eşleşecek şekilde güncelleştirin:

```console
[sssd]
config_file_version = 2
services = nss, pam
domains = CONTOSO100.COM

[domain/CONTOSO100.COM]
id_provider = ad
auth_provider = ad
chpass_provider = ad

ldap_uri = ldap://contoso100.com
ldap_search_base = dc=contoso100,dc=com
ldap_schema = rfc2307bis
ldap_sasl_mech = GSSAPI
ldap_user_object_class = user
ldap_group_object_class = group
ldap_user_home_directory = unixHomeDirectory
ldap_user_principal = userPrincipalName
ldap_account_expire_policy = ad
ldap_force_upper_case_realm = true
fallback_homedir = /home/%d/%u

krb5_server = contoso100.com
krb5_realm = CONTOSO100.COM
```

' CONTOSO100 ' öğesini değiştirin. COM ', yönetilen etki alanınız için DNS etki alanı adı ile. Etki alanı adını conf dosyasında büyük harflerle belirttiğinizden emin olun.


## <a name="join-the-linux-virtual-machine-to-the-managed-domain"></a>Linux sanal makinesini yönetilen etki alanına ekleyin
Artık gerekli paketler Linux sanal makinesinde yüklü olduğuna göre, bir sonraki görev sanal makineyi yönetilen etki alanına katabilir.

```console
sudo adcli join -D CONTOSO100.COM -U bob@CONTOSO100.COM -K /etc/krb5.keytab -H contoso-coreos.contoso100.com -N coreos
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

1. SSH terminalinizde, SSH kullanarak etki alanına katılmış CoreOS sanal makinesine bağlanmak için aşağıdaki komutu yazın. Yönetilen etki alanına ait bir etki alanı hesabı kullanın (örneğin, bu durumda 'bob@CONTOSO100.COM').)
    
    ```console
    ssh -l bob@CONTOSO100.COM contoso-coreos.contoso100.com
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
[Etki alanına ekleme sorunlarını giderme](join-windows-vm.md#troubleshoot-joining-a-domain) makalesine bakın.

## <a name="related-content"></a>İlgili İçerik
* [Azure AD Domain Services-Başlarken Kılavuzu](create-instance.md)
* [Windows Server sanal makinesini Azure AD Domain Services yönetilen bir etki alanına katma](active-directory-ds-admin-guide-join-windows-vm.md)
* [Linux çalıştıran bir sanal makinede oturum açma](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
