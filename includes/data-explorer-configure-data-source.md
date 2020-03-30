---
author: orspod
ms.service: data-explorer
ms.topic: include
ms.date: 11/03/2019
ms.author: orspodek
ms.openlocfilehash: 3cd9d017429b629acad39f5b902e842886c3c818
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "78304920"
---
## <a name="configure-the-data-source"></a>Veri kaynağını yapılandırma

Azure Veri Gezgini'ni pano aracınız için veri kaynağı olarak yapılandırmak için aşağıdaki adımları gerçekleştirirsiniz. Bu adımları bu bölümde daha ayrıntılı olarak ele alacağız:

1. Azure Etkin Dizin (Azure AD) hizmet ilkesi oluşturun. Hizmet sorumlusu, Azure Veri Gezgini hizmetine erişmek için pano aracınız tarafından kullanılır.

1. Azure Veri Gezgini veritabanındaki *görüntüleyenrolüne* Azure AD hizmet ilkesini ekleyin.

1. Azure AD hizmet sorumlusundaki bilgilere dayanarak pano araç bağlantı özelliklerinizi belirtin ve bağlantıyı test edin.

### <a name="create-a-service-principal"></a>Hizmet sorumlusu oluşturma

[Azure portalında](#azure-portal) veya [Azure CLI](#azure-cli) komut satırı deneyimini kullanarak hizmet ilkesini oluşturabilirsiniz. Hangi yöntemi kullanırsanız kullanın, oluşturmadan sonra daha sonraki adımlarda kullanacağınız dört bağlantı özelliği için değerler alırsınız.

#### <a name="azure-portal"></a>Azure portalında

1. Hizmet ilkesini oluşturmak için Azure [portal belgelerindeki](/azure/active-directory/develop/howto-create-service-principal-portal)yönergeleri izleyin.

    1. Uygulamayı [bir rol bölümüne atayın,](/azure/active-directory/develop/howto-create-service-principal-portal#assign-a-role-to-the-application) Azure Veri **Reader** Gezgini kümenize bir rol türü Reader atayın.

    1. Oturum [açma değerleri bölümünde,](/azure/active-directory/develop/howto-create-service-principal-portal#get-values-for-signing-in) adımlarda kapsanan üç özellik değerini kopyalayın: **Dizin Kimliği** (kiracı Kimliği), **Uygulama Kimliği**ve **Parola.**

1. Azure portalında, **Abonelikler'i** seçin ve hizmet ilkesini oluşturduğunuz abonelik için kimliği kopyalayın.

    ![Abonelik Kimliği - portal](media/data-explorer-configure-data-source/subscription-id-portal.png)

#### <a name="azure-cli"></a>Azure CLI

1. Bir hizmet ilkesi oluşturun. Uygun bir kapsam ve rol `reader`türü ayarlayın.

    ```azurecli
    az ad sp create-for-rbac --name "https://{UrlToYourDashboard}:{PortNumber}" --role "reader" \
                             --scopes /subscriptions/{SubID}/resourceGroups/{ResourceGroupName}
    ```

    Daha fazla bilgi için bkz. Azure [CLI ile bir Azure hizmet ilkesi oluştur.](/cli/azure/create-an-azure-service-principal-azure-cli)

1. Komut aşağıdaki gibi bir sonuç kümesi döndürür. Üç özellik değerlerini kopyalayın: **appID,** **şifre**ve **kiracı**.


    ```json
    {
      "appId": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX",
      "displayName": "{UrlToYourDashboard}:{PortNumber}",
      "name": "https://{UrlToYourDashboard}:{PortNumber}",
      "password": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX",
      "tenant": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX"
    }
    ```

1. Aboneliklerinizin bir listesini alın.

    ```azurecli
    az account list --output table
    ```

    Uygun abonelik kimliğini kopyalayın.

    ![Abonelik Kimliği - CLI](media/data-explorer-configure-data-source/subscription-id-cli.png)

### <a name="add-the-service-principal-to-the-viewers-role"></a>Hizmet ilkesini görüntüleyenrolüne ekleme

Artık bir hizmet yöneticiliğiniz olduğuna göre, bunu Azure Veri Gezgini veritabanındaki *görüntüleyenrolüne* eklersiniz. Bu görevi Azure portalında **İzinler** altında veya bir yönetim komutunu kullanarak **Sorgu** altında gerçekleştirebilirsiniz.

#### <a name="azure-portal---permissions"></a>Azure portalı - İzinler

1. Azure portalında Azure Veri Gezgini kümenize gidin.

1. Genel **Bakış** bölümünde, StormEvents örnek verileri içeren veritabanını seçin.

    ![Veritabanı seçin](media/data-explorer-configure-data-source/select-database.png)

1. **İzinleri** seçin ve **sonra Ekle.**

    ![Veritabanı izinleri](media/data-explorer-configure-data-source/database-permissions.png)

1. **Veritabanı izinleri ekle**altında, **Görüntüleyici** rolünü seçin ve **asılları seçin.**

    ![Veritabanı izinleri ekleme](media/data-explorer-configure-data-source/add-permission.png)

1. Oluşturduğunuz hizmet müdürünü arayın. Anaparayı seçin, sonra **seçin.**

    ![Azure portalında izinleri yönetme](media/data-explorer-configure-data-source/new-principals.png)

1. **Kaydet'i**seçin.

    ![Azure portalında izinleri yönetme](media/data-explorer-configure-data-source/save-permission.png)

#### <a name="management-command---query"></a>Yönetim komutu - Sorgula

1. Azure portalında Azure Veri Gezgini kümenize gidin ve **Sorgula'yı**seçin.

    ![Sorgu](media/data-explorer-configure-data-source/query.png)

1. Sorgu penceresinde aşağıdaki komutu çalıştırın. Azure portalından veya CLI'deki uygulama kimliğini ve kiracı kimliğini kullanın.

    ```kusto
    .add database {TestDatabase} viewers ('aadapp={ApplicationID};{TenantID}')
    ```

    Komut aşağıdaki gibi bir sonuç kümesi döndürür. Bu örnekte, ilk satır veritabanında varolan bir kullanıcı için, ikinci satır ise yeni eklenen hizmet ilkesi içindir.

    ![Sonuç kümesi](media/data-explorer-configure-data-source/result-set.png)
