---
title: Azure DevTest Labs'da ortamları Azure Ardışık Hatlarına entegre edin
description: Azure DevTest Labs ortamlarını Azure DevOps sürekli tümleştirmenize (CI) ve sürekli teslim (CD) boru hatlarınıza nasıl entegre edebilirsiniz öğrenin.
services: devtest-lab,virtual-machines,lab-services
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
ms.openlocfilehash: 3d7e481879326ac30093bd116222bddc28640398
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76169417"
---
# <a name="integrate-environments-into-your-azure-devops-cicd-pipelines"></a>Ortamları Azure DevOps CI/CD ardışık hatlarınıza entegre edin
Sürekli tümleştirmenizi (CI)/ sürekli teslim (CD) yapı ve sürüm ardışık alanınızı Azure DevTest Labs Labs ile kolayca entegre etmek için Azure DevOps Hizmetleri'nde (eski adıyla Visual Studio Team Services) yüklenen Azure DevTest Labs Görevleri uzantısını kullanabilirsiniz. Bu uzantılar, belirli bir test görevi için bir [ortamı](devtest-lab-test-env.md) hızlı bir şekilde dağıtmayı ve test tamamlandığında silmeyi kolaylaştırır. 

Bu makalede, bir ortam oluşturmak ve dağıtmak, sonra tüm tam bir ardışık düzen içinde, ortamı silmek nasıl gösterir. Normalde bu görevlerin her birini kendi özel yapı-test dağıtma ardışık ardışık ardışık alanınızda ayrı ayrı gerçekleştirirsiniz. Bu makalede kullanılan uzantılar, [DTL VM oluşturma/silme görevlerine](devtest-lab-integrate-ci-cd-vsts.md)ek olarak:

- Ortam Oluşturma
- Ortamı Silme

## <a name="before-you-begin"></a>Başlamadan önce
CI/CD ardışık sisteminizi Azure DevTest Labs ile tümleştirmeden önce Visual Studio Marketplace'ten [Azure DevTest Labs Görevleri](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks) uzantısını yükleyin. 

## <a name="create-and-configure-the-lab-for-environments"></a>Ortamlar için laboratuvar oluşturma ve yapılandırma
Bu bölümde, Azure ortamının dağıtılanacağı bir laboratuvar nasıl oluşturulacağı ve yapılandırılabildiği açıklanmaktadır.

1. Zaten bir laboratuvar yoksa [bir laboratuvar oluşturun.](devtest-lab-create-lab.md) 
2. Bu makaledeki yönergeleri izleyerek laboratuarı yapılandırın ve bir ortam şablonu oluşturun: [Azure Kaynak Yöneticisi şablonları ile çoklu VM ortamları ve PaaS kaynakları oluşturun.](devtest-lab-create-environment-from-arm.md)
3. Bu örnekte, varolan bir [https://azure.microsoft.com/resources/templates/201-web-app-redis-cache-sql-database/](https://azure.microsoft.com/resources/templates/201-web-app-redis-cache-sql-database/)Azure Hızlı Başlatma Şablonu kullanın.
4. **201-web-app-redis-cache-sql-database** klasörünü adım 2'de yapılandırılan depodaki **ArmTemplate** klasörüne kopyalayın.

## <a name="create-a-release-definition"></a>Yayın tanımı oluşturma
Sürüm tanımını oluşturmak için aşağıdakileri yapın:

1.  Yapı & Yayın **hub'ının** **Sürümler** sekmesinde **artı işareti (+)** düğmesini seçin.
2.  Sürüm **tanım oluşturma** penceresinde **Boş şablonu** seçin ve sonra **İleri'yi**seçin.
3.  **Daha Sonra Seç'i**seçin ve ardından tek bir varsayılan ortama sahip ve bağlantılı yapı oluşturmadan yeni bir sürüm tanımı oluşturmak için **Oluştur'u** seçin.
4.  Kısayol menüsünü açmak için, yeni sürüm tanımında, ortam adının yanındaki **elipsleri (...)** seçin ve ardından **değişkenleri Yapıla'yı**seçin.
5.  **Yapıl- ortam** penceresinde, sürüm tanımı görevlerinde kullandığınız değişkenler için aşağıdaki değerleri girin:
1.  **AdministratorLogin**için SQL Administrator giriş adını girin.
2.  **administratorLoginPassword**için SQL Administrator girişi tarafından kullanılacak parolayı girin. Parolayı gizlemek ve güvenli hale getirmek için "asma kilit" simgesini kullanın.
3.  **DatabaseName**için SQL Veritabanı adını girin.
4.  Bu değişkenler örnek ortamlar için özeldir, farklı ortamlarda farklı değişkenler olabilir.

## <a name="create-an-environment"></a>Ortam oluşturma
Dağıtımın bir sonraki aşaması, geliştirme veya sınama amacıyla kullanılacak ortamı oluşturmaktır.

1. Sürüm tanımında **görev ekle'yi**seçin.
2. **Görevler** sekmesinde, Azure DevTest Labs Create Environment görev ekleyin. Görevi aşağıdaki gibi yapılandırın:
    1. **Azure RM Aboneliği** **için, Kullanılabilir Azure Hizmet Bağlantıları** listesinde bir bağlantı seçin veya Azure aboneliğinize daha kısıtlı izinbağlantısı oluşturun. Daha fazla bilgi için Azure [Kaynak Yöneticisi hizmeti bitiş noktasına](/azure/devops/pipelines/library/service-endpoints)bakın.
2. **Laboratuvar Adı**için, daha önce oluşturduğunuz örneğin adını seçin*.
3. **Depo Adı**için, Kaynak Yöneticisi şablonu (201) için itilmiş olan depoyu seçin*.
4. **Şablon Adı**için, kaynak kod deponuza kaydettiğiniz ortamın adını seçin*. 
5. **Laboratuvar Adı,** **Depo Adı**ve **Şablon Adı,** Azure kaynak adlarının samimi gösterimleridir. Dostu adı el ile girmek hatalara neden olur, bilgileri seçmek için açılır listeleri kullanın.
6. **Çevre Adı**için, laboratuvardaki ortam örneğini benzersiz olarak tanımlamak için bir ad girin.  Laboratuarda eşsiz olmalı.
7. **Parametre Dosyası** ve **Parametreler,** özel parametrelerin ortama geçirilmesine izin verir. Parametre değerlerini ayarlamak için her ikisi de kullanılabilir. Bu örnekte, Parametreler bölümü kullanılacaktır. Örneğin, ortamda tanımladığınız değişkenlerin adlarını kullanın:`-administratorLogin "$(administratorLogin)" -administratorLoginPassword "$(administratorLoginPassword)" -databaseName "$(databaseName)" -cacheSKUCapacity 1`
8. Ortam şablonu içindeki bilgiler şablonun çıktı bölümünden aktarılabilir. Diğer görevlerin verileri kullanabilmesi için **ortam şablonu çıktısını temel alan çıktı değişkenleri oluştur'u** denetleyin. `$(Reference name.Output Name)`izlenir desendir. Örneğin, Başvuru Adı DTL ve şablondaki çıkış adı konum olsaydı `$(DTL.location)`değişken .

## <a name="delete-the-environment"></a>Ortamı silme
Son aşama, Azure DevTest Labs örneğinde dağıttığınız Ortamı silmektir. Geliştirme görevlerini yürüttükten veya dağıtılan kaynaklarda gereksinim duyduğunuz testleri çalıştırdıktan sonra ortamı normalde silersiniz.

Sürüm tanımında, **görev ekle'yi**ve ardından **Dağıt** sekmesinde **Azure DevTest Labs Delete Environment** görevin ekleyin'i seçin. Aşağıdaki gibi yapılandırın:

1. VM'yi silmek için [Azure DevTest Labs Görevleri'ne](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks)bakın:
    1. **Azure RM Aboneliği** **için, Kullanılabilir Azure Hizmet Bağlantıları** listesinde bir bağlantı seçin veya Azure aboneliğinize daha kısıtlı izinbağlantısı oluşturun. Daha fazla bilgi için Azure [Kaynak Yöneticisi hizmeti bitiş noktasına](/azure/devops/pipelines/library/service-endpoints)bakın.
    2. **Laboratuvar Adı**için, ortamın bulunduğu laboratuarı seçin.
    3. **Çevre Adı**için, kaldırılacak ortamın adını girin.
2. Sürüm tanımı için bir ad girin ve sonra kaydedin.

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelere bakın: 
- [Kaynak Yöneticisi şablonları ile çoklu VM ortamları oluşturun.](devtest-lab-create-environment-from-arm.md)
- [DevTest Labs GitHub deposundan](https://github.com/Azure/azure-quickstart-templates)DevTest Labs otomasyonu için Quickstart Kaynak Yöneticisi şablonları.
- [VSTS Sorun Giderme sayfası](/azure/devops/pipelines/troubleshooting)

