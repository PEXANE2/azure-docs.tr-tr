---
title: Azure Data Lake Storage 2. için ACL 'Leri yinelemeli olarak ayarla | Microsoft Docs
description: Üst dizinin varolan alt öğeleri için, erişim denetim listelerini yinelemeli olarak ekleyebilir, güncelleştirebilir ve kaldırabilirsiniz.
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: how-to
ms.date: 10/27/2020
ms.author: normesta
ms.reviewer: prishet
ms.custom: devx-track-csharp
ms.openlocfilehash: 247742cac3dc24b062fc8e1cb5eceb6c1a6f3f8b
ms.sourcegitcommit: d76108b476259fe3f5f20a91ed2c237c1577df14
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/29/2020
ms.locfileid: "92911622"
---
# <a name="set-access-control-lists-acls-recursively-for-azure-data-lake-storage-gen2"></a>Azure Data Lake Storage 2. için erişim denetim listelerini (ACL 'Ler) yinelemeli olarak ayarlama

ACL devralma, bir üst dizin altında oluşturulan yeni alt öğeler için zaten kullanılabilir. Ayrıca, bu değişiklikleri her bir alt öğe için ayrı ayrı yapmak zorunda kalmadan, bir üst dizinin varolan alt öğeleri için de, ACL 'Leri yinelemeli olarak ekleyebilir, güncelleştirebilir ve kaldırabilirsiniz.

> [!NOTE]
> Erişim listelerini yinelemeli olarak ayarlama özelliği genel önizlemede yer almaktadır ve tüm bölgelerde kullanılabilir.  

[Kitaplıklar](#libraries)  |  [Örnekler](#code-samples)  |  [En iyi uygulamalar](#best-practice-guidelines)  |  [Geri bildirimde](#provide-feedback) bulunun

## <a name="prerequisites"></a>Önkoşullar

- Azure aboneliği. Bkz. [Azure ücretsiz deneme sürümü edinme](https://azure.microsoft.com/pricing/free-trial/).

- Hiyerarşik ad alanı (HNS) etkin olan bir depolama hesabı. Bir tane oluşturmak için [Bu](data-lake-storage-quickstart-create-account.md) yönergeleri izleyin.

- Özyinelemeli ACL işlemini yürütmek için doğru izinler. Doğru izin aşağıdakilerden birini içerir: 

  - Hedef kapsayıcının, üst kaynak grubunun veya aboneliğin kapsamındaki [Depolama Blobu veri sahibi](../../role-based-access-control/built-in-roles.md#storage-blob-data-owner) rolüne atanan bir Azure ACTIVE DIRECTORY (ad) [güvenlik sorumlusu](https://docs.microsoft.com/azure/role-based-access-control/overview#security-principal) .   

  - Özyinelemeli ACL işleminin uygulanmasını planladığınız Hedef kapsayıcının veya dizinin sahibi olan kullanıcı. Bu, hedef kapsayıcı veya dizindeki tüm alt öğeleri içerir. 

- ACL 'Lerin dizinlere ve dosyalara nasıl uygulandığını anlama. [Azure Data Lake Storage 2. erişim denetimi '](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-access-control)ne bakın. 

PowerShell, .NET SDK ve Python SDK yükleme kılavuzunu görüntülemek için bu makalenin **projenizi ayarlama** bölümüne bakın.

## <a name="set-up-your-project"></a>Projenizi ayarlama

Gerekli kitaplıkları yükler.

### <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

1. .NET Framework 'ün yüklü olduğundan emin olun. Bkz. [indirme .NET Framework](https://dotnet.microsoft.com/download/dotnet-framework).
 
2. `5.1`Aşağıdaki komutu kullanarak yüklü PowerShell sürümünün veya daha yüksek olduğunu doğrulayın.    

   ```powershell
   echo $PSVersionTable.PSVersion.ToString() 
   ```
    
   PowerShell sürümünüzü yükseltmek için bkz. [var olan Windows PowerShell 'ı yükseltme](https://docs.microsoft.com/powershell/scripting/install/installing-windows-powershell)
    
3. PowershellGet modülünün en son sürümünü yükler.

   ```powershell
   install-Module PowerShellGet –Repository PSGallery –Force  
   ```

4. ' I kapatın ve ardından PowerShell konsolunu yeniden açın.

5. Install **az. Storage** Preview Module.

   ```powershell
   Install-Module Az.Storage -Repository PsGallery -RequiredVersion 2.5.2-preview -AllowClobber -AllowPrerelease -Force  
   ```

   PowerShell modüllerinin nasıl yükleneceği hakkında daha fazla bilgi için bkz [. Azure PowerShell modülünü Install](https://docs.microsoft.com/powershell/azure/install-az-ps)

### <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

1. [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview)açın veya Azure CLI 'yı yerel olarak [yüklediyseniz](https://docs.microsoft.com/cli/azure/install-azure-cli) , Windows PowerShell gibi bir komut konsol uygulaması açın.

2. Yüklü olan Azure CLı sürümünün `2.14.0` aşağıdaki komutu kullanarak veya daha yüksek olduğunu doğrulayın.

   ```azurecli
    az --version
   ```
   Azure CLı sürümünüz daha düşükse `2.14.0` , daha sonra yeni bir sürüm yüklersiniz. Bkz. [Azure CLI 'Yi yüklemeyi](https://docs.microsoft.com/cli/azure/install-azure-cli).

### <a name="net"></a>[.NET](#tab/dotnet)

1. Bir komut penceresi açın (örneğin: Windows PowerShell).

2. Proje dizininizden komutunu kullanarak Azure. Storage. Files. DataLake önizleme paketini yükledikten sonra `dotnet add package` .

   ```console
   dotnet add package Azure.Storage.Files.DataLake -v 12.5.0-preview.1 -s https://pkgs.dev.azure.com/azure-sdk/public/_packaging/azure-sdk-for-net/nuget/v3/index.json
   ```

3. Bu using deyimlerini, kod dosyanızın en üstüne ekleyin.

   ```csharp
   using Azure;
   using Azure.Core;
   using Azure.Storage;
   using Azure.Storage.Files.DataLake;
   using Azure.Storage.Files.DataLake.Models;
   using System.Collections.Generic;
   using System.Threading.Tasks;
    ```

### <a name="java"></a>[Java](#tab/java)

Başlamak için [Bu sayfayı](https://search.maven.org/artifact/com.azure/azure-storage-file-datalake) açın ve Java kitaplığı 'nın en son sürümünü bulun. Sonra, *pom.xml* dosyasını metin düzenleyicinizde açın. Bu sürüme başvuran bir bağımlılık öğesi ekleyin.

İstemci uygulamanızın kimlik doğrulamasını Azure Active Directory (AD) kullanarak ve sonra Azure gizli Istemci kitaplığına bir bağımlılık ekleyin. Bkz.  [projenize gizli Istemci kitaplığı paketi ekleme](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/identity/azure-identity#adding-the-package-to-your-project).

Ardından, bu Imports deyimlerini kod dosyanıza ekleyin.

```java
import com.azure.identity.ClientSecretCredential;
import com.azure.identity.ClientSecretCredentialBuilder;
import com.azure.core.http.rest.PagedIterable;
import com.azure.core.http.rest.Response;
import com.azure.storage.common.StorageSharedKeyCredential;
import com.azure.storage.file.datalake.DataLakeDirectoryClient;
import com.azure.storage.file.datalake.DataLakeFileClient;
import com.azure.storage.file.datalake.DataLakeFileSystemClient;
import com.azure.storage.file.datalake.DataLakeServiceClient;
import com.azure.storage.file.datalake.DataLakeServiceClientBuilder;
import com.azure.storage.file.datalake.models.AccessControlChangeResult;
import com.azure.storage.file.datalake.models.AccessControlType;
import com.azure.storage.file.datalake.models.ListPathsOptions;
import com.azure.storage.file.datalake.models.PathAccessControl;
import com.azure.storage.file.datalake.models.PathAccessControlEntry;
import com.azure.storage.file.datalake.models.PathItem;
import com.azure.storage.file.datalake.models.PathPermissions;
import com.azure.storage.file.datalake.models.PathRemoveAccessControlEntry;
import com.azure.storage.file.datalake.models.RolePermissions;
import com.azure.storage.file.datalake.options.PathSetAccessControlRecursiveOptions;
```

### <a name="python"></a>[Python](#tab/python)

1. [Python için Azure Data Lake Storage istemci kitaplığını](https://nam06.safelinks.protection.outlook.com/?url=https%3A%2F%2Frecursiveaclpr.blob.core.windows.net%2Fprivatedrop%2Fazure_storage_file_datalake-12.1.0b99-py2.py3-none-any.whl%3Fsv%3D2019-02-02%26st%3D2020-08-24T07%253A47%253A01Z%26se%3D2021-08-25T07%253A47%253A00Z%26sr%3Db%26sp%3Dr%26sig%3DH1XYw4FTLJse%252BYQ%252BfamVL21UPVIKRnnh2mfudA%252BfI0I%253D&data=02%7C01%7Cnormesta%40microsoft.com%7C95a5966d938a4902560e08d84912fe32%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C0%7C637339693209725909&sdata=acv4KWZdzkITw1lP0%2FiA3lZuW7NF5JObjY26IXttfGI%3D&reserved=0)indirin.

2. [PIP](https://pypi.org/project/pip/)kullanarak indirdiğiniz kitaplığı yükler.

   ```
   pip install azure_storage_file_datalake-12.1.0b99-py2.py3-none-any.whl
   ```

Bu import deyimlerini, kod dosyanızın en üstüne ekleyin.

```python
import os, uuid, sys
from azure.storage.filedatalake import DataLakeServiceClient
from azure.core._match_conditions import MatchConditions
from azure.storage.filedatalake._models import ContentSettings
```

---

## <a name="connect-to-your-account"></a>Hesabınıza bağlanma

Azure Active Directory (AD) kullanarak veya bir hesap anahtarı kullanarak bağlanabilirsiniz. 

### <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Bir Windows PowerShell komut penceresi açın ve komutuyla Azure aboneliğinizde oturum açın `Connect-AzAccount` ve ekrandaki yönergeleri izleyin.

```powershell
Connect-AzAccount
```

Kimliğiniz birden fazla abonelikle ilişkiliyse, etkin aboneliğinizi ' de dizin oluşturup yönetmek istediğiniz depolama hesabının aboneliğine ayarlayın. Bu örnekte, `<subscription-id>` yer tutucu değerini ABONELIĞINIZIN kimliğiyle değiştirin.

```powershell
Select-AzSubscription -SubscriptionId <subscription-id>
```

Sonra, komutlarınızın depolama hesabı için nasıl yetkilendirme elde etmek istediğinizi seçin. 

### <a name="option-1-obtain-authorization-by-using-azure-active-directory-ad"></a>Seçenek 1: Azure Active Directory kullanarak yetkilendirme alma (AD)

Bu yaklaşımda sistem, Kullanıcı hesabınızın uygun Azure rol tabanlı erişim denetimi (Azure RBAC) atamalarına ve ACL izinlerine sahip olmasını sağlar. 

```powershell
$ctx = New-AzStorageContext -StorageAccountName '<storage-account-name>' -UseConnectedAccount
```

Aşağıdaki tabloda, desteklenen rollerin ve ACL ayar yeteneğinin her biri gösterilmektedir.

|Rol|ACL ayarı özelliği|
|--|--|
|[Depolama Blob Verileri Sahibi](../../role-based-access-control/built-in-roles.md#storage-blob-data-owner)|Hesaptaki tüm dizinler ve dosyalar.|
|[Depolama Blob Verileri Katkıda Bulunanı](../../role-based-access-control/built-in-roles.md#storage-blob-data-contributor)|Yalnızca güvenlik sorumlusuna ait olan dizinler ve dosyalar.|

### <a name="option-2-obtain-authorization-by-using-the-storage-account-key"></a>Seçenek 2: depolama hesabı anahtarını kullanarak yetkilendirme alma

Bu yaklaşımda sistem Azure RBAC veya ACL izinlerini denetlemez.

```powershell
$storageAccount = Get-AzStorageAccount -ResourceGroupName "<resource-group-name>" -AccountName "<storage-account-name>"
$ctx = $storageAccount.Context
```

### <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

1. Azure CLı 'yi yerel olarak kullanıyorsanız Login komutunu çalıştırın.

   ```azurecli
   az login
   ```

   CLı varsayılan tarayıcınızı açabildiğinden, bunu yapılır ve bir Azure oturum açma sayfası yükler.

   Aksi takdirde, konumunda bir tarayıcı sayfası açın [https://aka.ms/devicelogin](https://aka.ms/devicelogin) ve terminalinizde görünen yetkilendirme kodunu girin. Ardından, tarayıcıda hesap kimlik bilgilerinizle oturum açın.

   Farklı kimlik doğrulama yöntemleri hakkında daha fazla bilgi edinmek için bkz. [Azure CLI ile blob veya kuyruk verilerine erişim yetkisi verme](../common/authorize-data-operations-cli.md).

2. Kimliğiniz birden fazla abonelikle ilişkiliyse, etkin aboneliğinizi statik Web sitenizi barındıracak depolama hesabının aboneliğine ayarlayın.

   ```azurecli
   az account set --subscription <subscription-id>
   ```

   `<subscription-id>`Yer tutucu değerini ABONELIĞINIZIN kimliğiyle değiştirin.

> [!NOTE]
> Bu makalede sunulan örnekte Azure Active Directory (AD) yetkilendirmesi gösterilmektedir. Yetkilendirme yöntemleri hakkında daha fazla bilgi edinmek için bkz. [Azure CLI ile blob veya kuyruk verilerine erişim yetkisi verme](../common/authorize-data-operations-cli.md).

### <a name="net"></a>[.NET](#tab/dotnet)

Bu makaledeki kod parçacıklarını kullanmak için depolama hesabını temsil eden bir [DataLakeServiceClient](https://docs.microsoft.com/dotnet/api/azure.storage.files.datalake.datalakeserviceclient) örneği oluşturmanız gerekir.

#### <a name="connect-by-using-azure-active-directory-ad"></a>Azure Active Directory kullanarak bağlanma (AD)

Azure AD ile uygulamanızın kimliğini doğrulamak için [.net Için Azure Identity istemci kitaplığı](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/identity/Azure.Identity) 'nı kullanabilirsiniz.

Paketi yükledikten sonra, bu using ifadesini kod dosyanızın en üstüne ekleyin.

```csharp
using Azure.Identity;
```

İstemci KIMLIĞI, bir istemci parolası ve kiracı KIMLIĞI alın. Bunu yapmak için bkz. [bir istemci uygulamasından istekleri yetkilendirmek Için Azure AD 'den belirteç alma](../common/storage-auth-aad-app.md). Bu işlemin bir parçası olarak, güvenlik sorumlusuna aşağıdaki [Azure rol tabanlı erişim denetimi (Azure RBAC)](../../role-based-access-control/overview.md) rollerinden birini atamanız gerekir. 

|Rol|ACL ayarı özelliği|
|--|--|
|[Depolama Blob Verileri Sahibi](../../role-based-access-control/built-in-roles.md#storage-blob-data-owner)|Hesaptaki tüm dizinler ve dosyalar.|
|[Depolama Blob Verileri Katkıda Bulunanı](../../role-based-access-control/built-in-roles.md#storage-blob-data-contributor)|Yalnızca güvenlik sorumlusuna ait olan dizinler ve dosyalar.|

Bu örnek, bir istemci KIMLIĞI, bir istemci parolası ve bir kiracı KIMLIĞI kullanarak bir [DataLakeServiceClient](https://docs.microsoft.com/dotnet/api/azure.storage.files.datalake.datalakeserviceclient?) örneği oluşturur.  

```cs
public void GetDataLakeServiceClient(ref DataLakeServiceClient dataLakeServiceClient, 
    String accountName, String clientID, string clientSecret, string tenantID)
{

    TokenCredential credential = new ClientSecretCredential(
        tenantID, clientID, clientSecret, new TokenCredentialOptions());

    string dfsUri = "https://" + accountName + ".dfs.core.windows.net";

    dataLakeServiceClient = new DataLakeServiceClient(new Uri(dfsUri), credential);
}

``` 

#### <a name="connect-by-using-an-account-key"></a>Hesap anahtarı kullanarak bağlan

Bu yaklaşım, bir hesaba bağlanmanın en kolay yoludur. 

Bu örnek, bir hesap anahtarı kullanarak bir [DataLakeServiceClient](https://docs.microsoft.com/dotnet/api/azure.storage.files.datalake.datalakeserviceclient?) örneği oluşturur.

```cs
public void GetDataLakeServiceClient(ref DataLakeServiceClient dataLakeServiceClient,
    string accountName, string accountKey)
{
    StorageSharedKeyCredential sharedKeyCredential =
        new StorageSharedKeyCredential(accountName, accountKey);

    string dfsUri = "https://" + accountName + ".dfs.core.windows.net";

    dataLakeServiceClient = new DataLakeServiceClient
        (new Uri(dfsUri), sharedKeyCredential);
}
```

> [!NOTE]
> Daha fazla örnek için bkz. [.net Için Azure kimlik istemci kitaplığı](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/identity/Azure.Identity) belgeleri.

### <a name="java"></a>[Java](#tab/java)

Bu makaledeki kod parçacıklarını kullanmak için depolama hesabını temsil eden bir **DataLakeServiceClient** örneği oluşturmanız gerekir. 

#### <a name="connect-by-using-an-account-key"></a>Hesap anahtarı kullanarak bağlan

Bu, bir hesaba bağlanmanın en kolay yoludur. 

Bu örnek, bir hesap anahtarı kullanarak bir **DataLakeServiceClient** örneği oluşturur.

```java

static public DataLakeServiceClient GetDataLakeServiceClient
(String accountName, String accountKey){

    StorageSharedKeyCredential sharedKeyCredential =
        new StorageSharedKeyCredential(accountName, accountKey);

    DataLakeServiceClientBuilder builder = new DataLakeServiceClientBuilder();

    builder.credential(sharedKeyCredential);
    builder.endpoint("https://" + accountName + ".dfs.core.windows.net");

    return builder.buildClient();
}      
```

#### <a name="connect-by-using-azure-active-directory-azure-ad"></a>Azure Active Directory kullanarak bağlanma (Azure AD)

Uygulamanızı Azure AD ile doğrulamak için [Azure kimlik istemci kitaplığı](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/identity/azure-identity) 'nı kullanabilirsiniz.

Bu örnek, bir istemci KIMLIĞI, bir istemci parolası ve bir kiracı KIMLIĞI kullanarak bir **DataLakeServiceClient** örneği oluşturur.  Bu değerleri almak için bkz. [bir istemci uygulamasından istekleri yetkilendirmek Için Azure AD 'den belirteç alma](../common/storage-auth-aad-app.md).

```java
static public DataLakeServiceClient GetDataLakeServiceClient
    (String accountName, String clientId, String ClientSecret, String tenantID){

    String endpoint = "https://" + accountName + ".dfs.core.windows.net";
        
    ClientSecretCredential clientSecretCredential = new ClientSecretCredentialBuilder()
    .clientId(clientId)
    .clientSecret(ClientSecret)
    .tenantId(tenantID)
    .build();
           
    DataLakeServiceClientBuilder builder = new DataLakeServiceClientBuilder();
    return builder.credential(clientSecretCredential).endpoint(endpoint).buildClient();
 } 
```

> [!NOTE]
> Daha fazla örnek için bkz. [Java Için Azure kimlik istemci kitaplığı](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/identity/azure-identity) belgeleri.

### <a name="python"></a>[Python](#tab/python)

Bu makaledeki kod parçacıklarını kullanmak için depolama hesabını temsil eden bir **DataLakeServiceClient** örneği oluşturmanız gerekir. 

### <a name="connect-by-using-azure-active-directory-ad"></a>Azure Active Directory kullanarak bağlanma (AD)

Azure AD ile uygulamanızın kimliğini doğrulamak için [Python Için Azure kimlik istemci kitaplığını](https://pypi.org/project/azure-identity/) kullanabilirsiniz.

Bu örnek, bir istemci KIMLIĞI, bir istemci parolası ve bir kiracı KIMLIĞI kullanarak bir **DataLakeServiceClient** örneği oluşturur.  Bu değerleri almak için bkz. [bir istemci uygulamasından istekleri yetkilendirmek Için Azure AD 'den belirteç alma](../common/storage-auth-aad-app.md). Bu işlemin bir parçası olarak, güvenlik sorumlusuna aşağıdaki [Azure rol tabanlı erişim denetimi (Azure RBAC)](../../role-based-access-control/overview.md) rollerinden birini atamanız gerekir. 

|Rol|ACL ayarı özelliği|
|--|--|
|[Depolama Blob Verileri Sahibi](../../role-based-access-control/built-in-roles.md#storage-blob-data-owner)|Hesaptaki tüm dizinler ve dosyalar.|
|[Depolama Blob Verileri Katkıda Bulunanı](../../role-based-access-control/built-in-roles.md#storage-blob-data-contributor)|Yalnızca güvenlik sorumlusuna ait olan dizinler ve dosyalar.|

```python
def initialize_storage_account_ad(storage_account_name, client_id, client_secret, tenant_id):
    
    try:  
        global service_client

        credential = ClientSecretCredential(tenant_id, client_id, client_secret)

        service_client = DataLakeServiceClient(account_url="{}://{}.dfs.core.windows.net".format(
            "https", storage_account_name), credential=credential)
    
    except Exception as e:
        print(e)
```

> [!NOTE]
> Daha fazla örnek için bkz. [Python Için Azure kimlik istemci kitaplığı](https://pypi.org/project/azure-identity/) belgeleri.

### <a name="connect-by-using-an-account-key"></a>Hesap anahtarı kullanarak bağlan

Bu, bir hesaba bağlanmanın en kolay yoludur. 

Bu örnek, bir hesap anahtarı kullanarak bir **DataLakeServiceClient** örneği oluşturur.

```python
try:  
    global service_client
        
    service_client = DataLakeServiceClient(account_url="{}://{}.dfs.core.windows.net".format(
        "https", storage_account_name), credential=storage_account_key)
    
except Exception as e:
    print(e)
```
 
- `storage_account_name`Yer tutucu değerini depolama hesabınızın adıyla değiştirin.

- `storage_account_key`Yer tutucu değerini depolama hesabı erişim anahtarınızla değiştirin.

---

## <a name="set-an-acl-recursively"></a>ACL 'yi yinelemeli olarak ayarlama

Bir ACL *ayarladığınızda* , tüm ACL 'leri dahil olmak üzere tüm ACL 'yi **değiştirirsiniz** . Bir güvenlik sorumlusunun izin düzeyini değiştirmek veya var olan girişleri etkilemeden ACL 'ye yeni bir güvenlik sorumlusu eklemek istiyorsanız, ACL 'yi *güncelleştirmeniz* gerekir. Bunu değiştirmek yerine bir ACL 'yi güncelleştirmek için, bu makalenin bir [ACL yinelemeli olarak güncelleştirme](#update-an-acl-recursively) bölümüne bakın.   

Bu bölüm, ACL ayarlama ile ilgili örnekleri içerir 

### <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

**Set-AzDataLakeGen2AclRecursive** cmdlet 'ini kullanarak bir ACL 'yi yinelemeli olarak ayarlayın.

Bu örnek, adlı bir dizinin ACL 'sini ayarlar `my-parent-directory` . Bu girişler, sahip olan kullanıcıya okuma, yazma ve yürütme izinleri verir, sahip olan gruba yalnızca okuma ve yürütme izinleri verir ve tüm diğerlerine erişim vermez. Bu örnekteki son ACL girişi, "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx" okuma ve yürütme izinlerine sahip belirli bir kullanıcıya izin verir.

```powershell
$filesystemName = "my-container"
$dirname = "my-parent-directory/"
$userID = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx";

$acl = Set-AzDataLakeGen2ItemAclObject -AccessControlType user -Permission rwx 
$acl = Set-AzDataLakeGen2ItemAclObject -AccessControlType group -Permission r-x -InputObject $acl 
$acl = Set-AzDataLakeGen2ItemAclObject -AccessControlType other -Permission "---" -InputObject $acl
$acl = Set-AzDataLakeGen2ItemAclObject -AccessControlType user -EntityId $userID -Permission r-x -InputObject $acl 

Set-AzDataLakeGen2AclRecursive -Context $ctx -FileSystem $filesystemName -Path $dirname -Acl $acl

```

> [!NOTE]
> **Varsayılan** bir ACL girişi ayarlamak Istiyorsanız, **set-AzDataLakeGen2ItemAclObject** komutunu çalıştırdığınızda **-DefaultScope** parametresini kullanın. Örneğin: `$acl = set-AzDataLakeGen2ItemAclObject -AccessControlType user -Permission rwx -DefaultScope`.

### <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

[Az Storage FS Access set-özyinelemeli](https://docs.microsoft.com/cli/azure/storage/fs/access#az_storage_fs_access_set_recursive) komutunu kullanarak bir ACL 'yi yinelemeli olarak ayarlayın.

Bu örnek, adlı bir dizinin ACL 'sini ayarlar `my-parent-directory` . Bu girişler, sahip olan kullanıcıya okuma, yazma ve yürütme izinleri verir, sahip olan gruba yalnızca okuma ve yürütme izinleri verir ve tüm diğerlerine erişim vermez. Bu örnekteki son ACL girişi, "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx" okuma ve yürütme izinlerine sahip belirli bir kullanıcıya izin verir.

```azurecli
az storage fs access set-recursive --acl "user::rwx,group::r-x,other::---,user:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx:r-x" -p my-parent-directory/ -f my-container --account-name mystorageaccount --auth-mode login
```

> [!NOTE]
> **Varsayılan** bir ACL girişi ayarlamak istiyorsanız, `default:` Her girişe öneki ekleyin. Örneğin `default:user::rwx` veya `default:user:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx:r-x` olabilir. 

### <a name="net"></a>[.NET](#tab/dotnet)

**Datalakedirectoryclient. SetAccessControlRecursiveAsync** yöntemini ÇAĞıRARAK bir ACL 'yi yinelemeli olarak ayarlayın. Bu yönteme [Pathaccesscontrolıdıtem](/dotnet/api/azure.storage.files.datalake.models.pathaccesscontrolitem) [listesi](/dotnet/api/system.collections.generic.list-1) geçirin. Her [Pathaccesscontrolıdıtem](/dotnet/api/azure.storage.files.datalake.models.pathaccesscontrolitem) bir ACL girdisi tanımlar. 

**Varsayılan** bir ACL girişi ayarlamak Istiyorsanız [pathaccesscontrolıdıtem](/dotnet/api/azure.storage.files.datalake.models.pathaccesscontrolitem) öğesinin [pathaccesscontrolıdıtem. DefaultScope](/dotnet/api/azure.storage.files.datalake.models.pathaccesscontrolitem.defaultscope#Azure_Storage_Files_DataLake_Models_PathAccessControlItem_DefaultScope) özelliğini **true** olarak ayarlayabilirsiniz. 

Bu örnek, adlı bir dizinin ACL 'sini ayarlar `my-parent-directory` . Bu yöntem `isDefaultScope` , varsayılan ACL 'nin ayarlanmış olup olmayacağını belirten adlı bir Boole parametresini kabul eder. Bu parametre [Pathaccesscontrolıdıtem](/dotnet/api/azure.storage.files.datalake.models.pathaccesscontrolitem)kurucusunda kullanılır. ACL 'nin girişleri, sahip olan kullanıcıya okuma, yazma ve yürütme izinleri verir, sahip olan gruba yalnızca okuma ve yürütme izinleri verir ve tüm diğerlerine erişim vermez. Bu örnekteki son ACL girişi, "" xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx "okuma ve yürütme izinlerine sahip belirli bir kullanıcıya izin verir.

```cs
public async void SetACLRecursively(DataLakeServiceClient serviceClient, bool isDefaultScope)
{
    DataLakeDirectoryClient directoryClient =
        serviceClient.GetFileSystemClient("my-container").
            GetDirectoryClient("my-parent-directory");

    List<PathAccessControlItem> accessControlList = 
        new List<PathAccessControlItem>() 
    {
        new PathAccessControlItem(AccessControlType.User, 
            RolePermissions.Read | 
            RolePermissions.Write | 
            RolePermissions.Execute, isDefaultScope),
                    
        new PathAccessControlItem(AccessControlType.Group, 
            RolePermissions.Read | 
            RolePermissions.Execute, isDefaultScope),
                    
        new PathAccessControlItem(AccessControlType.Other, 
            RolePermissions.None, isDefaultScope),

        new PathAccessControlItem(AccessControlType.User, 
            RolePermissions.Read | 
            RolePermissions.Execute, isDefaultScope,
            entityId: "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"),
    };

    await directoryClient.SetAccessControlRecursiveAsync
        (accessControlList, null);
}

```

### <a name="java"></a>[Java](#tab/java)

**Datalakedirectoryclient. Setaccesscontrolözyinelemeli** yöntemini ÇAĞıRARAK bir ACL 'yi yinelemeli olarak ayarlayın. Bu yöntemi [Pathaccesscontrolentry](https://azuresdkdocs.blob.core.windows.net/$web/java/azure-storage-file-datalake/12.3.0-beta.1/index.html) nesnelerinin bir [listesini](https://docs.oracle.com/javase/8/docs/api/java/util/List.html) geçirin. Her [Pathaccesscontrolentry](https://azuresdkdocs.blob.core.windows.net/$web/java/azure-storage-file-datalake/12.3.0-beta.1/index.html) bir ACL girdisi tanımlar. 

**Varsayılan** bir ACL girişi ayarlamak Istiyorsanız, [Pathaccesscontrolentry](https://azuresdkdocs.blob.core.windows.net/$web/java/azure-storage-file-datalake/12.3.0-beta.1/index.html) ve pass öğesinin **setdefaultscope** yöntemini **doğru** değerine çağırabilirsiniz. 

Bu örnek, adlı bir dizinin ACL 'sini ayarlar `my-parent-directory` . Bu yöntem `isDefaultScope` , varsayılan ACL 'nin ayarlanmış olup olmayacağını belirten adlı bir Boole parametresini kabul eder. Bu parametre, [Pathaccesscontrolentry](https://azuresdkdocs.blob.core.windows.net/$web/java/azure-storage-file-datalake/12.3.0-beta.1/index.html)öğesinin **setdefaultscope** metoduna yapılan her çağrıda kullanılır. ACL 'nin girişleri, sahip olan kullanıcıya okuma, yazma ve yürütme izinleri verir, sahip olan gruba yalnızca okuma ve yürütme izinleri verir ve tüm diğerlerine erişim vermez. Bu örnekteki son ACL girişi, "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx" okuma ve yürütme izinlerine sahip belirli bir kullanıcıya izin verir.

```java
static public void SetACLRecursively(DataLakeFileSystemClient fileSystemClient, Boolean isDefaultScope){
    
    DataLakeDirectoryClient directoryClient =
        fileSystemClient.getDirectoryClient("my-parent-directory");

    List<PathAccessControlEntry> pathAccessControlEntries = 
        new ArrayList<PathAccessControlEntry>();

    // Create owner entry.
    PathAccessControlEntry ownerEntry = new PathAccessControlEntry();

    RolePermissions ownerPermission = new RolePermissions();
    ownerPermission.setExecutePermission(true).setReadPermission(true).setWritePermission(true);

    ownerEntry.setDefaultScope(isDefaultScope);
    ownerEntry.setAccessControlType(AccessControlType.USER);
    ownerEntry.setPermissions(ownerPermission);

    pathAccessControlEntries.add(ownerEntry);

    // Create group entry.
    PathAccessControlEntry groupEntry = new PathAccessControlEntry();

    RolePermissions groupPermission = new RolePermissions();
    groupPermission.setExecutePermission(true).setReadPermission(true).setWritePermission(false);

    groupEntry.setDefaultScope(isDefaultScope);
    groupEntry.setAccessControlType(AccessControlType.GROUP);
    groupEntry.setPermissions(groupPermission);

    pathAccessControlEntries.add(groupEntry);

    // Create other entry.
    PathAccessControlEntry otherEntry = new PathAccessControlEntry();

    RolePermissions otherPermission = new RolePermissions();
    otherPermission.setExecutePermission(false).setReadPermission(false).setWritePermission(false);

    otherEntry.setDefaultScope(isDefaultScope);
    otherEntry.setAccessControlType(AccessControlType.OTHER);
    otherEntry.setPermissions(otherPermission);

    pathAccessControlEntries.add(otherEntry);

    // Create named user entry.
    PathAccessControlEntry userEntry = new PathAccessControlEntry();

    RolePermissions userPermission = new RolePermissions();
    userPermission.setExecutePermission(true).setReadPermission(true).setWritePermission(false);

    userEntry.setDefaultScope(isDefaultScope);
    userEntry.setAccessControlType(AccessControlType.USER);
    userEntry.setEntityId("xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx");
    userEntry.setPermissions(userPermission);    
    
    pathAccessControlEntries.add(userEntry);
    
    directoryClient.setAccessControlRecursive(pathAccessControlEntries);        

}
```

### <a name="python"></a>[Python](#tab/python)

**DataLakeDirectoryClient.set_access_control_recursive** yöntemini ÇAĞıRARAK bir ACL 'yi yinelemeli olarak ayarlayın.

**Varsayılan** bir ACL girişi ayarlamak istiyorsanız, DIZEYI `default:` her ACL Giriş dizesinin başına ekleyin. 

Bu örnek, adlı bir dizinin ACL 'sini ayarlar `my-parent-directory` . 

Bu yöntem `is_default_scope` , varsayılan ACL 'nin ayarlanmış olup olmayacağını belirten adlı bir Boole parametresini kabul eder. Bu parametre ise `True` , ACL girişlerinin listesi öncesinde dize olur `default:` . 

ACL 'nin girişleri, sahip olan kullanıcıya okuma, yazma ve yürütme izinleri verir, sahip olan gruba yalnızca okuma ve yürütme izinleri verir ve tüm diğerlerine erişim vermez. Bu örnekteki son ACL girişi, "" xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx "okuma ve yürütme izinlerine sahip belirli bir kullanıcıya izin verir. Bu girişler, sahip olan kullanıcıya okuma, yazma ve yürütme izinleri verir, sahip olan gruba yalnızca okuma ve yürütme izinleri verir ve tüm diğerlerine erişim vermez. Bu örnekteki son ACL girişi, "" xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx "okuma ve yürütme izinlerine sahip belirli bir kullanıcıya izin verir.

```python
def set_permission_recursively(is_default_scope):
    
    try:
        file_system_client = service_client.get_file_system_client(file_system="my-container")

        directory_client = file_system_client.get_directory_client("my-parent-directory")
              
        acl = 'user::rwx,group::rwx,other::rwx,user:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx:r--'   

        if is_default_scope:
           acl = 'default:user::rwx,default:group::rwx,default:other::rwx,default:user:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx:r--'

        directory_client.set_access_control_recursive(acl=acl)
        
    except Exception as e:
     print(e)
```

---

## <a name="update-an-acl-recursively"></a>ACL 'yi yinelemeli olarak güncelleştirme

Bir ACL 'yi *güncelleştirdiğinizde* ACL 'yi DEğIşTIRMEk yerine ACL 'yi değiştirirsiniz. Örneğin, ACL 'de listelenen diğer güvenlik sorumlularını etkilemeden ACL 'ye yeni bir güvenlik sorumlusu ekleyebilirsiniz.  ACL 'yi güncelleştirmek yerine değiştirmek için, bu makalenin [BIR ACL özyinelemeli olarak ayarlama](#set-an-acl-recursively) bölümüne bakın. 

Bir ACL 'yi güncelleştirmek için, güncelleştirmek istediğiniz ACL girdisiyle yeni bir ACL nesnesi oluşturun ve ACL 'yi Güncelleştir işleminde bu nesneyi kullanın. Mevcut ACL 'yi kullanmayın, yalnızca görüntülenecek ACL girdilerini sağlayın.

Bu bölüm, ACL güncelleştirme ile ilgili örnekleri içerir.

### <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

**Update-AzDataLakeGen2AclRecursive** cmdlet 'ini kullanarak bir ACL 'yi yinelemeli olarak güncelleştirin. 

Bu örnekte, yazma izniyle bir ACL girişi güncelleştirilir. 

```powershell
$filesystemName = "my-container"
$dirname = "my-parent-directory/"
$userID = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx";

$acl = Set-AzDataLakeGen2ItemAclObject -AccessControlType user -EntityId $userID -Permission rwx

Update-AzDataLakeGen2AclRecursive -Context $ctx -FileSystem $filesystemName -Path $dirname -Acl $acl

```

> [!NOTE]
> **Varsayılan** bir ACL girişini güncelleştirmek Istiyorsanız, **set-AzDataLakeGen2ItemAclObject** komutunu çalıştırdığınızda **-DefaultScope** parametresini kullanın. Örneğin: `$acl = set-AzDataLakeGen2ItemAclObject -AccessControlType user -EntityId $userID -Permission rwx -DefaultScope`.

### <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

[Az Storage FS Access Update-özyinelemeli](https://docs.microsoft.com/cli/azure/storage/fs/access#az_storage_fs_access_update_recursive) komutunu kullanarak bir ACL 'yi yinelemeli olarak güncelleştirin. 

Bu örnekte, yazma izniyle bir ACL girişi güncelleştirilir. 

```azurecli
az storage fs access update-recursive --acl "user:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx:rwx" -p my-parent-directory/ -f my-container --account-name mystorageaccount --auth-mode login
```

> [!NOTE]
> **Varsayılan** bir ACL girişini güncelleştirmek istiyorsanız, `default:` Her girişe öneki ekleyin. Örneğin, `default:user:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx:r-x`.

### <a name="net"></a>[.NET](#tab/dotnet)

**Datalakedirectoryclient. UpdateAccessControlRecursiveAsync** yöntemini ÇAĞıRARAK bir ACL 'yi yinelemeli olarak güncelleştirin.  Bu yönteme [Pathaccesscontrolıdıtem](/dotnet/api/azure.storage.files.datalake.models.pathaccesscontrolitem) [listesi](/dotnet/api/system.collections.generic.list-1) geçirin. Her [Pathaccesscontrolıdıtem](/dotnet/api/azure.storage.files.datalake.models.pathaccesscontrolitem) bir ACL girdisi tanımlar. 

**Varsayılan** bir ACL girişini güncelleştirmek Istiyorsanız, [pathaccesscontrolıdıtem](/dotnet/api/azure.storage.files.datalake.models.pathaccesscontrolitem) öğesinin [Pathaccesscontrolıdıtem. DefaultScope](/dotnet/api/azure.storage.files.datalake.models.pathaccesscontrolitem.defaultscope#Azure_Storage_Files_DataLake_Models_PathAccessControlItem_DefaultScope) özelliğini **true** olarak ayarlayabilirsiniz. 

Bu örnekte, yazma izniyle bir ACL girişi güncelleştirilir. Bu yöntem `isDefaultScope` , varsayılan ACL 'nin güncelleştirilmesini isteyip istemediğinizi belirten adlı bir Boole parametresini kabul eder. Bu parametre [Pathaccesscontrolıdıtem](/dotnet/api/azure.storage.files.datalake.models.pathaccesscontrolitem)kurucusunda kullanılır.

```cs
public async void UpdateACLsRecursively(DataLakeServiceClient serviceClient, bool isDefaultScope)
{
    DataLakeDirectoryClient directoryClient =
        serviceClient.GetFileSystemClient("my-container").
        GetDirectoryClient("my-parent-directory");

    List<PathAccessControlItem> accessControlListUpdate = 
        new List<PathAccessControlItem>()
    {
        new PathAccessControlItem(AccessControlType.User, 
            RolePermissions.Read |
            RolePermissions.Write | 
            RolePermissions.Execute, isDefaultScope, 
            entityId: "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"),
    };

    await directoryClient.UpdateAccessControlRecursiveAsync
        (accessControlListUpdate, null);

}
```

### <a name="java"></a>[Java](#tab/java)

**Datalakedirectoryclient. Updateaccesscontrolözyinelemeli** yöntemini ÇAĞıRARAK bir ACL 'yi yinelemeli olarak güncelleştirin.  Bu yöntemi [Pathaccesscontrolentry](https://azuresdkdocs.blob.core.windows.net/$web/java/azure-storage-file-datalake/12.3.0-beta.1/index.html) nesnelerinin bir [listesini](https://docs.oracle.com/javase/8/docs/api/java/util/List.html) geçirin. Her [Pathaccesscontrolentry](https://azuresdkdocs.blob.core.windows.net/$web/java/azure-storage-file-datalake/12.3.0-beta.1/index.html) bir ACL girdisi tanımlar. 

**Varsayılan** bir ACL girişini güncelleştirmek Istiyorsanız, [Pathaccesscontrolentry](https://azuresdkdocs.blob.core.windows.net/$web/java/azure-storage-file-datalake/12.3.0-beta.1/index.html) ve pass öğesinin **setdefaultscope** yöntemine **doğru** değerini verebilirsiniz. 

Bu örnekte, yazma izniyle bir ACL girişi güncelleştirilir. Bu yöntem `isDefaultScope` , varsayılan ACL 'nin güncelleştirilmesini isteyip istemediğinizi belirten adlı bir Boole parametresini kabul eder. Bu parametre, [Pathaccesscontrolentry](https://azuresdkdocs.blob.core.windows.net/$web/java/azure-storage-file-datalake/12.3.0-beta.1/index.html)öğesinin **setdefaultscope** metoduna yapılan çağrıda kullanılır. 

```java
static public void UpdateACLRecursively(DataLakeFileSystemClient fileSystemClient, Boolean isDefaultScope){

    DataLakeDirectoryClient directoryClient =
    fileSystemClient.getDirectoryClient("my-parent-directory");

    List<PathAccessControlEntry> pathAccessControlEntries = 
        new ArrayList<PathAccessControlEntry>();

    // Create named user entry.
    PathAccessControlEntry userEntry = new PathAccessControlEntry();

    RolePermissions userPermission = new RolePermissions();
    userPermission.setExecutePermission(true).setReadPermission(true).setWritePermission(true);

    userEntry.setDefaultScope(isDefaultScope);
    userEntry.setAccessControlType(AccessControlType.USER);
    userEntry.setEntityId("xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx");
    userEntry.setPermissions(userPermission);    
    
    pathAccessControlEntries.add(userEntry);
    
    directoryClient.updateAccessControlRecursive(pathAccessControlEntries);          
}
```

### <a name="python"></a>[Python](#tab/python)

**DataLakeDirectoryClient.update_access_control_recursive** yöntemini ÇAĞıRARAK bir ACL 'yi yinelemeli olarak güncelleştirin. **Varsayılan** bir ACL girişini güncelleştirmek istiyorsanız, DIZEYI `default:` her ACL Giriş dizesinin başına ekleyin. 

Bu örnekte, yazma izniyle bir ACL girişi güncelleştirilir.

Bu örnek, adlı bir dizinin ACL 'sini ayarlar `my-parent-directory` . Bu yöntem `is_default_scope` , varsayılan ACL 'nin güncelleştirilmesini isteyip istemediğinizi belirten adlı bir Boole parametresini kabul eder. Bu parametre ise `True` , GÜNCELLEŞTIRILMIŞ ACL girişi öncesinde dize olur `default:` .  

```python
def update_permission_recursively(is_default_scope):
    
    try:
        file_system_client = service_client.get_file_system_client(file_system="my-container")

        directory_client = file_system_client.get_directory_client("my-parent-directory")

        acl = 'user:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx:rwx'   

        if is_default_scope:
           acl = 'default:user:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx:rwx'

        directory_client.update_access_control_recursive(acl=acl)

        acl_props = directory_client.get_access_control()
        
        print(acl_props['permissions'])

    except Exception as e:
     print(e)
```

---

## <a name="remove-acl-entries-recursively"></a>ACL girdilerini yinelemeli olarak kaldır

Bir veya daha fazla ACL girişini yinelemeli olarak kaldırabilirsiniz. Bir ACL girişini kaldırmak için, ACL girdisi kaldırılacak yeni bir ACL nesnesi oluşturun ve sonra ACL 'yi kaldır işleminde bu nesneyi kullanın. Mevcut ACL 'yi almaz, kaldırılacak ACL girdilerini sağlamanız yeterlidir. 

Bu bölüm, ACL 'nin nasıl kaldırılacağını gösteren örnekleri içerir. 

### <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

**Remove-AzDataLakeGen2AclRecursive** cmdlet 'INI kullanarak acl girişlerini kaldırın. 

Bu örnek, kapsayıcının kök dizininden bir ACL girişini kaldırır.  

```powershell
$filesystemName = "my-container"
$userID = "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"

$acl = Set-AzDataLakeGen2ItemAclObject -AccessControlType user -EntityId $userID -Permission "---" 

Remove-AzDataLakeGen2AclRecursive -Context $ctx -FileSystem $filesystemName  -Acl $acl
```

> [!NOTE]
> **Varsayılan** bir ACL girişini kaldırmak Istiyorsanız, **set-AzDataLakeGen2ItemAclObject** komutunu çalıştırdığınızda **-DefaultScope** parametresini kullanın. Örneğin: `$acl = set-AzDataLakeGen2ItemAclObject -AccessControlType user -EntityId $userID -Permission "---" -DefaultScope`.

### <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

[Az Storage FS Access Remove-özyinelemeli](https://docs.microsoft.com/cli/azure/storage/fs/access#az_storage_fs_access_remove_recursive) komutunu kullanarak acl girişlerini kaldırın. 

Bu örnek, kapsayıcının kök dizininden bir ACL girişini kaldırır.  

```azurecli
az storage fs access remove-recursive --acl "user:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx" -p my-parent-directory/ -f my-container --account-name mystorageaccount --auth-mode login
```

> [!NOTE]
> **Varsayılan** bir ACL girişini kaldırmak istiyorsanız, `default:` Her girişe öneki ekleyin. Örneğin, `default:user:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx`.

### <a name="net"></a>[.NET](#tab/dotnet)

**Datalakedirectoryclient. RemoveAccessControlRecursiveAsync** YÖNTEMINI çağırarak acl girişlerini kaldırın. Bu yönteme [Pathaccesscontrolıdıtem](/dotnet/api/azure.storage.files.datalake.models.pathaccesscontrolitem) [listesi](/dotnet/api/system.collections.generic.list-1) geçirin. Her [Pathaccesscontrolıdıtem](/dotnet/api/azure.storage.files.datalake.models.pathaccesscontrolitem) bir ACL girdisi tanımlar. 

**Varsayılan** bir ACL girişini kaldırmak Istiyorsanız, [pathaccesscontrolıdıtem](/dotnet/api/azure.storage.files.datalake.models.pathaccesscontrolitem) öğesinin [pathaccesscontrolıdıtem. DefaultScope](/dotnet/api/azure.storage.files.datalake.models.pathaccesscontrolitem.defaultscope#Azure_Storage_Files_DataLake_Models_PathAccessControlItem_DefaultScope) özelliğini **true** olarak ayarlayabilirsiniz. 

Bu örnek, adlı dizinin ACL 'sinden bir ACL girişini kaldırır `my-parent-directory` . Bu yöntem `isDefaultScope` , girişin varsayılan ACL 'den kaldırılması gerekip gerekmediğini belirten adlı bir Boole parametresini kabul eder. Bu parametre [Pathaccesscontrolıdıtem](/dotnet/api/azure.storage.files.datalake.models.pathaccesscontrolitem)kurucusunda kullanılır.

```cs
public async void RemoveACLsRecursively(DataLakeServiceClient serviceClient, isDefaultScope)
{
    DataLakeDirectoryClient directoryClient =
        serviceClient.GetFileSystemClient("my-container").
            GetDirectoryClient("my-parent-directory");

    List<RemovePathAccessControlItem> accessControlListForRemoval = 
        new List<RemovePathAccessControlItem>()
        {
            new RemovePathAccessControlItem(AccessControlType.User, isDefaultScope,
            entityId: "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"),
        };

    await directoryClient.RemoveAccessControlRecursiveAsync
        (accessControlListForRemoval, null);

}
```

### <a name="java"></a>[Java](#tab/java)

**Datalakedirectoryclient. removeAccessControlRecursive** YÖNTEMINI çağırarak acl girişlerini kaldırın. Bu yöntemi [Pathaccesscontrolentry](https://azuresdkdocs.blob.core.windows.net/$web/java/azure-storage-file-datalake/12.3.0-beta.1/index.html) nesnelerinin bir [listesini](https://docs.oracle.com/javase/8/docs/api/java/util/List.html) geçirin. Her [Pathaccesscontrolentry](https://azuresdkdocs.blob.core.windows.net/$web/java/azure-storage-file-datalake/12.3.0-beta.1/index.html) bir ACL girdisi tanımlar. 

**Varsayılan** bir ACL girişini kaldırmak Istiyorsanız, [Pathaccesscontrolentry](https://azuresdkdocs.blob.core.windows.net/$web/java/azure-storage-file-datalake/12.3.0-beta.1/index.html) ve pass öğesinin **setdefaultscope** yöntemine **true** değerini verebilirsiniz.  

Bu örnek, adlı dizinin ACL 'sinden bir ACL girişini kaldırır `my-parent-directory` . Bu yöntem `isDefaultScope` , girişin varsayılan ACL 'den kaldırılması gerekip gerekmediğini belirten adlı bir Boole parametresini kabul eder. Bu parametre, [Pathaccesscontrolentry](https://azuresdkdocs.blob.core.windows.net/$web/java/azure-storage-file-datalake/12.3.0-beta.1/index.html)öğesinin **setdefaultscope** metoduna yapılan çağrıda kullanılır.


```java
static public void RemoveACLRecursively(DataLakeFileSystemClient fileSystemClient, Boolean isDefaultScope){

    DataLakeDirectoryClient directoryClient =
    fileSystemClient.getDirectoryClient("my-parent-directory");

    List<PathRemoveAccessControlEntry> pathRemoveAccessControlEntries = 
        new ArrayList<PathRemoveAccessControlEntry>();

    // Create named user entry.
    PathRemoveAccessControlEntry userEntry = new PathRemoveAccessControlEntry();

    RolePermissions userPermission = new RolePermissions();
    userPermission.setExecutePermission(true).setReadPermission(true).setWritePermission(true);

    userEntry.setDefaultScope(isDefaultScope);
    userEntry.setAccessControlType(AccessControlType.USER);
    userEntry.setEntityId("xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"); 
    
    pathRemoveAccessControlEntries.add(userEntry);
    
    directoryClient.removeAccessControlRecursive(pathRemoveAccessControlEntries);      

}
```

### <a name="python"></a>[Python](#tab/python)

**DataLakeDirectoryClient.remove_access_control_recursive** YÖNTEMINI çağırarak acl girişlerini kaldırın. **Varsayılan** bir ACL girişini kaldırmak istiyorsanız, dizeyi `default:` acl giriş dizesinin başına ekleyin. 

Bu örnek, adlı dizinin ACL 'sinden bir ACL girişini kaldırır `my-parent-directory` . Bu yöntem `is_default_scope` , girişin varsayılan ACL 'den kaldırılması gerekip gerekmediğini belirten adlı bir Boole parametresini kabul eder. Bu parametre ise `True` , GÜNCELLEŞTIRILMIŞ ACL girişi öncesinde dize olur `default:` . 

```python
def remove_permission_recursively(is_default_scope):
    
    try:
        file_system_client = service_client.get_file_system_client(file_system="my-container")

        directory_client = file_system_client.get_directory_client("my-parent-directory")
              
        acl = 'user:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx'

        if is_default_scope:
           acl = 'default:user:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx'

        directory_client.remove_access_control_recursive(acl=acl)

    except Exception as e:
     print(e)
```

---

## <a name="recover-from-failures"></a>Hatalardan kurtarma

Çalışma zamanı veya izin hatalarıyla karşılaşabilirsiniz. Çalışma zamanı hataları için işlemi baştan başlatın. Güvenlik sorumlusu, değiştirilen Dizin hiyerarşisindeki bir dizinin veya dosyanın ACL 'sini değiştirmek için yeterli izne sahip değilse, izin hataları oluşabilir. İzin sorununu çözün ve sonra bir devamlılık belirteci kullanarak işlemi hata noktasından sürdürmeyi seçin ya da işlemi baştan başlatın. Baştan başlatmayı tercih ediyorsanız devamlılık belirtecini kullanmanız gerekmez. ACL girdilerini negatif bir etki olmadan yeniden uygulayabilirsiniz.

### <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Sonuçları değişkene döndürün. Hatalı girdileri biçimlendirilen bir tabloya boru.

```powershell
$result = Set-AzDataLakeGen2AclRecursive -Context $ctx -FileSystem $filesystemName -Path $dirname -Acl $acl
$result
$result.FailedEntries | ft 
```

Tablonun çıktısına bağlı olarak, tüm izin hatalarını giderebilir ve sonra devamlılık belirtecini kullanarak yürütmeyi sürdürebilirsiniz.

```powershell
$result = Set-AzDataLakeGen2AclRecursive -Context $ctx -FileSystem $filesystemName -Path $dirname -Acl $acl -ContinuationToken $result.ContinuationToken
$result

```

### <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

Bir hata durumunda, parametresini olarak ayarlayarak bir devamlılık belirteci döndürebilirsiniz `--continue-on-failure` `true` . Hataları adresledikten sonra, komutu yeniden çalıştırarak ve sonra parametreyi devamlılık belirtecine ayarlayarak işlemi hata noktasından devam ettirebilirsiniz `--continuation` . 

```azurecli
az storage fs access set-recursive --acl "user::rw-,group::r-x,other::---" --continue-on-failure true --continuation xxxxxxx -p my-parent-directory/ -f my-container --account-name mystorageaccount --auth-mode login  
```

## <a name="net"></a>[.NET](#tab/dotnet)

Bu örnek hata durumunda bir devamlılık belirteci döndürür. Uygulama, hata bağlandıktan sonra bu örnek yöntemi yeniden çağırabilir ve devamlılık belirtecini geçiyolabilir. Bu örnek yöntem ilk kez çağrılırsa, uygulama `null` devamlılık belirteci parametresi için değerini geçirebilir. 

```cs
public async Task<string> ResumeAsync(DataLakeServiceClient serviceClient,
    DataLakeDirectoryClient directoryClient,
    List<PathAccessControlItem> accessControlList, 
    string continuationToken)
{
    try
    {
        var accessControlChangeResult =
            await directoryClient.SetAccessControlRecursiveAsync(
                accessControlList, continuationToken: continuationToken, null);

        if (accessControlChangeResult.Value.Counters.FailedChangesCount > 0)
        {
            continuationToken =
                accessControlChangeResult.Value.ContinuationToken;
        }

        return continuationToken;
    }
    catch (Exception ex)
    {
        Console.WriteLine(ex.ToString());
        return continuationToken;
    }

}
```

### <a name="java"></a>[Java](#tab/java)

Bu örnek hata durumunda bir devamlılık belirteci döndürür. Uygulama, hata bağlandıktan sonra bu örnek yöntemi yeniden çağırabilir ve devamlılık belirtecini geçiyolabilir. Bu örnek yöntem ilk kez çağrılırsa, uygulama `null` devamlılık belirteci parametresi için değerini geçirebilir. 

```java
static public String ResumeSetACLRecursively(DataLakeFileSystemClient fileSystemClient,
DataLakeDirectoryClient directoryClient,
List<PathAccessControlEntry> accessControlList, 
String continuationToken){

    try{
        PathSetAccessControlRecursiveOptions options = new PathSetAccessControlRecursiveOptions(accessControlList);
        
        options.setContinuationToken(continuationToken);
    
        Response<AccessControlChangeResult> accessControlChangeResult =  
            directoryClient.setAccessControlRecursiveWithResponse(options, null, null);

        if (accessControlChangeResult.getValue().getCounters().getFailedChangesCount() > 0)
        {
            continuationToken =
                accessControlChangeResult.getValue().getContinuationToken();
        }
    
        return continuationToken;

    }
    catch(Exception ex){
    
        System.out.println(ex.toString());
        return continuationToken;
    }
}
```

### <a name="python"></a>[Python](#tab/python)

Bu örnek hata durumunda bir devamlılık belirteci döndürür. Uygulama, hata bağlandıktan sonra bu örnek yöntemi yeniden çağırabilir ve devamlılık belirtecini geçiyolabilir. Bu örnek yöntem ilk kez çağrılırsa, uygulama ``None`` devamlılık belirteci parametresi için değerini geçirebilir. 

```python
def resume_set_acl_recursive(continuation_token):
    
    try:
        file_system_client = service_client.get_file_system_client(file_system="my-container")

        directory_client = file_system_client.get_directory_client("my-parent-directory")
              
        acl = 'user::rwx,group::rwx,other::rwx,user:xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx:r-x'

        acl_change_result = directory_client.set_access_control_recursive(acl=acl, continuation=continuation_token)

        continuation_token = acl_change_result.continuation

        return continuation_token
        
    except Exception as e:
     print(e) 
     return continuation_token
```

---

## <a name="resources"></a>Kaynaklar

Bu bölüm, kitaplıkların ve kod örneklerinin bağlantılarını içerir.

#### <a name="libraries"></a>Kitaplıklar

- [PowerShell](https://nam06.safelinks.protection.outlook.com/?url=https%3A%2F%2Fwww.powershellgallery.com%2Fpackages%2FAz.Storage%2F2.5.2-preview&data=02%7C01%7Cnormesta%40microsoft.com%7Ccdabce06132c42132b4008d849a2dfb1%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C0%7C637340311173215017&sdata=FWynO9UKTt7ESMCFgkWaL7J%2F%2BjODaRo5BD6G6yCx9os%3D&reserved=0)
- [Azure CLI](https://docs.microsoft.com/cli/azure/storage/fs/access)
- [.NET](https://pkgs.dev.azure.com/azure-sdk/public/_packaging/azure-sdk-for-net/nuget/v3/index.json)
- [Java](/java/api/overview/azure/storage-file-datalake-readme)
- [Python](https://nam06.safelinks.protection.outlook.com/?url=https%3A%2F%2Frecursiveaclpr.blob.core.windows.net%2Fprivatedrop%2Fazure_storage_file_datalake-12.1.0b99-py2.py3-none-any.whl%3Fsv%3D2019-02-02%26st%3D2020-08-24T07%253A47%253A01Z%26se%3D2021-08-25T07%253A47%253A00Z%26sr%3Db%26sp%3Dr%26sig%3DH1XYw4FTLJse%252BYQ%252BfamVL21UPVIKRnnh2mfudA%252BfI0I%253D&data=02%7C01%7Cnormesta%40microsoft.com%7C95a5966d938a4902560e08d84912fe32%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C0%7C637339693209725909&sdata=acv4KWZdzkITw1lP0%2FiA3lZuW7NF5JObjY26IXttfGI%3D&reserved=0)

#### <a name="code-samples"></a>Kod örnekleri

- PowerShell: [Benioku](https://recursiveaclpr.blob.core.windows.net/privatedrop/README.txt?sv=2019-02-02&st=2020-08-24T17%3A03%3A18Z&se=2021-08-25T17%3A03%3A00Z&sr=b&sp=r&sig=sPdKiCSXWExV62sByeOYqBTqpGmV2h9o8BLij3iPkNQ%3D)  |  [örneği](https://recursiveaclpr.blob.core.windows.net/privatedrop/samplePS.ps1?sv=2019-02-02&st=2020-08-24T17%3A04%3A44Z&se=2021-08-25T17%3A04%3A00Z&sr=b&sp=r&sig=dNNKS%2BZcp%2F1gl6yOx6QLZ6OpmXkN88ZjBeBtym1Mejo%3D)

- Azure CLı: [örnek](https://github.com/Azure/azure-cli/blob/2a55a5350696a3a93a13f364f2104ec8bc82cdd3/src/azure-cli/azure/cli/command_modules/storage/docs/ADLS%20Gen2.md)

- NET: [Benioku](https://nam06.safelinks.protection.outlook.com/?url=https%3A%2F%2Frecursiveaclpr.blob.core.windows.net%2Fprivatedrop%2FREADME%2520for%2520net%3Fsv%3D2019-02-02%26st%3D2020-08-25T23%253A20%253A42Z%26se%3D2021-08-26T23%253A20%253A00Z%26sr%3Db%26sp%3Dr%26sig%3DKrnHvasHoSoVeUyr2g%252FSc2aDVW3De4A%252Fvx0lFWZs494%253D&data=02%7C01%7Cnormesta%40microsoft.com%7Cda902e4fe6c24e6a07d908d8494fd4bd%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C0%7C637339954503767961&sdata=gd%2B2LphTtDFVb7pZko9rkGO9OG%2FVvmeXprHB9IOEYXE%3D&reserved=0)  |  [örneği](https://nam06.safelinks.protection.outlook.com/?url=https%3A%2F%2Frecursiveaclpr.blob.core.windows.net%2Fprivatedrop%2FRecursive-Acl-Sample-Net.zip%3Fsv%3D2019-02-02%26st%3D2020-08-24T07%253A45%253A28Z%26se%3D2021-09-25T07%253A45%253A00Z%26sr%3Db%26sp%3Dr%26sig%3D2GI3f0KaKMZbTi89AgtyGg%252BJePgNSsHKCL68V6I5W3s%253D&data=02%7C01%7Cnormesta%40microsoft.com%7C6eae76c57d224fb6de8908d848525330%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C0%7C637338865714571853&sdata=%2FWom8iI3DSDMSw%2FfYvAaQ69zbAoqXNTQ39Q9yVMnASA%3D&reserved=0)

- Python: [Benioku](https://nam06.safelinks.protection.outlook.com/?url=https%3A%2F%2Frecursiveaclpr.blob.core.windows.net%2Fprivatedrop%2FREADME%2520for%2520python%3Fsv%3D2019-02-02%26st%3D2020-08-25T23%253A21%253A47Z%26se%3D2021-08-26T23%253A21%253A00Z%26sr%3Db%26sp%3Dr%26sig%3DRq6Bl5lXrtYk79thy8wX7UTbjyd2f%252B6xzVBFFVYbdYg%253D&data=02%7C01%7Cnormesta%40microsoft.com%7Cda902e4fe6c24e6a07d908d8494fd4bd%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C0%7C637339954503777915&sdata=3e46Lp2miOHj755Gh0odH3M0%2BdTF3loGCCBENrulVTM%3D&reserved=0)  |  [örneği](https://recursiveaclpr.blob.core.windows.net/privatedrop/datalake_samples_access_control_async.py?sv=2019-02-02&st=2020-08-24T07%3A48%3A10Z&se=2021-08-25T07%3A48%3A00Z&sr=b&sp=r&sig=%2F1c540%2BpXYyNcuTmWPWHg2m9SyClXLIMw7ChLZGsyD0%3D)

## <a name="best-practice-guidelines"></a>En iyi yöntem yönergeleri

Bu bölümde, ACL 'Leri yinelemeli olarak ayarlamaya yönelik bazı en iyi yöntem yönergeleri sunulmaktadır. 

#### <a name="handling-runtime-errors"></a>Çalışma zamanı hatalarını işleme

Birçok nedenden dolayı bir çalışma zamanı hatası oluşabilir (örneğin: bir kesinti veya bir istemci bağlantı sorunu). Bir çalışma zamanı hatasıyla karşılaşırsanız, özyinelemeli ACL işlemini yeniden başlatın. ACL 'Ler, olumsuz bir etkiye neden olmadan öğelere yeniden uygulanabilir. 

#### <a name="handling-permission-errors-403"></a>İzin hatalarını işleme (403)

Özyinelemeli bir ACL işlemi çalıştırırken bir erişim denetimi özel durumuyla karşılaşırsanız, AD [güvenlik sorumlusu](https://docs.microsoft.com/azure/role-based-access-control/overview#security-principal) , Dizin hiyerarşisindeki bir veya daha fazla alt öğeye bir ACL uygulamak için yeterli izne sahip olmayabilir. Bir izin hatası oluştuğunda, işlem duraklar ve devamlılık belirteci sağlanır. İzin sorununu giderip kalan veri kümesini işlemek için devamlılık belirtecini kullanın. Zaten başarıyla işlenen dizinlerin ve dosyaların yeniden işlenmesi gerekmez. Özyinelemeli ACL işlemini de yeniden başlatmayı seçebilirsiniz. ACL 'Ler, olumsuz bir etkiye neden olmadan öğelere yeniden uygulanabilir. 

#### <a name="credentials"></a>Kimlik bilgileri 

Hedef depolama hesabı veya kapsayıcısının kapsamındaki [Depolama Blobu veri sahibi](../../role-based-access-control/built-in-roles.md#storage-blob-data-owner) rolü atanmış BIR Azure AD güvenlik sorumlusu sağlamanızı öneririz. 

#### <a name="performance"></a>Performans 

Gecikme süresini azaltmak için, özyinelemeli ACL işlemini depolama hesabınızla aynı bölgede bulunan bir Azure sanal makinesinde (VM) çalıştırmanızı öneririz. 

#### <a name="acl-limits"></a>ACL sınırları

Bir dizin veya dosyaya uygulayabileceğiniz en fazla ACL sayısı 32 erişim ACL 'Lerine ve 32 varsayılan ACL 'Lerine sahiptir. Daha fazla bilgi için [Azure Data Lake Storage 2. erişim denetimi](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-access-control)bölümüne bakın.

<a id="provide-feedback"></a>

### <a name="provide-feedback-or-report-issues"></a>Geri bildirim veya rapor sorunları sağlama

Görüşlerinizi verebilir veya hakkında bir sorun bildirebilirsiniz  [recursiveACLfeedback@microsoft.com](mailto:recursiveACLfeedback@microsoft.com) .

## <a name="see-also"></a>Ayrıca bkz.

- [Azure Data Lake Storage 2. Nesil'de Erişim Denetimi](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-access-control)
- [Bilinen sorunlar](data-lake-storage-known-issues.md)


