---
author: aahill
ms.author: aahi
ms.service: cognitive-services
ms.topic: include
ms.date: 10/08/2019
ms.openlocfilehash: 5089af4a4e1714d49b844a1b6823487a3f6a8dcf
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74483024"
---
Aşağıdaki Azure kaynaklarından birini oluşturarak Anomaly Detector hizmetini kullanmaya başlayın.

* <a href="https://azure.microsoft.com/try/cognitive-services/#decision" target="_blank" rel="noopener">Deneme kaynağı oluşturma (yeni bir sekmede açılır)</a>
    * Azure aboneliği gerekmez: 
    * Yedi gün boyunca ücretsiz. Kaydolduktan sonra, [Azure web sitesinde](https://azure.microsoft.com/try/cognitive-services/my-apis/)deneme anahtarı ve bitiş noktası kullanılabilir. 
    * Anomaly Detector'ı denemek istiyorsanız, ancak Azure aboneliğiniz yoksa bu harika bir seçenektir.

* <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesAnomalyDetector" target="_blank" rel="noopener">Bir Anomali Dedektörü kaynağı oluşturma (yeni bir sekmede açılır)</a>:
    * Kaynağı silene kadar Azure portalı nda kullanılabilir.
    * Hizmeti denemek için ücretsiz fiyatlandırma katmanını kullanın ve daha sonra üretim için ücretli bir katmana yükseltin.



### <a name="create-an-environment-variable"></a>Bir ortam değişkeni oluşturma

>[!NOTE]
> 1 Temmuz 2019'dan sonra oluşturulan deneme dışı kaynakların bitiş noktaları, aşağıda gösterilen özel alt etki alanı biçimini kullanır. Daha fazla bilgi ve bölgesel uç noktaların tam listesi [için, Bilişsel Hizmetler için Özel alt alan adları bölümüne](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-custom-subdomains)bakın. 

Oluşturduğunuz kaynaktan anahtar ve bitiş noktanızı kullanarak kimlik doğrulaması için iki ortam değişkeni oluşturun:

* `ANOMALY_DETECTOR_KEY`- İsteklerinizi doğrulamak için kaynak anahtarı.
* `ANOMALY_DETECTOR_ENDPOINT`- API isteklerini göndermek için kaynak bitiş noktası. Bu gibi görünecektir: 
  * `https://<your-custom-subdomain>.api.cognitive.microsoft.com` 

İşletim sisteminiziçin yönergeleri kullanın.

#### <a name="windows"></a>[Windows](#tab/windows)

```console
setx ANOMALY_DETECTOR_KEY <replace-with-your-anomaly-detector-key>
setx ANOMALY_DETECTOR_ENDPOINT <replace-with-your-anomaly-detector-endpoint>
```

Ortam değişkenini ekledikten sonra konsol penceresini yeniden başlatın.

#### <a name="linux"></a>[Linux](#tab/linux)

```bash
export ANOMALY_DETECTOR_KEY=<replace-with-your-anomaly-detector-key>
export ANOMALY_DETECTOR_ENDPOINT=<replace-with-your-anomaly-detector-endpoint>
```

Ortam değişkenini ekledikten sonra değişiklikleri uygulamak için konsol pencerenizden `source ~/.bashrc` çalıştırın.

#### <a name="macos"></a>[Macos](#tab/unix)

'nizi `.bash_profile`ve ortam değişkenini ekleyin:

```bash
export ANOMALY_DETECTOR_KEY=<replace-with-your-anomaly-detector-key>
export ANOMALY_DETECTOR_ENDPOINT=<replace-with-your-anomaly-detector-endpoint>
```

Ortam değişkenini ekledikten sonra değişiklikleri uygulamak için konsol pencerenizden `source .bash_profile` çalıştırın.

***