---
title: Dışarı aktarma düzeninde Azure Data Box geri göndermeye yönelik öğretici | Microsoft Docs
description: Dışarı aktarma sırası tamamlandıktan sonra Azure Data Box Microsoft 'a nasıl teslim alabileceğinizi öğrenin
services: databox
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: tutorial
ms.date: 07/21/2020
ms.author: alkohli
ms.openlocfilehash: 04e4394e6a439c923558ef90e13c14c0adaa4020
ms.sourcegitcommit: a2a7746c858eec0f7e93b50a1758a6278504977e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/12/2020
ms.locfileid: "88142120"
---
# <a name="tutorial-return-azure-data-box-preview"></a>Öğretici: Azure Data Box Döndür (Önizleme)

Bu öğreticide, Azure Data Box nasıl geri dönebileceğinizi ve cihazın Azure verilerinde alındıktan sonra verilerin nasıl silinmeyeceği açıklanır.

Bu öğreticide şu gibi konular hakkında bilgi edineceksiniz:

> [!div class="checklist"]
>
> * Ön koşullar
> * Göndermeye hazırlama
> * Data Box'ı Microsoft'a gönderme
> * Data Box'tan verileri silme

[!INCLUDE [Data Box feature is in preview](../../includes/data-box-feature-is-preview-info.md)]

## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce şunlardan emin olun:

* [Öğreticiyi tamamladınız: Azure Data Box verileri kopyalama](data-box-deploy-export-copy-data.md).
* Kopyalama işleri tamamlandı. Kopyalama işleri devam ediyorsa Göndermeye hazırlama işi gerçekleştirilemez.

## <a name="prepare-to-ship"></a>Göndermeye hazırlama

[!INCLUDE [data-box-export-prepare-to-ship](../../includes/data-box-export-prepare-to-ship.md)]

Sonraki adımlar cihazın iade edildiği bölgeye göre belirlenir.

## <a name="ship-data-box-back"></a>Data Box'ı geri gönderme

Cihazdan veri kopyasının tamamlandığından ve **göndermeye hazırlama** çalıştırmasının başarılı olduğundan emin olun. Cihazı gönderdiğiniz bölgeye bağlı olarak yordam farklıdır.

## <a name="us-canada-europe"></a>[ABD, Kanada, Avrupa](#tab/in-us-canada-europe)

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

## <a name="australia"></a>[Avustralya](#tab/in-australia)

Avustralya’daki Azure veri merkezlerinde ek güvenlik bildirimi uygulanır. Gelen tüm sevkiyatların önceden bildirilmesi gerekir. Avustralya’da gönderme için aşağıdaki adımları uygulayın.

1. İade gönderimi için cihazı gönderirken kullanılan orijinal kutuyu saklayın.
2. Cihaza veri kopyalamanın tamamlandığından ve **göndermeye hazırlama** işleminin başarılı olduğundan emin olun.
3. Cihazı kapatın ve kabloları çıkarın.
4. Cihaz ile beraber sağlanan güç kablosunu sararak emniyetli şekilde cihazın arkasına yerleştirin.
5. [DHL bağlantısına](https://mydhl.express.dhl/au/en/schedule-pickup.html#/schedule-pickup#label-reference)çevrimiçi malzeme çekme kitabı.

## <a name="japan"></a>[Japonya](#tab/in-japan)

1. İade gönderimi için cihazı gönderirken kullanılan orijinal kutuyu saklayın.
2. Cihazı kapatın ve kabloları çıkarın.
3. Cihaz ile beraber sağlanan güç kablosunu sararak emniyetli şekilde cihazın arkasına yerleştirin.
4. Sevk mektubuna, gönderici bilgisi olarak şirket adınızı ve adres bilgilerinizi yazın.
5. Aşağıdaki e-posta şablonunu kullanarak Quantium Solutions’a e-posta gönderin.

    * Japan Post Chakubarai sevk mektubu gönderilmediyse veya kaybolduysa, gönderdiğiniz e-postada bunu da bildirin. Quantium Solutions Japan, teslim alma esnasında Japan Post’un sevk mektubunu da getirmesini ister.
    * Birden fazla siparişiniz varsa her birinin ayrı olarak teslim alınması için e-posta gönderin.

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

6. Teslim alma rezervasyonu yaptıktan sonra Quantium Solutions’tan e-posta onay iletisi alırsınız. E-posta onay iletisi, Chakubarai sevk mektubuna yönelik bilgileri de içerir.

Gerekirse, aşağıdaki bilgilerden Quantium Solutions Desteği (Japonca) ile iletişime geçebilirsiniz: 

* E-posta: Customerservice.JP@quantiumsolutions.com
* Telefon：03-5755-0150

## <a name="singapore"></a>[Singapur](#tab/in-singapore)

1. İade gönderimi için cihazı gönderirken kullanılan orijinal kutuyu saklayın.
2. İzleme numarasını (Data Box yerel web kullanıcı arabiriminin Göndermeye Hazırlama sayfasında başvuru numarası olarak gösterilir) not alın. Bu numara, göndermeye hazırlama adımı başarıyla tamamlandıktan sonra oluşturulur. Bu sayfadaki gönderim etiketini indirin ve kutunun üzerine yapıştırın.
3. Cihazı kapatın ve kabloları çıkarın.
4. Cihaz ile beraber sağlanan güç kablosunu sararak emniyetli şekilde cihazın arkasına yerleştirin. 
5. Aşağıdaki e-posta şablonuna takip numarasını ekleyin ve SingPost Müşteri Hizmetleri'ne gönderin.

    ```
    To: kadcustcare@singpost.com
    Subject: Microsoft Azure Pickup - OrderName 
    Body: 
        1. Requestor name  
        2. Requestor contact number
        3. Requestor collection address
        4. Preferred collection date
    ```

   > [!NOTE]
   > İş gününde alınan rezervasyon istekleri için:
   >
   > * 15:00’ten önce, teslim alma sonraki iş gününde 09:00 ile 13:00 arasında gerçekleştirilir.
   > * 15:00’ten sonra, teslim alma sonraki iş gününde 14:00 ile 18:00 arasında gerçekleştirilir.  

## <a name="south-africa"></a>[Güney Afrika](#tab/in-sa)

1. İade sevk irsaliyesi için cihazı paketetmek üzere kullanılan orijinal kutuyu koruyun.
2. Cihazın yerel Web Kullanıcı arabiriminde gösterilen başvuru numarasını (Waybill Number) göz önünde kalın. Bu sayı, **göndermeye hazırlama çalıştırması** başarılı olduktan sonra görüntülenir.
3. Cihazın yerel Web Kullanıcı arabiriminde bulunan ve sevkiyat paketine yapıştırılmış sevkiyat etiketini indirin ve yazdırın.
4. DHL ile bir toplama kitabı sağlamak için aşağıdaki seçeneklerden birini seçin:

    * Müşteri hizmeti iletişim merkezini, **+ 27 (0) 11 9213600**' de 2:00 PM 'den önce çağırın, 1. seçeneği belirleyin ve ardından Waybill numarasını belirtin.
    * Aşağıdaki şablonu kullanarak e-posta gönderin [Priority.Support@dhl.com](mailto:Priority.Support@dhl.com) :

    ```output
    To: Priority.Support@dhl.com
    Subject: Pickup request for Microsoft Azure
    Body: Need pick up for the below shipment
      *  DHL tracking number (reference number/waybill number)
      *  Requested pickup date: yyyy/mm/dd;time:HH MM
    ```

    * Alternatif olarak, paketi en yakın DHL hizmet noktasında bırakabilirsiniz.

5. Herhangi bir sorunla karşılaşırsanız, karşılaştığınız sorunların ayrıntıları ile e-posta gönderin [Priority.Support@dhl.com](mailto:Priority.Support@dhl.com) ve Waybill numarasını Konu: satırına koyun. Ayrıca + 27 (0) 119213902 çağırabilirsiniz.

## <a name="hong-kong"></a>[Hong Kong](#tab/in-hk)

1. Orijinal kutuya iade sevk irsaliyesi için cihazı paketleme.
2. Cihazın yerel Web Kullanıcı arabiriminde gösterilen başvuru numarasını (ters sevkiyat için izleme numarası) göz önünde bulunmak. Bu sayı, **göndermeye hazırlama çalıştırması** başarılı olduktan sonra görüntülenir.
3. Cihazın yerel Web Kullanıcı arabiriminde bulunan ve sevkiyat paketine yapıştırılmış sevkiyat etiketini indirin ve yazdırın.
4. Cihaz ile beraber sağlanan güç kablosunu sararak emniyetli şekilde cihazın arkasına yerleştirin.
5. Office saatlerinde ( **852) 2318 1213** ' de **Quantium çözümleri** hattını (00 Ila 18:00, Pazartesi 'den Cuma 'ya) çağırın.  
6. Teklif Microsoft Azure toplama ve bir koleksiyon için düzenlemek üzere iade sevkiyat etiketindeki başvuru numarası ve izleme numarası (barkod üzerinde).
7. Teslim alma zamanlaması için sözlü teyit alırsınız. Courier Collection 'a ulaşmazsa, alternatif düzenlemeler için Quantium çözüm hattını çağırın.
8. Quantium ile bir toplama işlemi oluşturduktan sonra, aşağıdaki şablonu kullanarak onayı [Microsoft Data Box Operations Asya](mailto:adbo@microsoft.com) ile paylaşabilirsiniz:

    ```output
    To: adbo@microsoft.com
    Subject: Microsoft Data Box Job: [order name] has completed copy
    Body:
    We have confirmed the pickup details with Quantium.

       * Requestor name:
       * Requestor contact number:
       * Pickup Date:  
       * Pickup time:
    ```

[adbo@microsoft.com](mailto:adbo@microsoft.com)Konu başlığında iş adını sağlayan ve karşılaşılan sorunla Data Box karşılaşmanız gerekir.

## <a name="self-managed"></a>[Kullanıcı Yönetimli](#tab/in-selfmanaged)

Japonya, Singapur, Kore, Hindistan, Güney Afrika veya Batı Avrupa Data Box kullanıyorsanız ve sipariş oluşturma sırasında kendi kendine yönetilen sevkiyat seçeneğini belirlediyseniz, bu yönergeleri izleyin.

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

---

## <a name="erasure-of-data-from-data-box"></a>Data Box'tan verileri silme
 
Cihaz Azure veri merkezine ulaştığında, Data Box [NıST SP 800-88 düzeltme 1 yönergelerine](https://csrc.nist.gov/News/2014/Released-SP-800-88-Revision-1,-Guidelines-for-Medi)göre disketlerinin verilerini siler.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, aşağıdaki gibi konular hakkında bilgi edindiniz:

> [!div class="checklist"]
> * Ön koşullar
> * Göndermeye hazırlama
> * Data Box'ı Microsoft'a gönderme
> * Data Box'tan verileri silme

Data Box yönetme hakkında bilgi edinmek için sonraki makaleye ilerleyin.

> [!div class="nextstepaction"]
> [Azure portal aracılığıyla Data Box yönetme](./data-box-portal-admin.md)


