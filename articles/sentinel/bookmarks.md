---
title: Azure Sentinel'de veri incelemeleri için av yer imlerini kullanma
description: Bu makalede, verileri izlemek için Azure Sentinel av yer imleri nasıl kullanılacağı açıklanmaktadır.
author: yelevin
ms.author: yelevin
manager: rkarlin
ms.assetid: 320ccdad-8767-41f3-b083-0bc48f1eeb37
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.topic: conceptual
ms.custom: mvc
ms.date: 10/24/2019
ms.openlocfilehash: eacc104bf7157732f91507f631bcda2cd166bfb3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77588697"
---
# <a name="keep-track-of-data-during-hunting-with-azure-sentinel"></a>Azure Sentinel ile avlanma sırasında verileri takip edin

Tehdit avcılık genellikle kötü niyetli davranış kanıtı arayan günlük veri dağları gözden gerektirir. Bu süreçte, araştırmacılar hatırlamak, yeniden ziyaret etmek ve potansiyel hipotezleri doğrulamak ve bir uzlaşmanın tüm hikayesini anlamak bir parçası olarak analiz etmek istedikleri olayları bulurlar.

Azure Sentinel'de yer imleri avlamak, **Azure Sentinel - Logs'da**çalıştırdığınız sorguları ve alakalı olduğunu gördüğünüz sorgu sonuçlarını koruyarak bunu yapmanıza yardımcı olur. Ayrıca bağlamsal gözlemlerinizi kaydedebilir ve notlar ve etiketler ekleyerek bulgularınıza başvuruyapabilirsiniz. Yer işaretli veriler, kolay işbirliği için siz ve takım arkadaşlarınız tarafından görülebilir.

Yer işaretli verilerinizi **Av** bölmesinin **Yer İşaretleri** sekmesinde istediğiniz zaman yeniden ziyaret edebilirsiniz. Geçerli araştırmanız için belirli verileri hızla bulmak için filtreleme ve arama seçeneklerini kullanabilirsiniz. Alternatif olarak, yer işaretli verilerinizi Doğrudan Log Analytics çalışma alanınızdaki **HuntingBookmark** tablosunda görüntüleyebilirsiniz. Örnek:

> [!div class="mx-imgBorder"]
> ![HuntingBookmark tablosunu görüntüle](./media/bookmarks/bookmark-table.png)

Tablodan yer imlerini görüntülemek, yer işaretli verileri diğer veri kaynaklarıyla filtrelemenize, özetlemenize ve birleştirmenize olanak sağlayarak doğrulayıcı kanıt aramanızı kolaylaştırır.

Şu anda önizlemede, günlüklerinizde avlanma sırasında, birkaç tıklamayla acilen ele alınması gereken bir şey bulursanız, bir yer imi oluşturabilir ve bir olaya tanıtabilir veya yer imi mevcut bir olaya ekleyebilirsiniz. Olaylar hakkında daha fazla bilgi için [Bkz. Öğretici: Azure Sentinel ile ilgili olayları incele.](tutorial-investigate-cases.md) 

Önizlemede ayrıca, yer imi ayrıntılarından **Araştır'ı** tıklatarak yer işaretli verilerinizi görselleştirebilirsiniz. Bu, etkileşimli bir varlık grafiği diyagramı ve zaman çizelgesi kullanarak bulgularınızı görüntüleyebileceğiniz, araştırabileceğiniz ve görsel olarak iletebileceğiniz araştırma deneyimini başlatır.

## <a name="add-a-bookmark"></a>Bir yer işareti ekleme

1. Azure portalında, şüpheli ve anormal davranışlar için sorguları çalıştırmak için **Sentinel** > **Threat management** > **Hunting'e** gidin.

2. Avcılık sorgularından birini seçin ve sağda, avcılık sorgu ayrıntılarında **Sorguyı Çalıştır'ı**seçin. 

3. **Sorgu sonuçlarını görüntüle'yi**seçin. Örnek:
    
    > [!div class="mx-imgBorder"]
    > ![Azure Sentinel avının sorgu sonuçlarını görüntüleme](./media/bookmarks/new-processes-observed-example.png)
    
    Bu eylem, **Günlükler** bölmesinde sorgu sonuçlarını açar.

4. Günlük sorgusu sonuç listesinden, ilginç bulduğunuz bilgileri içeren bir veya daha fazla satır seçmek için onay kutularını kullanın.

5. **Yer Imi Ekle'yi**seçin :
    
    > [!div class="mx-imgBorder"]
    > ![Sorguya avlanma yer imi ekleme](./media/bookmarks/add-hunting-bookmark.png)

6. Sağda, **yer imi ekle** bölmesinde isteğe bağlı olarak, öğeyle ilgili ilginç olan şeyi belirlemenize yardımcı olmak için yer imi adını güncelleştirin, etiket ve notlar ekleyin.

7. Sorgu **Bilgileri** bölümünde, **Hesap**, **Ana Bilgisayar**ve **IP adresi** varlık türlerinin sorgu sonuçlarından bilgi ayıklamak için açılır kutuları kullanın. Bu eylem, seçili varlık türünü sorgu sonucundan belirli bir sütunla eşler. Örnek:
    
    > [!div class="mx-imgBorder"]
    > ![Av yer imi için harita varlık türleri](./media/bookmarks/map-entity-types-bookmark.png)
    
    Araştırma grafiğindeki yer işaretini görüntülemek için (şu anda önizlemede), **Hesap**, **Ana Bilgisayar**veya IP **adresi**olan en az bir varlık türünü eşlemelisiniz. 

5. Değişikliklerinizi işlemek ve yer imi eklemek için **Kaydet'i** tıklatın. Tüm yer imi verileri diğer araştırmacılarla paylaşılır ve işbirlikçi bir soruşturma deneyimine doğru atılmış ilk adımdır.

 
> [!NOTE]
> Bu bölme Azure Sentinel'den açıldığında günlük sorgusu sonuçları yer imlerini destekler. Örneğin, gezinti çubuğundan **Genel** > **Günlükler'i** seçin, soruşturma grafiğindeki olay bağlantılarını seçin veya bir olayın tüm ayrıntılarından bir uyarı kimliği (şu anda önizlemede) bir uyarı kimliği seçin. **Günlükler** bölmesi doğrudan Azure Monitor gibi diğer konumlardan açıldığında yer imleri oluşturamazsınız.

## <a name="view-and-update-bookmarks"></a>Yer imlerini görüntüleme ve güncelleştirme 

1. Azure portalında **Sentinel** > **Threat yönetimi** > **Avı'na**gidin. 

2. Yer **imleri** listesini görüntülemek için Yer İşaretleri sekmesini seçin.

3. Belirli bir yer işareti bulmanıza yardımcı olmak için arama kutusunu veya filtre seçeneklerini kullanın.

4. Tek tek yer imleri seçin ve sağ daki ayrıntılar bölmesinde yer imi ayrıntılarını görüntüleyin.

5. Değişikliklerinizi otomatik olarak kaydedilen gerektiği gibi yapın.

## <a name="exploring-bookmarks-in-the-investigation-graph"></a>Araştırma grafiğinde yer imlerini keşfetme

> [!IMPORTANT]
> Araştırma grafiğinde yer imleri ve araştırma grafiğinin kendisi şu anda genel önizlemede bulunmaktadır.
> Bu özellikler hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez.
> Daha fazla bilgi için Microsoft [Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.

1. Azure portalında **Sentinel** > **Threat yönetimi** > **Avcılık** > **Yer İşaretleri** sekmesine gidin ve araştırmak istediğiniz yer imi veya yer imlerini seçin.

2. Yer imi ayrıntılarında, en az bir varlığın eşlenindiğinden emin olun. Örneğin, **VARLıKLAR**için **IP,** **Makine**veya **Hesap**girişleri görürsünüz.

3. Soruşturma grafiğindeki yer imi görmek için **Araştır'ı** tıklatın.

Soruşturma grafiğini kullanma yönergeleri için [bkz.](tutorial-investigate-cases.md#use-the-investigation-graph-to-deep-dive)

## <a name="add-bookmarks-to-a-new-or-existing-incident"></a>Yeni veya varolan bir olaya yer imleri ekleme

> [!IMPORTANT]
> Yeni veya varolan bir olaya yer imleri eklemek şu anda genel önizlemede.
> Bu özellik bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez.
> Daha fazla bilgi için Microsoft [Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.

1. Azure portalında Sentinel**Threat yönetimi** > **Avcılık** > **Yer İşaretleri** **sekmesine** > gidin ve bir olaya eklemek istediğiniz yer imi veya yer imlerini seçin.

2. Komut çubuğundan **Olay eylemleri (Önizleme)** seçeneğini belirleyin:
    
    > [!div class="mx-imgBorder"]
    > ![Olaya yer imleri ekleme](./media/bookmarks/incident-actions.png)

3. Gerektiğinde **yeni olay oluşturun** veya **varolan olaya ekle'yi**seçin. Ardından:
    
    - Yeni bir olay için: İsteğe bağlı olarak olayın ayrıntılarını güncelleştirin ve ardından **Oluştur'u**seçin.
    - Varolan bir olaya yer işareti eklemek için: Bir olay seçin ve sonra **Ekle'yi**seçin. 

Olay daki yer işaretini görüntülemek için: **Sentinel** > **Tehdit yönetimi** > **Olaylarına** gidin ve olayı yer iminiz ile seçin. **Tüm ayrıntıları görüntüle'yi**seçin ve ardından **Yer İşaretleri** sekmesini seçin.

> [!TIP]
> Komut çubuğundaki **Olay eylemleri (Önizleme)** seçeneğine alternatif olarak, **yeni olay oluşturmak**için seçenekleri seçmek için bir veya daha fazla yer imi için bağlam menüsünü **(...**) kullanabilirsiniz , **varolan olaya ekle**ve **olaydan kaldır**. 

## <a name="view-bookmarked-data-in-logs"></a>Günlüklerde yer işaretli verileri görüntüleme

Yer imi sorgularını, sonuçları veya geçmişlerini görüntülemek için **Avcılık** > **Yer İşaretleri** sekmesinden yer imi'ni seçin ve ayrıntılar bölmesinde sağlanan bağlantıları kullanın: 

- **Günlükler** bölmesinde kaynak sorgusunu görüntülemek için **kaynak sorgusunu görüntüleyin.**

- Güncelleştirmeyi kimin yaptığını, güncelleştirilmiş değerleri ve güncelleştirmenin oluştuğu zamanı içeren tüm yer imi meta verilerini görmek için **yer imi günlüklerini görüntüleyin.**

Ayrıca, **Avcılık** > **Yer İşaretleri** sekmesindeki komut çubuğundan **Yer İşareti Günlükleri'ni** seçerek tüm yer imleri için ham yer imi verilerini görüntüleyebilirsiniz:

> [!div class="mx-imgBorder"]
> ![Yer Imi Günlükleri](./media/bookmarks/bookmark-logs.png)

Bu görünüm, ilişkili meta verilerle tüm yer imlerinizi gösterir. Aradığınız yer iminin en son sürümüne filtre lemek için [Anahtar Kelime Sorgu Dili](https://docs.microsoft.com/sharepoint/dev/general-development/keyword-query-language-kql-syntax-reference) (KQL) sorgularını kullanabilirsiniz.

> [!NOTE]
> Yer imi oluşturduğunuz saat ile **Yer İşaretleri** sekmesinde görüntülenme arasında önemli bir gecikme (dakika cinsinden ölçülür) olabilir.

## <a name="delete-a-bookmark"></a>Yer işaretini silme
 
1.  Azure portalında Sentinel**Threat yönetimi** > **Avcılık** > **Yer İşaretleri** **sekmesine** > gidin ve silmek istediğiniz yer imi veya yer imlerini seçin. 

2. Seçimlerinizi sağ tıklatın ve yer imi veya yer imlerini silme seçeneğini seçin. Örneğin, yalnızca bir yer imi seçtiyseniz **yer işaretini silin** ve iki yer imi seçtiyseniz **2 yer imini silin.**
    
Yer işaretini sevenin, **yer imi** sekmesindeki listeden kaldırır. Log Analytics çalışma alanınız için **HuntingBookmark** tablosu önceki yer imi girişlerini içermeye devam eder, ancak en son giriş **SoftDelete** değerini true olarak değiştirerek eski yer imlerini filtrelemesini kolaylaştırır. Yer imi siler, diğer yer imleri veya uyarılarla ilişkili araştırma deneyiminden herhangi bir varlığı kaldırmaz. 


## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Azure Sentinel'de yer imlerini kullanarak bir av araştırmasının nasıl yürütüldgerektiğini öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:


- [Proaktif tehditler için avı](hunting.md)
- [Otomatik avlanma kampanyaları yürütmek için dizüstü bilgisayarları kullanın](notebooks.md)
