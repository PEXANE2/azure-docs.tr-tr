---
title: 'Hızlı başlatma: Azure DevOps Projelerini kullanarak Go programlama dili için bir CI/CD ardışık'
description: DevOps Projects, Azure'a başlamayı kolaylaştırır. Birkaç hızlı adımda Azure hizmetinde Go programlama dili web uygulamasını başlatmanıza yardımcı olur.
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
ms.openlocfilehash: f7429a6de05a301b579354d722ad354b78ce6cbf
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "70899667"
---
# <a name="create-a-cicd-pipeline-for-go-by-using-azure-devops-projects"></a>Azure DevOps Projelerini kullanarak Go için BIR CI/CD ardışık

Azure DevOps Projeleri'ni kullanarak Go uygulamanız için sürekli tümleştirme (CI) ve sürekli teslimat (CD) yapılandırın. DevOps Projeleri, Azure DevOps yapı ve sürüm ardışık hattının ilk yapılandırmasını basitleştirir.

Azure aboneliğiniz yoksa, [Visual Studio Dev Essentials](https://visualstudio.microsoft.com/dev-essentials/) üzerinden ücretsiz edinebilirsiniz.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

DevOps Projects, Azure Ardışık Hatları'nda bir CI/CD ardışık alanı oluşturur. Yeni bir Azure DevOps kuruluşu oluşturabilir veya varolan bir kuruluş kullanabilirsiniz. DevOps Projects, seçtiğiniz Azure aboneliğinde Azure kaynakları da oluşturur.

1. [Azure portalında](https://portal.azure.com)oturum açın.

1. Sol bölmede **Kaynak oluştur**'u seçin.

1. Arama kutusunda **DevOps Project'i**yazın ve ardından **Oluştur'u**seçin.

    ![DevOps Projeleri panosu](_img/azure-devops-project-github/fullbrowser.png)

## <a name="select-a-sample-app-and-azure-service"></a>Örnek bir uygulama ve Azure hizmeti seçin

1. **Örnek** ver uygulamasını seç ve **sonra İleri'yi**seçin.  
    
1. **Basit Go uygulaması** varsayılan çerçevedir. **Sonraki'ni**seçin.  
    Daha önce seçtiğiniz uygulama çerçevesi, dağıtım için kullanılabilen Azure hizmet dağıtım hedeflerinin türünü belirler. 
    
1. Varsayılan Azure hizmetini bırakın ve **İleri'yi**seçin.

## <a name="configure-azure-devops-and-an-azure-subscription"></a>Azure DevOps'leri ve Azure aboneliğini yapılandırma 

1. Yeni bir ücretsiz Azure DevOps kuruluşu oluşturun veya varolan bir kuruluş seçin. 

1. Azure DevOps projeniz için bir ad girin. 

1. Azure aboneliğinizi ve konumunuzu seçin, uygulamanız için bir ad girin ve ardından **Bitti'yi**seçin.  
    Birkaç dakika sonra DevOps Projects panosu Azure portalında görüntülenir. Azure DevOps kuruluşunuzdaki bir repo'da örnek bir uygulama ayarlanır, bir yapı yürütülür ve uygulamanız Azure'a dağıtılır. 
    
    Pano, kod repo'nuzda, CI/CD ardınızda ve Azure'daki uygulamanızda görünürlük sağlar. Sağdan, çalışan uygulamanızı görüntülemek için **Gözat'ı** seçin.

    ![Pano görünümü](_img/azure-devops-project-go/dashboardnopreview.png) 

## <a name="commit-your-code-changes-and-execute-the-cicd"></a>Kod değişikliklerinizi işleme ve CI/CD'yi çalıştırın

DevOps Projects, Azure Repos veya GitHub'da bir Git repo'su oluşturur. Repo'yu görüntülemek ve uygulamanızda kod değişiklikleri yapmak için aşağıdakileri yapın:

1. DevOps Projelerinde, solda ana şubenizin bağlantısını seçin.  
    Bağlantı, yeni oluşturulan Git repo'ya bir görünüm açar.

1. Repo klon URL'sini görüntülemek için sağ üstteki **Klon'u** seçin.  
    Git reponuzu en sevdiğiniz IDE'de klonlayabilirsiniz. Sonraki birkaç adımda, kod değişiklikleri yapıp doğrudan ana dala işlemek için web tarayıcısını kullanabilirsiniz.

1. Solda, *views/index.html* dosyasına gidin ve sonra **Edit'i**seçin.

1. Dosyada değişiklik yapın. Örneğin, div etiketlerinden birinde bazı metni değiştirin.

1. **Commit'i**seçin ve değişikliklerinizi kaydedin.

1. Tarayıcınızda DevOps Projects panosuna gidin.  
    Bir yapı devam etmeli. Yaptığınız değişiklikler otomatik olarak oluşturulur ve bir CI/CD boru hattı aracılığıyla dağıtılır.

## <a name="examine-the-cicd-pipeline"></a>CI/CD boru hattını inceleyin

DevOps Projects, Azure Deposu'nda tam bir CI/CD ardışık hattını otomatik olarak yapılandırır. İşlem hattını gerektiği şekilde keşfedin ve özelleştirin. Azure DevOps yapı ve sürüm boru hatları hakkında bilgi almak için aşağıdakileri yapın:

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
