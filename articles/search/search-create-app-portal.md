---
title: Azure portal bir arama uygulaması oluşturma
titleSuffix: Azure Cognitive Search
description: İşlemsel bir Web uygulaması için HTML sayfaları ve betiği oluşturmak üzere uygulama oluşturma (Önizleme) Sihirbazı 'nı çalıştırın. Sayfa bir arama çubuğu, sonuç alanı, kenar çubuğu ve typeahead desteği içerir.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: quickstart
ms.date: 12/05/2019
ms.openlocfilehash: 9ed62ea6fdfc0732128ed340cc0bc48481eb73ec
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2020
ms.locfileid: "76841089"
---
# <a name="quickstart-create-a-search-app-in-the-portal-azure-cognitive-search"></a>Hızlı başlangıç: portalda bir arama uygulaması oluşturma (Azure Bilişsel Arama)

Tarayıcıda çalışan indirilebilir, "localhost" stilinde bir Web uygulaması oluşturmak için portalın **arama uygulaması oluşturma** Sihirbazı 'nı kullanın. Yapılandırmaya bağlı olarak, oluşturulan uygulama, uzak bir dizine canlı bağlantı ile ilk kullanımda çalışır. Varsayılan bir uygulama bir arama çubuğu, sonuç alanı, kenar çubuğu filtreleri ve typeahead desteği içerebilir.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun. 

## <a name="prerequisites"></a>Ön koşullar

[En son Microsoft Edge sürümüne](https://www.microsoft.com/edge) yükseltmek veya bu hızlı başlangıç için Google 'ın Chrome tarayıcısını kullanmak isteyeceksiniz.

Geçerli aboneliğinizde [bir Azure bilişsel arama hizmeti oluşturun](search-create-service-portal.md) veya [var olan bir hizmeti bulun](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) . Bu hızlı başlangıç için ücretsiz bir hizmet kullanabilirsiniz. 

Uygulamanızın temeli olarak kullanılacak [bir dizin oluşturun](search-create-index-portal.md) . 

Bu hızlı başlangıç, küçük resim görüntüleri içerdiğinden, yerleşik gerçek emlak örnek verilerini ve dizinini kullanır. Bu alıştırmada kullanılan dizini oluşturmak için, **veri alma** Sihirbazı ' nı çalıştırarak *reatastate-US-Sample* veri kaynağını seçin.

![örnek veriler için veri kaynağı sayfası](media/search-create-app-portal/import-data-realestate.png)

Dizin kullanıma hazırsa, sonraki adıma geçin.

## <a name="start-the-wizard"></a>Sihirbazı Başlat

1. [Azure Portal](https://portal.azure.com) oturum açın ve [arama hizmetinizi bulun](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices).

1. Genel Bakış sayfasında, sayfanın ortasındaki bağlantılardan **dizinler**' i seçin. 

1. Var olan dizinler listesinden *reatastate-US-Sample-Index* öğesini seçin.

1. Sihirbazı başlatmak için üst kısımdaki Dizin sayfasında, **arama uygulaması oluştur (Önizleme)** öğesini seçin.

1. İlk sihirbaz sayfasında, Dizin tanımınıza CORS desteği eklemek için **çapraz kaynak kaynak paylaşımını (CORS) etkinleştir** ' i seçin. Bu adım isteğe bağlıdır, ancak yerel Web uygulamanız bu olmadan uzak dizine bağlanmaz.

## <a name="configure-search-results"></a>Arama sonuçlarını yapılandırma

Sihirbaz, bir küçük resim görüntüsü, başlık ve açıklama için boşluk içeren işlenmiş arama sonuçları için temel bir düzen sağlar. Bu öğelerin her birini yedeklemek, dizininizdeki verileri sağlayan bir alandır. 

1. Küçük resim bölümünde *reatastate-US-Sample* dizinindeki *küçük resim* alanını seçin. Bu örnek, resim küçük resimlerini, *küçük resim*adlı bir alanda depolanan url ile adreslenen görüntüler biçiminde eklemek için oluşur. Dizininizdeki görüntü yoksa, bu alanı boş bırakın.

1. Başlık bölümünde her bir belgenin benzersizlik alanını ileten bir alan seçin. Bu örnekte, liste KIMLIĞI makul bir seçimdir.

1. Açıklama ' da, birisinin belirli bir belgeye tıklamaya karar vermesine yardımcı olabilecek ayrıntılar sağlayan bir alan seçin.

![örnek veriler için veri kaynağı sayfası](media/search-create-app-portal/configure-results.png)

## <a name="add-a-sidebar"></a>Kenar çubuğu ekle

Arama hizmeti, genellikle bir kenar çubuğu olarak işlenen çok yönlü gezintiyi destekler. Modeller, dizin şemasında gösterildiği gibi filtrelenebilir ve çok yönlü tablo alanlarını temel alır.

Azure Bilişsel Arama 'de, çok yönlü gezinme bir toplu filtreleme deneyimidir. Bir kategori içinde birden çok filtre seçildiğinde sonuçlar genişletilir (örneğin, şehir içinde Seattle ve Bellevue ' i seçin). Kategoriler arasında birden çok filtre seçildiğinde sonuçlar daraltır.

> [!TIP]
> Tam dizin şemasını portalda görüntüleyebilirsiniz. Her dizinin Genel Bakış sayfasındaki **Dizin tanımı (JSON)** bağlantısını bulun. Çok yönlü gezintiye uygun olan alanlarda "filtrelenebilir: true" ve "çok yönlü tablo: true" öznitelikleri vardır.

Geçerli model seçimini kabul edin ve sonraki sayfaya devam edin.


## <a name="add-typeahead"></a>Typeahead Ekle

Typeahead işlevselliği otomatik tamamlama ve sorgu önerileri biçiminde kullanılabilir. Sihirbaz sorgu önerilerini destekler. Kullanıcı tarafından sunulan tuş vuruşu girişlerini temel alarak arama hizmeti, giriş olarak seçilebilirler "tamamlanan" sorgu dizelerinin listesini döndürür.

Öneriler, belirli alan tanımlarında etkinleştirilir. Sihirbaz, bir önerisine Ne kadar bilgi ekleneceğini yapılandırmaya yönelik seçenekler sağlar. 

Aşağıdaki ekran görüntüsünde, sihirbazda bulunan işlenmiş bir sayfayla birlikte, sihirbazdaki seçenekler gösterilmektedir. Alan seçimlerinin nasıl kullanıldığını ve öneri içindeki etiketlemeyi dahil etmek veya hariç tutmak için "alan adını göster" seçeneğini görebilirsiniz.

![Sorgu önerisi yapılandırması](media/search-create-app-portal/suggestions.png)

## <a name="create-download-and-execute"></a>Oluşturma, indirme ve yürütme

1. HTML dosyasını oluşturmak için **arama uygulaması oluştur** ' u seçin.

1. İstendiğinde, dosyayı indirmek için **uygulamanızı indir** ' i seçin.

1. Dosyayı açın. Aşağıdaki ekran görüntüsüne benzer bir sayfa görmeniz gerekir. Bir terim girin ve sonuçları daraltmak için filtreleri kullanın. 

Temel alınan dizin, belgeler arasında çoğaltılan kurgusal, oluşturulan verilerden oluşur ve açıklamalar bazen görüntüyle eşleşmez. Kendi dizinlerinizi temel alan bir uygulama oluştururken daha açık bir deneyim de bekleyebilir.

![Uygulamayı çalıştırma](media/search-create-app-portal/run-app.png)


## <a name="clean-up-resources"></a>Kaynakları temizleme

Kendi aboneliğinizde çalışırken, sizin oluşturduğunuz kaynaklara hala ihtiyacınız olup olmadığını belirlemek için bir projenin sonunda iyi bir fikir olur. Çalışan kaynaklar sizin için ücret verebilir. Kaynakları tek tek silebilir veya kaynak grubunu silerek tüm kaynak kümesini silebilirsiniz.

Sol gezinti bölmesindeki **tüm kaynaklar** veya **kaynak grupları** bağlantısını kullanarak portalda kaynakları bulabilir ve yönetebilirsiniz.

Ücretsiz bir hizmet kullanıyorsanız, üç Dizin, Dizin Oluşturucu ve veri kaynağı ile sınırlı olduğunu unutmayın. Sınırın altında kalmak için portalda ayrı ayrı öğeleri silebilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

Varsayılan uygulama ilk keşif ve küçük görevler için yararlı olsa da, API 'Lerin erken şekilde incelenmesi, kavram ve iş akışını daha derin bir düzeyde anlamanıza yardımcı olur:

> [!div class="nextstepaction"]
> [.NET SDK kullanarak dizin oluşturma](https://docs.microsoft.com/azure/search/search-create-index-dotnet)