---
title: StorSimple Sanal dizi hizmeti Özet dikey penceresi | Microsoft Docs
description: StorSimple Aygıt Yöneticisi için hizmet Özeti dikey penceresini açıklar ve bu hizmetin StorSimple Sanal dizininizdeki sistem durumunu izlemek için nasıl kullanılacağını açıklar.
services: storsimple
documentationcenter: ''
author: manuaery
manager: syadav
editor: ''
ms.assetid: 8a2b9a84-b0e6-48b9-b366-d16f004241a5
ms.service: storsimple
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2016
ms.author: manuaery
ms.openlocfilehash: 31220a8f8b012e08d46195b60a26be9cfc261be5
ms.sourcegitcommit: 374e47efb65f0ae510ad6c24a82e8abb5b57029e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2020
ms.locfileid: "85514516"
---
# <a name="use-the-service-summary-blade-for-storsimple-device-manager-connected-to-storsimple-virtual-array"></a>StorSimple Sanal dizisine bağlı StorSimple Aygıt Yöneticisi için hizmet Özeti dikey penceresini kullanın
## <a name="overview"></a>Genel Bakış
StorSimple Aygıt Yöneticisi için hizmet Özeti dikey penceresi, hizmetinize bağlı olan StorSimple Sanal dizilerinin (Ayrıca, StorSimple şirket içi sanal cihazlar veya sanal cihazlar olarak da bilinir) bir Özet görünümünü sağlar ve bu sayede sistem yöneticisinin dikkatini çekmesini sağlar. Bu öğretici, hizmet Özeti dikey penceresini tanıtır, içerik ve işlevi açıklar ve bu dikey pencereden gerçekleştirebileceğiniz görevleri açıklar.

![Hizmet panosu](./media/storsimple-virtual-array-service-summary/service-blade.png)

## <a name="management-commands-and-essentials"></a>Yönetim komutları ve temel bileşenler
StorSimple Özet dikey penceresinde, StorSimple Aygıt Yöneticisi hizmetinizi ve bu hizmete kayıtlı sanal dizileri yönetme seçeneklerini görürsünüz. Yönetim komutlarını dikey pencerenin üst kısmında ve sol tarafta görürsünüz.

Paylaşımlar veya birimler ekleme gibi çeşitli işlemleri gerçekleştirmek veya sanal diziler üzerinde çalışan çeşitli işleri izlemek için bu seçenekleri kullanın.

Essentials alanı, StorSimple Aygıt Yöneticisi oluşturulduğu kaynak grubu, konum ve abonelik gibi bazı önemli özelliklerden bazılarını yakalar.

## <a name="storsimple-device-manager-service-summary"></a>StorSimple Aygıt Yöneticisi hizmeti Özeti
* **Uyarılar** kutucuğu, tüm sanal cihazlarda uyarı önem derecesine göre gruplandırılan tüm etkin uyarıların anlık görüntüsünü sağlar. Kutucuğa tıkladığınızda, önerilen eylemler de dahil olmak üzere uyarı hakkında ek ayrıntıları görüntülemek için tek bir uyarıya tıklabileceğiniz **Uyarılar** dikey penceresi açılır. Sorun çözümlenirse uyarıyı da temizleyebilirsiniz.
* **Kapasite** kutucuğu, tüm sanal cihazlarda kullanılabilir olan ve tüm sanal cihazlarda bulunan toplam depolamaya göre sağlanan ve kalan birincil depolamayı gösterir. **Sağlanan** ve kullanım için ayrılan depolama miktarına başvuruyorsa, **kalan** , tüm sanal cihazlarda sağlanabilen kalan kapasiteyi ifade eder. **Kalan katmanlı** kapasite, bulut dahil olmak üzere sağlanan kapasiteye sahip olsa da, **kalan yerel** , sanal dizilere bağlı disklerde kalan kapasiteye sahip olur.
* **Kullanım** grafiğinde, sanal cihazlarınızla ilgili ölçümleri görebilirsiniz. Tüm sanal cihazlarda kullanılan birincil depolamayı, ayrıca son 7 gün boyunca sanal aygıtlar tarafından tüketilen bulut depolama alanını ve varsayılan zaman aralığını görüntüleyebilirsiniz. Farklı bir zaman ölçeği seçmek için grafiğin sağ üst köşesindeki **Düzenle** seçeneğini kullanın.
* **Cihazlar** kutucuğu, storsimple Aygıt Yöneticisi cihaz durumuna göre gruplanmış sanal dizi sayısının özetini sağlar. Bu kutucuğa tıklayarak **cihazlar** listesi dikey penceresini açın ve cihaza özgü cihaz özetine gitmek için tek bir cihaza tıklayın. Ayrıca, belirli bir cihaz Özeti dikey penceresinden cihaza özgü eylemler gerçekleştirebilirsiniz. Cihaz Özeti dikey penceresi hakkında daha fazla bilgi için [cihaz Özeti dikey penceresine](storsimple-virtual-array-device-summary.md)gidin.

## <a name="view-the-activity-logs"></a>Etkinlik günlüklerini görüntüleme
StorSimple Aygıt Yöneticisi içinde yürütülen çeşitli işlemleri görüntülemek için, StorSimple hizmet Özeti dikey pencerenizin sol tarafındaki **etkinlik günlükleri** bağlantısına tıklayın. Bu, sizi gerçekleştirilen son işlemlerin özetini görebileceğiniz **etkinlik günlükleri** dikey penceresine götürür.

![Etkinlik Günlükleri](./media/storsimple-virtual-array-service-summary/activity-log.png)

## <a name="next-steps"></a>Sonraki adımlar
[StorSimple Sanal dizinizi yönetmek için yerel Web Kullanıcı arabirimini nasıl kullanacağınızı](storsimple-ova-web-ui-admin.md)öğrenin.

