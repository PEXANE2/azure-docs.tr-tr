---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 04/11/2019
ms.author: cynthn
ms.openlocfilehash: 9cbc48d8bca2f7491d0464be1c5bd64054927dc9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77608725"
---
Azure sanal makinelerini (VM' ler) tutarlı bir ölçekte oluşturmak ve yönetmek için genellikle bir tür otomasyon istenir. Azure altyapı dağıtımı ve yönetim yaşam döngüsünü otomatikleştirmenize olanak tanıyan birçok araç ve çözüm vardır. Bu makalede, Azure'da kullanabileceğiniz bazı altyapı otomasyon araçları tanıtışlanmaktadır. Bu araçlar genellikle aşağıdaki yaklaşımlardan birine uyar:

- VM'lerin yapılandırmasını otomatikleştirme
    - Araçlar [Ansible,](#ansible) [Şef](#chef)ve [Kukla](#puppet)içerir.
    - VM özelleştirmesine özgü araçlar arasında Linux VM'leri için [bulut ekleme,](#cloud-init) [PowerShell İstenen Durum Yapılandırması (DSC)](#powershell-dsc)ve tüm Azure VM'leri için [Azure Özel Komut Dosyası Uzantısı](#azure-custom-script-extension) yer almaktadır.
 
- Altyapı yönetimini otomatikleştirin
    - Araçlar, özel VM görüntü yapılarını otomatikleştirmek için [Packer'ı](#packer) ve altyapı oluşturma işlemini otomatikleştirmek için [Terraform'u](#terraform) içerir.
    - [Azure Otomasyonu,](#azure-automation) Azure ve şirket içi altyapınızda eylemler gerçekleştirebilir.

- Uygulama dağıtımını ve teslimini otomatikleştirin
    - Bunlara örnek olarak [Azure DevOps Hizmetleri](#azure-devops-services) ve [Jenkins](#jenkins)verilebilir.

## <a name="ansible"></a>Ansible
[Ansible,](https://www.ansible.com/) yapılandırma yönetimi, VM oluşturma veya uygulama dağıtımı için bir otomasyon motorudur. Ansible, hedef makinelerin kimliğini doğrulamak ve yönetmek için genellikle SSH tuşlarına sahip aracısız bir model kullanır. Yapılandırma görevleri oyun kitaplarında tanımlanır ve belirli görevleri gerçekleştirmek için bir dizi Ansible modülü kullanılabilir. Daha fazla bilgi için, [Ansible nasıl çalışır](https://www.ansible.com/how-ansible-works)bakın.

Şunları nasıl yapacağınızı öğrenin:

- [Ansible'ı Azure ile kullanmak üzere Linux'a yükleyin ve yapılandırın.](../articles/ansible/ansible-install-configure.md)
- [Bir Linux sanal makine oluşturun.](../articles/ansible/ansible-create-vm.md)
- [Bir Linux sanal makine yönetin.](../articles/ansible/ansible-manage-linux-vm.md)


## <a name="chef"></a>Chef
[Chef,](https://www.chef.io/) altyapınızın nasıl yapılandırıldığı, dağıtıldığı ve yönetildiğini tanımlamaya yardımcı olan bir otomasyon platformudur. Ek bileşenler arasında altyapı yerine uygulama yaşam döngüsü otomasyonu için Chef Habitat ve güvenlik ve politika gereksinimlerine uyumluluğu otomatikleştirmeye yardımcı olan Chef InSpec yer aldı. Chef İstemciler, yapılandırmaları depolayan ve yöneten bir veya daha fazla merkezi Chef Sunucusuyla hedef makinelere yüklenir. Daha fazla bilgi için [Şefe Genel Bakış](https://docs.chef.io/chef_overview.html)bölümüne bakın.

Şunları nasıl yapacağınızı öğrenin:

- [Azure Marketi'nden Chef Automate'i dağıtın.](https://azuremarketplace.microsoft.com/marketplace/apps/chef-software.chef-automate?tab=Overview)
- [Chef'i Windows'a yükleyin ve Azure VM'leri oluşturun.](../articles/chef/chef-automation.md)


## <a name="puppet"></a>Puppet
[Puppet,](https://www.puppet.com) uygulama teslimi ve dağıtım işlemini işleyen, kuruluşa hazır bir otomasyon platformudur. Puppet Master'ın Azure altyapısının ve VM'lerin istenilen yapılandırmasını tanımlayan manifestoları çalıştırabilmesi için aracılar hedef makinelere yüklenir. Puppet, geliştirilmiş devops iş akışı için Jenkins ve GitHub gibi diğer çözümlerle entegre olabilir. Daha fazla bilgi için [Puppet'Un nasıl çalıştığını](https://puppet.com/products/how-puppet-works)görün.

Şunları nasıl yapacağınızı öğrenin:

- [Azure Marketinden Kukla'yı dağıtın.](https://azuremarketplace.microsoft.com/marketplace/apps/Puppet.puppet-agent-windows-asm?tab=Overview)


## <a name="cloud-init"></a>Cloud-init
[Cloud-init](https://cloudinit.readthedocs.io), Linux VM’sini ilk kez önyüklendiğinde özelleştirmeyi sağlayan, sık kullanılan bir yaklaşımdır. cloud-init’i paket yükleme, dosyalara yazma ve kullanıcılar ile güvenliği yapılandırma işlemleri için kullanabilirsiniz. Bulut init ilk önyükleme işlemi sırasında çağrıldığı için, yapılandırmanızı uygulamak için ek adımlar veya gerekli aracılar yoktur.  Dosyalarınızı `#cloud-config` düzgün biçimlendirme hakkında daha fazla bilgi için [bulut init dokümantasyon sitesine](https://cloudinit.readthedocs.io/en/latest/topics/format.html#cloud-config-data)bakın.  `#cloud-config`dosyalar base64 kodlanmış metin dosyalarıdır.

Cloud-init, dağıtımlar arasında da çalışır. Örneğin, bir paket yüklemek için **apt-get install** veya **yum install** kullanmazsınız. Bunun yerine, yüklenecek paketlerin listesini tanımlayabilirsiniz. Cloud-init, seçtiğiniz dağıtım için yerel paket yönetim aracını otomatik olarak kullanır.

Azure pazarda bulut ekleme özellikli görüntülerin kullanılabilmesi için onaylanan Linux dağıtım ortaklarımızla aktif olarak çalışıyoruz. Bu görüntüler, bulut init dağıtımlarınızın ve yapılandırmalarınızın VM'ler ve sanal makine ölçek kümeleriyle sorunsuz çalışmasını sağlar. Azure'da bulut-init hakkında daha fazla bilgi edinin:

- [Azure'da Linux sanal makineleri için bulut desteği](../articles/virtual-machines/linux/using-cloud-init.md)
- [Bulut init kullanarak otomatik VM yapılandırması hakkında bir öğretici deneyin.](../articles/virtual-machines/linux/tutorial-automate-vm-deployment.md)


## <a name="powershell-dsc"></a>PowerShell DSC
[PowerShell İstenen Durum Yapılandırması (DSC),](/powershell/scripting/dsc/overview/overview) hedef makinelerin yapılandırmasını tanımlayan bir yönetim platformudur. DSC, Açık Yönetim Altyapısı [(OMI) sunucusu](https://collaboration.opengroup.org/omi/)aracılığıyla Linux'ta da kullanılabilir.

DSC yapılandırmaları, makineye ne yüklenir ve ana bilgisayar nasıl yapılandırılabildiğini tanımlar. Yerel Yapılandırma Yöneticisi (LCM) altyapısı, istenen eylemleri itilen yapılandırmalara göre işleyen her hedef düğümde çalışır. Çekme sunucusu, DSC yapılandırmalarını ve ilişkili kaynakları depolamak için merkezi bir ana bilgisayarda çalışan bir web hizmetidir. Çekme sunucusu, gerekli yapılandırmaları sağlamak ve uyumluluk hakkında rapor vermek için her hedef ana bilgisayardaki LCM motoruyla iletişim kurar.

Şunları nasıl yapacağınızı öğrenin:

- [Temel bir DSC yapılandırması oluşturun.](/powershell/scripting/dsc/quickstarts/website-quickstart)
- [DSC çekme sunucusuyapılandırın.](/powershell/scripting/dsc/pull-server/pullserver)
- [Linux için DSC kullanın.](/powershell/scripting/dsc/getting-started/lnxgettingstarted)


## <a name="azure-custom-script-extension"></a>Azure Özel Betik Uzantısı
[Linux](../articles/virtual-machines/linux/extensions-customscript.md) veya [Windows](../articles/virtual-machines/windows/extensions-customscript.md) için Azure Özel Komut Dosyası Uzantısı, Azure VM'lerinde komut dosyaları indirir ve yürütür. Uzantıyı, bir VM oluşturduğunuzda veya VM kullanımdan herhangi bir zamanda kullanabilirsiniz. 

Komut dosyaları Azure depolama dan veya GitHub deposu gibi herkese açık herhangi bir konumdan indirilebilir. Özel Komut Dosyası Uzantısı ile, kaynak VM üzerinde çalışan herhangi bir dilde komut dosyası yazabilirsiniz. Bu komut dosyaları uygulamaları yüklemek veya VM'yi istediğiniz gibi yapılandırmak için kullanılabilir. Kimlik bilgilerini güvence altına almak için parolalar gibi hassas bilgiler korumalı bir yapılandırmada depolanabilir. Bu kimlik bilgileri yalnızca VM içinde deşifre edilir.

Şunları nasıl yapacağınızı öğrenin:

- [Azure CLI ile bir Linux VM oluşturun ve Özel Komut Dosyası Uzantısı'nı kullanın.](../articles/virtual-machines/scripts/virtual-machines-linux-cli-sample-create-vm-nginx.md?toc=%2fcli%2fazure%2ftoc.json)
- [Azure PowerShell ile bir Windows VM oluşturun ve Özel Komut Dosyası Uzantısı'nı kullanın.](../articles/virtual-machines/scripts/virtual-machines-windows-powershell-sample-create-vm-iis.md?toc=%2fpowershell%2fmodule%2ftoc.json)


## <a name="packer"></a>Peker
[Packer,](https://www.packer.io) Azure'da özel bir VM görüntüsü oluşturduğunuzda yapı işlemini otomatikleştirir. İşletim sistemi tanımlamak ve VM'yi özel gereksinimlerinize göre özelleştiren yapılandırma sonrası komut dosyalarını çalıştırmak için Packer'ı kullanırsınız. Yapılandırıldıktan sonra, VM yönetilen disk görüntüsü olarak yakalanır. Packer, kaynak VM, ağ ve depolama kaynaklarını oluşturmak, yapılandırma komut dosyalarını çalıştırmak ve ardından VM görüntüsünü oluşturmak için işlemi otomatikleştirir.

Şunları nasıl yapacağınızı öğrenin:

- [Azure'da bir Linux VM görüntüsü oluşturmak için Packer'ı kullanın.](../articles/virtual-machines/linux/build-image-with-packer.md)
- [Azure'da bir Windows VM görüntüsü oluşturmak için Packer'ı kullanın.](../articles/virtual-machines/windows/build-image-with-packer.md)


## <a name="terraform"></a>Terraform
[Terraform,](https://www.terraform.io) hashiCorp Yapılandırma Dili (HCL) (HCL) gibi tek bir şablon biçimi diliyle tüm Azure altyapısını tanımlamanıza ve oluşturmanıza olanak tanıyan bir otomasyon aracıdır. Terraform ile, belirli bir uygulama çözümü için ağ, depolama ve VM kaynakları oluşturmak için işlemi otomatikleştiren şablonlar tanımlarsınız. Azure ile diğer platformlar için varolan Terraform şablonlarınızı kullanarak tutarlılığı sağlayabilir ve azure kaynak yöneticisi şablonuna dönüştürmenize gerek kalmadan altyapı dağıtımını basitleştirebilirsiniz.

Şunları nasıl yapacağınızı öğrenin:

- [Terraform'u Azure ile yükleyin ve yapılandırır.](../articles/terraform/terraform-install-configure.md)
- [Terraform ile bir Azure altyapısı oluşturun.](../articles/terraform/terraform-create-complete-vm.md)


## <a name="azure-automation"></a>Azure Otomasyonu
[Azure Otomasyonu,](https://azure.microsoft.com/services/automation/) hedeflediğiniz VM'lerde bir dizi görevi işlemek için runbook'ları kullanır. Azure Otomasyonu, bir altyapı oluşturmak yerine varolan VM'leri yönetmek için kullanılır. Azure Otomasyonu, hem Linux hem de Windows VM'lerde ve karma çalışma kitabı çalışanıyla şirket içi sanal veya fiziksel makinelerde kullanılabilir. Runbook'lar GitHub gibi bir kaynak denetim deposunda depolanabilir. Bu runbook'lar daha sonra el ile veya tanımlı bir zamanlamada çalıştırılabilir.

Azure Otomasyonu, belirli bir VM kümesinin nasıl yapılandırılması gerektiğine yönelik tanımlar oluşturmanıza olanak tanıyan bir İstenilen Durum Yapılandırması (DSC) hizmeti de sağlar. DSC daha sonra gerekli yapılandırmanın uygulanmasını ve VM'nin tutarlı kalmasını sağlar. Azure Automation DSC hem Windows hem de Linux makinelerinde çalışır.

Şunları nasıl yapacağınızı öğrenin:

- [PowerShell runbook oluşturun.](../articles/automation/automation-first-runbook-textual-powershell.md)
- [Şirket içi kaynakları yönetmek için Karma Runbook](../articles/automation/automation-hybrid-runbook-worker.md)İşçisi'ni kullanın.
- [Azure Otomasyon DSC'yi kullanın.](../articles/automation/automation-dsc-getting-started.md)


## <a name="azure-devops-services"></a>Azure DevOps Services
[Azure DevOps Hizmetleri,](https://www.visualstudio.com/team-services/) kodu paylaşmanıza ve izlemenize, otomatik yapılar kullanmanıza ve tam bir sürekli tümleştirme ve geliştirme (CI/CD) ardışık alanı oluşturmanıza yardımcı olan bir araç paketidir. Azure DevOps Hizmetleri, kullanımı kolaylaştırmak için Visual Studio ve diğer editörlerle tümleşir. Azure DevOps Hizmetleri ayrıca Azure VM'leri oluşturabilir ve yapılandırabilir ve ardından onlara kod dağıtabilir.

Aşağıdakiler hakkında daha fazla bilgi edinin:

- [Azure Devops Hizmetleri](https://docs.microsoft.com/azure/devops/user-guide/index?view=vsts).


## <a name="jenkins"></a>Jenkins
[Jenkins,](https://www.jenkins.io) uygulamaları dağıtmaya ve sınamana ve kod teslimi için otomatik ardışık hatlar oluşturmaya yardımcı olan sürekli bir tümleştirme sunucusudur. Çekirdek Jenkins platformu genişletmek için eklentileri yüzlerce vardır, ve aynı zamanda webhooks aracılığıyla diğer birçok ürün ve çözümler ile entegre edebilirsiniz. Jenkins'i bir Azure VM'ye el ile yükleyebilir, Bir Docker kapsayıcısından Jenkins çalıştırabilir veya önceden oluşturulmuş bir Azure Marketi görüntüsünü kullanabilirsiniz.

Şunları nasıl yapacağınızı öğrenin:

- [Jenkins, GitHub ve Docker ile Azure'da Bir Linux VM'de geliştirme altyapısı oluşturun.](../articles/jenkins/tutorial-jenkins-github-docker-cicd.md)


## <a name="next-steps"></a>Sonraki adımlar
Azure'da altyapı otomasyon araçlarını kullanmak için birçok farklı seçenek vardır. İhtiyaçlarınıza ve çevrenize en uygun çözümü kullanma özgürlüğüne sahipsiniz. Başlamak ve Azure'da yerleşik bazı araçları denemek için, [Bir Linux](../articles/virtual-machines/linux/tutorial-automate-vm-deployment.md) veya [Windows](../articles/virtual-machines/windows/tutorial-automate-vm-deployment.md) VM'nin özelleştirmesini nasıl otomatikleştirebilirsiniz' i görün.
