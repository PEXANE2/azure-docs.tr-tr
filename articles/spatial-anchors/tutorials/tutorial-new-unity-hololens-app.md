---
title: 'Öğretici: yeni bir HoloLens Unity uygulaması oluşturma'
description: Bu öğreticide, Azure uzamsal bağlayıcıları kullanarak yeni bir HoloLens Unity uygulaması oluşturmayı öğreneceksiniz.
author: craigktreasure
manager: vriveras
services: azure-spatial-anchors
ms.author: crtreasu
ms.date: 08/17/2020
ms.topic: tutorial
ms.service: azure-spatial-anchors
ms.openlocfilehash: 365fe8c330cadcc01fcd24de28b663cd80b55117
ms.sourcegitcommit: c52e50ea04dfb8d4da0e18735477b80cafccc2cf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/08/2020
ms.locfileid: "89535896"
---
# <a name="tutorial-step-by-step-instructions-to-create-a-new-hololens-unity-app-using-azure-spatial-anchors"></a>Öğretici: Azure uzamsal bağlayıcıları kullanarak yeni bir HoloLens Unity uygulaması oluşturmaya yönelik adım adım yönergeler

Bu öğreticide, Azure uzamsal bağlayıcılarla yeni bir HoloLens Unity uygulamasının nasıl oluşturulacağı gösterilmektedir.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için şunlar sahip olduğunuzdan emin olun:

1. **Evrensel Windows platformu geliştirme** iş yükü ve **Windows 10 SDK (10.0.18362.0 veya üzeri)** bileşeni ve <a href="https://git-scm.com/download/win" target="_blank">Windows için git</a>ile <a href="https://www.visualstudio.com/downloads/" target="_blank">Visual Studio 2017 +</a> yüklü bir Windows makinesi.
2. Visual Studio için [C++/Wınrt Visual Studio uzantısı (VSIX)](https://aka.ms/cppwinrt/vsix) [Visual Studio Market](https://marketplace.visualstudio.com/)yüklenmelidir.
3. [Geliştirici modu](https://docs.microsoft.com/windows/mixed-reality/using-visual-studio) etkin bir HoloLens cihazı. Bu makalede, [Windows 10 ekim 2018 güncelleştirmesi](https://docs.microsoft.com/windows/mixed-reality/release-notes-october-2018 ) (RS5 olarak da bilinir) Ile bir HoloLens cihazı gerekir. HoloLens 'te en son sürüme güncelleştirmek için **Ayarlar** uygulamasını açın, **güncelleştirme & güvenlik**' e gidin ve **Güncelleştirmeleri denetle** düğmesini seçin.

## <a name="getting-started"></a>Başlarken

İlk olarak proje ve Unity sahümüzü ayarlayacağız:
1. Unity 'yi başlatın.
2. **Yeni**’yi seçin.
4. **3B** 'in seçili olduğundan emin olun.
5. Projenizi adlandırın ve bir kaydetme **konumu**girin.
6. **Create project** (Proje oluştur) öğesini seçin.
7. Boş varsayılan sahneyi kullanarak yeni bir dosyaya kaydedin: **Dosya**  >  **farklı kaydet**.
8. Yeni sahneyi **Main** olarak adlandırın ve **Kaydet** düğmesine basın.

**Proje ayarlarını ayarlama**

Şimdi geliştirme için Windows holographic SDK 'sını hedefmize yardımcı olan bazı Unity proje ayarlarını ayarlayacağız.

İlk olarak, uygulamamız için kalite ayarlarını ayarlayalım.
1. **Edit**  >  **Proje ayarlarını**Düzenle  >  **kalitesini** seçin
2. **Windows Mağazası** logosunun altındaki sütunda, **varsayılan** satırdaki oka tıklayın ve **çok düşük**' ı seçin. **Windows Mağazası** sütununda ve **çok düşük** satırdaki kutu yeşil olduğunda ayarın doğru uygulandığını bilirsiniz.

Unity uygulamamızı 2B görünüm yerine bir tam ekran görünümüyle yapılandırmamız gerekir. Windows 10 SDK 'Yı hedefleyen Unity üzerinde sanal gerçeklik desteğini etkinleştirerek bir derinlikli görünüm oluşturabiliyoruz.
1. **Edit**  >  **Proje ayarları**  >  **oynatıcıyı**Düzenle ' ye gidin.
2. **Oynatıcı ayarları**Için **Inspector panelinde** **Windows** simgesini seçin.
3. **XR ayarları** grubunu genişletin.
4. **Oluşturma** bölümünde, yeni bir **sanal gerçeklik SDK** listesi eklemek için **sanal gerçeklik destekleniyor** onay kutusunu işaretleyin.
5. **Windows Mixed Reality** 'nin listede göründüğünü doğrulayın. Aksi takdirde, **+** listenin altındaki düğmeyi seçin ve **Windows Mixed Reality**' yi seçin.

> [!NOTE]
> Windows simgesini görmüyorsanız, yüklemeden önce Windows .NET betiği arka ucunu seçtiğinizden emin olmak için çift işaretleyin. Aksi takdirde, Unity 'yi doğru Windows yüklemesiyle yeniden yüklemeniz gerekebilir.

**Komut dosyası arka uç yapılandırmasını doğrula**
1. **Edit**  >  **Proje ayarları**  >  **oyuncusunu** Düzenle ' ye gidin (hala **Player 'ın** önceki adımdan açık olması gerekebilir).
2. **Oynatıcı ayarları**Için **Inspector panelinde** **Windows Mağazası** simgesini seçin.
3. **Diğer ayarlar** yapılandırması bölümünde, **komut dosyası arka** ucunun **IL2CPP**olarak ayarlandığından emin olun.

**Özellikleri ayarla**
1. **Edit**  >  **Proje ayarları**  >  **oyuncusunu** Düzenle ' ye gidin (hala **Player 'ın** önceki adımdan açık olması gerekebilir).
2. **Oynatıcı ayarları**Için **Inspector panelinde** **Windows Mağazası** simgesini seçin.
3. **Yayımlama ayarları** yapılandırması bölümünde, **ınternetclientserver** ve **spatialperception**' yı denetleyin.

**Ana sanal kamerayı ayarlama**
1. **Hiyerarşi panelinde**, **ana kamera**' ı seçin.
2. **Denetçisinde**, dönüştürme konumunu **0, 0, 0**olarak ayarlayın.
3. **Clear Flags** özelliğini bulun ve açılan menüyü **ufuk kutusundan** **düz renk**olarak değiştirin.
4. Bir renk seçici açmak için **arka plan** alanına tıklayın.
5. **R, G, B ve A** 'yı **0**olarak ayarlayın.
6. **Bileşen Ekle** ' yi seçin ve **uzamsal eşleme Collider**eklemek için arama yapın.

**Betiğimizi oluşturun**
1. **Proje** bölmesinde, **varlıklar** klasörü altında yeni bir klasör, **komut dosyaları**oluşturun.
2. Klasöre sağ tıklayın ve ardından >, **C# betiği** **Oluştur **' u seçin. Başlık **AzureSpatialAnchorsScript**.
3. **Oyun nesnesi**  ->  **Oluştur boş**öğesine gidin.
4. Bunu seçin ve **Inspector** 'Da **Gameobject** Iken **mixedrealitycloud**olarak yeniden adlandırın. **Bileşen Ekle** ' yi seçin ve **AzureSpatialAnchorsScript**ekleyin ve ekleyin.

**Sphere prefab oluşturma**
1. **Gameobject**  ->  **3B nesne**  ->  **Sphere**öğesine gidin.
2. **Denetçisinde**, ölçeğini **0,25, 0,25, 0,25**olarak ayarlayın.
3. **Hiyerarşi** bölmesinde **Sphere** nesnesini bulun. Üzerine tıklayın ve **Proje** bölmesindeki **varlıklar** klasörüne sürükleyin.
4. **Hiyerarşi** bölmesinde oluşturduğunuz orijinal küreyi sağ tıklayıp **silin** .

Artık **Proje** bölmesinizdeki bir Sphere prefab sahibi olmanız gerekir.

## <a name="trying-it-out"></a>Deneniyor
Her şeyin çalıştığını test etmek için, uygulamanızı **Unity** 'de derleyin ve **Visual Studio**'dan dağıtın. [ **Mr temel kuralları 100: Unity kursu ile çalışmaya** ](https://docs.microsoft.com/windows/mixed-reality/holograms-100#chapter-6---build-and-deploy-to-device-from-visual-studio) başlama başlıklı Bölüm 6 ' yı izleyin. Unity başlangıç ekranı ' nı ve ardından temiz bir ekran görmeniz gerekir.

## <a name="place-an-object-in-the-real-world"></a>Gerçek dünyaya bir nesne yerleştirme
Uygulamanızı kullanarak bir nesne oluşturalım &. [Uygulamamızı dağıttığımızda](#trying-it-out)oluşturduğumuz Visual Studio çözümünü açın.

İlk olarak, aşağıdaki içeri aktarmaları içine ekleyin `Assembly-CSharp (Universal Windows)\Scripts\AzureSpatialAnchorsScript.cs` :

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=19-24)]

Ardından, aşağıdaki üye değişkenlerini `AzureSpatialAnchorsScript` sınıfınıza ekleyin:

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=26-47,53-57,65-84)]

Devam etmeden önce, Spburprefab üye değişkenimizde oluşturduğumuz Sphere ön kümesini ayarlamanız gerekir. **Unity**'ye geri dönün.
1. **Unity**'de **hiyerarşi** bölmesinde **mixedrealitycloud** nesnesini seçin.
2. **Proje** bölmesinde kaydettiğiniz **Sphere** prefab öğesine tıklayın. Tıklattığınız **Sphere** öğesini **Inspector** bölmesindeki **Azure uzamsal bağlayıcı betiği (betik)** altındaki **Sphere prefab** alanına sürükleyin.

Artık betiğinizdeki ön **küm** kümesine sahip olmanız gerekir. **Unity** 'den derleyin ve ardından ortaya çıkan **Visual Studio** çözümünü yeniden [açın.](#trying-it-out)

**Visual Studio**'da `AzureSpatialAnchorsScript.cs` yeniden açın. Aşağıdaki kodu `Start()` yöntemine ekleyin. Bu kod `GestureRecognizer` , `HandleTap` bir hava dokunusını algıladığında çağracaktır.

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=86-95,98&highlight=4-10)]

Şimdi aşağıdaki yöntemi eklememiz gerekir `HandleTap()` `Update()` . Bu, bir Ray cast ve bir kürenin yerleştirileceği bir isabet noktası alır.

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=273-283,305-306,310-318)]

Şimdi Sphere öğesini oluşturuyoruz. Sphere başlangıçta beyaz olur, ancak bu değer daha sonra üzerinde ayarlanır. Aşağıdaki yöntemi ekleyin `CreateAndSaveSphere()` :

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=320-331,396)]

Uygulamanızı bir kez daha doğrulamak için **Visual Studio** 'dan çalıştırın. Bu kez, beyaz küetinizi istediğiniz yüzey üzerine yerleştirmek & oluşturmak için ekrana dokunun.

## <a name="set-up-the-dispatcher-pattern"></a>Dağıtıcı modelini ayarlama

Unity ile çalışırken, tüm Unity API 'Lerinin (örneğin, UI güncelleştirmelerini yapmak için kullandığınız API 'Ler) ana iş parçacığında gerçekleştirilmesi gerekir. Ancak yazılacak kodda, diğer iş parçacıklarında geri çağrılar yaptık. Bu geri çağırmalar içindeki kullanıcı arabirimini güncelleştirmek istiyoruz, bu nedenle bir yan iş parçacığından ana iş parçacığına gitmenin bir yolu olmalıdır. Bir yan iş parçacığından ana iş parçacığında kod yürütmek için dağıtıcı modelini kullanacağız.

Bir eylem sırası olan bir üye değişkeni ekleyelim `dispatchQueue` . Eylemleri sıraya göndereceğiz ve sonra ana iş parçacığında eylemleri yeniden kuyruğa alacak ve çalıştıracağız.

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=43-56&highlight=6-9)]

Ardından, kuyruğa eylem eklemenin bir yolunu ekleyelim. `QueueOnUpdate()`Hemen sonra Ekle `Update()` :

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=112-122)]

Kuyruğa alınmış bir eylem olup olmadığını denetlemek için Update () döngüsünü kullanabiliriz. Varsa, eylemi sıradan çıkar ve çalıştıracağız.

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=100-110&highlight=4-10)]

## <a name="get-the-azure-spatial-anchors-sdk"></a>Azure uzamsal çıpası SDK 'sını alın

## <a name="via-unity-package-manager-upm-package"></a>[Unity paket yöneticisi (UPD) paketi aracılığıyla](#tab/UPMPackage)

Bu yöntem Unity sürümleriyle uyumludur 2019.1 +.

### <a name="add-the-registry-to-your-unity-project"></a>Kayıt defterini Unity projenize ekleme

1. Dosya Gezgini 'nde Unity projenizin `Packages` klasörüne gidin. Proje bildirim dosyasını `manifest.json` bir metin düzenleyicisinde açın.
2. Dosyanın en üstünde, bölümüyle aynı düzeyde, `dependencies` Azure uzamsal bağlayıcı kayıt defterini projenize dahil etmek için aşağıdaki girişi ekleyin. `scopedRegistries`Giriş, Unity 'ye Azure uzamsal Tutturucuların SDK paketlerini nerede bakacağını söyler.

    [!code-json[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-unity-scoped-registry-setup.md?range=9-19&highlight=2-10)]

### <a name="add-the-sdk-package-to-your-unity-project"></a>Unity projenize SDK paketini ekleme

1. Azure uzamsal bağlayıcılarının Windows SDK paket adı ( `com.microsoft.azure.spatial-anchors-sdk.windows` ) ve paket sürümünü `dependencies` Proje bildiriminizde bölümüne ekleyin. Aşağıdaki örneğe bakın.

    [!code-json[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-unity-scoped-registry-setup.md?range=9-20&highlight=12)]

2. Dosyayı kaydedin ve kapatın `manifest.json` . Unity 'ye geri döndüğünüzde, Unity 'nin proje bildirimi değişikliğini otomatik olarak algılaması ve belirtilen paketleri alması gerekir. `Packages`Doğru paketlerin içeri aktarıldığını doğrulamak Için proje görünüminizdeki klasörü genişletebilirsiniz.

## <a name="via-unity-asset-package"></a>[Unity varlık paketi aracılığıyla](#tab/UnityAssetPackage)

> [!WARNING]
> Azure uzamsal Tutturucuların SDK 'sının Unity varlık paketi dağıtımı SDK sürümü 2.5.0 sonrasında kullanımdan kalkacaktır.

Azure uzamsal bağlayıcı SDK 'sını indirelim. [Azure uzamsal bağlantıları GitHub yayınları sayfasına](https://github.com/Azure/azure-spatial-anchors-samples/releases)gidin. **Varlıklar**altında **AzureSpatialAnchors. unitypackage**' ı indirin. Unity 'de **varlıklar**' a gidin, paket özel paketini **içeri aktar**  >  **..**. seçeneğini belirleyin. Pakete gidin ve **Aç**' ı seçin.

Açılan yeni **Unity paketi Içeri aktar** penceresinde, **eklentilerin** seçimini kaldırın ve sağ alt köşedeki **içeri aktar** ' ı seçin.

---

**Visual Studio** çözümünüzde aşağıdaki içeri aktarmayı ekleyin `<ProjectName>\Assets\Scripts\AzureSpatialAnchorsScript.cs` :

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=18-21&highlight=1)]

Ardından, aşağıdaki üye değişkenlerini `AzureSpatialAnchorsScript` sınıfınıza ekleyin:

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=53-68&highlight=6-11)]

## <a name="attach-a-local-azure-spatial-anchor-to-the-local-anchor"></a>Yerel bağlantı noktasına yerel bir Azure uzamsal Bağlayıcısı ekleyin

Azure uzamsal bağlayıcının CloudSpatialAnchorSession ayarlayalim. Aşağıdaki yöntemi sınıfınızın içine ekleyerek başlayacağız `InitializeSession()` `AzureSpatialAnchorsScript` . Bir kez çağrıldıktan sonra, uygulamanızın başlatılması sırasında bir Azure uzamsal bağlayıcı oturumunun oluşturulduğundan ve düzgün başlatılmış olduğundan emin olur.

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=179-208,211-215)]

Şimdi temsilci çağrılarını işlemek için kod yazdık. Devam ettiğimiz için bunlara daha fazla ekleyeceğiz.

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=217-232)]

Şimdi, yönteminizi uygulamanıza verlim `initializeSession()` `Start()` .

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=86-98&highlight=12)]

Son olarak, aşağıdaki kodu `CreateAndSaveSphere()` yöntemine ekleyin. Gerçek dünyaya yerleştirdiğimiz sphere öğesine yerel bir Azure uzamsal Bağlayıcısı ekler.

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=320-344,396&highlight=14-25)]

Devam etmeden önce, hesap tanımlayıcı, anahtar ve etki alanını almak için bir Azure uzamsal bağlayıcı hesabı oluşturmanız gerekir. Bu değerlere sahip değilseniz, bu değerleri almak için sonraki bölümü izleyin.

[!INCLUDE [Create Spatial Anchors resource](../../../includes/spatial-anchors-get-started-create-resource.md)]

## <a name="upload-your-local-anchor-into-the-cloud"></a>Yerel çıpası buluta yükleyin

Azure uzamsal bağlayıcılarınızın hesap tanımlayıcısı, anahtarınız ve etki alanınız olduktan sonra öğesine gidin ve içine, içine `Account Id` `SpatialAnchorsAccountId` ve içine yapıştırın `Account Key` `SpatialAnchorsAccountKey` `Account Domain` `SpatialAnchorsAccountDomain` .

Son olarak her şeyi bir araya alalım. `SpawnNewAnchoredObject()`Yönteminde aşağıdaki kodu ekleyin. `CreateAnchorAsync()`Sphere oluşturulduktan hemen sonra yöntemi çağıracaktır. Yöntem döndüğünde, aşağıdaki kod Sphere dosyanızı son kez güncelleştirir ve rengini mavi olarak değiştirir.

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=320-397&highlight=26-77)]

Uygulamanızı **Visual Studio** 'dan bir kez daha çalıştırın. Başlarınızın etrafında ilerleyin ve sonra kürenin yerleştirileceği yere dokunarak uçak yapın. Yeterli kare olduktan sonra Sphere, sarı bir şekilde döner ve bulut karşıya yüklemesi başlar. Karşıya yükleme tamamlandıktan sonra Sphere 'niz mavi olarak açılır. İsteğe bağlı olarak, uygulamanızın gönderdiği günlük iletilerini izlemek için **Visual Studio** içinde çıkış penceresini de kullanabilirsiniz. `RecommendedForCreateProgress`' Yi izleyebilir ve karşıya yükleme tamamlandıktan sonra buluttan döndürülen bağlantı tanımlayıcısını görebilirsiniz.

> [!NOTE]
> "DllNotFoundException: ' AzureSpatialAnchors ' DLL dosyası yüklenemiyor: belirtilen modül bulunamadı.", çözümünüzü yeniden **temizlemeniz** ve **derlemeniz** gerekir.

## <a name="locate-your-cloud-spatial-anchor"></a>Bulut uzamsal çıpası bulun

Bir bağlantı, buluta yüklendi, yeniden bulmaya başlamaya hazırız. Aşağıdaki kodu yönteminizin içine ekleyelim `HandleTap()` . Bu kod şu şekilde olur:

* Çağırın `ResetSession()` , bunu durdurur `CloudSpatialAnchorSession` ve var olan mavi kümizi ekrandan kaldırır.
* `CloudSpatialAnchorSession`Yeniden başlatın. Bu, bulduğumuz bağlayıcının, oluşturduğumuz yerel bağlantı yerine buluttan geldiğinden emin olduğumuz için biz.
* Azure uzamsal bağlayıcılarında karşıya yüklemediğimiz bağlayıcıyı aramak için bir **izleyici** oluşturun.

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=273-311&highlight=13-31,35-36)]

Şimdi ve yöntemlerinizi ekleyelim `ResetSession()` `CleanupObjects()` . Bunları aşağıya yerleştirebilirsiniz `QueueOnUpdate()`

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=124-177)]

Şimdi, sorgulamakta olduğumuz bağlayıcı bulunduğunda çağrılacak kodu yedeklemeniz gerekir. İçinde `InitializeSession()` , aşağıdaki geri çağırmaları ekleyin:

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=206-212&highlight=4-5)]


Şimdi, CloudSpatialAnchor bulunduğu bir yeşil Sphere & oluşturacak kodu ekleyebilmenizi sağlar. Ayrıca, ekranı dokunarak bir kez daha yeniden etkinleştirerek tüm senaryoyu daha sonra tekrarlayabilirsiniz: başka bir yerel bağlayıcı oluşturun, karşıya yükleyin ve tekrar bulun.

[!code-csharp[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md?range=234-271)]

İşte bu kadar! Tüm senaryoyu uçtan uca denemek için uygulamanızı **Visual Studio** 'dan en son bir kez çalıştırın. Cihazınızın etrafında ilerleyin ve beyaz kürenin yerinizi koyun. Daha sonra, Sphere sarıya dönüşene kadar baş ortamınızı ortam verilerini yakalamaya devam ettirmeyi sürdürün. Yerel çıpası karşıya yüklenecek ve Sphere 'niz mavi kullanacaktır. Son olarak, yerel bağlayıcıyı kaldırmak ve buluta karşılık gelen bir sorgu başlatmak için ekranınıza daha fazla dokunun. Bulut uzamsal bağlantı noktası bulunana kadar cihazınızı etrafında taşımaya devam edin. Yeşil Sphere 'ın doğru konumda görünmesi gerekir ve tüm senaryoyu yeniden yineleyebilirsiniz.

[!INCLUDE [AzureSpatialAnchorsScript](../../../includes/spatial-anchors-new-unity-hololens-app-finished.md)]