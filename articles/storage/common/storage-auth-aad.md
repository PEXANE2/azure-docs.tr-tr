---
title: Active Directory'yi kullanarak blob'lara ve kuyruklara erişim yetkisi verme
titleSuffix: Azure Storage
description: Azure Active Directory'yi kullanarak Azure bloblarına ve kuyruklarına erişimi yetkilendirmeyin.
services: storage
author: tamram
ms.service: storage
ms.topic: conceptual
ms.date: 2/23/2020
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: b8a42723a9b56665160e660c0ea1451253c3d185
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79255372"
---
# <a name="authorize-access-to-blobs-and-queues-using-azure-active-directory"></a>Azure Etkin Dizini'ni kullanarak blob'lara ve kuyruklara erişimi yetkilendirme

Azure Depolama, Blob ve Queue depolamasına istekleri yetkilendirmek için Azure Active Directory (Azure AD) kullanarak destekler. Azure AD ile, kullanıcı, grup veya uygulama hizmeti ilkesi olabilecek bir güvenlik ilkesine izin vermek için rol tabanlı erişim denetimini (RBAC) kullanabilirsiniz. Güvenlik ilkesi, OAuth 2.0 belirteci döndürmek için Azure AD tarafından doğrulanır. Belirteç daha sonra Blob veya Queue depolama karşı bir istek yetkilendirmek için kullanılabilir.

Azure AD ile Azure Depolama'ya karşı istekleri yetkilendirmek, Paylaşılan Anahtar yetkilendirmesi üzerinden üstün güvenlik ve kullanım kolaylığı sağlar. Microsoft, Paylaşılan Anahtar'ın doğasında bulunan olası güvenlik açıklarını en aza indirmek için mümkün olduğunda blob ve kuyruk uygulamalarınızla Azure AD yetkilendirmesini kullanmanızı önerir.

Azure AD ile yetkilendirme, tüm genel amaçlı ve blob depolama hesapları için tüm genel amaçlı bölgelerde ve ulusal bulutlarda kullanılabilir. Yalnızca Azure Kaynak Yöneticisi dağıtım modeliyle oluşturulan depolama hesapları Azure AD yetkilendirmesini destekler.

Blob depolama ayrıca Azure AD kimlik bilgileriyle imzalanmış paylaşılan erişim imzaları (SAS) oluşturmayı da destekler. Daha fazla bilgi için bkz: [Paylaşılan erişim imzaları ile verilere sınırlı erişim verme.](storage-sas-overview.md)

Azure Dosyaları, yalnızca etki alanına katılan VM'ler için SMB üzerinden AD (önizleme) veya Azure AD DS (GA) ile yetkilendirmeyi destekler. Azure Dosyaları için SMB üzerinden AD (önizleme) veya Azure AD DS (GA) kullanma hakkında bilgi edinmek [için, Kobİ erişimi için Azure Dosyalarına Genel Bakış kimlik tabanlı kimlik doğrulama desteğine](../files/storage-files-active-directory-overview.md)bakın.

Azure AD ile yetkilendirme, Azure Tablo depolama alanı için desteklenmez. İstekleri Tablo depolama alanına yetkilendirmek için Paylaşılan Anahtar'ı kullanın.

## <a name="overview-of-azure-ad-for-blobs-and-queues"></a>Blobs ve kuyruklar için Azure AD'ye genel bakış

Bir güvenlik sorumlusu (kullanıcı, grup veya uygulama) bir blob veya sıra kaynağına erişmeye çalıştığında, anonim erişim için kullanılabilir bir blob olmadığı sürece istek yetkilendirilmelidir. Azure AD ile kaynağa erişim iki adımlı bir işlemdir. İlk olarak, güvenlik sorumlusunun kimliği doğrulanır ve bir OAuth 2.0 belirteci döndürülür. Ardından, belirteç Blob veya Sıra hizmetine bir istek parçası olarak geçirilir ve belirtilen kaynağa erişim yetkisi vermek için hizmet tarafından kullanılır.

Kimlik doğrulama adımı, bir uygulamanın çalışma zamanında bir OAuth 2.0 erişim belirteci istemesini gerektirir. Bir uygulama Azure VM, sanal makine ölçeği kümesi veya Azure İşlevler uygulaması gibi bir Azure varlığı içinden çalışıyorsa, blob'lara veya kuyruklara erişmek için yönetilen bir [kimliği](../../active-directory/managed-identities-azure-resources/overview.md) kullanabilir. Yönetilen bir kimlik tarafından yapılan istekleri Azure Blob veya Queue hizmetine nasıl yetkiverdiğinizi öğrenmek için Azure [Etkin Dizini ve Azure Kaynakları için yönetilen kimliklerle blob'lara ve kuyruklara erişimi yetkilendirme'ye](storage-auth-aad-msi.md)bakın.

Yetkilendirme adımı, bir veya daha fazla RBAC rolünün güvenlik ilkesine atanmasını gerektirir. Azure Depolama, blob ve sıra verileri için ortak izin kümelerini kapsayan RBAC rolleri sağlar. Bir güvenlik ilkesine atanan roller, asıl ilkelerin sahip olacağı izinleri belirler. Azure Depolama için RBAC rolleri atama hakkında daha fazla bilgi edinmek için [bkz.](storage-auth-aad-rbac.md)

Azure Blob veya Queue hizmetine istekte bulunarak yerel uygulamalar ve web uygulamaları da Azure AD ile erişim yetkisi verebilir. Bir erişim jetonunu nasıl isteyeceğinizi ve blob veya kuyruk verileri için istekleri yetkilendirmek için nasıl kullanacağınızı öğrenmek için, [bir Azure Depolama uygulamasından Azure AD ile Azure Depolama'ya erişimi yetkilendirme](storage-auth-aad-app.md)'ye bakın.

## <a name="assign-rbac-roles-for-access-rights"></a>Erişim hakları için RBAC rollerini atama

Azure Etkin Dizin (Azure AD), rol tabanlı [erişim denetimi (RBAC)](../../role-based-access-control/overview.md)aracılığıyla güvenli kaynaklara erişim haklarına izin vermektedir. Azure Depolama, blob ve sıra verilerine erişmek için kullanılan ortak izin kümelerini kapsayan yerleşik RBAC rolleri kümesini tanımlar. Blob ve sıra verilerine erişmek için özel roller de tanımlayabilirsiniz.

Bir Azure REKLAM güvenlik ilkesine bir RBAC rolü atandığında, Azure bu güvenlik ilkesi için bu kaynaklara erişim sağlar. Erişim, abonelik düzeyine, kaynak grubuna, depolama hesabına veya tek bir kapsayıcı veya sıraya kadar kapsama edilebilir. Azure AD güvenlik ilkesi, [Azure kaynakları için](../../active-directory/managed-identities-azure-resources/overview.md)bir kullanıcı, bir grup, bir uygulama hizmeti ilkesi veya yönetilen bir kimlik olabilir.

### <a name="built-in-rbac-roles-for-blobs-and-queues"></a>Blobs ve kuyruklar için yerleşik RBAC rolleri

[!INCLUDE [storage-auth-rbac-roles-include](../../../includes/storage-auth-rbac-roles-include.md)]

Yerleşik bir RBAC rolünü bir güvenlik ilkesine nasıl atayacağımöğrenmek için aşağıdaki makalelerden birine bakın:

- [Azure portalında RBAC ile Azure blob ve kuyruk verilerine erişim izni verme](storage-auth-aad-rbac-portal.md)
- [Azure CLI kullanarak RBAC ile Azure blob ve kuyruk verilerine erişim izni verme](storage-auth-aad-rbac-cli.md)
- [PowerShell kullanarak RBAC ile Azure blob ve kuyruk verilerine erişim izni verme](storage-auth-aad-rbac-powershell.md)

Azure Depolama için yerleşik rollerin nasıl tanımlandığı hakkında daha fazla bilgi için [bkz.](../../role-based-access-control/role-definitions.md#management-and-data-operations) Özel RBAC rolleri oluşturma hakkında bilgi [için](../../role-based-access-control/custom-roles.md)bkz.

### <a name="access-permissions-for-data-operations"></a>Veri işlemleri için erişim izinlerine erişim

Belirli Blob veya Queue hizmet işlemlerini aramak için gereken izinlerle ilgili ayrıntılar [için, blob ve sıra veri işlemlerini çağırmak için İzinler'e](https://docs.microsoft.com/rest/api/storageservices/authorize-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations)bakın.

## <a name="resource-scope"></a>Kaynak kapsamı

[!INCLUDE [storage-auth-resource-scope-include](../../../includes/storage-auth-resource-scope-include.md)]

## <a name="access-data-with-an-azure-ad-account"></a>Azure AD hesabıyla verilere erişin

Azure portalı, PowerShell veya Azure CLI üzerinden blob veya kuyruk verilerine erişim, kullanıcının Azure AD hesabını kullanarak veya hesap erişim anahtarlarını (Paylaşılan Anahtar yetkilendirmesi) kullanarak yetkilendirilebilir.

### <a name="data-access-from-the-azure-portal"></a>Azure portalından veri erişimi

Azure portalı, azure depolama hesabındaki blob ve sıra verilerine erişmek için Azure AD hesabınızı veya hesap erişim anahtarlarını kullanabilir. Azure portalının hangi yetkilendirme düzenini kullandığı, size atanan RBAC rollerine bağlıdır.

Blob veya sıra verilerine erişmeye çalıştığınızda, Azure portalı önce **Microsoft.Storage/storageAccounts/listkeys/action**ile bir RBAC rolü atanıp atanmadığınızı denetler. Bu eylemle bir rol atanmışsa, Azure portalı Paylaşılan Anahtar yetkilendirmesi aracılığıyla blob ve kuyruk verilerine erişmek için hesap anahtarını kullanır. Bu eylemle size bir rol atanmamışsa, Azure portalı Azure REKLAM hesabınızı kullanarak verilere erişmeye çalışır.

Azure AD hesabınızı kullanarak Azure portalındaki blob veya sıra verilerine erişmek için, blob ve sıra verilerine erişmek için izinlere ve Azure portalındaki depolama hesabı kaynaklarında gezinmek için izinlere de ihtiyacınız vardır. Azure Depolama tarafından sağlanan yerleşik roller, blob ve sıra kaynaklarına erişim sağlar, ancak depolama hesabı kaynaklarına izin vermez. Bu nedenle, portala erişim, depolama hesabı veya daha yüksek düzeye yönelik [Okuyucu](../../role-based-access-control/built-in-roles.md#reader) rolü gibi bir Azure Kaynak Yöneticisi rolünün atanmasını da gerektirir. **Reader** rolü en kısıtlı izinleri verir, ancak depolama hesabı yönetim kaynaklarına erişim sağlayan başka bir Azure Kaynak Yöneticisi rolü de kabul edilebilir. Azure AD hesabıyla Azure portalında veri erişimi için kullanıcılara izin atama hakkında daha fazla bilgi edinmek için Azure [portalında RBAC ile Azure blob ve kuyruk verilerine erişim izni 'ne](storage-auth-aad-rbac-portal.md)bakın.

Azure portalı, bir kapsayıcıya veya kuyruğa gidince hangi yetkilendirme düzeninin kullanıldığını gösterir. Portaldaki veri erişimi hakkında daha fazla bilgi için [bkz.](storage-access-blobs-queues-portal.md)

### <a name="data-access-from-powershell-or-azure-cli"></a>PowerShell veya Azure CLI'den veri erişimi

Azure CLI ve PowerShell, Azure AD kimlik bilgileriyle oturum açmayı destekler. Oturum açtıktan sonra oturumunuz bu kimlik bilgileri altında çalışır. Daha fazla bilgi edinmek [için, blob veya kuyruk verilerine erişmek için Azure AD kimlik bilgilerine sahip Azure CLI veya PowerShell komutlarını çalıştır'a](authorize-active-directory-powershell.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Etkin Dizini ve Yönetilen kimliklerle Azure Kaynakları için blob'lara ve kuyruklara erişimi yetkilendirme](storage-auth-aad-msi.md)
- [Blob'lara ve kuyruklara erişmek için bir uygulamadan Azure Active Directory ile yetkilendirme](storage-auth-aad-app.md)
- [Azure Etkin Dizin tabanlı erişim denetimi için Azure Depolama desteği genellikle kullanılabilir](https://azure.microsoft.com/blog/azure-storage-support-for-azure-ad-based-access-control-now-generally-available/)
