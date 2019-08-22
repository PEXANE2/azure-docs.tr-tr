---
title: Azure portal kullanarak bir arama dizinine veri aktarma Azure Search
description: Azure VM 'lerinde Cosmos DB, BLOB depolama, tablo depolama, SQL veritabanı ve SQL Server Azure verilerini gezinmek için Azure portal veri alma Sihirbazı 'nın nasıl kullanılacağını öğrenin.
author: HeidiSteen
manager: nitinme
services: search
ms.service: search
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: e784cbf351bd062712e0fd66332799907a3bcae8
ms.sourcegitcommit: bb8e9f22db4b6f848c7db0ebdfc10e547779cccc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2019
ms.locfileid: "69648244"
---
# <a name="import-data-wizard-for-azure-search"></a>Azure Search için veri içeri aktarma Sihirbazı

Azure portalı, Azure Search panosunda verileri bir dizine yüklemeye yönelik **Verileri içeri aktarma** sihirbazını içerir. Sihirbaz arka planda *veri kaynağını*, *dizini*ve Dizin *oluşturucuyu* yapılandırır ve dizin oluşturma işleminin çeşitli adımlarını otomatik hale getirerek çağırır: 

* Aynı Azure aboneliğindeki bir dış veri kaynağına bağlanır.
* İsteğe bağlı olarak, yapılandırılmamış verilerden metin ayıklamak için optik karakter tanımayı veya doğal dil işlemesini tümleştirir.
* Veri örneklemesi ve dış veri kaynağının meta verilerini temel alan bir dizin oluşturur.
* Aranabilir içerik için veri kaynağında gezinir, JSON belgelerinin serileştirilmesi ve dizine yüklenmesi.

Sihirbaz önceden tanımlanmış bir dizine bağlanamaz veya var olan bir dizin oluşturucuyu çalıştırabilir, ancak sihirbaz içinde, ihtiyacınız olan yapıyı ve davranışları desteklemek için yeni bir dizin veya Dizin Oluşturucu yapılandırabilirsiniz.

Azure Search'ü ilk kez mi kullanıyorsunuz? [Hızlı başlangıç adımları: **Verileri içeri aktar** ve yerleşik gerçek emlak örnek veri](search-get-started-portal.md) kümesi kullanarak, veri alma ve dizin oluşturma işlemlerini test etmek için Portal araçlarını kullanarak içeri aktarın, dizin oluşturup sorgulayın.

## <a name="start-importing-data"></a>Veri içeri aktarmayı Başlat

Bu bölümde, sihirbazın nasıl başlatılacağı ve her adımın yüksek düzeyli bir genel bakış sunulmaktadır.

1. [Azure Portal](https://portal.azure.com), panodaki arama hizmeti sayfasını açın veya hizmet listesinde [hizmetinizi bulun](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) .

2. En üstteki hizmete genel bakış sayfasında, **verileri Içeri aktar**' a tıklayın.

   ![Portalda verileri Içeri aktar komutu](./media/search-import-data-portal/import-data-cmd2.png "Veri alma Sihirbazı 'Nı başlatma")

   > [!NOTE]
   > Azure Cosmos DB, Azure SQL veritabanı ve Azure Blob depolama dahil olmak üzere diğer Azure hizmetlerinden **Içeri aktarma verilerini** başlatabilirsiniz. Hizmet genel bakış sayfasında sol gezinti bölmesinde **Azure Search Ekle** ' ye bakın.

3. Sihirbaz, **verilerinize bağlanmak**için açılır; burada, bu içeri aktarma için kullanılacak bir dış veri kaynağı seçebilirsiniz. Bu adım hakkında bilmeniz gereken birkaç şey vardır, bu nedenle daha fazla ayrıntı için [veri kaynağı girişleri](#data-source-inputs) bölümünü okuduğunuzdan emin olun.

   ![Portalda veri alma Sihirbazı](./media/search-import-data-portal/import-data-wizard-startup.png "Azure Search için veri Içeri aktarma Sihirbazı")

4. Daha sonra, görüntü dosyalarındaki metnin optik karakter tanıma (OCR) özelliğini veya yapılandırılmamış veriler üzerinde metin analizini eklemek istiyorsanız bilişsel **Arama ekleyin**. Bilişsel hizmetler 'den AI algoritmaları bu görev için çekilir. Bu adımın iki bölümü vardır:
  
   İlk olarak, bir Azure Search beceri bir bilişsel [Hizmetler kaynağı ekleyin](cognitive-search-attach-cognitive-services.md) .
  
   İkincisi, Beceri içinde hangi AI zenginleştirmelerinin ekleneceğini seçin. İzlenecek yol gösterimi için bu [hızlı başlangıç](cognitive-search-quickstart-blob.md)bölümüne bakın.

   Yalnızca verileri içeri aktarmak istiyorsanız, bu adımı atlayın ve doğrudan dizin tanımına gidin.

5. Daha sonra, sihirbazda sunulan Dizin şemasını kabul edebilir veya değiştirebileceğiniz **hedef dizini özelleştirin**. Sihirbaz, verileri örnekleyerek ve dış veri kaynağından meta verileri okuyarak alanları ve veri türlerini okur.

   Her alan için, belirli davranışları etkinleştirmek üzere [Dizin özniteliklerini denetleyin](#index-definition) . Herhangi bir öznitelik seçmezseniz, dizininiz kullanılamaz. 

6. Bir sonraki adımda, bu sihirbazın bir ürünü olan **bir Dizin Oluşturucu oluşturulur**. Dizin Oluşturucu, bir dış Azure veri kaynağından aranabilir verileri ve meta verileri çıkaran bir gezgin. Veri kaynağını seçip becerileri (isteğe bağlı) ve bir dizin ekleyerek, sihirbazın her adımında geçiş yaparken bir Dizin Oluşturucu yapılandırıyorsunuz demektir.

   Dizin oluşturucuya bir ad verin ve içeri aktarma işlemine başlamak için **Gönder** ' e tıklayın. 

Dizin **oluşturucular** listesinde Dizin oluşturucuyu tıklayarak portalda Dizin oluşturmayı izleyebilirsiniz. Belgeler yüklendikçe, tanımladığınız dizine ait belge sayısı artacaktır. Bazı durumlarda portal sayfasının en son güncelleştirmeleri seçmesi birkaç dakika sürer.

İlk belge yüklendikten hemen sonra Dizin sorgulamaya hazırdır. Bu görev için [Arama Gezgini](search-explorer.md) ' ni kullanabilirsiniz.

<a name="data-source-inputs"></a>

## <a name="data-source-inputs"></a>Veri kaynağı girişleri

**Veri Içeri aktarma** Sihirbazı, bir dış veri kaynağına yönelik bağlantı bilgilerini belirten kalıcı bir veri kaynağı nesnesi oluşturur. Veri kaynağı nesnesi yalnızca [Dizin oluşturucularla](search-indexer-overview.md) birlikte kullanılır ve aşağıdaki veri kaynakları için oluşturulabilir: 

* [Azure SQL](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md)
* [Azure Cosmos DB](search-howto-index-cosmosdb.md)
* [Azure Blob Depolama](search-howto-indexing-azure-blob-storage.md)
* [Azure Tablo depolama](search-howto-indexing-azure-tables.md) (bilişsel [arama](cognitive-search-concept-intro.md) işlem hatları için desteklenmez)

Yalnızca tek bir tablo, veritabanı görünümü veya eşdeğer veri yapısından içeri aktarabilirsiniz; ancak yapı hiyerarşik veya iç içe geçmiş alt yapılar içerebilir. Daha fazla bilgi için bkz. [karmaşık türleri Modelme](search-howto-complex-data-types.md).

Sihirbazı çalıştırmadan önce bu veri yapısını oluşturmanız gerekir ve içerik içermesi gerekir. **Veri alma** Sihirbazı 'nı boş bir veri kaynağı üzerinde çalıştırmayın.

|  Seçim | Açıklama |
| ---------- | ----------- |
| **Mevcut veri kaynağı** |Arama hizmetinizde önceden tanımlanmış dizin oluşturuculara sahipseniz başka bir içeri aktarma için var olan bir veri kaynağı seçebilirsiniz. Azure Search, veri kaynağı nesneleri yalnızca dizin oluşturucular tarafından kullanılır. Programlı bir şekilde veya **veri alma** Sihirbazı aracılığıyla bir veri kaynağı nesnesi oluşturabilirsiniz.|
| **Örnekler**| Azure Search, Azure Search istekleri içeri ve içeri aktarma hakkında bilgi edinmek için kullanabileceğiniz ücretsiz bir Global Azure SQL veritabanı barındırır. Bkz [. hızlı başlangıç: Bir izlenecek yol için Portal araçlarını](search-get-started-portal.md) kullanarak içeri aktarın, Dizin yapın ve sorgulayın. |
| **Azure SQL Veritabanı** |Hizmet adı, okuma iznine sahip bir veritabanı kullanıcısının kimlik bilgileri ve veritabanı adı, sayfa üzerinde ya da ADO.NET bağlantı dizesi aracılığıyla belirtilebilir. Özellikleri görüntülemek veya özelleştirmek için bağlantı dizesini seçin. <br/><br/>Sayfada satır kümesini sağlayan tablo veya görünüm belirtilmelidir. Bu seçenek bağlantı başarılı olduktan sonra görünür ve bir seçim yapmanızı sağlayan açılır listeyi gösterir. |
| **Azure VM’lerde SQL Server** |Tam hizmet adını, kullanıcı KIMLIĞINI ve parolayı ve bir bağlantı dizesi olarak veritabanını belirtin. Bu veri kaynağını kullanmak için bağlantıyı şifreleyen yerel depoya daha önce bir sertifika yüklemiş olmanız gerekir. Yönergeler için bkz. [Azure Search ile SQL VM bağlantısı](search-howto-connecting-azure-sql-iaas-to-azure-search-using-indexers.md). <br/><br/>Sayfada satır kümesini sağlayan tablo veya görünüm belirtilmelidir. Bu seçenek bağlantı başarılı olduktan sonra görünür ve bir seçim yapmanızı sağlayan açılır listeyi gösterir. |
| **Cosmos DB** |Hesap, veritabanı ve bağlantı gereklidir. Koleksiyondaki tüm belgeler dizine dahil edilir. Satır kümesini düzleştirmek veya filtrelemek için bir sorgu tanımlayabilir veya sorguyu boş bırakabilirsiniz. Bu sihirbazda bir sorgu gerekli değildir.|
| **Azure Blob Depolama** |Depolama hesabı ve bir kapsayıcı gereklidir. İsteğe bağlı olarak, gruplandırma amacıyla blob adlarından önce bir sanal adlandırma kuralı varsa adın sanal dizin kısmını kapsayıcı altındaki bir klasör olarak belirtebilirsiniz. Daha fazla bilgi için bkz. [Blob Depolama Dizini Oluşturma](search-howto-indexing-azure-blob-storage.md). |
| **Azure Tablo Depolama** |Depolama hesabı ve bir tablo adı gereklidir. İsteğe bağlı olarak, tabloların bir alt kümesini almak için sorgu belirtebilirsiniz. Daha fazla bilgi için bkz. [Tablo Depolama Dizini Oluşturma](search-howto-indexing-azure-tables.md). |


<a name="index-definition"></a>

## <a name="index-attributes"></a>Dizin öznitelikleri

**Veri Içeri aktarma** Sihirbazı, giriş veri kaynağından elde edilen belgelerle doldurulacak bir dizin oluşturur. 

İşlevsel dizin için aşağıdaki öğelerin tanımlandığından emin olun.

1. Bir alan, her belgeyi benzersiz bir şekilde tanımlamak için kullanılan bir **anahtar**olarak işaretlenmelidir. **Anahtar** *Edm. String*olmalıdır. 

   Alan değerleri boşluk veya tire içeriyorsa, bu karakterlerin doğrulama denetimini bastırmak için, **Dizin Oluşturucu oluştur** adımında, **Gelişmiş Seçenekler**altında **Base-64 kodlama anahtarı** seçeneğini ayarlamanız gerekir.

1. Her alan için Dizin özniteliklerini ayarlayın. Öznitelik yok ' u seçerseniz, gerekli anahtar alanı dışında dizininiz aslında boştur. En azından, her alan için bu özniteliklerin bir veya daha fazlasını seçin.
   
   + **Alınabilir** , arama sonuçlarında alanı geri döndürür. Arama sonuçlarına içerik sağlayan her alan bu özniteliğe sahip olmalıdır. Bu alanın ayarlanması, etkin dizin boyutunu etkilemez.
   + **Filtrelenebilir** , alana filtre ifadelerinde başvurulmak için izin verir. **$Filter** ifadesinde kullanılan her alan bu özniteliğe sahip olmalıdır. Filtre ifadeleri tam eşleşmeler içindir. Metin dizeleri değişmeden kaldığı için, tam içeriğe uyum sağlamak için ek depolama alanı gereklidir.
   + **Aranabilir** tam metin aramasını sunar. Serbest form sorgularında veya sorgu ifadelerinde kullanılan her alan bu özniteliğe sahip olmalıdır. Ters çevrilen dizinler, **aranabilir**olarak işaretlediğiniz her alan için oluşturulur.

1. İsteğe bağlı olarak, bu özniteliği gereken şekilde ayarlayın:

   + **Sıralanabilir** , alanın bir sıralama içinde kullanılmasına izin verir. **$OrderBy** ifadesinde kullanılan her alan bu özniteliğe sahip olmalıdır.
   + Çok **yönlü tablo** , çok yönlü gezinme için alanı sunar. Yalnızca **Filterable** olarak işaretlenen alanlar, çok **yönlü tablo**olarak işaretlenebilir.

1. Dil ile Gelişmiş dizin oluşturma ve sorgulama istiyorsanız **çözümleyici** ayarlayın. Varsayılan değer *Standart Lucene* ' dir, ancak düzensiz ad ve fiil formlarını çözme gibi gelişmiş sözcük Işleme için Microsoft 'un çözümleyicisini kullanmak Istiyorsanız *Microsoft English* ' i seçebilirsiniz.

   + **Çözümleyici** listesini etkinleştirmek için **aranabilir** ' i seçin.
   + Listede sunulan bir çözümleyici seçin. 
   
   Şu anda yalnızca dil çözümleyicileri belirtilebilir. Özel bir çözümleyici veya Keyword, Pattern gibi dil dışı bir çözümleyici kullanılması kod gerektirir. Çözümleyiciler hakkında daha fazla bilgi için bkz. [birden çok dildeki belgeler için dizin oluşturma](search-language-support.md).

1. Seçili alanlar üzerinde tür ön sorgu önerilerini etkinleştirmek için **öneri aracı** onay kutusunu seçin.


## <a name="next-steps"></a>Sonraki adımlar
Dizin oluşturucular hakkında daha fazla bilgi için bu bağlantıları gözden geçirin:

* [Azure SQL Veritabanı dizini oluşturma](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md)
* [Azure Cosmos DB’yi dizine ekleme](search-howto-index-cosmosdb.md)
* [Blob Depolama dizini oluşturma](search-howto-indexing-azure-blob-storage.md)
* [Tablo Depolama dizini oluşturma](search-howto-indexing-azure-tables.md)