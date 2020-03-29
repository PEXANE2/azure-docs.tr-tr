---
title: EDIFACT iletilerini çözme
description: Kurumsal Tümleştirme Paketi ile Azure Mantık Uygulamaları için EDIFACT ileti kod çözücüsü yle EDIFACT ile onayları doğrulayın ve onaylar oluşturun
services: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, divswa, logicappspm
ms.topic: article
ms.date: 01/27/2017
ms.openlocfilehash: 78c0d20c0f32a6d63d134e958b30d38fe11fcc5c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74790674"
---
# <a name="decode-edifact-messages-for-azure-logic-apps-with-the-enterprise-integration-pack"></a>Kurumsal Tümleştirme Paketi ile Azure Mantık Uygulamaları için EDIFACT mesajlarını çözme

EDIFACT ileti bağlayıcısını Çözerek EDI ve iş ortağına özgü özellikleri doğrulayabilir, kavşakları hareket kümelerine bölebilir veya tüm ara ayarları koruyabilir ve işlenmiş hareketler için bildirimler oluşturabilirsiniz. Bu bağlayıcıyı kullanmak için, konektörü mantık uygulamanızdaki varolan bir tetikleyiciye eklemeniz gerekir.

## <a name="before-you-start"></a>Başlamadan önce

İhtiyacınız olan öğeler şunlardır:

* Bir Azure hesabı; ücretsiz bir [hesap](https://azure.microsoft.com/free) oluşturabilirsiniz
* Azure aboneliğiniz ile tanımlanmış ve ilişkili bir [entegrasyon hesabı.](logic-apps-enterprise-integration-create-integration-account.md) Decode EDIFACT ileti bağlayıcısını kullanmak için bir entegrasyon hesabınız olmalıdır. 
* Tümleştirme hesabınızda zaten tanımlanmış en az iki [ortak](logic-apps-enterprise-integration-partners.md)
* Entegrasyon hesabınızda zaten tanımlanmış bir [EDIFACT anlaşması](logic-apps-enterprise-integration-edifact.md)

## <a name="decode-edifact-messages"></a>EDIFACT iletilerini çözme

1. [Bir mantık uygulaması oluşturun.](quickstart-create-first-logic-app-workflow.md)

2. DEcode EDIFACT ileti bağlayıcısının tetikleyicisi yoktur, bu nedenle mantık uygulamanızı başlatmak için İstek tetikleyicisi gibi bir tetikleyici eklemeniz gerekir. Logic App Designer'da bir tetikleyici ekleyin ve ardından mantık uygulamanıza bir eylem ekleyin.

3. Arama kutusuna filtreniz olarak "EDIFACT" girin. **EDIFACT İletisini Çöz'ün'ünü**seçin.
   
    ![arama EDIFACT](./media/logic-apps-enterprise-integration-edifact-decode/edifactdecodeimage1.png)

3. Daha önce tümleştirme hesabınıza herhangi bir bağlantı oluşturmadıysanız, bu bağlantıyı şimdi oluşturmanız istenir. Bağlantınızı adlandırın ve bağlanmak istediğiniz tümleştirme hesabını seçin.
   
    ![entegrasyon hesabı oluşturma](./media/logic-apps-enterprise-integration-edifact-decode/edifactdecodeimage2.png)

    Yıldız işareti olan özellikler gereklidir.

    | Özellik | Ayrıntılar |
    | --- | --- |
    | Bağlantı Adı * |Bağlantınız için herhangi bir ad girin. |
    | Entegrasyon Hesabı * |Tümleştirme hesabınız için bir ad girin. Entegrasyon hesabınızın ve mantık uygulamanızın aynı Azure konumunda olduğundan emin olun. |

4. Bağlantınızı oluşturmayı bitirmeniz bittiğinde **Oluştur'u**seçin. Bağlantı ayrıntılarınız bu örneğe benzer olmalıdır:

    ![entegrasyon hesabı ayrıntıları](./media/logic-apps-enterprise-integration-edifact-decode/edifactdecodeimage3.png)  

5. Bağlantınız oluşturulduktan sonra, bu örnekte gösterildiği gibi, çözmek için EDIFACT düz dosya iletisini seçin.

    ![entegrasyon hesabı bağlantısı oluşturuldu](./media/logic-apps-enterprise-integration-edifact-decode/edifactdecodeimage4.png)  

    Örnek:

    ![Çözme için EDIFACT düz dosya iletisi seçin](./media/logic-apps-enterprise-integration-edifact-decode/edifactdecodeimage5.png)  

## <a name="edifact-decoder-details"></a>EDIFACT kod çözücü detayları

Decode EDIFACT bağlayıcısı şu görevleri yerine getirir: 

* Ticari ortak sözleşmesine karşı zarfı doğrular.
* Gönderen niteleyici ve alıcı niteleyici & tanımlayıcı& eşleştirerek anlaşmayı çözer.
* Bir değiş tokuş, sözleşmenin alma ayarları yapılandırmasına göre birden fazla işlem olduğunda birden çok hareket olarak bölünür.
* Değiş tokuş un sökülmesi.
* EDI'yi ve iş ortağına özgü özellikleri doğrular:
  * Değişim zarfı yapısının doğrulanması
  * Kontrol şemasına karşı zarfın şema doğrulanması
  * İleti şemasına karşı işlem kümesi veri öğelerinin şema doğrulanması
  * Hareket ayarlı veri öğeleriüzerinde gerçekleştirilen EDI doğrulaması
* Değişim, grup ve işlem kümesi denetim numaralarının yinelenmediğini doğrular (yapılandırılırsa) 
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
* Teknik (denetim) ve/veya İşlevsel bildirim oluşturur (yapılandırılırsa).
  * Teknik Bildirim veya CONTRL ACK, alınan tam değişimin sözdizimsel denetiminin sonuçlarını bildirir.
  * İşlevsel bir bildirim, alınan bir değişimi veya grubu kabul eder veya reddeder

## <a name="view-swagger-file"></a>Swagger dosyayı görüntüleme
EDIFACT konektörünsünü görmek için [EDIFACT'e](/connectors/edifact/)bakın.

## <a name="next-steps"></a>Sonraki adımlar
[Kurumsal Entegrasyon Paketi hakkında daha fazla bilgi edinin](logic-apps-enterprise-integration-overview.md "Kurumsal Entegrasyon Paketi hakkında bilgi edinin") 

