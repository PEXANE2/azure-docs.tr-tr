---
title: Azure'da Windows VM resimlerini seçin
description: Market VM görüntüleri için yayımcı, teklif, SKU ve sürümü belirlemek için Azure PowerShell'i kullanın.
author: cynthn
ms.service: virtual-machines-windows
ms.subservice: imaging
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 01/25/2019
ms.author: cynthn
ms.openlocfilehash: 46a2badbbe957f6a8a6af7f5a40633ea24cadcd4
ms.sourcegitcommit: 09a124d851fbbab7bc0b14efd6ef4e0275c7ee88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2020
ms.locfileid: "82083374"
---
# <a name="find-windows-vm-images-in-the-azure-marketplace-with-azure-powershell"></a>Azure PowerShell ile Azure Market'te Windows VM görüntülerini bulma

Bu makalede, Azure Marketi'nde VM görüntülerini bulmak için Azure PowerShell'in nasıl kullanılacağı açıklanmaktadır. Daha sonra PowerShell, Resource Manager şablonları veya diğer araçlarla programlı bir VM oluşturduğunuzda bir Market görüntüsü belirtebilirsiniz.

Azure [Marketi](https://azuremarketplace.microsoft.com/) mağazası, [Azure portalı](https://portal.azure.com)veya [Azure CLI'yi](../linux/cli-ps-findimage.md)kullanarak kullanılabilir resimlere ve tekliflere de göz atabilirsiniz. 

 

[!INCLUDE [virtual-machines-common-image-terms](../../../includes/virtual-machines-common-image-terms.md)]

## <a name="table-of-commonly-used-windows-images"></a>Sık kullanılan Windows görüntülerinin tablosu

Bu tablo, belirtilen Yayıncılar ve Teklifler için kullanılabilir Skus bir alt kümesini gösterir.

| Yayımcı | Sunduğu | Sku |
|:--- |:--- |:--- |
| MicrosoftWindowsServer |WindowsServer |2019-Datacenter |
| MicrosoftWindowsServer |WindowsServer |2019-Datacenter-Çekirdek |
| MicrosoftWindowsServer |WindowsServer |2019-Datacenter-With-Containers |
| MicrosoftWindowsServer |WindowsServer |2016-Datacenter |
| MicrosoftWindowsServer |WindowsServer |2016-Datacenter-Server-Core |
| MicrosoftWindowsServer |WindowsServer |2016-Datacenter-With-Containers |
| MicrosoftWindowsServer |WindowsServer |2012-R2-Datacenter |
| MicrosoftWindowsServer |WindowsServer |2012-Datacenter |
| MicrosoftSharePoint |MicrosoftSharePointServer |sp2019 |
| MicrosoftSQLServer |SQL2019-WS2016 |Enterprise |
| MicrosoftRServer |RServer-WS2016 |Enterprise |

## <a name="navigate-the-images"></a>Resimlerde gezinme

Bir yerde bir görüntü bulmak için bir yolu [get-AzVMImagePublisher](https://docs.microsoft.com/powershell/module/az.compute/get-azvmimagepublisher)çalıştırmak için , [Get-AzVMImageOffer](https://docs.microsoft.com/powershell/module/az.compute/get-azvmimageoffer), ve [Sırayla Get-AzVMImageSku](https://docs.microsoft.com/powershell/module/az.compute/get-azvmimagesku) cmdlets:

1. Görüntü yayımcılarını listeleyin.
2. Belirli bir yayımcı varsa yayımcının tekliflerini listeleyin.
3. Belirli bir teklif varsa SKU’larını listeleyin.

Ardından, seçili bir SKU için, dağıtılacak sürümleri listelemek için [Get-AzVMImage'i](https://docs.microsoft.com/powershell/module/az.compute/get-azvmimage) çalıştırın.

1. Yayıncıları listele:

    ```powershell
    $locName="<Azure location, such as West US>"
    Get-AzVMImagePublisher -Location $locName | Select PublisherName
    ```

2. Seçtiğiniz yayıncı adını doldurun ve teklifleri listele:

    ```powershell
    $pubName="<publisher>"
    Get-AzVMImageOffer -Location $locName -PublisherName $pubName | Select Offer
    ```

3. Seçtiğiniz teklif adını doldurun ve SNU'ları listele:

    ```powershell
    $offerName="<offer>"
    Get-AzVMImageSku -Location $locName -PublisherName $pubName -Offer $offerName | Select Skus
    ```

4. Seçtiğiniz SKU adını doldurun ve resim sürümünü alın:

    ```powershell
    $skuName="<SKU>"
    Get-AzVMImage -Location $locName -PublisherName $pubName -Offer $offerName -Sku $skuName | Select Version
    ```
    
`Get-AzVMImage` Komutun çıktısından, yeni bir sanal makine dağıtmak için bir sürüm görüntüsü seçebilirsiniz.

Aşağıdaki örnek, komutların tam sırasını ve çıktılarını gösterir:

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

*MicrosoftWindowsServer* yayımcısı için:

```powershell
$pubName="MicrosoftWindowsServer"
Get-AzVMImageOffer -Location $locName -PublisherName $pubName | Select Offer
```

Çıktı:

```
Offer
-----
Windows-HUB
WindowsServer
WindowsServerSemiAnnual
```

*WindowsServer* teklifi için:

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

Daha sonra, *2019-Datacenter* SKU için:

```powershell
$skuName="2019-Datacenter"
Get-AzVMImage -Location $locName -PublisherName $pubName -Offer $offerName -Sku $skuName | Select Version
```

Artık seçili yayımcı, teklif, SKU ve sürümü bir URN (:) ayrılmış değerler) olarak birleştirebilirsiniz. [Yeni-AzVM](https://docs.microsoft.com/powershell/module/az.compute/new-azvm) `--image` cmdlet ile bir VM oluştururken bu URN'yi parametre ile geçirin. İsteğe bağlı olarak URN'deki sürüm numarasını görüntünün en son sürümünü almak için "en son" ile değiştirebilirsiniz.

Kaynak Yöneticisi şablonu yla bir VM dağıtırsanız, görüntü parametrelerini `imageReference` özelliklerde tek tek ayarlarsınız. Bkz. [şablon başvurusu](/azure/templates/microsoft.compute/virtualmachines).

[!INCLUDE [virtual-machines-common-marketplace-plan](../../../includes/virtual-machines-common-marketplace-plan.md)]

### <a name="view-plan-properties"></a>Plan özelliklerini görüntüleme

Bir resmin satın alma planı bilgilerini `Get-AzVMImage` görüntülemek için cmdlet'i çalıştırın. Çıktıdaki `PurchasePlan` özellik değilse, `null`görüntünün programlı dağıtımdan önce kabul etmesi gereken terimler vardır.  

Örneğin, *Windows Server 2016 Datacenter* görüntüsünün ek koşulları `PurchasePlan` yoktur, `null`bu nedenle bilgiler:

```powershell
$version = "2016.127.20170406"
Get-AzVMImage -Location $locName -PublisherName $pubName -Offer $offerName -Skus $skuName -Version $version
```

Çıktı:

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

Aşağıdaki örnek, Veri Bilimi *Sanal Makine - Windows 2016* görüntüsü için `PurchasePlan` aşağıdaki `name` `product`özelliklere `publisher`sahip benzer bir komut gösterir: , ve . Bazı görüntülerde de `promotion code` bir özellik vardır. Bu resmi dağıtmak için, koşulları kabul etmek ve programatik dağıtımı etkinleştirmek için aşağıdaki bölümlere bakın.

```powershell
Get-AzVMImage -Location "westus" -PublisherName "microsoft-ads" -Offer "windows-data-science-vm" -Skus "windows2016" -Version "0.2.02"
```

Çıktı:

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

Lisans koşullarını görüntülemek için [Get-AzMarketplaceterms](https://docs.microsoft.com/powershell/module/az.marketplaceordering/get-azmarketplaceterms) cmdlet'i kullanın ve satın alma planı parametrelerini geçirin. Çıktı, Market resminin koşullarına bir bağlantı sağlar ve şartları daha önce kabul edip etmediğinizi gösterir. Parametre değerlerindeki tüm küçük harfleri kullandığınızdan emin olun.

```powershell
Get-AzMarketplaceterms -Publisher "microsoft-ads" -Product "windows-data-science-vm" -Name "windows2016"
```

Çıktı:

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

Şartları kabul etmek veya reddetmek için [Set-AzMarketplaceterms](https://docs.microsoft.com/powershell/module/az.marketplaceordering/set-azmarketplaceterms) cmdlet'i kullanın. Görüntü için abonelik başına yalnızca bir kez şartları kabul etmeniz gerekir. Parametre değerlerindeki tüm küçük harfleri kullandığınızdan emin olun. 

```powershell
$agreementTerms=Get-AzMarketplaceterms -Publisher "microsoft-ads" -Product "windows-data-science-vm" -Name "windows2016"

Set-AzMarketplaceTerms -Publisher "microsoft-ads" -Product "windows-data-science-vm" -Name "windows2016" -Terms $agreementTerms -Accept
```

Çıktı:

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

### <a name="deploy-using-purchase-plan-parameters"></a>Satınalma planı parametrelerini kullanarak dağıtma

Görüntü koşullarını kabul ettikten sonra, bu abonelikte bir VM dağıtabilirsiniz. Aşağıdaki snippet'te gösterildiği gibi, VM nesnesi için Pazar Planı bilgilerini ayarlamak için [Set-AzVMPlan](https://docs.microsoft.com/powershell/module/az.compute/set-azvmplan) cmdlet'i kullanın. VM için ağ ayarları oluşturmak ve dağıtımı tamamlamak için tam bir komut dosyası için [PowerShell komut dosyası örneklerine](powershell-samples.md)bakın.

```powershell
...

$vmConfig = New-AzVMConfig -VMName "myVM" -VMSize Standard_D1

# Set the Marketplace plan information

$publisherName = "microsoft-ads"

$productName = "windows-data-science-vm"

$planName = "windows2016"

$vmConfig = Set-AzVMPlan -VM $vmConfig -Publisher $publisherName -Product $productName -Name $planName

$cred=Get-Credential

$vmConfig = Set-AzVMOperatingSystem -Windows -VM $vmConfig -ComputerName "myVM" -Credential $cred

# Set the Marketplace image

$offerName = "windows-data-science-vm"

$skuName = "windows2016"

$version = "19.01.14"

$vmConfig = Set-AzVMSourceImage -VM $vmConfig -PublisherName $publisherName -Offer $offerName -Skus $skuName -Version $version
...
```
Daha sonra vm yapılandırmasını ağ yapılandırma nesneleri ile `New-AzVM` birlikte cmdlet'e geçirirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Temel görüntü bilgilerini kullanarak `New-AzVM` cmdlet ile hızlı bir şekilde sanal bir makine oluşturmak için [powershell ile windows sanal makine oluştur'a](quick-create-powershell.md)bakın.


[Tam olarak yapılandırılmış bir sanal makine oluşturmak](../scripts/virtual-machines-windows-powershell-sample-create-vm.md)için powershell komut dosyası örneğine bakın.


