---
title: Metin Analizi ve Güç Otomatikleştirme'yi kullanarak Excel'de bilgi ayıklama
titleSuffix: Azure Cognitive Services
description: Metin Analizi ve Power Automate'i kullanarak kod yazmak zorunda kalmadan Excel metnini nasıl ayıklayın öğrenin.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: article
ms.date: 02/27/2019
ms.author: aahi
ms.openlocfilehash: fd70fe14d3765fb7c21b92f62b4d73564176baa2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78201193"
---
# <a name="extract-information-in-excel-using-text-analytics-and-power-automate"></a>Metin Analizi ve Güç Otomatikleştirme'yi kullanarak Excel'de bilgi ayıklama 

Bu öğreticide, kod yazmak zorunda kalmadan Excel elektronik tablosundaki metni ayıklamak için bir Güç Otomatikleştir akışı oluşturursunuz. 

Bu akış, bir apartman kompleksi hakkında bildirilen sorunların bir elektronik tablosu nu alır ve bunları iki kategoriye ayırır: tesisat ve diğer. Ayrıca onları gönderen kiracıların adlarını ve telefon numaralarını ayıklayacak. Son olarak, akış bu bilgileri Excel sayfasına ekler. 

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Akış oluşturmak için Güç Otomatikleştir'i kullanın
> * OneDrive for Business'tan Excel verilerini yükleyin
> * Excel'den metin ayıklayın ve Metin Analizi API'sine gönderin 
> * Excel sayfasını güncelleştirmek için API'deki bilgileri kullanın.

## <a name="prerequisites"></a>Ön koşullar

- Bir Microsoft Azure hesabı. [Ücretsiz bir deneme başlatın](https://azure.microsoft.com/free/) veya [oturum açın](https://portal.azure.com/).
- Bir Metin Analizi kaynağı. Yoksa, [Azure portalında bir tane oluşturabilir](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesTextAnalytics) ve bu öğreticiyi tamamlamak için ücretsiz katmanı kullanabilirsiniz.
- Kayıt sırasında sizin için oluşturulan [anahtar ve bitiş noktası.](../../cognitive-services-apis-create-account.md#get-the-keys-for-your-resource)
- Kiracı sorunlarını içeren bir elektronik tablo. Örnek veriler GitHub'da sağlanır
- Office 365, OneDrive for Business ile birlikte.

## <a name="add-the-excel-file-to-onedrive-for-business"></a>İş Için OneDrive'a Excel dosyasını ekleme

Örnek Excel dosyasını [GitHub'dan indirin.](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/TextAnalytics/sample-data/ReportedIssues.xlsx) Bu dosya, OneDrive for Business hesabınızda depolanmalıdır.

> [!div class="mx-imgBorder"] 
> :::image type="content" source="../media/tutorials/excel/example-data.png" alt-text="Excel dosyasından örnekler.":::

Sorunlar ham metinde bildirilir. Kişi adını ve telefon numarasını ayıklamak için Text Analytics API'nin Adlandırılmış Varlık Tanıma sını kullanacağız. Sonra akış sorunları kategorize etmek için açıklamasında kelime "sıhhi tesisat" arayacaktır. 

## <a name="create-a-new-power-automate-workflow"></a>Yeni bir Power Otomatikleştirme iş akışı oluşturun

[Power Automate sitesine](https://preview.flow.microsoft.com/)gidin ve oturum açın. Ardından **Oluştur** ve **Zamanlama akışını**tıklatın.

> [!div class="mx-imgBorder"] 
> :::image type="content" source="../media/tutorials/excel/flow-creation.png" alt-text="Akış oluşturma ekranı.":::


**Zamanlanmış akış** sayfası oluştur'da, akışınızı aşağıdaki alanları içeren başlatma:

|Alan |Değer  |
|---------|---------|
|**Akış adı**     | **Zamanlanmış İnceleme** veya başka bir ad.         |
|**Başlatılıyor**     |  Geçerli tarih ve saati girin.       |
|**Her tekrarlayın**     | **1 saat**        |

## <a name="add-variables-to-the-flow"></a>Akışa değişken ekleme

> [!NOTE]
> Tamamlanan akışın bir görüntüsünü görmek istiyorsanız, [GitHub'dan](https://github.com/Azure-Samples/cognitive-services-sample-data-files/tree/master/TextAnalytics/flow-diagrams)indirebilirsiniz. 

Excel dosyasına eklenecek bilgileri temsil eden değişkenler oluşturun. **Yeni Adım'ı** tıklatın ve **Değişkeni Başlatma'yı**arayın. Dört değişken oluşturmak için bunu dört kez yapın.

> [!div class="mx-imgBorder"] 
> :::image type="content" source="../media/tutorials/excel/initialize-variables.png" alt-text="Değişkenleri başlangıç.":::

Oluşturduğunuz değişkenlere aşağıdaki bilgileri ekleyin. Excel dosyasının sütunlarını temsil ederler. Herhangi bir değişken daraltılırsa, bunları genişletmek için bunları tıklatabilirsiniz.

| Eylem |Adı   | Tür | Değer |
|---------|---------|---|---|
| Değişkeni başlatma | var_person | Dize | Kişi |
| Değişken 2'yi başlatma | var_phone | Dize | Phone_Number |
| Değişken 3'e başharf | var_plumbing | Dize | Sıhhi tesisat |
| Değişken 4'e başharf | var_other | Dize | diğer | 

> [!div class="mx-imgBorder"] 
> :::image type="content" source="../media/tutorials/excel/flow-variables.png" alt-text="akış değişkenlerinde yer alan bilgiler":::

## <a name="read-the-excel-file"></a>Excel dosyasını okuyun

**Yeni Adım'ı** tıklatın ve **Excel**yazın, ardından eylemler listesinden **tabloda bulunan liste satırlarını** seçin.

> [!div class="mx-imgBorder"] 
> :::image type="content" source="../media/tutorials/excel/list-excel-rows.png" alt-text="excel satırları ekleyin.":::

Bu eylemdeki alanları doldurarak Excel dosyasını akışa ekleyin. Bu öğretici, dosyanın OneDrive for Business'a yüklenmesini gerektirir.

> [!div class="mx-imgBorder"] 
> :::image type="content" source="../media/tutorials/excel/list-excel-rows-options.png" alt-text="excel satırları ekleyin.":::

**Yeni Adım'ı** tıklatın ve **her eyleme bir Uygula** ekleyin.

> [!div class="mx-imgBorder"] 
> :::image type="content" source="../media/tutorials/excel/add-apply-action.png" alt-text="bir uygula komutu ekleyin.":::

Önceki **adımdan çıktı seç'e**tıklayın. Görünen Dinamik içerik kutusunda **değeri**seçin.

> [!div class="mx-imgBorder"] 
> :::image type="content" source="../media/tutorials/excel/select-output.png" alt-text="Excel dosyasından çıktıseçin.":::

## <a name="send-a-request-to-the-text-analytics-api"></a>Metin Analizi API'sine istek gönderme

Henüz yapmadıysanız, Azure portalında bir [Metin Analizi kaynağı](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesTextAnalytics) oluşturmanız gerekir.

### <a name="create-a-text-analytics-connection"></a>Metin Analizi bağlantısı oluşturma

Her **birine Uygula'da** **eylem ekle'yi**tıklatın. Azure portalındaki Metin Analizi kaynağınızın **anahtar ve bitiş noktası** sayfasına gidin ve Metin Analizi kaynağınızın anahtar ve bitiş noktasını alın.

Akışınızda, yeni bir Metin Analizi bağlantısı oluşturmak için aşağıdaki bilgileri girin.

> [!NOTE]
> Zaten bir Metin Analizi bağlantısı oluşturduysanız ve bağlantı bilgilerinizi değiştirmek istiyorsanız, sağ üst köşedeki elipsleri tıklatın ve **+ Yeni bağlantı ekle'yi**tıklatın.

| Alan           | Değer                                                                                                             |
|-----------------|-------------------------------------------------------------------------------------------------------------------|
| Bağlantı Adı | Text Analytics kaynağınıza bağlantıiçin bir ad. Örneğin, `TAforPowerAutomate`. |
| Hesap anahtarı     | Text Analytics kaynağınızın anahtarı.                                                                                   |
| Site URL'si        | Text Analytics kaynağınızın bitiş noktası.                                                       |

> [!div class="mx-imgBorder"] 
> :::image type="content" source="../media/tutorials/excel/add-credentials.png" alt-text="Akışınıza Metin Analizi kimlik bilgilerini ekleyin.":::

## <a name="extract-the-excel-content"></a>Excel içeriğini ayıklama 

Bağlantı oluşturulduktan sonra **Metin Analizi'ni** arayın ve **Varlıkları**seçin. Bu, sorunun açıklama sütunundan bilgi ayıklar.

> [!div class="mx-imgBorder"] 
> :::image type="content" source="../media/tutorials/excel/extract-info.png" alt-text="Akışınıza Metin Analizi kimlik bilgilerini ekleyin.":::

**Metin** alanına tıklayın ve görünen Dinamik içerik pencerelerinden **Açıklama'yı** seçin. Dil `en` için girin. (Dil görmüyorsanız gelişmiş seçenekleri göster'i tıklatın)

> [!div class="mx-imgBorder"] 
> :::image type="content" source="../media/tutorials/excel/description-from-dynamic-content.png" alt-text="Akışınıza Metin Analizi kimlik bilgilerini ekleyin.":::


## <a name="extract-the-person-name"></a>Kişi adını ayıklama

Ardından, Text Analytics çıktısında kişi varlık türünü bulacağız. Her **birine Uygula'da** **eylem ekle'yi**tıklatın ve her eylem için başka bir **Uygula** oluşturun. Metin kutusunun içini tıklatın ve görünen Dinamik İçerik penceresinde **Varlıkları** seçin.

> [!div class="mx-imgBorder"] 
> :::image type="content" source="../media/tutorials/excel/add-apply-action-2.png" alt-text="Akışınıza Metin Analizi kimlik bilgilerini ekleyin.":::

Yeni oluşturulan **her 2 eyleme uygula'da** **eylem ekle'yi**tıklatın ve **Durum** denetimi ekleyin.

> [!div class="mx-imgBorder"] 
> :::image type="content" source="../media/tutorials/excel/create-condition.png" alt-text="Akışınıza Metin Analizi kimlik bilgilerini ekleyin.":::

Durum penceresinde, ilk metin kutusunu tıklatın. Dinamik içerik penceresinde, **Varlıklar Türünü** arayın ve seçin.

> [!div class="mx-imgBorder"] 
> :::image type="content" source="../media/tutorials/excel/choose-entities-value.png" alt-text="Akışınıza Metin Analizi kimlik bilgilerini ekleyin.":::

İkinci kutunun **''ye eşit olarak**ayarlandıklarına emin olun Ardından üçüncü kutuyu seçin `var_person` ve Dinamik içerik penceresinde arama yapın. 

> [!div class="mx-imgBorder"] 
> :::image type="content" source="../media/tutorials/excel/choose-variable-value.png" alt-text="Akışınıza Metin Analizi kimlik bilgilerini ekleyin.":::

Evet **çıkarsa** koşulunda, Excel'de yazın ve **ardından Satır ı Güncelleştir'i**seçin.

> [!div class="mx-imgBorder"] 
> :::image type="content" source="../media/tutorials/excel/yes-column-action.png" alt-text="Akışınıza Metin Analizi kimlik bilgilerini ekleyin.":::

Excel bilgilerini girin ve **Anahtar Sütun,** **Anahtar Değeri** ve **Kişi Adı** alanlarını güncelleştirin. Bu, API tarafından algılanan adı Excel sayfasına ekler. 

> [!div class="mx-imgBorder"] 
> :::image type="content" source="../media/tutorials/excel/yes-column-action-options.png" alt-text="Akışınıza Metin Analizi kimlik bilgilerini ekleyin.":::

## <a name="get-the-phone-number"></a>Telefon numarasını alın

Adı tıklayarak her 2 eylem **için Uygula** en aza indirin. Ardından, **her eyleme** daha önce olduğu gibi başka bir Uygula ekleyin. her 3 **için Uygula**olarak adlandırılacaktır. Metin kutusunu seçin ve bu eylem için çıktı olarak **varlıklar** ekleyin. 

> [!div class="mx-imgBorder"] 
> :::image type="content" source="../media/tutorials/excel/add-apply-action-3.png" alt-text="Akışınıza Metin Analizi kimlik bilgilerini ekleyin.":::

Her **3'e Uygula**içinde, bir **Durum** denetimi ekleyin. Bu Durum **2**olarak adlandırılacaktır. İlk metin kutusunda Dinamik içerik penceresinden **Varlıklar Türünü** arayın ve ekleyin. Orta kutunun 'a **eşit olarak**ayarlandıklarına emin olun Daha sonra, sağ metin `var_phone`kutusuna, girin. 

> [!div class="mx-imgBorder"] 
> :::image type="content" source="../media/tutorials/excel/condition-2-options.png" alt-text="Akışınıza Metin Analizi kimlik bilgilerini ekleyin.":::

Evet **çıkarsa** koşulunda, bir satır eylemini **güncelleştir'** ekleyin. Daha sonra excel sayfasının telefon numaraları sütununa yukarıda yaptığımız gibi bilgileri girin. Bu, API tarafından algılanan telefon numarasını Excel sayfasına ekler. 

> [!div class="mx-imgBorder"] 
> :::image type="content" source="../media/tutorials/excel/condition-2-yes-column.png" alt-text="Akışınıza Metin Analizi kimlik bilgilerini ekleyin.":::


## <a name="get-the-plumbing-issues"></a>Tesisat sorunlarını alın

En aza indirin Her **3'e** adı tıklayarak uygulayın. Ardından, üst eylemdeki **her biri için** başka bir Uygula oluşturun. Metin kutusunu seçin ve Dinamik içerik penceresinden bu eylem için çıktı olarak **Varlıklar** ekleyin. 

> [!div class="mx-imgBorder"] 
> :::image type="content" source="../media/tutorials/excel/add-apply-action-4.png" alt-text="Akışınıza Metin Analizi kimlik bilgilerini ekleyin.":::


Ardından, akış Excel tablo satırındaki sorun açıklamasının "tesisat" sözcüğünden olup olmadığını denetler. Evet ise, IssueType sütununa "sıhhi tesisat" ekler. Yoksa, "diğer" gireriz.

Her **4 eylemiçin Uygula** içinde, bir **Koşul** Denetimi ekleyin. Bu Durum **3**olarak adlandırılacaktır. İlk metin kutusunda Dinamik içerik penceresini kullanarak Excel dosyasından **Açıklama'yı** arayın ve ekleyin. Orta daki kutunun **.** Daha sonra, sağ metin kutusunda, `var_plumbing`bul ve seçin. 

> [!div class="mx-imgBorder"] 
> :::image type="content" source="../media/tutorials/excel/condition-3-options.png" alt-text="Akışınıza Metin Analizi kimlik bilgilerini ekleyin.":::


Evet **çıkarsa** koşulunda, **eylem ekle'yi**tıklatın ve **satırı güncelleştir'i**seçin. Sonra eskisi gibi bilgileri girin. IssueType sütununda `var_plumbing`. Bu satıra bir "sıhhi tesisat" etiketi uygular.

If **koşulunda,** **eylem ekle'yi**tıklatın ve **satırı Güncelleştir'i**seçin. Sonra eskisi gibi bilgileri girin. IssueType sütununda `var_other`. Bu satıra bir "diğer" etiketi uygular.

> [!div class="mx-imgBorder"] 
> :::image type="content" source="../media/tutorials/excel/plumbing-issue-condition.png" alt-text="Akışınıza Metin Analizi kimlik bilgilerini ekleyin.":::

## <a name="test-the-workflow"></a>İş akışını test etme

Ekranın sağ üst köşesinde **Kaydet'i**tıklatın, ardından **Test edin.** **Tetikleyici eylemini gerçekleştireceğimi**seçin. **& Test kaydet,** **akışı çalıştır'** ı, ardından **Bitti'yi**tıklatın.

Excel dosyası OneDrive hesabınızda güncellenir. Aşağıdaki gibi görünecektir.

> [!div class="mx-imgBorder"] 
> :::image type="content" source="../media/tutorials/excel/updated-excel-sheet.png" alt-text="Güncelleştirilmiş excel elektronik tablosu.":::

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Daha fazla çözüm keşfedin](../text-analytics-user-scenarios.md)
