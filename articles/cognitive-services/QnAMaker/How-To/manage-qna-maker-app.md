---
title: Soru-Cevap Oluşturma uygulamayı yönetme-Soru-Cevap Oluşturma
description: Soru-Cevap Oluşturma, bir Bilgi Bankası 'nda birden çok kişinin işbirliği yapmasına izin verir. Soru-Cevap Oluşturma, etkin öğrenme ile bilgi Bankalarınızın kalitesini artırmak için bir özellik sunar. Bunlardan biri, mevcut soruları kaldırmadan veya değiştirmeden gözden geçirebilir, kabul edebilir veya reddedebilir ve ekleyebilir.
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: conceptual
ms.date: 11/09/2020
ms.openlocfilehash: 9c042d044f5ceba5a64d6bd7dfefa34bbc69b107
ms.sourcegitcommit: 9eda79ea41c60d58a4ceab63d424d6866b38b82d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/30/2020
ms.locfileid: "96353213"
---
# <a name="manage-qna-maker-app"></a>Soru-Cevap Oluşturma uygulamayı yönetme

Soru-Cevap Oluşturma, işbirmci rolüne bağlı olarak ortak çalışan erişimini kısıtlamak için bir yetenek sunarak farklı yazarlar ve içerik düzenleyicilerle işbirliği yapmanızı sağlar.
[Soru-cevap oluşturma ortak çalışan kimlik doğrulama kavramları](../Concepts/role-based-access-control.md)hakkında daha fazla bilgi edinin.

Ayrıca, [etkin öğrenme](../Concepts/active-learning-suggestions.md)aracılığıyla alternatif sorular önererek bilgi Bankalarınızın kalitesini geliştirebilirsiniz. Kullanıcı Gönderimleri göz önüne alınır ve alternatif sorular listesinde öneri olarak görünür. Bu önerileri diğer sorular olarak ekleme veya reddetme esnekliği vardır.

Bilgi tabanınız otomatik olarak değişmez. Değişikliklerin etkili olabilmesi için önerileri kabul etmelisiniz. Bu öneriler, sorular ekler ancak mevcut soruları değiştirmez veya kaldırmaz.

## <a name="add-azure-role-based-access-control-azure-rbac"></a>Azure rol tabanlı erişim denetimi (Azure RBAC) ekleme

Soru-Cevap Oluşturma, birden fazla kişinin aynı Soru-Cevap Oluşturma kaynaktaki tüm bilgi tabanlarında işbirliği yapmasına izin verir. Bu özellik [Azure rol tabanlı erişim denetimi (Azure RBAC)](../../../role-based-access-control/role-assignments-portal.md)ile birlikte sağlanır.

## <a name="access-at-the-qna-maker-resource-level"></a>Soru-Cevap Oluşturma kaynak düzeyinde erişim

Soru-Cevap Oluşturma hizmetinde belirli bir Bilgi Bankası 'nı paylaşamazsınız. Daha ayrıntılı erişim denetimi istiyorsanız bilgi tabanlarınızı farklı Soru-Cevap Oluşturma kaynaklarına dağıtmayı ve ardından her kaynağa roller eklemeyi düşünün.

## <a name="add-a-role-to-a-resource"></a>Kaynağa rol ekleme

### <a name="add-a-user-account-to-the-qna-maker-resource"></a>Soru-Cevap Oluşturma kaynağına bir kullanıcı hesabı ekleme

Aşağıdaki adımlarda ortak çalışan rolü kullanılır, ancak bu adımlar kullanılarak [Roller](../reference-role-based-access-control.md) eklenebilir

1. [Azure](https://portal.azure.com/) portalında oturum açın ve soru-cevap oluşturma kaynağına gidin.

    ![Soru-Cevap Oluşturma kaynak listesi](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-resource-list.png)

1. **Access Control (IAM)** sekmesine gidin.

    ![Soru-Cevap Oluşturma ıAM](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-iam.png)

1. **Add (Ekle)** seçeneğini belirleyin.

    ![Soru-Cevap Oluşturma ıAM ekleme](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-iam-add.png)

1. Aşağıdaki listeden bir rol seçin:

    |Rol|
    |--|
    |Sahip|
    |Katılımcı|
    |Bilişsel hizmetler Soru-Cevap Oluşturma okuyucu|
    |Bilişsel hizmetler Soru-Cevap Oluşturma Düzenleyicisi|
    |Bilişsel hizmetler kullanıcısı|

    :::image type="content" source="../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-add-role-iam.png" alt-text="Soru-Cevap Oluşturma ıAM rol Ekle.":::

1. Kullanıcının e-posta adresini girin ve **Kaydet**' e basın.

    ![Soru-Cevap Oluşturma ıAM e-posta Ekle](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-iam-add-email.png)

### <a name="view-qna-maker-knowledge-bases"></a>Bilgi temellerini görüntüleyin Soru-Cevap Oluşturma

Soru-Cevap Oluşturma hizmetinizi paylaştığınız kişi [soru-cevap oluşturma portalında](https://qnamaker.ai)oturum açtıklarında, bu hizmet içindeki tüm bilgi temellerini kendi rollerine göre görebilirler.

Bir Bilgi Bankası seçerken, bu Soru-Cevap Oluşturma kaynaktaki geçerli rolleri Bilgi Bankası adının yanında görünür.

:::image type="content" source="../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-knowledge-base-role-name.png" alt-text="Web sayfasının sol üst köşesinde bulunan Bilgi Bankası adı ' nın yanında, parantez içinde rol adı olan düzenleme modunda Bilgi Bankası 'nın ekran görüntüsü.":::

## <a name="upgrade-runtime-version-to-use-active-learning"></a>Çalışma zamanı sürümünü etkin öğrenimi kullanacak şekilde yükseltme

# <a name="qna-maker-ga-stable-release"></a>[Soru-Cevap Oluşturma GA (kararlı sürüm)](#tab/v1)

Etkin öğrenme, çalışma zamanı sürümü 4.4.0 ve üzeri sürümlerde desteklenir. Bilgi tabanınız daha önceki bir sürümde oluşturulduysa, [çalışma alanınızı](set-up-qnamaker-service-azure.md#get-the-latest-runtime-updates) bu özelliği kullanacak şekilde yükseltin.

# <a name="qna-maker-managed-preview-release"></a>[Soru-Cevap Oluşturma Managed (Önizleme sürümü)](#tab/v2)

Soru-Cevap Oluşturma yönetilen (Önizleme) sürümünde, çalışma zamanı Soru-Cevap Oluşturma hizmetin kendisi tarafından barındırıldığından, çalışma zamanını el ile yükseltmeniz gerekmez.

---

## <a name="turn-on-active-learning-for-alternate-questions"></a>Diğer sorular için etkin öğrenmeyi açın

# <a name="qna-maker-ga-stable-release"></a>[Soru-Cevap Oluşturma GA (kararlı sürüm)](#tab/v1)

Etkin öğrenme varsayılan olarak kapalıdır. Önerilen soruları görmek için etkinleştirin. Etkin öğrenmeyi etkinleştirdikten sonra, Soru-Cevap Oluşturma için istemci uygulamasından bilgi göndermeniz gerekir. Daha fazla bilgi için bkz. [GenerateAnswer kullanımı Için mimari akış ve bir bot 'Tan API 'Ler eğitme](improve-knowledge-base.md#architectural-flow-for-using-generateanswer-and-train-apis-from-a-bot).

1. Bilgi Bankası 'nı yayımlamak için **Yayımla** ' yı seçin. Etkin öğrenme sorguları yalnızca GenerateAnswer API tahmin uç noktasından toplanır. Soru-Cevap Oluşturma portalındaki test bölmesi sorguları etkin öğrenmeyi etkilemez.

1. Soru-Cevap Oluşturma portalında etkin öğrenimi açmak için sağ üst köşeye gidin, **adınızı** seçin, [**hizmet ayarları**](https://www.qnamaker.ai/UserSettings)' na gidin.

    ![Etkin öğrenimi 'nin önerilen soru alternatiflerini hizmet ayarları sayfasından açın. Sağ üst menüdeki Kullanıcı adınızı seçip hizmet ayarları ' nı seçin.](../media/improve-knowledge-base/Endpoint-Keys.png)


1. Soru-Cevap Oluşturma hizmeti bulun ve ardından **etkin öğrenimi** değiştirin.

    > [!div class="mx-imgBorder"]
    > [![Hizmet ayarları sayfasında, etkin öğrenme özelliği ' nde geçiş yapın. Özelliği geçiş yapadıysanız hizmetinizi yükseltmeniz gerekebilir.](../media/improve-knowledge-base/turn-active-learning-on-at-service-setting.png)](../media/improve-knowledge-base/turn-active-learning-on-at-service-setting.png#lightbox)
    > [!Note]
    > Önceki görüntüde yer alan tam sürüm yalnızca bir örnek olarak gösterilir. Sürümünüz farklı olabilir.
    **Etkin öğrenme** etkinleştirildikten sonra, Bilgi Bankası Kullanıcı tarafından gönderilen sorulara göre düzenli aralıklarla yeni sorular önerir. Ayarı yeniden değiştirerek **etkin öğrenmeyi** devre dışı bırakabilirsiniz.

# <a name="qna-maker-managed-preview-release"></a>[Soru-Cevap Oluşturma Managed (Önizleme sürümü)](#tab/v2)

Varsayılan olarak, etkin öğrenme Soru-Cevap Oluşturma **yönetilmektedir (** Önizleme). Önerilen alternatif soruları görmek için düzenleme sayfasında [Görünüm seçeneklerini kullanın](../How-To/improve-knowledge-base.md#view-suggested-questions) .

---

## <a name="review-suggested-alternate-questions"></a>Önerilen alternatif soruları gözden geçirin

Her bilgi tabanının **düzenleme** sayfasında [Önerilen diğer soruları gözden geçirin](improve-knowledge-base.md) .

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bilgi Bankası oluşturma](./manage-knowledge-bases.md)