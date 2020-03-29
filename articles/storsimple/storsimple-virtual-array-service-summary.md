---
title: StorSimple Virtual Array hizmet özeti bıçak | Microsoft Dokümanlar
description: StorSimple Device Manager için hizmet özeti bıçak açıklar ve StorSimple Virtual Array durumunu izlemek için nasıl kullanılacağını açıklar.
services: storsimple
documentationcenter: ''
author: manuaery
manager: syadav
editor: ''
ms.assetid: 8a2b9a84-b0e6-48b9-b366-d16f004241a5
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2016
ms.author: manuaery
ms.openlocfilehash: 9c05bddaeb3c34400db1ec75c624ef00a85d9444
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "64720722"
---
# <a name="use-the-service-summary-blade-for-storsimple-device-manager-connected-to-storsimple-virtual-array"></a>StorSimple Virtual Array'e bağlı StorSimple Device Manager için servis özet bıçağını kullanın
## <a name="overview"></a>Genel Bakış
StorSimple Device Manager'ın hizmet özet bıçağı, hizmetinize bağlı Olan StorSimple Sanal Dizilerin (şirket içi sanal aygıtlar veya sanal aygıtlar olarak da bilinir) özet görünümünü sağlayarak sisteme ihtiyaç duyanları vurgular yöneticinin dikkati. Bu öğretici, hizmet özeti bıçak tanıttı, içerik ve işlev açıklar ve bu bıçak gerçekleştirebilirsiniz görevleri açıklar.

![Servis panosu](./media/storsimple-virtual-array-service-summary/service-blade.png)

## <a name="management-commands-and-essentials"></a>Yönetim komutları ve temelleri
StorSimple özet bıçak, StorSimple Aygıt Yöneticisi hizmeti yanı sıra bu hizmete kayıtlı sanal dizileri yönetmek için seçenekler bakın. Yönetimin komutlarını bıçağın üstündeve sol tarafında görüyorsunuz.

Paylaşım veya birim eklemek veya sanal dizilerde çalışan çeşitli işleri izlemek gibi çeşitli işlemleri gerçekleştirmek için bu seçenekleri kullanın.

Temel alan, StorSimple Aygıt Yöneticinizin oluşturulduğu kaynak grubu, konum ve abonelik gibi bazı önemli özellikleri yakalar.

## <a name="storsimple-device-manager-service-summary"></a>StorSimple Device Manager hizmet özeti
* **Uyarılar** döşemesi, uyarı şiddetine göre gruplanmış tüm sanal aygıtlar daki tüm etkin uyarıların anlık görüntüsünü sağlar. Döşemeyi tıklattığınızda, önerilen eylemler de dahil olmak üzere bu uyarıyla ilgili ek ayrıntıları görüntülemek için tek bir uyarıyı tıklatabileceğiniz **Uyarılar** bıçağı açılır. Sorun çözülmüşse uyarıyı da temizleyebilirsiniz.
* **Kapasite** döşemesi ekranları, tüm sanal aygıtlar arasında kullanılabilen toplam depolama alanına göre tüm sanal aygıtlar arasında sağlanan ve kalan birincil depolamayı gösterir. **Sağlanan,** kullanılmak üzere hazırlanan ve ayrılan depolama miktarını ifade eder, **Kalan** tüm sanal aygıtlar arasında sağlanabilir kalan kapasiteyi ifade eder. **Kalan Katmanlı** kapasite bulut da dahil olmak üzere sağlanabilir kullanılabilir kapasitedir, **Kalan Yerel** ise sanal dizilere bağlı disklerde kalan kapasitedir.
* **Kullanım** grafiğinde, sanal aygıtlarınız için ilgili ölçümleri görebilirsiniz. Tüm sanal aygıtlarda kullanılan birincil depolama alanının yanı sıra, varsayılan zaman dilimi olan son 7 gün içinde sanal aygıtlar tarafından tüketilen bulut depolamasını da görüntüleyebilirsiniz. Farklı bir zaman ölçeği seçmek için grafiğin sağ üst köşesindeki **Edit** seçeneğini kullanın.
* **Aygıtlar** döşemesi, aygıt durumuna göre gruplanan StorSimple Device Manager'ınızdaki sanal dizi sayısının bir özetini sağlar. **Cihazlar** listesi bıçağını açmak için bu döşemeyi tıklatın ve ardından cihaza özgü aygıt özetini delmek için tek bir aygıtı tıklatın. Belirli bir aygıt özet bıçağından aygıta özel eylemler de gerçekleştirebilirsiniz. Cihaz özet bıçağı hakkında daha fazla bilgi için [Aygıt özet bıçağına](storsimple-virtual-array-device-summary.md)gidin.

## <a name="view-the-activity-logs"></a>Etkinlik günlüklerini görüntüleme
StorSimple Device Manager'ınızda gerçekleştirilen çeşitli işlemleri görüntülemek için StorSimple servis özet bıçağınızın sol tarafındaki **Etkinlik günlükleri** bağlantısını tıklatın. Bu, gerçekleştirilen son işlemlerin bir özetini görebileceğiniz **Etkinlik günlükleri** bıçağına götürür.

![Etkinlik Günlükleri](./media/storsimple-virtual-array-service-summary/activity-log.png)

## <a name="next-steps"></a>Sonraki adımlar
[StorSimple Virtual Array'inizi yönetmek için yerel web kullanıcı](storsimple-ova-web-ui-admin.md)arama ayını nasıl kullanacağınızı öğrenin.

