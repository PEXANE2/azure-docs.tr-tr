---
title: Azure Resource Manager şablonu kullanarak Azure blok zinciri hizmeti üyesi oluşturma
description: Azure Resource Manager şablonu kullanarak Azure blok zinciri hizmeti üyesini oluşturmayı öğrenin.
services: azure-resource-manager
author: PatAltimore
ms.service: azure-resource-manager
ms.topic: quickstart
ms.custom: subject-armqs
ms.author: patricka
ms.date: 06/04/2020
ms.openlocfilehash: 8eabb6806dee96871648ea419ba36d768d32a2ab
ms.sourcegitcommit: 124f7f699b6a43314e63af0101cd788db995d1cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86078175"
---
# <a name="quickstart-create-an-azure-blockchain-service-member-using-an-arm-template"></a>Hızlı başlangıç: ARM şablonu kullanarak Azure blok zinciri hizmeti üyesi oluşturma

Bu hızlı başlangıçta, yeni bir blok zinciri üyesi ve konsorsiyunuzu bir Azure Resource Manager şablonu (ARM şablonu) kullanarak Azure blok zinciri hizmetine dağıtırsınız. Bir Azure blok zinciri hizmeti üyesi, özel bir konsorsiyum blok zinciri ağında bir blok zinciri düğümüdür. Bir üyeyi sağlarken bir konsorsiyum ağı oluşturabilir veya katabilirsiniz. Bir konsorsiyum ağı için en az bir üyeye ihtiyacınız vardır. Katılımcılar tarafından gereken blok zinciri üyelerinin sayısı senaryonuza bağlıdır. Konsorsiyum katılımcıları bir veya daha fazla blok zinciri üyesine sahip olabilir veya diğer katılımcılarla üyeleri paylaşabilir. Consorda hakkında daha fazla bilgi için bkz. [Azure blok zinciri hizmeti Consortium](consortium.md).

[!INCLUDE [About Azure Resource Manager](../../../includes/resource-manager-quickstart-introduction.md)]

Ortamınız önkoşulları karşılıyorsa ve ARM şablonlarını kullanma hakkında bilginiz varsa, **Azure 'A dağıt** düğmesini seçin. Şablon Azure portal açılır.

[![Azure’a dağıtma](../../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-blockchain-asaservice%2Fazuredeploy.json)

## <a name="prerequisites"></a>Önkoşullar

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="review-the-template"></a>Şablonu gözden geçirme

Bu hızlı başlangıçta kullanılan şablon [Azure hızlı başlangıç şablonlarından](https://azure.microsoft.com/resources/templates/201-blockchain-asaservice/).

:::code language="json" source="~/quickstart-templates/201-blockchain-asaservice/azuredeploy.json" range="1-84" highlight="52-80":::

Şablonda tanımlanan Azure kaynakları:

* [**Microsoft. Blockzincirine/blockchainMembers**](/azure/templates/microsoft.blockchain/blockchainmembers)

## <a name="deploy-the-template"></a>Şablonu dağıtma

1. Azure 'da oturum açmak ve bir şablonu açmak için aşağıdaki bağlantıyı seçin.

    [![Azure’a dağıtma](../../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-blockchain-asaservice%2Fazuredeploy.json)

1. Azure blok zinciri hizmeti üyesine yönelik ayarları belirtin.

    Ayar | Description
    --------|------------
    Abonelik | Hizmetiniz için kullanmak istediğiniz Azure aboneliğini seçin. Birden fazla aboneliğiniz varsa kaynağın faturalandığı aboneliği seçin.
    Kaynak grubu | Yeni bir kaynak grubu adı oluşturun veya aboneliğinizden mevcut bir tane seçin.
    Konum | Üyenin oluşturulacağı bir konum seçin. Konsorsiyumun tüm üyeleri aynı konumda olmalıdır. Dağıtım için kullanılabilir konumlar şunlardır *westeurope, eastus, Güneydoğu, westeurope, northeurope, westus2*ve *japaneast*.
    Üye adı | Azure blok zinciri hizmeti üyesi için benzersiz bir ad seçin. Blok zinciri üye adı yalnızca küçük harf ve sayı içerebilir. İlk karakter bir harf olmalıdır. Değer 2 ila 20 karakter uzunluğunda olmalıdır.
    Konsorsiyum adı | Benzersiz bir ad girin. Consorda hakkında daha fazla bilgi için bkz. [Azure blok zinciri hizmeti Consortium](consortium.md).
    Üye parolası | Üye hesabı parolası, üyelik için oluşturulan Ethereum hesabının özel anahtarını şifrelemek için kullanılır. Konsorsiyum yönetimi için üye hesabı ve üye hesabı parolasını kullanırsınız.
    SKU katmanı | Yeni hizmetinizin fiyatlandırma katmanı. **Standart** ve **temel** katmanlar arasında seçim yapın. Geliştirme, test ve kavram kanıtı için *temel* katmanı kullanın. Üretim sınıfı dağıtımları için *Standart* katmanı kullanın. Ayrıca, blok Veri Yöneticisi Zinciri kullanıyorsanız veya yüksek hacimli özel işlemler gönderiyorsanız *Standart* katmanı da kullanmanız gerekir. Üye oluşturulduktan sonra temel ve standart arasındaki fiyatlandırma katmanını değiştirmek desteklenmez.
    SKU adı | Yeni hizmetinizin düğüm yapılandırması ve maliyeti.

1. Şablonu dağıtmak için **satın al** ' ı seçin.

  Azure portal, şablonu dağıtmak için burada kullanılır. Azure PowerShell, Azure CLı ve REST API de kullanabilirsiniz. Diğer dağıtım yöntemlerini öğrenmek için bkz. [şablonları dağıtma](../../azure-resource-manager/templates/deploy-powershell.md).

## <a name="review-deployed-resources"></a>Dağıtılan kaynakları gözden geçirme

Dağıtılan Azure blok zinciri hizmeti üyesinin ayrıntılarını görüntülemek için Azure portal kullanabilirsiniz. Portalda, Azure blok zinciri hizmeti üyesini içeren kaynak grubuna gidin. Oluşturduğunuz blok zinciri üyesini seçin.

![Azure portal dağıtılan Azure blok zinciri üyesine genel bakış ayrıntıları](./media/create-member-template/deployed-member.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Sonraki hızlı başlangıç veya öğretici için oluşturduğunuz blok zinciri üyesini kullanabilirsiniz. Artık gerekli değilse, hızlı başlangıç için oluşturduğunuz kaynak grubunu silerek kaynakları silebilirsiniz.

Kaynak grubunu silmek için:

1. Azure portal sol gezinti bölmesindeki **kaynak grubu** ' na gidin ve silmek istediğiniz kaynak grubunu seçin.
2. **Kaynak grubunu sil**'i seçin. Kaynak grubu adını girip **Sil**' i seçerek silmeyi doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir Azure blok zinciri hizmeti üyesi ve yeni bir konsorsiyu dağıttınız. Azure blok zinciri hizmeti üyesine eklemek için Ethereum için Azure blok zinciri geliştirme seti 'ni kullanmak üzere bir sonraki hızlı başlangıcı deneyin.

> [!div class="nextstepaction"]
> [Azure blok zinciri hizmetine bağlanmak için Visual Studio Code kullanma](connect-vscode.md)