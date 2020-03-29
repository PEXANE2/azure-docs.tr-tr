---
title: Azure Hizmet Kumaş Örgü Uygulama Sırlarını Depolama ve Kullanma
description: Hizmet Fabric Mesh, Azure kaynakları olarak Sırları destekler. Service Fabric Mesh uygulamalarınızla sırları şu şekilde saklayabilirsiniz ve yönetebilirsiniz.
author: erikadoyle
ms.author: edoyle
ms.date: 10/25/2018
ms.topic: conceptual
ms.openlocfilehash: da4eaa34840f14714616b85e24fd62cf65602b84
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76277630"
---
# <a name="service-fabric-mesh-application-secrets"></a>Servis Kumaş Örgü uygulama sırları
Hizmet Fabric Mesh, Azure kaynakları olarak Sırları destekler. Service Fabric Mesh sırrı, depolama bağlantı dizeleri, parolalar veya güvenli bir şekilde depolanması ve iletilmesi gereken diğer değerler gibi hassas metin bilgileri olabilir.

![Mesh Sırları Genel Bakış][sf-mesh-secrets-overview]

## <a name="mesh-secrets-resources"></a>Kafes sırları kaynakları
Mesh uygulaması Secret aşağıdakilerden oluşur:
* Metin sırlarını depolayan bir kap olan **Sırlar** kaynağı. **Sırlar** kaynağında bulunan sırlar güvenli bir şekilde saklanır ve iletilir.
* **Sırlar** kaynak kapsayıcısında depolanan bir veya daha fazla **Sırlar/Değerler** kaynakları. Her **Sırlar/Değerler** kaynağı bir sürüm numarasıyla ayırt edilir.

## <a name="next-steps"></a>Sonraki adımlar 
Service Fabric Mesh Secrets hakkında daha fazla bilgi edinmek için bkz:
- [Servis Kumaş Örgü Uygulama Sırlarını Yönet](service-fabric-mesh-howto-manage-secrets.md)
- [Hizmet kumaş kaynak modeline giriş](service-fabric-mesh-service-fabric-resources.md)

<!-- pics -->
[sf-mesh-secrets-overview]: ./media/service-fabric-mesh-secrets-overview/MeshAppSecretsOverview.png
