---
title: Eylemleri kapsama göre gruplandırma ve çalıştırma
description: Azure Logic Apps'taki grup durumuna göre çalışan kapsamlı eylemler oluşturma
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.date: 10/03/2018
ms.topic: article
ms.openlocfilehash: b84db69f79b1611347a4c55d929e5426141e7ac6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74791497"
---
# <a name="run-actions-based-on-group-status-by-using-scopes-in-azure-logic-apps"></a>Azure Logic Apps'taki kapsamları kullanarak grup durumuna göre eylemleri çalıştırma

Eylemleri yalnızca başka bir eylem grubu başarılı veya başarısız olduktan sonra çalıştırmak için, bu eylemleri bir *kapsam*içinde gruplandırma. Bu yapı, eylemleri mantıksal bir grup olarak düzenlemek, grubun durumunu değerlendirmek ve kapsamın durumunu temel alan eylemler gerçekleştirmek istediğinizde yararlıdır. Bir kapsamdaki tüm eylemler çalışma ktan sonra, kapsam da kendi durumunu alır. Örneğin, [özel durum ve hata işleme](../logic-apps/logic-apps-exception-handling.md#scopes)uygulamak istediğinizde kapsamları kullanabilirsiniz. 

Bir kapsamın durumunu denetlemek için, "Başarılı", "Başarısız", "İptal Edildi" gibi bir mantık uygulamalarının çalışma durumunu belirlemek için kullandığınız ölçütleri kullanabilirsiniz. Varsayılan olarak, tüm kapsam eylemleri başarılı olduğunda, kapsamın durumu "Başarılı" olarak işaretlenir. Ancak kapsamdaki herhangi bir eylem başarısız olduğunda veya iptal edildiğinde, kapsamın durumu "Başarısız" olarak işaretlenir. Kapsamsınırlamaları için [Sınırlar ve config'e](../logic-apps/logic-apps-limits-and-config.md)bakın. 

Örneğin, belirli eylemleri çalıştırmak için bir kapsam ve kapsamın durumunu denetlemek için bir koşul kullanan üst düzey bir mantık uygulaması aşağıda verilmiştir. Kapsamdaki herhangi bir eylem beklenmeyen bir şekilde başarısız olursa veya sona ererse, kapsam sırasıyla "Başarısız" veya "İptal" olarak işaretlenir ve mantık uygulaması "Kapsam başarısız oldu" iletisi gönderir. Tüm kapsamlı eylemler başarılı olursa, mantık uygulaması bir "Kapsam başarılı" iletisi gönderir.

!["Zamanlama - Yineleme" tetikleyicisi ayarlama](./media/logic-apps-control-flow-run-steps-group-scopes/scope-high-level.png)

## <a name="prerequisites"></a>Ön koşullar

Bu makaledeki örneği izlemek için aşağıdaki öğelere ihtiyacınız vardır:

* Azure aboneliği. Aboneliğiniz yoksa, [ücretsiz bir Azure hesabı için kaydolun](https://azure.microsoft.com/free/). 

* Logic Apps tarafından desteklenen herhangi bir e-posta sağlayıcısından gelen bir e-posta hesabı. Bu örnekte Outlook.com kullanır. Farklı bir sağlayıcı kullanıyorsanız, genel akış aynı kalır, ancak uI'niz farklı görünür.

* Bing Haritalar anahtarı. Bu anahtarı almak için bing <a href="https://msdn.microsoft.com/library/ff428642.aspx" target="_blank">haritaları tuşu al'</a>a bakın.

* [Mantık uygulamaları oluşturma](../logic-apps/quickstart-create-first-logic-app-workflow.md) hakkında temel bilgiler

## <a name="create-sample-logic-app"></a>Örnek mantık uygulaması oluşturma

İlk olarak, daha sonra bir kapsam ekleyebilmeniz için bu örnek mantık uygulamasını oluşturun:

![Örnek mantık uygulaması oluşturma](./media/logic-apps-control-flow-run-steps-group-scopes/finished-sample-app.png)

* Zamanlama - Bing Haritalar hizmetini belirttiğiniz bir aralıkta kontrol eden **yineleme tetikleyicisi**
* Bing **Haritalar -** İki konum arasındaki seyahat süresini kontrol eden rota eylemi alın
* Seyahat süresinin belirtilen seyahat sürenizi aşıp aşmadığını kontrol eden koşullu bir bildirim
* Geçerli seyahat süresinin belirtilen süreyi aştığını e-posta yla gönderen bir eylem

Mantık uygulamanızı istediğiniz zaman kaydedebilirsiniz, bu nedenle çalışmanızı sık sık kaydedebilirsiniz.

1. Azure <a href="https://portal.azure.com" target="_blank">portalında</a>oturum açın , henüz yapmadıysanız. Boş bir mantıksal uygulama oluşturma.

1. Zamanlama ekle - Bu ayarlarla **yineleme** tetikleyicisi: **Interval** = "1" ve **Sıklık** = "Dakika"

   !["Zamanlama - Yineleme" tetikleyicisi ayarlama](./media/logic-apps-control-flow-run-steps-group-scopes/recurrence.png)

   > [!TIP]
   > Görünümünüzü görsel olarak basitleştirmek ve tasarımcıda her eylemin ayrıntılarını gizlemek için, bu adımlarda ilerlerken her eylemin şeklini daraltın.

1. Bing **Haritalarını** Ekleyin - Rota eylemini alın. 

   1. Bing Haritalar bağlantınız zaten yoksa, bir bağlantı oluşturmanız istenir.

      | Ayar | Değer | Açıklama |
      | ------- | ----- | ----------- |
      | **Bağlantı Adı** | BingMapsConnection | Bağlantınıza bir ad verin. | 
      | **API Anahtarı** | <*sizin-Bing-Haritalar-anahtar*> | Daha önce aldığınız Bing Haritalar anahtarını girin. | 
      ||||  

   1. Bu resmin altındaki tabloda gösterildiği gibi **Rota Al** eyleminizi ayarlayın:

      !["Bing Haritalar - Rota al" eylemini ayarlama](./media/logic-apps-control-flow-run-steps-group-scopes/get-route.png) 

      Bu parametreler hakkında daha fazla bilgi için bkz. [Rota hesaplama](https://msdn.microsoft.com/library/ff701717.aspx).

      | Ayar | Değer | Açıklama |
      | ------- | ----- | ----------- |
      | **Güzergah noktası 1** | <*Başlatmak*> | Rotanızın kaynağını girin. | 
      | **Güzergah noktası 2** | <*Son -unda*> | Rotanızın hedefini girin. | 
      | **Kaçının** | None | Rotanızda otoyollar, geçiş ücretleri ve benzeri nedenlerden kaçınmak için öğeleri girin. Olası değerler için [bkz.](https://msdn.microsoft.com/library/ff701717.aspx) | 
      | **Optimize** | timeWithTraffic | Rotanızı en iyi duruma getirmek için mesafe, geçerli trafik bilgileriyle birlikte zaman ve benzeri bir parametre seçin. Bu örnekte bu değer kullanır: "timeWithTraffic" | 
      | **Mesafe birimi** | <*sizin tercihiniz*> | Rotanızı hesaplamak için mesafe birimini girin. Bu örnekte bu değer kullanır: "Mile" | 
      | **Seyahat modu** | Sürüş | Rotanız için seyahat moduna girin. Bu örnekte bu değer "Sürüş" kullanır | 
      | **Toplu Ulaşım Tarih-Saati** | None | Yalnızca geçiş modu için geçerlidir. | 
      | **Transit Tarih Tipi** | None | Yalnızca geçiş modu için geçerlidir. | 
      ||||  

1. Trafikle birlikte geçerli seyahat süresinin belirli bir süreyi aşıp aşmadığını kontrol eden bir [koşul ekleyin.](../logic-apps/logic-apps-control-flow-conditional-statement.md) 
   Bu örnek için aşağıdaki adımları izleyin:

   1. Bu açıklamayla koşulu yeniden adlandırın: **Trafik süresi belirtilen saatten fazlaysa**

   1. En solsütunda, dinamik içerik listesinin görünmesi için **değer** kutusunu seç'ini tıklatın. Bu listeden, saniyecinsinden **olan Seyahat Süresi Trafiği** alanını seçin. 

      ![Koşul derleme](./media/logic-apps-control-flow-run-steps-group-scopes/build-condition.png)

   1. Orta kutuda, bu işleç seçin: **daha büyüktür**

   1. En sağdaki sütunda, saniye cinsinden ve 10 dakikaya eşdeğer olan bu karşılaştırma değerini girin: **600**

      İşlem tamamlandığında koşulunuz şu örnekteki gibi görünür:

      ![Bitmiş koşul](./media/logic-apps-control-flow-run-steps-group-scopes/finished-condition.png)

1. Gerçek **Solsa** şubesine, e-posta sağlayıcınız için bir "e-posta gönder" eylemi ekleyin. 
   Bu görüntünün altındaki adımları izleyerek bu eylemi ayarlayın:

   !["Doğruysa" şubesine "E-posta gönder" eylemi ekleme](./media/logic-apps-control-flow-run-steps-group-scopes/send-email.png)

   1. **To** alanında, test amacıyla e-posta adresinizi girin.

   1. **Konu** alanına şu metni girin:

      ```Time to leave: Traffic more than 10 minutes```

   1. **Gövde** alanına, bu metni bir uzama alanı yla girin: 

      ```Travel time:```

      İmleciniz **Gövde** alanında görünürken, dinamik içerik listesi açık kalır, böylece bu noktada kullanılabilen parametreleri seçebilirsiniz.

   1. Dinamik içerik listesinde **İfade**’yi seçin.

   1. **Div()** işlevini bulun ve seçin. 
      İmlecinizi işlevin parantezinin içine koyun.

   1. İmleciniz işlevin parantezinin içindeyken, dinamik içerik listesinin görünmesi için **Dinamik içeriği** seçin. 
   
   1. **Rotayı Al** bölümünden **Trafik Süresi Trafiği** alanını seçin.

      !["Trafik Süresi Trafiği" seçeneğini belirleyin](./media/logic-apps-control-flow-run-steps-group-scopes/send-email-2.png)

   1. Alan JSON biçimine çözüldükten sonra, Trafik```,```Süresi **Trafiği'ndeki** değeri saniyeden dakikaya dönüştürmek için sayıyı ```60``` takip eden bir **virgül** ekleyin. 
   
      ```
      div(body('Get_route')?['travelDurationTraffic'],60)
      ```

      İfadeniz şimdi şu örneğe benzer:

      ![İfadeyi bitir](./media/logic-apps-control-flow-run-steps-group-scopes/send-email-3.png)  

   1. İşiniz bittiğinde **Tamam**’ı seçin.

   <!-- markdownlint-disable MD038 -->
   1. İfade çözüldükten sonra, bu metni satır aralığıyla ekleyin:``` minutes```
  
       **Vücut** alanınız şimdi şu örnekte görünür:

       ![Bitmiş "Gövde" alanı](./media/logic-apps-control-flow-run-steps-group-scopes/send-email-4.png)
   <!-- markdownlint-enable MD038 -->

1. Mantıksal uygulamanızı kaydedin.

Ardından, belirli eylemleri gruplandırmak ve durumlarını değerlendirmek için bir kapsam ekleyin.

## <a name="add-a-scope"></a>Kapsam ekleme

1. Henüz yapmadıysanız, mantık uygulamanızı Logic App Designer'da açın. 

1. İş akışı konumuna istediğiniz kapsam ekleyin. Örneğin, mantık uygulaması iş akışındaki varolan adımlar arasına bir kapsam eklemek için aşağıdaki adımları izleyin: 

   1. İşaretçinizi kapsamı eklemek istediğiniz okun üzerine taşıyın. 
   Artı **işaretini** **+** seçin ( ) > **Eylem ekle**.

      ![Kapsam ekleme](./media/logic-apps-control-flow-run-steps-group-scopes/add-scope.png)

   1. Arama kutusuna filtreniz olarak "kapsam" girin. 
   **Kapsam** eylemini seçin.

## <a name="add-steps-to-scope"></a>Kapsama adımlar ekleme

1. Şimdi, kapsam içinde çalıştırmak istediğiniz adımları ekleyin veya sürükleyin. Bu örnekiçin, bu eylemleri kapsamına sürükleyin:
      
   * **Rotayı alın**
   * **Trafik süresi belirtilen süreden fazlaysa,** hem **doğru** hem de **yanlış** dalları içerir

   Mantık uygulamanız şimdi şu örneğe benzer:

   ![Kapsam eklendi](./media/logic-apps-control-flow-run-steps-group-scopes/scope-added.png)

1. Kapsam altında, kapsamın durumunu denetleyen bir koşul ekleyin. Bu açıklamayla koşulu yeniden adlandırın: **Kapsam başarısız olduysa**

   ![Kapsam durumunu denetlemek için koşul ekleme](./media/logic-apps-control-flow-run-steps-group-scopes/add-condition-check-scope-status.png)
  
1. Koşulda, kapsamın durumunun "Başarısız" veya "İptal" durumuna eşit olup olmadığını kontrol eden bu ifadeleri ekleyin. 

   1. Başka bir satır eklemek için **Ekle'yi**seçin. 

   1. Her satırda, dinamik içerik listesinin görünmesi için sol daki kutunun içini tıklatın. 
   Dinamik içerik listesinden **İfade'yi**seçin. Edit kutusuna, bu ifadeyi girin ve sonra **Tamam'ı**seçin: 
   
      `result('Scope')[0]['status']`

      ![Kapsam durumunu denetleyen ifade ekleme](./media/logic-apps-control-flow-run-steps-group-scopes/check-scope-status.png)

   1. Her iki satır için de seçil, işleç olarak **eşittir.** 
   
   1. Karşılaştırma değerleri için, ilk satırda. `Failed` 
   İkinci satırda, `Aborted`girin. 

      İşlem tamamlandığında koşulunuz şu örnekteki gibi görünür:

      ![Kapsam durumunu denetleyen ifade ekleme](./media/logic-apps-control-flow-run-steps-group-scopes/check-scope-status-finished.png)

      Şimdi, koşul kapsam `runAfter` durumunu denetler ve daha sonraki adımlarda tanımladığınız eşleşen eylemi çalıştırın şekilde koşulun özelliğini ayarlayın.

   1. Kapsam **başarısız olduğunda,** **elips (...)** düğmesini seçin ve **sonra 'dan sonra çalıştır'ı yapılandır'ı**seçin.

      !['runAfter' özelliğini yapılandırma](./media/logic-apps-control-flow-run-steps-group-scopes/configure-run-after.png)

   1. Tüm bu kapsam durumlarını seçin: **başarılı,** **başarısız oldu,** **atlandı**ve **zaman doldu**

      ![Kapsam durumlarını seçme](./media/logic-apps-control-flow-run-steps-group-scopes/select-run-after-statuses.png)

   1. İşiniz bittiğinde **Bitti**'yi seçin. 
   Koşul şimdi bir "bilgi" simgesi gösterir.

1. If **true** ve **If false** dallarında, her kapsam durumuna göre gerçekleştirmek istediğiniz eylemleri ekleyin, örneğin, bir e-posta veya ileti gönderin.

   ![Kapsam durumuna göre yapılacak eylemler ekleme](./media/logic-apps-control-flow-run-steps-group-scopes/handle-true-false-branches.png)

1. Mantıksal uygulamanızı kaydedin.

Bitmiş mantık uygulaması şimdi şu örnek gibi görünüyor:

![Kapsamlı bitmiş mantık uygulaması](./media/logic-apps-control-flow-run-steps-group-scopes/scopes-overview.png)

## <a name="test-your-work"></a>Çalışmanızı test edin

Tasarımcı araç çubuğunda **Çalıştır'ı**seçin. Tüm kapsamlı eylemler başarılı olursa, bir "Kapsam başarılı" iletisi alırsınız. Kapsamlı eylemler başarılı olmazsa, "Kapsam başarısız oldu" iletisi alırsınız. 

<a name="scopes-json"></a>

## <a name="json-definition"></a>JSON tanımı

Kod görünümünde çalışıyorsanız, bunun yerine mantık uygulamanızın JSON tanımında bir kapsam yapısı tanımlayabilirsiniz. Örneğin, bir önceki mantık uygulamasında tetikleyici ve eylemler için JSON tanımı aşağıda verilmiştir:

``` json
"triggers": {
  "Recurrence": {
    "type": "Recurrence",
    "recurrence": {
       "frequency": "Minute",
       "interval": 1
    }
  }
}
```

```json
"actions": {
  "If_scope_failed": {
    "type": "If",
    "actions": {
      "Scope_failed": {
        "type": "ApiConnection",
        "inputs": {
          "body": {
            "Body": "Scope failed. Scope status: @{result('Scope')[0]['status']}",
            "Subject": "Scope failed",
            "To": "<your-email@domain.com>"
          },
          "host": {
            "connection": {
              "name": "@parameters('$connections')['outlook']['connectionId']"
            }
          },
          "method": "post",
          "path": "/Mail"
        },
        "runAfter": {}
      }
    },
    "else": {
      "actions": {
        "Scope_succeded": {
          "type": "ApiConnection",
          "inputs": {
            "body": {
              "Body": "Scope succeeded. Scope status: @{result('Scope')[0]['status']}",
              "Subject": "Scope succeeded",
              "To": "<your-email@domain.com>"
            },
            "host": {
              "connection": {
               "name": "@parameters('$connections')['outlook']['connectionId']"
              }
            },
            "method": "post",
            "path": "/Mail"
          },
          "runAfter": {}
        }
      }
    },
    "expression": {
      "or": [ 
         {
            "equals": [ 
              "@result('Scope')[0]['status']", 
              "Failed"
            ]
         },
         {
            "equals": [
               "@result('Scope')[0]['status']", 
               "Aborted"
            ]
         } 
      ]
    },
    "runAfter": {
      "Scope": [
        "Failed",
        "Skipped",
        "Succeeded",
        "TimedOut"
      ]
    }
  },
  "Scope": {
    "type": "Scope",
    "actions": {
      "Get_route": {
        "type": "ApiConnection",
        "inputs": {
          "host": {
            "connection": {
              "name": "@parameters('$connections')['bingmaps']['connectionId']"
            }
          },
          "method": "get",
          "path": "/REST/V1/Routes/Driving",
          "queries": {
            "distanceUnit": "Mile",
            "optimize": "timeWithTraffic",
            "travelMode": "Driving",
            "wp.0": "<start>",
            "wp.1": "<end>"
          }
        },
        "runAfter": {}
      },
      "If_traffic_time_is_more_than_specified_time": {
        "type": "If",
        "actions": {
          "Send_mail_when_traffic_exceeds_10_minutes": {
            "type": "ApiConnection",
            "inputs": {
              "body": {
                 "Body": "Travel time:@{div(body('Get_route')?['travelDurationTraffic'],60)} minutes",
                 "Subject": "Time to leave: Traffic more than 10 minutes",
                 "To": "<your-email@domain.com>"
              },
              "host": {
                "connection": {
                   "name": "@parameters('$connections')['outlook']['connectionId']"
                }
              },
              "method": "post",
              "path": "/Mail"
            },
            "runAfter": {}
          }
        },
        "expression": {
          "and" : [
            {
               "greater": [ 
                  "@body('Get_route')?['travelDurationTraffic']", 
                  600
               ]
            }
          ]
        },
        "runAfter": {
          "Get_route": [
            "Succeeded"
          ]
        }
      }
    },
    "runAfter": {}
  }
},
```

## <a name="get-support"></a>Destek alın

* Sorularınız için [Azure Logic Apps forumunu](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) ziyaret edin.
* Özellikler ve öneriler göndermek veya oy kullanmak için [Azure Logic Apps kullanıcı geri bildirim sitesini](https://aka.ms/logicapps-wish)ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar

* [Koşula dayalı adımları çalıştırma (koşullu ifadeler)](../logic-apps/logic-apps-control-flow-conditional-statement.md)
* [Farklı değerlere dayalı adımları çalıştırma (geçiş deyimleri)](../logic-apps/logic-apps-control-flow-switch-statement.md)
* [Adımları çalıştırma ve yineleme (döngüler)](../logic-apps/logic-apps-control-flow-loops.md)
* [Paralel adımları çalıştırma veya birleştirme (dallar)](../logic-apps/logic-apps-control-flow-branches.md)
