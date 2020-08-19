---
author: IEvangelist
ms.author: dapine
ms.date: 02/19/2020
ms.service: cognitive-services
ms.topic: include
ms.openlocfilehash: 2ac93f5aba722eea78267a512999a5581a887b99
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "77474200"
---
Kapsayıcıya yönelik sorgular, için kullanılan Azure kaynağının fiyatlandırma katmanında faturalandırılır `ApiKey` .

Azure bilişsel hizmetler kapsayıcıları, ölçüm/faturalandırma uç noktasına bağlı kalmadan çalıştırılmak üzere lisanslanmaz. Her zaman Faturalandırma bitiş noktasıyla faturalandırma bilgilerini iletmek için kapsayıcıları etkinleştirmeniz gerekir. Bilişsel hizmetler kapsayıcıları, müşteri verilerini (örneğin, çözümlenen resim veya metin gibi) Microsoft 'a göndermez.

### <a name="connect-to-azure"></a>Azure'a Bağlanma

Kapsayıcının çalışması için faturalandırma bağımsız değişken değerlerinin olması gerekir. Bu değerler kapsayıcının faturalandırma uç noktasına bağlanmasına izin verir. Kapsayıcı her 10 ila 15 dakikada bir kullanım raporu sağlar. Kapsayıcı, izin verilen zaman penceresinde Azure 'a bağlanmazsa, kapsayıcı çalışmaya devam eder, ancak faturalandırma uç noktası geri yüklenene kadar sorgu hizmeti vermez. Bağlantı, 10 ila 15 dakika aynı zaman aralığında 10 kez denenir. 10 deneciler içindeki faturalandırma uç noktasına bağlanamıyorsa kapsayıcı, istekleri sunmaya yanıt vermez.

### <a name="billing-arguments"></a>Faturalandırma bağımsız değişkenleri

Aşağıdaki seçeneklerden üçü de geçerli değerlerle sağlandığında <a href="https://docs.docker.com/engine/reference/commandline/run/" target="_blank">komut kapsayıcıyı başlatır: `docker run` <span class="docon docon-navigate-external x-hidden-focus"></span> </a>

| Seçenek | Açıklama |
|--------|-------------|
| `ApiKey` | Fatura bilgilerini izlemek için kullanılan bilişsel hizmetler kaynağının API anahtarı.<br/>Bu seçeneğin değeri, içinde belirtilen sağlanan kaynak için bir API anahtarı olarak ayarlanmalıdır `Billing` . |
| `Billing` | Fatura bilgilerini izlemek için kullanılan bilişsel hizmetler kaynağının uç noktası.<br/>Bu seçeneğin değeri, sağlanan bir Azure kaynağının uç nokta URI 'sine ayarlanmalıdır.|
| `Eula` | Kapsayıcının lisansını kabul ettiğinizi gösterir.<br/>Bu seçeneğin değeri **kabul**edilecek şekilde ayarlanmalıdır. |
