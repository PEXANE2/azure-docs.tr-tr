---
title: Öğretici - CLI kullanarak Azure'da SAP HANA DB yedeklemesi
description: Bu eğitimde, Azure VM'de çalışan SAP HANA veritabanlarını Azure CLI'yi kullanarak Azure Yedekleme Kurtarma Hizmetleri kasasına nasıl yedekleyeceğimiz öğrenin.
ms.topic: tutorial
ms.date: 12/4/2019
ms.openlocfilehash: cdc8a8fb09a086a2b9212c21d071f267991fa275
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "78206631"
---
# <a name="tutorial-back-up-sap-hana-databases-in-an-azure-vm-using-azure-cli"></a>Öğretici: Azure CLI kullanarak SAP HANA veritabanlarını Azure VM'de yedekleme

Azure CLI, Komut Satırı'ndan veya komut dosyaları aracılığıyla Azure kaynaklarını oluşturmak ve yönetmek için kullanılır. Bu dokümantasyon, bir SAP HANA veritabanını nasıl yedekleyeceğimiz ve tümü Azure CLI kullanarak isteğe bağlı yedeklemeleri nasıl tetikleyeceğimiz hakkında ayrıntılı bilgi sağlar. Bu adımları [Azure portalını](https://docs.microsoft.com/azure/backup/backup-azure-sap-hana-database)kullanarak da gerçekleştirebilirsiniz.

Bu belge, Azure VM'de zaten yüklü bir SAP HANA veritabanınız olduğunu varsayar. (Azure [CLI'yi kullanarak bir VM](https://docs.microsoft.com/azure/virtual-machines/linux/quick-create-cli)de oluşturabilirsiniz). Bu eğitimin sonunda şunları yapabileceksiniz:

> [!div class="checklist"]
>
> * Kurtarma hizmetleri kasası oluşturma
> * SAP HANA örneğini kaydedin ve veritabanı(lar) üzerinde keşfedin
> * SAP HANA veritabanında yedeklemeyi etkinleştirme
> * İsteğe bağlı yedeklemeyi tetikleme

Şu anda SAP HANA için [desteklediğimiz senaryolara](https://docs.microsoft.com/azure/backup/sap-hana-backup-support-matrix#scenario-support) göz atın.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLI'yi yerel olarak yüklemek ve kullanmak için Azure CLI sürümünü xx.xxx.x veya daha sonra çalıştırmanız gerekir. CLI sürümünü bulmak için şunu çalıştırın: `az --version`. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli).

## <a name="create-a-recovery-services-vault"></a>Kurtarma hizmetleri kasası oluşturma

Kurtarma Hizmetleri kasası, Azure VM'leri veya SQL veya HANA veritabanları gibi Azure VM'lerde çalışan iş yükleri gibi her korumalı kaynağın yedekleme verilerini depolayan mantıksal bir kapsayıcıdır. Koruma altındaki bir kaynak için yedekleme işi çalıştığında Kurtarma Hizmetleri kasasının içinde bir kurtarma noktası oluşturulur. Daha sonra bu kurtarma noktalarından birini kullanarak verileri dilediğiniz zaman geri yükleyebilirsiniz.

[az backup vault create](https://docs.microsoft.com/cli/azure/backup/vault#az-backup-vault-create) komutuyla bir Kurtarma Hizmetleri kasası oluşturun. Korumak istediğiniz sanal makineyle aynı kaynak grubunu ve konumu belirtin. Bu [VM hızlı başlatma](https://docs.microsoft.com/azure/virtual-machines/linux/quick-create-cli)ile Azure CLI'yi kullanarak nasıl bir VM oluşturabilirsiniz öğrenin.

Bu öğretici için aşağıdakileri kullanacağız:

* *saphanaResourceGroup* adlı bir kaynak grubu
* *saphanaVM* adlı bir VM
* *westus2* konumunda kaynaklar.

*SaphanaVault*adında bir kasa yaratacağız.

```azurecli-interactive
az backup vault create --resource-group saphanaResourceGroup \
    --name saphanaVault \
    --location westus2
```

Varsayılan olarak Kurtarma Hizmetleri kasasında Coğrafi Olarak Yedekli depolama özelliği etkindir. Coğrafi Olarak Yedekli depolama, yedeklenen verilerinizin birincil bölgeden yüzlerce kilometre uzaktaki ikincil bir Azure bölgesinde çoğaltılmasını sağlar. Depolama artıklığı ayarını değiştirilmesi gerekiyorsa, az [yedekleme tonoz yedekleme özellikleri cmdlet ayarlayın](https://docs.microsoft.com/cli/azure/backup/vault/backup-properties?view=azure-cli-latest#az-backup-vault-backup-properties-set) kullanın.

```azurecli
az backup vault backup-properties set \
    --name saphanaVault  \
    --resource-group saphanaResourceGroup \
    --backup-storage-redundancy "LocallyRedundant/GeoRedundant"
```

Kasanızın başarıyla oluşturulıp oluşturulmadısını görmek için [az yedek kasa listesi](https://docs.microsoft.com/cli/azure/backup/vault?view=azure-cli-latest#az-backup-vault-list) cmdlet'i kullanın. Aşağıdaki yanıtı görürsünüz:

```output
Location   Name             ResourceGroup
---------  ---------------  -------------  
westus2    saphanaVault     saphanaResourceGroup
```

## <a name="register-and-protect-the-sap-hana-instance"></a>SAP HANA örneğini kaydedin ve koruyun

Azure hizmetleri tarafından keşfedilecek SAP HANA örneğinin (üzerinde SAP HANA yüklü VM) bulunması için SAP HANA makinesinde bir [ön kayıt komut dosyasının](https://aka.ms/scriptforpermsonhana) çalıştırılması gerekir. Komut dosyasını çalıştırmadan önce tüm [ön koşulların](https://docs.microsoft.com/azure/backup/tutorial-backup-sap-hana-db#prerequisites) karşılandıkktan emin olun. Komut dosyasının ne yaptığı hakkında daha fazla bilgi edinmek [için, ön kayıt komut dosyasının ne yaptığı bölümüne](tutorial-backup-sap-hana-db.md#what-the-pre-registration-script-does) bakın.

Komut dosyası çalıştırıladıktan sonra, SAP HANA örneği daha önce oluşturduğumuz kurtarma hizmetleri kasasına kaydedilebilir. Örneği kaydetmek için [az yedekleme kapsayıcı sıcağı cmdlet'i](https://docs.microsoft.com/cli/azure/backup/container?view=azure-cli-latest#az-backup-container-register) kullanın. *VMResourceId,* SAP HANA'yı yüklemek için oluşturduğunuz VM'nin kaynak kimliğidir.

```azurecli-interactive
az backup container register --resource-group saphanaResourceGroup \
    --vault-name saphanaVault \
    --location westus2 \
    --workload-type SAPHANA \
    --backup-management-type AzureWorkload \
    --resource-id VMResourceId
```

>[!NOTE]
>VM kasayla aynı kaynak grubunda değilse, *saphanaResourceGroup* kasanın oluşturulduğu kaynak grubunu ifade eder.

SAP HANA örneğini kaydetmek, tüm geçerli veritabanlarını otomatik olarak keşfeder. Ancak, gelecekte ekleyebilecek yeni veritabanlarını keşfetmek için kayıtlı SAP HANA örnek bölümüne [eklenen Yeni Veritabanlarını Bulma bölümüne](tutorial-sap-hana-manage-cli.md#protect-new-databases-added-to-an-sap-hana-instance) bakın.

SAP HANA örneğinin kasanıza başarıyla kaydedilip kaydedilemeip kaydedilemeylistesini kontrol etmek için [az yedekleme konteyner listesi](https://docs.microsoft.com/cli/azure/backup/container?view=azure-cli-latest#az-backup-container-list) cmdlet'i kullanın. Aşağıdaki yanıtı görürsünüz:

```output
Name                                                    Friendly Name    Resource Group        Type           Registration Status
------------------------------------------------------  --------------   --------------------  ---------      ----------------------
VMAppContainer;Compute;saphanaResourceGroup;saphanaVM   saphanaVM        saphanaResourceGroup  AzureWorkload  Registered
```

>[!NOTE]
> Yukarıdaki çıktıdaki "ad" sütunu kapsayıcı adını ifade eder. Bu kapsayıcı adı, yedeklemeleri etkinleştirmek ve tetiklemek için sonraki bölümlerde kullanılacaktır. Bu durumda, *VMAppContainer olduğunu; Bilgi işlem;saphanaResourceGroup;saphanaVM*.

## <a name="enable-backup-on-sap-hana-database"></a>SAP HANA veritabanında yedeklemeyi etkinleştirme

[Az yedekleme korumalı madde listesi](https://docs.microsoft.com/cli/azure/backup/protectable-item?view=azure-cli-latest#az-backup-protectable-item-list) cmdlet, önceki adımda kaydettiğiniz SAP HANA örneğinde bulunan tüm veritabanlarını listeler.

```azurecli-interactive
az backup protectable-item list --resource-group saphanaResourceGroup \
    --vault-name saphanaVault \
    --workload-type SAPHANA \
    --output table
```

Yedeklemek istediğiniz veritabanını bu listede aşağıdaki gibi görüneceğiniz şekilde bulmanız gerekir:

```output
Name                           Protectable Item Type    ParentName    ServerName    IsProtected
-----------------------------  ----------------------   ------------  -----------   ------------
saphanasystem;hxe              SAPHanaSystem            HXE           hxehost       NotProtected  
saphanadatabase;hxe;systemdb   SAPHanaDatabase          HXE           hxehost       NotProtected
saphanadatabase;hxe;hxe        SAPHanaDatabase          HXE           hxehost       NotProtected
```

Yukarıdaki çıktıdan da görebileceğiniz gibi SAP HANA sisteminin SID'si HXE'dir. Bu öğreticide, *saphanadatabase;hxe;hxe* veritabanı için yedeklemeyi yapılandıracağız ve *hxeserver* sunucusunda bulunur.

Yedeklemeyi bir veritabanında korumak ve yapılandırmak [için, az yedekleme koruma enable-for-azurewl](https://docs.microsoft.com/cli/azure/backup/protection?view=azure-cli-latest#az-backup-protection-enable-for-azurewl) cmdlet'i kullanırız. Kullanmak istediğiniz ilkenin adını sağlayın. CLI kullanarak bir ilke oluşturmak için [az yedekleme ilkesi oluşturmak](https://docs.microsoft.com//cli/azure/backup/policy?view=azure-cli-latest#az-backup-policy-create) cmdlet kullanın. Bu öğretici için, *sapahanaPolicy* politikasını kullanacağız.

```azurecli-interactive
az backup protection enable-for-azurewl --resource-group saphanaResourceGroup \
    --policy-name saphanaPolicy \
    --protectable-item-name saphanadatabase;hxe;hxe  \
    --protectable-item-type SAPHANADatabse \
    --server-name hxehost \
    --workload-type SAPHANA \
    --output table
```

Yukarıdaki yedekleme yapılandırması [az yedekleme iş listesi](https://docs.microsoft.com/cli/azure/backup/job?view=azure-cli-latest#az-backup-job-list) cmdlet kullanarak tamamlanmış olup olmadığını kontrol edebilirsiniz. Çıktı aşağıdaki gibi görüntülenir:

```output
Name                                  Operation         Status     Item Name   Start Time UTC
------------------------------------  ---------------   ---------  ----------  -------------------  
e0f15dae-7cac-4475-a833-f52c50e5b6c3  ConfigureBackup   Completed  hxe         2019-12-03T03:09:210831+00:00  
```

[Az yedekleme iş listesi](https://docs.microsoft.com/cli/azure/backup/job?view=azure-cli-latest#az-backup-job-list) cmdlet, kayıt, yapılandırma, yedekleme verilerini silme vb. gibi diğer işlemlere ek olarak, korumalı veritabanında çalışan veya şu anda çalışmakta olan tüm yedekleme işlerini (planlanmış veya isteğe bağlı) listeler.

## <a name="trigger-an-on-demand-backup"></a>İsteğe bağlı yedeklemeyi tetikleme

Yukarıdaki bölümde zamanlanmış bir yedeklemenin nasıl yapılandırılabildiğini ayrıntılarıyla anlatırken, bu bölüm isteğe bağlı yedeklemeyi tetiklemekten bahseder. Bunu yapmak için, [az yedekleme yedekleme-şimdi](https://docs.microsoft.com/cli/azure/backup/protection#az-backup-protection-backup-now) cmdlet kullanın.

>[!NOTE]
> İsteğe bağlı yedeklemenin bekletme ilkesi, veritabanı için temel bekletme ilkesi tarafından belirlenir.

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

Yanıt size iş adını verecektir. Bu iş adı [az yedekleme iş gösterisi](https://docs.microsoft.com/cli/azure/backup/job?view=azure-cli-latest#az-backup-job-show) cmdlet kullanarak iş durumunu izlemek için kullanılabilir.

>[!NOTE]
>Tam veya diferansiyel yedekleme zamanlamaya ek olarak, bunlar şu anda el ile de tetiklenebilir. Günlük yedeklemeleri otomatik olarak tetiklenir ve SAP HANA tarafından dahili olarak yönetilir.
>
> Artımlı yedeklemeler şu anda Azure Yedekleme tarafından desteklenmez.

## <a name="next-steps"></a>Sonraki adımlar

* CLI kullanarak Azure VM'de bir SAP HANA veritabanını nasıl geri yükleyeceğimi öğrenmek için öğreticiye devam edin – [CLI kullanarak Azure VM'de bir SAP HANA veritabanını geri yükleme](tutorial-sap-hana-restore-cli.md)

* Azure portalLarını kullanarak Azure VM'de çalışan bir SAP HANA veritabanını nasıl yedekleyeceğimi öğrenmek için Azure [VM'lerinde BIR SAP HANA veritabanlarını yedeklemeye](https://docs.microsoft.com/azure/backup/backup-azure-sap-hana-database) bakın
