---
title: Azure VM’sini oluşturduğunuz sırada yedeklemeyi etkinleştirme
description: Azure Yedekleme ile bir Azure VM oluştururken yedeklemeyi nasıl etkinleştirdiğinizi açıklar.
ms.topic: conceptual
ms.date: 06/13/2019
ms.openlocfilehash: 7739109eb8bad88c9b723e67e13adc78c127499a
ms.sourcegitcommit: b129186667a696134d3b93363f8f92d175d51475
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80672816"
---
# <a name="enable-backup-when-you-create-an-azure-vm"></a>Azure VM’sini oluşturduğunuz sırada yedeklemeyi etkinleştirme

Azure sanal makinelerini (VM) yedeklemek için Azure Yedekleme hizmetini kullanın. VM'ler yedekleme ilkesinde belirtilen zamanlamaya göre yedeklenir ve yedeklemelerden kurtarma noktaları oluşturulur. Kurtarma noktaları Kurtarma Hizmetleri kasalarında depolanır.

Bu makalede, Azure portalında sanal bir makine (VM) oluştururken yedeklemeyi nasıl etkinleştirebilirsiniz ayrıntıları.  

## <a name="before-you-start"></a>Başlamadan önce

- VM oluştururken yedeklemeyi etkinleştiriyorsanız hangi işletim sistemlerinin desteklenilip desteklenilip desteklenildiğinden [denetleyin.](backup-support-matrix-iaas.md#supported-backup-actions)

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

Hesabınızda zaten oturum açmadıysanız, [Azure portalında](https://portal.azure.com)oturum açın.

## <a name="create-a-vm-with-backup-configured"></a>Yedekleme yapılandırılmış bir VM oluşturma

1. Azure portalında **kaynak oluştur'u**tıklatın.

2. Azure Marketi'nde **Bilgi İşlem'i**tıklatın ve ardından bir VM resmi seçin.

3. VM'yi [Windows](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal) veya [Linux](https://docs.microsoft.com/azure/virtual-machines/linux/quick-create-portal) yönergelerine uygun olarak ayarlayın.

4. **Yönetim** sekmesinde, **yedeklemeyi etkinleştir'de**, **'''yi**tıklatın.
5. Kurtarma Hizmetleri kasasına Azure Yedekleme yedekleri. Varolan bir kasanız yoksa **Yeni Oluştur'u** tıklatın.
6. Önerilen kasa adını kabul edin veya kendi adınızı belirtin.
7. Kasanın bulunacağı bir kaynak grubu belirtin veya oluşturun. Kaynak grubu kasası VM kaynak grubundan farklı olabilir.

    ![VM için yedeklemeyi etkinleştirme](./media/backup-during-vm-creation/enable-backup.png)

8. Varsayılan yedekleme ilkesini kabul edin veya ayarları değiştirin.
    - Yedekleme ilkesi, VM'nin yedek anlık görüntülerini ne sıklıkta alacağını ve bu yedekleme kopyalarını ne kadar süreyle saklarısak onu belirtir.
    - Varsayılan ilke, VM'yi günde bir kez yedekler.
    - Bir Azure VM'nin günlük veya haftalık yedekleme alması için kendi yedekleme ilkenizi özelleştirebilirsiniz.
    - Azure VM'leri için yedekleme hususları hakkında [daha fazla bilgi edinin.](backup-azure-vms-introduction.md#backup-and-restore-considerations)
    - Anında geri yükleme işlevi hakkında [daha fazla bilgi edinin.](backup-instant-restore-capability.md)

      ![Varsayılan yedekleme ilkesi](./media/backup-during-vm-creation/daily-policy.png)

## <a name="azure-backup-resource-group-for-virtual-machines"></a>Sanal Makineler için Azure Yedekleme kaynak grubu

Yedekleme hizmeti, geri yükleme noktası koleksiyonunu (RPC) depolamak için VM'nin kaynak grubundan farklı ayrı bir kaynak grubu (RG) oluşturur. RPC, yönetilen VM'lerin anlık kurtarma noktalarını barındırabilir. Yedekleme hizmeti tarafından oluşturulan kaynak grubunun varsayılan adlandırma `AzureBackupRG_<Geo>_<number>`biçimi: . Örneğin: *AzureBackupRG_northeurope_1*. Artık Azure Yedekleme tarafından oluşturulan Kaynak grup adını özelleştirebilirsiniz.

Dikkat edilmesi gereken noktalar:

1. RG'nin varsayılan adını kullanabilir veya şirket gereksinimlerinize göre edinebilirsiniz.
2. VM yedekleme ilkesi oluşturma sırasında RG ad deseni giriş olarak sağlarsınız. RG adı aşağıdaki biçimde olmalıdır: `<alpha-numeric string>* n <alpha-numeric string>`. 'n' bir tamsayı ile değiştirilir (1'den başlayarak) ve ilk RG doluysa ölçeklendirme için kullanılır. Bir RG bugün 600 RPC'ler max olabilir.
              ![İlke oluştururken ad seçin](./media/backup-during-vm-creation/create-policy.png)
3. Desen aşağıdaki RG adlandırma kurallarına uymalı ve toplam uzunluk izin verilen maksimum RG ad uzunluğunu aşmamalıdır.
    1. Kaynak grubu adları yalnızca alfasayısal karakterlere, dönemlere, alt çizgiçizlere, tirelere ve paranteze izin verir. Bir dönemde sona eremezler.
    2. Kaynak grubu adları, RG'nin adı ve sonek de dahil olmak üzere en fazla 74 karakter içerebilir.
4. Birincisi `<alpha-numeric-string>` zorunluiken 'n' den sonraki ikinci zorunludur. Bu, yalnızca özelleştirilmiş bir ad verdiğinizde geçerlidir. Textbox'lardan herhangi birinde hiçbir şey girmezseniz, varsayılan ad kullanılır.
5. Gerektiğinde ilkeyi değiştirerek RG'nin adını değiştirebilirsiniz. Ad deseni değiştirilirse, yeni RG'de yeni RP'ler oluşturulur. Ancak, RP Collection kaynak hareketini desteklemediği için eski RP'ler eski RG'de yaşamaya devam eder ve taşınmaz. Sonunda, puanlar sona erdiğinde, RP'ler çöp leri toplayacak.
![İlkeyi değiştirirken adı değiştirme](./media/backup-during-vm-creation/modify-policy.png)
6. Yedekleme hizmeti tarafından kullanılmak üzere oluşturulan kaynak grubunu kilitlememesi önerilir.

PowerShell kullanarak Sanal Makineler için Azure Yedekleme kaynak grubunu yapılandırmak için [anlık görüntü tutma sırasında Azure Yedekleme kaynak grubu oluşturma'ya](backup-azure-vms-automation.md#creating-azure-backup-resource-group-during-snapshot-retention)bakın.

## <a name="start-a-backup-after-creating-the-vm"></a>VM'yi oluşturduktan sonra yedekleme başlatın

VM yedeklemeniz yedekleme politikanıza uygun olarak çalışır. Ancak, bir ilk yedekleme çalıştırmanızı öneririz.

VM oluşturulduktan sonra aşağıdakileri yapın:

1. VM özelliklerinde **Yedekleme'yi**tıklatın. VM durumu, ilk yedekleme çalışana kadar Bekleyen İlk Yedeklemedir
2. İsteğe bağlı yedekleme çalıştırmak için **şimdi Geri'yi** tıklatın.

    ![İsteğe bağlı yedekleme çalıştırma](./media/backup-during-vm-creation/run-backup.png)

## <a name="use-a-resource-manager-template-to-deploy-a-protected-vm"></a>Korumalı bir VM dağıtmak için Kaynak Yöneticisi şablonu kullanma

Önceki adımlar, sanal bir makine oluşturmak ve kurtarma hizmetleri kasasında korumak için Azure portalını nasıl kullanacağımı açıklar. Bir veya daha fazla VM'yi hızla dağıtmak ve bunları Kurtarma Hizmetleri kasasında korumak için, windows [vm dağıt şablonuna bakın ve yedeklemeyi etkinleştirin.](https://azure.microsoft.com/resources/templates/101-recovery-services-create-vm-and-configure-backup/)

## <a name="next-steps"></a>Sonraki adımlar

VM'nizi koruduğunuza göre, bunları nasıl yönetip geri yükleyeceğimize bakın.

- [VM’leri yönetme ve izleme](backup-azure-manage-vms.md)
- [Sanal makineyi geri yükleme](backup-azure-arm-restore-vms.md)

Herhangi bir sorunla karşılaşırsanız, sorun giderme kılavuzunu [inceleyin.](backup-azure-vms-troubleshoot.md)
