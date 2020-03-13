---
title: 'Öğretici: CLı kullanarak yedeklenen SAP HANA DB yönetme'
description: Bu öğreticide, Azure CLı kullanarak bir Azure VM üzerinde çalışan yedeklenen SAP HANA veritabanlarını yönetmeyi öğrenin.
ms.topic: tutorial
ms.date: 12/4/2019
ms.openlocfilehash: 2c088c27a678a4541cbba3c4c43c9cd830c60ff0
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79238744"
---
# <a name="tutorial-manage-sap-hana-databases-in-an-azure-vm-using-azure-cli"></a>Öğretici: Azure CLı kullanarak bir Azure VM 'de SAP HANA veritabanlarını yönetme

Azure CLı, komut satırından veya betiklerden Azure kaynakları oluşturmak ve yönetmek için kullanılır. Bu belgelerde, Azure CLı kullanarak Azure VM 'de yedeklenmiş SAP HANA veritabanının nasıl yönetileceği anlatılmaktadır. Ayrıca [, Azure Portal](https://docs.microsoft.com/azure/backup/sap-hana-db-manage)kullanarak da bu adımları uygulayabilirsiniz.

CLı komutlarını çalıştırmak için [Azure Cloud Shell](tutorial-sap-hana-backup-cli.md) kullanın.

Bu öğreticinin sonunda şunları yapabileceksiniz:

> [!div class="checklist"]
>
> * Yedekleme ve geri yükleme işlerini izleme
> * SAP HANA örneğine eklenen yeni veritabanlarını koruma
> * İlkeyi değiştirme
> * Korumayı Durdur
> * Korumayı sürdürür

SAP HANA veritabanınızı yedeklemek için [CLI kullanarak Azure 'da bir SAP HANA veritabanı yedeklemeyi](tutorial-sap-hana-backup-cli.md) kullandıysanız aşağıdaki kaynakları kullanıyorsunuz:

* *Saphanaresourcegroup* adlı bir kaynak grubu
* *Saphanavault* adlı bir kasa
* *Vmappcontainer adlı korumalı kapsayıcı; İşlem; saphanaResourceGroup; saphanaVM*
* saphanadatabase adlı yedeklenen veritabanı/öğe *; hxe; hxe*
* *westus2* bölgesindeki kaynaklar

Azure CLı, Azure Backup kullanılarak yedeklenen bir Azure VM üzerinde çalışan bir SAP HANA veritabanının yönetilmesini kolaylaştırır. Bu öğretici yönetim işlemlerinin her biri için ayrıntılar.

## <a name="monitor-backup-and-restore-jobs"></a>Yedekleme ve geri yükleme işlerini izleme

Tamamlanan veya çalışmakta olan işleri (yedekleme veya geri yükleme) izlemek için [az Backup Job List](https://docs.microsoft.com/cli/azure/backup/job?view=azure-cli-latest#az-backup-job-list) cmdlet 'ini kullanın. CLı, çalışmakta [olan bir işi askıya](https://docs.microsoft.com/cli/azure/backup/job?view=azure-cli-latest#az-backup-job-stop) almanıza veya [bir iş tamamlanana kadar beklemeniz](https://docs.microsoft.com/cli/azure/backup/job?view=azure-cli-latest#az-backup-job-wait)de sağlar.

```azurecli-interactive
az backup job list --resource-group saphanaResourceGroup \
    --vault-name saphanaVault \
    --output table
```

Çıktı şuna benzer şekilde görünecektir:

```output
Name                                  Operation              Status      Item Name       Start Time UTC
------------------------------------  ---------------        ---------   ----------      -------------------  
e0f15dae-7cac-4475-a833-f52c50e5b6c3  ConfigureBackup        Completed   hxe             2019-12-03T03:09:210831+00:00  
ccdb4dce-8b15-47c5-8c46-b0985352238f  Backup (Full)          Completed   hxe [hxehost]   2019-12-01T10:30:58.867489+00:00
4980af91-1090-49a6-ab96-13bc905a5282  Backup (Differential)  Completed   hxe [hxehost]   2019-12-01T10:36:00.563909+00:00
F7c68818-039f-4a0f-8d73-e0747e68a813  Restore (Log)          Completed   hxe [hxehost]   2019-12-03T05:44:51.081607+00:00
```

## <a name="change-policy"></a>İlkeyi Değiştir

SAP HANA yedekleme yapılandırmasını temel alan ilkeyi değiştirmek için [az Backup Policy set](https://docs.microsoft.com/cli/azure/backup/policy?view=azure-cli-latest#az-backup-policy-set) cmdlet 'ini kullanın. Bu cmdlet 'teki name parametresi, ilkesi değiştirmek istediğimiz yedekleme öğesini ifade eder. Bu öğretici için, yeni bir *Newsaphanapolicy*ilkesi ile SAP HANA veritabanı *saphanadatabase; hxe; hxe* ilkesini değiştirmektir. Yeni ilkeler, [az Backup Policy Create](https://docs.microsoft.com/cli/azure/backup/policy?view=azure-cli-latest#az-backup-policy-create) cmdlet 'i kullanılarak oluşturulabilir.

```azurecli-interactive
az backup item set policy --resource-group saphanaResourceGroup \
    --vault-name saphanaVault \
    --container-name VMAppContainer;Compute;saphanaResourceGroup;saphanaVM \
    --policy-name newsaphanaPolicy \
    --name saphanadatabase;hxe;hxe \
```

Çıktı şöyle görünmelidir:

```output
Name                                  Resource Group
------------------------------------- --------------
cb110094-9b15-4c55-ad45-6899200eb8dd  SAPHANA
```

## <a name="protect-new-databases-added-to-an-sap-hana-instance"></a>SAP HANA örneğine eklenen yeni veritabanlarını koruma

Bir [SAP HANA örneğini kurtarma hizmetleri Kasası Ile kaydetme](tutorial-sap-hana-backup-cli.md#register-and-protect-the-sap-hana-instance) , bu örnekteki tüm veritabanlarını otomatik olarak bulur.

Bununla birlikte, yeni veritabanlarının SAP HANA örneğine daha sonra Eklenme durumunda [az Backup korunabilir-Item Initialize](https://docs.microsoft.com/cli/azure/backup/protectable-item?view=azure-cli-latest#az-backup-protectable-item-initialize) cmdlet 'ini kullanın. Bu cmdlet, eklenen yeni veritabanlarını bulur.

```azurecli-interactive
az backup protectable-item initialize --resource-group saphanaResourceGroup \
    --vault-name saphanaVault \
    --container-name VMAppContainer;Compute;saphanaResourceGroup;saphanaVM \
    --workload-type SAPHANA
```

Daha sonra, SAP HANA örneğiniz üzerinde bulunan tüm veritabanlarını listelemek için [az Backup korunabilir-Item List](https://docs.microsoft.com/cli/azure/backup/protectable-item?view=azure-cli-latest#az-backup-protectable-item-list) cmdlet 'ini kullanın. Ancak bu liste, yedeklemenin zaten yapılandırıldığı veritabanlarını dışlar. Yedeklenecek veritabanı bulunduğunda, [SAP HANA veritabanında yedeklemeyi etkinleştir](tutorial-sap-hana-backup-cli.md#enable-backup-on-sap-hana-database)' e başvurun.

```azurecli-interactive
az backup protectable-item list --resource-group saphanaResourceGroup \
    --vault-name saphanaVault \
    --workload-type SAPHANA \
    --output table
```

Yedeklemek istediğiniz yeni veritabanı bu listede görünür ve bu, şöyle görünür:

```output
Name                            Protectable Item Type    ParentName    ServerName    IsProtected
---------------------------     ----------------------   ------------  -----------   ------------
saphanasystem;hxe               SAPHanaSystem            HXE           hxehost       NotProtected  
saphanadatabase;hxe;systemdb    SAPHanaDatabase          HXE           hxehost       NotProtected
saphanadatabase;hxe;newhxe      SAPHanaDatabase          HXE           hxehost       NotProtected
```

## <a name="stop-protection-for-an-sap-hana-database"></a>SAP HANA veritabanı korumasını durdurma

SAP HANA veritabanının korunmasını birkaç yolla durdurabilirsiniz:

* Gelecekteki tüm yedekleme işlerini durdurun ve tüm kurtarma noktalarını silin.
* Gelecekteki tüm yedekleme işlerini durdurun ve kurtarma noktalarını bozulmadan bırakın.

Kurtarma noktalarından ayrıldığınızda bu ayrıntıları göz önünde bulundurun:

* Tüm kurtarma noktaları süresiz olarak kalır, tüm ayıklama, verileri sakla ile korumayı durdurma sırasında durdurulur.
* Korumalı örnek ve tüketilen depolama alanı için ücretlendirilirsiniz.
* Yedeklemeleri durdurmadan bir veri kaynağını silerseniz, yeni yedeklemeler başarısız olur.

Daha ayrıntılı bir şekilde korumayı durdurmak için her bir yola göz atalım.

### <a name="stop-protection-with-retain-data"></a>Verileri bekleterek korumayı durdurun

Verileri bekletme ile korumayı durdurmak için [az Backup Protection Disable](https://docs.microsoft.com/cli/azure/backup/protection?view=azure-cli-latest#az-backup-protection-disable) cmdlet 'ini kullanın.

```azurecli-interactive
az backup protection disable --resource-group saphanaResourceGroup \
    --vault-name saphanaVault \
    --container-name VMAppContainer;Compute;saphanaResourceGroup;saphanaVM \
    --item-name saphanadatabase;hxe;hxe \
    --workload-type SAPHANA \
    --output table
```

Çıktı şöyle görünmelidir:

```output
Name                                  ResourceGroup
------------------------------------  ---------------  
g0f15dae-7cac-4475-d833-f52c50e5b6c3  saphanaResourceGroup
```

Bu işlemin durumunu denetlemek için [az Backup Job Show](https://docs.microsoft.com/cli/azure/backup/job?view=azure-cli-latest#az-backup-job-show) cmdlet 'ini kullanın.

### <a name="stop-protection-without-retain-data"></a>Verileri saklamadan korumayı durdur

Verileri saklamadan korumayı durdurmak için [az Backup Protection Disable](https://docs.microsoft.com/cli/azure/backup/protection?view=azure-cli-latest#az-backup-protection-disable) cmdlet 'ini kullanın.

```azurecli-interactive
az backup protection disable --resource-group saphanaResourceGroup \
    --vault-name saphanaVault \
    --container-name VMAppContainer;Compute;saphanaResourceGroup;saphanaVM \
    --item-name saphanadatabase;hxe;hxe \
    --workload-type SAPHANA \
    --delete-backup-data true \
    --output table
```

Çıktı şöyle görünmelidir:

```output
Name                                  ResourceGroup
------------------------------------  ---------------  
g0f15dae-7cac-4475-d833-f52c50e5b6c3  saphanaResourceGroup
```

Bu işlemin durumunu denetlemek için [az Backup Job Show](https://docs.microsoft.com/cli/azure/backup/job?view=azure-cli-latest#az-backup-job-show) cmdlet 'ini kullanın.

## <a name="resume-protection"></a>Korumayı sürdürür

Verileri sakla SAP HANA veritabanının korumasını durdurduğunuzda, daha sonra korumayı sürdürebilirsiniz. Yedeklenen verileri korumazsanız korumaya devam edemeyeceksiniz.

Korumayı yeniden başlatmak için [az Backup Protection özgeçmişi](https://docs.microsoft.com/cli/azure/backup/protection?view=azure-cli-latest#az-backup-protection-resume) cmdlet 'ini kullanın.

```azurecli-interactive
az backup protection resume --resource-group saphanaResourceGroup \
    --vault-name saphanaVault \
    --container-name VMAppContainer;Compute;saphanaResourceGroup;saphanaVM \
    --policy-name saphanaPolicy \
    --output table
```

Çıktı şöyle görünmelidir:

```output
Name                                  ResourceGroup
------------------------------------  ---------------  
b2a7f108-1020-4529-870f-6c4c43e2bb9e  saphanaResourceGroup
```

Bu işlemin durumunu denetlemek için [az Backup Job Show](https://docs.microsoft.com/cli/azure/backup/job?view=azure-cli-latest#az-backup-job-show) cmdlet 'ini kullanın.

## <a name="next-steps"></a>Sonraki adımlar

* Azure portal kullanarak Azure VM 'de çalışan bir SAP HANA veritabanını nasıl yedekleyeceğinizi öğrenmek için bkz. [Azure VM 'Lerde yedekleme SAP HANA veritabanları](https://docs.microsoft.com/azure/backup/backup-azure-sap-hana-database)

* Azure portal kullanarak Azure VM 'de çalışan yedeklenmiş SAP HANA bir veritabanını yönetmeyi öğrenmek için bkz. [Azure VM 'de yedeklenen SAP HANA veritabanlarını yönetme](https://docs.microsoft.com/azure/backup/sap-hana-db-manage)
