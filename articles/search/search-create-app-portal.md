---
title: Azure portalında arama uygulaması oluşturma
titleSuffix: Azure Cognitive Search
description: Operasyonel bir web uygulaması için HTML sayfaları ve komut dosyası oluşturmak için Uygulama Oluştur (önizleme) sihirbazını çalıştırın. Sayfa, bir arama çubuğu, sonuç alanı, kenar çubuğu ve typeahead desteği içerir.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: quickstart
ms.date: 03/25/2020
ms.openlocfilehash: 248ef093601eda7a180a6465ccb97e6fc1c9fe41
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80369720"
---
# <a name="quickstart-create-a-search-app-in-the-portal-azure-cognitive-search"></a>Hızlı başlangıç: Portalda bir arama uygulaması oluşturma (Azure Bilişsel Arama)

Bir tarayıcıda çalışan indirilebilir, "localhost" tarzı bir web uygulaması oluşturmak için portalın **Arama oluştur sihirbazını** kullanın. Yapılandırmasına bağlı olarak, oluşturulan uygulama ilk kullanımda çalışır durumda dır ve uzak bir dizine canlı bağlantı sağlar. Varsayılan uygulama bir arama çubuğu, sonuç alanı, kenar çubuğu filtreleri ve typeahead desteği içerebilir.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun. 

## <a name="prerequisites"></a>Ön koşullar

[Microsoft Edge'in en son sürümüne](https://www.microsoft.com/edge) yükseltin veya bu hızlı başlangıç için Google'ın Chrome tarayıcısını kullanın.

[Bir Azure Bilişsel Arama hizmeti oluşturun](search-create-service-portal.md) veya geçerli aboneliğiniz altında [varolan bir hizmeti bulun.](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) Bu hızlı başlangıç için ücretsiz bir hizmet kullanabilirsiniz. 

Uygulamanızın temeli olarak kullanmak üzere [bir dizin oluşturun.](search-create-index-portal.md) 

Bu hızlı başlatma, küçük resim resimleri olduğundan yerleşik Emlak örnek verilerini ve dizinini kullanır (sihirbaz sonuçlar sayfasına resim eklemeyi destekler). Bu alıştırmada kullanılan dizini oluşturmak *için, emlak-us-örnek* veri kaynağını seçerek **İçe Aktar veri** sihirbazını çalıştırın.

![örnek veriler için veri kaynağı sayfası](media/search-create-app-portal/import-data-realestate.png)

Dizin kullanıma hazır olduğunda, bir sonraki adıma geçin.

## <a name="start-the-wizard"></a>Sihirbazı başlatın

1. [Azure portalında](https://portal.azure.com) oturum açın ve [arama hizmetinizi bulun.](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices)

1. Genel Bakış sayfasında, sayfanın ortasındaki bağlantılardan **Dizinler'i**seçin. 

1. Varolan dizinler listesinden *gayrimenkul-us-örnek dizini* seçin.

1. Dizin sayfasında, en üstte, sihirbazı başlatmak için **arama uygulaması oluştur'u (önizleme)** seçin.

1. İlk sihirbaz sayfasında, dizin tanımınıza CORS desteği eklemek için **Çapraz Başlangıç Kaynak Paylaşımını (CORS)** Etkinleştir'i'ni seçin. Bu adım isteğe bağlıdır, ancak yerel web uygulamanız onsuz uzak dizine bağlanmaz.

## <a name="configure-search-results"></a>Arama sonuçlarını yapılandırma

Sihirbaz, küçük resim resmi, başlık ve açıklama için alan içeren işlenmiş arama sonuçları için temel bir düzen sağlar. Bu öğelerin her birini destekleyen verileri sağlayan dizin bir alandır. 

1. Küçük Resim'de, *realestate-us-sample* dizinindeki *küçük resim* alanını seçin. Bu örnek, *küçük resim*adı verilen bir alanda depolanan URL adresli görüntüler şeklinde resim küçük resimleri içerir. Dizininizin görüntüleri yoksa, bu alanı boş bırakın.

1. Başlık'ta, her belgenin benzersizliğini ifade eden bir alan seçin. Bu örnekte, listeleme kimliği makul bir seçimdir.

1. Açıklama'da, bir kişinin o belgeyi tıklatıp tıklatmayacağına karar vermesine yardımcı olabilecek ayrıntılar sağlayan bir alan seçin.

![örnek veriler için veri kaynağı sayfası](media/search-create-app-portal/configure-results.png)

## <a name="add-a-sidebar"></a>Kenar çubuğu ekleme

Arama hizmeti genellikle bir kenar çubuğu olarak işlenen yönlü gezinme destekler. Fasonlar, dizin şemasında ifade edilen filtrelenebilir ve yüz yüze alanlara dayanır.

Azure Bilişsel Arama'da, yönlü gezinme kümülatif bir filtreleme deneyimidir. Bir kategori içinde, birden çok filtre seçmek sonuçları genişletir (örneğin, Şehir içinde Seattle ve Bellevue'yi seçmek). Kategoriler arasında, birden çok filtre seçmek sonuçları daraltır.

> [!TIP]
> Portalda tam dizin şema görüntüleyebilirsiniz. Her dizinin genel bakış sayfasında **Dizin tanımı (JSON)** bağlantısını arayın. Yönlü gezinme için uygun alanlar "filtrelenebilir: doğru" ve "facetable: true" öznitelikleri vardır.

Geçerli fatür seçimini kabul edin ve bir sonraki sayfaya devam edin.


## <a name="add-typeahead"></a>İleride yazma ekleme

Typeahead işlevselliği otomatik tamamlama ve sorgu önerileri şeklinde kullanılabilir. Sihirbaz sorgu önerilerini destekler. Kullanıcı tarafından sağlanan tuş vuruşu girişlerine bağlı olarak, arama hizmeti giriş olarak seçilebilen "tamamlanmış" sorgu dizelerinin bir listesini döndürür.

Öneriler belirli alan tanımlarında etkinleştirilir. Sihirbaz, bir öneride ne kadar bilginin yer aldığına yapılandırmanız için seçenekler sunar. 

Aşağıdaki ekran görüntüsü, uygulamada işlenmiş bir sayfayla yan yana getirilen sihirbazdaki seçenekleri gösterir. Alan seçimlerinin nasıl kullanıldığını ve öneriiçinde etiketlemeyi eklemek veya hariç tutmak için "Alan Adını Göster"in nasıl kullanıldığını görebilirsiniz.

![Sorgu önerisi yapılandırması](media/search-create-app-portal/suggestions.png)

## <a name="create-download-and-execute"></a>Oluşturma, indirme ve çalıştırma

1. HTML dosyasını oluşturmak için **arama oluştur uygulamasını** seçin.

1. İstendiğinde, dosyayı indirmek için **uygulamanızı indir'i** seçin.

1. Dosyayı açın. Aşağıdaki ekran görüntüsüne benzer bir sayfa görmeniz gerekir. Bir terim girin ve sonuçları daraltmak için filtreler kullanın. 

Temel dizin, belgeler arasında çoğaltılan hayali, oluşturulan verilerden oluşur ve açıklamalar bazen görüntüyle eşleşmez. Kendi dizinlerinizi temel alan bir uygulama oluşturduğunuzda daha uyumlu bir deneyim bekleyebilirsiniz.

![Uygulamayı çalıştırma](media/search-create-app-portal/run-app.png)


## <a name="clean-up-resources"></a>Kaynakları temizleme

Kendi aboneliğinizde çalışırken, projenin sonunda oluşturduğunuz kaynaklara hala ihtiyacınız olup olmadığını belirlemek iyi bir fikirdir. Çalışır durumda bırakılan kaynaklar maliyetlerinizin artmasına neden olabilir. Kaynakları teker teker silebilir veya tüm kaynak grubunu silerek kaynak kümesinin tamamını kaldırabilirsiniz.

Sol navigasyon bölmesindeki **Tüm kaynaklar** veya **Kaynak grupları** bağlantısını kullanarak portaldaki kaynakları bulabilir ve yönetebilirsiniz.

Ücretsiz bir hizmet kullanıyorsanız, üç dizin, dizin ve veri kaynağıyla sınırlı olduğunuzu unutmayın. Sınırın altında kalmak için portaldaki tek tek öğeleri silebilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

Varsayılan uygulama ilk arama ve küçük görevler için yararlı olsa da, API'leri erken gözden geçirmek kavramları ve iş akışını daha derin bir düzeyde anlamanıza yardımcı olacaktır:

> [!div class="nextstepaction"]
> [.NET SDK'yı kullanarak dizin oluşturma](https://docs.microsoft.com/azure/search/search-create-index-dotnet)