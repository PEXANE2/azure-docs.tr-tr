---
title: Azure portal bir bilgi deposu oluşturun Azure Search
description: Bilişsel arama işlem hattından, Azure portal verileri Içeri aktarma Sihirbazı 'nı kullanarak kalıcı zenginler için Azure Search bilgi deposu oluşturun.
author: lisaleib
services: search
ms.service: search
ms.subservice: cognitive-search
ms.topic: tutorial
ms.date: 09/03/2019
ms.author: v-lilei
ms.openlocfilehash: 14996d0ac9ee4e086a5dccd9275ef694adca06ca
ms.sourcegitcommit: dd69b3cda2d722b7aecce5b9bd3eb9b7fbf9dc0a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2019
ms.locfileid: "70963025"
---
# <a name="create-an-azure-search-knowledge-store-in-the-azure-portal"></a>Azure portal Azure Search bilgi deposu oluşturun

> [!Note]
> Bilgi deposu önizlemededir ve üretimde kullanılmamalıdır. [Azure Search REST API sürüm 2019-05-06-önizleme](search-api-preview.md) bu özelliği sağlar. Şu anda .NET SDK desteği yok.
>

Bilgi deposu, daha sonraki analizler veya diğer aşağı akış işlemleri için bir AI zenginleştirme ardışık düzeninde çıktıyı sürekli olarak sürdüren Azure Search bir özelliktir. Bir AI zenginleştirme işlem hattı, görüntü dosyalarını veya yapılandırılmamış metin dosyalarını kabul eder, Azure Search kullanarak dizinler, bilişsel hizmetlerden AI zenginleştirmelerini (görüntü analizi ve doğal dil işleme) uygular ve sonuçları Azure 'da bir bilgi deposuna kaydeder. Depo. Daha sonra bilgi deposunu araştırmak için Power BI veya Depolama Gezgini gibi araçları kullanabilirsiniz.

Bu makalede, AI zenginleştirmelerini almak, dizinlemek ve bir otel gözden geçirmeleri kümesine uygulamak için Azure portal veri alma Sihirbazı 'nı kullanacaksınız. Otel İncelemeleri Azure blog depolama alanına aktarılır ve sonuçlar Azure Tablo Depolaması 'nda bir bilgi deposu olarak kaydedilir.

Bilgi deposunu oluşturduktan sonra, bu bilgi deposuna Depolama Gezgini veya Power BI kullanarak nasıl erişebileceğinizi öğrenebilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

+ Geçerli aboneliğinizde [bir Azure Search hizmeti oluşturun](search-create-service-portal.md) veya [var olan bir hizmeti bulun](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) . Bu öğretici için ücretsiz bir hizmet kullanabilirsiniz.

+ Örnek verileri ve bilgi deposunu depolamak için [bir Azure depolama hesabı oluşturun](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account) . Depolama hesabınızın aynı konumu kullanması gerekir (Azure Search hizmetiniz gibi ABD-WEas) ve *Hesap türü* *StorageV2 (genel amaçlı v2)* (varsayılan) veya *depolama (genel amaçlı v1)* olmalıdır.

## <a name="load-the-data"></a>Verileri yükleme

Bir Azure Search Indexer tarafından erişilebilmesi ve AI zenginleştirme ardışık düzeninde beslenmesini sağlamak için otel, CSV dosyasını Azure Blob depolama alanına yükleyin.

### <a name="create-an-azure-blob-container-with-the-data"></a>Verilerle bir Azure Blob kapsayıcısı oluşturma

1. [BIR CSV dosyasına kaydedilmiş otel gözden geçirme verilerini indirin (HotelReviews_Free. csv)](https://knowledgestoredemo.blob.core.windows.net/hotel-reviews/HotelReviews_Free.csv?st=2019-07-29T17%3A51%3A30Z&se=2021-07-30T17%3A51%3A00Z&sp=rl&sv=2018-03-28&sr=c&sig=LnWLXqFkPNeuuMgnohiz3jfW4ijePeT5m2SiQDdwDaQ%3D). Bu veriler Kaggle.com adresinden kaynaklanır ve oteller hakkında müşteri geri bildirimi içerir.
1. [Azure Portal oturum açın](https://portal.azure.com)ve Azure depolama hesabınıza gidin.
1. [BLOB kapsayıcısı oluşturma](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal) Bunu yapmak için, depolama hesabınızın sol gezinti çubuğunda, **Bloblar**' a tıklayın ve ardından komut çubuğunda **+ kapsayıcı** ' ya tıklayın.
1. Yeni kapsayıcı **adı**için girin `hotel-reviews`.
1. Herhangi bir **genel erişim düzeyi**seçin. Varsayılan değer kullandık.
1. Azure Blob kapsayıcısını oluşturmak için **Tamam** ' ı tıklatın.
1. Yeni `hotels-review` kapsayıcıyı açın, **karşıya yükle**' ye tıklayın ve ilk adımda indirdiğiniz **HotelReviews-Free. csv** dosyasını seçin.

    ![Verileri karşıya yükleme](media/knowledge-store-create-portal/upload-command-bar.png "Otel Incelemelerini karşıya yükleyin")

1. CSV dosyasını Azure Blob depolamaya aktarmak için **karşıya yükle** ' ye tıklayın. Yeni kapsayıcı görüntülenir.

    ![Azure Blob kapsayıcısını oluşturma](media/knowledge-store-create-portal/hotel-reviews-blob-container.png "Azure Blob kapsayıcısını oluşturma")

### <a name="get-the-azure-storage-account-connection-string"></a>Azure depolama hesabı bağlantı dizesini al

1. Portalda Azure depolama hesabınıza gidin.
1. Hizmetin sol gezinti bölmesinde **erişim tuşları**' na tıklayın.
1. **Anahtar 1**' ın altında *bağlantı dizesini*kopyalayın ve kaydedin. Dize ile `DefaultEndpointsProtocol=https`başlar. Depolama hesabınızın adı ve anahtarınız dizeye katıştırılır. Bu dizeyi kullanışlı tutun. Bu, gelecekteki adımlarda gerekecektir.

## <a name="create-and-run-ai-enrichments"></a>AI zenginleştirme oluşturma ve çalıştırma

Bilgi deposu oluşturmak için veri alma Sihirbazı 'nı kullanın. Bir veri kaynağı oluşturacaksınız, zenginler ' i seçin, bir bilgi deposu ve bir dizin yapılandırın ve ardından öğesini yürütün.

### <a name="start-the-import-data-wizard"></a>Veri alma Sihirbazı 'nı başlatma

1. Azure portal, [arama hizmetinizi bulun](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices).

1. İçeri Aktarma Sihirbazı 'nı başlatmak için komut çubuğunda **verileri Içeri aktar** ' a tıklayın.

### <a name="connect-to-your-data-import-data-wizard"></a>Verilerinize Bağlanma (veri alma Sihirbazı)

Bu sihirbaz adımında, Azure Blobundan otel verilerinize sahip bir veri kaynağı oluşturacaksınız.

1. **Veri kaynağı** listesinde **Azure Blob depolama**' yı seçin.
1. **Ad**için girin `hotel-reviews-ds`.
1. **Ayrıştırma modu**Için, **sınırlandırılmış metin**' i seçin ve ardından **üstbilgi içerir** onay kutusunu seçin. **Sınırlayıcı karakterin** virgül (,) olduğundan emin olun.
1. Önceki bir adımda kaydettiğiniz depolama hizmeti **Bağlantı dizenizi** girin.
1. **Kapsayıcı adı**için girin `hotel-reviews`.
1. İleri **' ye tıklayın: Bilişsel arama ekleyin (Isteğe**bağlı).

      ![Veri kaynağı nesnesi oluşturma](media/knowledge-store-create-portal/hotel-reviews-ds.png "Veri kaynağı nesnesi oluşturma")

## <a name="add-cognitive-search-import-data-wizard"></a>Bilişsel arama ekleme (veri alma Sihirbazı)

Bu sihirbaz adımında Bilişsel Beceri bilişsel becerileri ile bir beceri oluşturacaksınız. Bu örnekte kullandığımız yetenekler, anahtar tümceleri ayıklar ve dili ve yaklaşımı tespit eder. Bu zenginler, Azure tabloları olarak bir bilgi deposunda "yansıtılmaktadır" olacaktır.

1. Bilişsel **Hizmetler Ekle**' yi genişletin. **Ücretsiz (sınırlı enzenginler)** varsayılan olarak seçilidir. Bu kaynağı, HotelReviews-Free. csv içindeki kayıt sayısı 19 ve bu ücretsiz kaynak günde en fazla 20 işlem sağladığından kullanabilirsiniz.
1. **Zenginleştirme Ekle**' yi genişletin.
1. **Beceri adı**için girin `hotel-reviews-ss`.
1. **Kaynak veri alanı**için **reviews_text*öğesini seçin.
1. **Zenginleştirme ayrıntı düzeyi**için **sayfalar (5000 karakter öbekleri)** seçeneğini belirleyin
1. Bu bilişsel becerileri seçin:
    + **Anahtar ifadeleri ayıklama**
    + **Dili algıla**
    + **Yaklaşımı Algıla**

      ![Beceri oluşturma](media/knowledge-store-create-portal/hotel-reviews-ss.png "Beceri oluşturma")

1. **Zenginleştirme bilgilerini bilgi deposuna kaydet**' i genişletin.
1. Önceki bir adımda kaydettiğiniz **depolama hesabı bağlantı dizesini** girin.
1. Şu **Azure Tablo projeksiyonlarını**seçin:
    + **Belgelerini**
    + **Sayfalar**
    + **Anahtar tümceleri**

    ![Bilgi deposunu yapılandırma](media/knowledge-store-create-portal/hotel-reviews-ks.png "Bilgi deposunu yapılandırma")

1. İleri **' ye tıklayın: Hedef dizini**özelleştirin.

### <a name="import-data-import-data-wizard"></a>Veri alma (veri alma Sihirbazı)

Bu sihirbaz adımında isteğe bağlı tam metin arama sorguları için bir dizin yapılandıracaksınız. Sihirbaz, alanları ve veri türlerini çıkarması için veri kaynağınızı örnekedecektir. Yalnızca istediğiniz davranışın özniteliklerini seçmeniz gerekir. Örneğin, **alınabilir** özniteliği arama hizmetinin bir alan değeri döndürmesini sağlar, ancak **aranabilir** bir alanda tam metin aramasını etkinleştirir.

1. **Dizin adı**için girin `hotel-reviews-idx`.
1. Öznitelikler için şu seçimleri yapın:
    + Tüm alanlar için **alınabilir** ' i seçin.
    + Bu alanlar için **filtrelenebilir** ve çok **yönlü tablo** seçin:Yaklaşım, *dil*, *Keyphrases*
    + Şu alanlar için **aranabilir** öğesini seçin: *şehir*, *ad*, *reviews_text*, *dil*, *Keyphrases*

    Dizininiz aşağıdaki görüntüye benzer görünmelidir. Liste uzun olduğundan, tüm alanlar görüntüde görünür değildir.

    ![Dizin yapılandırma](media/knowledge-store-create-portal/hotel-reviews-idx.png "Dizin yapılandırma")

1. İleri **' ye tıklayın: Dizin Oluşturucu**oluşturun.

### <a name="create-an-indexer"></a>Dizin oluşturucu oluşturun

Bu sihirbaz adımında, veri kaynağını, beceri ve önceki sihirbaz adımlarında tanımladığınız dizini birlikte alacak bir Dizin Oluşturucu yapılandıracaksınız.

1. **Ad**için girin `hotel-reviews-idxr`.
1. **Zamanlama**Için varsayılanı **bir kez**saklayın.
1. Dizin oluşturucuyu çalıştırmak için **Gönder** ' e tıklayın. Veri ayıklama, dizin oluşturma, bilişsel yeteneklerin uygulaması bu adımda gerçekleşir.

### <a name="monitor-the-notifications-queue-for-status"></a>Durum için bildirim kuyruğunu izleme

1. Azure portal, tıklanabilir bir **Azure Search bildirim** durumu bağlantısı için bildirimler etkinlik günlüğünü izleyin. Yürütmenin tamamlanması birkaç dakika sürebilir.

## <a name="next-steps"></a>Sonraki adımlar

Bilişsel hizmetler 'i kullanarak verilerinizi zenginleştirdikten ve sonuçları bir bilgi deposuna yansıdığınıza göre, zenginleştirilmiş veri kümesini araştırmak için Depolama Gezgini veya Power BI kullanabilirsiniz.

Bu bilgi deposunu Depolama Gezgini kullanarak nasıl keşfedeceğinizi öğrenmek için aşağıdaki izlenecek yolu inceleyin.

> [!div class="nextstepaction"]
> [Depolama Gezgini ile görüntüle](knowledge-store-view-storage-explorer.md)

Bu bilgi deposunu Power BI nasıl bağlayacağınızı öğrenmek için aşağıdaki izlenecek yolu inceleyin.

> [!div class="nextstepaction"]
> [Power BI ile bağlanma](knowledge-store-connect-power-bi.md)

Bu alıştırmayı yinelemek veya farklı bir AI zenginleştirme Kılavuzu denemek istiyorsanız, *otel-incelemeler-ıdxr* Dizin oluşturucuyu silin. Dizin oluşturucunun silinmesi, ücretsiz günlük işlem sayacını yeniden sıfıra sıfırlar.
