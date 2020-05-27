---
title: 'Hızlı başlangıç: toplu iş soruları ile test bilgi tabanı'
description: Beklenen yanıtlar, güvenirlik puanları ve çok yönlü istemler için Soru-Cevap Oluşturma kaynaklarınızın bilgi temellerini test etmek üzere Soru-Cevap Oluşturma Batch test aracını kullanın.
ms.topic: quickstart
ms.date: 05/26/2020
ms.openlocfilehash: 9845b7c7cc19550c450a1eb00ec02731ef2d9d44
ms.sourcegitcommit: 64fc70f6c145e14d605db0c2a0f407b72401f5eb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2020
ms.locfileid: "83873670"
---
# <a name="quickstart-test-knowledge-base-with-batch-questions-and-expected-answers"></a>Hızlı başlangıç: toplu sorularla ve beklenen yanıtlardan test Bilgi Bankası

Beklenen yanıtlar, güvenirlik puanları ve çok yönlü istemler için Soru-Cevap Oluşturma kaynaklarınızın bilgi temellerini test etmek üzere Soru-Cevap Oluşturma Batch test aracını kullanın.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
* [Soru-cevap oluşturma bir hizmet oluşturun](create-publish-knowledge-base.md) ya da İngilizce dilini kullanan mevcut bir hizmeti kullanın.
* [Çoklu çift örnek `.docx` dosyasını](https://github.com/Azure-Samples/cognitive-services-sample-data-files/blob/master/qna-maker/data-source-formats/multi-turn.docx) indirin
* [Batch test aracı](https://aka.ms/qnamakerbatchtestingtool)'nı indirin, dosyadan yürütülebilir dosyayı ayıklayın `.zip` .

## <a name="sign-into-qna-maker-portal"></a>Soru-Cevap Oluşturma portalında oturum açın

Soru-Cevap Oluşturma portalında [oturum açın](https://www.qnamaker.ai/) .

## <a name="create-a-new-knowledge-base-from-the-multi-turn-sampledocx-file"></a>Çoklu çift örnek. docx dosyasından yeni bir Bilgi Bankası oluşturun

1. Araç çubuğundan **Bilgi Bankası oluştur** ' u seçin.
1. **1. adımı** atlayın, var olan kaynak bilgilerinizi seçmek için **Adım 2** ' ye kadar bir soru-cevap oluşturma kaynağınız olması gerekir:
    * Azure Active Directory KIMLIĞI
    * Azure abonelik adı
    * Azure QnA hizmeti adı
    * Dil-Ingilizce dili
1. `Multi-turn batch test quickstart`Bilgi Bankalarınızın adı olarak adı girin.

1. **4. adımda**ayarları aşağıdaki tabloyla yapılandırın:

    |Ayar|Değer|
    |--|--|
    |**URL 'Ler,. PDF veya. docx dosyalarından Çoklu açma ayıklamasını etkinleştirin.**|İşaretli|
    |**Varsayılan yanıt metni**| `Batch test - default answer not found.`|
    |**+ Dosya Ekle**|Ön koşullarda indirilen `.docx` dosya listesini seçin.|
    |**Chit-sohbet**|**Profesyonel** seçin|

1. **5. adımda**, **KB 'nizi oluştur**' u seçin.

    Oluşturma işlemi tamamlandığında Portal, düzenlenebilir Bilgi Bankası 'nı görüntüler.

## <a name="save-train-and-publish-knowledge-base"></a>Bilgi Bankası 'nı kaydetme, eğitme ve yayımlama

1. Bilgi Bankası 'nı kaydetmek için araç çubuğundan **Kaydet ve eğitme '** yi seçin.
1. Araç çubuğundan **Yayımla** ' yı seçin ve ardından Bilgi Bankası 'nı yayımlamak Için yeniden **Yayımla** ' yı seçin. Yayımlama, bilgi bankasını ortak bir URL uç noktasından sorgular için kullanılabilir hale getirir. Yayımlama tamamlandığında, **Yayımla** sayfasında gösterilen ana bilgisayar URL 'sini ve uç nokta anahtarı bilgilerini kaydedin.

    |Gerekli veriler| Örnek|
    |--|--|
    |Yayınlanan konak|`https://YOUR-RESOURCE-NAME.azurewebsites.net`|
    |Yayınlanan anahtar|`XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX`(32 karakter dizesi sonra gösteriliyor `Endpoint` )|
    |Uygulama Kimliği|`xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx`(36 bir parçası olarak gösterilen karakter dizesi `POST` ) |

## <a name="create-batch-test-file-with-question-ids"></a>Soru kimlikleriyle Batch test dosyası oluşturma

Batch test aracını kullanmak için metin düzenleyici ile adlı bir dosya oluşturun `batch-test-data-1.tsv` . Dosyanın aşağıdaki sütunları bir sekmeyle ayrılmış olması gerekir.

|TSV giriş dosyası alanları|Notlar|Örnek|
|--|--|--|
|Bilgi Bankası KIMLIĞI|Bilgi Bankası KIMLIĞINIZ Yayımla sayfasında bulunur. Tek bir dosyada farklı Bilgi Bankası kimliklerini kullanarak tek bir dosyada aynı hizmette bulunan birçok bilgi bankasını test edin.|`xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx`(36 bir parçası olarak gösterilen karakter dizesi `POST` ) |
|Soru|Kullanıcının girebileceği soru metni. en fazla 1.000 karakter.|`How do I sign out?`|
|Meta veri etiketleri|isteğe bağlı|`topic:power`_anahtar: değer_ biçimini kullanır|
|Üst parametre|isteğe bağlı|`25`|
|Beklenen yanıt KIMLIĞI|isteğe bağlı|`13`|

Bu Bilgi Bankası için, dosyaya yalnızca 2 gerekli sütun 3 satırı ekleyin. İlk sütun Bilgi Bankası KIMLIĞINIZ ve ikinci sütun aşağıdaki soru listesi olmalıdır:

|Sütun 2-sorular|
|--|
|`Use Windows Hello to sign in`|
|`Charge your Surface Pro 4`|
|`Get to know Windows 10`|

Bu sorular bilgi bankasındaki tam bir ifade olup, Güvenirlik puanı olarak 100 döndürmelidir.

Daha sonra, aynı Bilgi Bankası KIMLIĞI kullanılarak bu sorulara benzer ancak 3 daha fazla satırda tam olarak aynı olan birkaç soru ekleyin:

|Sütun 2-sorular|
|--|
|`What is Windows Hello?`|
|`How do I charge the laptop?`|
|`What features are in Windows 10?`|

> [!CAUTION]
> Her sütunun yalnızca bir sekme sınırlayıcısıyla ayrıldığından emin olun. Sütun verilerine baştaki veya sondaki boşluklar eklenir ve tür veya boyut yanlış olduğunda programın özel durum oluşturmasına neden olur.

Excel 'de açıldığında, toplu işlem test dosyası aşağıdaki görüntüde gibi görünür. Bilgi Bankası KIMLIĞI `xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx` güvenlik için değiştirilmiştir. Kendi Batch testiniz için, sütunun Bilgi Bankası KIMLIĞINIZI görüntülediğinden emin olun.

> [!div class="mx-imgBorder"]
> ![Toplu iş testinde. tsv dosyasının ilk sürümünü girin](../media/batch-test/batch-test-1-input.png)

## <a name="test-the-batch-file"></a>Toplu iş dosyasını test etme

Komut satırında aşağıdaki CLı biçimini kullanarak Batch test programı 'nı çalıştırın.

`YOUR-RESOURCE-NAME`Ve `ENDPOINT-KEY` değerlerini hizmet adı ve uç nokta anahtarı için kendi değerlerinizle değiştirin. Bu değerler Soru-Cevap Oluşturma portalındaki **Ayarlar** sayfasında bulunur.

```console
batchtesting.exe batch-test-data-1.tsv https://YOUR-RESOURCE-NAME.azurewebsites.net ENDPOINT-KEY out.tsv
```
Test tamamlanır ve `out.tsv` dosyayı oluşturur:

> [!div class="mx-imgBorder"]
> ![Batch testinde. tsv dosyasının ilk sürümünü çıkar](../media/batch-test/batch-test-1-output.png)

Bilgi Bankası KIMLIĞI `xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx` güvenlik için değiştirilmiştir. Kendi Batch testiniz için, sütun Bilgi Bankası KIMLIĞINIZI görüntüler.

Her soru Bilgi Bankası 'nda göründüğü gibi tam olarak aynı olduğu için, 4 sütunundaki Güvenirlik puanı test çıktısı, en çok 100 puanı döndüren ilk 3 soruyu gösterir. Son 3 soru, söz konusu sorudan yeni bir deyişle, Güvenirlik puanı olarak 100 ' i döndürmeyin. Hem test hem de kullanıcılarınız için puanı artırmak üzere Bilgi Bankası 'na daha fazla başka soru eklemeniz gerekir.

## <a name="testing-with-the-optional-fields"></a>İsteğe bağlı alanlarla test etme

Biçim ve işlemi anladıktan sonra, sohbet günlüklerinden gibi bir veri kaynağından bilgi tabanınızdan çalıştırmak için bir test dosyası oluşturabilirsiniz.

Veri kaynağı ve işlemi otomatikleştiriltiğinden, doğru değerleri belirleyebilmek için test dosyası farklı ayarlarla birçok kez çalıştırılabilir.

Örneğin, bir sohbet günlüğü varsa ve hangi sohbet günlüğü metninin hangi meta veri alanlarına uygulanacağını öğrenmek istiyorsanız, bir test dosyası oluşturun ve her satır için meta veri alanlarını ayarlayın. Testi çalıştırın, sonra meta verilerle eşleşen satırları gözden geçirin. Genellikle eşleşmeler pozitif olmalıdır, ancak hatalı pozitif sonuçlar için sonuçları gözden geçirmeniz gerekir. Yanlış pozitif değeri, meta verilerle eşleşen ancak metne göre eşleşmeyen bir satırdır.

## <a name="using-optional-fields-in-the-input-batch-test-file"></a>Giriş toplu işlem test dosyasında isteğe bağlı alanlar kullanma

İsteğe bağlı veriler için alan değerlerinin nasıl bulunacağını anlamak için aşağıdaki grafiği kullanın.

|Sütun numarası|İsteğe bağlı sütun|Veri konumu|
|--|--|--|
|3|meta veriler|Mevcut _anahtar: değer_ çiftleri için mevcut bilgi bankasını dışarı aktarın.|
|4|üst|Varsayılan değeri `25` önerilir.|
|5|Soru ve yanıt kümesi KIMLIĞI|KIMLIK değerleri için mevcut bilgi bankasını dışarı aktarın. Ayrıca, kimlikler çıkış dosyasında döndürülmediğine dikkat edin.|

## <a name="add-metadata-to-the-knowledge-base"></a>Bilgi Bankası 'na meta veri ekleme

1. QnA portalında, **düzenleme** sayfasında, aşağıdaki sorulara meta verileri ekleyin `topic:power` :

    |Sorular|
    |--|
    |Surface Pro 4 ' ü ücretlendirin|
    |Pil düzeyini denetleme|

    İki QnA çiftinin meta veri kümesi var.

    > [!TIP]
    > Her bir küme için meta verileri ve QnA kimliklerini görmek üzere Bilgi Bankası 'nı dışarı aktarın. **Ayarlar** sayfasını seçin ve ardından dosya olarak **Dışa Aktar** ' ı seçin `.xls` . Bu indirilen dosyayı bulun ve meta veriler ve KIMLIK için Excel gözden geçirimiyle açın.

1. **Kaydet ve eğit**' i seçin, ardından **Yayımla** sayfasını seçin ve ardından **Yayınla** düğmesini seçin. Bu eylemler, değişikliği toplu test için kullanılabilir hale getirir. Bilgi Bankası 'nı **Ayarlar** sayfasından indirin.

    İndirilen dosyanın meta veriler için doğru biçimi ve doğru soru ve yanıt kümesi KIMLIĞI vardır. Sonraki bölümde bu alanları kullan

    > [!div class="mx-imgBorder"]
    > ![Meta verilerle dışarıya aktarılmış bilgi tabanı](../media/batch-test/exported-knowledge-base-with-metadata.png)

## <a name="create-a-second-batch-test"></a>İkinci bir Batch testi oluşturma

Toplu test için iki ana senaryo vardır:
* **Sohbet günlüğü dosyalarını işle** -daha önce görülmeyen bir soru için en iyi yanıtı belirleme-en yaygın durum, bir sohbet bot 'un Kullanıcı soruları gibi, sorguların günlük dosyası olan bir işlemdir. Yalnızca gerekli sütunlarla bir toplu iş dosyası testi oluşturun. Test, her soru için en iyi yanıtı döndürür. Bu, en iyi yanıtın doğru yanıt olduğu anlamına gelmez. Bu testi tamamladıktan sonra doğrulama testine geçin.
* **Doğrulama testi** -beklenen yanıtı doğrulayın. Bu test, toplu iş testinde tüm soruların ve beklenen yanıtları eşleştirmesinin doğrulanmasını gerektirir. Bu, bazı el ile işlem gerektirebilir.

Aşağıdaki yordamda, senaryonun sohbet günlüklerini işlemek için olduğu varsayılmaktadır

1. İsteğe bağlı verileri dahil etmek için yeni bir Batch test dosyası oluşturun `batch-test-data-2.tsv` . Orijinal Batch testi giriş dosyasından 6 satır ekleyin, ardından her satır için meta veri, üst ve QnA çifti KIMLIĞINI ekleyin.

    Bilgi Bankası 'nda sohbet günlüklerinden yeni metin denetleme işleminin benzetimini yapmak için, her bir sütunun meta verilerini aynı değere ayarlayın: `topic:power` .

    > [!div class="mx-imgBorder"]
    > ![Batch testinde. tsv dosyasının ikinci sürümünü girin](../media/batch-test/batch-test-2-input.png)

1. Testi yeniden çalıştırın, giriş ve çıkış dosyası adlarını değiştirerek ikinci test olduğunu belirtin.

    > [!div class="mx-imgBorder"]
    > ![Batch testinde. tsv dosyasının ikinci sürümünü çıktı](../media/batch-test/batch-test-2-output.png)

## <a name="test-results-and-an-automated-test-system"></a>Test sonuçları ve otomatikleştirilmiş bir test sistemi

Bu test çıkış dosyası otomatik bir sürekli test işlem hattının parçası olarak ayrıştırılabilir.

Bu özel test çıkışı şu şekilde okunmalıdır: her satır meta verilerle filtrelenmiştir ve her satır bilgi bankasındaki meta verilerle eşleşmediğinden, bu eşleşmeyen satırların varsayılan yanıtı ("KB 'de iyi eşleşme bulunamadı") döndürülür. Eşleşen bu satırlardan, QnA KIMLIĞI ve puanı döndürüldü.

Beklenen yanıt KIMLIĞIYLE eşleşen satır olmadığından tüm satırlar yanlış etiketini döndürdü.

Bu sonuçlarla, sohbet günlüğü uygulayabileceğiniz ve metni her bir satırın sorgusu olarak kullanabileceğiniz şekilde görebilmelisiniz. Veriler hakkında herhangi bir şey bilmeden sonuçlar, daha sonra ilerlemeden daha fazla bilgi elde etmek için size çok fazla bilgi bildirir:

* meta veriler
* QnA KIMLIĞI
* puan

Test için tam bir fikir olan meta verilerle filtreleniyor mu? Evet ve hayır. Test sistemi her meta veri çiftinin test dosyalarını ve meta veri çiftlerine sahip olmayan bir testi oluşturacaktır.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bilgi bankasını sınamaya devam edemeyecekleriniz, toplu iş dosyası aracını ve test dosyalarını silin.

Bu Bilgi Bankası 'nı kullanmaya devam edemeyecekleriniz, Bilgi Bankası 'nı aşağıdaki adımlarla silin:

1. Soru-Cevap Oluşturma portalında, üst menüden **bilgi tabanlarım** ' ı seçin.
1. Bilgi tabanı listesinde, bu hızlı başlangıçta Bilgi Bankası 'nın satırındaki **Sil** simgesini seçin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Soru-Cevap Oluşturma (V4) REST API Başvurusu](https://go.microsoft.com/fwlink/?linkid=2092179)
