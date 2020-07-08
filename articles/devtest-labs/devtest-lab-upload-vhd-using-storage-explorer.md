---
title: Depolama Gezgini kullanarak Azure DevTest Labs VHD dosyasını karşıya yükleme
description: Microsoft Azure Depolama Gezgini kullanarak VHD dosyasını laboratuvar depolama hesabına yükleyin
ms.topic: article
ms.date: 06/26/2020
ms.openlocfilehash: ccdf27488147bc4840d1e505ba2761bf328d6a32
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85480771"
---
# <a name="upload-vhd-file-to-labs-storage-account-using-microsoft-azure-storage-explorer"></a>Microsoft Azure Depolama Gezgini kullanarak VHD dosyasını laboratuvar depolama hesabına yükleyin

[!INCLUDE [devtest-lab-upload-vhd-selector](../../includes/devtest-lab-upload-vhd-selector.md)]

Azure DevTest Labs, VHD dosyaları, sanal makineleri sağlamak için kullanılan özel görüntüler oluşturmak için kullanılabilir. Bu makalede, bir laboratuvarın depolama hesabına VHD dosyası yüklemek için [Microsoft Azure Depolama Gezgini](../vs-azure-tools-storage-manage-with-storage-explorer.md) nasıl kullanılacağı gösterilmektedir. VHD dosyanızı karşıya yükledikten sonra, [sonraki adımlar bölümünde](#next-steps) KARŞıYA yüklenen VHD dosyasından nasıl özel bir görüntü oluşturulacağını gösteren bazı makaleler listelenmektedir. Azure 'daki diskler ve VHD 'ler hakkında daha fazla bilgi için bkz. [yönetilen disklere giriş](../virtual-machines/linux/managed-disks-overview.md)

## <a name="step-by-step-instructions"></a>Adım adım yönergeler

Aşağıdaki adımlar, [Microsoft Azure Depolama Gezgini](../vs-azure-tools-storage-manage-with-storage-explorer.md)kullanarak DevTest Labs 'e bir VHD dosyası yükleme işleminde size rehberlik sağlar.

1. [Microsoft Azure Depolama Gezgini en son sürümünü indirip yükleyin](https://www.storageexplorer.com).

1. Azure portal kullanarak laboratuvarın depolama hesabının adını alın:

    1. [Azure portalında](https://go.microsoft.com/fwlink/p/?LinkID=525040) oturum açın.
    
    1. **Tüm hizmetler**' i seçin ve ardından listeden **DevTest Labs** ' i seçin.
    
    1. Laboratuvarlar listesinden istediğiniz Laboratuvarı seçin.  
    
    1. Laboratuvarın dikey penceresinde **yapılandırma**' yı seçin. 
    
    1. Laboratuvar **yapılandırması** dikey penceresinde **özel görüntüler (VHD)** seçeneğini belirleyin.
    
    1. **Özel görüntüler** dikey penceresinde **+ Ekle**' yi seçin. 
    
    1. **Özel görüntü** dikey penceresinde, **VHD**' yi seçin.
    
    1. **VHD** dikey penceresinde **PowerShell kullanarak bir VHD 'yi karşıya yükle**' yi seçin.
    
        ![PowerShell kullanarak VHD 'YI karşıya yükleme][0]
    
    1. **PowerShell kullanarak görüntü yükleme** dikey penceresi, **Add-AzureVhd** cmdlet 'ine bir çağrı görüntüler. İlk parametre (*hedef*), laboratuvar için depolama hesabı adını aşağıdaki biçimde içerir:
    
        `https://<STORAGE-ACCOUNT-NAME>.blob.core.windows.net/uploads/...`

    1. Daha sonraki adımlarda kullanılan depolama hesabı adını unutmayın.
    
1. Depolama Gezgini kullanarak bir Azure abonelik hesabına bağlanın.

    > [!TIP] 
    > 
    > Depolama Gezgini çeşitli bağlantı seçeneklerini destekler. Bu bölümde, Azure aboneliğinizle ilişkili bir depolama hesabına bağlanma gösterilmektedir. Depolama Gezgini tarafından desteklenen diğer bağlantı seçeneklerini görmek için, [Depolama Gezgini kullanmaya](../vs-azure-tools-storage-manage-with-storage-explorer.md)Başlarken makalesine başvurun.
 
    1. Depolama Gezgini'ni açın.
    
    1. Depolama Gezgini ' de **Azure hesap ayarları**' nı seçin. 
    
        ![Azure hesap ayarları][1]
    
    1. Sol bölmede, oturum açtığınız Microsoft hesapları görüntülenir. Başka bir hesaba bağlanmak için **Hesap ekle**’yi seçin ve en az bir etkin Azure aboneliği ile ilişkilendirilen bir Microsoft hesabı ile oturum açmak için iletişim kutularını izleyin.
    
        ![Hesap ekleme][2]
    
    1. Bir Microsoft hesabı ile başarıyla oturum açtıktan sonra sol bölme ilgili hesapla ilişkili Azure abonelikleriyle doldurulur. Birlikte çalışmak istediğiniz Azure aboneliklerini seçin ve ardından **Uygula**’yı seçin. ( **Tüm abonelikler** seçildiğinde, listelenen Azure aboneliklerinin tümünün veya hiçbirinin seçimi geçer.)
    
        ![Azure aboneliklerini seçme][3]
    
    1. Sol bölmede seçili Azure abonelikleriyle ilişkili depolama hesapları gösterilir.
    
        ![Seçili Azure abonelikleri][4]

1. Laboratuvarın depolama hesabını bulun:

    1. Depolama Gezgini sol bölmede, laboratuvarın sahibi olan Azure aboneliğinin düğümünü bulun ve genişletin.
    
    1. Aboneliğin düğümü altında, **depolama hesapları**' nı genişletin.

    1. **BLOB kapsayıcıları**, **dosya paylaşımları**, **Kuyruklar**ve **Tablolar**için düğümleri göstermek üzere laboratuvarın depolama hesabı düğümünü genişletin.
    
    1. **BLOB kapsayıcıları** düğümünü genişletin.
    
    1. Sağ bölmedeki içeriğini göstermek için blob kapsayıcısını karşıya yükleme ' yi seçin.
        
        ![Dizini karşıya yükle][5]

1. Depolama Gezgini kullanarak VHD dosyasını karşıya yükleyin:

    1. Depolama Gezgini sağ bölmede, laboratuvarın depolama **hesabının blob kapsayıcısındaki blob 'ların** bir listesini görmeniz gerekir. Blob Düzenleyicisi araç çubuğunda **karşıya yükle** ' yi seçin. 
        
        ![Karşıya yükle düğmesi][6]
    
    1. **Karşıya yükle** açılan menüsünde **dosyaları karşıya yükle...** öğesini seçin.
    
    1. **Dosyaları karşıya yükle** iletişim kutusunda üç nokta simgesini seçin.
        
        ![Dosya Seç][8]  

    1. **Karşıya yüklenecek dosyaları seçin** iletişim kutusunda, istenen VHD dosyasına gidin, seçin ve sonra **Aç**' ı seçin.
    
    1. **Dosyaları karşıya yükle** iletişim kutusuna döndürülen **BLOB türünü** **Sayfa Blobu**olarak değiştirin.
    
    1. **Karşıya Yükle**'yi seçin.

        ![Dosya Seç][9]  
    
    1. Depolama Gezgini **etkinlik günlüğü** bölmesi, indirme durumunu gösterir (karşıya yüklemeyi iptal etmek için bağlantılarla birlikte). Bir VHD dosyasını karşıya yükleme işlemi, VHD dosyasının boyutuna ve bağlantı hızına bağlı olarak uzun olabilir. 

        ![Karşıya yükleme dosyası durumu][10]  

## <a name="next-steps"></a>Sonraki adımlar

- [Azure portal kullanarak bir VHD dosyasından Azure DevTest Labs özel görüntü oluşturma](devtest-lab-create-template.md)
- [PowerShell kullanarak bir VHD dosyasından Azure DevTest Labs özel görüntü oluşturma](devtest-lab-create-custom-image-from-vhd-using-powershell.md)

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
