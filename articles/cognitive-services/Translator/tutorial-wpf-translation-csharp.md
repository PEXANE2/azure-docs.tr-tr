---
title: 'Öğretici: WPF, C# ile çeviri uygulaması oluşturma - Çevirmen Metin API'
titleSuffix: Azure Cognitive Services
description: Bu eğitimde, metin çevirisi, dil algılama ve yazım denetimi gerçekleştirmek için tek bir abonelik anahtarıyla bir WPF uygulaması oluşturursunuz.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: tutorial
ms.date: 02/10/2020
ms.author: swmachan
ms.openlocfilehash: ecb42d200eb8808f6bfa4cfb91e98909e350038b
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77118611"
---
# <a name="tutorial-create-a-translation-app-with-wpf"></a>Öğretici: WPF ile çeviri uygulaması oluşturma

Bu eğitimde, metin çevirisi, dil algılama ve yazım denetimi için tek bir abonelik anahtarıyla Azure Bilişsel Hizmetleri kullanan bir [Windows Sunu Vakfı (WPF)](https://docs.microsoft.com/visualstudio/designers/getting-started-with-wpf?view=vs-2019) uygulaması oluşturursunuz. Özellikle, uygulamanız Çevirmen Metin ve [Bing Yazım Denetimi'nden](https://azure.microsoft.com/services/cognitive-services/spell-check/)API'leri arayacaktır.

WPF nedir? Masaüstü istemci uygulamaları oluşturan bir UI çerçevesidir. WPF geliştirme platformu, uygulama modeli, kaynaklar, denetimler, grafikler, düzen, veri bağlama, belgeler ve güvenlik gibi çok çeşitli uygulama geliştirme özelliklerini destekler. Bu .NET Framework'ün bir alt kümesidir, bu nedenle ASP.NET veya Windows Forms kullanarak .NET Framework ile daha önce uygulama inşa ettiyseniz, programlama deneyimi tanıdık olmalıdır. WPF, uygulama programlama için bildirimsel bir model sağlamak için Genişletilebilir biçimlendirme dili (XAML) kullanır ve bunu önümüzdeki bölümlerde inceleyeceğiz.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Visual Studio'da bir WPF projesi oluşturma
> * Projenize montajlar ve NuGet paketleri ekleyin
> * XAML ile uygulamanızın UI'ini oluşturun
> * Dil almak, metni çevirmek ve kaynak dili algılamak için Çevirmen Metin API'sini kullanın
> * Girişinizi doğrulamak ve çeviri doğruluğunu artırmak için Bing Yazım Denetimi API'sini kullanın
> * WPF uygulamanızı çalıştırın

### <a name="cognitive-services-used-in-this-tutorial"></a>Bu eğitimde kullanılan Bilişsel Hizmetler

Bu liste, bu öğreticide kullanılan Bilişsel Hizmetleri içerir. Her özellik için API başvurusuna göz atmak için bağlantıyı izleyin.

| Hizmet | Özellik | Açıklama |
|---------|---------|-------------|
| Translator Metin Çevirisi | [Dil Alın](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-languages) | Metin çevirisi için desteklenen dillerin tam listesini alın. |
| Translator Metin Çevirisi | [Çevirme](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-translate) | Metni 60'tan fazla dile çevirin. |
| Translator Metin Çevirisi | [Algılama](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-detect) | Giriş metninin dilini algılayın. Algılama için güven puanı içerir. |
| Bing Yazım Denetimi | [Yazım Denetimi](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v7-reference) | Çeviri doğruluğunu artırmak için yazım hatalarını düzeltin. |

## <a name="prerequisites"></a>Ön koşullar

Devam etmeden önce aşağıdakileri yapmanız gerekir:

* Azure Bilişsel Hizmetler aboneliği. [Bilişsel Hizmetler anahtarı alın.](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account#create-a-new-azure-cognitive-services-resource)
* Bir Windows makinesi
* [Visual Studio 2019](https://www.visualstudio.com/downloads/) - Topluluk veya Kurumsal

> [!NOTE]
> Bu öğretici için Aboneliği Batı ABD bölgesinde oluşturmanızı öneririz. Aksi takdirde, bu alıştırma da çalışırken koddaki uç noktaları ve bölgeleri değiştirmeniz gerekir.  

## <a name="create-a-wpf-app-in-visual-studio"></a>Visual Studio'da WPF uygulaması oluşturma

Yapmamız gereken ilk şey Visual Studio'da projemizi kurmak.

1. Visual Studio'yu açın. **Yeni bir proje oluştur'u**seçin.
1. **Yeni bir proje oluştur'da** **WPF Uygulamasını (.NET Framework)** bulun ve seçin. Seçenekleri daraltmak için **Dil'den** C# seçeneğini belirleyebilirsiniz.
1. **İleri'yi**seçin ve `MSTranslatorTextDemo`ardından projenizi adlandırın.
1. Çerçeve sürümünü **.NET Framework 4.7.2** veya sonraki sürümolarak ayarlayın ve **Oluştur'u**seçin.
   ![Visual Studio'ya ad ve çerçeve sürümünü girin](media/name-wpf-project-visual-studio.png)

Projeniz oluşturuldu. Açık iki sekme olduğunu fark `MainWindow.xaml` edeceksiniz: `MainWindow.xaml.cs`ve . Bu öğretici boyunca, bu iki dosyaya kod ekleyeceğiz. Uygulamanın kullanıcı `MainWindow.xaml` arabirimini değiştireceğiz. Çevirmen Metni `MainWindow.xaml.cs` ve Bing Yazım Denetimi çağrılarımızı değiştireceğiz.
   ![Ortamınızı gözden geçirin](media/blank-wpf-project.png)

Bir sonraki bölümde, JSON ayrıştırma gibi ek işlevsellik için projemize derlemeler ve NuGet paketi ekleyeceğiz.

## <a name="add-references-and-nuget-packages-to-your-project"></a>Projenize referanslar ve NuGet paketleri ekleyin

Projemiz, NuGet paket yöneticisini kullanarak yükleyeceğimiz bir avuç .NET Framework derlemesi ve NewtonSoft.Json gerektirir.

### <a name="add-net-framework-assemblies"></a>.NET Framework derlemeleri ekle

Nesneleri seri hale getirmek ve deserialize etmek ve HTTP isteklerini ve yanıtlarını yönetmek için projemize derlemeler ekleyelim.

1. Projenizi Visual Studio'nun Çözüm Gezgini'nde bulun. Projenizi sağ tıklatın, ardından **Başvuru Yöneticisi'ni**açan **> Başvuru Ekle'yi**seçin.
1. **Derlemeler** sekmesi, başvuru için kullanılabilen tüm .NET Framework derlemelerini listeler. Başvuruları aramak için sağ üstteki arama çubuğunu kullanın.
   ![Montaj başvuruları ekleme](media/add-assemblies-2019.png)
1. Projeniz için aşağıdaki başvuruları seçin:
   * [System.Runtime.Serialization](https://docs.microsoft.com/dotnet/api/system.runtime.serialization)
   * [System.Web](https://docs.microsoft.com/dotnet/api/system.web)
   * System.Web.Extensions
   * [System.Windows](https://docs.microsoft.com/dotnet/api/system.windows)
1. Bu başvuruları projenize ekledikten **sonra, Başvuru Yöneticisi'ni**kapatmak için **Tamam'ı** tıklatabilirsiniz.

> [!NOTE]
> Derleme başvuruları hakkında daha fazla bilgi edinmek istiyorsanız, [bkz.](https://docs.microsoft.com/visualstudio/ide/how-to-add-or-remove-references-by-using-the-reference-manager?view=vs-2019)

### <a name="install-newtonsoftjson"></a>NewtonSoft.Json'ı yükleyin

Uygulamamız JSON nesnelerini deserialize etmek için NewtonSoft.Json'u kullanacaktır. Paketi yüklemek için aşağıdaki yönergeleri izleyin.

1. Projenizi Visual Studio'nun Çözüm Gezgini'nde bulun ve projenize sağ tıklayın. **NuGet Paketlerini Yönet'i**seçin.
1. **Gözat** sekmesini bulun ve seçin.
1. [NewtonSoft.Json'ı](https://www.nuget.org/packages/Newtonsoft.Json/) arama çubuğuna girin.

    ![NewtonSoft.Json'ı bulun ve kurun](media/nuget-package-manager.png)

1. Paketi seçin ve **Yükle'yi**tıklatın.
1. Yükleme tamamlandığında, sekmeyi kapatın.

## <a name="create-a-wpf-form-using-xaml"></a>XAML kullanarak WPF formu oluşturma

Uygulamanızı kullanmak için bir kullanıcı arabirimine ihtiyacınız olacak. XAML'yi kullanarak, kullanıcıların giriş ve çeviri dillerini seçmelerine, çevrilmesi için metin girmelerine ve çeviri çıktısını görüntülemelerine olanak tanıyan bir form oluştururuz.

Ne inşa ettiğimize bir bakalım.

![WPF XAML kullanıcı arabirimi](media/translator-text-csharp-xaml.png)

Kullanıcı arabirimi şu bileşenleri içerir:

| Adı | Tür | Açıklama |
|------|------|-------------|
| `FromLanguageComboBox` | ComboBox | Metin çevirisi için Microsoft Translator tarafından desteklenen dillerin listesini görüntüler. Kullanıcı çeviri yaptığı kaynak dili seçer. |
| `ToLanguageComboBox` | ComboBox | `FromComboBox`Aynı dil listesini , ancak kullanıcının çevirdiği dili seçmek için kullanılır. |
| `TextToTranslate` | TextBox | Kullanıcının çevrilecek metni girmesini sağlar. |
| `TranslateButton` | Düğme | Metni çevirmek için bu düğmeyi kullanın. |
| `TranslatedTextLabel` | Etiketle | Çeviriyi görüntüler. |
| `DetectedLanguageLabel` | Etiketle | Çevrilecek metnin algılanan dilini görüntüler`TextToTranslate`( ). |

> [!NOTE]
> Bu formu XAML kaynak kodunu kullanarak oluşturuyoruz, ancak Visual Studio'daki düzenleyiciile formu oluşturabilirsiniz.

Projemize kodu ekleyelim.

1. Visual Studio'da. `MainWindow.xaml`
1. Bu kodu projenize kopyalayın ve değişikliklerinizi kaydetmek için **> Kaydet MainWindow.xaml'ı** seçin.
   ```xaml
   <Window x:Class="MSTranslatorTextDemo.MainWindow"
           xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
           xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
           xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
           xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
           xmlns:local="clr-namespace:MSTranslatorTextDemo"
           mc:Ignorable="d"
           Title="Microsoft Translator" Height="400" Width="700" BorderThickness="0">
       <Grid>
           <Label x:Name="label" Content="Microsoft Translator" HorizontalAlignment="Left" Margin="39,6,0,0" VerticalAlignment="Top" Height="49" FontSize="26.667"/>
           <TextBox x:Name="TextToTranslate" HorizontalAlignment="Left" Height="23" Margin="42,160,0,0" TextWrapping="Wrap" VerticalAlignment="Top" Width="600" FontSize="14" TabIndex="3"/>
           <Label x:Name="EnterTextLabel" Content="Text to translate:" HorizontalAlignment="Left" Margin="40,129,0,0" VerticalAlignment="Top" FontSize="14"/>
           <Label x:Name="toLabel" Content="Translate to:" HorizontalAlignment="Left" Margin="304,58,0,0" VerticalAlignment="Top" FontSize="14"/>

           <Button x:Name="TranslateButton" Content="Translate" HorizontalAlignment="Left" Margin="39,206,0,0" VerticalAlignment="Top" Width="114" Height="31" Click="TranslateButton_Click" FontSize="14" TabIndex="4" IsDefault="True"/>
           <ComboBox x:Name="ToLanguageComboBox"
                   HorizontalAlignment="Left"
                   Margin="306,88,0,0"
                   VerticalAlignment="Top"
                   Width="175" FontSize="14" TabIndex="2">

           </ComboBox>
           <Label x:Name="fromLabel" Content="Translate from:" HorizontalAlignment="Left" Margin="40,58,0,0" VerticalAlignment="Top" FontSize="14"/>
           <ComboBox x:Name="FromLanguageComboBox"
               HorizontalAlignment="Left"
               Margin="42,88,0,0"
               VerticalAlignment="Top"
               Width="175" FontSize="14" TabIndex="1"/>
           <Label x:Name="TranslatedTextLabel" Content="Translation is displayed here." HorizontalAlignment="Left" Margin="39,255,0,0" VerticalAlignment="Top" Width="620" FontSize="14" Height="85" BorderThickness="0"/>
           <Label x:Name="DetectedLanguageLabel" Content="Autodetected language is displayed here." HorizontalAlignment="Left" Margin="39,288,0,0" VerticalAlignment="Top" Width="620" FontSize="14" Height="84" BorderThickness="0"/>
       </Grid>
   </Window>
   ```
Şimdi Visual Studio'da uygulamanın kullanıcı arabiriminin önizlemesini görmeniz gerekir. Yukarıdaki resme benzer görünmelidir.

İşte bu, formun hazır. Şimdi Metin Çevirisi ve Bing Yazım Denetimi'ni kullanmak için bazı kodlar yazalım.

> [!NOTE]
> Bu formu tweak veya kendi oluşturmak için çekinmeyin.

## <a name="create-your-app"></a>Uygulamanızı oluşturun

`MainWindow.xaml.cs`uygulamamızı kontrol eden kodu içerir. Sonraki birkaç bölümde, açılan menülerimizi doldurmak ve Çevirmen Metin ve Bing Yazım Denetimi tarafından açığa çıkarılan bir avuç API'yi çağırmak için kod ekleyeceğiz.

* Program başlatıldığında `MainWindow` ve anında başlatıldığında, `Languages` Dil seçimi açılır bırakmalarımızı almak ve doldurmak için Çevirmen Metin API'sinin yöntemi çağrılır. Bu, her oturumun başında bir kez olur.
* **Çevir** düğmesi tıklandığında, kullanıcının dil seçimi ve metni alınır, girişte yazım denetimi gerçekleştirilir ve kullanıcı için çeviri ve algılanan dil görüntülenir.
  * Çevirmen `Translate` Metin API yöntemi metin çevirmek `TextToTranslate`için denir. Bu çağrı, `to` açılır `from` menüler kullanılarak seçilen dilleri de içerir.
  * Çevirmen `Detect` Metin API yöntemi metin dilini belirlemek için `TextToTranslate`çağrılır.
  * Bing Yazım Denetimi, `TextToTranslate` yazım hatalarını doğrulamak ve ayarlamak için kullanılır.

Tüm projemiz `MainWindow : Window` sınıfa eklenmiş durumda. Abonelik anahtarınızı ayarlamak için kod ekleyerek başlayalım, Çevirmen Metni ve Bing Yazım Denetimi için uç noktaları bildirelim ve uygulamayı başlatalım.

1. Visual Studio'da. `MainWindow.xaml.cs`
1. Önceden doldurulmuş `using` ifadeleri aşağıdakilerle değiştirin.  
   ```csharp
   using System;
   using System.Windows;
   using System.Net;
   using System.Net.Http;
   using System.IO;
   using System.Collections.Generic;
   using System.Linq;
   using System.Text;
   using Newtonsoft.Json;
   ```
1. `MainWindow : Window` Sınıfı bulun ve bu kodla değiştirin:
   ```csharp
   {
       // This sample uses the Cognitive Services subscription key for all services. To learn more about
       // authentication options, see: https://docs.microsoft.com/azure/cognitive-services/authentication.
       const string COGNITIVE_SERVICES_KEY = "YOUR_COG_SERVICES_KEY";
       // Endpoints for Translator Text and Bing Spell Check
       public static readonly string TEXT_TRANSLATION_API_ENDPOINT = "https://api.cognitive.microsofttranslator.com/{0}?api-version=3.0";
       const string BING_SPELL_CHECK_API_ENDPOINT = "https://westus.api.cognitive.microsoft.com/bing/v7.0/spellcheck/";
       // An array of language codes
       private string[] languageCodes;

       // Dictionary to map language codes from friendly name (sorted case-insensitively on language name)
       private SortedDictionary<string, string> languageCodesAndTitles =
           new SortedDictionary<string, string>(Comparer<string>.Create((a, b) => string.Compare(a, b, true)));

       // Global exception handler to display error message and exit
       private static void HandleExceptions(object sender, UnhandledExceptionEventArgs args)
       {
           Exception e = (Exception)args.ExceptionObject;
           MessageBox.Show("Caught " + e.Message, "Error", MessageBoxButton.OK, MessageBoxImage.Error);
           System.Windows.Application.Current.Shutdown();
       }
       // MainWindow constructor
       public MainWindow()
       {
           // Display a message if unexpected error is encountered
           AppDomain.CurrentDomain.UnhandledException += new UnhandledExceptionEventHandler(HandleExceptions);

           if (COGNITIVE_SERVICES_KEY.Length != 32)
           {
               MessageBox.Show("One or more invalid API subscription keys.\n\n" +
                   "Put your keys in the *_API_SUBSCRIPTION_KEY variables in MainWindow.xaml.cs.",
                   "Invalid Subscription Key(s)", MessageBoxButton.OK, MessageBoxImage.Error);
               System.Windows.Application.Current.Shutdown();
           }
           else
           {
               // Start GUI
               InitializeComponent();
               // Get languages for drop-downs
               GetLanguagesForTranslate();
               // Populate drop-downs with values from GetLanguagesForTranslate
               PopulateLanguageMenus();
           }
       }
   // NOTE:
   // In the following sections, we'll add code below this.
   }
   ```
1. Bilişsel Hizmetler abonelik anahtarınızı ekleyin ve kaydedin.

Bu kod bloğunda, çeviri için kullanılabilir diller hakkında bilgi içeren iki üye değişken beyan ettik:

| Değişken | Tür | Açıklama |
|----------|------|-------------|
|`languageCodes` | Dize dizileri |Dil kodlarını önbelleğe alır. Translator hizmeti dilleri belirlemek için kısa kodlar kullanır (örneğin İngilizce için `en`). |
|`languageCodesAndTitles` | Sıralanmış sözlük | Kullanıcı arabirimindeki "kolay anlaşılır" adları, API’de kullanılan kısa kodlarla eşleştirir. Büyük küçük harf kullanımından bağımsız olarak alfabetik sırayla tutulur. |

Sonra, `MainWindow` yapıcı içinde, biz ile `HandleExceptions`hata işleme ekledik. Bu hata işleme, bir özel durum işlenmezse bir uyarı sağlanmasını sağlar. Daha sonra sağlanan abonelik anahtarının uzunluğu 32 karakter olduğunu doğrulamak için bir denetim çalıştırılır. Anahtar 32 karakterden küçük/büyükse bir hata atılır.

En azından doğru uzunlukta anahtarlar varsa, `InitializeComponent()` arama, ana uygulama penceresinin XAML açıklamasını bularak, yükleyerek ve anında ayarlayarak kullanıcı arabirimini yuvarlar.

Son olarak, çeviri için dil almak ve uygulamamızın kullanıcı arabirimi için açılan menüleri doldurmak için arama yöntemlerini çağırmak için kod ekledik. Merak etme, yakında bu aramaların arkasındaki koda ulaşacağız.

## <a name="get-supported-languages"></a>Desteklenen dilleri alma

Çevirmen Metin API'si şu anda 60'tan fazla dili desteklemektedir. Zaman içinde yeni dil desteği ekleneceği için, uygulamanızdaki dil listesini kodlamak yerine Çevirmen Metni tarafından açığa çıkarılan Diller kaynağını aramanızı öneririz.

Bu bölümde, Diller kaynağına `GET` çeviri için kullanılabilen dillerin listesini istediğimizi belirten bir istek oluştururuz.

> [!NOTE]
> Diller kaynağı, dil desteğini aşağıdaki sorgu parametreleriyle filtrelemenize olanak tanır: çeviri, sözlük ve çeviri. Daha fazla bilgi için [API başvurusuna](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-languages)bakın.

Daha ileri gitmeden önce, Diller kaynağına bir çağrı için örnek çıktıya bir göz atalım:

```json
{
  "translation": {
    "af": {
      "name": "Afrikaans",
      "nativeName": "Afrikaans",
      "dir": "ltr"
    },
    "ar": {
      "name": "Arabic",
      "nativeName": "العربية",
      "dir": "rtl"
    }
    // Additional languages are provided in the full JSON output.
}
```

Bu çıktıdan, belirli bir dilin `name` dil kodunu ve dil kodunu ayıklayabiliriz. Uygulamamız JSON nesnesi deserialize NewtonSoft.Json kullanır ([`JsonConvert.DeserializeObject`](https://www.newtonsoft.com/json/help/html/M_Newtonsoft_Json_JsonConvert_DeserializeObject__1.htm)).

Son bölümde kaldığımız yerden devam ederek, desteklenen dilleri uygulamamıza getirmek için bir yöntem ekleyelim.

1. Visual Studio'da, '' için `MainWindow.xaml.cs`sekmeyi açın.
2. Bu kodu projenize ekleyin:
   ```csharp
   // ***** GET TRANSLATABLE LANGUAGE CODES
   private void GetLanguagesForTranslate()
   {
       // Send request to get supported language codes
       string uri = String.Format(TEXT_TRANSLATION_API_ENDPOINT, "languages") + "&scope=translation";
       WebRequest WebRequest = WebRequest.Create(uri);
       WebRequest.Headers.Add("Accept-Language", "en");
       WebResponse response = null;
       // Read and parse the JSON response
       response = WebRequest.GetResponse();
       using (var reader = new StreamReader(response.GetResponseStream(), UnicodeEncoding.UTF8))
       {
           var result = JsonConvert.DeserializeObject<Dictionary<string, Dictionary<string, Dictionary<string, string>>>>(reader.ReadToEnd());
           var languages = result["translation"];

           languageCodes = languages.Keys.ToArray();
           foreach (var kv in languages)
           {
               languageCodesAndTitles.Add(kv.Value["name"], kv.Key);
           }
       }
   }
   // NOTE:
   // In the following sections, we'll add code below this.
   ```

Yöntem `GetLanguagesForTranslate()` bir HTTP GET isteği oluşturur `scope=translation` ve sorgu dize parametresini kullanarak çeviri için desteklenen dillere isteğin kapsamını sınırlamak için kullanılır. Desteklenen dillerin İngilizce olarak döndürülmesi için `en` değerine sahip `Accept-Language` üst bilgisi eklenir.

JSON yanıtı ayrıştırılır ve bir sözlük dönüştürülür. Daha sonra dil kodları `languageCodes` üye değişkene eklenir. Dil kodlarını ve kolay dil adlarını içeren anahtar/değer çiftleri döndürülür ve `languageCodesAndTitles` üye değişkenine eklenir. Formdaki açılır menüler dost adları görüntüler, ancak çeviri istemek için kodlar gereklidir.

## <a name="populate-language-drop-down-menus"></a>Dil açılır menülerini doldurma

Kullanıcı arabirimi XAML kullanılarak tanımlanır, bu nedenle arama `InitializeComponent()`dışında kurmak için çok yapmanız gerekmez. Yapmanız gereken tek şey, çeviri ye ve **Translate from** açılan **menülere Çevir'e** samimi dil adlarını eklemektir. Yöntem `PopulateLanguageMenus()` adları ekler.

1. Visual Studio'da, '' için `MainWindow.xaml.cs`sekmeyi açın.
2. Bu kodu projenize aşağıdaki `GetLanguagesForTranslate()` yöntemin altına ekleyin:
   ```csharp
   private void PopulateLanguageMenus()
   {
       // Add option to automatically detect the source language
       FromLanguageComboBox.Items.Add("Detect");

       int count = languageCodesAndTitles.Count;
       foreach (string menuItem in languageCodesAndTitles.Keys)
       {
           FromLanguageComboBox.Items.Add(menuItem);
           ToLanguageComboBox.Items.Add(menuItem);
       }

       // Set default languages
       FromLanguageComboBox.SelectedItem = "Detect";
       ToLanguageComboBox.SelectedItem = "English";
   }
   // NOTE:
   // In the following sections, we'll add code below this.
   ```

Bu yöntem `languageCodesAndTitles` sözlük üzerinde yineler ve her iki menüye her iki anahtar ekler. Menüler doldurulduktan sonra, dillerden gelen ve gelen varsayılan varsayılan değer sırasıyla **Algılama** ve **İngilizce** olarak ayarlanır.

> [!TIP]
> Menüler için varsayılan bir seçim olmadan, kullanıcı önce bir "hedef" veya "kaynak" dil seçmeden **Çevir**’e tıklayabilir. Varsayılan değerler bu sorunla başa çıkma gereksinimini ortadan kaldırır.

Şimdi `MainWindow` bu başlatılan ve kullanıcı arabirimi oluşturulan, bu kod **Çevirme** düğmesi tıklanana kadar çalışmaz.

## <a name="detect-language-of-source-text"></a>Kaynak metnin dilini algılama

Şimdi, Çevirmen Metin API'sini kullanarak kaynak metnin dilini (metin alanımıza girilen metin) algılama yöntemi oluşturacağız. Bu istek tarafından döndürülen değer daha sonra çeviri isteğimizde kullanılacaktır.

1. Visual Studio'da, '' için `MainWindow.xaml.cs`sekmeyi açın.
2. Bu kodu projenize aşağıdaki `PopulateLanguageMenus()` yöntemin altına ekleyin:
   ```csharp
   // ***** DETECT LANGUAGE OF TEXT TO BE TRANSLATED
   private string DetectLanguage(string text)
   {
       string detectUri = string.Format(TEXT_TRANSLATION_API_ENDPOINT ,"detect");

       // Create request to Detect languages with Translator Text
       HttpWebRequest detectLanguageWebRequest = (HttpWebRequest)WebRequest.Create(detectUri);
       detectLanguageWebRequest.Headers.Add("Ocp-Apim-Subscription-Key", COGNITIVE_SERVICES_KEY);
       detectLanguageWebRequest.Headers.Add("Ocp-Apim-Subscription-Region", "westus");
       detectLanguageWebRequest.ContentType = "application/json; charset=utf-8";
       detectLanguageWebRequest.Method = "POST";

       // Send request
       var serializer = new System.Web.Script.Serialization.JavaScriptSerializer();
       string jsonText = serializer.Serialize(text);

       string body = "[{ \"Text\": " + jsonText + " }]";
       byte[] data = Encoding.UTF8.GetBytes(body);

       detectLanguageWebRequest.ContentLength = data.Length;

       using (var requestStream = detectLanguageWebRequest.GetRequestStream())
           requestStream.Write(data, 0, data.Length);

       HttpWebResponse response = (HttpWebResponse)detectLanguageWebRequest.GetResponse();

       // Read and parse JSON response
       var responseStream = response.GetResponseStream();
       var jsonString = new StreamReader(responseStream, Encoding.GetEncoding("utf-8")).ReadToEnd();
       dynamic jsonResponse = serializer.DeserializeObject(jsonString);

       // Fish out the detected language code
       var languageInfo = jsonResponse[0];
       if (languageInfo["score"] > (decimal)0.5)
       {
           DetectedLanguageLabel.Content = languageInfo["language"];
           return languageInfo["language"];
       }
       else
           return "Unable to confidently detect input language.";
   }
   // NOTE:
   // In the following sections, we'll add code below this.
   ```

Bu yöntem, Algıla kaynağına bir HTTP `POST` isteği oluşturur. Bu tek bir `text`argüman alır, hangi istek gövdesi olarak birlikte geçirilir. Daha sonra, çeviri isteğimizi oluşturduğumuzda, Kullanıcı Arabirimi'mize girilen metin dil algılama için bu yönteme geçirilecektir.

Ayrıca, bu yöntem yanıtın güven puanını değerlendirir. Puan, kullanıcı arabirimimizde algılanan dilden büyükse `0.5`görüntülenir.

## <a name="spell-check-the-source-text"></a>Kaynak metni denetleme

Şimdi Bing Yazım Denetimi API'sini kullanarak kaynak metnimizi denetlemek için bir yöntem oluşturacağız. Yazım denetimi, Translator Text API'den doğru çevirileri geri almamızı sağlar. Kaynak metindeki düzeltmeler, **Çeviri** düğmesi tıklandığında çeviri isteğimizde iletilir.

1. Visual Studio'da, '' için `MainWindow.xaml.cs`sekmeyi açın.
2. Bu kodu projenize aşağıdaki `DetectLanguage()` yöntemin altına ekleyin:

```csharp
// ***** CORRECT SPELLING OF TEXT TO BE TRANSLATED
private string CorrectSpelling(string text)
{
    string uri = BING_SPELL_CHECK_API_ENDPOINT + "?mode=spell&mkt=en-US";

    // Create a request to Bing Spell Check API
    HttpWebRequest spellCheckWebRequest = (HttpWebRequest)WebRequest.Create(uri);
    spellCheckWebRequest.Headers.Add("Ocp-Apim-Subscription-Key", COGNITIVE_SERVICES_KEY);
    spellCheckWebRequest.Method = "POST";
    spellCheckWebRequest.ContentType = "application/x-www-form-urlencoded"; // doesn't work without this

    // Create and send the request
    string body = "text=" + System.Web.HttpUtility.UrlEncode(text);
    byte[] data = Encoding.UTF8.GetBytes(body);
    spellCheckWebRequest.ContentLength = data.Length;
    using (var requestStream = spellCheckWebRequest.GetRequestStream())
        requestStream.Write(data, 0, data.Length);
    HttpWebResponse response = (HttpWebResponse)spellCheckWebRequest.GetResponse();

    // Read and parse the JSON response; get spelling corrections
    var serializer = new System.Web.Script.Serialization.JavaScriptSerializer();
    var responseStream = response.GetResponseStream();
    var jsonString = new StreamReader(responseStream, Encoding.GetEncoding("utf-8")).ReadToEnd();
    dynamic jsonResponse = serializer.DeserializeObject(jsonString);
    var flaggedTokens = jsonResponse["flaggedTokens"];

    // Construct sorted dictionary of corrections in reverse order (right to left)
    // This ensures that changes don't impact later indexes
    var corrections = new SortedDictionary<int, string[]>(Comparer<int>.Create((a, b) => b.CompareTo(a)));
    for (int i = 0; i < flaggedTokens.Length; i++)
    {
        var correction = flaggedTokens[i];
        var suggestion = correction["suggestions"][0];  // Consider only first suggestion
        if (suggestion["score"] > (decimal)0.7)         // Take it only if highly confident
            corrections[(int)correction["offset"]] = new string[]   // dict key   = offset
                { correction["token"], suggestion["suggestion"] };  // dict value = {error, correction}
    }

    // Apply spelling corrections, in order, from right to left
    foreach (int i in corrections.Keys)
    {
        var oldtext = corrections[i][0];
        var newtext = corrections[i][1];

        // Apply capitalization from original text to correction - all caps or initial caps
        if (text.Substring(i, oldtext.Length).All(char.IsUpper)) newtext = newtext.ToUpper();
        else if (char.IsUpper(text[i])) newtext = newtext[0].ToString().ToUpper() + newtext.Substring(1);

        text = text.Substring(0, i) + newtext + text.Substring(i + oldtext.Length);
    }
    return text;
}
// NOTE:
// In the following sections, we'll add code below this.
```

## <a name="translate-text-on-click"></a>Metni tıklatıldığında çevirme

Yapmamız gereken son şey, kullanıcı arabirimimizdeki **Çevir** düğmesi tıklatıldığında çağrılan bir yöntem oluşturmaktır.

1. Visual Studio'da, '' için `MainWindow.xaml.cs`sekmeyi açın.
1. Bu kodu projenize metodun altına ekleyin ve kaydedin: `CorrectSpelling()`  
   ```csharp
   // ***** PERFORM TRANSLATION ON BUTTON CLICK
   private async void TranslateButton_Click(object sender, EventArgs e)
   {
       string textToTranslate = TextToTranslate.Text.Trim();

       string fromLanguage = FromLanguageComboBox.SelectedValue.ToString();
       string fromLanguageCode;

       // auto-detect source language if requested
       if (fromLanguage == "Detect")
       {
           fromLanguageCode = DetectLanguage(textToTranslate);
           if (!languageCodes.Contains(fromLanguageCode))
           {
               MessageBox.Show("The source language could not be detected automatically " +
                   "or is not supported for translation.", "Language detection failed",
                   MessageBoxButton.OK, MessageBoxImage.Error);
               return;
           }
       }
       else
           fromLanguageCode = languageCodesAndTitles[fromLanguage];

       string toLanguageCode = languageCodesAndTitles[ToLanguageComboBox.SelectedValue.ToString()];

       // spell-check the source text if the source language is English
       if (fromLanguageCode == "en")
       {
           if (textToTranslate.StartsWith("-"))    // don't spell check in this case
               textToTranslate = textToTranslate.Substring(1);
           else
           {
               textToTranslate = CorrectSpelling(textToTranslate);
               TextToTranslate.Text = textToTranslate;     // put corrected text into input field
           }
       }
       // handle null operations: no text or same source/target languages
       if (textToTranslate == "" || fromLanguageCode == toLanguageCode)
       {
           TranslatedTextLabel.Content = textToTranslate;
           return;
       }

       // send HTTP request to perform the translation
       string endpoint = string.Format(TEXT_TRANSLATION_API_ENDPOINT, "translate");
       string uri = string.Format(endpoint + "&from={0}&to={1}", fromLanguageCode, toLanguageCode);

       System.Object[] body = new System.Object[] { new { Text = textToTranslate } };
       var requestBody = JsonConvert.SerializeObject(body);

       using (var client = new HttpClient())
       using (var request = new HttpRequestMessage())
       {
           request.Method = HttpMethod.Post;
           request.RequestUri = new Uri(uri);
           request.Content = new StringContent(requestBody, Encoding.UTF8, "application/json");
           request.Headers.Add("Ocp-Apim-Subscription-Key", COGNITIVE_SERVICES_KEY);
           request.Headers.Add("Ocp-Apim-Subscription-Region", "westus");
           request.Headers.Add("X-ClientTraceId", Guid.NewGuid().ToString());

           var response = await client.SendAsync(request);
           var responseBody = await response.Content.ReadAsStringAsync();

           var result = JsonConvert.DeserializeObject<List<Dictionary<string, List<Dictionary<string, string>>>>>(responseBody);
           var translation = result[0]["translations"][0]["text"];

           // Update the translation field
           TranslatedTextLabel.Content = translation;
       }
   }
   ```

İlk adım ,"from" ve "to" dillerini ve kullanıcının formumuza girdiği metni almaktır. Kaynak dil **Algıla**olarak `DetectLanguage()` ayarlanmışsa, kaynak metnin dilini belirlemek için çağrılır. Metin, Çevirmen API'sinin desteklemediği bir dilde olabilir. Bu durumda, kullanıcıyı bilgilendirmek için bir ileti görüntüleyin ve metni çevirmeden döndürün.

Kaynak dil İngilizce ise (belirtilerek veya algılanarak), metnin yazımını `CorrectSpelling()` ile denetleyin ve düzeltmeleri uygulayın. Düzeltilen metin, kullanıcının düzeltme yapıldığını görmesi için metin alanına geri eklenir.

Metni çevirecek kod tanıdık görünmelidir: URI'yi oluşturun, bir istek oluşturun, gönderin ve yanıtı ayrıştırın. JSON dizisi çeviri için birden fazla nesne içerebilir, ancak uygulamamız yalnızca bir nesne gerektirir.

Başarılı bir istekten sonra, `TranslatedTextLabel.Content` `translation`çevrilen metni görüntülemek için kullanıcı arabirimini güncelleyen ,

## <a name="run-your-wpf-app"></a>WPF uygulamanızı çalıştırın

Bu kadar, WPF kullanılarak oluşturulmuş çalışan bir çeviri uygulaması var. Uygulamanızı çalıştırmak için Visual Studio'da **Başlat** düğmesini tıklatın.

## <a name="source-code"></a>Kaynak kod

Bu projenin kaynak kodu GitHub'da kullanılabilir.

* [Kaynak kodu keşfedin](https://github.com/MicrosoftTranslator/Text-Translation-API-V3-C-Sharp-Tutorial)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Microsoft Translator Metin Çevirisi API’si başvurusu](https://docs.microsoft.com/azure/cognitive-services/Translator/reference/v3-0-reference)
