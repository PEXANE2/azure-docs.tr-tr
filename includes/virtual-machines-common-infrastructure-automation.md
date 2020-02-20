---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 04/11/2019
ms.author: cynthn
ms.openlocfilehash: 525eba8a5a4e891526eb32a24287ea3887ee6743
ms.sourcegitcommit: 64def2a06d4004343ec3396e7c600af6af5b12bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77474246"
---
Oluşturup uygun ölçekte tutarlı bir şekilde Azure sanal makineleri (VM'ler) yönetmek için Otomasyon biçimi genellikle istenildiği gibi. Birçok araca ve tam Azure altyapı dağıtımı ve Yönetimi yaşam döngüsünü otomatikleştirmenize olanak tanıyan çözümler vardır. Bu makalede, Azure'da kullanabileceğiniz altyapı Otomasyonu araçlardan bazıları tanıtılmaktadır. Bu araçlar genellikle aşağıdaki yaklaşımlardan birini sığacak:

- Sanal Makine yapılandırmasını otomatikleştirin
    - Araçlar, [anormal](#ansible), [Chef](#chef)ve [pupevcil hayvan](#puppet)içerir.
    - VM özelleştirmesine özgü araçlar, Linux sanal makineleri için [Cloud-init](#cloud-init) , [PowerShell İstenen Durum Yapılandırması (DSC)](#powershell-dsc)ve tüm Azure VM 'Leri Için [Azure Özel Betik uzantısı](#azure-custom-script-extension) 'nı içerir.
 
- Altyapı yönetimini otomatikleştirme
    - Araçlar, özel VM görüntü yapılarını otomatikleştirmek için [Packer](#packer) ve altyapı oluşturma işlemini otomatikleştirmek Için [terrayform](#terraform) içerir.
    - [Azure Otomasyonu](#azure-automation) , Azure ve şirket içi altyapınız üzerinde eylemler gerçekleştirebilir.

- Uygulama dağıtımı ve teslim otomatikleştirin
    - [Azure DevOps Services](#azure-devops-services) ve [Jenkins](#jenkins)örnekleri sayılabilir.

## <a name="ansible"></a>Ansible
[Anormal](https://www.ansible.com/) , yapılandırma YÖNETIMI, VM oluşturma veya uygulama dağıtımı için bir Otomasyon altyapısıdır. Ansible, kimlik doğrulaması ve hedef makineleri yönetmek için genellikle SSH anahtarlarıyla bir aracısız modelini kullanır. Belirli görevleri gerçekleştirmek kullanılabilir olan Ansible modül sayısı ile playbook'ları yapılandırma görevleri tanımlanır. Daha fazla bilgi için bkz. [anormal çalışma](https://www.ansible.com/how-ansible-works).

Şunları nasıl yapacağınızı öğrenin:

- [Azure ile kullanmak Için Linux üzerinde erişilebilir ve yapılandırın](../articles/ansible/ansible-install-configure.md).
- [Bir Linux sanal makinesi oluşturun](../articles/ansible/ansible-create-vm.md).
- [Linux sanal makinesini yönetin](../articles/ansible/ansible-manage-linux-vm.md).


## <a name="chef"></a>Chef
[Chef](https://www.chef.io/) , altyapınızın nasıl yapılandırıldığını, dağıtıldığını ve yönetildiğini tanımlamaya yardımcı olan bir Otomasyon platformudur. Uygulama yaşam döngüsü Otomasyon altyapısı yerine, Chef, Habitat ek bileşenler dahil ve güvenlik ve ilke gereksinimlerine uyum Chef yardımcı olan InSpec otomatikleştirin. Chef istemciler, bir veya daha fazla merkezi Chef, yapılandırmaları yönetin ve sunucuları ile hedef makinelerde yüklenir. Daha fazla bilgi için bkz. [Chef 'e genel bakış](https://docs.chef.io/chef_overview.html).

Şunları nasıl yapacağınızı öğrenin:

- [Azure Marketi 'Nden Chef otomatikleştirmesini dağıtın](https://azuremarketplace.microsoft.com/marketplace/apps/chef-software.chef-automate?tab=Overview).
- [Windows üzerinde Chef 'ı yükleyip Azure VM 'leri oluşturun](../articles/virtual-machines/windows/chef-automation.md).


## <a name="puppet"></a>Puppet
[Pupevcil hayvan](https://www.puppet.com) , uygulama teslimi ve dağıtım sürecini işleyen kurumsal özellikli bir Otomasyon platformudur. Aracılar Puppet Azure altyapısının istenen yapılandırmasını tanımlamak bildirimleri çalıştırmak için ana ve Vm'leri izin vermek için hedef makinede yüklü. Puppet bir Gelişmiş devops iş akışı için Jenkins ve GitHub gibi diğer çözümlerle tümleştirilebilir. Daha fazla bilgi için bkz. [Pupevcil hayvan nasıl çalıştığını öğrenin](https://puppet.com/products/how-puppet-works).

Şunları nasıl yapacağınızı öğrenin:

- [Azure Marketi 'Nden Pupevcil hayvan 'ı dağıtın](https://azuremarketplace.microsoft.com/marketplace/apps/Puppet.puppet-agent-windows-asm?tab=Overview).


## <a name="cloud-init"></a>Cloud-init
[Cloud-init](https://cloudinit.readthedocs.io), Linux VM’sini ilk kez önyüklendiğinde özelleştirmeyi sağlayan, sık kullanılan bir yaklaşımdır. cloud-init’i paket yükleme, dosyalara yazma ve kullanıcılar ile güvenliği yapılandırma işlemleri için kullanabilirsiniz. Cloud-init önyükleme işlemi sırasında çağrıldığı için ek adımlar veya yapılandırmanıza uygulayabileceğiniz gerekli aracı yoktur.  `#cloud-config` dosyalarınızı düzgün şekilde biçimlendirme hakkında daha fazla bilgi için bkz. [Cloud-init belge sitesi](https://cloudinit.readthedocs.io/en/latest/topics/format.html#cloud-config-data).  `#cloud-config` dosyalar, Base64 olarak kodlanmış metin dosyalarıdır.

Cloud-init, dağıtımlar arasında da çalışır. Örneğin, bir paket yüklemek için **apt-get install** veya **yum install** kullanmazsınız. Bunun yerine, yüklenecek paketlerin listesini tanımlayabilirsiniz. Cloud-init, seçtiğiniz dağıtım için yerel paket yönetim aracını otomatik olarak kullanır.

Etkin olarak desteklenen Linux distro ortaklarımızla birlikte kullanılabilir cloud-init etkinleştirilmiş görüntüleri Azure Market'te sahip olmak için çalışıyoruz. Bu görüntüleri cloud-init dağıtımlarınızı yapın ve yapılandırmaları VM'ler ve sanal makine ölçek kümeleri ile sorunsuz bir şekilde çalışır. Azure'da cloud-init hakkında daha ayrıntılı bilgi edinin:

- [Azure 'da Linux sanal makineleri için Cloud-init desteği](../articles/virtual-machines/linux/using-cloud-init.md)
- [Cloud-init kullanarak OTOMATIK VM yapılandırması ile ilgili bir öğretici deneyin](../articles/virtual-machines/linux/tutorial-automate-vm-deployment.md).


## <a name="powershell-dsc"></a>PowerShell DSC
[PowerShell Istenen durum yapılandırması (DSC)](/powershell/scripting/dsc/overview/overview) , hedef makinelerin yapılandırmasını tanımlayan bir yönetim platformudur. DSC, [Open Management Infrastructure (OMı) sunucusu](https://collaboration.opengroup.org/omi/)üzerinden Linux üzerinde de kullanılabilir.

DSC yapılandırmaları ne bir makineye yükleyin ve ana bilgisayar yapılandırma tanımlar. İstenen Eylemler gönderilen yapılandırmalarına göre işler her hedef düğümde yerel Configuration Manager'ı (LCM) altyapısı çalıştırır. Bir çekme sunucusu DSC yapılandırmaları ve ilişkili kaynakları depolamak için merkezi bir konakta çalışan bir web hizmetidir. Çekme sunucusu, uyumluluk ve gerekli yapılandırmaları sağlamak için her bir hedef ana bilgisayarda LCM altyapısı ile iletişim kurar.

Şunları nasıl yapacağınızı öğrenin:

- [Temel BIR DSC yapılandırması oluşturun](/powershell/scripting/dsc/quickstarts/website-quickstart).
- [DSC çekme sunucusu yapılandırın](/powershell/scripting/dsc/pull-server/pullserver).
- [Linux IÇIN DSC kullanın](/powershell/scripting/dsc/getting-started/lnxgettingstarted).


## <a name="azure-custom-script-extension"></a>Azure özel betik uzantısı
[Linux](../articles/virtual-machines/linux/extensions-customscript.md) veya [Windows](../articles/virtual-machines/windows/extensions-customscript.md) Için Azure Özel Betik uzantısı, Azure VM 'lerinde betikleri indirir ve yürütür. VM oluşturma veya VM sonra istediğiniz zaman kullanımda olduğunda, uzantıyı kullanabilirsiniz. 

Betikler Azure depolama veya GitHub deposu gibi genel bir konumdan indirilebilir. Özel betik uzantısı ile komut dosyaları, kaynak VM üzerinde çalışan herhangi bir dilde yazabilirsiniz. Bu komut dosyaları, uygulamaları yüklemek veya VM olarak istenen yapılandırmak için kullanılabilir. Güvenli kimlik bilgileri için korumalı bir yapılandırmada parolalar gibi hassas bilgileri depolanabilir. Bu kimlik bilgileri, yalnızca sanal makine içinde şifresi çözülür.

Şunları nasıl yapacağınızı öğrenin:

- [Azure CLI ile bir LINUX VM oluşturun ve özel Betik uzantısı 'nı kullanın](../articles/virtual-machines/scripts/virtual-machines-linux-cli-sample-create-vm-nginx.md?toc=%2fcli%2fazure%2ftoc.json).
- [Azure PowerShell ile bir Windows sanal makinesi oluşturun ve özel betik uzantısını kullanın](../articles/virtual-machines/scripts/virtual-machines-windows-powershell-sample-create-vm-iis.md?toc=%2fpowershell%2fmodule%2ftoc.json).


## <a name="packer"></a>Packer
[Packer](https://www.packer.io) , Azure 'da özel bir VM görüntüsü oluşturduğunuzda derleme işlemini otomatikleştirir. Packer, işletim sistemini tanımlamak ve VM için kendi özel gereksinimlerinize göre özelleştirme yapılandırma sonrası komut dosyalarını çalıştırmak için kullanın. Yapılandırıldıktan sonra VM'ye yönetilen Disk görüntüsü dahil edilir. Packer kaynak VM, ağ ve depolama kaynakları oluşturma, yapılandırma betiklerini çalıştırın ve ardından sanal makine görüntüsü oluşturma işlemini otomatikleştirir.

Şunları nasıl yapacağınızı öğrenin:

- [Azure 'da bir LINUX VM görüntüsü oluşturmak Için Packer 'ı kullanın](../articles/virtual-machines/linux/build-image-with-packer.md).
- [Azure 'da bir WINDOWS VM görüntüsü oluşturmak Için Packer 'ı kullanın](../articles/virtual-machines/windows/build-image-with-packer.md).


## <a name="terraform"></a>Terraform
[Terkform](https://www.terraform.io) , tek bir şablon biçimi ile Azure altyapısının tamamını tanımlamanıza ve oluşturmanıza olanak tanıyan bir otomasyon aracıdır. Bu, diyez ICORP yapılandırma DILI (HCL). Terraform ile ağ, depolama ve uygulama çözümü için VM kaynakları oluşturma işlemini otomatik hale getiren şablonları tanımlayın. Mevcut Terraform şablonlarınızı tutarlılığı sağlamak ve altyapı dağıtımı için bir Azure Resource Manager şablonu dönüştürmek zorunda kalmadan basitleştirmek için Azure ile diğer platformlar için kullanabilirsiniz.

Şunları nasıl yapacağınızı öğrenin:

- [Azure Ile Terrayform 'U yükleyip yapılandırın](../articles/terraform/terraform-install-configure.md).
- [Terrayform ile bir Azure altyapısı oluşturun](../articles/terraform/terraform-create-complete-vm.md).


## <a name="azure-automation"></a>Azure Otomasyonu
[Azure Otomasyonu](https://azure.microsoft.com/services/automation/) , hedeflediğiniz VM 'lerde bir dizi görevi işlemek için Runbook 'ları kullanır. Azure Otomasyonu, varolan Vm'leri yönetmek yerine bir altyapı oluşturmak için kullanılır. Azure Otomasyonu, hem Linux ve Windows sanal makineleri, hem de karma runbook çalışanı ile şirket içi sanal veya fiziksel makineler üzerinde çalıştırabilir. Runbook'ları, GitHub gibi kaynak denetim deposu depolanabilir. Bu runbook, daha sonra el ile ya da tanımlanmış bir zamanlamaya göre çalıştırabilir.

Azure Otomasyonu, Vm'leri belirli bir dizi nasıl yapılandırılmalıdır tanımları oluşturmanıza olanak sağlayan bir Desired State Configuration ' nı (DSC) hizmet de sunar. DSC, ardından gerekli yapılandırma uygulanır ve VM tutarlı kalmasını sağlar. Azure Automation DSC, hem Windows hem de Linux makinelerinde çalışır.

Şunları nasıl yapacağınızı öğrenin:

- [PowerShell runbook 'U oluşturun](../articles/automation/automation-first-runbook-textual-powershell.md).
- [Şirket içi kaynakları yönetmek Için karma Runbook Worker 'ı kullanın](../articles/automation/automation-hybrid-runbook-worker.md).
- [Azure Automation DSC kullanın](../articles/automation/automation-dsc-getting-started.md).


## <a name="azure-devops-services"></a>Azure DevOps Services
[Azure DevOps Services](https://www.visualstudio.com/team-services/) , kodu paylaşmanıza ve izlemenize, otomatik derlemeler kullanmanıza ve eksiksiz bir sürekli tümleştirme ve GELIŞTIRME (CI/CD) işlem hattı oluşturmanıza yardımcı olan bir araç paketidir. Azure DevOps hizmetleriyle, kullanımı kolaylaştırmak için Visual Studio ve diğer düzenleyiciler ile tümleştirilir. Azure DevOps hizmetleriyle oluşturabilir ve Azure Vm'leri yapılandırabilir ve ardından kod bu bilgisayarlara dağıtın.

Daha fazla bilgi:

- [Azure DevOps Services](https://docs.microsoft.com/azure/devops/user-guide/index?view=vsts).


## <a name="jenkins"></a>Jenkins
[Jenkins](https://www.jenkins.io) , uygulamaları dağıtmaya ve test etmeye ve kod teslimi için otomatik işlem hatları oluşturmaya yardımcı olan bir sürekli tümleştirme sunucusudur. Eklenti temel Jenkins platformu genişletmek için yüzlerce vardır ve diğer birçok ürün ve Web kancaları aracılığıyla çözümler ile de tümleştirebilirsiniz. El ile Jenkins Azure VM üzerinde yüklemek, Jenkins'den Docker kapsayıcısı içinde çalıştırın veya önceden oluşturulmuş Azure Market görüntüsü kullanın.

Şunları nasıl yapacağınızı öğrenin:

- [Azure 'Da Jenkins, GitHub ve Docker ile bir Linux sanal makinesi üzerinde bir geliştirme altyapısı oluşturun](../articles/virtual-machines/linux/tutorial-jenkins-github-docker-cicd.md).


## <a name="next-steps"></a>Sonraki adımlar
Azure'da altyapı Otomasyon araçları kullanmak için birçok farklı seçenek vardır. Sahip olduğunuz bir ortamı ve gereksinimlerinize en uygun çözümü kullanma özgürlüğünü tanıyabilir. Kullanmaya başlamak ve Azure 'da yerleşik araçları denemek için bkz. bir [Linux](../articles/virtual-machines/linux/tutorial-automate-vm-deployment.md) veya [Windows](../articles/virtual-machines/windows/tutorial-automate-vm-deployment.md) sanal makinesi özelleştirmesini otomatikleştirme.
