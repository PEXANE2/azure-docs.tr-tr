---
title: IBM ana bilgisayarlarındaki 3270 uygulamaya bağlanma
description: Azure Logic Apps ve IBM 3270 konektörünü kullanarak Azure ile 3270 ekran ayarı uygulamaları entegre edin ve otomatikleştirin
services: logic-apps
ms.suite: integration
author: ChristopherHouser
ms.author: chrishou
ms.reviewer: estfan, valthom
ms.topic: article
ms.date: 03/06/2019
tags: connectors
ms.openlocfilehash: 808eef5424d678559ae94ffd04e41eacd0f16aee
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80371112"
---
# <a name="integrate-3270-screen-driven-apps-on-ibm-mainframes-with-azure-by-using-azure-logic-apps-and-ibm-3270-connector"></a>Azure Logic Apps ve IBM 3270 konektörünü kullanarak IBM ana bilgisayarlarında 3270 ekran ayarı uygulamaları Azure ile tümleştirin

> [!NOTE]
> Bu bağlayıcı [*genel önizlemede.*](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) 

Azure Logic Apps ve IBM 3270 konektörü ile, genellikle 3270 emülatör ekranlarda gezinerek yönlendirdiğiniz IBM ana bilgisayar uygulamalarına erişebilir ve çalıştırabilirsiniz. Bu şekilde, Azure Logic Apps ile otomatik iş akışları oluşturarak IBM ana bilgisayar uygulamalarınızı Azure, Microsoft ve diğer uygulamalar, hizmetler ve sistemlerle tümleştirebilirsiniz. Bağlayıcı, TN3270 protokolünü kullanarak IBM ana bilgisayarlarıyla iletişim kurar ve Azure Kamu Yönetimi ve Azure China 21Vianet hariç tüm Azure Logic Apps bölgelerinde kullanılabilir. Mantıksal uygulamalarda yeniyseniz, [Azure Mantık Uygulamaları nedir'yi inceleyin?](../logic-apps/logic-apps-overview.md)

Bu makalede, 3270 konektörü kullanmak için bu yönleri açıklanır: 

* Azure Logic Apps'ta IBM 3270 konektörünü neden ve bağlayıcının 3270 ekran ayarı uygulamaları nasıl çalıştırdığını kullanman?

* 3270 konektörü kullanmak için ön koşullar ve kurulum

* Mantık uygulamanıza 3270 konektör eylemi ekleme adımları

## <a name="why-use-this-connector"></a>Neden bu konektörü kullanıyorsun?

IBM ana bilgisayarlarındaki uygulamalara erişmek için genellikle genellikle "yeşil ekran" olarak adlandırılan 3270 terminal emülatörü kullanırsınız. Bu yöntem zaman test edilmiş bir yoldur, ancak sınırlamaları vardır. Ana Bilgisayar Tümleştirme Sunucusu (HIS) bu uygulamalarla doğrudan çalışmanıza yardımcı olsa da, bazen ekranı ve iş mantığını ayırmak mümkün olmayabilir. Veya, belki artık ana bilgisayar uygulamalarının nasıl çalıştığıhakkında bilginiz yoktur.

Bu senaryoları genişletmek için Azure Logic Apps'taki IBM 3270 konektörü, belirli bir görev için kullanılan ana ekranekranlarını kaydetmek veya "yakalamak" için kullandığınız 3270 Tasarım Aracı ile birlikte çalışır, ana bilgisayar uygulamanız aracılığıyla bu göreviçin gezinti akışını tanımlar ve bu görev için giriş ve çıkış parametreleri ile yöntemleri. Tasarım aracı, bu bilgileri 3270 bağlayıcısının mantık uygulamanızdan bu görevi temsil eden bir eylemi ararken kullandığı meta verilere dönüştürür.

Tasarım aracından meta veri dosyasını oluşturduktan sonra, bu dosyayı Azure'daki bir tümleştirme hesabına eklersiniz. Bu şekilde, 3270 bağlayıcı eylemi eklediğinizde mantık uygulamanız uygulamanızın meta verilerine erişebilir. Bağlayıcı, tümleştirme hesabınızdaki meta veri dosyasını okur, 3270 ekranlarda gezinmeyi işler ve 3270 bağlayıcı eyleminin parametrelerini dinamik olarak sunar. Daha sonra ana bilgisayar uygulamasına veri sağlayabilirsiniz ve bağlayıcı sonuçları mantık uygulamanıza döndürür. Bu şekilde, eski uygulamalarınızı Azure, Microsoft ve Azure Logic Apps'ın desteklediği diğer uygulamalar, hizmetler ve sistemlerle tümleştirebilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/).

* [Mantık uygulamaları oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md) hakkında temel bilgiler

* Önerilen: Bir [entegrasyon hizmet ortamı (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment.md) 

  Bu ortamı, mantık uygulamanızı oluşturmak ve çalıştırmak için konum olarak seçebilirsiniz. Bir ISE, mantıksal uygulamanızdan Azure sanal ağları içinde korunan kaynaklara erişim sağlar.

* 3270 ekran tabanlı uygulamanızı otomatikleştirmek ve çalıştırmak için kullanılacak mantık uygulaması

  IBM 3270 konektörütetikleyicisi yoktur, bu nedenle **yineleme** tetikleyicisi gibi mantık uygulamanızı başlatmak için başka bir tetikleyici kullanın. Daha sonra 3270 bağlayıcı eylemleri ekleyebilirsiniz. Başlamak için [boş bir mantık uygulaması oluşturun.](../logic-apps/quickstart-create-first-logic-app-workflow.md) 
  Bir İmKB kullanıyorsanız, mantık uygulamanızın konumu olarak o İmKB'yi seçin.

* [3270 Tasarım Aracını indirin ve kurun.](https://aka.ms/3270-design-tool-download)
Tek ön koşul [Microsoft .NET Framework 4.6.1..](https://aka.ms/net-framework-download)

  Bu araç, ekleyip 3270 konektör eylemleri olarak çalıştırdığınız uygulamanızdaki görevlerin ekranlarını, gezinme yollarını, yöntemlerini ve parametrelerini kaydetmenize yardımcı olur. Araç, ana bilgisayar uygulamanızı kullanmak için konektör için gerekli meta verileri sağlayan bir Ana Bilgisayar Tümleştirme Tasarımcısı XML (HIDX) dosyası oluşturur.
  
  Bu aracı indirip yükledikten sonra, ana bilgisayara bağlanmak için aşağıdaki adımları izleyin:

  1. 3270 Tasarım Aracını açın. **Oturum** menüsünden, **Toplantı Alanı Oturumları'nı**seçin.
  
  1. TN3270 ana bilgisayar sunucu bilgilerinizi sağlayın.

* Mantıksal uygulamanızın bu dosyadaki meta verilere ve yöntem tanımlarına erişebilmeleri için HIDX dosyanızı harita olarak depoladığınız bir [entegrasyon hesabıdır.](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) 

  Tümleştirme hesabınızın kullanmakta olduğunuz mantık uygulamasına bağlı olduğundan emin olun. Ayrıca, Bir İmKB kullanıyorsanız, entegrasyon hesabınızın konumunun mantık uygulamanızın kullandığı İmKB ile aynı olduğundan emin olun.

* Ana bilgisayar uygulamanızı barındıran TN3270 sunucusuna erişim

<a name="define-app-metadata"></a>

## <a name="create-metadata-overview"></a>Meta verilere genel bakış oluşturma

3270 ekran tabanlı bir uygulamada, ekranlar ve veri alanları senaryolarınıza özgüolduğundan, 3270 konektörü uygulamanız hakkında meta veri olarak sağlayabileceğiniz bu bilgilere ihtiyaç duyar. Bu meta veriler, mantık uygulamanızın ekranları tanımlamasına ve tanımasına yardımcı olan bilgileri açıklar, ekranlar arasında nasıl gezinilir, verileri nereden giriş e-reciare yerleştirir ve sonuçları nerede bekleyebileceğinizi açıklar. Bu meta verileri belirtmek ve oluşturmak için, daha sonra daha ayrıntılı olarak açıklandığı gibi, bu belirli modlarda (veya *aşamalarda)* size yol gösteren 3270 Tasarım Aracı'nı kullanırsınız:

* **Yakalama**: Bu modda, belirli bir görevi tamamlamak için gereken ekranları ana bilgisayar uygulamanızla (örneğin banka bakiyesi almak) kaydedersiniz.

* **Gezinme**: Bu modda, belirli bir görev için ana bilgisayar uygulamanızın ekranlarında gezinmenin planını veya yolunu belirtirsiniz.

* **Yöntem**: Bu modda, örneğin, ekran `GetBalance`gezinti yolunu açıklayan yöntemi tanımlarsınız. Ayrıca, her ekranda yöntemin giriş ve çıkış parametreleri haline gelen alanları da seçersiniz.

### <a name="unsupported-elements"></a>Desteklenmeyen öğeler

Tasarım aracı bu öğeleri desteklemez:

* Kısmi IBM Temel Haritalama Desteği (BMS) haritaları: Bir BMS eşlem içe aktarıyorsanız, tasarım aracı kısmi ekran tanımlarını yok sayar.
* Katılım/Çıkış parametreleri: In/Out parametrelerini tanımlayamam.
* Menü işleme: Önizleme sırasında desteklenmiyor
* Dizi işleme: Önizleme sırasında desteklenmiyor

<a name="capture-screens"></a>

## <a name="capture-screens"></a>Ekranları yakalama

Bu modda, her 3270 ekranda bu ekranı benzersiz olarak tanımlayan bir öğe yi işaretlersiniz. Örneğin, belirli bir metin ve boş olmayan alan gibi bir metin satırı veya daha karmaşık bir koşul kümesi belirtebilirsiniz. Bu ekranları ana bilgisayar sunucusuna canlı bağlantı üzerinden kaydedebilir veya bu bilgileri bir IBM Temel Eşleme Desteği (BMS) haritasından içe aktarabilirsiniz. Canlı bağlantı, ana bilgisayara bağlanmak için bir TN3270 emülatörü kullanır. Her bağlayıcı eylemi, oturumunuza bağlanmayla başlayan ve oturumunuzun bağlantısının kesilmesiyle sona eren tek bir göreve eşlenmelidir.

1. Henüz yapmadıysanız, 3270 Tasarım Aracı'nı açın. Araç çubuğunda, Yakalama moduna girebilmek için **Yasla'yı** seçin.

1. Kaydı başlatmak için F5 tuşuna basın veya **Kayıt** menüsünden **Kayıt Başlat'ı**seçin. 

1. **Oturum** menüsünden **Bağlan'ı**seçin.

1. **Uygulamanızdaki** ilk ekrandan başlayarak Yakalama bölmesinde, kaydettiğiniz belirli bir görev için uygulamanızda adım atın.

1. Görevi tamamladıktan sonra, uygulamanızdan her zaman yaptığınız gibi çıkış yapın.

1. **Oturum** menüsünden **Bağlantıyı Kesme'yi**seçin.

1. Kaydı durdurmak için Shift + F5 tuşlarına veya **Kayıt** menüsünden **Kaydı Durdur'u**seçin.

   Bir görev için ekranları yakaladıktan sonra, tasarımcı aracı bu ekranları temsil eden küçük resimleri gösterir. Bu küçük resimler hakkında bazı notlar:

   * Yakalanan ekranlarınıza "Boş" adlı bir ekrana sahipsiniz.

     [CICS'e](https://www.ibm.com/it-infrastructure/z/cics)ilk bağlandığınızda, çalıştırmak istediğiniz işlemin adını girebilmek için önce "Temizle" tuşunu göndermeniz gerekir. "Temizle" tuşunu gönderdiğiniz ekranda Ekran Tanıma düzenleyicisini kullanarak ekleyebileceğiniz ekran başlığı gibi herhangi bir *tanıma özelliği*yoktur. Bu ekranı temsil etmek için küçük resimlerde "Boş" adlı bir ekran vardır. Daha sonra bu ekranı hareket adını girdiğiniz ekranı temsil etmek için kullanabilirsiniz.

   * Varsayılan olarak, yakalanan bir ekranın adı ekrandaki ilk sözcüğü kullanır. Bu ad zaten varsa, tasarım aracı adı bir alt alt puan ve bir sayı ile ekler, örneğin, "WBGB" ve "WBGB_1".

1. Yakalanan ekrana daha anlamlı bir ad vermek için aşağıdaki adımları izleyin:

   1. Ana **Bilgisayar Ekranları** bölmesinde, yeniden adlandırmak istediğiniz ekranı seçin.

   1. Aynı bölmede, aynı bölmede alt kısmında, **Ekran Adı** özelliğini bulun.

   1. Geçerli ekran adını daha açıklayıcı bir adla değiştirin.

1. Şimdi her ekranı tanımlamak için alanları belirtin.

   3270 veri akışı yla ekranlarda varsayılan tanımlayıcılar yoktur, bu nedenle her ekranda benzersiz bir metin seçmeniz gerekir. Karmaşık senaryolar için, benzersiz metin ve belirli bir koşula sahip bir alan gibi birden çok koşul belirtebilirsiniz.

Tanıma alanlarını seçtikten sonra bir sonraki moda geçin.

### <a name="conditions-for-identifying-repeated-screens"></a>Yinelenen ekranları tanımlama koşulları

Bağlayıcının ekranlar arasında gezinmek ve ekranlar arasında ayrım yapmak için, genellikle yakalanan ekranlar arasında tanımlayıcı olarak kullanabileceğiniz bir ekranda benzersiz metin bulursunuz. Yinelenen ekranlar için daha fazla tanımlama yöntemi gerekebilir. Örneğin, bir ekran geçerli bir değer döndürürken diğer ekran bir hata iletisi döndürür dışında aynı görünen iki ekranolduğunu varsayalım.

Tasarım aracında, Ekran Tanıma düzenleyicisini kullanarak *tanıma öznitelikleri*(örneğin, "Hesap Bakiyesi Al" gibi bir ekran başlığı ekleyebilirsiniz. Çatallı bir yolunuz varsa ve her iki dal da aynı ekranı döndürse ancak farklı sonuçlarla başka tanıma özniteliklerine ihtiyacınız vardır. Çalışma zamanında, bağlayıcı geçerli dalı ve çatalı belirlemek için bu öznitelikleri kullanır. Kullanabileceğiniz koşullar şunlardır:

* Belirli değer: Bu değer belirtilen konumda belirtilen dize yle eşleşir.
* Değİl belirli bir değer: Bu değer belirtilen konumda belirtilen dize eşleşmiyor.
* Boş: Bu alan boş.
* BOŞ Değİl: Bu alan boş değil.

Daha fazla bilgi edinmek için, bu konudadaha sonra [Örnek navigasyon planına](#example-plan) bakın.

<a name="define-navigation"></a>

## <a name="define-navigation-plans"></a>Navigasyon planlarını tanımlama

Bu modda, belirli göreviniz için ana bilgisayar uygulamanızın ekranlarında gezinmek için akışı veya adımları tanımlarsınız. Örneğin, bazen uygulamanızın bir yolun doğru sonucu ürettiği durumlarda kullanabileceği birden fazla yolunuz olabilirken, diğer yol bir hata üretir. Her ekran için, bir sonraki ekrana geçmek için gereken `CICSPROD <enter>`tuş vuruşlarını belirtin, örneğin.

> [!TIP]
> Aynı bağlantı ve ekranları kesen birkaç görevi otomatikleştiriyorsanız, tasarım aracı özel Bağlantı ve Bağlantı Planı türleri sağlar. Bu planları tanımladığınızda, bunları gezinti planınızın başlangıcına ve sonuna ekleyebilirsiniz.

### <a name="guidelines-for-plan-definitions"></a>Plan tanımları için yönergeler

* Bağlantı yla başlayan ve bağlantıyı kesmeyle biten tüm ekranları ekleyin.

* Bağımsız bir plan oluşturabilir veya tüm işlemlerinizde ortak olan bir dizi ekranı yeniden kullanmanıza izin veren Bağlan ve Bağlantıyı Kesme planlarını kullanabilirsiniz.

  * Connect planınızdaki son ekran, navigasyon planınızdaki ilk ekranla aynı olmalıdır.

  * Bağlantıyı Kesme planınızdaki ilk ekran, gezinti planınızdaki son ekranla aynı olmalıdır.

* Yakalanan ekranlarınız çok sayıda yinelenen ekran içerebilir, bu nedenle planınızda tekrarlanan ekranların yalnızca bir örneğini seçin ve kullanın. Yinelenen ekranlardan bazı örnekler aşağıda verilmiştir:

  * Oturum açma ekranı, örneğin, **MSG-10** ekranı
  * CICS için hoş geldin ekranı
  * "Açık" veya **Boş** ekran

<a name="create-plans"></a>

### <a name="create-plans"></a>Plan oluşturma

1. 3270 Tasarım Aracı'nın araç çubuğunda, Gezinti moduna girmek için **Gezinti'yi** seçin.

1. Planınızı başlatmak için **Gezinti** bölmesinde **Yeni Plan'ı**seçin.

1. **Yeni Plan Adı Seçin**altında, planınız için bir ad girin. **Tür** listesinden plan türünü seçin:

   | Plan türü | Açıklama |
   |-----------|-------------|
   | **İşleme** | Bağımsız veya birleşik planlar için |
   | **Bağlan** | Connect planları için |
   | **Bağlantıyı kes** | Bağlantı Kesme planları için |
   |||

1. Ana **Bilgisayar Ekranları** bölmesinden, yakalanan küçük resimleri **Gezinti** bölmesindeki gezinti planı yüzeyine sürükleyin.

   Hareket adını girdiğiniz boş ekranı temsil etmek için "Boş" ekranını kullanın.

1. Ekranları tanımladığınız görevi açıklayan sırada düzenleyin.

1. Tasarım aracının araç çubuğunda çatal lar ve birleştirmeler de dahil olmak üzere ekranlar arasındaki akış yolunu tanımlamak için **Akış'ı**seçin.

1. Akıştaki ilk ekranı seçin. Akışıbir sonraki ekrana bir bağlantı sürükleyin ve çizin.

1. Her ekran **için, AID Key** özelliği (Dikkat Tanımlayıcısı) ve akışı bir sonraki ekrana taşıyan **Sabit Metin** özelliği için değerleri sağlayın.

   Yalnızca AID tuşu veya hem AID tuşu hem de sabit metin olabilir.

Gezinti planınızı tamamladıktan sonra, [bir sonraki modda yöntemleri tanımlayabilirsiniz.](#define-method)

<a name="example-plan"></a>

### <a name="example"></a>Örnek

Bu örnekte, "WBGB" adlı ve şu adımlara sahip bir CICS hareketi çalıştırdığınızı varsayalım: 

* İlk ekranda bir ad ve hesap girersiniz.
* İkinci ekranda, hesap bakiyesini alırsınız.
* "Boş" ekranına çıkıyorsunuz.
* CICS'den "MSG-10" ekranına çıkış yapıyorsun.

Ayrıca, bu adımları yinelediğinizi, ancak hatayı gösteren ekranı yakalayabilmek için yanlış veri girdiğinizi varsayalım. İşte yakaladığınız ekranlar:

* MSG-10
* CICS Hoşgeldiniz
* Boş
* WBGB_1 (giriş)
* WBGB_2 (hata)
* Empty_1
* MSG-10_1

Birçok ekran burada benzersiz adlar almak rağmen, bazı ekranlar aynı ekran, örneğin, "MSG-10" ve "Boş". Yinelenen bir ekran için, planınızdaki bu ekran için yalnızca bir örnek kullanın. Bağımsız bir planın, Connect planının, Bağlantıyı Kesme planının ve birleştirilmiş planın nasıl görünebileceğini gösteren örnekler aşağıda verilmiştir:

* Bağımsız plan

  ![Bağımsız navigasyon planı](./media/connectors-create-api-3270/standalone-plan.png)

* Planı bağla

  ![Planı bağla](./media/connectors-create-api-3270/connect-plan.png)

* Planı kesme

  ![Planı kesme](./media/connectors-create-api-3270/disconnect-plan.png)

* Birleşik plan

  ![Birleşik plan](./media/connectors-create-api-3270/combined-plan.png)

#### <a name="example-identify-repeated-screens"></a>Örnek: Yinelenen ekranları tanımlama

Bağlayıcının ekranlarda gezinmesi ve farklılaşması için, genellikle yakalanan ekranlarda tanımlayıcı olarak kullanabileceğiniz bir ekranda benzersiz metin bulursunuz. Yinelenen ekranlar için daha fazla tanımlama yöntemi gerekebilir. Örnek planda, benzer ekranlar alabileceğiniz bir çatal vardır. Bir ekran hesap bakiyesini döndürürken, diğer ekran bir hata iletisi döndürür.

Tasarım aracı, Ekran Tanıma düzenleyicisini kullanarak tanıma öznitelikleri (örneğin Hesap Bakiyesi Al) adlı bir ekran başlığı eklemenize olanak tanır. Benzer ekranlarda, başka özniteliklere ihtiyacınız vardır. Çalışma zamanında, bağlayıcı dalı ve çatalı belirlemek için bu öznitelikleri kullanır.

* Hesap bakiyesi olan ekran olan geçerli girişi döndüren dalda, "boş değil" koşulu olan bir alan ekleyebilirsiniz.

* Hatayla döndüren dalda, "boş" koşulu olan bir alan ekleyebilirsiniz.

<a name="define-method"></a>

## <a name="define-methods"></a>Yöntemleri tanımlama

Bu modda, gezinti planınla ilişkili bir yöntem tanımlarsınız. Her yöntem parametresi için dize, tamsayı, tarih veya saat gibi veri türünü belirtirsiniz. İşinizi bitirdiğinizde, metodunuzu canlı ana bilgisayarda sınayabilir ve yöntemin beklendiği gibi çalıştığını onaylayabilirsiniz. Daha sonra meta veri dosyasını veya IBM 3270 bağlayıcısı için bir eylem oluşturmak ve çalıştırmak için kullanılacak yöntem tanımlarına sahip Olan Ana Bilgisayar Tümleştirme Tasarımcısı XML (HIDX) dosyasını oluşturursunuz.

1. 3270 Tasarım Aracı'nın araç çubuğunda Yöntemler moduna girebilmeniz için **Yöntemler'i** seçin. 

1. **Gezinti** bölmesinde, istediğiniz giriş alanlarına sahip ekranı seçin.

1. Yönteminizin ilk giriş parametresini eklemek için aşağıdaki adımları izleyin:

   1. **Yakalama** bölmesinde, 3270 emülatör ekranında, ilk giriş olarak istediğiniz yalnızca alan içindeki metni değil, tüm alanı seçin.

      > [!TIP]
      > Tüm alanları görüntülemek ve **Görünüm** menüsünde tüm alanı seçtiğinizden emin olmak için **Tüm Alanlar'ı**seçin.

   1. Tasarım aracının araç çubuğunda **Giriş Alanı'nı**seçin. 

   Daha fazla giriş parametresi eklemek için, her parametre için önceki adımları yineleyin.

1. Yönteminizin ilk çıktı parametresini eklemek için aşağıdaki adımları izleyin:

   1. **Yakalama** bölmesinde, 3270 emülatör ekranında, yalnızca alan içindeki metni değil, ilk çıktı olarak istediğiniz tüm alanı seçin.

      > [!TIP]
      > Tüm alanları görüntülemek ve **Görünüm** menüsünde tüm alanı seçtiğinizden emin olmak için **Tüm Alanlar'ı**seçin.

   1. Tasarım aracının araç çubuğunda **Çıktı Alanı'nı**seçin.

   Daha fazla çıktı parametresi eklemek için, her parametre için önceki adımları yineleyin.

1. Yönteminizin tüm parametrelerini ekledikten sonra, her parametre için aşağıdaki özellikleri tanımlayın:

   | Özellik adı | Olası değerler | 
   |---------------|-----------------|
   | **Veri Türü** | Bayt, Tarih Saati, Ondalık, İnt, Uzun, Kısa, String |
   | **Alan Doldurma Tekniği** | Parametreler gerekirse boşluklarla doldurarak bu dolgu türlerini destekler: <p><p>- **Tür**: Karakterleri alana sırayla girin. <p>- **Dolgu**: Gerekirse boşluklarla doldurarak alanın içeriğini karakterlerle değiştirin. <p>- **EraseEofType**: Alanı temizleyin ve karakterleri alana sırayla girin. |
   | **Biçim Dizesi** | Bazı parametre veri türleri, 3270 bağlayıcısına metin ekrandan nasıl dönüştürüleceklerini bildiren bir biçim dizesi kullanır: <p><p>- **DateTime**: DateTime biçim dizesi [.NET özel tarih ve saat biçimi dizelerini](https://docs.microsoft.com/dotnet/standard/base-types/custom-date-and-time-format-strings)izler. Örneğin, tarih `06/30/2019` biçim dizesini `MM/dd/yyyy`kullanır. <p>- **Ondalık**: Ondalık biçim dizesi [COBOL Resim yan tümcesini](https://www.ibm.com/support/knowledgecenter/SS6SG3_5.2.0/com.ibm.cobol52.ent.doc/PGandLR/ref/rlddepic.html)kullanır. Örneğin, sayı `100.35` biçim dizesini `999V99`kullanır. |
   |||

## <a name="save-and-view-metadata"></a>Meta verileri kaydetme ve görüntüleme

Yönteminizi tanımladıktan sonra, ancak yönteminizi test etmeden önce, şimdiye kadar tanımladığınız tüm bilgileri bir RAP (.rap) dosyasına kaydedin.
Herhangi bir modda istediğiniz zaman bu RAP dosyasına kaydedebilirsiniz. Tasarım aracı ayrıca, bu konumdaki tasarım aracının kurulum klasörüne göz atarak ve "WoodgroveBank.rap" dosyasını açarak açabileceğiniz ve gözden geçirebileceğiniz örnek bir RAP dosyası içerir:

`..\Program Files\Microsoft Host Integration Server - 3270 Design Tool\SDK\WoodgroveBank.rap`

Ancak, örnek RAP dosyasında yapılan değişiklikleri kaydetmeyi veya dosya tasarım aracının yükleme klasöründe kalırken örnek RAP dosyasından bir HIDX dosyası oluşturmayı denerseniz, bir "erişim reddedildi" hatası alabilirsiniz. Varsayılan olarak, tasarım aracı yüksek izinler olmadan Program Dosyaları klasörünüze yüklenir. Bir hata alırsanız, şu çözümlerden birini deneyin:

* Örnek dosyayı farklı bir konuma kopyalayın.
* Tasarım aracını yönetici olarak çalıştırın.
* Kendinizi SDK klasörünsahibi yapın.

## <a name="test-your-method"></a>Yönteminizi test edin

1. Yönteminizi canlı ana bilgisayara karşı çalıştırmak için, Yöntemler modundayken F5 tuşuna veya tasarım aracının araç çubuğundan **Çalıştır'ı**seçin.

   > [!TIP]
   > Modları istediğiniz zaman değiştirebilirsiniz. **Dosya** menüsünde **Mod'u**seçin ve ardından istediğiniz modu seçin.

1. Parametrelerinizin değerlerini girin ve **Tamam'ı**seçin.

1. Bir sonraki ekrana devam etmek için **İleri'yi**seçin.

1. Bitirdikten sonra, çıktı parametre değerlerinizi gösteren **Bitti'yi**seçin.

<a name="add-metadata-integration-account"></a>

## <a name="generate-and-upload-hidx-file"></a>HIDX dosyaoluşturma ve yükleme

Hazır olduğunuzda, entegrasyon hesabınıza yükleyebilmeniz için HIDX dosyasını oluşturun. 3270 Tasarım Aracı, HIDX dosyasını RAP dosyanızı kaydettiğiniz yeni bir alt klasörde oluşturur.

1. 3270 Tasarım Aracı'nın araç çubuğunda **Kod Oluştur'u**seçin.

1. RAP dosyanızı içeren klasöre gidin ve HIDX dosyanızı oluşturduktan sonra aracın oluşturduğu alt klasörü açın. Aracın HIDX dosyasını oluşturduğunu doğrulayın.

1. [Azure portalında](https://portal.azure.com)oturum açın ve entegrasyon hesabınızı bulun.

1. [Harita eklemek için aşağıdaki benzer adımları izleyerek](../logic-apps/logic-apps-enterprise-integration-liquid-transform.md)HIDX dosyanızı entegrasyon hesabınıza harita olarak ekleyin, ancak harita türünü seçtiğinizde **HIDX'i**seçin.

Daha sonra bu konuyla, mantık uygulamanıza ilk kez bir IBM 3270 eylemi eklediğinizde, entegrasyon hesabınız ve ana bilgisayar sunucunuz gibi bağlantı bilgileri sağlayarak mantık uygulamanız ile ana bilgisayar sunucusu arasında bir bağlantı oluşturmanız istenir . Bağlantıyı oluşturduktan sonra, önceden eklenen HIDX dosyanızı, çalışacak yöntemi ve kullanılacak parametreleri seçebilirsiniz.

Tüm bu adımları tamamladığınızda, ibm ana bilgisayarınıza bağlanmak, uygulamanız için ekranları yönlendirmek, verileri girmek, sonuçları döndürmek ve benzeri için mantık uygulamanızda oluşturduğunuz eylemi kullanabilirsiniz. Diğer uygulamalar, hizmetler ve sistemlerle tümleştirmek için mantık uygulamanıza başka eylemler eklemeye devam edebilirsiniz.

<a name="run-action"></a>

## <a name="run-ibm-3270-actions"></a>IBM 3270 eylemlerini çalıştırın

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. [Azure portalında](https://portal.azure.com)oturum açın ve mantık uygulamanızı zaten açık değilse Mantık Uygulama Tasarımcısı'nda açın.

1. Eylem eklemek istediğiniz son adımAltında, **Yeni adım'ı**seçin ve **eylem ekle'yi**seçin. 

1. Arama kutusunun altında **Enterprise'ı**seçin. Arama kutusuna filtreniz olarak "3270" girin. Eylemler listesinden şu eylemi seçin: **TN3270 bağlantısı üzerinden bir ana bilgisayar programı çalıştırın**

   ![3270 aksiyon seçin](./media/connectors-create-api-3270/select-3270-action.png)

   Adımlar arasında eylem eklemek için işaretçinizin üzerine adımların arasında ilerleyin. 
   Görünen artı işaretini (**+**) seçin ve ardından eylem **ekle'yi**seçin.

1. Henüz bağlantı yoksa, bağlantınız için gerekli bilgileri sağlayın ve **Oluştur'u**seçin.

   | Özellik | Gerekli | Değer | Açıklama |
   |----------|----------|-------|-------------|
   | **Bağlantı Adı** | Evet | <*bağlantı adı*> | Bağlantınızın adı |
   | **Entegrasyon Hesap Kimliği** | Evet | <*tümleştirme-hesap-adı*> | Tümleştirme hesabınızın adı |
   | **Entegrasyon Hesabı SAS URL** | Evet | <*entegrasyon-hesap-SAS-URL*> | Tümleştirme hesabınızın Azure portalındaki entegrasyon hesabınızın ayarlarından oluşturabileceğiniz Paylaşılan Erişim İmza (SAS) URL'si. <p>1. Entegrasyon hesabı menüsünde **Settings**Ayarlar **altında, Geri Arama URL'sini**seçin. <br>2. Sağ **bölmede, Oluşturulan Geri Arama URL** değerini kopyalayın. |
   | **Sunucu** | Evet | <*TN3270-sunucu adı*> | TN3270 hizmetinizin sunucu adı |
   | **Bağlantı noktası** | Hayır | <*TN3270-sunucu-bağlantı noktası*> | TN3270 sunucunuz tarafından kullanılan bağlantı noktası. Boş bırakılırsa, bağlayıcı `23` varsayılan değer olarak kullanır. |
   | **Cihaz Türü** | Hayır | <*IBM terminal modeli*> | IBM terminalinin taklit ediletilecek model adı veya numarası. Boş bırakılırsa, bağlayıcı varsayılan değerleri kullanır. |
   | **Kod Sayfası** | Hayır | <*kod-sayfa numarası*> | Ana bilgisayar için kod sayfa numarası. Boş bırakılırsa, bağlayıcı `37` varsayılan değer olarak kullanır. |
   | **Mantıksal Birim Adı** | Hayır | <*mantıksal-birim adı*> | Ana bilgisayardan istenecek belirli mantıksal birim adı |
   | **SSL etkinleştirin?** | Hayır | Aveya kapalı | TLS şifrelemeyi açın veya kapatın. |
   | **Ev sahibi ssl sertifikasını doğrulamak mı?** | Hayır | Aveya kapalı | Sunucunun sertifikası için doğrulamayı açın veya kapatın. |
   ||||

   Örnek:

   ![Bağlantı özellikleri](./media/connectors-create-api-3270/connection-properties.png)

1. Eylem için gerekli bilgileri sağlayın:

   | Özellik | Gerekli | Değer | Açıklama |
   |----------|----------|-------|-------------|
   | **Hidx Adı** | Evet | <*HIDX-dosya adı*> | Kullanmak istediğiniz 3270 HIDX dosyasını seçin. |
   | **Yöntem Adı** | Evet | <*yöntem adı*> | Kullanmak istediğiniz HIDX dosyasındaki yöntemi seçin. Bir yöntem seçtikten sonra, **yeni parametre** ekle listesi görüntülenir, böylece bu yöntemle kullanılacak parametreleri seçebilirsiniz. |
   ||||

   Örnek:

   **HIDX dosyasını seçin**

   ![HIDX dosyayı seçin](./media/connectors-create-api-3270/select-hidx-file.png)

   **Yöntemi seçin**

   ![Yöntem seçin](./media/connectors-create-api-3270/select-method.png)

   **Parametreleri seçin**

   ![Parametreleri seçme](./media/connectors-create-api-3270/add-parameters.png)

1. İşinizi bitirdiğinizde, mantık uygulamanızı kaydedin ve çalıştırın.

   Mantık uygulamanız çalışma bittikten sonra, çalıştırmadan sonraki adımlar görüntülenir. 
   Başarılı adımlar onay işaretlerini gösterirken, başarısız adımlar "X" harfini gösterir.

1. Her adım için girdi ve çıktıları gözden geçirmek için bu adımı genişletin.

1. Çıktıları gözden geçirmek için **Ham çıktıları**gör'üni seçin.

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Tetikleyiciler, eylemler ve konektörün Swagger dosyasında açıklandığı gibi sınırlar gibi bu bağlayıcı hakkında daha fazla teknik ayrıntı için [bağlayıcının başvuru sayfasına](https://docs.microsoft.com/connectors/si3270/)bakın.

> [!NOTE]
> [Bir entegrasyon hizmeti ortamındaki (İmKB)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md)mantık uygulamaları için, bu bağlayıcının İmKB etiketli sürümü bunun yerine [İmKB ileti sınırlarını](../logic-apps/logic-apps-limits-and-config.md#message-size-limits) kullanır.

## <a name="next-steps"></a>Sonraki adımlar

* Diğer [Logic Apps bağlayıcıları](../connectors/apis-list.md) hakkında bilgi edinin
