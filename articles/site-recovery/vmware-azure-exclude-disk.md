---
title: Azure Site Kurtarma ile VMware VM disklerini olağanüstü durum kurtarmadan Azure'a hariç tutma
description: Azure Site Kurtarma ile VMware VM disklerinin çoğaltılmasından Azure'a nasıl dışlanır?
author: mayurigupta13
manager: rochakm
ms.date: 12/10/2019
ms.author: mayg
ms.topic: conceptual
ms.openlocfilehash: cd54da5ee01206e576157435135065189bfb8035
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75495358"
---
# <a name="exclude-disks-from-vmware-vm-replication-to-azure"></a>Diskleri VMware VM çoğaltmadan Azure'a hariç tutma

Bu makalede, olağanüstü durum kurtarma için VMware VM'leri Azure'a kopyalarken disklerin nasıl dışlanmayacağı açıklanmaktadır. Diskleri birkaç nedenden dolayı çoğaltmadan hariç tutmak isteyebilirsiniz:

- Dışlanan diskte çalkalanan önemsiz verilerin çoğaltılmadığından emin olun.
- Çoğaltmanız gerekmeyen diskleri hariç tutarak tüketilen çoğaltma bant genişliğini veya hedef tarafındaki kaynakları optimize edin.
- İhtiyacınız olmayan verileri çoğaltmayarak depolama alanı ve ağ kaynaklarını kaydedin.

Diskleri çoğaltmadan çıkarmadan önce:

- Diskleri hariç tlandırma hakkında [daha fazla bilgi edinin.](exclude-disks-replication.md)
- Bir diski dışlamanın çoğaltmayı, başarısızı ve başarısızı nasıl etkilediğini gösteren [tipik dışlama senaryolarını](exclude-disks-replication.md#typical-scenarios) ve [örneklerini](exclude-disks-replication.md#example-1-exclude-the-sql-server-tempdb-disk) gözden geçirin.

## <a name="before-you-start"></a>Başlamadan önce

 Başlamadan önce aşağıdakilere dikkat edin:

- **Çoğaltma**: Varsayılan olarak, bir makinedeki tüm diskler çoğaltılır.
- **Disk türü**: Çoğaltma dışında yalnızca temel diskler dışlanabilir. İşletim sistemi veya dinamik diskler dışarıda tutulamaz.
- **Mobilite hizmeti**: Bir diski çoğaltmadan hariç tutmak için, çoğaltmayı etkinleştirmeden önce makineye Mobilite hizmetini el ile yüklemeniz gerekir. Bu yöntem, yalnızca çoğaltma etkinleştirildikten sonra Mobilite hizmetini vm'ye yüklediğinden, itme yüklemesini kullanamazsınız.  
- **Diskekleme/kaldırma/dışlama**: Çoğaltmayı etkinleştirdikten sonra çoğaltma için diskleri ekleyip/çıkaramazsınız/çıkaramazsınız. Disk eklemek/kaldırmak veya dışlamak istiyorsanız, makine nin korumasını devre dışı bırakıp yeniden etkinleştirmeniz gerekir.
- **Failover**: Başarısız olduktan sonra, uygulamaların çalışması için dışlanmış disklere ihtiyaç duyulduğunda başarısız olduysanız, bu diskleri el ile oluşturmanız gerekir. Alternatif olarak, makinenin arızalanması sırasında diskoluşturmak için Azure otomasyonu kurtarma planına entegre edebilirsiniz.
- **Failback-Windows**: Başarısız olduktan sonra şirket içi sitenize geri döndüğünüzde, Azure'da el ile oluşturduğunuz Windows diskleri geri başarısız olmaz. Örneğin, üç disk üzerinde başarısız olur ve doğrudan Azure VM'lerinde iki disk oluşturursanız, yalnızca üzerinde başarısız olan üç disk geri başarısız olur.
- **Failback-Linux**: Linux makinelerinin geri tepmesi için Azure'da el ile oluşturduğunuz diskler geri başarısız olur. Örneğin, üç disk üzerinde başarısız olur ve doğrudan Azure VM'lerinde iki disk oluşturursanız, beşi de başarısız olur. Failback'te veya VM'lerin yeniden korunmasında el ile oluşturulan diskleri hariç tutamazsınız.



## <a name="exclude-disks-from-replication"></a>Diskleri çoğaltmanın dışında tutma

1. Bir VMware VM için [çoğaltmayı etkinleştirdiğinizde,](site-recovery-hyper-v-site-to-azure.md) çoğaltmak istediğiniz VM'leri seçtikten sonra, **Çoğaltma** > **Özelliklerini** > **Yapılandırmayı** Etkinleştir sayfasında, **Sütunçoğaltmak için Diskleri** gözden geçirin. Varsayılan olarak tüm diskler çoğaltma için seçilir.
2. Belirli bir diski çoğaltmak istemiyorsanız, dışlamak istediğiniz disklerin seçimini **çoğaltmak** için Diskler'de. 

    ![Diskleri çoğaltmanın dışında tutma](./media/vmware-azure-exclude-disk/enable-replication-exclude-disk1.png)



## <a name="next-steps"></a>Sonraki adımlar
Dağıtımınız ayarlandıktan ve çalışmaya başladıktan sonra farklı türdeki yük devretmeler hakkında [daha fazla bilgi edinebilirsiniz](failover-failback-overview.md).
