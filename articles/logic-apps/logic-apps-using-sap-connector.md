---
title: SAP sistemlerine bağlanın
description: Azure Logic Apps ile iş akışlarını otomatikleştirerek SAP kaynaklarına erişin ve yönetin
services: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: estfan, logicappspm
ms.topic: article
ms.date: 08/30/2019
tags: connectors
ms.openlocfilehash: 39ab222f64d964e95b16e043c9cdeccd8170ace3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77651024"
---
# <a name="connect-to-sap-systems-from-azure-logic-apps"></a>Azure Logic Apps’ten SAP sistemlerine bağlanma

> [!IMPORTANT]
> Önceki SAP Application Server ve SAP Message Server bağlayıcıları 29 Şubat 2020 tarihinde amortismana alınır. Geçerli SAP bağlayıcısı, bağlantı türünü değiştirmek zorunda kalmamanız, önceki bağlayıcılarla tam uyumlu olması, birçok ek özellik sağlaması ve SAP .Net bağlayıcı kitaplığını kullanmaya devam etmesi için bu önceki SAP bağlayıcılarını birleştirir ( SAP NCo).
>
> Eski bağlayıcıları kullanan mantık uygulamaları için lütfen amortisman tarihinden önce [en son konektöre geçirin.](#migrate) Aksi takdirde, bu mantık uygulamaları yürütme hatalarıyla karşılaşır ve SAP sisteminize ileti gönderemez.

Bu makalede, SAP bağlayıcısını kullanarak şirket içi SAP kaynaklarına bir mantık uygulaması nın içinden nasıl erişebileceğiniz gösterilmektedir. Konektör, SAP'nin R/3 ve ECC sistemleri gibi klasik sürümleriyle çalışır. Konektör, ister şirket içinde ister bulutta barındırılan s/4 HANA gibi SAP'nin yeni HANA tabanlı SAP sistemleriyle tümleşmeye de olanak tanır. SAP bağlayıcısı, SAP NetWeaver tabanlı sistemlere Ara Belge (IDoc), İş Uygulama Programlama Arabirimi (BAPI) veya Uzaktan İşlev Çağrısı (RFC) aracılığıyla ileti veya veri tümleştirmesini destekler.

SAP bağlayıcısı [SAP .NET Bağlayıcısı (NCo) kitaplığını](https://support.sap.com/en/product/connectors/msnet.html) kullanır ve aşağıdaki işlemleri sağlar:

* **SAP'ye mesaj gönder**: IDoc'u tRFC üzerinden gönderin, RFC üzerinden BAPI işlevlerini arayın veya SAP sistemlerinde RFC/tRFC'yi arayın.
* **SAP'den bir ileti alındığı zaman**: TRFC üzerinden IDoc alın, tRFC üzerinden BAPI işlevlerini arayın veya SAP sistemlerinde RFC/tRFC'yi arayın.
* **Şema oluşturma**: IDoc, BAPI veya RFC için SAP yapıları için şemalar oluşturun.

Bu işlemler için SAP bağlayıcısı, kullanıcı adları ve parolalar aracılığıyla temel kimlik doğrulamasını destekler. Bağlayıcı ayrıca [Secure Network Communications (SNC)](https://help.sap.com/doc/saphelp_nw70/7.0.31/e6/56f466e99a11d1a5b00000e835363f/content.htm?no_cache=true)destekler. SNC, SAP NetWeaver tek oturum açma (SSO) veya harici bir güvenlik ürünü tarafından sağlanan ek güvenlik yetenekleri için kullanılabilir.

SAP konektörü şirket içi [veri ağ geçidi](../logic-apps/logic-apps-gateway-connection.md)aracılığıyla şirket içi SAP sistemleriyle bütünleşir. Örneğin, bir ileti bir mantık uygulamasından SAP sistemine gönderildiğinde, veri ağ geçidi bir RFC istemcisi gibi davranır ve mantık uygulamasından alınan istekleri SAP'ye iletir. Aynı şekilde, alma senaryolarında veri ağ geçidi SAP'den istek alan ve bunları mantık uygulamasına ileten bir RFC sunucusu gibi davranır.

Bu makalede, daha önce açıklanan tümleştirme senaryolarını kapsayan SAP ile tümleğe sahip örnek mantık uygulamalarının nasıl oluşturulacağı gösterilmektedir. Eski SAP bağlayıcılarını kullanan mantık uygulamaları için bu makalede, mantık uygulamalarınızın en son SAP bağlayıcısına nasıl geçirilen bir uygulama gösterilmektedir.

<a name="pre-reqs"></a>

## <a name="prerequisites"></a>Ön koşullar

Bu makaleyi takip etmek için aşağıdaki öğelere ihtiyacınız vardır:

* Azure aboneliği. Henüz bir Azure aboneliğiniz yoksa, [ücretsiz bir Azure hesabına kaydolun.](https://azure.microsoft.com/free/)

* SAP sisteminize erişmek istediğiniz yerden gelen mantık uygulaması ve mantık uygulamanızın iş akışını başlatan bir tetikleyici. Mantık uygulamalarında yeniyseniz, Azure [Mantık Uygulamaları nedir?](../logic-apps/logic-apps-overview.md) ve [Quickstart: İlk mantık uygulamanızı oluşturun.](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* [SAP uygulama sunucunuz](https://wiki.scn.sap.com/wiki/display/ABAP/ABAP+Application+Server) veya [SAP ileti sunucunuz.](https://help.sap.com/saphelp_nw70/helpdata/en/40/c235c15ab7468bb31599cc759179ef/frameset.htm)

* Şirket içinde en son [veri ağ geçidini](https://www.microsoft.com/download/details.aspx?id=53127) şirket içi bilgisayara indirin ve yükleyin. Devam etmeden önce ağ geçidinizi Azure portalında ayarladığınızdan emin olun. Ağ geçidi, şirket içi verilere ve kaynaklara güvenli bir şekilde erişmenize yardımcı olur. Daha fazla bilgi için azure [mantık uygulamaları için şirket içi veri ağ geçidi yükleyin'e](../logic-apps/logic-apps-gateway-install.md)bakın.

* SNC'yi SSO ile kullanıyorsanız, ağ geçidinin SAP kullanıcısına karşı eşlenen bir kullanıcı olarak çalıştığını unutmayın. Varsayılan hesabı değiştirmek için **Hesabı Değiştir'i**seçin ve kullanıcı kimlik bilgilerini girin.

  ![Ağ geçidi hesabını değiştirme](./media/logic-apps-using-sap-connector/gateway-account.png)

* Harici bir güvenlik ürünüyle SNC'yi etkinleştirdiyseniz, SNC kitaplığını veya dosyaları ağ geçidinin yüklü olduğu makinede kopyalayın. SNC ürünlerinin bazı örnekleri [sapseculib,](https://help.sap.com/saphelp_nw74/helpdata/en/7a/0755dc6ef84f76890a77ad6eb13b13/frameset.htm)Kerberos ve NTLM içerir.

* [Microsoft .NET 3.0.22.0 için microsoft .net 3.0.22.0 için ,NET Framework 4.0 - Windows 64-bit (x64) ile derlenen](https://softwaredownloads.sap.com/file/0020000001000932019)en son SAP istemci kitaplığını şirket içi veri ağ geçidiyle aynı bilgisayara indirin ve kurun. Bu nedenle bu sürümü veya daha sonra yükleyin:

  * Aynı anda birden fazla IDoc iletisi gönderildiğinde önceki SAP NCo sürümleri kilitlenebilir. Bu durum, SAP hedefine gönderilen sonraki tüm iletileri engeller ve bu da iletilerin zaman almasına neden olur.
  
  * Şirket içi veri ağ geçidi yalnızca 64 bit sistemlerde çalışır. Aksi takdirde, veri ağ geçidi ana bilgisayarı hizmeti 32 bit derlemeleri desteklemediği için "kötü görüntü" hatası alırsınız.
  
  * Hem veri ağ geçidi ana bilgisayarı hizmeti hem de Microsoft SAP Bağdaştırıcısı .NET Framework 4.5 kullanır. .NET Framework 4.0 için SAP NCo,.NET çalışma süresi 4.0 ile 4.7.1'i kullanan işlemlerle çalışır. .NET Framework 2.0 için SAP NCo ,.NET çalışma zamanı 2.0 ile 3.5 kullanan işlemlerle çalışır, ancak artık en son şirket içi veri ağ geçidiyle çalışmaz.

* Örnek bir IDoc dosyası gibi SAP sunucunuza gönderebileceğiniz ileti içeriği XML biçiminde olmalı ve kullanmak istediğiniz SAP eylemi için ad alanını içermelidir.

<a name="migrate"></a>

## <a name="migrate-to-current-connector"></a>Geçerli bağlayıcıya geçirin

1. Bunu daha önce yapmadıysanız, [şirket içi veri ağ geçidinizi](https://www.microsoft.com/download/details.aspx?id=53127) güncelleştirin, böylece en son sürüme sahip olabilirsiniz. Daha fazla bilgi için azure [mantık uygulamaları için şirket içi veri ağ geçidi yükleyin'e](../logic-apps/logic-apps-gateway-install.md)bakın.

1. Eski SAP bağlayıcısını kullanan mantık uygulamasında **SAP'ye Gönder eylemini** silin.

1. En son SAP **bağlayıcısından, SAP eylemine ileti gönder'i** ekleyin. Bu eylemi kullanmadan önce SAP sisteminize olan bağlantıyı yeniden oluşturun.

1. Işiniz bittiğinde, mantık uygulamanızı kaydedin.

<a name="add-trigger"></a>

## <a name="send-message-to-sap"></a>SAP'ye ileti gönderme

Bu örnekte, http isteğiyle tetikleyebileceğiniz bir mantık uygulaması kullanır. Mantık uygulaması bir SAP sunucusuna bir IDoc gönderir ve mantık uygulamasını çağıran isteksahibine bir yanıt verir.

### <a name="add-an-http-request-trigger"></a>HTTP İstek tetikleyicisi ekleme

Azure Logic Apps'ta, her mantık uygulaması, belirli bir olay olduğunda veya belirli bir koşul yerine getirildiğinde ateş eden bir [tetikleyiciyle](../logic-apps/logic-apps-overview.md#logic-app-concepts)başlamalıdır. Tetikleyici her ateşleninher yerinde, Logic Apps motoru bir mantık uygulaması örneği oluşturur ve uygulamanızın iş akışını çalıştırmaya başlar.

Bu örnekte, http *post isteklerini* mantık uygulamanıza gönderebilmeniz için Azure'da bitiş noktası olan bir mantık uygulaması oluşturursunuz. Mantık uygulamanız bu HTTP isteklerini aldığında, tetikleyici iş akışınızdaki bir sonraki adımı tetikler ve çalıştırAr.

1. Azure [portalında,](https://portal.azure.com)Logic App Designer'ı açan boş bir mantık uygulaması oluşturun.

1. Arama kutusuna filtreniz olarak girin. `http request` **Tetikleyiciler** listesinden, **bir HTTP isteği ne zaman alındığını**seçin.

   ![HTTP İstek tetikleyicisi ekle](./media/logic-apps-using-sap-connector/add-http-trigger-logic-app.png)

1. Artık mantık uygulamanız için bir uç nokta URL'si oluşturabilmeniz için mantık uygulamanızı kaydedin. Tasarımcı araç çubuğunda **Kaydet'i**seçin.

   Uç nokta URL artık tetikleyicinizde görünür, örneğin:

   ![Uç nokta için URL oluşturma](./media/logic-apps-using-sap-connector/generate-http-endpoint-url.png)

<a name="add-action"></a>

### <a name="add-an-sap-action"></a>SAP eylemi ekleme

Azure Logic Apps'ta [eylem,](../logic-apps/logic-apps-overview.md#logic-app-concepts) iş akışınızda bir tetikleyici veya başka bir eylemi izleyen bir adımdır. Mantık uygulamanıza henüz bir tetikleyici eklemediyseniz ve bu örneği takip etmek istiyorsanız, [bu bölümde açıklanan tetikleyiciyi ekleyin.](#add-trigger)

1. Mantık Uygulama Tasarımcısı'nda, tetikleyici altında **Yeni adım'ı**seçin.

   ![Mantık uygulamasına yeni adım ekleme](./media/logic-apps-using-sap-connector/add-sap-action-logic-app.png)

1. Arama kutusuna filtreniz olarak girin. `sap` **Eylemler** listesinden **SAP'ye ileti gönder'i**seçin.
  
   !["SAP'ye ileti gönder" eylemini seçin](media/logic-apps-using-sap-connector/select-sap-send-action.png)

   Veya, **Kurumsal** sekmesini ve SAP eylemini seçebilirsiniz.

   ![Kurumsal sekmesinden "SAP'ye ileti gönder" eylemini seçin](media/logic-apps-using-sap-connector/select-sap-send-action-ent-tab.png)

1. Bağlantınız zaten varsa, SAP eyleminizi ayarlamak için bir sonraki adımla devam edin. Ancak, bağlantı ayrıntıları için sizden istenirse, bilgileri şirket içi SAP sunucunuza bağlantı oluşturabilmeniz için sağlayın.

   1. Bağlantı için bir ad sağlayın.

   1. Veri **Ağ Geçidi** bölümünde, **Abonelik**altında, ağ geçidi yüklemeniz için azure portalında oluşturduğunuz ağ geçidi kaynağı için azure aboneliğini seçin. 
   
   1. **Bağlantı Ağ Geçidi**altında, ağ geçidi kaynağınızı seçin.

   1. Bağlantı hakkında bilgi sağlamaya devam edin. **Logon Type** özelliği için, özelliğin **Application Server** veya **Group**olarak ayarlanıp ayarlmadığına bağlı olarak adımı izleyin:
   
      * **Application Server**için, genellikle isteğe bağlı görünen bu özellikler gereklidir:

        ![SAP uygulama sunucusu bağlantısı oluşturma](media/logic-apps-using-sap-connector/create-SAP-application-server-connection.png)

      * **Grup**için, genellikle isteğe bağlı görünen bu özellikler gereklidir:

        ![SAP ileti sunucusu bağlantısı oluşturma](media/logic-apps-using-sap-connector/create-SAP-message-server-connection.png)  

      Varsayılan olarak, şemaya karşı XML doğrulaması gerçekleştirerek geçersiz değerleri denetlemek için güçlü yazı yazmak kullanılır. Bu davranış, sorunları daha önce algılamanıza yardımcı olabilir. **Güvenli Yazma** seçeneği geriye dönük uyumluluk için kullanılabilir ve yalnızca dize uzunluğunu denetler. Güvenli Yazma [seçeneği](#safe-typing)hakkında daha fazla bilgi edinin.

   1. Bitirdikten sonra **Oluştur'u**seçin.

      Logic Apps, bağlantının düzgün çalıştığından emin olmak için bağlantınızı kurar ve sınar.

1. Şimdi SAP sunucunuzdan bir eylem bulun ve seçin.

   1. SAP **Eylem** kutusunda klasör simgesini seçin. Dosya listesinden, kullanmak istediğiniz SAP iletisini bulun ve seçin. Listede gezinmek için okları kullanın.

      Bu örnek, **Siparişler** türüne sahip bir IDoc seçer.

      ![IDoc eylemini bulma ve seçme](./media/logic-apps-using-sap-connector/SAP-app-server-find-action.png)

      İstediğiniz eylemi bulamıyorsanız, örneğin bir yolu el ile girebilirsiniz:

      ![IDoc eylemine yol el ile sağlayın](./media/logic-apps-using-sap-connector/SAP-app-server-manually-enter-action.png)

      > [!TIP]
      > İfade düzenleyicisi aracılığıyla **SAP Eylemi** değerini sağlayın. Bu şekilde, farklı ileti türleri için aynı eylemi kullanabilirsiniz.

      IDoc işlemleri hakkında daha fazla bilgi için, [IDOC işlemleri için Mesaj şemalarına](https://docs.microsoft.com/biztalk/adapters-and-accelerators/adapter-sap/message-schemas-for-idoc-operations)bakın.

   1. Dinamik içerik listesinin görünmesi için **Giriş İletisi** kutusunun içini tıklatın. Bu listeden, **bir HTTP isteği aldığında,** **Gövde** alanını seçin.

      Bu adım, HTTP İstek tetikleyicinizdeki gövde içeriğini içerir ve bu çıktıyı SAP sunucunuza gönderir.

      ![Tetikleyiciden "Gövde" özelliğini seçin](./media/logic-apps-using-sap-connector/SAP-app-server-action-select-body.png)

      İşinizi bitirdiğinizde, SAP eyleminiz şu örneğe benzer:

      ![SAP eylemini tamamlama](./media/logic-apps-using-sap-connector/SAP-app-server-complete-action.png)

1. Mantıksal uygulamanızı kaydedin. Tasarımcı araç çubuğunda **Kaydet'i**seçin.

<a name="add-response"></a>

### <a name="add-an-http-response-action"></a>HTTP yanıt eylemi ekleme

Şimdi mantık uygulamanızın iş akışına bir yanıt eylemi ekleyin ve SAP eyleminden çıktıyı ekleyin. Bu şekilde, mantık uygulamanız SAP sunucunuzdaki sonuçları orijinal istekte bulunduracak şekilde döndürür.

1. Mantık Uygulama Tasarımcısı'nda, SAP eylemi altında **Yeni adımı**seçin.

1. Arama kutusuna filtreniz olarak girin. `response` **Eylemler** listesinden **Yanıt'ı**seçin.

1. Dinamik içerik listesinin görünmesi için **Gövde** kutusunun içini tıklatın. Bu listeden **SAP'ye ileti gönder**altında **Gövde** alanını seçin.

   ![SAP eylemini tamamla](./media/logic-apps-using-sap-connector/select-sap-body-for-response-action.png)

1. Mantıksal uygulamanızı kaydedin.

### <a name="test-your-logic-app"></a>Mantık uygulamanızı test edin

1. Mantık uygulamanız zaten etkin değilse, mantık uygulaması menüsünde **Genel Bakış'ı**seçin. Araç çubuğunda **Etkinleştir'i**seçin.

1. Tasarımcı araç çubuğunda **Çalıştır'ı**seçin. Bu adım mantık uygulamanızı el ile başlatır.

1. HTTP İstek tetikleyicinizdeki URL'ye bir HTTP POST isteği göndererek mantık uygulamanızı tetikler.
İleti içeriğinizi isteğinizle birlikte ekleyin. İstek göndermek için [Postacı](https://www.getpostman.com/apps)gibi bir araç kullanabilirsiniz.

   Bu makale için istek, XML biçiminde olması gereken ve örneğin kullanmakta olduğunuz SAP eylemi için ad alanını içeren bir IDoc dosyası gönderir:

   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <Send xmlns="http://Microsoft.LobServices.Sap/2007/03/Idoc/2/ORDERS05//720/Send">
      <idocData>
         <...>
      </idocData>
   </Send>
   ```

1. HTTP isteğinizi gönderdikten sonra, mantık uygulamanızdan yanıt bekleyin.

   > [!NOTE]
   > Yanıt için gereken tüm adımlar [istek zaman sınırı](./logic-apps-limits-and-config.md)içinde bitmezse, mantık uygulamanız zaman alabilir. Bu durum gerçekleşirse, istekler engellenebilir. Sorunları tanılamanıza yardımcı olmak için, [mantık uygulamalarınızı](../logic-apps/monitor-logic-apps.md)nasıl kontrol edip izleyebilirsiniz öğrenin.

Artık SAP sunucunuzla iletişim kurabilen bir mantık uygulaması oluşturdunuz. Artık mantık uygulamanız için bir SAP bağlantısı kurduğunuza göre, BAPI ve RFC gibi diğer kullanılabilir SAP eylemlerini keşfedebilirsiniz.

<a name="receive-from-sap"></a>

## <a name="receive-message-from-sap"></a>SAP'den ileti alma

Bu örnek, uygulama sap sisteminden bir ileti aldığında tetikleyen bir mantık uygulaması kullanır.

### <a name="add-an-sap-trigger"></a>SAP tetikleyicisi ekleme

1. Azure portalında, Logic App Designer'ı açan boş bir mantık uygulaması oluşturun.

1. Arama kutusuna filtreniz olarak girin. `sap` **Tetikleyiciler** listesinden **SAP'den ileti ne zaman alındığını**seçin.

   ![SAP tetikleyicisi ekleme](./media/logic-apps-using-sap-connector/add-sap-trigger-logic-app.png)

   Veya, **Kurumsal** sekmesini ve ardından tetikleyiciyi seçebilirsiniz:

   ![Kuruluş sekmesinden SAP tetikleyicisi ekleme](./media/logic-apps-using-sap-connector/add-sap-trigger-ent-tab.png)

1. Bağlantınız zaten varsa, SAP eyleminizi ayarlamak için bir sonraki adımla devam edin. Ancak, bağlantı ayrıntıları için sizden istenirse, bilgileri şirket içi SAP sunucunuza bağlantı oluşturabilmeniz için sağlayın.

   1. Bağlantı için bir ad sağlayın.

   1. Veri **Ağ Geçidi** bölümünde, **Abonelik**altında, ağ geçidi yüklemeniz için azure portalında oluşturduğunuz ağ geçidi kaynağı için azure aboneliğini seçin. 

   1. **Bağlantı Ağ Geçidi**altında, ağ geçidi kaynağınızı seçin.

   1. Bağlantı hakkında bilgi sağlamaya devam edin. **Logon Type** özelliği için, özelliğin **Application Server** veya **Group**olarak ayarlanıp ayarlmadığına bağlı olarak adımı izleyin:

      * **Application Server**için, genellikle isteğe bağlı görünen bu özellikler gereklidir:

        ![SAP uygulama sunucusu bağlantısı oluşturma](media/logic-apps-using-sap-connector/create-SAP-application-server-connection.png)

      * **Grup**için, genellikle isteğe bağlı görünen bu özellikler gereklidir:

        ![SAP ileti sunucusu bağlantısı oluşturma](media/logic-apps-using-sap-connector/create-SAP-message-server-connection.png)

      Varsayılan olarak, şemaya karşı XML doğrulaması gerçekleştirerek geçersiz değerleri denetlemek için güçlü yazı yazmak kullanılır. Bu davranış, sorunları daha önce algılamanıza yardımcı olabilir. **Güvenli Yazma** seçeneği geriye dönük uyumluluk için kullanılabilir ve yalnızca dize uzunluğunu denetler. Güvenli Yazma [seçeneği](#safe-typing)hakkında daha fazla bilgi edinin.

   1. Bitirdikten sonra **Oluştur'u**seçin.

      Logic Apps, bağlantının düzgün çalıştığından emin olmak için bağlantınızı kurar ve sınar.

1. SAP sistem yapılandırmanıza dayalı olarak gerekli parametreleri sağlayın.

   İsteğe bağlı olarak bir veya daha fazla SAP eylemi sağlayabilirsiniz. Bu eylem listesi, tetikleyicinin sap sunucunuzdan aldığı iletileri veri ağ geçidi aracılığıyla belirtir. Boş bir liste, tetikleyicinin tüm iletileri aldığını belirtir. Listede birden fazla ileti varsa, tetikleyici yalnızca listede belirtilen iletileri alır. SAP sunucunuzdan gönderilen diğer iletiler ağ geçidi tarafından reddedilir.

   Dosya seçiciden bir SAP eylemi seçebilirsiniz:

   ![Mantık uygulamasına SAP eylemi ekleme](media/logic-apps-using-sap-connector/select-SAP-action-trigger.png)  

   Veya bir eylemi el ile belirtebilirsiniz:

   ![SAP eylemini el ile girin](media/logic-apps-using-sap-connector/manual-enter-SAP-action-trigger.png)

   Tetikleyiciyi birden fazla ileti alacak şekilde ayarladığınızda eylemin nasıl göründüğünü gösteren bir örnek aşağıda verilmiştir.

   ![Birden çok ileti alan tetikleyici örneği](media/logic-apps-using-sap-connector/example-trigger.png)

   SAP eylemi hakkında daha fazla bilgi [için, IDOC işlemleri için Mesaj şemalarına](https://docs.microsoft.com/biztalk/adapters-and-accelerators/adapter-sap/message-schemas-for-idoc-operations) bakın

1. Artık sap sisteminizden mesaj almaya başlayabilmek için mantık uygulamanızı kaydedin. Tasarımcı araç çubuğunda **Kaydet'i**seçin.

Mantık uygulamanız artık SAP sisteminizden mesaj almaya hazır.

> [!NOTE]
> SAP tetikleyicisi bir yoklama tetikleyicisi değil, bunun yerine webhook tabanlı bir tetikleyicidir. Tetikleyici yalnızca bir ileti olduğunda ağ geçidinden çağrılır, bu nedenle yoklama gerekmez.

### <a name="test-your-logic-app"></a>Mantık uygulamanızı test edin

1. Mantık uygulamanızı tetiklemek için SAP sisteminizden bir ileti gönderin.

1. Mantık uygulaması menüsünde **Genel Bakış'ı**seçin. Mantık uygulamanız için yeni çalıştırmalar için **Koşu geçmişini** inceleyin.

1. Tetikleyici çıktılar bölümünde SAP sisteminizden gönderilen iletiyi gösteren en son çalıştırmayı açın.

## <a name="receive-idoc-packets-from-sap"></a>SAP'den IDOC paketleri alın

SAP'yi, toplu gruplar veya IDOC grupları olan [paketler halinde IDOC'lar gönderecek](https://help.sap.com/viewer/8f3819b0c24149b5959ab31070b64058/7.4.16/en-US/4ab38886549a6d8ce10000000a42189c.html)şekilde ayarlayabilirsiniz. IDOC paketlerini almak için SAP konektörü ve özellikle tetikleyicinin ek yapılandırmaya ihtiyacı yoktur. Ancak, tetikleyici paketi aldıktan sonra bir IDOC paketindeki her öğeyi işlemek için, paketi tek tek IDOC'lara bölmek için bazı ek adımlar gerekir.

Aşağıda, işlevi kullanarak bir paketten tek tek IDOC'ların nasıl ayıklanır gösteriş yapılacağını gösteren bir örnek verilmiştir: [ `xpath()` ](./workflow-definition-language-functions-reference.md#xpath)

1. Başlamadan önce SAP tetikleyicisi olan bir mantık uygulamasına ihtiyacınız var. Bu mantık uygulaması zaten yoksa, SAP tetikleyicisi olan [bir mantık uygulaması ayarlamak](#receive-from-sap)için bu konudaki önceki adımları izleyin.

   Örnek:

   ![Mantık uygulamasına SAP tetikleyicisi ekleme](./media/logic-apps-using-sap-connector/first-step-trigger.png)

1. Mantık uygulamanızın SAP'den aldığı XML IDOC'tan kök ad alanını alın. XML belgesinden bu ad alanını ayıklamak için, yerel bir dize değişkeni oluşturan `xpath()` ve bir ifade kullanarak bu ad alanını depolayan bir adım ekleyin:

   `xpath(xml(triggerBody()?['Content']), 'namespace-uri(/*)')`

   ![IDOC'tan kök ad alanı alma](./media/logic-apps-using-sap-connector/get-namespace.png)

1. Tek bir IDOC ayıklamak için, bir dizi değişkeni oluşturan ve başka `xpath()` bir ifade kullanarak IDOC koleksiyonunu depolayan bir adım ekleyin:

   `xpath(xml(triggerBody()?['Content']), '/*[local-name()="Receive"]/*[local-name()="idocData"]')`

   ![Öğelerin dizi alın](./media/logic-apps-using-sap-connector/get-array.png)

   Dizi değişkeni, her IDOC'un mantıksal uygulamanızın koleksiyon üzerinden ayrı ayrı işlenmesini sağlar. Bu örnekte, mantık uygulaması her IDOC'u bir döngü kullanarak bir SFTP sunucusuna aktarmaktadır:

   ![IDOC'u SFTP sunucusuna gönderme](./media/logic-apps-using-sap-connector/loop-batch.png)

   Her IDOC kök ad alanını içermelidir, bu nedenle dosya içeriği `<Receive></Receive` bir öğenin içinde kök ad alanı yla birlikte paketlenir ve bu durumda IDOC'u alt akış uygulamasına veya SFTP sunucusuna gönderir.

Yeni bir mantık uygulaması oluştururken Mantık Uygulama Tasarımcısı'nda bu şablonu seçerek bu desen için hızlı başlangıç şablonu kullanabilirsiniz.

![Toplu mantık uygulaması şablonu seçin](./media/logic-apps-using-sap-connector/select-batch-logic-app-template.png)

## <a name="generate-schemas-for-artifacts-in-sap"></a>SAP'deki eserler için şema oluşturma

Bu örnekte, http isteğiyle tetikleyebileceğiniz bir mantık uygulaması kullanır. SAP eylemi, belirtilen IDoc ve BAPI için şemaoluşturmak için sap sistemine bir istek gönderir. Yanıtta geri dönen şemalar, Azure Kaynak Yöneticisi bağlayıcısı kullanılarak bir tümleştirme hesabına yüklenir.

### <a name="add-an-http-request-trigger"></a>HTTP İstek tetikleyicisi ekleme

1. Azure portalında, Logic App Designer'ı açan boş bir mantık uygulaması oluşturun.

1. Arama kutusuna filtreniz olarak girin. `http request` **Tetikleyiciler** listesinden, **bir HTTP isteği ne zaman alındığını**seçin.

   ![HTTP İstek tetikleyicisi ekle](./media/logic-apps-using-sap-connector/add-http-trigger-logic-app.png)

1. Şimdi mantık uygulamanızı kaydedin, böylece mantık uygulamanız için bir uç nokta URL'si oluşturabilirsiniz.
Tasarımcı araç çubuğunda **Kaydet'i**seçin.

   Uç nokta URL artık tetikleyicinizde görünür, örneğin:

   ![Uç nokta için URL oluşturma](./media/logic-apps-using-sap-connector/generate-http-endpoint-url.png)

### <a name="add-an-sap-action-to-generate-schemas"></a>Şema oluşturmak için sap eylemi ekleme

1. Mantık Uygulama Tasarımcısı'nda, tetikleyici altında **Yeni adım'ı**seçin.

   ![Mantık uygulamasına yeni adım ekleme](./media/logic-apps-using-sap-connector/add-sap-action-logic-app.png)

1. Arama kutusuna filtreniz olarak girin. `sap` **Eylemler** listesinden **şema oluştur'u**seçin.
  
   ![Mantık uygulamasına "Şema oluşturma" eylemini ekleme](media/logic-apps-using-sap-connector/select-sap-schema-generator-action.png)

   Veya, **Kurumsal** sekmesini ve SAP eylemini seçebilirsiniz.

   ![Kurumsal sekmesinden SAP gönder eylemini seçin](media/logic-apps-using-sap-connector/select-sap-schema-generator-ent-tab.png)

1. Bağlantınız zaten varsa, SAP eyleminizi ayarlamak için bir sonraki adımla devam edin. Ancak, bağlantı ayrıntıları için sizden istenirse, bilgileri şirket içi SAP sunucunuza bağlantı oluşturabilmeniz için sağlayın.

   1. Bağlantı için bir ad sağlayın.

   1. Veri **Ağ Geçidi** bölümünde, **Abonelik**altında, ağ geçidi yüklemeniz için azure portalında oluşturduğunuz ağ geçidi kaynağı için azure aboneliğini seçin. 
   
   1. **Bağlantı Ağ Geçidi**altında, ağ geçidi kaynağınızı seçin.

   1. Bağlantı hakkında bilgi sağlamaya devam edin. **Logon Type** özelliği için, özelliğin **Application Server** veya **Group**olarak ayarlanıp ayarlmadığına bağlı olarak adımı izleyin:
   
      * **Application Server**için, genellikle isteğe bağlı görünen bu özellikler gereklidir:

        ![SAP uygulama sunucusu bağlantısı oluşturma](media/logic-apps-using-sap-connector/create-SAP-application-server-connection.png)

      * **Grup**için, genellikle isteğe bağlı görünen bu özellikler gereklidir:

        ![SAP ileti sunucusu bağlantısı oluşturma](media/logic-apps-using-sap-connector/create-SAP-message-server-connection.png)  

      Varsayılan olarak, şemaya karşı XML doğrulaması gerçekleştirerek geçersiz değerleri denetlemek için güçlü yazı yazmak kullanılır. Bu davranış, sorunları daha önce algılamanıza yardımcı olabilir. **Güvenli Yazma** seçeneği geriye dönük uyumluluk için kullanılabilir ve yalnızca dize uzunluğunu denetler. Güvenli Yazma [seçeneği](#safe-typing)hakkında daha fazla bilgi edinin.

   1. Bitirdikten sonra **Oluştur'u**seçin.

      Logic Apps, bağlantının düzgün çalıştığından emin olmak için bağlantınızı kurar ve sınar.

1. Şemayı oluşturmak istediğiniz yapıya giden yolu sağlayın.

   DOSYA seçiciden SAP eylemini seçebilirsiniz:

   ![SAP eylemini seçin](media/logic-apps-using-sap-connector/select-SAP-action-schema-generator.png)  

   Veya eylemi el ile girebilirsiniz:

   ![SAP eylemini el ile girin](media/logic-apps-using-sap-connector/manual-enter-SAP-action-schema-generator.png)

   Birden fazla yapı için şema oluşturmak için, örneğin her yapı için SAP eylem ayrıntılarını sağlayın:

   ![Yeni öğe ekle'yi seçin](media/logic-apps-using-sap-connector/schema-generator-array-pick.png)

   ![İki öğeyi göster](media/logic-apps-using-sap-connector/schema-generator-example.png)

   SAP eylemi hakkında daha fazla bilgi [için, IDOC işlemleri için İleti şemalarına](https://docs.microsoft.com/biztalk/adapters-and-accelerators/adapter-sap/message-schemas-for-idoc-operations)bakın.

1. Mantıksal uygulamanızı kaydedin. Tasarımcı araç çubuğunda **Kaydet'i**seçin.

### <a name="test-your-logic-app"></a>Mantık uygulamanızı test edin

1. Tasarımcı araç çubuğunda, mantık uygulamanız için bir çalıştırmayı tetiklemek için **Çalıştır'ı** seçin.

1. Çalıştırmayı açın ve **Şema Oluştur** eylemi için çıktıları denetleyin.

   Çıktılar, belirtilen ileti listesi için oluşturulan şemaları gösterir.

### <a name="upload-schemas-to-an-integration-account"></a>Şemaları bir tümleştirme hesabına yükleme

İsteğe bağlı olarak, oluşturulan şemaları blob, depolama veya tümleştirme hesabı gibi depolarda indirebilir veya depolayabilirsiniz. Tümleştirme hesapları diğer XML eylemleriyle birinci sınıf bir deneyim sağlar, bu nedenle bu örnek, Azure Kaynak Yöneticisi bağlayıcısını kullanarak aynı mantık uygulaması için bir tümleştirme hesabına şemaların nasıl yüklenilebildiğini gösterir.

1. Mantık Uygulama Tasarımcısı'nda, tetikleyici altında **Yeni adım'ı**seçin.

1. Arama kutusuna filtreniz olarak girin. `Resource Manager` **Kaynak Oluştur'u veya güncelleştir'i**seçin.

   ![Azure Kaynak Yöneticisi eylemini seçin](media/logic-apps-using-sap-connector/select-azure-resource-manager-action.png)

1. Azure aboneliğiniz, Azure kaynak grubunuz ve tümleştirme hesabınız da dahil olmak üzere eylemin ayrıntılarını girin. Alanlara SAP belirteçleri eklemek için, bu alanların kutularının içine tıklayın ve görünen dinamik içerik listesinden seçim yapmayı n için.

   1. Yeni **parametre** ekle listesini açın ve **Konum** ve **Özellikler** alanlarını seçin.

   1. Bu örnekte gösterildiği gibi bu yeni alanlar için ayrıntıları sağlayın.

      ![Azure Kaynak Yöneticisi eylemi için ayrıntıları girin](media/logic-apps-using-sap-connector/azure-resource-manager-action.png)

   SAP **Oluşturma şemaları** eylem bir koleksiyon olarak şema oluşturur, böylece tasarımcı otomatik olarak eylem **için her** döngü için bir döngü ekler. Aşağıda, bu eylemin nasıl göründüğünü gösteren bir örnek verilmiştir:

   !["Her biri için" döngüsüyle Azure Kaynak Yöneticisi eylemi](media/logic-apps-using-sap-connector/azure-resource-manager-action-foreach.png)

   > [!NOTE]
   > Şemalar base64 kodlanmış biçimi kullanır. Şemaları bir tümleştirme hesabına yüklemek `base64ToString()` için, işlevleri kullanılarak deşifre edilmeleri gerekir. Öğenin kodunu gösteren bir örnek `"properties"` aşağıda verilmiştir:
   >
   > ```json
   > "properties": {
   >    "Content": "@base64ToString(items('For_each')?['Content'])",
   >    "ContentType": "application/xml",
   >    "SchemaType": "Xml"
   > }
   > ```

1. Mantıksal uygulamanızı kaydedin. Tasarımcı araç çubuğunda **Kaydet'i**seçin.

### <a name="test-your-logic-app"></a>Mantık uygulamanızı test edin

1. Tasarımcı araç çubuğunda, mantık uygulamanızı el ile tetiklemek için **Çalıştır'ı** seçin.

1. Başarılı bir çalıştırmadan sonra tümleştirme hesabına gidin ve oluşturulan şemaların var olup olmadığını kontrol edin.

## <a name="enable-secure-network-communications"></a>Güvenli Ağ İletişimini Etkinleştir

Başlamadan önce, daha önce listelenen ön [koşulları](#pre-reqs)yerine getirerek:

* Şirket içi veri ağ geçidi, SAP sisteminizle aynı ağda bulunan bir makineye yüklenir.
* SSO için ağ geçidi, sap kullanıcısına eşlenen bir kullanıcı olarak çalışıyor.
* Ek güvenlik işlevleri sağlayan SNC kitaplığı, veri ağ geçidiyle aynı makineye yüklenir. Bazı örnekler [sapseculib,](https://help.sap.com/saphelp_nw74/helpdata/en/7a/0755dc6ef84f76890a77ad6eb13b13/frameset.htm)Kerberos ve NTLM içerir.

   SAP sistemine veya SAP sisteminden gelen istekleriniz için SNC'yi etkinleştirmek için SAP bağlantısındaki **SNC kullan** onay kutusunu seçin ve şu özellikleri sağlayın:

   ![Bağlantıda SAP SNC'yi yapılandır](media/logic-apps-using-sap-connector/configure-sapsnc.png)

   | Özellik | Açıklama |
   |----------| ------------|
   | **SNC Kütüphane Yolu** | NCo yükleme konumuna veya mutlak yola göre SNC kitaplık adı veya yolu. Örnekler `sapsnc.dll` veya `.\security\sapsnc.dll` `c:\security\sapsnc.dll`. |
   | **SNC SSO** | SNC üzerinden bağlandığınızda, SNC kimliği genellikle arayanın kimliğini doğrulamak için kullanılır. Başka bir seçenek, kullanıcı ve parola bilgilerinin arayanın kimliğini doğrulamak için kullanılabilmesi için geçersiz kılmaktır, ancak satır hala şifrelenir. |
   | **SNC Benim Adım** | Çoğu durumda, bu özellik atlanabilir. Yüklü SNC çözümü genellikle kendi SNC adını bilir. Yalnızca birden çok kimliği destekleyen çözümler için, bu belirli hedef veya sunucu için kullanılacak kimliği belirtmeniz gerekebilir. |
   | **SNC Ortak Adı** | Arka uç SNC'nin adı. |
   | **SNC Koruma Kalitesi** | Bu belirli hedefin veya sunucunun SNC iletişimi için kullanılacak hizmet kalitesi. Varsayılan değer arka uç sistemi tarafından tanımlanır. Maksimum değer, SNC için kullanılan güvenlik ürünü tarafından tanımlanır. |
   |||

   > [!NOTE]
   > Veri ağ geçidine ve SNC kitaplığına sahip olduğunuz makinede ortam değişkenlerini SNC_LIB ve SNC_LIB_64 ayarlamayın. Ayarlanırsa, bağlayıcıdan geçen SNC kitaplık değerinden önceliklidirler.

<a name="safe-typing"></a>

## <a name="safe-typing"></a>Güvenli yazma

Varsayılan olarak, SAP bağlantınızı oluşturduğunuzda, şemaya karşı XML doğrulaması gerçekleştirerek geçersiz değerleri denetlemek için güçlü yazı yazmak kullanılır. Bu davranış, sorunları daha önce algılamanıza yardımcı olabilir. **Güvenli Yazma** seçeneği geriye dönük uyumluluk için kullanılabilir ve yalnızca dize uzunluğunu denetler. **Güvenli Yazma'yı**seçerseniz, SAP'deki DATS türü ve TIMS türü XML eşdeğerleri yerine dizeleri olarak kabul edilir `xs:date` ve `xs:time`, nerede `xmlns:xs="http://www.w3.org/2001/XMLSchema"`. Güvenli yazma tüm şema oluşturma, hem "gönderildi" yükü ve "alındı" yanıtı ve tetikleyici için gönderme iletisi için davranışı etkiler. 

Güçlü yazma kullanıldığında **(Güvenli Yazma** etkin değil), şema DATS ve TIMS türlerini daha basit XML türleri ile eşler:

```xml
<xs:element minOccurs="0" maxOccurs="1" name="UPDDAT" nillable="true" type="xs:date"/>
<xs:element minOccurs="0" maxOccurs="1" name="UPDTIM" nillable="true" type="xs:time"/>
```

Güçlü yazıkullanarak ileti gönderdiğinde, DATS ve TIMS yanıtı eşleşen XML türü biçimine uyar:

```xml
<DATE>9999-12-31</DATE>
<TIME>23:59:59</TIME>
```

**Güvenli Yazma** etkinleştirildiğinde, şema DATS ve TIMS türlerini yalnızca uzunluk kısıtlamalarıyla XML dize alanlarıyla eşler, örneğin:

```xml
<xs:element minOccurs="0" maxOccurs="1" name="UPDDAT" nillable="true">
  <xs:simpleType>
    <xs:restriction base="xs:string">
      <xs:maxLength value="8" />
    </xs:restriction>
  </xs:simpleType>
</xs:element>
<xs:element minOccurs="0" maxOccurs="1" name="UPDTIM" nillable="true">
  <xs:simpleType>
    <xs:restriction base="xs:string">
      <xs:maxLength value="6" />
    </xs:restriction>
  </xs:simpleType>
</xs:element>
```

**Güvenli Yazma** etkinken iletiler gönderildiğinde, DATS ve TIMS yanıtı aşağıdaki örnek gibi görünür:

```xml
<DATE>99991231</DATE>
<TIME>235959</TIME>
```

## <a name="advanced-scenarios"></a>Gelişmiş senaryolar

### <a name="confirm-transaction-explicitly"></a>Hareketi açıkça onaylama

Logic Apps'tan SAP'ye işlem gönderdiğinde, bu değişim SAP belgesinde açıklandığı gibi iki adımda gerçekleşir, [Transactional RFC Server Programs](https://help.sap.com/doc/saphelp_nwpi71/7.1/en-US/22/042ad7488911d189490000e829fbbd/content.htm?no_cache=true). Varsayılan olarak, **SAP'ye Gönder** eylemi hem işlev aktarımı hem de tek bir çağrıda hareket onayı için adımları işler. SAP bağlayıcısı, bu adımları ayırma seçeneği sunar. Bir IDOC gönderebilir ve işlemi otomatik olarak onaylamak yerine, açık **Hareket Kimliğini Onayla** eylemini kullanabilirsiniz.

İşlem kimliği onayını ayırma özelliği, sap'daki hareketleri çoğaltmak istemediğiniz durumlarda (örneğin, ağ sorunları gibi nedenlerden kaynaklanan hatalar nedeniyle hatalar meydana gelebilecek senaryolarda) yararlıdır. Hareket kimliğini ayrı ayrı onaylayarak, işlem SAP sisteminizde yalnızca bir kez tamamlanır.

Aşağıda, bu deseni gösteren bir örnek verilmiştir:

1. Boş bir mantık uygulaması oluşturun ve bir HTTP tetikleyicisi ekleyin.

1. SAP bağlayıcısından **IDOC gönder** eylemini ekleyin. SAP sisteminize gönderdiğiniz IDOC'un ayrıntılarını sağlayın.

1. **TiD'yi onayla** alanında işlem kimliğini ayrı bir adımda açıkça onaylamak için **No**seçeneğini belirleyin. İsteğe bağlı **İşlem Kimliği GUID** alanı için, değeri el ile belirtebilir veya bağlayıcının iDOC Gönder eyleminden gelen yanıtta bu GUID'i otomatik olarak oluşturmasını ve döndürmesini sağlayabilirsiniz.

   ![IDOC eylem özelliklerini gönderme](./media/logic-apps-using-sap-connector/send-idoc-action-details.png)

1. İşlem kimliğini açıkça onaylamak **için, hareket kimliğini onayla** eylemini ekleyin. Dinamik içerik listesinin görünmesi için **İşlem Kimliği** kutusunun içini tıklatın. Bu listeden, **IDOC Gönder** eyleminden döndürülen **İşlem Kimliği** değerini seçin.

   ![İşlem kimliği eylemini onaylama](./media/logic-apps-using-sap-connector/explicit-transaction-id.png)

   Bu adım çalıştırıladıktan sonra, geçerli hareket her iki uçta, SAP bağlayıcı tarafında ve SAP sistem tarafında tamamlanır.

## <a name="known-issues-and-limitations"></a>Bilinen sorunlar ve sınırlamalar

SAP bağlayıcısı için şu anda bilinen sorunlar ve sınırlamalar şunlardır:

* SAP tetikleyicisi veri ağ geçidi kümelerini desteklemez. Bazı başarısız durumlarda, SAP sistemiyle iletişim kurandaki veri ağ geçidi düğümü etkin düğümden farklı olabilir ve bu da beklenmeyen davranışlara neden olabilir. Gönderme senaryoları için veri ağ geçidi kümeleri desteklenir.

* SAP bağlayıcısı şu anda SAP yönlendirici dizelerini desteklemiyor. Şirket içi veri ağ geçidi, bağlanmak istediğiniz SAP sistemiyle aynı LAN'da bulunmalıdır.

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Tetikleyiciler, eylemler ve konektörün Swagger dosyasında açıklandığı gibi sınırlar gibi bu bağlayıcı hakkında daha fazla teknik ayrıntı için [bağlayıcının başvuru sayfasına](https://docs.microsoft.com/connectors/sap/)bakın.

> [!NOTE]
> [Bir entegrasyon hizmeti ortamındaki (İmKB)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md)mantık uygulamaları için, bu bağlayıcının İmKB etiketli sürümü bunun yerine [İmKB ileti sınırlarını](../logic-apps/logic-apps-limits-and-config.md#message-size-limits) kullanır.

## <a name="next-steps"></a>Sonraki adımlar

* Azure Logic Apps'tan [şirket içi sistemlere bağlanın.](../logic-apps/logic-apps-gateway-connection.md)
* [Kurumsal Tümleştirme Paketi](../logic-apps/logic-apps-enterprise-integration-overview.md)ile diğer ileti işlemlerini nasıl doğrulayasınız, dönüştürecek ve kullanacağınızı öğrenin.
* Diğer [Logic Apps bağlayıcıları](../connectors/apis-list.md)hakkında bilgi edinin.
