---
title: X12 iletilerini kodlama
description: KURUMSAL Tümleştirme Paketi ile Azure Logic Apps'ta EDI'yi doğrulayın ve XML kodlu iletileri X12 ileti kodlayıcısıyla dönüştürün
services: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, logicappspm
ms.topic: article
ms.date: 01/27/2017
ms.openlocfilehash: c87a61dc77e656a1cfe667ce87f852303a0cc486
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74792355"
---
# <a name="encode-x12-messages-in-azure-logic-apps-with-enterprise-integration-pack"></a>Kurumsal Tümleştirme Paketi ile Azure Logic Apps'ta X12 iletilerini kodlayın

Encode X12 ileti bağlayıcısı ile EDI ve iş ortağına özgü özellikleri doğrulayabilir, XML kodlanmış iletileri değişimde EDI işlem kümelerine dönüştürebilir ve Teknik Bildirim, İşlevsel Bildirim veya her ikisini de isteyebilirsiniz.
Bu bağlayıcıyı kullanmak için, konektörü mantık uygulamanızdaki varolan bir tetikleyiciye eklemeniz gerekir.

## <a name="before-you-start"></a>Başlamadan önce

İhtiyacınız olan öğeler şunlardır:

* Bir Azure hesabı; ücretsiz bir [hesap](https://azure.microsoft.com/free) oluşturabilirsiniz
* Azure aboneliğiniz ile tanımlanmış ve ilişkili bir [entegrasyon hesabı.](logic-apps-enterprise-integration-create-integration-account.md) Encode X12 ileti bağlayıcısını kullanmak için bir entegrasyon hesabınız olmalıdır.
* Tümleştirme hesabınızda zaten tanımlanmış en az iki [ortak](logic-apps-enterprise-integration-partners.md)
* Entegrasyon hesabınızda zaten tanımlanmış bir [X12 anlaşması](logic-apps-enterprise-integration-x12.md)

## <a name="encode-x12-messages"></a>X12 iletilerini kodlama

1. [Bir mantık uygulaması oluşturun.](quickstart-create-first-logic-app-workflow.md)

2. Encode X12 ileti bağlayıcısının tetikleyicisi yoktur, bu nedenle mantık uygulamanızı başlatmak için İstek tetikleyicisi gibi bir tetikleyici eklemeniz gerekir. Logic App Designer'da bir tetikleyici ekleyin ve ardından mantık uygulamanıza bir eylem ekleyin.

3.  Arama kutusuna filtreniz için "x12" girin. **X12 - Anlaşma adına X12 iletisini kodla** veya **X12 'ye kodla - Kimliklere göre X12 iletisine kodlama.**
   
    !["x12" arama](./media/logic-apps-enterprise-integration-x12-encode/x12decodeimage1.png) 

3. Daha önce tümleştirme hesabınıza herhangi bir bağlantı oluşturmadıysanız, bu bağlantıyı şimdi oluşturmanız istenir. Bağlantınızı adlandırın ve bağlanmak istediğiniz tümleştirme hesabını seçin. 
   
    ![entegrasyon hesabı bağlantısı](./media/logic-apps-enterprise-integration-x12-encode/x12encodeimage1.png)

    Yıldız işareti olan özellikler gereklidir.

    | Özellik | Ayrıntılar |
    | --- | --- |
    | Bağlantı Adı * |Bağlantınız için herhangi bir ad girin. |
    | Entegrasyon Hesabı * |Tümleştirme hesabınız için bir ad girin. Entegrasyon hesabınızın ve mantık uygulamanızın aynı Azure konumunda olduğundan emin olun. |

5.  İşinizi bitirdiğınızda, bağlantı bilgileriniz bu örneğe benzer olmalıdır. Bağlantınızı oluşturmayı bitirmek için **Oluştur'u**seçin.

    ![entegrasyon hesabı bağlantısı oluşturuldu](./media/logic-apps-enterprise-integration-x12-encode/x12encodeimage2.png)

    Bağlantınız artık oluşturuldu.

    ![entegrasyon hesabı bağlantı ayrıntıları](./media/logic-apps-enterprise-integration-x12-encode/x12encodeimage3.png) 

#### <a name="encode-x12-messages-by-agreement-name"></a>X12 iletilerini anlaşma adına göre kodla

X12 iletilerini anlaşma adına göre kodlamayı seçtiyseniz, **X12 sözleşme** listesini açın, mevcut X12 anlaşmanızı girin veya seçin. Kodlamak için XML iletisini girin.

![Kodlamak için X12 anlaşma adı ve XML iletisi girin](./media/logic-apps-enterprise-integration-x12-encode/x12encodeimage4.png)

#### <a name="encode-x12-messages-by-identities"></a>X12 mesajlarını kimliklere göre kodlama

X12 iletilerini kimliklere göre kodlamayı seçerseniz, X12 sözleşmenizde yapılandırılan gönderen tanımlayıcısını, gönderen niteleyicisini, alıcı tanımlayıcısını ve alıcı niteleyicisini girin. Kodlamak için XML iletisini seçin.
   
![Gönderen ve alıcı için kimlik sağlayın, kodlamak için XML iletisini seçin](./media/logic-apps-enterprise-integration-x12-encode/x12encodeimage5.png) 

## <a name="x12-encode-details"></a>X12 Ayrıntıları kodla

X12 Encode bağlayıcısı şu görevleri gerçekleştirir:

* Gönderen ve alıcı bağlam özellikleri eşleştirerek anlaşma çözümü.
* Edi değişimini seri hale getirerek XML kodlanmış iletileri değişimdeki EDI hareket kümelerine dönüştürür.
* İşlem kümesi üstbilgi ve römork bölümlerini uygular
* Giden her değişim için bir değişim kontrol numarası, bir grup kontrol numarası ve işlem kümesi kontrol numarası oluşturur
* Yük verilerindeki ayırıcıları değiştirir
* EDI'yi ve iş ortağına özgü özellikleri doğrular
  * Schema iletisine karşı işlem kümesi veri öğelerinin şema doğrulanması
  * Hareket ayarlı veri öğeleri üzerinde gerçekleştirilen EDI doğrulaması.
  * Hareket kümesi veri öğeleriüzerinde gerçekleştirilen genişletilmiş doğrulama
* Teknik ve/veya İşlevsel bildirim ister (yapılandırılırsa).
  * Teknik Bildirim, üstbilgi doğrulaması sonucunda oluşturur. Teknik bildirim, bir değişim üstbilgisi nin ve römorkun adres alıcısı tarafından işlenmesinin durumunu bildirir
  * İşlevsel Bildirim, gövde doğrulamasının bir sonucu olarak oluşturur. Alınan belgeyi işlerken karşılaşılan her hatanın işlevsel bildirim raporları

## <a name="view-the-swagger"></a>Göster
[Swagger ayrıntılarıbakın.](/connectors/x12/) 

## <a name="next-steps"></a>Sonraki adımlar
[Kurumsal Entegrasyon Paketi hakkında daha fazla bilgi edinin](logic-apps-enterprise-integration-overview.md "Kurumsal Entegrasyon Paketi hakkında bilgi edinin") 

