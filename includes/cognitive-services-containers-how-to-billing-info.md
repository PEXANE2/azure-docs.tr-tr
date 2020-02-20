---
author: IEvangelist
ms.author: dapine
ms.date: 02/19/2020
ms.service: cognitive-services
ms.topic: include
ms.openlocfilehash: 2ac93f5aba722eea78267a512999a5581a887b99
ms.sourcegitcommit: 64def2a06d4004343ec3396e7c600af6af5b12bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77474200"
---
Kapsayıcıya yönelik sorgular, `ApiKey`için kullanılan Azure kaynağının fiyatlandırma katmanında faturalandırılır.

Azure bilişsel hizmetler kapsayıcıları, ölçüm/faturalandırma uç noktasına bağlı kalmadan çalıştırılmak üzere lisanslanmaz. Her zaman Faturalandırma bitiş noktasıyla faturalandırma bilgilerini iletmek için kapsayıcıları etkinleştirmeniz gerekir. Bilişsel hizmetler kapsayıcıları, müşteri verilerini (örneğin, çözümlenen resim veya metin gibi) Microsoft 'a göndermez.

### <a name="connect-to-azure"></a>Azure'a Bağlanma

Kapsayıcının çalışması için faturalandırma bağımsız değişken değerlerinin olması gerekir. Bu değerler kapsayıcının faturalandırma uç noktasına bağlanmasına izin verir. Kapsayıcı her 10 ila 15 dakikada bir kullanım raporu sağlar. Kapsayıcı, izin verilen zaman penceresinde Azure 'a bağlanmazsa, kapsayıcı çalışmaya devam eder, ancak faturalandırma uç noktası geri yüklenene kadar sorgu hizmeti vermez. Bağlantı, 10 ila 15 dakika aynı zaman aralığında 10 kez denenir. 10 deneciler içindeki faturalandırma uç noktasına bağlanamıyorsa kapsayıcı, istekleri sunmaya yanıt vermez.

### <a name="billing-arguments"></a>Faturalandırma bağımsız değişkenleri

Aşağıdaki seçeneklerden üçü de geçerli değerlerle sağlandıysa <a href="https://docs.docker.com/engine/reference/commandline/run/" target="_blank">`docker run` <span class="docon docon-navigate-external x-hidden-focus"></span> </a> komut kapsayıcıyı başlatır:

| Seçenek | Açıklama |
|--------|-------------|
| `ApiKey` | Fatura bilgilerini izlemek için kullanılan bilişsel hizmetler kaynağının API anahtarı.<br/>Bu seçeneğin değeri, `Billing`belirtilen sağlanan kaynak için bir API anahtarı olarak ayarlanmalıdır. |
| `Billing` | Fatura bilgilerini izlemek için kullanılan bilişsel hizmetler kaynağının uç noktası.<br/>Bu seçeneğin değeri, sağlanan bir Azure kaynağının uç nokta URI 'sine ayarlanmalıdır.|
| `Eula` | Kapsayıcının lisansını kabul ettiğinizi gösterir.<br/>Bu seçeneğin değeri **kabul**edilecek şekilde ayarlanmalıdır. |
