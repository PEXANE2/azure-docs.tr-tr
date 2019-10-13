---
title: 'Öğretici: Azure DevOps Projeleri kullanarak mevcut kodunuz için bir CI/CD işlem hattı oluşturun'
description: Azure DevOps Projeleri Azure 'u kullanmaya başlamanızı kolaylaştırır. DevOps Projeleri, birkaç hızlı adımda seçtiğiniz bir Azure hizmetinde bir uygulamayı başlatmak için kendi kodunuzu ve GitHub deponuzu kullanmanıza yardımcı olur.
services: vsts
documentationcenter: vs-devops-build
ms.author: mlearned
ms.manager: gwallace
editor: ''
ms.assetid: ''
ms.workload: web
ms.prod: devops
ms.technology: devops-cicd
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 06/27/2019
author: mlearned
ms.custom: mvc
monikerRange: vsts
ms.openlocfilehash: 4a48b5c413f6dfb58a1ba3f896ded49250cde306
ms.sourcegitcommit: e0a1a9e4a5c92d57deb168580e8aa1306bd94723
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2019
ms.locfileid: "72286253"
---
# <a name="tutorial-create-a-cicd-pipeline-for-your-existing-code-by-using-azure-devops-projects"></a>Öğretici: Azure DevOps Projeleri kullanarak mevcut kodunuz için bir CI/CD işlem hattı oluşturun

Azure DevOps Projeleri, mevcut kodunuzu ve git deponuzu getirebileceğiniz veya Azure 'a sürekli tümleştirme (CI) ve sürekli teslim (CD) işlem hattı oluşturmak için örnek bir uygulama seçebileceğiniz basitleştirilmiş bir deneyim sunar.

Yapacaklarınız:

> [!div class="checklist"]
> * CI/CD işlem hattı oluşturmak için DevOps Projeleri kullanma
> * GitHub deponuza erişimi yapılandırma ve bir çerçeve seçme
> * Azure DevOps ve bir Azure aboneliği yapılandırma 
> * Değişiklikleri GitHub 'a işleyin ve Azure 'a otomatik olarak dağıtın
> * Azure Pipelines CI/CD işlem hattını inceleyin
> * Kaynakları temizleme

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. [Visual Studio Dev Essentials](https://visualstudio.microsoft.com/dev-essentials/) aracılığıyla ücretsiz bir abonelik alabilirsiniz.
* .NET, Java, PHP, Node, Python veya static Web kodu içeren bir GitHub veya dış git deposuna erişim.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

Azure DevOps Projeleri, Azure Pipelines bir CI/CD işlem hattı oluşturur. Yeni bir Azure DevOps kuruluşu oluşturabilir veya var olan bir kuruluşu kullanabilirsiniz. Azure DevOps Projeleri ayrıca Azure kaynaklarını tercih ettiğiniz Azure aboneliğinde de oluşturur.

1. [Azure Portal](https://portal.azure.com)’ında oturum açın.

2. Sol bölmede + sol gezinti çubuğunda **+ kaynak oluştur** ' u seçin ve ardından **DevOps Projeleri**arayın, **Oluştur**' u seçin.

   ![DevOps Projeleri panosu](_img/azure-devops-project-github/azuredashboard.png)

3. **Kendi kodunuzu getir**' i seçin ve ardından **İleri**' yi seçin.

## <a name="configure-access-to-your-github-repo-and-choose-a-framework"></a>GitHub deponuza erişimi yapılandırma ve bir çerçeve seçme

1. **GitHub** veya harici bir **Git** kod deposu seçin. Bu öğretici için **GitHub**' ı seçin. GitHub deponuza Azure erişimi sağlamak için GitHub ile ilk kez kimlik doğrulaması yapmanız gerekebilir.

2. **Depoyu** ve **dalı**seçip **İleri**' yi seçin.

3. Docker Kapsayıcıları değişikliği kullanıyorsanız, **uygulama Dockeru** **Evet**olarak ayarlanırsa, bu **öğretici seçili** değilse, **İleri**' yi seçin. Docker kapsayıcılarını kullanma hakkında daha fazla bilgi için **ı** simgesinin üzerine gelin.

   ![.NET Framework](_img/azure-devops-project-github/appframework.png)

4. Açılan kutudan **uygulama çalışma zamanını** ve **çerçevesini**seçip **İleri**' yi seçin. Seçtiğiniz uygulama çerçevesi, kullanılabilir olan Azure hizmet dağıtım hedefinin türünü belirler.

5. Uygulamayı dağıtmak için **Azure hizmetini** seçin ve ardından **İleri**' yi seçin.

## <a name="configure-azure-devops-and-an-azure-subscription"></a>Azure DevOps ve bir Azure aboneliği yapılandırma

1. Bir **Proje adı**girin.

2. Yeni bir ücretsiz **Azure DevOps organizasyonu** oluşturun veya açılan listeden mevcut bir kuruluştan birini seçin.

3. **Azure aboneliğinizi**seçin, **Web uygulamanız** için bir ad girin veya varsayılan yapın. Bir **konum**seçin ve **bitti**' yi seçin. Birkaç dakika sonra DevOps proje dağıtımına genel bakış Azure portal görüntülenir.

4. DevOps Proje panosunu görüntülemek için **Kaynağa Git** ' i seçin. Hızlı erişim için, sağ üst köşede **projeyi** panonuza sabitleyin. Azure DevOps Projeleri otomatik olarak bir CI derleme ve yayın tetikleyicisi yapılandırır. Kodunuz, GitHub deponuzda veya başka bir dış depoda kalır. Örnek bir uygulama, **Azure DevOps kuruluşunuzda**bir depoda ayarlanır. Bir yapı yürütülür ve uygulamanız Azure 'a dağıtılır.

   ![DevOps Projeleri pano görünümü](_img/azure-devops-project-github/projectsdashboard.png)

5. Pano, kod deponuzda, CI/CD işlem hattınızla ve Azure 'daki uygulamanız için görünürlük sağlar. Çalışan uygulamanızı görüntülemek için Azure kaynakları ' nın sağ tarafında bulunan **Araştır** ' ı seçin.

## <a name="commit-changes-to-github-and-automatically-deploy-them-to-azure"></a>Değişiklikleri GitHub 'a işleyin ve Azure 'a otomatik olarak dağıtın

Şimdi, en son çalışmalarınızı otomatik olarak Web sitenize dağıtan bir CI/CD işlemi kullanarak uygulamanızdaki bir takımla işbirliği yapmaya hazırsınız. GitHub deposunda yapılan her değişiklik Azure DevOps 'da bir derleme başlatır ve bir CD işlem hattı Azure 'a bir dağıtım yürütür.

1. DevOps proje panonuzdan **depolar**' ı seçin. GitHub deponuz yeni bir tarayıcı sekmesinde açılır. uygulamanızda bir değişiklik yapın ve ardından **Değişiklikleri Kaydet**' e tıklayın.

2. Birkaç dakika sonra, bir derleme Azure Pipelines başlar. Yapı durumunu DevOps Projeleri panosunda izleyebilir veya proje panosundan **derleme işlem hatları** sekmesini seçerek Azure DevOps kuruluşunuzda izleyebilirsiniz.

## <a name="examine-the-azure-pipelines-cicd-pipeline"></a>Azure Pipelines CI/CD işlem hattını inceleyin

Azure DevOps Projeleri, Azure Pipelines bir CI/CD işlem hattını otomatik olarak yapılandırır. İşlem hattını gerektiği şekilde keşfedin ve özelleştirin. Derleme ve yayın işlem hatları hakkında bilgi edinmek için aşağıdakileri yapın:

1. DevOps Projeleri panosunda **derleme işlem hatları**' nı seçin.

2. **Azure Pipelines** sayfanız açıldıktan sonra, en son derlemelerin ve her bir yapının durumunun geçmişini görürsünüz.

   ![Azure DevOps işlem hattı derlemeleri](_img/azure-devops-project-github/pipelinesbuildpage.png)

3. **Yapılar** sayfasının sağ üst köşesinde, geçerli yapıyı **düzenleme** , yeni bir derlemeyi sıraya alma **kuyruğu** ve üç nokta ( **&#8942;** ) ile daha fazla seçenek içeren bir menü açmak için, **Düzenle**' yi seçin.

4. Derleme, depodan kaynak getirme, bağımlılıkları geri yükleme ve dağıtımlar için yayımlama çıkışları gibi çeşitli görevleri gerçekleştirir. **Ad**' ın altında, derleme işlem hattı adını daha açıklayıcı bir şekilde değiştirin. **& kuyruğu kaydet**' i seçin, sonra **kaydedin**, bir açıklama bırakın ve sonra yeniden **Kaydet** ' i seçin.

   ![Azure DevOps derlemeleri sayfası](_img/azure-devops-project-github/buildpage.png)

5. Derleme için son değişikliklerinizin denetim izini görmek için **Geçmiş** sekmesini seçin. Azure DevOps, derleme ardışık düzeninde yapılan tüm değişiklikleri izler ve sürümleri karşılaştırmanızı sağlar.

6. **Tetikleyiciler** sekmesini seçin. Azure DevOps projesi, bazı varsayılan ayarlarla otomatik olarak bir CI tetikleyicisi oluşturur. **Sürekli tümleştirmeyi etkinleştir** gibi Tetikleyiciler, bir kod değişikliğinin her işlendiği her seferinde bir derlemeyi yürütmek üzere ayarlanabilir veya derlemeleri belirli zamanlarda çalışacak şekilde planlıyor olabilir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olmadığında Azure App Service 'i ve bu öğreticide oluşturduğunuz ilgili kaynakları silebilirsiniz. Bunu yapmak için DevOps Projeleri panosundaki **silme** işlevini kullanın.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide CI/CD işleminizi yapılandırdığınızda, Azure DevOps Projeleri içinde otomatik olarak bir derleme ve yayın işlem hattı oluşturuldu. Ekibinizin ihtiyaçlarını karşılamak için bu derleme ve yayın işlem hatlarını değiştirebilirsiniz. Şunları öğrendiniz:

> [!div class="checklist"]
>  * CI/CD işlem hattı oluşturmak için DevOps Projeleri kullanma
> * GitHub deponuza erişimi yapılandırma ve bir çerçeve seçme
> * Azure DevOps ve bir Azure aboneliği yapılandırma
> * Değişiklikleri GitHub 'a işleyin ve Azure 'a otomatik olarak dağıtın
> * Azure Pipelines CI/CD işlem hattını inceleyin
> * Kaynakları temizleme

CI/CD işlem hattı hakkında daha fazla bilgi edinmek için bkz.:

> [!div class="nextstepaction"]
> [Çoklu aşamalı sürekli dağıtım (CD) işlem hattınızı tanımlama](https://docs.microsoft.com/azure/devops/pipelines/release/define-multistage-release-process?view=vsts)

Uygulama izleme hakkında daha fazla bilgi için bkz.
  
 > [!div class="nextstepaction"]
 > [Azure izleyici nedir?](https://docs.microsoft.com/azure/azure-monitor/overview)
