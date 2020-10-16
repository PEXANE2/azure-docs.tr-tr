---
title: Azure NetApp Files için NFS istemcisi yapılandırma | Microsoft Docs
description: NFS istemcilerinin Azure NetApp Files ile kullanmak üzere nasıl yapılandırılacağını açıklar.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 09/28/2020
ms.author: b-juche
ms.openlocfilehash: b2e597ff8fc761b66de6228063c471933a364144
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91449658"
---
# <a name="configure-an-nfs-client-for-azure-netapp-files"></a>Azure NetApp Files için NFS istemcisini yapılandırma

Bu makalede açıklanan NFS istemci yapılandırması, [NFSv 4.1 Kerberos şifrelemesini yapılandırırken](configure-kerberos-encryption.md) veya [bir çift protokol birimi oluşturduğunuzda](create-volumes-dual-protocol.md)kurulum 'un bir parçasıdır. Azure NetApp Files ile kullanılabilecek çok çeşitli Linux dağıtımları vardır. Bu makalede, daha yaygın olarak kullanılan ortamların ikisi için yapılandırma açıklanmaktadır: RHEL 8 ve Ubuntu 18,04. 

Kullandığınız Linux Flavor ne olursa olsun, aşağıdaki konfigürasyonlar gereklidir:
* Zaman eğimiyle ilgili sorunları önlemek için bir NTP İstemcisi yapılandırın.
* Ad çözümlemesi için Linux istemcisinin DNS girişlerini yapılandırın.  
    Bu yapılandırma, "A" (ileri) kaydı ve PTR (ters) kaydını içerir. 
* Etki alanına katılması için hedef Active Directory (bölge birleşimi komutu sırasında oluşturulur) bir bilgisayar hesabı oluşturun. 
    > [!NOTE] 
    > `$SERVICEACCOUNT`Aşağıdaki komutlarda kullanılan değişken, hedeflenen kuruluş biriminde bir bilgisayar hesabı oluşturmak için izinlere veya temsilciliine sahip bir kullanıcı hesabı olmalıdır.
* İstemcisini NFS birimleri ve diğer ilgili izleme araçlarını bağlamak için etkinleştirin.

## <a name="rhel-8-configuration"></a>RHEL 8 yapılandırması 

1. Paketleri yükler:   
    `sudo yum -y install realmd sssd adcli samba-common krb5-workstation chrony`

2. NTP istemcisini yapılandırın:  
    RHEL 8 `chrony` Varsayılan olarak kullanır.  [NTP 'yi yapılandırmak için, zaman hatası paketini kullanma](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/html/configuring_basic_system_settings/using-chrony-to-configure-ntp)konusundaki yapılandırma yönergelerini takip edin.

3. Active Directory etki alanına ekleyin:  
    `sudo realm join $DOMAIN.NAME -U $SERVICEACCOUNT --computer-ou= OU=$YOUROU,DC=$DOMAIN,DC=TLD`

## <a name="ubuntu-configuration"></a>Ubuntu yapılandırması 
Bu bölümde, NFS istemcileri için Ubuntu yapılandırması açıklanmaktadır.  

### <a name="if-you-are-using-nfsv41-kerberos-encryption"></a>NFSv 4.1 Kerberos Şifrelemesi kullanıyorsanız 

1. Paketleri yükler:  
    `sudo yum -y install realmd packagekit sssd adcli samba-common krb5-workstation chrony`

2. NTP istemcisini yapılandırın.  
    Ubuntu 18,04 `chrony` Varsayılan olarak kullanır.  [Ubuntu Bionic](https://ubuntu.com/blog/ubuntu-bionic-using-chrony-to-configure-ntp)içindeki yapılandırma yönergelerinden sonra, NTP 'yi yapılandırmak için zaman hatası kullanımı.

3. Active Directory Etki Alanı ekleyin:  
    `sudo realm join $DOMAIN.NAME -U $SERVICEACCOUNT --computer-ou= OU=$YOUROU,DC=$DOMAIN,DC=TLD`

### <a name="if-you-are-using-dual-protocol"></a>İkili protokol kullanıyorsanız  

1. Yüklü paketleri yükseltmek için aşağıdaki komutu çalıştırın:  
    `sudo apt update && sudo apt install libnss-ldap libpam-ldap ldap-utils nscd`

    Örnek:   

    `base dc=hariscus,dc=com` `uri ldap://10.20.0.4:389/`
    `ldap_version 3`
    `rootbinddn cn=admin,cn=Users,dc=hariscus,dc=com`
    `pam_password ad`
 
2. Hizmetini yeniden başlatıp etkinleştirmek için aşağıdaki komutu çalıştırın:   
    `sudo systemctl restart nscd && sudo systemctl enable nscd`

Aşağıdaki örnek, bir LDAP kullanıcısı için Ubuntu LDAP istemcisinden AD LDAP sunucusunu sorgular `ldapu1` :   

`root@cbs-k8s-varun4-04:/home/cbs# getent passwd hari1`   
`hari1:*:1237:1237:hari1:/home/hari1:/bin/bash`   

## <a name="next-steps"></a>Sonraki adımlar  

* [Azure NetApp Files için NFS birimi oluşturma](azure-netapp-files-create-volumes.md)
* [Azure NetApp Files için çift protokol birimi oluşturma](create-volumes-dual-protocol.md)

