---
title: Şirket içi SQL Server
titleSuffix: ML Studio (classic) - Azure
description: Azure Machine Learning Studio (klasik) ile gelişmiş analizler gerçekleştirmek için bir şirket içi SQL Server veritabanından veri kullanın.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: likebupt
ms.author: keli19
ms.custom: seodec18
ms.date: 03/13/2017
ms.openlocfilehash: 9afac1adef801956f176dd339c795e2df533a2c7
ms.sourcegitcommit: bdf31d87bddd04382effbc36e0c465235d7a2947
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2020
ms.locfileid: "77169127"
---
# <a name="perform-analytics-with-azure-machine-learning-studio-classic-using-an-on-premises-sql-server-database"></a>Şirket içi SQL Server veritabanı kullanarak Azure Machine Learning Studio (klasik) analiz gerçekleştirme

Genellikle şirket içi verilerle çalışacak kuruluşların avantajı ölçek ve makine öğrenimi iş yükleri için bulutun çevikliğinden yapmak istiyorsunuz. Ancak, geçerli iş süreçleri ve iş akışları, şirket içi verileri buluta taşıyarak kesintiye istemiyorsanız. Azure Machine Learning Studio (klasik) artık verilerinizi şirket içi SQL Server veritabanından okumayı ve ardından bu verilerle bir modeli eğitmek ve Puanlama destekler. Artık bu el ile kopyalayın ve Bulut ve şirket içi sunucunuz arasında veri eşitlemeyi gerekmez. Bunun yerine, Azure Machine Learning Studio (klasik) **veri alma** modülü artık eğitim ve Puanlama işleriniz için şirket içi SQL Server veritabanından doğrudan okuyabilir.

Bu makalede, şirket içi SQL Server verilerinin Azure Machine Learning Studio (klasik) içine nasıl alınacağını gösteren bir genel bakış sunulmaktadır. Çalışma alanları, modüller, veri kümeleri, denemeleri *vb.* gibi Studio (klasik) kavramlarıyla ilgili bilgi sahibi olduğunuzu varsayar.

> [!NOTE]
> Bu özellik, ücretsiz çalışma alanları için kullanılamaz. Machine Learning fiyatlandırması ve katmanları hakkında daha fazla bilgi için bkz. [Azure Machine Learning fiyatlandırması](https://azure.microsoft.com/pricing/details/machine-learning/).
>
>

<!-- -->



## <a name="install-the-data-factory-self-hosted-integration-runtime"></a>Data Factory şirket içinde barındırılan Integration Runtime'ı yükleme
Azure Machine Learning Studio (klasik) ' de şirket içi SQL Server veritabanına erişmek için, daha önce Veri Yönetimi ağ geçidi olarak bilinen Data Factory kendi kendine barındırılan Integration Runtime indirip yüklemeniz gerekir. Bağlantıyı Machine Learning Studio (klasik) ' de yapılandırdığınızda, aşağıda açıklanan **veri ağ geçidini indir ve Kaydet** iletişim kutusunu kullanarak INTEGRATION RUNTIME (IR) indirme ve yükleme şansınız vardır.


Ayrıca, [Microsoft Indirme merkezi](https://www.microsoft.com/download/details.aspx?id=39717)' nden MSI kurulum paketini indirip çalıştırarak da daha önce IR yükleyebilirsiniz. MSI Ayrıca, mevcut bir IR 'yi en son sürüme yükseltmek için kullanılabilir ve tüm ayarlar korunur.

Data Factory şirket içinde barındırılan Integration Runtime, aşağıdaki önkoşulları vardır:

* Data Factory şirket içinde barındırılan Integration bir 64-bit işletim sistemi .NET Framework 4.6.1 veya üstü gerektirir.
* Desteklenen Windows işletim sistemi sürümleri olan Windows 10, Windows Server 2012, Windows Server 2012 R2, Windows Server 2016. 
* IR makine için önerilen yapılandırma, en az 2 GHz, 4 çekirdekli CPU, 8 GB RAM ve 80 GB disk ' dir.
* Konak makine hazırda bekleme, IR veri isteklerine yanıt vermiyor. Bu nedenle, IR yüklemeden önce uygun bir güç planı bilgisayarda yapılandırma Makine hazırda bekleme için yapılandırılmışsa, IR yükleme bir ileti görüntüler.
* Kopyalama etkinliği, belirli bir sıklıkta oluştuğundan, makine üzerinde kaynak kullanımı (CPU, bellek), ayrıca yoğun ve boş kalma sürelerinde ile aynı deseni izler. Kaynak Kullanımı Yoğun taşınan veri miktarı da bağlıdır. Birden çok kopyası işleri devam ederken, kaynak kullanımı yoğun zamanlarda Yukarı Git gözlemleyin. Yukarıda listelenen en düşük yapılandırmayı teknik olarak yeterli olsa da, en düşük yapılandırmayı veri taşıma için belirli yük bağlı olarak daha fazla kaynak ile bir yapılandırmaya sahip isteyebilirsiniz.

Ayarlama ve Data Factory şirket içinde barındırılan Integration Runtime'ı kullanırken aşağıdakileri göz önünde bulundurun:

* IR yalnızca bir örneği, tek bir bilgisayara yükleyebilirsiniz.
* Birden çok şirket içi veri kaynakları için tek bir IR kullanabilirsiniz.
* Farklı bilgisayarlara birden fazla IRS aynı şirket içi veri kaynağına bağlanabilirsiniz.
* Bir IRS tek seferde yalnızca bir çalışma alanı için yapılandırırsınız. Şu anda IRS çalışma alanları arasında paylaşılamaz.
* Tek bir çalışma alanı için birden çok IRS yapılandırabilirsiniz. Örneğin, geliştirme sırasında test veri kaynaklarınıza bağlanmış bir IR ve kullanıma hazır olduğunuzda bir üretim IR kullanmak isteyebilirsiniz.
* IR veri kaynağı ile aynı makinede olması gerekmez. Ancak, veri kaynağına daha yakından kaldığını ağ geçidinin veri kaynağına bağlanmak süreyi azaltır. Kaynaklar için ağ geçidi ve veri kaynağı yoksa rekabet. böylece şirket içi veri kaynağı barındıran olandan farklı bir makineye IR yüklemenizi öneririz.
* Bilgisayarınızda Power BI veya Azure Data Factory senaryolarına hizmet eden bir IR zaten varsa, başka bir bilgisayara Azure Machine Learning Studio (klasik) için ayrı bir IR yüklersiniz.

  > [!NOTE]
  > Data Factory şirket içinde barındırılan Integration Runtime ve Power BI Gateway aynı bilgisayarda çalıştıramazsınız.
  >
  >
* Diğer veriler için Azure ExpressRoute kullanıyor olsanız bile Azure Machine Learning Studio (klasik) için Data Factory kendinden konak Integration Runtime kullanmanız gerekir. Veri kaynağı (bir güvenlik duvarının arkasında olan) bir şirket içi veri kaynağı olarak kabul etmelidir da ExpressRoute kullanırken. Data Factory şirket içinde barındırılan Integration Runtime, makine öğrenimi ve veri kaynağı arasında bağlantı kurmak için kullanın.

Yükleme önkoşulları, yükleme adımları ve sorun giderme ipuçları hakkında ayrıntılı bilgileri [Data Factory Integration Runtime](../../data-factory/concepts-integration-runtime.md)makalesinde bulabilirsiniz.

## <a name="span-idusing-the-data-gateway-step-by-step-walk-classanchorspan-id_toc450838866-classanchorspanspaningress-data-from-your-on-premises-sql-server-database-into-azure-machine-learning"></a><span id="using-the-data-gateway-step-by-step-walk" class="anchor"><span id="_Toc450838866" class="anchor"></span></span>Şirket içi SQL Server veritabanınızdaki verileri Azure Machine Learning olarak alma
Bu kılavuzda, bir Azure Machine Learning çalışma alanında Azure Data Factory Integration Runtime ayarlayacaksınız, yapılandırıp sonra şirket içi SQL Server veritabanından verileri okuyacaksınız.

> [!TIP]
> Başlamadan önce tarayıcınızın açılır pencere engelleyicisini `studio.azureml.net`devre dışı bırakın. Google Chrome tarayıcısını kullanıyorsanız, Google Chrome WebStore ['](https://chrome.google.com/webstore/search/clickonce?_category=extensions)da bulunan çeşitli eklentilerden birini indirip yükleyin.
>
> [!NOTE]
> Azure Data Factory şirket içinde barındırılan Integration Runtime, daha önce veri yönetimi ağ geçidi biliniyordu. Adım adım öğretici için bir ağ geçidi olarak başvurmaya devam edecek.  

### <a name="step-1-create-a-gateway"></a>1\. adım: bir ağ geçidi oluşturma
İlk adım, oluşturmak ve şirket içi SQL veritabanınıza erişmek için ağ geçidi ayarlama sağlamaktır.

1. [Azure Machine Learning Studio (klasik)](https://studio.azureml.net/Home/) oturumunu açın ve içinde çalışmak istediğiniz çalışma alanını seçin.
2. Sol taraftaki **Ayarlar** dikey penceresine tıklayın ve sonra ÜSTTEKI **veri ağ geçitleri** sekmesine tıklayın.
3. Ekranın alt kısmındaki **yenı VERI ağ geçidi** ' ne tıklayın.

    ![Yeni veri ağ geçidi](./media/use-data-from-an-on-premises-sql-server/new-data-gateway-button.png)
4. **Yeni veri ağ geçidi** iletişim kutusunda, **ağ geçidi adını** girin ve Isteğe bağlı olarak bir **Açıklama**ekleyin. Alt sağ köşesinde yapılandırmasının sonraki adıma geçmek için oka tıklayın.

    ![Ağ geçidi ad ve açıklama girin](./media/use-data-from-an-on-premises-sql-server/new-data-gateway-dialog-enter-name.png)
5. Yükleme ve kaydetme veri ağ geçidi iletişim kutusunda, ağ geçidi kayıt ANAHTARINI panoya kopyalayın.

    ![İndirme ve veri ağ geçidini kaydetme](./media/use-data-from-an-on-premises-sql-server/download-and-register-data-gateway.png)
6. <span id="note-1" class="anchor"></span>Henüz Microsoft Veri Yönetimi ağ geçidini indirip yüklemediyseniz, **veri yönetimi ağ geçidini indir**' e tıklayın. Bu Microsoft Download, indirin, ihtiyacınız olan ağ geçidi sürümü seçin ve yükleme Center götürür. Yükleme önkoşulları, yükleme adımları ve sorun giderme ipuçları hakkında ayrıntılı bilgileri makalenin başlangıç bölümlerinde bulabilirsiniz. [veri yönetimi ağ geçidi ile şirket içi kaynaklar ve bulut arasında veri taşıma](../../data-factory/tutorial-hybrid-copy-portal.md).
7. Ağ Geçidi yüklendikten sonra Veri Yönetimi ağ geçidi Configuration Manager açılır ve **ağ geçidi kaydet** iletişim kutusu görüntülenir. Panoya kopyaladığınız **ağ geçidi kayıt anahtarını** yapıştırın ve **Kaydet**' e tıklayın.
8. Yüklü bir ağ geçidi zaten varsa, veri yönetimi ağ geçidi Yapılandırma Yöneticisi'ni çalıştırın. **Anahtarı Değiştir**' e tıklayın, önceki adımda panoya kopyaladığınız **ağ geçidi kayıt anahtarını** yapıştırın ve **Tamam**' a tıklayın.
9. Yükleme tamamlandığında, Microsoft Veri Yönetimi Gateway Configuration Manager için **ağ geçidi kaydet** iletişim kutusu görüntülenir. Önceki adımda panoya kopyaladığınız ağ GEÇIDI kayıt anahtarını yapıştırın ve **Kaydet**' e tıklayın.

    ![Ağ geçidini kaydetme](./media/use-data-from-an-on-premises-sql-server/data-gateway-configuration-manager-register-gateway.png)
10. Microsoft Veri Yönetimi Gateway Configuration Manager **giriş** sekmesinde aşağıdaki değerler ayarlandığında ağ geçidi yapılandırması tamamlanır:

    * **Ağ geçidi adı** ve **örnek adı** , ağ geçidinin adına ayarlanır.
    * **Kayıt** **kaydedildi**olarak ayarlandı.
    * **Durum** **başlatıldı**olarak ayarlandı.
    * Alttaki durum çubuğu, bir yeşil onay işaretiyle birlikte **veri yönetimi ağ geçidi bulut hizmetine bağlı olarak** görüntülenir.

      ![Veri Yönetimi Ağ Geçidi Yöneticisi](./media/use-data-from-an-on-premises-sql-server/data-gateway-configuration-manager-registered.png)

      Azure Machine Learning Studio (klasik), kayıt başarılı olduğunda da güncellenir.

    ![Başarılı ağ geçidi kaydı](./media/use-data-from-an-on-premises-sql-server/gateway-registered.png)
11. **Veri ağ geçidini indir ve Kaydet** iletişim kutusunda, kurulumu gerçekleştirmek için onay işaretine tıklayın. **Ayarlar** sayfası ağ geçidi durumunu "çevrimiçi" olarak görüntüler. Sağ bölmede, durumunu ve diğer yararlı bilgiler bulabilirsiniz.

    ![Ağ Geçidi ayarları](./media/use-data-from-an-on-premises-sql-server/gateway-status.png)
12. Microsoft Veri Yönetimi ağ geçidi Configuration Manager **sertifika** sekmesine geçin. Bu sekmede belirtilen sertifika, portalda belirttiğiniz şirket içi veri deposunun kimlik bilgilerini şifrelemek/şifrelerini çözmek için kullanılır. Bu sertifika, varsayılan sertifikadır. Microsoft, bu sertifika yönetimi sisteminizde yedeklemeniz kendi sertifikanızı değiştirerek önerir. Bunun yerine kendi sertifikanızı kullanmak için **Değiştir** ' e tıklayın.

    ![Ağ geçidi sertifikayı Değiştir](./media/use-data-from-an-on-premises-sql-server/data-gateway-configuration-manager-certificate.png)
13. seçim Ağ geçidiyle ilgili sorunları gidermek için ayrıntılı günlük kaydını etkinleştirmek istiyorsanız, Microsoft Veri Yönetimi ağ Configuration Manager geçidinde, **Tanılama** sekmesine geçin ve **sorun giderme amacıyla ayrıntılı günlüğü etkinleştir** seçeneğini işaretleyin. Günlüğe kaydetme bilgileri, **uygulama ve hizmet günlükleri** -&gt; **veri yönetimi ağ geçidi** düğümü altında Windows Olay Görüntüleyicisi bulunabilir. Ağ geçidini kullanarak şirket içi veri kaynağına bağlantıyı test etmek için **Tanılama** sekmesini de kullanabilirsiniz.

    ![Ayrıntılı günlüğe yazmayı etkinleştir](./media/use-data-from-an-on-premises-sql-server/data-gateway-configuration-manager-verbose-logging.png)

Bu işlem, Azure Machine Learning Studio (klasik) ağ geçidi kurulum işlemini tamamlar.
Artık, şirket içi verileri kullanmaya hazırsınız.

Her çalışma alanı için Studio 'da (klasik) birden çok ağ geçidi oluşturabilir ve ayarlayabilirsiniz. Örneğin, geliştirme sırasında test veri kaynaklarınıza bağlanmak istediğiniz bir ağ geçidi farklı bir ağ geçidi olarak da, üretim veri kaynakları için sahip olabilir. Azure Machine Learning Studio (klasik), kurumsal ortamınıza bağlı olarak birden çok ağ geçidi ayarlama esnekliği sağlar. Şu anda çalışma alanları arasında bir ağ geçidi paylaşamaz ve yalnızca bir ağ geçidi tek bir bilgisayara yüklenebilir. Daha fazla bilgi için bkz. [veri yönetimi ağ geçidi ile şirket içi kaynaklar ve bulut arasında veri taşıma](../../data-factory/tutorial-hybrid-copy-portal.md).

### <a name="step-2-use-the-gateway-to-read-data-from-an-on-premises-data-source"></a>2\. adım: ağ geçidini bir şirket içi veri kaynağından verileri okumak için kullanın.
Ağ geçidini ayarladıktan sonra, verileri şirket içi SQL Server veritabanından veren bir denemeye **Içeri aktarma verileri** modülü ekleyebilirsiniz.

1. Machine Learning Studio (klasik) içinde, **denemeleri** sekmesini seçin, sol alt köşedeki **+ Yeni** ' ye tıklayın ve **boş denemeler** ' i seçin (veya kullanılabilir örnek denemeleri birini seçin).
2. **Veri alma** modülünü bulun ve deneme tuvaline sürükleyin.
3. Tuvalin altındaki **farklı kaydet** ' e tıklayın. Deneme adı için "Azure Machine Learning Studio (klasik) Şirket Içi SQL Server öğreticisini girin, çalışma alanını seçin ve **Tamam** onay işaretine tıklayın.

   ![Deneme yeni bir adla kaydet](./media/use-data-from-an-on-premises-sql-server/experiment-save-as.png)
4. **Verileri Içeri aktar** modülünü seçin ve ardından tuvalin sağındaki **Özellikler** bölmesinde, **veri kaynağı** açılır listesinde "Şirket içi SQL veritabanı" ' nı seçin.
5. Yüklediğiniz ve kaydettiğiniz **veri ağ geçidini** seçin. Başka bir ağ geçidi "(yeni bir veri ağ geçidi Ekle …)"'i seçerek ayarlayabilirsiniz.

   ![Verileri içeri aktarma modülü için veri ağ geçidi seçin](./media/use-data-from-an-on-premises-sql-server/import-data-select-on-premises-data-source.png)
6. Yürütmek istediğiniz SQL **veritabanı sorgusunun** yanı sıra SQL **veritabanı sunucu adını** ve **veritabanı adını**girin.
7. **Kullanıcı adı ve parola** altında **değer girin** ' e tıklayın ve veritabanı kimlik bilgilerinizi girin. Windows tümleşik kimlik doğrulaması veya SQL Server şirket içi SQL Server'ınızı nasıl yapılandırıldığına bağlı olarak kimlik doğrulaması kullanabilirsiniz.

   ![Veritabanı kimlik bilgileri girin](./media/use-data-from-an-on-premises-sql-server/database-credentials.png)

   Yeşil onay işaretiyle birlikte "değerleri kümesi" ileti "gerekli değerleri" değişiklikler. Veritabanı bilgileri veya parola değiştirilmediği sürece kimlik bilgilerinin bir kere girmeniz yeterlidir. Azure Machine Learning Studio (klasik), Bulutta kimlik bilgilerini şifrelemek için ağ geçidini yüklediğinizde verdiğiniz sertifikayı kullanır. Azure, hiçbir zaman şifreleme olmadan şirket içi kimlik bilgilerini depolar.

   ![Veri modülü özelliklerini alma](./media/use-data-from-an-on-premises-sql-server/import-data-properties-entered.png)
8. Denemeyi çalıştırmak için **Çalıştır** ' a tıklayın.

Deneme çalışmayı bitirdiğinde, **veri alma** modülünün çıkış bağlantı noktasına tıklayıp **Görselleştir**' i seçerek veritabanından içeri aktardığınız verileri görselleştirebilirsiniz.

Geliştirme deneyiminizi tamamladıktan sonra dağıtabilir ve modelinizi kullanıma hazır hale getirin. Toplu yürütme hizmetini kullanarak **veri Içeri aktarma** modülünde yapılandırılan şirket içi SQL Server veritabanındaki veriler okunacaktır ve skor için kullanılacaktır. Puanlama için Istek yanıtı hizmetini şirket içi veriler için kullanabilirsiniz, Microsoft bunun yerine [Excel eklentisinin](excel-add-in-for-web-services.md) kullanılmasını önerir. Şu anda, **verileri dışarı aktarma** yoluyla şirket içi SQL Server veritabanına yazma denemeleri veya yayımlanmış Web hizmetlerinde desteklenmez.
