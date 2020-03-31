---
title: Uygulamayı anlık görüntüden geri yükleme
description: Uygulamanızı anlık görüntüden nasıl geri yükleyin öğrenin. Otomatik gölge kopyalarıyla Premium katmanında beklenmeyen veri kaybından kurtulun.
ms.assetid: 4164f9b5-f735-41c6-a2bb-71f15cdda417
ms.topic: article
ms.date: 04/04/2018
ms.reviewer: nicking
ms.custom: seodec18
ms.openlocfilehash: b17a49535541c8f75f65cdbe9986a895427f3877
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78255145"
---
# <a name="restore-an-app-in-azure-from-a-snapshot"></a>Azure'daki bir uygulamayı anlık görüntüden geri yükleme
Bu makalede, [Azure Uygulama Hizmeti'ndeki](../app-service/overview.md) bir uygulamayı anlık görüntüden nasıl geri yükleyebilirsiniz gösterilmektedir. Uygulamanızın anlık görüntülerinden birine dayanarak uygulamanızı önceki duruma geri yükleyebilirsiniz. Anlık görüntü yedekleme etkinleştirmek gerekmez, platform otomatik olarak veri kurtarma amacıyla tüm uygulamaların bir anlık kaydeder.

Anlık görüntüler artımlı gölge kopyalarıdır ve normal [yedeklemelere](manage-backup.md)göre çeşitli avantajlar sunar:
- Dosya kilitleri nedeniyle dosya kopyalama hatası yok.
- Depolama boyutu sınırlaması yok.
- Yapılandırma gerekmez.

Anlık görüntülerden geri ödeme, **Premium** katmanda veya daha yüksek bir seviyede çalışan uygulamalar için kullanılabilir. Uygulamanızı ölçeklendirme hakkında bilgi için [Azure'da bir uygulamayı ölçeklendir'e](manage-scale-up.md)bakın.

## <a name="limitations"></a>Sınırlamalar

- Özellik şu anda önizlemede.
- Yalnızca aynı uygulamaya veya bu uygulamaya ait bir yuvaya geri yükleyebilirsiniz.
- Uygulama Hizmeti geri yükleme yaparken hedef uygulamayı veya hedef yuvasını durdurur.
- Uygulama Hizmeti, platform veri kurtarma amacıyla üç aylık anlık görüntüler tutar.
- Anlık görüntüleri yalnızca son 30 gün için geri yükleyebilirsiniz.
- Uygulama Hizmeti Ortamında çalışan Uygulama Hizmetleri anlık görüntüleri desteklemez.
 

## <a name="restore-an-app-from-a-snapshot"></a>Anlık görüntüden uygulamayı geri yükleme

1. [Azure portalında](https://portal.azure.com)uygulamanızın **Ayarlar** sayfasında Yedekler sayfasını görüntülemek için **Yedekler'i** tıklatın. **Backups** Ardından **Anlık Görüntü (Önizleme)** bölümünün altında **Geri Yükle'yi** tıklatın.
   
    ![](./media/app-service-web-restore-snapshots/1.png)

2. Geri **Yükleme** sayfasında, geri yüklemek için anlık görüntü seçin.
   
    ![](./media/app-service-web-restore-snapshots/2.png)
   
3. **Geri Yükleme hedefinde**uygulama geri yüklemesi için hedef belirtin.
   
    ![](./media/app-service-web-restore-snapshots/3.png)
   
   > [!WARNING]
   > **Overwrite'ı**seçerseniz, uygulamanızın geçerli dosya sistemindeki tüm varolan veriler silinir ve üzerine yazılır. **Tamam'ı**tıklatmadan önce, yapmak istediğiniz şeyin bu olduğundan emin olun.
   > 
   > 
      
   > [!Note]
   > Geçerli teknik sınırlamalar nedeniyle, yalnızca aynı ölçek birimindeki uygulamalara geri yükleyebilirsiniz. Bu sınırlama gelecekteki bir sürümde kaldırılır.
   > 
   > 
   
    Yuvaya geri yüklemek için **Varolan Uygulamayı** seçebilirsiniz. Bu seçeneği kullanmadan önce, uygulamanızda zaten bir yuva oluşturmuş olmalısınız.

4. Sitenizin yapılandırmasını geri yüklemeyi seçebilirsiniz.
   
    ![](./media/app-service-web-restore-snapshots/4.png)

5. **Tamam**'a tıklayın.
