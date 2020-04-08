---
title: Azure Yedekleme Kurtarma Hizmetleri kasaları nasıl taşınır?
description: Kurtarma hizmetlerinin Azure abonelikleri ve kaynak gruplarında nasıl taşınacağına ilişkin talimatlar.
ms.reviewer: sogup
ms.topic: conceptual
ms.date: 04/08/2019
ms.openlocfilehash: 3cfd442d49de2661d68de3c4e4b3575119504eb4
ms.sourcegitcommit: 6397c1774a1358c79138976071989287f4a81a83
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "80804427"
---
# <a name="move-a-recovery-services-vault-across-azure-subscriptions-and-resource-groups"></a>Kurtarma Hizmetleri kasasının Azure Abonelikleri ve Kaynak Grupları arasında taşınması

Bu makalede, Azure Yedekleme için yapılandırılan bir Kurtarma Hizmetleri kasasının Azure abonelikleri arasında veya aynı abonelikteki başka bir kaynak grubuna nasıl taşınır. Kurtarma Hizmetleri kasasını taşımak için Azure portalını veya PowerShell'i kullanabilirsiniz.

## <a name="supported-regions"></a>Desteklenen bölgeler

Kurtarma Hizmetleri kasası için kaynak hareketi Avustralya Doğu, Avustralya Güney Doğu, Kanada Orta, Kanada Doğu, Güney Doğu Asya, Doğu Asya, Orta ABD, Kuzey Orta ABD, Doğu ABD, Doğu ABD, Güney Orta ABD, Güney Orta ABD, Batı Orta ABD, Batı ABD, Batı ABD, Batı ABD, Orta Hindistan, Güney Hindistan, Japonya Doğu, Japonya Batı, Kore Orta, Güney, Kuzey Avrupa, Batı Avrupa, Güney Afrika Kuzey desteklenir , Güney Afrika Batı, İngiltere Güney ve İngiltere Batı.

## <a name="unsupported-regions"></a>Desteklenmeyen bölgeler

Fransa Merkez, Fransa Güney, Almanya Kuzeydoğu, Almanya Merkez, ABD Gov Iowa, Çin Kuzey, Çin Kuzey2, Çin Doğu, Çin Doğu2

## <a name="prerequisites-for-moving-recovery-services-vault"></a>Kurtarma Hizmetleri kasasının taşınması için ön koşullar

- Kaynak grupları arasında kasa taşıma sırasında, hem kaynak hem de hedef kaynak grupları kilitlenerek yazma ve silme işlemlerini engeller. Daha fazla bilgi için bu [makaleye](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-move-resources)bakın.
- Yalnızca yönetici aboneliği, kasa taşıma izinlerine sahiptir.
- Kasaları abonelikler arasında taşımak için, hedef aboneliğin kaynak abonelikle aynı kiracıda olması ve durumunun etkinleştirilmesi gerekir.
- Hedef kaynak grubunda yazma işlemleri gerçekleştirmek için izniniz olmalıdır.
- Kasanın taşınması yalnızca kaynak grubunu değiştirir. Kurtarma Hizmetleri kasası aynı yerde ikamet edecek ve değiştirilemez.
- Bölge başına aynı anda yalnızca bir Kurtarma Hizmetleri kasası taşıyabilirsiniz.
- Bir VM, abonelikler arasında Kurtarma Hizmetleri kasasıyla veya yeni bir kaynak grubuna taşımazsa, geçerli VM kurtarma noktaları süresi dolana kadar kasada bozulmadan kalır.
- VM kasayla taşınsa da taşınmasa da, VM'yi kasadaki yedek geçmişinden her zaman geri yükleyebilirsiniz.
- Azure Disk Şifrelemesi, anahtar kasasının ve VM'lerin aynı Azure bölgesinde ve abonelikte ikamet ettiğini gerektirir.
- Yönetilen disklerle sanal bir makineyi taşımak için bu [makaleye](https://azure.microsoft.com/blog/move-managed-disks-and-vms-now-available/)bakın.
- Klasik model de dağıtılan kaynakları taşıma seçenekleri, kaynakları abonelik içinde mi yoksa yeni bir aboneye mi taşıyor sanız değişir. Daha fazla bilgi için bu [makaleye](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-move-resources)bakın.
- Kasa için tanımlanan yedekleme ilkeleri, kasa abonelikler arasında veya yeni bir kaynak grubuna geçtikten sonra korunur.
- IaaS VM'lerde Azure Dosyaları, Azure Dosya Eşitlemi veya SQL ile kasaların abonelikler ve kaynak grupları arasında taşınması desteklenmez.
- VM yedekleme verilerini içeren bir kasayı abonelikler arasında taşırsanız, yedeklemelere devam etmek için VM'lerinizi aynı aboneliğe taşımanız ve aynı hedef VM kaynak grup adını (eski abonelikte olduğu gibi) kullanmanız gerekir.

> [!NOTE]
> Azure Yedekleme için Kurtarma Hizmetleri kasalarını Azure bölgelerine taşımak desteklenmez.<br><br>
> **Azure Site Kurtarma'yı**kullanarak olağanüstü durum kurtarma için herhangi bir VM (Azure IaaS, Hyper-V, VMware) veya fiziksel makineleri yapılandırmışsanız, taşıma işlemi engellenir. Azure Site Kurtarma için kasaları taşımak istiyorsanız, tonozları el ile taşıma hakkında bilgi edinmek için [bu makaleyi](https://docs.microsoft.com/azure/site-recovery/move-vaults-across-regions) inceleyin.

## <a name="use-azure-portal-to-move-recovery-services-vault-to-different-resource-group"></a>Kurtarma Hizmetleri kasasını farklı kaynak grubuna taşımak için Azure portalını kullanma

Kurtarma hizmetleri kasasını ve ilişkili kaynaklarını farklı kaynak grubuna taşımak için

1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. Kurtarma Hizmetleri **kasalarının** listesini açın ve taşımak istediğiniz tonoz'u seçin. Kasa panosu açıldığında, aşağıdaki resimde gösterildiği gibi görünür.

   ![Açık Kurtarma Hizmet Kasası](./media/backup-azure-move-recovery-services/open-recover-service-vault.png)

   Kasanızın **Essentials** bilgilerini görmüyorsanız açılır simgeyi tıklatın. Şimdi kasanız için Essentials bilgilerini görmelisiniz.

   ![Temel Bilgiler sekmesi](./media/backup-azure-move-recovery-services/essentials-information-tab.png)

3. Kasagenel bakış menüsünde, **Kaynakları Taşı** bakışını açmak için **Kaynak grubunun**yanındaki **değiştir'i** tıklatın.

   ![Kaynak Grubunu Değiştir](./media/backup-azure-move-recovery-services/change-resource-group.png)

4. Kaynakları **Taşı** bıçağında, seçili kasa için aşağıdaki resimde gösterildiği gibi onay kutusunu seçerek isteğe bağlı ilgili kaynakları taşıması önerilir.

   ![Aboneliği Taşı](./media/backup-azure-move-recovery-services/move-resource.png)

5. Hedef kaynak grubunu eklemek için, **Kaynak grubu** açılır listesinde varolan bir kaynak grubu seçin veya yeni bir grup seçeneği **oluştur'u** tıklatın.

   ![Kaynak Oluşturma](./media/backup-azure-move-recovery-services/create-a-new-resource.png)

6. Kaynak grubunu ekledikten sonra, **taşınan kaynaklarla ilişkili araçların ve komut dosyalarının, yeni kaynak tanımları seçeneğini kullanmak üzere bunları güncelleştirene kadar çalışmayacağını anladığımı** ve kasayı taşımayı tamamlamak için **Tamam'ı** tıklatın.

   ![Onay Mesajı](./media/backup-azure-move-recovery-services/confirmation-message.png)

## <a name="use-azure-portal-to-move-recovery-services-vault-to-a-different-subscription"></a>Kurtarma Hizmetleri kasasını farklı bir aboneye taşımak için Azure portalını kullanın

Kurtarma Hizmetleri kasasını ve ilişkili kaynaklarını farklı bir aboneye taşıyabilirsiniz

1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. Kurtarma Hizmetleri kasalarının listesini açın ve taşımak istediğiniz tonoz'u seçin. Kasa panosu açıldığında, aşağıdaki resimde gösterildiği gibi görünür.

    ![Açık Kurtarma Hizmet Kasası](./media/backup-azure-move-recovery-services/open-recover-service-vault.png)

    Kasanızın **Essentials** bilgilerini görmüyorsanız açılır simgeyi tıklatın. Şimdi kasanız için Essentials bilgilerini görmelisiniz.

    ![Temel Bilgiler sekmesi](./media/backup-azure-move-recovery-services/essentials-information-tab.png)

3. Kasagenel bakış menüsünde, **Kaynakları Taşı** bakışını açmak için **Abonelik'in**yanındaki **değiştir'i** tıklatın.

   ![Aboneliği Değiştir](./media/backup-azure-move-recovery-services/change-resource-subscription.png)

4. Taşınacak kaynakları seçin, burada listelenen tüm isteğe bağlı kaynakları seçmek için **Tümünü Seç** seçeneğini kullanmanızı öneririz.

   ![kaynağı taşıma](./media/backup-azure-move-recovery-services/move-resource-source-subscription.png)

5. Kasanın taşınmasını istediğiniz **Abonelik** açılır listesinden hedef aboneliği seçin.
6. Hedef kaynak grubunu eklemek için, **Kaynak grubu** açılır listesinde varolan bir kaynak grubu seçin veya yeni bir grup seçeneği **oluştur'u** tıklatın.

   ![Azure](./media/backup-azure-move-recovery-services/add-subscription.png)

7. **Taşınan kaynaklarla ilişkili araçların ve komut dosyalarının, onaylamak için yeni kaynak iD'leri seçeneğini kullanmak üzere güncelleştirene kadar çalışmayacağını ve** ardından **Tamam'ı**tıklatın.

> [!NOTE]
> Çapraz abonelik yedeklemesi (RS kasası ve korumalı VM'ler farklı aboneliklerdedir) desteklenen bir senaryo değildir. Ayrıca, depolama artıklığı seçeneği yerel yedekdepolama (LRS) küresel yedekli depolama (GRS) ve tam tersi kasa taşıma işlemi sırasında değiştirilemez.
>
>

## <a name="use-powershell-to-move-recovery-services-vault"></a>Kurtarma Hizmetleri kasasını taşımak için PowerShell'i kullanın

Kurtarma Hizmetleri kasasını başka bir kaynak `Move-AzureRMResource` grubuna taşımak için cmdlet'i kullanın. `Move-AzureRMResource`kaynak adını ve kaynak türünü gerektirir. Her ikisini de `Get-AzureRmRecoveryServicesVault` cmdlet'ten alabilirsiniz.

```powershell
$destinationRG = "<destinationResourceGroupName>"
$vault = Get-AzureRmRecoveryServicesVault -Name <vaultname> -ResourceGroupName <vaultRGname>
Move-AzureRmResource -DestinationResourceGroupName $destinationRG -ResourceId $vault.ID
```

Kaynakları farklı abonelik lere taşımak `-DestinationSubscriptionId` için parametreyi ekleyin.

```powershell
Move-AzureRmResource -DestinationSubscriptionId "<destinationSubscriptionID>" -DestinationResourceGroupName $destinationRG -ResourceId $vault.ID
```

Yukarıdaki cmdlets çalıştırdıktan sonra, belirtilen kaynakları taşımak istediğinizi onaylamak için istenir. Onaylamak için **Y** yazın. Başarılı bir doğrulamadan sonra kaynak taşınır.

## <a name="use-cli-to-move-recovery-services-vault"></a>Kurtarma Hizmetleri kasasını taşımak için CLI'yi kullanın

Kurtarma Hizmetleri kasasını başka bir kaynak grubuna taşımak için aşağıdaki cmdlet'i kullanın:

```azurecli
az resource move --destination-group <destinationResourceGroupName> --ids <VaultResourceID>
```

Yeni bir aboneye geçmek `--destination-subscription-id` için parametreyi sağlayın.

## <a name="post-migration"></a>Geçiş sonrası

1. Kaynak gruplarının erişim denetimlerini ayarlayın/doğrulayın.  
2. Yedekleme raporlama ve izleme özelliğinin, taşımanın tamamlanabilmesi için kasa gönderimi için yeniden yapılandırılması gerekir. Önceki yapılandırma taşıma işlemi sırasında kaybolur.

## <a name="next-steps"></a>Sonraki adımlar

Kaynak grupları ve abonelikler arasında birçok farklı kaynak türü taşıyabilirsiniz.

Daha fazla bilgi için bkz. [Kaynakları yeni kaynak grubuna veya aboneliğe taşıma](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-move-resources).
