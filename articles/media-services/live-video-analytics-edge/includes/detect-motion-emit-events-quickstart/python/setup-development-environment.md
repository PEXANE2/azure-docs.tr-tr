---
ms.openlocfilehash: be983a643b45847d2a44db018b1383aa56ab2302
ms.sourcegitcommit: 56cbd6d97cb52e61ceb6d3894abe1977713354d9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2020
ms.locfileid: "88691199"
---
1. Depoyu bu konumdan kopyala: https://github.com/Azure-Samples/live-video-analytics-iot-edge-python .
1. Visual Studio Code, deponun indirildiği klasörü açın.
1. Visual Studio Code, *src/buluttan cihaza-Console-App* klasörüne gidin. Burada bir dosya oluşturun ve *appsettings.js*. Bu dosya, programı çalıştırmak için gereken ayarları içerir.
1. Bu hızlı başlangıçta oluşturduğunuz dosyadaki *~/CloudDrive/LVA-Sample/appsettings.js* içeriğini kopyalayın.

    Metin aşağıdaki çıktıya benzer şekilde görünmelidir.

    ```
    {  
        "IoThubConnectionString" : "HostName=xxx.azure-devices.net;SharedAccessKeyName=iothubowner;SharedAccessKey=XXX",  
        "deviceId" : "lva-sample-device",  
        "moduleId" : "lvaEdge"  
    }
    ```
1. *Src/Edge* klasörüne gidin ve *. env*adlı bir dosya oluşturun.
1. */CloudDrive/LVA-Sample/Edge-Deployment/-env* dosyasının içeriğini kopyalayın. Metin aşağıdaki kod gibi görünmelidir.

    ```
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
    