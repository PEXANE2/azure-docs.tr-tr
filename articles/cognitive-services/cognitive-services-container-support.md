---
title: Kapsayıcı desteği
titleSuffix: Azure Cognitive Services
description: Docker Kapsayıcıları 'nin bilişsel Hizmetleri verilerinize daha yakın bir şekilde nasıl alabileceğinizi öğrenin.
services: cognitive-services
author: aahill
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.topic: article
ms.date: 05/07/2020
ms.author: aahi
ms.openlocfilehash: 69984f9dbd94bcdca2e272a5bdebbb7fc1464dae
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86104426"
---
# <a name="container-support-in-azure-cognitive-services"></a>Azure bilişsel hizmetler 'de kapsayıcı desteği

Azure bilişsel hizmetler 'deki kapsayıcı desteği, geliştiricilerin Azure 'da kullanılabilen aynı zengin API 'Leri kullanmasına izin verir ve [Docker kapsayıcılarıyla](https://www.docker.com/what-container)gelen hizmetlerin dağıtılması ve barındıralınması için esneklik sağlar. Kapsayıcı desteği şu anda Azure bilişsel hizmetler 'in şu parçalar dahil bir alt kümesi için kullanılabilir:

> [!div class="checklist"]
> * [Anomali Algılayıcısı][ad-containers]
> * [Görüntü İşleme][cv-containers]
> * [Yüz Tanıma][fa-containers]
> * [Form Tanıma][fr-containers]
> * [Language Understanding (LUIS)][lu-containers]
> * [Konuşma Hizmeti API’si][sp-containers]
> * [Metin Analizi][ta-containers]

> [!VIDEO https://www.youtube.com/embed/hdfbn4Q8jbo]

Kapsayıcılama, bağımlılık & yapılandırma dahil olmak üzere bir uygulama veya hizmetin bir kapsayıcı görüntüsü olarak birlikte paketlendiği yazılım dağıtımına yönelik bir yaklaşımdır. Çok az değişiklikle bir kapsayıcı ana bilgisayarına bir kapsayıcı görüntüsü dağıtılabilir. Kapsayıcılar, bir sanal makineden daha küçük bir parmak izine sahip ve temel alınan işletim sisteminden yalıtılmıştır. Kapsayıcılar, kısa süreli görevlere ait kapsayıcı görüntülerinden oluşturulabilir ve artık gerekli olmadığında kaldırılır.

Bilişsel hizmetler kaynakları [Microsoft Azure](https://azure.microsoft.com)kullanılabilir. Bu hizmetler için Azure kaynakları oluşturmak ve araştırmak üzere [Azure Portal](https://portal.azure.com/) oturum açın.

## <a name="features-and-benefits"></a>Özellikler ve avantajlar

- **Sabit altyapı**: değişikliklere uyum sağlayabilirken, düzenli ve güvenilir bir bilinen sistem parametreleri kümesinden yararlanmak Için DevOps ekiplerini etkinleştirin. Kapsayıcılar, öngörülebilir bir ekosistem içinde Pivot esnekliği sağlar ve yapılandırma drmasını önler.
- **Veriler üzerinde denetim**: müşterilerin bu bilişsel hizmetlerin verileri nerede işleyeceğini seçmesine izin verin. Bu, buluta veri gönderemediği ancak bilişsel hizmetler teknolojisine erişmesi gereken müşteriler için gereklidir. Karma ortamlarda, veriler, yönetim, kimlik ve güvenlik arasında tutarlılık desteği.
- **Model güncelleştirmeleri üzerinde denetim**: müşterilere, çözümlerinde dağıtılan modellerin sürümü oluşturma ve güncelleştirme konusunda esneklik sağlar.
- **Taşınabilir mimari**: Azure 'da, şirket içinde ve kenarda dağıtılabilecek taşınabilir bir uygulama mimarisi oluşturmayı etkinleştirin. Kapsayıcılar doğrudan [Azure Kubernetes hizmetine](../aks/index.yml), [Azure Container Instances](../container-instances/index.yml)veya [Azure Stack](/azure-stack/operator)dağıtılan bir [Kubernetes](https://kubernetes.io/) kümesine dağıtılabilir. Daha fazla bilgi için bkz. [Azure Stack Için Kubernetes dağıtma](/azure-stack/user/azure-stack-solution-template-kubernetes-deploy).
- **Yüksek aktarım hızı/düşük gecikme süresi**: müşterilerin uygulama mantığına ve verilerine fiziksel olarak yakın şekilde çalışmasını sağlayarak yüksek aktarım hızı ve düşük gecikme süreli gereksinimlere yönelik ölçeklendirme olanağı sağlar. Kapsayıcılar, saniye başına işlem (TPS) değildir ve gerekli donanım kaynaklarını sağladıysanız, her ikisi de ölçeği yukarı ve dışarı işlemek için kullanılabilir.
- **Ölçeklenebilirlik**: sürekli büyüyen ve Kubernetes gibi kapsayıcı düzenleme yazılımıyla birlikte artan popülerlik. ölçeklenebilirlik, Forefront of teknolojik gelişmelerden oluşur. Ölçeklenebilir bir küme kurucusu üzerinde derleme, uygulama geliştirme ve yüksek kullanılabilirlik.

## <a name="containers-in-azure-cognitive-services"></a>Azure bilişsel hizmetler 'deki kapsayıcılar

Azure bilişsel hizmetler kapsayıcıları, her biri Azure bilişsel hizmetler 'deki hizmetlerden bir işlev alt kümesini içeren aşağıdaki Docker Kapsayıcıları kümesini sağlar:

| Hizmet | Desteklenen Fiyatlandırma Katmanı | Kapsayıcı | Description |
|--|--|--|--|
| [Anomali algılayıcısı][ad-containers] | F0, S0 | **Anomali-algılayıcı** | Anomali algılayıcı API 'SI, makine öğrenimi ile zaman serisi verilerinizde bulunan normalleştirikleri izlemenize ve algılamanıza olanak sağlar.<br>[Erişim izni isteme][request-access] |
| [Görüntü İşleme][cv-containers] | F0, S1 | **Okuma** | Farklı yüzeylerden (örneğin, alındılar, posterleri ve iş kartları), çeşitli nesnelerin görüntülerinden yazdırılan metni ayıklar. Okuma kapsayıcısı resimlerde *el yazısı metinleri* de ALGıLAR ve PDF/TIFF/çok sayfalı destek sağlar.<br/><br/>**Önemli:** Okuma kapsayıcısı Şu anda yalnızca Ingilizce ile çalışmaktadır. |
| [Yüz Tanıma][fa-containers] | F0, S0 | **Yüz Tanıma** | Görüntülerde insan yüzlerini algılar ve yüzleri (Nosi ve gözler gibi), cinsiyet, yaş ve diğer makine tarafından öngörülen yüz özellikleri dahil olmak üzere öznitelikleri tanımlar. Algılamanın yanı sıra, benzer bir veya aynı yüzün zaten mevcut olup olmadığını görmek için aynı görüntüde veya farklı görüntülerde iki yüzün aynı olup olmadığını denetleyebilir veya bir veritabanı ile yüzleri karşılaştırın. Ayrıca, paylaşılan görsel nitelikleri kullanarak benzer yüzeyleri gruplar halinde düzenleyebilir.<br>[Erişim izni isteme][request-access] |
| [Form tanıyıcı][fr-containers] | F0, S0 | **Form Tanıma** | Form, formlardan anahtar-değer çiftlerini ve tabloları tanımlamak ve ayıklamak için makine öğrenimi teknolojisini uygular.<br>[Erişim izni isteme][request-access] |
| [LUIS][lu-containers] | F0, S0 | **Lusıs** ([görüntü](https://go.microsoft.com/fwlink/?linkid=2043204&clcid=0x409)) | Bir Docker kapsayıcısı olarak da bilinen eğitilen veya yayınlanan Language Understanding modelini bir Docker kapsayıcısına yükler ve kapsayıcının API uç noktalarından sorgu tahminlerine erişim sağlar. Kapsayıcıda sorgu günlüklerini toplayabilir ve uygulamanın tahmin doğruluğunu artırmak için bunları [Luo portalına](https://www.luis.ai) geri yükleyebilirsiniz. |
| [Konuşma Hizmeti API’si][sp-containers-stt] | F0, S0 | **Konuşmayı metne dönüştürme** | Sürekli, gerçek zamanlı konuşmaları metne dönüştürür. |
| [Konuşma Hizmeti API’si][sp-containers-cstt] | F0, S0 | **Özel Konuşma Tanıma metin** | Özel bir model kullanarak sürekli gerçek zamanlı konuşmayı metne dönüştürme. |
| [Konuşma Hizmeti API’si][sp-containers-tts] | F0, S0 | **Metin okuma** | Metni, doğal sesli konuşmaya dönüştürür. |
| [Konuşma Hizmeti API’si][sp-containers-ctts] | F0, S0 | **Özel metin okuma** | Özel bir model kullanarak metni doğal-sounkonuşmaya dönüştürür. |
| [Metin Analizi][ta-containers-keyphrase] | F0, S | **Anahtar ifade ayıklama** ([görüntü](https://go.microsoft.com/fwlink/?linkid=2018757&clcid=0x409)) | Ana noktaları belirlemek için anahtar tümceleri ayıklar. Örneğin, "The food was delicious and there were wonderful staff" (Yemekler lezzetliydi ve personel harikaydı) giriş metni olduğunda API, "food" (yemek) ve "wonderful staff" (personel harikaydı) ana konuşma noktalarını döndürür. |
| [Metin Analizi][ta-containers-language] | F0, S | **Dil algılama** ([görüntü](https://go.microsoft.com/fwlink/?linkid=2018759&clcid=0x409)) | 120 dilde dile kadar, giriş metninin hangi dilde yazıldığını algılar ve istekte gönderilen her belge için tek bir dil kodu bildirir. Dil kodu, puanın ağırlığını belirten bir puanla eşleştirilir. |
| [Metin Analizi][ta-containers-sentiment] | F0, S | **Yaklaşım Analizi v3** ([görüntü](https://go.microsoft.com/fwlink/?linkid=2018654&clcid=0x409)) | Olumlu veya negatif yaklaşım hakkında ipuçları için ham metni analiz eder. Bu yaklaşım analizinin bu sürümü, her belge ve tümce için yaklaşım etiketlerini (örneğin *pozitif* veya *negatif*) döndürür. |
| [Metin Analizi][ta-containers-health] | F0, S | **Sistem durumu için Metin Analizi** | Yapılandırılmamış klinik metinden tıp bilgilerini ayıklayın. |

<!--
|[Personalizer](https://go.microsoft.com/fwlink/?linkid=2083923&clcid=0x409) |F0, S0|**Personalizer** ([image](https://go.microsoft.com/fwlink/?linkid=2083928&clcid=0x409))|Azure Personalizer is a cloud-based API service that allows you to choose the best experience to show to your users, learning from their real-time behavior.|
-->

Ayrıca, bazı kapsayıcılar bilişsel Hizmetler [**hepsi bir arada sunum**](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesAllInOne) kaynak anahtarları içinde desteklenir. Tek bir bilişsel hizmetler oluşturup bir adet tek kaynak oluşturabilir ve aşağıdaki hizmetler için desteklenen hizmetler genelinde aynı faturalandırma anahtarını kullanabilirsiniz:

* Görüntü İşleme
* Yüz Tanıma
* LUIS
* Metin Analizi

## <a name="container-availability-in-azure-cognitive-services"></a>Azure bilişsel hizmetler 'de kapsayıcı kullanılabilirliği

Azure bilişsel hizmetler kapsayıcıları Azure aboneliğiniz aracılığıyla herkese açık ve Docker kapsayıcı görüntüleri Microsoft Container Registry ya da Docker Hub 'ından alınabilir. Uygun kayıt defterinden bir kapsayıcı görüntüsünü indirmek için [Docker Pull](https://docs.docker.com/engine/reference/commandline/pull/) komutunu kullanabilirsiniz.

[!INCLUDE [Container repositories and images](containers/includes/cognitive-services-container-images.md)]

## <a name="prerequisites"></a>Önkoşullar

Azure bilişsel hizmetler kapsayıcılarını kullanmadan önce aşağıdaki önkoşulları karşılamanız gerekir:

**Docker altyapısı**: Docker altyapısının yerel olarak yüklü olması gerekir. Docker, [MacOS](https://docs.docker.com/docker-for-mac/), [Linux](https://docs.docker.com/engine/installation/#supported-platforms)ve [Windows](https://docs.docker.com/docker-for-windows/)üzerinde Docker ortamını yapılandıran paketler sağlar. Windows 'da, Docker 'ın Linux kapsayıcılarını destekleyecek şekilde yapılandırılması gerekir. Docker Kapsayıcıları ayrıca doğrudan [Azure Kubernetes hizmetine](../aks/index.yml) veya [Azure Container Instances](../container-instances/index.yml)dağıtılabilir.

Kapsayıcıların Azure 'a bağlanıp faturalandırma verilerini göndermesini sağlamak için Docker yapılandırılmalıdır.

**Microsoft Container Registry ve Docker Ile benzerlik**: kayıt defterleri, depolar, kapsayıcılar ve kapsayıcı görüntüleri gibi Microsoft Container Registry ve Docker kavramlarının yanı sıra temel komutlar hakkında bilgi sahibi olmanız gerekir `docker` .

Docker ve kapsayıcı temel bilgileri ile ilgili giriş yapmak için [Docker’a genel bakış](https://docs.docker.com/engine/docker-overview/) bölümüne bakın.

Tek tek kapsayıcılar, sunucu ve bellek ayırma gereksinimleri dahil olmak üzere kendi gereksinimlerine sahip olabilir.

[!INCLUDE [Cognitive Services container security](containers/includes/cognitive-services-container-security.md)]

[!INCLUDE [Discoverability of more container information](../../includes/cognitive-services-containers-discoverability.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bilişsel hizmetler ile kullanabileceğiniz [kapsayıcı Tarifler](containers/container-reuse-recipe.md) hakkında bilgi edinin.

Azure bilişsel hizmetler 'de kapsayıcılar tarafından sunulan işlevleri yükleyip araştırın:

* [Anomali algılayıcı kapsayıcıları][ad-containers]
* [Görüntü İşleme kapsayıcılar][cv-containers]
* [Yüz kapsayıcıları][fa-containers]
* [Form tanıyıcı kapsayıcıları][fr-containers]
* [Language Understanding (LUSıS) kapsayıcıları][lu-containers]
* [Konuşma hizmeti API kapsayıcıları][sp-containers]
* [Metin Analizi kapsayıcılar][ta-containers]

<!--* [Personalizer containers](https://go.microsoft.com/fwlink/?linkid=2083928&clcid=0x409)
-->

[ad-containers]: anomaly-Detector/anomaly-detector-container-howto.md
[cv-containers]: computer-vision/computer-vision-how-to-install-containers.md
[fa-containers]: face/face-how-to-install-containers.md
[fr-containers]: form-recognizer/form-recognizer-container-howto.md
[lu-containers]: luis/luis-container-howto.md
[sp-containers]: speech-service/speech-container-howto.md
[sp-containers-stt]: speech-service/speech-container-howto.md?tabs=stt
[sp-containers-cstt]: speech-service/speech-container-howto.md?tabs=cstt
[sp-containers-tts]: speech-service/speech-container-howto.md?tabs=tts
[sp-containers-ctts]: speech-service/speech-container-howto.md?tabs=ctts
[ta-containers]: text-analytics/how-tos/text-analytics-how-to-install-containers.md
[ta-containers-keyphrase]: text-analytics/how-tos/text-analytics-how-to-install-containers.md?tabs=keyphrase
[ta-containers-language]: text-analytics/how-tos/text-analytics-how-to-install-containers.md?tabs=language
[ta-containers-sentiment]: text-analytics/how-tos/text-analytics-how-to-install-containers.md?tabs=sentiment
[ta-containers-health]: text-analytics/how-tos/text-analytics-how-to-install-containers.md?tabs=health
[request-access]: https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRyQZ7B8Cg2FEjpibPziwPcZUNlQ4SEVORFVLTjlBSzNLRlo0UzRRVVNPVy4u