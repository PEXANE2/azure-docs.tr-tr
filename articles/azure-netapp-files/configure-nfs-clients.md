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
ms.date: 07/27/2020
ms.author: b-juche
ms.openlocfilehash: bdcffcea8d695b6a3d49272ffa135187d77aae1e
ms.sourcegitcommit: 3d56d25d9cf9d3d42600db3e9364a5730e80fa4a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/03/2020
ms.locfileid: "87535564"
---
# <a name="configure-an-nfs-client-for-azure-netapp-files"></a>Azure NetApp Files için NFS istemcisini yapılandırma

Azure NetApp Files ile kullanılabilecek çok çeşitli Linux dağıtımları vardır. Bu makalede, daha yaygın olarak kullanılan ortamların ikisi için yapılandırma açıklanmaktadır: RHEL 8 ve Ubuntu 18,04.

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

1. Paketleri yükler:  
    `sudo yum -y install realmd packagekit sssd adcli samba-common krb5-workstation chrony`

2. NTP istemcisini yapılandırın.  
    Ubuntu 18,04 `chrony` Varsayılan olarak kullanır.  [Ubuntu Bionic](https://ubuntu.com/blog/ubuntu-bionic-using-chrony-to-configure-ntp)içindeki yapılandırma yönergelerinden sonra, NTP 'yi yapılandırmak için zaman hatası kullanımı.

3. Active Directory Etki Alanı ekleyin:  
    `sudo realm join $DOMAIN.NAME -U $SERVICEACCOUNT --computer-ou= OU=$YOUROU,DC=$DOMAIN,DC=TLD`

## <a name="next-steps"></a>Sonraki adımlar  

* [Azure NetApp Files için NFS birimi oluşturma](azure-netapp-files-create-volumes.md)
* [Azure NetApp Files için çift protokol birimi oluşturma](create-volumes-dual-protocol.md)

