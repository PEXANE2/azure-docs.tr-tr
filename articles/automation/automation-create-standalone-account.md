---
title: Tek başına Azure Otomasyonu hesabı oluşturma
description: Bu makale, Azure Otomasyonu'nda örnek bir güvenlik temel kimlik doğrulaması oluşturma, test etme ve kullanma adımlarında size yol sunar.
services: automation
ms.subservice: process-automation
ms.date: 01/15/2019
ms.topic: conceptual
ms.openlocfilehash: 3a885f071c89ff6d9bb79d908b19c9451b4ed735
ms.sourcegitcommit: ea006cd8e62888271b2601d5ed4ec78fb40e8427
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81383314"
---
# <a name="create-a-standalone-azure-automation-account"></a>Tek başına Azure Otomasyonu hesabı oluşturma

Bu makalede, Azure portalında nasıl bir Azure Otomasyon hesabı oluşturabileceğiniz gösterilmektedir. Ek yönetim çözümleri veya Azure Monitor günlükleriyle tümleştirme kullanmadan Otomasyon'u değerlendirmek ve öğrenmek için portal Otomasyon hesabını kullanabilirsiniz. Bu yönetim çözümlerini ekleyebilir veya gelecekte herhangi bir noktada runbook işlerinin gelişmiş izlenmesi için Azure Monitor günlükleriyle tümleştirebilirsiniz.

Otomasyon hesabıyla, Azure Kaynak Yöneticisi veya klasik dağıtım modelindeki kaynakları yöneterek runbook'ların kimliğini doğrulayabilirsiniz. Bir Otomasyon Hesabı belirli bir kiracı için kaynakları tüm bölgelerde ve aboneliklerde yönetebilir.

Azure portalında bir Otomasyon hesabı oluşturduğunuzda, bu hesaplar otomatik olarak oluşturulur:

* **Hesap Olarak çalıştırın.** Bu hesap aşağıdaki görevleri yapar:
  * Azure Etkin Dizin (Azure AD)'nde bir hizmet ilkesi oluşturur.
  * Bir sertifika oluşturur.
  * Azure Kaynak Yöneticisi kaynaklarını runbook'ları kullanarak yöneten Katılımcı RolüTabanlı Erişim Denetimi'ni (RBAC) atar.

Sizin için oluşturulan bu hesaplarla, otomasyon ihtiyaçlarınızı desteklemek için çalışma defterleri oluşturmaya ve dağıtmaya hızlı bir şekilde başlayabilirsiniz.

## <a name="permissions-required-to-create-an-automation-account"></a>Otomasyon hesabı oluşturmak için gereken izinler

Bir Otomasyon hesabı oluşturmak veya güncelleştirmek ve bu makalede açıklanan görevleri tamamlamak için aşağıdaki ayrıcalıklara ve izinlere sahip olmalısınız:

* Bir Otomasyon hesabı oluşturmak için Azure AD kullanıcı hesabınızın Microsoft'un Sahibi rolüne eşdeğer izinlere sahip bir role eklenmesi **gerekir. Otomasyon** kaynakları. Daha fazla bilgi için Azure [Otomasyonunda Rol Tabanlı Erişim Denetimi'ne](automation-role-based-access-control.md)bakın.
* Azure portalında, **Azure Active Directory** > **MANAGE** > **Kullanıcı ayarları**altında, Uygulama **kayıtları** **Evet**olarak ayarlanmışsa, Azure AD kiracınızdaki yönetici olmayan kullanıcılar [Active Directory uygulamalarını kaydedebilir.](../active-directory/develop/howto-create-service-principal-portal.md#check-azure-subscription-permissions) **Uygulama kayıtları** **Hayır**olarak ayarlanmışsa, bu eylemi gerçekleştiren kullanıcının Azure AD'de genel bir yönetici olması gerekir.

Aboneliğin genel yönetici/yardımcı yönetici rolüne eklenmeden önce aboneliğin Active Directory örneğinin bir üyesi değilseniz, Etkin Dizini'ne konuk olarak eklenirsiniz. Bu senaryoda, Bu iletiyi **Ek Otomasyon Hesabı** sayfasında görürsünüz: "Oluşturma izniniz yoktur."

Bir kullanıcı önce genel yönetici/yardımcı yönetici rolüne eklenirse, bunları aboneliğin Etkin Dizin örneğinden kaldırabilir ve ardından Etkin Dizini'nde tam Kullanıcı rolüne göre okuyabilirsiniz.

Kullanıcı rollerini doğrulamak için:

1. Azure portalında **Azure Etkin Dizin** bölmesine gidin.
1. **Kullanıcıları ve grupları**seçin.
1. **Tüm kullanıcıları**seçin.
1. Belirli bir kullanıcıyı seçtikten sonra **Profil'i**seçin. Kullanıcı profili altında **Kullanıcı türü** özniteliğinin değeri **Konuk**olmamalıdır.

## <a name="create-a-new-automation-account-in-the-azure-portal"></a>Azure portalında yeni bir Otomasyon hesabı oluşturma

Azure portalında bir Azure Otomasyon hesabı oluşturmak için aşağıdaki adımları tamamlayın:

1. Abonelik Yöneticileri rolünün üyesi ve aboneliğin yardımcı yöneticisi olan bir hesapla Azure portalında oturum açın.
1. Seçin **+ Kaynak Oluştur**.
1. **Otomasyon**ara . Arama sonuçlarında **Otomasyon'u**seçin.

   ![Azure Marketi'nde Otomasyon & Denetimi'ni arayın ve seçin](media/automation-create-standalone-account/automation-marketplace-select-create-automationacct.png)

1. Sonraki ekranda **Oluştur'u**seçin.

   ![Otomasyon hesabı ekle](media/automation-create-standalone-account/automation-create-automationacct-properties.png)

   > [!NOTE]
   > Aşağıdaki iletiyi Otomasyon **Hesabı Ekle** bölmesinde görürseniz, hesabınız abonelik Yöneticileri rolünün bir üyesi ve aboneliğin yardımcı yöneticisi değildir.
   >
   > ![Otomasyon hesap uyarısı ekle](media/automation-create-standalone-account/create-account-without-perms.png)

1. Ek **Otomasyon Hesabı** bölmesinde, **Ad** kutusuna yeni Otomasyon hesabınız için bir ad girin. Bu ad seçildikten sonra değiştirilemez. *Otomasyon Hesabı adları bölge ve kaynak grubuna göre benzersizdir. Silinen Otomasyon Hesapları nın adları hemen kullanılamayabilir.*
1. **Abonelik** kutusunda birden fazla aboneliğiniz varsa, yeni hesap için kullanmak istediğiniz aboneliği belirtin.
1. **Kaynak grubu**için, yeni veya varolan bir kaynak grubu girin veya seçin.
1. **Konum**için bir Azure veri merkezi konumu seçin.
1. Azure **Run As hesabı** için Evet **seçildiğinden** emin olun ve ardından **Oluştur'u**seçin.

   > [!NOTE]
   > Azure Çalıştır Hesabı Olarak **Oluştur'a Hayır'ı** seçerek Çalıştır hesabı oluşturmamayı seçerseniz, **Hesap Ekle** bölmesinde bir ileti görüntülenir. **Create Azure Run As account** Hesap Azure portalında oluşturulsa da, hesabın klasik dağıtım modeli aboneliğinizde veya Azure Kaynak Yöneticisi abonelik dizini hizmetinde karşılık gelen bir kimlik kimliği yoktur. Bu nedenle, Otomasyon hesabının aboneliğinizdeki kaynaklara erişimi yoktur. Bu, bu hesaba başvuran tüm runbook'ların bu dağıtım modellerindeki kaynaklara karşı görevleri doğrulamasını ve gerçekleştirebilmelerini önler.
   >
   > ![Otomasyon hesap uyarısı ekle](media/automation-create-standalone-account/create-account-decline-create-runas-msg.png)
   >
   > Hizmet sorumlusu oluşturulmadı, Katılımcı rolü atanmaz.
   >

1. Otomasyon hesabı oluşturma nın ilerlemesini izlemek için menüde **Bildirimler'i**seçin.

### <a name="resources-included"></a>Kaynaklar dahil

Otomasyon hesabı başarıyla oluşturulduğunda bazı kaynaklar sizin için otomatik olarak oluşturulur. Oluşturma dan sonra, bu runbook'lar saklamak istemiyorsanız güvenle silinebilir. Hesap Olarak Çalıştır, bir runbook'ta hesabınıza kimlik doğrulamak için kullanılabilir ve başka bir hesap oluşturmadığınız veya gerektirmediğiniz sürece bırakılmalıdır. Aşağıdaki tabloda Farklı Çalıştır hesabının kaynakları özetlenmektedir.

| Kaynak | Açıklama |
| --- | --- |
| AzureAutomationTutorial Runbook |Çalıştır hesabı nı kullanarak kimlik doğrulamasını gösteren örnek bir grafik çalışma kitabı. Runbook tüm Kaynak Yöneticisi kaynaklarını alır. |
| AzureAutomationTutorialScript Runbook |Run As hesabını kullanarak kimlik doğrulamasını gösteren örnek bir PowerShell runbook. Runbook tüm Kaynak Yöneticisi kaynaklarını alır. |
| AzureAutomationTutorialPython2 Runbook |Run As hesabını kullanarak kimlik doğrulamasını gösteren örnek bir Python runbook. Runbook, abonelikte bulunan tüm kaynak gruplarını listeler. |
| AzureRunAsCertificate |Otomasyon hesabı oluşturulduğunda veya varolan bir hesap için PowerShell komut dosyası kullanılarak otomatik olarak oluşturulan bir sertifika kıymeti. Sertifika, Azure ile kimlik doğrulaması yaparak Azure Kaynak Yöneticisi kaynaklarını runbook'lardan yönetebilirsiniz. Bu sertifikanın bir yıllık kullanım ömrü vardır. |
| AzureRunAsConnection |Otomasyon hesabı oluşturulduğunda veya varolan bir hesap için PowerShell komut dosyası kullanılarak otomatik olarak oluşturulan bir bağlantı varlığı. |

## <a name="create-a-classic-run-as-account"></a>Hesap Olarak Klasik Çalıştır Oluşturma

Bir Azure Otomasyon hesabı oluşturduğunuzda, Klasik Çalıştır Hesapları artık varsayılan olarak oluşturulmaz. Hala bir Klasik Çalıştır Hesabı gerekiyorsa:

1. Otomasyon hesabınızdan **Hesap Ayarları**altında Hesap **Olarak Çalıştır'ı** seçin.
2. **Hesap Olarak Azure Klasik Çalıştır'ı**seçin.
3. Klasik Çalıştır Hesabı Oluşturma'ya devam etmek için **Oluştur'u** tıklatın.

## <a name="next-steps"></a>Sonraki adımlar

* Grafik yazma hakkında daha fazla bilgi edinmek için [Azure Otomasyonu'nda Grafik yazma'ya](automation-graphical-authoring-intro.md)bakın.
* PowerShell runbook'ları ile başlamak için [ilk PowerShell runbook'uma](automation-first-runbook-textual-powershell.md)bakın.
* PowerShell iş akışı runbook'larına başlamak için [ilk PowerShell iş akışı çalışma kitabıma](automation-first-runbook-textual.md)bakın.
* Python 2 runbook'ları ile başlamak için [ilk Python2 runbook'uma](automation-first-runbook-textual-python2.md)bakın.
* PowerShell cmdlet referansı için [Az.Automation'a](https://docs.microsoft.com/powershell/module/az.automation/?view=azps-3.7.0#automation
)bakın.
