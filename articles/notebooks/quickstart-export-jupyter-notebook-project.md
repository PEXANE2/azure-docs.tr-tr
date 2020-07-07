---
title: Azure Notebooks önizlemeden Jupyter Not defteri projesini dışarı aktarma
description: Jupyter Not defteri projesini hızlıca dışarı aktarın.
ms.topic: quickstart
ms.date: 06/29/2020
ms.openlocfilehash: ad37db7e9bdb1251a3e62bd567960979f556b489
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85832041"
---
# <a name="quickstart-export-a-jupyter-notebook-project-in-azure-notebooks-preview"></a>Hızlı başlangıç: Azure Notebooks önizlemede bir Jupyter Not defteri projesini dışarı aktarma

[!INCLUDE [notebooks-status](../../includes/notebooks-status.md)]

Bu hızlı başlangıçta, diğer Jupyter Not defteri çözümlerinde kullanmak üzere bir Azure Notebooks projesi indirirsiniz. 

## <a name="prerequisites"></a>Önkoşullar
Mevcut bir Azure Notebooks projesi.

## <a name="export-an-azure-notebooks-project"></a>Azure Notebooks projesi dışarı aktarma

1. [Azure Notebooks](https://notebooks.azure.com) gidin ve oturum açın. Ayrıntılar için bkz. [hızlı başlangıç-Azure Notebooks Için oturum açma](quickstart-sign-in-azure-notebooks.md).

1. Ortak profil sayfanızda sayfanın en üstündeki **Projelerim** ' nı seçin:

    ![Projelerim tarayıcı penceresinin üst kısmında bağlantı](media/quickstarts/my-projects-link.png)

1. Bir proje seçin.
1. Tüm proje dosyalarınızı içeren bir ZIP dosya indirme tetiklenmesi için "Indir" düğmesine tıklayın.
1. Alternatif olarak, belirli bir proje sayfasından, belirli bir projenin tüm dosyalarını indirmek için "projeyi Indir" düğmesine tıklayın.

Proje dosyalarınızı indirdikten sonra, bunları diğer Jupyter Notebook çözümleriyle birlikte kullanabilirsiniz. Aşağıdaki bölümlerde açıklanan bazı seçenekler şunlardır: 
- [Visual Studio Code](#use-notebooks-in-visual-studio-code)
- [Visual Studio Codespaces](#use-notebooks-in-visual-studio-codespaces)
- [Azure Machine Learning](#use-notebooks-with-azure-machine-learning)
- [Azure Lab Services](#use-azure-lab-services)
- [GitHub](#use-github)

## <a name="create-an-environment-for-notebooks"></a>Not defterleri için ortam oluşturma

Azure Notebooks Önizlemeiyle eşleşen bir ortam oluşturmak istiyorsanız, GitHub 'da belirtilen komut dosyasını kullanabilirsiniz.

1. Azure Notebooks [GitHub deposuna](https://github.com/microsoft/AzureNotebooks) gidin veya [ortam klasörüne doğrudan erişin](https://aka.ms/aznbrequirementstxt).
1. Bir komut isteminden, projeleriniz için kullanmak istediğiniz dizine gidin.
1. Ortam klasörü içeriğini indirin ve BENIOKU yönergelerini izleyerek Azure Notebooks paketi bağımlılıklarını yükleyin.


## <a name="use-notebooks-in-visual-studio-code"></a>Visual Studio Code içindeki not defterlerini kullanma

[Vs Code](https://code.visualstudio.com/) , yerel olarak kullanabileceğiniz veya uzaktan işlem ile bağlantılı kullanabileceğiniz ücretsiz bir kod düzenleyicisidir. Python uzantısıyla birlikte kullanıldığında, jupi Not defterleri ile çalışmaya yönelik zengin bir yerel deneyim dahil olmak üzere Python geliştirmesi için tam bir ortam sunar. 

![VS Code Jupyter Notebook desteği](media/vs-code-jupyter-notebook.png)

Proje dosyalarınızı [indirdikten](#export-an-azure-notebooks-project) sonra, vs Code kullanabilirsiniz. Jupi Not defterleri ile VS Code kullanan rehberlik için, Visual Studio Code öğreticilerde [Visual Studio Code jupi not defterlerine](https://code.visualstudio.com/docs/python/jupyter-support) ve [veri bilimi](https://code.visualstudio.com/docs/python/data-science-tutorial) ile çalışma bölümüne bakın.

Azure Notebooks önizlemesiyle eşleşen bir ortam oluşturmak için Visual Studio Code ile [Azure Notebooks ortam betiğini](#create-an-environment-for-notebooks) de kullanabilirsiniz.

## <a name="use-notebooks-in-visual-studio-codespaces"></a>Visual Studio Codespaces içindeki not defterlerini kullanma

Visual Studio Codespaces, Visual Studio Code veya Web tarayıcınızı kullanarak not defterlerinizi düzenleyebileceğiniz bulutta barındırılan ortamlar sağlar. Aynı harika Jupde deneyimi VS Code, ancak cihazınıza her şeyi yüklemeye gerek kalmadan sunar. Yerel bir ortam ayarlamak ve bulutta desteklenen bir çözümü tercih etmek istemiyorsanız, codespace oluşturmak harika bir seçenektir. Başlamak için:

1. Proje dosyalarınızı [indirin](#export-an-azure-notebooks-project) .
1. Not defterlerinizi depolamak için [bir GitHub deposu oluşturun](https://help.github.com/github/getting-started-with-github/create-a-repo) . 
1. [Dosyalarınızı depoya ekleyin](https://help.github.com/github/managing-files-in-a-repository/adding-a-file-to-a-repository) .
1. Visual Studio Codespaces 'ı [tarayıcıda](https://docs.microsoft.com/visualstudio/online/how-to/browser), [Visual studio](https://docs.microsoft.com/visualstudio/online/how-to/vside)'da veya [Visual Studio Code](https://docs.microsoft.com/visualstudio/online/how-to/vscode)ayarlayın.

## <a name="use-notebooks-with-azure-machine-learning"></a>Azure Machine Learning ile not defterlerini kullanma

Azure Machine Learning, kullanıcıların Azure 'da modelleri daha hızlı oluşturup dağıtmasını sağlayan uçtan uca bir makine öğrenimi platformu sağlar. Azure ML, Jupyıter not defterlerini bir VM 'de veya paylaşılan küme bilgi işlem ortamında çalıştırmanıza olanak sağlar. Deneme izleme, veri kümesi Yönetimi ve daha fazlasını içeren ML iş yükünüz için bulut tabanlı bir çözüme ihtiyacınız varsa Azure Machine Learning önerilir. Azure ML 'yi kullanmaya başlamak için:

1. Proje dosyalarınızı [indirin](#export-an-azure-notebooks-project) .
1. Azure portal [bir çalışma alanı oluşturun](../machine-learning/how-to-manage-workspace.md) .

   ![Çalışma Alanı oluşturma](../machine-learning/media/how-to-manage-workspace/create-workspace.gif)
 
1. [Azure Studio 'yu (Önizleme)](https://ml.azure.com/)açın.
1. Sol taraftaki Gezinti çubuğunu kullanarak **Not defterleri**' ni seçin.
1. **Dosyaları karşıya yükle** düğmesine tıklayın ve Azure Notebooks indirdiğiniz proje dosyalarını karşıya yükleyin.

Azure ML ve Jupimter not defterlerini çalıştırma hakkında ek bilgi için [belgeleri](../machine-learning/how-to-run-jupyter-notebooks.md) gözden geçirebilir veya Microsoft Learn [Machine Learning modüle yönelik girişi](https://docs.microsoft.com/learn/modules/intro-to-azure-machine-learning-service/) deneyebilirsiniz.


## <a name="use-azure-lab-services"></a>Azure Lab Services kullan

[Azure Lab Services](https://azure.microsoft.com/services/lab-services/) , eğitimciler 'in tamamında kolayca kurulum ve önceden yapılandırılmış VM 'lere isteğe bağlı erişim sağlamasına izin verir. Özel bir sınıf ortamında Jupyıter Not defterleri ve bulut işlemi ile çalışmanın bir yolunu arıyorsanız, Laboratuvar Hizmetleri harika bir seçenektir.

![image](../lab-services/media/tutorial-setup-classroom-lab/new-lab-button.png)

 Proje dosyalarınızı [indirdikten](#export-an-azure-notebooks-project) sonra, Azure Lab Services kullanabilirsiniz. Laboratuvar ayarlamayla ilgili rehberlik için bkz. [Python ve jupi Not defterleri ile veri bilimi öğretmek için laboratuvar ayarlama](../lab-services/class-type-jupyter-notebook.md)

## <a name="use-github"></a>GitHub 'ı kullanma

GitHub, not defterlerini (ve diğer dosyaları) depolamak, not defterlerinizi başkalarıyla paylaşmak ve işbirliği yapmak için ücretsiz, kaynak denetimi ile desteklenen bir yol sağlar. Projelerinizi paylaşmanın ve başkalarıyla işbirliği yapmaya yönelik bir yol arıyorsanız, GitHub harika bir seçenektir ve harika bir geliştirme deneyimi için [Visual Studio Codespaces](#use-notebooks-in-visual-studio-codespaces) ile birleştirilebilir. GitHub 'ı kullanmaya başlamak için

1. Proje dosyalarınızı [indirin](#export-an-azure-notebooks-project) .
1. Not defterlerinizi depolamak için [bir GitHub deposu oluşturun](https://help.github.com/github/getting-started-with-github/create-a-repo) . 
1. [Dosyalarınızı depoya ekleyin](https://help.github.com/github/managing-files-in-a-repository/adding-a-file-to-a-repository) .

## <a name="next-steps"></a>Sonraki adımlar

- [Visual Studio Code 'de Python hakkında bilgi edinin](https://code.visualstudio.com/docs/python/python-tutorial)
- [Azure Machine Learning ve Jupyıter not defterlerini öğrenin](../machine-learning/how-to-run-jupyter-notebooks.md)
- [Visual Studio Codespaces hakkında bilgi edinin](https://visualstudio.microsoft.com/services/visual-studio-codespaces/)
- [Azure Lab Services hakkında bilgi edinin](https://azure.microsoft.com/services/lab-services/)
- [GitHub hakkında bilgi edinin](https://help.github.com/github/getting-started-with-github/)