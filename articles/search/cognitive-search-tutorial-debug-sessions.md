---
title: 'Öğretici: beceri kodunuzda yapılan değişiklikleri tanılamak, gidermek ve işlemek için hata ayıklama oturumlarını kullanma'
titleSuffix: Azure Cognitive Search
description: Hata ayıklama oturumları (Önizleme) becerileri bilgisayarınızdaki sorunları/hataları değerlendirmek ve onarmak için portal tabanlı bir arabirim sağlar
author: tchristiani
ms.author: terrychr
manager: nitinme
ms.service: cognitive-search
ms.topic: tutorial
ms.date: 09/25/2020
ms.openlocfilehash: 8bbd0b1979da69e5d4d18009100a7caee5a3d722
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91397411"
---
# <a name="tutorial-diagnose-repair-and-commit-changes-to-your-skillset"></a>Öğretici: beceri kodunuzda tanılama, onarma ve değişiklikleri yapma

Bu makalede, belirtilen beceri sorunlarını gidermek için hata ayıklama oturumlarına erişmek üzere Azure portal kullanacaksınız. Beceri, giderilmesi gereken bazı hatalara sahiptir. Bu öğretici, yetenek girişleri ve çıkışları sorunlarını belirlemek ve çözmek için bir hata ayıklama oturumunda size kılavuzluk eder.

> [!Important]
> Hata ayıklama oturumları, bir hizmet düzeyi sözleşmesi olmadan sunulan bir önizleme özelliğidir ve üretim iş yükleri için önerilmez. Daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
>

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

> [!div class="checklist"]
> * Azure aboneliği. Ücretsiz bir [Hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun veya geçerli aboneliğinizi kullanın
> * Azure Bilişsel Arama hizmet örneği
> * Bir Azure depolama hesabı
> * [Postman masaüstü uygulaması](https://www.getpostman.com/)

## <a name="create-services-and-load-data"></a>Hizmet oluşturma ve veri yükleme

Bu öğretici Azure Bilişsel Arama ve Azure depolama hizmetlerini kullanır.

* 19 dosyadan oluşan [örnek verileri indirin](https://github.com/Azure-Samples/azure-search-sample-data/tree/master/clinical-trials-pdf-19) .

* [Bir Azure depolama hesabı oluşturun](../storage/common/storage-account-create.md?tabs=azure-portal) veya [var olan bir hesabı bulun](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Storage%2storageAccounts/). 

   Bant genişliği ücretlerinden kaçınmak için Azure Bilişsel Arama ile aynı bölgeyi seçin.
   
   StorageV2 (genel amaçlı v2) hesap türünü seçin.

* Depolama Hizmetleri sayfalarını açın ve bir kapsayıcı oluşturun. En iyi yöntem, "özel" erişim düzeyini belirtmektir. Kapsayıcınızı adlandırın `clinicaltrialdataset` .

* Kapsayıcıda, ilk adımda indirdiğiniz ve daraltılmış örnek dosyaları karşıya yüklemek için **Yükle** ' ye tıklayın.

* [Bir Azure bilişsel arama hizmeti oluşturun](search-create-service-portal.md) veya [var olan bir hizmeti bulun](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices). Bu hızlı başlangıç için ücretsiz bir hizmet kullanabilirsiniz.

## <a name="get-a-key-and-url"></a>Anahtar ve URL al

REST çağrıları için her istekte hizmet URL'sinin ve bir erişim anahtarının iletilmesi gerekir. Her ikisiyle de bir arama hizmeti oluşturulur. bu nedenle, aboneliğinize Azure Bilişsel Arama eklediyseniz, gerekli bilgileri almak için aşağıdaki adımları izleyin:

1. [Azure Portal oturum açın](https://portal.azure.com/)ve arama hizmetine **genel bakış** sayfasında URL 'yi alın. Örnek uç nokta `https://mydemo.search.windows.net` şeklinde görünebilir.

1. **Ayarlar**  >  **anahtarlar**' da, hizmette tam haklar için bir yönetici anahtarı alın. Üzerinde bir tane almanız gereken iş sürekliliği için iki adet değiştirilebilir yönetici anahtarı vardır. Nesneleri eklemek, değiştirmek ve silmek için isteklerde birincil veya ikincil anahtarı kullanabilirsiniz.

:::image type="content" source="media/search-get-started-postman/get-url-key.png" alt-text="HTTP uç noktası ve erişim anahtarı al" border="false":::

Tüm istekler hizmetinize gönderilen her istekte bir API anahtarı gerektirir. İstek başına geçerli bir anahtara sahip olmak, isteği gönderen uygulama ve bunu işleyen hizmet arasında güven oluşturur.

## <a name="create-data-source-skillset-index-and-indexer"></a>Veri kaynağı, Beceri, dizin ve Dizin Oluşturucu oluşturma

Bu bölümde, arama hizmetinin veri kaynağını, Beceri, dizinini ve Dizin oluşturucuyu oluşturmak için Postman ve sağlanmış bir koleksiyon kullanılır.

1. Postman yoksa [Postman Desktop uygulamasını buradan indirebilirsiniz](https://www.getpostman.com/).
1. [Hata ayıklama oturumları Postman koleksiyonunu indirin](https://github.com/Azure-Samples/azure-search-postman-samples/tree/master/Debug-sessions)
1. Postman Başlat
1. Yeni **dosyalar**altında  >  **New**, içeri aktarılacak koleksiyonu seçin.
1. Koleksiyon alındıktan sonra, eylemler listesini (...) genişletin.
1. **Düzenle**’ye tıklayın.
1. SearchService 'nizin adını girin (örneğin, uç nokta ise, `https://mydemo.search.windows.net` hizmet adı " `mydemo` ").
1. Ara hizmetinizin birincil veya ikincil anahtarıyla apiKey değerini girin.
1. Azure depolama hesabınızın anahtarlar sayfasından storageConnectionString öğesini girin.
1. Depolama hesabında oluşturduğunuz kapsayıcının Kapsayıcıadı ' nı girin.

> :::image type="content" source="media/cognitive-search-debug/postman-enter-variables.png" alt-text="HTTP uç noktası ve erişim anahtarı al":::

Koleksiyon, bu bölümü gerçekleştirmek için kullanılan dört farklı REST çağrısı içerir.

İlk çağrı veri kaynağını oluşturur. `clinical-trials-ds`. İkinci çağrı, Beceri oluşturur `clinical-trials-ss` . Üçüncü çağrı dizinini oluşturur `clinical-trials` . Dördüncü ve son çağrı Dizin oluşturucuyu oluşturur `clinical-trials-idxr` . Koleksiyondaki tüm çağrılar tamamlandıktan sonra Postman ' ı kapatıp Azure portal döndürün.

> :::image type="content" source="media/cognitive-search-debug/postman-create-data-source.png" alt-text="HTTP uç noktası ve erişim anahtarı al":::

## <a name="check-the-results"></a>Sonuçları denetleyin

Beceri, yaygın olarak karşılaşılan birkaç hata içerir. Bu bölümde, tüm belgeleri döndürmek için boş bir sorgu çalıştırıldığında birden çok hata görüntülenir. Sonraki adımlarda, sorunlar bir hata ayıklama oturumu kullanılarak çözümlenir.

1. Azure portal arama hizmetinize gidin. 
1. **Dizinler** sekmesini seçin. 
1. Dizini seçin `clinical-trials`
1. Boş bir sorgu çalıştırmak için **Ara** ' ya tıklayın. 

Arama tamamlandıktan sonra, veri listelenmesiz iki alan; "kuruluşlar" ve "konumlar" pencerede listelenmiştir. Beceri tarafından üretilen tüm sorunları bulmaya yönelik adımları izleyin.

1. Arama hizmeti genel bakış sayfasına dönün.
1. **Dizin oluşturucular** sekmesini seçin. 
1. Tıklayın `clinical-trials-idxr` ve uyarılar bildirimini seçin. 

Bir veya daha fazla yetenek girişi geçersiz olduğundan, yansıtma çıktı alanı eşlemelerinde ve üçüncü sayfada üç sorun vardır.

Arama hizmeti genel bakış ekranına dönün.

## <a name="start-your-debug-session"></a>Hata ayıklama oturumunuzu başlatın

> :::image type="content" source="media/cognitive-search-debug/new-debug-session-screen-required.png" alt-text="HTTP uç noktası ve erişim anahtarı al":::

1. Hata ayıklama oturumları (Önizleme) sekmesine tıklayın.
1. + NewDebugSession seçin
1. Oturuma bir ad verin. 
1. Oturumu depolama hesabınıza bağlayın. 
1. Dizin Oluşturucu adını sağlayın. Dizin oluşturucunun veri kaynağına, beceri ve dizine başvuruları vardır.
1. Koleksiyondaki ilk belge için varsayılan belge seçimini kabul edin. 
1. Oturumu **kaydedin** . Oturumun kaydedilmesi, beceri tarafından tanımlanan AI zenginleştirme işlem hattını başlatabilir.

> [!Important]
> Bir hata ayıklama oturumu yalnızca tek bir belge ile kullanılabilir. Veri kümesindeki belirli bir belge > seçilebilir veya oturum, varsayılan olarak ilk belgeye ayarlanır.

> :::image type="content" source="media/cognitive-search-debug/debug-execution-complete1.png" alt-text="HTTP uç noktası ve erişim anahtarı al":::

Hata ayıklama oturumunun yürütülmesi tamamlandığında, oturum varsayılan olarak, Yetenek grafiğini vurgulayarak AI zenginleştirme sekmesine göre yapılır.

+ Beceri grafiği, Beceri görsel hiyerarşisini ve en üstten en alta yürütme sırasını sağlar. Grafikte yan yana olan yetenekler paralel olarak yürütülür. Grafikteki yeteneklerin renk kodlaması, Beceri içinde yürütülen yetenek türlerini gösterir. Örnekte yeşil yetenekler metin ve kırmızı beceri vizyonu. Grafikteki tek bir yeteneğe tıkladığınızda, oturum penceresinin sağ bölmesinde beceri örneğinin ayrıntıları görüntülenir. Beceri ayarları, bir JSON Düzenleyicisi, yürütme ayrıntıları ve hataların/uyarıların tümü İnceleme ve düzenleme için kullanılabilir.
+ Zenginleştirilmiş veri yapısı, kaynak belgenin içeriğinden yetenekler tarafından oluşturulan zenginleştirme ağacındaki düğümlerin ayrıntılarını oluşturur.

Hatalar/Uyarılar sekmesi, daha önce görüntülenenden daha küçük bir liste sağlar, çünkü bu liste yalnızca tek bir belge için hataları ayrıntılendirilir. Dizin Oluşturucu tarafından gösterildiği gibi, bir uyarı iletisine tıklayabilir ve bu uyarının ayrıntılarına bakabilirsiniz.

## <a name="fix-missing-skill-input-value"></a>Eksik yetenek girişi değerini düzeltir

Hatalar/uyarılar sekmesinde, etiketli bir işlem için hata vardır `Enrichment.NerSkillV2.#1` . Bu hatanın ayrıntıları, '/document/languageCode ' yetenek girişi değeriyle ilgili bir sorun olduğunu açıklamaktadır. 

1. AI zenginleştirme sekmesine dönün.
1. **Beceri grafiğine**tıklayın.
1. Ayrıntılarını sağ bölmede göstermek için #1 etiketli yeteneğe tıklayın.
1. "LanguageCode" girdisini bulun.
1. **</>** Satırın başındaki simgeyi seçin ve Ifade değerlendirici ' ni açın.
1. Bu ifadenin bir hataya neden olduğunu onaylamak için **değerlendir** düğmesine tıklayın. "LanguageCode" özelliğinin geçerli bir giriş olduğunu doğrulayacaktır.

> :::image type="content" source="media/cognitive-search-debug/expression-evaluator-language.png" alt-text="HTTP uç noktası ve erişim anahtarı al" öğesine karşılık gelen eşlenmiş yol yok.

> :::image type="content" source="media/cognitive-search-debug/enriched-data-structure-language.png" alt-text="HTTP uç noktası ve erişim anahtarı al" etiketli hatanın gitti olduğunu gösterir. Ancak, hizmetin, kuruluşların ve konumların çıkış alanlarını arama dizinine eşleyemediğinden oluşan iki uyarı hala vardır. Eksik değerler var: '/Document/merged_content/kuruluşlar ' ve '/Document/merged_content/Locations '.

## <a name="fix-missing-skill-output-values"></a>Eksik yetenek çıkış değerlerini düzeltir

> :::image type="content" source="media/cognitive-search-debug/warnings-missing-value-locations-organizations.png" alt-text="HTTP uç noktası ve erişim anahtarı al":::

Bir yeteneğin eksik çıkış değerleri var. Beceriyle ilgili hatayı belirlemek için zenginleştirilmiş veri yapısına gidin, değer adını bulun ve kaynak kaynağına bakın. Eksik kuruluşlar ve konumlar değerleri söz konusu olduğunda, Beceri #1 çıktılardır. Her yol için </> Ifade Değerlendiricisi açmak, sırasıyla '/Document/Content/organizasyonlar ' ve '/Document/Content/Locations ' olarak listelenen ifadeleri görüntüler.

> :::image type="content" source="media/cognitive-search-debug/expression-eval-missing-value-locations-organizations.png" alt-text="HTTP uç noktası ve erişim anahtarı al" girişi Için Ifade değerlendirici ' ni açın.

> :::image type="content" source="media/cognitive-search-debug/input-skill-missing-value-locations-organizations.png" alt-text="HTTP uç noktası ve erişim anahtarı al" çıktıları için ifade değerlendirici ' ni açın.

> :::image type="content" source="media/cognitive-search-debug/merge-output-detail-missing-value-locations-organizations.png" alt-text="HTTP uç noktası ve erişim anahtarı al" varlığı Için Ifade değerlendirici ' ni açın.

> :::image type="content" source="media/cognitive-search-debug/skill-output-detail-missing-value-locations-organizations.png" alt-text="HTTP uç noktası ve erişim anahtarı al" ayarına çift tıklayın ve '/Document/merged_content ' okumak için düzenleyin.
1. Sağ, yetenek ayrıntıları bölmesinde **Kaydet** ' e tıklayın.
1. Oturumlar penceresi menüsünde **Çalıştır** ' a tıklayın. Bu, belgeyi kullanarak beceri 'in başka bir yürütmesini de başlatabilir.

> :::image type="content" source="media/cognitive-search-debug/skill-setting-context-correction-missing-value-locations-organizations.png" alt-text="HTTP uç noktası ve erişim anahtarı al":::

Tüm hatalar çözüldü.

## <a name="commit-changes-to-the-skillset"></a>Değişiklikleri beceri 'e Kaydet

Hata ayıklama oturumu başlatıldığında, arama hizmeti beceri bir kopyasını oluşturmuştur. Bu, yapılan değişiklikler üretim sisteminin etkilenmemesi için yapılmıştır. Beceri hata ayıklamayı bitirdiğiniz için, düzeltmeler üretim sistemine (orijinal beceri üzerine yazabilir) uygulanabilir. Üretim sistemini etkilemeden beceri üzerinde değişiklik yapmaya devam etmek istiyorsanız, hata ayıklama oturumu daha sonra kaydedebilir ve yeniden açılabilir.

1. Ana hata ayıklama oturumları menüsünde **değişiklikleri Yürüt** ' e tıklayın.
1. Beceri hesabınızı güncelleştirmek istediğinizi onaylamak için **Tamam** ' ı tıklatın.
1. Hata ayıklama oturumunu kapatın ve **Dizin oluşturucular** sekmesini seçin.
1. ' Klinik-deneme-ıdxr ' dosyanızı açın.
1. **Sıfırla**' ya tıklayın.
1. **Çalıştır**'a tıklayın. Onaylamak için **Tamam** ' ı tıklatın.

Dizin Oluşturucu çalışmayı bitirdiğinde, yürütme geçmişi sekmesinde en son çalıştırma için zaman damgasının yanında yeşil onay işareti ve başarı sözcüğü gerekir. Değişikliklerin uygulandığından emin olmak için:

1. **Dizin oluşturucudan** çıkın ve **Dizin** sekmesini seçin.
1. ' Klinik-denemeler ' dizinini açın ve arama Gezgini sekmesinde **Ara**' ya tıklayın.
1. Sonuç penceresi, kuruluşların ve konumların varlıklarının artık beklenen değerlerle doldurulduğunu göstermelidir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Kendi aboneliğinizde çalışırken, projenin sonunda oluşturduğunuz kaynaklara hala ihtiyacınız olup olmadığını belirlemek iyi bir fikirdir. Çalışır durumda bırakılan kaynaklar maliyetlerin artmasına neden olabilir. Kaynakları teker teker silebilir veya tüm kaynak grubunu silerek kaynak kümesinin tamamını kaldırabilirsiniz.

Sol gezinti bölmesindeki **tüm kaynaklar** veya **kaynak grupları** bağlantısını kullanarak portalda kaynakları bulabilir ve yönetebilirsiniz.

Ücretsiz bir hizmet kullanıyorsanız, üç Dizin, Dizin Oluşturucu ve veri kaynağı ile sınırlı olduğunu unutmayın. Sınırın altında kalmak için portalda ayrı ayrı öğeleri silebilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> Becerileri hakkında daha [fazla bilgi edinin](./cognitive-search-working-with-skillsets.md) 
>  [Artımlı zenginleştirme ve önbelleğe alma hakkında daha fazla bilgi edinin](./cognitive-search-incremental-indexing-conceptual.md)