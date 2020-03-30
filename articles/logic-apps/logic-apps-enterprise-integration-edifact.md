---
title: B2B entegrasyonu için EDIFACT mesajları
description: Kurumsal Tümleştirme Paketi ile Azure Logic Apps'ta B2B kurumsal tümleştirmesi için EDI formatında EDI ile değiştirin
services: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, logicappspm
ms.topic: article
ms.date: 07/26/2016
ms.openlocfilehash: 3ada12a0cde122fb78815a1d3241d8acb9da2580
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77651466"
---
# <a name="exchange-edifact-messages-for-b2b-enterprise-integration-in-azure-logic-apps-with-enterprise-integration-pack"></a>Kurumsal Tümleştirme Paketi ile Azure Logic Apps'ta B2B kurumsal tümleştirme için EDIFACT iletilerini değiştirme

EDIFACT iletilerini Azure Logic Apps için değiştiremeden önce bir EDIFACT sözleşmesi oluşturmanız ve bu sözleşmeyi tümleştirme hesabınızda depolamanız gerekir. İşte bir EDIFACT anlaşması oluşturmak için nasıl adımlar.

> [!NOTE]
> Bu sayfa Azure Mantık Uygulamaları için EDIFACT özelliklerini kapsar. Daha fazla bilgi için [Bkz. X12.](logic-apps-enterprise-integration-x12.md)

## <a name="before-you-start"></a>Başlamadan önce

İhtiyacınız olan öğeler şunlardır:

* Azure aboneliğiniz önceden tanımlanmış ve ilişkili bir [tümleştirme hesabı](logic-apps-enterprise-integration-create-integration-account.md)  
* Tümleştirme hesabınızda zaten tanımlanmış en az iki [ortak](logic-apps-enterprise-integration-partners.md)

> [!NOTE]
> Bir anlaşma oluşturduğunuzda, iş ortağına ve iş ortağından aldığınız veya gönderdiğiniz iletilerde bulunan içeriğin anlaşma türüyle eşleşmesi gerekir.

Bir [tümleştirme hesabı oluşturduktan](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) ve [iş ortakları ekledikten](logic-apps-enterprise-integration-partners.md)sonra, aşağıdaki adımları izleyerek bir EDIFACT anlaşması oluşturabilirsiniz.

## <a name="create-an-edifact-agreement"></a>EDIFACT anlaşması oluşturma 

1. [Azure portalında](https://portal.azure.com "Azure portalında")oturum açın. 

2. Ana Azure menüsünde **Tüm hizmetler'i**seçin. Arama kutusuna "tümleştirme" girin ve ardından **Tümleştirme hesaplarını**seçin.

   ![Entegrasyon hesabınızı bulun](./media/logic-apps-enterprise-integration-edifact/edifact-0.png)

   > [!TIP]
   > **Tüm hizmetler** görünmüyorsa, önce menüyü genişletmeniz gerekebilir. Daraltılmış menünün üst kısmında **metin etiketlerini göster'i**seçin.

3. **Tümleştirme Hesapları**altında, sözleşme oluşturmak istediğiniz tümleştirme hesabını seçin.

   ![Anlaşmanın oluşturulacağı tümleştirme hesabını seçin](./media/logic-apps-enterprise-integration-edifact/edifact-1-4.png)

4. **Anlaşmaları**seçin. Anlaşmalı döşemeniz yoksa, önce döşemeyi ekleyin.   

   !["Anlaşmalar" döşemesini seçin](./media/logic-apps-enterprise-integration-edifact/edifact-1-5.png)

5. Anlaşmalar sayfasında **Ekle'yi**seçin.

   !["Ekle" seçeneğini belirleyin](./media/logic-apps-enterprise-integration-edifact/edifact-agreement-2.png)

6. **Add**, anlaşmanız için bir **Ad** girin. **Sözleşme türü için** **EDIFACT'i**seçin. Sözleşmeniz için **Ev Sahibi İş Ortağı,** **Ev Sahibi Kimliği,** **Konuk İş Ortağı**ve **Konuk Kimliği'ni** seçin.

   ![Anlaşma ayrıntılarını sağlama](./media/logic-apps-enterprise-integration-edifact/edifact-1.png)

   | Özellik | Açıklama |
   | --- | --- |
   | Adı |Anlaşmanın adı |
   | Anlaşma Türü | EDIFACT olmalı |
   | Ev Sahibi Ortak |Bir anlaşmanın hem ev sahibi hem de konuk ortak olması gerekir. Ana bilgisayar ortağı, anlaşmayı yapılandıran kuruluşu temsil eder. |
   | Ev Sahibi Kimliği |Ev sahibi ortak için bir tanımlayıcı |
   | Misafir Ortak |Bir anlaşmanın hem ev sahibi hem de konuk ortak olması gerekir. Konuk ortak, ev sahibi ortakla iş yapan kuruluşu temsil eder. |
   | Misafir Kimliği |Konuk ortak için bir tanımlayıcı |
   | Ayarları Al |Bu özellikler, bir anlaşma tarafından alınan tüm iletiler için geçerlidir. |
   | Ayarları Gönder |Bu özellikler, bir anlaşma tarafından gönderilen tüm iletiler için geçerlidir. |
   ||| 

## <a name="configure-how-your-agreement-handles-received-messages"></a>Anlaşmanızın alınan iletileri nasıl işleyeceğini yapılandırma

Artık sözleşme özelliklerini ayarladığınızda, bu anlaşmanın bu anlaşma aracılığıyla ortağınızdan alınan gelen iletileri nasıl tanımladığını ve işleyeceğini yapılandırabilirsiniz.

1. **Ekle'nin** **altında, Ayarları Al'ı**seçin.
Bu özellikleri, sizinle ileti alışverişinde bulunan iş ortağıyla yaptığınız anlaşmaya göre yapılandırın. Özellik açıklamaları için bu bölümdeki tablolara bakın.

   **Alma Ayarları** şu bölümlerde düzenlenir: Tanımlayıcılar, Bildirim, Şemalar, Denetim Numaraları, Doğrulama ve Dahili Ayarlar.

   !["Ayarları Al" yapılandırma](./media/logic-apps-enterprise-integration-edifact/edifact-2.png)  

2. Işiniz bittikten sonra **Tamam'ı**seçerek ayarlarınızı kaydettiğinizden emin olun.

Artık sözleşmeniz, seçtiğiniz ayarlara uygun gelen iletileri işlemeye hazırdır.

### <a name="identifiers"></a>Tanımlayıcılar

| Özellik | Açıklama |
| --- | --- |
| UNB6.1 (Alıcı Başvuru Parolası) |1 ile 14 karakter arasında değişen alfasayısal bir değer girin. |
| UNB6.2 (Alıcı Başvuru Elemesi) |En az bir karakter ve en fazla iki karakter içeren alfasayısal bir değer girin. |

### <a name="acknowledgments"></a>İlgili kaynaklar

| Özellik | Açıklama |
| --- | --- |
| İleti Makbuzu (CONTRL) |Teknik (CONTRL) bildirimini değişim gönderene döndürmek için bu onay kutusunu seçin. Bildirim, anlaşma için Gönder Ayarları'na göre değişim gönderene gönderilir. |
| Bildirim (CONTRL) |İşlevsel (CONTRL) bildirimini değiştiriciye döndürmek için bu onay kutusunu seçin Bildirim, anlaşma için Gönder Ayarları'nı temel alınabilmek için değişim gönderene gönderilir. |

### <a name="schemas"></a>Şemalar

| Özellik | Açıklama |
| --- | --- |
| UNH2.1 (Tİp) |Bir hareket kümesi türü seçin. |
| UNH2.2 (SÜRÜM) |İleti sürüm numarasını girin. (En az, bir karakter; en fazla üç karakter). |
| UNH2.3 (YAYIN) |İleti yayın numarasını girin. (En az, bir karakter; en fazla üç karakter). |
| UNH2.5 (İlGİlİ ATANAN KOD) |Atanan kodu girin. (Maksimum, altı karakter. Alfanümerik olmalıdır). |
| UNG2.1 (UYGULAMA GÖNDEREN Kimliği) |En az bir karakter ve en fazla 35 karakter içeren alfasayısal bir değer girin. |
| UNG2.2 (UYGULAMA GÖNDEREN KODU NITELİĞİ) |En fazla dört karakteriçeren alfasayısal bir değer girin. |
| Şema |İlişkili tümleştirme hesabınızdan kullanmak istediğiniz daha önce yüklenen şemayı seçin. |

### <a name="control-numbers"></a>Kontrol numaraları

| Özellik | Açıklama |
| --- | --- |
| Disallow Değişim Kontrol Numarası yinelenenleri |Yinelenen kavşakları engellemek için bu özelliği seçin. Seçilirse, EDIFACT Decode Action alınan değişim için değişim kontrol numarasının (UNB5) daha önce işlenmiş bir değişim kontrol numarasıyla eşleşmediğini denetler. Bir eşleşme algılanırsa, değiş tokuş işlenmez. |
| Yinelenen UNB5'i her (gün) denetleyin |Yinelenen değişim denetim numaralarına izin vermemeyi seçtiyseniz, bu ayar için uygun değeri vererek denetimi gerçekleştireceğiniz gün sayısını belirtebilirsiniz. |
| Disallow Grubu kontrol numarası yinelenenleri |Yinelenen grup denetim numaralarıyla (UNG5) kavşakları engellemek için bu özelliği seçin. |
| İzin Verme İşlemi denetim numarası yinelenenleri ayarlayın |Yinelenen işlem kümesi denetim numaraları (UNH1) ile kavşakları engellemek için bu özelliği seçin. |
| EDIFACT Onay Kontrol Numarası |Bir bildirimde kullanılmak üzere hareket kümesi referans numaralarını belirlemek için önek için bir değer, bir referans numarası aralığı ve bir sonek girin. |

### <a name="validation"></a>Doğrulama

Her doğrulama satırını tamamladığınızda, otomatik olarak bir başkası eklenir. Herhangi bir kural belirtmezseniz, doğrulama "Varsayılan" satırını kullanır.

| Özellik | Açıklama |
| --- | --- |
| İleti Türü |EDI ileti türünü seçin. |
| EDI Doğrulama |Şema'nın EDI özellikleri, uzunluk kısıtlamaları, boş veri öğeleri ve son ayırıcılar tarafından tanımlanan veri türlerinde EDI doğrulaması gerçekleştirin. |
| Genişletilmiş Doğrulama |Veri türü EDI değilse, doğrulama veri öğesi gereksinimi ve izin tekrarı, sayısallaştırmalar ve veri öğesi uzunluğu doğrulama (min/max) üzerindedir. |
| Satır Aralığı/Sondaki Sıfırlar'a İzin Ver |Herhangi bir ek satır aralığı veya sondaki sıfır ve boşluk karakterleri koruyun. Bu karakterleri kaldırma. |
| Kırpma Satır Aralığı/Sondaki Sıfırlar |Satır aralığını veya sondaki sıfır ve boşluk karakterlerini kaldırın. |
| İzleyen Ayırıcı İlkesi |İzleyen ayırıcılar oluşturun. <p>Alınan değiş tokuşta sınırlayıcıları ve ayırıcıları yasaklamak için **İzin Verilmez'i** seçin. Değiş tokuşun sınırlayıcıları ve ayırıcıları varsa, değiş tokuş geçerli değil olarak bildirilir. <p>Sınır layıcılar ve ayırıcılar ile veya sınırlayıcılar olmadan kavşakları kabul etmek için **İsteğe Bağlı'yı** seçin. <p>Alınan değiş tokuşun sınırlayıcıve ayırıcılara sahip olması gerektiğinde **Zorunlu'yu** seçin. |

### <a name="internal-settings"></a>Dahili ayarlar

| Özellik | Açıklama |
| --- | --- |
| İzleyen ayırıcılara izin veriliyorsa boş XML etiketleri oluşturma |Değiş tokuş gönderenin ayırıcıları takip etmek için boş XML etiketleri eklemesi için bu onay kutusunu seçin. |
| İşlem setleri olarak Kavşağı Böl - hata üzerine işlem kümelerini askıya alma|Bir kavşakta ayarlanan her hareketi, uygun zarfı hareket kümesine uygulayarak ayrı bir XML belgesine ayırır. Yalnızca doğrulamayı başarısız olan hareket kümelerini askıya alın. |
| İşlem setleri olarak Bölme Kavşağı - hata üzerinde değişimi askıya alma|Bir kavşakta ayarlanan her hareketi, uygun zarfı uygulayarak ayrı bir XML belgesine ayırır. Bir veya daha fazla işlem ayarlandığında tüm değiş todeğişikliği askıya alın geçerliliğini yerine lendirmek. | 
| Etkileşimi Koru - hata üzerine işlem kümelerini askıya alma |Değişimi bozulmadan bırakır, toplu tüm değişim için bir XML belgesi oluşturur. Diğer tüm hareket kümelerini işlemeye devam ederken yalnızca doğrulamayı başarısız olan hareket kümelerini askıya alın. |
| Değişimi Koru - hata yla ilgili değişimi askıya alın |Değişimi bozulmadan bırakır, toplu tüm değişim için bir XML belgesi oluşturur. Bir veya daha fazla işlem ayarlandığında tüm değiş todeğişikliği askıya alın geçerliliğini yerine lendirmek. |

## <a name="configure-how-your-agreement-sends-messages"></a>Anlaşmanızın iletigönderme şeklini yapılandırma

Bu anlaşmanın iş ortaklarınıza gönderdiğiniz giden iletileri nasıl tanımladığını ve işleyeceğini bu anlaşma yla yapılandırabilirsiniz.

1.  **Ekle'nin** **altında, Ayarlar Gönder'i**seçin.
Bu özellikleri, sizinle ileti alışverişinde bulunan ortağınızla yaptığınız anlaşmaya göre yapılandırın. Özellik açıklamaları için bu bölümdeki tablolara bakın.

    **Ayar Gönder** şu bölümlerde düzenlenir: Tanımlayıcılar, Bildirim, Şemalar, Zarflar, Karakter Kümeleri ve Ayırıcılar, Denetim Numaraları ve Doğrulamalar.

    !["Ayarları Gönder" yapılandırma](./media/logic-apps-enterprise-integration-edifact/edifact-3.png)    

2. Işiniz bittikten sonra **Tamam'ı**seçerek ayarlarınızı kaydettiğinizden emin olun.

Artık sözleşmeniz, seçtiğiniz ayarlara uygun giden iletileri işlemeye hazırdır.

### <a name="identifiers"></a>Tanımlayıcılar

| Özellik | Açıklama |
| --- | --- |
| UNB1.2 (Sözdizimi sürümü) |**1** ile **4**arasında bir değer seçin. |
| UNB2.3 (Gönderen Ters Yönlendirme Adresi) |En az bir karakter ve en fazla 14 karakter içeren alfasayısal bir değer girin. |
| UNB3.3 (Alıcı Ters Yönlendirme Adresi) |En az bir karakter ve en fazla 14 karakter içeren alfasayısal bir değer girin. |
| UNB6.1 (Alıcı Başvuru Parolası) |En az bir ve en fazla 14 karakter içeren alfasayısal bir değer girin. |
| UNB6.2 (Alıcı Başvuru Elemesi) |En az bir karakter ve en fazla iki karakter içeren alfasayısal bir değer girin. |
| UNB7 (Başvuru Referans Kimliği) |En az bir karakter ve en fazla 14 karakter içeren alfasayısal bir değer girin |

### <a name="acknowledgment"></a>Bildirim

| Özellik | Açıklama |
| --- | --- |
| İleti Makbuzu (CONTRL) |Barındırılan iş ortağı teknik (CONTRL) bir bildirim almayı bekliyorsa bu onay kutusunu seçin. Bu ayar, ileti gönderen barındırılan iş ortağının konuk iş ortağından onay istediğini belirtir. |
| Bildirim (CONTRL) |Barındırılan iş ortağı işlevsel bir (CONTRL) bildirim almayı bekliyorsa bu onay kutusunu seçin. Bu ayar, ileti gönderen barındırılan iş ortağının konuk iş ortağından onay istediğini belirtir. |
| Kabul edilen işlem kümeleri için SG1/SG4 döngüsü oluşturma |İşlevsel bir onay talep etmeyi seçtiyseniz, kabul edilen işlem kümeleri için işlevsel CONTRL bildirimlerinde SG1/SG4 döngülerinin üretilmeye zorlamak için bu onay kutusunu seçin. |

### <a name="schemas"></a>Şemalar

| Özellik | Açıklama |
| --- | --- |
| UNH2.1 (Tİp) |Bir hareket kümesi türü seçin. |
| UNH2.2 (SÜRÜM) |İleti sürüm numarasını girin. |
| UNH2.3 (YAYIN) |İleti yayın numarasını girin. |
| Şema |Kullanılacak şemayı seçin. Şemalar entegrasyon hesabınızda bulunur. Şemalarınıza erişmek için öncelikle entegrasyon hesabınızı Mantık uygulamanıza bağla. |

### <a name="envelopes"></a>Zarf

| Özellik | Açıklama |
| --- | --- |
| UNB8 (İşleme Öncelik Kodu) |Birden fazla karakter uzunluğunda olmayan alfabetik bir değer girin. |
| UNB10 (İletişim Anlaşması) |En az bir karakter ve en fazla 40 karakter içeren alfasayısal bir değer girin. |
| UNB11 (Test Göstergesi) |Oluşturulan değişimin test verileri olduğunu belirtmek için bu onay kutusunu seçin |
| UNA Segmentuygula (Service String Advice) |Bu onay kutusunu seçerek, kavşak ların gönderilmesi için bir UNA kesimi oluşturun. |
| UNG Segmentleri Uygula (Fonksiyon Grubu Başlığı) |Konuk iş ortağına gönderilen iletilerde işlevsel grup başlığında gruplandırma segmentleri oluşturmak için bu onay kutusunu seçin. AŞAĞıDAKI değerler UNG segmentleri oluşturmak için kullanılır: <p>**UNG1**için, en az bir karakter ve en fazla altı karakter içeren alfasayısal bir değer girin. <p>**UNG2.1**için, en az bir karakter ve en fazla 35 karakter içeren alfasayısal bir değer girin. <p>**UNG2.2**için, en fazla dört karakteriçeren alfasayısal bir değer girin. <p>**UNG3.1**için, en az bir karakter ve en fazla 35 karakter içeren alfasayısal bir değer girin. <p>**UNG3.2**için, en fazla dört karakteriçeren alfasayısal bir değer girin. <p>**UNG6**için, en az bir ve en fazla üç karakter içeren alfasayısal bir değer girin. <p>**UNG7.1**için, en az bir karakter ve en fazla üç karakter içeren alfasayısal bir değer girin. <p>**UNG7.2**için, en az bir karakter ve en fazla üç karakter içeren alfasayısal bir değer girin. <p>**UNG7.3**için, en az 1 karakter ve en fazla 6 karakter içeren alfasayısal bir değer girin. <p>**UNG8**için, en az bir karakter ve en fazla 14 karakter içeren alfasayısal bir değer girin. |

### <a name="character-sets-and-separators"></a>Karakter kümeleri ve ayırıcılar

Karakter kümesi dışında, her ileti türü için kullanılacak farklı bir sınır layıcı kümesi girebilirsiniz. Belirli bir ileti şeması için bir karakter kümesi belirtilmemişse, varsayılan karakter kümesi kullanılır.

| Özellik | Açıklama |
| --- | --- |
| UNB1.1 (Sistem Tanımlayıcısı) |Giden kavşakta uygulanacak EDIFACT karakter kümesini seçin. |
| Şema |Açılan listeden bir şema seçin. Her satırı tamamladıktan sonra otomatik olarak yeni bir satır eklenir. Seçili şema için, aşağıdaki ayırıcı açıklamaları temel alınarak kullanmak istediğiniz ayırıcı kümesini seçin. |
| Giriş Türü |Açılan listeden bir giriş türü seçin. |
| Bileşen Ayırıcı |Bileşik veri öğelerini ayırmak için tek bir karakter girin. |
| Veri Elemanı Ayırıcısı |Bileşik veri öğeleri içindeki basit veri öğelerini ayırmak için tek bir karakter girin. |
| Segment Terminatör |BIR EDI kesiminin sonunu belirtmek için tek bir karakter girin. |
| Soneki |Segment tanımlayıcısıyla kullanılan karakteri seçin. Bir sonek belirlerseniz, segment sonlandırıcı veri öğesi boş olabilir. Segment sonlandırıcısı boş bırakılırsa, bir sonek belirlemeniz gerekir. |

### <a name="control-numbers"></a>Kontrol numaraları

| Özellik | Açıklama |
| --- | --- |
| UNB5 (Değişim Kontrol Numarası) |Bir önek, değişim denetim numarası için bir değer aralığı ve bir sonek girin. Bu değerler giden bir değiş tokuş oluşturmak için kullanılır. Önek ve sonek isteğe bağlıdır, denetim numarası ise gereklidir. Denetim numarası her yeni ileti için artımlanır; önek ve sonek aynı kalır. |
| UNG5 (Grup Kontrol Numarası) |Bir önek, değişim denetim numarası için bir değer aralığı ve bir sonek girin. Bu değerler grup denetim numarasını oluşturmak için kullanılır. Önek ve sonek isteğe bağlıdır, denetim numarası ise gereklidir. Denetim numarası, en yüksek değere ulaşılıncaya kadar her yeni ileti için artımlanır; önek ve sonek aynı kalır. |
| UNH1 (İleti Üstbilgi Başvuru Numarası) |Bir önek, değişim denetim numarası için bir değer aralığı ve bir sonek girin. Bu değerler ileti üstbilgi başvuru numarasını oluşturmak için kullanılır. Önek ve sonek isteğe bağlıdır, referans numarası ise gereklidir. Başvuru numarası her yeni ileti için artımlı; önek ve sonek aynı kalır. |

### <a name="validation"></a>Doğrulama

Her doğrulama satırını tamamladığınızda, otomatik olarak bir başkası eklenir. Herhangi bir kural belirtmezseniz, doğrulama "Varsayılan" satırını kullanır.

| Özellik | Açıklama |
| --- | --- |
| İleti Türü |EDI ileti türünü seçin. |
| EDI Doğrulama |Şemanın EDI özellikleri, uzunluk kısıtlamaları, boş veri öğeleri ve ayırıcılar tarafından tanımlanan veri türlerinde EDI doğrulaması gerçekleştirin. |
| Genişletilmiş Doğrulama |Veri türü EDI değilse, doğrulama veri öğesi gereksinimi ve izin tekrarı, sayısallaştırmalar ve veri öğesi uzunluğu doğrulama (min/max) üzerindedir. |
| Satır Aralığı/Sondaki Sıfırlar'a İzin Ver |Herhangi bir ek satır aralığı veya sondaki sıfır ve boşluk karakterleri koruyun. Bu karakterleri kaldırma. |
| Kırpma Satır Aralığı/Sondaki Sıfırlar |Satır aralığını veya sondaki sıfır karakterleri kaldırın. |
| İzleyen Ayırıcı İlkesi |İzleyen ayırıcılar oluşturun. <p>Gönderilen kavşakta sınırlayıcıları ve ayırıcıları yasaklamak için **İzin Verilmez'i** seçin. Değiş tokuşun sınırlayıcıları ve ayırıcıları varsa, değiş tokuş geçerli değil olarak bildirilir. <p>Sınır layıcılar ve ayırıcılar ile veya olmadan kavşak göndermek için **İsteğe Bağlı'yı** seçin. <p>Gönderilen değiş tokuşun sınırlayıcıve ayırıcıları olması gerekiyorsa **Zorunlu'yu** seçin. |

## <a name="find-your-created-agreement"></a>Oluşturduğunuz sözleşmeyi bulma

1.  Tüm anlaşma özelliklerinizi ayarlamayı bitirdikten sonra, **Ekle** sayfasında, anlaşmanızı oluşturmayı bitirmek ve entegrasyon hesabınıza dönmek için **Tamam'ı** seçin.

    Yeni eklenen sözleşmeniz artık **Anlaşmalar** listenizde görünür.

2.  Anlaşmalarınızı entegrasyon hesabınıza genel bakışta da görüntüleyebilirsiniz. Entegrasyon hesabı menüsünde **Genel Bakış'ı**seçin ve **ardından Anlaşmalar** döşemesini seçin. 

    !["Anlaşmalar" döşemesini seçin](./media/logic-apps-enterprise-integration-edifact/edifact-4.png)   

## <a name="connector-reference"></a>Bağlayıcı başvurusu

Bu bağlayıcı hakkında bağlayıcının Swagger dosyasında açıklandığı gibi eylemler ve sınırlar gibi daha fazla teknik ayrıntı için [bağlayıcının başvuru sayfasına](https://docs.microsoft.com/connectors/edifact/)bakın.

> [!NOTE]
> [Bir entegrasyon hizmeti ortamındaki (İmKB)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md)mantık uygulamaları için, bu bağlayıcının İmKB etiketli sürümü bunun yerine [İmKB ileti sınırlarını](../logic-apps/logic-apps-limits-and-config.md#message-size-limits) kullanır.

## <a name="next-steps"></a>Sonraki adımlar

* Diğer [Logic Apps bağlayıcıları](../connectors/apis-list.md) hakkında bilgi edinin