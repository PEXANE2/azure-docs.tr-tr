---
title: Bilgi tabanını geliştirin - QnA Maker
description: Aktif öğrenme ile bilgi tabanınızın kalitesini artırın. Varolan soruları kaldırmadan veya değiştirmeden gözden geçirin, kabul edin veya reddedin.
ms.topic: conceptual
ms.date: 03/18/2020
ms.openlocfilehash: 33b3c547b0aea9a1e235bf8a05d01aa16b468a71
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80071138"
---
# <a name="use-active-learning-to-improve-your-knowledge-base"></a>Bilgi bankanızı geliştirmek için etkin öğrenmeyi kullanma

[Aktif öğrenme,](../Concepts/active-learning-suggestions.md) alternatif sorular önererek bilgi tabanınızın kalitesini artırmanızı sağlar. Kullanıcı gönderimleri dikkate alınır ve alternatif soru listesinde öneri olarak gösterilmektedir. Bu önerileri alternatif soru olarak ekleme veya reddetme esnekliğine sahipsiniz.

Bilgi tabanınız otomatik olarak değişmez. Herhangi bir değişikliğin etkili olması için önerileri kabul etmelisiniz. Bu öneriler soru ekler, ancak varolan soruları değiştirmez veya kaldırmaz.


## <a name="upgrade-runtime-version-to-use-active-learning"></a>Etkin öğrenmeyi kullanmak için çalışma zamanı sürümünü yükseltme

Active Learning çalışma zamanı sürüm 4.4.0 ve üzeri desteklenir. Bilgi tabanınız daha önceki bir sürümde oluşturulduysa, bu özelliği kullanmak için [çalışma sürenizi yükseltin.](set-up-qnamaker-service-azure.md#get-the-latest-runtime-updates)

## <a name="turn-on-active-learning-for-alternate-questions"></a>Alternatif sorular için etkin öğrenmeyi açın

Etkin öğrenme varsayılan olarak kapalıdır. Önerilen soruları görmek için açın. Etkin öğrenmeyi açtıktan sonra, istemci uygulamasından QnA Maker'a bilgi göndermeniz gerekir. Daha fazla bilgi [için, bir bottan GenerateAnswer ve Train API'lerini kullanmak için Mimari akışa](improve-knowledge-base.md#architectural-flow-for-using-generateanswer-and-train-apis-from-a-bot)bakın.

1. Bilgi tabanını yayımlamak için **Yayımla'yı** seçin. Etkin öğrenme sorguları yalnızca GenerateAnswer API tahmin bitiş noktasından toplanır. QnA Maker portalındaki Test bölmesine yapılan sorgular etkin öğrenmeyi etkilemez.

1. QnA Maker portalında aktif öğrenmeyi açmak için sağ üst köşeye gidin, **Adınızı**seçin, [**Servis Ayarları'na**](https://www.qnamaker.ai/UserSettings)gidin.

    ![Hizmet ayarları sayfasından etkin öğrenmenin önerilen soru alternatiflerini açın. Sağ üst menüde kullanıcı adınızı seçin ve ardından Hizmet Ayarları'nı seçin.](../media/improve-knowledge-base/Endpoint-Keys.png)


1. QnA Maker hizmetini bulun ve ardından **Aktif Öğrenme'yi geçiştirin.**

    > [!div class="mx-imgBorder"]
    > [![Hizmet ayarları sayfasında, Etkin Öğrenme özelliğini değiştirin. Özelliği geçiştiremezseniz, hizmetinizi yükseltmeniz gerekebilir.](../media/improve-knowledge-base/turn-active-learning-on-at-service-setting.png)](../media/improve-knowledge-base/turn-active-learning-on-at-service-setting.png#lightbox)

    > [!Note]
    > Önceki resimdeki tam sürüm yalnızca örnek olarak gösterilir. Sizin yorumunuz farklı olabilir.

    **Aktif Öğrenme** etkinleştirildikten sonra, bilgi tabanı kullanıcı tarafından gönderilen sorulara dayalı olarak düzenli aralıklarla yeni sorular önerir. **Ayarı** yeniden değiştirerek Active Learning'i devre dışı kullanabilirsiniz.

## <a name="review-suggested-alternate-questions"></a>Önerilen alternatif soruları gözden geçirme

Her bilgi tabanının **Edit** sayfasında [önerilen alternatif soruları gözden geçirin.](improve-knowledge-base.md)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bilgi bankası oluşturma](./manage-knowledge-bases.md)