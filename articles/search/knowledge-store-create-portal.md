---
title: Azure portalında bir bilgi deposu (önizleme) oluşturma
titleSuffix: Azure Cognitive Search
description: Zenginleştirilmiş içeriği kalıcı olarak sürdürmek için kullanılan bir bilgi deposu oluşturmak için İçe Aktarma verisi sihirbazını kullanın. Diğer uygulamalardan analiz için bir bilgi deposuna bağlanın veya zenginleştirilmiş içeriği akış aşağı işlemlerine gönderin. Bu özellik şu anda genel önizlemede dir.
author: HeidiSteen
ms.author: heidist
manager: nitinme
ms.service: cognitive-search
ms.topic: quickstart
ms.date: 01/29/2020
ms.openlocfilehash: 21279b2b4735a25210e8373d76d0d63f9c711bfc
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77472375"
---
# <a name="quickstart-create-an-azure-cognitive-search-knowledge-store-in-the-azure-portal"></a>Hızlı başlangıç: Azure portalında Bir Azure Bilişsel Arama bilgi deposu oluşturun

> [!IMPORTANT] 
> Bilgi deposu şu anda genel önizlemede. Önizleme işlevi hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Daha fazla bilgi için Microsoft [Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın. 

Bilgi deposu, sonraki analizler veya akış aşağı işleme için bilişsel beceriler ardışık kaynaktan çıktı yısada Azure Bilişsel Arama'nın bir özelliğidir. 

Bir ardışık işlem, yapılandırılmamış metin ve görüntüleri ham içerik olarak kabul eder, Bilişsel Hizmetler (OCR, görüntü analizi ve doğal dil işleme gibi) aracılığıyla AI uygular, bilgileri ayıklar ve yeni yapılar ve bilgiler çıkarır. Bir boru hattı tarafından oluşturulan fiziksel eserlerden biri, içeriği analiz etmek ve keşfetmek için araçlar aracılığıyla erişebileceğiniz bir [bilgi deposudur.](knowledge-store-concept-intro.md)

Bu hızlı başlangıçta, bir bilgi deposu oluşturmak için Azure bulutundaki hizmetleri ve verileri birleştirirsiniz. Her şey yerli yerine oturtuktan sonra, hepsini bir araya getirmek için portaldaki **Alma veri** sihirbazını çalıştıracaksınız. Sonuç, özgün metin içeriği nin yanı sıra portalda görüntülenebilen AI tarafından oluşturulan içeriktir[(Depolama gezgini).](knowledge-store-view-storage-explorer.md)

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="create-services-and-load-data"></a>Hizmet oluşturma ve veri yükleme

Bu hızlı başlangıç, AI için Azure Bilişsel Arama, Azure Blob depolama ve [Azure Bilişsel Hizmetleri'ni](https://azure.microsoft.com/services/cognitive-services/) kullanır. 

İş yükü çok küçük olduğundan, Bilişsel Hizmetler günde en fazla 20 işlem için ücretsiz işleme sağlamak için arka planda dokunulmaktadır. Veri kümesi çok küçük olduğundan, Bilişsel Hizmetler kaynağı oluşturmayı veya eklemeyi atlayabilirsiniz.

1. [Karşıdan HotelReviews_Free.csv .](https://knowledgestoredemo.blob.core.windows.net/hotel-reviews/HotelReviews_Free.csv?sp=r&st=2019-11-04T01:23:53Z&se=2025-11-04T16:00:00Z&spr=https&sv=2019-02-02&sr=b&sig=siQgWOnI%2FDamhwOgxmj11qwBqqtKMaztQKFNqWx00AY%3D) Bu veriler, bir CSV dosyasına kaydedilen (Kaggle.com kaynaklı) otel inceleme verileridir ve tek bir otel hakkında 19 müşteri geri bildirimi içerir. 

1. [Bir Azure depolama hesabı oluşturun](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account?tabs=azure-portal) veya geçerli aboneliğiniz altında [varolan bir hesap bulun.](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Storage%2storageAccounts/) Azure depolama alanını hem alınacak ham içerik hem de sonuçta elde edilecek bilgi deposu için kullanırsınız.

   **StorageV2 (genel amaçlı V2)** hesap türünü seçin.

1. Blob hizmetleri sayfalarını açın ve *otel incelemeleri*adlı bir konteyner oluşturun.

1. **Karşıya Yükle**'ye tıklayın.

    ![Verileri yükleyin](media/knowledge-store-create-portal/upload-command-bar.png "Otel yorumlarını yükleyin")

1. İlk adımda indirdiğiniz **HotelReviews-Free.csv** dosyasını seçin.

    ![Azure Blob kapsayıcısını oluşturma](media/knowledge-store-create-portal/hotel-reviews-blob-container.png "Azure Blob kapsayıcısını oluşturma")

1. Bu kaynakla neredeyse bitirdiniz, ancak bu sayfaları terk etmeden **önce, Erişim Tuşları** sayfasını açmak için sol gezinti bölmesindeki bir bağlantıyı kullanın. Blob depolamadan veri almak için bir bağlantı dizesi alın. Bağlantı dizesi aşağıdaki örneğe benzer:`DefaultEndpointsProtocol=https;AccountName=<YOUR-ACCOUNT-NAME>;AccountKey=<YOUR-ACCOUNT-KEY>;EndpointSuffix=core.windows.net`

1. Hala portalda, Azure Bilişsel Arama'ya geçin. [Yeni bir hizmet oluşturun](search-create-service-portal.md) veya [varolan bir hizmeti bulun.](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) Bu hızlı başlangıç için ücretsiz bir hizmet kullanabilirsiniz.

Artık Veri Aktar sihirbazı üzerinde hareket etmeye hazırsınız.

## <a name="run-the-import-data-wizard"></a>Veri Aktar sihirbazını çalıştır

Arama hizmetigenel bakış sayfasında, dört adımda bir bilgi deposu oluşturmak için komut çubuğundaki **verileri içe aktar'ı** tıklatın.

  ![Verileri içeri aktar komutu](media/cognitive-search-quickstart-blob/import-data-cmd2.png)

### <a name="step-1-create-a-data-source"></a>1. Adım: Veri kaynağı oluşturma

1. **Verilerinize Bağlan'da** **Azure Blob depolama alanını**seçin, oluşturduğunuz hesabı ve kapsayıcıyı seçin. 
1. **Ad**için , `hotel-reviews-ds`girin .
1. **Ayrıştırma modu için,** **Sınırlı Metni**seçin ve ardından **İlk Satır İçeren Üstbilgi** onay kutusunu seçin. **Delimiter karakterinin** virgül (,) olduğundan emin olun.
1. **Bağlantı Dizesi'nde,** Azure Depolama'daki **Access Keys** sayfasından kopyaladığınız bağlantı dizesini yapıştırın.
1. **Kapsayıcılarda,** verileri tutan blob kapsayıcısının adını girin.

    Sayfanız aşağıdaki ekran görüntüsüne benzer olmalıdır.

    ![Veri kaynağı nesnesi oluşturma](media/knowledge-store-create-portal/hotel-reviews-ds.png "Veri kaynağı nesnesi oluşturma")

1. Sonraki sayfaya devam edin.

### <a name="step-2-add-cognitive-skills"></a>2. Adım: Bilişsel yetenekler ekleme

Bu sihirbaz adımda, bilişsel beceri zenginleştirmeler ile bir skillset yaratacak. Kaynak veriler çeşitli dillerde müşteri yorumlarından oluşur. Bu veri kümesi yle ilgili beceriler anahtar ifade çıkarma, duygu algılama ve metin çeviriiçerir. Daha sonraki bir adımda, bu zenginleştirmeler Azure tabloları olarak bir bilgi deposuna "yansıtılacaktır".

1. **Genişlet bilişsel hizmetler ekle.** **Ücretsiz (Sınırlı zenginleştirmeler)** varsayılan olarak seçilir. HotelReviews-Free.csv'deki kayıt sayısı 19 olduğundan ve bu ücretsiz kaynak günde en fazla 20 işlem yapılmasına izin verdiğinden bu kaynağı kullanabilirsiniz.
1. Genişlet **zenginleştirmeler ekle.**
1. **Skillset adı**için `hotel-reviews-ss`, girin .
1. **Kaynak veri alanı**için **reviews_text' yi**seçin.
1. **Zenginleştirme parçalı lık düzeyi** **için, Sayfaları seçin (5000 karakter öbek)**
1. Şu bilişsel becerileri seçin:
    + **Anahtar ifadeleri ayıklama**
    + **Metin çevirme**
    + **Duyguları algılama**

      ![Beceri kümesi oluşturma](media/knowledge-store-create-portal/hotel-reviews-ss.png "Beceri kümesi oluşturma")

1. **Zenginleştirmeleri bilgi deposuna genişletin.**
1. Bu **Azure tablo projeksiyonlarını**seçin:
    + **Belgeler**
    + **Sayfalar**
    + **Anahtar tümcecikler**
1. Önceki adımda kaydettiğiniz **Depolama hesabı Bağlantı Dizesini** girin.

    ![Bilgi deposuyapıla](media/knowledge-store-create-portal/hotel-reviews-ks.png "Bilgi deposuyapıla")

1. İsteğe bağlı olarak, bir Power BI şablonu indirin. Şablona sihirbazdan eriştiyseniz, yerel .pbit dosyası verilerinizin şeklini yansıtacak şekilde uyarlanır.

1. Sonraki sayfaya devam edin.

### <a name="step-3-configure-the-index"></a>3. Adım: Dizini yapılandırma

Bu sihirbaz adımda, isteğe bağlı tam metin arama sorguları için bir dizin yapılandıracak. Sihirbaz, alanları ve veri türlerini çıkarmak için veri kaynağınızı örnekleyecektir. Yalnızca istediğiniz davranış için öznitelikleri seçmeniz gerekir. Örneğin, **Alınabilen** öznitelik arama hizmetinin alan değerini döndürmesine izin verirken, **Aranabilir** alandaki tam metin aramasını etkinleştirecektir.

1. **Dizin adı** `hotel-reviews-idx`için , girin .
1. Öznitelikler için varsayılan seçimleri kabul edin: Ardışık iştisi oluşturmakta olduğu yeni alanlar için **alınıp** **aranabilir.**

    Dizininiz aşağıdaki resme benzer olmalıdır. Liste uzun olduğundan, tüm alanlar görüntüde görünmez.

    ![Dizini yapılandırma](media/knowledge-store-create-portal/hotel-reviews-idx.png "Dizini yapılandırma")

1. Sonraki sayfaya devam edin.

### <a name="step-4-configure-the-indexer"></a>4. Adım: Dizin oluşturucuyu yapılandırma

Bu sihirbaz adımda, önceki sihirbaz adımlarında tanımladığınız veri kaynağını, skillset'i ve dizini bir araya getirecek bir dizin oluşturmanız gerekir.

1. **Ad**için `hotel-reviews-idxr`, girin .
1. **Zamanlama**için varsayılan ı **bir kez**saklayın.
1. Dizinleyiciyi çalıştırmak için **Gönder'i** tıklatın. Veri çıkarma, indeksleme, bilişsel becerilerin uygulanması bu adımda gerçekleşir.

## <a name="monitor-status"></a>Monitör durumu

Bilişsel beceri dizini nindeksi tipik metin tabanlı dizin daha tamamlamak için daha uzun sürer. Sihirbaz, ilerlemeyi izleyebilmeniz için genel bakış sayfasındaKi Dizinleyici listesini açmalıdır. Kendi kendine gezinme için Genel Bakış sayfasına gidin ve **Dizin Leyiciler'i**tıklatın.

Azure portalında, tıklanabilir bir **Azure Bilişsel Arama bildirim** durumu bağlantısı için Bildirimler etkinlik günlüğünü de izleyebilirsiniz. Yürütmenin tamamlanması birkaç dakika sürebilir.

## <a name="next-steps"></a>Sonraki adımlar

Artık Bilişsel Hizmetler'i kullanarak verilerinizi zenginleştirdiğinize ve sonuçları bir bilgi deposuna yansıttığınıza göre, zenginleştirilmiş veri setinizi keşfetmek için Storage Explorer veya Power BI'yi kullanabilirsiniz.

Depolama Gezgini'nde içeriği görüntüleyebilir veya visualization aracılığıyla öngörüler elde etmek için Power BI ile içeriği bir adım daha ileri götürebilirsiniz.

> [!div class="nextstepaction"]
> [Power](knowledge-store-view-storage-explorer.md)
> BI ile Depolama Explorer[Bağlanilen ile](knowledge-store-connect-power-bi.md) görüntüle

> [!Tip]
> Bu alıştırmayı tekrarlamak veya farklı bir AI zenginleştirme gözden geçirme denemek istiyorsanız, *otel-reviews-idxr* indexer'ı silin. Dizinleyicinin silmesi, Bilişsel Hizmetler işleme için ücretsiz günlük işlem sayacını sıfırlar.
