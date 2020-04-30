---
author: linda33wj
ms.service: data-factory
ms.topic: include
ms.date: 11/09/2018
ms.author: jingwang
ms.openlocfilehash: b838e411e2795405c439a4107daab7aa8f033059
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "76021020"
---
## <a name="verify-the-output"></a>Çıktıyı doğrulama
İşlem hattı adftutorial blob kapsayıcısında çıktı klasörünü otomatik olarak oluşturur. Ardından, giriş klasöründeki emp.txt dosyasını çıktı klasörüne kopyalar. 

1. Çıktı klasörünü görmek için, Azure portalındaki **adftutorial** kapsayıcı sayfasında **Yenile**’ye tıklayın. 
    
    ![Yenile](media/data-factory-quickstart-verify-output-cleanup/output-refresh.png)
2. Klasör listesinde **output** öğesine tıklayın. 
2. **emp.txt** dosyasının output klasörüne kopyalandığını onaylayın. 

    ![Yenile](media/data-factory-quickstart-verify-output-cleanup/output-file.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme
Hızlı başlangıç bölümünde oluşturduğunuz kaynakları iki şekilde temizleyebilirsiniz. Kaynak grubundaki tüm kaynakları içeren [Azure kaynak grubunu](../articles/azure-resource-manager/management/overview.md) silebilirsiniz. Diğer kaynakları korumak istiyorsanız yalnızca bu öğreticide oluşturduğunuz veri kaynağını silin.

Bir kaynak grubunun silinmesi, içindeki veri fabrikaları dahil olmak üzere tüm kaynakları siler. Kaynak grubunun tamamını silmek için şu komutu çalıştırın: 
```powershell
Remove-AzResourceGroup -ResourceGroupName $resourcegroupname
```

Not: Bir kaynak grubunun bırakılması biraz zaman alabilir. Lütfen işlemin tamamlanmasını bekleyin

Tüm kaynak grubu yerine yalnızca veri fabrikasını silmek istiyorsanız aşağıdaki aşağıdaki komutu çalıştırın: 

```powershell
Remove-AzDataFactoryV2 -Name $dataFactoryName -ResourceGroupName $resourceGroupName
```