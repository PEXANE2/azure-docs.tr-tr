---
title: Azure not defterleri genel bakış
description: Jupyter not defterleri hiçbir Kurulum veya yapılandırma gerekli olduğu ücretsiz Azure not defterleri hizmetini kullanarak bulutta çalıştırın.
ms.topic: overview
ms.date: 04/05/2019
ms.openlocfilehash: b3dfa6cec962809fad1a03f5100c55315ff6a9ad
ms.sourcegitcommit: 653e9f61b24940561061bd65b2486e232e41ead4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2019
ms.locfileid: "74277677"
---
# <a name="overview-of-azure-notebooks"></a>Azure not defterleri genel bakış

Azure Notebooks, yükleme yapmadan Jupyter not defterlerini geliştirmeye ve çalıştırmaya yönelik, ücretsiz ve barındırılan bir hizmettir. [Jupyter](https://jupyter.org/) (eski adıyla IPython), markı metnini, çalıştırılabilir kodu, kalıcı verileri, grafikleri ve görselleştirmeleri tek bir paylaşılabilir tuval, *not defteri* (Jupyter.org Image hitap) üzerinde kolayca birleştirebilmenizi sağlayan açık kaynaklı bir projem ' dir:

[Jupi Not defterlerinin ![örnekleri](https://jupyter.org/assets/jupyterpreview.png)](https://jupyter.org/assets/jupyterpreview.png#lightbox)

Jupyter kodu, grafik ve açıklayıcı metin içeren bu güçlü birleşim nedeniyle, veri bilimi yönergesi, veri temizleme ve dönüştürme, sayısal bir simülasyon, modelleme ve geliştirmeye yönelik dahil olmak üzere pek çok kullanımı için popüler hale gelmiştir Makine öğrenimi modelleri.

## <a name="hassle-free-experience"></a>Sorunsuz bir deneyim

Azure not defterleri, kullanmaya başlama hakkında hızlı bir şekilde prototip oluşturma, veri bilimi, akademik araştırmasını veya Python programa öğrenme almak için yardımcı olur:

- Bir veri Bilimcisi yükleme tam Anaconda ortamıyla anında erişebilir.
- Bir Öğretmen Öğrenciler için sorunsuz bir Python ortamı sağlar.
- Bir benzeri bir sunum verebilirsiniz konuşma veya yazılım yükleme 45 dakika harcayabileceğiniz katılımcıları isteyen olmadan Web Semineri.
- Bir geliştirici ya da hobi bir hızlı kod karalama defteri not defterlerini kullanabilirsiniz.

Kişiler üzerinde Azure Not Defterleri (önizlemede) gibi tarayıcı erişilebilir bir bulut hizmeti aracılığıyla çalışabileceğiniz yaptığınızda not defterlerini daha güçlü hale gelir. Bulutta, kullanıcıların Jupyter yerel olarak yüklemez veya kendilerini bir ortam bakımıyla ilgili. Bulut ayrıca paylaşım not defterlerini (ve ilişkili veri dosyaları) diğer yetkili kullanıcılarla not defterleri ile kaynak denetimi depoları gibi dış anlamına gelir paylaşımı zorluklar önleme kolaylaştırır. Azure not defterleri ile kullanıcılar ayrıca kopyalama (veya "kopyalama") not defterleri için değişiklik ya da yönerge amaçları için özellikle yararlıdır deneme, kendi hesabına denetleyebilirsiniz.

Azure not defterleri yazma genel bir kod olduğundan, yürütme ve platform, paylaşım, çok sayıda çeşitli senaryolar için kullanabilirsiniz:

- Yeni bir programlama dili öğrenin – [FrontPage öğreticilerden](https://notebooks.azure.com/Microsoft/projects/samples/html/Introduction%20to%20Python.ipynb) birini deneyin
- Veri bilimi öğrenme – TRY [Jake VanderPlas ' Book](https://notebooks.azure.com/jakevdp/projects/PythonDataScienceHandbook)
- Yüzlerce öğrenci için [Kurs öğretin](https://notebooks.azure.com/garth-wells/projects/CUED-IA-Computing-Michaelmas)
- Yükleme zaman olmadan bir konferansta bir çevrimiçi olması veya bir Web seminerine verin 
- GitHub kullanıcılarının [bir GitHub başlatma rozet oluşturarak](https://notebooks.azure.com/help/projects/sharing/create-a-github-badge) not defterlerini doğrudan yüklemesini ve çalıştırmasını sağlama
- Slaytlara, slaytların çalıştırılabilir olduğu [Slayt gösterileri gibi PowerPoint](https://notebooks.azure.com/help/jupyter-notebooks/slides) sunun!

Kısacası, Azure not defterleri işlerinizi daha verimli bir şekilde gerçekleştirmek ve bu nedenle daha fazla bilgi elde etmenize yardımcı olur.

> [!Note]
> Jupi 'in kendisi hakkında daha fazla bilgi [Jupyter.org](https://jupyter.org/) ve [jupyıter belgelerinde](https://jupyter-notebook.readthedocs.io/en/latest/)bulunabilir.

## <a name="pricing-and-quotas"></a>Fiyatlandırma ve kotalar

Azure Notebooks ücretsiz bir hizmettir, ancak kötüye kullanımı engellemek için her proje 4 GB bellek ve 1 GB verilerle sınırlandırılmıştır. Bu sınırları aşan kullanıcıların dizüstü çalıştırmaya devam etmek için bir Captcha testini bakın.

Tüm limitleri serbest bırakmak için Azure Notebooks Azure Active Directory (örneğin, bir şirket hesabı) kullanarak bir hesapla oturum açın. Bu hesap bir Azure aboneliğiyle ilişkiliyse, bu abonelik içindeki tüm Azure Veri Bilimi Sanal Makinesi örneklerine bağlanabilirsiniz. Daha fazla bilgi için bkz. [projeleri yönetme ve yapılandırma-işlem katmanı](configure-manage-azure-notebooks-projects.md#compute-tier).

Not defteri sunucularının en fazla 8 saat içinde var olduğu garanti edilir. Çoğu durumda, Kapsayıcınız bu sınıra tabi değildir ve bu sürenin ötesinde çalışmaya devam eder, ancak uzun süreli oturumlar sistem kararlılığını sağlamak için zaman zaman kapatılabilir.

## <a name="available-kernels-and-environments"></a>Kullanılabilen çekirdekler ve ortamları

Her not defteri için herhangi bir kod hücreleri çalıştırmak için kullanılan çekirdek (diğer bir deyişle, çalışma zamanı ortamı) seçin. Azure not defterleri aşağıdaki çekirdeklere destekler:

- Python 2.7 + Anaconda2 5.3.0
- Python 3.6 + Anaconda3 5.3.0
- Python 3.5 + Anaconda3 4.2.0 (kullanım dışı bırakılacak)
- R 3.4.1 + Microsoft R Open 3.4.1
- F#4.1.9

Azure not defterleri, ayrıca temel dağıtımları ötesinde ek paketleri içerir. Python çekirdekler, örneğin numpy, pandas scikit-learn ve matplotlib ve bokeh kitaplıkları.

Ayrıca, o proje içinde tüm dizüstü bilgisayarlar için bir ortam oluşturmaya yönelik bir proje özelleştirebilirsiniz. Daha fazla bilgi için bkz. [hızlı başlangıç: özel bir ortamla proje oluşturma](quickstart-create-jupyter-notebook-project-environment.md).

Temel dağıtımların yanı sıra Azure not defterleri veri uzmanları için faydalı olan birçok ek paketlerin önceden yüklü olarak gelen. Ayrıca, her dil için tipik bir işlem kullanarak kendi paketlerini yükleyebilirsiniz.

## <a name="pre-configured-jupyter-extensions"></a>Önceden yapılandırılmış Jupyter uzantıları

Azure not defterleri ile aşağıdaki Jupyter uzantıları önceden yapılandırılmış:

- [RISE](https://github.com/damianavila/RISE): Jupyter slayt gösterisi uzantısı (live_reveal olarak da bilinir). Daha fazla bilgi için bkz. [Not defteri slayt gösterisi çalıştırma](present-jupyter-notebooks-slideshow.md).
- [Jupyterlab](https://github.com/jupyterlab/jupyterlab): Jupyter Not defterleri ile çalışmaya yönelik tam bir hesaplama ortamıdır.
- [Altair](https://github.com/ellisonbg/altair): Python için bildirime dayalı istatistiksel görselleştirme kitaplığı.
- [Bqçiz](https://github.com/bloomberg/bqplot): Jupyter Not defterleri için etkileşimli bir çizim çerçevesi.
- [Ipypencere öğeleri](https://github.com/jupyter-widgets/ipywidgets): Jupyter Not defterleri IÇIN etkileşimli HTML pencere öğeleri.

## <a name="issues-and-getting-help"></a>Sorunlar ve Yardım alma

Azure not defterleri hala Önizleme aşamasında olduğundan, hizmet daha sık veya diğer Azure Hizmetleri artık kalıcı olabilecek geçici kesinti yaşayabilirsiniz. Bazı özellikler eksik veya hatalar içeriyor.

Şu anda, iş açısından kritik uygulamalar veya hassas not defterlerini ve verileri için Azure not defterleri Önizleme kullanılmasından öneririz.

Azure Notebooks hakkındaki sorularınızı tartışmak için [GitHub deposunda](https://github.com/Microsoft/AzureNotebooks/issues)bir sorun verin.

## <a name="next-steps"></a>Sonraki adımlar  

- [Örnek not defterlerini keşfet](azure-notebooks-samples.md)

- Hızlı Başlangıçlar:

  - [Not defteri oluşturma ve paylaşma](quickstart-create-share-jupyter-notebook.md)
  - [Not Defterini Kopyala](quickstart-clone-jupyter-notebook.md)
  - [Yerel bir Jupyter Not defterini geçirme](quickstart-migrate-local-jupyter-notebook.md)
  - [Özel ortam kullanma](quickstart-create-jupyter-notebook-project-environment.md)
  - [Oturum açın ve bir kullanıcı KIMLIĞI ayarlayın](quickstart-sign-in-azure-notebooks.md)

- Öğreticiler:

  - [Not defteri oluşturma ve çalıştırma](tutorial-create-run-jupyter-notebook.md  )

- Nasıl yapılır makaleleri:
  
  - [Projeleri oluşturma ve kopyalama](create-clone-jupyter-notebooks.md)
  - [Projeleri yapılandırma ve yönetme](configure-manage-azure-notebooks-projects.md)
  - [Bir not defteri içinden paket yükler](install-packages-jupyter-notebook.md)
  - [Bir slayt gösterisi sunun](present-jupyter-notebooks-slideshow.md)
  - [Veri dosyalarıyla çalışma](work-with-project-data-files.md)
  - [Veri kaynaklarına erişin](access-data-resources-jupyter-notebooks.md)
  - [Azure Machine Learning kullan](use-machine-learning-services-jupyter-notebooks.md)
