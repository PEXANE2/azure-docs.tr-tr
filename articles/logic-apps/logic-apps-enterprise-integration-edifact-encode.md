---
title: EDIOLGU iletilerini kodla
description: Enterprise Integration Pack ile Azure Logic Apps için, EDI 'yı doğrulayın ve EDIOLGU ileti Kodlayıcısı ile XML oluşturun
services: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, divswa, logicappspm
ms.topic: article
ms.date: 01/27/2017
ms.openlocfilehash: 257cbd0b1a68ddd2b16235e6f8dec5d5b0eb10e2
ms.sourcegitcommit: 76b48a22257a2244024f05eb9fe8aa6182daf7e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74790650"
---
# <a name="encode-edifact-messages-for-azure-logic-apps-with-enterprise-integration-pack"></a>Enterprise Integration Pack ile Azure Logic Apps için EDIOLGU iletilerini kodla

DıOLGU ileti bağlayıcısını kodlayın, EDI ve iş ortaklarına özgü özellikleri doğrulayabilir, her bir işlem kümesi için bir XML belgesi oluşturabilir ve teknik onay, Işlevsel bildirim veya her ikisini de isteyebilirsiniz.
Bu bağlayıcıyı kullanmak için, mantıksal uygulamanızdaki mevcut bir tetikleyiciye bağlayıcıyı eklemeniz gerekir.

## <a name="before-you-start"></a>Başlamadan önce

İhtiyacınız olan öğeler aşağıda verilmiştir:

* Bir Azure hesabı; [ücretsiz bir hesap](https://azure.microsoft.com/free) oluşturabilirsiniz
* Zaten tanımlanmış ve Azure aboneliğinizle ilişkilendirilen bir [tümleştirme hesabı](logic-apps-enterprise-integration-create-integration-account.md) . Encode EDIOLGU ileti bağlayıcısını kullanmak için bir tümleştirme hesabınız olmalıdır. 
* Tümleştirme hesabınızda zaten tanımlanmış olan en az iki [iş ortağı](logic-apps-enterprise-integration-partners.md)
* Tümleştirme hesabınızda zaten tanımlanmış olan bir [Ediolgu sözleşmesi](logic-apps-enterprise-integration-edifact.md)

## <a name="encode-edifact-messages"></a>EDIOLGU iletilerini kodla

1. [Mantıksal uygulama oluşturun](quickstart-create-first-logic-app-workflow.md).

2. , EDIOLGU ileti bağlayıcısının tetikleyicisi yoktur, bu nedenle bir Istek tetikleyicisi gibi mantıksal uygulamanızı başlatmak için bir tetikleyici eklemeniz gerekir. Mantıksal uygulama tasarımcısında bir tetikleyici ekleyin ve sonra mantıksal uygulamanıza bir eylem ekleyin.

3.  Arama kutusuna filtreniz olarak "EDIOLGU" yazın. **Anlaşma adına göre EDIOLGU Iletisini kodla** veya **KIMLIKLERE göre Ediolgu iletisine kodlayarak**öğesini seçin.
   
    ![EDIOLGU ara](media/logic-apps-enterprise-integration-edifact-encode/edifactdecodeimage1.png)  

3. Daha önce tümleştirme hesabınızla bağlantı oluşturmadıysanız, bu bağlantıyı şimdi oluşturmanız istenir. Bağlantınızı adlandırın ve bağlamak istediğiniz tümleştirme hesabını seçin.

    ![Tümleştirme hesabı bağlantısı oluştur](media/logic-apps-enterprise-integration-edifact-encode/edifactencodeimage1.png)  

    Yıldız işareti olan özellikler gereklidir.

    | Özellik | Ayrıntılar |
    | --- | --- |
    | Bağlantı adı * |Bağlantınız için bir ad girin. |
    | Tümleştirme hesabı * |Tümleştirme hesabınız için bir ad girin. Tümleştirme hesabınızın ve mantıksal uygulamanızın aynı Azure konumunda olduğundan emin olun. |

5.  İşiniz bittiğinde, bağlantı ayrıntılarınız bu örneğe benzer görünmelidir. Bağlantınızın oluşturulmasını tamamlaması için **Oluştur**' u seçin.

    ![Tümleştirme hesabı bağlantı ayrıntıları](media/logic-apps-enterprise-integration-edifact-encode/edifactencodeimage2.png)

    Bağlantınız artık oluşturuldu.

    ![Tümleştirme hesabı bağlantısı oluşturuldu](media/logic-apps-enterprise-integration-edifact-encode/edifactencodeimage4.png)

#### <a name="encode-edifact-message-by-agreement-name"></a>EDIOLGU Iletisini anlaşma adına göre kodla

EDIOLGU iletilerini anlaşma adına göre kodlamayı seçtiyseniz, **ediolgu sözleşmesi listesinin adını** açın, ediolgu sözleşmesi adınızı girin veya seçin. Kodlanacak XML iletisini girin.

![Kodlanacak EDIOLGU sözleşmesi adını ve XML iletisini girin](media/logic-apps-enterprise-integration-edifact-encode/edifactencodeimage6.png)

#### <a name="encode-edifact-message-by-identities"></a>Kimliğe göre EDIOLGU Iletisini kodla

EDIOLGU iletilerini kimliklere göre kodlamayı tercih ederseniz, bir gönderen tanımlayıcısı, gönderici niteleyicisi, alıcı tanımlayıcısı ve alıcı niteleyicisi, EDIOLGU sözleşmenizde yapılandırıldığı şekilde girin. Kodlanacak XML iletisini seçin.

![Gönderen ve alıcı için kimlikler sağlayın, kodlanacak XML iletisini seçin](media/logic-apps-enterprise-integration-edifact-encode/edifactencodeimage7.png)

## <a name="edifact-encode-details"></a>EDIOLGU kodlama ayrıntıları

Encode EDIOLGU Bağlayıcısı şu görevleri gerçekleştirir: 

* Gönderici niteleyicisi & tanımlayıcı ve alıcı niteleyicisi ile tanımlayıcıyı eşleştirerek sözleşmeyi çözümleyin
* XML kodlu iletileri değişim içindeki EDI işlem kümelerine dönüştürerek EDI Interchange 'i seri hale getirir.
* İşlem kümesi üst bilgisi ve treyler segmentlerini uygular
* Her giden değişim için bir değişim denetim numarası, bir Grup denetim numarası ve bir işlem kümesi denetim numarası üretir
* Yük verilerinde ayırıcıları değiştirir
* EDI ve iş ortaklarına özgü özellikleri doğrular
  * İleti şemasına karşı işlem kümesi veri öğeleri için şema doğrulaması.
  * EDI doğrulaması işlem kümesi veri öğelerinde gerçekleştirildi.
  * İşlem kümesi veri öğelerinde Genişletilmiş Doğrulama gerçekleştirildi
* Her işlem kümesi için bir XML belgesi oluşturur.
* Bir teknik ve/veya Işlevsel onay (yapılandırıldıysa) ister.
  * Teknik bir bildirim olarak, CONM iletisi bir değişim alındığını gösterir.
  * İşlevsel bir bildirim olarak, CONLA iletisi alınan değişim, Grup veya iletinin bir hata veya desteklenmeyen işlev listesiyle kabul veya Red olduğunu belirtir

## <a name="view-swagger-file"></a>Swagger dosyasını görüntüle
EDIOLGU bağlayıcısının Swagger ayrıntılarını görüntülemek için, bkz. [ediolgu](/connectors/edifact/).

## <a name="next-steps"></a>Sonraki adımlar
[Enterprise Integration Pack hakkında daha fazla bilgi edinin](logic-apps-enterprise-integration-overview.md "Enterprise Integration Pack hakkında bilgi edinin") 

