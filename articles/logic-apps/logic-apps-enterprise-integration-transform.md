---
title: XML'i biçimler arasında dönüştürme
description: Kurumsal Tümleştirme Paketi ile Azure Logic Apps'taki biçimler arasında XML dönüştüren dönüşümler veya haritalar oluşturun
services: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, logicappspm
ms.topic: article
ms.date: 07/08/2016
ms.openlocfilehash: 500769a39ba7658b35c1abb80101f6234170c941
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74792385"
---
# <a name="create-maps-that-transform-xml-between-formats-in-azure-logic-apps-with-enterprise-integration-pack"></a>Kurumsal Tümleştirme Paketi ile Azure Logic Apps'taki biçimler arasında XML dönüştüren haritalar oluşturun

Kurumsal tümleştirme Transform bağlayıcısı verileri bir biçimden başka bir biçime dönüştürür. Örneğin, YearMonthDay biçiminde geçerli tarihi içeren gelen bir iletiniz olabilir. Bir dönüşümü, MonthDayYear biçiminde olmak üzere yeniden biçimlendirmek için kullanabilirsiniz.

## <a name="what-does-a-transform-do"></a>Dönüşüm ne işe yarar?
Harita olarak da bilinen Dönüşüm, Kaynak XML şeması (giriş) ve Target XML şemasından (çıktı) oluşur. Dize manipülasyonları, koşullu atamalar, aritmetik ifadeler, tarih zamanı formatters ve hatta döngü yapıları da dahil olmak üzere verileri işlemek veya denetlemek yardımcı olmak için farklı yerleşik işlevleri kullanabilirsiniz.

## <a name="how-to-create-a-transform"></a>Nasıl bir dönüşüm oluşturmak için?
Visual Studio [Enterprise Integration SDK'yı](https://aka.ms/vsmapsandschemas)kullanarak bir dönüşüm/harita oluşturabilirsiniz. Dönüşümü oluşturmayı ve test etmeyi bitirdiğinizde, dönüşümü entegrasyon hesabınıza yüklersiniz. 

## <a name="how-to-use-a-transform"></a>Dönüşüm nasıl kullanılır?
Dönüşüm/haritayı entegrasyon hesabınıza yükledikten sonra, bir Mantık uygulaması oluşturmak için kullanabilirsiniz. Logic uygulaması, Logic uygulaması tetiklendiğinde dönüşümlerinizi çalıştırır (ve dönüştürülmesi gereken giriş içeriği vardır).

**Dönüşüm için gereken adımlar şunlardır:**

### <a name="prerequisites"></a>Ön koşullar

* Bir tümleştirme hesabı oluşturun ve bir harita ekleyin  

Artık ön koşulları hallettiğinize göre, Mantık uygulamanızı oluşturma nın zamanı doldu:  

1. Bir Mantık uygulaması oluşturun ve haritayı içeren [tümleştirme hesabınıza bağla.](../logic-apps/logic-apps-enterprise-integration-accounts.md "Bir tümleştirme hesabını Bir Mantık uygulamasına bağlamayı öğrenin")
2. Mantık uygulamanıza **İstek** tetikleyicisi ekleme  
   ![](./media/logic-apps-enterprise-integration-transforms/transform-1.png)    
3. Önce eylem ekle'yi seçerek **XML'i Dönüştür** **eylemini** ekleme   
   ![](./media/logic-apps-enterprise-integration-transforms/transform-2.png)   
4. Tüm eylemleri kullanmak istediğiniz eve filtrelemek için arama kutusuna *dönüşüm* sözcüğe girin  
   ![](./media/logic-apps-enterprise-integration-transforms/transform-3.png)  
5. **XML'i Dönüştür** eylemini seçin   
6. Dönüştürdüğünüz XML **İçERİğİnİ** EKLEYIN. HTTP isteğinde aldığınız XML verilerini **İçERİk**olarak kullanabilirsiniz. Bu örnekte, Mantık uygulamasını tetikleyen HTTP isteğinin gövdesini seçin.

   > [!NOTE]
   > **Transform XML'in** içeriğinin XML olduğundan emin olun. İçerik XML'de değilse veya base64 kodluysa, içeriği işleyen bir ifade belirtmeniz gerekir. Örneğin, içeriği niçin çözme ```@base64ToBinary``` veya ```@xml``` içeriği XML olarak işlemek gibi [işlevleri](logic-apps-workflow-definition-language.md#functions)kullanabilirsiniz.
 

7. Dönüşümü gerçekleştirmek için kullanmak istediğiniz **MAP'in** adını seçin. Harita zaten entegrasyon hesabınızda olmalıdır. Daha önceki bir adımda, Logic uygulamanıza haritanızı içeren tümleştirme hesabınıza erişim izni vermişsiniz.      
   ![](./media/logic-apps-enterprise-integration-transforms/transform-4.png) 
8. Çalışmanızı kaydetme  
    ![](./media/logic-apps-enterprise-integration-transforms/transform-5.png) 

Bu noktada, haritanızı ayarlamayı bitirdiniz. Gerçek bir uygulamada, dönüştürülmüş verileri SalesForce gibi bir LOB uygulamasında depolamak isteyebilirsiniz. Dönüşümün çıktısını Salesforce'a göndermek için kolayca bir eylem olarak yapabilirsiniz. 

Artık http bitiş noktasına bir istekte bulunarak dönüşümünüzü test edebilirsiniz.  


## <a name="features-and-use-cases"></a>Özellikler ve kullanım örnekleri
* Bir haritada oluşturulan dönüştürme, bir belgeden diğerine ad ve adres kopyalama gibi basit olabilir. Veya, kutudan çıkma harita işlemlerini kullanarak daha karmaşık dönüşümler oluşturabilirsiniz.  
* Dizeleri, tarih saati işlevleri ve benzeri dahil olmak üzere birden çok harita işlemleri veya işlevleri kolayca kullanılabilir.  
* Şemalar arasında doğrudan veri kopyası yapabilirsiniz. SDK'da yer alan Mapper'da bu, kaynak şemadaki öğeleri hedef şemadaki benzerleriyle birbirine bağlayan bir çizgi çizmek kadar basittir.  
* Bir harita oluştururken, oluşturduğunuz tüm ilişkileri ve bağlantıları gösteren haritanın grafik gösterimini görüntüleyebilirsiniz.
* Örnek bir XML iletisi eklemek için Test Haritası özelliğini kullanın. Basit bir tıklamayla, oluşturduğunuz haritayı sınayabilir ve oluşturulan çıktıyı görebilirsiniz.  
* Varolan haritaları yükleme  
* XML biçimi için destek içerir.

## <a name="advanced-features"></a>Gelişmiş özellikler

### <a name="reference-assembly-or-custom-code-from-maps"></a>Haritalardan referans montaj veya özel kod 
Dönüştürme eylemi, dış derlemeye atıfta bulunarak haritaları veya dönüşümleri de destekler. Bu özellik, aramaları doğrudan XSLT eşlemlerinden özel .NET koduna göre yapmanızı sağlar. Burada haritalarda montaj kullanmak için ön koşullar vardır.

* Haritadan başvurulan harita ve derlemenin [entegrasyon hesabına yüklenmesi](./logic-apps-enterprise-integration-maps.md)gerekir. 

  > [!NOTE]
  > Harita ve montajın belirli bir sırada yüklenmesi gerekir. Derlemeye atıfta bulunulan haritayı yüklemeden önce derlemeyi yüklemeniz gerekir.

* Haritada ayrıca bu özniteliklere ve derleme koduna çağrıiçeren bir CDATA bölümü de olmalıdır:

    * **adı** özel montaj adıdır.
    * **ad alanı,** derlemenizde özel kodu içeren ad alanıdır.

  Bu örnek, "XslUtilitiesLib" adlı bir derlemeye `circumreference` başvuran ve yöntemi derlemeden çağıran bir haritayı gösterir.

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform" xmlns:msxsl="urn:schemas-microsoft-com:xslt" xmlns:user="urn:my-scripts">
  <msxsl:script language="C#" implements-prefix="user">
    <msxsl:assembly name="XsltHelperLib"/>
    <msxsl:using namespace="XsltHelpers"/>
    <![CDATA[public double circumference(int radius){ XsltHelper helper = new XsltHelper(); return helper.circumference(radius); }]]>
  </msxsl:script>
  <xsl:template match="data">
     <circles>
        <xsl:for-each select="circle">
            <circle>
                <xsl:copy-of select="node()"/>
                    <circumference>
                        <xsl:value-of select="user:circumference(radius)"/>
                    </circumference>
            </circle>
        </xsl:for-each>
     </circles>
    </xsl:template>
    </xsl:stylesheet>
  ```


### <a name="byte-order-mark"></a>Bayt Sipariş İşareti
Varsayılan olarak, dönüşümden gelen yanıt Byte Order Mark (BOM) ile başlar. Bu işlevselliğe yalnızca Code View düzenleyicisinde çalışırken erişebilirsiniz. Bu işlevselliği devre dışı `disableByteOrderMark` kalmak için özellik için belirtin: `transformOptions`

```json
"Transform_XML": {
    "inputs": {
        "content": "@{triggerBody()}",
        "integrationAccount": {
            "map": {
                "name": "TestMap"
            }
        },
        "transformOptions": "disableByteOrderMark"
    },
    "runAfter": {},
    "type": "Xslt"
}
```





## <a name="learn-more"></a>Daha fazla bilgi edinin
* [Kurumsal Entegrasyon Paketi hakkında daha fazla bilgi edinin](../logic-apps/logic-apps-enterprise-integration-overview.md "Kurumsal Entegrasyon Paketi hakkında bilgi edinin")  
* [Haritalar hakkında daha fazla bilgi edinin](../logic-apps/logic-apps-enterprise-integration-maps.md "Kurumsal entegrasyon haritaları hakkında bilgi edinin")  

