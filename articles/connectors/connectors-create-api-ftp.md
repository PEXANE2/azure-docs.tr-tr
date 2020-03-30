---
title: FTP sunucusuna bağlanın
description: Azure Mantık Uygulamaları'nı kullanarak FTP sunucusunda dosya oluşturan, izleyen ve yöneten görevleri ve iş akışlarını otomatikleştirin
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: conceptual
ms.date: 12/15/2019
tags: connectors
ms.openlocfilehash: 94ca609d9dc070e6e2b4dc878ecd8dfaf9331ede
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77648181"
---
# <a name="create-monitor-and-manage-ftp-files-by-using-azure-logic-apps"></a>Azure Logic Apps'ı kullanarak FTP dosyalarını oluşturma, izleme ve yönetme

Azure Logic Apps ve FTP bağlayıcısı ile, örneğin diğer eylemlerle birlikte bir FTP sunucusunda hesabınız üzerinden dosya oluşturan, izleyen, gönderen ve alan otomatik görevler ve iş akışları oluşturabilirsiniz:

* Dosyalar eklendiğinde veya değiştirildiğinde izleyin.
* Dosyaları alın, oluşturun, kopyalayın, güncelleyin, listeleyin ve silin.
* Dosya içeriği ve meta veriler alın.
* Arşivleri klasörlere ayıklayın.

FTP sunucunuzdan yanıt alan ve çıktıyı diğer eylemler için kullanılabilir hale getiren tetikleyiciler kullanabilirsiniz. FTP sunucunuzdaki dosyaları yönetmek için mantık uygulamalarınızdaki çalıştırma eylemlerini kullanabilirsiniz. FTP eylemlerinden çıkan çıktıyı başka eylemlerin de kullanmasını da sağlayabilirsiniz. Örneğin, FTP sunucunuzdan düzenli olarak dosya alırsanız, Office 365 Outlook bağlayıcısını veya Outlook.com bağlayıcısını kullanarak bu dosyalar ve içerikleri hakkında e-posta gönderebilirsiniz. Mantıksal uygulamalarda yeniyseniz, [Azure Mantıksal Uygulamaları nedir'yi](../logic-apps/logic-apps-overview.md)inceleyin.

## <a name="limitations"></a>Sınırlamalar

* FTP konektörü yalnızca SSL (FTPS) üzerinden açık FTP'yi destekler ve örtülü FTPS ile uyumlu değildir.

* Varsayılan olarak, FTP eylemleri *50 MB veya daha küçük*dosyaları okuyabilir veya yazabilir. 50 MB'dan büyük dosyaları işlemek için FTP eylemleri [ileti ödentiyi](../logic-apps/logic-apps-handle-large-messages.md)destekler. **Dosya içeriği al** eylemi, örtülü olarak yığınlama kullanır.

* FTP tetikleyicileri ötme desteklemez. Dosya içeriği isteğinde bulunurken, yalnızca 50 MB veya daha küçük dosyaları seçin. 50 MB'dan büyük dosyaları almak için aşağıdaki deseni izleyin:

  * **Dosya eklendiğinde veya değiştirildiğinde (yalnızca özellikler)** gibi dosya özelliklerini döndüren bir FTP tetikleyicisi kullanın.

  * Tüm **dosyayı** okuyan ve dolaylı olarak yığınlama kullanan FTP Get dosya içeriği eylemi ile tetikleyiciyi izleyin.

* Şirket içinde FTP sunucunuz varsa, şirket içi veri ağ geçidi kullanmadan şirket içi veri kaynaklarına erişmenize izin veren bir [tümleştirme hizmeti ortamı (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md) oluşturmayı veya [Azure App Service Karma bağlantılarını](../app-service/app-service-hybrid-connections.md)kullanmayı düşünün.

## <a name="how-ftp-triggers-work"></a>FTP nasıl çalışır?

FTP, FTP dosya sistemini yoklayarak ve son anketten bu yana değiştirilen herhangi bir dosyayı arayarak çalışmayı tetikler. Bazı araçlar, dosyalar değiştiğinde zaman damgasını korumanızı sağlar. Bu gibi durumlarda, tetikleyicinizin çalışabilmesi için bu özelliği devre dışı beşinizgerekir. Bazı yaygın ayarlar şunlardır:

| SFTP istemcisi | Eylem |
|-------------|--------|
| Winscp | **Seçeneklere** > Git**Tercihleri** > **Aktar** > **Değiştir** > Zaman**damgası** > Devre**Dışı** Bırak |
| Filezilla | **AktarAn** > dosyaların > **Disable** devre dışı**bırak'ın zaman damgalarını kaydet** |
|||

Tetikleyici yeni bir dosya bulduğunda, tetikleyici yeni dosyanın tamamlanıp kısmen yazılmadığını denetler. Örneğin, tetikleyici dosya sunucusunu denetlediğinde bir dosyanın devam eden değişiklikleri olabilir. Kısmen yazılmış bir dosyayı döndürmeyi önlemek için, tetikleyici son değişiklikler eki olan ancak bu dosyayı hemen döndürmeyen dosyanın zaman damgasını not eder. Tetikleyici, yalnızca sunucuyu yeniden yoklarken dosyayı döndürür. Bazen, bu davranış, tetikleyicinin yoklama aralığının iki katına kadar olan bir gecikmeye neden olabilir.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/).

* FTP ana bilgisayar sunucu adresiniz ve hesap kimlik bilgileriniz

  FTP konektörü, FTP sunucunuza internetten erişilebis ve *pasif* modda çalışacak şekilde ayarlanabilir olmasını gerektirir. Kimlik bilgileriniz, mantık uygulamanızın bir bağlantı oluşturmasına ve FTP hesabınıza erişmesine izin vermez.

* [Mantık uygulamaları oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md) hakkında temel bilgiler

* FTP hesabınıza erişmek istediğiniz mantık uygulaması. FTP tetikleyicisi ile başlamak için [boş bir mantık uygulaması oluşturun.](../logic-apps/quickstart-create-first-logic-app-workflow.md) FTP eylemi kullanmak için, mantık uygulamanızı başka bir tetikleyiciyle başlatın, **örneğin, Yineleme** tetikleyicisi.

## <a name="connect-to-ftp"></a>FTP'ye bağlan

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. [Azure portalında](https://portal.azure.com)oturum açın ve Mantık Uygulama Tasarımcısı'nda mantık uygulamanızı açın.

1. Boş mantık uygulamaları için, arama `ftp` kutusuna filtreniz olarak girin. **Tetikleyiciler** listesinden istediğiniz tetikleyiciyi seçin.

   -veya-

   Varolan mantık uygulamaları için, eylem eklemek istediğiniz son adımaltında **Yeni adım'ı**seçin ve ardından **eylem ekle'yi**seçin. Arama kutusuna filtreniz olarak girin. `ftp` **Eylemler** listesinden, istediğiniz eylemi seçin.

   Adımlar arasında eylem eklemek için işaretçinizin üzerine adımların arasında ilerleyin. Görünen artı işaretini (**+**) seçin ve ardından eylem **ekle'yi**seçin.

1. Bağlantı bilgilerinizi sağlayın ve **Oluştur'u**seçin.

1. Seçtiğiniz tetikleyici veya eylem için bilgileri sağlayın ve mantık uygulamanızın iş akışını oluşturmaya devam edin.

## <a name="examples"></a>Örnekler

<a name="file-added-modified"></a>

### <a name="add-ftp-trigger"></a>FTP tetikleyicisi ekleme

**Bir dosya eklendiğinde veya değiştirildiğinde (yalnızca özellikler)** tetikleyici, bir FTP sunucusuna bir dosyanın ekildiğini veya değiştirildiğini algıladığında bir mantık uygulaması iş akışı başlatır. Örneğin, dosyanın içeriğini denetleyen ve içeriğin belirli bir koşulla karşılanıp karşılamadığını temel alan bu içeriği alıp almayacağına karar veren bir koşul ekleyebilirsiniz. Son olarak, dosyanın içeriğini alan bir eylem ekleyebilir ve bu içeriği SFTP sunucusunda farklı bir klasöre koyabilirsiniz.

Örneğin, müşteri siparişlerini açıklayan yeni dosyalar için bir FTP klasörünü izlemek için bu tetikleyiciyi kullanabilirsiniz. Daha sonra, bu yeni dosyanın özelliklerini almak için **dosya meta verilerini al** gibi bir FTP eylemini kullanabilir ve daha sonra dosya içeriğini daha fazla işlem için dosya içeriğini almak ve bu siparişi bir sipariş veritabanında depolamak için dosya içeriğini **al'ı** kullanabilirsiniz.

Burada, **ne zaman dosya eklendiğinde veya değiştirildiğinde (yalnızca özellikler)** tetikleyicisinin nasıl kullanılacağını gösteren bir örnek verilmiştir.

1. [Azure portalında](https://portal.azure.com)oturum açın ve mantık uygulamanızı zaten açık değilse Mantık Uygulama Tasarımcısı'nda açın.

1. Boş mantık uygulamaları için, arama `ftp` kutusuna filtreniz olarak girin. Tetikleyiciler listesinin altında şu tetikleyiciyi seçin: **Dosyada dosya eklendiğinde veya değiştirildiğinde (yalnızca özellikler)**

   ![FTP tetikleyicisini bulun ve seçin](./media/connectors-create-api-ftp/select-ftp-trigger-logic-app.png)

1. Bağlantınız için gerekli ayrıntıları sağlayın ve ardından **Oluştur'u**seçin.

   Varsayılan olarak, bu bağlayıcı dosyaları metin biçiminde aktarMaktadır. Dosyaları ikili biçimde aktarmak için, örneğin, kodlamanın nerede ve ne zaman kullanıldığı, **İkili Aktarım'ı**seçin.

   ![FTP sunucusuna bağlantı oluşturma](./media/connectors-create-api-ftp/create-ftp-connection-trigger.png)

1. **Klasör** kutusunda, bir listenin görünmesi için klasör simgesini seçin. Yeni veya düzenlenmiş dosyalar için izlemek istediğiniz klasörü bulmak için, doğru açı ok (),**>** bu klasöre göz atın ve sonra klasörü seçin seçin.

   ![İzlenecek klasörü bulma ve seçme](./media/connectors-create-api-ftp/select-folder-ftp-trigger.png)

   Seçtiğiniz klasör **Klasör** kutusunda görünür.

   ![Seçili klasör "Klasör" özelliğinde görünür](./media/connectors-create-api-ftp/selected-folder-ftp-trigger.png)

1. Mantıksal uygulamanızı kaydedin. Tasarımcı araç çubuğunda **Kaydet'i**seçin.

Artık mantık uygulamanızın bir tetikleyicisi olduğuna göre, mantık uygulamanız yeni veya düzenlenmiş bir dosya bulduğunda çalıştırmak istediğiniz eylemleri ekleyin. Bu örnekte, yeni veya güncelleştirilmiş içeriği alan bir FTP eylemi ekleyebilirsiniz.

<a name="get-content"></a>

### <a name="add-ftp-action"></a>FTP eylemi ekleme

Dosya meta veri sa'nı **al,** FTP sunucunuzdaki bir dosyanın özelliklerini alır ve Dosya içeriği ni **al,** dosya içeriğini FTP sunucunuzdaki bu dosyayla ilgili bilgilere göre alır. Örneğin, bu dosya eklendikten veya düzenlendikten sonra dosyanın içeriğini almak için önceki örnekteki tetikleyiciyi ve bu eylemleri ekleyebilirsiniz.

1. Tetikleyici veya diğer eylemler altında **Yeni adım'ı**seçin.

1. Arama kutusuna filtreniz olarak girin. `ftp` Eylemler listesinin altında, şu eylemi seçin: **Dosya meta verilerini alın**

   !["Dosya meta verilerini al" eylemini seçin](./media/connectors-create-api-ftp/select-get-file-metadata-ftp-action.png)

1. FTP sunucunuz ve hesabınızla zaten bir bağlantınız varsa, bir sonraki adıma geçin. Aksi takdirde, bu bağlantı için gerekli ayrıntıları sağlayın ve sonra **Oluştur'u**seçin.

   ![FTP sunucu bağlantısı oluşturma](./media/connectors-create-api-ftp/create-ftp-connection-action.png)

1. Dosya **meta veri seylemi görüntüledikten** sonra, dinamik içerik listesinin görünmesi için **Dosya** kutusunun içini tıklatın. Artık önceki adımlardaki çıktılar için özellikleri seçebilirsiniz. Dinamik içerik listesinde, **Dosya meta verilerini al**altında, dosyanın eklendiği veya güncelleştirildiği koleksiyona başvuran **Dosyalar Kimliği Özelliği'ni** seçin.

   !["Kimlik Dosyaları Listesi" özelliğini bulun ve seçin](./media/connectors-create-api-ftp/select-list-of-files-id-output.png)

   **Dosya Kimliği Listesi** özelliği artık **Dosya** kutusunda görünür.

   ![Seçili "Dosya Listesi Kimliği" özelliği](./media/connectors-create-api-ftp/selected-list-file-ids-ftp-action.png)

1. Şimdi bu FTP eylemini ekleyin: **Dosya içeriğini alın**

   !["Dosya içeriğini edinin" eylemini bulun ve seçin](./media/connectors-create-api-ftp/select-get-file-content-ftp-action.png)

1. Dosya **içeriği eylemini al** göründükten sonra, dinamik içerik listesinin görünmesi için **Dosya** kutusunun içine tıklayın. Artık önceki adımlardaki çıktılar için özellikleri seçebilirsiniz. Dinamik içerik listesinde, **Dosya meta verilerini al**altında, eklenen veya güncelleştirilen dosyaya başvurulan **Id** özelliğini seçin.

   !["Id" özelliğini bulun ve seçin](./media/connectors-create-api-ftp/get-file-content-id-output.png)

   **Kimlik** özelliği artık **Dosya** kutusunda görünür.

   ![Seçili "Id" özelliği](./media/connectors-create-api-ftp/selected-get-file-content-id-ftp-action.png)

1. Mantıksal uygulamanızı kaydedin.

## <a name="test-your-logic-app"></a>Mantık uygulamanızı test edin

İş akışınızın beklediğiniz içeriği döndürür dekontrol etmek için, yüklenen veya güncellenen dosyadaki içeriği size gönderen başka bir eylem ekleyin.

1. Dosya **içeriği al** eyleminin altında, dosyanın içeriğini gönderebilecek bir eylem ekleyin. Bu örnek, Office 365 Outlook için **e-posta gönder** eylemini ekler.

   ![E-posta göndermek için eylem ekleme](./media/connectors-create-api-ftp/select-send-email-action.png)

1. Eylem göründükten sonra, bilgileri sağlayın ve sınamak istediğiniz özellikleri ekleyin. Örneğin, dosya **içeriği** al bölümünde daha fazlasını **gör'ü** seçtikten sonra dinamik içerik listesinde görünen **Dosya içeriği** özelliğini ekleyin.

   ![E-posta eylemi hakkında bilgi sağlama](./media/connectors-create-api-ftp/selected-send-email-action.png)

1. Mantıksal uygulamanızı kaydedin. Mantık uygulamasını çalıştırmak ve tetiklemek için, araç çubuğunda **Çalıştır'ı**seçin ve ardından mantık uygulamanızın artık izlediği FTP klasörüne bir dosya ekleyin.

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Tetikleyiciler, eylemler ve konektörün Swagger dosyasında açıklandığı gibi sınırlar gibi bu bağlayıcı hakkında daha fazla teknik ayrıntı için [bağlayıcının başvuru sayfasına](https://docs.microsoft.com/connectors/ftpconnector/)bakın.

> [!NOTE]
> [Bir entegrasyon hizmeti ortamındaki (İmKB)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md)mantık uygulamaları için, bu bağlayıcının İmKB etiketli sürümü bunun yerine [İmKB ileti sınırlarını](../logic-apps/logic-apps-limits-and-config.md#message-size-limits) kullanır.

## <a name="next-steps"></a>Sonraki adımlar

* Diğer [Logic Apps bağlayıcıları](../connectors/apis-list.md) hakkında bilgi edinin
