---
title: Azure HDInsight'ta etki alanı hesapları için SSH erişimini yönetme
description: HDInsight'ta Azure AD hesapları için SSH erişimini yönetme adımları.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 02/14/2020
ms.openlocfilehash: 5529989384df75b592afa8f5e4960eb9817fb2d7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77472524"
---
# <a name="manage-ssh-access-for-domain-accounts-in-azure-hdinsight"></a>Azure HDInsight'ta etki alanı hesapları için SSH erişimini yönetme

Güvenli kümelerde, varsayılan olarak, [Azure AD DS'deki](../../active-directory-domain-services/overview.md) tüm etki alanı kullanıcılarının baş ve kenar düğümlerine [SSH](../hdinsight-hadoop-linux-use-ssh-unix.md) girmesine izin verilir. Bu kullanıcılar sudoers grubunun bir parçası değildir ve kök erişim alamadım. Küme oluşturma sırasında oluşturulan SSH kullanıcısı kök erişimine sahip olacaktır.

## <a name="manage-access"></a>Erişimi yönetme

Belirli kullanıcılara veya gruplara SSH `/etc/ssh/sshd_config` erişimini değiştirmek için düğümlerin her birini güncelleştirin.

1. Kümenize bağlanmak için [ssh komutunu](../hdinsight-hadoop-linux-use-ssh-unix.md) kullanın. CLUSTERNAME'yi kümenizin adıyla değiştirerek aşağıdaki komutu düzenleme ve ardından komutu girin:

    ```cmd
    ssh sshuser@CLUSTERNAME-ssh.azurehdinsight.net
    ```

1. G `ssh_confi`dosyasını açın.

    ```bash
    sudo nano /etc/ssh/sshd_config
    ```

1. Dosyayı `sshd_config` istediğiniz gibi değiştirin. Kullanıcıları belirli gruplarla sınırlandırırsanız, yerel hesaplar bu düğüme SSH veremez. Aşağıda yalnızca sözdizimi örneği verilmiştir:

    ```bash
    AllowUsers useralias1 useralias2

    AllowGroups groupname1 groupname2
    ```

    Sonra değişiklikleri **kaydedin: Ctrl + X**, **Y**, **Girin**.

1. SSHD'yi yeniden başlatın.

    ```bash
    sudo systemctl restart sshd
    ```

1. Her düğüm için yukarıdaki adımları yineleyin.

## <a name="ssh-authentication-log"></a>SSH kimlik doğrulama günlüğü

SSH kimlik doğrulama günlüğü `/var/log/auth.log`' ye yazılır. Yerel veya etki alanı hesapları için SSH üzerinden herhangi bir giriş hatası görürseniz, hataları hata ayıklamak için günlük üzerinden gitmek gerekir. Genellikle sorun belirli kullanıcı hesapları ile ilgili olabilir ve genellikle varsayılan SSH kullanıcı (yerel hesap) kullanarak diğer kullanıcı hesapları veya SSH denemek ve sonra bir kinit girişimi iyi bir uygulamadır.

## <a name="ssh-debug-log"></a>SSH hata ayıklama günlüğü

Ayrıntılı günlüğe kaydetmeyi etkinleştirmek `sshd` `-d` için, seçeneğiyle yeniden başlatmanız gerekir. Ana `/usr/sbin/sshd -d` SSH `sshd` daemon durdurmak zorunda kalmamak için özel bir bağlantı noktasında (2222 gibi) çalıştırabilirsiniz gibi. Daha fazla `-v` günlük almak için SSH istemcisi ile seçeneği de kullanabilirsiniz (hataların istemci tarafı görünümü).

## <a name="next-steps"></a>Sonraki adımlar

* [Kurumsal Güvenlik Paketi ile HDInsight kümelerini yönetme](./apache-domain-joined-manage.md)
* [SSH kullanarak HDInsight'a (Apache Hadoop) bağlanın.](../hdinsight-hadoop-linux-use-ssh-unix.md)
