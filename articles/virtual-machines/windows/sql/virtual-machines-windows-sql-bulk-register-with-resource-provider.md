---
title: Azure'daki SQL sanal makinelerini SQL VM kaynak sağlayıcısına toplu olarak kaydettirin | Microsoft Dokümanlar
description: Yönetilebilirliği artırmak için SQL VM kaynak sağlayıcısına SQL Server VM'leri toplu olarak kaydedin.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
tags: azure-resource-manager
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 10/21/2019
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: 015aa4c209a99921a930a51b15c3d0230722519a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75353880"
---
# <a name="bulk-register-sql-virtual-machines-in-azure-with-the-sql-vm-resource-provider"></a>Azure'daki SQL sanal makinelerini SQL VM kaynak sağlayıcısına toplu olarak kaydettirin

Bu makalede, `Register-SqlVMs` PowerShell cmdlet'i kullanarak SQL VM kaynak sağlayıcısına Azure'da SQL Server sanal makinenizi (VM) toplu olarak nasıl kaydedebilirsiniz.

Cmdlet, `Register-SqlVMs` tüm sanal makineleri belirli bir abonelik, kaynak grupları veya belirli sanal makinelerin listesine kaydetmek için kullanılabilir. Cmdlet, sanal makineleri _hafif_ yönetim modunda kaydeder ve ardından hem rapor hem de [günlük dosyası](#output-description)oluşturur. 

Kayıt işlemi hiçbir risk taşımaz, kesinti süresi yoktur ve SQL Server'ı veya sanal makineyi yeniden başlatmaz. 

Kaynak sağlayıcısı hakkında daha fazla bilgi için [SQL VM kaynak sağlayıcısına](virtual-machines-windows-sql-register-with-resource-provider.md)bakın. 

## <a name="prerequisites"></a>Ön koşullar

SQL Server VM'nizi kaynak sağlayıcısına kaydetmek için aşağıdakilere ihtiyacınız olacaktır: 

- [Kaynak sağlayıcısına kayıtlı](virtual-machines-windows-sql-register-with-resource-provider.md#register-subscription-with-rp) olan ve kayıtsız SQL Server sanal makineleri içeren bir [Azure aboneliği.](https://azure.microsoft.com/free/) 
- Sanal makineleri kaydetmek için kullanılan istemci kimlik bilgileri aşağıdaki RBAC rollerinden herhangi birinde bulunur: **Sanal Makine katılımcısı,** **Katkıda Bulunan**veya **Sahibi**. 
- [Az PowerShell](/powershell/azure/new-azureps-module-az)en son sürümü . 
- [Az.SqlVirtualMachine](https://www.powershellgallery.com/packages/Az.SqlVirtualMachine/0.1.0)en son sürümü .

## <a name="getting-started"></a>Başlarken

İşleme geçmeden önce komut dosyasının yerel bir kopyasını oluşturmanız, PowerShell modülü olarak almanız ve Azure'a bağlanmanız gerekir. 

### <a name="create-script"></a>Komut dosyası oluşturma

Komut dosyasını oluşturmak için, bu makalenin sonundaki tam komut `RegisterSqlVMs.psm1` [dosyasını](#full-script) kopyalayın ve yerel olarak '' olarak kaydedin. 

### <a name="import-script"></a>Komut dosyası alma

Komut dosyası oluşturulduktan sonra, powershell terminalinde bir modül olarak içe aktarabilirsiniz. 

Yönetimkurulu PowerShell terminalini açın ve `RegisterSqlVMs.psm1` dosyayı kaydettiğiniz yere gidin. Ardından, komut dosyasını modül olarak almak için aşağıdaki PowerShell cmdlet'i çalıştırın: 

```powershell-interactive
Import-Module .\RegisterSqlVMs.psm1
```

### <a name="connect-to-azure"></a>Azure'a Bağlanma

Azure'a bağlanmak için aşağıdaki PowerShell cmdlet'i kullanın:

```powershell-interactive
Connect-AzAccount
```


## <a name="all-vms-in-list-of-subscriptions"></a>Aboneliklistesindeki tüm VM'ler 

Tüm SQL Server sanal makinelerini abonelik listesine kaydetmek için aşağıdaki cmdlet'i kullanın:

```powershell-interactive
Register-SqlVMs -SubscriptionList SubscriptionId1,SubscriptionId2
```

Örnek çıktı: 

```
Number of Subscriptions registration failed for 
because you do not have access or credentials are wrong: 1
Total VMs Found: 10
VMs Already registered: 1
Number of VMs registered successfully: 4
Number of VMs failed to register due to error: 1
Number of VMs skipped as VM or the guest agent on VM is not running: 3
Number of VMs skipped as they are not running SQL Server On Windows: 1

Please find the detailed report in file RegisterSqlVMScriptReport1571314821.txt
Please find the error details in file VMsNotRegisteredDueToError1571314821.log
```

## <a name="all-vms-in-a-single-subscription"></a>Tek bir abonelikteki tüm VM'ler

Tüm SQL Server sanal makinelerini tek bir abonelikte kaydetmek için aşağıdaki cmdlet'i kullanın: 

```powershell-interactive
Register-SqlVMs -Subscription SubscriptionId1
```

Örnek çıktı:

```
Total VMs Found: 10
VMs Already registered: 1
Number of VMs registered successfully: 5
Number of VMs failed to register due to error: 1
Number of VMs skipped as VM or the  guest agent on VM is not running: 2
Number of VMs skipped as they are not running SQL Server On Windows: 1

Please find the detailed report in file RegisterSqlVMScriptReport1571314821.txt
Please find the error details in file VMsNotRegisteredDueToError1571314821.log
```

## <a name="all-vms-in-multiple-resource-groups"></a>Birden çok kaynak grubundaki tüm VM'ler

Tüm SQL Server sanal makinelerini tek bir abonelik içinde birden çok kaynak grubuna kaydetmek için aşağıdaki cmdlet'i kullanın:

```powershell-interactive
Register-SqlVMs -Subscription SubscriptionId1 -ResourceGroupList ResourceGroup1,ResourceGroup2
```

Örnek çıktı:

```
Total VMs Found: 4
VMs Already registered: 1
Number of VMs registered successfully: 1
Number of VMs failed to register due to error: 1
Number of VMs skipped as they are not running SQL Server On Windows: 1

Please find the detailed report in file RegisterSqlVMScriptReport1571314821.txt
Please find the error details in file VMsNotRegisteredDueToError1571314821.log
```

## <a name="all-vms-in-a-resource-group"></a>Kaynak grubundaki tüm VM'ler

Tüm SQL Server sanal makinelerini tek bir kaynak grubuna kaydetmek için aşağıdaki cmdlet'i kullanın: 

```powershell-interactive
Register-SqlVMs -Subscription SubscriptionId1 -ResourceGroupName ResourceGroup1
```

Örnek çıktı:

```
Total VMs Found: 4
VMs Already registered: 1
Number of VMs registered successfully: 1
Number of VMs failed to register due to error: 1
Number of VMs skipped as VM or the guest agent on VM is not running: 1

Please find the detailed report in file RegisterSqlVMScriptReport1571314821.txt
Please find the error details in file VMsNotRegisteredDueToError1571314821.log
```

## <a name="specific-vms-in-single-resource-group"></a>Tek kaynak grubundaki özel VM'ler

Belirli SQL Server sanal makinelerini tek bir kaynak grubuna kaydetmek için aşağıdaki cmdlet'i kullanın:

```powershell-interactive
Register-SqlVMs -Subscription SubscriptionId1 -ResourceGroupName ResourceGroup1 -VmList VM1,VM2,VM3
```

Örnek çıktı:

```
Total VMs Found: 3
VMs Already registered: 0
Number of VMs registered successfully: 1
Number of VMs skipped as VM or the guest agent on VM is not running: 1
Number of VMs skipped as they are not running SQL Server On Windows: 1

Please find the detailed report in file RegisterSqlVMScriptReport1571314821.txt
Please find the error details in file VMsNotRegisteredDueToError1571314821.log
```

## <a name="specific-vm"></a>Özel VM

Belirli bir SQL Server sanal makinesini kaydetmek için aşağıdaki cmdlet'i kullanın: 

```powershell-interactive
Register-SqlVMs -Subscription SubscriptionId1 -ResourceGroupName ResourceGroup1 -Name VM1
```

Örnek çıktı:

```
Total VMs Found: 1
VMs Already registered: 0
Number of VMs registered successfully: 1

Please find the detailed report in  file RegisterSqlVMScriptReport1571314821.txt
```


## <a name="output-description"></a>Çıktı açıklaması

`Register-SqlVMs` Cmdlet her kullanıldığında hem rapor hem de günlük dosyası oluşturulur. 

### <a name="report"></a>Rapor

Rapor, zaman damgasının `RegisterSqlVMScriptReport<Timestamp>.txt` cmdlet'in başlatıldığı zaman olduğu yerde adlı bir `.txt` dosya olarak oluşturulur. Rapor aşağıdaki ayrıntıları listeler:

| **Çıkış değeri** | **Açıklama** |
| :--------------  | :-------------- | 
| Erişiminiz veya kimlik bilgileriniz yanlış olduğu için abonelik kaydı nın başarısız olduğu sayısı | Bu, sağlanan kimlik doğrulamasıyla ilgili sorunları olan aboneliklerin numarasını ve listesini sağlar. Ayrıntılı hata abonelik kimliği arayarak günlük bulunabilir. | 
| RP'ye kayıtlı olmadıkları için denenmeyen abonelik sayısı | Bu bölümde, SQL VM kaynak sağlayıcısına kaydedilmemiş aboneliklerin sayısı ve listesi yer almaktadır. |
| Bulunan toplam VM | Parametreler kapsamında bulunan sanal makinelerin sayısı cmdlet'e geçti. | 
| VM'ler zaten kayıtlı | Kaynak sağlayıcısına zaten kayıtlı oldukları için atlanan sanal makinelerin sayısı. |
| Başarıyla kaydedilmiş VM sayısı | Cmdlet çalıştırıldıktan sonra başarıyla kaydedilmiş sanal makinelerin sayısı. Kayıtlı sanal makineleri biçimde `SubscriptionID, Resource Group, Virtual Machine`listeler. | 
| VM sayısı hata nedeniyle kaydonok başarısız oldu | Bazı hatalar nedeniyle kaydolamayan sanal makinelerin sayısı. Hatanın ayrıntıları günlük dosyasında bulunabilir. | 
| VM veya VM üzerindeki gust ajanı çalışmadığı için atlanan VM sayısı | Sanal makine veya sanal makinedeki konuk ajan olarak kaydedilen sanal makinelerin sayısı ve listesi çalışmıyordu. Bunlar, sanal makine veya konuk aracı başlatıldıktan sonra yeniden denenebilir. Ayrıntılar günlük dosyasında bulunabilir. |
| Windows'da SQL Server çalıştırılmamadıkları için atlanan VM sayısı | SQL Server'ı çalıştırmıyor veya Windows sanal makinesi olmadıkları için atlanan sanal makinelerin sayısı. Sanal makineler biçiminde `SubscriptionID, Resource Group, Virtual Machine`listelenir. | 
| &nbsp; | &nbsp; |

### <a name="log"></a>Günlük 

Hatalar, zaman damgasının komut `VMsNotRegisteredDueToError<Timestamp>.log` dosyasının başlatıldığı saat olduğu günlük dosyasına kaydedilir. Hata abonelik düzeyindeyse, günlük virgülle ayrılmış SubscriptionID ve hata iletisini içerir. Hata sanal makine kaydıyla ilgiliyse, günlük Abonelik Kimliği, Kaynak grup adı, sanal makine adı, hata kodu ve virgülle ayrılmış ileti içerir. 

## <a name="remarks"></a>Açıklamalar

Sağlanan komut dosyasını kullanarak SQL Server VM'lerini kaynak sağlayıcısına kaydettirdiğinizde aşağıdakileri göz önünde bulundurun:

- Kaynak sağlayıcısına kaydolmak için SQL Server VM'de çalışan bir konuk aracı gerektirir. Windows Server 2008 görüntüleri bir konuk aracısı yok, bu nedenle bu sanal makineler başarısız olacak ve [NoAgent yönetim modu](virtual-machines-windows-sql-register-with-resource-provider.md#management-modes)kullanılarak el ile kaydedilmelidir.
- Saydam hataların üstesinden gelmek için yerleşik yeniden deneme mantığı vardır. Sanal makine başarıyla kaydedilirse, o zaman hızlı bir işlemdir. Ancak, kayıt başarısız olursa, her sanal makine yeniden denenecektir.  Bu nedenle, kayıt işlemini tamamlamak için önemli bir zaman ayırmanız gerekir - ancak gerçek zaman gereksinimi hataların türüne ve sayısına bağlıdır. 

## <a name="full-script"></a>Tam betik

GitHub'daki tam komut dosyası için, [Az PowerShell ile Birlikte SQL VM'leri toplu olarak kaydettirin.](https://github.com/Azure/azure-docs-powershell-samples/blob/master/sql-virtual-machine/register-sql-vms/RegisterSqlVMs.psm1) 

Tam komut dosyasını kopyalayın ve ' olarak `RegisterSqLVMs.psm1`kaydedin.

[!code-powershell-interactive[main](../../../../powershell_scripts/sql-virtual-machine/register-sql-vms/RegisterSqlVMs.psm1 "Bulk register SQL Server virtual machines")]

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için aşağıdaki makalelere bakın: 

* [Windows VM'de SQL Server'a Genel Bakış](virtual-machines-windows-sql-server-iaas-overview.md)
* [Windows VM'de SQL Server için SSS](virtual-machines-windows-sql-server-iaas-faq.md)
* [Windows VM'de SQL Server için fiyatlandırma kılavuzu](virtual-machines-windows-sql-server-pricing-guidance.md)
* [Windows VM'de SQL Server için sürüm notları](virtual-machines-windows-sql-server-iaas-release-notes.md)
