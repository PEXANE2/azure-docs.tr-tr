---
title: PowerShell kullanarak VHD dosyasını Azure DevTest Labs karşıya yükleme | Microsoft Docs
description: Bu makalede, PowerShell kullanarak Azure DevTest Labs VHD dosyasını karşıya yükleme işleminin nasıl yapılacağı gösterilmektedir.
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
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "76759764"
---
# <a name="upload-vhd-file-to-labs-storage-account-using-powershell"></a>PowerShell kullanarak VHD dosyasını laboratuvar depolama hesabına yükleyin

[!INCLUDE [devtest-lab-upload-vhd-selector](../../includes/devtest-lab-upload-vhd-selector.md)]

Azure DevTest Labs, VHD dosyaları, sanal makineleri sağlamak için kullanılan özel görüntüler oluşturmak için kullanılabilir. Aşağıdaki adımlarda, PowerShell kullanarak bir laboratuvarın depolama hesabına VHD dosyası yükleme işleminde izlenecek yol gösterilmektedir. VHD dosyanızı karşıya yükledikten sonra, [sonraki adımlar bölümünde](#next-steps) KARŞıYA yüklenen VHD dosyasından nasıl özel bir görüntü oluşturulacağını gösteren bazı makaleler listelenmektedir. Azure 'daki diskler ve VHD 'ler hakkında daha fazla bilgi için bkz. [yönetilen disklere giriş](../virtual-machines/linux/managed-disks-overview.md)

## <a name="step-by-step-instructions"></a>Adım adım yönergeler

Aşağıdaki adımlarda, PowerShell kullanarak Azure DevTest Labs bir VHD dosyasını karşıya yükleme işlemi adım adım gösterilmektedir. 

1. [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040) oturum açın.

1. **Tüm hizmetler**' i seçin ve ardından listeden **DevTest Labs** ' i seçin.

1. Laboratuvarlar listesinden istediğiniz Laboratuvarı seçin.  

1. Laboratuvarın dikey penceresinde **yapılandırma**' yı seçin. 

1. Laboratuvar **yapılandırması** dikey penceresinde **özel görüntüler (VHD)** seçeneğini belirleyin.

1. **Özel görüntüler** dikey penceresinde **+ Ekle**' yi seçin. 

1. **Özel görüntü** dikey penceresinde, **VHD**' yi seçin.

1. **VHD** dikey penceresinde **PowerShell kullanarak bir VHD 'yi karşıya yükle**' yi seçin.

    ![PowerShell kullanarak VHD 'YI karşıya yükleme](./media/devtest-lab-upload-vhd-using-powershell/upload-image-using-psh.png)

1. **PowerShell kullanarak bir görüntüyü karşıya yükle** dikey penceresinde, oluşturulan PowerShell betiğini bir metin düzenleyicisine kopyalayın.

1. **Add-AzureVhd** cmdlet 'Inin **LocalFilePath** PARAMETRESINI, yüklemek istediğiniz VHD dosyasının konumunu işaret etmek üzere değiştirin.

1. Bir PowerShell isteminde **Add-AzureVhd** cmdlet 'ini (Modified **LocalFilePath** parametresiyle birlikte) çalıştırın.

> [!WARNING] 
> 
> Bir VHD dosyasını karşıya yükleme işlemi, VHD dosyasının boyutuna ve bağlantı hızına bağlı olarak uzun olabilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure portal kullanarak bir VHD dosyasından Azure DevTest Labs özel görüntü oluşturma](devtest-lab-create-template.md)
- [PowerShell kullanarak bir VHD dosyasından Azure DevTest Labs özel görüntü oluşturma](devtest-lab-create-custom-image-from-vhd-using-powershell.md)
