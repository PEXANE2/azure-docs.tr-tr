---
title: Öğretici-CLı kullanarak Azure 'da DB yedeklemesini SAP HANA
description: Bu öğreticide, Azure CLı kullanarak bir Azure VM üzerinde çalışan SAP HANA veritabanlarını Azure Backup kurtarma hizmetleri kasasına nasıl yedekleyeceğinizi öğrenin.
ms.topic: tutorial
ms.date: 12/4/2019
ms.custom: devx-track-azurecli
ms.openlocfilehash: 505c4502281f4d32e1e28d913ed5a040e533cfb2
ms.sourcegitcommit: ac7ae29773faaa6b1f7836868565517cd48561b2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/25/2020
ms.locfileid: "88824330"
---
# <a name="tutorial-back-up-sap-hana-databases-in-an-azure-vm-using-azure-cli"></a>Öğretici: Azure CLı kullanarak Azure VM 'de SAP HANA veritabanlarını yedekleme

Azure CLı, komut satırından veya betiklerden Azure kaynakları oluşturmak ve yönetmek için kullanılır. Bu belgelerde, Azure CLı 'yi kullanarak bir SAP HANA veritabanını yedekleme ve isteğe bağlı yedeklemeleri tetikleme ayrıntıları yer alır. Ayrıca, [Azure Portal](./backup-azure-sap-hana-database.md)kullanarak da bu adımları uygulayabilirsiniz.

Bu belgede zaten bir Azure VM 'de yüklü bir SAP HANA veritabanınız olduğunu varsaymaktadır. (Ayrıca, [Azure CLI kullanarak BIR VM oluşturabilirsiniz](../virtual-machines/linux/quick-create-cli.md)). Bu öğreticinin sonunda şunları yapabileceksiniz:

> [!div class="checklist"]
>
> * Kurtarma Hizmetleri kasası oluşturma
> * SAP HANA örneğini kaydedin ve üzerinde veritabanlarını bulun
> * SAP HANA veritabanında yedeklemeyi etkinleştir
> * İsteğe bağlı yedekleme tetikleyin

[Şu anda SAP HANA için desteklediğimiz senaryolara](./sap-hana-backup-support-matrix.md#scenario-support) göz atın.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLı 'yi yerel olarak yüklemek ve kullanmak için Azure CLı sürüm xx. xxx. x veya üstünü çalıştırmanız gerekir. CLI sürümünü bulmak için şunu çalıştırın: `az --version`. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme](/cli/azure/install-azure-cli).

## <a name="create-a-recovery-services-vault"></a>Kurtarma Hizmetleri kasası oluşturma

Kurtarma Hizmetleri Kasası, Azure VM 'Leri ya da SQL veya HANA veritabanları gibi Azure VM 'lerde çalışan iş yükleri gibi korunan her kaynak için yedekleme verilerini depolayan bir mantıksal kapsayıcıdır. Koruma altındaki bir kaynak için yedekleme işi çalıştığında Kurtarma Hizmetleri kasasının içinde bir kurtarma noktası oluşturulur. Daha sonra bu kurtarma noktalarından birini kullanarak verileri dilediğiniz zaman geri yükleyebilirsiniz.

[az backup vault create](/cli/azure/backup/vault#az-backup-vault-create) komutuyla bir Kurtarma Hizmetleri kasası oluşturun. Korumak istediğiniz sanal makineyle aynı kaynak grubunu ve konumu belirtin. Bu [VM hızlı](../virtual-machines/linux/quick-create-cli.md)başlangıcı Ile Azure CLI kullanarak sanal makine oluşturmayı öğrenin.

Bu öğreticide, aşağıdakileri kullanacağız:

* *Saphanaresourcegroup* adlı bir kaynak grubu
* *Saphanavm* ADLı bir VM
* *westus2* konumundaki kaynaklar.

*Saphanavault*adlı bir kasa oluşturacağız.

```azurecli-interactive
az backup vault create --resource-group saphanaResourceGroup \
    --name saphanaVault \
    --location westus2
```

Varsayılan olarak Kurtarma Hizmetleri kasasında Coğrafi Olarak Yedekli depolama özelliği etkindir. Coğrafi Olarak Yedekli depolama, yedeklenen verilerinizin birincil bölgeden yüzlerce kilometre uzaktaki ikincil bir Azure bölgesinde çoğaltılmasını sağlar. Depolama artıklığı ayarının değiştirilmesi gerekiyorsa, [az Backup Kasası Backup-Properties set](/cli/azure/backup/vault/backup-properties?view=azure-cli-latest#az-backup-vault-backup-properties-set) cmdlet 'ini kullanın.

```azurecli
az backup vault backup-properties set \
    --name saphanaVault  \
    --resource-group saphanaResourceGroup \
    --backup-storage-redundancy "LocallyRedundant/GeoRedundant"
```

Kasanızın başarıyla oluşturulduğunu görmek için [az Backup kasa listesi](/cli/azure/backup/vault?view=azure-cli-latest#az-backup-vault-list) cmdlet 'ini kullanın. Aşağıdaki yanıtı görürsünüz:

```output
Location   Name             ResourceGroup
---------  ---------------  -------------  
westus2    saphanaVault     saphanaResourceGroup
```

## <a name="register-and-protect-the-sap-hana-instance"></a>SAP HANA örneğini kaydetme ve koruma

Azure hizmetleri tarafından keşfedilecek SAP HANA örneği (üzerinde SAP HANA yüklü VM) için, SAP HANA makinesinde bir [ön kayıt betiği](https://aka.ms/scriptforpermsonhana) çalıştırılmalıdır. Betiği çalıştırmadan önce tüm [önkoşulların](./tutorial-backup-sap-hana-db.md#prerequisites) karşılandığından emin olun. Betiğin ne yaptığı hakkında daha fazla bilgi edinmek için, [ön kayıt betiğinin ne yaptığını](tutorial-backup-sap-hana-db.md#what-the-pre-registration-script-does) bölümüne bakın.

Betik çalıştırıldıktan sonra, SAP HANA örneği daha önce oluşturduğumuz kurtarma hizmetleri kasasıyla kaydedilebilir. Örneği kaydetmek için [az Backup Container Register](/cli/azure/backup/container?view=azure-cli-latest#az-backup-container-register) cmdlet 'ini kullanın. *Vmresourceıd* , SAP HANA yüklemek için oluşturduğunuz sanal makınenın kaynak kimliğidir.

```azurecli-interactive
az backup container register --resource-group saphanaResourceGroup \
    --vault-name saphanaVault \
    --location westus2 \
    --workload-type SAPHANA \
    --backup-management-type AzureWorkload \
    --resource-id VMResourceId
```

>[!NOTE]
>VM, kasala aynı kaynak grubunda değilse, *Saphanaresourcegroup* , kasanın oluşturulduğu kaynak grubunu belirtir.

SAP HANA örneğinin kaydedilmesi, tüm geçerli veritabanlarını otomatik olarak bulur. Bununla birlikte, gelecekte eklenebilecek yeni veritabanlarını bulmak için, [kayıtlı SAP HANA örneği bölümüne eklenen yeni veritabanlarının keşfedilmesine](tutorial-sap-hana-manage-cli.md#protect-new-databases-added-to-an-sap-hana-instance) bakın.

SAP HANA örneğinin kasaya başarıyla kaydedilip kaydedilmediği kontrol etmek için [az Backup Container List](/cli/azure/backup/container?view=azure-cli-latest#az-backup-container-list) cmdlet 'ini kullanın. Aşağıdaki yanıtı görürsünüz:

```output
Name                                                    Friendly Name    Resource Group        Type           Registration Status
------------------------------------------------------  --------------   --------------------  ---------      ----------------------
VMAppContainer;Compute;saphanaResourceGroup;saphanaVM   saphanaVM        saphanaResourceGroup  AzureWorkload  Registered
```

>[!NOTE]
> Yukarıdaki çıktıda "Name" sütunu, kapsayıcı adına başvurur. Bu kapsayıcı adı, yedeklemeleri etkinleştirmek ve onları tetiklemek için sonraki bölümlerde kullanılacaktır. Bu durumda, *Vmappcontainer; Compute; saphanaResourceGroup; saphanaVM*.

## <a name="enable-backup-on-sap-hana-database"></a>SAP HANA veritabanında yedeklemeyi etkinleştir

[Az Backup korunabilir-Item List](/cli/azure/backup/protectable-item?view=azure-cli-latest#az-backup-protectable-item-list) cmdlet 'i, önceki adımda kaydettiğiniz SAP HANA örneğinde bulunan tüm veritabanlarını listeler.

```azurecli-interactive
az backup protectable-item list --resource-group saphanaResourceGroup \
    --vault-name saphanaVault \
    --workload-type SAPHANA \
    --output table
```

Bu listede yedeklemek istediğiniz veritabanını bulmanız gerekir, bu, aşağıdaki gibi görünür:

```output
Name                           Protectable Item Type    ParentName    ServerName    IsProtected
-----------------------------  ----------------------   ------------  -----------   ------------
saphanasystem;hxe              SAPHanaSystem            HXE           hxehost       NotProtected  
saphanadatabase;hxe;systemdb   SAPHanaDatabase          HXE           hxehost       NotProtected
saphanadatabase;hxe;hxe        SAPHanaDatabase          HXE           hxehost       NotProtected
```

Yukarıdaki çıktıdan görebileceğiniz gibi SAP HANA sisteminin SID 'SI HXE olur. Bu öğreticide, *hxehost* sunucusunda bulunan *saphanadatabase; hxe; hxe* veritabanı için yedeklemeyi yapılandıracağız.

Tek seferde bir veritabanında yedeklemeyi korumak ve yapılandırmak için [az Backup Protection Enable-for-azurewl](/cli/azure/backup/protection?view=azure-cli-latest#az-backup-protection-enable-for-azurewl) cmdlet 'ini kullanırız. Kullanmak istediğiniz ilkenin adını belirtin. CLı kullanarak ilke oluşturmak için [az Backup Policy Create](/cli/azure/backup/policy?view=azure-cli-latest#az-backup-policy-create) cmdlet 'ini kullanın. Bu öğreticide, *Sapahanapolicy* ilkesini kullanacağız.

```azurecli-interactive
az backup protection enable-for-azurewl --resource-group saphanaResourceGroup \
    --policy-name saphanaPolicy \
    --protectable-item-name saphanadatabase;hxe;hxe  \
    --protectable-item-type SAPHANADatabse \
    --server-name hxehost \
    --workload-type SAPHANA \
    --output table
```

Yukarıdaki yedekleme yapılandırmasının, [az Backup Job List](/cli/azure/backup/job?view=azure-cli-latest#az-backup-job-list) cmdlet 'ini kullanarak tamamlandığını kontrol edebilirsiniz. Çıktı aşağıdaki gibi görüntülenir:

```output
Name                                  Operation         Status     Item Name   Start Time UTC
------------------------------------  ---------------   ---------  ----------  -------------------  
e0f15dae-7cac-4475-a833-f52c50e5b6c3  ConfigureBackup   Completed  hxe         2019-12-03T03:09:210831+00:00  
```

[Az Backup Job List](/cli/azure/backup/job?view=azure-cli-latest#az-backup-job-list) cmdlet 'i, korunan veritabanında çalışan veya çalışmakta olan tüm yedekleme işlerini (zamanlanmış veya isteğe bağlı) listeler; Kaydet, yedeklemeyi yapılandırma, yedekleme verilerini silme gibi diğer işlemlere ek olarak.

>[!NOTE]
>Azure Backup, bir Azure VM 'de çalışan bir SAP HANA Veritabanının yedeklenmesinde gün ışığından yararlanma saati değişikliklerini otomatik olarak ayarlamaz.
>
>İlkeyi gerektiği şekilde el ile değiştirin.

## <a name="trigger-an-on-demand-backup"></a>İsteğe bağlı yedekleme tetikleyin

Yukarıdaki bölümde, zamanlanmış bir yedeklemenin nasıl yapılandırılacağı ayrıntılandığında, bu bölüm isteğe bağlı bir yedeklemenin tetiklenmesi hakkında konuşur. Bunu yapmak için [az Backup Protection Backup-Now](/cli/azure/backup/protection#az-backup-protection-backup-now) cmdlet 'ini kullanırız.

>[!NOTE]
> İsteğe bağlı bir yedeklemenin bekletme ilkesi, veritabanı için temeldeki bekletme ilkesine göre belirlenir.

```azurecli-interactive
az backup protection backup-now --resource-group saphanaResourceGroup \
    --item-name saphanadatabase;hxe;hxe \
    --vault-name saphanaVault \
    --container-name VMAppContainer;Compute;saphanaResourceGroup;saphanaVM \
    --backup-type Full
    --retain-until 01-01-2040
    --output table
```

Çıktı aşağıdaki gibi görüntülenir:

```output
Name                                  ResourceGroup
------------------------------------  -------------
e0f15dae-7cac-4475-a833-f52c50e5b6c3  saphanaResourceGroup
```

Yanıt size iş adı verecektir. Bu iş adı, [az Backup Job Show](/cli/azure/backup/job?view=azure-cli-latest#az-backup-job-show) cmdlet 'ini kullanarak iş durumunu izlemek için kullanılabilir.

>[!NOTE]
>Tam veya değişiklik yedeklemesi zamanlamaya ek olarak, şu anda el ile de tetiklenebilir. Günlük yedeklemeleri, SAP HANA tarafından dahili olarak otomatik olarak tetiklenir ve yönetilir.
>
> Artımlı yedeklemeler Şu anda Azure Backup tarafından desteklenmemektedir.

## <a name="next-steps"></a>Sonraki adımlar

* CLı kullanarak Azure VM 'de bir SAP HANA veritabanını geri yükleme hakkında bilgi edinmek için bkz. [CLI kullanarak Azure VM 'de bir SAP HANA veritabanını geri yükleme](tutorial-sap-hana-restore-cli.md)

* Azure portal kullanarak Azure VM 'de çalışan bir SAP HANA veritabanını nasıl yedekleyeceğinizi öğrenmek için bkz. [Azure VM 'lerinde SAP HANA veritabanları yedekleme](./backup-azure-sap-hana-database.md)
