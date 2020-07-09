---
title: Microsoft Azure Data Box'ı geri göndermeye yönelik öğretici | Microsoft Docs
description: Azure Data Box’ınızı Microsoft'a nasıl geri göndereceğinizi öğrenin
services: databox
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: tutorial
ms.date: 09/20/2019
ms.author: alkohli
ms.localizationpriority: high
ms.openlocfilehash: efcc5e8f629c8b6ccc903478473b54989c0e9fd6
ms.sourcegitcommit: 845a55e6c391c79d2c1585ac1625ea7dc953ea89
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2020
ms.locfileid: "85959486"
---
::: zone target="docs"

# <a name="tutorial-return-azure-data-box-and-verify-data-upload-to-azure"></a>Öğretici: Azure Data Box'ı iade etme ve Azure'a verilerin yüklendiğini doğrulama

::: zone-end

::: zone target="chromeless"

## <a name="return-data-box-and-verify-data-upload-to-azure"></a>Data Box'ı iade etme ve Azure'a verilerin yüklendiğini doğrulama

::: zone-end

::: zone target="docs"

Bu öğretici, Azure Data Box’ın nasıl iade edileceğini ve yüklenen verileri nasıl doğrulayabileceğinizi anlatır.

Bu öğreticide şu gibi konular hakkında bilgi edineceksiniz:

> [!div class="checklist"]
> * Ön koşullar
> * Göndermeye hazırlama
> * Data Box'ı Microsoft'a gönderme
> * Azure'a verilerin yüklendiğini doğrulama
> * Data Box'tan verileri silme

## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce şunlardan emin olun:

- [Öğretici: Verileri Azure Data Box'a kopyalama ve doğrulama](data-box-deploy-copy-data.md) bölümünü tamamladınız. 
- Kopyalama işleri tamamlanıp **Bağlan ve kopyala** sayfasında hata kalmadıysa. Kopyalama işleri devam ediyorsa veya **Bağlan ve kopyala** sayfasında hatalar varsa göndermeye hazırlama adımı çalışmaz.

## <a name="prepare-to-ship"></a>Göndermeye hazırlama

[!INCLUDE [data-box-prepare-to-ship](../../includes/data-box-prepare-to-ship.md)]

::: zone-end

::: zone target="chromeless"

Veri kopyalama işlemi tamamlandıktan sonra cihazı hazırlayıp gönderin. Cihaz Azure veri merkezine ulaştığında veriler Azure'a otomatik olarak yüklenir.

## <a name="prepare-to-ship"></a>Göndermeye hazırlama

Göndermeye hazırlama işleminden önce kopyalama işlerinin tamamlandığından emin olun.

1. Yerel web kullanıcı arabirimindeki **Göndermeye hazırla** sayfasına gidin ve gönderme hazırlığına başlayın. 
2. Cihazı yerel web kullanıcı arabiriminden kapatın. Kabloları cihazdan çıkarın. 

Sonraki adımlar cihazın iade edildiği bölgeye göre belirlenir.

::: zone-end

::: zone target="docs"

## <a name="ship-data-box-back"></a>Data Box'ı geri gönderme

Cihaza veri kopyalamanın tamamlandığından ve **göndermeye hazırlama** işleminin başarılı olduğundan emin olun. Cihazı gönderdiğiniz bölgeye bağlı olarak yordam farklıdır.

::: zone-end

## <a name="in-us-canada-europe"></a>[ABD, Kanada, Avrupa](#tab/in-us-canada-europe)

Cihazı ABD, Kanada veya Avrupa’da iade ediyorsanız aşağıdaki adımları uygulayın.

1. Cihazın kapalı olduğundan ve kabloların çıkartılmış olduğundan emin olun. 
2. Cihaz ile beraber sağlanan güç kablosunu sararak emniyetli şekilde cihazın arkasına yerleştirin.
3. E-ink ekranda gönderi etiketinin görüntülendiğinden emin olun ve taşıyıcınızdan bir teslim alma randevusu alın. Etiket hasar görür veya kaybolursa ya da E-ink ekranında gösterilmezse Microsoft Desteği’ne başvurun. Destek öneride bulunursa Azure portalındaki **Genel Bakış > Gönderim etiketini indir** bölümüne gidebilirsiniz. Gönderim etiketini indirip cihaza yapıştırın. 
4. Cihazı iade ediyorsanız UPS ile bir toplama zamanı ayarlayın. Teslim alma planlamak için:

    - Yerel UPS şubesini (ülkeye/bölgeye özel ücretsiz hatlar) arayın.
    - Görüşme esnasında, E-ink ekranında veya basılı etiketinizde gördüğünüz iade izleme numarasını bildirin.
    - İzleme numarası bildirilmezse, teslim alma esnasında UPS’ye ek ücret ödemeniz gerekir.

    Teslim alma zamanlama yerine Data Box’ı en yakın teslim etme yerine bırakabilirsiniz.
4. Data Box nakliyeciniz tarafından toplandıktan ve tarandıktan sonra, portaldaki sipariş durumu **Toplandı** olarak güncelleştirilir. Ayrıca bir takip numarası da görüntülenir.

::: zone target="chromeless"

## <a name="verify-data-upload-to-azure"></a>Azure'a verilerin yüklendiğini doğrulama

[!INCLUDE [data-box-verify-upload](../../includes/data-box-verify-upload.md)]

## <a name="erasure-of-data-from-data-box"></a>Data Box'tan verileri silme
 
Veriler Azure'a yüklendikten sonra Data Box disklerindeki veriyi [NIST SP 800-88 Revision 1 yönergelerine](https://csrc.nist.gov/News/2014/Released-SP-800-88-Revision-1,-Guidelines-for-Medi) uygun şekilde siler.

::: zone-end

::: zone target="docs"

[!INCLUDE [data-box-verify-upload-return](../../includes/data-box-verify-upload-return.md)]



::: zone-end


## <a name="in-australia"></a>[Avustralya](#tab/in-australia)

Avustralya’daki Azure veri merkezlerinde ek güvenlik bildirimi uygulanır. Gelen tüm sevkiyatların önceden bildirilmesi gerekir. Avustralya’da gönderme için aşağıdaki adımları uygulayın.


1. İade gönderimi için cihazı gönderirken kullanılan orijinal kutuyu saklayın.
2. Cihaza veri kopyalamanın tamamlandığından ve **göndermeye hazırlama** işleminin başarılı olduğundan emin olun.
3. Cihazı kapatın ve kabloları çıkarın.
4. Cihaz ile beraber sağlanan güç kablosunu sararak emniyetli şekilde cihazın arkasına yerleştirin.
5. [DHL Bağlantısından](https://mydhl.express.dhl/au/en/schedule-pickup.html#/schedule-pickup#label-reference) çevrimiçi olarak bir teslim alma rezervasyonu yapın.

::: zone target="chromeless"

## <a name="verify-data-upload-to-azure"></a>Azure'a verilerin yüklendiğini doğrulama

[!INCLUDE [data-box-verify-upload](../../includes/data-box-verify-upload.md)]

## <a name="erasure-of-data-from-data-box"></a>Data Box'tan verileri silme
 
Veriler Azure'a yüklendikten sonra Data Box disklerindeki veriyi [NIST SP 800-88 Revision 1 yönergelerine](https://csrc.nist.gov/News/2014/Released-SP-800-88-Revision-1,-Guidelines-for-Medi) uygun şekilde siler.

::: zone-end

::: zone target="docs"

[!INCLUDE [data-box-verify-upload-return](../../includes/data-box-verify-upload-return.md)]

::: zone-end

## <a name="in-japan"></a>[Japonya](#tab/in-japan) 

1. İade gönderimi için cihazı gönderirken kullanılan orijinal kutuyu saklayın.
2. Cihazı kapatın ve kabloları çıkarın.
3. Cihaz ile beraber sağlanan güç kablosunu sararak emniyetli şekilde cihazın arkasına yerleştirin.
4. Sevk mektubuna, gönderici bilgisi olarak şirket adınızı ve adres bilgilerinizi yazın.
5. Aşağıdaki e-posta şablonunu kullanarak Quantium Solutions’a e-posta gönderin.

    - Japan Post Chakubarai sevk mektubu gönderilmediyse veya kaybolduysa, gönderdiğiniz e-postada bunu da bildirin. Quantium Solutions Japan, teslim alma esnasında Japan Post’un sevk mektubunu da getirmesini ister.
    - Birden fazla siparişiniz varsa her birinin ayrı olarak teslim alınması için e-posta gönderin.

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

3. Teslim alma rezervasyonu yaptıktan sonra Quantium Solutions’tan e-posta onay iletisi alırsınız. E-posta onay iletisi, Chakubarai sevk mektubuna yönelik bilgileri de içerir.

Gerekirse, aşağıdaki bilgilerden Quantium Solutions Desteği (Japonca) ile iletişime geçebilirsiniz: 

- E-posta: Customerservice.JP@quantiumsolutions.com 
- Telefon：03-5755-0150 

::: zone target="chromeless"

## <a name="verify-data-upload-to-azure"></a>Azure'a verilerin yüklendiğini doğrulama

[!INCLUDE [data-box-verify-upload](../../includes/data-box-verify-upload.md)]

## <a name="erasure-of-data-from-data-box"></a>Data Box'tan verileri silme
 
Veriler Azure'a yüklendikten sonra Data Box disklerindeki veriyi [NIST SP 800-88 Revision 1 yönergelerine](https://csrc.nist.gov/News/2014/Released-SP-800-88-Revision-1,-Guidelines-for-Medi) uygun şekilde siler.

::: zone-end

::: zone target="docs"

[!INCLUDE [data-box-verify-upload-return](../../includes/data-box-verify-upload-return.md)]

::: zone-end

## <a name="in-singapore"></a>[Singapur’da](#tab/in-singapore) 

1. İade gönderimi için cihazı gönderirken kullanılan orijinal kutuyu saklayın.
2. İzleme numarasını (Data Box yerel web kullanıcı arabiriminin Göndermeye Hazırlama sayfasında başvuru numarası olarak gösterilir) not alın. Bu numara, göndermeye hazırlama adımı başarıyla tamamlandıktan sonra oluşturulur. Bu sayfadaki gönderim etiketini indirin ve kutunun üzerine yapıştırın. 
3. Cihazı kapatın ve kabloları çıkarın.
4. Cihaz ile beraber sağlanan güç kablosunu sararak emniyetli şekilde cihazın arkasına yerleştirin. 
5. Aşağıdaki e-posta şablonuna takip numarasını ekleyin ve SingPost Müşteri Hizmetleri'ne gönderin.

    ```
    To: kadcustcare@singpost.com
    Subject: Microsoft Azure Pick-up - OrderName 
    Body: 
        1. Requestor name  
        2. Requestor contact number
        3. Requestor collection address
        4. Preferred collection date
    ```

   > [!NOTE]
   > İş gününde alınan rezervasyon istekleri için:
   > - 15:00’ten önce, teslim alma sonraki iş gününde 09:00 ile 13:00 arasında gerçekleştirilir.
   > - 15:00’ten sonra, teslim alma sonraki iş gününde 14:00 ile 18:00 arasında gerçekleştirilir.  

::: zone target="chromeless"

## <a name="verify-data-upload-to-azure"></a>Azure'a verilerin yüklendiğini doğrulama

[!INCLUDE [data-box-verify-upload](../../includes/data-box-verify-upload.md)]

## <a name="erasure-of-data-from-data-box"></a>Data Box'tan verileri silme
 
Veriler Azure'a yüklendikten sonra Data Box disklerindeki veriyi [NIST SP 800-88 Revision 1 yönergelerine](https://csrc.nist.gov/News/2014/Released-SP-800-88-Revision-1,-Guidelines-for-Medi) uygun şekilde siler.

::: zone-end

::: zone target="docs"

[!INCLUDE [data-box-verify-upload-return](../../includes/data-box-verify-upload-return.md)]

::: zone-end


<!--## [In Korea](#tab/in-korea) 

1. Retain the original box used to ship the device for return shipment.
2. Note down the tracking number (shown as reference number on the Prepare to Ship page of the Data Box local web UI). This is available after the prepare to ship step successfully completes. Download the shipping label from this page and paste on the packing box. 
3. Power off the device and remove the cables.
4. Spool and securely place the power cord that was provided with the device in the back of the device. 

Request pickup  
If consignment note is present:  

1. Call Quantium Solutions International hotline at 070-8231-1418 during office hours (10 AM to 5 PM, Monday to Friday). Quote Microsoft Azure pickup and the service request number to arrange for a collection.
2. If the hotline is busy, email microsoft@rocketparcel.com, with the email subject Microsoft Azure Pickup and the service request number as reference.  
3. If the courier does not arrive for collection, call Quantium Solutions International hotline for alternate arrangements.  
4. You will receive an email confirmation for the pickup schedule.  

Exception process
If the consignment note is not present:
1. Call Quantium Solutions International hotline at 070-8231-1418 during office hours (10 AM to 5 PM, Monday to Friday). Quote Microsoft Azure pickup and the service request number. Specify that you need a new consignment note to arrange for a collection. Provide sender (customer), receiver information (Azure datacenter), and reference number (service request number).
2. If the hotline is busy, email microsoft@rocketparcel.com, with the email subject Microsoft Azure Pickup and the service request number as reference.
3. If the courier does not arrive for collection, call Quantium Solutions International hotline for alternate arrangements.
4. You get a verbal confirmation if request is made via telephone.  
::: zone target="chromeless"

## Verify data upload to Azure

[!INCLUDE [data-box-verify-upload](../../includes/data-box-verify-upload.md)]

## Erasure of data from Data Box
 
Once the upload to Azure is complete, the Data Box erases the data on its disks as per the [NIST SP 800-88 Revision 1 guidelines](https://csrc.nist.gov/News/2014/Released-SP-800-88-Revision-1,-Guidelines-for-Medi).

::: zone-end

::: zone target="docs"

[!INCLUDE [data-box-verify-upload-return](../../includes/data-box-verify-upload-return.md)]

::: zone-end
-->

## <a name="self-managed"></a>[Kullanıcı Yönetimli](#tab/in-selfmanaged) 

Japonya, Singapur, Kore ve Batı Avrupa’da Data Box kullanıyorsanız ve sipariş oluşturma sırasında kullanıcı yönetimli gönderim seçeneğini belirlediyseniz bu yönergeleri izleyin. 

1. Bu adım başarıyla tamamlandıktan sonra Data Box yerel web kullanıcı arabiriminin Göndermeye Hazırlama sayfasında gösterilen Yetkilendirme kodunu not alın.
2. Cihazı kapatın ve kabloları çıkarın. Cihaz ile beraber sağlanan güç kablosunu sararak emniyetli şekilde cihazın arkasına yerleştirin.
3. Cihazı iade etmeye hazırsanız aşağıdaki şablonu kullanarak Azure Data Box Operasyon ekibine bir e-posta gönderin.
    
    ```
    To: adbops@microsoft.com 
    Subject: Request for Azure Data Box drop-off for order: ‘orderName’ 
    Body: 
        1. Order name  
        2. Authorization code available after Prepare to Ship has completed [Yes/No]  
        3. Contact name of the person dropping off. You will need to display a Government approved ID during the drop off.
    ```

::: zone target="chromeless"

## <a name="verify-data-upload-to-azure"></a>Azure'a verilerin yüklendiğini doğrulama

[!INCLUDE [data-box-verify-upload](../../includes/data-box-verify-upload.md)]

## <a name="erasure-of-data-from-data-box"></a>Data Box'tan verileri silme
 
Veriler Azure'a yüklendikten sonra Data Box disklerindeki veriyi [NIST SP 800-88 Revision 1 yönergelerine](https://csrc.nist.gov/News/2014/Released-SP-800-88-Revision-1,-Guidelines-for-Medi) uygun şekilde siler.

::: zone-end

::: zone target="docs"

[!INCLUDE [data-box-verify-upload-return](../../includes/data-box-verify-upload-return.md)]

::: zone-end

