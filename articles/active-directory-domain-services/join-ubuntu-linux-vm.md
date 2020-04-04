---
title: Azure AD Etki Alanı Hizmetleri için Bir Ubuntu VM'ye katılın | Microsoft Dokümanlar
description: Bir Ubuntu Linux sanal makinesini Azure AD Etki Alanı Hizmetleri yönetilen bir etki alanına nasıl yapılandırıp katılacağınızı öğrenin.
services: active-directory-ds
author: iainfoulds
manager: daveba
ms.assetid: 804438c4-51a1-497d-8ccc-5be775980203
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: how-to
ms.date: 01/22/2020
ms.author: iainfou
ms.openlocfilehash: 95373ab8ff78c5bcb856e6d7e6d67d8525cd3f7e
ms.sourcegitcommit: 62c5557ff3b2247dafc8bb482256fef58ab41c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80655127"
---
# <a name="join-an-ubuntu-linux-virtual-machine-to-an-azure-ad-domain-services-managed-domain"></a>Bir Ubuntu Linux sanal makinesine Azure AD Etki Alanı Hizmetleri yönetilen etki alanına katılın

Kullanıcıların azure'da tek bir kimlik bilgileri kümesi kullanarak sanal makinelerde (VM) oturum açmalarına izin vermek için, Sanal Makinelere Azure Active Directory Domain Services (AD DS) yönetilen etki alanına katılabilirsiniz. Bir Azure AD DS yönetilen etki alanına Bir VM'ye katıldığınızda, etki alanından kullanıcı hesapları ve kimlik bilgileri sunucuları oturum açma ve yönetmek için kullanılabilir. Azure AD DS yönetilen etki alanından grup üyelikleri, VM'deki dosyalara veya hizmetlere erişimi denetlemenize izin vermek için de uygulanır.

Bu makalede, Bir Ubuntu Linux VM'ye Azure AD DS yönetilen etki alanına nasıl katılacağınız gösterilmektedir.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdaki kaynaklara ve ayrıcalıklara ihtiyacınız vardır:

* Etkin bir Azure aboneliği.
    * Azure aboneliğiniz yoksa [bir hesap oluşturun.](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
* Aboneliğinizle ilişkili bir Azure Etkin Dizin kiracısı, şirket içi bir dizini veya yalnızca bulut dizininizle eşitlenir.
    * Gerekirse, [bir Azure Etkin Dizin kiracısı oluşturun][create-azure-ad-tenant] veya [bir Azure aboneliğini hesabınızla ilişkilendirin.][associate-azure-ad-tenant]
* Azure Etkin Dizin Etki Alanı Hizmetleri, Azure AD kiracınızda etkin leştirilmiş ve yapılandırılan bir etki alanı yönetildi.
    * Gerekirse, ilk öğretici [bir Azure Active Directory Etki Alanı Hizmetleri örneği oluşturur ve yapılandırır.][create-azure-ad-ds-instance]
* Azure AD DS yönetilen etki alanının bir parçası olan bir kullanıcı hesabı.

## <a name="create-and-connect-to-an-ubuntu-linux-vm"></a>Bir Ubuntu Linux VM oluşturun ve bağlanın

Azure'da mevcut bir Ubuntu Linux VM'niz varsa, SSH kullanarak ona bağlanın ve [VM'yi yapılandırmaya başlamak](#configure-the-hosts-file)için bir sonraki adıma geçin.

Bir Ubuntu Linux VM oluşturmanız gerekiyorsa veya bu makalede kullanılmak üzere bir test VM oluşturmak istiyorsanız, aşağıdaki yöntemlerden birini kullanabilirsiniz:

* [Azure portal](../virtual-machines/linux/quick-create-portal.md)
* [Azure CLI](../virtual-machines/linux/quick-create-cli.md)
* [Azure PowerShell](../virtual-machines/linux/quick-create-powershell.md)

VM'yi oluşturduğunuzda, VM'nin Azure AD DS yönetilen etki alanıyla iletişim kurabilmesini sağlamak için sanal ağ ayarlarına dikkat edin:

* VM'yi Azure AD Etki Alanı Hizmetlerini etkinleştirdiğiniz aynı veya eşlenmiş sanal ağa dağıtın.
* VM'yi Azure AD Etki Alanı Hizmetleri örneğinden farklı bir alt ağa dağıtın.

VM dağıtıldıktan sonra, SSH kullanarak VM'ye bağlanmak için gereken adımları izleyin.

## <a name="configure-the-hosts-file"></a>Ana bilgisayar dosyasını yapılandırma

VM ana bilgisayar adının yönetilen etki alanı için doğru şekilde yapılandırıldığından emin olmak *için/etc/hosts* dosyasını düzenleyin ve ana bilgisayar adını ayarlayın:

```console
sudo vi /etc/hosts
```

Ana *bilgisayarlar* dosyasında, *localhost* adresini güncelleştirin. Aşağıdaki örnekte:

* *aaddscontoso.com,* Azure AD DS yönetilen etki alanınızın DNS etki alanı adıdır.
* *ubuntu,* yönetilen etki alanına katıldığınız Ubuntu VM'nizin ana adıdır.

Bu adları kendi değerlerinizle güncelleştirin:

```console
127.0.0.1 ubuntu.aaddscontoso.com ubuntu
```

Bittiğinde, düzenleyicinin komutunu `:wq` kullanarak ana *bilgisayar* dosyasını kaydedin ve çıkın.

## <a name="install-required-packages"></a>Gerekli paketleri yükleme

VM'nin Azure AD DS yönetilen etki alanına katılmak için bazı ek paketlere ihtiyacı vardır. Bu paketleri yüklemek ve yapılandırmak için etki alanı birleştirme araçlarını`apt-get`

Kerberos kurulumu sırasında, *krb5 kullanıcı* paketi TÜM BÜYÜK HARF'te bölge adı için istenir. Örneğin, Azure AD DS yönetilen etki alanınızın adı *aaddscontoso.com*ise, *AADDSCONTOSO.COM* bölge olarak girin. Yükleme `[realm]` */etc/krb5.conf* yapılandırma dosyasındaki bölümleri ve `[domain_realm]` bölümleri yazar. Alemi bir TÜM BÜYÜK HARF olarak belirttiğinden emin olun:

```console
sudo apt-get update
sudo apt-get install krb5-user samba sssd sssd-tools libnss-sss libpam-sss ntp ntpdate realmd adcli
```

## <a name="configure-network-time-protocol-ntp"></a>Ağ Süresi Protokolünü Yapılandırma (NTP)

Etki alanı iletişiminin doğru çalışması için, Ubuntu VM'nizin tarih ve saatinin Azure AD DS yönetilen etki alanıyla eşitlemesi gerekir. Azure AD DS yönetilen etki alanınızın NTP ana bilgisayar adını */etc/ntp.conf* dosyasına ekleyin.

1. *Ntp.conf* dosyasını bir editörle açın:

    ```console
    sudo vi /etc/ntp.conf
    ```

1. *ntp.conf* dosyasında, Azure AD DS yönetilen etki alanınızın DNS adını eklemek için bir satır oluşturun. Aşağıdaki örnekte, *aaddscontoso.com* için bir giriş eklenir. Kendi DNS adınızı kullanın:

    ```console
    server aaddscontoso.com
    ```

    Bittiğinde, editör komutunu `:wq` kullanarak *ntp.conf* dosyasını kaydedin ve çıkın.

1. VM'nin Azure AD DS yönetilen etki alanıyla senkronize olduğundan emin olmak için aşağıdaki adımlar gereklidir:

    * NTP sunucusunu durdurun
    * Yönetilen etki alanından tarih ve saati güncelleştirme
    * NTP hizmetini başlatın

    Bu adımları tamamlamak için aşağıdaki komutları çalıştırın. `ntpdate` Komutu ile kendi DNS adınızı kullanın:

    ```console
    sudo systemctl stop ntp
    sudo ntpdate aaddscontoso.com
    sudo systemctl start ntp
    ```

## <a name="join-vm-to-the-managed-domain"></a>Yönetilen etki alanına VM'ye katılma

VM'de gerekli paketler yüklendiğinden ve NTP yapılandırıldığına göre, VM'yi Azure AD DS yönetilen etki alanına katılın.

1. Azure `realm discover` AD DS yönetilen etki alanını keşfetmek için komutu kullanın. Aşağıdaki örnek, *AADDSCONTOSO.COM*alemi keşfeder. TÜM BÜYÜK HARF'te kendi Azure AD DS yönetilen etki alanı adınızı belirtin:

    ```console
    sudo realm discover AADDSCONTOSO.COM
    ```

   `realm discover` Komut Azure AD DS yönetilen etki alanınızı bulamazsa, aşağıdaki sorun giderme adımlarını gözden geçirin:

    * Etki alanına VM'den erişilebilen olduğundan emin olun. Olumlu `ping aaddscontoso.com` bir yanıtın döndürülip döndürülmedigini görmeye çalışın.
    * VM'nin Azure AD DS yönetilen etki alanının kullanılabildiği aynı veya eşlenmiş bir sanal ağa dağıtılmış olup olmadığını denetleyin.
    * Sanal ağ için DNS sunucu ayarlarının Azure AD DS yönetilen etki alanının etki alanı denetleyicilerini işaret etmek üzere güncelleştirildiğini doğrulayın.

1. Şimdi komutu kullanarak Kerberos'u hayata aşındır. `kinit` Azure AD DS yönetilen etki alanının bir parçası olan bir kullanıcı belirtin. Gerekirse, [Azure AD'deki bir gruba bir kullanıcı hesabı ekleyin.](../active-directory/fundamentals/active-directory-groups-members-azure-portal.md)

    Yine, Azure AD DS yönetilen etki alanı adı TÜM UPPERCASE girilmelidir. Aşağıdaki örnekte, adlı `contosoadmin@aaddscontoso.com` hesap Kerberos'u başlatmak için kullanılır. Azure AD DS yönetilen etki alanının bir parçası olan kendi kullanıcı hesabınızı girin:

    ```console
    kinit contosoadmin@AADDSCONTOSO.COM
    ```

1. Son olarak, `realm join` komutu kullanarak makineyi Azure AD DS yönetilen etki alanına katılın. Önceki `kinit` komutta belirttiğiniz Azure AD DS yönetilen etki alanının bir parçası olan `contosoadmin@AADDSCONTOSO.COM`aynı kullanıcı hesabını kullanın:

    ```console
    sudo realm join --verbose AADDSCONTOSO.COM -U 'contosoadmin@AADDSCONTOSO.COM' --install=/
    ```

VM'ye Azure AD DS yönetilen etki alanına katılmak birkaç dakika sürer. Aşağıdaki örnek çıktı, VM'nin Azure AD DS yönetilen etki alanına başarıyla katıldığını gösterir:

```output
Successfully enrolled machine in realm
```

VM'niz etki alanı birleştirme işlemini başarıyla tamamlayamazsa, VM'nin ağ güvenlik grubunun TCP + UDP bağlantı noktası 464'teki giden Kerberos trafiğine Azure AD DS yönetilen etki alanınız için sanal ağ alt ağına izin verdiğinden emin olun.

## <a name="update-the-sssd-configuration"></a>SSSD yapılandırmasını güncelleştirme

Bir önceki adımda yüklenen paketlerden biri Sistem Güvenlik Hizmetleri Daemon (SSSD) içindi. Bir kullanıcı etki alanı kimlik bilgilerini kullanarak bir VM'de oturum açmaya çalıştığında, SSSD isteği bir kimlik doğrulama sağlayıcısına iletir. Bu senaryoda, SSSD isteğin kimliğini doğrulamak için Azure AD DS kullanır.

1. *Sssd.conf* dosyasını bir editörle açın:

    ```console
    sudo vi /etc/sssd/sssd.conf
    ```

1. Aşağıdaki gibi *use_fully_qualified_names* için satır dışarı yorum:

    ```console
    # use_fully_qualified_names = True
    ```

    Bittiğinde, editörün `:wq` komutunu kullanarak *sssd.conf* dosyasını kaydedin ve çıkın.

1. Değişikliği uygulamak için SSSD hizmetini yeniden başlatın:

    ```console
    sudo service sssd restart
    ```

## <a name="configure-user-account-and-group-settings"></a>Kullanıcı hesabını ve grup ayarlarını yapılandırma

VM'nin Azure AD DS yönetilen etki alanına katılması ve kimlik doğrulaması için yapılandırılmasıyla, tamamlanması gereken birkaç kullanıcı yapılandırma seçeneği vardır. Bu yapılandırma değişiklikleri, parola tabanlı kimlik doğrulamaya izin vermeyi ve etki alanı kullanıcıları ilk oturum açtığında yerel VM'de otomatik olarak ev dizinleri oluşturmayı içerir.

### <a name="allow-password-authentication-for-ssh"></a>SSH için parola kimlik doğrulamasını izin verme

Varsayılan olarak, kullanıcılar yalnızca SSH ortak anahtar tabanlı kimlik doğrulamasını kullanarak bir VM'de oturum açabilir. Parola tabanlı kimlik doğrulama başarısız olur. VM'ye Azure AD DS yönetilen bir etki alanına katıldığınızda, bu etki alanı hesaplarının parola tabanlı kimlik doğrulaması kullanması gerekir. Parola tabanlı kimlik doğrulamasına aşağıdaki gibi izin vermek için SSH yapılandırmasını güncelleştirin.

1. *sshd_conf* dosyasını bir düzenleyiciyle açın:

    ```console
    sudo vi /etc/ssh/sshd_config
    ```

1. *PasswordAuthentication* için satırı *evet'e*güncelleyin:

    ```console
    PasswordAuthentication yes
    ```

    Bittiğinde, düzenleyicinin komutunu `:wq` kullanarak *sshd_conf* dosyasını kaydedin ve çıkın.

1. Değişiklikleri uygulamak ve kullanıcıların parola kullanarak oturum açmalarına izin vermek için SSH hizmetini yeniden başlatın:

    ```console
    sudo systemctl restart ssh
    ```

### <a name="configure-automatic-home-directory-creation"></a>Otomatik ev dizini oluşturma yı yapılandırma

Bir kullanıcı ilk giriş yaptığında ev dizininin otomatik olarak oluşturulmasını etkinleştirmek için aşağıdaki adımları tamamlayın:

1. */etc/pam.d/common-session* dosyasını bir düzenleyicide açın:

    ```console
    sudo vi /etc/pam.d/common-session
    ```

1. Satırın `session optional pam_sss.so`altına bu dosyada aşağıdaki satırı ekleyin:

    ```console
    session required pam_mkhomedir.so skel=/etc/skel/ umask=0077
    ```

    Bittiğinde, düzenleyicinin komutunu `:wq` kullanarak ortak *oturum* dosyasını kaydedin ve çıkın.

### <a name="grant-the-aad-dc-administrators-group-sudo-privileges"></a>'AAD DC Yöneticileri' grup sudo ayrıcalıkları hibe

Ubuntu *VM'de AAD DC Yöneticileri* grup yönetim ayrıcalıkları vermek için, */etc/sudoers'a*bir giriş eklersiniz. Eklendikten sonra, *AAD DC Yöneticileri* grubunun `sudo` üyeleri Ubuntu VM üzerindeki komutu kullanabilir.

1. Düzenleme için *sudoers* dosyasını açın:

    ```console
    sudo visudo
    ```

1. */etc/sudoers* dosyasının sonuna aşağıdaki girişi ekleyin:

    ```console
    # Add 'AAD DC Administrators' group members as admins.
    %AAD\ DC\ Administrators ALL=(ALL) NOPASSWD:ALL
    ```

    Bittiğinde, komutu kullanarak düzenleyiciyi `Ctrl-X` kaydedin ve çıkın.

## <a name="sign-in-to-the-vm-using-a-domain-account"></a>Etki alanı hesabını kullanarak VM'de oturum açma

VM'nin Azure AD DS yönetilen etki alanına başarıyla katıldığını doğrulamak için, bir etki alanı kullanıcı hesabı kullanarak yeni bir SSH bağlantısı başlatın. Bir ev dizininin oluşturulduğunu ve etki alanından grup üyeliğinin uygulandığını doğrulayın.

1. Konsolunuzdan yeni bir SSH bağlantısı oluşturun. Yönetilen etki alanına ait bir etki `ssh -l` alanı hesabı `contosoadmin@aaddscontoso.com` gibi komutu kullanarak kullanın ve daha sonra *ubuntu.aaddscontoso.com*gibi VM adresinizi girin. Azure Bulut Kabuğu'nu kullanıyorsanız, dahili DNS adı yerine VM'nin genel IP adresini kullanın.

    ```console
    ssh -l contosoadmin@AADDSCONTOSO.com ubuntu.aaddscontoso.com
    ```

1. VM'ye başarıyla bağlandığınızda, ev dizininin doğru şekilde başolarak başlandığını doğrulayın:

    ```console
    pwd
    ```

    Kullanıcı hesabıyla eşleşen kendi dizininizde */home* dizininde olmalısınız.

1. Şimdi grup üyeliklerinin doğru şekilde çözülüp çözülmediğini kontrol edin:

    ```console
    id
    ```

    Azure AD DS yönetilen etki alanından grup üyeliklerinizi görmeniz gerekir.

1. VM'de *AAD DC Yöneticileri* grubunun bir üyesi olarak oturum imzaladıysanız, komutu `sudo` doğru şekilde kullanıp kullanamayacağınızı kontrol edin:

    ```console
    sudo apt-get update
    ```

## <a name="next-steps"></a>Sonraki adımlar

VM'yi Azure AD DS yönetilen etki alanına bağlamak veya bir etki alanı hesabıyla oturum açmada sorun yaşıyorsanız, [Sorun Giderme etki alanı birleştirme sorunlarına](join-windows-vm.md#troubleshoot-domain-join-issues)bakın.

<!-- INTERNAL LINKS -->
[create-azure-ad-tenant]: ../active-directory/fundamentals/sign-up-organization.md
[associate-azure-ad-tenant]: ../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md
[create-azure-ad-ds-instance]: tutorial-create-instance.md
