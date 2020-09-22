---
title: Azure Güvenlik Merkezi güvenlik önerilerini ve güvenli puanı bir kaynağı muaf tutma
description: Bir kaynağı güvenlik önerilerinden ve güvenli puanın dışında bırakma hakkında bilgi edinin
author: memildin
ms.author: memildin
ms.date: 9/12/2020
ms.topic: how-to
ms.service: security-center
manager: rkarlin
ms.openlocfilehash: b4d043812800bec1070624382755aeb355ea595a
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90941970"
---
# <a name="exempt-a-resource-from-recommendations-and-secure-score"></a>Önerilerden ve güvenli puanın kaynağını muaf tutma

Her güvenlik ekibinin temel önceliği, analistlerin kuruluşa bağlı görevlere ve olaylara odaklanabildiğinden emin olmanıza çalışıyor. Güvenlik Merkezi, en çok önceliklendirilmesini ve güvenli puanınızın kuruluşunuzun güvenlik kararlarının geçerli bir yansıması olmasını sağlamak için birçok özelliğe sahiptir. Kaynakları muaf tutmak bu tür bir özelliktir.

Azure Güvenlik Merkezi 'nde bir güvenlik önerisi araştırdığınızda, gözden geçirenlerin ilk bilgi parçalarından biri etkilenen kaynakların listesidir.

Bazen, dahil edilmemeniz gerektiğini belirten bir kaynak listelenecektir. Güvenlik Merkezi tarafından izlenmeyen bir işlem tarafından düzeltilebilir olabilir. Ya da kuruluşunuz söz konusu kaynak için riski kabul etmeye karar verdi. 

Böyle durumlarda, bir istisna kuralı oluşturabilir ve kaynağın gelecekte sağlıksız kaynaklarla listelenmemesini sağlayabilir ve güvenli puanınızı etkilemez. 

Kaynak geçerli değil olarak listelenecektir ve bu nedenle seçtiğiniz gerekçe içeren "muaf tutulan" olarak gösterilir.

## <a name="availability"></a>Kullanılabilirlik

|Görünüş|Ayrıntılar|
|----|:----|
|Yayın durumu:|Önizleme|
|Fiyat|Bu, Azure Defender müşterileri için ek ücret ödemeden sunulan bir Premium Azure ilke özelliğidir. Diğer kullanıcılar için ücretler gelecekte uygulanabilir.|
|Gerekli roller ve izinler:|Muafiyet oluşturmak için **abonelik sahibi** veya **ilke katılımcısı**<br>Bir kural oluşturmak için Azure Ilkesinde ilkeleri düzenleme izinlerinizin olması gerekir.<br>[Azure Ilkesinde RBAC izinlerinde](../governance/policy/overview.md#rbac-permissions-in-azure-policy)daha fazla bilgi edinin.|
|Larının|![Yes](./media/icons/yes-icon.png) Ticari bulutlar<br>![Hayır](./media/icons/no-icon.png) Ulusal/Sogeign (US Gov, Çin gov, diğer gov)|
|||


## <a name="create-an-exemption-rule"></a>Muafiyet kuralı oluşturma

1. Sağlıksız kaynaklar listesinde, muaf tutmak istediğiniz kaynak için üç nokta menüsünü ("...") seçin.

    :::image type="content" source="./media/exempt-resource/create-exemption.png" alt-text="Bağlam menüsünden muafiyet seçeneği oluştur":::

    Muafiyet oluştur bölmesi açılır.

    :::image type="content" source="./media/exempt-resource/exemption-rule-options.png" alt-text="Muafiyet bölmesi oluştur":::

1. Ölçütlerinizi girin ve bu kaynağın neden dışlandığı için bir ölçüt seçin:
    - **Azaltıldığında** , bu sorun, önerilenden farklı bir araç veya işlem tarafından işlendiği için kaynakla ilgili değildir
    - **Feragat aldığınız** -bu kaynak için risk kabul ediliyor
1. **Kaydet**’i seçin.
1. Bir süre sonra (24 saate kadar sürebilir):
    - Kaynak, güvenli puanınızı etkilemez.
    - Kaynak, öneri Ayrıntıları sayfasının **geçerli değil** sekmesinde listelenir
    - Öneri Ayrıntıları sayfasının en üstündeki bilgi şeridi, muaf tutulan kaynakların sayısını listeler:
        
        :::image type="content" source="./media/exempt-resource/info-banner.png" alt-text="Muaf tutulan kaynak sayısı":::

1. Muaf tutulan kaynaklarınızı gözden geçirmek için, **geçerli değil** sekmesini açın.

    :::image type="content" source="./media/exempt-resource/modifying-exemption.png" alt-text="Muafiyeti değiştirme":::

    Her bir istisna için nedeni tabloya dahil edilir. 

    Bir istisnayı değiştirmek veya silmek için (2) gösterildiği gibi üç nokta menüsünü ("...") seçin.


## <a name="review-all-of-the-exemption-rules-on-your-subscription"></a>Aboneliğinizdeki tüm muafiyet kurallarını gözden geçirin

İstisna kuralları, ilke atamasında kaynak için bir istisna oluşturmak üzere Azure ilkesi 'ni kullanır.

**Muafiyet** sayfasında tüm muafiyetinizi Izlemek Için Azure ilkesi 'ni kullanabilirsiniz:

:::image type="content" source="./media/exempt-resource/policy-page-exemption.png" alt-text="Azure Ilkesinin muafiyet sayfası":::



## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, güvenli puanınızı etkilememesi için bir kaynağı öneriden muaf tutmak öğrendiniz. Güvenli Puanlama hakkında daha fazla bilgi için bkz.

- [Azure Güvenlik Merkezi 'nde güvenli puan](secure-score-security-controls.md)