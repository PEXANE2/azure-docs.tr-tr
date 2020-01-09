---
title: Öğretici-CLı kullanarak Azure 'da DB geri yükleme SAP HANA
description: Bu öğreticide, Azure CLı kullanarak bir Azure Backup kurtarma hizmetleri kasasından Azure VM üzerinde çalışan SAP HANA veritabanlarını geri yüklemeyi öğrenin.
ms.topic: tutorial
ms.date: 12/4/2019
ms.openlocfilehash: 6dbe0c4382b648506d853feb281c70a8e8401595
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75472075"
---
# <a name="tutorial-restore-sap-hana-databases-in-an-azure-vm-using-azure-cli"></a>Öğretici: Azure CLı kullanarak Azure VM 'de SAP HANA veritabanlarını geri yükleme

Azure CLı, komut satırından veya betiklerden Azure kaynakları oluşturmak ve yönetmek için kullanılır. Bu belgelerde, Azure CLı kullanarak bir Azure VM 'de yedeklenmiş SAP HANA veritabanını geri yükleme ayrıntıları yer alır. Ayrıca, [Azure Portal](https://docs.microsoft.com/azure/backup/sap-hana-db-restore)kullanarak da bu adımları uygulayabilirsiniz.

CLı komutlarını çalıştırmak için [Azure Cloud Shell](tutorial-sap-hana-backup-cli.md) kullanın.

Bu öğreticinin sonuna kadar şunları yapabilirsiniz:

> [!div class="checklist"]
>
> * Yedeklenen bir veritabanının geri yükleme noktalarını görüntüleme
> * Veritabanını geri yükleme

Bu öğreticide, Azure Backup kullanılarak yedeklenen Azure VM 'de çalışan bir SAP HANA veritabanınız olduğunu varsaymaktadır. SAP HANA veritabanınızı yedeklemek için [CLI kullanarak Azure 'da bir SAP HANA veritabanı yedeklemeyi](tutorial-sap-hana-backup-cli.md) kullandıysanız aşağıdaki kaynakları kullanıyorsunuz:

* *Saphanaresourcegroup* adlı bir kaynak grubu
* *Saphanavault* adlı bir kasa
* *Vmappcontainer adlı korumalı kapsayıcı; İşlem; saphanaResourceGroup; saphanaVM*
* saphanadatabase adlı yedeklenen veritabanı/öğe *; hxe; hxe*
* *westus2* bölgesindeki kaynaklar

## <a name="view-restore-points-for-a-backed-up-database"></a>Yedeklenen bir veritabanının geri yükleme noktalarını görüntüleme

Bir veritabanı için tüm kurtarma noktalarının listesini görüntülemek için [az Backup RecoveryPoint List](https://docs.microsoft.com/cli/azure/backup/recoverypoint?view=azure-cli-latest#az-backup-recoverypoint-show-log-chain) cmdlet 'ini aşağıdaki gibi kullanın:

```azurecli-interactive
az backup recoverypoint list --resource-group saphanaResourceGroup \
    --vault-name saphanaVault \
    --container-name VMAppContainer;Compute;saphanaResourceGroup;saphanaVM \
    --item-name saphanadatabase;hxe;hxe \
   --output table
```

Kurtarma noktalarının listesi aşağıdaki gibi görünür:

```output
Name                      Time                               BackupManagementType   Item Name               RecoveryPointType
-------------------       ---------------------------------  ---------------------  ----------------------  ------------------
7660777527047692711       2019-12-10T04:00:32.346000+00:00   AzureWorkload          SAPHanaDtabase;hxe;hxe  Full
7896624824685666836       2019-12-15T10:33:32.346000+00:00   AzureWorkload          SAPHanaDtabase;hxe;hxe  Differential
DefaultRangeRecoveryPoint                                    AzureWorkload          SAPHanaDtabase;hxe;hxe  Log
```

Gördüğünüz gibi, yukarıdaki listede üç kurtarma noktası bulunur: her biri tam, fark ve günlük yedeklemesi için.

>[!NOTE]
>[Az Backup RecoveryPoint Show-log-zincir](https://docs.microsoft.com/cli/azure/backup/recoverypoint?view=azure-cli-latest#az-backup-recoverypoint-show-log-chain) cmdlet 'ini kullanarak, tüm kopuk günlük yedekleme zincirlerinin başlangıç ve bitiş noktalarını da görüntüleyebilirsiniz.

## <a name="prerequisites-to-restore-a-database"></a>Veritabanını geri yükleme önkoşulları

Bir veritabanını geri yüklemeden önce aşağıdaki önkoşulların karşılandığından emin olun:

* Veritabanını yalnızca aynı bölgedeki bir SAP HANA örneğine geri yükleyebilirsiniz
* Hedef örnek, kaynakla aynı kasada kayıtlı olmalıdır
* Azure Backup aynı VM 'de iki farklı SAP HANA örneği tanımlayamıyor. Bu nedenle, verileri aynı VM 'de bir örnekten diğerine geri yüklemek mümkün değildir.

## <a name="restore-a-database"></a>Veritabanını geri yükleme

Azure Backup, Azure VM 'lerinde çalışan SAP HANA veritabanlarını şu şekilde geri yükleyebilir:

* Günlük yedeklemeleri kullanarak belirli bir tarih veya saate (ikinci olarak) geri yükleyin. Azure Backup, uygun tam, değişiklik yedeklemelerini ve seçilen saate göre geri yüklemek için gereken günlük yedeklemeleri zincirini otomatik olarak belirler.
* Belirli bir kurtarma noktasına geri yüklemek için belirli bir tam veya değişiklik yedeklemesine geri yükleme yapın.

Bir veritabanını geri yüklemek için, girdilerden biri olarak bir kurtarma yapılandırma nesnesi gerektiren [az restore restore-azurewl](https://docs.microsoft.com/cli/azure/backup/restore?view=azure-cli-latest#az-backup-restore-restore-azurewl) cmdlet 'ini kullanın. Bu nesne [az Backup recoveryconfig Show](https://docs.microsoft.com/cli/azure/backup/recoveryconfig?view=azure-cli-latest#az-backup-recoveryconfig-show) cmdlet 'i kullanılarak oluşturulabilir. Kurtarma yapılandırma nesnesi, geri yükleme gerçekleştirmek için tüm ayrıntıları içerir. Bunlardan biri geri yükleme modu: **originalworkloadresdeposu** veya **alternateworkloadresrestore**.

>[!NOTE]
> **Originalworkloadresstore** -verileri özgün kaynakla aynı SAP HANA örneğine geri yükleyin. Bu seçenek özgün veritabanının üzerine yazar. <br>
> **Alternateworkloadresstore** -veritabanını alternatif bir konuma geri yükleyin ve özgün kaynak veritabanını koruyun.

## <a name="restore-to-alternate-location"></a>Alternatif konuma geri yükle

Bir veritabanını alternatif bir konuma geri yüklemek için, geri yükleme modu olarak **Alternateworkloadresdeposu** ' nu kullanın. Ardından, önceki bir nokta veya önceki geri yükleme noktalarından biri olabilecek geri yükleme noktasını seçmeniz gerekir.

Bu öğreticide, önceki bir geri yükleme noktasına geri yükleyeceksiniz. Veritabanının [geri yükleme noktalarının listesini görüntüleyin](#view-restore-points-for-a-backed-up-database) ve geri yüklemek istediğiniz noktayı seçin. Bu öğretici, geri yükleme noktasını *7660777527047692711*adıyla kullanacaktır.

Yukarıdaki geri yükleme noktası adını ve geri yükleme modunu kullanarak, [az Backup recoveryconfig Show](https://docs.microsoft.com/cli/azure/backup/recoveryconfig?view=azure-cli-latest#az-backup-recoveryconfig-show) cmdlet 'ini kullanarak kurtarma yapılandırma nesnesini oluşturalım. Bu cmdlet 'teki kalan parametrelerin her birinin ne anlama geldiğini inceleyelim:

* **--Target-öğe-adı** Bu, geri yüklenen veritabanının kullanacağı addır. Bu durumda *restored_database*adı kullandık.
* **--target-sunucu-adı** Bu, bir kurtarma hizmetleri kasasında başarıyla kaydedilen ve geri yüklenecek veritabanıyla aynı bölgede yer alan bir SAP HANA sunucusunun adıdır. Bu öğreticide, veritabanını, *hxehost*adlı, koruduğumuz SAP HANA sunucuya geri yükleyeceğiz.
* **--target-sunucu türü** SAP HANA veritabanlarının geri yüklenmesi için **Saphanadatabase** kullanılması gerekir.

```azurecli-interactive

az backup recoveryconfig show --resource-group saphanaResourceGroup \
    --vault-name saphanaVault \
    --container-name VMAppContainer;Compute;saphanaResourceGroup;saphanaVM \
    --item-name saphanadatabase;hxe;hxe \
    --restore-mode AlternateWorkloadRestore \
    --rp-name 7660777527047692711 \
    --target-item-name restored_database \
    --target-server-name hxehost \
    --target-server-type HANAInstance \
    --workload-type SAPHANA \
    --output json
```

Yukarıdaki sorguya olan yanıt, şuna benzer bir kurtarma yapılandırma nesnesi olacaktır:

```output
"{\"restore_mode\": \"OriginalLocation\", \"container_uri\": \" VMAppContainer;Compute;saphanaResourceGroup;saphanaVM \", \"item_uri\": \"SAPHanaDatabase;hxe;hxe\", \"recovery_point_id\": \"DefaultRangeRecoveryPoint\", \"log_point_in_time\": \"28-11-2019-09:53:00\", \"item_type\": \"SAPHana\", \"source_resource_id\": \"/subscriptions/ef4ab5a7-c2c0-4304-af80-af49f48af3d1/resourceGroups/saphanaResourceGroup/providers/Microsoft.Compute/virtualMachines/saphanavm\", \"database_name\": null, \"container_id\": null, \"alternate_directory_paths\": null}"
```

Şimdi veritabanını geri yüklemek için [az restore restore-azurewl](https://docs.microsoft.com/cli/azure/backup/restore?view=azure-cli-latest#az-backup-restore-restore-azurewl) cmdlet 'ini çalıştırın. Bu komutu kullanmak için, *recoveryconfig. JSON*adlı bir dosyaya kaydedilen yukarıdaki JSON çıkışını girmeniz gerekir.

```azurecli-interactive
az backup restore restore-azurewl --resource-group saphanaResourceGroup \
    --vault-name saphanaVault \
    --restore-config recoveryconfig.json \
    --output table
```

Çıktı şöyle görünür:

```output
Name                                  Resource
------------------------------------  -------
5b198508-9712-43df-844b-977e5dfc30ea  SAPHANA
```

Yanıt size iş adı verecektir. Bu iş adı [az Backup Job Show](https://docs.microsoft.com/cli/azure/backup/job?view=azure-cli-latest#az-backup-job-show) cmdlet 'ini kullanarak iş durumunu izlemek için kullanılabilir.

## <a name="restore-and-overwrite"></a>Geri yükleme ve üzerine yazma

Özgün konuma geri yüklemek için geri yükleme modu olarak **OrignialWorkloadRestore** kullanacağız. Ardından, önceki bir nokta veya önceki geri yükleme noktalarından biri olabilecek geri yükleme noktasını seçmeniz gerekir.

Bu öğreticide, geri yüklemek için önceki "28-11-2019-09:53:00" zaman noktasını seçeceğiz. Bu geri yükleme noktasını şu biçimlerde sağlayabilirsiniz: gg-aa-yyyy, gg-aa-yyyy-HH: mm: ss. Geri yüklenecek geçerli bir zaman noktası seçmek için, bozulan günlük zinciri yedeklerinin aralıklarını listeleyen [az Backup RecoveryPoint Show-log-zincir](https://docs.microsoft.com/cli/azure/backup/recoverypoint?view=azure-cli-latest#az-backup-recoverypoint-show-log-chain) cmdlet 'ini kullanın.

```azurecli-interactive
az backup recoveryconfig show --resource-group saphanaResourceGroup \
    --vault-name saphanaVault \
    --container-name VMAppContainer;Compute;saphanaResourceGroup;saphanaVM \
    --item-name saphanadatabase;hxe;hxe \
    --restore-mode OriginalWorkloadRestore \
    --log-point-in-time 28-11-2019-09:53:00 \
    --output json
```

Yukarıdaki sorguya olan yanıt, aşağıdaki gibi görünen bir kurtarma yapılandırma nesnesi olacaktır:

```output
"{\"restore_mode\": \"OriginalLocation\", \"container_uri\": \" VMAppContainer;Compute;saphanaResourceGroup;saphanaVM \", \"item_uri\": \"SAPHanaDatabase;hxe;hxe\", \"recovery_point_id\": \"DefaultRangeRecoveryPoint\", \"log_point_in_time\": \"28-11-2019-09:53:00\", \"item_type\": \"SAPHana\", \"source_resource_id\": \"/subscriptions/ef4ab5a7-c2c0-4304-af80-af49f48af3d1/resourceGroups/saphanaResourceGroup/providers/Microsoft.Compute/virtualMachines/saphanavm\", \"database_name\": null, \"container_id\": null, \"alternate_directory_paths\": null}"
```

Şimdi veritabanını geri yüklemek için [az restore restore-azurewl](https://docs.microsoft.com/cli/azure/backup/restore?view=azure-cli-latest#az-backup-restore-restore-azurewl) cmdlet 'ini çalıştırın. Bu komutu kullanmak için, *recoveryconfig. JSON*adlı bir dosyaya kaydedilen yukarıdaki JSON çıkışını girmeniz gerekir.

```azurecli-interactive
az backup restore restore-azurewl --resource-group saphanaResourceGroup \
    --vault-name saphanaVault \
    --restore-config recoveryconfig.json \
    --output table
```

Çıktı şöyle görünür:

```output
Name                                  Resource
------------------------------------  --------
5b198508-9712-43df-844b-977e5dfc30ea  SAPHANA
```

Yanıt size iş adı verecektir. Bu iş adı, [az Backup Job Show](https://docs.microsoft.com/cli/azure/backup/job?view=azure-cli-latest#az-backup-job-show) cmdlet 'ini kullanarak iş durumunu izlemek için kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar

* Azure CLı kullanarak yedeklenen SAP HANA veritabanlarının nasıl yönetileceğini öğrenmek için, [CLI kullanarak Azure VM 'de SAP HANA veritabanını yönetme](tutorial-sap-hana-backup-cli.md) öğreticisine devam edin

* Azure VM 'de çalışan bir SAP HANA veritabanını Azure portal kullanarak geri yükleme hakkında bilgi edinmek için bkz. [Azure VM 'lerinde SAP HANA veritabanlarını geri yükleme](https://docs.microsoft.com/azure/backup/sap-hana-db-restore)
