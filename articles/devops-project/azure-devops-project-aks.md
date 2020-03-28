---
title: 'Öğretici: Azure DevOps Projeleri ile Azure Kubernetes Hizmetine ASP.NET Temel uygulamaları dağıtın'
description: Azure DevOps Projeleri, Azure'a başlamayı kolaylaştırır. DevOps Projects ile ASP.NET Core uygulamanızı Azure Kubernetes Hizmeti (AKS) ile birkaç hızlı adımda dağıtabilirsiniz.
ms.author: mlearned
ms.manager: gwallace
ms.prod: devops
ms.technology: devops-cicd
ms.topic: tutorial
ms.date: 07/09/2018
author: mlearned
monikerRange: vsts
ms.openlocfilehash: b27d56d78296dc5500f97802f811a8923c4e87a8
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "71969681"
---
# <a name="tutorial-deploy-aspnet-core-apps-to-azure-kubernetes-service-with-azure-devops-projects"></a>Öğretici: Azure DevOps Projeleri ile Azure Kubernetes Hizmetine ASP.NET Temel uygulamaları dağıtın

Azure DevOps Projeleri, mevcut kodunuzu ve Git repo'nuzu getirebileceğiniz veya Azure'a sürekli tümleştirme (CI) ve sürekli teslimat (CD) ardışık bir çözüm hattı oluşturmak için örnek bir uygulama seçebileceğiniz basitleştirilmiş bir deneyim sunar. 

DevOps Projeleri de:
* Azure Kubernetes Hizmeti (AKS) gibi Azure kaynaklarını otomatik olarak oluşturur.
* Azure DevOps'te CI/CD için bir yapı ve sürüm ardışık hattı oluşturan bir sürüm ardışık alanı oluşturur ve yapılandırır.
* İzleme için bir Azure Application Insights kaynağı oluşturur.
* AKS kümesindeki kapsayıcı iş yüklerinin performansını izlemek [için kapsayıcılar için Azure Monitörünü](https://docs.microsoft.com/azure/azure-monitor/insights/container-insights-overview) etkinleştirin

Bu öğreticide şunları yapacaksınız:

> [!div class="checklist"]
> * AKS'ye ASP.NET Core uygulamasını dağıtmak için DevOps Projelerini kullanın
> * Azure DevOps'leri ve Azure aboneliğini yapılandırma 
> * AKS kümesini inceleme
> * CI işlem hattını inceleme
> * CD işlem hattını inceleme
> * Değişiklikleri Git'e işleme ve bunları otomatik olarak Azure'a dağıtma
> * Kaynakları temizleme

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. [Visual Studio Dev Essentials](https://visualstudio.microsoft.com/dev-essentials/) aracılığıyla ücretsiz bir abonelik alabilirsiniz.

## <a name="use-devops-projects-to-deploy-an-aspnet-core-app-to-aks"></a>AKS'ye ASP.NET Core uygulamasını dağıtmak için DevOps Projelerini kullanın

DevOps Projects, Azure Ardışık Hatları'nda bir CI/CD ardışık alanı oluşturur. Yeni bir Azure DevOps kuruluşu oluşturabilir veya varolan bir kuruluş kullanabilirsiniz. DevOps Projects, seçtiğiniz Azure aboneliğinde AKS kümesi gibi Azure kaynakları da oluşturur.

1. [Azure portalında](https://portal.azure.com)oturum açın.

1. Sol bölmede **Kaynak oluştur**'u seçin.

1. Arama **kutusunda, DevOps Projeleri**yazın ve sonra **Oluştur'u**seçin.

    ![DevOps Projeleri panosu](_img/azure-devops-project-github/fullbrowser.png)

1. **.NET'i**seçin ve sonra **İleri'yi**seçin.

1. **Bir uygulama çerçevesi seçin,** ASP.NET **Core'u**seçin.

1. **Kubernetes Servisi'ni**seçin ve sonra **İleri'yi**seçin. 

## <a name="configure-azure-devops-and-an-azure-subscription"></a>Azure DevOps'leri ve Azure aboneliğini yapılandırma

1. Yeni bir Azure DevOps kuruluşu oluşturun veya varolan bir kuruluş seçin. 

1. Azure DevOps projeniz için bir ad girin. 

1. Azure aboneliğinizi seçin.

1. Ek Azure yapılandırma ayarlarını görüntülemek ve AKS kümesi için düğüm sayısını belirlemek için **Değiştir'i**seçin.  
    Bu bölme, Azure hizmetlerinin türünü ve konumunu yapılandırmak için çeşitli seçenekler görüntüler.
 
1. Azure yapılandırma alanından çıkın ve **ardından Bitti'yi**seçin.  
    Birkaç dakika sonra işlem tamamlanır. Azure DevOps kuruluşunuzdaki Bir Git reposunda örnek ASP.NET Core uygulaması ayarlanır, bir AKS kümesi oluşturulur, BIR CI/CD ardışık adı yürütülür ve uygulamanız Azure'a dağıtılır. 

    Tüm bunlar tamamlandıktan sonra, Azure DevOps Project panosu Azure portalında görüntülenir. DevOps Projects panosuna doğrudan Azure portalındaki **tüm kaynaklardan** gidebilirsiniz. 

    Bu pano, Azure DevOps kod deponuzda, CI/CD ardışık sisteminizde ve AKS kümenizde görünürlük sağlar. Azure DevOps ardışık sisteminizde ek CI/CD seçenekleri yapılandırabilirsiniz. Sağdan, çalışan uygulamanızı görüntülemek için **Gözat'ı** seçin.

## <a name="examine-the-aks-cluster"></a>AKS kümesini inceleme

DevOps Projects, keşfedebileceğiniz ve özelleştirebileceğiniz bir AKS kümesini otomatik olarak yapılandırır. AKS kümesini tanımak için aşağıdakileri yapın:

1. DevOps Projeleri panosuna gidin.

1. Sağdaki AKS hizmetini seçin.  
    AKS kümesi için bir bölme açılır. Bu görünümden, kapsayıcı durumunu izleme, günlükleri arama ve Kubernetes panosunu açma gibi çeşitli eylemler gerçekleştirebilirsiniz.

1. Sağdaki **Kubernetes panosunu Görüntüle'yi**seçin.  
    İsteğe bağlı olarak, Kubernetes panosunu açmak için adımları izleyin.

## <a name="examine-the-ci-pipeline"></a>CI işlem hattını inceleme

DevOps Projects, Azure DevOps kuruluşunuzdaki BIR CI/CD ardışık hattını otomatik olarak yapılandırır. İşlem hattını inceleyebilir ve özelleştirebilirsiniz. Kendinizi tanımak için aşağıdakileri yapın:

1. DevOps Projeleri panosuna gidin.

1. DevOps Projeleri panosunun üst **kısmında, Yapı Boru Hatları'nı**seçin.  
    Tarayıcı sekmesi, yeni projeniz için yapı ardışık hattını görüntüler.

1. **Durum** alanını işaret edin ve sonra elipsleri seçin (...).  
    Menü, yeni bir yapıyı sıraya alma, yapıyı duraklatma ve yapı ardışık hattını düzenleme gibi çeşitli seçenekler görüntüler.

1. **Edit'i**seçin.

1. Bu bölmede, yapı ardışık hattınız için çeşitli görevleri inceleyebilirsiniz.  
    Yapı, Git repo'sundan kaynak alma, bağımlılıkları geri getirme ve dağıtımlar için kullanılan çıktıları yayımlama gibi çeşitli görevleri gerçekleştirir.

1. Derleme işlem hattının üst kısmında derleme işlem hattı adı’nı seçin.

1. Yapı ardışık noktanızın adını daha açıklayıcı bir şeyle değiştirin, **& sırasını kaydet'i**seçin ve sonra **Kaydet'i**seçin.

1. Derleme işlem hattı adınızın altında **Geçmiş**’i seçin.  
    Bu bölme, yapı için son değişikliklerinizin denetim izini görüntüler. Azure DevOps, yapı ardışık yapısında yapılan değişiklikleri izler ve sürümleri karşılaştırmanızı sağlar.

1. **Tetikleyiciler**’i seçin.  
    DevOps Projeleri otomatik olarak bir CI tetikleyicisi oluşturur ve repo'ya her taahhüt yeni bir yapı başlatır. İsteğe bağlı olarak, ci işleminden dalları eklemeyi veya hariç tutmayı seçebilirsiniz.

1. **Saklama**’yı seçin.  
    Senaryonuza bağlı olarak, belirli sayıda yapıyı tutmak veya kaldırmak için ilkeler belirtebilirsiniz.

## <a name="examine-the-cd-release-pipeline"></a>CD sürüm ardışık hattını inceleyin

DevOps Projects, Azure DevOps kuruluşunuzdan Azure aboneliğinize dağıtmak için gerekli adımları otomatik olarak oluşturur ve yapılandırır. Bu adımlar, Azure DevOps'lerin Azure aboneliğinize kimlik doğrulaması yapmak için bir Azure hizmet bağlantısını yapılandırmayı içerir. Otomasyon, CD'yi Azure'a sağlayan bir sürüm ardışık hattı da oluşturur. Sürüm ardışık hattı hakkında daha fazla bilgi edinmek için aşağıdakileri yapın:

1. **Oluştur ve Yayınla'yı**seçin ve ardından **Sürümler'i**seçin.  
    DevOps Projects, Azure'daki dağıtımları yönetmek için bir sürüm ardışık hattı oluşturur.

1. Sürüm ardışık ardışık alanınızın yanındaki elipsleri (...) seçin ve ardından **Edit'i**seçin.  
    Yayın işlem hattı, yayın işlemini tanımlayan bir *işlem hattı* içerir.

1. **Yapıtlar**’ın altında **Bırak**’ı seçin.  
    Önceki adımlarda incelediğiniz yapı ardışık hattı, yapı için kullanılan çıktıyı üretir. 

1. **Bırak** simgesinin sağında, **Sürekli dağıtım tetikleyicisini**seçin.  
    Bu sürüm ardışık yapısı, yeni bir yapı artifakı kullanılabilir olduğunda dağıtımı yürüten etkin bir CD tetikleyicisi vardır. İsteğe bağlı olarak, dağıtımlarınızın el ile yürütülmesini gerektirecek şekilde tetikleyiciyi devre dışı kullanabilirsiniz. 

1. Sağda, sürümgeçmişi görüntülemek için **görünüm sürümlerini** seçin.

1. Bir sürümün yanındaki elipsleri (...) seçin ve ardından **Aç'ı**seçin.  
    Sürüm özeti, ilişkili iş öğeleri ve testler gibi çeşitli menüleri keşfedebilirsiniz.

1. **İşlemeler**'i seçin.  
    Bu görünüm, bu dağıtımla ilişkili kod işlemelerini gösterir. Dağıtımlar arasındaki işleme farklılıklarını görmek için yayınları karşılaştırın.

1. **Günlükleri**seçin.  
    Günlüklerde, dağıtım işlemiyle ilgili yararlı bilgiler bulunur. Bunları dağıtımlar sırasında ve sonrasında görüntüleyebilirsiniz.

## <a name="commit-changes-to-azure-repos-and-automatically-deploy-them-to-azure"></a>Azure Deposu'nda değişiklik yapma ve bunları otomatik olarak Azure'a dağıtma 

 > [!NOTE]
 > Aşağıdaki yordam, basit bir metin değişikliği yaparak CI/CD ardışık hattını sınar.

Artık en son çalışmanızı web sitenize otomatik olarak dağıtan bir CI/CD işlemi kullanarak uygulamanızdaki bir ekiple işbirliği yapmaya hazırsınız. Git repo'sundaki her değişiklik Azure DevOps'te bir yapı yı başlatır ve CD ardışık bir yapı Azure'a dağıtım yürütür. Bu bölümdeki yordamı izleyin veya repo'nuzda değişiklik yapmak için başka bir teknik kullanın. Örneğin, en sevdiğiniz araçta veya IDE'de Git repo'yu klonlayabilir ve değişiklikleri bu repo'ya itebilirsiniz.

1. Azure DevOps menüsünde **Kod** > **Dosyaları'nı**seçin ve ardından repo'nuza gidin.

1. *Görünümler\Ev* dizinine gidin, *Index.cshtml* dosyasının yanındaki elipsleri (...) seçin ve ardından **Edit'i**seçin.

1. Div etiketlerinden birine metin eklemek gibi dosyada değişiklik yapın. 

1. Sağ üstte **Commit'i**seçin ve sonra değişikliğinizi zorlamak için yeniden **Commit'i** seçin.  
    Birkaç dakika sonra, Azure DevOps'te bir yapı başlar ve değişiklikleri dağıtmak için bir sürüm yürütülür. DevOps Projects panosunda veya Azure DevOps kuruluşunuzla tarayıcıda yapı durumunu izleyin.

1. Sürüm tamamlandıktan sonra, değişikliklerinizi doğrulamak için uygulamanızı yenileyin.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Sınıyorsanız, kaynaklarınızı temizleyerek faturalandırma ücretleri tahakkuk etmekten kaçınabilirsiniz. Artık ihtiyaç duyulmadığında, bu öğreticide oluşturduğunuz AKS kümesini ve ilgili kaynakları silebilirsiniz. Bunu yapmak için DevOps Projects panosundaki **Sil** işlevini kullanın.

> [!IMPORTANT]
> Aşağıdaki yordam kaynakları kalıcı olarak siler. *Silme* işlevi, DevOps Projects'te proje tarafından oluşturulan verileri hem Azure hem de Azure DevOps'lerde yok eder ve bu verileri geri alamazsınız. Bu yordamı yalnızca istemleri dikkatle okuduktan sonra kullanın.

1. Azure portalında DevOps Projects panosuna gidin.
2. Sağ üstte **Sil'i**seçin. 
3. İstemde, kaynakları *kalıcı olarak silmek* için **Evet'i** seçin.

## <a name="next-steps"></a>Sonraki adımlar

Ekibinizin gereksinimlerine uygun olarak bu derleme ve yayın işlem hatlarını istediğiniz gibi değiştirebilirsiniz. Ayrıca bu CI/CD desenini diğer işlem hatlarınızda şablon olarak kullanabilirsiniz. Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * AKS'ye ASP.NET Core uygulamasını dağıtmak için DevOps Projelerini kullanın
> * Azure DevOps'leri ve Azure aboneliğini yapılandırma 
> * AKS kümesini inceleme
> * CI işlem hattını inceleme
> * CD işlem hattını inceleme
> * Değişiklikleri Git'e işleme ve bunları otomatik olarak Azure'a dağıtma
> * Kaynakları temizleme

Kubernetes panosunu kullanma hakkında daha fazla bilgi edinmek için bkz:

> [!div class="nextstepaction"]
> [Kubernetes panosunu kullanma](https://docs.microsoft.com/azure/aks/kubernetes-dashboard)
