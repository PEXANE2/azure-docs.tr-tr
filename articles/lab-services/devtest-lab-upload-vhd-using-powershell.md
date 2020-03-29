---
title: PowerShell kullanarak Azure DevTest Labs'a VHD dosyasını yükleyin | Microsoft Dokümanlar
description: Bu makale, PowerShell kullanarak Azure DevTest Labs'a vhd dosyasını nasıl yükleydiğinizi gösteren bir izlenebilir.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/24/2020
ms.author: spelluru
ms.openlocfilehash: a9e5a69cd7840bbaff081a4d47cb60f62a76eda6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76759764"
---
# <a name="upload-vhd-file-to-labs-storage-account-using-powershell"></a>PowerShell'i kullanarak VHD dosyasını laboratuvarın depolama hesabına yükleyin

[!INCLUDE [devtest-lab-upload-vhd-selector](../../includes/devtest-lab-upload-vhd-selector.md)]

Azure DevTest Labs'da VHD dosyaları, sanal makineleri sağlamak için kullanılan özel görüntüler oluşturmak için kullanılabilir. Aşağıdaki adımlar, bir laboratuvarın depolama hesabına bir VHD dosyası yüklemek için PowerShell'i kullanmanızı sağlar. VHD dosyanızı yükledikten sonra, [Sonraki adımlar bölümünde](#next-steps) yüklenen VHD dosyasından nasıl özel bir resim oluşturulabileceğinizi gösteren bazı makaleler listelenir. Azure'daki diskler ve VHD'ler hakkında daha fazla bilgi için [yönetilen disklere Giriş'e](../virtual-machines/linux/managed-disks-overview.md) bakın

## <a name="step-by-step-instructions"></a>Adım adım yönergeler

Aşağıdaki adımlar, PowerShell'i kullanarak Azure DevTest Labs'a bir VHD dosyası yüklemenize yol açar. 

1. [Azure portalında](https://go.microsoft.com/fwlink/p/?LinkID=525040)oturum açın.

1. **Tüm hizmetleri**seçin ve ardından listeden **DevTest Labs'ı** seçin.

1. Laboratuvarlar listesinden istediğiniz laboratuarı seçin.  

1. Laboratuvarın bıçağında **Yapılandırma'yı**seçin. 

1. Laboratuvar **Yapılandırma** bıçağında **Özel görüntüler (VHD'ler) seçeneğini belirleyin.**

1. Özel **görüntüler** bıçağında **+Ekle'yi**seçin. 

1. Özel **görüntü** bıçağında **VHD'yi**seçin.

1. **VHD** bıçak üzerinde, **PowerShell kullanarak bir VHD yükleyin**seçin.

    ![PowerShell kullanarak VHD yükleyin](./media/devtest-lab-upload-vhd-using-powershell/upload-image-using-psh.png)

1. **PowerShell bıçağını kullanarak bir görüntü** yükle'de, oluşturulan PowerShell komut dosyasını bir metin düzenleyicisine kopyalayın.

1. Yüklemek istediğiniz VHD dosyasının konumunu işaret etmek için **Ekle-AzureVhd** cmdlet'in **LocalFilePath** parametresini değiştirin.

1. PowerShell komut isteminde **Ekle-AzureVhd** cmdlet'i (değiştirilmiş **LocalFilePath** parametresi ile) çalıştırın.

> [!WARNING] 
> 
> VHD dosyasını yükleme işlemi, VHD dosyasının boyutuna ve bağlantı hızınıza bağlı olarak uzun olabilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure portalını kullanarak bir VHD dosyasından Azure DevTest Labs'da özel bir resim oluşturma](devtest-lab-create-template.md)
- [PowerShell'i kullanarak bir VHD dosyasından Azure DevTest Labs'da özel bir görüntü oluşturun](devtest-lab-create-custom-image-from-vhd-using-powershell.md)
