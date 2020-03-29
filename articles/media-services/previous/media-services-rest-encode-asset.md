---
title: Media Encoder Standard'ı kullanarak bir Azure kıymetini kodlama | Microsoft Dokümanlar
description: Azure Medya Hizmetleri'ndeki medya içeriğini kodlamak için Media Encoder Standard'ı nasıl kullanacağınızı öğrenin. Kod örnekleri REST API kullanır.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.assetid: 2a7273c6-8a22-4f82-9bfe-4509ff32d4a4
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2019
ms.author: juliako
ms.openlocfilehash: 6854400f2152a5952a7b24dbd860d7ad4bfc943d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76774927"
---
# <a name="how-to-encode-an-asset-by-using-media-encoder-standard"></a>Media Encoder Standard'ı kullanarak bir varlığın kodlaması
> [!div class="op_single_selector"]
> * [.NET](media-services-dotnet-encode-with-media-encoder-standard.md)
> * [Geri kalanı](media-services-rest-encode-asset.md)
> * [Portal](media-services-portal-encode.md)
>
>

## <a name="overview"></a>Genel Bakış

Internet üzerinden dijital video sunmak için ortamı sıkıştırmanız gerekir. Dijital video dosyaları büyüktür ve Internet üzerinden teslim edilemeyecek kadar büyük olabilir veya müşterilerinizin aygıtlarının düzgün görüntülenmesi için. Kodlama, müşterilerinizin medyanızı görüntüleyebilmeleri için video ve sesi sıkıştırma işlemidir.

Kodlama işleri, Azure Medya Hizmetleri'nde en yaygın işleme işlemlerinden biridir. Medya dosyalarını bir kodlamadan diğerine dönüştürmek için kodlama işleri oluşturursunuz. Kodladiğinizde, Ortam Hizmetleri yerleşik kodlayıcısını (Media Encoder Standard) kullanabilirsiniz. Medya Hizmetleri ortağı tarafından sağlanan bir kodlayıcıyı da kullanabilirsiniz. Azure Marketi'nden üçüncü taraf kodlayıcılar kullanılabilir. Kodlayıcınız için tanımlanan önceden ayarlanmış dizeleri kullanarak veya önceden ayarlanmış yapılandırma dosyalarını kullanarak görevleri kodlamanın ayrıntılarını belirtebilirsiniz. Kullanılabilir hazır ayar türlerini görmek [için Ortam Kodlayıcı Standardı için Görev Hazır Ayarları'na](https://msdn.microsoft.com/library/mt269960)bakın.

Her işin, gerçekleştirmek istediğiniz işlem türüne bağlı olarak bir veya daha fazla görevi olabilir. REST API aracılığıyla, işleri ve ilgili görevleri iki şekilde oluşturabilirsiniz:

* Görevler, İş varlıklarıüzerindeki Görevler gezinti özelliği aracılığıyla satır satır olarak tanımlanabilir.
* OData toplu işlemeyi kullanın.

Kaynak dosyalarınızı her zaman uyarlanabilir bitrate MP4 setine kodlamanızı ve [ardından dinamik ambalaj](media-services-dynamic-packaging-overview.md)kullanarak seti istenilen biçime dönüştürmenizi öneririz.

Çıktı varlığınız depolama şifreliyse, varlık teslim ilkesini yapılandırmanız gerekir. Daha fazla bilgi için [bkz.](media-services-rest-configure-asset-delivery-policy.md)

## <a name="considerations"></a>Dikkat edilmesi gerekenler

Medya Hizmetleri'ndeki varlıklara erişirken, HTTP isteklerinizde belirli üstbilgi alanları ve değerleri belirlemeniz gerekir. Daha fazla bilgi için Medya [Hizmetleri REST API Geliştirme kurulumu'na](media-services-rest-how-to-use.md)bakın.

Ortam işlemcilerine başvurmaya başlamadan önce, doğru ortam işlemci kimliğine sahip olduğunuzu doğrulayın. Daha fazla bilgi için [bkz.](media-services-rest-get-media-processor.md)

## <a name="connect-to-media-services"></a>Media Services’e bağlanmak

AMS API'sine nasıl bağlanabileceğiniz hakkında bilgi için Azure [AD kimlik doğrulaması yla Azure Medya Hizmetleri API'sine eriş'e](media-services-use-aad-auth-to-access-ams-api.md)bakın. 

## <a name="create-a-job-with-a-single-encoding-task"></a>Tek bir kodlama görevi olan bir iş oluşturma

> [!NOTE]
> Medya Hizmetleri REST API ile çalışırken aşağıdaki hususlar geçerlidir:
>
> Medya Hizmetleri'ndeki varlıklara erişirken, HTTP isteklerinizde belirli üstbilgi alanları ve değerleri belirlemeniz gerekir. Daha fazla bilgi için Medya [Hizmetleri REST API geliştirme kurulumuna](media-services-rest-how-to-use.md)bakın.
>
> JSON kullanırken ve istekte **__metadata** anahtar sözcüğün kullanılacağını belirtirken (örneğin, bağlantılı bir nesneye başvurmak için), **üstbilgisini** [JSON Verbose biçimine](https://www.odata.org/documentation/odata-version-3-0/json-verbose-format/)ayarlamanız gerekir: Kabul et: uygulama/json;odata=verbose.
>
>

Aşağıdaki örnek, belirli bir çözünürlükte ve kalitede bir videoyu kodlamak için tek bir görev kümesiyle nasıl bir iş oluşturup gönderilebilirsiniz. Media Encoder Standard ile kodlarken, [burada](https://msdn.microsoft.com/library/mt269960)belirtilen görev yapılandırma hazır ayarlarını kullanabilirsiniz.

İstek:

    POST https://media.windows.net/API/Jobs HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.19
        Authorization: Bearer <ENCODED JWT TOKEN> 
        x-ms-client-request-id: 00000000-0000-0000-0000-000000000000
        Host: media.windows.net

    {"Name" : "NewTestJob", "InputMediaAssets" : [{"__metadata" : {"uri" : "https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3Aaab7f15b-3136-4ddf-9962-e9ecb28fb9d2')"}}],  "Tasks" : [{"Configuration" : "Adaptive Streaming", "MediaProcessorId" : "nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",  "TaskBody" : "<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset>JobOutputAsset(0)</outputAsset></taskBody>"}]}

Yanıt:

    HTTP/1.1 201 Created

    . . .

### <a name="set-the-output-assets-name"></a>Çıktı varlığının adını ayarlama
Aşağıdaki örnek, varlık Adı özniteliğinin nasıl ayarlanır:

    { "TaskBody" : "<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset assetName=\"CustomOutputAssetName\">JobOutputAsset(0)</outputAsset></taskBody>"}

## <a name="considerations"></a>Dikkat edilmesi gerekenler
* TaskBody özellikleri, görev tarafından kullanılan giriş veya çıktı varlıklarını tanımlamak için gerçek XML kullanmalıdır. Görev makalesi XML için XML Şema Tanımı içerir.
* TaskBody tanımında, her iç `<inputAsset>` `<outputAsset>` değer için ve JobInputAsset(değer) veya JobOutputAsset (değer) olarak ayarlanmalıdır.
* Bir görevin birden çok çıktı varlığı olabilir. Bir JobOutputAsset(x) bir işte bir görevin çıktısı olarak yalnızca bir kez kullanılabilir.
* JobInputAsset veya JobOutputAsset'i görevin giriş varlığı olarak belirtebilirsiniz.
* Görevler bir döngü oluşturmamalıdır.
* JobInputAsset veya JobOutputAsset'e geçtiğiniz değer parametresi, bir varlığın dizin değerini temsil eder. Gerçek varlıklar, iş varlığı tanımındaki InputMediaAssets ve OutputMediaAssets navigasyon özelliklerinde tanımlanır.
* Medya Hizmetleri OData v3 üzerine kurulduğundan, InputMediaAssets ve OutputMediaAssets navigasyon özellik koleksiyonlarındaki tek tek varlıklar "__metadata: uri" ad değeri çifti aracılığıyla başvurulur.
* InputMediaAssets, Medya Hizmetleri'nde oluşturduğunuz bir veya daha fazla varlıkla eşler. OutputMediaAssets sistem tarafından oluşturulur. Varolan bir varlığa atıfta bulunmuyorlar.
* OutputMediaAssets assetName özniteliği kullanılarak adlandırılabilir. Bu öznitelik yoksa, OutputMediaAsset'in adı, `<outputAsset>` öğenin iç metin değerinin, İş Adı değerinin veya İş Kimliği değerinin soneğiyle (Ad özelliğinin tanımlanmamış olması durumunda) ne olursa olsun. Örneğin, varlık Adı için bir değer "Örnek" olarak ayarlarsanız, OutputMediaAsset Name özelliği "Örnek" olarak ayarlanır. Ancak, varlık Adı için bir değer ayarlamadıysanız, ancak iş adını "NewJob" olarak ayarladıysanız, OutputMediaAsset Adı "JobOutputAsset(değer)_NewJob" olacaktır.

## <a name="create-a-job-with-chained-tasks"></a>Zincirli görevlerle bir iş oluşturma
Birçok uygulama senaryosunda, geliştiriciler bir dizi işleme görevi oluşturmak ister. Medya Hizmetleri'nde, bir dizi zincirlenmiş görev oluşturabilirsiniz. Her görev farklı işleme adımları gerçekleştirir ve farklı medya işlemcileri kullanabilirsiniz. Zincirlenmiş görevler, kıymet üzerinde doğrusal bir görev sırasını gerçekleştirecek şekilde bir varlığı bir görevden diğerine devredebilir. Ancak, bir işte gerçekleştirilen görevlerin sıralı olması gerekmez. Zincirlenmiş bir görev oluşturduğunuzda, zincirlenmiş **ITask** nesneleri tek bir **IJob** nesnesinde oluşturulur.

> [!NOTE]
> Şu anda iş başına 30 görev sınırı vardır. 30'dan fazla görev zinciri yapmanız gerekiyorsa, görevleri içerecek şekilde birden fazla iş oluşturun.
>
>

    POST https://media.windows.net/api/Jobs HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.19
    Authorization: Bearer <ENCODED JWT TOKEN> 
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000

    {  
       "Name":"NewTestJob",
       "InputMediaAssets":[  
          {  
             "__metadata":{  
                "uri":"https://testrest.cloudapp.net/api/Assets('nb%3Acid%3AUUID%3A910ffdc1-2e25-4b17-8a42-61ffd4b8914c')"
             }
          }
       ],
       "Tasks":[  
          {  
             "Configuration":"H264 Adaptive Bitrate MP4 Set 720p",
             "MediaProcessorId":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
             "TaskBody":"<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset>JobOutputAsset(0)</outputAsset></taskBody>"
          },
          {  
             "Configuration":"H264 Smooth Streaming 720p",
             "MediaProcessorId":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
             "TaskBody":"<?xml version=\"1.0\" encoding=\"utf-16\"?><taskBody><inputAsset>JobOutputAsset(0)</inputAsset><outputAsset>JobOutputAsset(1)</outputAsset></taskBody>"
          }
       ]
    }


### <a name="considerations"></a>Dikkat edilmesi gerekenler
Görev zincirleme etkinleştirmek için:

* Bir işin en az iki görevi olmalıdır.
* Girdiişteki başka bir görevin çıktısı olan en az bir görev olmalıdır.

## <a name="use-odata-batch-processing"></a>OData toplu işlemeyi kullanma
Aşağıdaki örnek, bir iş ve görev oluşturmak için OData toplu iş işlemenasıl kullanılacağını gösterir. Toplu işleme hakkında bilgi için [Açık Veri Protokolü (OData) Toplu İşleme'ye](https://www.odata.org/documentation/odata-version-3-0/batch-processing/)bakın.

    POST https://media.windows.net/api/$batch HTTP/1.1
    DataServiceVersion: 1.0;NetFx
    MaxDataServiceVersion: 3.0;NetFx
    Content-Type: multipart/mixed; boundary=batch_a01a5ec4-ba0f-4536-84b5-66c5a5a6d34e
    Accept: multipart/mixed
    Accept-Charset: UTF-8
    Authorization: Bearer <ENCODED JWT TOKEN> 
    x-ms-version: 2.19
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000
    Host: media.windows.net


    --batch_a01a5ec4-ba0f-4536-84b5-66c5a5a6d34e
    Content-Type: multipart/mixed; boundary=changeset_122fb0a4-cd80-4958-820f-346309967e4d

    --changeset_122fb0a4-cd80-4958-820f-346309967e4d
    Content-Type: application/http
    Content-Transfer-Encoding: binary

    POST https://media.windows.net/api/Jobs HTTP/1.1
    Content-ID: 1
    Content-Type: application/json
    Accept: application/json
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    Accept-Charset: UTF-8
    Authorization: Bearer <ENCODED JWT TOKEN> 
    x-ms-version: 2.19
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000

    {"Name" : "NewTestJob", "InputMediaAssets@odata.bind":["https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3A2a22445d-1500-80c6-4b34-f1e5190d33c6')"]}

    --changeset_122fb0a4-cd80-4958-820f-346309967e4d
    Content-Type: application/http
    Content-Transfer-Encoding: binary

    POST https://media.windows.net/api/$1/Tasks HTTP/1.1
    Content-ID: 2
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    Accept-Charset: UTF-8
    Authorization: Bearer <ENCODED JWT TOKEN> 
    x-ms-version: 2.19
    x-ms-client-request-id: 00000000-0000-0000-0000-000000000000

    {  
       "Configuration":"H264 Adaptive Bitrate MP4 Set 720p",
       "MediaProcessorId":"nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56",
       "TaskBody":"<?xml version=\"1.0\" encoding=\"utf-8\"?><taskBody><inputAsset>JobInputAsset(0)</inputAsset><outputAsset assetName=\"Custom output name\">JobOutputAsset(0)</outputAsset></taskBody>"
    }

    --changeset_122fb0a4-cd80-4958-820f-346309967e4d--
    --batch_a01a5ec4-ba0f-4536-84b5-66c5a5a6d34e--



## <a name="create-a-job-by-using-a-jobtemplate"></a>İş Şablonu kullanarak iş oluşturma
Ortak bir görev kümesi kullanarak birden çok varlığı işlediğinizi, varsayılan görev hazır ayarlarını belirtmek veya görevlerin sırasını ayarlamak için bir İş Şablonu kullanın.

Aşağıdaki örnek, sıralı olarak tanımlanan Bir Görev Şablonu ile Bir İş Şablonu nasıl oluşturulacak gösterilmektedir. TaskTemplate, varlık dosyasını kodlamak için MediaProcessor olarak Media Encoder Standard'ı kullanır. Ancak, diğer MediaProcessors de kullanılabilir.

    POST https://media.windows.net/API/JobTemplates HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.19
    Authorization: Bearer <ENCODED JWT TOKEN> 
    Host: media.windows.net


    {"Name" : "NewJobTemplate25", "JobTemplateBody" : "<?xml version=\"1.0\" encoding=\"utf-8\"?><jobTemplate><taskBody taskTemplateId=\"nb:ttid:UUID:071370A3-E63E-4E81-A099-AD66BCAC3789\"><inputAsset>JobInputAsset(0)</inputAsset><outputAsset>JobOutputAsset(0)</outputAsset></taskBody></jobTemplate>", "TaskTemplates" : [{"Id" : "nb:ttid:UUID:071370A3-E63E-4E81-A099-AD66BCAC3789", "Configuration" : "H264 Smooth Streaming 720p", "MediaProcessorId" : "nb:mpid:UUID:ff4df607-d419-42f0-bc17-a481b1331e56", "Name" : "SampleTaskTemplate2", "NumberofInputAssets" : 1, "NumberofOutputAssets" : 1}] }


> [!NOTE]
> Diğer Medya Hizmetleri kuruluşlarının aksine, her TaskTemplate için yeni bir GUID tanımlayıcısı tanımlamanız ve bunu istek gövdenizdeki TaskTemplateId ve Id özelliğine yerleştirmeniz gerekir. İçerik tanımlama şeması, Azure Medya Hizmetlerini Tanımla'da açıklanan şemayı izlemelidir. Ayrıca, İş Şablonları güncelleştirilemez. Bunun yerine, güncelleştirilmiş değişiklikleriniz ile yeni bir tane oluşturmanız gerekir.
>
>

Başarılı olursa, aşağıdaki yanıt döndürülür:

    HTTP/1.1 201 Created

    . . .


Aşağıdaki örnek, Bir İş Şablonu Kimliğine başvuran bir işin nasıl oluşturulutur:

    POST https://media.windows.net/API/Jobs HTTP/1.1
    Content-Type: application/json;odata=verbose
    Accept: application/json;odata=verbose
    DataServiceVersion: 3.0
    MaxDataServiceVersion: 3.0
    x-ms-version: 2.19
    Authorization: Bearer <ENCODED JWT TOKEN> 
    Host: media.windows.net


    {"Name" : "NewTestJob", "InputMediaAssets" : [{"__metadata" : {"uri" : "https://media.windows.net/api/Assets('nb%3Acid%3AUUID%3A3f1fe4a2-68f5-4190-9557-cd45beccef92')"}}], "TemplateId" : "nb:jtid:UUID:15e6e5e6-ac85-084e-9dc2-db3645fbf0aa"}


Başarılı olursa, aşağıdaki yanıt döndürülür:

    HTTP/1.1 201 Created

    . . .


## <a name="advanced-encoding-features-to-explore"></a>Keşfedilecek Gelişmiş Kodlama Özellikleri
* [Küçük resimler nasıl üretilir?](media-services-dotnet-generate-thumbnail-with-mes.md)
* [Kodlama sırasında küçük resim oluşturma](media-services-dotnet-generate-thumbnail-with-mes.md#example-of-generating-a-thumbnail-while-encoding)
* [Kodlama sırasında videoları kırpma](media-services-crop-video.md)
* [Kodlama ön ayarlarını özelleştirme](media-services-custom-mes-presets-with-dotnet.md)
* [Görüntüyü içeren bir videoyu yerle bir veya filigranla kaplama](media-services-advanced-encoding-with-mes.md#overlay)

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="next-steps"></a>Sonraki adımlar
Artık bir varlığı kodlamak için nasıl bir iş oluşturacağınız konusunda bildiğinize göre, [Medya Hizmetleri ile iş ilerlemesini nasıl denetleyeceksiniz.](media-services-rest-check-job-progress.md)

## <a name="see-also"></a>Ayrıca bkz.
[Medya İşlemcileri Alın](media-services-rest-get-media-processor.md)
