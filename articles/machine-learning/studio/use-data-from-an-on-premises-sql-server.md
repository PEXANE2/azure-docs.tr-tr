---
title: Şirket Içi SQL Server
titleSuffix: ML Studio (classic) - Azure
description: Azure Machine Learning Studio (klasik) ile gelişmiş analitik gerçekleştirmek için şirket içi BIR SQL Server veritabanındaki verileri kullanın.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: likebupt
ms.author: keli19
ms.custom: seodec18
ms.date: 03/13/2017
ms.openlocfilehash: 648dbdb7e9e9d1b20c55d3fa5b314b7e4657d5e7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79204191"
---
# <a name="perform-analytics-with-azure-machine-learning-studio-classic-using-an-on-premises-sql-server-database"></a>Şirket içi SQL Server veritabanını kullanarak Azure Machine Learning Studio (klasik) ile analitik gerçekleştirin

[!INCLUDE [Notebook deprecation notice](../../../includes/aml-studio-notebook-notice.md)]

Şirket içi verilerle çalışan işletmeler genellikle, makine öğrenimi iş yükleri için bulutun ölçeğinden ve çevikliğinden yararlanmak ister. Ancak şirket içi verilerini buluta taşıyarak mevcut iş süreçlerini ve iş akışlarını bozmak istemezler. Azure Machine Learning Studio (klasik) artık verilerinizi şirket içi bir SQL Server veritabanından okumayı ve ardından bu verilerle bir modeli eğitip puanlamayı destekler. Artık verileri bulut ve şirket içi sunucunuz arasında el ile kopyalamanız ve eşitlemeniz gerekmeyecektir. Bunun yerine, Azure Machine Learning Studio'daki (klasik) **Veri Alma** modülü artık eğitim ve puanlama işleriniz için şirket içi SQL Server veritabanınızdan doğrudan okuyabilir.

Bu makalede, şirket içi SQL sunucu verilerinin Azure Machine Learning Studio'ya (klasik) nasıl girilen bir genel bakış vardır. Çalışma alanları, modüller, veri setleri, *denemeler, vb.* gibi Studio (klasik) kavramlara aşina olduğunuzu varsayar.

> [!NOTE]
> Bu özellik ücretsiz çalışma alanları için kullanılamaz. Machine Learning fiyatlandırması ve katmanları hakkında daha fazla bilgi için Azure [Makine Öğrenimi Fiyatlandırması'na](https://azure.microsoft.com/pricing/details/machine-learning/)bakın.
>
>

<!-- -->



## <a name="install-the-data-factory-self-hosted-integration-runtime"></a>Veri Fabrikası'nı Kendi Barındıran Tümleştirme Çalışma Süresini Yükleme
Azure Machine Learning Studio'da (klasik) şirket içi bir SQL Server veritabanına erişmek için, eskiden Veri Yönetimi Ağ Geçidi olarak bilinen Data Factory Self barındırılan Tümleştirme Çalışma Süresini indirmeniz ve yüklemeniz gerekir. Makine Öğrenimi Stüdyosu'ndaki (klasik) bağlantıyı yapılandırdığınızda, aşağıda açıklanan **veri ağ geçidi** iletişim kutusunu kullanarak Tümleştirme Çalışma Süresi'ni (IR) indirme ve yükleme fırsatına sahip olursunuz.


Ayrıca, [Microsoft Download Center'dan](https://www.microsoft.com/download/details.aspx?id=39717)MSI kurulum paketini indirip çalıştırarak IR'yi önceden yükleyebilirsiniz. MSI, varolan bir IR'yi tüm ayarları korunmuş olarak en son sürüme yükseltmek için de kullanılabilir.

Veri Fabrikası Self-Hosted Entegrasyon Runtime aşağıdaki ön koşullara sahiptir:

* Veri Fabrikası Self-Hosted Tümleştirme .NET Framework 4.6.1 veya üzeri 64 bit İşletim Sistemi gerektirir.
* Desteklenen Windows işletim sistemi sürümleri Windows 10, Windows Server 2012, Windows Server 2012 R2, Windows Server 2016'dır. 
* IR makinesi için önerilen yapılandırma en az 2 GHz, 4 Core CPU, 8GB RAM ve 80GB disktir.
* Ana bilgisayar makinesi hazırda bekletme durumunda, IR veri isteklerine yanıt vermez. Bu nedenle, IR yüklemeden önce bilgisayarda uygun bir güç planı yapılandırın. Makine hazırda bekleme değiştirmek için yıkılım yapacak şekilde yürütülüyorsa, IR yüklemesi bir ileti
* Kopyalama etkinliği belirli bir frekansta gerçekleştiğinden, makinedeki kaynak kullanımı (CPU, bellek) aynı deseni en yüksek ve boşta olan zamanlarda da izler. Kaynak kullanımı da büyük ölçüde taşınan veri miktarına bağlıdır. Birden çok kopyalama işi devam ederken, kaynak kullanımının yoğun zamanlarda arta ulaştığını gözlemleceksiniz. Yukarıda listelenen minimum yapılandırma teknik olarak yeterli olsa da, veri hareketi için özel yükünüze bağlı olarak minimum yapılandırmadan daha fazla kaynağa sahip bir yapılandırmaya sahip olmak isteyebilirsiniz.

Veri Fabrikası Kendi Kendine Barındırılan Tümleştirme Çalışma Zamanı'nı kurarken ve kullanırken aşağıdakileri göz önünde bulundurun:

* Tek bir bilgisayara yalnızca bir IR örneği yükleyebilirsiniz.
* Birden çok şirket içi veri kaynağı için tek bir IR kullanabilirsiniz.
* Farklı bilgisayarlardaki birden çok IRs'yi aynı şirket içi veri kaynağına bağlayabilirsiniz.
* Bir ANDa yalnızca bir çalışma alanı için bir IR yapılandırAbilirsiniz. Şu anda, IRs çalışma alanları arasında paylaşılamaz.
* Tek bir çalışma alanı için birden çok IR yapılandırabilirsiniz. Örneğin, geliştirme sırasında test veri kaynaklarınıza bağlı bir IR ve operasyonelhale hazır olduğunuzda bir üretim IR'si kullanmak isteyebilirsiniz.
* IR'nin veri kaynağıyla aynı makinede olması gerekmez. Ancak veri kaynağına yakın kalmak, ağ geçidinin veri kaynağına bağlanma süresini kısaltıyor. Ağ geçidi ve veri kaynağının kaynaklar için rekabet etmemesi için şirket içi veri kaynağını barındıran makineden farklı bir makineye IR yüklemenizi öneririz.
* Bilgisayarınızda Power BI veya Azure Veri Fabrikası senaryoları sunan bir IR yüklüyse, azure machine learning studio (klasik) için başka bir bilgisayara ayrı bir IR yükleyin.

  > [!NOTE]
  > Veri Fabrikası'nı kendi barındırılan Tümleştirme Çalışma Süresi ve Power BI Ağ Geçidi'ni aynı bilgisayarda çalıştıramazsınız.
  >
  >
* Azure Machine Learning Studio (klasik) için Veri Fabrikası'nın kendi kendine barındırılan Tümleştirme Çalışma Süresini, diğer veriler için Azure ExpressRoute kullanıyor olsanız bile kullanmanız gerekir. ExpressRoute'u kullandığınızda bile veri kaynağınızı şirket içi veri kaynağı (güvenlik duvarının arkasında) olarak değerlendirmelisiniz. Machine Learning ve veri kaynağı arasında bağlantı kurmak için Veri Fabrikası'nın kendi kendine barındırılan Tümleştirme Çalışma Zamanını kullanın.

[Veri Fabrikası'nda Tümleştirme Çalışma Süresi makalesinde](../../data-factory/concepts-integration-runtime.md)kurulum ön koşulları, yükleme adımları ve sorun giderme ipuçları hakkında ayrıntılı bilgi bulabilirsiniz.

## <a name="span-idusing-the-data-gateway-step-by-step-walk-classanchorspan-id_toc450838866-classanchorspanspaningress-data-from-your-on-premises-sql-server-database-into-azure-machine-learning"></a><span id="using-the-data-gateway-step-by-step-walk" class="anchor"><span id="_Toc450838866" class="anchor"></span></span>Şirket içi SQL Server veritabanınızdaki verileri Azure Machine Learning'e girme
Bu gözden geçirmede, Azure Machine Learning çalışma alanında bir Azure Veri Fabrikası Tümleştirme Çalışma Süresi ayarlar, onu yapılandırır ve ardından şirket içi bir SQL Server veritabanından verileri okursunuz.

> [!TIP]
> Başlamadan önce, tarayıcınızın açılır pencere engelleyicisi `studio.azureml.net`için devre dışı. Google Chrome tarayıcısını kullanıyorsanız, Google Chrome WebStore'da bulunan birkaç eklentiden birini [indirin](https://chrome.google.com/webstore/search/clickonce?_category=extensions)ve yükleyin.
>
> [!NOTE]
> Azure Veri Fabrikası Self barındırılan Tümleştirme Çalışma Süresi eskiden Veri Yönetimi Ağ Geçidi olarak biliniyordu. Adım adım öğretici bir ağ geçidi olarak başvurmaya devam edecektir.  

### <a name="step-1-create-a-gateway"></a>Adım 1: Ağ geçidi oluşturma
İlk adım, şirket içi SQL veritabanınıza erişmek için ağ geçidioluşturmak ve kurmaktır.

1. [Azure Machine Learning Studio'da (klasik)](https://studio.azureml.net/Home/) oturum açın ve çalışmak istediğiniz çalışma alanını seçin.
2. Soldaki **AYARLAR** bıyısını tıklatın ve sonra üstteki **DATA GATEWAYS** sekmesini tıklatın.
3. Ekranın alt kısmında **Yenİ VERİ AĞ AĞGEÇIDI'nu** tıklatın.

    ![Yeni Veri Ağ Geçidi](./media/use-data-from-an-on-premises-sql-server/new-data-gateway-button.png)
4. Yeni **veri ağ geçidi** iletişim kutusunda, **Ağ Geçidi Adı'nı** girin ve isteğe bağlı olarak **Açıklama**ekleyin. Yapılandırmanın bir sonraki adımına gitmek için sağ alt köşedeki oku tıklatın.

    ![Ağ geçidi adı ve açıklama girin](./media/use-data-from-an-on-premises-sql-server/new-data-gateway-dialog-enter-name.png)
5. Veri ağ geçidini İndir ve kaydedin iletişim kutusunda, AĞ GEÇIDI KAYIT ANAHTARINI panoya kopyalayın.

    ![Veri ağ geçidini indirin ve kaydettirin](./media/use-data-from-an-on-premises-sql-server/download-and-register-data-gateway.png)
6. <span id="note-1" class="anchor"></span>Microsoft Veri Yönetimi Ağ Geçidi'ni henüz indirmediyseniz ve yüklemediyseniz, **veri yönetimi ağ geçidini karşıdan yükleyin.** Bu, sizi ihtiyacınız olan ağ geçidi sürümünü seçebileceğiniz, karşıdan yükleyebileceğiniz ve yükleyebileceğiniz Microsoft İndirme Merkezi'ne götürür. [Veri Yönetimi Ağ Geçidi ile verileri şirket içi kaynaklar ve bulut arasında taşıyın](../../data-factory/tutorial-hybrid-copy-portal.md)makalenin başlangıç bölümlerinde yükleme ön koşulları, yükleme adımları ve sorun giderme ipuçları hakkında ayrıntılı bilgi bulabilirsiniz.
7. Ağ geçidi yüklendikten sonra Veri Yönetimi Ağ Geçidi Yapılandırma Yöneticisi açılır ve **Kayıt ağ geçidi** iletişim kutusu görüntülenir. Panoya kopyaladığınız **Ağ Geçidi Kayıt Anahtarını** yapıştırın ve **Kaydol'u**tıklatın.
8. Zaten yüklü bir ağ geçidiniz varsa, Veri Yönetimi Ağ Geçidi Yapılandırma Yöneticisi'ni çalıştırın. Önceki adımda panoya kopyaladığınız **Ağ Geçidi Kayıt Anahtarını** **Değiştir'i**tıklatın ve **Tamam'ı**tıklatın.
9. Yükleme tamamlandığında, Microsoft Veri Yönetimi Ağ Geçidi Yapılandırma Yöneticisi için **Kayıt ağ geçidi** iletişim kutusu görüntülenir. Önceki adımda panoya kopyaladığınız GATEWAY REGISTRATION KEY'i yapıştırın ve **Kaydol'u**tıklatın.

    ![Ağ geçidini kaydetme](./media/use-data-from-an-on-premises-sql-server/data-gateway-configuration-manager-register-gateway.png)
10. Microsoft Veri Yönetimi Ağ Geçidi Yapılandırma Yöneticisi'nde **Ana sayfa** sekmesinde aşağıdaki değerler ayarlandığında ağ geçidi yapılandırması tamamlanır:

    * **Ağ geçidi adı** ve **Örnek adı** ağ geçidinin adına ayarlanır.
    * **Kayıt** **Kayıtlı**olarak ayarlanır.
    * **Durum** **Başlatıldı**olarak ayarlanır.
    * Alttaki durum çubuğu, yeşil bir onay işaretiyle birlikte **Veri Yönetimi Ağ Geçidi Bulut Hizmetine Bağlı'yı** görüntüler.

      ![Veri Yönetimi Ağ Geçidi Yöneticisi](./media/use-data-from-an-on-premises-sql-server/data-gateway-configuration-manager-registered.png)

      Azure Machine Learning Studio (klasik) kayıt başarılı olduğunda da güncellenir.

    ![Ağ geçidi kaydı başarılı](./media/use-data-from-an-on-premises-sql-server/gateway-registered.png)
11. Veri **ağ geçidini Karşıdan Yükle ve kaydet** iletişim kutusunda, kurulumu tamamlamak için onay işaretini tıklatın. **Ayarlar** sayfası ağ geçidi durumunu "Çevrimiçi" olarak görüntüler. Sağ bölmede durum ve diğer yararlı bilgileri bulabilirsiniz.

    ![Ağ geçidi ayarları](./media/use-data-from-an-on-premises-sql-server/gateway-status.png)
12. Microsoft Veri Yönetimi Ağ Geçidi Yapılandırma Yöneticisi'nde **Sertifika** sekmesine geçin. Bu sekmede belirtilen sertifika, portalda belirttiğiniz şirket içi veri deposunun kimlik bilgilerini şifrelemek/çözmek için kullanılır. Bu sertifika varsayılan sertifikadır. Microsoft, sertifika yönetim sisteminizde yedeklediğiniz bu sertifikayı kendi sertifikanızla değiştirmenizi önerir. Bunun yerine kendi sertifikanızı kullanmak için **Değiştir'i** tıklatın.

    ![Ağ geçidi sertifikasını değiştirme](./media/use-data-from-an-on-premises-sql-server/data-gateway-configuration-manager-certificate.png)
13. (isteğe bağlı) Ağ geçidiyle ilgili sorunları gidermek için ayrıntılı günlüğe kaydetmeyi etkinleştirmek istiyorsanız, Microsoft Veri Yönetimi Ağ Geçidi Yapılandırma Yöneticisi'nde **Tanılama** sekmesine geçin ve **sorun giderme amacıyla verbose günlüğe** kaydetme seçeneğini denetleyin. Günlük **bilgileri, Uygulamalar ve Hizmetler Günlükleri**  - &gt; **Veri Yönetimi Ağ Geçidi** düğümü altında Windows Event Viewer'da bulunabilir. Ağ geçidini kullanarak şirket içi bir veri kaynağına bağlantıyı sınamak için **Tanılama** sekmesini de kullanabilirsiniz.

    ![Ayrıntılı günlük kaydını etkinleştir](./media/use-data-from-an-on-premises-sql-server/data-gateway-configuration-manager-verbose-logging.png)

Bu, Azure Machine Learning Studio'daki (klasik) ağ geçidi kurulum işlemini tamamlar.
Artık şirket içi verilerinizi kullanmaya hazırsınız.

Studio'da (klasik) her çalışma alanı için birden çok ağ geçidi oluşturabilir ve ayarlayabilirsiniz. Örneğin, geliştirme sırasında test veri kaynaklarınıza bağlanmak istediğiniz bir ağ geçidiniz ve üretim veri kaynaklarınız için farklı bir ağ geçidiniz olabilir. Azure Machine Learning Studio (klasik), kurumsal ortamınıza bağlı olarak birden çok ağ geçidi kurma esnekliği sağlar. Şu anda çalışma alanları arasında bir ağ geçidi paylaşamaz ve tek bir bilgisayara yalnızca bir ağ geçidi yüklenebilir. Daha fazla bilgi için bkz. [Veri Yönetimi Ağ Geçidi ile verileri şirket içi kaynaklar ve bulut arasında taşıyın.](../../data-factory/tutorial-hybrid-copy-portal.md)

### <a name="step-2-use-the-gateway-to-read-data-from-an-on-premises-data-source"></a>Adım 2: Şirket içi veri kaynağından verileri okumak için ağ geçidini kullanın
Ağ geçidini ayarladıktan sonra, şirket içi SQL Server veritabanından verileri giriş yapan bir denemeye **Bir Alma Verisi** modülü ekleyebilirsiniz.

1. Machine Learning Studio'da (klasik), **DENEYLER** sekmesini seçin, sol alt köşede **+Yenİ'yi** tıklatın ve **Boş Deneme'yi** seçin (veya kullanılabilir birkaç örnek denemeden birini seçin).
2. **Veri Alma** modüllerini bulun ve deneme tuvaline sürükleyin.
3. Tuvalin altındaki **Kaydet'i** tıklatın. Deneme adı için "Azure Machine Learning Studio (klasik) Şirket İçi SQL Server Tutorial"ı girin, çalışma alanını seçin ve **Tamam** onay işaretini tıklatın.

   ![Yeni bir adla deneme kaydetme](./media/use-data-from-an-on-premises-sql-server/experiment-save-as.png)
4. Seçmek için **Veri İçe Aktar'ı** tıklatın ve ardından tuvalin sağındaki **Özellikler** bölmesinde **Veri kaynağı** açılır listesindeki "Şirket İçi SQL Veritabanı"nı seçin.
5. Yüklediğiniz ve kaydettiğiniz **Veri ağ geçidini** seçin. "(yeni Veri Ağ Geçidi ekle...)" seçeneğini seçerek başka bir ağ geçidi ayarlayabilirsiniz.

   ![Alma Verileri modülü için veri ağ geçidini seçin](./media/use-data-from-an-on-premises-sql-server/import-data-select-on-premises-data-source.png)
6. Yürütmek istediğiniz SQL **Veritabanı sorgusuyla** birlikte SQL **Veritabanı sunucu adını** ve Veritabanı **adını**girin.
7. Kullanıcı adı **ve parola** altında **değerleri girin** ve veritabanı kimlik bilgilerinizi girin'i tıklatın. Şirket içi SQL Server'ınızın nasıl yapılandırıldığına bağlı olarak Windows Tümleşik Kimlik Doğrulama veya SQL Server Kimlik Doğrulaması'nı kullanabilirsiniz.

   ![Veritabanı kimlik bilgilerini girin](./media/use-data-from-an-on-premises-sql-server/database-credentials.png)

   "Gerekli değerler" iletisi yeşil onay işaretiyle "değerler kümesi" olarak değişir. Veritabanı bilgileri veya parola lar değişmedikçe kimlik bilgilerini yalnızca bir kez girmeniz gerekir. Azure Machine Learning Studio (klasik), buluttaki kimlik bilgilerini şifrelemek için ağ geçidini yüklediğinizde sağladığınız sertifikayı kullanır. Azure, şirket içi kimlik bilgilerini şifreleme olmadan asla depolar.

   ![Veri modül özelliklerini alma](./media/use-data-from-an-on-premises-sql-server/import-data-properties-entered.png)
8. Denemeyi çalıştırmak için **KAÇ'ı** tıklatın.

Deneme çalışma bittikten sonra, **İçe Aktar Veri** modülünün çıkış bağlantı noktasını tıklatarak ve Visualize'ı seçerek veritabanından içe aktardığınız verileri **görselleştirebilirsiniz.**

Denemenizi geliştirmeyi bitirdikten sonra, modelinizi dağıtabilir ve işlevsel hale getirebilirsiniz. Toplu İşlem Eki hizmeti kullanılarak, **İçe İşlem Verileri** modülünde yapılandırılan şirket içi SQL Server veritabanından alınan veriler okunur ve puanlama için kullanılır. Şirket içi verileri puanlamak için İstek Yanıt Servisi'ni kullanabilirsiniz, ancak Microsoft bunun yerine [Excel Eklentisi'ni](excel-add-in-for-web-services.md) kullanmanızı önerir. Şu anda, **Dışa Aktarma Verileri** aracılığıyla şirket içi bir SQL Server veritabanına yazma denemelerinizde veya yayınlanan web hizmetlerinde desteklenmez.
