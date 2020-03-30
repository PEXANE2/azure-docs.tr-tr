---
title: Premium Encoder ile birden çok giriş dosyası ve bileşen özelliği - Azure | Microsoft Dokümanlar
description: Bu konu, birden çok giriş dosyasını kullanmak ve özel verileri Media Encoder Premium İş Akışı ortam işlemcisine aktarmak için setRuntimeProperties'in nasıl kullanılacağını açıklar.
services: media-services
documentationcenter: ''
author: xpouyat
manager: femila
editor: ''
ms.assetid: 7fb35bdd-9891-4401-a65b-ef3cc8190e8a
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/18/2019
ms.author: xpouyat
ms.reviewer: anilmur;juliako
ms.openlocfilehash: 27bdf82d4515678e28eadf07fe325860fe5df063
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79251004"
---
# <a name="using-multiple-input-files-and-component-properties-with-premium-encoder"></a>Premium Encoder ile birden fazla giriş dosyası ve bileşen özelliği kullanma
## <a name="overview"></a>Genel Bakış
**Media Encoder Premium İş Akışı** ortam işlemcisi ile görev gönderirken bileşen özelliklerini özelleştirmeniz, Küçük Liste XML içeriğini belirtmeniz veya birden çok giriş dosyası göndermeniz gerekebileceği senaryolar vardır. Bazı örnekler şunlardır:

* Her giriş videosu için çalışma saatinde metni videoya üst üste bindirin ve metin değerini (örneğin, geçerli tarih) ayarlayın.
* Küçük Ler Listesi XML'i özelleştirme (kırpma lı veya kırpmasız, vb. bir veya birkaç kaynak dosya belirtmek için).
* Video kodlanırken giriş videosunda bir logo görüntüsünün yer leşmesi.
* Birden çok ses dili kodlama.

**Media Encoder Premium İş** Akışı'na, görevi oluşturduğunuzda veya birden çok giriş dosyası gönderdiğinde iş akışındaki bazı özellikleri değiştirdiğinizi bildirmek **için, setRuntimeProperties** ve/veya **transcodeSource**içeren bir yapılandırma dizesi kullanmanız gerekir. Bu konu, bunların nasıl kullanılacağını açıklar.

## <a name="configuration-string-syntax"></a>Yapılandırma dize sözdizimi
Kodlama görevinde ayarlanması için yapılandırma dizesi, aşağıdaki gibi görünen bir XML belgesi kullanır:

```xml
<?xml version="1.0" encoding="utf-8"?>
<transcodeRequest>
  <transcodeSource>
  </transcodeSource>
  <setRuntimeProperties>
    <property propertyPath="Media File Input/filename" value="VideoFileName.mp4" />
  </setRuntimeProperties>
</transcodeRequest>
```

XML yapılandırmasını bir dosyadan okuyan, doğru video dosya adı ile güncelleştiren ve bir işte göreve aktaran C# kodu aşağıda veda eder:

```csharp
string premiumConfiguration = ReadAllText(@"D:\home\site\wwwroot\Presets\SetRuntime.xml").Replace("VideoFileName", myVideoFileName);

// Declare a new job.
IJob job = _context.Jobs.Create("Premium Workflow encoding job");

// Get a media processor reference, and pass to it the name of the 
// processor to use for the specific task.
IMediaProcessor processor = GetLatestMediaProcessorByName("Media Encoder Premium Workflow");

// Create a task with the encoding details, using a string preset.
ITask task = job.Tasks.AddNew("Premium Workflow encoding task",
                              processor,
                              premiumConfiguration,
                              TaskOptions.None);

// Specify the input assets
task.InputAssets.Add(workflow); // workflow asset
task.InputAssets.Add(video); // video asset with multiple files

// Add an output asset to contain the results of the job. 
// This output is specified as AssetCreationOptions.None, which 
// means the output asset is not encrypted. 
task.OutputAssets.AddNew("Output asset", AssetCreationOptions.None);
```

## <a name="customizing-component-properties"></a>Bileşen özelliklerini özelleştirme
### <a name="property-with-a-simple-value"></a>Basit bir değere sahip özellik
Bazı durumlarda, Media Encoder Premium İş Akışı tarafından yürütülecek iş akışı dosyasıyla birlikte bir bileşen özelliğini özelleştirmek yararlıdır.

Videolarınızdaki metni bökezleyen bir iş akışı tasarladığınızı ve metnin (örneğin, geçerli tarih) çalışma zamanında ayarlandığı varsayalım. Bunu, kodlama görevinden bindirme bileşeninin metin özelliği için yeni değer olarak ayarlanacak metni göndererek yapabilirsiniz. Bu mekanizmayı, iş akışındaki bir bileşenin diğer özelliklerini (bindirmenin konumu veya rengi, AVC kodlayıcının bithızı vb.) değiştirmek için kullanabilirsiniz.

**setRuntimeProperties** iş akışı bileşenlerindeki bir özelliği geçersiz kılmak için kullanılır.

Örnek:

```xml
<?xml version="1.0" encoding="utf-8"?>
  <transcodeRequest>
    <setRuntimeProperties>
      <property propertyPath="Media File Input/filename" value="MyInputVideo.mp4" />
      <property propertyPath="/primarySourceFile" value="MyInputVideo.mp4" />
      <property propertyPath="Optional Text Overlay/Text To Image Converter/text" value="Today is Friday the 13th of May, 2016"/>
  </setRuntimeProperties>
</transcodeRequest>
```

### <a name="property-with-an-xml-value"></a>XML değeri olan özellik
XML değeri bekleyen bir özellik ayarlamak için, kullanarak `<![CDATA[ and ]]>`kapsülleme.

Örnek:

```xml
<?xml version="1.0" encoding="utf-8"?>
  <transcodeRequest>
    <setRuntimeProperties>
      <property propertyPath="/primarySourceFile" value="start.mxf" />
      <property propertyPath="/inactiveTimeout" value="65" />
      <property propertyPath="clipListXml" value="xxx">
      <extendedValue><![CDATA[<clipList>
        <clip>
          <videoSource>
            <mediaFile>
              <file>start.mxf</file>
            </mediaFile>
          </videoSource>
          <audioSource>
            <mediaFile>
              <file>start.mxf</file>
            </mediaFile>
          </audioSource>
        </clip>
        <primaryClipIndex>0</primaryClipIndex>
        </clipList>]]>
      </extendedValue>
      </property>
      <property propertyPath="Media File Input Logo/filename" value="logo.png" />
    </setRuntimeProperties>
  </transcodeRequest>
```

> [!NOTE]
> Hemen sonra `<![CDATA[`bir araba dönüş koymak değil emin olun.

### <a name="propertypath-value"></a>propertyPath değeri
Önceki örneklerde, propertyPath "/Media File Input/filename" veya "/inactiveTimeout" veya "clipListXml" idi.
Bu, genel olarak, bileşenin adı, sonra özelliğin adıdır. Yol, "/primarySourceFile" (özellik iş akışının kökünde olduğundan) veya "/Video İşleme/Grafik Bindirme/Opaklık" gibi daha fazla veya daha az düzeye sahip olabilir (Çünkü Bindirme bir gruptadır).    

Yol ve özellik adını denetlemek için, her özelliğin hemen yanındaki eylem düğmesini kullanın. Bu eylem düğmesini tıklatıp **Edit'i**seçebilirsiniz. Bu, özelliğin gerçek adını ve hemen üstündeki ad alanını gösterir.

![Eylem/Edit](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture6_actionedit.png)

![Özellik](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture7_viewproperty.png)

## <a name="multiple-input-files"></a>Birden çok giriş dosyası
**Media Encoder Premium İş Akışına** gönderdiğiniz her görev iki varlık gerektirir:

* Bunlardan ilki, iş akışı dosyası içeren bir *İş Akışı Varlığıdır.* İş Akışı [Tasarımcısı'nı](media-services-workflow-designer.md)kullanarak iş akışı dosyalarını tasarlayabilirsiniz.
* İkincisi, kodlamak istediğiniz ortam dosyasını(lar) içeren bir *Media Asset'dir.*

**Media Encoder Premium İş Akışı** kodlayıcısına birden fazla ortam dosyası gönderirken aşağıdaki kısıtlamalar geçerlidir:

* Tüm ortam dosyaları aynı *Media Asset'de*olmalıdır. Birden çok Medya Varlığı kullanmak desteklenmez.
* Bu Media Asset'deki birincil dosyayı ayarlamanız gerekir (ideal olarak, bu kodlayıcının işlemesi istenen ana video dosyasıdır).
* **SetRuntimeProperties** ve/veya **transcodeSource** öğesini içeren yapılandırma verilerini işlemciye aktarmak gerekir.
  * **setRuntimeProperties,** iş akışının bileşenlerindeki dosya adı özelliğini veya başka bir özelliği geçersiz kılmak için kullanılır.
  * **transcodeSource,** Klip Listesi XML içeriğini belirtmek için kullanılır.

İş akışındaki bağlantılar:

* Bir veya birkaç Ortam Dosyası Girişi bileşeni kullanıyorsanız ve dosya adını belirtmek için **setRuntimeProperties'i** kullanmayı planlıyorsanız, birincil dosya bileşeni pinini bunlara bağlamın. Birincil dosya nesnesi ile Ortam Dosyası Girişi(ler) arasında bağlantı olmadığından emin olun.
* Clip List XML ve bir Medya Kaynağı bileşenini kullanmayı tercih ederseniz, her ikisini de birbirine bağlayabilirsiniz.

![Birincil Kaynak Dosyadan Ortam Dosyası Girişine bağlantı yok](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture0_nopin.png)

*Dosya adı özelliğini ayarlamak için setRuntimeProperties kullanıyorsanız, birincil dosyadan Media File Input bileşenine(ler) bağlantı yoktur.*

![Kliplistesi XML'den Klip Listesi Kaynağına Bağlantı](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture1_pincliplist.png)

*Clip List XML'i Media Source'a bağlayabilir ve transcodeSource'u kullanabilirsiniz.*

### <a name="clip-list-xml-customization"></a>Klip Listesi XML özelleştirme
**TranscodeSource** yapılandırma dizesi XML kullanarak çalışma zamanında iş akışında Klip Listesi XML belirtebilirsiniz. Bu, Klip Listesi XML pininin iş akışındaki Medya Kaynağı bileşenine bağlanmasını gerektirir.

```xml
<?xml version="1.0" encoding="utf-16"?>
  <transcodeRequest>
    <transcodeSource>
      <clipList>
        <clip>
          <videoSource>
            <mediaFile>
              <file>video-part1.mp4</file>
            </mediaFile>
          </videoSource>
          <audioSource>
            <mediaFile>
              <file>video-part1.mp4</file>
            </mediaFile>
          </audioSource>
        </clip>
        <primaryClipIndex>0</primaryClipIndex>
      </clipList>
    </transcodeSource>
    <setRuntimeProperties>
      <property propertyPath="Media File Input Logo/filename" value="logo.png" />
    </setRuntimeProperties>
  </transcodeRequest>
```

Çıktı dosyalarını 'İfadeler' kullanarak adlandırmak için bu özelliği kullanmak için /primarySourceFile belirtmek istiyorsanız, Clip List'in /primarySourceFile ayarı tarafından geçersiz kılınmasını önlemek için Klip Listesi XML'i /primarySourceFile özelliğinden *sonra* özellik olarak geçirmenizi öneririz.

```xml
<?xml version="1.0" encoding="utf-8"?>
  <transcodeRequest>
    <setRuntimeProperties>
      <property propertyPath="/primarySourceFile" value="c:\temp\start.mxf" />
      <property propertyPath="/inactiveTimeout" value="65" />
      <property propertyPath="clipListXml" value="xxx">
      <extendedValue><![CDATA[<clipList>
        <clip>
          <videoSource>
            <mediaFile>
              <file>c:\temp\start.mxf</file>
            </mediaFile>
          </videoSource>
          <audioSource>
            <mediaFile>
              <file>c:\temp\start.mxf</file>
            </mediaFile>
          </audioSource>
        </clip>
        <primaryClipIndex>0</primaryClipIndex>
        </clipList>]]>
      </extendedValue>
      </property>
      <property propertyPath="Media File Input Logo/filename" value="logo.png" />
    </setRuntimeProperties>
  </transcodeRequest>
```

Ek çerçeve doğru kırpma ile:

```xml
<?xml version="1.0" encoding="utf-8"?>
  <transcodeRequest>
    <setRuntimeProperties>
      <property propertyPath="/primarySourceFile" value="start.mxf" />
      <property propertyPath="/inactiveTimeout" value="65" />
      <property propertyPath="clipListXml" value="xxx">
      <extendedValue><![CDATA[<clipList>
        <clip>
          <videoSource>
            <trim>
              <inPoint fps="25">00:00:05:24</inPoint>
              <outPoint fps="25">00:00:10:24</outPoint>
            </trim>
            <mediaFile>
              <file>start.mxf</file>
            </mediaFile>
          </videoSource>
          <audioSource>
            <trim>
              <inPoint fps="25">00:00:05:24</inPoint>
              <outPoint fps="25">00:00:10:24</outPoint>
            </trim>
            <mediaFile>
              <file>start.mxf</file>
            </mediaFile>
          </audioSource>
        </clip>
        <primaryClipIndex>0</primaryClipIndex>
        </clipList>]]>
      </extendedValue>
      </property>
      <property propertyPath="Media File Input Logo/filename" value="logo.png" />
    </setRuntimeProperties>
  </transcodeRequest>
```

## <a name="example-1--overlay-an-image-on-top-of-the-video"></a>Örnek 1 : Videonun üstüne bir görüntü yerle bir

### <a name="presentation"></a>Sunum
Video kodlanırken giriş videosuna bir logo resmi bindirmek istediğiniz bir örnek düşünün. Bu örnekte, giriş videosu "Microsoft_HoloLens_Possibilities_816p24.mp4" ve logosu "logo.png" olarak adlandırılır. Aşağıdaki adımları gerçekleştirmeniz gerekir:

* İş akışı dosyasıyla birlikte bir İş Akışı Varlığı oluşturun (aşağıdaki örneğe bakın).
* İki dosya içeren bir Medya Varlığı oluşturun: MyInputVideo.mp4 birincil dosya ve MyLogo.png olarak.
* Yukarıdaki giriş varlıklarıyla Media Encoder Premium İş Akışı ortam işlemcisine bir görev gönderin ve aşağıdaki yapılandırma dizesini belirtin.

Yapılandırma:

```xml
<?xml version="1.0" encoding="utf-16"?>
  <transcodeRequest>
    <setRuntimeProperties>
      <property propertyPath="Media File Input/filename" value="Microsoft_HoloLens_Possibilities_816p24.mp4" />
      <property propertyPath="/primarySourceFile" value="Microsoft_HoloLens_Possibilities_816p24.mp4" />
      <property propertyPath="Media File Input Logo/filename" value="logo.png" />
    </setRuntimeProperties>
  </transcodeRequest>
```

Yukarıdaki örnekte, video dosyasının adı Media File Input bileşenine ve birincilSourceFile özelliğine gönderilir. Logo dosyasının adı, grafik kaplama bileşenine bağlı başka bir Medya Dosyası Girişi'ne gönderilir.

> [!NOTE]
> Video dosyası adı birincilSourceFile özelliğine gönderilir. Bunun nedeni, örneğin İfadeler'i kullanarak doğru çıktı dosya adını oluşturmak için iş akışında bu özelliği kullanmaktır.

### <a name="step-by-step-workflow-creation"></a>Adım adım iş akışı oluşturma
Burada, giriş olarak iki dosya alan bir iş akışı oluşturma adımları ve bir görüntü ve bir görüntü ve giriş vardır. Görüntüyü videonun üstüne yerleştirir.

**İş Akışı Tasarımcısı'nı** açın ve **Dosya** > **Yeni İş Alanı** > **Transcode Blueprint'i**seçin.

Yeni iş akışı üç öğeyi gösterir:

* Birincil Kaynak Dosya
* Klip Listesi XML
* Çıktı Dosyası/Varlık  

![Yeni Kodlama İş Akışı](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture9_empty.png)

*Yeni Kodlama İş Akışı*

Giriş ortamı dosyasını kabul etmek için, bir Ortam Dosyası Girişi bileşeni ekleyerek başlayın. İş akışına bir bileşen eklemek için, Depo arama kutusunda onu arayın ve istenen girişi tasarımcı bölmesine sürükleyin.

Ardından, iş akışınızı tasarlamak için kullanılacak video dosyasını ekleyin. Bunu yapmak için, İş Akışı Tasarımcısı'ndaki arka plan bölmesini tıklatın ve sağ daki özellik bölmesinde Birincil Kaynak Dosya özelliğini arayın. Klasör simgesini tıklatın ve uygun video dosyasını seçin.

![Birincil Dosya Kaynağı](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture10_primaryfile.png)

*Birincil Dosya Kaynağı*

Ardından, Medya Dosyası Girişi bileşeninde video dosyasını belirtin.   

![Medya Dosya Giriş Kaynağı](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture11_mediafileinput.png)

*Medya Dosya Giriş Kaynağı*

Bu işlem yapılır yapılmaz, Ortam Dosyası Girişi bileşeni dosyayı inceler ve denetlediği dosyayı yansıtacak şekilde çıktı pinlerini doldurur.

Bir sonraki adım, Rec.709'a renk alanını belirtmek için bir "Video Veri Türü Güncelleyici" eklemektir. Veri Düzeni/Düzen türü = Yapılandırılabilir Düzlemsel'e ayarlanan bir "Video Biçimi Dönüştürücü" ekleyin. Bu, video akışını bindirme bileşeninin kaynağı olarak alınabilecek bir biçime dönüştürür.

![video Veri Türü Güncelleyici ve Biçim Dönüştürücü](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture12_formatconverter.png)

*Video Veri Türü Güncelleyici ve Biçim Lendirici*

![Düzen türü = Yapılandırılabilir Düzlemsel](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture12_formatconverter2.png)

*Düzen türü Yapılandırılabilir Düzlemsel*

Ardından, bir Video Bindirme bileşeni ekleyin ve (sıkıştırılmamış) video pinini ortam dosyası girişinin (sıkıştırılmamış) video pinine bağlayın.

Başka bir Medya Dosyası Girişi ekleyin (logo dosyasını yüklemek için), bu bileşene tıklayın ve "Media File Input Logo" olarak yeniden adlandırın ve dosya özelliğinde bir resim (örneğin .png dosyası) seçin. Sıkıştırılmamış görüntü pinini bindirmenin sıkıştırılmamış görüntü pimasına bağlayın.

![Yer kaplama bileşeni ve görüntü dosyası kaynağı](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture13_overlay.png)

*Yer kaplama bileşeni ve görüntü dosyası kaynağı*

Videodaki logonun konumunu değiştirmek istiyorsanız (örneğin, videonun sol üst köşesinden yüzde 10 oranında konumlandırmak isteyebilirsiniz), "El Ile Giriş" onay kutusunu temizleyin. Logo dosyasını bindirme bileşenine sağlamak için bir Ortam Dosyası Girişi kullandığınız için bunu yapabilirsiniz.

![Bindirme konumu](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture14_overlay_position.png)

*Bindirme konumu*

Video akışını H.264'e kodlamak için, Tasarımcı yüzeyine AVC Video Encoder ve AAC kodlayıcı bileşenlerini ekleyin. Pimleri bağlayın.
AAC kodlayıcısını ayarlayın ve Ses Biçimi Dönüştürme/Hazır Set: 2.0 (L, R) seçeneğini belirleyin.

![Ses ve Video Kodlayıcılar](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture15_encoders.png)

*Ses ve Video Kodlayıcılar*

Şimdi **ISO Mpeg-4 Multiplexer** ve **Dosya Çıkışı** bileşenlerini ekleyin ve pimleri gösterildiği gibi bağlayın.

![MP4 multiplexer ve dosya çıktısı](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture16_mp4output.png)

*MP4 multiplexer ve dosya çıktısı*

Çıktı dosyasının adını ayarlamanız gerekir. Dosya **Çıktısı** bileşenini tıklatın ve dosya için ifadeyi tıklatın:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_withoverlay.mp4

![Dosya çıktısı adı](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture17_filenameoutput.png)

*Dosya çıktısı adı*

Doğru çalışıp çalışmadığını denetlemek için iş akışını yerel olarak çalıştırabilirsiniz.

Bittiğinde, Azure Medya Hizmetleri'nde çalıştırabilirsiniz.

İlk olarak, Azure Medya Hizmetleri'nde içinde iki dosya bulunan bir varlık hazırlayın: video dosyası ve logo. Bunu .NET veya REST API'yi kullanarak yapabilirsiniz. Bunu, Azure portalını veya [Azure Medya Hizmetleri Gezgini'ni](https://github.com/Azure/Azure-Media-Services-Explorer) (AMSE) kullanarak da yapabilirsiniz.

Bu öğretici, AMSE ile varlıkları nasıl yöneteceğimize gösterilmektedir. Bir varlığa dosya eklemenin iki yolu vardır:

* Yerel bir klasör oluşturun, iki dosyayı kopyalayın ve klasörü sürükleyip **Varlık** sekmesine bırakın.
* Video dosyasını varlık olarak yükleyin, varlık bilgilerini görüntüleyin, dosyalar sekmesine gidin ve ek bir dosya (logo) yükleyin.

> [!NOTE]
> Kıymette (ana video dosyası) birincil dosyayı ayarladıklı olun.

![AMSE'deki varlık dosyaları](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture18_assetinamse.png)

*AMSE'deki varlık dosyaları*

Varlığı seçin ve Premium Kodlayıcı ile kodlamayı seçin. İş akışını yükleyin ve seçin.

Verileri işlemciye aktarmak için düğmeyi tıklatın ve çalışma zamanı özelliklerini ayarlamak için aşağıdaki XML'i ekleyin:

![AMSE'de Premium Kodlayıcı](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture19_amsepremium.png)

*AMSE'de Premium Kodlayıcı*

Ardından, aşağıdaki XML verilerini yapıştırın. Hem Medya Dosyası Girişi hem de primarySourceFile için video dosyasının adını belirtmeniz gerekir. Logo için dosya adının adını da belirtin.

```xml
<?xml version="1.0" encoding="utf-16"?>
  <transcodeRequest>
    <setRuntimeProperties>
      <property propertyPath="Media File Input/filename" value="Microsoft_HoloLens_Possibilities_816p24.mp4" />
      <property propertyPath="/primarySourceFile" value="Microsoft_HoloLens_Possibilities_816p24.mp4" />
      <property propertyPath="Media File Input Logo/filename" value="logo.png" />
    </setRuntimeProperties>
  </transcodeRequest>
```

![setRuntimeProperties](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture20_amsexmldata.png)

*setRuntimeProperties*

Görevi oluşturmak ve çalıştırmak için .NET SDK'yı kullanırsanız, bu XML verilerinin yapılandırma dizesi olarak geçirilmesi gerekir.

```csharp
public ITask AddNew(string taskName, IMediaProcessor mediaProcessor, string configuration, TaskOptions options);
```

İş tamamlandıktan sonra, çıkış varlığındaki MP4 dosyası bindirmeyi görüntüler!

![Videodaki yer kaplaması](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture21_resultoverlay.png)

*Videodaki yer kaplaması*

Örnek iş akışını [GitHub'dan](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows/)indirebilirsiniz.

## <a name="example-2--multiple-audio-language-encoding"></a>Örnek 2 : Birden çok ses dili kodlaması

Birden çok ses dili kodlama iş akışı örneği [GitHub](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows/MultilanguageAudioEncoding)mevcuttur.

Bu klasör, bir MXF dosyasını birden çok ses parçası içeren çok MP4 dosyaları varlığa kodlamak için kullanılabilecek örnek bir iş akışı içerir.

Bu iş akışı, MXF dosyasının tek bir ses parçası içerdiğini varsayar; ek ses parçaları ayrı ses dosyaları (WAV veya MP4...) olarak geçirilmelidir.

Kodlamak için lütfen aşağıdaki adımları izleyin:

* MXF dosyası ve Ses dosyaları (0 - 18 ses dosyası) ile bir Medya Hizmetleri varlık oluşturun.
* MXF dosyasının birincil dosya olarak ayarlandıkdığından emin olun.
* Premium İş Akışı Encoder işlemcisini kullanarak bir iş ve görev oluşturun. Sağlanan iş akışını kullanın (MultiMP4-1080p-19audio-v1.iş akışı).
* setruntime.xml verilerini göreve aktarın (Azure Medya Hizmetleri Gezgini kullanıyorsanız, "xml verilerini iş akışına geçir" düğmesini kullanın).
  * Doğru dosya adlarını ve dil etiketlerini belirtmek için lütfen XML verilerini güncelleştirin.
  * İş akışında Ses 1'den Ses 18'e kadar ses bileşenleri vardır.
  * RFC5646 dil etiketi için desteklenir.

```xml
<?xml version="1.0" encoding="utf-16"?>
<transcodeRequest>
  <setRuntimeProperties>
    <property propertyPath="Media File Input Video/filename" value="MainVideo.mxf" />
    <property propertyPath="Language/language_code" value="en" />
    <property propertyPath="/primarySourceFile" value="MainVideo.mxf" />
    <property propertyPath="Audio 1/Media File Input/filename" value="french-audio.wav" />
    <property propertyPath="Audio 1/Language/language_code" value="fr" />
    <property propertyPath="Audio 2/Media File Input/filename" value="german-audio.wav" />
    <property propertyPath="Audio 2/Language/language_code" value="de" />
    <property propertyPath="Audio 3/Media File Input/filename" value="japanese-audio.wav" />
    <property propertyPath="Audio 3/Language/language_code" value="ja" />
  </setRuntimeProperties>
</transcodeRequest>
```

* Kodlanmış varlık çok dilli ses parçaları içerir ve bu parçalar Azure Media Player'da seçilebilir olmalıdır.

## <a name="see-also"></a>Ayrıca bkz.
* [Azure Medya Hizmetlerinde Premium Kodlama Ile Tanış](https://azure.microsoft.com/blog/2015/03/05/introducing-premium-encoding-in-azure-media-services)
* [Azure Medya Hizmetlerinde Premium Kodlama nasıl kullanılır?](https://azure.microsoft.com/blog/2015/03/06/how-to-use-premium-encoding-in-azure-media-services)
* [Azure Medya Hizmetleri ile isteğe bağlı içeriği kodlama](media-services-encode-asset.md#media-encoder-premium-workflow)
* [Media Encoder Premium İş Akışı biçimleri ve codec'leri](media-services-premium-workflow-encoder-formats.md)
* [Örnek iş akışı dosyaları](https://github.com/Azure/azure-media-services-samples)
* [Azure Media Services Gezgini aracı](https://aka.ms/amse)

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]
