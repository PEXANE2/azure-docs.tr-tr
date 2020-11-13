---
title: Sanal ağda Azure Machine Learning Studio 'yu etkinleştirme
titleSuffix: Azure Machine Learning
description: Bir sanal ağ içinde depolanan verilere erişmek için Azure Machine Learning Studio 'yu nasıl yapılandıracağınızı öğrenin.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: how-to
ms.reviewer: larryfr
ms.author: aashishb
author: aashishb
ms.date: 10/21/2020
ms.custom: contperfq4, tracking-python
ms.openlocfilehash: 7cdd70a44a090b03a3f11626805565469c7c04e7
ms.sourcegitcommit: dc342bef86e822358efe2d363958f6075bcfc22a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/12/2020
ms.locfileid: "94554674"
---
# <a name="use-azure-machine-learning-studio-in-an-azure-virtual-network"></a>Azure sanal ağında Azure Machine Learning Studio 'yu kullanma

Bu makalede, bir sanal ağda Azure Machine Learning Studio 'yu kullanmayı öğreneceksiniz. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> - Bir sanal ağın içindeki bir kaynaktan Studio 'ya erişin.
> - Depolama hesapları için özel uç noktaları yapılandırın.
> - Bir sanal ağ içinde depolanan verilere Studio erişimi verin.
> - Studio 'nun depolama güvenliğini nasıl etkilediğini anlayın.

Bu makale, bir Azure Machine Learning iş akışını güvenli hale getirmek için size kılavuzluk eden beş bölümlü bir serinin beş bölümüdür. İlk olarak birinci mimariyi anlamak için [tek bir bölüm (VNet genel bakış](how-to-network-security-overview.md) ) okumanız önemle tavsiye ederiz. 

Bu serideki diğer makalelere göz atın:

[1. VNET 'e genel bakış](how-to-network-security-overview.md)  >  [2. Çalışma alanı](how-to-secure-workspace-vnet.md)3 ' ü güvenli hale getirin  >  [. Eğitim ortamının](how-to-secure-training-vnet.md)4 ' ü koruyun  >  [. Invenli ortam](how-to-secure-inferencing-vnet.md)5 ' i güvenli hale getirin  >  **. Studio işlevselliğini etkinleştir**


> [!IMPORTANT]
> Çalışma alanınız, Azure Kamu veya Azure Çin 21Vianet gibi bir bağımsız __bulutta__ ise, tümleşik Not defterleri sanal bir ağdaki depolamanın _kullanımını desteklemez._ Bunun yerine işlem örneğinde Jupyter Notebook'larını kullanabilirsiniz. Daha fazla bilgi için, [Işlem örneği Not Defteri Içindeki erişim verileri](how-to-secure-training-vnet.md#access-data-in-a-compute-instance-notebook) bölümüne bakın.


## <a name="prerequisites"></a>Ön koşullar

+ Genel sanal ağ senaryolarını ve genel sanal ağ mimarisini anlamak için [ağ güvenliğine genel bakış](how-to-network-security-overview.md) makalesini okuyun.

+ Kullanılacak önceden var olan bir sanal ağ ve alt ağ.

+ [Özel bağlantı etkin olan bir Azure Machine Learning çalışma alanı](how-to-secure-workspace-vnet.md#secure-the-workspace-with-private-endpoint).

+ Mevcut bir [Azure depolama hesabı, Sanal ağınızı ekledi](how-to-secure-workspace-vnet.md#secure-azure-storage-accounts-with-service-endpoints).

## <a name="access-the-studio-from-a-resource-inside-the-vnet"></a>VNet 'in içindeki bir kaynaktaki Studio 'ya erişme

Studio 'ya bir sanal ağın içindeki bir kaynaktan erişiyorsanız (örneğin, bir işlem örneği veya sanal makine), sanal ağdan Studio 'ya giden trafiğe izin vermeniz gerekir. 

Örneğin, giden trafiği kısıtlamak için ağ güvenlik grupları (NSG) kullanıyorsanız, __Azurefrontkapısı. ön uç__ 'nin __hizmet etiketi__ hedefine bir kural ekleyin.

## <a name="access-data-using-the-studio"></a>Studio 'yu kullanarak verilere erişme

Bir [hizmet uç noktası](how-to-secure-workspace-vnet.md#secure-azure-storage-accounts-with-service-endpoints) veya [Özel uç nokta](how-to-secure-workspace-vnet.md#secure-azure-storage-accounts-with-private-endpoints)Ile sanal ağınıza bir Azure depolama hesabı ekledikten sonra, depolama hesabınızı, Studio 'ya verilerinize erişim vermek için [yönetilen kimlik](../active-directory/managed-identities-azure-resources/overview.md) kullanacak şekilde yapılandırmanız gerekir.

Yönetilen kimlik ' i etkinleştirmezseniz bu hatayı alırsınız, `Error: Unable to profile this dataset. This might be because your data is stored behind a virtual network or your data does not support profile.` Ayrıca aşağıdaki işlemler devre dışı bırakılır:

* Studio 'daki verileri önizleyin.
* Tasarımcıda verileri görselleştirin.
* Bir oto ml denemesi gönder.
* Etiketleme projesi başlatın.

Studio, bir sanal ağdaki aşağıdaki veri deposu türlerinden veri okumayı destekler:

* Azure Blob
* Azure Data Lake Storage Gen1
* Azure Data Lake Storage Gen2
* Azure SQL Veritabanı

### <a name="grant-workspace-managed-identity-__reader__-access-to-storage-private-link"></a>Depolama özel bağlantısına çalışma alanı yönetilen kimlik __okuyucusu__ erişimi verme

Bu adım yalnızca, Azure Storage hesabını [özel bir uç nokta](how-to-secure-workspace-vnet.md#secure-azure-storage-accounts-with-private-endpoints)ile sanal ağınıza eklediyseniz gereklidir. Daha fazla bilgi için bkz. [Reader](../role-based-access-control/built-in-roles.md#reader) yerleşik rolü.

### <a name="configure-datastores-to-use-workspace-managed-identity"></a>Veri depolarını çalışma alanı yönetilen kimliğini kullanacak şekilde yapılandırma

Azure Machine Learning, depolama hesaplarına bağlanmak için [veri depolarını](concept-data.md#datastores) kullanır. Veri mağazalarınızı yönetilen kimlik kullanacak şekilde yapılandırmak için aşağıdaki adımları kullanın. 

1. Studio 'da __veri depoları__ ' nı seçin.

1. Yeni bir veri deposu oluşturmak için __+ Yeni veri deposu__ ' nu seçin.

    Mevcut bir veri deposunu güncelleştirmek için veri deposunu seçin ve __kimlik bilgilerini güncelleştir__ ' i seçin.

1. Veri deposu ayarları ' nda, __çalışma alanı tarafından yönetilen kimliği kullanarak Azure Machine Learning hizmetin depolamaya erişmesine Izin ver__ için __Evet__ ' i seçin.


Bu adımlar, Azure Kaynak tabanlı erişim denetimi (Azure RBAC) kullanarak, çalışma alanı tarafından yönetilen kimliği depolama hizmetine __okuyucu__ olarak ekler. __Okuyucu__ erişimi, çalışma alanının güvenlik duvarı ayarlarını almasına ve verilerin sanal ağdan çıkmadığınızdan emin olmanızı sağlar.

> [!NOTE]
> Bu değişikliklerin etkili olması 10 dakika kadar sürebilir.

## <a name="technical-notes-for-managed-identity"></a>Yönetilen kimlik için teknik notlar

Depolama hizmetlerine erişmek için yönetilen kimlik kullanılması bazı güvenlik konularını etkiler. Bu bölümde, her depolama hesabı türü için değişiklikler açıklanmaktadır.

> [!IMPORTANT]
> Bu konular, eriştiğiniz __depolama hesabı türüne__ özeldir.

### <a name="azure-blob-storage"></a>Azure Blob depolama

__Azure Blob depolama__ için çalışma alanı tarafından yönetilen kimlik, blob depolamadan veri okuyabilmesi Için bir [BLOB veri okuyucusu](../role-based-access-control/built-in-roles.md#storage-blob-data-reader) olarak da eklenir.

### <a name="azure-data-lake-storage-gen2-access-control"></a>Azure Data Lake Storage 2. Access Control

Bir sanal ağ içindeki veri erişimini denetlemek için hem Azure RBAC hem de POSIX stili erişim denetim listelerini (ACL 'Ler) kullanabilirsiniz.

Azure RBAC kullanmak için, [BLOB veri okuyucusu](../role-based-access-control/built-in-roles.md#storage-blob-data-reader) rolüne çalışma alanı tarafından yönetilen kimliği ekleyin. Daha fazla bilgi için bkz. [Azure rol tabanlı erişim denetimi](../storage/blobs/data-lake-storage-access-control-model.md#role-based-access-control).

ACL 'Leri kullanmak için, çalışma alanı tarafından yönetilen kimliğe, tıpkı diğer güvenlik ilkelerine benzer şekilde erişim atanabilir. Daha fazla bilgi için bkz. [dosya ve dizinlerdeki erişim denetim listeleri](../storage/blobs/data-lake-storage-access-control.md#access-control-lists-on-files-and-directories).

### <a name="azure-data-lake-storage-gen1-access-control"></a>Azure Data Lake Storage 1. Access Control

Azure Data Lake Storage 1. yalnızca POSIX stili erişim denetim listelerini destekler. Diğer güvenlik ilkelerine benzer şekilde, çalışma alanı tarafından yönetilen kimlik erişimini kaynaklara atayabilirsiniz. Daha fazla bilgi için [Azure Data Lake Storage 1. erişim denetimi](../data-lake-store/data-lake-store-access-control.md)bölümüne bakın.

### <a name="azure-sql-database-contained-user"></a>Azure SQL veritabanı kullanıcı içeriyordu

Yönetilen kimlik kullanarak bir Azure SQL veritabanında depolanan verilere erişmek için, yönetilen kimliğe eşlenen bir SQL kapsanan Kullanıcı oluşturmanız gerekir. Bir dış sağlayıcıdan Kullanıcı oluşturma hakkında daha fazla bilgi için bkz. [Azure AD kimliklerine eşlenmiş içerilen kullanıcılar oluşturma](../azure-sql/database/authentication-aad-configure.md#create-contained-users-mapped-to-azure-ad-identities).

Bir SQL kapsanan Kullanıcı oluşturduktan sonra, bu kullanıcıya izin ver [T-SQL komutunu](/sql/t-sql/statements/grant-object-permissions-transact-sql)kullanarak izin verin.

### <a name="azure-machine-learning-designer-default-datastore"></a>Azure Machine Learning Designer varsayılan veri deposu

Tasarımcı, çıktıyı varsayılan olarak depolamak için çalışma alanınıza bağlı depolama hesabını kullanır. Bununla birlikte, erişimi olan herhangi bir veri deposu için çıktıyı depolamak üzere belirtebilirsiniz. Ortamınız sanal ağlar kullanıyorsa, verilerinizin güvenli ve erişilebilir kalmasını sağlamak için bu denetimleri kullanabilirsiniz.

Bir işlem hattı için yeni bir varsayılan depolama alanı ayarlamak için:

1. İşlem hattı Taslağınızda, işlem hatlarınızın başlığının yakınında bulunan **Ayarlar dişli simgesini** seçin.
1. **Varsayılan veri deposunu Seç** ' i seçin.
1. Yeni bir veri deposu belirtin.

Ayrıca modül temelinde varsayılan veri deposunu geçersiz kılabilirsiniz. Bu, her bir modülün depolama konumu üzerinde denetim sağlar.

1. Çıktısını belirtmek istediğiniz modülü seçin.
1. **Çıkış ayarları** bölümünü genişletin.
1. **Varsayılan çıkış ayarlarını geçersiz kıl ' ı** seçin.
1. **Çıkış ayarlarını ayarla** ' yı seçin.
1. Yeni bir veri deposu belirtin.

## <a name="next-steps"></a>Sonraki adımlar

Bu makale, dört bölümlü bir sanal ağ serisinin isteğe bağlı bir parçasıdır. Bir sanal ağın güvenliğini nasıl sağlayacağınızı öğrenmek için makalelerin geri kalanına bakın:

* [1. kısım: sanal ağa genel bakış](how-to-network-security-overview.md)
* [2. Bölüm: çalışma alanı kaynaklarını güvenli hale getirme](how-to-secure-workspace-vnet.md)
* [3. kısım: eğitim ortamının güvenliğini sağlama](how-to-secure-training-vnet.md)
* [4. Bölüm: ikinci dereceden sınırlama ortamının güvenliğini sağlama](how-to-secure-inferencing-vnet.md)