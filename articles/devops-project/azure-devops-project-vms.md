---
title: 'Öğretici: Azure DevOps Projelerini kullanarak ASP.NET uygulamanızı Azure sanal makinelerine dağıtın'
description: DevOps Projects, Azure'a başlamanızı ve ASP.NET uygulamanızı birkaç hızlı adımda Azure sanal makinelerine dağıtmanızı kolaylaştırır.
ms.author: mlearned
manager: gwallace
ms.prod: devops
ms.technology: devops-cicd
ms.topic: tutorial
ms.date: 07/09/2018
author: mlearned
monikerRange: vsts
ms.openlocfilehash: b03fb27c46d4fd925b91b8927fdd50ff4912aaec
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "71969552"
---
# <a name="tutorial-deploy-your-aspnet-app-to-azure-virtual-machines-by-using-azure-devops-projects"></a>Öğretici: Azure DevOps Projelerini kullanarak ASP.NET uygulamanızı Azure sanal makinelerine dağıtın

Azure DevOps Projeleri, mevcut kodunuzu ve Git repo'nuzu getirebileceğiniz veya Azure'a sürekli tümleştirme (CI) ve sürekli teslimat (CD) ardışık bir çözüm hattı oluşturmak için örnek bir uygulama seçebileceğiniz basitleştirilmiş bir deneyim sunar. 

DevOps Projeleri de:
* Yeni bir Azure sanal makinesi (VM) gibi Azure kaynaklarını otomatik olarak oluşturur.
* Azure DevOps'te CI için bir yapı ardışık yapısı içeren bir sürüm ardışık alanı oluşturur ve yapılandırır.
* CD için bir sürüm ardışık hattı ayarlar. 
* İzleme için bir Azure Application Insights kaynağı oluşturur.

Bu öğreticide şunları yapacaksınız:

> [!div class="checklist"]
> * ASP.NET uygulamanızı dağıtmak için DevOps Projelerini kullanın
> * Azure DevOps'leri ve Azure aboneliğini yapılandırma 
> * CI işlem hattını inceleme
> * CD işlem hattını inceleme
> * Azure Deposu'nda değişiklik yapma ve bunları otomatik olarak Azure'a dağıtma
> * Application Insights izlemeyi yapılandırma
> * Kaynakları temizleme

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliği. [Visual Studio Dev Essentials](https://visualstudio.microsoft.com/dev-essentials/) aracılığıyla ücretsiz bir abonelik alabilirsiniz.

## <a name="use-devops-projects-to-deploy-your-aspnet-app"></a>ASP.NET uygulamanızı dağıtmak için DevOps Projelerini kullanın

DevOps Projects, Azure Ardışık Hatları'nda bir CI/CD ardışık alanı oluşturur. Yeni bir Azure DevOps kuruluşu oluşturabilir veya varolan bir kuruluş kullanabilirsiniz. DevOps Projects, seçtiğiniz Azure aboneliğinde sanal makineler gibi Azure kaynakları da oluşturur.

1. [Azure portalında](https://portal.azure.com)oturum açın.

1. Sol bölmede **Yeni'yi**seçin.

1. Arama **kutusunda, DevOps Projeleri**yazın ve sonra **Oluştur'u**seçin.

    ![DevOps Projeleri panosu](_img/azure-devops-project-github/fullbrowser.png)

1. **.NET'i**seçin ve sonra **İleri'yi**seçin.

1. **Bir uygulama Çerçevesi seçin,** **ASP.NET**seçin ve sonra **İleri'yi**seçin.  
    Önceki adımda seçtiğiniz uygulama çerçevesi, burada kullanılabilen Azure hizmet dağıtım hedefitürünü belirler. 

1. Sanal makineyi seçin ve sonra **İleri'yi**seçin.

## <a name="configure-azure-devops-and-an-azure-subscription"></a>Azure DevOps'leri ve Azure aboneliğini yapılandırma

1. Yeni bir Azure DevOps kuruluşu oluşturun veya varolan bir kuruluş seçin. 

1. Azure DevOps projeniz için bir ad girin. 

1. Azure abonelik hizmetlerinizi seçin.  
    İsteğe bağlı olarak, **Değiştir'i** seçebilir ve ardından Azure kaynaklarının konumu gibi daha fazla yapılandırma ayrıntısı girebilirsiniz.
 
1. Yeni Azure sanal makine kaynağınız için sanal makine adı, kullanıcı adı ve parola girin ve **ardından Bitti'yi**seçin.  
    Birkaç dakika sonra Azure sanal makinesi hazır olur. Azure DevOps kuruluşunuzdaki bir repo'da örnek ASP.NET uygulaması ayarlanır, bir yapı ve sürüm yürütülür ve uygulamanız yeni oluşturulan Azure VM'ye dağıtılır. 

    Tamamlandıktan sonra DevOps Projects panosu Azure portalında görüntülenir. Ayrıca, Azure portalındaki **tüm kaynaklardan** doğrudan panoya gidebilirsiniz. 

    Pano, Azure DevOps kod repo'nuzda, CI/CD ardışık sisteminizde ve Azure'daki çalışan uygulamanızda görünürlük sağlar.   

    ![Pano görünümü](_img/azure-devops-project-vms/dashboardnopreview.png)

DevOps Projects, repo'nuza kod değişiklikleri dağıtan bir CI yapı ve sürüm tetikleyicisini otomatik olarak yapılandırır. Azure DevOps’ta başka seçenekleri de yapılandırabilirsiniz. Çalışan uygulamanızı görüntülemek için **Gözat'ı**seçin.
    
## <a name="examine-the-ci-pipeline"></a>CI işlem hattını inceleme
 
DevOps Projects, Azure Ardışık Hatları'nda otomatik olarak bir CI/CD ardışık yapılandırıldı. İşlem hattını inceleyebilir ve özelleştirebilirsiniz. Yapı ardışık hattını tanımak için aşağıdakileri yapın:

1. DevOps Projeleri panosunun üst **kısmında, Yapı Boru Hatları'nı**seçin.  
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

## <a name="examine-the-cd-pipeline"></a>CD işlem hattını inceleme

DevOps Projects, Azure DevOps kuruluşunuzdan Azure aboneliğinize dağıtmak için gerekli adımları otomatik olarak oluşturur ve yapılandırır. Bu adımlar, Azure DevOps'lerin Azure aboneliğinize kimlik doğrulaması yapmak için bir Azure hizmet bağlantısını yapılandırmayı içerir. Otomasyon ayrıca, CD'yi Azure sanal makinesine sağlayan bir CD ardışık birimi de oluşturur. Azure DevOps CD ardışık alt bilgi hattı hakkında daha fazla bilgi edinmek için aşağıdakileri yapın:

1. **Oluştur ve Yayınla'yı**seçin ve ardından **Sürümler'i**seçin.  
    DevOps Projects, Azure'daki dağıtımları yönetmek için bir sürüm ardışık hattı oluşturur.

1. Sürüm ardışık ardışık alanınızın yanındaki elipsleri (...) seçin ve ardından **Edit'i**seçin.  
    Yayın işlem hattı, yayın işlemini tanımlayan bir *işlem hattı* içerir.

1. **Yapıtlar**’ın altında **Bırak**’ı seçin.  
    Önceki adımlarda incelediğiniz yapı ardışık hattı, yapı için kullanılan çıktıyı üretir. 

1. **Bırak** simgesinin yanında **Sürekli dağıtım tetikleyicisini**seçin.  
    Bu sürüm ardışık yapısı, yeni bir yapı artifakı kullanılabilir olduğunda bir dağıtım yürütür etkin bir CD tetikleyicivardır. İsteğe bağlı olarak, dağıtımlarınızın el ile yürütülmesini gerektirecek şekilde tetikleyiciyi devre dışı kullanabilirsiniz. 

1. Solda, **Görevler'i**seçin ve ardından ortamınızı seçin.  
    Görevler, dağıtım işleminizin yürüttüğü ve aşamalar halinde gruplandırılan etkinliklerdir. Bu sürüm ardışık iki aşamada olur:
    * İlk aşama, iki şey yapan bir Azure Kaynak Grubu Dağıtım görevi içerir:
      * Dağıtım için VM'yi yapılandırır
      * Yeni VM'yi Azure DevOps dağıtım grubuna ekler. Azure DevOps’taki VM dağıtım grubu, dağıtım hedefi makinelerinin mantıksal gruplarını yönetir.
    * İkinci aşamada, Bir IIS Web App Manage görevi VM'de bir IIS web sitesi oluşturur. Siteyi dağıtmak için ikinci bir IIS Web Uygulaması Dağıtma görevi oluşturulur.

1. Sağda, sürümgeçmişi görüntülemek için **görünüm sürümlerini** seçin.

1. Bir sürümün yanındaki elipsleri (...) seçin ve ardından **Aç'ı**seçin.  
    Sürüm özeti, ilişkili iş öğeleri ve testler gibi çeşitli menüleri keşfedebilirsiniz.

1. **İşlemeler**'i seçin.  
    Bu görünüm, bu dağıtımla ilişkili kod işlemelerini gösterir. Dağıtımlar arasındaki işleme farklılıklarını görmek için yayınları karşılaştırın.

1. **Günlükleri**seçin.  
    Günlüklerde, dağıtım işlemiyle ilgili yararlı bilgiler bulunur. Bunları dağıtımlar sırasında ve sonrasında görüntüleyebilirsiniz.

## <a name="commit-changes-to-azure-repos-and-automatically-deploy-them-to-azure"></a>Azure Deposu'nda değişiklik yapma ve bunları otomatik olarak Azure'a dağıtma 

Artık en son çalışmanızı web sitenize otomatik olarak dağıtan bir CI/CD işlemi kullanarak uygulamanızdaki bir ekiple işbirliği yapmaya hazırsınız. Git repo'sundaki her değişiklik Azure DevOps'te bir yapı yı başlatır ve CD ardışık bir yapı Azure'a dağıtım yürütür. Bu bölümdeki yordamı izleyin veya repo'nuzda değişiklik yapmak için başka bir teknik kullanın. Kod değişiklikleri CI/CD işlemini başlatır ve değişikliklerinizi Azure VM'deki IIS web sitesine otomatik olarak dağıtır.

1. Sol bölmede **Kod'u**seçin ve ardından repo'nuza gidin.

1. *Görünümler\Ev* dizinine gidin, *Index.cshtml* dosyasının yanındaki elipsleri (...) seçin ve ardından **Edit'i**seçin.

1. Div etiketlerinden birine metin eklemek gibi dosyada değişiklik yapın. 

1. Sağ üstte **Commit'i**seçin ve sonra değişikliğinizi zorlamak için yeniden **Commit'i** seçin.  
    Birkaç dakika sonra, Azure DevOps'te bir yapı başlar ve değişiklikleri dağıtmak için bir sürüm yürütülür. DevOps Projects panosunda veya Azure DevOps kuruluşunuzla tarayıcıda yapı durumunu izleyin.

1. Sürüm tamamlandıktan sonra, değişikliklerinizi doğrulamak için uygulamanızı yenileyin.

## <a name="configure-azure-application-insights-monitoring"></a>Application Insights izlemeyi yapılandırma

Azure Application Insights ile, uygulamanızın performansını ve kullanımını kolayca izleyebilirsiniz. DevOps Projects, uygulamanız için bir Application Insights kaynağını otomatik olarak yapılandırır. Gerekirse başka uyarılar ve izleme özellikleri de yapılandırabilirsiniz.

1. Azure portalında DevOps Projects panosuna gidin. 

1. Sağ alttaki uygulamanız için **Uygulama Öngörüleri** bağlantısını seçin.  
    **Uygulama Öngörüleri** bölmesi açılır. Bu görünüm uygulamanızın kullanım, performans ve kullanılabilirlik izleme bilgilerini içerir.

    ![Uygulama Öngörüleri bölmesi](_img/azure-devops-project-github/appinsights.png) 

1. **Zaman aralığını**seçin ve ardından **Son saat'i**seçin. Sonuçlara filtre uygulayacak şekilde **Güncelleştir'i**seçin.  
    Artık son 60 dakikadaki tüm etkinlikleri görüntüleyebilirsiniz. 
    
1. Zaman aralığından çıkmak için **x'i**seçin.

1. **Uyarıları**seçin ve ardından **metrik uyarı ekle'yi**seçin. 

1. Uyarı için bir ad girin.

1. **Metrik** açılır listede, çeşitli uyarı ölçümlerini inceleyin.  
    Varsayılan uyarı, **1 saniyeden uzun sunucu yanıt süresi** içindir. Çeşitli uyarıları kolayca yapılandırıp uygulamanızın izleme özelliklerini geliştirebilirsiniz.

1. **E-posta sahipleri, katkıda bulunanlar ve okuyucular la Bildirim** onay kutusunu seçin.  
    İsteğe bağlı olarak, bir Azure mantık uygulaması yürüterek bir uyarı görüntülendiğinde ek eylemler gerçekleştirebilirsiniz.

1. Uyarıyı oluşturmak için **Tamam'ı** seçin.  
    Birkaç dakika sonra, uyarı panoda etkin olarak görünür. 

1. **Uyarılar** alanından çıkın ve Uygulama **Öngörüleri** bölmesine geri dön.

1. **Kullanılabilirlik'i**seçin ve ardından **test ekle'yi**seçin. 

1. Bir test adı girin ve sonra **Oluştur'u**seçin.  
    Uygulamanızın kullanılabilirliğini doğrulamak için basit bir ping testi oluşturulur. Birkaç dakika sonra, test sonuçları sağlanır ve Application Insights panosu bir kullanılabilirlik durumu görüntüler.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Sınıyorsanız, kaynaklarınızı temizleyerek faturalandırma ücretleri tahakkuk etmekten kaçınabilirsiniz. Artık ihtiyaç duyulmadığında, bu eğitimde oluşturduğunuz Azure sanal makinesini ve ilgili kaynakları silebilirsiniz. Bunu yapmak için DevOps Project panosundaki **Sil** işlevini kullanın. 

> [!IMPORTANT]
> Aşağıdaki yordam kaynakları kalıcı olarak siler. *Silme* işlevi, DevOps Projects'te proje tarafından oluşturulan verileri hem Azure hem de Azure DevOps'lerde yok eder ve bu verileri geri alamazsınız. Bu yordamı yalnızca istemleri dikkatle okuduktan sonra kullanın.

1. Azure portalında DevOps Projects panosuna gidin.
1. Sağ üstte **Sil'i**seçin. 
1. İstemde, kaynakları *kalıcı olarak silmek* için **Evet'i** seçin.

Ekibinizin gereksinimlerine uygun olarak bu derleme ve yayın işlem hatlarını istediğiniz gibi değiştirebilirsiniz. Ayrıca bu CI/CD desenini diğer işlem hatlarınızda şablon olarak kullanabilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * ASP.NET uygulamanızı dağıtmak için DevOps Projelerini kullanın
> * Azure DevOps'leri ve Azure aboneliğini yapılandırma 
> * CI işlem hattını inceleme
> * CD işlem hattını inceleme
> * Azure Deposu'nda değişiklik yapma ve bunları otomatik olarak Azure'a dağıtma
> * Application Insights izlemeyi yapılandırma
> * Kaynakları temizleme

CI/CD ardışık boru hattı hakkında daha fazla bilgi için bkz:

> [!div class="nextstepaction"]
> [Çok aşamalı sürekli dağıtım (CD) ardışık sisteminizi tanımlayın](https://docs.microsoft.com/azure/devops/pipelines/release/define-multistage-release-process?view=vsts)
