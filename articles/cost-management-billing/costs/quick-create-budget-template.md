---
title: Hızlı Başlangıç - Azure Resource Manager şablonuyla bütçe oluşturma
description: Azure Resource Manager şablonuyla nasıl bütçe oluşturulacağını gösteren hızlı başlangıç.
author: bandersmsft
ms.author: banders
tags: azure-resource-manager
ms.service: cost-management-billing
ms.topic: quickstart
ms.date: 06/10/2020
ms.custom: subject-armqs
ms.openlocfilehash: dc37039d6777a77f9de247808329930f1621ee82
ms.sourcegitcommit: 537c539344ee44b07862f317d453267f2b7b2ca6
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2020
ms.locfileid: "84686418"
---
# <a name="quickstart-create-a-budget-with-an-azure-resource-manager-template"></a>Hızlı Başlangıç: Bir Azure Resource Manager şablonuyla bütçe oluşturma

Maliyet Yönetimi hizmetindeki bütçe işlevi, kuruluşunuzda sorumluluk kültürünü planlamanıza ve güçlendirmenize yardımcı olur. Bütçeleri kullanarak belirli bir dönem içinde kullandığınız veya abone olduğunuz Azure hizmetlerini takip edebilirsiniz. Bütçeler, maliyetleri önceden yönetmek için diğer kullanıcıları harcamaları konusunda bilgilendirmenize ve zaman içindeki harcamayı izlemenize yardımcı olur. Oluşturduğunuz bütçe eşikleri aşıldığında bildirimler tetiklenir. Kaynaklarınızın hiçbiri etkilenmez ve tüketiminiz durdurulmaz. Bütçeleri kullanarak maliyet analizi sırasında harcamaları karşılaştırabilir ve izleyebilirsiniz. Bu hızlı başlangıçta, Resource Manager şablonu kullanılarak nasıl bütçe oluşturulacağı gösterilir.

[!INCLUDE [About Azure Resource Manager](../../../includes/resource-manager-quickstart-introduction.md)]

## <a name="prerequisites"></a>Ön koşullar

Azure Resource Manager şablonu yalnızca Kurumsal Anlaşmalar (EA) kapsamındaki Azure aboneliklerini destekler. Diğer abonelik türleri şablon tarafından desteklenmez.

Bütçe oluşturmak ve yönetmek için katkıda bulunan izninizin olması gerekir. EA abonelikleri ve kaynak grupları için ayrı bütçeler oluşturabilirsiniz. Ancak EA ödeme hesapları için bütçe oluşturamazsınız. Azure EA aboneliklerinde bütçeleri görüntülemek için okuma yetkisine sahip olmanız gerekir.

Bütçe oluşturulduktan sonra Azure hesabınızın bütçeleri görüntüleyebilmesi için en azından okuma erişiminiz olmalıdır.

Yeni bir aboneliğiniz varsa hemen bir bütçe oluşturamaz veya diğer Maliyet Yönetimi özelliklerini kullanamazsınız. Maliyet Yönetimi özelliklerini kullanabilmek için 48 saat kadar beklemeniz gerekebilir.

Aşağıdaki Azure izinleri veya kapsamları abonelik başına kullanıcı ve grup tarafından bütçe oluşturulması için desteklenir. Kapsamlar hakkında daha fazla bilgi için bkz. [Kapsamları anlama ve birlikte çalışma](understand-work-scopes.md).

- Sahip – Bir abonelik için bütçe oluşturabilir, değiştirebilir veya silebilir.
- Katkıda bulunan ve Maliyet Yönetimi katkıda bulunanı - Kendi bütçelerini oluşturabilir, değiştirebilir veya silebilir. Başkaları tarafından oluşturulan bütçelerin miktarlarını değiştirebilir.
- Okuyucu ve Maliyet Yönetimi okuyucusu - İzin verilen bütçeleri görüntüleyebilir.

Maliyet Yönetimi verilerine izin atama hakkında daha fazla bilgi için bkz. [Maliyet Yönetimi verilerine erişim atama](assign-access-acm-data.md).

## <a name="create-a-budget"></a>Bütçe oluşturma

### <a name="review-the-template"></a>Şablonu gözden geçirme

Bu hızlı başlangıçta kullanılan şablon [Azure Hızlı Başlangıç şablonlarından](https://azure.microsoft.com/resources/templates/create-budget) alınmıştır.

:::code language="json" source="~/quickstart-templates/create-budget/azuredeploy.json" range="1-146" highlight="110-139":::

Şablonda bir Azure kaynağı tanımlanmıştır:

* [Microsoft.Consumption/budgets](/azure/templates/microsoft.consumption/budgets): Azure bütçesi oluşturma.

### <a name="deploy-the-template"></a>Şablonu dağıtma

1. Aşağıdaki görüntüyü seçerek Azure'da oturum açıp bir şablon açın. Şablon bir bütçe oluşturur.

   [![Azure’a dağıtma](../../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fcreate-budget%2Fazuredeploy.json)

2. Aşağıdaki değerleri seçin veya girin.

   [![Resource Manager şablonu, Bütçe oluştur, portala dağıt](./media/quick-create-budget-template/create-budget-using-template-portal.png)](./media/quick-create-budget-template/create-budget-using-template-portal.png#lightbox)

    * **Abonelik**: Bir Azure aboneliği seçin.
    * **Kaynak grubu**: **Yeni oluştur**’u seçin, kaynak grubu için benzersiz bir ad girin ve ardından **Tamam**’a tıklayın veya mevcut bir kaynak grubunu seçin.
    * **Konum**: Bir konum seçin. Örneğin **Orta ABD**.
    * **Bütçe Adı**: Bütçe için bir ad girin. Ad, kaynak grubu içinde benzersiz olmalıdır. Yalnızca alfasayısal karakterler, alt çizgi ve kısa çizgi kullanılabilir.
    * **Tutar**: Bütçeyle izlenecek toplam maliyet veya kullanım tutarını girin.
    * **Bütçe Kategorisi**: Bütçenin kategorisini seçin; bütçe **Maliyet**’i veya **Kullanım**’ı izleyebilir.
    * **Zaman Dilimi**: Bütçenin kapsadığı süreyi girin. İzin verilen değerler Aylık, Üç Aylık ve Yıllık değerleridir. Zaman diliminin sonunda bütçe sıfırlanır.
    * **Başlangıç Tarihi**: Ayın ilk ayını gösteren başlangıç tarihini YYYY-AA-GG biçiminde girin. Gelecekteki başlangıç tarihleri bugünden en fazla üç ay sonraya denk gelebilir. Saat Dilimi ile geçmiş bir başlangıç tarihi belirtebilirsiniz.
    * **Bitiş Tarihi**: Bütçenin bitiş tarihini YYYY-AA-GG biçiminde girin. Bu değer sağlanmazsa varsayılan olarak başlangıç tarihinden 10 yıl sonraya ayarlanır.
    * **İşleç**: Karşılaştırma işlecini seçin. Olası değerler EqualTo, GreaterThan veya GreaterThanOrEqualTo işleçleridir.
    * **Eşik**: Bildirim için eşik değerini girin. Maliyet eşiği aştığında bildirim gönderilir. Bu her zaman yüzde değeridir ve 0 ile 1000 arasında olmalıdır.
    * **İlgili Kişi E-postaları**: Eşik aşıldığında bütçe bildiriminin gönderileceği e-posta adresleri listesini girin. Beklenen biçim: `["user1@domain.com","user2@domain.com"]`.
    * **İlgili Kişi Rolleri**: Eşik aşıldığında bütçe bildiriminin gönderileceği ilgili kişi rollerinin listesini girin. Varsayılan değerler Sahip, Katkıda Bulunan ve Okuyucu’dur. Beklenen biçim: `["Owner","Contributor","Reader"]`.
    * **İlgili Kişi Grupları**, eşik aşıldığında bütçe bildiriminin gönderileceği eylem grubu kaynak kimlikleri listesini tam kaynak URI'leri olarak girin. Dize dizileri kabul edilir. Beklenen biçim: `["action group resource ID1","action group resource ID2"]`. Eylem gruplarını kullanmak istemiyorsanız `[]` girin.
    * **Kaynak Filtresi**: Kaynaklar için filtre listesini girin. Beklenen biçim: `["Resource Filter Name1","Resource Filter Name2"]`. Filtre uygulamak istemiyorsanız `[]` girin. Kaynak filtresi girerseniz, **ölçüm filtreleri** değerlerini de girmelisiniz.
    * **Ölçüm Filtresi**: Ölçüm filtreleri listesini girin; **Kullanım** bütçe kategorisindeki bütçeler için zorunludur. Beklenen biçim: `["Meter Filter Name1","Meter Filter Name2"]`. **Kaynak filtresi** girmediyseniz `[]` girin.
    * **Yukarıda belirtilen hüküm ve koşulları kabul ediyorum**: Bunu seçin.

3. **Satın al**'ı seçin. Bütçe başarıyla dağıtıldıktan sonra bir bildirim alırsınız:

   ![Resource Manager şablonu, bütçe, portala dağıtma bildirimi](./media/quick-create-budget-template/resource-manager-template-portal-deployment-notification.png)

Şablonu dağıtmak için Azure portalı kullanılır. Azure portalının yanı sıra Azure PowerShell, Azure CLI ve REST API de kullanabilirsiniz. Diğer dağıtım şablonları hakkında bilgi edinmek için bkz. [Dağıtım şablonları](../../azure-resource-manager/templates/deploy-powershell.md).

## <a name="validate-the-deployment"></a>Dağıtımı doğrulama

Azure portalını kullanıp **Maliyet Yönetimi + Faturalama** > kapsam seçin > **Bütçeler**’e giderek bütçenin oluşturulduğunu doğrulayabilirsiniz. İsterseniz, bütçeyi görüntülemek için aşağıdaki Azure CLI veya Azure PowerShell betiklerini de kullanabilirsiniz.

# <a name="cli"></a>[CLI](#tab/CLI)

```azurecli-interactive
az consumption budget list
```

# <a name="powershell"></a>[PowerShell](#tab/PowerShell)

```azurepowershell-interactive
Get-AzConsumptionBudget
```

---

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bütçeye ihtiyacınız olmadığında, aşağıdaki yöntemlerden birini kullanarak bütçeyi silin:

### <a name="azure-portal"></a>Azure portal

**Maliyet Yönetimi + Faturalandırma**’ya gidip bir faturalama kapsamı seçin, **Bütçeler**’den bir bütçe seçin ve sonra **Bütçeyi sil** seçeneğini belirleyin.

### <a name="command-line"></a>Komut satırı

Azure CLI veya Azure PowerShell kullanarak bütçeyi kaldırabilirsiniz.

# <a name="cli"></a>[CLI](#tab/CLI)

```azurecli-interactive
echo "Enter the budget name:" &&
read budgetName &&
az consumption budget delete --budget-name $budgetName &&
echo "Press [ENTER] to continue ..."
```

# <a name="powershell"></a>[PowerShell](#tab/PowerShell)

```azurepowershell-interactive
$budgetName = Read-Host -Prompt "Enter the budget name"
Remove-AzConsumptionBudget -Name $budgetName
Write-Host "Press [ENTER] to continue..."
```

---

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta dağıtım için bir Azure bütçesi oluşturdunuz. Azure Maliyet Yönetimi ve Faturalama ile Azure Resource Manager hakkında daha fazla bilgi edinmek için aşağıdaki makalelerden devam edin.

- [Maliyet Yönetimi ve Faturalama](../cost-management-billing-overview.md) konusuna genel bakış bilgilerini okuyun
- Azure portalında [bütçe oluşturma](tutorial-acm-create-budgets.md)
- [Azure Resource Manager](../../azure-resource-manager/management/overview.md) hakkında daha fazla bilgi edinin
