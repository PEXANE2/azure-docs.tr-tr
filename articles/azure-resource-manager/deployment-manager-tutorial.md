---
title: Kaynak Yöneticisi şablonlarla Azure Dağıtım Yöneticisi kullanma | Microsoft Docs
description: Azure kaynaklarını dağıtmak için Azure Dağıtım Yöneticisi birlikte Kaynak Yöneticisi şablonlarını kullanın.
services: azure-resource-manager
documentationcenter: ''
author: mumian
ms.service: azure-resource-manager
ms.date: 05/23/2019
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: 97d9aa1ed9440011fdaab3aa8eb9d3942b5a8acf
ms.sourcegitcommit: aef6040b1321881a7eb21348b4fd5cd6a5a1e8d8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72170355"
---
# <a name="tutorial-use-azure-deployment-manager-with-resource-manager-templates-public-preview"></a>Öğretici: Azure Dağıtım Yöneticisi Kaynak Yöneticisi şablonlarıyla kullanma (Genel Önizleme)

[Azure dağıtım Yöneticisi](./deployment-manager-overview.md) kullanarak uygulamalarınızı birden çok bölgede dağıtma hakkında bilgi edinin. Daha hızlı bir yaklaşım tercih ediyorsanız, [Azure dağıtım Yöneticisi hızlı başlangıç](https://github.com/Azure-Samples/adm-quickstart) aboneliğinizde gerekli konfigürasyonları oluşturur ve bir uygulamayı birden çok bölgede dağıtmak için yapıtları özelleştirir. Hızlı başlangıç, bu öğreticide olduğu gibi aynı görevleri gerçekleştirir.

Dağıtım Yöneticisi kullanmak için iki şablon oluşturmanız gerekir:

* **Topoloji şablonu**: uygulamalarınızı oluşturan Azure kaynaklarını ve bunların nereye dağıtılacağını açıklar.
* **Bir piyasaya çıkma şablonu**: uygulamalarınızı dağıttığınızda yapmanız gereken adımları açıklar.

> [!IMPORTANT]
> Aboneliğiniz yeni Azure özelliklerini test etmek üzere işaretlenmişse, Azure Dağıtım Yöneticisi 'yi yalnızca Canary bölgelerine dağıtmak için kullanabilirsiniz. 

Bu öğretici aşağıdaki görevleri ele almaktadır:

> [!div class="checklist"]
> * Senaryoyu anlayın
> * Öğretici dosyalarını indirin
> * Yapıtları hazırlama
> * Kullanıcı tanımlı yönetilen kimlik oluşturma
> * Hizmet topolojisi şablonu oluşturma
> * Piyasaya çıkma şablonu oluşturma
> * Şablonları dağıtma
> * Dağıtımı doğrulama
> * Daha yeni sürümü dağıt
> * Kaynakları temizleme

Ek kaynaklar:

* [Azure Dağıtım Yöneticisi REST API başvurusu](https://docs.microsoft.com/rest/api/deploymentmanager/).
* [Öğretici: Azure dağıtım Yöneticisi 'da sistem durumu denetimi kullanın](./deployment-manager-tutorial-health-check.md).

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/) .

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Önkoşullar

Bu makaleyi tamamlayabilmeniz için şunlar gerekir:

* [Azure Resource Manager şablonları](./resource-group-overview.md)geliştirmeyle ilgili bir deneyim.
* Azure PowerShell. Daha fazla bilgi için bkz. [Azure PowerShell kullanmaya başlama](https://docs.microsoft.com/powershell/azure/get-started-azureps).
* Dağıtım Yöneticisi cmdlet 'leri. Bu ön sürüm cmdlet 'lerini yüklemek için PowerShellGet 'in en son sürümüne ihtiyacınız vardır. En son sürümü almak için bkz. [PowerShellGet 'ı yükleme](/powershell/gallery/installing-psget). PowerShellGet yükledikten sonra PowerShell pencerenizi kapatın. Yeni bir yükseltilmiş PowerShell penceresi açın ve aşağıdaki komutu kullanın:

    ```powershell
    Install-Module -Name Az.DeploymentManager
    ```

* [Microsoft Azure Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/). Azure Depolama Gezgini gerekli değildir, ancak bu işlemleri kolaylaştırır.

## <a name="understand-the-scenario"></a>Senaryoyu anlayın

Hizmet topolojisi şablonu, hizmetinizi oluşturan Azure kaynaklarını ve bunların nereye dağıtılacağını açıklar. Hizmet topolojisi tanımı aşağıdaki hiyerarşiye sahiptir:

* Hizmet topolojisi
  * Servislere
    * Hizmet birimleri

Aşağıdaki diyagramda Bu öğreticide kullanılan hizmet topolojisi gösterilmektedir:

![Azure Dağıtım Yöneticisi öğreticisi senaryo diyagramı](./media/deployment-manager-tutorial/azure-deployment-manager-tutorial-scenario-diagram.png)

Batı ABD ve Doğu ABD konumlarında ayrılan iki hizmet vardır.  Her hizmette iki hizmet birimi vardır. bir Web uygulaması ön ucu ve arka uç için bir depolama hesabı. Hizmet birimi tanımları, Web uygulamaları ve depolama hesapları oluşturmak için şablon ve parametre dosyalarına bağlantılar içerir.

## <a name="download-the-tutorial-files"></a>Öğretici dosyalarını indirin

1. Bu öğretici tarafından kullanılan [şablonları ve yapıtları](https://github.com/Azure/azure-docs-json-samples/raw/master/tutorial-adm/ADMTutorial.zip) indirin.
2. Dosyaları konum bilgisayarınıza ayıklayın.

Kök klasörü altında iki klasör vardır:

* **Admtemplates**: şunları içeren dağıtım Yöneticisi şablonlarını içerir:
  * CreateADMServiceTopology. JSON
  * CreateADMServiceTopology. Parameters. JSON
  * Createadmpiyasaya çıkma. JSON
  * Createadmpiyasaya çıkma. Parameters. JSON
* **Artifactstore**: hem şablon yapılarını hem de ikili yapıtları içerir. Bkz. [yapıtları hazırlama](#prepare-the-artifacts).

İki şablon kümesi olduğunu aklınızda edin.  Bir küme, hizmet topolojisini ve dağıtımı dağıtmak için kullanılan Dağıtım Yöneticisi şablonlardır; diğer küme, Web Hizmetleri ve depolama hesapları oluşturmak için hizmet birimlerinden çağırılır.

## <a name="prepare-the-artifacts"></a>Yapıtları hazırlama

İndirmenin ArtifactStore klasörü iki klasör içerir:

![Azure Dağıtım Yöneticisi öğreticisi yapıt kaynak diyagramı](./media/deployment-manager-tutorial/azure-deployment-manager-tutorial-artifact-source-diagram.png)

* **Şablonlar** klasörü: şablon yapılarını içerir. **1.0.0.0** ve **1.0.0.1** ikili yapıtların iki sürümünü temsil eder. Her sürümde, her hizmet için bir klasör vardır (hizmet Doğu ABD ve Service Batı ABD). Her hizmet bir depolama hesabı oluşturmak için bir dizi şablon ve parametre dosyası ve bir Web uygulaması oluşturmak için başka bir çiftin vardır. Web uygulaması şablonu, Web uygulaması dosyalarını içeren sıkıştırılmış bir paket çağırır. Sıkıştırılmış dosya, ikili dosyalar klasöründe depolanan bir ikili yapıdır.
* **İkililer** klasörü: ikili yapıtları içerir. **1.0.0.0** ve **1.0.0.1** ikili yapıtların iki sürümünü temsil eder. Her sürümde, Batı ABD konumunda Web uygulaması oluşturmak için bir ZIP dosyası ve diğer ZIP dosyası da Doğu ABD konumunda Web uygulaması oluşturmak için kullanılır.

İki sürüm (1.0.0.0 ve 1.0.0.1), [Düzeltme dağıtımı](#deploy-the-revision)içindir. Hem şablon yapıtları hem de ikili yapıtlar iki sürüme sahip olsa da, iki sürüm arasında yalnızca ikili yapılar farklıdır. Uygulamada, ikili yapıtlar, şablon yapılarıyla karşılaştırıldığında daha sık güncelleştirilir.

1. **\ArtifactStore\templates\1.0.0.0\ServiceWUS\CreateStorageAccount.JSON** 'i bir metin düzenleyicisinde açın. Depolama hesabı oluşturmaya yönelik temel bir şablondur.
2. **\ArtifactStore\templates\1.0.0.0\ServiceWUS\CreateWebApplication.JSON**'i açın.

    ![Azure Dağıtım Yöneticisi öğreticisi Web uygulaması şablonu oluşturma](./media/deployment-manager-tutorial/azure-deployment-manager-tutorial-create-web-application-packageuri.png)

    Şablon, Web uygulamasının dosyalarını içeren bir dağıtım paketini çağırır. Bu öğreticide, sıkıştırılmış paket yalnızca bir index. html dosyası içerir.
3. **\ArtifactStore\templates\1.0.0.0\ServiceWUS\CreateWebApplicationParameters.JSON**'i açın.

    ![Azure Dağıtım Yöneticisi öğreticisi Web uygulaması şablon parametreleri kapsayıcı kökü oluşturma](./media/deployment-manager-tutorial/azure-deployment-manager-tutorial-create-web-application-parameters-deploypackageuri.png)

    DeployPackageUri değeri dağıtım paketinin yoludur. Parametre bir **$containerRoot** değişkeni içeriyor. $ContainerRoot değeri, yapıt kaynağı SAS konumunu, yapıt kökünü ve deployPackageUri 'sini birleştirerek [dağıtım şablonunda](#create-the-rollout-template) sağlanır.
4. **\ArtifactStore\binaries\1.0.0.0\helloWorldWebAppWUS.zip\index.html**'i açın.

    ```html
    <html>
      <head>
        <title>Azure Deployment Manager tutorial</title>
      </head>
      <body>
        <p>Hello world from west U.S.!</p>
        <p>Version 1.0.0.0</p>
      </body>
    </html>
    ```

    HTML, konumu ve sürüm bilgilerini gösterir. 1\.0.0.1 klasöründeki ikili dosya "Version 1.0.0.1" öğesini gösterir. Hizmeti dağıttıktan sonra, bu sayfalara gidebilirsiniz.
5. Diğer yapıt dosyalarına göz atın. Senaryoyu daha iyi anlamanıza yardımcı olur.

Şablon yapıtları, hizmet topolojisi şablonu tarafından kullanılır ve ikili yapılar, piyasaya çıkma şablonu tarafından kullanılır. Topoloji şablonu ve dağıtım şablonu, dağıtımda kullanılan şablon ve ikili yapıtlar için Kaynak Yöneticisi işaret etmek üzere kullanılan bir kaynak olan yapıt kaynak Azure kaynağı tanımlar. Öğreticiyi basitleştirmek için, hem şablon yapılarını hem de ikili yapıtları depolamak için bir depolama hesabı kullanılır. Her iki yapıt kaynağı da aynı depolama hesabını işaret.

1. Bir Azure depolama hesabı oluşturun. Yönergeler için bkz. [hızlı başlangıç: Azure Portal kullanarak Blobları karşıya yükleme, indirme ve listeleme](../storage/blobs/storage-quickstart-blobs-portal.md).
2. Depolama hesabında bir blob kapsayıcısı oluşturun.
3. İki klasörü (ikili dosyalar ve şablonlar) ve iki klasörün içeriğini blob kapsayıcısına kopyalayın. [Microsoft Azure Depolama Gezgini](https://go.microsoft.com/fwlink/?LinkId=708343&clcid=0x409) sürükle ve bırak özelliğini destekler.
4. Aşağıdaki yönergeleri kullanarak kapsayıcının SAS konumunu alın:

    1. Azure Depolama Gezgini, blob kapsayıcısına gidin.
    2. Sol bölmeden blob kapsayıcısına sağ tıkladıktan sonra **paylaşılan erişim Imzasını al**' ı seçin.
    3. **Başlangıç saatini** ve **süre sonu saatini**yapılandırın.
    4. **Oluştur**’u seçin.
    5. URL 'nin bir kopyasını oluşturun. İki parametre dosyasındaki bir alanı, [topoloji parametreleri dosyasını](#topology-parameters-file) ve [dağıtım parametreleri dosyasını](#rollout-parameters-file)doldurmak için bu URL gereklidir.

## <a name="create-the-user-assigned-managed-identity"></a>Kullanıcı tarafından atanan yönetilen kimliği oluşturma

Öğreticide daha sonra bir dağıtım dağıtırsınız. Dağıtım eylemlerini gerçekleştirmek için Kullanıcı tarafından atanan yönetilen kimlik (örneğin, Web uygulamalarını ve depolama hesabını dağıtın) gereklidir. Bu kimliğe, hizmeti dağıttığınız Azure aboneliğine erişim verilmelidir ve yapıt dağıtımını gerçekleştirmek için yeterli izne sahip olmanız gerekir.

Kullanıcı tarafından atanan bir yönetilen kimlik oluşturmanız ve aboneliğiniz için erişim denetimini yapılandırmanız gerekir.

> [!IMPORTANT]
> Kullanıcı tarafından atanan yönetilen kimliğin, [dağıtım](#create-the-rollout-template)ile aynı konumda olması gerekir. Şu anda, dağıtım dahil Dağıtım Yöneticisi kaynakları yalnızca Orta ABD veya Doğu ABD 2 oluşturulabilir. Ancak, bu yalnızca Dağıtım Yöneticisi kaynaklar (hizmet topolojisi, hizmetler, hizmet birimleri, dağıtım ve adımlar gibi) için geçerlidir. Hedef kaynaklarınız, desteklenen herhangi bir Azure bölgesine dağıtılabilir. Bu öğreticide, örneğin Dağıtım Yöneticisi Kaynakları Orta ABD dağıtılır, ancak hizmetler Doğu ABD ve Batı ABD dağıtılır. Bu kısıtlama gelecekte yükseltilmemiş olacaktır.

1. [Azure portalı](https://portal.azure.com)’nda oturum açın.
2. [Kullanıcı tarafından atanan yönetilen kimlik](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md)oluşturma.
3. Portalda, sol taraftaki menüden **abonelikler** ' i seçin ve ardından aboneliğinizi seçin.
4. **Erişim denetimi (IAM)** öğesini seçin ve ardından **rol ataması Ekle**' yi seçin.
5. Aşağıdaki değerleri girin veya seçin:

    ![Azure Dağıtım Yöneticisi öğreticisi Kullanıcı tarafından atanan yönetilen kimlik erişimi denetimi](./media/deployment-manager-tutorial/azure-deployment-manager-tutorial-access-control.png)

    * **Rol**: yapıt dağıtımını (Web uygulamaları ve depolama hesapları) tamamlamaya yetecek kadar izin verin. Bu öğreticide **katkıda** bulunan öğesini seçin. Uygulamada, izinleri en az ile kısıtlamak istiyorsunuz.
    * **Erişim atanmış**: **Kullanıcı tarafından atanan yönetilen kimlik**' i seçin.
    * Öğreticide daha önce oluşturduğunuz Kullanıcı tarafından atanan yönetilen kimliği seçin.
6. **Kaydet**’i seçin.

## <a name="create-the-service-topology-template"></a>Hizmet topolojisi şablonu oluşturma

**\Admtemplates\createadmservicetopologyı.exe**' i açın.

### <a name="the-parameters"></a>Parametreler

Şablon aşağıdaki parametreleri içerir:

![Azure Dağıtım Yöneticisi öğretici topolojisi şablon parametreleri](./media/deployment-manager-tutorial/azure-deployment-manager-tutorial-topology-template-parameters.png)

* **NamePrefix**: Bu ön ek, dağıtım Yöneticisi kaynaklarının adlarını oluşturmak için kullanılır. Örneğin, "jtikan" önekini kullanarak hizmet topolojisi adı **jtikan**servicetopology olur.  Kaynak adları, bu şablonun değişkenler bölümünde tanımlanmıştır.
* **azureResourcelocation**: Öğreticiyi basitleştirmek için, aksi belirtilmediği takdirde tüm kaynaklar bu konumu paylaşır. Şu anda Azure Dağıtım Yöneticisi kaynakları yalnızca **Orta ABD** veya **Doğu ABD 2**oluşturulabilir.
* **Artifactsourcesaslocation**: hizmet birim şablonu ve parametre dosyalarının dağıtım Için depolandığı blob kapsayıcısının SAS URI 'si.  Bkz. [yapıtları hazırlama](#prepare-the-artifacts).
* **Templateartifactroot**: şablon ve parametrelerin depolandığı blob kapsayıcısından gelen yol yolu. Varsayılan değer **Şablonlar/1.0.0.0**değeridir. [Yapıtları hazırlama](#prepare-the-artifacts)bölümünde açıklanan klasör yapısını değiştirmek istemediğiniz müddetçe bu değeri değiştirmeyin. Göreli yollar Bu öğreticide kullanılır.  Tam yol, **Artifactsourcesaslocation**, **templateartifactroot**ve **Templateartifactsourcerelativepath** (ya da **parametersartıactsourcerelativepath**) ile birleştirerek oluşturulur.
* **Targetsubscriptionıd**: Dağıtım Yöneticisi kaynaklarının dağıtılacağı ve faturalandırılacağı abonelik kimliği. Bu öğreticide abonelik KIMLIĞINIZI kullanın.

### <a name="the-variables"></a>Değişkenler

Değişkenler bölümü, kaynakların adlarını, iki hizmet için Azure konumlarını tanımlar: **HIZMET WUS** ve **hizmet EUS**ve yapıt yolları:

![Azure Dağıtım Yöneticisi öğretici topolojisi şablon değişkenleri](./media/deployment-manager-tutorial/azure-deployment-manager-tutorial-topology-template-variables.png)

Yapıt yollarını, depolama hesabına yüklediğiniz klasör yapısıyla karşılaştırın. Yapıt yollarının göreli yollar olduğunu fark edin. Tam yol, **Artifactsourcesaslocation**, **templateartifactroot**ve **Templateartifactsourcerelativepath** (ya da **parametersartıactsourcerelativepath**) ile birleştirerek oluşturulur.

### <a name="the-resources"></a>Kaynaklar

Kök düzeyinde, tanımlı iki kaynak vardır: *yapıt kaynağı*ve *hizmet topolojisi*.

Yapıt kaynak tanımı:

![Azure Dağıtım Yöneticisi öğretici topolojisi şablon kaynakları yapıt kaynağı](./media/deployment-manager-tutorial/azure-deployment-manager-tutorial-topology-template-resources-artifact-source.png)

Aşağıdaki ekran görüntüsünde yalnızca hizmet topolojisi, hizmetler ve hizmet birimleri tanımlarının bazı bölümleri gösterilmektedir:

![Azure Dağıtım Yöneticisi öğretici topolojisi şablon kaynakları hizmet topolojisi](./media/deployment-manager-tutorial/azure-deployment-manager-tutorial-topology-template-resources-service-topology.png)

* **Artifactsourceıd** , yapıt kaynak kaynağını hizmet topolojisi kaynağıyla ilişkilendirmek için kullanılır.
* **Bağımlıdson**: tüm hizmet topolojisi kaynakları, yapıt kaynak kaynağına bağlıdır.
* **yapıtlar** , şablon yapıtlarına işaret noktasıdır.  Göreli yollar burada kullanılır. Tam yol, artifactSourceSASLocation (yapıt kaynağında tanımlanır), artifactRoot (yapıt kaynağında tanımlı) ve templateArtifactSourceRelativePath (ya da parametersArtifactSourceRelativePath) eklenerek oluşturulur.

### <a name="topology-parameters-file"></a>Topoloji parametreleri dosyası

Topoloji şablonuyla kullanılan bir parametreler dosyası oluşturursunuz.

1. Visual Studio Code veya herhangi bir metin düzenleyicisinde **\Admtemplates\createadmservicetopologyı.exe parametresini** açın.
2. Parametre değerlerini doldur:

    * **NamePrefix**: 4-5 karakter içeren bir dize girin. Bu ön ek, benzersiz Azure Kaynak adları oluşturmak için kullanılır.
    * **azureResourceLocation**: Azure konumları hakkında bilgi sahibi değilseniz bu öğreticide **merkezileştirme** 'yi kullanın.
    * **Artifactsourcesaslocation**: hizmet birimi şablonunun ve parametre dosyalarının dağıtım için depolandığı kök dizine (blob KAPSAYıCıSı) SAS URI 'sini girin.  Bkz. [yapıtları hazırlama](#prepare-the-artifacts).
    * **Templateartifactroot**: yapıtların klasör yapısını değiştirmediğiniz müddetçe, bu öğreticide **Şablonlar/1.0.0.0** kullanın.
    * **Targetscriptionıd**: Azure abonelik kimliğinizi girin.

> [!IMPORTANT]
> Topoloji şablonu ve dağıtım şablonu bazı ortak parametreleri paylaşır. Bu parametrelerin değerleri aynı olmalıdır. Bu parametreler şunlardır: **NamePrefix**, **AzureResourceLocation**ve **artifactsourcesaslocation** (her iki yapıt kaynağı da bu öğreticide aynı depolama hesabını paylaşır).

## <a name="create-the-rollout-template"></a>Piyasaya çıkma şablonu oluşturma

**\ADMTemplates\CreateADMRollout.JSON**'i açın.

### <a name="the-parameters"></a>Parametreler

Şablon aşağıdaki parametreleri içerir:

![Azure Dağıtım Yöneticisi öğreticisi dağıtım şablonu parametreleri](./media/deployment-manager-tutorial/azure-deployment-manager-tutorial-rollout-template-parameters.png)

* **NamePrefix**: Bu ön ek, dağıtım Yöneticisi kaynaklarının adlarını oluşturmak için kullanılır. Örneğin, "jtikan" önekini kullanarak, dağıtım adı **jtikan**piyasaya sürülme olur.  Adlar, şablonun değişkenler bölümünde tanımlanmıştır.
* **azureResourcelocation**: Öğreticiyi basitleştirmek için, aksi belirtilmediği takdirde tüm dağıtım Yöneticisi kaynakları bu konumu paylaşır. Şu anda Azure Dağıtım Yöneticisi kaynakları yalnızca **Orta ABD** veya **Doğu ABD 2**oluşturulabilir.
* **Artifactsourcesaslocation**: hizmet birim şablonu ve parametre dosyalarının dağıtım için depolandığı kök dizine (blob KAPSAYıCıSı) SAS URI 'si.  Bkz. [yapıtları hazırlama](#prepare-the-artifacts).
* **Binaryartifactroot**: varsayılan değer **ikili/1.0.0.0**değeridir. [Yapıtları hazırlama](#prepare-the-artifacts)bölümünde açıklanan klasör yapısını değiştirmek istemediğiniz müddetçe bu değeri değiştirmeyin. Göreli yollar Bu öğreticide kullanılır.  Tam yol, **Artifactsourcesaslocation**, **binaryartifactroot**ve createwebapplicationparameters. JSON Içinde belirtilen **Deploypackageuri** ile birleştirerek oluşturulur.  Bkz. [yapıtları hazırlama](#prepare-the-artifacts).
* **Managedıdentityıd**: dağıtım eylemlerini gerçekleştiren kullanıcı tarafından atanan yönetilen kimlik. Bkz. [Kullanıcı tarafından atanan yönetilen kimlik oluşturma](#create-the-user-assigned-managed-identity).

### <a name="the-variables"></a>Değişkenler

Değişkenler bölümü, kaynakların adlarını tanımlar. Hizmet topolojisi adının, hizmet adlarının ve hizmet birimi adlarının [topoloji şablonunda](#create-the-service-topology-template)tanımlanan adlarla eşleştiğinden emin olun.

![Azure Dağıtım Yöneticisi öğreticisi piyasaya çıkma şablonu değişkenleri](./media/deployment-manager-tutorial/azure-deployment-manager-tutorial-rollout-template-variables.png)

### <a name="the-resources"></a>Kaynaklar

Kök düzeyinde, tanımlı üç kaynak vardır: yapıt kaynağı, adım ve dağıtım.

Yapıt kaynak tanımı, topoloji şablonunda tanımlanan tanınki ile aynıdır.  Daha fazla bilgi için bkz. [hizmet topolojisi şablonu oluşturma](#create-the-service-topology-template) .

Aşağıdaki ekran görüntüsünde, bekleme adımı tanımı gösterilmektedir:

![Azure Dağıtım Yöneticisi öğreticisi dağıtım şablonu kaynakları bekleme adımı](./media/deployment-manager-tutorial/azure-deployment-manager-tutorial-rollout-template-resources-wait-step.png)

Süre, [ıso 8601 standardını](https://en.wikipedia.org/wiki/ISO_8601#Durations)kullanıyor. **PT1M** (büyük harfler gereklidir) 1 dakikalık bir bekleme örneğidir.

Aşağıdaki ekran görüntüsünde yalnızca dağıtım tanımının bazı bölümleri gösterilmektedir:

![Azure Dağıtım Yöneticisi öğreticisi dağıtım şablonu kaynakları dağıtımı](./media/deployment-manager-tutorial/azure-deployment-manager-tutorial-rollout-template-resources-rollout.png)

* **Bağımlıldson**: dağıtım kaynağı, yapıt kaynak kaynağına ve tanımlanan adımlara bağlıdır.
* **Artifactsourceıd**: yapıt kaynak kaynağını piyasaya çıkma kaynağıyla ilişkilendirmek için kullanılır.
* **Targetservicetopologyıd**: hizmet topolojisi kaynağını piyasaya çıkma kaynağıyla ilişkilendirmek için kullanılır.
* **Deploymenttargetıd**: hizmet topolojisi kaynağının hizmet BIRIMI kaynak kimliğidir.
* **predeploymentsteps** ve **postdeploymentsteps**: dağıtım adımlarını içerir. Şablonda bir bekleme adımı çağırılır.
* **Bağımlıdsonstepgroups**: adım grupları arasındaki bağımlılıkları yapılandırın.

### <a name="rollout-parameters-file"></a>Dağıtım parametreleri dosyası

Piyasaya çıkma şablonuyla kullanılan bir parametreler dosyası oluşturursunuz.

1. Visual Studio Code veya herhangi bir metin düzenleyicisinde **\ADMTemplates\CreateADMRollout.Parameters** açın.
2. Parametre değerlerini doldur:

    * **NamePrefix**: 4-5 karakter içeren bir dize girin. Bu ön ek, benzersiz Azure Kaynak adları oluşturmak için kullanılır.
    * **azureResourceLocation**: Şu anda Azure dağıtım Yöneticisi kaynakları yalnızca **Orta ABD** veya **Doğu ABD 2**oluşturulabilir.
    * **Artifactsourcesaslocation**: hizmet birimi şablonunun ve parametre dosyalarının dağıtım için depolandığı kök dizine (blob KAPSAYıCıSı) SAS URI 'sini girin.  Bkz. [yapıtları hazırlama](#prepare-the-artifacts).
    * **Binaryartifactroot**: yapıtların klasör yapısını değiştirmediğiniz müddetçe bu öğreticide **ikili/1.0.0.0** kullanın.
    * **Managedıdentityıd**: Kullanıcı tarafından atanan yönetilen kimliği girin. Bkz. [Kullanıcı tarafından atanan yönetilen kimlik oluşturma](#create-the-user-assigned-managed-identity). Sözdizimi şöyledir:

        ```
        "/subscriptions/<SubscriptionID>/resourcegroups/<ResourceGroupName>/providers/Microsoft.ManagedIdentity/userassignedidentities/<ManagedIdentityName>"
        ```

> [!IMPORTANT]
> Topoloji şablonu ve dağıtım şablonu bazı ortak parametreleri paylaşır. Bu parametrelerin değerleri aynı olmalıdır. Bu parametreler şunlardır: **NamePrefix**, **AzureResourceLocation**ve **artifactsourcesaslocation** (her iki yapıt kaynağı da bu öğreticide aynı depolama hesabını paylaşır).

## <a name="deploy-the-templates"></a>Şablonları dağıtma

Azure PowerShell, şablonları dağıtmak için kullanılabilir.

1. Hizmet topolojisini dağıtmak için betiği çalıştırın.

    ```azurepowershell
    $resourceGroupName = "<Enter a Resource Group Name>"
    $location = "Central US"
    $filePath = "<Enter the File Path to the Downloaded Tutorial Files>"

    # Create a resource group
    New-AzResourceGroup -Name $resourceGroupName -Location "$location"

    # Create the service topology
    New-AzResourceGroupDeployment `
        -ResourceGroupName $resourceGroupName `
        -TemplateFile "$filePath\ADMTemplates\CreateADMServiceTopology.json" `
        -TemplateParameterFile "$filePath\ADMTemplates\CreateADMServiceTopology.Parameters.json"
    ```

    > [!NOTE]
    > `New-AzResourceGroupDeployment` zaman uyumsuz bir çağrıdır. Başarı iletisi yalnızca dağıtımın başarıyla başladığını gösterir. Dağıtımı doğrulamak için, bu yordamın adım 2 ve adım 4 ' e bakın.

2. Hizmet topolojisini ve altı çizili kaynakların Azure portal kullanılarak başarıyla oluşturulduğunu doğrulayın:

    ![Azure Dağıtım Yöneticisi öğreticisi dağıtılan hizmet topolojisi kaynakları](./media/deployment-manager-tutorial/azure-deployment-manager-tutorial-deployed-topology-resources.png)

    Kaynakları görmek için **gizli türlerin gösterilmesi** seçilmelidir.

3. <a id="deploy-the-rollout-template"></a>Dağıtım şablonunu dağıtma:

    ```azurepowershell
    # Create the rollout
    New-AzResourceGroupDeployment `
        -ResourceGroupName $resourceGroupName `
        -TemplateFile "$filePath\ADMTemplates\CreateADMRollout.json" `
        -TemplateParameterFile "$filePath\ADMTemplates\CreateADMRollout.Parameters.json"
    ```

4. Aşağıdaki PowerShell betiğini kullanarak dağıtım ilerlemesini denetleyin:

    ```azurepowershell
    # Get the rollout status
    $rolloutname = "<Enter the Rollout Name>" # "adm0925Rollout" is the rollout name used in this tutorial
    Get-AzDeploymentManagerRollout `
        -ResourceGroupName $resourceGroupName `
        -Name $rolloutName `
        -Verbose
    ```

    Bu cmdlet 'i çalıştırmadan önce Dağıtım Yöneticisi PowerShell cmdlet 'lerinin yüklü olması gerekir. Önkoşulları inceleyin. Tüm çıktıyı görmek için-verbose anahtarı kullanılabilir.

    Aşağıdaki örnek, çalışma durumunu gösterir:

    ```
    VERBOSE:

    Status: Succeeded
    ArtifactSourceId: /subscriptions/<AzureSubscriptionID>/resourceGroups/adm0925rg/providers/Microsoft.DeploymentManager/artifactSources/adm0925ArtifactSourceRollout
    BuildVersion: 1.0.0.0

    Operation Info:
        Retry Attempt: 0
        Skip Succeeded: False
        Start Time: 03/05/2019 15:26:13
        End Time: 03/05/2019 15:31:26
        Total Duration: 00:05:12

    Service: adm0925ServiceEUS
        TargetLocation: EastUS
        TargetSubscriptionId: <AzureSubscriptionID>

        ServiceUnit: adm0925ServiceEUSStorage
            TargetResourceGroup: adm0925ServiceEUSrg

            Step: Deploy
                Status: Succeeded
                StepGroup: stepGroup3
                Operation Info:
                    DeploymentName: 2F535084871E43E7A7A4CE7B45BE06510adm0925ServiceEUSStorage
                    CorrelationId: 0b6f030d-7348-48ae-a578-bcd6bcafe78d
                    Start Time: 03/05/2019 15:26:32
                    End Time: 03/05/2019 15:27:41
                    Total Duration: 00:01:08
                Resource Operations:

                    Resource Operation 1:
                    Name: txq6iwnyq5xle
                    Type: Microsoft.Storage/storageAccounts
                    ProvisioningState: Succeeded
                    StatusCode: OK
                    OperationId: 64A6E6EFEF1F7755

    ...

    ResourceGroupName       : adm0925rg
    BuildVersion            : 1.0.0.0
    ArtifactSourceId        : /subscriptions/<SubscriptionID>/resourceGroups/adm0925rg/providers/Microsoft.DeploymentManager/artifactSources/adm0925ArtifactSourceRollout
    TargetServiceTopologyId : /subscriptions/<SubscriptionID>/resourceGroups/adm0925rg/providers/Microsoft.DeploymentManager/serviceTopologies/adm0925ServiceTopology
    Status                  : Running
    TotalRetryAttempts      : 0
    OperationInfo           : Microsoft.Azure.Commands.DeploymentManager.Models.PSRolloutOperationInfo
    Services                : {adm0925ServiceEUS, adm0925ServiceWUS}
    Name                    : adm0925Rollout
    Type                    : Microsoft.DeploymentManager/rollouts
    Location                : centralus
    Id                      : /subscriptions/<SubscriptionID>/resourcegroups/adm0925rg/providers/Microsoft.DeploymentManager/rollouts/adm0925Rollout
    Tags                    :
    ```

    Dağıtım başarıyla dağıtıldıktan sonra, her hizmet için bir tane olmak üzere iki kaynak grubu oluşturmuş olursunuz.

## <a name="verify-the-deployment"></a>Dağıtımı doğrulama

1. [Azure portalı](https://portal.azure.com)’nı açın.
2. Dağıtım dağıtımı tarafından oluşturulan yeni kaynak grupları altındaki Yeni oluştur Web uygulamalarına gidin.
3. Web uygulamasını bir Web tarayıcısında açın. İndex. html dosyasındaki konumu ve sürümü doğrulayın.

## <a name="deploy-the-revision"></a>Düzeltmeyi dağıtma

Web uygulaması için yeni bir sürümünüz (1.0.0.1) varsa. Web uygulamasını yeniden dağıtmak için aşağıdaki yordamı kullanabilirsiniz.

1. Createadmpiyasaya çıkma. Parameters. json dosyasını açın.
2. **Binaryartıactroot** **dosyalarını ikili/1.0.0.1**olarak güncelleştirin.
3. [Şablonları dağıtma](#deploy-the-rollout-template)bölümünde belirtildiği gibi dağıtımı yeniden dağıtın.
4. Dağıtımı [doğrulama](#verify-the-deployment)bölümünde belirtildiği gibi dağıtımı doğrulayın. Web sayfasında 1.0.0.1 sürümü gösterilmektedir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Azure kaynaklarına artık ihtiyaç duyulmadığında, kaynak grubunu silerek dağıttığınız kaynakları temizleyin.

1. Azure portal, sol menüden **kaynak grubu** ' nu seçin.
2. Bu öğreticide oluşturulan kaynak gruplarını daraltmak için **ada göre filtrele** alanını kullanın. 3-4 şu şekilde olacaktır:

    * **&lt;namePrefix > RG**: Dağıtım Yöneticisi kaynaklarını içerir.
    * **&lt;namePrefix > ServiceWUSrg**: ServiceWUS tarafından tanımlanan kaynakları içerir.
    * **&lt;namePrefix > ServiceEUSrg**: ServiceEUS tarafından tanımlanan kaynakları içerir.
    * Kullanıcı tanımlı yönetilen kimliğin kaynak grubu.
3. Kaynak grubu adını seçin.
4. Üstteki menüden **kaynak grubunu sil** ' i seçin.
5. Bu öğretici tarafından oluşturulan diğer kaynak gruplarını silmek için son iki adımı tekrarlayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide Azure Dağıtım Yöneticisi kullanmayı öğrendiniz. Azure Dağıtım Yöneticisi sistem durumu izlemeyi tümleştirmek için bkz. [öğretici: Azure 'da sistem durumu denetimi kullanma dağıtım Yöneticisi](./deployment-manager-tutorial-health-check.md).
