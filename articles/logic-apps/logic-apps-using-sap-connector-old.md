---
title: SAP sistemlerine bağlanma-Azure Logic Apps | Microsoft Docs
description: Azure Logic Apps ile iş akışlarını otomatikleştirerek SAP kaynaklarına erişme ve bunları yönetme
author: ecfan
ms.author: estfan
ms.date: 05/31/2018
ms.topic: article
ms.service: logic-apps
services: logic-apps
ms.reviewer: klam, divswa, LADocs
ms.suite: integration
tags: connectors
ms.openlocfilehash: 31f6a3fc281b8dc309ddcd237246c870c85ae20b
ms.sourcegitcommit: 47b00a15ef112c8b513046c668a33e20fd3b3119
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/22/2019
ms.locfileid: "69971636"
---
# <a name="connect-to-sap-systems-from-azure-logic-apps"></a>Azure Logic Apps 'dan SAP sistemlerine bağlanma

> [!NOTE]
> Bu SAP Bağlayıcısı kullanımdan kaldırılması için zamanlandı. Lütfen [daha yeni ve daha GELIŞMIŞ SAP bağlayıcısını](./logic-apps-using-sap-connector.md)kullanın veya geçirin. 

Bu makalede SAP uygulama sunucusu ve SAP Ileti sunucusu bağlayıcıları kullanarak SAP kaynaklarınıza bir Logic app içinden nasıl erişebileceğiniz gösterilmektedir. Bu şekilde, Logic Apps oluşturarak SAP verilerinizi ve kaynaklarınızı yöneten görevleri, işlemleri ve iş akışlarını otomatikleştirebileceğinizi unutmayın.

Bu örnek, bir HTTP isteğiyle tetikleyebileceğiniz bir mantıksal uygulama kullanır. Mantıksal uygulama bir SAP sunucusuna ara belge (IDoc) gönderir ve mantıksal uygulamayı çağıran istek sahibine bir yanıt döndürür.
Geçerli SAP bağlayıcılarında eylemler var, ancak tetiklenemez, bu nedenle bu örnek mantıksal uygulamanın iş akışındaki ilk adım olarak [http istek tetikleyicisi](../connectors/connectors-native-reqres.md) 'ni kullanır. SAP bağlayıcısına özgü teknik bilgiler için şu başvuru makalelerine bakın: 

* <a href="https://docs.microsoft.com/connectors/sap" target="blank">SAP uygulama sunucusu Bağlayıcısı</a>
* <a href="https://docs.microsoft.com/connectors/sap/#send-message-to-sap" target="blank">SAP Ileti sunucusu Bağlayıcısı</a>

Henüz bir Azure aboneliğiniz yoksa <a href="https://azure.microsoft.com/free/" target="_blank">ücretsiz bir Azure hesabı için kaydolun</a>.

## <a name="prerequisites"></a>Önkoşullar

Bu makaleyle birlikte takip etmek için şu öğelere ihtiyacınız vardır:

* SAP sisteminize erişmek istediğiniz mantıksal uygulama ve mantıksal uygulamanızın iş akışını başlatan bir tetikleyici. SAP bağlayıcıları Şu anda yalnızca eylemleri sunmaktadır. Logic Apps 'e yeni başladıysanız [Azure Logic Apps](../logic-apps/logic-apps-overview.md) ve [hızlı başlangıç ' ı inceleyin: İlk mantıksal uygulamanızı](../logic-apps/quickstart-create-first-logic-app-workflow.md)oluşturun.

* <a href="https://wiki.scn.sap.com/wiki/display/ABAP/ABAP+Application+Server" target="_blank">SAP uygulama sunucunuz</a> veya <a href="https://help.sap.com/saphelp_nw70/helpdata/en/40/c235c15ab7468bb31599cc759179ef/frameset.htm" target="_blank">SAP ileti sunucunuz</a>

* Şirket içi bilgisayarlara en son şirket içi [veri ağ geçidini](https://www.microsoft.com/download/details.aspx?id=53127) indirip yükleyin. Devam etmeden önce Azure portal ağ geçidinizi ayarladığınızdan emin olun. Ağ Geçidi, verilere ve kaynaklara güvenli bir şekilde erişmenize yardımcı olur. Daha fazla bilgi için bkz. [Azure Logic Apps için şirket içi veri ağ geçidini yüklemeyin](../logic-apps/logic-apps-gateway-install.md).

* <a href="https://softwaredownloads.sap.com/file/0020000000086282018" target="_blank">Microsoft .NET Framework 4,0 ve Windows 64bit (x64) için şu anda SAP Bağlayıcısı (NCo) 3.0.20.0</a>olan en son SAP istemci kitaplığını, şirket içi veri ağ geçidiyle aynı bilgisayarda indirin ve yükleyin. Bu sürümü veya daha yenisini bu nedenlerle yükler:

  * Daha eski SAP NCo sürümleri aynı anda birden fazla IDoc iletisi gönderildiğinde kilitlenebilir. 
  Bu koşul, SAP hedefine gönderilen ve mesajların zaman aşımına neden olan tüm sonraki iletileri engeller.

  * Şirket içi veri ağ geçidi yalnızca 64 bitlik sistemlerde çalışır. 
  Aksi takdirde, veri ağ geçidi ana bilgisayar hizmeti 32 bitlik derlemeleri desteklemediğinden "kötü görüntü" hatası alırsınız.

  * Hem veri ağ geçidi ana bilgisayar hizmeti hem de Microsoft SAP bağdaştırıcısı .NET Framework 4,5 kullanır. .NET Framework 4,0 için SAP NCo, 4.7.1 için .NET Runtime 4,0 kullanan işlemlerle çalışmaktadır. 
  .NET Framework 2,0 için SAP NCo, .NET çalışma zamanı 2,0 ile 3,5 ve en son şirket içi veri ağ geçidiyle artık çalışmakla birlikte çalışan işlemlerle çalışmaktadır.

* SAP sunucunuza gönderebilmeniz için örnek bir IDoc dosyası gibi ileti içeriği. Bu içerik XML biçiminde olmalıdır ve kullanmak istediğiniz SAP eyleminin ad alanını içermelidir.

<a name="add-trigger"></a>

## <a name="add-http-request-trigger"></a>HTTP istek tetikleyicisi Ekle

Azure Logic Apps, her mantıksal uygulama, belirli bir olay gerçekleştiğinde [](../logic-apps/logic-apps-overview.md#logic-app-concepts)veya belirli bir koşul karşılandığında tetiklenen bir tetikleyiciyle başlamalıdır. Tetikleyici her tetiklendiğinde Logic Apps altyapısı bir mantıksal uygulama örneği oluşturur ve uygulamanızın iş akışını çalıştırmaya başlar.

Bu örnekte, mantıksal uygulamanıza *http post istekleri* gönderebilmeniz için Azure 'da bir uç nokta ile bir mantıksal uygulama oluşturursunuz. Mantıksal uygulamanız bu HTTP isteklerini aldığında, tetikleyici ateşlenir ve iş akışınızda bir sonraki adımı çalıştırır.

1. Azure portal, mantıksal uygulama Tasarımcısı ' nı açan boş bir mantıksal uygulama oluşturun. 

2. Arama kutusuna filtreniz olarak "http isteği" yazın. Tetikleyiciler listesinde, bu tetikleyiciyi seçin: **İstek-bir HTTP isteği alındığında**

   ![HTTP Istek tetikleyicisi Ekle](./media/logic-apps-using-sap-connector-old/add-trigger.png)

3. Mantıksal uygulamanız için bir uç nokta URL 'SI oluşturabilmeniz için artık mantıksal uygulamanızı kaydedin.
Tasarımcı araç çubuğunda **Kaydet**'i seçin. 

   Uç nokta URL 'SI Şu anda tetikleyicide görüntülenir, örneğin:

   ![Uç nokta için URL Oluştur](./media/logic-apps-using-sap-connector-old/generate-http-endpoint-url.png)

<a name="add-action"></a>

## <a name="add-sap-action"></a>SAP eylemi ekleme

Azure Logic Apps bir [eylem](../logic-apps/logic-apps-overview.md#logic-app-concepts) , iş akışınızda bir tetikleyiciyi veya başka bir eylemi izleyen bir adımdır. Mantıksal uygulamanıza henüz bir tetikleyici eklemediyseniz ve bu örneği izlemek istiyorsanız, [Bu bölümde açıklanan tetikleyiciyi ekleyin](#add-trigger).

1. Mantıksal uygulama Tasarımcısı ' nda, tetikleyici altında **yeni adım** > **Eylem Ekle**' yi seçin.

   ![Eylem ekle](./media/logic-apps-using-sap-connector-old/add-action.png) 

2. Arama kutusuna filtreniz olarak "sap Server" yazın. Eylemler listesinden SAP sunucunuz için eylemi seçin: 

   * **SAP uygulama sunucusu-SAP 'ye gönderme**
   * **SAP Ileti sunucusu-SAP 'ye gönderme**

   Bu örnek, bu eylemi kullanır: **SAP uygulama sunucusu-SAP 'ye gönderme**

   !["SAP Application Server" veya "SAP Message Server" ı seçin](media/logic-apps-using-sap-connector-old/select-sap-action.png)

3. Bağlantı ayrıntıları istenirse, SAP bağlantınızı şimdi oluşturun. Aksi takdirde, bağlantınız zaten varsa, SAP eyleminizi ayarlamak için bir sonraki adımla devam edin. 

   **Şirket içi SAP bağlantısı oluşturma**

   1. **Ağ geçitleri**için şirket içi bağlantı özelliklerinin görünmesi için şirket **içi veri ağ geçidi üzerinden Bağlan** ' ı seçin.

   2. SAP sunucunuzun bağlantı bilgilerini sağlayın. 
   **Ağ geçidi** özelliği için, ağ geçidi yüklemeniz için Azure Portal oluşturduğunuz veri ağ geçidini seçin; örneğin:

      **SAP uygulama sunucusu**

      ![SAP uygulama sunucusu bağlantısı oluşturma](./media/logic-apps-using-sap-connector-old/create-SAP-app-server-connection.png)  

      **SAP Ileti sunucusu**

      ![SAP ileti sunucusu bağlantısı oluştur](media/logic-apps-using-sap-connector-old/create-SAP-message-server-connection.png) 

   2. İşiniz bittiğinde **Oluştur**’u seçin.

      Logic Apps bağlantınızın düzgün çalıştığından emin olmak için bağlantınızı ayarlar ve test eder.

4. Şimdi SAP sunucunuzdaki bir eylemi bulun ve seçin. 

   1. **SAP eylemi** kutusunda klasör simgesini seçin. 
   Klasör listesinde, kullanmak istediğiniz eylemi bulun ve seçin. 

      Bu örnek, IDoc eylemi için **IDoc** kategorisini seçer. 

      ![IDoc eylemini bul ve Seç](./media/logic-apps-using-sap-connector-old/SAP-app-server-find-action.png)

      İstediğiniz eylemi bulamazsanız, el ile bir yol girebilirsiniz, örneğin:

      ![IDoc eyleminin yolunu el ile sağlayın](./media/logic-apps-using-sap-connector-old/SAP-app-server-manually-enter-action.png)

      IDOC işlemleri hakkında daha fazla bilgi için bkz. [IDOC işlemleri Için ileti şemaları](https://docs.microsoft.com/biztalk/adapters-and-accelerators/adapter-sap/message-schemas-for-idoc-operations)

   2. Dinamik içerik listesinin görünmesi için **giriş iletisi** kutusunun içine tıklayın. 
   Bu listede, **BIR http isteği alındığında**altında **gövde** alanını seçin. 

      Bu adım, HTTP istek tetikleyicinizden gelen gövde içeriğini içerir ve bu çıktıyı SAP sunucunuza gönderir.

      !["Gövde" alanını seçin](./media/logic-apps-using-sap-connector-old/SAP-app-server-action-select-body.png)

      İşiniz bittiğinde, SAP eyleminiz Şu örneğe benzer şekilde görünür:

      ![Tamamlanmış SAP eylemi](./media/logic-apps-using-sap-connector-old/SAP-app-server-complete-action.png)

6. Mantıksal uygulamanızı kaydedin. Tasarımcı araç çubuğunda **Kaydet**'i seçin.

<a name="add-response"></a>

## <a name="add-http-response-action"></a>HTTP yanıtı ekleme eylemi

Şimdi mantıksal uygulamanızın iş akışına bir yanıt eylemi ekleyin ve SAP eyleminden çıktıyı dahil edin. Bu şekilde, mantıksal uygulamanız SAP sunucunuzdaki sonuçları özgün istek sahibine döndürür. 

1. Mantıksal uygulama Tasarımcısı ' nda, SAP eylemi altında **yeni adım** > **Eylem Ekle**' yi seçin.

2. Arama kutusuna filtreniz olarak "yanıt" yazın. Eylemler listesinden şu eylemi seçin: **İstek-yanıt**

3. Dinamik içerik listesinin görünmesi için **gövde** kutusunun içine tıklayın. Bu listeden, **SAP gönder**' ın altında **gövde** alanını seçin. 

   ![Tamamlanmış SAP eylemi](./media/logic-apps-using-sap-connector-old/select-sap-body-for-response-action.png)

4. Mantıksal uygulamanızı kaydedin. 

## <a name="test-your-logic-app"></a>Mantıksal uygulamanızı test etme

1. Mantıksal uygulamanız zaten etkin değilse, mantıksal uygulama menüsünde **genel bakış**' ı seçin. Araç çubuğunda **Etkinleştir**' i seçin. 

2. Mantıksal uygulama Tasarımcısı araç çubuğunda **Çalıştır**' ı seçin. Bu adım mantıksal uygulamanızı el ile başlatır.

3. HTTP istek tetikleyicinizdeki URL 'ye bir HTTP POST isteği göndererek mantıksal uygulamanızı tetikleyin ve isteğiniz ile ileti içeriğinizi ekleyin. İsteği göndermek için [Postman](https://www.getpostman.com/apps)gibi bir araç kullanabilirsiniz. 

   Bu makalede, istek XML biçiminde olması gereken bir IDoc dosyası gönderir ve kullanmakta olduğunuz SAP eyleminin ad alanını içermelidir, örneğin: 

   ``` xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <Send xmlns="http://Microsoft.LobServices.Sap/2007/03/Idoc/3/ORDERS05//620/Send">
      <idocData>
         <...>
      </idocData>
   </Send>
   ```

4. HTTP isteğinizi gönderdikten sonra, mantıksal uygulamanızdan gelen yanıtı bekleyin.

> [!NOTE]
> Yanıt için gereken tüm adımlar [istek zaman aşımı sınırı](./logic-apps-limits-and-config.md)içinde tamamlanmazsa mantıksal uygulamanız zaman aşımına uğrayabilir. Bu durum oluşursa, istekler engellenebilir. Sorunları tanılamanıza yardımcı olması için [Logic Apps 'i nasıl denetleyebilir ve izleyebileceğinizi](../logic-apps/logic-apps-monitor-your-logic-apps.md)öğrenin.

Tebrikler, artık SAP sunucunuz ile iletişim kurabilen bir mantıksal uygulama oluşturdunuz. Mantıksal uygulamanız için bir SAP bağlantısı ayarladığınıza göre, BAPı ve RFC gibi diğer kullanılabilir SAP eylemlerini inceleyebilirsiniz.

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Bağlayıcıların Swagger dosyalarında açıklandığı şekilde bağlayıcı hakkında teknik ayrıntılar için aşağıdaki başvuru makalelerine bakın: 

* [SAP uygulama sunucusu](/connectors/sap)
* [SAP Ileti sunucusu](/connectors/sap/#send-message-to-sap)

## <a name="get-support"></a>Destek alın

* Sorularınız için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.
* Özelliklerle ilgili fikirlerinizi göndermek veya gönderilmiş olanları oylamak için [Logic Apps kullanıcı geri bildirimi sitesini](https://aka.ms/logicapps-wish) ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

* Logic Apps 'ten [Şirket içi sistemlere bağlanma](../logic-apps/logic-apps-gateway-connection.md)
* [Enterprise Integration Pack](../logic-apps/logic-apps-enterprise-integration-overview.md) ile doğrulama, dönüştürme ve diğer ileti işlemlerini nasıl doğrulayacağınızı öğrenin
* Diğer [Logic Apps bağlayıcıları](../connectors/apis-list.md) hakkında bilgi edinin
