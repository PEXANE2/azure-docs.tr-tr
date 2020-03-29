---
title: B2B entegrasyonu için RosettaNet mesajları
description: Kurumsal Tümleştirme Paketi ile Azure Logic Apps'ta RosettaNet mesajlarını değiştirin
services: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, logicappspm
ms.topic: article
ms.date: 06/22/2019
ms.openlocfilehash: 2cc2ac08b9624c1d1d9bee9ce91a7c91189d7f2c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74792427"
---
# <a name="exchange-rosettanet-messages-for-b2b-enterprise-integration-in-azure-logic-apps"></a>Azure Logic Apps'ta B2B kurumsal tümleştirme için RosettaNet mesajlarını değiştirme

[RosettaNet,](https://resources.gs1us.org) iş bilgilerini paylaşmak için standart süreçler oluşturmuştur kar amacı gütmeyen bir konsorsiyumdur. Bu standartlar tedarik zinciri süreçleri için yaygın olarak kullanılır ve yarı iletken, elektronik ve lojistik endüstrilerinde yaygındır. RosettaNet konsorsiyumu, tüm RosettaNet ileti alışverişleri için ortak iş süreci tanımları sağlayan Ortak Arabirim Süreçleri (PIP) oluşturur ve sürdürür. RosettaNet XML'yi temel alır ve mesaj yönergelerini, iş süreçleri için arayüzleri ve şirketler arasındaki iletişim için uygulama çerçevelerini tanımlar.

[Azure Logic Apps'ta](../logic-apps/logic-apps-overview.md)RosettaNet konektörü, RosettaNet standartlarını destekleyen entegrasyon çözümleri oluşturmanıza yardımcı olur. Konektör RosettaNet Uygulama Çerçevesi (RNIF) sürüm 2.0.01 dayanmaktadır. RNIF, iş ortaklarının RosettaNet PIP'lerini işbirliği yle çalıştırmasını sağlayan açık ağ uygulama çerçevesidir. Bu çerçeve, ileti yapısını, bildirimlere duyulan gereksinimi, Çok Amaçlı Internet Posta Uzantıları (MIME) kodlamasını ve dijital imzayı tanımlar.

Özellikle, bağlayıcı bu yetenekleri sağlar:

* RosettaNet mesajlarını kodlayın veya alın.
* RosettaNet iletilerinin şifresini çözveya gönderin.
* Yanıt ve Hata Bildirimi oluşturma bekleyin.

Bu özellikler için bağlayıcı, RNIF 2.0.01 tarafından tanımlanan tüm PIN'leri destekler. Ortakla iletişim eşzamanlı veya eşzamanlı olabilir.

## <a name="rosettanet-concepts"></a>RosettaNet kavramları

RosettaNet belirtimine özgü ve RosettaNet tabanlı tümleştirmeleri yaparken önemli olan bazı kavramlar ve terimler şunlardır:

* **Pip**

  RosettaNet organizasyonu, tüm RosettaNet ileti alışverişleri için ortak iş süreci tanımları sağlayan Ortak Arabirim İşlemleri (PIS) oluşturur ve sürdürür. Her PIP belirtimi bir belge türü tanımı (DTD) dosyası ve bir ileti kılavuzu belgesi sağlar. DTD dosyası hizmet içeriği ileti yapısını tanımlar. İnsan tarafından okunabilir bir HTML dosyası olan ileti kılavuzu belgesi, öğe düzeyinde kısıtlamalar belirtir. Birlikte, bu dosyalar iş sürecinin tam bir tanımını sağlar.

   PIN'ler üst düzey bir iş işlevine veya kümeye ve bir alt işleve veya segmente göre sınıflandırılır. Örneğin, "3A4" Satınalma Siparişi için PIP iken, "3" Sipariş Yönetimi işlevi, "3A" ise Teklif & Sipariş Girişi alt işlevidir. Daha fazla bilgi için [RosettaNet sitesine](https://resources.gs1us.org)bakın.

* **Eylem**

  PIP'in bir parçası olan eylem iletileri, ortaklar arasında değiş tokuş edilen iş iletileridir.

* **Sinyal**

   PIP'in bir parçası olan sinyal iletileri, eylem iletilerine yanıt olarak gönderilen bildirimlerdir.

* **Tek eylem ve çift eylem**

  Tek eylemli BIR PIP için tek yanıt bir bildirim sinyali iletisidir. Çift eylemli bir PIP için, başlatıcı bir yanıt iletisi alır ve tek eylemli ileti akışına ek olarak bir bildirimle yanıtlar.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Henüz bir Azure aboneliğiniz yoksa, [ücretsiz bir Azure hesabına kaydolun.](https://azure.microsoft.com/free/)

* Anlaşmanızı ve diğer B2B eserlerini depolamak için bir [entegrasyon hesabı.](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) Bu tümleştirme hesabı, Azure aboneliğinizle ilişkilendirilmelidir.

* İş **Kimlikleri** altında entegrasyon hesabınızda tanımlanan ve "DUNS" niteleyicisiyle yapılandırılan en az iki [ortak](../logic-apps/logic-apps-enterprise-integration-partners.md)

* Entegrasyon hesabınızda RosettaNet iletileri göndermek veya almak için gereken bir PIP işlem yapılandırması. İşlem yapılandırması tüm PIP yapılandırma özelliklerini depolar. Daha sonra iş ortağı ile bir anlaşma oluşturduğunuzda bu yapılandırmabaşvurur. Entegrasyon hesabınızda bir PIP işlemi yapılandırması oluşturmak için [bkz.](#add-pip)

* Tümleştirme hesabına yüklediğiniz iletileri şifrelemek, şifresini çözmek veya imzalamak için isteğe bağlı [sertifikalar.](../logic-apps/logic-apps-enterprise-integration-certificates.md) Sertifikalar yalnızca imzalama veya şifreleme kullanıyorsanız gereklidir.

<a name="add-pip"></a>

## <a name="add-pip-process-configuration"></a>PIP işlem yapılandırması ekleme

Entegrasyon hesabınıza BIR PIP işlemi yapılandırması eklemek için aşağıdaki adımları izleyin:

1. Azure [portalında,](https://portal.azure.com)entegrasyon hesabınızı bulun ve açın.

1. Genel **Bakış** bölmesindeki **RosettaNet PIP** döşemesini seçin.

   ![RosettaNet karosu seçin](media/logic-apps-enterprise-integration-rosettanet/select-rosettanet-tile.png)

1. **RosettaNet PIP**altında, **Ekle'yi**seçin. PIP bilgilerinizi sağlayın.

   ![RosettaNet PIP ayrıntılarını ekleyin](media/logic-apps-enterprise-integration-rosettanet/add-rosettanet-pip.png)

   | Özellik | Gerekli | Açıklama |
   |----------|----------|-------------|
   | **Adı** | Evet | PIP adınız |
   | **PIP Kodu** | Evet | PIP üç basamaklı kod. Daha fazla bilgi için [RosettaNet PIP'leri'ne](https://docs.microsoft.com/biztalk/adapters-and-accelerators/accelerator-rosettanet/rosettanet-pips)bakın. |
   | **PIP Sürümü** | Evet | Seçtiğiniz PIP koduna göre kullanılabilen PIP sürüm numarası |
   ||||

   Bu PIP özellikleri hakkında daha fazla bilgi için [RosettaNet web sitesini](https://resources.gs1us.org/RosettaNet-Standards/Standards-Library/PIP-Directory#1043208-pipsreg)ziyaret edin.

1. İşi bittiğinde, PIP yapılandırmasını oluşturan **Tamam'ı**seçin.

1. İşlem yapılandırmasını görüntülemek veya düzenlemek için PIP'i seçin ve **JSON olarak Edit'i**seçin.

   Tüm işlem yapılandırma ayarları PIP'in belirtimlerinden gelir. Logic Apps, bu özellikler için en sık kullanılan değerler olan varsayılan değerlerle ayarların çoğunu doldurur.

   ![RosettaNet PIP yapılandırmayı düzenleme](media/logic-apps-enterprise-integration-rosettanet/edit-rosettanet-pip.png)

1. Ayarların uygun PIP belirtimindeki değerlere karşılık geldiğini ve iş ihtiyaçlarınızı karşıladığını doğrulayın. Gerekirse, JSON'daki değerleri güncelleştirin ve bu değişiklikleri kaydedin.

## <a name="create-rosettanet-agreement"></a>RosettaNet anlaşması oluşturun

1. Azure [portalında,](https://portal.azure.com)zaten açık değilse tümleştirme hesabınızı bulun ve açın.

1. Genel **Bakış** bölmesindeki **Anlaşmalar** döşemesini seçin.

   ![Anlaşmalar döşemesi seçin](media/logic-apps-enterprise-integration-rosettanet/select-agreement-tile.png)

1. **Anlaşmalar**altında **Ekle'yi**seçin. Anlaşma bilgilerinizi sağlayın.

   ![Anlaşma ayrıntıları ekleme](media/logic-apps-enterprise-integration-rosettanet/add-agreement-details.png)

   | Özellik | Gerekli | Açıklama |
   |----------|----------|-------------|
   | **Adı** | Evet | Anlaşmanın adı |
   | **Anlaşma türü** | Evet | **RosettaNet'i**seçin. |
   | **Ev Sahibi Ortak** | Evet | Bir anlaşma hem ev sahibi hem de konuk ortak gerektirir. Ana bilgisayar ortağı, anlaşmayı yapılandıran kuruluşu temsil eder. |
   | **Ev Sahibi Kimliği** | Evet | Ev sahibi ortak için bir tanımlayıcı |
   | **Misafir Ortak** | Evet | Bir anlaşma hem ev sahibi hem de konuk ortak gerektirir. Konuk ortak, ev sahibi ortakla iş yapan kuruluşu temsil eder. |
   | **Misafir Kimliği** | Evet | Konuk ortak için bir tanımlayıcı |
   | **Ayarları Al** | Değişir | Bu özellikler, ana bilgisayar ortağı tarafından alınan tüm iletiler için geçerlidir |
   | **Ayarları Gönder** | Değişir | Bu özellikler, ana bilgisayar ortağı tarafından gönderilen tüm iletiler için geçerlidir |  
   | **RosettaNet PIP referansları** | Evet | Anlaşma için PIP referansları. Tüm RosettaNet iletileri PIP yapılandırmaları gerektirir. |
   ||||

1. Konuk iş ortağından gelen iletileri almak için anlaşmanızı ayarlamak için **Ayarları Al'ı**seçin.

   ![Ayarları alma](media/logic-apps-enterprise-integration-rosettanet/add-agreement-receive-details.png)

   1. Gelen iletiler için imzalamayı veya şifrelemeyi etkinleştirmek için **İletiler**altında **İleti'yi** seçin veya İleti sırasıyla **şifrelenmelidir.**

      | Özellik | Gerekli | Açıklama |
      |----------|----------|-------------|
      | **İleti imzalanmalıdır** | Hayır | Seçilen sertifikayla gelen iletileri imzalayın. |
      | **Sertifika** | Evet, oturum açma etkinse | İmzalamak için kullanılacak sertifika |
      | **İleti şifrelemesini etkinleştirme** | Hayır | Gelen iletileri seçili sertifikayla şifreleyin. |
      | **Sertifika** | Evet, şifreleme etkinse | Şifreleme için kullanılacak sertifika |
      ||||

   1. Her seçimde, imzalama veya şifreleme için kullanmak üzere daha önce entegrasyon hesabınıza eklediğiniz ilgili [sertifikayı](./logic-apps-enterprise-integration-certificates.md)seçin.

1. Konuk iş ortağına mesaj göndermek için anlaşmanızı ayarlamak için **Ayarlar Gönder'i**seçin.

   ![Ayarları gönder](media/logic-apps-enterprise-integration-rosettanet/add-agreement-send-details.png)

   1. Giden iletiler için imzalamayı veya şifrelemeyi etkinleştirmek **için, İletiler**altında, **sırasıyla ileti imzalamayı etkinleştir** veya **ileti şifrelemesini etkinleştir'i** seçin. Her seçimde, imzalama veya şifreleme için kullanmak üzere daha önce tümleştirme hesabınıza eklediğiniz ilgili algoritmayı ve [sertifikayı](./logic-apps-enterprise-integration-certificates.md)seçin.

      | Özellik | Gerekli | Açıklama |
      |----------|----------|-------------|
      | **İleti imzalamayı etkinleştirme** | Hayır | Giden iletileri seçili imzalama algoritması ve sertifikasıyla imzalayın. |
      | **İmza Algoritması** | Evet, oturum açma etkinse | Seçili sertifikayı temel alan kullanılacak imzalama algoritması |
      | **Sertifika** | Evet, oturum açma etkinse | İmzalamak için kullanılacak sertifika |
      | **İleti şifrelemesini etkinleştirme** | Hayır | Seçili şifreleme algoritması ve sertifikası ile gidenşifreni şifreleyin. |
      | **Şifreleme Algoritması** | Evet, şifreleme etkinse | Seçili sertifikayı temel alan kullanılacak şifreleme algoritması |
      | **Sertifika** | Evet, şifreleme etkinse | Şifreleme için kullanılacak sertifika |
      ||||

   1. **Uç Noktaları**altında, eylem iletileri ve bildirimler göndermek için kullanmak için gerekli URL'leri belirtin.

      | Özellik | Gerekli | Açıklama |
      |----------|----------|-------------|
      | **Eylem URL'si** |  Evet | Eylem iletileri göndermek için kullanılacak URL. URL, hem senkron hem de eşzamanlı iletiler için gerekli bir alandır. |
      | **Bildirim URL'si** | Evet | Bildirim iletileri göndermek için kullanılacak URL. URL, eşzamanlı iletiler için gerekli bir alandır. |
      ||||

1. İş ortakları için RosettaNet PIP referansları ile anlaşmanızı ayarlamak için **RosettaNet PIP referanslarını**seçin. **PIP Adı**altında, daha önce oluşturduğunuz PIP'in adını seçin.

   ![PIP referansları](media/logic-apps-enterprise-integration-rosettanet/add-agreement-pip-details.png)

   Seçiminiz, tümleştirme hesabınızda ayarladığınız PIP'e dayalı kalan özellikleri doldurur. Gerekirse, **PIP Rolünü**değiştirebilirsiniz.

   ![Seçilen PIP](media/logic-apps-enterprise-integration-rosettanet/add-agreement-selected-pip.png)

Bu adımları tamamladıktan sonra RosettaNet iletileri göndermeye veya almaya hazırsınız.

## <a name="rosettanet-templates"></a>RosettaNet şablonları

Geliştirmeyi hızlandırmak ve tümleştirme desenleri önermek için RosettaNet iletilerini çözmek ve kodlamak için mantık uygulama şablonlarını kullanabilirsiniz. Bir mantık uygulaması oluşturduğunuzda, Logic App Designer'daki şablon galerisinden seçim yapabilirsiniz. Bu şablonları Azure Logic [Apps için GitHub deposunda](https://github.com/Azure/logicapps)da bulabilirsiniz.

![RosettaNet şablonları](media/logic-apps-enterprise-integration-rosettanet/decode-encode-rosettanet-templates.png)

## <a name="receive-or-decode-rosettanet-messages"></a>RosettaNet iletilerini alma veya çözme

1. [Boş bir mantık uygulaması oluşturun.](quickstart-create-first-logic-app-workflow.md)

1. Entegrasyon hesabınızı mantık [uygulamanıza bağla.](logic-apps-enterprise-integration-create-integration-account.md#link-account)

1. RosettaNet iletisini çözmek için bir eylem eklemeden önce, İstek tetikleyicisi gibi mantık uygulamanızı başlatmak için bir tetikleyici eklemeniz gerekir.

1. Tetikleyiciyi ekledikten sonra **Yeni adımı**seçin.

   ![İstek tetikleyicisi ekle](media/logic-apps-enterprise-integration-rosettanet/request-trigger.png)

1. Arama kutusuna "rosettanet" girin ve şu eylemi seçin: **RosettaNet Decode**

   !["RosettaNet Decode" eylemini bulun ve seçin](media/logic-apps-enterprise-integration-rosettanet/select-decode-rosettanet-action.png)

1. Eylemin özellikleri için bilgi sağlayın:

   ![Eylem ayrıntıları sağlama](media/logic-apps-enterprise-integration-rosettanet/decode-action-details.png)

   | Özellik | Gerekli | Açıklama |
   |----------|----------|-------------|
   | **İleti** | Evet | Kod çözme için RosettaNet iletisi  |
   | **Üst Bilgiler** | Evet | RNIF sürümü olan sürüm için değerleri sağlayan HTTP üstbilgive ortaklar arasındaki iletişim türünü gösteren yanıt türü ve eşzamanlı veya eşzamanlı olabilir |
   | **Rol** | Evet | PIP'de ev sahibi ortağın rolü |
   ||||

   RosettaNet Decode eyleminden, çıktı, diğer özelliklerle birlikte, kodlamayı ve ortağa geri dönmeyi seçebileceğiniz **giden sinyali**içerir veya bu çıktı yla ilgili başka herhangi bir işlem gerçekleştirebilirsiniz.

## <a name="send-or-encode-rosettanet-messages"></a>RosettaNet iletilerini gönderme veya kodlama

1. [Boş bir mantık uygulaması oluşturun.](quickstart-create-first-logic-app-workflow.md)

1. Entegrasyon hesabınızı mantık [uygulamanıza bağla.](logic-apps-enterprise-integration-create-integration-account.md#link-account)

1. RosettaNet iletisini kodlamak için bir eylem eklemeden önce, İstek tetikleyicisi gibi mantık uygulamanızı başlatmak için bir tetikleyici eklemeniz gerekir.

1. Tetikleyiciyi ekledikten sonra **Yeni adımı**seçin.

   ![İstek tetikleyicisi ekle](media/logic-apps-enterprise-integration-rosettanet/request-trigger.png)

1. Arama kutusuna "rosettanet" girin ve şu eylemi seçin: **RosettaNet Encode**

   !["RosettaNet Encode" eylemini bulun ve seçin](media/logic-apps-enterprise-integration-rosettanet/select-encode-rosettanet-action.png)

1. Eylemin özellikleri için bilgi sağlayın:

   ![Eylem ayrıntıları sağlama](media/logic-apps-enterprise-integration-rosettanet/encode-action-details.png)

   | Özellik | Gerekli | Açıklama |
   |----------|----------|-------------|
   | **İleti** | Evet | Kodlamak için RosettaNet iletisi  |
   | **Ev sahibi iş ortağı** | Evet | Ev sahibi ortak adı |
   | **Konuk ortak** | Evet | Konuk ortak adı |
   | **PIP kodu** | Evet | PIP kodu |
   | **PIP sürümü** | Evet | PIP sürümü |  
   | **PIP örnek kimliği** | Evet | Bu PIP iletisi için benzersiz tanımlayıcı |  
   | **Mesaj türü** | Evet | Kodlamak için iletinin türü |  
   | **Rol** | Evet | Ev sahibi ortağın rolü |
   ||||

   Kodlanmış ileti artık ortaka göndermeye hazır.

1. Kodlanmış iletiyi göndermek için bu örnek, "HTTP - Kodlanmış iletiyi iş ortağına gönder" olarak yeniden adlandırılan **HTTP** eylemini kullanır.

   ![RosettaNet iletisi göndermek için HTTP eylem](media/logic-apps-enterprise-integration-rosettanet/send-rosettanet-message-to-partner.png)

   RosettaNet standartlarına göre, iş hareketleri yalnızca PIP tarafından tanımlanan tüm adımlar tamamlandığında tamamlanmış olarak kabul edilir.

1. Ana bilgisayar kodlanmış iletiyi ortaklara gönderdikten sonra, ana bilgisayar sinyali ve bildirimi bekler. Bu görevi gerçekleştirmek için, yanıt eylemi **için RosettaNet bekleyin** ekleyin.

   !["RosettaNet yanıt için bekleyin" eylemi ekleyin](media/logic-apps-enterprise-integration-rosettanet/rosettanet-wait-for-response-action.png)

   Bekleme için kullanılacak süre ve yeniden deneme sayısı, tümleştirme hesabınızdaki PIP yapılandırmasına bağlıdır. Yanıt alınmazsa, bu eylem bir Hata Bildirimi oluşturur. Yeniden denemeleri işlemek için Her zaman **Encode'u** koyun ve yanıt eylemlerini **bir Bekleme** döngüsüne **bekleyin.**

   ![RosettaNet eylemleri ile döngü kadar](media/logic-apps-enterprise-integration-rosettanet/rosettanet-loop.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Kurumsal Tümleştirme Paketi](../logic-apps/logic-apps-enterprise-integration-overview.md) ile diğer ileti işlemlerini nasıl doğrulayanın, dönüştüreceklerinizi ve diğer ileti işlemlerini öğrenin
* Diğer [Logic Apps bağlayıcıları](../connectors/apis-list.md) hakkında bilgi edinin
