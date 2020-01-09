---
author: IEvangelist
ms.author: dapine
ms.date: 06/25/2019
ms.service: cognitive-services
ms.topic: include
ms.openlocfilehash: a24300958c27daaaf49cc3045a5e99d77c938ab7
ms.sourcegitcommit: ce4a99b493f8cf2d2fd4e29d9ba92f5f942a754c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/28/2019
ms.locfileid: "67704190"
---
Kapsayıcıya yönelik sorgular, `<ApiKey>`için kullanılan Azure kaynağının fiyatlandırma katmanında faturalandırılır.

Azure bilişsel hizmetler kapsayıcıları, ölçüm için faturalandırma uç noktasına bağlı kalmadan çalıştırılmak üzere lisanslanmaz. Her zaman Faturalandırma bitiş noktasıyla faturalandırma bilgilerini iletmek için kapsayıcıları etkinleştirmeniz gerekir. Bilişsel hizmetler kapsayıcıları, müşteri verilerini (örneğin, çözümlenen resim veya metin gibi) Microsoft 'a göndermez. 

### <a name="connect-to-azure"></a>Azure'a Bağlanma

Kapsayıcının çalışması için faturalandırma bağımsız değişken değerlerinin olması gerekir. Bu değerler kapsayıcının faturalandırma uç noktasına bağlanmasına izin verir. Kapsayıcı her 10 ila 15 dakikada bir kullanım raporu sağlar. Kapsayıcı, izin verilen zaman penceresinde Azure 'a bağlanmazsa, kapsayıcı çalışmaya devam eder, ancak faturalandırma uç noktası geri yüklenene kadar sorgu hizmeti vermez. Bağlantı, 10 ila 15 dakika aynı zaman aralığında 10 kez denenir. 10 deneciler içindeki faturalandırma uç noktasına bağlanamıyorsa kapsayıcı çalışmayı durduruyor. 

### <a name="billing-arguments"></a>Faturalandırma bağımsız değişkenleri

Kapsayıcıyı başlatmak için `docker run` komutu için aşağıdaki seçeneklerden üçü de geçerli değerlerle belirtilmelidir:

| Seçenek | Açıklama |
|--------|-------------|
| `ApiKey` | Fatura bilgilerini izlemek için kullanılan bilişsel hizmetler kaynağının API anahtarı.<br/>Bu seçeneğin değeri, `Billing`belirtilen sağlanan kaynak için bir API anahtarı olarak ayarlanmalıdır. |
| `Billing` | Fatura bilgilerini izlemek için kullanılan bilişsel hizmetler kaynağının uç noktası.<br/>Bu seçeneğin değeri, sağlanan bir Azure kaynağının uç nokta URI 'sine ayarlanmalıdır.|
| `Eula` | Kapsayıcının lisansını kabul ettiğinizi gösterir.<br/>Bu seçeneğin değeri **kabul**edilecek şekilde ayarlanmalıdır. |


