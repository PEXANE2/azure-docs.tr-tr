---
title: AzCopy kullanarak Azure DevTest Labs'a VHD dosyasını yükleyin | Microsoft Dokümanlar
description: Bu makale, Bir VHD dosyasını Azure DevTest Labs'daki bir laboratuvarın depolama hesabına yüklemek için AzCopy komut satırı yardımcı programını kullanmak için bir izlenebilirlik sağlar.
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
ms.openlocfilehash: 49dc70788bf2a44b6925c5f3f8226fdadab8768c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76757431"
---
# <a name="upload-vhd-file-to-labs-storage-account-using-azcopy"></a>AzCopy kullanarak bilgisayarın depolama hesabına VHD dosyasını yükleme

[!INCLUDE [devtest-lab-upload-vhd-selector](../../includes/devtest-lab-upload-vhd-selector.md)]

Azure DevTest Labs'da VHD dosyaları, sanal makineleri sağlamak için kullanılan özel görüntüler oluşturmak için kullanılabilir. Bir Bilgisayarın depolama hesabına Bir VHD dosyası yüklemek için aşağıdaki adımlar AzCopy komut satırı yardımcı programını kullanmanıza yardımcı olur. VHD dosyanızı yükledikten sonra, [Sonraki adımlar bölümünde](#next-steps) yüklenen VHD dosyasından nasıl özel bir resim oluşturulabileceğinizi gösteren bazı makaleler listelenir. Azure'daki diskler ve VHD'ler hakkında daha fazla bilgi için [yönetilen disklere Giriş'e](../virtual-machines/linux/managed-disks-overview.md) bakın

> [!NOTE] 
>  
> AzCopy yalnızca Windows komut satırı yardımcı programıdır.

## <a name="step-by-step-instructions"></a>Adım adım yönergeler

Aşağıdaki adımlar, [AzCopy](https://aka.ms/downloadazcopy)kullanarak Azure DevTest Labs'a bir VHD dosyası yüklemenize yol açar. 

1. Azure portalını kullanarak laboratuvarın depolama hesabının adını alın:

1. [Azure portalında](https://go.microsoft.com/fwlink/p/?LinkID=525040)oturum açın.

1. **Tüm hizmetleri**seçin ve ardından listeden **DevTest Labs'ı** seçin.

1. Laboratuvarlar listesinden istediğiniz laboratuarı seçin.  

1. Laboratuvarın bıçağında **Yapılandırma'yı**seçin. 

1. Laboratuvar **Yapılandırma** bıçağında **Özel görüntüler (VHD'ler) seçeneğini belirleyin.**

1. Özel **görüntüler** bıçağında **+Ekle'yi**seçin. 

1. Özel **görüntü** bıçağında **VHD'yi**seçin.

1. **VHD** bıçak üzerinde, **PowerShell kullanarak bir VHD yükleyin**seçin.

    ![PowerShell kullanarak VHD yükleyin](./media/devtest-lab-upload-vhd-using-azcopy/upload-image-using-psh.png)

1. PowerShell blade **kullanarak görüntü yükle,** **Ekle-AzureVhd** cmdlet'e bir çağrı görüntüler. İlk parametre *(Hedef)* aşağıdaki biçimde bir blob kapsayıcı *(yükleme)* için URI içerir:

    ```
    https://<STORAGE-ACCOUNT-NAME>.blob.core.windows.net/uploads/...
    ``` 

1. Daha sonraki adımlarda kullanıldığı gibi tam URI not olun.

1. AzCopy kullanarak VHD dosyasını yükleyin:
 
1. [AzCopy'nin en son sürümünü indirin ve kurun.](https://aka.ms/downloadazcopy)

1. Komut penceresini açın ve AzCopy yükleme dizinine gidin. İsteğe bağlı olarak, AzCopy yükleme konumunu sistem yolunuza ekleyebilirsiniz. Varsayılan olarak, AzCopy aşağıdaki dizine yüklenir:

    ```command-line
    %ProgramFiles(x86)%\Microsoft SDKs\Azure\AzCopy
    ```

1. Depolama hesabı anahtarı nı ve blob konteyneri URI'yi kullanarak aşağıdaki komutu komut isteminde çalıştırın. *vhdFileName* değerinin tırnak içinde olması gerekir. VHD dosyasını yükleme işlemi, VHD dosyasının boyutuna ve bağlantı hızınıza bağlı olarak uzun olabilir.   

    ```command-line
    AzCopy /Source:<sourceDirectory> /Dest:<blobContainerUri> /DestKey:<storageAccountKey> /Pattern:"<vhdFileName>" /BlobType:page
    ```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure portalını kullanarak bir VHD dosyasından Azure DevTest Labs'da özel bir resim oluşturma](devtest-lab-create-template.md)
- [PowerShell'i kullanarak bir VHD dosyasından Azure DevTest Labs'da özel bir görüntü oluşturun](devtest-lab-create-custom-image-from-vhd-using-powershell.md)