---
title: Azure Machine Learning bilgi işlem örneği nedir?
titleSuffix: Azure Machine Learning
description: Tam olarak yönetilen bulut tabanlı bir iş istasyonu olan Azure Machine Learning bilgi işlem örneği hakkında bilgi edinin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: sgilley
author: sdgilley
ms.date: 12/13/2019
ms.openlocfilehash: 280851b2fea0b8100a7d0f8ec8105109a41c8c83
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79283933"
---
# <a name="what-is-an-azure-machine-learning-compute-instance"></a>Azure Machine Learning bilgi işlem örneği nedir?

Azure Machine Learning bilgi işlem örneği (önizleme), veri bilimciler için tam olarak yönetilen bulut tabanlı bir iş istasyonudur. 

Bilgi işlem örnekleri, Azure Machine Learning geliştirmeye başlamayı kolaylaştırır ve BT yöneticileri için yönetim ve kurumsal hazırlık özellikleri sağlar.  

Bulutta tam olarak yapılandırılmış ve yönetilen geliştirme ortamınız olarak bir bilgi işlem örneği kullanın.

İşlem örnekleri genellikle geliştirme ortamları olarak kullanılır.  Ayrıca, geliştirme ve test için eğitim ve çıkarılan bir bilgi işlem hedefi olarak da kullanılabilir.  Büyük görevlerde, çok düğümlü ölçekleme özelliklerine sahip bir [Azure Machine Learning bilgi işlem kümesi](how-to-set-up-training-targets.md#amlcompute) daha iyi bir bilgi işlem hedef seçimidir.


## <a name="why-use-a-compute-instance"></a>Neden bir işlem örneği kullanıyorsun?

Bilgi işlem örneği, makine öğrenimi geliştirme ortamınız için optimize edilmiş tam yönetilen bulut tabanlı bir iş istasyonudur. Aşağıdaki yararları sağlar:

|Önemli avantajlar||
|----|----|
|Üretkenlik|Veri bilimciler, entegre dizüstü bilgisayarları ve web tarayıcılarında aşağıdaki araçları kullanarak modeller oluşturabilir ve dağıtabilir:<br/>- Jupyter<br/>- JupyterLab<br/>- RStudio|
|Yönetilen & güvenli|Güvenlik ayak izinizi azaltın ve kurumsal güvenlik gereksinimlerine uygunluk ekleyin. Bilgi işlem örnekleri, sağlam yönetim ilkeleri ve şu gibi güvenli ağ yapılandırmaları sağlar:<br/><br/>- Kaynak Yöneticisi şablonlarından veya Azure Machine Learning SDK'dan otomatik sağlama<br/>- [Rol tabanlı erişim kontrolü (RBAC)](/azure/role-based-access-control/overview)<br/>- [Sanal ağ desteği](how-to-enable-virtual-network.md#compute-instance)<br/>- SSH erişimini etkinleştirmek/devre dışı kalmak için SSH ilkesi|
|Önceden yapılandırılmış&nbsp;&nbsp;veya ML|Önceden yapılandırılmış ve güncel ML paketleri, derin öğrenme çerçeveleri, GPU sürücüleri ile kurulum görevlerinde zamandan tasarruf edin.|
|Tamamen özelleştirilebilir|GPU'lar ve paketleri ve sürücüleri yükleme gibi düşük düzeyli özelleştirme de dahil olmak üzere Azure VM türleri için geniş destek, gelişmiş senaryoları kolaylaştırır. |

## <a name="tools-and-environments"></a><a name="contents"></a>Araçlar ve ortamlar

Azure Machine Learning bilgi işlem örneği, çalışma alanınızda modelleri tam entegre bir dizüstü bilgisayar deneyiminde yazmanızı, eğitmenizi ve dağıtmanızı sağlar.


Bu araçlar ve ortamlar işlem örneğine yüklenir: 

|Genel araçlar & ortamlar|Ayrıntılar|
|----|:----:|
|Sürücüler|`CUDA`</br>`cuDNN`</br>`NVIDIA`</br>`Blob FUSE` |
|Intel MPI kitaplığı||
|Azure CLI ||
|Azure Machine Learning örnekleri ||
|Azure Machine Learning EDAT motoru ||
|Docker||
|Nginx||
|NCCL 2.0 ||
|Protobuf|| 

|**Ortamlar** & R araçları|Ayrıntılar|
|----|:----:|
|RStudio Server Açık Kaynak Sürümü||
|R çekirdeği||
|R için Azure Machine Learning SDK|[azuremlsdk](https://azure.github.io/azureml-sdk-for-r/reference/index.html)</br>SDK örnekleri|

|**PYTHON** araçları & ortamlar|Ayrıntılar|
|----|----|
|Anaconda Python||
|Jupyter ve uzantıları||
|Jupyterlab ve uzantıları||
|Visual Studio Code ||
[Python için Azure Machine Learning SDK'sı](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py)</br>gönderen PyPI|`azureml-sdk[notebooks,contrib,automl,explain]`</br>`azureml-contrib-datadrift`</br>`azureml-telemetry`</br>`azureml-tensorboard`</br>`azureml-contrib-opendatasets`</br>`azureml-opendatasets`</br>`azureml-contrib-reinforcementlearning`</br>`azureml-mlflow`</br>`azureml-contrib-interpret` |
|Diğer PyPI paketleri|`jupytext`</br>`jupyterlab-git`</br>`tensorboard`</br>`nbconvert`</br>`notebook`</br>`Pillow`|
|Conda paketleri|`cython`</br>`numpy`</br>`ipykernel`</br>`scikit-learn`</br>`matplotlib`</br>`tqdm`</br>`joblib`</br>`nodejs`</br>`nb_conda_kernels`|
|Derin öğrenme paketleri|`PyTorch`</br>`TensorFlow`</br>`Keras`</br>`Horovod`</br>`MLFlow`</br>`pandas-ml`</br>`scrapbook`|
|ONNX paketleri|`keras2onnx`</br>`onnx`</br>`onnxconverter-common`</br>`skl2onnx`</br>`onnxmltools`|
|Azure Machine Learning Python & R SDK örnekleri||

Python paketlerinin tümü **Python 3.6 - AzureML** ortamında yüklenir.  

İşlem örnekleri genellikle geliştirme ortamları olarak kullanılır.  Ayrıca, geliştirme ve test için eğitim ve çıkarılan bir bilgi işlem hedefi olarak da kullanılabilir.  Büyük görevlerde, çok düğümlü ölçekleme özelliklerine sahip bir [Azure Machine Learning bilgi işlem kümesi](how-to-set-up-training-targets.md#amlcompute) daha iyi bir bilgi işlem hedef seçimidir.

### <a name="installing-packages"></a>Paketleri yükleme

Paketleri doğrudan bir Jupyter dizüstü bilgisayara veya Rstudio'ya yükleyebilirsiniz:

* RStudio Sağ alttaki **Paketler** sekmesini veya sol üstteki **Konsol** sekmesini kullanın.  
* Python: Yükle kodu ekleyin ve bir Jupyter dizüstü bilgisayar hücresinde çalıştırın.

Veya terminal penceresine şu yollardan herhangi birini erişebilirsiniz:

* RStudio: Sol üstteki **Terminal** sekmesini seçin.
* Jupyter Lab: Başlatıcı sekmesinde **Diğer** başlık altında **Terminal** döşemeseçin.
* Jupyter: Dosyalar sekmesinde sağ üstte **Yeni>Terminali'ni** seçin.
* SSH makineye.  Ardından Python paketlerini **Python 3.6 - AzureML** ortamına yükleyin.  R paketlerini **R** ortamına yükleyin.

## <a name="accessing-files"></a>Dosyalara erişim

Not defterleri ve R komut dosyaları, Azure dosya paylaşımında çalışma alanınızın varsayılan depolama hesabında depolanır.  Bu dosyalar "Kullanıcı dosyaları" dizininizin altında yer alır. Bu depolama, not defterlerini bilgi işlem örnekleri arasında paylaşmayı kolaylaştırır. Depolama hesabı, bir işlem örneğini durdurduğunuzda veya sildiğinizde not defterlerinizin güvenli bir şekilde korunmasını da sağlar.

Çalışma alanınızın Azure dosya paylaşım hesabı, bilgi işlem örneğinde sürücü olarak monte edilir. Bu sürücü Jupyter, Jupyter Labs ve RStudio için varsayılan çalışma dizinidir.

Dosya paylaşımındaki dosyalara aynı çalışma alanındaki tüm bilgi işlem örneklerinden erişilebilir. Bilgi işlem örneğinde bu dosyalarda yapılacak değişiklikler, dosya paylaşımına güvenilir bir şekilde geri kalıcı olarak devam edecektir.

Ayrıca, çalışma alanı dosya paylaşımındaki kullanıcı dosyaları dizininin altında en son Azure Machine Learning örneklerini klasörünüze kopyalayabilirsiniz.

Küçük dosyalar yazmak, ağ sürücülerinde VM'ye yazmaktan daha yavaş olabilir.  Çok sayıda küçük dosya yazıyorsanız, dizin gibi doğrudan işlem örneğinde `/tmp` bir dizin kullanmayı deneyin. Lütfen bu dosyalara çalışma alanındaki diğer bilgi işlem örneklerinden erişilmeyeceğini unutmayın.

## <a name="managing-a-compute-instance"></a>İşlem örneğini yönetme

Azure Machine Learning stüdyosundaki çalışma **alanınızda, Bilgi İşlem'i**seçin ve ardından en üstte **Bilgi İşlem Örneği'ni** seçin.

![İşlem örneğini yönetme](./media/concept-compute-instance/manage-compute-instance.png)

Aşağıdaki eylemleri gerçekleştirebilirsiniz:

* İşlem örneği oluşturma. GPU'lar da dahil olmak üzere adını, Azure VM türünü belirtin (lütfen VM türü oluşturulduktan sonra değiştirilemez), SSH erişimini etkinleştirin/devre dışı kullanabilirsiniz ve isteğe bağlı olarak sanal ağ ayarlarını yapılandırın. Ayrıca doğrudan tümleşik not defterlerinden, Azure portalından, Kaynak Yöneticisi şablonundan veya Azure Machine Learning SDK'dan bir örnek oluşturabilirsiniz. Bilgi işlem örneği oluşturma için geçerli olan bölge başına özel çekirdek kotası birleştirilir ve Azure Machine Learning bilgi işlem küme kotası ile paylaşılır.
* İşlem örnekleri sekmesini yenile
* İşlem örneğini başlatın, durdurun ve yeniden başlatın
* İşlem örneğini silme

Çalışma alanınızdaki her işlem örneği için şunları yapabilirsiniz:

* Erişim Jupyter, JupyterLab, RStudio bilgi işlem örneğinde
* SSH işlem örneğine. SSH erişimi varsayılan olarak devre dışı bırakılır, ancak bilgi işlem örneği oluşturma zamanında etkinleştirilebilir. SSH erişimi ortak/özel anahtar mekanizması üzerinden yapılır. Sekme, IP adresi, kullanıcı adı ve bağlantı noktası numarası gibi SSH bağlantısı yla ilgili ayrıntıları verir.
* IP adresi ve bölge gibi belirli bir bilgi işlem örneği hakkında ayrıntılı bilgi alın.

[RBAC,](/azure/role-based-access-control/overview) çalışma alanındaki hangi kullanıcıların bir bilgi işlem örneğini oluşturup, silebileceğini, başlatabileceğini, durdurabileceğini, yeniden başlatabileceğini denetlemenize olanak tanır. Çalışma alanı katılımcısı ve sahip rolündeki tüm kullanıcılar çalışma alanı boyunca bilgi işlem örnekleri oluşturabilir, silebilir, başlatabilir, durdurabilir ve yeniden başlatabilir. Ancak, belirli bir bilgi işlem örneğinin yalnızca yaratıcısı, bu bilgi işlem örneğinde Jupyter, JupyterLab ve RStudio'ya erişebilir. Bilgi işlem örneğinin yaratıcısı, onlara adanmış bilgi işlem örneğine sahiptir, kök erişimine sahiptir ve Jupyter aracılığıyla terminale geçebilir. İşlem örneği, içerik oluşturucu kullanıcının tek kullanıcılı girişine sahip olacak ve tüm eylemler bu kullanıcının kimliğini RBAC ve deneme çalıştırmalarının atıfta bulunularak kullanacaktır. SSH erişimi ortak/özel anahtar mekanizması ile kontrol edilir.

Ayrıca bir örnek oluşturabilirsiniz
* Doğrudan entegre dizüstü bilgisayar deneyiminden
* Azure portalında
* Azure Kaynak Yöneticisi şablonundan
* Azure Machine Learning SDK ile

Bilgi işlem örneği oluşturma için geçerli olan bölge kotası başına ayrılmış çekirdekler birleştirilir ve Azure Machine Learning eğitim küme kotası ile paylaşılır. 

## <a name="compute-target"></a>İşlem Hedefi

Bilgi işlem örnekleri, Azure Machine Learning bilgi işlem eğitim kümelerine benzer bir [eğitim bilgi işlem hedefi](concept-compute-target.md#train) olarak kullanılabilir. TensorFlow/PyTorch tahmincileri kullanarak dağıtılmış eğitim işlerini çalıştırmak için çoklu GPU VM sağlanması. Ayrıca bir çalıştırma yapılandırması oluşturabilir ve denemenizi işlem örneğinde çalıştırmak için kullanabilirsiniz. Test/hata ayıklama senaryoları için yerel bir çıkarıcı dağıtım hedefi olarak işlem örneğini kullanabilirsiniz.

## <a name="what-happened-to-notebook-vm"></a><a name="notebookvm"></a>Notebook VM'e ne oldu?

İşlem örnekleri, Not Defteri VM'sinin yerini alıyor.  

Çalışma alanı dosya paylaşımında depolanan tüm not defteri dosyalarına ve çalışma alanı veri depolarında bulunan verilere bir bilgi işlem örneğinden erişilebilir. Ancak, daha önce Bir Notebook VM'ye yüklenmiş özel paketlerin işlem örneğine yeniden yüklenmesi gerekir. İşlem kümeleri oluşturma için geçerli kota sınırlamaları, işlem örneği oluşturma için de geçerli olacaktır. 

Yeni Notebook VM'leri oluşturulamıyor. Ancak, oluşturduğunuz Dizüstü Bilgisayar'a tam işlevsellikle erişebilir ve bunları kullanabilirsiniz. İşlem örnekleri, varolan Notebook VM'leri ile aynı çalışma alanında oluşturulabilir. 


## <a name="next-steps"></a>Sonraki adımlar

 * [Öğretici: Tren ilk ML modeli](tutorial-1st-experiment-sdk-train.md) entegre bir dizüstü bilgisayar ile bir işlem örneği nasıl kullanılacağını gösterir.
