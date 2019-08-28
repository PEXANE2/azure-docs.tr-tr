---
title: Azure Data Box geri göndermeye yönelik öğretici | Microsoft Docs
description: Azure Data Box’ınızı Microsoft'a nasıl geri göndereceğinizi öğrenin
services: databox
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: tutorial
ms.date: 8/27/2019
ms.author: alkohli
ms.openlocfilehash: 368439d6e15d6c94bbb96d67fcb48ab006234c95
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70098850"
---
::: zone target="docs"

# <a name="tutorial-return-azure-data-box-and-verify-data-upload-to-azure"></a>Öğretici: Azure Data Box döndürün ve verileri Azure 'a yükleyin

::: zone-end

::: zone target="chromeless"

# <a name="return-data-box-and-verify-data-upload-to-azure"></a>Data Box döndürün ve verileri Azure 'a yükleyin

::: zone-end

::: zone target="docs"

Bu öğretici, Azure Data Box’ın nasıl iade edileceğini ve yüklenen verileri nasıl doğrulayabileceğinizi anlatır.

Bu öğreticide şu gibi konular hakkında bilgi edineceksiniz:

> [!div class="checklist"]
> * Önkoşullar
> * Göndermeye Hazırlama
> * Data Box'ı Microsoft'a gönderme
> * Azure'a verilerin yüklendiğini doğrulama
> * Data Box'tan verileri silme

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce şunları yaptığınızdan emin olun:

- [Öğreticiyi tamamladınız: Azure Data Box verileri kopyalayın ve doğrulayın](data-box-deploy-copy-data.md). 
- Kopyalama işleri tamamlanmıştır. Kopyalama işleri devam ediyorsa Göndermeye Hazırlama çalıştırılamaz.

## <a name="prepare-to-ship"></a>Göndermeye Hazırlama

[!INCLUDE [data-box-prepare-to-ship](../../includes/data-box-prepare-to-ship.md)]

::: zone-end

::: zone target="chromeless"

Veri kopyalama işlemi tamamlandıktan sonra, cihazı hazırlayın ve sevk edersiniz. Cihaz Azure veri merkezine ulaştığında, veriler Azure 'a otomatik olarak yüklenir.

## <a name="prepare-to-ship"></a>Göndermeye Hazırlama

Göndermeye hazırlanmadan önce, kopyalama işlerinin tamam olduğundan emin olun.

1. Yerel web kullanıcı arabirimindeki **Göndermeye hazırla** sayfasına gidin ve gönderme hazırlığına başlayın. 
2. Cihazı yerel web kullanıcı arabiriminden kapatın. Kabloları cihazdan çıkarın. 

Sonraki adımlar, cihazı döndürmekte olduğunuz konuma göre belirlenir.

::: zone-end

::: zone target="docs"

## <a name="ship-data-box-back"></a>Data Box'ı geri gönderme

Cihaza veri kopyalamanın tamamlandığından ve **göndermeye hazırlama** çalıştırmasının başarılı olduğundan emin olun. Cihazı sevk ettiğiniz bölgeye göre yordam farklıdır.

::: zone-end

## <a name="ship-in-us-canada-europe"></a>ABD, Kanada, Avrupa 'da gönder

Cihazı ABD, Kanada veya Avrupa 'ya döndürürken aşağıdaki adımları uygulayın.

1. Cihazın açık olduğundan ve kabloların kaldırıldığından emin olun. 
2. Cihaz ile beraber sağlanan güç kablosunu sararak emniyetli şekilde cihazın arkasına yerleştirin.
3. E-ink ekranda gönderi etiketinin görüntülendiğinden emin olun ve taşıyıcınızdan bir teslim alma randevusu alın. Etiket hasar görmüşse veya kaybolursa ya da E-mürekkep ekranda görüntülenmiyorsa, Microsoft Desteği başvurun. Destek ' i öneriyorsa, genel bakış ' a giderek Azure portal **> sevkiyat etiketini yükleyebilirsiniz** . Sevkiyat etiketini ve afdüzeltmesini cihaza indirin. 
4. Cihaz döndürülürken UPS ile bir toplama zamanlayın. Bir toplama zamanlamak için:

    - Yerel UPS (ülkeye/bölgeye özel ücretsiz numara) çağırın.
    - Çağrınızda, E-mürekkep görüntüsünde veya yazdırılmış etiketinizde gösterildiği gibi ters Sevkiyat izleme numarasını tırnak içine edin.
    - İzleme numarası tırnak işareti yoksa, UPS toplama sırasında ek ücret ödemeniz gerekir.

    Toplama zamanlamak yerine, en yakın bırakma konumundaki Data Box de bırakabilirsiniz.
4. Data Box nakliyeciniz tarafından toplandıktan ve tarandıktan sonra, portaldaki sipariş durumu **Toplandı** olarak güncelleştirilir. Ayrıca bir takip numarası da görüntülenir.


## <a name="ship-in-australia"></a>Avustralya 'da gönder

Avustralya 'daki Azure veri merkezlerinde ek bir güvenlik bildirimi vardır. Tüm gelen sevkiyatlar gelişmiş bir bildirime sahip olmalıdır. Avustralya 'da göndermek için aşağıdaki adımları uygulayın.


1. Cihazı iade sevk irsaliyesiyle göndermek için kullanılan orijinal kutuyu koruyun.
2. Cihaza veri kopyalama işleminin tamamlanıp **göndermeye hazırlama çalıştırmasının** başarılı olduğundan emin olun.
3. Cihazın gücünü kapatın ve kabloları kaldırın.
4. Kuyruğa alındı ve cihazın arka planda sağlanmış olan güç kablosunu güvenli bir şekilde yerleştirin.
5. Toplama istemek için e-posta Quantium çözümleri. Azure portal belirtilen hizmet başvuru numarasına başvurun. Şu e-posta şablonunu kullanın:- *Tau kodlu ters Sevkiyat Etiketi isteği*. E-postaya aşağıdaki ayrıntıları eklediğinizden emin olun: 

    ```
    To: Azure@quantiumsolutions.com
    Subject: Pickup request for Azure｜Reference number：XXX XXX XXX
    Body: 
    - Company name：
    - Address:
    - Contact name:
    - Contact number:
    - Requested pickup date: mm/dd
    ```
6. Quantium çözümleri Avustralya, size bir iade sevkiyat etiketi e-postası verecektir.
7. Dönüş etiketini Yazdır ve teslim kutusunda bunu afet.
8. Paketini Courier 'e elle paketleyin.

Gerekirse, Azure@quantiumsolutions.com veya telefonda e-posta Quantium çözüm desteğini gönderebilirsiniz.

Telefon aracılığıyla siparişiniz hakkında sorgu için:

- Önce çekme için bir e-posta gönderin.
- Telefon üzerinden sipariş adınızı girin.

## <a name="ship-in-japan"></a>Japonya 'da gönder 

1. Cihazı iade sevk irsaliyesiyle göndermek için kullanılan orijinal kutuyu koruyun.
2. Cihazın gücünü kapatın ve kabloları kaldırın.
3. Kuyruğa alındı ve cihazın arka planda sağlanmış olan güç kablosunu güvenli bir şekilde yerleştirin.
4. Şirket adı ve adres bilgilerinizi, gönderici bilgileriniz olarak konsinye notuna yazın.
5. Aşağıdaki e-posta şablonunu kullanarak e-posta Quantium çözümü.

    - Japon Chakubarai konsinye veya eksik olduğunda, bu e-postada bu göz önünde olduğunu unutmayın. Quantium çözümleri Japonya, toplama sırasında konsinye dekontunu getirmek için Japon gönderi ister.
    - Birden çok siparişiniz varsa, bireysel toplama sağlamak için e-posta gönderin.

    ```
    To: Customerservice.JP@quantiumsolutions.com
    Subject: Pickup request for Azure Data Box｜Job name： 
    Body: 
    - Japan Post Yu-Pack tracking number (reference number)：
    - Requested pickup date：mmdd (Select a requested time slot from below).
    a. 08：00-13：00 
    b. 13：00-15：00 
    c. 15：00-17：00 
    d. 17：00-19：00 
    ```

3. Bir toplama zamanladıktan sonra, Quantium çözümlerinden bir e-posta onayı alın. E-posta onayı, Chakubarai konsinye notuna ilişkin bilgileri de içerir.

Gerekirse, aşağıdaki bilgileri izleyerek Quantium çözüm desteği (Japonca dil) ile iletişim kurabilmeniz gerekir: 

- E-posta:Customerservice.JP@quantiumsolutions.com 
- Telefon: 03-5755-0150 

::: zone target="docs"

## <a name="verify-data-upload-to-azure"></a>Azure'a verilerin yüklendiğini doğrulama

Microsoft cihazı alıp taradığında, sipariş durumu **Alındı** olarak güncelleştirilir. Ardından cihaz hasar veya kurcalama belirtileri için fiziksel doğrulama sürecinden geçer.

Doğrulama tamamlandıktan sonra Data Box, Azure veri merkezindeki ağa bağlanır. Veri kopyalaması otomatik olarak başlar. Verilerin boyutuna bağlı olarak, kopyalama işleminin tamamlanması birkaç saatten bir güne kadar sürebilir. Kopyalama işinin ilerleme durumunu portalda izleyebilirsiniz.

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

::: zone target="chromeless"

## <a name="verify-data-upload-to-azure"></a>Azure'a verilerin yüklendiğini doğrulama

[!INCLUDE [data-box-verify-upload](../../includes/data-box-verify-upload.md)]

::: zone-end

## <a name="erasure-of-data-from-data-box"></a>Data Box'tan verileri silme
 
Veriler Azure'a yüklendikten sonra Data Box disklerindeki veriyi [NIST SP 800-88 Revision 1 yönergelerine](https://csrc.nist.gov/News/2014/Released-SP-800-88-Revision-1,-Guidelines-for-Medi) uygun şekilde siler.

::: zone target="docs"

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide aşağıdaki Azure Data Box konularını öğrendiniz:

> [!div class="checklist"]
> * Önkoşullar
> * Göndermeye Hazırlama
> * Data Box'ı Microsoft'a gönderme
> * Azure'a verilerin yüklendiğini doğrulama
> * Data Box'tan verileri silme

Data Box’ı yerel web arabirimini kullanarak yönetmeyi öğrenmek için şu makaleye ilerleyin.

> [!div class="nextstepaction"]
> [Azure Data Box'ı yönetmek için yerel web arabirimini kullanma](./data-box-local-web-ui-admin.md)

::: zone-end


