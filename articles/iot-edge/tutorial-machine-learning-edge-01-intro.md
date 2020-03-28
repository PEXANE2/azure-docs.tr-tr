---
title: "Öğretici: Azure IoT Edge'de Makine Öğreniminin ayrıntılı walkthrough"
description: Uçtan uca, kenar senaryosunda makine öğrenimi oluşturmak için gereken çeşitli görevlerarasında yürüyen üst düzey bir öğretici.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 11/11/2019
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 965c420fa29c4cf82517148c01e17d6d7dd6ea97
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "74106499"
---
# <a name="tutorial-an-end-to-end-solution-using-azure-machine-learning-and-iot-edge"></a>Öğretici: Azure Machine Learning ve IoT Edge kullanarak uçtan uca çözüm

Sık sık, IoT uygulamaları akıllı bulut ve akıllı kenar yararlanmak istiyorum. Bu eğitimde, buluttaki IoT aygıtlarından toplanan verilerle bir makine öğrenimi modelini eğitmeniz, bu modeli IoT Edge'e dağıtmanız ve modeli periyodik olarak korumanız ve rafine etmede size yol görüyoruz.

Bu öğreticinin birincil amacı, ioT verilerinin işlenmesini makine öğrenimi ile, özellikle kenarda tanıtmaktır. Genel bir makine öğrenimi iş akışının birçok yönüne değinsek de, bu öğretici makine öğrenimine derinlemesine bir giriş olarak tasarlanmamıştır. Bir örnek olarak, kullanım örneği için son derece optimize edilmiş bir model oluşturmaya çalışmıyoruz – ioT veri işleme için uygun bir model oluşturma ve kullanma sürecini göstermek için yeterli çabayı gösteriyoruz.

## <a name="target-audience-and-roles"></a>Hedef kitle ve roller

Bu makale kümesi, IoT geliştirme veya makine öğrenimi deneyimi olmayan geliştiriciler için tasarlanmıştır. Makine öğreniminin sınırda dağıtılması, çok çeşitli teknolojilerin nasıl bağlanılabildiği hakkında bilgi gerektirir. Bu nedenle, bu öğretici, bir IoT çözümü için bu teknolojileri bir araya etmenin bir yolunu göstermek için uçtan uca bir senaryoyu kapsar. Gerçek bir ortamda, bu görevler farklı uzmanlıkları olan birkaç kişi arasında dağıtılabilir. Örneğin, geliştiriciler cihaz veya bulut koduna odaklanırken, veri bilimciler analiz modellerini tasarlarlar. Tek bir geliştiricinin bu öğreticiyi başarıyla tamamlamasını sağlamak için, neyin yapıldığını ve neden yapıldığını anlamak için yeterli olduğunu umduğumuz daha fazla bilgiye ilişkin öngörüler ve bağlantılar içeren tamamlayıcı rehberlik sağladık.

Alternatif olarak, öğreticiyi birlikte takip etmek, tüm uzmanlığınızı taşıyarak farklı rollerdeki iş arkadaşlarınızla bir araya gelebilir ve takım olarak işlerin birbirine nasıl uyduğunu öğrenebilirsiniz.

Her iki durumda da, okuyucu(lar) yönlendirmek yardımcı olmak için, bu öğretici her makale kullanıcının rolünü gösterir. Bu roller şunlardır:

* Bulut geliştirme (DevOps kapasitesinde çalışan bir bulut geliştiricisi dahil)
* Veri analizi

## <a name="use-case-predictive-maintenance"></a>Kullanım örneği: Tahmine dayalı bakım

Bu senaryoyu 2008'de Prognostik ve Sağlık Yönetimi Konferansı'nda (PHM08) sunulan bir kullanım örneğine dayandırdık. Amaç turbofan uçak motorları bir dizi yararlı ömrü (RUL) kalan tahmin etmektir. Bu veriler MAPSS (Modüler Aero-İtiş Sistemi Simülasyonu) yazılımının ticari versiyonu olan C-MAPSS kullanılarak oluşturulmuştur. Bu yazılım, sağlık, kontrol ve motor parametrelerini uygun bir şekilde simüle etmek için esnek bir turbofan motor simülasyon ortamı sağlar.

Bu eğitimde kullanılan veriler [Turbofan motor bozunması simülasyon veri kümesinden alınmıştır.](https://ti.arc.nasa.gov/tech/dash/groups/pcoe/prognostic-data-repository/#turbofan)

Okuma dosyasından:

***Deneysel Senaryo***

*Veri kümeleri birden çok değişkenli zaman serilerinden oluşur. Her veri kümesi daha fazla eğitim ve test alt kümeleri ayrılır. Her zaman serisi farklı bir motordan - yani, veri aynı tip motor filosundan olarak kabul edilebilir. Her motor, kullanıcı tarafından bilinmeyen farklı kullanım ve üretim varyasyonu dereceleriyle başlar. Bu aşınma ve varyasyon normal olarak kabul edilir, yani, bir hata durumu olarak kabul edilmez. Motor performansı üzerinde önemli bir etkisi olan üç çalışma ayarı vardır. Bu ayarlar da verilere dahildir. Veriler sensör gürültüsü yle kirlenmiş.*

*Motor her zaman serisinin başında normal olarak çalışıyor ve seri sırasında bir noktada bir hata geliştirir. Eğitim setinde, sistem arızalanana kadar fay büyüklük olarak büyür. Test kümesinde, zaman serisi sistem hatasından bir süre önce sona erer. Yarışmanın amacı, test setinde ki arızadan önce kalan çalışma döngüsünün sayısını tahmin etmektir, yani motorun çalışmaya devam edeceği son döngüden sonraki çalışma döngülerinin sayısını. Ayrıca test verileri için gerçek Kalan Yararlı Yaşam (RUL) değerlerinin bir vektörü sağlanmıştır.*

Veriler bir yarışma için yayınlandığı için, makine öğrenimi modellerini türetmek için çeşitli yaklaşımlar bağımsız olarak yayınlanmıştır. Örneklerin incelenmesinin, belirli bir makine öğrenimi modelinin oluşturulmasında yer alan süreci ve muhakemeyi anlamada yararlı olduğunu bulduk. Örneğin bkz:

GitHub kullanıcı jancervenka tarafından [Uçak motoru arızası tahmin modeli.](https://github.com/jancervenka/turbofan_failure)

GitHub kullanıcı hankroark tarafından [Turbofan motor bozulması.](https://github.com/hankroark/Turbofan-Engine-Degradation)

## <a name="process"></a>İşleme

Aşağıdaki resimde bu öğreticide izlediğimiz kaba adımlar gösteriş verilmiştir:

![İşlem adımları için mimari diyagramı](media/tutorial-machine-learning-edge-01-intro/tutorial-steps-overview.png)

1. **Eğitim verilerini toplama**: Süreç eğitim verilerini toplayarak başlar. Bazı durumlarda, veriler zaten toplanmış ve bir veritabanında veya veri dosyaları biçiminde kullanılabilir. Diğer durumlarda, özellikle IoT senaryoları için verilerin IoT aygıtlarından ve sensörlerinden toplanması ve bulutta depolanması gerekir.

   Turbofan motorları koleksiyonunuz olmadığını varsayıyoruz, bu yüzden proje dosyaları nasa cihaz verilerini buluta gönderen basit bir cihaz simülatörü içeriyor.

1. **Veri hazırlayın.** Çoğu durumda, cihazlardan ve sensörlerden toplanan ham veriler makine öğrenimine hazırlık gerektirir. Bu adım, veri temizleme, veri yeniden biçimlendirme veya ek bilgi makine öğrenme enjekte etmek için ön işleme içerebilir kapalı anahtar olabilir.

   Uçak motoru makinesi verilerimiz için, veri hazırlama, veriler üzerindeki gerçek gözlemlere dayanarak örnekteki her veri noktası için açık zaman-hata süreleri hesaplamayı içerir. Bu bilgiler, makine öğrenimi algoritmasının gerçek sensör veri kalıpları ile motorun beklenen kalan ömrü arasındaki korelasyonları bulmasını sağlar. Bu adım, etki alanına son derece özeldir.

1. **Bir makine öğrenme modeli oluşturun.** Hazırlanan verilere dayanarak, artık modelleri eğitmek ve sonuçları birbirleriyle karşılaştırmak için farklı makine öğrenimi algoritmaları ve parametreizasyonları deneyebilirsiniz.

   Bu durumda, test için, model tarafından hesaplanan öngörülen sonucu bir dizi motorda gözlenen gerçek sonuçla karşılaştırıyoruz. Azure Machine Learning'de, model kayıt defterinde oluşturduğumuz modellerin farklı yinelemelerini yönetebiliriz.

1. **Modeli dağıtın.** Başarı kriterlerimizi karşılayan bir modele sahip olduğumuzda, dağıtıma geçebiliriz. Bu, modeli REST aramaları ve iade analizi sonuçları kullanılarak veriyle beslenebilen bir web hizmeti uygulamasına sarmayı içerir. Web hizmeti uygulaması daha sonra bir docker konteyner içine paketlenir, sırayla bulut ya da bir IoT Edge modülü olarak dağıtılabilir. Bu örnekte, IoT Edge'e dağıtıma odaklanıyoruz.

1. **Modeli koruyun ve geliştirin.** Model dağıtıldıktan sonra işimiz yapılmaz. Çoğu durumda, veri toplamaya devam etmek ve bu verileri düzenli olarak buluta yüklemek istiyoruz. Daha sonra bu verileri, modelimizi yeniden eğitmek ve iyileştirmek için kullanabiliriz, ki bu da daha sonra IoT Edge'e yeniden dağıtabiliriz.

## <a name="prerequisites"></a>Ön koşullar

Öğreticiyi tamamlamak için, kaynak oluşturma hakkına sahip olduğunuz bir Azure aboneliğine erişmeniz gerekir. Bu öğreticide kullanılan hizmetlerden bazıları Azure ücretlerine tabi olacaktır. Azure aboneliğiniz yoksa, [Azure Ücretsiz Hesabı'na](https://azure.microsoft.com/offers/ms-azr-0044p/)başlayabilirsiniz.

Ayrıca, geliştirme makineniz olarak bir Azure Sanal Makine ayarlamak için komut dosyalarını çalıştırabileceğiniz PowerShell yüklü bir makineye de ihtiyacınız vardır.

Bu belgede, aşağıdaki araçlar kümesini kullanırız:

* Veri yakalama için bir Azure IoT hub'ı

* Azure Dizüstü Bilgisayarlar, veri hazırlama ve makine öğrenimi denemeleri için ana ön uç umuzdır. Örnek verilerin bir alt kümesinde bir not defterinde python kodunu çalıştırmak, veri hazırlama sırasında hızlı yinelemeli ve etkileşimli geri dönüş elde etmenin harika bir yoludur. Jupyter dizüstü bilgisayarlar da bir bilgi işlem arka uçta ölçekte çalıştırmak için komut dosyaları hazırlamak için kullanılabilir.

* Azure Machine Learning, ölçekte makine öğrenimi ve makine öğrenimi görüntü oluşturma için bir arka uç olarak gelir. Jupyter dizüstü bilgisayarlarda hazırlanan ve test edilen komut dosyalarını kullanarak Azure Machine Learning arka ucunu kullanıyoruz.

* Makine öğrenme görüntüsünün bulut dışı uygulaması için Azure IoT Edge

Açıkçası, başka seçenekler de mevcut. Bazı senaryolarda, örneğin, IoT Central, IoT aygıtlarından ilk eğitim verilerini yakalamak için kodsuz bir alternatif olarak kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğretici aşağıdaki bölümlere ayrılmıştır:

1. Geliştirme makinenizi ve Azure hizmetlerinizi ayarlayın.
2. Makine öğrenimi modülü için eğitim verilerini oluşturun.
3. Makine öğrenimi modüllerini eğitin ve dağıtın.
4. Bir IoT Edge aygıtını saydam bir ağ geçidi olarak hareket etmek üzere yapılandırın.
5. IoT Edge modülleri oluşturun ve dağıtın.
6. IoT Edge cihazınıza veri gönderin.

Bir geliştirme makinesi kurmak ve Azure kaynaklarını sağlamak için bir sonraki makaleye devam edin.

> [!div class="nextstepaction"]
> [IoT Edge'de makine öğrenimi için bir ortam ayarlama](tutorial-machine-learning-edge-02-prepare-environment.md)
