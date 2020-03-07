---
title: Azure VM’sini oluşturduğunuz sırada yedeklemeyi etkinleştirme
description: Azure Backup ile bir Azure VM oluşturduğunuzda yedeklemenin nasıl etkinleştirileceğini açıklar.
ms.topic: conceptual
ms.date: 06/13/2019
ms.openlocfilehash: 0cfea6579791c4fd23c1b7acdfe722d57b5ec2fd
ms.sourcegitcommit: 509b39e73b5cbf670c8d231b4af1e6cfafa82e5a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78363759"
---
# <a name="enable-backup-when-you-create-an-azure-vm"></a>Azure VM’sini oluşturduğunuz sırada yedeklemeyi etkinleştirme

Azure sanal makinelerini (VM) yedeklemek için Azure Backup hizmetini kullanın. VM 'Ler, yedekleme ilkesinde belirtilen bir zamanlamaya göre yedeklenir ve kurtarma noktaları yedeklerden oluşturulur. Kurtarma noktaları Kurtarma Hizmetleri kasalarında depolanır.

Bu makalede, Azure portal bir sanal makine (VM) oluşturduğunuzda yedeklemenin nasıl etkinleştirileceği açıklanır.  

## <a name="before-you-start"></a>Başlamadan önce

- Bir VM oluştururken yedeklemeyi etkinleştirirseniz, hangi işletim sistemlerinin desteklendiğini [denetleyin](backup-support-matrix-iaas.md#supported-backup-actions) .

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

Hesabınızda zaten oturum açmadıysanız [Azure Portal](https://portal.azure.com)oturum açın.

## <a name="create-a-vm-with-backup-configured"></a>Yedekleme yapılandırılmış bir VM oluşturma

1. Azure portal, **kaynak oluştur ' a**tıklayın.

2. Azure Marketi 'nde **işlem**' e tıklayın ve ardından bir VM görüntüsü seçin.

3. [Windows](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal) veya [Linux](https://docs.microsoft.com/azure/virtual-machines/linux/quick-create-portal) yönergelerine uygun olarak VM 'yi ayarlayın.

4. **Yönetim** sekmesinde, **yedeklemeyi etkinleştir**' de, **Açık**' a tıklayın.
5. Kurtarma Hizmetleri kasasındaki yedeklemeleri Azure Backup. Mevcut bir kasanız yoksa **Yeni oluştur** ' a tıklayın.
6. Önerilen kasa adını kabul edin veya kendinizinkini belirtin.
7. Kasasının bulunduğu bir kaynak grubu belirtin veya oluşturun. Kaynak grubu Kasası, VM kaynak grubundan farklı olabilir.

    ![VM için yedeklemeyi etkinleştirme](./media/backup-during-vm-creation/enable-backup.png)

8. Varsayılan yedekleme ilkesini kabul edin veya ayarları değiştirin.
    - Yedekleme ilkesi, VM 'nin yedekleme anlık görüntülerinin ne sıklıkta alınacağını ve bu yedek kopyalarının ne kadar süreyle saklanacağını belirtir.
    - Varsayılan ilke, VM 'yi günde bir kez yedekler.
    - Her gün veya haftalık yedeklemeler almak için, bir Azure VM için kendi yedekleme ilkenizi özelleştirebilirsiniz.
    - Azure VM 'Leri için yedekleme konuları hakkında [daha fazla bilgi edinin](backup-azure-vms-introduction.md#backup-and-restore-considerations) .
    - Anında geri yükleme işlevselliği hakkında [daha fazla bilgi edinin](backup-instant-restore-capability.md) .

      ![Varsayılan yedekleme ilkesi](./media/backup-during-vm-creation/daily-policy.png)

## <a name="azure-backup-resource-group-for-virtual-machines"></a>Sanal makineler için Azure Backup kaynak grubu

Yedekleme hizmeti, geri yükleme noktası koleksiyonunu (RPC) depolamak için VM 'nin kaynak grubundan farklı olan ayrı bir kaynak grubu (RG) oluşturur. RPC, yönetilen sanal makinelerin anlık kurtarma noktalarını barındırır. Yedekleme hizmeti tarafından oluşturulan kaynak grubunun varsayılan adlandırma biçimi: `AzureBackupRG_<Geo>_<number>`. Örneğin: *AzureBackupRG_northeurope_1*. Artık Azure Backup tarafından oluşturulan kaynak grubu adını özelleştirebilirsiniz.

Dikkat edilecek noktalar:

1. RG varsayılan adını kullanabilir veya Şirket gereksinimlerinize göre düzenleyebilirsiniz.
2. VM yedekleme ilkesi oluşturma sırasında RG Name modelini girdi olarak sağlarsınız. RG adı şu biçimde olmalıdır: `<alpha-numeric string>* n <alpha-numeric string>`. ' n ', (1 ' den başlayarak) bir tamsayı ile değiştirilmiştir ve ilk RG dolduğunda ölçeği genişletmek için kullanılır. Bir RG bugün en fazla 600 RPC olabilir.
              ![ilke oluştururken ad seçin](./media/backup-during-vm-creation/create-policy.png)
3. Bu model, aşağıdaki RG adlandırma kurallarını izlemelidir ve Toplam uzunluk izin verilen maksimum RG ad uzunluğunu aşmamalıdır.
    1. Kaynak grubu adları yalnızca alfasayısal karakter, nokta, alt çizgi, kısa çizgi ve parantezlere izin verir. Nokta ile bitmemelidir.
    2. Kaynak grubu adları, RG ve suffix 'un adı da dahil olmak üzere en fazla 74 karakter içerebilir.
4. İkinci bir değer ' n ' isteğe bağlı olduğu sürece ilk `<alpha-numeric-string>` zorunludur. Bu, yalnızca özelleştirilmiş bir ad vermeniz durumunda geçerlidir. Herhangi bir metin kutusunda hiçbir şey girmezseniz, varsayılan ad kullanılır.
5. Gerektiğinde ilkeyi değiştirerek RG 'nin adını düzenleyebilirsiniz. Ad deseninin değişmesi durumunda yeni bir RG yeni RPs oluşturulur. Ancak, eski RPs 'Ler hala eski RG 'de yer alır ve RP koleksiyonu kaynak taşımayı desteklemediği için taşınmaz. Bu süre sonunda, noktaların süre dolduğunda RPs atık olarak toplanır.
ilke değiştirilirken ![adı değiştirin](./media/backup-during-vm-creation/modify-policy.png)
6. Yedekleme hizmeti tarafından kullanılmak üzere oluşturulan kaynak grubunu kilitlemeniz önerilir.

## <a name="start-a-backup-after-creating-the-vm"></a>VM oluşturduktan sonra bir yedekleme başlatma

VM yedeğiniz, yedekleme ilkenize uygun olarak çalışacaktır. Ancak, bir ilk yedekleme çalıştırmanızı öneririz.

VM oluşturulduktan sonra şunları yapın:

1. VM özellikleri ' nde **Yedekle**' ye tıklayın. İlk yedekleme çalışana kadar VM durumu Ilk yedekleme bekliyor
2. İsteğe bağlı yedekleme çalıştırmak için **Şimdi Yedekle** ' ye tıklayın.

    ![İsteğe bağlı yedekleme çalıştırma](./media/backup-during-vm-creation/run-backup.png)

## <a name="use-a-resource-manager-template-to-deploy-a-protected-vm"></a>Korunan bir VM dağıtmak için Kaynak Yöneticisi şablonu kullanma

Önceki adımlarda, bir sanal makine oluşturmak ve bir kurtarma hizmetleri kasasında korumak için Azure portal nasıl kullanılacağı açıklanmaktadır. Bir veya daha fazla VM 'yi hızlıca dağıtmak ve bir kurtarma hizmetleri kasasında korumak için, [bir Windows sanal makinesi dağıtma ve yedeklemeyi etkinleştirme](https://azure.microsoft.com/resources/templates/101-recovery-services-create-vm-and-configure-backup/)şablonuna bakın.

## <a name="next-steps"></a>Sonraki adımlar

VM 'nizi koruduktan sonra, bunları yönetmeyi ve geri yüklemeyi öğrenin.

- [VM 'Leri yönetme ve izleme](backup-azure-manage-vms.md)
- [Sanal makineyi geri yükleme](backup-azure-arm-restore-vms.md)

Herhangi bir sorunla karşılaşırsanız sorun giderme kılavuzunu [gözden geçirin](backup-azure-vms-troubleshoot.md) .
