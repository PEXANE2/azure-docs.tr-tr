---
title: Azure Service Fabric CLI (sfctl) Betik Dağıtma Örneği
description: Bir uygulamayı Azure Service Fabric CLI’sini kullanarak bir Azure Service Fabric kümesine dağıtma
services: service-fabric
documentationcenter: ''
author: athinanthny
manager: chackdan
editor: ''
tags: azure-service-management
ms.assetid: ''
ms.service: service-fabric
ms.workload: multiple
ms.topic: sample
ms.date: 04/16/2018
ms.author: atsenthi
ms.custom: mvc
ms.openlocfilehash: ff40dc62b4dcd622156a78518bbdcb6b9b430644
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "75526609"
---
# <a name="deploy-an-application-to-a-service-fabric-cluster-using-the-service-fabric-cli"></a>Service Fabric CLI'yi kullanarak bir Hizmeti Kumaş kümesine uygulama dağıtma

Bu örnek betik bir uygulama paketini küme görüntü deposuna kopyalar, kümede uygulama türünü kaydeder ve uygulama türünden bir uygulama örneği oluşturur. Aynı anda tüm varsayılan hizmetler de oluşturulur.

Gerekirse [Service Fabric CLI](../service-fabric-cli.md)'sını da yükleyin.

## <a name="sample-script"></a>Örnek betik

[!code-sh[main](../../../cli_scripts/service-fabric/deploy-application/deploy-application.sh "Deploy an application to a cluster")]

## <a name="clean-up-deployment"></a>Dağıtımı temizleme

İşiniz bittiğinde, [kaldırma](cli-remove-application.md) betiği uygulamayı kaldırmak için kullanılabilir. Kaldırma betiği uygulama örneğini siler, uygulama türünün kaydını siler ve uygulama paketini görüntü deposundan kaldırır.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için bkz. [Service Fabric CLI belgeleri](../service-fabric-cli.md).

Azure Service Fabric’e yönelik ek Service Fabric CLI örnekleri [Service Fabric CLI örnekleri](../samples-cli.md)’nde bulunabilir.
