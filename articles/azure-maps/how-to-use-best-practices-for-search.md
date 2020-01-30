---
title: Azure haritalar 'ı kullanarak verimli bir şekilde arama Arama Hizmeti | Microsoft Azure haritaları
description: Microsoft Azure haritaları kullanarak arama hizmeti için en iyi yöntemleri uygulamayı öğrenin
author: walsehgal
ms.author: v-musehg
ms.date: 01/23/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: 68c7408f13027ded7beaabf46fb663217a90c52b
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2020
ms.locfileid: "76845756"
---
# <a name="best-practices-to-use-azure-maps-search-service"></a>Azure haritalar 'ı kullanmak için en iyi uygulamalar Arama Hizmeti

Azure haritalar [Arama hizmeti](https://docs.microsoft.com/rest/api/maps/search) çeşitli özellikleri olan API 'leri içerir. Örneğin, adres araması API 'SI, belirli bir konum etrafında Ilgi noktası (POı) veya verileri aramak için kullanılır. Bu makalede, Azure haritalar arama hizmetlerinden verileri çağırırken uygulanacak ses uygulamaları gösterilir. Şunları öğrenirsiniz:

* İlgili eşleşmeleri döndürmek için sorgular oluşturun
* Arama sonuçlarını sınırla
* Çeşitli sonuç türleri arasındaki farkı öğrenin
* Adres arama yanıtı yapısını okuyun


## <a name="prerequisites"></a>Ön koşullar

Haritalar hizmeti API 'Lerine herhangi bir çağrı yapmak için bir Azure Maps hesabına ve bir anahtara ihtiyacınız vardır. Gerekirse, [Hesap oluşturma](quick-demo-map-app.md#create-an-account-with-azure-maps) ve [birincil anahtar edinme](quick-demo-map-app.md#get-the-primary-key-for-your-account)konusundaki yönergeleri izleyin. Azure haritalar 'da kimlik doğrulaması hakkında daha fazla bilgi için bkz. [Azure haritalar 'da kimlik doğrulamasını yönetme](./how-to-manage-authentication.md).

> [!Tip]
> Arama hizmetini sorgulamak için [Postman uygulamasını](https://www.getpostman.com/apps) kullanarak Rest çağrıları oluşturabilir veya tercih ettiğiniz HERHANGI bir API geliştirme ortamını kullanabilirsiniz.


## <a name="best-practices-for-geocoding-address-search"></a>Coğrafi kodlama için en iyi uygulamalar (adres arama)

Azure haritalar Arama Hizmeti kullanarak tam veya kısmi bir adres aradığınızda, API arama sorguınızdan anahtar sözcükleri okur ve adresin boylam ve enlem koordinatlarını döndürür. Bu işleme geokodlamaya denir. Bir ülkede coğrafi kod yeteneği, coğrafi veri kapsamına ve coğrafi kodlama hizmetinin coğrafi kodlama duyarlığına bağımlıdır.

Azure haritalar coğrafi kodlama özelliklerini ülkeye/bölgeye göre daha fazla bilgi edinmek için bkz. [coğrafi kodlama kapsamı](https://docs.microsoft.com/azure/azure-maps/geocoding-coverage) .

### <a name="limit-search-results"></a>Arama sonuçlarını sınırla

 Azure haritalar arama API 'SI, arama sonuçlarını uygun şekilde sınırlamanıza yardımcı olabilir. böylece kullanıcılarınıza ilgili verileri görüntüleyebilirsiniz.

   > [!Note]
   > Arama API 'lerinin tümünün desteklenme parametreleri aşağıda listelenmiştir

   **Coğrafi sapma arama sonuçları**

   Sonuçlarınızı kullanıcılarınızın ilgili alanına coğrafi olarak sıralamak için, olası en yüksek ayrıntılı konum girişini her zaman eklemeniz gerekir. Arama sonuçlarını kısıtlamak için aşağıdaki giriş türlerini eklemeyi göz önünde bulundurun:

   1. `countrySet` parametresini, örneğin "US, FR" olarak ayarlayın. Varsayılan arama davranışı, büyük olasılıkla gereksiz sonuçlar döndüren dünyanın tamamında arama yapmak için kullanılır. Sorgunuzun `countrySet` parametresi varsa, arama yanlış sonuçlar döndürebilir. Örneğin, **Bellevue** adlı bir şehri aramak, Fransa ve ABD 'de **Bellevue** adlı ŞEHIRLERIN olduğu için ABD ve Fransa 'dan sonuçlar döndürür.

   2. Aramayı haritadaki belirli bir alanla kısıtlamak için, `btmRight` ve `topleft` parametrelerini sınırlayıcı kutusunu ayarlamak için kullanabilirsiniz.

   3. Sonuçlara ilişkin ilgi alanını etkilemek için, `lat`ve `lon` koordinat parametrelerini tanımlayabilir ve `radius` parametresini kullanarak arama alanının yarıçapını ayarlayabilirsiniz.


   **Benzer arama parametreleri**
   
  Azure Maps [,](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy) Kullanıcı girişlerinizin bir arama sorgusu için ne olduğunu bilmiyorsanız kullanılması önerilen hizmettir. API, Ilgi noktası (POı) arama ve coğrafi kodlama 'yı kurallı *tek satırlı bir aramada*birleştirir.

   1. `minFuzzyLevel` ve `maxFuzzyLevel` yardım, sorgu parametreleri tam olarak istenen bilgilerle eşleşmediği zaman bile ilgili eşleşmeleri döndürür. Performans kazanmak ve olağandışı sonuçları azaltmak için, varsayılan arama sorguları `minFuzzyLevel=1` ve `maxFuzzyLevel=2`. "Restrant" arama terimine bir örnek alın, `maxFuzzyLevel` 2 olarak ayarlandığında "Restoran" ile eşleştirilir. Varsayılan benzer düzeyler gerektiğinde geçersiz kılınabilir.  

   2. Ayrıca, `idxSet` parametresi kullanılarak döndürülecek sonuç türlerinin tam kümesini önceliklendirde atayabilirsiniz. Bu amaçla, virgülle ayrılmış dizinlerin bir listesini gönderebilirsiniz; öğe sırası önemi yoktur. Aşağıdaki dizinler desteklenir:

       * `Addr` - **adres aralıkları**: bazı Streets için, cadde başı ve sonundan sonra gelen adres noktaları vardır. Bu noktaların adres aralıkları olarak temsil edilir.
       * `Geo` - **coğrafi**bölgeler: bir haritanın yönetim bölümünü temsil eden, yani ülke, eyalet, şehir.
       * `PAD` - **noktası adresi**: bir dizin içinde sokak adı ve numarası olan belirli bir adresin bulunabileceği bir haritada nokta, örneğin, Soquel Dr 2501. Bu ıdxset değeri, adresler için kullanılabilen en yüksek doğruluk düzeyidir.  
       * `POI` - **Ilgi noktaları**: bir haritanın dikkat çekici olduğunu ve ilginç olabileceğini gösterir.  [Arama adresini al](https://docs.microsoft.com/rest/api/maps/search/getsearchaddress) , POI döndürmez.  
       * `Str` - **Streets**: haritadaki Streets temsili.
       * `XStr` - **çapraz Streets/kesişmeler: Juntler**temsili; İki cadde kesiştiğinde yer koyar.


       **Kullanım örnekleri**:

       * ıdxset = POı (yalnızca Ilgilendiğiniz arama noktaları) 

       * ıdxset = PAD, addr (yalnızca arama adresleri, PAD = Point adresi, addr = adres aralığı)

### <a name="reverse-geocode-and-geography-entity-type-filter"></a>Coğrafi kod ve Coğrafya varlık türü filtresini ters çevir

[Arama adresi ters API 'si](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse)ile ters coğrafi kod araması yaparken hizmetin, yönetim alanlarının çokgenler döndürme özelliği vardır. İstekte parametre `entityType` sağlayarak, belirtilen Coğrafya varlık türleri için aramayı daraltabilirsiniz. Elde edilen yanıt, Coğrafya KIMLIĞI ve eşleşen varlık türü içerir. Birden fazla varlık sağlarsanız, uç nokta **kullanılabilir en küçük varlığı**döndürür. Döndürülen geometri KIMLIĞI, bu coğrafi bölge 'nin geometrisini [Al (çokgen) hizmeti](https://docs.microsoft.com/rest/api/maps/search/getsearchpolygon)aracılığıyla almak için kullanılabilir.

**Örnek istek:**

```HTTP
https://atlas.microsoft.com/search/address/reverse/json?api-version=1.0&subscription-key={subscription-key}&query=47.6394532,-122.1304551&language=en-US&entityType=Municipality
```

**Yanıtıyla**

```JSON
{
    "summary": {
        "queryTime": 14,
        "numResults": 1
    },
    "addresses": [
        {
            "address": {
                "routeNumbers": [],
                "countryCode": "US",
                "countrySubdivision": "WA",
                "countrySecondarySubdivision": "King",
                "countryTertiarySubdivision": "Seattle East",
                "municipality": "Redmond",
                "country": "United States",
                "countryCodeISO3": "USA",
                },
                "countrySubdivisionName": "Washington"
            },
            "position": "47.639454,-122.130455",
            "dataSources": {
                "geometry": {
                    "id": "00005557-4100-3c00-0000-0000596ae571"
                }
            },
            "entityType": "Municipality"
        }
    ]
}
```

### <a name="search-results-language"></a>Arama sonuçları dili

`language` parametresi, API tarafından döndürülen sonuçların dilini seçmenizi sağlar. İstek içinde dil ayarlanmamışsa, arama hizmeti ülke/bölgedeki en yaygın dili otomatik olarak varsayılan olarak alır. Ayrıca, belirtilen dildeki veriler kullanılabilir olmadığında, varsayılan dil kullanılır. Azure haritalar hizmetlerinden ülkeye/bölgeye göre desteklenen dillerin listesi için [desteklenen diller](https://docs.microsoft.com/azure/azure-maps/supported-languages) bölümüne bakın.


### <a name="predictive-mode-autosuggest"></a>Tahmine dayalı mod (otomatik öneri)

Kısmi sorgularla ilgili daha fazla eşleşme bulmak için `typeahead` parametresi ' true ' olarak ayarlanmalıdır. Sorgu kısmi giriş olarak yorumlanır ve arama tahmine dayalı moda girer. Aksi takdirde hizmet ilgili tüm bilgilerin geçtiğini varsayacaktır.

Aşağıdaki örnek sorguda, arama adresi hizmeti 'nin "mikro so" için `typeahead` parametresi **true**olarak ayarlanmış şekilde sorgulandığını görebilirsiniz. Yanıtı gözlemlerseniz, arama hizmetinin sorguyu kısmi sorgu olarak yorumdığını görebilirsiniz. Yanıt, oto önerilmiş sorguya ilişkin sonuçları içerir.

**Örnek sorgu:**

```HTTP
https://atlas.microsoft.com/search/address/json?subscription-key={subscription-key}&api-version=1.0&typeahead=true&countrySet=US&lat=47.6370891183&lon=-122.123736172&query=Microsoft
```

**Yanıtıyla**

```JSON
{
    "summary": {
        "query": "microsoft",
        "queryType": "NON_NEAR",
        "queryTime": 18,
        "numResults": 7,
        "offset": 0,
        "totalResults": 7,
        "fuzzyLevel": 1,
        "geoBias": {
            "lat": 47.6370891183,
            "lon": -122.123736172
        }
    },
    "results": [
        {
            "type": "Street",
            "id": "US/STR/p0/9438784",
            "score": 2.594099998474121,
            "dist": 314.0590106663596,
            "address": {
                "streetName": "Microsoft Way",
                "municipalitySubdivision": "Redmond",
                "municipality": "Redmond",
            },
            "position": {
                "lat": 47.63988,
                "lon": -122.12438
            },
            "viewport": {
                "topLeftPoint": {
                    "lat": 47.64223,
                    "lon": -122.1256,
                    "valid": true
                },
                "btmRightPoint": {
                    "lat": 47.63748,
                    "lon": -122.12309,
                    "valid": true
                }
            }
        },
        {
            "type": "Street",
            "id": "US/STR/p0/1756074",
            "score": 2.592679977416992,
            "dist": 876.0272035824189,
            "address": {
                "streetName": "Microsoft Road",
                "municipalitySubdivision": "Redmond",
                "municipality": "Redmond",
                "countrySecondarySubdivision": "King",
                "countryTertiarySubdivision": "Seattle East",
                "countrySubdivision": "WA",
                "countrySubdivisionName": "Washington",
                "postalCode": "98052",
                "countryCode": "US",
                "country": "United States",
                "countryCodeISO3": "USA",
                "freeformAddress": "Microsoft Road, Redmond, WA 98052"
            },
            "position": {
                "lat": 47.64032,
                "lon": -122.1344
            },
            "viewport": {
                "topLeftPoint": {
                    "lat": 47.64253,
                    "lon": -122.13535,
                    "valid": true
                },
                "btmRightPoint": {
                    "lat": 47.63816,
                    "lon": -122.13305,
                    "valid": true
                }
            }
        },
        {
            "type": "Street",
            "id": "US/STR/p0/1470668",
            "score": 2.5290400981903076,
            "dist": 2735.4883918101486,
            "address": {
                "streetName": "Microsoft West Campus Road",
                "municipalitySubdivision": "Redmond",
                "municipality": "Bellevue",
                "countrySecondarySubdivision": "King",
                "countryTertiarySubdivision": "Seattle East",
                "countrySubdivision": "WA",
                "countrySubdivisionName": "Washington",
                "postalCode": "98007",
                "countryCode": "US",
                "country": "United States",
                "countryCodeISO3": "USA",
                "freeformAddress": "Microsoft West Campus Road, Bellevue, WA 98007"
            },
            "position": {
                "lat": 47.65784,
                "lon": -122.14335
            },
            "viewport": {
                "topLeftPoint": {
                    "lat": 47.65785,
                    "lon": -122.14335,
                    "valid": true
                },
                "btmRightPoint": {
                    "lat": 47.65784,
                    "lon": -122.14325,
                    "valid": true
                }
            }
        },
        {
            "type": "Street",
            "id": "US/STR/p0/12812615",
            "score": 2.527509927749634,
            "dist": 2870.9579016916873,
            "address": {
                "streetName": "Microsoft West Campus Road",
                "municipalitySubdivision": "Redmond",
                "municipality": "Redmond",
                "countrySecondarySubdivision": "King",
                "countryTertiarySubdivision": "Seattle East",
                "countrySubdivision": "WA",
                "countrySubdivisionName": "Washington",
                "postalCode": "98052",
                "countryCode": "US",
                "country": "United States",
                "countryCodeISO3": "USA",
                "freeformAddress": "Microsoft West Campus Road, Redmond, WA 98052"
            },
            "position": {
                "lat": 47.66034,
                "lon": -122.1404
            },
            "viewport": {
                "topLeftPoint": {
                    "lat": 47.66039,
                    "lon": -122.14325,
                    "valid": true
                },
                "btmRightPoint": {
                    "lat": 47.65778,
                    "lon": -122.13749,
                    "valid": true
                }
            }
        },
        {
            "type": "Street",
            "id": "US/STR/p0/197588",
            "score": 2.4630401134490967,
            "dist": 878.1404663812472,
            "address": {
                "streetName": "157th Avenue Northeast",
                "municipalitySubdivision": "Redmond",
                "municipality": "Redmond",
                "countrySecondarySubdivision": "King",
                "countryTertiarySubdivision": "Seattle East",
                "countrySubdivision": "WA",
                "countrySubdivisionName": "Washington",
                "postalCode": "98052",
                "extendedPostalCode": "980525344, 980525398, 980525399",
                "countryCode": "US",
                "country": "United States",
                "countryCodeISO3": "USA",
                "freeformAddress": "157th Avenue Northeast, Redmond, WA 98052"
            },
            "position": {
                "lat": 47.64351,
                "lon": -122.13056
            },
            "viewport": {
                "topLeftPoint": {
                    "lat": 47.64473,
                    "lon": -122.13058,
                    "valid": true
                },
                "btmRightPoint": {
                    "lat": 47.6425,
                    "lon": -122.13016,
                    "valid": true
                }
            }
        },
        {
            "type": "Street",
            "id": "US/STR/p0/3033991",
            "score": 2.0754499435424805,
            "dist": 3655467.8844475765,
            "address": {
                "streetName": "Microsoft Way",
                "municipalitySubdivision": "Yorkmount, Charlotte",
            },
            "position": {
                "lat": 35.14267,
                "lon": -80.91824
            },
            "viewport": {
                "topLeftPoint": {
                    "lat": 35.14287,
                    "lon": -80.91839,
                    "valid": true
                },
                "btmRightPoint": {
                    "lat": 35.14267,
                    "lon": -80.91814,
                    "valid": true
                }
            }
        },
        {
            "type": "Street",
            "id": "US/STR/p0/8395877",
            "score": 2.0754499435424805,
            "dist": 3655437.0037482483,
            "address": {
                "streetName": "Microsoft Way",
                "municipalitySubdivision": "Charlotte",
                "municipality": "Charlotte",
                "countrySecondarySubdivision": "Mecklenburg",
                "countryTertiarySubdivision": "Township 1 Charlotte",
                "countrySubdivision": "NC",
                "countrySubdivisionName": "North Carolina",
                "postalCode": "28273",
                "extendedPostalCode": "282738105, 282738106, 282738108, 2827382, 282738200",
                "countryCode": "US",
                "country": "United States",
                "countryCodeISO3": "USA",
                "freeformAddress": "Microsoft Way, Charlotte, NC 28273"
            },
            "position": {
                "lat": 35.14134,
                "lon": -80.9198
            },
            "viewport": {
                "topLeftPoint": {
                    "lat": 35.14274,
                    "lon": -80.92159,
                    "valid": true
                },
                "btmRightPoint": {
                    "lat": 35.14002,
                    "lon": -80.91824,
                    "valid": true
                }
            }
        }
    ]
}
```


### <a name="uri-encoding-to-handle-special-characters"></a>Özel karakterleri işlemek için URI kodlaması 

Çapraz cadde adreslerini bulmak için, adresin özel karakterlerini işlemek üzere URI 'yi kodlamanız gerekir. Bu adresi göz önünde bulundurun: "1. Aven& UNION Street, Seattle". İstek gönderilmeden önce ' & ' özel karakterinin kodlanması gerekiyor. Tüm karakterlerin bir '% ' karakteri ve UTF-8 karakterlerine karşılık gelen iki karakterlik bir onaltılık değer kullanılarak kodlandığı bir URI 'de karakter verilerini kodlamayı öneririz.

**Kullanım örnekleri**:

Arama adresini al:

```
query=1st Avenue & E 111th St, New York
```

 şöyle kodlanır:

```
query=1st%20Avenue%20%26%20E%20111th%20St%2C%20New%20York
```


Farklı diller için kullanılacak farklı yöntemler şunlardır: 

JavaScript/TypeScript:
```Javascript
encodeURIComponent(query)
```

C#VB
```csharp
Uri.EscapeDataString(query)
```

Java:
```Java
URLEncoder.encode(query, "UTF-8") 
```

Python:
```Python
import urllib.parse 
urllib.parse.quote(query)
```

C++:
```C++
#include <curl/curl.h>
curl_easy_escape(query)
```

PHP:
```PHP
urlencode(query)
```

Söyleniş
```Ruby
CGI::escape(query) 
```

SWIFT
```Swift
query.stringByAddingPercentEncodingWithAllowedCharacters(.URLHostAllowedCharacterSet()) 
```

Sayfasına
```Go
import ("net/url") 
url.QueryEscape(query)
```


## <a name="best-practices-for-poi-search"></a>POı araması için en iyi uygulamalar

Ilgi noktaları (POı) arama, bir ada göre POı sonuçları talep etmenize olanak tanır. Örneğin, iş adına göre arama yapın. Varsayılan davranış tüm dünyayı aramak, büyük olasılıkla gereksiz sonuçları geri almak ve/veya daha uzun arama süreleriyle sonuçlandığı için, uygulamanızın kapsama ihtiyacı olan ülkeleri belirtmek üzere `countrySet` parametresini kullanmanızı önemle tavsiye ederiz.

### <a name="brand-search"></a>Marka araması

Sonuçların ve yanıttaki bilgilerin uygunluğunu artırmak için, Ilgi noktası (POı) arama yanıtı, yanıtı ayrıştırmak için kullanılabilecek marka bilgilerini içerir.

Ayrıca, istekteki marka adlarının virgülle ayrılmış bir listesini de gönderebilirsiniz. `brandSet` parametresini kullanarak sonuçları belirli markalara kısıtlamak için listeyi kullanabilirsiniz. Öğe sırası önemi yoktur. Birden çok marka sağlandığında, yalnızca belirtilen listelerden birine (en az) ait sonuçlar döndürülür.

Microsoft kampüs (Redmond, WA) yakınında gaz istasyonlara yönelik bir [POI kategorisi arama](https://docs.microsoft.com/rest/api/maps/search/getsearchpoicategory) isteği oluşturalım. Yanıtı gözlemlerseniz, dönen her bir POM için marka bilgilerini görebilirsiniz.


**Örnek sorgu:**

```HTTP
https://atlas.microsoft.com/search/poi/json?subscription-key={subscription-key}&api-version=1.0&query=gas%20station&limit=3&lat=47.6413362&lon=-122.1327968
```

**Yanıtıyla**

```JSON
{
    "summary": {
        "query": "gas station",
        "queryType": "NON_NEAR",
        "queryTime": 276,
        "numResults": 3,
        "offset": 0,
        "totalResults": 762680,
        "fuzzyLevel": 1,
        "geoBias": {
            "lat": 47.6413362,
            "lon": -122.1327968
        }
    },
    "results": [
        {
            "type": "POI",
            "id": "US/POI/p0/8831765",
            "score": 5.6631999015808105,
            "dist": 1037.0280221303253,
            "info": "search:ta:840531000004190-US",
            "poi": {
                "name": "Chevron",
                "phone": "+(1)-(425)-6532200",
                "brands": [
                    {
                        "name": "Chevron"
                    }
                ],
                "categorySet": [
                    {
                        "id": 7311
                    }
                ],
                "url": "www.chevron.com",
                "categories": [
                    "petrol station"
                ],
                "classifications": [
                    {
                        "code": "PETROL_STATION",
                        "names": [
                            {
                                "nameLocale": "en-US",
                                "name": "petrol station"
                            }
                        ]
                    }
                ]
            },
            "address": {
                "streetNumber": "2444",
                "streetName": "Bel Red Rd",
                "municipalitySubdivision": "Northeast Bellevue, Bellevue",
            },
            "position": {
                "lat": 47.63201,
                "lon": -122.13281
            },
            "viewport": {
                "topLeftPoint": {
                    "lat": 47.63291,
                    "lon": -122.13414,
                    "valid": true
                },
                "btmRightPoint": {
                    "lat": 47.63111,
                    "lon": -122.13148,
                    "valid": true
                }
            },
            "entryPoints": [
                {
                    "type": "main",
                    "position": {
                        "lat": 47.63222,
                        "lon": -122.13312,
                        "valid": true
                    }
                }
            ]
        },
        {
            "type": "POI",
            "id": "US/POI/p0/8831752",
            "score": 5.662710189819336,
            "dist": 1330.1278248163273,
            "info": "search:ta:840539001100326-US",
            "poi": {
                "name": "76",
                "phone": "+(1)-(425)-7472126",
                "brands": [
                    {
                        "name": "76"
                    }
                ],
                "categorySet": [
                    {
                        "id": 7311
                    }
                ],
                "url": "www.76.com",
                "categories": [
                    "petrol station"
                ],
                "classifications": [
                    {
                        "code": "PETROL_STATION",
                        "names": [
                            {
                                "nameLocale": "en-US",
                                "name": "petrol station"
                            }
                        ]
                    }
                ]
            },
            "address": {
                "streetNumber": "2421",
                "streetName": "148Th Ave Ne",
                "municipalitySubdivision": "Redmond, Bridle Trails, Bellevue",
                "municipality": "Redmond, Bellevue",
                "countrySecondarySubdivision": "King",
                "countryTertiarySubdivision": "Seattle East",
                "countrySubdivision": "WA",
                "countrySubdivisionName": "Washington",
                "postalCode": "98007",
                "countryCode": "US",
                "country": "United States",
                "countryCodeISO3": "USA",
                "freeformAddress": "2421 148Th Ave Ne, Bellevue, WA 98007",
                "localName": "Bellevue"
            },
            "position": {
                "lat": 47.63187,
                "lon": -122.14365
            },
            "viewport": {
                "topLeftPoint": {
                    "lat": 47.63277,
                    "lon": -122.14498,
                    "valid": true
                },
                "btmRightPoint": {
                    "lat": 47.63097,
                    "lon": -122.14232,
                    "valid": true
                }
            },
            "entryPoints": [
                {
                    "type": "minor",
                    "position": {
                        "lat": 47.63187,
                        "lon": -122.14374,
                        "valid": true
                    }
                },
                {
                    "type": "main",
                    "position": {
                        "lat": 47.63186,
                        "lon": -122.14313,
                        "valid": true
                    }
                }
            ]
        },
        {
            "type": "POI",
            "id": "US/POI/p0/8831764",
            "score": 5.662449836730957,
            "dist": 1458.645407416307,
            "info": "search:ta:840539000488527-US",
            "poi": {
                "name": "BROWN BEAR CAR WASH",
                "phone": "+(1)-(425)-6442868",
                "brands": [
                    {
                        "name": "Texaco"
                    }
                ],
                "categorySet": [
                    {
                        "id": 7311
                    }
                ],
                "url": "www.texaco.com/",
                "categories": [
                    "petrol station"
                ],
                "classifications": [
                    {
                        "code": "PETROL_STATION",
                        "names": [
                            {
                                "nameLocale": "en-US",
                                "name": "petrol station"
                            }
                        ]
                    }
                ]
            },
            "address": {
                "streetNumber": "15248",
                "streetName": "Bel Red Rd",
                "municipalitySubdivision": "Redmond",
            },
            "position": {
                "lat": 47.62843,
                "lon": -122.13628
            },
            "viewport": {
                "topLeftPoint": {
                    "lat": 47.62933,
                    "lon": -122.13761,
                    "valid": true
                },
                "btmRightPoint": {
                    "lat": 47.62753,
                    "lon": -122.13495,
                    "valid": true
                }
            },
            "entryPoints": [
                {
                    "type": "main",
                    "position": {
                        "lat": 47.62827,
                        "lon": -122.13628,
                        "valid": true
                    }
                }
            ]
        }
    ]
}
```


### <a name="airport-search"></a>Havaalanı araması

POı araması, resmi Havaalanı kodlarını kullanarak havaalanları aramayı destekler. Örneğin, **Sea** (Seattle-Tacoma Uluslararası Havaalanı). 

```HTTP
https://atlas.microsoft.com/search/poi/json?subscription-key={subscription-key}&api-version=1.0&query=SEA 
```

### <a name="nearby-search"></a>Yakındaki arama

Belirli bir konumun etrafında yalnızca POı sonuçlarını almak için, [yakın arama API 'si](https://docs.microsoft.com/rest/api/maps/search/getsearchnearby) doğru seçim olabilir. Bu uç nokta yalnızca POI sonuçlarını döndürür ve bir arama sorgu parametresi almaz. Sonuçları sınırlandırmak için yarıçap ayarlanması önerilir.

## <a name="understanding-the-responses"></a>Yanıtları anlama

Seattle 'daki bir adres için Azure haritalar [Arama hizmeti](https://docs.microsoft.com/rest/api/maps/search) 'Ne bir adres arama isteği oluşturalım. Aşağıdaki istek URL 'sini dikkatle görüyorsanız, `countrySet` parametresini, Amerika Birleşik Devletler, adresi aramak için **bizimle** ayarlayacağız.

**Örnek sorgu:**

```HTTP
https://atlas.microsoft.com/search/address/json?subscription-key={subscription-key}&api-version=1&query=400%20Broad%20Street%2C%20Seattle%2C%20WA&countrySet=US
```

Aşağıda yanıt yapısına göz atalım. Yanıttaki sonuç nesnelerinin sonuç türleri farklıdır. Dikkatlice gözlemlerseniz, "nokta adresi", "cadde" ve "çapraz cadde" olan üç farklı sonuç nesnesi türü olduğunu görebilirsiniz. Adres araması 'nın POI döndürmediğine dikkat edin. Her yanıt nesnesi için `Score` parametresi, aynı yanıttaki diğer nesnelerin puanlarını gösteren göreli eşleşen puanı gösterir. Yanıt nesnesi parametreleri hakkında daha fazla bilgi edinmek için bkz. [arama adresini al](https://docs.microsoft.com/rest/api/maps/search/getsearchaddress) .

**Desteklenen sonuç türleri:**

* **Nokta adresi:** Cadde adı ve numarası olan belirli bir adrese sahip bir haritada işaret eder. Adresler için kullanılabilir en yüksek doğruluk düzeyi. 

* **Adres Aralığı:**  Bazı Streets için, sokak başı ve sonundan alınmış adres noktaları vardır; Bu noktaların adres aralıkları olarak temsil edilir. 

* **Coğrafya:** Bir kapıdaki yönetim bölümünü temsil eden, yani ülke, eyalet, şehir. 

* **Poi-(Ilgi noktaları):** Bir haritanın dikkatini çekecek ve ilgi çekici olabilecek bir haritada işaret ediyor.

* **Cadde:** Haritadaki cadde temsili. Adresler, adresi içeren sokak 'nin Enlem/Boylam koordinatı olarak çözümlenir. Evin numarası işlenemiyor olabilir. 

* **Çapraz cadde:** Kesişimlerini. Junler; İki cadde kesiştiğinde yer koyar.

**Yanıtıyla**

```JSON
{
    "summary": {
        "query": "400 broad street seattle wa",
        "queryType": "NON_NEAR",
        "queryTime": 146,
        "numResults": 6,
        "offset": 0,
        "totalResults": 7,
        "fuzzyLevel": 1
    },
    "results": [
        {
            "type": "Point Address",
            "id": "US/PAD/p0/28725082",
            "score": 9.893799781799316,
            "address": {
                "streetNumber": "400",
                "streetName": "Broad Street",
            },
            "position": {
                "lat": 47.62039,
                "lon": -122.34928
            },
            "viewport": {
                "topLeftPoint": {
                    "lat": 47.62129,
                    "lon": -122.35061,
                    "valid": true
                },
                "btmRightPoint": {
                    "lat": 47.61949,
                    "lon": -122.34795,
                    "valid": true
                }
            },
            "entryPoints": [
                {
                    "type": "main",
                    "position": {
                        "lat": 47.61982,
                        "lon": -122.34886,
                        "valid": true
                    }
                }
            ]
        },
        {
            "type": "Street",
            "id": "US/STR/p0/6700384",
            "score": 8.129190444946289,
            "address": {
                "streetName": "Broad Street",
            },
            "position": {
                "lat": 47.61724,
                "lon": -122.35207
            },
            "viewport": {
                "topLeftPoint": {
                    "lat": 47.61825,
                    "lon": -122.35336,
                    "valid": true
                },
                "btmRightPoint": {
                    "lat": 47.61626,
                    "lon": -122.35078,
                    "valid": true
                }
            }
        },
        {
            "type": "Street",
            "id": "US/STR/p0/9701953",
            "score": 8.129190444946289,
            "address": {
                "streetName": "Broad Street",
            },
            "position": {
                "lat": 47.61965,
                "lon": -122.349
            },
            "viewport": {
                "topLeftPoint": {
                    "lat": 47.62066,
                    "lon": -122.35041,
                    "valid": true
                },
                "btmRightPoint": {
                    "lat": 47.61857,
                    "lon": -122.34761,
                    "valid": true
                }
            }
        },
        {
            "type": "Street",
            "id": "US/STR/p0/11721297",
            "score": 8.129190444946289,
            "address": {
                "streetName": "Broad Street",
                "municipalitySubdivision": "Seattle, Downtown Seattle, Denny Regrade, Belltown",
                "municipality": "Seattle",
                "countrySecondarySubdivision": "King",
                "countryTertiarySubdivision": "Seattle",
                "countrySubdivision": "WA",
                "countrySubdivisionName": "Washington",
                "postalCode": "98121",
                "extendedPostalCode": "981211237",
                "countryCode": "US",
                "country": "United States",
                "countryCodeISO3": "USA",
                "freeformAddress": "Broad Street, Seattle, WA 98121"
            },
            "position": {
                "lat": 47.61825,
                "lon": -122.35078
            },
            "viewport": {
                "topLeftPoint": {
                    "lat": 47.61857,
                    "lon": -122.35078,
                    "valid": true
                },
                "btmRightPoint": {
                    "lat": 47.61825,
                    "lon": -122.35041,
                    "valid": true
                }
            }
        },
        {
            "type": "Cross Street",
            "id": "US/XSTR/p1/232144",
            "score": 6.754479885101318,
            "address": {
                "streetName": "Broad Street & Valley Street",
                "municipalitySubdivision": "South Lake Union, Seattle",
            },
            "position": {
                "lat": 47.62545,
                "lon": -122.33974
            },
            "viewport": {
                "topLeftPoint": {
                    "lat": 47.62635,
                    "lon": -122.34107,
                    "valid": true
                },
                "btmRightPoint": {
                    "lat": 47.62455,
                    "lon": -122.33841,
                    "valid": true
                }
            }
        }
    ]
}
```

### <a name="geometry"></a>Geometrisi

Yanıt türü **geometri**olduğunda, "Geometry" ve "ID" altındaki **DataSources** nesnesinde döndürülen geometri kimliğini içerebilir. Örneğin, [Çokgen hizmeti al](https://docs.microsoft.com/rest/api/maps/search/getsearchpolygon) , GeoJSON biçiminde geometri verileri isteyebilmeniz için izin verir. Bir varlık kümesi için şehir veya Havaalanı ana hattı gibi. Bu sınır verilerini [, geometri Içinde](https://docs.microsoft.com/rest/api/maps/search/postsearchinsidegeometry) [bölge](https://docs.microsoft.com/azure/azure-maps/tutorial-geofence) sınırlaması veya arama POI için kullanabilirsiniz.


[Arama adresi](https://docs.microsoft.com/rest/api/maps/search/getsearchaddress) veya [arama benzer](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy) API yanıtları, "geometri" ve "kimlik" altındaki veri kaynakları nesnesinde döndürülen **geometri kimliğini** içerebilir.


```JSON 
"dataSources": { 
        "geometry": { 
            "id": "00005557-4100-3c00-0000-000059690938" // The geometry ID is returned in the dataSources object under "geometry" and "id".
        }
} 
```

## <a name="next-steps"></a>Sonraki adımlar

* [Azure haritalar arama hizmeti istekleri oluşturmayı](https://docs.microsoft.com/azure/azure-maps/how-to-search-for-address)öğrenin.
* Azure haritalar [Arama hizmeti API 'si belgelerini](https://docs.microsoft.com/rest/api/maps/search)inceleyin. 
