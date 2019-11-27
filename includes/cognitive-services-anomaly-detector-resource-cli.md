---
author: aahill
ms.author: aahi
ms.service: cognitive-services
ms.topic: include
ms.date: 10/08/2019
ms.openlocfilehash: 5089af4a4e1714d49b844a1b6823487a3f6a8dcf
ms.sourcegitcommit: 8cf199fbb3d7f36478a54700740eb2e9edb823e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/25/2019
ms.locfileid: "74483024"
---
Aşağıdaki Azure kaynaklarından birini oluşturarak anomali algılayıcı hizmetini kullanmaya başlayın.

* <a href="https://azure.microsoft.com/try/cognitive-services/#decision" target="_blank" rel="noopener">Deneme kaynağı oluşturma (yeni bir sekmede açılır)</a>
    * Azure aboneliği gerekli değil: 
    * Ücretsiz olarak yedi gün için geçerlidir. Kaydolduktan sonra [Azure Web sitesinde](https://azure.microsoft.com/try/cognitive-services/my-apis/)bir deneme anahtarı ve uç noktası kullanılabilir olacaktır. 
    * Anomali algılayıcısı denemek istiyorsanız, Azure aboneliğiniz yoksa bu harika bir seçenektir.

* <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesAnomalyDetector" target="_blank" rel="noopener">Anomali algılayıcı kaynağı oluşturma (yeni bir sekmede açılır)</a>:
    * Kaynak silininceye kadar Azure portal ile kullanılabilir.
    * Ücretsiz fiyatlandırma katmanını kullanarak hizmeti deneyin ve daha sonra üretime yönelik ücretli bir katmana yükseltin.



### <a name="create-an-environment-variable"></a>Ortam değişkeni oluşturma

>[!NOTE]
> 1 Temmuz 2019 ' den sonra oluşturulan deneme olmayan kaynaklar için uç noktalar aşağıda gösterilen özel alt etki alanı biçimini kullanır. Daha fazla bilgi ve bölgesel uç noktaların tamamen listesi için bkz. bilişsel [Hizmetler Için özel alt etki alanı adları](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-custom-subdomains). 

Oluşturduğunuz kaynaktan anahtarınızı ve uç noktayı kullanarak, kimlik doğrulama için iki ortam değişkeni oluşturun:

* `ANOMALY_DETECTOR_KEY`-isteklerinizin kimliğini doğrulamak için kaynak anahtarı.
* `ANOMALY_DETECTOR_ENDPOINT`-API isteklerinin gönderilmesi için kaynak uç noktası. Şöyle görünür: 
  * `https://<your-custom-subdomain>.api.cognitive.microsoft.com` 

İşletim sisteminiz için yönergeleri kullanın.

#### <a name="windowstabwindows"></a>[Windows](#tab/windows)

```console
setx ANOMALY_DETECTOR_KEY <replace-with-your-anomaly-detector-key>
setx ANOMALY_DETECTOR_ENDPOINT <replace-with-your-anomaly-detector-endpoint>
```

Ortam değişkenini ekledikten sonra konsol penceresini yeniden başlatın.

#### <a name="linuxtablinux"></a>[Linux](#tab/linux)

```bash
export ANOMALY_DETECTOR_KEY=<replace-with-your-anomaly-detector-key>
export ANOMALY_DETECTOR_ENDPOINT=<replace-with-your-anomaly-detector-endpoint>
```

Ortam değişkenini ekledikten sonra değişiklikleri uygulamak için konsol pencerenizden `source ~/.bashrc` çalıştırın.

#### <a name="macostabunix"></a>[macOS](#tab/unix)

`.bash_profile`düzenleyin ve ortam değişkenini ekleyin:

```bash
export ANOMALY_DETECTOR_KEY=<replace-with-your-anomaly-detector-key>
export ANOMALY_DETECTOR_ENDPOINT=<replace-with-your-anomaly-detector-endpoint>
```

Ortam değişkenini ekledikten sonra değişiklikleri uygulamak için konsol pencerenizden `source .bash_profile` çalıştırın.

***