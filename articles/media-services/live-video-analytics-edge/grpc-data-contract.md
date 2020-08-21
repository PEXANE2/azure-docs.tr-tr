---
title: gRPC uzantısı veri sözleşmesi-Azure
description: Bu makalede, canlı video analizi modülü ile AI veya CV özel uzantısı arasında ileti göndermek için gRPC protokolünü kullanma hakkında bilgi edineceksiniz.
ms.topic: overview
ms.date: 09/14/2020
ms.openlocfilehash: 0221d20245a6db69791d8bf13ba9e00de3b96ecc
ms.sourcegitcommit: 56cbd6d97cb52e61ceb6d3894abe1977713354d9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2020
ms.locfileid: "88691881"
---
# <a name="grpc-extension-data-contract"></a>gRPC uzantısı veri sözleşmesi

Bu makalede, canlı video analizi modülü ile AI veya CV özel uzantısı arasında ileti göndermek için gRPC protokolünü kullanma hakkında bilgi edineceksiniz.

gRPC, herhangi bir ortamda çalışan modern, açık kaynaklı ve yüksek performanslı bir RPC çerçevesidir. GRPC aktarma hizmeti, aralarında HTTP/2 çift yönlü akış kullanır:

* gRPC istemcisi (IoT Edge modülünde canlı video analizi) ve 
* gRPC sunucusu (özel uzantınız).

GRPC oturumu, gRPC istemcisinden TCP/TLS bağlantı noktası üzerinden gRPC sunucusuna tek bağlantıdır. 

Tek bir oturumda: istemci, gRPC akış oturumu üzerinde [prototipsiz](https://developers.google.com/protocol-buffers) bir ileti olarak bir medya akış tanımlayıcısı ve ardından sunucuya video çerçeveleri gönderir. Sunucu akış tanımlayıcısını doğrular, video çerçevesini analiz edin ve çıkarım sonuçlarını bir prototipme iletisi olarak döndürür.

![gRPC uzantı sözleşmesi](./media/data-contracts/grpc.png)

## <a name="implementing-grpc-protocol"></a>GRPC protokolünü uygulama

### <a name="creating-a-grpc-connection"></a>GRPC bağlantısı oluşturma

Özel uzantının aşağıdaki gRPC hizmetini uygulaması gerekir:

```
service MediaGraphExtension {
  rpc ProcessMediaStream(stream MediaStreamMessage) returns (stream MediaStreamMessage);
}
```

Çağrıldığında, gRPC uzantısı ve canlı video analizi grafiği arasında akışa almak üzere çift yönlü bir akış açar. Her taraf tarafından Bu akışta gönderilen ilk ileti, aşağıdaki MediaSamples ' de hangi bilgilerin gönderileceğini tanımlayan bir MediaStreamDescriptor içerir.

Örneğin, Graph uzantısı, gRPC iletilerinde özel uzantıya gömülü 416x416 Rgb24 kodlu çerçeveler gönderemeyeceğini belirtmek için iletiyi (JSON 'da ifade edilir) gönderebilir.

```
 {
    "sequence_number": 1,
    "ack_sequence_number": 0,
    "media_stream_descriptor": {
        "graph_identifier": {
            "media_services_arm_id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/resourceGroupName/providers/microsoft.media/mediaservices/mediaAccountName",
            "graph_instance_name": "mediaGraphName",
            "graph_node_name": "grpcExtension"
        },
        "media_descriptor": {
            "timescale": 90000,
            "video_frame_sample_format": {
                "encoding": "RAW",
                "pixel_format": "RGB24",
                "dimensions": {
                    "width": 416,
                    "height": 416
                },
                "stride_bytes": 1248
            }
        }
    }
}
```

Özel uzantı yanıt olarak, yanıt olarak, yalnızca döndürmekte olduğunu göstermek için aşağıdaki iletiyi gönderir.

```
{
    "sequence_number": 1,
    "ack_sequence_number": 1,
    "media_stream_descriptor": {
        "extension_identifier": "customExtensionName"    }
}
```

Her iki taraf da medya tanımlayıcılarını değiş tokuş eden canlı video analizi, çerçeveleri uzantıya iletmeye başlayacaktır.

### <a name="sequence-numbers"></a>Dizi numaraları

Hem gRPC uzantı düğümü hem de özel uzantı, iletilerine atanan ayrı bir dizi numara kümesi tutar. Bu sıra numaraları, 1 ' den başlayarak tek parçalı artışlara sahip olmalıdır. `ack_sequence_number` ileti onaylanmazsa, ilk ileti gönderildiğinde ortaya çıkabilecek bir ileti yok sayılabilir.

Karşılık gelen ileti tam olarak işlendikten sonra bir istek onaylanmalıdır. Paylaşılan bellek aktarımı söz konusu olduğunda, bu bildirim gönderenin paylaşılan belleği serbest bırakabilir ve alıcının buna artık erişemeyeceğini gösterir. Gönderenin, kabul edilene kadar paylaşılan belleği tutması gerekir.

### <a name="reading-embedded-content"></a>Katıştırılmış içerik okunuyor

Katıştırılmış içerik `MediaSample` , s alanı aracılığıyla doğrudan iletiden okunabilir `content_byte` . Veriler öğesine göre kodlanacak `MediaDescriptor` .

### <a name="reading-content-from-shared-memory"></a>Paylaşılan bellekten Içerik okuma

İçerik, paylaşılan bellek üzerinden aktarılırken, `SharedMemoryBufferTransferProperties` `MediaStreamDescriptor` ilk olarak bir oturum açtıklarında gönderici dahil edilir. Bu, aşağıdaki gibi görünebilir (JSON ile ifade edilir):

```
{
  "handle_name": "inference_client_share_memory_2146989006636459346"
  "length_bytes": 20971520
}
```

Alıcı daha sonra dosyayı açar `/dev/shm/inference_client_share_memory_2146989006636459346` . Gönderen `MediaSample` , `ContentReference` Bu dosyadaki belirli bir yere başvuran bir ileti gönderir.

```
{
    "timestamp": 143598615750000,
    "content_reference": {
        "address_offset": 519168,
        "length_bytes": 173056
    }
}
```

Daha sonra alıcı, dosyadaki bu konumdaki verileri okur.

Canlı video analizi kapsayıcısının paylaşılan bellek üzerinden iletişim kurması için, kapsayıcının IPC modunun doğru şekilde yapılandırılması gerekir. Bu pek çok şekilde yapılabilir, ancak önerilen bazı konfigürasyonlar aşağıda verilmiştir.

* Konak cihazda çalışan bir gRPC ınsırlı altyapısı ile iletişim kurarken, IPC modu ana bilgisayar olarak ayarlanmalıdır.
* Başka bir IoT Edge modülünde çalışan bir gRPC sunucusuyla iletişim kurarken, IPC modunun canlı video analizi modülü ve özel uzantı için paylaşılabilir olarak ayarlanması gerekir `container:liveVideoAnalytics` ; burada, `liveVideoAnalytics` canlı video analizi modülünün adıdır.

Bu, yukarıda bulunan ilk seçenek kullanılarak Device ikizi 'ta nasıl görünebileceğini aşağıda bulabilirsiniz.

```
"liveVideoAnalytics": {
  "version": "1.0",
  "type": "docker",
  "status": "running",
  "restartPolicy": "always",
  "settings": {
    "image": "mcr.microsoft.com/media/live-video-analytics:1",
    "createOptions": 
      "HostConfig": {
        "IpcMode": "host"
      }
    }
  }
}
```

IPC modları hakkında daha fazla bilgi için bkz https://docs.docker.com/engine/reference/run/#ipc-settings---ipc ..

## <a name="media-graph-grpc-extension-contract-definitions"></a>Medya grafiği gRPC uzantısı sözleşme tanımları

Bu bölümde, veri akışını tanımlayan gRPC sözleşmesi tanımlanmaktadır.

### <a name="protocol-messages"></a>Protokol iletileri

![Protokol iletileri](./media/data-contracts/grpc2.png)
 
### <a name="client-authentication"></a>İstemci kimlik doğrulaması

Özel uzantıların uygulayıcıları, gRPC uzantısı düğümünden geldiğinden emin olmak için gelen gRPC bağlantılarının orijinalliğini doğrulayabilir. Düğüm, doğrulama için istek üst bilgilerinde bir giriş sağlar.

Bu, bunu gerçekleştirmek için Kullanıcı adı/parola kimlik bilgileri kullanılabilir. GRPC uzantı düğümü oluştururken, kimlik bilgileri aşağıdaki gibi sağlanır:

```
{
  "@type": "#Microsoft.Media.MediaGraphGrpcExtension",
  "name": "{moduleIdentifier}",
  "endpoint": {
    "@type": "#Microsoft.Media.MediaGraphUnsecuredEndpoint",
    "url": "tcp://customExtension:8081",
    "credentials": {
      "@type": "#Microsoft.Media.MediaGraphUsernamePasswordCredentials",
      "username": "username",
      "password": "password"
    }
  }
  // Other fields omitted
}
```

GRPC isteği gönderildiğinde, aşağıdaki üst bilgi istek meta verilerinde yer alır, HTTP temel kimlik doğrulaması da bu şekilde görünür.

`x-ms-authentication: Basic (Base64 Encoded username:password)`

## <a name="using-grpc-over-tls"></a>TLS üzerinden gRPC kullanma

Inconnectıon için kullanılan bir gRPC bağlantısı TLS üzerinden güvenli hale getirilmiş olabilir. Bu, canlı video analizi ve ınırm motoru arasındaki ağ güvenliğinin garanti edilemez olduğu durumlarda faydalıdır. TLS, gRPC iletilerine katıştırılmış olan tüm içerikleri şifreleyerek, daha yüksek bir hızda çerçeveler aktarırken ek CPU ek yüküne neden olur.

Ignorehostname ve ıgnoresignature doğrulama seçenekleri gRPC tarafından desteklenmez. bu nedenle, ınsırlama altyapısının sunduğu sunucu sertifikası, gRPC uzantısı düğümünün Endpoint URL 'sindeki IP adresi/ana bilgisayar adı ile tam olarak eşleşen bir CN içermelidir.

## <a name="next-steps"></a>Sonraki adımlar

[Çıkarım meta verileri şeması hakkında bilgi edinin](inference-metadata-schema.md)
