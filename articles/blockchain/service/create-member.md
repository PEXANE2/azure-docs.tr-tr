---
title: Azure portal kullanarak bir Azure blok zinciri hizmeti blok zinciri üyesi oluşturma
description: Bir blok zinciri üyesi oluşturmak için Azure blok zinciri hizmetini kullanın.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 08/29/2019
ms.topic: quickstart
ms.service: azure-blockchain
ms.reviewer: janders
manager: femila
ms.openlocfilehash: 9f58379acd88fd93a00edc11b73b41dc3d6226a5
ms.sourcegitcommit: 6794fb51b58d2a7eb6475c9456d55eb1267f8d40
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70241035"
---
# <a name="quickstart-create-an-azure-blockchain-service-blockchain-member-using-the-azure-portal"></a>Hızlı Başlangıç: Azure portal kullanarak bir Azure blok zinciri hizmeti blok zinciri üyesi oluşturma

Azure blok zinciri hizmeti, iş mantığınızı akıllı bir sözleşme içinde yürütebilmeniz için bir blok zinciri platformudur. Bu hızlı başlangıçta, Azure portal kullanarak bir blok zinciri üyesi oluşturarak nasıl başlacağınız gösterilmektedir.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-blockchain-member"></a>Blok zinciri üyesi oluşturma

Yeni veya mevcut bir konsorsiyda çekirdek defter protokolünü çalıştıran bir blok zinciri üyesi oluşturun.

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Azure portalının sol üst köşesinde bulunan **Kaynak oluştur** öğesini seçin.
1. **Blok zinciri** > **Azure blok zinciri hizmeti**' ni seçin.
1. Şablonu doldurun.

    ![Hizmet Oluşturma](./media/create-member/create-member.png)

    Ayar | Açıklama
    --------|------------
    Blok zinciri üyesi | Azure blok zinciri hizmeti blok zinciri üyesini tanımlayan benzersiz bir ad seçin. Blok zinciri üye adı yalnızca küçük harf ve sayı içerebilir. İlk karakter bir harf olmalıdır. Değer 2 ila 20 karakter uzunluğunda olmalıdır.
    Subscription | Hizmetiniz için kullanmak istediğiniz Azure aboneliğini seçin. Birden fazla aboneliğiniz varsa kaynağın faturalandığı aboneliği seçin.
    Resource group | Yeni bir kaynak grubu adı veya aboneliğinizde var olan bir kaynak grubu.
    Bölge | Bu, konsorsiyumun tüm üyeleri için aynı olmalıdır.
    Üye hesabı parolası | Üye hesabı parolası, üyelik için oluşturulan Ethereum hesabının özel anahtarını şifrelemek için kullanılır. Konsorsiyum yönetimi için üye hesabı ve üye hesabı parolasını kullanırsınız.
    Konsorsiyum adı | Yeni bir konsorsiyum için benzersiz bir ad girin. Bir toplantıya bir davet aracılığıyla katılırken, bu değer katıldığınız konsorsiyumun değeridir.
    Açıklama | Konsorsiyumun açıklaması.
    Protocol |  Önizleme, çekirdek protokolünü destekler.
    Fiyatlandırma | Yeni hizmetinizin düğüm yapılandırması. **Standart**' ı seçin. 2 Doğrulayıcı düğümleri ve 1 işlem düğümü varsayılan ayarlardır.
    İşlem düğümü parolası | Üyenin varsayılan işlem düğümü için parola. Blok zinciri üyesinin varsayılan işlem düğümü genel uç noktasına bağlanırken temel kimlik doğrulaması için parolayı kullanın.

1. Hizmeti sağlamak için **Oluştur** ' u seçin. Sağlama yaklaşık 10 dakika sürer.
1. Dağıtım işlemini izlemek için araç çubuğunda **Bildirimler** ' i seçin.
1. Dağıtımdan sonra, blok zinciri üyesine gidin.

**Genel bakış**' ı seçtiğinizde, hizmetinize ilişkin temel bilgileri rootcontract adresi ve üye hesabı da dahil olmak üzere görüntüleyebilirsiniz.

![Blok zinciri üyesine genel bakış](./media/create-member/overview.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Sonraki hızlı başlangıç veya öğretici için oluşturduğunuz üyeyi kullanabilirsiniz. Artık gerekli değilse, Azure blok zinciri hizmeti tarafından oluşturduğunuz `myResourceGroup` kaynak grubunu silerek kaynakları silebilirsiniz.

Kaynak grubunu silmek için:

1. Azure portal sol gezinti bölmesindeki **kaynak grubu** ' na gidin ve silmek istediğiniz kaynak grubunu seçin.
2. **Kaynak grubunu sil**'i seçin. Kaynak grubu adını girip **Sil**' i seçerek silmeyi doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [MetaMask kullanarak akıllı bir sözleşmeyi bağlama ve dağıtma](connect-metamask.md)