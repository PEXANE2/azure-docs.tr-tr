---
author: IEvangelist
ms.author: dapine
ms.date: 02/19/2020
ms.service: cognitive-services
ms.topic: include
ms.openlocfilehash: 2ac93f5aba722eea78267a512999a5581a887b99
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77474200"
---
Kapsayıcıdaki sorgular, `ApiKey`Azure kaynağının fiyatlandırma katmanında faturalandırılır.

Azure Bilişsel Hizmetler kapsayıcıları, ölçüm / faturalandırma bitiş noktasına bağlı olmadan çalışma lisansına sahip değildir. Kapsayıcıların fatura landırma bilgilerini her zaman fatura bitiş noktasıyla iletmesini sağlamanız gerekir. Bilişsel Hizmetler kapsayıcıları, analiz edilen resim veya metin gibi müşteri verilerini Microsoft'a göndermez.

### <a name="connect-to-azure"></a>Azure'a Bağlanma

Kapsayıcının çalışması için fatura bağımsız değişkeni değerlerine ihtiyacı vardır. Bu değerler, kapsayıcının faturalandırma bitiş noktasına bağlanmasına olanak sağlar. Kapsayıcı her 10 ila 15 dakikada bir kullanımı bildirir. Kapsayıcı izin verilen zaman penceresi içinde Azure'a bağlanmazsa, kapsayıcı çalışmadevam eder, ancak faturalandırma bitiş noktası geri yüklenene kadar sorguları sunmaz. Bağlantı 10 ila 15 dakikalık aynı zaman aralığında 10 kez denenir. 10 denemede faturalandırma bitiş noktasına bağlanamazsa, kapsayıcı isteklerin sunulmasını durdurur.

### <a name="billing-arguments"></a>Faturalama bağımsız değişkenleri

<a href="https://docs.docker.com/engine/reference/commandline/run/" target="_blank"> `docker run` Aşağıdaki <span class="docon docon-navigate-external x-hidden-focus"></span> </a> seçeneklerin üçü geçerli değerlerle sağlandığında komut kapsayıcıyı başlatır:

| Seçenek | Açıklama |
|--------|-------------|
| `ApiKey` | Fatura bilgilerini izlemek için kullanılan Bilişsel Hizmetler kaynağının API anahtarı.<br/>Bu seçeneğin değeri, `Billing`'de belirtilen sağlanan kaynak için bir API anahtarına ayarlanmalıdır. |
| `Billing` | Fatura bilgilerini izlemek için kullanılan Bilişsel Hizmetler kaynağının bitiş noktası.<br/>Bu seçeneğin değeri, sağlanan bir Azure kaynağının bitiş noktası URI olarak ayarlanmalıdır.|
| `Eula` | Kapsayıcının lisansını kabul ettiğinizi gösterir.<br/>Bu seçeneğin değeri **kabul**etmek için ayarlanmalıdır. |
