---
title: Sıvı dönüşümleri ile JSON verilerini dönüştürme
description: Logic Apps ve Liquid şablonu kullanarak gelişmiş JSON dönüşümleri için dönüşümler veya haritalar oluşturun
services: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: estfan, logicappspm
ms.topic: article
ms.date: 04/01/2020
ms.openlocfilehash: 0ab9297e772a3b75a077da1c2ae74e5058b2731f
ms.sourcegitcommit: 62c5557ff3b2247dafc8bb482256fef58ab41c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80657295"
---
# <a name="perform-advanced-json-transformations-with-liquid-templates-in-azure-logic-apps"></a>Azure Logic Apps'ta Sıvı şablonları ile gelişmiş JSON dönüşümleri gerçekleştirin

Mantıksal uygulamalarınızda temel JSON **dönüşümlerini, Compose** veya **Parse JSON**gibi yerel veri işlem eylemleri ile gerçekleştirebilirsiniz. Gelişmiş JSON dönüşümleri gerçekleştirmek için, esnek web uygulamaları için açık kaynak kodlu bir şablon dili olan [Liquid](https://shopify.github.io/liquid/)ile şablonlar veya haritalar oluşturabilirsiniz. Sıvı şablonu JSON çıktısını nasıl dönüştüreceklerini tanımlar ve yinelemeler, denetim akışları, değişkenler gibi daha karmaşık JSON dönüşümlerini destekler.

Mantık uygulamanızda Sıvı dönüşümü gerçekleştirmeden önce JSON-JSON eşlemesi ile Sıvı şablonu tanımlamanız ve bu haritayı entegrasyon hesabınızda saklamanız gerekir. Bu makalede, bu Sıvı şablonu veya haritasını nasıl oluşturabileceğinizve kullanacağınız gösterilmektedir.

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Aboneliğiniz yoksa, [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/).

* [Mantık uygulamaları oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md) hakkında temel bilgiler

* Temel [bir tümleştirme hesabı](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md)

* [Sıvı şablon dili](https://shopify.github.io/liquid/) hakkında temel bilgi

## <a name="create-liquid-template-or-map-for-your-integration-account"></a>Entegrasyon hesabınız için Sıvı şablonu veya harita oluşturma

1. Bu örnekte, bu adımda açıklanan örnek Sıvı şablonu oluşturun. Sıvı şablonunuzda, [DotLiquid](https://dotliquidmarkup.org/) ve C# adlandırma kurallarını kullanan [Sıvı filtreleri](https://shopify.github.io/liquid/basics/introduction/#filters)kullanabilirsiniz.

   > [!NOTE]
   > Filtre adlarının şablonunuzda *cümle kılıfı* kullandığından emin olun. Aksi takdirde, filtreler çalışmaz. Ayrıca, haritalar [dosya boyutu sınırları](../logic-apps/logic-apps-limits-and-config.md#artifact-capacity-limits)var.

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

1. Azure [portalında,](https://portal.azure.com)Azure arama kutusundan, `integration accounts` **Tümleştirme hesaplarını**girin ve seçin.

   !["Tümleştirme hesapları" bul](./media/logic-apps-enterprise-integration-liquid-transform/find-integration-accounts.png)

1. Entegrasyon hesabınızı bulun ve seçin.

   ![Entegrasyon hesabını seçin](./media/logic-apps-enterprise-integration-liquid-transform/select-integration-account.png)

1. Genel **Bakış** bölmesinde, **Bileşenler** **altında, Haritalar'ı**seçin.

    !["Haritalar" döşemesi seçin](./media/logic-apps-enterprise-integration-liquid-transform/select-maps-tile.png)

1. **Haritalar** bölmesinde, **Ekle'yi** seçin ve haritanız için şu ayrıntıları sağlayın:

   | Özellik | Değer | Açıklama | 
   |----------|-------|-------------|
   | **Adı** | `JsonToJsonTemplate` | Bu örnekte "JsonToJsonTemplate" olan haritanızın adı | 
   | **Harita türü** | **Sıvı** | Haritanızın türü. JSON JSON dönüşümü için, **sıvı**seçmeniz gerekir. | 
   | **Harita** | `SimpleJsonToJsonTemplate.liquid` | Bu örnekte "SimpleJsonToJsonTemplate.liquid" olan dönüşüm için kullanılacak varolan bir Sıvı şablonu veya harita dosyası. Bu dosyayı bulmak için dosya seçiciyi kullanabilirsiniz. Eşboyutu sınırları için [Sınırlar ve yapılandırmaya](../logic-apps/logic-apps-limits-and-config.md#artifact-capacity-limits)bakın. |
   ||| 

   ![Sıvı şablonu ekle](./media/logic-apps-enterprise-integration-liquid-transform/add-liquid-template.png)
    
## <a name="add-the-liquid-action-for-json-transformation"></a>JSON dönüşümü için Sıvı eylem ekle

1. Azure portalında, [boş bir mantık uygulaması oluşturmak](../logic-apps/quickstart-create-first-logic-app-workflow.md)için aşağıdaki adımları izleyin.

1. Logic App Designer'da mantık uygulamanıza [İstek tetikleyicisini](../connectors/connectors-native-reqres.md#add-request) ekleyin.

1. Tetikleyicinin altında **Yeni adımı**seçin. Arama kutusuna filtreniz olarak girin `liquid` ve şu eylemi seçin: **JSON'u JSON'a dönüştürün - Sıvı**

   ![Sıvı eylemini bulma ve seçme](./media/logic-apps-enterprise-integration-liquid-transform/search-action-liquid.png)

1. **Harita** listesini açın ve bu örnekte "JsonToJsonTemplate" olan Sıvı şablonunuzu seçin.

   ![Harita seçin](./media/logic-apps-enterprise-integration-liquid-transform/select-map.png)

   Haritalar listesi boşsa, büyük olasılıkla mantık uygulamanız entegrasyon hesabınıza bağlı değildir. 
   Mantık uygulamanızı Sıvı şablonu veya haritasıolan tümleştirme hesabına bağlamak için aşağıdaki adımları izleyin:

   1. Mantık uygulaması menüsünde **İş Akışı ayarlarını**seçin.

   1. **Tümleştirme hesabı seç** listesinden, entegrasyon hesabınızı seçin ve **Kaydet'i**seçin.

      ![Mantık uygulamasını entegrasyon hesabına bağlama](./media/logic-apps-enterprise-integration-liquid-transform/link-integration-account.png)

1. Şimdi bu eyleme **İçerik** özelliğini ekleyin. Yeni **parametre** ekle listesini açın ve **İçerik'i**seçin.

   ![Eyleme "İçerik" özelliği ekleme](./media/logic-apps-enterprise-integration-liquid-transform/add-content-property-to-action.png)

1. **İçerik** özellik değerini ayarlamak için, dinamik içerik listesinin görünmesi için **İçerik** kutusunun içini tıklatın. Tetikden vücut içeriği çıktısını temsil eden **Gövde** belirteci'ni seçin.

   !["İçerik" özellik değeri için "Gövde" belirteci'ni seçin](./media/logic-apps-enterprise-integration-liquid-transform/select-body.png)

   İşlem tamamlandığında eylem şu örnekteki gibi görünür:

   !["Transform JSON to JSON" eylemi tamamlandı](./media/logic-apps-enterprise-integration-liquid-transform/finished-transform-action.png)

## <a name="test-your-logic-app"></a>Mantık uygulamanızı test edin

[Postman](https://www.getpostman.com/postman) veya benzer bir araçtan mantık uygulamanıza JSON girişi gönderin. Mantık uygulamanızdan dönüştürülmüş JSON çıkışı aşağıdaki örnek gibi görünür:
  
![Örnek çıktı](./media/logic-apps-enterprise-integration-liquid-transform/example-output-jsontojson.png)

## <a name="more-liquid-action-examples"></a>Diğer Sıvı eylem örnekleri
Sıvı sadece JSON dönüşümleri ile sınırlı değildir. Sıvı'yı kullanan diğer kullanılabilir dönüştürme eylemleri şunlardır.

* JSON'u metne dönüştürme
  
  Bu örnek için kullanılan Sıvı şablonu aşağıda verilmiştir:
   
   ``` json
   {{content.firstName | Append: ' ' | Append: content.lastName}}
   ```
   Örnek giriş ve çıktılar şunlardır:
  
   ![Metiniçin örnek çıktı JSON](./media/logic-apps-enterprise-integration-liquid-transform/example-output-jsontotext.png)

* XML'i JSON'a dönüştürün
  
  Bu örnek için kullanılan Sıvı şablonu aşağıda verilmiştir:
   
   ``` json
   [{% JSONArrayFor item in content -%}
        {{item}}
    {% endJSONArrayFor -%}]
   ```
   Örnek giriş ve çıktılar şunlardır:

   ![JSON'a örnek çıktı XML](./media/logic-apps-enterprise-integration-liquid-transform/example-output-xmltojson.png)

* XML'i metne dönüştürme
  
  Bu örnek için kullanılan Sıvı şablonu aşağıda verilmiştir:

   ``` json
   {{content.firstName | Append: ' ' | Append: content.lastName}}
   ```

   Örnek giriş ve çıktılar şunlardır:

   ![Örnek çıktı XML'den metne](./media/logic-apps-enterprise-integration-liquid-transform/example-output-xmltotext.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Kurumsal Entegrasyon Paketi hakkında daha fazla bilgi edinin](../logic-apps/logic-apps-enterprise-integration-overview.md "Kurumsal Entegrasyon Paketi hakkında bilgi edinin")  
* [Haritalar hakkında daha fazla bilgi edinin](../logic-apps/logic-apps-enterprise-integration-maps.md "Kurumsal entegrasyon haritaları hakkında bilgi edinin")  

