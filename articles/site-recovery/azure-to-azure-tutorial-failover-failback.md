---
title: Azure Site Kurtarma hizmetiyle olağanüstü durum kurtarma için ikincil bir Azure bölgesine çoğaltılan Azure VM'leri üzerinde başarısız olun ve yeniden koruyun.
description: Azure Site Kurtarma hizmetiyle, olağanüstü durum kurtarma için ikincil bir Azure bölgesine çoğaltılan Azure VM'leri üzerinde nasıl başarısız olacağız ve yeniden korumayı öğrenin.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 08/05/2019
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 9bc0d25e19ad3412e62eb3386b0faf3ae5d2a444
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "68782585"
---
# <a name="fail-over-and-reprotect-azure-vms-between-regions"></a>Azure VM'leri üzerinde başarısız olun ve bölgeler arasında yeniden koruyun

Bu öğretici, [Azure Site Kurtarma](site-recovery-overview.md) hizmetiyle ikincil bir Azure bölgesine bir Azure sanal makinesi (VM) üzerinde nasıl başarısız olunan açıklanır. Başarısız olduktan sonra VM'yi yeniden korursun. Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Azure VM’ye yük devretme
> * İkincil Azure VM'yi yeniden koruyun, böylece birincil bölgeye çoğaltın.

> [!NOTE]
> Bu öğretici varsayılan ayarları ve minimum özelleştirme ile en basit yolu içerir. Daha karmaşık senaryolar için Azure VM'leri için 'Nasıl Kullanılır' altındaki makaleleri kullanın.


## <a name="prerequisites"></a>Ön koşullar

- Başlamadan önce, failover hakkında [sık sorulan soruları](site-recovery-faq.md#failover) gözden geçirin.
- Her şeyin beklenildiği gibi çalışıp çalışmadığını denetlemek için bir [olağanüstü durum kurtarma tatbikatını](azure-to-azure-tutorial-dr-drill.md) tamamladığınızdan emin olun.
- Yük devretme testini çalıştırmadan önce VM özelliklerini doğrulayın. VM, [Azure gereksinimlerine](azure-to-azure-support-matrix.md#replicated-machine-operating-systems) uymalıdır.

## <a name="run-a-failover-to-the-secondary-region"></a>İkincil bölgeye yük devretme çalıştırma

1. **Çoğaltılmış öğeler** bölümünde, yük devretmek istediğiniz VM’yi seçin > **Yük devretme**

   ![Yük devretme](./media/azure-to-azure-tutorial-failover-failback/failover.png)

2. **Failover'da,** başarısız olmak için bir **Kurtarma Noktası** seçin. Aşağıdaki seçeneklerden birini kullanabilirsiniz:

   * **En Son** (varsayılan): Site Kurtarma hizmetindeki tüm verileri işler ve en düşük Kurtarma Noktası Hedefini (RPO) sağlar.
   * **En son işlenen**: Sanal makineyi Site Kurtarma hizmeti tarafından işlenen en son kurtarma noktasına geri verir.
   * **Özel**: Belirli bir kurtarma noktasına kadar başarısız olur. Bu seçenek, bir yük devretme testi gerçekleştirmek için faydalıdır.

3. Site Kurtarma'nın başarısızlığı tetiklemeden önce kaynak VM'lerin kapatılmasını denemesini istiyorsanız, başarısız olmadan **önce kapatma makinesini** seçin. Kapatma, veri kaybı olmamasını sağlamaya yardımcı olur. Kapatma işlemi başarısız olsa bile yük devretme devam eder. Site Kurtarma, başarısız olduktan sonra kaynağı temizlemez.

4. Yük devretme ilerleme durumunu **İşler** sayfasından takip edin.

5. Yük devretmeden sonra, sanal makineyi doğrulamak için makinede oturum açın. Sanal makine için başka bir kurtarma noktasına gitmek istiyorsanız, **Kurtarma noktasını değiştir** seçeneğini kullanabilirsiniz.

6. Yük devredilmiş sanal makineden memnun kaldığınızda, yük devretmeyi **Yürütebilirsiniz**.
   Yürütme işlemi, hizmette kullanılabilir olan tüm kurtarma noktalarını siler. Artık kurtarma noktasını değiştiremezsin.

> [!NOTE]
> VM için çoğaltmayı etkinleştirdikten sonra disk eklediğiniz bir VM üzerinde başarısız olduğunuzda, çoğaltma noktaları kurtarma için kullanılabilen diskleri gösterir. Örneğin, bir VM'de tek bir disk varsa ve yeni bir disk eklerseniz, diski eklemeden önce oluşturulan çoğaltma noktaları çoğaltma noktasının "2 diskten 1'inden" oluştuğunu gösterir.

![Eklenen bir diskle başarısız](./media/azure-to-azure-tutorial-failover-failback/failover-added.png)

## <a name="reprotect-the-secondary-vm"></a>İkincil VM’yi yeniden koruma

VM’nin yük devretmesinden sonra, birincil bölgeye geri çoğaltması için VM’yi yeniden korumalısınız.

1. VM’nin **Yük devretme yürütüldü** durumunda olduğundan emin olun ve birincil bölgenin kullanılabilir olduğunu ve içinde yeni kaynaklar oluşturup bunlara erişebildiğinizi denetleyin.
2. **Vault** > **Çoğaltılan öğelerde,** başarısız olan VM'ye sağ tıklayın ve ardından Yeniden **Koruma'yı**seçin.

   ![Yeniden korumaya sağ tıklayın](./media/azure-to-azure-tutorial-failover-failback/reprotect.png)

2. Birincil bölgeye ikincil koruma yönünün zaten seçildiğini doğrulayın.
3. **Kaynak grubu, Ağ, Depolama ve Kullanılabilirlik kümeleri** bilgilerini gözden geçirin. Yeni olarak işaretlenmiş tüm kaynaklar yeniden koruma işleminin bir parçası olarak oluşturulur.
4. Yeniden koruma işini tetiklemek için **Tamam**’a tıklayın. Bu iş, hedef siteye en son verileri sağlar. Ardından, deltaları birincil bölgeye çoğaltır. VM artık korunan bir durumdadır.

## <a name="next-steps"></a>Sonraki adımlar
- Yeniden korumadan sonra, kullanılabilir olduğunda birincil bölgeye nasıl geri [dönültmeyi öğrenin.](azure-to-azure-tutorial-failback.md)
- Yeniden koruma akışı hakkında [daha fazla bilgi edinin.](azure-to-azure-how-to-reprotect.md#what-happens-during-reprotection)
