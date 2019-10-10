---
title: Kaynak Yöneticisi şablonuyla atama oluşturma
description: Bu makale, uyumlu olmayan kaynakları belirlemek üzere bir ilke ataması oluşturmak için Kaynak Yöneticisi şablonu kullanma adımlarında size yol gösterir.
author: DCtheGeek
ms.author: dacoulte
ms.date: 03/13/2019
ms.topic: quickstart
ms.service: azure-policy
ms.openlocfilehash: 842b29160e3abaea5edc4bfb4f2e1c345e3ffc85
ms.sourcegitcommit: 1c2659ab26619658799442a6e7604f3c66307a89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2019
ms.locfileid: "72254807"
---
# <a name="quickstart-create-a-policy-assignment-to-identify-non-compliant-resources-by-using-a-resource-manager-template"></a>Hızlı başlangıç: Kaynak Yöneticisi şablonu kullanarak uyumlu olmayan kaynakları belirlemek için bir ilke ataması oluşturma

Azure 'da uyumluluğu anlamak için ilk adım kaynaklarınızın durumunu belirlemektir.
Bu hızlı başlangıç, yönetilen diskleri kullanmayan sanal makineleri belirlemek için bir ilke ataması oluşturma işleminde size kılavuzluk edecek.

Bu işlemin sonunda, yönetilen disk kullanmayan sanal makineleri başarıyla tanımlayacaksınız. İlke atamasıyla *uyumlu değildir* .

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="create-a-policy-assignment"></a>İlke ataması oluşturma

Bu hızlı başlangıçta, bir ilke ataması oluşturup *yönetilen diskleri kullanmayan VM 'Leri denetle*adlı yerleşik bir ilke tanımı atayabilirsiniz. Kullanılabilir yerleşik ilkelerin kısmi listesi için bkz. [Azure ilke örnekleri](./samples/index.md).

İlke atamaları oluşturmak için çeşitli yöntemler vardır. Bu hızlı başlangıçta bir [hızlı başlangıç şablonu](https://azure.microsoft.com/resources/templates/101-azurepolicy-assign-builtinpolicy-resourcegroup/)kullanırsınız.
Şablonun bir kopyası aşağıda verilmiştir:

[!code-json[policy-assignment](~/quickstart-templates/101-azurepolicy-assign-builtinpolicy-resourcegroup/azuredeploy.json)]

> [!NOTE]
> Azure Ilke hizmeti ücretsizdir.  Daha fazla bilgi için bkz. [Azure Ilkesine genel bakış](./overview.md).

1. Azure portal oturum açmak ve şablonu açmak için aşağıdaki görüntüyü seçin:

   [![Ilke şablonunu Azure 'a dağıtma](./media/assign-policy-template/deploy-to-azure.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-azurepolicy-assign-builtinpolicy-resourcegroup%2Fazuredeploy.json)

1. Aşağıdaki değerleri seçin veya girin:

   | Name | Değer |
   |------|-------|
   | Aboneliğiniz | Azure aboneliğinizi seçin. |
   | Kaynak grubu | **Yeni oluştur**' u seçin, bir ad belirtin ve ardından **Tamam**' ı seçin. Ekran görüntüsünde, kaynak grubu adı *mypolicyquickstart @ no__t-1Date Ile mmdd > RG '* dir. |
   | Konum | Bir bölge seçin. Örneğin, **Orta ABD**. |
   | İlke atama adı | Bir ilke atama adı belirtin. İsterseniz ilke tanımı ekranını kullanabilirsiniz. Örneğin, **yönetilen disk kullanmayan VM 'Leri denetleyin**. |
   | RG adı | İlkeyi atamak istediğiniz kaynak grubu adını belirtin. Bu hızlı başlangıçta **[resourceGroup (). Name]** varsayılan değerini kullanın. **[resourceGroup ()](../../azure-resource-manager/resource-group-template-functions-resource.md#resourcegroup)** , kaynak grubunu alan bir şablon işlevidir. |
   | İlke tanımı KIMLIĞI | **/Providers/Microsoft.Authorization/policyDefinitions/0a914e76-4921-4C19-B460-a2d36003525a**belirtin. |
   | Yukarıda belirtilen hüküm ve koşulları kabul ediyorum | Seçin |

1. **Satın al**' ı seçin.

Bazı ek kaynaklar:

- Daha fazla örnek şablonu bulmak için bkz. [Azure hızlı başlangıç şablonu](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Authorization&pageNumber=1&sort=Popular).
- Şablon başvurusunu görmek için [Azure şablon başvurusu](/azure/templates/microsoft.authorization/allversions)' na gidin.
- Kaynak Yöneticisi şablonlarının nasıl geliştirileceği hakkında bilgi edinmek için bkz. [Azure Resource Manager belgeleri](/azure/azure-resource-manager/).
- Abonelik düzeyinde dağıtım hakkında bilgi edinmek için bkz. [abonelik düzeyinde kaynak grupları ve kaynaklar oluşturma](../../azure-resource-manager/deploy-to-subscription.md).

## <a name="identify-non-compliant-resources"></a>Uyumlu olmayan kaynakları tanımla

Sayfanın sol tarafındaki **Uyumluluk** ' i seçin. Ardından, oluşturduğunuz **yönetilen diskler ilke atamasını kullanmayan VM 'Leri denetle** seçeneğini bulun.

![İlke uyumluluğuna genel bakış sayfası](./media/assign-policy-template/policy-compliance.png)

Bu yeni atamayla uyumlu olmayan mevcut kaynaklar varsa **uyumlu olmayan kaynaklar**altında görünürler.

Daha fazla bilgi için bkz. [Uyumluluk nasıl çalıştığını öğrenin](./how-to/get-compliance-data.md#how-compliance-works).

## <a name="clean-up-resources"></a>Kaynakları Temizleme

Oluşturulan atamayı kaldırmak için şu adımları izleyin:

1. Azure Ilkesi sayfasının sol tarafındaki **Uyumluluk** (veya **atamalar**) ' ı seçin ve oluşturduğunuz **yönetilen diskleri kullanmayan VM 'leri denetle** ilke atamasını bulun.

1. Yönetilen diskler ilkesi atamasını **kullanmayan VM 'Leri denetle** ' ye sağ tıklayın ve **atamayı Sil**' i seçin.

   ![Uyumluluk genel bakış sayfasından bir atamayı silme](./media/assign-policy-template/delete-assignment.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir kapsama yerleşik bir ilke tanımı atamış ve uyumluluk raporunu değerlendirdiniz. İlke tanımı, kapsamdaki tüm kaynakların uyumlu olduğunu doğrular ve hangilerinin olduğunu tanımlar.

Yeni kaynakların uyumlu olduğunu doğrulamak üzere ilke atama hakkında daha fazla bilgi edinmek için öğreticiye geçin:

> [!div class="nextstepaction"]
> [İlke oluşturma ve yönetme](./tutorials/create-and-manage.md)