---
title: Azure 'da Red Hat Enterprise Linux görüntülerini yerinde yükseltme
description: Red Hat Enterprise 7. x görüntülerinden en son 8. x sürümüne yerinde yükseltme gerçekleştirme adımlarını bulun
author: mathapli
ms.service: virtual-machines-linux
ms.topic: article
ms.date: 04/16/2020
ms.author: alsin
ms.reviewer: cynthn
ms.openlocfilehash: 48884e6faa5f26f027c772b44d5f960979a40d1d
ms.sourcegitcommit: 6109f1d9f0acd8e5d1c1775bc9aa7c61ca076c45
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2020
ms.locfileid: "94447905"
---
# <a name="red-hat-enterprise-linux-in-place-upgrades"></a>Yerinde yükseltmeleri Red Hat Enterprise Linux

Bu makalede, Azure 'daki ' Leapp ' yardımcı programını kullanarak Red Hat Enterprise Linux 7 ' den Red Hat Enterprise Linux 8 ' e yerinde yükseltme gerçekleştirme hakkında adım adım yönergeler sağlanmaktadır. Yerinde yükseltme sırasında, var olan RHEL 7 işletim sistemi, RHEL 8 sürümü ile değiştirilmiştir.

>[!Note] 
> Red Hat Enterprise Linux tekliflerle SQL Server, Azure 'da yerinde yükseltmeyi desteklemez.

## <a name="what-to-expect-during-the-upgrade"></a>Yükseltme sırasında beklenmeniz gerekenler
Sistem, yükseltme sırasında birkaç kez yeniden başlatılır ve normal olur. Son yeniden başlatma işlemi, VM 'yi RHEL 8 en son küçük sürümüne yükseltir.

## <a name="preparations-for-the-upgrade"></a>Yükseltme hazırlıkları
Yerinde yükseltmeler, müşterilerin sisteminizi bir sonraki ana sürüme yükseltmelerini sağlamak için Red Hat ve Azure tarafından önerilen resmi olarak önerilen bir yoldur. Yükseltmenin daha önce gerçekleştirilmesi gereken bazı şeyler aşağıda verilmiştir ve göz önüne almanız gerekir. 

>[!Important] 
> Lütfen yükseltme işlemini gerçekleştirmeden önce görüntünün bir anlık görüntüsünü alın.

>[!NOTE]
> Bu makaledeki komutların kök hesap kullanılarak çalıştırılması gerekir

* Şu anda RHEL 7,9 olan en son RHEL 7 sürümünü kullandığınızdan emin olun. Kilitli bir sürüm kullanıyorsanız ve RHEL 7,9 sürümüne yükseltirsiniz, [buradaki adımları kullanarak BIR EUS deposuna geçiş](https://docs.microsoft.com/azure/virtual-machines/workloads/redhat/redhat-rhui#switch-a-rhel-7x-vm-back-to-non-eus-remove-a-version-lock)yapabilirsiniz.

* Yükseltmenin nasıl gittiğini ve tamamlanmacağınızı öğrenmek için aşağıdaki komutu çalıştırın. Komut, '/var/log/leapp/leapp-report.txt ' altında, işlemi ve ne yapılması gerektiğini ve yükseltmenin mümkün olup olmadığını açıklayan bir dosya üretmelidir
    ```bash
    leapp preupgrade --no-rhsm
    ```

## <a name="steps-for-performing-the-upgrade"></a>Yükseltmeyi gerçekleştirmeye yönelik adımlar

Bu adımları dikkatle gerçekleştirin. Üretim örneklerinden önce bir test makinesinde yükseltmeyi denemek kesinlikle önerilir.

1. En son istemci paketlerini getirmek için bir yıum güncelleştirmesi gerçekleştirin.
    ```bash
    yum update
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
    
1. /Etc/ssh/sshd_config 'da Permitrootlogın 'i etkinleştirme
    1. /Etc/ssh/sshd_config dosyasını açın
    1. ' #PermitRootLogin Yes ' araması yapın
    1. Açıklama eklemek için ' # ' öğesini kaldır



1. ' Leapp ' yükseltmesini gerçekleştirin.
    ```bash
    leapp upgrade --no-rhsm
    ```
1. Değişikliklerin etkili olması için SSHD hizmetini yeniden başlatın
    ```bash
    systemctl restart sshd
    ```
1. /Etc/ssh/sshd_config için Permitrootlogın 'e açıklama ekleyin
    1. /Etc/ssh/sshd_config dosyasını açın
    1. ' #PermitRootLogin Yes ' araması yapın
    1. Açıklamaya ' # ' ekleyin

## <a name="next-steps"></a>Sonraki adımlar
* [Azure 'Da Red Hat görüntüleri](./redhat-images.md)hakkında daha fazla bilgi edinin.
* [Red Hat güncelleştirme altyapısı](./redhat-rhui.md)hakkında daha fazla bilgi edinin.
* [RHEL BYOS teklifi](./byos.md)hakkında daha fazla bilgi edinin.
* Red Hat yerinde yükseltme işlemleriyle ilgili bilgiler Red Hat belgelerinde bulunabilir ve [RHEL 7 ' den RHEL 8](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/html-single/upgrading_from_rhel_7_to_rhel_8/index) ' e yükseltilir.
* Tüm RHEL sürümleri için Red Hat destek ilkeleriyle ilgili bilgiler [Red Hat Enterprise Linux yaşam döngüsü](https://access.redhat.com/support/policy/updates/errata) sayfasında bulunabilir.