---
title: Azure Data Box Heavy geri göndermeye yönelik öğretici | Microsoft Docs
description: Azure Data Box Heavy Microsoft 'a nasıl teslim alabileceğinizi öğrenin
services: databox
author: alkohli
ms.service: databox
ms.subservice: heavy
ms.topic: tutorial
ms.date: 09/03/2019
ms.author: alkohli
ms.localizationpriority: high
ms.openlocfilehash: e438fb38afb649f6f4c7f595059ef64800977242
ms.sourcegitcommit: 6794fb51b58d2a7eb6475c9456d55eb1267f8d40
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70240339"
---
::: zone target = "docs"

# <a name="tutorial-return-azure-data-box-heavy-and-verify-data-upload-to-azure"></a>Öğretici: Azure Data Box Heavy döndürün ve verileri Azure 'a yükleyin

::: zone-end

::: zone target = "chromeless"

# <a name="return-azure-data-box-heavy-and-verify-data-upload-to-azure"></a>Azure Data Box Heavy döndürün ve verileri Azure 'a yükleyin

::: zone-end

::: zone target = "docs"

Bu öğreticide, Azure Data Box Heavy nasıl geri dönebileceğinizi ve Azure 'a yüklenen verilerin nasıl doğrulanalınacağını açıklanmaktadır.

Bu öğreticide şu gibi konular hakkında bilgi edineceksiniz:

> [!div class="checklist"]
> * Önkoşullar
> * Göndermeye Hazırlama
> * Data Box Heavy Microsoft 'a gönder
> * Azure'a verilerin yüklendiğini doğrulama
> * Data Box Heavy verilerin erasiyi

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce şunları yaptığınızdan emin olun:

- [Öğreticiyi tamamladınız: Azure Data Box verileri kopyalayın ve doğrulayın](data-box-heavy-deploy-copy-data.md).
- Kopyalama işleri tamamlanmıştır. Kopyalama işleri devam ediyorsa Göndermeye Hazırlama çalıştırılamaz.


## <a name="prepare-to-ship"></a>Göndermeye Hazırlama

[!INCLUDE [data-box-heavy-prepare-to-ship](../../includes/data-box-heavy-prepare-to-ship.md)]

::: zone-end

::: zone target = "chromeless"

## <a name="prepare-to-ship"></a>Göndermeye Hazırlama

Göndermeye hazırlanmadan önce, kopyalama işlerinin tamam olduğundan emin olun.

1. Yerel Web Kullanıcı arabirimindeki Göndermeye Hazırlama sayfasına gidin ve sevk hazırlığı ' ni başlatın.
2. Cihazı yerel web kullanıcı arabiriminden kapatın. Kabloları cihazdan çıkarın.

Artık cihazınızı geri göndermeye hazırsınız.

::: zone-end

## <a name="ship-data-box-heavy-back"></a>Data Box Heavy geri gönder

1. Cihazın kapatıldığından ve tüm kabloların kaldırıldığından emin olun. Biriktirme ve 4 güç gücünü cihazın arkasında erişebileceğiniz tepsiye güvenli bir şekilde yerleştirin.
2. Cihaz, ABD 'de FedEx aracılığıyla LTL navlun ve AB 'de DHL 'yi sevk eder

    1. Toplama ile ilgili bilgi almak ve iade sevkiyat etiketini almak için [Data Box işlemlerine](mailto:DataBoxOps@microsoft.com) ulaşın.
    2. Çekme planını zamanlamak için sevkiyat taşıyıcınızın yerel numarasını çağırın.
    3. Sevkiyat etiketinin, sevkıyatın dış tarafında göze çarpacak şekilde görüntülendiğinden emin olun.
    4. Önceki sevkiyatta yer alan eski sevkiyat etiketlerinin cihazdan kaldırıldığından emin olun.
3. Data Box Heavy, taşıyıcınızın üzerinden çekildikten ve tarandıktan sonra Portal 'daki sipariş durumu, **çekildi**olarak güncelleştirilir. Ayrıca bir takip numarası da görüntülenir.

::: zone target = "docs"

## <a name="verify-data-upload-to-azure"></a>Azure'a verilerin yüklendiğini doğrulama

Microsoft cihazı alıp taradığında, sipariş durumu **Alındı** olarak güncelleştirilir. Ardından cihaz hasar veya kurcalama belirtileri için fiziksel doğrulama sürecinden geçer.

Doğrulama tamamlandıktan sonra, Data Box Heavy Azure veri merkezindeki ağa bağlanır. Veri kopyalaması otomatik olarak başlar. Verilerin boyutuna bağlı olarak, kopyalama işleminin tamamlanması birkaç saatten bir güne kadar sürebilir. Kopyalama işinin ilerleme durumunu portalda izleyebilirsiniz.

Kopyalama tamamlandıktan sonra, sipariş durumu **Tamamlandı** olarak güncelleştirilir.

Kaynaktan silmeden önce verilerinizin Azure 'a yüklendiğini doğrulayın. Verileriniz şu durumda olabilir:

- Azure depolama hesabınız. Data Box'a veri kopyaladığınızda, türlerine bağlı olarak bu veriler Azure Depolama hesabınızda aşağıdaki yollardan birine yüklenir.

  - Blok blobları ve sayfa blobları için: `https://<storage_account_name>.blob.core.windows.net/<containername>/files/a.txt`
  - Azure Dosyaları için: `https://<storage_account_name>.file.core.windows.net/<sharename>/files/a.txt`

    Alternatif olarak Azure portalda Azure depolama hesabınıza gidip oradan ilerleyebilirsiniz.

- Yönetilen disk kaynak grubunuz. Yönetilen diskler oluştururken, VHD 'ler sayfa Blobları olarak yüklenir ve ardından yönetilen disklere dönüştürülür. Yönetilen diskler, sipariş oluşturma sırasında belirtilen kaynak gruplarına iliştirilir. 

    - Azure 'da yönetilen disklere kopyalama işlemi başarılı olduysa, Azure portal **sipariş ayrıntılarına** gidebilir ve yönetilen diskler için belirtilen kaynak gruplarını bir yere göz önünde yapabilirsiniz.

        ![Yönetilen disk kaynak gruplarını tanımla](media/data-box-deploy-copy-data-from-vhds/order-details-managed-disk-resource-groups.png)

        Belirtilen kaynak grubuna gidin ve yönetilen disklerinizi bulun.

        ![Kaynak gruplarına bağlı yönetilen disk](media/data-box-deploy-copy-data-from-vhds/managed-disks-resource-group.png)

    - Bir VHDX veya dinamik/fark kayıt VHD 'SI kopyaladıysanız, VHDX/VHD, bir Sayfa Blobu olarak hazırlama depolama hesabına yüklenir, ancak VHD 'nin yönetilen diske dönüştürülmesi başarısız olur. **Blob > hazırlama depolama hesabınıza** gidin ve ardından uygun kapsayıcıyı seçin-Standart SSD, Standart HDD veya Premium SSD. VHD 'ler, hazırlama depolama hesabınızda sayfa Blobları olarak karşıya yüklenir.
    
::: zone-end

::: zone target = "chromeless"

## <a name="verify-data-upload-to-azure"></a>Azure'a verilerin yüklendiğini doğrulama

Data Box Heavy cihaz Azure veri merkezi ağına bağlıyken, Azure 'a yüklenen veriler otomatik olarak başlatılır. Data Box hizmeti, veri kopyasının Azure portal aracılığıyla tamamlandığını bildirir.

- Hata günlüklerini kontrol ederek hata olup olmadığını kontrol edin ve gerekli eylemleri gerçekleştirin.
- Kaynaktan silmeden önce verilerinizin depolama hesaplarında olduğundan emin olun.

::: zone-end

## <a name="erasure-of-data-from-data-box-heavy"></a>Data Box Heavy verilerin erasiyi
 
Veriler Azure'a yüklendikten sonra Data Box disklerindeki veriyi [NIST SP 800-88 Revision 1 yönergelerine](https://csrc.nist.gov/News/2014/Released-SP-800-88-Revision-1,-Guidelines-for-Medi) uygun şekilde siler. Silinme tamamlandıktan sonra [Sipariş geçmişini indirebilirsiniz](data-box-portal-admin.md#download-order-history).

::: zone target = "docs"

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide aşağıdaki Azure Data Box konularını öğrendiniz:

> [!div class="checklist"]
> * Önkoşullar
> * Göndermeye Hazırlama
> * Data Box Heavy Microsoft 'a gönder
> * Azure'a verilerin yüklendiğini doğrulama
> * Data Box Heavy verilerin erasiyi

Data Box Heavy yerel Web Kullanıcı arabirimi aracılığıyla yönetmeyi öğrenmek için aşağıdaki makaleye ilerleyin.

> [!div class="nextstepaction"]
> [Azure Data Box'ı yönetmek için yerel web arabirimini kullanma](./data-box-local-web-ui-admin.md)

::: zone-end


