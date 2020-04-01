---
title: 'Öğretici: Azure DevOps Projeleri ile Azure Cosmos DB tarafından desteklenen Node.js uygulamalarını dağıtma'
description: Azure DevOps Projeleri, Azure'a başlamayı kolaylaştırır. DevOps Projects ile Azure Cosmos DB tarafından desteklenen Node.js uygulamanızı birkaç hızlı adımda Windows Web Uygulamasına dağıtabilirsiniz.
ms.author: mlearned
ms.manager: gwallace
ms.prod: devops
ms.technology: devops-cicd
ms.topic: tutorial
ms.date: 07/11/2019
author: mlearned
monikerRange: vsts
ms.openlocfilehash: 229b4b9f53ea3866dce1169645f6d6da20827271
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "73888899"
---
# <a name="deploy-nodejs-apps-powered-by-azure-cosmos-db-with-devops-projects"></a>DevOps Projeleri ile Azure Cosmos DB tarafından desteklenen Node.js uygulamalarını dağıtma

Azure DevOps Projects, Azure'a sürekli tümleştirme (CI) ve sürekli dağıtım (CD) ardışık hatlar oluşturabileceğiniz kolaylaştırılmış bir deneyim sunar. Bunu, varolan kodunuzu ve Git deposunuzu (repo) kullanarak veya örnek bir uygulama seçerek yaparsınız.

DevOps Projeleri de:

* Azure Cosmos DB, Azure Uygulama Öngörüleri, Azure Uygulama Hizmeti ve Uygulama Hizmeti planları gibi Azure kaynaklarını otomatik olarak oluşturur

* Azure DevOps'lerde BIR CI/CD sürüm ardışık hattı oluşturur ve yapılandırır

Bu öğreticide şunları yapacaksınız:

> [!div class="checklist"]
> * Azure Cosmos DB tarafından desteklenen bir Düğüm.js uygulamasını dağıtmak için DevOps Projelerini kullanın
> * Azure DevOps'leri ve Azure aboneliğini yapılandırma
> * Azure Cosmos DB'yi inceleyin
> * CI işlem hattını inceleme
> * CD işlem hattını inceleme
> * Değişiklikleri Git'e ada ve otomatik olarak Azure'a dağıtın
> * Kaynakları temizleme

## <a name="prerequisites"></a>Ön koşullar

[Visual Studio Dev Essentials](https://visualstudio.microsoft.com/dev-essentials/) üzerinden ücretsiz olarak ulaşabileceğiniz bir Azure aboneliğine ihtiyacınız vardır.

## <a name="use-devops-projects-to-deploy-nodejs-app"></a>Node.js uygulamasını dağıtmak için DevOps Projelerini kullanın

DevOps Projects, Azure Ardışık Hatları'nda bir CI/CD ardışık alanı oluşturur. Yeni bir Azure DevOps kuruluşu oluşturabilir veya varolan bir kuruluş kullanabilirsiniz. DevOps Projects, seçtiğiniz Azure aboneliğinde Azure Cosmos DB, Application Insights, App Service ve App Service planları gibi Azure kaynakları da oluşturur.

1. [Azure portalında](https://portal.azure.com)oturum açın.

1. Sol bölmede **Kaynak oluştur**'u seçin.

1. Arama **kutusunda, DevOps Projeleri'ni**girin ve sonra **Ekle'yi**seçin.

   ![DevOps Projeleri bölmesi](_img/azure-devops-project-cosmos-db/devops-project.png)

1. Çalışma zamanı olarak **Düğüm.js'yi** seçin ve sonra **İleri'yi**seçin. **Bir uygulama çerçevesi seçin**, **Express.js**seçin.

1. Etkinleştir bölümü **Cosmos DB**için **bir veritabanı ekleyin** ve sonra **İleri'yi**seçin.

    ![Veritabanı ekleyin](_img/azure-devops-project-cosmos-db/add-database.png)

    Azure DevOps Projeleri **Express.js,** **Sample Node.js uygulaması**ve **Sail.js**gibi çeşitli uygulama çerçevelerini destekler. Bu öğreticide **Express.js**kullanıyoruz.

1. Uygulamayı dağıtmak için bir Azure hizmeti seçin ve sonra **İleri'yi**seçin. Seçenekleriniz arasında Windows Web Uygulaması, Azure Kubernetes Hizmeti ve Kapsayıcılar için Azure Web Uygulaması yer almaktadır. Bu eğitimde, **Windows Web App**kullanın.

## <a name="configure-azure-devops-and-azure-subscription"></a>Azure DevOps ve Azure aboneliğini yapılandırma

1. Azure DevOps projeniz için bir ad girin.

1. Yeni bir Azure DevOps kuruluşu oluşturun veya varolan bir kuruluş seçin.

1. Azure aboneliğinizi seçin.

1. Ek Azure yapılandırma ayarlarını görüntülemek veya fiyatlandırma katmanını ve konumunu tanımlamak için **Ek ayarları**seçin. Bu bölme, Azure hizmetlerinin fiyatlandırma katmanını ve konumunu yapılandırmak için çeşitli seçenekler gösterir.

1. Azure yapılandırma alanından çıkın ve **ardından Bitti'yi**seçin.

1. İşlem birkaç dakika sonra sona erer. Azure DevOps kuruluşunuzdaki bir Git reposunda örnek bir Düğüm.js uygulaması ayarlanır. Daha sonra Azure Cosmos DB, App Service, App Service planı ve Application Insights kaynaklarının yanı sıra bir CI/CD ardışık alanı oluşturulur. Uygulamanız daha sonra Azure'a dağıtılır.

   Tüm bu işlemler bittikten sonra, Azure DevOps Project panosu Azure portalında görüntülenir. DevOps Projects panosuna doğrudan Azure portalındaki **tüm kaynaklardan** gidebilirsiniz.

   Bu pano, Azure DevOps kod deponuzda, CI/CD ardınızda ve Azure Cosmos DB veritabanınızda görünürlük sağlar. Azure DevOps ardışık sisteminizde ek CI/CD seçenekleri yapılandırabilirsiniz. Panonun sağ tarafında, bu seçenekleri görüntülemek için **Azure Cosmos DB'yi** seçin.

## <a name="examine-azure-cosmos-db"></a>Azure Cosmos DB'yi inceleyin

DevOps Projects, keşfedebileceğiniz ve özelleştirebileceğiniz Azure Cosmos DB'yi otomatik olarak yapılandırır. Azure Cosmos DB'yi tanımak için aşağıdakileri yapın:

1. DevOps Projeleri panosuna gidin.

    ![DevOps Projeleri panosu](_img/azure-devops-project-cosmos-db/devops-project-dashboard.png)

1. Sağdaki Azure Cosmos DB'yi seçin. Azure Cosmos DB için bir bölme açılır. Bu görünümden, işlemleri izleme ve günlükleri arama gibi çeşitli eylemler gerçekleştirebilirsiniz.

    ![Azure Cosmos DB bölmesi](_img/azure-devops-project-cosmos-db/cosmos-db.png)

## <a name="examine-the-ci-pipeline"></a>CI işlem hattını inceleme

DevOps Projects, Azure DevOps kuruluşunuzdaki BIR CI/CD ardışık hattını otomatik olarak yapılandırır. İşlem hattını inceleyebilir ve özelleştirebilirsiniz. Kendinizi tanımak için aşağıdakileri yapın:

1. DevOps Projeleri panosuna gidin.

1. **Build**altında köprü seçin. Tarayıcı sekmesi, yeni projeniz için yapı ardışık hattını görüntüler.

    ![Bölme oluşturma](_img/azure-devops-project-cosmos-db/build.png)

1. **Edit'i**seçin. Bu bölmede, yapı ardışık hattınız için çeşitli görevleri inceleyebilirsiniz. Yapı, Git repo'sundan kaynak kodu alma, uygulamayı oluşturma, birim testlerini çalıştırma ve dağıtımlar için kullanılan çıktıları yayımlama gibi çeşitli görevleri gerçekleştirir.

1. **Tetikleyiciler**’i seçin. DevOps Projeleri otomatik olarak bir CI tetikleyicisi oluşturur ve repo'ya her taahhüt yeni bir yapı başlatır. Dalları CI işleminden eklemeyi veya hariç tutmayı seçebilirsiniz.

1. **Saklama**’yı seçin. Senaryonuza bağlı olarak, belirli sayıda yapıyı tutmak veya kaldırmak için ilkeler belirtebilirsiniz.

1. Derleme işlem hattının üst kısmında derleme işlem hattı adı’nı seçin.

1. Yapı ardışık noktanızın adını daha açıklayıcı bir şeyle değiştirin ve ardından & sıra açılır **hale kaydet'i** seçin. **Save**

1. Derleme işlem hattı adınızın altında **Geçmiş**’i seçin. Bu bölme, yapı için son değişikliklerinizin denetim izini görüntüler. Azure DevOps, yapı ardışık yapısında yapılan değişiklikleri izler ve sürümleri karşılaştırmanızı sağlar.

## <a name="examine-the-cd-release-pipeline"></a>CD sürüm ardışık hattını inceleyin

DevOps Projects, Azure DevOps kuruluşunuzdan Azure aboneliğinize dağıtmak için gerekli adımları otomatik olarak oluşturur ve yapılandırır. Bu adımlar, Azure DevOps'lerin Azure aboneliğinize kimlik doğrulaması yapmak için bir Azure hizmet bağlantısını yapılandırmayı içerir. Otomasyon, CD'yi Azure'a sağlayan bir sürüm ardışık hattı da oluşturur. Sürüm ardışık hattı hakkında daha fazla bilgi edinmek için aşağıdakileri yapın:

1. **Pipelines'a** gidin ve **Sürümler'i**seçin.

1. **Edit'i**seçin.

1. **Yapıtlar**’ın altında **Bırak**’ı seçin. Önceki adımlarda incelediğiniz yapı ardışık hattı, yapı için kullanılan çıktıyı üretir.

1. **Bırak** simgesinin sağında, **Sürekli dağıtım tetikleyicisini**seçin. Bu sürüm ardışık etkisi, yeni bir yapı artifakı kullanılabilir olduğunda bir dağıtım yürüten sürekli dağıtım tetikleyicisini etkinleştirdi. Dağıtımlarınızın el ile yürütülmesi için tetikleyiciyi devre dışı kullanabilirsiniz.

1. Sağda, sürümgeçmişi görüntülemek için **Görünüm sürümleri** bölümünü seçin.

1. Ardışık hattı görüntüleyecek sürümü seçin. Sürüm özetini, taahhütlerini veya ilişkili iş öğelerini denetlemek için istediğiniz ortamı seçin.

1. **İşlemeler**'i seçin. Bu görünüm, bu dağıtımla ilişkili kod işlemelerini gösterir. Dağıtımlar arasındaki işleme farklılıklarını görmek için yayınları karşılaştırın.

1. **Günlükleri Görüntüle'yi**seçin. Günlüklerde, dağıtım işlemiyle ilgili yararlı bilgiler bulunur. Bunları dağıtımlar sırasında ve sonrasında görüntüleyebilirsiniz.

## <a name="commit-code-changes-and-execute-the-cicd-pipeline"></a>Kod değişiklikleri yapma ve CI/CD ardışık hattını yürütme

> [!NOTE]
> Aşağıdaki yordam, basit bir metin değişikliği yaparak CI/CD ardışık hattını sınar.

Artık en son çalışmanızı Uygulama Hizmetinize dağıtan bir CI/CD işlemi kullanarak uygulamanızdaki bir ekiple işbirliği yapmaya hazırsınız. Git repo'sundaki her değişiklik Azure DevOps'te bir yapı yı başlatır ve CD ardışık bir yapı Azure'a dağıtım yürütür. Bu bölümdeki yordamı izleyin veya repo'nuzda değişiklik yapmak için başka bir teknik kullanın. Örneğin, en sevdiğiniz araçta veya IDE'de Git repo'yu klonlayabilir ve değişiklikleri bu repo'ya itebilirsiniz.

1. Azure DevOps **menüsünde, Repos** ve ardından **Dosyalar'ı**seçin. O zaman repo'na git.

1. Repo zaten oluşturma işleminde seçtiğiniz uygulama dilini temel alan kod içerir. **Uygulama/görünümler/index.pug** dosyasını açın.

1. **Düzenle'yi**seçin ve ardından **15 numaralı satırda**değişiklik yapın. Örneğin, "Azure Cosmos DB tarafından desteklenen Azure Uygulama Hizmetine İlk dağıtımım" olarak değiştirebilirsiniz.

1. Sağ üst **köşede, Commit'i**seçin ve sonra değişikliğinizi zorlamak için yeniden **Commit'i** seçin.

     Birkaç saniye sonra, Azure DevOps'te bir yapı başlar ve değişiklikleri dağıtmak için bir sürüm yürütülür. DevOps Projects panosunda veya Azure DevOps kuruluşunuzla tarayıcıda yapı durumunu izleyin.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık ihtiyacınız olmadığında oluşturduğunuz ilgili kaynakları silin. DevOps Projeleri panosundaki **Sil** işlevini kullanın.

## <a name="next-steps"></a>Sonraki adımlar

Ekibinizin ihtiyaçlarını karşılamak için bu derleme ve yayın işlem hatlarını değiştirebilirsiniz. Ayrıca bu CI/CD desenini diğer işlem hatlarınızda şablon olarak kullanabilirsiniz. Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Azure Cosmos DB tarafından desteklenen bir Düğüm.js uygulamasını dağıtmak için DevOps Projelerini kullanın
> * Azure DevOps'leri ve Azure aboneliğini yapılandırma 
> * Azure Cosmos DB'yi inceleyin
> * CI işlem hattını inceleme
> * CD işlem hattını inceleme
> * Değişiklikleri Git'e işleme ve bunları otomatik olarak Azure'a dağıtma
> * Kaynakları temizleme

Bkz. Daha fazla bilgi ve sonraki adımlar için [çok aşamalı sürekli dağıtım (CD) ardışık sisteminizi tanımlayın.](https://docs.microsoft.com/azure/devops/pipelines/release/define-multistage-release-process?view=azure-devops&viewFallbackFrom=vsts)


