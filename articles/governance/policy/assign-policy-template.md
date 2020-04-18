---
title: 'Quickstart: Şablonlarla yeni ilke ataması'
description: Bu hızlı başlatmada, uyumlu olmayan kaynakları tanımlamak için bir ilke ataması oluşturmak için bir Kaynak Yöneticisi şablonu kullanırsınız.
ms.date: 03/16/2020
ms.topic: quickstart
ms.custom: subject-armqs
ms.openlocfilehash: ae6b2d151baaa904215639276c7fb74766810c6a
ms.sourcegitcommit: 5e49f45571aeb1232a3e0bd44725cc17c06d1452
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81606596"
---
# <a name="quickstart-create-a-policy-assignment-to-identify-non-compliant-resources-by-using-a-resource-manager-template"></a>Hızlı başlangıç: Kaynak Yöneticisi şablonu kullanarak uyumlu olmayan kaynakları tanımlamak için bir ilke ataması oluşturma

Azure’da uyumluluğu anlamanın ilk adımı, kaynaklarınızın durumunu belirlemektir.
Bu hızlı başlangıç, yönetilen disk kullanmayan sanal makineleri belirlemek üzere ilke ataması oluşturma işleminde size yol gösterir. Bu işlemin sonunda, yönetilen disk kullanmayan sanal makineleri başarılı bir şekilde belirlemiş olacaksınız. Bu sanal makineler, ilke ataması ile _uyumsuzdur_.

[!INCLUDE [About Azure Resource Manager](../../../includes/resource-manager-quickstart-introduction.md)]

## <a name="prerequisites"></a>Ön koşullar

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="create-a-policy-assignment"></a>İlke ataması oluşturma

Bu hızlı başlangıçta, bir ilke ataması oluşturur sunuz ve _yönetilen diskleri kullanmayan Denetim VM'leri_adlı yerleşik bir ilke tanımı atarsınız. Kullanılabilir yerleşik ilkelerin kısmi bir listesi için [Azure İlkesi örneklerine](./samples/index.md)bakın.

### <a name="review-the-template"></a>Şablonu gözden geçirme

Bu hızlı başlatmada kullanılan şablon [Azure Quickstart şablonlarındandır.](https://azure.microsoft.com/resources/templates/101-azurepolicy-assign-builtinpolicy-resourcegroup/)

:::code language="json" source="~/quickstart-templates/101-azurepolicy-assign-builtinpolicy-resourcegroup/azuredeploy.json" range="1-36" highlight="26-34":::

Şablonda tanımlanan kaynak:

- [Microsoft.Authorization/policyAtamalar](/azure/templates/microsoft.authorization/policyassignments)

### <a name="deploy-the-template"></a>Şablonu dağıtma

> [!NOTE]
> Azure İlkesi hizmeti ücretsizdir. Daha fazla bilgi için Azure [İlkesi'ne Genel Bakış](./overview.md)bölümüne bakın.

1. Azure portalında oturum açıp şablonu açmak için aşağıdaki görüntüyü seçin:

   [![İlke şablonu Azure'a dağıtma](../../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-azurepolicy-assign-builtinpolicy-resourcegroup%2Fazuredeploy.json)

1. Aşağıdaki değerleri seçin veya girin:

   | Adı | Değer |
   |------|-------|
   | Abonelik | Azure aboneliğinizi seçin. |
   | Kaynak grubu | **Yeni Oluştur'u**seçin, bir ad belirtin ve ardından **Tamam'ı**seçin. Ekran görüntüsünde, kaynak grubu adı _MMDD\<\>rg mypolicyquickstart Date_olduğunu. |
   | Konum | Bir bölge seçin. Örneğin, **Orta ABD**. |
   | İlke Atama Adı | İlke atama adı belirtin. İsterseniz ilke tanımı ekranını kullanabilirsiniz. Örneğin, **yönetilen diskleri kullanmayan Denetim VM'leri.** |
   | Rg Adı | İlkeyi atamak istediğiniz kaynak grubu adı belirtin. Bu hızlı başlatmada varsayılan değeri **[resourceGroup().name]** kullanın. **[resourceGroup()](../../azure-resource-manager/templates/template-functions-resource.md#resourcegroup)** kaynak grubunu alan bir şablon işlevidir. |
   | İlke Tanımı Kimliği | Belirtin **/providers/Microsoft.Authorization/policyDefinitions/0a914e76-4921-4c19-b460-a2d36003525a**. |
   | Yukarıda belirtilen hüküm ve koşulları kabul ediyorum | (Seçiniz) |

1. **Satın al**'ı seçin.

Bazı ek kaynaklar:

- Daha fazla örnek bulmak için [Azure Quickstart şablonuna](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Authorization&pageNumber=1&sort=Popular)bakın.
- Şablon başvuruyu görmek için [Azure şablonu başvurusuna](/azure/templates/microsoft.authorization/allversions)gidin.
- Kaynak Yöneticisi şablonlarını nasıl geliştireceklerini öğrenmek için [Azure Kaynak Yöneticisi belgelerine](../../azure-resource-manager/management/overview.md)bakın.
- Abonelik düzeyinde dağıtım öğrenmek için [bkz.](../../azure-resource-manager/templates/deploy-to-subscription.md)

## <a name="validate-the-deployment"></a>Dağıtımı doğrulama

Sayfanın sol tarafında **Uyumluluk'u** seçin. Ardından, oluşturduğunuz yönetilen diskler ilkesi atamasını **kullanmayan Denetim VM'lerini** bulun.

![İlke uyumluluğuna genel bakış sayfası](./media/assign-policy-template/policy-compliance.png)

Bu yeni atamayla uyumlu olmayan varolan kaynaklar varsa, bunlar **uyumlu olmayan kaynaklar**altında görünür.

Daha fazla bilgi için [uyumluluk nasıl çalışır.](./how-to/get-compliance-data.md#how-compliance-works)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Oluşturulan atamayı kaldırmak için aşağıdaki adımları izleyin:

1. Azure İlkesi sayfasının sol tarafından **Uyumluluk**’u (veya **Atamalar**’ı) seçin ve oluşturduğunuz **Yönetilen disk kullanmayan VM'leri denetle** ilke atamasını bulun.

1. Yönetilen diskler ilkesi atamasını **kullanmayan Denetim VM'lerini** sağ tıklatın ve **atamayı sil'i**seçin.

   ![Bir atamayı uyumluluk genel bakış sayfasından silme](./media/assign-policy-template/delete-assignment.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir kapsama yerleşik bir ilke tanımı atadınız ve uyumluluk raporunu değerlendirdiniz. İlke tanımı, kapsamdaki tüm kaynakların uyumlu olduğunu doğrular ve hangilerinin uyumlu olmadığını tanımlar.

Yeni kaynakların uyumlu olduğunu doğrulamak için ilkeler atama hakkında daha fazla bilgi edinmek için aşağıdakiler için öğreticiye devam edin:

> [!div class="nextstepaction"]
> [İlke oluşturma ve yönetme](./tutorials/create-and-manage.md)