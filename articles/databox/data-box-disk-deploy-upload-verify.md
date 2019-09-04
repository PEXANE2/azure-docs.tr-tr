---
title: Azure Data Box Disk depolama hesabına veri yüklemeyi doğrulama öğreticisi | Microsoft Docs
description: Azure Data Box Disk Azure depolama hesabına yüklenen verileri nasıl doğrulayacağınızı öğrenmek için bu öğreticiyi kullanın.
services: databox
author: alkohli
ms.service: databox
ms.subservice: disk
ms.topic: tutorial
ms.localizationpriority: high
ms.date: 09/03/2019
ms.author: alkohli
Customer intent: As an IT admin, I need to be able to order Data Box Disk to upload on-premises data from my server onto Azure.
ms.openlocfilehash: a4d814ab5b1f26a6a2b871a850fd5e3153e256f5
ms.sourcegitcommit: 6794fb51b58d2a7eb6475c9456d55eb1267f8d40
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70240285"
---
::: zone target="docs"

# <a name="tutorial-verify-data-upload-from-azure-data-box-disk"></a>Öğretici: Azure Data Box Disk verileri karşıya yüklemeyi doğrula

Bu, serinin son öğreticisidir: Azure Data Box Disk dağıtın. Bu öğreticide şunları öğreneceksiniz:

> [!div class="checklist"]
> * Azure'a verilerin yüklendiğini doğrulama
> * Data Box Disk'ten verileri silinme

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce [öğreticiyi tamamladığınızdan emin olun: Azure Data Box Disk](data-box-disk-deploy-picked-up.md)döndürün.


## <a name="verify-data-upload-to-azure"></a>Azure'a verilerin yüklendiğini doğrulama

Diskler nakliyeciniz tarafından toplandıktan sonra, portaldaki sipariş durumu **Toplandı** olarak güncelleştirilir. Ayrıca bir takip numarası da görüntülenir.

![Diskler toplandı](media/data-box-disk-deploy-picked-up/data-box-portal-pickedup.png)

Microsoft diski alıp taradığında, iş durumu **Alındı** olarak güncelleştirilir. 

![Diskler alındı](media/data-box-disk-deploy-picked-up/data-box-portal-received.png)

Diskler Azure veri merkezindeki bir sunucuya bağlandıktan sonra veriler otomatik olarak kopyalanır. Verilerin boyutuna bağlı olarak, kopyalama işleminin tamamlanması birkaç saatten bir güne kadar sürebilir. Kopyalama işinin ilerleme durumunu portalda izleyebilirsiniz.

Kopyalama tamamlandıktan sonra, sipariş durumu **Tamamlandı** olarak güncelleştirilir.

![Veri kopyalama tamamlandı](media/data-box-disk-deploy-picked-up/data-box-portal-completed.png)

Kopyalama hatalarla tamamlanırsa, bkz. [karşıya yükleme hatalarını giderme](data-box-disk-troubleshoot-upload.md).

Kaynaktan silmeden önce verilerinizin depolama hesaplarında olduğundan emin olun. Verileriniz şu durumda olabilir:

- Azure depolama hesabınız. Data Box'a veri kopyaladığınızda, türlerine bağlı olarak bu veriler Azure Depolama hesabınızda aşağıdaki yollardan birine yüklenir.

  - Blok blobları ve sayfa blobları için: `https://<storage_account_name>.blob.core.windows.net/<containername>/files/a.txt`
  - Azure Dosyaları için: `https://<storage_account_name>.file.core.windows.net/<sharename>/files/a.txt`

    Alternatif olarak Azure portalda Azure depolama hesabınıza gidip oradan ilerleyebilirsiniz.

- Yönetilen disk kaynak grubunuz. Yönetilen diskler oluştururken, VHD 'ler sayfa Blobları olarak yüklenir ve ardından yönetilen disklere dönüştürülür. Yönetilen diskler, sipariş oluşturma sırasında belirtilen kaynak gruplarına iliştirilir.

  - Azure 'da yönetilen disklere kopyalama işlemi başarılı olduysa, Azure portal **sipariş ayrıntılarına** gidebilir ve yönetilen diskler için belirtilen kaynak grubunu bir yere göz önünde yapabilirsiniz.

      ![Sipariş ayrıntılarını görüntüle](media/data-box-disk-deploy-picked-up/order-details-resource-group.png)

    Belirtilen kaynak grubuna gidin ve yönetilen disklerinizi bulun.

      ![Yönetilen diskler için kaynak grubu](media/data-box-disk-deploy-picked-up/resource-group-attached-managed-disk.png)

  - Bir VHDX veya dinamik/fark kayıt VHD 'SI kopyaladıysanız, VHDX/VHD, bir Blok Blobu olarak hazırlama depolama hesabına yüklenir. **Blob > hazırlama depolama hesabınıza** gidin ve ardından uygun kapsayıcıyı seçin-Standardssd, standardhdd veya PremiumSSD. VHDX/VHD 'ler, hazırlama depolama hesabınızda blok Blobları olarak gösterilmelidir.
  
::: zone-end

::: zone target="chromeless"

# <a name="verify-data-upload-to-azure"></a>Azure'a verilerin yüklendiğini doğrulama

Veriler Azure 'a yüklendikten sonra, kaynaktan silmeden önce verilerinizin depolama hesaplarına ait olduğundan emin olun. Verileriniz şu durumda olabilir:

- Azure depolama hesabınız. Data Box'a veri kopyaladığınızda, türlerine bağlı olarak bu veriler Azure Depolama hesabınızda aşağıdaki yollardan birine yüklenir.

    - **Blok Blobları ve sayfa Blobları için**: https://< storage_account_name >. blob. Core. Windows.<containername>NET//Files/a.txt

    - **Azure dosyaları için**: https://< storage_account_name >. File. Core. Windows. net/<sharename>/Files/a.txt

- Yönetilen disk kaynak grubunuz. Yönetilen diskler oluştururken, VHD 'ler sayfa Blobları olarak yüklenir ve ardından yönetilen disklere dönüştürülür. Yönetilen diskler, sipariş oluşturma sırasında belirtilen kaynak gruplarına iliştirilir.

::: zone-end

Verilerin Azure 'a yüklendiğini doğrulamak için aşağıdaki adımları uygulayın:

1. Disk siparişinizle ilişkilendirilmiş depolama hesabına gidin.
2. **Blob hizmeti > Bloblara göz atın** seçeneğine gidin. Kapsayıcı listesi gösterilir. *BlockBlob* ve *PageBlob* klasörlerinin altında oluşturduğunuz alt klasöre karşılık olarak, depolama hesabınızda aynı adlı kapsayıcılar oluşturulur.
    Klasör adları Azure adlandırma kurallarına uymuyorsa, Azure'a veri yükleme işlemi başarısız olur.

3. Veri kümesinin tamamının yüklendiğini doğrulamak için, Microsoft Azure Depolama Gezgini'ni kullanın. Data Box Disk sırasına karşılık gelen depolama hesabını ekleyin ve ardından blob kapsayıcıları listesine bakın. Bir kapsayıcı seçin, **…Diğer** düğmesine tıklayın ve sonra da **Klasör istatistikleri**'ne tıklayın. **Etkinlikler** bölmesinde, blob sayısı ve toplam blob boyutu gibi o klasöre özgü istatistikler görüntülenir. Bayt cinsinden toplam blob boyutu, veri kümesinin boyutuyla eşleşmelidir.

    ![Depolama Gezgini'nde klasör istatistikleri](media/data-box-disk-deploy-picked-up/folder-statistics-storage-explorer.png)

## <a name="erasure-of-data-from-data-box-disk"></a>Data Box Disk'ten verileri silinme

Kopyalama tamamlandıktan ve verilerin Azure depolama hesabında olduğunu doğruladıktan sonra, diskler NıST standardına göre güvenli bir şekilde silinir.

::: zone target="docs"

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide aşağıdaki Azure Data Box Disk konularını öğrendiniz:

> [!div class="checklist"]
> * Azure'a verilerin yüklendiğini doğrulama
> * Data Box Disk'ten verileri silinme


Azure portalı yoluyla Data Box Disk'i yönetmeyi öğrenmek için bir sonraki nasıl yapılır makalesine geçin.

> [!div class="nextstepaction"]
> [Azure portalını kullanarak Azure Data Box Disk'i yönetme](./data-box-portal-ui-admin.md)

::: zone-end




