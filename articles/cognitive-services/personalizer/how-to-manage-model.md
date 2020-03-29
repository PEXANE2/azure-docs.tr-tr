---
title: Model ve öğrenme ayarlarını yönetme - Personalizer
description: Makinede öğrenilen model ve öğrenme ayarları, kendi kaynak kontrol sisteminizde yedekleme için dışa aktarılabilir.
ms.topic: conceptual
ms.date: 02/20/2020
ms.openlocfilehash: c544d058019c8d507f65dc6bfd854376b364abd1
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77624302"
---
# <a name="how-to-manage-model-and-learning-settings"></a>Model ve öğrenme ayarlarını yönetme

Makinede öğrenilen model ve öğrenme ayarları, kendi kaynak kontrol sisteminizde yedekleme için dışa aktarılabilir.

## <a name="export-the-personalizer-model"></a>Personalizer modelini dışa aktarma

**Model ve öğrenme ayarları**için Kaynak yönetiminin bölümünden, model oluşturma ve son güncellenen tarihi gözden geçirin ve geçerli modeli dışa aktarın. Bir model dosyasını arşivleme amacıyla dışa aktarmak için Azure portalını veya Personalizer API'lerini kullanabilirsiniz.

![Mevcut Personalizer modelini dışa aktarma](media/settings/export-current-personalizer-model.png)

## <a name="clear-data-for-your-learning-loop"></a>Öğrenme döngünüz için verileri temizleme

1. Azure portalında, Personalizer kaynağınız **için, Model ve öğrenme ayarları** sayfasında Verileri **Temizle'yi**seçin.
1. Tüm verileri temizlemek ve öğrenme döngülerini özgün duruma sıfırlamak için 3 onay kutusunun tümünü seçin.

    ![Azure portalında, Personalizer kaynağından verileri temizleyin.](./media/settings/clear-data-from-personalizer-resource.png)

    |Değer|Amaç|
    |--|--|
    |Günlüğe kaydedilmiş kişiselleştirme ve ödül verileri.|Bu günlük verileri çevrimdışı değerlendirmelerde kullanılır. Kaynağınızı sıfırlıyorsanız verileri temizleyin.|
    |Personalizer modelini sıfırla.|Bu model her yeniden eğitimde değişir. Bu eğitim sıklığı **Yapılandırma** **sayfasındaki yükleme modeli sıklığında** belirtilir. |
    |Öğrenme ilkesini varsayılan olarak ayarlayın.|Çevrimdışı değerlendirmenin bir parçası olarak öğrenme ilkesini değiştirdiyseniz, bu durum özgün öğrenme ilkesine sıfırlanır.|

1. Temizleme işlemini başlatmak için **seçili verileri** temizle'yi seçin. Durum, sağ üst gezintide Azure bildirimlerinde bildirilir.

## <a name="import-a-new-learning-policy"></a>Yeni bir öğrenme ilkesi alma

[Öğrenme ilkesi](concept-active-learning.md#understand-learning-policy-settings) ayarları, model eğitiminin _hiperparametrelerini_ belirler. Yeni bir öğrenme ilkesi bulmak için [çevrimdışı bir değerlendirme](how-to-offline-evaluation.md) gerçekleştirin.

1. Azure [portalını](https://portal.azure.com)açın ve Personalizer kaynağınızı seçin.
1. **Kaynak Yönetimi** bölümünde Model ve **öğrenme ayarlarını** seçin.
1. Alma **öğrenme ayarları** için, yukarıda belirtilen JSON formatında oluşturduğunuz dosyayı seçin ve **ardından Yükle** düğmesini seçin.

    Öğrenme ilkesinin başarıyla yüklendiği bildirimini bekleyin.

## <a name="export-a-learning-policy"></a>Bir öğrenme politikası dışa aktarma

1. Azure [portalını](https://portal.azure.com)açın ve Personalizer kaynağınızı seçin.
1. **Kaynak Yönetimi** bölümünde Model ve **öğrenme ayarlarını** seçin.
1. Learning **ayarları** için **Learning öğrenme ayarlarını** dışa aktar düğmesini seçin. Bu, dosyayı `json` yerel bilgisayarınıza kaydeder.

## <a name="next-steps"></a>Sonraki adımlar

[Öğrenme politikasını nasıl yönetiriz öğrenin](how-to-manage-model.md)
