---
title: "Öğretici: Azure DevOps Projelerini kullanarak ASP.NET Core uygulamanızı Azure Hizmet Dokusu'na dağıtın"
description: Azure DevOps Projeleri, Azure'a başlamayı kolaylaştırır. DevOps Projects ile ASP.NET Core uygulamanızı birkaç hızlı adımda Azure Hizmet Kumaşı'na dağıtabilirsiniz.
ms.author: mlearned
ms.manager: gwallace
ms.prod: devops
ms.technology: devops-cicd
ms.topic: tutorial
ms.date: 07/09/2018
author: mlearned
monikerRange: vsts
ms.openlocfilehash: 5f14164da5cd89cc7d0578e6b64c39d227734d75
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "71969469"
---
# <a name="tutorial-deploy-your-aspnet-core-app-to-azure-service-fabric-by-using-azure-devops-projects"></a>Öğretici: Azure DevOps Projelerini kullanarak ASP.NET Core uygulamanızı Azure Hizmet Dokusu'na dağıtın

Azure DevOps Projeleri, mevcut kodunuzu ve Git repo'nuzu getirebileceğiniz veya Azure'a sürekli tümleştirme (CI) ve sürekli teslimat (CD) ardışık bir çözüm hattı oluşturmak için örnek bir uygulama seçebileceğiniz basitleştirilmiş bir deneyim sunar. 

DevOps Projeleri de:
* Azure Hizmet Kumaşı gibi Azure kaynaklarını otomatik olarak oluşturur.
* Azure DevOps'te bir CI/CD ardışık yapı alanı oluşturan bir sürüm ardışık alanı oluşturur ve yapılandırır.
* İzleme için bir Azure Application Insights kaynağı oluşturur.

Bu öğreticide şunları yapacaksınız:

> [!div class="checklist"]
> * ASP.NET Bir Core uygulaması oluşturmak ve Service Fabric'e dağıtmak için DevOps Projelerini kullanın
> * Azure DevOps'leri ve Azure aboneliğini yapılandırma 
> * CI işlem hattını inceleme
> * CD işlem hattını inceleme
> * Git’e değişiklikleri işleme ve Azure’a otomatik olarak dağıtma
> * Kaynakları temizleme

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. [Visual Studio Dev Essentials](https://visualstudio.microsoft.com/dev-essentials/) aracılığıyla ücretsiz bir abonelik alabilirsiniz.

## <a name="use-devops-projects-to-create-an-aspnet-core-app-and-deploy-it-to-service-fabric"></a>ASP.NET Bir Core uygulaması oluşturmak ve Service Fabric'e dağıtmak için DevOps Projelerini kullanın

DevOps Projects, Azure Ardışık Hatları'nda bir CI/CD ardışık alanı oluşturur. Yeni bir Azure DevOps kuruluşu oluşturabilir veya varolan bir kuruluş kullanabilirsiniz. DevOps Projects, seçtiğiniz Azure aboneliğinde Hizmet Kumaşı kümesi gibi Azure kaynakları da oluşturur.

1. [Azure portalında](https://portal.azure.com)oturum açın.

1. Sol bölmede **Kaynak oluştur**'u seçin.

1. Arama **kutusunda, DevOps Projeleri**yazın ve sonra **Oluştur'u**seçin.

    ![DevOps Projeleri panosu](_img/azure-devops-project-github/fullbrowser.png)

1. **.NET'i**seçin ve sonra **İleri'yi**seçin.

1. **Bir uygulama çerçevesi seçin,** ASP.NET **Core'u**seçin ve sonra **İleri'yi**seçin.

1. **Servis Kumaş Kümesi'ni**seçin ve sonra **İleri'yi**seçin. 

## <a name="configure-azure-devops-and-an-azure-subscription"></a>Azure DevOps'leri ve Azure aboneliğini yapılandırma

1. Yeni bir Azure DevOps kuruluşu oluşturun veya varolan bir kuruluş seçin. 

1. Azure DevOps projeniz için bir ad girin. 

1. Azure aboneliğinizi seçin.

1. Ek Azure yapılandırma ayarlarını görüntülemek ve Hizmet Kumaşı kümesi için düğüm sanal makine boyutunu ve işletim sistemini tanımlamak için **Değiştir'i**seçin.  
    Bu bölme, Azure hizmetlerinin türünü ve konumunu yapılandırmak için çeşitli seçenekler görüntüler.
 
1. Azure yapılandırma alanından çıkın ve **ardından Bitti'yi**seçin.  
    Birkaç dakika sonra işlem tamamlanır. Azure DevOps kuruluşunuzdaki Bir Git repo'sunda core ASP.NET örnek bir uygulama ayarlanır, Bir Hizmet Kumaşı kümesi oluşturulur, BIR CI/CD ardışık adı yürütülür ve uygulamanız Azure'a dağıtılır. 

    Tüm bunlar tamamlandıktan sonra DevOps Projects panosu Azure portalında görüntülenir. DevOps Projects panosuna doğrudan Azure portalındaki **tüm kaynaklardan** gidebilirsiniz. 

    Bu pano, Azure DevOps kod repo'nuza, CI/CD ardışık sisteminizde ve Service Fabric kümenizde görünürlük sağlar. Azure Repos'ta CI/CD ardışık hattınız için ek seçenekleri yapılandırabilirsiniz. Sağdan, çalışan uygulamanızı görüntülemek için **Gözat'ı** seçin.

## <a name="examine-the-ci-pipeline"></a>CI işlem hattını inceleme

DevOps Projects, Azure Ardışık Hatları'nda bir CI/CD ardışık hattını otomatik olarak yapılandırır. İşlem hattını inceleyebilir ve özelleştirebilirsiniz. Kendinizi tanımak için aşağıdakileri yapın:

1. DevOps Project panosuna gidin.

1. DevOps Projeleri panosunun üst kısmında, **yapı dizilerini**seçin.  
    Tarayıcı sekmesi, yeni projeniz için yapı ardışık hattını görüntüler.

1. **Durum** alanını işaret edin ve sonra elipsleri seçin (...).  
    Menü, yeni bir yapıyı sıraya alma, yapıyı duraklatma ve yapı ardışık hattını düzenleme gibi çeşitli seçenekler görüntüler.

1. **Edit'i**seçin.

1. Bu bölmede, yapı ardışık hattınız için çeşitli görevleri inceleyebilirsiniz.  
    Yapı, Git repo'sundan kaynak alma, bağımlılıkları geri getirme ve dağıtımlar için kullanılan çıktıları yayımlama gibi çeşitli görevleri gerçekleştirir.

1. Derleme işlem hattının üst kısmında derleme işlem hattı adı’nı seçin. 

1. Derleme işlem hattı adınızın altında **Geçmiş**’i seçin.  
    Bu bölme, yapı için son değişikliklerinizin denetim izini görüntüler. Azure DevOps, yapı ardışık yapısında yapılan değişiklikleri izler ve sürümleri karşılaştırmanızı sağlar.

1. **Tetikleyiciler**’i seçin.  
    DevOps Projeleri otomatik olarak bir CI tetikleyicisi oluşturur ve repo'ya her taahhüt yeni bir yapı başlatır. İsteğe bağlı olarak, ci işleminden dalları eklemeyi veya hariç tutmayı seçebilirsiniz.

1. **Saklama**’yı seçin.  
    Senaryonuza bağlı olarak, belirli sayıda yapıyı tutmak veya kaldırmak için ilkeler belirtebilirsiniz.

## <a name="examine-the-cd-pipeline"></a>CD işlem hattını inceleme

DevOps Projects, Azure DevOps kuruluşunuzdan Azure aboneliğinize dağıtmak için gerekli adımları otomatik olarak oluşturur ve yapılandırır. Bu adımlar, Azure DevOps'lerin Azure aboneliğinize kimlik doğrulaması yapmak için bir Azure hizmet bağlantısını yapılandırmayı içerir. Otomasyon, CD'yi Azure'a sağlayan bir sürüm ardışık hattı da oluşturur. Sürüm ardışık hattı hakkında daha fazla bilgi edinmek için aşağıdakileri yapın:

1. **Oluştur ve Yayınla'yı**seçin ve ardından **Sürümler'i**seçin.  
    DevOps Projects, Azure'daki dağıtımları yönetmek için bir sürüm ardışık hattı oluşturur.

1. Sürüm ardışık ardışık alanınızın yanındaki elipsleri (...) seçin ve ardından **Edit'i**seçin.  
    Yayın işlem hattı, yayın işlemini tanımlayan bir *işlem hattı* içerir.

1. **Yapıtlar**’ın altında **Bırak**’ı seçin.  
    Daha önce incelediğiniz yapı ardışık hattı, yapı için kullanılan çıktıyı üretir. 

1. **Bırak** simgesinin sağında, **Sürekli dağıtım tetikleyicisini**seçin.  
    Bu sürüm ardışık yapısı, yeni bir yapı artifakı kullanılabilir olduğunda dağıtımı yürüten etkin bir CD tetikleyicisi vardır. İsteğe bağlı olarak, dağıtımlarınızın el ile yürütülmesini gerektirecek şekilde tetikleyiciyi devre dışı kullanabilirsiniz. 

1. Sağda, sürümgeçmişi görüntülemek için **görünüm sürümlerini** seçin.

1. Bir sürümün yanındaki elipsleri (...) seçin ve ardından **Aç'ı**seçin.  
    Sürüm özeti, ilişkili iş öğeleri ve testler gibi çeşitli menüleri keşfedebilirsiniz.

1. **İşlemeler**'i seçin.  
    Bu görünüm, bu dağıtımla ilişkili kod işlemelerini gösterir. Dağıtımlar arasındaki işleme farklılıklarını görmek için yayınları karşılaştırın.

1. **Günlükleri**seçin.  
    Günlüklerde, dağıtım işlemiyle ilgili yararlı bilgiler bulunur. Bunları dağıtımlar sırasında ve sonrasında görüntüleyebilirsiniz.

## <a name="commit-changes-to-git-and-automatically-deploy-them-to-azure"></a>Değişiklikleri Git'e işleme ve bunları otomatik olarak Azure'a dağıtma 

 > [!NOTE]
 > Aşağıdaki yordam, basit bir metin değişikliği yaparak CI/CD ardışık hattını sınar.

Artık en son çalışmanızı web sitenize otomatik olarak dağıtan bir CI/CD işlemi kullanarak uygulamanızdaki bir ekiple işbirliği yapmaya hazırsınız. Git repo'sundaki her değişiklik bir yapıyı başlatır ve bir sürüm değişikliklerinizi Azure'a dağıtır. Bu bölümdeki yordamı izleyin veya repo'nuzda değişiklik yapmak için başka bir teknik kullanın. Örneğin, en sevdiğiniz araçta veya IDE'de Git repo'yu klonlayabilir ve değişiklikleri bu repo'ya itebilirsiniz.

1. Azure DevOps menüsünde **Kod** > **Dosyaları'nı**seçin ve ardından repo'nuza gidin.

1. *Görünümler\Ev* dizinine gidin, *Index.cshtml* dosyasının yanındaki elipsleri (...) seçin ve ardından **Edit'i**seçin.

1. Div etiketlerinden birine metin eklemek gibi dosyada değişiklik yapın. 

1. Sağ üstte **Commit'i**seçin ve sonra değişikliğinizi zorlamak için yeniden **Commit'i** seçin.  
    Birkaç dakika sonra, bir yapı başlar ve sonra değişiklikleri dağıtmak için bir sürüm yürütülür. DevOps Projects panosunda veya Azure DevOps gerçek zamanlı günlüğe kaydetme ile tarayıcıda yapı durumunu izleyebilirsiniz.

1. Sürüm tamamlandıktan sonra, değişikliklerinizi doğrulamak için uygulamanızı yenileyin.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Sınıyorsanız, kaynaklarınızı temizleyerek faturalandırma ücretleri tahakkuk etmekten kaçınabilirsiniz. Artık ihtiyaç duyulmadığında, bu öğreticide oluşturduğunuz Azure Hizmet Dokusu kümesini ve ilgili kaynakları silebilirsiniz. Bunu yapmak için DevOps Projects panosundaki **Sil** işlevini kullanın.

> [!IMPORTANT]
> Aşağıdaki yordam kaynakları kalıcı olarak siler. *Silme* işlevi, DevOps Projects'te proje tarafından oluşturulan verileri hem Azure hem de Azure DevOps'lerde yok eder ve bu verileri geri alamazsınız. Bu yordamı yalnızca istemleri dikkatle okuduktan sonra kullanın.

1. Azure portalında DevOps Projects panosuna gidin.
1. Sağ üstte **Sil'i**seçin. 
1. İstemde, kaynakları *kalıcı olarak silmek* için **Evet'i** seçin.

## <a name="next-steps"></a>Sonraki adımlar

Ekibinizin gereksinimlerine uygun olarak bu Azure CI/CD işlem hatlarını istediğiniz gibi değiştirebilirsiniz. Ayrıca bu CI/CD desenini diğer işlem hatlarınızda şablon olarak kullanabilirsiniz. Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * ASP.NET Bir Core uygulaması oluşturmak ve Service Fabric'e dağıtmak için DevOps Projelerini kullanın
> * Azure DevOps'leri ve Azure aboneliğini yapılandırma 
> * CI işlem hattını inceleme
> * CD işlem hattını inceleme
> * Değişiklikleri Git'e işleme ve bunları otomatik olarak Azure'a dağıtma
> * Kaynakları temizleme

Service Fabric ve microservices hakkında daha fazla bilgi edinmek için bkz:

> [!div class="nextstepaction"]
> [Uygulamaları oluşturmak için bir mikro hizmetler yaklaşımı kullanma](https://docs.microsoft.com/azure/devops/pipelines/release/define-multistage-release-process?view=vsts)
