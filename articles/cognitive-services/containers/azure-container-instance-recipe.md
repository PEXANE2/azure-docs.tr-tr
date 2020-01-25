---
title: Azure Container Instance tarifi
titleSuffix: Azure Cognitive Services
description: Bilişsel hizmetler kapsayıcılarını Azure Container Instance üzerinde dağıtmayı öğrenin
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.topic: conceptual
ms.date: 01/23/2020
ms.author: dapine
ms.openlocfilehash: 78f35042678aa7c30cebf73796df3e5d564b4502
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76717008"
---
# <a name="deploy-and-run-container-on-azure-container-instance"></a>Azure Container Örneğinde kapsayıcı dağıtma ve çalıştırma

Aşağıdaki adımlarla, Azure [Container Instances](https://docs.microsoft.com/azure/container-instances/)Ile Bulutta Azure bilişsel hizmetler uygulamalarını kolayca ölçeklendirin. Kapsayıcılama, altyapıyı yönetmek yerine uygulamalarınızı oluşturmaya odaklanmanıza yardımcı olur. Kapsayıcıları kullanma hakkında daha fazla bilgi için bkz. [Özellikler ve avantajlar](../cognitive-services-container-support.md#features-and-benefits).

## <a name="prerequisites"></a>Ön koşullar

Tarif, tüm bilişsel hizmetler kapsayıcısıyla birlikte çalışarak. Bilişsel hizmet kaynağının, tarif kullanılmadan önce Azure portal oluşturulması gerekir. Kapsayıcıları destekleyen her bilişsel hizmet hizmeti, bir kapsayıcı için hizmeti yüklemek ve yapılandırmak üzere özellikle bir "yükleme" belgesine sahiptir. Bazı hizmetler, kapsayıcı için girdi olarak bir dosya veya dosya kümesi gerektirir, bu çözümü kullanmadan önce kapsayıcıyı anlamanız ve başarıyla kullanmış olmanız önemlidir.

* Azure portal içinde oluşturulan bilişsel hizmet kaynağı.
* Bilişsel hizmet **uç noktası URL 'si** -uç nokta url 'sinin Azure Portal içinden nerede olduğunu ve URL 'nin doğru bir örneğini nasıl göründüğünü bulmak için, belirli hizmetinizin "nasıl yüklenir" konusunu gözden geçirin. Tam biçim hizmetten hizmete değiştirilebilir.
* Bilişsel hizmet **anahtarı** -anahtarlar, Azure kaynağı için **anahtarlar** sayfasıdır. Yalnızca iki anahtarlarından biri gerekir. Anahtar, 32 alfasayısal karakterlerden oluşan bir dizedir.
* Yerel ana bilgisayarınızda (Bilgisayarınız) tek bir bilişsel hizmetler kapsayıcısı. Şunları yapadığınızdan emin olun:
  * `docker pull` komutuyla görüntüyü çekin.
  * Tüm gerekli yapılandırma ayarları ile bir `docker run` komutuyla yerel kapsayıcıyı başarıyla çalıştırın.
  * HTTP 2xx ve JSON yanıtının geri yanıtını alarak kapsayıcının uç noktasını çağırın.

Açılı parantezdeki tüm değişkenlerin (`<>`) kendi değerlerinizle değiştirilmesini gerektirir. Bu değiştirme açılı ayraçları içerir.

[!INCLUDE [Create a Text Analytics Containers on Azure Container Instances](includes/create-container-instances-resource.md)]

## <a name="use-the-container-instance"></a>Kapsayıcı örneğini kullanma

1. **Genel bakış** ' ı SEÇIN ve IP adresini kopyalayın. `55.55.55.55`gibi sayısal bir IP adresi olacaktır.
1. Yeni bir tarayıcı sekmesi açın ve IP adresini kullanın, örneğin `http://<IP-address>:5000 (http://55.55.55.55:5000`). Kapsayıcının ana sayfasını görürsünüz ve kapsayıcının çalıştığını bilmenizi sağlar.

1. Kapsayıcının Swagger sayfasını görüntülemek için **hizmet API 'Si açıklaması** ' nı seçin.

1. **Post** API 'lerinden birini seçin ve **deneyin**' i seçin.  Parametreler, giriş dahil görüntülenir. Parametreleri girin.

1. İsteği kapsayıcı örneğinize göndermek için **Yürüt** ' ü seçin.

    Azure Container Instance 'da bilişsel hizmetler kapsayıcılarını başarıyla oluşturdunuz ve kullandınız.
