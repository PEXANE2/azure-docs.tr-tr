---
title: Azure CLI'yi kullanarak Bilişsel Hizmetler kaynağı oluşturma
titleSuffix: Azure Cognitive Services
description: Azure komut satırı arabirimini kullanarak bir kaynağa abone olarak Azure Bilişsel Hizmetleri'ne başlayın.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: conceptual
ms.date: 10/04/2019
ms.author: aahi
ms.openlocfilehash: 72b00d78d19ed0e963b4dad01b82033c659e1efd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79219615"
---
# <a name="create-a-cognitive-services-resource-using-the-azure-command-line-interfacecli"></a>Azure Komut Satırı Arabirimi(CLI) kullanarak Bilişsel Hizmetler kaynağı oluşturma

[Azure Komut Satırı Arabirimi(CLI)](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)kullanarak Azure Bilişsel Hizmetleri'ne başlamak için bu hızlı başlangıcı kullanın. Bilişsel Hizmetler, Azure aboneliğinizde oluşturduğunuz Azure [kaynakları](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-portal) yla temsil edilir. Kaynağı oluşturduktan sonra, uygulamalarınızın kimliğini doğrulamak için oluşturulan tuşları ve bitiş noktasını kullanın. 


Bu hızlı başlangıçta, Azure Bilişsel Hizmetleri'ne nasıl kaydolacağız ve [Azure Komut Satırı Arabirimini (CLI)](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)kullanarak tek hizmet veya çok hizmet aboneliği olan bir hesap oluşturmayı öğreneceksiniz. Bu hizmetler, Azure Bilişsel Hizmetler API'lerinden birine veya daha fazlasına bağlanmanızı sağlayan Azure [kaynakları](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-portal)tarafından temsil edilir.

[!INCLUDE [cognitive-services-subscription-types](../../includes/cognitive-services-subscription-types.md)]

## <a name="prerequisites"></a>Ön koşullar

* Geçerli bir Azure aboneliği - Ücretsiz [bir abonelik oluşturun.](https://azure.microsoft.com/free/)
* [Azure Komut Satırı Arabirimi(CLI)](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)

## <a name="install-the-azure-cli-and-sign-in"></a>Azure CLI'yi yükleyin ve oturum açın 

Azure [CLI'yi](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)yükleyin. CLI'nin yerel yüklemesinde oturum açmak için [az giriş](https://docs.microsoft.com/cli/azure/reference-index#az-login) komutunu çalıştırın:

```azurecli-interactive
az login
```

Tarayıcınızda bu komutları çalıştırmak için yeşil **Try It** düğmesini de kullanabilirsiniz.
 
## <a name="create-a-new-azure-cognitive-services-resource-group"></a>Yeni bir Azure Bilişsel Hizmetler kaynak grubu oluşturun

Bilişsel Hizmetler kaynağı oluşturmadan önce, kaynağı içerecek bir Azure kaynak grubuna sahip olmalısınız. Yeni bir kaynak oluşturduğunuzda, yeni bir kaynak grubu oluşturma veya varolan bir kaynak kullanma seçeneğiniz olur. Bu makalede, yeni bir kaynak grubu oluşturmak için nasıl gösterir.

### <a name="choose-your-resource-group-location"></a>Kaynak grubu konumunuzu seçin

Kaynak oluşturmak için aboneliğiniz için kullanılabilen Azure konumlarından birine ihtiyacınız vardır. [Az hesap listesi konumları](/cli/azure/account#az-account-list-locations) komutu ile kullanılabilir yerlerin listesini alabilirsiniz. Bilişsel Hizmetlerin çoğuna çeşitli yerlerden erişilebilir. Size en yakın olanı seçin veya hizmet için hangi konumların kullanılabileceğini görün.

> [!IMPORTANT]
> * Azure Bilişsel Hizmetleri'ni ararken ihtiyacınız olacağı için Azure konumunuzu unutmayın.
> * Bazı Bilişsel Hizmetler durumu bölgeye göre değişebilir. Daha fazla bilgi için [bölgeye göre Azure ürünlerine](https://azure.microsoft.com/global-infrastructure/services/?products=cognitive-services)bakın.  

```azurecli-interactive
az account list-locations \
    --query "[].{Region:name}" \
    --out table
```

Azure konumunuzu aldıktan sonra, az grubu oluşturma komutunu kullanarak Azure CLI'de yeni bir kaynak grubu [oluşturun.](/cli/azure/group#az-group-create)

Aşağıdaki örnekte, azure konumunu `westus2` aboneliğiniz için kullanılabilen Azure konumlarından biriyle değiştirin.

```azurecli-interactive
az group create \
    --name cognitive-services-resource-group \
    --location westus2
```

## <a name="create-a-cognitive-services-resource"></a>Bilişsel Hizmetler kaynağı oluşturma

### <a name="choose-a-cognitive-service-and-pricing-tier"></a>Bilişsel hizmet ve fiyatlandırma katmanı seçin

Yeni bir kaynak oluştururken, istediğiniz [fiyatlandırma katmanıyla](https://azure.microsoft.com/pricing/details/cognitive-services/) (veya sku) birlikte kullanmak istediğiniz hizmetin "türünü" bilmeniz gerekir. Kaynağı oluştururken bu ve diğer bilgileri parametre olarak kullanırsınız.

### <a name="multi-service"></a>Çoklu hizmet

| Hizmet                    | Tip                      |
|----------------------------|---------------------------|
| Birden fazla hizmet. Daha fazla bilgi için [fiyatlandırma](https://azure.microsoft.com/pricing/details/cognitive-services/) sayfasına bakın.            | `CognitiveServices`     |


> [!NOTE]
> Aşağıdaki Bilişsel Hizmetler'in çoğu, hizmeti denemek için kullanabileceğiniz ücretsiz bir katmana sahiptir. Ücretsiz katmanı kullanmak için `F0` kaynağınız için sku olarak kullanın.

### <a name="vision"></a>Görsel

| Hizmet                    | Tip                      |
|----------------------------|---------------------------|
| Görüntü İşleme            | `ComputerVision`          |
| Özel Vizyon - Tahmin | `CustomVision.Prediction` |
| Özel Vizyon - Eğitim   | `CustomVision.Training`   |
| Yüz                       | `Face`                    |
| Form Tanıma            | `FormRecognizer`          |
| Mürekkep Tanıma             | `InkRecognizer`           |

### <a name="search"></a>Search

| Hizmet            | Tip                  |
|--------------------|-----------------------|
| Bing Otomatik Öneri   | `Bing.Autosuggest.v7` |
| Bing Özel Arama | `Bing.CustomSearch`   |
| Bing Varlık Arama | `Bing.EntitySearch`   |
| Bing Arama        | `Bing.Search.v7`      |
| Bing Yazım Denetimi   | `Bing.SpellCheck.v7`  |

### <a name="speech"></a>Konuşma

| Hizmet            | Tip                 |
|--------------------|----------------------|
| Konuşma Hizmetleri    | `SpeechServices`     |
| Konuşma Tanıma | `SpeakerRecognition` |

### <a name="language"></a>Dil

| Hizmet            | Tip                |
|--------------------|---------------------|
| Form Anlama | `FormUnderstanding` |
| LUIS               | `LUIS`              |
| Soru-Cevap Oluşturucu          | `QnAMaker`          |
| Metin Analizi     | `TextAnalytics`     |
| Metin Çevirisi   | `TextTranslation`   |

### <a name="decision"></a>Karar

| Hizmet           | Tip               |
|-------------------|--------------------|
| Anomali Algılayıcısı  | `AnomalyDetector`  |
| Content Moderator | `ContentModerator` |
| Kişiselleştirme      | `Personalizer`     |

Az bilişsel [hizmetler hesap listesi-türleri](https://docs.microsoft.com/cli/azure/cognitiveservices/account?view=azure-cli-latest#az-cognitiveservices-account-list-kinds) komutu ile kullanılabilir Bilişsel Hizmet "türleri" bir listesini bulabilirsiniz:

```azurecli-interactive
az cognitiveservices account list-kinds
```

### <a name="add-a-new-resource-to-your-resource-group"></a>Kaynak grubunuza yeni bir kaynak ekleme

Yeni bir Bilişsel Hizmetler kaynağı oluşturmak ve abone olmak için [az cognitiveservices hesabı oluşturma komutunu](https://docs.microsoft.com/cli/azure/cognitiveservices/account?view=azure-cli-latest#az-cognitiveservices-account-create) kullanın. Bu komut, daha önce oluşturulan kaynak grubuna yeni bir faturalandırılabilir kaynak ekler. Yeni kaynağınızı oluştururken, fiyatlandırma katmanı (veya sku) ve Azure konumuyla birlikte kullanmak istediğiniz "hizmet türünü" bilmeniz gerekir:

Anomaly Dedektörü için aşağıdaki komutla adlandırılan `anomaly-detector-resource` bir F0 (ücretsiz) kaynak oluşturabilirsiniz.

```azurecli-interactive
az cognitiveservices account create \
    --name anomaly-detector-resource \
    --resource-group cognitive-services-resource-group \
    --kind AnomalyDetector \
    --sku F0 \
    --location westus2 \
    --yes
```

## <a name="get-the-keys-for-your-resource"></a>Kaynağınızın anahtarlarını alın

Komut Satırı Arabirimi(CLI) yerel yükleme oturum açmak [için, az giriş](https://docs.microsoft.com/cli/azure/reference-index?view=azure-cli-latest#az-login) komutunu kullanın.

```azurecli-interactive
az login
```

Bilişsel Hizmet kaynağınızın anahtarlarını almak için [az cognitiveservices hesap anahtarları listesi](https://docs.microsoft.com/cli/azure/cognitiveservices/account/keys?view=azure-cli-latest#az-cognitiveservices-account-keys-list) komutunu kullanın.

```azurecli-interactive
    az cognitiveservices account keys list \
    --name anomaly-detector-resource \
    --resource-group cognitive-services-resource-group
```

[!INCLUDE [cognitive-services-environment-variables](../../includes/cognitive-services-environment-variables.md)]

## <a name="pricing-tiers-and-billing"></a>Fiyatlandırma katmanları ve faturalandırma

Fiyatlandırma katmanları (ve faturalandırılan tutar), kimlik doğrulama bilgilerinizi kullanarak gönderdiğiniz işlem sayısına bağlıdır. Her fiyatlandırma katmanı aşağıdakileri belirtir:
* saniyede izin verilen en fazla hareket sayısı (TPS).
* fiyatlandırma katmanı içinde etkin leştirilmiş hizmet özellikleri.
* Önceden tanımlanmış bir hareket tutarının maliyeti. Bu tutarın üzerine gitmek, hizmetinizin [fiyatlandırma ayrıntılarında](https://azure.microsoft.com/pricing/details/cognitive-services/custom-vision-service/) belirtildiği gibi ek bir ücrete neden olacaktır.

## <a name="get-current-quota-usage-for-your-resource"></a>Kaynağınız için geçerli kota kullanımını alın

Bilişsel Hizmet kaynağınızın kullanımını elde etmek için [az cognitiveservices hesap listesi kullanım](https://docs.microsoft.com/cli/azure/cognitiveservices/account?view=azure-cli-latest#az-cognitiveservices-account-list-usage) komutunu kullanın.

```azurecli-interactive
az cognitiveservices account list-usage \
    --name anomaly-detector-resource \
    --resource-group cognitive-services-resource-group \
    --subscription subscription-name
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bilişsel Hizmetler kaynağını temizlemek ve kaldırmak istiyorsanız, kaynağı veya kaynak grubunu silebilirsiniz. Kaynak grubunun silinmesi, grupta bulunan diğer kaynakları da siler.

Kaynak grubunu ve ilişkili kaynaklarını kaldırmak için az grubu silme komutunu kullanın.

```azurecli-interactive
az group delete --name cognitive-services-resource-group
```

## <a name="see-also"></a>Ayrıca bkz.

* [Azure Bilişsel Hizmetleri'ndeki istekleri doğrulama](authentication.md)
* [Azure Bilişsel Hizmetler nedir?](Welcome.md)
* [Doğal dil desteği](language-support.md)
* [Docker konteyner desteği](cognitive-services-container-support.md)
