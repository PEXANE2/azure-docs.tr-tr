---
title: Xamarin nesne (Blob) depolama nasıl kullanılır | Microsoft Dokümanlar
description: Xamarin için Azure Depolama istemci kitaplığı, geliştiricilerin kendi yerel kullanıcı arabirimleriyle iOS, Android ve Windows Mağazası uygulamaları oluşturmasına olanak tanır. Bu öğretici, Azure Blob depolama alanını kullanan bir uygulama oluşturmak için Xamarin'in nasıl kullanılacağını gösterir.
author: mhopkins-msft
ms.author: mhopkins
ms.date: 05/11/2017
ms.service: storage
ms.subservice: blobs
ms.topic: conceptual
ms.openlocfilehash: 8a1c91c8a8a59af26386e70e68e7c4fd93f5eaa9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68726332"
---
# <a name="how-to-use-blob-storage-from-xamarin"></a>Xamarin blob Depolama nasıl kullanılır

Xamarin, geliştiricilerin yerel kullanıcı arayüzleriyle iOS, Android ve Windows Mağazası uygulamaları oluşturmak için paylaşılan bir C# kod tabanını kullanmalarını sağlar. Bu öğretici, Xamarin uygulamasıyla Azure Blob depolama alanını nasıl kullanacağınızı gösterir. Koda dalmadan önce Azure Depolama hakkında daha fazla bilgi edinmek istiyorsanız, [Microsoft Azure Depolamasına Giriş 'e](../common/storage-introduction.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)bakın.

[!INCLUDE [storage-create-account-include](../../../includes/storage-create-account-include.md)]

[!INCLUDE [storage-mobile-authentication-guidance](../../../includes/storage-mobile-authentication-guidance.md)]

## <a name="create-a-new-xamarin-application"></a>Yeni bir Xamarin Uygulaması Oluşturun

Bu öğretici için, Android, iOS ve Windows'u hedefleyen bir uygulama oluşturacağız. Bu uygulama sadece bir konteyner oluşturmak ve bu kapsayıcı içine bir blob yükleyin. Windows'da Visual Studio'yu kullanacağız, ancak macOS'ta Xamarin Studio'yu kullanarak bir uygulama oluştururken de aynı öğrenmeler uygulanabilir.

Uygulamanızı oluşturmak için aşağıdaki adımları izleyin:

1. Zaten değilseniz, indirin ve [Visual Studio için Xamarin](https://www.xamarin.com/download)yükleyin.
2. Visual Studio'u açın ve Boş Bir Uygulama (Native Portable) oluşturun: **Dosya > Yeni > Projesi > Çapraz Platform > Boş Uygulama (Native Portable)**.
3. Çözüm Gezgini bölmesinde çözümünüzü sağ tıklatın ve **Çözüm için Paketleri Yönet'i**seçin. **WindowsAzure.Storage'ı** arayın ve çözümdeki tüm projelere en son kararlı sürümü yükleyin.
4. Projenizi oluşturun ve çalıştırın.

Artık sayaç ları oluşturan bir düğmeyi tıklatmanızı sağlayan bir uygulamanız olmalıdır.

## <a name="create-container-and-upload-blob"></a>Kapsayıcı oluşturma ve blob yükleme

Ardından, projeniz altında, `(Portable)` ''ye `MyClass.cs`bazı kodlar eklersiniz. Bu kod bir kapsayıcı oluşturur ve bu kapsayıcıya bir leke yükler. `MyClass.cs`aşağıdaki gibi görünmelidir:

```csharp
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Blob;
using System.Threading.Tasks;

namespace XamarinApp
{
    public class MyClass
    {
        public MyClass ()
        {
        }

        public static async Task performBlobOperation()
        {
            // Retrieve storage account from connection string.
            CloudStorageAccount storageAccount = CloudStorageAccount.Parse("DefaultEndpointsProtocol=https;AccountName=your_account_name_here;AccountKey=your_account_key_here");

            // Create the blob client.
            CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

            // Retrieve reference to a previously created container.
            CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

            // Create the container if it doesn't already exist.
            await container.CreateIfNotExistsAsync();

            // Retrieve reference to a blob named "myblob".
            CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob");

            // Create the "myblob" blob with the text "Hello, world!"
            await blockBlob.UploadTextAsync("Hello, world!");
        }
    }
}
```

"your_account_name_here" ve "your_account_key_here" ile gerçek hesap adınız ve hesap anahtarınızı değiştirdiğinizden emin olun.

iOS, Android ve Windows Phone projelerinizin tümünün Taşınabilir projenize referansları vardır , yani paylaşılan kodlarınızın tümlerini tek bir yerde yazabilir ve tüm projelerinizde kullanabilirsiniz. Artık her projeye aşağıdaki kod satırını ekleyerek yararlanmaya başlayabilirsiniz:`MyClass.performBlobOperation()`

### <a name="xamarinappdroid--mainactivitycs"></a>XamarinApp.Droid > MainActivity.cs

```csharp
using Android.App;
using Android.Widget;
using Android.OS;

namespace XamarinApp.Droid
{
    [Activity (Label = "XamarinApp.Droid", MainLauncher = true, Icon = "@drawable/icon")]
    public class MainActivity : Activity
    {
        int count = 1;

        protected override async void OnCreate (Bundle bundle)
        {
            base.OnCreate (bundle);

            // Set our view from the "main" layout resource
            SetContentView (Resource.Layout.Main);

            // Get our button from the layout resource,
            // and attach an event to it
            Button button = FindViewById<Button> (Resource.Id.myButton);

            button.Click += delegate {
                button.Text = string.Format ("{0} clicks!", count++);
            };

            await MyClass.performBlobOperation();
            }
        }
    }
}
```

### <a name="xamarinappios--viewcontrollercs"></a>XamarinApp.iOS > ViewController.cs

```csharp
using System;
using UIKit;

namespace XamarinApp.iOS
{
    public partial class ViewController : UIViewController
    {
        int count = 1;

        public ViewController (IntPtr handle) : base (handle)
        {
        }

        public override async void ViewDidLoad ()
        {
            int count = 1;

            public ViewController (IntPtr handle) : base (handle)
            {
            }

            public override async void ViewDidLoad ()
            {
                base.ViewDidLoad ();
                // Perform any additional setup after loading the view, typically from a nib.
                Button.AccessibilityIdentifier = "myButton";
                Button.TouchUpInside += delegate {
                    var title = string.Format ("{0} clicks!", count++);
                    Button.SetTitle (title, UIControlState.Normal);
                };

                await MyClass.performBlobOperation();
            }

            public override void DidReceiveMemoryWarning ()
            {
                base.DidReceiveMemoryWarning ();
                // Release any cached data, images, etc. that aren't in use.
            }
        }
    }
}
```

### <a name="xamarinappwinphone--mainpagexaml--mainpagexamlcs"></a>XamarinApp.WinPhone > MainPage.xaml > MainPage.xaml.cs

```csharp
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Navigation;

// The Blank Page item template is documented at https://go.microsoft.com/fwlink/?LinkId=391641

namespace XamarinApp.WinPhone
{
    /// <summary>
    /// An empty page that can be used on its own or navigated to within a Frame.
    /// </summary>
    public sealed partial class MainPage : Page
    {
        int count = 1;

        public MainPage()
        {
            this.InitializeComponent();

            this.NavigationCacheMode = NavigationCacheMode.Required;
        }

        /// <summary>
        /// Invoked when this page is about to be displayed in a Frame.
        /// </summary>
        /// <param name="e">Event data that describes how this page was reached.
        /// This parameter is typically used to configure the page.</param>
        protected override async void OnNavigatedTo(NavigationEventArgs e)
        {
            int count = 1;

            public MainPage()
            {
                this.InitializeComponent();

                this.NavigationCacheMode = NavigationCacheMode.Required;
            }

            /// <summary>
            /// Invoked when this page is about to be displayed in a Frame.
            /// </summary>
            /// <param name="e">Event data that describes how this page was reached.
            /// This parameter is typically used to configure the page.</param>
            protected override async void OnNavigatedTo(NavigationEventArgs e)
            {
                // TODO: Prepare page for display here.

                // TODO: If your application contains multiple pages, ensure that you are
                // handling the hardware Back button by registering for the
                // Windows.Phone.UI.Input.HardwareButtons.BackPressed event.
                // If you are using the NavigationHelper provided by some templates,
                // this event is handled for you.
                Button.Click += delegate {
                    var title = string.Format("{0} clicks!", count++);
                    Button.Content = title;
                };

                await MyClass.performBlobOperation();
            }
        }
    }
}
```

## <a name="run-the-application"></a>Uygulamayı çalıştırma

Artık bu uygulamayı bir Android veya Windows Phone emülatöründe çalıştırabilirsiniz. Bu uygulamayı bir iOS emülatörde de çalıştırabilirsiniz, ancak bunun için Mac gerekir. Bunun nasıl yapılacılailgili çağrılari için, Visual Studio' yu Mac' e [bağlamak](https://developer.xamarin.com/guides/ios/getting_started/installation/windows/connecting-to-mac/) için gerekli belgeleri

Uygulamanızı çalıştırdıktan sonra, Depolama `mycontainer` hesabınızda kapsayıcı oluşturur. İçinde metin olan blob `myblob` `Hello, world!`içermelidir. [Microsoft Azure Depolama Gezgini'ni](https://storageexplorer.com/)kullanarak bunu doğrulayabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, Xamarin'de Azure Depolama'yı kullanan ve özellikle Blob Depolama'daki tek bir senaryoya odaklanan bir çapraz platform uygulaması oluşturmayı öğrendiniz. Ancak, yalnızca Blob Depolama ile değil, Tablo, Dosya ve Sıra Depolama ile de çok daha fazlasını yapabilirsiniz. Daha fazla bilgi için lütfen aşağıdaki makalelere göz atın:

* [.NET kullanarak Azure Blob Storage’ı kullanmaya başlayın](storage-dotnet-how-to-use-blobs.md)
* [Azure Dosyaları'na giriş](../files/storage-files-introduction.md)
* [.NET ile Azure Dosyaları için geliştirme](../files/storage-dotnet-how-to-use-files.md)
* [.NET kullanarak Azure Table Storage’ı kullanmaya başlayın](../../cosmos-db/table-storage-how-to-use-dotnet.md)
* [.NET kullanarak Azure Kuyruk Depolamaya başlayın](../queues/storage-dotnet-how-to-use-queues.md)

[!INCLUDE [storage-try-azure-tools-blobs](../../../includes/storage-try-azure-tools-blobs.md)]
