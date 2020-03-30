---
title: Azure Blockchain Hizmeti üyesi oluşturma - Azure portalı
description: Azure portalını kullanan blockchain konsorsiyumu için bir Azure Blockchain Hizmeti üyesi oluşturun.
ms.date: 03/12/2020
ms.topic: quickstart
ms.reviewer: ravastra
ms.openlocfilehash: 3c468633a193d78fb1c017a756ee372c6feefb12
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "79203670"
---
# <a name="quickstart-create-an-azure-blockchain-service-blockchain-member-using-the-azure-portal"></a>Hızlı başlatma: Azure portalını kullanarak bir Azure Blockchain Hizmeti blockchain üyesi oluşturun

Bu hızlı başlangıçta, Azure portalını kullanarak Azure Blockchain Hizmeti'nde yeni bir blockchain üyesi ve konsorsiyum uyguluyorsunuz.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-blockchain-member"></a>Blockchain üyesi oluşturma

Azure Blockchain Service üyesi, özel bir konsorsiyum blockchain ağında ki blockchain düğümüdür. Bir üye sağlarken, bir konsorsiyum ağı oluşturabilir veya katılabilirsiniz. Bir konsorsiyum ağı için en az bir üyeye ihtiyacınız var. Katılımcıların ihtiyaç duyduğu blockchain üye sayısı senaryonuza bağlıdır. Konsorsiyum katılımcılarının bir veya daha fazla blockchain üyesi olabilir veya üyeleri diğer katılımcılarla paylaşabilirler. Konsorsiyumlar hakkında daha fazla bilgi için [Azure Blockchain Service konsorsiyumuna](consortium.md)bakın.

1. [Azure portalında](https://portal.azure.com)oturum açın.
1. Azure portalının sol üst köşesinde bulunan **Kaynak oluştur** öğesini seçin.
1. **Blockchain** > **Azure Blockchain Hizmeti 'ni seçin (önizleme)**.

    ![Hizmet Oluştur](./media/create-member/create-member.png)

    Ayar | Açıklama
    --------|------------
    Abonelik | Hizmetiniz için kullanmak istediğiniz Azure aboneliğini seçin. Birden fazla aboneliğiniz varsa kaynağın faturalandığı aboneliği seçin.
    Kaynak grubu | Yeni bir kaynak grubu adı oluşturun veya aboneliğinizden varolan bir tane seçin.
    Bölge | Üye oluşturmak için bir bölge seçin. Konsorsiyumun tüm üyeleri aynı yerde olmalıdır.
    Protokol | Şu anda Azure Blockchain Hizmet Önizlemesi, Quorum protokolünü destekler.
    Konsorsiyum | Yeni bir konsorsiyum için benzersiz bir ad girin. Bir davet yoluyla bir konsorsiyuma katılıyorsanız, katıldığınız konsorsiyumu seçin. Konsorsiyumlar hakkında daha fazla bilgi için [Azure Blockchain Service konsorsiyumuna](consortium.md)bakın.
    Adı | Azure Blockchain Hizmeti üyesi için benzersiz bir ad seçin. Blockchain üye adı yalnızca küçük harfler ivedi lik ve sayılar içerebilir. İlk karakter bir harf olmalıdır. Değer 2 ile 20 karakter uzunluğunda olmalıdır.
    Üye hesabı şifresi | Üye hesabı parolası, üyeniz için oluşturulan Ethereum hesabının özel anahtarını şifrelemek için kullanılır. Konsorsiyum yönetimi için üye hesabı ve üye hesabı parolasını kullanırsınız.
    Fiyatlandırma | Düğüm yapılandırması ve yeni hizmetinizin maliyeti. **Standart** ve **Temel** katmanlar arasında seçim yapmak için **Değiştir** bağlantısını seçin. *Temel* katmanı kullanmak, kavramların geliştirilmesi, test edilmesi ve kanıtedilmesi içindir. Üretim sınıfı dağıtımları için *Standart* katmanı kullanın.
    Düğüm parolası | Üyenin varsayılan hareket düğümünün parolası. Blockchain üyesinin varsayılan işlem düğümü ortak bitiş noktasına bağlanırken temel kimlik doğrulaması için parolayı kullanın.

1. Ayarlarınızı doğrulamak için **Gözden Geçir + oluştur'u** seçin. Hizmeti sağlamak için **Oluştur'u** seçin. Sağlama yaklaşık 10 dakika sürer.
1. Dağıtım işlemini izlemek için araç çubuğundaki **Bildirimler'i** seçin.
1. Dağıtımdan sonra blockchain üyenize gidin.

**Genel Bakış'ı**seçin, RootContract adresi ve üye hesabı da dahil olmak üzere hizmetinizle ilgili temel bilgileri görüntüleyebilirsiniz.

![Blockchain üyesine genel bakış](./media/create-member/overview.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Oluşturduğunuz üyeyi bir sonraki hızlı başlangıç veya öğretici için kullanabilirsiniz. Artık gerekmediğinde, hızlı başlangıç için oluşturduğunuz `myResourceGroup` kaynak grubunu silerek kaynakları silebilirsiniz.

Kaynak grubunu silmek için:

1. Azure portalında, sol gezinti bölmesindeki **Kaynak grubuna** gidin ve silmek istediğiniz kaynak grubunu seçin.
2. **Kaynak grubunu sil**'i seçin. Kaynak grup adını girerek silme işlemini doğrulayın ve **Sil'i**seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir Azure Blockchain Service üyesi ve yeni bir konsorsiyum görevlendirdiniz. Bir Azure Blockchain Hizmeti üyesine takmak için Ethereum için Azure Blockchain Geliştirme Kiti'ni kullanmak için bir sonraki hızlı başlangıcı deneyin.

> [!div class="nextstepaction"]
> [Azure Blockchain Hizmetine bağlanmak için Visual Studio Kodunu kullanma](connect-vscode.md)
