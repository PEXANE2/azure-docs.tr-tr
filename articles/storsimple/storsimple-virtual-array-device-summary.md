---
title: StorSimple Virtual Array cihaz özet bıçak | Microsoft Dokümanlar
description: StorSimple Aygıt Yöneticisi için aygıt özet bıçağını açıklar ve StorSimple Virtual Array'inizin durumunu izlemek için nasıl kullanılacağını açıklar.
services: storsimple
documentationcenter: ''
author: manuaery
manager: syadav
editor: ''
ms.assetid: a13c1ea7-6428-4234-84a6-0ebf51670a85
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/29/2016
ms.author: manuaery
ms.openlocfilehash: 9edc0b552f5c2f38e646bc4b44dd8df5c16b0457
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "61408515"
---
# <a name="use-the-device-summary-blade-for-storsimple-device-manager-connected-to-storsimple-virtual-array"></a>StorSimple Virtual Array'e bağlı StorSimple Device Manager için aygıt özet bıçağını kullanın

## <a name="overview"></a>Genel Bakış

StorSimple Device Manager aygıt bıçağı, belirli bir StorSimple Aygıt Yöneticisi'ne kayıtlı bir StorSimple Virtual Array'in özet görünümünü sağlayarak sistem yöneticisinin dikkatine ihtiyaç duyan aygıt sorunlarını vurgular. Bu öğretici, aygıt özeti bıçağını tanıtır, içeriği ve işlevi açıklar ve bu bıçaktan gerçekleştirebileceğiniz görevleri açıklar.

Cihaz özeti bıçak aşağıdaki bilgileri görüntüler:

![Cihaz panosu](./media/storsimple-virtual-array-device-summary/device-blade.png)



## <a name="management"></a>Yönetim

StorSimple aygıt bıçaklarında, StorSimple cihazınızı yönetme seçeneklerini görürsünüz. Yönetimin komutlarını bıçağın üstündeve sol tarafında görüyorsunuz. Paylaşımlar veya birimler eklemek veya sanal diziniz üzerinde güncelleştirmek veya başarısız olmak için bu seçenekleri kullanın.

Temel alan, durum, model, yazılım sürümü gibi bazı önemli özellikleri n yanı sıra dizinin **Web Ara Birimi'ne** bağlantı yakalar. Bir dahili ağdaysanız, sanal dizinizi yönetmek için [doğrudan yerel web web web ui'sini](storsimple-ova-web-ui-admin.md) başlatabilirsiniz.

![Cihaz temelleri](./media/storsimple-virtual-array-device-summary/device-essentials.png)

## <a name="storsimple-device-summary"></a>StorBasit cihaz özeti

* **Uyarılar** döşemesi, uyarı şiddetine göre gruplanmış sanal diziniz için tüm etkin uyarıların anlık görüntüsünü sağlar. **Uyarılar** bıçağını açmak için döşemeyi tıklatın ve ardından önerilen eylemler de dahil olmak üzere bu uyarıyla ilgili ek ayrıntıları görüntülemek için tek bir uyarıyı tıklatın. Sorun çözülmüşse uyarıyı da temizleyebilirsiniz.

* **Kapasite** döşemesi, sanal aygıtta aynı depoiçin kullanılabilen toplam depolama alanına göre sağlanan ve sanal aygıt ta kalan birincil depolamayı görüntüler. **Sağlanan,** kullanılmak üzere hazırlanan ve ayrılan depolama miktarını ifade eder, **Kalan** bu aygıt ta sağlanabilir kalan kapasiteyi ifade eder. **Kalan Katmanlı** kapasite bulut da dahil olmak üzere sağlanabilir kullanılabilir kapasitedir, **Kalan Yerel** ise bu sanal diziye bağlı disklerde kalan kapasitedir.

* **Kullanım** grafiğinde, sanal dizinizde kullanılan birincil depolama alanının yanı sıra varsayılan zaman dilimi olan son 7 gün içinde tüketilen bulut depolamasını görüntüleyebilirsiniz. Farklı bir zaman ölçeği seçmek için grafiğin sağ üst köşesindeki **Edit** seçeneğini kullanın.

* **Hisse** ler veya **Birimler** döşemesi, cihazınızda duruma göre gruplanmış hisse veya birim sayısının bir özetini sağlar. **Hisse** ler veya **Birimler** liste bıçaklarını açmak için döşemeyi tıklatın ve sonra özelliklerini görüntülemek veya değiştirmek için tek bir paylaşım veya birimin üzerine tıklayın. Daha fazla bilgi [için, paylaşımları](storsimple-virtual-array-manage-shares.md) nasıl yöneteceklerine veya birimleri nasıl [yönetenene](storsimple-virtual-array-manage-volumes.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar
Şunları nasıl yapacağınızı öğrenin:
- [StorSimple Virtual Array'deki paylaşımları yönetme](storsimple-virtual-array-manage-shares.md)
    
- [StorSimple Virtual Array'de birimleri yönetme](storsimple-virtual-array-manage-volumes.md)

