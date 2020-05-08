---
title: Azure Cloud Shell özellikleri | Microsoft Docs
description: Azure Cloud Shell özelliklere genel bakış
services: Azure
documentationcenter: ''
author: maertendMSFT
manager: timlt
tags: azure-resource-manager
ms.assetid: ''
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 04/26/2019
ms.author: damaerte
ms.openlocfilehash: 7490eef7b6203b6bb2623efa4518c7bee5400fb3
ms.sourcegitcommit: 50ef5c2798da04cf746181fbfa3253fca366feaa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2020
ms.locfileid: "82610777"
---
# <a name="features--tools-for-azure-cloud-shell"></a>Azure Cloud Shell için özellikler & araçları

[!INCLUDE [features-introblock](../../includes/cloud-shell-features-introblock.md)]

Azure Cloud Shell üzerinde `Ubuntu 16.04 LTS`çalışır.

## <a name="features"></a>Özellikler

### <a name="secure-automatic-authentication"></a>Güvenli otomatik kimlik doğrulaması

Cloud Shell Azure CLı ve Azure PowerShell için hesap erişiminin güvenli bir şekilde ve otomatik olarak kimliğini doğrular.

### <a name="home-persistence-across-sessions"></a>Oturumlar arasında kalıcılığı $HOME

Dosyaları oturumlarda kalıcı hale getirmek için Cloud Shell ilk başlatma sırasında bir Azure dosya paylaşımının iliştirilmesi için adım adım yol gösterir.
İşlem tamamlandıktan sonra, Cloud Shell tüm gelecek oturumlar için depolama alanınızı otomatik `$HOME\clouddrive`olarak ekler (olarak bağlanır).
Ayrıca, `$HOME` dizininiz Azure dosya paylaşımınızda bir. img olarak kalıcı hale getirilir.
`$HOME` Ve makine durumunun dışındaki dosyalar oturumlarda kalıcı olmaz. SSH anahtarları gibi gizli dizileri depolarken en iyi uygulamaları kullanın. Azure Key Vault gibi hizmetlerde [Kurulum öğreticileri vardır](https://docs.microsoft.com/azure/key-vault/key-vault-manage-with-cli2#prerequisites).

[Cloud Shell 'de kalıcı dosyalar hakkında daha fazla bilgi edinin.](persisting-shell-storage.md)

### <a name="azure-drive-azure"></a>Azure sürücüsü (Azure:)

Cloud Shell içindeki PowerShell, Azure sürücüsünü (`Azure:`) sağlar. İle Giriş dizininize Azure sürücüsüne `cd Azure:` geçiş yapabilirsiniz. `cd  ~`
Azure sürücüsü Işlem, ağ, depolama vb. gibi Azure kaynakları için dosya sistemi gezintisine benzer kolay bulma ve gezinme sağlar.
Bu kaynakları, içinde olduğunuz sürücüden bağımsız olarak yönetmek için tanıdık [Azure PowerShell cmdlet 'lerini](https://docs.microsoft.com/powershell/azure) kullanmaya devam edebilirsiniz.
Azure kaynaklarında yapılan tüm değişiklikler doğrudan Azure portal veya Azure PowerShell cmdlet 'leri aracılığıyla yapılır, Azure sürücüsüne yansıtılır.  Kaynaklarınızı yenilemek için `dir -Force` ' i çalıştırabilirsiniz.

![](media/features-powershell/azure-drive.png)

### <a name="manage-exchange-online"></a>Exchange Online 'ı yönetme

Cloud Shell içindeki PowerShell, Exchange Online modülünün özel bir derlemesini içerir.  Öğesini `Connect-EXOPSSession` çalıştırarak Exchange cmdlet 'lerinizi alın.

![](media/features-powershell/exchangeonline.png)

 `Get-Command -Module tmp_*` öğesini çalıştırın
> [!NOTE]
> Modül adı ile `tmp_`başlamalıdır, aynı ön eke sahip modüller yüklediyseniz, cmdlet 'leri de ortaya çıkmış olur. 

![](media/features-powershell/exchangeonlinecmdlets.png)

### <a name="deep-integration-with-open-source-tooling"></a>Açık kaynak araçları ile derin tümleştirme

Cloud Shell, Terkform, Anbotsız ve Chef InSpec gibi açık kaynaklı araçlar için önceden yapılandırılmış kimlik doğrulaması içerir. Örnek yönergelerden deneyin.

## <a name="tools"></a>Araçlar

|Kategori   |Name   |
|---|---|
|Linux araçları            |bash<br> ZSH<br> dak<br> tmux<br> derinlemesine<br>               |
|Azure Araçları            |[Azure CLI](https://github.com/Azure/azure-cli) ve [Azure klasik CLI](https://github.com/Azure/azure-xplat-cli)<br> [AzCopy](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-v10)<br> [Azure İşlevleri CLI](https://github.com/Azure/azure-functions-core-tools)<br> [Service Fabric CLI](https://docs.microsoft.com/azure/service-fabric/service-fabric-cli)<br> [Batch Shipyard](https://github.com/Azure/batch-shipyard)<br> [blobxfer](https://github.com/Azure/blobxfer)|
|Metin düzenleyicileri           |kod (Cloud Shell Düzenleyicisi)<br> vim<br> nano<br> emacs    |
|Kaynak denetimi         |git                    |
|Derleme araçları            |make<br> maven<br> npm<br> Pip         |
|Kapsayıcılar             |[Docker Makinesi](https://github.com/docker/machine)<br> [Kubectl](https://kubernetes.io/docs/user-guide/kubectl-overview/)<br> [Helm](https://github.com/kubernetes/helm)<br> [DC/OS CLı](https://github.com/dcos/dcos-cli)         |
|Veritabanları              |MySQL istemcisi<br> PostgreSql istemcisi<br> [sqlcmd yardımcı programı](https://docs.microsoft.com/sql/tools/sqlcmd-utility)<br> [MSSQL-Scripter](https://github.com/Microsoft/sql-xplat-cli) |
|Diğer                  |IPython Istemcisi<br> [Cloud Foundry CLı](https://github.com/cloudfoundry/cli)<br> [Terraform](https://www.terraform.io/docs/providers/azurerm/)<br> [Ansible](https://www.ansible.com/microsoft-azure)<br> [Chef InSpec](https://www.chef.io/inspec/)<br> [Pupevcil hayvan sürgüsü](https://puppet.com/docs/bolt/latest/bolt.html)<br> [HashiCorp Packer](https://www.packer.io/)|

## <a name="language-support"></a>Dil desteği

|Dil   |Sürüm   |
|---|---|
|.NET Core  |2.2.402       |
|Başlayın         |1.9        |
|Java       |1,8        |
|Node.js    |8.16.0      |
|PowerShell |[7.0.0](https://github.com/PowerShell/powershell/releases)       |
|Python     |2,7 ve 3,5 (varsayılan)|

## <a name="next-steps"></a>Sonraki adımlar
[Bash Cloud Shell hızlı başlangıç](quickstart.md) <br>
[PowerShell Cloud Shell hızlı başlangıç](quickstart-powershell.md) <br>
[Azure CLı hakkında bilgi edinin](https://docs.microsoft.com/cli/azure/) <br>
[Azure PowerShell hakkında bilgi edinin](https://docs.microsoft.com/powershell/azure/) <br>
