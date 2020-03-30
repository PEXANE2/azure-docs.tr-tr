---
title: API Yönetimi'nde yedekleme ve geri yükleme kullanarak olağanüstü durum kurtarma uygulama
titleSuffix: Azure API Management
description: Azure API Yönetimi'nde olağanüstü durum kurtarma gerçekleştirmek için yedeklemeyi ve geri yüklemeyi nasıl kullanacağınızı öğrenin.
services: api-management
documentationcenter: ''
author: mikebudzynski
manager: erikre
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 02/03/2020
ms.author: apimpm
ms.openlocfilehash: f9758678708338a284a35b45f7f9dd43b9a9017c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80335951"
---
# <a name="how-to-implement-disaster-recovery-using-service-backup-and-restore-in-azure-api-management"></a>Azure API Management'ta hizmet yedekleme ve geri yükleme işlevlerini kullanarak acil durumda kurtarma

API'lerinizi Azure API Yönetimi aracılığıyla yayımlayarak ve yöneterek, aksi takdirde el ile tasarladığınız, uygulayacağınız ve yöneteceğimiz hata toleransı ve altyapı özelliklerinden yararlanırsınız. Azure platformu, maliyetin çok küçük bir kısmında ki olası hataları azaltır.

API Yönetimi hizmetinizi barındıran bölgeyi etkileyen kullanılabilirlik sorunlarını atlatmak için, hizmetinizi herhangi bir zamanda başka bir bölgede yeniden oluşturmaya hazır olun. Kurtarma süresi amacınıza bağlı olarak, bir veya daha fazla bölgede bekleme hizmeti tutmak isteyebilirsiniz. Ayrıca, yapılandırmalarını ve içeriklerini kurtarma noktası hedefinize göre etkin hizmetle eşit olarak sürdürmeyi de deneyebilirsiniz. Hizmet yedekleme ve geri yükleme özellikleri, olağanüstü durum kurtarma stratejisini uygulamak için gerekli yapı taşlarını sağlar.

Yedekleme ve geri yükleme işlemleri, API Yönetimi hizmet yapılandırması gibi geliştirme ve evreleme gibi operasyonel ortamlar arasında çoğaltılması için de kullanılabilir. Kullanıcılar ve abonelikler gibi çalışma zamanı verilerinin de kopyalanacağına dikkat edin, ki bu her zaman islemeyebilir.

Bu kılavuz, yedekleme ve geri yükleme işlemlerini nasıl otomatikleştirip geri yükleyeceklerini ve Azure Kaynak Yöneticisi tarafından yedekleme ve geri yükleme isteklerinin başarılı bir şekilde doğrulamasını nasıl sağlayacağını gösterir.

> [!IMPORTANT]
> Geri yükleme işlemi, hedef hizmetin özel ana bilgisayar adı yapılandırmasını değiştirmez. Hem etkin hem de bekleme hizmetleri için aynı özel ana bilgisayar adı ve TLS sertifikasını kullanmanızı öneririz, böylece geri yükleme işlemi tamamlandıktan sonra trafiğin basit bir DNS CNAME değişikliği yle bekleme örneğine yeniden yönlendirilen.
>
> Yedekleme işlemi, Azure portalında Analytics bıçağında gösterilen raporlarda kullanılan önceden toplanmış günlük verilerini yakalamaz.

> [!WARNING]
> Her yedeklemenin süresi 30 gün sonra doluyor. 30 günlük son kullanma süresi dolduktan sonra yedeklemeyi geri yüklemeyi `Cannot restore: backup expired` denerseniz, geri yükleme bir iletiyle birlikte başarısız olur.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]

## <a name="authenticating-azure-resource-manager-requests"></a>Azure Kaynak Yöneticisi isteklerini doğrulama

> [!IMPORTANT]
> Yedekleme ve geri yükleme için REST API'si Azure Kaynak Yöneticisi'ni kullanır ve API Yönetimi varlıklarınızı yönetmek için REST API'lerden farklı bir kimlik doğrulama mekanizmasına sahiptir. Bu bölümdeki adımlar, Azure Kaynak Yöneticisi isteklerinin nasıl doğrulaşdırılabildiğini açıklar. Daha fazla bilgi için Bkz. [Azure Kaynak Yöneticisi isteklerini kimlik doğrulama.](/rest/api/index)

Azure Kaynak Yöneticisi'ni kullanarak kaynaklarda yaptığınız tüm görevlerin, aşağıdaki adımları kullanarak Azure Etkin Dizini ile doğrulanması gerekir:

-   Azure Etkin Dizin kiracısına bir uygulama ekleyin.
-   Eklediğiniz uygulama için izinleri ayarlayın.
-   İsteklerin kimlik doğrulamasını Azure Kaynak Yöneticisi'ne taşıyın.

### <a name="create-an-azure-active-directory-application"></a>Azure Etkin Dizin uygulaması oluşturma

1. [Azure portalında](https://portal.azure.com)oturum açın.
2. API Yönetimi hizmet örneğini içeren aboneliği kullanarak Azure Etkin **Dizini'ndeki** **Uygulama kayıtları** sekmesine (Azure Active Directory > Manage/App kayıtları) gidin.

    > [!NOTE]
    > Azure Etkin Dizin varsayılan dizini hesabınızda görülemıyorsa, hesabınıza gerekli izinleri vermek için Azure aboneliğinin yöneticisine başvurun.

3. **Yeni uygulama kaydı**’na tıklayın.

    **Create** penceresi sağda görünür. AAD uygulamasıyla ilgili bilgileri girdiğiniz yer orasıdır.

4. Uygulama için bir ad girin.
5. Uygulama türü için **Yerel'i**seçin.
6. Gerekli bir alan olduğu `http://resources` için **Uri'yi Yönlendirme**gibi bir yer tutucu URL'si girin, ancak değer daha sonra kullanılmaz. Uygulamayı kaydetmek için onay kutusunu tıklatın.
7. **Oluştur'u**tıklatın.

### <a name="add-an-application"></a>Uygulama ekleme

1. Uygulama oluşturulduktan sonra **API izinlerini**tıklatın.
2. + **İzin ekle'yi**tıklatın.
4. **Microsoft API'lerini seçin'** e basın.
5. **Azure Hizmet Yönetimi'ni**seçin.
6. **Seç**tuşuna basın.

    ![İzin ekleme](./media/api-management-howto-disaster-recovery-backup-restore/add-app.png)

7. Yeni eklenen uygulamanın yanında **Temsilciler İzni'ni** tıklatın, **Access Azure Hizmet Yönetimi (önizleme)** için kutuyu işaretleyin.
8. **Seç**tuşuna basın.
9. **Hibe İzinleri'ni**tıklatın.

### <a name="configuring-your-app"></a>Uygulamanızı yapılandırma

Yedeklemeyi oluşturan ve geri yükleyen API'leri aramadan önce bir belirteç almanız gerekir. Aşağıdaki örnekte, belirteci almak için [Microsoft.IdentityModel.Clients.ActiveDirectory](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory) NuGet paketini kullanır.

```csharp
using Microsoft.IdentityModel.Clients.ActiveDirectory;
using System;

namespace GetTokenResourceManagerRequests
{
    class Program
    {
        static void Main(string[] args)
        {
            var authenticationContext = new AuthenticationContext("https://login.microsoftonline.com/{tenant id}");
            var result = authenticationContext.AcquireTokenAsync("https://management.azure.com/", "{application id}", new Uri("{redirect uri}"), new PlatformParameters(PromptBehavior.Auto)).Result;

            if (result == null) {
                throw new InvalidOperationException("Failed to obtain the JWT token");
            }

            Console.WriteLine(result.AccessToken);

            Console.ReadLine();
        }
    }
}
```

Değiştir `{tenant id}` `{application id}`, `{redirect uri}` ve aşağıdaki yönergeleri kullanarak:

1. Oluşturduğunuz Azure Etkin Dizin uygulamasının kiracı kimliğiyle değiştirin. `{tenant id}` **Uygulama kayıtları** -> **Bitiş Noktaları'na**tıklayarak kimlik erişebilirsiniz.

    ![Uç Noktalar][api-management-endpoint]

2. `{application id}` **Ayarlar** sayfasına yönlendirerek aldığınız değerle değiştirin.
3. `{redirect uri}` Azure Active Directory uygulamanızın **Yönlendirme URL'leri** sekmesinden değeri değiştirin.

    Değerler belirtildikten sonra, kod örneği aşağıdaki örneğe benzer bir belirteç döndürmelidir:

    ![Belirteç][api-management-arm-token]

    > [!NOTE]
    > Belirteç belirli bir süre sonra sona erebilir. Yeni bir belirteç oluşturmak için kod örneğini yeniden yürütün.

## <a name="calling-the-backup-and-restore-operations"></a>Yedekleme ve geri yükleme işlemlerini çağırma

REST API'ler [Api Yönetim Hizmeti - Yedekleme](/rest/api/apimanagement/2019-01-01/apimanagementservice/backup) ve [Api Yönetim Hizmeti - Geri Yükleyin.](/rest/api/apimanagement/2019-01-01/apimanagementservice/restore)

Aşağıdaki bölümlerde açıklanan "yedekleme ve geri yükleme" işlemlerini çağırmadan önce, REST aramanız için yetkilendirme isteği üstbilgisini ayarlayın.

```csharp
request.Headers.Add(HttpRequestHeader.Authorization, "Bearer " + token);
```

### <a name="back-up-an-api-management-service"></a><a name="step1"> </a>API Yönetimi hizmetini yedekleme

Bir API Yönetimi hizmeti sorununu yedeklemek için aşağıdaki HTTP isteği:

```http
POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ApiManagement/service/{serviceName}/backup?api-version={api-version}
```

burada:

-   `subscriptionId`- Yedeklemeye çalıştığınız API Yönetimi hizmetini tutan aboneliğin kimliği
-   `resourceGroupName`- Azure API Yönetimi hizmetinizin kaynak grubunun adı
-   `serviceName`- oluşturulduğu anda belirtilen bir yedekleme yapıyoruz API Yönetimi hizmetinin adı
-   `api-version`- ile değiştirin`2018-06-01-preview`

İsteğin gövdesinde, hedef Azure depolama hesabı adını, erişim anahtarını, blob kapsayıcı adını ve yedek adı belirtin:

```json
{
    "storageAccount": "{storage account name for the backup}",
    "accessKey": "{access key for the account}",
    "containerName": "{backup container name}",
    "backupName": "{backup blob name}"
}
```

İstek üstbilgisinin `Content-Type` değerini ' `application/json`olarak ayarlayın

Yedekleme, tamamlanması bir dakikadan fazla sürebilir uzun süren bir işlemdir. İstek başarılı olduysa ve yedekleme işlemi başladıysa, üstbilgi `Location` içeren bir `202 Accepted` yanıt durum kodu alırsınız. İşlemin durumunu öğrenmek için `Location` üstbilgideki URL'ye 'GET' isteklerinde bulunun. Yedekleme devam ederken, '202 Kabul Edilenler' durum kodunu almaya devam egenelolarak devam egenelolarak. Yanıt kodu, `200 OK` yedekleme işleminin başarıyla tamamlanır.

Yedekleme veya geri yükleme isteği yaparken aşağıdaki kısıtlamalara dikkat edin:

-   İstek gövdesinde belirtilen **kapsayıcı** **nın bulunması gerekir.**
-   Yedekleme devam ederken, SKU yükseltmeveya düşürme, etki alanı adı değişikliği ve daha fazlası gibi **hizmetteki yönetim değişikliklerinden kaçının.**
-   Yedeklemenin geri yüklenmesi, oluşturuldurulmasdan bu yana **yalnızca 30 gün boyunca garanti edilir.**
-   Analitik raporları oluşturmak için kullanılan **kullanım verileri** yedeklemeye **dahil değildir.** Güvenlik sağlamak için analitik raporları düzenli aralıklarla almak için [Azure API Yönetimi REST API'sini][azure api management rest api] kullanın.
-   Buna ek olarak, aşağıdaki öğeler yedekleme verilerinin bir parçası değildir: özel etki alanı TLS/SSL sertifikaları ve müşteri, geliştirici portalı içeriği ve sanal ağ tümleştirme ayarları tarafından yüklenen ara veya kök sertifikalar.
-   Hizmet yedeklemelerini gerçekleştirme sıklığı, kurtarma noktası hedefinizi etkiler. En aza indirmek için, API Yönetimi hizmetinizde değişiklik yaptıktan sonra düzenli yedeklemeuygulamanızı ve isteğe bağlı yedeklemeler gerçekleştirmenizi öneririz.
-   Yedekleme işlemi devam ederken hizmet yapılandırmasında yapılan **değişiklikler** (örneğin, API'ler, ilkeler ve geliştirici portalı görünümü) **yedekleme nin dışında tutulabilir ve kaybedilebilir.**
-   [Güvenlik duvarı][azure-storage-ip-firewall] etkinse denetim düzleminden Azure Depolama Hesabı'na erişime **izin verin.** Müşteri, Yedekleme veya Geri Yükleme için Depolama Hesabında [Azure API Yönetim Denetimi Düzlemi IP Adresleri][control-plane-ip-address] kümesini açmalıdır. 

> [!NOTE]
> Aynı Azure Bölgesinde [güvenlik duvarı][azure-storage-ip-firewall] etkinleştirilmiş bir depolama hesabı kullanarak bir API Yönetimi hizmetine yedekleme/geri yükleme yapmaya çalışırsanız, bu çalışmaz. Bunun nedeni, Azure Depolama'ya yönelik isteklerin Bilgi işlem > (Azure Api Yönetimi denetim Düzlemi) tarafından ortak bir IP'ye sayılmamamasıdır. Bölgeler Arası depolama isteği kesilecektir.

### <a name="restore-an-api-management-service"></a><a name="step2"> </a>API Yönetimi hizmetini geri yükleme

Daha önce oluşturulmuş bir yedeklemeden bir API Yönetimi hizmetini geri yüklemek için aşağıdaki HTTP isteğini yapın:

```http
POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ApiManagement/service/{serviceName}/restore?api-version={api-version}
```

burada:

-   `subscriptionId`- Yedeklemeyi geri aldığınız API Yönetimi hizmetini elinde bulunduran aboneliğin kimliği
-   `resourceGroupName`- Yedeklemeyi geri dönüştüren Azure API Yönetimi hizmetini elinde bulunduran kaynak grubunun adı
-   `serviceName`- API Yönetimi hizmetinin adı, oluşturma zamanında belirtilen şekilde geri yükleniyor
-   `api-version`- ile değiştirin`2018-06-01-preview`

İsteğin gövdesinde yedek dosya konumunu belirtin. Diğer bir şekilde, Azure depolama hesabı adı, erişim anahtarı, blob kapsayıcı adı ve yedek adı ekleyin:

```json
{
    "storageAccount": "{storage account name for the backup}",
    "accessKey": "{access key for the account}",
    "containerName": "{backup container name}",
    "backupName": "{backup blob name}"
}
```

İstek üstbilgisinin `Content-Type` değerini ' `application/json`olarak ayarlayın

Geri yükleme, tamamlanması 30 dakika veya daha uzun sürebilir uzun süren bir işlemdir. İstek başarılı olduysa ve geri yükleme işlemi `202 Accepted` başladıysa, `Location` üstbilgi içeren bir yanıt durum kodu alırsınız. İşlemin durumunu öğrenmek için `Location` üstbilgideki URL'ye 'GET' isteklerinde bulunun. Geri yükleme devam ederken, '202 Kabul Edildi' durum kodunu almaya devam egenelolarak. Yanıt kodu, `200 OK` geri yükleme işleminin başarıyla tamamlanır.

> [!IMPORTANT]
> Geri yüklenen hizmetin **SKU'su,** geri yüklenen hizmetin SKU'suyla **eşleşmelidir.**
>
> Geri yükleme işlemi devam ederken hizmet yapılandırmasında yapılan **değişiklikler** (örneğin, API'ler, ilkeler, geliştirici portalı görünümü) **üzerine yazılabilir.**

<!-- Dummy comment added to suppress markdown lint warning -->

> [!NOTE]
> Yedekleme ve geri yükleme işlemleri sırasıyla PowerShell [_Backup-AzApiManagement_](/powershell/module/az.apimanagement/backup-azapimanagement) ve [_Restore-AzApiManagement_](/powershell/module/az.apimanagement/restore-azapimanagement) komutları ile de gerçekleştirilebilir.

## <a name="next-steps"></a>Sonraki adımlar

Yedekleme/geri yükleme işleminin farklı izlikleri için aşağıdaki kaynaklara göz atın.

-   [Azure API Yönetim Hesaplarını Çoğaltma](https://www.returngis.net/en/2015/06/replicate-azure-api-management-accounts/)
-   [Logic Apps ile API Management Yedekleme ve Geri Yükleme İşlemlerini Otomatikleştirme](https://github.com/Azure/api-management-samples/tree/master/tutorials/automating-apim-backup-restore-with-logic-apps)
-   [Azure API Yönetimi: Yedekleme ve Yapılandırmayı](https://blogs.msdn.com/b/stuartleeks/archive/2015/04/29/azure-api-management-backing-up-and-restoring-configuration.aspx)
    Geri Alma_Stuart tarafından ayrıntılı olarak açıklanan yaklaşım resmi kılavuzla eşleşmez, ancak ilginçtir._

[backup an api management service]: #step1
[restore an api management service]: #step2
[azure api management rest api]: https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/api-management-rest
[api-management-add-aad-application]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-add-aad-application.png
[api-management-aad-permissions]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-permissions.png
[api-management-aad-permissions-add]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-permissions-add.png
[api-management-aad-delegated-permissions]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-delegated-permissions.png
[api-management-aad-default-directory]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-default-directory.png
[api-management-aad-resources]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-resources.png
[api-management-arm-token]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-arm-token.png
[api-management-endpoint]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-endpoint.png
[control-plane-ip-address]: api-management-using-with-vnet.md#control-plane-ips
[azure-storage-ip-firewall]: ../storage/common/storage-network-security.md#grant-access-from-an-internet-ip-range
