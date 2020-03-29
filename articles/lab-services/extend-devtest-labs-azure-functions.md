---
title: Azure İşlevlerini Kullanarak Azure DevTest Laboratuvarlarını Genişletme | Microsoft Dokümanlar
description: Azure İşlevlerini kullanarak Azure DevTest Laboratuvarlarını nasıl genişleteceklerini öğrenin.
services: devtest-lab,lab-services
documentationcenter: na
author: spelluru
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/22/2019
ms.author: spelluru
ms.openlocfilehash: dd1fc4c1076d89c12b25837db9fa6a0ac3e1f3a5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "70014366"
---
# <a name="use-azure-functions-to-extend-devtest-labs"></a>DevTest Labs'i genişletmek için Azure İşlevleri'ni kullanma
DevTest Labs tarafından zaten desteklenen senaryoların ötesindeki ek senaryoları desteklemek için Azure İşlevlerini kullanabilirsiniz. Azure İşlevleri, hizmetin yerleşik işlevselliğini işletmenize özel gereksinimlerinizi karşılamak üzere genişletmek için kullanılabilir. Aşağıdaki liste, olası senaryolardan bazılarını sağlar. Bu makalede, bu örnek senaryolardan birini nasıl uygulayacağınızı gösterir.

- Laboratuvarda sanal makinelerin (VM'ler) üst düzey bir özetini sağlayın
- [Uzak masaüstü ağ geçidi kullanmak için bir laboratuvar yapılandırma](configure-lab-remote-desktop-gateway.md)
- İç destek sayfasında uyumluluk raporlaması
- Kullanıcıların abonelikte daha fazla izin gerektiren işlemleri tamamlamasını sağlama
- [DevTest Labs olaylarını temel alan iş akışlarının başlatılması](https://github.com/RogerBestMsft/DTL-SecureArtifactData)

## <a name="overview"></a>Genel Bakış
[Azure İşlevler,](../azure-functions/functions-overview.md) Azure'da sunucusuz bir bilgi işlem platformudur. Azure Fonksiyonlarını DevTest Labs ile bir çözümde kullanmak, mevcut özellikleri kendi özel kodumuzla genişletmemize olanak tanır. Azure İşlevler hakkında daha fazla bilgi için [Azure İşlevleri belgelerine](../azure-functions/functions-overview.md)bakın. Azure Fonksiyonlarının GeliştirmeTest Labs'da gereksinimlerinizi karşılamaya veya senaryoları tamamlamaya nasıl yardımcı olabileceğini göstermek için, bu makalede Laboratuvar'da üst düzey bir VM özeti aşağıdaki gibi bir örnek bulunur:

**Örnek gereksinim/senaryo**: Kullanıcılar işletim sistemi, sahibi ve uygulanan yapılar da dahil olmak üzere bir laboratuvarda tüm VM'ler hakkındaki ayrıntıları görebilir.  Ayrıca, Windows **Güncelleştirmeleri Uygula** yapı tonu son zamanlarda uygulanmadıysa, bunu uygulamanın kolay bir yolu vardır.

Senaryoyu tamamlamak için, aşağıdaki diyagramda açıklandığı gibi iki işlev kullanırsınız:  

![Genel akış](./media/extend-devtest-labs-azure-functions/flow.png)

Bu örnek işlevlerin kaynak kodu [DevTest Labs GitHub deposunda](https://github.com/Azure/azure-devtestlab/tree/master/samples/DevTestLabs/AzureFunctions) bulunur (hem C# hem de PowerShell uygulamaları mevcuttur).

- **UpdateInternalSupportPage**: Bu işlev DevTest Labs sorgular ve sanal makineler hakkında ayrıntıları ile doğrudan iç destek sayfasını günceller.
- **ApplyWindowsUpdateArtifact**: Bir laboratuardaki bir VM için bu işlev **Windows update** artifakı uygular.

## <a name="how-it-works"></a>Nasıl çalışır?
Kullanıcılar DevTest Labs'da **İç destek** sayfasını seçtiklerinde, VM'ler, laboratuvar sahipleri ve destek kişileri hakkında bilgi içeren önceden doldurulmuş bir sayfaya sahiptir.  

**Yenilemek için burada Seç** düğmesini seçtiğinizde, sayfa ilk Azure işlevini çağırır: **UpdateInternalSupportPage**. İşlev, Bilgi için DevTest Labs'ı sorgular ve ardından **Dahili destek** sayfasını yeni bilgilerle yeniden yazar.

Windows Update yapılarının son zamanlarda uygulanmadığı tüm VM'ler için, windows güncelleştirmelerini VM'ye uygulamak için bir düğme olacak. VM için ***Çalıştır Windows güncelleştirme** düğmesini seçtiğinizde, sayfa ikinci Azure İşlevi çağırır: **WindowsUpdateArtifact uygula**. Bu işlev, sanal makinenin çalışıp çalışmadığını denetler ve varsa doğrudan [Windows Update](https://github.com/Azure/azure-devtestlab/tree/master/Artifacts/windows-install-windows-updates) artifakı uygular.

## <a name="step-by-step-walkthrough"></a>Adım adım geçiş
Bu bölümde, **İç destek** sayfasını güncelleştirmek için gereken Azure Kaynakları'nı ayarlamak için adım adım yönergeler sağlanmaktadır. Bu izlik, DevTest Laboratuvarlarını genişletmeye bir örnek sağlar. Bu deseni diğer senaryolar için kullanabilirsiniz.

### <a name="step-1-create-a-service-principal"></a>Adım 1: Hizmet ilkesi oluşturma 
İlk adım, laboratuarı içeren abonelik izni ne sahip bir hizmet müdürü elde etmektir. Hizmet sorumlusu parola tabanlı kimlik doğrulamasını kullanmalıdır. [Azure CLI,](/cli/azure/create-an-azure-service-principal-azure-cli?view=azure-cli-latest) [Azure PowerShell](/powershell/azure/create-azure-service-principal-azureps?view=azps-2.5.0)veya Azure [portalı](../active-directory/develop/howto-create-service-principal-portal.md)ile yapılabilir. Zaten kullanmak için bir hizmet sorumlusu varsa, bu adımı atlayabilirsiniz.

Hizmet sorumlusunun **başvuru kimliğini,** **anahtarını**ve **kiracı kimliğini** not edin. Bu izlenecek yol da daha sonra onlara ihtiyacınız olacak. 

### <a name="step-2-download-the-sample-and-open-in-visual-studio-2019"></a>Adım 2: Örneği indirin ve Visual Studio 2019'da açın
[C# Azure İşlevler örneğinin](https://github.com/Azure/azure-devtestlab/tree/master/samples/DevTestLabs/AzureFunctions/CSharp) bir kopyasını yerel olarak indirin (depoyu klonlayarak veya depoyu [buradan](https://github.com/Azure/azure-devtestlab/archive/master.zip)indirerek).  

1. Visual Studio 2019 ile örnek çözümü açın.  
1. Zaten yüklü değilseniz Visual Studio için **Azure geliştirme** iş yükünü yükleyin. **Araçlar** -> **Al Araçları ve Özellikleri** menü öğesi) üzerinden yüklenebilir.

    ![Azure geliştirme iş yükü](./media/extend-devtest-labs-azure-functions/azure-development-workload-vs.png)
1. Çözümü derleyin. **Oluştur'u** ve ardından **Çözüm Oluştur** menü öğeyi seçin.

### <a name="step-3-deploy-the-sample-to-azure"></a>Adım 3: Örneği Azure'a dağıtma
Visual Studio'da, **Çözüm Gezgini** penceresinde **Azure İşlevler** projesine sağ tıklayın ve ardından **Yayımla'yı**seçin. Yayımlamayı tamamlamak için sihirbazı yeni veya varolan bir Azure İşlevi Uygulaması'na uygulayın. Visual Studio'yı kullanarak Azure işlevlerini geliştirme ve dağıtma hakkında ayrıntılı bilgi için Visual [Studio'yı kullanarak Azure İşlevlerini Geliştir'e](../azure-functions/functions-develop-vs.md)bakın.

![İletişim kutusu yayımlama](./media/extend-devtest-labs-azure-functions/publish-dialog.png)


### <a name="step-4--gather-application-settings"></a>Adım 4: Uygulama ayarlarını toplama
Işlevler yayımlandıktan sonra, Azure portalından bu işlevler için URL almanız gerekir. 

1. [Azure portalına](https://portal.azure.com) gidin. 
1. İşlev uygulamasını bulun.
1. **İşlev Uygulamaları** sayfasında işlevi seçin. 
1. Aşağıdaki resimde gösterildiği gibi **işlev URL'sini al'ı** seçin. 

    ![Azure işlevleri URL'leri](./media/extend-devtest-labs-azure-functions/function-url.png)
4. URL'yi kopyalayın ve kaydedin. Diğer Azure işlevi için bu adımları yineleyin. 

Ayrıca, uygulama kimliği, anahtar ve kiracı kimliği gibi hizmet ilkesi hakkında ek bilgilere de ihtiyacınız olacaktır.


### <a name="step-5--update-application-settings"></a>Adım 5: Uygulama Ayarlarını Güncelleştir
Visual Studio'da, Azure İşlevi'ni yayımladıktan **sonra, Eylemler**altında **Azure Uygulama Hizmet Ayarlarını Edit'i** seçin. Aşağıdaki uygulama ayarlarını (uzaktan) güncelleştirin:

- AzureFunctionUrl_ApplyUpdates
- AzureFunctionUrl_UpdateSupportPage
- WindowsUpdateAllowedDays (varsayılan 7)
- ServicePrincipal_AppId
- ServicePrincipal_Key
- ServicePrincipal_Tenant

    ![Uygulama ayarları](./media/extend-devtest-labs-azure-functions/application-settings.png)

### <a name="step-6-test-the-azure-function"></a>Adım 6: Azure işlevini test edin
Bu iznin son adımı Azure işlevini sınamaktır.  

1. Adım 3'te oluşturulan işlev uygulamasındaki **UpdateInternalSupportPage** işlevine gidin. 
1. Sayfanın sağ tarafında **Test'i** seçin. 
1. Rota özelliklerini girin (LABNAME, RESOURCEGROUPNAME ve SUBSCRIPTIONID).
1. İşlevi yürütmek için **Çalıştır'ı** seçin.  

    Bu işlev, belirtilen laboratuvarın iç destek sayfasını güncelleştirecektir. Ayrıca, kullanıcıların bir sonraki sefere doğrudan işlevi aramaları için bir düğme içerir

    ![Test fonksiyonu](./media/extend-devtest-labs-azure-functions/test-function.png)

## <a name="next-steps"></a>Sonraki adımlar
Azure İşletmeleri, DevTest Labs'ın işlevselliğini zaten yerleşik olanın ötesine genişletmeye ve müşterilerin ekipleri için benzersiz gereksinimlerini karşılamalarına yardımcı olabilir. Bu desen genişletilebilir & daha da kapsayacak şekilde genişletilebilir.  DevTest Labs hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın: 

- [Devtest Labs Kurumsal Referans Mimarisi](devtest-lab-reference-architecture.md)
- [Sık Sorulan Sorular](devtest-lab-faq.md)
- [DevTest Laboratuvarlarını Ölçekleme](devtest-lab-guidance-scale.md)
- [PowerShell ile DevTest Laboratuvarlarını Otomatikleştirmek](https://github.com/Azure/azure-devtestlab/tree/master/samples/DevTestLabs/Modules/Library/Tests)








