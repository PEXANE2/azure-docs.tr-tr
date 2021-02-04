---
title: Unity için Azure uzamsal bağlayıcıları 'nı yükler
description: Azure uzamsal bağlayıcıları kullanmak için Unity projesi yapılandırma
author: msftradford
manager: MehranAzimi-msft
services: azure-spatial-anchors
ms.author: parkerra
ms.date: 2/3/2021
ms.topic: how-to
ms.service: azure-spatial-anchors
ms.openlocfilehash: e058186d8848256bf97d99ee1b8b1ddae7d78383
ms.sourcegitcommit: 5b926f173fe52f92fcd882d86707df8315b28667
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/04/2021
ms.locfileid: "99550632"
---
# <a name="configuring-azure-spatial-anchors-in-a-unity-project"></a>Unity projesinde Azure uzamsal bağlayıcılarını yapılandırma

Bu kılavuzda, Unity projenizde Azure uzamsal bağlayıcı SDK 'sını kullanmaya nasıl başlacağınız gösterilmektedir.

## <a name="requirements"></a>Gereksinimler

Azure uzamsal bağlayıcıları Şu anda Unity 2019,4 (LTS) ' i şu yapılandırmalarda desteklemektedir.

* 2.4.0 + Azure uzamsal bağlayıcılarını içeren Unity 2019,4, AR Foundation 3,1 ile desteklenir.

## <a name="configuring-a-project"></a>Proje yapılandırma

### <a name="download-packages"></a>Paketleri İndir
[!INCLUDE [Download Unity Packages](../../../includes/spatial-anchors-unity-download-packages.md)]

### <a name="import-packages"></a>Paketleri içeri aktarma
[!INCLUDE [Import Unity Packages](../../../includes/spatial-anchors-unity-import-packages.md)]

### <a name="android-only-configure-the-maintemplategradle-file"></a>Yalnızca Android: mainTemplate. Gradle dosyasını yapılandırma

1.   >  **Proje ayarları**  >  **oynatıcıyı** Düzenle ' ye gidin.
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

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Nasıl yapılır: Unity 'de bağlayıcı oluşturma ve bulma](./create-locate-anchors-unity.md)
