---
title: X12 iletilerini kodla
description: X12 ileti Kodlayıcısı ile EDI 'yı doğrula ve Enterprise Integration Pack ile Azure Logic Apps XML kodlu iletileri Dönüştür
services: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, logicappspm
ms.topic: article
ms.date: 01/27/2017
ms.openlocfilehash: c87a61dc77e656a1cfe667ce87f852303a0cc486
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "74792355"
---
# <a name="encode-x12-messages-in-azure-logic-apps-with-enterprise-integration-pack"></a>Enterprise Integration Pack iletileri Azure Logic Apps kodlama

Encode x12 ileti Bağlayıcısı ile EDI ve iş ortaklarına özgü özellikleri doğrulayabilir, XML kodlu iletileri değişim içindeki EDI işlem kümelerine dönüştürebilir ve teknik bir onay, Işlevsel bildirim veya her ikisini de isteyebilirsiniz.
Bu bağlayıcıyı kullanmak için, mantıksal uygulamanızdaki mevcut bir tetikleyiciye bağlayıcıyı eklemeniz gerekir.

## <a name="before-you-start"></a>Başlamadan önce

İhtiyacınız olan öğeler aşağıda verilmiştir:

* Bir Azure hesabı; [ücretsiz bir hesap](https://azure.microsoft.com/free) oluşturabilirsiniz
* Zaten tanımlanmış ve Azure aboneliğinizle ilişkilendirilen bir [tümleştirme hesabı](logic-apps-enterprise-integration-create-integration-account.md) . Encode x12 ileti bağlayıcısını kullanmak için bir tümleştirme hesabınız olmalıdır.
* Tümleştirme hesabınızda zaten tanımlanmış olan en az iki [iş ortağı](logic-apps-enterprise-integration-partners.md)
* Tümleştirme hesabınızda zaten tanımlanmış olan bir [x12 sözleşmesi](logic-apps-enterprise-integration-x12.md)

## <a name="encode-x12-messages"></a>X12 iletilerini kodla

1. [Mantıksal uygulama oluşturun](quickstart-create-first-logic-app-workflow.md).

2. Encode x12 Message bağlayıcısının tetikleyicisi yoktur, bu nedenle bir Istek tetikleyicisi gibi mantıksal uygulamanızı başlatmak için bir tetikleyici eklemeniz gerekir. Mantıksal uygulama tasarımcısında bir tetikleyici ekleyin ve sonra mantıksal uygulamanıza bir eylem ekleyin.

3.  Arama kutusuna filtreniz için "x12" yazın. **X12-kodla, anlaşma adına göre x12** ya da **x12-encode ' i seçerek Iletileri kimliklere göre x12**.
   
    !["X12" araması yapın](./media/logic-apps-enterprise-integration-x12-encode/x12decodeimage1.png) 

3. Daha önce tümleştirme hesabınızla bağlantı oluşturmadıysanız, bu bağlantıyı şimdi oluşturmanız istenir. Bağlantınızı adlandırın ve bağlamak istediğiniz tümleştirme hesabını seçin. 
   
    ![Tümleştirme hesabı bağlantısı](./media/logic-apps-enterprise-integration-x12-encode/x12encodeimage1.png)

    Yıldız işareti olan özellikler gereklidir.

    | Özellik | Ayrıntılar |
    | --- | --- |
    | Bağlantı adı * |Bağlantınız için bir ad girin. |
    | Tümleştirme hesabı * |Tümleştirme hesabınız için bir ad girin. Tümleştirme hesabınızın ve mantıksal uygulamanızın aynı Azure konumunda olduğundan emin olun. |

5.  İşiniz bittiğinde, bağlantı ayrıntılarınız bu örneğe benzer görünmelidir. Bağlantınızın oluşturulmasını tamamlaması için **Oluştur**' u seçin.

    ![Tümleştirme hesabı bağlantısı oluşturuldu](./media/logic-apps-enterprise-integration-x12-encode/x12encodeimage2.png)

    Bağlantınız artık oluşturuldu.

    ![Tümleştirme hesabı bağlantı ayrıntıları](./media/logic-apps-enterprise-integration-x12-encode/x12encodeimage3.png) 

#### <a name="encode-x12-messages-by-agreement-name"></a>Anlaşma adına göre x12 iletilerini kodla

X12 iletilerini anlaşma adına göre kodlamayı seçtiyseniz, **x12 anlaşma listesinin adını** açın, var olan x12 sözleşmenizi girin veya seçin. Kodlanacak XML iletisini girin.

![Kodlamak için x12 anlaşma adı ve XML iletisi girin](./media/logic-apps-enterprise-integration-x12-encode/x12encodeimage4.png)

#### <a name="encode-x12-messages-by-identities"></a>Kimliklere göre x12 iletilerini kodla

X12 iletilerini kimliklere göre kodlamayı seçerseniz, x12 sözleşmenizde yapılandırıldığı şekilde gönderen tanımlayıcı, gönderici niteleyicisi, alıcı kimliği ve alıcı niteleyicisi girin. Kodlanacak XML iletisini seçin.
   
![Gönderen ve alıcı için kimlikler sağlayın, kodlanacak XML iletisini seçin](./media/logic-apps-enterprise-integration-x12-encode/x12encodeimage5.png) 

## <a name="x12-encode-details"></a>X12 kodlama ayrıntıları

X12 encode Bağlayıcısı şu görevleri gerçekleştirir:

* Gönderen ve alıcı bağlamı özellikleriyle eşleşen sözleşme çözümlemesi.
* XML kodlu iletileri değişim içindeki EDI işlem kümelerine dönüştürerek EDI Interchange 'i seri hale getirir.
* İşlem kümesi üst bilgisi ve treyler segmentlerini uygular
* Her giden değişim için bir değişim denetim numarası, bir Grup denetim numarası ve bir işlem kümesi denetim numarası üretir
* Yük verilerinde ayırıcıları değiştirir
* EDI ve iş ortaklarına özgü özellikleri doğrular
  * İleti şemasına karşı işlem kümesi veri öğeleri için şema doğrulaması
  * EDI doğrulaması işlem kümesi veri öğelerinde gerçekleştirildi.
  * İşlem kümesi veri öğelerinde Genişletilmiş Doğrulama gerçekleştirildi
* Bir teknik ve/veya Işlevsel onay (yapılandırıldıysa) ister.
  * Teknik bildirim, üst bilgi doğrulamanın sonucu olarak oluşturulur. Teknik bildirim, bir değişim üst bilgisinin ve adres alıcısının artmesinin durumunu bildirir
  * Işlevsel bir bildirim, gövde doğrulamanın sonucu olarak oluşturulur. Alınan belgeyi işlerken işlevsel bildirim, karşılaşılan her hatayı raporlar

## <a name="view-the-swagger"></a>Swagger 'yi görüntüleme
[Swagger ayrıntılarına](/connectors/x12/)bakın. 

## <a name="next-steps"></a>Sonraki adımlar
[Enterprise Integration Pack hakkında daha fazla bilgi edinin](logic-apps-enterprise-integration-overview.md "Enterprise Integration Pack hakkında bilgi edinin") 

