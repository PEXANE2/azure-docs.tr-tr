---
title: Azure RBAC için Azure yerleşik rolleri
description: Bu makalede, Azure rol tabanlı erişim denetimi (RBAC) için Azure yerleşik rolleri açıklanmaktadır. Eylemleri, Eylemleri, DataActions'ı ve NotDataActions'u listeler.
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
ms.date: 03/22/2020
ms.author: rolyon
ms.reviewer: bagovind
ms.custom: it-pro
ms.openlocfilehash: ba07a0b0d3be0366179bba14c786fafad8753dd7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80280703"
---
# <a name="azure-built-in-roles"></a>Azure yerleşik rolleri

[Azure rol tabanlı erişim denetiminde (RBAC),](overview.md) kullanıcılara, gruplara, hizmet ilkelerine ve yönetilen kimliklere atayabileceğiniz birkaç Azure yerleşik rolü vardır. Rol atamaları, Azure kaynaklarına erişimi denetleme şeklinizdir. Yerleşik roller kuruluşunuzun özel gereksinimlerini karşılamazsa, kendi Azure özel [rollerinizi](custom-roles.md)oluşturabilirsiniz.

Bu makalede, her zaman gelişmekte olan Azure kaynakları için yerleşik roller listelenir. En son rolleri almak için [Get-AzRoleDefinition](/powershell/module/az.resources/get-azroledefinition) veya [az rol tanım listesini](/cli/azure/role/definition#az-role-definition-list)kullanın. Azure Etkin Dizin (Azure AD) için yönetici rolleri arıyorsanız, [Azure Etkin Dizin'de Yönetici rol izinlerine](../active-directory/users-groups-roles/directory-assign-admin-roles.md)bakın.

## <a name="all"></a>Tümü

Aşağıdaki tablo, kısa bir açıklama ve her yerleşik rolün benzersiz kimliğini sağlar. `Actions`Listeyi `NotActions` `DataActions`görmek için rol adını ve `NotDataActions` her rol için seçin. Bu eylemlerin ne anlama geldiğini ve yönetim ve veri düzlemlerine nasıl uygulandığı hakkında bilgi için Azure [kaynakları için rol tanımlarını anlayın.](role-definitions.md)


> [!div class="mx-tableFixed"]
> | Yerleşik rol | Açıklama | Kimlik |
> | --- | --- | --- |
> | **Genel** |  |  |
> | [Katkıda Bulunan](#contributor) | Kaynaklara erişim izni vermek dışında her şeyi yönetmenize olanak tanır. | b24988ac-6180-42a0-ab88-20f7382dd24c |
> | [Sahibi](#owner) | Kaynaklara erişim de dahil olmak üzere her şeyi yönetmenize olanak tanır. | 8e3af657-a8ff-443c-a75c-2fe8c4bcb635 |
> | [Okuyucu](#reader) | Her şeyi görüntülemenizi sağlar, ancak herhangi bir değişiklik yapmaz. | acdd72a7-3385-48ef-bd42-f606fba81ae7 |
> | [Kullanıcı Erişimi Yöneticisi](#user-access-administrator) | Azure kaynaklarına kullanıcı erişimini yönetmenize olanak tanır. | 18d7d88d-d35e-4fb5-a5c3-7773c20a72d9 |
> | **İşlem** |  |  |
> | [Klasik Sanal Makine Katılımcısı](#classic-virtual-machine-contributor) | Klasik sanal makineleri yönetmenize olanak tanır, ancak bunlara erişmemenize ve bağlı oldukları sanal ağ veya depolama hesabını değil. | d73bb868-a0df-4d4d-bd69-98a00b01fccb |
> | [Sanal Makine Yöneticisi Girişi](#virtual-machine-administrator-login) | Sanal Makineleri portalda görüntüleyin ve yönetici olarak giriş yapın | 1c0163c0-47e6-4577-8991-ea5c82e286e4 |
> | [Sanal Makine Katılımcısı](#virtual-machine-contributor) | Sanal makineleri yönetmenize olanak tanır, ancak bunlara erişmez ve bağlı oldukları sanal ağ veya depolama hesabı nı değil. | 9980e02c-c2be-4d73-94e8-173b1dc7cf3c |
> | [Sanal Makine Kullanıcı Girişi](#virtual-machine-user-login) | Sanal Makineleri portalda görüntüleyin ve normal bir kullanıcı olarak giriş yapın. | fb879df8-f326-4884-b1cf-06f3ad86be52 |
> | **Ağ Oluşturma** |  |  |
> | [CDN Bitiş Noktası Katılımcısı](#cdn-endpoint-contributor) | CDN uç noktalarını yönetebilir, ancak diğer kullanıcılara erişim izni verilemez. | 426e0c7f-0c7e-4658-b36f-ff54d6c29b45 |
> | [CDN Uç Nokta Okuyucu](#cdn-endpoint-reader) | CDN uç noktalarını görüntüleyebilir, ancak değişiklik yapamaz. | 871e35f6-b5c1-49cc-a043-bde969a0f2cd |
> | [CDN Profil Katılımcısı](#cdn-profile-contributor) | CDN profillerini ve uç noktalarını yönetebilir, ancak diğer kullanıcılara erişim izni veremez. | ec156ff8-a8d1-4d15-830c-5b80698ca432 |
> | [CDN Profil Okuyucu](#cdn-profile-reader) | CDN profillerini ve uç noktalarını görüntüleyebilir, ancak değişiklik yapamaz. | 8f96442b-4075-438f-813d-ad51ab4019af |
> | [Klasik Ağ Katılımcısı](#classic-network-contributor) | Klasik ağları yönetmenize olanak tanır, ancak bunlara erişmemenizi sağlar. | b34d265f-36f7-4a0d-a4d4-e158ca92e90f |
> | [DNS Bölgesi Katılımcısı](#dns-zone-contributor) | Azure DNS'de DNS bölgelerini ve kayıt kümelerini yönetmenize olanak tanır, ancak bunlara kimlerin erişebilenleri denetlemenize izin vermez. | befefa01-2a29-4197-83a8-272ff33ce314 |
> | [Ağ Katılımcısı](#network-contributor) | Ağları yönetmenize olanak tanır, ancak bunlara erişmemenize olanak tanır. | 4d97b98b-1d4f-4787-a291-c67834d212e7 |
> | [Trafik Yöneticisi Katılımcı](#traffic-manager-contributor) | Trafik Yöneticisi profillerini yönetmenize olanak tanır, ancak bu profillere kimlerin erişebilenleri denetlemenize izin vermez. | a4b10055-b0c7-44c2-b00f-c7b5b3550cf7 |
> | **Depolama** |  |  |
> | [Avere Katılımcısı](#avere-contributor) | Bir Avere vFXT kümesi oluşturabilir ve yönetebilirsiniz. | 4f8fab4f-1852-4a58-a46a-8eaf358af14a |
> | [Avere Operatörü](#avere-operator) | Kümeyi yönetmek için Avere vFXT kümesi tarafından kullanılır | c025889f-8102-4ebf-b32c-fc0c6f0c6bd9 |
> | [Yedekleme Katılımcısı](#backup-contributor) | Yedekleme hizmetini yönetmenize olanak tanır, ancak kasa oluşturamaz ve başkalarına erişim veremez | 5e467623-bb1f-42f4-a55d-6e525e1384b |
> | [Yedekleme Operatörü](#backup-operator) | Yedeklemenin kaldırılması, kasa oluşturma ve başkalarına erişim izni dışında yedekleme hizmetlerini yönetmenize olanak tanır | 00c29273-979b-4161-815c-10b084fb9324 |
> | [Yedek Okuyucu](#backup-reader) | Yedekleme hizmetlerini görüntüleyebilir, ancak değişiklik yapamaz | a795c7a0-d4a2-40c1-ae25-d81f01202912 |
> | [Klasik Depolama Hesabı Katılımcısı](#classic-storage-account-contributor) | Klasik depolama hesaplarını yönetmenize olanak tanır, ancak bunlara erişmemenize olanak tanır. | 86e8f5dc-a6e9-4c67-9d15-de283e8eac25 |
> | [Klasik Depolama Hesabı Anahtar Operatör Hizmeti Rolü](#classic-storage-account-key-operator-service-role) | Klasik Depolama Hesabı Anahtar Operatörleri, Klasik Depolama Hesaplarında anahtarları listeleyebilir ve yeniden oluşturabilir | 985d6b00-f706-48f5-a6fe-d0ca12fb668d |
> | [Veri Kutusu Katılımcısı](#data-box-contributor) | Başkalarına erişim sağlamak dışında Veri Kutusu Hizmeti kapsamındaki her şeyi yönetmenize olanak tanır. | add466c9-e687-43fc-8d98-dfcf8d720be5 |
> | [Veri Kutusu Okuyucu](#data-box-reader) | Sipariş oluşturma veya sipariş ayrıntılarını düzenleme ve başkalarına erişim verme dışında Veri Kutusu Hizmetini yönetmenize olanak tanır. | 028f4ed7-e2a9-465e-a8f4-9c0ffdfdc027 |
> | [Veri Gölü Analytics Geliştiricisi](#data-lake-analytics-developer) | Kendi işlerinizi göndermenize, izlemenize ve yönetmenize olanak tanır, ancak Data Lake Analytics hesapları oluşturmamanıza veya silmemenizi sağlar. | 47b7735b-770e-4598-a7da-8b91488b4c88 |
> | [Okuyucu ve Veri Erişimi](#reader-and-data-access) | Her şeyi görüntülemenize olanak tanır, ancak bir depolama hesabı veya içerdiği kaynağı silmenize veya oluşturmanıza izin vermez. Ayrıca, depolama hesabı anahtarlarına erişim yoluyla bir depolama hesabında bulunan tüm verilere okuma/yazma erişimi sağlar. | c12c1c16-33a1-487b-954d-41c89c60f349 |
> | [Depolama Hesabı Katılımcısı](#storage-account-contributor) | Depolama hesaplarının yönetimine izin verir. Paylaşılan Anahtar yetkilendirmesi yoluyla verilere erişmek için kullanılabilen hesap anahtarına erişim sağlar. | 17d1049b-9a84-46fb-8f53-869881c3d3ab |
> | [Depolama Hesabı Anahtar Operatör Hizmeti Rolü](#storage-account-key-operator-service-role) | Depolama hesabı erişim anahtarlarını listeleme ve yenilemeye izin verir. | 81a9662b-bebf-436f-a333-f67b29880f12 |
> | [Depolama Blob Veri Katılımcısı](#storage-blob-data-contributor) | Azure Depolama kapsayıcılarını ve lekelerini okuyun, yazın ve silin. Belirli bir veri işlemi için hangi eylemlerin gerekli olduğunu öğrenmek [için, blob ve sıra veri işlemlerini çağırmak için İzinler'e](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations)bakın. | ba92f5b4-2d11-453d-a403-e96b0029c9fe |
> | [Depolama Blob Veri Sahibi](#storage-blob-data-owner) | POSIX erişim denetimi atama da dahil olmak üzere Azure Depolama blob kapsayıcılarına ve verilerine tam erişim sağlar. Belirli bir veri işlemi için hangi eylemlerin gerekli olduğunu öğrenmek [için, blob ve sıra veri işlemlerini çağırmak için İzinler'e](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations)bakın. | b7e6dc6d-f1e8-4753-8033-0f276bb0955b |
> | [Depolama Blob Veri Okuyucu](#storage-blob-data-reader) | Azure Depolama kapsayıcılarını ve lekelerini okuyun ve listelayın. Belirli bir veri işlemi için hangi eylemlerin gerekli olduğunu öğrenmek [için, blob ve sıra veri işlemlerini çağırmak için İzinler'e](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations)bakın. | 2a2b9908-6ea1-4ae2-8e65-a410df84e7d1 |
> | [Depolama Blob Delegator](#storage-blob-delegator) | Azure AD kimlik bilgileriyle imzalanmış bir kapsayıcı veya blob için paylaşılan erişim imzası oluşturmak için kullanılabilecek bir kullanıcı delegasyonu anahtarı alın. Daha fazla bilgi için [bkz.](https://docs.microsoft.com/rest/api/storageservices/create-user-delegation-sas) | db58b8e5-c6ad-4a2a-8342-4190687cbf4a |
> | [Depolama Dosyası Verileri Kobİ Payı Katılımcısı](#storage-file-data-smb-share-contributor) | Azure dosya paylaşımlarında dosyalara/dizinlerde erişimin okunmasına, yazılmasına ve silinmesine izin verir. Bu rolün Windows dosya sunucularında yerleşik eşdeğeri yoktur. | 0c867c2a-1d8c-454a-a3db-ab2ea1bdc8bb |
> | [Depolama Dosya Verileri Kobİ Payı Yüksek Katılımcı](#storage-file-data-smb-share-elevated-contributor) | Azure dosya paylaşımlarında dosya/dizinler üzerinde ABM'lerin okunmasına, yazılmasına, silinmesine ve değiştirilmesine olanak tanır. Bu rol, Windows dosya sunucularında değişiklik bir dosya payı ACL eşdeğerdir. | a7264617-510b-434b-a828-9731dc254ea7 |
> | [Depolama Dosyası Veri Kobİ Pay Okuyucu](#storage-file-data-smb-share-reader) | Azure dosya paylaşımlarında dosyalara/dizinlere erişim inokunmasını sağlar. Bu rol, Windows dosya sunucularında okunan bir dosya paylaşımı ACL'sine eşdeğerdir. | aba4ae5f-2193-4029-9191-0cb91df5e314 |
> | [Depolama Sırası Veri Katılımcısı](#storage-queue-data-contributor) | Azure Depolama kuyruklarını ve sıra iletilerini okuyun, yazın ve silin. Belirli bir veri işlemi için hangi eylemlerin gerekli olduğunu öğrenmek [için, blob ve sıra veri işlemlerini çağırmak için İzinler'e](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations)bakın. | 974c5e8b-45b9-4653-ba55-5f855d0fb88 |
> | [Depolama Sırası Veri İletisi İşlemcisi](#storage-queue-data-message-processor) | Azure Depolama kuyruğundan bir iletiyi gözetleme, alma ve silme. Belirli bir veri işlemi için hangi eylemlerin gerekli olduğunu öğrenmek [için, blob ve sıra veri işlemlerini çağırmak için İzinler'e](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations)bakın. | 8a0f0c08-91a1-4084-bc3d-661d67233fed |
> | [Depolama Sırası Veri İletisi Gönderen](#storage-queue-data-message-sender) | Azure Depolama kuyruğuna ileti ekleyin. Belirli bir veri işlemi için hangi eylemlerin gerekli olduğunu öğrenmek [için, blob ve sıra veri işlemlerini çağırmak için İzinler'e](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations)bakın. | c6a89b2d-59bc-44d0-9896-0f6e12d7b80a |
> | [Depolama Sırası Veri Okuyucu](#storage-queue-data-reader) | Azure Depolama kuyruklarını ve sıra iletilerini okuyun ve listelenin. Belirli bir veri işlemi için hangi eylemlerin gerekli olduğunu öğrenmek [için, blob ve sıra veri işlemlerini çağırmak için İzinler'e](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations)bakın. | 19e7f393-937e-4f77-808e-94535e297925 |
> | **Web** |  |  |
> | [Azure Haritalar Veri Okuyucu (Önizleme)](#azure-maps-data-reader-preview) | Azure haritaları hesabından haritayla ilgili verilere erişim izni verir. | 423170ca-a8f6-4b0f-8487-9e4eb8f49bfa |
> | [Arama Hizmeti Katılımcısı](#search-service-contributor) | Arama hizmetlerini yönetmenize olanak tanır, ancak bunlara erişmez. | 7ca78c08-252a-4471-8644-bb5ff32d4ba0 |
> | [Web Planı Katılımcısı](#web-plan-contributor) | Web sitelerinin web planlarını yönetmenize olanak tanır, ancak bunlara erişmemenize olanak tanır. | 2cc479cb-7b4d-49a8-b449-8c00fd0f0a4b |
> | [Web Sitesi Katılımcısı](#website-contributor) | Web sitelerini (web planlarını değil) yönetmenize olanak tanır, ancak bunlara erişmemenize olanak tanır. | de139f84-1756-47ae-9be6-808fbbe84772 |
> | **Kapsayıcılar** |  |  |
> | [AcrDelete](#acrdelete) | acr silme | c2f4ef07-c644-48eb-af81-4b1b4947fb11 |
> | [AcrImageSigner](#acrimagesigner) | acr görüntü imzalayan | 6cef56e8-d556-48e5-a04f-b8e64114680f |
> | [AcrPull](#acrpull) | acr çekme | 7f951dda-4ed3-4680-a7ca-43fe172d538d |
> | [AcrPush](#acrpush) | acr itme | 8311e382-0749-4cb8-b61a-304f252e45ec |
> | [AcrQuarantineReader](#acrquarantinereader) | acr karantina veri okuyucu | cdda3590-29a3-44f6-95f2-9f980659eb04 |
> | [AcrQuarantineWriter](#acrquarantinewriter) | acr karantina veri yazar | c8d4ff99-41c3-41a8-9f60-21dfdad59608 |
> | [Azure Kubernetes Hizmet Kümesi Yönetici Rolü](#azure-kubernetes-service-cluster-admin-role) | Liste küme yöneticisi kimlik bilgisi eylemi. | 0ab0b1a8-8aac-4efd-b8c2-3ee1fb270be8 |
> | [Azure Kubernetes Hizmet Kümesi Kullanıcı Rolü](#azure-kubernetes-service-cluster-user-role) | Liste küme kullanıcı kimlik bilgileri eylemi. | 4abbcc35-e782-43d8-92c5-2d3f1bd2253f |
> | **Veritaban -ları** |  |  |
> | [Cosmos DB Hesap Okuyucu Rolü](#cosmos-db-account-reader-role) | Azure Cosmos DB hesap verilerini okuyabilir. Azure Cosmos DB hesaplarını yönetmek için [DocumentDB Hesap](#documentdb-account-contributor) Katılımcısı'na bakın. | fbdf93bf-df7d-467e-a4d2-9458aa1360c8 |
> | [Cosmos DB Operatörü](#cosmos-db-operator) | Azure Cosmos DB hesaplarını yönetmenize olanak tanır, ancak bu hesaplardaki verilere erişmez. Hesap anahtarlarına ve bağlantı dizelerine erişimi engeller. | 230815da-be43-4aae-9cb4-875f7bd000aa |
> | [CosmosBackupOperator](#cosmosbackupoperator) | Cosmos DB veritabanı veya bir hesap için bir kapsayıcı için geri yükleme isteği gönderebilir | db7b14f2-5adf-42da-9f96-f2ee17bab5cb |
> | [DocumentDB Hesap Katılımcısı](#documentdb-account-contributor) | Azure Cosmos DB hesaplarını yönetebilirsiniz. Azure Cosmos DB eskiden DocumentDB olarak bilinir. | 5bd9cd88-fe45-4216-938b-f97437e15450 |
> | [Redis Önbellek Katkıda Bulunan](#redis-cache-contributor) | Redis önbelleklerini yönetmenize izin verir, ancak bunlara erişmemenize izin verir. | e0f68234-74aa-48ed-b826-c38b57376e17 |
> | [SQL DB Katılımcısı](#sql-db-contributor) | SQL veritabanlarını yönetmenize olanak tanır, ancak bunlara erişmez. Ayrıca, güvenlikle ilgili ilkelerini veya üst SQL sunucularını yönetemezsiniz. | 9b7fa17d-e63e-47b0-bb0a-15c516ac86ec |
> | [SQL Yönetilen Örnek Katılımcı](#sql-managed-instance-contributor) | SQL Yönetilen Örnekleri ve gerekli ağ yapılandırmasını yönetmenize olanak tanır, ancak başkalarına erişim veremez. | 4939a1f6-9ae0-4e48-a1e0-f2cbe897382d |
> | [SQL Güvenlik Yöneticisi](#sql-security-manager) | SQL sunucularının ve veritabanlarının güvenlikle ilgili ilkelerini yönetmenize olanak tanır, ancak bunlara erişmez. | 056cd41c-7e88-42e1-933e-88ba6a50c9c3 |
> | [SQL Server Katılımcısı](#sql-server-contributor) | SQL sunucularını ve veritabanlarını yönetmenize olanak tanır, ancak bunlara erişmez ve güvenlikle ilgili ilkelerini yönetmez. | 6d8ee4ec-f05a-4a1d-8b00-a9b17e38b437 |
> | **Analiz** |  |  |
> | [Azure Etkinlik Hub'ları Veri Sahibi](#azure-event-hubs-data-owner) | Azure Etkinlik Hub'ları kaynaklarına tam erişim sağlar. | f526a384-b230-433a-b45c-95f59c4a2dec |
> | [Azure Etkinlik Hub'ları Veri Alıcısı](#azure-event-hubs-data-receiver) | Azure Etkinlik Hub'ları kaynaklarına erişim sağlar. | a638d3c7-ab3a-418d-83e6-5f17a39d4fde |
> | [Azure Olay Hub'ları Veri Gönderen](#azure-event-hubs-data-sender) | Azure Etkinlik Hub'ları kaynaklarına erişim gönderme olanağı sağlar. | 2b629674-e913-4c01-ae53-ef4638d8f975 |
> | [Veri Fabrikası Katılımcısı](#data-factory-contributor) | Veri fabrikalarının yanı sıra içlerindeki alt kaynaklar oluşturun ve yönetin. | 673868aa-7521-48a0-acc6-0f60742d39f5 |
> | [Veri Purger](#data-purger) | Analiz verilerini temizleyebilir | 150f5e0c-0603-4f03-8c7f-cf70034c4e90 |
> | [HDInsight Küme Operatörü](#hdinsight-cluster-operator) | HDInsight küme yapılandırmalarını okumanızı ve değiştirmenizi sağlar. | 61ed4efc-fab3-44fd-b111-e24485cc132a |
> | [HDInsight Etki Alanı Hizmetleri Katılımcısı](#hdinsight-domain-services-contributor) | HDInsight Kurumsal Güvenlik Paketi için gerekli Olan Etki Alanı Hizmetlerini Okuyabilir, Oluşturabilir, Değiştirebilir ve Silebilirsiniz | 8d8d5a11-05d3-4bda-a417-a08778121c7c |
> | [Log Analytics Katkıda Bulunan](#log-analytics-contributor) | Log Analytics Katılımcısı tüm izleme verilerini okuyabilir ve izleme ayarlarını düzeltebilir. İzleme ayarlarını düzenleme, VM uzantısını VM'lere eklemeyi içerir; Azure Depolama'dan günlük koleksiyonunu yapılandırabilmek için depolama hesabı anahtarlarını okuma; Otomasyon hesapları oluşturma ve yapılandırma; çözümler ekleme; ve Azure tanılamalarını tüm Azure kaynaklarında yapılandırma. | 92aaf0da-9dab-42b6-94a3-d43ce8d16293 |
> | [Log Analytics Okuyucusu](#log-analytics-reader) | Log Analytics Reader, Azure tanılama yapılandırmasını tüm Azure kaynaklarında görüntülemek de dahil olmak üzere tüm izleme verilerinin yanı sıra izleme ayarlarını görüntüleyebilir ve görüntüleyebilir. | 73c42c96-874c-492b-b04d-ab87d138a893 |
> | **Blok Zinciri** |  |  |
> | [Blockchain Üye Düğümü Erişimi (Önizleme)](#blockchain-member-node-access-preview) | Blockchain Üye düğümlerine erişim sağlar | 31a002a1-acaf-453e-8a5b-297c9ca1ea24 |
> | **AI + makine öğrenimi** |  |  |
> | [Bilişsel Hizmetler Katılımcısı](#cognitive-services-contributor) | Bilişsel Hizmetler'in anahtarlarını oluşturmanızı, okumanızı, güncellemenizi, silmenizi ve yönetmenize olanak tanır. | 25fbc0a9-bd7c-42a3-aa1a-3b75d497ee68 |
> | [Bilişsel Hizmetler Veri Okuyucu (Önizleme)](#cognitive-services-data-reader-preview) | Bilişsel Hizmetler verilerini okumanızı sağlar. | b59867f0-fa02-499b-be73-45a86b5b3e1c |
> | [Bilişsel Hizmetler Kullanıcı](#cognitive-services-user) | Bilişsel Hizmetler'in anahtarlarını okumanızı ve listelemenizi sağlar. | a97b65f3-24c7-4388-baec-2e87135dc908 |
> | **Karma gerçeklik** |  |  |
> | [Uzamsal Çapalar Hesap Katılımcısı](#spatial-anchors-account-contributor) | Hesabınızdaki uzamsal çapaları yönetmenize olanak tanır, ancak bunları silmez | 8bbe83f1-e2a6-4df7-8cb4-4e04d4e5c827 |
> | [Uzamsal Çapalar Hesap Sahibi](#spatial-anchors-account-owner) | Hesabınızdaki uzamsal çapaları silme dahil olmak üzere yönetmenize olanak tanır | 70bbe301-9835-447d-afdd-19eb3167307c |
> | [Uzamsal Çapalar Hesap Okuyucu](#spatial-anchors-account-reader) | Hesabınızdaki uzamsal çapaların özelliklerini bulmanızı ve okumanızı sağlar | 5d51204f-eb77-4b1c-b86a-2ec626c49413 |
> | **Tümleştirme** |  |  |
> | [API Yönetim Hizmeti Katılımcısı](#api-management-service-contributor) | Hizmeti ve API'leri yönetebilir | 312a565d-c81f-4fd8-895a-4e21e48d571c |
> | [API Yönetim Hizmeti Operatörü Rolü](#api-management-service-operator-role) | Hizmeti yönetebilir, ancak API'leri yönetemez | e022efe7-f5ba-4159-bbe4-b44f577e9b61 |
> | [API Yönetimi Hizmet Okuyucu Rolü](#api-management-service-reader-role) | Hizmet e ve API'lere salt okunur erişim | 71522526-b88f-4d52-b57f-d31fc3546d0d |
> | [Uygulama Yapılandırma Veri Sahibi](#app-configuration-data-owner) | Uygulama Yapılandırma verilerine tam erişim sağlar. | 5ae67dd6-50cb-40e7-96ff-dc2bfa4b606b |
> | [Uygulama Yapılandırma Veri Okuyucu](#app-configuration-data-reader) | Uygulama Yapılandırma verilerine okuma erişimi sağlar. | 516239f1-63e1-4d78-a4de-a74fb236a071 |
> | [Azure Hizmet Veri Veri Seçişleri Veri Sahibi](#azure-service-bus-data-owner) | Azure Hizmet Veri Servisi kaynaklarına tam erişim sağlar. | 090c5cfd-751d-490a-894a-3ce6f1109419 |
> | [Azure Hizmet Veri Veri Alıcısı](#azure-service-bus-data-receiver) | Azure Hizmet Veri Servisi kaynaklarına erişim sağlar. | 4f6d3b9b-027b-4f4c-9142-0e5a2247e0 |
> | [Azure Hizmet Veri Veri Gönderen](#azure-service-bus-data-sender) | Azure Hizmet Veri Servisi kaynaklarına erişim göndermeolanağı sağlar. | 69a216fc-b8fb-44d8-bc22-1f3c2cd27a39 |
> | [Azure Yığını Kayıt Sahibi](#azure-stack-registration-owner) | Azure Yığını kayıtlarını yönetmenize olanak tanır. | 6f12a6df-dd06-4f3e-bcb1-ce8be600526a |
> | [EventGrid EventSubscription Katılımcısı](#eventgrid-eventsubscription-contributor) | EventGrid etkinlik abonelik işlemlerini yönetmenize olanak tanır. | 4280ff0-5e57-4d9c-a221-2c70d0e0a443 |
> | [EventGrid EventSubscription Okuyucu](#eventgrid-eventsubscription-reader) | EventGrid etkinlik aboneliklerini okumanızı sağlar. | 2414bbcf-6497-4faf-8c65-045460748405 |
> | [Akıllı Sistemler Hesabı Katılımcısı](#intelligent-systems-account-contributor) | Akıllı Sistemler hesaplarını yönetmenize olanak tanır, ancak bunlara erişmemenize olanak tanır. | 03a6d094-3444-4b3d-88af-7477090a9e5e |
> | [Mantık Uygulaması Katılımcısı](#logic-app-contributor) | Mantıksal uygulamaları yönetmenize olanak tanır, ancak bunlara erişimi değiştirmez. | 87a39d53-fc1b-424a-814c-f7e04687dc9e |
> | [Mantık Uygulama Operatörü](#logic-app-operator) | Mantık uygulamalarını okumanızı, etkinleştirmenizi ve devre dışı bırakamanızı, ancak bunları güncellememenizi veya güncellememenizi sağlar. | 515c2055-d9d4-4321-b1b9-bd0c9a0f79fe |
> | **Kimlik** |  |  |
> | [Yönetilen Kimlik Katkıda Bulunan](#managed-identity-contributor) | Kullanıcı Tarafından Atanan Kimlik Oluşturma, Okuma, Güncelleme ve Silme | e40ec5ca-96e0-45a2-b4ff-59039f2c2b59 |
> | [Yönetilen Kimlik Operatörü](#managed-identity-operator) | Kullanıcı Atanan Kimliği Okuma ve Atama | f1a07417-d97a-45cb-824c-7a7467783830 |
> | **Güvenlik** |  |  |
> | [Azure Sentinel Katılımcısı](#azure-sentinel-contributor) | Azure Sentinel Katılımcısı | ab8e14d6-4a74-4a29-9ba8-549422addade |
> | [Azure Sentinel Okuyucu](#azure-sentinel-reader) | Azure Sentinel Okuyucu | 8d289c81-5878-46d4-8554-54e1e3d8b5cb |
> | [Azure Sentinel Yanıtlayıcı](#azure-sentinel-responder) | Azure Sentinel Yanıtlayıcı | 3e150937-b8fe-4cfb-8069-0eaf05ecd056 |
> | [Anahtar Vault Katılımcı](#key-vault-contributor) | Anahtar kasalarını yönetmenize olanak tanır, ancak bunlara erişmemenize izin verir. | f25e0fa2-a7c8-4377-a976-54943a77a395 |
> | [Güvenlik Yöneticisi](#security-admin) | Güvenlik ilkelerini görüntüleyebilir, güvenlik durumlarını görüntüleyebilir, güvenlik ilkelerini edinebilir, uyarıları ve önerileri görüntüleyebilir, uyarıları ve önerileri reddedebilir. | fb1c8493-542b-48eb-b624-b4c8fea62acd |
> | [Güvenlik Değerlendirmesi Katılımcısı](#security-assessment-contributor) | Değerlendirmeleri Güvenlik Merkezi'ne itmenizi sağlar | 612c2aa1-cb24-443b-ac28-3ab7272de6f5 |
> | [Güvenlik Yöneticisi (Eski)](#security-manager-legacy) | Bu eski bir rol. Bunun yerine Lütfen Güvenlik Yöneticisi'ni kullanın. | e3d13bf0-dd5a-482e-ba6b-9b8433878d10 |
> | [Güvenlik Okuyucu](#security-reader) | Önerileri ve uyarıları görüntüleyebilir, güvenlik ilkelerini görüntüleyebilir, güvenlik durumlarını görüntüleyebilir, ancak değişiklik yapamaz. | 39bc4728-0917-49c7-9d2c-d95423bc2eb4 |
> | **DevOps** |  |  |
> | [Devtest Labs Kullanıcı](#devtest-labs-user) | Azure DevTest Laboratuvarlarınızda sanal makinelerinizi bağlamanızı, başlatmanızı, yeniden başlatmanızı ve kapatmanızı sağlar. | 76283e04-6283-4c54-8f91-bcf1374a3c64 |
> | [Laboratuvar Oluşturucusu](#lab-creator) | Azure Laboratuvar Hesaplarınız altında yönetilen laboratuvarlarınızı oluşturmanıza, yönetmenize ve silmenize olanak tanır. | b97fb8bc-a8b2-4522-a38b-dd33c7e65ead |
> | **İzleyici** |  |  |
> | [Uygulama Öngörüleri Bileşen Katılımcısı](#application-insights-component-contributor) | Uygulama Öngörüleri bileşenlerini yönetebilir | ae349356-3a1b-4a5e-921d-050484c6347e |
> | [Uygulama Öngörüleri Anlık Görüntü Hata Ayıkcısı](#application-insights-snapshot-debugger) | Kullanıcıya Application Insights Snapshot Debugger ile toplanan hata ayıklama anlık görüntülerini görüntüleme ve indirme izni verir. Bu izinlerin [Sahibi](#owner) veya [Katılımcı](#contributor) rollerine dahil olmadığını unutmayın. | 08954f03-6346-4c2e-81c0-ec3a5cfae23b |
> | [Katılımcıyı İzleme](#monitoring-contributor) | Tüm izleme verilerini okuyabilir ve izleme ayarlarını düzeltebilir. Ayrıca [Azure Monitor ile roller, izinler ve güvenlik le başlayın.](https://docs.microsoft.com/azure/azure-monitor/platform/roles-permissions-security#built-in-monitoring-roles) | 749f88d5-cbae-40b8-bcfc-e573ddc772fa |
> | [İzleme Ölçümleri Yayıncı](#monitoring-metrics-publisher) | Azure kaynaklarına karşı yayımlama ölçümleri sağlar | 3913510d-42f4-4e42-8a64-420c390055eb |
> | [İzleme Okuyucu](#monitoring-reader) | Tüm izleme verilerini (ölçümler, günlükler, vb.) okuyabilir. Ayrıca [Azure Monitor ile roller, izinler ve güvenlik le başlayın.](https://docs.microsoft.com/azure/azure-monitor/platform/roles-permissions-security#built-in-monitoring-roles) | 43d0d8ad-25c7-4714-9337-8ba259a9fe05 |
> | [Çalışma Kitabı Katılımcısı](#workbook-contributor) | Paylaşılan çalışma kitaplarını kaydedebilirsiniz. | e8ddcd69-c73f-4f9f-9844-4100522f16ad |
> | [Çalışma Kitabı Okuyucu](#workbook-reader) | Çalışma kitaplarını okuyabilirsin. | b279062a-9be3-42a0-92ae-8b3cf002ec4d |
> | **Yönetim + yönetim** |  |  |
> | [Otomasyon İş Operatörü](#automation-job-operator) | Otomasyon Runbook'larını kullanarak İş Oluşturma ve Yönetme. | 4fe576fe-1146-4730-92eb-48519fa6bf9f |
> | [Otomasyon Operatörü](#automation-operator) | Otomasyon Operatörleri işleri başlatabilir, durdurabilir, askıya alabiliyor ve devam ettirebilir | d3881f73-407a-4167-8283-e981cbba0404 |
> | [Otomasyon Runbook Operatörü](#automation-runbook-operator) | Runbook özelliklerini okuyun - runbook İşleri oluşturabilmek için. | 5fb5aef8-1081-4b8e-bb16-9d5d03855 |
> | [Azure Bağlı Makine Onboarding](#azure-connected-machine-onboarding) | Azure Bağlantılı Makineler'de kullanılabilir. | b64e21ea-ac4e-4cdf-9dc9-5b892992bee7 |
> | [Azure Bağlı Makine Kaynak Yöneticisi](#azure-connected-machine-resource-administrator) | Azure Bağlantılı Makineler'i okuyabilir, yazabilir, silebilir ve yeniden kullanabilir. | cd570a14-e51a-42ad-bac8-bafd67325302 |
> | [Faturalama Okuyucusu](#billing-reader) | Faturalandırma verilerine okuma erişimi sağlar | fa23ad8b-c56e-40d8-ac0c-ce449e1d2c64 |
> | [Plan Katılımcısı](#blueprint-contributor) | Plan tanımlarını yönetebilir, ancak atamaz. | 41077137-e803-4205-871c-5a86e6a753b4 |
> | [Blueprint Operatörü](#blueprint-operator) | Varolan yayımlanmış planları atayabilir, ancak yeni planlar oluşturamaz. Bu yalnızca atama kullanıcı tarafından atanan yönetilen bir kimlikle yapılırsa işe yarar. | 437d2ced-4a38-4302-8479-ed2bcb43d090 |
> | [Maliyet Yönetimi Katılımcısı](#cost-management-contributor) | Maliyetleri görüntüleyebilir ve maliyet yapılandırmalarını yönetebilir (örn. bütçeler, dışa aktarma) | 434105ed-43f6-45c7-a02f-909b2ba83430 |
> | [Maliyet Yönetimi Okuyucu](#cost-management-reader) | Maliyet verilerini ve yapılandırmayı görüntüleyebilir (örn. bütçeler, dışa aktarma) | 72fafb9e-0641-4937-9268-a91bfd8191a3 |
> | [Yönetilen Uygulama Katılımcısıfatı Rolü](#managed-application-contributor-role) | Yönetilen uygulama kaynakları oluşturmanızı sağlar. | 641177b8-a67a-45b9-a033-47bc880bb21e |
> | [Yönetilen Uygulama Operatör Rolü](#managed-application-operator-role) | Yönetilen Uygulama kaynaklarında eylemleri okumanızı ve gerçekleştirmenizi sağlar | c7393b34-138c-406f-901b-d8cf2b17e6ae |
> | [Yönetilen Uygulamalar Okuyucu](#managed-applications-reader) | Yönetilen bir uygulamadaki kaynakları okumanızı ve JIT erişimi istemenizi sağlar. | b9331d3-8a36-4f8c-b097-4f54124fdb44 |
> | [Yönetilen Hizmetler Kayıt atama Silme Rolü](#managed-services-registration-assignment-delete-role) | Yönetilen Hizmetler Kayıt Atama Silme Rolü, yönetici kiracı kullanıcıların kiracılarına atanan kayıt atamasını silmelerine olanak tanır. | 91c1777a-f3dc-4fae-b103-61d183457e46 |
> | [Yönetim Grubu Katılımcısı](#management-group-contributor) | Yönetim Grubu Katılımcı Rolü | 5d58bcaf-24a5-4b20-bdb6-eed9f69fbe4c |
> | [Yönetim Grubu Okuyucu](#management-group-reader) | Yönetim Grubu Okuyucu Rolü | ac63b705-f282-497d-ac71-919bf39d939d |
> | [Yeni Emanet APM Hesap Katılımcısı](#new-relic-apm-account-contributor) | Yeni Emanet Uygulama Performans Yönetimi hesaplarını ve uygulamalarını yönetmenize olanak tanır, ancak bunlara erişmez. | 5d28c62d-5b37-4476-8438-e587778df237 |
> | [İlke Öngörüleri Veri Yazarı (Önizleme)](#policy-insights-data-writer-preview) | Kaynak ilkelerine okuma erişimi ve kaynak bileşeni ilkesi olaylarına yazma erişimi sağlar. | 66bb4e9e-b016-4a94-8249-4c0511c2be84 |
> | [Kaynak İlkesine Katkıda Bulunan](#resource-policy-contributor) | Kaynak ilkesi oluşturma/değiştirme, destek bileti oluşturma ve kaynakları/hiyerarşiyi okuma hakları olan kullanıcılar. | 36243c78-bf99-498c-9df9-86d9f8d28608 |
> | [Site Kurtarma Katılımcısı](#site-recovery-contributor) | Kasa oluşturma ve rol ataması dışında Site Kurtarma hizmetini yönetmenize olanak tanır | 6670b86e-a3f7-4917-ac9b-5d6ab1be4567 |
> | [Site Kurtarma Operatörü](#site-recovery-operator) | Başarısız ve başarısız ama diğer Site Kurtarma yönetimi işlemleri gerçekleştirmek değil sağlar | 494ae006-db33-4328-bf46-533a6560a3ca |
> | [Site Kurtarma Okuyucu](#site-recovery-reader) | Site Kurtarma durumunu görüntülemenize izin verir, ancak diğer yönetim işlemlerini gerçekleştirmezsiniz | dbaa88c4-0c30-4179-9fb3-46319faa6149 |
> | [Destek İstek Katılımcısı](#support-request-contributor) | Destek istekleri oluşturmanıza ve yönetmenize olanak tanır | cfd33db0-3dd1-45e3-aa9d-cdbdf3b6f24e |
> | [Tag Katılımcı](#tag-contributor) | Varlıkların kendilerine erişim sağlamadan varlıklar üzerindeki etiketleri yönetmenize olanak tanır. | 4a9ae827-6dc8-4573-8ac7-8239d42aa03f |
> | **Diğer** |  |  |
> | [BizTalk Katılımcısı](#biztalk-contributor) | BizTalk hizmetlerini yönetmenize olanak tanır, ancak bunlara erişmemenize olanak tanır. | 5e3c6656-6cfa-4708-81fe-0de47ac73342 |
> | [Zamanlayıcı İş Koleksiyonları Katılımcı](#scheduler-job-collections-contributor) | Zamanlayıcı iş koleksiyonlarını yönetmenize olanak tanır, ancak bunlara erişmez. | 188a0f2f-5c9e-469b-ae67-2aa5ce574b94 |


## <a name="general"></a>Genel


### <a name="contributor"></a>Katılımcı

Kaynaklara erişim izni vermek dışında her şeyi yönetmenize olanak tanır.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | * | Her türlü kaynak oluşturma ve yönetme |
> | **NotActions** |  |
> | Microsoft.Authorization/*/Delete | Rolleri, ilke atamalarını, ilke tanımlarını ve ilke kümesi tanımlarını silme |
> | Microsoft.Authorization/*/Yazma | Roller, rol atamaları, ilke atamaları, ilke tanımları ve ilke kümesi tanımları oluşturma |
> | Microsoft.Authorization/yükseltmeAccess/Action | Çağrı yapana kiracı kapsamında Kullanıcı Erişimi Yöneticisi erişim izni verir |
> | Microsoft.Blueprint/blueprintAtamaları/yazma | Plan atamaları oluşturma veya güncelleştirme |
> | Microsoft.Blueprint/blueprintAtamaları/silme | Plan atamalarını silme |
> | **DataActions** |  |
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

Kaynaklara erişim de dahil olmak üzere her şeyi yönetmenize olanak tanır.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | * | Her türlü kaynak oluşturma ve yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
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

Her şeyi görüntülemenizi sağlar, ancak herhangi bir değişiklik yapmaz.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | */okumak | Sırlar hariç her türlü kaynağı okuyun. |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
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

Azure kaynaklarına kullanıcı erişimini yönetmenize olanak tanır.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | */okumak | Sırlar hariç her türlü kaynağı okuyun. |
> | Microsoft.Authorization/* | Yetkilendirmeyi yönetme |
> | Microsoft.Support/* | Destek biletleri oluşturma ve yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
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


### <a name="classic-virtual-machine-contributor"></a>Klasik Sanal Makine Katılımcısı

Klasik sanal makineleri yönetmenize olanak tanır, ancak bunlara erişmemenize ve bağlı oldukları sanal ağ veya depolama hesabını değil.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/oku | Rolleri ve rol ödevlerini okuma |
> | Microsoft.ClassicCompute/alan Adları/* | Klasik işlem alan adları oluşturma ve yönetme |
> | Microsoft.ClassicCompute/virtualMachines/* | Sanal makine oluşturma ve yönetme |
> | Microsoft.ClassicNetwork/networkSecurityGroups/join/action |  |
> | Microsoft.ClassicNetwork/reservedIps/link/action | Ayrılmış bir Ip'yi bağla |
> | Microsoft.ClassicNetwork/reservedIps/read | Ayrılmış Ips alır |
> | Microsoft.ClassicNetwork/virtualNetworks/join/action | Sanal ağa katılır. |
> | Microsoft.ClassicNetwork/virtualNetworks/oku | Sanal ağı alın. |
> | Microsoft.ClassicStorage/storageAccounts/diskler/read | Depolama hesabı diskini döndürür. |
> | Microsoft.ClassicStorage/storageAccounts/images/read | Depolama hesabı görüntüsünü verir. (Amortismana uğradı. 'Microsoft.ClassicStorage/storageAccounts/vmImages' kullanın) |
> | Microsoft.ClassicStorage/storageAccounts/listKeys/action | Depolama hesaplarının erişim anahtarlarını listeler. |
> | Microsoft.ClassicStorage/storageAccounts/read | Depolama hesabını verilen hesapla döndürün. |
> | Microsoft.Insights/alertRules/* | İstatistikler uyarı kuralları oluşturma ve yönetme |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynakların kullanılabilirlik durumlarını alır |
> | Microsoft.Resources/deployments/* | Kaynak grubu dağıtımları oluşturma ve yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Destek biletleri oluşturma ve yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
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

### <a name="virtual-machine-administrator-login"></a>Sanal Makine Yöneticisi Girişi

Sanal Makineleri portalda görüntüleyin ve yönetici olarak giriş yapın

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.Network/publicIPAddresses/read | Genel ip adresi tanımı nı alır. |
> | Microsoft.Network/virtualNetworks/read | Sanal ağ tanımını alma |
> | Microsoft.Network/loadBalancers/oku | Yük dengeleyici tanımını alır |
> | Microsoft.Network/networkInterfaces/read | Ağ arabirimi tanımı nı alır.  |
> | Microsoft.Compute/virtualMachines/*/oku |  |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | Microsoft.Compute/virtualMachines/login/action | Normal bir kullanıcı olarak sanal bir makinede oturum açma |
> | Microsoft.Compute/virtualMachines/loginAsAdmin/action | Windows yöneticisi veya Linux root kullanıcı ayrıcalıkları ile sanal bir makinede oturum açma |
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

Sanal makineleri yönetmenize olanak tanır, ancak bunlara erişmez ve bağlı oldukları sanal ağ veya depolama hesabı nı değil.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/oku | Rolleri ve rol ödevlerini okuma |
> | Microsoft.Compute/availabilitySets/* | İşlem kullanılabilirlik kümeleri oluşturma ve yönetme |
> | Microsoft.Compute/locations/* | İşlem konumları oluşturma ve yönetme |
> | Microsoft.Compute/virtualMachines/* | Sanal makine oluşturma ve yönetme |
> | Microsoft.Compute/virtualMachineScaleSets/* | Sanal makine ölçek kümeleri oluşturma ve yönetme |
> | Microsoft.Compute/disks/write | Yeni bir Disk oluşturur veya varolan bir diski güncelleştirir |
> | Microsoft.Compute/disks/read | Disközelliklerini alma |
> | Microsoft.Compute/diskler/silme | Diski siler |
> | Microsoft.DevTestLab/zamanlamaları/* |  |
> | Microsoft.Insights/alertRules/* | İstatistikler uyarı kuralları oluşturma ve yönetme |
> | Microsoft.Network/applicationGateways/backendAddressPools/join/action | Bir uygulama ağ geçidi arka uç adresi havuzuna katılır. Uyarılabilir değil. |
> | Microsoft.Network/loadBalancers/backendAddressPools/join/action | Yük dengeleyici arka uç adres havuzuna katılır. Uyarılabilir değil. |
> | Microsoft.Network/loadBalancers/inboundNatPools/join/action | Yük dengeleyici gelen NAT havuzuna katılır. Uyarılabilir değil. |
> | Microsoft.Network/loadBalancers/inboundNatRules/join/action | Yük dengeleyici gelen nat kuralına katılır. Uyarılabilir değil. |
> | Microsoft.Network/loadBalancers/probes/join/action | Yük dengeleyicisinin problarının kullanılmasına izin verir. Örneğin, bu izin le vm ölçeği kümesinin bu izin ekiProbe özelliği sondabaşvuru verebilir. Uyarılabilir değil. |
> | Microsoft.Network/loadBalancers/oku | Yük dengeleyici tanımını alır |
> | Microsoft.Network/konumları/* | Ağ konumlarını oluşturma ve yönetme |
> | Microsoft.Network/networkInterfaces/* | Ağ arabirimlerini oluşturma ve yönetme |
> | Microsoft.Network/networkSecurityGroups/join/action | Bir ağ güvenlik grubuna katılır. Uyarılabilir değil. |
> | Microsoft.Network/networkSecurityGroups/oku | Ağ güvenliği grubu tanımını alır |
> | Microsoft.Network/publicIPAddresses/join/action | Herkese açık bir ip adresine katılır. Uyarılabilir değil. |
> | Microsoft.Network/publicIPAddresses/read | Genel ip adresi tanımı nı alır. |
> | Microsoft.Network/virtualNetworks/read | Sanal ağ tanımını alma |
> | Microsoft.Network/virtualNetworks/subnets/join/action | Sanal ağa katılır. Uyarılabilir değil. |
> | Microsoft.RecoveryServices/locations/* |  |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/backupProtectionIntent/write | Yedekleme Koruma Amacıyla Oluşturma |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/*/read |  |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/read | Korumalı Öğenin nesne ayrıntılarını verir |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/write | Yedek Korumalı Öğe Oluşturma |
> | Microsoft.RecoveryServices/Vaults/backupPolicies/read | Tüm Koruma İlkeleri'ni döndürür |
> | Microsoft.RecoveryServices/Vaults/backupPolicies/write | Koruma Politikası Oluşturur |
> | Microsoft.RecoveryServices/Vaults/read | Vault'u Al işlemi, 'vault' türündeki Azure kaynağını temsil eden bir nesne alır |
> | Microsoft.RecoveryServices/Vaults/usages/read | Kurtarma Hizmetleri Kasası için kullanım ayrıntılarını verir. |
> | Microsoft.RecoveryServices/Vaults/write | Vault oluşturma işlemi, 'vault' türünde bir Azure kaynağı oluşturur |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynakların kullanılabilirlik durumlarını alır |
> | Microsoft.Resources/deployments/* | Kaynak grubu dağıtımları oluşturma ve yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.SqlVirtualMachine/* |  |
> | Microsoft.Storage/storageAccounts/listKeys/action | Belirtilen depolama hesabının erişim anahtarlarını döndürür. |
> | Microsoft.Storage/storageAccounts/read | Depolama hesapları listesini verir veya belirtilen depolama hesabının özelliklerini alır. |
> | Microsoft.Support/* | Destek biletleri oluşturma ve yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
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

### <a name="virtual-machine-user-login"></a>Sanal Makine Kullanıcı Girişi

Sanal Makineleri portalda görüntüleyin ve normal bir kullanıcı olarak giriş yapın.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.Network/publicIPAddresses/read | Genel ip adresi tanımı nı alır. |
> | Microsoft.Network/virtualNetworks/read | Sanal ağ tanımını alma |
> | Microsoft.Network/loadBalancers/oku | Yük dengeleyici tanımını alır |
> | Microsoft.Network/networkInterfaces/read | Ağ arabirimi tanımı nı alır.  |
> | Microsoft.Compute/virtualMachines/*/oku |  |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | Microsoft.Compute/virtualMachines/login/action | Normal bir kullanıcı olarak sanal bir makinede oturum açma |
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

## <a name="networking"></a>Ağ Oluşturma


### <a name="cdn-endpoint-contributor"></a>CDN Bitiş Noktası Katılımcısı

CDN uç noktalarını yönetebilir, ancak diğer kullanıcılara erişim izni verilemez.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/oku | Rolleri ve rol ödevlerini okuma |
> | Microsoft.Cdn/edgenodes/read |  |
> | Microsoft.Cdn/operationresults/* |  |
> | Microsoft.Cdn/profiles/endpoints/* |  |
> | Microsoft.Insights/alertRules/* | İstatistikler uyarı kuralları oluşturma ve yönetme |
> | Microsoft.Resources/deployments/* | Kaynak grubu dağıtımları oluşturma ve yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Destek biletleri oluşturma ve yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
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

### <a name="cdn-endpoint-reader"></a>CDN Uç Nokta Okuyucu

CDN uç noktalarını görüntüleyebilir, ancak değişiklik yapamaz.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/oku | Rolleri ve rol ödevlerini okuma |
> | Microsoft.Cdn/edgenodes/read |  |
> | Microsoft.Cdn/operationresults/* |  |
> | Microsoft.Cdn/profiles/endpoints/*/oku |  |
> | Microsoft.Insights/alertRules/* | İstatistikler uyarı kuralları oluşturma ve yönetme |
> | Microsoft.Resources/deployments/* | Kaynak grubu dağıtımları oluşturma ve yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Destek biletleri oluşturma ve yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
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

### <a name="cdn-profile-contributor"></a>CDN Profil Katılımcısı

CDN profillerini ve uç noktalarını yönetebilir, ancak diğer kullanıcılara erişim izni veremez.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/oku | Rolleri ve rol ödevlerini okuma |
> | Microsoft.Cdn/edgenodes/read |  |
> | Microsoft.Cdn/operationresults/* |  |
> | Microsoft.Cdn/profiller/* |  |
> | Microsoft.Insights/alertRules/* | İstatistikler uyarı kuralları oluşturma ve yönetme |
> | Microsoft.Resources/deployments/* | Kaynak grubu dağıtımları oluşturma ve yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Destek biletleri oluşturma ve yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
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

### <a name="cdn-profile-reader"></a>CDN Profil Okuyucu

CDN profillerini ve uç noktalarını görüntüleyebilir, ancak değişiklik yapamaz.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/oku | Rolleri ve rol ödevlerini okuma |
> | Microsoft.Cdn/edgenodes/read |  |
> | Microsoft.Cdn/operationresults/* |  |
> | Microsoft.Cdn/profiles/*/oku |  |
> | Microsoft.Insights/alertRules/* | İstatistikler uyarı kuralları oluşturma ve yönetme |
> | Microsoft.Resources/deployments/* | Kaynak grubu dağıtımları oluşturma ve yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Destek biletleri oluşturma ve yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
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

Klasik ağları yönetmenize olanak tanır, ancak bunlara erişmemenizi sağlar.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/oku | Rolleri ve rol ödevlerini okuma |
> | Microsoft.ClassicNetwork/* | Klasik ağlar oluşturma ve yönetme |
> | Microsoft.Insights/alertRules/* | İstatistikler uyarı kuralları oluşturma ve yönetme |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynakların kullanılabilirlik durumlarını alır |
> | Microsoft.Resources/deployments/* | Kaynak grubu dağıtımları oluşturma ve yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Destek biletleri oluşturma ve yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
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

### <a name="dns-zone-contributor"></a>DNS Bölgesi Katılımcısı

Azure DNS'de DNS bölgelerini ve kayıt kümelerini yönetmenize olanak tanır, ancak bunlara kimlerin erişebilenleri denetlemenize izin vermez.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/oku | Rolleri ve rol ödevlerini okuma |
> | Microsoft.Insights/alertRules/* | İstatistikler uyarı kuralları oluşturma ve yönetme |
> | Microsoft.Network/dnsZones/* | DNS bölgeleri ve kayıtları oluşturma ve yönetme |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynakların kullanılabilirlik durumlarını alır |
> | Microsoft.Resources/deployments/* | Kaynak grubu dağıtımları oluşturma ve yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Destek biletleri oluşturma ve yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
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

Ağları yönetmenize olanak tanır, ancak bunlara erişmemenize olanak tanır.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/oku | Rolleri ve rol ödevlerini okuma |
> | Microsoft.Insights/alertRules/* | İstatistikler uyarı kuralları oluşturma ve yönetme |
> | Microsoft.Network/* | Ağlar oluşturma ve yönetme |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynakların kullanılabilirlik durumlarını alır |
> | Microsoft.Resources/deployments/* | Kaynak grubu dağıtımları oluşturma ve yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Destek biletleri oluşturma ve yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
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

### <a name="traffic-manager-contributor"></a>Trafik Yöneticisi Katılımcı

Trafik Yöneticisi profillerini yönetmenize olanak tanır, ancak bu profillere kimlerin erişebilenleri denetlemenize izin vermez.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/oku | Rolleri ve rol ödevlerini okuma |
> | Microsoft.Insights/alertRules/* | İstatistikler uyarı kuralları oluşturma ve yönetme |
> | Microsoft.Network/trafficManagerProfiles/* |  |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynakların kullanılabilirlik durumlarını alır |
> | Microsoft.Resources/deployments/* | Kaynak grubu dağıtımları oluşturma ve yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Destek biletleri oluşturma ve yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
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

Bir Avere vFXT kümesi oluşturabilir ve yönetebilirsiniz.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/oku | Rolleri ve rol ödevlerini okuma |
> | Microsoft.Compute/*/oku |  |
> | Microsoft.Compute/availabilitySets/* |  |
> | Microsoft.Compute/virtualMachines/* |  |
> | Microsoft.Compute/disks/* |  |
> | Microsoft.Network/*/oku |  |
> | Microsoft.Network/networkInterfaces/* |  |
> | Microsoft.Network/virtualNetworks/read | Sanal ağ tanımını alma |
> | Microsoft.Network/virtualNetworks/subnets/read | Sanal ağ alt ağ tanımı nı alır |
> | Microsoft.Network/virtualNetworks/subnets/join/action | Sanal ağa katılır. Uyarılabilir değil. |
> | Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/action | Depolama hesabı veya SQL veritabanı gibi kaynakları bir alt ağa birleştirir. Uyarılabilir değil. |
> | Microsoft.Network/networkSecurityGroups/join/action | Bir ağ güvenlik grubuna katılır. Uyarılabilir değil. |
> | Microsoft.Resources/deployments/* | Kaynak grubu dağıtımları oluşturma ve yönetme |
> | Microsoft.Insights/alertRules/* | İstatistikler uyarı kuralları oluşturma ve yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Storage/*/oku |  |
> | Microsoft.Storage/storageAccounts/* | Depolama hesapları oluşturma ve yönetme |
> | Microsoft.Support/* | Destek biletleri oluşturma ve yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/resources/read | Kaynak grubu için kaynakları alır. |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/delete | Bir blob silme sonucunu verir |
> | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read | Bir blob veya blobs listesini verir |
> | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/write | Bir blob yazma sonucunu verir |
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

### <a name="avere-operator"></a>Avere Operatörü

Kümeyi yönetmek için Avere vFXT kümesi tarafından kullanılır

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.Compute/virtualMachines/read | Sanal makinenin özelliklerini alın |
> | Microsoft.Network/networkInterfaces/read | Ağ arabirimi tanımı nı alır.  |
> | Microsoft.Network/networkInterfaces/write | Bir ağ arabirimi oluşturur veya varolan bir ağ arabirimini güncelleştirir.  |
> | Microsoft.Network/virtualNetworks/read | Sanal ağ tanımını alma |
> | Microsoft.Network/virtualNetworks/subnets/read | Sanal ağ alt ağ tanımı nı alır |
> | Microsoft.Network/virtualNetworks/subnets/join/action | Sanal ağa katılır. Uyarılabilir değil. |
> | Microsoft.Network/networkSecurityGroups/join/action | Bir ağ güvenlik grubuna katılır. Uyarılabilir değil. |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Storage/storageAccounts/blobServices/containers/delete | Kapsayıcısilme sonucunu verir |
> | Microsoft.Storage/storageAccounts/blobServices/containers/read | Kapsayıcıların listesini döndürür |
> | Microsoft.Storage/storageAccounts/blobServices/containers/write | Blob konteyner koymak sonucu döndürür |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/delete | Bir blob silme sonucunu verir |
> | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read | Bir blob veya blobs listesini verir |
> | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/write | Bir blob yazma sonucunu verir |
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

### <a name="backup-contributor"></a>Yedekleme Katılımcısı

Yedekleme hizmetini yönetmenize olanak tanır, ancak kasa oluşturamaz ve başkalarına erişim veremez

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/oku | Rolleri ve rol ödevlerini okuma |
> | Microsoft.Network/virtualNetworks/read | Sanal ağ tanımını alma |
> | Microsoft.RecoveryServices/locations/* |  |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/operationResults/* | Yedekleme yönetimi yle ilgili operasyon sonuçlarını yönetme |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/* | Kurtarma Hizmetleri kasasının yedek kumaşları içinde yedekleme kapları oluşturma ve yönetme |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/refreshContainers/action | Konteyner listesini yeniler |
> | Microsoft.RecoveryServices/Vaults/backupJobs/* | Yedekleme işleri oluşturma ve yönetme |
> | Microsoft.RecoveryServices/Vaults/backupJobsExport/action | İhracat İşleri |
> | Microsoft.RecoveryServices/Vaults/backupOperationResults/* | Yedekleme yönetimi operasyonlarının sonuçlarını oluşturma ve yönetme |
> | Microsoft.RecoveryServices/Vaults/backupPolicies/* | Yedekleme ilkeleri oluşturma ve yönetme |
> | Microsoft.RecoveryServices/Vaults/backupProtectableItems/* | Yedeklenebilen öğeler oluşturma ve yönetme |
> | Microsoft.RecoveryServices/Vaults/backupProtectedItems/* | Yedeklenmiş öğeler oluşturma ve yönetme |
> | Microsoft.RecoveryServices/Vaults/backupProtectionContainers/* | Yedekleme öğelerini tutan kapsayıcılar oluşturma ve yönetme |
> | Microsoft.RecoveryServices/Vaults/backupSecurityPIN/* |  |
> | Microsoft.RecoveryServices/Vaults/backupKullanımÖzetleri/oku | Kurtarma Hizmetleri için Korumalı Öğeler ve Korumalı Sunucular için özetleri döndürür. |
> | Microsoft.RecoveryServices/Vaults/certificates/* | Kurtarma Hizmetleri kasasında yedeklemeyle ilgili sertifikalar oluşturma ve yönetme |
> | Microsoft.RecoveryServices/Vaults/extendedInformation/* | Kasa yla ilgili genişletilmiş bilgiler oluşturma ve yönetme |
> | Microsoft.RecoveryServices/Vaults/monitoringAlerts/read | Kurtarma hizmetleri kasası için uyarıları alır. |
> | Microsoft.RecoveryServices/Vaults/monitoringConfigurations/* |  |
> | Microsoft.RecoveryServices/Vaults/read | Vault'u Al işlemi, 'vault' türündeki Azure kaynağını temsil eden bir nesne alır |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/* | Kayıtlı kimlikler oluşturma ve yönetme |
> | Microsoft.RecoveryServices/Vaults/kullanımları/* | Kurtarma Hizmetleri kasasının kullanımını oluşturma ve yönetme |
> | Microsoft.Resources/deployments/* | Kaynak grubu dağıtımları oluşturma ve yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Storage/storageAccounts/read | Depolama hesapları listesini verir veya belirtilen depolama hesabının özelliklerini alır. |
> | Microsoft.RecoveryServices/Vaults/backupstorageconfig/* |  |
> | Microsoft.RecoveryServices/Vaults/backupconfig/* |  |
> | Microsoft.RecoveryServices/Vaults/backupValidateİşlem/action | Korumalı Maddede İşlemi Doğrula |
> | Microsoft.RecoveryServices/Vaults/write | Vault oluşturma işlemi, 'vault' türünde bir Azure kaynağı oluşturur |
> | Microsoft.RecoveryServices/Vaults/backupOperations/read | Kurtarma Hizmetleri Vault için Yedekleme İşlem durumu verir. |
> | Microsoft.RecoveryServices/Vaults/backupEngines/read | Vault'a kayıtlı tüm yedekleme yönetim sunucularını döndürür. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/backupProtectionIntent/* |  |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectableContainers/read | Tüm korunabilir kapları alın |
> | Microsoft.RecoveryServices/locations/backupDurum/eylem | Kurtarma Hizmetleri Kasaları için Yedekleme Durumunu Denetle |
> | Microsoft.RecoveryServices/locations/backupPreValidateProtection/action |  |
> | Microsoft.RecoveryServices/locations/backupValidateFeatures/action | Özellikleri Doğrula |
> | Microsoft.RecoveryServices/Vaults/monitoringAlerts/write | Uyarıyı giderir. |
> | Microsoft.RecoveryServices/operations/read | Operasyon, Kaynak Sağlayıcının İşlemler Listesini döndürür |
> | Microsoft.RecoveryServices/locations/operationStatus/read | Belirli bir İşlem için Operasyon Durumu Alır |
> | Microsoft.RecoveryServices/Vaults/backupProtectionIntents/read | Tüm yedekleme Koruma Niyetlerini listele |
> | Microsoft.Support/* | Destek biletleri oluşturma ve yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
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

### <a name="backup-operator"></a>Yedekleme Operatörü

Yedeklemenin kaldırılması, kasa oluşturma ve başkalarına erişim izni dışında yedekleme hizmetlerini yönetmenize olanak tanır

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/oku | Rolleri ve rol ödevlerini okuma |
> | Microsoft.Network/virtualNetworks/read | Sanal ağ tanımını alma |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/operationResults/read | İşlemin durumunu döndürür |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/operationResults/read | Koruma Konteyneri üzerinde gerçekleştirilen operasyonun sonucunu alır. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/backup/action | Korumalı Öğe için Yedekleme gerçekleştirir. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/operationResults/read | Korunan Öğeler üzerinde gerçekleştirilen işlemin sonucunu alır. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/operationsStatus/read | Korumalı Öğeler üzerinde gerçekleştirilen işlemin durumunu verir. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/read | Korumalı Öğenin nesne ayrıntılarını verir |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/provisionInstantItemRecovery/action | Korumalı Madde için Anında Madde Kurtarma Sağlama |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/read | Korumalı Öğeler için Kurtarma Noktaları alın. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/restore/action | Korumalı Öğeler için Kurtarma Noktalarını Geri Yükleyin. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/revokeInstantItemRecovery/action | Korumalı Öğe için Anında Öğe Kurtarmayı İptal |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/write | Yedek Korumalı Öğe Oluşturma |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/read | Tüm kayıtlı kapsayıcıları döndürür |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/refreshContainers/action | Konteyner listesini yeniler |
> | Microsoft.RecoveryServices/Vaults/backupJobs/* | Yedekleme işleri oluşturma ve yönetme |
> | Microsoft.RecoveryServices/Vaults/backupJobsExport/action | İhracat İşleri |
> | Microsoft.RecoveryServices/Vaults/backupOperationResults/* | Yedekleme yönetimi operasyonlarının sonuçlarını oluşturma ve yönetme |
> | Microsoft.RecoveryServices/Vaults/backupPolicies/operationResults/read | İlke İşleminIn Sonuçlarını Alın. |
> | Microsoft.RecoveryServices/Vaults/backupPolicies/read | Tüm Koruma İlkeleri'ni döndürür |
> | Microsoft.RecoveryServices/Vaults/backupProtectableItems/* | Yedeklenebilen öğeler oluşturma ve yönetme |
> | Microsoft.RecoveryServices/Vaults/backupProtectedItems/read | Tüm Korumalı Öğelerin listesini verir. |
> | Microsoft.RecoveryServices/Vaults/backupProtectionContainers/read | Aboneye ait tüm kapsayıcıları döndürür |
> | Microsoft.RecoveryServices/Vaults/backupKullanımÖzetleri/oku | Kurtarma Hizmetleri için Korumalı Öğeler ve Korumalı Sunucular için özetleri döndürür. |
> | Microsoft.RecoveryServices/Vaults/certificates/write | Kaynak Sertifikasını Güncelleştir işlemi kaynak/kasa kimlik bilgilerini güncelleştirir. |
> | Microsoft.RecoveryServices/Vaults/extendedInformation/read | Genişletilmiş Bilgi Al işlemi, bir nesnenin Azure türünün ?vault kaynağını temsil eden Genişletilmiş Bilgilerini alır? |
> | Microsoft.RecoveryServices/Vaults/extendedInformation/write | Genişletilmiş Bilgi Al işlemi, bir nesnenin Azure türünün ?vault kaynağını temsil eden Genişletilmiş Bilgilerini alır? |
> | Microsoft.RecoveryServices/Vaults/monitoringAlerts/read | Kurtarma hizmetleri kasası için uyarıları alır. |
> | Microsoft.RecoveryServices/Vaults/monitoringConfigurations/* |  |
> | Microsoft.RecoveryServices/Vaults/read | Vault'u Al işlemi, 'vault' türündeki Azure kaynağını temsil eden bir nesne alır |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read | İşlem Sonuçları Al işlemi, eşzamanlı olarak gönderilen işlem için işlem durumunu ve sonucunu almak için kullanılabilir |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/read | Get Containers işlemi, bir kaynak için kaydedilmiş kapları almak için kullanılabilir. |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/write | Kayıt Hizmeti Kapsayıcısı işlemi, bir kapsayıcıyı Kurtarma Hizmetine kaydetmek için kullanılabilir. |
> | Microsoft.RecoveryServices/Vaults/usages/read | Kurtarma Hizmetleri Kasası için kullanım ayrıntılarını verir. |
> | Microsoft.Resources/deployments/* | Kaynak grubu dağıtımları oluşturma ve yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Storage/storageAccounts/read | Depolama hesapları listesini verir veya belirtilen depolama hesabının özelliklerini alır. |
> | Microsoft.RecoveryServices/Vaults/backupstorageconfig/* |  |
> | Microsoft.RecoveryServices/Vaults/backupValidateİşlem/action | Korumalı Maddede İşlemi Doğrula |
> | Microsoft.RecoveryServices/Vaults/backupOperations/read | Kurtarma Hizmetleri Vault için Yedekleme İşlem durumu verir. |
> | Microsoft.RecoveryServices/Vaults/backupPolicies/operations/read | İlke İşlemin Durumunu Alın. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/write | Kayıtlı bir kapsayıcı oluşturur |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/inquire/action | Konteyner içindeki iş yükleri için sorgulama yapın |
> | Microsoft.RecoveryServices/Vaults/backupEngines/read | Vault'a kayıtlı tüm yedekleme yönetim sunucularını döndürür. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/backupProtectionIntent/write | Yedekleme Koruma Amacıyla Oluşturma |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/backupProtectionIntent/read | Yedekleme Koruma Amacı Alın |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectableContainers/read | Tüm korunabilir kapları alın |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/items/read | Tüm öğeleri bir kapta alın |
> | Microsoft.RecoveryServices/locations/backupDurum/eylem | Kurtarma Hizmetleri Kasaları için Yedekleme Durumunu Denetle |
> | Microsoft.RecoveryServices/locations/backupPreValidateProtection/action |  |
> | Microsoft.RecoveryServices/locations/backupValidateFeatures/action | Özellikleri Doğrula |
> | Microsoft.RecoveryServices/Vaults/monitoringAlerts/write | Uyarıyı giderir. |
> | Microsoft.RecoveryServices/operations/read | Operasyon, Kaynak Sağlayıcının İşlemler Listesini döndürür |
> | Microsoft.RecoveryServices/locations/operationStatus/read | Belirli bir İşlem için Operasyon Durumu Alır |
> | Microsoft.RecoveryServices/Vaults/backupProtectionIntents/read | Tüm yedekleme Koruma Niyetlerini listele |
> | Microsoft.Support/* | Destek biletleri oluşturma ve yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
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

### <a name="backup-reader"></a>Yedek Okuyucu

Yedekleme hizmetlerini görüntüleyebilir, ancak değişiklik yapamaz

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/oku | Rolleri ve rol ödevlerini okuma |
> | Microsoft.RecoveryServices/locations/allocatedStamp/read | GetAllocatedStamp hizmet tarafından kullanılan dahili işlemdir |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/operationResults/read | İşlemin durumunu döndürür |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/operationResults/read | Koruma Konteyneri üzerinde gerçekleştirilen operasyonun sonucunu alır. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/operationResults/read | Korunan Öğeler üzerinde gerçekleştirilen işlemin sonucunu alır. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/operationsStatus/read | Korumalı Öğeler üzerinde gerçekleştirilen işlemin durumunu verir. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/read | Korumalı Öğenin nesne ayrıntılarını verir |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/recoveryPoints/read | Korumalı Öğeler için Kurtarma Noktaları alın. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/read | Tüm kayıtlı kapsayıcıları döndürür |
> | Microsoft.RecoveryServices/Vaults/backupİşler/operationResults/read | İş İşleminin Sonucunu verir. |
> | Microsoft.RecoveryServices/Vaults/backupİşler/okuma | Tüm İş Nesnelerini döndürür |
> | Microsoft.RecoveryServices/Vaults/backupJobsExport/action | İhracat İşleri |
> | Microsoft.RecoveryServices/Vaults/backupOperationResults/read | Kurtarma Hizmetleri Vault için Yedekleme İşlemi Sonucunu döndürür. |
> | Microsoft.RecoveryServices/Vaults/backupPolicies/operationResults/read | İlke İşleminIn Sonuçlarını Alın. |
> | Microsoft.RecoveryServices/Vaults/backupPolicies/read | Tüm Koruma İlkeleri'ni döndürür |
> | Microsoft.RecoveryServices/Vaults/backupProtectedItems/read | Tüm Korumalı Öğelerin listesini verir. |
> | Microsoft.RecoveryServices/Vaults/backupProtectionContainers/read | Aboneye ait tüm kapsayıcıları döndürür |
> | Microsoft.RecoveryServices/Vaults/backupKullanımÖzetleri/oku | Kurtarma Hizmetleri için Korumalı Öğeler ve Korumalı Sunucular için özetleri döndürür. |
> | Microsoft.RecoveryServices/Vaults/extendedInformation/read | Genişletilmiş Bilgi Al işlemi, bir nesnenin Azure türünün ?vault kaynağını temsil eden Genişletilmiş Bilgilerini alır? |
> | Microsoft.RecoveryServices/Vaults/monitoringAlerts/read | Kurtarma hizmetleri kasası için uyarıları alır. |
> | Microsoft.RecoveryServices/Vaults/read | Vault'u Al işlemi, 'vault' türündeki Azure kaynağını temsil eden bir nesne alır |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read | İşlem Sonuçları Al işlemi, eşzamanlı olarak gönderilen işlem için işlem durumunu ve sonucunu almak için kullanılabilir |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/read | Get Containers işlemi, bir kaynak için kaydedilmiş kapları almak için kullanılabilir. |
> | Microsoft.RecoveryServices/Vaults/backupstorageconfig/read | Kurtarma Hizmetleri Vault için Depolama Yapılandırması döndürür. |
> | Microsoft.RecoveryServices/Vaults/backupconfig/read | Kurtarma Hizmetleri Vault için Yapılandırmayı döndürür. |
> | Microsoft.RecoveryServices/Vaults/backupOperations/read | Kurtarma Hizmetleri Vault için Yedekleme İşlem durumu verir. |
> | Microsoft.RecoveryServices/Vaults/backupPolicies/operations/read | İlke İşlemin Durumunu Alın. |
> | Microsoft.RecoveryServices/Vaults/backupEngines/read | Vault'a kayıtlı tüm yedekleme yönetim sunucularını döndürür. |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/backupProtectionIntent/read | Yedekleme Koruma Amacı Alın |
> | Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/items/read | Tüm öğeleri bir kapta alın |
> | Microsoft.RecoveryServices/locations/backupDurum/eylem | Kurtarma Hizmetleri Kasaları için Yedekleme Durumunu Denetle |
> | Microsoft.RecoveryServices/Vaults/monitoringConfigurations/* |  |
> | Microsoft.RecoveryServices/Vaults/monitoringAlerts/write | Uyarıyı giderir. |
> | Microsoft.RecoveryServices/operations/read | Operasyon, Kaynak Sağlayıcının İşlemler Listesini döndürür |
> | Microsoft.RecoveryServices/locations/operationStatus/read | Belirli bir İşlem için Operasyon Durumu Alır |
> | Microsoft.RecoveryServices/Vaults/backupProtectionIntents/read | Tüm yedekleme Koruma Niyetlerini listele |
> | Microsoft.RecoveryServices/Vaults/usages/read | Kurtarma Hizmetleri Kasası için kullanım ayrıntılarını verir. |
> | Microsoft.RecoveryServices/locations/backupValidateFeatures/action | Özellikleri Doğrula |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
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

### <a name="classic-storage-account-contributor"></a>Klasik Depolama Hesabı Katılımcısı

Klasik depolama hesaplarını yönetmenize olanak tanır, ancak bunlara erişmemenize olanak tanır.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/oku | Rolleri ve rol ödevlerini okuma |
> | Microsoft.ClassicStorage/storageAccounts/* | Depolama hesapları oluşturma ve yönetme |
> | Microsoft.Insights/alertRules/* | İstatistikler uyarı kuralları oluşturma ve yönetme |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynakların kullanılabilirlik durumlarını alır |
> | Microsoft.Resources/deployments/* | Kaynak grubu dağıtımları oluşturma ve yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Destek biletleri oluşturma ve yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
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

### <a name="classic-storage-account-key-operator-service-role"></a>Klasik Depolama Hesabı Anahtar Operatör Hizmeti Rolü

Klasik Depolama Hesabı Anahtar Operatörleri, Klasik Depolama Hesaplarında anahtarları listeleyebilir ve yeniden oluşturabilir

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.ClassicStorage/storageAccounts/listkeys/action | Depolama hesaplarının erişim anahtarlarını listeler. |
> | Microsoft.ClassicStorage/storageAccounts/regeneratekey/action | Depolama hesabının varolan erişim anahtarlarını yeniden oluşturur. |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
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

### <a name="data-box-contributor"></a>Veri Kutusu Katılımcısı

Başkalarına erişim sağlamak dışında Veri Kutusu Hizmeti kapsamındaki her şeyi yönetmenize olanak tanır.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/oku | Rolleri ve rol ödevlerini okuma |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynakların kullanılabilirlik durumlarını alır |
> | Microsoft.Resources/deployments/* | Kaynak grubu dağıtımları oluşturma ve yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Destek biletleri oluşturma ve yönetme |
> | Microsoft.Databox/* |  |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
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

### <a name="data-box-reader"></a>Veri Kutusu Okuyucu

Sipariş oluşturma veya sipariş ayrıntılarını düzenleme ve başkalarına erişim verme dışında Veri Kutusu Hizmetini yönetmenize olanak tanır.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/oku | Rolleri ve rol ödevlerini okuma |
> | Microsoft.Databox/*/oku |  |
> | Microsoft.Databox/jobs/listsecrets/action |  |
> | Microsoft.Databox/jobs/listcredentials/action | Siparişle ilgili şifrelenmemiş kimlik bilgilerini listeler. |
> | Microsoft.Databox/locations/availableSkus/action | Bu yöntem kullanılabilir skus listesini döndürür. |
> | Microsoft.Databox/locations/validateInputs/action | Bu yöntem, tüm doğrulamaları yapar. |
> | Microsoft.Databox/locations/regionConfiguration/action | Bu yöntem, bölge yapılandırmalarını döndürür. |
> | Microsoft.Databox/locations/validateAddress/action | Sevkiyat adresini doğrular ve varsa alternatif adresler sağlar. |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynakların kullanılabilirlik durumlarını alır |
> | Microsoft.Support/* | Destek biletleri oluşturma ve yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
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

### <a name="data-lake-analytics-developer"></a>Veri Gölü Analytics Geliştiricisi

Kendi işlerinizi göndermenize, izlemenize ve yönetmenize olanak tanır, ancak Data Lake Analytics hesapları oluşturmamanıza veya silmemenizi sağlar.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/oku | Rolleri ve rol ödevlerini okuma |
> | Microsoft.BigAnalytics/hesapları/* |  |
> | Microsoft.DataLakeAnalytics/hesapları/* |  |
> | Microsoft.Insights/alertRules/* | İstatistikler uyarı kuralları oluşturma ve yönetme |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynakların kullanılabilirlik durumlarını alır |
> | Microsoft.Resources/deployments/* | Kaynak grubu dağıtımları oluşturma ve yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Destek biletleri oluşturma ve yönetme |
> | **NotActions** |  |
> | Microsoft.BigAnalytics/accounts/Delete |  |
> | Microsoft.BigAnalytics/accounts/TakeOwnership/action |  |
> | Microsoft.BigAnalytics/accounts/Write |  |
> | Microsoft.DataLakeAnalytics/accounts/Delete | DataLakeAnalytics hesabını silin. |
> | Microsoft.DataLakeAnalytics/accounts/TakeOwnership/action | Diğer kullanıcılar tarafından gönderilen işleri iptal etmek için izin ler tanıyın. |
> | Microsoft.DataLakeAnalytics/accounts/Write | DataLakeAnalytics hesabı oluşturun veya güncelleyin. |
> | Microsoft.DataLakeAnalytics/accounts/dataLakeStoreAccounts/Write | Bir DataLakeAnalytics hesabının bağlantılı Bir DataLakeStore hesabı oluşturun veya güncelleyin. |
> | Microsoft.DataLakeAnalytics/accounts/dataLakeStoreAccounts/Delete | DataLakeAnalytics hesabından bir DataLakeStore hesabının bağlantısını açın. |
> | Microsoft.DataLakeAnalytics/accounts/storageAccounts/Write | DataLakeAnalytics hesabının bağlantılı bir Depolama hesabı oluşturun veya güncelleyin. |
> | Microsoft.DataLakeAnalytics/accounts/storageAccounts/Delete | Bir DataLakeAnalytics hesabından bir Depolama hesabının bağlantısını açın. |
> | Microsoft.DataLakeAnalytics/accounts/firewallRules/Write | Güvenlik duvarı kuralı oluşturun veya güncelleştirin. |
> | Microsoft.DataLakeAnalytics/accounts/firewallRules/Delete | Güvenlik duvarı kuralını silin. |
> | Microsoft.DataLakeAnalytics/accounts/computePolicies/Write | Bir bilgi işlem ilkesi oluşturun veya güncelleştirin. |
> | Microsoft.DataLakeAnalytics/accounts/computePolicies/Delete | İşlem ilkesini silin. |
> | **DataActions** |  |
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

### <a name="reader-and-data-access"></a>Okuyucu ve Veri Erişimi

Her şeyi görüntülemenize olanak tanır, ancak bir depolama hesabı veya içerdiği kaynağı silmenize veya oluşturmanıza izin vermez. Ayrıca, depolama hesabı anahtarlarına erişim yoluyla bir depolama hesabında bulunan tüm verilere okuma/yazma erişimi sağlar.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.Storage/storageAccounts/listKeys/action | Belirtilen depolama hesabının erişim anahtarlarını döndürür. |
> | Microsoft.Storage/storageAccounts/ListAccountSas/action | Belirtilen depolama hesabı için Hesap SAS belirteci verir. |
> | Microsoft.Storage/storageAccounts/read | Depolama hesapları listesini verir veya belirtilen depolama hesabının özelliklerini alır. |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
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

Depolama hesaplarının yönetimine izin verir. Paylaşılan Anahtar yetkilendirmesi yoluyla verilere erişmek için kullanılabilen hesap anahtarına erişim sağlar.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/oku | Rolleri ve rol ödevlerini okuma |
> | Microsoft.Insights/alertRules/* | İstatistikler uyarı kuralları oluşturma ve yönetme |
> | Microsoft.Insights/diagnosticSettings/* | Analysis Server için tanılama ayarını oluşturur, günceller veya okur |
> | Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/action | Depolama hesabı veya SQL veritabanı gibi kaynakları bir alt ağa birleştirir. Uyarılabilir değil. |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynakların kullanılabilirlik durumlarını alır |
> | Microsoft.Resources/deployments/* | Kaynak grubu dağıtımları oluşturma ve yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Storage/storageAccounts/* | Depolama hesapları oluşturma ve yönetme |
> | Microsoft.Support/* | Destek biletleri oluşturma ve yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
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

### <a name="storage-account-key-operator-service-role"></a>Depolama Hesabı Anahtar Operatör Hizmeti Rolü

Depolama hesabı erişim anahtarlarını listeleme ve yenilemeye izin verir.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.Storage/storageAccounts/listkeys/action | Belirtilen depolama hesabının erişim anahtarlarını döndürür. |
> | Microsoft.Storage/storageAccounts/regeneratekey/action | Belirtilen depolama hesabının erişim anahtarlarını yeniler. |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
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

### <a name="storage-blob-data-contributor"></a>Depolama Blob Veri Katılımcısı

Azure Depolama kapsayıcılarını ve lekelerini okuyun, yazın ve silin. Belirli bir veri işlemi için hangi eylemlerin gerekli olduğunu öğrenmek [için, blob ve sıra veri işlemlerini çağırmak için İzinler'e](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations)bakın.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.Storage/storageAccounts/blobServices/containers/delete | Bir kapsayıcıyı silin. |
> | Microsoft.Storage/storageAccounts/blobServices/containers/read | Bir kapsayıcı veya kapsayıcıların listesini döndürün. |
> | Microsoft.Storage/storageAccounts/blobServices/containers/write | Kapsayıcının meta verilerini veya özelliklerini değiştirin. |
> | Microsoft.Storage/storageAccounts/blobServices/generateUserDelegationKey/action | Blob hizmeti için bir kullanıcı delegasyonu anahtarı verir. |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/delete | Bir lekeyi silin. |
> | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read | Bir blob veya lekeler in listesini döndürün. |
> | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/write | Bir blob yazın. |
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

### <a name="storage-blob-data-owner"></a>Depolama Blob Veri Sahibi

POSIX erişim denetimi atama da dahil olmak üzere Azure Depolama blob kapsayıcılarına ve verilerine tam erişim sağlar. Belirli bir veri işlemi için hangi eylemlerin gerekli olduğunu öğrenmek [için, blob ve sıra veri işlemlerini çağırmak için İzinler'e](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations)bakın.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.Storage/storageAccounts/blobServices/containers/* | Konteynerlerde tam izinler. |
> | Microsoft.Storage/storageAccounts/blobServices/generateUserDelegationKey/action | Blob hizmeti için bir kullanıcı delegasyonu anahtarı verir. |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/* | Lekeler üzerinde tam izinler. |
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

### <a name="storage-blob-data-reader"></a>Depolama Blob Veri Okuyucu

Azure Depolama kapsayıcılarını ve lekelerini okuyun ve listelayın. Belirli bir veri işlemi için hangi eylemlerin gerekli olduğunu öğrenmek [için, blob ve sıra veri işlemlerini çağırmak için İzinler'e](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations)bakın.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.Storage/storageAccounts/blobServices/containers/read | Bir kapsayıcı veya kapsayıcıların listesini döndürün. |
> | Microsoft.Storage/storageAccounts/blobServices/generateUserDelegationKey/action | Blob hizmeti için bir kullanıcı delegasyonu anahtarı verir. |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read | Bir blob veya lekeler in listesini döndürün. |
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

### <a name="storage-blob-delegator"></a>Depolama Blob Delegator

Azure AD kimlik bilgileriyle imzalanmış bir kapsayıcı veya blob için paylaşılan erişim imzası oluşturmak için kullanılabilecek bir kullanıcı delegasyonu anahtarı alın. Daha fazla bilgi için [bkz.](https://docs.microsoft.com/rest/api/storageservices/create-user-delegation-sas)

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.Storage/storageAccounts/blobServices/generateUserDelegationKey/action | Blob hizmeti için bir kullanıcı delegasyonu anahtarı verir. |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
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

### <a name="storage-file-data-smb-share-contributor"></a>Depolama Dosyası Verileri Kobİ Payı Katılımcısı

Azure dosya paylaşımlarında dosyalara/dizinlerde erişimin okunmasına, yazılmasına ve silinmesine izin verir. Bu rolün Windows dosya sunucularında yerleşik eşdeğeri yoktur.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | *yok* |  |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | Microsoft.Storage/storageAccounts/fileServices/fileshares/files/read | Bir dosya/klasör veya dosya/klasör listesini verir. |
> | Microsoft.Storage/storageAccounts/fileServices/fileshares/files/write | Dosya yazma veya klasör oluşturma sonucunu döndürür. |
> | Microsoft.Storage/storageAccounts/fileServices/fileshares/files/delete | Dosya/klasör silme sonucunu verir. |
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

### <a name="storage-file-data-smb-share-elevated-contributor"></a>Depolama Dosya Verileri Kobİ Payı Yüksek Katılımcı

Azure dosya paylaşımlarında dosya/dizinler üzerinde ABM'lerin okunmasına, yazılmasına, silinmesine ve değiştirilmesine olanak tanır. Bu rol, Windows dosya sunucularında değişiklik bir dosya payı ACL eşdeğerdir.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | *yok* |  |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | Microsoft.Storage/storageAccounts/fileServices/fileshares/files/read | Bir dosya/klasör veya dosya/klasör listesini verir. |
> | Microsoft.Storage/storageAccounts/fileServices/fileshares/files/write | Dosya yazma veya klasör oluşturma sonucunu döndürür. |
> | Microsoft.Storage/storageAccounts/fileServices/fileshares/files/delete | Dosya/klasör silme sonucunu verir. |
> | Microsoft.Storage/storageAccounts/fileServices/fileshares/files/modifypermissions/action | Bir dosya/klasörde izin değiştirme sonucunu döndürür. |
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

### <a name="storage-file-data-smb-share-reader"></a>Depolama Dosyası Veri Kobİ Pay Okuyucu

Azure dosya paylaşımlarında dosyalara/dizinlere erişim inokunmasını sağlar. Bu rol, Windows dosya sunucularında okunan bir dosya paylaşımı ACL'sine eşdeğerdir.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | *yok* |  |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | Microsoft.Storage/storageAccounts/fileServices/fileshares/files/read | Bir dosya/klasör veya dosya/klasör listesini verir. |
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

### <a name="storage-queue-data-contributor"></a>Depolama Sırası Veri Katılımcısı

Azure Depolama kuyruklarını ve sıra iletilerini okuyun, yazın ve silin. Belirli bir veri işlemi için hangi eylemlerin gerekli olduğunu öğrenmek [için, blob ve sıra veri işlemlerini çağırmak için İzinler'e](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations)bakın.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.Storage/storageAccounts/queueServices/queues/delete | Bir sırayı silin. |
> | Microsoft.Storage/storageAccounts/queueServices/queues/read | Bir sıra veya sıra listesi döndürün. |
> | Microsoft.Storage/storageAccounts/queueServices/queues/write | Sıra meta verilerini veya özelliklerini değiştirin. |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | Microsoft.Storage/storageAccounts/queueServices/queues/messages/delete | Bir kuyruktan bir veya daha fazla ileti silin. |
> | Microsoft.Storage/storageAccounts/queueServices/queues/messages/read | Bir kuyruktan bir veya daha fazla iletiye göz atın veya alın. |
> | Microsoft.Storage/storageAccounts/queueServices/queues/messages/write | Kuyruğa ileti ekleyin. |
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

### <a name="storage-queue-data-message-processor"></a>Depolama Sırası Veri İletisi İşlemcisi

Azure Depolama kuyruğundan bir iletiyi gözetleme, alma ve silme. Belirli bir veri işlemi için hangi eylemlerin gerekli olduğunu öğrenmek [için, blob ve sıra veri işlemlerini çağırmak için İzinler'e](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations)bakın.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | *yok* |  |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | Microsoft.Storage/storageAccounts/queueServices/queues/messages/read | Bir iletiyi gözetle. |
> | Microsoft.Storage/storageAccounts/queueServices/queues/messages/process/action | İletiyi alın ve silin. |
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

### <a name="storage-queue-data-message-sender"></a>Depolama Sırası Veri İletisi Gönderen

Azure Depolama kuyruğuna ileti ekleyin. Belirli bir veri işlemi için hangi eylemlerin gerekli olduğunu öğrenmek [için, blob ve sıra veri işlemlerini çağırmak için İzinler'e](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations)bakın.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | *yok* |  |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | Microsoft.Storage/storageAccounts/queueServices/queues/messages/add/action | Kuyruğa ileti ekleyin. |
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

### <a name="storage-queue-data-reader"></a>Depolama Sırası Veri Okuyucu

Azure Depolama kuyruklarını ve sıra iletilerini okuyun ve listelenin. Belirli bir veri işlemi için hangi eylemlerin gerekli olduğunu öğrenmek [için, blob ve sıra veri işlemlerini çağırmak için İzinler'e](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-blob-and-queue-data-operations)bakın.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.Storage/storageAccounts/queueServices/queues/read | Bir sıra yı veya kuyruk listesini döndürür. |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | Microsoft.Storage/storageAccounts/queueServices/queues/messages/read | Bir kuyruktan bir veya daha fazla iletiye göz atın veya alın. |
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


### <a name="azure-maps-data-reader-preview"></a>Azure Haritalar Veri Okuyucu (Önizleme)

Azure haritaları hesabından haritayla ilgili verilere erişim izni verir.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | *yok* |  |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | Microsoft.Maps/accounts/data/read | Haritalar hesabına veri okuma erişimi verir. |
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
        "Microsoft.Maps/accounts/data/read"
      ],
      "notDataActions": []
    }
  ],
  "roleName": "Azure Maps Data Reader (Preview)",
  "roleType": "BuiltInRole",
  "type": "Microsoft.Authorization/roleDefinitions"
}
```

### <a name="search-service-contributor"></a>Arama Hizmeti Katılımcısı

Arama hizmetlerini yönetmenize olanak tanır, ancak bunlara erişmez.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/oku | Rolleri ve rol ödevlerini okuma |
> | Microsoft.Insights/alertRules/* | İstatistikler uyarı kuralları oluşturma ve yönetme |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynakların kullanılabilirlik durumlarını alır |
> | Microsoft.Resources/deployments/* | Kaynak grubu dağıtımları oluşturma ve yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Search/searchServices/* | Arama hizmetleri oluşturma ve yönetme |
> | Microsoft.Support/* | Destek biletleri oluşturma ve yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
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

### <a name="web-plan-contributor"></a>Web Planı Katılımcısı

Web sitelerinin web planlarını yönetmenize olanak tanır, ancak bunlara erişmemenize olanak tanır.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/oku | Rolleri ve rol ödevlerini okuma |
> | Microsoft.Insights/alertRules/* | İstatistikler uyarı kuralları oluşturma ve yönetme |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynakların kullanılabilirlik durumlarını alır |
> | Microsoft.Resources/deployments/* | Kaynak grubu dağıtımları oluşturma ve yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Destek biletleri oluşturma ve yönetme |
> | Microsoft.Web/serverFarms/* | Sunucu çiftliklerini oluşturma ve yönetme |
> | Microsoft.Web/hostingOrtamları/Birleştirme/Eylem | Uygulama Hizmet Ortamına Katılır |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
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

### <a name="website-contributor"></a>Web Sitesi Katılımcısı

Web sitelerini (web planlarını değil) yönetmenize olanak tanır, ancak bunlara erişmemenize olanak tanır.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/oku | Rolleri ve rol ödevlerini okuma |
> | Microsoft.Insights/alertRules/* | İstatistikler uyarı kuralları oluşturma ve yönetme |
> | Microsoft.Insights/components/* | Öngörüler bileşenleri oluşturma ve yönetme |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynakların kullanılabilirlik durumlarını alır |
> | Microsoft.Resources/deployments/* | Kaynak grubu dağıtımları oluşturma ve yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Destek biletleri oluşturma ve yönetme |
> | Microsoft.Web/sertifikalar/* | Web sitesi sertifikaları oluşturma ve yönetme |
> | Microsoft.Web/listSitesAssignedToHostName/oku | Barındırılabilmek için atanan sitelerin adlarını alın. |
> | Microsoft.Web/serverFarms/join/action |  |
> | Microsoft.Web/serverFarms/okumak | Özellikleri Uygulama Hizmet Planına alın |
> | Microsoft.Web/siteler/* | Web siteleri oluşturma ve yönetme (site oluşturma, ilişkili Uygulama Hizmeti Planı'na yazma izinleri de gerektirir) |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
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

acr silme

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.ContainerRegistry/registries/artifacts/delete | Kapsayıcı kayıt defterindeki artifakı silin. |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
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

### <a name="acrimagesigner"></a>AcrImageSigner

acr görüntü imzalayan

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.ContainerRegistry/registries/sign/write | Bir kapsayıcı kayıt defteri için itme/çekme içeriği meta verilere güvenin. |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
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

acr çekme

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.ContainerRegistry/registries/pull/read | Bir konteyner kayıt defterinden görüntüleri çekin veya alın. |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
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

acr itme

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.ContainerRegistry/registries/pull/read | Bir konteyner kayıt defterinden görüntüleri çekin veya alın. |
> | Microsoft.ContainerRegistry/registries/push/write | Görüntüleri bir kapsayıcı kayıt defterine itin veya yazın. |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
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

### <a name="acrquarantinereader"></a>AcrQuarantineReader

acr karantina veri okuyucu

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.ContainerRegistry/registries/karantina/okuma | Konteyner kayıt defterinden karantinaya alınmış görüntüleri çekme veya alma |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
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

acr karantina veri yazar

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.ContainerRegistry/registries/karantina/okuma | Konteyner kayıt defterinden karantinaya alınmış görüntüleri çekme veya alma |
> | Microsoft.ContainerRegistry/registries/karantina/yazma | Karantina durumunu karantinaya alma/değiştirme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
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

### <a name="azure-kubernetes-service-cluster-admin-role"></a>Azure Kubernetes Hizmet Kümesi Yönetici Rolü

Liste küme yöneticisi kimlik bilgisi eylemi.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.ContainerService/managedClusters/listClusterAdminCredential/action | Yönetilen bir kümenin clusterAdmin kimlik bilgileri listele |
> | Microsoft.ContainerService/managedClusters/accessProfiles/listCredential/action | Liste kimlik bilgilerini kullanarak rol adına göre yönetilen küme erişim profili alma |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
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

### <a name="azure-kubernetes-service-cluster-user-role"></a>Azure Kubernetes Hizmet Kümesi Kullanıcı Rolü

Liste küme kullanıcı kimlik bilgileri eylemi.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.ContainerService/managedClusters/listClusterUserCredential/action | Yönetilen bir kümenin clusterUser kimlik bilgilerini listele |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
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


### <a name="cosmos-db-account-reader-role"></a>Cosmos DB Hesap Okuyucu Rolü

Azure Cosmos DB hesap verilerini okuyabilir. Azure Cosmos DB hesaplarını yönetmek için [DocumentDB Hesap](#documentdb-account-contributor) Katılımcısı'na bakın.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/oku | Rolleri ve rol ödevlerini okuma |
> | Microsoft.DocumentDB/*/okuma | Herhangi bir koleksiyonu okuyun |
> | Microsoft.DocumentDB/databaseAccounts/readonlykeys/action | Veritabanı hesabını yalnızca anahtarları okur. |
> | Microsoft.Insights/MetricDefinitions/read | Metrik tanımları okuma |
> | Microsoft.Insights/Metrics/read | Ölçümleri okuyun |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Destek biletleri oluşturma ve yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
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

### <a name="cosmos-db-operator"></a>Cosmos DB Operatörü

Azure Cosmos DB hesaplarını yönetmenize olanak tanır, ancak bu hesaplardaki verilere erişmez. Hesap anahtarlarına ve bağlantı dizelerine erişimi engeller.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.DocumentDb/databaseAccounts/* |  |
> | Microsoft.Insights/alertRules/* | İstatistikler uyarı kuralları oluşturma ve yönetme |
> | Microsoft.Authorization/*/oku | Rolleri ve rol ödevlerini okuma |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynakların kullanılabilirlik durumlarını alır |
> | Microsoft.Resources/deployments/* | Kaynak grubu dağıtımları oluşturma ve yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Destek biletleri oluşturma ve yönetme |
> | Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/action | Depolama hesabı veya SQL veritabanı gibi kaynakları bir alt ağa birleştirir. Uyarılabilir değil. |
> | **NotActions** |  |
> | Microsoft.DocumentDB/databaseAccounts/readonlyKeys/* |  |
> | Microsoft.DocumentDB/databaseAccounts/regenerateKey/* |  |
> | Microsoft.DocumentDB/databaseAccounts/listKeys/* |  |
> | Microsoft.DocumentDB/databaseAccounts/listConnectionStrings/* |  |
> | **DataActions** |  |
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

Cosmos DB veritabanı veya bir hesap için bir kapsayıcı için geri yükleme isteği gönderebilir

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.DocumentDB/databaseAccounts/backup/action | Yedeklemeyi yapılandırmak için istek gönderme |
> | Microsoft.DocumentDB/databaseAccounts/geri yükleme/eylem | Geri yükleme isteği gönderme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
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

### <a name="documentdb-account-contributor"></a>DocumentDB Hesap Katılımcısı

Azure Cosmos DB hesaplarını yönetebilirsiniz. Azure Cosmos DB eskiden DocumentDB olarak bilinir.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/oku | Rolleri ve rol ödevlerini okuma |
> | Microsoft.DocumentDb/databaseAccounts/* | Azure Cosmos DB hesapları oluşturma ve yönetme |
> | Microsoft.Insights/alertRules/* | İstatistikler uyarı kuralları oluşturma ve yönetme |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynakların kullanılabilirlik durumlarını alır |
> | Microsoft.Resources/deployments/* | Kaynak grubu dağıtımları oluşturma ve yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Destek biletleri oluşturma ve yönetme |
> | Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/action | Depolama hesabı veya SQL veritabanı gibi kaynakları bir alt ağa birleştirir. Uyarılabilir değil. |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
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

### <a name="redis-cache-contributor"></a>Redis Önbellek Katkıda Bulunan

Redis önbelleklerini yönetmenize izin verir, ancak bunlara erişmemenize izin verir.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/oku | Rolleri ve rol ödevlerini okuma |
> | Microsoft.Önbellek/redis/* | Redis önbellekleri oluşturma ve yönetme |
> | Microsoft.Insights/alertRules/* | İstatistikler uyarı kuralları oluşturma ve yönetme |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynakların kullanılabilirlik durumlarını alır |
> | Microsoft.Resources/deployments/* | Kaynak grubu dağıtımları oluşturma ve yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Destek biletleri oluşturma ve yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
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

SQL veritabanlarını yönetmenize olanak tanır, ancak bunlara erişmez. Ayrıca, güvenlikle ilgili ilkelerini veya üst SQL sunucularını yönetemezsiniz.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/oku | Rolleri ve rol ödevlerini okuma |
> | Microsoft.Insights/alertRules/* | İstatistikler uyarı kuralları oluşturma ve yönetme |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynakların kullanılabilirlik durumlarını alır |
> | Microsoft.Resources/deployments/* | Kaynak grubu dağıtımları oluşturma ve yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Sql/locations/*/oku |  |
> | Microsoft.Sql/servers/veritabanları/* | SQL veritabanı oluşturma ve yönetme |
> | Microsoft.Sql/servers/read | Sunucu listesini döndürün veya belirtilen sunucunun özelliklerini alır. |
> | Microsoft.Support/* | Destek biletleri oluşturma ve yönetme |
> | Microsoft.Insights/metrics/read | Ölçümleri okuyun |
> | Microsoft.Insights/metricDefinitions/read | Metrik tanımları okuma |
> | **NotActions** |  |
> | Microsoft.Sql/managedInstances/databases/currentSensitivityLabels/* |  |
> | Microsoft.Sql/managedInstances/databases/recommendedSensitivityLabels/* |  |
> | Microsoft.Sql/managedInstances/databases/schemas/tables/columns/sensitivityLabels/* |  |
> | Microsoft.Sql/managedInstances/veritabanları/securityAlertPolicies/* |  |
> | Microsoft.Sql/managedInstances/veritabanları/sensitivityLabels/* |  |
> | Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/* |  |
> | Microsoft.Sql/managedInstances/securityAlertPolicies/* |  |
> | Microsoft.Sql/managedInstances/vulnerabilityAssessments/* |  |
> | Microsoft.Sql/servers/databases/auditingPolicies/* | Denetim ilkelerini edin |
> | Microsoft.Sql/servers/databases/auditingSettings/* | Denetim ayarlarını edin |
> | Microsoft.Sql/servers/databases/auditRecords/read | Veritabanı blob denetim kayıtlarını alma |
> | Microsoft.Sql/servers/databases/connectionPolicies/* | Bağlantı ilkelerini edin |
> | Microsoft.Sql/servers/databases/currentSensitivityLabels/* |  |
> | Microsoft.Sql/servers/veritabanları/dataMaskingPolicies/* | Veri maskeleme ilkelerini edin |
> | Microsoft.Sql/servers/databases/extendedAuditingSettings/* |  |
> | Microsoft.Sql/servers/veritabanları/recommendedSensitivityLabels/* |  |
> | Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/* |  |
> | Microsoft.Sql/servers/veritabanları/securityAlertPolicies/* | Güvenlik uyarı ilkelerini edin |
> | Microsoft.Sql/servers/veritabanları/securityMetrics/* | Güvenlik ölçümlerini edin |
> | Microsoft.Sql/servers/veritabanları/sensitivityLabels/* |  |
> | Microsoft.Sql/servers/veritabanları/güvenlik açığıAssessments/* |  |
> | Microsoft.Sql/servers/veritabanları/güvenlik açığıAssessmentScans/* |  |
> | Microsoft.Sql/servers/veritabanları/güvenlik açığıAssessmentSettings/* |  |
> | Microsoft.Sql/servers/vulnerabilityAssessments/* |  |
> | **DataActions** |  |
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

### <a name="sql-managed-instance-contributor"></a>SQL Yönetilen Örnek Katılımcı

SQL Yönetilen Örnekleri ve gerekli ağ yapılandırmasını yönetmenize olanak tanır, ancak başkalarına erişim veremez.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynakların kullanılabilirlik durumlarını alır |
> | Microsoft.Resources/deployments/* | Kaynak grubu dağıtımları oluşturma ve yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Network/networkSecurityGroups/* |  |
> | Microsoft.Network/routeTables/* |  |
> | Microsoft.Sql/locations/*/oku |  |
> | Microsoft.Sql/managedInstances/* |  |
> | Microsoft.Support/* | Destek biletleri oluşturma ve yönetme |
> | Microsoft.Network/virtualNetworks/subnets/* |  |
> | Microsoft.Network/virtualNetworks/* |  |
> | Microsoft.Authorization/*/oku | Rolleri ve rol ödevlerini okuma |
> | Microsoft.Insights/alertRules/* | İstatistikler uyarı kuralları oluşturma ve yönetme |
> | Microsoft.Insights/metrics/read | Ölçümleri okuyun |
> | Microsoft.Insights/metricDefinitions/read | Metrik tanımları okuma |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
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

SQL sunucularının ve veritabanlarının güvenlikle ilgili ilkelerini yönetmenize olanak tanır, ancak bunlara erişmez.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/oku | Rolleri ve rol ödevlerini okuma |
> | Microsoft.Insights/alertRules/* | İstatistikler uyarı kuralları oluşturma ve yönetme |
> | Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/action | Depolama hesabı veya SQL veritabanı gibi kaynakları bir alt ağa birleştirir. Uyarılabilir değil. |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynakların kullanılabilirlik durumlarını alır |
> | Microsoft.Resources/deployments/* | Kaynak grubu dağıtımları oluşturma ve yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Sql/managedInstances/databases/currentSensitivityLabels/* |  |
> | Microsoft.Sql/managedInstances/databases/recommendedSensitivityLabels/* |  |
> | Microsoft.Sql/managedInstances/databases/schemas/tables/columns/sensitivityLabels/* |  |
> | Microsoft.Sql/managedInstances/veritabanları/securityAlertPolicies/* |  |
> | Microsoft.Sql/managedInstances/veritabanları/sensitivityLabels/* |  |
> | Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/* |  |
> | Microsoft.Sql/managedInstances/securityAlertPolicies/* |  |
> | Microsoft.Sql/managedInstances/veritabanları/transparentDataEncryption/* |  |
> | Microsoft.Sql/managedInstances/vulnerabilityAssessments/* |  |
> | Microsoft.Sql/servers/auditingPolicies/* | SQL sunucu denetim ilkeleri oluşturma ve yönetme |
> | Microsoft.Sql/servers/auditingSettings/* | SQL sunucu denetim ayarı oluşturma ve yönetme |
> | Microsoft.Sql/servers/extendedAuditingSettings/read | Belirli bir sunucuda yapılandırılan genişletilmiş sunucu blob denetim ilkesinin ayrıntılarını alma |
> | Microsoft.Sql/servers/databases/auditingPolicies/* | SQL sunucu veritabanı denetim ilkeleri oluşturma ve yönetme |
> | Microsoft.Sql/servers/databases/auditingSettings/* | SQL sunucu veritabanı denetim ayarlarını oluşturma ve yönetme |
> | Microsoft.Sql/servers/databases/auditRecords/read | Veritabanı blob denetim kayıtlarını alma |
> | Microsoft.Sql/servers/databases/connectionPolicies/* | SQL server veritabanı bağlantı ilkeleri oluşturma ve yönetme |
> | Microsoft.Sql/servers/databases/currentSensitivityLabels/* |  |
> | Microsoft.Sql/servers/veritabanları/dataMaskingPolicies/* | SQL server veritabanı veri maskeleme ilkeleri oluşturma ve yönetme |
> | Microsoft.Sql/servers/databases/extendedAuditingSettings/read | Belirli bir veritabanında yapılandırılan genişletilmiş blob denetim ilkesinin ayrıntılarını alma |
> | Microsoft.Sql/servers/databases/read | Veritabanları listesini döndürün veya belirtilen veritabanının özelliklerini alır. |
> | Microsoft.Sql/servers/veritabanları/recommendedSensitivityLabels/* |  |
> | Microsoft.Sql/servers/databases/schemas/read | Bir veritabanı şeması al. |
> | Microsoft.Sql/servers/databases/schemas/tables/columns/read | Bir veritabanı sütunu alın. |
> | Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/* |  |
> | Microsoft.Sql/servers/databases/schemas/tables/read | Bir veritabanı tablosu al. |
> | Microsoft.Sql/servers/veritabanları/securityAlertPolicies/* | SQL server veritabanı güvenlik uyarı ilkeleri oluşturma ve yönetme |
> | Microsoft.Sql/servers/veritabanları/securityMetrics/* | SQL sunucu veritabanı güvenlik ölçümlerini oluşturma ve yönetme |
> | Microsoft.Sql/servers/veritabanları/sensitivityLabels/* |  |
> | Microsoft.Sql/servers/veritabanları/transparentDataEncryption/* |  |
> | Microsoft.Sql/servers/veritabanları/güvenlik açığıAssessments/* |  |
> | Microsoft.Sql/servers/veritabanları/güvenlik açığıAssessmentScans/* |  |
> | Microsoft.Sql/servers/veritabanları/güvenlik açığıAssessmentSettings/* |  |
> | Microsoft.Sql/sunucular/firewallRules/* |  |
> | Microsoft.Sql/servers/read | Sunucu listesini döndürün veya belirtilen sunucunun özelliklerini alır. |
> | Microsoft.Sql/servers/securityAlertPolicies/* | SQL sunucu güvenliği uyarı ilkeleri oluşturma ve yönetme |
> | Microsoft.Sql/servers/vulnerabilityAssessments/* |  |
> | Microsoft.Support/* | Destek biletleri oluşturma ve yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
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

### <a name="sql-server-contributor"></a>SQL Server Katılımcısı

SQL sunucularını ve veritabanlarını yönetmenize olanak tanır, ancak bunlara erişmez ve güvenlikle ilgili ilkelerini yönetmez.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/oku | Rolleri ve rol ödevlerini okuma |
> | Microsoft.Insights/alertRules/* | İstatistikler uyarı kuralları oluşturma ve yönetme |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynakların kullanılabilirlik durumlarını alır |
> | Microsoft.Resources/deployments/* | Kaynak grubu dağıtımları oluşturma ve yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Sql/locations/*/oku |  |
> | Microsoft.Sql/sunucular/* | SQL sunucuları oluşturma ve yönetme |
> | Microsoft.Support/* | Destek biletleri oluşturma ve yönetme |
> | Microsoft.Insights/metrics/read | Ölçümleri okuyun |
> | Microsoft.Insights/metricDefinitions/read | Metrik tanımları okuma |
> | **NotActions** |  |
> | Microsoft.Sql/managedInstances/databases/currentSensitivityLabels/* |  |
> | Microsoft.Sql/managedInstances/databases/recommendedSensitivityLabels/* |  |
> | Microsoft.Sql/managedInstances/databases/schemas/tables/columns/sensitivityLabels/* |  |
> | Microsoft.Sql/managedInstances/veritabanları/securityAlertPolicies/* |  |
> | Microsoft.Sql/managedInstances/veritabanları/sensitivityLabels/* |  |
> | Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/* |  |
> | Microsoft.Sql/managedInstances/securityAlertPolicies/* |  |
> | Microsoft.Sql/managedInstances/vulnerabilityAssessments/* |  |
> | Microsoft.Sql/servers/auditingPolicies/* | SQL sunucu denetim ilkelerini düzenleme |
> | Microsoft.Sql/servers/auditingSettings/* | SQL sunucu denetim ayarlarını düzenleme |
> | Microsoft.Sql/servers/databases/auditingPolicies/* | SQL server veritabanı denetim ilkelerini düzenleme |
> | Microsoft.Sql/servers/databases/auditingSettings/* | SQL server veritabanı denetim ayarlarını düzenleme |
> | Microsoft.Sql/servers/databases/auditRecords/read | Veritabanı blob denetim kayıtlarını alma |
> | Microsoft.Sql/servers/databases/connectionPolicies/* | SQL server veritabanı bağlantı ilkelerini düzenleme |
> | Microsoft.Sql/servers/databases/currentSensitivityLabels/* |  |
> | Microsoft.Sql/servers/veritabanları/dataMaskingPolicies/* | SQL server veritabanı veri maskeleme ilkelerini düzenleme |
> | Microsoft.Sql/servers/databases/extendedAuditingSettings/* |  |
> | Microsoft.Sql/servers/veritabanları/recommendedSensitivityLabels/* |  |
> | Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/* |  |
> | Microsoft.Sql/servers/veritabanları/securityAlertPolicies/* | SQL server veritabanı güvenlik uyarı ilkelerini düzenleme |
> | Microsoft.Sql/servers/veritabanları/securityMetrics/* | SQL server veritabanı güvenlik ölçümlerini düzenleme |
> | Microsoft.Sql/servers/veritabanları/sensitivityLabels/* |  |
> | Microsoft.Sql/servers/veritabanları/güvenlik açığıAssessments/* |  |
> | Microsoft.Sql/servers/veritabanları/güvenlik açığıAssessmentScans/* |  |
> | Microsoft.Sql/servers/veritabanları/güvenlik açığıAssessmentSettings/* |  |
> | Microsoft.Sql/servers/extendedAuditingSettings/* |  |
> | Microsoft.Sql/servers/securityAlertPolicies/* | SQL sunucu güvenlik uyarı ilkelerini düzenleme |
> | Microsoft.Sql/servers/vulnerabilityAssessments/* |  |
> | **DataActions** |  |
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


### <a name="azure-event-hubs-data-owner"></a>Azure Etkinlik Hub'ları Veri Sahibi

Azure Etkinlik Hub'ları kaynaklarına tam erişim sağlar.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.EventHub/* |  |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | Microsoft.EventHub/* |  |
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

### <a name="azure-event-hubs-data-receiver"></a>Azure Etkinlik Hub'ları Veri Alıcısı

Azure Etkinlik Hub'ları kaynaklarına erişim sağlar.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.EventHub/*/eventhubs/consumergroups/read |  |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | Microsoft.EventHub/*/receive/action |  |
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

### <a name="azure-event-hubs-data-sender"></a>Azure Olay Hub'ları Veri Gönderen

Azure Etkinlik Hub'ları kaynaklarına erişim gönderme olanağı sağlar.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.EventHub/*/eventhubs/oku |  |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | Microsoft.EventHub/*/gönder/eylem |  |
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

### <a name="data-factory-contributor"></a>Veri Fabrikası Katılımcısı

Veri fabrikalarının yanı sıra içlerindeki alt kaynaklar oluşturun ve yönetin.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/oku | Rolleri ve rol ödevlerini okuma |
> | Microsoft.DataFactory/dataFactorys/* | Veri fabrikaları ve bunların içindeki alt kaynakları oluşturun ve yönetin. |
> | Microsoft.DataFactory/fabrikalar/* | Veri fabrikaları ve bunların içindeki alt kaynakları oluşturun ve yönetin. |
> | Microsoft.Insights/alertRules/* | İstatistikler uyarı kuralları oluşturma ve yönetme |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynakların kullanılabilirlik durumlarını alır |
> | Microsoft.Resources/deployments/* | Kaynak grubu dağıtımları oluşturma ve yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Destek biletleri oluşturma ve yönetme |
> | Microsoft.EventGrid/eventSubscriptions/write | Bir etkinlik oluşturma veya güncellemeAbonelik |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
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

### <a name="data-purger"></a>Veri Purger

Analiz verilerini temizleyebilir

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.Insights/components/*/read |  |
> | Microsoft.Insights/components/tasfiye/action | Uygulama Öngörülerinden verileri temizleme |
> | Microsoft.OperationalInsights/çalışma alanları/*/okuyun | Günlük analizi verilerini görüntüleme |
> | Microsoft.OperationalInsights/çalışma alanları/tasfiye/eylem | Belirtilen verileri çalışma alanından silme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
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

### <a name="hdinsight-cluster-operator"></a>HDInsight Küme Operatörü

HDInsight küme yapılandırmalarını okumanızı ve değiştirmenizi sağlar.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.HDInsight/*/oku |  |
> | Microsoft.HDInsight/kümeler/getGatewayAyarlar/action | HDInsight Cluster için ağ geçidi ayarlarını edinin |
> | Microsoft.HDInsight/kümeler/updateGatewaySettings/action | HDInsight Cluster için ağ geçidi ayarlarını güncelleştirin |
> | Microsoft.HDInsight/kümeler/yapılandırmalar/* |  |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Resources/deployments/operations/read | Dağıtım işlemlerini alır veya listeler. |
> | Microsoft.Insights/alertRules/* | İstatistikler uyarı kuralları oluşturma ve yönetme |
> | Microsoft.Authorization/*/oku | Rolleri ve rol ödevlerini okuma |
> | Microsoft.Support/* | Destek biletleri oluşturma ve yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
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

### <a name="hdinsight-domain-services-contributor"></a>HDInsight Etki Alanı Hizmetleri Katılımcısı

HDInsight Kurumsal Güvenlik Paketi için gerekli Olan Etki Alanı Hizmetlerini Okuyabilir, Oluşturabilir, Değiştirebilir ve Silebilirsiniz

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.AAD/*/oku |  |
> | Microsoft.AAD/domainServices/*/oku |  |
> | Microsoft.AAD/domainServices/oucontainer/* |  |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
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

Log Analytics Katılımcısı tüm izleme verilerini okuyabilir ve izleme ayarlarını düzeltebilir. İzleme ayarlarını düzenleme, VM uzantısını VM'lere eklemeyi içerir; Azure Depolama'dan günlük koleksiyonunu yapılandırabilmek için depolama hesabı anahtarlarını okuma; Otomasyon hesapları oluşturma ve yapılandırma; çözümler ekleme; ve Azure tanılamalarını tüm Azure kaynaklarında yapılandırma.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | */okumak | Sırlar hariç her türlü kaynağı okuyun. |
> | Microsoft.Automation/automationAccounts/* |  |
> | Microsoft.ClassicCompute/virtualMachines/uzantı/* |  |
> | Microsoft.ClassicStorage/storageAccounts/listKeys/action | Depolama hesaplarının erişim anahtarlarını listeler. |
> | Microsoft.Compute/virtualMachines/extensions/* |  |
> | Microsoft.Insights/alertRules/* | İstatistikler uyarı kuralları oluşturma ve yönetme |
> | Microsoft.Insights/diagnosticSettings/* | Analysis Server için tanılama ayarını oluşturur, günceller veya okur |
> | Microsoft.OperationalInsights/* |  |
> | Microsoft.OperationsManagement/* |  |
> | Microsoft.Resources/deployments/* | Kaynak grubu dağıtımları oluşturma ve yönetme |
> | Microsoft.Resources/subscriptions/resourcegroups/deployments/* |  |
> | Microsoft.Storage/storageAccounts/listKeys/action | Belirtilen depolama hesabının erişim anahtarlarını döndürür. |
> | Microsoft.Support/* | Destek biletleri oluşturma ve yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
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

Log Analytics Reader, Azure tanılama yapılandırmasını tüm Azure kaynaklarında görüntülemek de dahil olmak üzere tüm izleme verilerinin yanı sıra izleme ayarlarını görüntüleyebilir ve görüntüleyebilir.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | */okumak | Sırlar hariç her türlü kaynağı okuyun. |
> | Microsoft.OperationalInsights/çalışma alanları/analitik/sorgu/eylem | Yeni motoru kullanarak arama yapın. |
> | Microsoft.OperationalInsights/çalışma alanları/arama/eylem | Arama sorgusunun yürütülmesi |
> | Microsoft.Support/* | Destek biletleri oluşturma ve yönetme |
> | **NotActions** |  |
> | Microsoft.OperationalInsights/çalışma alanları/sharedKeys/read | Çalışma alanı için paylaşılan anahtarları alır. Bu anahtarlar, Microsoft Operasyonel Öngörüler aracılarını çalışma alanına bağlamak için kullanılır. |
> | **DataActions** |  |
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


### <a name="blockchain-member-node-access-preview"></a>Blockchain Üye Düğümü Erişimi (Önizleme)

Blockchain Üye düğümlerine erişim sağlar

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.Blockchain/blockchainÜyeler/transactionNodes/read | Varolan Blockchain Üye İşlem Düğümü(ler) alır veya listeler. |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | Microsoft.Blockchain/blockchainÜyeler/transactionNodes/connect/action | Blockchain Üye İşlem Düğümüne bağlanır. |
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


### <a name="cognitive-services-contributor"></a>Bilişsel Hizmetler Katılımcısı

Bilişsel Hizmetler'in anahtarlarını oluşturmanızı, okumanızı, güncellemenizi, silmenizi ve yönetmenize olanak tanır.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/oku | Rolleri ve rol ödevlerini okuma |
> | Microsoft.CognitiveServices/* |  |
> | Microsoft.Özellikler/özellikler/okuma | Aboneliğin özelliklerini alır. |
> | Microsoft.Özellikler/sağlayıcılar/özellikler/okuma | Belirli bir kaynak sağlayıcısında abonelik özelliğini alır. |
> | Microsoft.Insights/alertRules/* | İstatistikler uyarı kuralları oluşturma ve yönetme |
> | Microsoft.Insights/diagnosticSettings/* | Analysis Server için tanılama ayarını oluşturur, günceller veya okur |
> | Microsoft.Insights/logDefinitions/read | Günlük tanımlarını okuma |
> | Microsoft.Insights/metricdefinitions/read | Metrik tanımları okuma |
> | Microsoft.Insights/metrics/read | Ölçümleri okuyun |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynakların kullanılabilirlik durumlarını alır |
> | Microsoft.Resources/deployments/* | Kaynak grubu dağıtımları oluşturma ve yönetme |
> | Microsoft.Resources/deployments/operations/read | Dağıtım işlemlerini alır veya listeler. |
> | Microsoft.Resources/subscriptions/operationresults/read | Abonelik işlemi sonuçlarını alın. |
> | Microsoft.Resources/abonelikler/okuma | Aboneliklistesini alır. |
> | Microsoft.Resources/subscriptions/resourcegroups/deployments/* |  |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Destek biletleri oluşturma ve yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
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

### <a name="cognitive-services-data-reader-preview"></a>Bilişsel Hizmetler Veri Okuyucu (Önizleme)

Bilişsel Hizmetler verilerini okumanızı sağlar.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | *yok* |  |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | Microsoft.CognitiveServices/*/oku |  |
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

### <a name="cognitive-services-user"></a>Bilişsel Hizmetler Kullanıcı

Bilişsel Hizmetler'in anahtarlarını okumanızı ve listelemenizi sağlar.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.CognitiveServices/*/oku |  |
> | Microsoft.CognitiveServices/accounts/listkeys/action | Liste Anahtarları |
> | Microsoft.Insights/alertRules/read | Klasik metrik uyarıyı okuma |
> | Microsoft.Insights/diagnosticSettings/read | Kaynak tanılama ayarını okuma |
> | Microsoft.Insights/logDefinitions/read | Günlük tanımlarını okuma |
> | Microsoft.Insights/metricdefinitions/read | Metrik tanımları okuma |
> | Microsoft.Insights/metrics/read | Ölçümleri okuyun |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynakların kullanılabilirlik durumlarını alır |
> | Microsoft.Resources/deployments/operations/read | Dağıtım işlemlerini alır veya listeler. |
> | Microsoft.Resources/subscriptions/operationresults/read | Abonelik işlemi sonuçlarını alın. |
> | Microsoft.Resources/abonelikler/okuma | Aboneliklistesini alır. |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Destek biletleri oluşturma ve yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | Microsoft.CognitiveServices/* |  |
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


### <a name="spatial-anchors-account-contributor"></a>Uzamsal Çapalar Hesap Katılımcısı

Hesabınızdaki uzamsal çapaları yönetmenize olanak tanır, ancak bunları silmez

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | *yok* |  |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | Microsoft.MixedReality/SpatialAnchorsAccounts/create/action | Uzamsal çapalar oluşturma |
> | Microsoft.MixedReality/SpatialAnchorsAccounts/discovery/read | Yakındaki mekansal çapaları keşfedin |
> | Microsoft.MixedReality/SpatialAnchorsAccounts/properties/read | Uzamsal çapaların özelliklerini alın |
> | Microsoft.MixedReality/SpatialAnchorsAccounts/query/read | Mekansal çapaları bulma |
> | Microsoft.MixedReality/SpatialAnchorsAccounts/submitdiag/read | Azure Uzamsal Çapalar hizmetinin kalitesini artırmaya yardımcı olmak için tanılama verilerini gönderin |
> | Microsoft.MixedReality/SpatialAnchorsAccounts/write | Uzamsal bağlantı özelliklerini güncelleştirme |
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

### <a name="spatial-anchors-account-owner"></a>Uzamsal Çapalar Hesap Sahibi

Hesabınızdaki uzamsal çapaları silme dahil olmak üzere yönetmenize olanak tanır

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | *yok* |  |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | Microsoft.MixedReality/SpatialAnchorsAccounts/create/action | Uzamsal çapalar oluşturma |
> | Microsoft.MixedReality/SpatialAnchorsAccounts/delete | Uzamsal çapaları silme |
> | Microsoft.MixedReality/SpatialAnchorsAccounts/discovery/read | Yakındaki mekansal çapaları keşfedin |
> | Microsoft.MixedReality/SpatialAnchorsAccounts/properties/read | Uzamsal çapaların özelliklerini alın |
> | Microsoft.MixedReality/SpatialAnchorsAccounts/query/read | Mekansal çapaları bulma |
> | Microsoft.MixedReality/SpatialAnchorsAccounts/submitdiag/read | Azure Uzamsal Çapalar hizmetinin kalitesini artırmaya yardımcı olmak için tanılama verilerini gönderin |
> | Microsoft.MixedReality/SpatialAnchorsAccounts/write | Uzamsal bağlantı özelliklerini güncelleştirme |
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

### <a name="spatial-anchors-account-reader"></a>Uzamsal Çapalar Hesap Okuyucu

Hesabınızdaki uzamsal çapaların özelliklerini bulmanızı ve okumanızı sağlar

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | *yok* |  |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | Microsoft.MixedReality/SpatialAnchorsAccounts/discovery/read | Yakındaki mekansal çapaları keşfedin |
> | Microsoft.MixedReality/SpatialAnchorsAccounts/properties/read | Uzamsal çapaların özelliklerini alın |
> | Microsoft.MixedReality/SpatialAnchorsAccounts/query/read | Mekansal çapaları bulma |
> | Microsoft.MixedReality/SpatialAnchorsAccounts/submitdiag/read | Azure Uzamsal Çapalar hizmetinin kalitesini artırmaya yardımcı olmak için tanılama verilerini gönderin |
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


### <a name="api-management-service-contributor"></a>API Yönetim Hizmeti Katılımcısı

Hizmeti ve API'leri yönetebilir

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.ApiManagement/service/* | API Yönetimi hizmeti oluşturma ve yönetme |
> | Microsoft.Authorization/*/oku | Rolleri ve rol ödevlerini okuma |
> | Microsoft.Insights/alertRules/* | İstatistikler uyarı kuralları oluşturma ve yönetme |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynakların kullanılabilirlik durumlarını alır |
> | Microsoft.Resources/deployments/* | Kaynak grubu dağıtımları oluşturma ve yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Destek biletleri oluşturma ve yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
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

### <a name="api-management-service-operator-role"></a>API Yönetim Hizmeti Operatörü Rolü

Hizmeti yönetebilir, ancak API'leri yönetemez

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.ApiManagement/service/*/read | API Yönetim Hizmeti örneklerini okuyun |
> | Microsoft.ApiManagement/service/backup/action | Kullanıcı tarafından sağlanan depolama hesabında belirtilen kapsayıcıya YEDEKLEME API Yönetim Hizmeti |
> | Microsoft.ApiManagement/service/delete | API Yönetim Hizmeti örneğini silme |
> | Microsoft.ApiManagement/service/managedeployments/action | SKU/birimlerini değiştirin, API Yönetim Hizmetinin bölgesel dağıtımlarını ekleyin/kaldırın |
> | Microsoft.ApiManagement/service/read | API Yönetim Hizmeti örneği için meta verileri okuyun |
> | Microsoft.ApiManagement/service/restore/action | Kullanıcı tarafından sağlanan depolama hesabında belirtilen kapsayıcıdan API Yönetim Hizmeti geri yükleme |
> | Microsoft.ApiManagement/service/updatecertificate/action | API Yönetim Hizmeti için SSL sertifikası yükleme |
> | Microsoft.ApiManagement/service/updatehostname/action | API Yönetim Hizmeti için özel alan adları ayarlama, güncelleştirme veya kaldırma |
> | Microsoft.ApiManagement/service/write | API Yönetim Hizmeti örneği oluşturma veya güncelleştirme |
> | Microsoft.Authorization/*/oku | Rolleri ve rol ödevlerini okuma |
> | Microsoft.Insights/alertRules/* | İstatistikler uyarı kuralları oluşturma ve yönetme |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynakların kullanılabilirlik durumlarını alır |
> | Microsoft.Resources/deployments/* | Kaynak grubu dağıtımları oluşturma ve yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Destek biletleri oluşturma ve yönetme |
> | **NotActions** |  |
> | Microsoft.ApiManagement/service/users/keys/read | Kullanıcıyla ilişkili anahtarlar alma |
> | **DataActions** |  |
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

### <a name="api-management-service-reader-role"></a>API Yönetimi Hizmet Okuyucu Rolü

Hizmet e ve API'lere salt okunur erişim

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.ApiManagement/service/*/read | API Yönetim Hizmeti örneklerini okuyun |
> | Microsoft.ApiManagement/service/read | API Yönetim Hizmeti örneği için meta verileri okuyun |
> | Microsoft.Authorization/*/oku | Rolleri ve rol ödevlerini okuma |
> | Microsoft.Insights/alertRules/* | İstatistikler uyarı kuralları oluşturma ve yönetme |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynakların kullanılabilirlik durumlarını alır |
> | Microsoft.Resources/deployments/* | Kaynak grubu dağıtımları oluşturma ve yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Destek biletleri oluşturma ve yönetme |
> | **NotActions** |  |
> | Microsoft.ApiManagement/service/users/keys/read | Kullanıcıyla ilişkili anahtarlar alma |
> | **DataActions** |  |
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

### <a name="app-configuration-data-owner"></a>Uygulama Yapılandırma Veri Sahibi

Uygulama Yapılandırma verilerine tam erişim sağlar.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | *yok* |  |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | Microsoft.AppConfiguration/configurationStores/*/oku |  |
> | Microsoft.AppConfiguration/configurationStores/*/write |  |
> | Microsoft.AppConfiguration/configurationStores/*/delete |  |
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

### <a name="app-configuration-data-reader"></a>Uygulama Yapılandırma Veri Okuyucu

Uygulama Yapılandırma verilerine okuma erişimi sağlar.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | *yok* |  |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | Microsoft.AppConfiguration/configurationStores/*/oku |  |
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

### <a name="azure-service-bus-data-owner"></a>Azure Hizmet Veri Veri Seçişleri Veri Sahibi

Azure Hizmet Veri Servisi kaynaklarına tam erişim sağlar.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.ServiceBus/* |  |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | Microsoft.ServiceBus/* |  |
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

### <a name="azure-service-bus-data-receiver"></a>Azure Hizmet Veri Veri Alıcısı

Azure Hizmet Veri Servisi kaynaklarına erişim sağlar.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.ServiceBus/*/kuyruklar/okuma |  |
> | Microsoft.ServiceBus/*/topics/read |  |
> | Microsoft.ServiceBus/*/topics/subscriptions/read |  |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | Microsoft.ServiceBus/*/receive/action |  |
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

### <a name="azure-service-bus-data-sender"></a>Azure Hizmet Veri Veri Gönderen

Azure Hizmet Veri Servisi kaynaklarına erişim göndermeolanağı sağlar.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.ServiceBus/*/kuyruklar/okuma |  |
> | Microsoft.ServiceBus/*/topics/read |  |
> | Microsoft.ServiceBus/*/topics/subscriptions/read |  |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | Microsoft.ServiceBus/*/gönder/eylem |  |
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

### <a name="azure-stack-registration-owner"></a>Azure Yığını Kayıt Sahibi

Azure Yığını kayıtlarını yönetmenize olanak tanır.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.AzureStack/kayıtlar/ürünler/*/eylem |  |
> | Microsoft.AzureStack/kayıtlar/ürünler/okuma | Azure Yığını Marketi ürününün özelliklerini alır |
> | Microsoft.AzureStack/kayıtlar/okuma | Azure Yığını kaydının özelliklerini alır |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
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

EventGrid etkinlik abonelik işlemlerini yönetmenize olanak tanır.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/oku | Rolleri ve rol ödevlerini okuma |
> | Microsoft.EventGrid/eventSubscriptions/* |  |
> | Microsoft.EventGrid/topicTypes/eventSubscriptions/read | Genel etkinlik aboneliklerini konu türüne göre listele |
> | Microsoft.EventGrid/locations/eventSubscriptions/read | Bölgesel etkinlik aboneliklerini listele |
> | Microsoft.EventGrid/locations/topicTypes/eventSubscriptions/read | Bölgesel etkinlik aboneliklerini konu türüne göre listele |
> | Microsoft.Insights/alertRules/* | İstatistikler uyarı kuralları oluşturma ve yönetme |
> | Microsoft.Resources/deployments/* | Kaynak grubu dağıtımları oluşturma ve yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Destek biletleri oluşturma ve yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
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

### <a name="eventgrid-eventsubscription-reader"></a>EventGrid EventSubscription Okuyucu

EventGrid etkinlik aboneliklerini okumanızı sağlar.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/oku | Rolleri ve rol ödevlerini okuma |
> | Microsoft.EventGrid/eventSubscriptions/read | Bir etkinliği okuyunAbonelik |
> | Microsoft.EventGrid/topicTypes/eventSubscriptions/read | Genel etkinlik aboneliklerini konu türüne göre listele |
> | Microsoft.EventGrid/locations/eventSubscriptions/read | Bölgesel etkinlik aboneliklerini listele |
> | Microsoft.EventGrid/locations/topicTypes/eventSubscriptions/read | Bölgesel etkinlik aboneliklerini konu türüne göre listele |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
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

### <a name="intelligent-systems-account-contributor"></a>Akıllı Sistemler Hesabı Katılımcısı

Akıllı Sistemler hesaplarını yönetmenize olanak tanır, ancak bunlara erişmemenize olanak tanır.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/oku | Rolleri ve rol ödevlerini okuma |
> | Microsoft.Insights/alertRules/* | İstatistikler uyarı kuralları oluşturma ve yönetme |
> | Microsoft.IntelligentSystems/hesapları/* | Akıllı sistem hesapları oluşturma ve yönetme |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynakların kullanılabilirlik durumlarını alır |
> | Microsoft.Resources/deployments/* | Kaynak grubu dağıtımları oluşturma ve yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Destek biletleri oluşturma ve yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
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

### <a name="logic-app-contributor"></a>Mantık Uygulaması Katılımcısı

Mantıksal uygulamaları yönetmenize olanak tanır, ancak bunlara erişimi değiştirmez.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/oku | Rolleri ve rol ödevlerini okuma |
> | Microsoft.ClassicStorage/storageAccounts/listKeys/action | Depolama hesaplarının erişim anahtarlarını listeler. |
> | Microsoft.ClassicStorage/storageAccounts/read | Depolama hesabını verilen hesapla döndürün. |
> | Microsoft.Insights/alertRules/* | İstatistikler uyarı kuralları oluşturma ve yönetme |
> | Microsoft.Insights/metricAlerts/* |  |
> | Microsoft.Insights/diagnosticSettings/* | Analysis Server için tanılama ayarını oluşturur, günceller veya okur |
> | Microsoft.Insights/logdefinitions/* | Bu izin, portal üzerinden Etkinlik Günlükleri'ne erişmesi gereken kullanıcılar için gereklidir. Günlük kategorilerini Etkinlik Günlüğü'nde listeleyin. |
> | Microsoft.Insights/metricDefinitions/* | Metrik tanımları okuyun (kaynak için kullanılabilir metrik türlerinin listesi). |
> | Microsoft.Logic/* | Logic Apps kaynaklarını yönetir. |
> | Microsoft.Resources/deployments/* | Kaynak grubu dağıtımları oluşturma ve yönetme |
> | Microsoft.Resources/subscriptions/operationresults/read | Abonelik işlemi sonuçlarını alın. |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Storage/storageAccounts/listkeys/action | Belirtilen depolama hesabının erişim anahtarlarını döndürür. |
> | Microsoft.Storage/storageAccounts/read | Depolama hesapları listesini verir veya belirtilen depolama hesabının özelliklerini alır. |
> | Microsoft.Support/* | Destek biletleri oluşturma ve yönetme |
> | Microsoft.Web/connectionAğ GeçitLeri/* | Bağlantı Ağ Geçidi oluşturun ve yönetin. |
> | Microsoft.Web/bağlantılar/* | Bağlantı oluşturun ve yönetir. |
> | Microsoft.Web/customApis/* | Özel API oluşturur ve yönetir. |
> | Microsoft.Web/serverFarms/join/action |  |
> | Microsoft.Web/serverFarms/okumak | Özellikleri Uygulama Hizmet Planına alın |
> | Microsoft.Web/siteler/işlevler/listSecrets/action | Liste Fonksiyonu sırları. |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
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

### <a name="logic-app-operator"></a>Mantık Uygulama Operatörü

Mantık uygulamalarını okumanızı, etkinleştirmenizi ve devre dışı bırakamanızı, ancak bunları güncellememenizi veya güncellememenizi sağlar.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/oku | Rolleri ve rol ödevlerini okuma |
> | Microsoft.Insights/alertRules/*/oku | Insights uyarı kurallarını okuyun |
> | Microsoft.Insights/metricAlerts/*/oku |  |
> | Microsoft.Insights/diagnosticSettings/*/read | Logic Apps için tanılama ayarlarını alır |
> | Microsoft.Insights/metricDefinitions/*/oku | Logic Apps için kullanılabilir ölçümleri alır. |
> | Microsoft.Logic/*/oku | Logic Apps kaynaklarını okur. |
> | Microsoft.Logic/iş akışları/devre dışı/eylem | İş akışını devre dışı kılabilir. |
> | Microsoft.Logic/iş akışları/etkinleştirme/eylem | İş akışını sağlar. |
> | Microsoft.Logic/iş akışları/doğrulama/eylem | İş akışını doğrular. |
> | Microsoft.Resources/deployments/operations/read | Dağıtım işlemlerini alır veya listeler. |
> | Microsoft.Resources/subscriptions/operationresults/read | Abonelik işlemi sonuçlarını alın. |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Destek biletleri oluşturma ve yönetme |
> | Microsoft.Web/connectionAğ GeçitLeri/*/oku | Bağlantı Ağ Geçitlerini okuyun. |
> | Microsoft.Web/bağlantılar/*/okuma | Bağlantılar'ı okuyun. |
> | Microsoft.Web/customApis/*/oku | Özel API'yi okuyun. |
> | Microsoft.Web/serverFarms/okumak | Özellikleri Uygulama Hizmet Planına alın |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
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


### <a name="managed-identity-contributor"></a>Yönetilen Kimlik Katkıda Bulunan

Kullanıcı Tarafından Atanan Kimlik Oluşturma, Okuma, Güncelleme ve Silme

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.ManagedIdentity/userAssignedIdentities/read | Varolan bir kullanıcıya atanan kimliği alır |
> | Microsoft.ManagedIdentity/userAssignedIdentities/write | Atanan yeni bir kullanıcı kimliği oluşturur veya varolan kullanıcı tarafından atanan kimlikle ilişkili etiketleri güncelleştirir |
> | Microsoft.ManagedIdentity/userAssignedIdentities/delete | Varolan kullanıcıya atanan kimliği siler |
> | Microsoft.Authorization/*/oku | Rolleri ve rol ödevlerini okuma |
> | Microsoft.Insights/alertRules/* | İstatistikler uyarı kuralları oluşturma ve yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Resources/deployments/* | Kaynak grubu dağıtımları oluşturma ve yönetme |
> | Microsoft.Support/* | Destek biletleri oluşturma ve yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
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

### <a name="managed-identity-operator"></a>Yönetilen Kimlik Operatörü

Kullanıcı Atanan Kimliği Okuma ve Atama

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.ManagedIdentity/userAssignedIdentities/*/oku |  |
> | Microsoft.ManagedIdentity/userAssignedIdentities/*/atay/eylem |  |
> | Microsoft.Authorization/*/oku | Rolleri ve rol ödevlerini okuma |
> | Microsoft.Insights/alertRules/* | İstatistikler uyarı kuralları oluşturma ve yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Resources/deployments/* | Kaynak grubu dağıtımları oluşturma ve yönetme |
> | Microsoft.Support/* | Destek biletleri oluşturma ve yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
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
> | Microsoft.SecurityInsights/* |  |
> | Microsoft.OperationalInsights/çalışma alanları/analitik/sorgu/eylem | Yeni motoru kullanarak arama yapın. |
> | Microsoft.OperationalInsights/çalışma alanları/*/okuyun | Günlük analizi verilerini görüntüleme |
> | Microsoft.OperationalInsights/çalışma alanları/savedSearches/* |  |
> | Microsoft.OperationsManagement/solutions/read | Çıkan OMS çözümlerini alın |
> | Microsoft.OperationalInsights/çalışma alanları/sorgu/okuma | Çalışma alanındaki verilerin üzerinde sorgu çalıştırma |
> | Microsoft.OperationalInsights/çalışma alanları/sorgu/*/okuma |  |
> | Microsoft.OperationalInsights/çalışma alanları/dataSources/read | Bir çalışma alanı altında veri kaynakları alın. |
> | Microsoft.Insights/çalışma kitapları/* |  |
> | Microsoft.Authorization/*/oku | Rolleri ve rol ödevlerini okuma |
> | Microsoft.Insights/alertRules/* | İstatistikler uyarı kuralları oluşturma ve yönetme |
> | Microsoft.Resources/deployments/* | Kaynak grubu dağıtımları oluşturma ve yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Destek biletleri oluşturma ve yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
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

### <a name="azure-sentinel-reader"></a>Azure Sentinel Okuyucu

Azure Sentinel Okuyucu

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.SecurityInsights/*/oku |  |
> | Microsoft.OperationalInsights/çalışma alanları/analitik/sorgu/eylem | Yeni motoru kullanarak arama yapın. |
> | Microsoft.OperationalInsights/çalışma alanları/*/okuyun | Günlük analizi verilerini görüntüleme |
> | Microsoft.OperationalInsights/çalışma alanları/LinkedServices/read | Verilen çalışma alanı altında bağlantılı hizmetler alın. |
> | Microsoft.OperationalInsights/çalışma alanları/savedSearches/read | Kaydedilmiş bir arama sorgusu alır |
> | Microsoft.OperationsManagement/solutions/read | Çıkan OMS çözümlerini alın |
> | Microsoft.OperationalInsights/çalışma alanları/sorgu/okuma | Çalışma alanındaki verilerin üzerinde sorgu çalıştırma |
> | Microsoft.OperationalInsights/çalışma alanları/sorgu/*/okuma |  |
> | Microsoft.OperationalInsights/çalışma alanları/dataSources/read | Bir çalışma alanı altında veri kaynakları alın. |
> | Microsoft.Insights/çalışma kitapları/okuma | Çalışma kitabını okuma |
> | Microsoft.Authorization/*/oku | Rolleri ve rol ödevlerini okuma |
> | Microsoft.Insights/alertRules/* | İstatistikler uyarı kuralları oluşturma ve yönetme |
> | Microsoft.Resources/deployments/* | Kaynak grubu dağıtımları oluşturma ve yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Destek biletleri oluşturma ve yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
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

### <a name="azure-sentinel-responder"></a>Azure Sentinel Yanıtlayıcı

Azure Sentinel Yanıtlayıcı

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.SecurityInsights/*/oku |  |
> | Microsoft.SecurityInsights/durumlarda/* |  |
> | Microsoft.SecurityInsights/olaylar/* |  |
> | Microsoft.OperationalInsights/çalışma alanları/analitik/sorgu/eylem | Yeni motoru kullanarak arama yapın. |
> | Microsoft.OperationalInsights/çalışma alanları/*/okuyun | Günlük analizi verilerini görüntüleme |
> | Microsoft.OperationalInsights/çalışma alanları/dataSources/read | Bir çalışma alanı altında veri kaynakları alın. |
> | Microsoft.OperationalInsights/çalışma alanları/savedSearches/read | Kaydedilmiş bir arama sorgusu alır |
> | Microsoft.OperationsManagement/solutions/read | Çıkan OMS çözümlerini alın |
> | Microsoft.OperationalInsights/çalışma alanları/sorgu/okuma | Çalışma alanındaki verilerin üzerinde sorgu çalıştırma |
> | Microsoft.OperationalInsights/çalışma alanları/sorgu/*/okuma |  |
> | Microsoft.OperationalInsights/çalışma alanları/dataSources/read | Bir çalışma alanı altında veri kaynakları alın. |
> | Microsoft.Insights/çalışma kitapları/okuma | Çalışma kitabını okuma |
> | Microsoft.Authorization/*/oku | Rolleri ve rol ödevlerini okuma |
> | Microsoft.Insights/alertRules/* | İstatistikler uyarı kuralları oluşturma ve yönetme |
> | Microsoft.Resources/deployments/* | Kaynak grubu dağıtımları oluşturma ve yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Destek biletleri oluşturma ve yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
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

### <a name="key-vault-contributor"></a>Anahtar Vault Katılımcı

Anahtar kasalarını yönetmenize olanak tanır, ancak bunlara erişmemenize izin verir.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/oku | Rolleri ve rol ödevlerini okuma |
> | Microsoft.Insights/alertRules/* | İstatistikler uyarı kuralları oluşturma ve yönetme |
> | Microsoft.KeyVault/* |  |
> | Microsoft.Resources/deployments/* | Kaynak grubu dağıtımları oluşturma ve yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Destek biletleri oluşturma ve yönetme |
> | **NotActions** |  |
> | Microsoft.KeyVault/locations/deletedVaults/tasfiye/action | Yumuşak silinmiş anahtar kasası temizleme |
> | Microsoft.KeyVault/hsmPools/* |  |
> | **DataActions** |  |
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

Güvenlik ilkelerini görüntüleyebilir, güvenlik durumlarını görüntüleyebilir, güvenlik ilkelerini edinebilir, uyarıları ve önerileri görüntüleyebilir, uyarıları ve önerileri reddedebilir.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/oku | Rolleri ve rol ödevlerini okuma |
> | Microsoft.Authorization/policyAssignments/* | İlke atamaları oluşturma ve yönetme |
> | Microsoft.Authorization/policyDefinitions/* | İlke tanımları oluşturma ve yönetme |
> | Microsoft.Authorization/policySetDefinitions/* | İlke kümeleri oluşturma ve yönetme |
> | Microsoft.Insights/alertRules/* | İstatistikler uyarı kuralları oluşturma ve yönetme |
> | Microsoft.Management/managementGroups/read | Kimlik doğrulaması yapılan kullanıcı için liste yönetim grupları. |
> | Microsoft.operationalInsights/çalışma alanları/*/oku | Günlük analizi verilerini görüntüleme |
> | Microsoft.Resources/deployments/* | Kaynak grubu dağıtımları oluşturma ve yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Security/* | Güvenlik bileşenleri ve ilkeleri oluşturma ve yönetme |
> | Microsoft.Support/* | Destek biletleri oluşturma ve yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
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

### <a name="security-assessment-contributor"></a>Güvenlik Değerlendirmesi Katılımcısı

Değerlendirmeleri Güvenlik Merkezi'ne itmenizi sağlar

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.Security/assessments/write | Aboneliğinizde güvenlik değerlendirmeleri oluşturma veya güncelleme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
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

### <a name="security-manager-legacy"></a>Güvenlik Yöneticisi (Eski)

Bu eski bir rol. Bunun yerine Lütfen Güvenlik Yöneticisi'ni kullanın.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/oku | Rolleri ve rol ödevlerini okuma |
> | Microsoft.ClassicCompute/*/oku | Yapılandırma bilgilerini klasik sanal makineleri okuyun |
> | Microsoft.ClassicCompute/virtualMachines/*/write | Klasik sanal makineler için yapılandırma yazma |
> | Microsoft.ClassicNetwork/*/oku | Klasik ağ hakkındaki yapılandırma bilgilerini okuma |
> | Microsoft.Insights/alertRules/* | İstatistikler uyarı kuralları oluşturma ve yönetme |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynakların kullanılabilirlik durumlarını alır |
> | Microsoft.Resources/deployments/* | Kaynak grubu dağıtımları oluşturma ve yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Security/* | Güvenlik bileşenleri ve ilkeleri oluşturma ve yönetme |
> | Microsoft.Support/* | Destek biletleri oluşturma ve yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
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

### <a name="security-reader"></a>Güvenlik Okuyucu

Önerileri ve uyarıları görüntüleyebilir, güvenlik ilkelerini görüntüleyebilir, güvenlik durumlarını görüntüleyebilir, ancak değişiklik yapamaz.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/oku | Rolleri ve rol ödevlerini okuma |
> | Microsoft.Insights/alertRules/* | İstatistikler uyarı kuralları oluşturma ve yönetme |
> | Microsoft.operationalInsights/çalışma alanları/*/oku | Günlük analizi verilerini görüntüleme |
> | Microsoft.Resources/deployments/* | Kaynak grubu dağıtımları oluşturma ve yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Security/*/oku | Güvenlik bileşenlerini ve ilkelerini okuyun |
> | Microsoft.Support/* | Destek biletleri oluşturma ve yönetme |
> | Microsoft.Management/managementGroups/read | Kimlik doğrulaması yapılan kullanıcı için liste yönetim grupları. |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
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


### <a name="devtest-labs-user"></a>Devtest Labs Kullanıcı

Azure DevTest Laboratuvarlarınızda sanal makinelerinizi bağlamanızı, başlatmanızı, yeniden başlatmanızı ve kapatmanızı sağlar.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/oku | Rolleri ve rol ödevlerini okuma |
> | Microsoft.Compute/availabilitySets/read | Kullanılabilirlik kümesinin özelliklerini alma |
> | Microsoft.Compute/virtualMachines/*/oku | Sanal makinenin özelliklerini okuyun (VM boyutları, çalışma süresi durumu, VM uzantıları, vb.) |
> | Microsoft.Compute/virtualMachines/deallocate/action | Sanal makineyi kapatır ve bilgi işlem kaynaklarını serbest bırakır |
> | Microsoft.Compute/virtualMachines/read | Sanal makinenin özelliklerini alın |
> | Microsoft.Compute/virtualMachines/restart/action | Sanal makineyi yeniden başlatır |
> | Microsoft.Compute/virtualMachines/start/action | Sanal makineyi başlatır |
> | Microsoft.DevTestLab/*/oku | Bir laboratuvarın özelliklerini okuyun |
> | Microsoft.DevTestLab/labs/claimAnyVm/action | Laboratuvarda rastgele talep edilebilir bir sanal makine talep edin. |
> | Microsoft.DevTestLab/labs/createEnvironment/action | Laboratuvarda sanal makineler oluşturun. |
> | Microsoft.DevTestLab/labs/ensureCurrentUserProfile/action | Geçerli kullanıcının laboratuarda geçerli bir profile sahip olduğundan emin olun. |
> | Microsoft.DevTestLab/labs/formüller/silme | Formülleri silin. |
> | Microsoft.DevTestLab/labs/formüller/okuma | Formülleri okuyun. |
> | Microsoft.DevTestLab/labs/formulas/write | Formüller ekleyin veya değiştirin. |
> | Microsoft.DevTestLab/labs/policySets/evaluatePolitikalar/eylem | Laboratuvar politikasını değerlendirir. |
> | Microsoft.DevTestLab/labs/virtualMachines/claim/action | Varolan bir sanal makinenin sahipliğini alma |
> | Microsoft.DevTestLab/labs/virtualmachines/listApplicableSchedules/action | Varsa, geçerli başlangıç/durdurma zamanlamalarını listeler. |
> | Microsoft.DevTestLab/labs/virtualMachines/getRdpFileContents/action | Sanal makine için RDP dosyasının içeriğini temsil eden bir dize alır |
> | Microsoft.Network/loadBalancers/backendAddressPools/join/action | Yük dengeleyici arka uç adres havuzuna katılır. Uyarılabilir değil. |
> | Microsoft.Network/loadBalancers/inboundNatRules/join/action | Yük dengeleyici gelen nat kuralına katılır. Uyarılabilir değil. |
> | Microsoft.Network/networkInterfaces/*/oku | Ağ arabiriminin özelliklerini okuyun (örneğin, ağ arabiriminin parçası olduğu tüm yük dengeleyicileri) |
> | Microsoft.Network/networkInterfaces/join/action | Sanal Makine'yi ağ arabirimine katılır. Uyarılabilir değil. |
> | Microsoft.Network/networkInterfaces/read | Ağ arabirimi tanımı nı alır.  |
> | Microsoft.Network/networkInterfaces/write | Bir ağ arabirimi oluşturur veya varolan bir ağ arabirimini güncelleştirir.  |
> | Microsoft.Network/publicIPAddresses/*/oku | Genel BIR IP adresinin özelliklerini okuyun |
> | Microsoft.Network/publicIPAddresses/join/action | Herkese açık bir ip adresine katılır. Uyarılabilir değil. |
> | Microsoft.Network/publicIPAddresses/read | Genel ip adresi tanımı nı alır. |
> | Microsoft.Network/virtualNetworks/subnets/join/action | Sanal ağa katılır. Uyarılabilir değil. |
> | Microsoft.Resources/deployments/operations/read | Dağıtım işlemlerini alır veya listeler. |
> | Microsoft.Resources/deployments/read | Dağıtımları alır veya listeler. |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Storage/storageAccounts/listKeys/action | Belirtilen depolama hesabının erişim anahtarlarını döndürür. |
> | **NotActions** |  |
> | Microsoft.Compute/virtualMachines/vmSizes/read | Kullanılabilir boyutları listeler sanal makine güncellenebilir |
> | **DataActions** |  |
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

### <a name="lab-creator"></a>Laboratuvar Oluşturucusu

Azure Laboratuvar Hesaplarınız altında yönetilen laboratuvarlarınızı oluşturmanıza, yönetmenize ve silmenize olanak tanır.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/oku | Rolleri ve rol ödevlerini okuma |
> | Microsoft.LabServices/labAccounts/*/oku |  |
> | Microsoft.LabServices/labAccounts/createLab/action | Laboratuvar hesabında bir laboratuvar oluşturun. |
> | Microsoft.LabServices/labAccounts/sizes/getRegionalAvailability/action |  |
> | Microsoft.LabServices/labAccounts/getRegionalAvailability/action | Laboratuvar hesabı altında yapılandırılan her boyut kategorisi için bölgesel kullanılabilirlik bilgilerini alın |
> | Microsoft.LabServices/labAccounts/getPricingAndAvailability/action | Laboratuvar hesabı için boyutlar, coğrafyalar ve işletim sistemlerinin kombinasyonlarının fiyatlandırılmasını ve kullanılabilirliğini öğrenin. |
> | Microsoft.LabServices/labAccounts/getRestrictionsAndUsage/action | Bu abonelik için temel kısıtlamaları ve kullanımı alın |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Destek biletleri oluşturma ve yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
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


### <a name="application-insights-component-contributor"></a>Uygulama Öngörüleri Bileşen Katılımcısı

Uygulama Öngörüleri bileşenlerini yönetebilir

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/oku | Rolleri ve rol ödevlerini okuma |
> | Microsoft.Insights/alertRules/* | Klasik uyarı kuralları oluşturma ve yönetme |
> | Microsoft.Insights/metricAlerts/* | Yeni uyarı kuralları oluşturma ve yönetme |
> | Microsoft.Insights/components/* | Öngörüler bileşenleri oluşturma ve yönetme |
> | Microsoft.Insights/webtests/* | Insights web testlerini oluşturma ve yönetme |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynakların kullanılabilirlik durumlarını alır |
> | Microsoft.Resources/deployments/* | Kaynak grubu dağıtımları oluşturma ve yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Destek biletleri oluşturma ve yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
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

### <a name="application-insights-snapshot-debugger"></a>Uygulama Öngörüleri Anlık Görüntü Hata Ayıkcısı

Kullanıcıya Application Insights Snapshot Debugger ile toplanan hata ayıklama anlık görüntülerini görüntüleme ve indirme izni verir. Bu izinlerin [Sahibi](#owner) veya [Katılımcı](#contributor) rollerine dahil olmadığını unutmayın.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/oku | Rolleri ve rol ödevlerini okuma |
> | Microsoft.Insights/alertRules/* | İstatistikler uyarı kuralları oluşturma ve yönetme |
> | Microsoft.Insights/components/*/read |  |
> | Microsoft.Resources/deployments/* | Kaynak grubu dağıtımları oluşturma ve yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Destek biletleri oluşturma ve yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
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

### <a name="monitoring-contributor"></a>Katılımcıyı İzleme

Tüm izleme verilerini okuyabilir ve izleme ayarlarını düzeltebilir. Ayrıca [Azure Monitor ile roller, izinler ve güvenlik le başlayın.](https://docs.microsoft.com/azure/azure-monitor/platform/roles-permissions-security#built-in-monitoring-roles)

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | */okumak | Sırlar hariç her türlü kaynağı okuyun. |
> | Microsoft.AlertsManagement/alerts/* |  |
> | Microsoft.AlertsManagement/alertsSummary/* |  |
> | Microsoft.Insights/actiongroups/* |  |
> | Microsoft.Insights/activityLogAlerts/* |  |
> | Microsoft.Insights/AlertRules/* | İstatistikler uyarı kuralları oluşturma ve yönetme |
> | Microsoft.Insights/components/* | Öngörüler bileşenleri oluşturma ve yönetme |
> | Microsoft.Insights/DiagnosticSettings/* | Analysis Server için tanılama ayarını oluşturur, günceller veya okur |
> | Microsoft.Insights/eventtypes/* | Bir abonelikteki Etkinlik Günlüğü olaylarını (yönetim olaylarını) listele. Bu izin, Etkinlik Günlüğü'ne hem programatik hem de portal erişimi için geçerlidir. |
> | Microsoft.Insights/LogDefinitions/* | Bu izin, portal üzerinden Etkinlik Günlükleri'ne erişmesi gereken kullanıcılar için gereklidir. Günlük kategorilerini Etkinlik Günlüğü'nde listeleyin. |
> | Microsoft.Insights/metricalerts/* |  |
> | Microsoft.Insights/MetricDefinitions/* | Metrik tanımları okuyun (kaynak için kullanılabilir metrik türlerinin listesi). |
> | Microsoft.Insights/Metrics/* | Kaynak için ölçümleri okuyun. |
> | Microsoft.Insights/Register/Action | Microsoft Insights sağlayıcısını kaydedin |
> | Microsoft.Insights/scheduledqueryrules/* |  |
> | Microsoft.Insights/webtests/* | Insights web testlerini oluşturma ve yönetme |
> | Microsoft.Insights/çalışma kitapları/* |  |
> | Microsoft.OperationalInsights/çalışma alanları/intelligencepacks/* | Günlük analizi çözüm paketlerini okuma/yazma/silme. |
> | Microsoft.OperationalInsights/çalışma alanları/savedSearches/* | Okuma/yazma/silme günlük analitiği aramaları kaydetti. |
> | Microsoft.OperationalInsights/çalışma alanları/arama/eylem | Arama sorgusunun yürütülmesi |
> | Microsoft.OperationalInsights/workspaces/sharedKeys/action | Çalışma alanı için paylaşılan anahtarları alır. Bu anahtarlar, Microsoft Operasyonel Öngörüler aracılarını çalışma alanına bağlamak için kullanılır. |
> | Microsoft.OperationalInsights/çalışma alanları/storageinsightconfigs/* | Günlük analizi depolama içgörü yapılandırmalarını okuma/yazma/silme yapılandırmaları. |
> | Microsoft.Support/* | Destek biletleri oluşturma ve yönetme |
> | Microsoft.WorkloadMonitor/monitörler/* |  |
> | Microsoft.WorkloadMonitor/notificationSettings/* |  |
> | Microsoft.AlertsManagement/smartDetectorAlertRules/* |  |
> | Microsoft.AlertsManagement/actionRules/* |  |
> | Microsoft.AlertsManagement/smartGroups/* |  |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
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

### <a name="monitoring-metrics-publisher"></a>İzleme Ölçümleri Yayıncı

Azure kaynaklarına karşı yayımlama ölçümleri sağlar

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.Insights/Register/Action | Microsoft Insights sağlayıcısını kaydedin |
> | Microsoft.Support/* | Destek biletleri oluşturma ve yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | Microsoft.Insights/Metrics/Write | Metrikleri yazma |
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

### <a name="monitoring-reader"></a>İzleme Okuyucu

Tüm izleme verilerini (ölçümler, günlükler, vb.) okuyabilir. Ayrıca [Azure Monitor ile roller, izinler ve güvenlik le başlayın.](https://docs.microsoft.com/azure/azure-monitor/platform/roles-permissions-security#built-in-monitoring-roles)

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | */okumak | Sırlar hariç her türlü kaynağı okuyun. |
> | Microsoft.OperationalInsights/çalışma alanları/arama/eylem | Arama sorgusunun yürütülmesi |
> | Microsoft.Support/* | Destek biletleri oluşturma ve yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
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

### <a name="workbook-contributor"></a>Çalışma Kitabı Katılımcısı

Paylaşılan çalışma kitaplarını kaydedebilirsiniz.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.Insights/çalışma kitapları/yazma | Çalışma kitabı oluşturma veya güncelleştirme |
> | Microsoft.Insights/çalışma kitapları/silme | Bir çalışma kitabını silme |
> | Microsoft.Insights/çalışma kitapları/okuma | Çalışma kitabını okuma |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
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

### <a name="workbook-reader"></a>Çalışma Kitabı Okuyucu

Çalışma kitaplarını okuyabilirsin.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | microsoft.insights/çalışma kitapları/oku | Çalışma kitabını okuma |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
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

## <a name="management--governance"></a>Yönetim + yönetim


### <a name="automation-job-operator"></a>Otomasyon İş Operatörü

Otomasyon Runbook'larını kullanarak İş Oluşturma ve Yönetme.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/oku | Rolleri ve rol ödevlerini okuma |
> | Microsoft.Automation/automationAccounts/hybridRunbookWorkerGroups/read | Hibrit Runbook İşçi Kaynaklarını Okur |
> | Microsoft.Automation/automationAccounts/jobs/read | Azure Otomasyonu işi alır |
> | Microsoft.Automation/automationAccounts/jobs/resume/action | Azure Otomasyonu işini devam ettirer |
> | Microsoft.Automation/automationAccounts/jobs/stop/action | Azure Otomasyonu işini durdurur |
> | Microsoft.Automation/automationAccounts/jobs/streams/read | Azure Otomasyoniş akışı na alır |
> | Microsoft.Automation/automationAccounts/jobs/suspend/action | Azure Otomasyonu işini askıya aldı |
> | Microsoft.Automation/automationAccounts/jobs/write | Azure Otomasyonu işi oluşturur |
> | Microsoft.Automation/automationAccounts/jobs/output/read | Bir işin çıktısını alır |
> | Microsoft.Insights/alertRules/* | İstatistikler uyarı kuralları oluşturma ve yönetme |
> | Microsoft.Resources/deployments/* | Kaynak grubu dağıtımları oluşturma ve yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Destek biletleri oluşturma ve yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
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

Otomasyon Operatörleri işleri başlatabilir, durdurabilir, askıya alabiliyor ve devam ettirebilir

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/oku | Rolleri ve rol ödevlerini okuma |
> | Microsoft.Automation/automationAccounts/hybridRunbookWorkerGroups/read | Hibrit Runbook İşçi Kaynaklarını Okur |
> | Microsoft.Automation/automationAccounts/jobs/read | Azure Otomasyonu işi alır |
> | Microsoft.Automation/automationAccounts/jobs/resume/action | Azure Otomasyonu işini devam ettirer |
> | Microsoft.Automation/automationAccounts/jobs/stop/action | Azure Otomasyonu işini durdurur |
> | Microsoft.Automation/automationAccounts/jobs/streams/read | Azure Otomasyoniş akışı na alır |
> | Microsoft.Automation/automationAccounts/jobs/suspend/action | Azure Otomasyonu işini askıya aldı |
> | Microsoft.Automation/automationAccounts/jobs/write | Azure Otomasyonu işi oluşturur |
> | Microsoft.Automation/automationAccounts/jobSchedules/read | Azure Otomasyonu iş zamanlamasını alır |
> | Microsoft.Automation/automationAccounts/jobSchedules/write | Azure Otomasyonu iş zamanlamasını oluşturur |
> | Microsoft.Automation/automationAccounts/linkedWorkspace/read | Çalışma alanını otomasyon hesabına bağlı hale getirin |
> | Microsoft.Automation/automationAccounts/read | Azure Otomasyon hesabı alır |
> | Microsoft.Automation/automationAccounts/runbooks/read | Azure Otomasyonu runbook'u alır |
> | Microsoft.Automation/automationAccounts/schedules/read | Azure Otomasyon zamanlama varlık alır |
> | Microsoft.Automation/automationAccounts/schedules/write | Azure Otomasyon zamanlama varlığı oluşturur veya güncelleştirir |
> | Microsoft.Insights/alertRules/* | İstatistikler uyarı kuralları oluşturma ve yönetme |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynakların kullanılabilirlik durumlarını alır |
> | Microsoft.Resources/deployments/* | Kaynak grubu dağıtımları oluşturma ve yönetme |
> | Microsoft.Automation/automationAccounts/jobs/output/read | Bir işin çıktısını alır |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Destek biletleri oluşturma ve yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
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

### <a name="automation-runbook-operator"></a>Otomasyon Runbook Operatörü

Runbook özelliklerini okuyun - runbook İşleri oluşturabilmek için.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/oku | Rolleri ve rol ödevlerini okuma |
> | Microsoft.Automation/automationAccounts/runbooks/read | Azure Otomasyonu runbook'u alır |
> | Microsoft.Insights/alertRules/* | İstatistikler uyarı kuralları oluşturma ve yönetme |
> | Microsoft.Resources/deployments/* | Kaynak grubu dağıtımları oluşturma ve yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Destek biletleri oluşturma ve yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
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

### <a name="azure-connected-machine-onboarding"></a>Azure Bağlı Makine Onboarding

Azure Bağlantılı Makineler'de kullanılabilir.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.HybridCompute/machines/read | Azure Arc makinelerini okuyun |
> | Microsoft.HybridCompute/machines/write | Azure Yay makineleri yazar |
> | Microsoft.GuestConfiguration/guestConfigurationAssignments/read | Konuk yapılandırma ataması alın. |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
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

### <a name="azure-connected-machine-resource-administrator"></a>Azure Bağlı Makine Kaynak Yöneticisi

Azure Bağlantılı Makineler'i okuyabilir, yazabilir, silebilir ve yeniden kullanabilir.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.HybridCompute/machines/read | Azure Arc makinelerini okuyun |
> | Microsoft.HybridCompute/machines/write | Azure Yay makineleri yazar |
> | Microsoft.HybridCompute/machines/delete | Azure Yay makinelerini siler |
> | Microsoft.HybridCompute/machines/reconnect/action | Azure Ark makinelerini yeniden bağlar |
> | Microsoft.HybridCompute/machines/extensions/write | Azure Ark uzantıları yükler veya güncelleştirir |
> | Microsoft.HybridCompute/*/oku |  |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
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
> | Microsoft.Authorization/*/oku | Rolleri ve rol ödevlerini okuma |
> | Microsoft.Faturalama/*/okuma | Faturalandırma bilgilerini okuma |
> | Microsoft.Commerce/*/oku |  |
> | Microsoft.Consumption/*/oku |  |
> | Microsoft.Management/managementGroups/read | Kimlik doğrulaması yapılan kullanıcı için liste yönetim grupları. |
> | Microsoft.CostManagement/*/oku |  |
> | Microsoft.Support/* | Destek biletleri oluşturma ve yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
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

### <a name="blueprint-contributor"></a>Plan Katılımcısı

Plan tanımlarını yönetebilir, ancak atamaz.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/oku | Rolleri ve rol ödevlerini okuma |
> | Microsoft.Blueprint/blueprints/* | Plan tanımları veya plan yapıları oluşturun ve yönetin. |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Resources/deployments/* | Kaynak grubu dağıtımları oluşturma ve yönetme |
> | Microsoft.Support/* | Destek biletleri oluşturma ve yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
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

### <a name="blueprint-operator"></a>Blueprint Operatörü

Varolan yayımlanmış planları atayabilir, ancak yeni planlar oluşturamaz. Bu yalnızca atama kullanıcı tarafından atanan yönetilen bir kimlikle yapılırsa işe yarar.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/oku | Rolleri ve rol ödevlerini okuma |
> | Microsoft.Blueprint/blueprintAtamaları/* | Plan atamaları oluşturun ve yönetin. |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Resources/deployments/* | Kaynak grubu dağıtımları oluşturma ve yönetme |
> | Microsoft.Support/* | Destek biletleri oluşturma ve yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
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

### <a name="cost-management-contributor"></a>Maliyet Yönetimi Katılımcısı

Maliyetleri görüntüleyebilir ve maliyet yapılandırmalarını yönetebilir (örn. bütçeler, dışa aktarma)

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.Consumption/* |  |
> | Microsoft.CostManagement/* |  |
> | Microsoft.Billing/billingDönemler/okuma |  |
> | Microsoft.Resources/abonelikler/okuma | Aboneliklistesini alır. |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Destek biletleri oluşturma ve yönetme |
> | Microsoft.Advisor/configurations/read | Yapılandırmaları alın |
> | Microsoft.Advisor/öneriler/okuma | Önerileri okur |
> | Microsoft.Management/managementGroups/read | Kimlik doğrulaması yapılan kullanıcı için liste yönetim grupları. |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
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

### <a name="cost-management-reader"></a>Maliyet Yönetimi Okuyucu

Maliyet verilerini ve yapılandırmayı görüntüleyebilir (örn. bütçeler, dışa aktarma)

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.Consumption/*/oku |  |
> | Microsoft.CostManagement/*/oku |  |
> | Microsoft.Billing/billingDönemler/okuma |  |
> | Microsoft.Resources/abonelikler/okuma | Aboneliklistesini alır. |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Destek biletleri oluşturma ve yönetme |
> | Microsoft.Advisor/configurations/read | Yapılandırmaları alın |
> | Microsoft.Advisor/öneriler/okuma | Önerileri okur |
> | Microsoft.Management/managementGroups/read | Kimlik doğrulaması yapılan kullanıcı için liste yönetim grupları. |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
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

### <a name="managed-application-contributor-role"></a>Yönetilen Uygulama Katılımcısıfatı Rolü

Yönetilen uygulama kaynakları oluşturmanızı sağlar.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | */okumak | Sırlar hariç her türlü kaynağı okuyun. |
> | Microsoft.Solutions/uygulamalar/* |  |
> | Microsoft.Solutions/register/action | Çözümlere Kaydolun. |
> | Microsoft.Resources/subscriptions/resourceGroups/* |  |
> | Microsoft.Resources/deployments/* | Kaynak grubu dağıtımları oluşturma ve yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
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

### <a name="managed-application-operator-role"></a>Yönetilen Uygulama Operatör Rolü

Yönetilen Uygulama kaynaklarında eylemleri okumanızı ve gerçekleştirmenizi sağlar

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | */okumak | Sırlar hariç her türlü kaynağı okuyun. |
> | Microsoft.Solutions/applications/read | Uygulamaların listesini alır. |
> | Microsoft.Solutions/*/action |  |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
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

### <a name="managed-applications-reader"></a>Yönetilen Uygulamalar Okuyucu

Yönetilen bir uygulamadaki kaynakları okumanızı ve JIT erişimi istemenizi sağlar.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | */okumak | Sırlar hariç her türlü kaynağı okuyun. |
> | Microsoft.Resources/deployments/* | Kaynak grubu dağıtımları oluşturma ve yönetme |
> | Microsoft.Solutions/jitRequests/* |  |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
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

### <a name="managed-services-registration-assignment-delete-role"></a>Yönetilen Hizmetler Kayıt atama Silme Rolü

Yönetilen Hizmetler Kayıt Atama Silme Rolü, yönetici kiracı kullanıcıların kiracılarına atanan kayıt atamasını silmelerine olanak tanır.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.ManagedServices/registrationAtamaları/okuma | Yönetilen Hizmetler kayıt atamalarının listesini alır. |
> | Microsoft.ManagedServices/registrationAtamaları/silme | Yönetilen Hizmetler kayıt ataması kaldırır. |
> | Microsoft.ManagedServices/operationStatuses/read | Kaynağın çalışma durumunu okur. |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
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

### <a name="management-group-contributor"></a>Yönetim Grubu Katılımcısı

Yönetim Grubu Katılımcı Rolü

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.Management/managementGroups/delete | Yönetim grubunu silin. |
> | Microsoft.Management/managementGroups/read | Kimlik doğrulaması yapılan kullanıcı için liste yönetim grupları. |
> | Microsoft.Management/managementGroups/subscriptions/delete | Yönetim grubundan aboneliği devre dışı bırakabilir. |
> | Microsoft.Management/managementGroups/subscriptions/write | Varolan aboneliği yönetim grubuyla ilişkilendirer. |
> | Microsoft.Management/managementGroups/write | Bir yönetim grubu oluşturun veya güncelleştirin. |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
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

### <a name="management-group-reader"></a>Yönetim Grubu Okuyucu

Yönetim Grubu Okuyucu Rolü

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.Management/managementGroups/read | Kimlik doğrulaması yapılan kullanıcı için liste yönetim grupları. |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
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

### <a name="new-relic-apm-account-contributor"></a>Yeni Emanet APM Hesap Katılımcısı

Yeni Emanet Uygulama Performans Yönetimi hesaplarını ve uygulamalarını yönetmenize olanak tanır, ancak bunlara erişmez.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/oku | Rolleri ve rol ödevlerini okuma |
> | Microsoft.Insights/alertRules/* | İstatistikler uyarı kuralları oluşturma ve yönetme |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynakların kullanılabilirlik durumlarını alır |
> | Microsoft.Resources/deployments/* | Kaynak grubu dağıtımları oluşturma ve yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Destek biletleri oluşturma ve yönetme |
> | NewRelic.APM/hesaplar/* |  |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
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

### <a name="policy-insights-data-writer-preview"></a>İlke Öngörüleri Veri Yazarı (Önizleme)

Kaynak ilkelerine okuma erişimi ve kaynak bileşeni ilkesi olaylarına yazma erişimi sağlar.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.Authorization/policyassignments/read | İlke ataması hakkında bilgi alın. |
> | Microsoft.Authorization/policydefinitions/read | İlke tanımı hakkında bilgi alın. |
> | Microsoft.Authorization/policysetdefinitions/read | İlke kümesi tanımı hakkında bilgi alın. |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
> | Microsoft.PolicyInsights/checkDataPolicyCompliance/action | Belirli bir bileşenin veri ilkelerine karşı uyumluluk durumunu denetleyin. |
> | Microsoft.PolicyInsights/policyEvents/logDataEvents/action | Kaynak bileşeni ilkesi olaylarını günlüğe kaydedin. |
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

Kaynak ilkesi oluşturma/değiştirme, destek bileti oluşturma ve kaynakları/hiyerarşiyi okuma hakları olan kullanıcılar.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | */okumak | Sırlar hariç her türlü kaynağı okuyun. |
> | Microsoft.Authorization/policyassignments/* | İlke atamaları oluşturma ve yönetme |
> | Microsoft.Authorization/policydefinitions/* | İlke tanımları oluşturma ve yönetme |
> | Microsoft.Authorization/policysetdefinitions/* | İlke kümeleri oluşturma ve yönetme |
> | Microsoft.PolicyInsights/* |  |
> | Microsoft.Support/* | Destek biletleri oluşturma ve yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
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

### <a name="site-recovery-contributor"></a>Site Kurtarma Katılımcısı

Kasa oluşturma ve rol ataması dışında Site Kurtarma hizmetini yönetmenize olanak tanır

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/oku | Rolleri ve rol ödevlerini okuma |
> | Microsoft.Insights/alertRules/* | İstatistikler uyarı kuralları oluşturma ve yönetme |
> | Microsoft.Network/virtualNetworks/read | Sanal ağ tanımını alma |
> | Microsoft.RecoveryServices/locations/allocatedStamp/read | GetAllocatedStamp hizmet tarafından kullanılan dahili işlemdir |
> | Microsoft.RecoveryServices/locations/allocateStamp/action | AllocateStamp hizmet tarafından kullanılan dahili işlemdir |
> | Microsoft.RecoveryServices/Vaults/certificates/write | Kaynak Sertifikasını Güncelleştir işlemi kaynak/kasa kimlik bilgilerini güncelleştirir. |
> | Microsoft.RecoveryServices/Vaults/extendedInformation/* | Kasa yla ilgili genişletilmiş bilgiler oluşturma ve yönetme |
> | Microsoft.RecoveryServices/Vaults/read | Vault'u Al işlemi, 'vault' türündeki Azure kaynağını temsil eden bir nesne alır |
> | Microsoft.RecoveryServices/Vaults/refreshContainers/read |  |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/* | Kayıtlı kimlikler oluşturma ve yönetme |
> | Microsoft.RecoveryServices/vaults/replicationAlertSettings/* | Çoğaltma uyarı ayarlarını oluşturma veya güncelleştirme |
> | Microsoft.RecoveryServices/vaults/replicationEvents/read | Tüm Etkinlikleri Okuyun |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/* | Çoğaltma kumaşları oluşturma ve yönetme |
> | Microsoft.RecoveryServices/vaults/replicationJobs/* | Çoğaltma işleri oluşturma ve yönetme |
> | Microsoft.RecoveryServices/vaults/replicationPolicies/* | Çoğaltma ilkeleri oluşturma ve yönetme |
> | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/* | Kurtarma planları oluşturma ve yönetme |
> | Microsoft.RecoveryServices/Vaults/storageConfig/* | Kurtarma Hizmetleri kasasının depolama yapılandırması oluşturma ve yönetme |
> | Microsoft.RecoveryServices/Vaults/tokenInfo/read |  |
> | Microsoft.RecoveryServices/Vaults/usages/read | Kurtarma Hizmetleri Kasası için kullanım ayrıntılarını verir. |
> | Microsoft.RecoveryServices/Vaults/vaultTokens/read | Vault Token işlemi vault seviyesi arka uç işlemleri için Vault Token almak için kullanılabilir. |
> | Microsoft.RecoveryServices/Vaults/monitoringAlerts/* | Kurtarma hizmetleri kasası için uyarıları okuyun |
> | Microsoft.RecoveryServices/Vaults/monitoringConfigurations/notificationConfiguration/read |  |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynakların kullanılabilirlik durumlarını alır |
> | Microsoft.Resources/deployments/* | Kaynak grubu dağıtımları oluşturma ve yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Storage/storageAccounts/read | Depolama hesapları listesini verir veya belirtilen depolama hesabının özelliklerini alır. |
> | Microsoft.RecoveryServices/vaults/replicationOperationStatus/read | Herhangi bir Vault Çoğaltma İşlemi Durumunu Okuyun |
> | Microsoft.Support/* | Destek biletleri oluşturma ve yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
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

### <a name="site-recovery-operator"></a>Site Kurtarma Operatörü

Başarısız ve başarısız ama diğer Site Kurtarma yönetimi işlemleri gerçekleştirmek değil sağlar

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/oku | Rolleri ve rol ödevlerini okuma |
> | Microsoft.Insights/alertRules/* | İstatistikler uyarı kuralları oluşturma ve yönetme |
> | Microsoft.Network/virtualNetworks/read | Sanal ağ tanımını alma |
> | Microsoft.RecoveryServices/locations/allocatedStamp/read | GetAllocatedStamp hizmet tarafından kullanılan dahili işlemdir |
> | Microsoft.RecoveryServices/locations/allocateStamp/action | AllocateStamp hizmet tarafından kullanılan dahili işlemdir |
> | Microsoft.RecoveryServices/Vaults/extendedInformation/read | Genişletilmiş Bilgi Al işlemi, bir nesnenin Azure türünün ?vault kaynağını temsil eden Genişletilmiş Bilgilerini alır? |
> | Microsoft.RecoveryServices/Vaults/read | Vault'u Al işlemi, 'vault' türündeki Azure kaynağını temsil eden bir nesne alır |
> | Microsoft.RecoveryServices/Vaults/refreshContainers/read |  |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read | İşlem Sonuçları Al işlemi, eşzamanlı olarak gönderilen işlem için işlem durumunu ve sonucunu almak için kullanılabilir |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/read | Get Containers işlemi, bir kaynak için kaydedilmiş kapları almak için kullanılabilir. |
> | Microsoft.RecoveryServices/vaults/replicationAlertSettings/read | Tüm Uyarılar Ayarlarını Okuyun |
> | Microsoft.RecoveryServices/vaults/replicationEvents/read | Tüm Etkinlikleri Okuyun |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/checkConsistency/action | Kumaşın Tutarlılığını Kontrol Eder |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/read | Herhangi bir Kumaş okuyun |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/reassociateGateway/action | Ağ Geçidini Yeniden Ilişkilendir |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/renewcertificate/action | Kumaş SertifikasıNı Yenile |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/read | Tüm Ağları Okuyun |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/replicationNetworkMappings/read | Ağ Eşlemelerini Okuyun |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/read | Herhangi bir Koruma Kapları okuyun |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectableItems/read | Herhangi bir Korunabilir Öğeyi Okuyun |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/applyRecoveryPoint/action | Kurtarma Noktası Uygula |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/failoverCommit/action | Failover Commit |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/plannedFailover/action | Planlı Yük Devretme |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/read | Korunan Öğeleri Okuyun |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/recoveryPoints/read | Herhangi bir Çoğaltma Kurtarma Noktalarını Okuyun |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/repairReplication/action | Onarım çoğaltma |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/reProtect/action | Korumalı Öğeyi Yeniden Koruma |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/switchprotection/action | Anahtar Koruma Konteyneri |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/testFailover/action | Test Yük Devretmesi |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/testFailoverCleanup/action | Test Failover Temizleme |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/unplansızFailover/action | Yük devretme |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/updateMobilityService/action | Mobilite Hizmetini Güncelle |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectionContainersMappings/read | Herhangi bir Koruma Konteyner Haritalamaları okuyun |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/read | Kurtarma Hizmetleri Sağlayıcıları'nı okuyun |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/refreshProvider/action | Sağlayıcıyı Yenile |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/read | Depolama Sınıflandırmalarını Okuyun |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/replicationStorageClassifications/replicationStorageClassificationMappings/read | Depolama Sınıflandırma Haritalarını okuyun |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationvCenters/read | Herhangi bir vCenters okuyun |
> | Microsoft.RecoveryServices/vaults/replicationJobs/* | Çoğaltma işleri oluşturma ve yönetme |
> | Microsoft.RecoveryServices/vaults/replicationPolicies/read | Tüm İlkeleri Okuyun |
> | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/failoverCommit/action | Failover Commit Kurtarma Planı |
> | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/plannedFailover/action | Planlı Başarısız Kurtarma Planı |
> | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/read | Kurtarma Planlarını Okuyun |
> | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/reProtect/action | Kurtarma Planını Yeniden Koruyun |
> | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/testFailover/action | Test Failover Kurtarma Planı |
> | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/testFailoverCleanup/action | Test Failover Temizleme Kurtarma Planı |
> | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/unplanolmayanFailover/action | Failover Kurtarma Planı |
> | Microsoft.RecoveryServices/Vaults/monitoringAlerts/* | Kurtarma hizmetleri kasası için uyarıları okuyun |
> | Microsoft.RecoveryServices/Vaults/monitoringConfigurations/notificationConfiguration/read |  |
> | Microsoft.RecoveryServices/Vaults/storageConfig/read |  |
> | Microsoft.RecoveryServices/Vaults/tokenInfo/read |  |
> | Microsoft.RecoveryServices/Vaults/usages/read | Kurtarma Hizmetleri Kasası için kullanım ayrıntılarını verir. |
> | Microsoft.RecoveryServices/Vaults/vaultTokens/read | Vault Token işlemi vault seviyesi arka uç işlemleri için Vault Token almak için kullanılabilir. |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynakların kullanılabilirlik durumlarını alır |
> | Microsoft.Resources/deployments/* | Kaynak grubu dağıtımları oluşturma ve yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Storage/storageAccounts/read | Depolama hesapları listesini verir veya belirtilen depolama hesabının özelliklerini alır. |
> | Microsoft.Support/* | Destek biletleri oluşturma ve yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
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

### <a name="site-recovery-reader"></a>Site Kurtarma Okuyucu

Site Kurtarma durumunu görüntülemenize izin verir, ancak diğer yönetim işlemlerini gerçekleştirmezsiniz

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/oku | Rolleri ve rol ödevlerini okuma |
> | Microsoft.RecoveryServices/locations/allocatedStamp/read | GetAllocatedStamp hizmet tarafından kullanılan dahili işlemdir |
> | Microsoft.RecoveryServices/Vaults/extendedInformation/read | Genişletilmiş Bilgi Al işlemi, bir nesnenin Azure türünün ?vault kaynağını temsil eden Genişletilmiş Bilgilerini alır? |
> | Microsoft.RecoveryServices/Vaults/monitoringAlerts/read | Kurtarma hizmetleri kasası için uyarıları alır. |
> | Microsoft.RecoveryServices/Vaults/monitoringConfigurations/notificationConfiguration/read |  |
> | Microsoft.RecoveryServices/Vaults/read | Vault'u Al işlemi, 'vault' türündeki Azure kaynağını temsil eden bir nesne alır |
> | Microsoft.RecoveryServices/Vaults/refreshContainers/read |  |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read | İşlem Sonuçları Al işlemi, eşzamanlı olarak gönderilen işlem için işlem durumunu ve sonucunu almak için kullanılabilir |
> | Microsoft.RecoveryServices/Vaults/registeredIdentities/read | Get Containers işlemi, bir kaynak için kaydedilmiş kapları almak için kullanılabilir. |
> | Microsoft.RecoveryServices/vaults/replicationAlertSettings/read | Tüm Uyarılar Ayarlarını Okuyun |
> | Microsoft.RecoveryServices/vaults/replicationEvents/read | Tüm Etkinlikleri Okuyun |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/read | Herhangi bir Kumaş okuyun |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/read | Tüm Ağları Okuyun |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/replicationNetworkMappings/read | Ağ Eşlemelerini Okuyun |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/read | Herhangi bir Koruma Kapları okuyun |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectableItems/read | Herhangi bir Korunabilir Öğeyi Okuyun |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/read | Korunan Öğeleri Okuyun |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/recoveryPoints/read | Herhangi bir Çoğaltma Kurtarma Noktalarını Okuyun |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectionContainersMappings/read | Herhangi bir Koruma Konteyner Haritalamaları okuyun |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/read | Kurtarma Hizmetleri Sağlayıcıları'nı okuyun |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/read | Depolama Sınıflandırmalarını Okuyun |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/replicationStorageClassifications/replicationStorageClassificationMappings/read | Depolama Sınıflandırma Haritalarını okuyun |
> | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationvCenters/read | Herhangi bir vCenters okuyun |
> | Microsoft.RecoveryServices/vaults/replicationİşler/okuma | İş İllerini Okuyun |
> | Microsoft.RecoveryServices/vaults/replicationPolicies/read | Tüm İlkeleri Okuyun |
> | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/read | Kurtarma Planlarını Okuyun |
> | Microsoft.RecoveryServices/Vaults/storageConfig/read |  |
> | Microsoft.RecoveryServices/Vaults/tokenInfo/read |  |
> | Microsoft.RecoveryServices/Vaults/usages/read | Kurtarma Hizmetleri Kasası için kullanım ayrıntılarını verir. |
> | Microsoft.RecoveryServices/Vaults/vaultTokens/read | Vault Token işlemi vault seviyesi arka uç işlemleri için Vault Token almak için kullanılabilir. |
> | Microsoft.Support/* | Destek biletleri oluşturma ve yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
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

### <a name="support-request-contributor"></a>Destek İstek Katılımcısı

Destek istekleri oluşturmanıza ve yönetmenize olanak tanır

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/oku | Rolleri ve rol ödevlerini okuma |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Destek biletleri oluşturma ve yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
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

### <a name="tag-contributor"></a>Tag Katılımcı

Varlıkların kendilerine erişim sağlamadan varlıklar üzerindeki etiketleri yönetmenize olanak tanır.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/oku | Rolleri ve rol ödevlerini okuma |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Resources/subscriptions/resourceGroups/resources/read | Kaynak grubu için kaynakları alır. |
> | Microsoft.Resources/subscriptions/resources/read | Aboneliğin kaynaklarını alır. |
> | Microsoft.Resources/deployments/* | Kaynak grubu dağıtımları oluşturma ve yönetme |
> | Microsoft.Insights/alertRules/* | İstatistikler uyarı kuralları oluşturma ve yönetme |
> | Microsoft.Support/* | Destek biletleri oluşturma ve yönetme |
> | Microsoft.Resources/tags/* |  |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
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

BizTalk hizmetlerini yönetmenize olanak tanır, ancak bunlara erişmemenize olanak tanır.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/oku | Rolleri ve rol ödevlerini okuma |
> | Microsoft.BizTalkServices/BizTalk/* | BizTalk hizmetlerini oluşturun ve yönetin |
> | Microsoft.Insights/alertRules/* | İstatistikler uyarı kuralları oluşturma ve yönetme |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynakların kullanılabilirlik durumlarını alır |
> | Microsoft.Resources/deployments/* | Kaynak grubu dağıtımları oluşturma ve yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Support/* | Destek biletleri oluşturma ve yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
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

### <a name="scheduler-job-collections-contributor"></a>Zamanlayıcı İş Koleksiyonları Katılımcı

Zamanlayıcı iş koleksiyonlarını yönetmenize olanak tanır, ancak bunlara erişmez.

> [!div class="mx-tableFixed"]
> |  |  |
> | --- | --- |
> | **Eylemler** |  |
> | Microsoft.Authorization/*/oku | Rolleri ve rol ödevlerini okuma |
> | Microsoft.Insights/alertRules/* | İstatistikler uyarı kuralları oluşturma ve yönetme |
> | Microsoft.ResourceHealth/availabilityStatuses/read | Belirtilen kapsamdaki tüm kaynakların kullanılabilirlik durumlarını alır |
> | Microsoft.Resources/deployments/* | Kaynak grubu dağıtımları oluşturma ve yönetme |
> | Microsoft.Resources/subscriptions/resourceGroups/read | Kaynak gruplarını alır veya listeler. |
> | Microsoft.Scheduler/iş koleksiyonları/* | İş koleksiyonları oluşturma ve yönetme |
> | Microsoft.Support/* | Destek biletleri oluşturma ve yönetme |
> | **NotActions** |  |
> | *yok* |  |
> | **DataActions** |  |
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

- [Kaynak sağlayıcısını hizmetle eşleştirme](../azure-resource-manager/management/azure-services-resource-providers.md)
- [Azure özel rolleri](custom-roles.md)
- [Azure Güvenlik Merkezi'nde İzinler](../security-center/security-center-permissions.md)
