---
title: Güvenlik avı için Azure Sentinel ile dizüstü bilgisayar kullanma
description: Bu makalede, Azure Sentinel avlanma özelliklerine sahip dizüstü bilgisayarların nasıl kullanılacağı açıklanmaktadır.
services: sentinel
author: yelevin
ms.author: yelevin
ms.assetid: 1721d0da-c91e-4c96-82de-5c7458df566b
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.topic: conceptual
ms.custom: mvc
ms.date: 11/25/2019
ms.openlocfilehash: 84b72a71ed2de910bce44b0c3f3309782f096680
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77581846"
---
# <a name="use-jupyter-notebooks-to-hunt-for-security-threats"></a>Güvenlik tehditleri için avlanmak için Jupyter dizüstü bilgisayarları kullanın

Azure Sentinel'in temeli veri deposudur; yüksek performanslı sorgulama, dinamik şema ve büyük veri hacimleri ölçeklendirmeleri birleştirir. Azure portalı ve tüm Azure Sentinel araçları, bu veri deposuna erişmek için ortak bir API kullanır. Aynı API, [Jupyter](https://jupyter.org/) dizüstü bilgisayarlar ve Python gibi harici araçlar için de kullanılabilir. Portalda birçok genel görev gerçekleştirilebilse de, Jupyter bu verilerle yapabileceklerin kapsamını genişletir. Tam programlanabilirliği makine öğrenimi, görselleştirme ve veri analizi için büyük bir kütüphane koleksiyonuyla birleştirir. Bu özellikler Jupyter'ı güvenlik soruşturması ve avcılık için zorlayıcı bir araç haline getirsin.

![örnek not defteri](./media/notebooks/sentinel-notebooks-map.png)

Jupyter deneyimini Azure portalına entegre ederek verilerinizi analiz etmek için dizüstü bilgisayarlar oluşturmanızı ve çalıştırmanızı kolaylaştırdık. *Kqlmagic* kitaplığı, Azure Sentinel'den sorguları alıp doğrudan bir not defterinin içinde çalıştırmanızı sağlayan tutkal sağlar. Sorgular [Kusto Sorgu Dili](https://kusto.azurewebsites.net/docs/query/index.html)kullanın. Microsoft'un bazı güvenlik analistleri tarafından geliştirilen çeşitli dizüstü bilgisayarlar Azure Sentinel ile birlikte paketlenir. Bu not defterlerinden bazıları belirli bir senaryo için oluşturulmuştur ve olduğu gibi kullanılabilir. Diğerleri, kopyalayabildiğiniz veya kendi not defterlerinizde kullanılmak üzere uyarlanabileceğiniz teknikleri ve özellikleri göstermek için örnek olarak tasarlanmıştır. Diğer dizüstü bilgisayarlar da Azure Sentinel topluluğu GitHub'dan içe aktarılabilir.

Tümleşik Jupyter deneyimi, dizüstü bilgisayarları depolamak, paylaşmak ve yürütmek için [Azure Not Defterlerini](https://notebooks.azure.com/) kullanır. Bilgisayarınızda Python ortamı ve Jupyter varsa veya Azure Databricks gibi diğer JupterHub ortamlarında bu not defterlerini yerel olarak da çalıştırabilirsiniz.

Not defterlerinin iki bileşeni vardır:

- Sorguları ve kodu girdiğiniz ve çalıştırdığınız ve yürütme sonuçlarının görüntülendiği tarayıcı tabanlı arabirim.
- Kodun kendisini ayrıştırma ve yürütmeden sorumlu bir *çekirdek.* 

Azure Not Defterlerinde varsayılan olarak bu çekirdek Azure *Boş Bulut İşlemi ve Depolama'da*çalışır. Dizüstü bilgisayarlarınız karmaşık makine öğrenimi modelleri veya görselleştirmeler içeriyorsa, [Data Science Virtual Machines](https://azure.microsoft.com/services/virtual-machines/data-science-virtual-machines/) (DSVM) gibi daha güçlü ve özel bilgi işlem kaynaklarını kullanmayı düşünün. Hesabınızdaki defterler, paylaşmayı seçmediğiniz sürece gizli tutulur.

Azure Sentinel dizüstü bilgisayarlar pandalar, matplotlib, bokeh ve diğerleri gibi birçok popüler Python kitaplıklarını kullanır. Aralarından seçim yapabileceğiniz birçok Python paketi vardır:

- Görselleştirmeler ve grafikler
- Veri işleme ve analizi
- İstatistik ler ve sayısal bilgi işlem
- Makine öğrenimi ve derin öğrenme

Ayrıca [msticpy](https://github.com/Microsoft/msticpy/)adlı bir paket içinde bazı açık kaynak Jupyter güvenlik araçları yayımladık. Bu paket, dahil edilen dizüstü bilgisayarların çoğunda kullanılır. Msticpy araçları özellikle avcılık ve araştırma için dizüstü bilgisayarlar oluşturmaya yardımcı olmak için tasarlanmıştır ve yeni özellikler ve iyileştirmeler üzerinde aktif olarak çalışıyoruz.

İlk not defterleri şunlardır:

- **Güdümlü araştırma - İşlem Uyarıları**: Etkilenen ana bilgisayar veya ana bilgisayarlardaki etkinliği analiz ederek uyarıları hızlı bir şekilde üçlemenizi sağlar.
- **Güdümlü avcılık - Windows ana bilgisayar gezgini gezgini**: Hesap etkinliğini, işlem yürütmelerini, ağ etkinliğini ve ana bilgisayardaki diğer olayları keşfetmenizi sağlar.
- **Güdümlü avcılık - Office365-Exploring**: Birden fazla Office 365 veri kümesinde şüpheli Office 365 etkinliğini avla.

[Azure Sentinel Topluluğu GitHub deposu,](https://github.com/Azure/Azure-Sentinel) Microsoft tarafından oluşturulmuş veya topluluktan katkıda bulunulan gelecekteki Azure Sentinel dizüstü bilgisayarlar için konumdur.

Not defterlerini kullanmak için bir Azure Not Defteri hesabınız olması gerekir. Daha fazla bilgi için, Azure Notebook belgelerinden [Quickstart: Oturum açın ve bir kullanıcı kimliği ayarlayın.](https://docs.microsoft.com/azure/notebooks/quickstart-sign-in-azure-notebooks) Bu hesabı oluşturmak için **Azure Sentinel - Notebook'taki**komut çubuğundan **Azure Not Defterlerine Kaydol** seçeneğini kullanabilirsiniz:

> [!div class="mx-imgBorder"]
>![Azure Dizüstü Bilgisayarlar seçeneğine kaydolun](./media/notebooks/sentinel-azure-sign-up-azure-notebooks.png)

Doğrudan Azure Sentinel'den bir not defteri çalıştırabilir veya tüm Azure Sentinel not defterlerini yeni bir Azure Notebook projesinde klonlayabilirsiniz.

## <a name="run-a-notebook-from-azure-sentinel"></a>Azure Sentinel'den bir not defteri çalıştırma
 
1. Azure portalından, Azure Sentinel'in sağladığı dizüstü bilgisayarları görebileceğiniz **Azure Sentinel** > **Tehdit yönetimi** > **Not Defterleri'ne**gidin. 

2. Açıklamalarını, gerekli veri türlerini ve veri kaynaklarını okumak için tek tek not defterlerini seçin. Örnek:
    
    > [!div class="mx-imgBorder"]
    > ![başlatma dizüstü bilgisayarı](./media/notebooks/sentinel-azure-notebooks-launch.png)

3. Kullanmak istediğiniz not defterini seçin ve ardından not defterini Azure Sentinel çalışma alanınıza bağlanan yeni bir Azure Notebook projesine klonlamak ve yapılandırmak için **Not Defterini Başlat 'ı (Önizleme)** seçin. İşlem tamamlandığında, çalışmanız için not defteri Azure Not Defterleri'nde açılır.

## <a name="clone-azure-sentinel-notebooks-to-a-new-azure-notebooks-project"></a>Azure Sentinel dizüstü bilgisayarlarını yeni bir Azure Dizüstü Bilgisayar projesine klonla

Bu yordam, sizin için Azure Sentinel dizüstü bilgisayarlarını içeren bir Azure Notebook projesi oluşturur. Daha sonra not defterlerini olduğu gibi çalıştırabilir veya bunlarda değişiklik yapıp çalıştırabilirsiniz.

1. Azure portalından Azure **Sentinel** > **Tehdit yönetimi** > **Not Defterlerine** gidin ve ardından komut çubuğundan **Klon Not Defterleri'ni** seçin:
  
    > [!div class="mx-imgBorder"]
    >![Kloniknot Not Defterleri seçeneği](./media/notebooks/sentinel-azure-clone-notebooks.png)

2. Aşağıdaki iletişim kutusu görüntülendiğinde, GitHub repo'yu Azure Not Defterleri projenize kopyalamak için **İçe Aktar'ı** seçin. Varolan bir Azure Not Defteri hesabınız yoksa, bir hesap oluşturmanız ve oturum açmanız istenir.

   ![Not defterini içe aktarma](./media/notebooks/sentinel-notebooks-clone.png)

3. Upload **GitHub Deposu** iletişim kutusunda, bu seçenek bağlı GitHub depolarını ifade ettiği için **Clone'u özyinelemeli olarak** seçmeyin. Proje adı için varsayılan adı veya yenisini yazın. Ardından, tamamlanması birkaç dakika sürebilen GitHub içeriğini klonlamaya başlamak için **İçe Aktar'ı** tıklatın.

   ![Not defterini içe aktarma](./media/notebooks/sentinel-create-project.png)

4. Oluşturduğunuz projeyi açın ve not defterlerini görmek için **Not Defterleri** klasörünü açın. Örnek:

   ![İthalat repo](./media/notebooks/sentinel-open-notebook1.png)

Daha sonra not defterlerini Azure Not Defterleri'nden çalıştırabilirsiniz. Azure Sentinel'den bu not defterlerine dönmek için Azure Sentinel - **Notebook'taki**komut çubuğundan **Not Defterlerinize Git'i** seçin:

> [!div class="mx-imgBorder"]
>![Not Defterleri seçeneğinize gidin](./media/notebooks/sentinel-azure-to-go-notebooks.png)


## <a name="using-notebooks-to-hunt"></a>Avlanmak için defter kullanma

Her not defteri bir av veya soruşturma yürütmek için adımları size yol. Kitaplıklar ve not defterinin gerektirdiği diğer bağımlılıklar not defterinin kendisinden veya basit bir yapılandırma yordamı yla yüklenebilir. Dizüstü bilgisayar projenizi Azure Sentinel aboneliğinize bağlayan yapılandırma, önceki adımlarda otomatik olarak sağlanır.

1. Azure Not Defterleri'nde zaten değilseniz, Azure Sentinel - **Notebook'taki**komut çubuğundan **Not Defterlerinize Git** seçeneğini kullanabilirsiniz:
    
    > [!div class="mx-imgBorder"]
    >![Not Defterleri seçeneğinize gidin](./media/notebooks/sentinel-azure-to-go-notebooks.png)
    
    Azure Not Defterleri'nde **Projelerim'i,** ardından Azure Sentinel not defterlerini içeren projeyi ve son olarak **Dam'lar** klasörünü seçin.
    
2. Bir not defterini açmadan önce, varsayılan olarak, not defterlerini çalıştırmak için Ücretsiz Bilgi İşlem'in seçildiğini unutmayın:
    
   ![not defterini seçin](./media/notebooks/sentinel-open-notebook2.png)
    
    Girişte açıklandığı gibi kullanmak üzere bir Veri Bilimi Sanal Makineleri (DSVM) yapılandırıldıysanız, ilk dizüstü bilgisayarı açmadan önce DSVM'yi seçin ve kimlik doğrulamasını belirleyin. 

3. Açmak için bir not defteri seçin.
    
    Bir not defterini ilk açtığınızda, çekirdek sürümünü seçmeniz istenebilir. İstenmiyorsanız, **Çekirdek** >  **Değiştir çekirdeğinden**çekirdek sürümünü seçebilir ve ardından en az 3,6 olan bir sürümü seçebilirsiniz. Seçili çekirdek sürümü not defteri penceresinin sağ üst kısmında görüntülenir:
    
   ![not defterini seçin](./media/notebooks/sentinel-select-kernel.png)

4. İndirdiğiniz not defterinde herhangi bir değişiklik yapmadan önce, orijinal not defterinin bir kopyasını yapmak ve kopya üzerinde çalışmak iyi bir fikirdir. Bunu yapmak için **Dosya** > **Kopya Yap'ı**seçin. Kopyalar üzerinde çalışmak, verilerinizin hiçbirinin üzerine yazmadan dizüstü bilgisayarların gelecekteki sürümlerini güvenli bir şekilde güncelleştirmenizi sağlar.
    
    Artık seçili not defterini çalıştırmaya veya yönetmeye hazırsınız.

Öneri:

- Azure Sentinel'de veri sorgulamaya hızlı bir giriş için ana **Notebook'lar** klasöründeki [Başlangıç](https://github.com/Azure/Azure-Sentinel/blob/master/Notebooks/Get%20Started.ipynb) not defterine bakın. 

- **Örnek Defterler** alt klasöründe ek örnek not defterleri bulabilirsiniz. Bu örnek not defterleri verilerle kaydedilmiştir, böylece amaçlanan çıktıyı daha kolay görebilirsiniz. Bu dizüstü bilgisayarları [nbviewer'da](https://nbviewer.jupyter.org/)görüntülemenizi öneririz. 

- **HowTos** klasörü, örneğin varsayılan Python sürümünü açıklayan not defterleri içerir, DSVM'yi yapılandırmak, not defterinden Azure Sentinel yer imleri oluşturma ve diğer konular.

Sağlanan not defterleri, hem yararlı araçlar hem de kendi dizüstü bilgisayarınızın geliştirilmesinde kullanabileceğiniz çizimler ve kod örnekleri olarak tasarlanmıştır.

Öneriler, özellikler için istekler, katkıda bulunulan Not Defterleri, hata raporları veya mevcut not defterlerine iyileştirmeler ve eklemeler olsun, geri bildirimleri memnuniyetle karşılarız. Bir sorun veya çatal oluşturmak için [Azure Sentinel Community GitHub'a](https://github.com/Azure/Azure-Sentinel) gidin ve bir katkı yükleyin.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Azure Sentinel'de Jupyter dizüstü bilgisayarlarını kullanmaya nasıl başlasınız öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Proaktif tehditler için avı](hunting.md)
- [Avlanırken ilginç bilgileri kaydetmek için yer imlerini kullanın](bookmarks.md)
