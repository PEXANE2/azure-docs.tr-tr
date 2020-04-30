---
title: 'Hızlı başlangıç: şablonlarla paylaşılan bir sorgu oluşturma'
description: Bu hızlı başlangıçta, sanal makineleri işletim sistemine göre sayan kaynak grafik paylaşılan sorgusu oluşturmak için bir Kaynak Yöneticisi şablonu kullanırsınız.
ms.date: 04/28/2020
ms.topic: quickstart
ms.custom: subject-armqs
ms.openlocfilehash: 050cf26da2054883fceaa08b11f94c6af4c85a16
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82234335"
---
# <a name="quickstart-create-a-shared-query-by-using-a-resource-manager-template"></a>Hızlı başlangıç: Kaynak Yöneticisi şablonu kullanarak paylaşılan sorgu oluşturma

Kaynak Grafiği sorguları, _özel bir sorgu_ veya _paylaşılan sorgu_olarak kaydedilebilir. Özel bir sorgu bireyler portalı profiline kaydedilir ve başkaları tarafından görülemez. Paylaşılan sorgu, izinler ve rol tabanlı erişim aracılığıyla başkalarıyla paylaşılabilecek Kaynak Yöneticisi nesnesidir. Paylaşılan sorgu, kaynak bulmanın ortak ve tutarlı bir şekilde yürütülmesini sağlar. Bu hızlı başlangıç, paylaşılan bir sorgu oluşturmak için bir Kaynak Yöneticisi şablonu kullanır.

[!INCLUDE [About Azure Resource Manager](../../../includes/resource-manager-quickstart-introduction.md)]

## <a name="prerequisites"></a>Ön koşullar

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="create-a-shared-query"></a>Paylaşılan sorgu oluşturma

Bu hızlı başlangıçta, _işletim sistemi tarafından Count VM 'ler_adlı bir paylaşılan sorgu oluşturacaksınız. Bu sorguyu SDK 'da veya kaynak Graph Explorer ile portalda denemek için bkz. [OS türlerine göre örnek sayısı sanal makineleri](./samples/starter.md#count-os).

### <a name="review-the-template"></a>Şablonu gözden geçirin

Bu hızlı başlangıçta kullanılan şablon [Azure hızlı başlangıç şablonlarından](https://azure.microsoft.com/resources/templates/resourcegraph-sharedquery-countos/).

:::code language="json" source="~/quickstart-templates/resourcegraph-sharedquery-countos/azuredeploy.json" highlight="28-37":::

Şablonda tanımlanan kaynak:

- [Microsoft. ResourceGraph/sorgular](/azure/templates/microsoft.resourcegraph/queries)

### <a name="deploy-the-template"></a>Şablonu dağıtma

> [!NOTE]
> Azure Kaynak Grafiği hizmeti ücretsizdir. Daha fazla bilgi için bkz. [Azure Kaynak grafiğine genel bakış](./overview.md).

1. Azure portalında oturum açıp şablonu açmak için aşağıdaki görüntüyü seçin:

   [![Ilke şablonunu Azure 'a dağıtma](../../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fresourcegraph-sharedquery-countos%2Fazuredeploy.json)

1. Aşağıdaki değerleri seçin veya girin:

   | Adı | Değer |
   |------|-------|
   | Abonelik | Azure aboneliğinizi seçin. |
   | Kaynak grubu | **Yeni oluştur**' u seçin, bir ad belirtin ve ardından **Tamam**' ı seçin. |
   | Konum | Bir bölge seçin. Örneğin, **Orta ABD**. |
   | Sorgu adı | Varsayılan değer **sayısı VM 'lerini işletim sistemi olarak**bırakın. |
   | Sorgu kodu | Varsayılan değeri bırakın`Resources | where type =~ 'Microsoft.Compute/virtualMachines' | summarize count() by tostring(properties.storageProfile.osDisk.osType)` |
   | Sorgu açıklaması | **Bu paylaşılan sorgunun tüm sanal makine kaynaklarını saydığı ve işletim sistemi türüne göre özetlediği** varsayılan değeri bırakın. |
   | Yukarıda belirtilen hüküm ve koşulları kabul ediyorum | Seçin |

1. **Satın al**'ı seçin.

Bazı ek kaynaklar:

- Daha fazla örnek şablonu bulmak için bkz. [Azure hızlı başlangıç şablonu](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Authorization&pageNumber=1&sort=Popular).
- Şablon başvurusunu görmek için [Azure şablon başvurusu](/azure/templates/microsoft.resourcegraph/allversions)' na gidin.
- Kaynak Yöneticisi şablonlarının nasıl geliştirileceği hakkında bilgi edinmek için bkz. [Azure Resource Manager belgeleri](../../azure-resource-manager/management/overview.md).
- Abonelik düzeyinde dağıtım hakkında bilgi edinmek için bkz. [abonelik düzeyinde kaynak grupları ve kaynaklar oluşturma](../../azure-resource-manager/templates/deploy-to-subscription.md).

## <a name="validate-the-deployment"></a>Dağıtımı doğrulama

Yeni paylaşılan sorguyu çalıştırmak için aşağıdaki adımları izleyin:

1. Portal arama çubuğundan **Kaynak Grafiği sorgularını** arayın ve seçin.

1. **İşletim sistemine göre sayı VM 'leri**adlı paylaşılan sorguyu seçin, sonra **genel bakış** sayfasında **sonuçlar** sekmesini seçin.

Alternatif olarak, paylaşılan sorgu kaynak grafik Gezgini ' nden açılabilir:

1. Portal arama çubuğundan **Kaynak Grafiği Gezgini** ' ni arayın ve seçin.

1. **Sorgu aç** düğmesini seçin.

1. **Türü** _paylaşılan sorgular_olarak değiştirin. Listedeki **VM 'Leri bayt sayısı** ' nı görmüyorsanız, sonuçları sınırlandırmak için filtre kutusunu kullanın. **İşletim sistemi paylaşılan sorgusuna göre VM sayısı** görünür olduğunda, adını seçin.

1. Sorgu yüklendikten sonra **Sorguyu Çalıştır** düğmesini seçin. Sonuçlar aşağıdaki **sonuçlar** sekmesinde görüntülenir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Oluşturulan paylaşılan sorguyu kaldırmak için şu adımları izleyin:

1. Portal arama çubuğundan **Kaynak Grafiği sorgularını** arayın ve seçin.

1. **İşletim sisteminin say**adlı paylaşılan sorgunun yanındaki onay kutusunu belirleyin.

1. Sayfanın üst kısmında **Sil** düğmesini seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, kaynak grafiği paylaşılan sorgusu oluşturdunuz.

Paylaşılan sorgular hakkında daha fazla bilgi edinmek için öğreticiye geçin:

> [!div class="nextstepaction"]
> [Azure portal sorguları yönetme](./tutorials/create-share-query.md)