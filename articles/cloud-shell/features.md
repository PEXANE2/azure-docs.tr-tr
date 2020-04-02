---
title: Azure Bulut BulutU özellikleri | Microsoft Dokümanlar
description: Azure Bulut BulutU Bulutu'ndaki özelliklere genel bakış
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
ms.openlocfilehash: 78275211b77cb34cf9aa92e44778e5d4d81fffed
ms.sourcegitcommit: b0ff9c9d760a0426fd1226b909ab943e13ade330
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80521005"
---
# <a name="features--tools-for-azure-cloud-shell"></a>Azure Bulut Bulutu için özellikler & araçları

[!INCLUDE [features-introblock](../../includes/cloud-shell-features-introblock.md)]

Azure Bulut Shell `Ubuntu 16.04 LTS`çalışır.

## <a name="features"></a>Özellikler

### <a name="secure-automatic-authentication"></a>Güvenli otomatik kimlik doğrulama

Cloud Shell, Azure CLI ve Azure PowerShell için hesap erişimini güvenli ve otomatik olarak doğrular.

### <a name="home-persistence-across-sessions"></a>oturumlar arasında $HOME kalıcılığı

Oturumlar arasında dosyaları sürdürmek için Cloud Shell, ilk başlatmada bir Azure dosya paylaşımı eklemekonusunda size yol sunar.
Tamamlandığında, Cloud Shell gelecekteki tüm oturumlar `$HOME\clouddrive`için depolama alanınızı otomatik olarak (monte edilmiş olarak) takacaktır.
Ayrıca, dizininiz `$HOME` Azure Dosya paylaşımınızda bir .img olarak kalıcıdır.
Dışındaki dosyalar `$HOME` ve makine durumu oturumları arasında kalıcı değildir. SSH anahtarları gibi sırları saklarken en iyi uygulamaları kullanın. Azure Key Vault gibi hizmetlerin [kurulum için öğreticileri vardır.](https://docs.microsoft.com/azure/key-vault/key-vault-manage-with-cli2#prerequisites)

[Cloud Shell'de kalıcı dosyalar hakkında daha fazla bilgi edinin.](persisting-shell-storage.md)

### <a name="azure-drive-azure"></a>Azure sürücüsü (Azure:)

Cloud Shell'deki PowerShell,`Azure:`Azure sürücüsüsağlar ( ). Azure sürücüsüne ' le `cd Azure:` geçiş yapabilir ve `cd  ~`'' ile ev dizininize geri dönebilirsiniz.
Azure sürücüsü, dosya sistemi gezintisine benzer şekilde İşlem, Ağ, Depolama vb. gibi Azure kaynaklarının kolayca keşfedilmesini ve gezinmesini sağlar.
Bu kaynakları, içinde bulunduğunuz sürücüden bağımsız olarak yönetmek için tanıdık [Azure PowerShell cmdletlerini](https://docs.microsoft.com/powershell/azure) kullanmaya devam edebilirsiniz.
Azure kaynaklarında doğrudan Azure portalında veya Azure PowerShell cmdlets aracılığıyla yapılan değişiklikler Azure sürücüsüne yansıtılır.  Kaynaklarınızı `dir -Force` yenilemek için çalıştırabilirsiniz.

![](media/features-powershell/azure-drive.png)

### <a name="manage-exchange-online"></a>Exchange Online yönet

Cloud Shell'deki PowerShell, Exchange Online modülünün özel bir yapısına evrilmektedir.  Exchange `Connect-EXOPSSession` cmdlets almak için çalıştırın.

![](media/features-powershell/exchangeonline.png)

 `Get-Command -Module tmp_*` öğesini çalıştırın
> [!NOTE]
> Modül adı ile `tmp_`başlamalıdır , aynı önek ile modülleri yüklü varsa, onların cmdlets de su yüzüne olacaktır. 

![](media/features-powershell/exchangeonlinecmdlets.png)

### <a name="deep-integration-with-open-source-tooling"></a>Açık kaynak takımlarıyla derin entegrasyon

Cloud Shell, Terraform, Ansible ve Chef InSpec gibi açık kaynak araçları için önceden yapılandırılmış kimlik doğrulaması içerir. Örnek gözden geçirmelerden deneyin.

## <a name="tools"></a>Araçlar

|Kategori   |Adı   |
|---|---|
|Linux araçları            |bash<br> zş<br> ş<br> tmux<br> Kazmak<br>               |
|Azure araçları            |[Azure CLI](https://github.com/Azure/azure-cli) ve [Azure klasik CLI](https://github.com/Azure/azure-xplat-cli)<br> [AzCopy](https://docs.microsoft.com/previous-versions/azure/storage/storage-use-azcopy#writing-your-first-azcopy-command)<br> [Azure İşlevleri CLI](https://github.com/Azure/azure-functions-core-tools)<br> [Service Fabric CLI](https://docs.microsoft.com/azure/service-fabric/service-fabric-cli)<br> [Batch Shipyard](https://github.com/Azure/batch-shipyard)<br> [blobxfer](https://github.com/Azure/blobxfer)|
|Metin editörleri           |kod (Cloud Shell düzenleyicisi)<br> vim<br> nano<br> emacs    |
|Kaynak denetimi         |git                    |
|Araçlar oluşturma            |make<br> maven<br> npm<br> Pip         |
|Kapsayıcılar             |[Docker Makinesi](https://github.com/docker/machine)<br> [Kubectl](https://kubernetes.io/docs/user-guide/kubectl-overview/)<br> [Dümen](https://github.com/kubernetes/helm)<br> [DC/OS CLI](https://github.com/dcos/dcos-cli)         |
|Veritabanları              |MySQL istemcisi<br> PostgreSql istemcisi<br> [sqlcmd Yardımcı Programı](https://docs.microsoft.com/sql/tools/sqlcmd-utility)<br> [mssql-scripter](https://github.com/Microsoft/sql-xplat-cli) |
|Diğer                  |iPython İstemci<br> [Bulut Döküm CLI](https://github.com/cloudfoundry/cli)<br> [Terraform](https://www.terraform.io/docs/providers/azurerm/)<br> [Ansible](https://www.ansible.com/microsoft-azure)<br> [Şef InSpec](https://www.chef.io/inspec/)<br> [Kukla Cıvata](https://puppet.com/docs/bolt/latest/bolt.html)<br> [HashiCorp Packer](https://www.packer.io/)|

## <a name="language-support"></a>Dil desteği

|Dil   |Sürüm   |
|---|---|
|.NET Core  |2.2.402       |
|Başlayın         |1.9        |
|Java       |1,8        |
|Node.js    |8.16.0      |
|PowerShell |[7.0.0](https://github.com/PowerShell/powershell/releases)       |
|Python     |2.7 ve 3.5 (varsayılan)|

## <a name="next-steps"></a>Sonraki adımlar
[Bulut Kabuk Quickstart Bash](quickstart.md) <br>
[PowerShell Bulut Shell Quickstart](quickstart-powershell.md) <br>
[Azure CLI hakkında bilgi edinin](https://docs.microsoft.com/cli/azure/) <br>
[Azure PowerShell hakkında bilgi edinin](https://docs.microsoft.com/powershell/azure/) <br>
