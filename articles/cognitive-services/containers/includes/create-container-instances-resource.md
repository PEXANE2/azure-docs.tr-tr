---
title: Kapsayıcı desteği
titleSuffix: Azure Cognitive Services
description: Azure Container Instance kaynağı oluşturmayı öğrenin.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 11/21/2019
ms.author: dapine
ms.openlocfilehash: 18f4edf5cc63a448779423cc1b302130b4b80724
ms.sourcegitcommit: 2f8ff235b1456ccfd527e07d55149e0c0f0647cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/07/2020
ms.locfileid: "75692522"
---
## <a name="create-an-azure-container-instance-resource"></a>Azure Container Instance kaynağı oluşturma

1. Container Instances için [Oluştur](https://ms.portal.azure.com/#create/Microsoft.ContainerInstances) sayfasına gidin.

2. **Temel bilgiler** sekmesinde, aşağıdaki ayrıntıları girin:

    |Ayar|Değer|
    |--|--|
    |Abonelik|Aboneliğinizi seçin.|
    |Kaynak grubu|Kullanılabilir kaynak grubunu seçin veya `cognitive-services`gibi yeni bir tane oluşturun.|
    |Kapsayıcı adı|`cognitive-container-instance`gibi bir ad girin. Ad, daha küçük bir harf olmalıdır.|
    |Konum|Dağıtım için bir bölge seçin.|
    |Görüntü türü|Kapsayıcı resminiz kimlik bilgileri gerektirmeyen bir kapsayıcı kayıt defterinde depolanıyorsa `Public`seçin. Kapsayıcı yansımanıza erişim için kimlik bilgileri gerekiyorsa `Private`' yi seçin. Kapsayıcı görüntüsünün `Public` mı yoksa `Private` ("genel önizleme") olup olmadığına ilişkin ayrıntılar için [kapsayıcı depoları ve resimlere](../../cognitive-services-container-support.md#container-repositories-and-images) bakın. |
    |Görüntü adı|Bilişsel hizmetler kapsayıcı konumunu girin. Konum, `docker pull` komutuna bir bağımsız değişken olarak kullanılan şeydir. Kullanılabilir görüntü adları ve bunlara karşılık gelen depo için [kapsayıcı depoları ve görüntüleri](../../cognitive-services-container-support.md#container-repositories-and-images) inceleyin.<br><br>Görüntü adı, üç bölümden oluşan tam olarak nitelenmiş olmalıdır. İlk olarak, kapsayıcı kayıt defteri, ardından depo, son olarak görüntü adı: `<container-registry>/<repository>/<image-name>`.<br><br>Aşağıda, `mcr.microsoft.com/azure-cognitive-services/keyphrase` Azure bilişsel hizmetler deposu altında Microsoft Container Registry Anahtar İfade Ayıklama görüntüsünü temsil eden bir örnek verilmiştir. Diğer bir örnek `containerpreview.azurecr.io/microsoft/cognitive-services-speech-to-text`, kapsayıcı önizleme kapsayıcısı kayıt defterinin Microsoft deposundaki konuşma metin görüntüsünü temsil eden bir örnektir. |
    |İşletim sistemi türü|`Linux`|
    |Boyut|Belirli bilişsel hizmet Kapsayıcınızın boyutunu önerilen önerilerle değiştirin:<br>2 CPU çekirdeği<br>4 GB

3. **Ağ** sekmesinde, aşağıdaki ayrıntıları girin:

    |Ayar|Değer|
    |--|--|
    |Bağlantı Noktaları|TCP bağlantı noktasını `5000`olarak ayarlayın. 5000 numaralı bağlantı noktasında kapsayıcıyı kullanıma sunar.|

4. **Gelişmiş** sekmesinde, Azure Container Instance kaynağının kapsayıcı faturalama ayarları Için gereken **ortam değişkenlerini** girin:

    | Anahtar | Değer |
    |--|--|
    |`apikey`|Kaynağın **anahtarlar** sayfasından kopyalanmış. Boşluk veya kesik çizgi olmayan 32 alfasayısal karakter dizesidir `xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`.|
    |`billing`|Kaynağın **genel bakış** sayfasından kopyalanmış.|
    |`eula`|`accept`|

5. **Gözden geçir ve oluştur** 'a tıklayın
6. Doğrulama işleminden sonra oluşturma işlemini tamamladıktan sonra **Oluştur** ' a tıklayın.
7. Kaynak başarıyla dağıtıldığında,
