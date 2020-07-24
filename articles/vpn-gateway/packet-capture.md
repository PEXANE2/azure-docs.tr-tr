---
title: 'Azure VPN Gateway: paket yakalamaları yapılandırma'
description: VPN ağ geçitleri üzerinde kullanabileceğiniz paket yakalama işlevleri hakkında bilgi edinin.
services: vpn-gateway
author: radwiv
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 10/15/2019
ms.author: radwiv
ms.openlocfilehash: 3ba3046367ceece6bf0ddf157451025c79977324
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87077216"
---
# <a name="configure-packet-captures-for-vpn-gateways"></a>VPN ağ geçitleri için paket yakalamaları yapılandırma

Bağlantı ve performansla ilgili sorunlar genellikle karmaşıktır ve sorunun nedenini azaltmak için önemli miktarda zaman ve çaba alır. Paket yakalama özelliği büyük ölçüde sorunun kapsamını ağın müşteri tarafında, ağın Azure tarafında veya arasında bir yerde olup olmadığı gibi, ağın belirli bölümleriyle daraltma süresini azaltmaya yardımcı olur. Sorun daraltıldıktan sonra, hata ayıklama ve düzeltici eylem gerçekleştirmek çok daha verimlidir.

Paket yakalama için bazı yaygın olarak kullanılabilecek araçlar vardır. Özellikle yüksek hacimli trafik senaryolarıyla çalışırken, bu araçlarla ilgili paket yakalamalarını almak çok daha fazla olabilir. Bir VPN ağ geçidi paket yakalaması tarafından sunulan filtreleme özellikleri, önemli bir farklılık haline gelir. Yaygın olarak kullanılabilir paket yakalama araçlarına ek olarak bir VPN Gateway paket yakalaması kullanabilirsiniz.

## <a name="vpn-gateway-packet-capture-filtering-capabilities"></a>VPN Gateway paket yakalama filtreleme özellikleri

VPN Gateway paket yakalamaları, ağ geçidinde veya müşteri gereksinimlerine bağlı olarak belirli bir bağlantıda çalıştırılabilir. Aynı anda birden çok tünelde paket yakalamalarını çalıştırabilirsiniz. Bir VPN ağ geçidi üzerinde filtreleme ile birlikte tek veya çift yönlü trafiği, ıKE ve ESP trafiğini ve iç paketleri yakalayabilirsiniz.

Beş demet filtresi (kaynak alt ağ, hedef alt ağ, kaynak bağlantı noktası, hedef bağlantı noktası, protokol) ve TCP bayrakları (SYN, ACK, FIN, URG, PSH, RST) kullanımı, yüksek hacimli bir trafikte sorunları yalıtmak için yararlıdır.

Her bir özelliğin açıklaması ile bir JSON ve JSON şeması örneğine bakın. Ayrıca, paket yakalamalarını çalıştırırken bazı sınırlamalara göz önünde bulabilirsiniz:
- Şemada, filtre bir dizi olarak gösterilir, ancak tek seferde yalnızca bir filtre kullanılabilir.
- Aynı anda birden çok ağ geçidi genelinde paket yakalamalarına izin verilmiyor.
- Aynı bağlantı üzerinde aynı anda birden çok paket yakalamalarına izin verilmiyor. Paket yakalamaları farklı bağlantılarda aynı anda çalıştırabilirsiniz.
- En fazla beş paket yakalama, ağ geçidi başına paralel olarak çalıştırılabilir. Bu paket yakalamaları, ağ geçidi genelinde paket yakalama veya bağlantı paketi yakalama başına bir bileşim olabilir.

### <a name="example-json"></a>Örnek JSON
```JSON-interactive
{
  "TracingFlags": 11,
  "MaxPacketBufferSize": 120,
  "MaxFileSize": 200,
  "Filters": [
    {
      "SourceSubnets": [
        "20.1.1.0/24"
      ],
      "DestinationSubnets": [
        "10.1.1.0/24"
      ],
      "SourcePort": [
        500
      ],
      "DestinationPort": [
        4500
      ],
      "Protocol": [
        6
      ],
      "TcpFlags": 16,
      "CaptureSingleDirectionTrafficOnly": true
    }
  ]
}
```
### <a name="json-schema"></a>JSON şeması
```JSON-interactive
{
    "type": "object",
    "title": "The Root Schema",
    "description": "The root schema input JSON filter for packet capture",
    "default": {},
    "additionalProperties": true,
    "required": [
        "TracingFlags",
        "MaxPacketBufferSize",
        "MaxFileSize",
        "Filters"
    ],
    "properties": {
        "TracingFlags": {
            "$id": "#/properties/TracingFlags",
            "type": "integer",
            "title": "The Tracingflags Schema",
            "description": "Tracing flags that customer can pass to define which packets are to be captured. Supported values are CaptureESP = 1, CaptureIKE = 2, CaptureOVPN = 8. The final value is OR of the bits.",
            "default": 11,
            "examples": [
                11
            ]
        },
        "MaxPacketBufferSize": {
            "$id": "#/properties/MaxPacketBufferSize",
            "type": "integer",
            "title": "The Maxpacketbuffersize Schema",
            "description": "Maximum buffer size of each packet. The capture will only contain contents of each packet truncated to this size.",
            "default": 120,
            "examples": [
                120
            ]
        },
        "MaxFileSize": {
            "$id": "#/properties/MaxFileSize",
            "type": "integer",
            "title": "The Maxfilesize Schema",
            "description": "Maximum file size of the packet capture file. It is a circular buffer.",
            "default": 100,
            "examples": [
                100
            ]
        },
        "Filters": {
            "$id": "#/properties/Filters",
            "type": "array",
            "title": "The Filters Schema",
            "description": "An array of filters that can be passed to filter inner ESP traffic.",
            "default": [],
            "examples": [
                [
                    {
                        "Protocol": [
                            6
                        ],
                        "CaptureSingleDirectionTrafficOnly": true,
                        "SourcePort": [
                            500
                        ],
                        "DestinationPort": [
                            4500
                        ],
                        "TcpFlags": 16,
                        "SourceSubnets": [
                            "20.1.1.0/24"
                        ],
                        "DestinationSubnets": [
                            "10.1.1.0/24"
                        ]
                    }
                ]
            ],
            "additionalItems": true,
            "items": {
                "$id": "#/properties/Filters/items",
                "type": "object",
                "title": "The Items Schema",
                "description": "An explanation about the purpose of this instance.",
                "default": {},
                "examples": [
                    {
                        "SourcePort": [
                            500
                        ],
                        "DestinationPort": [
                            4500
                        ],
                        "TcpFlags": 16,
                        "SourceSubnets": [
                            "20.1.1.0/24"
                        ],
                        "DestinationSubnets": [
                            "10.1.1.0/24"
                        ],
                        "Protocol": [
                            6
                        ],
                        "CaptureSingleDirectionTrafficOnly": true
                    }
                ],
                "additionalProperties": true,
                "required": [
                    "SourceSubnets",
                    "DestinationSubnets",
                    "SourcePort",
                    "DestinationPort",
                    "Protocol",
                    "TcpFlags",
                    "CaptureSingleDirectionTrafficOnly"
                ],
                "properties": {
                    "SourceSubnets": {
                        "$id": "#/properties/Filters/items/properties/SourceSubnets",
                        "type": "array",
                        "title": "The Sourcesubnets Schema",
                        "description": "An array of source subnets that need to match the Source IP address of a packet. Packet can match any one value in the array of inputs.",
                        "default": [],
                        "examples": [
                            [
                                "20.1.1.0/24"
                            ]
                        ],
                        "additionalItems": true,
                        "items": {
                            "$id": "#/properties/Filters/items/properties/SourceSubnets/items",
                            "type": "string",
                            "title": "The Items Schema",
                            "description": "An explanation about the purpose of this instance.",
                            "default": "",
                            "examples": [
                                "20.1.1.0/24"
                            ]
                        }
                    },
                    "DestinationSubnets": {
                        "$id": "#/properties/Filters/items/properties/DestinationSubnets",
                        "type": "array",
                        "title": "The Destinationsubnets Schema",
                        "description": "An array of destination subnets that need to match the Destination IP address of a packet. Packet can match any one value in the array of inputs.",
                        "default": [],
                        "examples": [
                            [
                                "10.1.1.0/24"
                            ]
                        ],
                        "additionalItems": true,
                        "items": {
                            "$id": "#/properties/Filters/items/properties/DestinationSubnets/items",
                            "type": "string",
                            "title": "The Items Schema",
                            "description": "An explanation about the purpose of this instance.",
                            "default": "",
                            "examples": [
                                "10.1.1.0/24"
                            ]
                        }
                    },
                    "SourcePort": {
                        "$id": "#/properties/Filters/items/properties/SourcePort",
                        "type": "array",
                        "title": "The Sourceport Schema",
                        "description": "An array of source ports that need to match the Source port of a packet. Packet can match any one value in the array of inputs.",
                        "default": [],
                        "examples": [
                            [
                                500
                            ]
                        ],
                        "additionalItems": true,
                        "items": {
                            "$id": "#/properties/Filters/items/properties/SourcePort/items",
                            "type": "integer",
                            "title": "The Items Schema",
                            "description": "An explanation about the purpose of this instance.",
                            "default": 0,
                            "examples": [
                                500
                            ]
                        }
                    },
                    "DestinationPort": {
                        "$id": "#/properties/Filters/items/properties/DestinationPort",
                        "type": "array",
                        "title": "The Destinationport Schema",
                        "description": "An array of destination ports that need to match the Destination port of a packet. Packet can match any one value in the array of inputs.",
                        "default": [],
                        "examples": [
                            [
                                4500
                            ]
                        ],
                        "additionalItems": true,
                        "items": {
                            "$id": "#/properties/Filters/items/properties/DestinationPort/items",
                            "type": "integer",
                            "title": "The Items Schema",
                            "description": "An explanation about the purpose of this instance.",
                            "default": 0,
                            "examples": [
                                4500
                            ]
                        }
                    },
                    "Protocol": {
                        "$id": "#/properties/Filters/items/properties/Protocol",
                        "type": "array",
                        "title": "The Protocol Schema",
                        "description": "An array of protocols that need to match the Protocol of a packet. Packet can match any one value in the array of inputs.",
                        "default": [],
                        "examples": [
                            [
                                6
                            ]
                        ],
                        "additionalItems": true,
                        "items": {
                            "$id": "#/properties/Filters/items/properties/Protocol/items",
                            "type": "integer",
                            "title": "The Items Schema",
                            "description": "An explanation about the purpose of this instance.",
                            "default": 0,
                            "examples": [
                                6
                            ]
                        }
                    },
                    "TcpFlags": {
                        "$id": "#/properties/Filters/items/properties/TcpFlags",
                        "type": "integer",
                        "title": "The Tcpflags Schema",
                        "description": "A list of TCP flags. The TCP flags set on the packet must match any flag in the list of flags provided. FIN = 0x01,SYN = 0x02,RST = 0x04,PSH = 0x08,ACK = 0x10,URG = 0x20,ECE = 0x40,CWR = 0x80. An OR of flags can be provided.",
                        "default": 0,
                        "examples": [
                            16
                        ]
                    },
                    "CaptureSingleDirectionTrafficOnly": {
                        "$id": "#/properties/Filters/items/properties/CaptureSingleDirectionTrafficOnly",
                        "type": "boolean",
                        "title": "The Capturesingledirectiontrafficonly Schema",
                        "description": "A flags which when set captures reverse traffic also.",
                        "default": false,
                        "examples": [
                            true
                        ]
                    }
                }
            }
        }
    }
}
```

## <a name="setup-packet-capture-using-powershell"></a>PowerShell kullanarak paket yakalamayı ayarlama

Paket yakalamalarını başlatmak ve durdurmak için PowerShell komutları için aşağıdaki örneklere bakın. Parametre seçenekleri hakkında daha fazla bilgi için bu PowerShell [belgesine](https://docs.microsoft.com/powershell/module/az.network/start-azvirtualnetworkgatewaypacketcapture)bakın.

### <a name="start-packet-capture-for-a-vpn-gateway"></a>VPN ağ geçidi için paket yakalamayı Başlat

```azurepowershell-interactive
Start-AzVirtualnetworkGatewayPacketCapture -ResourceGroupName "YourResourceGroupName" -Name "YourVPNGatewayName"
```

İsteğe bağlı parametre **-FilterData** parametresi filtre uygulamak için kullanılabilir.

### <a name="stop-packet-capture-for-a-vpn-gateway"></a>VPN ağ geçidi için paket yakalamayı durdur

```azurepowershell-interactive
Stop-AzVirtualNetworkGatewayPacketCapture -ResourceGroupName "YourResourceGroupName" -Name "YourVPNGatewayName" -SasUrl "YourSASURL"
```

### <a name="start-packet-capture-for-a-vpn-gateway-connection"></a>VPN Ağ Geçidi bağlantısı için paket yakalamayı Başlat

```azurepowershell-interactive
Start-AzVirtualNetworkGatewayConnectionPacketCapture -ResourceGroupName "YourResourceGroupName" -Name "YourVPNGatewayConnectionName"
```

İsteğe bağlı parametre **-FilterData** parametresi filtre uygulamak için kullanılabilir.

### <a name="stop-packet-capture-on-a-vpn-gateway-connection"></a>VPN ağ geçidi bağlantısında paket yakalamayı durdur

```azurepowershell-interactive
Stop-AzVirtualNetworkGatewayConnectionPacketCapture -ResourceGroupName "YourResourceGroupName" -Name "YourVPNGatewayConnectionName" -SasUrl "YourSASURL"
```

## <a name="key-considerations"></a>Dikkat edilmesi gereken temel konular

- Paket yakalamalarını çalıştırmak performansı etkileyebilir. Gerekli olmadığında paket yakalamayı durdurmayı unutmayın.
- Önerilen minimum paket yakalama süresi 600 saniyedir. Yol üzerindeki birden çok bileşen arasındaki eşitleme sorunları nedeniyle, daha kısa bir paket yakalama süresi, verilerin tamamını sağlamayabilir.
- Paket yakalama veri dosyaları PCAP biçiminde oluşturulur. PCAP dosyalarını açmak için Wireshark veya diğer yaygın olarak kullanılabilir uygulamaları kullanın.

## <a name="next-steps"></a>Sonraki adımlar

VPN Gateway hakkında daha fazla bilgi için bkz. [VPN Gateway hakkında](vpn-gateway-about-vpngateways.md)
