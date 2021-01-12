---
title: "& ACL 'Ler için Java SDK Azure Data Lake Storage 2."
description: Hiyerarşik ad alanı (HNS) etkin olan depolama hesaplarında dizinleri ve dosya ve Dizin erişim denetim listelerini (ACL) yönetmek için Java için Azure depolama kitaplıklarını kullanın.
author: normesta
ms.service: storage
ms.date: 01/11/2021
ms.custom: devx-track-java
ms.author: normesta
ms.topic: how-to
ms.subservice: data-lake-storage-gen2
ms.reviewer: prishet
ms.openlocfilehash: 1cc6954569c509c977634a8e1cdd52c5c55b2100
ms.sourcegitcommit: 48e5379c373f8bd98bc6de439482248cd07ae883
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/12/2021
ms.locfileid: "98108136"
---
# <a name="use-java-to-manage-directories-files-and-acls-in-azure-data-lake-storage-gen2"></a>Azure Data Lake Storage 2. içinde dizinleri, dosyaları ve ACL 'Leri yönetmek için Java kullanın

Bu makalede, Java kullanarak hiyerarşik ad alanı (HNS) etkinleştirilmiş depolama hesaplarında Dizin, dosya ve izinleri oluşturma ve bunları yönetme işlemi gösterilmektedir. 

[Paket (Maven)](https://search.maven.org/artifact/com.azure/azure-storage-file-datalake)  |  [Örnekler](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/storage/azure-storage-file-datalake)  |  [API başvurusu](/java/api/overview/azure/storage-file-datalake-readme)  |  [Gen1 to Gen2 Mapping](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/storage/azure-storage-file-datalake/GEN1_GEN2_MAPPING.md)  |  [Geri bildirimde](https://github.com/Azure/azure-sdk-for-java/issues) bulunun

## <a name="prerequisites"></a>Önkoşullar

> [!div class="checklist"]
> * Azure aboneliği. Bkz. [Azure ücretsiz deneme sürümü edinme](https://azure.microsoft.com/pricing/free-trial/).
> * Hiyerarşik ad alanı (HNS) etkin olan bir depolama hesabı. Bir tane oluşturmak için [Bu](../common/storage-account-create.md) yönergeleri izleyin.

## <a name="set-up-your-project"></a>Projenizi ayarlama

Başlamak için [Bu sayfayı](https://search.maven.org/artifact/com.azure/azure-storage-file-datalake) açın ve Java kitaplığı 'nın en son sürümünü bulun. Sonra, *pom.xml* dosyasını metin düzenleyicinizde açın. Bu sürüme başvuran bir bağımlılık öğesi ekleyin.

İstemci uygulamanızın kimlik doğrulamasını Azure Active Directory (AD) kullanarak ve sonra Azure gizli Istemci kitaplığına bir bağımlılık ekleyin. Bkz.  [projenize gizli Istemci kitaplığı paketi ekleme](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/identity/azure-identity#adding-the-package-to-your-project).

Ardından, bu Imports deyimlerini kod dosyanıza ekleyin.

```java
import com.azure.storage.common.StorageSharedKeyCredential;
import com.azure.storage.file.datalake.DataLakeDirectoryClient;
import com.azure.storage.file.datalake.DataLakeFileClient;
import com.azure.storage.file.datalake.DataLakeFileSystemClient;
import com.azure.storage.file.datalake.DataLakeServiceClient;
import com.azure.storage.file.datalake.DataLakeServiceClientBuilder;
import com.azure.storage.file.datalake.models.ListPathsOptions;
import com.azure.storage.file.datalake.models.PathItem;
import com.azure.storage.file.datalake.models.AccessControlChangeCounters;
import com.azure.storage.file.datalake.models.AccessControlChangeResult;
import com.azure.storage.file.datalake.models.AccessControlType;
import com.azure.storage.file.datalake.models.PathAccessControl;
import com.azure.storage.file.datalake.models.PathAccessControlEntry;
import com.azure.storage.file.datalake.models.PathPermissions;
import com.azure.storage.file.datalake.models.PathRemoveAccessControlEntry;
import com.azure.storage.file.datalake.models.RolePermissions;
import com.azure.storage.file.datalake.options.PathSetAccessControlRecursiveOptions;
```

## <a name="connect-to-the-account"></a>Hesaba Bağlan 

Bu makaledeki kod parçacıklarını kullanmak için depolama hesabını temsil eden bir **DataLakeServiceClient** örneği oluşturmanız gerekir. 

### <a name="connect-by-using-an-account-key"></a>Hesap anahtarı kullanarak bağlan

Bu, bir hesaba bağlanmanın en kolay yoludur. 

Bu örnek, bir hesap anahtarı kullanarak bir **DataLakeServiceClient** örneği oluşturur.

:::code language="java" source="~/azure-storage-snippets/blobs/howto/Java/Java-v12/src/main/java/com/datalake/manage/Authorize_DataLake.java" id="Snippet_AuthorizeWithKey":::

### <a name="connect-by-using-azure-active-directory-azure-ad"></a>Azure Active Directory kullanarak bağlanma (Azure AD)

Uygulamanızı Azure AD ile doğrulamak için [Azure kimlik istemci kitaplığı](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/identity/azure-identity) 'nı kullanabilirsiniz.

Bu örnek, bir istemci KIMLIĞI, bir istemci parolası ve bir kiracı KIMLIĞI kullanarak bir **DataLakeServiceClient** örneği oluşturur.  Bu değerleri almak için bkz. [bir istemci uygulamasından istekleri yetkilendirmek Için Azure AD 'den belirteç alma](../common/storage-auth-aad-app.md).

:::code language="java" source="~/azure-storage-snippets/blobs/howto/Java/Java-v12/src/main/java/com/datalake/manage/Authorize_DataLake.java" id="Snippet_AuthorizeWithAzureAD":::

> [!NOTE]
> Daha fazla örnek için bkz. [Java Için Azure kimlik istemci kitaplığı](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/identity/azure-identity) belgeleri.


## <a name="create-a-container"></a>Kapsayıcı oluşturma

Bir kapsayıcı dosyalarınız için bir dosya sistemi görevi görür. **DataLakeServiceClient. createFileSystem** metodunu çağırarak bir tane oluşturabilirsiniz.

Bu örnek adlı bir kapsayıcı oluşturur `my-file-system` . 

:::code language="java" source="~/azure-storage-snippets/blobs/howto/Java/Java-v12/src/main/java/com/datalake/manage/CRUD_DataLake.java" id="Snippet_CreateFileSystem":::

## <a name="create-a-directory"></a>Dizin oluşturma

**Datalakefilesystemclient. createDirectory** metodunu çağırarak bir dizin başvurusu oluşturun.

Bu örnek, bir kapsayıcıya adlı bir dizin ekler `my-directory` ve sonra adlı bir alt dizin ekler `my-subdirectory` . 

:::code language="java" source="~/azure-storage-snippets/blobs/howto/Java/Java-v12/src/main/java/com/datalake/manage/CRUD_DataLake.java" id="Snippet_CreateDirectory":::

## <a name="rename-or-move-a-directory"></a>Bir dizini yeniden adlandırma veya taşıma

**Datalakedirectoryclient. Rename** metodunu çağırarak bir dizini yeniden adlandırın veya taşıyın. İstenen dizinin yolunu bir parametre olarak geçirin. 

Bu örnek, bir alt dizini ada yeniden adlandırır `my-subdirectory-renamed` .

:::code language="java" source="~/azure-storage-snippets/blobs/howto/Java/Java-v12/src/main/java/com/datalake/manage/CRUD_DataLake.java" id="Snippet_RenameDirectory":::

Bu örnek adlı dizini adlı bir `my-subdirectory-renamed` dizinin alt dizinine taşır `my-directory-2` . 

:::code language="java" source="~/azure-storage-snippets/blobs/howto/Java/Java-v12/src/main/java/com/datalake/manage/CRUD_DataLake.java" id="Snippet_MoveDirectory":::

## <a name="delete-a-directory"></a>Bir dizini silme

**Datalakedirectoryclient. deleteWithResponse** yöntemini çağırarak bir dizini silin.

Bu örnek adlı bir dizini siler `my-directory` .   

:::code language="java" source="~/azure-storage-snippets/blobs/howto/Java/Java-v12/src/main/java/com/datalake/manage/CRUD_DataLake.java" id="Snippet_DeleteDirectory":::

## <a name="upload-a-file-to-a-directory"></a>Dizine dosya yükleme

İlk olarak, **Datalakefileclient** sınıfının bir örneğini oluşturarak hedef dizinde bir dosya başvurusu oluşturun. **Datalakefileclient. Append** metodunu çağırarak bir dosyayı karşıya yükleyin. **Datalakefileclient. FlushAsync** yöntemini çağırarak karşıya yüklemeyi tamamladığınızdan emin olun.

Bu örnek, adlı bir dizine bir metin dosyası yükler `my-directory` .

:::code language="java" source="~/azure-storage-snippets/blobs/howto/Java/Java-v12/src/main/java/com/datalake/manage/CRUD_DataLake.java" id="Snippet_UploadFile":::

> [!TIP]
> Dosya boyutunuz büyükse, kodunuzun **Datalakefileclient. Append** yöntemine birden çok çağrı yapması gerekir. Bunun yerine **Datalakefileclient. uploadFromFile** metodunu kullanmayı düşünün. Bu şekilde, tüm dosyayı tek bir çağrıda karşıya yükleyebilirsiniz. 
>
> Bir örnek için sonraki bölüme bakın.

## <a name="upload-a-large-file-to-a-directory"></a>Bir dizine büyük bir dosya yükleme

**Datalakefileclient. Append** yöntemine birden çok çağrı yapmak zorunda kalmadan büyük dosyaları karşıya yüklemek Için **Datalakefileclient. uploadfromfile** yöntemini kullanın.

:::code language="java" source="~/azure-storage-snippets/blobs/howto/Java/Java-v12/src/main/java/com/datalake/manage/CRUD_DataLake.java" id="Snippet_UploadFileBulk":::

## <a name="download-from-a-directory"></a>Bir dizinden indir

İlk olarak, indirmek istediğiniz dosyayı temsil eden bir **Datalakefileclient** örneği oluşturun. Dosyayı okumak için **Datalakefileclient. Read** metodunu kullanın. Akıştan bir dosyaya bayt kaydetmek için herhangi bir .NET dosya işleme API 'sini kullanın. 

:::code language="java" source="~/azure-storage-snippets/blobs/howto/Java/Java-v12/src/main/java/com/datalake/manage/CRUD_DataLake.java" id="Snippet_DownloadFile":::

## <a name="list-directory-contents"></a>Dizin içeriğini listeleme

Bu örnek, adlı bir dizinde bulunan her bir dosyanın adını yazdırır `my-directory` .

:::code language="java" source="~/azure-storage-snippets/blobs/howto/Java/Java-v12/src/main/java/com/datalake/manage/CRUD_DataLake.java" id="Snippet_ListFilesInDirectory":::

## <a name="manage-access-control-lists-acls"></a>Erişim denetim listelerini (ACL 'Ler) yönetme

Dizinler ve dosyalar için erişim izinlerini alabilir, ayarlayabilir ve güncelleştirebilirsiniz.

> [!NOTE]
> Erişimi yetkilendirmek için Azure Active Directory (Azure AD) kullanıyorsanız, güvenlik sorumlusuna [Depolama Blobu veri sahibi rolü](../../role-based-access-control/built-in-roles.md#storage-blob-data-owner)atandığından emin olun. ACL izinlerinin nasıl uygulandığı ve bunların nasıl değiştirileceği hakkında daha fazla bilgi edinmek için  [Azure Data Lake Storage 2. erişim denetimi](./data-lake-storage-access-control.md)' ne bakın.

### <a name="manage-a-directory-acl"></a>Dizin ACL 'sini yönetme

Bu örnek, adlı bir dizinin ACL 'sini alır ve ayarlar `my-directory` . Bu örnek, sahip olan kullanıcıya okuma, yazma ve yürütme izinlerini verir, sahip olan gruba yalnızca okuma ve yürütme izinleri verir ve diğerlerinin tüm okuma erişimini sağlar.

> [!NOTE]
> Uygulamanız Azure Active Directory (Azure AD) kullanarak erişim yetkisi alıyorsa, uygulamanızın erişim yetkisi vermek için kullandığı güvenlik sorumlusuna [Depolama Blobu veri sahibi rolü](../../role-based-access-control/built-in-roles.md#storage-blob-data-owner)atandığından emin olun. ACL izinlerinin nasıl uygulandığı ve bunların nasıl değiştirileceği hakkında daha fazla bilgi edinmek için  [Azure Data Lake Storage 2. erişim denetimi](./data-lake-storage-access-control.md)' ne bakın.

:::code language="java" source="~/azure-storage-snippets/blobs/howto/Java/Java-v12/src/main/java/com/datalake/manage/ACL_DataLake.java" id="Snippet_ManageDirectoryACLs":::

Ayrıca, bir kapsayıcının kök dizininin ACL 'sini de alabilir ve ayarlayabilirsiniz. Kök dizini almak için `""` **Datalakefilesystemclient. getdirectoryclient** metoduna boş bir dize () geçirin.

### <a name="manage-a-file-acl"></a>Dosya ACL 'sini yönetme

Bu örnek, adlı bir dosyanın ACL 'sini alır ve ayarlar `upload-file.txt` . Bu örnek, sahip olan kullanıcıya okuma, yazma ve yürütme izinlerini verir, sahip olan gruba yalnızca okuma ve yürütme izinleri verir ve diğerlerinin tüm okuma erişimini sağlar.

> [!NOTE]
> Uygulamanız Azure Active Directory (Azure AD) kullanarak erişim yetkisi alıyorsa, uygulamanızın erişim yetkisi vermek için kullandığı güvenlik sorumlusuna [Depolama Blobu veri sahibi rolü](../../role-based-access-control/built-in-roles.md#storage-blob-data-owner)atandığından emin olun. ACL izinlerinin nasıl uygulandığı ve bunların nasıl değiştirileceği hakkında daha fazla bilgi edinmek için  [Azure Data Lake Storage 2. erişim denetimi](./data-lake-storage-access-control.md)' ne bakın.

:::code language="java" source="~/azure-storage-snippets/blobs/howto/Java/Java-v12/src/main/java/com/datalake/manage/ACL_DataLake.java" id="Snippet_ManageFileACLs":::

### <a name="set-an-acl-recursively"></a>ACL 'yi yinelemeli olarak ayarlama

Bu değişiklikleri her bir alt öğe için ayrı ayrı yapmak zorunda kalmadan, bir üst dizinin varolan alt öğelerinde ACL 'Leri yinelemeli olarak ekleyebilir, güncelleştirebilir ve kaldırabilirsiniz. Daha fazla bilgi için bkz. [Azure Data Lake Storage 2. için erişim denetim listelerini (ACL 'ler) yinelemeli olarak ayarlama](recursive-access-control-lists.md).

## <a name="see-also"></a>Ayrıca bkz.

* [API başvuru belgeleri](/java/api/overview/azure/storage-file-datalake-readme)
* [Paket (Maven)](https://search.maven.org/artifact/com.azure/azure-storage-file-datalake)
* [Örnekler](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/storage/azure-storage-file-datalake)
* [Gen1 to Gen2 Mapping](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/storage/azure-storage-file-datalake/GEN1_GEN2_MAPPING.md)
* [Bilinen sorunlar](data-lake-storage-known-issues.md#api-scope-data-lake-client-library)
* [Geri bildirimde bulunun](https://github.com/Azure/azure-sdk-for-java/issues)