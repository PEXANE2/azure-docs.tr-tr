---
title: Azure Konteyner Örneklerinde Anomali Dedektörü Konteyneri Çalıştır
titleSuffix: Azure Cognitive Services
description: Anomaly Detector kapsayıcısını bir Azure Kapsayıcı Örneği'ne dağıtın ve bir web tarayıcısında test edin.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: anomaly-detector
ms.topic: conceptual
ms.date: 01/23/2020
ms.author: dapine
ms.openlocfilehash: 2fba0a0d64502a30b6dfbc9f4f109bca65cca8b9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "76716354"
---
# <a name="deploy-an-anomaly-detector-container-to-azure-container-instances"></a>Azure Kapsayıcı Örneklerine Bir Anomali Dedektörü kapsayıcısı dağıtma

Bilişsel Hizmetler [Anomali Dedektörü](../anomaly-detector-container-howto.md) konteynerini Azure [Konteyner Örneklerine](https://docs.microsoft.com/azure/container-instances/)nasıl dağıtabileceğinizi öğrenin. Bu yordam, bir Anomali Dedektörü kaynağının oluşturulmasını göstermektedir. Sonra ilişkili konteyner görüntü çekme tartışmak. Son olarak, bir tarayıcıdan iki orkestrasyon egzersiz yeteneğini vurgulamak. Kapsayıcıların kullanılması, geliştiricilerin dikkatini altyapı yönetiminden uygulama geliştirmeye odaklanmak yerine uzaklaştırabilir.

[!INCLUDE [Prerequisites](../../containers/includes/container-preview-prerequisites.md)]

## <a name="request-access-to-the-private-container-registry"></a>Özel konteyner kayıt defterine erişim isteği

Konteynere erişim talebinde bulunmak için önce [Anomali Dedektör konteyneri talep formunu](https://aka.ms/adcontainer) doldurmanız ve göndermeniz gerekmektedir.

[!INCLUDE [Request access](../../../../includes/cognitive-services-containers-request-access-only.md)]

[!INCLUDE [Create a Cognitive Services Anomaly Detector resource](../includes/create-anomaly-detector-resource.md)]

[!INCLUDE [Create an Anomaly Detector container on Azure Container Instances](../../containers/includes/create-container-instances-resource-from-azure-cli.md)]

[!INCLUDE [API documentation](../../../../includes/cognitive-services-containers-api-documentation.md)]

## <a name="next-steps"></a>Sonraki adımlar

* Kapsayıcı görüntüsünü çekmek [için kapsayıcıları yükleyin](../anomaly-detector-container-configuration.md) ve çalıştırın ve kapsayıcıyı çalıştırın
* Yapılandırma ayarları için [kapsayıcıları yapılandırmayı](../anomaly-detector-container-configuration.md) gözden geçirin
* [Anomaly Detector API hizmeti hakkında daha fazla bilgi edinin](https://go.microsoft.com/fwlink/?linkid=2080698&clcid=0x409)
