---
title: Tek başına Azure Otomasyonu hesabı oluşturma
description: Bu makalede, tek başına Azure Otomasyonu hesabının ve klasik farklı çalıştır hesabının nasıl oluşturulacağı açıklanır.
services: automation
ms.subservice: process-automation
ms.date: 01/15/2019
ms.topic: conceptual
ms.openlocfilehash: 3739f8691a144f183e72af19233f776a24066cbc
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84118791"
---
# <a name="create-a-standalone-azure-automation-account"></a>Tek başına Azure Otomasyonu hesabı oluşturma

Bu makalede, Azure portal Azure Otomasyonu hesabının nasıl oluşturulacağı gösterilmektedir. Ek yönetim özellikleri kullanmadan veya Azure Izleyici günlükleriyle tümleştirmeden Otomasyon hakkında bilgi edinmek ve öğrenmek için Portal Otomasyonu hesabını kullanabilirsiniz. Gelecekte herhangi bir noktada runbook işlerinin gelişmiş izlenmesi için, yönetim özellikleri ekleyebilir veya Azure Izleyici günlükleriyle tümleştirebilirsiniz.

Otomasyon hesabı ile Azure Resource Manager veya klasik dağıtım modelinde kaynakları yöneterek runbook 'ları doğrulayabilirsiniz. Bir Otomasyon Hesabı belirli bir kiracı için kaynakları tüm bölgelerde ve aboneliklerde yönetebilir.

Azure portal bir Otomasyon hesabı oluşturduğunuzda, **Farklı Çalıştır** hesabı otomatik olarak oluşturulur. Bu hesap aşağıdaki görevleri yapar:

* Azure Active Directory (Azure AD) içinde bir hizmet sorumlusu oluşturur.
* Bir sertifika oluşturur.
* Runbook 'ları kullanarak Azure Resource Manager kaynaklarını yöneten, katkıda bulunan rol tabanlı Access Control (RBAC) atar.

Sizin için oluşturulan bu hesap sayesinde, Otomasyon gereksinimlerinizi desteklemek için Runbook 'ları oluşturmaya ve dağıtmaya hızlı bir başlangıç yapabilirsiniz.

## <a name="permissions-required-to-create-an-automation-account"></a>Otomasyon hesabı oluşturmak için gereken izinler

Bir Otomasyon hesabı oluşturmak veya güncelleştirmek ve bu makalede açıklanan görevleri gerçekleştirmek için aşağıdaki ayrıcalıklara ve izinlere sahip olmanız gerekir:

* Bir Otomasyon hesabı oluşturmak için, Azure AD Kullanıcı hesabınızın, kaynaklar için sahip rolüne eşdeğer izinlere sahip bir role eklenmesi gerekir `Microsoft.Automation` . Daha fazla bilgi için bkz. [Azure Otomasyonu 'Nda rol tabanlı Access Control](automation-role-based-access-control.md).
* Azure Portal **Azure Active Directory**  >  **MANAGE**  >  **Kullanıcı ayarlarını**Yönet altında **uygulama kayıtları** **Evet**olarak ayarlanırsa, Azure AD kiracınızdaki yönetici olmayan kullanıcılar [Active Directory uygulamalarını kaydedebilir](../active-directory/develop/howto-create-service-principal-portal.md#check-azure-subscription-permissions). **Uygulama kayıtları** **Hayır**olarak ayarlanırsa, bu EYLEMI gerçekleştiren kullanıcının Azure AD 'de en az bir uygulama geliştirici rolüne sahip olması gerekir.

Aboneliğin genel yönetici/ortak yönetici rolüne eklenmeden önce aboneliğin Active Directory örneğinin bir üyesi değilseniz, konuk olarak Active Directory olarak eklendiniz. Bu senaryoda, bu iletiyi Otomasyon hesabı Ekle bölmesinde görürsünüz:`You do not have permissions to create.`

Önce genel yönetici/ortak yönetici rolüne bir Kullanıcı eklendiyse, kullanıcıyı aboneliğin Active Directory örneğinden kaldırabilirsiniz. Kullanıcıyı Active Directory kullanıcı rolüne yeniden ekleyebilirsiniz. Kullanıcı rollerini doğrulamak için:

1. Azure portal Azure Active Directory bölmesine gidin.
1. **Kullanıcılar ve gruplar ' ı**seçin.
1. **Tüm kullanıcılar**' ı seçin.
1. Belirli bir Kullanıcı seçtikten sonra **profil**' i seçin. Kullanıcı profili altındaki **Kullanıcı türü** özniteliğinin değeri **konuğa**sahip olmamalıdır.

## <a name="create-a-new-automation-account-in-the-azure-portal"></a>Azure portal yeni bir Otomasyon hesabı oluşturun

Azure portal bir Azure Otomasyonu hesabı oluşturmak için aşağıdaki adımları izleyin:

1. Abonelik yöneticileri rolünün üyesi ve aboneliğin ortak Yöneticisi olan bir hesapla Azure portal oturum açın.
1. **+ Kaynak oluştur**' u seçin.
1. **Otomasyon**araması yapın. Arama sonuçlarında **Otomasyon**' u seçin.

   ![Azure Marketi 'nde Otomasyon & denetimini arama ve seçme](media/automation-create-standalone-account/automation-marketplace-select-create-automationacct.png)

1. Sonraki ekranda **Yeni oluştur**' u seçin.

   ![Otomasyon hesabı ekle](media/automation-create-standalone-account/automation-create-automationacct-properties.png)

   > [!NOTE]
   > Otomasyon hesabı Ekle bölmesinde aşağıdaki iletiyi görürseniz, hesabınız abonelik yöneticileri rolünün bir üyesi ve aboneliğin ortak Yöneticisi değildir.
   >
   > ![Otomasyon hesabı ekleme uyarısı](media/automation-create-standalone-account/create-account-without-perms.png)

1. Otomasyon hesabı Ekle bölmesinde, **ad** alanına yeni otomasyon hesabınız için bir ad girin. Bu adı seçtikten sonra değiştiremezsiniz. 

    > [!NOTE]
    > Otomasyon hesabı adları, bölge ve kaynak grubu başına benzersizdir. Silinen Otomasyon hesaplarının adları hemen kullanılamayabilir.

1. Birden fazla aboneliğiniz varsa, yeni hesap için kullanılacak aboneliği belirtmek için **abonelik** alanını kullanın.
1. **Kaynak grubu**için, yeni veya mevcut bir kaynak grubu girin veya seçin.
1. **Konum**Için bir Azure veri merkezi konumu seçin.
1. **Azure farklı çalıştır hesabı oluştur** seçeneği için **Evet** ' in seçili olduğundan emin olun ve ardından **Oluştur**' a tıklayın.

   > [!NOTE]
   > **Azure farklı çalıştır hesabı oluştur**' **u seçerek farklı** Çalıştır hesabı oluşturmayı seçerseniz, Otomasyon hesabı Ekle bölmesinde bir ileti görüntülenir. Hesap Azure portal oluşturulsa da hesabın klasik dağıtım modeli aboneliğinizde veya Azure Resource Manager abonelik dizin hizmetinde ilgili bir kimlik doğrulama kimliği yoktur. Bu nedenle, Otomasyon hesabının aboneliğinizdeki kaynaklara erişimi yoktur. Bu, bu hesaba başvuran runbook 'ların bu dağıtım modellerindeki kaynaklara göre kimlik doğrulaması yapmasını ve görevler gerçekleştirmesini engeller.
   >
   > ![Otomasyon hesabı ekleme uyarısı](media/automation-create-standalone-account/create-account-decline-create-runas-msg.png)
   >
   > Hizmet sorumlusu oluşturulmadığında katkıda bulunan rolü atanmaz.
   >

1. Otomasyon hesabı oluşturma işleminin ilerlemesini izlemek için menüdeki **Bildirimler** ' i seçin.

Otomasyon hesabı başarıyla oluşturulduğunda bazı kaynaklar sizin için otomatik olarak oluşturulur. Oluşturulduktan sonra, bu runbook 'ları korumak istemiyorsanız güvenli bir şekilde silinebilir. Farklı Çalıştır hesapları, bir runbook 'ta hesabınızda kimlik doğrulaması yapmak için kullanılabilir ve başka bir tane oluşturmadığınız ya da bunları gerektirmediğiniz sürece bu durumda bırakılmalıdır. Aşağıdaki tabloda Farklı Çalıştır hesabının kaynakları özetlenmektedir.

| Kaynak | Açıklama |
| --- | --- |
| AzureAutomationTutorial Runbook |Farklı Çalıştır hesabını kullanarak kimlik doğrulamanın nasıl yapılacağını gösteren örnek bir grafik runbook. Runbook tüm Kaynak Yöneticisi kaynaklarını alır. |
| AzureAutomationTutorialScript Runbook |Farklı Çalıştır hesabını kullanarak kimlik doğrulaması yapılacağını gösteren örnek bir PowerShell runbook 'u. Runbook tüm Kaynak Yöneticisi kaynaklarını alır. |
| AzureAutomationTutorialPython2 Runbook |Farklı Çalıştır hesabını kullanarak kimlik doğrulaması yapılacağını gösteren örnek bir Python runbook. Runbook, abonelikte bulunan tüm kaynak gruplarını listeler. |
| AzureRunAsCertificate |Otomasyon hesabı oluşturulduğunda veya var olan bir hesap için bir PowerShell betiği kullanılarak otomatik olarak oluşturulan bir sertifika varlığı. Sertifika, runbook 'lardan Azure Resource Manager kaynaklarını yönetebilmeniz için Azure ile kimlik doğrulaması yapar. Bu sertifikanın bir yıllık kullanım ömrü vardır. |
| AzureRunAsConnection |Otomasyon hesabı oluşturulduğunda veya var olan bir hesap için bir PowerShell betiği kullanılarak otomatik olarak oluşturulan bağlantı varlığı. |

## <a name="create-a-classic-run-as-account"></a>Klasik farklı çalıştır hesabı oluşturma

Klasik farklı çalıştır hesapları artık bir Azure Otomasyonu hesabı oluşturduğunuzda varsayılan olarak oluşturulmaz. Hala klasik bir Farklı Çalıştır hesabına ihtiyacınız varsa:

1. Otomasyon hesabınızdan **Hesap ayarları**altında **Farklı Çalıştır hesapları** ' nı seçin.
2. **Azure klasik farklı çalıştır hesabı**' nı seçin.
3. Klasik farklı çalıştır hesabı oluşturma işlemine devam etmek için **Oluştur** ' a tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

* Grafik yazma hakkında daha fazla bilgi edinmek için bkz. [Azure Otomasyonu 'nda grafik runbook 'Ları yazma](automation-graphical-authoring-intro.md).
* PowerShell runbook 'larını kullanmaya başlamak için bkz. [öğretici: PowerShell runbook 'U oluşturma](learn/automation-tutorial-runbook-textual-powershell.md).
* PowerShell Iş akışı runbook 'larını kullanmaya başlamak için bkz. [öğretici: PowerShell iş akışı runbook 'U oluşturma](learn/automation-tutorial-runbook-textual.md).
* Python 2 runbook 'ları kullanmaya başlamak için bkz. [öğretici: Python 2 runbook oluşturma](learn/automation-tutorial-runbook-textual-python2.md).
* PowerShell cmdlet başvurusu için bkz. [az. Automation](https://docs.microsoft.com/powershell/module/az.automation/?view=azps-3.7.0#automation
).