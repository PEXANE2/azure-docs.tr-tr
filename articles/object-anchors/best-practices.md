---
title: En İyi Uygulamalar
description: İyileştirilmiş sonuçları elde etmek için önerilen en iyi uygulamalar
author: ariye
ms.author: crtreasu
ms.date: 02/17/2021
ms.topic: best-practice
ms.service: azure-object-anchors
ms.openlocfilehash: da3be6e3d97e50b27ded29ba017164fdbd9a0a5b
ms.sourcegitcommit: 15d27661c1c03bf84d3974a675c7bd11a0e086e6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2021
ms.locfileid: "102503054"
---
# <a name="best-practices"></a>En iyi uygulamalar

En iyi sonuçları elde etmek için bu adımlardan bazılarını denemeyi öneririz.

## <a name="ingestion"></a>Alma

- Fiziksel nesnelerinizin boyutlarını denetleyin. Nesne bağlayıcıları, en küçük boyutu önerilen 1m-10m aralığında olan nesneler için en iyi şekilde kullanılır.
- Aşağıdaki ayrıntılar için, [**MeshLab**](https://www.meshlab.net/) gibi yazılımda 3B modelinizi inceleyin.
  - 3B modelin üçgen bir kafesi olduğundan ve dış yüzeydeki üçgenin dışarıya dönük olduğundan emin olun. Diğer bir deyişle, bu köşelerin, normalleştirme yönündeki doğru kuralı izlemesini sağlamak için köşeler yönlendirilmelidir.
  - 3D modelin fiziksel nesnelere göre doğru ölçek birimleriyle belirtildiğinden emin olun. Birimler şunlardan biri olmalıdır: ***santimetre, yaprak döken, fit, inç, kilometre, metre, milimetre, metreküp***.
  - Nesnenin gerçek dünya dikey yönüne karşılık gelen nominal yönü onaylayın. Nesnenin aşağı dikey/yerçekimi-Y ise, ***(0,-1, 0)** _ veya _*_ (0, 0,-1) _ * * for-Z ve başka herhangi bir yön için aynı şekilde kullanın.
  - 3D modelin desteklenen biçimlerden birinde kodlanmış olduğundan emin olun: `.glb` , `.gltf` ,, `.ply` `.fbx` , `.obj` .
- Model dönüştürme hizmetimiz, büyük, yüksek bir LOD (ayrıntı düzeyi) modelini işlemek uzun zaman alabilir. Göre etkinliğine için, iç yüzeyleri kaldırmak üzere 3B modelinizi önceden işleyebilirsiniz.

## <a name="detection"></a>Algılama

> [!VIDEO https://channel9.msdn.com/Shows/Docs-Mixed-Reality/Azure-Object-Anchors-Detection-and-Alignment-Best-Practices/player]

- Belirtilen çalışma zamanı SDK 'Sı, fiziksel nesneleri aramak ve algılamak için Kullanıcı tarafından sağlanmış bir arama bölgesi gerektirir. Arama bölgesi bir sınırlayıcı kutusu, küre, görünüm Frustum veya bunların herhangi bir birleşimini olabilir. Yanlış algılamayı önlemek için, bir arama bölgesinin nesneyi kapsayacak kadar büyük bir şekilde ayarlanması tercih edilir. Sunulan örnek uygulamaları kullanırken, en yakın yüzeyden 2 Ölçüm hakkında nesnenin bir tarafına tek tek ekleyebilirsiniz ve uygulamayı başlatabilirsiniz.
- HoloLens 2 cihazında nesne tutturucularını uygulamayı başlatmadan önce, çalışma alanınızın çevresinin, ***Ayarlar->sistem >hologram***

  Bu adım, bir araba gibi yeni bir nesnenin daha önceden başka bir ile aynı alanda mevcut olması veya nesnenin hedef alandan taşınmış olması, eski ve ilgisiz hologramlar kalıcı olmamasını ve şu anda görünümdeki nesne için kafa karıştırıcı görselleştirmeyi oluşturmamasını sağlar.
- Hologramlar kaldırıldıktan ve uygulamayı başlatmadan önce, cihazı yaklaşık 1-2 metreden arayarak ve nesneyi bir veya iki kez yavaş ilerleyerek bir otomobil gibi nesneyi tarayın.

  Bu adım, alanınızda daha önceki nesneler ve taramalarda oluşturulan tüm kalan yüzey tahminlerinin, birlikte çalıştığınız geçerli hedef nesnenin yüzeyleriyle yenilenmesini sağlar. Aksi takdirde uygulama, 3B modelinizin ve ilişkili hologramlar için yanlış hizalamasına sahip çift hayalet yüzeyler görebilir. Nesnenin ön taranması, AOA algılama gecikmesini, örneğin 30 saniye ila 5 saniyeye kadar de önemli ölçüde azaltır.
- Koyu ve yüksek oranda yansıtmalı nesneler için, nesneyi daha yakın bir aralıkta taramak ve ayrıca cihazın birden çok açıdan ve birden çok uzaklıkta yüzey görmesini sağlamak için baş ve aşağı ve sol ve sağ hareket ettirilmesi gerekebilir.
- Çevrilmekte olan yön veya eğimli bir model gibi yanlış bir nesne algılamasında gördüğünüz zaman, uzamsal eşlemeyi görselleştirmelisiniz. Genellikle hatalı sonuçlar zayıf veya tamamlanmamış yüzey yeniden oluşturma nedeniyle oluşur. Hologramlar ' ı kaldırabilir, nesneyi tarayabilir ve uygulama üzerinde nesne algılamayı yeniden çalıştırabilirsiniz.
- Sağlanan çalışma zamanı SDK 'Sı, kullanıcıların algılama işlemini, örnek uygulamalarımız bölümünde gösterildiği gibi ince ayar yapmasına izin vermek için birkaç parametre sağlar. Varsayılan parametreler çoğu nesne için iyi çalışır. Bunları belirli nesneler için ayarlamanız gerektiğini fark ederseniz, bazı öneriler aşağıda verilmiştir:
  - Fiziksel nesne büyük, koyu veya parlak ise, daha düşük bir yüzey kapsamı eşiği kullanın.
  - Bir otomobil gibi büyük bir nesne için küçük ölçekli değişikliğe (örneğin, 0,1) izin verin.
  - Nesne bir eğim üzerinde olduğunda nesnenin yerel dikey yönü ve yerçekimi arasındaki derecelerde bir sapmanın kullanılmasına izin verin.
