---
title: 'Quickstart: Toplu sorularla test bilgi tabanı'
titleSuffix: Azure Cognitive Services
description: ''
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: quickstart
ms.date: 02/08/2020
ms.author: diberry
ms.openlocfilehash: 3bc095d8949f177ccb6c4cc111ba4b272027904e
ms.sourcegitcommit: 441db70765ff9042db87c60f4aa3c51df2afae2d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80756704"
---
# <a name="quickstart-test-knowledge-base-with-batch-questions-and-expected-answers"></a>Quickstart: Toplu sorular ve beklenen yanıtlarla bilgi tabanını test edin

Beklenen yanıtlar, güven puanları ve çoklu dönüş istemleri için QnA Maker kaynağınızdaki bilgi tabanlarını test etmek için QnA Maker toplu test aracını kullanın.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği - [ücretsiz bir abonelik oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
* [Bir QnA Maker hizmeti oluşturun](create-publish-knowledge-base.md) veya İngilizce dilini kullanan varolan bir hizmeti kullanın.
* Çok [döndürme `.docx` örnek dosyasını](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/multi-turn.docx) indirin
* Toplu [işlem aracını](https://aka.ms/qnamakerbatchtestingtool)indirin, dosyadan `.zip` çalıştırılabilir dosyayı ayıklayın.

## <a name="sign-into-qna-maker-portal"></a>QnA Maker portalında oturum açın

QnA Maker portalında [oturum açın.](https://www.qnamaker.ai/)

## <a name="create-a-new-knowledge-base-from-the-multi-turn-sampledocx-file"></a>Çok döndürme sample.docx dosyasından yeni bir bilgi tabanı oluşturma

1. Araç çubuğundan **bilgi tabanı oluştur'u** seçin.
1. Varolan kaynak bilgilerinizi seçmek için **Adım 2'ye** geçerek zaten bir QnA Maker kaynağınız olması gerektiğinden **Adım 1'i** atlayın:
    * Azure Etkin Dizin Kimliği
    * Azure Abonelik Adı
    * Azure QnA Hizmet Adı
    * Dil - İngilizce
1. Adını `Multi-turn batch test quickstart` bilgi tabanınızın adı olarak girin.

1. **Adım**4'te, ayarları aşağıdaki tabloyla yapılandırın:

    |Ayar|Değer|
    |--|--|
    |**URL'lerden, .pdf veya .docx dosyalarından çoklu dönüş çıkarma olanağı sağlar.**|İşaretli|
    |**Varsayılan yanıt metni**| `Batch test - default answer not found.`|
    |**+ Dosya Ekle**|Ön koşullarda `.docx` indirilen dosya listesini seçin.|
    |**Chit-sohbet**|**Profesyonel'i** Seçin|

1. **Adım 5'te** **KB'nizi oluştur'u**seçin.

    Oluşturma işlemi sona erdiğinde, portal editable bilgi tabanını görüntüler.

## <a name="save-train-and-publish-knowledge-base"></a>Bilgi tabanını kaydedin, eğitin ve yayınlayın

1. Bilgi tabanını kaydetmek için araç çubuğundan **Kaydet'i** seçin ve eğitin.
1. Araç çubuğundan **Yayımla'yı** seçin ve bilgi tabanını yayımlamak için yeniden **Yayımla'yı** seçin. Yayımlama, bilgi tabanını genel bir URL bitiş noktasından gelen sorgular için kullanılabilir hale getirir. Yayımlama tamamlandığında, **Yayımla** sayfasında gösterilen ana bilgisayar URL'sini ve bitiş noktası anahtar bilgilerini kaydedin.

    |Gerekli veriler| Örnek|
    |--|--|
    |Yayınlanan Host|`https://YOUR-RESOURCE-NAME.azurewebsites.net`|
    |Yayınlanan Anahtar|`XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX`(32 karakter dize sonra `Endpoint` gösterilir )|
    |Uygulama Kimliği|`xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx`(36 karakter dize `POST`parçası olarak gösterilir ) |

## <a name="create-batch-test-file-with-question-ids"></a>Soru t,.c.'leri olan toplu test dosyası oluşturma

Toplu iş testi aracını kullanmak için `batch-test-data-1.tsv` metin düzenleyicisi ile birlikte bir dosya oluşturun. Dosyanın aşağıdaki sütunların bir sekme yle ayrılması gerekir.

|TSV giriş dosya alanları|Notlar|Örnek|
|--|--|--|
|Bilgi bankası kimliği|Bilgi temel kimliğiniz Yayımla sayfasında bulunur. Tek bir dosyada farklı bilgi tabanı dislerini kullanarak tek bir dosyada aynı hizmetteki birden çok bilgi tabanını test edin.|`xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx`(36 karakter dize `POST`parçası olarak gösterilir ) |
|Soru|Kullanıcının gireceği soru metni. En fazla 1000 karakter.|`How do I sign out?`|
|Meta veri etiketleri|isteğe bağlı|`topic:power`anahtar _kullanır:değer_ biçimi|
|Üst parametre|isteğe bağlı|`25`|
|Beklenen yanıt kimliği|isteğe bağlı|`13`|

Bu bilgi tabanı için, dosyaya sadece 2 gerekli sütundan 3 satır ekleyin. İlk sütun bilgi temel kimliğinizdir ve ikinci sütun aşağıdaki soru listesi olmalıdır:

|Sütun 2 - sorular|
|--|
|`Use Windows Hello to sign in`|
|`Charge your Surface Pro 4`|
|`Get to know Windows 10`|

Bu sorular bilgi tabanından tam ifadeler ve güven puanı olarak 100 dönmelidir.

Ardından, aynı bilgi temel kimliğini kullanarak, bu sorulara benzer ancak 3 satırda tam olarak aynı olmayan birkaç soru ekleyin:

|Sütun 2 - sorular|
|--|
|`What is Windows Hello?`|
|`How do I charge the laptop?`|
|`What features are in Windows 10?`|

> [!CAUTION]
> Her sütunun yalnızca bir sekme sınırlayıcı yla ayrıldığından emin olun. Satır aralığı veya sondaki boşluklar sütun verilerine eklenir ve tür veya boyut yanlış olduğunda programın özel durumlar atmasını sağlar.

Excel'de açıldığında toplu iş testi dosyası aşağıdaki resme benzer. Bilgi bankası kimliği güvenlik `xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx` için değiştirildi. Kendi toplu iş testiniz için, sütunun bilgi temel kimliğinizi görüntülediğinden emin olun.

> [!div class="mx-imgBorder"]
> ![.tsv dosyasının ilk sürümünü toplu işlemden giriş yapma](../media/batch-test/batch-test-1-input.png)

## <a name="test-the-batch-file"></a>Toplu iş dosyasını test edin

Komut satırında aşağıdaki CLI biçimini kullanarak toplu iş testi programını çalıştırın.

Hizmet `YOUR-RESOURCE-NAME` `ENDPOINT-KEY` adı ve bitiş noktası anahtarı için değiştirin ve kendi değerlerinizi değiştirin. Bu değerler QnA Maker portalındaki **Ayarlar** sayfasında bulunur.

```console
batchtesting.exe batch-test-data-1.tsv https://YOUR-RESOURCE-NAME.azurewebsites.net ENDPOINT-KEY out.tsv
```
Test tamamlar ve dosyayı `out.tsv` oluşturur:

> [!div class="mx-imgBorder"]
> ![.tsv dosyasının toplu testten çıktı ilk sürümü](../media/batch-test/batch-test-1-output.png)

Bilgi bankası kimliği güvenlik `xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx` için değiştirildi. Kendi toplu iş testiniz için sütun bilgi temel kimliğinizi görüntüler.

4. sütundaki güven puanının test çıktısı, her soru bilgi tabanında göründüğü gibi tam olarak aynı olduğundan, en iyi 3 sorunun beklendiği gibi 100 puan geri döndüğünü gösterir. Son 3 soru, soru yeni ifadeler ile, güven puanı olarak 100 dönmeyin. Hem test hem de kullanıcılarınızın puanını artırmak için bilgi tabanına daha fazla alternatif soru eklemeniz gerekir.

## <a name="testing-with-the-optional-fields"></a>İsteğe bağlı alanlar ile test etme

Biçimi ve işlemi anladıktan sonra, sohbet günlükleri gibi bir veri kaynağından bilgi tabanınıza karşı çalışacak bir test dosyası oluşturabilirsiniz.

Veri kaynağı ve işlem otomatik olduğundan, test dosyası doğru değerleri belirlemek için farklı ayarlarla birçok kez çalıştırılabilir.

Örneğin, bir sohbet günlüğünuz varsa ve hangi sohbet günlüğü metninin hangi meta veri alanlarına uygulanabilen bir test dosyası oluşturmak ve her satır için meta veri alanlarını ayarlamak istiyorsanız. Testi çalıştırın, ardından meta verilerle eşleşen satırları gözden geçirin. Genellikle, eşleşmeler pozitif olmalıdır, ancak yanlış pozitif sonuçları gözden geçirmelisiniz. Yanlış pozitif, meta verilerle eşleşen bir satırdır, ancak metne göre eşleşmemelidir.

## <a name="using-optional-fields-in-the-input-batch-test-file"></a>Giriş toplu test dosyasında isteğe bağlı alanları kullanma

İsteğe bağlı veriler için alan değerlerini nasıl bulacağımı anlamak için aşağıdaki grafiği kullanın.

|Sütun numarası|İsteğe bağlı sütun|Veri konumu|
|--|--|--|
|3|meta veriler|Varolan _anahtar:değer_ çiftleri için varolan bilgi tabanını dışa aktarın.|
|4|üst|Varsayılan değeri `25` önerilir.|
|5|Soru ve cevap seti kimliği|Kimlik değerleri için varolan bilgi tabanını dışa aktarın. Ayrıca, çıktı dosyasında döndürülen t.c.|

## <a name="add-metadata-to-the-knowledge-base"></a>Bilgi tabanına meta veri ekleme

1. QnA portalında, **Edit** sayfasında, aşağıdaki sorulara `topic:power` meta veri ekleyin:

    |Sorular|
    |--|
    |Surface Pro 4'ünüzü şarj edin|
    |Pil seviyesini kontrol edin|

    İki QnA çifti meta veri kümesine sahiptir.

    > [!TIP]
    > Her kümenin meta verilerini ve QnA d'lerini görmek için bilgi tabanını dışa aktarın. **Ayarlar** sayfasını seçin ve ardından `.xls` dosya olarak **Dışa Aktar'ı** seçin. İndirilen bu dosyayı bulun ve Excel'in meta veri ve kimlik incelemesiyle açın.

1. **Kaydet ve eğit'i**seçin, ardından **Yayımla** sayfasını seçin ve ardından **Yayımla** düğmesini seçin. Bu eylemler, değişikliği toplu iş testi için kullanılabilir hale getirin. Bilgi tabanını **Ayarlar** sayfasından indirin.

    İndirilen dosya meta veriler için doğru biçime ve doğru soru ve yanıt kümesi kimliğine sahiptir. Sonraki bölümde bu alanları kullanın

    > [!div class="mx-imgBorder"]
    > ![Meta verilerle dışa aktarılan bilgi tabanı](../media/batch-test/exported-knowledge-base-with-metadata.png)

## <a name="create-a-second-batch-test"></a>İkinci bir toplu iş testi oluşturma

Toplu iş testi için iki ana senaryo vardır:
* **İşlem sohbet günlüğü dosyaları** - Daha önce görülmemiş bir soru için en iyi yanıtı belirleyin - en yaygın durum, bir sohbet botunun kullanıcı soruları gibi sorguların günlük dosyası nın işlenmesi gereken durumdur. Yalnızca gerekli sütunları içeren bir toplu iş dosyası testi oluşturun. Test, her sorunun en iyi yanıtını döndürür. Bu, en iyi cevabın doğru cevap olduğu anlamına gelmez. Bu testi tamamladıktan sonra doğrulama testine geçin.
* **Doğrulama testi** - Beklenen yanıtı doğrulayın. Bu test, toplu iş testindeki tüm soruların ve eşleşen beklenen yanıtların doğrulanmış olması gerekir. Bu bazı el ile işlem gerektirebilir.

Aşağıdaki yordam, senaryonun sohbet günlüklerini

1. İsteğe bağlı verileri eklemek için `batch-test-data-2.tsv`yeni bir toplu iş testi dosyası oluşturun. Özgün toplu iş testi giriş dosyasından 6 satır ekleyin, ardından her satır için meta veri, üst ve QnA çifti kimliği ekleyin.

    Sohbet günlüklerinden yeni metni bilgi tabanına göre denetleme işlemini simüle etmek için, her `topic:power`sütun için meta verileri aynı değere ayarlayın: .

    > [!div class="mx-imgBorder"]
    > ![.tsv dosyasının ikinci sürümünü toplu testten giriş](../media/batch-test/batch-test-2-input.png)

1. İkinci test olduğunu belirtmek için giriş ve çıktı dosya adlarını değiştirerek testi yeniden çalıştırın.

    > [!div class="mx-imgBorder"]
    > ![.tsv dosyasının toplu testten çıktı ikinci sürümü](../media/batch-test/batch-test-2-output.png)

## <a name="test-results-and-an-automated-test-system"></a>Test sonuçları ve otomatik test sistemi

Bu test çıktısı dosyası, otomatik bir sürekli test ardışık hattının parçası olarak ayrıştırılabilir.

Bu özel test çıktısı şu şekilde okunmalı: her satır meta verilerle filtrelendi ve her satır bilgi tabanındaki meta verilerle eşleşmediği için, döndürülen eşleşmeyen satırlar için varsayılan yanıt ("kb'de iyi eşleşme bulunamadı"). Eşleşen bu satırlar, QnA KIMLIĞI ve puanı döndürüldü.

Hiçbir satır beklenen yanıt kimliğiyle eşleşmediği için tüm satırlar yanlış etiketini döndürür.

Bu sonuçlarla sohbet günlüğü alıp her satırın sorgusu olarak metni kullanabileceğinizi görebilmelisiniz. Veriler hakkında hiçbir şey bilmeden, sonuçlar size daha sonra ileriye taşımak için kullanabileceğiniz veriler hakkında çok şey anlatır:

* meta-veri
* QnA Kimliği
* puan

Meta verilerle filtreleme test için iyi bir fikir miydi? Evet ve hayır. Test sistemi, her meta veri çifti için test dosyalarının yanı sıra meta veri çifti olmayan bir test oluşturmalıdır.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bilgi tabanını test etmeye devam etmeyecekseniz, toplu dosya aracını ve test dosyalarını silin.

Bu bilgi tabanını kullanmaya devam etmeyecekseniz, aşağıdaki adımlarla bilgi tabanını silin:

1. QnA Maker portalında, üst menüden **Bilgim tabanlarını** seçin.
1. Bilgi tabanları listesinde, bu hızlı başlatmanın bilgi tabanının satırındaki **Sil** simgesini seçin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Soru-Cevap Oluşturma (V4) REST API Başvurusu](https://go.microsoft.com/fwlink/?linkid=2092179)
