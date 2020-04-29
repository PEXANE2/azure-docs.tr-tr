---
title: Kapsayıcı desteği
titleSuffix: Azure Cognitive Services
description: Azure Container Instance kaynağı oluşturmayı öğrenin.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 04/01/2020
ms.author: aahi
ms.openlocfilehash: 09957461fd92114d1181a570550ee1a189edd8ea
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80876480"
---
## <a name="create-an-azure-container-instance-resource"></a>Azure Container Instance kaynağı oluşturma

1. Container Instances için [Oluştur](https://ms.portal.azure.com/#create/Microsoft.ContainerInstances) sayfasına gidin.

2. **Temel bilgiler** sekmesinde, aşağıdaki ayrıntıları girin:

    |Ayar|Değer|
    |--|--|
    |Abonelik|Aboneliğinizi seçin.|
    |Kaynak grubu|Kullanılabilir kaynak grubunu seçin veya gibi yeni bir tane oluşturun `cognitive-services`.|
    |Kapsayıcı adı|Gibi bir ad girin `cognitive-container-instance`. Ad, daha küçük bir harf olmalıdır.|
    |Konum|Dağıtım için bir bölge seçin.|
    |Görüntü türü|Kapsayıcı resminiz kimlik bilgileri gerektirmeyen bir kapsayıcı kayıt defterinde depolanıyorsa, öğesini seçin `Public`. Kapsayıcı yansımanıza erişim için kimlik bilgileri gerekiyorsa, öğesini `Private`seçin. Kapsayıcı görüntüsünün `Public` mi yoksa `Private` ("genel önizleme") olup olmadığı hakkında ayrıntılar için [kapsayıcı depoları ve resimlere](../../cognitive-services-container-support.md#container-repositories-and-images) bakın. |
    |Görüntü adı|Bilişsel hizmetler kapsayıcı konumunu girin. Konum, `docker pull` komuta bir bağımsız değişken olarak kullanılan şeydir. Kullanılabilir görüntü adları ve bunlara karşılık gelen depo için [kapsayıcı depoları ve görüntüleri](../../cognitive-services-container-support.md#container-repositories-and-images) inceleyin.<br><br>Görüntü adı, üç bölümden oluşan tam olarak nitelenmiş olmalıdır. İlk olarak, kapsayıcı kayıt defteri, ardından depo, son olarak görüntü adı: `<container-registry>/<repository>/<image-name>`.<br><br>Azure bilişsel Hizmetler `mcr.microsoft.com/azure-cognitive-services/keyphrase` deposu altında Microsoft Container Registry anahtar ifade ayıklama görüntüsünü temsil eden bir örnek aşağıda verilmiştir. Diğer bir örnek olarak `containerpreview.azurecr.io/microsoft/cognitive-services-speech-to-text` , kapsayıcı önizleme kapsayıcısı kayıt defterinin Microsoft deposunda konuşmayı metne dönüştürme görüntüsünü temsil eden bir örnektir. |
    |İşletim sistemi türü|`Linux`|
    |Boyut|Belirli bilişsel hizmet Kapsayıcınızın boyutunu önerilen önerilerle değiştirin:<br>2 CPU çekirdeği<br>4 GB

3. **Ağ** sekmesinde, aşağıdaki ayrıntıları girin:

    |Ayar|Değer|
    |--|--|
    |Bağlantı noktaları|TCP bağlantı noktasını olarak `5000`ayarlayın. 5000 numaralı bağlantı noktasında kapsayıcıyı kullanıma sunar.|

4. **Gelişmiş** sekmesinde, Azure Container Instance kaynağının kapsayıcı faturalama ayarları Için gereken **ortam değişkenlerini** girin:

    | Anahtar | Değer |
    |--|--|
    |`apikey`|Kaynağın **anahtarlar** sayfasından kopyalanmış. Boşluk veya tire içermeyen 32 alfasayısal karakter dizesidir `xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`.|
    |`billing`|Kaynağın **genel bakış** sayfasından kopyalanmış.|
    |`eula`|`accept`|

5. **Gözden geçir ve oluştur** 'a tıklayın
6. Doğrulama işleminden sonra oluşturma işlemini tamamladıktan sonra **Oluştur** ' a tıklayın.
7. Kaynak başarıyla dağıtıldığında,
