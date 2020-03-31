---
title: Microsoft Azure StorBasit Sanal Dizi yedekleme eğitimi | Microsoft Dokümanlar
description: StorSimple Virtual Array paylaşımlarını ve birimlerini nasıl yedekleyiyarıtanımlar.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: ''
ms.assetid: e3cdcd9e-33b1-424d-82aa-b369d934067e
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 02/27/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a61dcca1f78b6ba444a2deefcf6b8bb4fd5c5087
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60581403"
---
# <a name="back-up-shares-or-volumes-on-your-storsimple-virtual-array"></a>StorSimple Virtual Array'inizdeki paylaşımları veya birimleri yedekleme

## <a name="overview"></a>Genel Bakış

StorSimple Virtual Array, dosya sunucusu veya iSCSI sunucusu olarak yapılandırılabilen karma bulut depolama alanıdır. Sanal dizi, kullanıcının aygıttaki tüm paylaşımların veya birimlerin zamanlanmış ve el ile yedeklerini oluşturmasına olanak tanır. Bir dosya sunucusu olarak yapılandırıldığınızda, madde düzeyinde kurtarma sağlar. Bu öğretici, sanal dizinizde silinmiş bir dosyayı geri yüklemek için zamanlanmış ve el ile yedeklemelerin nasıl oluşturulup madde düzeyinde kurtarma gerçekleştirişize ilişkindir.

Bu öğretici yalnızca StorSimple Sanal Diziler için geçerlidir. 8000 serisi hakkında bilgi için [8000 serisi aygıtı için yedek oluşturma](storsimple-manage-backup-policies-u2.md)

## <a name="back-up-shares-and-volumes"></a>Paylaşımları ve hacimleri yedekleme

Yedeklemeler zaman içinde koruma sağlar, kurtarılabilirliği artırır ve paylaşımlar ve birimler için geri yükleme sürelerini en aza indirir. StorSimple cihazınızdaki bir hisse veya birimi iki şekilde yedekleyebilirsiniz: **Zamanlanmış** veya **Manuel.** Yöntemlerin her biri aşağıdaki bölümlerde ele alınmıştır.

## <a name="change-the-backup-start-time"></a>Yedekleme başlangıç saatini değiştirme

> [!NOTE]
> Bu sürümde, zamanlanmış yedeklemeler, her gün belirli bir zamanda çalışan ve aygıttaki tüm hisseleri veya birimleri yedekleyen varsayılan bir ilke tarafından oluşturulur. Şu anda zamanlanmış yedeklemeler için özel ilkeler oluşturmak mümkün değildir.


StorSimple Virtual Array'inizin varsayılan yedekleme ilkesi günün belirli bir saatinde (22:30) başlar ve aygıttaki tüm paylaşımları veya birimleri günde bir yedekler. Yedeklemenin başladığı zamanı değiştirebilirsiniz, ancak sıklık ve bekletme (saklanacak yedekleme sayısını belirten) değiştirilemez. Bu yedeklemeler sırasında tüm sanal aygıt yedeklenir. Bu, aygıtın performansını ve aygıtta dağıtılan iş yüklerini etkileyebilir. Bu nedenle, bu yedeklemeleri yoğun olmayan saatler için zamanlamanızı öneririz.

 Varsayılan yedekleme başlangıç saatini değiştirmek için Azure [portalında](https://portal.azure.com/)aşağıdaki adımları gerçekleştirin.

#### <a name="to-change-the-start-time-for-the-default-backup-policy"></a>Varsayılan yedekleme ilkesinin başlangıç saatini değiştirmek için

1. **Cihazlara**gidin. StorSimple Device Manager hizmetinize kayıtlı cihazların listesi görüntülenir. 
   
    ![cihazlara gezinme](./media/storsimple-virtual-array-backup/changebuschedule1.png)

2. Cihazınızı seçin ve tıklatın. **Ayarlar** bıçağı görüntülenir. > **Yedekleme ilkelerini yönet'e**gidin.
   
    ![cihazınızı seçin](./media/storsimple-virtual-array-backup/changebuschedule2.png)

3. Yedekleme **ilkeleri** bıçak, varsayılan başlangıç saati 22:30'dur. Aygıt saat dilimindegünlük zamanlamanın yeni başlangıç saatini belirtebilirsiniz.
   
    ![yedekleme ilkelerine gidin](./media/storsimple-virtual-array-backup/changebuschedule5.png)

4. **Kaydet**'e tıklayın.

### <a name="take-a-manual-backup"></a>Manuel yedekleme alın

Zamanlanmış yedeklemelere ek olarak, istediğiniz zaman aygıt verilerinin el ile (isteğe bağlı) yedeklemesini alabilirsiniz.

#### <a name="to-create-a-manual-backup"></a>El ile yedekleme oluşturmak için

1. **Cihazlara**gidin. Cihazınızı seçin ve seçili satırda en sağda **...** sağ tıklatın. Bağlam menüsünden **yedeklemeyi al'ı**seçin.
   
    ![yedekleme almak için gezinme](./media/storsimple-virtual-array-backup/takebackup1m.png)

2. Yedekleme **yi al** bıçağında **yedeklemeyi al'ı**tıklatın. Bu, dosya sunucusundaki tüm paylaşımları veya iSCSI sunucunuzdaki tüm birimleri yedekler. 
   
    ![yedekleme başlatma](./media/storsimple-virtual-array-backup/takebackup2m.png)
   
    İsteğe bağlı yedekleme başlar ve yedekleme işinin başladığını görürsünüz.
   
    ![yedekleme başlatma](./media/storsimple-virtual-array-backup/takebackup3m.png) 
   
    İş başarıyla tamamlandıktan sonra, yeniden bildirilir. Yedekleme işlemi daha sonra başlar.
   
    ![oluşturulan yedekleme işi](./media/storsimple-virtual-array-backup/takebackup4m.png)

3. Yedeklemelerin ilerlemesini izlemek ve iş ayrıntılarına bakmak için bildirimi tıklatın. Bu iş **ayrıntıları**götürür.
   
     ![yedekleme iş ayrıntıları](./media/storsimple-virtual-array-backup/takebackup5m.png)

4. Yedekleme tamamlandıktan **sonra, Yönetim > Yedekleme kataloğuna**gidin. Cihazınızdaki tüm paylaşımların (veya birimlerin) bulut anlık görüntüsünü görürsünüz.
   
    ![Tamamlanmış yedekleme](./media/storsimple-virtual-array-backup/takebackup19m.png) 

## <a name="view-existing-backups"></a>Varolan yedeklemeleri görüntüleme
Varolan yedeklemeleri görüntülemek için Azure portalında aşağıdaki adımları gerçekleştirin.

#### <a name="to-view-existing-backups"></a>Varolan yedeklemeleri görüntülemek için

1. **Devices** blade'e git. Cihazınızı seçin ve tıklatın. **Ayarlar** bıçak, Yönetim **> Yedekleme Kataloğu**gidin.
   
    ![Yedekleme kataloğuna gidin](./media/storsimple-virtual-array-backup/viewbackups1.png)
2. Filtreleme için kullanılacak aşağıdaki ölçütleri belirtin:
   
   - **Zaman aralığı** – **Son 1 saat**olabilir , **Son 24 saat**, Son 7 **gün**, **Son 30 gün**, Geçmiş **yıl**, ve **Özel tarih**.
    
   - **Cihazlar** – StorSimple Device Manager hizmetinize kayıtlı dosya sunucuları veya iSCSI sunucuları listesinden seçim inizi seçin.
   
   - **Başlatılan** – otomatik olarak **Zamanlanabilir** (bir yedekleme ilkesi tarafından) veya **Manuel olarak** başlatılabilir (sizin tarafından).
   
     ![Filtre yedeklemeleri](./media/storsimple-virtual-array-backup/viewbackups2.png)

3. **Uygula**’ya tıklayın. Filtre uygulanmış yedekleme listesi Yedekleme **kataloğu** nda görüntülenir. Belirli bir anda yalnızca 100 yedekleme öğesi görüntülenebileceğini unutmayın.
   
    ![Güncelleştirilmiş yedekleme kataloğu](./media/storsimple-virtual-array-backup/viewbackups3.png)

## <a name="next-steps"></a>Sonraki adımlar

[StorSimple Sanal Dizinizi yönetme](storsimple-ova-web-ui-admin.md)hakkında daha fazla bilgi edinin.

