---
title: Azure DevTest Labs ve DevOps tümleştirmesi | Microsoft Docs
description: Bir kurumsal ortamdaki sürekli tümleştirme (CI)/sürekli teslim (CD) işlem hatları içinde Azure DevTest Labs laboratuvarlarını nasıl kullanacağınızı öğrenin.
ms.topic: article
ms.date: 06/26/2020
ms.openlocfilehash: 8a5d35a541e079b7d39cae2ec43da608274533f5
ms.sourcegitcommit: 1d9f7368fa3dadedcc133e175e5a4ede003a8413
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2020
ms.locfileid: "85481077"
---
# <a name="integration-of-azure-devtest-labs-and-azure-devops"></a>Azure DevTest Labs ve Azure DevOps tümleştirmesi
DevOps, bir sistem için yazılım geliştirmeyi (dev) işlemler (OPS) ile tümleştiren yazılım geliştirme yöntemledir. Bu sistem, iş hedeflerine göre hizalamadaki yeni özellikleri, güncelleştirmeleri ve düzeltmeleri teslim edebilir. Bu metodolojide, hedeflere, kullanım düzenlerine ve müşteri geri bildirimlerine göre yeni özellikler tasarlamadan her şey yer alır. sorunları ortaya çıktığında sistemi düzeltmek, kurtarmak ve sağlamlaştırma. Bu metodolojide kolayca tanımlanmış bir bileşen, sürekli tümleştirme (CI)/sürekli teslim (CD) ardışık düzeni. Bir CI/CD işlem hattı, sistemi oluşturmak için derleme, test ve dağıtım içeren bir dizi adım aracılığıyla bilgi, kod ve kaynak alır. Bu makale, bir kurumsal ortamdaki bir işlem hattı içinde laboratuvarları etkin bir şekilde kullanmanın farklı yollarına odaklanır. 

## <a name="benefits-of-using-labs-in-devops-workflow"></a>DevOps iş akışında Labs kullanmanın avantajları 

### <a name="focused-access"></a>Odaklanmış erişim 
Laboratuvar bir bileşen olarak kullanılması, belirli bir ekosistemin sınırlı bir kişi grubuyla ilişkilendirilmesine izin verir. Genellikle, ortak bir alanda veya belirli bir özellikte çalışan bir ekip veya gruba bir laboratuvar atanmış olur.   

### <a name="infrastructure-replication-in-the-cloud"></a>Bulutta altyapı çoğaltma 
Geliştirici, kaynak kodu ve araçları olan bir geliştirici kutusu içeren bir geliştirme ekosistemi hızlıca ayarlayabilir. Geliştirici ayrıca üretim yapılandırmasıyla neredeyse özdeş olan bir ortam oluşturabilir. Daha hızlı iç döngü geliştirmeye yardımcı olur. 

### <a name="pre-production"></a>Üretim öncesi 
CI/CD ardışık düzeninde bir laboratuvarın olması, zaman uyumsuz test için aynı anda birden çok farklı üretim öncesi ortama veya makineye sahip olmayı kolaylaştırır. Yapı aracıları gibi farklı destek altyapıları, bir laboratuvar içinde dağıtılabilir ve yönetilebilir. 

## <a name="devops-with-devtest-labs"></a>DevTest Labs ile DevOps 

### <a name="development--operation"></a>Geliştirme/Işlem 
Laboratuvar, bir özellik alanında çalışan bir takıma odaklanmalıdır. Bu ortak odak, Araçlar, betikler veya Kaynak Yöneticisi şablonları gibi alana özgü kaynakların paylaşılmasına olanak sağlar. Negatif etkileri daha küçük bir grup ile sınırlandırırken daha hızlı değişikliklere izin verir. Bu paylaşılan kaynaklar, geliştiricilerin tüm gerekli kod, araç ve yapılandırmayla geliştirme VM 'Leri oluşturmalarına olanak tanır. Bunlar dinamik olarak ya da özelleştirmelerle temel görüntüler oluşturan bir sisteme sahip olabilir. Geliştirici VM 'Ler oluşturmayabilir, ancak aynı zamanda laboratuarda uygun Azure kaynaklarını oluşturmak için gerekli şablonları temel alan DevTest Labs ortamları oluşturabilirler. Herhangi bir değişiklik veya zararlı iş, başka bir şeyi etkilemeden laboratuar ortamına karşı yapılabilir. Ürünün, müşterinin makinesinde yüklü olan tek başına bir sistem olduğu senaryoyu göz önünde bulundurun. Bu senaryoda, DevTest Labs, derleme kullanarak ek yazılım yüklemeyi ve kodun daha hızlı iç döngüsü testi için önceden oluşturulan müşteri yapılandırmasını içeren sanal makine oluşturmayı iyileştirmiştir. 
  
## <a name="cicd-pipeline"></a>CI/CD işlem hattı 
CI/CD işlem hattı, bir geliştiricinin çekme isteğinden kod taşıyan, mevcut kodla tümleştiren ve bunu üretim ekosistemine dağıtan, DevOps 'daki kritik bileşenlerden biridir. Tüm kaynakların laboratuvar dahilinde olması gerekmez. Örneğin, bir Jenkins ana bilgisayarı daha kalıcı bir kaynak olarak laboratuvar dışında ayarlanabilir. Labs 'in işlem hattına tümleştirilmesine özgü bazı örnekler aşağıda verilmiştir. 

### <a name="build"></a>Yapı 
Yapı işlem hattı, sürüm ardışık düzenine dönmek için birlikte test edilecek bir bileşen paketi oluşturmaya odaklanır. Laboratuvarlar derleme işlem hattının parçası olarak yapı aracılarının ve diğer destek kaynaklarının bir parçası olabilir. Altyapıyı dinamik olarak oluşturma yeteneğinin daha fazla denetim sağlar. Laboratuarda birden çok ortam kullanabilme sayesinde her derleme zaman uyumsuz olarak çalıştırılabilir ve bu durumda, kaynakları belirli bir yapıda benzersiz şekilde tanımlamak için ortam bilgilerinin parçası olarak yapı KIMLIĞI kullanılıyor olabilir.   

Yapı aracılarında, laboratuvarın erişimi kısıtlama özelliği güvenliği artırır ve yanlışlıkla bozulma olasılığını azaltır.  

### <a name="test"></a>Test etme 
DevTest Labs, bir CI/CD işlem hattının otomatik ve el ile test için kullanılabilen Azure Kaynak (VM 'Ler, ortamlar) oluşturulmasını otomatik hale getirmeye olanak tanır. VM 'Ler, test için gereken farklı özel yapılandırmaların oluşturulması için derleme işlemindeki bilgileri kullanan yapıtlar veya formüller kullanılarak oluşturulur.   

### <a name="release"></a>Sürüm 
DevTest Labs genellikle yayın bölümünde kod dağıtılmadan önce doğrulama için kullanılır. Yapı bölümünde test etmek benzerdir. Üretim kaynakları DevTest Labs içinde dağıtılmamalıdır. 

### <a name="customization"></a>Özelleştirme 
Azure DevOps 'da, belirli laboratuvarlarda VM 'lerin ve ortamların kullanılmasına izin veren mevcut görevler vardır. Azure DevOps Services CI/CD işlem hattını yönetmenin bir yolu olsa da laboratuvarı, REST API 'Leri çağırma, PowerShell betikleri yürütme veya Azure CLı kullanma yeteneğini destekleyen herhangi bir sistemle tümleştirilebilir. 

Bazı CI/CD işlem hattı yöneticilerinin Azure ve DevTest Labs içindeki kaynakları yönetebilen mevcut açık kaynaklı eklentileri vardır. İşlem hattının belirli ihtiyaçlarını karşılamak için bazı özel komut dosyaları kullanmanız gerekebilir.  Göz önünde bulundurularak, bir görevi yürütürken, laboratuvara erişim kazanmak için uygun rolle bir hizmet sorumlusu kullanılır. Hizmet sorumlusu genellikle laboratuvara katkıda bulunan rol erişimine ihtiyaç duyuyor. Daha fazla bilgi için bkz. Azure [DevOps laboratuvarlarını Azure DevOps sürekli tümleştirme ve teslim ardışık düzeni Ile tümleştirme](devtest-lab-integrate-ci-cd.md). 
 