---
title: Özel terim listeleri ne zaman olacak metin - İçerik Moderatör
titleSuffix: Azure Cognitive Services
description: Metin Moderasyonu API'sinde kullanılacak özel terimler listelerini oluşturmak için Liste Yönetimi API'sini kullanın.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: conceptual
ms.date: 01/10/2019
ms.author: pafarley
ms.openlocfilehash: 16cfb6c15a4d17ff3fb4f7f41f59f9f80af1e9e7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "75382132"
---
# <a name="moderate-with-custom-term-lists-in-the-api-console"></a>API konsolunda özel terim listeleri ile orta

Azure Content Moderator'daki varsayılan genel terim listesi, içerik moderasyonu ihtiyaçlarının büyük bölümü için yeterlidir. Bununla birlikte, kuruluşunuza özgü terimleri elemek gerekebilir. Örneğin, daha fazla incelemek üzere rakiplerin adlarını etiketlemek isteyebilirsiniz. 

Metin Moderasyonu API'sinde kullanılacak özel terimler listelerini oluşturmak için [Liste Yönetimi API'sini](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f67f) kullanın. **Metin - Ekran** işlemi metninizi küfür için tarar ve metni özel ve paylaşılan engelleme listeleriyle karşılaştırır.

> [!NOTE]
> En çok **5 terim listeniz** olabilir ve her listedeki **terimlerin sayısı 10.000'i aşmamalıdır**.
>

Liste Yönetimi API'sini aşağıdaki görevleri yapmak için kullanabilirsiniz:
- Liste oluşturma.
- Listeye terimleri ekleme.
- Terimleri listedeki terimlere göre eleme.
- Listeden terim silme.
- Listeyi silme.
- Liste bilgileri düzenleme.
- Listede yapılan değişikliklerin yeni taramaya eklenmesi için dizini yenileyin.

## <a name="use-the-api-console"></a>API konsolunu kullanma

API'yi çevrimiçi konsolda test etmeden önce abonelik anahtarınız gerekir. Bu anahtar **Ayarlar** sekmesinde, **Ocp-Apim-Abonelik-Anahtar** kutusunda bulunur. Daha fazla bilgi için bkz. [Genel Bakış](overview.md).

## <a name="refresh-search-index"></a>Arama dizini yenile

Dönem listesinde değişiklik yaptıktan sonra, gelecekteki taramalara eklenecek değişiklikler için dizinini yenilemeniz gerekir. Bu adım, masaüstünüzdeki bir arama motorunun (etkinse) veya web arama motorunun dizinini sürekli olarak yeni dosyalar veya sayfalar içerecek şekilde yenilemesine benzer.

1. Terim [Listesi Yönetimi API başvurusu,](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f67f)sol menüde, **Terim Listeleri'ni**seçin ve ardından **Arama Dizini Yenile'yi**seçin. 

   **Dönem Listeleri - Arama Dizini Yenile** sayfası açılır.

2. **API test konsolunu Aç**için, konumunuzu en yakından açıklayan bölgeyi seçin. 

   ![Dönem Listeleri - Arama Dizini sayfa bölge seçimini yenile](images/test-drive-region.png)

   **Dönem Listeleri - Arama Dizini** API konsolunu Yenile konsolu açılır.

3. **ListId** kutusuna, liste kimliği girin. Abonelik anahtarınızı girin ve sonra **Gönder'i**seçin.

   ![Dönem Listeleri API - Arama Dizini konsolu Yanıt içerik kutusunu yenile](images/try-terms-list-refresh-1.png)

## <a name="create-a-term-list"></a>Terim listesi oluşturma
1. [Dönem Listesi Yönetimi API başvurusuna](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f67f)gidin. 

   **Dönem Listeleri - Oluştur** sayfası açılır.

2. **API test konsolunu Aç**için, konumunuzu en yakından açıklayan bölgeyi seçin. 

   ![Dönem Listeleri - Sayfa bölgesi seçimi oluşturma](images/test-drive-region.png)

   **Dönem Listeleri - CREATE** API konsolu açılır.
 
3. **Ocp-Apim-Abonelik-Anahtar** kutusuna abonelik anahtarınızı girin.

4. İstek **gövdesi** kutusuna, **Ad** (örneğin, MyList) ve **Açıklama**için değerler girin.

   ![Dönem Listeleri - Konsol oluşturma İstek gövde adı ve açıklaması](images/try-terms-list-create-1.png)

5. Listenize daha açıklayıcı meta veriler atamak için anahtar değeri çifti yer tutucularını kullanın.

       {
          "Name": "MyExclusionList",
          "Description": "MyListDescription",
          "Metadata": 
          {
             "Category": "Competitors",
             "Type": "Exclude"
          }
       }

   Liste meta verilerini gerçek terimler olarak değil, anahtar değer çiftleri olarak ekleyin.
 
6. **Gönder**’i seçin. Listeniz oluşturulur. Yeni listeyle ilişkili **kimlik** değerini not edin. Diğer dönem listesi yönetimi işlevleri için bu kİmLİğİngerek.

   ![Dönem Listeleri - Konsol Oluştur Yanıt içerik kutusu liste kimliğini gösterir](images/try-terms-list-create-2.png)
 
7. MyList'e terimler ekleyin. Sol menüde, **Terim**altında , **Terim Ekle'yi**seçin. 

   **Terim - Terim Ekle** sayfası açılır. 

8. **API test konsolunu Aç**için, konumunuzu en yakından açıklayan bölgeyi seçin. 

   ![Dönem - Dönem sayfa bölge seçimi ekle](images/test-drive-region.png)

   **Terim - Terim Ekle** API konsolu açılır.
 
9. **ListId** kutusuna, oluşturduğunuz liste kimliğini girin ve **dil**için bir değer seçin. Abonelik anahtarınızı girin ve sonra **Gönder'i**seçin.

   ![Terim - Terim ekle konsol sorgu parametreleri](images/try-terms-list-create-3.png)
 
10. Terimin listeye eklendiğini doğrulamak için, sol menüde **Terim'i**seçin ve ardından **Tüm Terimleri Al'ı**seçin. 

    **Terim - Get All Terms** API konsolu açılır.

11. **ListId** kutusuna, liste kimliğigirin ve ardından abonelik anahtarınızı girin. **Gönder**’i seçin.

12. Yanıt **içeriği** kutusunda, girdiğiniz terimleri doğrulayın.

    ![Dönem - Get All Terms console Response içerik kutusu girdiğiniz terimleri listeler](images/try-terms-list-create-4.png)
 
13. Birkaç terim daha ekle. Artık özel bir terim listesi oluşturduğunuza göre, özel terim listesini kullanarak [bazı metinleri taramayı](try-text-api.md) deneyin. 

## <a name="delete-terms-and-lists"></a>Terimleri ve listeleri silme

Terim veya listeyi silmek basit bir işlemdir. Aşağıdaki görevleri yapmak için API'yi kullanırsınız:

- Terim silme. (**Dönem - Sil**)
- Listeyi silmeden listedeki tüm terimleri silme. (**Dönem - Tüm Terimleri Sil**)
- Listeyi ve tüm içeriğini silme. (**Dönem Listeleri - Sil**)

Bu örnek tek bir terimi siler.

1. Terim [Listesi Yönetimi API başvurusu,](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f67f)sol menüde, **Terim**'i seçin ve sonra **Sil'i**seçin. 

   **Terim - Sil** açılır.

2. **API test konsolunu Aç**için, konumunuzu en yakından açıklayan bölgeyi seçin. 

   ![Terim - Sayfa bölge seçimini silme](images/test-drive-region.png)

   **Terim - Delete** API konsolu açılır.
  
3. **ListId** kutusuna, bir terimi silmek istediğiniz listenin kimliğini girin. Bu kimlik, **Terim Listeleri'nde** döndürülen numaradır (örneğimizde, **122)**- MyList için Ayrıntılar al konsolu. Terimi girin ve bir dil seçin.
 
   ![Terim - Konsol sorgu parametrelerini silme](images/try-terms-list-delete-1.png)

4. Abonelik anahtarınızı girin ve sonra **Gönder'i**seçin.

5. Terimin silindiğini doğrulamak için Terim Listeleri - Tüm konsolu **al'ı** kullanın.

   ![Dönem Listeleri - Tüm konsol Yanıtı içerik kutusunu alın bu terimin silindiğini gösterir](images/try-terms-list-delete-2.png)
 
## <a name="change-list-information"></a>Liste bilgilerini değiştirme

Bir listenin adını ve açıklamasını dinleyebilir ve meta veri öğeleri ekleyebilirsiniz.

1. Terim [Listesi Yönetimi API başvurusu,](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f67f)sol menüde, **Dönem Listeleri'ni**seçin ve ardından **Ayrıntıları Güncelleştir'i**seçin. 

   **Dönem Listeleri - Güncelleme Ayrıntıları** sayfası açılır.

2. **API test konsolunu Aç**için, konumunuzu en yakından açıklayan bölgeyi seçin. 

   ![Dönem Listeleri - Ayrıntılar sayfa bölge seçimini güncelleştir](images/test-drive-region.png)

   **Dönem Listeleri - Ayrıntıları Güncelleştir** API konsolu açılır.

3. **ListId** kutusuna, liste kimliğigirin ve ardından abonelik anahtarınızı girin.

4. İstek **gövdesi** kutusunda, aramalarınızı yapın ve sonra **Gönder'i**seçin.

   ![Dönem Listeleri - Ayrıntıları Güncelle konsol İstek gövde lisitleri](images/try-terms-list-change-1.png)
 

## <a name="next-steps"></a>Sonraki adımlar

Kodunuzda REST API'sini kullanın veya uygulamanızla tümleştirmek için [Terim listeleri .NET quickstart](term-lists-quickstart-dotnet.md) ile başlayın.
