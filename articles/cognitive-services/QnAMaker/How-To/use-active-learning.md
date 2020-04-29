---
title: Bilgi Bankası Soru-Cevap Oluşturma geliştirme-
description: Etkin öğrenme ile bilgi Bankalarınızın kalitesini artırabilirsiniz. Mevcut soruları kaldırmadan veya değiştirmeden gözden geçirin, kabul edin veya reddedin, ekleyin.
ms.topic: conceptual
ms.date: 03/18/2020
ms.openlocfilehash: 33b3c547b0aea9a1e235bf8a05d01aa16b468a71
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80071138"
---
# <a name="use-active-learning-to-improve-your-knowledge-base"></a>Bilgi bankanızı geliştirmek için etkin öğrenmeyi kullanma

[Etkin öğrenme](../Concepts/active-learning-suggestions.md) , alternatif sorular önererek bilgi Bankalarınızın kalitesini artırmanıza olanak tanır. Kullanıcı Gönderimleri göz önüne alınır ve diğer sorular listesinde öneriler olarak gösterilir. Bu önerileri diğer sorular olarak ekleme veya reddetme esnekliği vardır.

Bilgi tabanınız otomatik olarak değişmez. Değişikliklerin etkili olabilmesi için önerileri kabul etmelisiniz. Bu öneriler, sorular ekler ancak mevcut soruları değiştirmez veya kaldırmaz.


## <a name="upgrade-runtime-version-to-use-active-learning"></a>Çalışma zamanı sürümünü etkin öğrenimi kullanacak şekilde yükseltme

Etkin öğrenme, çalışma zamanı sürümü 4.4.0 ve üzeri sürümlerde desteklenir. Bilgi tabanınız daha önceki bir sürümde oluşturulduysa, [çalışma alanınızı](set-up-qnamaker-service-azure.md#get-the-latest-runtime-updates) bu özelliği kullanacak şekilde yükseltin.

## <a name="turn-on-active-learning-for-alternate-questions"></a>Diğer sorular için etkin öğrenmeyi açın

Etkin öğrenme varsayılan olarak kapalıdır. Önerilen soruları görmek için etkinleştirin. Etkin öğrenmeyi etkinleştirdikten sonra, Soru-Cevap Oluşturma için istemci uygulamasından bilgi göndermeniz gerekir. Daha fazla bilgi için bkz. [GenerateAnswer kullanımı Için mimari akış ve bir bot 'Tan API 'Ler eğitme](improve-knowledge-base.md#architectural-flow-for-using-generateanswer-and-train-apis-from-a-bot).

1. Bilgi Bankası 'nı yayımlamak için **Yayımla** ' yı seçin. Etkin öğrenme sorguları yalnızca GenerateAnswer API tahmin uç noktasından toplanır. Soru-Cevap Oluşturma portalındaki test bölmesi sorguları etkin öğrenmeyi etkilemez.

1. Soru-Cevap Oluşturma portalında etkin öğrenimi açmak için sağ üst köşeye gidin, **adınızı**seçin, [**hizmet ayarları**](https://www.qnamaker.ai/UserSettings)' na gidin.

    ![Etkin öğrenimi 'nin önerilen soru alternatiflerini hizmet ayarları sayfasından açın. Sağ üst menüdeki Kullanıcı adınızı seçip hizmet ayarları ' nı seçin.](../media/improve-knowledge-base/Endpoint-Keys.png)


1. Soru-Cevap Oluşturma hizmeti bulun ve ardından **etkin öğrenimi**değiştirin.

    > [!div class="mx-imgBorder"]
    > [![Hizmet ayarları sayfasında, etkin öğrenme özelliği ' nde geçiş yapın. Özelliği geçiş yapadıysanız hizmetinizi yükseltmeniz gerekebilir.](../media/improve-knowledge-base/turn-active-learning-on-at-service-setting.png)](../media/improve-knowledge-base/turn-active-learning-on-at-service-setting.png#lightbox)

    > [!Note]
    > Önceki görüntüde yer alan tam sürüm yalnızca bir örnek olarak gösterilir. Sürümünüz farklı olabilir.

    **Etkin öğrenme** etkinleştirildikten sonra, Bilgi Bankası Kullanıcı tarafından gönderilen sorulara göre düzenli aralıklarla yeni sorular önerir. Ayarı yeniden değiştirerek **etkin öğrenmeyi** devre dışı bırakabilirsiniz.

## <a name="review-suggested-alternate-questions"></a>Önerilen alternatif soruları gözden geçirin

Her bilgi tabanının **düzenleme** sayfasında [Önerilen diğer soruları gözden geçirin](improve-knowledge-base.md) .

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bilgi bankası oluşturma](./manage-knowledge-bases.md)