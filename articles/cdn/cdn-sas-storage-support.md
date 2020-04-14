---
title: Azure CDN'yi SAS ile kullanma | Microsoft Dokümanlar
description: Azure CDN, özel depolama kapsayıcılarına sınırlı erişim sağlamak için Paylaşılan Erişim İmzası'nın (SAS) kullanımını destekler.
services: cdn
documentationcenter: ''
author: asudbring
manager: danielgi
editor: ''
ms.assetid: ''
ms.service: azure-cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/21/2018
ms.author: allensu
ms.openlocfilehash: c2580aa4ee22996c1bf0fe5c86064a6543450071
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81260182"
---
# <a name="using-azure-cdn-with-sas"></a>Azure CDN'yi SAS ile kullanma

İçeriği önbelleğe almak için Kullanmak üzere Azure İçerik Dağıtım Ağı (CDN) için bir depolama hesabı ayarladığınızda, varsayılan olarak depolama kapsayıcılarınızın URL'lerini bilen herkes yüklediğiniz dosyalara erişebilir. Depolama hesabınızdaki dosyaları korumak için, depolama kaplarınızın erişimini genelden özele ayarlayabilirsiniz. Ancak, bunu yaparsanız, hiç kimse dosyalarınıza erişemez. 

Özel depolama kapsayıcılarına sınırlı erişim vermek istiyorsanız, Azure depolama hesabınızın Paylaşılan Erişim İmzası (SAS) özelliğini kullanabilirsiniz. Bir SAS, hesap anahtarınızı açığa çıkarmadan Azure Depolama kaynaklarınıza kısıtlı erişim hakları veren bir URI'dir. Depolama hesabı anahtarınızla güvenmediğiniz ancak belirli depolama hesabı kaynaklarına erişimi devretmek istediğiniz istemcilere Bir SAS sağlayabilirsiniz. Bu istemcilere paylaşılan erişim imzası URI dağıtarak, onlara belirli bir süre için bir kaynağa erişim izni vermiş olursunuz.
 
SAS ile, başlangıç ve son kullanma saatleri, izinler (okuma/yazma) ve IP aralıkları gibi bir blob'a erişimin çeşitli parametrelerini tanımlayabilirsiniz. Bu makalede, Azure CDN ile birlikte SAS nasıl kullanılacağı açıklanmaktadır. SAS hakkında, nasıl oluşturulacak ve parametre seçenekleri de dahil olmak üzere daha fazla bilgi için [bkz.](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1)

## <a name="setting-up-azure-cdn-to-work-with-storage-sas"></a>Azure CDN'yi depolama alanı SAS ile çalışacak şekilde ayarlama
Azure CDN ile SAS kullanmak için aşağıdaki üç seçenek önerilir. Tüm seçenekler, zaten çalışan bir SAS oluşturduğunuzu varsayar (ön koşullara bakın). 
 
### <a name="prerequisites"></a>Ön koşullar
Başlamak için bir depolama hesabı oluşturun ve ardından varlığınız için bir SAS oluşturun. İki tür depolanmış erişim imzası oluşturabilirsiniz: bir hizmet SAS veya bir hesap SAS. Daha fazla bilgi için [bkz.](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1#types-of-shared-access-signatures)

Bir SAS belirteci oluşturduktan sonra, URL'nize ekleyerek `?sv=<SAS token>` blob depolama dosyanıza erişebilirsiniz. Bu URL aşağıdaki biçime sahiptir: 

`https://<account name>.blob.core.windows.net/<container>/<file>?sv=<SAS token>`
 
Örneğin:
 ```
https://democdnstorage1.blob.core.windows.net/container1/demo.jpg?sv=2017-07-29&ss=b&srt=co&sp=r&se=2038-01-02T21:30:49Z&st=2018-01-02T13:30:49Z&spr=https&sig=QehoetQFWUEd1lhU5iOMGrHBmE727xYAbKJl5ohSiWI%3D
```

Parametreleri ayarlama hakkında daha fazla bilgi için Bkz. [SAS parametre değerlendirmeleri](#sas-parameter-considerations) ve [Paylaşılan erişim imza parametreleri.](https://docs.microsoft.com/azure/storage/common/storage-sas-overview#how-a-shared-access-signature-works)

![CDN SAS ayarları](./media/cdn-sas-storage-support/cdn-sas-settings.png)

### <a name="option-1-using-sas-with-pass-through-to-blob-storage-from-azure-cdn"></a>Seçenek 1: Azure CDN'den geçişli blob depolama alanıyla SAS kullanma

Bu seçenek en basit seçenektir ve Azure CDN'den başlangıç sunucusuna geçirilen tek bir SAS belirteci kullanır.
 
1. Bir bitiş noktası seçin, **Önbelleğe Alma kurallarını**seçin ve ardından **Sorgu dizesi önbelleğe alma** listesindeki her benzersiz **URL'yi Önbelleğe** Alma'yı seçin.

    ![CDN önbelleğe alma kuralları](./media/cdn-sas-storage-support/cdn-caching-rules.png)

2. Depolama hesabınızda SAS'ı ayarladıktan sonra, dosyaya erişmek için CDN bitiş noktası ve başlangıç sunucusu URL'leri ile Birlikte SAS belirteci kullanmanız gerekir. 
   
   Ortaya çıkan CDN bitiş noktası URL'si aşağıdaki biçime sahiptir:`https://<endpoint hostname>.azureedge.net/<container>/<file>?sv=<SAS token>`

   Örneğin:   
   ```
   https://demoendpoint.azureedge.net/container1/demo.jpg/?sv=2017-07-29&ss=b&srt=c&sp=r&se=2027-12-19T17:35:58Z&st=2017-12-19T09:35:58Z&spr=https&sig=kquaXsAuCLXomN7R00b8CYM13UpDbAHcsRfGOW3Du1M%3D
   ```
   
3. Önbelleğe alma kurallarını kullanarak veya başlangıç sunucusuna `Cache-Control` üstbilgi ekleyerek önbellek süresine ince ayar sağlayın. Azure CDN, SAS belirteci'ni düz bir sorgu dizesi olarak ele aldığı için, en iyi yöntem olarak SAS'ın son kullanma süresinde veya bitiminden önce süresi dolan bir önbelleğe alma süresi ayarlamanız gerekir. Aksi takdirde, bir dosya SAS etkin olduğundan daha uzun bir süre önbelleğe alınmışsa, Dosyaya SAS son kullanma süresi dolduktan sonra Azure CDN başlangıç sunucusundan erişilebilir. Bu durum oluşursa ve önbelleğe alınan dosyanızı erişilemez hale getirmek istiyorsanız, önbellekten temizlemek için dosyaüzerinde bir temizleme işlemi gerçekleştirmeniz gerekir. Azure CDN'de önbellek süresini ayarlama hakkında bilgi için [önbelleğe alma kurallarıyla Azure CDN önbelleğe alma davranışını denetle'ye](cdn-caching-rules.md)bakın.

### <a name="option-2-hidden-cdn-sas-token-using-a-rewrite-rule"></a>Seçenek 2: Yeniden yazma kuralını kullanarak gizli CDN SAS belirteci
 
Bu seçenek yalnızca **Verizon profillerinden Azure CDN Premium** için kullanılabilir. Bu seçenekle, kaynak sunucusundaki blob depolama sını güvenli hale getirebilirsiniz. Dosya için belirli erişim kısıtlamalarına ihtiyacınız yoksa, ancak kullanıcıların Azure CDN boşaltma sürelerini iyileştirmek için depolama kaynağına doğrudan erişmesini engellemek istiyorsanız, bu seçeneği kullanmak isteyebilirsiniz. Kullanıcı tarafından bilinmeyen SAS belirteci, kaynak sunucunun belirtilen kapsayıcısındaki dosyalara erişen herkes için gereklidir. Ancak, URL Yeniden Yazma kuralı nedeniyle, CDN bitiş noktasında SAS belirteci gerekli değildir.
 
1. URL Yeniden Yazma kuralı oluşturmak için [kurallar altyapısını](cdn-rules-engine.md) kullanın. Yeni kuralların yayılması 4 saat kadar sürer.

   ![CDN Yönet düğmesi](./media/cdn-sas-storage-support/cdn-manage-btn.png)

   ![CDN kuralları motor düğmesi](./media/cdn-sas-storage-support/cdn-rules-engine-btn.png)

   Aşağıdaki örnek URL Yeniden Yazma kuralı, bir yakalama grubu ve *sasstoragedemo*adlı bir bitiş noktası ile düzenli bir ifade deseni kullanır:
   
   Kaynak:   
   `(container1\/.*)`
   
   Hedef:   
   ```
   $1?sv=2017-07-29&ss=b&srt=c&sp=r&se=2027-12-19T17:35:58Z&st=2017-12-19T09:35:58Z&spr=https&sig=kquaXsAuCLXomN7R00b8CYM13UpDbAHcsRfGOW3Du1M%3D
   ```
   ![CDN URL Yeniden Yazma](./media/cdn-sas-storage-support/cdn-url-rewrite-rule.png)
   ![kuralı - sol CDN URL Yeniden yazma kuralı - sağ](./media/cdn-sas-storage-support/cdn-url-rewrite-rule-option-4.png)

2. Yeni kural etkin hale geldikten sonra, URL'de SAS belirteci kullanıp kullanmadığına bakılmaksızın herkes CDN bitiş noktasındaki belirtilen kapsayıcıdaki dosyalara erişebilir. Burada biçimidir:`https://<endpoint hostname>.azureedge.net/<container>/<file>`
 
   Örneğin:   
   `https://sasstoragedemo.azureedge.net/container1/demo.jpg`
       

3. Önbelleğe alma kurallarını kullanarak veya başlangıç sunucusuna `Cache-Control` üstbilgi ekleyerek önbellek süresine ince ayar sağlayın. Azure CDN, SAS belirteci'ni düz bir sorgu dizesi olarak ele aldığı için, en iyi yöntem olarak SAS'ın son kullanma süresinde veya bitiminden önce süresi dolan bir önbelleğe alma süresi ayarlamanız gerekir. Aksi takdirde, bir dosya SAS etkin olduğundan daha uzun bir süre önbelleğe alınmışsa, Dosyaya SAS son kullanma süresi dolduktan sonra Azure CDN başlangıç sunucusundan erişilebilir. Bu durum oluşursa ve önbelleğe alınan dosyanızı erişilemez hale getirmek istiyorsanız, önbellekten temizlemek için dosyaüzerinde bir temizleme işlemi gerçekleştirmeniz gerekir. Azure CDN'de önbellek süresini ayarlama hakkında bilgi için [önbelleğe alma kurallarıyla Azure CDN önbelleğe alma davranışını denetle'ye](cdn-caching-rules.md)bakın.

### <a name="option-3-using-cdn-security-token-authentication-with-a-rewrite-rule"></a>Seçenek 3: CDN güvenlik belirteç kimlik doğrulamasını yeniden yazma kuralıyla kullanma

Azure CDN güvenlik belirteç kimlik doğrulamasını kullanmak için Verizon profilinden bir **Azure CDN Premium'una** sahip olmalısınız. Bu seçenek en güvenli ve özelleştirilebilir. İstemci erişimi, güvenlik belirteci üzerinde ayarladığınız güvenlik parametrelerini temel adatır. Güvenlik belirteci'ni oluşturduktan ve ayarladıktan sonra, tüm CDN uç nokta URL'lerinde gereklidir. Ancak, URL Yeniden Yazma kuralı nedeniyle, CDN bitiş noktasında SAS belirteci gerekli değildir. SAS belirteci daha sonra geçersiz hale gelirse, Azure CDN artık kaynağı sunucudaki içeriği yeniden geçersiz kılamaz.

1. [Bir Azure CDN güvenlik belirteci oluşturun](https://docs.microsoft.com/azure/cdn/cdn-token-auth#setting-up-token-authentication) ve kullanıcılarınızın dosyaya erişebileceği CDN bitiş noktası ve yolu için kurallar altyapısını kullanarak etkinleştirin.

   Güvenlik belirteci uç noktası URL'si aşağıdaki biçime sahiptir:   
   `https://<endpoint hostname>.azureedge.net/<container>/<file>?<security_token>`
 
   Örneğin:   
   ```
   https://sasstoragedemo.azureedge.net/container1/demo.jpg?a4fbc3710fd3449a7c99986bkquaXsAuCLXomN7R00b8CYM13UpDbAHcsRfGOW3Du1M%3D
   ```
       
   Güvenlik belirteci kimlik doğrulaması için parametre seçenekleri, SAS belirteci için parametre seçeneklerinden farklıdır. Bir güvenlik belirteci oluştururken bir son kullanma süresi kullanmayı seçerseniz, bunu SAS belirteci için son kullanma süresiyle aynı değere ayarlamanız gerekir. Bunu yapmak, son kullanma süresinin tahmin edilebilir olmasını sağlar. 
 
2. SAS'ın kapsayıcıdaki tüm lekelere erişim sağlamasını sağlamak için bir URL Yeniden Yazma kuralı oluşturmak için [kurallar altyapısını](cdn-rules-engine.md) kullanın. Yeni kuralların yayılması 4 saat kadar sürer.

   Aşağıdaki örnek URL Yeniden Yazma kuralı, bir yakalama grubu ve *sasstoragedemo*adlı bir bitiş noktası ile düzenli bir ifade deseni kullanır:
   
   Kaynak:   
   `(container1\/.*)`
   
   Hedef:   
   ```
   $1&sv=2017-07-29&ss=b&srt=c&sp=r&se=2027-12-19T17:35:58Z&st=2017-12-19T09:35:58Z&spr=https&sig=kquaXsAuCLXomN7R00b8CYM13UpDbAHcsRfGOW3Du1M%3D
   ```
   ![CDN URL Yeniden Yazma](./media/cdn-sas-storage-support/cdn-url-rewrite-rule.png)
   ![kuralı - sol CDN URL Yeniden yazma kuralı - sağ](./media/cdn-sas-storage-support/cdn-url-rewrite-rule-option-4.png)

3. SAS'yi yenilerseniz, Url Yeniden Yazma kuralını yeni SAS belirteciyle güncelleştirdiğinizden emin olun. 

## <a name="sas-parameter-considerations"></a>SAS parametre hususları

SAS parametreleri Azure CDN tarafından görülemediğinden, Azure CDN teslim davranışını bunlara göre değiştiremez. Tanımlanan parametre kısıtlamaları yalnızca Azure CDN'nin istemciden Azure CDN'ye gelen istekler için değil, başlangıç sunucusuna yaptığı istekler için geçerlidir. SAS parametrelerini ayarlarken göz önünde bulundurulması gereken bu ayrım önemlidir. Bu gelişmiş özellikler gerekiyorsa ve [Seçenek 3](#option-3-using-cdn-security-token-authentication-with-a-rewrite-rule)kullanıyorsanız, Azure CDN güvenlik belirteci üzerinde uygun kısıtlamaları ayarlayın.

| SAS parametre adı | Açıklama |
| --- | --- |
| Başlat | Azure CDN'nin blob dosyasına erişmeye başlayabileceğiniz süre. Saat çarpıklık nedeniyle (bir saat sinyali farklı bileşenler için farklı zamanlarda geldiğinde), varlığın hemen kullanılabilir olmasını istiyorsanız 15 dakika önce bir süre seçin. |
| End | Azure CDN'nin artık blob dosyasına erişemeyeceği zaman. Azure CDN'de önceden önbelleğe alınmış dosyalara hala erişilebilir. Dosyanın son kullanma süresini denetlemek için Azure CDN güvenlik belirteci üzerinde uygun son kullanma süresini ayarlayın veya kıymeti temize çıkarın. |
| İzin verilen IP adresleri | İsteğe bağlı. **Verizon'dan Azure CDN**kullanıyorsanız, bu parametreyi [Verizon Edge Server IP Aralıkları'ndan Azure CDN'de](/azure/cdn/cdn-pop-list-api)tanımlanan aralıklara ayarlayabilirsiniz. **Akamai'den Azure CDN**kullanıyorsanız, IP adresleri statik olmadığından IP aralıkları parametresini ayarlayamazsınız.|
| İzin verilen protokoller | Protokol(ler) hesabı SAS ile yapılan bir istek için izin verdi. HTTPS ayarı önerilir.|

## <a name="next-steps"></a>Sonraki adımlar

SAS hakkında daha fazla bilgi için aşağıdaki makalelere bakın:
- [Paylaşılan erişim imzaları (SAS) kullanma](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1)
- [Paylaşılan Erişim İmzaları, Bölüm 2: Blob depolama alanı ile Bir SAS oluşturma ve kullanma](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-shared-access-signature-part-2)

Belirteç kimlik doğrulaması ayarlama hakkında daha fazla bilgi için [bkz.](https://docs.microsoft.com/azure/cdn/cdn-token-auth)
