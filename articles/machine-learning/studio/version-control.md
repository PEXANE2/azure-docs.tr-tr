---
title: Uygulama yaşam döngüsü yönetimi
titleSuffix: ML Studio (classic) - Azure
description: Azure Machine Learning Studio'da Uygulama Yaşam Döngüsü Yönetimi en iyi uygulamaları uygulayın (klasik)
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: likebupt
ms.author: keli19
ms.date: 10/27/2016
ms.openlocfilehash: 3f22ce3b1fb750e33e35d35ee1fe5ad1893abcfd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79204145"
---
# <a name="application-lifecycle-management-in-azure-machine-learning-studio-classic"></a>Azure Machine Learning Studio'da Uygulama Yaşam Döngüsü Yönetimi (klasik)

[!INCLUDE [Notebook deprecation notice](../../../includes/aml-studio-notebook-notice.md)]

Azure Machine Learning Studio (klasik), Azure bulut platformunda işlevsel hale getirilen makine öğrenimi denemeleri geliştirmek için bir araçtır. Visual Studio IDE ve ölçeklenebilir bulut hizmetinin tek bir platformda birleştirilmesi gibidir. Çeşitli varlıkların sürümünden otomatik yürütme ve dağıtıma kadar standart Uygulama Yaşam Döngüsü Yönetimi (ALM) uygulamalarını Azure Machine Learning Studio'ya (klasik) dahil edebilirsiniz. Bu makalede, bazı seçenekler ve yaklaşımlar tartışılmaktadır.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="versioning-experiment"></a>Sürüm denemesi
Denemelerinizi sürümlemek için önerilen iki yol vardır. Yerleşik çalışma geçmişine güvenebilir veya denemeyi harici olarak yönetmek için JSON biçiminde dışa aktarabilirsiniz. Her yaklaşım artılarını ve eksilerini ile birlikte gelir.

### <a name="experiment-snapshots-using-run-history"></a>Çalışma Geçmişi'ni kullanarak deneme anlık görüntüleri
Azure Machine Learning Studio (klasik) öğrenme deneyinin yürütme modelinde, deneme düzenleyicisinde **Çalıştır'ı** tıklattığınızda denemenin değişmez bir görüntüsü iş zamanlayıcısına gönderilir. Anlık görüntü listesini görüntülemek için, deneme düzenleyicisi görünümünde komut çubuğunda **Geçmişi Çalıştır'ı** tıklatın.

![Geçmiş'i çalıştır düğmesi](./media/version-control/runhistory.png)

Daha sonra, denemenin çalıştırılacağı ve anlık görüntünün alındığı sırada denemenin adını tıklatarak Kilitli modda anlık görüntüaçabilirsiniz. Geçerli denemeyi temsil eden listedeki yalnızca ilk öğenin Editable durumunda olduğuna dikkat edin. Ayrıca, her anlık görüntü, Bitmiş (Kısmi çalıştırma), Başarısız, Başarısız (Kısmi çalıştırma) veya Taslak dahil olmak üzere çeşitli Durum durumlarında da olabileceğini unutmayın.

![Geçmiş listesini çalıştır](./media/version-control/runhistorylist.png)

Açıldıktan sonra, anlık görüntü denemesini yeni bir deneme olarak kaydedebilir ve sonra değiştirebilirsiniz. Deneme anlık görüntünüz, güncelleştirilmiş sürümler gibi eğitilmiş modeller, dönüşümler veya veri kümeleri gibi varlıklar içeriyorsa, anlık görüntü alındığında orijinal sürüme yapılan başvuruları saklar. Kilitli anlık fotoğrafı yeni bir deneme olarak kaydederseniz, Azure Machine Learning Studio (klasik) bu varlıkların daha yeni bir sürümünün varlığını algılar ve bunları yeni denemede otomatik olarak güncelleştirir.

Denemeyi silerseniz, bu denemenin tüm anlık görüntüleri silinir.

### <a name="exportimport-experiment-in-json-format"></a>JSON formatında verme/alma denemesi
Çalışma geçmişi anlık görüntüleri, denemenin değişmez bir sürümünü Azure Machine Learning Studio'da (klasik) her çalıştırış için gönderildiğinde tutar. Ayrıca denemenin yerel bir kopyasını kaydedebilir ve Team Foundation Server gibi favori kaynak denetim sisteminizde iade edebilir ve daha sonra bu yerel dosyadan bir denemeyeniden oluşturabilirsiniz. Bunu başarmak için [Azure Machine Learning PowerShell](https://aka.ms/amlps) komutlarını [*Export-AmlExperimentGraph*](https://github.com/hning86/azuremlps#export-amlexperimentgraph) ve [*Import-AmlExperimentGraph*](https://github.com/hning86/azuremlps#import-amlexperimentgraph) komut larını kullanabilirsiniz.

JSON dosyası, veri kümesi veya eğitilmiş bir model gibi çalışma alanındaki varlıklara başvuru içerebilecek deneme grafiğinin metinsel bir gösterimidir. Varlığın serileştirilmiş bir sürümünü içermez. JSON belgesini çalışma alanına geri aktarmayı denerseniz, başvurulan varlıkların denemede başvurulan aynı varlık kimlikleriyle zaten var olması gerekir. Aksi takdirde içe aktarılan denemeye erişemezsiniz.

## <a name="versioning-trained-model"></a>Sürüm eğitimli model
Azure Machine Learning Studio'da (klasik) eğitilmiş bir model, iLearner`.iLearner`dosyası olarak bilinen bir biçime seri hale getirilmiştir ve çalışma alanıyla ilişkili Azure Blob depolama hesabında depolanır. iLearner dosyasının bir kopyasını almak için bir yolu yeniden eğitim API geçer. [Bu makalede,](/azure/machine-learning/studio/retrain-machine-learning-model) yeniden eğitim API nasıl çalıştığını açıklar. Üst düzey adımlar:

1. Eğitim deneyiminizi ayarlayın.
2. Tren Modeli modülüne veya Tune Model Hyperparameter veya Create R Model gibi eğitilmiş modeli üreten modüle bir web hizmeti çıkış bağlantı noktası ekleyin.
3. Eğitim denemenizi çalıştırın ve ardından bir model eğitim web hizmeti olarak dağıtın.
4. Eğitim web hizmetinin BES bitiş noktasını arayın ve depolanacak istenilen iLearner dosya adını ve Blob depolama hesap konumunu belirtin.
5. BES araması bittikten sonra üretilen iLearner dosyasını hasat edin.

iLearner dosyasını almak için başka bir yolu PowerShell komut [*indir-AmlExperimentNodeOutput*](https://github.com/hning86/azuremlps#download-amlexperimentnodeoutput)geçer. Modeli programlı olarak yeniden eğitmeye gerek kalmadan iLearner dosyasının bir kopyasını almak istiyorsanız bu daha kolay olabilir.

Eğitilen modeli içeren iLearner dosyasını aldıktan sonra, kendi sürüm stratejinizi kullanabilirsiniz. Strateji, bir adlandırma kuralı kadar ön/postfix uygulamak ve iLearner dosyasını Blob depolamaalanında bırakmak veya sürüm kontrol sisteminize kopyalamak/almak kadar basit olabilir.

Kaydedilen iLearner dosyası daha sonra dağıtılan web hizmetleri üzerinden puanlama için kullanılabilir.

## <a name="versioning-web-service"></a>Web hizmetinin sürümü
Azure Machine Learning Studio (klasik) denemesinden iki tür web hizmeti dağıtabilirsiniz. Klasik web hizmeti, çalışmanın yanı sıra denemeyle de sıkı bir şekilde birleştiğinde çalışır. Yeni web hizmeti Azure Kaynak Yöneticisi çerçevesini kullanır ve artık özgün deneme veya çalışma alanıyla birleştiğinde değildir.

### <a name="classic-web-service"></a>Klasik web hizmeti
Klasik bir web hizmetini sürüm olarak, web hizmeti bitiş noktası yapısından yararlanabilirsiniz. Burada tipik bir akış:

1. Öngörülü denemenizden, varsayılan bir bitiş noktası içeren yeni bir klasik web hizmeti dağıtmış olursunuz.
2. Deneme/eğitilmiş modelin geçerli sürümünü ortaya çıkaran ep2 adında yeni bir bitiş noktası oluşturursunuz.
3. Geri dönüp tahmine dayalı deneyinizi ve eğitimli modelinizi güncelleyin.
4. Varsayılan bitiş noktasını güncelleştiren tahmine dayalı denemeyi yeniden dağıtırsınız. Ama bu ep2 değişmez.
5. Denemenin ve eğitilmiş modelin yeni sürümünü ortaya çıkaran ep3 adlı ek bir bitiş noktası oluşturursunuz.
6. Gerekirse adım 3'e geri dön.

Zaman içinde, aynı web hizmetinde birçok uç nokta oluşturulabilir. Her uç nokta, eğitilen modelin zaman içinde nokta sürümünü içeren denemenin zaman içinde bir kopyasını temsil eder. Daha sonra hangi bitiş noktasının çağrılmasını belirlemek için dış mantığı kullanabilirsiniz, bu da puanlama çalışması için eğitilmiş modelin bir sürümünü seçmek anlamına gelir.

Ayrıca birçok aynı web hizmeti uç noktaları oluşturabilir ve daha sonra benzer bir etki elde etmek için bitiş noktasına iLearner dosyasının farklı sürümleriye yama. [Bu makalede,](create-models-and-endpoints-with-powershell.md) bunu nasıl başaranacakları daha ayrıntılı olarak açıklanmaktadır.

### <a name="new-web-service"></a>Yeni web hizmeti
Yeni bir Azure Kaynak Yöneticisi tabanlı web hizmeti oluşturursanız, bitiş noktası yapısı artık kullanılamaz. Bunun yerine, Dışa [Aktarma-AmlWebServiceDefinitionFromExperiment](https://github.com/hning86/azuremlps#export-amlwebservicedefinitionfromexperiment) PowerShell komut izini kullanarak veya dağıtılmış Kaynak Yöneticisi tabanlı bir web hizmetinden [*Export-AzMlWebservice*](https://docs.microsoft.com/powershell/module/az.machinelearning/export-azmlwebservice) PowerShell komut dosyalarını kullanarak, tahmine dayalı denemenizden JSON formatında web hizmeti tanımı (WSD) dosyaları oluşturabilirsiniz.

Dışa aktarılan WSD dosyasını ve sürüm denetimini yaptıktan sonra, WSD'yi farklı bir Azure bölgesinde farklı bir web hizmeti planında yeni bir web hizmeti olarak da dağıtabilirsiniz. Yeni web servis planı kimliğinin yanı sıra uygun depolama hesabı yapılandırmasını sağladığından emin olun. Farklı iLearner dosyalarını yamalamak için WSD dosyasını değiştirebilir ve eğitilen modelin konum başvurularını güncelleyebilir ve yeni bir web hizmeti olarak dağıtabilirsiniz.

## <a name="automate-experiment-execution-and-deployment"></a>Deneme yürütme ve dağıtımını otomatikleştirin
ALM'nin önemli bir yönü, uygulamanın yürütme ve dağıtım işlemini otomatikleştirebilmektir. Azure Machine Learning Studio'da (klasik), [PowerShell modüllerini](https://aka.ms/amlps)kullanarak bunu başarabilirsiniz. Azure [Machine Learning Studio (klasik) PowerShell modüllerini](https://aka.ms/amlps)kullanarak standart BIR ALM otomatik yürütme/dağıtım işlemiyle alakalı uçtan uca adımlara bir örnek aşağıda verilmiştir. Her adım, bu adımı gerçekleştirmek için kullanabileceğiniz bir veya daha fazla PowerShell komut aletine bağlıdır.

1. [Bir veri seti yükleyin.](https://github.com/hning86/azuremlps#upload-amldataset)
2. Bir eğitim denemesini [çalışma alanından](https://github.com/hning86/azuremlps#copy-amlexperiment) veya [Galeri'den](https://github.com/hning86/azuremlps#copy-amlexperimentfromgallery)çalışma alanına kopyalayın veya [dışa aktarılan](https://github.com/hning86/azuremlps#export-amlexperimentgraph) bir denemeyi yerel diskten [aktarın.](https://github.com/hning86/azuremlps#import-amlexperimentgraph)
3. Eğitim deneyindeki [veri kümesini güncelleştirin.](https://github.com/hning86/azuremlps#update-amlexperimentuserasset)
4. [Eğitim deneyini çalıştırın.](https://github.com/hning86/azuremlps#start-amlexperiment)
5. [Eğitimli modeli tanıtın.](https://github.com/hning86/azuremlps#promote-amltrainedmodel)
6. [Tahmine dayalı bir denemeyi](https://github.com/hning86/azuremlps#copy-amlexperiment) çalışma alanına kopyalayın.
7. Tahmine dayalı deneyde [eğitilmiş modeli güncelleştirin.](https://github.com/hning86/azuremlps#update-amlexperimentuserasset)
8. [Tahmin deneyi çalıştırın.](https://github.com/hning86/azuremlps#start-amlexperiment)
9. Tahmine dayalı denemeden [bir web hizmeti dağıtın.](https://github.com/hning86/azuremlps#new-amlwebservice)
10. Web hizmeti [RRS](https://github.com/hning86/azuremlps#invoke-amlwebservicerrsendpoint) veya [BES](https://github.com/hning86/azuremlps#invoke-amlwebservicebesendpoint) bitiş noktasını test edin.

## <a name="next-steps"></a>Sonraki adımlar
* Azure [Machine Learning Studio (klasik) PowerShell](https://aka.ms/amlps) modüllerini indirin ve ALM görevlerinizi otomatikleştirmeye başlayın.
* PowerShell aracılığıyla tek bir deneme kullanarak ve API'yi yeniden [eğiterek çok sayıda ML modeli oluşturmayı](create-models-and-endpoints-with-powershell.md) ve nasıl yöneteceğimize öğrenin.
* [Azure Machine Learning web hizmetlerini dağıtma](deploy-a-machine-learning-web-service.md)hakkında daha fazla bilgi edinin.
