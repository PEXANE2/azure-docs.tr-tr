---
title: 'Öğretici: Azure DevOps Projelerini kullanarak varolan kodunuz için bir CI/CD ardışık kaynak oluşturma'
description: Azure DevOps Projeleri, Azure'a başlamayı kolaylaştırır. DevOps Projects birkaç hızlı adımda, azure hizmetinde bir uygulama başlatmak için kendi kodunuzu ve GitHub repo'nuzu kullanmanıza yardımcı olur.
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
ms.openlocfilehash: a0496999a9a5dc3e9bfd57df0ec035e6db77d620
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "73615119"
---
# <a name="tutorial-create-a-cicd-pipeline-for-your-existing-code-by-using-azure-devops-projects"></a>Öğretici: Azure DevOps Projelerini kullanarak varolan kodunuz için bir CI/CD ardışık kaynak oluşturma

Azure DevOps Projeleri, Azure'a sürekli tümleştirme (CI) ve sürekli teslim (CD) ardışık hatlar oluşturmak için basitleştirilmiş bir işlem sunar. Varolan kodunuzu ve Git repo'nuzu getirebilir veya örnek bir uygulama seçebilirsiniz.

Yapacaklarınız:

> [!div class="checklist"]
> * CI/CD ardışık hatlar oluşturmak için DevOps Projelerini kullanma
> * GitHub repo'nuza erişimi yapılandırın ve bir çerçeve seçin
> * Azure DevOps'leri ve Azure aboneliğini yapılandırma 
> * Değişiklikleri GitHub'a işleme ve bunları otomatik olarak Azure'a dağıtma
> * Azure Ardışık Hatları CI/CD ardışık hattını inceleyin
> * Kaynakları temizleme

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. [Visual Studio Dev Essentials](https://visualstudio.microsoft.com/dev-essentials/) aracılığıyla ücretsiz bir abonelik alabilirsiniz.
* .NET, Java, PHP, Node.js, Python veya statik web kodu içeren bir GitHub veya harici Git repo'ya erişim.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

Azure DevOps Projeleri, Azure Ardışık Hatları'nda bir CI/CD ardışık alanı oluşturur. Yeni bir Azure DevOps kuruluşu oluşturabilir veya varolan bir kuruluş kullanabilirsiniz. Azure DevOps Projeleri, seçtiğiniz Azure aboneliğinde Azure kaynakları da oluşturur.

1. [Azure portalında](https://portal.azure.com)oturum açın.

2. Azure portalı menüsünden **kaynak oluştur'u**seçin.

   ![Azure portal menüsü - Kaynak oluşturma](_img/azure-devops-project-github/createaresource.png)

3. **Devops** > **Devops Projesini**seçin.

   ![DevOps Projeleri panosu](_img/azure-devops-project-github/azuredashboard.png)

1. **Kendi kodunuzu Getir'i**seçin ve sonra **İleri'yi**seçin.

## <a name="configure-access-to-your-github-repo-and-select-a-framework"></a>GitHub repo'nuza erişimi yapılandırın ve bir çerçeve seçin

1. **GitHub** veya **harici** git kodu deposunu seçin. Bu öğretici için **GitHub'ı**seçin. Azure'un GitHub deponuza erişmesine izin vermek için ilk kez GitHub ile kimlik doğrulamanız gerekebilir.

1. Bir Depo ve **Dal**seçin ve sonra **İleri'yi**seçin. **Repository**

1. Docker kapsayıcıları kullanıyorsanız, değişiklik EVET için **YES** **uygulama Dockerized olduğunu.** Bu öğretici için **NO'nun** seçilmesini ve ardından **İleri'yi**seçin. Docker kapsayıcıları kullanma hakkında daha fazla bilgi için **i** simgesinin üzerine titreyin.

   ![Açılan menüde uygulama çerçevesi seçimi](_img/azure-devops-project-github/appframework.png)

1. Açılan menülerden, bir uygulama **çalışma zamanı** ve **uygulama çerçevesi**seçin ve sonra **İleri'yi**seçin. Uygulama çerçevesi, kullanılabilir Azure hizmet dağıtım hedefinin türünü belirler.

1. Uygulamayı dağıtmak için bir **Azure hizmeti** seçin ve sonra **İleri'yi**seçin.

## <a name="configure-azure-devops-and-an-azure-subscription"></a>Azure DevOps'leri ve Azure aboneliğini yapılandırma

1. **Proje adı**için bir ad girin.

1. **Azure DevOps Organizasyonu'nda** yeni bir ücretsiz kuruluş oluşturun veya açılır menüden varolan bir kuruluş seçin.

1. **Azure Aboneliği'nde**aboneliğinizi seçin ve **Web uygulamasına** bir ad girin veya varsayılan ı kullanın. **Konum'u**seçin ve ardından **Bitti'yi**seçin. Birkaç dakika sonra, DevOps Projects dağıtım genel bakışı Azure portalında görüntülenir.

1. DevOps Projeleri panosunu görüntülemek için **kaynağa git'i** seçin. Sağ üst köşede, hızlı erişim için **Proje'yi** panonuza sabitle. Azure DevOps Projects, bir CI yapı oluşturma ve sürüm tetikleyicisi otomatik olarak yapılandırır. Kodunuz GitHub repo'nuzda veya başka bir harici repo'nuzda kalır ve Azure **DevOps Organizasyonu'nda**bir repo'da örnek bir uygulama ayarlanır. Azure DevOps Projects yapıyı çalıştırıyor ve uygulamayı Azure'a dağıtıyor.

   ![Azure DevOps Projeleri pano görünümü](_img/azure-devops-project-github/projectsdashboard.png)

1. Pano, kod reponuzu, CI/CD ardınızı ve uygulamanızı Azure'da gösterir. Sağdaki, Azure kaynakları altında, çalışan uygulamanızı görüntülemek için **Gözat'ı** seçin.

## <a name="commit-changes-to-github-and-automatically-deploy-them-to-azure"></a>Değişiklikleri GitHub'a işleme ve bunları otomatik olarak Azure'a dağıtma

Artık uygulamanızda bir ekiple işbirliği yapmaya hazırsınız. CI/CD işlemi en son çalışmanızı otomatik olarak web sitenize dağır. GitHub repo'sundaki her değişiklik Azure DevOps'te bir yapı yı başlatır ve CD ardışık bir yapı Azure'a dağıtım çalıştırır.

1. DevOps Projects panonuzdan, **Depolar'ı**seçin. GitHub deponuz yeni bir tarayıcı sekmesinde açılır. Uygulamanızda değişiklik yapın ve ardından **Değişiklikleri Gerçekleştir'i**seçin.

1. Birkaç dakika sonra Azure Ardışık Hatlar'da bir yapı başlar. DevOps Projeleri panosunda yapı durumunu izleyebilirsiniz. DevOps Projects panosundan **Yapı denetim dizileri** sekmesini seçerek Azure DevOps kuruluşunuzda da izleyebilirsiniz.

## <a name="examine-the-azure-pipelines-cicd-pipeline"></a>Azure Ardışık Hatları CI/CD ardışık hattını inceleyin

Azure DevOps Projects, Azure Ardışık Hatları'nda bir CI/CD ardışık hattını otomatik olarak yapılandırır. İşlem hattını gerektiği şekilde keşfedin ve özelleştirin. Yapı ve sürüm ardışık hatları hakkında bilgi almak için aşağıdakileri yapın:

1. DevOps Projects panosundan **yapı boru hatlarını**seçin.

1. Azure **Ardışık Hatları** sayfanız açıldıktan sonra, en son yapılana ve her yapının durumunun geçmişini görürsünüz.

   ![Azure Pipelines sayfası oluşturur](_img/azure-devops-project-github/pipelinesbuildpage.png)

1. **Yapılar** sayfasının sağ üst köşesinde, geçerli yapıyı değiştirmek için **Edit'i,** yeni bir yapı eklemek için **Sıra'yı** veya daha fazla seçenek içeren bir menüyü açmak için dikey elips düğmesini** (&#8942;) **seçebilirsiniz. **Edit'i**seçin.

1. Yapı, repo'dan kaynak alma, bağımlılıkları geri getirme ve dağıtımlar için çıktıyayımyayınlama gibi çeşitli görevler yapar. Sağda, **Ad**altında, yapı ardışık yapı adı daha açıklayıcı bir şey değiştirin. **& Sırayı Kaydet'i**ve ardından **Kaydet'i**seçin. Bir açıklama girin ve ardından yeniden **Kaydet'i** seçin.

   ![Azure DevOps sayfası oluşturur](_img/azure-devops-project-github/buildpage.png)

1. Yapı için son değişikliklerinizin denetim izini görmek için **Geçmiş** sekmesini seçin.  Azure DevOps, yapı ardışık yapısında yapılan değişiklikleri izler ve sürümleri karşılaştırmanızı sağlar.

1. **Tetikleyiciler** sekmesini seçin. Azure DevOps Projeleri, bazı varsayılan ayarlarla otomatik olarak bir CI tetikleyicisi oluşturur. Her kod değişikliği yaptığınızda bir yapıyı çalıştırmak için **sürekli tümleştirmeyi etkinleştir** gibi tetikleyiciler ayarlayabilirsiniz. Tetikleyicileri belirli zamanlarda çalışacak şekilde zamanlamak üzere de ayarlayabilirsiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Azure Uygulama Hizmeti'ne ve bu eğitimde oluşturduğunuz ilgili kaynaklara artık ihtiyacınız olmadığında bunları silebilirsiniz. DevOps Projeleri panosundaki **Sil** işlevini kullanın.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide CI/CD işleminizi yapılandırdığınızda, Azure DevOps Projelerinde otomatik olarak bir yapı ve sürüm ardışık hattı oluşturdunuz. Ekibinizin ihtiyaçlarını karşılamak için bu derleme ve yayın işlem hatlarını değiştirebilirsiniz.

CI/CD ardışık boru hattı hakkında daha fazla bilgi için bkz:

> [!div class="nextstepaction"]
> [Çok aşamalı sürekli dağıtım (CD) ardışık sisteminizi tanımlayın](https://docs.microsoft.com/azure/devops/pipelines/release/define-multistage-release-process?view=vsts)

Uygulama izleme hakkında daha fazla bilgi edinmek için bkz:
  
 > [!div class="nextstepaction"]
 > [Azure monitör nedir?](https://docs.microsoft.com/azure/azure-monitor/overview)
