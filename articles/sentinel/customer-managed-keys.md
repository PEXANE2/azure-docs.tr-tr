---
title: Azure Sentinel'de müşteri tarafından yönetilen anahtarları ayarlama| Microsoft Dokümanlar
description: Azure Sentinel'de müşteri tarafından yönetilen anahtarları (CMK) nasıl ayarlayabilirsiniz öğrenin.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/30/2019
ms.author: yelevin
ms.openlocfilehash: bc2fa02925e7f5c671085eb87ca0431d3fca7691
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77587966"
---
# <a name="set-up-azure-sentinel-customer-managed-key"></a>Azure Sentinel müşteri tarafından yönetilen anahtarı ayarlama


Bu makalede, Azure Sentinel için müşteri tarafından yönetilen bir anahtarı (CMK) yapılandırmak için arka plan bilgileri ve adımlar sağlanmaktadır. CMK, azure sentinel'e kaydedilen veya gönderilen tüm verilerin, sizin oluşturduğunuz veya sahip olduğunuz bir Azure Anahtar Kasası anahtarıyla ilgili tüm depolama kaynaklarında şifrelenmesini sağlar.

> [!NOTE]
> -   Azure Sentinel CMK özelliği yalnızca **yeni** olan müşterilere sağlanır ve bu özelliğe erişim Azure özellik kaydı tarafından denetlenir.İletişim azuresentinelCMK@microsoft.comkurarak erişim isteğinde bulunabilirsiniz ve kapasite kullanılabilir olduğundan bekleyen istekler onaylanacaktır.
> -   Azure Sentinel CMK özelliği yalnızca Doğu ABD, Batı ABD 2 ve Güney-Orta ABD bölgelerinde kullanılabilir.
> -   CMK özelliği yalnızca günde 1 TB veya daha fazla gönderen müşteriler tarafından kullanılabilir. Azure aboneliğinizde CMK sağlamak için Microsoft'a başvurduğunuzda ek fiyatlandırma hakkında bilgi alırsınız. [Log Analytics](../azure-monitor/platform/customer-managed-keys.md#disclaimers) şarjı hakkında daha fazla bilgi edinin.

## <a name="how-cmk-works"></a>CMK nasıl çalışır? 

Azure Sentinel çözümü, günlük koleksiyonu ve özellikleri için birkaç depolama kaynağı kullanır, bunlar arasında Günlük Analizi ve diğer depolama kaynakları yer alır. Azure Sentinel CMK yapılandırmasının bir parçası olarak, ilgili depolama kaynaklarında CMK ayarlarını da yapılandırmanız gerekir. Log Analytics dışındaki depolama kaynaklarına kaydedilen veriler de şifrelenir.

> [!NOTE]
> Azure Sentinel'de CMK'yı etkinleştiriseniz, CMK'yı desteklemeyen herhangi bir Genel Önizleme özelliği etkinleştirilmez.

## <a name="enable-cmk"></a>CMK'yı etkinleştir 

CMK'yı sağlamak için aşağıdaki adımları izleyin: 

1.  Azure Anahtar Kasası ve depolama anahtarı oluşturun.

2.  Log Analytics çalışma alanınızda CMK'yı etkinleştirin.

3.  Cosmos DB için kaydolun.

4.  Azure Anahtar Kasası örneğinize bir erişim ilkesi ekleyin.

5.  Azure Sentinel'de CMK'yı etkinleştirin.

6.  Azure Sentinel'i etkinleştirin.

### <a name="step-1-create-an-azure-key-vault-and-storing-key"></a>ADIM 1: Azure Anahtar Kasası oluşturma ve anahtar depolama

1.  [Azure Key Vault kaynağını oluşturun,](https://docs.microsoft.com/azure-stack/user/azure-stack-key-vault-manage-portal?view=azs-1910)ardından veri şifrelemeiçin kullanılacak bir anahtar oluşturun veya içe aktarın.
    > [!NOTE]
    >  Azure Anahtar Kasası, anahtarınızı ve erişimi korumak için kurtarılabilir olarak yapılandırılmalıdır.

1.  [Kurtarma seçeneklerini açın:](../key-vault/key-vault-best-practices.md#turn-on-recovery-options)

    -   Yumuşak [Silme'nin](../key-vault/key-vault-ovw-soft-delete.md) açık olduğundan emin olun.

    -   Yumuşak silme işleminden sonra bile gizli/kasanın zorla silinmesine karşı korumak için [Temizleme korumasını](../key-vault/key-vault-ovw-soft-delete.md#purge-protection) açın.

### <a name="step-2-enable-cmk-on-your-log-analytics-workspace"></a>ADIM 2: Log Analytics çalışma alanınızda CMK'yı etkinleştirin

Aşağıdaki adımlarda Azure Sentinel çalışma alanı olarak kullanılacak bir CMK çalışma alanı oluşturmak için [Azure Monitor müşteri tarafından yönetilen anahtar yapılandırmasındaki](../azure-monitor/platform/customer-managed-keys.md) yönergeleri izleyin.

### <a name="step-3-register-for-cosmos-db"></a>ADIM 3: Cosmos DB'ye kaydolun

Azure Sentinel, ek depolama kaynağı olarak Cosmos DB ile çalışır. Cosmos DB'ye kaydolduğunuzdan emin olun.

Azure aboneliğiniz için [Azure Cosmos DB](../cosmos-db/how-to-setup-cmk.md#register-resource-provider) kaynak sağlayıcısını kaydetmek için Cosmos DB yönergesine uyun.

### <a name="step-4-add-an-access-policy-to-your-azure-key-vault-instance"></a>ADIM 4: Azure Anahtar Kasası örneğinize bir erişim ilkesi ekleme

Cosmos DB'den Azure Key Vault örneğinize erişim eklediğinizden emin olun. Azure Cosmos DB sorumlusuyla [Azure Key Vault örneğinize bir erişim ilkesi eklemek için](../cosmos-db/how-to-setup-cmk.md#add-an-access-policy-to-your-azure-key-vault-instance) Cosmos DB yönergesini izleyin.

### <a name="step-5-enable-cmk-in-azure-sentinel"></a>ADIM 5: Azure Sentinel'de CMK'yı etkinleştirme

Azure Sentinel CMK özelliği, yeni müşterilere yalnızca doğrudan Azure ürün grubundan erişim aldıktan sonra sağlanır. Çözümünüzde CMK'yı etkinleştirmek için Azure Sentinel ekibinden onay almak için Microsoft'taki kişilerinizi kullanın.

Onay aldıktan sonra CMK özelliğini etkinleştirmek için aşağıdaki bilgileri sağlamanız istenir.

-  CMK'yı etkinleştirmek istediğiniz çalışma alanı kimliği

-  Anahtar Vault URL' si: Anahtarın "Anahtar Tanımlayıcısını" son ileri eğretme ye kadar kopyalayın:  
    

    ![anahtar tanımlayıcısı](./media/customer-managed-keys/key-identifier.png)

    Azure Sentinel ekibi, sağladığınız çalışma alanı için Azure Sentinel CMK özelliğini etkinleştirecektir.

-  Azure Sentinel ürün ekibinden bu özelliği kullanmanız için onaylandığınıdoğrulama. Devam etmeden önce buna sahip olmalısınız.

### <a name="step-6-enable-azure-sentinel"></a>ADIM 6: Azure Sentinel'i etkinleştirme


Azure portalına gidin ve CMK'yı ayarladığınız çalışma alanında Azure Sentinel'i etkinleştirin. Daha fazla bilgi için Azure [Sentinel Onboarding'e](quickstart-onboard.md)bakın.

## <a name="key-encryption-key-revocation-or-deletion"></a>Anahtar Şifreleme Anahtarı iptali veya silme


Bir kullanıcının anahtar şifreleme anahtarını silerek veya Azure Sentinel'in erişimini kaldırarak iptal etme durumunda, Azure Sentinel değişikliği onurlandıracak ve veriler artık kullanılamıyormuş gibi çalışacaktır. Bu noktada, veri alma, kalıcı yapılandırma değişiklikleri ve olay oluşturma gibi kalıcı depolama kaynaklarını kullanan herhangi bir işlem engellenir. Daha önce depolanan veriler silinmez, ancak erişilemez kalır. Erişilemeyen veriler veri saklama ilkesi tarafından yönetilir ve bu ilke uyarınca tasfiye edilecektir.

Şifreleme anahtarı iptal edildikten veya silindikten sonra mümkün olan tek işlem hesap silme işlemidir.

İptal edildikten sonra erişim geri yüklenirse, Azure Sentinel verilere erişimi bir saat içinde geri yüklenir.

Azure Monitor'da bunun nasıl çalıştığı hakkında daha fazla şey öğrenmek için Azure [Monitor CMK iptaline](../azure-monitor/platform/customer-managed-keys.md#cmk-kek-revocation)bakın.

## <a name="key-encryption-key-rotation"></a>Anahtar şifreleme anahtarı döndürme


Azure Sentinel ve Log Analytics anahtar döndürmeyi destekler. Bir kullanıcı Key Vault'ta anahtar döndürme gerçekleştirdiğinde, Azure Sentinel yeni anahtarı bir saat içinde destekler.

Anahtar Atlama'da, anahtarın yeni bir sürümünü oluşturarak anahtar döndürme gerçekleştirebilirsiniz:

![anahtar döndürme](./media/customer-managed-keys/key-rotation.png)

Anahtarın önceki sürümünü 24 saat sonra devre dışı kullanabilirsiniz veya Azure Key Vault denetim günlükleri artık önceki sürümü kullanan herhangi bir etkinlik göstermedikten sonra.

Azure Sentinel ve Log Analytics'te aynı anahtarı kullanıyorsanız, anahtar döndürme gerçekleştirmeniz gerekirse, Log Analytics'teki küme kaynağını yeni Azure Key Vault anahtar sürümüyle açıkça güncelleştirmeniz gerekir. Daha fazla bilgi için Azure [Monitor CMK döndürme'ye](../azure-monitor/platform/customer-managed-keys.md#cmk-kek-rotation)bakın.

## <a name="next-steps"></a>Sonraki adımlar
Bu belgede, Azure Sentinel'de müşteri tarafından yönetilen bir anahtarı nasıl ayarlayabileceğinizi öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Verilerinize ve olası tehditlere](quickstart-get-visibility.md)nasıl görünürlük elde edebilirsiniz öğrenin.
- Azure [Sentinel ile tehditleri algılamaya](tutorial-detect-threats.md)başlayın.
- Verilerinizi izlemek için [çalışma kitaplarını kullanın.](tutorial-monitor-your-data.md)

