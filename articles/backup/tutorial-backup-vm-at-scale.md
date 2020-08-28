---
title: Öğretici-birden çok Azure sanal makinesini yedekleme
description: Bu öğreticide, bir kurtarma hizmetleri Kasası oluşturmayı, bir yedekleme ilkesi tanımlamanızı ve aynı anda birden çok sanal makineyi nasıl yedekleyeceğinizi öğrenin.
ms.date: 07/26/2020
ms.topic: tutorial
ms.custom: mvc
ms.openlocfilehash: 6556731ed18bd591e930ed89264cd62893fe9ac6
ms.sourcegitcommit: 419cf179f9597936378ed5098ef77437dbf16295
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89022063"
---
# <a name="use-azure-portal-to-back-up-multiple-virtual-machines"></a>Birden çok sanal makineyi yedeklemek için Azure portalını kullanma

Azure’da verileri yedeklediğinizde söz konusu veriler Kurtarma Hizmetleri kasası adında bir Azure kaynağında depolanır. Kurtarma Hizmetleri kasası kaynağı, Azure hizmetlerinin birçoğunun Ayarlar menüsünden kullanılabilir. Kurtarma Hizmetleri kasasının çoğu Azure hizmeti 'nin ayarlar menüsüyle tümleşik olmasının avantajı, verilerin yedeklenmesi kolaylığıdır. Ancak, işinizde her bir veritabanı veya sanal makine ile ayrı olarak çalışmak sıkıcı olur. Tüm sanal makinelere ait verileri tek bir departmanda veya tek bir konumda yedeklemek istediğinizde ne yapabilirsiniz? Bir yedekleme ilkesi oluşturarak ve bu ilkeyi istenen sanal makinelere uygulayarak birden çok sanal makineyi yedeklemek kolaydır. Bu öğreticide, aşağıdaki işlemlerin nasıl yapılacağı açıklanmaktadır:

> [!div class="checklist"]
>
> * Kurtarma Hizmetleri kasası oluşturma
> * Yedekleme ilkesi tanımlama
> * Birden çok sanal makineyi korumak için yedekleme ilkesini uygulama
> * Korumalı sanal makineler için isteğe bağlı bir yedekleme işi tetikleme

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure portalında](https://portal.azure.com/) oturum açın.

## <a name="create-a-recovery-services-vault"></a>Kurtarma Hizmetleri kasası oluşturma

Kurtarma Hizmetleri kasası, yedekleme verilerini ve korumalı sanal makinelere uygulanan yedekleme ilkesini içerir. Sanal makineleri yedekleme işlemi, yerel bir işlemdir. Bir sanal makineyi başka bir konumdaki kurtarma hizmetleri kasasından bir konumdan yedekleyemiyoruz. Bu nedenle, yedeklenecek sanal makineler içeren her Azure konumu için, söz konusu konumda en az bir Kurtarma Hizmetleri kasası mevcut olmalıdır.

1. Sol taraftaki menüden **tüm hizmetler**' i seçin.

    ![Tüm hizmetleri seçin](./media/tutorial-backup-vm-at-scale/click-all-services.png)

1. **Tüm hizmetler** Iletişim kutusunda *Kurtarma Hizmetleri*' ni girin. Giriş listenize göre filtrelerin kaynak listesi. Kaynak listesinde, **Kurtarma Hizmetleri kasaları**' nı seçin.

    ![Kurtarma Hizmetleri kasalarını girin ve seçin](./media/tutorial-backup-vm-at-scale/all-services.png)

    Abonelikteki kurtarma hizmetleri kasalarının listesi görüntülenir.

1. **Kurtarma Hizmetleri kasaları** panosunda **Ekle**' yi seçin.

    ![Kurtarma Hizmetleri Kasası ekleme](./media/tutorial-backup-vm-at-scale/add-button-create-vault.png)

1. Kurtarma Hizmetleri kasası menüsünde,

    * **Ad** alanına *myRecoveryServicesVault* yazın.
    * **Abonelik** bölümünde geçerli abonelik kimliği görüntülenir. Ek abonelikleriniz varsa yeni kasa için başka bir abonelik seçebilirsiniz.
    * **Kaynak grubu**için **var olanı kullan** ' ı seçin ve *myresourcegroup*öğesini seçin. *myResourceGroup* yoksa **Yeni oluştur**’u seçin ve *myResourceGroup* yazın.
    * **Konum** açılan menüsünden *Batı Avrupa*’yı seçin.

    ![Kurtarma Hizmetleri Kasası değerleri](./media/tutorial-backup-vm-at-scale/review-and-create.png)

    Kurtarma Hizmetleri kasası, korunan sanal makinelerle aynı konumda olmalıdır. Birden çok bölgede sanal makineniz varsa her bölgede bir Kurtarma Hizmetleri kasası oluşturun. Bu öğretici, *Myvm* (hızlı başlangıç ile oluşturulan sanal makine) oluşturulduğu için *Batı Avrupa* bir kurtarma hizmetleri Kasası oluşturur.

1. Kurtarma Hizmetleri kasasını oluşturmaya hazırsanız **Oluştur**' u seçin.

    ![Kurtarma Hizmetleri kasasını oluşturma](./media/tutorial-backup-vm-at-scale/click-create-button.png)

1. Kurtarma Hizmetleri kasasının oluşturulması biraz zaman alabilir. Portalın sağ üst köşesindeki **Bildirimler** alanında durum bildirimlerini izleyin. Kasanızın oluşturulduktan sonra kurtarma hizmetleri kasaları listesinde görünür. Kasanızı görmüyorsanız **Yenile**' yi seçin.

     ![Yedekleme kasaları listesini yenileme](./media/tutorial-backup-vm-at-scale/refresh-button.png)

Kurtarma Hizmetleri kasası oluşturduğunuzda kasa varsayılan olarak coğrafi olarak yedekli depolama işlevine sahip olur. Veri esnekliği sağlamak için coğrafi olarak yedekli depolama, verileri iki Azure bölgesi arasında birden çok kez çoğaltır.

## <a name="set-backup-policy-to-protect-vms"></a>VM’leri korumak için yedekleme ilkesi ayarlama

Kurtarma Hizmetleri kasasını oluşturduktan sonraki adım, veri türü için kasanın yapılandırılması ve yedekleme ilkesinin ayarlanmasıdır. Yedekleme ilkesi, kurtarma noktalarının ne sıklıkta ve ne zaman alınacağına yönelik zamanlamadır. İlke aynı zamanda kurtarma noktaları için bekletme aralığını içerir. Bu öğreticide, işletmenizin bir otel, Stadium ve restoran ve concessions ile karmaşık bir spor olduğunu ve sanal makinelerdeki verileri korudığınızı varsayalım. Aşağıdaki adımlarda, finansal veriler için bir yedekleme ilkesi oluşturulmaktadır.

1. Kurtarma Hizmetleri kasalarının listesinden, ilgili panoyu seçmek için **myRecoveryServicesVault** seçeneğini belirleyin.

   ![Senaryo menüsünü açma](./media/tutorial-backup-vm-at-scale/open-vault-from-list.png)

1. Kasa panosu **menüsünde yedekle ' yi seçerek yedekleme** menüsünü açın.

1. Yedekleme Hedefi menüsünde, **İş yükünüz nerede çalışıyor?** açılan menüsünden *Azure*’ı seçin. **Ne yedeklemek** istiyorsunuz açılır listesinden *sanal makine*' yi seçin ve ardından **Yedekle**' yi seçin.

    Bu eylemler, Kurtarma Hizmetleri kasasını bir sanal makine ile etkileşime geçmesi için hazırlar. Kurtarma Hizmetleri kasaları her gün bir geri yükleme noktası oluşturan ve geri yükleme noktalarını 30 gün boyunca tutan, varsayılan bir ilkeye sahiptir.

    ![Yedekleme hedefi](./media/tutorial-backup-vm-at-scale/backup-goal.png)

1. Yeni bir ilke oluşturmak için yedekleme ilkesi menüsünde, **yedekleme Ilkesi Seç** açılan menüsünde *Yeni ilke oluştur*' u seçin.

    ![Yeni ilke oluştur](./media/tutorial-backup-vm-at-scale/create-new-policy.png)

1. **Yedekleme ilkesi** bölmesi açılır. Aşağıdaki ayrıntıları doldurun:
   * **Ilke adı** türü *finans*için. Yedekleme ilkesi için aşağıdaki değişiklikleri girin:
   * **Yedekleme sıklığı** için saat dilimini *Orta Amerika Saati* olarak ayarlayın. Spor tesisi Teksas’ta olduğundan, tesisin sahibi zamanlamanın yerel olarak ayarlanmasını istemektedir. Yedekleme sıklığını Günlük olarak saat 03:30'a ayarlanmış halde bırakın.
   * **Günlük yedekleme noktası bekletmesi** için süreyi 90 gün olarak ayarlayın.
   * **Haftalık yedekleme noktası bekletmesi** için *Pazartesi* geri yükleme noktasını kullanın ve 52 hafta boyunca tutun.
   * **Aylık yedekleme noktası bekletmesi** için her ayın İlk Pazar günündeki geri yükleme noktasını kullanın ve 36 ay boyunca tutun.
   * **Yıllık yedekleme noktası bekletmesi** seçeneğinin işaretini kaldırın. Finans lideri, verileri 36 aydan uzun süre tutmak istememektedir.
   * Yedekleme ilkesini oluşturmak için **Tamam ' ı** seçin.

     ![Yedekleme ilkesi ayarları](./media/tutorial-backup-vm-at-scale/set-new-policy.png)

     Yedekleme ilkesini oluşturduktan sonra ilkeyi sanal makineler ile ilişkilendirin.

1. **Sanal makineler**altında **Ekle**' yi seçin.

     ![Sanal makine Ekle](./media/tutorial-backup-vm-at-scale/add-virtual-machines.png)

1. **Sanal makineler Seç** bölmesi açılır. Yedekleme ilkesini sanal makinelere dağıtmak için *Myvm* ' yi seçin ve **Tamam** ' ı seçin.

    Aynı konumda olan ve henüz bir yedekleme ilkesiyle ilişkili olmayan tüm sanal makineler görünür. *myVMH1* ve *myVMR1* seçilerek *Finans* ilkesiyle ilişkilendirilir.

    ![Korunacak VM 'Leri seçin](./media/tutorial-backup-vm-at-scale/choose-vm-to-protect.png)

1. Sanal makineler seçildikten sonra **yedeklemeyi etkinleştir**' i seçin.

    Dağıtım tamamlandığında, dağıtımın başarıyla tamamlandığını belirten bir bildirim alırsınız.

## <a name="initial-backup"></a>İlk yedekleme

Kurtarma Hizmetleri kasaları için yedeklemeyi etkinleştirdiniz, ancak bir ilk yedekleme oluşturulmadı. Verilerinizin korunması için ilk yedeklemenin tetiklenmesi için bir olağanüstü durum kurtarma en iyi uygulamadır.

İsteğe bağlı yedekleme işi çalıştırmak için:

1. Kasa panosunda, yedekleme öğeleri menüsünü açmak için **yedekleme öğeleri**altında **3** ' ü seçin.

    ![Yedekleme öğeleri](./media/tutorial-backup-vm-at-scale/tutorial-vm-back-up-now.png)

    **Yedekleme Öğeleri** menüsü açılır.

1. **Yedekleme öğeleri** menüsünde **Azure sanal makine** ' yi seçerek kasayla ilişkili sanal makinelerin listesini açın.

    ![Sanal makinelerin listesi](./media/tutorial-backup-vm-at-scale/three-virtual-machines.png)

1. **Yedekleme Öğeleri** listesi açılır.

    ![Tetiklenmiş yedekleme işi](./media/tutorial-backup-vm-at-scale/initial-backup-context-menu.png)

1. **Yedekleme öğeleri** listesinde, bağlam menüsünü açmak için üç nokta **...** simgesini seçin.

1. Bağlam menüsünde **Şimdi yedekle** seçeneğine tıklayın.

    ![Bağlam menüsü-Şimdi yedekle ' yi seçin](./media/tutorial-backup-vm-at-scale/context-menu.png)

    Şimdi Yedekle menüsü açılır.

1. Şimdi Yedekle menüsünde, kurtarma noktasının saklanacağı son günü girin ve **Tamam**' ı seçin.

    ![Yedeklemenin şimdi kurtarma noktasının korunduğu son günü ayarla](./media/tutorial-backup-vm-at-scale/backup-now-short.png)

    Dağıtım bildirimleri, yedekleme işinin tetiklendiğini ve Yedekleme işleri sayfasında işin ilerleme durumunu izleyebileceğinizi bilmenizi sağlar. Sanal makinenizin boyutuna bağlı olarak, ilk yedeklemenin oluşturulması biraz zaman alabilir.

    İlk yedekleme işi tamamlandığında Yedekleme işi menüsünde işin durumunu görebilirsiniz. İsteğe bağlı yedekleme işi, *myVM* için ilk yükleme noktasını oluşturmuştur. Diğer sanal makineleri yedeklemek istiyorsanız, her sanal makine için bu adımları yineleyin.

    ![Yedekleme İşleri kutucuğu](./media/tutorial-backup-vm-at-scale/initial-backup-complete.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Sonraki öğreticilerle çalışmaya devam etmeyi planlıyorsanız, bu öğreticide oluşturulan kaynakları temizlemeyin. Devam etmeyi planlamıyorsanız, Azure portal Bu öğretici tarafından oluşturulan tüm kaynakları silmek için aşağıdaki adımları kullanın.

1. Yedekleme **öğeleri menüsünü açmak Için** **Myrecoveryserviceskasa** panosunda **3** ' ü seçin.

    ![Yedekleme öğeleri menüsünü aç](./media/tutorial-backup-vm-at-scale/tutorial-vm-back-up-now.png)

1. **Yedekleme öğeleri** menüsünde **Azure sanal makine** ' yi seçerek kasayla ilişkili sanal makinelerin listesini açın.

    ![Sanal makinelerin listesi](./media/tutorial-backup-vm-at-scale/three-virtual-machines.png)

    **Yedekleme Öğeleri** listesi açılır.

1. **Yedekleme öğeleri** menüsünde, bağlam menüsünü açmak için üç nokta simgesini seçin.

    ![Yedekleme öğeleri menüsünde bağlam menüsünü açın](./media/tutorial-backup-vm-at-scale/context-menu-to-delete-vm.png)

1. Bağlam menüsünde yedeklemeyi **Durdur** ' u seçerek Yedeklemeyi Durdur menüsünü açın.

    ![Yedekleme menüsünü durdur](./media/tutorial-backup-vm-at-scale/context-menu-for-delete.png)

1. **Yedeklemeyi Durdur** menüsünde, üst taraftaki açılan menüyü seçip **Yedekleme Verilerini Sil** seçeneğini belirleyin.

1. **Yedekleme öğesinin adını yazın** iletişim kutusuna *myVM* yazın.

1. Yedekleme öğesi doğrulandıktan sonra (bir onay işareti görünür), **Yedeklemeyi Durdur** düğmesi etkin olur. İlkeyi durdurmak ve geri yükleme noktalarını silmek için **Yedeklemeyi Durdur** ' u seçin.

    ![Kasayı silmek için Yedeklemeyi Durdur ' u seçin](./media/tutorial-backup-vm-at-scale/provide-reason-for-delete.png)

    >[!NOTE]
    >Silinen öğeler, 14 gün boyunca geçici silme durumunda tutulur. Yalnızca bu dönemden sonra kasa silinebilir. Daha fazla bilgi için bkz. [Azure Backup Recovery Services kasasını silme](backup-azure-delete-vault.md).

1. Kasada daha fazla öğe yoksa **Sil**' i seçin.

    ![Sil ' i seçin](./media/tutorial-backup-vm-at-scale/deleting-the-vault.png)

    Kasa silindikten sonra kurtarma hizmetleri kasaları listesine geri dönersiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure portalı kullanarak şu işlemleri gerçekleştirdiniz:

> [!div class="checklist"]
>
> * Kurtarma Hizmetleri kasası oluşturma
> * Kasayı sanal makineleri koruyacak şekilde ayarlama
> * Özel yedekleme ve bekletme ilkesi oluşturma
> * Birden çok sanal makineyi korumak için ilkeyi atama
> * Sanal makineler için isteğe bağlı bir yedeklemeyi tetikleme

Bir Azure sanal makinesini diskten geri yüklemek için sonraki öğreticiye devam edin.

> [!div class="nextstepaction"]
> [CLI kullanarak VM’leri geri yükleme](./tutorial-restore-disk.md)
