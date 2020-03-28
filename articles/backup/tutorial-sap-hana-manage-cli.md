---
title: "Öğretici: CLI kullanarak yedeklenmiş SAP HANA DB'yi yönetin"
description: Bu eğitimde, Azure CLI'yi kullanarak Azure VM'de çalışan yedeklenmiş SAP HANA veritabanlarını nasıl yöneteceğimiz öğrenin.
ms.topic: tutorial
ms.date: 12/4/2019
ms.openlocfilehash: 2c088c27a678a4541cbba3c4c43c9cd830c60ff0
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79238744"
---
# <a name="tutorial-manage-sap-hana-databases-in-an-azure-vm-using-azure-cli"></a>Öğretici: Azure CLI kullanarak BIR Azure VM'de SAP HANA veritabanlarını yönetme

Azure CLI, Komut Satırı'ndan veya komut dosyaları aracılığıyla Azure kaynaklarını oluşturmak ve yönetmek için kullanılır. Bu dokümantasyon, Azure VM'de yedeklenmiş bir SAP HANA veritabanını nasıl yöneteceğimizin ayrıntılarını ve tümü Azure CLI'yi kullanır. Bu adımları [Azure portalını](https://docs.microsoft.com/azure/backup/sap-hana-db-manage)kullanarak da gerçekleştirebilirsiniz.

CLI komutlarını çalıştırmak için [Azure Bulut Kabuğu'nu](tutorial-sap-hana-backup-cli.md) kullanın.

Bu eğitimin sonunda şunları yapabileceksiniz:

> [!div class="checklist"]
>
> * Yedeklemeyi izleme ve işleri geri yükleme
> * SAP HANA örneğine eklenen yeni veritabanlarını koruma
> * İlkeyi değiştirme
> * Korumayı durdurma
> * Özgeçmiş koruması

SAP HANA veritabanınızı yedeklemek için [CLI'yi kullanarak Azure'da bir SAP HANA veritabanını yedeklemeyi](tutorial-sap-hana-backup-cli.md) kullandıysanız, aşağıdaki kaynakları kullanıyorsunuz demektir:

* *saphanaResourceGroup* adlı bir kaynak grubu
* *saphanaVault* adlı bir kasa
* VMAppContainer adlı korumalı *konteyner; Bilgi işlem;saphanaResourceGroup;saphanaVM*
* saphanadatabase adlı yedekli *veritabanı/öğe;hxe;hxe*
* *westus2* bölgesindeki kaynaklar

Azure CLI, Azure Yedekleme'yi kullanarak yedeklenen bir Azure VM'de çalışan bir SAP HANA veritabanını yönetmeyi kolaylaştırır. Bu öğretici, yönetim işlemlerinin her birini ayrıntılarıyla anlatır.

## <a name="monitor-backup-and-restore-jobs"></a>Yedeklemeyi izleme ve işleri geri yükleme

Tamamlanan veya şu anda çalışan işleri izlemek için (yedekleme veya geri yükleme), [az yedekleme iş listesi](https://docs.microsoft.com/cli/azure/backup/job?view=azure-cli-latest#az-backup-job-list) cmdlet kullanın. CLI [ayrıca, şu anda çalışan bir işi askıya](https://docs.microsoft.com/cli/azure/backup/job?view=azure-cli-latest#az-backup-job-stop) almanızı veya bir iş [tamamlanana kadar beklemenizi](https://docs.microsoft.com/cli/azure/backup/job?view=azure-cli-latest#az-backup-job-wait)sağlar.

```azurecli-interactive
az backup job list --resource-group saphanaResourceGroup \
    --vault-name saphanaVault \
    --output table
```

Çıkış şuna benzer olacaktır:

```output
Name                                  Operation              Status      Item Name       Start Time UTC
------------------------------------  ---------------        ---------   ----------      -------------------  
e0f15dae-7cac-4475-a833-f52c50e5b6c3  ConfigureBackup        Completed   hxe             2019-12-03T03:09:210831+00:00  
ccdb4dce-8b15-47c5-8c46-b0985352238f  Backup (Full)          Completed   hxe [hxehost]   2019-12-01T10:30:58.867489+00:00
4980af91-1090-49a6-ab96-13bc905a5282  Backup (Differential)  Completed   hxe [hxehost]   2019-12-01T10:36:00.563909+00:00
F7c68818-039f-4a0f-8d73-e0747e68a813  Restore (Log)          Completed   hxe [hxehost]   2019-12-03T05:44:51.081607+00:00
```

## <a name="change-policy"></a>İlkeyi değiştir

SAP HANA yedekleme yapılandırmasının altında yatan ilkeyi değiştirmek için [az yedekleme ilkesi kümesi](https://docs.microsoft.com/cli/azure/backup/policy?view=azure-cli-latest#az-backup-policy-set) cmdlet'i kullanın. Bu cmdlet'teki ad parametresi, ilkesini değiştirmek istediğimiz yedekleme maddesini ifade eder. Bu öğretici için, sap HANA veritabanı *saphanadatabase;hxe;hxe* politikasını yeni bir ilke *newsaphanaPolicy*ile değiştireceğiz. Yeni ilkeler az yedekleme [ilkesi cmdlet oluşturmak](https://docs.microsoft.com/cli/azure/backup/policy?view=azure-cli-latest#az-backup-policy-create) kullanılarak oluşturulabilir.

```azurecli-interactive
az backup item set policy --resource-group saphanaResourceGroup \
    --vault-name saphanaVault \
    --container-name VMAppContainer;Compute;saphanaResourceGroup;saphanaVM \
    --policy-name newsaphanaPolicy \
    --name saphanadatabase;hxe;hxe \
```

Çıkış aşağıdakine benzer olmalıdır:

```output
Name                                  Resource Group
------------------------------------- --------------
cb110094-9b15-4c55-ad45-6899200eb8dd  SAPHANA
```

## <a name="protect-new-databases-added-to-an-sap-hana-instance"></a>SAP HANA örneğine eklenen yeni veritabanlarını koruma

[Bir KURTARMA hizmetleri kasasına sap HANA örneğini kaydetmek,](tutorial-sap-hana-backup-cli.md#register-and-protect-the-sap-hana-instance) bu örnekteki tüm veritabanlarını otomatik olarak keşfeder.

Ancak, sap HANA örneğine daha sonra yeni veritabanları eklendiğinde, [az yedekleme korumalı öğe nin cmdlet'i başlatmasını](https://docs.microsoft.com/cli/azure/backup/protectable-item?view=azure-cli-latest#az-backup-protectable-item-initialize) kullanın. Bu cmdlet eklenen yeni veritabanları keşfeder.

```azurecli-interactive
az backup protectable-item initialize --resource-group saphanaResourceGroup \
    --vault-name saphanaVault \
    --container-name VMAppContainer;Compute;saphanaResourceGroup;saphanaVM \
    --workload-type SAPHANA
```

Ardından, SAP HANA örneğiniz de bulunan tüm veritabanlarını listelemek için [az yedekleme korumalı öğe listesi](https://docs.microsoft.com/cli/azure/backup/protectable-item?view=azure-cli-latest#az-backup-protectable-item-list) cmdlet'i kullanın. Ancak bu liste, yedeklemenin zaten yapılandırıldığı veritabanlarını hariç tutar. Yedeklenecek veritabanı keşfedildikten sonra SAP [HANA veritabanında yedeklemeyi etkinleştir'e](tutorial-sap-hana-backup-cli.md#enable-backup-on-sap-hana-database)bakın.

```azurecli-interactive
az backup protectable-item list --resource-group saphanaResourceGroup \
    --vault-name saphanaVault \
    --workload-type SAPHANA \
    --output table
```

Yedeklemek istediğiniz yeni veritabanı aşağıdaki gibi görünecektir bu listede görünür:

```output
Name                            Protectable Item Type    ParentName    ServerName    IsProtected
---------------------------     ----------------------   ------------  -----------   ------------
saphanasystem;hxe               SAPHanaSystem            HXE           hxehost       NotProtected  
saphanadatabase;hxe;systemdb    SAPHanaDatabase          HXE           hxehost       NotProtected
saphanadatabase;hxe;newhxe      SAPHanaDatabase          HXE           hxehost       NotProtected
```

## <a name="stop-protection-for-an-sap-hana-database"></a>SAP HANA veritabanı için korumayı durdurma

SAP HANA veritabanını birkaç şekilde korumayı durdurabilirsiniz:

* Gelecekteki tüm yedekleme işlerini durdurun ve tüm kurtarma noktalarını silin.
* Gelecekteki tüm yedekleme işlerini durdurun ve kurtarma noktalarını sağlam bırakın.

Kurtarma noktalarını bırakmayı seçerseniz, şu ayrıntıları aklınızda bulundurun:

* Tüm kurtarma noktaları sonsuza kadar bozulmadan kalacak, tüm budama veri korumak ile durdurma koruma duracaktır.
* Korunan örnek ve tüketilen depolama için ücretlendirilirsiniz.
* Yedeklemeleri durdurmadan bir veri kaynağını silerseniz, yeni yedeklemeler başarısız olur.

Korumayı durdurmanın yollarının her birine daha ayrıntılı olarak bakalım.

### <a name="stop-protection-with-retain-data"></a>Verileri bekleterek korumayı durdurun

Korumayı tutma verileriyle durdurmak için cmdlet'i [devre dışı bırakarak az yedekleme korumasını](https://docs.microsoft.com/cli/azure/backup/protection?view=azure-cli-latest#az-backup-protection-disable) kullanın.

```azurecli-interactive
az backup protection disable --resource-group saphanaResourceGroup \
    --vault-name saphanaVault \
    --container-name VMAppContainer;Compute;saphanaResourceGroup;saphanaVM \
    --item-name saphanadatabase;hxe;hxe \
    --workload-type SAPHANA \
    --output table
```

Çıkış aşağıdakine benzer olmalıdır:

```output
Name                                  ResourceGroup
------------------------------------  ---------------  
g0f15dae-7cac-4475-d833-f52c50e5b6c3  saphanaResourceGroup
```

Bu işlemin durumunu kontrol etmek için [az yedekleme iş gösterisi](https://docs.microsoft.com/cli/azure/backup/job?view=azure-cli-latest#az-backup-job-show) cmdlet kullanın.

### <a name="stop-protection-without-retain-data"></a>Verileri saklamadan korumayı durdurma

Verileri saklamadan korumayı durdurmak için cmdlet'i [devre dışı bırak.](https://docs.microsoft.com/cli/azure/backup/protection?view=azure-cli-latest#az-backup-protection-disable)

```azurecli-interactive
az backup protection disable --resource-group saphanaResourceGroup \
    --vault-name saphanaVault \
    --container-name VMAppContainer;Compute;saphanaResourceGroup;saphanaVM \
    --item-name saphanadatabase;hxe;hxe \
    --workload-type SAPHANA \
    --delete-backup-data true \
    --output table
```

Çıkış aşağıdakine benzer olmalıdır:

```output
Name                                  ResourceGroup
------------------------------------  ---------------  
g0f15dae-7cac-4475-d833-f52c50e5b6c3  saphanaResourceGroup
```

Bu işlemin durumunu kontrol etmek için [az yedekleme iş gösterisi](https://docs.microsoft.com/cli/azure/backup/job?view=azure-cli-latest#az-backup-job-show) cmdlet kullanın.

## <a name="resume-protection"></a>Özgeçmiş koruması

SAP HANA veritabanının korumasını tutma verileriyle durdurunca, daha sonra korumaya devam edebilirsiniz. Yedeklenen verileri saklamazsanız, korumaya devam edemezsiniz.

Korumaya devam etmek için [az yedekleme koruması devam](https://docs.microsoft.com/cli/azure/backup/protection?view=azure-cli-latest#az-backup-protection-resume) cmdlet'ini kullanın.

```azurecli-interactive
az backup protection resume --resource-group saphanaResourceGroup \
    --vault-name saphanaVault \
    --container-name VMAppContainer;Compute;saphanaResourceGroup;saphanaVM \
    --policy-name saphanaPolicy \
    --output table
```

Çıkış aşağıdakine benzer olmalıdır:

```output
Name                                  ResourceGroup
------------------------------------  ---------------  
b2a7f108-1020-4529-870f-6c4c43e2bb9e  saphanaResourceGroup
```

Bu işlemin durumunu kontrol etmek için [az yedekleme iş gösterisi](https://docs.microsoft.com/cli/azure/backup/job?view=azure-cli-latest#az-backup-job-show) cmdlet kullanın.

## <a name="next-steps"></a>Sonraki adımlar

* Azure portalını kullanarak Azure VM'de çalışan bir SAP HANA veritabanını nasıl yedekleyeceğimi öğrenmek için [Azure VM'lerinde Yedekleme SAP HANA veritabanlarına](https://docs.microsoft.com/azure/backup/backup-azure-sap-hana-database) bakın

* Azure portalını kullanarak Azure VM'de çalışan yedeklenmiş BIR SAP HANA veritabanını nasıl yöneteceğimize ilişkin bilgi edinmek için Azure [VM'deki Yedeklenmiş SAP HANA veritabanlarını yönet'e](https://docs.microsoft.com/azure/backup/sap-hana-db-manage) bakın
