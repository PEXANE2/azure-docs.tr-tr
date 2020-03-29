---
title: Kişiselleştirme’yi yapılandırma
description: Hizmet yapılandırması, hizmetin ödülleri nasıl ele aldığı, hizmetin ne sıklıkta araştırdığını, modelin ne sıklıkta yeniden eğitildiğini ve ne kadar veri depolanırını içerir.
ms.topic: conceptual
ms.date: 02/19/2020
ms.openlocfilehash: ac31a9f907defeb44dbd4748a4395d3aec34d30c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79219362"
---
# <a name="configure-personalizer-learning-loop"></a>Personalizer öğrenme döngüsüyapılandırma

Hizmet yapılandırması, hizmetin ödülleri nasıl ele aldığı, hizmetin ne sıklıkta araştırdığını, modelin ne sıklıkta yeniden eğitildiğini ve ne kadar veri depolanırını içerir.

Bu Personalizer kaynağı için Azure portalında **Yapılandırma** sayfasındaki öğrenme döngüsüne göre yapılandırın.

<a name="configure-service-settings-in-the-azure-portal"></a>
<a name="configure-reward-settings-for-the-feedback-loop-based-on-use-case"></a>

## <a name="configure-rewards-for-the-feedback-loop"></a>Geri bildirim döngüsü için ödülleri yapılandırma

Öğrenme döngünüzün ödülleri kullanması için hizmeti yapılandırın. Aşağıdaki değerlerdeki değişiklikler geçerli Personalizer modelini sıfırlar ve son 2 günlük verilerle yeniden eğitecektir.

> [!div class="mx-imgBorder"]
> ![Geri bildirim döngüsü için ödül değerlerini yapılandırma](media/settings/configure-model-reward-settings.png)

|Değer|Amaç|
|--|--|
|Ödül bekleme süresi|Personalizer'ın Rank çağrısının gerçekleştiği andan itibaren Rank çağrısı için ödül değerlerini topladığı süreyi ayarlar. Bu değer şu soruyu sorarak belirlenir: "Personalizer ödül çağrıları için ne kadar beklemeli?" Bu pencereden sonra gelen herhangi bir ödül günlüğe kaydedilir, ancak öğrenmek için kullanılmaz.|
|Varsayılan ödül|Rank çağrısıyla ilişkili Ödül Bekleme Süresi penceresinde Personalizer tarafından ödül çağrısı alınmazsa, Personalizer Varsayılan Ödülü atar. Varsayılan olarak ve çoğu senaryoda Varsayılan Ödül sıfırdır (0).|
|Ödül toplama|Aynı Rank API çağrısı için birden çok ödül alınırsa, bu toplama yöntemi kullanılır: **toplam** veya **en erken**. En erken alınan en erken puanı alır ve geri kalanını atar. Bu, yinelenen aramalar arasında benzersiz bir ödül istiyorsanız yararlıdır. |

Bu değerleri değiştirdikten sonra **Kaydet'i**seçtiğinizden emin olun.

## <a name="configure-exploration-to-allow-the-learning-loop-to-adapt"></a>Öğrenme döngüsünün uyum sağlaması için keşfi yapılandırın

Kişiselleştirme, eğitilmiş modelin tahminini kullanmak yerine alternatifleri keşfederek yeni desenler keşfedebilir ve kullanıcı davranışı değişikliklerine zaman içinde uyum sağlayabilir. **Keşif** değeri, Rank çağrılarının yüzde kaçının keşifle yanıtlanmasını belirler.

Bu değerdeki değişiklikler geçerli Personalizer modelini sıfırlar ve son 2 günlük verilerle yeniden eğitecektir.

![Keşif değeri, Rank çağrılarının yüzde kaçının keşifle yanıtlanmasını belirler](media/settings/configure-exploration-setting.png)

Bu değeri değiştirdikten sonra **Kaydet'i**seçtiğinizden emin olun.

<a name="model-update-frequency"></a>

## <a name="configure-model-update-frequency-for-model-training"></a>Model eğitimi için model güncelleştirme sıklığını yapılandırma

**Model güncelleştirme sıklığı,** modelin ne sıklıkta eğitildiğini ayarlar.

|Frekans ayarı|Amaç|
|--|--|
|1 dakika|Bir dakikalık güncelleştirme frekansları, Personalizer kullanarak bir uygulamanın kodunu **hata ayıklarken,** demolar yaparken veya makine öğrenimi yönlerini etkileşimli olarak test ederken yararlıdır.|
|15 dakika|Yüksek model güncelleştirme sıklıkları, kullanıcı davranışlarındaki **değişiklikleri yakından izlemek** istediğiniz durumlar için yararlıdır. Örnekler arasında canlı haberler, viral içerik veya canlı ürün teklifinde çalışan siteler verilebilir. Bu senaryolarda 15 dakikalık bir frekans kullanabilirsiniz. |
|1 saat|Çoğu kullanım için daha düşük güncelleştirme sıklığı etkilidir.|

![Model güncelleştirme sıklığı, yeni bir Personalizer modelinin ne sıklıkta yeniden eğitildiğini ayarlar.](media/settings/configure-model-update-frequency-settings-15-minutes.png)

Bu değeri değiştirdikten sonra **Kaydet'i**seçtiğinizden emin olun.

## <a name="data-retention"></a>Veri saklama

**Veri saklama süresi,** Personalizer'ın veri günlüklerini kaç gün tuttuğunu belirler. Geçmiş veri günlükleri, Kisizer'in etkinliğini ölçmek ve Öğrenme Politikası'nı optimize etmek için kullanılan [çevrimdışı değerlendirmeleri](concepts-offline-evaluation.md)gerçekleştirmek için gereklidir.

Bu değeri değiştirdikten sonra **Kaydet'i**seçtiğinizden emin olun.

<a name="clear-data-for-your-learning-loop"></a>

## <a name="settings-that-include-resetting-the-model"></a>Modeli sıfırlamayı içeren ayarlar

Aşağıdaki eylemler, son 2 günlük verilerle modelin hemen yeniden eğitilmesini içerir.

* Ödül
* Araştırma

Tüm verilerinizi [temizlemek](how-to-manage-model.md) için **Model ve öğrenme ayarlarını ** sayfasını kullanın.

## <a name="next-steps"></a>Sonraki adımlar

[Modelinizi nasıl yöneteceklerinizi öğrenin](how-to-manage-model.md)
