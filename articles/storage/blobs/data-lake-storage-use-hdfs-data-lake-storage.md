---
title: Azure Veri Gölü Depolama Gen2 ile HDFS CLI'yi kullanma
description: Veri Gölü Depolama Gen2 için HDFS CLI Giriş
services: storage
author: normesta
ms.service: storage
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: normesta
ms.subservice: data-lake-storage-gen2
ms.reviewer: artek
ms.openlocfilehash: 1d5313f3f0fff128dd09f9c9857b7dd9921ea4f8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "69992212"
---
# <a name="using-the-hdfs-cli-with-data-lake-storage-gen2"></a>Veri Gölü Depolama Gen2 ile HDFS CLI kullanma

[Hadoop Distributed File System (HDFS)](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html)ile olduğu gibi bir komut satırı arabirimi kullanarak depolama hesabınızdaki verilere erişebilir ve yönetebilirsiniz. Bu makalede, başlamak yardımcı olacak bazı örnekler sağlar.

HDInsight, bilgi işlem düğümlerine yerel olarak bağlı olan dağıtılmış kapsayıcıya erişim sağlar. Bu kapsayıcıya HDFS ve Hadoop'un desteklediği diğer dosya sistemleriyle doğrudan etkileşimde olan kabuğu nu kullanarak erişebilirsiniz.

HDFS CLI hakkında daha fazla bilgi için [resmi belgelere](https://hadoop.apache.org/docs/r2.4.1/hadoop-project-dist/hadoop-common/FileSystemShell.html) ve [HDFS İzinleri Kılavuzu'na](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html) bakın

>[!NOTE]
>HDInsight yerine Azure Databricks kullanıyorsanız ve bir komut satırı arabirimi kullanarak verilerinizle etkileşimkurmak istiyorsanız, Databricks CLI'yi Kullanarak Databricks dosya sistemiyle etkileşimkurabilirsiniz. Bkz. [Databricks CLI](https://docs.azuredatabricks.net/user-guide/dev-tools/databricks-cli.html).

## <a name="use-the-hdfs-cli-with-an-hdinsight-hadoop-cluster-on-linux"></a>HDFS CLI'yi Linux üzerinde HDInsight Hadoop kümesiyle kullanın

İlk olarak, [hizmetlere uzaktan erişim](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-linux-information#remote-access-to-services)kurmak. [SSH](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix) seçerseniz örnek PowerShell kodu aşağıdaki gibi görünür:

```powershell
#Connect to the cluster via SSH.
ssh sshuser@clustername-ssh.azurehdinsight.net
#Execute basic HDFS commands. Display the hierarchy.
hdfs dfs -ls /
#Create a sample directory.
hdfs dfs -mkdir /samplefolder
```
Bağlantı dizesi, Azure portalındaki HDInsight küme bıçağının "SSH + Cluster giriş" bölümünde bulunabilir. Küme oluşturma sırasında SSH kimlik bilgileri belirtilmiştir.

>[!IMPORTANT]
>HDInsight küme faturalandırması bir küme oluşturulduktan sonra başlar ve küme silindiğinde durur. Fatura dakikalara eşit olarak dağıtıldığından, kullanılmayan kümelerinizi mutlaka silmelisiniz. Bir kümeyi nasıl sildiğinizi öğrenmek için [konuyla ilgili makalemize](../../hdinsight/hdinsight-delete-cluster.md)bakın. Ancak, Bir HDInsight kümesi silindikten sonra bile Veri Gölü Depolama Gen2 etkin leştirilmiş bir depolama hesabında depolanan veriler devam eder.

## <a name="create-a-container"></a>Bir kapsayıcı oluşturma

    hdfs dfs -D "fs.azure.createRemoteFileSystemDuringInitialization=true" -ls abfs://<container-name>@<storage-account-name>.dfs.core.windows.net/

* Yer `<container-name>` tutucuyu kabınıza vermek istediğiniz adla değiştirin.

* Yer `<storage-account-name>` tutucuyu depolama hesabınızın adı ile değiştirin.

## <a name="get-a-list-of-files-or-directories"></a>Dosyaların veya dizinlerin listesini alma

    hdfs dfs -ls <path>

Yer `<path>` tutucuyu konteyner veya kapsayıcı klasörünün URI'si ile değiştirin.

Örneğin, `hdfs dfs -ls abfs://my-file-system@mystorageaccount.dfs.core.windows.net/my-directory-name`

## <a name="create-a-directory"></a>Dizin oluşturma

    hdfs dfs -mkdir [-p] <path>

Yer `<path>` tutucuyu kök kapsayıcı adı veya kapsayıcınızdaki bir klasörle değiştirin.

Örneğin, `hdfs dfs -mkdir abfs://my-file-system@mystorageaccount.dfs.core.windows.net/`

## <a name="delete-a-file-or-directory"></a>Dosya veya dizin silme

    hdfs dfs -rm <path>

Yer `<path>` tutucuyu, silmek istediğiniz dosya veya klasörün URI'si ile değiştirin.

Örneğin, `hdfs dfs -rmdir abfs://my-file-system@mystorageaccount.dfs.core.windows.net/my-directory-name/my-file-name`

## <a name="display-the-access-control-lists-acls-of-files-and-directories"></a>Dosyaların ve dizinlerin Erişim Denetim Listelerini (ALISANS'ları) görüntüleme

    hdfs dfs -getfacl [-R] <path>

Örnek:

`hdfs dfs -getfacl -R /dir`

Bkz. [getfacl](https://hadoop.apache.org/docs/r2.4.1/hadoop-project-dist/hadoop-common/FileSystemShell.html#getfacl)

## <a name="set-acls-of-files-and-directories"></a>Dosyaların ve dizinlerin ADA'larını ayarlama

    hdfs dfs -setfacl [-R] [-b|-k -m|-x <acl_spec> <path>]|[--set <acl_spec> <path>]

Örnek:

`hdfs dfs -setfacl -m user:hadoop:rw- /file`

Bkz. [setfacl](https://hadoop.apache.org/docs/r2.4.1/hadoop-project-dist/hadoop-common/FileSystemShell.html#setfacl)

## <a name="change-the-owner-of-files"></a>Dosyaların sahibini değiştirme

    hdfs dfs -chown [-R] <new_owner>:<users_group> <URI>

Bkz. [chown](https://hadoop.apache.org/docs/r2.4.1/hadoop-project-dist/hadoop-common/FileSystemShell.html#chown)

## <a name="change-group-association-of-files"></a>Dosyaların grup ilişkilendirmesini değiştirme

    hdfs dfs -chgrp [-R] <group> <URI>

bkz. [chgrp](https://hadoop.apache.org/docs/r2.4.1/hadoop-project-dist/hadoop-common/FileSystemShell.html#chgrp)

## <a name="change-the-permissions-of-files"></a>Dosyaların izinlerini değiştirme

    hdfs dfs -chmod [-R] <mode> <URI>

Bkz. [chmod](https://hadoop.apache.org/docs/r2.4.1/hadoop-project-dist/hadoop-common/FileSystemShell.html#chmod)

[Apache Hadoop 2.4.1 Dosya Sistemi Kabuk Kılavuzu](https://hadoop.apache.org/docs/r2.4.1/hadoop-project-dist/hadoop-common/FileSystemShell.html) Web Sitesindeki komutların tam listesini görüntüleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Veri Tuğlaları'nda Azure Veri Gölü Depolama Gen2 özellikli bir hesap kullanın](./data-lake-storage-quickstart-create-databricks-account.md)

* [Dosyalar ve dizinler üzerindeki erişim denetim listeleri hakkında bilgi edinin](https://docs.microsoft.com/azure/storage/blobs/data-lake-storage-access-control)
