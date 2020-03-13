---
title: Azure blok zinciri hizmeti üyesi oluşturma-Azure portal
description: Azure portal kullanarak bir blok zinciri Konsorsiyumu için Azure blok zinciri hizmeti üyesi oluşturun.
ms.date: 03/12/2020
ms.topic: quickstart
ms.reviewer: ravastra
ms.openlocfilehash: 3c468633a193d78fb1c017a756ee372c6feefb12
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79203670"
---
# <a name="quickstart-create-an-azure-blockchain-service-blockchain-member-using-the-azure-portal"></a>Hızlı başlangıç: Azure portal kullanarak bir Azure blok zinciri hizmeti blok zinciri üyesi oluşturma

Bu hızlı başlangıçta, Azure blok zinciri hizmeti 'nde Azure portal kullanarak yeni bir blok zinciri üyesi ve konsorsiyu dağıtırsınız.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-blockchain-member"></a>Blok zinciri üyesi oluşturma

Bir Azure blok zinciri hizmeti üyesi, özel bir konsorsiyum blok zinciri ağında bir blok zinciri düğümüdür. Bir üyeyi sağlarken bir konsorsiyum ağı oluşturabilir veya katabilirsiniz. Bir konsorsiyum ağı için en az bir üyeye ihtiyacınız vardır. Katılımcılar tarafından gereken blok zinciri üyelerinin sayısı senaryonuza bağlıdır. Konsorsiyum katılımcıları bir veya daha fazla blok zinciri üyesine sahip olabilir veya diğer katılımcılarla üyeleri paylaşabilir. Consorda hakkında daha fazla bilgi için bkz. [Azure blok zinciri hizmeti Consortium](consortium.md).

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Azure portalının sol üst köşesinde bulunan **Kaynak oluştur** öğesini seçin.
1. **Azure blok zinciri hizmeti (Önizleme)**  > **blok zinciri** seçin.

    ![Hizmet Oluştur](./media/create-member/create-member.png)

    Ayar | Açıklama
    --------|------------
    Abonelik | Hizmetiniz için kullanmak istediğiniz Azure aboneliğini seçin. Birden fazla aboneliğiniz varsa kaynağın faturalandığı aboneliği seçin.
    Kaynak grubu | Yeni bir kaynak grubu adı oluşturun veya aboneliğinizden mevcut bir tane seçin.
    Bölge | Üyeyi oluşturmak için bir bölge seçin. Konsorsiyumun tüm üyeleri aynı konumda olmalıdır.
    Protokol | Şu anda Azure blok zinciri hizmeti önizlemesi çekirdek protokolünü destekliyor.
    Konsorsiyum | Yeni bir konsorsiyum için benzersiz bir ad girin. Bir toplantıya bir davet aracılığıyla katılırken katıldığınız Konsorsiyumu seçin. Consorda hakkında daha fazla bilgi için bkz. [Azure blok zinciri hizmeti Consortium](consortium.md).
    Adı | Azure blok zinciri hizmeti üyesi için benzersiz bir ad seçin. Blok zinciri üye adı yalnızca küçük harf ve sayı içerebilir. İlk karakter bir harf olmalıdır. Değer 2 ila 20 karakter uzunluğunda olmalıdır.
    Üye hesabı parolası | Üye hesabı parolası, üyelik için oluşturulan Ethereum hesabının özel anahtarını şifrelemek için kullanılır. Konsorsiyum yönetimi için üye hesabı ve üye hesabı parolasını kullanırsınız.
    Fiyatlandırma | Yeni hizmetinizin düğüm yapılandırması ve maliyeti. **Standart** ve **temel** katmanlar arasında seçim yapmak için **değişiklik** bağlantısını seçin. *Temel* katmanı kullanarak geliştirme, test etme ve kavram kanıtı vardır. Üretim sınıfı dağıtımları için *Standart* katmanı kullanın.
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

Bu hızlı başlangıçta, bir Azure blok zinciri hizmeti üyesi ve yeni bir konsorsiyu dağıttınız. Azure blok zinciri hizmeti üyesine eklemek için Ethereum için Azure blok zinciri geliştirme seti 'ni kullanmak üzere bir sonraki hızlı başlangıcı deneyin.

> [!div class="nextstepaction"]
> [Azure blok zinciri hizmetine bağlanmak için Visual Studio Code kullanma](connect-vscode.md)
