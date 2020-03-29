---
title: SFTP hesabına bağlanın
description: Azure Logic Apps'ı kullanarak SSH aracılığıyla bir SFTP sunucusuiçin dosyaları izleyen, oluşturan, yöneten, gönderen ve alan görevleri ve işlemleri otomatikleştirin
services: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.reviewer: divswa, klam, logicappspm
ms.topic: article
ms.date: 11/01/2019
tags: connectors
ms.openlocfilehash: d0da98070fa8da5403677e1a67bda75456c74d80
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74789282"
---
# <a name="monitor-create-and-manage-sftp-files-by-using-azure-logic-apps"></a>Azure Logic Apps kullanarak SFTP dosyalarını izleme, oluşturma ve yönetme

> [!IMPORTANT]
> SFTP konektörü amortismana lı olduğundan lütfen [SFTP-SSH konektörünü](../connectors/connectors-sftp-ssh.md) kullanın. Artık Logic App Designer'da SFTP tetikleyicilerini ve eylemlerini seçemezsiniz.

[Güvenli Dosya Aktarım Protokolü (SFTP)](https://www.ssh.com/ssh/sftp/) sunucusunda dosyaları izleyen, oluşturan, gönderen ve alan görevleri otomatikleştirmek için Azure Logic Apps ve SFTP bağlayıcısını kullanarak tümleştirme iş akışlarını oluşturabilir ve otomatikleştirebilirsiniz. SFTP, herhangi bir güvenilir veri akışı üzerinde dosya erişimi, dosya aktarımı ve dosya yönetimi sağlayan bir ağ protokolüdür. Otomatikleştirebileceğiniz bazı örnek görevler şunlardır:

* Dosyalar eklendiğinde veya değiştirildiğinde izleyin.
* Dosyaları alın, oluşturun, kopyalayın, güncelleyin, listeleyin ve silin.
* Dosya içeriği ve meta veriler alın.
* Arşivleri klasörlere ayıklayın.

SFTP sunucunuzdaki olayları izleyen ve çıktıyı diğer eylemler için kullanılabilir hale getiren tetikleyiciler kullanabilirsiniz. SFTP sunucunuzda çeşitli görevleri gerçekleştiren eylemleri kullanabilirsiniz. Ayrıca, mantık uygulamanızda SFTP eylemlerinden elde edilen çıktıyı kullanarak başka eylemler de alabilirsiniz. Örneğin, SFTP sunucunuzdan düzenli olarak dosya alırsanız, Office 365 Outlook bağlayıcısını veya Outlook.com bağlayıcısını kullanarak bu dosyalar ve içerikleri hakkında e-posta uyarıları gönderebilirsiniz. Mantıksal uygulamalarda yeniyseniz, [Azure Mantık Uygulamaları nedir'yi inceleyin?](../logic-apps/logic-apps-overview.md)

## <a name="limits"></a>Sınırlar

SFTP bağlayıcısı yalnızca *50 MB veya daha küçük* dosyaları işler ve ileti [ödentiyi](../logic-apps/logic-apps-handle-large-messages.md)desteklemez. Daha büyük dosyalar için [SFTP-SSH konektörünü](../connectors/connectors-sftp-ssh.md)kullanın. SFTP konektörü ile SFTP-SSH konektörü arasındaki farklar için, [SFTP-SSH makalesinde SFTP-SSH ile SFTP'yi karşılaştır'ı](../connectors/connectors-sftp-ssh.md#comparison) gözden geçirin.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Azure aboneliğiniz yoksa [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/).

* Mantık uygulamanızın SFTP hesabınıza erişmesini sağlayan SFTP sunucu adresiniz ve hesap kimlik bilgileriniz. [Secure Shell (SSH)](https://www.ssh.com/ssh/protocol/) protokolünü kullanmak için bir SSH özel anahtarına ve SSH özel anahtar parolasına da erişmeniz gerekir.

  > [!NOTE]
  >
  > SFTP bağlayıcısı bu özel anahtar biçimlerini destekler: OpenSSH, ssh.com ve PuTTY
  >
  > Mantık uygulamanızı oluştururken, istediğiniz SFTP tetikleyicisini veya eylemi ekledikten sonra, SFTP sunucunuz için bağlantı bilgileri sağlamanız gerekir. 
  > Bir SSH özel anahtarı kullanıyorsanız, anahtarı SSH özel anahtar dosyanızdan ***kopyaladığınızdan*** emin olun ve bu anahtarı bağlantı ayrıntılarına ***yapıştırın,*** ***anahtarı el ile girmeyin veya da***bağlantının arızalanmasına neden olabilir. 
  > Daha fazla bilgi için bu makaledeki sonraki adımlara bakın.

* [Mantık uygulamaları oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md) hakkında temel bilgiler

* SFTP hesabınıza erişmek istediğiniz mantık uygulaması. Bir SFTP tetikleyicisi ile başlamak için [boş bir mantık uygulaması oluşturun.](../logic-apps/quickstart-create-first-logic-app-workflow.md) Bir SFTP eylemi kullanmak için, mantık uygulamanızı başka bir tetikleyiciyle başlatın, **örneğin, Yineleme** tetikleyicisi.

## <a name="how-sftp-triggers-work"></a>SFTP nasıl çalışır?

SFTP, SFTP dosya sistemini yoklayarak ve son anketten bu yana değiştirilen herhangi bir dosyayı arayarak çalışmayı tetikler. Bazı araçlar, dosyalar değiştiğinde zaman damgasını korumanızı sağlar. Bu gibi durumlarda, tetikleyicinizin çalışabilmesi için bu özelliği devre dışı beşinizgerekir. Bazı yaygın ayarlar şunlardır:

| SFTP istemcisi | Eylem |
|-------------|--------|
| Winscp | **Seçeneklere** > Git**Tercihleri** > **Aktar** > **Değiştir** > Zaman**damgası** > Devre**Dışı** Bırak |
| Filezilla | **AktarAn** > dosyaların > **Disable** devre dışı**bırak'ın zaman damgalarını kaydet** |
|||

Tetikleyici yeni bir dosya bulduğunda, tetikleyici yeni dosyanın tamamlanıp kısmen yazılmadığını denetler. Örneğin, tetikleyici dosya sunucusunu denetlediğinde bir dosyanın devam eden değişiklikleri olabilir. Kısmen yazılmış bir dosyayı döndürmeyi önlemek için, tetikleyici son değişiklikler eki olan ancak bu dosyayı hemen döndürmeyen dosyanın zaman damgasını not eder. Tetikleyici, yalnızca sunucuyu yeniden yoklarken dosyayı döndürür. Bazen, bu davranış, tetikleyicinin yoklama aralığının iki katına kadar olan bir gecikmeye neden olabilir.

## <a name="connect-to-sftp"></a>SFTP'ye bağlan

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. [Azure portalında](https://portal.azure.com)oturum açın ve mantık uygulamanızı zaten açık değilse Mantık Uygulama Tasarımcısı'nda açın.

1. Boş mantık uygulamaları için, arama kutusuna filtreniz olarak "sftp" girin. Tetikleyiciler listesinin altında, istediğiniz tetikleyiciyi seçin.

   -veya-

   Varolan mantık uygulamaları için, eylem eklemek istediğiniz son adım altında **Yeni adımı**seçin. Arama kutusuna filtreniz olarak "sftp" girin. Eylemler listesinin altında, istediğiniz eylemi seçin.

   Adımlar arasında eylem eklemek için işaretçinizin üzerine adımların arasında ilerleyin. Görünen artı işaretini (**+**) seçin ve ardından eylem **ekle'yi**seçin.

1. Bağlantınız için gerekli ayrıntıları sağlayın.

   > [!IMPORTANT]
   >
   > SSH özel anahtar anahtarınızı **SSH özel anahtar** özelliğine girdiğinizde, bu özellik için tam ve doğru değeri sağladığınızdan emin olmak için aşağıdaki ek adımları izleyin. 
   > Geçersiz bir anahtar, bağlantının başarısız olmasını neden olur.

   Herhangi bir metin düzenleyicisini kullanabiliyor olsanız da, notepad.exe'yi örnek olarak kullanarak anahtarınızı doğru şekilde kopyalamayı ve yapıştırmayı gösteren örnek adımlar aşağıda verilmiştir.

   1. SSH özel anahtar dosyanızı bir metin düzenleyicisinde açın. Bu adımlar örnek olarak Not Defteri'ni kullanır.

   1. Not Defteri **Edit** menüsünde **Tümünü Seç'i**seçin.

   1. **Kopyayı** **Edit'i** > seçin.

   1. Eklediğiniz SFTP tetikleyicisinde veya eyleminde, kopyaladığınız *anahtarın tamamını* birden çok satırı destekleyen **SSH özel anahtar** özelliğine yapıştırın. Anahtarı ***yapıştırdığınızdan emin olun.*** ***Anahtarı el ile girmeyin veya atmayın.***

1. Bağlantı ayrıntılarını girmeyi bitirdikten sonra **Oluştur'u**seçin.

1. Seçtiğiniz tetikleyici veya eylem için gerekli ayrıntıları sağlayın ve mantık uygulamanızın iş akışını oluşturmaya devam edin.

## <a name="examples"></a>Örnekler

<a name="file-add-modified"></a>

### <a name="sftp-trigger-when-a-file-is-added-or-modified"></a>SFTP tetikleyicisi: Bir dosya eklendiğinde veya değiştirildiğinde

Bu tetikleyici, bir SFTP sunucusuna bir dosya eklendiğinde veya değiştirildiğinde bir mantık uygulaması iş akışı başlatır. Örneğin, dosyanın içeriğini denetleyen ve içeriğin belirtilen bir koşulu karşılayıp karşılamadığını temel alan bir koşul ekleyebilirsiniz. Daha sonra, dosyanın içeriğini alan ve bu içeriği SFTP sunucusundaki bir klasöre koyan bir eylem ekleyebilirsiniz.

**Kurumsal örnek**: Müşteri siparişlerini temsil eden yeni dosyalar için bir SFTP klasörünü izlemek için bu tetikleyiciyi kullanabilirsiniz. Daha **sonra, siparişin içeriğini** daha fazla işlemek için almak ve bu siparişi bir sipariş veritabanında depolamak için dosya içeriğini al gibi bir SFTP eylemi kullanabilirsiniz.

<a name="get-content"></a>

### <a name="sftp-action-get-content"></a>SFTP eylemi: İçerik alın

Bu eylem, içeriği Bir SFTP sunucusundaki bir dosyadan alır. Örneğin, önceki örnekteki tetikleyiciyi ve dosyanın içeriğinin karşılaması gereken bir koşul ekleyebilirsiniz. Koşul doğruysa, içeriği alan eylem çalıştırılabilir.

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Bağlayıcının OpenAPI (eski adıyla Swagger) açıklamasıyla açıklanan tetikleyiciler, eylemler ve sınırlar hakkındaki teknik ayrıntılar için bağlayıcının [başvuru sayfasını](/connectors/sftpconnector/)inceleyin.

## <a name="next-steps"></a>Sonraki adımlar

* Diğer [Logic Apps bağlayıcıları](../connectors/apis-list.md) hakkında bilgi edinin
