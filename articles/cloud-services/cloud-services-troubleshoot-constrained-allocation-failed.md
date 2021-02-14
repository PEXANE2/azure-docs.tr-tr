---
title: Azure 'a bir bulut hizmeti dağıtma sırasında ConstrainedAllocationFailed sorunlarını giderme | Microsoft Docs
description: Bu makalede, Azure 'a bir bulut hizmeti dağıtıldığında bir ConstrainedAllocationFailed özel durumunun nasıl çözümleneceği gösterilmektedir.
services: cloud-services
author: mibufo
ms.author: v-mibufo
ms.service: cloud-services
ms.topic: troubleshooting
ms.date: 02/04/2020
ms.openlocfilehash: de344bbcd89158676bacf2a8aa1743d282700b9d
ms.sourcegitcommit: e972837797dbad9dbaa01df93abd745cb357cde1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2021
ms.locfileid: "100521175"
---
# <a name="troubleshoot-constrainedallocationfailed-when-deploying-a-cloud-service-to-azure"></a>Azure 'a bulut hizmeti dağıtma sırasında ConstrainedAllocationFailed sorunlarını giderme

Bu makalede, Azure Cloud Services kısıtlamalar nedeniyle dağıtabeceği ayırma hataları giderilir.

Şunları yaptığınızda Microsoft Azure ayırır:

- Cloud Services örnekleri yükseltiliyor

- Yeni Web veya çalışan rolü örnekleri ekleme

- Bir bulut hizmetine örnek dağıtma

Azure abonelik sınırına ulaşmadan bile bu işlemler sırasında zaman zaman bir hata alabilirsiniz.

> [!TIP]
> Ayrıca, hizmetlerinizin dağıtımını planlarken bilgiler de yararlı olabilir.

## <a name="symptom"></a>Belirti

Azure portal, bulut hizmetinize gidin ve kenar çubuğu 'nda günlükleri görüntülemek için *işlem günlükleri (klasik)* seçeneğini belirleyin.

Bulut hizmetinizin günlüklerini incelerken aşağıdaki özel durumu görürsünüz:

|Özel durum türü  |Hata İletisi  |
|---------|---------|
|ConstrainedAllocationFailed     |' ' Azure işlemi `{Operation ID}` , kod işlem. ConstrainedAllocationFailed ile başarısız oldu. Ayrıntılar: ayırma başarısız oldu; istekteki kısıtlamalar karşılayamadı. İstenen yeni hizmet dağıtımı bir Benzeşim Grubuna bağlı veya bir Sanal Ağı hedefliyor ya da bu barındırılan hizmetin altında mevcut bir dağıtım var. Bu koşullardan herhangi biri, yeni dağıtımı belirli Azure kaynaklarına kısıtlar. Daha sonra yeniden deneyin veya VM boyutunu veya rol örneği sayısını azaltmayı deneyin. Alternatif olarak, mümkünse, yukarıda belirtilen kısıtlamaları kaldırın veya farklı bir bölgeye dağıtım yapmayı deneyin.|

## <a name="cause"></a>Nedeni

Dağıttığınız bölge veya kümeyle ilgili bir kapasite sorunu vardır. Seçtiğiniz kaynak SKU 'SU belirtilen konum için kullanılabilir olmadığında gerçekleşir.

> [!NOTE]
> Bir bulut hizmetinin ilk düğümü dağıtıldığında, bir kaynak havuzuna *sabitlenmiştir* . Kaynak havuzu tek bir küme veya bir grup küme olabilir.
>
> Zaman içinde, bu kaynak havuzundaki kaynaklar tam olarak kullanılabilir hale gelebilir. Bir bulut hizmeti, sabitlenmiş kaynak havuzunda yeterli kaynak olmadığında ek kaynaklar için bir ayırma isteği yapıyorsa, istek bir [ayırma hatasına](cloud-services-allocation-failures.md)neden olur.

## <a name="solution"></a>Çözüm

Bu senaryoda, bulut hizmetinizi dağıtmak için farklı bir bölge veya SKU seçmeniz gerekir. Bulut hizmetinizi dağıtmak veya yükseltmeden önce, bir bölgede veya kullanılabilirlik alanında hangi SKU 'Ların kullanılabildiğini belirleyebilirsiniz. Aşağıdaki [Azure CLI](#list-skus-in-region-using-azure-cli), [PowerShell](#list-skus-in-region-using-powershell)veya [REST API](#list-skus-in-region-using-rest-api) süreçlerini izleyin.

### <a name="list-skus-in-region-using-azure-cli"></a>Azure CLı kullanarak bölgedeki SKU 'Ları listeleme

[Az VM List-SKU](https://docs.microsoft.com/cli/azure/vm.html#az_vm_list_skus) komutunu kullanabilirsiniz.

- Kullanmakta olduğunuz `--location` konuma çıktıyı filtrelemek için parametresini kullanın.
- `--size`Kısmi bir boyut adına göre arama yapmak için parametresini kullanın.
- Daha fazla bilgi için bkz. [SKU kullanılamıyor Kılavuzu Için çözüm hatası](../azure-resource-manager/templates/error-sku-not-available.md#solution-2---azure-cli) .

    **Örneğin:**

    ```azurecli
    az vm list-skus --location southcentralus --size Standard_F --output table
    ```

    **Örnek sonuçlar:** ![ ' Az VM List-SKU--location Güneydoğu ABD--boyut Standard_F--çıkış tablosu ' komutunun çalıştırıldığı Azure CLı çıkışı kullanılabilir SKU 'Ları gösterir.](./media/cloud-services-troubleshoot-constrained-allocation-failed/cloud-services-troubleshoot-constrained-allocation-failed-1.png)

#### <a name="list-skus-in-region-using-powershell"></a>PowerShell kullanarak bölgedeki SKU 'Ları listeleme

[Get-AzComputeResourceSku](https://docs.microsoft.com/powershell/module/az.compute/get-azcomputeresourcesku) komutunu kullanabilirsiniz.

- Sonuçları konuma göre filtreleyin.
- Bu komut için en son PowerShell sürümüne sahip olmanız gerekir.
- Daha fazla bilgi için bkz. [SKU kullanılamıyor Kılavuzu Için çözüm hatası](../azure-resource-manager/templates/error-sku-not-available.md#solution-1---powershell) .

**Örneğin:**

```azurepowershell
Get-AzComputeResourceSku | where {$_.Locations -icontains "centralus"}
```

**Diğer bazı yararlı komutlar:**

Boyut (Standard_DS14_v2) içeren konumları filtreleyin:

```azurepowershell
Get-AzComputeResourceSku | where {$_.Locations.Contains("centralus") -and $_.ResourceType.Contains("virtualMachines") -and $_.Name.Contains("Standard_DS14_v2")}
```

Boyut (v3) içeren tüm konumları filtreleyin:

```azurepowershell
Get-AzComputeResourceSku | where {$_.Locations.Contains("centralus") -and $_.ResourceType.Contains("virtualMachines") -and $_.Name.Contains("v3")} | fc
```

#### <a name="list-skus-in-region-using-rest-api"></a>REST API kullanarak bölgede SKU 'Ları listeleme

[Kaynak SKU 'ları-listeleme](https://docs.microsoft.com/rest/api/compute/resourceskus/list) işlemini kullanabilirsiniz. Kullanılabilir SKU 'Ları ve bölgeleri aşağıdaki biçimde döndürür:

```json
{
  "value": [
    {
      "resourceType": "virtualMachines",
      "name": "Standard_A0",
      "tier": "Standard",
      "size": "A0",
      "locations": [
        "eastus"
      ],
      "restrictions": []
    },
    {
      "resourceType": "virtualMachines",
      "name": "Standard_A1",
      "tier": "Standard",
      "size": "A1",
      "locations": [
        "eastus"
      ],
      "restrictions": []
    },
    <Rest_of_your_file_is_located_here...>
  ]
}
    
```

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla ayırma hatası çözümü ve bunların nasıl oluşturulduğunu daha iyi anlamak için:

> [!div class="nextstepaction"]
> [Ayırma arızaları (bulut Hizmetleri)](cloud-services-allocation-failures.md)

Azure sorununuz bu makalede giderilmemişse [MSDN ve Stack Overflow](https://azure.microsoft.com/support/forums/)Azure forumlarını ziyaret edin. Sorununuzu bu forumlara gönderebilir veya [ @AzureSupport Twitter 'da](https://twitter.com/AzureSupport)ilan edebilirsiniz. Ayrıca, bir Azure destek isteği gönderebilirsiniz. Destek isteği göndermek için [Azure desteği](https://azure.microsoft.com/support/options/) sayfasında *Destek Al*' ı seçin.
