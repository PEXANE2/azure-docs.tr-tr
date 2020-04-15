---
title: Quickstart - Azure Kaynak Yöneticisi şablonu kullanarak Bir Azure Analiz Hizmetleri sunucu kaynağı oluşturma
description: Azure Kaynak Yöneticisi şablonu kullanarak Azure Çözümhizmetleri sunucu kaynağının nasıl yapılacağını hızlı bir şekilde gösterin.
author: minewiskan
ms.author: owend
tags: azure-resource-manager
ms.service: analysis-services
ms.topic: quickstart
ms.date: 04/14/2020
ms.custom: subject-armqs
ms.openlocfilehash: d292500c5e26d3c07ff2402964166b3928cc7e44
ms.sourcegitcommit: ea006cd8e62888271b2601d5ed4ec78fb40e8427
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81384260"
---
# <a name="quickstart-create-a-server---azure-resource-manager-template"></a>Hızlı başlangıç: Sunucu oluşturma - Azure Kaynak Yöneticisi şablonu

Bu hızlı başlangıç, Kaynak Yöneticisi şablonu kullanarak Azure aboneliğinizde Bir Analiz Hizmetleri sunucusu kaynağının nasıl oluşturulturunu açıklar.

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

## <a name="prerequisites"></a>Ön koşullar

* **Azure aboneliği**: Hesap oluşturmak için [Azure Ücretsiz Deneme Sürümü](https://azure.microsoft.com/offers/ms-azr-0044p/)’nü ziyaret edin.
* **Azure Active Directory**: Aboneliğinizin bir Azure Active Directory Kiracısı ile ilişkilendirilmiş olması gerekir. Ayrıca bu Azure Active Directory içindeki bir hesapla Azure'da oturum açmış olmanız gerekir. Daha fazla bilgi edinmek için bkz. [Kimlik doğrulaması ve kullanıcı izinleri](analysis-services-manage-users.md).

## <a name="create-a-server"></a>Sunucu oluşturma

### <a name="review-the-template"></a>Şablonu gözden geçirme

Bu hızlı başlatmada kullanılan şablon [Azure Quickstart şablonlarındandır.](https://azure.microsoft.com/resources/templates/101-analysis-services-create/)

:::code language="json" source="~/quickstart-templates/101-analysis-services-create/azuredeploy.json":::

Şablonda güvenlik duvarı kuralına sahip tek bir [Microsoft.AnalysisServices/servers](https://docs.microsoft.com/azure/templates/microsoft.analysisservices/2017-08-01/servers) kaynağı tanımlanır. 

### <a name="deploy-the-template"></a>Şablonu dağıtma

1. Azure'da oturum açmak ve şablon açmak için aşağıdaki Azure'a Dağıt bağlantısını seçin. Şablon, bir Çözümleme Hizmetleri sunucusu kaynağı oluşturmak ve gerekli ve isteğe bağlı özellikleri belirtmek için kullanılır.

   <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-analysis-services-create%2Fazuredeploy.json"><img src="./media/analysis-services-create-template/deploy-azure.png" alt="deploy to azure"/></a>

2. Aşağıdaki değerleri seçin veya girin.

    Aksi belirtilmedikçe, varsayılan değerleri kullanın.

    * **Abonelik**: Azure aboneliği seçin.
    * **Kaynak grubu**: **Yeni Oluştur'u**tıklatın ve ardından yeni kaynak grubu için benzersiz bir ad girin.
    * **Konum**: Kaynak grubunda oluşturulan kaynaklar için varsayılan konum seçin.
    * **Sunucu Adı**: Sunucu kaynağıiçin bir ad girin. 
    * **Yer**: Analiz Hizmetleri için yoksay. Konum Sunucu Konumu'nda belirtilir.
    * **Sunucu konumu**: Çözümleme Hizmetleri sunucusunun konumunu girin. Bu genellikle Kaynak Grubu için belirtilen varsayılan Konum ile aynı bölgedir, ancak gerekli değildir. Örneğin **Kuzey Orta ABD**. Desteklenen bölgeler için bölgeye [göre Analiz Hizmetleri kullanılabilirliğine](analysis-services-overview.md#availability-by-region)bakın.
    * **Sku Adı**: Oluşturmak için Analiz Hizmetleri sunucusu için sku adını girin. Seçim: B1, B2, D1, S0, S1, S2, S3, S4, S8v2, S9v2. Sku kullanılabilirliği bölgeye bağlıdır. Değerlendirme ve test için S0 veya D1 önerilir.
    * **Kapasite**: Sorgu çoğaltma ölçeklendirme örneklerinin toplam sayısını girin. Yalnızca belirli bölgelerde birden fazla örnekten ölçeklendirme desteklenir.
    * **Güvenlik Duvarı Ayarları**: Sunucu için tanımlamak için gelen güvenlik duvarı kurallarını girin. Belirtilmemişse, güvenlik duvarı devre dışı bırakılır.
    * **Yedek Blob Container Uri**: SAS URI'yi okuma, yazma ve liste izinleriyle özel bir Azure Blob Depolama kabına girin. Yalnızca [Yedekleme/geri yükleme](analysis-services-backup.md)kullanmak istiyorsanız gereklidir.
    * **Yukarıda belirtilen hüküm ve koşulları kabul ediyorum**: Seçin.

3. **Satın al**'ı seçin. Sunucu başarıyla dağıtıldıktan sonra bir bildirim alırsınız:

   ![Kaynak Yöneticisi şablonu, portal bildirimi dağıtma](./media/analysis-services-create-template/notification.png)

## <a name="validate-the-deployment"></a>Dağıtımı doğrulama

Kaynak grubunu ve sunucu kaynağının oluşturulduğunu doğrulamak için Azure portalını veya Azure PowerShell'i kullanın.

#### <a name="powershell"></a>PowerShell

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
(Get-AzResource -ResourceType "Microsoft.AnalysisServices/servers" -ResourceGroupName $resourceGroupName).Name
 Write-Host "Press [ENTER] to continue..."
```

---

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekmediğinde, kaynak grubunu ve sunucu kaynağını silmek için Azure portalını, Azure CLI'yi veya Azure PowerShell'i kullanın.

# <a name="cli"></a>[CLI](#tab/CLI)

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
az group delete --name $resourceGroupName &&
echo "Press [ENTER] to continue ..."
```

# <a name="powershell"></a>[PowerShell](#tab/PowerShell)

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
Remove-AzResourceGroup -Name $resourceGroupName
Write-Host "Press [ENTER] to continue..."
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, yeni bir kaynak grubu ve bir Azure Çözümleme Hizmetleri sunucu kaynağı oluşturmak için bir Azure Kaynak Yöneticisi şablonu kullandınız. Şablonu kullanarak bir sunucu kaynağı oluşturduktan sonra aşağıdakileri göz önünde bulundurun:
- [Hızlı başlangıç: Sunucu oluşturma - PowerShell](analysis-services-create-powershell.md)
- [Portaldan örnek model ekleme](analysis-services-create-sample-model.md)
- [Sunucu yönetici ve kullanıcı rollerini yapılandırma](tutorials/analysis-services-tutorial-roles.md)