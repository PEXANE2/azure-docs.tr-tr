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
ms.date: 06/22/2020
ms.openlocfilehash: 3edb8da6d535ac7817c29db254aa84efd92321b6
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87012986"
---
# <a name="what-is-an-azure-machine-learning-compute-instance"></a>Azure Machine Learning işlem örneği nedir?

Azure Machine Learning işlem örneği, veri bilimcileri için yönetilen bulut tabanlı bir iş istasyonudur.

İşlem örnekleri, Azure Machine Learning geliştirmeye başlamanızı kolaylaştırır ve BT yöneticileri için yönetim ve kurumsal hazırlık özellikleri sağlar.  

Makine öğrenimi için bulutta tam olarak yapılandırılmış ve yönetilen geliştirme ortamınız olarak bir işlem örneği kullanın. Ayrıca, geliştirme ve test amacıyla bilgi işlem hedefi olarak da kullanılabilir.  

Üretim sınıfı modeli eğitimi için çok düğümlü ölçeklendirme özelliklerine sahip bir [Azure Machine Learning işlem kümesi](how-to-set-up-training-targets.md#amlcompute) kullanın. Üretim sınıfı modeli dağıtımı için [Azure Kubernetes hizmet kümesi](how-to-deploy-azure-kubernetes-service.md)' ni kullanın.

## <a name="why-use-a-compute-instance"></a>Neden bir işlem örneği kullanılmalıdır?

İşlem örneği, Machine Learning geliştirme ortamınız için iyileştirilmiş, tam olarak yönetilen bulut tabanlı bir iş istasyonudur. Aşağıdaki avantajları sağlar:

|Önemli avantajlar|Açıklama|
|----|----|
|Üretkenlik|Tümleşik not defterlerini ve Azure Machine Learning Studio 'daki aşağıdaki araçları kullanarak modeller oluşturabilir ve dağıtabilirsiniz:<br/>-Jupyıter<br/>-Jupyıterlab<br/>-RStudio (Önizleme)<br/>İşlem örneği, Azure Machine Learning çalışma alanı ve Studio ile tamamen tümleşiktir. Çalışma alanındaki diğer veri bilimcileri ile not defterlerini ve verileri paylaşabilirsiniz. [SSH](how-to-set-up-vs-code-remote.md) kullanarak uzaktan geliştirme vs Code de ayarlayabilirsiniz |
|Yönetilen & güvenli|Güvenlik ayak izini azaltın ve kurumsal güvenlik gereksinimleriyle uyumluluk ekleyin. İşlem örnekleri, güçlü yönetim ilkeleri ve güvenli ağ yapılandırmalarının yanı sıra şunları sağlar:<br/><br/>-Kaynak Yöneticisi şablonlarından veya Azure Machine Learning SDK 'dan otomatik sağlama<br/>- [Rol tabanlı erişim denetimi (RBAC)](/azure/role-based-access-control/overview)<br/>- [Sanal ağ desteği](how-to-enable-virtual-network.md#compute-instance)<br/>-SSH erişimini etkinleştirmek/devre dışı bırakmak için SSH ilkesi<br/>TLS 1,2 etkin |
|&nbsp;Ml için önceden yapılandırılmış &nbsp;|Önceden yapılandırılmış ve güncel ML paketleri, derin öğrenme çerçeveleri, GPU sürücüleri ile kurulum görevlerinde zaman kazanın.|
|Tamamen özelleştirilebilir|GPU 'Lar ve kalıcı alt düzey özelleştirme dahil olmak üzere Azure VM türleri için kapsamlı destek; Örneğin, paket ve sürücü yükleme, gelişmiş senaryolar için bir Breeze sağlar. |

## <a name="tools-and-environments"></a><a name="contents"></a>Araçlar ve ortamlar

> [!IMPORTANT]
> Aşağıda işaretlenmiş araçlar (Önizleme) Şu anda genel önizlemede.
> Önizleme sürümü, bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Azure Machine Learning işlem örneği, çalışma alanınızdaki tamamen tümleşik bir not defteri deneyiminde modelleri yazmanıza, eğmenize ve dağıtmanıza olanak sağlar.

Bu araçlar ve ortamlar, işlem örneğine yüklenir: 

|Genel araçlar & ortamları|Ayrıntılar|
|----|:----:|
|Sürücüler|`CUDA`</br>`cuDNN`</br>`NVIDIA`</br>`Blob FUSE` |
|Intel MPı kitaplığı||
|Azure CLI ||
|Azure Machine Learning örnekleri ||
|Docker||
|Nginx||
|NCCL 2,0 ||
|Protobuf|| 

|**R** araçları & ortamları|Ayrıntılar|
|----|:----:|
|RStudio Server açık kaynak sürümü (Önizleme)||
|R çekirdeği||
|R için SDK Azure Machine Learning|[azuremlsdk](https://azure.github.io/azureml-sdk-for-r/reference/index.html)</br>SDK örnekleri|

|**Python** araçları & ortamları|Ayrıntılar|
|----|----|
|Anaconda Python||
|Jupi ve uzantıları||
|Jupi terlab ve uzantıları||
[Python için Azure Machine Learning SDK'sı](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py)</br>Pypı 'den|, Azureml ek paketlerin çoğunu içerir.  Tam listeyi görmek için, [işlem örneğiniz üzerinde bir Terminal penceresi açın](how-to-run-jupyter-notebooks.md#terminal) ve şunu çalıştırın: <br/> `conda list -n azureml_py36 azureml*` |
|Diğer Pypı paketleri|`jupytext`</br>`tensorboard`</br>`nbconvert`</br>`notebook`</br>`Pillow`|
|Conda paketleri|`cython`</br>`numpy`</br>`ipykernel`</br>`scikit-learn`</br>`matplotlib`</br>`tqdm`</br>`joblib`</br>`nodejs`</br>`nb_conda_kernels`|
|Derin öğrenme paketleri|`PyTorch`</br>`TensorFlow`</br>`Keras`</br>`Horovod`</br>`MLFlow`</br>`pandas-ml`</br>`scrapbook`|
|ONNX paketleri|`keras2onnx`</br>`onnx`</br>`onnxconverter-common`</br>`skl2onnx`</br>`onnxmltools`|
|Azure Machine Learning Python & R SDK örnekleri||

Python paketlerinin hepsi **python 3,6-AzureML** ortamında yüklüdür.  

### <a name="installing-packages"></a>Paketleri yükleme

Paketleri doğrudan bir Jupyter Not defteri 'ne veya rstudio 'Ya yükleyebilirsiniz:

* RStudio sağ alt köşedeki **paketler** sekmesini veya sol üstteki **konsol** sekmesini kullanın.  
* Python: bir Jupyter Not defteri hücresinde Install Code ve Execute özelliğini ekleyin.

Ya da bir Terminal penceresine şu yollarla erişebilirsiniz:

* RStudio: sol üst taraftaki **Terminal** sekmesini seçin.
* Jupyter Laboratuvarı: Başlatıcı sekmesinde **diğer** başlığın altında bulunan **Terminal** kutucuğunu seçin.
* Jupyter: Dosyalar sekmesinde sağ üstteki **yeni>Terminal** ' i seçin.
* Makineye SSH.  Ardından Python paketlerini **python 3,6-AzureML** ortamına yüklemeyin.  R paketlerini **r** ortamına yükler.

## <a name="accessing-files"></a>Dosyalara erişme

Not defterleri ve R betikleri, Azure dosya paylaşımında çalışma alanınızın varsayılan depolama hesabında depolanır.  Bu dosyalar "Kullanıcı dosyaları" dizininizin altında bulunur. Bu depolama, işlem örnekleri arasında Not defterlerinin paylaşılmasını kolaylaştırır. Depolama hesabı Ayrıca bir işlem örneğini durdurduğunuzda veya sildiğinizde Not defterlerinizin güvenle korunmalarını önler.

Çalışma alanınızın Azure dosya paylaşma hesabı, işlem örneğine bir sürücü olarak bağlanır. Bu sürücü, jupi, Jupyıter Labs ve RStudio için varsayılan çalışma dizinidir. Diğer bir deyişle, Jupiter, Jupiterlab veya RStudio 'da oluşturduğunuz Not defterleri ve diğer dosyalar otomatik olarak dosya paylaşımında depolanır ve diğer işlem örneklerinde kullanılabilir.

Dosya paylaşımındaki dosyalara aynı çalışma alanındaki tüm işlem örneklerinden erişilebilir. İşlem örneğindeki bu dosyalarda yapılan tüm değişiklikler, dosya paylaşımında güvenilir bir şekilde kalıcı olarak kalıcı hale getirilir.

Ayrıca, en son Azure Machine Learning örnekleri çalışma alanı dosya paylaşımındaki Kullanıcı dosyaları dizini altında klasörünüze kopyalayabilirsiniz.

Küçük dosyaların yazılması, ağ sürücülerinde, işlem örneği yerel diskinin üzerine yazılmasından daha yavaş olabilir.  Çok sayıda küçük dosya yazıyorsanız, Dizin gibi işlem örneğinde doğrudan bir dizin kullanmayı deneyin `/tmp` . Lütfen bu dosyalara diğer işlem örneklerinden erişilemeyecek. 

`/tmp`Geçici verileriniz için işlem örneğindeki dizini kullanabilirsiniz.  Ancak, işlem örneğinin işletim sistemi diskine büyük veri dosyaları eklemeyin.  Bunun yerine [veri depoları](concept-azure-machine-learning-architecture.md#datasets-and-datastores) kullanın. Jupyıterlab git uzantısını yüklediyseniz, işlem örneği performansının yavaşmasına de yol açabilir.

## <a name="managing-a-compute-instance"></a>İşlem örneğini yönetme

Azure Machine Learning Studio 'daki çalışma alanınızda **işlem**' ı seçin ve ardından en üstteki **işlem örneği** ' ni seçin.

![İşlem örneğini yönetme](./media/concept-compute-instance/manage-compute-instance.png)

Aşağıdaki eylemleri gerçekleştirebilirsiniz:

* [Bir işlem örneği oluşturun](#create). 
* İşlem örnekleri sekmesini yenileyin.
* Bir işlem örneğini başlatın, durdurun ve yeniden başlatın.  Her çalıştığında örnek için ödeme yaparsınız. Maliyeti azaltmak için kullanmıyorsanız, işlem örneğini durdurun. Bir işlem örneğinin durdurulması onu kaldırır. Daha sonra ihtiyacınız olduğunda yeniden başlatın. 
* Bir işlem örneğini silin.
* İşlem örnekleri listesini, oluşturduğunuz olanlarla filtreleyin.  Bunlar, erişebileceğiniz işlem örnekleridir.

Erişiminiz olan çalışma alanınızdaki her bir işlem örneği için şunları yapabilirsiniz:

* Jupyıter, Jupiterlab, RStudio 'yu işlem örneği üzerinde erişme
* İşlem örneğine SSH. SSH erişimi varsayılan olarak devre dışıdır ancak işlem örneği oluşturma sırasında etkinleştirilebilir. SSH erişimi, ortak/özel anahtar mekanizmasıyla gerçekleştirilir. Sekmesi, IP adresi, Kullanıcı adı ve bağlantı noktası numarası gibi SSH bağlantısı için Ayrıntılar verecektir.
* IP adresi ve bölge gibi belirli bir işlem örneği hakkındaki ayrıntıları alın.

[RBAC](/azure/role-based-access-control/overview) , çalışma alanındaki hangi kullanıcıların bir bilgi işlem örneği oluşturabileceğinizi, silebileceği, başlatabileceği, durdurabileceğinizi denetlemenize olanak tanır. Çalışma alanı katılımcısı ve sahip rolündeki tüm kullanıcılar çalışma alanı genelinde işlem örnekleri oluşturabilir, silebilir, başlatabilir, durdurabilir ve yeniden başlatabilir. Ancak, yalnızca belirli bir işlem örneğinin Oluşturucusu, bu işlem örneği üzerinde Jupiter, Jupiterlab ve RStudio erişimine izin verilir. İşlem örneğinin Oluşturucusu kendisine adanmış bir işlem örneğidir, kök erişimi vardır ve jupi/Jupiterlab/RStudio aracılığıyla oturum açabilir. İşlem örneği, Oluşturucu kullanıcı için tek kullanıcılı oturum açmaya sahip olur ve tüm eylemler bu kullanıcının kimliğini, Deneme çalıştırmalarının RBAC ve atısyonu için kullanır. SSH erişimi, ortak/özel anahtar mekanizması aracılığıyla denetlenir.

Bu eylemler RBAC tarafından denetlenebilir:
* *Microsoft. MachineLearningServices/çalışma alanları/hesaplar/okundu*
* *Microsoft. MachineLearningServices/çalışma alanları/hesaplar/yaz*
* *Microsoft. MachineLearningServices/çalışma alanları/hesaplar/Sil*
* *Microsoft. MachineLearningServices/çalışma alanları/hesaplar/Başlat/eylem*
* *Microsoft. MachineLearningServices/Workspaces/hesaplar/durdur/eylem*
* *Microsoft. MachineLearningServices/Workspaces/hesaplar/yeniden Başlat/eylem*

### <a name="create-a-compute-instance"></a><a name="create"></a>İşlem örneği oluşturma

Azure Machine Learning Studio 'daki çalışma alanınızda, Not defterlerinizden birini çalıştırmaya hazırsanız **işlem** bölümünden veya **Not defterleri** bölümünde yeni bir işlem örneği oluşturun.

:::image type="content" source="media/concept-compute-instance/create-compute-instance.png" alt-text="Yeni bir işlem örneği oluştur":::


|Alan  |Açıklama  |
|---------|---------|
|İşlem adı     |  <li>Ad gereklidir ve 3 ila 24 karakter uzunluğunda olmalıdır.</li><li>Geçerli karakterler büyük ve küçük harfler, rakamlar ve **-** karakterdir.</li><li>Ad bir harfle başlamalıdır</li><li>Adın, bir Azure bölgesindeki tüm mevcut hesaplar arasında benzersiz olması gerekir. Seçtiğiniz ad benzersiz değilse bir uyarı görürsünüz</li><li>**-** Karakter kullanılıyorsa, daha sonra adının sonunda en az bir harf gelmelidir</li>     |
|Sanal makine türü |  CPU veya GPU seçin. Bu tür, oluşturulduktan sonra değiştirilemez     |
|Sanal makine boyutu     |  Desteklenen sanal makine boyutları bölgenizde kısıtlanmış olabilir. [Kullanılabilirlik listesini](https://azure.microsoft.com/global-infrastructure/services/?products=virtual-machines) denetleme     |
|SSH erişimini etkinleştirme/devre dışı bırakma     |   SSH erişimi varsayılan olarak devre dışıdır.  SSH erişimi olamaz. oluşturulduktan sonra değiştirildi. [Vs Code uzak](how-to-set-up-vs-code-remote.md) ile etkileşimli olarak hata ayıklamayı planlıyorsanız erişimi etkinleştirdiğinizden emin olun   |
|Gelişmiş ayarlar     |  İsteğe bağlı. Bir sanal ağ yapılandırın. Bir Azure sanal ağı (VNet) içinde işlem örneği oluşturmak için **kaynak grubu**, **sanal ağ**ve **alt ağ** belirtin. Daha fazla bilgi için, VNET için bu [ağ gereksinimlerine](how-to-enable-virtual-network.md#compute-instance) bakın.        |

Ayrıca, bir örnek oluşturabilirsiniz
* Doğrudan [Tümleşik Not defteri deneyiminden](tutorial-1st-experiment-sdk-setup.md#azure)
* Azure portal
* Azure Resource Manager şablondan
* [Azure MACHINE LEARNING SDK](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training/train-on-computeinstance/train-on-computeinstance.ipynb) ile
* [Azure Machine Learning Için CLI uzantısı](reference-azure-machine-learning-cli.md#computeinstance)

VM ailesi kotası başına bölge başına adanmış çekirdek ve işlem örneği oluşturma için geçerli olan toplam bölgesel kota. Birleşik ve Azure Machine Learning eğitim işlem kümesi kotasıyla paylaşılır. İşlem örneği durdurulduğunda, işlem örneğini yeniden başlatabileceksiniz emin olmak için kota serbest bırakılır.

## <a name="compute-target"></a>İşlem hedefi

İşlem örnekleri, Azure Machine Learning işlem eğitimi kümelerine benzer bir [eğitim işlem hedefi](concept-compute-target.md#train) olarak kullanılabilir. 

İşlem örneği:
* İş kuyruğu vardır.
* Bir sanal ağ ortamında, kuruluşların SSH bağlantı noktasını açmasına gerek olmadan işleri güvenli bir şekilde çalıştırır. İş kapsayıcılı bir ortamda yürütülür ve model bağımlılıklarınızı bir Docker kapsayıcısında paketleyebilir.
* Birden çok küçük işi paralel olarak çalıştırabilir (Önizleme).  Çekirdek başına iki iş paralel olarak çalışabilir, ancak işlerin geri kalanı sıraya alınır.

Test/hata ayıklama senaryoları için, işlem örneğini yerel bir ınırm dağıtım hedefi olarak kullanabilirsiniz.

> [!NOTE]
> Dağıtılmış eğitim işleri, işlem örneği üzerinde desteklenmez.  Dağıtılmış eğitim için (işlem kümeleri] (nasıl yapılır-ayarla---------------------

Daha ayrıntılı bilgi için bkz. [computeinstance-on-](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training/train-on-computeinstance/train-on-computeinstance.ipynb)Not Bu not defteri, *eğitim/eğitme-on-computeinstance*Içindeki Studio **örnekleri** klasöründe de mevcuttur.

## <a name="what-happened-to-notebook-vm"></a><a name="notebookvm"></a>Not defteri VM 'sine ne oldu?

İşlem örnekleri, Not defteri VM 'sini değiştiriyor.  

Çalışma alanı dosya paylaşımında depolanan tüm not defteri dosyalarına ve çalışma alanı veri depolarındaki verilere bir işlem örneğinden erişilebilecektir. Ancak, daha önce bir not defteri sanal makinesine yüklenmiş olan tüm özel paketlerin, işlem örneğinde yeniden yüklenmesi gerekir. İşlem kümeleri oluşturma için uygulanan kota sınırlamaları, işlem örneği oluşturma için de geçerlidir.

Yeni Not defteri VM 'Leri oluşturulamıyor. Ancak, tam işlevsellikle oluşturmuş olduğunuz not defteri VM 'lerine erişmeye ve bunları kullanmaya devam edebilirsiniz. İşlem örnekleri, mevcut not defteri VM 'leriyle aynı çalışma alanında oluşturulabilir.


## <a name="next-steps"></a>Sonraki adımlar

 * [Öğretici: Ilk ml modelinize eğitme](tutorial-1st-experiment-sdk-train.md) bir işlem örneğinin tümleşik bir not defteriyle nasıl kullanılacağını gösterir.
