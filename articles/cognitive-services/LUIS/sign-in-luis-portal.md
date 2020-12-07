---
title: LUIS portalında oturum açma
description: LUU portalında oturum açan yeni bir kullanıcı varsa, oturum açma deneyimi geçerli kullanıcı hesabınıza göre biraz farklılık gösterir.
services: cognitive-services
ms.custom: ''
ms.service: cognitive-services
ms.subservice: language-understanding
ms.date: 09/08/2020
ms.topic: how-to
ms.author: nitinme
author: nitinme
ms.openlocfilehash: 3235f6285edb99776b42014678cd2b6c60d17f62
ms.sourcegitcommit: d6e92295e1f161a547da33999ad66c94cf334563
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/07/2020
ms.locfileid: "96763866"
---
# <a name="sign-in-to-luis-portal"></a>LUIS portalında oturum açma

LUO portalını kullanmaya başlamak ve bir yazma kaynağı oluşturmak için bu makaleyi kullanın. Bu makaledeki adımları tamamladıktan sonra, LUO uygulamaları oluşturup yayımlayabilirsiniz.

## <a name="access-the-portal"></a>Portala erişin


1. LUO 'yu kullanmaya başlamak için, [Luo portalına](https://www.luis.ai)gidin. Henüz bir aboneliğiniz yoksa, [ücretsiz hesap](https://azure.microsoft.com//free/cognitive-services/) oluşturmanız ve portala geri dönmeniz istenir.
2. Yeni oluşturduğunuz aboneliğiniz ile güncelleştirmek için sayfayı yenileyin
3. Açılır listeden aboneliğinizi seçin

    > [!div class="mx-imgBorder"]
    > ![abonelikleri Seç](./media/migrate-authoring-key/select-subscription-sign-in-2.png)

4. Aboneliğiniz başka bir kiracıya sahip olursa, mevcut pencereden kiracılar arasında geçiş yapamazsınız. Bu pencereyi kapatarak ve üst çubukta baş harflerinizi içeren en sağdaki avatarı seçerek kiracılar arasında geçiş yapabilirsiniz. Pencereyi yeniden açmak için yukarıdan **farklı bir yazma kaynağı seçin** öğesine tıklayın.

    > [!div class="mx-imgBorder"]
    > ![dizinleri Değiştir](./media/migrate-authoring-key/switch-directories.png)

5. Aboneliğinizle ilişkili mevcut bir LUSıS yazma kaynağınız varsa, bunu açılan listeden seçin. Bu yazma kaynağı altında oluşturulan tüm uygulamaları görüntüleyebilirsiniz.
6. Aksi takdirde, bu kalıcı ' ın altında **Yeni yazma kaynağı oluştur ' a** tıklayın.
7.  Yeni bir yazma kaynağı oluştururken aşağıdaki bilgileri sağlayın:

    > [!div class="mx-imgBorder"]
    > ![Yeni kaynak oluştur](./media/migrate-authoring-key/create-new-authoring-resource-2.png)

    * **Kiracı adı** -Azure aboneliğinizin ilişkilendirildiği kiracı. Mevcut pencereden kiracılar arasında geçiş yapamazsınız. Bu pencereyi kapatarak ve ekran harflerinizi içeren ekranın sağ üst köşesindeki avatar öğesini seçerek kiracılar arasında geçiş yapabilirsiniz. Pencereyi yeniden açmak için yukarıdan **farklı bir yazma kaynağı seçin öğesini** seçin.
    * **Azure Kaynak grubu adı** -aboneliğinizde seçtiğiniz özel bir kaynak grubu adı. Kaynak grupları, erişim ve yönetim için Azure kaynaklarını gruplandıreklemenize olanak tanır. Şu anda aboneliğinizde bir kaynak grubunuz yoksa, LUı portalında bir tane oluşturmanıza izin verilmez. Bir tane oluşturmak için [Azure Portal](https://ms.portal.azure.com/#create/Microsoft.ResourceGroup) gidin ve oturum açma işlemine devam etmek için lusıs 'e gidin.
    * **Azure Kaynak adı** -seçtiğiniz özel bir ad, yazma IŞLEMLERINIZ için URL 'nin bir parçası olarak kullanılır. Kaynak adınız yalnızca alfasayısal karakterler içerebilir, `-` ile başlayamaz veya bitemez `-` . Adda başka semboller varsa, kaynak oluşturma işlemi başarısız olur.
    * **Konum** -Şu anda lubu tarafından desteklenen [üç yazma](https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions) konumundan birinde uygulamalarınızı yazmak Için seçin: Batı ABD, Batı Avrupa ve Doğu Avustralya
    * **Fiyatlandırma katmanı** varsayılan olarak, F0 Authoring fiyatlandırma katmanı, önerilen şekilde seçilir. Ek bir güvenlik katmanı arıyorsanız Azure portal [müşterinin yönettiği bir anahtar](https://docs.microsoft.com/azure/cognitive-services/luis/luis-encryption-of-data-at-rest#customer-managed-keys-for-language-understanding) oluşturun.
8. Artık LUO 'da oturumunuz başarıyla açıldı. Artık uygulama oluşturmaya başlayabilirsiniz.

## <a name="troubleshooting"></a>Sorun giderme

* Yeni bir kaynak oluştururken, kaynak adının yalnızca alfasayısal karakterler içerdiğinden, '-' olduğundan emin olun ve '-' ile başlayamaz veya bitemez. Aksi takdirde, başarısız olur.
* [Azure kaynağı oluşturmak için aboneliğinizde uygun izinlere](../../role-based-access-control/rbac-and-directory-admin-roles.md#azure-roles)sahip olduğunuzdan emin olun. Uygun izinleriniz yoksa size yeterli izin vermek için aboneliğinizin yöneticisine başvurun.

## <a name="next-steps"></a>Sonraki adımlar

* [Yeni bir uygulama başlatmayı](luis-how-to-start-new-app.md) öğrenin
