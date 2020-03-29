---
title: Özel listelere ve API konsoluna sahip Orta dereceli görüntüler - İçerik Moderatör
titleSuffix: Azure Content Moderator
description: Azure İçerik Moderatörü'ndeki Liste Yönetimi API'sini özel resim listeleri oluşturmak için kullanırsınız.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: conceptual
ms.date: 01/10/2019
ms.author: pafarley
ms.openlocfilehash: 27d9b12d9e1a0237050243c2b5f07edaa8d8857a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "72757186"
---
# <a name="moderate-with-custom-image-lists-in-the-api-console"></a>API konsolunda özel görüntü listeleri ile orta

Azure İçerik Moderatörü'ndeki [Liste Yönetimi API'sini](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f672) özel resim listeleri oluşturmak için kullanırsınız. Görüntü Moderasyon API'si ile özel resim listelerini kullanın. Görüntü denetleme işlemi görüntünüzün değerlendirilmesi. Özel listeler oluşturursanız, işlem özel listelerinizdeki resimlerle de karşılaştırır. Görüntüyü engellemek veya izin vermek için özel listeler kullanabilirsiniz.

> [!NOTE]
> Liste sayısı üst sınırı, her biri **10.000 görüntüyü aşmamak** kaydıyla **5 görüntü listesidir**.
>

Liste Yönetimi API'sini aşağıdaki görevleri yapmak için kullanırsınız:

- Liste oluşturma.
- Resimlerin bir listeye eklenmesi.
- Görüntüleri listedeki resimlere karşı taraya.
- Resimleri listeden silin.
- Listeyi silme.
- Liste bilgileri düzenleme.
- Listede yapılan değişikliklerin yeni taramaya eklenmesi için dizini yenileyin.

## <a name="use-the-api-console"></a>API konsolunu kullanma
API'yi çevrimiçi konsolda test etmeden önce abonelik anahtarınız gerekir. Bu, **Ayarlar** sekmesinde, **Ocp-Apim-Abonelik-Anahtar** kutusunda bulunur. Daha fazla bilgi için bkz. [Genel Bakış](overview.md).

## <a name="refresh-search-index"></a>Arama dizini yenile

Görüntü listesinde değişiklik yaptıktan sonra, gelecekteki taramalara eklenecek değişiklikler için dizinini yenilemeniz gerekir. Bu adım, masaüstünüzdeki bir arama motorunun (etkinse) veya web arama motorunun dizinini sürekli olarak yeni dosyalar veya sayfalar içerecek şekilde yenilemesine benzer.

1. Resim [Listesi Yönetimi API başvurusu,](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f672)sol menüde, **Resim Listeleri'ni**seçin ve ardından **Arama Dizini Yenile'yi**seçin.

   **Resim Listeleri - Arama Dizini Yenile** sayfası açılır.

2. **API test konsolunu Aç**için, konumunuzu en yakından açıklayan bölgeyi seçin. 
 
    ![Resim Listeleri - Arama Dizini sayfa bölge seçimini yenile](images/test-drive-region.png)

    **Resim Listeleri - Arama Dizini** API konsolunu Yenile açılır.

3. **ListId** kutusuna, liste kimliği girin. Abonelik anahtarınızı girin ve sonra **Gönder'i**seçin.

   ![Resim Listeleri - Arama Dizini konsolu Yanıt içerik kutusunu yenile](images/try-image-list-refresh-1.png)


## <a name="create-an-image-list"></a>Resim listesi oluşturma

1. [Görüntü Listesi Yönetimi API başvurusuna](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f672)gidin.

   **Resim Listeleri - Oluştur** sayfası açılır. 

3. **API test konsolunu Aç**için, konumunuzu en yakından açıklayan bölgeyi seçin.

   ![Resim Listeleri - Sayfa bölgesi seçimi oluşturma](images/test-drive-region.png)

   **Resim Listeleri - CREATE** API konsolu açılır.
 
4. **Ocp-Apim-Abonelik-Anahtar** kutusuna abonelik anahtarınızı girin.

5. İstek **gövdesi** kutusuna, **Ad** (örneğin, MyList) ve **Açıklama**için değerler girin.

   ![Resim Listeleri - Konsol oluşturma İstek gövde adı ve açıklaması](images/try-terms-list-create-1.png)

6. Listenize daha açıklayıcı meta veriler atamak için anahtar değeri çifti yer tutucularını kullanın.

       {
          "Name": "MyExclusionList",
          "Description": "MyListDescription",
          "Metadata": 
          {
            "Category": "Competitors",
            "Type": "Exclude"
          }
       }

   Liste meta verilerini gerçek görüntüler olarak değil, anahtar değer çiftleri olarak ekleyin.
 
7. **Gönder**’i seçin. Listeniz oluşturulur. Yeni listeyle ilişkili **kimlik** değerini not edin. Diğer görüntü listesi yönetim işlevleri için bu kİmLİğİnGEREKDİ.

   ![Resim Listeleri - Konsol Oluştur Yanıt içerik kutusu liste kimliğini gösterir](images/try-terms-list-create-2.png)
 
8. Ardından, MyList'e resim ekleyin. Sol menüde **Resim'i**seçin ve ardından **Resim Ekle'yi**seçin.

   **Resim - Resim Ekle** sayfası açılır. 

9. **API test konsolunu Aç**için, konumunuzu en yakından açıklayan bölgeyi seçin.

   ![Resim - Resim sayfası bölge seçimi ekle](images/test-drive-region.png)

   **Resim - Resim Ekle** API konsolu açılır.
 
10. **ListId** kutusuna, oluşturduğunuz liste kimliğini girin ve ardından eklemek istediğiniz resmin URL'sini girin. Abonelik anahtarınızı girin ve sonra **Gönder'i**seçin.

11. Resmin listeye eklendiğini doğrulamak için sol menüde **Resim'i**seçin ve ardından **Tüm Görüntü Kimliklerini Al'ı**seçin.

    **Resim - Tüm Görüntü Ids** API konsolu açılır alın.
  
12. **ListId** kutusuna, liste kimliğigirin ve ardından abonelik anahtarınızı girin. **Gönder**’i seçin.

    ![Resim - Tüm Resim Kimlikleri konsolu Yanıt içerik kutusunu alın girdiğiniz görüntüleri listeler](images/try-image-list-create-11.png)
 
10. Birkaç resim daha ekle. Artık özel bir resim listesi oluşturduğunuza göre, özel resim listesini kullanarak [görüntüleri değerlendirmeyi](try-image-api.md) deneyin. 

## <a name="delete-images-and-lists"></a>Resimleri ve listeleri silme

Bir resmi veya listeyi siler. Aşağıdaki görevleri yapmak için API'yi kullanabilirsiniz:

- Görüntüyü silin. (**Resim - Sil**)
- Listeyi silmeden listedeki tüm görüntüleri silin. (**Resim - Tüm Görüntüleri Sil**)
- Listeyi ve tüm içeriğini silme. (**Resim Listeleri - Sil**)

Bu örnek tek bir resmi siler:

1. Resim [Listesi Yönetimi API başvurusu,](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f672)sol menüde, **Resim'i**seçin ve sonra **Sil'i**seçin. 

   **Resim - Sil** sayfası açılır.

2. **API test konsolunu Aç**için, konumunuzu en yakından açıklayan bölgeyi seçin. 

   ![Resim - Sayfa bölge seçimini silme](images/test-drive-region.png)
 
   **Resim - API konsolu silin** açılır.
 
3. **ListId** kutusuna, bir resmi silmek için listenin kimliğini girin.  Bu, Resim'de döndürülen **numaradır -** MyList için Tüm Görüntü Kimlikleri konsolu alın. Ardından, silmek için resmin **ImageId'ini** girin. 

Örneğimizde, liste kimliği **58953**, **ContentSource**için değeri. Görüntü kimliği **59021**, **ContentIds**için değer .

1. Abonelik anahtarınızı girin ve sonra **Gönder'i**seçin.

1. Resmin silindiğini doğrulamak için **Image - Get All Image Ids** konsoluna sahip olun.
 
## <a name="change-list-information"></a>Liste bilgilerini değiştirme

Bir listenin adını ve açıklamasını dinleyebilir ve meta veri öğeleri ekleyebilirsiniz.

1. Resim [Listesi Yönetimi API başvurusu,](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f672)sol menüde, **Resim Listeleri'ni**seçin ve ardından **Ayrıntıları Güncelleştir'i**seçin. 

   **Resim Listeleri - Ayrıntıları Güncelle** sayfası açılır.

2. **API test konsolunu Aç**için, konumunuzu en yakından açıklayan bölgeyi seçin.  

    ![Resim Listeleri - Ayrıntılar sayfa bölge seçimini güncelleştir](images/test-drive-region.png)

    **Resim Listeleri - Ayrıntıları Güncelle** API konsolu açılır.
 
3. **ListId** kutusuna, liste kimliğigirin ve ardından abonelik anahtarınızı girin.

4. İstek **gövdesi** kutusunda, aramalarınızı yapın ve ardından sayfadaki **Gönder** düğmesini seçin.

   ![Resim Listeleri - Ayrıntıları Güncelle konsol İstek gövde lisitleri](images/try-terms-list-change-1.png)
 

## <a name="next-steps"></a>Sonraki adımlar

Kodunuzda REST API'sini kullanın veya uygulamanızla tümleştirmek için [Görüntü listeleri .NET quickstart](image-lists-quickstart-dotnet.md) ile başlayın.
