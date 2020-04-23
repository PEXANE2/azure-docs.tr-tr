---
title: EDIOLGU iletilerinin kodunu çözme
description: EDI 'yi doğrulayın ve Enterprise Integration Pack Azure Logic Apps için EDIOLGU ileti kod çözücüsüyle bildirimleri oluşturun
services: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, divswa, logicappspm
ms.topic: article
ms.date: 04/22/2020
ms.openlocfilehash: c32b3ee5c4689e960834d543de1ca377e918751d
ms.sourcegitcommit: 354a302d67a499c36c11cca99cce79a257fe44b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2020
ms.locfileid: "82106296"
---
# <a name="decode-edifact-messages-for-azure-logic-apps-with-the-enterprise-integration-pack"></a>Enterprise Integration Pack ile Azure Logic Apps için EDIOLGU iletilerinin kodunu çözün

DıBULGULAR ileti bağlayıcısıyla birlikte, EDI ve iş ortaklarına özgü özellikleri doğrulayabilir, değişiklikleri işlem kümelerine bölebilir veya tüm değişiklikleri koruyabilir ve işlenen işlemler için bildirimler oluşturabilirsiniz. Bu bağlayıcıyı kullanmak için, mantıksal uygulamanızdaki mevcut bir tetikleyiciye bağlayıcıyı eklemeniz gerekir.

## <a name="before-you-start"></a>Başlamadan önce

İhtiyacınız olan öğeler aşağıda verilmiştir:

* Bir Azure hesabı; [ücretsiz bir hesap](https://azure.microsoft.com/free) oluşturabilirsiniz
* Zaten tanımlanmış ve Azure aboneliğinizle ilişkilendirilen bir [tümleştirme hesabı](logic-apps-enterprise-integration-create-integration-account.md) . Kod çözme EDIOLGU ileti bağlayıcısını kullanmak için bir tümleştirme hesabınız olmalıdır. 
* Tümleştirme hesabınızda zaten tanımlanmış olan en az iki [iş ortağı](logic-apps-enterprise-integration-partners.md)
* Tümleştirme hesabınızda zaten tanımlanmış olan bir [Ediolgu sözleşmesi](logic-apps-enterprise-integration-edifact.md)

## <a name="decode-edifact-messages"></a>EDIOLGU iletilerinin kodunu çözme

> [!IMPORTANT]
> EDIOLGU Bağlayıcısı yalnızca UTF-8 karakterlerini destekler.
> Çıktılarınız beklenmeyen karakterler içeriyorsa, EDIOLGU iletilerinizin UTF-8 karakter kümesini kullanıp kullandığından emin olun. 

1. [Mantıksal uygulama oluşturun](quickstart-create-first-logic-app-workflow.md).

2. Kod çözme EDIOLGU ileti bağlayıcısının tetikleyicisi yok, bu nedenle bir Istek tetikleyicisi gibi mantıksal uygulamanızı başlatmak için bir tetikleyici eklemeniz gerekir. Mantıksal uygulama tasarımcısında bir tetikleyici ekleyin ve sonra mantıksal uygulamanıza bir eylem ekleyin.

3. Arama kutusuna filtreniz olarak "EDIOLGU" yazın. **EDIOLGU Iletisinin kodunu çöz**' ü seçin.
   
    ![EDIOLGU ara](./media/logic-apps-enterprise-integration-edifact-decode/edifactdecodeimage1.png)

3. Daha önce tümleştirme hesabınızla bağlantı oluşturmadıysanız, bu bağlantıyı şimdi oluşturmanız istenir. Bağlantınızı adlandırın ve bağlamak istediğiniz tümleştirme hesabını seçin.
   
    ![Tümleştirme hesabı oluştur](./media/logic-apps-enterprise-integration-edifact-decode/edifactdecodeimage2.png)

    Yıldız işareti olan özellikler gereklidir.

    | Özellik | Ayrıntılar |
    | --- | --- |
    | Bağlantı adı * |Bağlantınız için bir ad girin. |
    | Tümleştirme hesabı * |Tümleştirme hesabınız için bir ad girin. Tümleştirme hesabınızın ve mantıksal uygulamanızın aynı Azure konumunda olduğundan emin olun. |

4. Bağlantınızı oluşturmayı bitirmeyi bitirdiğinizde **Oluştur**' u seçin. Bağlantı ayrıntılarınız Şu örneğe benzer görünmelidir:

    ![Tümleştirme hesabı ayrıntıları](./media/logic-apps-enterprise-integration-edifact-decode/edifactdecodeimage3.png)  

5. Bağlantınız oluşturulduktan sonra, bu örnekte gösterildiği gibi, kodunun çözülmesi için EDIOLGU düz dosya iletisini seçin.

    ![Tümleştirme hesabı bağlantısı oluşturuldu](./media/logic-apps-enterprise-integration-edifact-decode/edifactdecodeimage4.png)  

    Örneğin:

    ![Kod çözme için EDIOLGU düz dosya iletisini seçin](./media/logic-apps-enterprise-integration-edifact-decode/edifactdecodeimage5.png)  

## <a name="edifact-decoder-details"></a>EDIOLGU kod çözücü ayrıntıları

Kod çözme EDIOLGU Bağlayıcısı şu görevleri gerçekleştirir: 

* Zarfı, ticaret ortağı sözleşmesine karşı doğrular.
* , Gönderici niteleyicisi & tanımlayıcı ve alıcı niteleyicisi & tanımlayıcısı ile eşleştirerek sözleşmeyi çözer.
* Değişim, anlaşmanın alma ayarları yapılandırmasına göre birden fazla işlem olduğunda, bir değişimi birden çok işlem içine böler.
* Değişimi ayrıştırır.
* EDI ve ortağa özgü özellikleri doğrular, örneğin:
  * Değişim zarfı yapısının doğrulanması
  * Zarfın denetim şemasına karşı şema doğrulaması
  * İleti şemasına karşı işlem kümesi veri öğeleri için şema doğrulaması
  * EDI doğrulaması işlem kümesi veri öğelerinde gerçekleştirildi
* Değişim, Grup ve işlem kümesi denetim numaralarının tekrarlanmadığını doğrular (yapılandırıldıysa) 
  * Daha önce alınan karşılıklı değişikliklere karşı değişim denetim numarasını denetler. 
  * Grup denetim numarasını, değişim içindeki diğer grup denetim numaralarına karşı denetler. 
  * İşlem kümesi denetim numarasını bu gruptaki diğer işlem kümesi denetim numaralarına karşı denetler.
* Değişimi işlem kümelerine böler veya tüm değişimi korur:
  * Değişimi işlem kümeleri olarak Böl-hata durumunda işlem kümelerini askıya al: değişimi işlem kümelerine böler ve her işlem kümesini ayrıştırır. 
  X12 kod çözme eylemi yalnızca doğrulama `badMessages`başarısız olan işlem kümelerini çıkış yapar ve kalan işlem kümelerini olarak `goodMessages`verir.
  * Değişimi işlem kümeleri olarak Böl-hata durumunda değişimi askıya al: değişimi işlem kümelerine böler ve her işlem kümesini ayrıştırır. 
  Değişim başarısız doğrulamasında bir veya daha fazla işlem kümesi varsa, x12 kod çözme eylemi bu değişim içindeki tüm işlem kümelerini olarak `badMessages`çıkarır.
  * Değiş tokuş etme-işlem kümelerini askıya alma hata durumunda: değişimi koruma ve toplu tüm değişimi işleme. 
  X12 kod çözme eylemi yalnızca doğrulama `badMessages`başarısız olan işlem kümelerini çıkış yapar ve kalan işlem kümelerini olarak `goodMessages`verir.
  * Değişimi koru-hata durumunda değişimi askıya al: değişimi koruma ve toplu tüm değişimi işleme. 
  Değişim başarısız doğrulamasında bir veya daha fazla işlem kümesi varsa, x12 kod çözme eylemi bu değişim içindeki tüm işlem kümelerini olarak `badMessages`çıkarır.
* Teknik bir (denetim) ve/veya Işlevsel onay (yapılandırıldıysa) oluşturur.
  * Teknik bir bildirim veya CONSI ACK, alınan tüm değiş tokuş için sözdizimsel bir denetim sonucunu raporlar.
  * Bir işlevsel bildirim alınan bir değişimi veya grubu kabul eder veya reddeder

## <a name="view-swagger-file"></a>Swagger dosyasını görüntüle
EDIOLGU bağlayıcısının Swagger ayrıntılarını görüntülemek için, bkz. [ediolgu](/connectors/edifact/).

## <a name="next-steps"></a>Sonraki adımlar
[Enterprise Integration Pack hakkında daha fazla bilgi edinin](logic-apps-enterprise-integration-overview.md "Enterprise Integration Pack hakkında bilgi edinin") 

