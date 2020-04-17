---
title: B2B için AS2 mesajları gönderme ve alma
description: Azure Logic Apps'ı Kurumsal Tümleştirme Paketi ile kullanarak B2B kurumsal tümleştirme senaryoları için AS2 iletilerini değiştirme
services: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, logicappspm
ms.topic: article
ms.date: 02/27/2020
ms.openlocfilehash: 0e7c34e42d0ab68a5dab9718075f02a85322ce6c
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81458837"
---
# <a name="exchange-as2-messages-for-b2b-enterprise-integration-in-azure-logic-apps-with-enterprise-integration-pack"></a>Azure Logic Apps'ta Kurumsal Tümleştirme Paketi ile B2B kurumsal tümleştirme için AS2 iletilerini değiştirme

> [!IMPORTANT]
> Orijinal AS2 konektörü amortismana sokuluyor, bu nedenle **as2 (v2)** konektörü kullandığınızdan emin olun. Bu sürüm orijinal sürümle aynı yetenekleri sağlar, Logic Apps çalışma süresine özgüdür ve iş oluşturma ve ileti boyutu açısından önemli performans iyileştirmeleri sağlar. Ayrıca, yerel v2 bağlayıcısı tümleştirme hesabınıza bir bağlantı oluşturmanızı gerektirmez. Bunun yerine, ön koşullarda açıklandığı gibi, entegrasyon hesabınızı bağlayıcıyı kullanmayı planladığınız mantık uygulamasına bağladığınızdan emin olun.

Azure Logic Apps'ta AS2 iletileriyle çalışmak için, AS2 iletişimini yönetmek için tetikleyiciler ve eylemler sağlayan AS2 bağlayıcısını kullanabilirsiniz. Örneğin, ileti leri iletirirken güvenlik ve güvenilirlik oluşturmak için şu eylemleri kullanabilirsiniz:

* [ **AS2** ](#encode) İnkar etmemeyi desteklemeye yardımcı olan İleti Verme Bildirimleri (MDN) aracılığıyla şifreleme, dijital imzalama ve bildirimler sağlamak için eylemi kodlayın. Örneğin, bu eylem AS2/HTTP üstbilgilerini uygular ve yapılandırıldığınızda bu görevleri gerçekleştirir:

  * Giden iletileri işaretler.
  * Giden iletileri şifreler.
  * İletiyi sıkıştırır.
  * MIME üstbilgisinde dosya adını iletir.

* İleti Verme Bildirimleri (MDN) aracılığıyla şifre çözme, dijital imzalama ve bildirimler sağlamak için [ **AS2 Decode** eylemi.](#decode) Örneğin, bu eylem şu görevleri gerçekleştirir:

  * AS2/HTTP üstbilgilerini işler.
  * Uzlaştırmalar alınan MDN'ler ile özgün giden iletiler.
  * Reddedilmeyen veritabanındaki kayıtları güncelleştirir ve ilişkilendirir.
  * AS2 durum raporlaması için kayıtlar yazar.
  * Çıkışlar yük içeriği base64 kodlanmış olarak.
  * MDN'lerin gerekli olup olmadığını belirler. AS2 anlaşmasına dayanarak, MDN'lerin senkron mu yoksa eşzamanlı mı olması gerektiğini belirler.
  * AS2 anlaşmasına dayalı senkron veya eşzamanlı MdN'ler oluşturur.
  * MDN'ler üzerindeki korelasyon belirteçlerini ve özelliklerini ayarlar.

  Bu eylem, yapılandırıldığınızda da bu görevleri gerçekleştirir:

  * İmzayı doğrular.
  * İletilerin şifresini çözer.
  * İletiyi sıkıştırır.
  * İleti kimliği yinelenenleri denetleyin ve izin verin.

Bu makalede, varolan bir mantık uygulamasına AS2 kodlama ve kod çözme eylemlerinin nasıl eklenilen bir şekilde eklenilen gösteriliyor.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Henüz bir Azure aboneliğiniz yoksa, [ücretsiz bir Azure hesabına kaydolun.](https://azure.microsoft.com/free/)

* AS2 konektörünü kullanmak istediğiniz yerden gelen mantık uygulaması ve mantık uygulamanızın iş akışını başlatan bir tetikleyici. AS2 bağlayıcısı tetikleyiciler değil, yalnızca eylemler sağlar. Mantıksal uygulamalarda yeniyseniz, [Azure Mantık Uygulamaları ve](../logic-apps/logic-apps-overview.md) Quickstart nedir'yi inceleyin: İlk [mantık uygulamanızı oluşturun.](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* Azure aboneliğinizle ilişkili ve AS2 konektörünü kullanmayı planladığınız mantık uygulamasına bağlı bir [entegrasyon hesabı.](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) Hem mantık uygulamanız hem de tümleştirme hesabınız aynı konumda veya Azure bölgesinde bulunmalıdır.

* AS2 kimlik niteleyicisini kullanarak entegrasyon hesabınızda zaten tanımladığınız en az iki [ticaret ortağı.](../logic-apps/logic-apps-enterprise-integration-partners.md)

* AS2 konektörünü kullanamadan önce, ticaret ortaklarınız arasında bir AS2 [anlaşması](../logic-apps/logic-apps-enterprise-integration-agreements.md) oluşturmanız ve bu anlaşmayı entegrasyon hesabınızda saklamanız gerekir.

* Sertifika yönetimi için [Azure Key Vault](../key-vault/general/overview.md) kullanıyorsanız, kasa anahtarlarınızın **Şifreleme** ve **Şifre çözme** işlemlerine izin verdiğini kontrol edin. Aksi takdirde, kodlama ve kod çözme eylemleri başarısız olur.

  Azure portalında, anahtar kasanızdaki anahtara gidin, anahtarınızın **İzin Verilen işlemlerini**gözden geçirin ve örneğin **Şifreleme** ve Şifre **Çözme** işlemlerinin seçildiğini onaylayın:

  ![Kasa anahtar işlemlerini kontrol edin](media/logic-apps-enterprise-integration-as2/key-vault-permitted-operations.png)

<a name="encode"></a>

## <a name="encode-as2-messages"></a>AS2 iletilerini kodlama

1. Azure [portalında](https://portal.azure.com)henüz yapmadıysanız, mantık uygulamanızı Logic App Designer'da açın.

1. Tasarımcıda, mantık uygulamanıza yeni bir eylem ekleyin.

1. Bir eylem ve arama kutusunu **seçin** **altında, Tümü'nü**seçin. Arama kutusuna "as2 kod" girin ve AS2 (v2) eylemini seçtiğinizden emin olun: **AS2 Encode**

   !["AS2 Encode" seçeneğini belirleyin](./media/logic-apps-enterprise-integration-as2/select-as2-encode.png)

1. Şimdi bu özellikler için bilgi sağlayın:

   | Özellik | Açıklama |
   |----------|-------------|
   | **Kodlamak için ileti** | İleti yükü |
   | **itibaren AS2** | AS2 sözleşmenizde belirtildiği şekilde ileti gönderenin tanımlayıcısı |
   | **AS2 için** | AS2 sözleşmenizde belirtildiği şekilde ileti alıcısının tanımlayıcısı |
   |||

   Örneğin:

   ![İleti kodlama özellikleri](./media/logic-apps-enterprise-integration-as2/as2-message-encoding-details.png)

> [!TIP]
> İmzalı veya şifreli iletigönderirken sorun yaşıyorsanız, farklı SHA256 algoritma biçimlerini denemeyi düşünün. AS2 belirtimi SHA256 biçimleri hakkında herhangi bir bilgi sağlamaz, bu nedenle her sağlayıcı kendi uygulama veya biçimini kullanır.

<a name="decode"></a>

## <a name="decode-as2-messages"></a>AS2 iletilerini çözme

1. Azure [portalında](https://portal.azure.com)henüz yapmadıysanız, mantık uygulamanızı Logic App Designer'da açın.

1. Tasarımcıda, mantık uygulamanıza yeni bir eylem ekleyin.

1. Bir eylem ve arama kutusunu **seçin** **altında, Tümü'nü**seçin. Arama kutusuna "as2 decode" girin ve AS2 (v2) eylemini seçtiğinizden emin olun: **AS2 Decode**

   !["AS2 Kod Çözme" seçeneğini belirleyin](media/logic-apps-enterprise-integration-as2/select-as2-decode.png)

1. **İleti'nin kodlaması** ve **İleti üstbilgiözellikleri** için bu değerleri önceki tetikleyici veya eylem çıktılarından seçin.

   Örneğin, mantık uygulamanızın bir İstek tetikleyicisi aracılığıyla ileti aldığını varsayalım. Bu tetikleyiciden çıktıları seçebilirsiniz.

   ![İstek çıktılarından Gövde ve Üstbilgi seçin](media/logic-apps-enterprise-integration-as2/as2-message-decoding-details.png)

## <a name="sample"></a>Örnek

Tam işlevsel bir mantık uygulaması dağıtmayı denemek ve AS2 senaryosunu örneklemek için [AS2 mantık uygulaması şablonuna ve senaryosuna](https://azure.microsoft.com/documentation/templates/201-logic-app-as2-send-receive/)bakın.

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Bu bağlayıcı hakkında bağlayıcının Swagger dosyasında açıklandığı gibi eylemler ve sınırlar gibi daha fazla teknik ayrıntı için [bağlayıcının başvuru sayfasına](https://docs.microsoft.com/connectors/as2/)bakın. 

> [!NOTE]
> [Bir entegrasyon hizmeti ortamındaki (İmKB)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md)mantık uygulamaları için, bu bağlayıcının orijinal İmKB etiketli sürümü bunun yerine [İmKB ileti sınırlarını](../logic-apps/logic-apps-limits-and-config.md#message-size-limits) kullanır.

## <a name="next-steps"></a>Sonraki adımlar

* Diğer [Logic Apps bağlayıcıları](../connectors/apis-list.md) hakkında bilgi edinin
