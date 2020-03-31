---
title: Azure AD Etki Alanı Hizmetlerine CoreOS VM'ye katılın | Microsoft Dokümanlar
description: CoreOS sanal makinesini Azure AD Etki Alanı Hizmetleri yönetilen bir etki alanına nasıl yapılandırıp katılacağınızı öğrenin.
services: active-directory-ds
author: iainfoulds
manager: daveba
ms.assetid: 5db65f30-bf69-4ea3-9ea5-add1db83fdb8
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: conceptual
ms.date: 01/23/2020
ms.author: iainfou
ms.openlocfilehash: b97b542d11e405bab00519c68d2365dada6b6c7f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78298880"
---
# <a name="join-a-coreos-virtual-machine-to-an-azure-ad-domain-services-managed-domain"></a>Azure AD Etki Alanı Hizmetleri yönetilen etki alanına CoreOS sanal makinesine katılın

Kullanıcıların azure'da tek bir kimlik bilgileri kümesi kullanarak sanal makinelerde (VM) oturum açmalarına izin vermek için, Sanal Makinelere Azure Active Directory Domain Services (AD DS) yönetilen etki alanına katılabilirsiniz. Bir Azure AD DS yönetilen etki alanına Bir VM'ye katıldığınızda, etki alanından kullanıcı hesapları ve kimlik bilgileri sunucuları oturum açma ve yönetmek için kullanılabilir. Azure AD DS yönetilen etki alanından grup üyelikleri, VM'deki dosyalara veya hizmetlere erişimi denetlemenize izin vermek için de uygulanır.

Bu makalede, Bir CoreOS VM'ye Azure AD DS yönetilen etki alanına nasıl katılacağınız gösterilmektedir.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdaki kaynaklara ve ayrıcalıklara ihtiyacınız vardır:

* Etkin bir Azure aboneliği.
    * Azure aboneliğiniz yoksa [bir hesap oluşturun.](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
* Aboneliğinizle ilişkili bir Azure Etkin Dizin kiracısı, şirket içi bir dizini veya yalnızca bulut dizininizle eşitlenir.
    * Gerekirse, [bir Azure Etkin Dizin kiracısı oluşturun][create-azure-ad-tenant] veya [bir Azure aboneliğini hesabınızla ilişkilendirin.][associate-azure-ad-tenant]
* Azure Etkin Dizin Etki Alanı Hizmetleri, Azure AD kiracınızda etkin leştirilmiş ve yapılandırılan bir etki alanı yönetildi.
    * Gerekirse, ilk öğretici [bir Azure Active Directory Etki Alanı Hizmetleri örneği oluşturur ve yapılandırır.][create-azure-ad-ds-instance]
* Azure AD DS yönetilen etki alanının bir parçası olan bir kullanıcı hesabı.

## <a name="create-and-connect-to-a-coreos-linux-vm"></a>CoreOS Linux VM oluşturun ve bağlanın

Azure'da mevcut bir CoreOS Linux VM'niz varsa, SSH kullanarak ona bağlanın ve [VM'yi yapılandırmaya başlamak](#configure-the-hosts-file)için bir sonraki adıma geçin.

Bir CoreOS Linux VM oluşturmanız gerekiyorsa veya bu makalede kullanılmak üzere bir test VM oluşturmak istiyorsanız, aşağıdaki yöntemlerden birini kullanabilirsiniz:

* [Azure portalında](../virtual-machines/linux/quick-create-portal.md)
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
* *coreos,* yönetilen etki alanına katıldığınız CoreOS VM'nizin ana adıdır.

Bu adları kendi değerlerinizle güncelleştirin:

```console
127.0.0.1 coreos coreos.aaddscontoso.com
```

Bittiğinde, düzenleyicinin komutunu `:wq` kullanarak ana *bilgisayar* dosyasını kaydedin ve çıkın.

## <a name="configure-the-sssd-service"></a>SSSD hizmetini yapılandırma

*/etc/sssd/sssd.conf* SSSD yapılandırmasını güncelleyin.

```console
sudo vi /etc/sssd/sssd.conf
```

Aşağıdaki parametreler için kendi Azure AD DS yönetilen etki alanı adınızı belirtin:

* TÜM BÜYÜK HARF'te *alan adları*
* *[etki alanı/AADDS]* AADDS TÜM BÜYÜK HARF
* *ldap_uri*
* *ldap_search_base*
* *krb5_server*
* TÜM BÜYÜK HARFE *krb5_realm*

```console
[sssd]
config_file_version = 2
services = nss, pam
domains = AADDSCONTOSO.COM

[domain/AADDSCONTOSO.COM]
id_provider = ad
auth_provider = ad
chpass_provider = ad

ldap_uri = ldap://aaddscontoso.com
ldap_search_base = dc=aaddscontoso,dc=com
ldap_schema = rfc2307bis
ldap_sasl_mech = GSSAPI
ldap_user_object_class = user
ldap_group_object_class = group
ldap_user_home_directory = unixHomeDirectory
ldap_user_principal = userPrincipalName
ldap_account_expire_policy = ad
ldap_force_upper_case_realm = true
fallback_homedir = /home/%d/%u

krb5_server = aaddscontoso.com
krb5_realm = AADDSCONTOSO.COM
```

## <a name="join-the-vm-to-the-managed-domain"></a>Yönetilen etki alanına VM'ye katılın

SSSD yapılandırma dosyası güncelleştirildiği için, şimdi yönetilen etki alanına sanal makinekatılın.

1. İlk olarak, `adcli info` Azure AD DS yönetilen etki alanı yla ilgili bilgileri görebileceğinizi doğrulamak için komutu kullanın. Aşağıdaki örnek, etki alanı *AADDSCONTOSO.COM*için bilgi alır. TÜM BÜYÜK HARF'te kendi Azure AD DS yönetilen etki alanı adınızı belirtin:

    ```console
    sudo adcli info AADDSCONTOSO.COM
    ```

   `adcli info` Komut Azure AD DS yönetilen etki alanınızı bulamazsa, aşağıdaki sorun giderme adımlarını gözden geçirin:

    * Etki alanına VM'den erişilebilen olduğundan emin olun. Olumlu `ping aaddscontoso.com` bir yanıtın döndürülip döndürülmedigini görmeye çalışın.
    * VM'nin Azure AD DS yönetilen etki alanının kullanılabildiği aynı veya eşlenmiş bir sanal ağa dağıtılmış olup olmadığını denetleyin.
    * Sanal ağ için DNS sunucu ayarlarının Azure AD DS yönetilen etki alanının etki alanı denetleyicilerini işaret etmek üzere güncelleştirildiğini doğrulayın.

1. Şimdi `adcli join` Komutu kullanarak Azure AD DS yönetilen etki alanına VM katılın. Azure AD DS yönetilen etki alanının bir parçası olan bir kullanıcı belirtin. Gerekirse, [Azure AD'deki bir gruba bir kullanıcı hesabı ekleyin.](../active-directory/fundamentals/active-directory-groups-members-azure-portal.md)

    Yine, Azure AD DS yönetilen etki alanı adı TÜM UPPERCASE girilmelidir. Aşağıdaki örnekte, adlı `contosoadmin@aaddscontoso.com` hesap Kerberos'u başlatmak için kullanılır. Azure AD DS yönetilen etki alanının bir parçası olan kendi kullanıcı hesabınızı girin.

    ```console
    sudo adcli join -D AADDSCONTOSO.COM -U contosoadmin@AADDSCONTOSO.COM -K /etc/krb5.keytab -H coreos.aaddscontoso.com -N coreos
    ```

    VM, Azure AD DS yönetilen etki alanına başarıyla katıldığında `adcli join` komut hiçbir bilgi döndürmez.

1. Etki alanına katılma yapılandırmasını uygulamak için SSSD hizmetini başlatın:
  
    ```console
    sudo systemctl start sssd.service
    ```

## <a name="sign-in-to-the-vm-using-a-domain-account"></a>Etki alanı hesabını kullanarak VM'de oturum açma

VM'nin Azure AD DS yönetilen etki alanına başarıyla katıldığını doğrulamak için, bir etki alanı kullanıcı hesabı kullanarak yeni bir SSH bağlantısı başlatın. Bir ev dizininin oluşturulduğunu ve etki alanından grup üyeliğinin uygulandığını doğrulayın.

1. Konsolunuzdan yeni bir SSH bağlantısı oluşturun. Yönetilen etki alanına ait bir etki `ssh -l` alanı hesabı `contosoadmin@aaddscontoso.com` gibi komutu kullanarak kullanın ve daha sonra *coreos.aaddscontoso.com*gibi VM adresinizi girin. Azure Bulut Kabuğu'nu kullanıyorsanız, dahili DNS adı yerine VM'nin genel IP adresini kullanın.

    ```console
    ssh -l contosoadmin@AADDSCONTOSO.com coreos.aaddscontoso.com
    ```

1. Şimdi grup üyeliklerinin doğru şekilde çözülüp çözülmediğini kontrol edin:

    ```console
    id
    ```

    Azure AD DS yönetilen etki alanından grup üyeliklerinizi görmeniz gerekir.

## <a name="next-steps"></a>Sonraki adımlar

VM'yi Azure AD DS yönetilen etki alanına bağlamak veya bir etki alanı hesabıyla oturum açmada sorun yaşıyorsanız, [Sorun Giderme etki alanı birleştirme sorunlarına](join-windows-vm.md#troubleshoot-domain-join-issues)bakın.

<!-- INTERNAL LINKS -->
[create-azure-ad-tenant]: ../active-directory/fundamentals/sign-up-organization.md
[associate-azure-ad-tenant]: ../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md
[create-azure-ad-ds-instance]: tutorial-create-instance.md
