---
title: Azure Otomasyonu farklı çalıştır hesabını yönetme
description: Bu makalede, farklı çalıştır hesabınızı PowerShell ile veya Azure portal nasıl yöneteceğiniz açıklanmaktadır.
services: automation
ms.subservice: shared-capabilities
ms.date: 06/26/2020
ms.topic: conceptual
ms.openlocfilehash: 0d854c8815ff6acd54036e023b35c3b3ac2c56cf
ms.sourcegitcommit: 74ba70139781ed854d3ad898a9c65ef70c0ba99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/26/2020
ms.locfileid: "85444531"
---
# <a name="manage-an-azure-automation-run-as-account"></a>Azure Otomasyonu farklı çalıştır hesabını yönetme

Azure Otomasyonu 'nda farklı çalıştır hesapları Azure cmdlet 'lerini kullanarak Azure 'da kaynak yönetimine yönelik kimlik doğrulaması sağlar. Farklı Çalıştır hesabı oluşturduğunuzda, Azure Active Directory (AD) içinde yeni bir hizmet sorumlusu kullanıcısı oluşturur ve bu kullanıcıya abonelik düzeyinde katılımcı rolü atar.

## <a name="types-of-run-as-accounts"></a>Farklı Çalıştır hesabı türleri

Azure Otomasyonu iki farklı çalıştır hesabı türü kullanır:

* Azure farklı çalıştır hesabı
* Azure klasik farklı çalıştır hesabı

>[!NOTE]
>Azure bulut çözümü sağlayıcısı (CSP) abonelikleri yalnızca Azure Resource Manager modelini destekler. Azure Resource Manager olmayan hizmetler programda yok. CSP aboneliği kullanırken, Azure klasik farklı çalıştır hesabı oluşturulmaz, ancak Azure farklı çalıştır hesabı oluşturulur. CSP abonelikleri hakkında daha fazla bilgi edinmek için bkz. [CSP aboneliklerinde kullanılabilir hizmetler](https://docs.microsoft.com/azure/cloud-solution-provider/overview/azure-csp-available-services).

Farklı Çalıştır hesabı için hizmet sorumlusu varsayılan olarak Azure AD 'yi okuma izinlerine sahip değildir. Azure AD 'yi okuma veya yönetme izinleri eklemek istiyorsanız, **API izinleri**altında hizmet sorumlusu için izinleri vermeniz gerekir. Daha fazla bilgi için bkz. [Web API 'lerine erişim Izinleri ekleme](../active-directory/develop/quickstart-configure-app-access-web-apis.md#add-permissions-to-access-web-apis).

### <a name="run-as-account"></a>Farklı Çalıştır hesabı

Farklı Çalıştır hesabı, [Kaynak Yöneticisi dağıtım modeli](../azure-resource-manager/management/deployment-models.md) kaynaklarını yönetir. Aşağıdaki görevleri yapar.

* Otomatik olarak imzalanan bir sertifika ile Azure AD uygulaması oluşturur, Azure AD’de bu uygulama için bir hizmet sorumlusu hesabı oluşturur ve geçerli aboneliğinizde hesap için Katkıda Bulunan rolünü atar. Sertifika ayarını sahip veya başka herhangi bir rol olarak değiştirebilirsiniz. Daha fazla bilgi için bkz. [Azure Otomasyonu’nda rol tabanlı erişim denetimi](automation-role-based-access-control.md).
  
* Belirtilen Otomasyon hesabında adlı bir Otomasyon sertifikası varlığı oluşturur `AzureRunAsCertificate` . Sertifika varlığı, Azure AD uygulaması tarafından kullanılan sertifika özel anahtarını içerir.
  
* Belirtilen Otomasyon hesabında adlı bir Otomasyon bağlantı varlığı oluşturur `AzureRunAsConnection` . Bağlantı varlığı uygulama KIMLIĞI, kiracı KIMLIĞI, abonelik KIMLIĞI ve sertifika parmak izini barındırır.

### <a name="azure-classic-run-as-account"></a>Azure Klasik Farklı Çalıştır Hesabı

Klasik Azure farklı çalıştır hesabı [klasik dağıtım modeli](../azure-resource-manager/management/deployment-models.md) kaynaklarını yönetir. Bu hesap türünü oluşturmak veya yenilemek için abonelikte ortak yönetici olmanız gerekir.

Azure klasik farklı çalıştır hesabı aşağıdaki görevleri gerçekleştirir.

  * Abonelikte bir yönetim sertifikası oluşturur.

  * Belirtilen Otomasyon hesabında adlı bir Otomasyon sertifikası varlığı oluşturur `AzureClassicRunAsCertificate` . Sertifika varlığı, yönetim sertifikası tarafından kullanılan sertifika özel anahtarını içerir.

  * Belirtilen Otomasyon hesabında adlı bir Otomasyon bağlantı varlığı oluşturur `AzureClassicRunAsConnection` . Bağlantı varlığı, abonelik adı, abonelik KIMLIĞI ve sertifika varlık adını içerir.

>[!NOTE]
>Azure klasik farklı çalıştır hesabı, bir Otomasyon hesabı oluştururken aynı anda varsayılan olarak oluşturulmaz. Bu hesap, bu makalenin ilerleyen kısımlarında açıklanan adımları izleyerek tek tek oluşturulur.

## <a name="obtain-run-as-account-permissions"></a><a name="permissions"></a>Farklı Çalıştır hesabı izinlerini alma

Bu bölümde hem normal farklı Çalıştır hesaplarının hem de klasik Farklı Çalıştır hesaplarının izinleri tanımlanmaktadır.

### <a name="get-permissions-to-configure-run-as-accounts"></a>Farklı Çalıştır hesaplarını yapılandırma izinleri alın

Farklı Çalıştır hesabı oluşturmak veya güncelleştirmek için, belirli ayrıcalıklara ve izinlere sahip olmanız gerekir. Azure Active Directory ve bir abonelikteki bir sahip uygulama Yöneticisi tüm görevleri tamamlayabilir. Görevlerinin ayrılmanız durumunda aşağıdaki tabloda, görevlerin bir listesi, eşdeğer cmdlet ve gerekli izinler gösterilmektedir:

|Görev|Cmdlet  |Minimum Izinler  |İzinleri ayarladığınız yer|
|---|---------|---------|---|
|Azure AD uygulaması oluşturma|[New-AzADApplication](https://docs.microsoft.com/powershell/module/az.resources/new-azadapplication)     | Uygulama geliştirici rolü<sup>1</sup>        |[Azure AD](../active-directory/develop/howto-create-service-principal-portal.md#required-permissions)</br>Azure AD > uygulama kayıtlarını > giriş |
|Uygulamaya bir kimlik bilgisi ekleyin.|[New-AzADAppCredential](https://docs.microsoft.com/powershell/module/az.resources/new-azadappcredential)     | Uygulama Yöneticisi veya genel yönetici<sup>1</sup>         |[Azure AD](../active-directory/develop/howto-create-service-principal-portal.md#required-permissions)</br>Azure AD > uygulama kayıtlarını > giriş|
|Azure AD hizmet sorumlusu oluşturma ve edinme|[New-AzADServicePrincipal](https://docs.microsoft.com/powershell/module/az.resources/new-azadserviceprincipal)</br>[Get-AzADServicePrincipal](https://docs.microsoft.com/powershell/module/az.resources/get-azadserviceprincipal)     | Uygulama Yöneticisi veya genel yönetici<sup>1</sup>        |[Azure AD](../active-directory/develop/howto-create-service-principal-portal.md#required-permissions)</br>Azure AD > uygulama kayıtlarını > giriş|
|Belirtilen sorumlu için RBAC rolünü ata veya al|[New-AzRoleAssignment](https://docs.microsoft.com/powershell/module/az.resources/new-azroleassignment)</br>[Get-AzRoleAssignment](https://docs.microsoft.com/powershell/module/Az.Resources/Get-AzRoleAssignment)      | Kullanıcı erişimi Yöneticisi veya sahibi ya da aşağıdaki izinlere sahip:</br></br><code>Microsoft.Authorization/Operations/read</br>Microsoft.Authorization/permissions/read</br>Microsoft.Authorization/roleDefinitions/read</br>Microsoft.Authorization/roleAssignments/write</br>Microsoft.Authorization/roleAssignments/read</br>Microsoft.Authorization/roleAssignments/delete</code></br></br> | [Abonelik](../role-based-access-control/role-assignments-portal.md)</br>Giriş > abonelikleri > \<subscription name\> -Access Control (IAM)|
|Otomasyon sertifikası oluşturma veya kaldırma|[New-AzAutomationCertificate](https://docs.microsoft.com/powershell/module/Az.Automation/New-AzAutomationCertificate)</br>[Remove-AzAutomationCertificate](https://docs.microsoft.com/powershell/module/az.automation/remove-azautomationcertificate)     | Kaynak grubunda katkıda bulunan         |Otomasyon hesabı kaynak grubu|
|Otomasyon bağlantısı oluşturma veya kaldırma|[New-AzAutomationConnection](https://docs.microsoft.com/powershell/module/az.automation/new-azautomationconnection)</br>[Remove-AzAutomationConnection](https://docs.microsoft.com/powershell/module/az.automation/remove-azautomationconnection)|Kaynak grubunda katkıda bulunan |Otomasyon hesabı kaynak grubu|

<sup>1</sup> Azure AD kiracınızdaki yönetici olmayan kullanıcılar, Kullanıcı ayarları sayfasında **uygulamalar KAYDEDEBILIYORSANıZ** , [ad uygulamalarını kayıt](../active-directory/develop/howto-create-service-principal-portal.md#required-permissions) **edebilir.** Uygulama kayıt ayarı **Hayır**ise, bu eylemi gerçekleştiren kullanıcının bu tabloda tanımlanmış olması gerekir.

Aboneliğin genel yönetici rolüne eklenmeden önce aboneliğin Active Directory örneğinin bir üyesi değilseniz, konuk olarak eklendiniz. Bu durumda, `You do not have permissions to create…` **Otomasyon hesabı ekle** sayfasında bir uyarı alırsınız.

Genel yönetici rolü atandığında aboneliğin Active Directory örneğinin bir üyesiyseniz, `You do not have permissions to create…` **Otomasyon hesabı ekle** sayfasında de bir uyarı alabilirsiniz. Bu durumda, aboneliğin Active Directory örneğinden kaldırma isteğinde bulunabilir ve sonra, Active Directory bir tam Kullanıcı olacak şekilde yeniden eklenmeleri istenir.

Hata iletisini üreten durumun giderilmiş olduğunu doğrulamak için:

1. Azure portal Azure Active Directory bölmesinden **Kullanıcılar ve gruplar**' ı seçin.
2. **Tüm kullanıcılar**' ı seçin.
3. Adınızı seçip **profil**' i seçin. 
4. Kullanıcı profili altındaki **Kullanıcı türü** özniteliğinin değerinin **Konuk**olarak ayarlı olmadığından emin olun.

### <a name="get-permissions-to-configure-classic-run-as-accounts"></a><a name="permissions-classic"></a>Klasik farklı çalıştır hesaplarını yapılandırma izinleri alın

Klasik farklı çalıştır hesaplarını yapılandırmak veya yenilemek için, abonelik düzeyinde ortak yönetici rolüne sahip olmanız gerekir. Klasik abonelik izinleri hakkında daha fazla bilgi için bkz. [Azure klasik abonelik yöneticileri](../role-based-access-control/classic-administrators.md#add-a-co-administrator).

## <a name="create-a-run-as-account-in-azure-portal"></a>Azure portal farklı çalıştır hesabı oluşturma

Azure portal Azure Otomasyonu hesabınızı güncelleştirmek için aşağıdaki adımları gerçekleştirin. Farklı Çalıştır ve klasik farklı çalıştır hesaplarını ayrı ayrı oluşturun. Klasik kaynak oluşturmanıza gerek yoksa yalnızca Azure Farklı Çalıştır hesabını oluşturabilirsiniz.

1. Azure portalında Abonelik Yöneticileri rolünün üyesi ve aboneliğin ortak yöneticisi olan bir hesapla oturum açın.

2. **Otomasyon hesaplarını**arayın ve seçin.

3. Otomasyon hesapları sayfasında, listeden Otomasyon hesabınızı seçin.

4. Sol bölmede hesap ayarları bölümünde **Farklı Çalıştır hesapları** ' nı seçin.

5. Gereken hesaba bağlı olarak **Azure Farklı Çalıştır Hesabı**’nı veya **Azure Klasik Farklı Çalıştır Hesabı**’nı seçin. 

6. İlgilendiğiniz hesaba bağlı olarak, **Azure farklı çalıştır** veya **Azure klasik farklı çalıştır hesabı** Ekle bölmesini kullanın. Genel bakış bilgilerini inceledikten sonra **Oluştur**' a tıklayın.

7. Azure Farklı Çalıştır hesabını oluşturduğu sırada menünün **Bildirimler** öğesi altında ilerleme durumunu izleyebilirsiniz. Hesabın oluşturulduğunu belirten bir başlık de görüntülenir. İşlemin tamamlanması birkaç dakika sürebilir.

## <a name="delete-a-run-as-or-classic-run-as-account"></a>Farklı Çalıştır veya Klasik Farklı Çalıştır hesabını silme

Bu bölümde, farklı çalıştır veya klasik farklı çalıştır hesabının nasıl silineceği açıklanmaktadır. Bu eylemi gerçekleştirdiğinizde Otomasyon hesabı korunur. Hesabı sildikten sonra Azure portal yeniden oluşturabilirsiniz.

1. Azure portalında Otomasyon hesabınızı açın.

2. Sol bölmede hesap ayarları bölümünde **Farklı Çalıştır hesapları** ' nı seçin.

3. Farklı Çalıştır Hesapları özellikleri sayfasında silmek istediğiniz Farklı Çalıştır Hesabını veya Klasik Farklı Çalıştır Hesabını seçin. 

4. Seçili hesabın Özellikler bölmesinde **Sil**' e tıklayın.

   ![Farklı Çalıştır hesabını silme](media/manage-runas-account/automation-account-delete-runas.png)

5. Hesap silinirken menünün **Bildirimler** öğesi altında ilerleme durumunu izleyebilirsiniz.

6. Hesap silindikten sonra Farklı Çalıştır Hesapları özellikler sayfasında **Azure Farklı Çalıştır Hesabı** seçeneğini belirleyerek hesabı yeniden oluşturabilirsiniz.

   ![Otomasyon Farklı Çalıştır hesabını yeniden oluşturma](media/manage-runas-account/automation-account-create-runas.png)

## <a name="renew-a-self-signed-certificate"></a><a name="cert-renewal"></a>Kendinden imzalı bir sertifikayı yenileme

Farklı Çalıştır hesabı için oluşturduğunuz otomatik olarak imzalanan sertifika, oluşturma tarihinden itibaren bir yıl sonra dolar. Farklı Çalıştır hesabınızın süresi dolmadan önce sertifikayı yenilemeniz gerekir. Bu süreyi, süresi dolmadan önce yenileyebilirsiniz. 

Otomatik olarak imzalanan sertifikayı yenilediğinizde, kuyruğa alınan veya etkin olarak çalışan ve farklı çalıştır hesabıyla kimlik doğrulaması yapılan tüm runbook 'ların olumsuz şekilde etkilenmemesini sağlamak için geçerli geçerli sertifika tutulur. Sertifika, sona erme tarihine kadar geçerliliğini sürdürür.

>[!NOTE]
>Farklı çalıştır hesabının tehlikede olduğunu düşünüyorsanız, otomatik olarak imzalanan sertifikayı silip yeniden oluşturabilirsiniz.

>[!NOTE]
>Farklı Çalıştır hesabınızı, kuruluş sertifika yetkiliniz tarafından verilen bir sertifika kullanmak üzere yapılandırdıysanız ve otomatik olarak imzalanan bir sertifika seçeneğini yenileme seçeneğini kullanırsanız, kurumsal sertifika otomatik olarak imzalanan bir sertifika ile değiştirilmiştir.

Otomatik olarak imzalanan sertifikayı yenilemek için aşağıdaki adımları kullanın.

1. Azure portalında Otomasyon hesabınızı açın.

1. Hesap ayarları bölümünde **Farklı Çalıştır hesapları** ' nı seçin.

    ![Otomasyon hesabı özellikleri bölmesi](media/manage-runas-account/automation-account-properties-pane.png)

1. Farklı Çalıştır hesapları özellikleri sayfasında, sertifikayı yenilemek istediğiniz farklı çalıştır hesabını veya klasik farklı çalıştır hesabını seçin.

1. Seçili hesabın Özellikler bölmesinde **Sertifikayı Yenile**' ye tıklayın.

    ![Farklı Çalıştır hesabının sertifikasını yenileme](media/manage-runas-account/automation-account-renew-runas-certificate.png)

1. Sertifika yenilenirken menünün **Bildirimler** öğesi altında ilerleme durumunu izleyebilirsiniz.

## <a name="limit-run-as-account-permissions"></a>Farklı Çalıştır hesabı izinlerini sınırla

Azure 'daki kaynaklara karşı Otomasyon hedeflemesini denetlemek için [Update-AutomationRunAsAccountRoleAssignments.ps1](https://aka.ms/AA5hug8) betiğini çalıştırabilirsiniz. Bu betik, özel bir rol tanımı oluşturmak ve kullanmak için mevcut farklı çalıştır hesabı hizmet sorumlunuzu değiştirir. Rolün [Key Vault](https://docs.microsoft.com/azure/key-vault/)hariç tüm kaynaklar için izinleri vardır.

>[!IMPORTANT]
>**Update-AutomationRunAsAccountRoleAssignments.ps1** betiğini çalıştırdıktan sonra, Farklı Çalıştır hesaplarının kullanımı üzerinden Key Vault erişen runbook 'lar artık çalışmaz. Betiği çalıştırmadan önce, Azure Key Vault çağrılar için hesabınızdaki runbook 'ları gözden geçirmeniz gerekir. Azure Otomasyonu runbook 'lardan Key Vault erişimi etkinleştirmek için, [Farklı Çalıştır hesabını Key Vault izinlerine eklemeniz](#add-permissions-to-key-vault)gerekir.

Farklı Çalıştır hizmeti sorumlusunu kısıtlamak istiyorsanız, `NotActions` özel rol tanımının öğesine başka kaynak türleri de ekleyebilirsiniz. Aşağıdaki örnek, erişimini kısıtlar `Microsoft.Compute/*` . Bu kaynak türünü `NotActions` rol tanımı için öğesine eklerseniz, rol herhangi bir işlem kaynağına erişemez. Rol tanımları hakkında daha fazla bilgi edinmek için bkz. [Azure kaynakları için rol tanımlarını anlama](../role-based-access-control/role-definitions.md).

```powershell
$roleDefinition = Get-AzRoleDefinition -Name 'Automation RunAs Contributor'
$roleDefinition.NotActions.Add("Microsoft.Compute/*")
$roleDefinition | Set-AzRoleDefinition
```

Farklı Çalıştır hesabınız tarafından kullanılan hizmet sorumlusunun katkıda bulunan rol tanımında mi yoksa özel bir tane mi olduğunu belirleyebilirsiniz. 

1. Otomasyon hesabınıza gidin ve hesap ayarları bölümünde **Farklı Çalıştır hesapları** ' nı seçin.
2. **Azure farklı çalıştır hesabı**' nı seçin. 
3. Kullanılmakta olan rol tanımını bulmak için **rol** ' i seçin.

[![](media/manage-runas-account/verify-role.png "Verify the Run As Account role")](media/manage-runas-account/verify-role-expanded.png#lightbox)

Farklı Çalıştır hesapları tarafından birden çok abonelik veya Otomasyon hesabı için kullanılan rol tanımını da belirleyebilirsiniz. Bunu PowerShell Galerisi [Check-AutomationRunAsAccountRoleAssignments.ps1](https://aka.ms/AA5hug5) betiği kullanarak yapın.

### <a name="add-permissions-to-key-vault"></a>Key Vault izinler ekleme

Key Vault ve farklı çalıştır hesabı hizmet sorumlunun özel bir rol tanımı kullandığını doğrulamak için Azure Otomasyonu 'na izin verebilirsiniz. Şunları yapmanız gerekir:

* Key Vault izin verin.
* Erişim ilkesini ayarlayın.

Farklı Çalıştır hesabı izinlerini Key Vault sağlamak için PowerShell Galerisi [Extend-AutomationRunAsAccountRoleAssignmentToKeyVault.ps1](https://aka.ms/AA5hugb) betiğini kullanabilirsiniz. Key Vault izinlerini ayarlama hakkında daha fazla bilgi için bkz. [uygulamalara bir Anahtar Kasası erişimi verme](../key-vault/general/group-permissions-for-apps.md) .

## <a name="resolve-misconfiguration-issues-for-run-as-accounts"></a>Farklı Çalıştır hesapları için yanlış yapılandırma sorunlarını çözün

Farklı Çalıştır veya klasik farklı çalıştır hesabı için gerekli bazı yapılandırma öğeleri, ilk kurulum sırasında silinmiş veya hatalı oluşturulmuş olabilir. Hatalı yanlış yapılandırma örnekleri şunları içerir:

* Sertifika varlığı
* Bağlantı varlığı
* Farklı Çalıştır hesabı, katkıda bulunan rolünden kaldırıldı
* Azure AD'de hizmet sorumlusu veya uygulama

Bu tür yanlış yapılandırma örnekleri için, Otomasyon hesabı değişiklikleri algılar ve hesabın farklı çalıştır hesapları Özellikler bölmesinde *tamamlanmamış* durumunu görüntüler.

![Tamamlanmamış Farklı Çalıştır hesabı yapılandırma durumu](media/manage-runas-account/automation-account-runas-incomplete-config.png)

Farklı Çalıştır hesabını seçtiğinizde, hesap özellikleri bölmesinde aşağıdaki hata iletisi görüntülenir:

```text
The Run As account is incomplete. Either one of these was deleted or not created - Azure Active Directory Application, Service Principal, Role, Automation Certificate asset, Automation Connect asset - or the Thumbprint is not identical between Certificate and Connection. Please delete and then re-create the Run As Account.
```

Hesabı silip yeniden oluşturarak Farklı Çalıştır hesabıyla ilgili bu sorunları hızlı bir şekilde çözebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [Uygulama nesneleri ve hizmet sorumlusu nesneleri](../active-directory/develop/app-objects-and-service-principals.md).
* [Azure Cloud Services sertifikalara genel bakış](../cloud-services/cloud-services-certs-create.md).
