---
title: Azure AD Etki Alanı Hizmetlerine CentOS VM'ye katılın | Microsoft Dokümanlar
description: Bir CentOS Linux sanal makinesini Azure AD Etki Alanı Hizmetleri yönetilen bir etki alanına nasıl yapılandırabileceğinizi ve bu makineye nasıl katılacağınızı öğrenin.
services: active-directory-ds
author: iainfoulds
manager: daveba
ms.assetid: 16100caa-f209-4cb0-86d3-9e218aeb51c6
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: how-to
ms.date: 01/23/2020
ms.author: iainfou
ms.openlocfilehash: d08552dcae51c897f2419d94e5e61e857247f09a
ms.sourcegitcommit: 62c5557ff3b2247dafc8bb482256fef58ab41c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80655227"
---
# <a name="join-a-centos-linux-virtual-machine-to-an-azure-ad-domain-services-managed-domain"></a>Bir CentOS Linux sanal makinesine Azure AD Etki Alanı Hizmetleri yönetilen etki alanına katılın

Kullanıcıların azure'da tek bir kimlik bilgileri kümesi kullanarak sanal makinelerde (VM) oturum açmalarına izin vermek için, Sanal Makinelere Azure Active Directory Domain Services (AD DS) yönetilen etki alanına katılabilirsiniz. Bir Azure AD DS yönetilen etki alanına Bir VM'ye katıldığınızda, etki alanından kullanıcı hesapları ve kimlik bilgileri sunucuları oturum açma ve yönetmek için kullanılabilir. Azure AD DS yönetilen etki alanından grup üyelikleri, VM'deki dosyalara veya hizmetlere erişimi denetlemenize izin vermek için de uygulanır.

Bu makalede, Bir CentOS Linux VM'ye Azure AD DS yönetilen etki alanına nasıl katılacağınız gösterilmektedir.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdaki kaynaklara ve ayrıcalıklara ihtiyacınız vardır:

* Etkin bir Azure aboneliği.
    * Azure aboneliğiniz yoksa [bir hesap oluşturun.](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
* Aboneliğinizle ilişkili bir Azure Etkin Dizin kiracısı, şirket içi bir dizini veya yalnızca bulut dizininizle eşitlenir.
    * Gerekirse, [bir Azure Etkin Dizin kiracısı oluşturun][create-azure-ad-tenant] veya [bir Azure aboneliğini hesabınızla ilişkilendirin.][associate-azure-ad-tenant]
* Azure Etkin Dizin Etki Alanı Hizmetleri, Azure AD kiracınızda etkin leştirilmiş ve yapılandırılan bir etki alanı yönetildi.
    * Gerekirse, ilk öğretici [bir Azure Active Directory Etki Alanı Hizmetleri örneği oluşturur ve yapılandırır.][create-azure-ad-ds-instance]
* Azure AD DS yönetilen etki alanının bir parçası olan bir kullanıcı hesabı.

## <a name="create-and-connect-to-a-centos-linux-vm"></a>CentOS Linux VM oluşturun ve bağlanın

Azure'da mevcut bir CentOS Linux VM'iniz varsa, SSH kullanarak ona bağlanın ve [VM'yi yapılandırmaya başlamak](#configure-the-hosts-file)için bir sonraki adıma geçin.

Bir CentOS Linux VM oluşturmanız gerekiyorsa veya bu makalede kullanılmak üzere bir test VM oluşturmak istiyorsanız, aşağıdaki yöntemlerden birini kullanabilirsiniz:

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
* *centos,* yönetilen etki alanına katıldığınız CentOS VM'nizin ana adıdır.

Bu adları kendi değerlerinizle güncelleştirin:

```console
127.0.0.1 centos.aaddscontoso.com centos
```

Bittiğinde, düzenleyicinin komutunu `:wq` kullanarak ana *bilgisayar* dosyasını kaydedin ve çıkın.

## <a name="install-required-packages"></a>Gerekli paketleri yükleme

VM'nin Azure AD DS yönetilen etki alanına katılmak için bazı ek paketlere ihtiyacı vardır. Bu paketleri yüklemek ve yapılandırmak için aşağıdakileri kullanarak `yum`etki alanı birleştirme araçlarını güncelleştirin ve yükleyin:

```console
sudo yum install realmd sssd krb5-workstation krb5-libs oddjob oddjob-mkhomedir samba-common-tools
```

## <a name="join-vm-to-the-managed-domain"></a>Yönetilen etki alanına VM'ye katılma

VM'de gerekli paketler yüklendiğinden, VM'yi Azure AD DS yönetilen etki alanına katılın.

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
    sudo realm join --verbose AADDSCONTOSO.COM -U 'contosoadmin@AADDSCONTOSO.COM'
    ```

VM'ye Azure AD DS yönetilen etki alanına katılmak birkaç dakika sürer. Aşağıdaki örnek çıktı, VM'nin Azure AD DS yönetilen etki alanına başarıyla katıldığını gösterir:

```output
Successfully enrolled machine in realm
```

VM'niz etki alanı birleştirme işlemini başarıyla tamamlayamazsa, VM'nin ağ güvenlik grubunun TCP + UDP bağlantı noktası 464'teki giden Kerberos trafiğine Azure AD DS yönetilen etki alanınız için sanal ağ alt ağına izin verdiğinden emin olun.

## <a name="allow-password-authentication-for-ssh"></a>SSH için parola kimlik doğrulamasını izin verme

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
    sudo systemctl restart sshd
    ```

## <a name="grant-the-aad-dc-administrators-group-sudo-privileges"></a>'AAD DC Yöneticileri' grup sudo ayrıcalıkları hibe

*AAD DC Yöneticileri* grup üyelerine CentOS VM'de yönetim ayrıcalıkları vermek için/etc/sudoers'a bir giriş eklersiniz. */etc/sudoers* Eklendikten sonra, *AAD DC Yöneticileri* grubunun `sudo` üyeleri CentOS VM üzerindeki komutu kullanabilir.

1. Düzenleme için *sudoers* dosyasını açın:

    ```console
    sudo visudo
    ```

1. */etc/sudoers* dosyasının sonuna aşağıdaki girişi ekleyin. *AAD DC Yöneticileri* grubu adında beyaz boşluk içerir, bu nedenle grup adına ters eğik çizgi kaçış karakterini içerir. *aaddscontoso.com*gibi kendi etki alanı adınızı ekleyin:

    ```console
    # Add 'AAD DC Administrators' group members as admins.
    %AAD\ DC\ Administrators@aaddscontoso.com ALL=(ALL) NOPASSWD:ALL
    ```

    Bittiğinde, editörün komutunu `:wq` kullanarak düzenleyiciyi kaydedin ve çıkın.

## <a name="sign-in-to-the-vm-using-a-domain-account"></a>Etki alanı hesabını kullanarak VM'de oturum açma

VM'nin Azure AD DS yönetilen etki alanına başarıyla katıldığını doğrulamak için, bir etki alanı kullanıcı hesabı kullanarak yeni bir SSH bağlantısı başlatın. Bir ev dizininin oluşturulduğunu ve etki alanından grup üyeliğinin uygulandığını doğrulayın.

1. Konsolunuzdan yeni bir SSH bağlantısı oluşturun. Yönetilen etki `ssh -l` alanına ait bir etki alanı hesabı `contosoadmin@aaddscontoso.com` gibi komutu kullanarak kullanın ve daha sonra *centos.aaddscontoso.com*gibi VM adresinizi girin. Azure Bulut Kabuğu'nu kullanıyorsanız, dahili DNS adı yerine VM'nin genel IP adresini kullanın.

    ```console
    ssh -l contosoadmin@AADDSCONTOSO.com centos.aaddscontoso.com
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
    sudo yum update
    ```

## <a name="next-steps"></a>Sonraki adımlar

VM'yi Azure AD DS yönetilen etki alanına bağlamak veya bir etki alanı hesabıyla oturum açmada sorun yaşıyorsanız, [Sorun Giderme etki alanı birleştirme sorunlarına](join-windows-vm.md#troubleshoot-domain-join-issues)bakın.

<!-- INTERNAL LINKS -->
[create-azure-ad-tenant]: ../active-directory/fundamentals/sign-up-organization.md
[associate-azure-ad-tenant]: ../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md
[create-azure-ad-ds-instance]: tutorial-create-instance.md
