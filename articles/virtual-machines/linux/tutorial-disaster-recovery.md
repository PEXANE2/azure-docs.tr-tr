---
title: Öğretici-Azure Site Recovery Linux VM 'Leri için olağanüstü durum kurtarmayı ayarlama
description: Linux sanal makineleri için Azure Site Recovery hizmetini kullanarak farklı bir Azure bölgesine olağanüstü durum kurtarmayı ayarlamayı öğrenin.
author: rayne-wiselman
ms.service: virtual-machines
ms.collection: linux
ms.subservice: recovery
ms.topic: tutorial
ms.date: 11/05/2020
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: fa43f40d4849a8e773241fa17a1e1787ce86a8ff
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2021
ms.locfileid: "102564756"
---
# <a name="tutorial-set-up-disaster-recovery-for-linux-virtual-machines"></a>Öğretici: Linux sanal makineleri için olağanüstü durum kurtarmayı ayarlama


Bu öğreticide, Linux çalıştıran Azure VM 'Leri için olağanüstü durum kurtarmayı ayarlama gösterilmektedir. Bu makalede şunları yapmayı öğreneceksiniz:

> [!div class="checklist"]
> * Linux VM için olağanüstü durum kurtarmayı etkinleştirme
> * Olağanüstü durum kurtarma tatbikatı çalıştırma
> * Ayrıntıdan sonra VM 'yi çoğaltmayı durdur

Bir sanal makine için çoğaltmayı etkinleştirdiğinizde, Site Recovery Mobility hizmeti uzantısı sanal makineye yüklenir ve [Azure Site Recovery](../../site-recovery/site-recovery-overview.md)kaydeder. Çoğaltma sırasında, VM disk yazmaları kaynak bölgedeki bir önbellek depolama hesabına gönderilir. Veriler buradan hedef bölgeye gönderilir ve kurtarma noktaları verilerden oluşturulur.  Olağanüstü durum kurtarma sırasında VM 'yi başka bir bölgeye devretmek, hedef bölgedeki VM 'yi geri yüklemek için bir kurtarma noktası kullanılır.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/pricing/free-trial/) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

1. Azure aboneliğinizin hedef bölgede bir VM oluşturmanıza izin verdiğinden emin olun. Ücretsiz Azure hesabınızı oluşturduysanız, aboneliğin yöneticisi olursunuz ve ihtiyacınız olan izinleriniz vardır.
2. Abonelik yöneticisi değilseniz, size atamak için yönetici ile çalışın:
    - Sanal makine katılımcısı için yerleşik rol ya da belirli izinler:
        - Seçilen sanal ağda sanal makine oluşturma.
        - Bir Azure depolama hesabına yazın.
        - Azure yönetilen diskine yazın.
     - Site Recovery katkıda bulunan yerleşik rolü, kasadaki Site Recovery işlemlerini yönetmek için. 
3. Linux VM 'nin [desteklenen bir işletim sistemi](../../site-recovery/azure-to-azure-support-matrix.md#linux)çalıştırdığından emin olun.
4. VM giden bağlantılar, URL tabanlı bir ara sunucu kullanıyorsa, bu URL 'Lere erişebildiğinden emin olun. Kimliği doğrulanmış bir ara sunucu kullanılması desteklenmez.

    **Ad** | **Genel bulut** | **Kamu Bulutu** | **Ayrıntılar**
    --- | --- | --- | ---
    Depolama | `*.blob.core.windows.net` | `*.blob.core.usgovcloudapi.net`| VM 'den kaynak bölgesindeki önbellek depolama hesabına veri yazın. 
    Azure AD  | `login.microsoftonline.com` | `login.microsoftonline.us`| Site Recovery hizmeti URL 'Lerini yetkilendirme ve kimlik doğrulama. 
    Çoğaltma | `*.hypervrecoverymanager.windowsazure.com` | `*.hypervrecoverymanager.windowsazure.com`  |Site Recovery hizmetiyle VM iletişimi. 
    Service Bus | `*.servicebus.windows.net` | `*.servicebus.usgovcloudapi.net` | İzleme ve Tanılama verileri için Site Recovery VM yazma işlemleri. 

4. VM 'Ler için ağ trafiğini sınırlandırmak üzere ağ güvenlik grupları (NSG) kullanıyorsanız, bu hizmet etiketlerini (IP adresi grupları) kullanarak VM için giden bağlantıya (HTTPS 443) izin veren NSG kuralları oluşturun. Önce bir test NSG üzerindeki kuralları deneyin.

    **Tag** | **İzin Ver** 
    --- | --- 
    Depolama etiketi | Verilerin VM 'den önbellek depolama hesabına yazılmasına izin verir.
    Azure AD etiketi | Azure AD 'ye karşılık gelen tüm IP adreslerine erişim sağlar.
    EventsHub etiketi | Site Recovery izlemeye erişim sağlar.
    Azuresterecovery etiketi | Herhangi bir bölgedeki Site Recovery hizmetine erişim sağlar.
    GuestAndHybridManagement | Çoğaltma için etkinleştirilen VM 'lerde çalışan Site Recovery Mobility aracısını otomatik olarak yükseltmek istiyorsanız kullanın.
5. VM 'Lerin en son kök sertifikalara sahip olduğundan emin olun. Linux sanal makinelerinde, VM 'deki en son güvenilen kök sertifikaları ve sertifika iptal listesini almak için Linux dağıtıcınız tarafından sunulan yönergeleri izleyin.

## <a name="enable-disaster-recovery"></a>Olağanüstü durum kurtarmayı etkinleştir

1. Azure portal VM özellikleri sayfasını açın.
2. **İşlemler** menüsünden **Olağanüstü durum kurtarma** seçeneğini belirleyin.
3. **Temel bilgiler**  >  **hedef bölgesinde**, VM 'yi çoğaltmak istediğiniz bölgeyi seçin. Kaynak ve hedef bölgeler aynı Azure Active Directory kiracısında olmalıdır.
4. **Gözden geçir + çoğaltmayı Başlat**' a tıklayın.

    :::image type="content" source="./media/tutorial-disaster-recovery/disaster-recovery.png" alt-text="VM özellikleri olağanüstü durum kurtarma sayfasında çoğaltmayı etkinleştirin.":::

5. **Gözden geçir + çoğaltmayı Başlat**' da ayarları doğrulayın:

    - **Hedef ayarlar**. Varsayılan olarak, Site Recovery hedef kaynakları oluşturmak için kaynak ayarlarını yansıtır.
    - **Depolama ayarları-önbellek depolama hesabı**. Kurtarma, kaynak bölgedeki bir depolama hesabını kullanır. Kaynak VM değişiklikleri, hedef konuma çoğaltılmadan önce bu hesapta önbelleğe alınır.
    - **Depolama ayarları-çoğaltma diski**. Site Recovery, varsayılan olarak, kaynak VM ile yönetilen diskleri aynı depolama türüyle (Standart veya Premium) yansıtan hedef bölgede çoğaltma tarafından yönetilen diskler oluşturur.
    - **Çoğaltma ayarları**. Kasa ayrıntılarını gösterir ve Site Recovery tarafından oluşturulan kurtarma noktalarının 24 saat boyunca tutulup tutulmadığını gösterir.
    - **Uzantı ayarları**. Site Recovery, çoğaltılan VM 'lerde yüklü olan Site Recovery Mobility hizmeti uzantısı güncelleştirmelerini yöneteceğini gösterir. Belirtilen Azure Otomasyonu hesabı güncelleştirme işlemini yönetir.

    :::image type="content" source="./media/tutorial-disaster-recovery/settings-summary.png" alt-text="Hedef ve çoğaltma ayarlarının özetini gösteren sayfa.":::

2. **Çoğaltmayı Başlat**' ı seçin. Dağıtım başlar ve Site Recovery hedef kaynak oluşturmaya başlar. Bildirimlerde çoğaltma ilerlemesini izleyebilirsiniz.

    :::image type="content" source="./media/tutorial-disaster-recovery/notifications.png" alt-text="Çoğaltma ilerlemesi için bildirim.":::

## <a name="check-vm-status"></a>VM durumunu denetle

Çoğaltma işi bittikten sonra, sanal makine çoğaltma durumunu kontrol edebilirsiniz.

1. VM özellikleri sayfasını açın.
2. **İşlemler** menüsünden **Olağanüstü durum kurtarma** seçeneğini belirleyin.
3. Kasa, çoğaltma ilkesi ve hedef ayarları hakkında Varsayılanları gözden geçirmek için **temel** bileşenler bölümünü genişletin.
4. **Sağlık ve durum**' da, VM için çoğaltma durumu, aracı sürümü, yük devretme hazırlığı ve en son kurtarma noktaları hakkında bilgi alın. 

    :::image type="content" source="./media/tutorial-disaster-recovery/essentials.png" alt-text="VM olağanüstü durum kurtarma için Essentials görünümü.":::

5. **Altyapı görünümü**' nde kaynak ve hedef VM 'ler, yönetilen diskler ve önbellek depolama hesabı için görsel bir genel bakış alın.

    :::image type="content" source="./media/tutorial-disaster-recovery/infrastructure.png" alt-text="VM olağanüstü durum kurtarma için altyapı görsel haritası.":::


## <a name="run-a-drill"></a>Detaya gitme Çalıştır

Olağanüstü durum kurtarma 'nın beklendiği gibi çalıştığından emin olmak için bir detaya gitme çalıştırın. Yük devretme testi çalıştırdığınızda, devam eden çoğaltma üzerinde veya üretim ortamınızda hiçbir etkisi olmadan sanal makinenin bir kopyasını oluşturur.

1. VM olağanüstü durum kurtarma sayfasında **Yük devretmeyi sına**' yı seçin.
2. **Yük devretme testi** bölümünde, kurtarma noktası Için varsayılan **en son IŞLENEN (düşük RPO)** ayarını bırakın.

   Bu seçenek en düşük kurtarma noktası hedefini (RPO) sağlar ve genellikle VM 'yi hedef bölgede en hızlı bir şekilde getirir. İlk olarak, Site Recovery hizmetine gönderilen tüm verileri, yük devretmadan önce her VM için bir kurtarma noktası oluşturmak üzere işler. Bu kurtarma noktası, yük devretme tetiklendiğinde Site Recovery tüm verilerin çoğaltılmasını sağlar.

3. Yük devretmeden sonra VM 'nin bulunduğu sanal ağı seçin. 

     :::image type="content" source="./media/tutorial-disaster-recovery/test-failover-settings.png" alt-text="Yük devretme testi seçeneklerini ayarlamak için sayfa.":::

4. Yük devretme testi işlemi başlar. İlerleme durumunu bildirimlerde izleyebilirsiniz.

    :::image type="content" source="./media/tutorial-disaster-recovery/test-failover-notification.png" alt-text="Yük devretme bildirimlerini test edin."::: 
    
   Yük devretme testi tamamlandıktan sonra, VM **Essentials** sayfasında *Temizleme testi yük devretme bekleme* durumunda olur. 
  
## <a name="clean-up-resources"></a>Kaynakları temizleme

Ayrıntıdan sonra Site Recovery, VM otomatik olarak temizlenir. 
 
1. Otomatik temizlemeyi başlatmak için **Test yük devretmesini temizle**' yi seçin.

    :::image type="content" source="./media/tutorial-disaster-recovery/start-cleanup.png" alt-text="Essentials sayfasında temizlemeyi başlatın."::: 

6. **Yük devretme sınamasını Temizleme** bölümünde, yük devretme için kaydetmek istediğiniz notları yazın ve ardından test Tamam ' ı seçin **. Test yük devretmesi sanal makinesini silin**. Ardından **Tamam**’ı seçin.

    :::image type="content" source="./media/tutorial-disaster-recovery/delete-test.png" alt-text="Not kaydetme ve test VM 'yi silme sayfası."::: 

7. Silme işlemi başlar. Bildirimlerin ilerlemesini izleyebilirsiniz.

    :::image type="content" source="./media/tutorial-disaster-recovery/delete-test-notification.png" alt-text="İzlenecek bildirimler test sanal makinesini siler."::: 


### <a name="stop-replicating-the-vm"></a>VM 'yi çoğaltmayı durdur

Olağanüstü durum kurtarma detaya gitmeyi tamamladıktan sonra, tam yük devretmeyi denemeye devam etmeyi öneririz. Tam yük devretme yapmak istemiyorsanız, çoğaltmayı devre dışı bırakabilirsiniz. Bu, şunları yapar:

- VM 'yi çoğaltılan makinelerin Site Recovery listesinden kaldırır.
- VM için Site Recovery faturalandırmayı durduruyor.
- Kaynak çoğaltma ayarlarını otomatik olarak temizler.

Şu adımlara göre çoğaltmayı durdurun:

1. VM olağanüstü durum kurtarma sayfasında **çoğaltmayı devre dışı bırak**' ı seçin.
2. **Çoğaltmayı devre dışı bırak**' da, çoğaltmayı devre dışı bırakmak istediğiniz nedenleri seçin. Ardından **Tamam**’ı seçin.

    :::image type="content" source="./media/tutorial-disaster-recovery/disable-replication.png" alt-text="Çoğaltmayı devre dışı bırakma ve bir neden sağlama sayfası."::: 


Çoğaltma sırasında VM 'de yüklü Site Recovery uzantısı otomatik olarak kaldırılmaz. VM için çoğaltmayı devre dışı bırakır ve daha sonra tekrar çoğaltmak istemiyorsanız, Site Recovery uzantısını aşağıdaki şekilde el ile kaldırabilirsiniz: 

1. VM > **ayarları**  >  **uzantılarına** gidin.
2. **Uzantılar** sayfasında, Linux için *Microsoft. Azure. recoveryservices* girişini seçin.
3. Uzantının Özellikler sayfasında **Kaldır**' ı seçin.


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir Azure VM için olağanüstü durum kurtarma yapılandırmış ve bir olağanüstü durum kurtarma ayrıntısı çalıştırdınız. Şimdi VM için tam yük devretme gerçekleştirebilirsiniz.

> [!div class="nextstepaction"]
> [VM'nin yükünü başka bir bölgeye devretme](../../site-recovery/azure-to-azure-tutorial-dr-drill.md)
