---
title: Azure Service Fabric ağı için Visual Studio 'Yu iyileştirin
description: Bu makalede, ilk hata ayıklama çalıştırmasının (F5) çok daha hızlı olması için Service Fabric ağı projelerinin Visual Studio performansının nasıl iyileştirileceği gösterilmektedir.
author: georgewallace
ms.author: gwallace
ms.date: 11/29/2018
ms.topic: conceptual
ms.openlocfilehash: aa7a959128d3bcdfcce67d3abeac245975339a9f
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91840311"
---
# <a name="optimize-visual-studio-performance-for-service-fabric-mesh-projects"></a>Service Fabric kafes projeleri için Visual Studio performansını iyileştirin

Bu makalede, ilk hata ayıklama çalıştırmasının (F5) çok daha hızlı olması için Service Fabric ağı projelerinin Visual Studio performansının nasıl iyileştirileceği gösterilmektedir.  

## <a name="change-visual-studio-settings"></a>Visual Studio ayarlarını değiştir
 
Visual Studio 'da, **Araçlar**  >  **Seçenekler**   >  **Service Fabric ağ araçları**  >  **genel**' in altında, aşağıdaki ayarları yapabilirsiniz:

- Projede **çekme gerekli Docker görüntüleri** , proje yüklenirken görüntü indirme işlemini başlatarak ilk hata ayıklamanın (F5) daha hızlı çalışmasını sağlar.  
- **Projeyi Project Open üzerinde dağıt** , proje açıldıktan sonra dağıtım işlemini başlatarak ilk hata ayıklamanın çalışmasını (F5) daha hızlı hale getirir.  
- Proje kapatıldığında, uygulamayı kaldırarak uygulama için ayrılmış geri kazanır kaynaklarını (CPU, RAM) **kaldırın** .  

Service Fabric Araçları çıktı penceresinde, Visual Studio 'nun ' görüntü çekme ', ' hazırlama ' veya ' uygulamayı kaldırma ' olduğunu belirten iletiler, yukarıdaki ayarlara başvurmakta olur. Bu ayarları devre dışı bırakabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

[Bir kafes uygulamasının hatalarını ayıkla öğreticisini](service-fabric-mesh-tutorial-debug-service-fabric-mesh-app.md) okuyun