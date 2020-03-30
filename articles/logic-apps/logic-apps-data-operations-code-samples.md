---
title: Veri işlemlerini kullanmak için kod örnekleri
description: Azure Logic Apps ile oluşturulan otomatik iş akışlarında veri işlemlerini nasıl kullanabileceğinizi gösteren bu kod örneklerini deneyin
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: conceptual
ms.date: 09/01/2019
ms.openlocfilehash: c56c31bb6f56efb10808ce25b6b232089391e831
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79270530"
---
# <a name="data-operation-code-samples-for-azure-logic-apps"></a>Azure Logic Apps için veri işlem kodu örnekleri

Burada makalede veri işlemi eylem tanımları için kod örnekleri, [veri işlemleri gerçekleştirin.](../logic-apps/logic-apps-perform-data-operations.md) Bu örnekleri, kendi mantık uygulamanızın temel iş akışı tanımı, Azure aboneliği ve API bağlantılarıyla örnekleri denemek istediğinizde kullanabilirsiniz. Bu eylem tanımlarını mantık uygulamanızın iş akışı tanımı için kod görünümü düzenleyicisine kopyalayıp yapıştırın ve ardından belirli iş akışınız için tanımları değiştirin. 

JavaScript Nesne Gösterimi (JSON) standartlarına göre, bu eylem tanımları alfabetik sırada görünür. Ancak, Logic App Designer'da, her eylem tanımının `runAfter` özelliği çalışma sırasını belirttiğinden, her tanım iş akışınızda doğru sırada görünür.

<a name="compose-action-example"></a>

## <a name="compose"></a>Oluştur

[ **Eylem Oluştur** örneğini](../logic-apps/logic-apps-perform-data-operations.md#compose-action)denemek için kullanabileceğiniz eylem tanımları aşağıda verilmiştir:

```json
"actions": {
  "Compose": {
    "type": "Compose",
    "inputs": {
      "age": "@variables('ageVar')",
      "fullName": "@{variables('lastNameVar')}, @{variables('firstNameVar')}"
    },
    "runAfter": {
      "Initialize_variable_-_ageVar": [
          "Succeeded"
      ]
    }
  },
  "Initialize_variable_-_ageVar": {
    "type": "InitializeVariable",
    "inputs": {
      "variables": [
        {
          "name": "ageVar",
          "type": "Integer",
          "value": 35
        }
      ]
    },
    "runAfter": {
      "Initialize_variable_-_lastNameVar": [
        "Succeeded"
      ]
    }
  },
  "Initialize_variable_-_firstNameVar": {
    "type": "InitializeVariable",
    "inputs": {
      "variables": [
        {
          "name": "firstNameVar",
          "type": "String",
          "value": "Sophie "
        }
      ]
    },
    "runAfter": {}
  },
  "Initialize_variable_-_lastNameVar": {
    "type": "InitializeVariable",
    "inputs": {
      "variables": [
        {
          "name": "lastNameVar",
          "type": "String",
          "value": "Owen"
        }
      ]
    },
    "runAfter": {
      "Initialize_variable_-_firstNameVar": [
        "Succeeded"
      ]
    }
  }
},
```

<a name="create-csv-table-action-example"></a>

## <a name="create-csv-table"></a>CSV tablosu oluşturma

[ **CSV tablo** eylem işlemleri çalışma çalıÅ](../logic-apps/logic-apps-perform-data-operations.md#create-csv-table-action)ımını denemek için kullanıcılarınız eylem tanımları aşağıda verilmiştir:

```json
"actions": {
   "Create_CSV_table": {
      "type": "Table",     
      "inputs": {
         "format": "CSV",
         "from": "@variables('myJSONArray')"
      },
      "runAfter": {
         "Initialize_variable_-_JSON_array": [
            "Succeeded"
         ]
      }
   },
   "Initialize_variable_-_JSON_array": {
      "type": "InitializeVariable",
      "inputs": {
         "variables": [ 
            {
               "name": "myJSONArray",
               "type": "Array",
                  "value": [
                     {
                        "Description": "Apples",
                        "Product_ID": 1
                     },
                     {
                        "Description": "Oranges",
                        "Product_ID": 2
                     }
                  ]
            }
         ]
      },
      "runAfter": {}
   }
},
```

<a name="create-html-table-action-example"></a>

## <a name="create-html-table"></a>HTML tablosu oluşturma

[ **HTML tablo** eylem işlemleri çalıÅ](../logic-apps/logic-apps-perform-data-operations.md#create-html-table-action)tırıcı çalışmak için kullanıcılık tanımları ve şeyi:

```json
"actions": {
   "Create_HTML_table": {
      "type": "Table",     
      "inputs": {
         "format": "HTML",
         "from": "@variables('myJSONArray')"
      },
      "runAfter": {
         "Initialize_variable_-_JSON_array": [
            "Succeeded"
         ]
      }
   },
   "Initialize_variable_-_JSON_array": {
      "type": "InitializeVariable",
      "inputs": {
         "variables": [ 
            {
               "name": "myJSONArray",
               "type": "Array",
                  "value": [
                     {
                        "Description": "Apples",
                        "Product_ID": 1
                     },
                     {
                        "Description": "Oranges",
                        "Product_ID": 2
                     }
                  ]
            }
         ]
      },
      "runAfter": {}
   }
},
```

<a name="filter-array-action-example"></a>

## <a name="filter-array"></a>Filtre dizisi

[ **Filtre dizisi** eylem örneğini](../logic-apps/logic-apps-perform-data-operations.md#filter-array-action)denemek için, kullanabileceğiniz eylem tanımları şunlardır:

```json
"actions": {
   "Filter_array": {
      "type": "Query",
      "inputs": {
         "from": "@variables('myIntegerArray')",
         "where": "@greater(item(), 1)"
      },
      "runAfter": {
         "Initialize_variable_-_integer_array": [
            "Succeeded"
         ]
      }
   },
   "Initialize_variable_-_integer_array": {
      "type": "InitializeVariable",
      "inputs": {
         "variables": [ 
            {
               "name": "myIntegerArray",
               "type": "Array",
               "value": [
                  1,
                  2,
                  3,
                  4
               ]
            }
         ]
      },
      "runAfter": {}
   }
},
```

<a name="join-action-example"></a>

## <a name="join"></a>Birleştir

[ **Eyleme Katıl** örneğini](../logic-apps/logic-apps-perform-data-operations.md#join-action)denemek için kullanabileceğiniz eylem tanımları aşağıda verilmiştir:

```json
"actions": {
   "Initialize_variable_-_integer_array": {
      "type": "InitializeVariable",
      "inputs": {
         "variables": [ 
            {
               "name": "myIntegerArray",
               "type": "Array",
               "value": [
                  1,
                  2,
                  3,
                  4
               ]
            }
         ]
      },
      "runAfter": {}
   },
   "Join": {
      "type": "Join",
      "inputs": {
         "from": "@variables('myIntegerArray')",
         "joinWith": ":"
      },
      "runAfter": {
         "Initialize_variable_-_integer_array": [
             "Succeeded"
         ]
      }
   }
},
```

<a name="parse-json-action-example"></a>

## <a name="parse-json"></a>JSON Ayrıştır

[ **Parse JSON** eylem örneğini](../logic-apps/logic-apps-perform-data-operations.md#parse-json-action)denemek için, burada kullanabileceğiniz eylem tanımları şunlardır:

```json
"actions": {
   "Initialize_variable_-_JSON_object": {
      "type": "InitializeVariable",
      "inputs": {
         "variables": [
            {
               "name": "myJSONObject",
               "type": "Object",
               "value": {
                  "Member": {
                     "Email": "Sophie.Owen@contoso.com",
                     "FirstName": "Sophie",
                     "LastName": "Owen"
                  }
               }
            }
         ]
      },
      "runAfter": {}
   },
   "Parse_JSON": {
      "type": "ParseJson",
      "inputs": {
         "content": "@variables('myJSONObject')",
         "schema": {
            "type": "object",
            "properties": {
               "Member": {
                  "type": "object",
                  "properties": {
                     "Email": {
                        "type": "string"
                     },
                     "FirstName": {
                        "type": "string"
                     },
                     "LastName": {
                        "type": "string"
                     }
                  }
               }
            }
         }
      },
      "runAfter": {
         "Initialize_variable_-_JSON_object": [
            "Succeeded"
         ]
      }
},
```

<a name="select-action-example"></a>

## <a name="select"></a>Şunu seçin:

[ **Eylem seç** örneğini](../logic-apps/logic-apps-perform-data-operations.md#select-action)denemek için kullanabileceğiniz eylem tanımları aşağıda verilmiştir:

```json
"actions": {
   "Initialize_variable_-_integer_array": {
      "type": "InitializeVariable",
      "inputs": {
         "variables": [ 
            {
               "name": "myIntegerArray",
               "type": "Array",
               "value": [
                  1,
                  2,
                  3,
                  4
               ]
            }
         ]
      },
      "runAfter": {}
   },
   "Select": {
      "type": "Select",
      "inputs": {
         "from": "@variables('myIntegerArray')",
         "select": {
            "Product_ID": "@item()"
         }
      },
      "runAfter": {
         "Initialize_variable_-_integer_array": [
           "Succeeded"
         ]
      }
   }
},
```

## <a name="next-steps"></a>Sonraki adımlar

* [Veri işlemleri gerçekleştirme](../logic-apps/logic-apps-perform-data-operations.md)
