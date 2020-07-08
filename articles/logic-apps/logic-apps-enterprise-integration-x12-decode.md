---
title: X12 iletilerinin kodunu çözme
description: Enterprise Integration Pack Azure Logic Apps için EDI doğrulama ve x12 ileti kod çözücüsü ile bildirimleri oluşturma
services: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, logicappspm
ms.topic: article
ms.date: 01/27/2017
ms.openlocfilehash: 918516a5629f8570d54c641ffc29f2367937266f
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "74792361"
---
# <a name="decode-x12-messages-in-azure-logic-apps-with-enterprise-integration-pack"></a>Enterprise Integration Pack ile Azure Logic Apps’te X12 iletilerinin kodunu çözme

Kod Çözme X12 ileti bağlayıcısıyla zarfı ticari ortak sözleşmesiyle doğrulayabilir, EDI ve iş ortağına özgü özellikleri doğrulayabilir, değişimleri işlem kümelerine ayırabilir veya değişimlerin tamamını koruyabilir ve yapılan işlemler için onaylar oluşturabilirsiniz. Bu bağlayıcıyı kullanmak için bunu mantıksal uygulamanızdaki tetikleyicilerden birine eklemeniz gerekir.

## <a name="before-you-start"></a>Başlamadan önce

İhtiyacınız olan öğeler aşağıda verilmiştir:

* Bir Azure hesabı; [ücretsiz bir hesap](https://azure.microsoft.com/free) oluşturabilirsiniz
* Zaten tanımlanmış ve Azure aboneliğinizle ilişkilendirilen bir [tümleştirme hesabı](logic-apps-enterprise-integration-create-integration-account.md) . Kod çözme x12 ileti bağlayıcısını kullanabilmeniz için bir tümleştirme hesabınız olmalıdır.
* Tümleştirme hesabınızda zaten tanımlanmış olan en az iki [iş ortağı](logic-apps-enterprise-integration-partners.md)
* Tümleştirme hesabınızda zaten tanımlanmış olan bir [x12 sözleşmesi](logic-apps-enterprise-integration-x12.md)

## <a name="decode-x12-messages"></a>X12 iletilerinin kodunu çözme

1. [Mantıksal uygulama oluşturun](quickstart-create-first-logic-app-workflow.md).

2. Kod çözme x12 ileti bağlayıcısının tetikleyicisi yoktur, bu nedenle mantıksal uygulamanızı başlatmak için bir Istek tetikleyicisi gibi bir tetikleyici eklemeniz gerekir. Mantıksal uygulama tasarımcısında bir tetikleyici ekleyin ve sonra mantıksal uygulamanıza bir eylem ekleyin.

3.  Arama kutusuna filtreniz için "x12" yazın. **X12-kodunu çöz x12 iletisini**seçin.
   
    !["X12" araması yapın](media/logic-apps-enterprise-integration-x12-decode/x12decodeimage1.png)  

3. Daha önce tümleştirme hesabınızla bağlantı oluşturmadıysanız, bu bağlantıyı şimdi oluşturmanız istenir. Bağlantınızı adlandırın ve bağlamak istediğiniz tümleştirme hesabını seçin. 

    ![Tümleştirme hesabı bağlantı ayrıntılarını sağlayın](media/logic-apps-enterprise-integration-x12-decode/x12decodeimage4.png)

    Yıldız işareti olan özellikler gereklidir.

    | Özellik | Ayrıntılar |
    | --- | --- |
    | Bağlantı adı * |Bağlantınız için bir ad girin. |
    | Tümleştirme hesabı * |Tümleştirme hesabınız için bir ad girin. Tümleştirme hesabınızın ve mantıksal uygulamanızın aynı Azure konumunda olduğundan emin olun. |

5.  İşiniz bittiğinde, bağlantı ayrıntılarınız bu örneğe benzer görünmelidir. Bağlantınızın oluşturulmasını tamamlaması için **Oluştur**' u seçin.
   
    ![Tümleştirme hesabı bağlantı ayrıntıları](media/logic-apps-enterprise-integration-x12-decode/x12decodeimage5.png) 

6. Bağlantınız oluşturulduktan sonra, bu örnekte gösterildiği gibi, kod çözmek için x12 düz dosya iletisini seçin.

    ![Tümleştirme hesabı bağlantısı oluşturuldu](media/logic-apps-enterprise-integration-x12-decode/x12decodeimage6.png) 

    Örneğin:

    ![Kod çözme için x12 düz dosya iletisi seçin](media/logic-apps-enterprise-integration-x12-decode/x12decodeimage7.png) 

   > [!NOTE]
   > İleti dizisi için gerçek ileti içeriği veya yükü, iyi veya hatalı, Base64 kodlandı. Bu nedenle, bu içeriği işleyen bir ifade belirtmeniz gerekir.
   > Aşağıda, içeriği kod görünümünde girebileceğiniz veya tasarımcıda ifade Oluşturucu kullanarak XML olarak işleyen bir örnek verilmiştir.
   > ``` json
   > "content": "@xml(base64ToBinary(item()?['Payload']))"
   > ```
   > ![İçerik örneği](media/logic-apps-enterprise-integration-x12-decode/content-example.png)
   >


## <a name="x12-decode-details"></a>X12 Kod Çözme Ayrıntıları

X12 kod çözme Bağlayıcısı şu görevleri gerçekleştirir:

* Zarfı ticari iş ortağı sözleşmesine karşı doğrular
* EDI ve iş ortaklarına özgü özellikleri doğrular
  * EDI yapısal doğrulaması ve genişletilmiş şema doğrulaması
  * Değişim zarfı yapısının doğrulanması.
  * Zarfın denetim şemasına karşı şema doğrulaması.
  * İleti şemasına karşı işlem kümesi veri öğeleri için şema doğrulaması.
  * EDI doğrulaması işlem kümesi veri öğelerinde gerçekleştirildi 
* Değişim, Grup ve işlem kümesi denetim numaralarının yinelendiğini doğrular
  * Daha önce alınan karşılıklı değişikliklere karşı değişim denetim numarasını denetler.
  * Grup denetim numarasını, değişim içindeki diğer grup denetim numaralarına karşı denetler.
  * İşlem kümesi denetim numarasını bu gruptaki diğer işlem kümesi denetim numaralarına karşı denetler.
* Değişimi işlem kümelerine böler veya tüm değişimi korur:
  * Değişimi işlem kümeleri olarak Böl-hata durumunda işlem kümelerini askıya al: değişimi işlem kümelerine böler ve her işlem kümesini ayrıştırır. 
  X12 kod çözme eylemi yalnızca doğrulama başarısız olan işlem kümelerini çıkış yapar `badMessages` ve kalan işlem kümelerini olarak verir `goodMessages` .
  * Değişimi işlem kümeleri olarak Böl-hata durumunda değişimi askıya al: değişimi işlem kümelerine böler ve her işlem kümesini ayrıştırır. 
  Değişim başarısız doğrulamasında bir veya daha fazla işlem kümesi varsa, x12 kod çözme eylemi bu değişim içindeki tüm işlem kümelerini olarak çıkarır `badMessages` .
  * Değiş tokuş etme-işlem kümelerini askıya alma hata durumunda: değişimi koruma ve toplu tüm değişimi işleme. 
  X12 kod çözme eylemi yalnızca doğrulama başarısız olan işlem kümelerini çıkış yapar `badMessages` ve kalan işlem kümelerini olarak verir `goodMessages` .
  * Değişimi koru-hata durumunda değişimi askıya al: değişimi koruma ve toplu tüm değişimi işleme. 
  Değişim başarısız doğrulamasında bir veya daha fazla işlem kümesi varsa, x12 kod çözme eylemi bu değişim içindeki tüm işlem kümelerini olarak çıkarır `badMessages` . 
* Bir teknik ve/veya Işlevsel onay üretir (yapılandırıldıysa).
  * Teknik bildirim, üst bilgi doğrulamanın sonucu olarak oluşturulur. Teknik bildirim, bir değişim üst bilgisinin işleme durumunu ve adres alıcısının artbilgisini bildirir.
  * Işlevsel bir bildirim, gövde doğrulamanın sonucu olarak oluşturulur. Alınan belgeyi işlerken işlevsel bildirim, karşılaşılan her hatayı raporlar

## <a name="view-the-swagger"></a>Swagger 'yi görüntüleme
[Swagger ayrıntılarına](/connectors/x12/)bakın. 

## <a name="next-steps"></a>Sonraki adımlar
[Enterprise Integration Pack hakkında daha fazla bilgi edinin](../logic-apps/logic-apps-enterprise-integration-overview.md "Enterprise Integration Pack hakkında bilgi edinin") 

