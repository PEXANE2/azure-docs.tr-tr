---
title: Azure portalında dışa aktarma şablonu
description: Aboneliğinizdeki kaynaklardan bir Azure Kaynak Yöneticisi şablonu dışa aktarmak için Azure portalını kullanın.
ms.topic: conceptual
ms.date: 12/12/2019
ms.openlocfilehash: 8cdba58a7a2ba998bac7fc0225ff957047cd69b0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79273741"
---
# <a name="single-and-multi-resource-export-to-a-template-in-azure-portal"></a>Azure portalındaki şablona tek ve çok kaynak lı dışa aktarma

Azure Kaynak Yöneticisi şablonları oluşturmaya yardımcı olmak için, varolan kaynaklardan bir şablon dışa aktarabilirsiniz. Dışa aktarılan şablon, Kaynaklarınızı dağıtan JSON sözdizimini ve özelliklerini anlamanıza yardımcı olur. Gelecekteki dağıtımları otomatikleştirmek için dışa aktarılan şablonla başlayın ve senaryonuz için değiştirin.

Kaynak Yöneticisi, şablona dışa aktarmak için bir veya daha fazla kaynak seçmeni sağlar. Şablonda tam olarak gereksinim duyduğunuz kaynaklara odaklanabilirsiniz.

Bu makalede, portal üzerinden şablonların nasıl dışa aktarılabildiğini gösterir. [Azure CLI, Azure](../management/manage-resource-groups-cli.md#export-resource-groups-to-templates) [PowerShell](../management/manage-resource-groups-powershell.md#export-resource-groups-to-templates)veya [REST API'yi](/rest/api/resources/resourcegroups/exporttemplate)de kullanabilirsiniz.

## <a name="choose-the-right-export-option"></a>Doğru dışa aktarma seçeneğini seçin

Şablonu dışarı aktarmanın iki yolu vardır:

* **Kaynak grubundan veya kaynaktan dışa aktarma.** Bu seçenek, varolan kaynaklardan yeni bir şablon oluşturur. Dışa aktarılan şablon, kaynak grubunun geçerli durumunun bir "anlık görüntüsü"dür. Bu kaynak grubu içindeki tüm kaynak grubunu veya belirli kaynakları dışa aktarabilirsiniz.

* **Dağıtımdan önce veya tarihten dışa aktarma.** Bu seçenek, dağıtım için kullanılan şablonun tam bir kopyasını alır.

Seçtiğiniz seçene bağlı olarak, dışa aktarılan şablonlar farklı niteliklere sahiptir.

| Kaynak grubundan veya kaynaktan | Dağıtımdan önce veya tarihten |
| --------------------- | ----------------- |
| Şablon, kaynakların geçerli durumunun anlık görüntüsüdür. Dağıtımdan sonra yaptığınız el ile değişiklikleri içerir. | Şablon yalnızca dağıtım sırasındaki kaynakların durumunu gösterir. Dağıtımdan sonra yaptığınız el ile yapılan değişiklikler dahil değildir. |
| Bir kaynak grubundan hangi kaynakların dışa aktarılabileceğini seçebilirsiniz. | Belirli bir dağıtım için tüm kaynaklar dahildir. Bu kaynakların bir alt kümesini seçemezsiniz veya farklı bir zamanda eklenen kaynaklar ekemezsiniz. |
| Şablon, dağıtım sırasında normalde ayarlamadığınız bazı özellikler de dahil olmak üzere kaynaklar için tüm özellikleri içerir. Şablonu yeniden kullanmadan önce bu özellikleri kaldırmak veya temizlemek isteyebilirsiniz. | Şablon yalnızca dağıtım için gereken özellikleri içerir. Şablon kullanıma hazırdır. |
| Şablon büyük olasılıkla yeniden kullanmak için gereken tüm parametreleri içermez. Çoğu özellik değeri şablonda sabit kodlanır. Şablonu diğer ortamlarda yeniden dağıtmak için, kaynakları yapılandırma yeteneğini artıran parametreler eklemeniz gerekir.  Kendi parametrelerinizi yazabilmek için **Include parametrelerini** seçebilirsiniz. | Şablon, farklı ortamlarda yeniden dağıtılmasını kolaylaştıran parametreler içerir. |

Aşağıdaki anda şablonu bir kaynak grubundan veya kaynaktan dışa aktarma:

* Özgün dağıtımdan sonra yapılan kaynaklardaki değişiklikleri yakalamanız gerekir.
* Hangi kaynakların dışa aktarılmasını seçmek istiyorsunuz.

Şablonu dağıtımdan önce veya geçmişten, aşağıdaki zaman dışa aktarma:

* Yeniden kullanımı kolay bir şablon istiyorsunuz.
* Özgün dağıtımdan sonra yaptığınız değişiklikleri eklemeniz gerekmez.

## <a name="limitations"></a>Sınırlamalar

Bir kaynak grubundan veya kaynaktan dışa aktarırken, dışa aktarılan şablon her kaynak türü için [yayımlanmış şemalardan](https://github.com/Azure/azure-resource-manager-schemas/tree/master/schemas) oluşturulur. Bazen, şema bir kaynak türü için en son sürümü yok. Gereksinim duyduğunuz özellikleri içerdiğinden emin olmak için dışa aktarılan şablonunuzu denetleyin. Gerekirse, gereksinim duyduğunuz API sürümünü kullanmak için dışa aktarılan şablonu edin.

Dışa aktarma şablonu özelliği Azure Veri Fabrikası kaynaklarını dışa aktarmayı desteklemez. Veri Fabrikası kaynaklarını nasıl dışa aktarabileceğiniz hakkında bilgi edinmek için Azure [Veri Fabrikası'nda veri fabrikasıkopyala veya klonlama'ya](https://aka.ms/exportTemplateViaAdf)bakın.

Klasik dağıtım modeli yle oluşturulan kaynakları dışa aktarmak [için kaynak yöneticisi dağıtım modeline geçirmeniz](https://aka.ms/migrateclassicresourcetoarm)gerekir.

## <a name="export-template-from-a-resource-group"></a>Kaynak grubundan şablon uy

Kaynak grubundan bir veya daha fazla kaynak dışa aktarmak için:

1. Dışa aktarmak istediğiniz kaynakları içeren kaynak grubunu seçin.

1. Onay kutularını seçerek bir veya daha fazla kaynak seçin.  Tümünü seçmek **için, Ad'In**solundaki onay kutusunu seçin. **Dışa Aktarma şablonu** menüsü öğesi yalnızca en az bir kaynak seçtikten sonra etkinleştirilir.

   ![Tüm kaynakları dışa aktarma](./media/export-template-portal/select-all-resources.png)

    Ekran görüntüsünde yalnızca depolama hesabı seçilir.
1. **Dışa Aktar şablonu'nun**

1. Dışa aktarılan şablon görüntülenir ve karşıdan yüklenebilir ve dağıtılabedilebilir.

   ![Şablonu göster](./media/export-template-portal/show-template.png)

   **Include parametreleri** varsayılan olarak seçilir.  Seçildiğinde, şablon oluşturulduğunda tüm şablon parametreleri dahil edilir. Kendi parametrelerinizi yazaristiyorsanız, bu onay kutusunu bunları içermeyecek şekilde değiştirin.

## <a name="export-template-from-a-resource"></a>Kaynaktan şablon dışa aktarma

Bir kaynak dışa aktarmak için:

1. Dışa aktarmak istediğiniz kaynağı içeren kaynak grubunu seçin.

1. Kaynağı açmak için dışa aktarmak istediğiniz kaynağı seçin.

1. Bu kaynak için, sol bölmedeki Dışa Aktarma **şablonu'nu** seçin.

   ![Kaynak dışa aktarma](./media/export-template-portal/export-single-resource.png)

1. Dışa aktarılan şablon görüntülenir ve karşıdan yüklenebilir ve dağıtılabedilebilir. Şablon yalnızca tek bir kaynak içerir. **Include parametreleri** varsayılan olarak seçilir.  Seçildiğinde, şablon oluşturulduğunda tüm şablon parametreleri dahil edilir. Kendi parametrelerinizi yazaristiyorsanız, bu onay kutusunu bunları içermeyecek şekilde değiştirin.

## <a name="export-template-before-deployment"></a>Dağıtımdan önce şablonu dışa aktarma

1. Dağıtmak istediğiniz Azure hizmetini seçin.

1. Yeni hizmetin değerlerini doldurun.

1. Doğrulamayı geçtikten sonra, ancak dağıtımı başlatmadan **önce, otomasyon için şablon indir'i**seçin.

   ![Şablon indirme](./media/export-template-portal/download-before-deployment.png)

1. Şablon görüntülenir ve karşıdan yükleme ve dağıtma için kullanılabilir.


## <a name="export-template-after-deployment"></a>Dağıtımdan sonra dışa aktarma şablonu

Varolan kaynakları dağıtmak için kullanılan şablonu dışa aktarabilirsiniz. Aldığınız şablon tam olarak dağıtım için kullanılan şablondur.

1. Dışa aktarmak istediğiniz kaynak grubunu seçin.

1. **Dağıtımlar**altındaki bağlantıyı seçin.

   ![Dağıtım geçmişini seçin](./media/export-template-portal/select-deployment-history.png)

1. Dağıtım geçmişinden dağıtımlardan birini seçin.

   ![Dağıtımı seçin](./media/export-template-portal/select-details.png)

1. **Şablon'u**seçin. Bu dağıtım için kullanılan şablon görüntülenir ve karşıdan yüklenebilir.

   ![Şablon seçme](./media/export-template-portal/show-template-from-history.png)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure CLI](../management/manage-resource-groups-cli.md#export-resource-groups-to-templates), [Azure PowerShell](../management/manage-resource-groups-powershell.md#export-resource-groups-to-templates)veya REST [API](/rest/api/resources/resourcegroups/exporttemplate)ile şablonları nasıl dışa aktarırın öğrenin.
- Kaynak Yöneticisi şablonsözdizimini öğrenmek için [bkz.](template-syntax.md)
- Şablonları nasıl geliştireceklerini öğrenmek için [adım adım öğreticilere](/azure/azure-resource-manager/)bakın.
- Azure Kaynak Yöneticisi şablon şemalarını görüntülemek için [şablon başvurusuna](/azure/templates/)bakın.
