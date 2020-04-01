---
title: 'Öğretici: Azure DevOps Projeleri ile ASP.NET uygulamaları Azure İşlevlerini dağıtma'
description: Azure DevOps Projeleri, Azure'a başlamayı kolaylaştırır. DevOps Projects ile ASP.NET uygulamanızı birkaç hızlı adımda Azure İşlevlerine dağıtabilirsiniz.
ms.author: mlearned
ms.manager: gwallace
ms.prod: devops
ms.technology: devops-cicd
ms.topic: tutorial
ms.date: 06/20/2019
author: mlearned
monikerRange: vsts
ms.openlocfilehash: 4e7e9428af86f131632650f18d45e7dd48f4b5cb
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "71971568"
---
# <a name="continuously-deploy-to-azure-functions-with-devops-projects"></a>DevOps Projeleri ile Azure İşlevlerini sürekli olarak dağıtma

Azure DevOps Projeleri, mevcut kodunuzu ve Git repo'nuzu getirebileceğiniz veya Azure'a sürekli tümleştirme (CI) ve sürekli teslimat (CD) ardışık bir çözüm hattı oluşturmak için örnek bir uygulama seçebileceğiniz basitleştirilmiş bir deneyim sunar.

DevOps Projeleri de:

* Azure Fonksiyonları gibi Azure kaynaklarını otomatik olarak oluşturur

* CI/CD için Azure DevOps'lerde bir sürüm ardışık hattı oluşturur ve yapılandırır

Bu öğreticide şunları yapacaksınız:

> [!div class="checklist"]
>* ASP.NET bir uygulamayı Azure İşlevi'ne dağıtmak için DevOps Projelerini kullanma
>* Azure DevOps'leri ve Azure aboneliğini yapılandırma
>* Azure İşi'ni İnceleyin
>* CI işlem hattını inceleme
>* CD işlem hattını inceleme
>* Değişiklikleri Git'e işleme ve bunları otomatik olarak Azure'a dağıtma
>* Kaynakları temizleme

Şu anda işlevler için desteklenen çalışma süreleri **.NET** ve **Node.js'dir.** Biz kullanırız. Bu öğreticinin Azure İşlevlerine dağıtılaması için NET çalışma süresi. 

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. Visual Studio Dev [Essentials](https://visualstudio.microsoft.com/dev-essentials/) ile ücretsiz bir tane alabilirsiniz

## <a name="use-devops-projects-to-deploy-an-aspnet-app-to-azure-functions"></a>Azure İşlevleri'ne ASP.NET bir uygulama dağıtmak için DevOps Projelerini kullanma

DevOps Projects, Azure Ardışık Hatları'nda bir CI/CD ardışık alanı oluşturur. Yeni bir Azure DevOps kuruluşu oluşturabilir veya varolan bir kuruluş kullanabilirsiniz. DevOps Projects, seçtiğiniz Azure aboneliğinde IoTHub gibi Azure kaynakları da oluşturur.

1. [Azure portalında](https://portal.azure.com) oturum açın

1. Sol bölmede **Kaynak oluştur**'u seçin.

1. Arama **kutusunda, DevOps Projeleri**yazın ve sonra **Ekle'yi**tıklatın.

   ![DevOps Projeleri](_img/azure-devops-project-functions/devops-project.png)

1. **.NET'i**seçin ve sonra **İleri'yi**seçin. **Bir uygulama çerçevesi seçin,** **ASP.NET** seçin ve **İleri'yi**tıklatın.

1. **İşlev Uygulaması'nı** seçin ve sonra **İleri'yi**seçin.

## <a name="configure-azure-devops-and-azure-subscription"></a>Azure DevOps ve Azure aboneliğini yapılandırma

1. Azure DevOps projeniz için bir ad girin.

1. Yeni bir Azure DevOps kuruluşu oluşturun veya varolan bir kuruluş seçin.

1. Azure aboneliğinizi seçin.

1. Ek Azure yapılandırma ayarlarını görüntülemek ve fiyatlandırma katmanını ve konumunu belirlemek için Ek ayarları tıklatın. Bu bölme, Azure hizmetlerinin fiyatlandırma katmanını ve konumunu yapılandırmak için çeşitli seçenekler görüntüler.

1. Azure yapılandırma alanından çıkın ve ardından Bitti'yi seçin.

1. Birkaç dakika sonra işlem tamamlanır. Azure DevOps kuruluşunuzdaki Bir Git reposunda örnek ASP.NET uygulaması ayarlanır, Bir İşlev Uygulaması oluşturulur ve Uygulama Öngörüleri oluşturulur, bir CI/CD ardışık adı yürütülür ve uygulamanız Azure'a dağıtılır.

   Tüm bunlar tamamlandıktan sonra, Azure DevOps Project panosu Azure portalında görüntülenir. DevOps Projects panosuna doğrudan Azure portalındaki **tüm kaynaklardan** gidebilirsiniz.

   Bu pano, Azure DevOps kod deponuzda, CI/CD ardışık sisteminizde ve Azure İşi'nizde görünürlük sağlar. Azure DevOps ardışık sisteminizde ek CI/CD seçenekleri yapılandırabilirsiniz. Sağdaki işlev **uygulamasını** seçin.

## <a name="examine-the-function-app"></a>İşlev Uygulamasını İncele

DevOps Projects, keşfedebileceğiniz ve özelleştirebileceğiniz fonksiyon uygulamasını otomatik olarak yapılandırır. İşlev uygulamasını tanımak için aşağıdakileri yapın:

1. DevOps Projeleri panosuna gidin.

    ![DevOps Projeleri Panosu](_img/azure-devops-project-functions/devops-projects-dashboard.png)

1. Sağdaki işlev uygulamasını seçin. İşlev uygulaması için bir bölme açılır. Bu görünümden işlem izleme, günlükleri arama gibi çeşitli eylemler gerçekleştirebilirsiniz.

    ![İşlev Uygulaması](_img/azure-devops-project-functions/function-app.png)

## <a name="examine-the-ci-pipeline"></a>CI işlem hattını inceleme

DevOps Projects, Azure DevOps kuruluşunuzdaki BIR CI/CD ardışık hattını otomatik olarak yapılandırır. İşlem hattını inceleyebilir ve özelleştirebilirsiniz. Kendinizi tanımak için aşağıdakileri yapın:

1. DevOps Projeleri panosuna gidin.

1. **Build**altındaki köprüye tıklayın. Tarayıcı sekmesi, yeni projeniz için yapı ardışık hattını görüntüler.

    ![Yapı](_img/azure-devops-project-functions/build.png)

1. **Edit'i**seçin. Bu bölmede, yapı ardışık hattınız için çeşitli görevleri inceleyebilirsiniz. Yapı, Git repo'sundan kaynak kodu alma, uygulamayı oluşturma, birim testlerini çalıştırma ve dağıtımlar için kullanılan çıktıları yayımlama gibi çeşitli görevleri gerçekleştirir.

1. **Tetikleyiciler**’i seçin. DevOps Projeleri otomatik olarak bir CI tetikleyicisi oluşturur ve repo'ya her taahhüt yeni bir yapı başlatır. İsteğe bağlı olarak, ci işleminden dalları eklemeyi veya hariç tutmayı seçebilirsiniz.

1. **Saklama**’yı seçin. Senaryonuza bağlı olarak, belirli sayıda yapıyı tutmak veya kaldırmak için ilkeler belirtebilirsiniz.

1. Derleme işlem hattının üst kısmında derleme işlem hattı adı’nı seçin.

1. Yapı ardışık noktanızın adını daha açıklayıcı bir şeyle değiştirin ve ardından & sıra açılır **hale kaydet'i** seçin. **Save**

1. Derleme işlem hattı adınızın altında **Geçmiş**’i seçin. Bu bölme, yapı için son değişikliklerinizin denetim izini görüntüler. Azure DevOps yapı ardışık yapısında yapılan değişiklikleri izler ve sürümleri karşılaştırmanızı sağlar.

## <a name="examine-the-cd-release-pipeline"></a>CD sürüm ardışık hattını inceleyin

DevOps Projects, Azure DevOps kuruluşunuzdan Azure aboneliğinize dağıtmak için gerekli adımları otomatik olarak oluşturur ve yapılandırır. Bu adımlar, Azure DevOps'lerin Azure aboneliğinize kimlik doğrulaması yapmak için bir Azure hizmet bağlantısını yapılandırmayı içerir. Otomasyon, CD'yi Azure'a sağlayan bir sürüm ardışık hattı da oluşturur. Sürüm ardışık hattı hakkında daha fazla bilgi edinmek için aşağıdakileri yapın:

1. Boru Hatlarına Gidin **| Bültenleri**.

1. **Edit'e**tıklayın.

1. **Yapıtlar**’ın altında **Bırak**’ı seçin. Önceki adımlarda incelediğiniz yapı ardışık hattı, yapı için kullanılan çıktıyı üretir.

1. **Bırak** simgesinin sağında, **Sürekli dağıtım tetikleyicisini**seçin. Bu sürüm ardışık yapısı, yeni bir yapı artifakı kullanılabilir olduğunda dağıtımı yürüten etkin bir CD tetikleyicisi vardır. İsteğe bağlı olarak, dağıtımlarınızın el ile yürütülmesini gerektirecek şekilde tetikleyiciyi devre dışı kullanabilirsiniz.

1. Sağda, sürümgeçmişi görüntülemek için **görünüm sürümlerini** seçin.

1. Ardışık hattı görüntüleyecek sürüm, tıklayın. Sürüm **Özetini, Commits,** ilişkili Çalışma **Öğelerini**denetlemek için herhangi bir ortama tıklayın.

1. **İşlemeler**'i seçin. Bu görünüm, bu dağıtımla ilişkili kod işlemelerini gösterir. Dağıtımlar arasındaki işleme farklılıklarını görmek için yayınları karşılaştırın.

1. **Günlükleri Görüntüle'yi**seçin. Günlüklerde, dağıtım işlemiyle ilgili yararlı bilgiler bulunur. Bunları dağıtımlar sırasında ve sonrasında görüntüleyebilirsiniz.

## <a name="commit-code-changes-and-execute-cicd"></a>Kod değişikliklerini işleme ve CI/CD’yi yürütme

> [!NOTE]
> Aşağıdaki yordam, basit bir metin değişikliği yaparak CI/CD ardışık hattını sınar.

Artık en son çalışmanızı Azure İşlevinize otomatik olarak dağıtan bir CI/CD işlemi kullanarak uygulamanızdaki bir ekiple işbirliği yapmaya hazırsınız. Git repo'sundaki her değişiklik Azure DevOps'te bir yapı yı başlatır ve CD ardışık bir yapı Azure'a dağıtım yürütür. Bu bölümdeki yordamı izleyin veya repo'nuzda değişiklik yapmak için başka bir teknik kullanın. Örneğin, en sevdiğiniz araçta veya IDE'de Git repo'yu klonlayabilir ve değişiklikleri bu repo'ya itebilirsiniz.

1. Azure DevOps menüsünde **Repos | Dosyalar**ve sonra repo gidin.

1. Depo, oluşturma işleminde seçtiğiniz uygulama diline göre **SampleFunctionApp** adlı kodu zaten içerir. **Uygulama/SampleFunctionApp/Function1.cs** dosyasını açın.

1. **Düzenle'yi**seçin ve ardından **31 numaralı satırda** değişiklik yapın. Örneğin, orada Hello için **güncelleyebilirsiniz! DevOps Projelerini Kullanarak Azure Fonksiyonlarına Hoş Geldiniz**

1. Sağ üstte **Commit'i**seçin ve sonra değişikliğinizi zorlamak için yeniden **Commit'i** seçin.

1. **Uygulama/SampleFunctionApp.Test/Function1TestRunner.cs** dosyasını açın. 

1. **Düzenle'yi**seçin ve ardından **21 numaralı satırda**değişiklik yapın. Örneğin, orada Hello için **güncelleyebilirsiniz! Azure DevOps Projelerini kullanarak Azure İşlevlerini kullanmaya hoş geldiniz.**

     Birkaç dakika sonra, Azure DevOps'te bir yapı başlar ve değişiklikleri dağıtmak için bir sürüm yürütülür. DevOps Projects panosunda veya Azure DevOps kuruluşunuzla tarayıcıda yapı durumunu izleyin.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık ihtiyacınız olmadığında oluşturduğunuz ilgili kaynakları silebilirsiniz. DevOps Projeleri panosundaki **Sil** işlevini kullanın.

## <a name="next-steps"></a>Sonraki adımlar

Ekibinizin gereksinimlerine uygun olarak bu derleme ve yayın işlem hatlarını istediğiniz gibi değiştirebilirsiniz. Ayrıca bu CI/CD desenini diğer işlem hatlarınızda şablon olarak kullanabilirsiniz. Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * ASP.NET Core uygulamasını Azure İşlevi'ne dağıtmak için DevOps Projelerini kullanın
> * Azure DevOps'leri ve Azure aboneliğini yapılandırma 
> * Azure İşi'ni İnceleyin
> * CI işlem hattını inceleme
> * CD işlem hattını inceleme
> * Değişiklikleri Git'e işleme ve bunları otomatik olarak Azure'a dağıtma
> * Kaynakları temizleme

