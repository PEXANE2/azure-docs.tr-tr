---
title: Bir web hizmetini dağıtma
titleSuffix: ML Studio (classic) - Azure
description: Bir eğitim denemesini tahmine dayalı bir denemeye dönüştürme, dağıtıma hazırlama ve ardından Azure Machine Learning Studio (klasik) web hizmeti olarak dağıtma.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: likebupt
ms.author: keli19
ms.custom: previous-ms.author=yahajiza, previous-author=YasinMSFT
ms.date: 01/06/2017
ms.openlocfilehash: 6c81e50560de69f7702e852d4602680fde7f01f3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79218140"
---
# <a name="deploy-an-azure-machine-learning-studio-classic-web-service"></a>Azure Machine Learning Studio (klasik) web hizmetini dağıtma

[!INCLUDE [Notebook deprecation notice](../../../includes/aml-studio-notebook-notice.md)]

Azure Machine Learning Studio (klasik), tahmine dayalı bir analitik çözüm oluşturmanıza ve test etmenizi sağlar. Ardından çözümü bir web hizmeti olarak dağıtabilirsiniz.

Machine Learning Studio (klasik) web hizmetleri, bir uygulama ile Machine Learning Studio (klasik) iş akışı puanlama modeli arasında bir arayüz sağlar. Harici bir uygulama gerçek zamanlı olarak bir Machine Learning Studio (klasik) iş akışı puanlama modeli ile iletişim kurabilir. Machine Learning Studio (klasik) web hizmetine yapılan arama, tahmin sonuçlarını harici bir uygulamaya döndürür. Bir web hizmetine çağrı yapmak için web hizmeti dağıtılırken oluşturulan bir API anahtarını geçirirsiniz. Machine Learning Studio (klasik) web hizmeti, web programlama projeleri için popüler bir mimari seçim olan REST'e dayanmaktadır.

Azure Machine Learning Studio (klasik) iki tür web hizmetine sahiptir:

* İstek-Yanıt Hizmeti (RRS): Tek bir veri kaydı nı alan düşük gecikmeli, yüksek ölçeklenebilir bir hizmettir.
* Toplu Yürütme Hizmeti (BES): Bir veri kaydı toplu puanlayan bir eşzamanlı hizmet.

BES girişi, RRS’nin kullandığı veri girişi gibidir. Aralarındaki temel fark, BES'nin Azure Blob depolama, Azure Tablo depolama, Azure SQL Veritabanı, HDInsight (hive sorgusu) ve HTTP kaynakları gibi çeşitli kaynaklardan bir kayıt bloğunu okumasıdır.

Üst düzey bir bakış açısından, modelinizi üç adımda dağıtmış sınız:

* **[Bir eğitim deneyi oluşturun]** - Studio'da (klasik), büyük bir dahili makine öğrenimi algoritması seti kullanarak sağladığınız eğitim verilerini kullanarak tahmine dayalı bir analitik model eğitebilir ve test edebilirsiniz.
* **[Bunu tahmine dayalı bir denemeye dönüştürün]** - Modeliniz varolan verilerle eğitildikten ve yeni veriler elde etmek için kullanmaya hazır olduğunuzda, denemenizi tahminler için hazırlar ve kolaylaştırırsınız.
* Yeni bir **[web hizmeti]** veya **[Klasik web hizmeti]** olarak **dağıtın** - Tahmine dayalı denemenizi Azure web hizmeti olarak dağıttığınızda, kullanıcılar modelinize veri gönderebilir ve modelinizin tahminlerini alabilir.

## <a name="create-a-training-experiment"></a>Bir eğitim deneyi oluşturma

Tahmine dayalı bir analitik modeli eğitmek için, eğitim verilerini yüklemek, verileri gerektiği gibi hazırlamak, makine öğrenimi algoritmalarını uygulamak ve Sonuç -ları. Bir denemeyi yineleyebilir ve sonuçları karşılaştırmak ve değerlendirmek için farklı makine öğrenimi algoritmalarını deneyebilirsiniz.

Eğitim deneyleri oluşturma ve yönetme süreci başka yerlerde daha ayrıntılı olarak ele alınmıştır. Daha fazla bilgi için şu makalelere bakın:

* [Azure Machine Learning Studio'da basit bir deneme oluşturun (klasik)](create-experiment.md)
* [Azure Machine Learning Studio (klasik) ile tahmine dayalı bir çözüm geliştirin](tutorial-part1-credit-risk.md)
* [Eğitim verilerinizi Azure Machine Learning Studio'ya aktarın (klasik)](import-data.md)
* [Azure Machine Learning Studio'da deneme yinelemelerini yönetme (klasik)](manage-experiment-iterations.md)

## <a name="convert-the-training-experiment-to-a-predictive-experiment"></a>Eğitim denemesini tahmine dayalı bir deneye dönüştürme

Modelinizi eğittikten sonra, yeni veriler elde etmek için eğitim denemenizi tahmine dayalı bir denemeye dönüştürmeye hazırsınız.

Tahmine dayalı bir denemeye dönüştürerek, eğitimli modelinizi puanlama web hizmeti olarak dağıtılacak şekilde hazır hale emiş oluyorsunuz. Web hizmetinin kullanıcıları giriş verilerini modelinize gönderebilir ve modeliniz tahmin sonuçlarını geri gönderir. Öngörücü bir denemeye dönüştürürken, modelinizin başkaları tarafından nasıl kullanılmasını beklediğiniz konusunda aklınızda bulundurun.

Eğitim denemenizi tahmine dayalı bir denemeye dönüştürmek için, deneme tuvalinin altındaki **Çalıştır'ı** tıklatın, **Web Hizmeti Ayarla'yı**tıklatın ve ardından **Tahmine Dayalı Web Hizmeti'ni**seçin.

![Puanlama deneyine dönüştürme](./media/publish-a-machine-learning-web-service/figure-1.png)

Bu dönüşümün nasıl gerçekleştirilebildiğini hakkında daha fazla bilgi [için, modelinizi Azure Machine Learning Studio'da (klasik) dağıtıma nasıl hazırlayacağınızı](convert-training-experiment-to-scoring-experiment.md)öğrenin.

Aşağıdaki adımlar, tahmine dayalı bir denemeyi Yeni bir web hizmeti olarak dağıtmayı açıklar. Denemeyi Klasik web hizmeti olarak da dağıtabilirsiniz.

## <a name="deploy-it-as-a-new-web-service"></a>Yeni bir web hizmeti olarak dağıtın

Öngörülü deneme hazırlandığına göre, bunu yeni (Kaynak Yöneticisi tabanlı) Azure web hizmeti olarak dağıtabilirsiniz. Web hizmetini kullanarak, kullanıcılar modelinize veri gönderebilir ve model tahminlerini döndürecektir.

Tahmine dayalı denemenizi dağıtmak için, deneme tuvalinin altındaki **Çalıştır'ı** tıklatın. Deneme çalışma tamamlandıktan **sonra, Web Hizmeti Dağıt'ı** tıklatın ve **Web Hizmeti [Yeni]Dağıt'ı**seçin.  Machine Learning Studio (klasik) Web Hizmeti portalının dağıtım sayfası açılır.

> [!NOTE] 
> Yeni bir web hizmetini dağıtmak için, web hizmetini dağıttığınız abonelikte yeterli izinlere sahip olmalısınız. Daha fazla bilgi için Azure [Machine Learning Web Services portalını kullanarak bir Web hizmetini yönetin.](manage-new-webservice.md) 

### <a name="web-service-portal-deploy-experiment-page"></a>Web Hizmeti portalı Dağıtma Deneme Sayfası

Denemedağıt sayfasında, web hizmeti için bir ad girin.
Bir fiyatlandırma planı seçin. Varolan bir fiyatlandırma planınız varsa, bunu seçebilirsiniz, aksi takdirde hizmet için yeni bir fiyat planı oluşturmanız gerekir.

1. Fiyat **Planı** açılırken, varolan bir planı seçin veya **yeni plan** seç seçeneğini seçin.
2. **Plan**Adı'nda, faturanızdaki planı tanımlayacak bir ad yazın.
3. Aylık Plan **Katmanlarından**birini seçin. Plan, varsayılan bölgenizin planlarını varsayılan olarak katmanlar ve web hizmetiniz bu bölgeye dağıtılır.

**Dağıt'ı** tıklatın ve web hizmetiniz için **Hızlı Başlangıç** sayfası açılır.

Web hizmeti Quickstart sayfası, bir web hizmeti oluşturduktan sonra gerçekleştireceğiniz en yaygın görevler hakkında size erişim ve rehberlik sağlar. Buradan hem Test sayfasına hem de Tüket sayfasına kolayca erişebilirsiniz.

<!-- ![Deploy the web service](./media/publish-a-machine-learning-web-service/figure-2.png)-->

### <a name="test-your-new-web-service"></a>Yeni web hizmetinizi test edin

Yeni web hizmetinizi test etmek için, ortak görevler altında **web hizmetini test** edin'i tıklatın. Test sayfasında, web hizmetinizi İstek-Yanıt Hizmeti (RRS) veya Toplu Yürütme hizmeti (BES) olarak test edebilirsiniz.

RRS test sayfası, deneme için tanımladığınız girdileri, çıktıları ve tüm genel parametreleri görüntüler. Web hizmetini sınamak için, girişler için uygun değerleri el ile girebilir veya test değerlerini içeren virgülle ayrılmış değer (CSV) biçimlendirilmiş bir dosya sağlayabilirsiniz.

RRS kullanarak test etmek için, liste görünümü modundan, girişler için uygun değerleri girin ve **Test İstek-Yanıt'ı**tıklatın. Tahmin sonuçlarınız soldaki çıkış sütununda görüntülenir.

![Web hizmetinizi test etmek için uygun değerleri girin](./media/publish-a-machine-learning-web-service/figure-5-test-request-response.png)

BES'inizi test etmek için **Toplu İşlem'i**tıklatın. Toplu İşlem test sayfasında, girişinizin altında Gözat'ı tıklatın ve uygun örnek değerleri içeren bir CSV dosyası seçin. CSV dosyanız yoksa ve Machine Learning Studio'yu (klasik) kullanarak öngörülü denemenizi oluşturduysanız, tahmine dayalı deneyinizin veri kümesini indirebilir ve kullanabilirsiniz.

Veri kümesini indirmek için Machine Learning Studio'yu (klasik) açın. Tahmine dayalı denemenizi açın ve denemeniz için girişi sağ tıklatın. Bağlam menüsünden **veri kümesini** seçin ve ardından **İndir'i**seçin.

![Veri setinizi Studio (klasik) tuvalinden indirin](./media/publish-a-machine-learning-web-service/figure-7-mls-download.png)

**Test'i**tıklatın. Toplu İşlem İşler altında sağa Toplu Yürütme iş görüntüler **durumu.**

![Toplu İşlem işinizi web servis portalı ile test edin](./media/publish-a-machine-learning-web-service/figure-6-test-batch-execution.png)

<!--![Test the web service](./media/publish-a-machine-learning-web-service/figure-3.png)-->

**CONFIGURATION** sayfasında, açıklamayı, başlığı değiştirebilir, depolama hesabı anahtarını güncelleyebilir ve web hizmetiniz için örnek verileri etkinleştirebilirsiniz.

![Web hizmetinizi yapılandırın](./media/publish-a-machine-learning-web-service/figure-8-arm-configure.png)

### <a name="access-your-new-web-service"></a>Yeni web hizmetinize erişin

Web hizmetinizi Machine Learning Studio'dan (klasik) dağıttıktan sonra, hizmete veri gönderebilir ve programlı yanıta yanıt alabilirsiniz.

**Tüket sayfası,** web hizmetinize erişmek için ihtiyacınız olan tüm bilgileri sağlar. Örneğin, API anahtarı hizmete yetkili erişime izin vermek için sağlanır.

Machine Learning Studio (klasik) web hizmetine erişim hakkında daha fazla bilgi için Azure [Machine Learning Studio (klasik) Web hizmetini nasıl tüketir'](consume-web-services.md)e bakın.

### <a name="manage-your-new-web-service"></a>Yeni web hizmetinizi yönetin

Yeni web hizmetlerinizi Machine Learning Studio (klasik) Web Hizmetleri portalını kullanarak yönetebilirsiniz. Ana [portal sayfasından](https://services.azureml.net/) **Web Hizmetleri'ni**tıklatın. Web hizmetleri sayfasından bir hizmeti silebilir veya kopyalayabilirsiniz. Belirli bir hizmeti izlemek için hizmeti tıklatın ve ardından **Pano'yu**tıklatın. Web hizmetiyle ilişkili toplu işleri izlemek için **Toplu İstek Günlüğü'ne**tıklayın.

### <a name="deploy-your-new-web-service-to-multiple-regions"></a><a id="multi-region"></a>Yeni web hizmetinizi birden çok bölgeye dağıtma

Birden çok abonelik veya çalışma alanı gerekkalmadan yeni bir web hizmetini birden çok bölgeye kolayca dağıtabilirsiniz.

Fiyatlandırma bölgeye özgüdür, bu nedenle web hizmetini dağıtacağınız her bölge için bir faturalandırma planı tanımlamanız gerekir.

#### <a name="create-a-plan-in-another-region"></a>Başka bir bölgede plan oluşturma

1. [Microsoft Azure Machine Learning Web Services'da](https://services.azureml.net/)oturum açın.
2. **Planlar** menüsü seçeneğini tıklatın.
3. Görünüm sayfasında Planlar'da **Yeni'yi**tıklatın.
4. **Abonelik** açılır tarihinden, yeni planın yer alacağı aboneliği seçin.
5. **Bölge** açılır tarafından, yeni plan için bir bölge seçin. Seçili bölge için Plan Seçenekleri sayfanın **Plan Seçenekleri** bölümünde görüntülenir.
6. Kaynak **Grubu** açılır tarafından plan için bir kaynak grubu seçin. Kaynak grupları hakkında daha fazla bilgi için [Azure Kaynak Yöneticisi'ne genel bakış](../../azure-resource-manager/management/overview.md)bilgisine bakın.
7. **Plan Adı'nda** planın adı yazın.
8. **Plan Seçenekleri**altında, yeni plan için fatura düzeyini tıklatın.
9. **Oluştur'u**tıklatın.

#### <a name="deploy-the-web-service-to-another-region"></a>Web hizmetini başka bir bölgeye dağıtma

1. Microsoft Azure Machine Learning Web Hizmetleri sayfasında **Web Hizmetleri** menüsü seçeneğini tıklatın.
2. Yeni bir bölgeye dağıttığınız Web Hizmeti'ni seçin.
3. **Kopyala'yı**tıklatın.
4. **Web Hizmet Adı'nda,** web hizmeti için yeni bir ad yazın.
5. **Web hizmeti açıklamasında,** web hizmeti için bir açıklama yazın.
6. **Abonelik** açılır sayfasından, yeni web hizmetinin bulunduğu aboneliği seçin.
7. Kaynak **Grubu** açılır sayfasından, web hizmeti için bir kaynak grubu seçin. Kaynak grupları hakkında daha fazla bilgi için [Azure Kaynak Yöneticisi'ne genel bakış](../../azure-resource-manager/management/overview.md)bilgisine bakın.
8. **Bölge** açılır sayfasından, web hizmetini dağıtmak için bölgeyi seçin.
9. Depolama **hesabı** açılır sayfasından, web hizmetini depolamak için bir depolama hesabı seçin.
10. Fiyat **Planı** açılır tarihinden, adım 8'de seçtiğiniz bölgede bir plan seçin.
11. **Kopyala'yı**tıklatın.

## <a name="deploy-it-as-a-classic-web-service"></a>Klasik web hizmeti olarak dağıtın

Öngörü denemesi yeterince hazırlandığına göre, bunu Klasik Azure web hizmeti olarak dağıtabilirsiniz. Web hizmetini kullanarak, kullanıcılar modelinize veri gönderebilir ve model tahminlerini döndürecektir.

Tahmine dayalı denemenizi dağıtmak için deneme tuvalinin altındaki **Çalıştır'ı** tıklatın ve ardından **Web Hizmetini Dağıt'ı**tıklatın. Web hizmeti ayarlanır ve web hizmeti panosuna yerleştirilir.

![Web hizmetinizi Studio'dan dağıtın (klasik)](./media/publish-a-machine-learning-web-service/figure-2.png)

### <a name="test-your-classic-web-service"></a>Klasik web hizmetinizi test edin

Web hizmetini Machine Learning Studio (klasik) Web Hizmetleri portalında veya Machine Learning Studio'da (klasik) test edebilirsiniz.

İstek Yanıtı web hizmetini test etmek için web hizmeti panosundaki **Test** düğmesini tıklatın. Hizmetin giriş verilerini sormak için bir iletişim kutusu açılır. Bunlar puanlama deneyinin beklediği sütunlardır. Bir veri kümesi girin ve ardından **Tamam'ı**tıklatın. Web hizmeti tarafından oluşturulan sonuçlar pano altında görüntülenir.

Hizmetinizi Azure Machine Learning Studio (klasik) Web Hizmetleri portalında daha önce Yeni web hizmeti bölümünde gösterildiği gibi test etmek için **Test** önizleme bağlantısını tıklatabilirsiniz.

Toplu İşlem Yürütme Hizmetini test etmek için **Test** önizleme bağlantısını tıklatın. Toplu İşlem test sayfasında, girişinizin altında Gözat'ı tıklatın ve uygun örnek değerleri içeren bir CSV dosyası seçin. CSV dosyanız yoksa ve Machine Learning Studio'yu (klasik) kullanarak öngörülü denemenizi oluşturduysanız, tahmine dayalı deneyinizin veri kümesini indirebilir ve kullanabilirsiniz.

![Web hizmetini test etme](./media/publish-a-machine-learning-web-service/figure-3.png)

**CONFIGURATION** sayfasında, hizmetin görüntü adını değiştirebilir ve bir açıklama verebilirsiniz. Ad ve açıklama, web hizmetlerinizi yönettiğiniz [Azure portalında](https://portal.azure.com/) görüntülenir.

**Giriş schema**, OUTPUT **SCHEMA**ve **Web SERVICE PARAMETER**altında her sütun için bir dize girerek giriş verileri, çıktı verileri ve web hizmeti parametreleri için bir açıklama sağlayabilirsiniz. Bu açıklamalar, web hizmeti için sağlanan örnek kod belgelerinde kullanılır.

Web hizmetinize erişildiğinde gördüğünüz hataları tanılamak için günlüğe kaydetmeyi etkinleştirebilirsiniz. Daha fazla bilgi için Machine [Learning Studio (klasik) web hizmetleri için günlük etkinleştir medeni'ye](web-services-logging.md)bakın.

![Web hizmetleri portalında oturum açmayı etkinleştirme](./media/publish-a-machine-learning-web-service/figure-4.png)

Azure Machine Learning Web Services portalındaki web hizmetinin bitiş noktalarını, Yeni web hizmeti bölümünde daha önce gösterilen yordama benzer şekilde de yapılandırabilirsiniz. Seçenekler farklıdır, hizmet açıklamasını ekleyebilir veya değiştirebilir, günlüğe kaydetmeyi etkinleştirebilir ve test için örnek verileri etkinleştirebilirsiniz.

### <a name="access-your-classic-web-service"></a>Klasik web hizmetinize erişin

Web hizmetinizi Azure Machine Learning Studio'dan (klasik) dağıttıktan sonra, hizmete veri gönderebilir ve programlı yanıta yanıt alabilirsiniz.

Pano, web hizmetinize erişmek için gereken tüm bilgileri sağlar. Örneğin, HIZMETe yetkili erişime izin vermek için API anahtarı sağlanır ve kodunuzu yazmaya başlamanıza yardımcı olmak için API yardım sayfaları sağlanır.

Machine Learning Studio (klasik) web hizmetine erişim hakkında daha fazla bilgi için Azure [Machine Learning Studio (klasik) Web hizmetini nasıl tüketir'](consume-web-services.md)e bakın.

### <a name="manage-your-classic-web-service"></a>Klasik web hizmetinizi yönetin

Bir web hizmetini izlemek için gerçekleştirebileceğiniz çeşitli eylemler vardır. Güncelleyebilir ve silebilirsiniz. Ayrıca, dağıtırken oluşturulan varsayılan bitiş noktasına ek olarak Klasik web hizmetine ek uç noktalar ekleyebilirsiniz.

Daha fazla bilgi için Azure [Machine Learning Studio (klasik) çalışma alanı yönet](manage-workspace.md) ve Azure Machine Learning Studio [(klasik) Web Hizmetleri portalını kullanarak bir web hizmetini yönet'](manage-new-webservice.md)e bakın.

## <a name="update-the-web-service"></a>Web hizmetini güncelleştirme
Modeli ek eğitim verileriyle güncelleştirmek ve özgün web hizmetinin üzerine yazmak gibi web hizmetinizde değişiklikler yapabilir ve yeniden dağıtabilirsiniz.

Web hizmetini güncelleştirmek için, web hizmetini dağıtmak için kullandığınız orijinal tahmin denemesini açın ve **KAYDET'i**tıklatarak güncellenebilir bir kopya yapın. Değişikliklerinizi yapın ve ardından **Web Hizmeti Dağıt'ı**tıklatın.

Bu denemeyi daha önce dağıttığınız için, (Klasik Web Hizmeti) üzerine yazmak veya (Yeni web hizmeti) varolan hizmeti güncelleştirmek isteyip istemediğiniz sorulur. **EVET** veya **Güncelleştir'i** tıklatmak varolan web hizmetini durdurur ve yerine yeni tahmine dayalı deneme dağıtılır.

> [!NOTE]
> Orijinal web hizmetinde yapılandırma değişiklikleri yaptıysanız, örneğin yeni bir görüntü adı veya açıklama girerek, bu değerleri yeniden girmeniz gerekir.

Web hizmetinizi güncelleştirmek için bir seçenek, modeli programlı olarak yeniden eğitmektir. Daha fazla bilgi için, [retrain Machine Learning Studio (klasik) modelleri programlı](/azure/machine-learning/studio/retrain-machine-learning-model)bakın.

## <a name="next-steps"></a>Sonraki adımlar

* Dağıtımın nasıl çalıştığı hakkında daha fazla teknik bilgi için, [Machine Learning Studio (klasik) modelinin bir denemeden operasyonel bir Web hizmetine nasıl ilerlediğine](model-progression-experiment-to-web-service.md)bakın.

* Modelinizi dağıtmaya nasıl hazırlayacağınızla ilgili ayrıntılar için, [modelinizi Azure Machine Learning Studio'da (klasik) dağıtıma nasıl hazırlayacağınızı](convert-training-experiment-to-scoring-experiment.md)öğrenin.

* REST API’sini kullanmanın ve web hizmetine erişmenin birkaç yolu vardır. Azure [Machine Learning Studio (klasik) web hizmetini nasıl tüketirmisiniz](consume-web-services.md)bkz.

<!-- internal links -->
[Bir eğitim deneyi oluşturma]: #create-a-training-experiment
[Tahmine dayalı bir deneye dönüştürün]: #convert-the-training-experiment-to-a-predictive-experiment
[Yeni web hizmeti]: #deploy-it-as-a-new-web-service
[Klasik web hizmeti]: #deploy-it-as-a-classic-web-service
[Yeni]: #deploy-it-as-a-new-web-service
[classic]: #deploy-the-predictive-experiment-as-a-classic-web-service
[Access]: #access-the-Web-service
[Manage]: #manage-the-Web-service-in-the-azure-management-portal
[Update]: #update-the-Web-service
