---
title: 'Quickstart: Azure DevOps Projeleri ile PHP için BIR CI/CD ardışık kaynak oluşturma'
description: DevOps Projects, Azure'a başlamayı kolaylaştırır. Birkaç hızlı adımda, tercih ettiğiniz bir Azure hizmetinde uygulama başlatmanıza yardımcı olur.
ms.prod: devops
ms.technology: devops-cicd
services: vsts
documentationcenter: vs-devops-build
author: mlearned
manager: gwallace
editor: ''
ms.assetid: ''
ms.workload: web
ms.tgt_pltfrm: na
ms.topic: quickstart
ms.date: 07/09/2018
ms.author: mlearned
ms.custom: mvc
monikerRange: vsts
ms.openlocfilehash: 7c097c2a7247c9cad6cdce8a89059ee8d90d159e
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "70899585"
---
# <a name="create-a-cicd-pipeline-for-php-with-azure-devops-projects"></a>Azure DevOps Projeleri ile PHP için BIR CI/CD ardışık

Azure DevOps Projeleri, Azure kaynakları oluşturan ve Azure Boru Hatları'ndaki PHP uygulamanız için sürekli entegrasyon (CI) ve sürekli teslimat (CD) ardışık birimi oluşturan basitleştirilmiş bir deneyim sunar.  

Azure aboneliğiniz yoksa Visual [Studio Dev Essentials](https://visualstudio.microsoft.com/dev-essentials/)aracılığıyla ücretsiz olarak alabilirsiniz.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

 DevOps Projects, Azure Ardışık Hatları'nda bir CI/CD ardışık alanı oluşturur. Ücretsiz yeni bir Azure DevOps kuruluşu oluşturabilir veya varolan bir kuruluş kullanabilirsiniz. DevOps Projects, seçtiğiniz Azure aboneliğinde Azure kaynakları da oluşturur.

1. Microsoft Azure [portalında](https://portal.azure.com)oturum açın.

1. Sol bölmede, kaynak simgesi **oluştur'u** seçin ve ardından **DevOps Projeleri'ni**arayın.  

3. **Oluştur'u**seçin.

    ![Sürekli teslimat yapılandırması başlatma](_img/azure-devops-project-php/fullbrowser.png)

## <a name="select-a-sample-application-and-azure-service"></a>Örnek uygulama ve Azure hizmeti seçme

1. PHP örnek uygulamasını seçin.  
        PHP örnekleri çeşitli uygulama çerçeveleri bir seçim içerir. Varsayılan örnek çerçeve Laravel'dir. 
        
2. Varsayılan ayarı bırakın ve sonra **İleri'yi**seçin.  

1. Kapsayıcılar için Web App varsayılan dağıtım hedefidir.  
    Daha önce seçtiğiniz uygulama çerçevesi, burada kullanılabilen Azure hizmet dağıtım hedefitürünü belirler.  Varsayılan hizmeti bırakın ve sonra **İleri'yi**seçin.
 
## <a name="configure-azure-devops-and-an-azure-subscription"></a>Azure DevOps'leri ve Azure aboneliğini yapılandırma 

1. Yeni bir Azure DevOps kuruluşu oluşturun veya varolan bir kuruluş seçin. 

    a. Azure DevOps'te projeniz için bir ad seçin. 
    
    b. Azure aboneliğinizi ve konumunuzu seçin, uygulamanız için bir ad girin ve ardından **Bitti'yi**seçin.   
        Birkaç dakika sonra DevOps Projects panosu Azure portalında görüntülenir. Azure DevOps kuruluşunuzdaki bir depoda örnek bir uygulama ayarlanır, yapı çalışır ve uygulamanız Azure'a dağıtılır. Bu pano, kod deponuzda, CI/CD ardınızda ve Azure'daki uygulamanızda görünürlük sağlar.  
        
2. Çalışan uygulamanızı görüntülemek için **Gözat'ı** seçin.

    ![Pano görünümü](_img/azure-devops-project-php/dashboardnopreview.png) 
    
   DevOps Projects otomatik olarak bir CI yapı ve sürüm tetikleyicisi yapılandırıldı.  Artık en son çalışmanızı otomatik olarak web sitenize dağıtan bir CI/CD işlemiyle PHP uygulaması üzerinde bir ekiple birlikte çalışmaya hazırsınız.

## <a name="commit-code-changes-and-execute-cicd"></a>Kod değişikliklerini işleme ve CI/CD’yi yürütme

 DevOps Projects, Azure Repos veya GitHub'da bir Git deposu oluşturur. Depoyu görüntülemek ve uygulamanızda kod değişiklikleri yapmak için aşağıdaki adımları izleyin:

1. DevOps Projeleri panosunun solunda ana şubenizin bağlantısını seçin.   
    Bu bağlantı yeni oluşturulan Git deposuna bir görünüm açar.

1. Depo kopya URL'sini görüntülemek için tarayıcının sağ üst kısmından **Kopya**’yı seçin.   
    Git deponuzu en sevdiğiniz IDE’de kopyalayabilirsiniz. Sonraki birkaç adımda, kod değişikliklerini doğrudan ana şubeye yapmak ve işlemek için web tarayıcısını kullanın.

1. Solda, **kaynaklar/görünümler/welcome.blade.php** dosyasına gidin.

1. **Edit'i**seçin ve ardından metnin bazılarına değişiklik yapın.  Örneğin, div etiketlerinden biri için metnin bir kısmını değiştirin.

1. **Commit'i**seçin ve değişikliklerinizi kaydedin.

1. Tarayıcınızda DevOps Projects panosuna gidin.  
Şimdi devam etmekte olan bir yapı görmeniz gerekir. Az önce yaptığınız değişiklikler otomatik olarak oluşturulur ve bir CI/CD boru hattı aracılığıyla dağıtılır.

## <a name="examine-the-cicd-pipeline"></a>CI/CD boru hattını inceleyin

 DevOps Projects, Azure Ardışık Hatları'nda tam bir CI/CD ardışık hattını otomatik olarak yapılandırır. İşlem hattını gerektiği şekilde keşfedin ve özelleştirin. Yapı ve sürüm ardışık hatları hakkında bilgi almak için aşağıdakileri yapın:

1. DevOps Projeleri panosunun üst **kısmında, Yapı Boru Hatları'nı**seçin.  
    Bu bağlantı, yeni projeniz için bir tarayıcı sekmesi ve yapı ardışık hattı açılır.

1. **Durum** alanını işaret edin ve sonra **elipsleri** seçin (...).  
    Menü, yeni bir yapıyı sıraya alma, yapıyı duraklatma ve yapı ardışık hattını düzenleme gibi çeşitli seçenekler görüntüler.

1. **Edit'i**seçin.

1. Bu bölmede, yapı ardışık hattınız için çeşitli görevleri inceleyebilirsiniz.  
    Yapı, Git deposundan kaynak alma, bağımlılıkları geri getirme ve dağıtımlar için kullanılan çıktıları yayımlama gibi çeşitli görevleri çalıştırır.

1. Derleme işlem hattının üst kısmında derleme işlem hattı adı’nı seçin.

1. Yapı ardışık noktanızın adını daha açıklayıcı bir şeyle değiştirin, seçin, **& sırasını kaydedin**ve ardından **Kaydet'i**seçin.

1. Derleme işlem hattı adınızın altında **Geçmiş**’i seçin.   
    **Geçmiş** bölmesi, yapı için son değişikliklerinizin denetim izini görüntüler. Azure Ardışık Hatları, yapı ardışık yapısında yapılan değişiklikleri izler ve sürümleri karşılaştırmanızı sağlar.

1. **Tetikleyiciler**’i seçin.  
      DevOps Projects otomatik olarak bir CI tetikleyicisi oluşturdu ve depoya her taahhüt yeni bir yapı başlatır. İsteğe bağlı olarak dalları CI işlemine dahil etmeyi veya işlemden hariç tutmayı seçebilirsiniz.

1. **Saklama**’yı seçin.   
    Senaryonuza bağlı olarak, belirli sayıda yapıyı tutmak veya kaldırmak için ilkeler belirtebilirsiniz.

1. **Oluştur ve Yayınla'yı**seçin ve ardından **Sürümler'i**seçin.  
     DevOps Projects, Azure'daki dağıtımları yönetmek için bir sürüm ardışık hattı oluşturur.

1. Sürüm ardışık ardışık alanınızın yanındaki elipsleri (...) seçin ve ardından **Edit'i**seçin.  
    Yayın işlem hattı, yayın işlemini tanımlayan bir işlem hattı içerir. 

12. **Yapıtlar**’ın altında **Bırak**’ı seçin.  
    Önceki adımlarda incelediğiniz yapı ardışık hattı, yapı için kullanılan çıktıyı üretir. 

1. **Bırak** simgesinin yanında **Sürekli dağıtım tetikleyicisini**seçin.   
    Bu sürüm ardışık yapısı, her yeni yapı artifakı olduğunda dağıtım çalıştıran etkin bir CD tetikleyicisi vardır.  İsteğe bağlı olarak, dağıtımlarınızın el ile yürütülmesini gerektirecek şekilde tetikleyiciyi devre dışı kullanabilirsiniz. 

1. Solda, **Görevler'i**seçin.  
        Görevler, dağıtım işleminizin gerçekleştirdiği etkinliklerdir.  Bu örnekte, Azure Uygulama Hizmetine dağıtmak için bir görev oluşturuldu.

1. Sağda, sürümgeçmişi görüntülemek için **görünüm sürümlerini** seçin.

1. Sürümlerinizden birinin yanındaki elipsleri (...) seçin ve ardından **Aç'ı**seçin.  
        Bu görünümde keşfedilebilecek yayın özeti, ilişkili iş öğeleri ve testler gibi çeşitli menüler vardır.

1. **İşlemeler**'i seçin.  
        Bu görünüm, belirli dağıtımile ilişkili kod işlemelerini gösterir. 

1. **Günlükleri**seçin.  
        Günlüklerde, dağıtım işlemiyle ilgili yararlı bilgiler bulunur. Bunlar hem dağıtım sırasında hem de sonrasında görüntülenebilir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Azure Uygulama Hizmeti'ni ve diğer ilgili kaynakları artık ihtiyacınız olmadığında silebilirsiniz. DevOps Projeleri panosundaki **Sil** işlevini kullanın.

## <a name="next-steps"></a>Sonraki adımlar

CI/CD işleminizi yapılandırdığınızda, yapı ve sürüm ardışık hatları otomatik olarak oluşturuldu. Ekibinizin ihtiyaçlarını karşılamak için bu derleme ve yayın işlem hatlarını değiştirebilirsiniz. CI/CD ardışık işlem hattı hakkında daha fazla bilgi edinmek için şu öğreticiye bakın:

> [!div class="nextstepaction"]
> [CD işlemini özelleştirme](https://docs.microsoft.com/azure/devops/pipelines/release/define-multistage-release-process?view=vsts)
