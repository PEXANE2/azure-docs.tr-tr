---
title: Depolama Alanlarını Doğrudan Çalıştıran Azure VM'lerini Azure Site Kurtarma ile çoğaltma
description: Azure Site Kurtarma'yı kullanarak Doğrudan Depolama Alanları çalıştıran Azure VM'leri nasıl kopyalayacağınızı öğrenin.
author: sideeksh
manager: rochakm
ms.topic: how-to
ms.date: 01/29/2019
ms.openlocfilehash: 9f394fa8d618c97d74a47ff6e42a002f177cf7d9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75973660"
---
# <a name="replicate-azure-vms-running-storage-spaces-direct-to-another-region"></a>Depolama Alanlarını doğrudan başka bir bölgeye çalıştıran Azure VM'leri çoğaltma

Bu makalede, doğrudan depolama alanları çalıştıran Azure VM'lerin olağanüstü durum kurtarma etkinleştirilmesini nasıl etkinleştirebilirsiniz.

>[!NOTE]
>Yalnızca kilitlenme tutarlı kurtarma noktaları depolama alanları için doğrudan kümeleri için desteklenir.
>

[Doğrudan depolama alanları (S2D),](https://docs.microsoft.com/windows-server/storage/storage-spaces/deploy-storage-spaces-direct) Azure'da [konuk kümeleri](https://blogs.msdn.microsoft.com/clustering/2017/02/14/deploying-an-iaas-vm-guest-clusters-in-microsoft-azure) oluşturmanın bir yolunu sağlayan yazılım tanımlı depolama alanıdır.  Microsoft Azure'daki konuk kümesi, IaaS VM'lerden oluşan bir başarısız kümedir. Barındırılan VM iş yüklerinin konuk kümeleri arasında başarısız olmasına ve tek bir Azure VM'nin sağlayabileceğinden daha yüksek kullanılabilirlik sla'sı elde etmesine olanak tanır. VM'nin SQL veya ölçeklendirme dosya sunucusu gibi kritik bir uygulamayı barındırdığı senaryolarda yararlıdır.

## <a name="disaster-recovery-with-storage-spaces-direct"></a>Doğrudan depolama alanları ile olağanüstü durum kurtarma

Tipik bir senaryoda, dosya sunucusuölçek gibi uygulamanızın daha yüksek esnekliği için Azure'da sanal makineler konuk kümeniz olabilir. Bu, uygulamanızın daha yüksek kullanılabilirlik sağlamasına neden olsa da, herhangi bir bölge düzeyi hatası için Site Kurtarma'yı kullanarak bu uygulamaları korumak istersiniz. Site Kurtarma, bir bölgeden başka bir Azure bölgesine verileri çoğaltır ve başarısız olması durumunda olağanüstü durum kurtarma bölgesindeki kümeyi gündeme getirir.

Aşağıdaki diyagram, doğrudan depolama alanlarını kullanarak iki düğümlü Azure VM failover kümesini gösterir.

![depolama alanları doğrudan](./media/azure-to-azure-how-to-enable-replication-s2d-vms/storagespacedirect.png)


- Bir Windows Failover Cluster'daki iki Azure sanal makinesi ve her sanal makinede iki veya daha fazla veri diski bulunur.
- S2D veri diskindeki verileri eşitler ve senkronize depolama havuzu olarak senkronize depolama alanı sunar.
- Depolama havuzu, failover kümesine paylaşılan bir birim (CSV) olarak sunar.
- Failover kümesi veri sürücüleri için CSV kullanır.

**Olağanüstü Durum Kurtarma Hususlar**

1. Küme için [bulut tanığı](https://docs.microsoft.com/windows-server/failover-clustering/deploy-cloud-witness#CloudWitnessSetUp) ayarlarken, Olağanüstü Durum Kurtarma bölgesinde tanık olun.
2. Kaynak bölgeden farklı olan DR bölgesindeki alt ağa sanal makineler üzerinde başarısız olacaksanız, küme IP adresinin başarısız olduktan sonra değiştirilmesi gerekir.  Kümenin IP'sini değiştirmek için Site [Kurtarma kurtarma planı komut dosyasını](https://docs.microsoft.com/azure/site-recovery/site-recovery-runbook-automation) kullanmanız gerekir.</br>
[Özel komut dosyası](https://github.com/krnese/azure-quickstart-templates/blob/master/asr-automation-recovery/scripts/ASR-Wordpress-ChangeMysqlConfig.ps1) uzantısını kullanarak VM içindeki komutu çalıştırmak için örnek komut 

### <a name="enabling-site-recovery-for-s2d-cluster"></a>S2D kümesi için Site Kurtarma'yı etkinleştirme:

1. Kurtarma hizmetleri kasasının içinde "+çoğaltma" seçeneğini tıklayın
1. Kümedeki tüm düğümleri seçin ve [bunları Multi-VM tutarlılık grubunun](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-common-questions#multi-vm-consistency) bir parçası yapın
1. Uygulama tutarlılığı kapalı* olan çoğaltma ilkesini seçin (yalnızca kilitlenme tutarlılığı desteği kullanılabilir)
1. Çoğaltmayı etkinleştirme

   ![storagespacesdirect koruma](./media/azure-to-azure-how-to-enable-replication-s2d-vms/multivmgroup.png)

2. Çoğaltılan öğelere gidin ve hem sanal makine durumunu görebilirsiniz.
3. Her iki sanal makine de korunuyor ve aynı zamanda çoklu VM tutarlılık grubunun bir parçası olarak gösteriliyor.

   ![storagespacesdirect koruma](./media/azure-to-azure-how-to-enable-replication-s2d-vms/storagespacesdirectgroup.PNG)

## <a name="creating-a-recovery-plan"></a>Kurtarma planı oluşturma
Kurtarma planı, bir hata sırasında çok katmanlı bir uygulamada çeşitli katmanların sırasını destekler. Sıralama, uygulama tutarlılığını korumaya yardımcı olur. Çok katmanlı bir web uygulaması için bir kurtarma planı oluşturduğunuzda, [Site Kurtarma'yı kullanarak kurtarma planı oluştur'da](site-recovery-create-recovery-plans.md)açıklanan adımları tamamlayın.

### <a name="adding-virtual-machines-to-failover-groups"></a>Başarısız gruplara sanal makineler ekleme

1.  Sanal makineleri ekleyerek bir kurtarma planı oluşturun.
2.  VM'leri gruplandırmak için 'Özelleştir'e tıklayın. Varsayılan olarak, tüm VM'ler 'Grup 1'in bir parçasıdır.


### <a name="add-scripts-to-the-recovery-plan"></a>Kurtarma planına komut dosyaları ekleme
Uygulamalarınızın düzgün çalışması için, başarısız olduktan sonra veya bir test başarısızolması sırasında Azure sanal makinelerinde bazı işlemler yapmanız gerekebilir. Bazı başarısız lık sonrası işlemleri otomatikleştirebilirsiniz. Örneğin, burada yük dengeleyicisi ve küme IP değişiyor.


### <a name="failover-of-the-virtual-machines"></a>Sanal makinelerin failover 
VM'lerin her iki düğümünün de Site Kurtarma [kurtarma planını](https://docs.microsoft.com/azure/site-recovery/site-recovery-create-recovery-plans) kullanarak başarısız olması gerekir 

![storagespacesdirect koruma](./media/azure-to-azure-how-to-enable-replication-s2d-vms/recoveryplan.PNG)

## <a name="run-a-test-failover"></a>Yük devretme testi çalıştırma
1.  Azure portalında Kurtarma Hizmetleri kasanızı seçin.
2.  Oluşturduğunuz kurtarma planını seçin.
3.  **Yük Devretme Testi**'ni seçin.
4.  Test başarısız lık işlemini başlatmak için kurtarma noktasını ve Azure sanal ağını seçin.
5.  İkincil ortam dolduğunda, doğrulamaları gerçekleştirin.
6.  Doğrulamalar tamamlandığında, başarısız ortamı temizlemek için **Temizleme testi failover'ı**seçin.

Daha fazla bilgi için [bkz.](site-recovery-test-failover-to-azure.md)

## <a name="run-a-failover"></a>Yük devretme çalıştırma

1.  Azure portalında Kurtarma Hizmetleri kasanızı seçin.
2.  SAP uygulamaları için oluşturduğunuz kurtarma planını seçin.
3.  **Yük devretme**'yi seçin.
4.  Başarısız işlemi başlatmak için kurtarma noktasını seçin.

Daha fazla bilgi için Site [Kurtarma'da Failover'a](site-recovery-failover.md)bakın.
## <a name="next-steps"></a>Sonraki adımlar

Failback çalıştırma hakkında [daha fazla bilgi edinin.](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-tutorial-failover-failback)
