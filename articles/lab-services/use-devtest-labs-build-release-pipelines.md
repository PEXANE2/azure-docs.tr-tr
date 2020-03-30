---
title: Azure Pipelines derleme ve yayın işlem hatlarında DevTest Labs’i kullanma
description: Azure Ardışık Hatları oluşturma ve serbest bırakma da Azure DevTest Labs'ı nasıl kullanacağınızı öğrenin.
services: devtest-lab, lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/16/2020
ms.author: spelluru
ms.openlocfilehash: e16f3c5a0c0b2b86d6a893f541cefb275a8e7d07
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76169231"
---
# <a name="use-devtest-labs-in-azure-pipelines-build-and-release-pipelines"></a>Azure Pipelines derleme ve yayın işlem hatlarında DevTest Labs’i kullanma
Bu makalede, DevTest Labs'ın Azure Ardışık Hatları oluşturma ve serbest bırakma da nasıl kullanabileceği hakkında bilgi verilmektedir. 

## <a name="overall-flow"></a>Genel akış
Temel akış, aşağıdaki görevleri yapan bir **yapı ardışık**

1. Uygulama kodunu oluşturun.
1. DevTest Labs'da temel ortamı oluşturun.
1. Ortamı özel bilgilerle güncelleştirin.
1. Uygulamayı DevTest Labs ortamına dağıtın
1. Kodu test edin. 

Yapı başarıyla tamamlandıktan sonra, **sürüm ardışık alanı,** derlemeyi veya üretimi dağıtmak için yapı yapı larını kullanır. 

Gerekli önermelerden biri, test edilen ekosistemi yeniden oluşturmak için gereken tüm bilgilerin Azure kaynaklarının yapılandırması da dahil olmak üzere yapı yapılarında bulunmasıdır. Azure kaynakları kullanıldığında bir maliyete tabi olduğu için, şirketler bu kaynakların kullanımını denetlemek veya izlemek ister. Bazı durumlarda, kaynakları oluşturmak ve yapılandırmak için kullanılan Azure Kaynak Yöneticisi şablonları BT gibi başka bir departman tarafından yönetilebilir. Ve, bu şablonlar farklı bir depoda depolanabilir. Bu, bir yapının oluşturulacağı ve test edildiği ilginç bir duruma yol açar ve hem kod hem de yapılandırmanın üretimde sistemi düzgün bir şekilde yeniden oluşturmak için yapı yapıları içinde depolanmış olması gerekir. 

Yapı/test aşamasında DevTest Laboratuvarlarını kullanarak, sürüm aşamasında test etmek için kullanılan tam yapılandırmanın üretime dağıtılması için Yapı kaynaklarına Azure Kaynak Yöneticisi şablonları ve destekleyici dosyalar ekleyebilirsiniz. Uygun yapılandırmaya sahip **Azure DevTest Labs Ortamı Oluşturma** görevi, yapı yapıları içindeki Kaynak Yöneticisi şablonlarını kaydeder. Bu örnekte, Web uygulamasını Azure'da dağıtmak ve test etmek için [Öğretici: Azure Uygulama Hizmeti'nde bir .NET Core ve SQL Veritabanı web uygulaması oluşturun.](../app-service/app-service-web-tutorial-dotnetcore-sqldb.md)

![Genel akış](./media/use-devtest-labs-build-release-pipelines/overall-flow.png)

## <a name="set-up-azure-resources"></a>Azure kaynakları ayarlama
Önceden oluşturulması gereken birkaç öğe vardır:

- İki depo. Öğretici den kodu ve iki ek VMs ile bir Kaynak Yöneticisi şablonu ile ilki. İkincisi, temel Azure Kaynak Yöneticisi şablonu (varolan yapılandırma) içerir.
- Üretim kodu ve yapılandırma dağıtımı için bir Kaynak Grubu.
- Yapı denetimi için [yapılandırma deposuna bağlantı](devtest-lab-create-environment-from-arm.md) içeren bir laboratuvar kurulması gerekir. Kaynak Yöneticisi şablonu, DevTest Labs'ın şablonu tanımasına ve dağıtmasına olanak sağlamak için metadata.json ile azuredeploy.json olarak yapılandırma deposunda işaretlenmesi gerekir.

Yapı ardışık bir DevTest Labs ortamı oluşturur ve test için kod dağıtılır.

## <a name="set-up-a-build-pipeline"></a>Yapı ardışık hattı ayarlama
Azure Ardışık Durumlarda, Öğretici'deki kodu kullanarak bir yapı ardışık hattı [oluşturun: Azure Uygulama Hizmeti'nde bir .NET Core ve SQL Veritabanı web uygulaması oluşturun.](../app-service/app-service-web-tutorial-dotnetcore-sqldb.md) Kodu oluşturmak, sınamak ve yayımlamak için gerekli görevi dolduracak **olan ASP.NET Çekirdek** şablonuna kullanın.

![ASP.NET şablonu seçin](./media/use-devtest-labs-build-release-pipelines/select-asp-net.png)

DevTest Labs'da ortam oluşturmak ve ortama dağıtmak için üç ek görev eklemeniz gerekir.

![Üç görevli boru hattı](./media/use-devtest-labs-build-release-pipelines/pipeline-tasks.png)

### <a name="create-environment-task"></a>Ortam görevi oluşturma
Ortam oluşturma görevinde **(Azure DevTest Labs Create Environment** task) aşağıdaki değerleri seçmek için açılır listeleri kullanın:

- Azure aboneliği
- laboratuvarın adı
- deponun adı
- şablonun adı (ortamın depolandığı klasörü gösterir). 

Bilgileri el ile girmek yerine sayfadaki açılır listeleri kullanmanızı öneririz. Bilgileri el ile girerseniz, tam nitelikli Azure Kaynak Kimlikleri girin. Görev, kaynak Kimlikleri yerine uygun adları görüntüler. 

Ortam adı, DevTest Labs içinde gösterilen görüntüdeki addır. Her yapı için benzersiz bir ad olmalıdır. Örneğin: **TestEnv$(Build.BuildId)**. 

Kaynak Yöneticisi şablonuna bilgi aktarmak için parametreler dosyasını veya parametreleri belirtebilirsiniz. 

Ortam **şablonu çıktısı** seçeneğini temel alan çıktı değişkenleri oluştur'u seçin ve bir referans adı girin. Bu örnekte, başvuru adı için **BaseEnv** girin. Bir sonraki görevi yapılandırırken bu BaseEnv'i kullanırsınız. 

![Azure DevTest Labs Ortamı görev oluşturma](./media/use-devtest-labs-build-release-pipelines/create-environment.png)

### <a name="populate-environment-task"></a>Ortam görevini doldurma
İkinci görev **(Azure DevTest Labs Ortam doldurma** görevi) varolan DevTest Labs ortamını güncelleştirmektir. Ortam oluşturma görev çıktıları Bu görev için ortam adını yapılandırmak için kullanılan **BaseEnv.environmentResourceId.** Bu örnek için Kaynak Yöneticisi şablonu iki parametreye sahiptir : **adminUserName** ve **adminPassword**. 

![Azure DevTest Labs Ortamı görevini doldurma](./media/use-devtest-labs-build-release-pipelines/populate-environment.png)

## <a name="app-service-deploy-task"></a>Uygulama Hizmeti dağıtma görevi
Üçüncü görev, **Azure App Service Deploy** görevidir. Uygulama türü Web **App** olarak ayarlanır ve App Service adı **$(WebSitesi)** olarak ayarlanır.

![Uygulama Hizmeti Dağıtma görevi](./media/use-devtest-labs-build-release-pipelines/app-service-deploy.png)

## <a name="set-up-release-pipeline"></a>Sürüm ardışık hattını ayarlama
İki görevden bir sürüm ardışık hattı oluşturursunuz: **Azure Dağıtımı: Kaynak Grubu Oluşturma veya Güncelleştirme** ve Azure Uygulama Hizmetini **Dağıtma.** 

İlk görev için kaynak grubunun adını ve konumunu belirtin. Şablon konumu bağlantılı bir yapıdır. Kaynak Yöneticisi şablonu bağlantılı şablonlar içeriyorsa, özel bir kaynak grubu dağıtımının uygulanması gerekir. Şablon, yayımlanmış damla artifakındadır. Kaynak Yöneticisi şablonu için şablon parametrelerini geçersiz kılın. Kalan ayarları varsayılan değerlerle bırakabilirsiniz. 

İkinci görev **için Azure Uygulama Hizmeti dağıtın,** Azure aboneliğini belirtin, **Uygulama türü**için **Web Uygulamasını** ve Uygulama Hizmeti **adı**için **$(Web Sitesi)** seçeneğini belirleyin. Kalan ayarları varsayılan değerlerle bırakabilirsiniz. 

## <a name="test-run"></a>Test Çalıştır
Artık her iki ardışık hat lar da kurulduğuna göre, bir yapıyı el ile sıraya alır ve çalıştığını görür. Bir sonraki adım, yapı için uygun tetikleyiciyi ayarlamak ve yapıyı sürüm ardışık lığına bağlamaktır.

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelere bakın:

- [Azure DevTest Laboratuvarlarını Azure Ardışık Hatları sürekli tümleştirme ve teslim boru hattınıza entegre edin](devtest-lab-integrate-ci-cd-vsts.md)
- [Ortamları Azure Pipelines CI/CD ardışık hatlarınıza tümleştirin](integrate-environments-devops-pipeline.md)
