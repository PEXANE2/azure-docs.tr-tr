---
title: Azure DevTest Labs'da Hizmet Olarak Platform (PaaS) hizmetlerini kullanma
description: Azure DevTest Labs'da Hizmet Olarak Platform (Geçiş) hizmetlerini nasıl kullanacağınızı öğrenin.
services: devtest-lab,lab-services
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
ms.openlocfilehash: 88bbf921fedae4bcdba2b6386ce6e08105206cd2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76169200"
---
# <a name="use-platform-as-a-service-paas-services-in-azure-devtest-labs"></a>Azure DevTest Labs'da Hizmet Olarak Platform (PaaS) hizmetlerini kullanma
PaaS, DevTest Labs'da ortam özelliği ile desteklenir. DevTest Labs'daki ortamlar, git deposunda önceden yapılandırılmış Azure Kaynak Yöneticisi şablonları tarafından desteklenir. Ortamlar hem PaaS hem de IaaS kaynaklarını içerebilir. Sanal makineler, veritabanları, sanal ağlar ve Web uygulamaları gibi birlikte çalışacak şekilde özelleştirilmiş Azure kaynaklarını içerebilen karmaşık sistemler oluşturmanıza olanak tanır. Bu şablonlar, kaynak kodu denetimini kullanarak ortamların tutarlı dağıtımına ve gelişmiş yönetimine olanak tanır. 

Düzgün ayarlanmış bir sistem aşağıdaki senaryoları sağlar: 

- Geliştiricilerin bağımsız ve birden çok ortama sahip olması
- Farklı yapılandırmalar üzerinde eşzamanlı olarak test etme
- Şablon değişikliği olmadan evreleme ve üretim boru hatlarına entegrasyon
- Aynı laboratuvarda hem geliştirme makinelerinin hem de ortamların olması yönetim ve maliyet raporlama kolaylığını artırır.  

DevTest Labs kaynak sağlayıcısı laboratuvar kullanıcısı adına kaynaklar oluşturur, bu nedenle PaaS kaynaklarının kullanımını sağlamak için laboratuvar kullanıcısına ekstra izin verilmesi gerekmez. Aşağıdaki resimde, laboratuvarda bir ortam olarak bir Service Fabric kümesi gösterilmektedir.

![Bir ortam olarak Hizmet Kumaş kümesi](./media/create-environment-service-fabric-cluster/cluster-created.png)

Ortamayarlama hakkında daha fazla bilgi için azure [kaynak yöneticisi şablonlarıyla çoklu VM ortamları ve PaaS kaynakları oluştur](devtest-lab-create-environment-from-arm.md)'a bakın. DevTest Labs, harici bir GitHub kaynağına tek başınıza bağlanmak zorunda kalmadan ortam oluşturmak için kullanabileceğiniz Azure Kaynak Yöneticisi şablonlarının genel deposuna sahiptir. Ortak ortamlar hakkında daha fazla bilgi için Azure [DevTest Labs'da yapılandırılan ortak ortamları](devtest-lab-configure-use-public-environments.md)kullanın ve genel olarak kullanın.

Büyük kuruluşlarda, geliştirme ekipleri genellikle özelleştirilmiş/yalıtılmış test ortamları gibi ortamlar sağlar. Bir iş birimi veya bölüm içindeki tüm takımlar tarafından kullanılacak ortamlar olabilir. BT grubu, kuruluştaki tüm takımlar tarafından kullanılabilecek ortamlarını sağlamak isteyebilir.  

## <a name="customizations"></a>Özelleştirmeler

#### <a name="sandbox"></a>Korumalı alan 
Laboratuvar sahibi, kullanıcının rolünü **okuyucudan** kaynak grubu içindeki **katkıda bulunana** değiştirmek için laboratuvar ortamlarını özelleştirebilir. Bu özellik, laboratuvarın **Yapılandırması ve İlkeleri** altındaki **Laboratuvar Ayarları** sayfasında dır. Roldeki bu değişiklik, kullanıcının bu ortam açısından kaynak eklemesine veya kaldırımına olanak sağlıyor. Erişimi daha fazla kısıtlamak istiyorsanız, Azure ilkelerini kullanın. Bu işlevsellik, abonelik düzeyinde erişim olmadan kaynakları veya yapılandırmayı özelleştirmenize olanak tanır.

#### <a name="custom-tokens"></a>Özel belirteçler
Kaynak grubunun dışında olan ve şablonun erişebileceği ortamlara özgü bazı özel laboratuvar bilgileri vardır. Bunlardan bazıları şunlardır: 

- Laboratuvar ağı tanımlama
- Konum
- Kaynak Yöneticisi şablonları dosyalarının depolandığı depolama hesabı. 
 
#### <a name="lab-virtual-network"></a>Laboratuvar sanal ağı
[Laboratuvarın sanal ağ makalesine Bağlanma ortamları,](connect-environment-lab-virtual-network.md) `$(LabSubnetId)` kaynak yöneticisi şablonunuzu belirteç kullanmak üzere nasıl değiştirilebildiğini açıklar. Bir ortam oluşturulduğunda, `$(LabSubnetId)` belirteç, **sanal makine oluşturma seçeneğinde kullanımın** **doğru**olarak ayarlandığı ilk alt ağ işaretiyle değiştirilir. Ortamımızın daha önce oluşturulmuş ağları kullanmasını sağlar. Hazırlama ve üretim gibi test ortamlarında aynı Kaynak Yöneticisi şablonlarını `$(LabSubnetId)` kullanmak istiyorsanız, Kaynak Yöneticisi şablon parametresinde varsayılan değer olarak kullanın. 

#### <a name="environment-storage-account"></a>Çevre Depolama Hesabı
DevTest Labs iç [içe kaynak yöneticisi şablonlarının](../azure-resource-manager/templates/linked-templates.md)kullanımını destekler. [ Test[ortamları için iç içe geçen Azure Kaynak Yöneticisi şablonlarını dağıt](deploy-nested-template-environments.md) makalesinde, ana şablonun iç içe olduğu klasörde veya iç içe klasöründe Bir Kaynak Yöneticisi şablonuna URI oluşturmak için nasıl kullanılacağı `_artifactsLocation` ve `_artifactsLocationSasToken` belirteçleri açıklanır. Bu iki belirteç hakkında daha fazla bilgi için [Azure Kaynak Yöneticisi – En İyi Uygulamalar Kılavuzu'nun](https://github.com/Azure/azure-quickstart-templates/blob/master/1-CONTRIBUTION-GUIDE/best-practices.md)Dağıtım **yapıları** bölümüne bakın.

## <a name="user-experience"></a>Kullanıcı Deneyimi

## <a name="developer"></a>Geliştirici
Geliştiriciler belirli bir ortam oluşturmak için bir VM oluşturmak için aynı iş akışını kullanır. Ortam ve makine görüntüsünü seçerler ve şablonun gerektirdiği gerekli bilgileri girerler. Bir ortama sahip her geliştirici, değişikliklerin dağıtılmasına ve geliştirilmiş iç döngü hata ayıklamasına olanak tanır. Ortam, en son şablon kullanılarak her zaman oluşturulabilir.  Bu özellik, sistemin el ile oluşturulmasından veya hata sınamasından kurtarma dan kesinti süresini azaltmaya yardımcı olmak için ortamların yok edilmesine ve yeniden oluşturulmasına olanak tanır.  

### <a name="testing"></a>Sınama
DevTest Labs ortamları, belirli kod ve yapılandırmaların eşit olarak bağımsız olarak sınanmesine olanak tanır. Yaygın uygulama, ortamı tek tek çekme isteğinin koduyla ayarlamak ve herhangi bir otomatik sınama başlatmaktır. Otomatik sınama tamamlandıktan sonra, herhangi bir el ile sınama belirli ortama karşı yürütülebilir. Genellikle bu işlem CI/CD ardışık hattının bir parçası olarak yapılır. 

## <a name="management-experience"></a>Yönetim Deneyimi

### <a name="cost-tracking"></a>Maliyet Takibi
Maliyet izleme özelliği, genel maliyet eğiliminin bir parçası olarak farklı ortamlardaki Azure kaynaklarını içerir. Kaynakların maliyeti, ortam içindeki farklı kaynakları ortaya çıkarmaz, ancak ortamı tek bir maliyet olarak görüntüler.

### <a name="security"></a>Güvenlik
DevTest Labs ile düzgün yapılandırılmış bir Azure aboneliği, [Azure kaynaklarına erişimi yalnızca laboratuvar aracılığıyla sınırlandırabilir.](devtest-lab-add-devtest-user.md) Ortamlarda, laboratuvar sahibi kullanıcıların diğer Azure kaynaklarına erişmesine izin vermeden onaylanan yapılandırmalarla PaaS kaynaklarına erişmesine izin verebilir. Laboratuvar kullanıcılarının ortamları özelleştirdiği senaryoda, laboratuvar sahibi katılımcıerişimine izin verebilir. Katılımcı erişimi, laboratuvar kullanıcısının Azure kaynağını yalnızca yönetilen kaynak grubuna eklemesine veya kaldırmasına olanak tanır. Kullanıcı katılımcısının aboneliğe erişmesine izin vermekle karşı ları daha kolay izleme ve yönetim sağlar.

### <a name="automation"></a>Automation
Otomasyon, büyük ölçekli ve etkili bir ekosistem için önemli bir bileşendir. Abonelikler ve laboratuvarlar arasında birden fazla ortamı yönetmek veya izlemek için otomasyon gereklidir.

### <a name="cicd-pipeline"></a>CI/CD Boru Hattı
DevTest Labs'daki PaaS hizmetleri, erişimin laboratuvar tarafından kontrol edildiği odaklanmış dağıtımlara sahip olarak CI/CD ardışık hattının iyileştirilmesine yardımcı olabilir.

## <a name="next-steps"></a>Sonraki adımlar
Ortamlar hakkında ayrıntılar için aşağıdaki makalelere bakın: 

- 
- [Azure Resource Manager şablonları ile çoklu VM ortamları ve PaaS kaynakları oluşturma](devtest-lab-create-environment-from-arm.md)
- [Azure DevTest Labs'da ortak ortamları yapılandırma ve kullanma](devtest-lab-configure-use-public-environments.md)
- [Azure DevTest Labs'da bağımsız Hizmet Kumaşı kümesiyle bir ortam oluşturun](create-environment-service-fabric-cluster.md)
- [Azure DevTest Labs'da bir ortamı laboratuvarınızın sanal ağına bağlayın](connect-environment-lab-virtual-network.md)
- [Ortamları Azure DevOps CI/CD ardışık hatlarınıza entegre edin](integrate-environments-devops-pipeline.md)
 





