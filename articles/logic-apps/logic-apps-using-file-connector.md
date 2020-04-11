---
title: Şirket içinde dosya sistemlerine bağlanma
description: Azure Logic Apps'taki şirket içi veri ağ geçidi aracılığıyla Dosya Sistemi bağlayıcısı ile şirket içi dosya sistemlerine bağlanan görevleri ve iş akışlarını otomatikleştirin
services: logic-apps
ms.suite: integration
author: derek1ee
ms.author: deli
ms.reviewer: klam, estfan, logicappspm
ms.topic: article
ms.date: 01/13/2019
ms.openlocfilehash: ab17137f162b893b54942d870b07a36f87d1b71d
ms.sourcegitcommit: fb23286d4769442631079c7ed5da1ed14afdd5fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/10/2020
ms.locfileid: "81115082"
---
# <a name="connect-to-on-premises-file-systems-with-azure-logic-apps"></a>Azure Logic Apps ile şirket içi dosya sistemlerine bağlanma

Azure Logic Apps ve Dosya Sistemi bağlayıcısı ile, örneğin şirket içi dosya paylaşımında dosya oluşturan ve yöneten otomatik görevler ve iş akışları oluşturabilirsiniz:

- Dosyaları oluşturun, alın, ekleyin, güncelleyin ve silin.
- Klasörlerdeki veya kök klasörlerdeki dosyaları listeleyin.
- Dosya içeriği ve meta veriler alın.

Bu makale, bu örnek senaryoda açıklandığı gibi şirket içi bir dosya sistemine nasıl bağlanabileceğinizi gösterir: Dropbox'a yüklenen bir dosyayı dosya paylaşımına kopyalayın ve ardından bir e-posta gönderin. Şirket içi sistemlere güvenli bir şekilde bağlanmak ve erişmek için, mantık uygulamaları [şirket içi veri ağ geçidini](../logic-apps/logic-apps-gateway-connection.md)kullanır. Mantıksal uygulamalarda yeniyseniz, [Azure Mantık Uygulamaları nedir inceleyin?](../logic-apps/logic-apps-overview.md) Bağlayıcıya özgü teknik bilgiler için [Dosya Sistemi bağlayıcısı başvurusuna](/connectors/filesystem/)bakın.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/).

* Mantık uygulamalarını dosya sistemi sunucunuz gibi şirket içi sistemlere bağlamadan [önce, şirket içi bir veri ağ geçidi yüklemeniz ve kurmanız](../logic-apps/logic-apps-gateway-install.md)gerekir. Bu şekilde, mantık uygulamanızdan dosya sistemi bağlantısını oluştururken ağ geçidi yüklemenizi kullanacağınızı belirtebilirsiniz.

* Ücretsiz olarak kaydolabileceğiniz [dropbox hesabı.](https://www.dropbox.com/) Mantık uygulamanız ile Dropbox hesabınız arasında bağlantı oluşturmak için hesap kimlik bilgileriniz gereklidir.

* Kullanmak istediğiniz dosya sistemine sahip bilgisayara erişim. Örneğin, veri ağ geçidini dosya sisteminizle aynı bilgisayara yüklerseniz, bu bilgisayarın hesap kimlik bilgilerini gerekir.

* Office 365 Outlook, Outlook.com veya Gmail gibi Logic Apps tarafından desteklenen bir sağlayıcıdan gelen e-posta hesabı. Diğer sağlayıcılar için [buradaki bağlayıcı listesini inceleyin](https://docs.microsoft.com/connectors/). Bu mantıksal uygulama bir Office 365 Outlook hesabı kullanır. Başka bir e-posta hesabı kullanıyorsanız genel adımlar aynıdır, ancak kullanıcı arabirimi biraz farklı olabilir.

* [Mantık uygulamaları oluşturmak için nasıl](../logic-apps/quickstart-create-first-logic-app-workflow.md)temel bilgi . Bu örnekte, boş bir mantık uygulamasına ihtiyacınız var.

## <a name="add-trigger"></a>Tetikleyici ekleme

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. [Azure portalında](https://portal.azure.com)oturum açın ve mantık uygulamanızı zaten açık değilse Mantık Uygulama Tasarımcısı'nda açın.

1. Arama kutusuna filtreniz olarak "dropbox" girin. Tetikleyiciler listesinden şu tetikleyiciyi seçin: **Dosya oluşturulduğunda**

   ![Dropbox tetikleyicisi seçin](media/logic-apps-using-file-connector/select-dropbox-trigger.png)

1. Dropbox hesap kimlik bilgilerinizle oturum açın ve Azure Logic Apps için Dropbox verilerinize erişim yetkisi verin.

1. Tetikleyiciniz için gerekli bilgileri sağlayın.

   ![Dropbox tetikleme](media/logic-apps-using-file-connector/dropbox-trigger.png)

## <a name="add-actions"></a>Eylem ekleme

1. Tetikleyicinin altında Sonraki **adımı**seçin. Arama kutusuna filtreniz olarak "dosya sistemi" girin. Eylemler listesinden şu eylemi seçin: **Dosya oluşturma**

   ![Dosya Sistemi konektörünü bul](media/logic-apps-using-file-connector/find-file-system-action.png)

1. Dosya sisteminizle zaten bir bağlantınız yoksa, bir bağlantı oluşturmanız istenir.

   ![Bağlantı oluşturma](media/logic-apps-using-file-connector/file-system-connection.png)

   | Özellik | Gerekli | Değer | Açıklama |
   | -------- | -------- | ----- | ----------- |
   | **Bağlantı Adı** | Evet | <*bağlantı adı*> | Bağlantınız için istediğiniz ad |
   | **Kök klasörü** | Evet | <*kök-klasör-adı*> | Örneğin, dosya sisteminizin kök klasörü, şirket içi veri ağ geçidinin yüklendiği bilgisayara yerel bir klasör veya bilgisayarın erişebileceği bir ağ paylaşımı klasörü gibi şirket içi veri ağ geçidinizi yüklediyseniz. <p>Örneğin, `\\PublicShare\\DropboxFiles` <p>Kök klasör, dosyayla ilgili tüm eylemler için göreli yollar için kullanılan ana üst klasördür. |
   | **Kimlik Doğrulama Türü** | Hayır | <*auth tipi*> | Dosya sisteminizin kullandığı kimlik doğrulama türü: **Windows** |
   | **Username** | Evet | <*etki alanı*>\\<*kullanıcı adı*> | Dosya sisteminizin bulunduğu bilgisayarın kullanıcı adı |
   | **Parola** | Evet | <*şifreniz*> | Dosya sisteminizin bulunduğu bilgisayarın şifresi |
   | **Ağ geçidi** | Evet | <*yüklü ağ geçidi adı*> | Daha önce yüklenmiş ağ geçidinizin adı |
   |||||

1. İşiniz bittiğinde **Oluştur**’u seçin.

   Logic Apps bağlantınızı yapılandırır ve sınar, bu da bağlantının düzgün çalıştığından emin olun. Bağlantı doğru ayarlanmışsa, daha önce seçtiğiniz eylem için seçenekler görüntülenir.

1. Dosya **oluştur** eyleminde, dosya yı kopyalamak için ayrıntıları Dropbox'tan şirket içi dosya paylaşımınızdaki kök klasöre sağlayın. Önceki adımlardan çıktı eklemek için kutuların içini tıklatın ve dinamik içerik listesi göründüğünde kullanılabilir alanlardan seçim yapmayı n için.

   ![Dosya eylemi oluşturma](media/logic-apps-using-file-connector/create-file-filled.png)

1. Şimdi, uygun kullanıcıların yeni dosyayı bilmesi için e-posta gönderen bir Outlook eylemi ekleyin. E-postanın alıcılarını, başlığını ve gövdesini girin. Test etmek için kendi e-posta adresinizi kullanabilirsiniz.

   ![E-posta eylemi gönderme](media/logic-apps-using-file-connector/send-email.png)

1. Mantıksal uygulamanızı kaydedin. Dropbox'a bir dosya yükleyerek uygulamanızı test edin.

   Mantık uygulamanız dosyayı şirket içi dosya paylaşımınıza kopyalamalı ve alıcılara kopyalanan dosya yla ilgili bir e-posta göndermelidir.

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Tetikleyiciler, eylemler ve konektörün Swagger dosyasında açıklandığı gibi sınırlar gibi bu bağlayıcı hakkında daha fazla teknik ayrıntı için [bağlayıcının başvuru sayfasına](https://docs.microsoft.com/connectors/fileconnector/)bakın.

> [!NOTE]
> [Bir entegrasyon hizmeti ortamındaki (İmKB)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md)mantık uygulamaları için, bu bağlayıcının İmKB etiketli sürümü bunun yerine [İmKB ileti sınırlarını](../logic-apps/logic-apps-limits-and-config.md#message-size-limits) kullanır.

## <a name="next-steps"></a>Sonraki adımlar

* Şirket içi verilere nasıl [bağlanıyarı](../logic-apps/logic-apps-gateway-connection.md) zedilen öğrenin 
* Diğer [Logic Apps bağlayıcıları](../connectors/apis-list.md) hakkında bilgi edinin
