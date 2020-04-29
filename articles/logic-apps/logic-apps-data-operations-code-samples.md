---
title: Veri işlemlerini kullanmaya yönelik kod örnekleri
description: Azure Logic Apps ile oluşturulan otomatikleştirilmiş iş akışlarında veri işlemlerini nasıl kullanabileceğinizi gösteren bu kod örneklerini deneyin
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: conceptual
ms.date: 09/01/2019
ms.openlocfilehash: c56c31bb6f56efb10808ce25b6b232089391e831
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79270530"
---
# <a name="data-operation-code-samples-for-azure-logic-apps"></a>Azure Logic Apps için veri işlemi kodu örnekleri

Makalede veri işlemi eylem tanımlarının kod örnekleri aşağıda verilmiştir, [veri Işlemleri gerçekleştirin](../logic-apps/logic-apps-perform-data-operations.md). Bu örnekleri, kendi mantıksal uygulamanızın temel alınan iş akışı tanımı, Azure aboneliği ve API bağlantıları ile örnekleri denemek istediğinizde kullanabilirsiniz. Bu eylem tanımlarını kopyalayın ve mantıksal uygulamanızın iş akışı tanımı için kod görünümü düzenleyicisine yapıştırın ve ardından belirli iş akışınız için tanımları değiştirin. 

JavaScript Nesne Gösterimi (JSON) standartlarına dayalı olarak, bu eylem tanımları alfabetik sırada görünür. Ancak, mantıksal uygulama tasarımcısında her bir tanım iş akışınız dahilinde doğru sırada görünür, çünkü her bir eylem tanımının `runAfter` özelliği çalışma sırasını belirtir.

<a name="compose-action-example"></a>

## <a name="compose"></a>Oluştur

[ **Oluşturma** eylemi örneğini](../logic-apps/logic-apps-perform-data-operations.md#compose-action)denemek için kullanabileceğiniz eylem tanımları aşağıda verilmiştir:

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

[ **CSV tablosu oluşturma** eylem örneğini](../logic-apps/logic-apps-perform-data-operations.md#create-csv-table-action)denemek için kullanabileceğiniz eylem tanımları aşağıda verilmiştir:

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

[ **HTML tablosu oluşturma** eylem örneğini](../logic-apps/logic-apps-perform-data-operations.md#create-html-table-action)denemek için kullanabileceğiniz eylem tanımları aşağıda verilmiştir:

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

## <a name="filter-array"></a>Diziyi filtrele

[ **Filtre dizisi** eylem örneğini](../logic-apps/logic-apps-perform-data-operations.md#filter-array-action)denemek için kullanabileceğiniz eylem tanımları aşağıda verilmiştir:

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

[ **JOIN** eylemi örneğini](../logic-apps/logic-apps-perform-data-operations.md#join-action)denemek için kullanabileceğiniz eylem tanımları aşağıda verilmiştir:

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

[ **JSON** eylemi örneğini ayrıştırmayı](../logic-apps/logic-apps-perform-data-operations.md#parse-json-action)denemek için kullanabileceğiniz eylem tanımları aşağıda verilmiştir:

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

[ **Seçim** eylemi örneğini](../logic-apps/logic-apps-perform-data-operations.md#select-action)denemek için kullanabileceğiniz eylem tanımları aşağıda verilmiştir:

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
