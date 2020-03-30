---
title: Duyarlılık analizi kullanım örneği
titleSuffix: ML Studio (classic) - Azure
description: Metin ön işleme, N-gram veya özellik karmalama modüllerini kullanarak Azure Machine Learning Studio'da (klasik) metin analizi modelleri nasıl oluşturulur?
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: likebupt
ms.author: keli19
ms.custom: seodec18
ms.date: 03/14/2018
ms.openlocfilehash: 89fc5196977f53e040e1a6553b46ca57f39b18b4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79217891"
---
# <a name="create-a-sentiment-analysis-model-in-azure-machine-learning-studio-classic"></a>Azure Machine Learning Studio'da bir duygu analizi modeli oluşturun (klasik)

[!INCLUDE [Notebook deprecation notice](../../../includes/aml-studio-notebook-notice.md)]

Metin analizi modelleri oluşturmak ve işlevsel hale getirmek için Azure Machine Learning Studio'yı (klasik) kullanabilirsiniz. Bu modeller, örneğin belge sınıflandırma veya duyarlılık analizi sorunlarını çözmenize yardımcı olabilir.

Bir metin analizi deneyinde, genellikle şunları yapardınız:

1. Temizleme ve işlem öncesi metin veri kümesi
2. Önceden işlenmiş metinden sayısal özellik vektörlerini ayıklama
3. Tren sınıflandırması veya regresyon modeli
4. Skoru ve modeli doğrula
5. Modeli üretime dağıtma

Bu eğitimde, Biz Amazon Kitap İncelemeleri dataset kullanarak bir duygu analizi modeli ile yürürken bu adımları öğrenmek (John Blitzer tarafından bu araştırma kağıt "Biyografiler, Bollywood, Boom-kutuları ve Blenders: Domain Adaptasyon Sentiment Sınıflandırma" bakın, Mark Dredze ve Fernando Pereira; Hesaplamalı Dilbilim Derneği (ACL), 2007.) Bu veri kümesi gözden geçirme puanları (1-2 veya 4-5) ve serbest biçimli bir metinden oluşur. Amaç değerlendirme puanı tahmin etmektir: düşük (1-2) veya yüksek (4-5).

Azure AI Galerisi'nde bu öğreticide yer alan denemeleri bulabilirsiniz:

[Kitap İncelemelerini Tahmin Et](https://gallery.azure.ai/Experiment/Predict-Book-Reviews-1)

[Kitap İncelemelerini Tahmin Et - Tahmindeney](https://gallery.azure.ai/Experiment/Predict-Book-Reviews-Predictive-Experiment-1)

## <a name="step-1-clean-and-preprocess-text-dataset"></a>Adım 1: Metin veri kümesini temizleme ve işleme öncesi
Deneye, inceleme puanlarını kategorik düşük ve yüksek kovalara bölerek sorunu iki sınıflı sınıflandırma olarak formüle ederek başlıyoruz. Meta verileri ve [Grup Kategorik Değerleri](https://msdn.microsoft.com/library/azure/dn906014.aspx) modüllerini [edit](https://msdn.microsoft.com/library/azure/dn905986.aspx) kullanıyoruz.

![Etiket Oluştur](./media/text-analytics-module-tutorial/create-label.png)

Daha sonra, [Preprocess Text](https://msdn.microsoft.com/library/azure/mt762915.aspx) modüllerini kullanarak metni temizliyoruz. Temizleme, veri kümesindeki gürültüyü azaltır, en önemli özellikleri bulmanıza yardımcı olur ve son modelin doğruluğunu artırır. "the" veya "a" gibi yaygın sözcükleri ve sayıları, özel karakterleri, yinelenen karakterleri, e-posta adreslerini ve URL'leri kaldırırız. Ayrıca metni küçük harfe dönüştürür, sözcükleri lemmatize eder ve önceden işlenmiş metinde "|||" sembolüyle gösterilen cümle sınırlarını algılarız.

![Metni Ön İşleme](./media/text-analytics-module-tutorial/preprocess-text.png)

Özel bir stopwords listesi kullanmak isterseniz ne olur? İsteğe bağlı giriş olarak geçirebilirsiniz. Alt dizeleri değiştirmek ve sözcükleri konuşmanın bir bölümüolan adlar, fiiller veya sıfatlar olarak kaldırmak için özel C# sözdizimi normal ifadesini de kullanabilirsiniz.

Ön işleme tamamlandıktan sonra, verileri tren ve test kümelerine böleriz.

## <a name="step-2-extract-numeric-feature-vectors-from-pre-processed-text"></a>Adım 2: Önceden işlenmiş metinden sayısal özellik vektörlerini ayıklama
Metin verileri için bir model oluşturmak için genellikle serbest biçimli metni sayısal özellik vektörlerine dönüştürmeniz gerekir. Bu örnekte, metin verilerini bu biçime dönüştürmek için [Metin modülünden Alıntı N-Gram Özelliklerini](https://msdn.microsoft.com/library/azure/mt762916.aspx) kullanırız. Bu modül, beyazuzaya ayrılmış sözcüklerden oluşan bir sütun alır ve veri kümenizde görünen sözcüklersözlüğüveya N gram sözcükleri hesaplar. Daha sonra, her sözcüğün veya N gramın her kayıtta kaç kez göründüğünü sayar ve bu sayımlardan özellik vektörleri oluşturur. Bu öğreticide, N-gram boyutunu 2 olarak ayarlıyoruz, böylece özellik vektörlerimiz tek kelime ve sonraki iki kelimenin birleşimlerini içerir.

![N-gram özü](./media/text-analytics-module-tutorial/extract-ngrams.png)

N-gram sayımlarına TF*IDF (Dönem Frekansı Ters Belge Frekansı) ağırlıklarını uyguluyoruz. Bu yaklaşım, tek bir kayıtta sık sık görünen ancak tüm veri kümesinde nadir bulunan sözcüklerin ağırlığını ekler. Diğer seçenekler ikili, TF ve grafik tartımı içerir.

Bu tür metin özellikleri genellikle yüksek boyutsallığa sahiptir. Örneğin, korpusunuzda 100.000 benzersiz sözcük varsa, özellik alanınız 100.000 boyuta veya N-gram kullanılırsa daha fazla boyuta sahip olur. Extract N-Gram Özellikler modülü, boyutsallığı azaltmak için bir dizi seçenek sunar. Önemli bir tahmin değerine sahip olmak için kısa veya uzun veya çok nadir veya çok sık olan sözcükleri hariç tutmayı seçebilirsiniz. Bu öğreticide, 5'ten az kayıtta veya kayıtların %80'inden fazlasında görünen N gramını hariç tutarız.

Ayrıca, yalnızca tahmin hedefinizle en ilişkili özellikleri seçmek için özellik seçimini kullanabilirsiniz. 1000 özellik seçmek için Chi-Squared özellik seçimini kullanıyoruz. Extract N-gram modülünün doğru çıktısını tıklatarak seçili sözcüklerin veya N-gram'ın sözcük dağarcığını görüntüleyebilirsiniz.

Extract N-Gram Özelliklerini kullanmaya alternatif bir yaklaşım olarak Özellik Karma modülünü kullanabilirsiniz. [Özellik Karma'nın](https://msdn.microsoft.com/library/azure/dn906018.aspx) yap-in özellik seçimi yetenekleri veya TF*IDF tartım özelliklerine sahip olmamasına rağmen dikkat edin.

## <a name="step-3-train-classification-or-regression-model"></a>Adım 3: Tren sınıflandırmaveya regresyon modeli
Şimdi metin sayısal özellik sütunlarına dönüştürüldü. Veri kümesi hala önceki aşamalardan gelen dize sütunları içerir, bu nedenle bunları dışlamak için Veri Kümesi'ndeki Sütunları Seç'i kullanırız.

Daha sonra hedefimizi tahmin etmek için [Iki Sınıf Lojistik Regresyon](https://msdn.microsoft.com/library/azure/dn905994.aspx) kullanıyoruz: yüksek veya düşük inceleme puanı. Bu noktada, metin analizi sorunu düzenli bir sınıflandırma sorununa dönüştürülmüştür. Modeli geliştirmek için Azure Machine Learning Studio'da (klasik) kullanılabilen araçları kullanabilirsiniz. Örneğin, ne kadar doğru sonuçlar verdiklerini öğrenmek için farklı sınıflandırıcılarla denemeler yapabilir veya doğruluğu artırmak için hiperparametre atomasını kullanabilirsiniz.

![Tren ve Puan](./media/text-analytics-module-tutorial/scoring-text.png)

## <a name="step-4-score-and-validate-the-model"></a>Adım 4: Puan ve modeli doğrulamak
Eğitimli modeli nasıl onaylarsınız? Test veri setine karşı puanve doğruluğunu değerlendiririz. Ancak, model n-gram kelime ve ağırlıkları eğitim dataset öğrendim. Bu nedenle, kelime dağarcığını yeniden oluşturmak yerine, test verilerinden özellikleri ayıklarken bu kelime dağarcığını ve bu ağırlıkları kullanmalıyız. Bu nedenle, denemenin puanlama dalına Extract N-Gram Özellikleri modülünü ekliyoruz, eğitim dalından çıktı sözcük dağarcığı bağlıyoruz ve kelime dağarcığı modunu salt okunur şekilde ayarlıyoruz. Ayrıca, en az 1 örneğini ve maksimumı %100'e ayarlayarak N-gram filtrelemeözelliğini frekansa göre devre dışı bırakıp, özellik seçimini kapatıyoruz.

Test verilerindeki metin sütunu sayısal özellik sütunlarına dönüştürüldükten sonra, dize sütunlarını eğitim dalındaki gibi önceki aşamalardan hariç tutarız. Daha sonra tahmin yapmak ve doğruluğu değerlendirmek için Model modülünün değerlendirilmesi için Puan Modeli modüllerini kullanırız.

## <a name="step-5-deploy-the-model-to-production"></a>Adım 5: Modeli üretime dağıtma
Model üretime gönderilmeye neredeyse hazır. Web hizmeti olarak dağıtıldığında, giriş olarak serbest biçimli metin dizealır ve bir tahmin "yüksek" veya "düşük" döndürün. Metni özelliklere dönüştürmek için öğrenilen N-gram kelime dağarcığını kullanır ve bu özelliklerden bir tahminde bulunmak için lojistik regresyon modelini eğitir. 

Tahmine dayalı deneyi ayarlamak için, önce N-gram kelime dağarcığını veri kümesi olarak ve eğitilmiş lojistik regresyon modelini deneyin eğitim dalından kaydediyoruz. Daha sonra, tahmine dayalı deney için bir deneme grafiği oluşturmak için "Farklı Kaydet" kullanarak denemeyi kaydederiz. Bölünmüş Veri modüllerini ve eğitim dalını deneyden kaldırıyoruz. Daha sonra daha önce kaydedilmiş N-gram kelime ve modelini sırasıyla N-Gram Özelliklerini ve Puan Model modüllerini ayıklamak için bağlıyız. Model Değerlendir modüllerini de kaldırıyoruz.

Etiket sütununu kaldırmak için Preprocess Text modülünden önce Dataset modülüne Select Columns'u ekleriz ve Puan Modülünde "Dataset'e Ek puan sütunu" seçeneğini kaldırırız. Bu şekilde, web hizmeti tahmin etmeye çalıştığı etiketi talep etmez ve yanıt olarak giriş özelliklerini yankılanmaz.

![Tahmine Dayalı Deney](./media/text-analytics-module-tutorial/predictive-text.png)

Şimdi bir web hizmeti olarak yayınlanabilir ve istek yanıt veya toplu yürütme API'leri kullanılarak adlandırılan bir deneme var.

## <a name="next-steps"></a>Sonraki Adımlar
[MSDN belgelerinden](https://msdn.microsoft.com/library/azure/dn905886.aspx)metin analizi modülleri hakkında bilgi edinin.

