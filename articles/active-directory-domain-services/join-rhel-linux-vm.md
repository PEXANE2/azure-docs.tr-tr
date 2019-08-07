---
title: "Azure Active Directory Domain Services: Bir RHEL VM 'yi yönetilen bir etki alanına katma | Microsoft Docs"
description: Red Hat Enterprise Linux bir sanal makineyi Azure AD Domain Services 'a ekleyin
services: active-directory-ds
documentationcenter: ''
author: iainfoulds
manager: daveba
editor: curtand
ms.assetid: d76ae997-2279-46dd-bfc5-c0ee29718096
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/20/2019
ms.author: iainfou
ms.openlocfilehash: d0acbd02103ebd8dd3819579c85b4ddac22dba78
ms.sourcegitcommit: 4b5dcdcd80860764e291f18de081a41753946ec9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2019
ms.locfileid: "68773108"
---
# <a name="join-a-red-hat-enterprise-linux-7-virtual-machine-to-a-managed-domain"></a>Red Hat Enterprise Linux 7 sanal makinesini yönetilen bir etki alanına ekleme
Bu makalede, bir Red Hat Enterprise Linux (RHEL) 7 sanal makinesini Azure AD Domain Services yönetilen bir etki alanına nasıl katılabilmeniz gösterilmektedir.

[!INCLUDE [active-directory-ds-prerequisites.md](../../includes/active-directory-ds-prerequisites.md)]

## <a name="before-you-begin"></a>Başlamadan önce
Bu makalede listelenen görevleri gerçekleştirmek için şunlar gerekir:  
1. Geçerli bir **Azure aboneliği**.
2. Bir **Azure ad dizini** -şirket içi bir dizinle veya yalnızca bulut diziniyle eşitlenir.
3. Azure AD dizini için **Azure AD Domain Services** etkinleştirilmelidir. Bunu yapmadıysanız, [Başlarken kılavuzunda](create-instance.md)özetlenen tüm görevleri izleyin.
4. Yönetilen etki alanının IP adreslerini, sanal ağ için DNS sunucuları olarak yapılandırdığınızdan emin olun. Daha fazla bilgi için bkz [. Azure sanal ağı IÇIN DNS ayarlarını güncelleştirme](active-directory-ds-getting-started-dns.md)
5. [Parolaları Azure AD Domain Services yönetilen etki alanı ile senkronize](active-directory-ds-getting-started-password-sync.md)etmek için gereken adımları uygulayın.


## <a name="provision-a-red-hat-enterprise-linux-virtual-machine"></a>Red Hat Enterprise Linux sanal makinesi sağlama
Aşağıdaki yöntemlerden birini kullanarak Azure 'da bir RHEL 7 sanal makinesi sağlayın:
* [Azure portal](../virtual-machines/linux/quick-create-portal.md)
* [Azure CLI](../virtual-machines/linux/quick-create-cli.md)
* [Azure PowerShell](../virtual-machines/linux/quick-create-powershell.md)

> [!IMPORTANT]
> * Sanal makineyi **Azure AD Domain Services etkinleştirdiğiniz aynı sanal ağa**dağıtın.
> * Azure AD Domain Services etkinleştirdiğiniz olandan **farklı bir alt ağ** seçin.
>


## <a name="connect-remotely-to-the-newly-provisioned-linux-virtual-machine"></a>Yeni sağlanan Linux sanal makinesine uzaktan Bağlan
RHEL 7,2 sanal makinesi Azure 'da sağlandı. Sonraki görev, sanal makine sağlanırken oluşturulan yerel yönetici hesabı kullanılarak sanal makineye uzaktan bağlanmamalıdır.

[Linux çalıştıran bir sanal makinede oturum açma](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)makalesindeki yönergeleri izleyin.


## <a name="configure-the-hosts-file-on-the-linux-virtual-machine"></a>Linux sanal makinesinde Hosts dosyasını yapılandırma
SSH terminalinizde/etc/hosts dosyasını düzenleyin ve makinenizin IP adresini ve ana bilgisayar adını güncelleştirin.

```console
sudo vi /etc/hosts
```

Konaklar dosyasında aşağıdaki değeri girin:

```console
127.0.0.1 contoso-rhel.contoso100.com contoso-rhel
```

Burada, ' contoso100.com ', yönetilen etki alanının DNS etki alanı adıdır. ' contoso-RHEL ', yönetilen etki alanına katıldığınız RHEL sanal makinesinin ana bilgisayar adıdır.


## <a name="install-required-packages-on-the-linux-virtual-machine"></a>Linux sanal makinesine gereken paketleri yükler
Sonra, sanal makinede etki alanına katılması için gereken paketleri yükler. SSH terminalinizde, gerekli paketleri yüklemek için aşağıdaki komutu yazın:

```console
sudo yum install realmd sssd krb5-workstation krb5-libs samba-common-tools
```


## <a name="join-the-linux-virtual-machine-to-the-managed-domain"></a>Linux sanal makinesini yönetilen etki alanına ekleyin
Artık gerekli paketler Linux sanal makinesinde yüklü olduğuna göre, bir sonraki görev sanal makineyi yönetilen etki alanına katabilir.

1. AAD etki alanı Hizmetleri tarafından yönetilen etki alanını bulun. SSH terminalinizde aşağıdaki komutu yazın:

    ```console
    sudo realm discover CONTOSO100.COM
    ```

   > [!NOTE]
   > **Sorunu** *Bölge bulma* , yönetilen etki alanınızı bulamazsa:
   >   * Etki alanının sanal makineden ulaşılabilir olduğundan emin olun (PING komutunu deneyin).
   >   * Sanal makinenin, yönetilen etki alanının kullanılabildiği aynı sanal ağa gerçekten dağıtılıp dağıtılmadığını denetleyin.
   >   * Sanal ağın DNS sunucusu ayarlarını, yönetilen etki alanının etki alanı denetleyicilerini işaret etmek üzere güncelleştirmiş olup olmadığını denetleyin.

2. Kerberos başlatın. SSH terminalinizde aşağıdaki komutu yazın:

    > [!TIP]
    > * ' AAD DC Administrators ' grubuna ait olan bir Kullanıcı belirttiğinizden emin olun.
    > * Etki alanı adını büyük harfle belirtin, aksi kinit başarısız olur.

    ```console
    kinit bob@CONTOSO100.COM
    ```

3. Makineyi etki alanına ekleyin. SSH terminalinizde aşağıdaki komutu yazın:

    > [!TIP]
    > Önceki adımda belirttiğiniz kullanıcı hesabını kullanın (' kinit ').
    >
    > VM 'niz etki alanına katılamediğinde, VM 'nin ağ güvenlik grubunun TCP + UDP bağlantı noktası 464 üzerinde giden Kerberos trafiğinin Azure AD DS yönetilen etki alanınıza yönelik sanal ağ alt ağına izin verdiğinden emin olun.

    ```console
    sudo realm join --verbose CONTOSO100.COM -U 'bob@CONTOSO100.COM'
    ```

Makine yönetilen etki alanına başarıyla katıldığında bir ileti ("makinenin bölgeye başarıyla kaydedildi") almanız gerekir.


## <a name="verify-domain-join"></a>Etki alanına katılımı doğrula
Makinenin yönetilen etki alanına başarıyla katılıp katılmadığını doğrulayın. Farklı bir SSH bağlantısı kullanarak etki alanına katılmış RHEL VM 'ye bağlanın. Bir etki alanı kullanıcı hesabı kullanın ve ardından Kullanıcı hesabının doğru çözümlenmiş olup olmadığını kontrol edin.

1. SSH terminalinizde, SSH kullanarak etki alanına katılmış RHEL sanal makinesine bağlanmak için aşağıdaki komutu yazın. Yönetilen etki alanına ait bir etki alanı hesabı kullanın (örneğin, bu durumda 'bob@CONTOSO100.COM').)
    
    ```console
    ssh -l bob@CONTOSO100.COM contoso-rhel.contoso100.com
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
* [Kerberos yükleme](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/html/Managing_Smart_Cards/installing-kerberos.html)
* [Red Hat Enterprise Linux 7-Windows tümleştirme Kılavuzu](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Windows_Integration_Guide/index.html)
