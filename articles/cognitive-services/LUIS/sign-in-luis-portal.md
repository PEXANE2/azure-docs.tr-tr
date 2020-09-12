---
title: LUIS portalında oturum açma
description: LUU portalında oturum açan yeni bir kullanıcı varsa, oturum açma deneyimi geçerli kullanıcı hesabınıza göre biraz farklılık gösterir.
services: cognitive-services
ms.custom: ''
ms.service: cognitive-services
ms.date: 09/08/2020
ms.topic: how-to
ms.author: a-sakand
author: skandil
ms.openlocfilehash: 58212fc35df7f230682d4f80b5eadf6cd8ca0902
ms.sourcegitcommit: 3c66bfd9c36cd204c299ed43b67de0ec08a7b968
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2020
ms.locfileid: "90008107"
---
# <a name="sign-in-to-luis-portal"></a>LUIS portalında oturum açma

LUU portalında oturum açan yeni bir kullanıcı varsa, oturum açma deneyimi geçerli kullanıcı hesabınıza göre biraz farklılık gösterir:
  * Bir Azure aboneliğiyle ilişkili
  * Bir Azure aboneliği ile ilişkilendirilmemiş

## <a name="determine-account-type"></a>Hesap türünü belirleme

LUO portalında ilk kez oturum açtığınızda, hesap türünü öğrenmek için aşağıdaki görsel göstergeleri kullanın.

### <a name="account-without-azure-subscription"></a>Azure aboneliği olmayan hesap

Bir Azure aboneliği ile ilişkilendirilmemiş bir hesap, sağ üst gezinti çubuğunda Azure simgesine sahiptir. İlişkili hesap türüne geçiş yaptıktan sonra simge artık görünmez.

:::image type="content" source="media/sign-in/sign-in-with-account-without-azure-subscription.png" alt-text="Kısmi ekran-Azure simgesiyle HALSıS gezinti çubuğunun görüntüsü.":::

### <a name="account-with-azure-subscription"></a>Azure aboneliğine sahip hesap

Azure aboneliğiyle ilişkili bir hesap, kullanılacak aboneliğinizi ve kaynağı seçmenize olanak sağlar.

:::image type="content" source="media/sign-in/resource-selection.png" alt-text="Kısmi ekran-kaynak seçimi açılan seçim kutularıyla HALSıS portalının görüntüsü.":::

## <a name="sign-in-with-account-associated-with-an-azure-subscription"></a>Bir Azure aboneliğiyle ilişkili hesapla oturum açın

1. [Luo portalında](https://www.luis.ai) oturum açın ve kullanım koşullarını kabul edin.

1. Kaydolan iki seçeneğe sahip olursunuz:

    * Önerilen yol olan ve yakında kullanılabilen tek yol olacak bir Azure kaynağı kullanmaya devam edin. Bu yol, aboneliğinizde var olan bir kaynağı seçerek veya yeni bir kaynak oluştururken, bir Azure Authoring Resource ile LUE hesabınızı bağlayabilmeniz için izin verir. Bu, daha sonra [geçiş işlemine](luis-migration-authoring.md#what-is-migration) başlamadan önce, geçişi yapılan imzalama ile eşdeğerdir. 2 Kasım 2020 ' ye kadar tüm kullanıcıların geçirilmesi gerekecektir.

    * Başlangıç veya deneme anahtarını kullanmaya devam edin. Bu yol, herhangi bir kaynak oluşturmaya gerek kalmadan, halde Başlatıcı veya deneme kaynağı ile oturum açmanıza olanak tanır. Bu yolu seçerseniz, sonunda [Hesabınızı geçirmeniz](luis-migration-authoring.md#migration-steps) ve uygulamalarınızı bir yazma kaynağına bağlamanız gerekecektir. Bu nedenle, Azure kaynağınız ile devam ettiğiniz yolu tercih etmeniz önerilir.

    [Yazma ve başlangıç anahtarları hakkında daha fazla bilgi edinin](luis-how-to-azure-subscription.md#luis-resources). Her iki kaynak de size 1.000.000 ücretsiz yazma işlemi ve 1000 ücretsiz tahmin uç noktası işlemi sunar.

    :::image type="content" source="media/sign-in/signup-landing-page.png" alt-text="Kısmi ekran görüntüsü Language Understanding yazma kaynağı türünü seçmek için.":::

1. Mevcut bir yazma kaynağı kullan

    :::image type="content" source="media/sign-in/signup-choose-resource.png" alt-text="Kaynak yazma seçin":::

    Hala aboneliğinizde zaten bir kaynak yazıyorsanız ve oturum açma sırasında LUO hesabınızla bir ilişki oluşturduysanız, **mevcut yazma kaynağını kullan** seçeneğini belirleyin ve aşağıdaki bilgileri sağlayın:

    * **Kiracı** -Azure aboneliğinizin ilişkilendirildiği kiracı. Mevcut pencereden kiracılar arasında geçiş yapamazsınız. Üst kısımdaki baş harflerinizi içeren en sağdaki avatarını seçerek kiracılar arasında geçiş yapabilirsiniz.
    * **Abonelik adı** -kaynakla ilişkilendirilecek abonelik. Kiracınıza ait birden fazla aboneliğiniz varsa, açılan listeden istediğiniz birini seçin.
    * **Kaynak adı** -hesabınızın ilişkilendirilmesini istediğiniz yazma kaynağı.

    > [!Note]
    > Aradığınız yazma kaynağı açılan listede gri ise, farklı bir bölgesel portalda oturum açtınız demektir. [Bölgesel portalların kavramını anlayın](luis-reference-regions.md#luis-authoring-regions).

1. Yeni bir yazma kaynağı oluşturma

    :::image type="content" source="media/sign-in/signup-create-resource.png" alt-text="Yazma kaynağı oluşturma":::

    **Yeni bir yazma kaynağı oluştururken**aşağıdaki bilgileri sağlayın:

    * **Kiracı** -Azure aboneliğinizin ilişkilendirildiği kiracı. Mevcut pencereden kiracılar arasında geçiş yapamazsınız. Üst kısımdaki baş harflerinizi içeren en sağdaki avatarını seçerek kiracılar arasında geçiş yapabilirsiniz.
    * **Kaynak adı** -seçtiğiniz özel bir ad, yazma IŞLEMLERINIZ için URL 'nin bir parçası olarak kullanılır. Kaynak adınız yalnızca alfasayısal karakter içerebilir, '-', '-' ile başlayamaz veya bitemez. Adda başka semboller varsa, kaynak oluşturma işlemi başarısız olur.
    * **Abonelik adı** -kaynakla ilişkilendirilecek abonelik. Kiracınıza ait birden fazla aboneliğiniz varsa, açılan listeden istediğiniz birini seçin.
    * **Kaynak grubu** -aboneliğinizde seçtiğiniz özel bir kaynak grubu adı. Kaynak grupları, erişim ve yönetim için Azure kaynaklarını gruplandıreklemenize olanak tanır. Şu anda aboneliğinizde bir kaynak grubunuz yoksa, LUı portalında bir tane oluşturmanıza izin verilmez. Bir tane oluşturmak için [Azure Portal](https://ms.portal.azure.com/#create/Microsoft.ResourceGroup) gidin ve oturum açma işlemine devam etmek için lusıs 'e gidin.

1. Yolunuzu seçtikten sonra, "hesabınız başarılı bir şekilde geçirildi" ifadesini gösteren bir oturum açana kadar birkaç saniye sürebilir. Devam ' i seçerek son ' a **geçin**.

    :::image type="content" source="media/sign-in/signup-confirm-2.png" alt-text="Kaynak yazmayı Onayla":::

    > [!Note]
    > Portalda oturum açmak için kullandığınız bölgede bir aboneliğiniz ve en az bir yazma kaynağınız varsa, hangi yolun gidebileceğini seçme gereksinimi olmadan LUYA geçişi ve bir kaynakla ilişkili olarak oturum açmanız gerekir.


## <a name="sign-in-with-user-account-not-associated-with-an-azure-subscription"></a>Bir Azure aboneliğiyle ilişkili olmayan kullanıcı hesabıyla oturum açın

1. [Luo portalında](https://www.luis.ai) oturum açın ve kullanım koşullarını kabul ettiğinizi kontrol edin.

1. Devam ' i seçerek son ' a **geçin**. Deneme/başlangıç anahtarıyla otomatik olarak oturum açacaksınız. Bu, sonunda [Hesabınızı geçirmeniz](luis-migration-authoring.md#migration-steps) ve uygulamalarınızı bir yazma kaynağına bağlamanız gerektiği anlamına gelir. Geçiş işlemine dönmek için [Azure Ücretsiz deneme sürümü](https://azure.microsoft.com/free/)için oturum açmanız gerekir.

    :::image type="content" source="media/sign-in/signup-no-subscription.png" alt-text="Abonelik senaryosu yok":::

## <a name="troubleshooting"></a>Sorun giderme

* Oturum açarken kullandığınız portaldan farklı bir bölgedeki Azure portal bir yazma kaynağı oluşturursanız, yazma kaynağı gri kalır.
* Yeni bir kaynak oluştururken, kaynak adının yalnızca alfasayısal karakterler içerdiğinden, '-' olduğundan emin olun ve '-' ile başlayamaz veya bitemez. Aksi takdirde, başarısız olur.
* [Azure kaynağı oluşturmak için aboneliğinizde uygun izinlere](../../role-based-access-control/rbac-and-directory-admin-roles.md#azure-roles)sahip olduğunuzdan emin olun. Uygun izinleriniz yoksa size yeterli izin vermek için aboneliğinizin yöneticisine başvurun.

## <a name="next-steps"></a>Sonraki adımlar

* [Yeni bir uygulama başlatmayı](luis-how-to-start-new-app.md) öğrenin
