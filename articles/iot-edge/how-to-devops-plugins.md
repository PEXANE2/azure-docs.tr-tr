---
title: Jenkins eklentisi ile CI/CD 'yi etkinleştirme-Azure IoT Edge | Microsoft Docs
description: Jenkins için Azure IoT Edge uzantısı, IoT Edge geliştirme ve dağıtım görevlerini mevcut DevOps çözümünüz ile tümleştirmenize olanak sağlar.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 01/22/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 75ed5f5eda48f10776b854ce4de4d2f855a53634
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "76510252"
---
# <a name="integrate-azure-iot-edge-with-jenkins-pipelines"></a>Jenkins işlem hatlarına Azure IoT Edge tümleştirin

Azure IoT Edge, Azure DevOps ve Azure DevOps Projeleri için yerleşik desteğe sahiptir, ancak Ayrıca DevOps işlevselliğini Jenkins 'e genişletmek için bir eklenti sağlar. [Jenkins](https://jenkins.io/) , IoT Edge dahil olmak üzere birçok tür geliştirme ve dağıtım projesini desteklemek için eklentileri kullanan açık kaynaklı bir Otomasyon sunucusudur.

Jenkins için Azure IoT Edge eklentisi sürekli tümleştirme ve sürekli dağıtıma odaklanır. Modüller oluşturup kapsayıcı görüntülerini kapsayıcı Kayıt defterinize ileten bir yapı ve gönderim işlem hattı oluşturabilirsiniz. Daha sonra, IoT Edge cihazlarınıza modüller dağıtan bir yayın işlem hattı oluşturun.

Jenkins için IoT Edge eklentisini kullanmaya başlamadan önce, Azure 'da bir IoT Hub ve kapsayıcı görüntülerinizi tutacak bir kapsayıcı kayıt defteri gerekir. Bir Azure hizmet sorumlusu kullanarak, eklentinin IoT Edge cihazlarınız için dağıtımlar oluşturabilmesi için IoT Hub 'ınıza Jenkins 'e katkıda bulunan izinleri verin.

Başlamak için hazırsanız, [Jenkins için Azure IoT Edge eklentisi](https://plugins.jenkins.io/azure-iot-edge)için yükleme ve kullanım ayrıntılarını bulun.
