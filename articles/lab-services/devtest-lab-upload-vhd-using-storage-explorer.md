---
title: Depolama Gezgini'ni kullanarak Azure DevTest Labs'a VHD dosyasını yükleyin
description: Microsoft Azure Depolama Gezgini'ni kullanarak VHD dosyasını laboratuvarın depolama hesabına yükleme
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
ms.date: 01/16/2020
ms.author: spelluru
ms.openlocfilehash: defafdd5809b7e537b3b9abb78f8cb63d0033c16
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76170365"
---
# <a name="upload-vhd-file-to-labs-storage-account-using-microsoft-azure-storage-explorer"></a>Microsoft Azure Depolama Gezgini'ni kullanarak VHD dosyasını laboratuvarın depolama hesabına yükleme

[!INCLUDE [devtest-lab-upload-vhd-selector](../../includes/devtest-lab-upload-vhd-selector.md)]

Azure DevTest Labs'da VHD dosyaları, sanal makineleri sağlamak için kullanılan özel görüntüler oluşturmak için kullanılabilir. Bu makalede, bir VHD dosyasını bir laboratuvarın depolama hesabına yüklemek için [Microsoft Azure Depolama Gezgini'nin](../vs-azure-tools-storage-manage-with-storage-explorer.md) nasıl kullanılacağı gösterilmiştir. VHD dosyanızı yükledikten sonra, [Sonraki adımlar bölümünde](#next-steps) yüklenen VHD dosyasından nasıl özel bir resim oluşturulabileceğinizi gösteren bazı makaleler listelenir. Azure'daki diskler ve VHD'ler hakkında daha fazla bilgi için [yönetilen disklere Giriş'e](../virtual-machines/linux/managed-disks-overview.md) bakın

## <a name="step-by-step-instructions"></a>Adım adım yönergeler

Microsoft [Azure Depolama Gezgini'ni](../vs-azure-tools-storage-manage-with-storage-explorer.md)kullanarak DevTest Labs'a bir VHD dosyası yükleme konusunda aşağıdaki adımlar sizi sağlar.

1. [Microsoft Azure Depolama Gezgini'nin en son sürümünü indirin ve yükleyin.](https://www.storageexplorer.com)

1. Azure portalını kullanarak laboratuvarın depolama hesabının adını alın:

    1. [Azure portalında](https://go.microsoft.com/fwlink/p/?LinkID=525040)oturum açın.
    
    1. **Tüm hizmetleri**seçin ve ardından listeden **DevTest Labs'ı** seçin.
    
    1. Laboratuvarlar listesinden istediğiniz laboratuarı seçin.  
    
    1. Laboratuvarın bıçağında **Yapılandırma'yı**seçin. 
    
    1. Laboratuvar **Yapılandırma** bıçağında **Özel görüntüler (VHD'ler) seçeneğini belirleyin.**
    
    1. Özel **görüntüler** bıçağında **+Ekle'yi**seçin. 
    
    1. Özel **görüntü** bıçağında **VHD'yi**seçin.
    
    1. **VHD** bıçak üzerinde, **PowerShell kullanarak bir VHD yükleyin**seçin.
    
        ![PowerShell kullanarak VHD yükleyin][0]
    
    1. PowerShell blade **kullanarak görüntü yükle,** **Ekle-AzureVhd** cmdlet'e bir çağrı görüntüler. İlk parametre *(Hedef)* aşağıdaki biçimde laboratuvar için depolama hesabı adını içerir:
    
        `https://<STORAGE-ACCOUNT-NAME>.blob.core.windows.net/uploads/...`

    1. Daha sonraki adımlarda kullanıldığı gibi depolama hesabı adını not alın.
    
1. Depolama Gezgini'ni kullanarak bir Azure abonelik hesabına bağlanın.

    > [!TIP] 
    > 
    > Depolama Gezgini çeşitli bağlantı seçeneklerini destekler. Bu bölümde, Azure aboneliğinizle ilişkili bir depolama hesabına bağlanma gösteriş gösteriş gösteriş. Depolama Gezgini tarafından desteklenen diğer bağlantı seçeneklerini görmek için, [Depolama Gezgini ile başlarken](../vs-azure-tools-storage-manage-with-storage-explorer.md)makaleye bakın.
 
    1. Depolama Gezgini'ni açın.
    
    1. Depolama Gezgini'nde **Azure Hesabı ayarlarını**seçin. 
    
        ![Azure hesap ayarları][1]
    
    1. Sol bölmede oturum açtığınız Microsoft hesapları görüntülenir. Başka bir hesaba bağlanmak için **Hesap ekle**’yi seçin ve en az bir etkin Azure aboneliği ile ilişkilendirilen bir Microsoft hesabı ile oturum açmak için iletişim kutularını izleyin.
    
        ![Hesap ekleme][2]
    
    1. Bir Microsoft hesabı ile başarıyla oturum açtıktan sonra sol bölme ilgili hesapla ilişkili Azure abonelikleriyle doldurulur. Birlikte çalışmak istediğiniz Azure aboneliklerini seçin ve ardından **Uygula**’yı seçin. (Tüm **abonelikleri** seçmek, listelenen Azure aboneliklerinin tamamının veya hiçbirinin seçimini değiştirmez.)
    
        ![Azure aboneliklerini seçme][3]
    
    1. Sol bölmede seçili Azure abonelikleriyle ilişkili depolama hesapları gösterilir.
    
        ![Seçili Azure abonelikleri][4]

1. Laboratuvarın depolama hesabını bulun:

    1. Depolama Gezgini'nin sol bölmesinde, laboratuvarın sahibi olan Azure aboneliğiiçin düğümü bulun ve genişletin.
    
    1. Aboneliğin düğümü altında, **Depolama Hesapları'nı**genişletin.

    1. **Blob Kapsayıcılar,** **Dosya Paylaşımları,** **Kuyruklar**ve **Tablolar**için düğümleri ortaya çıkarmak için laboratuvarın depolama hesap düğümlerini genişletin.
    
    1. **Blob Containers** düğümlerini genişletin.
    
    1. İçeriğini sağ bölmede görüntülemek için yüklemeblob kapsayıcısını seçin.
        
        ![Yükleme dizini][5]

1. Depolama Gezgini'ni kullanarak VHD dosyasını yükleyin:

    1. Depolama Gezgini sağ bölmesinde, laboratuvarın depolama hesabının **yükleme** blob kapsayıcısındaki lekelerin listesini görmeniz gerekir. Blob düzenleyiciaraç çubuğunda **Yükle'yi** seçin 
        
        ![Karşıya yükle düğmesi][6]
    
    1. **Upload** açılır menüsünden Dosyaları **Yükle'yi seçin...**.
    
    1. Dosyaları **Yükle** iletişim kutusunda elipsleri seçin.
        
        ![Dosyayı seçin][8]  

    1. İletişim **kutusu yüklemek için Dosyaları** Seç'te, istenen VHD dosyasına göz atın, seçin ve sonra **Aç'ı**seçin.
    
    1. **Dosyaları Yükle** iletişim kutusuna döndürüldüğünde, **Blob türünü** Sayfa **Blob**olarak değiştirin.
    
    1. **Yükle'yi**seçin.

        ![Dosyayı seçin][9]  
    
    1. Depolama Gezgini **Etkinlik Günlüğü** bölmesi indirme durumunu (yüklemeyi iptal etmek için bağlantılarla birlikte) gösterir. VHD dosyasını yükleme işlemi, VHD dosyasının boyutuna ve bağlantı hızınıza bağlı olarak uzun olabilir. 

        ![Yükleme-dosya durumu][10]  

## <a name="next-steps"></a>Sonraki adımlar

- [Azure portalını kullanarak bir VHD dosyasından Azure DevTest Labs'da özel bir resim oluşturma](devtest-lab-create-template.md)
- [PowerShell'i kullanarak bir VHD dosyasından Azure DevTest Labs'da özel bir görüntü oluşturun](devtest-lab-create-custom-image-from-vhd-using-powershell.md)

[0]: ./media/devtest-lab-upload-vhd-using-storage-explorer/upload-image-using-psh.png
[1]: ./media/devtest-lab-upload-vhd-using-storage-explorer/settings-icon.png
[2]: ./media/devtest-lab-upload-vhd-using-storage-explorer/add-account-link.png
[3]: ./media/devtest-lab-upload-vhd-using-storage-explorer/subscriptions-list.png
[4]: ./media/devtest-lab-upload-vhd-using-storage-explorer/storage-accounts-list.png
[5]: ./media/devtest-lab-upload-vhd-using-storage-explorer/upload-dir.png
[6]: ./media/devtest-lab-upload-vhd-using-storage-explorer/upload-button.png
[7]: ./media/devtest-lab-upload-vhd-using-storage-explorer/upload-files.png
[8]: ./media/devtest-lab-upload-vhd-using-storage-explorer/select-file.png
[9]: ./media/devtest-lab-upload-vhd-using-storage-explorer/upload-file.png
[10]: ./media/devtest-lab-upload-vhd-using-storage-explorer/upload-status.png
