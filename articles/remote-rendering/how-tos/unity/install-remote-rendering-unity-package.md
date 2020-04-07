---
title: Unity için Uzaktan İşleme paketini yükleme
description: Unity için Uzaktan İşlem istemcisi DL'lerinin nasıl yüklenirolduğunu açıklar
author: florianborn71
ms.author: flborn
ms.date: 02/26/2020
ms.topic: how-to
ms.openlocfilehash: 86ffe47c009f9e0ae121ffc6ab57bff8fb73210f
ms.sourcegitcommit: 642a297b1c279454df792ca21fdaa9513b5c2f8b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80681187"
---
# <a name="install-the-remote-rendering-package-for-unity"></a>Unity için Uzaktan İşleme paketini yükleme

Azure Uzaktan İşleme, tümleştirmeyi Unity'ye dahil etmek için bir Birlik paketi kullanır.

## <a name="manage-the-remote-rendering-packages-in-unity"></a>Uzaktan görüntüleme paketlerini Unity'de yönetme

Birlik paketleri, Unity'nin [Paket Yöneticisi](https://docs.unity3d.com/Packages/com.unity.package-manager-ui@1.8/manual/index.html)aracılığıyla yönetilebilen konteynerlerdir.
Bu paket, Azure Uzaktan İşleme'yi Unity ile kullanmak için gereken tüm eklenti ikililerinin yanı sıra tüm C# API'sını da içerir.
Unity'nin paketler için adlandırma düzenini takiben, paket **com.microsoft.azure.remote rendering**olarak adlandırılır.

Paket, [ARR numune deposunun](https://github.com/Azure/azure-remote-rendering)bir parçası değildir ve Unity'nin dahili paket kayıt defterinden kullanılamaz. Projeye eklemek için, aşağıdakileri eklemek için projenin `manifest.md` dosyasını el ile düzenlemesi niz gerekir:
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
Bu eklendikten sonra, en son sürüme sahip olmak için Birlik Paket Yöneticisi'ni kullanabilirsiniz.
Daha kapsamlı talimatlar [Öğretici verilmiştir: Sıfırdan bir Birlik projesi kurma](../../tutorials/unity/project-setup.md).

## <a name="unity-render-pipelines"></a>Birlik işleme boru hatları

Uzaktan İşleme, hem **Evrensel işleme ardışık ardışık ardışık hem** de Standart işleme **ardışık** Performans nedenleriyle, Evrensel işleme ardışık alanı önerilir.

**Universal render ardışık hattını**kullanmak için paketinin Unity'ye yüklenmesi gerekir. Bu, Unity'nin Paket **Yöneticisi** Kullanıcı Arabirimi'nde (paket adı **Universal RP,** sürüm 7.2.1 veya daha yeni) veya `Packages/manifest.json` [Unity proje kurulum öğreticisinde](../../tutorials/unity/project-setup.md#configure-the-projects-manifest)açıklandığı gibi dosya üzerinden yapılabilir.

## <a name="next-steps"></a>Sonraki adımlar

* [Unity oyun nesneleri ve bileşenleri](objects-components.md)
* [Öğretici: Sıfırdan bir Birlik projesi kurma](../../tutorials/unity/project-setup.md)
