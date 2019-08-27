---
title: Kapsayıcı desteği
titleSuffix: Azure Cognitive Services
description: Docker kapsayıcıları yakın Bilişsel hizmetler verilerinize nasıl edinebildiğini öğrenin.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.topic: article
ms.date: 8/21/2019
ms.author: dapine
ms.openlocfilehash: c12e64fc51eb53aebffbef1ad886b7cc404cf699
ms.sourcegitcommit: bba811bd615077dc0610c7435e4513b184fbed19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2019
ms.locfileid: "70051228"
---
# <a name="container-support-in-azure-cognitive-services"></a>Azure Bilişsel hizmetler kapsayıcı desteği

Azure Bilişsel hizmetler kapsayıcı desteği sayesinde geliştiriciler, Azure'da kullanılabilen zengin API'leri kullanmak için ve nerede dağıtmak ve gelir hizmetlerini barındırmak esneklik sağlar [Docker kapsayıcıları](https://www.docker.com/what-container). Kapsayıcı desteği şu anda Azure bilişsel hizmetler 'in şu parçalar dahil olmak üzere bir alt kümesi için önizleme aşamasında sunulmaktadır:

* [Anomali algılayıcısı][ad-containers]
* [Görüntü İşleme][cv-containers]
* [Yüz tanıma][fa-containers]
* [Form tanıyıcı][fr-containers]
* [Language Understanding (LUIS)][lu-containers]
* [Konuşma Hizmeti API’si][sp-containers]
* [Metin Analizi][ta-containers]

> [!VIDEO https://www.youtube.com/embed/hdfbn4Q8jbo]

Kapsayıcı içinde bir uygulama veya onun bağımlılıklarını & yapılandırması gibi hizmet birlikte bir kapsayıcı görüntüsüne paketlenmiştir yazılım dağıtımı için bir yaklaşımdır. Çok az kayıpla veya hiç değişiklik yapmadan kapsayıcı görüntüsü, bir kapsayıcı konağı üzerinde dağıtılabilir. Birbirine ve bir sanal makine değerinden daha küçük bir kaplama alanı ile temel işletim sistemi, yalıtılmış kapsayıcılardır. Kapsayıcılar için kısa vadeli görevleri kapsayıcı görüntülerinden oluşturulan ve artık gerekli olmadığında kaldırıldı.

Bilişsel hizmetler kaynakları [Microsoft Azure](https://azure.microsoft.com)kullanılabilir. Oturum [Azure portalında](https://portal.azure.com/) oluşturma ve bu hizmetler için Azure kaynaklarını keşfedin.

## <a name="features-and-benefits"></a>Özellikler ve avantajlar

- **Veriler üzerinde denetim**: Müşterilerin bu bilişsel hizmetler 'in verilerini nerede işleyeceğini seçmesine izin verin. Bu, verileri buluta Gönder olamaz, ancak Bilişsel hizmetler teknoloji erişmesi gereken müşteriler için gereklidir. Karma ortamlarda, veriler, yönetim, kimlik ve güvenlik arasında tutarlılık desteği.
- **Model güncelleştirmeleri üzerinde denetim**: Kullanıcılara, çözümlerinde dağıtılan modellerin sürümü oluşturma ve güncelleştirme esnekliği sağlar.
- **Taşınabilir mimari**: Azure 'da, şirket içinde ve kenarda dağıtılabilecek taşınabilir bir uygulama mimarisi oluşturmayı etkinleştirin. Kapsayıcıları doğrudan dağıtılabilir [Azure Kubernetes hizmeti](../aks/index.yml), [Azure Container Instances](../container-instances/index.yml), veya bir [Kubernetes](https://kubernetes.io/) kümesi dağıtıldı için [Azure Yığın](/azure-stack/operator). Daha fazla bilgi için [Azure Stack dağıtma Kubernetes](/azure-stack/user/azure-stack-solution-template-kubernetes-deploy).
- **Yüksek aktarım hızı/düşük gecikme süresi**: Bilişsel hizmetler 'in uygulama mantığına ve verilerine fiziksel olarak yakın çalışmasını sağlayarak müşterilere yüksek aktarım hızı ve düşük gecikme süreli gereksinimleri ölçeklendirme olanağı sağlayın. Kapsayıcılar, saniye başına işlem (TPS) cap değil ve gerekli donanım kaynakları sağlarsanız, isteğe bağlı işlemek için yukarı ve dışarı ölçeklendirme yapılabilir. 

## <a name="containers-in-azure-cognitive-services"></a>Azure Bilişsel hizmetler, kapsayıcılar

Azure Bilişsel hizmetler kapsayıcılar, Docker kapsayıcıları, aşağıdaki dizi her biri, Azure Bilişsel hizmetler hizmetlerden işlevlerinin bir alt kümesini içeren sağlar:

| Hizmet | Desteklenen Fiyatlandırma Katmanı | Kapsayıcı | Açıklama |
|---------|----------|----------|-------------|
|[Anomali algılayıcısı][ad-containers] |F0, S0|**Anomali-algılayıcı** |Anomali algılayıcı API 'SI, makine öğrenimi ile zaman serisi verilerinizde bulunan normalleştirikleri izlemenize ve algılamanıza olanak sağlar.<br>[Erişim isteği](https://aka.ms/adcontainer)|
|[Görüntü İşleme][cv-containers] |F0, S1|**Metin tanıma** |Farklı yüzey ve arka planlar, giriş ve posterler kartvizitler gibi çeşitli nesne görüntülerdeki yazdırılan metin ayıklar.<br/><br/>**Önemli:** Metni Tanı kapsayıcı şu anda yalnızca İngilizce ile çalışır.<br>[Erişim isteği](Computer-vision/computer-vision-how-to-install-containers.md#request-access-to-the-private-container-registry)|
|[Yüz tanıma][fa-containers] |F0, S0|**Yüz tanıma** |Görüntülerdeki İnsan yüzlerini algılar ve yüz yer işareti (örneğin, noses ve gözler), cinsiyet, geçerlilik süresi ve diğer makine tahmin yüz özellikleri dahil olmak üzere, öznitelikleri tanımlar. Yüz algılama ek olarak, iki yüzün aynı görüntü ya da farklı görüntüleri bir güven puanı kullanarak aynı olduğundan veya bir benzeyen olmadığını görmek için bir veritabanında yüzleri karşılaştırın veya aynı yüz zaten kontrol edebilirsiniz. Bu gibi durumlarda, benzer yüzlerden de paylaşılan visual nitelikler kullanarak gruplar halinde düzenleyebilirsiniz.<br>[Erişim isteği](Face/face-how-to-install-containers.md#request-access-to-the-private-container-registry) |
|[Form tanıyıcı][fr-containers] |F0, S0|**Form tanıyıcı** |Form, formlardan anahtar-değer çiftlerini ve tabloları tanımlamak ve ayıklamak için makine öğrenimi teknolojisini uygular.<br>[Erişim isteği](https://aka.ms/FormRecognizerContainerRequestAccess)|
|[LUIS][lu-containers] |F0, S0|**LUIS** ([görüntü](https://go.microsoft.com/fwlink/?linkid=2043204&clcid=0x409))|Bir eğitilen veya yayımlanmış dil anlama modeli olarak da bilinen bir LUIS uygulaması bir docker kapsayıcısına yükler ve kapsayıcının API uç noktalardan gelen sorgu tahminler elde etmek için erişim sağlar. Kapsayıcıdan sorgu günlüklerini toplamak ve bu geri yükleme [LUIS portalı](https://www.luis.ai) uygulamanın tahmin doğruluğunu artırmak için.|
|[Konuşma Hizmeti API’si][sp-containers] |F0, S0|**Konuşmayı metne dönüştürme** |Sürekli, gerçek zamanlı konuşmaları metne dönüştürür.<br>[Erişim isteği](https://aka.ms/speechcontainerspreview/)|
|[Konuşma Hizmeti API’si][sp-containers] |F0, S0|**Metin okuma** |Metni, doğal sesli konuşmaya dönüştürür.<br>[Erişim isteği](https://aka.ms/speechcontainerspreview/)|
|[Metin Analizi][ta-containers] |F0, S|**Anahtar ifade ayıklama** ([görüntü](https://go.microsoft.com/fwlink/?linkid=2018757&clcid=0x409)) |Ana noktaları belirleyin, anahtar ifadeleri ayıklar. Örneğin, "The food was delicious and there were wonderful staff" (Yemekler lezzetliydi ve personel harikaydı) giriş metni olduğunda API, "food" (yemek) ve "wonderful staff" (personel harikaydı) ana konuşma noktalarını döndürür. |
|[Metin Analizi][ta-containers]|F0, S|**Dil algılama** ([görüntü](https://go.microsoft.com/fwlink/?linkid=2018759&clcid=0x409)) |En fazla 120 dil için hangi dil giriş metni yazılır ve rapor istekte gönderilen her belge için bir tek dil kodu algılar. Dil kodu, puanın ağırlığını belirten bir puanla eşleştirilir. |
|[Metin Analizi][ta-containers]|F0, S|**Yaklaşım analizi** ([görüntü](https://go.microsoft.com/fwlink/?linkid=2018654&clcid=0x409)) |Ham metin pozitif veya negatif yaklaşım hakkında ipuçları için analiz eder. API, her belge için 0 ile 1 arasında bir yaklaşım puanı döndürür ve 1 en pozitif değerdir. Analiz modelleri metin ve doğal dil Microsoft teknolojilerinin kapsamlı bir gövdesi kullanarak önceden eğitilir. API, [seçili dillerde](./text-analytics/language-support.md) sağladığınız ham metni analiz edip puanlayabilir ve sonuçları doğrudan çağrıyı yapan uygulamaya döndürebilir. |

<!--
|[Personalizer](https://go.microsoft.com/fwlink/?linkid=2083923&clcid=0x409) |F0, S0|**Personalizer** ([image](https://go.microsoft.com/fwlink/?linkid=2083928&clcid=0x409))|Azure Personalizer is a cloud-based API service that allows you to choose the best experience to show to your users, learning from their real-time behavior.|
-->

Ayrıca, bazı kapsayıcılar bilişsel Hizmetler [**hepsi bir arada sunum**](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesAllInOne) kaynak anahtarları içinde desteklenir. Tek bir bilişsel hizmetler oluşturup bir adet tek kaynak oluşturabilir ve aşağıdaki hizmetler için desteklenen hizmetler genelinde aynı faturalandırma anahtarını kullanabilirsiniz:

* Görüntü İşleme
* Yüz
* LUIS
* Metin Analizi

## <a name="container-availability-in-azure-cognitive-services"></a>Azure Bilişsel hizmetler kapsayıcı kullanılabilirlik

Azure aboneliğiniz üzerinden genel kullanıma açık Azure Bilişsel hizmetler kapsayıcıları ve Docker kapsayıcı görüntülerini Microsoft kapsayıcı kayıt defteri veya Docker Hub çekilebilir. Kullanabileceğiniz [docker isteği](https://docs.docker.com/engine/reference/commandline/pull/) uygun kayıt defterinden bir kapsayıcı görüntüsü indirilemedi komutu.

> [!IMPORTANT]
> Şu anda, aşağıdaki kapsayıcılara erişmek için bir kaydolma işlemini doldurmanız gerekir. burada, sizin, şirketiniz ve kapsayıcıları uygulamak istediğiniz kullanım durumu ile ilgili soruları içeren bir soru formunu doldurun ve gönderebilirsiniz. Erişim izni verildiğinde ve kimlik bilgilerini sağladıktan sonra, Azure Container Registry tarafından barındırılan özel bir kapsayıcı kayıt defterinden kapsayıcı görüntülerini çekebilirsiniz.
> * [Anomali algılayıcısı](Anomaly-Detector/anomaly-detector-container-howto.md#request-access-to-the-container-registry)
> * [Yüz tanıma](Face/face-how-to-install-containers.md)
> * [Form tanıyıcı](form-recognizer/form-recognizer-container-howto.md#request-access-to-the-container-registry)
> * [Metin tanıma](Computer-vision/computer-vision-how-to-install-containers.md)
> * [Konuşmadan metne ve metinden konuşmaya](Speech-Service/speech-container-howto.md#request-access-to-the-container-registry)

[!INCLUDE [Container repositories and images](containers/includes/cognitive-services-container-images.md)]

## <a name="prerequisites"></a>Önkoşullar

Azure Bilişsel hizmetler kapsayıcıları kullanmadan önce aşağıdaki önkoşulları karşılamanız gerekir:

**Docker altyapısı**: Docker altyapısının yerel olarak yüklü olması gerekir. Docker üzerinde Docker ortamını yapılandıran paketler sağlar [macOS](https://docs.docker.com/docker-for-mac/), [Linux](https://docs.docker.com/engine/installation/#supported-platforms), ve [Windows](https://docs.docker.com/docker-for-windows/). Windows Docker Linux kapsayıcıları destekleyecek şekilde yapılandırılması gerekir. Docker kapsayıcıları da dağıtılabilir doğrudan [Azure Kubernetes hizmeti](../aks/index.yml) veya [Azure Container Instances](../container-instances/index.yml).

Docker, kapsayıcılar ile bağlanma ve faturalama verileri Azure'a göndermek izin verecek şekilde yapılandırılmalıdır.

**Microsoft Container Registry ve Docker Ile benzerlik**: Hem Microsoft Container Registry hem de Docker kavramlarını, kayıt defterleri, depolar, kapsayıcılar ve kapsayıcı görüntüleri ve temel `docker` komutlar hakkında bilgi sahibi olmanız gerekir.

Docker ve kapsayıcı temelleri hakkında bilgi için bkz: [Docker'a genel bakış](https://docs.docker.com/engine/docker-overview/).

Kapsayıcılara sunucu ve bellek ayırma gereksinimleri dahil olmak üzere kendi gereksinimleriyle de sahip olabilir.

[!INCLUDE [Discoverability of more container information](../../includes/cognitive-services-containers-discoverability.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bilişsel hizmetler ile kullanabileceğiniz [kapsayıcı Tarifler](containers/container-reuse-recipe.md) hakkında bilgi edinin.

Yükleme ve Azure Bilişsel hizmetler, kapsayıcılar tarafından sağlanan işlevselliği keşfedin:

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
[ta-containers]: text-analytics/how-tos/text-analytics-how-to-install-containers.md