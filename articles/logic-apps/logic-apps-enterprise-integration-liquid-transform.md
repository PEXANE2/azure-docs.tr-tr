---
title: JSON verilerini sıvı dönüştürmeleri ile Dönüştür
description: Logic Apps ve sıvı şablonu kullanarak gelişmiş JSON dönüştürmeleri için dönüşümler veya haritalar oluşturma
services: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: estfan, logicappspm
ms.topic: article
ms.date: 04/01/2020
ms.openlocfilehash: d2598dfe9d7972dcb764abf4a1239613a1e8417a
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "80879182"
---
# <a name="perform-advanced-json-transformations-with-liquid-templates-in-azure-logic-apps"></a>Azure Logic Apps'te Liquid şablonlarıyla gelişmiş JSON dönüşümleri gerçekleştirme

Mantıksal uygulamalarınızda, JSON **oluşturma** veya **ayrıştırma**gibi yerel veri Işleme eylemleriyle temel JSON dönüşümleri gerçekleştirebilirsiniz. Gelişmiş JSON dönüştürmeleri gerçekleştirmek için, esnek Web uygulamalarına yönelik açık kaynaklı bir şablon dili olan [likit](https://shopify.github.io/liquid/)ile şablonlar veya haritalar oluşturabilirsiniz. Liquid şablonları, JSON çıktısının nasıl dönüştürüleceğini tanımlar ve yinelemeler, denetim akışları ve değişkenler gibi daha karmaşık JSON dönüşümlerini destekler.

Mantıksal uygulamanızda bir likit dönüştürme gerçekleştirebilmek için önce JSON ile JSON eşlemeyi bir sıvı şablonuyla tanımlamanız ve bu eşlemeyi tümleştirme hesabınızda depolamanız gerekir. Bu makalede, bu likit şablon veya haritanın nasıl oluşturulduğu ve kullanılacağı gösterilir.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Aboneliğiniz yoksa, [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/).

* [Mantıksal uygulamalar oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md) hakkında temel bilgi

* Temel bir [tümleştirme hesabı](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md)

* [Sıvı şablonu dili](https://shopify.github.io/liquid/) hakkında temel bilgi

## <a name="create-liquid-template-or-map-for-your-integration-account"></a>Tümleştirme hesabınız için likit şablon veya eşleme oluşturma

1. Bu örnekte, bu adımda açıklanan örnek likit şablonunu oluşturun. Sıvı şablonunuzda, [dotlikit](https://github.com/dotliquid/dotliquid) ve C# adlandırma kurallarını kullanan [sıvı filtrelerini](https://shopify.github.io/liquid/basics/introduction/#filters)kullanabilirsiniz.

   > [!NOTE]
   > Filtre adlarının şablonunuzda *tümce* kullanmasına dikkat edin. Aksi takdirde, filtreler çalışmaz. Ayrıca, haritalar [Dosya boyutu sınırlarına](../logic-apps/logic-apps-limits-and-config.md#artifact-capacity-limits)sahiptir.

   ```json
   {%- assign deviceList = content.devices | Split: ', ' -%}
   
   {
      "fullName": "{{content.firstName | Append: ' ' | Append: content.lastName}}",
      "firstNameUpperCase": "{{content.firstName | Upcase}}",
      "phoneAreaCode": "{{content.phone | Slice: 1, 3}}",
      "devices" : [
         {%- for device in deviceList -%}
            {%- if forloop.Last == true -%}
            "{{device}}"
            {%- else -%}
            "{{device}}",
            {%- endif -%}
        {%- endfor -%}
        ]
   }
   ```

1. [Azure Portal](https://portal.azure.com)Azure Arama kutusuna girin `integration accounts` ve **tümleştirme hesapları**' nı seçin.

   !["Tümleştirme hesaplarını" bul](./media/logic-apps-enterprise-integration-liquid-transform/find-integration-accounts.png)

1. Tümleştirme hesabınızı bulun ve seçin.

   ![Tümleştirme hesabı seçin](./media/logic-apps-enterprise-integration-liquid-transform/select-integration-account.png)

1. **Genel bakış** bölmesinde, **Bileşenler**altında **haritalar**' ı seçin.

    !["Haritalar" kutucuğunu seçin](./media/logic-apps-enterprise-integration-liquid-transform/select-maps-tile.png)

1. **Haritalar** bölmesinde **Ekle** ' yi seçin ve Haritalarınız için bu ayrıntıları sağlayın:

   | Özellik | Değer | Açıklama | 
   |----------|-------|-------------|
   | **Adı** | `JsonToJsonTemplate` | Bu örnekteki "JsonToJsonTemplate" olan haritaınızın adı | 
   | **Eşleme türü** | **sıvı** | Haritalarınızın türü. JSON-JSON dönüştürmesi için **likit**' i seçmeniz gerekir. | 
   | **Harita** | `SimpleJsonToJsonTemplate.liquid` | Bu örnekte "SimpleJsonToJsonTemplate. sıvı" olan dönüştürme için kullanılacak mevcut bir likit şablon veya eşleme dosyası. Bu dosyayı bulmak için dosya seçiciyi kullanabilirsiniz. Harita boyutu sınırları için bkz. [sınırlara ve yapılandırma](../logic-apps/logic-apps-limits-and-config.md#artifact-capacity-limits). |
   ||| 

   ![Likit Şablon Ekle](./media/logic-apps-enterprise-integration-liquid-transform/add-liquid-template.png)
    
## <a name="add-the-liquid-action-for-json-transformation"></a>JSON dönüştürmesi için likit eylemi ekleme

1. Azure portal [boş bir mantıksal uygulama oluşturmak](../logic-apps/quickstart-create-first-logic-app-workflow.md)için aşağıdaki adımları izleyin.

1. Mantıksal uygulama Tasarımcısı ' nda, mantıksal uygulamanıza [istek tetikleyiciyi](../connectors/connectors-native-reqres.md#add-request) ekleyin.

1. Tetikleyici altında **yeni adım**' ı seçin. Arama kutusuna `liquid` filtreniz olarak girin ve şu eylemi seçin: **JSON 'u JSON 'a Dönüştür-sıvı**

   ![Sıvı eylemini bul ve Seç](./media/logic-apps-enterprise-integration-liquid-transform/search-action-liquid.png)

1. **Harita** listesini açın ve bu örnekteki "JsonToJsonTemplate" olan sıvı şablonunuzu seçin.

   ![Eşleme Seç](./media/logic-apps-enterprise-integration-liquid-transform/select-map.png)

   Haritalar listesi boşsa, büyük olasılıkla mantıksal uygulamanız tümleştirme hesabınıza bağlı değildir. 
   Mantıksal uygulamanızı likit şablon veya eşleme içeren tümleştirme hesabına bağlamak için şu adımları izleyin:

   1. Mantıksal uygulama menünüzde **Iş akışı ayarları**' nı seçin.

   1. **Tümleştirme hesabı seçin** listesinden tümleştirme hesabınızı seçin ve **Kaydet**' i seçin.

      ![Mantıksal uygulamayı tümleştirme hesabına bağlama](./media/logic-apps-enterprise-integration-liquid-transform/link-integration-account.png)

1. Şimdi bu eyleme **içerik** özelliğini ekleyin. **Yeni parametre Ekle** listesini açın ve **içerik**' i seçin.

   ![Eyleme "Içerik" özelliği Ekle](./media/logic-apps-enterprise-integration-liquid-transform/add-content-property-to-action.png)

1. **İçerik** özellik değerini ayarlamak Için, **içerik** kutusunun içine tıklayarak dinamik içerik listesinin görünmesini sağlayın. Tetikleyiciden gelen gövde içerik çıkışını temsil eden **gövde** belirtecini seçin.

   !["Içerik" özelliği değeri için "gövde" belirtecini seçin](./media/logic-apps-enterprise-integration-liquid-transform/select-body.png)

   İşlem tamamlandığında eylem şu örnekteki gibi görünür:

   !["JSON 'dan JSON 'a dönüştürme" eylemi bitti](./media/logic-apps-enterprise-integration-liquid-transform/finished-transform-action.png)

## <a name="test-your-logic-app"></a>Mantıksal uygulamanızı test etme

JSON girişini [Postman](https://www.getpostman.com/postman) veya benzer bir araçtan mantıksal uygulamanıza gönderin. Mantıksal uygulamanızdan dönüştürülmüş JSON çıktısı şu örneğe benzer şekilde görünür:
  
![Örnek çıkış](./media/logic-apps-enterprise-integration-liquid-transform/example-output-jsontojson.png)

## <a name="more-liquid-action-examples"></a>Diğer likit eylem örnekleri
Sıvı yalnızca JSON dönüşümlerine sınırlı değildir. Aşağıda, sıvı kullanan diğer kullanılabilir dönüştürme eylemleri verilmiştir.

* JSON 'ı metne Dönüştür
  
  Bu örnek için kullanılan likit şablon aşağıda verilmiştir:
   
   ``` json
   {{content.firstName | Append: ' ' | Append: content.lastName}}
   ```
   Örnek girişler ve çıktılar aşağıda verilmiştir:
  
   ![Örnek çıkış JSON-Text](./media/logic-apps-enterprise-integration-liquid-transform/example-output-jsontotext.png)

* XML 'i JSON 'ye Dönüştür
  
  Bu örnek için kullanılan likit şablon aşağıda verilmiştir:
   
   ``` json
   [{% JSONArrayFor item in content -%}
        {{item}}
    {% endJSONArrayFor -%}]
   ```
   Örnek girişler ve çıktılar aşağıda verilmiştir:

   ![Örnek çıkış XML-JSON](./media/logic-apps-enterprise-integration-liquid-transform/example-output-xmltojson.png)

* XML 'e metin dönüştürme
  
  Bu örnek için kullanılan likit şablon aşağıda verilmiştir:

   ``` json
   {{content.firstName | Append: ' ' | Append: content.lastName}}
   ```

   Örnek girişler ve çıktılar aşağıda verilmiştir:

   ![Örnek çıkış XML-Text](./media/logic-apps-enterprise-integration-liquid-transform/example-output-xmltotext.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Enterprise Integration Pack hakkında daha fazla bilgi edinin](../logic-apps/logic-apps-enterprise-integration-overview.md "Enterprise Integration Pack hakkında bilgi edinin")  
* [Haritalar hakkında daha fazla bilgi edinin](../logic-apps/logic-apps-enterprise-integration-maps.md "Kurumsal tümleştirme haritaları hakkında bilgi edinin")  

