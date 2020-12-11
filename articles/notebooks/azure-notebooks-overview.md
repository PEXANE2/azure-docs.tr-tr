---
title: Azure Notebooks önizlemeye genel bakış
description: Kurulum veya yapılandırma gerektirmeyen ücretsiz Azure Notebooks önizleme hizmetini kullanarak bulutta Jupyıter not defterlerini çalıştırın.
ms.topic: overview
ms.date: 04/05/2019
ms.openlocfilehash: ef0a033c5125b3a359d281be30dd0ec2a2aa1747
ms.sourcegitcommit: 6172a6ae13d7062a0a5e00ff411fd363b5c38597
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2020
ms.locfileid: "97109496"
---
# <a name="overview-of-azure-notebooks-preview"></a>Azure Notebooks önizlemeye genel bakış

[!INCLUDE [notebooks-status](../../includes/notebooks-status.md)]

Azure Notebooks, yükleme yapmadan Jupyter not defterlerini geliştirmeye ve çalıştırmaya yönelik, ücretsiz ve barındırılan bir hizmettir. [Jupyter](https://jupyter.org/) (eski adıyla IPython), markı metnini, çalıştırılabilir kodu, kalıcı verileri, grafikleri ve görselleştirmeleri *Not defteri* olarak adlandırılan tek ve paylaşılabilir bir tuval üzerinde kolayca birleştirebilmenizi sağlayan açık kaynaklı bir projem ' dir (Jupyter.org Image hitap):

[![Jupyıter Not defteri örnekleri](https://jupyter.org/assets/jupyterpreview.png)](https://jupyter.org/assets/jupyterpreview.png#lightbox)

Kod, grafik ve açıklayıcı metinlerin bu güçlü birleşimi nedeniyle, Jupyıter, veri bilimi yönergeleri, veri temizleme ve dönüştürme, sayısal benzetim, istatistiksel modelleme ve makine öğrenimi modellerinin geliştirilmesi gibi birçok kullanım için popüler hale geldi.

## <a name="hassle-free-experience"></a>Sorun ücretsiz deneyim

Azure Notebooks prototipsiz, veri bilimi, akademik araştırma veya program Python 'u öğrenmeye yönelik hızlı bir şekilde başlamanıza yardımcı olur:

- Bir veri bilimi, yükleme olmadan tam bir Anaconda ortamına anında erişebilir.
- Öğretmen, öğrenciler için sorunsuz bir Python ortamı sağlayabilir.
- Bir sunucu, katılımcıları 45 dakikalık bir yazılım yüklemesi yapmadan katılanlara veya web seminerine benzer bir şekilde izin verebilir.
- Geliştirici veya Hobbyist, dizüstü bilgisayarları bir hızlı kod karalama paneli olarak kullanabilir.

Kişiler, Azure Notebooks (önizlemede) gibi tarayıcıda erişilebilen bir bulut hizmeti aracılığıyla insanlar üzerinde işbirliği yapabilseler de daha güçlü hale gelir. Bulutta, kullanıcıların Jupyıter 'ı yerel olarak yüklememeleri veya bir ortamı korumasıyla ilgili sorun olması gerekir. Bulut Ayrıca, diğer yetkili kullanıcılarla not defterlerini (ve ilişkili veri dosyalarını) paylaşmayı kolaylaştırır ve kaynak denetimi depoları gibi dış yollarla not defterlerini paylaşma zorluklar önler. Azure Notebooks, kullanıcılar, not defterlerini değişiklik veya deneme için kendi hesaplarına kopyalayabilir (veya "klonlayabilir"), özellikle de yönerge amaçlarıyla yararlı olur.

Azure Notebooks genel bir kod yazma, yürütme ve paylaşma platformu olduğundan, bunu birçok farklı senaryo için kullanabilirsiniz:

- Yeni bir programlama dili öğrenin – [FrontPage öğreticilerden](https://notebooks.azure.com/Microsoft/projects/samples/html/Introduction%20to%20Python.ipynb) birini deneyin
- Veri bilimi öğrenme – TRY [Jake VanderPlas ' Book](https://notebooks.azure.com/jakevdp/projects/PythonDataScienceHandbook)
- Yüzlerce öğrenci için [Kurs öğretin](https://notebooks.azure.com/garth-wells/projects/CUED-IA-Computing-Michaelmas)
- Yükleme sırasında harcamadan çevrimiçi veya bir konferansta web seminerine bir Web semineri sunun 
- GitHub kullanıcılarının [bir GitHub başlatma rozet oluşturarak](https://notebooks.azure.com/help/projects/sharing/create-a-github-badge) not defterlerini doğrudan yüklemesini ve çalıştırmasını sağlama
- Slaytlara, slaytların çalıştırılabilir olduğu [Slayt gösterileri gibi PowerPoint](https://notebooks.azure.com/help/jupyter-notebooks/slides) sunun!

Kısacası, Azure Notebooks çalışmanızı daha verimli bir şekilde gerçekleştirmenize ve bu sayede daha fazla bilgi almanıza yardımcı olur.

> [!Note]
> Jupi 'in kendisi hakkında daha fazla bilgi [Jupyter.org](https://jupyter.org/) ve [jupyıter belgelerinde](https://jupyter-notebook.readthedocs.io/en/latest/)bulunabilir.

## <a name="pricing-and-quotas"></a>Fiyatlandırma ve Kotalar

Azure Notebooks ücretsiz bir hizmettir, ancak kötüye kullanımı engellemek için her proje 4 GB bellek ve 1 GB verilerle sınırlandırılmıştır. Bu sınırları aşan meşru kullanıcılar, not defterlerini çalıştırmaya devam etmek için CAPTCHA Challenge bölümüne bakın.

Tüm limitleri serbest bırakmak için Azure Notebooks Azure Active Directory (örneğin, bir şirket hesabı) kullanarak bir hesapla oturum açın. Bu hesap bir Azure aboneliğiyle ilişkiliyse, bu abonelik içindeki tüm Azure Veri Bilimi Sanal Makinesi örneklerine bağlanabilirsiniz. 

Not defteri sunucularının en fazla 8 saat içinde var olduğu garanti edilir. Çoğu durumda, Kapsayıcınız bu sınıra tabi değildir ve bu sürenin ötesinde çalışmaya devam eder, ancak uzun süreli oturumlar sistem kararlılığını sağlamak için zaman zaman kapatılabilir.

## <a name="available-kernels-and-environments"></a>Kullanılabilir çekirdekler ve ortamlar

Her bir not defteri için, herhangi bir kod hücresini çalıştırmak için kullanılan çekirdeği (çalışma zamanı ortamı) seçersiniz. Azure Notebooks aşağıdaki çekirdekleri destekler:

- Python 2,7 + Anaconda2-5.3.0
- Python 3,6 + Anaconda3-5.3.0
- Python 3,5 + Anaconda3-4.2.0 (kullanım dışı olacaktır)
- R 3.4.1 + Microsoft R Open 3.4.1
- F # 4.1.9

Azure Notebooks, temel dağıtımların ötesinde ek paketler de içerir. Örneğin, Python kerels, örneğin sayısal tuş takımı, Pandas, scikit-öğren, Matplotlib ve bokeh kitaplıklarını içerir.

Ayrıca, projedeki tüm not defterleri için bir ortam oluşturmak üzere bir projeyi özelleştirebilirsiniz.

Temel dağıtımların yanı sıra, Azure Notebooks veri bilimcileri için yararlı olan çok sayıda ek pakete önceden yüklenmiş olarak gelir. Ayrıca, her dil için tipik işlemi kullanarak kendi paketlerinizi yükleyebilirsiniz.

## <a name="pre-configured-jupyter-extensions"></a>Önceden yapılandırılmış Jupyıter uzantıları

Azure Notebooks aşağıdaki Jupyter uzantılarıyla önceden yapılandırılmıştır:

- [RISE](https://github.com/damianavila/RISE): Jupyter slayt gösterisi uzantısı (live_reveal olarak da bilinir).
- [Jupyterlab](https://github.com/jupyterlab/jupyterlab): Jupyter Not defterleri ile çalışmaya yönelik tam bir hesaplama ortamıdır.
- [Altair](https://github.com/ellisonbg/altair): Python için bildirime dayalı istatistiksel görselleştirme kitaplığı.
- [Bqçiz](https://github.com/bloomberg/bqplot): Jupyter Not defterleri için etkileşimli bir çizim çerçevesi.
- [Ipypencere öğeleri](https://github.com/jupyter-widgets/ipywidgets): Jupyter Not defterleri IÇIN etkileşimli HTML pencere öğeleri.

## <a name="issues-and-getting-help"></a>Sorunlar ve yardım alma

Azure Notebooks hala önizlemede olduğundan, hizmet diğer Azure hizmetlerinden daha sık veya daha fazla olabilecek geçici kesintiler yaşayabilir. Bazı özellikler tamamlanmamış veya hata içeriyor olabilir.

Bu noktada, iş açısından kritik uygulamalar veya hassas Not defterleri ve veriler için Azure Notebooks önizlemesi kullanılması önerilir.

Azure Notebooks hakkındaki sorularınızı tartışmak için [GitHub deposunda](https://github.com/Microsoft/AzureNotebooks/issues)bir sorun verin.

## <a name="next-steps"></a>Sonraki adımlar  

- Hızlı Başlangıçlar:

  - [Oturum açma ve kullanıcı kimliği ayarlama](quickstart-sign-in-azure-notebooks.md)
  - [Jupyter Not defterini dışarı aktarma](quickstart-export-jupyter-notebook-project.md)
