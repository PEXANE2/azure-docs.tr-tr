---
title: Python'u kullanarak Azure Veri Tuğlalarından Azure Veri Gezgini'ne bağlanın
description: Bu konu, iki kimlik doğrulama yönteminden birini kullanarak Azure Veri Gezgini'nden verilere erişmek için Azure Veri Tuğlaları'nda Python kitaplığını nasıl kullanacağınızı gösterir.
author: orspod
ms.author: orspodek
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: conceptual
ms.date: 11/27/2018
ms.openlocfilehash: 03dee0570faa863ca411ed91f2a6ec85a1e38380
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76985688"
---
# <a name="connect-to-azure-data-explorer-from-azure-databricks-by-using-python"></a>Python'u kullanarak Azure Veri Tuğlaları'ndan Azure Veri Gezgini'ne bağlanma

[Azure Databricks,](https://docs.microsoft.com/azure/azure-databricks/what-is-azure-databricks) Microsoft Azure platformu için optimize edilmiş Apache Spark tabanlı bir analiz platformudur. Bu makalede, Azure Veri Gezgini'nden verilere erişmek için Azure Veri Tuğlaları'nda Python kitaplığı nasıl kullanılacağı gösterilmektedir. Aygıt girişi ve Azure Etkin Dizin (Azure AD) uygulaması da dahil olmak üzere Azure Veri Gezgini ile kimlik doğrulaması yapmanın çeşitli yolları vardır.

## <a name="prerequisites"></a>Ön koşullar

- [Azure Veri Gezgini kümesi ve veritabanı oluşturun.](/azure/data-explorer/create-cluster-database-portal)
- [Azure Databricks çalışma alanı oluşturun.](/azure/azure-databricks/quickstart-create-databricks-workspace-portal#create-an-azure-databricks-workspace) **Azure Databricks Hizmeti**altında, Fiyatlandırma **Katmanı** açılır listesinde **Premium'u**seçin. Bu seçim, kimlik bilgilerinizi depolamak ve not defterlerinde ve işlerde başvurmak için Azure Databricks sırlarını kullanmanıza olanak tanır.

- Azure Veri Tuğlaları'nda aşağıdaki belirtimlere sahip [bir küme oluşturun](https://docs.azuredatabricks.net/user-guide/clusters/create.html) (örnek not defterlerini çalıştırmak için gereken minimum ayarlar):

   ![Küme oluşturmak için teknik özellikler](media/connect-from-databricks/databricks-create-cluster.png)

## <a name="install-the-python-library-on-your-azure-databricks-cluster"></a>Azure Veri Tuğlaları kümenizde Python kitaplığını yükleme

[Python kitaplığını](/azure/kusto/api/python/kusto-python-client-library) Azure Databricks kümenize yüklemek için:

1. Azure Databricks çalışma alanınıza gidin ve [bir kitaplık oluşturun.](https://docs.azuredatabricks.net/user-guide/libraries.html#create-a-library)
2. [Python PyPI paketi veya Python Yumurtası yükleyin.](https://docs.azuredatabricks.net/user-guide/libraries.html#upload-a-python-pypi-package-or-python-egg)
   - Kitaplığı Veri Tuğlaları kümenize yükleyin, yükleyin ve ekleyin.
   - PyPi adını girin: **azure-kusto-data**.

## <a name="connect-to-azure-data-explorer-by-using-a-device-login"></a>Aygıt girişi kullanarak Azure Veri Gezgini'ne bağlanma

[Sorgu-ADX-aygıt-giriş](https://github.com/Azure/azure-kusto-docs-samples/blob/master/Databricks_notebooks/Query-ADX-device-login.ipynb) not defterini kullanarak [bir not defteri ni aktarın.](https://docs.azuredatabricks.net/user-guide/notebooks/notebook-manage.html#import-a-notebook) Daha sonra kimlik bilgilerinizi kullanarak Azure Veri Gezgini'ne bağlanabilirsiniz.

## <a name="connect-to-adx-by-using-an-azure-ad-app"></a>Azure AD uygulamasını kullanarak ADX'e bağlanma

1. [Azure AD uygulamasını sağlayarak](/azure/kusto/management/access-control/how-to-provision-aad-app)Azure AD uygulaması oluşturun.
1. Azure Veri Gezgini veritabanınızda Azure AD uygulamanıza erişimi aşağıdaki şekilde tanıyın:

    ```kusto
    .set database <DB Name> users ('aadapp=<AAD App ID>;<AAD Tenant ID>') 'AAD App to connect Spark to ADX
    ```
    |   |   |
    | - | - |
    | ```DB Name``` | veritabanı adınız |
    | ```AAD App ID``` | Azure AD uygulama kimliğiniz |
    | ```AAD Tenant ID``` | Azure AD kiracı kimliğiniz |

### <a name="find-your-azure-ad-tenant-id"></a>Azure AD kiracı kimliğinizi bulma

Azure Veri Gezgini, bir uygulamanın kimliğini doğrulamak için Azure AD kiracı kimliğinizi kullanır. Kiracı kimliğinizi bulmak için aşağıdaki URL'yi kullanın. Etki alanınızı YourDomain'in yerine *değiştirin.*

```
https://login.windows.net/<YourDomain>/.well-known/openid-configuration/
```

Örneğin, etki alanınız *contoso.com* olduğunda URL şöyle olur: [https://login.windows.net/contoso.com/.well-known/openid-configuration/](https://login.windows.net/contoso.com/.well-known/openid-configuration/). Sonuçları görmek için bu URL'yi seçin. İlk satır aşağıdaki gibidir: 

```
"authorization_endpoint":"https://login.windows.net/6babcaad-604b-40ac-a9d7-9fd97c0b779f/oauth2/authorize"
```

Kiracı `6babcaad-604b-40ac-a9d7-9fd97c0b779f`kimliğiniz. 

### <a name="store-and-secure-your-azure-ad-app-id-and-key"></a>Azure AD uygulama kimliğinizi ve anahtarınızı depolamave koruma 

Azure Databricks [sırlarını](https://docs.azuredatabricks.net/user-guide/secrets/index.html#secrets) aşağıdaki gibi kullanarak Azure AD uygulama kimliğinizi ve anahtarınızı saklayın ve güvenli hale verin:
1. [CLI'yi ayarlayın.](https://docs.azuredatabricks.net/user-guide/dev-tools/databricks-cli.html#set-up-the-cli)
1. [CLI'yi yükleyin.](https://docs.azuredatabricks.net/user-guide/dev-tools/databricks-cli.html#install-the-cli) 
1. [Kimlik doğrulamasını ayarlayın.](https://docs.azuredatabricks.net/user-guide/dev-tools/databricks-cli.html#set-up-authentication)
1. Aşağıdaki örnek komutları kullanarak [sırları](https://docs.azuredatabricks.net/user-guide/secrets/index.html#secrets) yapılandırın:

    ```databricks secrets create-scope --scope adx```

    ```databricks secrets put --scope adx --key myaadappid```

    ```databricks secrets put --scope adx --key myaadappkey```

    ```databricks secrets list --scope adx```

### <a name="import-a-notebook"></a>Not defteri alma
Azure Veri Gezgini'ne bağlanmak için [Sorgu-ADX-AAD-App](https://github.com/Azure/azure-kusto-docs-samples/blob/master/Databricks_notebooks/Query-ADX-AAD-App.ipynb) not defterini kullanarak [bir not defteri ni aktarın.](https://docs.azuredatabricks.net/user-guide/notebooks/notebook-manage.html#import-a-notebook) Yer tutucu değerlerini küme adınız, veritabanı adınız ve Azure AD kiracı kimliğinizle güncelleştirin.
