---
title: 'Öğretici: Azure DevOps Projeleri kullanarak mevcut kodunuz için bir CI/CD işlem hattı oluşturun'
description: Azure DevOps projeleri, Azure'da kullanmaya başlamak kolaylaştırır. DevOps Projeleri, birkaç hızlı adımda seçtiğiniz bir Azure hizmetinde bir uygulamayı başlatmak için kendi kodunuzu ve GitHub deponuzu kullanmanıza yardımcı olur.
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
ms.date: 07/09/2018
author: mlearned
ms.custom: mvc
monikerRange: vsts
ms.openlocfilehash: 2abe24ad65e1e8997b48a28b35ec0e65162022f2
ms.sourcegitcommit: 3e7646d60e0f3d68e4eff246b3c17711fb41eeda
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2019
ms.locfileid: "70898003"
---
# <a name="tutorial-create-a-cicd-pipeline-for-your-existing-code-by-using-azure-devops-projects"></a>Öğretici: Azure DevOps Projeleri kullanarak mevcut kodunuz için bir CI/CD işlem hattı oluşturun

Azure DevOps Projeleri, mevcut kodunuzu ve git deponuzu getirebileceğiniz veya Azure 'a sürekli tümleştirme (CI) ve sürekli teslim (CD) işlem hattı oluşturmak için örnek bir uygulama seçebileceğiniz basitleştirilmiş bir deneyim sunar.

Yapacaklarınız:

> [!div class="checklist"]
> * CI/CD işlem hattı oluşturmak için DevOps Projeleri kullanma
> * GitHub deponuza erişimi yapılandırma ve bir çerçeve seçme
> * Azure DevOps ve Azure aboneliğinin yapılandırın 
> * Değişiklikleri GitHub 'a işleyin ve Azure 'a otomatik olarak dağıtın
> * Azure Pipelines CI/CD işlem hattını inceleyin
> * Application Insights izlemeyi yapılandırma
> * Kaynakları temizleme

## <a name="prerequisites"></a>Önkoşullar

* Azure aboneliği. [Visual Studio Dev Essentials](https://visualstudio.microsoft.com/dev-essentials/) aracılığıyla ücretsiz bir abonelik alabilirsiniz.
* .NET, Java, PHP, Node, Python veya static Web kodu içeren bir GitHub veya dış git deposuna erişim.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

Azure DevOps Projeleri, Azure Pipelines bir CI/CD işlem hattı oluşturur. Yeni bir Azure DevOps kuruluşu oluşturabilir veya var olan bir kuruluşu kullanabilirsiniz. Azure DevOps Projeleri ayrıca Azure kaynaklarını tercih ettiğiniz Azure aboneliğinde de oluşturur.

1. [Azure Portal](https://portal.azure.com) oturum açın.

1. Sol bölmede **Yeni**' yi seçin.

1. Arama kutusuna **DevOps Projeleri**yazın ve ardından **Oluştur**' u seçin.

    ![DevOps Projeleri panosu](_img/azure-devops-project-github/fullbrowser.png)

1. **Kendi kodunuzu getir**' i seçin ve ardından **İleri**' yi seçin.

## <a name="configure-access-to-your-github-repo-and-choose-a-framework"></a>GitHub deponuza erişimi yapılandırma ve bir çerçeve seçme

1. **GitHub** veya harici bir git deposu seçin ve ardından deponuzu ve uygulamanızı içeren dalı seçin.

1. Web çatısını seçin ve ardından **İleri**' yi seçin.

    ![.NET Framework](_img/azure-devops-project-github/webframework.png)

    Daha önce seçtiğiniz uygulama çerçevesi, burada kullanılabilir olan Azure hizmet dağıtım hedefinin türünü belirler. 
    
1. Hedef hizmeti seçin ve ardından **İleri**' yi seçin.

## <a name="configure-azure-devops-and-an-azure-subscription"></a>Azure DevOps ve Azure aboneliğinin yapılandırın 

1. Yeni bir Azure DevOps organizasyonu oluşturun veya var olan bir kuruluşu seçin.

    a. Azure DevOps 'da projeniz için bir ad girin. 
    
    b. Azure aboneliğinizi ve konumunuzu seçin, uygulamanız için bir ad girin ve **bitti**' yi seçin.

    DevOps projeleri Pano, birkaç dakika sonra Azure portalında görüntülenir. Azure DevOps kuruluşunuzda bir depoda bir örnek uygulama ayarlanır, bir derleme yürütülür ve uygulamanız Azure 'a dağıtılır. Bu pano GitHub Code deponuz, CI/CD işlem hattı ve Azure 'daki uygulamanız için görünürlük sağlar. 
    
1. Çalışan uygulamanızı görüntülemek için **Araştır** ' ı seçin.

    ![DevOps Projeleri pano görünümü](_img/azure-devops-project-github/dashboardnopreview.png) 
    
Azure DevOps Projeleri otomatik olarak bir CI derleme ve yayın tetikleyicisi yapılandırır. Kodunuz, GitHub deponuzda veya başka bir dış depoda kalır. 

## <a name="commit-changes-to-github-and-automatically-deploy-them-to-azure"></a>Değişiklikleri GitHub 'a işleyin ve Azure 'a otomatik olarak dağıtın 

Şimdi, en son çalışmalarınızı otomatik olarak Web sitenize dağıtan bir CI/CD işlemi kullanarak uygulamanızdaki bir takımla işbirliği yapmaya hazırsınız. GitHub deposunda yapılan her değişiklik Azure DevOps 'da bir derleme başlatır ve bir CD işlem hattı Azure 'a bir dağıtım yürütür.

1. Uygulamanızda bir değişiklik yapın ve ardından değişikliği GitHub deponuzda yürütün.  
    Birkaç dakika sonra, bir derleme Azure Pipelines başlar. DevOps Projeleri panosunda derleme durumunu izleyebilir veya Azure DevOps kuruluşunuzla tarayıcıda izleyebilirsiniz.

1. Derleme tamamlandıktan sonra, değişikliklerinizi doğrulamak için uygulamanızı yenileyin.

## <a name="examine-the-azure-pipelines-cicd-pipeline"></a>Azure Pipelines CI/CD işlem hattını inceleyin

Azure DevOps Projeleri, Azure Pipelines bir CI/CD işlem hattını otomatik olarak yapılandırır. İşlem hattını gerektiği şekilde keşfedin ve özelleştirin. Derleme ve yayın işlem hatları hakkında bilgi edinmek için aşağıdakileri yapın:

1. DevOps Projeleri panosunun en üstünde **derleme işlem hatları**' nı seçin.  
    Bir tarayıcı sekmesi, yeni projeniz için derleme işlem hattını görüntüler.

1. **Durum** alanını işaret edin ve ardından üç nokta (...) simgesini seçin.  
    Bir menü, yeni bir derlemeyi sıraya alma, bir derlemeyi duraklatma ve derleme işlem hattını düzenlemeyle çeşitli seçenekleri görüntüler.

1. **Düzenle**’yi seçin.

1. Bu bölmede, derleme işlem hattı için çeşitli görevleri inceleyebilirsiniz.  
    Derleme git deposundan kaynak getirme, bağımlılıkları geri yükleme ve dağıtımlar için kullanılan yayınlama çıkışları gibi çeşitli görevleri gerçekleştirir.

1. Derleme işlem hattı üstünde derleme işlem hattı adı seçin.

1. Bir şeyler daha açıklayıcı, select, derleme işlem hattı adını değiştirmek **Kaydet ve kuyruğa**ve ardından **Kaydet**.

1. Derleme işlem hattı adınızın altında **Geçmiş**’i seçin.  
    Derleme için yaptığınız son değişikliklere ait denetim kaydını görürsünüz. Azure DevOps, derleme ardışık düzeninde yapılan tüm değişiklikleri izler ve sürümleri karşılaştırmanızı sağlar.

1. **Tetikleyiciler**’i seçin.  
    Azure DevOps Projeleri otomatik olarak bir CI tetikleyicisi oluşturur ve depoya yapılan her bir işleme yeni bir derleme başlatır. İsteğe bağlı olarak, CI işlemindeki dalları dahil etmek veya hariç tutmak seçebilirsiniz.

1. **Saklama**’yı seçin.  
        Senaryonuza bağlı olarak, saklamak veya belirli bir sayıda derlemeleri kaldırmak için ilkeleri belirtebilirsiniz.

1. **Build ve Release**' i seçin ve ardından **yayınlar**' ı seçin.  
    Azure DevOps Projeleri, Azure dağıtımlarını yönetmek için bir yayın işlem hattı oluşturur.

1. Yayın işlem hattının yanındaki üç nokta (...) simgesini seçin ve ardından **Düzenle**' yi seçin.  
    Yayın işlem hattı, yayın işlemini tanımlayan bir *işlem hattı* içerir. 
    
1. **Yapıtlar**’ın altında **Bırak**’ı seçin.  
    Önceki adımlarda inceettiğiniz derleme işlem hattı, yapıt için kullanılan çıktıyı üretir. 

1. **Bırakma** simgesinin yanındaki **sürekli dağıtım tetikleyicisi**' ni seçin.  
    Bu yayın işlem hattı, yeni bir derleme yapıtı kullanılabilir olduğunda bir dağıtım yürüten CD tetikleyicisini etkinleştirdi. İsteğe bağlı olarak, el ile yürütme dağıtımlarınızı gerektirir böylece tetikleyiciyi devre dışı bırakabilirsiniz. 

1. Solda, **Görevler**' i seçin.  
    Görevler, dağıtım işleminizin çalıştırdığı etkinliklerdir. Bu örnekte, Azure App Service 'e dağıtmak için bir görev oluşturulmuştur.

1. Sağ tarafta, sürümlerin geçmişini görüntülemek için **yayınları görüntüle** ' yi seçin.

1. Bir yayının yanındaki üç nokta (...) simgesini seçin ve sonra **Aç**' ı seçin.  
    Yayın özeti, ilişkili iş öğeleri ve test gibi keşfetmek için birkaç menüleri vardır.

1. **İşlemeler**'i seçin.  
    Bu görünüm, bu dağıtımla ilişkili kod işlemelerini gösterir. 

1. **Günlükler**’i seçin.  
    Günlüklerde, dağıtım işlemiyle ilgili yararlı bilgiler bulunur. Bunları, dağıtımları sırasında ve sonrasında görüntüleyebilirsiniz.

## <a name="configure-azure-application-insights-monitoring"></a>Application Insights izlemeyi yapılandırma

Azure Application Insights ile, uygulamanızın performansını ve kullanımını kolayca izleyebilirsiniz. Azure DevOps Projeleri, uygulamanız için Application Insights kaynağını otomatik olarak yapılandırır. Gerekirse başka uyarılar ve izleme özellikleri de yapılandırabilirsiniz.

1. Azure portal, DevOps Projeleri panosuna gidin. 

1. Sağ alt köşedeki uygulamanızın **Application Insights** bağlantısını seçin.  
    **Application Insights** bölmesi açılır. Bu görünüm uygulamanızın kullanım, performans ve kullanılabilirlik izleme bilgilerini içerir.

    ![Application Insights bölmesi](_img/azure-devops-project-github/appinsights.png) 

1. **Zaman aralığı**' nı seçin ve ardından **son saat**' i seçin. Sonuçları filtrelemek için **Güncelleştir**' i seçin.  
    Artık son 60 dakikadan tüm etkinlikleri görüntüleyebilirsiniz. Zaman aralığından çıkmak için **x**' i seçin.

1. **Uyarılar**' ı seçin ve ardından **ölçüm uyarısı Ekle**' yi seçin. 

1. Uyarı için bir ad girin.

1. **Kaynak değiştirme açık** açılan listesinde App Service kaynağınızı seçin **.** <!-- Please confirm whether this should be "Source Alter on" or "Source Alert on" -->

1. **Ölçüm** açılan listesinde çeşitli uyarı ölçümlerini inceleyin.  
    Varsayılan uyarı, **1 saniyeden uzun sunucu yanıt süresi** içindir. Çeşitli uyarıları kolayca yapılandırıp uygulamanızın izleme özelliklerini geliştirebilirsiniz.

1. **E-posta sahipleri, katkıda bulunanlar ve okuyucular aracılığıyla bildir** onay kutusunu seçin.  
    İsteğe bağlı olarak, bir Azure mantıksal uygulaması yürüterek bir uyarı görüntülendiğinde ek eylemler gerçekleştirebilirsiniz.

1. Uyarıyı oluşturmak için **Tamam ' ı** seçin.  
    Birkaç dakika sonra, uyarı panoda etkin olarak görünür.
    
1. **Uyarılar** alanından çıkıp **Application Insights** bölmesine geri dönün.

1. **Kullanılabilirlik**' i seçin ve ardından **Test Ekle**' yi seçin. 

1. Bir test adı girin ve ardından **Oluştur**' u seçin.  
    Uygulamanızın kullanılabilirliğini doğrulamak için basit bir ping testi oluşturulur. Birkaç dakika sonra, test sonuçları sağlanır ve Application Insights panosu bir kullanılabilirlik durumu görüntüler.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olmadığında Azure App Service 'i ve bu öğreticide oluşturduğunuz ilgili kaynakları silebilirsiniz. Bunu yapmak için DevOps Projeleri panosundaki **silme** işlevini kullanın.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide CI/CD işleminizi yapılandırdığınızda, Azure DevOps Projeleri içinde otomatik olarak bir derleme ve yayın işlem hattı oluşturuldu. Ekibinizin ihtiyaçlarını karşılamak için bu derleme ve yayın işlem hatlarını değiştirebilirsiniz. Şunları öğrendiniz:

> [!div class="checklist"]
> * CI/CD işlem hattı oluşturmak için DevOps Projeleri kullanma
> * GitHub deponuza erişimi yapılandırma ve bir çerçeve seçme
> * Azure DevOps ve Azure aboneliğinin yapılandırın 
> * Değişiklikleri GitHub 'a işleyin ve Azure 'a otomatik olarak dağıtın
> * Azure Pipelines CI/CD işlem hattını inceleyin
> * Application Insights izlemeyi yapılandırma
> * Kaynakları temizleme

CI/CD işlem hattı hakkında daha fazla bilgi edinmek için bkz.:

> [!div class="nextstepaction"]
> [Çoklu aşamalı sürekli dağıtım (CD) işlem hattınızı tanımlama](https://docs.microsoft.com/azure/devops/pipelines/release/define-multistage-release-process?view=vsts)
