---
title: Azure Blockchain Workbench yapılandırma meta veri başvurusu
description: Azure Blockchain Workbench Önizleme uygulama yapılandırma meta veri genel bakış.
ms.date: 12/09/2019
ms.topic: article
ms.reviewer: brendal
ms.openlocfilehash: 661e795f0e85f872b1072a8f641b8938115c5d7a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79252200"
---
# <a name="azure-blockchain-workbench-configuration-reference"></a>Azure Blockchain Workbench yapılandırma başvurusu

Azure Blockchain Workbench uygulamaları, yapılandırma meta verileri ve akıllı sözleşme kodu yla tanımlanan çok taraflı iş akışlarıdır. Yapılandırma meta verileri blockchain uygulamasının üst düzey iş akışlarını ve etkileşim modelini tanımlar. Akıllı sözleşmeler blockchain uygulamasının iş mantığını tanımlar. Workbench, blockchain uygulaması kullanıcı deneyimleri oluşturmak için yapılandırma ve akıllı sözleşme kodu kullanır.

Yapılandırma meta verileri her blockchain uygulaması için aşağıdaki bilgileri belirtir:

* Blockchain uygulamasının adı ve açıklaması
* Blockchain uygulamasında hareket edebilen veya katılabilen kullanıcılar için benzersiz roller
* Bir veya daha fazla iş akışı. Her iş akışı, iş mantığının akışını denetlemek için bir durum makinesi gibi davranır. İş akışları bağımsız olabilir veya birbiriyle etkileşimde bulunabilir.

Tanımlanan her iş akışı aşağıdakileri belirtir:

* İş akışının adı ve açıklaması
* İş akışının durumları.  Her durum iş mantığının kontrol akışında bir aşamadır. 
* Bir sonraki duruma geçiş için eylemler
* Her eylemi başlatmaya izin verilen kullanıcı rolleri
* Kod dosyalarında iş mantığını temsil eden akıllı sözleşmeler

## <a name="application"></a>Uygulama

Blockchain uygulaması, yapılandırma meta verilerini, iş akışlarını ve uygulama içinde hareket edebilen veya uygulama içinde yer alabilen kullanıcı rollerini içerir.

| Alan | Açıklama | Gerekli |
|-------|-------------|:--------:|
| ApplicationName | Benzersiz uygulama adı. İlgili akıllı sözleşme, ilgili sözleşme sınıfı için aynı **ApplicationName'i** kullanmalıdır.  | Evet |
| DisplayName | Uygulamanın samimi ekran adı. | Evet |
| Açıklama | Uygulamanın açıklaması. | Hayır |
| Uygulama Rolleri | [Uygulama](#application-roles)Rolleri Koleksiyonu . Uygulama içinde hareket edebilen veya katılabilecek kullanıcı rolleri.  | Evet |
| İş akışları | İş [Akışlarının](#workflows)Toplanması . Her iş akışı, iş mantığının akışını denetlemek için bir durum makinesi gibi davranır. | Evet |

Örneğin, yapılandırma [dosyası örneğine](#configuration-file-example)bakın.

## <a name="workflows"></a>İş akışları

Bir uygulamanın iş mantığı, bir eylemde bulunmanın iş mantığının akışının bir durumdan diğerine geçmesine neden olduğu bir durum makinesi olarak modellenebilir. İş akışı, bu tür durumların ve eylemlerin bir koleksiyonudur. Her iş akışı, kod dosyalarındaki iş mantığını temsil eden bir veya daha fazla akıllı sözleşmeden oluşur. Çalıştırılabilir sözleşme, iş akışının bir örneğidir.

| Alan | Açıklama | Gerekli | Uzunluk üst sınırı |
|-------|-------------|:--------:|-----------:|
| Adı | Benzersiz iş akışı adı. İlgili akıllı sözleşme, ilgili sözleşme sınıfı için aynı **Adı** kullanmalıdır. | Evet | 50 |
| DisplayName | İş akışının samimi görüntü adı. | Evet | 255 |
| Açıklama | İş akışının açıklaması. | Hayır | 255 |
| Başlatıcı | [Uygulama](#application-roles)Rolleri Koleksiyonu . İş akışında sözleşme oluşturmaya yetkili kullanıcılara atanan roller. | Evet | |
| Başlangıç Durumu | İş akışının ilk durumunun adı. | Evet | |
| Özellikler | [Tanımlayıcıların](#identifiers)toplanması . Zincirdışında okunabilen veya bir kullanıcı deneyimi aracında görselleştirilebilen verileri temsil eder. | Evet | |
| Oluşturucu | İş akışının bir örneğini oluşturmak için giriş parametrelerini tanımlar. | Evet | |
| İşlevler | İş akışında yürütülebilecek [işlevler](#functions) topluluğu. | Evet | |
| Durumlar | İş akışı [durumları](#states)koleksiyonu. | Evet | |

Örneğin, yapılandırma [dosyası örneğine](#configuration-file-example)bakın.

## <a name="type"></a>Tür

Desteklenen veri türleri.

| Tür | Açıklama |
|-------|-------------|
| adres  | *Sözleşmeler* veya *kullanıcılar*gibi Blockchain adres türü. |
| array    | Tip insa, bool, para veya zaman tek seviyeli dizi. Diziler statik veya dinamik olabilir. Dizi içindeki öğelerin veri türünü belirtmek için **ElementType'i** kullanın. [Bkz. örnek yapılandırma.](#example-configuration-of-type-array) |
| bool     | Boolean veri türü. |
| Sözleşme | Tür sözleşmesinin adresi. |
| enum     | Numaralandırılmış değerler kümesi numaralandırılmış. Enum türünü kullanırken, EnumValues listesini de belirtirsiniz. Her değer 255 karakterle sınırlıdır. Geçerli değer karakterleri büyük ve küçük harfler (A-Z, a-z) ve sayılar (0-9) içerir. [Solidity'deki örnek yapılandırma ve kullanıma](#example-configuration-of-type-enum)bakın. |
| int      | Sonsayı veri türü. |
| Para    | Para veri türü. |
| durum    | İş akışı durumu. |
| string  | Dize veri türü. Maksimum 4000 karakter. [Bkz. örnek yapılandırma.](#example-configuration-of-type-string) |
| kullanıcı     | Tür kullanıcıadresi. |
| time     | Zaman veri türü. |
|`[ Application Role Name ]`| Uygulama rolünde belirtilen herhangi bir ad. Kullanıcıları bu rol türünden olarak sınırlar. |

### <a name="example-configuration-of-type-array"></a>Tür dizisinin örnek yapılandırması

```json
{
  "Name": "Quotes",
  "Description": "Market quotes",
  "DisplayName": "Quotes",
  "Type": {
    "Name": "array",
    "ElementType": {
        "Name": "int"
    }
  }
}
```

#### <a name="using-a-property-of-type-array"></a>Tür dizisi özelliğini kullanma

Bir özelliği yapılandırmada tür diziolarak tanımlıyorsanız, Katılık'ta dizi türünün ortak özelliğini döndürmek için açık bir get işlevi eklemeniz gerekir. Örnek:

```
function GetQuotes() public constant returns (int[]) {
     return Quotes;
}
```

### <a name="example-configuration-of-type-string"></a>Tür dizesinin örnek yapılandırması

``` json
{
  "Name": "description",
  "Description": "Descriptive text",
  "DisplayName": "Description",
  "Type": {
    "Name": "string"
  }
}
```

### <a name="example-configuration-of-type-enum"></a>Tür enum örnek yapılandırma

``` json
{
  "Name": "PropertyType",
  "DisplayName": "Property Type",
  "Description": "The type of the property",
  "Type": {
    "Name": "enum",
    "EnumValues": ["House", "Townhouse", "Condo", "Land"]
  }
}
```

#### <a name="using-enumeration-type-in-solidity"></a>Solidity'de numaralandırma türünü kullanma

Bir numaralandırma yapılandırmada tanımlandıktan sonra, Numaralandırma türlerini Solidity'de kullanabilirsiniz. Örneğin, PropertyTypeEnum adlı bir enum tanımlayabilirsiniz.

```
enum PropertyTypeEnum {House, Townhouse, Condo, Land} PropertyTypeEnum public PropertyType; 
```

Blockchain Workbench'te geçerli ve tutarlı bildirimler olması için yapılandırma ve akıllı sözleşme arasında eşleşmesi gereken dizelerin listesi.

Atama örneği:

```
PropertyType = PropertyTypeEnum.Townhouse;
```

Fonksiyon parametresi örneği: 

``` 
function AssetTransfer(string description, uint256 price, PropertyTypeEnum propertyType) public
{
    InstanceOwner = msg.sender;
    AskingPrice = price;
    Description = description;
    PropertyType = propertyType;
    State = StateType.Active;
    ContractCreated();
}

```

## <a name="constructor"></a>Oluşturucu

İş akışı örneği için giriş parametrelerini tanımlar.

| Alan | Açıklama | Gerekli |
|-------|-------------|:--------:|
| Parametreler | Akıllı bir sözleşme başlatmak için gereken [tanımlayıcıların](#identifiers) toplanması. | Evet |

### <a name="constructor-example"></a>Yapıcı örneği

``` json
{
  "Parameters": [
    {
      "Name": "description",
      "Description": "The description of this asset",
      "DisplayName": "Description",
      "Type": {
        "Name": "string"
      }
    },
    {
      "Name": "price",
      "Description": "The price of this asset",
      "DisplayName": "Price",
      "Type": {
        "Name": "money"
      }
    }
  ]
}
```

## <a name="functions"></a>İşlevler

İş akışında yürütülebilecek işlevleri tanımlar.

| Alan | Açıklama | Gerekli | Uzunluk üst sınırı |
|-------|-------------|:--------:|-----------:|
| Adı | İşlevin benzersiz adı. İlgili akıllı sözleşme, geçerli işlev için aynı **Adı** kullanmalıdır. | Evet | 50 |
| DisplayName | Fonksiyonun dostça görüntü adı. | Evet | 255 |
| Açıklama | Fonksiyonun açıklaması | Hayır | 255 |
| Parametreler | İşlevin parametrelerine karşılık gelen [tanımlayıcıların](#identifiers) toplanması. | Evet | |

### <a name="functions-example"></a>Fonksiyonlar örneği

``` json
"Functions": [
  {
    "Name": "Modify",
    "DisplayName": "Modify",
    "Description": "Modify the description/price attributes of this asset transfer instance",
    "Parameters": [
      {
        "Name": "description",
        "Description": "The new description of the asset",
        "DisplayName": "Description",
        "Type": {
          "Name": "string"
        }
      },
      {
        "Name": "price",
        "Description": "The new price of the asset",
        "DisplayName": "Price",
        "Type": {
          "Name": "money"
        }
      }
    ]
  },
  {
    "Name": "Terminate",
    "DisplayName": "Terminate",
    "Description": "Used to cancel this particular instance of asset transfer",
    "Parameters": []
  }
]

```

## <a name="states"></a>Durumlar

İş akışı içindeki benzersiz durumların bir koleksiyonu. Her durum iş mantığının kontrol akışında bir adım yakalar. 

| Alan | Açıklama | Gerekli | Uzunluk üst sınırı |
|-------|-------------|:--------:|-----------:|
| Adı | Devletin eşsiz adı. İlgili akıllı sözleşme, geçerli durum için aynı **Adı** kullanmalıdır. | Evet | 50 |
| DisplayName | Devletin dostça görüntü adı. | Evet | 255 |
| Açıklama | Devletin tanımı. | Hayır | 255 |
| Percentcomplete | İş mantığı kontrol akışındaki ilerlemeyi göstermek için Blockchain Workbench kullanıcı arabiriminde görüntülenen bir sonda değeri. | Evet | |
| Stil | Durum bir başarı veya başarısızlık durumunu temsil eder mi gösteren görsel ipucu. İki geçerli değer `Success` vardır: veya `Failure`. | Evet | |
| Geçişler | Kullanılabilir [geçişlerin](#transitions) geçerli durumdan sonraki durum kümesine toplanması. | Hayır | |

### <a name="states-example"></a>Durumlar örneği

``` json
"States": [
    {
      "Name": "Active",
      "DisplayName": "Active",
      "Description": "The initial state of the asset transfer workflow",
      "PercentComplete": 20,
      "Style": "Success",
      "Transitions": [
        {
          "AllowedRoles": [],
          "AllowedInstanceRoles": [ "InstanceOwner" ],
          "Description": "Cancels this instance of asset transfer",
          "Function": "Terminate",
          "NextStates": [ "Terminated" ],
          "DisplayName": "Terminate Offer"
        },
        {
          "AllowedRoles": [ "Buyer" ],
          "AllowedInstanceRoles": [],
          "Description": "Make an offer for this asset",
          "Function": "MakeOffer",
          "NextStates": [ "OfferPlaced" ],
          "DisplayName": "Make Offer"
        },
        {
          "AllowedRoles": [],
          "AllowedInstanceRoles": [ "InstanceOwner" ],
          "Description": "Modify attributes of this asset transfer instance",
          "Function": "Modify",
          "NextStates": [ "Active" ],
          "DisplayName": "Modify"
        }
      ]
    },
    {
      "Name": "Accepted",
      "DisplayName": "Accepted",
      "Description": "Asset transfer process is complete",
      "PercentComplete": 100,
      "Style": "Success",
      "Transitions": []
    },
    {
      "Name": "Terminated",
      "DisplayName": "Terminated",
      "Description": "Asset transfer has been canceled",
      "PercentComplete": 100,
      "Style": "Failure",
      "Transitions": []
    }
  ]
```

## <a name="transitions"></a>Geçişler

Bir sonraki duruma kullanılabilir eylemler. Bir veya daha fazla kullanıcı rolü, bir eylemin iş akışındaki bir durumu başka bir duruma geçirebileceği her durumda bir eylem gerçekleştirebilir. 

| Alan | Açıklama | Gerekli |
|-------|-------------|:--------:|
| İzin Verilen Roller | Geçişi başlatmak için izin verilen uygulama rollerinin listesi. Belirtilen rolün tüm kullanıcıları eylemi gerçekleştirebilir. | Hayır |
| İzin Verilen Örnek Roller | Geçişin başlatılmasına izin verilen akıllı sözleşmeye katılan veya belirtilen kullanıcı rollerinin listesi. Örnek rolleri, iş akışları içindeki **Özellikler'de** tanımlanır. AllowedInstanceRoles, akıllı bir sözleşme örneğine katılan bir kullanıcıyı temsil eder. İzin VerilenInstanceRoles, bir sözleşme örneğinde bir eylemi kullanıcı rolüyle sınırlama olanağı sağlar.  Örneğin, yalnızca sözleşmeyi (InstanceOwner) oluşturan kullanıcının, AllowRoles'da rolü belirttiğiniz tüm rol türündeki (Sahip) kullanıcılar yerine sonlandırmasına izin vermek isteyebilirsiniz. | Hayır |
| DisplayName | Geçiş in dostu ekran adı. | Evet |
| Açıklama | Geçişin açıklaması. | Hayır |
| İşlev | Geçişi başlatmak için işlevin adı. | Evet |
| Sonraki Durumlar | Başarılı bir geçişten sonra gelecek olası durumların bir koleksiyonu. | Evet |

### <a name="transitions-example"></a>Geçişler örneği

``` json
"Transitions": [
  {
    "AllowedRoles": [],
    "AllowedInstanceRoles": [ "InstanceOwner" ],
    "Description": "Cancels this instance of asset transfer",
    "Function": "Terminate",
    "NextStates": [ "Terminated" ],
    "DisplayName": "Terminate Offer"
  },
  {
    "AllowedRoles": [ "Buyer" ],
    "AllowedInstanceRoles": [],
    "Description": "Make an offer for this asset",
    "Function": "MakeOffer",
    "NextStates": [ "OfferPlaced" ],
    "DisplayName": "Make Offer"
  },
  {
    "AllowedRoles": [],
    "AllowedInstanceRoles": [ "InstanceOwner" ],
    "Description": "Modify attributes of this asset transfer instance",
    "Function": "Modify",
    "NextStates": [ "Active" ],
    "DisplayName": "Modify"
  }
]

```

## <a name="application-roles"></a>Uygulama rolleri

Uygulama rolleri, uygulama içinde hareket etmek veya katılmak isteyen kullanıcılara atanabilecek bir dizi rol tanımlar. Uygulama rolleri, blockchain uygulaması ve ilgili iş akışları içindeki eylemleri ve katılımı kısıtlamak için kullanılabilir. 

| Alan | Açıklama | Gerekli | Uzunluk üst sınırı |
|-------|-------------|:--------:|-----------:|
| Adı | Uygulama rolünün benzersiz adı. İlgili akıllı sözleşme, geçerli rol için aynı **Adı** kullanmalıdır. Temel tür adları ayrılmıştır. [Tür'le](#type) aynı ada sahip bir uygulama rolünü adlandıramazsınız| Evet | 50 |
| Açıklama | Uygulama rolünün açıklaması. | Hayır | 255 |

### <a name="application-roles-example"></a>Uygulama rolleri örneği

``` json
"ApplicationRoles": [
  {
    "Name": "Appraiser",
    "Description": "User that signs off on the asset price"
  },
  {
    "Name": "Buyer",
    "Description": "User that places an offer on an asset"
  }
]
```
## <a name="identifiers"></a>Tanımlayıcılar

Tanımlayıcılar, iş akışı özelliklerini, oluşturucuve işlev parametrelerini tanımlamak için kullanılan bir bilgi koleksiyonunu temsil eder. 

| Alan | Açıklama | Gerekli | Uzunluk üst sınırı |
|-------|-------------|:--------:|-----------:|
| Adı | Özelliğin veya parametrenin benzersiz adı. İlgili akıllı sözleşme, ilgili özellik veya parametre için aynı **Adı** kullanmalıdır. | Evet | 50 |
| DisplayName | Özellik veya parametre için dostça görüntü adı. | Evet | 255 |
| Açıklama | Özelliğin veya parametrenin açıklaması. | Hayır | 255 |
| Tür | Özellik [veri türü.](#type) | Evet |

### <a name="identifiers-example"></a>Tanımlayıcılar örneği

``` json
"Properties": [
  {
    "Name": "State",
    "DisplayName": "State",
    "Description": "Holds the state of the contract",
    "Type": {
      "Name": "state"
    }
  },
  {
    "Name": "Description",
    "DisplayName": "Description",
    "Description": "Describes the asset being sold",
    "Type": {
      "Name": "string"
    }
  }
]
```

## <a name="configuration-file-example"></a>Yapılandırma dosyası örneği

Varlık transferi, bir denetçi ve eksper gerektiren yüksek değerli varlıkları satın almak ve satmak için akıllı bir sözleşme senaryosudur. Satıcılar bir varlık transferi akıllı sözleşme anında kendi varlıklarını listeleyebilirsiniz. Alıcılar akıllı sözleşme üzerinde bir eylemde bulunarak teklifyapabilir ve diğer taraflar varlığı incelemek veya değerlendirmek için harekete geçebilirsiniz. Kıymet hem denetlenir hem de değerlendirildi olarak işaretlendikten sonra, alıcı ve satıcı sözleşme tamamlanmadan önce satışı yeniden onaylar. Sürecin her noktasında, tüm katılımcılar güncelleştirildikçe sözleşmenin durumuna görünürlüğe sahiptir. 

Kod dosyaları da dahil olmak üzere daha fazla bilgi [için Azure Blockchain Workbench için varlık transferi örneğine](https://github.com/Azure-Samples/blockchain/tree/master/blockchain-workbench/application-and-smart-contract-samples/asset-transfer) bakın

Aşağıdaki yapılandırma dosyası varlık transferi örneği içindir:

``` json
{
  "ApplicationName": "AssetTransfer",
  "DisplayName": "Asset Transfer",
  "Description": "Allows transfer of assets between a buyer and a seller, with appraisal/inspection functionality",
  "ApplicationRoles": [
    {
      "Name": "Appraiser",
      "Description": "User that signs off on the asset price"
    },
    {
      "Name": "Buyer",
      "Description": "User that places an offer on an asset"
    },
    {
      "Name": "Inspector",
      "Description": "User that inspects the asset and signs off on inspection"
    },
    {
      "Name": "Owner",
      "Description": "User that signs off on the asset price"
    }
  ],
  "Workflows": [
    {
      "Name": "AssetTransfer",
      "DisplayName": "Asset Transfer",
      "Description": "Handles the business logic for the asset transfer scenario",
      "Initiators": [ "Owner" ],
      "StartState":  "Active",
      "Properties": [
        {
          "Name": "State",
          "DisplayName": "State",
          "Description": "Holds the state of the contract",
          "Type": {
            "Name": "state"
          }
        },
        {
          "Name": "Description",
          "DisplayName": "Description",
          "Description": "Describes the asset being sold",
          "Type": {
            "Name": "string"
          }
        },
        {
          "Name": "AskingPrice",
          "DisplayName": "Asking Price",
          "Description": "The asking price for the asset",
          "Type": {
            "Name": "money"
          }
        },
        {
          "Name": "OfferPrice",
          "DisplayName": "Offer Price",
          "Description": "The price being offered for the asset",
          "Type": {
            "Name": "money"
          }
        },
        {
          "Name": "InstanceAppraiser",
          "DisplayName": "Instance Appraiser",
          "Description": "The user that appraises the asset",
          "Type": {
            "Name": "Appraiser"
          }
        },
        {
          "Name": "InstanceBuyer",
          "DisplayName": "Instance Buyer",
          "Description": "The user that places an offer for this asset",
          "Type": {
            "Name": "Buyer"
          }
        },
        {
          "Name": "InstanceInspector",
          "DisplayName": "Instance Inspector",
          "Description": "The user that inspects this asset",
          "Type": {
            "Name": "Inspector"
          }
        },
        {
          "Name": "InstanceOwner",
          "DisplayName": "Instance Owner",
          "Description": "The seller of this particular asset",
          "Type": {
            "Name": "Owner"
          }
        }
      ],
      "Constructor": {
        "Parameters": [
          {
            "Name": "description",
            "Description": "The description of this asset",
            "DisplayName": "Description",
            "Type": {
              "Name": "string"
            }
          },
          {
            "Name": "price",
            "Description": "The price of this asset",
            "DisplayName": "Price",
            "Type": {
              "Name": "money"
            }
          }
        ]
      },
      "Functions": [
        {
          "Name": "Modify",
          "DisplayName": "Modify",
          "Description": "Modify the description/price attributes of this asset transfer instance",
          "Parameters": [
            {
              "Name": "description",
              "Description": "The new description of the asset",
              "DisplayName": "Description",
              "Type": {
                "Name": "string"
              }
            },
            {
              "Name": "price",
              "Description": "The new price of the asset",
              "DisplayName": "Price",
              "Type": {
                "Name": "money"
              }
            }
          ]
        },
        {
          "Name": "Terminate",
          "DisplayName": "Terminate",
          "Description": "Used to cancel this particular instance of asset transfer",
          "Parameters": []
        },
        {
          "Name": "MakeOffer",
          "DisplayName": "Make Offer",
          "Description": "Place an offer for this asset",
          "Parameters": [
            {
              "Name": "inspector",
              "Description": "Specify a user to inspect this asset",
              "DisplayName": "Inspector",
              "Type": {
                "Name": "Inspector"
              }
            },
            {
              "Name": "appraiser",
              "Description": "Specify a user to appraise this asset",
              "DisplayName": "Appraiser",
              "Type": {
                "Name": "Appraiser"
              }
            },
            {
              "Name": "offerPrice",
              "Description": "Specify your offer price for this asset",
              "DisplayName": "Offer Price",
              "Type": {
                "Name": "money"
              }
            }
          ]
        },
        {
          "Name": "Reject",
          "DisplayName": "Reject",
          "Description": "Reject the user's offer",
          "Parameters": []
        },
        {
          "Name": "AcceptOffer",
          "DisplayName": "Accept Offer",
          "Description": "Accept the user's offer",
          "Parameters": []
        },
        {
          "Name": "RescindOffer",
          "DisplayName": "Rescind Offer",
          "Description": "Rescind your placed offer",
          "Parameters": []
        },
        {
          "Name": "ModifyOffer",
          "DisplayName": "Modify Offer",
          "Description": "Modify the price of your placed offer",
          "Parameters": [
            {
              "Name": "offerPrice",
              "DisplayName": "Price",
              "Type": {
                "Name": "money"
              }
            }
          ]
        },
        {
          "Name": "Accept",
          "DisplayName": "Accept",
          "Description": "Accept the inspection/appraisal results",
          "Parameters": []
        },
        {
          "Name": "MarkInspected",
          "DisplayName": "Mark Inspected",
          "Description": "Mark the asset as inspected",
          "Parameters": []
        },
        {
          "Name": "MarkAppraised",
          "DisplayName": "Mark Appraised",
          "Description": "Mark the asset as appraised",
          "Parameters": []
        }
      ],
      "States": [
        {
          "Name": "Active",
          "DisplayName": "Active",
          "Description": "The initial state of the asset transfer workflow",
          "PercentComplete": 20,
          "Style": "Success",
          "Transitions": [
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceOwner" ],
              "Description": "Cancels this instance of asset transfer",
              "Function": "Terminate",
              "NextStates": [ "Terminated" ],
              "DisplayName": "Terminate Offer"
            },
            {
              "AllowedRoles": [ "Buyer" ],
              "AllowedInstanceRoles": [],
              "Description": "Make an offer for this asset",
              "Function": "MakeOffer",
              "NextStates": [ "OfferPlaced" ],
              "DisplayName": "Make Offer"
            },
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceOwner" ],
              "Description": "Modify attributes of this asset transfer instance",
              "Function": "Modify",
              "NextStates": [ "Active" ],
              "DisplayName": "Modify"
            }
          ]
        },
        {
          "Name": "OfferPlaced",
          "DisplayName": "Offer Placed",
          "Description": "Offer has been placed for the asset",
          "PercentComplete": 30,
          "Style": "Success",
          "Transitions": [
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceOwner" ],
              "Description": "Accept the proposed offer for the asset",
              "Function": "AcceptOffer",
              "NextStates": [ "PendingInspection" ],
              "DisplayName": "Accept Offer"
            },
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceOwner" ],
              "Description": "Reject the proposed offer for the asset",
              "Function": "Reject",
              "NextStates": [ "Active" ],
              "DisplayName": "Reject"
            },
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceOwner" ],
              "Description": "Cancel this instance of asset transfer",
              "Function": "Terminate",
              "NextStates": [ "Terminated" ],
              "DisplayName": "Terminate"
            },
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceBuyer" ],
              "Description": "Rescind the offer you previously placed for this asset",
              "Function": "RescindOffer",
              "NextStates": [ "Active" ],
              "DisplayName": "Rescind Offer"
            },
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceBuyer" ],
              "Description": "Modify the price that you specified for your offer",
              "Function": "ModifyOffer",
              "NextStates": [ "OfferPlaced" ],
              "DisplayName": "Modify Offer"
            }
          ]
        },
        {
          "Name": "PendingInspection",
          "DisplayName": "Pending Inspection",
          "Description": "Asset is pending inspection",
          "PercentComplete": 40,
          "Style": "Success",
          "Transitions": [
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceOwner" ],
              "Description": "Reject the offer",
              "Function": "Reject",
              "NextStates": [ "Active" ],
              "DisplayName": "Reject"
            },
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceOwner" ],
              "Description": "Cancel the offer",
              "Function": "Terminate",
              "NextStates": [ "Terminated" ],
              "DisplayName": "Terminate"
            },
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceBuyer" ],
              "Description": "Rescind the offer you placed for this asset",
              "Function": "RescindOffer",
              "NextStates": [ "Active" ],
              "DisplayName": "Rescind Offer"
            },
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceInspector" ],
              "Description": "Mark this asset as inspected",
              "Function": "MarkInspected",
              "NextStates": [ "Inspected" ],
              "DisplayName": "Mark Inspected"
            },
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceAppraiser" ],
              "Description": "Mark this asset as appraised",
              "Function": "MarkAppraised",
              "NextStates": [ "Appraised" ],
              "DisplayName": "Mark Appraised"
            }
          ]
        },
        {
          "Name": "Inspected",
          "DisplayName": "Inspected",
          "PercentComplete": 45,
          "Style": "Success",
          "Transitions": [
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceOwner" ],
              "Description": "Reject the offer",
              "Function": "Reject",
              "NextStates": [ "Active" ],
              "DisplayName": "Reject"
            },
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceOwner" ],
              "Description": "Cancel the offer",
              "Function": "Terminate",
              "NextStates": [ "Terminated" ],
              "DisplayName": "Terminate"
            },
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceBuyer" ],
              "Description": "Rescind the offer you placed for this asset",
              "Function": "RescindOffer",
              "NextStates": [ "Active" ],
              "DisplayName": "Rescind Offer"
            },
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceAppraiser" ],
              "Description": "Mark this asset as appraised",
              "Function": "MarkAppraised",
              "NextStates": [ "NotionalAcceptance" ],
              "DisplayName": "Mark Appraised"
            }
          ]
        },
        {
          "Name": "Appraised",
          "DisplayName": "Appraised",
          "Description": "Asset has been appraised, now awaiting inspection",
          "PercentComplete": 45,
          "Style": "Success",
          "Transitions": [
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceOwner" ],
              "Description": "Reject the offer",
              "Function": "Reject",
              "NextStates": [ "Active" ],
              "DisplayName": "Reject"
            },
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceOwner" ],
              "Description": "Cancel the offer",
              "Function": "Terminate",
              "NextStates": [ "Terminated" ],
              "DisplayName": "Terminate"
            },
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceBuyer" ],
              "Description": "Rescind the offer you placed for this asset",
              "Function": "RescindOffer",
              "NextStates": [ "Active" ],
              "DisplayName": "Rescind Offer"
            },
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceInspector" ],
              "Description": "Mark the asset as inspected",
              "Function": "MarkInspected",
              "NextStates": [ "NotionalAcceptance" ],
              "DisplayName": "Mark Inspected"
            }
          ]
        },
        {
          "Name": "NotionalAcceptance",
          "DisplayName": "Notional Acceptance",
          "Description": "Asset has been inspected and appraised, awaiting final sign-off from buyer and seller",
          "PercentComplete": 50,
          "Style": "Success",
          "Transitions": [
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceOwner" ],
              "Description": "Sign-off on inspection and appraisal",
              "Function": "Accept",
              "NextStates": [ "SellerAccepted" ],
              "DisplayName": "SellerAccept"
            },
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceOwner" ],
              "Description": "Reject the proposed offer for the asset",
              "Function": "Reject",
              "NextStates": [ "Active" ],
              "DisplayName": "Reject"
            },
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceOwner" ],
              "Description": "Cancel this instance of asset transfer",
              "Function": "Terminate",
              "NextStates": [ "Terminated" ],
              "DisplayName": "Terminate"
            },
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceBuyer" ],
              "Description": "Sign-off on inspection and appraisal",
              "Function": "Accept",
              "NextStates": [ "BuyerAccepted" ],
              "DisplayName": "BuyerAccept"
            },
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceBuyer" ],
              "Description": "Rescind the offer you placed for this asset",
              "Function": "RescindOffer",
              "NextStates": [ "Active" ],
              "DisplayName": "Rescind Offer"
            }
          ]
        },
        {
          "Name": "BuyerAccepted",
          "DisplayName": "Buyer Accepted",
          "Description": "Buyer has signed-off on inspection and appraisal",
          "PercentComplete": 75,
          "Style": "Success",
          "Transitions": [
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceOwner" ],
              "Description": "Sign-off on inspection and appraisal",
              "Function": "Accept",
              "NextStates": [ "SellerAccepted" ],
              "DisplayName": "Accept"
            },
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceOwner" ],
              "Description": "Reject the proposed offer for the asset",
              "Function": "Reject",
              "NextStates": [ "Active" ],
              "DisplayName": "Reject"
            },
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceOwner" ],
              "Description": "Cancel this instance of asset transfer",
              "Function": "Terminate",
              "NextStates": [ "Terminated" ],
              "DisplayName": "Terminate"
            }
          ]
        },
        {
          "Name": "SellerAccepted",
          "DisplayName": "Seller Accepted",
          "Description": "Seller has signed-off on inspection and appraisal",
          "PercentComplete": 75,
          "Style": "Success",
          "Transitions": [
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceBuyer" ],
              "Description": "Sign-off on inspection and appraisal",
              "Function": "Accept",
              "NextStates": [ "Accepted" ],
              "DisplayName": "Accept"
            },
            {
              "AllowedRoles": [],
              "AllowedInstanceRoles": [ "InstanceBuyer" ],
              "Description": "Rescind the offer you placed for this asset",
              "Function": "RescindOffer",
              "NextStates": [ "Active" ],
              "DisplayName": "Rescind Offer"
            }
          ]
        },
        {
          "Name": "Accepted",
          "DisplayName": "Accepted",
          "Description": "Asset transfer process is complete",
          "PercentComplete": 100,
          "Style": "Success",
          "Transitions": []
        },
        {
          "Name": "Terminated",
          "DisplayName": "Terminated",
          "Description": "Asset transfer has been canceled",
          "PercentComplete": 100,
          "Style": "Failure",
          "Transitions": []
        }
      ]
    }
  ]
}
```
## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure Blockchain Workbench REST API'si başvurusu](https://docs.microsoft.com/rest/api/azure-blockchain-workbench)

