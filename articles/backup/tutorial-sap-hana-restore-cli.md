---
title: Öğretici - CLI kullanarak Azure'da SAP HANA DB geri yüklemesi
description: Bu eğitimde, Azure CLI'yi kullanarak Azure Yedekleme Kurtarma Hizmetleri kasasından Azure VM'de çalışan SAP HANA veritabanlarını nasıl geri yükleyebilirsiniz öğrenin.
ms.topic: tutorial
ms.date: 12/4/2019
ms.openlocfilehash: 6dbe0c4382b648506d853feb281c70a8e8401595
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "75472075"
---
# <a name="tutorial-restore-sap-hana-databases-in-an-azure-vm-using-azure-cli"></a>Öğretici: Azure CLI kullanarak Bir Azure VM'de SAP HANA veritabanlarını geri yükleme

Azure CLI, komut satırından veya komut dosyaları aracılığıyla Azure kaynaklarını oluşturmak ve yönetmek için kullanılır. Bu belge, Azure CLI'yi kullanarak azure vm'de yedeklenmiş bir SAP HANA veritabanını nasıl geri yükleyeceğimiz hakkında ayrıntılı bilgi sağlar. Bu adımları [Azure portalını](https://docs.microsoft.com/azure/backup/sap-hana-db-restore)kullanarak da gerçekleştirebilirsiniz.

CLI komutlarını çalıştırmak için [Azure Bulut Kabuğu'nu](tutorial-sap-hana-backup-cli.md) kullanın.

Bu eğitimin sonunda şunları yapabileceksiniz:

> [!div class="checklist"]
>
> * Yedeklenmiş bir veritabanı için geri yükleme noktalarını görüntüleme
> * Veritabanını geri yükleme

Bu öğretici, Azure VM'de çalışan ve Azure Yedekleme'yi kullanarak yedeklenen bir SAP HANA veritabanınız olduğunu varsayar. SAP HANA veritabanınızı yedeklemek için [CLI'yi kullanarak Azure'da bir SAP HANA veritabanını yedeklemeyi](tutorial-sap-hana-backup-cli.md) kullandıysanız, aşağıdaki kaynakları kullanıyorsunuz demektir:

* *saphanaResourceGroup* adlı bir kaynak grubu
* *saphanaVault* adlı bir kasa
* VMAppContainer adlı korumalı *konteyner; Bilgi işlem;saphanaResourceGroup;saphanaVM*
* saphanadatabase adlı yedekli *veritabanı/öğe;hxe;hxe*
* *westus2* bölgesindeki kaynaklar

## <a name="view-restore-points-for-a-backed-up-database"></a>Yedeklenmiş bir veritabanı için geri yükleme noktalarını görüntüleme

Bir veritabanı için tüm kurtarma noktalarının listesini görüntülemek [için, az yedekleme kurtarma noktası listesini](https://docs.microsoft.com/cli/azure/backup/recoverypoint?view=azure-cli-latest#az-backup-recoverypoint-show-log-chain) cmdlet aşağıdaki gibi kullanın:

```azurecli-interactive
az backup recoverypoint list --resource-group saphanaResourceGroup \
    --vault-name saphanaVault \
    --container-name VMAppContainer;Compute;saphanaResourceGroup;saphanaVM \
    --item-name saphanadatabase;hxe;hxe \
   --output table
```

Kurtarma noktaları listesi aşağıdaki gibi görünecektir:

```output
Name                      Time                               BackupManagementType   Item Name               RecoveryPointType
-------------------       ---------------------------------  ---------------------  ----------------------  ------------------
7660777527047692711       2019-12-10T04:00:32.346000+00:00   AzureWorkload          SAPHanaDtabase;hxe;hxe  Full
7896624824685666836       2019-12-15T10:33:32.346000+00:00   AzureWorkload          SAPHanaDtabase;hxe;hxe  Differential
DefaultRangeRecoveryPoint                                    AzureWorkload          SAPHanaDtabase;hxe;hxe  Log
```

Gördüğünüz gibi, yukarıdaki liste üç kurtarma noktası içerir: tam, diferansiyel ve günlük yedekleme için birer tane.

>[!NOTE]
>Ayrıca [az yedekleme recoverypoint show-log-chain](https://docs.microsoft.com/cli/azure/backup/recoverypoint?view=azure-cli-latest#az-backup-recoverypoint-show-log-chain) cmdlet kullanarak, her kırılmamış günlük yedekleme zincirinin başlangıç ve bitiş noktalarını görüntüleyebilirsiniz.

## <a name="prerequisites-to-restore-a-database"></a>Veritabanını geri yüklemek için ön koşullar

Veritabanını geri yüklemeden önce aşağıdaki ön koşulların karşılandığından emin olun:

* Veritabanını yalnızca aynı bölgede bulunan bir SAP HANA örneğine geri yükleyebilirsiniz
* Hedef örnek, kaynakla aynı kasaya kaydedilmelidir
* Azure Yedekleme aynı VM'de iki farklı SAP HANA örneğini tanımlayamaz. Bu nedenle, verileri aynı VM'de bir örnekten diğerine geri getirmek mümkün değildir.

## <a name="restore-a-database"></a>Veritabanını geri yükleme

Azure Yedekleme, Azure VM'lerinde çalışan SAP HANA veritabanlarını aşağıdaki gibi geri yükleyebilir:

* Günlük yedeklemelerini kullanarak belirli bir tarihe veya saate (ikinciye) geri yükleyin. Azure Yedekleme, seçilen süreye bağlı olarak geri yüklemesi gereken uygun tam, diferansiyel yedeklemeleri ve günlük yedeklemeleri zincirini otomatik olarak belirler.
* Belirli bir kurtarma noktasına geri yüklemek için belirli bir tam veya diferansiyel yedekleme geri yükleyin.

Bir veritabanını geri yüklemek için, girişlerden biri olarak kurtarma config nesnesi gerektiren [az geri yükleme-azurewl](https://docs.microsoft.com/cli/azure/backup/restore?view=azure-cli-latest#az-backup-restore-restore-azurewl) cmdlet'i kullanın. Bu nesne [az yedekleme config show](https://docs.microsoft.com/cli/azure/backup/recoveryconfig?view=azure-cli-latest#az-backup-recoveryconfig-show) cmdlet kullanılarak oluşturulabilir. Kurtarma config nesnesi geri yükleme gerçekleştirmek için tüm ayrıntıları içerir. Bunlardan biri geri yükleme modu olmak - **OriginalWorkloadRestore** veya **AlternateWorkloadRestore**.

>[!NOTE]
> **OriginalWorkloadRestore** - Verileri orijinal kaynakla aynı SAP HANA örneğine geri yükleyin. Bu seçenek özgün veritabanının üzerine yazar. <br>
> **AlternativeWorkloadRestore** - Veritabanını alternatif bir konuma geri yükleyin ve özgün kaynak veritabanını saklayın.

## <a name="restore-to-alternate-location"></a>Alternatif konuma geri yükleme

Veritabanını alternatif bir konuma geri yüklemek için geri yükleme modu olarak **Alternatif İş Yükü Geri Yükle'yi** kullanın. Daha sonra, önceki bir zaman noktası veya önceki geri yükleme noktalarından biri olabilecek geri yükleme noktasını seçmeniz gerekir.

Bu öğreticide, önceki bir geri yükleme noktasına geri yükleyin. Veritabanı [için geri yükleme noktaları listesini görüntüleyin](#view-restore-points-for-a-backed-up-database) ve geri yüklemek istediğiniz noktayı seçin. Bu öğretici adı *7660777527047692711*ile geri yükleme noktası kullanacaktır.

Yukarıdaki geri yükleme noktası adını ve geri yükleme modunu kullanarak, [az yedekleme config show](https://docs.microsoft.com/cli/azure/backup/recoveryconfig?view=azure-cli-latest#az-backup-recoveryconfig-show) cmdlet kullanarak kurtarma config nesnesini oluşturalım. Bu cmdlette kalan parametrelerin her birinin ne anlama geldiğini inceleyelim:

* **--hedef-öğe adı** Bu, geri yüklenen veritabanının kullanarak çalışacağı addır. Bu durumda, *restored_database*adını kullandık.
* **--hedef sunucu adı** Bu, kurtarma hizmetleri kasasına başarıyla kaydedilmiş ve geri yüklenecek veritabanıyla aynı bölgede bulunan bir SAP HANA sunucusunun adıdır. Bu öğretici için, biz *hxehost*adlı, koruduğumuz aynı SAP HANA sunucusuna veritabanı geri yükleyeceğiz.
* **--hedef sunucu türü** SAP HANA veritabanlarının geri yükedimi için **SapHanaDatabase** kullanılmalıdır.

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

Yukarıdaki sorguya yanıt şuna benzer bir kurtarma config nesnesi olacaktır:

```output
"{\"restore_mode\": \"OriginalLocation\", \"container_uri\": \" VMAppContainer;Compute;saphanaResourceGroup;saphanaVM \", \"item_uri\": \"SAPHanaDatabase;hxe;hxe\", \"recovery_point_id\": \"DefaultRangeRecoveryPoint\", \"log_point_in_time\": \"28-11-2019-09:53:00\", \"item_type\": \"SAPHana\", \"source_resource_id\": \"/subscriptions/ef4ab5a7-c2c0-4304-af80-af49f48af3d1/resourceGroups/saphanaResourceGroup/providers/Microsoft.Compute/virtualMachines/saphanavm\", \"database_name\": null, \"container_id\": null, \"alternate_directory_paths\": null}"
```

Şimdi, veritabanını geri yüklemek için [az geri yükleme-azurewl](https://docs.microsoft.com/cli/azure/backup/restore?view=azure-cli-latest#az-backup-restore-restore-azurewl) cmdlet çalıştırın. Bu komutu kullanmak için, *recoveryconfig.json*adlı bir dosyaya kaydedilen yukarıdaki json çıktısını gireceğiz.

```azurecli-interactive
az backup restore restore-azurewl --resource-group saphanaResourceGroup \
    --vault-name saphanaVault \
    --restore-config recoveryconfig.json \
    --output table
```

Çıktı şu şekilde görünecektir:

```output
Name                                  Resource
------------------------------------  -------
5b198508-9712-43df-844b-977e5dfc30ea  SAPHANA
```

Yanıt size iş adını verecektir. Bu iş adı az yedekleme iş [gösterisi](https://docs.microsoft.com/cli/azure/backup/job?view=azure-cli-latest#az-backup-job-show) cmdlet kullanarak iş durumunu izlemek için kullanılabilir.

## <a name="restore-and-overwrite"></a>Geri yükleme ve üzerine yazma

Özgün konuma geri yüklemek için **OrignialWorkloadRestore'i** geri yükleme modu olarak kullanırız. Daha sonra, önceki bir zaman noktası veya önceki geri yükleme noktalarından biri olabilecek geri yükleme noktasını seçmeniz gerekir.

Bu öğretici için, geri yüklemek için bir önceki nokta-in-time "28-11-2019-09:53:00" seçeceğiz. Bu geri yükleme noktasını aşağıdaki biçimlerde sağlayabilirsiniz: dd-mm-yyyy, dd-mm-yyyy-hh:mm:ss. Geri yüklemek için geçerli bir zaman noktası seçmek için, kırılmamış günlük zinciri yedekleme aralıklarını listeleyen [az yedekleme kurtarma noktası göster-günlük zinciri](https://docs.microsoft.com/cli/azure/backup/recoverypoint?view=azure-cli-latest#az-backup-recoverypoint-show-log-chain) cmdlet'i kullanın.

```azurecli-interactive
az backup recoveryconfig show --resource-group saphanaResourceGroup \
    --vault-name saphanaVault \
    --container-name VMAppContainer;Compute;saphanaResourceGroup;saphanaVM \
    --item-name saphanadatabase;hxe;hxe \
    --restore-mode OriginalWorkloadRestore \
    --log-point-in-time 28-11-2019-09:53:00 \
    --output json
```

Yukarıdaki sorguya yanıt aşağıdaki gibi görünen bir kurtarma config nesnesi olacaktır:

```output
"{\"restore_mode\": \"OriginalLocation\", \"container_uri\": \" VMAppContainer;Compute;saphanaResourceGroup;saphanaVM \", \"item_uri\": \"SAPHanaDatabase;hxe;hxe\", \"recovery_point_id\": \"DefaultRangeRecoveryPoint\", \"log_point_in_time\": \"28-11-2019-09:53:00\", \"item_type\": \"SAPHana\", \"source_resource_id\": \"/subscriptions/ef4ab5a7-c2c0-4304-af80-af49f48af3d1/resourceGroups/saphanaResourceGroup/providers/Microsoft.Compute/virtualMachines/saphanavm\", \"database_name\": null, \"container_id\": null, \"alternate_directory_paths\": null}"
```

Şimdi, veritabanını geri yüklemek için [az geri yükleme-azurewl](https://docs.microsoft.com/cli/azure/backup/restore?view=azure-cli-latest#az-backup-restore-restore-azurewl) cmdlet çalıştırın. Bu komutu kullanmak için, *recoveryconfig.json*adlı bir dosyaya kaydedilen yukarıdaki json çıktısını gireceğiz.

```azurecli-interactive
az backup restore restore-azurewl --resource-group saphanaResourceGroup \
    --vault-name saphanaVault \
    --restore-config recoveryconfig.json \
    --output table
```

Çıktı şu şekilde görünecektir:

```output
Name                                  Resource
------------------------------------  --------
5b198508-9712-43df-844b-977e5dfc30ea  SAPHANA
```

Yanıt size iş adını verecektir. Bu iş adı [az yedekleme iş gösterisi](https://docs.microsoft.com/cli/azure/backup/job?view=azure-cli-latest#az-backup-job-show) cmdlet kullanarak iş durumunu izlemek için kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar

* Azure CLI kullanılarak yedeklenen SAP HANA veritabanlarını nasıl yöneteceğimize dair bilgi edinmek için, [CLI kullanarak Azure VM'de SAP HANA veritabanını yönet'e](tutorial-sap-hana-backup-cli.md) okumaya devam edin

* Azure portalını kullanarak Azure VM'de çalışan bir SAP HANA veritabanını nasıl geri yükleyeceğimi öğrenmek için Azure [VM'lerde SAP HANA veritabanlarını geri yükleme'ye](https://docs.microsoft.com/azure/backup/sap-hana-db-restore) bakın
