---
title: Azure 'da Red Hat Enterprise Linux görüntülerini yerinde yükseltme
description: Red Hat Enterprise 7. x görüntülerinden en son 8. x sürümüne yerinde yükseltme gerçekleştirme adımlarını bulun
author: mathapli
ms.service: virtual-machines-linux
ms.topic: article
ms.date: 04/16/2020
ms.author: alsin
ms.reviewer: cynthn
ms.openlocfilehash: beede74134affeb3ee0d4bdd20d5da3b4c5e6eda
ms.sourcegitcommit: 04fb3a2b272d4bbc43de5b4dbceda9d4c9701310
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/12/2020
ms.locfileid: "94566631"
---
# <a name="red-hat-enterprise-linux-in-place-upgrades"></a>Yerinde yükseltmeleri Red Hat Enterprise Linux

Bu makalede, Azure 'daki ' Leapp ' yardımcı programını kullanarak Red Hat Enterprise Linux 7 ' den Red Hat Enterprise Linux 8 ' e yerinde yükseltme gerçekleştirme hakkında adım adım yönergeler sağlanmaktadır. Yerinde yükseltme sırasında, var olan RHEL 7 işletim sistemi, RHEL 8 sürümü ile değiştirilmiştir.

>[!Note] 
> Red Hat Enterprise Linux tekliflerle SQL Server, Azure 'da yerinde yükseltmeyi desteklemez.

## <a name="what-to-expect-during-the-upgrade"></a>Yükseltme sırasında beklenmeniz gerekenler
Sistem, yükseltme sırasında birkaç kez yeniden başlatılır ve normal olur. Son yeniden başlatma işlemi, VM 'yi RHEL 8 en son küçük sürümüne yükseltir. 

Yükseltme işlemi 20 dakikadan birkaç saate kadar sürebilir. Bu, VM boyutu ve sistemde yüklü paketlerin sayısı gibi çeşitli faktörlere bağlıdır.

## <a name="preparations-for-the-upgrade"></a>Yükseltme hazırlıkları
Yerinde yükseltme, müşterilerin sistemi bir sonraki ana sürüme yükseltmesine olanak tanımak için Red Hat ve Azure 'un resmi olarak önerilmiş yoludur. Yükseltmenin daha önce gerçekleştirilmesi gereken bazı şeyler aşağıda verilmiştir ve göz önüne almanız gerekir. 

>[!Important] 
> Lütfen yükseltme işlemini gerçekleştirmeden önce görüntünün bir anlık görüntüsünü alın.

>[!NOTE]
> Bu makaledeki komutların kök hesap kullanılarak çalıştırılması gerekir

* Şu anda RHEL 7,9 olan en son RHEL 7 sürümünü kullandığınızdan emin olun. Kilitli bir sürüm kullanıyorsanız ve RHEL 7,9 sürümüne yükseltirsiniz, [buradaki adımları kullanarak BIR EUS deposuna geçiş](https://docs.microsoft.com/azure/virtual-machines/workloads/redhat/redhat-rhui#switch-a-rhel-7x-vm-back-to-non-eus-remove-a-version-lock)yapabilirsiniz.

* Yükseltmenin nasıl gittiğini ve tamamlanmacağınızı öğrenmek için aşağıdaki komutu çalıştırın. Komut, '/var/log/leapp/leapp-report.txt ' altında, işlemi ve ne yapılması gerektiğini ve yükseltmenin mümkün olup olmadığını açıklayan bir dosya üretmelidir
    ```bash
    leapp preupgrade --no-rhsm
    ```
* Yükseltme işlemi sırasında izlemeye izin verdiğinden, seri konsolunun işlevsel olduğundan emin olun.

* ' De SSH kök erişimini etkinleştir `/etc/ssh/sshd_config`
    1. `/etc/ssh/sshd_config` dosyasını açın
    1. ' #PermitRootLogin Yes ' araması yapın
    1. Açıklama eklemek için ' # ' öğesini kaldır

## <a name="steps-for-performing-the-upgrade"></a>Yükseltmeyi gerçekleştirmeye yönelik adımlar

Bu adımları dikkatle gerçekleştirin. Üretim örneklerinden önce bir test makinesinde yükseltmeyi denemek kesinlikle önerilir.

1. En son istemci paketlerini getirmek için bir yıum güncelleştirmesi gerçekleştirin.
    ```bash
    yum update -y
    ```

1. Liapp-Client-Package ' i yükler.
    ```bash
    yum install leapp-rhui-azure
    ```
    
1. Leapp-Data. tar. gz dosyasını repomap.csv ve pes-events.jsile birlikte kullanarak, [RedHat portalında](https://access.redhat.com/articles/3664871)sunun ve bunları ayıklayın. 
    1. Dosyayı indirin.
    1. Aşağıdaki komutu kullanarak içeriği ayıklayın ve dosyayı kaldırın:
    ```bash
    tar -xzf leapp-data12.tar.gz -C /etc/leapp/files && rm leapp-data12.tar.gz
    ```

1. ' Liapp ' için ' yanıtlar ' dosyası ekleyin.
    ```bash
    leapp answer --section remove_pam_pkcs11_module_check.confirm=True --add
    ``` 

1. ' Leapp ' yükseltmesini gerçekleştirin.
    ```bash
    leapp upgrade --no-rhsm
    ```
1.  `leapp upgrade`Komut başarıyla tamamlandıktan sonra, işlemi gerçekleştirmek için sistemi el ile yeniden başlatın. Sistem, devre dışı bırakılacak şekilde birkaç kez yeniden başlatılacak. Seri konsolunu kullanarak işlemi izleyin.

1.  Yükseltmenin başarıyla tamamlandığını doğrulayın.
    ```bash
    uname -a && cat /etc/redhat-release
    ```

1. Yükseltme tamamlandıktan sonra kök SSH erişimini kaldırın.
    1. `/etc/ssh/sshd_config` dosyasını açın
    1. ' #PermitRootLogin Yes ' araması yapın
    1. Açıklamaya ' # ' ekleyin

1. Değişikliklerin etkili olması için SSHD hizmetini yeniden başlatın
    ```bash
    systemctl restart sshd
    ```

## <a name="common-issues"></a>Genel Sorunlar
Bunlar `leapp preupgrade` veya işlemin başarısız olduğu bazı yaygın örneklerden bazılarıdır `leapp upgrade` .

**Hata: aşağıdaki devre dışı eklenti desenleri için eşleşme bulunamadı**
```plaintext
STDERR:
No matches found for the following disabled plugin patterns: subscription-manager
Warning: Packages marked by Leapp for upgrade not found in repositories metadata: gpg-pubkey
```
**Çözüm**\
Dosyayı düzenleyerek `/etc/yum/pluginconf.d/subscription-manager.conf` ve etkin olarak ' ye değiştirerek abonelik Yöneticisi eklentisini devre dışı bırakın `enabled=0` .

Bu, PAYG VM 'Leri için kullanılmayan abonelik Yöneticisi 'nin etkinleştirilme eklentisine neden olur.

**Hata: kök kullanılarak uzaktan oturum açmada olası sorunlar** `leapp preupgrade` Şu hatayla başarısız olabilir:
```structured-text
============================================================
                     UPGRADE INHIBITED
============================================================

Upgrade has been inhibited due to the following problems:
    1. Inhibitor: Possible problems with remote login using root account
Consult the pre-upgrade report for details and possible remediation.

============================================================
                     UPGRADE INHIBITED
============================================================
```
**Çözüm**\
' De kök erişimi etkinleştirin `/etc/sshd_conf` .
Bunun nedeni, `/etc/sshd_conf` "[yükseltme hazırlıkları](#preparations-for-the-upgrade)" bölümünde olduğu gibi kök SSH erişiminin etkinleştirilmemesinin nedeni. 

## <a name="next-steps"></a>Sonraki adımlar
* [Azure 'Da Red Hat görüntüleri](./redhat-images.md)hakkında daha fazla bilgi edinin.
* [Red Hat güncelleştirme altyapısı](./redhat-rhui.md)hakkında daha fazla bilgi edinin.
* [RHEL BYOS teklifi](./byos.md)hakkında daha fazla bilgi edinin.
* Red Hat yerinde yükseltme işlemleriyle ilgili bilgiler Red Hat belgelerinde bulunabilir ve [RHEL 7 ' den RHEL 8](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/html-single/upgrading_from_rhel_7_to_rhel_8/index) ' e yükseltilir.
* Tüm RHEL sürümleri için Red Hat destek ilkeleriyle ilgili bilgiler [Red Hat Enterprise Linux yaşam döngüsü](https://access.redhat.com/support/policy/updates/errata) sayfasında bulunabilir.