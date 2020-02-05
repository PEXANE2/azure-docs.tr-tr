---
title: Python kullanarak Azure Databricks Azure Veri Gezgini 'a bağlanma
description: Bu konu başlığı altında, iki kimlik doğrulama yönteminden birini kullanarak Azure Veri Gezgini verilerine erişmek için Azure Databricks bir Python kitaplığını nasıl kullanacağınızı gösterir.
author: orspod
ms.author: orspodek
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: conceptual
ms.date: 11/27/2018
ms.openlocfilehash: 03dee0570faa863ca411ed91f2a6ec85a1e38380
ms.sourcegitcommit: 4f6a7a2572723b0405a21fea0894d34f9d5b8e12
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/04/2020
ms.locfileid: "76985688"
---
# <a name="connect-to-azure-data-explorer-from-azure-databricks-by-using-python"></a>Python kullanarak Azure Databricks Azure Veri Gezgini 'a bağlanma

[Azure Databricks](https://docs.microsoft.com/azure/azure-databricks/what-is-azure-databricks) , Microsoft Azure platformu için en iyi duruma getirilmiş Apache Spark tabanlı bir analiz platformudur. Bu makalede, Azure Veri Gezgini verilerine erişmek için Azure Databricks bir Python kitaplığını nasıl kullanabileceğiniz gösterilmektedir. Bir cihaz oturum açma ve bir Azure Active Directory (Azure AD) uygulaması da dahil olmak üzere Azure Veri Gezgini kimlik doğrulaması için birkaç yol vardır.

## <a name="prerequisites"></a>Ön koşullar

- [Azure Veri Gezgini kümesi ve veritabanı oluşturun](/azure/data-explorer/create-cluster-database-portal).
- [Azure Databricks çalışma alanı oluşturun](/azure/azure-databricks/quickstart-create-databricks-workspace-portal#create-an-azure-databricks-workspace). **Azure Databricks hizmeti**altında, **fiyatlandırma katmanı** açılan listesinde **Premium**' u seçin. Bu seçim, kimlik bilgilerinizi depolamak ve bunları not defterlerine ve işlere başvurmak için Azure Databricks gizli dizileri kullanmanıza olanak sağlar.

- Aşağıdaki belirtimlerle Azure Databricks [bir küme oluşturun](https://docs.azuredatabricks.net/user-guide/clusters/create.html) (örnek not defterlerini çalıştırmak için gereken minimum ayarlar):

   ![Küme oluşturma belirtimleri](media/connect-from-databricks/databricks-create-cluster.png)

## <a name="install-the-python-library-on-your-azure-databricks-cluster"></a>Azure Databricks kümenize Python kitaplığı 'nı yükler

Azure Databricks kümenize [Python kitaplığını](/azure/kusto/api/python/kusto-python-client-library) yüklemek için:

1. Azure Databricks çalışma alanınıza gidin ve [bir kitaplık oluşturun](https://docs.azuredatabricks.net/user-guide/libraries.html#create-a-library).
2. [Python Pypı paketini veya Python Yumurg 'Sini karşıya yükleyin](https://docs.azuredatabricks.net/user-guide/libraries.html#upload-a-python-pypi-package-or-python-egg).
   - Kitaplığı yükleyin, yükleyin ve Databricks kümenize ekleyin.
   - Pypı adını girin: **Azure-kusto-Data**.

## <a name="connect-to-azure-data-explorer-by-using-a-device-login"></a>Bir cihaz oturum açma kullanarak Azure Veri Gezgini bağlanma

[Sorgu-ADX-Device-Login](https://github.com/Azure/azure-kusto-docs-samples/blob/master/Databricks_notebooks/Query-ADX-device-login.ipynb) Not defteri kullanarak [bir not defteri içeri aktarın](https://docs.azuredatabricks.net/user-guide/notebooks/notebook-manage.html#import-a-notebook) . Daha sonra, kimlik bilgilerinizi kullanarak Azure Veri Gezgini bağlanabilirsiniz.

## <a name="connect-to-adx-by-using-an-azure-ad-app"></a>Azure AD uygulaması kullanarak ADX 'e bağlanma

1. Azure AD [uygulaması](/azure/kusto/management/access-control/how-to-provision-aad-app)sağlayarak Azure AD uygulaması oluşturun.
1. Azure Veri Gezgini veritabanınızdaki Azure AD uygulamanıza şu şekilde erişim izni verin:

    ```kusto
    .set database <DB Name> users ('aadapp=<AAD App ID>;<AAD Tenant ID>') 'AAD App to connect Spark to ADX
    ```
    |   |   |
    | - | - |
    | ```DB Name``` | veritabanı adınız |
    | ```AAD App ID``` | Azure AD uygulama KIMLIĞINIZ |
    | ```AAD Tenant ID``` | Azure AD kiracı KIMLIĞINIZ |

### <a name="find-your-azure-ad-tenant-id"></a>Azure AD kiracı KIMLIĞINIZI bulun

Azure Veri Gezgini, bir uygulamanın kimliğini doğrulamak için Azure AD kiracı KIMLIĞINIZI kullanır. Kiracı KIMLIĞINIZI bulmak için aşağıdaki URL 'YI kullanın. *Etki alanınız için etki*alanınızı değiştirin.

```
https://login.windows.net/<YourDomain>/.well-known/openid-configuration/
```

Örneğin, etki alanınız *contoso.com* olduğunda URL şöyle olur: [https://login.windows.net/contoso.com/.well-known/openid-configuration/](https://login.windows.net/contoso.com/.well-known/openid-configuration/). Sonuçları görmek için bu URL 'YI seçin. İlk satır aşağıdaki gibidir: 

```
"authorization_endpoint":"https://login.windows.net/6babcaad-604b-40ac-a9d7-9fd97c0b779f/oauth2/authorize"
```

Kiracı KIMLIĞINIZ `6babcaad-604b-40ac-a9d7-9fd97c0b779f`. 

### <a name="store-and-secure-your-azure-ad-app-id-and-key"></a>Azure AD uygulama KIMLIĞINIZI ve anahtarınızı depolayın ve koruyun 

Azure AD uygulama KIMLIĞINIZI ve anahtarınızı aşağıda gösterildiği gibi Azure Databricks [gizli](https://docs.azuredatabricks.net/user-guide/secrets/index.html#secrets) dizileri kullanarak depolayın ve güvenli hale getirin:
1. [CLI 'Yi ayarlayın](https://docs.azuredatabricks.net/user-guide/dev-tools/databricks-cli.html#set-up-the-cli).
1. [CLI 'Yı yükler](https://docs.azuredatabricks.net/user-guide/dev-tools/databricks-cli.html#install-the-cli). 
1. [Kimlik doğrulamasını ayarlayın](https://docs.azuredatabricks.net/user-guide/dev-tools/databricks-cli.html#set-up-authentication).
1. Aşağıdaki örnek komutları kullanarak [gizli](https://docs.azuredatabricks.net/user-guide/secrets/index.html#secrets) dizileri yapılandırın:

    ```databricks secrets create-scope --scope adx```

    ```databricks secrets put --scope adx --key myaadappid```

    ```databricks secrets put --scope adx --key myaadappkey```

    ```databricks secrets list --scope adx```

### <a name="import-a-notebook"></a>Bir not defteri içeri aktar
Azure Veri Gezgini bağlanmak için [Query-ADX-AAD-App](https://github.com/Azure/azure-kusto-docs-samples/blob/master/Databricks_notebooks/Query-ADX-AAD-App.ipynb) Not defterini kullanarak [bir not defteri içeri aktarın](https://docs.azuredatabricks.net/user-guide/notebooks/notebook-manage.html#import-a-notebook) . Yer tutucu değerlerini küme adınızla, veritabanı adınızla ve Azure AD kiracı KIMLIĞINIZLE güncelleştirin.
