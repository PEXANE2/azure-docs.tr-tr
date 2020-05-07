---
title: Azure yerleşik rolleri-Azure RBAC
description: Bu makalede, Azure rol tabanlı erişim denetimi (Azure RBAC) için Azure yerleşik rolleri açıklanmaktadır. Eylemler, NotActions, DataActions ve NotDataActions öğelerini listeler.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: role-based-access-control
ms.devlang: ''
ms.topic: reference
ms.tgt_pltfrm: ''
ms.workload: identity
ms.date: 05/04/2020
ms.author: rolyon
ms.reviewer: bagovind
ms.custom: it-pro
ms.openlocfilehash: b6587ca113319c266d2335c9cbb154e0ecc4afa1
ms.sourcegitcommit: 11572a869ef8dbec8e7c721bc7744e2859b79962
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/05/2020
ms.locfileid: "82838748"
---
# <a name="azure-built-in-roles"></a>Azure yerleşik rolleri

[Azure rol tabanlı erişim denetimi (Azure RBAC)](overview.md) , kullanıcılara, gruplara, hizmet sorumlularına ve yönetilen kimliklere atayabileceğiniz birkaç Azure yerleşik rolüne sahiptir. Rol atamaları, Azure kaynaklarına erişimi denetlemenize olanak sağlar. Yerleşik roller kuruluşunuzun belirli ihtiyaçlarını karşılamıyorsa, kendi [Azure özel rollerinizi](custom-roles.md)de oluşturabilirsiniz.

Bu makalede, her zaman gelişen Azure yerleşik rolleri listelenir. En son rolleri almak için [Get-AzRoleDefinition](/powershell/module/az.resources/get-azroledefinition) veya [az role Definition List](/cli/azure/role/definition#az-role-definition-list)komutunu kullanın. Azure Active Directory (Azure AD) için yönetici rolleri arıyorsanız, [Azure Active Directory Içindeki yönetici rolü izinleri](../active-directory/users-groups-roles/directory-assign-admin-roles.md)bölümüne bakın.

## <a name="all"></a>Tümü

Aşağıdaki tabloda, her yerleşik rolün bir kısa açıklaması ve benzersiz KIMLIĞI verilmiştir. Her rolün `Actions`, `NotActions` `DataActions`, ve `NotDataActions` listesini görmek için rol adını seçin. Bu eylemlerin ne anlama geldiğini ve bunların yönetim ve veri düzlemleri için nasıl uygulandığını hakkında bilgi için bkz. [Azure rol tanımlarını anlama](role-definitions.md).


> [!div class="mx-tableFixed"]
> | Yerleşik rol | Açıklama | Kimlik |
> | --- | --- | --- |
> | **Genel** |  |  |
> | [Katkıda Bulunan](#contributor) | Kaynaklara erişim verme dışında her şeyi yönetmenizi sağlar. | b24988ac-6180-42a0-ab88-20f7382dd24c |
> | [Sahibi](#owner) | Kaynaklara erişim de dahil olmak üzere her şeyi yönetmenizi sağlar. | 8e3af657-a8ff-443c-a75c-2fe8c4bcb635 |
> | [Okuyucu](#reader) | Her şeyi görüntülemenize izin verir, ancak hiçbir değişiklik yapmamalıdır. | acdd72a7-3385-48ef-bd42-f606fba81ae7 |
> | [Kullanıcı Erişimi Yöneticisi](#user-access-administrator) | Azure kaynaklarına Kullanıcı erişimini yönetmenizi sağlar. | 18d7d88d-d35e-4fb5-a5c3-7773c20a72d9 |
> | **İşlem** |  |  |
> | [Klasik sanal makine Katılımcısı](#classic-virtual-machine-contributor) | Klasik sanal makineleri yönetmenizi sağlar ancak bunlara yönelik erişimi, bağlı oldukları sanal ağ veya depolama hesabı için değil. | d73bb868-a0df-4d4d-bd69-98a00b01fccb |
> | [Sanal Makine Yöneticisi oturum açma](#virtual-machine-administrator-login) | Portalda sanal makineleri görüntüleme ve yönetici olarak oturum açma | 1c0163c0-47e6-4577-8991-ea5c82e286e4 |
> | [Sanal Makine Katılımcısı](#virtual-machine-contributor) | Sanal makineleri yönetmenize izin verir, ancak bunlara bağlı oldukları sanal ağ veya depolama hesabına erişemez. | 9980e02c-c2be-4d73-94E8-173b1dc7cf3c |
> | [Sanal makine Kullanıcı oturumu açma](#virtual-machine-user-login) | Portalda sanal makineleri görüntüleyin ve normal bir kullanıcı olarak oturum açın. | fb879df8-f326-4884-b1cf-06f3ad86be52 |
> | **Ağ** |  |  |
> | [CDN uç noktası Katılımcısı](#cdn-endpoint-contributor) | CDN uç noktalarını yönetebilir, ancak diğer kullanıcılara erişim izni veremez. | 426e0c7f-0c7e-4658-b36f-ff54d6c29b45 |
> | [CDN uç nokta okuyucusu](#cdn-endpoint-reader) | CDN uç noktalarını görüntüleyebilir, ancak değişiklik yapamaz. | 871e35f6-b5c1-49cc-A043-bde969a0f2cd |
> | [CDN profili Katılımcısı](#cdn-profile-contributor) | CDN profillerini ve uç noktalarını yönetebilir, ancak diğer kullanıcılara erişim izni veremez. | ec156ff8-a8d1-4d15-830c-5b80698ca432 |
> | [CDN profili okuyucu](#cdn-profile-reader) | CDN profillerini ve uç noktalarını görüntüleyebilir, ancak değişiklik yapamaz. | 8f96442b-4075-438f-813D-ad51ab4019af |
> | [Klasik Ağ Katılımcısı](#classic-network-contributor) | Klasik ağları yönetmenize izin verir, ancak bunlara erişemez. | b34d265f-36f7-4a0d-a4d4-e158ca92e90f |
> | [DNS bölgesi Katılımcısı](#dns-zone-contributor) | Azure DNS, DNS bölgelerini ve kayıt kümelerini yönetmenizi sağlar, ancak bunlara kimlerin erişebileceğini denetlemenize izin vermez. | befefa01-2a29-4197-83a8-272ff33ce314 |
> | [Ağ Katılımcısı](#network-contributor) | Ağları yönetmenizi sağlar ancak onlara yönelik erişimleri vermez. | 4d97b98b-1d4f-4787-A291-c67834d212e7 |
> | [Katkıda bulunan Traffic Manager](#traffic-manager-contributor) | Traffic Manager profillerini yönetmenizi sağlar, ancak bunlara kimlerin erişebileceğini denetlemenize izin vermez. | a4b10055-b0c7-44c2-b00f-c7b5b3550cf7 |
> | **Depolama** |  |  |
> | [Avere Katılımcısı](#avere-contributor) | , Bir avere vFXT kümesi oluşturabilir ve yönetebilir. | 4f8fab4f-1852-4a58-a46a-8eaf358af14a |
> | [Avere Işleci](#avere-operator) | Kümeyi yönetmek için avere vFXT kümesi tarafından kullanılır | c025889f-8102-4ebf-b32c-fc0c6f0c6bd9 |
> | [Yedek Katılımcısı](#backup-contributor) | Yedekleme hizmetini yönetmenizi sağlar, ancak kasaların oluşturamaz ve başkalarına erişim izni verebilir | 5e467623-bb1f-42F4-a55d-6e525e11384b |
> | [Yedekleme Işletmeni](#backup-operator) | Yedekleme kaldırma, kasa oluşturma ve başkalarına erişim verme dışında yedekleme hizmetlerini yönetmenizi sağlar | 00c29273-979b-4161-815c-10b084fb9324 |
> | [Yedekleme okuyucusu](#backup-reader) | Yedekleme hizmetlerini görüntüleyebilir, ancak değişiklik yapamaz | a795c7a0-d4a2-40c1-ae25-d81f01202912 |
> | [Klasik depolama hesabı Katılımcısı](#classic-storage-account-contributor) | Klasik depolama hesaplarını yönetmenize izin verir, ancak bunlara erişimi kalmaz. | 86e8f5dc-a6e9-4c67-9d15-de283e8eac25 |
> | [Klasik depolama hesabı anahtar operatörü hizmet rolü](#classic-storage-account-key-operator-service-role) | Klasik depolama hesabı anahtar Işleçleri klasik depolama hesaplarında anahtarları listeleme ve yeniden oluşturma izni verilir | 985d6b00-f706-48f5-a6fe-d0ca12fb668d |
> | [Katkıda bulunan Data Box](#data-box-contributor) | , Diğer kullanıcılara erişim izni hariç Data Box hizmeti altındaki her şeyi yönetmenizi sağlar. | add466c9-e687-43fc-8d98-dfcf8d720be5 |
> | [Data Box okuyucu](#data-box-reader) | Sipariş oluşturma veya düzenleme sırası ayrıntıları ve başkalarına erişim verme dışında Data Box hizmetini yönetmenizi sağlar. | 028f4ed7-e2a9-465E-a8f4-9c0ffdfdc027 |
> | [Data Lake Analytics geliştirici](#data-lake-analytics-developer) | Kendi işlerinizi göndermenize, izlemenize ve yönetmenize izin verir, ancak Data Lake Analytics Hesapları oluşturamaz veya silemezsiniz. | 47b7735b-770e-4598-a7da-8b91488b4c88 |
> | [Okuyucu ve veri erişimi](#reader-and-data-access) | Her şeyi görüntülemenize izin verir, ancak bir depolama hesabını veya kapsanan kaynağı silmenize veya oluşturmanıza izin vermez. Ayrıca depolama hesabı anahtarlarına erişim aracılığıyla bir depolama hesabında bulunan tüm verilere okuma/yazma erişimi de sağlar. | c12c1c16-33a1-487b-954d-41c89c60f349 |
> | [Depolama Hesabı Katılımcısı](#storage-account-contributor) | Depolama hesaplarının yönetimine izin verir. Paylaşılan anahtar yetkilendirmesi aracılığıyla verilere erişmek için kullanılabilen hesap anahtarına erişim sağlar. | 17d1049b-9a84-46fb-8F53-869881c3d3ab |
> | [Depolama hesabı anahtar operatörü hizmet rolü](#storage-account-key-operator-service-role) | Depolama hesabı erişim anahtarlarının listelenmesi ve yeniden oluşturulmasına izin verir. | 81a9662b-bebf-436f-a333-f67b29880f12 |
> | [Depolama Blobu veri Katılımcısı](#storage-blob-data-contributor) | Azure depolama kapsayıcıları ve bloblarını okuyun, yazın ve silin. Belirli bir veri işlemi için hangi eylemlerin gerekli olduğunu öğrenmek için bkz. [BLOB ve kuyruk verisi işlemlerini çağırma izinleri](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations). | ba92f5b4-2d11-453d-a403-e96b0029c9fe |
> | [Depolama Blobu veri sahibi](#storage-blob-data-owner) | , POSIX erişim denetimi atama dahil olmak üzere Azure depolama blob kapsayıcılarına ve verilerine tam erişim sağlar. Belirli bir veri işlemi için hangi eylemlerin gerekli olduğunu öğrenmek için bkz. [BLOB ve kuyruk verisi işlemlerini çağırma izinleri](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations). | b7e6dc6d-f1e8-4753-8033-0f276bb0955b |
> | [Depolama Blobu veri okuyucusu](#storage-blob-data-reader) | Azure depolama kapsayıcıları ve bloblarını okuyun ve listeleyin. Belirli bir veri işlemi için hangi eylemlerin gerekli olduğunu öğrenmek için bkz. [BLOB ve kuyruk verisi işlemlerini çağırma izinleri](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations). | 2a2b9908-6ea1-4ae2-8e65-a410df84e7d1 |
> | [Depolama Blobu Delegator](#storage-blob-delegator) | Azure AD kimlik bilgileriyle imzalanan bir kapsayıcı veya blob için paylaşılan erişim imzası oluşturmak üzere kullanılabilen bir Kullanıcı temsili anahtarı alın. Daha fazla bilgi için bkz. [Kullanıcı TEMSILI SAS oluşturma](https://docs.microsoft.com/rest/api/storageservices/create-user-delegation-sas). | db58b8e5-c6ad-4a2a-8342-4190687cbf4a |
> | [Depolama dosyası veri SMB paylaşımında katkıda bulunan](#storage-file-data-smb-share-contributor) | Azure dosya paylaşımlarında dosya/dizinlerde okuma, yazma ve silme erişimine izin verir. Bu rolün Windows dosya sunucularında yerleşik bir eşdeğeri yoktur. | 0c867c2a-1d8c-454a-a3db-ab2ea1bdc8bb |
> | [Depolama dosyası veri SMB paylaşımında yükseltilmiş katkıda bulunan](#storage-file-data-smb-share-elevated-contributor) | Azure dosya paylaşımlarında dosya/dizinlerde okuma, yazma, silme ve değiştirme ACL 'Lerine izin verir. Bu rol, Windows dosya sunucularındaki değişikliğin bir dosya paylaşımının ACL 'sine eşdeğerdir. | a7264617-510b-434b-a828-9731dc254ea7 |
> | [Depolama dosyası veri SMB paylaşma okuyucusu](#storage-file-data-smb-share-reader) | Azure dosya paylaşımlarında dosya/dizinlerde okuma erişimine izin verir. Bu rol, Windows dosya sunucularında okunan bir dosya paylaşma ACL 'sine eşdeğerdir. | aba4ae5f-2193-4029-9191-0cb91df5e314 |
> | [Depolama kuyruğu verileri Katılımcısı](#storage-queue-data-contributor) | Azure depolama kuyruklarını ve sıra iletilerini okuyun, yazın ve silin. Belirli bir veri işlemi için hangi eylemlerin gerekli olduğunu öğrenmek için bkz. [BLOB ve kuyruk verisi işlemlerini çağırma izinleri](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations). | 974c5e8b-45b9-4653-ba55-5f855dd0fb88 |
> | [Depolama kuyruğu veri Iletisi Işlemcisi](#storage-queue-data-message-processor) | Azure depolama kuyruğundan bir iletiyi göz atın, alın ve silin. Belirli bir veri işlemi için hangi eylemlerin gerekli olduğunu öğrenmek için bkz. [BLOB ve kuyruk verisi işlemlerini çağırma izinleri](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations). | 8a0f0c08-91a1-4084-bc3d-661d67233bes |
> | [Depolama kuyruğu veri Iletisi gönderici](#storage-queue-data-message-sender) | Bir Azure depolama kuyruğuna ileti ekleyin. Belirli bir veri işlemi için hangi eylemlerin gerekli olduğunu öğrenmek için bkz. [BLOB ve kuyruk verisi işlemlerini çağırma izinleri](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations). | c6a89b2d-59bc-44d0-9896-0f6e12d7b80a |
> | [Depolama kuyruğu veri okuyucusu](#storage-queue-data-reader) | Azure depolama kuyruklarını ve sıra iletilerini okuyun ve listeleyin. Belirli bir veri işlemi için hangi eylemlerin gerekli olduğunu öğrenmek için bkz. [BLOB ve kuyruk verisi işlemlerini çağırma izinleri](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations). | 19e7f393-937e-4F77-808e-94535e297925 |
> | **Web** |  |  |
> | [Azure haritalar veri okuyucu](#azure-maps-data-reader) | Azure haritalar hesabından ilgili harita okuma verilerine erişim izni verir. | 423170ca-a8f6-4b0f-8487-9e4eb8f49bfa |
> | [Katkıda bulunan Arama Hizmeti](#search-service-contributor) | Arama hizmetlerini yönetmenize izin verir, ancak bunlara erişim izni vermez. | 7ca78c08-252a-4471-8644-bb5ff32d4ba0 |
> | [Web planı Katılımcısı](#web-plan-contributor) | Web siteleri için Web planlarını yönetmenizi sağlar, ancak bunlara erişemez. | 2cc479cb-7b4d-49a8-b449-8c00fd0f0a4b |
> | [Web sitesi Katılımcısı](#website-contributor) | Web sitelerini yönetmenizi sağlar, ancak bunlara erişemez. | de139f84-1756-47ae-9be6-808fbbe84772 |
> | **Kapsayıcılar** |  |  |
> | [AcrDelete](#acrdelete) | ACR silme | c2f4ef07-c644-48eb-af81-4b1b4947fb11 |
> | [Acrimageimzalayan](#acrimagesigner) | ACR görüntü imzalayan | 6cef56e8-d556-48e5-a04f-b8e64114680f |
> | [AcrPull](#acrpull) | ACR çekme | 7f951ddav-4ed3-4680-a7ca-43fe172d538d |
> | [AcrPush](#acrpush) | ACR gönderimi | 8311e382-0749-4cb8-b61a-304f252e45ec |
> | [Acrquaranınereader](#acrquarantinereader) | ACR karantina veri okuyucusu | cdda3590-29a3-44f6-95f2-9f980659eb04 |
> | [AcrQuarantineWriter](#acrquarantinewriter) | ACR karantina veri yazıcısı | c8d4ff99-41c3-41a8-9f60-21dfdad59608 |
> | [Azure Kubernetes hizmet kümesi yönetici rolü](#azure-kubernetes-service-cluster-admin-role) | Küme Yöneticisi kimlik bilgisi eylemini listeleyin. | 0ab0b1a8-8aac-4efd-b8c2-3ee1fb270be8 |
> | [Azure Kubernetes hizmet kümesi Kullanıcı rolü](#azure-kubernetes-service-cluster-user-role) | Küme kullanıcı kimlik bilgilerini Listele eylemi. | 4abbcc35-E782-43d8-92C5-2d3f1bd2253f |
> | **Veritabanları** |  |  |
> | [Cosmos DB hesabı okuyucu rolü](#cosmos-db-account-reader-role) | Azure Cosmos DB hesabı verilerini okuyabilir. Azure Cosmos DB hesaplarını yönetmek için [DocumentDB hesabı katılımcısı](#documentdb-account-contributor) konusuna bakın. | fbdf93bf-df7d-467e-a4d2-9458aa1360c8 |
> | [Cosmos DB Işleci](#cosmos-db-operator) | Azure Cosmos DB hesaplarını yönetmenizi sağlar ancak içerdikleri verilere erişemez. Hesap anahtarlarına ve bağlantı dizelerine erişimi engeller. | 230815da-be43-4aae-9cb4-875f7bd000aa |
> | [CosmosBackupOperator](#cosmosbackupoperator) | Bir Cosmos DB veritabanı veya bir hesabın kapsayıcısı için geri yükleme isteği gönderebilir | db7b14f2-5adf-42da-9f96-f2ee17bab5cb |
> | [DocumentDB hesabı Katılımcısı](#documentdb-account-contributor) | , Azure Cosmos DB hesaplarını yönetebilir. Azure Cosmos DB daha önce DocumentDB olarak bilinirdi. | 5bd9cd88-fe45-4216-938b-f97437e15450 |
> | [Katkıda bulunan Redis Cache](#redis-cache-contributor) | Redsıs önbellekler yönetmenize izin verir, ancak bunlara erişimi olmaz. | e0f68234-74aa-48ed-b826-c38b57376e17 |
> | [SQL DB Katılımcısı](#sql-db-contributor) | SQL veritabanlarını yönetmenizi sağlar ancak onlara yönelik erişimleri vermez. Ayrıca, güvenlikle ilgili ilkeleri veya bunların üst SQL sunucularını yönetemezsiniz. | 9b7fa17d-e63e-47b0-bb0a-15c516ac86ec |
> | [SQL yönetilen örnek Katılımcısı](#sql-managed-instance-contributor) | SQL yönetilen örnekleri ve gerekli ağ yapılandırmasını yönetmenizi sağlar, ancak başkalarına erişim izni veremeyiz. | 4939a1f6-9ae0-4E48-A1E0-f2cbe897382d |
> | [SQL Güvenlik Yöneticisi](#sql-security-manager) | , SQL Server ve veritabanlarının güvenlikle ilgili ilkelerini yönetmenizi sağlar, ancak bunlara erişemez. | 056cd41c-7e88-42e1-933e-88ba6a50c9c3 |
> | [Katkıda bulunan SQL Server](#sql-server-contributor) | SQL Server ve veritabanlarını yönetmenizi sağlar, ancak bunlara yönelik erişimleri ve güvenlikle ilgili ilkeleri yönetemez. | 6d8ee4ec-f05a-4a1d-8b00-a9b17e38b437 |
> | **Analiz** |  |  |
> | [Azure Event Hubs veri sahibi](#azure-event-hubs-data-owner) | Azure Event Hubs kaynaklarına tam erişim sağlar. | f526a384-b230-433a-b45c-95f59c4a2dec |
> | [Azure Event Hubs veri alıcısı](#azure-event-hubs-data-receiver) | Azure Event Hubs kaynaklarına erişim izni verir. | a638d3c7-ab3a-418d-83e6-5f17a39d4fde |
> | [Azure Event Hubs veri gönderici](#azure-event-hubs-data-sender) | Azure Event Hubs kaynaklarına erişim gönderilmesine izin verir. | 2b629674-e913-4C01-ae53-ef4638d8f975 |
> | [Katkıda bulunan Data Factory](#data-factory-contributor) | Veri fabrikalarının yanı sıra bunların içindeki alt kaynakları oluşturun ve yönetin. | 673868aa-7521-48A0-acc6-0f60742d39f5 |
> | [Veri Takiger](#data-purger) | Analiz verilerini temizedebilir | 150f5e0c-0603-4f03-8C7F-cf70034c4e90 |
> | [HDInsight küme operatörü](#hdinsight-cluster-operator) | HDInsight küme yapılandırmasını okuyup değiştirmenize izin verir. | 61ed4efc-fab3-44fd-B111-e24485cc132a |
> | [HDInsight etki alanı Hizmetleri Katılımcısı](#hdinsight-domain-services-contributor) | HDInsight için gereken etki alanı Hizmetleri ile ilgili işlemleri okuyabilir, oluşturabilir, değiştirebilir ve silebilir Kurumsal Güvenlik Paketi | 8d8d5a11-05d3-4bdav-A417-a08778121c7c |
> | [Log Analytics Katkıda Bulunan](#log-analytics-contributor) | Log Analytics katkı, tüm izleme verilerini okuyabilir ve izleme ayarlarını düzenleyebilir. İzleme ayarlarını düzenlediğinizde VM 'lere VM uzantısının eklenmesi dahildir; Azure depolama 'dan günlüklerin toplanmasını yapılandırabilmek için depolama hesabı anahtarlarını okuma; Otomasyon hesapları oluşturma ve yapılandırma; çözümler ekleme; ve Azure tanılama 'yı tüm Azure kaynaklarında yapılandırma. | 92aaf0dad-9dadb-42b6-94a3-d43ce8d16293 |
> | [Log Analytics Okuyucusu](#log-analytics-reader) | Log Analytics okuyucu tüm izleme verilerini görüntüleyip arayabilir ve tüm Azure kaynaklarında Azure tanılama 'nın yapılandırılmasını görüntüleme dahil olmak üzere izleme ayarlarını görüntüleyebilir. | 73c42c96-874c-492b-b04d-ab87d138a893 |
> | **Blok zinciri** |  |  |
> | [Blok zinciri üye düğümü erişimi (Önizleme)](#blockchain-member-node-access-preview) | Blok zinciri üye düğümlerine erişim sağlar | 31a002a1-acaf-453E-8a5b-297c9ca1ea24 |
> | **AI + makine öğrenimi** |  |  |
> | [Bilişsel hizmetler Katılımcısı](#cognitive-services-contributor) | Bilişsel hizmetler için anahtar oluşturma, okuma, güncelleştirme, silme ve yönetme olanağı sağlar. | 25fbc0a9-bd7c-42A3-aa1a-3b75d497ee68 |
> | [Bilişsel hizmetler veri okuyucu (Önizleme)](#cognitive-services-data-reader-preview) | Bilişsel hizmetler verilerini okumanızı sağlar. | b59867f0-fa02-499b-be73-45a86b5b3e1c |
> | [Bilişsel hizmetler kullanıcısı](#cognitive-services-user) | Bilişsel hizmetler 'in anahtarlarını okuyup listelemenizi sağlar. | a97b65f3-24c7-4388-baec-2e87135dc908 |
> | **Karma Gerçeklik** |  |  |
> | [Uzamsal bağlayıcı hesabı Katılımcısı](#spatial-anchors-account-contributor) | Hesabınızdaki uzamsal bağlantıları yönetmenizi sağlar, ancak onları silmez | 8bbe83f1-e2a6-4df7-8cb4-4e04d4e5c827 |
> | [Uzamsal bağlayıcı hesap sahibi](#spatial-anchors-account-owner) | Hesabınızdaki uzamsal bağlayıcıları, silme dahil olmak üzere yönetmenizi sağlar | 70bbe301-9835-447d-afdd-19eb3167307c |
> | [Uzamsal bağlayıcı hesap okuyucu](#spatial-anchors-account-reader) | Hesabınızdaki uzamsal Tutturucuların özelliklerini bulmanıza ve okumanızı sağlar | 5d51204f-EB77-4B1C-b86a-2ec626c49413 |
> | **Tümleştirme** |  |  |
> | [API Management hizmet Katılımcısı](#api-management-service-contributor) | Hizmeti ve API 'Leri yönetebilir | 312a565d-c81f-4fd8-895a-4e21e48d571c |
> | [Hizmet Işletmeni rolü API Management](#api-management-service-operator-role) | Hizmeti yönetebilir, ancak API 'Leri yönetemez | e022efe7-F5BA-4159-bbe4-b44f577e9b61 |
> | [API Management hizmeti okuyucu rolü](#api-management-service-reader-role) | Hizmet ve API 'lere salt okuma erişimi | 71522526-b88f-4d52-b57f-d31fc3546d0d |
> | [Uygulama yapılandırma veri sahibi](#app-configuration-data-owner) | Uygulama yapılandırma verilerine tam erişim sağlar. | 5ae67dd6-50cb-40e7-96ff-dc2bfa4b606b |
> | [Uygulama yapılandırma veri okuyucusu](#app-configuration-data-reader) | Uygulama yapılandırma verilerine okuma erişimi sağlar. | 516239f1-63e1-4d78-a4de-a74fb236a071 |
> | [Azure Service Bus veri sahibi](#azure-service-bus-data-owner) | Azure Service Bus kaynaklara tam erişim sağlar. | 090c5cfd-751d-490a-894a-3ce6f1109419 |
> | [Azure Service Bus veri alıcısı](#azure-service-bus-data-receiver) | Azure Service Bus kaynaklarına erişim izni verir. | 4f6d3b9b-027b-4f4c-9142-0e5a2a2247e0 |
> | [Veri Göndericisini Azure Service Bus](#azure-service-bus-data-sender) | Azure Service Bus kaynaklarına erişim izni verir. | 69a216fc-b8fb-44d8-bc22-1f3c2cd27a39 |
> | [Kayıt sahibini Azure Stack](#azure-stack-registration-owner) | Azure Stack kayıtlarını yönetmenizi sağlar. | 6f12a6df-dd06-4f3e-bcb1-ce8be600526a |
> | [EventGrid EventSubscription Katılımcısı](#eventgrid-eventsubscription-contributor) | EventGrid olay aboneliği işlemlerini yönetmenizi sağlar. | 428e0ff0-5e57-4d9c-A221-2c70d0e0a443 |
> | [EventGrid EventSubscription okuyucusu](#eventgrid-eventsubscription-reader) | EventGrid olay aboneliklerini okumanızı sağlar. | 2414bbcf-6497-4FAF-8c65-045460748405 |
> | [Intelligent Systems hesabı Katılımcısı](#intelligent-systems-account-contributor) | Akıllı sistem hesaplarını yönetmenizi sağlar ancak onlara yönelik erişimleri vermez. | 03a6d094-3444-4b3d-88af-7477090a9e5e |
> | [Mantıksal uygulama Katılımcısı](#logic-app-contributor) | Mantıksal uygulamaları yönetmenize izin verir, ancak bunlara erişimi değiştirmeyin. | 87a39d53-fc1b-424a-814c-f7e04687dc9e |
> | [Logic App Işleci](#logic-app-operator) | Logic Apps 'i okumanızı, etkinleştirmenizi ve devre dışı bırakmanızı sağlar, ancak bunları düzenleyemez veya güncelleştiremez. | 515c2055-d9d4-4321-b1b9-bd0c9a0f79fe |
> | **Kimlik** |  |  |
> | [Yönetilen kimlik Katılımcısı](#managed-identity-contributor) | Kullanıcı tarafından atanan kimlik oluşturma, okuma, güncelleştirme ve silme | e40ec5ca-96e0-45a2-b4ff-59039f2c2b59 |
> | [Yönetilen kimlik Işleci](#managed-identity-operator) | Kullanıcı tarafından atanan kimliği okuma ve atama | f1a07417-d97a-45cb-824c-7a7467783830 |
> | **Güvenlik** |  |  |
> | [Azure Sentinel Katılımcısı](#azure-sentinel-contributor) | Azure Sentinel Katılımcısı | ab8e14d6-4a74-4a29-9ba8-549422addade |
> | [Azure Sentinel okuyucusu](#azure-sentinel-reader) | Azure Sentinel okuyucusu | 8d289c81-5878-46d4-8554-54e1e3d8b5cb |
> | [Azure Sentinel Yanıtlayıcısı](#azure-sentinel-responder) | Azure Sentinel Yanıtlayıcısı | 3e150937-b8fe-4CFB-8069-0eaf05ecd056 |
> | [Katkıda bulunan Key Vault](#key-vault-contributor) | Anahtar kasalarını yönetmenize izin verir, ancak bunlara erişim sağlamaz. | f25e0fa2-a7c8-4377-a976-54943a77a395 |
> | [Güvenlik Yöneticisi](#security-admin) | Güvenlik Merkezi için izinleri görüntüleyin ve güncelleştirin. Güvenlik okuyucu rolüyle aynı izinler ve ayrıca güvenlik ilkesini güncelleştirebilir ve uyarıları ve önerileri kapatabilir. | fb1c8493-542b-48eb-b624-b4c8fea62acd |
> | [Güvenlik değerlendirmesi Katılımcısı](#security-assessment-contributor) | Değerlendirmelere Güvenlik Merkezi 'ne gönderim olanağı sağlar | 612c2aa1-CB24-443B-ac28-3ab7272de6f5 |
> | [Güvenlik Yöneticisi (eski)](#security-manager-legacy) | Bu eski bir roldür. Lütfen bunun yerine Güvenlik Yöneticisi 'ni kullanın. | e3d13bf0-dd5a-482e-ba6b-9b8433878d10 |
> | [Güvenlik okuyucusu](#security-reader) | Güvenlik Merkezi için izinleri görüntüleyin. Önerileri, uyarıları, güvenlik ilkesini ve güvenlik durumlarını görüntüleyebilir, ancak değişiklik yapamaz. | 39bc4728-0917-49c7-9d2c-d95423bc2eb4 |
> | **DevOps** |  |  |
> | [DevTest Labs kullanıcısı](#devtest-labs-user) | Azure DevTest Labs sanal makinelerinizi bağlamanıza, başlatmanıza, yeniden başlatmanıza ve kapatımanıza olanak sağlar. | 76283e04-6283-4c54-8F91-bcf1374a3c64 |
> | [Laboratuvar Oluşturucu](#lab-creator) | Azure Laboratuvar hesaplarınız kapsamında yönetilen Laboratuvarlarınızı oluşturmanıza, yönetmenize ve silmenize olanak sağlar. | b97fb8bc-a8b2-4522-a38b-dd33c7e65ead |
> | **İzleyici** |  |  |
> | [Application Insights bileşeni Katılımcısı](#application-insights-component-contributor) | Application Insights bileşenlerini yönetebilir | ae349356-3a1b-4a5e-921d-050484c6347e |
> | [Application Insights Snapshot Debugger](#application-insights-snapshot-debugger) | Application Insights Snapshot Debugger ile toplanan hata ayıklama anlık görüntülerini görüntülemek ve indirmek için kullanıcıya izin verir. Bu izinlerin [sahip](#owner) veya [katkıda](#contributor) bulunan rollerine dahil edilmediğini unutmayın. | 08954f03-6346-4c2e-81c0-ec3a5cfae23b |
> | [Katkıda bulunan izleniyor](#monitoring-contributor) | Tüm izleme verilerini okuyabilir ve izleme ayarlarını düzenleyebilir. Ayrıca bkz. [Azure izleyici ile roller, izinler ve güvenlik ile çalışmaya başlama](https://docs.microsoft.com/azure/azure-monitor/platform/roles-permissions-security#built-in-monitoring-roles). | 749f88d5-cbae-40b8-bcfc-e573ddc772fa |
> | [Ölçüm yayımcısını izleme](#monitoring-metrics-publisher) | Azure kaynaklarında ölçüm yayımlamaya izin vermez | 3913510d-42F4-4E42-8A64-420c390055eb |
> | [İzleme okuyucusu](#monitoring-reader) | Tüm izleme verilerini okuyabilir (ölçümler, Günlükler vb.). Ayrıca bkz. [Azure izleyici ile roller, izinler ve güvenlik ile çalışmaya başlama](https://docs.microsoft.com/azure/azure-monitor/platform/roles-permissions-security#built-in-monitoring-roles). | 43d0d8ad-25c7-4714-9337-8ba259a9fe05 |
> | [Çalışma kitabı Katılımcısı](#workbook-contributor) | , Paylaşılan çalışma kitaplarını kaydedebilir. | e8ddcd69-c73f-4f9f-9844-4100522f16ad |
> | [Çalışma kitabı okuyucusu](#workbook-reader) | , Çalışma kitaplarını okuyabilir. | b279062a-9be3-42a0-92ae-8b3cf002ec4d |
> | **Yönetim + idare** |  |  |
> | [Automation Iş Işleci](#automation-job-operator) | Otomasyon Runbook 'Larını kullanarak Iş oluşturun ve yönetin. | 4fe576fe-1146-4730-92eb-48519fa6bf9f |
> | [Automation Işleci](#automation-operator) | Automation Işleçleri, işleri başlatabilir, durdurabilir, askıya alabilir ve sürdürebilir | d3881f73-407A-4167-8283-e981cbba0404 |
> | [Otomasyon Runbook Işleci](#automation-runbook-operator) | Runbook 'un Işlerini oluşturabilmek için Runbook özelliklerini okuyun. | 5fb5aef8-1081-4b8e-bb16-9d5d0385bab5 |
> | [Azure bağlı makine ekleme](#azure-connected-machine-onboarding) | , Azure bağlantılı makineler ekleyebilir. | b64e21ea-ac4e-4cdf-9dc9-5b892992bee7 |
> | [Azure bağlı makine kaynak yöneticisi](#azure-connected-machine-resource-administrator) | Azure bağlı makinelerini okuyabilir, yazabilir, silebilir ve yeniden ekleyebilir. | cd570a14-e51a-42ad-bac8-bafd67325302 |
> | [Faturalama okuyucusu](#billing-reader) | Faturalandırma verilerine okuma erişimi sağlar | fa23ad8b-c56e-40d8-ac0c-ce449e1d2c64 |
> | [Blueprint Katılımcısı](#blueprint-contributor) | Şema tanımlarını yönetebilir, ancak atamazsınız. | 41077137-e803-4205-871C-5a86e6a753b4 |
> | [Blueprint Işleci](#blueprint-operator) | Varolan yayımlanmış şemaları atayabilir, ancak yeni şemaları oluşturamaz. Bu, yalnızca atama Kullanıcı tarafından atanan yönetilen bir kimlikle yapıldığında işe yarar. | 437d2ced-4a38-4302-8479-ed2bcb43d090 |
> | [Maliyet yönetimi Katılımcısı](#cost-management-contributor) | Maliyetleri görüntüleyebilir ve maliyet yapılandırmasını yönetebilir (örn. bütçeler, dışarı aktarmalar) | 434105ed-43f6-45c7-a02f-909b2ba83430 |
> | [Maliyet yönetimi okuyucusu](#cost-management-reader) | Maliyet verilerini ve yapılandırmayı görüntüleyebilir (örneğin, bütçeler, dışarı aktarmalar) | 72fafb9e-0641-4937-9268-a91bfd8191a3 |
> | [Yönetilen uygulama katılımcısı rolü](#managed-application-contributor-role) | Yönetilen uygulama kaynakları oluşturulmasına izin verir. | 641177b8-a67a-45b9-a033-47bc880bb21e |
> | [Yönetilen uygulama Işletmeni rolü](#managed-application-operator-role) | Yönetilen uygulama kaynakları üzerinde işlemleri okuyup gerçekleştirmenize olanak tanır | c7393b34-138c-406f-901b-d8cf2b17e6ae |
> | [Yönetilen uygulamalar okuyucusu](#managed-applications-reader) | Yönetilen bir uygulamadaki kaynakları okumanızı ve JıT erişimi isteğinizi yapmanızı sağlar. | b9331d33-8a36-4f8c-b097-4f54124fdb44 |
> | [Yönetilen hizmetler kayıt ataması rol silme](#managed-services-registration-assignment-delete-role) | Yönetilen hizmetler kayıt ataması silme rolü, kiracı kullanıcılarının kiracıya atanan kayıt atamasını silmesine izin verir. | 91c1777a-f3dc-4fae-B103-61d183457e46 |
> | [Yönetim grubu Katılımcısı](#management-group-contributor) | Yönetim grubu katılımcısı rolü | 5d58bcaf-24a5-4b20-bdb6-eed9f69fbe4c |
> | [Yönetim grubu okuyucusu](#management-group-reader) | Yönetim grubu okuyucusu rolü | ac63b705-f282-497d-ac71-919bf39d939d |
> | [Yeni relik APM hesabı Katılımcısı](#new-relic-apm-account-contributor) | New Relic Application Performance Management hesaplarını ve uygulamaları yönetmenize izin verir, ancak bunlara erişimi kalmaz. | 5d28c62d-5b37-4476-8438-e587778df237 |
> | [İlke öngörüleri veri yazıcısı (Önizleme)](#policy-insights-data-writer-preview) | Kaynak ilkelerine okuma erişimine ve kaynak bileşen ilkesi olaylarına yazma erişimine izin verir. | 66bb4e9e-B016-4A94-8249-4c0511c2be84 |
> | [Kaynak İlkesine Katkıda Bulunan](#resource-policy-contributor) | Kaynak ilkesi oluşturma/değiştirme, destek bileti oluşturma ve kaynakları/hiyerarşisi okuma haklarına sahip kullanıcılar. | 36243c78-bf99-498c-9df9-86d9f8d28608 |
> | [Katkıda bulunan Site Recovery](#site-recovery-contributor) | Kasa oluşturma ve rol atama dışında Site Recovery hizmetini yönetmenizi sağlar | 6670b86e-a3f7-4917-AC9B-5d6ab1be4567 |
> | [Site Recovery Işleci](#site-recovery-operator) | Yük devretme ve yeniden çalışma ve diğer Site Recovery yönetim işlemlerini gerçekleştirmenize izin verir | 494ae006-DB33-4328-BF46-533a6560a3ca |
> | [Site Recovery okuyucu](#site-recovery-reader) | Site Recovery durumunu görüntülemenize izin verir, ancak diğer yönetim işlemlerini gerçekleştirmenize izin vermez | dbaa88c4-0c30-4179-9fb3-46319faa6149 |
> | [Destek Isteği Katılımcısı](#support-request-contributor) | Destek istekleri oluşturmanızı ve yönetmenizi sağlar | cfd33db0-3dd1-45e3-aa9d-cdbdf3b6f24e |
> | [Etiket Katılımcısı](#tag-contributor) | Varlıklarda kendilerine erişim sağlamamanıza gerek kalmadan varlıklarda etiketleri yönetmenizi sağlar. | 4a9ae827-6dc8-4573-8ac7-8239d42aa03f |
> | **Diğer** |  |  |
> | [BizTalk Katılımcısı](#biztalk-contributor) | BizTalk hizmetlerini yönetmenizi sağlar ancak onlara erişim izni vermez. | 5e3c6656-6cfa-4708-81fe-0de47ac73342 |
> | [Zamanlayıcı Iş koleksiyonları Katılımcısı](#scheduler-job-collections-contributor) | Zamanlayıcı iş koleksiyonlarını yönetmenizi sağlar, ancak bunlara erişimi kalmaz. | 188a0f2f-5c9e-469B-ae67-2aa5ce574b94 |


## <a name="general"></a>Genel


### <a name="contributor"></a>Katılımcı

Kaynaklara erişim verme dışında her şeyi yönetmenizi sağlar.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | * | Tüm türlerin kaynaklarını oluşturma ve yönetme |
> | **NotActions** |  |
> | Microsoft. Authorization/*/Delete | Rolleri, ilke atamalarını, ilke tanımlarını ve ilke kümesi tanımlarını Sil |
> | Microsoft. Authorization/*/Write | Roller, rol atamaları, ilke atamaları, ilke tanımları ve ilke kümesi tanımları oluşturun |
> | Microsoft. Authorization/yükseltir Teaccess/Action | Çağrı yapana kiracı kapsamında Kullanıcı Erişimi Yöneticisi erişim izni verir |
> | Microsoft. Blueprint/Blueprintasbir/Write | Şema atamalarını oluşturun veya güncelleştirin |
> | Microsoft. Blueprint/Blueprintasbir/Delete | Tüm şema atamalarını silme |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage everything except access to resources.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/b24988ac-6180-42a0-ab88-20f7382dd24c",
  "name": "b24988ac-6180-42a0-ab88-20f7382dd24c",
  "permissions": [
    {
      "actions": [
        "*"
      ],
      "notActions": [
        "Microsoft.Authorization/*/Delete",
        "Microsoft.Authorization/*/Write",
        "Microsoft.Authorization/elevateAccess/Action",
        "Microsoft.Blueprint/blueprintAssignments/write",
        "Microsoft.Blueprint/blueprintAssignments/delete"
      ],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="owner"></a>Sahip

Kaynaklara erişim de dahil olmak üzere her şeyi yönetmenizi sağlar.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | * | Tüm türlerin kaynaklarını oluşturma ve yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage everything, including access to resources.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/8e3af657-a8ff-443c-a75c-2fe8c4bcb635",
  "name": "8e3af657-a8ff-443c-a75c-2fe8c4bcb635",
  "permissions": [
    {
      "actions": [
        "*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Owner",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="reader"></a>Okuyucu

Her şeyi görüntülemenize izin verir, ancak hiçbir değişiklik yapmamalıdır.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | */Read | Gizli dizileri hariç tüm türlerin kaynaklarını okuyun. |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you view everything, but not make any changes.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/acdd72a7-3385-48ef-bd42-f606fba81ae7",
  "name": "acdd72a7-3385-48ef-bd42-f606fba81ae7",
  "permissions": [
    {
      "actions": [
        "*/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="user-access-administrator"></a>Kullanıcı Erişimi Yöneticisi

Azure kaynaklarına Kullanıcı erişimini yönetmenizi sağlar.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | */Read | Gizli dizileri hariç tüm türlerin kaynaklarını okuyun. |
> | Microsoft. Authorization/* | Yetkilendirmeyi Yönetme |
> | Microsoft. support/* | Destek bileti oluşturma ve güncelleştirme |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage user access to Azure resources.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/18d7d88d-d35e-4fb5-a5c3-7773c20a72d9",
  "name": "18d7d88d-d35e-4fb5-a5c3-7773c20a72d9",
  "permissions": [
    {
      "actions": [
        "*/read",
        "Microsoft.Authorization/*",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "User Access Administrator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

## <a name="compute"></a>İşlem


### <a name="classic-virtual-machine-contributor"></a>Klasik sanal makine Katılımcısı

Klasik sanal makineleri yönetmenizi sağlar ancak bunlara yönelik erişimi, bağlı oldukları sanal ağ veya depolama hesabı için değil.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. Authorization/*/Read | Rolleri ve rol atamalarını oku |
> | Microsoft. ClassicCompute/domainNames/* | Klasik işlem etki alanı adları oluşturma ve yönetme |
> | Microsoft. ClassicCompute/virtualMachines/* | Sanal makine oluşturma ve yönetme |
> | Microsoft. ClassicNetwork/networkSecurityGroups/JOIN/Action |  |
> | Microsoft. ClassicNetwork/Rezervedips/bağlantı/eylem | Ayrılmış bir IP 'yi bağlama |
> | Microsoft. ClassicNetwork/Rezervedips/Read | Ayrılmış IP 'Leri alır |
> | Microsoft. ClassicNetwork/virtualNetworks/JOIN/Action | Sanal ağı birleştirir. |
> | Microsoft. ClassicNetwork/virtualNetworks/Read | Sanal ağı alın. |
> | Microsoft. ClassicStorage/storageAccounts/Disks/Read | Depolama hesabı diskini döndürür. |
> | Microsoft. ClassicStorage/storageAccounts/Images/Read | Depolama hesabı görüntüsünü döndürür. Kullanım dışı. ' Microsoft. ClassicStorage/storageAccounts/Vmımages ' kullanın) |
> | Microsoft. ClassicStorage/storageAccounts/listKeys/Action | Depolama hesaplarının erişim anahtarlarını listeler. |
> | Microsoft. ClassicStorage/storageAccounts/Read | Verilen hesaba sahip depolama hesabını döndürün. |
> | Microsoft. Insights/alertRules/* | Klasik ölçüm uyarısı oluşturma ve yönetme |
> | Microsoft. ResourceHealth/kullanılabilirlik Bilitydurumlar/okuma | Belirtilen kapsamdaki tüm kaynaklar için Kullanılabilirlik durumlarını alır |
> | Microsoft. resources/dağıtımlar/* | Dağıtım oluşturma ve yönetme |
> | Microsoft. resources/abonelikler/resourceGroups/Read | Kaynak gruplarını alır veya listeler. |
> | Microsoft. support/* | Destek bileti oluşturma ve güncelleştirme |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage classic virtual machines, but not access to them, and not the virtual network or storage account they're connected to.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/d73bb868-a0df-4d4d-bd69-98a00b01fccb",
  "name": "d73bb868-a0df-4d4d-bd69-98a00b01fccb",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.ClassicCompute/domainNames/*",
        "Microsoft.ClassicCompute/virtualMachines/*",
        "Microsoft.ClassicNetwork/networkSecurityGroups/join/action",
        "Microsoft.ClassicNetwork/reservedIps/link/action",
        "Microsoft.ClassicNetwork/reservedIps/read",
        "Microsoft.ClassicNetwork/virtualNetworks/join/action",
        "Microsoft.ClassicNetwork/virtualNetworks/read",
        "Microsoft.ClassicStorage/storageAccounts/disks/read",
        "Microsoft.ClassicStorage/storageAccounts/images/read",
        "Microsoft.ClassicStorage/storageAccounts/listKeys/action",
        "Microsoft.ClassicStorage/storageAccounts/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Classic Virtual Machine Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="virtual-machine-administrator-login"></a>Sanal Makine Yöneticisi oturum açma

Portalda sanal makineleri görüntüleme ve yönetici olarak oturum açma

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. Network/publicIPAddresses/Read | Genel IP adresi tanımını alır. |
> | Microsoft. Network/virtualNetworks/Read | Sanal ağ tanımını al |
> | Microsoft. Network/loadBalancers/Read | Yük dengeleyici tanımını alır |
> | Microsoft. Network/NetworkInterfaces/Read | Bir ağ arabirimi tanımını alır.  |
> | Microsoft. COMPUTE/virtualMachines/*/Read |  |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | Microsoft. COMPUTE/virtualMachines/LOGIN/Action | Bir sanal makinede normal kullanıcı olarak oturum açın |
> | Microsoft. COMPUTE/virtualMachines/loginAsAdmin/Action | Windows yönetici veya Linux kök kullanıcı ayrıcalıklarına sahip bir sanal makinede oturum açma |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "View Virtual Machines in the portal and login as administrator",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/1c0163c0-47e6-4577-8991-ea5c82e286e4",
  "name": "1c0163c0-47e6-4577-8991-ea5c82e286e4",
  "permissions": [
    {
      "actions": [
        "Microsoft.Network/publicIPAddresses/read",
        "Microsoft.Network/virtualNetworks/read",
        "Microsoft.Network/loadBalancers/read",
        "Microsoft.Network/networkInterfaces/read",
        "Microsoft.Compute/virtualMachines/*/read"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.Compute/virtualMachines/login/action",
        "Microsoft.Compute/virtualMachines/loginAsAdmin/action"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Virtual Machine Administrator Login",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="virtual-machine-contributor"></a>Sanal Makine Katılımcısı

Sanal makineleri yönetmenize izin verir, ancak bunlara bağlı oldukları sanal ağ veya depolama hesabına erişemez.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. Authorization/*/Read | Rolleri ve rol atamalarını oku |
> | Microsoft. COMPUTE/kullanılabilirliği Bilitysets/* | İşlem kullanılabilirlik kümeleri oluşturma ve yönetme |
> | Microsoft. COMPUTE/Locations/* | İşlem konumları oluşturma ve yönetme |
> | Microsoft. COMPUTE/virtualMachines/* | Sanal makine oluşturma ve yönetme |
> | Microsoft. COMPUTE/virtualMachineScaleSets/* | Sanal makine ölçek kümeleri oluşturma ve yönetme |
> | Microsoft. COMPUTE/Disks/Write | Yeni bir disk oluşturur veya var olan bir diski güncelleştirir |
> | Microsoft. COMPUTE/Disks/Read | Bir diskin özelliklerini al |
> | Microsoft. COMPUTE/diskler/Sil | Diski siler |
> | Microsoft. DevTestLab/zamanlamalar/* |  |
> | Microsoft. Insights/alertRules/* | Klasik ölçüm uyarısı oluşturma ve yönetme |
> | Microsoft. Network/Applicationgateway/Backendavddresspoir/JOIN/Action | Bir uygulama ağ geçidi arka uç adres havuzunu birleştirir. Alertable değil. |
> | Microsoft. Network/loadBalancers/Backendadddresspoir/JOIN/Action | Yük dengeleyici arka uç adres havuzunu birleştirir. Alertable değil. |
> | Microsoft. Network/loadBalancers/ınboundnatpools/JOIN/Action | Yük dengeleyici gelen NAT havuzunu birleştirir. Alertable değil. |
> | Microsoft. Network/loadBalancers/ınboundnatrules/JOIN/Action | Yük dengeleyici gelen NAT kuralına katılır. Alertable değil. |
> | Microsoft. Network/loadBalancers/araştırmaları/JOIN/Action | Yük dengeleyicinin araştırmalarını sağlar. Örneğin, bu izinle VM Ölçek kümesinin Healtharaştırma özelliği araştırmasına başvurabilir. Alertable değil. |
> | Microsoft. Network/loadBalancers/Read | Yük dengeleyici tanımını alır |
> | Microsoft. Network/Locations/* | Ağ konumları oluşturma ve yönetme |
> | Microsoft. Network/NetworkInterfaces/* | Ağ arabirimlerini oluşturma ve yönetme |
> | Microsoft. Network/networkSecurityGroups/JOIN/Action | Bir ağ güvenlik grubuna katılır. Alertable değil. |
> | Microsoft. Network/networkSecurityGroups/Read | Bir ağ güvenlik grubu tanımını alır |
> | Microsoft. Network/publicIPAddresses/JOIN/Action | Genel bir IP adresini birleştirir. Alertable değil. |
> | Microsoft. Network/publicIPAddresses/Read | Genel IP adresi tanımını alır. |
> | Microsoft. Network/virtualNetworks/Read | Sanal ağ tanımını al |
> | Microsoft. Network/virtualNetworks/alt ağlar/JOIN/Action | Bir sanal ağı birleştirir. Alertable değil. |
> | Microsoft. RecoveryServices/konumlar/* |  |
> | Microsoft. RecoveryServices/Vaults/Backupdokuları/Backupprotectionamacını/Write | Yedekleme koruma hedefi oluşturma |
> | Microsoft. RecoveryServices/Vaults/Backupdokuları/protectionContainers/korunabilir/*/Read |  |
> | Microsoft. RecoveryServices/Vaults/Backupdokuları/protectionContainers/korunabilir/Read | Korumalı öğenin nesne ayrıntılarını döndürür |
> | Microsoft. RecoveryServices/Vaults/Backupdokuları/protectionContainers/korunabilir/yazma | Yedekleme korumalı öğesi oluştur |
> | Microsoft. RecoveryServices/Vaults/backupPolicies/Read | Tüm koruma Ilkelerini döndürür |
> | Microsoft. RecoveryServices/Vaults/backupPolicies/Write | Koruma Ilkesi oluşturur |
> | Microsoft. RecoveryServices/Vaults/okuma | Kasayı al işlemi, ' kasa ' türündeki Azure kaynağını temsil eden bir nesneyi alır |
> | Microsoft. RecoveryServices/Kasaults/kullanımlar/okuma | Bir kurtarma hizmetleri Kasası için kullanım ayrıntılarını döndürür. |
> | Microsoft. RecoveryServices/Vaults/Write | Kasa oluşturma işlemi, ' kasa ' türünde bir Azure kaynağı oluşturur |
> | Microsoft. ResourceHealth/kullanılabilirlik Bilitydurumlar/okuma | Belirtilen kapsamdaki tüm kaynaklar için Kullanılabilirlik durumlarını alır |
> | Microsoft. resources/dağıtımlar/* | Dağıtım oluşturma ve yönetme |
> | Microsoft. resources/abonelikler/resourceGroups/Read | Kaynak gruplarını alır veya listeler. |
> | Microsoft. SqlVirtualMachine/* |  |
> | Microsoft. Storage/storageAccounts/listKeys/Action | Belirtilen depolama hesabı için erişim anahtarlarını döndürür. |
> | Microsoft. Storage/storageAccounts/Read | Depolama hesaplarının listesini döndürür veya belirtilen depolama hesabının özelliklerini alır. |
> | Microsoft. support/* | Destek bileti oluşturma ve güncelleştirme |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage virtual machines, but not access to them, and not the virtual network or storage account they're connected to.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
  "name": "9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Compute/availabilitySets/*",
        "Microsoft.Compute/locations/*",
        "Microsoft.Compute/virtualMachines/*",
        "Microsoft.Compute/virtualMachineScaleSets/*",
        "Microsoft.Compute/disks/write",
        "Microsoft.Compute/disks/read",
        "Microsoft.Compute/disks/delete",
        "Microsoft.DevTestLab/schedules/*",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Network/applicationGateways/backendAddressPools/join/action",
        "Microsoft.Network/loadBalancers/backendAddressPools/join/action",
        "Microsoft.Network/loadBalancers/inboundNatPools/join/action",
        "Microsoft.Network/loadBalancers/inboundNatRules/join/action",
        "Microsoft.Network/loadBalancers/probes/join/action",
        "Microsoft.Network/loadBalancers/read",
        "Microsoft.Network/locations/*",
        "Microsoft.Network/networkInterfaces/*",
        "Microsoft.Network/networkSecurityGroups/join/action",
        "Microsoft.Network/networkSecurityGroups/read",
        "Microsoft.Network/publicIPAddresses/join/action",
        "Microsoft.Network/publicIPAddresses/read",
        "Microsoft.Network/virtualNetworks/read",
        "Microsoft.Network/virtualNetworks/subnets/join/action",
        "Microsoft.RecoveryServices/locations/*",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/backupProtectionIntent/write",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/*/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/write",
        "Microsoft.RecoveryServices/Vaults/backupPolicies/read",
        "Microsoft.RecoveryServices/Vaults/backupPolicies/write",
        "Microsoft.RecoveryServices/Vaults/read",
        "Microsoft.RecoveryServices/Vaults/usages/read",
        "Microsoft.RecoveryServices/Vaults/write",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.SqlVirtualMachine/*",
        "Microsoft.Storage/storageAccounts/listKeys/action",
        "Microsoft.Storage/storageAccounts/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Virtual Machine Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="virtual-machine-user-login"></a>Sanal makine Kullanıcı oturumu açma

Portalda sanal makineleri görüntüleyin ve normal bir kullanıcı olarak oturum açın.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. Network/publicIPAddresses/Read | Genel IP adresi tanımını alır. |
> | Microsoft. Network/virtualNetworks/Read | Sanal ağ tanımını al |
> | Microsoft. Network/loadBalancers/Read | Yük dengeleyici tanımını alır |
> | Microsoft. Network/NetworkInterfaces/Read | Bir ağ arabirimi tanımını alır.  |
> | Microsoft. COMPUTE/virtualMachines/*/Read |  |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | Microsoft. COMPUTE/virtualMachines/LOGIN/Action | Bir sanal makinede normal kullanıcı olarak oturum açın |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "View Virtual Machines in the portal and login as a regular user.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/fb879df8-f326-4884-b1cf-06f3ad86be52",
  "name": "fb879df8-f326-4884-b1cf-06f3ad86be52",
  "permissions": [
    {
      "actions": [
        "Microsoft.Network/publicIPAddresses/read",
        "Microsoft.Network/virtualNetworks/read",
        "Microsoft.Network/loadBalancers/read",
        "Microsoft.Network/networkInterfaces/read",
        "Microsoft.Compute/virtualMachines/*/read"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.Compute/virtualMachines/login/action"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Virtual Machine User Login",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

## <a name="networking"></a>Ağ


### <a name="cdn-endpoint-contributor"></a>CDN uç noktası Katılımcısı

CDN uç noktalarını yönetebilir, ancak diğer kullanıcılara erişim izni veremez.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. Authorization/*/Read | Rolleri ve rol atamalarını oku |
> | Microsoft. CDN/edgenodes/okuma |  |
> | Microsoft. CDN/operationresults/* |  |
> | Microsoft. CDN/profiller/uç noktaları/* |  |
> | Microsoft. Insights/alertRules/* | Klasik ölçüm uyarısı oluşturma ve yönetme |
> | Microsoft. resources/dağıtımlar/* | Dağıtım oluşturma ve yönetme |
> | Microsoft. resources/abonelikler/resourceGroups/Read | Kaynak gruplarını alır veya listeler. |
> | Microsoft. support/* | Destek bileti oluşturma ve güncelleştirme |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can manage CDN endpoints, but can't grant access to other users.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/426e0c7f-0c7e-4658-b36f-ff54d6c29b45",
  "name": "426e0c7f-0c7e-4658-b36f-ff54d6c29b45",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Cdn/edgenodes/read",
        "Microsoft.Cdn/operationresults/*",
        "Microsoft.Cdn/profiles/endpoints/*",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "CDN Endpoint Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="cdn-endpoint-reader"></a>CDN uç nokta okuyucusu

CDN uç noktalarını görüntüleyebilir, ancak değişiklik yapamaz.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. Authorization/*/Read | Rolleri ve rol atamalarını oku |
> | Microsoft. CDN/edgenodes/okuma |  |
> | Microsoft. CDN/operationresults/* |  |
> | Microsoft. CDN/Profiles/uç noktaları/*/Read |  |
> | Microsoft. Insights/alertRules/* | Klasik ölçüm uyarısı oluşturma ve yönetme |
> | Microsoft. resources/dağıtımlar/* | Dağıtım oluşturma ve yönetme |
> | Microsoft. resources/abonelikler/resourceGroups/Read | Kaynak gruplarını alır veya listeler. |
> | Microsoft. support/* | Destek bileti oluşturma ve güncelleştirme |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can view CDN endpoints, but can't make changes.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/871e35f6-b5c1-49cc-a043-bde969a0f2cd",
  "name": "871e35f6-b5c1-49cc-a043-bde969a0f2cd",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Cdn/edgenodes/read",
        "Microsoft.Cdn/operationresults/*",
        "Microsoft.Cdn/profiles/endpoints/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "CDN Endpoint Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="cdn-profile-contributor"></a>CDN profili Katılımcısı

CDN profillerini ve uç noktalarını yönetebilir, ancak diğer kullanıcılara erişim izni veremez.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. Authorization/*/Read | Rolleri ve rol atamalarını oku |
> | Microsoft. CDN/edgenodes/okuma |  |
> | Microsoft. CDN/operationresults/* |  |
> | Microsoft. CDN/Profiles/* |  |
> | Microsoft. Insights/alertRules/* | Klasik ölçüm uyarısı oluşturma ve yönetme |
> | Microsoft. resources/dağıtımlar/* | Dağıtım oluşturma ve yönetme |
> | Microsoft. resources/abonelikler/resourceGroups/Read | Kaynak gruplarını alır veya listeler. |
> | Microsoft. support/* | Destek bileti oluşturma ve güncelleştirme |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can manage CDN profiles and their endpoints, but can't grant access to other users.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/ec156ff8-a8d1-4d15-830c-5b80698ca432",
  "name": "ec156ff8-a8d1-4d15-830c-5b80698ca432",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Cdn/edgenodes/read",
        "Microsoft.Cdn/operationresults/*",
        "Microsoft.Cdn/profiles/*",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "CDN Profile Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="cdn-profile-reader"></a>CDN profili okuyucu

CDN profillerini ve uç noktalarını görüntüleyebilir, ancak değişiklik yapamaz.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. Authorization/*/Read | Rolleri ve rol atamalarını oku |
> | Microsoft. CDN/edgenodes/okuma |  |
> | Microsoft. CDN/operationresults/* |  |
> | Microsoft. CDN/profiller/*/Read |  |
> | Microsoft. Insights/alertRules/* | Klasik ölçüm uyarısı oluşturma ve yönetme |
> | Microsoft. resources/dağıtımlar/* | Dağıtım oluşturma ve yönetme |
> | Microsoft. resources/abonelikler/resourceGroups/Read | Kaynak gruplarını alır veya listeler. |
> | Microsoft. support/* | Destek bileti oluşturma ve güncelleştirme |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can view CDN profiles and their endpoints, but can't make changes.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/8f96442b-4075-438f-813d-ad51ab4019af",
  "name": "8f96442b-4075-438f-813d-ad51ab4019af",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Cdn/edgenodes/read",
        "Microsoft.Cdn/operationresults/*",
        "Microsoft.Cdn/profiles/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "CDN Profile Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="classic-network-contributor"></a>Klasik Ağ Katılımcısı

Klasik ağları yönetmenize izin verir, ancak bunlara erişemez.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. Authorization/*/Read | Rolleri ve rol atamalarını oku |
> | Microsoft. ClassicNetwork/* | Klasik ağları oluşturma ve yönetme |
> | Microsoft. Insights/alertRules/* | Klasik ölçüm uyarısı oluşturma ve yönetme |
> | Microsoft. ResourceHealth/kullanılabilirlik Bilitydurumlar/okuma | Belirtilen kapsamdaki tüm kaynaklar için Kullanılabilirlik durumlarını alır |
> | Microsoft. resources/dağıtımlar/* | Dağıtım oluşturma ve yönetme |
> | Microsoft. resources/abonelikler/resourceGroups/Read | Kaynak gruplarını alır veya listeler. |
> | Microsoft. support/* | Destek bileti oluşturma ve güncelleştirme |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage classic networks, but not access to them.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/b34d265f-36f7-4a0d-a4d4-e158ca92e90f",
  "name": "b34d265f-36f7-4a0d-a4d4-e158ca92e90f",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.ClassicNetwork/*",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Classic Network Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="dns-zone-contributor"></a>DNS bölgesi Katılımcısı

Azure DNS, DNS bölgelerini ve kayıt kümelerini yönetmenizi sağlar, ancak bunlara kimlerin erişebileceğini denetlemenize izin vermez.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. Authorization/*/Read | Rolleri ve rol atamalarını oku |
> | Microsoft. Insights/alertRules/* | Klasik ölçüm uyarısı oluşturma ve yönetme |
> | Microsoft. Network/dnsZones/* | DNS bölgeleri ve kayıtları oluşturma ve yönetme |
> | Microsoft. ResourceHealth/kullanılabilirlik Bilitydurumlar/okuma | Belirtilen kapsamdaki tüm kaynaklar için Kullanılabilirlik durumlarını alır |
> | Microsoft. resources/dağıtımlar/* | Dağıtım oluşturma ve yönetme |
> | Microsoft. resources/abonelikler/resourceGroups/Read | Kaynak gruplarını alır veya listeler. |
> | Microsoft. support/* | Destek bileti oluşturma ve güncelleştirme |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage DNS zones and record sets in Azure DNS, but does not let you control who has access to them.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/befefa01-2a29-4197-83a8-272ff33ce314",
  "name": "befefa01-2a29-4197-83a8-272ff33ce314",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Network/dnsZones/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "DNS Zone Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="network-contributor"></a>Ağ Katılımcısı

Ağları yönetmenizi sağlar ancak onlara yönelik erişimleri vermez.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. Authorization/*/Read | Rolleri ve rol atamalarını oku |
> | Microsoft. Insights/alertRules/* | Klasik ölçüm uyarısı oluşturma ve yönetme |
> | Microsoft. Network/* | Ağ oluşturma ve yönetme |
> | Microsoft. ResourceHealth/kullanılabilirlik Bilitydurumlar/okuma | Belirtilen kapsamdaki tüm kaynaklar için Kullanılabilirlik durumlarını alır |
> | Microsoft. resources/dağıtımlar/* | Dağıtım oluşturma ve yönetme |
> | Microsoft. resources/abonelikler/resourceGroups/Read | Kaynak gruplarını alır veya listeler. |
> | Microsoft. support/* | Destek bileti oluşturma ve güncelleştirme |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage networks, but not access to them.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/4d97b98b-1d4f-4787-a291-c67834d212e7",
  "name": "4d97b98b-1d4f-4787-a291-c67834d212e7",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Network/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Network Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="traffic-manager-contributor"></a>Katkıda bulunan Traffic Manager

Traffic Manager profillerini yönetmenizi sağlar, ancak bunlara kimlerin erişebileceğini denetlemenize izin vermez.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. Authorization/*/Read | Rolleri ve rol atamalarını oku |
> | Microsoft. Insights/alertRules/* | Klasik ölçüm uyarısı oluşturma ve yönetme |
> | Microsoft. Network/trafficManagerProfiles/* |  |
> | Microsoft. ResourceHealth/kullanılabilirlik Bilitydurumlar/okuma | Belirtilen kapsamdaki tüm kaynaklar için Kullanılabilirlik durumlarını alır |
> | Microsoft. resources/dağıtımlar/* | Dağıtım oluşturma ve yönetme |
> | Microsoft. resources/abonelikler/resourceGroups/Read | Kaynak gruplarını alır veya listeler. |
> | Microsoft. support/* | Destek bileti oluşturma ve güncelleştirme |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage Traffic Manager profiles, but does not let you control who has access to them.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/a4b10055-b0c7-44c2-b00f-c7b5b3550cf7",
  "name": "a4b10055-b0c7-44c2-b00f-c7b5b3550cf7",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Network/trafficManagerProfiles/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Traffic Manager Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

## <a name="storage"></a>Depolama


### <a name="avere-contributor"></a>Avere Katılımcısı

, Bir avere vFXT kümesi oluşturabilir ve yönetebilir.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. Authorization/*/Read | Rolleri ve rol atamalarını oku |
> | Microsoft. COMPUTE/*/Read |  |
> | Microsoft. COMPUTE/kullanılabilirliği Bilitysets/* |  |
> | Microsoft. COMPUTE/virtualMachines/* |  |
> | Microsoft. COMPUTE/Disks/* |  |
> | Microsoft. Network/*/Read |  |
> | Microsoft. Network/NetworkInterfaces/* |  |
> | Microsoft. Network/virtualNetworks/Read | Sanal ağ tanımını al |
> | Microsoft. Network/virtualNetworks/alt ağlar/okuma | Bir sanal ağ alt ağ tanımını alır |
> | Microsoft. Network/virtualNetworks/alt ağlar/JOIN/Action | Bir sanal ağı birleştirir. Alertable değil. |
> | Microsoft. Network/virtualNetworks/alt ağlar/Jodavetli Aserviceendpoint/Action | Depolama hesabı veya SQL veritabanı gibi kaynağı bir alt ağa birleştirir. Alertable değil. |
> | Microsoft. Network/networkSecurityGroups/JOIN/Action | Bir ağ güvenlik grubuna katılır. Alertable değil. |
> | Microsoft. resources/dağıtımlar/* | Dağıtım oluşturma ve yönetme |
> | Microsoft. Insights/alertRules/* | Klasik ölçüm uyarısı oluşturma ve yönetme |
> | Microsoft. resources/abonelikler/resourceGroups/Read | Kaynak gruplarını alır veya listeler. |
> | Microsoft. Storage/*/Read |  |
> | Microsoft. Storage/storageAccounts/* | Depolama hesapları oluşturma ve yönetme |
> | Microsoft. support/* | Destek bileti oluşturma ve güncelleştirme |
> | Microsoft. resources/abonelikler/resourceGroups/kaynaklar/okuma | Kaynak grubunun kaynaklarını alır. |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | Microsoft. Storage/storageAccounts/blobServices/kapsayıcılar/Bloblar/Sil | Bir blobu silmenin sonucunu döndürür |
> | Microsoft. Storage/storageAccounts/blobServices/kapsayıcılar/blob/okuma | Blob veya Blobların listesini döndürür |
> | Microsoft. Storage/storageAccounts/blobServices/kapsayıcılar/Bloblar/yazma | Blob yazma sonucunu döndürür |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can create and manage an Avere vFXT cluster.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/4f8fab4f-1852-4a58-a46a-8eaf358af14a",
  "name": "4f8fab4f-1852-4a58-a46a-8eaf358af14a",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Compute/*/read",
        "Microsoft.Compute/availabilitySets/*",
        "Microsoft.Compute/virtualMachines/*",
        "Microsoft.Compute/disks/*",
        "Microsoft.Network/*/read",
        "Microsoft.Network/networkInterfaces/*",
        "Microsoft.Network/virtualNetworks/read",
        "Microsoft.Network/virtualNetworks/subnets/read",
        "Microsoft.Network/virtualNetworks/subnets/join/action",
        "Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/action",
        "Microsoft.Network/networkSecurityGroups/join/action",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Storage/*/read",
        "Microsoft.Storage/storageAccounts/*",
        "Microsoft.Support/*",
        "Microsoft.Resources/subscriptions/resourceGroups/resources/read"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/delete",
        "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read",
        "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/write"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Avere Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="avere-operator"></a>Avere Işleci

Kümeyi yönetmek için avere vFXT kümesi tarafından kullanılır

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. COMPUTE/virtualMachines/okuma | Bir sanal makinenin özelliklerini al |
> | Microsoft. Network/NetworkInterfaces/Read | Bir ağ arabirimi tanımını alır.  |
> | Microsoft. Network/NetworkInterfaces/Write | Ağ arabirimi oluşturur veya var olan bir ağ arabirimini güncelleştirir.  |
> | Microsoft. Network/virtualNetworks/Read | Sanal ağ tanımını al |
> | Microsoft. Network/virtualNetworks/alt ağlar/okuma | Bir sanal ağ alt ağ tanımını alır |
> | Microsoft. Network/virtualNetworks/alt ağlar/JOIN/Action | Bir sanal ağı birleştirir. Alertable değil. |
> | Microsoft. Network/networkSecurityGroups/JOIN/Action | Bir ağ güvenlik grubuna katılır. Alertable değil. |
> | Microsoft. resources/abonelikler/resourceGroups/Read | Kaynak gruplarını alır veya listeler. |
> | Microsoft. Storage/storageAccounts/blobServices/kapsayıcılar/Delete | Bir kapsayıcıyı silmenin sonucunu döndürür |
> | Microsoft. Storage/storageAccounts/blobServices/kapsayıcılar/okuma | Kapsayıcıların listesini döndürür |
> | Microsoft. Storage/storageAccounts/blobServices/kapsayıcılar/Write | Put blob kapsayıcısının sonucunu döndürür |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | Microsoft. Storage/storageAccounts/blobServices/kapsayıcılar/Bloblar/Sil | Bir blobu silmenin sonucunu döndürür |
> | Microsoft. Storage/storageAccounts/blobServices/kapsayıcılar/blob/okuma | Blob veya Blobların listesini döndürür |
> | Microsoft. Storage/storageAccounts/blobServices/kapsayıcılar/Bloblar/yazma | Blob yazma sonucunu döndürür |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Used by the Avere vFXT cluster to manage the cluster",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/c025889f-8102-4ebf-b32c-fc0c6f0c6bd9",
  "name": "c025889f-8102-4ebf-b32c-fc0c6f0c6bd9",
  "permissions": [
    {
      "actions": [
        "Microsoft.Compute/virtualMachines/read",
        "Microsoft.Network/networkInterfaces/read",
        "Microsoft.Network/networkInterfaces/write",
        "Microsoft.Network/virtualNetworks/read",
        "Microsoft.Network/virtualNetworks/subnets/read",
        "Microsoft.Network/virtualNetworks/subnets/join/action",
        "Microsoft.Network/networkSecurityGroups/join/action",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Storage/storageAccounts/blobServices/containers/delete",
        "Microsoft.Storage/storageAccounts/blobServices/containers/read",
        "Microsoft.Storage/storageAccounts/blobServices/containers/write"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/delete",
        "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read",
        "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/write"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Avere Operator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="backup-contributor"></a>Yedek Katılımcısı

Yedekleme hizmetini yönetmenizi sağlar, ancak kasaların oluşturamaz ve başkalarına erişim izni verebilir

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. Authorization/*/Read | Rolleri ve rol atamalarını oku |
> | Microsoft. Network/virtualNetworks/Read | Sanal ağ tanımını al |
> | Microsoft. RecoveryServices/konumlar/* |  |
> | Microsoft. RecoveryServices/Vaults/Backupyapılar/operationResults/* | Yedekleme yönetiminde işlemin sonuçlarını yönetme |
> | Microsoft. RecoveryServices/Vaults/Backupdokuları/protectionContainers/* | Kurtarma Hizmetleri kasasının yedekleme dokuları içinde yedekleme kapsayıcıları oluşturma ve yönetme |
> | Microsoft. RecoveryServices/Vaults/Backupdokuları/refreshContainers/eylem | Kapsayıcı listesini yeniler |
> | Microsoft. RecoveryServices/Vaults/backupJobs/* | Yedekleme işleri oluşturma ve yönetme |
> | Microsoft. RecoveryServices/Vaults/backupJobsExport/eylem | Işleri dışarı aktar |
> | Microsoft. RecoveryServices/Vaults/backupOperationResults/* | Yedekleme yönetimi işlemlerinin sonuçlarını oluşturma ve yönetme |
> | Microsoft. RecoveryServices/Vaults/backupPolicies/* | Yedekleme ilkeleri oluşturma ve yönetme |
> | Microsoft. RecoveryServices/Vaults/Backupkorunabilir/* | Yedeklenebilir öğeleri oluşturma ve yönetme |
> | Microsoft. RecoveryServices/Vaults/Backupkorunabilir/* | Yedeklenen öğeleri oluşturma ve yönetme |
> | Microsoft. RecoveryServices/Vaults/backupProtectionContainers/* | Yedekleme öğelerini tutan kapsayıcılar oluşturma ve yönetme |
> | Microsoft. RecoveryServices/Vaults/backupSecurityPIN/* |  |
> | Microsoft. RecoveryServices/Vaults/backupUsageSummaries/Read | Kurtarma Hizmetleri için korumalı öğeler ve korumalı sunucular için özetler döndürür. |
> | Microsoft. RecoveryServices/Kasaults/sertifikalar/* | Kurtarma Hizmetleri kasasında yedekleme ile ilgili sertifikalar oluşturma ve yönetme |
> | Microsoft. RecoveryServices/Kasaults/Extendedınformation/* | Kasa ile ilgili genişletilmiş bilgileri oluşturma ve yönetme |
> | Microsoft. RecoveryServices/Kasaults/monitoringAlerts/okuma | Kurtarma Hizmetleri Kasası için uyarıları alır. |
> | Microsoft. RecoveryServices/Vaults/monitoringConfigurations/* |  |
> | Microsoft. RecoveryServices/Vaults/okuma | Kasayı al işlemi, ' kasa ' türündeki Azure kaynağını temsil eden bir nesneyi alır |
> | Microsoft. RecoveryServices/Vaults/registeredIdentities/* | Kayıtlı kimlikler oluşturma ve yönetme |
> | Microsoft. RecoveryServices/Kasaults/kullanımlar/* | Kurtarma Hizmetleri kasasının kullanımını oluşturma ve yönetme |
> | Microsoft. resources/dağıtımlar/* | Dağıtım oluşturma ve yönetme |
> | Microsoft. resources/abonelikler/resourceGroups/Read | Kaynak gruplarını alır veya listeler. |
> | Microsoft. Storage/storageAccounts/Read | Depolama hesaplarının listesini döndürür veya belirtilen depolama hesabının özelliklerini alır. |
> | Microsoft. RecoveryServices/Vaults/backupstorageconfig/* |  |
> | Microsoft. RecoveryServices/Vaults/backupconfig/* |  |
> | Microsoft. RecoveryServices/Vaults/backupValidateOperation/Action | Korumalı öğe üzerinde Işlemi doğrula |
> | Microsoft. RecoveryServices/Vaults/Write | Kasa oluşturma işlemi, ' kasa ' türünde bir Azure kaynağı oluşturur |
> | Microsoft. RecoveryServices/Vaults/backupOperations/Read | Kurtarma Hizmetleri Kasası için yedekleme Işlemi durumunu döndürür. |
> | Microsoft. RecoveryServices/Vaults/backupEngines/Read | Kasaya kayıtlı tüm yedekleme yönetimi sunucularını döndürür. |
> | Microsoft. RecoveryServices/Vaults/Backupdokuları/Backupprotectionamaç/* |  |
> | Microsoft. RecoveryServices/Vaults/Backupdokuları/korunabilir | Tüm korunabilir kapsayıcıları al |
> | Microsoft. RecoveryServices/konumlar/backupStatus/Action | Kurtarma Hizmetleri kasaları için yedekleme durumunu denetle |
> | Microsoft. RecoveryServices/Locations/backupPreValidateProtection/Action |  |
> | Microsoft. RecoveryServices/Locations/backupValidateFeatures/Action | Özellikleri doğrulama |
> | Microsoft. RecoveryServices/Vaults/monitoringAlerts/Write | Uyarıyı çözer. |
> | Microsoft. RecoveryServices/işlemler/okuma | İşlem, bir kaynak sağlayıcısı için Işlem listesini döndürür |
> | Microsoft. RecoveryServices/konumlar/operationStatus/Read | Belirli bir Işlem için Işlem durumunu alır |
> | Microsoft. RecoveryServices/Vaults/Backupprotectionhedefleri/okuma | Tüm yedekleme koruma amaçlarını Listele |
> | Microsoft. support/* | Destek bileti oluşturma ve güncelleştirme |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage backup service,but can't create vaults and give access to others",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/5e467623-bb1f-42f4-a55d-6e525e11384b",
  "name": "5e467623-bb1f-42f4-a55d-6e525e11384b",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Network/virtualNetworks/read",
        "Microsoft.RecoveryServices/locations/*",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/operationResults/*",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/*",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/refreshContainers/action",
        "Microsoft.RecoveryServices/Vaults/backupJobs/*",
        "Microsoft.RecoveryServices/Vaults/backupJobsExport/action",
        "Microsoft.RecoveryServices/Vaults/backupOperationResults/*",
        "Microsoft.RecoveryServices/Vaults/backupPolicies/*",
        "Microsoft.RecoveryServices/Vaults/backupProtectableItems/*",
        "Microsoft.RecoveryServices/Vaults/backupProtectedItems/*",
        "Microsoft.RecoveryServices/Vaults/backupProtectionContainers/*",
        "Microsoft.RecoveryServices/Vaults/backupSecurityPIN/*",
        "Microsoft.RecoveryServices/Vaults/backupUsageSummaries/read",
        "Microsoft.RecoveryServices/Vaults/certificates/*",
        "Microsoft.RecoveryServices/Vaults/extendedInformation/*",
        "Microsoft.RecoveryServices/Vaults/monitoringAlerts/read",
        "Microsoft.RecoveryServices/Vaults/monitoringConfigurations/*",
        "Microsoft.RecoveryServices/Vaults/read",
        "Microsoft.RecoveryServices/Vaults/registeredIdentities/*",
        "Microsoft.RecoveryServices/Vaults/usages/*",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Storage/storageAccounts/read",
        "Microsoft.RecoveryServices/Vaults/backupstorageconfig/*",
        "Microsoft.RecoveryServices/Vaults/backupconfig/*",
        "Microsoft.RecoveryServices/Vaults/backupValidateOperation/action",
        "Microsoft.RecoveryServices/Vaults/write",
        "Microsoft.RecoveryServices/Vaults/backupOperations/read",
        "Microsoft.RecoveryServices/Vaults/backupEngines/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/backupProtectionIntent/*",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectableContainers/read",
        "Microsoft.RecoveryServices/locations/backupStatus/action",
        "Microsoft.RecoveryServices/locations/backupPreValidateProtection/action",
        "Microsoft.RecoveryServices/locations/backupValidateFeatures/action",
        "Microsoft.RecoveryServices/Vaults/monitoringAlerts/write",
        "Microsoft.RecoveryServices/operations/read",
        "Microsoft.RecoveryServices/locations/operationStatus/read",
        "Microsoft.RecoveryServices/Vaults/backupProtectionIntents/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Backup Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="backup-operator"></a>Yedekleme Işletmeni

Yedekleme kaldırma, kasa oluşturma ve başkalarına erişim verme dışında yedekleme hizmetlerini yönetmenizi sağlar

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. Authorization/*/Read | Rolleri ve rol atamalarını oku |
> | Microsoft. Network/virtualNetworks/Read | Sanal ağ tanımını al |
> | Microsoft. RecoveryServices/Vaults/Backupyapılar/operationResults/Read | İşlemin durumunu döndürür |
> | Microsoft. RecoveryServices/Vaults/Backupyapılar/protectionContainers/operationResults/Read | Koruma kapsayıcısında gerçekleştirilen Işlemin sonucunu alır. |
> | Microsoft. RecoveryServices/Vaults/Backupyapılar/protectionContainers/korunabilir/yedekleme/eylem | Korumalı öğe için yedekleme gerçekleştirir. |
> | Microsoft. RecoveryServices/Vaults/Backupdokuları/protectionContainers/korunabilir/operationResults/Read | Korumalı öğeler üzerinde gerçekleştirilen Işlemin sonucunu alır. |
> | Microsoft. RecoveryServices/Vaults/Backupdokuları/protectionContainers/korunabilir/operationsStatus/Read | Korumalı öğeler üzerinde gerçekleştirilen Işlemin durumunu döndürür. |
> | Microsoft. RecoveryServices/Vaults/Backupdokuları/protectionContainers/korunabilir/Read | Korumalı öğenin nesne ayrıntılarını döndürür |
> | Microsoft. RecoveryServices/Vaults/Backupdokuları/protectionContainers/korunabilir/provisionInstantItemRecovery/eylem | Korumalı öğe için anında öğe kurtarma sağla |
> | Microsoft. RecoveryServices/Vaults/Backupdokuları/protectionContainers/korunabilir/kurtarma noktaları/okuma | Korumalı öğeler için kurtarma noktalarını alın. |
> | Microsoft. RecoveryServices/Vaults/Backupdokuları/protectionContainers/korunabilir/kurtarma noktaları/geri yükleme/eylem | Korumalı öğeler için kurtarma noktalarını geri yükleyin. |
> | Microsoft. RecoveryServices/Vaults/Backupdokuları/protectionContainers/korunabilir/revokeInstantItemRecovery/eylem | Korumalı öğe için anında öğe kurtarmayı iptal et |
> | Microsoft. RecoveryServices/Vaults/Backupdokuları/protectionContainers/korunabilir/yazma | Yedekleme korumalı öğesi oluştur |
> | Microsoft. RecoveryServices/Vaults/Backupdokuları/protectionContainers/okuma | Tüm kayıtlı kapsayıcıları döndürür |
> | Microsoft. RecoveryServices/Vaults/Backupdokuları/refreshContainers/eylem | Kapsayıcı listesini yeniler |
> | Microsoft. RecoveryServices/Vaults/backupJobs/* | Yedekleme işleri oluşturma ve yönetme |
> | Microsoft. RecoveryServices/Vaults/backupJobsExport/eylem | Işleri dışarı aktar |
> | Microsoft. RecoveryServices/Vaults/backupOperationResults/* | Yedekleme yönetimi işlemlerinin sonuçlarını oluşturma ve yönetme |
> | Microsoft. RecoveryServices/Vaults/backupPolicies/operationResults/Read | Ilke Işleminin sonuçlarını alın. |
> | Microsoft. RecoveryServices/Vaults/backupPolicies/Read | Tüm koruma Ilkelerini döndürür |
> | Microsoft. RecoveryServices/Vaults/Backupkorunabilir/* | Yedeklenebilir öğeleri oluşturma ve yönetme |
> | Microsoft. RecoveryServices/Vaults/Backupkorunabilir/Read | Tüm korumalı öğelerin listesini döndürür. |
> | Microsoft. RecoveryServices/Vaults/backupProtectionContainers/Read | Aboneliğe ait tüm kapsayıcıları döndürür |
> | Microsoft. RecoveryServices/Vaults/backupUsageSummaries/Read | Kurtarma Hizmetleri için korumalı öğeler ve korumalı sunucular için özetler döndürür. |
> | Microsoft. RecoveryServices/Vaults/sertifikalar/yazma | Kaynak sertifikası güncelleştirme işlemi kaynak/kasa kimlik bilgisi sertifikasını güncelleştirir. |
> | Microsoft. RecoveryServices/Kasaults/Extendedınformation/Read | Genişletilmiş bilgileri al işlemi, bir nesnenin,? Kasası türünde Azure kaynağını temsil eden genişletilmiş bilgilerini alır mi? |
> | Microsoft. RecoveryServices/Kasaults/Extendedınformation/Write | Genişletilmiş bilgileri al işlemi, bir nesnenin,? Kasası türünde Azure kaynağını temsil eden genişletilmiş bilgilerini alır mi? |
> | Microsoft. RecoveryServices/Kasaults/monitoringAlerts/okuma | Kurtarma Hizmetleri Kasası için uyarıları alır. |
> | Microsoft. RecoveryServices/Vaults/monitoringConfigurations/* |  |
> | Microsoft. RecoveryServices/Vaults/okuma | Kasayı al işlemi, ' kasa ' türündeki Azure kaynağını temsil eden bir nesneyi alır |
> | Microsoft. RecoveryServices/Vaults/registeredIdentities/operationResults/Read | Işlem sonuçlarını al işlemi, zaman uyumsuz olarak gönderilen işlemin işlem durumunu ve sonucunu almak için kullanılabilir |
> | Microsoft. RecoveryServices/Vaults/registeredIdentities/Read | Kapsayıcıları al işlemi, bir kaynak için kayıtlı olan kapsayıcıları almak için kullanılabilir. |
> | Microsoft. RecoveryServices/Vaults/registeredIdentities/Write | Hizmet kapsayıcısını Kaydet işlemi, bir kapsayıcıyı kurtarma hizmeti 'ne kaydetmek için kullanılabilir. |
> | Microsoft. RecoveryServices/Kasaults/kullanımlar/okuma | Bir kurtarma hizmetleri Kasası için kullanım ayrıntılarını döndürür. |
> | Microsoft. resources/dağıtımlar/* | Dağıtım oluşturma ve yönetme |
> | Microsoft. resources/abonelikler/resourceGroups/Read | Kaynak gruplarını alır veya listeler. |
> | Microsoft. Storage/storageAccounts/Read | Depolama hesaplarının listesini döndürür veya belirtilen depolama hesabının özelliklerini alır. |
> | Microsoft. RecoveryServices/Vaults/backupstorageconfig/* |  |
> | Microsoft. RecoveryServices/Vaults/backupValidateOperation/Action | Korumalı öğe üzerinde Işlemi doğrula |
> | Microsoft. RecoveryServices/Vaults/backupOperations/Read | Kurtarma Hizmetleri Kasası için yedekleme Işlemi durumunu döndürür. |
> | Microsoft. RecoveryServices/Vaults/backupPolicies/işlemler/okuma | Ilke Işleminin durumunu alın. |
> | Microsoft. RecoveryServices/Vaults/Backupyapılar/protectionContainers/Write | Kayıtlı bir kapsayıcı oluşturur |
> | Microsoft. RecoveryServices/Vaults/Backupyapılar/protectionContainers/sorgu/eylem | Bir kapsayıcı içindeki iş yükleri için sorgulama yap |
> | Microsoft. RecoveryServices/Vaults/backupEngines/Read | Kasaya kayıtlı tüm yedekleme yönetimi sunucularını döndürür. |
> | Microsoft. RecoveryServices/Vaults/Backupdokuları/Backupprotectionamacını/Write | Yedekleme koruma hedefi oluşturma |
> | Microsoft. RecoveryServices/Vaults/Backupdokuların/Backupprotectionamacını/Read | Yedekleme koruması hedefi al |
> | Microsoft. RecoveryServices/Vaults/Backupdokuları/korunabilir | Tüm korunabilir kapsayıcıları al |
> | Microsoft. RecoveryServices/Vaults/Backupyapılar/protectionContainers/öğeler/okuma | Bir kapsayıcıdaki tüm öğeleri Al |
> | Microsoft. RecoveryServices/konumlar/backupStatus/Action | Kurtarma Hizmetleri kasaları için yedekleme durumunu denetle |
> | Microsoft. RecoveryServices/Locations/backupPreValidateProtection/Action |  |
> | Microsoft. RecoveryServices/Locations/backupValidateFeatures/Action | Özellikleri doğrulama |
> | Microsoft. RecoveryServices/Vaults/monitoringAlerts/Write | Uyarıyı çözer. |
> | Microsoft. RecoveryServices/işlemler/okuma | İşlem, bir kaynak sağlayıcısı için Işlem listesini döndürür |
> | Microsoft. RecoveryServices/konumlar/operationStatus/Read | Belirli bir Işlem için Işlem durumunu alır |
> | Microsoft. RecoveryServices/Vaults/Backupprotectionhedefleri/okuma | Tüm yedekleme koruma amaçlarını Listele |
> | Microsoft. support/* | Destek bileti oluşturma ve güncelleştirme |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage backup services, except removal of backup, vault creation and giving access to others",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/00c29273-979b-4161-815c-10b084fb9324",
  "name": "00c29273-979b-4161-815c-10b084fb9324",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Network/virtualNetworks/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/operationResults/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/operationResults/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/backup/action",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/operationResults/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/operationsStatus/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/provisionInstantItemRecovery/action",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/restore/action",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/revokeInstantItemRecovery/action",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/write",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/refreshContainers/action",
        "Microsoft.RecoveryServices/Vaults/backupJobs/*",
        "Microsoft.RecoveryServices/Vaults/backupJobsExport/action",
        "Microsoft.RecoveryServices/Vaults/backupOperationResults/*",
        "Microsoft.RecoveryServices/Vaults/backupPolicies/operationResults/read",
        "Microsoft.RecoveryServices/Vaults/backupPolicies/read",
        "Microsoft.RecoveryServices/Vaults/backupProtectableItems/*",
        "Microsoft.RecoveryServices/Vaults/backupProtectedItems/read",
        "Microsoft.RecoveryServices/Vaults/backupProtectionContainers/read",
        "Microsoft.RecoveryServices/Vaults/backupUsageSummaries/read",
        "Microsoft.RecoveryServices/Vaults/certificates/write",
        "Microsoft.RecoveryServices/Vaults/extendedInformation/read",
        "Microsoft.RecoveryServices/Vaults/extendedInformation/write",
        "Microsoft.RecoveryServices/Vaults/monitoringAlerts/read",
        "Microsoft.RecoveryServices/Vaults/monitoringConfigurations/*",
        "Microsoft.RecoveryServices/Vaults/read",
        "Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read",
        "Microsoft.RecoveryServices/Vaults/registeredIdentities/read",
        "Microsoft.RecoveryServices/Vaults/registeredIdentities/write",
        "Microsoft.RecoveryServices/Vaults/usages/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Storage/storageAccounts/read",
        "Microsoft.RecoveryServices/Vaults/backupstorageconfig/*",
        "Microsoft.RecoveryServices/Vaults/backupValidateOperation/action",
        "Microsoft.RecoveryServices/Vaults/backupOperations/read",
        "Microsoft.RecoveryServices/Vaults/backupPolicies/operations/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/write",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/inquire/action",
        "Microsoft.RecoveryServices/Vaults/backupEngines/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/backupProtectionIntent/write",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/backupProtectionIntent/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectableContainers/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/items/read",
        "Microsoft.RecoveryServices/locations/backupStatus/action",
        "Microsoft.RecoveryServices/locations/backupPreValidateProtection/action",
        "Microsoft.RecoveryServices/locations/backupValidateFeatures/action",
        "Microsoft.RecoveryServices/Vaults/monitoringAlerts/write",
        "Microsoft.RecoveryServices/operations/read",
        "Microsoft.RecoveryServices/locations/operationStatus/read",
        "Microsoft.RecoveryServices/Vaults/backupProtectionIntents/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Backup Operator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="backup-reader"></a>Yedekleme okuyucusu

Yedekleme hizmetlerini görüntüleyebilir, ancak değişiklik yapamaz

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. Authorization/*/Read | Rolleri ve rol atamalarını oku |
> | Microsoft. RecoveryServices/konumlar/allocatedStamp/Read | GetAllocatedStamp, hizmet tarafından kullanılan iç işlemdir |
> | Microsoft. RecoveryServices/Vaults/Backupyapılar/operationResults/Read | İşlemin durumunu döndürür |
> | Microsoft. RecoveryServices/Vaults/Backupyapılar/protectionContainers/operationResults/Read | Koruma kapsayıcısında gerçekleştirilen Işlemin sonucunu alır. |
> | Microsoft. RecoveryServices/Vaults/Backupdokuları/protectionContainers/korunabilir/operationResults/Read | Korumalı öğeler üzerinde gerçekleştirilen Işlemin sonucunu alır. |
> | Microsoft. RecoveryServices/Vaults/Backupdokuları/protectionContainers/korunabilir/operationsStatus/Read | Korumalı öğeler üzerinde gerçekleştirilen Işlemin durumunu döndürür. |
> | Microsoft. RecoveryServices/Vaults/Backupdokuları/protectionContainers/korunabilir/Read | Korumalı öğenin nesne ayrıntılarını döndürür |
> | Microsoft. RecoveryServices/Vaults/Backupdokuları/protectionContainers/korunabilir/kurtarma noktaları/okuma | Korumalı öğeler için kurtarma noktalarını alın. |
> | Microsoft. RecoveryServices/Vaults/Backupdokuları/protectionContainers/okuma | Tüm kayıtlı kapsayıcıları döndürür |
> | Microsoft. RecoveryServices/Vaults/backupJobs/operationResults/Read | Iş Işleminin sonucunu döndürür. |
> | Microsoft. RecoveryServices/Vaults/backupJobs/Read | Tüm Iş nesnelerini döndürür |
> | Microsoft. RecoveryServices/Vaults/backupJobsExport/eylem | Işleri dışarı aktar |
> | Microsoft. RecoveryServices/Vaults/backupOperationResults/Read | Kurtarma Hizmetleri Kasası için yedekleme Işlemi sonucunu döndürür. |
> | Microsoft. RecoveryServices/Vaults/backupPolicies/operationResults/Read | Ilke Işleminin sonuçlarını alın. |
> | Microsoft. RecoveryServices/Vaults/backupPolicies/Read | Tüm koruma Ilkelerini döndürür |
> | Microsoft. RecoveryServices/Vaults/Backupkorunabilir/Read | Tüm korumalı öğelerin listesini döndürür. |
> | Microsoft. RecoveryServices/Vaults/backupProtectionContainers/Read | Aboneliğe ait tüm kapsayıcıları döndürür |
> | Microsoft. RecoveryServices/Vaults/backupUsageSummaries/Read | Kurtarma Hizmetleri için korumalı öğeler ve korumalı sunucular için özetler döndürür. |
> | Microsoft. RecoveryServices/Kasaults/Extendedınformation/Read | Genişletilmiş bilgileri al işlemi, bir nesnenin,? Kasası türünde Azure kaynağını temsil eden genişletilmiş bilgilerini alır mi? |
> | Microsoft. RecoveryServices/Kasaults/monitoringAlerts/okuma | Kurtarma Hizmetleri Kasası için uyarıları alır. |
> | Microsoft. RecoveryServices/Vaults/okuma | Kasayı al işlemi, ' kasa ' türündeki Azure kaynağını temsil eden bir nesneyi alır |
> | Microsoft. RecoveryServices/Vaults/registeredIdentities/operationResults/Read | Işlem sonuçlarını al işlemi, zaman uyumsuz olarak gönderilen işlemin işlem durumunu ve sonucunu almak için kullanılabilir |
> | Microsoft. RecoveryServices/Vaults/registeredIdentities/Read | Kapsayıcıları al işlemi, bir kaynak için kayıtlı olan kapsayıcıları almak için kullanılabilir. |
> | Microsoft. RecoveryServices/Vaults/backupstorageconfig/Read | Kurtarma Hizmetleri Kasası için depolama yapılandırmasını döndürür. |
> | Microsoft. RecoveryServices/Vaults/backupconfig/Read | Kurtarma Hizmetleri Kasası için yapılandırmayı döndürür. |
> | Microsoft. RecoveryServices/Vaults/backupOperations/Read | Kurtarma Hizmetleri Kasası için yedekleme Işlemi durumunu döndürür. |
> | Microsoft. RecoveryServices/Vaults/backupPolicies/işlemler/okuma | Ilke Işleminin durumunu alın. |
> | Microsoft. RecoveryServices/Vaults/backupEngines/Read | Kasaya kayıtlı tüm yedekleme yönetimi sunucularını döndürür. |
> | Microsoft. RecoveryServices/Vaults/Backupdokuların/Backupprotectionamacını/Read | Yedekleme koruması hedefi al |
> | Microsoft. RecoveryServices/Vaults/Backupyapılar/protectionContainers/öğeler/okuma | Bir kapsayıcıdaki tüm öğeleri Al |
> | Microsoft. RecoveryServices/konumlar/backupStatus/Action | Kurtarma Hizmetleri kasaları için yedekleme durumunu denetle |
> | Microsoft. RecoveryServices/Vaults/monitoringConfigurations/* |  |
> | Microsoft. RecoveryServices/Vaults/monitoringAlerts/Write | Uyarıyı çözer. |
> | Microsoft. RecoveryServices/işlemler/okuma | İşlem, bir kaynak sağlayıcısı için Işlem listesini döndürür |
> | Microsoft. RecoveryServices/konumlar/operationStatus/Read | Belirli bir Işlem için Işlem durumunu alır |
> | Microsoft. RecoveryServices/Vaults/Backupprotectionhedefleri/okuma | Tüm yedekleme koruma amaçlarını Listele |
> | Microsoft. RecoveryServices/Kasaults/kullanımlar/okuma | Bir kurtarma hizmetleri Kasası için kullanım ayrıntılarını döndürür. |
> | Microsoft. RecoveryServices/Locations/backupValidateFeatures/Action | Özellikleri doğrulama |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can view backup services, but can't make changes",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/a795c7a0-d4a2-40c1-ae25-d81f01202912",
  "name": "a795c7a0-d4a2-40c1-ae25-d81f01202912",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.RecoveryServices/locations/allocatedStamp/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/operationResults/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/operationResults/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/operationResults/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/operationsStatus/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/read",
        "Microsoft.RecoveryServices/Vaults/backupJobs/operationResults/read",
        "Microsoft.RecoveryServices/Vaults/backupJobs/read",
        "Microsoft.RecoveryServices/Vaults/backupJobsExport/action",
        "Microsoft.RecoveryServices/Vaults/backupOperationResults/read",
        "Microsoft.RecoveryServices/Vaults/backupPolicies/operationResults/read",
        "Microsoft.RecoveryServices/Vaults/backupPolicies/read",
        "Microsoft.RecoveryServices/Vaults/backupProtectedItems/read",
        "Microsoft.RecoveryServices/Vaults/backupProtectionContainers/read",
        "Microsoft.RecoveryServices/Vaults/backupUsageSummaries/read",
        "Microsoft.RecoveryServices/Vaults/extendedInformation/read",
        "Microsoft.RecoveryServices/Vaults/monitoringAlerts/read",
        "Microsoft.RecoveryServices/Vaults/read",
        "Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read",
        "Microsoft.RecoveryServices/Vaults/registeredIdentities/read",
        "Microsoft.RecoveryServices/Vaults/backupstorageconfig/read",
        "Microsoft.RecoveryServices/Vaults/backupconfig/read",
        "Microsoft.RecoveryServices/Vaults/backupOperations/read",
        "Microsoft.RecoveryServices/Vaults/backupPolicies/operations/read",
        "Microsoft.RecoveryServices/Vaults/backupEngines/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/backupProtectionIntent/read",
        "Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/items/read",
        "Microsoft.RecoveryServices/locations/backupStatus/action",
        "Microsoft.RecoveryServices/Vaults/monitoringConfigurations/*",
        "Microsoft.RecoveryServices/Vaults/monitoringAlerts/write",
        "Microsoft.RecoveryServices/operations/read",
        "Microsoft.RecoveryServices/locations/operationStatus/read",
        "Microsoft.RecoveryServices/Vaults/backupProtectionIntents/read",
        "Microsoft.RecoveryServices/Vaults/usages/read",
        "Microsoft.RecoveryServices/locations/backupValidateFeatures/action"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Backup Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="classic-storage-account-contributor"></a>Klasik depolama hesabı Katılımcısı

Klasik depolama hesaplarını yönetmenize izin verir, ancak bunlara erişimi kalmaz.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. Authorization/*/Read | Rolleri ve rol atamalarını oku |
> | Microsoft. ClassicStorage/storageAccounts/* | Depolama hesapları oluşturma ve yönetme |
> | Microsoft. Insights/alertRules/* | Klasik ölçüm uyarısı oluşturma ve yönetme |
> | Microsoft. ResourceHealth/kullanılabilirlik Bilitydurumlar/okuma | Belirtilen kapsamdaki tüm kaynaklar için Kullanılabilirlik durumlarını alır |
> | Microsoft. resources/dağıtımlar/* | Dağıtım oluşturma ve yönetme |
> | Microsoft. resources/abonelikler/resourceGroups/Read | Kaynak gruplarını alır veya listeler. |
> | Microsoft. support/* | Destek bileti oluşturma ve güncelleştirme |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage classic storage accounts, but not access to them.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/86e8f5dc-a6e9-4c67-9d15-de283e8eac25",
  "name": "86e8f5dc-a6e9-4c67-9d15-de283e8eac25",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.ClassicStorage/storageAccounts/*",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Classic Storage Account Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="classic-storage-account-key-operator-service-role"></a>Klasik depolama hesabı anahtar operatörü hizmet rolü

Klasik depolama hesabı anahtar Işleçleri klasik depolama hesaplarında anahtarları listeleme ve yeniden oluşturma izni verilir

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. ClassicStorage/storageAccounts/ListKeys/Action | Depolama hesaplarının erişim anahtarlarını listeler. |
> | Microsoft. ClassicStorage/storageAccounts/RegenerateKey/Action | Depolama hesabı için mevcut erişim anahtarlarını yeniden oluşturur. |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Classic Storage Account Key Operators are allowed to list and regenerate keys on Classic Storage Accounts",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/985d6b00-f706-48f5-a6fe-d0ca12fb668d",
  "name": "985d6b00-f706-48f5-a6fe-d0ca12fb668d",
  "permissions": [
    {
      "actions": [
        "Microsoft.ClassicStorage/storageAccounts/listkeys/action",
        "Microsoft.ClassicStorage/storageAccounts/regeneratekey/action"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Classic Storage Account Key Operator Service Role",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="data-box-contributor"></a>Katkıda bulunan Data Box

, Diğer kullanıcılara erişim izni hariç Data Box hizmeti altındaki her şeyi yönetmenizi sağlar.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. Authorization/*/Read | Rolleri ve rol atamalarını oku |
> | Microsoft. ResourceHealth/kullanılabilirlik Bilitydurumlar/okuma | Belirtilen kapsamdaki tüm kaynaklar için Kullanılabilirlik durumlarını alır |
> | Microsoft. resources/dağıtımlar/* | Dağıtım oluşturma ve yönetme |
> | Microsoft. resources/abonelikler/resourceGroups/Read | Kaynak gruplarını alır veya listeler. |
> | Microsoft. support/* | Destek bileti oluşturma ve güncelleştirme |
> | Microsoft. databox/* |  |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage everything under Data Box Service except giving access to others.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/add466c9-e687-43fc-8d98-dfcf8d720be5",
  "name": "add466c9-e687-43fc-8d98-dfcf8d720be5",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*",
        "Microsoft.Databox/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Data Box Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="data-box-reader"></a>Data Box okuyucu

Sipariş oluşturma veya düzenleme sırası ayrıntıları ve başkalarına erişim verme dışında Data Box hizmetini yönetmenizi sağlar.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. Authorization/*/Read | Rolleri ve rol atamalarını oku |
> | Microsoft. databox/*/Read |  |
> | Microsoft. databox/Jobs/listgizlilikler/Action |  |
> | Microsoft. databox/Jobs/listcredentials/Action | Siparişle ilgili şifrelenmemiş kimlik bilgilerini listeler. |
> | Microsoft. databox/Locations/Availablesku 'Lar/eylem | Bu yöntem, kullanılabilir SKU 'ların listesini döndürür. |
> | Microsoft. databox/Locations/Validategirişlerinde/Action | Bu yöntem, tüm doğrulama türlerini yapar. |
> | Microsoft. databox/Locations/regionConfiguration/Action | Bu yöntem, bölgenin yapılandırmasını döndürür. |
> | Microsoft. databox/Locations/validateAddress/Action | Sevkiyat adresini doğrular ve varsa alternatif adresler sağlar. |
> | Microsoft. ResourceHealth/kullanılabilirlik Bilitydurumlar/okuma | Belirtilen kapsamdaki tüm kaynaklar için Kullanılabilirlik durumlarını alır |
> | Microsoft. support/* | Destek bileti oluşturma ve güncelleştirme |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage Data Box Service except creating order or editing order details and giving access to others.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/028f4ed7-e2a9-465e-a8f4-9c0ffdfdc027",
  "name": "028f4ed7-e2a9-465e-a8f4-9c0ffdfdc027",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Databox/*/read",
        "Microsoft.Databox/jobs/listsecrets/action",
        "Microsoft.Databox/jobs/listcredentials/action",
        "Microsoft.Databox/locations/availableSkus/action",
        "Microsoft.Databox/locations/validateInputs/action",
        "Microsoft.Databox/locations/regionConfiguration/action",
        "Microsoft.Databox/locations/validateAddress/action",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Data Box Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="data-lake-analytics-developer"></a>Data Lake Analytics geliştirici

Kendi işlerinizi göndermenize, izlemenize ve yönetmenize izin verir, ancak Data Lake Analytics Hesapları oluşturamaz veya silemezsiniz.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. Authorization/*/Read | Rolleri ve rol atamalarını oku |
> | Microsoft. BigAnalytics/accounts/* |  |
> | Microsoft. DataLakeAnalytics/accounts/* |  |
> | Microsoft. Insights/alertRules/* | Klasik ölçüm uyarısı oluşturma ve yönetme |
> | Microsoft. ResourceHealth/kullanılabilirlik Bilitydurumlar/okuma | Belirtilen kapsamdaki tüm kaynaklar için Kullanılabilirlik durumlarını alır |
> | Microsoft. resources/dağıtımlar/* | Dağıtım oluşturma ve yönetme |
> | Microsoft. resources/abonelikler/resourceGroups/Read | Kaynak gruplarını alır veya listeler. |
> | Microsoft. support/* | Destek bileti oluşturma ve güncelleştirme |
> | **NotActions** |  |
> | Microsoft. BigAnalytics/hesaplar/Sil |  |
> | Microsoft. BigAnalytics/hesaplar/Takesahiplik/eylem |  |
> | Microsoft. BigAnalytics/hesaplar/yazma |  |
> | Microsoft. DataLakeAnalytics/hesaplar/Sil | Bir DataLakeAnalytics hesabını silin. |
> | Microsoft. DataLakeAnalytics/hesaplar/Takesahiplik/eylem | Diğer kullanıcılar tarafından gönderilen işleri iptal etmek için izin verin. |
> | Microsoft. DataLakeAnalytics/hesaplar/yazma | Bir DataLakeAnalytics hesabı oluşturun veya güncelleştirin. |
> | Microsoft. DataLakeAnalytics/accounts/dataLakeStoreAccounts/Write | Bir DataLakeAnalytics hesabının bağlı DataLakeStore hesabını oluşturun veya güncelleştirin. |
> | Microsoft. DataLakeAnalytics/accounts/dataLakeStoreAccounts/Delete | Bir DataLakeStore hesabının bir DataLakeAnalytics hesabıyla bağlantısını kaldırın. |
> | Microsoft. DataLakeAnalytics/hesaplar/storageAccounts/Write | Bir DataLakeAnalytics hesabı için bağlı bir depolama hesabı oluşturun veya güncelleştirin. |
> | Microsoft. DataLakeAnalytics/hesaplar/storageAccounts/Delete | Bir depolama hesabının bir DataLakeAnalytics hesabından bağlantısını kaldırın. |
> | Microsoft. DataLakeAnalytics/accounts/firewallRules/Write | Bir güvenlik duvarı kuralı oluşturun veya güncelleştirin. |
> | Microsoft. DataLakeAnalytics/accounts/firewallRules/Delete | Bir güvenlik duvarı kuralını silin. |
> | Microsoft. DataLakeAnalytics/accounts/computePolicies/Write | Bir işlem ilkesi oluşturun veya güncelleştirin. |
> | Microsoft. DataLakeAnalytics/accounts/computePolicies/Delete | İşlem ilkesini silin. |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you submit, monitor, and manage your own jobs but not create or delete Data Lake Analytics accounts.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/47b7735b-770e-4598-a7da-8b91488b4c88",
  "name": "47b7735b-770e-4598-a7da-8b91488b4c88",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.BigAnalytics/accounts/*",
        "Microsoft.DataLakeAnalytics/accounts/*",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [
        "Microsoft.BigAnalytics/accounts/Delete",
        "Microsoft.BigAnalytics/accounts/TakeOwnership/action",
        "Microsoft.BigAnalytics/accounts/Write",
        "Microsoft.DataLakeAnalytics/accounts/Delete",
        "Microsoft.DataLakeAnalytics/accounts/TakeOwnership/action",
        "Microsoft.DataLakeAnalytics/accounts/Write",
        "Microsoft.DataLakeAnalytics/accounts/dataLakeStoreAccounts/Write",
        "Microsoft.DataLakeAnalytics/accounts/dataLakeStoreAccounts/Delete",
        "Microsoft.DataLakeAnalytics/accounts/storageAccounts/Write",
        "Microsoft.DataLakeAnalytics/accounts/storageAccounts/Delete",
        "Microsoft.DataLakeAnalytics/accounts/firewallRules/Write",
        "Microsoft.DataLakeAnalytics/accounts/firewallRules/Delete",
        "Microsoft.DataLakeAnalytics/accounts/computePolicies/Write",
        "Microsoft.DataLakeAnalytics/accounts/computePolicies/Delete"
      ],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Data Lake Analytics Developer",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="reader-and-data-access"></a>Okuyucu ve veri erişimi

Her şeyi görüntülemenize izin verir, ancak bir depolama hesabını veya kapsanan kaynağı silmenize veya oluşturmanıza izin vermez. Ayrıca depolama hesabı anahtarlarına erişim aracılığıyla bir depolama hesabında bulunan tüm verilere okuma/yazma erişimi de sağlar.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. Storage/storageAccounts/listKeys/Action | Belirtilen depolama hesabı için erişim anahtarlarını döndürür. |
> | Microsoft. Storage/storageAccounts/ListAccountSas/eylem | Belirtilen depolama hesabı için hesap SAS belirtecini döndürür. |
> | Microsoft. Storage/storageAccounts/Read | Depolama hesaplarının listesini döndürür veya belirtilen depolama hesabının özelliklerini alır. |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you view everything but will not let you delete or create a storage account or contained resource. It will also allow read/write access to all data contained in a storage account via access to storage account keys.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/c12c1c16-33a1-487b-954d-41c89c60f349",
  "name": "c12c1c16-33a1-487b-954d-41c89c60f349",
  "permissions": [
    {
      "actions": [
        "Microsoft.Storage/storageAccounts/listKeys/action",
        "Microsoft.Storage/storageAccounts/ListAccountSas/action",
        "Microsoft.Storage/storageAccounts/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Reader and Data Access",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="storage-account-contributor"></a>Depolama Hesabı Katılımcısı

Depolama hesaplarının yönetimine izin verir. Paylaşılan anahtar yetkilendirmesi aracılığıyla verilere erişmek için kullanılabilen hesap anahtarına erişim sağlar.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. Authorization/*/Read | Rolleri ve rol atamalarını oku |
> | Microsoft. Insights/alertRules/* | Klasik ölçüm uyarısı oluşturma ve yönetme |
> | Microsoft. Insights/diagnosticSettings/* | Analiz Sunucusu için tanılama ayarını oluşturur, güncelleştirir veya okur |
> | Microsoft. Network/virtualNetworks/alt ağlar/Jodavetli Aserviceendpoint/Action | Depolama hesabı veya SQL veritabanı gibi kaynağı bir alt ağa birleştirir. Alertable değil. |
> | Microsoft. ResourceHealth/kullanılabilirlik Bilitydurumlar/okuma | Belirtilen kapsamdaki tüm kaynaklar için Kullanılabilirlik durumlarını alır |
> | Microsoft. resources/dağıtımlar/* | Dağıtım oluşturma ve yönetme |
> | Microsoft. resources/abonelikler/resourceGroups/Read | Kaynak gruplarını alır veya listeler. |
> | Microsoft. Storage/storageAccounts/* | Depolama hesapları oluşturma ve yönetme |
> | Microsoft. support/* | Destek bileti oluşturma ve güncelleştirme |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage storage accounts, including accessing storage account keys which provide full access to storage account data.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/17d1049b-9a84-46fb-8f53-869881c3d3ab",
  "name": "17d1049b-9a84-46fb-8f53-869881c3d3ab",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Insights/diagnosticSettings/*",
        "Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/action",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Storage/storageAccounts/*",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Storage Account Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="storage-account-key-operator-service-role"></a>Depolama hesabı anahtar operatörü hizmet rolü

Depolama hesabı erişim anahtarlarının listelenmesi ve yeniden oluşturulmasına izin verir.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. Storage/storageAccounts/ListKeys/Action | Belirtilen depolama hesabı için erişim anahtarlarını döndürür. |
> | Microsoft. Storage/storageAccounts/RegenerateKey/ACTION | Belirtilen depolama hesabı için erişim anahtarlarını yeniden oluşturur. |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Storage Account Key Operators are allowed to list and regenerate keys on Storage Accounts",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/81a9662b-bebf-436f-a333-f67b29880f12",
  "name": "81a9662b-bebf-436f-a333-f67b29880f12",
  "permissions": [
    {
      "actions": [
        "Microsoft.Storage/storageAccounts/listkeys/action",
        "Microsoft.Storage/storageAccounts/regeneratekey/action"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Storage Account Key Operator Service Role",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="storage-blob-data-contributor"></a>Depolama Blobu veri Katılımcısı

Azure depolama kapsayıcıları ve bloblarını okuyun, yazın ve silin. Belirli bir veri işlemi için hangi eylemlerin gerekli olduğunu öğrenmek için bkz. [BLOB ve kuyruk verisi işlemlerini çağırma izinleri](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations).

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. Storage/storageAccounts/blobServices/kapsayıcılar/Delete | Kapsayıcıyı silin. |
> | Microsoft. Storage/storageAccounts/blobServices/kapsayıcılar/okuma | Bir kapsayıcı veya kapsayıcıların listesini döndürün. |
> | Microsoft. Storage/storageAccounts/blobServices/kapsayıcılar/Write | Kapsayıcının meta verilerini veya özelliklerini değiştirin. |
> | Microsoft. Storage/storageAccounts/blobServices/generateUserDelegationKey/Action | Blob hizmeti için Kullanıcı temsili anahtarını döndürür. |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | Microsoft. Storage/storageAccounts/blobServices/kapsayıcılar/Bloblar/Sil | Bir blobu silin. |
> | Microsoft. Storage/storageAccounts/blobServices/kapsayıcılar/blob/okuma | Blob veya Blobların listesini döndürün. |
> | Microsoft. Storage/storageAccounts/blobServices/kapsayıcılar/Bloblar/taşıma/eylem | Blobu bir yoldan diğerine kaydırır |
> | Microsoft. Storage/storageAccounts/blobServices/kapsayıcılar/Bloblar/yazma | Bir bloba yazın. |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for read, write and delete access to Azure Storage blob containers and data",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/ba92f5b4-2d11-453d-a403-e96b0029c9fe",
  "name": "ba92f5b4-2d11-453d-a403-e96b0029c9fe",
  "permissions": [
    {
      "actions": [
        "Microsoft.Storage/storageAccounts/blobServices/containers/delete",
        "Microsoft.Storage/storageAccounts/blobServices/containers/read",
        "Microsoft.Storage/storageAccounts/blobServices/containers/write",
        "Microsoft.Storage/storageAccounts/blobServices/generateUserDelegationKey/action"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/delete",
        "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read",
        "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/move/action",
        "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/write"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Storage Blob Data Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="storage-blob-data-owner"></a>Depolama Blobu veri sahibi

, POSIX erişim denetimi atama dahil olmak üzere Azure depolama blob kapsayıcılarına ve verilerine tam erişim sağlar. Belirli bir veri işlemi için hangi eylemlerin gerekli olduğunu öğrenmek için bkz. [BLOB ve kuyruk verisi işlemlerini çağırma izinleri](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations).

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. Storage/storageAccounts/blobServices/kapsayıcılar/* | Kapsayıcılarda tam izinler. |
> | Microsoft. Storage/storageAccounts/blobServices/generateUserDelegationKey/Action | Blob hizmeti için Kullanıcı temsili anahtarını döndürür. |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | Microsoft. Storage/storageAccounts/blobServices/kapsayıcılar/Bloblar/* | Bloblarda tam izinler. |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for full access to Azure Storage blob containers and data, including assigning POSIX access control.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/b7e6dc6d-f1e8-4753-8033-0f276bb0955b",
  "name": "b7e6dc6d-f1e8-4753-8033-0f276bb0955b",
  "permissions": [
    {
      "actions": [
        "Microsoft.Storage/storageAccounts/blobServices/containers/*",
        "Microsoft.Storage/storageAccounts/blobServices/generateUserDelegationKey/action"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/*"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Storage Blob Data Owner",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="storage-blob-data-reader"></a>Depolama Blobu veri okuyucusu

Azure depolama kapsayıcıları ve bloblarını okuyun ve listeleyin. Belirli bir veri işlemi için hangi eylemlerin gerekli olduğunu öğrenmek için bkz. [BLOB ve kuyruk verisi işlemlerini çağırma izinleri](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations).

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. Storage/storageAccounts/blobServices/kapsayıcılar/okuma | Bir kapsayıcı veya kapsayıcıların listesini döndürün. |
> | Microsoft. Storage/storageAccounts/blobServices/generateUserDelegationKey/Action | Blob hizmeti için Kullanıcı temsili anahtarını döndürür. |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | Microsoft. Storage/storageAccounts/blobServices/kapsayıcılar/blob/okuma | Blob veya Blobların listesini döndürün. |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for read access to Azure Storage blob containers and data",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/2a2b9908-6ea1-4ae2-8e65-a410df84e7d1",
  "name": "2a2b9908-6ea1-4ae2-8e65-a410df84e7d1",
  "permissions": [
    {
      "actions": [
        "Microsoft.Storage/storageAccounts/blobServices/containers/read",
        "Microsoft.Storage/storageAccounts/blobServices/generateUserDelegationKey/action"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Storage Blob Data Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="storage-blob-delegator"></a>Depolama Blobu Delegator

Azure AD kimlik bilgileriyle imzalanan bir kapsayıcı veya blob için paylaşılan erişim imzası oluşturmak üzere kullanılabilen bir Kullanıcı temsili anahtarı alın. Daha fazla bilgi için bkz. [Kullanıcı TEMSILI SAS oluşturma](https://docs.microsoft.com/rest/api/storageservices/create-user-delegation-sas).

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. Storage/storageAccounts/blobServices/generateUserDelegationKey/Action | Blob hizmeti için Kullanıcı temsili anahtarını döndürür. |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for generation of a user delegation key which can be used to sign SAS tokens",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/db58b8e5-c6ad-4a2a-8342-4190687cbf4a",
  "name": "db58b8e5-c6ad-4a2a-8342-4190687cbf4a",
  "permissions": [
    {
      "actions": [
        "Microsoft.Storage/storageAccounts/blobServices/generateUserDelegationKey/action"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Storage Blob Delegator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="storage-file-data-smb-share-contributor"></a>Depolama dosyası veri SMB paylaşımında katkıda bulunan

Azure dosya paylaşımlarında dosya/dizinlerde okuma, yazma ve silme erişimine izin verir. Bu rolün Windows dosya sunucularında yerleşik bir eşdeğeri yoktur.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | *yok* |  |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | Microsoft. Storage/storageAccounts/fileServices/fileshares/Files/Read | Dosya/klasör veya dosya/klasör listesini döndürür. |
> | Microsoft. Storage/storageAccounts/fileServices/fileshares/Files/Write | Bir dosya yazmanın veya bir klasör oluşturmanın sonucunu döndürür. |
> | Microsoft. Storage/storageAccounts/fileServices/fileshares/Files/Delete | Dosya/klasör silmenin sonucunu döndürür. |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for read, write, and delete access in Azure Storage file shares over SMB",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/0c867c2a-1d8c-454a-a3db-ab2ea1bdc8bb",
  "name": "0c867c2a-1d8c-454a-a3db-ab2ea1bdc8bb",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.Storage/storageAccounts/fileServices/fileshares/files/read",
        "Microsoft.Storage/storageAccounts/fileServices/fileshares/files/write",
        "Microsoft.Storage/storageAccounts/fileServices/fileshares/files/delete"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Storage File Data SMB Share Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="storage-file-data-smb-share-elevated-contributor"></a>Depolama dosyası veri SMB paylaşımında yükseltilmiş katkıda bulunan

Azure dosya paylaşımlarında dosya/dizinlerde okuma, yazma, silme ve değiştirme ACL 'Lerine izin verir. Bu rol, Windows dosya sunucularındaki değişikliğin bir dosya paylaşımının ACL 'sine eşdeğerdir.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | *yok* |  |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | Microsoft. Storage/storageAccounts/fileServices/fileshares/Files/Read | Dosya/klasör veya dosya/klasör listesini döndürür. |
> | Microsoft. Storage/storageAccounts/fileServices/fileshares/Files/Write | Bir dosya yazmanın veya bir klasör oluşturmanın sonucunu döndürür. |
> | Microsoft. Storage/storageAccounts/fileServices/fileshares/Files/Delete | Dosya/klasör silmenin sonucunu döndürür. |
> | Microsoft. Storage/storageAccounts/fileServices/fileshares/Files/modifypermissions/Action | Bir dosya/klasör üzerinde izin değiştirme sonucunu döndürür. |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for read, write, delete and modify NTFS permission access in Azure Storage file shares over SMB",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/a7264617-510b-434b-a828-9731dc254ea7",
  "name": "a7264617-510b-434b-a828-9731dc254ea7",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.Storage/storageAccounts/fileServices/fileshares/files/read",
        "Microsoft.Storage/storageAccounts/fileServices/fileshares/files/write",
        "Microsoft.Storage/storageAccounts/fileServices/fileshares/files/delete",
        "Microsoft.Storage/storageAccounts/fileServices/fileshares/files/modifypermissions/action"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Storage File Data SMB Share Elevated Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="storage-file-data-smb-share-reader"></a>Depolama dosyası veri SMB paylaşma okuyucusu

Azure dosya paylaşımlarında dosya/dizinlerde okuma erişimine izin verir. Bu rol, Windows dosya sunucularında okunan bir dosya paylaşma ACL 'sine eşdeğerdir.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | *yok* |  |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | Microsoft. Storage/storageAccounts/fileServices/fileshares/Files/Read | Dosya/klasör veya dosya/klasör listesini döndürür. |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for read access to Azure File Share over SMB",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/aba4ae5f-2193-4029-9191-0cb91df5e314",
  "name": "aba4ae5f-2193-4029-9191-0cb91df5e314",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.Storage/storageAccounts/fileServices/fileshares/files/read"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Storage File Data SMB Share Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="storage-queue-data-contributor"></a>Depolama kuyruğu verileri Katılımcısı

Azure depolama kuyruklarını ve sıra iletilerini okuyun, yazın ve silin. Belirli bir veri işlemi için hangi eylemlerin gerekli olduğunu öğrenmek için bkz. [BLOB ve kuyruk verisi işlemlerini çağırma izinleri](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations).

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. Storage/storageAccounts/queueServices/kuyruklar/Sil | Kuyruğu silme. |
> | Microsoft. Storage/storageAccounts/queueServices/kuyrukları/okuma | Kuyruğu veya kuyruk listesini döndürün. |
> | Microsoft. Storage/storageAccounts/queueServices/kuyruklar/Write | Sıra meta verilerini veya özelliklerini değiştirin. |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | Microsoft. Storage/storageAccounts/queueServices/kuyruklar/iletiler/Sil | Bir kuyruktan bir veya daha fazla iletiyi silin. |
> | Microsoft. Storage/storageAccounts/queueServices/kuyruklar/iletiler/okundu | Bir kuyruktaki bir veya daha fazla iletiyi Peek veya alma. |
> | Microsoft. Storage/storageAccounts/queueServices/kuyruklar/iletiler/yazma | Bir kuyruğa ileti ekleyin. |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for read, write, and delete access to Azure Storage queues and queue messages",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/974c5e8b-45b9-4653-ba55-5f855dd0fb88",
  "name": "974c5e8b-45b9-4653-ba55-5f855dd0fb88",
  "permissions": [
    {
      "actions": [
        "Microsoft.Storage/storageAccounts/queueServices/queues/delete",
        "Microsoft.Storage/storageAccounts/queueServices/queues/read",
        "Microsoft.Storage/storageAccounts/queueServices/queues/write"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.Storage/storageAccounts/queueServices/queues/messages/delete",
        "Microsoft.Storage/storageAccounts/queueServices/queues/messages/read",
        "Microsoft.Storage/storageAccounts/queueServices/queues/messages/write"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Storage Queue Data Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="storage-queue-data-message-processor"></a>Depolama kuyruğu veri Iletisi Işlemcisi

Azure depolama kuyruğundan bir iletiyi göz atın, alın ve silin. Belirli bir veri işlemi için hangi eylemlerin gerekli olduğunu öğrenmek için bkz. [BLOB ve kuyruk verisi işlemlerini çağırma izinleri](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations).

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | *yok* |  |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | Microsoft. Storage/storageAccounts/queueServices/kuyruklar/iletiler/okundu | İletiye göz atın. |
> | Microsoft. Storage/storageAccounts/queueServices/kuyruklar/iletiler/işlem/eylem | Bir iletiyi alın ve silin. |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for peek, receive, and delete access to Azure Storage queue messages",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/8a0f0c08-91a1-4084-bc3d-661d67233fed",
  "name": "8a0f0c08-91a1-4084-bc3d-661d67233fed",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.Storage/storageAccounts/queueServices/queues/messages/read",
        "Microsoft.Storage/storageAccounts/queueServices/queues/messages/process/action"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Storage Queue Data Message Processor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="storage-queue-data-message-sender"></a>Depolama kuyruğu veri Iletisi gönderici

Bir Azure depolama kuyruğuna ileti ekleyin. Belirli bir veri işlemi için hangi eylemlerin gerekli olduğunu öğrenmek için bkz. [BLOB ve kuyruk verisi işlemlerini çağırma izinleri](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations).

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | *yok* |  |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | Microsoft. Storage/storageAccounts/queueServices/kuyruklar/iletiler/ekleme/eylem | Bir kuyruğa ileti ekleyin. |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for sending of Azure Storage queue messages",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/c6a89b2d-59bc-44d0-9896-0f6e12d7b80a",
  "name": "c6a89b2d-59bc-44d0-9896-0f6e12d7b80a",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.Storage/storageAccounts/queueServices/queues/messages/add/action"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Storage Queue Data Message Sender",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="storage-queue-data-reader"></a>Depolama kuyruğu veri okuyucusu

Azure depolama kuyruklarını ve sıra iletilerini okuyun ve listeleyin. Belirli bir veri işlemi için hangi eylemlerin gerekli olduğunu öğrenmek için bkz. [BLOB ve kuyruk verisi işlemlerini çağırma izinleri](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations).

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. Storage/storageAccounts/queueServices/kuyrukları/okuma | Kuyruğu veya kuyruk listesini döndürür. |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | Microsoft. Storage/storageAccounts/queueServices/kuyruklar/iletiler/okundu | Bir kuyruktaki bir veya daha fazla iletiyi Peek veya alma. |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for read access to Azure Storage queues and queue messages",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/19e7f393-937e-4f77-808e-94535e297925",
  "name": "19e7f393-937e-4f77-808e-94535e297925",
  "permissions": [
    {
      "actions": [
        "Microsoft.Storage/storageAccounts/queueServices/queues/read"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.Storage/storageAccounts/queueServices/queues/messages/read"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Storage Queue Data Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

## <a name="web"></a>Web


### <a name="azure-maps-data-reader"></a>Azure haritalar veri okuyucu

Azure haritalar hesabından ilgili harita okuma verilerine erişim izni verir.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | *yok* |  |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | Microsoft. Maps/hesaplar/*/Read |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Grants access to read map related data from an Azure maps account.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/423170ca-a8f6-4b0f-8487-9e4eb8f49bfa",
  "name": "423170ca-a8f6-4b0f-8487-9e4eb8f49bfa",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.Maps/accounts/*/read"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Maps Data Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="search-service-contributor"></a>Katkıda bulunan Arama Hizmeti

Arama hizmetlerini yönetmenize izin verir, ancak bunlara erişim izni vermez.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. Authorization/*/Read | Rolleri ve rol atamalarını oku |
> | Microsoft. Insights/alertRules/* | Klasik ölçüm uyarısı oluşturma ve yönetme |
> | Microsoft. ResourceHealth/kullanılabilirlik Bilitydurumlar/okuma | Belirtilen kapsamdaki tüm kaynaklar için Kullanılabilirlik durumlarını alır |
> | Microsoft. resources/dağıtımlar/* | Dağıtım oluşturma ve yönetme |
> | Microsoft. resources/abonelikler/resourceGroups/Read | Kaynak gruplarını alır veya listeler. |
> | Microsoft. Search/searchServices/* | Arama Hizmetleri oluşturma ve yönetme |
> | Microsoft. support/* | Destek bileti oluşturma ve güncelleştirme |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage Search services, but not access to them.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/7ca78c08-252a-4471-8644-bb5ff32d4ba0",
  "name": "7ca78c08-252a-4471-8644-bb5ff32d4ba0",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Search/searchServices/*",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Search Service Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="web-plan-contributor"></a>Web planı Katılımcısı

Web siteleri için Web planlarını yönetmenizi sağlar, ancak bunlara erişemez.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. Authorization/*/Read | Rolleri ve rol atamalarını oku |
> | Microsoft. Insights/alertRules/* | Klasik ölçüm uyarısı oluşturma ve yönetme |
> | Microsoft. ResourceHealth/kullanılabilirlik Bilitydurumlar/okuma | Belirtilen kapsamdaki tüm kaynaklar için Kullanılabilirlik durumlarını alır |
> | Microsoft. resources/dağıtımlar/* | Dağıtım oluşturma ve yönetme |
> | Microsoft. resources/abonelikler/resourceGroups/Read | Kaynak gruplarını alır veya listeler. |
> | Microsoft. support/* | Destek bileti oluşturma ve güncelleştirme |
> | Microsoft. Web/Sunucugrupları/* | Sunucu grupları oluşturma ve yönetme |
> | Microsoft. Web/hostingEnvironments/JOIN/Action | Bir App Service Ortamı birleştirir |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage the web plans for websites, but not access to them.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/2cc479cb-7b4d-49a8-b449-8c00fd0f0a4b",
  "name": "2cc479cb-7b4d-49a8-b449-8c00fd0f0a4b",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*",
        "Microsoft.Web/serverFarms/*",
        "Microsoft.Web/hostingEnvironments/Join/Action"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Web Plan Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="website-contributor"></a>Web sitesi Katılımcısı

Web sitelerini yönetmenizi sağlar, ancak bunlara erişemez.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. Authorization/*/Read | Rolleri ve rol atamalarını oku |
> | Microsoft. Insights/alertRules/* | Klasik ölçüm uyarısı oluşturma ve yönetme |
> | Microsoft. Insights/bileşenler/* | Öngörüler bileşenleri oluşturma ve yönetme |
> | Microsoft. ResourceHealth/kullanılabilirlik Bilitydurumlar/okuma | Belirtilen kapsamdaki tüm kaynaklar için Kullanılabilirlik durumlarını alır |
> | Microsoft. resources/dağıtımlar/* | Dağıtım oluşturma ve yönetme |
> | Microsoft. resources/abonelikler/resourceGroups/Read | Kaynak gruplarını alır veya listeler. |
> | Microsoft. support/* | Destek bileti oluşturma ve güncelleştirme |
> | Microsoft. Web/Certificates/* | Web sitesi sertifikaları oluşturma ve yönetme |
> | Microsoft. Web/listSitesAssignedToHostName/Read | Ana bilgisayar adına atanan sitelerin adlarını alın. |
> | Microsoft. Web/Sunucugrupları/JOIN/Action |  |
> | Microsoft. Web/Sunucugrupları/okuma | App Service planında özellikleri al |
> | Microsoft. Web/Sites/* | Web siteleri oluşturma ve yönetme (site oluşturma, ayrıca ilişkili App Service planına yazma izinleri gerektirir) |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage websites (not web plans), but not access to them.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/de139f84-1756-47ae-9be6-808fbbe84772",
  "name": "de139f84-1756-47ae-9be6-808fbbe84772",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Insights/components/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*",
        "Microsoft.Web/certificates/*",
        "Microsoft.Web/listSitesAssignedToHostName/read",
        "Microsoft.Web/serverFarms/join/action",
        "Microsoft.Web/serverFarms/read",
        "Microsoft.Web/sites/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Website Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

## <a name="containers"></a>Kapsayıcılar


### <a name="acrdelete"></a>AcrDelete

ACR silme

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. ContainerRegistry/kayıt defterleri/yapıtlar/silme | Bir kapsayıcı kayıt defterinde yapıtı silin. |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "acr delete",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/c2f4ef07-c644-48eb-af81-4b1b4947fb11",
  "name": "c2f4ef07-c644-48eb-af81-4b1b4947fb11",
  "permissions": [
    {
      "actions": [
        "Microsoft.ContainerRegistry/registries/artifacts/delete"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "AcrDelete",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="acrimagesigner"></a>Acrimageimzalayan

ACR görüntü imzalayan

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. ContainerRegistry/kayıt defterleri/imzala/yaz | Kapsayıcı kayıt defteri için gönderme/çekme içeriği güven meta verileri. |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "acr image signer",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/6cef56e8-d556-48e5-a04f-b8e64114680f",
  "name": "6cef56e8-d556-48e5-a04f-b8e64114680f",
  "permissions": [
    {
      "actions": [
        "Microsoft.ContainerRegistry/registries/sign/write"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "AcrImageSigner",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="acrpull"></a>AcrPull

ACR çekme

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. ContainerRegistry/kayıt defterleri/çekme/okuma | Bir kapsayıcı kayıt defterinden görüntüleri çekin veya alın. |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "acr pull",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/7f951dda-4ed3-4680-a7ca-43fe172d538d",
  "name": "7f951dda-4ed3-4680-a7ca-43fe172d538d",
  "permissions": [
    {
      "actions": [
        "Microsoft.ContainerRegistry/registries/pull/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "AcrPull",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="acrpush"></a>AcrPush

ACR gönderimi

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. ContainerRegistry/kayıt defterleri/çekme/okuma | Bir kapsayıcı kayıt defterinden görüntüleri çekin veya alın. |
> | Microsoft. ContainerRegistry/kayıt defterleri/gönderme/yazma | Bir kapsayıcı kayıt defterine görüntü gönderin veya yazın. |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "acr push",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/8311e382-0749-4cb8-b61a-304f252e45ec",
  "name": "8311e382-0749-4cb8-b61a-304f252e45ec",
  "permissions": [
    {
      "actions": [
        "Microsoft.ContainerRegistry/registries/pull/read",
        "Microsoft.ContainerRegistry/registries/push/write"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "AcrPush",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="acrquarantinereader"></a>Acrquaranınereader

ACR karantina veri okuyucusu

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. ContainerRegistry/kayıt defterleri/karantina/okuma | Kapsayıcı kayıt defterinden karantinaya alınmış görüntüleri çekme veya alma |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "acr quarantine data reader",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/cdda3590-29a3-44f6-95f2-9f980659eb04",
  "name": "cdda3590-29a3-44f6-95f2-9f980659eb04",
  "permissions": [
    {
      "actions": [
        "Microsoft.ContainerRegistry/registries/quarantine/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "AcrQuarantineReader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="acrquarantinewriter"></a>AcrQuarantineWriter

ACR karantina veri yazıcısı

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. ContainerRegistry/kayıt defterleri/karantina/okuma | Kapsayıcı kayıt defterinden karantinaya alınmış görüntüleri çekme veya alma |
> | Microsoft. ContainerRegistry/kayıt defterleri/karantina/yazma | Karantinaya alınan görüntülerin karantina durumunu yazma/değiştirme |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "acr quarantine data writer",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/c8d4ff99-41c3-41a8-9f60-21dfdad59608",
  "name": "c8d4ff99-41c3-41a8-9f60-21dfdad59608",
  "permissions": [
    {
      "actions": [
        "Microsoft.ContainerRegistry/registries/quarantine/read",
        "Microsoft.ContainerRegistry/registries/quarantine/write"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "AcrQuarantineWriter",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="azure-kubernetes-service-cluster-admin-role"></a>Azure Kubernetes hizmet kümesi yönetici rolü

Küme Yöneticisi kimlik bilgisi eylemini listeleyin.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. ContainerService/Managedkümeler/listClusterAdminCredential/ACTION | Yönetilen kümenin clusterAdmin kimlik bilgisini listeleyin |
> | Microsoft. ContainerService/Managedkümeler/accessProfiles/listCredential/Action | Liste kimlik bilgisini kullanarak rol adına göre yönetilen küme erişim profili al |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "List cluster admin credential action.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/0ab0b1a8-8aac-4efd-b8c2-3ee1fb270be8",
  "name": "0ab0b1a8-8aac-4efd-b8c2-3ee1fb270be8",
  "permissions": [
    {
      "actions": [
        "Microsoft.ContainerService/managedClusters/listClusterAdminCredential/action",
        "Microsoft.ContainerService/managedClusters/accessProfiles/listCredential/action"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Kubernetes Service Cluster Admin Role",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="azure-kubernetes-service-cluster-user-role"></a>Azure Kubernetes hizmet kümesi Kullanıcı rolü

Küme kullanıcı kimlik bilgilerini Listele eylemi.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. ContainerService/Managedkümeler/listClusterUserCredential/Action | Yönetilen kümenin clusterUser kimlik bilgisini listeleyin |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "List cluster user credential action.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/4abbcc35-e782-43d8-92c5-2d3f1bd2253f",
  "name": "4abbcc35-e782-43d8-92c5-2d3f1bd2253f",
  "permissions": [
    {
      "actions": [
        "Microsoft.ContainerService/managedClusters/listClusterUserCredential/action"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Kubernetes Service Cluster User Role",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

## <a name="databases"></a>Veritabanları


### <a name="cosmos-db-account-reader-role"></a>Cosmos DB hesabı okuyucu rolü

Azure Cosmos DB hesabı verilerini okuyabilir. Azure Cosmos DB hesaplarını yönetmek için [DocumentDB hesabı katılımcısı](#documentdb-account-contributor) konusuna bakın.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. Authorization/*/Read | Rolleri ve rol atamalarını oku |
> | Microsoft. DocumentDB/*/Read | Tüm koleksiyonları okuyun |
> | Microsoft. DocumentDB/databaseAccounts/readonlykeys/Action | Veritabanı hesabı salt okunur anahtarlarını okur. |
> | Microsoft. Insights/MetricDefinitions/okuma | Ölçüm tanımlarını oku |
> | Microsoft. Insights/ölçümler/okuma | Ölçümleri oku |
> | Microsoft. resources/abonelikler/resourceGroups/Read | Kaynak gruplarını alır veya listeler. |
> | Microsoft. support/* | Destek bileti oluşturma ve güncelleştirme |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can read Azure Cosmos DB Accounts data",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/fbdf93bf-df7d-467e-a4d2-9458aa1360c8",
  "name": "fbdf93bf-df7d-467e-a4d2-9458aa1360c8",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.DocumentDB/*/read",
        "Microsoft.DocumentDB/databaseAccounts/readonlykeys/action",
        "Microsoft.Insights/MetricDefinitions/read",
        "Microsoft.Insights/Metrics/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Cosmos DB Account Reader Role",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="cosmos-db-operator"></a>Cosmos DB Işleci

Azure Cosmos DB hesaplarını yönetmenizi sağlar ancak içerdikleri verilere erişemez. Hesap anahtarlarına ve bağlantı dizelerine erişimi engeller.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. DocumentDb/databaseAccounts/* |  |
> | Microsoft. Insights/alertRules/* | Klasik ölçüm uyarısı oluşturma ve yönetme |
> | Microsoft. Authorization/*/Read | Rolleri ve rol atamalarını oku |
> | Microsoft. ResourceHealth/kullanılabilirlik Bilitydurumlar/okuma | Belirtilen kapsamdaki tüm kaynaklar için Kullanılabilirlik durumlarını alır |
> | Microsoft. resources/dağıtımlar/* | Dağıtım oluşturma ve yönetme |
> | Microsoft. resources/abonelikler/resourceGroups/Read | Kaynak gruplarını alır veya listeler. |
> | Microsoft. support/* | Destek bileti oluşturma ve güncelleştirme |
> | Microsoft. Network/virtualNetworks/alt ağlar/Jodavetli Aserviceendpoint/Action | Depolama hesabı veya SQL veritabanı gibi kaynağı bir alt ağa birleştirir. Alertable değil. |
> | **NotActions** |  |
> | Microsoft. DocumentDB/databaseAccounts/readonlyKeys/* |  |
> | Microsoft. DocumentDB/databaseAccounts/regenerateKey/* |  |
> | Microsoft. DocumentDB/databaseAccounts/listKeys/* |  |
> | Microsoft. DocumentDB/databaseAccounts/listConnectionStrings/* |  |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage Azure Cosmos DB accounts, but not access data in them. Prevents access to account keys and connection strings.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/230815da-be43-4aae-9cb4-875f7bd000aa",
  "name": "230815da-be43-4aae-9cb4-875f7bd000aa",
  "permissions": [
    {
      "actions": [
        "Microsoft.DocumentDb/databaseAccounts/*",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Authorization/*/read",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*",
        "Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/action"
      ],
      "notActions": [
        "Microsoft.DocumentDB/databaseAccounts/readonlyKeys/*",
        "Microsoft.DocumentDB/databaseAccounts/regenerateKey/*",
        "Microsoft.DocumentDB/databaseAccounts/listKeys/*",
        "Microsoft.DocumentDB/databaseAccounts/listConnectionStrings/*"
      ],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Cosmos DB Operator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="cosmosbackupoperator"></a>CosmosBackupOperator

Bir Cosmos DB veritabanı veya bir hesabın kapsayıcısı için geri yükleme isteği gönderebilir

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. DocumentDB/databaseAccounts/Backup/Action | Yedeklemeyi yapılandırmak için bir istek gönder |
> | Microsoft. DocumentDB/databaseAccounts/restore/Action | Geri yükleme isteği gönder |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can submit restore request for a Cosmos DB database or a container for an account",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/db7b14f2-5adf-42da-9f96-f2ee17bab5cb",
  "name": "db7b14f2-5adf-42da-9f96-f2ee17bab5cb",
  "permissions": [
    {
      "actions": [
        "Microsoft.DocumentDB/databaseAccounts/backup/action",
        "Microsoft.DocumentDB/databaseAccounts/restore/action"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "CosmosBackupOperator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="documentdb-account-contributor"></a>DocumentDB hesabı Katılımcısı

, Azure Cosmos DB hesaplarını yönetebilir. Azure Cosmos DB daha önce DocumentDB olarak bilinirdi.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. Authorization/*/Read | Rolleri ve rol atamalarını oku |
> | Microsoft. DocumentDb/databaseAccounts/* | Azure Cosmos DB hesapları oluşturma ve yönetme |
> | Microsoft. Insights/alertRules/* | Klasik ölçüm uyarısı oluşturma ve yönetme |
> | Microsoft. ResourceHealth/kullanılabilirlik Bilitydurumlar/okuma | Belirtilen kapsamdaki tüm kaynaklar için Kullanılabilirlik durumlarını alır |
> | Microsoft. resources/dağıtımlar/* | Dağıtım oluşturma ve yönetme |
> | Microsoft. resources/abonelikler/resourceGroups/Read | Kaynak gruplarını alır veya listeler. |
> | Microsoft. support/* | Destek bileti oluşturma ve güncelleştirme |
> | Microsoft. Network/virtualNetworks/alt ağlar/Jodavetli Aserviceendpoint/Action | Depolama hesabı veya SQL veritabanı gibi kaynağı bir alt ağa birleştirir. Alertable değil. |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage DocumentDB accounts, but not access to them.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/5bd9cd88-fe45-4216-938b-f97437e15450",
  "name": "5bd9cd88-fe45-4216-938b-f97437e15450",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.DocumentDb/databaseAccounts/*",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*",
        "Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/action"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "DocumentDB Account Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="redis-cache-contributor"></a>Katkıda bulunan Redis Cache

Redsıs önbellekler yönetmenize izin verir, ancak bunlara erişimi olmaz.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. Authorization/*/Read | Rolleri ve rol atamalarını oku |
> | Microsoft. Cache/reddir/* | Redsıs önbellekler oluşturma ve yönetme |
> | Microsoft. Insights/alertRules/* | Klasik ölçüm uyarısı oluşturma ve yönetme |
> | Microsoft. ResourceHealth/kullanılabilirlik Bilitydurumlar/okuma | Belirtilen kapsamdaki tüm kaynaklar için Kullanılabilirlik durumlarını alır |
> | Microsoft. resources/dağıtımlar/* | Dağıtım oluşturma ve yönetme |
> | Microsoft. resources/abonelikler/resourceGroups/Read | Kaynak gruplarını alır veya listeler. |
> | Microsoft. support/* | Destek bileti oluşturma ve güncelleştirme |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage Redis caches, but not access to them.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/e0f68234-74aa-48ed-b826-c38b57376e17",
  "name": "e0f68234-74aa-48ed-b826-c38b57376e17",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Cache/redis/*",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Redis Cache Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="sql-db-contributor"></a>SQL DB Katılımcısı

SQL veritabanlarını yönetmenizi sağlar ancak onlara yönelik erişimleri vermez. Ayrıca, güvenlikle ilgili ilkeleri veya bunların üst SQL sunucularını yönetemezsiniz.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. Authorization/*/Read | Rolleri ve rol atamalarını oku |
> | Microsoft. Insights/alertRules/* | Klasik ölçüm uyarısı oluşturma ve yönetme |
> | Microsoft. ResourceHealth/kullanılabilirlik Bilitydurumlar/okuma | Belirtilen kapsamdaki tüm kaynaklar için Kullanılabilirlik durumlarını alır |
> | Microsoft. resources/dağıtımlar/* | Dağıtım oluşturma ve yönetme |
> | Microsoft. resources/abonelikler/resourceGroups/Read | Kaynak gruplarını alır veya listeler. |
> | Microsoft. SQL/konumlar/*/Read |  |
> | Microsoft. SQL/Servers/veritabanları/* | SQL veritabanı oluşturma ve yönetme |
> | Microsoft. SQL/Servers/Read | Sunucu listesini döndürün veya belirtilen sunucunun özelliklerini alır. |
> | Microsoft. support/* | Destek bileti oluşturma ve güncelleştirme |
> | Microsoft. Insights/ölçümler/okuma | Ölçümleri oku |
> | Microsoft. Insights/metricDefinitions/okuma | Ölçüm tanımlarını oku |
> | **NotActions** |  |
> | Microsoft. SQL/ManagedInstances/Databases/currentSensitivityLabels/* |  |
> | Microsoft. SQL/ManagedInstances/Databases/recommendedSensitivityLabels/* |  |
> | Microsoft. SQL/ManagedInstances/veritabanları/şemalar/tablolar/sütunlar/sensitivityLabels/* |  |
> | Microsoft. SQL/ManagedInstances/Databases/Securityalcertpolicies/* |  |
> | Microsoft. SQL/ManagedInstances/Databases/sensitivityLabels/* |  |
> | Microsoft. SQL/ManagedInstances/veritabanları/ |  |
> | Microsoft. SQL/ManagedInstances/Securityalcertpolicies/* |  |
> | Microsoft. SQL/ManagedInstances/ |  |
> | Microsoft. SQL/Servers/veritabanları/auditingPolicies/* | Denetim ilkelerini düzenleme |
> | Microsoft. SQL/Servers/Databases/auditingSettings/* | Denetim ayarlarını Düzenle |
> | Microsoft. SQL/Servers/veritabanları/auditRecords/okuma | Veritabanı blobu denetim kayıtlarını alma |
> | Microsoft. SQL/Servers/veritabanları/connectionPolicies/* | Bağlantı ilkelerini düzenleme |
> | Microsoft. SQL/Servers/veritabanları/currentSensitivityLabels/* |  |
> | Microsoft. SQL/Servers/veritabanları/dataMaskingPolicies/* | Veri maskeleme ilkelerini düzenleme |
> | Microsoft. SQL/Servers/Databases/extendedAuditingSettings/* |  |
> | Microsoft. SQL/Servers/veritabanları/recommendedSensitivityLabels/* |  |
> | Microsoft. SQL/Servers/veritabanları/şemalar/tablolar/sütunlar/sensitivityLabels/* |  |
> | Microsoft. SQL/Servers/Databases/Securityalcertpolicies/* | Güvenlik Uyarısı ilkelerini düzenleme |
> | Microsoft. SQL/Servers/Databases/Securityölçümlerini/* | Güvenlik ölçümlerini düzenleme |
> | Microsoft. SQL/Servers/veritabanları/sensitivityLabels/* |  |
> | Microsoft. SQL/Servers/veritabanları/ |  |
> | Microsoft. SQL/Servers/veritabanları/ |  |
> | Microsoft. SQL/Servers/veritabanları/ |  |
> | Microsoft. SQL/Servers/ |  |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage SQL databases, but not access to them. Also, you can't manage their security-related policies or their parent SQL servers.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/9b7fa17d-e63e-47b0-bb0a-15c516ac86ec",
  "name": "9b7fa17d-e63e-47b0-bb0a-15c516ac86ec",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Sql/locations/*/read",
        "Microsoft.Sql/servers/databases/*",
        "Microsoft.Sql/servers/read",
        "Microsoft.Support/*",
        "Microsoft.Insights/metrics/read",
        "Microsoft.Insights/metricDefinitions/read"
      ],
      "notActions": [
        "Microsoft.Sql/managedInstances/databases/currentSensitivityLabels/*",
        "Microsoft.Sql/managedInstances/databases/recommendedSensitivityLabels/*",
        "Microsoft.Sql/managedInstances/databases/schemas/tables/columns/sensitivityLabels/*",
        "Microsoft.Sql/managedInstances/databases/securityAlertPolicies/*",
        "Microsoft.Sql/managedInstances/databases/sensitivityLabels/*",
        "Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/*",
        "Microsoft.Sql/managedInstances/securityAlertPolicies/*",
        "Microsoft.Sql/managedInstances/vulnerabilityAssessments/*",
        "Microsoft.Sql/servers/databases/auditingPolicies/*",
        "Microsoft.Sql/servers/databases/auditingSettings/*",
        "Microsoft.Sql/servers/databases/auditRecords/read",
        "Microsoft.Sql/servers/databases/connectionPolicies/*",
        "Microsoft.Sql/servers/databases/currentSensitivityLabels/*",
        "Microsoft.Sql/servers/databases/dataMaskingPolicies/*",
        "Microsoft.Sql/servers/databases/extendedAuditingSettings/*",
        "Microsoft.Sql/servers/databases/recommendedSensitivityLabels/*",
        "Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/*",
        "Microsoft.Sql/servers/databases/securityAlertPolicies/*",
        "Microsoft.Sql/servers/databases/securityMetrics/*",
        "Microsoft.Sql/servers/databases/sensitivityLabels/*",
        "Microsoft.Sql/servers/databases/vulnerabilityAssessments/*",
        "Microsoft.Sql/servers/databases/vulnerabilityAssessmentScans/*",
        "Microsoft.Sql/servers/databases/vulnerabilityAssessmentSettings/*",
        "Microsoft.Sql/servers/vulnerabilityAssessments/*"
      ],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "SQL DB Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="sql-managed-instance-contributor"></a>SQL yönetilen örnek Katılımcısı

SQL yönetilen örnekleri ve gerekli ağ yapılandırmasını yönetmenizi sağlar, ancak başkalarına erişim izni veremeyiz.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. ResourceHealth/kullanılabilirlik Bilitydurumlar/okuma | Belirtilen kapsamdaki tüm kaynaklar için Kullanılabilirlik durumlarını alır |
> | Microsoft. resources/dağıtımlar/* | Dağıtım oluşturma ve yönetme |
> | Microsoft. resources/abonelikler/resourceGroups/Read | Kaynak gruplarını alır veya listeler. |
> | Microsoft. Network/networkSecurityGroups/* |  |
> | Microsoft. Network/routeTables/* |  |
> | Microsoft. SQL/konumlar/*/Read |  |
> | Microsoft. SQL/ManagedInstances/* |  |
> | Microsoft. support/* | Destek bileti oluşturma ve güncelleştirme |
> | Microsoft. Network/virtualNetworks/alt ağları/* |  |
> | Microsoft. Network/virtualNetworks/* |  |
> | Microsoft. Authorization/*/Read | Rolleri ve rol atamalarını oku |
> | Microsoft. Insights/alertRules/* | Klasik ölçüm uyarısı oluşturma ve yönetme |
> | Microsoft. Insights/ölçümler/okuma | Ölçümleri oku |
> | Microsoft. Insights/metricDefinitions/okuma | Ölçüm tanımlarını oku |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage SQL Managed Instances and required network configuration, but can't give access to others.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/4939a1f6-9ae0-4e48-a1e0-f2cbe897382d",
  "name": "4939a1f6-9ae0-4e48-a1e0-f2cbe897382d",
  "permissions": [
    {
      "actions": [
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Network/networkSecurityGroups/*",
        "Microsoft.Network/routeTables/*",
        "Microsoft.Sql/locations/*/read",
        "Microsoft.Sql/managedInstances/*",
        "Microsoft.Support/*",
        "Microsoft.Network/virtualNetworks/subnets/*",
        "Microsoft.Network/virtualNetworks/*",
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Insights/metrics/read",
        "Microsoft.Insights/metricDefinitions/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "SQL Managed Instance Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="sql-security-manager"></a>SQL Güvenlik Yöneticisi

, SQL Server ve veritabanlarının güvenlikle ilgili ilkelerini yönetmenizi sağlar, ancak bunlara erişemez.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. Authorization/*/Read | Rolleri ve rol atamalarını oku |
> | Microsoft. Insights/alertRules/* | Klasik ölçüm uyarısı oluşturma ve yönetme |
> | Microsoft. Network/virtualNetworks/alt ağlar/Jodavetli Aserviceendpoint/Action | Depolama hesabı veya SQL veritabanı gibi kaynağı bir alt ağa birleştirir. Alertable değil. |
> | Microsoft. ResourceHealth/kullanılabilirlik Bilitydurumlar/okuma | Belirtilen kapsamdaki tüm kaynaklar için Kullanılabilirlik durumlarını alır |
> | Microsoft. resources/dağıtımlar/* | Dağıtım oluşturma ve yönetme |
> | Microsoft. resources/abonelikler/resourceGroups/Read | Kaynak gruplarını alır veya listeler. |
> | Microsoft. SQL/ManagedInstances/Databases/currentSensitivityLabels/* |  |
> | Microsoft. SQL/ManagedInstances/Databases/recommendedSensitivityLabels/* |  |
> | Microsoft. SQL/ManagedInstances/veritabanları/şemalar/tablolar/sütunlar/sensitivityLabels/* |  |
> | Microsoft. SQL/ManagedInstances/Databases/Securityalcertpolicies/* |  |
> | Microsoft. SQL/ManagedInstances/Databases/sensitivityLabels/* |  |
> | Microsoft. SQL/ManagedInstances/veritabanları/ |  |
> | Microsoft. SQL/ManagedInstances/Securityalcertpolicies/* |  |
> | Microsoft. SQL/ManagedInstances/Databases/transparentDataEncryption/* |  |
> | Microsoft. SQL/ManagedInstances/ |  |
> | Microsoft. SQL/Servers/auditingPolicies/* | SQL Server denetim ilkeleri oluşturma ve yönetme |
> | Microsoft. SQL/Servers/auditingSettings/* | SQL Server denetim ayarı oluşturma ve yönetme |
> | Microsoft. SQL/Servers/extendedAuditingSettings/Read | Belirli bir sunucuda yapılandırılmış genişletilmiş sunucu blobu denetim ilkesinin ayrıntılarını alma |
> | Microsoft. SQL/Servers/veritabanları/auditingPolicies/* | SQL Server veritabanı denetim ilkeleri oluşturma ve yönetme |
> | Microsoft. SQL/Servers/Databases/auditingSettings/* | SQL Server veritabanı denetim ayarlarını oluşturma ve yönetme |
> | Microsoft. SQL/Servers/veritabanları/auditRecords/okuma | Veritabanı blobu denetim kayıtlarını alma |
> | Microsoft. SQL/Servers/veritabanları/connectionPolicies/* | SQL Server veritabanı bağlantı ilkeleri oluşturma ve yönetme |
> | Microsoft. SQL/Servers/veritabanları/currentSensitivityLabels/* |  |
> | Microsoft. SQL/Servers/veritabanları/dataMaskingPolicies/* | SQL Server veritabanı veri maskeleme ilkeleri oluşturma ve yönetme |
> | Microsoft. SQL/Servers/veritabanları/extendedAuditingSettings/Read | Belirli bir veritabanında yapılandırılan genişletilmiş blob denetim ilkesinin ayrıntılarını alma |
> | Microsoft. SQL/Servers/veritabanları/okuma | Veritabanlarının listesini döndürün veya belirtilen veritabanının özelliklerini alır. |
> | Microsoft. SQL/Servers/veritabanları/recommendedSensitivityLabels/* |  |
> | Microsoft. SQL/Servers/veritabanları/şemalar/okuma | Veritabanı şeması alın. |
> | Microsoft. SQL/Servers/veritabanları/şemalar/tablolar/sütunlar/okuma | Veritabanı sütunu al. |
> | Microsoft. SQL/Servers/veritabanları/şemalar/tablolar/sütunlar/sensitivityLabels/* |  |
> | Microsoft. SQL/Servers/veritabanları/şemalar/tablolar/okuma | Veritabanı tablosu al. |
> | Microsoft. SQL/Servers/Databases/Securityalcertpolicies/* | SQL Server veritabanı güvenlik uyarısı ilkeleri oluşturma ve yönetme |
> | Microsoft. SQL/Servers/Databases/Securityölçümlerini/* | SQL Server veritabanı güvenlik ölçümleri oluşturma ve yönetme |
> | Microsoft. SQL/Servers/veritabanları/sensitivityLabels/* |  |
> | Microsoft. SQL/Servers/veritabanları/transparentDataEncryption/* |  |
> | Microsoft. SQL/Servers/veritabanları/ |  |
> | Microsoft. SQL/Servers/veritabanları/ |  |
> | Microsoft. SQL/Servers/veritabanları/ |  |
> | Microsoft. SQL/Servers/firewallRules/* |  |
> | Microsoft. SQL/Servers/Read | Sunucu listesini döndürün veya belirtilen sunucunun özelliklerini alır. |
> | Microsoft. SQL/Servers/Securityalcertpolicies/* | SQL Server güvenlik uyarı ilkeleri oluşturma ve yönetme |
> | Microsoft. SQL/Servers/ |  |
> | Microsoft. support/* | Destek bileti oluşturma ve güncelleştirme |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage the security-related policies of SQL servers and databases, but not access to them.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/056cd41c-7e88-42e1-933e-88ba6a50c9c3",
  "name": "056cd41c-7e88-42e1-933e-88ba6a50c9c3",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/action",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Sql/managedInstances/databases/currentSensitivityLabels/*",
        "Microsoft.Sql/managedInstances/databases/recommendedSensitivityLabels/*",
        "Microsoft.Sql/managedInstances/databases/schemas/tables/columns/sensitivityLabels/*",
        "Microsoft.Sql/managedInstances/databases/securityAlertPolicies/*",
        "Microsoft.Sql/managedInstances/databases/sensitivityLabels/*",
        "Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/*",
        "Microsoft.Sql/managedInstances/securityAlertPolicies/*",
        "Microsoft.Sql/managedInstances/databases/transparentDataEncryption/*",
        "Microsoft.Sql/managedInstances/vulnerabilityAssessments/*",
        "Microsoft.Sql/servers/auditingPolicies/*",
        "Microsoft.Sql/servers/auditingSettings/*",
        "Microsoft.Sql/servers/extendedAuditingSettings/read",
        "Microsoft.Sql/servers/databases/auditingPolicies/*",
        "Microsoft.Sql/servers/databases/auditingSettings/*",
        "Microsoft.Sql/servers/databases/auditRecords/read",
        "Microsoft.Sql/servers/databases/connectionPolicies/*",
        "Microsoft.Sql/servers/databases/currentSensitivityLabels/*",
        "Microsoft.Sql/servers/databases/dataMaskingPolicies/*",
        "Microsoft.Sql/servers/databases/extendedAuditingSettings/read",
        "Microsoft.Sql/servers/databases/read",
        "Microsoft.Sql/servers/databases/recommendedSensitivityLabels/*",
        "Microsoft.Sql/servers/databases/schemas/read",
        "Microsoft.Sql/servers/databases/schemas/tables/columns/read",
        "Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/*",
        "Microsoft.Sql/servers/databases/schemas/tables/read",
        "Microsoft.Sql/servers/databases/securityAlertPolicies/*",
        "Microsoft.Sql/servers/databases/securityMetrics/*",
        "Microsoft.Sql/servers/databases/sensitivityLabels/*",
        "Microsoft.Sql/servers/databases/transparentDataEncryption/*",
        "Microsoft.Sql/servers/databases/vulnerabilityAssessments/*",
        "Microsoft.Sql/servers/databases/vulnerabilityAssessmentScans/*",
        "Microsoft.Sql/servers/databases/vulnerabilityAssessmentSettings/*",
        "Microsoft.Sql/servers/firewallRules/*",
        "Microsoft.Sql/servers/read",
        "Microsoft.Sql/servers/securityAlertPolicies/*",
        "Microsoft.Sql/servers/vulnerabilityAssessments/*",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "SQL Security Manager",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="sql-server-contributor"></a>Katkıda bulunan SQL Server

SQL Server ve veritabanlarını yönetmenizi sağlar, ancak bunlara yönelik erişimleri ve güvenlikle ilgili ilkeleri yönetemez.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. Authorization/*/Read | Rolleri ve rol atamalarını oku |
> | Microsoft. Insights/alertRules/* | Klasik ölçüm uyarısı oluşturma ve yönetme |
> | Microsoft. ResourceHealth/kullanılabilirlik Bilitydurumlar/okuma | Belirtilen kapsamdaki tüm kaynaklar için Kullanılabilirlik durumlarını alır |
> | Microsoft. resources/dağıtımlar/* | Dağıtım oluşturma ve yönetme |
> | Microsoft. resources/abonelikler/resourceGroups/Read | Kaynak gruplarını alır veya listeler. |
> | Microsoft. SQL/konumlar/*/Read |  |
> | Microsoft. SQL/Servers/* | SQL Server 'lar oluşturma ve yönetme |
> | Microsoft. support/* | Destek bileti oluşturma ve güncelleştirme |
> | Microsoft. Insights/ölçümler/okuma | Ölçümleri oku |
> | Microsoft. Insights/metricDefinitions/okuma | Ölçüm tanımlarını oku |
> | **NotActions** |  |
> | Microsoft. SQL/ManagedInstances/Databases/currentSensitivityLabels/* |  |
> | Microsoft. SQL/ManagedInstances/Databases/recommendedSensitivityLabels/* |  |
> | Microsoft. SQL/ManagedInstances/veritabanları/şemalar/tablolar/sütunlar/sensitivityLabels/* |  |
> | Microsoft. SQL/ManagedInstances/Databases/Securityalcertpolicies/* |  |
> | Microsoft. SQL/ManagedInstances/Databases/sensitivityLabels/* |  |
> | Microsoft. SQL/ManagedInstances/veritabanları/ |  |
> | Microsoft. SQL/ManagedInstances/Securityalcertpolicies/* |  |
> | Microsoft. SQL/ManagedInstances/ |  |
> | Microsoft. SQL/Servers/auditingPolicies/* | SQL Server denetim ilkelerini düzenleme |
> | Microsoft. SQL/Servers/auditingSettings/* | SQL Server denetim ayarlarını Düzenle |
> | Microsoft. SQL/Servers/veritabanları/auditingPolicies/* | SQL Server veritabanı denetim ilkelerini düzenleme |
> | Microsoft. SQL/Servers/Databases/auditingSettings/* | SQL Server veritabanı denetim ayarlarını Düzenle |
> | Microsoft. SQL/Servers/veritabanları/auditRecords/okuma | Veritabanı blobu denetim kayıtlarını alma |
> | Microsoft. SQL/Servers/veritabanları/connectionPolicies/* | SQL Server veritabanı bağlantı ilkelerini düzenleme |
> | Microsoft. SQL/Servers/veritabanları/currentSensitivityLabels/* |  |
> | Microsoft. SQL/Servers/veritabanları/dataMaskingPolicies/* | SQL Server veritabanı veri maskeleme ilkelerini düzenleme |
> | Microsoft. SQL/Servers/Databases/extendedAuditingSettings/* |  |
> | Microsoft. SQL/Servers/veritabanları/recommendedSensitivityLabels/* |  |
> | Microsoft. SQL/Servers/veritabanları/şemalar/tablolar/sütunlar/sensitivityLabels/* |  |
> | Microsoft. SQL/Servers/Databases/Securityalcertpolicies/* | SQL Server veritabanı güvenlik uyarısı ilkelerini düzenleme |
> | Microsoft. SQL/Servers/Databases/Securityölçümlerini/* | SQL Server veritabanı güvenlik ölçümlerini düzenleme |
> | Microsoft. SQL/Servers/veritabanları/sensitivityLabels/* |  |
> | Microsoft. SQL/Servers/veritabanları/ |  |
> | Microsoft. SQL/Servers/veritabanları/ |  |
> | Microsoft. SQL/Servers/veritabanları/ |  |
> | Microsoft. SQL/Servers/extendedAuditingSettings/* |  |
> | Microsoft. SQL/Servers/Securityalcertpolicies/* | SQL Server güvenlik uyarısı ilkelerini düzenleme |
> | Microsoft. SQL/Servers/ |  |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage SQL servers and databases, but not access to them, and not their security -related policies.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/6d8ee4ec-f05a-4a1d-8b00-a9b17e38b437",
  "name": "6d8ee4ec-f05a-4a1d-8b00-a9b17e38b437",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Sql/locations/*/read",
        "Microsoft.Sql/servers/*",
        "Microsoft.Support/*",
        "Microsoft.Insights/metrics/read",
        "Microsoft.Insights/metricDefinitions/read"
      ],
      "notActions": [
        "Microsoft.Sql/managedInstances/databases/currentSensitivityLabels/*",
        "Microsoft.Sql/managedInstances/databases/recommendedSensitivityLabels/*",
        "Microsoft.Sql/managedInstances/databases/schemas/tables/columns/sensitivityLabels/*",
        "Microsoft.Sql/managedInstances/databases/securityAlertPolicies/*",
        "Microsoft.Sql/managedInstances/databases/sensitivityLabels/*",
        "Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/*",
        "Microsoft.Sql/managedInstances/securityAlertPolicies/*",
        "Microsoft.Sql/managedInstances/vulnerabilityAssessments/*",
        "Microsoft.Sql/servers/auditingPolicies/*",
        "Microsoft.Sql/servers/auditingSettings/*",
        "Microsoft.Sql/servers/databases/auditingPolicies/*",
        "Microsoft.Sql/servers/databases/auditingSettings/*",
        "Microsoft.Sql/servers/databases/auditRecords/read",
        "Microsoft.Sql/servers/databases/connectionPolicies/*",
        "Microsoft.Sql/servers/databases/currentSensitivityLabels/*",
        "Microsoft.Sql/servers/databases/dataMaskingPolicies/*",
        "Microsoft.Sql/servers/databases/extendedAuditingSettings/*",
        "Microsoft.Sql/servers/databases/recommendedSensitivityLabels/*",
        "Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/*",
        "Microsoft.Sql/servers/databases/securityAlertPolicies/*",
        "Microsoft.Sql/servers/databases/securityMetrics/*",
        "Microsoft.Sql/servers/databases/sensitivityLabels/*",
        "Microsoft.Sql/servers/databases/vulnerabilityAssessments/*",
        "Microsoft.Sql/servers/databases/vulnerabilityAssessmentScans/*",
        "Microsoft.Sql/servers/databases/vulnerabilityAssessmentSettings/*",
        "Microsoft.Sql/servers/extendedAuditingSettings/*",
        "Microsoft.Sql/servers/securityAlertPolicies/*",
        "Microsoft.Sql/servers/vulnerabilityAssessments/*"
      ],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "SQL Server Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

## <a name="analytics"></a>Analiz


### <a name="azure-event-hubs-data-owner"></a>Azure Event Hubs veri sahibi

Azure Event Hubs kaynaklarına tam erişim sağlar.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. EventHub/* |  |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | Microsoft. EventHub/* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for full access to Azure Event Hubs resources.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/f526a384-b230-433a-b45c-95f59c4a2dec",
  "name": "f526a384-b230-433a-b45c-95f59c4a2dec",
  "permissions": [
    {
      "actions": [
        "Microsoft.EventHub/*"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.EventHub/*"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Event Hubs Data Owner",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="azure-event-hubs-data-receiver"></a>Azure Event Hubs veri alıcısı

Azure Event Hubs kaynaklarına erişim izni verir.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. EventHub/*/eventhubs/consumergroups/Read |  |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | Microsoft. EventHub/*/Receive/Action |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows receive access to Azure Event Hubs resources.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/a638d3c7-ab3a-418d-83e6-5f17a39d4fde",
  "name": "a638d3c7-ab3a-418d-83e6-5f17a39d4fde",
  "permissions": [
    {
      "actions": [
        "Microsoft.EventHub/*/eventhubs/consumergroups/read"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.EventHub/*/receive/action"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Event Hubs Data Receiver",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="azure-event-hubs-data-sender"></a>Azure Event Hubs veri gönderici

Azure Event Hubs kaynaklarına erişim gönderilmesine izin verir.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. EventHub/*/eventhubs/Read |  |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | Microsoft. EventHub/*/Send/Action |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows send access to Azure Event Hubs resources.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/2b629674-e913-4c01-ae53-ef4638d8f975",
  "name": "2b629674-e913-4c01-ae53-ef4638d8f975",
  "permissions": [
    {
      "actions": [
        "Microsoft.EventHub/*/eventhubs/read"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.EventHub/*/send/action"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Event Hubs Data Sender",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="data-factory-contributor"></a>Katkıda bulunan Data Factory

Veri fabrikalarının yanı sıra bunların içindeki alt kaynakları oluşturun ve yönetin.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. Authorization/*/Read | Rolleri ve rol atamalarını oku |
> | Microsoft. DataFactory/DataFactory/* | Veri fabrikaları ve bunların içinde alt kaynaklar oluşturun ve yönetin. |
> | Microsoft. DataFactory/Factory/* | Veri fabrikaları ve bunların içinde alt kaynaklar oluşturun ve yönetin. |
> | Microsoft. Insights/alertRules/* | Klasik ölçüm uyarısı oluşturma ve yönetme |
> | Microsoft. ResourceHealth/kullanılabilirlik Bilitydurumlar/okuma | Belirtilen kapsamdaki tüm kaynaklar için Kullanılabilirlik durumlarını alır |
> | Microsoft. resources/dağıtımlar/* | Dağıtım oluşturma ve yönetme |
> | Microsoft. resources/abonelikler/resourceGroups/Read | Kaynak gruplarını alır veya listeler. |
> | Microsoft. support/* | Destek bileti oluşturma ve güncelleştirme |
> | Microsoft. EventGrid/Eventaboneliklerin/Write | EventSubscription oluşturma veya güncelleştirme |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Create and manage data factories, as well as child resources within them.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/673868aa-7521-48a0-acc6-0f60742d39f5",
  "name": "673868aa-7521-48a0-acc6-0f60742d39f5",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.DataFactory/dataFactories/*",
        "Microsoft.DataFactory/factories/*",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*",
        "Microsoft.EventGrid/eventSubscriptions/write"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Data Factory Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="data-purger"></a>Veri Takiger

Analiz verilerini temizedebilir

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. Insights/bileşenler/*/Read |  |
> | Microsoft. Insights/bileşenler/Temizleme/eylem | Application Insights verileri temizleme |
> | Microsoft. Operationalınsights/çalışma alanları/*/Read | Log Analytics verilerini görüntüleme |
> | Microsoft. Operationalınsights/çalışma alanları/temizleme/eylem | Belirtilen verileri çalışma alanından Sil |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can purge analytics data",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/150f5e0c-0603-4f03-8c7f-cf70034c4e90",
  "name": "150f5e0c-0603-4f03-8c7f-cf70034c4e90",
  "permissions": [
    {
      "actions": [
        "Microsoft.Insights/components/*/read",
        "Microsoft.Insights/components/purge/action",
        "Microsoft.OperationalInsights/workspaces/*/read",
        "Microsoft.OperationalInsights/workspaces/purge/action"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Data Purger",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="hdinsight-cluster-operator"></a>HDInsight küme operatörü

HDInsight küme yapılandırmasını okuyup değiştirmenize izin verir.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. HDInsight/*/Read |  |
> | Microsoft. HDInsight/kümeler/getGatewaySettings/Action | HDInsight kümesi için ağ geçidi ayarlarını al |
> | Microsoft. HDInsight/kümeler/updateGatewaySettings/Action | HDInsight kümesi için ağ geçidi ayarlarını güncelleştirme |
> | Microsoft. HDInsight/kümeler/konfigürasyonlar/* |  |
> | Microsoft. resources/abonelikler/resourceGroups/Read | Kaynak gruplarını alır veya listeler. |
> | Microsoft. resources/dağıtımlar/işlemler/okuma | Dağıtım işlemlerini alır veya listeler. |
> | Microsoft. Insights/alertRules/* | Klasik ölçüm uyarısı oluşturma ve yönetme |
> | Microsoft. Authorization/*/Read | Rolleri ve rol atamalarını oku |
> | Microsoft. support/* | Destek bileti oluşturma ve güncelleştirme |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you read and modify HDInsight cluster configurations.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/61ed4efc-fab3-44fd-b111-e24485cc132a",
  "name": "61ed4efc-fab3-44fd-b111-e24485cc132a",
  "permissions": [
    {
      "actions": [
        "Microsoft.HDInsight/*/read",
        "Microsoft.HDInsight/clusters/getGatewaySettings/action",
        "Microsoft.HDInsight/clusters/updateGatewaySettings/action",
        "Microsoft.HDInsight/clusters/configurations/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Resources/deployments/operations/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Authorization/*/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "HDInsight Cluster Operator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="hdinsight-domain-services-contributor"></a>HDInsight etki alanı Hizmetleri Katılımcısı

HDInsight için gereken etki alanı Hizmetleri ile ilgili işlemleri okuyabilir, oluşturabilir, değiştirebilir ve silebilir Kurumsal Güvenlik Paketi

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. AAD/*/Read |  |
> | Microsoft. AAD/domainServices/*/Read |  |
> | Microsoft. AAD/domainServices/oucontainer/* |  |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can Read, Create, Modify and Delete Domain Services related operations needed for HDInsight Enterprise Security Package",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/8d8d5a11-05d3-4bda-a417-a08778121c7c",
  "name": "8d8d5a11-05d3-4bda-a417-a08778121c7c",
  "permissions": [
    {
      "actions": [
        "Microsoft.AAD/*/read",
        "Microsoft.AAD/domainServices/*/read",
        "Microsoft.AAD/domainServices/oucontainer/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "HDInsight Domain Services Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="log-analytics-contributor"></a>Log Analytics Katkıda Bulunan

Log Analytics katkı, tüm izleme verilerini okuyabilir ve izleme ayarlarını düzenleyebilir. İzleme ayarlarını düzenlediğinizde VM 'lere VM uzantısının eklenmesi dahildir; Azure depolama 'dan günlüklerin toplanmasını yapılandırabilmek için depolama hesabı anahtarlarını okuma; Otomasyon hesapları oluşturma ve yapılandırma; çözümler ekleme; ve Azure tanılama 'yı tüm Azure kaynaklarında yapılandırma.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | */Read | Gizli dizileri hariç tüm türlerin kaynaklarını okuyun. |
> | Microsoft. Automation/automationAccounts/* |  |
> | Microsoft. ClassicCompute/virtualMachines/Extensions/* |  |
> | Microsoft. ClassicStorage/storageAccounts/listKeys/Action | Depolama hesaplarının erişim anahtarlarını listeler. |
> | Microsoft. COMPUTE/virtualMachines/Extensions/* |  |
> | Microsoft. HybridCompute/makineler/uzantılar/yazma | Bir Azure Arc uzantısını yükleme veya güncelleştirme |
> | Microsoft. Insights/alertRules/* | Klasik ölçüm uyarısı oluşturma ve yönetme |
> | Microsoft. Insights/diagnosticSettings/* | Analiz Sunucusu için tanılama ayarını oluşturur, güncelleştirir veya okur |
> | Microsoft. Operationalınsights/* |  |
> | Microsoft. OperationsManagement/* |  |
> | Microsoft. resources/dağıtımlar/* | Dağıtım oluşturma ve yönetme |
> | Microsoft. resources/abonelikler/ResourceGroups/dağıtımlar/* |  |
> | Microsoft. Storage/storageAccounts/listKeys/Action | Belirtilen depolama hesabı için erişim anahtarlarını döndürür. |
> | Microsoft. support/* | Destek bileti oluşturma ve güncelleştirme |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Log Analytics Contributor can read all monitoring data and edit monitoring settings. Editing monitoring settings includes adding the VM extension to VMs; reading storage account keys to be able to configure collection of logs from Azure Storage; creating and configuring Automation accounts; adding solutions; and configuring Azure diagnostics on all Azure resources.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/92aaf0da-9dab-42b6-94a3-d43ce8d16293",
  "name": "92aaf0da-9dab-42b6-94a3-d43ce8d16293",
  "permissions": [
    {
      "actions": [
        "*/read",
        "Microsoft.Automation/automationAccounts/*",
        "Microsoft.ClassicCompute/virtualMachines/extensions/*",
        "Microsoft.ClassicStorage/storageAccounts/listKeys/action",
        "Microsoft.Compute/virtualMachines/extensions/*",
        "Microsoft.HybridCompute/machines/extensions/write",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Insights/diagnosticSettings/*",
        "Microsoft.OperationalInsights/*",
        "Microsoft.OperationsManagement/*",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourcegroups/deployments/*",
        "Microsoft.Storage/storageAccounts/listKeys/action",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Log Analytics Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="log-analytics-reader"></a>Log Analytics Okuyucusu

Log Analytics okuyucu tüm izleme verilerini görüntüleyip arayabilir ve tüm Azure kaynaklarında Azure tanılama 'nın yapılandırılmasını görüntüleme dahil olmak üzere izleme ayarlarını görüntüleyebilir.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | */Read | Gizli dizileri hariç tüm türlerin kaynaklarını okuyun. |
> | Microsoft. Operationalınsights/çalışma alanları/analiz/sorgu/eylem | Yeni altyapıyı kullanarak arama yapın. |
> | Microsoft. Operationalınsights/çalışma alanları/arama/eylem | Arama sorgusu yürütür |
> | Microsoft. support/* | Destek bileti oluşturma ve güncelleştirme |
> | **NotActions** |  |
> | Microsoft. Operationalınsights/çalışma alanları/sharedKeys/Read | Çalışma alanının paylaşılan anahtarlarını alır. Bu anahtarlar, Microsoft operasyonel içgörüler aracılarını çalışma alanına bağlamak için kullanılır. |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Log Analytics Reader can view and search all monitoring data as well as and view monitoring settings, including viewing the configuration of Azure diagnostics on all Azure resources.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/73c42c96-874c-492b-b04d-ab87d138a893",
  "name": "73c42c96-874c-492b-b04d-ab87d138a893",
  "permissions": [
    {
      "actions": [
        "*/read",
        "Microsoft.OperationalInsights/workspaces/analytics/query/action",
        "Microsoft.OperationalInsights/workspaces/search/action",
        "Microsoft.Support/*"
      ],
      "notActions": [
        "Microsoft.OperationalInsights/workspaces/sharedKeys/read"
      ],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Log Analytics Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

## <a name="blockchain"></a>Blok zinciri


### <a name="blockchain-member-node-access-preview"></a>Blok zinciri üye düğümü erişimi (Önizleme)

Blok zinciri üye düğümlerine erişim sağlar

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. Blockzincirini/blockchainMembers/transactionNodes/Read | Varolan blok zinciri üye Işlem düğümlerini alır veya listeler. |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | Microsoft. Blockzincirini/blockchainMembers/transactionNodes/Connect/ACTION | Bir blok zinciri üye Işlem düğümüne bağlanır. |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for access to Blockchain Member nodes",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/31a002a1-acaf-453e-8a5b-297c9ca1ea24",
  "name": "31a002a1-acaf-453e-8a5b-297c9ca1ea24",
  "permissions": [
    {
      "actions": [
        "Microsoft.Blockchain/blockchainMembers/transactionNodes/read"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.Blockchain/blockchainMembers/transactionNodes/connect/action"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Blockchain Member Node Access (Preview)",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

## <a name="ai--machine-learning"></a>AI + makine öğrenimi


### <a name="cognitive-services-contributor"></a>Bilişsel hizmetler Katılımcısı

Bilişsel hizmetler için anahtar oluşturma, okuma, güncelleştirme, silme ve yönetme olanağı sağlar.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. Authorization/*/Read | Rolleri ve rol atamalarını oku |
> | Microsoft. Biliveservices/* |  |
> | Microsoft. Features/Features/Read | Bir aboneliğin özelliklerini alır. |
> | Microsoft. Özellikler/sağlayıcılar/Özellikler/okuma | Belirli bir kaynak sağlayıcısındaki bir aboneliğin özelliğini alır. |
> | Microsoft. Insights/alertRules/* | Klasik ölçüm uyarısı oluşturma ve yönetme |
> | Microsoft. Insights/diagnosticSettings/* | Analiz Sunucusu için tanılama ayarını oluşturur, güncelleştirir veya okur |
> | Microsoft. Insights/logDefinitions/okuma | Günlük tanımlarını oku |
> | Microsoft. Insights/MetricDefinitions/okuma | Ölçüm tanımlarını oku |
> | Microsoft. Insights/ölçümler/okuma | Ölçümleri oku |
> | Microsoft. ResourceHealth/kullanılabilirlik Bilitydurumlar/okuma | Belirtilen kapsamdaki tüm kaynaklar için Kullanılabilirlik durumlarını alır |
> | Microsoft. resources/dağıtımlar/* | Dağıtım oluşturma ve yönetme |
> | Microsoft. resources/dağıtımlar/işlemler/okuma | Dağıtım işlemlerini alır veya listeler. |
> | Microsoft. resources/abonelikler/operationresults/Read | Abonelik işlem sonuçlarını alın. |
> | Microsoft. resources/abonelikler/okuma | Aboneliklerin listesini alır. |
> | Microsoft. resources/abonelikler/ResourceGroups/dağıtımlar/* |  |
> | Microsoft. resources/abonelikler/resourceGroups/Read | Kaynak gruplarını alır veya listeler. |
> | Microsoft. support/* | Destek bileti oluşturma ve güncelleştirme |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you create, read, update, delete and manage keys of Cognitive Services.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/25fbc0a9-bd7c-42a3-aa1a-3b75d497ee68",
  "name": "25fbc0a9-bd7c-42a3-aa1a-3b75d497ee68",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.CognitiveServices/*",
        "Microsoft.Features/features/read",
        "Microsoft.Features/providers/features/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Insights/diagnosticSettings/*",
        "Microsoft.Insights/logDefinitions/read",
        "Microsoft.Insights/metricdefinitions/read",
        "Microsoft.Insights/metrics/read",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/deployments/operations/read",
        "Microsoft.Resources/subscriptions/operationresults/read",
        "Microsoft.Resources/subscriptions/read",
        "Microsoft.Resources/subscriptions/resourcegroups/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Cognitive Services Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="cognitive-services-data-reader-preview"></a>Bilişsel hizmetler veri okuyucu (Önizleme)

Bilişsel hizmetler verilerini okumanızı sağlar.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | *yok* |  |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | Microsoft. Biliveservices/*/Read |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you read Cognitive Services data.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/b59867f0-fa02-499b-be73-45a86b5b3e1c",
  "name": "b59867f0-fa02-499b-be73-45a86b5b3e1c",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.CognitiveServices/*/read"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Cognitive Services Data Reader (Preview)",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="cognitive-services-user"></a>Bilişsel hizmetler kullanıcısı

Bilişsel hizmetler 'in anahtarlarını okuyup listelemenizi sağlar.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. Biliveservices/*/Read |  |
> | Microsoft. Biliveservices/accounts/ListKeys/Action | Anahtarları Listele |
> | Microsoft. Insights/alertRules/Read | Klasik ölçüm uyarısını okuyun |
> | Microsoft. Insights/diagnosticSettings/Read | Kaynak tanılama ayarını oku |
> | Microsoft. Insights/logDefinitions/okuma | Günlük tanımlarını oku |
> | Microsoft. Insights/MetricDefinitions/okuma | Ölçüm tanımlarını oku |
> | Microsoft. Insights/ölçümler/okuma | Ölçümleri oku |
> | Microsoft. ResourceHealth/kullanılabilirlik Bilitydurumlar/okuma | Belirtilen kapsamdaki tüm kaynaklar için Kullanılabilirlik durumlarını alır |
> | Microsoft. resources/dağıtımlar/işlemler/okuma | Dağıtım işlemlerini alır veya listeler. |
> | Microsoft. resources/abonelikler/operationresults/Read | Abonelik işlem sonuçlarını alın. |
> | Microsoft. resources/abonelikler/okuma | Aboneliklerin listesini alır. |
> | Microsoft. resources/abonelikler/resourceGroups/Read | Kaynak gruplarını alır veya listeler. |
> | Microsoft. support/* | Destek bileti oluşturma ve güncelleştirme |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | Microsoft. Biliveservices/* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you read and list keys of Cognitive Services.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/a97b65f3-24c7-4388-baec-2e87135dc908",
  "name": "a97b65f3-24c7-4388-baec-2e87135dc908",
  "permissions": [
    {
      "actions": [
        "Microsoft.CognitiveServices/*/read",
        "Microsoft.CognitiveServices/accounts/listkeys/action",
        "Microsoft.Insights/alertRules/read",
        "Microsoft.Insights/diagnosticSettings/read",
        "Microsoft.Insights/logDefinitions/read",
        "Microsoft.Insights/metricdefinitions/read",
        "Microsoft.Insights/metrics/read",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/operations/read",
        "Microsoft.Resources/subscriptions/operationresults/read",
        "Microsoft.Resources/subscriptions/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.CognitiveServices/*"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Cognitive Services User",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

## <a name="mixed-reality"></a>Karma gerçeklik


### <a name="spatial-anchors-account-contributor"></a>Uzamsal bağlayıcı hesabı Katılımcısı

Hesabınızdaki uzamsal bağlantıları yönetmenizi sağlar, ancak onları silmez

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | *yok* |  |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | Microsoft. MixedReality/SpatialAnchorsAccounts/Create/ACTION | Uzamsal bağlayıcı oluşturma |
> | Microsoft. MixedReality/SpatialAnchorsAccounts/Discovery/Read | Yakın uzamsal bağlayıcıları bul |
> | Microsoft. MixedReality/SpatialAnchorsAccounts/Özellikler/okuma | Uzamsal Tutturucuların özelliklerini al |
> | Microsoft. MixedReality/SpatialAnchorsAccounts/sorgu/okuma | Uzamsal bağlayıcıları bul |
> | Microsoft. MixedReality/SpatialAnchorsAccounts/submitdiag/Read | Azure uzamsal bağlayıcı hizmeti 'nin kalitesini artırmaya yardımcı olmak için tanılama verileri gönderme |
> | Microsoft. MixedReality/SpatialAnchorsAccounts/Write | Uzamsal Tutturucuların özelliklerini güncelleştirme |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage spatial anchors in your account, but not delete them",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/8bbe83f1-e2a6-4df7-8cb4-4e04d4e5c827",
  "name": "8bbe83f1-e2a6-4df7-8cb4-4e04d4e5c827",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.MixedReality/SpatialAnchorsAccounts/create/action",
        "Microsoft.MixedReality/SpatialAnchorsAccounts/discovery/read",
        "Microsoft.MixedReality/SpatialAnchorsAccounts/properties/read",
        "Microsoft.MixedReality/SpatialAnchorsAccounts/query/read",
        "Microsoft.MixedReality/SpatialAnchorsAccounts/submitdiag/read",
        "Microsoft.MixedReality/SpatialAnchorsAccounts/write"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Spatial Anchors Account Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="spatial-anchors-account-owner"></a>Uzamsal bağlayıcı hesap sahibi

Hesabınızdaki uzamsal bağlayıcıları, silme dahil olmak üzere yönetmenizi sağlar

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | *yok* |  |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | Microsoft. MixedReality/SpatialAnchorsAccounts/Create/ACTION | Uzamsal bağlayıcı oluşturma |
> | Microsoft. MixedReality/SpatialAnchorsAccounts/Delete | Uzamsal bağlayıcıları Sil |
> | Microsoft. MixedReality/SpatialAnchorsAccounts/Discovery/Read | Yakın uzamsal bağlayıcıları bul |
> | Microsoft. MixedReality/SpatialAnchorsAccounts/Özellikler/okuma | Uzamsal Tutturucuların özelliklerini al |
> | Microsoft. MixedReality/SpatialAnchorsAccounts/sorgu/okuma | Uzamsal bağlayıcıları bul |
> | Microsoft. MixedReality/SpatialAnchorsAccounts/submitdiag/Read | Azure uzamsal bağlayıcı hizmeti 'nin kalitesini artırmaya yardımcı olmak için tanılama verileri gönderme |
> | Microsoft. MixedReality/SpatialAnchorsAccounts/Write | Uzamsal Tutturucuların özelliklerini güncelleştirme |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage spatial anchors in your account, including deleting them",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/70bbe301-9835-447d-afdd-19eb3167307c",
  "name": "70bbe301-9835-447d-afdd-19eb3167307c",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.MixedReality/SpatialAnchorsAccounts/create/action",
        "Microsoft.MixedReality/SpatialAnchorsAccounts/delete",
        "Microsoft.MixedReality/SpatialAnchorsAccounts/discovery/read",
        "Microsoft.MixedReality/SpatialAnchorsAccounts/properties/read",
        "Microsoft.MixedReality/SpatialAnchorsAccounts/query/read",
        "Microsoft.MixedReality/SpatialAnchorsAccounts/submitdiag/read",
        "Microsoft.MixedReality/SpatialAnchorsAccounts/write"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Spatial Anchors Account Owner",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="spatial-anchors-account-reader"></a>Uzamsal bağlayıcı hesap okuyucu

Hesabınızdaki uzamsal Tutturucuların özelliklerini bulmanıza ve okumanızı sağlar

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | *yok* |  |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | Microsoft. MixedReality/SpatialAnchorsAccounts/Discovery/Read | Yakın uzamsal bağlayıcıları bul |
> | Microsoft. MixedReality/SpatialAnchorsAccounts/Özellikler/okuma | Uzamsal Tutturucuların özelliklerini al |
> | Microsoft. MixedReality/SpatialAnchorsAccounts/sorgu/okuma | Uzamsal bağlayıcıları bul |
> | Microsoft. MixedReality/SpatialAnchorsAccounts/submitdiag/Read | Azure uzamsal bağlayıcı hizmeti 'nin kalitesini artırmaya yardımcı olmak için tanılama verileri gönderme |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you locate and read properties of spatial anchors in your account",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/5d51204f-eb77-4b1c-b86a-2ec626c49413",
  "name": "5d51204f-eb77-4b1c-b86a-2ec626c49413",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.MixedReality/SpatialAnchorsAccounts/discovery/read",
        "Microsoft.MixedReality/SpatialAnchorsAccounts/properties/read",
        "Microsoft.MixedReality/SpatialAnchorsAccounts/query/read",
        "Microsoft.MixedReality/SpatialAnchorsAccounts/submitdiag/read"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Spatial Anchors Account Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

## <a name="integration"></a>Tümleştirme


### <a name="api-management-service-contributor"></a>API Management hizmet Katılımcısı

Hizmeti ve API 'Leri yönetebilir

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. Apimanan/hizmet/* | API Management hizmeti oluşturma ve yönetme |
> | Microsoft. Authorization/*/Read | Rolleri ve rol atamalarını oku |
> | Microsoft. Insights/alertRules/* | Klasik ölçüm uyarısı oluşturma ve yönetme |
> | Microsoft. ResourceHealth/kullanılabilirlik Bilitydurumlar/okuma | Belirtilen kapsamdaki tüm kaynaklar için Kullanılabilirlik durumlarını alır |
> | Microsoft. resources/dağıtımlar/* | Dağıtım oluşturma ve yönetme |
> | Microsoft. resources/abonelikler/resourceGroups/Read | Kaynak gruplarını alır veya listeler. |
> | Microsoft. support/* | Destek bileti oluşturma ve güncelleştirme |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can manage service and the APIs",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/312a565d-c81f-4fd8-895a-4e21e48d571c",
  "name": "312a565d-c81f-4fd8-895a-4e21e48d571c",
  "permissions": [
    {
      "actions": [
        "Microsoft.ApiManagement/service/*",
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "API Management Service Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="api-management-service-operator-role"></a>Hizmet Işletmeni rolü API Management

Hizmeti yönetebilir, ancak API 'Leri yönetemez

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. Apimanayönetimi/hizmeti/*/Read | API Management hizmeti örneklerini oku |
> | Microsoft. Apimanayönetimi/hizmeti/yedekleme/eylem | API Management hizmetini Kullanıcı tarafından sağlanmış bir depolama hesabında belirtilen kapsayıcıya Yedekle |
> | Microsoft. Apimanayönetimi/hizmeti/silme | API Management hizmeti örneğini Sil |
> | Microsoft. Apimanayönetimi/hizmet/managedağıtımlar/eylem | SKU/birimleri değiştirme, API Management hizmetinin bölgesel dağıtımlarını ekleme/kaldırma |
> | Microsoft. Apimanayönetimi/hizmeti/okuma | API Management hizmet örneği için meta verileri oku |
> | Microsoft. Apimanayönetimi/hizmeti/geri yükleme/eylem | API Management hizmetini Kullanıcı tarafından sağlanmış bir depolama hesabındaki belirtilen kapsayıcıdan geri yükle |
> | Microsoft. Apimanayönetimi/hizmet/updatecertificate/Action | API Management hizmeti için TLS/SSL sertifikası yükleme |
> | Microsoft. Apimanaya/Service/updatehostname/Action | API Management hizmeti için özel etki alanı adlarını kurma, güncelleştirme veya kaldırma |
> | Microsoft. Apimanayönetimi/hizmeti/yazma | API Management hizmet örneği oluştur veya güncelleştir |
> | Microsoft. Authorization/*/Read | Rolleri ve rol atamalarını oku |
> | Microsoft. Insights/alertRules/* | Klasik ölçüm uyarısı oluşturma ve yönetme |
> | Microsoft. ResourceHealth/kullanılabilirlik Bilitydurumlar/okuma | Belirtilen kapsamdaki tüm kaynaklar için Kullanılabilirlik durumlarını alır |
> | Microsoft. resources/dağıtımlar/* | Dağıtım oluşturma ve yönetme |
> | Microsoft. resources/abonelikler/resourceGroups/Read | Kaynak gruplarını alır veya listeler. |
> | Microsoft. support/* | Destek bileti oluşturma ve güncelleştirme |
> | **NotActions** |  |
> | Microsoft. Apimanayönetimi/hizmeti/kullanıcıları/anahtarları/okuma | Kullanıcıyla ilişkili anahtarları al |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can manage service but not the APIs",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/e022efe7-f5ba-4159-bbe4-b44f577e9b61",
  "name": "e022efe7-f5ba-4159-bbe4-b44f577e9b61",
  "permissions": [
    {
      "actions": [
        "Microsoft.ApiManagement/service/*/read",
        "Microsoft.ApiManagement/service/backup/action",
        "Microsoft.ApiManagement/service/delete",
        "Microsoft.ApiManagement/service/managedeployments/action",
        "Microsoft.ApiManagement/service/read",
        "Microsoft.ApiManagement/service/restore/action",
        "Microsoft.ApiManagement/service/updatecertificate/action",
        "Microsoft.ApiManagement/service/updatehostname/action",
        "Microsoft.ApiManagement/service/write",
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [
        "Microsoft.ApiManagement/service/users/keys/read"
      ],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "API Management Service Operator Role",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="api-management-service-reader-role"></a>API Management hizmeti okuyucu rolü

Hizmet ve API 'lere salt okuma erişimi

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. Apimanayönetimi/hizmeti/*/Read | API Management hizmeti örneklerini oku |
> | Microsoft. Apimanayönetimi/hizmeti/okuma | API Management hizmet örneği için meta verileri oku |
> | Microsoft. Authorization/*/Read | Rolleri ve rol atamalarını oku |
> | Microsoft. Insights/alertRules/* | Klasik ölçüm uyarısı oluşturma ve yönetme |
> | Microsoft. ResourceHealth/kullanılabilirlik Bilitydurumlar/okuma | Belirtilen kapsamdaki tüm kaynaklar için Kullanılabilirlik durumlarını alır |
> | Microsoft. resources/dağıtımlar/* | Dağıtım oluşturma ve yönetme |
> | Microsoft. resources/abonelikler/resourceGroups/Read | Kaynak gruplarını alır veya listeler. |
> | Microsoft. support/* | Destek bileti oluşturma ve güncelleştirme |
> | **NotActions** |  |
> | Microsoft. Apimanayönetimi/hizmeti/kullanıcıları/anahtarları/okuma | Kullanıcıyla ilişkili anahtarları al |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Read-only access to service and APIs",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/71522526-b88f-4d52-b57f-d31fc3546d0d",
  "name": "71522526-b88f-4d52-b57f-d31fc3546d0d",
  "permissions": [
    {
      "actions": [
        "Microsoft.ApiManagement/service/*/read",
        "Microsoft.ApiManagement/service/read",
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [
        "Microsoft.ApiManagement/service/users/keys/read"
      ],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "API Management Service Reader Role",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="app-configuration-data-owner"></a>Uygulama yapılandırma veri sahibi

Uygulama yapılandırma verilerine tam erişim sağlar.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | *yok* |  |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | Microsoft. AppConfiguration/Configurationmağazaları/*/Read |  |
> | Microsoft. AppConfiguration/Configurationmağazaları/*/Write |  |
> | Microsoft. AppConfiguration/Configurationmağazaların/*/Delete |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows full access to App Configuration data.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/5ae67dd6-50cb-40e7-96ff-dc2bfa4b606b",
  "name": "5ae67dd6-50cb-40e7-96ff-dc2bfa4b606b",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.AppConfiguration/configurationStores/*/read",
        "Microsoft.AppConfiguration/configurationStores/*/write",
        "Microsoft.AppConfiguration/configurationStores/*/delete"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "App Configuration Data Owner",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="app-configuration-data-reader"></a>Uygulama yapılandırma veri okuyucusu

Uygulama yapılandırma verilerine okuma erişimi sağlar.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | *yok* |  |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | Microsoft. AppConfiguration/Configurationmağazaları/*/Read |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows read access to App Configuration data.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/516239f1-63e1-4d78-a4de-a74fb236a071",
  "name": "516239f1-63e1-4d78-a4de-a74fb236a071",
  "permissions": [
    {
      "actions": [],
      "notActions": [],
      "dataActions": [
        "Microsoft.AppConfiguration/configurationStores/*/read"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "App Configuration Data Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="azure-service-bus-data-owner"></a>Azure Service Bus veri sahibi

Azure Service Bus kaynaklara tam erişim sağlar.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. ServiceBus/* |  |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | Microsoft. ServiceBus/* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for full access to Azure Service Bus resources.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/090c5cfd-751d-490a-894a-3ce6f1109419",
  "name": "090c5cfd-751d-490a-894a-3ce6f1109419",
  "permissions": [
    {
      "actions": [
        "Microsoft.ServiceBus/*"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.ServiceBus/*"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Service Bus Data Owner",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="azure-service-bus-data-receiver"></a>Azure Service Bus veri alıcısı

Azure Service Bus kaynaklarına erişim izni verir.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. ServiceBus/*/Queues/Read |  |
> | Microsoft. ServiceBus/*/topics/Read |  |
> | Microsoft. ServiceBus/*/topics/Subscriptions/Read |  |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | Microsoft. ServiceBus/*/Receive/Action |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for receive access to Azure Service Bus resources.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/4f6d3b9b-027b-4f4c-9142-0e5a2a2247e0",
  "name": "4f6d3b9b-027b-4f4c-9142-0e5a2a2247e0",
  "permissions": [
    {
      "actions": [
        "Microsoft.ServiceBus/*/queues/read",
        "Microsoft.ServiceBus/*/topics/read",
        "Microsoft.ServiceBus/*/topics/subscriptions/read"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.ServiceBus/*/receive/action"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Service Bus Data Receiver",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="azure-service-bus-data-sender"></a>Veri Göndericisini Azure Service Bus

Azure Service Bus kaynaklarına erişim izni verir.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. ServiceBus/*/Queues/Read |  |
> | Microsoft. ServiceBus/*/topics/Read |  |
> | Microsoft. ServiceBus/*/topics/Subscriptions/Read |  |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | Microsoft. ServiceBus/*/Send/Action |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for send access to Azure Service Bus resources.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/69a216fc-b8fb-44d8-bc22-1f3c2cd27a39",
  "name": "69a216fc-b8fb-44d8-bc22-1f3c2cd27a39",
  "permissions": [
    {
      "actions": [
        "Microsoft.ServiceBus/*/queues/read",
        "Microsoft.ServiceBus/*/topics/read",
        "Microsoft.ServiceBus/*/topics/subscriptions/read"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.ServiceBus/*/send/action"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Service Bus Data Sender",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="azure-stack-registration-owner"></a>Kayıt sahibini Azure Stack

Azure Stack kayıtlarını yönetmenizi sağlar.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. AzureStack/kayıtlar/ürünler/*/Action |  |
> | Microsoft. AzureStack/kayıtlar/ürünler/okuma | Azure Stack Market ürününün özelliklerini alır |
> | Microsoft. AzureStack/kayıtlar/okuma | Azure Stack kaydın özelliklerini alır |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage Azure Stack registrations.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/6f12a6df-dd06-4f3e-bcb1-ce8be600526a",
  "name": "6f12a6df-dd06-4f3e-bcb1-ce8be600526a",
  "permissions": [
    {
      "actions": [
        "Microsoft.AzureStack/registrations/products/*/action",
        "Microsoft.AzureStack/registrations/products/read",
        "Microsoft.AzureStack/registrations/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Stack Registration Owner",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="eventgrid-eventsubscription-contributor"></a>EventGrid EventSubscription Katılımcısı

EventGrid olay aboneliği işlemlerini yönetmenizi sağlar.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. Authorization/*/Read | Rolleri ve rol atamalarını oku |
> | Microsoft. EventGrid/Eventabonelikleriniz/* |  |
> | Microsoft. EventGrid/topicTypes/Eventaboneliklerde/okunan | Küresel olay aboneliklerini konu türüne göre Listele |
> | Microsoft. EventGrid/Locations/Eventaboneliklerde/okunan | Bölgesel olay aboneliklerini listeleme |
> | Microsoft. EventGrid/Locations/topicTypes/Eventabonelikleri/okuma | TopicType 'a göre bölgesel olay aboneliklerini listeleyin |
> | Microsoft. Insights/alertRules/* | Klasik ölçüm uyarısı oluşturma ve yönetme |
> | Microsoft. resources/dağıtımlar/* | Dağıtım oluşturma ve yönetme |
> | Microsoft. resources/abonelikler/resourceGroups/Read | Kaynak gruplarını alır veya listeler. |
> | Microsoft. support/* | Destek bileti oluşturma ve güncelleştirme |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage EventGrid event subscription operations.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/428e0ff0-5e57-4d9c-a221-2c70d0e0a443",
  "name": "428e0ff0-5e57-4d9c-a221-2c70d0e0a443",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.EventGrid/eventSubscriptions/*",
        "Microsoft.EventGrid/topicTypes/eventSubscriptions/read",
        "Microsoft.EventGrid/locations/eventSubscriptions/read",
        "Microsoft.EventGrid/locations/topicTypes/eventSubscriptions/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "EventGrid EventSubscription Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="eventgrid-eventsubscription-reader"></a>EventGrid EventSubscription okuyucusu

EventGrid olay aboneliklerini okumanızı sağlar.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. Authorization/*/Read | Rolleri ve rol atamalarını oku |
> | Microsoft. EventGrid/Eventaboneliklerde/okunan | Bir eventSubscription okuma |
> | Microsoft. EventGrid/topicTypes/Eventaboneliklerde/okunan | Küresel olay aboneliklerini konu türüne göre Listele |
> | Microsoft. EventGrid/Locations/Eventaboneliklerde/okunan | Bölgesel olay aboneliklerini listeleme |
> | Microsoft. EventGrid/Locations/topicTypes/Eventabonelikleri/okuma | TopicType 'a göre bölgesel olay aboneliklerini listeleyin |
> | Microsoft. resources/abonelikler/resourceGroups/Read | Kaynak gruplarını alır veya listeler. |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you read EventGrid event subscriptions.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/2414bbcf-6497-4faf-8c65-045460748405",
  "name": "2414bbcf-6497-4faf-8c65-045460748405",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.EventGrid/eventSubscriptions/read",
        "Microsoft.EventGrid/topicTypes/eventSubscriptions/read",
        "Microsoft.EventGrid/locations/eventSubscriptions/read",
        "Microsoft.EventGrid/locations/topicTypes/eventSubscriptions/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "EventGrid EventSubscription Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="intelligent-systems-account-contributor"></a>Intelligent Systems hesabı Katılımcısı

Akıllı sistem hesaplarını yönetmenizi sağlar ancak onlara yönelik erişimleri vermez.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. Authorization/*/Read | Rolleri ve rol atamalarını oku |
> | Microsoft. Insights/alertRules/* | Klasik ölçüm uyarısı oluşturma ve yönetme |
> | Microsoft. ıntelligentsystems/accounts/* | Akıllı sistem hesapları oluşturma ve yönetme |
> | Microsoft. ResourceHealth/kullanılabilirlik Bilitydurumlar/okuma | Belirtilen kapsamdaki tüm kaynaklar için Kullanılabilirlik durumlarını alır |
> | Microsoft. resources/dağıtımlar/* | Dağıtım oluşturma ve yönetme |
> | Microsoft. resources/abonelikler/resourceGroups/Read | Kaynak gruplarını alır veya listeler. |
> | Microsoft. support/* | Destek bileti oluşturma ve güncelleştirme |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage Intelligent Systems accounts, but not access to them.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/03a6d094-3444-4b3d-88af-7477090a9e5e",
  "name": "03a6d094-3444-4b3d-88af-7477090a9e5e",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.IntelligentSystems/accounts/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Intelligent Systems Account Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="logic-app-contributor"></a>Mantıksal uygulama Katılımcısı

Mantıksal uygulamaları yönetmenize izin verir, ancak bunlara erişimi değiştirmeyin.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. Authorization/*/Read | Rolleri ve rol atamalarını oku |
> | Microsoft. ClassicStorage/storageAccounts/listKeys/Action | Depolama hesaplarının erişim anahtarlarını listeler. |
> | Microsoft. ClassicStorage/storageAccounts/Read | Verilen hesaba sahip depolama hesabını döndürün. |
> | Microsoft. Insights/alertRules/* | Klasik ölçüm uyarısı oluşturma ve yönetme |
> | Microsoft. Insights/metricAlerts/* |  |
> | Microsoft. Insights/diagnosticSettings/* | Analiz Sunucusu için tanılama ayarını oluşturur, güncelleştirir veya okur |
> | Microsoft. Insights/logdefinitions/* | Bu izin, Portal aracılığıyla etkinlik günlüklerine erişmesi gereken kullanıcılar için gereklidir. Etkinlik günlüğündeki günlük kategorilerini listeleyin. |
> | Microsoft. Insights/metricDefinitions/* | Ölçüm tanımlarını oku (bir kaynak için kullanılabilen ölçüm türlerinin listesi). |
> | Microsoft. Logic/* | Logic Apps kaynaklarını yönetir. |
> | Microsoft. resources/dağıtımlar/* | Dağıtım oluşturma ve yönetme |
> | Microsoft. resources/abonelikler/operationresults/Read | Abonelik işlem sonuçlarını alın. |
> | Microsoft. resources/abonelikler/resourceGroups/Read | Kaynak gruplarını alır veya listeler. |
> | Microsoft. Storage/storageAccounts/ListKeys/Action | Belirtilen depolama hesabı için erişim anahtarlarını döndürür. |
> | Microsoft. Storage/storageAccounts/Read | Depolama hesaplarının listesini döndürür veya belirtilen depolama hesabının özelliklerini alır. |
> | Microsoft. support/* | Destek bileti oluşturma ve güncelleştirme |
> | Microsoft. Web/Connectiongateway/* | Bir bağlantı ağ geçidi oluşturun ve yönetir. |
> | Microsoft. Web/Connections/* | Bir bağlantı oluşturun ve yönetir. |
> | Microsoft. Web/Customapsıs/* | Özel bir API oluşturur ve yönetir. |
> | Microsoft. Web/Sunucugrupları/JOIN/Action |  |
> | Microsoft. Web/Sunucugrupları/okuma | App Service planında özellikleri al |
> | Microsoft. Web/Sites/Functions/Listgizlilikler/Action | Işlev gizli dizileri listeleyin. |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage logic app, but not access to them.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/87a39d53-fc1b-424a-814c-f7e04687dc9e",
  "name": "87a39d53-fc1b-424a-814c-f7e04687dc9e",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.ClassicStorage/storageAccounts/listKeys/action",
        "Microsoft.ClassicStorage/storageAccounts/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Insights/metricAlerts/*",
        "Microsoft.Insights/diagnosticSettings/*",
        "Microsoft.Insights/logdefinitions/*",
        "Microsoft.Insights/metricDefinitions/*",
        "Microsoft.Logic/*",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/operationresults/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Storage/storageAccounts/listkeys/action",
        "Microsoft.Storage/storageAccounts/read",
        "Microsoft.Support/*",
        "Microsoft.Web/connectionGateways/*",
        "Microsoft.Web/connections/*",
        "Microsoft.Web/customApis/*",
        "Microsoft.Web/serverFarms/join/action",
        "Microsoft.Web/serverFarms/read",
        "Microsoft.Web/sites/functions/listSecrets/action"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Logic App Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="logic-app-operator"></a>Logic App Işleci

Logic Apps 'i okumanızı, etkinleştirmenizi ve devre dışı bırakmanızı sağlar, ancak bunları düzenleyemez veya güncelleştiremez.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. Authorization/*/Read | Rolleri ve rol atamalarını oku |
> | Microsoft. Insights/alertRules/*/Read | Öngörüler uyarı kurallarını okuyun |
> | Microsoft. Insights/metricAlerts/*/Read |  |
> | Microsoft. Insights/diagnosticSettings/*/Read | Logic Apps için tanılama ayarlarını alır |
> | Microsoft. Insights/metricDefinitions/*/Read | Logic Apps için kullanılabilir ölçümleri alır. |
> | Microsoft. Logic/*/Read | Logic Apps kaynaklarını okur. |
> | Microsoft. Logic/iş akışları/devre dışı bırakma/eylem | İş akışını devre dışı bırakır. |
> | Microsoft. Logic/iş akışları/etkinleştir/eylem | İş akışını izin vermez. |
> | Microsoft. Logic/iş akışları/doğrula/eylem | İş akışını doğrular. |
> | Microsoft. resources/dağıtımlar/işlemler/okuma | Dağıtım işlemlerini alır veya listeler. |
> | Microsoft. resources/abonelikler/operationresults/Read | Abonelik işlem sonuçlarını alın. |
> | Microsoft. resources/abonelikler/resourceGroups/Read | Kaynak gruplarını alır veya listeler. |
> | Microsoft. support/* | Destek bileti oluşturma ve güncelleştirme |
> | Microsoft. Web/Connectiongateway/*/Read | Bağlantı ağ geçitlerini okuyun. |
> | Microsoft. Web/Connections/*/Read | Bağlantıları oku. |
> | Microsoft. Web/Customapsıs/*/Read | Özel API 'YI okuyun. |
> | Microsoft. Web/Sunucugrupları/okuma | App Service planında özellikleri al |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you read, enable and disable logic app.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/515c2055-d9d4-4321-b1b9-bd0c9a0f79fe",
  "name": "515c2055-d9d4-4321-b1b9-bd0c9a0f79fe",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*/read",
        "Microsoft.Insights/metricAlerts/*/read",
        "Microsoft.Insights/diagnosticSettings/*/read",
        "Microsoft.Insights/metricDefinitions/*/read",
        "Microsoft.Logic/*/read",
        "Microsoft.Logic/workflows/disable/action",
        "Microsoft.Logic/workflows/enable/action",
        "Microsoft.Logic/workflows/validate/action",
        "Microsoft.Resources/deployments/operations/read",
        "Microsoft.Resources/subscriptions/operationresults/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*",
        "Microsoft.Web/connectionGateways/*/read",
        "Microsoft.Web/connections/*/read",
        "Microsoft.Web/customApis/*/read",
        "Microsoft.Web/serverFarms/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Logic App Operator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

## <a name="identity"></a>Kimlik


### <a name="managed-identity-contributor"></a>Yönetilen kimlik Katılımcısı

Kullanıcı tarafından atanan kimlik oluşturma, okuma, güncelleştirme ve silme

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. Managedıdentity/Useratandıdentities/Read | Mevcut kullanıcı tarafından atanan kimliği alır |
> | Microsoft. Managedıdentity/Useratandıdentities/Write | Yeni Kullanıcı tarafından atanmış bir kimlik oluşturur veya var olan bir kullanıcı tarafından atanan kimlikle ilişkili etiketleri güncelleştirir |
> | Microsoft. Managedıdentity/Useratandıdentities/Delete | Mevcut kullanıcı tarafından atanan bir kimliği siler |
> | Microsoft. Authorization/*/Read | Rolleri ve rol atamalarını oku |
> | Microsoft. Insights/alertRules/* | Klasik ölçüm uyarısı oluşturma ve yönetme |
> | Microsoft. resources/abonelikler/resourceGroups/Read | Kaynak gruplarını alır veya listeler. |
> | Microsoft. resources/dağıtımlar/* | Dağıtım oluşturma ve yönetme |
> | Microsoft. support/* | Destek bileti oluşturma ve güncelleştirme |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Create, Read, Update, and Delete User Assigned Identity",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/e40ec5ca-96e0-45a2-b4ff-59039f2c2b59",
  "name": "e40ec5ca-96e0-45a2-b4ff-59039f2c2b59",
  "permissions": [
    {
      "actions": [
        "Microsoft.ManagedIdentity/userAssignedIdentities/read",
        "Microsoft.ManagedIdentity/userAssignedIdentities/write",
        "Microsoft.ManagedIdentity/userAssignedIdentities/delete",
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Managed Identity Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="managed-identity-operator"></a>Yönetilen kimlik Işleci

Kullanıcı tarafından atanan kimliği okuma ve atama

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. Managedıdentity/Useratandıdentities/*/Read |  |
> | Microsoft. Managedıdentity/Useratandıdentities/*/atama/Action |  |
> | Microsoft. Authorization/*/Read | Rolleri ve rol atamalarını oku |
> | Microsoft. Insights/alertRules/* | Klasik ölçüm uyarısı oluşturma ve yönetme |
> | Microsoft. resources/abonelikler/resourceGroups/Read | Kaynak gruplarını alır veya listeler. |
> | Microsoft. resources/dağıtımlar/* | Dağıtım oluşturma ve yönetme |
> | Microsoft. support/* | Destek bileti oluşturma ve güncelleştirme |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Read and Assign User Assigned Identity",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/f1a07417-d97a-45cb-824c-7a7467783830",
  "name": "f1a07417-d97a-45cb-824c-7a7467783830",
  "permissions": [
    {
      "actions": [
        "Microsoft.ManagedIdentity/userAssignedIdentities/*/read",
        "Microsoft.ManagedIdentity/userAssignedIdentities/*/assign/action",
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Managed Identity Operator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

## <a name="security"></a>Güvenlik


### <a name="azure-sentinel-contributor"></a>Azure Sentinel Katılımcısı

Azure Sentinel Katılımcısı

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. Securityınsights/* |  |
> | Microsoft. Operationalınsights/çalışma alanları/analiz/sorgu/eylem | Yeni altyapıyı kullanarak arama yapın. |
> | Microsoft. Operationalınsights/çalışma alanları/*/Read | Log Analytics verilerini görüntüleme |
> | Microsoft. Operationalınsights/çalışma alanları/Savedaramalar/* |  |
> | Microsoft. OperationsManagement/Solutions/Read | OMS çözümünü çıkmadan al |
> | Microsoft. Operationalınsights/çalışma alanları/sorgu/okuma | Çalışma alanındaki veriler üzerinde sorgu çalıştırma |
> | Microsoft. Operationalınsights/çalışma alanları/sorgu/*/okuma |  |
> | Microsoft. Operationalınsights/çalışma alanları/veri kaynakları/okuma | Bir çalışma alanı altındaki veri kaynaklarını alın. |
> | Microsoft. Insights/çalışma kitapları/* |  |
> | Microsoft. Authorization/*/Read | Rolleri ve rol atamalarını oku |
> | Microsoft. Insights/alertRules/* | Klasik ölçüm uyarısı oluşturma ve yönetme |
> | Microsoft. resources/dağıtımlar/* | Dağıtım oluşturma ve yönetme |
> | Microsoft. resources/abonelikler/resourceGroups/Read | Kaynak gruplarını alır veya listeler. |
> | Microsoft. support/* | Destek bileti oluşturma ve güncelleştirme |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Azure Sentinel Contributor",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/ab8e14d6-4a74-4a29-9ba8-549422addade",
  "name": "ab8e14d6-4a74-4a29-9ba8-549422addade",
  "permissions": [
    {
      "actions": [
        "Microsoft.SecurityInsights/*",
        "Microsoft.OperationalInsights/workspaces/analytics/query/action",
        "Microsoft.OperationalInsights/workspaces/*/read",
        "Microsoft.OperationalInsights/workspaces/savedSearches/*",
        "Microsoft.OperationsManagement/solutions/read",
        "Microsoft.OperationalInsights/workspaces/query/read",
        "Microsoft.OperationalInsights/workspaces/query/*/read",
        "Microsoft.OperationalInsights/workspaces/dataSources/read",
        "Microsoft.Insights/workbooks/*",
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Sentinel Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="azure-sentinel-reader"></a>Azure Sentinel okuyucusu

Azure Sentinel okuyucusu

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. Securityınsights/*/Read |  |
> | Microsoft. Securityınsights/dataConnectorsCheckRequirements/Action | Kullanıcı yetkilendirmesini ve lisansını denetle |
> | Microsoft. Operationalınsights/çalışma alanları/analiz/sorgu/eylem | Yeni altyapıyı kullanarak arama yapın. |
> | Microsoft. Operationalınsights/çalışma alanları/*/Read | Log Analytics verilerini görüntüleme |
> | Microsoft. Operationalınsights/çalışma alanları/LinkedServices/okuma | Belirtilen çalışma alanı altındaki bağlı hizmetleri alın. |
> | Microsoft. Operationalınsights/çalışma alanları/Savedaramalar/okuma | Kayıtlı bir arama sorgusu alır |
> | Microsoft. OperationsManagement/Solutions/Read | OMS çözümünü çıkmadan al |
> | Microsoft. Operationalınsights/çalışma alanları/sorgu/okuma | Çalışma alanındaki veriler üzerinde sorgu çalıştırma |
> | Microsoft. Operationalınsights/çalışma alanları/sorgu/*/okuma |  |
> | Microsoft. Operationalınsights/çalışma alanları/veri kaynakları/okuma | Bir çalışma alanı altındaki veri kaynaklarını alın. |
> | Microsoft. Insights/çalışma kitapları/okuma | Çalışma kitabını okuma |
> | Microsoft. Authorization/*/Read | Rolleri ve rol atamalarını oku |
> | Microsoft. Insights/alertRules/* | Klasik ölçüm uyarısı oluşturma ve yönetme |
> | Microsoft. resources/dağıtımlar/* | Dağıtım oluşturma ve yönetme |
> | Microsoft. resources/abonelikler/resourceGroups/Read | Kaynak gruplarını alır veya listeler. |
> | Microsoft. support/* | Destek bileti oluşturma ve güncelleştirme |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Azure Sentinel Reader",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/8d289c81-5878-46d4-8554-54e1e3d8b5cb",
  "name": "8d289c81-5878-46d4-8554-54e1e3d8b5cb",
  "permissions": [
    {
      "actions": [
        "Microsoft.SecurityInsights/*/read",
        "Microsoft.SecurityInsights/dataConnectorsCheckRequirements/action",
        "Microsoft.OperationalInsights/workspaces/analytics/query/action",
        "Microsoft.OperationalInsights/workspaces/*/read",
        "Microsoft.OperationalInsights/workspaces/LinkedServices/read",
        "Microsoft.OperationalInsights/workspaces/savedSearches/read",
        "Microsoft.OperationsManagement/solutions/read",
        "Microsoft.OperationalInsights/workspaces/query/read",
        "Microsoft.OperationalInsights/workspaces/query/*/read",
        "Microsoft.OperationalInsights/workspaces/dataSources/read",
        "Microsoft.Insights/workbooks/read",
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Sentinel Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="azure-sentinel-responder"></a>Azure Sentinel Yanıtlayıcısı

Azure Sentinel Yanıtlayıcısı

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. Securityınsights/*/Read |  |
> | Microsoft. Securityınsights/dataConnectorsCheckRequirements/Action | Kullanıcı yetkilendirmesini ve lisansını denetle |
> | Microsoft. Securityınsights/Cases/* |  |
> | Microsoft. Securityınsights/olaylar/* |  |
> | Microsoft. Operationalınsights/çalışma alanları/analiz/sorgu/eylem | Yeni altyapıyı kullanarak arama yapın. |
> | Microsoft. Operationalınsights/çalışma alanları/*/Read | Log Analytics verilerini görüntüleme |
> | Microsoft. Operationalınsights/çalışma alanları/veri kaynakları/okuma | Bir çalışma alanı altındaki veri kaynaklarını alın. |
> | Microsoft. Operationalınsights/çalışma alanları/Savedaramalar/okuma | Kayıtlı bir arama sorgusu alır |
> | Microsoft. OperationsManagement/Solutions/Read | OMS çözümünü çıkmadan al |
> | Microsoft. Operationalınsights/çalışma alanları/sorgu/okuma | Çalışma alanındaki veriler üzerinde sorgu çalıştırma |
> | Microsoft. Operationalınsights/çalışma alanları/sorgu/*/okuma |  |
> | Microsoft. Operationalınsights/çalışma alanları/veri kaynakları/okuma | Bir çalışma alanı altındaki veri kaynaklarını alın. |
> | Microsoft. Insights/çalışma kitapları/okuma | Çalışma kitabını okuma |
> | Microsoft. Authorization/*/Read | Rolleri ve rol atamalarını oku |
> | Microsoft. Insights/alertRules/* | Klasik ölçüm uyarısı oluşturma ve yönetme |
> | Microsoft. resources/dağıtımlar/* | Dağıtım oluşturma ve yönetme |
> | Microsoft. resources/abonelikler/resourceGroups/Read | Kaynak gruplarını alır veya listeler. |
> | Microsoft. support/* | Destek bileti oluşturma ve güncelleştirme |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Azure Sentinel Responder",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/3e150937-b8fe-4cfb-8069-0eaf05ecd056",
  "name": "3e150937-b8fe-4cfb-8069-0eaf05ecd056",
  "permissions": [
    {
      "actions": [
        "Microsoft.SecurityInsights/*/read",
        "Microsoft.SecurityInsights/dataConnectorsCheckRequirements/action",
        "Microsoft.SecurityInsights/cases/*",
        "Microsoft.SecurityInsights/incidents/*",
        "Microsoft.OperationalInsights/workspaces/analytics/query/action",
        "Microsoft.OperationalInsights/workspaces/*/read",
        "Microsoft.OperationalInsights/workspaces/dataSources/read",
        "Microsoft.OperationalInsights/workspaces/savedSearches/read",
        "Microsoft.OperationsManagement/solutions/read",
        "Microsoft.OperationalInsights/workspaces/query/read",
        "Microsoft.OperationalInsights/workspaces/query/*/read",
        "Microsoft.OperationalInsights/workspaces/dataSources/read",
        "Microsoft.Insights/workbooks/read",
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Sentinel Responder",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="key-vault-contributor"></a>Katkıda bulunan Key Vault

Anahtar kasalarını yönetmenize izin verir, ancak bunlara erişim sağlamaz.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. Authorization/*/Read | Rolleri ve rol atamalarını oku |
> | Microsoft. Insights/alertRules/* | Klasik ölçüm uyarısı oluşturma ve yönetme |
> | Microsoft. Keykasası/* |  |
> | Microsoft. resources/dağıtımlar/* | Dağıtım oluşturma ve yönetme |
> | Microsoft. resources/abonelikler/resourceGroups/Read | Kaynak gruplarını alır veya listeler. |
> | Microsoft. support/* | Destek bileti oluşturma ve güncelleştirme |
> | **NotActions** |  |
> | Microsoft. Keykasası/konumlar/Silinkaults/Temizleme/eylem | Geçici olarak silinen bir anahtar kasasını temizle |
> | Microsoft. Keykasası/hsmPools/* |  |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage key vaults, but not access to them.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/f25e0fa2-a7c8-4377-a976-54943a77a395",
  "name": "f25e0fa2-a7c8-4377-a976-54943a77a395",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.KeyVault/*",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [
        "Microsoft.KeyVault/locations/deletedVaults/purge/action",
        "Microsoft.KeyVault/hsmPools/*"
      ],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Key Vault Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="security-admin"></a>Güvenlik Yöneticisi

Güvenlik Merkezi için izinleri görüntüleyin ve güncelleştirin. Güvenlik okuyucu rolüyle aynı izinler ve ayrıca güvenlik ilkesini güncelleştirebilir ve uyarıları ve önerileri kapatabilir.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. Authorization/*/Read | Rolleri ve rol atamalarını oku |
> | Microsoft. Authorization/Poliyasatamaları/* | İlke atamaları oluşturma ve yönetme |
> | Microsoft. Authorization/policyDefinitions/* | İlke tanımları oluşturma ve yönetme |
> | Microsoft. Authorization/policySetDefinitions/* | İlke kümeleri oluşturma ve yönetme |
> | Microsoft. Insights/alertRules/* | Klasik ölçüm uyarısı oluşturma ve yönetme |
> | Microsoft. Management/managementGroups/Read | Kimliği doğrulanmış kullanıcı için Yönetim gruplarını listeleyin. |
> | Microsoft. Operationalınsights/çalışma alanları/*/Read | Log Analytics verilerini görüntüleme |
> | Microsoft. resources/dağıtımlar/* | Dağıtım oluşturma ve yönetme |
> | Microsoft. resources/abonelikler/resourceGroups/Read | Kaynak gruplarını alır veya listeler. |
> | Microsoft. Security/* | Güvenlik bileşenleri ve ilkeleri oluşturma ve yönetme |
> | Microsoft. support/* | Destek bileti oluşturma ve güncelleştirme |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Security Admin Role",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/fb1c8493-542b-48eb-b624-b4c8fea62acd",
  "name": "fb1c8493-542b-48eb-b624-b4c8fea62acd",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Authorization/policyAssignments/*",
        "Microsoft.Authorization/policyDefinitions/*",
        "Microsoft.Authorization/policySetDefinitions/*",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Management/managementGroups/read",
        "Microsoft.operationalInsights/workspaces/*/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Security/*",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Security Admin",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="security-assessment-contributor"></a>Güvenlik değerlendirmesi Katılımcısı

Değerlendirmelere Güvenlik Merkezi 'ne gönderim olanağı sağlar

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. Security/değerlendirmeleri/yazma | Aboneliğinizde güvenlik değerlendirmeleri oluşturun veya güncelleştirin |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you push assessments to Security Center",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/612c2aa1-cb24-443b-ac28-3ab7272de6f5",
  "name": "612c2aa1-cb24-443b-ac28-3ab7272de6f5",
  "permissions": [
    {
      "actions": [
        "Microsoft.Security/assessments/write"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Security Assessment Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="security-manager-legacy"></a>Güvenlik Yöneticisi (eski)

Bu eski bir roldür. Lütfen bunun yerine Güvenlik Yöneticisi 'ni kullanın.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. Authorization/*/Read | Rolleri ve rol atamalarını oku |
> | Microsoft. ClassicCompute/*/Read | Yapılandırma bilgileri klasik sanal makineler 'i okuyun |
> | Microsoft. ClassicCompute/virtualMachines/*/Write | Klasik sanal makineler için yazma yapılandırması |
> | Microsoft. ClassicNetwork/*/Read | Klasik ağla ilgili yapılandırma bilgilerini okuyun |
> | Microsoft. Insights/alertRules/* | Klasik ölçüm uyarısı oluşturma ve yönetme |
> | Microsoft. ResourceHealth/kullanılabilirlik Bilitydurumlar/okuma | Belirtilen kapsamdaki tüm kaynaklar için Kullanılabilirlik durumlarını alır |
> | Microsoft. resources/dağıtımlar/* | Dağıtım oluşturma ve yönetme |
> | Microsoft. resources/abonelikler/resourceGroups/Read | Kaynak gruplarını alır veya listeler. |
> | Microsoft. Security/* | Güvenlik bileşenleri ve ilkeleri oluşturma ve yönetme |
> | Microsoft. support/* | Destek bileti oluşturma ve güncelleştirme |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "This is a legacy role. Please use Security Administrator instead",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/e3d13bf0-dd5a-482e-ba6b-9b8433878d10",
  "name": "e3d13bf0-dd5a-482e-ba6b-9b8433878d10",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.ClassicCompute/*/read",
        "Microsoft.ClassicCompute/virtualMachines/*/write",
        "Microsoft.ClassicNetwork/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Security/*",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Security Manager (Legacy)",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="security-reader"></a>Güvenlik okuyucusu

Güvenlik Merkezi için izinleri görüntüleyin. Önerileri, uyarıları, güvenlik ilkesini ve güvenlik durumlarını görüntüleyebilir, ancak değişiklik yapamaz.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. Authorization/*/Read | Rolleri ve rol atamalarını oku |
> | Microsoft. Insights/alertRules/* | Klasik ölçüm uyarısı oluşturma ve yönetme |
> | Microsoft. Operationalınsights/çalışma alanları/*/Read | Log Analytics verilerini görüntüleme |
> | Microsoft. resources/dağıtımlar/* | Dağıtım oluşturma ve yönetme |
> | Microsoft. resources/abonelikler/resourceGroups/Read | Kaynak gruplarını alır veya listeler. |
> | Microsoft. Security/*/Read | Güvenlik bileşenlerini ve ilkelerini okuyun |
> | Microsoft. support/* | Destek bileti oluşturma ve güncelleştirme |
> | Microsoft. Management/managementGroups/Read | Kimliği doğrulanmış kullanıcı için Yönetim gruplarını listeleyin. |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Security Reader Role",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/39bc4728-0917-49c7-9d2c-d95423bc2eb4",
  "name": "39bc4728-0917-49c7-9d2c-d95423bc2eb4",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.operationalInsights/workspaces/*/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Security/*/read",
        "Microsoft.Support/*",
        "Microsoft.Management/managementGroups/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Security Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

## <a name="devops"></a>DevOps


### <a name="devtest-labs-user"></a>DevTest Labs kullanıcısı

Azure DevTest Labs sanal makinelerinizi bağlamanıza, başlatmanıza, yeniden başlatmanıza ve kapatımanıza olanak sağlar.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. Authorization/*/Read | Rolleri ve rol atamalarını oku |
> | Microsoft. COMPUTE/kullanılabilirliği Bilitysets/Read | Bir kullanılabilirlik kümesinin özelliklerini al |
> | Microsoft. COMPUTE/virtualMachines/*/Read | Bir sanal makinenin özelliklerini okuyun (VM boyutları, çalışma zamanı durumu, VM uzantıları vb.) |
> | Microsoft. COMPUTE/virtualMachines/serbest bırakma/eylem | Sanal makineyi güçlendirir ve işlem kaynaklarını serbest bırakır |
> | Microsoft. COMPUTE/virtualMachines/okuma | Bir sanal makinenin özelliklerini al |
> | Microsoft. COMPUTE/virtualMachines/yeniden Başlat/eylem | Sanal makineyi yeniden başlatır |
> | Microsoft. COMPUTE/virtualMachines/Başlat/eylem | Sanal makineyi başlatır |
> | Microsoft. DevTestLab/*/Read | Laboratuvarın özelliklerini okuyun |
> | Microsoft. DevTestLab/Labs/claimAnyVm/eylem | Laboratuvarda rastgele bir çakışan sanal makine talep edin. |
> | Microsoft. DevTestLab/Labs/createEnvironment/eylem | Laboratuvarda sanal makineler oluşturun. |
> | Microsoft. DevTestLab/Labs/ensureCurrentUserProfile/Action | Geçerli kullanıcının laboratuvarda geçerli bir profile sahip olduğundan emin olun. |
> | Microsoft. DevTestLab/Labs/formüller/Sil | Formülleri silin. |
> | Microsoft. DevTestLab/Labs/formüller/okuma | Formülleri okuyun. |
> | Microsoft. DevTestLab/Labs/formüller/yazma | Formüller ekleme veya değiştirme. |
> | Microsoft. DevTestLab/Labs/policySets/evaluatePolicies/Action | Laboratuvar ilkesini değerlendirir. |
> | Microsoft. DevTestLab/Labs/virtualMachines/talep/eylem | Mevcut bir sanal makinenin sahipliğini al |
> | Microsoft. DevTestLab/Labs/virtualmachines/Listapperepblezamanlamalar/eylem | Varsa, geçerli başlatma/durdurma zamanlamalarını listeler. |
> | Microsoft. DevTestLab/Labs/virtualMachines/getRdpFileContents/Action | Sanal makine için RDP dosyasının içeriğini temsil eden bir dize alır |
> | Microsoft. Network/loadBalancers/Backendadddresspoir/JOIN/Action | Yük dengeleyici arka uç adres havuzunu birleştirir. Alertable değil. |
> | Microsoft. Network/loadBalancers/ınboundnatrules/JOIN/Action | Yük dengeleyici gelen NAT kuralına katılır. Alertable değil. |
> | Microsoft. Network/NetworkInterfaces/*/Read | Bir ağ arabiriminin özelliklerini okuyun (örneğin, ağ arabiriminin bir parçası olduğu tüm yük dengeleyiciler) |
> | Microsoft. Network/NetworkInterfaces/JOIN/Action | Bir sanal makineyi bir ağ arabirimine birleştirir. Alertable değil. |
> | Microsoft. Network/NetworkInterfaces/Read | Bir ağ arabirimi tanımını alır.  |
> | Microsoft. Network/NetworkInterfaces/Write | Ağ arabirimi oluşturur veya var olan bir ağ arabirimini güncelleştirir.  |
> | Microsoft. Network/publicIPAddresses/*/Read | Genel IP adresinin özelliklerini okuyun |
> | Microsoft. Network/publicIPAddresses/JOIN/Action | Genel bir IP adresini birleştirir. Alertable değil. |
> | Microsoft. Network/publicIPAddresses/Read | Genel IP adresi tanımını alır. |
> | Microsoft. Network/virtualNetworks/alt ağlar/JOIN/Action | Bir sanal ağı birleştirir. Alertable değil. |
> | Microsoft. resources/dağıtımlar/işlemler/okuma | Dağıtım işlemlerini alır veya listeler. |
> | Microsoft. resources/dağıtımlar/okuma | Dağıtımları alır veya listeler. |
> | Microsoft. resources/abonelikler/resourceGroups/Read | Kaynak gruplarını alır veya listeler. |
> | Microsoft. Storage/storageAccounts/listKeys/Action | Belirtilen depolama hesabı için erişim anahtarlarını döndürür. |
> | **NotActions** |  |
> | Microsoft. COMPUTE/virtualMachines/vmSizes/Read | Sanal makinenin güncelleştirileceği kullanılabilir boyutları listeler |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you connect, start, restart, and shutdown your virtual machines in your Azure DevTest Labs.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/76283e04-6283-4c54-8f91-bcf1374a3c64",
  "name": "76283e04-6283-4c54-8f91-bcf1374a3c64",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Compute/availabilitySets/read",
        "Microsoft.Compute/virtualMachines/*/read",
        "Microsoft.Compute/virtualMachines/deallocate/action",
        "Microsoft.Compute/virtualMachines/read",
        "Microsoft.Compute/virtualMachines/restart/action",
        "Microsoft.Compute/virtualMachines/start/action",
        "Microsoft.DevTestLab/*/read",
        "Microsoft.DevTestLab/labs/claimAnyVm/action",
        "Microsoft.DevTestLab/labs/createEnvironment/action",
        "Microsoft.DevTestLab/labs/ensureCurrentUserProfile/action",
        "Microsoft.DevTestLab/labs/formulas/delete",
        "Microsoft.DevTestLab/labs/formulas/read",
        "Microsoft.DevTestLab/labs/formulas/write",
        "Microsoft.DevTestLab/labs/policySets/evaluatePolicies/action",
        "Microsoft.DevTestLab/labs/virtualMachines/claim/action",
        "Microsoft.DevTestLab/labs/virtualmachines/listApplicableSchedules/action",
        "Microsoft.DevTestLab/labs/virtualMachines/getRdpFileContents/action",
        "Microsoft.Network/loadBalancers/backendAddressPools/join/action",
        "Microsoft.Network/loadBalancers/inboundNatRules/join/action",
        "Microsoft.Network/networkInterfaces/*/read",
        "Microsoft.Network/networkInterfaces/join/action",
        "Microsoft.Network/networkInterfaces/read",
        "Microsoft.Network/networkInterfaces/write",
        "Microsoft.Network/publicIPAddresses/*/read",
        "Microsoft.Network/publicIPAddresses/join/action",
        "Microsoft.Network/publicIPAddresses/read",
        "Microsoft.Network/virtualNetworks/subnets/join/action",
        "Microsoft.Resources/deployments/operations/read",
        "Microsoft.Resources/deployments/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Storage/storageAccounts/listKeys/action"
      ],
      "notActions": [
        "Microsoft.Compute/virtualMachines/vmSizes/read"
      ],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "DevTest Labs User",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="lab-creator"></a>Laboratuvar Oluşturucu

Azure Laboratuvar hesaplarınız kapsamında yönetilen Laboratuvarlarınızı oluşturmanıza, yönetmenize ve silmenize olanak sağlar.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. Authorization/*/Read | Rolleri ve rol atamalarını oku |
> | Microsoft. LabServices/labAccounts/*/Read |  |
> | Microsoft. LabServices/labAccounts/createLab/eylem | Laboratuvar hesabında laboratuvar oluşturun. |
> | Microsoft. LabServices/labAccounts/boyutlar/getRegionalAvailability/eylem |  |
> | Microsoft. LabServices/labAccounts/getRegionalAvailability/eylem | Laboratuvar hesabı altında yapılandırılan her boyut kategorisi için bölgesel kullanılabilirlik bilgilerini al |
> | Microsoft. LabServices/labAccounts/Getpricingandavvailability/eylem | Laboratuvar hesabı için boyut, coğrafi grafik ve işletim sistemi birleşimlerinin fiyatlandırmasını ve kullanılabilirliğini öğrenin. |
> | Microsoft. LabServices/labAccounts/getRestrictionsAndUsage/Action | Bu abonelik için çekirdek kısıtlamaları ve kullanımı al |
> | Microsoft. resources/abonelikler/resourceGroups/Read | Kaynak gruplarını alır veya listeler. |
> | Microsoft. support/* | Destek bileti oluşturma ve güncelleştirme |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you create, manage, delete your managed labs under your Azure Lab Accounts.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/b97fb8bc-a8b2-4522-a38b-dd33c7e65ead",
  "name": "b97fb8bc-a8b2-4522-a38b-dd33c7e65ead",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.LabServices/labAccounts/*/read",
        "Microsoft.LabServices/labAccounts/createLab/action",
        "Microsoft.LabServices/labAccounts/sizes/getRegionalAvailability/action",
        "Microsoft.LabServices/labAccounts/getRegionalAvailability/action",
        "Microsoft.LabServices/labAccounts/getPricingAndAvailability/action",
        "Microsoft.LabServices/labAccounts/getRestrictionsAndUsage/action",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Lab Creator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

## <a name="monitor"></a>İzleme


### <a name="application-insights-component-contributor"></a>Application Insights bileşeni Katılımcısı

Application Insights bileşenlerini yönetebilir

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. Authorization/*/Read | Rolleri ve rol atamalarını oku |
> | Microsoft. Insights/alertRules/* | Klasik uyarı kuralları oluşturma ve yönetme |
> | Microsoft. Insights/metricAlerts/* | Yeni uyarı kuralları oluşturma ve yönetme |
> | Microsoft. Insights/bileşenler/* | Öngörüler bileşenleri oluşturma ve yönetme |
> | Microsoft. Insights/webtests/* | Öngörüler Web testleri oluşturma ve yönetme |
> | Microsoft. ResourceHealth/kullanılabilirlik Bilitydurumlar/okuma | Belirtilen kapsamdaki tüm kaynaklar için Kullanılabilirlik durumlarını alır |
> | Microsoft. resources/dağıtımlar/* | Dağıtım oluşturma ve yönetme |
> | Microsoft. resources/abonelikler/resourceGroups/Read | Kaynak gruplarını alır veya listeler. |
> | Microsoft. support/* | Destek bileti oluşturma ve güncelleştirme |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can manage Application Insights components",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/ae349356-3a1b-4a5e-921d-050484c6347e",
  "name": "ae349356-3a1b-4a5e-921d-050484c6347e",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Insights/metricAlerts/*",
        "Microsoft.Insights/components/*",
        "Microsoft.Insights/webtests/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Application Insights Component Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="application-insights-snapshot-debugger"></a>Application Insights Snapshot Debugger

Application Insights Snapshot Debugger ile toplanan hata ayıklama anlık görüntülerini görüntülemek ve indirmek için kullanıcıya izin verir. Bu izinlerin [sahip](#owner) veya [katkıda](#contributor) bulunan rollerine dahil edilmediğini unutmayın.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. Authorization/*/Read | Rolleri ve rol atamalarını oku |
> | Microsoft. Insights/alertRules/* | Klasik ölçüm uyarısı oluşturma ve yönetme |
> | Microsoft. Insights/bileşenler/*/Read |  |
> | Microsoft. resources/dağıtımlar/* | Dağıtım oluşturma ve yönetme |
> | Microsoft. resources/abonelikler/resourceGroups/Read | Kaynak gruplarını alır veya listeler. |
> | Microsoft. support/* | Destek bileti oluşturma ve güncelleştirme |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Gives user permission to use Application Insights Snapshot Debugger features",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/08954f03-6346-4c2e-81c0-ec3a5cfae23b",
  "name": "08954f03-6346-4c2e-81c0-ec3a5cfae23b",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Insights/components/*/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Application Insights Snapshot Debugger",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="monitoring-contributor"></a>Katkıda bulunan izleniyor

Tüm izleme verilerini okuyabilir ve izleme ayarlarını düzenleyebilir. Ayrıca bkz. [Azure izleyici ile roller, izinler ve güvenlik ile çalışmaya başlama](https://docs.microsoft.com/azure/azure-monitor/platform/roles-permissions-security#built-in-monitoring-roles).

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | */Read | Gizli dizileri hariç tüm türlerin kaynaklarını okuyun. |
> | Microsoft. AlertsManagement/uyarılar/* |  |
> | Microsoft. AlertsManagement/alertsSummary/* |  |
> | Microsoft. Insights/actiongroups/* |  |
> | Microsoft. Insights/activityLogAlerts/* |  |
> | Microsoft. Insights/AlertRules/* | Klasik ölçüm uyarısı oluşturma ve yönetme |
> | Microsoft. Insights/bileşenler/* | Öngörüler bileşenleri oluşturma ve yönetme |
> | Microsoft. Insights/DiagnosticSettings/* | Analiz Sunucusu için tanılama ayarını oluşturur, güncelleştirir veya okur |
> | Microsoft. Insights/eventTypes/* | Bir abonelikteki etkinlik günlüğü olaylarını (yönetim olayları) listeleyin. Bu izin, etkinlik günlüğüne hem programlı hem de portala erişim için geçerlidir. |
> | Microsoft. Insights/LogDefinitions/* | Bu izin, Portal aracılığıyla etkinlik günlüklerine erişmesi gereken kullanıcılar için gereklidir. Etkinlik günlüğündeki günlük kategorilerini listeleyin. |
> | Microsoft. Insights/metricalerts/* |  |
> | Microsoft. Insights/MetricDefinitions/* | Ölçüm tanımlarını oku (bir kaynak için kullanılabilen ölçüm türlerinin listesi). |
> | Microsoft. Insights/ölçümler/* | Bir kaynak için ölçümleri okuyun. |
> | Microsoft. Insights/Register/ACTION | Microsoft Insights sağlayıcısını kaydedin |
> | Microsoft. Insights/scheduledqueryrules/* |  |
> | Microsoft. Insights/webtests/* | Öngörüler Web testleri oluşturma ve yönetme |
> | Microsoft. Insights/çalışma kitapları/* |  |
> | Microsoft. Insights/privateLinkScopes/* |  |
> | Microsoft. Insights/Privatelinkscopeoperationdurumlarının/* |  |
> | Microsoft. Operationalınsights/çalışma alanları/yazma | Mevcut çalışma alanından müşteri kimliğini sağlayarak yeni bir çalışma alanı veya mevcut bir çalışma alanına bağlantılar oluşturur. |
> | Microsoft. Operationalınsights/Workspaces/ıntelligencepacks/* | Log Analytics çözüm paketlerini okuma/yazma/silme. |
> | Microsoft. Operationalınsights/çalışma alanları/Savedaramalar/* | Log Analytics kayıtlı aramalarını okuma/yazma/silme. |
> | Microsoft. Operationalınsights/çalışma alanları/arama/eylem | Arama sorgusu yürütür |
> | Microsoft. Operationalınsights/Workspaces/sharedKeys/Action | Çalışma alanının paylaşılan anahtarlarını alır. Bu anahtarlar, Microsoft operasyonel içgörüler aracılarını çalışma alanına bağlamak için kullanılır. |
> | Microsoft. Operationalınsights/Workspaces/storageınsiizconfigs/* | Log Analytics depolama öngörülerini okuma/yazma/silme. |
> | Microsoft. support/* | Destek bileti oluşturma ve güncelleştirme |
> | Microsoft. WorkloadMonitor/izleyicileri/* |  |
> | Microsoft. WorkloadMonitor/notificationSettings/* |  |
> | Microsoft. AlertsManagement/smartDetectorAlertRules/* |  |
> | Microsoft. AlertsManagement/actionRules/* |  |
> | Microsoft. AlertsManagement/smartGroups/* |  |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can read all monitoring data and update monitoring settings.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/749f88d5-cbae-40b8-bcfc-e573ddc772fa",
  "name": "749f88d5-cbae-40b8-bcfc-e573ddc772fa",
  "permissions": [
    {
      "actions": [
        "*/read",
        "Microsoft.AlertsManagement/alerts/*",
        "Microsoft.AlertsManagement/alertsSummary/*",
        "Microsoft.Insights/actiongroups/*",
        "Microsoft.Insights/activityLogAlerts/*",
        "Microsoft.Insights/AlertRules/*",
        "Microsoft.Insights/components/*",
        "Microsoft.Insights/DiagnosticSettings/*",
        "Microsoft.Insights/eventtypes/*",
        "Microsoft.Insights/LogDefinitions/*",
        "Microsoft.Insights/metricalerts/*",
        "Microsoft.Insights/MetricDefinitions/*",
        "Microsoft.Insights/Metrics/*",
        "Microsoft.Insights/Register/Action",
        "Microsoft.Insights/scheduledqueryrules/*",
        "Microsoft.Insights/webtests/*",
        "Microsoft.Insights/workbooks/*",
        "Microsoft.Insights/privateLinkScopes/*",
        "Microsoft.Insights/privateLinkScopeOperationStatuses/*",
        "Microsoft.OperationalInsights/workspaces/write",
        "Microsoft.OperationalInsights/workspaces/intelligencepacks/*",
        "Microsoft.OperationalInsights/workspaces/savedSearches/*",
        "Microsoft.OperationalInsights/workspaces/search/action",
        "Microsoft.OperationalInsights/workspaces/sharedKeys/action",
        "Microsoft.OperationalInsights/workspaces/storageinsightconfigs/*",
        "Microsoft.Support/*",
        "Microsoft.WorkloadMonitor/monitors/*",
        "Microsoft.WorkloadMonitor/notificationSettings/*",
        "Microsoft.AlertsManagement/smartDetectorAlertRules/*",
        "Microsoft.AlertsManagement/actionRules/*",
        "Microsoft.AlertsManagement/smartGroups/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Monitoring Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="monitoring-metrics-publisher"></a>Ölçüm yayımcısını izleme

Azure kaynaklarında ölçüm yayımlamaya izin vermez

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. Insights/Register/ACTION | Microsoft Insights sağlayıcısını kaydedin |
> | Microsoft. support/* | Destek bileti oluşturma ve güncelleştirme |
> | Microsoft. resources/abonelikler/resourceGroups/Read | Kaynak gruplarını alır veya listeler. |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | Microsoft. Insights/ölçümler/yazma | Ölçümleri yaz |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Enables publishing metrics against Azure resources",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/3913510d-42f4-4e42-8a64-420c390055eb",
  "name": "3913510d-42f4-4e42-8a64-420c390055eb",
  "permissions": [
    {
      "actions": [
        "Microsoft.Insights/Register/Action",
        "Microsoft.Support/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.Insights/Metrics/Write"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Monitoring Metrics Publisher",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="monitoring-reader"></a>İzleme okuyucusu

Tüm izleme verilerini okuyabilir (ölçümler, Günlükler vb.). Ayrıca bkz. [Azure izleyici ile roller, izinler ve güvenlik ile çalışmaya başlama](https://docs.microsoft.com/azure/azure-monitor/platform/roles-permissions-security#built-in-monitoring-roles).

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | */Read | Gizli dizileri hariç tüm türlerin kaynaklarını okuyun. |
> | Microsoft. Operationalınsights/çalışma alanları/arama/eylem | Arama sorgusu yürütür |
> | Microsoft. support/* | Destek bileti oluşturma ve güncelleştirme |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can read all monitoring data.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/43d0d8ad-25c7-4714-9337-8ba259a9fe05",
  "name": "43d0d8ad-25c7-4714-9337-8ba259a9fe05",
  "permissions": [
    {
      "actions": [
        "*/read",
        "Microsoft.OperationalInsights/workspaces/search/action",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Monitoring Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="workbook-contributor"></a>Çalışma kitabı Katılımcısı

, Paylaşılan çalışma kitaplarını kaydedebilir.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. Insights/çalışma kitapları/yazma | Çalışma kitabı oluşturma veya güncelleştirme |
> | Microsoft. Öngörüler/çalışma kitapları/silme | Bir çalışma kitabını silme |
> | Microsoft. Insights/çalışma kitapları/okuma | Çalışma kitabını okuma |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can save shared workbooks.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/e8ddcd69-c73f-4f9f-9844-4100522f16ad",
  "name": "e8ddcd69-c73f-4f9f-9844-4100522f16ad",
  "permissions": [
    {
      "actions": [
        "Microsoft.Insights/workbooks/write",
        "Microsoft.Insights/workbooks/delete",
        "Microsoft.Insights/workbooks/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Workbook Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="workbook-reader"></a>Çalışma kitabı okuyucusu

, Çalışma kitaplarını okuyabilir.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. Insights/çalışma kitapları/okuma | Çalışma kitabını okuma |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can read workbooks.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/b279062a-9be3-42a0-92ae-8b3cf002ec4d",
  "name": "b279062a-9be3-42a0-92ae-8b3cf002ec4d",
  "permissions": [
    {
      "actions": [
        "microsoft.insights/workbooks/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Workbook Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

## <a name="management--governance"></a>Yönetim + idare


### <a name="automation-job-operator"></a>Automation Iş Işleci

Otomasyon Runbook 'Larını kullanarak Iş oluşturun ve yönetin.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. Authorization/*/Read | Rolleri ve rol atamalarını oku |
> | Microsoft. Automation/automationAccounts/hybridRunbookWorkerGroups/Read | Karma runbook çalışanı kaynaklarını okur |
> | Microsoft. Automation/automationAccounts/Jobs/okuma | Bir Azure Otomasyonu işini alır |
> | Microsoft. Automation/automationAccounts/Jobs/sürdürülecek/Action | Bir Azure Otomasyonu işini sürdürür |
> | Microsoft. Automation/automationAccounts/Jobs/durdur/eylem | Bir Azure Otomasyonu işini durduruyor |
> | Microsoft. Automation/automationAccounts/Jobs/akışlar/okuma | Bir Azure Otomasyonu iş akışı alır |
> | Microsoft. Automation/automationAccounts/Jobs/beklet/eylem | Bir Azure Otomasyonu işini askıya alır |
> | Microsoft. Automation/automationAccounts/Jobs/Write | Azure Otomasyonu işi oluşturur |
> | Microsoft. Automation/automationAccounts/Jobs/output/Read | Bir işin çıkışını alır |
> | Microsoft. Insights/alertRules/* | Klasik ölçüm uyarısı oluşturma ve yönetme |
> | Microsoft. resources/dağıtımlar/* | Dağıtım oluşturma ve yönetme |
> | Microsoft. resources/abonelikler/resourceGroups/Read | Kaynak gruplarını alır veya listeler. |
> | Microsoft. support/* | Destek bileti oluşturma ve güncelleştirme |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Create and Manage Jobs using Automation Runbooks.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/4fe576fe-1146-4730-92eb-48519fa6bf9f",
  "name": "4fe576fe-1146-4730-92eb-48519fa6bf9f",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Automation/automationAccounts/hybridRunbookWorkerGroups/read",
        "Microsoft.Automation/automationAccounts/jobs/read",
        "Microsoft.Automation/automationAccounts/jobs/resume/action",
        "Microsoft.Automation/automationAccounts/jobs/stop/action",
        "Microsoft.Automation/automationAccounts/jobs/streams/read",
        "Microsoft.Automation/automationAccounts/jobs/suspend/action",
        "Microsoft.Automation/automationAccounts/jobs/write",
        "Microsoft.Automation/automationAccounts/jobs/output/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Automation Job Operator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="automation-operator"></a>Otomasyon Operatörü

Automation Işleçleri, işleri başlatabilir, durdurabilir, askıya alabilir ve sürdürebilir

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. Authorization/*/Read | Rolleri ve rol atamalarını oku |
> | Microsoft. Automation/automationAccounts/hybridRunbookWorkerGroups/Read | Karma runbook çalışanı kaynaklarını okur |
> | Microsoft. Automation/automationAccounts/Jobs/okuma | Bir Azure Otomasyonu işini alır |
> | Microsoft. Automation/automationAccounts/Jobs/sürdürülecek/Action | Bir Azure Otomasyonu işini sürdürür |
> | Microsoft. Automation/automationAccounts/Jobs/durdur/eylem | Bir Azure Otomasyonu işini durduruyor |
> | Microsoft. Automation/automationAccounts/Jobs/akışlar/okuma | Bir Azure Otomasyonu iş akışı alır |
> | Microsoft. Automation/automationAccounts/Jobs/beklet/eylem | Bir Azure Otomasyonu işini askıya alır |
> | Microsoft. Automation/automationAccounts/Jobs/Write | Azure Otomasyonu işi oluşturur |
> | Microsoft. Automation/automationAccounts/Jobzamanlamalar/okuma | Bir Azure Otomasyonu iş zamanlaması alır |
> | Microsoft. Automation/automationAccounts/Jobzamanlamalar/yazma | Bir Azure Otomasyonu iş zamanlaması oluşturur |
> | Microsoft. Automation/automationAccounts/linkedWorkspace/Read | Otomasyon hesabına bağlı çalışma alanını alır |
> | Microsoft. Automation/automationAccounts/Read | Bir Azure Otomasyonu hesabını alır |
> | Microsoft. Automation/automationAccounts/runbook 'lar/okuma | Bir Azure Otomasyonu runbook 'unu alır |
> | Microsoft. Automation/automationAccounts/zamanlamalar/okuma | Bir Azure Otomasyonu zamanlama varlığını alır |
> | Microsoft. Automation/automationAccounts/zamanlamalar/yazma | Bir Azure Otomasyonu zamanlama varlığı oluşturur veya güncelleştirir |
> | Microsoft. Insights/alertRules/* | Klasik ölçüm uyarısı oluşturma ve yönetme |
> | Microsoft. ResourceHealth/kullanılabilirlik Bilitydurumlar/okuma | Belirtilen kapsamdaki tüm kaynaklar için Kullanılabilirlik durumlarını alır |
> | Microsoft. resources/dağıtımlar/* | Dağıtım oluşturma ve yönetme |
> | Microsoft. Automation/automationAccounts/Jobs/output/Read | Bir işin çıkışını alır |
> | Microsoft. resources/abonelikler/resourceGroups/Read | Kaynak gruplarını alır veya listeler. |
> | Microsoft. support/* | Destek bileti oluşturma ve güncelleştirme |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Automation Operators are able to start, stop, suspend, and resume jobs",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/d3881f73-407a-4167-8283-e981cbba0404",
  "name": "d3881f73-407a-4167-8283-e981cbba0404",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Automation/automationAccounts/hybridRunbookWorkerGroups/read",
        "Microsoft.Automation/automationAccounts/jobs/read",
        "Microsoft.Automation/automationAccounts/jobs/resume/action",
        "Microsoft.Automation/automationAccounts/jobs/stop/action",
        "Microsoft.Automation/automationAccounts/jobs/streams/read",
        "Microsoft.Automation/automationAccounts/jobs/suspend/action",
        "Microsoft.Automation/automationAccounts/jobs/write",
        "Microsoft.Automation/automationAccounts/jobSchedules/read",
        "Microsoft.Automation/automationAccounts/jobSchedules/write",
        "Microsoft.Automation/automationAccounts/linkedWorkspace/read",
        "Microsoft.Automation/automationAccounts/read",
        "Microsoft.Automation/automationAccounts/runbooks/read",
        "Microsoft.Automation/automationAccounts/schedules/read",
        "Microsoft.Automation/automationAccounts/schedules/write",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Automation/automationAccounts/jobs/output/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Automation Operator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="automation-runbook-operator"></a>Otomasyon Runbook Işleci

Runbook 'un Işlerini oluşturabilmek için Runbook özelliklerini okuyun.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. Authorization/*/Read | Rolleri ve rol atamalarını oku |
> | Microsoft. Automation/automationAccounts/runbook 'lar/okuma | Bir Azure Otomasyonu runbook 'unu alır |
> | Microsoft. Insights/alertRules/* | Klasik ölçüm uyarısı oluşturma ve yönetme |
> | Microsoft. resources/dağıtımlar/* | Dağıtım oluşturma ve yönetme |
> | Microsoft. resources/abonelikler/resourceGroups/Read | Kaynak gruplarını alır veya listeler. |
> | Microsoft. support/* | Destek bileti oluşturma ve güncelleştirme |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Read Runbook properties - to be able to create Jobs of the runbook.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/5fb5aef8-1081-4b8e-bb16-9d5d0385bab5",
  "name": "5fb5aef8-1081-4b8e-bb16-9d5d0385bab5",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Automation/automationAccounts/runbooks/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Automation Runbook Operator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="azure-connected-machine-onboarding"></a>Azure bağlı makine ekleme

, Azure bağlantılı makineler ekleyebilir.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. HybridCompute/makineler/okuma | Tüm Azure Arc makinelerini okuyun |
> | Microsoft. HybridCompute/makineler/yazma | Bir Azure Arc makinesi yazar |
> | Microsoft. GuestConfiguration/Guestconfigurationatamaları/okuma | Konuk yapılandırma atamasını al. |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can onboard Azure Connected Machines.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/b64e21ea-ac4e-4cdf-9dc9-5b892992bee7",
  "name": "b64e21ea-ac4e-4cdf-9dc9-5b892992bee7",
  "permissions": [
    {
      "actions": [
        "Microsoft.HybridCompute/machines/read",
        "Microsoft.HybridCompute/machines/write",
        "Microsoft.GuestConfiguration/guestConfigurationAssignments/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Connected Machine Onboarding",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="azure-connected-machine-resource-administrator"></a>Azure bağlı makine kaynak yöneticisi

Azure bağlı makinelerini okuyabilir, yazabilir, silebilir ve yeniden ekleyebilir.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. HybridCompute/makineler/okuma | Tüm Azure Arc makinelerini okuyun |
> | Microsoft. HybridCompute/makineler/yazma | Bir Azure Arc makinesi yazar |
> | Microsoft. HybridCompute/makineler/Sil | Azure yay makinelerini siler |
> | Microsoft. HybridCompute/makineler/yeniden bağlan/eylem | Bir Azure yay makinelerini yeniden bağlar |
> | Microsoft. HybridCompute/makineler/uzantılar/yazma | Bir Azure Arc uzantısını yükleme veya güncelleştirme |
> | Microsoft. HybridCompute/*/Read |  |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can read, write, delete and re-onboard Azure Connected Machines.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/cd570a14-e51a-42ad-bac8-bafd67325302",
  "name": "cd570a14-e51a-42ad-bac8-bafd67325302",
  "permissions": [
    {
      "actions": [
        "Microsoft.HybridCompute/machines/read",
        "Microsoft.HybridCompute/machines/write",
        "Microsoft.HybridCompute/machines/delete",
        "Microsoft.HybridCompute/machines/reconnect/action",
        "Microsoft.HybridCompute/machines/extensions/write",
        "Microsoft.HybridCompute/*/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Connected Machine Resource Administrator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="billing-reader"></a>Faturalama Okuyucusu

Faturalandırma verilerine okuma erişimi sağlar

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. Authorization/*/Read | Rolleri ve rol atamalarını oku |
> | Microsoft. faturalandırma/*/Read | Fatura bilgilerini okuyun |
> | Microsoft. Commerce/*/Read |  |
> | Microsoft. tüketim/*/Read |  |
> | Microsoft. Management/managementGroups/Read | Kimliği doğrulanmış kullanıcı için Yönetim gruplarını listeleyin. |
> | Microsoft. CostManagement/*/Read |  |
> | Microsoft. support/* | Destek bileti oluşturma ve güncelleştirme |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows read access to billing data",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/fa23ad8b-c56e-40d8-ac0c-ce449e1d2c64",
  "name": "fa23ad8b-c56e-40d8-ac0c-ce449e1d2c64",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Billing/*/read",
        "Microsoft.Commerce/*/read",
        "Microsoft.Consumption/*/read",
        "Microsoft.Management/managementGroups/read",
        "Microsoft.CostManagement/*/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Billing Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="blueprint-contributor"></a>Blueprint Katılımcısı

Şema tanımlarını yönetebilir, ancak atamazsınız.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. Authorization/*/Read | Rolleri ve rol atamalarını oku |
> | Microsoft. Blueprint/planlar/* | Şema tanımlarını veya şema yapılarını oluşturun ve yönetin. |
> | Microsoft. resources/abonelikler/resourceGroups/Read | Kaynak gruplarını alır veya listeler. |
> | Microsoft. resources/dağıtımlar/* | Dağıtım oluşturma ve yönetme |
> | Microsoft. support/* | Destek bileti oluşturma ve güncelleştirme |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can manage blueprint definitions, but not assign them.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/41077137-e803-4205-871c-5a86e6a753b4",
  "name": "41077137-e803-4205-871c-5a86e6a753b4",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Blueprint/blueprints/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Blueprint Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="blueprint-operator"></a>Blueprint Işleci

Varolan yayımlanmış şemaları atayabilir, ancak yeni şemaları oluşturamaz. Bu, yalnızca atama Kullanıcı tarafından atanan yönetilen bir kimlikle yapıldığında işe yarar.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. Authorization/*/Read | Rolleri ve rol atamalarını oku |
> | Microsoft. Blueprint/Blueprintasbir/* | Şema atamaları oluşturun ve yönetin. |
> | Microsoft. resources/abonelikler/resourceGroups/Read | Kaynak gruplarını alır veya listeler. |
> | Microsoft. resources/dağıtımlar/* | Dağıtım oluşturma ve yönetme |
> | Microsoft. support/* | Destek bileti oluşturma ve güncelleştirme |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can assign existing published blueprints, but cannot create new blueprints. NOTE: this only works if the assignment is done with a user-assigned managed identity.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/437d2ced-4a38-4302-8479-ed2bcb43d090",
  "name": "437d2ced-4a38-4302-8479-ed2bcb43d090",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Blueprint/blueprintAssignments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Blueprint Operator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="cost-management-contributor"></a>Maliyet yönetimi Katılımcısı

Maliyetleri görüntüleyebilir ve maliyet yapılandırmasını yönetebilir (örn. bütçeler, dışarı aktarmalar)

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. tüketim/* |  |
> | Microsoft. CostManagement/* |  |
> | Microsoft. faturalandırma/Billingdönemler/okuma |  |
> | Microsoft. resources/abonelikler/okuma | Aboneliklerin listesini alır. |
> | Microsoft. resources/abonelikler/resourceGroups/Read | Kaynak gruplarını alır veya listeler. |
> | Microsoft. support/* | Destek bileti oluşturma ve güncelleştirme |
> | Microsoft. danışman/yapılandırma/okuma | Yapılandırma al |
> | Microsoft. Advisor/öneriler/okuma | Önerileri okur |
> | Microsoft. Management/managementGroups/Read | Kimliği doğrulanmış kullanıcı için Yönetim gruplarını listeleyin. |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can view costs and manage cost configuration (e.g. budgets, exports)",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/434105ed-43f6-45c7-a02f-909b2ba83430",
  "name": "434105ed-43f6-45c7-a02f-909b2ba83430",
  "permissions": [
    {
      "actions": [
        "Microsoft.Consumption/*",
        "Microsoft.CostManagement/*",
        "Microsoft.Billing/billingPeriods/read",
        "Microsoft.Resources/subscriptions/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*",
        "Microsoft.Advisor/configurations/read",
        "Microsoft.Advisor/recommendations/read",
        "Microsoft.Management/managementGroups/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Cost Management Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="cost-management-reader"></a>Maliyet yönetimi okuyucusu

Maliyet verilerini ve yapılandırmayı görüntüleyebilir (örneğin, bütçeler, dışarı aktarmalar)

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. tüketim/*/Read |  |
> | Microsoft. CostManagement/*/Read |  |
> | Microsoft. faturalandırma/Billingdönemler/okuma |  |
> | Microsoft. resources/abonelikler/okuma | Aboneliklerin listesini alır. |
> | Microsoft. resources/abonelikler/resourceGroups/Read | Kaynak gruplarını alır veya listeler. |
> | Microsoft. support/* | Destek bileti oluşturma ve güncelleştirme |
> | Microsoft. danışman/yapılandırma/okuma | Yapılandırma al |
> | Microsoft. Advisor/öneriler/okuma | Önerileri okur |
> | Microsoft. Management/managementGroups/Read | Kimliği doğrulanmış kullanıcı için Yönetim gruplarını listeleyin. |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Can view cost data and configuration (e.g. budgets, exports)",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/72fafb9e-0641-4937-9268-a91bfd8191a3",
  "name": "72fafb9e-0641-4937-9268-a91bfd8191a3",
  "permissions": [
    {
      "actions": [
        "Microsoft.Consumption/*/read",
        "Microsoft.CostManagement/*/read",
        "Microsoft.Billing/billingPeriods/read",
        "Microsoft.Resources/subscriptions/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*",
        "Microsoft.Advisor/configurations/read",
        "Microsoft.Advisor/recommendations/read",
        "Microsoft.Management/managementGroups/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Cost Management Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="managed-application-contributor-role"></a>Yönetilen uygulama katılımcısı rolü

Yönetilen uygulama kaynakları oluşturulmasına izin verir.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | */Read | Gizli dizileri hariç tüm türlerin kaynaklarını okuyun. |
> | Microsoft. Solutions/Applications/* |  |
> | Microsoft. Solutions/Register/ACTION | Çözümlere kaydolun. |
> | Microsoft. resources/abonelikler/resourceGroups/* |  |
> | Microsoft. resources/dağıtımlar/* | Dağıtım oluşturma ve yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows for creating managed application resources.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/641177b8-a67a-45b9-a033-47bc880bb21e",
  "name": "641177b8-a67a-45b9-a033-47bc880bb21e",
  "permissions": [
    {
      "actions": [
        "*/read",
        "Microsoft.Solutions/applications/*",
        "Microsoft.Solutions/register/action",
        "Microsoft.Resources/subscriptions/resourceGroups/*",
        "Microsoft.Resources/deployments/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Managed Application Contributor Role",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="managed-application-operator-role"></a>Yönetilen uygulama Işletmeni rolü

Yönetilen uygulama kaynakları üzerinde işlemleri okuyup gerçekleştirmenize olanak tanır

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | */Read | Gizli dizileri hariç tüm türlerin kaynaklarını okuyun. |
> | Microsoft. Solutions/Applications/Read | Uygulamaların bir listesini alır. |
> | Microsoft. Solutions/*/Action |  |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you read and perform actions on Managed Application resources",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/c7393b34-138c-406f-901b-d8cf2b17e6ae",
  "name": "c7393b34-138c-406f-901b-d8cf2b17e6ae",
  "permissions": [
    {
      "actions": [
        "*/read",
        "Microsoft.Solutions/applications/read",
        "Microsoft.Solutions/*/action"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Managed Application Operator Role",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="managed-applications-reader"></a>Yönetilen uygulamalar okuyucusu

Yönetilen bir uygulamadaki kaynakları okumanızı ve JıT erişimi isteğinizi yapmanızı sağlar.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | */Read | Gizli dizileri hariç tüm türlerin kaynaklarını okuyun. |
> | Microsoft. resources/dağıtımlar/* | Dağıtım oluşturma ve yönetme |
> | Microsoft. Solutions/Jrequests/* |  |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you read resources in a managed app and request JIT access.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/b9331d33-8a36-4f8c-b097-4f54124fdb44",
  "name": "b9331d33-8a36-4f8c-b097-4f54124fdb44",
  "permissions": [
    {
      "actions": [
        "*/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Solutions/jitRequests/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Managed Applications Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="managed-services-registration-assignment-delete-role"></a>Yönetilen hizmetler kayıt ataması rol silme

Yönetilen hizmetler kayıt ataması silme rolü, kiracı kullanıcılarının kiracıya atanan kayıt atamasını silmesine izin verir.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. ManagedServices/Registrationatamaları/okuma | Yönetilen hizmetler kayıt atamalarının bir listesini alır. |
> | Microsoft. ManagedServices/Registrationatamaları/silme | Yönetilen hizmetler kayıt atamasını kaldırır. |
> | Microsoft. ManagedServices/Operationdurumlar/okuma | Kaynak için işlem durumunu okur. |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Managed Services Registration Assignment Delete Role allows the managing tenant users to delete the registration assignment assigned to their tenant.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/91c1777a-f3dc-4fae-b103-61d183457e46",
  "name": "91c1777a-f3dc-4fae-b103-61d183457e46",
  "permissions": [
    {
      "actions": [
        "Microsoft.ManagedServices/registrationAssignments/read",
        "Microsoft.ManagedServices/registrationAssignments/delete",
        "Microsoft.ManagedServices/operationStatuses/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Managed Services Registration assignment Delete Role",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="management-group-contributor"></a>Yönetim grubu Katılımcısı

Yönetim grubu katılımcısı rolü

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. Management/managementGroups/Delete | Yönetim grubunu silin. |
> | Microsoft. Management/managementGroups/Read | Kimliği doğrulanmış kullanıcı için Yönetim gruplarını listeleyin. |
> | Microsoft. Management/managementGroups/abonelikler/Sil | Aboneliği yönetim grubundan kaldır. |
> | Microsoft. Management/managementGroups/abonelikler/Write | Mevcut aboneliği yönetim grubuyla ilişkilendirir. |
> | Microsoft. Management/managementGroups/Write | Bir yönetim grubu oluşturun veya güncelleştirin. |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Management Group Contributor Role",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/5d58bcaf-24a5-4b20-bdb6-eed9f69fbe4c",
  "name": "5d58bcaf-24a5-4b20-bdb6-eed9f69fbe4c",
  "permissions": [
    {
      "actions": [
        "Microsoft.Management/managementGroups/delete",
        "Microsoft.Management/managementGroups/read",
        "Microsoft.Management/managementGroups/subscriptions/delete",
        "Microsoft.Management/managementGroups/subscriptions/write",
        "Microsoft.Management/managementGroups/write"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Management Group Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="management-group-reader"></a>Yönetim grubu okuyucusu

Yönetim grubu okuyucusu rolü

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. Management/managementGroups/Read | Kimliği doğrulanmış kullanıcı için Yönetim gruplarını listeleyin. |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Management Group Reader Role",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/ac63b705-f282-497d-ac71-919bf39d939d",
  "name": "ac63b705-f282-497d-ac71-919bf39d939d",
  "permissions": [
    {
      "actions": [
        "Microsoft.Management/managementGroups/read"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Management Group Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="new-relic-apm-account-contributor"></a>Yeni relik APM hesabı Katılımcısı

New Relic Application Performance Management hesaplarını ve uygulamaları yönetmenize izin verir, ancak bunlara erişimi kalmaz.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. Authorization/*/Read | Rolleri ve rol atamalarını oku |
> | Microsoft. Insights/alertRules/* | Klasik ölçüm uyarısı oluşturma ve yönetme |
> | Microsoft. ResourceHealth/kullanılabilirlik Bilitydurumlar/okuma | Belirtilen kapsamdaki tüm kaynaklar için Kullanılabilirlik durumlarını alır |
> | Microsoft. resources/dağıtımlar/* | Dağıtım oluşturma ve yönetme |
> | Microsoft. resources/abonelikler/resourceGroups/Read | Kaynak gruplarını alır veya listeler. |
> | Microsoft. support/* | Destek bileti oluşturma ve güncelleştirme |
> | Newrelik. APM/hesaplar/* |  |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage New Relic Application Performance Management accounts and applications, but not access to them.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/5d28c62d-5b37-4476-8438-e587778df237",
  "name": "5d28c62d-5b37-4476-8438-e587778df237",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*",
        "NewRelic.APM/accounts/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "New Relic APM Account Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="policy-insights-data-writer-preview"></a>İlke öngörüleri veri yazıcısı (Önizleme)

Kaynak ilkelerine okuma erişimine ve kaynak bileşen ilkesi olaylarına yazma erişimine izin verir.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. Authorization/poliyasatamaları/okuma | İlke atama hakkında bilgi alın. |
> | Microsoft. Authorization/PolicyDefinitions/Read | Bir ilke tanımı hakkında bilgi alın. |
> | Microsoft. Authorization/policysetdefinitions/Read | İlke kümesi tanımı hakkında bilgi alın. |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | Microsoft. Policınghts/Checkdatapolicyuyumluluk/eylem | Veri ilkelerine karşı belirli bir bileşenin uyumluluk durumunu kontrol edin. |
> | Microsoft. Poliyeleghts/policyEvents/logDataEvents/eylem | Kaynak bileşen ilkesi olaylarını günlüğe kaydedin. |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Allows read access to resource policies and write access to resource component policy events.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/66bb4e9e-b016-4a94-8249-4c0511c2be84",
  "name": "66bb4e9e-b016-4a94-8249-4c0511c2be84",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/policyassignments/read",
        "Microsoft.Authorization/policydefinitions/read",
        "Microsoft.Authorization/policysetdefinitions/read"
      ],
      "notActions": [],
      "dataActions": [
        "Microsoft.PolicyInsights/checkDataPolicyCompliance/action",
        "Microsoft.PolicyInsights/policyEvents/logDataEvents/action"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Policy Insights Data Writer (Preview)",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="resource-policy-contributor"></a>Kaynak İlkesine Katkıda Bulunan

Kaynak ilkesi oluşturma/değiştirme, destek bileti oluşturma ve kaynakları/hiyerarşisi okuma haklarına sahip kullanıcılar.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | */Read | Gizli dizileri hariç tüm türlerin kaynaklarını okuyun. |
> | Microsoft. Authorization/poliyasatamaları/* | İlke atamaları oluşturma ve yönetme |
> | Microsoft. Authorization/PolicyDefinitions/* | İlke tanımları oluşturma ve yönetme |
> | Microsoft. Authorization/policysetdefinitions/* | İlke kümeleri oluşturma ve yönetme |
> | Microsoft. Poliyeleghts/* |  |
> | Microsoft. support/* | Destek bileti oluşturma ve güncelleştirme |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Users with rights to create/modify resource policy, create support ticket and read resources/hierarchy.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/36243c78-bf99-498c-9df9-86d9f8d28608",
  "name": "36243c78-bf99-498c-9df9-86d9f8d28608",
  "permissions": [
    {
      "actions": [
        "*/read",
        "Microsoft.Authorization/policyassignments/*",
        "Microsoft.Authorization/policydefinitions/*",
        "Microsoft.Authorization/policysetdefinitions/*",
        "Microsoft.PolicyInsights/*",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Resource Policy Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="site-recovery-contributor"></a>Katkıda bulunan Site Recovery

Kasa oluşturma ve rol atama dışında Site Recovery hizmetini yönetmenizi sağlar

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. Authorization/*/Read | Rolleri ve rol atamalarını oku |
> | Microsoft. Insights/alertRules/* | Klasik ölçüm uyarısı oluşturma ve yönetme |
> | Microsoft. Network/virtualNetworks/Read | Sanal ağ tanımını al |
> | Microsoft. RecoveryServices/konumlar/allocatedStamp/Read | GetAllocatedStamp, hizmet tarafından kullanılan iç işlemdir |
> | Microsoft. RecoveryServices/Locations/Allocatedamga/eylem | AllocateStamp, hizmet tarafından kullanılan iç işlemdir |
> | Microsoft. RecoveryServices/Vaults/sertifikalar/yazma | Kaynak sertifikası güncelleştirme işlemi kaynak/kasa kimlik bilgisi sertifikasını güncelleştirir. |
> | Microsoft. RecoveryServices/Kasaults/Extendedınformation/* | Kasa ile ilgili genişletilmiş bilgileri oluşturma ve yönetme |
> | Microsoft. RecoveryServices/Vaults/okuma | Kasayı al işlemi, ' kasa ' türündeki Azure kaynağını temsil eden bir nesneyi alır |
> | Microsoft. RecoveryServices/Vaults/refreshContainers/Read |  |
> | Microsoft. RecoveryServices/Vaults/registeredIdentities/* | Kayıtlı kimlikler oluşturma ve yönetme |
> | Microsoft. RecoveryServices/Vaults/replicationAlertSettings/* | Çoğaltma uyarı ayarlarını oluşturma veya güncelleştirme |
> | Microsoft. RecoveryServices/kasaults/replicationEvents/okuma | Tüm olayları okuyun |
> | Microsoft. RecoveryServices/Vaults/Replicationdokuların/* | Çoğaltma yapıları oluşturma ve yönetme |
> | Microsoft. RecoveryServices/Vaults/replicationJobs/* | Çoğaltma işleri oluşturma ve yönetme |
> | Microsoft. RecoveryServices/Vaults/replicationPolicies/* | Çoğaltma ilkeleri oluşturma ve yönetme |
> | Microsoft. RecoveryServices/Vaults/Replicationrecoveryplan/* | Kurtarma planlarını oluşturma ve yönetme |
> | Microsoft. RecoveryServices/Vaults/storageConfig/* | Kurtarma Hizmetleri kasasının depolama yapılandırmasını oluşturma ve yönetme |
> | Microsoft. RecoveryServices/Vaults/TokenInfo/Read |  |
> | Microsoft. RecoveryServices/Kasaults/kullanımlar/okuma | Bir kurtarma hizmetleri Kasası için kullanım ayrıntılarını döndürür. |
> | Microsoft. RecoveryServices/Vaults/vaultTokens/Read | Kasa düzeyi arka uç işlemlerine ait kasa belirteci almak için kasa belirteci işlemi kullanılabilir. |
> | Microsoft. RecoveryServices/Vaults/monitoringAlerts/* | Kurtarma Hizmetleri Kasası için uyarıları okuyun |
> | Microsoft. RecoveryServices/Vaults/monitoringConfigurations/notificationConfiguration/Read |  |
> | Microsoft. ResourceHealth/kullanılabilirlik Bilitydurumlar/okuma | Belirtilen kapsamdaki tüm kaynaklar için Kullanılabilirlik durumlarını alır |
> | Microsoft. resources/dağıtımlar/* | Dağıtım oluşturma ve yönetme |
> | Microsoft. resources/abonelikler/resourceGroups/Read | Kaynak gruplarını alır veya listeler. |
> | Microsoft. Storage/storageAccounts/Read | Depolama hesaplarının listesini döndürür veya belirtilen depolama hesabının özelliklerini alır. |
> | Microsoft. RecoveryServices/Vaults/replicationOperationStatus/Read | Tüm kasa çoğaltma Işlemi durumunu okuyun |
> | Microsoft. support/* | Destek bileti oluşturma ve güncelleştirme |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage Site Recovery service except vault creation and role assignment",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/6670b86e-a3f7-4917-ac9b-5d6ab1be4567",
  "name": "6670b86e-a3f7-4917-ac9b-5d6ab1be4567",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Network/virtualNetworks/read",
        "Microsoft.RecoveryServices/locations/allocatedStamp/read",
        "Microsoft.RecoveryServices/locations/allocateStamp/action",
        "Microsoft.RecoveryServices/Vaults/certificates/write",
        "Microsoft.RecoveryServices/Vaults/extendedInformation/*",
        "Microsoft.RecoveryServices/Vaults/read",
        "Microsoft.RecoveryServices/Vaults/refreshContainers/read",
        "Microsoft.RecoveryServices/Vaults/registeredIdentities/*",
        "Microsoft.RecoveryServices/vaults/replicationAlertSettings/*",
        "Microsoft.RecoveryServices/vaults/replicationEvents/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/*",
        "Microsoft.RecoveryServices/vaults/replicationJobs/*",
        "Microsoft.RecoveryServices/vaults/replicationPolicies/*",
        "Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/*",
        "Microsoft.RecoveryServices/Vaults/storageConfig/*",
        "Microsoft.RecoveryServices/Vaults/tokenInfo/read",
        "Microsoft.RecoveryServices/Vaults/usages/read",
        "Microsoft.RecoveryServices/Vaults/vaultTokens/read",
        "Microsoft.RecoveryServices/Vaults/monitoringAlerts/*",
        "Microsoft.RecoveryServices/Vaults/monitoringConfigurations/notificationConfiguration/read",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Storage/storageAccounts/read",
        "Microsoft.RecoveryServices/vaults/replicationOperationStatus/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Site Recovery Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="site-recovery-operator"></a>Site Recovery Işleci

Yük devretme ve yeniden çalışma ve diğer Site Recovery yönetim işlemlerini gerçekleştirmenize izin verir

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. Authorization/*/Read | Rolleri ve rol atamalarını oku |
> | Microsoft. Insights/alertRules/* | Klasik ölçüm uyarısı oluşturma ve yönetme |
> | Microsoft. Network/virtualNetworks/Read | Sanal ağ tanımını al |
> | Microsoft. RecoveryServices/konumlar/allocatedStamp/Read | GetAllocatedStamp, hizmet tarafından kullanılan iç işlemdir |
> | Microsoft. RecoveryServices/Locations/Allocatedamga/eylem | AllocateStamp, hizmet tarafından kullanılan iç işlemdir |
> | Microsoft. RecoveryServices/Kasaults/Extendedınformation/Read | Genişletilmiş bilgileri al işlemi, bir nesnenin,? Kasası türünde Azure kaynağını temsil eden genişletilmiş bilgilerini alır mi? |
> | Microsoft. RecoveryServices/Vaults/okuma | Kasayı al işlemi, ' kasa ' türündeki Azure kaynağını temsil eden bir nesneyi alır |
> | Microsoft. RecoveryServices/Vaults/refreshContainers/Read |  |
> | Microsoft. RecoveryServices/Vaults/registeredIdentities/operationResults/Read | Işlem sonuçlarını al işlemi, zaman uyumsuz olarak gönderilen işlemin işlem durumunu ve sonucunu almak için kullanılabilir |
> | Microsoft. RecoveryServices/Vaults/registeredIdentities/Read | Kapsayıcıları al işlemi, bir kaynak için kayıtlı olan kapsayıcıları almak için kullanılabilir. |
> | Microsoft. RecoveryServices/Vaults/replicationAlertSettings/Read | Tüm uyarı ayarlarını okuyun |
> | Microsoft. RecoveryServices/kasaults/replicationEvents/okuma | Tüm olayları okuyun |
> | Microsoft. RecoveryServices/kasaults/Replicationdokuların/Checktutarlılığı/eylemi | Dokunun tutarlılığını denetler |
> | Microsoft. RecoveryServices/Vaults/Replicationyapılar/okuma | Tüm yapıları okuyun |
> | Microsoft. RecoveryServices/Vaults/Replicationdokuların/yeniden Ilişkilendirilmesi/eylem | Ağ geçidini yeniden ilişkilendir |
> | Microsoft. RecoveryServices/kasaults/Replicationyapılar/yenilenebilir sertifika/eylem | Doku için Sertifikayı Yenile |
> | Microsoft. RecoveryServices/Vaults/Replicationdokular/replicationNetworks/Read | Tüm ağları okuyun |
> | Microsoft. RecoveryServices/Vaults/Replicationyapılar/replicationNetworks/replicationNetworkMappings/Read | Tüm ağ eşlemelerini okuyun |
> | Microsoft. RecoveryServices/Vaults/Replicationyapılar/replicationProtectionContainers/Read | Tüm koruma kapsayıcılarını okuyun |
> | Microsoft. RecoveryServices/Vaults/Replicationyapılar/replicationProtectionContainers/Replicationkorunabilir | Korunabilir öğeleri oku |
> | Microsoft. RecoveryServices/Vaults/Replicationyapılar/replicationProtectionContainers/Replicationkorunabilir/applyRecoveryPoint/Action | Kurtarma noktası Uygula |
> | Microsoft. RecoveryServices/Vaults/Replicationdokuları/replicationProtectionContainers/Replicationkorunabilir/işlem | Yük devretme yürütmesi |
> | Microsoft. RecoveryServices/Vaults/Replicationdokuların/replicationProtectionContainers/Replicationkorunabilir/Plannedyük devretme/eylem | Planlı Yük Devretme |
> | Microsoft. RecoveryServices/Vaults/Replicationdokuların/replicationProtectionContainers/Replicationkorunabilir/Read | Tüm korumalı öğeleri okuyun |
> | Microsoft. RecoveryServices/Vaults/Replicationyapılar/replicationProtectionContainers/Replicationkorunabilir/recoveryPoints/Read | Tüm çoğaltma kurtarma noktalarını okuyun |
> | Microsoft. RecoveryServices/Vaults/Replicationdokuların/replicationProtectionContainers/Replicationkorunabilir/\ çoğaltma/eylem | Çoğaltmayı Onar |
> | Microsoft. RecoveryServices/Vaults/Replicationyapılar/replicationProtectionContainers/Replicationkorudıtems/reProtect/ACTION | Korumalı öğeyi yeniden koru |
> | Microsoft. RecoveryServices/Vaults/Replicationyapılar/replicationProtectionContainers/switchprotection/eylem | Koruma kapsayıcısını Değiştir |
> | Microsoft. RecoveryServices/Vaults/Replicationdokuları/replicationProtectionContainers/Replicationkorunabilir/TEMS/testFailover/ACTION | Test Yük Devretmesi |
> | Microsoft. RecoveryServices/Vaults/Replicationdokuları/replicationProtectionContainers/Replicationkorunabilir/test Failovercleanup/Action | Yük devretme sınamasını Temizleme |
> | Microsoft. RecoveryServices/Vaults/Replicationyapılar/replicationProtectionContainers/Replicationkorunabilir/unplannedFailover/ACTION | Yük devretme |
> | Microsoft. RecoveryServices/Vaults/Replicationdokuların/replicationProtectionContainers/Replicationkorunabilir/Updatebir Dıtems/Update, Ityservice/Action | Mobility hizmetini Güncelleştir |
> | Microsoft. RecoveryServices/Vaults/Replicationyapılar/replicationProtectionContainers/replicationProtectionContainerMappings/Read | Tüm koruma kapsayıcısı eşlemelerini okuyun |
> | Microsoft. RecoveryServices/Vaults/Replicationdokuların/replicationRecoveryServicesProviders/Read | Tüm kurtarma hizmetleri sağlayıcılarını okuyun |
> | Microsoft. RecoveryServices/Vaults/Replicationyapılar/replicationRecoveryServicesProviders/refreshProvider/Action | Sağlayıcıyı Yenile |
> | Microsoft. RecoveryServices/Vaults/Replicationyapılar/Replicationstorageclassıfler/okuma | Tüm depolama sınıflandırmalarını okuyun |
> | Microsoft. RecoveryServices/Vaults/Replicationdokuların/Replicationstorageclassıflıya/Replicationstorageclassıficationmappings/Read | Tüm depolama sınıflandırması eşlemelerini okuyun |
> | Microsoft. RecoveryServices/Vaults/Replicationyapılar/replicationvCenters/Read | Tüm sanal merkezleri okuyun |
> | Microsoft. RecoveryServices/Vaults/replicationJobs/* | Çoğaltma işleri oluşturma ve yönetme |
> | Microsoft. RecoveryServices/kasaults/replicationPolicies/okuma | Tüm Ilkeleri okuyun |
> | Microsoft. RecoveryServices/Vaults/Replicationrecoveryplan/Failovercommıt/Action | Yük devretme yürütmesi kurtarma planı |
> | Microsoft. RecoveryServices/Vaults/Replicationrecoveryplan/plannedFailover/ACTION | Planlı Yük devretme kurtarma planı |
> | Microsoft. RecoveryServices/Vaults/Replicationrecoveryplan/okuma | Tüm kurtarma planlarını okuyun |
> | Microsoft. RecoveryServices/Vaults/Replicationrecoveryplan/yeniden koruma/eylem | Kurtarma planını yeniden koru |
> | Microsoft. RecoveryServices/Vaults/Replicationrecoveryplan/testFailover/Action | Yük devretme kurtarma planını sına |
> | Microsoft. RecoveryServices/Vaults/Replicationrecoveryplan/testFailoverCleanup/Action | Sınama yük devretmesi Temizleme kurtarma planı |
> | Microsoft. RecoveryServices/Vaults/Replicationrecoveryplan/unplannedFailover/ACTION | Yük devretme kurtarma planı |
> | Microsoft. RecoveryServices/Vaults/monitoringAlerts/* | Kurtarma Hizmetleri Kasası için uyarıları okuyun |
> | Microsoft. RecoveryServices/Vaults/monitoringConfigurations/notificationConfiguration/Read |  |
> | Microsoft. RecoveryServices/Vaults/storageConfig/Read |  |
> | Microsoft. RecoveryServices/Vaults/TokenInfo/Read |  |
> | Microsoft. RecoveryServices/Kasaults/kullanımlar/okuma | Bir kurtarma hizmetleri Kasası için kullanım ayrıntılarını döndürür. |
> | Microsoft. RecoveryServices/Vaults/vaultTokens/Read | Kasa düzeyi arka uç işlemlerine ait kasa belirteci almak için kasa belirteci işlemi kullanılabilir. |
> | Microsoft. ResourceHealth/kullanılabilirlik Bilitydurumlar/okuma | Belirtilen kapsamdaki tüm kaynaklar için Kullanılabilirlik durumlarını alır |
> | Microsoft. resources/dağıtımlar/* | Dağıtım oluşturma ve yönetme |
> | Microsoft. resources/abonelikler/resourceGroups/Read | Kaynak gruplarını alır veya listeler. |
> | Microsoft. Storage/storageAccounts/Read | Depolama hesaplarının listesini döndürür veya belirtilen depolama hesabının özelliklerini alır. |
> | Microsoft. support/* | Destek bileti oluşturma ve güncelleştirme |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you failover and failback but not perform other Site Recovery management operations",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/494ae006-db33-4328-bf46-533a6560a3ca",
  "name": "494ae006-db33-4328-bf46-533a6560a3ca",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Network/virtualNetworks/read",
        "Microsoft.RecoveryServices/locations/allocatedStamp/read",
        "Microsoft.RecoveryServices/locations/allocateStamp/action",
        "Microsoft.RecoveryServices/Vaults/extendedInformation/read",
        "Microsoft.RecoveryServices/Vaults/read",
        "Microsoft.RecoveryServices/Vaults/refreshContainers/read",
        "Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read",
        "Microsoft.RecoveryServices/Vaults/registeredIdentities/read",
        "Microsoft.RecoveryServices/vaults/replicationAlertSettings/read",
        "Microsoft.RecoveryServices/vaults/replicationEvents/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/checkConsistency/action",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/reassociateGateway/action",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/renewcertificate/action",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/replicationNetworkMappings/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectableItems/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/applyRecoveryPoint/action",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/failoverCommit/action",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/plannedFailover/action",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/recoveryPoints/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/repairReplication/action",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/reProtect/action",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/switchprotection/action",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/testFailover/action",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/testFailoverCleanup/action",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/unplannedFailover/action",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/updateMobilityService/action",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectionContainerMappings/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/refreshProvider/action",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/replicationStorageClassificationMappings/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationvCenters/read",
        "Microsoft.RecoveryServices/vaults/replicationJobs/*",
        "Microsoft.RecoveryServices/vaults/replicationPolicies/read",
        "Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/failoverCommit/action",
        "Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/plannedFailover/action",
        "Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/read",
        "Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/reProtect/action",
        "Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/testFailover/action",
        "Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/testFailoverCleanup/action",
        "Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/unplannedFailover/action",
        "Microsoft.RecoveryServices/Vaults/monitoringAlerts/*",
        "Microsoft.RecoveryServices/Vaults/monitoringConfigurations/notificationConfiguration/read",
        "Microsoft.RecoveryServices/Vaults/storageConfig/read",
        "Microsoft.RecoveryServices/Vaults/tokenInfo/read",
        "Microsoft.RecoveryServices/Vaults/usages/read",
        "Microsoft.RecoveryServices/Vaults/vaultTokens/read",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Storage/storageAccounts/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Site Recovery Operator",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="site-recovery-reader"></a>Site Recovery okuyucu

Site Recovery durumunu görüntülemenize izin verir, ancak diğer yönetim işlemlerini gerçekleştirmenize izin vermez

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. Authorization/*/Read | Rolleri ve rol atamalarını oku |
> | Microsoft. RecoveryServices/konumlar/allocatedStamp/Read | GetAllocatedStamp, hizmet tarafından kullanılan iç işlemdir |
> | Microsoft. RecoveryServices/Kasaults/Extendedınformation/Read | Genişletilmiş bilgileri al işlemi, bir nesnenin,? Kasası türünde Azure kaynağını temsil eden genişletilmiş bilgilerini alır mi? |
> | Microsoft. RecoveryServices/Kasaults/monitoringAlerts/okuma | Kurtarma Hizmetleri Kasası için uyarıları alır. |
> | Microsoft. RecoveryServices/Vaults/monitoringConfigurations/notificationConfiguration/Read |  |
> | Microsoft. RecoveryServices/Vaults/okuma | Kasayı al işlemi, ' kasa ' türündeki Azure kaynağını temsil eden bir nesneyi alır |
> | Microsoft. RecoveryServices/Vaults/refreshContainers/Read |  |
> | Microsoft. RecoveryServices/Vaults/registeredIdentities/operationResults/Read | Işlem sonuçlarını al işlemi, zaman uyumsuz olarak gönderilen işlemin işlem durumunu ve sonucunu almak için kullanılabilir |
> | Microsoft. RecoveryServices/Vaults/registeredIdentities/Read | Kapsayıcıları al işlemi, bir kaynak için kayıtlı olan kapsayıcıları almak için kullanılabilir. |
> | Microsoft. RecoveryServices/Vaults/replicationAlertSettings/Read | Tüm uyarı ayarlarını okuyun |
> | Microsoft. RecoveryServices/kasaults/replicationEvents/okuma | Tüm olayları okuyun |
> | Microsoft. RecoveryServices/Vaults/Replicationyapılar/okuma | Tüm yapıları okuyun |
> | Microsoft. RecoveryServices/Vaults/Replicationdokular/replicationNetworks/Read | Tüm ağları okuyun |
> | Microsoft. RecoveryServices/Vaults/Replicationyapılar/replicationNetworks/replicationNetworkMappings/Read | Tüm ağ eşlemelerini okuyun |
> | Microsoft. RecoveryServices/Vaults/Replicationyapılar/replicationProtectionContainers/Read | Tüm koruma kapsayıcılarını okuyun |
> | Microsoft. RecoveryServices/Vaults/Replicationyapılar/replicationProtectionContainers/Replicationkorunabilir | Korunabilir öğeleri oku |
> | Microsoft. RecoveryServices/Vaults/Replicationdokuların/replicationProtectionContainers/Replicationkorunabilir/Read | Tüm korumalı öğeleri okuyun |
> | Microsoft. RecoveryServices/Vaults/Replicationyapılar/replicationProtectionContainers/Replicationkorunabilir/recoveryPoints/Read | Tüm çoğaltma kurtarma noktalarını okuyun |
> | Microsoft. RecoveryServices/Vaults/Replicationyapılar/replicationProtectionContainers/replicationProtectionContainerMappings/Read | Tüm koruma kapsayıcısı eşlemelerini okuyun |
> | Microsoft. RecoveryServices/Vaults/Replicationdokuların/replicationRecoveryServicesProviders/Read | Tüm kurtarma hizmetleri sağlayıcılarını okuyun |
> | Microsoft. RecoveryServices/Vaults/Replicationyapılar/Replicationstorageclassıfler/okuma | Tüm depolama sınıflandırmalarını okuyun |
> | Microsoft. RecoveryServices/Vaults/Replicationdokuların/Replicationstorageclassıflıya/Replicationstorageclassıficationmappings/Read | Tüm depolama sınıflandırması eşlemelerini okuyun |
> | Microsoft. RecoveryServices/Vaults/Replicationyapılar/replicationvCenters/Read | Tüm sanal merkezleri okuyun |
> | Microsoft. RecoveryServices/kasaults/replicationJobs/okuma | Tüm Işleri okuyun |
> | Microsoft. RecoveryServices/kasaults/replicationPolicies/okuma | Tüm Ilkeleri okuyun |
> | Microsoft. RecoveryServices/Vaults/Replicationrecoveryplan/okuma | Tüm kurtarma planlarını okuyun |
> | Microsoft. RecoveryServices/Vaults/storageConfig/Read |  |
> | Microsoft. RecoveryServices/Vaults/TokenInfo/Read |  |
> | Microsoft. RecoveryServices/Kasaults/kullanımlar/okuma | Bir kurtarma hizmetleri Kasası için kullanım ayrıntılarını döndürür. |
> | Microsoft. RecoveryServices/Vaults/vaultTokens/Read | Kasa düzeyi arka uç işlemlerine ait kasa belirteci almak için kasa belirteci işlemi kullanılabilir. |
> | Microsoft. support/* | Destek bileti oluşturma ve güncelleştirme |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you view Site Recovery status but not perform other management operations",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/dbaa88c4-0c30-4179-9fb3-46319faa6149",
  "name": "dbaa88c4-0c30-4179-9fb3-46319faa6149",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.RecoveryServices/locations/allocatedStamp/read",
        "Microsoft.RecoveryServices/Vaults/extendedInformation/read",
        "Microsoft.RecoveryServices/Vaults/monitoringAlerts/read",
        "Microsoft.RecoveryServices/Vaults/monitoringConfigurations/notificationConfiguration/read",
        "Microsoft.RecoveryServices/Vaults/read",
        "Microsoft.RecoveryServices/Vaults/refreshContainers/read",
        "Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read",
        "Microsoft.RecoveryServices/Vaults/registeredIdentities/read",
        "Microsoft.RecoveryServices/vaults/replicationAlertSettings/read",
        "Microsoft.RecoveryServices/vaults/replicationEvents/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/replicationNetworkMappings/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectableItems/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/recoveryPoints/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectionContainerMappings/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/replicationStorageClassificationMappings/read",
        "Microsoft.RecoveryServices/vaults/replicationFabrics/replicationvCenters/read",
        "Microsoft.RecoveryServices/vaults/replicationJobs/read",
        "Microsoft.RecoveryServices/vaults/replicationPolicies/read",
        "Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/read",
        "Microsoft.RecoveryServices/Vaults/storageConfig/read",
        "Microsoft.RecoveryServices/Vaults/tokenInfo/read",
        "Microsoft.RecoveryServices/Vaults/usages/read",
        "Microsoft.RecoveryServices/Vaults/vaultTokens/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Site Recovery Reader",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="support-request-contributor"></a>Destek Isteği Katılımcısı

Destek istekleri oluşturmanızı ve yönetmenizi sağlar

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. Authorization/*/Read | Rolleri ve rol atamalarını oku |
> | Microsoft. resources/abonelikler/resourceGroups/Read | Kaynak gruplarını alır veya listeler. |
> | Microsoft. support/* | Destek bileti oluşturma ve güncelleştirme |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you create and manage Support requests",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/cfd33db0-3dd1-45e3-aa9d-cdbdf3b6f24e",
  "name": "cfd33db0-3dd1-45e3-aa9d-cdbdf3b6f24e",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Support Request Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="tag-contributor"></a>Etiket Katılımcısı

Varlıklarda kendilerine erişim sağlamamanıza gerek kalmadan varlıklarda etiketleri yönetmenizi sağlar.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. Authorization/*/Read | Rolleri ve rol atamalarını oku |
> | Microsoft. resources/abonelikler/resourceGroups/Read | Kaynak gruplarını alır veya listeler. |
> | Microsoft. resources/abonelikler/resourceGroups/kaynaklar/okuma | Kaynak grubunun kaynaklarını alır. |
> | Microsoft. resources/abonelikler/kaynaklar/okuma | Bir aboneliğin kaynaklarını alır. |
> | Microsoft. resources/dağıtımlar/* | Dağıtım oluşturma ve yönetme |
> | Microsoft. Insights/alertRules/* | Klasik ölçüm uyarısı oluşturma ve yönetme |
> | Microsoft. support/* | Destek bileti oluşturma ve güncelleştirme |
> | Microsoft. resources/Tags/* |  |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage tags on entities, without providing access to the entities themselves.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/4a9ae827-6dc8-4573-8ac7-8239d42aa03f",
  "name": "4a9ae827-6dc8-4573-8ac7-8239d42aa03f",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Resources/subscriptions/resourceGroups/resources/read",
        "Microsoft.Resources/subscriptions/resources/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.Support/*",
        "Microsoft.Resources/tags/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Tag Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

## <a name="other"></a>Diğer


### <a name="biztalk-contributor"></a>BizTalk Katılımcısı

BizTalk hizmetlerini yönetmenizi sağlar ancak onlara erişim izni vermez.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. Authorization/*/Read | Rolleri ve rol atamalarını oku |
> | Microsoft. BizTalkServices/BizTalk/* | BizTalk Hizmetleri oluşturma ve yönetme |
> | Microsoft. Insights/alertRules/* | Klasik ölçüm uyarısı oluşturma ve yönetme |
> | Microsoft. ResourceHealth/kullanılabilirlik Bilitydurumlar/okuma | Belirtilen kapsamdaki tüm kaynaklar için Kullanılabilirlik durumlarını alır |
> | Microsoft. resources/dağıtımlar/* | Dağıtım oluşturma ve yönetme |
> | Microsoft. resources/abonelikler/resourceGroups/Read | Kaynak gruplarını alır veya listeler. |
> | Microsoft. support/* | Destek bileti oluşturma ve güncelleştirme |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage BizTalk services, but not access to them.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/5e3c6656-6cfa-4708-81fe-0de47ac73342",
  "name": "5e3c6656-6cfa-4708-81fe-0de47ac73342",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.BizTalkServices/BizTalk/*",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "BizTalk Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="scheduler-job-collections-contributor"></a>Zamanlayıcı Iş koleksiyonları Katılımcısı

Zamanlayıcı iş koleksiyonlarını yönetmenizi sağlar, ancak bunlara erişimi kalmaz.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft. Authorization/*/Read | Rolleri ve rol atamalarını oku |
> | Microsoft. Insights/alertRules/* | Klasik ölçüm uyarısı oluşturma ve yönetme |
> | Microsoft. ResourceHealth/kullanılabilirlik Bilitydurumlar/okuma | Belirtilen kapsamdaki tüm kaynaklar için Kullanılabilirlik durumlarını alır |
> | Microsoft. resources/dağıtımlar/* | Dağıtım oluşturma ve yönetme |
> | Microsoft. resources/abonelikler/resourceGroups/Read | Kaynak gruplarını alır veya listeler. |
> | Microsoft. Scheduler/jobcollections/* | İş koleksiyonları oluşturma ve yönetme |
> | Microsoft. support/* | Destek bileti oluşturma ve güncelleştirme |
> | **NotActions** |  |
> | *yok* |  |
> | **Veri eylemleri** |  |
> | *yok* |  |
> | **NotDataActions** |  |
> | *yok* |  |

```json
{
  "assignableScopes": [
    "/"
  ],
  "description": "Lets you manage Scheduler job collections, but not access to them.",
  "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/188a0f2f-5c9e-469b-ae67-2aa5ce574b94",
  "name": "188a0f2f-5c9e-469b-ae67-2aa5ce574b94",
  "permissions": [
    {
      "actions": [
        "Microsoft.Authorization/*/read",
        "Microsoft.Insights/alertRules/*",
        "Microsoft.ResourceHealth/availabilityStatuses/read",
        "Microsoft.Resources/deployments/*",
        "Microsoft.Resources/subscriptions/resourceGroups/read",
        "Microsoft.Scheduler/jobcollections/*",
        "Microsoft.Support/*"
      ],
      "notActions": [],
      "dataActions": [],
      "notDataActions": []
    }
  ],
  "roleName": "Scheduler Job Collections Contributor",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

## <a name="next-steps"></a>Sonraki adımlar

- [Kaynak sağlayıcısını hizmetle Eşleştir](../azure-resource-manager/management/azure-services-resource-providers.md)
- [Azure özel rolleri](custom-roles.md)
- [Azure Güvenlik Merkezi'nde İzinler](../security-center/security-center-permissions.md)
