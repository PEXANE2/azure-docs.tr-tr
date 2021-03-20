---
title: Azure Site Recovery Azure VM olağanüstü durum kurtarma detayına yönelik öğretici
description: Bu öğreticide, Site Recovery kullanarak başka bir bölgeye bir Azure VM olağanüstü durum kurtarma ayrıntısı çalıştırın.
services: site-recovery
ms.topic: tutorial
ms.date: 11/05/2020
ms.custom: mvc
ms.openlocfilehash: c7cd1898f27f3b7255009efb40f6bcc8938dbf9e
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "93395659"
---
# <a name="tutorial-run-a-disaster-recovery-drill-for-azure-vms"></a>Öğretici: Azure VM 'Leri için olağanüstü durum kurtarma ayrıntısı çalıştırma

[Azure Site Recovery](site-recovery-overview.md)Ile çoğaltılan Azure VM 'leri için başka bir Azure bölgesine olağanüstü durum kurtarma detayının nasıl çalıştırılacağını öğrenin. Bu makalede şunları yapacaksınız:

> [!div class="checklist"]
> * Önkoşulları doğrulama
> * Ayrıntıdan önce VM ayarlarını denetleyin
> * Yük devretme testi çalıştırma
> * Ayrıntıdan sonra temizle


> [!NOTE]
> Bu öğretici, olağanüstü durum kurtarma detaylarını çalıştırmak için en az adım sağlar. Tam altyapı testi ile detaya gitme çalıştırmak istiyorsanız, Azure VM [ağı](azure-to-azure-about-networking.md), [Otomasyonu](azure-to-azure-powershell.md)ve [sorun giderme](azure-to-azure-troubleshoot-errors.md)hakkında bilgi edinin.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiye başlamadan önce bir veya daha fazla Azure VM için olağanüstü durum kurtarmayı etkinleştirmeniz gerekir. Bunu yapmak için, bu serideki [ilk öğreticiyi doldurun](azure-to-azure-tutorial-enable-replication.md) .

## <a name="verify-vm-settings"></a>VM ayarlarını doğrulama

1. Kasada **çoğaltılan öğeleri**> VM 'yi seçin.

    ![VM özelliklerinde olağanüstü durum kurtarma sayfasını açma seçeneği](./media/azure-to-azure-tutorial-dr-drill/vm-settings.png)

2. **Genel bakış** sayfasında, sanal makinenin korumalı ve sağlıklı olduğunu denetleyin.
3. Yük devretme testi çalıştırdığınızda hedef bölgede bir Azure sanal ağı seçersiniz. Yük devretme işleminden sonra oluşturulan Azure VM bu ağa yerleştirilir. 

    - Bu öğreticide, yük devretme testini çalıştırdığımızda var olan bir ağı seçtik.
    - IP adreslerinin ve ağ bileşenlerinin üretim ağlarında kullanılabilmesini sağlamak için detaya yönelik üretim dışı bir ağ seçmenizi öneririz.
   - Ayrıca yük devretme testi için kullanılacak ağ ayarlarını önceden yapılandırabilirsiniz. Her NIC için atayabileceğiniz ayrıntılı ayarlar arasında alt ağ, özel IP adresi, genel IP adresi, yük dengeleyici ve ağ güvenlik grubu bulunur. Burada bu yöntemi kullanmıyoruz, ancak daha fazla bilgi edinmek için [Bu makaleyi gözden](azure-to-azure-customize-networking.md#customize-failover-and-test-failover-networking-configurations) geçirebilirsiniz.


## <a name="run-a-test-failover"></a>Yük devretme testi çalıştırma


1. **Genel bakış** sayfasında **Yük devretmeyi sına**' yı seçin.

    
    ![Çoğaltılan öğe için test yük devretmesi düğmesi](./media/azure-to-azure-tutorial-dr-drill/test-failover-button.png)

2. **Yük devretme testi** bölümünde, bir kurtarma noktası seçin. Hedef bölgedeki Azure VM, bu kurtarma noktasındaki veriler kullanılarak oluşturulur.
  
   - **En son işlenen**: Site Recovery tarafından işlenen en son kurtarma noktasını kullanır. Zaman damgası gösterilir. Verileri işlemek için zaman harcanmadan, düşük bir kurtarma süresi hedefi (RTO) sağlar.
   -  **En son**: Site Recovery gönderilen tüm verileri, yük devretmadan önce her VM için bir kurtarma noktası oluşturmak üzere işler. , Yük devretme tetiklendiğinde tüm veriler Site Recovery çoğaltıldığından, en düşük kurtarma noktası hedefini (RPO) sağlar.
   - **En son uygulamayla tutarlı**: Bu seçenek VM 'lerin üzerinde en son uygulamayla tutarlı kurtarma noktasına devredildi. Zaman damgası gösterilir.
   - **Özel**: yük devretme, belirli bir kurtarma noktasına devredildi. Özel yalnızca tek bir VM 'nin yükünü devretmek ve kurtarma planı kullanmıyorsanız kullanılabilir.

3. **Azure sanal ağ**' da, yük devretmeden sonra oluşturulan Azure VM 'lerinin yerleştirileceği hedef ağı seçin. Mümkünse, çoğaltmayı etkinleştirdiğinizde oluşturulan ağı değil, bir üretim dışı ağ seçin.

    ![Test yük devretmesi ayarları sayfası](./media/azure-to-azure-tutorial-dr-drill/test-failover-settings.png)    

4. Yük devretmeyi başlatmak için **Tamam**' ı seçin.
5. Bildirimlerde yük devretme testini izleyin.

    ![İlerleme durumu bildirimi ](./media/azure-to-azure-tutorial-dr-drill/notification-start-test-failover.png) ![ başarı bildirimi](./media/azure-to-azure-tutorial-dr-drill/notification-finish-test-failover.png)     


5. Yük devretme bittikten sonra, hedef bölgede oluşturulan Azure VM Azure portal **sanal makinelerde** görüntülenir. VM 'nin çalıştığından, uygun boyutta olduğundan ve seçtiğiniz ağa bağlı olduğundan emin olun.

## <a name="clean-up-resources"></a>Kaynakları temizleme

1. **Essentials** sayfasında, **Yük devretme testini temizle**' yi seçin.

    ![Temizleme işlemini başlatmak için düğme](./media/azure-to-azure-tutorial-dr-drill/select-cleanup.png)

2. **Yük devretme sınamasını Temizleme**  >  **notları**' nda, test yük devretmesi ile ilişkili tüm gözlemlerinizi kaydedin ve kaydedin. 
3. Yük devretme testi sırasında oluşturulan sanal makineleri silmek için **test etme tamam** ' ı seçin.

    ![Temizleme seçenekleriyle sayfa](./media/azure-to-azure-tutorial-dr-drill/cleanup-failover.png)

4. Bildirimlerde Temizleme ilerlemesini izleyin.

    ![Temizleme ilerleme durumu bildirimi ](./media/azure-to-azure-tutorial-dr-drill/notification-start-cleanup.png) ![ Temizleme başarılı bildirimi](./media/azure-to-azure-tutorial-dr-drill/notification-finish-cleanup.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, yük devretmenin beklendiği gibi çalıştığını denetlemek için bir olağanüstü durum kurtarma detayına sahip olursunuz. Artık tam yük devretmeyi deneyebilirsiniz.

> [!div class="nextstepaction"]
> [Üretim yük devretmesini çalıştırma](azure-to-azure-tutorial-failover-failback.md)
