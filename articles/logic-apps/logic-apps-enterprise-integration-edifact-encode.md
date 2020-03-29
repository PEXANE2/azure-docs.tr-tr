---
title: EDIFACT mesajlarını kodlama
description: KURUMSAL Tümleştirme Paketi ile Azure Logic Apps için EDIFACT ileti kodlayıcısı ile EDIFACT ileti kodlayıcısı ile EDII'yi doğrulayın ve XML oluşturun
services: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, divswa, logicappspm
ms.topic: article
ms.date: 01/27/2017
ms.openlocfilehash: 257cbd0b1a68ddd2b16235e6f8dec5d5b0eb10e2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74790650"
---
# <a name="encode-edifact-messages-for-azure-logic-apps-with-enterprise-integration-pack"></a>Kurumsal Tümleştirme Paketi ile Azure Logic Apps için EDIFACT mesajlarını kodla

Encode EDIFACT ileti bağlayıcısı ile EDI ve iş ortağına özgü özellikleri doğrulayabilir, her işlem kümesi için bir XML belgesi oluşturabilir ve Teknik Onay, İşlevsel Bildirim veya her ikisini de isteyebilirsiniz.
Bu bağlayıcıyı kullanmak için, konektörü mantık uygulamanızdaki varolan bir tetikleyiciye eklemeniz gerekir.

## <a name="before-you-start"></a>Başlamadan önce

İhtiyacınız olan öğeler şunlardır:

* Bir Azure hesabı; ücretsiz bir [hesap](https://azure.microsoft.com/free) oluşturabilirsiniz
* Azure aboneliğiniz ile tanımlanmış ve ilişkili bir [entegrasyon hesabı.](logic-apps-enterprise-integration-create-integration-account.md) Encode EDIFACT ileti bağlayıcısını kullanmak için bir tümleştirme hesabınız olmalıdır. 
* Tümleştirme hesabınızda zaten tanımlanmış en az iki [ortak](logic-apps-enterprise-integration-partners.md)
* Entegrasyon hesabınızda zaten tanımlanmış bir [EDIFACT anlaşması](logic-apps-enterprise-integration-edifact.md)

## <a name="encode-edifact-messages"></a>EDIFACT mesajlarını kodlama

1. [Bir mantık uygulaması oluşturun.](quickstart-create-first-logic-app-workflow.md)

2. Encode EDIFACT ileti bağlayıcısının tetikleyicisi yoktur, bu nedenle mantık uygulamanızı başlatmak için İstek tetikleyicisi gibi bir tetikleyici eklemeniz gerekir. Logic App Designer'da bir tetikleyici ekleyin ve ardından mantık uygulamanıza bir eylem ekleyin.

3.  Arama kutusuna filtreniz olarak "EDIFACT" girin. **EDIFACT İletisini anlaşma adına göre kodla** veya **identit'lere göre EDIFACT iletisine kodlama'yı**seçin.
   
    ![arama EDIFACT](media/logic-apps-enterprise-integration-edifact-encode/edifactdecodeimage1.png)  

3. Daha önce tümleştirme hesabınıza herhangi bir bağlantı oluşturmadıysanız, bu bağlantıyı şimdi oluşturmanız istenir. Bağlantınızı adlandırın ve bağlanmak istediğiniz tümleştirme hesabını seçin.

    ![entegrasyon hesabı bağlantısı oluşturma](media/logic-apps-enterprise-integration-edifact-encode/edifactencodeimage1.png)  

    Yıldız işareti olan özellikler gereklidir.

    | Özellik | Ayrıntılar |
    | --- | --- |
    | Bağlantı Adı * |Bağlantınız için herhangi bir ad girin. |
    | Entegrasyon Hesabı * |Tümleştirme hesabınız için bir ad girin. Entegrasyon hesabınızın ve mantık uygulamanızın aynı Azure konumunda olduğundan emin olun. |

5.  İşinizi bitirdiğınızda, bağlantı bilgileriniz bu örneğe benzer olmalıdır. Bağlantınızı oluşturmayı bitirmek için **Oluştur'u**seçin.

    ![entegrasyon hesabı bağlantı ayrıntıları](media/logic-apps-enterprise-integration-edifact-encode/edifactencodeimage2.png)

    Bağlantınız artık oluşturuldu.

    ![entegrasyon hesabı bağlantısı oluşturuldu](media/logic-apps-enterprise-integration-edifact-encode/edifactencodeimage4.png)

#### <a name="encode-edifact-message-by-agreement-name"></a>EDIFACT İletisini anlaşma adına göre kodlama

EDIFACT mesajlarını anlaşma adına kodlamayı seçtiyseniz, EDIFACT sözleşme listesini açın, **EDIFACT sözleşme** adınızı girin veya seçin. Kodlamak için XML iletisini girin.

![Kodlamak için EDIFACT anlaşma adı ve XML iletisini girin](media/logic-apps-enterprise-integration-edifact-encode/edifactencodeimage6.png)

#### <a name="encode-edifact-message-by-identities"></a>EDIFACT İletisini kimliklere göre kodlama

EDIFACT iletilerini kimliklere göre kodlamayı seçerseniz, EDIFACT anlaşmanızda yapılandırılan gönderen tanımlayıcısını, gönderen niteleyicisini, alıcı tanımlayıcısını ve alıcı niteleyicisini girin. Kodlamak için XML iletisini seçin.

![Gönderen ve alıcı için kimlik sağlayın, kodlamak için XML iletisini seçin](media/logic-apps-enterprise-integration-edifact-encode/edifactencodeimage7.png)

## <a name="edifact-encode-details"></a>EDIFACT Ayrıntıları kodla

Encode EDIFACT bağlayıcısı şu görevleri yerine getirir: 

* Gönderen niteleyici ve alıcı niteleyici ve tanımlayıcı & eşleştirerek anlaşmayı çözme
* Edi değişimini seri hale getirerek XML kodlanmış iletileri değişimdeki EDI hareket kümelerine dönüştürür.
* İşlem kümesi üstbilgi ve römork bölümlerini uygular
* Giden her değişim için bir değişim kontrol numarası, bir grup kontrol numarası ve işlem kümesi kontrol numarası oluşturur
* Yük verilerindeki ayırıcıları değiştirir
* EDI'yi ve iş ortağına özgü özellikleri doğrular
  * İleti şemasına karşı işlem kümesi veri öğelerinin şema doğrulaması.
  * Hareket ayarlı veri öğeleri üzerinde gerçekleştirilen EDI doğrulaması.
  * Hareket kümesi veri öğeleriüzerinde gerçekleştirilen genişletilmiş doğrulama
* Her hareket kümesi için bir XML belgesi oluşturur.
* Teknik ve/veya İşlevsel bildirim ister (yapılandırılırsa).
  * Teknik bir bildirim olarak, CONTRL iletisi bir değiş tokuşun alındığını gösterir.
  * İşlevsel bir bildirim olarak CONTRL iletisi, alınan değiş tokuşun, grubun veya iletinin hata veya desteklenmeyen işlevselliğin bir listesiyle kabul edildiğini veya reddedildiğini gösterir

## <a name="view-swagger-file"></a>Swagger dosyayı görüntüleme
EDIFACT konektörünsünü görmek için [EDIFACT'e](/connectors/edifact/)bakın.

## <a name="next-steps"></a>Sonraki adımlar
[Kurumsal Entegrasyon Paketi hakkında daha fazla bilgi edinin](logic-apps-enterprise-integration-overview.md "Kurumsal Entegrasyon Paketi hakkında bilgi edinin") 

