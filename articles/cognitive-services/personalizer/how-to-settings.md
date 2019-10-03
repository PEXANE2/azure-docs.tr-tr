---
title: Ayarları yapılandırma-kişiselleştirici
titleSuffix: Azure Cognitive Services
description: Hizmet yapılandırması, hizmetin yeniden nasıl davrandığını, hizmetin ne sıklıkla incelendiğine, modelin ne sıklıkta geri alınacağını ve ne kadar veri depolandığını içerir.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: conceptual
ms.date: 09/30/2019
ms.author: diberry
ms.openlocfilehash: bad581fbc53292b5a7c25157ef839e07f33e131e
ms.sourcegitcommit: 7c2dba9bd9ef700b1ea4799260f0ad7ee919ff3b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71827887"
---
# <a name="personalizer-settings"></a>Kişiselleştirici ayarları

Hizmet yapılandırması, hizmetin yeniden nasıl davrandığını, hizmetin ne sıklıkla incelendiğine, modelin ne sıklıkta geri alınacağını ve ne kadar veri depolandığını içerir.

## <a name="create-personalizer-resource"></a>Kişiselleştirici kaynağı oluştur

Her geri bildirim döngüsü için bir kişiselleştirici kaynağı oluşturun. 

1. [Azure portalda](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesPersonalizer) oturum açın. Önceki bağlantı sizi, kişiselleştirici hizmeti için **Oluştur** sayfasına götürür. 
1. Hizmet adınızı girin, bir abonelik, konum, fiyatlandırma katmanı ve kaynak grubu seçin.
1. Onayı seçin ve **Oluştur**' u seçin.

## <a name="configure-service-settings-in-the-azure-portal"></a>Azure portal hizmet ayarlarını yapılandırma

1. [Azure Portal](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesPersonalizer)’ında oturum açın.
1. Kişiselleştirici kaynağını bulun. 
1. **Kaynak yönetimi** bölümünde **Ayarlar**' ı seçin.

    Azure portal çıkmadan önce, **anahtarlar** sayfasından kaynak anahtarınızdan birini kopyalayın. Bu, [kişiselleştirici SDK 'yı](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.personalizer)kullanmak için gereklidir.

### <a name="configure-reward-settings-for-the-feedback-loop-based-on-use-case"></a>Kullanım örneğine göre geri bildirim döngüsü için yeniden yapılandırma ayarlarını yapılandırın

Geri bildirim döngünüz için hizmetin ayarlarını yapılandırın. Aşağıdaki ayarlarda yapılan değişiklikler geçerli kişiselleştirici modeli sıfırlar ve son 2 güne kadar verileri yeniden eğitecektir:

![Geri bildirim döngüsü için yeniden yapılandırma ayarlarını yapılandırın](media/settings/configure-model-reward-settings.png)

|Ayar|Amaç|
|--|--|
|Bekleme süresi|Bir derecelendirme çağrısı için Kişiselleştiriciye ait değer toplama işleminin ne kadar süre olacağını belirleyen süreyi ayarlar. Bu değer şu sorarak ayarlanır: "kişiselleştirici, yeniden ödüller için ne kadar bekleneceği?" Bu pencere günlüğe yazılır, ancak öğrenimi için kullanılmaz.|
|Varsayılan değer|Bir derecelendirme çağrısıyla ilişkili bir yeniden bekleme süresi penceresi sırasında kişiselleştirici tarafından hiçbir yeniden çağrı alınmıyorsa, kişiselleştirici varsayılan bir ödül atar. Varsayılan olarak ve çoğu senaryoda varsayılan değer sıfırdır.|
|Yeniden toplama|Aynı derecelendirme API çağrısı için birden fazla yeniden yer alınmışsa, bu toplama yöntemi kullanılır: **Sum** veya **en erken**. En erken alınan puanı seçer ve kalanı atar. Muhtemelen yinelenen çağrılar arasında benzersiz bir ödül istiyorsanız bu yararlı olur. |

Bu ayarları değiştirdikten sonra **Kaydet**' i seçtiğinizden emin olun.

### <a name="exploration-setting"></a>Araştırma ayarı 

Kişiselleştirme, yeni desenleri bulabilir ve alternatifleri inceleyerek zamanla Kullanıcı davranışı değişikliklerine uyum sağlayabilir. **Araştırma** ayarı, arama Ile hangi derece çağrı yüzdesinin yanıtlandığını belirler. 

Bu ayarda yapılan değişiklikler geçerli kişiselleştirici modelini sıfırlar ve son 2 güne kadar yeniden eğitecektir.

![Araştırma ayarı, araştırma ile hangi derece çağrı yüzdesinin yanıtlandığını belirler](media/settings/configure-exploration-setting.png)

Bu ayarı değiştirdikten sonra **Kaydet**' i seçtiğinizden emin olun.

### <a name="model-update-frequency"></a>Model güncelleştirme sıklığı

Her etkin olaydan gelen API çağrılarından eğitilen en son model, kişiselleştirici derecelendirme çağrısı tarafından otomatik olarak kullanılmaz. **Model güncelleştirme sıklığı** , derece çağrısı tarafından kullanılan modelin ne sıklıkta güncelleştirildiğini ayarlar. 

Yüksek model güncelleştirme frekansları, Kullanıcı davranışlardaki değişiklikleri yakından izlemek istediğiniz durumlar için yararlıdır. Örnek olarak canlı haberler, viral içerik veya canlı Ürün Deklarasyon üzerinde çalışan siteler bulunur. Bu senaryolarda 15 dakikalık bir sıklık kullanabilirsiniz. Çoğu kullanım durumu için, daha düşük bir güncelleştirme sıklığı etkilidir. Tek dakikalık güncelleştirme frekansları, bir uygulama kodunun kişiselleştirici kullanılarak hata ayıklaması, tanıtımlar yapılması veya makine öğrenimi yönlerini etkileşimli olarak test edilmesi için yararlıdır.

![Model güncelleştirme sıklığı, yeni bir kişiselleştirici modelin ne sıklıkta geri alınacağını ayarlar.](media/settings/configure-model-update-frequency-settings-15-minutes.png)

Bu ayarı değiştirdikten sonra **Kaydet**' i seçtiğinizden emin olun.

### <a name="data-retention"></a>Veri saklama

**Veri saklama süresi** , kişiselleştirmede kaç gün veri günlüğü tutacağını ayarlar. Son veri günlükleri, kişiselleştirici 'nin verimliliğini ölçmek ve öğrenme Ilkesini iyileştirmek için kullanılan [çevrimdışı değerlendirmeler](concepts-offline-evaluation.md)gerçekleştirmek için gereklidir.

Bu ayarı değiştirdikten sonra **Kaydet**' i seçtiğinizden emin olun.

## <a name="export-the-personalizer-model"></a>Kişiselleştirici modelini dışarı aktarma

**Model ve ilke**için kaynak yönetiminin bölümünde, model oluşturma ve son güncelleme tarihi ' ni gözden geçirin ve geçerli modeli dışarı aktarın. Arşiv amaçlarıyla bir model dosyasını dışarı aktarmak için Azure portal veya kişiselleştirici API 'Leri kullanabilirsiniz. 

![Geçerli kişiselleştirici modelini dışarı aktar](media/settings/export-current-personalizer-model.png)

## <a name="import-and-export-learning-policy"></a>Öğrenme ilkesini içeri ve dışarı aktarma

**Model ve ilke**için kaynak yönetiminin bölümünde yeni bir öğrenme ilkesi içeri aktarın veya geçerli öğrenme ilkesini dışarı aktarın.
Önceki dışarı aktarımlardan öğrenme ilkesi dosyalarını alabilir veya çevrimdışı değerlendirmeler sırasında bulunan en iyi duruma getirilmiş ilkeleri karşıdan yükleyebilirsiniz. Bu dosyalarda el ile yapılan değişikliklerin yapılması, makine öğrenimi performansını ve çevrimdışı değerlendirmelerinin doğruluğunu etkiler ve Microsoft, makine öğrenimi ve değerlendirmelerinin doğruluğunu veya el ile düzenlenen ilkelerden kaynaklanan hizmet özel durumlarını da aşamaz.

## <a name="clear-data-for-your-learning-loop"></a>Öğrenme döngünüz için verileri temizleme

1. Azure portal, kişiselleştirici kaynağınız için **model ve ilke** sayfasında, **verileri temizle**' yi seçin.
1. Tüm verileri temizlemek ve öğrenme döngüsünü özgün durumuna sıfırlamak için 3 onay kutusunu seçin.

    ![Azure portal, kişiselleştirici kaynaktaki verileri temizleyin.](./media/settings/clear-data-from-personalizer-resource.png)

    |Ayar|Amaç|
    |--|--|
    |Günlüğe kaydedilen kişiselleştirme ve yeniden dengeleme verileri.|Bu günlüğe kaydetme verileri, çevrimdışı değerlendirmelere göre kullanılır. Kaynağı sıfırlarken verileri temizleyin.|
    |Kişiselleştirici modelini sıfırlayın.|Bu model her yeniden eğitimine göre değişir. Bu eğitim frekansı, **Ayarlar** sayfasında **model yükleme sıklığı** ' nda belirtilmiştir. |
    |Öğrenme ilkesini varsayılan olarak ayarlayın.|Öğrenme ilkesini çevrimdışı değerlendirmenin bir parçası olarak değiştirdiyseniz, bu, özgün öğrenme ilkesine sıfırlanır.|

1. Temizleme işlemini başlatmak için **seçili verileri temizle** ' yi seçin. Durum, Azure bildirimlerinde sağ üst gezinti bölmesinde raporlanır. 

## <a name="next-steps"></a>Sonraki adımlar

<!--
[How to use the Personalizer container](https://go.microsoft.com/fwlink/?linkid=2083923&clcid=0x409)
-->
[Bölge kullanılabilirliği hakkında bilgi edinin](https://azure.microsoft.com/global-infrastructure/services/?products=cognitive-services)
