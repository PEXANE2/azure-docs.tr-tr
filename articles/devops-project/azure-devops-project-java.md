---
title: 'Quickstart: Java için BIR CI/CD ardışık kaynak oluşturma - Azure DevOps Projeleri'
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
ms.custom: mvc, seo-java-july2019, seo-java-august2019, seo-java-september2019
monikerRange: vsts
ms.openlocfilehash: 1a276770887bee39972ba8630fb13f52bcbe802d
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77049966"
---
# <a name="quickstart-set-up-a-cicd-pipeline-for-a-java-app-with-azure-devops-projects"></a>Quickstart: Azure DevOps Projeleri ile Java uygulaması için BIR CI/CD ardışık kaynak kurulumu

Bu hızlı başlangıçta, Azure Pipelines'daki Java uygulamanız için sürekli tümleştirme (CI) ve sürekli teslimat (CD) ardışık birimi oluşturmak için basitleştirilmiş Azure DevOps Projeleri deneyimini kullanırsınız. Uygulamanızı geliştirmek, dağıtmak ve izlemek için ihtiyacınız olan her şeyi ayarlamak için Azure DevOps Projelerini kullanabilirsiniz. 

## <a name="prerequisites"></a>Ön koşullar

- Etkin bir aboneliği olan bir Azure hesabı. [Ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) 
- Azure [DevOps](https://azure.microsoft.com/services/devops/) hesabı ve kuruluşu.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

DevOps Projects, Azure Ardışık Hatları'nda bir CI/CD ardışık alanı oluşturur. Yeni bir Azure DevOps kuruluşu oluşturabilir veya varolan bir kuruluş kullanabilirsiniz. DevOps Projects, seçtiğiniz Azure aboneliğinde Azure kaynakları da oluşturur.

1. [Azure portalında](https://portal.azure.com)oturum açın ve sol bölmede **kaynak oluştur'u**seçin. 

   ![Azure portalında Azure kaynağı oluşturma](_img/azure-devops-project-java/continuous-delivery-configuration-full-browser.png)

1. **DevOps Projelerini**arayın ve seçin ve ardından **Oluştur'u**seçin.

## <a name="select-a-sample-application-and-azure-service"></a>Örnek uygulama ve Azure hizmeti seçme

1. Java örnek uygulamasını seçin.  
Java örnekleri birkaç uygulama çerçevesi seçeneği içerir.

1. Varsayılan örnek çerçeve Spring’dir. Varsayılan ayarı bırakın ve sonra **İleri'yi**seçin.  Kapsayıcılar için Web App varsayılan dağıtım hedefidir. Daha önce seçtiğiniz uygulama çerçevesi, burada bulunan Azure hizmet dağıtım hedefitürünü belirler. 

2. Varsayılan hizmeti bırakın ve sonra **İleri'yi**seçin.
 
## <a name="configure-azure-devops-and-an-azure-subscription"></a>Azure DevOps'leri ve Azure aboneliğini yapılandırma 

1. Yeni bir Azure DevOps kuruluşu oluşturun veya var olan bir kuruluşu seçin. 
   
   1. Projeniz için bir ad seçin. 
   
   1. Azure aboneliğinizi ve konumunuzu seçin, uygulamanız için bir ad seçin ve ardından **Bitti'yi**seçin.  
   Birkaç dakika sonra DevOps Projects panosu Azure portalında görüntülenir. Azure DevOps kuruluşunuzdaki bir depoda örnek bir uygulama ayarlanır, bir yapı yürütülür ve uygulamanız Azure'a dağıtılır. Bu pano, kod deponuzda, CI/CD ardışık sisteminizde ve Azure'daki uygulamanızda görünürlük sağlar.
   
2. Çalışan uygulamanızı görüntülemek için **Gözat'ı** seçin.
   
   ![Azure portalında uygulama panosunun görüntüle](_img/azure-devops-project-java/azure-devops-application-dashboard.png) 

DevOps Projects otomatik olarak bir CI yapı ve sürüm tetikleyicisi yapılandırıldı.  Artık en son çalışmanızı otomatik olarak web sitenize dağıtan bir CI/CD işlemiyle Java uygulaması üzerinde bir ekiple birlikte çalışmaya hazırsınız.

## <a name="commit-code-changes-and-execute-cicd"></a>Kod değişikliklerini işleme ve CI/CD’yi yürütme

DevOps Projects, Azure Repos veya GitHub'da bir Git deposu oluşturur. Depoyu görüntülemek ve uygulamanızda kod değişiklikleri yapmak için aşağıdakileri yapın:

1. DevOps Projeleri panosunun solunda ana şubenizin bağlantısını seçin.  
Bu bağlantı yeni oluşturulan Git deposuna bir görünüm açar.

1. Depo klon URL'sini görüntülemek için tarayıcının sağ üst **kısmındaki Klon'u** seçin.   
    Git deponuzu en sevdiğiniz IDE’de kopyalayabilirsiniz. Sonraki birkaç adımda, kod değişiklikleri yapıp doğrudan ana dala işlemek için web tarayıcısını kullanabilirsiniz.

1. Tarayıcının sol tarafında, **src/main/webapp/index.html** dosyasına gidin.

1. **Edit'i**seçin ve ardından metnin bazılarına değişiklik yapın.
    Örneğin, div etiketlerinden biri için metnin bir kısmını değiştirin.

1. **Commit'i**seçin ve değişikliklerinizi kaydedin.

1. Tarayıcınızda DevOps Projects panosuna gidin.   
Şimdi devam etmekte olan bir yapı görmeniz gerekir. Az önce yaptığınız değişiklikler otomatik olarak oluşturulur ve bir CI/CD boru hattı aracılığıyla dağıtılır.

## <a name="examine-the-cicd-pipeline"></a>CI/CD boru hattını inceleyin

 Önceki adımda, DevOps Projects otomatik olarak tam bir CI/CD ardışık yapılandırıldı. İşlem hattını gerektiği şekilde keşfedin ve özelleştirin. Yapı ve sürüm boru hatları hakkında bilgi almak için aşağıdaki adımları atın.

1. DevOps Projeleri panosunun üst **kısmında, Yapı Boru Hatları'nı**seçin.  
Bu bağlantı, yeni projeniz için bir tarayıcı sekmesi ve yapı ardışık hattı açılır.

1. **Durum** alanını işaret edin ve sonra elipsleri seçin (...).  
    Bu eylem, yeni bir yapıyı sıraya alma, yapıyı duraklatma ve yapı ardışık düzeneği düzenleme gibi çeşitli etkinlikler başlatabileceğiniz bir menü açar.

1. **Edit'i**seçin.

1. Bu bölmede, yapı ardışık hattınız için çeşitli görevleri inceleyebilirsiniz.  
Yapı, Git deposundan kaynak alma, bağımlılıkları geri getirme ve dağıtımlar için kullanılan çıktıları yayımlama gibi çeşitli görevleri gerçekleştirir.

1. Derleme işlem hattının üst kısmında derleme işlem hattı adı’nı seçin.

1. Yapı ardışık noktanızın adını daha açıklayıcı bir şeyle değiştirin, **& sırasını kaydet'i**seçin ve sonra **Kaydet'i**seçin.

1. Derleme işlem hattı adınızın altında **Geçmiş**’i seçin.   
**Geçmiş** bölmesinde, yapı için son değişikliklerinizin bir denetim izini görürsünüz.  Azure Ardışık Hatları, yapı ardışık yapısında yapılan değişiklikleri izler ve sürümleri karşılaştırmanızı sağlar.

1. **Tetikleyiciler**’i seçin.   
 DevOps Projects otomatik olarak bir CI tetikleyicisi oluşturdu ve depoya her taahhüt yeni bir yapı başlatır.  İsteğe bağlı olarak dalları CI işlemine dahil etmeyi veya işlemden hariç tutmayı seçebilirsiniz.

1. **Saklama**’yı seçin.   
Senaryonuza bağlı olarak, belirli sayıda yapıyı tutmak veya kaldırmak için ilkeler belirtebilirsiniz.

1. **Oluştur ve Yayınla'yı**seçin ve ardından **Sürümler'i**seçin.  
 DevOps Projects, Azure'daki dağıtımları yönetmek için bir sürüm ardışık hattı oluşturur.

1. Solda, sürüm ardışık ardınızın yanındaki elipsleri (...) seçin ve ardından **Edit'i**seçin.  
Yayın işlem hattı, yayın işlemini tanımlayan bir işlem hattı içerir.  
    
12. **Yapıtlar**’ın altında **Bırak**’ı seçin.  
Önceki adımlarda incelediğiniz yapı ardışık hattı, yapı için kullanılan çıktıyı üretir. 

1. **Bırak** simgesinin yanında **Sürekli dağıtım tetikleyicisini**seçin.  
Bu sürüm ardışık alanı, her kullanılabilir yeni yapı artifakı olduğunda dağıtım çalıştıran etkin bir CD tetikleyicisi vardır. İsteğe bağlı olarak, dağıtımlarınızın el ile yürütülmesini gerektirecek şekilde tetikleyiciyi devre dışı kullanabilirsiniz. 

1. Solda, **Görevler'i**seçin.   
Görevler, dağıtım işleminizin gerçekleştirdiği etkinliklerdir. Bu örnekte, Azure Uygulama Hizmetine dağıtmak için bir görev oluşturuldu.

1. Sağda, görünüm **sürümlerini**seçin.  
Bu görünümde yayın geçmişi gösterilir.

1. Sürümlerinizden birinin yanındaki elipsleri (...) seçin ve ardından **Aç'ı**seçin.  
Sürüm özeti, ilişkili iş öğeleri ve testler gibi keşfedilecek birkaç menü vardır.

1. **İşlemeler**'i seçin.   
Bu görünüm, belirli dağıtımile ilişkili kod işlemelerini gösterir. 

1. **Günlükleri**seçin.  
Günlüklerde, dağıtım işlemiyle ilgili yararlı bilgiler bulunur. Bunlar hem dağıtım sırasında hem de sonrasında görüntülenebilir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Azure Uygulama Hizmeti'ni ve diğer ilgili kaynakları artık ihtiyacınız olmadığında silebilirsiniz. DevOps Projeleri panosundaki **Sil** işlevini kullanın.

## <a name="next-steps"></a>Sonraki adımlar

CI/CD işleminizi yapılandırdığınızda, yapı ve sürüm ardışık hatları otomatik olarak oluşturuldu. Ekibinizin ihtiyaçlarını karşılamak için bu derleme ve yayın işlem hatlarını değiştirebilirsiniz. CI/CD ardışık boru hattı hakkında daha fazla bilgi için bkz:

> [!div class="nextstepaction"]
> [CD işlemini özelleştirme](https://docs.microsoft.com/azure/devops/pipelines/release/define-multistage-release-process?view=vsts)
