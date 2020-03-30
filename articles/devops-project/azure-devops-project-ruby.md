---
title: 'Quickstart: Azure DevOps Projelerini kullanarak Ruby on Rails için bir CI/CD ardışık kaynak oluşturma'
description: Azure DevOps Projeleri, Azure'a başlamayı kolaylaştırır. Bir Azure hizmetinde ruby web uygulamasını birkaç hızlı adımda başlatabilirsiniz.
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
ms.openlocfilehash: 5cb47cdd76d1de284c4dc6dbdfbfedd0095653ab
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "70899576"
---
# <a name="create-a-cicd-pipeline-for-ruby-on-rails-by-using-azure-devops-projects"></a>Azure DevOps Projelerini kullanarak Ruby on Rails için bir CI/CD ardışık sistem hattı oluşturma

Azure DevOps Projeleri'ni kullanarak Ruby on Rails uygulamanız için sürekli tümleştirme (CI) ve sürekli teslimat (CD) yapılandırın. DevOps Projeleri, Azure DevOps yapı ve sürüm ardışık hattının ilk yapılandırmasını basitleştirir.

Azure aboneliğiniz yoksa, [Visual Studio Dev Essentials](https://visualstudio.microsoft.com/dev-essentials/) üzerinden ücretsiz edinebilirsiniz.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

Azure DevOps Projeleri, Azure Depoları'nda bir CI/CD ardışık Yeni bir Azure DevOps kuruluşu oluşturabilir veya varolan bir kuruluş kullanabilirsiniz. DevOps Projects, seçtiğiniz Azure aboneliğinde Azure kaynakları da oluşturur.

1. [Azure portalında](https://portal.azure.com)oturum açın.

1. Sol bölmede **Kaynak oluştur**'u seçin.

1. Arama **kutusunda, DevOps Projeleri**yazın ve sonra **Oluştur'u**seçin.

    ![DevOps Projeleri panosu](_img/azure-devops-project-github/fullbrowser.png)

## <a name="select-a-sample-app-and-azure-service"></a>Örnek bir uygulama ve Azure hizmeti seçin

1. **Ruby** örnek uygulamasını seçin.

1. **Ruby on Rails** uygulama çerçevesini seçin. Bittiğinde, **İleri'yi**seçin.

1. **Linux üzerinde Web App** varsayılan dağıtım hedefidir.  
    İsteğe bağlı olarak, **Kapsayıcılar için Web Uygulaması'nı**seçebilirsiniz. Daha önce seçtiğiniz uygulama çerçevesi, burada kullanılabilen Azure hizmet dağıtım hedefitürünü belirler. 
    
1. Seçtiğiniz hedef hizmeti seçin ve sonra **İleri'yi**seçin.

## <a name="configure-azure-devops-and-an-azure-subscription"></a>Azure DevOps'leri ve Azure aboneliğini yapılandırma 

1. Yeni bir ücretsiz Azure DevOps kuruluşu oluşturun veya varolan bir kuruluş seçin. 

1. Azure DevOps projeniz için bir ad girin. 

1. Azure aboneliğinizi ve konumunuzu seçin, uygulamanız için bir ad girin ve ardından **Bitti'yi**seçin.  
    Birkaç dakika sonra DevOps Projects panosu Azure portalında görüntülenir. Azure DevOps kuruluşunuzdaki bir repo'da örnek bir uygulama ayarlanır, bir yapı yürütülür ve uygulamanız Azure'a dağıtılır. 
    
    Pano, kod repo'nuzda, CI/CD ardınızda ve Azure'daki uygulamanızda görünürlük sağlar. Sağdan, çalışan uygulamanızı görüntülemek için **Gözat'ı** seçin.

    ![Pano görünümü](_img/azure-devops-project-go/dashboardnopreview.png) 

## <a name="commit-your-code-changes-and-execute-the-cicd"></a>Kod değişikliklerinizi işleme ve CI/CD'yi çalıştırın

Azure DevOps Projeleri, Azure Ardışık Hatları'nda veya GitHub'da bir Git repo'su oluşturur. Repo'yu görüntülemek ve uygulamanızda kod değişiklikleri yapmak için aşağıdakileri yapın:

1. DevOps Projects panosunda, soldaki ana dalınızın bağlantısını seçin.  
    Bağlantı, yeni oluşturulan Git repo'ya bir görünüm açar.

1. Repo klon URL'sini görüntülemek için sağ üstteki **Klon'u** seçin.  
    Git reponuzu en sevdiğiniz IDE'de klonlayabilirsiniz. Sonraki birkaç adımda, kod değişiklikleri yapıp doğrudan ana dala işlemek için web tarayıcısını kullanabilirsiniz.

1. Solda, *app/views/pages/home.html.erb* dosyasına gidin ve ardından **Edit'i**seçin.

1. Dosyada değişiklik yapın. Örneğin, div etiketlerinden birinde bazı metni değiştirin.

1. **Commit'i**seçin ve değişikliklerinizi kaydedin.

1. Tarayıcınızda DevOps Projects panosuna gidin.  
    Bir yapı devam etmeli. Yaptığınız değişiklikler otomatik olarak oluşturulur ve bir CI/CD boru hattı aracılığıyla dağıtılır.

## <a name="examine-the-azure-pipelines-cicd-pipeline"></a>Azure Ardışık Hatları CI/CD ardışık hattını inceleyin

Azure DevOps Projeleri, Azure DevOps kuruluşunuzdaki tam bir CI/CD ardışık hattını otomatik olarak yapılandırır. İşlem hattını gerektiği şekilde keşfedin ve özelleştirin. Azure DevOps yapı ve sürüm boru hatları hakkında bilgi almak için aşağıdakileri yapın:

1. DevOps Projeleri panosuna gidin.

1. En üstte, **yapı ardışık lıklarını**seçin.  
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

1. **Oluştur ve Yayınla'yı**seçin ve ardından **Sürümler'i**seçin.  
    DevOps Projects, Azure'daki dağıtımları yönetmek için bir sürüm ardışık hattı oluşturur.

1. Sürüm ardışık ardışık alanınızın yanındaki elipsleri (...) seçin ve ardından **Edit'i**seçin.  
    Yayın işlem hattı, yayın işlemini tanımlayan bir *işlem hattı* içerir.

1. **Yapıtlar**’ın altında **Bırak**’ı seçin.  
    Daha önce incelediğiniz yapı ardışık hattı, yapı için kullanılan çıktıyı üretir. 

1. **Bırak** simgesinin sağında, **Sürekli dağıtım tetikleyicisini**seçin.  
    Bu sürüm ardışık yapısı, yeni bir yapı artifakı kullanılabilir olduğunda dağıtımı yürüten etkin bir CD tetikleyicisi vardır. İsteğe bağlı olarak, dağıtımlarınızın el ile yürütülmesini gerektirecek şekilde tetikleyiciyi devre dışı kullanabilirsiniz. 

1. Solda, **Görevler'i**seçin.  
    Görevler, dağıtım işleminizin gerçekleştirdiği etkinliklerdir. Bu örnekte, Azure Uygulama Hizmetine dağıtmak için bir görev oluşturuldu.

1. Sağda, sürümgeçmişi görüntülemek için **görünüm sürümlerini** seçin.

1. Bir sürümün yanındaki elipsleri (...) seçin ve ardından **Aç'ı**seçin.  
    Sürüm özeti, ilişkili iş öğeleri ve testler gibi çeşitli menüleri keşfedebilirsiniz.

1. **İşlemeler**'i seçin.  
    Bu görünüm, bu dağıtımla ilişkili kod işlemelerini gösterir. 

1. **Günlükleri**seçin.  
    Günlüklerde, dağıtım işlemiyle ilgili yararlı bilgiler bulunur. Bunları dağıtımlar sırasında ve sonrasında görüntüleyebilirsiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık ihtiyaç duyulmadığında, bu hızlı başlangıçta oluşturduğunuz Azure Uygulama Hizmeti örneğini ve ilgili kaynakları silebilirsiniz. Bunu yapmak için DevOps Projects panosundaki **Sil** işlevini kullanın.

## <a name="next-steps"></a>Sonraki adımlar

Ekibinizin gereksinimlerini karşılamak için yapı ve sürüm ardışık hatlarını değiştirme hakkında daha fazla bilgi edinmek için bkz:

> [!div class="nextstepaction"]
> [Çok aşamalı sürekli dağıtım (CD) ardışık sisteminizi tanımlayın](https://docs.microsoft.com/azure/devops/pipelines/release/define-multistage-release-process?view=vsts)
