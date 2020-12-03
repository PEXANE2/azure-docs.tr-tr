---
title: Verilerinize otomatik olarak duyarlılık etiketleri uygulama
description: Duyarlılık etiketleri oluşturmayı ve tarama sırasında verilerinize otomatik olarak nasıl uygulanacağını öğrenin.
author: batamig
ms.author: bagol
ms.service: purview
ms.subservice: purview-data-catalog
ms.topic: how-to
ms.date: 12/03/2020
ms.openlocfilehash: 195ddfcc3c781c1866a7751f989c9460463edbb9
ms.sourcegitcommit: 65a4f2a297639811426a4f27c918ac8b10750d81
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2020
ms.locfileid: "96558173"
---
# <a name="automatically-label-your-data-in-azure-purview"></a>Verilerinizi Azure purview 'da otomatik olarak etiketleme

Bu makalede, Microsoft Information Protection (MıP) duyarlılık etiketlerinin nasıl oluşturulduğu ve Azure purview 'ta Azure varlıklarınıza otomatik olarak nasıl uygulanacağı açıklanır.

## <a name="what-are-sensitivity-labels"></a>Duyarlılık etiketleri nelerdir? 

İşleri yapmak için kuruluşunuzdaki kişiler, kuruluşun içindeki ve dışındaki kişilerle işbirliği yapmış. Veriler her zaman bulutta kalır ve genellikle cihazlar, uygulamalar ve hizmetler arasında her yerde gezinmez. 

Verileriniz bir gezinmeden, kuruluşunuzun iş ve uyumluluk ilkelerini karşılayan güvenli, korumalı bir şekilde bunu yapmak istersiniz.

Duyarlılık etiketlerini uygulamak, belirli verilerin kuruluşunuzda ne kadar hassas olduğunu belirtmenize olanak sağlar. Örneğin, belirli bir proje adı kuruluşunuzda çok gizli olabilir, ancak aynı terim diğer kuruluşlara gizli değildir. 

### <a name="sensitivity-labels-in-azure-purview"></a>Azure purview 'da duyarlılık etiketleri

Purview 'da, sınıflandırmalar konu etiketlerine benzerdir ve tarama sırasında verileriniz içinde bulunan belirli bir türün verilerini işaretlemek ve tanımlamak için kullanılır.

Takip, Microsoft 365 olarak gizli bilgi türleri olarak da bilinen aynı sınıflandırmaları kullanır.  MıP duyarlılık etiketleri Microsoft 365 güvenlik ve Uyumluluk Merkezi 'nde (SCC) oluşturulur. Bu, mevcut duyarlılık etiketlerinizi Azure purview varlıklarınız genelinde genişletmenizi sağlar.

> [!NOTE]
> Sınıflandırmalar doğrudan eşleştirirken (sosyal güvenlik numarası bir **sosyal güvenlik numarası** sınıflandırmasına sahiptir), bir veya daha fazla sınıflandırma ve senaryo birlikte bulunduğunda duyarlık etiketleri uygulanır. 
> 

Azure purview içindeki duyarlılık etiketleri, dosyalara ve veritabanı sütunlarına otomatik olarak Etiketler uygulamak için kullanılabilir.

Daha fazla bilgi için bkz:

- Microsoft 365 belgelerindeki [duyarlılık etiketleri hakkında bilgi edinin](/microsoft-365/compliance/sensitivity-labels)
- [Oto etiketleme kuralları nelerdir?](#what-are-autolabeling-rules)
- [Azure purview 'da duyarlılık etiketleri için desteklenen veri türleri](#supported-data-types-for-sensitivity-labels-in-azure-purview)

#### <a name="what-are-autolabeling-rules"></a>Oto etiketleme kuralları nelerdir?

Verileriniz sürekli büyümekte ve değişiyor. Şu anda etiketsiz olan verileri izlemek ve etiketleri elle uygulamak için eylem yapmak yalnızca gerekli değildir, ancak aynı zamanda gereksiz bir headache ' dir. 

Belirli bir etiketin ne zaman uygulanacağını belirten,, oto etiketleme kuralları belirttiğiniz koşullardır. Bu koşullar karşılandığında, etiket otomatik olarak verilere atanır ve verilerdeki tutarlı duyarlılık etiketlerini ölçekli olarak korur.

Etiketlerinizi oluştururken, her bir veri taramasında etiketlerinizi otomatik olarak uygulamak için hem [Dosya](#define-autolabeling-rules-for-files) hem de [veritabanı sütunları](#define-autolabeling-rules-for-database-columns) için otomatik etiketleme kuralları tanımlamadığınızdan emin olun. 

Verilerinizi takip ettiğiniz şekilde taradıktan sonra, takip görünümü kataloğunda ve Öngörüler raporlarında otomatik olarak uygulanan etiketleri görüntüleyebilirsiniz.

#### <a name="supported-data-types-for-sensitivity-labels-in-azure-purview"></a>Azure purview 'da duyarlılık etiketleri için desteklenen veri türleri

Duyarlılık etiketleri aşağıdaki veri türleri için Azure purview 'da desteklenir:

|Veri türü  |Kaynaklar  |
|---------|---------|
|Dosyalar için otomatik etiketleme     |     -Azure Blob depolama  </br>-Azure Data Lake Storage Gen 1 ve Gen 2  |
|Veritabanı sütunları için otomatik etiketleme     |  -SQL Server </br>-Azure SQL veritabanı </br>-Azure SQL veritabanı yönetilen örneği   <br> -Azure SYNAPSE  <br>-Azure Cosmos DB   |
| | |

## <a name="how-to-create-sensitivity-labels-in-microsoft-365"></a>Microsoft 365 duyarlılık etiketleri oluşturma

Duyarlılık etiketleriniz yoksa, bunları oluşturmanız ve Azure purview için kullanılabilir hale getirmeniz gerekir. Mevcut duyarlılık etiketleri, Azure purview için kullanılabilir hale getirmek üzere de değiştirilebilir.

Daha fazla bilgi için bkz:

- [Lisanslama gereksinimleri](#licensing-requirements)
- [Duyarlılık etiketlerini Azure purview 'a genişletme](#extending-sensitivity-labels-to-azure-purview)
- [Yeni duyarlılık etiketleri oluşturma veya varolan etiketleri değiştirme](#creating-new-sensitivity-labels-or-modifying-existing-labels)
### <a name="licensing-requirements"></a>Lisanslama gereksinimleri

MıP duyarlılık etiketleri Microsoft 365 güvenlik ve Uyumluluk Merkezi 'nde oluşturulur ve yönetilir. Azure purview 'da kullanılmak üzere duyarlılık etiketleri oluşturmak için etkin bir Microsoft 365 E5 lisansına sahip olmanız gerekir.

Gerekli lisansınız yoksa, [Microsoft 365 E5](https://www.microsoft.com/microsoft-365/business/compliance-solutions#midpagectaregion)deneme sürümü için kaydolabilirsiniz.

### <a name="extending-sensitivity-labels-to-azure-purview"></a>Duyarlılık etiketlerini Azure purview 'a genişletme

Varsayılan olarak, MıP duyarlılık etiketleri yalnızca Microsoft 365 varlıklar için kullanılabilir ve burada bunları dosyalara ve e-postalara uygulayabilirsiniz.

Azure purview 'daki Azure varlıklarına MıP duyarlılık etiketlerini uygulamak için, etiketleri genişletmeyi açık bir şekilde kabul etmeniz ve purview 'ta kullanılabilir olmasını istediğiniz belirli etiketleri seçmeniz gerekir.

Kuruluşlar, MıP 'nin duyarlılık etiketlerini Azure purview ile genişleterek, daha geniş bir veri kaynağı yelpazesinden uyumluluk riskini en aza indirerek daha fazla bilgi bulabilir, sınıflandırabilir ve bunlarla ilgili daha fazla bilgi alabilir.

> [!NOTE]
> Microsoft 365 ve Azure purview ayrı hizmetler olduğundan, farklı bölgelerde dağıtılabilecek bir olasılık vardır. Etiket adları ve özel hassas bilgi türü adları müşteri verileri olarak kabul edilir ve verilerinizin duyarlılığını korumak ve GDPR yasaları önlemek için varsayılan olarak aynı COĞRAFI konum içinde tutulur.
>
> Bu nedenle, Etiketler ve özel hassas bilgi türleri varsayılan olarak Azure purview ile paylaşılmaz ve bunları Azure purview ' de kullanmak için onay gerektirir.

> [!IMPORTANT]
> İzniniz, Microsoft 'un etiket adını ve özel hassas bilgi türü adını hem Azure purview hem *de* Azure Güvenlik MERKEZI (ASC) ile paylaşmasını sağlar. Microsoft, önerilerinizi ve uyarılarınızı ASC ile zenginleştirmek için Azure purview 'daki etiket bilgilerini kullanır. 
>
> Microsoft 365 Uyumluluk Merkezi 'nde bu verileri her iki hizmet ile paylaşmak için geçerlidir. Şu anda etiketleme bilgilerini yalnızca Azure purview ile paylaşma seçeneği yoktur.

**Duyarlılık etiketlerini takip görünümüne genişletmek için:**

Microsoft 365 **Information Protection** sayfasına gidin. **Azure purview 'daki varlıklara etiketlemeyi Genişlet**' de, **Aç** düğmesini seçin ve açılan onay iletişim kutusunda **Evet** ' i seçin.

Örnek:

:::image type="content" source="media/create-sensitivity-label/extend-sensitivity-labels-to-purview-small.png" alt-text="Duyarlılık etiketlerini purview 'a genişletmek için * * aç * * seçeneğini belirleyin" lightbox="media/create-sensitivity-label/extend-sensitivity-labels-to-purview.png":::
 
Etiketlemeyi Azure purview içindeki varlıklara genişlettikten sonra, purview 'ta kullanılabilir hale getirmek istediğiniz etiketleri seçebilirsiniz. Daha fazla bilgi için bkz. [yeni duyarlılık etiketleri oluşturma veya varolan etiketleri değiştirme](#creating-new-sensitivity-labels-or-modifying-existing-labels).
### <a name="creating-new-sensitivity-labels-or-modifying-existing-labels"></a>Yeni duyarlılık etiketleri oluşturma veya varolan etiketleri değiştirme

1. [Microsoft 365 güvenlik ve Uyumluluk Merkezi 'ni](https://protection.office.com/homepage)açın. 

1. **Çözümler** altında **bilgi koruması**' nı seçin ve **etiket oluştur**' u seçin. 

    :::image type="content" source="media/create-sensitivity-label/create-sensitivity-label-full-small.png" alt-text="Microsoft 365 güvenlik ve Uyumluluk Merkezi 'nde duyarlılık etiketleri oluşturma" lightbox="media/create-sensitivity-label/create-sensitivity-label-full.png":::

1. Etiketi adlandırın. Ardından, **Bu etiketin kapsamını tanımla** altında **Dosyalar ve e-postalar** ve **Azure purview varlıkları**' nı seçin.
    
    :::image type="content" source="media/create-sensitivity-label/create-label-scope-small.png" alt-text="Microsoft 365 güvenlik ve Uyumluluk Merkezi 'nde etiketinizi oluşturun" lightbox="media/create-sensitivity-label/create-label-scope.png":::

1. Etiket ayarlarınız için sihirbazdaki diğer istemlerin geri kalanını izleyin. 

    Özel olarak, dosyalar ve veritabanı sütunları için bir oto etiketleme kuralları tanımlayın:

    - [Dosyalar için oto etiketleme kuralları tanımlama](#define-autolabeling-rules-for-files)
    - [Veritabanı sütunları için oto etiketleme kuralları tanımlama](#define-autolabeling-rules-for-database-columns)

    Sihirbaz seçenekleri hakkında daha fazla bilgi için, Microsoft 365 belgelerinde [Hangi duyarlılık etiketlerinin yapabileceğini](/microsoft-365/compliance/sensitivity-labels#what-sensitivity-labels-can-do) görün.

1. Ek etiketler oluşturmak için yukarıda listelenen adımları yineleyin. 

    Alt etiket oluşturmak için > üst etiketi seçin **.**  >  .. **Diğer eylemler**  >  **Alt etiket ekleyin**.

1. Varolan etiketleri değiştirmek için **Information Protection**  >  **etiketlere** gidin ve etiketinizi seçin. 

    Ardından etiketi **Düzenle** ' yi seçerek, etiketi oluştururken tanımladığınız tüm ayarlarla **duyarlılık etiketini Düzenle** Sihirbazı ' nı tekrar açın.

    :::image type="content" source="media/create-sensitivity-label/edit-sensitivity-label-full-small.png" alt-text="Varolan duyarlılık etiketini Düzenle" lightbox="media/create-sensitivity-label/edit-sensitivity-label-full.png":::

1. Tüm etiketlerinizi oluşturmayı tamamladığınızda, etiket siparişinizi görüntülemenize ve bunları gerektiği gibi yeniden sıralamanıza dikkat edin. 

    Bir etiketin sırasını değiştirmek için **.** .. **> daha fazla eylem**  >  aşağı **taşınır** veya **aşağı taşıyın.** 

    Daha fazla bilgi için Microsoft 365 belgelerinde [etiket önceliği (sıralama)](/microsoft-365/compliance/sensitivity-labels#label-priority-order-matters) bölümüne bakın.

> [!IMPORTANT]
> Kullanıcılarınızın etkisini anlamadığınız müddetçe bir etiketi silmeyin. 
>
> Daha fazla bilgi için Microsoft 365 belgelerindeki [etiketleri kaldırma ve silme](/microsoft-365/compliance/create-sensitivity-labels#removing-and-deleting-labels) bölümüne bakın.

[Etiketleri otomatik olarak uygulamak için verilerinizi tarayarak](#scan-your-data-to-apply-labels-automatically)devam edin ve ardından:

- [Varlıklarda etiketleri görüntüleme](#view-labels-on-assets)
- [Sınıflandırmalar ve duyarlılık etiketleri için öngörüleri raporlarını görüntüleyin](#view-insight-reports-for-the-classifications-and-sensitivity-labels)

#### <a name="define-autolabeling-rules-for-files"></a>Dosyalar için oto etiketleme kuralları tanımlama

Etiketinizi oluştururken veya düzenlerken, sihirbazdaki dosyalar için bir oto etiketleme kuralları tanımlayın. 

**Office uygulamaları Için otomatik etiketleme** sayfasında, **Office uygulamaları için otomatik etiketlemeyi** etkinleştirin ve sonra etiketinizin verilerinize otomatik olarak uygulanmasını istediğiniz koşulları tanımlayın.

Örnek:

:::image type="content" source="media/create-sensitivity-label/create-auto-labeling-rules-files-small.png" alt-text="Microsoft 365 güvenlik ve Uyumluluk Merkezi 'nde dosyalar için oto etiketleme kuralları tanımlama" lightbox="media/create-sensitivity-label/create-auto-labeling-rules-files.png":::
 
Daha fazla bilgi için, Microsoft 365 belgelerinde [otomatik olarak bir duyarlılık etiketi uygulama](/microsoft-365/compliance/apply-sensitivity-label-automatically#how-to-configure-auto-labeling-for-office-apps) bölümüne bakın. 

#### <a name="define-autolabeling-rules-for-database-columns"></a>Veritabanı sütunları için oto etiketleme kuralları tanımlama

Etiketinizi oluştururken veya düzenlediğinizde sihirbazda veritabanı sütunları için bir oto etiketleme kuralları tanımlayın. 

**Azure purview varlıkları (Önizleme)** seçeneğinin altında:

1. **Veritabanı sütunları Için otomatik etiketleme** kaydırıcısını seçin.

1. Etiketinizden uygulamak istediğiniz hassas bilgi türlerini seçmek için **hassas bilgi türlerini denetle** ' yi seçin.

Örnek:
        
:::image type="content" source="media/create-sensitivity-label/create-auto-labeling-rules-db-columns-small.png" alt-text="Microsoft 365 güvenlik ve Uyumluluk Merkezi 'nde SQL sütunları için oto etiketleme kuralları tanımlama" lightbox="media/create-sensitivity-label/create-auto-labeling-rules-db-columns.png":::

## <a name="scan-your-data-to-apply-labels-automatically"></a>Etiketleri otomatik olarak uygulamak için verilerinizi tarayın

Tanımladığınız otomatik etiketleme kurallarına göre oluşturduğunuz etiketleri otomatik olarak uygulamak için verilerinizi Azure takip görünümünde tarayın. 

Azure purview 'da çeşitli varlıklarda taramalar ayarlama hakkında daha fazla bilgi için bkz.:

|Kaynak  |Başvuru  |
|---------|---------|
|**Azure Blob Depolama**     |[Azure Blob depolamayı kaydetme ve tarama](register-scan-azure-blob-storage-source.md)         |
|**Azure Data Lake Storage**     |[Kaydet ve Tara Azure Data Lake Storage 1.](register-scan-adls-gen1.md) </br>[Kaydet ve Tara Azure Data Lake Storage 2.](register-scan-adls-gen2.md)         |
|**Azure SQL Veritabanları**|[Azure SQL veritabanını kaydetme ve tarama](register-scan-azure-sql-database.md) </br>[Azure SQL veritabanı yönetilen örneğini kaydetme ve tarama](register-scan-azure-sql-database-managed-instance.md)|
|**Güvenlik duvarlarının arkasındaki depolama hesapları**     |[Azure purview 'da güvenlik duvarının arkasındaki depolama hesaplarını tarama](scan-sqlresource-firewall.md)         |
| | |

## <a name="view-labels-on-assets"></a>Varlıklarda etiketleri görüntüleme

Microsoft 365 etiketleriniz için otomatik etiketleme kuralları tanımladıktan ve verilerinizi Azure purview 'da taradıktan sonra, Etiketler otomatik olarak varlıklarınıza uygulanır. 

**Azure purview kataloğunda varlıklarınıza uygulanan etiketleri görüntülemek için:**

Azure purview kataloğunda, yalnızca belirli etiketlere sahip dosyaları göstermek için **etiket** filtreleme seçeneklerini kullanın. Örnek: 

:::image type="content" source="media/create-sensitivity-label/filter-search-results-small.png" alt-text="Varlıkları etikete göre ara" lightbox="media/create-sensitivity-label/filter-search-results.png":::

Örnek:

:::image type="content" source="media/create-sensitivity-label/view-labeled-files-blob-storage-small.png" alt-text="Azure Blob depolamadaki bir dosyadaki duyarlılık etiketini görüntüleme" lightbox="media/create-sensitivity-label/view-labeled-files-blob-storage.png":::

## <a name="view-insight-reports-for-the-classifications-and-sensitivity-labels"></a>Sınıflandırmalar ve duyarlılık etiketleri için öngörüleri raporlarını görüntüleyin

**Sınıflandırma** ve **duyarlılık etiketleme** raporlarını kullanarak Azure takiview 'ta sınıflandırılan ve etiketlenmiş verileriniz hakkında Öngörüler bulun.

> [!div class="nextstepaction"]
> [Sınıflandırma öngörüleri](./classification-insights.md)

> [!div class="nextstepaction"]
> [Duyarlılık etiketi öngörüleri](sensitivity-insights.md)


