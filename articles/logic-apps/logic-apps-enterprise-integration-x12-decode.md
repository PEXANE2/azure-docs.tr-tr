---
title: X12 iletilerinin şifresini çözme
description: Kurumsal Tümleştirme Paketi ile Azure Mantık Uygulamaları'nda EDI'yi doğrulayın ve X12 ileti kod çözücüsü yle onaylar oluşturun
services: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, logicappspm
ms.topic: article
ms.date: 01/27/2017
ms.openlocfilehash: 918516a5629f8570d54c641ffc29f2367937266f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74792361"
---
# <a name="decode-x12-messages-in-azure-logic-apps-with-enterprise-integration-pack"></a>Kurumsal Tümleştirme Paketi ile Azure Logic Apps'taki X12 iletilerini çözme

Decode X12 ileti bağlayıcısı ile zarfı bir ticaret ortağı anlaşmasına karşı doğrulayabilir, EDI ve iş ortağına özgü özellikleri doğrulayabilir, kavşakları işlem kümelerine bölebilir veya tüm kavşakları koruyabilirsiniz ve bildirimler oluşturabilirsiniz işlenmiş işlemler için. Bu bağlayıcıyı kullanmak için, konektörü mantık uygulamanızdaki varolan bir tetikleyiciye eklemeniz gerekir.

## <a name="before-you-start"></a>Başlamadan önce

İhtiyacınız olan öğeler şunlardır:

* Bir Azure hesabı; ücretsiz bir [hesap](https://azure.microsoft.com/free) oluşturabilirsiniz
* Azure aboneliğiniz ile tanımlanmış ve ilişkili bir [entegrasyon hesabı.](logic-apps-enterprise-integration-create-integration-account.md) Decode X12 ileti bağlayıcısını kullanmak için bir entegrasyon hesabınız olmalıdır.
* Tümleştirme hesabınızda zaten tanımlanmış en az iki [ortak](logic-apps-enterprise-integration-partners.md)
* Entegrasyon hesabınızda zaten tanımlanmış bir [X12 anlaşması](logic-apps-enterprise-integration-x12.md)

## <a name="decode-x12-messages"></a>X12 iletilerinin şifresini çözme

1. [Bir mantık uygulaması oluşturun.](quickstart-create-first-logic-app-workflow.md)

2. Decode X12 ileti bağlayıcısının tetikleyicisi yoktur, bu nedenle mantık uygulamanızı başlatmak için İstek tetikleyicisi gibi bir tetikleyici eklemeniz gerekir. Logic App Designer'da bir tetikleyici ekleyin ve ardından mantık uygulamanıza bir eylem ekleyin.

3.  Arama kutusuna filtreniz için "x12" girin. **X12 seçin - X12 iletisini çöz.**
   
    !["x12" arama](media/logic-apps-enterprise-integration-x12-decode/x12decodeimage1.png)  

3. Daha önce tümleştirme hesabınıza herhangi bir bağlantı oluşturmadıysanız, bu bağlantıyı şimdi oluşturmanız istenir. Bağlantınızı adlandırın ve bağlanmak istediğiniz tümleştirme hesabını seçin. 

    ![Tümleştirme hesabı bağlantı ayrıntılarını sağlama](media/logic-apps-enterprise-integration-x12-decode/x12decodeimage4.png)

    Yıldız işareti olan özellikler gereklidir.

    | Özellik | Ayrıntılar |
    | --- | --- |
    | Bağlantı Adı * |Bağlantınız için herhangi bir ad girin. |
    | Entegrasyon Hesabı * |Tümleştirme hesabınız için bir ad girin. Entegrasyon hesabınızın ve mantık uygulamanızın aynı Azure konumunda olduğundan emin olun. |

5.  İşinizi bitirdiğınızda, bağlantı bilgileriniz bu örneğe benzer olmalıdır. Bağlantınızı oluşturmayı bitirmek için **Oluştur'u**seçin.
   
    ![entegrasyon hesabı bağlantı ayrıntıları](media/logic-apps-enterprise-integration-x12-decode/x12decodeimage5.png) 

6. Bağlantınız oluşturulduktan sonra, bu örnekte gösterildiği gibi, çözmek için X12 düz dosya iletisini seçin.

    ![entegrasyon hesabı bağlantısı oluşturuldu](media/logic-apps-enterprise-integration-x12-decode/x12decodeimage6.png) 

    Örnek:

    ![Çözme için X12 düz dosya iletisi seçin](media/logic-apps-enterprise-integration-x12-decode/x12decodeimage7.png) 

   > [!NOTE]
   > İyi veya kötü olan ileti dizisinin gerçek ileti içeriği veya yükü base64 kodlanır. Bu nedenle, bu içeriği işleyen bir ifade belirtmeniz gerekir.
   > Aşağıda, içeriği kod görünümünde girebileceğiniz XML olarak veya tasarımcıda ifade oluşturucusu kullanarak işleyen bir örnek verilmiştir.
   > ``` json
   > "content": "@xml(base64ToBinary(item()?['Payload']))"
   > ```
   > ![İçerik örneği](media/logic-apps-enterprise-integration-x12-decode/content-example.png)
   >


## <a name="x12-decode-details"></a>X12 Şifre çözme ayrıntıları

X12 Decode konektörü şu görevleri gerçekleştirir:

* Ticari ortak sözleşmesine karşı zarfı doğrular
* EDI'yi ve iş ortağına özgü özellikleri doğrular
  * EDI yapısal doğrulama ve genişletilmiş şema doğrulama
  * Değişim zarfının yapısının doğrulanması.
  * Zarfın kontrol şemasına karşı şema doğrulaması.
  * İleti şemasına karşı işlem kümesi veri öğelerinin şema doğrulaması.
  * Hareket ayarlı veri öğeleriüzerinde gerçekleştirilen EDI doğrulaması 
* Değişim, grup ve işlem kümesi denetim numaralarının yineolmadığını doğrular
  * Daha önce alınan kavşaklar karşı değişim kontrol numarasını denetler.
  * Grup kontrol numarasını, değişimdeki diğer grup kontrol numaralarıyla karşılayagöre denetler.
  * Hareket kümesi denetim numarasını bu gruptaki diğer hareket kümesi denetim numaralarıyla karşılayagöre denetler.
* Ara değişimi işlem kümelerine böler veya tüm değişimi korur:
  * İşlem kümeleri olarak Kavşağı böl - işlem kümelerini hataya göre askıya alma: Geçişişlemini hareket kümelerine böler ve her hareket kümesini ayrıştur. 
  X12 Decode eylem çıkışları yalnızca doğrulama başarısız bu `badMessages`hareket kümeleri ve kalan `goodMessages`hareketleri ayarlar .
  * İşlem kümeleri olarak Kavşağı Böl - hata daki değişimi askıya alma: Geçişişlemini işlem kümelerine böler ve her hareket kümesini ayrışturun. 
  Bir veya daha fazla işlem geçişinde kümelenme başarısız olursa, X12 Decode eylem ilerler bu değişimdeki tüm hareket kümelerini `badMessages`.
  * Değişimi Koru - hata üzerine işlem kümelerini askıya alma: Değişimi koruyun ve toplu işlemin tamamını işleyebilir. 
  X12 Decode eylem çıkışları yalnızca doğrulama başarısız bu `badMessages`hareket kümeleri ve kalan `goodMessages`hareketleri ayarlar .
  * Değişimi koru - hata değişikliğini askıya alın: Değişimi koruyun ve toplu geçişin tamamını işle. 
  Bir veya daha fazla işlem geçişinde kümelenme başarısız olursa, X12 Decode eylem ilerler bu değişimdeki tüm hareket kümelerini `badMessages`. 
* Teknik ve/veya İşlevsel bildirim oluşturur (yapılandırılırsa).
  * Teknik Bildirim, üstbilgi doğrulaması sonucunda oluşturur. Teknik bildirim, bir değişim üstbilgisi nin ve römorkun adres alıcısı tarafından işlenmesidurumunu bildirir.
  * İşlevsel Bildirim, gövde doğrulamasının bir sonucu olarak oluşturur. Alınan belgeyi işlerken karşılaşılan her hatanın işlevsel bildirim raporları

## <a name="view-the-swagger"></a>Göster
[Swagger ayrıntılarıbakın.](/connectors/x12/) 

## <a name="next-steps"></a>Sonraki adımlar
[Kurumsal Entegrasyon Paketi hakkında daha fazla bilgi edinin](../logic-apps/logic-apps-enterprise-integration-overview.md "Kurumsal Entegrasyon Paketi hakkında bilgi edinin") 

