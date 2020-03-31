---
title: Azure Not Defterleri Önizlemesi'ne Genel Bakış
description: Kurulum veya yapılandırma gerektirmediği ücretsiz Azure Notebook Önizleme hizmetini kullanarak Jupyter dizüstü bilgisayarları bulutta çalıştırın.
ms.topic: overview
ms.date: 04/05/2019
ms.openlocfilehash: bd2355bdefcedca5026a25915dc1da55fdc33a36
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "75646339"
---
# <a name="overview-of-azure-notebooks-preview"></a>Azure Not Defterleri Önizlemesi'ne Genel Bakış

Azure Notebooks, yükleme yapmadan Jupyter not defterlerini geliştirmeye ve çalıştırmaya yönelik, ücretsiz ve barındırılan bir hizmettir. [Jupyter](https://jupyter.org/) (eski adıyla IPython), Markdown metnini, çalıştırılabilir kodu, kalıcı verileri, grafikleri ve görselleştirmeleri tek, sharable tuval, not defteri (jupyter.org görüntü nezaketi üzerine kolayca birleştirmenizi sağlayan açık kaynak *kodlu* bir projedir):

[![Jupyter dizüstü bilgisayar örnekleri](https://jupyter.org/assets/jupyterpreview.png)](https://jupyter.org/assets/jupyterpreview.png#lightbox)

Kod, grafik ve açıklayıcı metnin bu güçlü birleşimi sayesinde, Jupyter veri bilimi öğretimi, veri temizleme ve dönüştürme, sayısal simülasyon, istatistiksel modelleme ve makine öğrenme modelleri.

[!INCLUDE [notebooks-status](../../includes/notebooks-status.md)]

## <a name="hassle-free-experience"></a>Sorunsuz deneyim

Azure Dizüstü Bilgisayarlar, prototip oluşturma, veri bilimi, akademik araştırma veya Python programlamayı öğrenme konusunda hızlı bir şekilde başlamanıza yardımcı olur:

- Bir veri bilimcisi kurulum olmadan tam bir Anaconda ortamına anında erişebilir.
- Bir öğretmen öğrencilere sorunsuz bir Python ortamı sağlayabilir.
- Sunucu, katılımcılardan yazılım yükleyerek 45 dakika harcamalarını istemeden benzer bir konuşma veya web semineri verebilir.
- Bir geliştirici veya hobisi hızlı kod scratchpad olarak Dizüstü bilgisayarlar kullanabilirsiniz.

Dizüstü bilgisayarlar, azure not defterleri gibi tarayıcıtarafından erişilebilen bir bulut hizmeti aracılığıyla (Önizleme'de) insanlar onlarla işbirliği yaptığında daha da güçlü hale gelir. Bulutta, kullanıcıların Jupyter'ı yerel olarak yüklemeleri veya ortamı korumakla ilgilenmeleri gerekmez. Bulut ayrıca, not defterlerini (ve ilişkili veri dosyalarını) diğer yetkili kullanıcılarla paylaşmayı da kolaylaştırır ve kaynak kontrol depoları gibi dış yollarla not defterlerini paylaşmanın komplikasyonlarını önler. Azure Not Defterleri ile kullanıcılar, özellikle talimat amacıyla kullanışlı olan değişiklik veya deneme için not defterlerini kendi hesaplarında kopyalayabilir (veya "klonlayabilir").

Azure Not Defterleri genel bir kod yazma, yürütme ve paylaşım platformu olduğundan, bunu birçok farklı senaryoiçin kullanabilirsiniz:

- Yeni bir programlama dili öğrenin – [ön sayfa eğitimlerinden](https://notebooks.azure.com/Microsoft/projects/samples/html/Introduction%20to%20Python.ipynb) birini deneyin
- Veri Bilimi öğrenin - [Jake VanderPlas 'kitap](https://notebooks.azure.com/jakevdp/projects/PythonDataScienceHandbook) deneyin
- Yüzlerce öğrenciye [kurs öğretmek](https://notebooks.azure.com/garth-wells/projects/CUED-IA-Computing-Michaelmas)
- Kurulumda zaman harcamadan çevrimiçi veya bir konferansta web semineri verin 
- GitHub kullanıcılarının [Bir GitHub başlatma rozeti oluşturarak](https://notebooks.azure.com/help/projects/sharing/create-a-github-badge) dizüstü bilgisayarları doğrudan yüklemesini ve çalıştırmasını sağlama
- Slaytlarda kodun yürütülebileceği [slayt gösterileri gibi PowerPoint](https://notebooks.azure.com/help/jupyter-notebooks/slides) verin!

Kısacası, Azure Dizüstü Bilgisayarlar işinizi daha verimli bir şekilde gerçekleştirmenize ve böylece daha fazlasını elde eve ulaşmanıza yardımcı olur.

> [!Note]
> Jupyter kendisi hakkında daha fazla bilgi [jupyter.org](https://jupyter.org/) ve [Jupyter belgelerde](https://jupyter-notebook.readthedocs.io/en/latest/)bulunabilir.

## <a name="pricing-and-quotas"></a>Fiyatlandırma ve kotalar

Azure Dizüstü Bilgisayarlar ücretsiz bir hizmettir, ancak kötüye kullanımı önlemek için her proje 4 GB bellek ve 1GB veriile sınırlıdır. Bu sınırları aşan yasal kullanıcılar, dizüstü bilgisayarları çalıştırmaya devam etmek için Captcha zorluğunu görür.

Tüm sınırları serbest bırakmak için Azure Active Directory (şirket hesabı gibi) kullanarak bir hesapla Azure Not Defteri'nde oturum açın. Bu hesap bir Azure aboneliğiyle ilişkiliyse, bu abonelik içindeki tüm Azure Veri Bilimi Sanal Makine örneklerine bağlanabilirsiniz. Daha fazla bilgi için [projeleri yönet ve yapılandırma - Katmanı hesapla.](configure-manage-azure-notebooks-projects.md#compute-tier)

Dizüstü bilgisayar sunucularının en fazla 8 saat boyunca var olması garanti edilir. Çoğu durumda, kapsayıcınız bu sınıra tabi değildir ve bu sürenin ötesine devam eder, ancak uzun süreli oturumlar bazen sistem kararlılığı için kapatılabilir.

## <a name="available-kernels-and-environments"></a>Kullanılabilir çekirdekler ve ortamlar

Her not defteri için, herhangi bir kod hücresini çalıştırmak için kullanılan çekirdeği (diğer bir şekilde çalışma zamanı ortamı) seçersiniz. Azure Not Defterleri aşağıdaki çekirdekleri destekler:

- Python 2.7 + Anaconda2-5.3.0
- Python 3.6 + Anaconda3-5.3.0
- Python 3.5 + Anaconda3-4.2.0 (amortismana alınacaktır)
- R 3.4.1 + Microsoft R Açık 3.4.1
- F# 4.1.9

Azure Not Defterleri, temel dağıtımların ötesinde ekstra paketler de içerir. Python çekirdekleri, örneğin, numpy, pandalar, scikit-learn, matplotlib ve bokeh kütüphaneleri içerir.

Ayrıca, projedeki tüm not defterleri için bir ortam oluşturmak için projeyi özelleştirebilirsiniz. Daha fazla bilgi için [Bkz. Hızlı Başlangıç: Özel bir ortama sahip bir proje oluşturun.](quickstart-create-jupyter-notebook-project-environment.md)

Azure Dizüstü Bilgisayarlar, temel dağıtımlara ek olarak, veri bilimciler için yararlı olan birçok ek paketle birlikte önceden yüklenmiş olarak gelir. Ayrıca, her dil için tipik işlemi kullanarak kendi paketlerinizi yükleyebilirsiniz.

## <a name="pre-configured-jupyter-extensions"></a>Önceden yapılandırılmış Jupyter uzantıları

Azure Dizüstü Bilgisayarlar aşağıdaki Jupyter uzantılarıyla önceden yapılandırılmıştır:

- [RISE](https://github.com/damianavila/RISE): Bir Jupyter Slayt Uzantısı (live_reveal olarak da bilinir). Daha fazla bilgi için [bkz.](present-jupyter-notebooks-slideshow.md)
- [JupyterLab : Jupyter](https://github.com/jupyterlab/jupyterlab)dizüstü bilgisayarlarla çalışmak için tam bir hesaplama ortamı.
- [Altair](https://github.com/ellisonbg/altair): Python için bildirimsel istatistiksel görüntüleme kütüphanesi.
- [BQPlot](https://github.com/bloomberg/bqplot): Jupyter Notebook'lar için etkileşimli bir çizim çerçevesi.
- [IpyWidgets](https://github.com/jupyter-widgets/ipywidgets): Jupyter Notebook'lar için etkileşimli HTML widget'ları.

## <a name="issues-and-getting-help"></a>Sorunlar ve yardım alma

Azure Not Defterleri hala Önizleme'de olduğundan, hizmet diğer Azure hizmetlerine göre daha sık veya daha uzun süreli geçici kesintiler yaşayabilir. Bazı özellikler eksik olabilir veya hata içeriyor olabilir.

Şu anda, iş açısından kritik uygulamalar veya hassas not defterleri ve veriler için Azure Not Defteri Önizlemesi'ni kullanmamayı öneririz.

Azure Dizüstü Bilgisayarlar hakkındaki sorularınızı tartışmak için [GitHub deposunda](https://github.com/Microsoft/AzureNotebooks/issues)bir sorun dosyalayın.

## <a name="next-steps"></a>Sonraki adımlar  

- [Örnek not defterlerini keşfedin](azure-notebooks-samples.md)

- Hızlı Başlangıçlar:

  - [Not defteri oluşturma ve paylaşma](quickstart-create-share-jupyter-notebook.md)
  - [Not defterini kopyalama](quickstart-clone-jupyter-notebook.md)
  - [Yerel Jupyter not defterini geçirme](quickstart-migrate-local-jupyter-notebook.md)
  - [Özel ortam kullanma](quickstart-create-jupyter-notebook-project-environment.md)
  - [Oturum açma ve kullanıcı kimliği ayarlama](quickstart-sign-in-azure-notebooks.md)

- Öğreticiler:

  - [Not defteri oluşturma ve çalıştırma](tutorial-create-run-jupyter-notebook.md  )

- Nasıl-makaleler:
  
  - [Projeleri oluşturma ve kopyalama](create-clone-jupyter-notebooks.md)
  - [Projeleri yapılandırma ve yönetme](configure-manage-azure-notebooks-projects.md)
  - [Paketleri dizüstü bilgisayarın içinden yükleme](install-packages-jupyter-notebook.md)
  - [Slayt gösterisi yapma](present-jupyter-notebooks-slideshow.md)
  - [Veri dosyalarıyla çalışma](work-with-project-data-files.md)
  - [Veri kaynaklarına erişme](access-data-resources-jupyter-notebooks.md)
  - [Azure Machine Learning’i kullanma](use-machine-learning-services-jupyter-notebooks.md)
