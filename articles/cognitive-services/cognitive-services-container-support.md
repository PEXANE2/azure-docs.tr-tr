---
title: Kapsayıcı desteği
titleSuffix: Azure Cognitive Services
description: Docker kapsayıcıların Bilişsel Hizmetleri verilerinize nasıl yaklaştırabileceğini öğrenin.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.topic: article
ms.date: 01/10/2020
ms.author: dapine
ms.openlocfilehash: d75962b98543991a065f6b165279215614175925
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79219417"
---
# <a name="container-support-in-azure-cognitive-services"></a>Azure Bilişsel Hizmetlerde konteyner desteği

Azure Bilişsel Hizmetler'deki kapsayıcı desteği, geliştiricilerin Azure'da kullanılabilen aynı zengin API'leri kullanmasına olanak tanır ve [Docker kapsayıcılarıyla](https://www.docker.com/what-container)birlikte gelen hizmetleri nerede dağıtıp barındıracakları konusunda esneklik sağlar. Konteyner desteği şu anda Azure Bilişsel Hizmetlerinin bir alt kümesi için kullanılabilir:

> [!div class="checklist"]
> * [Anomali Algılayıcısı][ad-containers]
> * [BilgisayarLı Vizyon][cv-containers]
> * [Yüz][fa-containers]
> * [Form Tanıma][fr-containers]
> * [Dil Anlama (LUIS)][lu-containers]
> * [Konuşma Hizmeti API’si][sp-containers]
> * [Metin Analizi][ta-containers]

> [!VIDEO https://www.youtube.com/embed/hdfbn4Q8jbo]

Kapsayıcılaştırma, bir uygulamanın veya hizmetin bağımlılıkları & yapılandırması da dahil olmak üzere kapsayıcı görüntüsü olarak birlikte paketlendiği yazılım dağıtımına yönelik bir yaklaşımdır. Çok az değişiklik veya hiç değişiklik olmadan, bir kapsayıcı görüntüsü bir kapsayıcı ana bilgisayara dağıtılabilir. Kapsayıcılar birbirinden ve altta yatan işletim sisteminden sanal bir makineden daha küçük bir ayak izine sahip izole edilmiştir. Kapsayıcılar kısa süreli görevler için kapsayıcı görüntülerinden anında çıkarılabilir ve artık gerekmediğinde kaldırılabilir.

Bilişsel Hizmetler kaynakları [Microsoft Azure'da](https://azure.microsoft.com)kullanılabilir. Bu hizmetler için Azure kaynakları oluşturmak ve keşfetmek için [Azure portalında](https://portal.azure.com/) oturum açın.

## <a name="features-and-benefits"></a>Özellikler ve avantajlar

- **Değişmez altyapı**: DevOps ekiplerinin bilinen sistem parametrelerinin tutarlı ve güvenilir bir kümesinden yararlanmasını sağlarken, değişime uyum sağlayabilmelerini sağlar. Kapsayıcılar, öngörülebilir bir ekosistem içinde dönme ve yapılandırma sürüklenmesini önleme esnekliği sağlar.
- **Veri denetimi**: Müşterilerin bu Bilişsel Hizmetlerin verilerini nerede işletireceğini seçmelerine izin verin. Bu, buluta veri gönderemeyen ancak Bilişsel Hizmetler teknolojisine erişmeye ihtiyaç duyan müşteriler için gereklidir. Veri, yönetim, kimlik ve güvenlik genelinde karma ortamlarda tutarlılığı destekleyin.
- **Model güncelleştirmeleri üzerinde denetim**: Müşterilere çözümlerinde dağıtılan modellerin sürümlenmesi ve güncellenmesinde esneklik sağlayın.
- **Taşınabilir mimari**: Azure'da, şirket içinde ve kenarda dağıtılabilen taşınabilir bir uygulama mimarisioluşturulmasını etkinleştirin. Kapsayıcılar doğrudan Azure [Kubernetes Hizmetine](../aks/index.yml), [Azure Kapsayıcı Örneklerine](../container-instances/index.yml)veya [Azure Yığını'na](/azure-stack/operator)dağıtılan bir [Kubernetes](https://kubernetes.io/) kümesine dağıtılabilir. Daha fazla bilgi için bkz: [Kubernetes'i Azure Yığınına Dağıt.](/azure-stack/user/azure-stack-solution-template-kubernetes-deploy)
- **Yüksek iş sonu / düşük gecikme süresi**: Bilişsel Hizmetlerin uygulama mantığına ve verilerine fiziksel olarak yakın çalışmasını sağlayarak müşterilere yüksek iş ve düşük gecikme süresi gereksinimlerini ölçeklendirme olanağı sağlar. Kapsayıcılar hareketleri saniyede (TPS) kapatmaz ve gerekli donanım kaynaklarını sağlarsanız talebi işlemek için hem yukarı hem de dışarı ölçeklendirmek için yapılabilir.
- **Ölçeklenebilirlik**: Kubernetes gibi konteynerizasyon ve konteyner orkestrasyon yazılımının giderek artan popülaritesi ile; ölçeklenebilirlik teknolojik gelişmelerin ön planda yer almaktadır. Ölçeklenebilir küme temeli üzerine inşa edilebilen uygulama geliştirme, yüksek kullanılabilirlik sunar.

## <a name="containers-in-azure-cognitive-services"></a>Azure Bilişsel Hizmetlerde Kapsayıcılar

Azure Bilişsel Hizmetler kapsayıcıları, her biri Azure Bilişsel Hizmetler'deki hizmetlerden bir işlev alt kümesi içeren aşağıdaki Docker kapsayıcıkümesini sağlar:

| Hizmet | Desteklenen Fiyatlandırma Katmanı | Kapsayıcı | Açıklama |
|---------|----------|----------|-------------|
|[Anomali dedektörü][ad-containers] |F0, S0|**Anomali-Dedektör** |Anomali Dedektörü API'sı, zaman serisi verilerinizdeki anormallikleri makine öğrenimi ile izlemenizi ve algılamanızı sağlar.<br>[Erişim izni isteme](https://aka.ms/adcontainer)|
|[BilgisayarLı Vizyon][cv-containers] |F0, S1|**Okuma** |Makbuzlar, posterler ve kartvizitler gibi farklı yüzeylere ve arka planlara sahip çeşitli nesnelerin görüntülerinden yazdırılan metni ayıklar. Read kapsayıcısı ayrıca resimlerdeki *el yazısı metni* algılar ve PDF/TIFF/çok sayfalı destek sağlar.<br/><br/>**Önemli:** Read kapsayıcısı şu anda yalnızca İngilizce ile çalışır.|
|[Yüz][fa-containers] |F0, S0|**Yüz** |Görüntülerdeki insan yüzlerini algılar ve yüz işaretleri (burun ve gözler gibi), cinsiyet, yaş ve makine tarafından öngörülen diğer yüz özellikleri gibi özellikleri tanımlar. Algılamaya ek olarak, Face aynı görüntüdeki iki yüzün veya farklı görüntülerin bir güven puanı kullanarak aynı olup olmadığını denetleyebilir veya benzer veya aynı yüzün zaten var olup olmadığını görmek için yüzleri veritabanıyla karşılaştırabilir. Ayrıca, paylaşılan görsel özellikleri kullanarak benzer yüzleri gruplar halinde düzenleyebilir.<br>[Erişim izni isteme](Face/face-how-to-install-containers.md#request-access-to-the-private-container-registry) |
|[Form tanıyıcı][fr-containers] |F0, S0|**Form Tanıma** |Form Anlama, anahtar değeri çiftleri ve tabloları formlardan tanımlamak ve ayıklamak için makine öğrenimi teknolojisini uygular.<br>[Erişim izni isteme](https://aka.ms/FormRecognizerContainerRequestAccess)|
|[LUIS][lu-containers] |F0, S0|**LUIS** ([resim](https://go.microsoft.com/fwlink/?linkid=2043204&clcid=0x409))|Luis uygulaması olarak da bilinen eğitimli veya yayımlanmış bir Dil Bilgisi modelini bir docker konteynerine yükler ve kapsayıcının API uç noktalarından sorgu tahminlerine erişim sağlar. Kapsayıcıdan sorgu günlükleri toplayabilir ve uygulamanın tahmin doğruluğunu artırmak için bunları [LUIS portalına](https://www.luis.ai) geri yükleyebilirsiniz.|
|[Konuşma Hizmeti API’si][sp-containers-stt] |F0, S0|**Konuşmayı metne dönüştürme** |Sürekli, gerçek zamanlı konuşmaları metne dönüştürür.|
|[Konuşma Hizmeti API’si][sp-containers-cstt] |F0, S0|**Özel Konuşma-metin** |Sürekli gerçek zamanlı konuşmayı özel bir model kullanarak metne dönüştürür.|
|[Konuşma Hizmeti API’si][sp-containers-tts] |F0, S0|**Metin okuma** |Metni, doğal sesli konuşmaya dönüştürür.|
|[Konuşma Hizmeti API’si][sp-containers-ctts] |F0, S0|**Özel Metinden konuşmaya** |Özel bir model kullanarak metni doğal görünen konuşmaya dönüştürür.|
|[Metin Analizi][ta-containers-keyphrase] |F0, S|**Anahtar Tümceme Çıkarma** ([resim](https://go.microsoft.com/fwlink/?linkid=2018757&clcid=0x409)) |Ana noktaları tanımlamak için anahtar tümcecikleri ayıklar. Örneğin, "The food was delicious and there were wonderful staff" (Yemekler lezzetliydi ve personel harikaydı) giriş metni olduğunda API, "food" (yemek) ve "wonderful staff" (personel harikaydı) ana konuşma noktalarını döndürür. |
|[Metin Analizi][ta-containers-language]|F0, S|**Dil Algılama** ([resim](https://go.microsoft.com/fwlink/?linkid=2018759&clcid=0x409)) |120'ye kadar dil için, giriş metninin hangi dilde yazıldığını algılar ve istek üzerine gönderilen her belge için tek bir dil kodu bildirir. Dil kodu, puanın ağırlığını belirten bir puanla eşleştirilir. |
|[Metin Analizi][ta-containers-sentiment]|F0, S|**Duygu Analizi** ([resim](https://go.microsoft.com/fwlink/?linkid=2018654&clcid=0x409)) |Olumlu veya olumsuz duygular hakkında ipuçları için ham metni analiz eder. API, her belge için 0 ile 1 arasında bir yaklaşım puanı döndürür ve 1 en pozitif değerdir. Analiz modelleri, Microsoft'un kapsamlı metin ve doğal dil teknolojileri gövdesi kullanılarak önceden eğitilmiştir. API, [seçili dillerde](./text-analytics/language-support.md) sağladığınız ham metni analiz edip puanlayabilir ve sonuçları doğrudan çağrıyı yapan uygulamaya döndürebilir. |

<!--
|[Personalizer](https://go.microsoft.com/fwlink/?linkid=2083923&clcid=0x409) |F0, S0|**Personalizer** ([image](https://go.microsoft.com/fwlink/?linkid=2083928&clcid=0x409))|Azure Personalizer is a cloud-based API service that allows you to choose the best experience to show to your users, learning from their real-time behavior.|
-->

Buna ek olarak, bazı kapsayıcılar Bilişsel Hizmetler [**All-In-One sunan**](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesAllInOne) kaynak anahtarları desteklenir. Tek bir Bilişsel Hizmetler Hepsi Bir Kaynak oluşturabilir ve aşağıdaki hizmetler için desteklenen hizmetler arasında aynı faturalandırma anahtarını kullanabilirsiniz:

* Görüntü İşleme
* Yüz
* LUIS
* Metin Analizi

## <a name="container-availability-in-azure-cognitive-services"></a>Azure Bilişsel Hizmetler'de konteyner kullanılabilirliği

Azure Bilişsel Hizmetler kapsayıcıları Azure aboneliğiniz aracılığıyla herkese açıktır ve Docker kapsayıcı görüntüleri Microsoft Konteyner Kayıt Defteri'nden veya Docker Hub'ından çekilebilir. Uygun kayıt defterinden bir konteyner görüntüsü indirmek için [docker çekme](https://docs.docker.com/engine/reference/commandline/pull/) komutunu kullanabilirsiniz.

> [!IMPORTANT]
> Şu anda, aşağıdaki kapsayıcılara erişmek için bir kayıt işlemini tamamlamanız ve sizinle, şirketinizle ve kapsayıcıları uygulamak istediğiniz kullanım örneği yle ilgili sorular içeren bir soru formu doldurmanız ve gönderdiğiniz bir kayıt işlemini tamamlamanız gerekir. Erişim izni verildikten ve kimlik bilgileri sağlandıktan sonra, Azure Konteyner Kayıt Defteri tarafından barındırılan özel bir kapsayıcı kayıt defterinden kapsayıcı görüntülerini çekebilirsiniz.
> * [Anomali dedektörü](Anomaly-Detector/anomaly-detector-container-howto.md#request-access-to-the-container-registry)
> * [Yüz](Face/face-how-to-install-containers.md)
> * [Form Tanıma](form-recognizer/form-recognizer-container-howto.md#request-access-to-the-container-registry)
> * [Okuma](computer-vision/computer-vision-how-to-install-containers.md)
> * [Konuşmadan metne ve Metinden konuşmaya](Speech-Service/speech-container-howto.md#request-access-to-the-container-registry)

[!INCLUDE [Container repositories and images](containers/includes/cognitive-services-container-images.md)]

## <a name="prerequisites"></a>Ön koşullar

Azure Bilişsel Hizmetler kapsayıcılarını kullanmadan önce aşağıdaki ön koşulları karşılamanız gerekir:

**Docker Engine**: Docker Engine'i yerel olarak kurmuş olmalısınız. Docker, [macOS,](https://docs.docker.com/docker-for-mac/) [Linux](https://docs.docker.com/engine/installation/#supported-platforms)ve [Windows'da](https://docs.docker.com/docker-for-windows/)Docker ortamını yapılandıran paketler sağlar. Windows'da Docker, Linux kapsayıcılarını destekleyecek şekilde yapılandırılmalıdır. Docker kapsayıcıları doğrudan Azure [Kubernetes Hizmeti'ne](../aks/index.yml) veya [Azure Kapsayıcı Örnekleri'ne](../container-instances/index.yml)de dağıtılabilir.

Docker, kapsayıcıların Azure'a bağlanmasına ve fatura verilerini göndermesine izin verecek şekilde yapılandırılmalıdır.

**Microsoft Konteyner Kayıt Defteri ve Docker ile aşinalık**: Hem Microsoft Konteyner Kayıt Defteri hem de Docker kavramları,kayıt defterleri, depolar, `docker` kapsayıcılar ve kapsayıcı görüntüleri gibi temel bilgilere ve temel komutlar hakkında bilgi sahibi olmalısınız.

Docker ve kapsayıcı temel bilgileri ile ilgili giriş yapmak için [Docker’a genel bakış](https://docs.docker.com/engine/docker-overview/) bölümüne bakın.

Tek tek kapsayıcıların sunucu ve bellek ayırma gereksinimleri de dahil olmak üzere kendi gereksinimleri olabilir.

[!INCLUDE [Cognitive Services container security](containers/includes/cognitive-services-container-security.md)]

[!INCLUDE [Discoverability of more container information](../../includes/cognitive-services-containers-discoverability.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bilişsel Hizmetler ile kullanabileceğiniz [konteyner tarifleri](containers/container-reuse-recipe.md) hakkında bilgi edinin.

Azure Bilişsel Hizmetleri'nde kapsayıcılar tarafından sağlanan işlevselliği yükleyin ve keşfedin:

* [Anomali Dedektörü konteynerleri][ad-containers]
* [Bilgisayarlı Vizyon kapları][cv-containers]
* [Yüz kapları][fa-containers]
* [Form Tanıyıcı kapları][fr-containers]
* [Dil Anlama (LUIS) kapları][lu-containers]
* [Konuşma Hizmeti API kapları][sp-containers]
* [Metin Analizi kapları][ta-containers]

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