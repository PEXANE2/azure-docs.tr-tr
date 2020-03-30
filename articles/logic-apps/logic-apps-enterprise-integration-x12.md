---
title: B2B entegrasyonu için X12 iletileri
description: Kurumsal Tümleştirme Paketi ile Azure Mantık Uygulamalarında B2B kurumsal tümleştirmesi için EDI formatında X12 iletilerini değiştirin
services: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, logicappspm
ms.topic: article
ms.date: 01/31/2017
ms.openlocfilehash: cbf0a1f033ddafc68debab8de26dff29d73cc98e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77651483"
---
# <a name="exchange-x12-messages-for-b2b-enterprise-integration-in-azure-logic-apps-with-enterprise-integration-pack"></a>Kurumsal Tümleştirme Paketi ile Azure Logic Apps'ta B2B kurumsal tümleştirme için X12 iletilerini değiştirme

Azure Logic Apps için X12 iletilerini değiştiremeden önce bir X12 sözleşmesi oluşturmanız ve bu sözleşmeyi tümleştirme hesabınızda depolamanız gerekir. X12 anlaşmasının nasıl oluşturulabildiğini anlatan adımlar aşağıda veda edebilirsiniz.

> [!NOTE]
> Bu sayfa, Azure Mantık Uygulamaları için X12 özelliklerini kapsar. Daha fazla bilgi için [EDIFACT'e](logic-apps-enterprise-integration-edifact.md)bakın.

## <a name="before-you-start"></a>Başlamadan önce

İhtiyacınız olan öğeler şunlardır:

* Azure aboneliğiniz önceden tanımlanmış ve ilişkili bir [tümleştirme hesabı](logic-apps-enterprise-integration-create-integration-account.md)
* İş **Kimlikleri** altında entegrasyon hesabınızda tanımlanan ve X12 tanımlayıcısı ile yapılandırılan en az iki [ortak](../logic-apps/logic-apps-enterprise-integration-partners.md)    
* Entegrasyon hesabınıza yükleyebileceğiniz gerekli bir [şema](../logic-apps/logic-apps-enterprise-integration-schemas.md)

Bir [tümleştirme hesabı oluşturduktan,](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) [iş ortakları ekledikten](logic-apps-enterprise-integration-partners.md)ve kullanmak istediğiniz bir [şemaya](../logic-apps/logic-apps-enterprise-integration-schemas.md) sahip olduktan sonra, aşağıdaki adımları izleyerek bir X12 anlaşması oluşturabilirsiniz.

## <a name="create-an-x12-agreement"></a>X12 anlaşması oluşturma

1. [Azure portalında](https://portal.azure.com "Azure portalında")oturum açın. 

2. Ana Azure menüsünden **Tüm hizmetler'i**seçin. 
   Arama kutusuna "tümleştirme" girin ve ardından **Tümleştirme hesaplarını**seçin.  

   ![Entegrasyon hesabınızı bulun](./media/logic-apps-enterprise-integration-x12/account-1.png)

   > [!TIP]
   > **Tüm hizmetler** görünmüyorsa, önce menüyü genişletmeniz gerekebilir. Daraltılmış menünün üst kısmında **Göster menüsünü**seçin.

3. **Tümleştirme Hesapları**altında, sözleşmeeklemek istediğiniz tümleştirme hesabını seçin.

   ![Anlaşmanın oluşturulacağı tümleştirme hesabını seçin](./media/logic-apps-enterprise-integration-x12/account-3.png)

4. **Genel Bakış'ı**seçin, ardından **Anlaşmalar** döşemesini seçin. 
   Anlaşmalı döşemeniz yoksa, önce döşemeyi ekleyin. 

   !["Anlaşmalar" döşemesini seçin](./media/logic-apps-enterprise-integration-x12/agreement-1.png)

5. **Anlaşmalar**altında **Ekle'yi**seçin.

   !["Ekle" seçeneğini belirleyin](./media/logic-apps-enterprise-integration-x12/agreement-2.png)     

6. **Add**, anlaşmanız için bir **Ad** girin. 
   Anlaşma türü için **X12'yi**seçin. 
   Sözleşmeniz için **Ev Sahibi İş Ortağı,** **Ev Sahibi Kimliği,** **Konuk İş Ortağı**ve **Konuk Kimliği'ni** seçin. 
   Daha fazla özellik ayrıntıları için bu adımdaki tabloya bakın.

    ![Anlaşma ayrıntılarını sağlama](./media/logic-apps-enterprise-integration-x12/x12-1.png)  

    | Özellik | Açıklama |
    | --- | --- |
    | Adı |Anlaşmanın adı |
    | Anlaşma Türü | X12 olmalıdır |
    | Ev Sahibi Ortak |Bir anlaşmanın hem ev sahibi hem de konuk ortak olması gerekir. Ana bilgisayar ortağı, anlaşmayı yapılandıran kuruluşu temsil eder. |
    | Ev Sahibi Kimliği |Ev sahibi ortak için bir tanımlayıcı |
    | Misafir Ortak |Bir anlaşmanın hem ev sahibi hem de konuk ortak olması gerekir. Konuk ortak, ev sahibi ortakla iş yapan kuruluşu temsil eder. |
    | Misafir Kimliği |Konuk ortak için bir tanımlayıcı |
    | Ayarları Al |Bu özellikler, bir anlaşma tarafından alınan tüm iletiler için geçerlidir. |
    | Ayarları Gönder |Bu özellikler, bir anlaşma tarafından gönderilen tüm iletiler için geçerlidir. |  

   > [!NOTE]
   > X12 anlaşmasının çözümü gönderen niteleyici ve tanımlayıcı ile ortak ve gelen iletide tanımlanan alıcı niteleyicive tanımlayıcısının eşleşmesine bağlıdır. Bu değerler ortağınız için değişirse, sözleşmeyi de güncelleştirin.

## <a name="configure-how-your-agreement-handles-received-messages"></a>Anlaşmanızın alınan iletileri nasıl işleyeceğini yapılandırma

Artık sözleşme özelliklerini ayarladığınızda, bu anlaşmanın bu anlaşma aracılığıyla ortağınızdan alınan gelen iletileri nasıl tanımladığını ve işleyeceğini yapılandırabilirsiniz.

1.  **Ekle'nin** **altında, Ayarları Al'ı**seçin.
Bu özellikleri, sizinle ileti alışverişinde bulunan iş ortağıyla yaptığınız anlaşmaya göre yapılandırın. Özellik açıklamaları için bu bölümdeki tablolara bakın.

    **Alma Ayarları** şu bölümlerde düzenlenir: Tanımlayıcılar, Bildirim, Şemalar, Zarflar, Denetim Numaraları, Doğrulamalar ve Dahili Ayarlar.

2. Işiniz bittikten sonra **Tamam'ı**seçerek ayarlarınızı kaydettiğinizden emin olun.

Artık sözleşmeniz, seçtiğiniz ayarlara uygun gelen iletileri işlemeye hazırdır.

### <a name="identifiers"></a>Tanımlayıcılar

![Tanımlayıcı özelliklerini ayarlama](./media/logic-apps-enterprise-integration-x12/x12-2.png)  

| Özellik | Açıklama |
| --- | --- |
| ISA1 (Yetkilendirme Elemesi) |Açılan listeden Yetkilendirme niteleyici değerini seçin. |
| ISA2 |İsteğe bağlı. Yetkilendirme bilgi değerini girin. ISA1 için girdiğiniz değer 00'dan küçükse, en az bir alfasayısal karakter ve en fazla 10 karakter girin. |
| ISA3 (Güvenlik Elemesi) |Açılan listeden Güvenlik niteleme değerini seçin. |
| ISA4 |İsteğe bağlı. Güvenlik bilgi değerini girin. ISA3 için girdiğiniz değer 00'dan küçükse, en az bir alfasayısal karakter ve en fazla 10 karakter girin. |

### <a name="acknowledgment"></a>Bildirim

![Kabul özelliklerini ayarlama](./media/logic-apps-enterprise-integration-x12/x12-3.png) 

| Özellik | Açıklama |
| --- | --- |
| TA1 bekleniyor |Değişim gönderene teknik bir bildirim verir |
| FA bekleniyor |Değiştiriciye işlevsel bir bildirim verir. Ardından şema sürümüne göre 997 veya 999 bildirimleri isteyip istemediğiniz |
| AK2/IK2 Loop ekle |Kabul edilen işlem kümeleri için işlevsel bildirimlerde AK2 döngülerinin üretilmesini sağlar |

### <a name="schemas"></a>Şemalar

Her hareket türü (ST1) ve Gönderen Uygulaması (GS2) için bir şema seçin. Alma ardışık, gelen iletideki ST1 ve GS2 değerlerini burada ayarladığınız değerlerle ve burada ayarladığınız şema ile gelen iletinin şemalarıyla eşleştirerek gelen iletiyi söker.

![Şema seçin](./media/logic-apps-enterprise-integration-x12/x12-33.png) 

| Özellik | Açıklama |
| --- | --- |
| Sürüm |X12 sürümünü seçin |
| İşlem Türü (ST01) |Hareket türünü seçin |
| Gönderen Uygulaması (GS02) |Gönderen uygulamasını seçin |
| Şema |Kullanmak istediğiniz şema dosyasını seçin. Şemalar entegrasyon hesabınıza eklenir. |

> [!NOTE]
> Entegrasyon hesabınıza yüklenen gerekli [Şema'yı](../logic-apps/logic-apps-enterprise-integration-schemas.md) [yapılandırın.](../logic-apps/logic-apps-enterprise-integration-accounts.md)

### <a name="envelopes"></a>Zarf

![Bir işlem kümesinde ayırıcıyı belirtin: Standart Tanımlayıcı veya Tekrar Ayırıcı'yı seçin](./media/logic-apps-enterprise-integration-x12/x12-34.png)

| Özellik | Açıklama |
| --- | --- |
| ISA11 Kullanımı |Ayırıcıyı bir işlem kümesinde kullanılacak belirtir: <p>EDI alan ardışık alan da gelen belgenin ondalık gösterimi yerine ondalık gösterim için bir dönem (.) kullanmak için **Standart tanımlayıcıyı** seçin. <p>Basit bir veri öğesinin veya yinelenen bir veri yapısının yinelenen oluşumları için ayırıcıyı belirtmek için **Yineleme Ayırıcısı'nı** seçin. Örneğin, genellikle karat (^) tekrar ayırıcı olarak kullanılır. HIPAA şemaları için, sadece karat kullanabilirsiniz. |

### <a name="control-numbers"></a>Kontrol numaraları

![Denetim numarası yinelenenleri nasıl işleyeceğini seçme](./media/logic-apps-enterprise-integration-x12/x12-35.png) 

| Özellik | Açıklama |
| --- | --- |
| Disallow Değişim Kontrol Numarası yinelenenleri |Yinelenen kavşakları engelleyin. Alınan değişim kontrol numarası için değişim kontrol numarasını (ISA13) denetler. Bir eşleşme algılanırsa, alma ardışık hattı değiş tokuş işlemini işlemez. *Her (gün) yinelenen ISA13 için çek*değeri vererek denetimi gerçekleştirmek için gün sayısını belirtebilirsiniz. |
| Disallow Grubu kontrol numarası yinelenenleri |Yinelenen grup denetim numaralarıyla kavşakları engelleyin. |
| İzin Verme İşlemi denetim numarası yinelenenleri ayarlayın |Yinelenen hareket kümesi denetim numaralarıyla kavşakları engelleyin. |

### <a name="validation"></a>Doğrulama

![Alınan iletiler için doğrulama özelliklerini ayarlama](./media/logic-apps-enterprise-integration-x12/x12-36.png) 

Her doğrulama satırını tamamladığınızda, otomatik olarak bir başkası eklenir. Herhangi bir kural belirtmezseniz, doğrulama "Varsayılan" satırını kullanır.

| Özellik | Açıklama |
| --- | --- |
| İleti Türü |EDI ileti türünü seçin. |
| EDI Doğrulama |Şema'nın EDI özellikleri, uzunluk kısıtlamaları, boş veri öğeleri ve son ayırıcılar tarafından tanımlanan veri türlerinde EDI doğrulaması gerçekleştirin. |
| Genişletilmiş Doğrulama |Veri türü EDI değilse, doğrulama veri öğesi gereksinimi ve izin tekrarı, sayısallaştırmalar ve veri öğesi uzunluğu doğrulama (min/max) üzerindedir. |
| Satır Aralığı/Sondaki Sıfırlar'a İzin Ver |Herhangi bir ek satır aralığı veya sondaki sıfır ve boşluk karakterleri koruyun. Bu karakterleri kaldırma. |
| Kırpma Satır Aralığı/Sondaki Sıfırlar |Satır aralığını veya sondaki sıfır ve boşluk karakterlerini kaldırın. |
| İzleyen Ayırıcı İlkesi |İzleyen ayırıcılar oluşturun. <p>Alınan değiş tokuşta sınırlayıcıları ve ayırıcıları yasaklamak için **İzin Verilmez'i** seçin. Değiş tokuşun sınırlayıcıları ve ayırıcıları varsa, değiş tokuş geçerli değil olarak bildirilir. <p>Sınır layıcılar ve ayırıcılar ile veya sınırlayıcılar olmadan kavşakları kabul etmek için **İsteğe Bağlı'yı** seçin. <p>Değiş tokuşun sınırlayıcıve ayırıcılara sahip olması gerektiğinde **Zorunlu'yu** seçin. |

### <a name="internal-settings"></a>Dahili ayarlar

![Dahili ayarları seçin](./media/logic-apps-enterprise-integration-x12/x12-37.png) 

| Özellik | Açıklama |
| --- | --- |
| Zımni ondalık biçimi "Nn" taban 10 sayısal değere dönüştürme |"Nn" biçimiyle belirtilen bir EDI numarasını taban-10 sayısal değere dönüştürür |
| İzleyen ayırıcılara izin veriliyorsa boş XML etiketleri oluşturma |Değiş tokuş gönderenin ayırıcıları takip etmek için boş XML etiketleri eklemesi için bu onay kutusunu seçin. |
| İşlem setleri olarak Kavşağı Böl - hata üzerine işlem kümelerini askıya alma|Bir kavşakta ayarlanan her hareketi, uygun zarfı hareket kümesine uygulayarak ayrı bir XML belgesine ayırır. Yalnızca doğrulamanın başarısız olduğu hareketleri askıya asıtır. |
| İşlem setleri olarak Bölme Kavşağı - hata üzerinde değişimi askıya alma|Bir kavşakta ayarlanan her hareketi, uygun zarfı uygulayarak ayrı bir XML belgesine ayırır. Bir veya daha fazla işlem ayarlandığında, bir veya daha fazla işlem doğrulamayı başarısız olduğunda tüm değiş todeğişikliği askıya alır. | 
| Etkileşimi Koru - hata üzerine işlem kümelerini askıya alma |Değişimi bozulmadan bırakır, toplu tüm değişim için bir XML belgesi oluşturur. Diğer tüm hareket kümelerini işlemeye devam ederken yalnızca doğrulamada başarısız olan hareket kümelerini askıya alır. |
| Değişimi Koru - hata yla ilgili değişimi askıya alın |Değişimi bozulmadan bırakır, toplu tüm değişim için bir XML belgesi oluşturur. Bir veya daha fazla işlem geçiş doğrulama başarısız olduğunda tüm kavşak askıya alır. |

## <a name="configure-how-your-agreement-sends-messages"></a>Anlaşmanızın iletigönderme şeklini yapılandırma

Bu anlaşmanın, bu anlaşma aracılığıyla ortağınıza gönderdiğiniz giden iletileri nasıl tanımladığını ve işleyeceğini yapılandırabilirsiniz.

1.  **Ekle'nin** **altında, Ayarlar Gönder'i**seçin.
Bu özellikleri, sizinle ileti alışverişinde bulunan ortağınızla yaptığınız anlaşmaya göre yapılandırın. Özellik açıklamaları için bu bölümdeki tablolara bakın.

    **Ayar Gönder** şu bölümlerde düzenlenir: Tanımlayıcılar, Bildirim, Şemalar, Zarflar, Karakter Kümeleri ve Ayırıcılar, Denetim Numaraları ve Doğrulama.

2. Işiniz bittikten sonra **Tamam'ı**seçerek ayarlarınızı kaydettiğinizden emin olun.

Artık sözleşmeniz, seçtiğiniz ayarlara uygun giden iletileri işlemeye hazırdır.

### <a name="identifiers"></a>Tanımlayıcılar

![Tanımlayıcı özelliklerini ayarlama](./media/logic-apps-enterprise-integration-x12/x12-4.png)  

| Özellik | Açıklama |
| --- | --- |
| Yetkilendirme niteleme (ISA1) |Açılan listeden Yetkilendirme niteleyici değerini seçin. |
| ISA2 |Yetkilendirme bilgi değerini girin. Bu değer 00'dan küçükse, en az bir alfasayısal karakter ve en fazla 10 karakter girin. |
| Güvenlik niteleyicisi (ISA3) |Açılan listeden Güvenlik niteleme değerini seçin. |
| ISA4 |Güvenlik bilgi değerini girin. Bu değer 00'dan küçükse, Değer (ISA4) metin kutusu için en az bir alfasayısal değer ve en fazla 10 değeri girin. |

### <a name="acknowledgment"></a>Bildirim

![Kabul özelliklerini ayarlama](./media/logic-apps-enterprise-integration-x12/x12-5.png)  

| Özellik | Açıklama |
| --- | --- |
| TA1 bekleniyor |Teknik bildirimi (TA1) değiş tokuş gönderene döndürün. Bu ayar, iletiyi gönderen ana bilgisayar ortağının sözleşmedeki konuk ortaktan bir bildirim istediğini belirtir. Bu bildirimler, sözleşmenin Alma Ayarları'na göre ev sahibi ortak tarafından beklenir. |
| FA bekleniyor |İşlevsel bir bildirimi (FA) değiştirimi gönderene döndürün. Birlikte çalıştığınız şema sürümlerine göre 997 veya 999 onaylarını isteyip istemediğinizi seçin. Bu bildirimler, sözleşmenin Alma Ayarları'na göre ev sahibi ortak tarafından beklenir. |
| FA Sürümü |FA sürümünü seçin |

### <a name="schemas"></a>Şemalar

![Kullanmak için şema seçin](./media/logic-apps-enterprise-integration-x12/x12-5.png)  

| Özellik | Açıklama |
| --- | --- |
| Sürüm |X12 sürümünü seçin |
| İşlem Türü (ST01) |Hareket türünü seçin |
| Şema |Kullanılacak şemayı seçin. Şemalar entegrasyon hesabınızda bulunur. Önce şema'yı seçerseniz, sürüm ve işlem türünü otomatik olarak yapılandırır  |

> [!NOTE]
> Entegrasyon hesabınıza yüklenen gerekli [Şema'yı](../logic-apps/logic-apps-enterprise-integration-schemas.md) [yapılandırın.](../logic-apps/logic-apps-enterprise-integration-accounts.md)

### <a name="envelopes"></a>Zarf

![Bir işlem kümesinde ayırıcıyı belirtin: Standart Tanımlayıcı veya Tekrar Ayırıcı'yı seçin](./media/logic-apps-enterprise-integration-x12/x12-6.png) 

| Özellik | Açıklama |
| --- | --- |
| ISA11 Kullanımı |Ayırıcıyı bir işlem kümesinde kullanılacak belirtir: <p>EDI alan ardışık alan da gelen belgenin ondalık gösterimi yerine ondalık gösterim için bir dönem (.) kullanmak için **Standart tanımlayıcıyı** seçin. <p>Basit bir veri öğesinin veya yinelenen bir veri yapısının yinelenen oluşumları için ayırıcıyı belirtmek için **Yineleme Ayırıcısı'nı** seçin. Örneğin, genellikle karat (^) tekrar ayırıcı olarak kullanılır. HIPAA şemaları için, sadece karat kullanabilirsiniz. |

### <a name="control-numbers"></a>Kontrol numaraları

![Denetim numarası özelliklerini belirtin](./media/logic-apps-enterprise-integration-x12/x12-8.png) 

| Özellik | Açıklama |
| --- | --- |
| Kontrol Sürüm Numarası (ISA12) |X12 standardının sürümünü seçin |
| Kullanım Göstergesi (ISA15) |Bir değiş tokuşun bağlamını seçin.  Değerler bilgi, üretim verileri veya test verileridir |
| Şema |Gönder Boru Hattı'na gönderdiği X12 kodlu bir değişim için GS ve ST segmentleri oluşturur |
| GS1 |İsteğe bağlı olarak, açılan listeden işlevsel kod için bir değer seçin |
| GS2 |İsteğe bağlı, uygulama gönderen |
| GS3 |İsteğe bağlı, uygulama alıcısı |
| GS4 |İsteğe bağlı, CCYYMMDD veya YYMMDD seçin |
| GS5 |İsteğe bağlı, HHMM, HHMMSS veya HHMMSSdd'yi seçin |
| GS7 |İsteğe bağlı olarak, açılan listeden sorumlu kuruluş için bir değer seçin |
| GS8 |İsteğe bağlı, belgenin sürümü |
| Değişim Kontrol Numarası (ISA13) |Gerekli, değişim kontrol numarası için bir dizi değer girin. En az 1 ve en fazla 9999999999 9 999999999 9 9 999999999 9 9 999999999 9 9 99999999 9 9 99999999 9 9 9 999 |
| Grup Kontrol Numarası (GS06) |Gerekli, grup kontrol numarası için bir sayı aralığı girin. En az 1 ve en fazla 9999999999 9 999999999 9 9 999999999 9 9 999999999 9 9 99999999 9 9 99999999 9 9 9 999 |
| İşlem Seti Kontrol Numarası (ST02) |Gerekli, Hareket Kümesi Denetim numarası için bir sayı aralığı girin. En az 1 ve en fazla 9999999999 9 999999999 9 99999999 9 9 99999999 9 9 9 99999999 9 9 99999999 9 9 9 99999999 9 9 9 |
| Ön ek |İsteğe bağlı olarak, hareket kümesi denetim numaraları nın kapsamı için atanmış olarak kabul kullanılır. Orta daki iki alan için sayısal bir değer ve önek ve sonek alanları için alfasayısal bir değer (istenirse) girin. Orta alanlar gereklidir ve kontrol numarası için minimum ve maksimum değerleri içerir |
| Soneki |İsteğe bağlı olarak, bir bildirimde kullanılan hareket kümesi denetim numaraları aralığı için belirlenmiştir. Orta daki iki alan için sayısal bir değer ve önek ve sonek alanları için alfasayısal bir değer (istenirse) girin. Orta alanlar gereklidir ve kontrol numarası için minimum ve maksimum değerleri içerir |

### <a name="character-sets-and-separators"></a>Karakter kümeleri ve ayırıcılar

Karakter kümesi dışında, her ileti türü için farklı bir sınır layıcı kümesi girebilirsiniz. Belirli bir ileti şeması için bir karakter kümesi belirtilmemişse, varsayılan karakter kümesi kullanılır.

![İleti türleri için sınır layıcıları belirtin](./media/logic-apps-enterprise-integration-x12/x12-9.png) 

| Özellik | Açıklama |
| --- | --- |
| Kullanılacak Karakter Kümesi |Özellikleri doğrulamak için X12 karakter kümesini seçin. Seçenekler Temel, Genişletilmiş ve UTF8'dir. |
| Şema |Açılan listeden bir şema seçin. Her satırı tamamladıktan sonra otomatik olarak yeni bir satır eklenir. Seçili şema için, aşağıdaki ayırıcı açıklamaları temel alınarak kullanmak istediğiniz ayırıcı kümesini seçin. |
| Giriş Türü |Açılan listeden bir giriş türü seçin. |
| Bileşen Ayırıcı |Bileşik veri öğelerini ayırmak için tek bir karakter girin. |
| Veri Elemanı Ayırıcısı |Bileşik veri öğeleri içindeki basit veri öğelerini ayırmak için tek bir karakter girin. |
| Değiştirme Karakteri |Giden X12 iletisini oluştururken yük verilerindeki tüm ayırıcı karakterleri değiştirmek için kullanılan yedek bir karakter girin. |
| Segment Terminatör |BIR EDI kesiminin sonunu belirtmek için tek bir karakter girin. |
| Soneki |Segment tanımlayıcısıyla kullanılan karakteri seçin. Bir sonek belirlerseniz, segment sonlandırıcı veri öğesi boş olabilir. Segment sonlandırıcısı boş bırakılırsa, bir sonek belirlemeniz gerekir. |

> [!TIP]
> Özel karakter değerleri sağlamak için, anlaşmayı JSON olarak edin ve özel karakter için ASCII değerini sağlayın.

### <a name="validation"></a>Doğrulama

![İleti göndermek için doğrulama özelliklerini ayarlama](./media/logic-apps-enterprise-integration-x12/x12-10.png) 

Her doğrulama satırını tamamladığınızda, otomatik olarak bir başkası eklenir. Herhangi bir kural belirtmezseniz, doğrulama "Varsayılan" satırını kullanır.

| Özellik | Açıklama |
| --- | --- |
| İleti Türü |EDI ileti türünü seçin. |
| EDI Doğrulama |Şema'nın EDI özellikleri, uzunluk kısıtlamaları, boş veri öğeleri ve son ayırıcılar tarafından tanımlanan veri türlerinde EDI doğrulaması gerçekleştirin. |
| Genişletilmiş Doğrulama |Veri türü EDI değilse, doğrulama veri öğesi gereksinimi ve izin tekrarı, sayısallaştırmalar ve veri öğesi uzunluğu doğrulama (min/max) üzerindedir. |
| Satır Aralığı/Sondaki Sıfırlar'a İzin Ver |Herhangi bir ek satır aralığı veya sondaki sıfır ve boşluk karakterleri koruyun. Bu karakterleri kaldırma. |
| Kırpma Satır Aralığı/Sondaki Sıfırlar |Satır aralığını veya sondaki sıfır karakterleri kaldırın. |
| İzleyen Ayırıcı İlkesi |İzleyen ayırıcılar oluşturun. <p>Gönderilen kavşakta sınırlayıcıları ve ayırıcıları yasaklamak için **İzin Verilmez'i** seçin. Değiş tokuşun sınırlayıcıları ve ayırıcıları varsa, değiş tokuş geçerli değil olarak bildirilir. <p>Sınır layıcılar ve ayırıcılar ile veya olmadan kavşak göndermek için **İsteğe Bağlı'yı** seçin. <p>Gönderilen değiş tokuşun sınırlayıcıve ayırıcıları olması gerekiyorsa **Zorunlu'yu** seçin. |

## <a name="find-your-created-agreement"></a>Oluşturduğunuz sözleşmeyi bulma

1.  Tüm anlaşma özelliklerinizi ayarlamayı bitirdikten sonra, **Ekle** sayfasında, anlaşmanızı oluşturmayı bitirmek ve entegrasyon hesabınıza dönmek için **Tamam'ı** seçin.

    Yeni eklenen sözleşmeniz artık **Anlaşmalar** listenizde görünür.

2.  Anlaşmalarınızı entegrasyon hesabınıza genel bakışta da görüntüleyebilirsiniz. Entegrasyon hesabı menüsünde **Genel Bakış'ı**seçin ve **ardından Anlaşmalar** döşemesini seçin.

    !["Anlaşmalar" döşemesini seçin](./media/logic-apps-enterprise-integration-x12/x12-1-5.png)   

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Bu bağlayıcı hakkında bağlayıcının Swagger dosyasında açıklandığı gibi eylemler ve sınırlar gibi daha fazla teknik ayrıntı için [bağlayıcının başvuru sayfasına](https://docs.microsoft.com/connectors/x12/)bakın. 

> [!NOTE]
> [Bir entegrasyon hizmeti ortamındaki (İmKB)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md)mantık uygulamaları için, bu bağlayıcının İmKB etiketli sürümü bunun yerine [İmKB ileti sınırlarını](../logic-apps/logic-apps-limits-and-config.md#message-size-limits) kullanır.

## <a name="next-steps"></a>Sonraki adımlar

* Diğer [Logic Apps bağlayıcıları](../connectors/apis-list.md) hakkında bilgi edinin