---
title: Unity için Azure uzamsal bağlayıcıları 'nı yükler
description: Azure uzamsal bağlayıcıları kullanmak için Unity projesi yapılandırma
author: msftradford
manager: MehranAzimi-msft
services: azure-spatial-anchors
ms.author: parkerra
ms.date: 11/20/2020
ms.topic: how-to
ms.service: azure-spatial-anchors
ms.openlocfilehash: 2330310b0bf4e165af71208477db128650a787cf
ms.sourcegitcommit: b8eba4e733ace4eb6d33cc2c59456f550218b234
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/23/2020
ms.locfileid: "95496961"
---
# <a name="configuring-azure-spatial-anchors-in-a-unity-project"></a>Unity projesinde Azure uzamsal bağlayıcılarını yapılandırma

Bu kılavuzda, Unity projenizde Azure uzamsal bağlayıcı SDK 'sını kullanmaya nasıl başlacağınız gösterilmektedir.

## <a name="requirements"></a>Gereksinimler

Azure uzamsal bağlayıcıları Şu anda Unity 2019,4 (LTS) ' i şu yapılandırmalarda desteklemektedir.

* 2.4.0 + Azure uzamsal bağlayıcılarını içeren Unity 2019,4, AR Foundation 3,1 ile desteklenir.

## <a name="configuring-a-project"></a>Proje yapılandırma

### <a name="add-the-unity-package-manager-packages-to-your-project"></a>[Projenize Unity paket yöneticisi paketleri ekleyin](#tab/UPMPackage)

Unity için Azure uzamsal bağlantıları şu anda Unity paket yöneticisi (UPD) paketleri kullanılarak dağıtılır. Bu paketler, [NPM kayıt defterinizde](https://bintray.com/microsoft/AzureMixedReality-NPM)bulunabilir. Unity projesinde kapsamlı paket kayıt defterleri ile çalışma hakkında daha fazla bilgi edinmek için [buradan](https://docs.unity3d.com/Manual/upm-scoped.html)resmi Unity belgelerine bakın.

#### <a name="add-the-registry-to-your-unity-project"></a>Kayıt defterini Unity projenize ekleme

1. Dosya Gezgini 'nde Unity projenizin `Packages` klasörüne gidin. Proje bildirim dosyasını `manifest.json` bir metin düzenleyicisinde açın.
2. Dosyanın en üstünde, bölümüyle aynı düzeyde, `dependencies` Azure uzamsal bağlayıcı kayıt defterini projenize dahil etmek için aşağıdaki girişi ekleyin. `scopedRegistries`Giriş, Unity 'ye Azure uzamsal Tutturucuların SDK paketlerini nerede bakacağını söyler.

    [!code-json[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-unity-scoped-registry-setup.md?range=9-19&highlight=2-10)]

#### <a name="add-the-sdk-packages-to-your-unity-project"></a>SDK paketlerini Unity projenize ekleyin

| Platform | Paket adı                                    |
|----------|-------------------------------------------------|
| Android  | com. Microsoft. Azure. uzamsal-Tutturucuların-SDK. Android |
| iOS      | com. Microsoft. Azure. Spatial-Anchors-SDK. iOS     |
| HoloLens | com. Microsoft. Azure. uzamsal-Tutturucuların-SDK. Windows |

1. Projenizde desteklemek istediğiniz her platform (Android/iOS/HoloLens) için, `dependencies` Proje bildiriminizde bulunan bölümüne paket adı ve paket sürümü ile bir giriş ekleyin. Aşağıdaki örneğe bakın.

    [!code-json[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-unity-scoped-registry-setup.md?range=9-22&highlight=12-14)]

2. Dosyayı kaydedin ve kapatın `manifest.json` . Unity 'ye geri döndüğünüzde, Unity 'nin proje bildirimi değişikliğini otomatik olarak algılaması ve belirtilen paketleri alması gerekir. `Packages`Doğru paketlerin içeri aktarıldığını doğrulamak Için proje görünüminizdeki klasörü genişletebilirsiniz.

#### <a name="android-only-configure-the-maintemplategradle-file"></a>Yalnızca Android: mainTemplate. Gradle dosyasını yapılandırma

1. **Edit**  >  **Proje ayarları**  >  **oynatıcıyı** Düzenle ' ye gidin.
2. **Oynatıcı ayarları** Için **Inspector panelinde** **Android** simgesini seçin.
3. **Derleme** bölümünün altında özel **ana Gradle şablonu** onay kutusunu işaretleyerek, üzerinde özel bir Gradle şablonu oluşturun `Assets\Plugins\Android\mainTemplate.gradle` .
4. `mainTemplate.gradle`Dosyanızı bir metin düzenleyicisinde açın.
5. `dependencies`Bölümünde aşağıdaki bağımlılıkları yapıştırın:

    ```gradle
    implementation('com.squareup.okhttp3:okhttp:[3.11.0]')
    implementation('com.microsoft.appcenter:appcenter-analytics:[1.10.0]')
    ```

Her şey yapıldığında, `dependencies` bölüm şuna benzer şekilde görünmelidir:

[!code-gradle[AzureSpatialAnchorsScript](../../../includes/spatial-anchors-unity-android-gradle-setup.md?range=9-13&highlight=3-4)]

### <a name="import-the-asset-package"></a>[Varlık paketini içeri aktar](#tab/UnityAssetPackage)

> [!WARNING]
> Azure uzamsal bağlayıcı SDK 'sının Unity varlık paketi dağıtımı 2.5.0 sürümünde kullanımdan kaldırılmıştır ve artık 2.6.0 itibariyle kullanılabilir değildir.

1. `AzureSpatialAnchors.unitypackage` [GitHub sürümlerinden](https://github.com/Azure/azure-spatial-anchors-samples/releases)hedeflemek istediğiniz sürümün dosyasını indirin.
2. Unity varlık paketini projenize aktarmak için [buradaki](https://docs.unity3d.com/Manual/AssetPackagesImport.html) yönergeleri izleyin.

---

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Nasıl yapılır: Unity 'de bağlayıcı oluşturma ve bulma](./create-locate-anchors-unity.md)
