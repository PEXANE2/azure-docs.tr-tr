---
title: OPC Publisher'ı Yapılandırma - Azure | Microsoft Dokümanlar
description: Bu makalede, OPC Publisher'ın OPC UA düğümü veri değişikliklerini, yayımlanacak OPC UA olaylarını ve ayrıca telemetri biçimini belirtecek şekilde nasıl yapılandırılabildiğini açıklanmaktadır.
author: dominicbetts
ms.author: dobett
ms.date: 06/10/2019
ms.topic: conceptual
ms.service: industrial-iot
services: iot-industrialiot
manager: philmea
ms.openlocfilehash: 0ebbf0d41c05f71c571d9665903ba4ba44f71bd0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77198812"
---
# <a name="configure-opc-publisher"></a>OPC Yayımcısını Yapılandırma

OPC Publisher'ı aşağıdakileri belirtecek şekilde yapılandırabilirsiniz:

- OPC UA düğümü veri yayımlamak için değişiklikler.
- OPC UA olayları yayımlamak için.
- Telemetri formatı.

Yapılandırma dosyalarını veya yöntem çağrılarını kullanarak OPC Publisher'ı yapılandırabilirsiniz.

## <a name="use-configuration-files"></a>Yapılandırma dosyalarını kullanma

Bu bölümde, OPC UA düğümü yayımlamayı yapılandırma dosyalarıyla yapılandırma seçenekleri açıklanmaktadır.

### <a name="use-a-configuration-file-to-configure-publishing-data-changes"></a>Yayımlama veri değişikliklerini yapılandırmak için yapılandırma dosyası kullanma

OPC UA düğümlerini yayımlanacak şekilde yapılandırmanın en kolay yolu yapılandırma dosyasıdır. Yapılandırma dosyası biçimi depodaki [publishednodes.json'da](https://github.com/Azure/iot-edge-opc-publisher/blob/master/opcpublisher/publishednodes.json) belgelenmiştir.

Yapılandırma dosyası sözdizimi zaman içinde değişti. OPC Publisher hala eski biçimleri okur, ancak yapılandırmayı sürdürdüğünde bunları en son biçime dönüştürür.

Aşağıdaki örnek, yapılandırma dosyasının biçimini gösterir:

```json
[
  {
    "EndpointUrl": "opc.tcp://testserver:62541/Quickstarts/ReferenceServer",
    "UseSecurity": true,
    "OpcNodes": [
      {
        "Id": "i=2258",
        "OpcSamplingInterval": 2000,
        "OpcPublishingInterval": 5000,
        "DisplayName": "Current time"
      }
    ]
  }
]
```

### <a name="use-a-configuration-file-to-configure-publishing-events"></a>Yayımlama olaylarını yapılandırmak için yapılandırma dosyası kullanma

OPC UA olaylarını yayımlamak için, veri değişiklikleriyle aynı yapılandırma dosyasını kullanırsınız.

Aşağıdaki örnek, [SimpleEvents sunucusu](https://github.com/OPCFoundation/UA-.NETStandard/tree/master/SampleApplications/Workshop/SimpleEvents/Server)tarafından oluşturulan olaylar için yayımlamanın nasıl yapılandırılabildiğini gösterir. SimpleEvents sunucusu [OPC Vakfı deposunda](https://github.com/OPCFoundation/UA-.NETStandard) bulunabilir:

```json
[
  {
    "EndpointUrl": "opc.tcp://testserver:62563/Quickstarts/SimpleEventsServer",
    "OpcEvents": [
      {
        "Id": "i=2253",
        "DisplayName": "SimpleEventServerEvents",
        "SelectClauses": [
          {
            "TypeId": "i=2041",
            "BrowsePaths": [
              "EventId"
            ]
          },
          {
            "TypeId": "i=2041",
            "BrowsePaths": [
              "Message"
            ]
          },
          {
            "TypeId": "nsu=http://opcfoundation.org/Quickstarts/SimpleEvents;i=235",
            "BrowsePaths": [
              "/2:CycleId"
            ]
          },
          {
            "TypeId": "nsu=http://opcfoundation.org/Quickstarts/SimpleEvents;i=235",
            "BrowsePaths": [
              "/2:CurrentStep"
            ]
          }
        ],
        "WhereClause": [
          {
            "Operator": "OfType",
            "Operands": [
              {
                "Literal": "ns=2;i=235"
              }
            ]
          }
        ]
      }
    ]
  }
]
```

## <a name="use-method-calls"></a>Yöntem çağrılarını kullanma

Bu bölümde, OPC Publisher'ı yapılandırmak için kullanabileceğiniz yöntem çağrıları açıklanmaktadır.

### <a name="configure-using-opc-ua-method-calls"></a>OPC UA yöntemi çağrılarını kullanarak yapılandırma

OPC Publisher, 62222 no'lu bağlantı noktasından erişilebilen bir OPC UA Sunucusu içerir. Ana bilgisayar adı **yayımcıise,** bitiş noktası `opc.tcp://publisher:62222/UA/Publisher`URI şudur: .

Bu bitiş noktası aşağıdaki dört yöntemi ortaya çıkarır:

- PublishNode
- YayımsızunYeni
- GetPublishedNodes
- IoT HubDirectMethod

### <a name="configure-using-iot-hub-direct-method-calls"></a>IoT Hub doğrudan yöntem çağrılarını kullanarak yapılandırın

OPC Publisher aşağıdaki IoT Hub doğrudan yöntem çağrılarını uygular:

- YayınDüğümleri
- YayımsızunYeniler
- YayımsızunTümDüğümleri
- GetConfiguredEndpoints
- GetConfiguredNodesOnEndpoint
- GetDiagnosticInfo
- GetDiagnosticLog
- GetDiagnosticStartupLog
- Çıkış Uygulaması
- GetInfo

Yöntem isteği ve yanıtlarının JSON yükünün biçimi [opcpublisher/HubMethodModel.cs'de](https://github.com/Azure/iot-edge-opc-publisher/blob/master/opcpublisher/HubMethodModel.cs)tanımlanır.

Modülde bilinmeyen bir yöntem çağırırsanız, yöntemin uygulanmadığını belirten bir dizeyle yanıt verir. Modülü pinglemek için bilinmeyen bir yöntemi arayabilirsiniz.

### <a name="configure-username-and-password-for-authentication"></a>Kimlik doğrulama için kullanıcı adı ve parolayı yapılandırma

Kimlik doğrulama modu, IoT Hub doğrudan yöntem çağrıları aracılığıyla ayarlanabilir. Taşıma, **OpcAuthenticationMode** özelliğini ve kullanıcı adı ve parolayı içermelidir:

```csharp
{
    "EndpointUrl": "<Url of the endpoint to set authentication settings>",
    "OpcAuthenticationMode": "UsernamePassword",
    "Username": "<Username>",
    "Password": "<Password>"
    ...
}
```

Parola, IoT Hub İş Yükü İstemi tarafından şifrelenir ve yayımcının yapılandırmasında depolanır. Kimlik doğrulamayı anonim olarak değiştirmek için aşağıdaki taşıma kapasitesine sahip yöntemi kullanın:

```csharp
{
    "EndpointUrl": "<Url of the endpoint to set authentication settings>",
    "OpcAuthenticationMode": "Anonymous"
    ...
}
```

**OpcAuthenticationMode** özelliği yükte ayarlanmamışsa, kimlik doğrulama ayarları yapılandırmada değişmeden kalır.

## <a name="configure-telemetry-publishing"></a>Telemetri yayımlamayı yapılandırma

OPC Publisher, yayımlanmış bir düğümdeki değer değişikliği bildirimini aldığında, IoT Hub'a gönderilen JSON biçimli bir ileti oluşturur.

Bu JSON biçimlendirilmiş iletinin içeriğini yapılandırma dosyasını kullanarak yapılandırabilirsiniz. Seçenekte yapılandırma dosyası belirtilmemişse, [Bağlı fabrika çözüm hızlandırıcısıyla](https://github.com/Azure/azure-iot-connected-factory)uyumlu varsayılan bir yapılandırma kullanılır. `--tc`

OPC Publisher toplu iletilere göre yapılandırılırsa, bunlar geçerli bir JSON dizisi olarak gönderilir.

Telemetri aşağıdaki kaynaklardan türetilmiştir:

- Düğüm için OPC Publisher düğümü yapılandırması
- OPC Publisher'ın bildirim aldığı OPC UA yığınının **MonitoredItem** nesnesi.
- Veri değeri değişikliği yle ilgili ayrıntıları sağlayan bu bildirime geçirilen bağımsız değişken.

JSON biçimlendirilmiş iletisine konulan telemetri, bu nesnelerin önemli özelliklerinden bir seçkidir. Daha fazla özelliğe ihtiyacınız varsa, OPC Publisher kod tabanını değiştirmeniz gerekir.

Yapılandırma dosyasının sözdizimi aşağıdaki gibidir:

```json
// The configuration settings file consists of two objects:
// 1) The 'Defaults' object, which defines defaults for the telemetry configuration
// 2) An array 'EndpointSpecific' of endpoint specific configuration
// Both objects are optional and if they are not specified, then publisher uses
// its internal default configuration, which generates telemetry messages compatible
// with the Microsoft Connected factory Preconfigured Solution (https://github.com/Azure/azure-iot-connected-factory).

// A JSON telemetry message for Connected factory looks like:
//  {
//      "NodeId": "i=2058",
//      "ApplicationUri": "urn:myopcserver",
//      "DisplayName": "CurrentTime",
//      "Value": {
//          "Value": "10.11.2017 14:03:17",
//          "SourceTimestamp": "2017-11-10T14:03:17Z"
//      }
//  }

// The 'Defaults' object in the sample below, are similar to what publisher is
// using as its internal default telemetry configuration.
{
    "Defaults": {
        // The first two properties ('EndpointUrl' and 'NodeId' are configuring data
        // taken from the OpcPublisher node configuration.
        "EndpointUrl": {

            // The following three properties can be used to configure the 'EndpointUrl'
            // property in the JSON message send by publisher to IoT Hub.

            // Publish controls if the property should be part of the JSON message at all.
            "Publish": false,

            // Pattern is a regular expression, which is applied to the actual value of the
            // property (here 'EndpointUrl').
            // If this key is omitted (which is the default), then no regex matching is done
            // at all, which improves performance.
            // If the key is used you need to define groups in the regular expression.
            // Publisher applies the regular expression and then concatenates all groups
            // found and use the resulting string as the value in the JSON message to
            //sent to IoT Hub.
            // This example mimics the default behaviour and defines a group,
            // which matches the conplete value:
            "Pattern": "(.*)",
            // Here some more exaples for 'Pattern' values and the generated result:
            // "Pattern": "i=(.*)"
            // defined for Defaults.NodeId.Pattern, will generate for the above sample
            // a 'NodeId' value of '2058'to be sent by publisher
            // "Pattern": "(i)=(.*)"
            // defined for Defaults.NodeId.Pattern, will generate for the above sample
            // a 'NodeId' value of 'i2058' to be sent by publisher

            // Name allows you to use a shorter string as property name in the JSON message
            // sent by publisher. By default the property name is unchanged and will be
            // here 'EndpointUrl'.
            // The 'Name' property can only be set in the 'Defaults' object to ensure
            // all messages from publisher sent to IoT Hub have a similar layout.
            "Name": "EndpointUrl"

        },
        "NodeId": {
            "Publish": true,

            // If you set Defaults.NodeId.Name to "ni", then the "NodeId" key/value pair
            // (from the above example) will change to:
            //      "ni": "i=2058",
            "Name": "NodeId"
        },

        // The MonitoredItem object is configuring the data taken from the MonitoredItem
        // OPC UA object for published nodes.
        "MonitoredItem": {

            // If you set the Defaults.MonitoredItem.Flat to 'false', then a
            // 'MonitoredItem' object will appear, which contains 'ApplicationUri'
            // and 'DisplayNode' proerties:
            //      "NodeId": "i=2058",
            //      "MonitoredItem": {
            //          "ApplicationUri": "urn:myopcserver",
            //          "DisplayName": "CurrentTime",
            //      }
            // The 'Flat' property can only be used in the 'MonitoredItem' and
            // 'Value' objects of the 'Defaults' object and will be used
            // for all JSON messages sent by publisher.
            "Flat": true,

            "ApplicationUri": {
                "Publish": true,
                "Name": "ApplicationUri"
            },
            "DisplayName": {
                "Publish": true,
                "Name": "DisplayName"
            }
        },
        // The Value object is configuring the properties taken from the event object
        // the OPC UA stack provided in the value change notification event.
        "Value": {
            // If you set the Defaults.Value.Flat to 'true', then the 'Value'
            // object will disappear completely and the 'Value' and 'SourceTimestamp'
            // members won't be nested:
            //      "DisplayName": "CurrentTime",
            //      "Value": "10.11.2017 14:03:17",
            //      "SourceTimestamp": "2017-11-10T14:03:17Z"
            // The 'Flat' property can only be used for the 'MonitoredItem' and 'Value'
            // objects of the 'Defaults' object and will be used for all
            // messages sent by publisher.
            "Flat": false,

            "Value": {
                "Publish": true,
                "Name": "Value"
            },
            "SourceTimestamp": {
                "Publish": true,
                "Name": "SourceTimestamp"
            },
            // 'StatusCode' is the 32 bit OPC UA status code
            "StatusCode": {
                "Publish": false,
                "Name": "StatusCode"
                // 'Pattern' is ignored for the 'StatusCode' value
            },
            // 'Status' is the symbolic name of 'StatusCode'
            "Status": {
                "Publish": false,
                "Name": "Status"
            }
        }
    },

    // The next object allows to configure 'Publish' and 'Pattern' for specific
    // endpoint URLs. Those will overwrite the ones specified in the 'Defaults' object
    // or the defaults used by publisher.
    // It is not allowed to specify 'Name' and 'Flat' properties in this object.
    "EndpointSpecific": [
        // The following shows how a endpoint specific configuration can look like:
        {
            // 'ForEndpointUrl' allows to configure for which OPC UA server this
            // object applies and is a required property for all objects in the
            // 'EndpointSpecific' array.
            // The value of 'ForEndpointUrl' must be an 'EndpointUrl' configured in
            // the publishednodes.json confguration file.
            "ForEndpointUrl": "opc.tcp://<your_opcua_server>:<your_opcua_server_port>/<your_opcua_server_path>",
            "EndpointUrl": {
                // We overwrite the default behaviour and publish the
                // endpoint URL in this case.
                "Publish": true,
                // We are only interested in the URL part following the 'opc.tcp://' prefix
                // and define a group matching this.
                "Pattern": "opc.tcp://(.*)"
            },
            "NodeId": {
                // We are not interested in the configured 'NodeId' value, 
                // so we do not publish it.
                "Publish": false
                // No 'Pattern' key is specified here, so the 'NodeId' value will be
                // taken as specified in the publishednodes configuration file.
            },
            "MonitoredItem": {
                "ApplicationUri": {
                    // We already publish the endpoint URL, so we do not want
                    // the ApplicationUri of the MonitoredItem to be published.
                    "Publish": false
                },
                "DisplayName": {
                    "Publish": true
                }
            },
            "Value": {
                "Value": {
                    // The value of the node is important for us, everything else we
                    // are not interested in to keep the data ingest as small as possible.
                    "Publish": true
                },
                "SourceTimestamp": {
                    "Publish": false
                },
                "StatusCode": {
                    "Publish": false
                },
                "Status": {
                    "Publish": false
                }
            }
        }
    ]
}
```

## <a name="next-steps"></a>Sonraki adımlar

Şimdi Nasıl OPC Publisher yapılandırmak için öğrendim, önerilen sonraki adım [Nasıl OPC Publisher çalıştırın](howto-opc-publisher-run.md)öğrenmektir.
