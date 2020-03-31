---
title: Jenkins eklentisi ile CI/CD'yi etkinleştirme - Azure IoT Edge | Microsoft Dokümanlar
description: Jenkins için Azure IoT Edge uzantısı, IoT Edge geliştirme ve dağıtım görevlerini mevcut DevOps çözümünüze entegre etmenizi sağlar.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 01/22/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 75ed5f5eda48f10776b854ce4de4d2f855a53634
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76510252"
---
# <a name="integrate-azure-iot-edge-with-jenkins-pipelines"></a>Azure IoT Edge'i Jenkins boru hatlarıyla tümleştirme

Azure IoT Edge, Azure DevOps ve Azure DevOps Projeleri için yerleşik destek sağlar, ancak DevOps işlevselliğini Jenkins'e genişletmek için bir eklenti de sağlar. [Jenkins,](https://jenkins.io/) IoT Edge de dahil olmak üzere birçok geliştirme ve dağıtım projesi türünü desteklemek için eklentileri kullanan açık kaynak kodlu bir otomasyon sunucusudur.

Jenkins için Azure IoT Edge eklentisi sürekli tümleştirme ve sürekli dağıtıma odaklanır. Modüller oluşturan ve konteyner görüntülerini konteyner kayıt defterinize iten bir yapı ve itme ardışık hattı oluşturabilirsiniz. Ardından, modülleri IoT Edge aygıtlarınıza dağıtan bir sürüm ardışık hattı oluşturun.

Jenkins için IoT Edge eklentisini kullanmaya başlamadan önce, Azure'da bir IoT hub'ına ve konteyner görüntülerinizi tutmak için bir konteyner kayıt defterine ihtiyacınız vardır. Eklentinin IoT Edge aygıtlarınız için dağıtımlar oluşturabilmesi için Jenkins'e IoT hub'ınıza katkıda bulunanlara izin vermek için bir Azure Hizmet Sorumlusu kullanın.

Başlamaya hazırsanız, [Jenkins için Azure IoT Edge eklentisinin](https://plugins.jenkins.io/azure-iot-edge)yükleme ayrıntılarını bulun ve ayrıntıları kullanın.
