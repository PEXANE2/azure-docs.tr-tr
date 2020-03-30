---
title: Sorunsuz Akış Windows Mağazası Uygulama Öğretici | Microsoft Dokümanlar
description: Düzgün Akış içeriğini oynatmak için XML MediaElement denetimine sahip bir C# Windows Mağazası uygulaması oluşturmak için Azure Medya Hizmetlerini nasıl kullanacağınızı öğrenin.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.assetid: 0fa5d8c5-3d5f-4886-ae55-fb6de4f5256d
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/14/2019
ms.author: juliako
ms.openlocfilehash: 9ff961638aa170948d51793a21e86d18dd7e1d80
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "69016785"
---
# <a name="how-to-build-a-smooth-streaming-windows-store-application"></a>Düzgün Akışlı Windows Mağazası Uygulaması Nasıl Yapılsın?  

Windows 8 için Düzgün Akış İstemci SDK, geliştiricilerin isteğe bağlı olarak oynatabilen ve Düzgün Akış içeriği yaşayabilen Windows Mağazası uygulamaları oluşturmasına olanak tanır. SDK, Sorunsuz Akış içeriğinin temel oynatıntLığına ek olarak, Microsoft PlayReady koruması, kalite düzeyi kısıtlaması, Canlı DVR, ses akışı anahtarlama, durum güncelleştirmelerini dinleme (kalite düzeyi değişiklikleri gibi) ve hata olayları ve benzeri. Desteklenen özellikler hakkında daha fazla bilgi için [sürüm notlarına](https://www.iis.net/learn/media/smooth-streaming/smooth-streaming-client-sdk-for-windows-8-release-notes)bakın. Daha fazla bilgi [için Windows 8 için Player Framework'e](https://playerframework.codeplex.com/)bakın. 

Bu öğretici dört ders içerir:

1. Temel Bir Düzgün Akış Deposu Uygulaması Oluşturma
2. Ortam İlerlemesini Denetlemek için Kaydırıcı Çubuğu Ekleme
3. Düzgün Akış Akışları'nı seçin
4. Düzgün Akış Parçalarını Seçin

## <a name="prerequisites"></a>Ön koşullar
> [!NOTE]
> Windows Mağazası'nın 8.1 ve önceki sürüm projeleri Visual Studio 2017'de desteklenmez.  Daha fazla bilgi için bkz. [Visual Studio 2017 Platform Desteği ve Uyumluluk](https://www.visualstudio.com/en-us/productinfo/vs2017-compatibility-vs).

* Windows 8 32-bit veya 64-bit.
* Visual Studio sürümleri 2012 ile 2015.
* [Windows 8 için Microsoft Smooth Streaming İstemci SDK.](https://visualstudiogallery.msdn.microsoft.com/04423d13-3b3e-4741-a01c-1ae29e84fea6?SRC=Home)

Her ders için tamamlanan çözüm MSDN Geliştirici Kodu Örneklerinden (Code Gallery) indirilebilir: 

* [Ders 1](https://code.msdn.microsoft.com/Smooth-Streaming-Client-0bb1471f) - Basit bir Windows 8 Düzgün AkışLı Medya Oynatıcı, 
* [Ders 2](https://code.msdn.microsoft.com/A-simple-Windows-8-Smooth-ee98f63a) - Basit Bir Windows 8 Düz AkışLı Medya Oynatıcı sıyrık Çubuğu Kontrolü, 
* [Ders 3](https://code.msdn.microsoft.com/A-Windows-8-Smooth-883c3b44) - Akış Seçimi ile Windows 8 Düzgün Akışlı Medya Oynatıcı,  
* [Ders 4](https://code.msdn.microsoft.com/A-Windows-8-Smooth-aa9e4907) - Parça Seçimi ile Windows 8 Düzgün Akışlı Medya Oynatıcı.

## <a name="lesson-1-create-a-basic-smooth-streaming-store-application"></a>Ders 1: Temel Bir Düzgün Akış Deposu Uygulaması Oluşturma

Bu derste, Düzgün Akış içeriğini oynatmak için MediaElement denetimine sahip bir Windows Mağazası uygulaması oluşturursunuz.  Çalışan uygulama gibi görünüyor:

![Düzgün Akışlı Windows Mağazası uygulama örneği][PlayerApplication]

Windows Mağazası uygulamasını geliştirme hakkında daha fazla bilgi için windows [8 için Harika Uygulamalar Geliştir'e](https://msdn.microsoft.com/windows/apps/br229512.aspx)bakın. Bu ders aşağıdaki yordamları içerir:

1. Windows Mağazası projesi oluşturma
2. Kullanıcı arabirimini tasarla (XAML)
3. Dosyanın arkasındaki kodu değiştirme
4. Uygulamayı derleme ve test edin

### <a name="to-create-a-windows-store-project"></a>Windows Mağazası projesi oluşturmak için

1. Görsel Stüdyo çalıştırın; sürümleri 2012 ile 2015 desteklenir.
1. **DOSYA** menüsünde **Yeni**’ye ve sonra **Proje**’ye tıklayın.
1. Yeni Proje iletişim kutusundan aşağıdaki değerleri yazın veya seçin:

    | Adı | Değer |
    | --- | --- |
    | Şablon grubu |Yüklü/Şablonlar/Visual C#/Windows Mağazası |
    | Şablon |Boş Uygulama (XAML) |
    | Adı |SSPlayer |
    | Konum |C:\SSTutorials |
    | Çözüm Adı |SSPlayer |
    | Çözüm için dizin oluşturma |(seçilmiş) |

1. **Tamam**'a tıklayın.

### <a name="to-add-a-reference-to-the-smooth-streaming-client-sdk"></a>Düzgün Akış İstemci SDK'ya başvuru eklemek için

1. Solution Explorer'dan **SSPlayer'a**sağ tıklayın ve ardından **Başvuru Ekle'yi**tıklatın.
1. Aşağıdaki değerleri yazın veya seçin:

    | Adı | Değer |
    | --- | --- |
    | Referans grubu |Windows/Uzantılar |
    | Başvuru |Windows 8 ve Microsoft Visual C++ Çalışma Zamanı Paketi için Microsoft Smooth Streaming İstemci SDK'yi seçin |

1. **Tamam**'a tıklayın. 

Referansları ekledikten sonra, hedeflenen platformu (x64 veya x86) seçmeniz gerekir, ekleme referansları herhangi bir CPU platformu yapılandırması için çalışmaz.  Çözüm gezgininde, bu eklenen başvurular için sarı uyarı işareti görürsünüz.

### <a name="to-design-the-player-user-interface"></a>Oyuncu kullanıcı arabirimini tasarlamak için

1. Solution Explorer'dan, tasarım görünümünde açmak için **MainPage.xaml'ı** çift tıklatın.
2. ** &lt;Izgara&gt; ** ve ** &lt;&gt; /Izgara** etiketlerini XAML dosyasını bulun ve iki etiket arasına aşağıdaki kodu yapıştırın:

   ```xml
         <Grid.RowDefinitions>

            <RowDefinition Height="20"/>    <!-- spacer -->
            <RowDefinition Height="50"/>    <!-- media controls -->
            <RowDefinition Height="100*"/>  <!-- media element -->
            <RowDefinition Height="80*"/>   <!-- media stream and track selection -->
            <RowDefinition Height="50"/>    <!-- status bar -->
         </Grid.RowDefinitions>

         <StackPanel Name="spMediaControl" Grid.Row="1" Orientation="Horizontal">
            <TextBlock x:Name="tbSource" Text="Source :  " FontSize="16" FontWeight="Bold" VerticalAlignment="Center" />
            <TextBox x:Name="txtMediaSource" Text="https://ecn.channel9.msdn.com/o9/content/smf/smoothcontent/elephantsdream/Elephants_Dream_1024-h264-st-aac.ism/manifest" FontSize="10" Width="700" Margin="0,4,0,10" />
            <Button x:Name="btnSetSource" Content="Set Source" Width="111" Height="43" Click="btnSetSource_Click"/>
            <Button x:Name="btnPlay" Content="Play" Width="111" Height="43" Click="btnPlay_Click"/>
            <Button x:Name="btnPause" Content="Pause"  Width="111" Height="43" Click="btnPause_Click"/>
            <Button x:Name="btnStop" Content="Stop"  Width="111" Height="43" Click="btnStop_Click"/>
            <CheckBox x:Name="chkAutoPlay" Content="Auto Play" Height="55" Width="Auto" IsChecked="{Binding AutoPlay, ElementName=mediaElement, Mode=TwoWay}"/>
            <CheckBox x:Name="chkMute" Content="Mute" Height="55" Width="67" IsChecked="{Binding IsMuted, ElementName=mediaElement, Mode=TwoWay}"/>
         </StackPanel>

         <StackPanel Name="spMediaElement" Grid.Row="2" Height="435" Width="1072"
                    HorizontalAlignment="Center" VerticalAlignment="Center">
            <MediaElement x:Name="mediaElement" Height="356" Width="924" MinHeight="225"
                          HorizontalAlignment="Center" VerticalAlignment="Center" 
                          AudioCategory="BackgroundCapableMedia" />
            <StackPanel Orientation="Horizontal">
                <Slider x:Name="sliderProgress" Width="924" Height="44"
                        HorizontalAlignment="Center" VerticalAlignment="Center"
                        PointerPressed="sliderProgress_PointerPressed"/>
                <Slider x:Name="sliderVolume" 
                        HorizontalAlignment="Right" VerticalAlignment="Center" Orientation="Vertical" 
                        Height="79" Width="148" Minimum="0" Maximum="1" StepFrequency="0.1" 
                        Value="{Binding Volume, ElementName=mediaElement, Mode=TwoWay}" 
                        ToolTipService.ToolTip="{Binding Value, RelativeSource={RelativeSource Mode=Self}}"/>
            </StackPanel>
         </StackPanel>

         <StackPanel Name="spStatus" Grid.Row="4" Orientation="Horizontal">
            <TextBlock x:Name="tbStatus" Text="Status :  " 
               FontSize="16" FontWeight="Bold" VerticalAlignment="Center" HorizontalAlignment="Center" />
            <TextBox x:Name="txtStatus" FontSize="10" Width="700" VerticalAlignment="Center"/>
         </StackPanel>
   ```
   MediaElement denetimi medyayı oynatmak için kullanılır. Kaydırıcı denetimi, medya ilerlemesini denetlemek için bir sonraki derste kullanılacaktır.
3. Dosyayı kaydetmek için **CTRL+S** tuşuna basın.

MediaElement denetimi, kutudan düzgün akış içeriğini desteklemez. Düzgün Akış desteğini etkinleştirmek için, Düzgün Akış bayt akışı işleyicisini dosya adı uzantısı ve MIME türüne göre kaydetmeniz gerekir.  Kaydolmak için, Windows.Media ad alanının MediaExtensionManager.RegisterByteStreamHandler yöntemini kullanırsınız.

Bu XAML dosyasında, bazı olay işleyicileri denetimlerle ilişkilidir.  Bu olay işleyicileri tanımlamanız gerekir.

### <a name="to-modify-the-code-behind-file"></a>Dosyanın arkasındaki kodu değiştirmek için

1. Solution Explorer'dan **MainPage.xaml'a**sağ tıklayın ve ardından **Kodu Görüntüle'yi**tıklatın.
2. Dosyanın üst kısmında aşağıdaki leri kullanarak ifade ekleyin:
   
        using Windows.Media;
3. **MainPage** sınıfının başında aşağıdaki veri üyesini ekleyin:
   
         private MediaExtensionManager extensions = new MediaExtensionManager();
4. **MainPage** oluşturucusu sonunda, aşağıdaki iki satır ekleyin:
   
        extensions.RegisterByteStreamHandler("Microsoft.Media.AdaptiveStreaming.SmoothByteStreamHandler", ".ism", "text/xml");
        extensions.RegisterByteStreamHandler("Microsoft.Media.AdaptiveStreaming.SmoothByteStreamHandler", ".ism", "application/vnd.ms-sstr+xml");
5. **MainPage** sınıfının sonunda, aşağıdaki kodu yapıştırın:
   ```csharp
         # region UI Button Click Events
         private void btnPlay_Click(object sender, RoutedEventArgs e)
         {

         mediaElement.Play();
         txtStatus.Text = "MediaElement is playing ...";
         }
         private void btnPause_Click(object sender, RoutedEventArgs e)
         {

         mediaElement.Pause();
         txtStatus.Text = "MediaElement is paused";
         }
         private void btnSetSource_Click(object sender, RoutedEventArgs e)
         {

         sliderProgress.Value = 0;
         mediaElement.Source = new Uri(txtMediaSource.Text);

         if (chkAutoPlay.IsChecked == true)
         {
             txtStatus.Text = "MediaElement is playing ...";
         }
         else
         {
             txtStatus.Text = "Click the Play button to play the media source.";
         }
         }
         private void btnStop_Click(object sender, RoutedEventArgs e)
         {

         mediaElement.Stop();
         txtStatus.Text = "MediaElement is stopped";
         }
         private void sliderProgress_PointerPressed(object sender, PointerRoutedEventArgs e)
         {

         txtStatus.Text = "Seek to position " + sliderProgress.Value;
         mediaElement.Position = new TimeSpan(0, 0, (int)(sliderProgress.Value));
         }
         # endregion
   ```
   sliderProgress_PointerPressed olay işleyicisi burada tanımlanır.  Bu öğretici nin bir sonraki derste ele alınacaktır çalışma, almak için yapılacak daha fazla iş vardır.
6. Dosyayı kaydetmek için **CTRL+S** tuşuna basın.

Dosyanın arkasındaki kodu bitmiş aşağıdaki gibi görünecektir:

![Düzgün Akışlı Windows Mağazası uygulamasıvisual Studio Codeview][CodeViewPic]

### <a name="to-compile-and-test-the-application"></a>Uygulamayı derlemek ve test etmek için

1. **BUILD** menüsünden **Configuration Manager'ı**tıklatın.
2. Geliştirme platformunuza uyacak şekilde **Etkin çözüm platformunu** değiştirin.
3. Projeyi derlemek için **F6** tuşuna basın. 
4. Uygulamayı çalıştırmak için **F5**'e basın.
5. Uygulamanın üst kısmında, varsayılan Düzgün Akış URL'sini kullanabilir veya farklı bir URL girebilirsiniz. 
6. **Kaynak Ayarla'yı**tıklatın. **Otomatik Oynatma** varsayılan olarak etkinleştirildiğinden, ortam otomatik olarak oynatacaktır.  **Oynat,** **Duraklat** ve **Durdur** düğmelerini kullanarak ortamı kontrol edebilirsiniz.  Dikey kaydırıcıyı kullanarak ortam düzeyini kontrol edebilirsiniz.  Ancak medya ilerlemesini denetlemek için yatay kaydırıcı henüz tam olarak uygulanmamıştır. 

Dersi tamamladınız1.  Bu derste, Düzgün Akış içeriğini oynatmak için bir MediaElement denetimi kullanırsınız.  Bir sonraki derste, Düzgün Akış içeriğinin ilerlemesini denetlemek için bir kaydırıcı eklersiniz.

## <a name="lesson-2-add-a-slider-bar-to-control-the-media-progress"></a>Ders 2: Medya İlerlemesini Denetlemek için Kaydırıcı Çubuğu Ekleme

Ders 1'de, Düzgün Akış lı ortam içeriğini oynatmak için MediaElement XAML denetimine sahip bir Windows Mağazası uygulaması oluşturdunuz.  Başlat, durdur ve duraklatma gibi bazı temel medya işlevleri gelir.  Bu derste, uygulamaya bir kaydırıcı çubuğu denetimi eklersiniz.

Bu öğreticide, MediaElement denetiminin geçerli konumunu temel alan kaydırıcı konumunu güncelleştirmek için bir zamanlayıcı kullanacağız.  Kaydırıcı başlangıç ve bitiş saatinin de canlı içerik durumunda güncellenmesi gerekir.  Bu, uyarlanabilir kaynak güncelleştirme olayında daha iyi işlenebilir.

Medya kaynakları, medya verileri oluşturan nesnelerdir.  Kaynak çözümleyicibir URL veya bayt akışı alır ve bu içerik için uygun ortam kaynağını oluşturur.  Kaynak çözümleyici, uygulamaların ortam kaynakları oluşturmasının standart yoludur. 

Bu ders aşağıdaki yordamları içerir:

1. Düzgün Akış işleyicisini kaydedin 
2. Uyarlanabilir kaynak yöneticisi düzeyi olay işleyicileri ekleme
3. Uyarlanabilir kaynak düzeyi olay işleyicileri ekleme
4. MediaElement olay işleyicileri ekleme
5. Kaydırıcı çubuğuyla ilgili kod ekleme
6. Uygulamayı derleme ve test edin

### <a name="to-register-the-smooth-streaming-byte-stream-handler-and-pass-the-propertyset"></a>Düzgün Akış bayt akışı işleyicisini kaydetmek ve özellik kümesini geçmek için

1. Solution Explorer'dan **MainPage.xaml'ı**sağ tıklatın ve ardından **Kodu Görüntüle'yi**tıklatın.
2. Dosyanın başında aşağıdaki leri kullanarak ifade yi ekleyin:

   ```csharp
        using Microsoft.Media.AdaptiveStreaming;
   ```
3. MainPage sınıfının başında aşağıdaki veri üyelerini ekleyin:

   ```csharp
         private Windows.Foundation.Collections.PropertySet propertySet = new Windows.Foundation.Collections.PropertySet();             
         private IAdaptiveSourceManager adaptiveSourceManager;
   ```
4. **MainPage** oluşturucunun içine, bundan sonra aşağıdaki kodu **ekleyin. Bileşenleri Başlatma();** satır ve bir önceki derste yazılı kayıt kodu satırları:

   ```csharp
        // Gets the default instance of AdaptiveSourceManager which manages Smooth 
        //Streaming media sources.
        adaptiveSourceManager = AdaptiveSourceManager.GetDefault();
        // Sets property key value to AdaptiveSourceManager default instance.
        // {A5CE1DE8-1D00-427B-ACEF-FB9A3C93DE2D}" must be hardcoded.
        propertySet["{A5CE1DE8-1D00-427B-ACEF-FB9A3C93DE2D}"] = adaptiveSourceManager;
   ```
5. **MainPage** oluşturucuiçinde, ileri parametreleri eklemek için iki RegisterByteStreamHandler yöntemini değiştirin:

   ```csharp
         // Registers Smooth Streaming byte-stream handler for ".ism" extension and, 
         // "text/xml" and "application/vnd.ms-ss" mime-types and pass the propertyset. 
         // http://*.ism/manifest URI resources will be resolved by Byte-stream handler.
         extensions.RegisterByteStreamHandler(

            "Microsoft.Media.AdaptiveStreaming.SmoothByteStreamHandler", 
            ".ism", 
            "text/xml", 
            propertySet );
         extensions.RegisterByteStreamHandler(

            "Microsoft.Media.AdaptiveStreaming.SmoothByteStreamHandler", 
            ".ism", 
            "application/vnd.ms-sstr+xml", 
         propertySet);
   ```
6. Dosyayı kaydetmek için **CTRL+S** tuşuna basın.

### <a name="to-add-the-adaptive-source-manager-level-event-handler"></a>Uyarlanabilir kaynak yöneticisi düzeyi olay işleyicisi eklemek için

1. Solution Explorer'dan **MainPage.xaml'ı**sağ tıklatın ve ardından **Kodu Görüntüle'yi**tıklatın.
2. **MainPage** sınıfının içinde aşağıdaki veri üyesini ekleyin:

   ```csharp
     private AdaptiveSource adaptiveSource = null;
   ```
3. **MainPage** sınıfının sonunda aşağıdaki olay işleyicisini ekleyin:

   ```csharp
         # region Adaptive Source Manager Level Events
         private void mediaElement_AdaptiveSourceOpened(AdaptiveSource sender, AdaptiveSourceOpenedEventArgs args)
         {

            adaptiveSource = args.AdaptiveSource;
         }

         # endregion Adaptive Source Manager Level Events
   ```
4. **MainPage** oluşturucusu sonunda, uyarlanabilir kaynak açık olay abone olmak için aşağıdaki satırı ekleyin:

   ```csharp
         adaptiveSourceManager.AdaptiveSourceOpenedEvent += 
           new AdaptiveSourceOpenedEventHandler(mediaElement_AdaptiveSourceOpened);
   ```
5. Dosyayı kaydetmek için **CTRL+S** tuşuna basın.

### <a name="to-add-adaptive-source-level-event-handlers"></a>Uyarlanabilir kaynak düzeyi olay işleyicileri eklemek için

1. Solution Explorer'dan **MainPage.xaml'ı**sağ tıklatın ve ardından **Kodu Görüntüle'yi**tıklatın.
2. **MainPage** sınıfının içinde aşağıdaki veri üyesini ekleyin:

   ```csharp
     private AdaptiveSourceStatusUpdatedEventArgs adaptiveSourceStatusUpdate; 
     private Manifest manifestObject;
   ```
3. **MainPage** sınıfının sonunda aşağıdaki olay işleyicilerini ekleyin:

   ```csharp
         # region Adaptive Source Level Events
         private void mediaElement_ManifestReady(AdaptiveSource sender, ManifestReadyEventArgs args)
         {

            adaptiveSource = args.AdaptiveSource;
            manifestObject = args.AdaptiveSource.Manifest;
         }

         private void mediaElement_AdaptiveSourceStatusUpdated(AdaptiveSource sender, AdaptiveSourceStatusUpdatedEventArgs args)
         {

            adaptiveSourceStatusUpdate = args;
         }

         private void mediaElement_AdaptiveSourceFailed(AdaptiveSource sender, AdaptiveSourceFailedEventArgs args)
         {

            txtStatus.Text = "Error: " + args.HttpResponse;
         }

         # endregion Adaptive Source Level Events
   ```
4. **mediaElement AdaptiveSourceOpened** yönteminin sonunda, olaylara abone olmak için aşağıdaki kodu ekleyin:

   ```csharp
         adaptiveSource.ManifestReadyEvent +=

                    mediaElement_ManifestReady;
         adaptiveSource.AdaptiveSourceStatusUpdatedEvent += 

            mediaElement_AdaptiveSourceStatusUpdated;
         adaptiveSource.AdaptiveSourceFailedEvent += 

            mediaElement_AdaptiveSourceFailed;
   ```
5. Dosyayı kaydetmek için **CTRL+S** tuşuna basın.

Aynı olaylar, uygulamadaki tüm medya öğelerinin ortak işlevselliğini işlemek için kullanılabilen Uyarlanabilir Kaynak yemlik düzeyinde de kullanılabilir. Her AdaptiveSource kendi etkinliklerini içerir ve tüm AdaptiveSource etkinlikleri AdaptiveSourceManager altında basamaklanacaktır.

### <a name="to-add-media-element-event-handlers"></a>Media Element olay işleyicileri eklemek için

1. Solution Explorer'dan **MainPage.xaml'ı**sağ tıklatın ve ardından **Kodu Görüntüle'yi**tıklatın.
2. **MainPage** sınıfının sonunda aşağıdaki olay işleyicilerini ekleyin:

   ```csharp
         # region Media Element Event Handlers
         private void MediaOpened(object sender, RoutedEventArgs e)
         {

            txtStatus.Text = "MediaElement opened";
         }

         private void MediaFailed(object sender, ExceptionRoutedEventArgs e)
         {

            txtStatus.Text= "MediaElement failed: " + e.ErrorMessage;
         }

         private void MediaEnded(object sender, RoutedEventArgs e)
         {

            txtStatus.Text ="MediaElement ended.";
         }

         # endregion Media Element Event Handlers
   ```
3. **MainPage** oluşturucusu sonunda, olaylara alt yazı için aşağıdaki kodu ekleyin:

   ```csharp
         mediaElement.MediaOpened += MediaOpened;
         mediaElement.MediaEnded += MediaEnded;
         mediaElement.MediaFailed += MediaFailed;
   ```
4. Dosyayı kaydetmek için **CTRL+S** tuşuna basın.

### <a name="to-add-slider-bar-related-code"></a>Kaydırıcı çubuğuyla ilgili kod eklemek için

1. Solution Explorer'dan **MainPage.xaml'ı**sağ tıklatın ve ardından **Kodu Görüntüle'yi**tıklatın.
2. Dosyanın başında aşağıdaki leri kullanarak ifade yi ekleyin:

   ```csharp
        using Windows.UI.Core;
   ```
3. **MainPage** sınıfının içinde aşağıdaki veri üyelerini ekleyin:

   ```csharp
         public static CoreDispatcher _dispatcher;
         private DispatcherTimer sliderPositionUpdateDispatcher;
   ```
4. **MainPage** oluşturucusu sonunda, aşağıdaki kodu ekleyin:

   ```csharp
         _dispatcher = Window.Current.Dispatcher;
         PointerEventHandler pointerpressedhandler = new PointerEventHandler(sliderProgress_PointerPressed);
         sliderProgress.AddHandler(Control.PointerPressedEvent, pointerpressedhandler, true);    
   ```
5. **MainPage** sınıfının sonunda aşağıdaki kodu ekleyin:

   ```csharp
         # region sliderMediaPlayer
         private double SliderFrequency(TimeSpan timevalue)
         {

            long absvalue = 0;
            double stepfrequency = -1;

            if (manifestObject != null)
            {
                absvalue = manifestObject.Duration - (long)manifestObject.StartTime;
            }
            else
            {
                absvalue = mediaElement.NaturalDuration.TimeSpan.Ticks;
            }

            TimeSpan totalDVRDuration = new TimeSpan(absvalue);

            if (totalDVRDuration.TotalMinutes >= 10 && totalDVRDuration.TotalMinutes < 30)
            {
               stepfrequency = 10;
            }
            else if (totalDVRDuration.TotalMinutes >= 30 
                     && totalDVRDuration.TotalMinutes < 60)
            {
                stepfrequency = 30;
            }
            else if (totalDVRDuration.TotalHours >= 1)
            {
                stepfrequency = 60;
            }

            return stepfrequency;
         }

         void updateSliderPositionoNTicks(object sender, object e)
         {

            sliderProgress.Value = mediaElement.Position.TotalSeconds;
         }

         public void setupTimer()
         {

            sliderPositionUpdateDispatcher = new DispatcherTimer();
            sliderPositionUpdateDispatcher.Interval = new TimeSpan(0, 0, 0, 0, 300);
            startTimer();
         }

         public void startTimer()
         {

            sliderPositionUpdateDispatcher.Tick += updateSliderPositionoNTicks;
            sliderPositionUpdateDispatcher.Start();
         }

         // Slider start and end time must be updated in case of live content
         public async void setSliderStartTime(long startTime)
         {

            await _dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
            {
                TimeSpan timespan = new TimeSpan(adaptiveSourceStatusUpdate.StartTime);
                double absvalue = (int)Math.Round(timespan.TotalSeconds, MidpointRounding.AwayFromZero);
                sliderProgress.Minimum = absvalue;
            });
         }

         // Slider start and end time must be updated in case of live content
         public async void setSliderEndTime(long startTime)
         {

            await _dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
            {
                TimeSpan timespan = new TimeSpan(adaptiveSourceStatusUpdate.EndTime);
                double absvalue = (int)Math.Round(timespan.TotalSeconds, MidpointRounding.AwayFromZero);
                sliderProgress.Maximum = absvalue;
            });
         }

         # endregion sliderMediaPlayer
   ```

   > [!NOTE]
   > CoreDispatcher olmayan UI Iş parçacığı gelen UI iş parçacığı değişiklikleri yapmak için kullanılır. Sevk irsaliyesi iş parçacığıüzerinde darboğaz durumunda, geliştirici güncelleştirmek niyetinde ui-öğesi tarafından sağlanan sevk kullanarak seçebilirsiniz.  Örnek:

   ```csharp
         await sliderProgress.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () => { TimeSpan 

         timespan = new TimeSpan(adaptiveSourceStatusUpdate.EndTime); 
         double absvalue  = (int)Math.Round(timespan.TotalSeconds, MidpointRounding.AwayFromZero); 

         sliderProgress.Maximum = absvalue; }); 
   ```
6. **mediaElement_AdaptiveSourceStatusUpdated** yönteminin sonunda aşağıdaki kodu ekleyin:

   ```csharp
         setSliderStartTime(args.StartTime);
         setSliderEndTime(args.EndTime);
   ```
7. **MediaOpened** yönteminin sonunda aşağıdaki kodu ekleyin:

   ```csharp
         sliderProgress.StepFrequency = SliderFrequency(mediaElement.NaturalDuration.TimeSpan);
         sliderProgress.Width = mediaElement.Width;
         setupTimer();
   ```
8. Dosyayı kaydetmek için **CTRL+S** tuşuna basın.

### <a name="to-compile-and-test-the-application"></a>Uygulamayı derlemek ve test etmek için

1. Projeyi derlemek için **F6** tuşuna basın. 
2. Uygulamayı çalıştırmak için **F5**'e basın.
3. Uygulamanın üst kısmında, varsayılan Düzgün Akış URL'sini kullanabilir veya farklı bir URL girebilirsiniz. 
4. **Kaynak Ayarla'yı**tıklatın. 
5. Kaydırıcı çubuğunu test edin.

Ders 2'yi tamamladınız.  Bu derste uygulamaya bir kaydırıcı eklediniz. 

## <a name="lesson-3-select-smooth-streaming-streams"></a>Ders 3: Düzgün Akış Akışlarını Seçin
Düzgün Akış, görüntüleyenler tarafından seçilebilen birden çok dil ses parçasıyla içerik akışı yapabilir.  Bu derste, görüntüleyenlerin akışları seçmesini sağlayacaksınız. Bu ders aşağıdaki yordamları içerir:

1. XAML dosyasını değiştirme
2. Dosyanın arkasındaki kodu değiştirme
3. Uygulamayı derleme ve test edin

### <a name="to-modify-the-xaml-file"></a>XAML dosyasını değiştirmek için

1. Solution Explorer'dan **MainPage.xaml'a**sağ tıklayın ve ardından **Tasarımcıyı Görüntüle'yi**tıklatın.
2. Grid.RowDefinitions'ı &lt;&gt;bulun ve RowDefinitions'ı aşağıdaki gibi görünerek değiştirin:

   ```xml
         <Grid.RowDefinitions>            
            <RowDefinition Height="20"/>
            <RowDefinition Height="50"/>
            <RowDefinition Height="100"/>
            <RowDefinition Height="80"/>
            <RowDefinition Height="50"/>
         </Grid.RowDefinitions>
   ```
3. Izgara&gt;&lt;/Izgara&gt; etiketlerinin &lt;içinde, kullanıcıların kullanılabilir akışların listesini görebilmeleri ve akışları seçebilmeleri için liste kutusu denetimini tanımlamak için aşağıdaki kodu ekleyin:

   ```xml
         <Grid Name="gridStreamAndBitrateSelection" Grid.Row="3">
            <Grid.RowDefinitions>
                <RowDefinition Height="300"/>
            </Grid.RowDefinitions>
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="250*"/>
                <ColumnDefinition Width="250*"/>
            </Grid.ColumnDefinitions>
            <StackPanel Name="spStreamSelection" Grid.Row="1" Grid.Column="0">
                <StackPanel Orientation="Horizontal">
                    <TextBlock Name="tbAvailableStreams" Text="Available Streams:" FontSize="16" VerticalAlignment="Center"></TextBlock>
                    <Button Name="btnChangeStreams" Content="Submit" Click="btnChangeStream_Click"/>
                </StackPanel>
                <ListBox x:Name="lbAvailableStreams" Height="200" Width="200" Background="Transparent" HorizontalAlignment="Left" 
                    ScrollViewer.VerticalScrollMode="Enabled" ScrollViewer.VerticalScrollBarVisibility="Visible">
                    <ListBox.ItemTemplate>
                        <DataTemplate>
                            <CheckBox Content="{Binding Name}" IsChecked="{Binding isChecked, Mode=TwoWay}" />
                        </DataTemplate>
                    </ListBox.ItemTemplate>
                </ListBox>
            </StackPanel>
         </Grid>
   ```
4. Değişiklikleri kaydetmek için **CTRL+S** tuşuna basın.

### <a name="to-modify-the-code-behind-file"></a>Dosyanın arkasındaki kodu değiştirmek için

1. Solution Explorer'dan **MainPage.xaml'a**sağ tıklayın ve ardından **Kodu Görüntüle'yi**tıklatın.
2. SSPlayer ad alanı nın içine yeni bir sınıf ekleyin:

   ```csharp
        #region class Stream
   
        public class Stream
        {
            private IManifestStream stream;
            public bool isCheckedValue;
            public string name;
   
            public string Name
            {
                get { return name; }
                set { name = value; }
            }
   
            public IManifestStream ManifestStream
            {
                get { return stream; }
                set { stream = value; }
            }
   
            public bool isChecked
            {
                get { return isCheckedValue; }
                set
                {
                    // mMke the video stream always checked.
                    if (stream.Type == MediaStreamType.Video)
                    {
                        isCheckedValue = true;
                    }
                    else
                    {
                        isCheckedValue = value;
                    }
                }
            }
   
            public Stream(IManifestStream streamIn)
            {
                stream = streamIn;
                name = stream.Name;
            }
        }
        #endregion class Stream
   ```
3. MainPage sınıfının başında aşağıdaki değişken tanımlarını ekleyin:

   ```csharp
         private List<Stream> availableStreams;
         private List<Stream> availableAudioStreams;
         private List<Stream> availableTextStreams;
         private List<Stream> availableVideoStreams;
   ```
4. MainPage sınıfının içine aşağıdaki bölgeyi ekleyin:
   ```csharp
        #region stream selection
        ///<summary>
        ///Functionality to select streams from IManifestStream available streams
        /// </summary>
   
        // This function is called from the mediaElement_ManifestReady event handler 
        // to retrieve the streams and populate them to the local data members.
        public void getStreams(Manifest manifestObject)
        {
            availableStreams = new List<Stream>();
            availableVideoStreams = new List<Stream>();
            availableAudioStreams = new List<Stream>();
            availableTextStreams = new List<Stream>();
   
            try
            {
                for (int i = 0; i<manifestObject.AvailableStreams.Count; i++)
                {
                    Stream newStream = new Stream(manifestObject.AvailableStreams[i]);
                    newStream.isChecked = false;
   
                    //populate the stream lists based on the types
                    availableStreams.Add(newStream);
   
                    switch (newStream.ManifestStream.Type)
                    {
                        case MediaStreamType.Video:
                            availableVideoStreams.Add(newStream);
                            break;
                        case MediaStreamType.Audio:
                            availableAudioStreams.Add(newStream);
                            break;
                        case MediaStreamType.Text:
                            availableTextStreams.Add(newStream);
                            break;
                    }
   
                    // Select the default selected streams from the manifest.
                    for (int j = 0; j<manifestObject.SelectedStreams.Count; j++)
                    {
                        string selectedStreamName = manifestObject.SelectedStreams[j].Name;
                        if (selectedStreamName.Equals(newStream.Name))
                        {
                            newStream.isChecked = true;
                            break;
                        }
                    }
                }
            }
            catch (Exception e)
            {
                txtStatus.Text = "Error: " + e.Message;
            }
        }
   
        // This function set the list box ItemSource
        private async void refreshAvailableStreamsListBoxItemSource()
        {
            try
            {
                //update the stream check box list on the UI
                await _dispatcher.RunAsync(CoreDispatcherPriority.Normal, ()
                    => { lbAvailableStreams.ItemsSource = availableStreams; });
            }
            catch (Exception e)
            {
                txtStatus.Text = "Error: " + e.Message;
            }
        }
   
        // This function creates a selected streams list
        private void createSelectedStreamsList(List<IManifestStream> selectedStreams)
        {
            bool isOneVideoSelected = false;
            bool isOneAudioSelected = false;
   
            // Only one video stream can be selected
            for (int j = 0; j<availableVideoStreams.Count; j++)
            {
                if (availableVideoStreams[j].isChecked && (!isOneVideoSelected))
                {
                    selectedStreams.Add(availableVideoStreams[j].ManifestStream);
                    isOneVideoSelected = true;
                }
            }
   
            // Select the first video stream from the list if no video stream is selected
            if (!isOneVideoSelected)
            {
                availableVideoStreams[0].isChecked = true;
                selectedStreams.Add(availableVideoStreams[0].ManifestStream);
            }
   
            // Only one audio stream can be selected
            for (int j = 0; j<availableAudioStreams.Count; j++)
            {
                if (availableAudioStreams[j].isChecked && (!isOneAudioSelected))
                {
                    selectedStreams.Add(availableAudioStreams[j].ManifestStream);
                    isOneAudioSelected = true;
                    txtStatus.Text = "The audio stream is changed to " + availableAudioStreams[j].ManifestStream.Name;
                }
            }
   
            // Select the first audio stream from the list if no audio steam is selected.
            if (!isOneAudioSelected)
            {
                availableAudioStreams[0].isChecked = true;
                selectedStreams.Add(availableAudioStreams[0].ManifestStream);
            }
   
            // Multiple text streams are supported.
            for (int j = 0; j < availableTextStreams.Count; j++)
            {
                if (availableTextStreams[j].isChecked)
                {
                    selectedStreams.Add(availableTextStreams[j].ManifestStream);
                }
            }
        }
   
        // Change streams on a smooth streaming presentation with multiple video streams.
        private async void changeStreams(List<IManifestStream> selectStreams)
        {
            try
            {
                IReadOnlyList<IStreamChangedResult> returnArgs =
                    await manifestObject.SelectStreamsAsync(selectStreams);
            }
            catch (Exception e)
            {
                txtStatus.Text = "Error: " + e.Message;
            }
        }
        #endregion stream selection
   ```
5. mediaElement_ManifestReady yöntemini bulun, işlevin sonunda aşağıdaki kodu ekle:
   ```csharp
        getStreams(manifestObject);
        refreshAvailableStreamsListBoxItemSource();
   ```
    MediaElement bildirimi hazır olduğunda, kod kullanılabilir akışların listesini alır ve listeyle birlikte UI liste kutusunu doldurur.
6. MainPage sınıfının içinde, UI düğmelerini bulun olaylar bölgesini tıklatın ve ardından aşağıdaki işlev tanımını ekleyin:
   ```csharp
        private void btnChangeStream_Click(object sender, RoutedEventArgs e)
        {
            List<IManifestStream> selectedStreams = new List<IManifestStream>();
   
            // Create a list of the selected streams
            createSelectedStreamsList(selectedStreams);
   
            // Change streams on the presentation
            changeStreams(selectedStreams);
        }
   ```

### <a name="to-compile-and-test-the-application"></a>Uygulamayı derlemek ve test etmek için

1. Projeyi derlemek için **F6** tuşuna basın. 
2. Uygulamayı çalıştırmak için **F5**'e basın.
3. Uygulamanın üst kısmında, varsayılan Düzgün Akış URL'sini kullanabilir veya farklı bir URL girebilirsiniz. 
4. **Kaynak Ayarla'yı**tıklatın. 
5. Varsayılan dil audio_eng. audio_eng ve audio_es arasında geçiş yapmaya çalışın. Her zaman, yeni bir akış seçin, Gönder düğmesini tıklatmanız gerekir.

3. dersi tamamladınız.  Bu derste, akışları seçmek için işlevselliği eklersiniz.

## <a name="lesson-4-select-smooth-streaming-tracks"></a>Ders 4: Düzgün Akış parçalarını seçin

Düzgün Akış sunusu, farklı kalite düzeyleri (bit hızları) ve çözünürlüklerle kodlanmış birden çok video dosyası içerebilir. Bu derste, kullanıcıların parçaları seçmelerini sağlayacaksınız. Bu ders aşağıdaki yordamları içerir:

1. XAML dosyasını değiştirme
2. Dosyanın arkasındaki kodu değiştirme
3. Uygulamayı derleme ve test edin

### <a name="to-modify-the-xaml-file"></a>XAML dosyasını değiştirmek için

1. Solution Explorer'dan **MainPage.xaml'a**sağ tıklayın ve ardından **Tasarımcıyı Görüntüle'yi**tıklatın.
2. &lt;&gt; **GridStreamAndBitrateSelection adlı Izgara**etiketini bulun, etiketin sonundaki aşağıdaki kodu ekleyin:
   ```xml
         <StackPanel Name="spBitRateSelection" Grid.Row="1" Grid.Column="1">
         <StackPanel Orientation="Horizontal">
             <TextBlock Name="tbBitRate" Text="Available Bitrates:" FontSize="16" VerticalAlignment="Center"/>
             <Button Name="btnChangeTracks" Content="Submit" Click="btnChangeTrack_Click" />
         </StackPanel>
         <ListBox x:Name="lbAvailableVideoTracks" Height="200" Width="200" Background="Transparent" HorizontalAlignment="Left" 
                  ScrollViewer.VerticalScrollMode="Enabled" ScrollViewer.VerticalScrollBarVisibility="Visible">
             <ListBox.ItemTemplate>
                 <DataTemplate>
                     <CheckBox Content="{Binding Bitrate}" IsChecked="{Binding isChecked, Mode=TwoWay}"/>
                 </DataTemplate>
             </ListBox.ItemTemplate>
         </ListBox>
         </StackPanel>
   ```
3. Değişiklikleri kaydetmek için **CTRL+S** tuşuna basın

### <a name="to-modify-the-code-behind-file"></a>Dosyanın arkasındaki kodu değiştirmek için

1. Solution Explorer'dan **MainPage.xaml'a**sağ tıklayın ve ardından **Kodu Görüntüle'yi**tıklatın.
2. SSPlayer ad alanı nın içine yeni bir sınıf ekleyin:
   ```csharp
        #region class Track
        public class Track
        {
            private IManifestTrack trackInfo;
            public string _bitrate;
            public bool isCheckedValue;
   
            public IManifestTrack TrackInfo
            {
                get { return trackInfo; }
                set { trackInfo = value; }
            }
   
            public string Bitrate
            {
                get { return _bitrate; }
                set { _bitrate = value; }
            }
   
            public bool isChecked
            {
                get { return isCheckedValue; }
                set
                {
                    isCheckedValue = value;
                }
            }
   
            public Track(IManifestTrack trackInfoIn)
            {
                trackInfo = trackInfoIn;
                _bitrate = trackInfoIn.Bitrate.ToString();
            }
            //public Track() { }
        }
        #endregion class Track
   ```
3. MainPage sınıfının başında aşağıdaki değişken tanımlarını ekleyin:
   ```csharp
        private List<Track> availableTracks;
   ```
4. MainPage sınıfının içine aşağıdaki bölgeyi ekleyin:
   ```csharp
        #region track selection
        /// <summary>
        /// Functionality to select video streams
        /// </summary>
   
        /// This Function gets the tracks for the selected video stream
        public void getTracks(Manifest manifestObject)
        {
            availableTracks = new List<Track>();
   
            IManifestStream videoStream = getVideoStream();
            IReadOnlyList<IManifestTrack> availableTracksLocal = videoStream.AvailableTracks;
            IReadOnlyList<IManifestTrack> selectedTracksLocal = videoStream.SelectedTracks;
   
            try
            {
                for (int i = 0; i < availableTracksLocal.Count; i++)
                {
                    Track thisTrack = new Track(availableTracksLocal[i]);
                    thisTrack.isChecked = true;
   
                    for (int j = 0; j < selectedTracksLocal.Count; j++)
                    {
                        string selectedTrackName = selectedTracksLocal[j].Bitrate.ToString();
                        if (selectedTrackName.Equals(thisTrack.Bitrate))
                        {
                            thisTrack.isChecked = true;
                            break;
                        }
                    }
                    availableTracks.Add(thisTrack);
                }
            }
            catch (Exception e)
            {
                txtStatus.Text = e.Message;
            }
        }
   
        // This function gets the video stream that is playing
        private IManifestStream getVideoStream()
        {
            IManifestStream videoStream = null;
            for (int i = 0; i < manifestObject.SelectedStreams.Count; i++)
            {
                if (manifestObject.SelectedStreams[i].Type == MediaStreamType.Video)
                {
                    videoStream = manifestObject.SelectedStreams[i];
                    break;
                }
            }
            return videoStream;
        }
   
        // This function set the UI list box control ItemSource
        private async void refreshAvailableTracksListBoxItemSource()
        {
            try
            {
                // Update the track check box list on the UI 
                await _dispatcher.RunAsync(CoreDispatcherPriority.Normal, ()
                    => { lbAvailableVideoTracks.ItemsSource = availableTracks; });
            }
            catch (Exception e)
            {
                txtStatus.Text = "Error: " + e.Message;
            }        
        }
   
        // This function creates a list of the selected tracks.
        private void createSelectedTracksList(List<IManifestTrack> selectedTracks)
        {
            // Create a list of selected tracks
            for (int j = 0; j < availableTracks.Count; j++)
            {
                if (availableTracks[j].isCheckedValue == true)
                {
                    selectedTracks.Add(availableTracks[j].TrackInfo);
                }
            }
        }
   
        // This function selects the tracks based on user selection 
        private void changeTracks(List<IManifestTrack> selectedTracks)
        {
            IManifestStream videoStream = getVideoStream();
            try
            {
                videoStream.SelectTracks(selectedTracks);
            }
            catch (Exception ex)
            {
                txtStatus.Text = ex.Message;
            }
        }
        #endregion track selection
   ```
5. mediaElement_ManifestReady yöntemini bulun, işlevin sonunda aşağıdaki kodu ekle:
   ```csharp
         getTracks(manifestObject);
         refreshAvailableTracksListBoxItemSource();
   ```
6. MainPage sınıfının içinde, UI düğmelerini bulun olaylar bölgesini tıklatın ve ardından aşağıdaki işlev tanımını ekleyin:
   ```csharp
         private void btnChangeStream_Click(object sender, RoutedEventArgs e)
         {
            List<IManifestStream> selectedStreams = new List<IManifestStream>();

            // Create a list of the selected streams
            createSelectedStreamsList(selectedStreams);

            // Change streams on the presentation
            changeStreams(selectedStreams);
         }
   ```
   
### <a name="to-compile-and-test-the-application"></a>Uygulamayı derlemek ve test etmek için

1. Projeyi derlemek için **F6** tuşuna basın. 
2. Uygulamayı çalıştırmak için **F5**'e basın.
3. Uygulamanın üst kısmında, varsayılan Düzgün Akış URL'sini kullanabilir veya farklı bir URL girebilirsiniz. 
4. **Kaynak Ayarla'yı**tıklatın. 
5. Varsayılan olarak, video akışının tüm parçaları seçilir. Bit hızı değişikliklerini denemek için, kullanılabilir en düşük bit hızını seçebilir ve ardından kullanılabilir en yüksek bit hızını seçebilirsiniz. Her değişiklikten sonra Gönder'i tıklatmanız gerekir.  Video kalitesi değişikliklerini görebilirsiniz.

Ders 4'ün tamamlanmasını tamamladın.  Bu derste, parçaları seçmek için işlevselliği eklersiniz.

## <a name="media-services-learning-paths"></a>Media Services’i öğrenme yolları

[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Geri bildirimde bulunma
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="other-resources"></a>Diğer Kaynaklar:
* [Gelişmiş özelliklere sahip Bir Düzgün Akış Lı Windows 8 JavaScript uygulaması nasıl inşa edilebilen](https://blogs.iis.net/cenkd/archive/2012/08/10/how-to-build-a-smooth-streaming-windows-8-javascript-application-with-advanced-features.aspx)
* [Sorunsuz Akış Teknik Genel Bakış](https://www.iis.net/learn/media/on-demand-smooth-streaming/smooth-streaming-technical-overview)

[PlayerApplication]: ./media/media-services-build-smooth-streaming-apps/SSClientWin8-1.png
[CodeViewPic]: ./media/media-services-build-smooth-streaming-apps/SSClientWin8-2.png

