---
title: Not defterlerini Azure Cosmos DB Not defteri galerisine yayımlama
description: Genel Galeriden not defterlerini indirmeyi, bunları düzenlemenizi ve kendi not defterlerinizi galeriye yayımlamayı öğrenin.
author: deborahc
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.topic: how-to
ms.date: 03/02/2021
ms.author: dech
ms.openlocfilehash: 58ae61bc9e1736b13bb1802e2f39d5ada045cb6a
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102039334"
---
# <a name="publish-notebooks-to-the-azure-cosmos-db-notebook-gallery"></a>Not defterlerini Azure Cosmos DB Not defteri galerisine yayımlama
[!INCLUDE[appliesto-sql-api](includes/appliesto-sql-api.md)]

Azure Cosmos DB yerleşik jupi Not defterleri, Azure portal Azure Cosmos DB hesaplarınızla doğrudan tümleşiktir. Bu not defterlerini kullanarak, Azure portal verilerinizi çözümleyebilir ve görselleştirebilirsiniz. Azure Cosmos DB için yerleşik Not defterleri Şu anda [birçok bölgede](https://azure.microsoft.com/global-infrastructure/services/?products=cosmos-db&regions=all)kullanılabilir. Not defterlerini kullanmak için [Yeni bir Cosmos hesabı oluşturun](create-cosmosdb-resources-portal.md) veya bu bölgelerden birindeki [mevcut bir hesapta not defterlerini etkinleştirin](enable-notebooks.md) .

Azure portal Not defteri ortamında Azure Cosmos DB ekibi tarafından yayımlanan bazı örnekler vardır. Ayrıca, kendi not defterlerinizi yayımlayabileceğiniz ve paylaşabileceğiniz genel bir Galerisi de vardır. Bir not defteri galeriye yayımlandıktan sonra, tüm Azure Cosmos DB kullanıcıların görüntülemesi ve kullanması için kullanılabilir. Bu makalede, genel galerideki not defterlerini kullanmayı ve not defterinizi galeride yayımlamayı öğreneceksiniz.

## <a name="download-a-notebook-from-the-gallery"></a>Galeriden bir not defteri indirin

Galerideki not defterlerini görüntülemek ve kendi Not defterleri çalışma alanınıza indirmek için aşağıdaki adımları kullanın:

1. [Azure Portal](https://portal.azure.com/) oturum açın ve Not defteri ortamıyla etkinleştirilen Azure Cosmos DB hesabınıza gidin.

1. **Veri Gezgini**  >  **Not defteri**  >  **Galerisi**  >  **Genel Galeri** sekmesine gidin.

1. Galerideki not defterlerini görüntülemeden veya yayımlamadan önce [kullanım kodlarını](https://azure.microsoft.com/support/legal/cosmos-db-public-gallery-code-of-conduct/)  kabul edin. Kullanım kodu, her kullanıcı için abonelik düzeyine göre günlüğe kaydedilir. Farklı bir aboneliğe geçtiğinizde, galeriye erişmeden önce yeniden kabul etmelisiniz. Kullanım kodlarını kabul etmek için onay kutusunu seçin ve **devam edin**:

   :::image type="content" source="./media/publish-notebook-gallery/view-public-gallery.png" alt-text="Not defterleri genel galerisine gidin ve kullanım kodlarını kabul edin.":::

1. Daha sonra, Sık Kullanılanlar sekmesine belirli bir not defteri ekleyebilir veya dosyayı indirebilirsiniz. Bir not defteri indirdiğinizde, bu, çalıştırabileceğiniz veya düzenleyebileceğiniz Not defteri çalışma alanınıza kopyalanırlar.

   :::image type="content" source="./media/publish-notebook-gallery/download-notebook-gallery.png" alt-text="Galeriden çalışma alanınıza bir not defteri indirin.":::

## <a name="publish-a-notebook-to-the-gallery"></a>Galeriye Not defteri yayımlama

Kendi not defterlerinizi oluştururken veya mevcut not defterlerini farklı bir senaryoya sığacak şekilde düzenlediğinizde, bunları Galeri 'de yayımlamak isteyebilirsiniz. Bir not defteri galeriye yayımlandıktan sonra, diğer kullanıcılar buna erişebilir. Şu anda kullanılabilir olan not defterlerini görüntülemek için [Not defteri örnekleri galerisini](https://cosmos.azure.com/gallery.html) inceleyebilirsiniz.

Bir not defteri yayımlamak için aşağıdaki adımları kullanın:

1. [Azure Portal](https://portal.azure.com/) oturum açın ve Not defteri ortamıyla etkinleştirilen Azure Cosmos DB hesabınıza gidin.

1. Not Defterim **Veri Gezgini**  >  **Not** defterleri  >   sekmesine gidin.

1. Yayımlamak istediğiniz not defterine gidin. Komut çubuğunda **Kaydet** düğmesini seçin ve ardından **Galeriye Yayımla**' yı seçin:

   :::image type="content" source="./media/publish-notebook-gallery/choose-notebook-publish-option-2.png" alt-text="Galeriye yayımlanacak bir not defteri seçin.":::

   **Galeriye Yayımla** seçeneğini de bulabilirsiniz **.** Not defteri adının yanındaki düğme:

   :::image type="content" source="./media/publish-notebook-gallery/choose-notebook-publish.png" alt-text="Galeriye yayımlanacak bir not defteri seçmek için başka bir yaklaşım.":::

1. **Galeriye yayınla** formunu aşağıdaki ayrıntılarla doldur:

   * **Ad:** Not defterinizi tanımlayacak kolay bir ad.
   * **Açıklama:**  Not defterinizin ne yaptığını kısa bir açıklama.
   * **Etiketler:** Etiketler isteğe bağlıdır ve bir anahtar sözcük tarafından arandığı zaman sonuçlara filtre uygulamak için kullanılır.
   * **Kapak resmi:** Not defteri yayımlandığında kapak sayfasında kullanılan bir görüntü. Aşağıdaki seçeneklerden birini belirleyebilirsiniz:
   * **Özel görüntü** -bilgisayarınızdan bir resim yükleyebilirsiniz. En boy oranına sahip bir görüntü dosyası seçin 256 x144.
   * **URL** -görüntünün bulunduğu genel olarak ERIŞILEBILEN bir URL sağlar.
   * **Ekran görüntüsü al** -açık not defterinizin bir ekran görüntüsü otomatik olarak alınır ve önizlemeye yüklenir.
   * Görüntü çıkışı olan ilk hücrenin **ilk görüntüleme çıktısını kullan** . Yalnızca markaşağı/metin görüntüleyen hücreler bir görüntüleme çıktısı olarak sayılmaz.

   :::image type="content" source="./media/publish-notebook-gallery/publish-notebook.png" alt-text="Galeriye Yayımla formunu doldurun.":::

   > [!NOTE]
   > **..** . Içinden **Galeriye yayınla** seçeneğini kullanıyorsanız düğmesini seçerseniz, tüm **kapak resmi** seçeneklerini görmezsiniz. Bunun nedeni Not defteri açık olmayabilir ve Azure Cosmos DB ekran görüntüsü almak veya ilk ekran çıktısına erişmek için erişime sahip olmaz.

1. Önizlemenin iyi göründüğünü doğrulayıp **Yayımla**' yı seçin. Bu not defteri yayımlandığında, Azure Cosmos DB Not defteri genel galerisinde görüntülenir. Yayımlamadan önce hassas verileri veya çıktıyı kaldırdığınızdan emin olun. Not defteri içeriği, yayımlamadan önce Microsoft ilkelerinin herhangi bir ihlali için taranır. Bu işlemin tamamlanabilmesi için genellikle birkaç saniye sürer. Herhangi bir ihlal bulunursa bildirim sekmesinde bir ileti görüntülenir. Not defteriniz herhangi bir ihlal bulursa yayımlanmayacak, ihlal edilen metni kaldırır ve yeniden yayımlayabilirsiniz.

   > [!NOTE]
   > Not defterleri genel galeriye yayımlandıktan sonra, tüm Azure Cosmos DB kullanıcılar bunları görüntüleyebilir. Erişim, abonelik başına veya kuruluş düzeyi başına sınırlı değildir.

1. Not defteri galeriye yayımlandıktan sonra, onu **yayımlandığım iş** sekmesinde görebilirsiniz. Ayrıca, ortak Galeriden yayımladığınız not defterlerini kaldırabilir veya silebilirsiniz.

1. Ayrıca, kullanım kodlarını ihlal eden bir not defteri de bildirebilirsiniz. Bir ihlal bulunursa Cosmos DB, Not defterini otomatik olarak Galeriden kaldırır. Bir not defteri kaldırılırsa, kullanıcılar onu kaldırılan **notum ile yayınlanan iş** sekmesinde görebilir. Bir not defteri raporlamak için **Veri Gezgini**  >  **Not defterleri**  >  **Galerisi**  >  **Genel Galeri** sekmesine gidin. raporlamak istediğiniz Not defterini açın, sağ köşedeki **Yardım** düğmesine gelin ve **uygunsuz kullanım bildir**' i seçin.

   :::image type="content" source="./media/publish-notebook-gallery/report-notebook-violation.png" alt-text="Kullanım kodlarını ihlal eden bir not defteri bildirin.":::

## <a name="next-steps"></a>Sonraki adımlar

* [Jupyıter Not defterlerinin Azure Cosmos DB](cosmosdb-jupyter-notebooks.md) avantajları hakkında bilgi edinin
* [Python Not defteri özelliklerini ve komutlarını kullanma](use-python-notebook-features-and-commands.md)
* [C# Not defteri özelliklerini ve komutlarını kullanma](use-csharp-notebook-features-and-commands.md)
* [GitHub deposundan not defterlerini içeri aktarma](import-github-notebooks.md)
