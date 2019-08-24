---
title: 'Hızlı Başlangıç: Azure DevOps Projeleri ile Node. js için bir CI/CD işlem hattı oluşturma'
description: DevOps Projeleri Azure 'u kullanmaya başlamanızı kolaylaştırır. Birkaç hızlı adımda, tercih ettiğiniz bir Azure hizmetinde uygulama başlatmanıza yardımcı olur.
ms.prod: devops
ms.technology: devops-cicd
services: vsts
documentationcenter: vs-devops-build
author: mlearned
manager: douge
editor: ''
ms.assetid: ''
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 07/09/2018
ms.author: mlearned
ms.custom: mvc
monikerRange: vsts
ms.openlocfilehash: eeab0433310892f27b9cb2d2eb6f64e17e6f14ef
ms.sourcegitcommit: 007ee4ac1c64810632754d9db2277663a138f9c4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/23/2019
ms.locfileid: "69991716"
---
#  <a name="create-a-cicd-pipeline-for--nodejs-with-azure-devops-projects"></a>Azure DevOps Projeleri ile Node. js için bir CI/CD işlem hattı oluşturma 

Azure DevOps Projeleri, Azure kaynakları oluşturan ve Azure Pipelines ' de Node. js uygulamanız için sürekli tümleştirme (CI) ve sürekli teslim (CD) işlem hattı ayarlayan basitleştirilmiş bir deneyim sunar.  

Azure aboneliğiniz yoksa, [Visual Studio Dev Essentials](https://visualstudio.microsoft.com/dev-essentials/) üzerinden ücretsiz edinebilirsiniz.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

DevOps Projeleri, Azure Pipelines bir CI/CD işlem hattı oluşturur. Yeni bir Azure DevOps kuruluşu oluşturabilir veya var olan bir kuruluşu kullanabilirsiniz. DevOps projeleri, Azure kaynaklarını da tercih ettiğiniz bir Azure aboneliği oluşturur.

1. [Microsoft Azure portalında](https://portal.azure.com) oturum açın.

1. Sol bölmede, **kaynak oluştur**' u seçin ve ardından **DevOps Projeleri**arayın. 

    ![Sürekli Teslim yapılandırmasını başlatma](_img/azure-devops-project-nodejs/fullbrowser.png)

## <a name="select-a-sample-application-and-azure-service"></a>Örnek uygulama ve Azure hizmeti seçme

1. Node. js örnek uygulamasını seçin.  
    Node.js örnekleri birkaç uygulama çerçevesi seçeneği içerir.

1. Varsayılan örnek Framework Express. js ' dir. Varsayılan ayarı bırakın ve ardından **İleri**' yi seçin.  
    **Windows üzerinde Web App** varsayılan dağıtım hedefidir.  Daha önce seçtiğiniz uygulama çerçevesi, burada kullanılabilir olan Azure hizmet dağıtım hedefinin türünü belirler.  

2. Varsayılan hizmeti bırakın ve ardından **İleri**' yi seçin.
 
## <a name="configure-azure-devops-and-an-azure-subscription"></a>Azure DevOps ve Azure aboneliğinin yapılandırın 

1. Yeni bir Azure DevOps organizasyonu oluşturun veya var olan bir kuruluşu seçin. 

    a. Projeniz için bir ad girin.

    b. Azure aboneliğinizi ve konumunuzu seçin, uygulamanız için bir ad girin ve **bitti**' yi seçin.  
    Birkaç dakika sonra, DevOps Projeleri panosu Azure portal görüntülenir. Örnek uygulama, Azure DevOps kuruluşunuzda bir depoda ayarlanır, bir derleme yürütülür ve uygulamanız Azure 'a dağıtılır. Bu pano, kod deposu, CI/CD işlem hattı ve uygulamanızı azure'da görünürlük sağlar.
     
3. Çalışan uygulamanızı görüntülemek için **Araştır** ' ı seçin.

    ![Pano görünümü](_img/azure-devops-project-nodejs/dashboardnopreview.png) 
    
DevOps Projeleri bir CI derleme ve yayın tetikleyicisi otomatik olarak yapılandırıldı.  Artık en son çalışmalarınızı Web sitenize dağıtan bir CI/CD işlemiyle bir Node. js uygulamasında bir takımla işbirliği yapmaya hazırsınız.

## <a name="commit-code-changes-and-execute-cicd"></a>Kod değişikliklerini işleme ve CI/CD’yi yürütme

DevOps Projeleri, Azure Repos veya GitHub 'da bir git deposu oluşturur. Depoyu görüntülemek ve uygulamanızda kod değişikliği yapmak için aşağıdaki adımları uygulayın.

1. DevOps Projeleri panosunun sol tarafında, ana dalınızın bağlantısını seçin.  
Bu bağlantı yeni oluşturulan Git deposuna bir görünüm açar.

1. Depo kopyası URL 'sini görüntülemek için tarayıcının sağ üst köşesindeki **Kopyala** ' yı seçin.   
    Git deponuzu en sevdiğiniz IDE’de kopyalayabilirsiniz. Sonraki birkaç adımda, kod değişiklikleri yapıp doğrudan ana dala işlemek için web tarayıcısını kullanabilirsiniz.

1. Tarayıcının sol tarafında **Görünümler/index. Pug** dosyasına gidin.

1. **Düzenle**' yi seçin ve ardından H2 başlığında bir değişiklik yapın.  
    Örneğin, **Azure DevOps Projeleri ile hemen kullanmaya** başlayın yazın veya başka bir değişiklik yapın.

1. Seçin **işleme**ve ardından değişikliklerinizi kaydedin.

1. Tarayıcınızda DevOps Projeleri panosuna gidin.   
Artık sürmekte olan bir derleme görmeniz gerekir. Yeni yaptığınız değişiklikler otomatik olarak bir CI/CD işlem hattı aracılığıyla oluşturulup dağıtılır.

## <a name="examine-the-azure-cicd-pipeline"></a>Azure CI/CD işlem hattını inceleme

Önceki adımda, DevOps Projeleri otomatik olarak tam CI/CD işlem hattı yapılandırdı. İşlem hattını gerektiği şekilde keşfedin ve özelleştirin. Derleme ve yayın işlem hatları hakkında bilgi edinmek için aşağıdaki adımları uygulayın.

1. DevOps projeleri panonun üst kısmında seçin **derleme işlem hatlarını**.  
Bu bağlantı, yeni projeniz için bir tarayıcı sekmesi ve derleme işlem hattı açar.

1. **Durum** alanını işaret edin ve ardından üç nokta (...) simgesini seçin.  
    Bu eylem, yeni bir derlemeyi sıraya alma, bir derlemeyi duraklatma ve derleme işlem hattını düzenlemeyle çeşitli etkinlikleri başlatabileceğiniz bir menü açar.

1. **Düzenle**’yi seçin.

1. Bu bölmede, derleme işlem hattı için çeşitli görevleri inceleyebilirsiniz.  
Derleme, git deposundan kaynak getirme, bağımlılıkları geri yükleme ve dağıtımlar için kullanılan çıkışları yayımlama gibi çeşitli görevler gerçekleştirir.

1. Derleme işlem hattı üstünde derleme işlem hattı adı seçin.

1. Bir şeyler daha açıklayıcı, select, derleme işlem hattı adını değiştirmek **Kaydet ve kuyruğa**ve ardından **Kaydet**.

1. Derleme işlem hattı adınızın altında **Geçmiş**’i seçin.   
İçinde **geçmişi** bölmesinde, bir denetim kaydı yaptığınız son değişiklikler derleme için bkz.  Azure işlem hatları için derleme işlem hattı yapılan değişiklikleri izler ve sürümleri karşılaştırmanızı sağlar.

1. **Tetikleyiciler**’i seçin.   
 DevOps projeleri CI tetikleyicisini otomatik olarak oluşturulan ve depoya her işleme, yeni bir yapı başlatır.  İsteğe bağlı olarak dalları CI işlemine dahil etmeyi veya işlemden hariç tutmayı seçebilirsiniz.

1. **Saklama**’yı seçin.   
Senaryonuza bağlı olarak, saklamak veya belirli bir sayıda derlemeleri kaldırmak için ilkeleri belirtebilirsiniz.

1. **Build ve Release**' i seçin ve ardından **yayınlar**' ı seçin.  
 DevOps Projeleri, Azure dağıtımlarını yönetmek için bir yayın işlem hattı oluşturur.

1. Sol tarafta, yayın işlem hattının yanındaki üç nokta (...) simgesini seçin ve ardından **Düzenle**' yi seçin.  
Yayın işlem hattı yayın işlemini tanımlar.

12. **Yapıtlar**’ın altında **Bırak**’ı seçin.  
    Önceki adımlarda incelediğiniz derleme işlem hattı, yapıt için kullanılan çıkışı üretir. 

1. Yanındaki **bırak** simgesini seçme **sürekli dağıtım tetikleyicisi**.  
Bu yayın işlem hattı yok her seferinde yeni bir derleme yapıtının kullanılabilir bir dağıtım çalıştığı etkin bir CD tetikleyicisine sahiptir. İsteğe bağlı olarak, el ile yürütme dağıtımlarınızı gerektirir böylece tetikleyiciyi devre dışı bırakabilirsiniz. 


1. Sol tarafta, seçin **görevleri**.   
Dağıtım işleminizin gerçekleştiren etkinlikler görevlerdir. Bu örnekte, Azure App Service dağıtmak üzere bir görev oluşturulmuştur.


1. Sağ tarafta seçin **yayınları görüntüleyebilir**.  
Bu görünümde yayın geçmişi gösterilir.

1. Sürümlerinizin birini yanındaki üç nokta (...) seçin ve ardından **açık**.  
Yayın özeti, ilişkili iş öğeleri ve test gibi keşfetmek için birkaç menüleri vardır.

1. **İşlemeler**'i seçin.   
Bu görünümde, belirli bir dağıtım ile ilişkili kod tamamlama gösterilir.

1. **Günlükler**’i seçin.  
Günlüklerde, dağıtım işlemiyle ilgili yararlı bilgiler bulunur. Bunlar hem dağıtım sırasında hem de sonrasında görüntülenebilir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Azure App Service ve artık gerekmediğinde, oluşturduğunuz ilgili diğer kaynakları silebilirsiniz. Kullanım **Sil** DevOps projeleri Pano işlevselliği.


## <a name="next-steps"></a>Sonraki adımlar

CI/CD işleminizi yapılandırdığınızda, derleme ve yayın işlem hatları otomatik olarak oluşturulmuştur. Ekibinizin ihtiyaçlarını karşılamak için bu derleme ve yayın işlem hatlarını değiştirebilirsiniz. CI/CD işlem hattı hakkında daha fazla bilgi edinmek için bkz.:

> [!div class="nextstepaction"]
> [CD işlemini özelleştirme](https://docs.microsoft.com/azure/devops/pipelines/release/define-multistage-release-process?view=vsts)

## <a name="videos"></a>Videolar

> [!VIDEO https://www.youtube.com/embed/3etwjubReJs]
