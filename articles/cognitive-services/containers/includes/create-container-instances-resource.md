---
title: Kapsayıcı desteği
titleSuffix: Azure Cognitive Services
description: Azure kapsayıcı örnek kaynağını nasıl oluşturabilirsiniz öğrenin.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 11/21/2019
ms.author: dapine
ms.openlocfilehash: 18f4edf5cc63a448779423cc1b302130b4b80724
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "75692522"
---
## <a name="create-an-azure-container-instance-resource"></a>Azure Kapsayıcı Örneği kaynağı oluşturma

1. Kapsayıcı Örnekleri için [Oluştur](https://ms.portal.azure.com/#create/Microsoft.ContainerInstances) sayfasına gidin.

2. Temel **bilgiler** sekmesinde aşağıdaki ayrıntıları girin:

    |Ayar|Değer|
    |--|--|
    |Abonelik|Aboneliğinizi seçin.|
    |Kaynak grubu|Kullanılabilir kaynak grubunu seçin veya `cognitive-services`yeni bir tane oluşturun.|
    |Kapsayıcı adı|Gibi bir ad `cognitive-container-instance`girin. Ad alt kapaklarda olmalıdır.|
    |Konum|Dağıtım için bir bölge seçin.|
    |Görüntü türü|Kapsayıcı resminiz kimlik bilgileri gerektirmeyen bir kapsayıcı kayıt defterinde `Public`depolanmışsa, 'yi seçin. Kapsayıcı resminize erişmek kimlik bilgileri `Private`gerektiriyorsa, 'yi seçin. Kapsayıcı görüntüsünün veya `Public` ("Genel Önizleme") olup olmadığı yla `Private` ilgili ayrıntılar için [kapsayıcı depolarına ve görüntülerine](../../cognitive-services-container-support.md#container-repositories-and-images) bakın. |
    |Görüntü adı|Bilişsel Hizmetler konteyner konumunu girin. Konum, `docker pull` komutun bağımsız değişkeni olarak kullanılan yerdir. Kullanılabilir resim adları ve bunların karşılık gelen depoları için [kapsayıcı depolarına](../../cognitive-services-container-support.md#container-repositories-and-images) ve görüntülerine bakın.<br><br>Görüntü adı üç bölümden belirten tam nitelikli olmalıdır. İlk olarak, konteyner kayıt defteri, sonra depo, nihayet `<container-registry>/<repository>/<image-name>`görüntü adı: .<br><br>Aşağıda, `mcr.microsoft.com/azure-cognitive-services/keyphrase` Azure Bilişsel Hizmetler deposu nun altındaki Microsoft Kapsayıcı Kayıt Defteri'ndeki Anahtar Tümcecik Ayıklama görüntüsünü temsil eden bir örnek verilmiştir. Başka bir `containerpreview.azurecr.io/microsoft/cognitive-services-speech-to-text` örnek, Kapsayıcı Önizleme kapsayıcı kayıt defterinin Microsoft deposunda Metin Için Konuşma görüntüsünü temsil eder. |
    |İşletim sistemi türü|`Linux`|
    |Boyut|Belirli Bilişsel Hizmet kapsayıcınız için önerilen önerilerin boyutunu değiştirin:<br>2 CPU çekirdeği<br>4 GB

3. **Ağ** sekmesinde aşağıdaki ayrıntıları girin:

    |Ayar|Değer|
    |--|--|
    |Bağlantı Noktaları|TCP bağlantı noktasını `5000`' n' olarak ayarlayın 5000 portundaki konteynırı ortaya çıkarır.|

4. **Gelişmiş** sekmesinde, Azure Kapsayıcı Örneği kaynağının kapsayıcı faturalandırma ayarları için gerekli **Ortam Değişkenlerini** girin:

    | Anahtar | Değer |
    |--|--|
    |`apikey`|Kaynağın **Keys** sayfasından kopyalanır. Boşluksuz veya tiresiz 32 alfanümerik karakterli bir `xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`dizedir.|
    |`billing`|Kaynağın **Genel Bakış** sayfasından kopyalanır.|
    |`eula`|`accept`|

5. **Gözden Geçir ve Oluştur'u** tıklatın
6. Doğrulama geçtikten sonra, oluşturma işlemini tamamlamak için **Oluştur'u** tıklatın
7. Kaynak başarıyla dağıtıldığında,
