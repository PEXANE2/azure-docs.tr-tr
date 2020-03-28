---
title: 'Öğretici: Yeni bir HoloLens Unity uygulaması oluşturun'
description: Bu eğitimde, Azure Uzamsal Çapaları kullanarak yeni bir HoloLens Birlik uygulaması oluşturmayı öğrenirsiniz.
author: julianparismorgan
manager: vriveras
services: azure-spatial-anchors
ms.author: pmorgan
ms.date: 07/05/2019
ms.topic: tutorial
ms.service: azure-spatial-anchors
ms.openlocfilehash: e1abb759c80e770f1e650c232b6b2e21232b7e6f
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "75457729"
---
# <a name="tutorial-step-by-step-instructions-to-create-a-new-hololens-unity-app-using-azure-spatial-anchors"></a>Öğretici: Azure Uzamsal Çapaları kullanarak yeni bir HoloLens Birlik uygulaması oluşturmak için adım adım talimatlar

Bu öğretici, Azure Uzamsal Çapalar ile yeni bir HoloLens Birlik uygulaması nın nasıl oluşturulacağını gösterir.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için şunlar sahip olduğunuzdan emin olun:

1. Visual Studio <a href="https://www.visualstudio.com/downloads/" target="_blank">2017+</a> ile **Evrensel Windows Platformu geliştirme** iş yükü ve Windows **10 SDK (10.0.18362.0 veya daha yeni)** bileşeni ile yüklü bir Windows makinesi ve <a href="https://git-scm.com/download/win" target="_blank">Windows için Git</a>.
2. Visual Studio için [C++/WinRT Visual Studio Extension (VSIX)](https://aka.ms/cppwinrt/vsix) [Visual Studio Marketplace'ten](https://marketplace.visualstudio.com/)yüklenmelidir.
3. [Geliştirici modu](https://docs.microsoft.com/windows/mixed-reality/using-visual-studio) etkin bir HoloLens cihazı. Bu makale, [Windows 10 Ekim 2018 Güncelleştirmesi](https://docs.microsoft.com/windows/mixed-reality/release-notes-october-2018 ) (RS5 olarak da bilinir) içeren bir HoloLens aygıtı gerektirir. HoloLens'teki en son sürümde güncelleme yapmak için **Ayarlar** uygulamasını açın, **& Güvenliği Güncelleştir'e**gidin ve ardından **güncellemeler için Kontrol** düğmesini seçin.

## <a name="getting-started"></a>Başlarken

Önce projemizi ve Birlik sahnemizi kuracağız:
1. Birlik'i başlatın.
2. **Yeni'yi**seçin.
4. **3B'nin** seçildiğinden emin olun.
5. Projenizi adlandırın ve **konum**kaydet'i girin.
6. **Proje Oluştur'u**tıklatın.
7. Boş varsayılan sahneyi yeni bir dosyaya kaydedin: **Dosya** > **YıKın**.
8. Yeni sahne **ana** adını ve **Kaydet** düğmesine basın.

**Proje ayarlarını ayarlama**

Şimdi, geliştirme için Windows Holografik SDK'yı hedeflememize yardımcı olacak bazı Unity proje ayarlarını ayarlayacağız.

Öncelikle uygulamamız için kalite ayarlarını ayarlamanızı sağlar.
1. **Proje Ayarlarını** >  **İyile** > **Özelliğini** Seçin
2. **Windows Mağazası** logosunun altındaki **sütunda** Varsayılan satırdaki oka tıklayın ve **Çok Düşük'u**seçin. **Windows Mağazası** sütunundaki ve **Çok Düşük** satırdaki kutu yeşil olduğunda ayarın doğru uygulandığını anlarsınız.

Birliğe, dışa aktarmaya çalıştığımız uygulamanın 2B görünüm yerine sürükleyici bir görünüm oluşturması gerektiğini bildirmeliyiz. Windows 10 SDK'yı hedefleyen Unity'de Sanal Gerçeklik desteğini etkinleştirerek sürükleyici bir görünüm oluşturuyoruz.

1. **Proje Ayarlarını** >  **Edit** > **Player'a**gidin.
2. **Oyuncu Ayarları** **Denetleç Paneli'nde** Windows **Mağazası** simgesini seçin.
3. **XR Ayarları** grubunu genişletin.
4. **Oluşturma** bölümünde, yeni bir **Sanal Gerçeklik SDK** listesi eklemek için **Sanal Gerçeklik Destekli** onay kutusunu işaretleyin.
5. Windows **Karma Gerçekliğin** listede göründüğünü doğrulayın. Değilse, listenin **+** altındaki düğmeyi seçin ve **Windows Karışık Gerçeklik'i**seçin.

> [!NOTE]
> Windows Mağazası simgesini görmüyorsanız, yüklemeden önce Windows Mağazası .NET Komut Dosyası Arka Uç'unu seçtiğinizden emin olmak için iki kez denetleyin. Değilse, doğru Windows yüklemesi ile Unity'yi yeniden yüklemeniz gerekebilir.

**Komut Dosyası Arka Uç yapılandırması doğrula**
1. **Proje Ayarlarını** >  **Edit** > **Player'a** gidin **(Player'ı** bir önceki adımdan itibaren açık olmaya devam edebilirsiniz).
2. **Oyuncu Ayarları** **Denetleç Paneli'nde** Windows **Mağazası** simgesini seçin.
3. Diğer **Ayarlar** Yapılandırması bölümünde, **Komut Dosyası Arka** **Uç'unun IL2CPP**olarak ayarlandıklarına emin olun.

**Yetenekleri ayarlama**
1. **Proje Ayarlarını** >  **Edit** > **Player'a** gidin **(Player'ı** bir önceki adımdan itibaren açık olmaya devam edebilirsiniz).
2. **Oyuncu Ayarları** **Denetleç Paneli'nde** Windows **Mağazası** simgesini seçin.
3. **Yayımlama Ayarları** Yapılandırma **bölümünde, InternetClientServer** ve **SpatialPerception'ı**kontrol edin.

**Ana sanal kamerayı ayarlama**
1. Hiyerarşi **Paneli'nde** **Ana Kamera'yı**seçin.
2. **Müfettiş**olarak, dönüşüm konumunu **0,0,0**olarak ayarlayın.
3. Açık **Bayraklar** özelliğini bulun ve açılır pencereleri **Skybox'tan** **Katı Renk'e**değiştirin.
4. Renk seçiciyi açmak için **Arka Plan** alanına tıklayın.
5. **R, G, B ve A'yı** **0'a**ayarlayın.
6. **Bileşen Ekle'yi** seçin ve **Uzamsal Eşleme Çarpıştırıcısını**arayın ve ekleyin.

**Komut dosyamızı oluşturun**
1. **Proje** bölmesinde, **Varlıklar** klasörü altında yeni bir klasör, **Komut Dosyaları**oluşturun.
2. Klasöre sağ tıklayın, ardından **>Oluştur **, **C# Script'i**seçin. Başlık o **AzureSpatialAnchorsScript**.
3. **GameObject'e** -> Git**Boş Oluştur**.
4. Seçin ve **Denetçi'de** **GameObject'den** **MixedRealityCloud'a**yeniden adlandırın. **Bileşen Ekle'yi** seçin ve **AzureSpatialAnchorsScript'i**arayın ve ekleyin.

**Küre prefabrik oluşturun**
1. **GameObject** -> **3D Nesne** -> **Küresine**gidin.
2. **Müfettiş**olarak, **0.25, 0.25, 0.25**için ölçek ayarlayın.
3. **Hiyerarşi** bölmesinde **Küre** nesnesini bulun. Üzerine tıklayın ve **Proje** bölmesindeki **Varlıklar** klasörüne sürükleyin.
4. Sağ tıklatın ve **Hiyerarşi** bölmesinde oluşturduğunuz orijinal küreyi **silin.**

Şimdi **Proje** bölmenizde bir küre prefabrik olmalıdır.

## <a name="trying-it-out"></a>Denemek
Her şeyin işe yaradığını test etmek için, uygulamanızı **Unity'de** oluşturun ve **Visual Studio'dan**dağıtın. [ **MR Basics 100'den** ](https://docs.microsoft.com/windows/mixed-reality/holograms-100#chapter-6---build-and-deploy-to-device-from-visual-studio) Bölüm 6'yı takip edin: Bunu yapmak için Unity kursuna başlamak. Birlik başlangıç ekranını ve ardından net bir ekranı görmelisiniz.

## <a name="place-an-object-in-the-real-world"></a>Bir nesneyi gerçek dünyaya yerleştirme
Uygulamanızı kullanarak bir nesne & oluşturalım. [Uygulamamızı dağıttığımızda](#trying-it-out)oluşturduğumuz Visual Studio çözümlerini açın.

İlk olarak, aşağıdaki içeri `Assembly-CSharp (Universal Windows)\Scripts\AzureSpatialAnchorsScript.cs`alma ekleyin:

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=19-24)]

Ardından, sınıfınıza `AzureSpatialAnchorsScript` aşağıdaki üye değişkenlerini ekleyin:

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=26-42,48-52,60-79)]

Devam etmeden önce, oluşturduğumuz küre prefabrikini kürePrefabrik üye değişkenimiz üzerine ayarlamamız gerekir. **Birlik'e**geri dön.
1. **Unity'de,** **Hiyerarşi** bölmesinde **MixedRealityCloud** nesnesini seçin.
2. **Proje** bölmesine kaydettiğiniz **Küre** prefabrikine tıklayın. Tıklattıkladığınız **Küreyi,** **Denetçi** bölmesinde **Azure Uzamsal Çapalar Komut Dosyası (Script)** altında **Küre Prefabrik** alanına sürükleyin.

Şimdi **Küre'yi** senaryonuzda prefabrik olarak ayarlamalısınız. **Unity'den** oluşturun ve daha sonra sadece [denemede](#trying-it-out)yaptığınız gibi, ortaya çıkan **Visual Studio** çözümlerini tekrar açın.

**Visual Studio'da** `AzureSpatialAnchorsScript.cs` tekrar açın. Yönteminizin `Start()` içine aşağıdaki kodu ekleyin. Bu kod, `GestureRecognizer`bir hava musluk ve arama `HandleTap`olduğunda algılar, kanca olacaktır.

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=81-90,93&highlight=4-10)]

Şimdi aşağıdaki `HandleTap()` yöntemi eklemek `Update()`zorunda . Bir ışın döküm yapacak ve hangi bir küre yerleştirmek için bir hit noktası olsun.

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=267-277,299-300,304-312)]

Şimdi küreyi yaratmamız gerekiyor. Küre başlangıçta beyaz olacak, ancak bu değer daha sonra ayarlanır. Aşağıdaki `CreateAndSaveSphere()` yöntemi ekleyin:

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=314-325,390)]

Bir kez daha doğrulamak için uygulamanızı **Visual Studio'dan** çalıştırın. Bu kez, beyaz kürenizi seçtiğiniz yüzeyin üzerine yerleştirmek & oluşturmak için ekrana dokunun.

## <a name="set-up-the-dispatcher-pattern"></a>Sevk irsaliyesi deseni ayarlama

Unity ile çalışırken, örneğin UI güncelleştirmelerini yapmak için kullandığınız API'ler gibi tüm Unity API'lerin ana iş parçacığıüzerinde gerçekleşmesi gerekir. Ancak yazacağız kodda, diğer iş parçacıkları üzerinde geri arama lar olsun. Bu geri aramalarda kullanıcı arasını güncellemek istiyoruz, bu yüzden bir yan iş parçacığından ana iş parçacığına geçmenin bir yolunu bulmalıyız. Bir yan iş parçacığından ana iş parçacığı üzerinde kod yürütmek için, sevk deseni kullanacağız.

Eylemler Sırası olan bir üye değişkeni, dispatchQueue ekleyelim. Eylemler'i kuyruğa iteceğiz ve ardından ana iş parçacığındaki Eylemleri dequeue'yi çalıştıracağız.

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=38-51&highlight=6-9)]

Ardından, Sıraya Eylem eklemenin bir yolunu ekleyelim. Hemen `QueueOnUpdate()` sonra `Update()` ekle:

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=107-117)]

Şimdi, sıraya bir Eylem olup olmadığını kontrol etmek için Update() döngülerini kullanalım. Eğer öyleyse, eylemi bozup çalıştıracağız.

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=95-105&highlight=4-10)]

## <a name="get-the-azure-spatial-anchors-sdk"></a>Azure Uzamsal Çapalar SDK alın

## <a name="via-unity-package"></a>[Via Unity Paketi](#tab/UnityPackage)

Şimdi Azure Uzamsal Çapalar SDK'yı indireceğiz. [Azure Uzamsal Çapalar GitHub sürümleri sayfasına](https://github.com/Azure/azure-spatial-anchors-samples/releases)gidin. Varlıklar altında **AzureSpatialAnchors.unitypaketini**indirin. Unity'de **Varlıklar'a**gidin, **İthalat Paketi** > **Özel Paketi'ni tıklatın...** Pakete gidin ve **Aç'ı**seçin.

Açılan yeni **İçe Alma Birliği Paketi** **penceresinde, Eklentileri** seçin ve ardından sağ alt köşede **İçe Aktar'ı** tıklatın.

Azure Uzamsal Çapalar SDK'yı almak için artık Nuget paketlerini geri yüklememiz gerekiyor. **Unity'den** oluşturun ve ardından, [denemede](#trying-it-out)ayrıntılı olarak belirtildiği gibi, ortaya çıkan **Visual Studio** çözümlerini yeniden açın ve oluşturun.

## <a name="via-nugetforunity"></a>[NugetforUnity üzerinden](#tab/NuGetForUnity)

Öncelikle NuGetForUnity'yi yüklememiz gerekiyor. [NuGetForUnity GitHub bültenleri sayfasına](https://github.com/GlitchEnzo/NuGetForUnity/releases)gidin. Varlıklar altında, en son **NuGetForUnity.unitypackage**indirin. Unity'de **Varlıklar'a**gidin, **İthalat Paketi** > **Özel Paketi'ni tıklatın...** Pakete gidin ve **Aç'ı**seçin. Birlik şimdi NugetForUnity kuracak. Unity'de yeni bir **NuGet** açılır düşüşü görmüyorsanız, **Projects** > **Assets**altında sağ tıklamanız gerekebilir. Ardından **Tümünü Yeniden Aktar'ı**seçin.

NuGetForUnity yüklendikten sonra **NuGet** > **Manage NuGet Paketlerini**seçin. Ardından Microsoft.Azure.SpatialAnchors.Unity'yi arayın ve **Yükle'yi**seçin.

Şimdi gerçek Azure Uzamsal Çapalar SDK almak için inşa etmek gerekir, nuget paketi biz sadece yardımcı komut içerir gibi. **Unity'den** oluşturun ve ardından, [denemede](#trying-it-out)ayrıntılı olarak belirtildiği gibi, ortaya çıkan **Visual Studio** çözümlerini yeniden açın ve oluşturun.

---

Visual **Studio** çözümünüzde aşağıdaki içe `<ProjectName>\Assets\Scripts\AzureSpatialAnchorsScript.cs`aktarmayı ekleyin:

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=18-21&highlight=1)]

Ardından, sınıfınıza `AzureSpatialAnchorsScript` aşağıdaki üye değişkenleri ekleyin:

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=48-63&highlight=6-11)]

## <a name="attach-a-local-azure-spatial-anchor-to-the-local-anchor"></a>Yerel çapaya yerel bir Azure Uzamsal Çapa ekleme

Azure Uzamsal Çapa'nın CloudSpatialAnchorSession'ı ayarlayalım. Sınıfınızın içine aşağıdaki `InitializeSession()` yöntemi ekleyerek `AzureSpatialAnchorsScript` başlayacağız. Çağrıldıktan sonra, uygulamanızın başlatılması sırasında bir Azure Uzamsal Çapa oturumu oluşturulmasını ve düzgün bir şekilde başlatılmasını sağlar.

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=174-202,205-209)]

Şimdi temsilci çağrıları işlemek için kod yazmak gerekir. Devam ettikçe onlara daha fazlasını ekleyeceğiz.

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=211-226)]

Şimdi, yöntemini `initializeSession()` senin yönteminle `Start()` bağlayalım.

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=81-93&highlight=12)]

Son olarak, yönteminize `CreateAndSaveSphere()` aşağıdaki kodu ekleyin. Gerçek dünyada yerleştirdiğimiz küreye yerel bir Azure Uzamsal Çapa ekleyecek.

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=314-338,390&highlight=14-25)]

Daha fazla ilerlemeden önce, zaten sahip değilseniz bir Azure Uzamsal Çapa hesap Tanımlayıcısı ve Anahtarı oluşturmanız gerekir. Bunları elde etmek için aşağıdaki bölümü izleyin.

[!INCLUDE [Create Spatial Anchors resource](../../../includes/spatial-anchors-get-started-create-resource.md)]

## <a name="upload-your-local-anchor-into-the-cloud"></a>Yerel çapanızı buluta yükleyin

Azure Uzamsal Çapalar hesap Tanımlayıcınızı ve Anahtarınızı aldıktan sonra, `SpatialAnchorsAccountId` gidin `Account Key` `SpatialAnchorsAccountKey`ve `Account Id` içine ve içine yapıştırın.

Son olarak, her şeyi birbirine bağlayalım. Yönteminizde `SpawnNewAnchoredObject()` aşağıdaki kodu ekleyin. Küreniz oluşturulur `CreateAnchorAsync()` oluşturulmaz yöntemi çağırır. Yöntem döndükten sonra, aşağıdaki kod kürenize son bir güncelleştirme gerçekleştirerek rengini maviye değiştirir.

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=314-391&highlight=26-77)]

Uygulamanızı **Visual Studio'dan** bir kez daha çalıştırın. Kürenizi yerleştirmek için kafanızın etrafında hareket edin ve sonra hava musluğunun etrafında hareket edin. Yeterli çerçeveye sahip olduğumuzda küre sarıya döner ve bulut yüklemesi başlar. Yükleme tamamlandığında küreniz maviye dönecektir. İsteğe bağlı olarak, uygulamanızın gönderdiği günlük iletilerini izlemek için **Visual Studio'daki** Çıktı penceresini de kullanabilirsiniz. İlerleme oluşturmak için önerilen ilerlediği ve yükleme tamamlandıktan sonra bulutun geri döndüğü bağlantı tanımlayıcısını izleyebilirsiniz.

> [!NOTE]
> "DllNotFoundException: DLL 'AzureSpatialAnchors' yüklenemiyorsanız: Belirtilen modül bulunamadı." **Clean** **Build**

## <a name="locate-your-cloud-spatial-anchor"></a>Bulut uzamsal çapanızı bulun

Çapanızın buluta yüklenmesi, onu tekrar bulmaya hazırız. Yönteminizin `HandleTap()` içine aşağıdaki kodu ekleyelim. Bu kod:

* Çağrı `ResetSession()`, hangi `CloudSpatialAnchorSession` durdurmak ve ekrandan mevcut mavi küre kaldırmak.
* Tekrar `CloudSpatialAnchorSession` baş harflerini. Bunu, bulabildiğimiz çapanın oluşturduğumuz yerel çapa olmak yerine buluttan geldiğinden emin olmak için yapıyoruz.
* Azure Uzamsal Çapalar'a yüklediğimiz çapayı arayacak bir **Gözleyici** oluşturun.

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=267-305&highlight=13-31,35-36)]

Şimdi bizim `ResetSession()` ve `CleanupObjects()` yöntemleri ekleyelim. Bunları aşağıda koyabilirsiniz`QueueOnUpdate()`

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=119-172)]

Şimdi, sorguladığımız bağlantı bulunduğunda çağrılacak kodu bağlamamız gerekiyor. `InitializeSession()`İçinde, aşağıdaki geri aramaları ekleyin:

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=200-206&highlight=4-5)]


Şimdi CloudSpatialAnchor bulunduğunda yeşil bir küre & oluşturacak kod eklemenize olanak sağlar. Ayrıca ekrana tekrar dokunmayı da etkinleştirecek, böylece tüm senaryoyu bir kez daha tekrarlayabilirsiniz: başka bir yerel bağlantı noktası oluşturun, yükleyin ve yeniden bulun.

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=228-265)]

İşte bu kadar! Tüm senaryonun sonunu denemek için uygulamanızı **Visual Studio'dan** son bir kez çalıştırın. Cihazınızın etrafında hareket ettirin ve beyaz kürenizi yerleştirin. Ardından, küre sarıya dönene kadar çevre verilerini yakalamak için başınızı hareket ettirmeye devam edin. Yerel çapanız yüklenir ve küreniz maviye döner. Son olarak, yerel bağlantınızın kaldırılması için ekranınıza bir kez daha dokunun ve bulut muadili için sorgu yaparız. Bulut uzamsal çapanız bulunana kadar cihazınızı hareket ettirin. Yeşil bir küre doğru konumda görünmelidir ve tüm senaryoyu tekrar & durulayabilirsiniz.

[!INCLUDE [AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md)]