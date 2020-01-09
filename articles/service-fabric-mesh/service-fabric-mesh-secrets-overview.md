---
title: Azure Service Fabric kafes uygulama gizli dizilerini depolama ve kullanma
description: Service Fabric ağ, Azure kaynakları olarak gizli dizileri destekler. Service Fabric kafes uygulamalarınızla gizli dizileri depolamak ve yönetmek için aşağıdaki adımları uygulayın.
author: v-steg
ms.author: jeconnoc
ms.date: 10/25/2018
ms.topic: conceptual
ms.openlocfilehash: d45b9e98c1f325e5dbd656c85655a4ac72e4c3e3
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75459120"
---
# <a name="service-fabric-mesh-application-secrets"></a>Service Fabric kafes uygulama gizli dizileri
Service Fabric ağ, Azure kaynakları olarak gizli dizileri destekler. Service Fabric bir ağ parolası, depolama bağlantı dizeleri, parolalar veya güvenli bir şekilde depolanması ve aktarılması gereken diğer değerler gibi herhangi bir hassas metin bilgisi olabilir.

![Ağ gizlilikleri genel bakış][sf-mesh-secrets-overview]

## <a name="mesh-secrets-resources"></a>Ağ parolaları kaynakları
Bir kafes uygulama gizli dizisi aşağıdakilerden oluşur:
* Metin gizli dizileri depolayan bir kapsayıcı olan **gizlilikler** kaynağı. **Gizli** dizileri kaynağı içinde bulunan gizli dizileri, güvenli bir şekilde depolanır ve iletilir.
* **Gizlilikler** kaynak kapsayıcısında depolanan bir veya daha fazla **gizli dizi/değer** kaynağı. Her **gizli dizi/değer** kaynağı bir sürüm numarasıyla ayırt edilir.

## <a name="next-steps"></a>Sonraki adımlar 
Service Fabric ağ parolaları hakkında daha fazla bilgi edinmek için bkz.:
- [Service Fabric kafes uygulama gizli dizilerini yönetme](service-fabric-mesh-howto-manage-secrets.md)
- [Service Fabric kaynak modeline giriş](service-fabric-mesh-service-fabric-resources.md)

<!-- pics -->
[sf-mesh-secrets-overview]: ./media/service-fabric-mesh-secrets-overview/MeshAppSecretsOverview.png
