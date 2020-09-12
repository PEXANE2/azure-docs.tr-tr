---
title: Windows Önyükleme Yöneticisi hatası-0xC0000428 durum geçersiz görüntü karması
titlesuffix: Azure Virtual Machines
description: Bu makalede, bir önizleme görüntüsünün kullanıldığı ve deneme süresi sona erdiği ve bir Azure sanal makinesinin (VM) önyüklenmesini önleyen sorunları gidermeye yönelik adımlar sağlanmaktadır.
services: virtual-machines-windows, azure-resource-manager
documentationcenter: ''
author: v-miegge
manager: dcscontentpm
editor: ''
tags: azure-resource-manager
ms.assetid: 26c411f9-6c22-4f45-a445-c5781e547828
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.topic: troubleshooting
ms.date: 09/03/2020
ms.author: v-miegge
ms.openlocfilehash: f607ebb64b27c45ec696d7fcd431a0ba2342697f
ms.sourcegitcommit: bf1340bb706cf31bb002128e272b8322f37d53dd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/03/2020
ms.locfileid: "89447995"
---
# <a name="windows-boot-manager-error---0xc0000428-status-invalid-image-hash"></a>Windows Önyükleme Yöneticisi hatası-0xC0000428 durum geçersiz görüntü karması

Bu makalede, bir önizleme görüntüsünün kullanıldığı ve deneme süresi sona erdiği ve bir Azure sanal makinesinin (VM) önyüklenmesini önleyen sorunları gidermeye yönelik adımlar sağlanmaktadır.

## <a name="symptom"></a>Belirti

VM 'nin ekran görüntüsünü görüntülemek için [önyükleme tanılamayı](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/boot-diagnostics) kullandığınızda, ekran görüntüsünde Windows Önyükleme Yöneticisi 'nin şu iletiyle birlikte görüntülediğini görürsünüz:

  `File: \windows\system32\boot\winload.exe`

  `Status: 0xc0000428`

  `Info: Windows cannot verify the digital signature for this file.`

  ![Şekil 1 ' Ox0000428 ' durumu ile Windows Önyükleme Yöneticisi 'Ni ve "Windows bu dosyanın dijital imzasını doğrulayamayacak" bilgisini görüntüler.](./media/windows-boot-error-invalid-image-hash/1-cannot-verify-signature.png)

veya ileti:

  `File: \Windows\system32\winload.exe`

  `Status: 0xc0000428`

  `Info: The digital signature for this file couldn't be verified.`
    
  ![Şekil 2 "Ox0000428" durumuna sahip Windows Önyükleme Yöneticisi 'Ni ve "Bu dosyanın dijital imzası doğrulanamadı" bilgisini görüntüler.](./media/windows-boot-error-invalid-image-hash/2-digital-signature-not-verified.png)

## <a name="cause"></a>Nedeni

VM 'yi oluşturmak için kullanılan görüntü, RTM (üretim sürümü) görüntüsü yerine sona erme tarihi olan bir önizleme görüntüsüdür. 

Önizleme görüntülerinin belirlenmiş bir yaşam döngüsü ve gördüğünüz ekran görüntüsü, son kullanma tarihinden sonra görüntünün denemesi üzerine geldiğinde görüntülenir.

### <a name="example-of-preview-images"></a>Önizleme görüntüleri örneği

`a699494373c04fc0bc8f2bb1389d6106__Windows-Server-Technical-Preview-201505.01-en.us-127GB.vhd`

Önizleme resminin sona erme tarihini genişletemezsiniz. Önizlemenin süresi dolduktan sonra, sanal makine artık önyüklenemeyecektir.

- Ürüne bağlı olarak, deneme süresi farklılık gösterebilir. Örneğin, Windows önizleme görüntülerinin 180 günlük deneme süresi vardır.

- Azure 'da, önizleme sürümü olan Windows için tüm görüntüler, kendilerine üretime yönelik değildir ve yalnızca belirli bir deneme süresi için veya "önizleme sürümü" olarak kullanılabilir.

## <a name="solution"></a>Çözüm

Görüntünüz bir önizleme görüntüsü ise, kullanılan görüntünün sona erme tarihini genişletmenin bir yolu yoktur, Önizleme olmayan bir görüntü kullanarak [Yeni BIR VM dağıtmanız](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal) gerekir. Aşağıdaki adımlar, bir önizleme görüntüsü kullandıysanız ve bu VM 'den yeni bir VM 'ye veri aktarırken size yardımcı olacak kaynaklar sağlamanıza yardımcı olur. Görüntüyü bir önizleme görüntüsü olarak pozitif olarak belirlediyseniz, artık bu süre dolduğunda görüntü kurtarılamaz.

Tercihinize bağlı olarak, bir önizleme görüntüsü olup olmadığını anlamak üzere görüntünüzü sorgulamak için Azure PowerShell ya da Azure CLı kullanabilirsiniz. Görüntünün bir önizleme görüntüsü olduğunu onaylamak için bu komutları kullanabilirsiniz.

### <a name="query-using-azure-powershell"></a>Azure PowerShell kullanarak sorgulama

1. Windows PowerShell uygulamasını açın.
1. Aşağıdaki komutları çalıştırın:

   ```powershell
   $locName = "<LOCATION>" 
   $pubName = "<PUBLISHER NAME>" 
   $offerName = "<OFFER NAME>" 
   $skuName= "<YEAR WHEN THIS IMAGE WAS RELEASED>"
   Get-AzVMImagePublisher -Location $locName | Select $pubName
   Get-AzVMImageOffer -Location $locName -PublisherName $pubName | Select Offer
   Get-AzVMImageSku -Location $locName -PublisherName $pubName -Offer $offerName | Select Skus
   Get-AzVMImage -Location $locName -PublisherName $pubName -Offer $offerName -Skus $skuName | Select Version
   ```

- Önceki komutlarda,,, `<LOCATION>` ve, `<PUBLISHER NAME>` `<OFFER NAME>` `<YEAR WHEN THIS IMAGE WAS RELEASED>` belirtilen bilgilerle değiştirin. Ayrıca "<" ve ">" sembollerini kaldırın.

  Aşağıdaki örneğe bakın:

  ```powershell
  $locName = "West US" 
  $pubName = "MicrosoftWindowsServer" 
  $offerName = "WindowsServer" 
  $skuName= "2016-Datacenter"
  Get-AzVMImagePublisher -Location $locName | Select $pubName
  Get-AzVMImageOffer -Location $locName -PublisherName $pubName | Select Offer
  Get-AzVMImageSku -Location $locName -PublisherName $pubName -Offer $offerName | Select Skus
  Get-AzVMImage -Location $locName -PublisherName $pubName -Offer $offerName -Skus $skuName | Select Version
  ```

### <a name="query-using-the-azure-cli"></a>Azure CLı kullanarak sorgulama

1. Henüz yapmadıysanız, [Azure CLI 'yı yüklemeniz](https://docs.microsoft.com/cli/azure/install-azure-cli)gerekecektir.
1. İndirildikten sonra komut Istemi veya PowerShell 'i kullanarak `az login` komutu girin ve ardından hesap kimlik bilgilerinizle oturum açın.
1. Oturum açtıktan sonra aşağıdaki komutları girin:

   ```powershell
   az vm image list-publishers --location "<LOCATION>"
   az vm image list-offers --location "West US" --publisher "<PUBLISHER NAME>"
   az vm image list-skus --location "West US" --publisher "<PUBLISHER NAME>" --offer "<OFFER NAME>"
   az vm image list  --location "West US" --publisher "<PUBLISHER NAME>" --offer "<OFFER NAME>" --sku "<YEAR WHEN THIS IMAGE WAS RELEASED>"
   ```

- Önceki komutlarda,,, `<LOCATION>` ve, `<PUBLISHER NAME>` `<OFFER NAME>` `<YEAR WHEN THIS IMAGE WAS RELEASED>` belirtilen bilgilerle değiştirin. Ayrıca "<" ve ">" sembollerini kaldırın.

  Aşağıdaki örneğe bakın:

  ```powershell
  az vm image list-publishers --location "West US"
  az vm image list-offers --location "West US" --publisher "MicrosoftWindowsServer"
  az vm image list-skus --location "West US" --publisher "MicrosoftWindowsServer" --offer "WindowsServer"
  az vm image list  --location "West US" --publisher "MicrosoftWindowsServer" --offer "WindowsServer" --sku "2016-Datacenter"
  ```
