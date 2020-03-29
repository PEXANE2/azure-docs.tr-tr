---
title: Azure Zamanlayıcısı'ndan Azure Mantıksal Uygulamalarına geçiş yapın
description: Kullanımdan kaldırılan Azure Zamanlayıcısı'ndaki işleri Azure Mantık Uygulamaları ile nasıl değiştirebileceğinizi öğrenin
services: scheduler
ms.service: scheduler
ms.suite: infrastructure-services
author: derek1ee
ms.author: deli
ms.reviewer: klam, estfan
ms.topic: article
ms.date: 02/29/2020
ms.openlocfilehash: 90c3cc2e096b9b58465987bc53f718c5d06c6203
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78899044"
---
# <a name="migrate-azure-scheduler-jobs-to-azure-logic-apps"></a>Azure Zamanlayıcısı işlerini Azure Mantık Uygulamalarına geçirin

> [!IMPORTANT]
> [Azure Logic Apps,](../logic-apps/logic-apps-overview.md) [kullanımdan kaldırılan](#retire-date)Azure Zamanlayıcısı'nın yerini alıyor. Zamanlayıcı'da ayarladığınız işlerle çalışmaya devam etmek için, lütfen bu makaleyi izleyerek mümkün olan en kısa sürede Azure Logic Apps'a geçirin. 
>
> Zamanlayıcı artık Azure portalında kullanılamıyor, ancak işlerinizi ve iş koleksiyonlarınızı yönetebilmeniz için [ŞU anda REST API](/rest/api/scheduler) ve [Azure Scheduler PowerShell cmdlets'i](scheduler-powershell-reference.md) kullanılabilir durumda kalır.

Bu makalede, Azure Zamanlayıcısı yerine Azure Logic Apps ile otomatik iş akışları oluşturarak tek seferlik ve yinelenen işleri nasıl zamanlayabileceğiniz gösterilmektedir. Logic Apps ile zamanlanmış işler oluşturduğunuzda şu avantajlardan yararlanırsınız:

* Azure Blob Depolama, Azure Hizmet Veri Servisi, Office 365 Outlook ve SAP gibi yüzlerce hizmetten görsel bir tasarımcı ve [kullanıma hazır bağlayıcılar](../connectors/apis-list.md) kullanarak işinizi oluşturun.

* Her zamanlanmış iş akışını birinci sınıf bir Azure kaynağı olarak yönetin. Her mantık uygulaması ayrı bir Azure kaynağı *olduğundan, iş toplama* kavramı hakkında endişelenmenize gerek yoktur.

* Tek bir mantık uygulaması kullanarak birden çok tek seferlik işleri çalıştırın.

* Saat dilimlerini destekleyen zamanlamaları ayarlayın ve gün ışığından yararlanma saatine (DST) otomatik olarak ayarlayın.

Daha fazla bilgi edinmek için [Azure Mantık Uygulamaları nedir?](../logic-apps/logic-apps-overview.md) veya bu hızlı başlangıçta ilk mantık uygulamanızı oluşturmayı deneyin: [İlk mantık uygulamanızı oluşturun.](../logic-apps/quickstart-create-first-logic-app-workflow.md)

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/).

* HTTP istekleri göndererek mantık uygulamanızı tetiklemek için [Postacı masaüstü uygulaması](https://www.getpostman.com/apps)gibi bir araç kullanın.

## <a name="migrate-by-using-a-script"></a>Komut dosyası kullanarak geçirin

Her Zamanlayıcı işi benzersizdir, bu nedenle Zamanlayıcı işlerini Azure Logic Apps'a geçirmek için her şeye uyan tek bir araç yoktur. Ancak, [bu komut dosyasını](https://github.com/Azure/logicapps/tree/master/scripts/scheduler-migration) gereksinimlerinizi karşılamak için edinebilirsiniz.

## <a name="schedule-one-time-jobs"></a>Tek seferlik işler zamanlama

Tek bir mantık uygulaması oluşturarak birden çok tek seferlik işleri çalıştırabilirsiniz.

1. Azure [portalında,](https://portal.azure.com)Logic App Designer'da boş bir mantık uygulaması oluşturun.

   Temel adımlar için [Quickstart'ı izleyin: İlk mantık uygulamanızı oluşturun.](../logic-apps/quickstart-create-first-logic-app-workflow.md)

1. Arama kutusuna, `when a http request` İstek tetikleyicisini bulmak için girin. Tetikleyiciler listesinden şu tetikleyiciyi seçin: **Bir HTTP isteği aldığında**

   !["İstek" tetikleyicisi ekleme](./media/migrate-from-scheduler-to-logic-apps/request-trigger.png)

1. İstek tetikleyicisi için isteğe bağlı olarak, Mantık Uygulama Tasarımcısı'nın İstek tetikleyicisine gelen çağrıda yer alan girişlerin yapısını anlamasına yardımcı olan ve çıktıları iş akışınızda daha sonra seçmenize kolaylaştıran bir JSON şeması sağlayabilirsiniz.

   İstek **Gövdesi JSON Schema kutusuna,** örneğin şema girin:

   ![İstek şema](./media/migrate-from-scheduler-to-logic-apps/request-schema.png)

   Şema nız yoksa, ancak JSON formatında örnek bir yükünüz varsa, bu yükten bir şema oluşturabilirsiniz.

   1. İstek tetikleyicisinde **şema oluşturmak için örnek yükü kullan'ı**seçin.

   1. **Örnek bir JSON yükünü girin veya yapıştırın,** örnek yükünüzü sağlayın ve örneğin **Bitti'yi**seçin:

      ![Örnek yük](./media/migrate-from-scheduler-to-logic-apps/sample-payload.png)

      ```json
      {
         "runat": "2012-08-04T00:00Z",
         "endpoint": "https://www.bing.com"
      }
      ```

1. Tetikleyicinin altında **Sonraki adım'ı**seçin.

1. Arama kutusuna filtreniz olarak girin. `delay until` Eylemler listesinin altında, bu eylemi **seçin:**

   Bu eylem, mantık uygulaması iş akışınızı belirli bir tarih ve saate kadar duraklatlar.

   !["Gecikmeye kadar" eylemi ekleme](./media/migrate-from-scheduler-to-logic-apps/delay-until.png)

1. Mantık uygulamasının iş akışını başlatmak istediğinizde zaman damgasını girin.

   **Zaman Damgası** kutusunun içini tıklattığınızda, tetikleyiciden isteğe bağlı olarak bir çıktı seçebilmeniz için dinamik içerik listesi görüntülenir.

   !["Gecikme" ayrıntılarını sağlayın](./media/migrate-from-scheduler-to-logic-apps/delay-until-details.png)

1. [Yüzlerce kullanıma hazır konektör](../connectors/apis-list.md)arasından seçim yaparak çalıştırmak istediğiniz diğer eylemleri ekleyin.

   Örneğin, bir URL'ye istek gönderen bir HTTP eylemi veya Depolama Kuyrukları, Servis Veri Gönderi kuyrukları veya Hizmet Veri Gönderi konularıyla çalışan eylemler ekleyebilirsiniz:

   ![HTTP eylem](./media/migrate-from-scheduler-to-logic-apps/request-http-action.png)

1. Işiniz bittiğinde, mantık uygulamanızı kaydedin.

   ![Mantıksal uygulamanızı kaydetme](./media/migrate-from-scheduler-to-logic-apps/save-logic-app.png)

   Mantık uygulamanızı ilk kez kaydettiğinizde, mantık uygulamanızın İstek tetikleyicisinin bitiş noktası URL'si **HTTP POST URL** kutusunda görünür. Mantık uygulamanızı aramak ve işlenmek üzere mantık uygulamanıza giriş göndermek istediğinizde, bu URL'yi arama hedefi olarak kullanın.

   ![İstek tetikleyici uç noktası URL'si kaydet](./media/migrate-from-scheduler-to-logic-apps/request-endpoint-url.png)

1. Bu uç nokta URL'sini kopyalayın ve kaydedin, böylece daha sonra mantık uygulamanızı tetikleyen bir el ile istek gönderebilirsiniz.

## <a name="start-a-one-time-job"></a>Tek seferlik bir işe başlama

Tek seferlik bir işi el ile çalıştırmak veya tetiklemek için, mantık uygulamanızın İstek tetikleyicisi için bitiş noktası URL'sine bir çağrı gönderin. Bu çağrıda, bir şema belirterek daha önce açıklamış olabileceğiniz gönderilen girişi veya yükü belirtin.

Örneğin, Postacı uygulamasını kullanarak, bu örneğe benzer ayarlarla bir POST isteği oluşturabilir ve isteği yapmak için **Gönder'i** seçebilirsiniz.

| İstek yöntemi | URL'si | Gövde | Üst bilgiler |
|----------------|-----|------|---------|
| **POST** | <*uç nokta-URL*> | **Ham** <p>**JSON(uygulama/json)** <p>**Ham** kutuya, isteği göndermek istediğiniz yükü girin. <p>**Not**: Bu ayar **Üstbilgi** değerlerini otomatik olarak yapılandırır. | **Anahtar**: İçerik Türü <br>**Değer**: uygulama/json |
|||||

![Mantık uygulamanızı el ile tetiklemek için istek gönderme](./media/migrate-from-scheduler-to-logic-apps/postman-send-post-request.png)

Aramayı gönderdikten sonra, mantık uygulamanızdan gelen yanıt **Vücut** sekmesindeki **ham** kutunun altında görünür. 

<a name="workflow-run-id"></a>

> [!IMPORTANT]
>
> İşi daha sonra iptal etmek istiyorsanız, **Üstbilgi** sekmesini seçin. Yanıttaki **x-ms-iş akışı-çalıştır-id** üstbilgisi değerini bulun ve kopyalayın. 
>
> ![Yanıt](./media/migrate-from-scheduler-to-logic-apps/postman-response.png)

## <a name="cancel-a-one-time-job"></a>Tek seferlik bir işi iptal etme

Logic Apps'ta, her bir defaya tek seferlik iş tek bir mantık uygulaması çalıştırma örneği olarak yürütülür. Tek seferlik bir işi iptal etmek için, Logic Apps REST API'sinde [İş Akışı Çalıştırmalarını - İptal](https://docs.microsoft.com/rest/api/logic/workflowruns/cancel) Et'i kullanabilirsiniz. Tetikleyiciye bir çağrı gönderdiğinde, [iş akışı çalıştırma kimliğini](#workflow-run-id)sağlayın.

## <a name="schedule-recurring-jobs"></a>Yinelenen işleri zamanlama

1. Azure [portalında,](https://portal.azure.com)Logic App Designer'da boş bir mantık uygulaması oluşturun.

   Temel adımlar için [Quickstart'ı izleyin: İlk mantık uygulamanızı oluşturun.](../logic-apps/quickstart-create-first-logic-app-workflow.md)

1. Arama kutusuna filtreniz olarak "yineleme" girin. Tetikleyiciler listesinden şu tetikleyiciyi seçin: **Yineleme**

   !["Yineleme" tetikleyicisi ekleme](./media/migrate-from-scheduler-to-logic-apps/recurrence-trigger.png)

1. İsterseniz daha gelişmiş bir zamanlama ayarlayın.

   ![Gelişmiş zamanlama](./media/migrate-from-scheduler-to-logic-apps/recurrence-advanced-schedule.png)

   Gelişmiş zamanlama seçenekleri hakkında daha fazla bilgi için Azure [Mantık Uygulamaları ile yinelenen görevler ve iş akışları oluşturun ve çalıştırın.](../connectors/connectors-native-recurrence.md)

1. [Kullanıma hazır yüzlerce](../connectors/apis-list.md)eylem arasından seçim yaparak istediğiniz diğer eylemleri ekleyin. Tetikleyicinin altında **Sonraki adım'ı**seçin. İstediğiniz eylemleri bulun ve seçin.

   Örneğin, bir URL'ye istek gönderen bir HTTP eylemi veya Depolama Kuyrukları, Servis Veri Gönderi kuyrukları veya Hizmet Veri Gönderi konularıyla çalışan eylemler ekleyebilirsiniz:

   ![HTTP eylem](./media/migrate-from-scheduler-to-logic-apps/recurrence-http-action.png)

1. Işiniz bittiğinde, mantık uygulamanızı kaydedin.

   ![Mantıksal uygulamanızı kaydetme](./media/migrate-from-scheduler-to-logic-apps/save-logic-app.png)

## <a name="advanced-setup"></a>Gelişmiş kurulum

İşlerinizi özelleştirmenin diğer yolları şunlardır.

### <a name="retry-policy"></a>Yeniden deneme ilkesi

Bir eylemin aralıklı hatalar olduğunda mantık uygulamanızda yeniden çalıştırmaya çalışma şeklini denetlemek için, örneğin her eylemin ayarlarında [yeniden deneme ilkesini](../logic-apps/logic-apps-exception-handling.md#retry-policies) ayarlayabilirsiniz:

1. Eylemin elips **(...**) menüsünü açın ve **Ayarlar'ı**seçin.

   ![Eylem ayarlarını açma](./media/migrate-from-scheduler-to-logic-apps/action-settings.png)

1. İstediğiniz yeniden deneme ilkesini seçin. Her ilke hakkında daha fazla bilgi için [yeniden deneme ilkeleri ne](../logic-apps/logic-apps-exception-handling.md#retry-policies)bakın.

   ![Yeniden deneme ilkesini seçin](./media/migrate-from-scheduler-to-logic-apps/retry-policy.png)

## <a name="handle-exceptions-and-errors"></a>Özel durumları ve hataları işleme

Azure Zamanlayıcısı'nda, varsayılan eylem çalışmazsa, hata koşulunu gideren alternatif bir eylem çalıştırabilirsiniz. Azure Logic Apps'ta da aynı görevi gerçekleştirebilirsiniz.

1. Mantık Uygulama Tasarımcısı'nda, işlemek istediğiniz eylemin üzerinde, işaretçinizi adımlar arasında okun üzerine taşıyın ve **paralel dal ekle'yi**seçin.

   ![Paralel dal ekleme](./media/migrate-from-scheduler-to-logic-apps/add-parallel-branch.png)

1. Alternatif eylem olarak çalıştırmak istediğiniz eylemi bulun ve seçin.

   ![Paralel eylem ekleme](./media/migrate-from-scheduler-to-logic-apps/add-parallel-action.png)

1. Alternatif eylemde, elipsler (**...**) menüsünü açın ve **'den sonra çalıştır'ı yapılandır'ı**seçin.

   ![Çalışma dan sonra yapılandır](./media/migrate-from-scheduler-to-logic-apps/configure-run-after.png)

1. **Başarılı** özellik için kutuyu temizleyin. Bu özellikleri seçin: **başarısız oldu,** **atlandı**ve **zaman doldu**

   !["Sonra çalıştır" özelliklerini ayarlama](./media/migrate-from-scheduler-to-logic-apps/select-run-after-properties.png)

1. İşiniz bittiğinde **Bitti**'yi seçin.

Özel durum işleme hakkında daha fazla bilgi edinmek için [bkz.](../logic-apps/logic-apps-exception-handling.md#control-run-after-behavior)

## <a name="faq"></a>SSS

<a name="retire-date"></a>

**S**: Azure Zamanlayıcısı ne zaman emekli oluyor? <br>
**C**: Azure Zamanlayıcısı'nın 31 Aralık 2019'da tamamen emekli olması planlanmaktadır. Bu tarihten önce atılacak önemli adımlar ve ayrıntılı bir zaman çizelgesi [için](https://azure.microsoft.com/updates/extending-retirement-date-of-scheduler/)bkz. Genel güncelleştirmeler için [Azure güncelleştirmeleri - Zamanlayıcı '](https://azure.microsoft.com/updates/?product=scheduler)ya bakın.

**S**: Hizmet emekli olduktan sonra iş tahsilatlarıma ve işlerime ne olur? <br>
**C**: Tüm Scheduler iş koleksiyonları ve işleri çalışma durur ve sistemden silinir.

**S**: Zamanlayıcı işlerimi Logic Apps'a geçirmeden önce yedeklemem veya başka görevleri yerine getirmem gerekiyor mu? <br>
**C**: En iyi uygulama olarak, her zaman işinizi yedek. Zamanlayıcı işlerinizi silmeden veya devre dışı bırakmadan önce oluşturduğunuz mantık uygulamalarının beklendiği gibi çalıştığını kontrol edin.

**S**: İşlerimi Zamanlayıcı'dan Mantık Uygulamaları'na geçirmeme yardımcı olabilecek bir araç var mı? <br>
**C**: Her Zamanlayıcı işi benzersizdir, bu nedenle her şeye uyan bir araç yoktur. Ancak, gereksinimlerinize bağlı olarak, [Azure Zamanlayıcısı işlerini Azure Logic Apps'a geçirmek için bu komut dosyasını düzenleyebilirsiniz.](https://github.com/Azure/logicapps/tree/master/scripts/scheduler-migration)

**S**: Zamanlayıcı işlerimi geçirmek için nereden destek alabilirim? <br>
**C**: Destek almanın bazı yolları şunlardır:

**Azure portalında**

Azure aboneliğinizde ücretli bir destek planı varsa, Azure portalında teknik destek isteği oluşturabilirsiniz. Aksi takdirde, farklı bir destek seçeneği seçebilirsiniz.

1. Azure [portalı](https://portal.azure.com) ana menüsünde **Yardım + destek'i**seçin.

1. **Destek** menüsünden **Yeni destek isteği'ni**seçin. İsteğiniz hakkında şu bilgileri sağlayın:

   | Özellik | Değer |
   |---------|-------|
   | **Sorun türü** | **Teknik** |
   | **Abonelik** | <*sizin-Azure aboneliğiniz*> |
   | **Hizmet** | **İzleme & Yönetimi**altında, **Zamanlayıcı'yı**seçin. **Zamanlayıcı'yı**bulamıyorsanız, önce **Tüm hizmetleri** seçin. |
   ||| 

1. İstediğiniz destek seçeneğini seçin. Ücretli bir destek planınız varsa **İleri'yi**seçin.

**Topluluk**

* [Azure Mantık Uygulamaları forumu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps)
* [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-scheduler)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Logic Apps ile düzenli olarak çalışan görevler ve iş akışları oluşturun](../connectors/connectors-native-recurrence.md)