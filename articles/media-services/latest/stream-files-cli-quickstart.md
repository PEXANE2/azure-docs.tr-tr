---
title: Azure Medya Hizmetleri ve Azure CLI ile video dosyalarını akışla
description: Yeni bir Azure Medya Hizmetleri hesabı oluşturmak, bir dosyayı kodlamak ve Azure Media Player'a aktarmak için bu öğreticinin adımlarını izleyin.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
keywords: azure media services, akış
ms.service: media-services
ms.workload: media
ms.topic: tutorial
ms.custom: ''
ms.date: 08/19/2019
ms.author: juliako
ms.openlocfilehash: 91259e10966173cb701b867f5b3ed362112beef3
ms.sourcegitcommit: e040ab443f10e975954d41def759b1e9d96cdade
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2020
ms.locfileid: "80382792"
---
# <a name="tutorial-encode-a-remote-file-based-on-url-and-stream-the-video---azure-cli"></a>Öğretici: URL'ye dayalı uzak bir dosyayı kodlayın ve videoyu akışlayın - Azure CLI

Bu öğretici, Azure Medya Hizmetleri ve Azure CLI'yi kullanarak çeşitli tarayıcılarda ve cihazlarda videoları kolayca nasıl kodlayıp akışınizi gösterir. Giriş içeriğini, HTTPS veya SAS URL'lerini veya Azure Blob depolama sundaki dosyalara giden yolları kullanarak belirtebilirsiniz.

Bu makaledeki örnek, https URL üzerinden erişilebilen içerikleri kodlar. Media Services v3 şu anda HTTPS URL'leri üzerinden ödenebilir aktarım kodlamayı desteklemiyor.

Bu eğitimin sonunda, bir video akışı mümkün olacak.  

![Videoyu yürütme](./media/stream-files-dotnet-quickstart/final-video.png)

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="create-a-media-services-account"></a>Media Services hesabı oluşturma

Azure'da medya içeriğini şifrelemek, kodlamak, analiz etmek, yönetmek ve akışa aktarmak için önce bir Medya Hizmetleri hesabı oluşturmanız gerekir. Bu hesap bir veya daha fazla depolama hesabıyla ilişkilendirilmelidir.

Medya Hizmetleri hesabınız ve ilişkili tüm depolama hesapları aynı Azure aboneliğinde olmalıdır. Gecikme ve veri çıkış maliyetlerini sınırlamak için Medya Hizmetleri hesabıyla aynı yerde bulunan depolama hesaplarını kullanmanızı öneririz.

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

```azurecli-interactive
az group create -n amsResourceGroup -l westus2
```

### <a name="create-an-azure-storage-account"></a>Azure Storage hesabı oluşturma

Bu örnekte, genel amaçlı bir v2 Standart LRS hesabı oluştururuz.

Depolama hesaplarıyla denemeler yapmak istiyorsanız, 'yi kullanın. `--sku Standard_LRS` Üretim için bir SKU seçerken, `--sku Standard_RAGRS`iş sürekliliği için coğrafi çoğaltma sağlayan , kullanmayı düşünün. Daha fazla bilgi için [depolama hesaplarına](/cli/azure/storage/account)bakın.

```azurecli-interactive
az storage account create -n amsstorageaccount --kind StorageV2 --sku Standard_LRS -l westus2 -g amsResourceGroup
```

### <a name="create-an-azure-media-services-account"></a>Azure Media Services hesabı oluşturma

```azurecli-interactive
az ams account create --n amsaccount -g amsResourceGroup --storage-account amsstorageaccount -l westus2
```

Böyle bir yanıt alırsınız:

```
{
  "id": "/subscriptions/<id>/resourceGroups/amsResourceGroup/providers/Microsoft.Media/mediaservices/amsaccount",
  "location": "West US 2",
  "mediaServiceId": "8b569c2e-d648-4fcb-9035-c7fcc3aa7ddf",
  "name": "amsaccount",
  "resourceGroup": "amsResourceGroupTest",
  "storageAccounts": [
    {
      "id": "/subscriptions/<id>/resourceGroups/amsResourceGroup/providers/Microsoft.Storage/storageAccounts/amsstorageaccount",
      "resourceGroup": "amsResourceGroupTest",
      "type": "Primary"
    }
  ],
  "tags": null,
  "type": "Microsoft.Media/mediaservices"
}
```

## <a name="start-the-streaming-endpoint"></a>Akış uç noktasını başlatma

Aşağıdaki Azure CLI komutu varsayılan **Akış Bitiş Noktası'nı**başlatır.

```azurecli-interactive
az ams streaming-endpoint start  -n default -a amsaccount -g amsResourceGroup
```

Böyle bir yanıt alırsınız:

```
{
  "accessControl": null,
  "availabilitySetName": null,
  "cdnEnabled": true,
  "cdnProfile": "AzureMediaStreamingPlatformCdnProfile-StandardVerizon",
  "cdnProvider": "StandardVerizon",
  "created": "2019-02-06T21:58:03.604954+00:00",
  "crossSiteAccessPolicies": null,
  "customHostNames": [],
  "description": "",
  "freeTrialEndTime": "2019-02-21T22:05:31.277936+00:00",
  "hostName": "amsaccount-usw22.streaming.media.azure.net",
  "id": "/subscriptions/<id>/resourceGroups/amsResourceGroup/providers/Microsoft.Media/mediaservices/amsaccount/streamingendpoints/default",
  "lastModified": "2019-02-06T21:58:03.604954+00:00",
  "location": "West US 2",
  "maxCacheAge": null,
  "name": "default",
  "provisioningState": "Succeeded",
  "resourceGroup": "amsResourceGroup",
  "resourceState": "Running",
  "scaleUnits": 0,
  "tags": {},
  "type": "Microsoft.Media/mediaservices/streamingEndpoints"
}
```

Akış bitiş noktası zaten çalışıyorsa, şu iletiyi alırsınız:

```
(InvalidOperation) The server cannot execute the operation in its current state.
```

## <a name="create-a-transform-for-adaptive-bitrate-encoding"></a>Uyarlanabilir bithızı kodlaması için dönüşüm oluşturma

Videoları kodlamak veya çözümlemek için ortak görevleri yapılandırmak için **Dönüşüm** oluşturun. Bu örnekte, uyarlanabilir bitrate kodlama yapmak. Daha sonra yarattığımız dönüşüm altında bir iş göndeririz. İş, verilen video veya ses içeriği girişine dönüşümü uygulamak için Medya Hizmetleri'ne yapılan istektir.

```azurecli-interactive
az ams transform create --name testEncodingTransform --preset AdaptiveStreaming --description 'a simple Transform for Adaptive Bitrate Encoding' -g amsResourceGroup -a amsaccount
```

Böyle bir yanıt alırsınız:

```
{
  "created": "2019-02-15T00:11:18.506019+00:00",
  "description": "a simple Transform for Adaptive Bitrate Encoding",
  "id": "/subscriptions/<id>/resourceGroups/amsResourceGroup/providers/Microsoft.Media/mediaservices/amsaccount/transforms/testEncodingTransform",
  "lastModified": "2019-02-15T00:11:18.506019+00:00",
  "name": "testEncodingTransform",
  "outputs": [
    {
      "onError": "StopProcessingJob",
      "preset": {
        "odatatype": "#Microsoft.Media.BuiltInStandardEncoderPreset",
        "presetName": "AdaptiveStreaming"
      },
      "relativePriority": "Normal"
    }
  ],
  "resourceGroup": "amsResourceGroup",
  "type": "Microsoft.Media/mediaservices/transforms"
}
```

## <a name="create-an-output-asset"></a>Çıktı varlığı oluşturma

Kodlama işinin çıktısı olarak kullanılacak bir çıktı **Varlığı** oluşturun.

```azurecli-interactive
az ams asset create -n testOutputAssetName -a amsaccount -g amsResourceGroup
```

Böyle bir yanıt alırsınız:

```
{
  "alternateId": null,
  "assetId": "96427438-bbce-4a74-ba91-e38179b72f36",
  "container": null,
  "created": "2019-02-14T23:58:19.127000+00:00",
  "description": null,
  "id": "/subscriptions/<id>/resourceGroups/amsResourceGroup/providers/Microsoft.Media/mediaservices/amsaccount/assets/testOutputAssetName",
  "lastModified": "2019-02-14T23:58:19.127000+00:00",
  "name": "testOutputAssetName",
  "resourceGroup": "amsResourceGroup",
  "storageAccountName": "amsstorageaccount",
  "storageEncryptionFormat": "None",
  "type": "Microsoft.Media/mediaservices/assets"
}
```

## <a name="start-a-job-by-using-https-input"></a>HTTPS girişini kullanarak işe başlama

Videoları işlemek için iş gönderdiğinde, Medya Hizmetleri'ne giriş videosunu nerede bulacağını söylemeniz gerekir. Bir seçenek, bu örnekte gösterildiği gibi, iş girişi olarak bir HTTPS URL belirtmektir.

Çalıştırdığınızda, `az ams job start`işin çıktısı üzerinde bir etiket ayarlayabilirsiniz. Daha sonra çıktı varlığının ne için olduğunu belirlemek için etiketi kullanabilirsiniz.

- Etikete bir değer atarsanız, '--çıktı-varlıklar'ı "assetname=label" olarak ayarlayın.
- Etikete bir değer atadıysanız, '--çıktı-varlıklar'ı "assetname=" olarak ayarlayın.

  "=" eklediğimize dikkat `output-assets`edin.

```azurecli-interactive
az ams job start --name testJob001 --transform-name testEncodingTransform --base-uri 'https://nimbuscdn-nimbuspm.streaming.mediaservices.windows.net/2b533311-b215-4409-80af-529c3e853622/' --files 'Ignite-short.mp4' --output-assets testOutputAssetName= -a amsaccount -g amsResourceGroup
```

Böyle bir yanıt alırsınız:

```
{
  "correlationData": {},
  "created": "2019-02-15T05:08:26.266104+00:00",
  "description": null,
  "id": "/subscriptions/<id>/resourceGroups/amsResourceGroup/providers/Microsoft.Media/mediaservices/amsaccount/transforms/testEncodingTransform/jobs/testJob001",
  "input": {
    "baseUri": "https://nimbuscdn-nimbuspm.streaming.mediaservices.windows.net/2b533311-b215-4409-80af-529c3e853622/",
    "files": [
      "Ignite-short.mp4"
    ],
    "label": null,
    "odatatype": "#Microsoft.Media.JobInputHttp"
  },
  "lastModified": "2019-02-15T05:08:26.266104+00:00",
  "name": "testJob001",
  "outputs": [
    {
      "assetName": "testOutputAssetName",
      "error": null,
      "label": "",
      "odatatype": "#Microsoft.Media.JobOutputAsset",
      "progress": 0,
      "state": "Queued"
    }
  ],
  "priority": "Normal",
  "resourceGroup": "amsResourceGroup",
  "state": "Queued",
  "type": "Microsoft.Media/mediaservices/transforms/jobs"
}
```

### <a name="check-status"></a>Durumu kontrol etme

Beş dakika içinde, işin durumunu kontrol edin. "Bitti" olmalı. Daha bitmedi, birkaç dakika içinde tekrar kontrol et. Bittiğinde, bir sonraki adıma gidin ve bir **Akış Bulucu**oluşturun.

```azurecli-interactive
az ams job show -a amsaccount -g amsResourceGroup -t testEncodingTransform -n testJob001
```

## <a name="create-a-streaming-locator-and-get-a-path"></a>Akış bulucu oluşturma ve yol alma

Kodlama tamamlandıktan sonra, bir sonraki adım, çıktı varlığındaki videoyu oynatma kalım için istemcilerin kullanımına açmaktır. Bunu yapmak için önce bir Akış Bulucu oluşturun. Ardından, istemcilerin kullanabileceği akış URL'leri oluşturun.

### <a name="create-a-streaming-locator"></a>Akış bulucusu oluşturma

```azurecli-interactive
az ams streaming-locator create -n testStreamingLocator --asset-name testOutputAssetName --streaming-policy-name Predefined_ClearStreamingOnly  -g amsResourceGroup -a amsaccount 
```

Böyle bir yanıt alırsınız:

```
{
  "alternativeMediaId": null,
  "assetName": "output-3b6d7b1dffe9419fa104b952f7f6ab76",
  "contentKeys": [],
  "created": "2019-02-15T04:35:46.270750+00:00",
  "defaultContentKeyPolicyName": null,
  "endTime": "9999-12-31T23:59:59.999999+00:00",
  "id": "/subscriptions/<id>/resourceGroups/amsResourceGroup/providers/Microsoft.Media/mediaservices/amsaccount/streamingLocators/testStreamingLocator",
  "name": "testStreamingLocator",
  "resourceGroup": "amsResourceGroup",
  "startTime": null,
  "streamingLocatorId": "e01b2be1-5ea4-42ca-ae5d-7fe704a5962f",
  "streamingPolicyName": "Predefined_ClearStreamingOnly",
  "type": "Microsoft.Media/mediaservices/streamingLocators"
}
```

### <a name="get-streaming-locator-paths"></a>Akış bulucu yollarını alın

```azurecli-interactive
az ams streaming-locator get-paths -a amsaccount -g amsResourceGroup -n testStreamingLocator
```

Böyle bir yanıt alırsınız:

```
{
  "downloadPaths": [],
  "streamingPaths": [
    {
      "encryptionScheme": "NoEncryption",
      "paths": [
        "/e01b2be1-5ea4-42ca-ae5d-7fe704a5962f/ignite.ism/manifest(format=m3u8-aapl)"
      ],
      "streamingProtocol": "Hls"
    },
    {
      "encryptionScheme": "NoEncryption",
      "paths": [
        "/e01b2be1-5ea4-42ca-ae5d-7fe704a5962f/ignite.ism/manifest(format=mpd-time-csf)"
      ],
      "streamingProtocol": "Dash"
    },
    {
      "encryptionScheme": "NoEncryption",
      "paths": [
        "/e01b2be1-5ea4-42ca-ae5d-7fe704a5962f/ignite.ism/manifest"
      ],
      "streamingProtocol": "SmoothStreaming"
    }
  ]
}
```

HTTP canlı akış (HLS) yolunu kopyalayın. Bu durumda, `/e01b2be1-5ea4-42ca-ae5d-7fe704a5962f/ignite.ism/manifest(format=m3u8-aapl)`bu.

## <a name="build-the-url"></a>URL'yi oluşturun

### <a name="get-the-streaming-endpoint-host-name"></a>Akış uç nokta ana bilgisayar adını alma

```azurecli-interactive
az ams streaming-endpoint list -a amsaccount -g amsResourceGroup -n default
```

Değeri `hostName` kopyalayın. Bu durumda, `amsaccount-usw22.streaming.media.azure.net`bu.

### <a name="assemble-the-url"></a>URL'yi birleştirin

"https:// " &lt;+&gt; + &lt;hostName değeri Hls yol değeri&gt;

Bir örneği aşağıda verilmiştir:

`https://amsaccount-usw22.streaming.media.azure.net/7f19e783-927b-4e0a-a1c0-8a140c49856c/ignite.ism/manifest(format=m3u8-aapl)`

## <a name="test-playback-by-using-azure-media-player"></a>Azure Media Player'ı kullanarak oynatmayı test edin

> [!NOTE]
> Bir oyuncu bir HTTPS sitesinde barındırılırsa, URL'yi "https" ile başlattığınızdan emin olun.

1. Bir web tarayıcısı [https://aka.ms/azuremediaplayer/](https://aka.ms/azuremediaplayer/)açın ve .
2. **URL** kutusuna, önceki bölümde oluşturduğunuz URL'yi yapıştırın. URL'yi HLS, Dash veya Düzgün biçiminde yapıştırabilirsiniz. Azure Media Player, cihazınızda oynatma için uygun bir akış protokolünü otomatik olarak kullanır.
3. **Güncelle Player'ı**seçin.

>[!NOTE]
>Azure Media Player, test için kullanılabilir, ancak üretim ortamında kullanılmamalıdır.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğretici için oluşturduğunuz Medya Hizmetleri ve depolama hesapları da dahil olmak üzere kaynak grubunuzdaki kaynaklardan herhangi biri ne olursa olsun, kaynak grubunu silin.

Bu Azure CLI komutunu çalıştırın:

```azurecli-interactive
az group delete --name amsResourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

[Medya Hizmetlerine genel bakış](media-services-overview.md)
