---
title: Azure Site Kurtarma ile Hyper-V VM disklerini olağanüstü durum kurtarmadan Azure'a hariç tutma
description: Hyper-V VM disklerinin Azure Site Kurtarma ile çoğaltılmasından Azure'a nasıl dışlanır?
author: mayurigupta13
manager: rochakm
ms.topic: conceptual
ms.author: mayg
ms.date: 11/12/2019
ms.openlocfilehash: 50fb6da2905b2ae27547f25cce3d7a76ca7976b7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75498126"
---
# <a name="exclude-disks-from-replication"></a>Diskleri çoğaltmanın dışında tutma

Bu makalede, Hyper-V VM'leri Azure'a kopyalarken disklerin nasıl dışlanmayacağı açıklanmaktadır. Diskleri birkaç nedenden dolayı çoğaltmadan hariç tutmak isteyebilirsiniz:

- Dışlanan diskte çalkalanan önemsiz verilerin çoğaltılmadığından emin olun.
- Çoğaltmanız gerekmeyen diskleri hariç tutarak tüketilen çoğaltma bant genişliğini veya hedef tarafındaki kaynakları optimize edin.
- İhtiyacınız olmayan verileri çoğaltmayarak depolama alanı ve ağ kaynaklarını kaydedin.

Diskleri çoğaltmadan çıkarmadan önce:

- Diskleri hariç tlandırma hakkında [daha fazla bilgi edinin.](exclude-disks-replication.md)
- Bir diski dışlamanın çoğaltmayı, başarısızı ve başarısızı nasıl etkilediğini gösteren [tipik dışlama senaryolarını](exclude-disks-replication.md#typical-scenarios) ve [örneklerini](exclude-disks-replication.md#example-1-exclude-the-sql-server-tempdb-disk) gözden geçirin.

## <a name="before-you-start"></a>Başlamadan önce

Başlamadan önce aşağıdakilere dikkat edin:

- **Çoğaltma**: Varsayılan olarak bir makinedeki tüm diskler çoğaltılır.
- **Disk türü**:
    - Temel diskleri çoğaltmadan hariç tutabilirsiniz.
    - İşletim sistemi diskleri dışarıda tutulamaz.
    - Dinamik diskleri dışarıda tutmamanızı öneririz. Site Kurtarma, konuk VM'de hangi VHD'nin temel veya dinamik olduğunu belirleyemez.  Tüm bağımlı dinamik birim diskleri hariç tutmazsanız, korumalı dinamik disk VM üzerinde başarısız bir diske dönüşür ve bu diskteki verilere erişilemez.
- **Diskekleme/kaldırma/dışlama**: Çoğaltmayı etkinleştirdikten sonra çoğaltma için diskleri ekleyip/çıkaramazsınız/çıkaramazsınız. Bir disk eklemek/kaldırmak veya dışlamak istiyorsanız, VM için korumayı devre dışı bırakıp yeniden etkinleştirmeniz gerekir.
- **Failover**: Başarısız olduktan sonra, uygulamalar üzerinde başarısız olursa çalışmak için diskleri dışlamanız gerekir, bu diskleri el ile oluşturmanız gerekir. Alternatif olarak, makinenin arızalanması sırasında diskoluşturmak için Azure otomasyonu kurtarma planına entegre edebilirsiniz.
- **Failback**: Başarısız olduktan sonra şirket içi sitenize geri döndüğünüzde, Azure'da el ile oluşturduğunuz diskler geri başarısız olmaz. Örneğin, üç disk üzerinde başarısız olur ve doğrudan bir Azure VM üzerinde iki disk oluşturursanız, yalnızca üzerinde başarısız olan üç disk geri başarısız olur. Başarısız geri dönüşte veya VM'lerin ters çoğaltmasında el ile oluşturulan diskleri dahil emezsiniz.

## <a name="exclude-disks"></a>Diskleri dışlama

1. Bir Hyper-V VM için [çoğaltmayı etkinleştirdiğinizde](site-recovery-hyper-v-site-to-azure.md) diskleri dışlamak için, çoğaltmak istediğiniz VM'leri seçtikten sonra, **Çoğaltma** > **Özelliklerini** > **Yapılandırmayı** Etkinleştir sayfasında, **sütunçoğaltmak için Diskleri** gözden geçirin. Varsayılan olarak tüm diskler çoğaltma için seçilir.
2. Belirli bir diski çoğaltmak istemiyorsanız, dışlamak istediğiniz disklerin seçimini **çoğaltmak** için Diskler'de. 

    ![Diskleri çoğaltmanın dışında tutma](./media/hyper-v-exclude-disk/enable-replication6-with-exclude-disk.png)


## <a name="next-steps"></a>Sonraki adımlar
Dağıtımınız ayarlandıktan ve çalışmaya başladıktan sonra farklı türdeki yük devretmeler hakkında [daha fazla bilgi edinebilirsiniz](failover-failback-overview.md).
