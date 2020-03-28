---
title: Azure Media Services v3 kullanarak uzak bir dosyayı kodlayın ve akış
description: Bir URL'ye dayalı bir dosyayı kodlamak ve REST'i kullanarak Azure Medya Hizmetleri ile içeriğinizi aktarmak için bu öğreticinin adımlarını izleyin.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: tutorial
ms.custom: mvc
ms.date: 03/16/2020
ms.author: juliako
ms.openlocfilehash: 35be4ec2c4f5f8c299120c0ba7dbdcb1dd112473
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79472042"
---
# <a name="tutorial-encode-a-remote-file-based-on-url-and-stream-the-video---rest"></a>Öğretici: URL'ye dayalı uzak bir dosyayı kodlayın ve videoyu akışı - REST

Azure Medya Hizmetleri, medya dosyalarınızı çok çeşitli tarayıcılarda ve aygıtlarda oynatılabilen biçimlerde kodlamanıza olanak tanır. Örneğin, içeriğinizi Apple'ın HLS veya MPEG DASH biçimlerinde akışla göndermek isteyebilirsiniz. Akışla göndermeden önce yüksek kaliteli dijital medya dosyanızı kodlamanız gerekir. Kodlama yönergeleri için bkz. [Kodlama kavramı](encoding-concept.md).

Bu öğretici, bir URL'ye dayalı bir dosyayı nasıl kodladığınızı ve REST'i kullanarak Azure Medya Hizmetleri ile videoyu nasıl aktarabileceğinizi gösterir. 

![Videoyu yürütme](./media/stream-files-tutorial-with-api/final-video.png)

Bu öğretici şunların nasıl yapıldığını gösterir:    

> [!div class="checklist"]
> * Media Services hesabı oluşturma
> * Media Services API’sine erişim
> * Postman dosyalarını indirme
> * Postman'i yapılandırma
> * Postman kullanarak istek gönderme
> * Akış URL’sini test etme
> * Kaynakları temizleme

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Ön koşullar

- [Bir Medya Hizmetleri hesabı oluşturun.](create-account-cli-how-to.md)

    Kaynak grubu adı ve Medya Hizmetleri hesap adı için kullandığınız değerleri hatırladığınızdan emin olun

- AMS REST öğreticilerinden bazılarında gösterilen REST API'lerini yürütmek için [Postman](https://www.getpostman.com/) REST istemcisini yükleyin. 

    Biz **Postman**'ı kullanıyoruz, ancak herhangi bir REST aracı da olabilir. Diğer seçenekler şunlardır: REST eklentili **Visual Studio Code** veya **Telerik Fiddler**. 

## <a name="download-postman-files"></a>Postman dosyalarını indirme

Postman koleksiyonunu ve ortam dosyalarını içeren bir GitHub deposunu kopyalayın.

 ```bash
 git clone https://github.com/Azure-Samples/media-services-v3-rest-postman.git
 ```

## <a name="access-api"></a>Erişim API’si

Ayrıntılı bilgi için [Bkz. Medya Hizmetleri API'sine erişmek için kimlik bilgileri alın](access-api-howto.md)

## <a name="configure-postman"></a>Postman'i yapılandırma

### <a name="configure-the-environment"></a>Ortamı yapılandırma 

1. **Postacı** uygulamasını açın.
2. Ekranın sağ tarafında **Ortamı yönet** seçeneğini belirleyin.

    ![Ortamı yönetme](./media/develop-with-postman/postman-import-env.png)
4. **Ortamı yönet** iletişim kutusunda **İçe aktar**'ı tıklatın.
2. `https://github.com/Azure-Samples/media-services-v3-rest-postman.git` kopyasını oluşturduğunuzda indirilen `Azure Media Service v3 Environment.postman_environment.json` dosyasına gidin.
6. **Azure Media Service v3 Environment** ortamı eklenir.

    > [!Note]
    > Erişim değişkenlerini yukarıdaki **Media Services API'sine erişme** bölümünden aldığınız değerlerle güncelleştirin.

7. Seçili dosyaya çift tıklayın ve [API'ye erişim](#access-api) adımlarını izleyerek aldığınız değerleri girin.
8. İletişim kutusunu kapatın.
9. Aşağı açılan listeden **Azure Media Service v3 Environment** ortamını seçin.

    ![Ortamı seçme](./media/develop-with-postman/choose-env.png)
   
### <a name="configure-the-collection"></a>Koleksiyonu yapılandırma

1. Koleksiyon dosyasını içe aktarmak için **İçe Aktar**'ı tıklatın.
1. `https://github.com/Azure-Samples/media-services-v3-rest-postman.git` kopyasını oluşturduğunuzda indirilen `Media Services v3.postman_collection.json` dosyasına gidin
3. **Media Services v3.postman_collection.json** dosyasını seçin.

    ![Dosya içe aktarma](./media/develop-with-postman/postman-import-collection.png)

## <a name="send-requests-using-postman"></a>Postman kullanarak istek gönderme

Bu bölümde, dosyanızı akışla aktarabilmeniz için kodlama ve URL oluşturma ile ilgili istekler göndereceğiz. Özellikle de aşağıdaki istekleri göndereceğiz:

1. Hizmet Sorumlusu Kimlik Doğrulaması için Azure AD Belirteci alma
1. Akış Bitiş Noktası Başlat
2. Çıktı varlığı oluşturma
3. Dönüşüm Oluşturma
4. İş Oluşturma
5. Akış Lı Bulucu Oluşturma
6. Akış Bulucu'nun yollarını listele

> [!Note]
>  Bu öğretici, tüm kaynakları benzersiz adlarla oluşturmakta olduğunuzu varsayar.  

### <a name="get-azure-ad-token"></a>Azure AD Belirteci alma 

1. Postacı uygulamasının sol penceresinde "Adım 1: AAD Auth belirteci alın" seçeneğini belirleyin.
2. Sonra, "Hizmet Sorumlusu Kimlik Doğrulaması için Azure AD Belirteci alma"'yı seçin.
3. **Gönder**’e basın.

    Aşağıdaki **POST** işlemi gönderilir.

    ```
    https://login.microsoftonline.com/:tenantId/oauth2/token
    ```

4. Yanıt belirteç ile gelir ve "AccessToken" ortam değişkenini belirteç değerine ayarlar. "AccessToken"'ı ayarlayan kodu görmek için **Testler** sekmesine tıklayın. 

    ![AAD belirteci alma](./media/develop-with-postman/postman-get-aad-auth-token.png)


### <a name="start-a-streaming-endpoint"></a>Akış Bitiş Noktası Başlat

Akışı etkinleştirmek için, önce videoyu akışı istediğiniz [Akış Bitiş Noktasını](https://docs.microsoft.com/azure/media-services/latest/streaming-endpoint-concept) başlatmanız gerekir.

> [!NOTE]
> Yalnızca Akış Bitiş Noktanız çalışan durumdayken faturalandırılırsınız.

1. Postacı uygulamasının sol penceresinde "Akış ve Canlı" seçeneğini belirleyin.
2. Ardından "StreamingEndpoint'i başlat" seçeneğini belirleyin.
3. **Gönder**’e basın.

    * Aşağıdaki **POST** işlemi gönderilir:

        ```
        https://management.azure.com/subscriptions/:subscriptionId/resourceGroups/:resourceGroupName/providers/Microsoft.Media/mediaservices/:accountName/streamingEndpoints/:streamingEndpointName/start?api-version={{api-version}}
        ```
    * İstek başarılı olursa, `Status: 202 Accepted` döndürülür.

        Bu durum, isteğin işleme için kabul edildiği anlamına gelir; ancak, işleme tamamlanmadı. `Azure-AsyncOperation` Yanıt üstbilgideki değeri temel alan işlem durumunu sorgulayabilirsiniz.

        Örneğin, aşağıdaki GET işlemi işleminizin durumunu döndürür:
        
        `https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/<resourceGroupName>/providers/Microsoft.Media/mediaservices/<accountName>/streamingendpointoperations/1be71957-4edc-4f3c-a29d-5c2777136a2e?api-version=2018-07-01`

        [Parça eşzamanlı Azure işlemleri](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-async-operations) makalesi, yanıtta döndürülen değerler aracılığıyla eşzamanlı Azure işlemlerinin durumunu nasıl izleyebilirsiniz ayrıntılı olarak açıklar.

### <a name="create-an-output-asset"></a>Çıktı varlığı oluşturma

Çıktı [Varlığı](https://docs.microsoft.com/rest/api/media/assets), kodlama işinizin sonucunu depolar. 

1. Postacı uygulamasının sol penceresinde "Varlıklar" seçeneğini belirleyin.
2. Ardından "Varlık oluşturma veya güncelleştirme"'yi seçin.
3. **Gönder**’e basın.

    * Aşağıdaki **PUT** işlemi gönderilir:

        ```
        https://management.azure.com/subscriptions/:subscriptionId/resourceGroups/:resourceGroupName/providers/Microsoft.Media/mediaServices/:accountName/assets/:assetName?api-version={{api-version}}
        ```
    * İşlemin gövdesi aşağıdaki gibidir:

        ```json
        {
        "properties": {
            "description": "My Asset",
            "alternateId" : "some GUID"
         }
        }
        ```

### <a name="create-a-transform"></a>Dönüşüm oluşturma

Media Services’te içerik kodlarken veya işlerken, kodlama ayarlarını bir tarif olarak ayarlamak yaygın bir modeldir. Daha sonra bu tarifi bir videoya uygulamak üzere bir **İş** gönderirsiniz. Her yeni video için yeni iş göndererek, bu tarifi kitaplığınızdaki tüm videolara uyguluyorsunuz. Media Services içinde tarif, **Dönüşüm** olarak adlandırılır. Daha fazla bilgi için [Dönüşümler ve İşler'e](transform-concept.md)bakın. Bu öğreticide açıklanan örnek, videoyu çeşitli iOS ve Android cihazlarına akışla aktarmak için kodlayan bir tarifi tanımlar. 

Yeni bir [Dönüşüm](https://docs.microsoft.com/rest/api/media/transforms) örneği oluştururken çıktı olarak neyi üretmesi istediğinizi belirtmeniz gerekir. Gerekli parametre bir **TransformOutput** nesnesidir. Her **TransformOutput** bir **Ön ayar** içerir. **Ön ayar**, video ve/veya ses işleme işlemlerinin istenen **TransformOutput** nesnesini oluşturmak üzere kullanılacak adım adım yönergelerini açıklar. Bu makalede açıklanan örnek, **AdaptiveStreaming** adlı yerleşik bir Ön Ayar kullanır. Ön Ayar, giriş çözünürlüğü ve bit hızını temel alarak, giriş videosunu otomatik olarak oluşturulan bir bit hızı basamağına (bit hızı-çözünürlük çiftleri) kodlar ve her bir bit hızı-çözünürlük çiftine karşılık gelen H.264 video ve AAC sesi ile ISO MP4 dosyaları üretir. Bu Ön Ayar hakkında bilgi için bkz. [otomatik oluşturulan bit hızı basamağı](autogen-bitrate-ladder.md).

Yerleşik bir EncoderNamedPreset ön ayarını veya özel ön ayarları kullanabilirsiniz. 

> [!Note]
> Bir [Dönüşüm](https://docs.microsoft.com/rest/api/media/transforms) oluştururken, önce **Get** yöntemini kullanarak zaten bir tane olup olmadığını denetlemelisiniz. Bu öğretici, dönüştürmeyi benzersiz bir adla oluşturmakta olduğunuzu varsayar.

1. Postacı uygulamasının sol penceresinde "Kodlama ve Analiz" seçeneğini belirleyin.
2. Ardından, "Dönüşüm Oluşturma"'yı seçin.
3. **Gönder**’e basın.

    * Aşağıdaki **PUT** işlemi gönderilir.

        ```
        https://management.azure.com/subscriptions/:subscriptionId/resourceGroups/:resourceGroupName/providers/Microsoft.Media/mediaServices/:accountName/transforms/:transformName?api-version={{api-version}}
        ```
    * İşlemin gövdesi aşağıdaki gibidir:

        ```json
        {
            "properties": {
                "description": "Standard Transform using an Adaptive Streaming encoding preset from the library of built-in Standard Encoder presets",
                "outputs": [
                    {
                    "onError": "StopProcessingJob",
                "relativePriority": "Normal",
                    "preset": {
                        "@odata.type": "#Microsoft.Media.BuiltInStandardEncoderPreset",
                        "presetName": "AdaptiveStreaming"
                    }
                    }
                ]
            }
        }
        ```

### <a name="create-a-job"></a>Bir iş oluşturma

Burada [İş](https://docs.microsoft.com/rest/api/media/jobs), oluşturulan **Dönüşümü** belirli bir video girdisine veya ses içeriğine uygulamak için Media Services'e gönderilen istektir. **İş** giriş videosunun konumu ve çıktının konumu gibi bilgileri belirtir.

Bu örnekte, işin girişi bir HTTPS URL'sini ("https:\//nimbuscdn-nimbuspm.streaming.mediaservices.windows.net/2b533311-b215-4409-80af-529c3e853622/") temel alınr.

1. Postacı uygulamasının sol penceresinde "Kodlama ve Analiz" seçeneğini belirleyin.
2. Ardından "İş Oluşturma veya Güncelleştirme"'yi seçin.
3. **Gönder**’e basın.

    * Aşağıdaki **PUT** işlemi gönderilir.

        ```
        https://management.azure.com/subscriptions/:subscriptionId/resourceGroups/:resourceGroupName/providers/Microsoft.Media/mediaServices/:accountName/transforms/:transformName/jobs/:jobName?api-version={{api-version}}
        ```
    * İşlemin gövdesi aşağıdaki gibidir:

        ```json
        {
        "properties": {
            "input": {
            "@odata.type": "#Microsoft.Media.JobInputHttp",
            "baseUri": "https://nimbuscdn-nimbuspm.streaming.mediaservices.windows.net/2b533311-b215-4409-80af-529c3e853622/",
            "files": [
                    "Ignite-short.mp4"
                ]
            },
            "outputs": [
            {
                "@odata.type": "#Microsoft.Media.JobOutputAsset",
                "assetName": "testAsset1"
            }
            ]
        }
        }
        ```

İşin tamamlanması biraz sürüyor ve tamamlandığında bildirim almak istiyorsunuz. İşin ilerleme durumunu görmek için Event Grid'in kullanılmasını öneririz. Event Grid yüksek kullanılabilirlik, tutarlı performans ve dinamik ölçek için tasarlanmıştır. Event Grid ile uygulamalarınız neredeyse tüm Azure hizmetleri ve özel kaynaklardan gelen olayları takip edip bu olaylara yanıt verebilir. Basit, HTTP tabanlı reaktif olay işleme özelliği, olayların akıllı filtrelenmesi ve yönlendirilmesi sayesinde etkili çözümler oluşturmanıza yardımcı olur.  Bkz. [Olayları özel bir web uç noktasına yönlendirme](job-state-events-cli-how-to.md).

**İş** genellik şu aşamalardan geçer: **Zamanlandı**, **Kuyruğa Alındı**, **İşleniyor**, **Tamamlandı** (son aşama). İş bir hatayla karşılaştıysa **Hata** durumunu alırsınız. İş iptal edilme sürecindeyse **İptal Ediliyor** ve **İptal Edildi** durumunu alırsınız.

#### <a name="job-error-codes"></a>İş hata kodları

[Bkz. Hata kodları.](https://docs.microsoft.com/rest/api/media/jobs/get#joberrorcode)

### <a name="create-a-streaming-locator"></a>Akış bulucusu oluşturma

Kodlama işi tamamlandıktan sonra, bir sonraki adım, çıkış **Varlığı'ndaki** videoyu oynatma katmak için istemcilerin kullanımına açmaktır. Bunu iki adımda gerçekleştirebilirsiniz: ilk olarak, bir [StreamingLocator](https://docs.microsoft.com/rest/api/media/streaminglocators) oluşturun ve ikinci olarak, istemcilerin kullanabildiği akış URL’lerini derleyin. 

Akış bulucu oluşturma işlemine yayımlama denir. Varsayılan olarak, akış bulucu, API aramalarını yaptıktan hemen sonra geçerlidir ve isteğe bağlı başlangıç ve bitiş saatlerini yapılandırmadığınız sürece silinene kadar sürer. 

Bir [StreamingLocator](https://docs.microsoft.com/rest/api/media/streaminglocators) oluştururken istenen **StreamingPolicyName** değerini belirtmeniz gerekir. Bu örnekte, açık (veya şifrelenmemiş) içerik akışı olursunuz, böylece önceden tanımlanmış açık akış ilkesi "Predefined_ClearStreamingOnly" kullanılır.

> [!IMPORTANT]
> Özel [StreamingPolicy](https://docs.microsoft.com/rest/api/media/streamingpolicies)’yi kullanırken Media Service hesabınız için bu tür ilkelerin sınırlı bir kümesini tasarlamanız ve aynı şifreleme seçenekleri ve protokoller gerekli olduğunda StreamingLocators için bunları kullanmanız gerekir. 

Medya Hizmeti hesabınızda **Akış İlkesi** girişi sayısı için bir kota vardır. Her akış bulucu için yeni bir **Akış İlkesi** oluşturmamalısınız.

1. Postacı uygulamasının sol penceresinde "Akış İlkeleri ve Yer Bulucular" seçeneğini belirleyin.
2. Ardından, "Akış Bulucu Oluştur (açık)" seçeneğini belirleyin.
3. **Gönder**’e basın.

    * Aşağıdaki **PUT** işlemi gönderilir.

        ```
        https://management.azure.com/subscriptions/:subscriptionId/resourceGroups/:resourceGroupName/providers/Microsoft.Media/mediaServices/:accountName/streamingLocators/:streamingLocatorName?api-version={{api-version}}
        ```
    * İşlemin gövdesi aşağıdaki gibidir:

        ```json
        {
          "properties": {
            "streamingPolicyName": "Predefined_ClearStreamingOnly",
            "assetName": "testAsset1",
            "contentKeys": [],
            "filters": []
         }
        }
        ```

### <a name="list-paths-and-build-streaming-urls"></a>Yolları listeleme ve akış URL'lerini derleme

#### <a name="list-paths"></a>Yolları listeleme

[Artık Akış Bulucu](https://docs.microsoft.com/rest/api/media/streaminglocators) oluşturulduğuna göre, akış URL'lerini alabilirsiniz

1. Postacı uygulamasının sol penceresinde "Akış İlkeleri"ni seçin.
2. Ardından, "Yolları Listele"'yi seçin.
3. **Gönder**’e basın.

    * Aşağıdaki **POST** işlemi gönderilir.

        ```
        https://management.azure.com/subscriptions/:subscriptionId/resourceGroups/:resourceGroupName/providers/Microsoft.Media/mediaServices/:accountName/streamingLocators/:streamingLocatorName/listPaths?api-version={{api-version}}
        ```
        
    * İşlemin gövdesi yoktur:
        
4. Akış için kullanmak istediğiniz yollardan birini not edin; sonraki bölümde kullanacaksınız. Burada aşağıdaki yollar döndürülür:
    
    ```
    "streamingPaths": [
        {
            "streamingProtocol": "Hls",
            "encryptionScheme": "NoEncryption",
            "paths": [
                "/cdb80234-1d94-42a9-b056-0eefa78e5c63/Ignite-short.ism/manifest(format=m3u8-aapl)"
            ]
        },
        {
            "streamingProtocol": "Dash",
            "encryptionScheme": "NoEncryption",
            "paths": [
                "/cdb80234-1d94-42a9-b056-0eefa78e5c63/Ignite-short.ism/manifest(format=mpd-time-csf)"
            ]
        },
        {
            "streamingProtocol": "SmoothStreaming",
            "encryptionScheme": "NoEncryption",
            "paths": [
                "/cdb80234-1d94-42a9-b056-0eefa78e5c63/Ignite-short.ism/manifest"
            ]
        }
    ]
    ```

#### <a name="build-the-streaming-urls"></a>Akış URL'lerini derleme

Bu bölümde HLS akış URL'sini derleyeceğiz. URL'ler aşağıdaki değerlerden oluşur:

1. Veri gönderme protokolü. Burada "https".

    > [!NOTE]
    > Oynatıcı bir https sitesinde barındırılıyorsa, "https" URL’sini güncelleştirdiğinizden emin olun.

2. StreamingEndpoint'in konak adı. Burada "amsaccount-usw22.streaming.media.azure.net".

    Ana bilgisayar adını almak için aşağıdaki GET işlemini kullanabilirsiniz:
    
    ```
    https://management.azure.com/subscriptions/00000000-0000-0000-0000-0000000000000/resourceGroups/amsResourceGroup/providers/Microsoft.Media/mediaservices/amsaccount/streamingEndpoints/default?api-version={{api-version}}
    ```
    
3. Önceki bölümde (Yolları listeleme) aldığınız yol.  

Sonuç olarak, aşağıdaki HLS URL'si derlenir

```
https://amsaccount-usw22.streaming.media.azure.net/cdb80234-1d94-42a9-b056-0eefa78e5c63/Ignite-short.ism/manifest(format=m3u8-aapl)
```

## <a name="test-the-streaming-url"></a>Akış URL’sini test etme


> [!NOTE]
> Akış yapmak istediğiniz **Akış Bitiş Noktası'nın** çalışmadığından emin olun.

Bu makalede, akışı test etmek için Azure Media Player kullanılmaktadır. 

1. Bir web tarayıcısı [https://aka.ms/azuremediaplayer/](https://aka.ms/azuremediaplayer/)açın ve 'ye gidin.
2. **URL:** kutusuna derlediğiniz URL'yi yapıştırın. 
3. **Oynatıcıyı Güncelleştir** düğmesine basın.

Azure Media Player, test için kullanılabilir, ancak üretim ortamında kullanılmamalıdır. 

## <a name="clean-up-resources-in-your-media-services-account"></a>Media Services hesabınızdaki kaynakları temizleme

Genellikle, yeniden kullanmayı planladığınız nesneler dışındaki her şeyi temizlemeniz gerekir (genellikle **Transforms'u**yeniden kullanırsınız ve Akış Lı Konum **belirlemecileri,** vb.) devam erecektir. Deneme sonrasında hesabınızın temiz olmasını istiyorsanız, yeniden kullanmayı planlamadığınız kaynakları silmeniz gerekir.  

Bir kaynağı silmek için, silmek istediğiniz kaynağın altından "Sil ..." işlemini seçin.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğreticide oluşturduğunuz Media Services ve depolama hesapları dahil olmak üzere, kaynak grubunuzdaki kaynaklardan herhangi birine artık ihtiyacınız yoksa kaynak grubunu silebilirsiniz.  

Aşağıdaki CLI komutunu uygulayın:

```azurecli
az group delete --name amsResourceGroup
```

## <a name="ask-questions-give-feedback-get-updates"></a>Soru sorun, geri bildirimde, güncellemeler alın

Soru sormanın, geri bildirimde bulunabilir ve Medya Hizmetleri hakkında güncellemeler alabildiğiniz farklı yolları görmek için [Azure Medya Hizmetleri topluluk](media-services-community.md) makalesine göz atın.

## <a name="next-steps"></a>Sonraki adımlar

Videonuzu karşıya yükleme, kodlama ve akışla aktarma hakkında bilgi edindiğinize göre aşağıdaki makaleye bakabilirsiniz: 

> [!div class="nextstepaction"]
> [Videoları analiz etme](analyze-videos-tutorial-with-api.md)
