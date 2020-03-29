---
title: Azure Otomasyonu ile dosya içeriği değişikliklerini görüntüleme
description: Değiştirilen bir dosyanın içeriğini görüntülemek için Değiştir izlemenin dosya içeriği değiştirme özelliğini kullanın.
services: automation
ms.subservice: change-inventory-management
ms.date: 07/03/2018
ms.topic: conceptual
ms.openlocfilehash: 23c4f24e430d58895eb551c3e2cb62b5f0003ac0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75418820"
---
# <a name="view-contents-of-a-file-that-is-being-tracked-with-change-tracking"></a>Değişiklik İzleme ile izlenen bir dosyanın içeriğini görüntüleme

Dosya içeriği izleme, Değişiklik İzleme ile izlenen bir değişiklikten önce ve sonra bir dosyanın içeriğini görüntülemenize olanak tanır. Bunu yapmak için, her değişiklik gerçekleştikten sonra dosya içeriğini bir depolama hesabına kaydeder.

## <a name="requirements"></a>Gereksinimler

* Dosya içeriğini depolamak için Kaynak Yöneticisi dağıtım modelini kullanan standart bir depolama hesabı gereklidir. Premium ve klasik dağıtım modeli depolama hesapları kullanılmamalıdır. Depolama hesapları hakkında daha fazla bilgi için Azure [depolama hesapları hakkında](../storage/common/storage-create-storage-account.md) bilgi

* Kullanılan depolama hesabı yalnızca 1 Otomasyon Hesabı bağlı olabilir.

* [Otomasyon](automation-change-tracking.md) Hesabınızda Değişiklik İzleme etkinleştirilir.

## <a name="enable-file-content-tracking"></a>Dosya içeriği izlemeyi etkinleştirme

1. Azure portalında Otomasyon hesabınızı açın ve ardından **Izlemeyi Değiştir'i**seçin.
2. Üst menüde **Ayarları Edit'i**seçin.
3. **Dosya İçeriği'ni** seçin ve **Bağlantı'yı**tıklatın. Bu, **Değişiklik İzleme** için İçerik Konumu Ekle bölmesini açar.

   ![seçin](./media/change-tracking-file-contents/enable.png)

4. Dosya içeriğini depolamak için kullanılacak abonelik ve depolama hesabını seçin. Varolan tüm izlenen dosyalar için dosya içeriği izlemeyi etkinleştirmek istiyorsanız, **tüm ayarlar için Yükleme dosyası içeriğini**açmak için **Açık'ı** seçin. Daha sonra her dosya yolu için bunu değiştirebilirsiniz.

   ![depolama hesabı ayarlama](./media/change-tracking-file-contents/storage-account.png)

5. Etkinleştirildiğinde, depolama hesabı ve SAS Uris gösterilir. SAS Uris'in süresi 365 gün sonra sona erer ve **Yeniden Oluştur** düğmesini tıklatarak yeniden oluşturulabilir.

   ![liste hesap anahtarları](./media/change-tracking-file-contents/account-keys.png)

## <a name="add-a-file"></a>Dosya ekleme

Aşağıdaki adımlar, bir dosya için değişiklik izlemeyi açmada size yol açar:

1. **İzlemeyi Değiştir'in** **Ayarları Değiştir** **sayfasında, Windows Files** veya Linux **Dosyaları** sekmesini seçin ve **Ekle'yi** tıklatın

1. Dosya yolunun bilgilerini doldurun ve **tüm ayarlar için Yükleme dosyası içeriğialtında** **True'yu** seçin. Bu ayar, yalnızca bu dosya yolu için dosya içeriği izleme sağlar.

   ![linux dosyası ekleme](./media/change-tracking-file-contents/add-linux-file.png)

## <a name="viewing-the-contents-of-a-tracked-file"></a>İzlenen bir dosyanın içeriğini görüntüleme

1. Dosya veya yoldaki bir dosya için bir değişiklik algılandıktan sonra, portalda gösterir. Değişiklik listesinden dosya değişikliğini seçin. **Ayrıntıları Değiştir** bölmesi görüntülenir.

   ![liste değişiklikleri](./media/change-tracking-file-contents/change-list.png)

1. Ayrıntıları **Değiştir** sayfasında, dosya bilgilerinin önce ve sonra, sol üstte, dosyanın içeriğini görmek için **Dosya İçerik Değişikliklerini** Görüntüle'yi tıklatın.

   ![ayrıntıları değiştirme](./media/change-tracking-file-contents/change-details.png)

1. Yeni sayfa, dosya içeriğini yan yana görünümde gösterir. Değişikliklerin satır satır görünümünü görmek için **Satır Çizgisi'ni** de seçebilirsiniz.

   ![dosya değişikliklerini görüntüleme](./media/change-tracking-file-contents/view-file-changes.png)

## <a name="next-steps"></a>Sonraki adımlar

Çözümü kullanma hakkında daha fazla bilgi edinmek için İzlemeyi Değiştir'deki öğreticiyi ziyaret edin:

> [!div class="nextstepaction"]
> [Ortamınızdaki değişikliklerle ilgili sorunları giderme](automation-tutorial-troubleshoot-changes.md)

* Ayrıntılı değişiklik izleme verilerini görüntülemek için [Azure Monitor günlüklerinde Günlük aramalarını](../log-analytics/log-analytics-log-searches.md) kullanın.

