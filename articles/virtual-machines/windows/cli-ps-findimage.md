---
title: Azure Marketi görüntülerini ve planlarını bulun ve kullanın
description: Market VM görüntüleri için yayımcı, teklif, SKU, sürüm ve plan bilgilerini bulmak ve kullanmak üzere Azure PowerShell kullanın.
author: cynthn
ms.service: virtual-machines
ms.subservice: imaging
ms.topic: how-to
ms.workload: infrastructure
ms.date: 12/07/2020
ms.author: cynthn
ms.openlocfilehash: 45e6b157dba5ef7410d8a5c0223fd3ecb52f39d0
ms.sourcegitcommit: 80c1056113a9d65b6db69c06ca79fa531b9e3a00
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/09/2020
ms.locfileid: "96906276"
---
# <a name="find-and-use-azure-marketplace-vm-images-with-azure-powershell"></a>Azure PowerShell ile Azure Market VM görüntülerini bulma ve kullanma     

Bu makalede, Azure Marketi 'nde VM görüntülerini bulmak için Azure PowerShell nasıl kullanılacağı açıklanır. Daha sonra bir sanal makine oluştururken Market görüntüsü ve plan bilgileri belirtebilirsiniz.

Ayrıca, [Azure Marketi](https://azuremarketplace.microsoft.com/) storefront, [Azure Portal](https://portal.azure.com)veya [Azure CLI](../linux/cli-ps-findimage.md)kullanarak kullanılabilir görüntülere ve tekliflere de gidebilirsiniz. 


[!INCLUDE [virtual-machines-common-image-terms](../../../includes/virtual-machines-common-image-terms.md)]


## <a name="create-a-vm-from-vhd-with-plan-information"></a>Plan bilgileriyle VHD 'den VM oluşturma

Azure Marketi görüntüsü kullanılarak oluşturulmuş bir VHD varsa, bu VHD 'den yeni bir VM oluşturduğunuzda satın alma planı bilgilerini sağlamanız gerekebilir.

Hala orijinal VM veya aynı görüntüden oluşturulmuş başka bir VM varsa, Get-AzVM kullanarak bu bilgisayardan plan adı, yayımcı ve ürün bilgilerini alabilirsiniz. Bu örnek *Myresourcegroup* kaynak grubundaki *myvm* adlı bir VM 'yi alır ve ardından satın alma planı bilgilerini görüntüler.

```azurepowershell-interactive
$vm = Get-azvm `
   -ResourceGroupName myResourceGroup `
   -Name myVM
$vm.Plan
```

Özgün VM silinmeden önce plan bilgilerini almadıysanız, bir [destek isteği](https://ms.portal.azure.com/#create/Microsoft.Support)dosyası gönderebilirsiniz. Bu, VM adı, abonelik kimliği ve silme işleminin zaman damgasına ihtiyaç duyar.

Bir VHD kullanarak bir VM oluşturmak için, [Özel BIR VHD 'den VM oluşturma](create-vm-specialized.md) ve bir satıra ekleme ' yi, [set-AzVMPlan](/powershell/module/az.compute/set-azvmplan) ' i kullanarak VM yapılandırmasına plan bilgilerini eklemek için aşağıdaki makaleye bakın:

```azurepowershell-interactive
$vmConfig = Set-AzVMPlan `
   -VM $vmConfig `
   -Publisher "publisherName" `
   -Product "productName" `
   -Name "planName"
```

## <a name="create-a-new-vm-from-a-marketplace-image"></a>Market görüntüsünden yeni bir VM oluşturma

Kullanmak istediğiniz görüntü hakkında daha fazla bilgi zaten varsa, VM yapılandırmasına görüntü bilgilerini eklemek için bu bilgileri [set-Azvmsourceımage](/powershell/module/az.compute/set-azvmsourceimage) cmdlet 'ine geçirebilirsiniz. Market 'te bulunan görüntüleri aramak ve listelemek için bir sonraki bölüme bakın.

Ayrıca, bazı ücretli görüntüler, [set-AzVMPlan](/powershell/module/az.compute/set-azvmplan)kullanarak satın alma planı bilgilerini sağlamanızı gerektirir. 

```powershell
...

$vmConfig = New-AzVMConfig -VMName "myVM" -VMSize Standard_D1

# Set the Marketplace image
$offerName = "windows-data-science-vm"
$skuName = "windows2016"
$version = "19.01.14"
$vmConfig = Set-AzVMSourceImage -VM $vmConfig -PublisherName $publisherName -Offer $offerName -Skus $skuName -Version $version

# Set the Marketplace plan information, if needed
$publisherName = "microsoft-ads"
$productName = "windows-data-science-vm"
$planName = "windows2016"
$vmConfig = Set-AzVMPlan -VM $vmConfig -Publisher $publisherName -Product $productName -Name $planName

...
```

Daha sonra VM yapılandırmasını diğer yapılandırma nesneleriyle birlikte cmdlet 'e geçireceğiz `New-AzVM` . PowerShell ile VM yapılandırması kullanmayla ilgili ayrıntılı bir örnek için, bu [komut dosyasına](https://github.com/Azure/azure-docs-powershell-samples/blob/master/virtual-machine/create-vm-detailed/create-windows-vm-detailed.ps1)bakın.

Görüntü koşullarını kabul etme hakkında bir ileti alırsanız, bu makalenin ilerleyen kısımlarında yer alarak [Koşulları kabul](#accept-the-terms) edin bölümüne bakın.

## <a name="list-images"></a>Resimleri Listele

Bir konumda görüntü bulmanın bir yolu [Get-Azvmımagepublisher](/powershell/module/az.compute/get-azvmimagepublisher), [Get-Azvmımageteklifinin](/powershell/module/az.compute/get-azvmimageoffer)ve [Get-Azvmımagesku](/powershell/module/az.compute/get-azvmimagesku) cmdlet 'lerini sırasıyla çalıştırmak olur:

1. Görüntü yayımcılarını listeleyin.
2. Belirli bir yayımcı varsa yayımcının tekliflerini listeleyin.
3. Belirli bir teklif varsa SKU’larını listeleyin.

Daha sonra, seçilen bir SKU için, dağıtılacak sürümleri listelemek için [Get-Azvmımage](/powershell/module/az.compute/get-azvmimage) komutunu çalıştırın.

1. Yayımcıları listeleyin:

    ```powershell
    $locName="<Azure location, such as West US>"
    Get-AzVMImagePublisher -Location $locName | Select PublisherName
    ```

2. Seçtiğiniz yayımcının adını girin ve teklifleri listeleyin:

    ```powershell
    $pubName="<publisher>"
    Get-AzVMImageOffer -Location $locName -PublisherName $pubName | Select Offer
    ```

3. Seçtiğiniz teklif adını girin ve SKU 'Ları listeleyin:

    ```powershell
    $offerName="<offer>"
    Get-AzVMImageSku -Location $locName -PublisherName $pubName -Offer $offerName | Select Skus
    ```

4. Seçtiğiniz SKU adınızı girin ve görüntü sürümünü alın:

    ```powershell
    $skuName="<SKU>"
    Get-AzVMImage -Location $locName -PublisherName $pubName -Offer $offerName -Sku $skuName | Select Version
    ```
    
`Get-AzVMImage`Komutun çıktısından, yeni bir sanal makine dağıtmak için bir sürüm görüntüsü seçebilirsiniz.

Aşağıdaki örnek komutların ve çıktıların tam dizisini gösterir:

```powershell
$locName="West US"
Get-AzVMImagePublisher -Location $locName | Select PublisherName
```

Kısmi çıkış:

```
PublisherName
-------------
...
abiquo
accedian
accellion
accessdata-group
accops
Acronis
Acronis.Backup
actian-corp
actian_matrix
actifio
activeeon
adgs
advantech
advantech-webaccess
advantys
...
```

*Microsoftwindowsserver* yayımcısı için:

```powershell
$pubName="MicrosoftWindowsServer"
Get-AzVMImageOffer -Location $locName -PublisherName $pubName | Select Offer
```

Çıkış:

```
Offer
-----
Windows-HUB
WindowsServer
WindowsServerSemiAnnual
```

*Windowsserver* teklifi için:

```powershell
$offerName="WindowsServer"
Get-AzVMImageSku -Location $locName -PublisherName $pubName -Offer $offerName | Select Skus
```

Kısmi çıkış:

```
Skus
----
2008-R2-SP1
2008-R2-SP1-smalldisk
2012-Datacenter
2012-Datacenter-smalldisk
2012-R2-Datacenter
2012-R2-Datacenter-smalldisk
2016-Datacenter
2016-Datacenter-Server-Core
2016-Datacenter-Server-Core-smalldisk
2016-Datacenter-smalldisk
2016-Datacenter-with-Containers
2016-Datacenter-with-RDSH
2019-Datacenter
2019-Datacenter-Core
2019-Datacenter-Core-smalldisk
2019-Datacenter-Core-with-Containers
...
```

Ardından, *2019-Datacenter* SKU 'su için:

```powershell
$skuName="2019-Datacenter"
Get-AzVMImage -Location $locName -PublisherName $pubName -Offer $offerName -Sku $skuName | Select Version
```

Artık seçili yayımcı, teklif, SKU ve sürümü bir URN (:) ile ayrılmış değerler) ile birleştirebilirsiniz. `--image` [New-azvm](/powershell/module/az.compute/new-azvm) cmdlet 'INI kullanarak bir VM oluşturduğunuzda bu urn parametresini parametresiyle geçirin. Görüntünün en son sürümünü almak için, isteğe bağlı olarak URN 'deki sürüm numarasını "en son" ile değiştirebilirsiniz.

Bir sanal makineyi Kaynak Yöneticisi şablonuyla dağıtırsanız, Özellikler içinde görüntü parametrelerini ayrı olarak ayarlarsınız `imageReference` . Bkz. [şablon başvurusu](/azure/templates/microsoft.compute/virtualmachines).

[!INCLUDE [virtual-machines-common-marketplace-plan](../../../includes/virtual-machines-common-marketplace-plan.md)]

### <a name="view-plan-properties"></a>Plan özelliklerini görüntüle

Bir görüntünün satın alma planı bilgilerini görüntülemek için `Get-AzVMImage` cmdlet 'ini çalıştırın. `PurchasePlan`Çıktıda özelliği yoksa `null` , görüntü, programlı dağıtımdan önce kabul etmeniz gereken koşulları içerir.  

Örneğin, *Windows Server 2016 Datacenter* görüntüsünün ek terimleri yoktur, bu nedenle `PurchasePlan` bilgiler şu şekilde olur `null` :

```powershell
$version = "2016.127.20170406"
Get-AzVMImage -Location $locName -PublisherName $pubName -Offer $offerName -Skus $skuName -Version $version
```

Çıkış:

```
Id               : /Subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/Providers/Microsoft.Compute/Locations/westus/Publishers/MicrosoftWindowsServer/ArtifactTypes/VMImage/Offers/WindowsServer/Skus/2016-Datacenter/Versions/2019.0.20190115
Location         : westus
PublisherName    : MicrosoftWindowsServer
Offer            : WindowsServer
Skus             : 2019-Datacenter
Version          : 2019.0.20190115
FilterExpression :
Name             : 2019.0.20190115
OSDiskImage      : {
                     "operatingSystem": "Windows"
                   }
PurchasePlan     : null
DataDiskImages   : []

```

Aşağıdaki örnekte, aşağıdaki özelliklere sahip *veri bilimi sanal makinesi-Windows 2016* görüntüsü için benzer bir komut gösterilmektedir `PurchasePlan` : `name` , `product` , ve `publisher` . Bazı görüntülerin de bir `promotion code` özelliği vardır. Bu görüntüyü dağıtmak için, koşulları kabul etmek ve programlı dağıtımı etkinleştirmek için aşağıdaki bölümlere bakın.

```powershell
Get-AzVMImage -Location "westus" -PublisherName "microsoft-ads" -Offer "windows-data-science-vm" -Skus "windows2016" -Version "0.2.02"
```

Çıkış:

```
Id               : /Subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/Providers/Microsoft.Compute/Locations/westus/Publishers/microsoft-ads/ArtifactTypes/VMImage/Offers/windows-data-science-vm/Skus/windows2016/Versions/19.01.14
Location         : westus
PublisherName    : microsoft-ads
Offer            : windows-data-science-vm
Skus             : windows2016
Version          : 19.01.14
FilterExpression :
Name             : 19.01.14
OSDiskImage      : {
                     "operatingSystem": "Windows"
                   }
PurchasePlan     : {
                     "publisher": "microsoft-ads",
                     "name": "windows2016",
                     "product": "windows-data-science-vm"
                   }
DataDiskImages   : []

```

### <a name="accept-the-terms"></a>Koşulları kabul edin

Lisans koşullarını görüntülemek için [Get-AzMarketplaceterms](/powershell/module/az.marketplaceordering/get-azmarketplaceterms) cmdlet 'ini kullanın ve satın alma planı parametrelerini geçirin. Çıktı, Market görüntüsüne yönelik koşullara bir bağlantı sağlar ve koşulları önceden kabul edip etmeyeceğinizi gösterir. Parametre değerlerinde tüm küçük harfleri kullandığınızdan emin olun.

```powershell
Get-AzMarketplaceterms -Publisher "microsoft-ads" -Product "windows-data-science-vm" -Name "windows2016"
```

Çıkış:

```
Publisher         : microsoft-ads
Product           : windows-data-science-vm
Plan              : windows2016
LicenseTextLink   : https://storelegalterms.blob.core.windows.net/legalterms/3E5ED_legalterms_MICROSOFT%253a2DADS%253a24WINDOWS%253a2DDATA%253a2DSCIENCE%253a2DVM%253a24WINDOWS2016%253a24OC5SKMQOXSED66BBSNTF4XRCS4XLOHP7QMPV54DQU7JCBZWYFP35IDPOWTUKXUC7ZAG7W6ZMDD6NHWNKUIVSYBZUTZ245F44SU5AD7Q.txt
PrivacyPolicyLink : https://www.microsoft.com/EN-US/privacystatement/OnlineServices/Default.aspx
Signature         : 2UMWH6PHSAIM4U22HXPXW25AL2NHUJ7Y7GRV27EBL6SUIDURGMYG6IIDO3P47FFIBBDFHZHSQTR7PNK6VIIRYJRQ3WXSE6BTNUNENXA
Accepted          : False
Signdate          : 1/25/2019 7:43:00 PM
```

Koşulları kabul etmek veya reddetmek için [set-AzMarketplaceterms](/powershell/module/az.marketplaceordering/set-azmarketplaceterms) cmdlet 'ini kullanın. Her görüntü için abonelik başına koşulları kabul etmeniz yeterlidir. Parametre değerlerinde tüm küçük harfleri kullandığınızdan emin olun. 

```powershell
$agreementTerms=Get-AzMarketplaceterms -Publisher "microsoft-ads" -Product "windows-data-science-vm" -Name "windows2016"

Set-AzMarketplaceTerms -Publisher "microsoft-ads" -Product "windows-data-science-vm" -Name "windows2016" -Terms $agreementTerms -Accept
```

Çıkış:

```
Publisher         : microsoft-ads
Product           : windows-data-science-vm
Plan              : windows2016
LicenseTextLink   : https://storelegalterms.blob.core.windows.net/legalterms/3E5ED_legalterms_MICROSOFT%253a2DADS%253a24WINDOWS%253a2DDATA%253a2DSCIENCE%253a2DV
                    M%253a24WINDOWS2016%253a24OC5SKMQOXSED66BBSNTF4XRCS4XLOHP7QMPV54DQU7JCBZWYFP35IDPOWTUKXUC7ZAG7W6ZMDD6NHWNKUIVSYBZUTZ245F44SU5AD7Q.txt
PrivacyPolicyLink : https://www.microsoft.com/EN-US/privacystatement/OnlineServices/Default.aspx
Signature         : XXXXXXK3MNJ5SROEG2BYDA2YGECU33GXTD3UFPLPC4BAVKAUL3PDYL3KBKBLG4ZCDJZVNSA7KJWTGMDSYDD6KRLV3LV274DLBXXXXXX
Accepted          : True
Signdate          : 2/23/2018 7:49:31 PM
```



## <a name="next-steps"></a>Sonraki adımlar

Temel görüntü bilgilerini kullanarak cmdlet 'le hızlı bir şekilde sanal makine oluşturmak için `New-AzVM` bkz. [PowerShell ile Windows sanal makinesi oluşturma](quick-create-powershell.md).

Paylaşılan görüntü galerisinde özel görüntüler oluşturmak üzere Azure Marketi görüntülerini kullanma hakkında daha fazla bilgi için bkz. [görüntü oluştururken Azure Marketi satın alma planı bilgilerini sağlama](../marketplace-images.md).
