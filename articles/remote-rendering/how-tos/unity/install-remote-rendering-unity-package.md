---
title: Unity için Remote Rendering paketini yükleme
description: Unity için uzaktan Işleme istemci dll 'Lerinin nasıl yükleneceğini açıklar.
author: florianborn71
ms.author: flborn
ms.date: 02/26/2020
ms.topic: how-to
ms.openlocfilehash: 3375b28d94956d5c368db4bf3026bdf52ee2d58e
ms.sourcegitcommit: 053e5e7103ab666454faf26ed51b0dfcd7661996
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84021152"
---
# <a name="install-the-remote-rendering-package-for-unity"></a>Unity için Remote Rendering paketini yükleme

Azure uzaktan Işleme, tümleştirmeyi Unity 'ye kapsüllemek için bir Unity paketi kullanır.

## <a name="manage-the-remote-rendering-packages-in-unity"></a>Unity 'de uzaktan işleme paketlerini yönetme

Unity paketleri, Unity 'nin [Paket Yöneticisi](https://docs.unity3d.com/Packages/com.unity.package-manager-ui@1.8/manual/index.html)aracılığıyla yönetilebilen kapsayıcılardır.
Bu paket tüm C# API 'SININ yanı sıra Unity ile Azure uzaktan Işlemeyi kullanmak için gereken tüm eklenti ikililerini içerir.
Aşağıdaki Unity 'nin paketlere yönelik adlandırma şeması, pakete **com. Microsoft. Azure. Remote-Rendering**olarak adlandırılır.

Paket, [ARR örnekleri deposunun](https://github.com/Azure/azure-remote-rendering)bir parçası değildir ve Unity 'nin iç paket kayıt defterinde kullanılamaz. Projeyi bir projeye eklemek için, aşağıdakileri eklemek üzere projenin dosyasını el ile düzenlemeniz gerekir `manifest.md` :
```json
{
  "scopedRegistries": [
    {
      "name": "Azure Mixed Reality Services",
      "url": "https://api.bintray.com/npm/microsoft/AzureMixedReality-NPM/",
      "scopes": ["com.microsoft.azure"]
    }
   ],
  "dependencies": {
    "com.microsoft.azure.remote-rendering": "0.1.11",
    ...existing dependencies...
  }
}
```
Bu eklendikten sonra Unity paket yöneticisini kullanarak en son sürüme sahip olduğunuzdan emin olabilirsiniz.
Öğreticide daha kapsamlı yönergeler verilmiştir [: bir Unity projesini sıfırdan ayarlama](../../tutorials/unity/project-setup.md).

## <a name="unity-render-pipelines"></a>Unity işleme işlem hatları

Uzaktan Işleme hem hem de ile birlikte çalışarak **:::no-loc text="Universal render pipeline":::** **:::no-loc text="Standard render pipeline":::** . Performans nedenleriyle, evrensel işleme işlem hattı önerilir.

Öğesini kullanmak için **:::no-loc text="Universal render pipeline":::** , paketinin Unity 'ye yüklenmesi gerekir. Bu, Unity 'nin **Paket Yöneticisi** Kullanıcı arabiriminde (paket adı **Universal RP**, sürüm 7.2.1 veya üzeri) ya da `Packages/manifest.json` [Unity proje kurulumu öğreticisinde](../../tutorials/unity/project-setup.md#configure-the-projects-manifest)açıklandığı gibi dosya aracılığıyla yapılabilir.

## <a name="next-steps"></a>Sonraki adımlar

* [Unity oyun nesneleri ve bileşenleri](objects-components.md)
* [Öğretici: sıfırdan Unity projesi ayarlama](../../tutorials/unity/project-setup.md)
