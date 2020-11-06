---
title: Azure Site Recovery ile olağanüstü durum kurtarma sırasında Azure VM 'Leri bir birincil bölgeye geri dönme öğreticisi.
description: Azure Site Recovery ile Azure VM 'lerini birincil bölgeye geri alma hakkında bilgi edinmek için öğretici.
ms.topic: tutorial
ms.date: 11/05/2020
ms.custom: mvc
ms.openlocfilehash: 5c127010a7988bf08c77340a4fc10bb32dc76f87
ms.sourcegitcommit: 0ce1ccdb34ad60321a647c691b0cff3b9d7a39c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2020
ms.locfileid: "93393943"
---
# <a name="tutorial-fail-back-azure-vm-to-the-primary-region"></a>Öğretici: Azure VM 'yi birincil bölgeye geri dönme

Azure VM 'nin yükünü ikincil bir Azure bölgesine devretmek için, [Azure Site Recovery](site-recovery-overview.md)kullanarak VM 'Yi birincil Azure bölgesine devretmek için bu öğreticiyi izleyin.  Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> 
> * Önkoşulları gözden geçirin.
> * İkincil bölgedeki VM 'yi yeniden devreder.
> * Birincil VM 'Leri ikincil bölgeye yeniden koruyun.
> 
> [!NOTE]
> Bu öğreticide, en az adımla nasıl geri dönme yapılacağı gösterilmektedir. Tam ayarlarla yük devretme çalıştırmak istiyorsanız, Azure VM [ağı](azure-to-azure-about-networking.md), [Otomasyonu](azure-to-azure-powershell.md)ve [sorun giderme](azure-to-azure-troubleshoot-errors.md)hakkında bilgi edinin.



## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiye başlamadan önce şunları yapmalısınız:

1. En az bir Azure VM için [çoğaltmayı ayarlayın](azure-to-azure-tutorial-enable-replication.md) ve BT için bir [olağanüstü durum kurtarma detayına](azure-to-azure-tutorial-dr-drill.md) göz atın.
2. Birincil bölgeden ikincil bölgeye [yük devri](azure-to-azure-tutorial-failover-failback.md) yapın ve ikincil bölgeden birinciye çoğaltılabilmesi için yeniden korumalı hale gelir. 
3. Birincil bölgenin kullanılabilir olup olmadığını ve içinde yeni kaynaklar oluşturup bu kaynaklara erişebiliyor olduğunuzdan emin olun.

## <a name="fail-back-to-the-primary-region"></a>Birincil bölgeye geri dönme

VM 'Ler yeniden korunduktan sonra, gerekirse birincil bölgeye yeniden yük devreedebilirsiniz.

1. Kasada **çoğaltılan öğeleri** > VM 'yi seçin.

2. VM Genel Bakış sayfasında, yük devretme çalıştırmadan önce VM 'nin sağlıklı olduğunu ve Eşitlemenin tamamlandığını denetleyin. VM *korumalı* durumda olmalıdır.

    ![VM 'yi korumalı durumda gösteren VM 'ye Genel Bakış sayfası](./media/azure-to-azure-tutorial-failback/protected-state.png)

3. Genel Bakış sayfasında, **Yük devretme** ' yı seçin. Bu süre için yük devretme testi yapmadığımızdan, doğrulamanız istenir.

    [Yük devretme testi olmadan yük devretmeyi çalıştırmayı kabul ettiğimiz sayfayı gösteren sayfa](./media/azure-to-azure-tutorial-failback/no-test.png)

4. **Yük devretme** bölümünde ikinciye kadar olan yönü ve bir kurtarma noktası seçin. Hedefteki (birincil bölgedeki) Azure VM, bu noktadan alınan veriler kullanılarak oluşturulur.
   - **En son işlenen** : Site Recovery tarafından işlenen en son kurtarma noktasını kullanır. Zaman damgası gösterilir. Verileri işlemek için zaman harcanmadan, düşük bir kurtarma süresi hedefi (RTO) sağlar.
   -  **En son** : Site Recovery gönderilen tüm verileri, yük devretmadan önce her VM için bir kurtarma noktası oluşturmak üzere işler. , Yük devretme tetiklendiğinde tüm veriler Site Recovery çoğaltıldığından, en düşük kurtarma noktası hedefini (RPO) sağlar.
   - **En son uygulamayla tutarlı** : Bu seçenek VM 'lerin üzerinde en son uygulamayla tutarlı kurtarma noktasına devredildi. Zaman damgası gösterilir.
   - **Özel** : yük devretme, belirli bir kurtarma noktasına devredildi. Özel yalnızca tek bir VM 'nin yükünü devretmek ve kurtarma planı kullanmıyorsanız kullanılabilir.

    > [!NOTE]
    > VM için çoğaltmayı etkinleştirdikten sonra disk eklediğiniz bir VM 'nin yükünü devretmek, çoğaltma noktaları kurtarma için kullanılabilir diskleri gösterir. Örneğin, ikinci bir disk eklenmeden önce oluşturulan bir çoğaltma noktası "1/2 disk" olarak görünür.

4. Yük devretmeye başlamadan önce Site Recovery kaynak VM 'Leri kapatmayı denemek istiyorsanız, **yük devretmeye başlamadan önce makineyi Kapat ' ı** seçin. Kapalı, veri kaybı olmamasını sağlamaya yardımcı olur. Kapatma işlemi başarısız olsa bile yük devretme devam eder. 

    ![Yük devretme ayarları sayfası](./media/azure-to-azure-tutorial-failback/failover.png)    

3. Yük devretmeyi başlatmak için **Tamam** ' ı seçin.
4. Bildirimlerde yük devretmeyi izleyin.

    ![Yük devretme ilerleme durumu bildirimi](./media/azure-to-azure-tutorial-failback/notification-progress.png)  
    ![Yük devretme başarısı için bildirim](./media/azure-to-azure-tutorial-failback/notification-success.png)   

## <a name="reprotect-vms"></a>Sanal makineleri yeniden koruma

VM 'Leri birincil bölgeye geri başlattıktan sonra, ikincil bölgeye yeniden çoğaltılmaya başlayabilmeleri için bunları yeniden korumanız gerekir.

1. VM 'nin **genel bakış** sayfasında **yeniden koru** ' yı seçin.

    ![Birincil bölgeden yeniden koruma düğmesi](./media/azure-to-azure-tutorial-failback/reprotect.png)  

2. Birincil bölgenin hedef ayarlarını gözden geçirin. Yeni olarak işaretlenen kaynaklar, yeniden koruma işleminin bir parçası olarak Site Recovery tarafından oluşturulur.
3. Yeniden koru işlemini başlatmak için **Tamam ' ı** seçin. İşlem, ilk verileri hedef konuma gönderir ve ardından sanal makinelerin Delta bilgilerini hedefe çoğaltır.

     ![Çoğaltma ayarlarını gösteren sayfa](./media/azure-to-azure-tutorial-failback/replication-settings.png) 

4. Bildirimler içinde ilerlemeyi yeniden koruma. 

    ![İlerleme bildirimini yeniden koru ](./media/azure-to-azure-tutorial-failback/notification-reprotect-start.png) [ilerleme durumu bildirimi](./media/azure-to-azure-tutorial-failback/notification-reprotect-finish.png)
    
  

## <a name="clean-up-resources"></a>Kaynakları temizleme

Yönetilen diskleri olan VM 'Ler için, yeniden çalışma tamamlandıktan sonra ve VM birinciden ikinciye çoğaltma için yeniden korunduktan sonra, Site Recovery ikincil olağanüstü durum kurtarma bölgesindeki makineleri otomatik olarak temizler. İkincil bölgedeki VM 'Leri ve NIC 'leri el ile silmeniz gerekmez. Yönetilmeyen disklere sahip VM 'Ler temizlenmez.

Çoğaltmayı geri aldıktan sonra tamamen devre dışı bırakırsanız Site Recovery, tarafından korunan makineleri temizler. Bu durumda, yönetilen diskleri kullanmayan VM 'Ler için diskleri de temizler. 
 
## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, VM 'Leri ikincil bölgeden birincili geri yüklemeye başarısız olursunuz. Bu, bir VM için çoğaltmayı etkinleştirmeyi, olağanüstü durum kurtarma detaylarını denemeyi, birincil bölgeden ikinciye yük devretmesini ve son olarak geri dönmesi içeren işlemin son adımıdır.

> [!div class="nextstepaction"]
> Şimdi, Şirket [içi BIR VM](vmware-azure-tutorial-prepare-on-premises.md) için Azure 'da olağanüstü durum kurtarmayı deneyin

