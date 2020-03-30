---
title: Azure Site Kurtarma'da Azure VM'leri için çoğaltma yapılandırma
description: Site Kurtarma'yı kullanarak Azure VM'leri için çoğaltmayı başka bir bölgeye nasıl yapılandıracaklarını öğrenin.
author: sideeksh
manager: rochakm
ms.topic: how-to
ms.date: 04/29/2018
ms.openlocfilehash: 1c6b7cfbf193f02598052b6922efec17fb16ec83
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75973702"
---
# <a name="replicate-azure-vms-to-another-azure-region"></a>Azure VM'leri başka bir Azure bölgesine çoğaltma


Bu makalede, azure vm'lerinin bir Azure bölgesinden diğerine çoğaltılmasını nasıl etkinleştirilir.

## <a name="before-you-start"></a>Başlamadan önce

Bu makalede, [Azure'dan Azure'a olağanüstü durum kurtarma öğreticisinde](azure-to-azure-tutorial-enable-replication.md)açıklandığı gibi Site Kurtarma dağıtımı için hazırlandığınızı varsayar.

Ön koşullar yerinde olmalı ve bir Kurtarma Hizmetleri kasası oluşturmuş olmalısınız.


## <a name="enable-replication"></a>Çoğaltmayı etkinleştirme

Çoğaltmayı etkinleştirin. Bu yordam, birincil Azure bölgesinin Doğu Asya, ikincil bölgenin ise Güney Doğu Asya olduğunu varsayar.

1. Kasada **+Çoğaltma'yı**tıklatın.
2. Aşağıdaki alanlara dikkat edin:
   - **Kaynak**: Bu durumda **Azure**olan VM'lerin çıkış noktası.
   - **Kaynak konumu**: VM'lerinizi korumak istediğiniz yerden Azure bölgesi. Bu çizim için, kaynak konumu 'Doğu Asya'
   - **Dağıtım modeli**: Kaynak makinelerin Azure dağıtım modeli.
   - Kaynak abonelik : Kaynak VM'lerinizin ait olduğu **abonelik.** Bu abonelik, kurtarma hizmetleri kasanızın bulunduğu Azure Active Directory kiracısında bulunan aboneliklerden biri olabilir.
   - **Kaynak Grubu**: Kaynak sanal makinelerinizin ait olduğu kaynak grubudur. Seçili kaynak grubunun altındaki tüm VM'ler bir sonraki adımda korunmak üzere listelenir.

     ![Çoğaltmayı etkinleştirme](./media/site-recovery-replicate-azure-to-azure/enabledrwizard1.png)

3. **Sanal Makineler > Seçin sanal makinelerde,** çoğaltmak istediğiniz her VM'yi tıklatın ve seçin. Yalnızca çoğaltmanın etkinleştirildiği makineleri seçebilirsiniz. Ardından **Tamam**'a tıklayın.
    ![Çoğaltmayı etkinleştirme](./media/site-recovery-replicate-azure-to-azure/virtualmachine_selection.png)

4. **Ayarlar'da,** isteğe bağlı olarak hedef site ayarlarını yapılandırabilirsiniz:

   - **Hedef Konum**: Kaynak sanal makine verilerinizin çoğaltılacağı konum. Seçtiğiniz makinelerin konumuna bağlı olarak, Site Kurtarma size uygun hedef bölgelerin listesini sağlayacaktır. Hedef konumu Kurtarma Hizmetleri kasa konumuyla aynı tutmanızı öneririz.
   - **Hedef abonelik**: Olağanüstü durum kurtarma için kullanılan hedef abonelik. Hedef abonelik varsayılan olarak kaynak abonelikle aynı olur.
   - **Hedef kaynak grubu**: Çoğaltılan tüm sanal makinelerinizin ait olduğu kaynak grubudur.
       - Varsayılan olarak Site Kurtarma adında bir "asr" soneki ile hedef bölgede yeni bir kaynak grubu oluşturur.
       - Site Kurtarma tarafından oluşturulan kaynak grubu zaten varsa, yeniden kullanılır.
       - Kaynak grubu ayarlarını özelleştirebilirsiniz.
       - Hedef kaynak grubunun konumu, kaynak VM'lerin barındırıldığı bölge dışında herhangi bir Azure bölgesi olabilir.
   - **Hedef sanal ağ**: Varsayılan olarak, Site Kurtarma adında bir "asr" soneki ile hedef bölgede yeni bir sanal ağ oluşturur. Bu, kaynak ağınıza eşlenir ve gelecekteki herhangi bir koruma için kullanılır. Ağ eşleme hakkında [daha fazla bilgi edinin.](site-recovery-network-mapping-azure-to-azure.md)
   - **Hedef depolama hesapları (kaynak VM yönetilen diskleri kullanmaz)**: Varsayılan olarak, Site Kurtarma kaynak VM depolama yapılandırmanızı taklit eden yeni bir hedef depolama hesabı oluşturur. Depolama hesabı zaten varsa, yeniden kullanılır.
   - **Çoğaltma tarafından yönetilen diskler (kaynak VM yönetilen diskler kullanır)**: Site Kurtarma, kaynak VM'nin yönetilen diski ile aynı depolama türüne (Standart veya premium) sahip kaynak VM yönetilen diskleri yansıtmak için hedef bölgede yeni çoğaltma yönetilen diskler oluşturur.
   - **Önbellek Depolama hesapları**: Site Kurtarma kaynak bölgede önbellek depolama adı verilen ek depolama hesabı gerekir. Kaynak VM'lerde gerçekleşen tüm değişiklikler izlenir ve bunları hedef konuma kopyalamadan önce önbellek depolama hesabına gönderilir. Bu depolama hesabı Standart olmalıdır.
   - **Hedef kullanılabilirlik kümeleri**: Varsayılan olarak, Site Kurtarma, kaynak bölgede ayarlanan kullanılabilirlik kümesinin bir parçası olan VM'ler için, adında "asr" soneki ile hedef bölgede yeni bir kullanılabilirlik kümesi oluşturur. Site Kurtarma tarafından oluşturulan kullanılabilirlik kümesi zaten varsa, yeniden kullanılır.
   - **Hedef kullanılabilirlik bölgeleri**: Varsayılan olarak, Hedef Bölge kullanılabilirlik bölgelerini destekliyorsa, Site Kurtarma hedef bölgedeki kaynak bölgeyle aynı bölge numarasını atar.

     Hedef bölge kullanılabilirlik bölgelerini desteklemiyorsa, hedef VM'ler varsayılan olarak tek örnek olarak yapılandırılır. Gerekirse, 'Özelleştir'i tıklatarak bu tür VM'leri hedef bölgedeki kullanılabilirlik kümelerinin bir parçası olacak şekilde yapılandırabilirsiniz.

     >[!NOTE]
     >Çoğaltmayı etkinleştirdikten sonra kullanılabilirlik türünü - tek örnek, kullanılabilirlik kümesi veya kullanılabilirlik bölgesini değiştiremezsiniz. Kullanılabilirlik türünü değiştirmek için çoğaltmayı devre dışı bırakıp etkinleştirmeniz gerekir.
     >

   - **Çoğaltma İlkesi**: Kurtarma noktası tutma geçmişi ve uygulama tutarlı anlık görüntü sıklığı ayarlarını tanımlar. Varsayılan olarak, Azure Site Kurtarma kurtarma noktası tutma için varsayılan '24 saat' ve uygulama tutarlı anlık görüntü sıklığı için '4 saat' varsayılan ayarları ile yeni bir çoğaltma ilkesi oluşturur.

     ![Çoğaltmayı etkinleştirme](./media/site-recovery-replicate-azure-to-azure/enabledrwizard3.PNG)

### <a name="enable-replication-for-added-disks"></a>Eklenen diskler için çoğaltmayı etkinleştirme

Çoğaltmanın etkin olduğu bir Azure VM'sine disk eklerseniz, aşağıdakiler oluşur:
-   VM için çoğaltma durumu bir uyarı gösterir ve bir not bir veya daha fazla diskin korunmak için kullanılabildiğini bildirir.
-   Eklenen diskler için koruma yı etkinleştiriseniz, uyarı diskin ilk çoğaltıldıktan sonra kaybolur.
-   Disk için çoğaltmayı etkinleştirmemeyi seçerseniz, uyarıyı reddetmeyi seçebilirsiniz.


    ![Yeni disk eklendi](./media/azure-to-azure-how-to-enable-replication/newdisk.png)

Eklenen bir disk için çoğaltmayı etkinleştirmek için aşağıdakileri yapın:

1.  **Çoğaltılan Öğeler**> kasasında, diski eklediğiniz VM'yi tıklatın.
2.  **Diskler'i**tıklatın ve ardından çoğaltmayı etkinleştirmek istediğiniz veri diskini seçin (bu disklerin **korumalı olmayan** bir durumu vardır).
3.  **Disk Ayrıntıları'nda**, **çoğaltmayı etkinleştir'i**tıklatın.

    ![Eklenen disk için çoğaltmayı etkinleştirme](./media/azure-to-azure-how-to-enable-replication/enabled-added.png)

Etkinleştirçoğaltma işi çalıştırır ve ilk çoğaltma bittikten sonra, disk sorunu için çoğaltma sistem durumu uyarısı kaldırılır.



## <a name="customize-target-resources"></a>Hedef kaynakları özelleştirme

Site Kurtarma tarafından kullanılan varsayılan hedef ayarlarını değiştirebilirsiniz.

1. Varsayılan hedef aboneliği ni değiştirmek için **Özelleştir'i tıklatın:** 'Hedef abonelik'in yanında. Aboneliği, aynı Azure Etkin Dizini (AAD) kiracısında bulunan tüm abonelikler listesinden seçin.

2. **Özelleştir'i tıklatın:** varsayılan ayarları değiştirmek için:
    - **Hedef kaynak grubunda,** aboneliğin hedef konumundaki tüm kaynak grupları listesinden kaynak grubunu seçin.
    - **Hedef sanal ağda,** hedef konumdaki tüm sanal ağların listesinden ağı seçin.
    - **Kullanılabilirlik kümesinde,** kaynak bölgede ayarlanan kullanılabilirlik kümesinin bir parçasıysa, VM'ye kullanılabilirlik kümesi ayarlarını ekleyebilirsiniz.
    - **Hedef Depolama hesaplarında,** kullanmak istediğiniz hesabı seçin.

        ![Çoğaltmayı etkinleştirme](./media/site-recovery-replicate-azure-to-azure/customize.PNG)
3. Çoğaltma ayarlarını değiştirmek için **Özelleştir'i** tıklatın.
4. **Multi-VM tutarlılığında,** birlikte çoğaltmak istediğiniz VM'leri seçin.
    - Bir çoğaltma grubundaki tüm makineler, yük devredildiğinde paylaşılan kilitlenmeyle tutarlı ve uygulamayla tutarlı kurtarma noktalarına sahip olur.
    - Çoklu VM tutarlılığını etkinleştirmek iş yükü performansını etkileyebilir (CPU yoğun olduğu için). Yalnızca makineler aynı iş yükünü çalıştırıyorsa ve birden çok makinede tutarlılık gerekiyorsa etkinleştirilmelidir.
    - Örneğin, bir uygulamada 2 SQL Server sanal makinesi ve iki web sunucusu varsa, bir çoğaltma grubuna yalnızca SQL Server VM'leri eklemeniz gerekir.
    - Bir çoğaltma grubunda en fazla 16 VM'ye sahip olmayı seçebilirsiniz.
    - Çoklu VM tutarlılığını etkinleştirirseniz çoğaltma grubundaki makineler birbiriyle 20004 numaralı bağlantı noktası üzerinden iletişim kurar.
    - 20004 bağlantı noktası üzerinden VM'ler arasındaki dahili iletişimi engelleyen güvenlik duvarı cihazı olmadığından emin olun.
    - Linux VM'lerinin bir çoğaltma grubunun parçası olmasını istiyorsanız, 20004 bağlantı noktasındaki giden trafiğin belirli Linux sürümü kılavuzuna göre el ile açıldığından emin olun.
![Çoğaltmayı etkinleştirme](./media/site-recovery-replicate-azure-to-azure/multivmsettings.PNG)

5. **Hedef kaynak** > Oluştur**Çoğaltma'yı etkinleştir'i**tıklatın.
6. VM'ler çoğaltma için etkinleştirildikten sonra, **Çoğaltılan öğeler** altında VM durumunu kontrol edebilirsiniz

>[!NOTE]
>İlk çoğaltma sırasında durum ilerleme olmadan, yenilemek için biraz zaman alabilir. En son durumu almak için **Yenile** düğmesini tıklatın.
>

## <a name="next-steps"></a>Sonraki adımlar

Bir test başarısızlığını çalıştırma hakkında [daha fazla bilgi edinin.](site-recovery-test-failover-to-azure.md)
