---
title: Python betiklerini yürütme
titleSuffix: ML Studio (classic) - Azure
description: Machine Learning Studio (klasik) deneylerde ve web hizmetlerinde Python kodunu kullanmak için Execute Python Script modüllerini nasıl kullanacağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: likebupt
ms.author: keli19
ms.custom: previous-author=heatherbshapiro, previous-ms.author=hshapiro
ms.date: 03/12/2019
ms.openlocfilehash: c79f6bd63fa5d8d8c6b22ff271d8ca513a94fd64
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79218076"
---
# <a name="execute-python-machine-learning-scripts-in-azure-machine-learning-studio-classic"></a>Azure Machine Learning Studio'da Python makine öğrenimi komut dosyalarını çalıştırın (klasik)

[!INCLUDE [Notebook deprecation notice](../../../includes/aml-studio-notebook-notice.md)]

Python birçok veri bilim adamlarının araç göğsünde değerli bir araçtır. Veri arama, özellik çıkarma, model eğitimi ve doğrulama ve dağıtım dahil olmak üzere tipik makine öğrenimi iş akışlarının her aşamasında kullanılır.

Bu makalede, Azure Machine Learning Studio (klasik) denemeleri nizde ve web hizmetlerinizde Python kodunu kullanmak için Execute Python Script modüllerini nasıl kullanabileceğiniz açıklanmaktadır.

## <a name="using-the-execute-python-script-module"></a>Execute Python Script modüllerini kullanma

Studio Python birincil arayüzü (klasik) Python Script dosyası [çalıştırın][execute-python-script] geçer. En fazla üç girişi kabul eder ve [Execute R Script][execute-r-script] modülüne benzer iki çıktı üretir. Python kodu, parametre kutusuna özel olarak adlandırılmış `azureml_main`bir giriş noktası işlevi aracılığıyla girilir.

![Python Script modüllerini çalıştırın](./media/execute-python-scripts/execute-machine-learning-python-scripts-module.png)

![Modül parametre kutusunda örnek python kodu](./media/execute-python-scripts/embedded-machine-learning-python-script.png)

### <a name="input-parameters"></a>Giriş parametreleri

Python modülüne girişler Pandas DataFrames olarak ortaya alınır. İşlev, `azureml_main` en fazla iki isteğe bağlı Pandas DataFrame'i parametre olarak kabul eder.

Giriş bağlantı noktaları ve işlev parametreleri arasındaki eşleme konumsal:

- İlk bağlı giriş bağlantı noktası işlevin ilk parametresine eşlenir.
- İkinci giriş (bağlıysa) işlevin ikinci parametresine eşlenir.
- Üçüncü giriş [ek Python modülleri almak](#import-modules)için kullanılır.

Giriş bağlantı noktalarının `azureml_main` işlev parametrelerine nasıl eşlendirileceklerine daha ayrıntılı anlambilim aşağıda gösterilmiştir.

![Giriş bağlantı noktası yapılandırmaları tablosu ve ortaya çıkan Python imzası](./media/execute-python-scripts/python-script-inputs-mapped-to-parameters.png)

### <a name="output-return-values"></a>Çıktı iade değerleri

İşlev, `azureml_main` bir tuple, liste veya NumPy dizisi gibi Python [dizisinde](https://docs.python.org/2/c-api/sequence.html) paketlenmiş tek bir Pandas DataFrame döndürmelidir. Bu dizinin ilk öğesi modülün ilk çıkış noktasına döndürülür. Modülün ikinci çıkış bağlantı noktası [görselleştirmeler](#visualizations) için kullanılır ve bir dönüş değeri gerektirmez. Bu şema aşağıda gösterilmiştir.

![Giriş bağlantı noktalarını parametrelere eşleme ve çıkış bağlantı noktasına değeri döndürme](./media/execute-python-scripts/map-of-python-script-inputs-outputs.png)

## <a name="translation-of-input-and-output-data-types"></a>Giriş ve çıktı veri türlerinin çevirisi

Stüdyo veri kümeleri Panda DataFrames ile aynı değildir. Sonuç olarak, Studio'daki (klasik) giriş veri kümeleri Pandas DataFrame'e dönüştürülür ve çıktı DataFrame'leri Studio (klasik) veri kümelerine dönüştürülür. Bu dönüştürme işlemi sırasında aşağıdaki çeviriler de gerçekleştirilir:

 **Python veri türü** | **Stüdyo çeviri prosedürü** |
| --- | --- |
| Dizeleri ve sayısal| Olduğu gibi çevrilmiş |
| Pandalar 'NA' | 'Eksik değer' olarak çevrildi |
| Dizin vektörleri | Desteklenmeyen* |
| Dize dışı sütun adları | Sütun `str` adlarını arama |
| Yinelenen sütun adları | Sayısal sonek ekleyin: (1), (2), (3) ve benzeri.

**Python işlevindeki tüm giriş veri çerçeveleri her zaman 0'dan eksi 1 satır sayısına kadar 64 bit lik sayısal bir indekse sahiptir*

## <a name="importing-existing-python-script-modules"></a><a id="import-modules"></a>Varolan Python komut dosyası modüllerini alma

Python yürütmek için kullanılan arka uç [Anaconda](https://www.anaconda.com/distribution/)dayanmaktadır , yaygın olarak kullanılan bilimsel Python dağılımı. Veri merkezli iş yüklerinde kullanılan en yaygın Python paketlerinden 200'e yakını ile birlikte gelir. Studio (klasik) şu anda harici kitaplıkları kurmak ve yönetmek için Pip veya Conda gibi paket yönetim sistemlerinin kullanımını desteklememektedir.  Ek kitaplıklar dahil etme gereksinimini buluyorsanız, kılavuz olarak aşağıdaki senaryoyu kullanın.

Ortak bir kullanım örneği, varolan Python komut dosyalarını Studio (klasik) denemelere dahil etmektir. [Execute Python Script][execute-python-script] modülü, üçüncü giriş bağlantı noktasında Python modülleri içeren bir zip dosyasını kabul eder. Dosya çalışma zamanında yürütme çerçevesi tarafından fermuarsız ve içeriği Python yorumlayıcı kitaplık yoluna eklenir. Giriş `azureml_main` noktası işlevi daha sonra bu modülleri doğrudan içe aktarabilir. 

Örnek olarak, dosyayı basit bir "Hello, World" işlevi içeren Hello.py düşünün.

![Hello.py dosyada kullanıcı tanımlı işlev](./media/execute-python-scripts/figure4.png)

Ardından, Hello.py içeren bir Hello.zip dosyası oluşturuyoruz:

![Kullanıcı tanımlı Python kodu içeren zip dosyası](./media/execute-python-scripts/figure5.png)

Zip dosyasını veri seti olarak Studio'ya (klasik) yükleyin. Ardından, Hello.zip dosyasındaki Python kodunu aşağıdaki resimde gösterildiği gibi **Execute Python Script** modülünün üçüncü giriş bağlantı noktasına ekleyerek kullanan bir deneme oluşturun ve çalıştırın.

![Bir Execute Python Script modülüne giriş olarak Hello.zip ile örnek deneme](./media/execute-python-scripts/figure6a.png)

![Zip dosyası olarak yüklenen kullanıcı tanımlı Python kodu](./media/execute-python-scripts/figure6b.png)

Modül çıktısı, zip dosyasının paketlenmemiş olduğunu ve `print_hello` işlevin çalıştırıldığını gösterir.

![Kullanıcı tanımlı işlevi gösteren modül çıktısı](./media/execute-python-scripts/figure7.png)

## <a name="accessing-azure-storage-blobs"></a>Azure Depolama Bloblarına Erişim

Azure Blob Depolama hesabında depolanan verilere şu adımları kullanarak erişebilirsiniz:

1. Python [için Azure Blob Depolama paketini](https://azuremlpackagesupport.blob.core.windows.net/python/azure.zip) yerel olarak indirin.
1. Zip dosyasını bir veri kümesi olarak Studio (klasik) çalışma alanına yükleyin.
1. BlobService nesnenizi`protocol='http'`

```
from azure.storage.blob import BlockBlobService

# Create the BlockBlockService that is used to call the Blob service for the storage account
block_blob_service = BlockBlobService(account_name='account_name', account_key='account_key', protocol='http')
```

1. Depolama **Yapılandırma** ayar sekmesinde **gerekli Güvenli aktarım'ı** devre dışı

![Azure portalında gerekli Güvenli aktarımı devre dışı](./media/execute-python-scripts/disable-secure-transfer-required.png)

## <a name="operationalizing-python-scripts"></a>Python komut dosyalarını operasyonel hale getirme

Bir puanlama denemesinde kullanılan [Tüm Execute Python Script][execute-python-script] modülleri, web hizmeti olarak yayımlandığında çağrılır. Örneğin, aşağıdaki resimde tek bir Python ifadesini değerlendirmek için kodu içeren bir puanlama denemesi gösterilmektedir.

![Bir web hizmeti için stüdyo çalışma alanı](./media/execute-python-scripts/figure3a.png)

![Python Pandas ifadesi](./media/execute-python-scripts/python-script-with-python-pandas.png)

Bu denemeden oluşturulan bir web hizmeti aşağıdaki eylemleri alır:

1. Python ifadesini giriş olarak (dize olarak) alma
1. Python ifadesini Python yorumlayıcısına gönderme
1. Hem ifadeyi hem de değerlendirilen sonucu içeren bir tablo döndürür.

## <a name="working-with-visualizations"></a><a id="visualizations"></a>Görselleştirmelerle çalışma

MatplotLib kullanılarak oluşturulan çizimler Execute [Python Script][execute-python-script]tarafından döndürülebilir. Ancak, çizimler R kullanırken olduğu gibi otomatik olarak görüntülere yönlendirilir. Bu nedenle kullanıcı açıkça PNG dosyalarına herhangi bir çizim kaydetmek gerekir.

MatplotLib'den görüntü oluşturmak için aşağıdaki adımları atmanız gerekir:

1. Varsayılan Qt tabanlı işleyiciden arka ucu "AGG" olarak değiştirin.
1. Yeni bir şekil nesnesi oluşturun.
1. Ekseni alın ve içine tüm çizimler ilerler.
1. Şekli bir PNG dosyasına kaydedin.

Bu işlem, Pandalar'daki scatter_matrix işlevini kullanarak bir dağılım çizimmatrisi oluşturan aşağıdaki görüntülerde gösterilmiştir.

![MatplotLib figürlerini görüntülere kaydetmek için kod](./media/execute-python-scripts/figure-v1-8.png)

![Şekilleri görüntülemek için Python Script'i Çalıştır modülünde görselleştir'e tıklayın](./media/execute-python-scripts/figure-v2-9a.png)

![Python kodunu kullanarak örnek bir deneme için çizimleri görselleştirme](./media/execute-python-scripts/figure-v2-9b.png)

Farklı görüntülere kaydederek birden çok rakamı döndürmek mümkündür. Studio (klasik) çalışma zamanı tüm görüntüleri alır ve görselleştirme için onları birleştirir.

## <a name="advanced-examples"></a>Gelişmiş örnekler

Studio'da (klasik) kurulu Anaconda ortamı NumPy, SciPy ve Scikits-Learn gibi ortak paketleri içerir. Bu paketler, makine öğrenimi boru hattında veri işleme için etkili bir şekilde kullanılabilir.

Örneğin, aşağıdaki deneme ve komut dosyası, bir veri kümesi için özellik önem puanlarını hesaplamak için Scikits-Learn'te topluluk öğrenenlerin kullanımını göstermektedir. Puanlar, başka bir modele beslenmeden önce denetlenen özellik seçimini gerçekleştirmek için kullanılabilir.

Burada önem puanları hesaplamak ve puanları dayalı özellikleri sıralamak için kullanılan Python işlevi:

![Özellikleri puanlara göre sıralama işlevi](./media/execute-python-scripts/figure8.png)

Aşağıdaki deneme daha sonra Azure Machine Learning Studio'daki (klasik) "Pima Indian Diabetes" veri setindeki özelliklerin önem puanlarını hesaplar ve döndürür:

![Python kullanarak Pima Hint Diyabet veri setindeki özellikleri sıralamak için deneme](./media/execute-python-scripts/figure9a.png)

![Execute Python Script modülünün çıktısının görselleştirilmesi](./media/execute-python-scripts/figure9b.png)

## <a name="limitations"></a>Sınırlamalar

[Execute Python Script][execute-python-script] modülü şu anda aşağıdaki sınırlamalar vardır:

### <a name="sandboxed-execution"></a>Sandboxed yürütme

Python çalışma zamanı şu anda zandboxed ve kalıcı bir şekilde ağa veya yerel dosya sistemine erişim izin vermez. Yerel olarak kaydedilen tüm dosyalar modül tamamlandığında yalıtılır ve silinir. Python kodu, geçerli dizin ve alt dizinleri olmak üzere, çalıştığı makinedeki çoğu dizine erişemez.

### <a name="lack-of-sophisticated-development-and-debugging-support"></a>Sofistike geliştirme ve hata ayıklama desteğinin eksikliği

Python modülü şu anda intellisense ve hata ayıklama gibi IDE özelliklerini desteklememektedir. Ayrıca, modül çalışma zamanında başarısız olursa, tam Python yığını izleme kullanılabilir. Ancak modülün çıkış günlüğünde görüntülenmelidir. Şu anda, IPython gibi bir ortamda Python komut dosyalarını geliştirmenizi ve hata ayıklamanızı ve ardından kodu modüle aktarmanızı öneririz.

### <a name="single-data-frame-output"></a>Tek veri çerçevesi çıktısı

Python giriş noktasının yalnızca çıktı olarak tek bir veri çerçevesi döndürmesine izin verilir. Şu anda eğitimli modeller gibi rasgele Python nesnelerini doğrudan Studio (klasik) çalışma süresine döndürmek mümkün değildir. Aynı sınırlamaya sahip [Execute R Script][execute-r-script]gibi, birçok durumda nesneleri bir bayt diziiçine turşu ve daha sonra bir veri çerçevesi içinde döndürmek mümkündür.

### <a name="inability-to-customize-python-installation"></a>Python yüklemesini özelleştirememe

Şu anda, özel Python modülleri eklemek için tek yolu daha önce açıklanan zip dosya mekanizması üzerinden. Bu küçük modüller için uygun olsa da, büyük modüller (özellikle yerli DL'lere sahip modüller) veya çok sayıda modül için hantaldır.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için bkz. [Python Geliştirici Merkezi](https://azure.microsoft.com/develop/python/).

<!-- Module References -->
[execute-python-script]: https://docs.microsoft.com/azure/machine-learning/studio-module-reference/execute-python-script
[execute-r-script]: https://docs.microsoft.com/azure/machine-learning/studio-module-reference/execute-r-script
