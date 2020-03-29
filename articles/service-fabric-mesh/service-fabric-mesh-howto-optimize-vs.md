---
title: Azure Servis Kumaş Örgüiçin Visual Studio'yı Optimize Edin
description: Bu makalede, ilk hata ayıklama çalıştırmak (F5) çok daha hızlı olacak şekilde Hizmet Kumaş Mesh projeleri için Visual Studio performansını optimize etmek için nasıl gösterir.
author: dkkapur
ms.author: dekapur
ms.date: 11/29/2018
ms.topic: conceptual
ms.openlocfilehash: 40c4e9972a3d83a5dd8247bacac12e9d67a15f66
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75497985"
---
# <a name="optimize-visual-studio-performance-for-service-fabric-mesh-projects"></a>Service Fabric Mesh projeleri için Visual Studio performansını optimize edin

Bu makalede, ilk hata ayıklama çalıştırmak (F5) çok daha hızlı olacak şekilde Hizmet Kumaş Mesh projeleri için Visual Studio performansını optimize etmek için nasıl gösterir.  

## <a name="change-visual-studio-settings"></a>Visual Studio ayarlarını değiştirme
 
Visual Studio'da, **Tools** > **Options**  > **Service Fabric Mesh Tools** > **General**altında aşağıdaki ayarları ayarlayabilirsiniz:

- **Proje açıkta gerekli Docker görüntülerini çekme,** proje yüklenirken görüntü indirme işlemini başlatarak ilk hata ayıklama çalıştırmanızı (F5) daha hızlı hale getirir.  
- **Proje açık uygulamasında uygulama dağıtma,** proje açıldıktan sonra dağıtım işlemini başlatarak ilk hata ayıklama çalıştırmanızı (F5) hızlandırabilir.  
- **Proje kapatıldığında** Mesh uygulamasını kaldırarak uygulamaya ayrılan proje kapanışı kaynaklarını (CPU, RAM) kaldırın.  

Service Fabric Tools çıkış penceresinde Visual Studio'nun 'görüntüleri çektiği', 'ısınmasını' veya 'uygulamayı kaldırdığını' belirten iletileri gördüğünüzde, bu durum yukarıdaki ayarlara başvurur. Bu ayarları kapatabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

[Hata Ayıklama'yı mesh uygulaması öğreticisini](service-fabric-mesh-tutorial-debug-service-fabric-mesh-app.md) okuyun