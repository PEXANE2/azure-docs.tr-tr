---
title: Uzamsal analiz için Görüntü İşleme ile canlı videoyu çözümleme-Azure
description: Bu öğreticide, canlı video analizinin Azure bilişsel hizmetler 'deki Görüntü İşleme uzamsal analiz AI özelliği ile birlikte nasıl kullanılacağı gösterilmektedir. Bu, (benzetimli) bir IP kamerasından canlı video akışını analiz edebilir.
ms.topic: tutorial
ms.date: 09/08/2020
ms.openlocfilehash: 98ee57d4916ac0a8da8b48a9cdd881468b2d75d5
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90946799"
---
# <a name="analyze-live-video-with-computer-vision-for-spatial-analysis-preview"></a>Uzamsal analizler için Görüntü İşleme ile canlı videoyu çözümleme (Önizleme)

Bu öğreticide, canlı video analizinin Azure bilişsel [Hizmetler 'deki uzamsal ANALYSIS AI hizmeti için görüntü işleme](https://azure.microsoft.com/services/cognitive-services/computer-vision/) ile birlikte nasıl kullanılacağı gösterilmektedir. Bu, (benzetimli) bir IP kamerasından canlı video akışını analiz edebilir. Bu çıkarım sunucusunun, fiziksel alanda insanlar ve taşıma arasındaki uzamsal ilişkileri anlamak için akış videosunu analiz etmenizi nasıl sağladığını göreceksiniz.  Video akışındaki çerçevelerin bir alt kümesi bu çıkarım sunucusuna gönderilir ve sonuçlar IoT Edge hub 'a gönderilir ve bazı koşullar karşılandığında, video klipleri Azure Media Services varlık olarak kaydedilir ve saklanır.

Bu öğreticide şunları yapmanız gerekir:

> [!div class="checklist"]
> * Kaynakları ayarlayın.
> * Kodu inceleyin.
> * Örnek kodu çalıştırın.
> * Olayları izleme.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="suggested-pre-reading"></a>Önerilen önceden okuma

Başlamadan önce şu makaleleri okuyun:

* [IoT Edge genel bakış üzerinde canlı video analizi](overview.md)
* [IoT Edge terminolojisinde canlı video analizi](terminology.md)
* [Medya grafiği kavramları](media-graph-concept.md)
* [Olay tabanlı video kaydı](event-based-video-recording-concept.md)
* [Öğretici: IoT Edge modülünü geliştirme](https://docs.microsoft.com/azure/iot-edge/tutorial-develop-for-linux)
* [Azure Stack Edge üzerinde canlı video analizi dağıtma](deploy-azure-stack-edge-how-to.md) 

## <a name="prerequisites"></a>Önkoşullar

Aşağıda, uzamsal analiz modülünü canlı video analizi modülüne bağlama önkoşulları verilmiştir.

* Geliştirme makinenizde [Visual Studio Code](https://code.visualstudio.com/) . [Azure IoT araçları uzantısına](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools)sahip olduğunuzdan emin olun.
    * Geliştirme makinenizin bağlı olduğu ağın bağlantı noktası 5671 üzerinden gelişmiş Ileti kuyruğu protokolüne izin vermek için bağlı olduğundan emin olun. Bu kurulum, Azure IoT araçlarının Azure IoT Hub ile iletişim kurmasını sağlar.
* GPU hızlandırmalı [Azure Stack Edge](https://azure.microsoft.com/products/azure-stack/edge/) .  
    Azure Stack Edge 'i GPU hızlandırmalı kullanmanızı öneririz, ancak kapsayıcı, [NVIDIA Tesla T4 GPU 'su](https://www.nvidia.com/en-us/data-center/tesla-t4/)olan diğer tüm cihazlarda çalışır. 
* Uzamsal analiz için Azure bilişsel [hizmet görüntü işleme kapsayıcısı](https://azure.microsoft.com/services/cognitive-services/computer-vision/) .  
    Bu kapsayıcıyı kullanabilmeniz için, ilişkili **API anahtarını** ve bir **uç nokta URI 'sini**almak için bir görüntü işleme kaynağınız olması gerekir. API anahtarı Azure portal Görüntü İşleme genel bakış ve anahtarlar sayfalarında kullanılabilir. Kapsayıcıyı başlatmak için anahtar ve uç nokta gereklidir.

## <a name="overview"></a>Genel Bakış

> [!div class="mx-imgBorder"]
> :::image type="content" source="./media/spatial-analysis-tutorial/overview.png" alt-text="Uzamsal Analize genel bakış":::
 
Bu diyagramda, sinyallerin Bu öğreticide nasıl akagösterdiği gösterilmektedir. [Edge modülü](https://github.com/Azure/live-video-analytics/tree/master/utilities/rtspsim-live555) , gerçek zamanlı akış protokolü (RTSP) sunucusunu BARıNDıRAN bir IP kamerasına benzetim yapar. Bir [RTSP kaynak](media-graph-concept.md#rtsp-source) düğümü, bu sunucudan video akışını çeker ve [çerçeve hızı filtre işlemcisi](media-graph-concept.md#frame-rate-filter-processor) düğümüne video çerçeveleri gönderir. Bu işlemci, MediaGraphCognitiveServicesVisionExtension işlemci düğümüne ulaşan video akışının kare oranını sınırlandırır.

MediaGraphCognitiveServicesVisionExtension düğümü bir ara sunucu rolünü yürütür. Video çerçevelerini belirtilen görüntü türüne dönüştürür. Ardından, görüntüyü **paylaşılan bellek** üzerine bir GRPC uç noktasının ARKASıNDA bulunan AI işlemlerini çalıştıran başka bir uç modüle geçirir. Bu örnekte, bu Edge modülü, uzamsal analiz modülüdür. MediaGraphCognitiveServicesVisionExtension işlemci düğümü iki şey yapar:

* Sonuçları toplar ve olayları [IoT Hub havuz](media-graph-concept.md#iot-hub-message-sink) düğümüne yayımlar. Düğüm daha sonra bu olayları [IoT Edge hub 'ına](https://docs.microsoft.com/azure/iot-edge/iot-edge-glossary#iot-edge-hub)gönderir. 
* Ayrıca, [sinyal kapısı işlemcisi](media-graph-concept.md#signal-gate-processor) kullanarak RTSP kaynağından 30 saniyelik bir video klibi yakalar ve bunu bir Media Services varlığı olarak depolar.

## <a name="create-the-computer-vision-resource"></a>Görüntü İşleme kaynağı oluşturma

[Azure Portal](https://docs.microsoft.com/azure/iot-edge/how-to-deploy-modules-portal) ya da Azure clı aracılığıyla görüntü işleme türünde bir Azure kaynağı oluşturmanız gerekir. Kapsayıcıya erişim isteğiniz onaylandıktan ve Azure abonelik KIMLIĞINIZ kaydettirildikten sonra kaynağı oluşturabilirsiniz... https://aka.ms/csgateKullanım durumunu ve Azure ABONELIK kimliğinizi göndermek için adresine gidin.  Azure kaynağını, erişim Isteği formunda sağlanmış olan Azure aboneliğini kullanarak oluşturmanız gerekir.

### <a name="gathering-required-parameters"></a>Gerekli parametreler toplanıyor

Uzamsal analiz kapsayıcısı dahil olmak üzere, gereken tüm bilişsel hizmetler kapsayıcıları için üç birincil parametre vardır. Son Kullanıcı Lisans Sözleşmesi 'nin (EULA) kabul et değeriyle mevcut olması gerekir. Ayrıca, hem bir uç nokta URL 'SI hem de API anahtarı gereklidir.

### <a name="endpoint-uri-endpoint_uri"></a>Uç nokta URI 'SI {ENDPOINT_URI}

Uç nokta URI değeri, bilişsel hizmetler kaynağının Azure portal genel bakış sayfasında bulunur. Genel Bakış sayfasına gidin ve uç nokta URI 'sini bulun. 

> [!div class="mx-imgBorder"]
> :::image type="content" source="./media/spatial-analysis-tutorial/keys-endpoint.png" alt-text="Anahtarlar ve uç nokta":::

### <a name="keys-api_key"></a>Anahtarlar {API_KEY}

Bu anahtar, uzamsal analiz kapsayıcısını başlatmak için kullanılır ve ilgili bilişsel hizmet kaynağının Azure portal tuşları sayfasında kullanılabilir. Anahtarlar sayfasına gidin ve anahtarları bulun.

> [!div class="mx-imgBorder"]
> :::image type="content" source="./media/spatial-analysis-tutorial/endpoint-uri.png" alt-text="Uç nokta URI 'SI":::

## <a name="set-up-azure-stack-edge"></a>Azure Stack kenarını ayarlama

Azure Stack kenarını ayarlamak için aşağıdaki [adımları](https://docs.microsoft.com/azure/databox-online/azure-stack-edge-gpu-deploy-prep) Izleyin ve canlı video analizlerini ve uzamsal analiz modüllerini dağıtmak için aşağıdaki adımları izlemeye devam edin.

## <a name="set-up-your-development-environment"></a>Geliştirme ortamınızı ayarlama

1. Depoyu bu konumdan kopyala: https://github.com/Azure-Samples/live-video-analytics-iot-edge-csharp .
1. Visual Studio Code, deponun indirildiği klasörü açın.
1. Visual Studio Code, src/buluttan cihaza-Console-App klasörüne gidin. Burada bir dosya oluşturun ve appsettings.js. Bu dosya, programı çalıştırmak için gereken ayarları içerir.
1. Aşağıdaki adımları izleyerek Azure Stack kenarından ıothubconnectionstring ' i alın:

    * Azure portal IoT Hub gidin ve sol gezinti bölmesinde açık ' a tıklayın `Shared access policies` .
    * `iothubowner`Paylaşılan erişim anahtarlarını al ' a tıklayın.
    * Öğesini kopyalayın  `Connection String – primary key` ve VSCode 'daki giriş kutusuna yapıştırın.
    
        Bağlantı dizesi şöyle görünür: <br/>`HostName=xxx.azure-devices.net;SharedAccessKeyName=iothubowner;SharedAccessKey=xxx`
1. Aşağıdaki içeriği dosyaya kopyalayın. Değişkenleri değiştirdiğinizden emin olun.
    
    ```json
    {
        "IoThubConnectionString" : " HostName=<IoTHubName>.azure-devices.net;SharedAccessKeyName=iothubowner;SharedAccessKey=<SharedAccessKey>”,
        "deviceId" : "<your Azure Stack Edge name>",
        "moduleId" : "lvaEdge"
    } 
    ```
1. Src/Edge klasörüne gidin ve. env adlı bir dosya oluşturun.
1. /CloudDrive/LVA-Sample/Edge-Deployment/-env dosyasının içeriğini kopyalayın. Metin aşağıdaki kod gibi görünmelidir.

    ```env
    SUBSCRIPTION_ID="<Subscription ID>"  
    RESOURCE_GROUP="<Resource Group>"  
    AMS_ACCOUNT="<AMS Account ID>"  
    IOTHUB_CONNECTION_STRING="HostName=xxx.azure-devices.net;SharedAccessKeyName=iothubowner;SharedAccessKey=xxx"  
    AAD_TENANT_ID="<AAD Tenant ID>"  
    AAD_SERVICE_PRINCIPAL_ID="<AAD SERVICE_PRINCIPAL ID>"  
    AAD_SERVICE_PRINCIPAL_SECRET="<AAD SERVICE_PRINCIPAL ID>"  
    INPUT_VIDEO_FOLDER_ON_DEVICE="/home/lvaadmin/samples/input"  
    OUTPUT_VIDEO_FOLDER_ON_DEVICE="/var/media"
    APPDATA_FOLDER_ON_DEVICE="/var/local/mediaservices"
    CONTAINER_REGISTRY_USERNAME_myacr="<your container registry username>"  
    CONTAINER_REGISTRY_PASSWORD_myacr="<your container registry password>"   
    ```
    
## <a name="set-up-deployment-template"></a>Dağıtım şablonu ayarlama  

Uzamsal analiz modülünü üzerine/src/Edge/deployment.template.jsekleyin. Şablondan, lvaEdge modülü, rtspsım modülü ve uzamsal analiz modülümüzdür.

<p>
<details>
<summary>Bunu genişletin ve örnek dağıtım şablonumuzu görüntüleyin.  
İçeriği buradan kopyalayın ve üzerinde/src/Edge/deployment.template.jsyapıştırın.
</summary>
<pre><code>
{
  "$schema-template": "2.0.0",
  "modulesContent": {
    "$edgeAgent": {
      "properties.desired": {
        "schemaVersion": "1.0",
        "runtime": {
          "type": "docker",
          "settings": {
            "minDockerVersion": "v1.25",
            "loggingOptions": "",
            "registryCredentials": {
            }
          }
        },
        "systemModules": {
          "edgeAgent": {
            "type": "docker",
            "settings": {
              "image": "mcr.microsoft.com/azureiotedge-agent:1.0",
              "createOptions": {}
            }
          },
          "edgeHub": {
            "type": "docker",
            "status": "running",
            "restartPolicy": "always",
            "settings": {
              "image": "mcr.microsoft.com/azureiotedge-hub:1.0",
              "createOptions": {
                "HostConfig": {
                  "PortBindings": {
                    "5671/tcp": [
                      {
                        "HostPort": "5671"
                      }
                    ],
                    "8883/tcp": [
                      {
                        "HostPort": "8883"
                      }
                    ],
                    "443/tcp": [
                      {
                        "HostPort": "443"
                      }
                    ]
                  }
                }
              }
            }
          }
        },
        "modules": {
          "lvaEdge": {
            "version": "1.0",
            "type": "docker",
            "status": "running",
            "restartPolicy": "always",
            "settings": {
              "image": "mcr.microsoft.com/media/live-video-analytics:1",
              "createOptions": {
                "HostConfig": {
                  "LogConfig": {
                    "Type": "",
                    "Config": {
                      "max-size": "10m",
                      "max-file": "10"
                    }
                  },
                  "Binds": [
                    "$OUTPUT_VIDEO_FOLDER_ON_DEVICE:/var/media/",
                    "$APPDATA_FOLDER_ON_DEVICE:/var/lib/azuremediaservices"
                  ],
                  "IpcMode": "host",
                  "ShmSize": 1536870912
                }
              }
            },
            "env": {
              "IS_DEVELOPER_ENVIRONMENT": {
                "value": "true"
              }
            }
          },
          "rtspsim": {
              "version": "1.0",
              "type": "docker",
              "status": "running",
              "restartPolicy": "always",
              "settings": {
                "image": "mcr.microsoft.com/lva-utilities/rtspsim-live555:1.2",
                "createOptions": {
                  "HostConfig": {
                    "Mounts": [
                      {
                        "Target": "/live/mediaServer/media",
                        "Source": "lvaspatialanalysislocal",
                        "Type": "volume"
                      }
                    ],
                    "PortBindings": {
                      "554/tcp": [
                        {
                          "HostPort": "554"
                        }
                      ]
                    }
                  }
                }
              }
            },
          "spatialAnalysis": {
            "version": "1.0",
            "type": "docker",
            "status": "running",
            "restartPolicy": "always",
            "settings": {
              "image": "mcr.microsoft.com/azure-cognitive-services/spatial-analysis:1.0",
              "createOptions": {
                "HostConfig": {
                  "PortBindings": {
                    "50051/tcp": [
                      {
                        "HostPort": "50051"
                      }
                    ]
                  },
                  "IpcMode": "host",
                  "Binds": [
                      "/tmp/.X11-unix:/tmp/.X11-unix"
                  ],
                  "Runtime": "nvidia",
                  "ShmSize": 536870911,
                  "LogConfig": {
                      "Type": "json-file",
                      "Config": {
                          "max-size": "10m",
                          "max-file": "200"
                      }
                  }
                }
              }
            },
            "env": {
              "DISPLAY": {
                "value": ":0"
              },
              "ARCHON_SHARED_BUFFER_LIMIT": {
                "value": "377487360"
              },
              "ARCHON_PERF_MARKER": {
                "value": "false"
              },
              "QT_X11_NO_MITSHM": {
                "value": "1"
              },
              "OMP_WAIT_POLICY": {
                "value": "PASSIVE"
              },
              "EULA": {
                "value": "accept"
              },
              "BILLING_ENDPOINT": {
                "value": "<Use one key from Archon azure resource (keys page)>"
              },
              "API_KEY": {
                "value": "<Use endpoint from Archon azure resource (overview page)>"
              }
            }
          }
        }
      }
    },
    "$edgeHub": {
      "properties.desired": {
        "schemaVersion": "1.0",
        "routes": {
          "LVAToHub": "FROM /messages/modules/lvaEdge/outputs/* INTO $upstream"
        },
        "storeAndForwardConfiguration": {
          "timeToLiveSecs": 7200
        }
      }
    },
    "lvaEdge": {
      "properties.desired": {
        "applicationDataDirectory": "/var/lib/azuremediaservices",
        "azureMediaServicesArmId": "/subscriptions/$SUBSCRIPTION_ID/resourceGroups/$RESOURCE_GROUP/providers/microsoft.media/mediaservices/$AMS_ACCOUNT",
        "aadTenantId": "$AAD_TENANT_ID",
        "aadServicePrincipalAppId": "$AAD_SERVICE_PRINCIPAL_ID",
        "aadServicePrincipalSecret": "$AAD_SERVICE_PRINCIPAL_SECRET",
        "aadEndpoint": "https://login.microsoftonline.com",
        "aadResourceId": "https://management.core.windows.net/",
        "armEndpoint": "https://management.azure.com/",
        "diagnosticsEventsOutputName": "AmsDiagnostics",
        "operationalEventsOutputName": "AmsOperational",        
        "logLevel": "Info",
        "logCategories": "Application,Events,MediaPipeline",
        "allowUnsecuredEndpoints": true,
        "telemetryOptOut": false
      }
    },
    "spatialAnalysis": {
      "properties.desired": {
        "globalSettings": {
          "PlatformTelemetryEnabled": true,
          "CustomerTelemetryEnabled": true
        },
        "graphs": {
            "polygonCross": {
              "version": 2,
              "enabled": true,
              "platformloglevel": "info",
              "operationId": "cognitiveservices.vision.spatialanalysis-personcrossingpolygon.livevideoanalytics",
              "parameters": {
                  "BINDING_ADDRESS": "0.0.0.0:50051",
                  "DETECTOR_NODE_CONFIG": "{ \"show_debug_video\": false, \"gpu_index\": 0 }",
                  "SPACEANALYTICS_CONFIG": "{\"zones\":[{\"name\":\"polygon0\",\"polygon\":[[0,0],[0.6,0],[0.6,0.9],[0,0.9],[0,0]],\"threshold\":50,\"events\":[{\"type\":\"enter/exit\",\"config\":{\"trigger\":\"event\"}}]}]}"
              },
              "nodesloglevel": "info"
            },
            "personCount": {
              "version": 2,
              "enabled": false,
              "platformloglevel": "info",
              "operationId": "cognitiveservices.vision.spatialanalysis-personcount.livevideoanalytics",
              "parameters": {
                  "BINDING_ADDRESS": "0.0.0.0:50051",
                  "DETECTOR_NODE_CONFIG": "{ \"show_debug_video\": false, \"gpu_index\": 0 }",
                  "SPACEANALYTICS_CONFIG": "{\"zones\":[{\"name\":\"polygon0\",\"polygon\":[[0.8,0],[1,0],[1,1],[0.8,1],[0.8,0]],\"threshold\":50,\"events\":[{\"type\":\"count\",\"config\":{\"trigger\":\"event\"}}]}]}"
              },
              "nodesloglevel": "info"
            },
            "personDistance": {
              "version": 2,
              "enabled": false,
              "platformloglevel": "info",
              "operationId": "cognitiveservices.vision.spatialanalysis-persondistance.livevideoanalytics",
              "parameters": {
                  "BINDING_ADDRESS": "0.0.0.0:50051",
                  "DETECTOR_NODE_CONFIG": "{ \"show_debug_video\": false, \"gpu_index\": 0,\"gpu_index\": 0,\"do_calibration\": true}",
                  "SPACEANALYTICS_CONFIG": "{\"zones\":[{\"name\": \"distance_zone\", \"polygon\": [[0,0],[0,1],[1,1],[1,0],[0,0]],\"threshold\": 35.00,\"events\":[{\"type\": \"people_distance\",\"config\":{\"trigger\": \"event\",\"output_frequency\":1,\"minimum_distance_threshold\":6.0,\"maximum_distance_threshold\":35.0}}]}]}"
              },
              "nodesloglevel": "info"
            }
        }
      }
    }
  }
}
</code>
</pre>
</details>
</p>

Dikkat etmeniz gereken birkaç nokta vardır:

1. Bağlantı noktası bağlamayı ayarlayın.
    
    ```
    "PortBindings": {
        "50051/tcp": [
            {
                "HostPort": "50051"
            }
        ]
    },
    ```
1. LvaEdge ve uzamsal analiz modülü createOptions içindeki ıpcmode aynı olmalıdır ve konak olarak ayarlanmalıdır.
1. Dağıtım şablonu dosyanız dosya adı içinde "dağıtım" içermelidir, aksi takdirde dağıtım için bildirim üretilemez.
1. RTSP benzeticisinin çalışması için Birim sınırlarını oluşturduğunuzdan emin olun. Daha fazla bilgi için bkz. [Docker Volume takmaları kurma](deploy-azure-stack-edge-how-to.md#optional-setup-docker-volume-mounts).

    1. [SMB paylaşımıyla bağlantı kurmak](https://docs.microsoft.com/azure/databox-online/azure-stack-edge-deploy-add-shares#connect-to-an-smb-share) ve [video dosyasını](https://lvamedia.blob.core.windows.net/public/bulldozer.mkv) yerel paylaşıma kopyalayın.
    1. Rtspsim modülünün aşağıdakilere sahip olduğunu görün:
        
        ```json
        "createOptions": {
                            "HostConfig": {
                              "Mounts": [
                                {
                                  "Target": "/live/mediaServer/media",
                                  "Source": "<your Local Docker Volume Mount name>",
                                  "Type": "volume"
                                }
                              ],
                              "PortBindings": {
                                "554/tcp": [
                                  {
                                    "HostPort": "554"
                                  }
                                ]
                              }
                            }
                          }
        ```
## <a name="generate-and-deploy-the-deployment-manifest"></a>Dağıtım bildirimini oluşturma ve dağıtma

Dağıtım bildirimi, bir sınır cihazına hangi modüllerin dağıtıldığını tanımlar. Ayrıca, bu modüllerle ilgili yapılandırma ayarlarını tanımlar.

Şablon dosyasından bildirim oluşturmak ve ardından Edge cihazına dağıtmak için bu adımları izleyin.

1. Visual Studio Code’u açın.
1. AZURE ıOT HUB bölmesinin yanındaki IoT Hub bağlantı dizesini ayarlamak için diğer Eylemler simgesini seçin. Dizeyi src/buluttan cihazdan-Console-App/appsettings.jsdosyasından kopyalayabilirsiniz.

    > [!div class="mx-imgBorder"]
    > :::image type="content" source="./media/spatial-analysis-tutorial/connection-string.png" alt-text="Uzamsal analiz: bağlantı dizesi":::
1. Src/Edge/deployment.template.jsüzerinde sağ tıklayın ve IoT Edge dağıtım bildirimi oluştur ' u seçin.

    > [!div class="mx-imgBorder"]
    > :::image type="content" source="./media/spatial-analysis-tutorial/deployment-amd64-json.png" alt-text="Uzamsal analiz: dağıtım AMD64 JSON":::
    
    Bu eylem, src/Edge/config klasöründe deployment.amd64.jsadlı bir bildirim dosyası oluşturmalı.
1. Src/Edge/config/deployment.amd64.jsüzerinde sağ tıklayın, tek cihaz için dağıtım oluştur ' u seçin ve ardından Edge cihazınızın adını seçin.
    
    > [!div class="mx-imgBorder"]
    > :::image type="content" source="./media/spatial-analysis-tutorial/deployment-template-json.png" alt-text="Uzamsal analiz: dağıtım şablonu JSON":::   
1. IoT Hub bir cihaz seçmeniz istendiğinde, açılır menüden Azure Stack Edge adını seçin.
1. 30 saniye sonra, pencerenin sol alt köşesinde Azure IoT Hub ' yi yenileyin. Edge cihazında artık aşağıdaki dağıtılan modüller gösterilmektedir:
    
    * IoT Edge (modül adı lvaEdge) üzerinde canlı video analizi.
    * Gerçek zamanlı akış protokolü (RTSP) Simülatörü (modül adı rtspsım).
    * Uzamsal analiz (modül adı spatialAnalysis).
    
Başarıyla dağıtırsanız, çıktıda aşağıdakine benzer bir ileti olacaktır:

```
[Edge] Start deployment to device [<Azure Stack Edge name>]
[Edge] Deployment succeeded.
```

Sonra `lvaEdge` `rtspsim` `spatialAnalysis` `rtspsim` cihazlar/modüller altında, ve modüllerini bulabilir ve bunların durumu "çalışıyor" olmalıdır.

## <a name="prepare-to-monitor-events"></a>Olayları izlemeye hazırlanma

Bu olayları görmek için şu adımları izleyin:

1. Visual Studio Code Gezgin bölmesini açın ve sol alt köşedeki Azure IoT Hub arayın.
1. Cihazlar düğümünü genişletin.
1. Azure Stack Kenarıza sağ tıklayıp Izlemeyi Başlat yerleşik olay uç noktasını seçin.
    
    > [!div class="mx-imgBorder"]
    > :::image type="content" source="./media/spatial-analysis-tutorial/start-monitoring.png" alt-text="Uzamsal analiz: izlemeyi Başlat":::
     
## <a name="run-the-program"></a>Programı çalıştırma

Src/buluttan cihaza-Console-App/operations.jsüzerindeki doğrudan yöntemleri çağırabilecek bir program.cs vardır. operations.jskurulum ve medya Graf kullanımı için bir topoloji sağlaması gerekiyor.
operations.js:

Topolojiyi şu şekilde ayarlayın (yerel topoloji için topologyFile, çevrimiçi topoloji için topologyUrl):

```json
{
    "opName": "GraphTopologySet",
    "opParams": {
        "topologyFile": "../edge/spatialAnalysistopology.json"
    }
},
```

Bu şekilde bir grafik örneği oluşturun, topoloji içindeki parametreleri buradan ayarlayın:

```json
{
    "opName": "GraphInstanceSet",
    "opParams": {
        "name": "Sample-Graph-1",
        "properties": {
            "topologyName": "InferencingWithCVExtension",
            "description": "Sample graph description",
            "parameters": [
                {
                    "name": "rtspUrl",
                    "value": " rtsp://rtspsim:554/media/bulldozer.mkv"
                },
                {
                    "name": "rtspUserName",
                    "value": "testuser"
                },
                {
                    "name": "rtspPassword",
                    "value": "testpassword"
                }
            ]
        }
    }
},
```

<p>
<details>
<summary>SpatialAnalysis modülü için örnek topoloji dosyasını görmek için genişletin:
</summary>
<pre><code>
{
    "@apiVersion": "1.0",
    "name": "InferencingWithCVExtension",
    "properties": {
      "description": "Analyzing live video using spatialAnalysis Extension to send images to an external inference engine",
      "parameters": [
        {
          "name": "rtspUserName",
          "type": "String",
          "description": "rtsp source user name.",
          "default": "dummyUserName"
        },
        {
          "name": "rtspPassword",
          "type": "String",
          "description": "rtsp source password.",
          "default": "dummyPassword"
        },
        {
          "name": "rtspUrl",
          "type": "String",
          "description": "rtsp Url"
        },
        {
          "name": "grpcUrl",
          "type": "String",
          "description": "inferencing Url",
          "default": "tcp://spatialAnalysis:50051"
        },
        {
          "name": "frameRate",
          "type": "String",
          "description": "Rate of the frames per second to be received from LVA.",
          "default": "2"
        },
        {
          "name": "spatialanalysisusername",
          "type": "String",
          "description": "spatialanalysis endpoint username",
          "default": "not-in-use"
        },
        {
          "name": "spatialanalysispassword",
          "type": "String",
          "description": "spatialanalysis endpoint password",
          "default": "not-in-use"  
        }
      ],
      "sources": [
        {
          "@type": "#Microsoft.Media.MediaGraphRtspSource",
          "name": "rtspSource",
          "transport": "tcp",
          "endpoint": {
            "@type": "#Microsoft.Media.MediaGraphUnsecuredEndpoint",
            "url": "${rtspUrl}",
            "credentials": {
              "@type": "#Microsoft.Media.MediaGraphUsernamePasswordCredentials",
              "username": "${rtspUserName}",
              "password": "${rtspPassword}"
            }
          }
        }
      ],
      "processors": [
        {
          "@type": "#Microsoft.Media.MediaGraphFrameRateFilterProcessor",
          "name": "frameRateFilter",
          "inputs": [
            {
              "nodeName": "rtspSource"
            }
          ],
          "maximumFps": "${frameRate}"
        },
        {
          "@type": "#Microsoft.Media.MediaGraphCognitiveServicesVisionExtension",
          "name": "computerVisionExtension",
          "endpoint": {
            "@type": "#Microsoft.Media.MediaGraphUnsecuredEndpoint",
            "url": "${grpcUrl}",
            "credentials": {
              "@type": "#Microsoft.Media.MediaGraphUsernamePasswordCredentials",
              "username": "${spatialanalysisusername}",
              "password": "${spatialanalysispassword}"
            }
          },
          "image": {
            "scale": {
              "mode": "pad",
              "width": "1408",
              "height": "786"
            },
            "format": {
              "@type": "#Microsoft.Media.MediaGraphImageFormatRaw",
              "pixelFormat": "bgr24"
            }
          },
          "inputs": [
            {
              "nodeName": "frameRateFilter"
            }
          ]
        },
        {
            "@type": "#Microsoft.Media.MediaGraphSignalGateProcessor",
            "name": "signalGateProcessor",
            "inputs": [
              {
                "nodeName": "computerVisionExtension"
              },
              {
                "nodeName": "rtspSource"
              }
            ],
            "activationEvaluationWindow": "PT1S",
            "activationSignalOffset": "PT0S",
            "minimumActivationTime": "PT30S",
            "maximumActivationTime": "PT30S"
          }
      ],
      "sinks": [
        {
            "@type": "#Microsoft.Media.MediaGraphAssetSink",
            "name": "assetSink",
            "assetNamePattern": "sampleAssetFromEVR-CV-LVAEdge-${System.DateTime}",
            "segmentLength": "PT30S",
            "LocalMediaCacheMaximumSizeMiB": "200",
            "localMediaCachePath": "/var/lib/azuremediaservices/tmp/",
            "inputs": [
                {
                    "nodeName": "signalGateProcessor"
                }
            ]
        },
        {
          "@type": "#Microsoft.Media.MediaGraphIoTHubMessageSink",
          "name": "hubSink",
          "hubOutputName": "inferenceOutput",
          "inputs": [
            {
              "nodeName": "computerVisionExtension"
            }
          ]
        }
      ]
    }
  }
</code>
</pre>
</details>
</p>


MediaGraphRealTimeComputerVisionExtension kullanarak uzamsal analiz modülüyle bağlantı yapın. Tcp://spatialAnalysis: <PORT_NUMBER> $ {Grpkıvrı} ayarlayın, ör. tcp://spatialAnalysis:50051

```json
{
    "@type": "#Microsoft.Media.MediaGraphCognitiveServicesVisionExtension",
    "name": "computerVisionExtension",
    "endpoint": {
    "@type": "#Microsoft.Media.MediaGraphUnsecuredEndpoint",
    "url": "${grpcUrl}",
    "credentials": {
        "@type": "#Microsoft.Media.MediaGraphUsernamePasswordCredentials",
        "username": "${spatialanalysisusername}",
        "password": "${spatialanalysispassword}"
    }
    },
    "image": {
    "scale": {
        "mode": "pad",
        "width": "1408",
        "height": "786"
    },
    "format": {
        "@type": "#Microsoft.Media.MediaGraphImageFormatRaw",
        "pixelFormat": "bgr24"
    }
    },
    "inputs": [
    {
        "nodeName": "frameRateFilter"
    }
    ]
}
```

Bir hata ayıklama oturumu çalıştırın ve TERMINAL yönergelerini izleyin, topoloji ayarlar, grafik örneğini ayarlar, grafik örneğini etkinleştirir ve son olarak kaynakları siler.

## <a name="interpret-results"></a>Sonuçları yorumlama

Bir medya grafiği örneği oluşturulduğunda, "Mediasessionkurulduğu" olayını, burada bir [örnek Mediasessionkurulu olayını](detect-motion-emit-events-quickstart.md#mediasessionestablished-event)görmeniz gerekir.

Uzamsal analiz modülü Ayrıca, canlı video analizlerine AI Insight olayları gönderir ve ardından ıothub ' e, çıktıda da gösterilir. VARLıK, algılama nesneleri ve olay spaceanalytics olaylardır. Bu çıktı, canlı video analizine geçirilecek.

PersonZoneEvent için örnek çıkış (biliveservices. Vision. spatialanalysis-personcrossingçokgen. livevideoanalytics işlemi):

```
{
  "body": {
    "timestamp": 143810610210090,
    "inferences": [
      {
        "type": "entity",
        "subtype": "",
        "inferenceId": "a895c86824db41a898f776da1876d230",
        "relatedInferences": [],
        "entity": {
          "tag": {
            "value": "person",
            "confidence": 0.66026187
          },
          "attributes": [],
          "box": {
            "l": 0.26559368,
            "t": 0.17887735,
            "w": 0.49247605,
            "h": 0.76629657
          }
        },
        "extensions": {},
        "valueCase": "entity"
      },
      {
        "type": "event",
        "subtype": "",
        "inferenceId": "995fe4206847467f8dfde83a15187d76",
        "relatedInferences": [
          "a895c86824db41a898f776da1876d230"
        ],
        "event": {
          "name": "personZoneEvent",
          "properties": {
            "status": "Enter",
            "metadataVersion": "1.0",
            "zone": "polygon0",
            "trackingId": "a895c86824db41a898f776da1876d230"
          }
        },
        "extensions": {},
        "valueCase": "event"
      }
    ]
  },
  "applicationProperties": {
    "topic": "/subscriptions/81631a63-f0bd-4f35-8344-5bc541b36bfc/resourceGroups/lva-sample-resources/providers/microsoft.media/mediaservices/lvasamplea4bylcwoqqypi",
    "subject": "/graphInstances/Sample-Graph-1/processors/spatialanalysisExtension",
    "eventType": "Microsoft.Media.Graph.Analytics.Inference",
    "eventTime": "2020-08-20T03:54:29.001Z",
    "dataVersion": "1.0"
  }
}
```

## <a name="next-steps"></a>Sonraki adımlar

`spatialAnalysis`Dağıtım bildirimi dosyanızın grafik düğümündeki "etkin" bayrağını değiştirerek, modülün, **PersonCount** ve **persondistance** gibi sunduğu farklı işlemleri deneyin.
>[!Tip]
> Çerçevede birden fazla kişi olan bir [video dosyası](https://lvamedia.blob.core.windows.net/public/2018-03-07.16-50-00.16-55-00.school.G421.mkv) kullanın.

> [!NOTE]
> Tek seferde yalnızca bir işlem çalıştırabilirsiniz. Bu nedenle, lütfen yalnızca bir bayrağın **true** , diğerleri **ise false**olarak ayarlandığından emin olun.
