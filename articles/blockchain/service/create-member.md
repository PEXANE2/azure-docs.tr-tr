---
title: Azure blok zinciri hizmeti üyesi oluşturma-Azure portal
description: Azure portal kullanarak bir blok zinciri Konsorsiyumu için Azure blok zinciri hizmeti üyesi oluşturun.
ms.date: 11/18/2019
ms.topic: quickstart
ms.reviewer: janders
ms.openlocfilehash: 90f859e4d2e2621afad9cab11d66e81018bd3147
ms.sourcegitcommit: 12d902e78d6617f7e78c062bd9d47564b5ff2208
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/24/2019
ms.locfileid: "74455788"
---
# <a name="quickstart-create-an-azure-blockchain-service-blockchain-member-using-the-azure-portal"></a>Hızlı başlangıç: Azure portal kullanarak bir Azure blok zinciri hizmeti blok zinciri üyesi oluşturma

Bu hızlı başlangıçta, Azure blok zinciri hizmeti 'nde Azure portal kullanarak yeni bir blok zinciri üyesi ve konsorsiyu dağıtırsınız.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-blockchain-member"></a>Blok zinciri üyesi oluşturma

Yeni veya mevcut bir konsorsiyda çekirdek defter protokolünü çalıştıran bir blok zinciri üyesi oluşturun.

1. [Azure portalında](https://portal.azure.com) oturum açın.
1. Azure portalının sol üst köşesinde bulunan **Kaynak oluştur** öğesini seçin.
1. **Azure blok zinciri hizmeti (Önizleme)**  > **blok zinciri** seçin.

    ![Hizmet oluştur](./media/create-member/create-member.png)

    Ayar | Açıklama
    --------|------------
    Abonelik | Hizmetiniz için kullanmak istediğiniz Azure aboneliğini seçin. Birden fazla aboneliğiniz varsa kaynağın faturalandığı aboneliği seçin.
    Kaynak grubu | Yeni bir kaynak grubu adı oluşturun veya aboneliğinizden mevcut bir tane seçin.
    Bölge | Üyeyi oluşturmak için bir bölge seçin. Konsorsiyumun tüm üyeleri aynı konumda olmalıdır.
    Protokol | Şu anda Azure blok zinciri hizmeti önizlemesi çekirdek protokolünü destekliyor.
    Konsorsiyum | Yeni bir konsorsiyum için benzersiz bir ad girin. Bir toplantıya bir davet aracılığıyla katılırken katıldığınız Konsorsiyumu seçin.
    Name | Azure blok zinciri hizmeti üyesi için benzersiz bir ad seçin. Blok zinciri üye adı yalnızca küçük harf ve sayı içerebilir. İlk karakter bir harf olmalıdır. Değer 2 ila 20 karakter uzunluğunda olmalıdır.
    Üye hesabı parolası | Üye hesabı parolası, üyelik için oluşturulan Ethereum hesabının özel anahtarını şifrelemek için kullanılır. Konsorsiyum yönetimi için üye hesabı ve üye hesabı parolasını kullanırsınız.
    Açıklama | Konsorsiyumun açıklaması.
    Fiyatlandırma | Yeni hizmetinizin düğüm yapılandırması ve maliyeti. **Standart** ve **temel** katmanlar arasında seçim yapmak için **değişiklik** bağlantısını seçin.
    Düğüm parolası | Üyenin varsayılan işlem düğümü için parola. Blok zinciri üyesinin varsayılan işlem düğümü genel uç noktasına bağlanırken temel kimlik doğrulaması için parolayı kullanın.

1. Ayarlarınızı doğrulamak için **gözden geçir + oluştur** ' u seçin. Hizmeti sağlamak için **Oluştur** ' u seçin. Sağlama yaklaşık 10 dakika sürer.
1. Dağıtım işlemini izlemek için araç çubuğunda **Bildirimler** ' i seçin.
1. Dağıtımdan sonra, blok zinciri üyesine gidin.

**Genel bakış**' ı seçtiğinizde, hizmetinize ilişkin temel bilgileri rootcontract adresi ve üye hesabı da dahil olmak üzere görüntüleyebilirsiniz.

![Blok zinciri üyesine genel bakış](./media/create-member/overview.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Sonraki hızlı başlangıç veya öğretici için oluşturduğunuz üyeyi kullanabilirsiniz. Artık gerekli değilse, hızlı başlangıç için oluşturduğunuz `myResourceGroup` kaynak grubunu silerek kaynakları silebilirsiniz.

Kaynak grubunu silmek için:

1. Azure portal sol gezinti bölmesindeki **kaynak grubu** ' na gidin ve silmek istediğiniz kaynak grubunu seçin.
2. **Kaynak grubunu sil**'i seçin. Kaynak grubu adını girip **Sil**' i seçerek silmeyi doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir Azure blok zinciri hizmeti üyesi ve yeni bir konsorsiyu dağıttınız. Azure blok zinciri hizmeti 'nde bir konsorsiyume eklemek için, Ethereum için Azure blok zinciri geliştirme seti 'ni kullanmak üzere bir sonraki hızlı başlangıcı deneyin.

> [!div class="nextstepaction"]
> [Azure blok zinciri hizmetine bağlanmak için Visual Studio Code kullanma](connect-vscode.md)
