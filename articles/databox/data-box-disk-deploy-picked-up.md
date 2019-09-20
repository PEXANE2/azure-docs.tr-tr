---
title: Microsoft Azure Data Box Disk'i geri göndermeye yönelik öğretici | Microsoft Docs
description: Azure Data Box Disk'inizi Microsoft'a nasıl gönderebileceğinizi öğrenmek için bu öğreticiyi kullanın
services: databox
author: alkohli
ms.service: databox
ms.subservice: disk
ms.topic: tutorial
ms.date: 09/19/2019
ms.author: alkohli
ms.localizationpriority: high
Customer intent: As an IT admin, I need to be able to order Data Box Disk to upload on-premises data from my server onto Azure.
ms.openlocfilehash: 9f0bdbf69336f792479c0089be351e468d3babe2
ms.sourcegitcommit: 2ed6e731ffc614f1691f1578ed26a67de46ed9c2
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/19/2019
ms.locfileid: "71128696"
---
::: zone target="chromeless"

## <a name="return-azure-data-box-disk"></a>Azure Data Box Disk’i İade Etme 

::: zone-end

::: zone target="docs"

# <a name="tutorial-return-azure-data-box-disk"></a>Öğretici: Azure Data Box Disk’i İade Etme 

Bu öğretici, Azure Data Box Disk'inizi iade etmek için bir teslimat zamanlamayı açıklar. Öğretici yönergeleri, cihazı nereye iade ettiğinize bağlı olarak değişir. 

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Data Box Disk'i Microsoft'a gönderme
> * Data Box Disk’i farklı bölgelerde teslim alma

## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce [Öğretici: Verileri Azure Data Box Disk'e kopyalama ve doğrulama](data-box-disk-deploy-copy-data.md) kaynağını tamamladığınızdan emin olun.


## <a name="ship-data-box-disk-back"></a>Data Box Disk'i geri gönderme

::: zone-end

1. Veri doğrulama tamamlandıktan sonra diskleri çıkarın. Bağlantı kablolarını çıkarın.
2. Tüm diskleri ve bağlantı kablolarını kabarcıklı naylona sarın ve bunları sevkiyat kutusuna yerleştirin. Aksesuarlar eksikse ücret uygulanabilir.
    - Ürün size ilk ulaştığında gönderilen paketi kullanın.  
    - Disklerinizi, hava kabarcıklı ambalaj malzemesiyle sıkıca sararak paketlemenizi öneririz.
    - Diskin kutunun içinde hareket etmesini engellemek için kutuda boşluk kalmadığından emin olun.

Sonraki adımlar cihazın iade edildiği bölgeye göre belirlenir. ABD/Kanada, Avrupa Birliği (AB), Avustralya veya Asya’daki ülkeler için farklı yönergeler uygulanır.

### <a name="in-us-or-canadatabin-us-or-canada"></a>[ABD veya Kanada](#tab/in-us-or-canada)

Cihazı ABD veya Kanada’da iade ediyorsanız aşağıdaki adımları uygulayın.

1. Kutuya yapıştırılmış şeffaf plastik kılıftaki iade sevkiyat etiketini kullanın. Etiket zarar gördüyse veya kayıpsa:
    - **Genel Bakış > Sevkiyat Etiketini İndir**’e gidip bir iade sevkiyat etiketi indirin.
    - Etiketi cihaza iliştirin.

2. Sevkiyat kutusunu mühürleyin ve iade sevkiyat etiketinin görünür olduğundan emin olun.
3. UPS ile teslim alma zamanı planlayın. Teslim alma planlamak için:

    - Yerel UPS şubesini (ülkeye/bölgeye özel ücretsiz hatlar) arayın.
    - Görüşme esnasında, basılı etiketinizde gördüğünüz iade izleme numarasını bildirin.
    - İzleme numarası bildirilmezse, teslim alma esnasında UPS’ye ek ücret ödemeniz gerekir.
    - Teslim alma zamanlama yerine Data Box Disk’inizi en yakın teslim etme yerine bırakabilirsiniz.

### <a name="in-europetabin-europe"></a>[Avrupa](#tab/in-europe)

Cihazı Avrupa’da iade ediyorsanız aşağıdaki adımları uygulayın.

1. Kutuya yapıştırılmış şeffaf plastik kılıftaki iade sevkiyat etiketini kullanın. Etiket zarar gördüyse veya kayıpsa:
    - **Genel Bakış > Sevkiyat Etiketini İndir**’e gidip bir iade sevkiyat etiketi indirin.
    - Etiketi cihaza iliştirin.

2. Sevkiyat kutusunu mühürleyin ve iade sevkiyat etiketinin görünür olduğundan emin olun.
3. Cihazı Avrupa'da DHL ile iade ediyorsanız, DHL'in web sitesini ziyaret edip bir havayolu fatura numarası belirterek toplama isteğinde bulunun.
4. Ülkenin/bölgenin DHL Ekspres web sitesine gidin ve **Kurye Çağırma Rezervasyonu > eİade Sevkiyatı**'nı seçin.    
3. Konşimento numarasını belirtin ve toplama ayarlaması yapmak için **Toplama Zamanlama**'ya tıklayın.

### <a name="in-australiatabin-australia"></a>[Avustralya](#tab/in-australia)

Avustralya’daki Azure veri merkezlerinde ek güvenlik bildirimi uygulanır. Gelen tüm sevkiyatların önceden bildirilmesi gerekir. Avustralya’da teslim alma için aşağıdaki adımları uygulayın.

1. Sağlanan iade sevkiyatı etiketini kullanın ve bunun üzerinde TAU kodunun (referans numarası) bulunduğundan emin olun. Sağlanan sevkiyat etiketi eksikse veya başka bir sorununuz varsa şuraya e-posta gönderin [Data Box için Asya bölgesi İşlemleri](mailto:adbo@microsoft.com). Konu üst bilgisi olarak sipariş adını ve karşılaştığınız sorunun ayrıntılarını belirtin.
3. Etiketi kutuya iliştirin. 
4. Şu bağlantıdan çevrimiçi olarak bir teslim alma rezervasyonu yapın: https://mydhl.express.dhl/au/en/schedule-pickup.html#/schedule-pickup#label-reference. 

### <a name="in-japantabin-japan"></a>[Japonya](#tab/in-japan)

1. Sevk mektubuna, gönderici bilgisi olarak şirket adınızı ve adres bilgilerinizi yazın.
2. Aşağıdaki e-posta şablonunu kullanarak Quantium Solutions’a e-posta gönderin.

    ```
    To: Customerservice.JP@quantiumsolutions.com
    Subject: Pickup request for Microsoft Azure Data Box Disk｜Job Name： 
    Body: 
    - Japan Post Yu-Pack tracking number (reference number)：
    - Requested pickup date：mmdd (Select a requested time slot from below).
        a. 08：00-13：00 
        b. 13：00-15：00 
        c. 15：00-17：00 
        d. 17：00-19：00 
    ```
    - **Osaka’da teslim alıyorsanız** e-posta şablonundaki konu başlığını şöyle değiştirin: `Pickup request for Microsoft Azure OSA`.
    - Japan Post Chakubarai sevk mektubu gönderilmediyse veya kaybolduysa, gönderdiğiniz e-postada bunu da bildirin. Quantium Solutions Japan, teslim alma esnasında Japan Post’un sevk mektubunu da getirmesini ister.
    - Birden fazla siparişiniz varsa her birinin ayrı olarak teslim alınması için e-posta gönderin.

3. Teslim alma rezervasyonu yaptıktan sonra Quantium Solutions’tan e-posta onay iletisi alırsınız. E-posta onay iletisi, Chakubarai sevk mektubuna yönelik bilgileri de içerir.

Gerekirse, aşağıdaki bilgilerden Quantium Solutions Desteği (Japonca) ile iletişime geçebilirsiniz: 

- E-posta: Customerservice.JP@quantiumsolutions.com 
- Telefon：03-5755-0150 

### <a name="in-koreatabin-korea"></a>[Güney Kore](#tab/in-korea)

1. İade sevk mektubunu eklediğinizden emin olun.
2. Sevk mektubunuz mevcutken iade istemek için:
    1. Çalışma saatleri içinde (Pazartesi’den Cuma’ya kadar saat 10.00 - 17.00 arası) 070-8231-1418 numaralı telefondan *Quantium Solutions International* bilgi hattını arayın. Teslim alma planlamak için *Microsoft Azure teslim alma hizmetini* ve hizmet isteği numarasını bildirin.  
    2. Bilgi hattı meşgulse referans olarak *Microsoft Azure teslim alma hizmetini* ve hizmet isteği numarasını bildirerek `microsoft@rocketparcel.com` adresine e-posta gönderin.
    3. Teslim alma için kurye gönderilmezse alternatif planlamalar için *Quantium Solutions International*’ı arayın. 
    4. Teslim alma zamanlaması için bir onay e-posta iletisi alırsınız.
3. Bu adımı sevk mektubunun bulunmadığı durumlarda uygulayın. Teslim alma istemek için:
    1. Çalışma saatleri içinde (Pazartesi’den Cuma’ya kadar saat 10.00 - 17.00 arası) 070-8231-1418 numaralı telefondan *Quantium Solutions International* bilgi hattını arayın. Teslim alma planlamak için *Microsoft Azure teslim alma hizmetini* ve hizmet isteği numarasını bildirin. Teslim alma için yeni bir sevk mektubuna ihtiyaç duyduğunuzu belirtin. Gönderici (müşteri), alıcı (Azure veri merkezi) bilgilerini ve referans numarasını (hizmet isteği numarası) sağlayın. 
    2. Bilgi hattı meşgulse referans olarak *Microsoft Azure teslim alma hizmetini* ve hizmet isteği numarasını bildirerek `microsoft@rocketparcel.com` adresine e-posta gönderin.
    3. Teslim alma için kurye gönderilmezse alternatif planlamalar için *Quantium Solutions International*’ı arayın. 
    4. İstek telefon aracılığıyla yapıldıysa sözlü teyit alırsınız.


### <a name="in-singaporetabin-singapore"></a>[Singapur’da](#tab/in-singapore)

1. Sevk mektubunu yazdırın ve kutuya iliştirin. Etiket zarar gördüyse veya kayıpsa:
    - **Genel Bakış > Sevkiyat Etiketini İndir**’e gidip bir iade sevkiyat etiketi edinin.
    - Etiketi cihaza iliştirin. Etiketin görünür olduğundan emin olun.

2. Teslim alma istemek için:
    - Çalışma saatleri içinde (Pazartesi’den Cuma’ya kadar saat 09.00 - 17.00 arası) **6845 6485** numaralı telefondan **SingPost** yardım hattını arayın.  
    - Teslim alma planlamak için *Microsoft Azure teslim alma hizmetini* ve hizmet isteği numarasını (iade sevkiyat etiketinin üzerinde yer alan izleme numarası) bildirin. 
    - Teslim alma zamanlaması için sözlü teyit alırsınız. 
    - Teslim alması için kurye gönderilmezse alternatif planlamalar için **6845 6485** numaralı telefondan **SingPost**’u arayın. 
3. Paketi kuryeye teslim edin. 


::: zone target="docs"

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide aşağıdaki Azure Data Box Disk konularını öğrendiniz:

> [!div class="checklist"]
> * Data Box Disk'i Microsoft'a gönderme
> * Data Box Disk’i farklı bölgelerde teslim alma

Data Box Disk’ten Azure Depolama hesabına yapılan veri yüklemesini doğrulamayı öğrenmek için sonraki nasıl yapılır kılavuzuna geçin.

> [!div class="nextstepaction"]
> [Azure Data Box Disk’ten veri yüklemesini doğrulama](./data-box-disk-deploy-picked-up.md)

::: zone-end




