---
title: Azure Konteyner Örneği tarifi
titleSuffix: Azure Cognitive Services
description: Azure Kapsayıcı Örneğin'de Bilişsel Hizmetler Kapsayıcılarını nasıl dağıtabileceğinizi öğrenin
services: cognitive-services
author: aahill
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.topic: conceptual
ms.date: 04/01/2020
ms.author: aahi
ms.openlocfilehash: f247465c7e2c0a212df2821ebc7165d3ee5b15f3
ms.sourcegitcommit: 2d7910337e66bbf4bd8ad47390c625f13551510b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80876666"
---
# <a name="deploy-and-run-container-on-azure-container-instance"></a>Azure Container Örneğinde kapsayıcı dağıtma ve çalıştırma

Aşağıdaki adımlarla Azure Uygulama Larını Azure [Kapsayıcı Örnekleri](https://docs.microsoft.com/azure/container-instances/)ile bulutta kolayca ölçeklendirin. Konteynerleştirme, altyapıyı yönetmek yerine uygulamalarınızı oluşturmaya odaklanmanıza yardımcı olur. Kapsayıcıları kullanma hakkında daha fazla bilgi [için, bkz.](../cognitive-services-container-support.md#features-and-benefits)

## <a name="prerequisites"></a>Ön koşullar

Tarifi herhangi bir Bilişsel Hizmetler konteyner ile çalışır. Bilişsel Hizmet kaynağı, tarifi kullanmadan önce Azure portalında oluşturulmalıdır. Kapsayıcıları destekleyen her Bilişsel Hizmetin, hizmeti bir kapsayıcı için yüklemek ve yapılandırmak için özel olarak bir "Nasıl yüklenir" belgesi vardır. Bazı hizmetler kapsayıcı için giriş olarak bir dosya veya dosya kümesi gerektirir, bu çözümü kullanmadan önce kapsayıcıyı anlamanız ve başarıyla kullanmış olduğunuz önemlidir.

* Azure portalında oluşturulan bir Bilişsel Hizmet kaynağı.
* Bilişsel Hizmet **uç noktası URL'si** - Son nokta URL'sinin Azure portalının içinden olduğunu ve URL'nin doğru bir örneğinin nasıl göründüğünü bulmak için, kapsayıcı için özel hizmetinizin "Nasıl yüklenir" adlı dosyasını inceleyin. Tam biçim hizmetten hizmete değişebilir.
* Bilişsel Hizmet **anahtarı** - anahtarlar Azure kaynağının **Tuşları** sayfasındadır. İki anahtardan sadece birine ihtiyacın var. Anahtar 32 alfa-sayısal karakter bir dize.
* Yerel ana bilgisayar (bilgisayarınız) üzerinde tek bir Bilişsel Hizmetler Konteyner. Şunları yaptığınızdan emin olun:
  * Bir `docker pull` komutla görüntüyü aşağı çekin.
  * Bir `docker run` komutla gerekli tüm yapılandırma ayarlarıyla yerel kapsayıcıyı başarıyla çalıştırın.
  * HTTP 2xx ve JSON yanıtı geri bir yanıt alarak, konteyner bitiş noktasını arayın.

Açı braketleri tüm `<>`değişkenler, kendi değerleri ile değiştirilmesi gerekir. Bu değiştirme açı braketlerini içerir.

[!INCLUDE [Create a Text Analytics Containers on Azure Container Instances](includes/create-container-instances-resource.md)]

## <a name="use-the-container-instance"></a>Kapsayıcı Örneğini Kullanma

1. Genel **Bakış'ı** seçin ve IP adresini kopyalayın. Bu gibi `55.55.55.55`sayısal bir IP adresi olacak.
1. Yeni bir tarayıcı sekmesi açın ve örneğin `http://<IP-address>:5000 (http://55.55.55.55:5000`IP adresini kullanın). Kapsayıcının ana sayfasını göreceksiniz ve kapsayıcının çalıştığını bildirin.

1. Kapsayıcının swagger sayfasını görüntülemek için **Servis API Açıklaması'nı** seçin.

1. **POST** API'lerden herhangi birini seçin ve **Deneyin'i**seçin.  Parametreler giriş de dahil olmak üzere görüntülenir. Parametreleri doldurun.

1. İstek'i Kapsayıcı Örneği'ne göndermek için Yürüt''ün'u seçin. **Execute**

    Azure Kapsayıcı Örneği'nde Bilişsel Hizmetler kapsayıcılarını başarıyla oluşturdunuz ve kullandınız.
