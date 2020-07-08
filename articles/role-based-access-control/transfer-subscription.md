---
title: Azure aboneliğini farklı bir Azure AD dizinine aktarma (Önizleme)
description: Azure aboneliğini ve bilinen ilgili kaynakları farklı bir Azure Active Directory (Azure AD) dizinine aktarmayı öğrenin.
services: active-directory
author: rolyon
manager: mtillman
ms.service: role-based-access-control
ms.devlang: na
ms.topic: how-to
ms.workload: identity
ms.date: 07/01/2020
ms.author: rolyon
ms.openlocfilehash: f169cf45702d4a5051f9f6908b77c645c7a0018f
ms.sourcegitcommit: e132633b9c3a53b3ead101ea2711570e60d67b83
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2020
ms.locfileid: "86042399"
---
# <a name="transfer-an-azure-subscription-to-a-different-azure-ad-directory-preview"></a>Azure aboneliğini farklı bir Azure AD dizinine aktarma (Önizleme)

> [!IMPORTANT]
> Aboneliği farklı bir Azure AD dizinine aktarmak için şu adımları takip etmek şu anda genel önizlemededir.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Kuruluşların çeşitli Azure abonelikleri olabilir. Her abonelik belirli bir Azure Active Directory (Azure AD) diziniyle ilişkilendirilir. Yönetimi kolaylaştırmak için, farklı bir Azure AD dizinine bir abonelik aktarmak isteyebilirsiniz. Bir aboneliği farklı bir Azure AD dizinine aktardığınızda, bazı kaynaklar hedef dizine aktarılmaz. Örneğin, Azure rol tabanlı erişim denetimindeki (Azure RBAC) tüm rol atamaları ve özel roller, kaynak dizinden **kalıcı olarak** silinir ve hedef dizine aktarılmaz.

Bu makalede, bir aboneliği farklı bir Azure AD dizinine aktarmak ve aktarımdan sonra bazı kaynakları yeniden oluşturmak için izleyebileceğiniz temel adımlar açıklanmaktadır.

## <a name="overview"></a>Genel Bakış

Azure aboneliğini farklı bir Azure AD dizinine aktarmak, dikkatlice planlanmalıdır ve yürütülmesi gereken karmaşık bir işlemdir. Birçok Azure hizmeti, normal şekilde çalışması için güvenlik sorumlularını (kimlikler) gerektirir veya diğer Azure kaynaklarını yönetebilir. Bu makale, güvenlik ilkelerine büyük ölçüde bağlı olan, ancak kapsamlı olmayan Azure hizmetlerinin çoğunu kapsamaya çalışır.

> [!IMPORTANT]
> Bir aboneliğin aktarılması işlemi tamamlaması için kapalı kalma süresi gerekir.

Aşağıdaki diyagramda, bir aboneliği farklı bir dizine aktardığınızda izlemeniz gereken temel adımlar gösterilmektedir.

1. Aktarım için hazırlanma

1. Azure aboneliğinin faturalama sahipliğini başka bir hesaba aktarma

1. Hedef dizinde rol atamaları, özel roller ve yönetilen kimlikler gibi kaynakları yeniden oluşturun

    ![Aktarım aboneliği diyagramı](./media/transfer-subscription/transfer-subscription.png)

### <a name="deciding-whether-to-transfer-a-subscription-to-a-different-directory"></a>Bir aboneliğin farklı bir dizine aktarılmaya karar verme

Abonelik aktarmak isteyebileceğiniz bazı nedenler aşağıda verilmiştir:

- Bir şirket merceği veya alımı nedeniyle, birincil Azure AD dizininizde alınan bir aboneliği yönetmek istiyorsunuz.
- Kuruluşunuzdaki birisi bir abonelik oluşturdu ve yönetimi belirli bir Azure AD dizini ile birleştirmek istiyorsunuz.
- Belirli bir abonelik KIMLIĞINE veya URL 'ye bağlı olan uygulamalarınız vardır ve uygulama yapılandırmasını veya kodunu değiştirmek kolay değildir.
- İşletmenizin bir kısmı ayrı bir şirkete bölünür ve kaynaklarınızın bazılarını farklı bir Azure AD dizinine taşımanız gerekir.
- Güvenlik yalıtımı amacıyla, farklı bir Azure AD dizininde kaynaklarınızın bazılarını yönetmek istiyorsunuz.

Bir aboneliğin aktarılması işlemi tamamlaması için kapalı kalma süresi gerekir. Senaryonuza bağlı olarak, kaynakları yeniden oluşturmak ve verileri hedef dizine ve aboneliğe kopyalamak daha iyi olabilir.

### <a name="understand-the-impact-of-transferring-a-subscription"></a>Abonelik aktarma etkisini anlayın

Birkaç Azure kaynağı bir aboneliğe veya dizine bağımlılığı vardır. Durumunuza bağlı olarak, aşağıdaki tabloda bir aboneliği aktarmanın bilinen etkileri listelenmektedir. Bu makaledeki adımları gerçekleştirerek, abonelik aktarımından önce var olan kaynakların bazılarını yeniden oluşturabilirsiniz.

> [!IMPORTANT]
> Bu bölüm, aboneliğinize bağlı olan bilinen Azure hizmetlerini veya kaynaklarını listeler. Azure 'daki kaynak türleri sürekli olarak geliştiğinden, ortamınızda önemli bir değişikliğe neden olabilecek ek bağımlılıklar olabilir. 

| Hizmet veya kaynak | Etkilendiğini | Gider | Etkilensin mi? | Yapabilecekleriniz |
| --------- | --------- | --------- | --------- | --------- |
| Rol atamaları | Yes | Yes | [Rol atamalarını listeleme](#save-all-role-assignments) | Tüm rol atamaları kalıcı olarak silinir. Kullanıcıları, grupları ve hizmet sorumlularını hedef dizindeki ilgili nesnelerle eşlemeniz gerekir. Rol atamalarını yeniden oluşturmanız gerekir. |
| Özel roller | Yes | Yes | [Özel rolleri listeleme](#save-custom-roles) | Tüm özel roller kalıcı olarak silinir. Özel rolleri ve tüm rol atamalarını yeniden oluşturmanız gerekir. |
| Sistem tarafından atanan Yönetilen kimlikler | Yes | Yes | [Yönetilen kimlikleri listeleme](#list-role-assignments-for-managed-identities) | Yönetilen kimlikleri devre dışı bırakıp yeniden etkinleştirmeniz gerekir. Rol atamalarını yeniden oluşturmanız gerekir. |
| Kullanıcı tarafından atanan Yönetilen kimlikler | Yes | Yes | [Yönetilen kimlikleri listeleme](#list-role-assignments-for-managed-identities) | Yönetilen kimlikleri silmeniz, yeniden oluşturmanız ve uygun kaynağa bağlamanız gerekir. Rol atamalarını yeniden oluşturmanız gerekir. |
| Azure Key Vault | Yes | Yes | [Key Vault erişim ilkelerini listeleme](#list-other-known-resources) | Anahtar kasaları ile ilişkili kiracı KIMLIĞINI güncelleştirmeniz gerekir. Yeni erişim ilkelerini kaldırmalı ve eklemeniz gerekir. |
| Azure AD kimlik doğrulaması ile Azure SQL veritabanları | Evet | Hayır | [Azure AD kimlik doğrulamasıyla Azure SQL veritabanlarını denetleme](#list-other-known-resources) |  |  |
| Azure depolama ve Azure Data Lake Storage 2. | Yes | Yes |  | Tüm ACL 'Leri yeniden oluşturmanız gerekir. |
| Azure Data Lake Storage 1. Nesil | Evet |  |  | Tüm ACL 'Leri yeniden oluşturmanız gerekir. |
| Azure Dosyaları | Yes | Yes |  | Tüm ACL 'Leri yeniden oluşturmanız gerekir. |
| Azure Dosya Eşitleme | Yes | Yes |  |  |
| Azure Yönetilen Diskleri | Evet | YOK |  |  |
| Kubernetes için Azure Kapsayıcı Hizmetleri | Yes | Yes |  |  |
| Azure Active Directory Domain Services | Evet | Hayır |  |  |
| Uygulama kayıtları | Evet | Evet |  |  |

Aktarılmakta olan abonelikte aynı abonelikte olmayan bir anahtar kasasına bağımlılığı olan bir depolama hesabı veya SQL veritabanı gibi bir kaynak için geri kalan şifrelemeyi kullanıyorsanız kurtarılamaz bir senaryoya yol açabilir. Bu durumda, başka bir anahtar kasası kullanmak veya bu kurtarılamaz senaryoyu önlemek için müşteri tarafından yönetilen anahtarları geçici olarak devre dışı bırakmak için gerekli adımları uygulamanız gerekir.

## <a name="prerequisites"></a>Ön koşullar

Bu adımları tamamlayabilmeniz için şunlar gerekir:

- Azure Cloud Shell veya [Azure CLI](https://docs.microsoft.com/cli/azure) ['da Bash](/azure/cloud-shell/overview)
- Kaynak dizinde aktarmak istediğiniz aboneliğin Hesap Yöneticisi
- Hedef dizinde [sahip](built-in-roles.md#owner) rolü

## <a name="step-1-prepare-for-the-transfer"></a>1. Adım: aktarım için hazırlanma

### <a name="sign-in-to-source-directory"></a>Kaynak dizinde oturum açın

1. Azure 'da yönetici olarak oturum açın.

1. [Az Account List](/cli/azure/account#az-account-list) komutuyla aboneliklerinizin bir listesini alın.

    ```azurecli
    az account list --output table
    ```

1. Aktarmak istediğiniz etkin aboneliği ayarlamak için [az Account set](https://docs.microsoft.com/cli/azure/account#az-account-set) kullanın.

    ```azurecli
    az account set --subscription "Marketing"
    ```

### <a name="install-the-resource-graph-extension"></a>Kaynak-grafik uzantısını yükler

 Kaynak Grafiği uzantısı, Azure Resource Manager tarafından yönetilen kaynakları sorgulamak için [az Graph](https://docs.microsoft.com/cli/azure/ext/resource-graph/graph) komutunu kullanmanıza olanak sağlar. Sonraki adımlarda bu komutu kullanacaksınız.

1. *Kaynak grafik* uzantısının yüklenip yüklenmediğini görmek için [az Extension List](https://docs.microsoft.com/cli/azure/extension#az-extension-list) kullanın.

    ```azurecli
    az extension list
    ```

1. Aksi takdirde, *kaynak grafik* uzantısını yükler.

    ```azurecli
    az extension add --name resource-graph
    ```

### <a name="save-all-role-assignments"></a>Tüm rol atamalarını Kaydet

1. Tüm rol atamalarını (devralınan rol atamaları dahil) listelemek için [az role atama listesini](https://docs.microsoft.com/cli/azure/role/assignment#az-role-assignment-list) kullanın.

    Listeyi gözden geçirmeyi kolaylaştırmak için çıktıyı JSON, TSV veya tablo olarak dışarı aktarabilirsiniz. Daha fazla bilgi için bkz. [Azure RBAC ve Azure CLI kullanarak rol atamalarını listeleme](role-assignments-list-cli.md).

    ```azurecli
    az role assignment list --all --include-inherited --output json > roleassignments.json
    az role assignment list --all --include-inherited --output tsv > roleassignments.tsv
    az role assignment list --all --include-inherited --output table > roleassignments.txt
    ```

1. Rol atamalarının listesini kaydedin.

    Bir aboneliği aktardığınızda, tüm rol atamaları **kalıcı olarak** silinir, böylece bir kopya kaydetmeniz önemlidir.

1. Rol atamalarının listesini gözden geçirin. Hedef dizinde ihtiyacınız olmayacak rol atamaları olabilir.

### <a name="save-custom-roles"></a>Özel rolleri Kaydet

1. Özel rollerinizi listelemek için [az role Definition listesini](https://docs.microsoft.com/cli/azure/role/definition#az-role-definition-list) kullanın. Daha fazla bilgi için bkz. [Azure CLI kullanarak Azure kaynakları için özel roller oluşturma veya güncelleştirme](custom-roles-cli.md).

    ```azurecli
    az role definition list --custom-role-only true --output json --query '[].{roleName:roleName, roleType:roleType}'
    ```

1. Hedef dizinde gerekli olacak her bir özel rolü ayrı bir JSON dosyası olarak kaydedin.

    ```azurecli
    az role definition list --name <custom_role_name> > customrolename.json
    ```

1. Özel rol dosyalarının kopyalarını oluşturun.

1. Her kopyayı aşağıdaki biçimi kullanacak şekilde değiştirin.

    Bu dosyaları daha sonra hedef dizindeki özel rolleri yeniden oluşturmak için kullanacaksınız.

    ```json
    {
      "Name": "",
      "Description": "",
      "Actions": [],
      "NotActions": [],
      "DataActions": [],
      "NotDataActions": [],
      "AssignableScopes": []
    }
    ```

### <a name="determine-user-group-and-service-principal-mappings"></a>Kullanıcı, Grup ve hizmet sorumlusu eşlemelerini belirleme

1. Rol atamalarınızın listesine göre, hedef dizinde eşlenecek kullanıcıları, grupları ve hizmet sorumlularını saptayın.

    `principalType`Her rol atamasında özelliğine bakarak asıl türünü belirleyebilirsiniz.

1. Gerekirse, hedef dizinde, ihtiyacınız olacak tüm kullanıcıları, grupları veya hizmet sorumlularını oluşturun.

### <a name="list-role-assignments-for-managed-identities"></a>Yönetilen kimlikler için rol atamalarını listeleyin

Yönetilen kimlikler, bir abonelik başka bir dizine aktarıldığında güncellenmez. Sonuç olarak, sistem tarafından atanan mevcut veya Kullanıcı tarafından atanan Yönetilen kimlikler bozulur. Aktarımdan sonra, sistem tarafından atanan yönetilen kimlikleri yeniden etkinleştirebilirsiniz. Kullanıcı tarafından atanan Yönetilen kimlikler için, bunları hedef dizinde yeniden oluşturmanız ve bağlamanız gerekir.

1. Yönetilen kimlikleri [destekleyen Azure hizmetlerinin listesini](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md) gözden geçirin ve yönetilen kimlikleri nerede kullanabileceğinizi aklınızda bulabilirsiniz.

1. Sistem tarafından atanan ve Kullanıcı tarafından atanan yönetilen kimliklerinizi listelemek için [az ad SP listesini](/azure/ad/sp#az-ad-sp-list) kullanın.

    ```azurecli
    az ad sp list --all --filter "servicePrincipalType eq 'ManagedIdentity'"
    ```

1. Yönetilen kimlikler listesinde, hangisinin sistem tarafından atandığını ve Kullanıcı tarafından atandığını saptayın. Türü öğrenmek için aşağıdaki ölçütleri kullanabilirsiniz.

    | Ölçütler | Yönetilen kimlik türü |
    | --- | --- |
    | `alternativeNames`özellik eklemeleri`isExplicit=False` | Sistem tarafından atanan |
    | `alternativeNames`Özellik şunu içermez`isExplicit` | Sistem tarafından atanan |
    | `alternativeNames`özellik eklemeleri`isExplicit=True` | Kullanıcı tarafından atanan |

    Yalnızca Kullanıcı tarafından atanan yönetilen kimlikleri listelemek için [az Identity List](https://docs.microsoft.com/cli/azure/identity#az-identity-list) ' i de kullanabilirsiniz. Daha fazla bilgi için bkz. [Azure CLI kullanarak Kullanıcı tarafından atanan yönetilen kimlik oluşturma, listeleme veya silme](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-cli.md).

    ```azurecli
    az identity list
    ```

1. `objectId`Yönetilen kimliklerinizin değerlerinin listesini alın.

1. Yönetilen kimlikleriniz için herhangi bir rol ataması olup olmadığını görmek için rol atamaları listenizde arama yapın.

### <a name="list-key-vaults"></a>Anahtar kasalarını Listele

Bir Anahtar Kasası oluşturduğunuzda, otomatik olarak oluşturulduğu aboneliğin varsayılan Azure Active Directory kiracı KIMLIĞINE bağlanır. Tüm erişim ilkesi girdileri de bu kiracı kimliğine bağlanır. Daha fazla bilgi için bkz. [Azure Key Vault başka bir aboneliğe taşıma](../key-vault/general/keyvault-move-subscription.md).

> [!WARNING]
> Depolama hesabı veya SQL veritabanı gibi bir kaynak için bekleyen şifreleme kullanıyorsanız, aktarılmakta olan abonelikte aynı abonelikte olmayan bir Anahtar Kasası bağımlılığı olan bir kurtarılamaz senaryoya yol açabilir. Bu durumda, başka bir anahtar kasası kullanmak veya bu kurtarılamaz senaryoyu önlemek için müşteri tarafından yönetilen anahtarları geçici olarak devre dışı bırakmak için gerekli adımları uygulamanız gerekir.

- Anahtar kasanız varsa, erişim ilkelerini listelemek için [az keykasa Show](https://docs.microsoft.com/cli/azure/keyvault#az-keyvault-show) komutunu kullanın. Daha fazla bilgi için bkz. [erişim denetimi ilkesiyle Key Vault kimlik doğrulaması sağlama](../key-vault/key-vault-group-permissions-for-apps.md).

    ```azurecli
    az keyvault show --name MyKeyVault
    ```

### <a name="list-azure-sql-databases-with-azure-ad-authentication"></a>Azure AD kimlik doğrulamasıyla Azure SQL veritabanlarını listeleme

- Azure AD kimlik doğrulamasıyla Azure SQL veritabanları kullanıp kullankullandığınızı görmek için [az SQL Server ad-admin List](https://docs.microsoft.com/cli/azure/sql/server/ad-admin#az-sql-server-ad-admin-list) ve [az Graph](https://docs.microsoft.com/cli/azure/ext/resource-graph/graph) Extension kullanın. Daha fazla bilgi için bkz. [SQL ile Azure Active Directory kimlik doğrulamasını yapılandırma ve yönetme](../sql-database/sql-database-aad-authentication-configure.md).

    ```azurecli
    az sql server ad-admin list --ids $(az graph query -q 'resources | where type == "microsoft.sql/servers" | project id' -o tsv | cut -f1)
    ```

### <a name="list-acls"></a>ACL 'Leri listeleme

1. Azure Data Lake Storage 1. kullanıyorsanız, herhangi bir dosyaya uygulanan ACL 'Leri Azure portal veya PowerShell kullanarak listeleyin.

1. Azure Data Lake Storage 2. kullanıyorsanız, herhangi bir dosyaya uygulanan ACL 'Leri Azure portal veya PowerShell kullanarak listeleyin.

1. Azure dosyaları kullanıyorsanız, herhangi bir dosyaya uygulanan ACL 'Leri listeleyin.

### <a name="list-other-known-resources"></a>Diğer bilinen kaynakları listeleme

1. Abonelik KIMLIĞINIZI almak için [az Account Show](https://docs.microsoft.com/cli/azure/account#az-account-show) kullanın.

    ```azurecli
    subscriptionId=$(az account show --query id | sed -e 's/^"//' -e 's/"$//')
    ```

1. Diğer Azure kaynaklarını bilinen Azure AD dizin bağımlılıklarıyla listelemek için [az Graph](https://docs.microsoft.com/cli/azure/ext/resource-graph/graph) uzantısını kullanın.

    ```azurecli
    az graph query -q \
    'resources | where type != "microsoft.azureactivedirectory/b2cdirectories" | where  identity <> "" or properties.tenantId <> "" or properties.encryptionSettingsCollection.enabled == true | project name, type, kind, identity, tenantId, properties.tenantId' \
    --subscriptions $subscriptionId --output table
    ```

## <a name="step-2-transfer-billing-ownership"></a>2. Adım: Faturalandırma sahipliğini aktarma

Bu adımda, aboneliğin faturalama sahipliğini Kaynak dizinden hedef dizine aktarırsınız.

> [!WARNING]
> Aboneliğin fatura sahipliğini aktardığınızda, kaynak dizindeki tüm rol atamaları **kalıcı olarak** silinir ve geri yüklenemez. Aboneliğin fatura sahipliğini aktardıktan sonra geri dönemezsiniz. Bu adımı gerçekleştirmeden önce önceki adımları tamamladığınızdan emin olun.

1. Bir [Azure aboneliğinin faturalandırma sahipliğini başka bir hesaba aktarma](../cost-management-billing/manage/billing-subscription-transfer.md)bölümündeki adımları izleyin. Aboneliği farklı bir Azure AD dizinine aktarmak için **abonelik Azure AD kiracısı** onay kutusunu denetlemeniz gerekir.

1. Sahiplik aktarmayı tamamladıktan sonra, hedef dizindeki kaynakları yeniden oluşturmak için bu makaleye geri dönün.

## <a name="step-3-re-create-resources"></a>3. Adım: kaynakları yeniden oluşturma

### <a name="sign-in-to-target-directory"></a>Hedef dizinde oturum açın

1. Hedef dizinde, aktarım isteğini kabul eden kullanıcı olarak oturum açın.

    Yalnızca, aktarım isteğini kabul eden yeni hesaptaki Kullanıcı, kaynakları yönetmek için erişime sahip olur.

1. [Az Account List](https://docs.microsoft.com/cli/azure/account#az-account-list) komutuyla aboneliklerinizin bir listesini alın.

    ```azurecli
    az account list --output table
    ```

1. Kullanmak istediğiniz etkin aboneliği ayarlamak için [az Account set](https://docs.microsoft.com/cli/azure/account#az-account-set) kullanın.

    ```azurecli
    az account set --subscription "Contoso"
    ```

### <a name="create-custom-roles"></a>Özel roller oluşturma
        
- Daha önce oluşturduğunuz dosyalardaki her bir özel rolü oluşturmak için [az role Definition Create](https://docs.microsoft.com/cli/azure/role/definition#az-role-definition-create) ' i kullanın. Daha fazla bilgi için bkz. [Azure CLI kullanarak Azure kaynakları için özel roller oluşturma veya güncelleştirme](custom-roles-cli.md).

    ```azurecli
    az role definition create --role-definition <role_definition>
    ```

### <a name="create-role-assignments"></a>Rol atamaları oluşturma

- Kullanıcılar, gruplar ve hizmet sorumluları için rol atamaları oluşturmak için [az role atama oluştur](https://docs.microsoft.com/cli/azure/role/assignment#az-role-assignment-create) ' u kullanın. Daha fazla bilgi için bkz. [Azure RBAC ve Azure CLI kullanarak rol atamaları ekleme veya kaldırma](role-assignments-cli.md).

    ```azurecli
    az role assignment create --role <role_name_or_id> --assignee <assignee> --resource-group <resource_group>
    ```

### <a name="update-system-assigned-managed-identities"></a>Sistem tarafından atanan yönetilen kimlikleri güncelleştirme

1. Sistem tarafından atanan yönetilen kimlikleri devre dışı bırakın ve yeniden etkinleştirin.

    | Azure hizmeti | Daha fazla bilgi | 
    | --- | --- |
    | Sanal makineler | [Azure CLı kullanarak Azure VM 'de Azure kaynakları için Yönetilen kimlikler yapılandırma](../active-directory/managed-identities-azure-resources/qs-configure-cli-windows-vm.md#system-assigned-managed-identity) |
    | Sanal makine ölçek kümeleri | [Azure CLı kullanarak bir sanal makine ölçek kümesindeki Azure kaynakları için Yönetilen kimlikler yapılandırma](../active-directory/managed-identities-azure-resources/qs-configure-cli-windows-vmss.md#system-assigned-managed-identity) |
    | Diğer hizmetler | [Azure kaynakları için yönetilen kimlikleri destekleyen hizmetler](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md) |

1. Sistem tarafından atanan Yönetilen kimlikler için rol atamaları oluşturmak için [az role atama oluştur](https://docs.microsoft.com/cli/azure/role/assignment#az-role-assignment-create) kullanın. Daha fazla bilgi için bkz. [Azure CLI kullanarak bir kaynağa yönetilen kimlik erişimi atama](../active-directory/managed-identities-azure-resources/howto-assign-access-cli.md).

    ```azurecli
    az role assignment create --assignee <objectid> --role '<role_name_or_id>' --scope <scope>
    ```

### <a name="update-user-assigned-managed-identities"></a>Kullanıcı tarafından atanan yönetilen kimlikleri Güncelleştir

1. Kullanıcı tarafından atanan yönetilen kimlikleri silin, yeniden oluşturun ve ekleyin.

    | Azure hizmeti | Daha fazla bilgi | 
    | --- | --- |
    | Sanal makineler | [Azure CLı kullanarak Azure VM 'de Azure kaynakları için Yönetilen kimlikler yapılandırma](../active-directory/managed-identities-azure-resources/qs-configure-cli-windows-vm.md#user-assigned-managed-identity) |
    | Sanal makine ölçek kümeleri | [Azure CLı kullanarak bir sanal makine ölçek kümesindeki Azure kaynakları için Yönetilen kimlikler yapılandırma](../active-directory/managed-identities-azure-resources/qs-configure-cli-windows-vmss.md#user-assigned-managed-identity) |
    | Diğer hizmetler | [Azure kaynakları için yönetilen kimlikleri destekleyen hizmetler](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md)<br/>[Azure CLı kullanarak Kullanıcı tarafından atanan yönetilen kimlik oluşturma, listeleme veya silme](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-cli.md) |

1. Kullanıcı tarafından atanan Yönetilen kimlikler için rol atamaları oluşturmak için [az role atama oluştur](https://docs.microsoft.com/cli/azure/role/assignment#az-role-assignment-create) kullanın. Daha fazla bilgi için bkz. [Azure CLI kullanarak bir kaynağa yönetilen kimlik erişimi atama](../active-directory/managed-identities-azure-resources/howto-assign-access-cli.md).

    ```azurecli
    az role assignment create --assignee <objectid> --role '<role_name_or_id>' --scope <scope>
    ```

### <a name="update-key-vaults"></a>Anahtar kasalarını Güncelleştir

Bu bölümde, anahtar kasalarınızı güncelleştirmek için temel adımlar açıklanmaktadır. Daha fazla bilgi için bkz. [Azure Key Vault başka bir aboneliğe taşıma](../key-vault/general/keyvault-move-subscription.md).

1. Abonelikteki tüm mevcut anahtar kasaları ile ilişkili kiracı KIMLIĞINI hedef dizine güncelleştirin.

1. Mevcut tüm erişim ilkesi girdilerini kaldırın.

1. Hedef dizinle ilişkili yeni erişim ilkesi girdileri ekleyin.

### <a name="update-acls"></a>ACL 'Leri Güncelleştir

1. Azure Data Lake Storage 1. kullanıyorsanız, uygun ACL 'Leri atayın. Daha fazla bilgi için bkz. [Azure Data Lake Storage 1. depolanan verileri güvenli hale getirme](../data-lake-store/data-lake-store-secure-data.md).

1. Azure Data Lake Storage 2. kullanıyorsanız, uygun ACL 'Leri atayın. Daha fazla bilgi için [Azure Data Lake Storage 2. erişim denetimi](../storage/blobs/data-lake-storage-access-control.md)bölümüne bakın.

1. Azure dosyaları kullanıyorsanız, uygun ACL 'Leri atayın.

### <a name="review-other-security-methods"></a>Diğer güvenlik yöntemlerini gözden geçirme

Aktarım sırasında rol atamaları kaldırılsa da, özgün sahip hesabındaki kullanıcılar şu gibi diğer güvenlik yöntemleriyle aboneliğe erişime sahip olmaya devam edebilir:

- Depolama gibi hizmetler için erişim anahtarları.
- Kullanıcı yöneticisine abonelik kaynaklarına erişim izni veren [yönetim sertifikaları](../cloud-services/cloud-services-certs-create.md) .
- Azure Sanal Makineleri gibi hizmetler için Uzaktan Erişim kimlik bilgileri.

Amaç, hedef dizinde erişimleri olmaması için kaynak dizindeki kullanıcılardan erişimi kaldırmak istiyorsanız, herhangi bir kimlik bilgisi döndürmeyi göz önünde bulundurmanız gerekir. Kimlik bilgileri güncelleştirilene kadar, kullanıcılar aktarımdan sonra erişime sahip olmaya devam edecektir.

1. Depolama hesabı erişim anahtarlarını döndürün. Daha fazla bilgi için bkz. [depolama hesabı erişim anahtarlarını yönetme](../storage/common/storage-account-keys-manage.md).

1. Azure SQL veritabanı veya Azure Service Bus mesajlaşma gibi diğer hizmetler için erişim anahtarları kullanıyorsanız, erişim anahtarlarını döndürün.

1. Gizli dizileri kullanan kaynaklar için, kaynak ayarlarını açın ve parolayı güncelleştirin.

1. Sertifikaları kullanan kaynaklar için sertifikayı güncelleştirin.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure aboneliğinin faturalama sahipliğini başka bir hesaba aktarma](../cost-management-billing/manage/billing-subscription-transfer.md)
- [Azure aboneliklerini aboneler ile CSP'ler arasında aktarma](../cost-management-billing/manage/transfer-subscriptions-subscribers-csp.md)
- [Azure Active Directory kiracınıza bir Azure aboneliğini ekleme veya ilişkilendirme](../active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md)
