---
title: Azure Machine Learning işlem örneği nedir?
titleSuffix: Azure Machine Learning
description: Tam olarak yönetilen bulut tabanlı bir iş istasyonu olan Azure Machine Learning işlem örneği hakkında bilgi edinin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: sgilley
author: sdgilley
ms.date: 12/13/2019
ms.openlocfilehash: 280851b2fea0b8100a7d0f8ec8105109a41c8c83
ms.sourcegitcommit: 79cbd20a86cd6f516acc3912d973aef7bf8c66e4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2020
ms.locfileid: "77252085"
---
# <a name="what-is-an-azure-machine-learning-compute-instance"></a>Azure Machine Learning işlem örneği nedir?

Azure Machine Learning işlem örneği (Önizleme), veri bilimcileri için tam olarak yönetilen bulut tabanlı bir iş istasyonudur. 

İşlem örnekleri, Azure Machine Learning geliştirmeye başlamanızı kolaylaştırır ve BT yöneticileri için yönetim ve kurumsal hazırlık özellikleri sağlar.  

Bulutta tam olarak yapılandırılmış ve yönetilen geliştirme ortamınız olarak bir işlem örneği kullanın.

İşlem örnekleri genellikle geliştirme ortamları olarak kullanılır.  Ayrıca, geliştirme ve test için bilgi işlem hedefi olarak da kullanılabilir.  Büyük görevler için çok düğümlü ölçekleme özelliklerine sahip bir [Azure Machine Learning işlem kümesi](how-to-set-up-training-targets.md#amlcompute) daha iyi bir işlem hedefi seçimleridir.


## <a name="why-use-a-compute-instance"></a>Neden bir işlem örneği kullanılmalıdır?

İşlem örneği, Machine Learning geliştirme ortamınız için iyileştirilmiş, tam olarak yönetilen bulut tabanlı bir iş istasyonudur. Aşağıdaki avantajları sağlar:

|Önemli avantajlar||
|----|----|
|Üretkenlik|Veri bilimcileri, tümleşik not defterlerini ve Web tarayıcılarında aşağıdaki araçları kullanarak modeller oluşturabilir ve dağıtabilir:<br/>-Jupyıter<br/>-Jupyıterlab<br/>-RStudio|
|Yönetilen & güvenli|Güvenlik ayak izini azaltın ve kurumsal güvenlik gereksinimleriyle uyumluluk ekleyin. İşlem örnekleri, güçlü yönetim ilkeleri ve güvenli ağ yapılandırmalarının yanı sıra şunları sağlar:<br/><br/>-Kaynak Yöneticisi şablonlarından veya Azure Machine Learning SDK 'dan otomatik sağlama<br/>- [rol tabanlı erişim denetimi (RBAC)](/azure/role-based-access-control/overview)<br/>- [sanal ağ desteği](how-to-enable-virtual-network.md#compute-instance)<br/>-SSH erişimini etkinleştirmek/devre dışı bırakmak için SSH ilkesi|
|Önceden yapılandırılmış&nbsp;veya&nbsp;ML|Önceden yapılandırılmış ve güncel ML paketleri, derin öğrenme çerçeveleri, GPU sürücüleri ile kurulum görevlerinde zaman kazanın.|
|Tamamen özelleştirilebilir|GPU 'Lar ve kalıcı alt düzey özelleştirme dahil olmak üzere Azure VM türleri için kapsamlı destek; Örneğin, paket ve sürücü yükleme, gelişmiş senaryolar için bir Breeze sağlar. |

## <a name="contents"></a>Araçlar ve ortamlar

Azure Machine Learning işlem örneği, çalışma alanınızdaki tamamen tümleşik bir not defteri deneyiminde modelleri yazmanıza, eğmenize ve dağıtmanıza olanak sağlar.


Bu araçlar ve ortamlar, işlem örneğine yüklenir: 

|Genel Araçlar & ortamları|Ayrıntılar|
|----|:----:|
|Sürücüler|`CUDA`</br>`cuDNN`</br>`NVIDIA`</br>`Blob FUSE` |
|Intel MPı kitaplığı||
|Azure CLI ||
|Azure Machine Learning örnekleri ||
|Azure Machine Learning EDAT altyapısı ||
|Docker||
|Nginx||
|NCCL 2,0 ||
|Protobuf|| 

|**R** araçları & ortamları|Ayrıntılar|
|----|:----:|
|RStudio Server açık kaynak sürümü||
|R çekirdeği||
|R için SDK Azure Machine Learning|[azuremlsdk](https://azure.github.io/azureml-sdk-for-r/reference/index.html)</br>SDK örnekleri|

|**Python** araçları & ortamları|Ayrıntılar|
|----|----|
|Anaconda Python||
|Jupi ve uzantıları||
|Jupi terlab ve uzantıları||
|Visual Studio Code ||
[Python için SDK Azure Machine Learning](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py)</br>Pypı 'den|`azureml-sdk[notebooks,contrib,automl,explain]`</br>`azureml-contrib-datadrift`</br>`azureml-telemetry`</br>`azureml-tensorboard`</br>`azureml-contrib-opendatasets`</br>`azureml-opendatasets`</br>`azureml-contrib-reinforcementlearning`</br>`azureml-mlflow`</br>`azureml-contrib-interpret` |
|Diğer Pypı paketleri|`jupytext`</br>`jupyterlab-git`</br>`tensorboard`</br>`nbconvert`</br>`notebook`</br>`Pillow`|
|Conda paketleri|`cython`</br>`numpy`</br>`ipykernel`</br>`scikit-learn`</br>`matplotlib`</br>`tqdm`</br>`joblib`</br>`nodejs`</br>`nb_conda_kernels`|
|Derin öğrenme paketleri|`PyTorch`</br>`TensorFlow`</br>`Keras`</br>`Horovod`</br>`MLFlow`</br>`pandas-ml`</br>`scrapbook`|
|ONNX paketleri|`keras2onnx`</br>`onnx`</br>`onnxconverter-common`</br>`skl2onnx`</br>`onnxmltools`|
|Azure Machine Learning Python & R SDK örnekleri||

Python paketlerinin hepsi **python 3,6-AzureML** ortamında yüklüdür.  

İşlem örnekleri genellikle geliştirme ortamları olarak kullanılır.  Ayrıca, geliştirme ve test için bilgi işlem hedefi olarak da kullanılabilir.  Büyük görevler için çok düğümlü ölçekleme özelliklerine sahip bir [Azure Machine Learning işlem kümesi](how-to-set-up-training-targets.md#amlcompute) daha iyi bir işlem hedefi seçimleridir.

### <a name="installing-packages"></a>Paketleri yükleme

Paketleri doğrudan bir Jupyter Not defteri 'ne veya rstudio 'Ya yükleyebilirsiniz:

* RStudio sağ alt köşedeki **paketler** sekmesini veya sol üstteki **konsol** sekmesini kullanın.  
* Python: bir Jupyter Not defteri hücresinde Install Code ve Execute özelliğini ekleyin.

Ya da bir Terminal penceresine şu yollarla erişebilirsiniz:

* RStudio: sol üst taraftaki **Terminal** sekmesini seçin.
* Jupyter Laboratuvarı: Başlatıcı sekmesinde **diğer** başlığın altında bulunan **Terminal** kutucuğunu seçin.
* Jupyter: Dosyalar sekmesinde sağ üstteki **yeni > Terminal** ' i seçin.
* Makineye SSH.  Ardından Python paketlerini **python 3,6-AzureML** ortamına yüklemeyin.  R paketlerini **r** ortamına yükler.

## <a name="accessing-files"></a>Dosyalara erişme

Not defterleri ve R betikleri, Azure dosya paylaşımında çalışma alanınızın varsayılan depolama hesabında depolanır.  Bu dosyalar "Kullanıcı dosyaları" dizininizin altında bulunur. Bu depolama, işlem örnekleri arasında Not defterlerinin paylaşılmasını kolaylaştırır. Depolama hesabı Ayrıca bir işlem örneğini durdurduğunuzda veya sildiğinizde Not defterlerinizin güvenle korunmalarını önler.

Çalışma alanınızın Azure dosya paylaşma hesabı, işlem örneğine bir sürücü olarak bağlanır. Bu sürücü, jupi, Jupyıter Labs ve RStudio için varsayılan çalışma dizinidir.

Dosya paylaşımındaki dosyalara aynı çalışma alanındaki tüm işlem örneklerinden erişilebilir. İşlem örneğindeki bu dosyalarda yapılan tüm değişiklikler, dosya paylaşımında güvenilir bir şekilde kalıcı olarak kalıcı hale getirilir.

Ayrıca, en son Azure Machine Learning örnekleri çalışma alanı dosya paylaşımındaki Kullanıcı dosyaları dizini altında klasörünüze kopyalayabilirsiniz.

Küçük dosyaların yazılması, ağ sürücülerinde VM 'nin kendine yazılmasından daha yavaş olabilir.  Çok sayıda küçük dosya yazıyorsanız, bir dizin `/tmp` bir dizin gibi doğrudan işlem örneğinde kullanmayı deneyin. Lütfen bu dosyalara çalışma alanındaki diğer işlem örneklerinden erişilemeyecek.

## <a name="managing-a-compute-instance"></a>İşlem örneğini yönetme

Azure Machine Learning Studio 'daki çalışma alanınızda **işlem**' ı seçin ve ardından en üstteki **işlem örneği** ' ni seçin.

![İşlem örneğini yönetme](./media/concept-compute-instance/manage-compute-instance.png)

Aşağıdaki eylemleri gerçekleştirebilirsiniz:

* Bir işlem örneği oluşturun. GPU 'Lar dahil olmak üzere Azure VM türünü belirtin (lütfen not VM türü oluşturulduktan sonra değiştirilemez), SSH erişimini etkinleştirin/devre dışı bırakın ve sanal ağ ayarlarını isteğe bağlı olarak yapılandırın. Ayrıca, tümleşik not defterlerinden, Azure portal, Kaynak Yöneticisi şablondan veya Azure Machine Learning SDK 'dan doğrudan bir örnek oluşturabilirsiniz. İşlem örneği oluşturma için geçerli olan bölge başına adanmış çekirdekler, Azure Machine Learning işlem kümesi kotasıyla birleştirilmiştir ve paylaşılır.
* İşlem örnekleri sekmesini Yenile
* İşlem örneğini başlatma, durdurma ve yeniden başlatma
* İşlem örneğini silme

Çalışma alanınızdaki her bir işlem örneği için şunları yapabilirsiniz:

* Jupyıter, Jupiterlab, RStudio 'yu işlem örneği üzerinde erişme
* İşlem örneğine SSH. SSH erişimi varsayılan olarak devre dışıdır ancak işlem örneği oluşturma sırasında etkinleştirilebilir. SSH erişimi, ortak/özel anahtar mekanizmasıyla gerçekleştirilir. Sekmesi, IP adresi, Kullanıcı adı ve bağlantı noktası numarası gibi SSH bağlantısı için Ayrıntılar verecektir.
* IP adresi ve bölge gibi belirli bir işlem örneği hakkındaki ayrıntıları alın.

[RBAC](/azure/role-based-access-control/overview) , çalışma alanındaki hangi kullanıcıların bir bilgi işlem örneği oluşturabileceğinizi, silebileceği, başlatabileceği, durdurabileceğinizi denetlemenize olanak tanır. Çalışma alanı katılımcısı ve sahip rolündeki tüm kullanıcılar çalışma alanı genelinde işlem örnekleri oluşturabilir, silebilir, başlatabilir, durdurabilir ve yeniden başlatabilir. Ancak, yalnızca belirli bir işlem örneğinin Oluşturucusu, bu işlem örneği üzerinde Jupiter, Jupiterlab ve RStudio erişimine izin verilir. İşlem örneğinin Oluşturucusu kendisine adanmış bir işlem örneğine sahiptir, kök erişimi vardır ve Jupyıter aracılığıyla Terminal üzerinde oturum açabilir. İşlem örneği, Oluşturucu kullanıcı için tek kullanıcılı oturum açmaya sahip olur ve tüm eylemler bu kullanıcının kimliğini, Deneme çalıştırmalarının RBAC ve atısyonu için kullanır. SSH erişimi, ortak/özel anahtar mekanizması aracılığıyla denetlenir.

Ayrıca, bir örnek oluşturabilirsiniz
* Doğrudan tümleşik Not defteri deneyiminden
* Azure portal
* Azure Resource Manager şablondan
* Azure Machine Learning SDK ile

İşlem örneği oluşturma için geçerli olan bölge başına adanmış çekirdekler, Azure Machine Learning eğitim kümesi kotasıyla birleştirilmiştir ve paylaşılır. 

## <a name="compute-target"></a>İşlem hedefi

İşlem örnekleri, Azure Machine Learning işlem eğitimi kümelerine benzer bir [eğitim işlem hedefi](concept-compute-target.md#train) olarak kullanılabilir. TensorFlow/PyTorch estimators kullanarak dağıtılmış eğitim işlerini çalıştırmak için bir çok GPU VM sağlayın. Ayrıca, bir çalıştırma yapılandırması oluşturup bu yapılandırmayı kullanarak denemenizin işlem örneği üzerinde çalıştırılmasını sağlayabilirsiniz. Test/hata ayıklama senaryoları için, işlem örneğini yerel bir ınırm dağıtım hedefi olarak kullanabilirsiniz.

## <a name="notebookvm"></a>Not defteri VM 'sine ne oldu?

İşlem örnekleri, Not defteri VM 'sini değiştiriyor.  

Çalışma alanı dosya paylaşımında depolanan tüm not defteri dosyalarına ve çalışma alanı veri depolarındaki verilere bir işlem örneğinden erişilebilecektir. Ancak, daha önce bir not defteri sanal makinesine yüklenmiş olan tüm özel paketlerin, işlem örneğinde yeniden yüklenmesi gerekir. İşlem kümeleri oluşturma için uygulanan kota sınırlamaları, işlem örneği oluşturma için de geçerlidir. 

Yeni Not defteri VM 'Leri oluşturulamıyor. Ancak, tam işlevsellikle oluşturmuş olduğunuz not defteri VM 'lerine erişmeye ve bunları kullanmaya devam edebilirsiniz. İşlem örnekleri, mevcut not defteri VM 'leriyle aynı çalışma alanında oluşturulabilir. 


## <a name="next-steps"></a>Sonraki adımlar

 * [Öğretici: Ilk ml modelinize eğitme](tutorial-1st-experiment-sdk-train.md) bir işlem örneğinin tümleşik bir not defteriyle nasıl kullanılacağını gösterir.
