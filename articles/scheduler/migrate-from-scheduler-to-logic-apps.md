---
title: Azure Scheduler 'dan Azure Logic Apps 'e geçiş
description: Azure Scheduler 'da devre dışı bırakılan işleri Azure Logic Apps ile nasıl değiştireceğiniz hakkında bilgi edinin
services: scheduler
ms.service: scheduler
ms.suite: infrastructure-services
author: derek1ee
ms.author: deli
ms.reviewer: klam, LADocs
ms.topic: article
ms.date: 09/23/2019
ms.openlocfilehash: 6b80cbd16ac78f7f347bef9ab8e22c4d67d31058
ms.sourcegitcommit: 29880cf2e4ba9e441f7334c67c7e6a994df21cfe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/26/2019
ms.locfileid: "71301037"
---
# <a name="migrate-azure-scheduler-jobs-to-azure-logic-apps"></a>Azure Scheduler işlerini Azure Logic Apps geçirin

> [!IMPORTANT]
> [Azure Logic Apps](../logic-apps/logic-apps-overview.md) , [devre dışı bırakılmakta](#retire-date)olan Azure Scheduler 'ı değiştiriyor. Zamanlayıcı 'da ayarladığınız işlerle çalışmaya devam etmek için lütfen bu makaleyi izleyerek en kısa sürede Azure Logic Apps taşıyın.

Bu makalede, Azure Scheduler yerine Azure Logic Apps ile otomatik iş akışları oluşturarak tek seferlik ve yinelenen işleri nasıl planlayamazsınız gösterilmektedir. Logic Apps ile zamanlanmış işler oluşturduğunuzda, bu avantajları elde edersiniz:

* Bir görsel tasarımcı ve Azure Blob depolama, Azure Service Bus, Office 365 Outlook ve SAP gibi yüzlerce hizmetten [kullanıma hazırlama bağlayıcıları](../connectors/apis-list.md) kullanarak işinizi oluşturun.

* Zamanlanan her iş akışını birinci sınıf bir Azure kaynağı olarak yönetin. Her mantıksal uygulama ayrı bir Azure kaynağı olduğundan, bir *iş koleksiyonu* kavramı konusunda endişelenmeniz gerekmez.

* Tek bir mantıksal uygulama kullanarak birden çok tek seferlik iş çalıştırın.

* Saat dilimlerini destekleyen ve otomatik olarak yaz tasarrufu süresi (DST) olarak ayarlanan zamanlamalar ayarlayın.

Daha fazla bilgi edinmek için bkz. [Azure Logic Apps nedir?](../logic-apps/logic-apps-overview.md) veya bu hızlı başlangıçta ilk mantıksal uygulamanızı oluşturmayı deneyin: [İlk mantıksal uygulamanızı oluşturun](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/).

* HTTP istekleri göndererek mantıksal uygulamanızı tetiklemek için [Postman masaüstü uygulaması](https://www.getpostman.com/apps)gibi bir araç kullanın.

## <a name="schedule-one-time-jobs"></a>Tek seferlik işleri zamanlama

Tek bir mantıksal uygulama oluşturarak birden çok tek seferlik iş çalıştırabilirsiniz. 

1. [Azure Portal](https://portal.azure.com)mantıksal uygulama tasarımcısında boş bir mantıksal uygulama oluşturun. 

   Temel adımlar için hızlı başlangıç ' [ı izleyin: İlk mantıksal uygulamanızı](../logic-apps/quickstart-create-first-logic-app-workflow.md)oluşturun.

1. Arama kutusuna filtreniz olarak "http isteği" yazın. Tetikleyiciler listesinde, bu tetikleyiciyi seçin: **Bir HTTP isteği alındığında** 

   !["Istek" tetikleyicisi Ekle](./media/migrate-from-scheduler-to-logic-apps/request-trigger.png)

1. Istek tetikleyicisi için isteğe bağlı olarak, mantıksal uygulama Tasarımcısı 'nın gelen istekten gelen girişlerin yapısını anlamasına yardımcı olan bir JSON şeması sağlayabilirsiniz ve daha sonra iş akışınızda daha sonra seçim yapmanız için çıktıları daha kolay hale getirir.

   Bir şema belirtmek için, şemayı **Istek GÖVDESI JSON şemasına** girin, örneğin: 

   ![İstek şeması](./media/migrate-from-scheduler-to-logic-apps/request-schema.png)

   Bir şemanız yoksa ancak JSON biçiminde bir örnek yüküyle karşılaşırsanız, bu yükün bir şemasını oluşturabilirsiniz.

   1. Istek tetikleyicisinde, **şema oluşturmak için örnek yük kullan**' ı seçin.

   1. **Örnek BIR JSON yükü girin veya yapıştırın**, örnek yükünüzü sağlayın ve sonra **bitti**' yi seçin, örneğin:

      ![Örnek yük](./media/migrate-from-scheduler-to-logic-apps/sample-payload.png)

1. Tetikleyici altında, **İleri adım**' ı seçin. 

1. Arama kutusuna filtreniz olarak "geciktir Until" yazın. Eylemler listesi altında şu eylemi seçin: **Gecikme süresi**

   Bu eylem, mantıksal uygulama iş akışınızı belirtilen bir tarih ve saate kadar duraklatır.

   !["Gecikme süresi" eylemini Ekle](./media/migrate-from-scheduler-to-logic-apps/delay-until.png)

1. Mantıksal uygulamanın iş akışını başlatmak istediğiniz zaman zaman damgasını girin. 

   **Zaman damgası** kutusunun içine tıkladığınızda, isteğe bağlı olarak tetikleyiciden bir çıktı seçebilmeniz için dinamik içerik listesi görüntülenir.

   !["Gecikme süresi" ayrıntılarını sağlayın](./media/migrate-from-scheduler-to-logic-apps/delay-until-details.png)

1. [Yüzlerce kullanıma yönelik kullanılabilir bağlayıcıdan](../connectors/apis-list.md)seçim yaparak çalıştırmak istediğiniz diğer eylemleri ekleyin. 

   Örneğin, bir URL 'ye istek gönderen bir HTTP eylemi veya depolama kuyrukları, Service Bus kuyrukları veya Service Bus konuları ile çalışan eylemler ekleyebilirsiniz: 

   ![HTTP eylemi](./media/migrate-from-scheduler-to-logic-apps/request-http-action.png)

1. İşiniz bittiğinde mantıksal uygulamanızı kaydedin.

   ![Mantıksal uygulamanızı kaydetme](./media/migrate-from-scheduler-to-logic-apps/save-logic-app.png)

   Mantıksal uygulamanızı ilk kez kaydettiğinizde, mantıksal uygulamanızın Istek tetikleyicisi için uç nokta URL 'SI **http post URL 'si** kutusunda görünür. 
   Mantıksal uygulamanızı çağırmak ve işlem için mantıksal uygulamanıza giriş göndermek istediğinizde, bu URL 'YI çağrı hedefi olarak kullanın.

   ![Istek tetikleme uç nokta URL 'sini Kaydet](./media/migrate-from-scheduler-to-logic-apps/request-endpoint-url.png)

1. Daha sonra mantıksal uygulamanızı tetikleyen el ile bir istek gönderebilmeniz için bu uç nokta URL 'sini kopyalayıp kaydedin. 

## <a name="start-a-one-time-job"></a>Tek seferlik bir iş başlatın

Tek seferlik bir işi el ile çalıştırmak veya tetiklemek için, mantıksal uygulamanızın Istek tetikleyicisi için uç nokta URL 'sine bir çağrı gönderin. Bu çağrıda, daha önce bir şema belirterek daha önce açıklandığı gibi, göndermek için giriş veya yük belirtin. 

Örneğin, Postman uygulamasını kullanarak bu örneğe benzer ayarlarla bir POST isteği oluşturabilir ve sonra isteği yapmak için **Gönder** ' i seçebilirsiniz.

| İstek metodu | URL | Body | Üst bilgiler |
|----------------|-----|------|---------|
| **POST** | <*uç nokta-URL*> | **Madde** <p>**JSON (uygulama/JSON)** <p>**Ham** kutusuna, istekte göndermek istediğiniz yükü girin. <p>**Not**: Bu ayar **üst bilgi** değerlerini otomatik olarak yapılandırır. | **Anahtar**: Content-Type <br>**Değer**: uygulama/JSON |
|||||

![Mantıksal uygulamanızı el ile tetikleme isteği gönderme](./media/migrate-from-scheduler-to-logic-apps/postman-send-post-request.png)

Çağrıyı gönderdikten sonra, mantıksal uygulamanızdan gelen yanıt **gövde** sekmesindeki **Ham** kutusunda görünür. 

<a name="workflow-run-id"></a>

> [!IMPORTANT]
>
> İşi daha sonra iptal etmek istiyorsanız **üstbilgiler** sekmesini seçin. Yanıtta **x-MS-Workflow-Run-ID** üst bilgi değerini bulup kopyalayın. 
>
> ![Yanıt](./media/migrate-from-scheduler-to-logic-apps/postman-response.png)

## <a name="cancel-a-one-time-job"></a>Tek seferlik bir işi iptal etme

Logic Apps, her bir defalık iş tek bir mantıksal uygulama çalıştırma örneği olarak yürütülür. Tek seferlik bir işi iptal etmek için, Logic Apps REST API [Iş akışı çalıştırmalarını kullanabilirsiniz-iptal](https://docs.microsoft.com/rest/api/logic/workflowruns/cancel) ' i kullanabilirsiniz. Tetikleyiciye bir çağrı gönderdiğinizde, [iş akışı çalıştırma kimliğini](#workflow-run-id)sağlayın.

## <a name="schedule-recurring-jobs"></a>Yinelenen işleri zamanlama

1. [Azure Portal](https://portal.azure.com)mantıksal uygulama tasarımcısında boş bir mantıksal uygulama oluşturun. 

   Temel adımlar için hızlı başlangıç ' [ı izleyin: İlk mantıksal uygulamanızı](../logic-apps/quickstart-create-first-logic-app-workflow.md)oluşturun.

1. Arama kutusuna filtreniz olarak "yinelenme" yazın. Tetikleyiciler listesinde, bu tetikleyiciyi seçin: **Yineleme** 

   !["Yinelenme" tetikleyicisi Ekle](./media/migrate-from-scheduler-to-logic-apps/recurrence-trigger.png)

1. İsterseniz daha gelişmiş bir zamanlama ayarlayın.

   ![Gelişmiş zamanlama](./media/migrate-from-scheduler-to-logic-apps/recurrence-advanced-schedule.png)

   Gelişmiş zamanlama seçenekleri hakkında daha fazla bilgi için bkz. [Azure Logic Apps ile yinelenen görevler ve iş akışları oluşturma ve çalıştırma](../connectors/connectors-native-recurrence.md).

1. [Yüzlerce kullanıma kullanım](../connectors/apis-list.md)arasından seçerek istediğiniz diğer eylemleri ekleyin. Tetikleyici altında, **İleri adım**' ı seçin. İstediğiniz eylemleri bulun ve seçin.

   Örneğin, bir URL 'ye istek gönderen bir HTTP eylemi veya depolama kuyrukları, Service Bus kuyrukları veya Service Bus konuları ile çalışan eylemler ekleyebilirsiniz: 

   ![HTTP eylemi](./media/migrate-from-scheduler-to-logic-apps/recurrence-http-action.png)

1. İşiniz bittiğinde mantıksal uygulamanızı kaydedin.

   ![Mantıksal uygulamanızı kaydetme](./media/migrate-from-scheduler-to-logic-apps/save-logic-app.png)

## <a name="advanced-setup"></a>Gelişmiş kurulum

İşlerinizi özelleştirmek için kullanabileceğiniz diğer yollar şunlardır.

### <a name="retry-policy"></a>Yeniden deneme ilkesi

Bir eylemin işlem sırasında mantıksal uygulamanızda yeniden çalıştırmayı deneme biçimini denetlemek için, her bir eylemin ayarlarındaki [yeniden deneme ilkesini](../logic-apps/logic-apps-exception-handling.md#retry-policies) ayarlayabilirsiniz, örneğin:

1. Eylemin üç nokta ( **...** ) menüsünü açın ve **Ayarlar**' ı seçin.

   ![Eylem ayarlarını aç](./media/migrate-from-scheduler-to-logic-apps/action-settings.png)

1. İstediğiniz yeniden deneme ilkesini seçin. Her ilke hakkında daha fazla bilgi için bkz. [yeniden deneme ilkeleri](../logic-apps/logic-apps-exception-handling.md#retry-policies).

   ![Yeniden deneme ilkesini seçin](./media/migrate-from-scheduler-to-logic-apps/retry-policy.png)

## <a name="handle-exceptions-and-errors"></a>Özel durumları ve hataları işle

Azure Scheduler 'da, varsayılan eylem çalışamazsa, hata koşulunu ele alan bir alternatif eylem çalıştırabilirsiniz. Azure Logic Apps Ayrıca aynı görevi gerçekleştirebilirsiniz.

1. Mantıksal uygulama Tasarımcısı ' nda, işlemek istediğiniz eylemin üstünde, işaretçinizi adımlar arasındaki oka taşıyın ve **paralel dal Ekle**' yi seçin. 

   ![Paralel dal Ekle](./media/migrate-from-scheduler-to-logic-apps/add-parallel-branch.png)

1. Alternatif eylem yerine çalıştırmak istediğiniz eylemi bulun ve seçin.

   ![Paralel eylem Ekle](./media/migrate-from-scheduler-to-logic-apps/add-parallel-action.png)

1. Alternatif eylemde, üç nokta ( **...** ) menüsünü açın ve **sonra Çalıştır 'ı Yapılandır**' ı seçin.

   ![Sonra Çalıştır Yapılandırma](./media/migrate-from-scheduler-to-logic-apps/configure-run-after.png)

1. **Başarılı** özelliğinin kutusunun işaretini kaldırın. Şu özellikleri seçin: **başarısız oldu**, **atlandı**ve **zaman aşımına uğradı**

   !["Sonra Çalıştır" özelliklerini ayarla](./media/migrate-from-scheduler-to-logic-apps/select-run-after-properties.png)

1. İşiniz bittiğinde **Bitti**'yi seçin.

Özel durum işleme hakkında daha fazla bilgi edinmek için bkz. [işleme hataları ve özel durumlar-RunAfter özelliği](../logic-apps/logic-apps-exception-handling.md#catch-and-handle-failures-with-the-runafter-property).

## <a name="faq"></a>SSS

<a name="retire-date"></a>

**S**: Azure Scheduler 'ı devre dışı bırakma ne zaman? <br>
Y: Azure Scheduler, 31 Aralık 2019 tarihinde tamamen devre dışı bırakılacak şekilde zamanlandı. Bu tarihten ve ayrıntılı bir zaman çizelgesinden önce gerçekleştirilecek önemli adımlar için, bkz. [Scheduler için kullanımdan kaldırma tarihini 31 aralık 2019 ' e genişletme](https://azure.microsoft.com/en-us/updates/extending-retirement-date-of-scheduler/). Genel güncelleştirmeler için bkz. [Azure Updates-Scheduler](https://azure.microsoft.com/updates/?product=scheduler).

**S**: Hizmet yeniden kurulduktan sonra iş koleksiyonlarıma ve işlerime ne olur? <br>
Y: Tüm zamanlayıcı iş koleksiyonları ve işleri çalışmayı durdurur ve sistemden silinir.

**S**: Zamanlayıcı işlerim Logic Apps geçirmeden önce diğer görevleri yedeklemem veya gerçekleştirmem gerekir mi? <br>
Y: En iyi uygulama olarak, her zaman çalışmanızı yedekleyin. Zamanlayıcı İşlerinizi silmeden veya devre dışı bırakmadan önce oluşturduğunuz mantıksal uygulamaların beklendiği gibi çalıştığını denetleyin. 

**S**: Uygulamalarımı Scheduler 'dan Logic Apps geçişe geçirmeye yardımcı olabilecek bir araç var mı? <br>
Y: Her Scheduler işi benzersizdir, bu nedenle tek boyutlu bir-All aracı yok. Ancak, gereksinimlerinize göre değiştirebileceğiniz çeşitli betikler vardır. Betik kullanılabilirliği için daha sonra tekrar denetleyin.

**S**: Zamanlayıcı işlerinizin geçirilmesi için nereden destek alabilirim? <br>
Y: Destek almanın bazı yolları aşağıda verilmiştir: 

**Azure portal**

Azure aboneliğinizin ücretli bir destek planı varsa Azure portal bir teknik destek isteği oluşturabilirsiniz. Aksi takdirde, farklı bir destek seçeneği belirleyebilirsiniz.

1. [Azure Portal](https://portal.azure.com) ana menüsünde **Yardım + Destek**' i seçin.

1. **Destek** menüsünden **Yeni destek isteği**' ni seçin. İsteğiniz için aşağıdaki bilgileri sağlayın:

   | Özellik | Value |
   |---------|-------|
   | **Sorun türü** | **Teknik** |
   | **Abonelik** | <*Azure aboneliğiniz*> |
   | **Hizmet** | **& yönetimi izleme**altında **Zamanlayıcı**' yı seçin. **Zamanlayıcı 'yı**bulamıyorsanız, önce **tüm hizmetler** ' i seçin. |
   ||| 

1. İstediğiniz destek seçeneğini belirleyin. Ücretli bir destek planınız varsa **İleri**' yi seçin.

**Topluluk**

* [Azure Logic Apps Forumu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps)
* [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-scheduler)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Logic Apps ile düzenli olarak çalışan görevler ve iş akışları oluşturma](../connectors/connectors-native-recurrence.md)
* [Öğretici: Zamanlama tabanlı mantıksal uygulama ile trafiği denetleme](../logic-apps/tutorial-build-schedule-recurring-logic-app-workflow.md)
