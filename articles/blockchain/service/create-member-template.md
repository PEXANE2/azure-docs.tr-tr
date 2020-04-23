---
title: Azure Kaynak Yöneticisi şablonu kullanarak bir Azure Blockchain Hizmeti üyesi oluşturun
description: Azure Kaynak Yöneticisi şablonu kullanarak bir Azure Blockchain Hizmeti üyesini nasıl oluşturabilirsiniz öğrenin.
services: azure-resource-manager
author: PatAltimore
ms.service: azure-resource-manager
ms.topic: quickstart
ms.custom: subject-armqs
ms.author: patricka
ms.date: 04/22/2020
ms.openlocfilehash: 8fd5a1775a6a217ce9dd8c80439dd2c54f9cd727
ms.sourcegitcommit: 09a124d851fbbab7bc0b14efd6ef4e0275c7ee88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2020
ms.locfileid: "82087600"
---
# <a name="create-an-azure-blockchain-service-member-using-an-azure-resource-manager-template"></a>Azure Kaynak Yöneticisi şablonu kullanarak Azure Blockchain Hizmeti üyesi oluşturma

Bu hızlı başlangıçta, Azure Kaynak Yöneticisi şablonu kullanarak Azure Blockchain Hizmeti'nde yeni bir blockchain üyesi ve konsorsiyum uyguluyorsunuz.

Azure Blockchain Service üyesi, özel bir konsorsiyum blockchain ağında ki blockchain düğümüdür. Bir üye sağlarken, bir konsorsiyum ağı oluşturabilir veya katılabilirsiniz. Bir konsorsiyum ağı için en az bir üyeye ihtiyacınız var. Katılımcıların ihtiyaç duyduğu blockchain üye sayısı senaryonuza bağlıdır. Konsorsiyum katılımcılarının bir veya daha fazla blockchain üyesi olabilir veya üyeleri diğer katılımcılarla paylaşabilirler. Konsorsiyumlar hakkında daha fazla bilgi için [Azure Blockchain Service konsorsiyumuna](consortium.md)bakın.

[!INCLUDE [About Azure Resource Manager](../../../includes/resource-manager-quickstart-introduction.md)]

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Yok.

## <a name="review-the-template"></a>Şablonu gözden geçirme

Bu hızlı başlatmada kullanılan şablon [Azure Quickstart şablonlarındandır.](https://github.com/Azure/azure-quickstart-templates/blob/master/201-blockchain-asaservice/)

[!code-json[<Azure Resource Manager template create blockchain member>](~/quickstart-templates/201-blockchain-asaservice/azuredeploy.json)]

Şablonda tanımlanan azure kaynakları:

* [**Microsoft.Blockchain/blockchainÜyeler**](https://docs.microsoft.com/azure/templates/microsoft.blockchain/blockchainmembers)

## <a name="deploy-the-template"></a>Şablonu dağıtma

1. Azure'da oturum açmak ve şablon açmak için aşağıdaki bağlantıyı seçin.

    [![Azure’a dağıtma](./media/create-member-template/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-blockchain-asaservice%2Fazuredeploy.json)

1. Azure Blockchain Hizmeti üyesinin ayarlarını belirtin.

    Ayar | Açıklama
    --------|------------
    Abonelik | Hizmetiniz için kullanmak istediğiniz Azure aboneliğini seçin. Birden fazla aboneliğiniz varsa kaynağın faturalandığı aboneliği seçin.
    Kaynak grubu | Yeni bir kaynak grubu adı oluşturun veya aboneliğinizden varolan bir tane seçin.
    Konum | Üyeoluşturmak için bir konum seçin. Konsorsiyumun tüm üyeleri aynı yerde olmalıdır. Konuşlandırma için mevcut yerler *westeurope, eastus, southeastasia, westeurope, northeurope, westus2*, ve *Japaneast*vardır.
    Üye adı | Azure Blockchain Hizmeti üyesi için benzersiz bir ad seçin. Blockchain üye adı yalnızca küçük harfler ivedi lik ve sayılar içerebilir. İlk karakter bir harf olmalıdır. Değer 2 ile 20 karakter uzunluğunda olmalıdır.
    Konsorsiyum adı | Benzersiz bir ad girin. Konsorsiyumlar hakkında daha fazla bilgi için [Azure Blockchain Service konsorsiyumuna](consortium.md)bakın.
    Üye şifresi | Üye hesabı parolası, üyeniz için oluşturulan Ethereum hesabının özel anahtarını şifrelemek için kullanılır. Konsorsiyum yönetimi için üye hesabı ve üye hesabı parolasını kullanırsınız.
    Sku kademe | Yeni hizmetinizin fiyatlandırma katmanı. **Standart** ve **Temel** katmanlar arasında seçim yapın. Kavramların geliştirilmesi, test edilmesi ve kanıtedilmesi için *Temel* katmanı kullanın. Üretim sınıfı dağıtımları için *Standart* katmanı kullanın. Blockchain Data Manager kullanıyorsanız veya yüksek hacimli özel işlemler *gönderiyorsanız, Standart* katmanı da kullanmalısınız. Üye oluşturulduktan sonra temel ve standart arasındaki fiyatlandırma katmanının değiştirilmesi desteklenmez.
    Sku adı | Düğüm yapılandırması ve yeni hizmetinizin maliyeti.

1. Şablonu dağıtmak için **Satın Alma'yı** seçin.

  Azure portalı burada şablonu dağıtmak için kullanılır. Azure PowerShell, Azure CLI ve REST API'yi de kullanabilirsiniz. Diğer dağıtım yöntemlerini öğrenmek için [şablonları dağıt'a](../../azure-resource-manager/templates/deploy-powershell.md)bakın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Oluşturduğunuz blockchain üyesini bir sonraki hızlı başlangıç veya öğretici için kullanabilirsiniz. Artık gerekmediğinde, hızlı başlangıç için oluşturduğunuz kaynak grubunu silerek kaynakları silebilirsiniz.

Kaynak grubunu silmek için:

1. Azure portalında, sol gezinti bölmesindeki **Kaynak grubuna** gidin ve silmek istediğiniz kaynak grubunu seçin.
2. **Kaynak grubunu sil**'i seçin. Kaynak grup adını girerek silme işlemini doğrulayın ve **Sil'i**seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir Azure Blockchain Service üyesi ve yeni bir konsorsiyum görevlendirdiniz. Bir Azure Blockchain Hizmeti üyesine takmak için Ethereum için Azure Blockchain Geliştirme Kiti'ni kullanmak için bir sonraki hızlı başlangıcı deneyin.

> [!div class="nextstepaction"]
> [Azure Blockchain Hizmetine bağlanmak için Visual Studio Kodunu kullanma](connect-vscode.md)