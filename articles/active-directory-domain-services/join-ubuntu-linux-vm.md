---
title: "Azure Active Directory Domain Services: Ubuntu VM 'sini yönetilen bir etki alanına katma | Microsoft Docs"
description: Ubuntu Linux bir sanal makineyi Azure AD Domain Services 'a ekleyin
services: active-directory-ds
documentationcenter: ''
author: iainfoulds
manager: daveba
editor: curtand
ms.assetid: 804438c4-51a1-497d-8ccc-5be775980203
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/20/2019
ms.author: iainfou
ms.openlocfilehash: 78afec75269876c309b2c324d8a5973fd5ebf9a8
ms.sourcegitcommit: 4b5dcdcd80860764e291f18de081a41753946ec9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2019
ms.locfileid: "68773031"
---
# <a name="join-an-ubuntu-virtual-machine-in-azure-to-a-managed-domain"></a>Azure 'daki bir Ubuntu sanal makinesini yönetilen bir etki alanına katma
Bu makalede Ubuntu Linux bir sanal makineyi Azure AD Domain Services yönetilen bir etki alanına nasıl katılabilmeniz gösterilmektedir.

[!INCLUDE [active-directory-ds-prerequisites.md](../../includes/active-directory-ds-prerequisites.md)]

## <a name="before-you-begin"></a>Başlamadan önce
Bu makalede listelenen görevleri gerçekleştirmek için şunlar gerekir:  
1. Geçerli bir **Azure aboneliği**.
2. Bir **Azure ad dizini** -şirket içi bir dizinle veya yalnızca bulut diziniyle eşitlenir.
3. Azure AD dizini için **Azure AD Domain Services** etkinleştirilmelidir. Bunu yapmadıysanız, [Başlarken kılavuzunda](create-instance.md)özetlenen tüm görevleri izleyin.
4. Yönetilen etki alanının IP adreslerini, sanal ağ için DNS sunucuları olarak yapılandırdığınızdan emin olun. Daha fazla bilgi için bkz [. Azure sanal ağı IÇIN DNS ayarlarını güncelleştirme](active-directory-ds-getting-started-dns.md)
5. [Parolaları Azure AD Domain Services yönetilen etki alanı ile senkronize](active-directory-ds-getting-started-password-sync.md)etmek için gereken adımları uygulayın.


## <a name="provision-an-ubuntu-linux-virtual-machine"></a>Ubuntu Linux sanal makinesi sağlama
Aşağıdaki yöntemlerden herhangi birini kullanarak Azure 'da Ubuntu Linux bir sanal makine sağlayın:
* [Azure portal](../virtual-machines/linux/quick-create-portal.md)
* [Azure CLI](../virtual-machines/linux/quick-create-cli.md)
* [Azure PowerShell](../virtual-machines/linux/quick-create-powershell.md)

> [!IMPORTANT]
> * Sanal makineyi **Azure AD Domain Services etkinleştirdiğiniz aynı sanal ağa**dağıtın.
> * Azure AD Domain Services etkinleştirdiğiniz olandan **farklı bir alt ağ** seçin.
>


## <a name="connect-remotely-to-the-ubuntu-linux-virtual-machine"></a>Ubuntu Linux sanal makinesine uzaktan bağlanma
Ubuntu sanal makinesi Azure 'da sağlandı. Sonraki görev, sanal makine sağlanırken oluşturulan yerel yönetici hesabı kullanılarak sanal makineye uzaktan bağlanmamalıdır.

[Linux çalıştıran bir sanal makinede oturum açma](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)makalesindeki yönergeleri izleyin.


## <a name="configure-the-hosts-file-on-the-linux-virtual-machine"></a>Linux sanal makinesinde Hosts dosyasını yapılandırma
SSH terminalinizde/etc/hosts dosyasını düzenleyin ve makinenizin IP adresini ve ana bilgisayar adını güncelleştirin.

```console
sudo vi /etc/hosts
```

Konaklar dosyasında aşağıdaki değeri girin:

```console
127.0.0.1 contoso-ubuntu.contoso100.com contoso-ubuntu
```

Burada, ' contoso100.com ', yönetilen etki alanının DNS etki alanı adıdır. ' contoso-Ubuntu ', yönetilen etki alanına katıldığınız Ubuntu sanal makinesinin ana bilgisayar adıdır.


## <a name="install-required-packages-on-the-linux-virtual-machine"></a>Linux sanal makinesine gereken paketleri yükler
Sonra, sanal makinede etki alanına katılması için gereken paketleri yükler. Aşağıdaki adımları uygulayın:

1.  SSH terminalinizde paket listelerini depolardan indirmek için aşağıdaki komutu yazın. Bu komut, paketlerin en yeni sürümleriyle ve bağımlılıklarıyla ilgili bilgi almak için paket listelerini güncelleştirir.

    ```console
    sudo apt-get update
    ```

2. Gerekli paketleri yüklemek için aşağıdaki komutu yazın.

    ```console
      sudo apt-get install krb5-user samba sssd sssd-tools libnss-sss libpam-sss ntp ntpdate realmd adcli
    ```

3. Kerberos yüklemesi sırasında, pembe bir ekran görürsünüz. ' Krb5-User ' paketinin yüklenmesi bölge adı (tüm büyük harfle) için istemde bulunur. Yükleme,/etc/kronb5,confiçindeki [Realm] ve [domain_realm] bölümlerini yazar.

    > [!TIP]
    > Yönetilen etki alanının adı contoso100.com ise, bölge olarak CONTOSO100.COM girin. Bölge adının büyük harfle belirtilmiş olması gerektiğini unutmayın.


## <a name="configure-the-ntp-network-time-protocol-settings-on-the-linux-virtual-machine"></a>Linux sanal makinesinde NTP (ağ zaman Protokolü) ayarlarını yapılandırma
Ubuntu VM 'nizin tarih ve saati, yönetilen etki alanı ile eşitlenmelidir. Yönetilen etki alanının NTP ana bilgisayar adını/etc/NTP.conf dosyasına ekleyin.

```console
sudo vi /etc/ntp.conf
```

NTP. conf dosyasında aşağıdaki değeri girin ve dosyayı kaydedin:

```console
server contoso100.com
```

Burada, ' contoso100.com ', yönetilen etki alanının DNS etki alanı adıdır.

Şimdi, Ubuntu VM 'sinin tarih ve saatini NTP sunucusu ile eşitleyin ve ardından NTP hizmetini başlatın:

```console
sudo systemctl stop ntp
sudo ntpdate contoso100.com
sudo systemctl start ntp
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
    >

    ```console
    kinit bob@CONTOSO100.COM
    ```

3. Makineyi etki alanına ekleyin. SSH terminalinizde aşağıdaki komutu yazın:

    > [!TIP]
    > Önceki adımda belirttiğiniz kullanıcı hesabını kullanın (' kinit ').
    >
    > VM 'niz etki alanına katılamediğinde, VM 'nin ağ güvenlik grubunun TCP + UDP bağlantı noktası 464 üzerinde giden Kerberos trafiğinin Azure AD DS yönetilen etki alanınıza yönelik sanal ağ alt ağına izin verdiğinden emin olun.

    ```console
    sudo realm join --verbose CONTOSO100.COM -U 'bob@CONTOSO100.COM' --install=/
    ```

Makine yönetilen etki alanına başarıyla katıldığında bir ileti ("makinenin bölgeye başarıyla kaydedildi") almanız gerekir.


## <a name="update-the-sssd-configuration-and-restart-the-service"></a>SSSD yapılandırmasını güncelleştirin ve hizmeti yeniden başlatın
1. SSH terminalinizde aşağıdaki komutu yazın. Sssd. conf dosyasını açın ve aşağıdaki değişikliği yapın
    
    ```console
    sudo vi /etc/sssd/sssd.conf
    ```

2. Line **use_fully_qualified_names = true** olarak açıklama ekleyin ve dosyayı kaydedin.
    
    ```console
    # use_fully_qualified_names = True
    ```

3. SSSD hizmetini yeniden başlatın.
    
    ```console
    sudo service sssd restart
    ```


## <a name="configure-automatic-home-directory-creation"></a>Otomatik Ana Dizin oluşturmayı yapılandırma
Kullanıcıların oturum açmasını tamamladıktan sonra ana dizinin otomatik olarak oluşturulmasını etkinleştirmek için, PuTTY terminalinize aşağıdaki komutları yazın:

```console
sudo vi /etc/pam.d/common-session
```

Aşağıdaki satırı bu dosyaya satır ' oturumu isteğe bağlı pam_sss. so ' altına ekleyin ve kaydedin:

```console
session required pam_mkhomedir.so skel=/etc/skel/ umask=0077
```


## <a name="verify-domain-join"></a>Etki alanına katılımı doğrula
Makinenin yönetilen etki alanına başarıyla katılıp katılmadığını doğrulayın. Farklı bir SSH bağlantısı kullanarak etki alanına katılmış Ubuntu VM 'sine bağlanın. Bir etki alanı kullanıcı hesabı kullanın ve ardından Kullanıcı hesabının doğru çözümlenmiş olup olmadığını kontrol edin.

1. SSH terminalinizde, SSH kullanarak etki alanına katılmış Ubuntu sanal makinesine bağlanmak için aşağıdaki komutu yazın. Yönetilen etki alanına ait bir etki alanı hesabı kullanın (örneğin, bu durumda 'bob@CONTOSO100.COM').)
    
    ```console
    ssh -l bob@CONTOSO100.COM contoso-ubuntu.contoso100.com
    ```

2. SSH terminalinizde, giriş dizininin doğru şekilde başlatılmış olup olmadığını görmek için aşağıdaki komutu yazın.
    
    ```console
    pwd
    ```

3. SSH terminalinizde, grup üyeliklerinin doğru çözümlenip çözümlenmediğini görmek için aşağıdaki komutu yazın.
    
    ```console
    id
    ```


## <a name="grant-the-aad-dc-administrators-group-sudo-privileges"></a>' AAD DC yöneticileri ' Grup sudo ayrıcalıklarına izin verme
Ubuntu VM 'de ' AAD DC Administrators ' grubu yönetici ayrıcalıklarının üyelerine izin verebilirsiniz. Sudo dosyası/etc/sudoerskonumunda bulunur. Sudoers 'a eklenen AD gruplarının üyeleri sudo yapabilir.

1. SSH terminalinizde, süper kullanıcı ayrıcalıklarıyla oturum açtığınızdan emin olun. VM oluştururken belirttiğiniz yerel yönetici hesabını kullanabilirsiniz. Aşağıdaki komutu yürütün:
    
    ```console
    sudo vi /etc/sudoers
    ```

2. Aşağıdaki girişi/etc/sudoers dosyasına ekleyin ve kaydedin:
    
    ```console
    # Add 'AAD DC Administrators' group members as admins.
    %AAD\ DC\ Administrators ALL=(ALL) NOPASSWD:ALL
    ```

3. Artık ' AAD DC Administrators ' grubunun bir üyesi olarak oturum açabilir ve VM üzerinde yönetici ayrıcalıklarına sahip olmanız gerekir.


## <a name="troubleshooting-domain-join"></a>Etki alanına katılması sorunlarını giderme
[Etki alanına ekleme sorunlarını giderme](join-windows-vm.md#troubleshoot-joining-a-domain) makalesine bakın.


## <a name="related-content"></a>İlgili İçerik
* [Azure AD Domain Services-Başlarken Kılavuzu](create-instance.md)
* [Windows Server sanal makinesini Azure AD Domain Services yönetilen bir etki alanına katma](active-directory-ds-admin-guide-join-windows-vm.md)
* [Linux çalıştıran bir sanal makinede oturum açma](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
