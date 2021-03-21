---
title: Güvenlik ele almak için Azure Sentinel ile not defterlerini kullanma
description: Bu makalede, Azure Sentinel arama özellikleri ile Not defterlerinin nasıl kullanılacağı açıklanır.
services: sentinel
author: yelevin
ms.author: yelevin
ms.assetid: 1721d0da-c91e-4c96-82de-5c7458df566b
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.topic: conceptual
ms.custom: mvc
ms.date: 09/06/2020
ms.openlocfilehash: 43d7a697b3cb013a73a0b14db8ec1758244ae3b9
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "97092202"
---
# <a name="use-jupyter-notebook-to-hunt-for-security-threats"></a>Güvenlik tehditlerini avlamak için Jupyter Notebook kullanma

Azure Sentinel 'in temeli veri deposudur; yüksek performanslı sorgulamayı, dinamik şemayı birleştirir ve çok büyük veri birimlerine ölçeklendirir. Azure portal ve tüm Azure Sentinel araçları, bu veri deposuna erişmek için ortak bir API kullanır. Aynı API, [jupi](https://jupyter.org/) Not defterleri ve Python gibi dış araçlar için de kullanılabilir. Portalda birçok ortak görev gerçekleştirilebilirken, Jupyıter bu verilerle yapabileceklerinizi kapsamını genişletir. Makine öğrenimi, görselleştirme ve veri analizi için çok büyük bir kitaplık koleksiyonuyla tam programlama ile birleşir. Bu öznitelikler, Jupa 'nın güvenlik araştırması ve araştırma için etkileyici bir araç haline getirir.

![Örnek Not defteri](./media/notebooks/sentinel-notebooks-map.png)

Jupi deneyimini Azure portal ile tümleştirdik, böylece verilerinizi çözümlemek için not defterlerini oluşturup çalıştırabilirsiniz. *Ktomagic* kitaplığı, Azure Sentinel 'ten sorgular almanıza ve bunları doğrudan bir not defteri içinde çalıştırmanıza olanak tanıyan tutkalla 'yi sağlar. Sorgular [kusto sorgu dilini](https://kusto.azurewebsites.net/docs/kusto/query/index.html)kullanır. Microsoft 'un Güvenlik analistleri tarafından geliştirilen birkaç not defteri, Azure Sentinel ile paketlenmiştir. Bu not defterlerinden bazıları belirli bir senaryo için oluşturulmuştur ve olduğu gibi kullanılabilir. Diğerleri, kendi Not defterlerinizde kullanmak üzere kopyalayabileceğiniz veya bunları uyarlayabileceğiniz teknikleri ve özellikleri göstermek için örnek olarak tasarlanmıştır. Diğer Not defterleri de Azure Sentinel Community GitHub 'dan içeri aktarılabilir.

Tümleşik Jupyıter deneyimi, not defterlerini depolamak, paylaşmak ve yürütmek için [Azure Notebooks](https://notebooks.azure.com/) kullanır. Bilgisayarınızda bir Python ortamınız ve jupi varsa veya Azure Databricks gibi diğer JupterHub ortamlarında bu not defterlerini yerel olarak da çalıştırabilirsiniz.

Not defterlerinin iki bileşeni vardır:

- Sorgu ve kod girip çalıştırdığınız ve yürütme sonuçlarının görüntülendiği tarayıcı tabanlı arabirim.
- Kodu ayrıştırmaktan ve yürütmeden sorumlu bir *çekirdek* .

Azure Sentinel Not defteri 'nin çekirdeği bir Azure sanal makinesinde (VM) çalışır. Not defterleriniz karmaşık makine öğrenimi modelleri içeriyorsa, daha güçlü sanal makinelerden yararlanmak için çeşitli lisans seçenekleri mevcuttur.

Azure Sentinel Not defterleri, Pandas, Matplotlib, bokeh ve diğerleri gibi birçok popüler Python kitaplıklarını kullanır. Arasından seçim yapabileceğiniz çok sayıda başka Python paketi vardır:

- Görselleştirmeler ve grafikler
- Veri işleme ve analiz
- İstatistikler ve sayısal bilgi işlem
- Makine öğrenmesi ve derin öğrenme

Ayrıca, [msticter](https://github.com/Microsoft/msticpy/)adlı bir pakette bazı açık kaynaklı jupi güvenlik araçları da yayımladık. Bu paket, dahil edilen birçok Not defteri içinde kullanılır. Msticticizme araçları, araştırma ve araştırma için Not defteri oluşturma konusunda yardımcı olmak üzere tasarlanmıştır ve yeni özellikler ve geliştirmeler üzerinde etkin bir şekilde çalışıyoruz.

[Azure Sentinel Community GitHub deposu](https://github.com/Azure/Azure-Sentinel) , Microsoft tarafından oluşturulan veya topluluk tarafından katkıda bulunulan, gelecek Azure Sentinel Not defterlerinin konumudur.

Not defterlerini kullanmak için, önce bir Azure Machine Learning (ML) çalışma alanı oluşturmanız gerekir.

## <a name="create-an-azure-ml-workspace"></a>Azure ML çalışma alanı oluşturma

1. Azure Portal **Azure Sentinel**  >  **tehdit yönetimi**  >  **Not defterleri** ' ne gidin ve ardından **Not defterini Başlat**' ı seçin.

    > [!div class="mx-imgBorder"]
    > ![Azure ML çalışma alanını başlatmak için Not defterini Başlat](./media/notebooks/sentinel-notebooks-launch.png)

1. **AzureML çalışma alanı** altında **Yeni oluştur**' u seçin.

    > [!div class="mx-imgBorder"]
    > ![çalışma alanı oluştur](./media/notebooks/sentinel-notebooks-azureml-create.png)

1. **Machine Learning** sayfasında, aşağıdaki bilgileri sağlayın ve ardından **gözden geçir + oluştur**' u seçin.

    |Alan|Açıklama|
    |--|--|
    |Abonelik|Kullanmak istediğiniz Azure aboneliğini seçin.|
    |Kaynak grubu|Aboneliğinizde mevcut kaynak gruplarından birini seçin veya bir ad girerek yeni bir kaynak grubu oluşturun. Kaynak grubu, bir Azure çözümü için ilgili kaynakları barındırır. Bu örnekte, **AzureMLRG** kullanacağız.|
    |Çalışma alanı adı|Çalışma alanınızı tanımlayan benzersiz bir ad girin. Bu örnekte, **testworkspace1** kullanacağız. Adlar, kaynak grubu genelinde benzersiz olmalıdır. Başkaları tarafından oluşturulan çalışma alanlarını birbirinden ayırmak ve geri çekmek için kolay bir ad kullanın.|
    |Region|Çalışma alanınızı oluşturmak için kullanıcılarınıza en yakın konumu ve veri kaynaklarını seçin.|
    |Çalışma alanı sürümü|Bu örnekte, çalışma alanı türü olarak **temel** ' yı seçin. Çalışma alanı türü (temel & kurumsal), erişim ve fiyatlandırmaya sahip olduğunuz özellikleri belirler.|

    > [!div class="mx-imgBorder"]
    > ![çalışma alanı ayrıntıları sağla](./media/notebooks/sentinel-notebooks-azureml-basics.png)

1. Bilgileri gözden geçirin, doğru olduğunu doğrulayın ve ardından **Oluştur** ' u seçerek çalışma alanınızın dağıtımını başlatın.

    > [!div class="mx-imgBorder"]
    > ![çalışma alanı ayrıntılarını gözden geçirme](./media/notebooks/sentinel-notebooks-azureml-review.png)

    **Genel bakış** sayfasında geçerli dağıtım durumu görüntülendiği zaman, çalışma alanınızı bulutta oluşturmak birkaç dakika sürebilir.

    > [!div class="mx-imgBorder"]
    > ![çalışma alanı dağıtımı](./media/notebooks/sentinel-notebooks-azureml-deploy.png)

Dağıtımınız tamamlandıktan sonra, yeni Azure ML çalışma alanınızda Not defterleri başlatabilirsiniz.

> [!div class="mx-imgBorder"]
> ![çalışma alanı dağıtımı başarılı oldu](./media/notebooks/sentinel-notebooks-azureml-complete.png)

## <a name="launch-a-notebook-using-your-azure-ml-workspace"></a>Azure ML çalışma alanınızı kullanarak bir not defteri başlatın

1. Azure Portal **Azure Sentinel**  >  **tehdit yönetimi**  >  **Not defterleri**' ne giderek Azure Sentinel 'in sağladığı not defterlerini görebilirsiniz.

    > [!TIP]
    > **Kılavuzlar & geri bildirimde bulunan kılavuzlar** ' ı seçerek not defterlerine ek yardım ve yönergeler içeren bir bölme açın.
    > ![Not defteri kılavuzlarını görüntüle](./media/notebooks/sentinel-azure-notebooks-guides.png)

1. Açıklamalarını, gerekli veri türlerini ve veri kaynaklarını görüntülemek için ayrı ayrı not defterlerini seçin.

    > [!div class="mx-imgBorder"]
    > ![Not defteri ayrıntılarını görüntüle](./media/notebooks/sentinel-azure-notebooks-view.png)

1. Kullanmak istediğiniz Not defterini seçin ve ardından not defterini kopyalayıp Azure Sentinel çalışma alanınıza bağlanan yeni bir Azure Notebooks projesine kopyalamak ve yapılandırmak için **Not defterini Başlat** ' ı seçin. İşlem tamamlandığında, çalıştırmak için Not defteri Azure Notebooks içinde açılır.

    > [!div class="mx-imgBorder"]
    > ![Not defteri seçin](./media/notebooks/sentinel-azure-notebooks-select.png)

1. AzureML çalışma alanı altında, Azure ML çalışma alanınızı seçin ve ardından **Başlat**' ı seçin.

    > [!div class="mx-imgBorder"]
    > ![Not defterini Başlat](./media/notebooks/sentinel-azure-notebooks-launch.png)

1. Bir işlem örneği seçin. İşlem örneğiniz yoksa şunları yapın:
    1. **Yeni işlem örneği** sihirbazını başlatmak için artı işaretini (+) seçin.

        > [!div class="mx-imgBorder"]
        > ![işlem örneği Sihirbazını Başlat](./media/notebooks/sentinel-azure-notebooks-compute-wizard.png)

    1. **Yeni işlem örneği** sayfasında, gerekli bilgileri sağlayın ve **Oluştur**' u seçin.

        > [!div class="mx-imgBorder"]
        > ![işlem örneği oluştur](./media/notebooks/sentinel-azure-notebooks-compute-create.png)

1. Not defteri sunucunuz oluşturulduktan sonra, her bir hücrede kod yürütme simgesini seçerek not defterlerinde kodu çalıştırın.

    > [!div class="mx-imgBorder"]
    > ![Not defterini çalıştır](./media/notebooks/sentinel-azure-notebooks-run.png)

Öneri

- Azure Sentinel 'de verileri sorgulamaya yönelik hızlı bir giriş için [Azure ML not defterlerine Başlarken ve Azure Sentinel](https://github.com/Azure/Azure-Sentinel-Notebooks/blob/master/A%20Getting%20Started%20Guide%20For%20Azure%20Sentinel%20ML%20Notebooks.ipynb) kılavuzuna bakın.

- [**Örnek Not defterleri**](https://github.com/Azure/Azure-Sentinel-Notebooks/tree/master/Sample-Notebooks) GitHub alt klasöründe ek örnek Not defterleri bulacaksınız. Bu örnek Not defterleri verilerle birlikte kaydedildiğinden, amaçlanan çıktıyı görmeniz daha kolay olur. Bu not defterlerini [nbviewer](https://nbviewer.jupyter.org/)'da görüntülemeniz önerilir.

- [**HOWTOs**](https://github.com/Azure/Azure-Sentinel-Notebooks/tree/master/HowTos) GitHub alt klasörü, örnek olarak açıklandığı gibi not defterlerini içerir, örneğin: varsayılan Python sürümünüzü ayarlama, dsvm yapılandırma, bir not defterinden Azure Sentinel yer işaretlerini oluşturma ve diğer konular.

Sağlanan Not defterleri, kendi Not defterlerinizin geliştirilmesinde kullanabileceğiniz hem faydalı araçlar, hem de çizimler ve kod örnekleri olarak hazırlanmıştır.

Öneriler, özellikler için istekler, katkıda bulunulan Not defterleri, hata raporları ya da geliştirmeler ve mevcut not defterlerine ekler hakkında geri bildirimde bulunun. Bir sorun veya çatal oluşturmak için [Azure Sentinel Community GitHub](https://github.com/Azure/Azure-Sentinel) ' a gidin ve bir katkıyı karşıya yükleyin.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Azure Sentinel 'de Jupyter Notebook kullanmaya başlama hakkında daha fazla öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Tehditler için proaktif araya](hunting.md)
- [Avlarken ilginç bilgileri kaydetmek için yer işaretlerini kullanın](bookmarks.md)
