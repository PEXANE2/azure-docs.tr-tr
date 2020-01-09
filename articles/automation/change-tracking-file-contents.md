---
title: Azure Otomasyonu ile dosya içeriği değişikliklerini görüntüleme
description: Değiştirilen bir dosyanın içeriğini görüntülemek için değişiklik izlemenin dosya içeriği değiştirme özelliğini kullanın.
services: automation
ms.subservice: change-inventory-management
ms.date: 07/03/2018
ms.topic: conceptual
ms.openlocfilehash: 23c4f24e430d58895eb551c3e2cb62b5f0003ac0
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75418820"
---
# <a name="view-contents-of-a-file-that-is-being-tracked-with-change-tracking"></a>Değişiklik İzleme ile izlenmekte olan bir dosyanın içeriğini görüntüleme

Dosya içeriği izleme, Değişiklik İzleme ile izlenmekte olan bir değişiklikten önce ve sonra bir dosyanın içeriğini görüntülemenizi sağlar. Bunu yapmak için, her değişiklik oluştuktan sonra dosya içeriğini bir depolama hesabına kaydeder.

## <a name="requirements"></a>Gereksinimler

* Dosya içeriğini depolamak için Kaynak Yöneticisi dağıtım modelini kullanan standart bir depolama hesabı gerekir. Premium ve klasik dağıtım modeli depolama hesapları kullanılmamalıdır. Depolama hesapları hakkında daha fazla bilgi için bkz. [Azure depolama hesapları hakkında](../storage/common/storage-create-storage-account.md)

* Kullanılan depolama hesabında yalnızca 1 Otomasyon hesabı bağlı olabilir.

* Otomasyon hesabınızda [değişiklik izleme](automation-change-tracking.md) etkinleştirilir.

## <a name="enable-file-content-tracking"></a>Dosya içeriği izlemeyi etkinleştir

1. Azure portal Otomasyon hesabınızı açın ve sonra **değişiklik izleme**' yi seçin.
2. Üstteki menüde **Ayarları Düzenle**' yi seçin.
3. **Dosya içeriği** ' ni seçin ve **bağlantı**' ya tıklayın. Bu, **değişiklik izleme Için Içerik ekleme konumunu** açar.

   ![seçin](./media/change-tracking-file-contents/enable.png)

4. Dosya içeriğini depolamak için kullanılacak aboneliği ve depolama hesabını seçin. Tüm mevcut izlenen dosyalar için dosya içeriği izlemeyi etkinleştirmek istiyorsanız, **Tüm ayarlar için dosya Içeriğini karşıya yükle** **' yi seçin.** Bunu, daha sonra her dosya yolu için değiştirebilirsiniz.

   ![depolama hesabı ayarla](./media/change-tracking-file-contents/storage-account.png)

5. Etkinleştirildikten sonra, depolama hesabı ve SAS URI 'Leri gösterilir. SAS URI 'Lerinin 365 gün sonra süre sonu olur ve yeniden **Oluştur** düğmesine tıklayarak yeniden oluşturulabilir.

   ![Hesap anahtarlarını listeleme](./media/change-tracking-file-contents/account-keys.png)

## <a name="add-a-file"></a>Dosya Ekle

Aşağıdaki adımlarda, bir dosya için değişiklik izlemeyi açma işleminde izlenecek yol gösterilmektedir:

1. **Değişiklik izleme** **Ayarları Düzenle** sayfasında, **Windows dosyaları** veya **Linux dosyaları** sekmesini seçin ve **Ekle** ' ye tıklayın.

1. Dosya yolu için bilgileri doldurun ve **Tüm ayarlar için dosya Içeriğini karşıya yükle**altında **true** ' ı seçin. Bu ayar yalnızca bu dosya yolu için dosya içeriği izlemeyi mümkün.

   ![Linux dosyası ekleme](./media/change-tracking-file-contents/add-linux-file.png)

## <a name="viewing-the-contents-of-a-tracked-file"></a>İzlenen bir dosyanın içeriğini görüntüleme

1. Dosya veya yoldaki bir dosya için bir değişiklik algılandıktan sonra portalda görüntülenir. Değişiklik listesinden dosya değişikliğini seçin. **Değişiklik ayrıntıları** bölmesi görüntülenir.

   ![liste değişiklikleri](./media/change-tracking-file-contents/change-list.png)

1. **Ayrıntıları değiştir** sayfasında, dosyanın içeriğini görmek için, sol üstteki, dosya bilgilerini görüntüle ' ye tıklayarak dosya **içeriğini görüntüle** ' ye tıklayın.

   ![değişiklik ayrıntıları](./media/change-tracking-file-contents/change-details.png)

1. Yeni sayfa, dosya içeriğini yan yana görünümde gösterir. Ayrıca, değişikliklerin satır içi görünümünü görmek için **satır içi** ' ı da seçebilirsiniz.

   ![dosya değişikliklerini görüntüle](./media/change-tracking-file-contents/view-file-changes.png)

## <a name="next-steps"></a>Sonraki adımlar

Çözümü kullanma hakkında daha fazla bilgi edinmek için Değişiklik İzleme öğreticisini ziyaret edin:

> [!div class="nextstepaction"]
> [Ortamınızdaki değişikliklerle ilgili sorunları giderme](automation-tutorial-troubleshoot-changes.md)

* Ayrıntılı değişiklik izleme verilerini görüntülemek için [Azure izleyici günlüklerinde günlük aramalarını](../log-analytics/log-analytics-log-searches.md) kullanın.

