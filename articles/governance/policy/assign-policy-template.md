---
title: 'Hızlı başlangıç: şablonlarla yeni ilke ataması'
description: Bu hızlı başlangıçta, uyumlu olmayan kaynakları belirlemek üzere bir ilke ataması oluşturmak için bir Azure Resource Manager şablonu (ARM şablonu) kullanırsınız.
ms.date: 05/21/2020
ms.topic: quickstart
ms.custom: subject-armqs
ms.openlocfilehash: c3a37e9086744fb45c8d1e4ebe3bd16da7a83770
ms.sourcegitcommit: f684589322633f1a0fafb627a03498b148b0d521
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/06/2020
ms.locfileid: "85971156"
---
# <a name="quickstart-create-a-policy-assignment-to-identify-non-compliant-resources-by-using-an-azure-resource-manager-template"></a>Hızlı başlangıç: Azure Resource Manager şablonu kullanarak uyumlu olmayan kaynakları belirlemek için bir ilke ataması oluşturma

Azure’da uyumluluğu anlamanın ilk adımı, kaynaklarınızın durumunu belirlemektir.
Bu hızlı başlangıç, yönetilen disk kullanmayan sanal makineleri belirlemek üzere ilke ataması oluşturma işleminde size yol gösterir. Bu işlemin sonunda, yönetilen disk kullanmayan sanal makineleri başarılı bir şekilde belirlemiş olacaksınız. Bu sanal makineler, ilke ataması ile _uyumsuzdur_.

[!INCLUDE [About Azure Resource Manager](../../../includes/resource-manager-quickstart-introduction.md)]

## <a name="prerequisites"></a>Önkoşullar

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="create-a-policy-assignment"></a>İlke ataması oluşturma

Bu hızlı başlangıçta, bir ilke ataması oluşturup _yönetilen diskleri kullanmayan VM 'Leri denetle_adlı yerleşik bir ilke tanımı atayabilirsiniz. Kullanılabilir yerleşik ilkelerin kısmi listesi için bkz. [Azure ilke örnekleri](./samples/index.md).

### <a name="review-the-template"></a>Şablonu gözden geçirme

Bu hızlı başlangıçta kullanılan şablon [Azure Hızlı Başlangıç şablonlarından](https://azure.microsoft.com/resources/templates/101-azurepolicy-assign-builtinpolicy-resourcegroup/) alınmıştır.

:::code language="json" source="~/quickstart-templates/101-azurepolicy-assign-builtinpolicy-resourcegroup/azuredeploy.json" range="1-30" highlight="20-28":::

Şablonda tanımlanan kaynak:

- [Microsoft. Authorization/Poliyasatamaları](/azure/templates/microsoft.authorization/policyassignments)

### <a name="deploy-the-template"></a>Şablonu dağıtma

> [!NOTE]
> Azure Ilke hizmeti ücretsizdir. Daha fazla bilgi için bkz. [Azure Ilkesine genel bakış](./overview.md).

1. Azure portalında oturum açıp şablonu açmak için aşağıdaki görüntüyü seçin:

   :::image type="content" source="../../media/template-deployments/deploy-to-azure.svg" alt-text="Azure 'a Azure Ilkesi atamak için ARM şablonunu dağıtma" border="false" link="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-azurepolicy-assign-builtinpolicy-resourcegroup%2Fazuredeploy.json":::

1. Aşağıdaki değerleri seçin veya girin:

   | Name | Değer |
   |------|-------|
   | Abonelik | Azure aboneliğinizi seçin. |
   | Kaynak grubu | **Yeni oluştur**' u seçin, bir ad belirtin ve ardından **Tamam**' ı seçin. Ekran görüntüsünde, kaynak grubu adı _mypolicyquickstart \<Date in MMDD\> RG_' dir. |
   | Konum | Bir bölge seçin. Örneğin **Orta ABD**. |
   | İlke atama adı | Bir ilke atama adı belirtin. İsterseniz ilke tanımı ekranını kullanabilirsiniz. Örneğin, **yönetilen disk kullanmayan VM 'Leri denetleyin**. |
   | RG adı | İlkeyi atamak istediğiniz kaynak grubu adını belirtin. Bu hızlı başlangıçta **[resourceGroup (). Name]** varsayılan değerini kullanın. **[resourceGroup ()](../../azure-resource-manager/templates/template-functions-resource.md#resourcegroup)** , kaynak grubunu alan bir şablon işlevidir. |
   | İlke tanımı KIMLIĞI | **/Providers/Microsoft.Authorization/policyDefinitions/0a914e76-4921-4C19-B460-a2d36003525a**belirtin. |
   | Yukarıda belirtilen hüküm ve koşulları kabul ediyorum | Seçin |

1. **Satın al**'ı seçin.

Bazı ek kaynaklar:

- Daha fazla örnek şablonu bulmak için bkz. [Azure hızlı başlangıç şablonu](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Authorization&pageNumber=1&sort=Popular).
- Şablon başvurusunu görmek için [Azure şablon başvurusu](/azure/templates/microsoft.authorization/allversions)' na gidin.
- Kaynak Yöneticisi şablonlarının nasıl geliştirileceği hakkında bilgi edinmek için, bkz. [Kaynak Yöneticisi belgeleri](../../azure-resource-manager/management/overview.md).
- Abonelik düzeyinde dağıtım hakkında bilgi edinmek için bkz. [abonelik düzeyinde kaynak grupları ve kaynaklar oluşturma](../../azure-resource-manager/templates/deploy-to-subscription.md).

## <a name="validate-the-deployment"></a>Dağıtımı doğrulama

Sayfanın sol tarafındaki **Uyumluluk** ' i seçin. Ardından, oluşturduğunuz **yönetilen diskler ilke atamasını kullanmayan VM 'Leri denetle** seçeneğini bulun.

:::image type="content" source="./media/assign-policy-template/policy-compliance.png" alt-text="İlke uyumluluğuna genel bakış sayfası" border="false":::

Bu yeni atamayla uyumlu olmayan mevcut kaynaklar varsa **uyumlu olmayan kaynaklar**altında görünürler.

Daha fazla bilgi için bkz. [Uyumluluk nasıl çalıştığını öğrenin](./how-to/get-compliance-data.md#how-compliance-works).

## <a name="clean-up-resources"></a>Kaynakları temizleme

Oluşturulan atamayı kaldırmak için şu adımları izleyin:

1. Azure İlkesi sayfasının sol tarafından **Uyumluluk**’u (veya **Atamalar**’ı) seçin ve oluşturduğunuz **Yönetilen disk kullanmayan VM'leri denetle** ilke atamasını bulun.

1. Yönetilen diskler ilkesi atamasını **kullanmayan VM 'Leri denetle** ' ye sağ tıklayın ve **atamayı Sil**' i seçin.

   :::image type="content" source="./media/assign-policy-template/delete-assignment.png" alt-text="Uyumluluk genel bakış sayfasından bir atamayı silme" border="false":::

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir kapsama yerleşik bir ilke tanımı atamış ve uyumluluk raporunu değerlendirdiniz. İlke tanımı, kapsamdaki tüm kaynakların uyumlu olduğunu doğrular ve hangilerinin olduğunu tanımlar.

Yeni kaynakların uyumlu olduğunu doğrulamak üzere ilke atama hakkında daha fazla bilgi edinmek için öğreticiye geçin:

> [!div class="nextstepaction"]
> [İlke oluşturma ve yönetme](./tutorials/create-and-manage.md)