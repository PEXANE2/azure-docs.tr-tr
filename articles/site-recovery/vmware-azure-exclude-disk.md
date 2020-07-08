---
title: VMware VM disklerini olağanüstü durum kurtarma 'dan Azure 'a Azure Site Recovery hariç tutma
description: Azure Site Recovery ile VMware VM disklerini Azure 'a çoğaltmadan dışlama.
author: mayurigupta13
manager: rochakm
ms.date: 12/10/2019
ms.author: mayg
ms.topic: conceptual
ms.openlocfilehash: cd54da5ee01206e576157435135065189bfb8035
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "75495358"
---
# <a name="exclude-disks-from-vmware-vm-replication-to-azure"></a>Diskleri VMware VM çoğaltmasıyla Azure 'a dışlama

Bu makalede, olağanüstü durum kurtarma için VMware VM 'lerini Azure 'a çoğaltmakta olan disklerin nasıl hariç tutulacağı açıklanır. Birçok nedenden dolayı diskleri çoğaltmadan dışlamak isteyebilirsiniz:

- Çıkarılan disk üzerinde önemli olmayan verilerin çoğaltılmadığından emin olun.
- Çoğaltmak zorunda olmadığınız diskleri dışlayarak tüketilen çoğaltma bant genişliğini veya hedef tarafı kaynakları iyileştirin.
- İhtiyaç duymamakta olduğunuz verileri çoğaltmayana depolama ve ağ kaynaklarını kaydedin.

Diskleri çoğaltmadan hariç tutmak için:

- Diskleri dışarıda bırakma hakkında [daha fazla bilgi edinin](exclude-disks-replication.md) .
- Bir diskin çoğaltma, yük devretme ve yeniden çalışma işlemlerini nasıl etkilediğini gösteren [tipik dışlama senaryolarını](exclude-disks-replication.md#typical-scenarios) ve [örnekleri](exclude-disks-replication.md#example-1-exclude-the-sql-server-tempdb-disk) gözden geçirin.

## <a name="before-you-start"></a>Başlamadan önce

 Başlamadan önce aşağıdakilere dikkat edin:

- **Çoğaltma**: varsayılan olarak, bir makinedeki tüm diskler çoğaltılır.
- **Disk türü**: çoğaltmadaki yalnızca temel diskler dışarıda bırakılabilirler. İşletim sistemi veya dinamik diskler dışarıda tutulamaz.
- **Mobility hizmeti**: bir diski çoğaltmadan dışlamak için çoğaltmayı etkinleştirmeden önce Mobility hizmetini makineye el ile yüklemelisiniz. Bu yöntem, yalnızca çoğaltma etkinleştirildikten sonra Mobility hizmetini bir sanal makineye yüklediğinden, anında yüklemeyi kullanamazsınız.  
- **Diskleri Ekle/Kaldır/çıkar**: çoğaltmayı etkinleştirdikten sonra çoğaltma için disk ekleyemez/kaldıramaz/dışlayabilirsiniz. Disk eklemek/kaldırmak veya dışlamak istiyorsanız, makinenin korumasını devre dışı bırakmanız ve yeniden etkinleştirmeniz gerekir.
- **Yük devretme**: yük devretmenin ardından, başarısız olan uygulamaların çalışması için dışlanan disklerin olması gerekiyorsa, bu diskleri el ile oluşturmanız gerekir. Alternatif olarak, makinenin yük devretmesi sırasında diski oluşturmak için Azure Otomasyonu 'nu bir kurtarma planıyla tümleştirebilirsiniz.
- **Yeniden çalışma-Windows**: yük devretmeden sonra şirket içi sitenize geri döndüğünüzde, Azure 'da El Ile oluşturduğunuz Windows diskleri yeniden başarısız olur. Örneğin, üç disk üzerinde yük devretmek ve doğrudan Azure VM 'lerinde iki disk oluşturursanız, yalnızca yük devredilen üç disk yeniden çalışır hale getirilir.
- **Yeniden çalışma-Linux**: Linux makinelerin yeniden çalışması için Azure 'da el ile oluşturduğunuz diskler yeniden çalışır duruma getirilir. Örneğin, üç disk üzerinde yük devretmek ve doğrudan Azure VM 'lerde iki disk oluşturursanız, tüm beş yeniden başarısız olur. Yeniden çalışma sırasında veya VM 'lerin yeniden korunmasında el ile oluşturulan diskleri dışlayamazsınız.



## <a name="exclude-disks-from-replication"></a>Diskleri çoğaltmanın dışında tutma

1. Bir VMware sanal makinesi için [çoğaltmayı etkinleştirdiğinizde](site-recovery-hyper-v-site-to-azure.md) , çoğaltmak istediğiniz VM 'leri seçtikten sonra, **çoğaltma**  >  **özelliklerini**  >  **Yapılandır Özellikler** sayfasında, sütunu **çoğaltmak için diskleri** gözden geçirin. Varsayılan olarak, tüm diskler çoğaltma için seçilir.
2. Belirli bir diski çoğaltmak istemiyorsanız, **diskler** ' de dışlamak istediğiniz disklerin seçimini temizleyin. 

    ![Diskleri çoğaltmanın dışında tutma](./media/vmware-azure-exclude-disk/enable-replication-exclude-disk1.png)



## <a name="next-steps"></a>Sonraki adımlar
Dağıtımınız ayarlandıktan ve çalışmaya başladıktan sonra farklı türdeki yük devretmeler hakkında [daha fazla bilgi edinebilirsiniz](failover-failback-overview.md).
