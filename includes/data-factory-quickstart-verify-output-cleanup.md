---
ms.service: data-factory
ms.topic: include
ms.date: 11/09/2018
author: linda33wj
ms.author: jingwang
ms.openlocfilehash: c0fcdf1cf69c55f63288138bc7377a78295cb2be
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86544511"
---
## <a name="review-deployed-resources"></a>Dağıtılan kaynakları gözden geçirme

İşlem hattı adftutorial blob kapsayıcısında çıktı klasörünü otomatik olarak oluşturur. Ardından, giriş klasöründeki emp.txt dosyasını çıktı klasörüne kopyalar. 

1. Azure portal, **adföğreticisi** kapsayıcı sayfasında, çıktı klasörünü görmek için **Yenile** ' yi seçin. 
    
    ![Yenile](media/data-factory-quickstart-verify-output-cleanup/output-refresh.png)

2. Klasör listesinde **Çıkış ' ı** seçin. 

3. **emp.txt** dosyasının output klasörüne kopyalandığını onaylayın. 

    ![Yenile](media/data-factory-quickstart-verify-output-cleanup/output-file.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Hızlı başlangıç bölümünde oluşturduğunuz kaynakları iki şekilde temizleyebilirsiniz. Kaynak grubundaki tüm kaynakları içeren [Azure kaynak grubunu](../articles/azure-resource-manager/management/overview.md) silebilirsiniz. Diğer kaynakları korumak istiyorsanız yalnızca bu öğreticide oluşturduğunuz veri kaynağını silin.

Bir kaynak grubunun silinmesi, içindeki veri fabrikaları dahil olmak üzere tüm kaynakları siler. Kaynak grubunun tamamını silmek için şu komutu çalıştırın: 

```powershell
Remove-AzResourceGroup -ResourceGroupName $resourcegroupname
```

> [!Note]
> Kaynak grubunun düşürülme zaman alabilir. Lütfen işlemin tamamlanmasını bekleyin

Tüm kaynak grubu yerine yalnızca veri fabrikasını silmek istiyorsanız aşağıdaki aşağıdaki komutu çalıştırın: 

```powershell
Remove-AzDataFactoryV2 -Name $dataFactoryName -ResourceGroupName $resourceGroupName
```