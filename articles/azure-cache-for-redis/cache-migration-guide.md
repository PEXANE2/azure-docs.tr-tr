---
title: Redis için Azure Cache’e geçiş
description: Mevcut önbelleğinizi Redsıs için Azure önbelleğine geçirmeyi öğrenin
author: yegu-ms
ms.service: cache
ms.topic: conceptual
ms.date: 07/22/2020
ms.author: yegu
ms.openlocfilehash: 2a95aa9e9fccdb7047c2c0901f4349fecfbab672
ms.sourcegitcommit: 98854e3bd1ab04ce42816cae1892ed0caeedf461
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2020
ms.locfileid: "88009588"
---
# <a name="migrate-to-azure-cache-for-redis"></a>Redis için Azure Cache’e geçiş
Bu makalede, şirket içinde veya başka bir bulut hizmetinde çalışan mevcut bir redo önbelleğinin redin için Azure önbelleğine geçirilmesi için çeşitli yaklaşımlar açıklanmaktadır.

## <a name="migration-scenarios"></a>Geçiş senaryoları
Açık kaynaklı redin birçok işlem ortamında çalıştırılabilir. Ortak örnekler şunlardır:

- Özel veri merkezlerinde çalışan **Şirket içi** reddir önbellekleri.
- **Bulut tabanlı VM 'ler** -Redsıs, Azure VM 'lerde, AWS EC2, vb. üzerinde çalışan önbelleklerdir.
- **Barındırma hizmetleri** -yönetilen redsıs Hizmetleri (AWS ElastiCache gibi).
- **Farklı bölgeler** -redsıs önbellekler başka bir Azure bölgesinde bulunur.

Bu tür bir önbelleğiniz varsa, en az kesinti veya kapalı kalma süresine sahip Reda için Azure önbelleğine taşıyabilirsiniz.

## <a name="migration-options"></a>Geçiş seçenekleri

Bir önbellekten diğerine geçiş yapmak için kullanabileceğiniz farklı yollar vardır. Önbelleğinizin nerede olduğuna ve uygulamanızın onunla etkileşime bağlı olarak, bir yöntem diğerlerinden daha yararlı olacaktır. Sık kullanılan bazı geçiş stratejileri aşağıda ayrıntılı olarak verilmiştir.

   | Seçenek       | Avantajlar | Dezavantajlar |
   | ------------ | ---------- | ------------- |
   | Yeni önbellek oluştur | En basit uygulama. | Verileri yeni önbellekte yeniden doldurmanız gerekir, bu da birçok uygulamayla çalışmayabilir. |
   | RDB dosyası aracılığıyla verileri içeri ve dışarı aktarma | Tüm Redsıs önbelleğiyle genellikle uyumludur. | Bazı veriler, RDB dosya oluşturulduktan sonra var olan önbelleğe yazıldıklarında kaybolabilir. | 
   | İki önbelleğe çift yazma verileri | Veri kaybı veya küçük şehir yok. Mevcut önbelleğin kesintisiz işlemleri. Yeni önbelleğin daha kolay testi. | Uzun bir süre için iki önbellek gerekir. | 
   | Verileri program aracılığıyla geçirme | Verilerin nasıl taşınacağı hakkında tam denetim. | Özel kod gerektirir. | 

### <a name="create-a-new-azure-cache-for-redis"></a>Redsıs için yeni bir Azure önbelleği oluşturun

Bu yaklaşım teknik olarak bir geçiş değildir. Verilerin kaybedilmesi sorun yaratmazsa, redin için Azure önbelleğine taşımanın en kolay yolu, önbellek örneği oluşturmak ve uygulamanızı bu sunucuya bağlamak içindir. Örneğin, Reddir 'i veritabanı kayıtlarının bir görünüm önbelleği olarak kullanırsanız, önbelleği sıfırdan kolayca yeniden oluşturabilirsiniz.

Bu seçeneği uygulamak için genel adımlar şunlardır:

1. Redsıs örneği için yeni bir Azure önbelleği oluşturun.

2. Uygulamanızı yeni örneği kullanacak şekilde güncelleştirin.

3. Eski Redsıs örneğini silin.

### <a name="export-data-to-an-rdb-file-and-import-it-into-azure-cache-for-redis"></a>Verileri bir RDB dosyasına aktarın ve Redsıs için Azure önbelleğine aktarın

Açık kaynaklı redo, bir Önbelleğin bellek içi veri kümesinin anlık görüntüsünü almak ve bir dosyaya kaydetmek için standart bir mekanizma tanımlar. RDB adlı bu dosya başka bir Redsıs önbelleği tarafından okunabilir. [Redsıs Premium katmanının Azure önbelleği](cache-overview.md#service-tiers) , verileri RDB dosyalar aracılığıyla bir önbellek örneğine aktarmayı destekler. Bir RDB dosyasını, Redsıs için mevcut bir önbellekten Azure önbelleğine veri aktarmak için kullanabilirsiniz.

> [!IMPORTANT]
> RDB dosya biçimi, Redsıs sürümleri arasında değişebilir ve geriye dönük uyumluluğu koruyamayabilir. Dışarı aktardığınız önbelleğin Redsıs sürümü Redsıs için Azure Cache tarafından verilen sürümden eşit veya ondan küçük olmalıdır.
>

Bu seçeneği uygulamak için genel adımlar şunlardır:

1. Premium katmanda var olan önbellekten aynı boyutta (veya ondan büyük) bir Redsıs örneği için yeni bir Azure önbelleği oluşturun.

2. Var olan Redsıs önbelleğinin bir anlık görüntüsünü kaydedin. [Redsıs 'Yi, anlık görüntüleri](https://redis.io/topics/persistence) düzenli aralıklarla kaydetmek veya [Kaydet](https://redis.io/commands/save) veya [bgsave](https://redis.io/commands/bgsave) komutlarını kullanarak el ile çalıştırmak için yapılandırabilirsiniz. RDB dosyası varsayılan olarak "dump. RDB" olarak adlandırılır ve *redsıs. conf* yapılandırma dosyasında belirtilen yolda yer alır.

    > [!NOTE]
    > Redu için Azure Cache içindeki verileri geçiriyorsanız, [BIR RDB dosyasını dışarı aktarma](cache-how-to-import-export-data.md) veya bunun yerine [PowerShell Export cmdlet 'ini](https://docs.microsoft.com/powershell/module/azurerm.rediscache/export-azurermrediscache?view=azurermps-6.13.0&viewFallbackFrom=azurermps-6.4.0) kullanma hakkındaki yönergelere bakın.
    >

3. RDB dosyasını yeni önbelleğinizin bulunduğu bölgedeki bir Azure depolama hesabına kopyalayın. Bu görev için AzCopy kullanabilirsiniz.

4. Bu [içeri aktarma yönergelerini](cache-how-to-import-export-data.md) veya [PowerShell içeri aktarma CMDLET 'ini](https://docs.microsoft.com/powershell/module/azurerm.rediscache/import-azurermrediscache?view=azurermps-6.13.0&viewFallbackFrom=azurermps-6.4.0)kullanarak RDB dosyasını yeni önbelleğe aktarın.

5. Uygulamanızı yeni önbellek örneğini kullanacak şekilde güncelleştirin.

### <a name="write-to-two-redis-caches-simultaneously-during-migration-period"></a>Geçiş dönemi boyunca aynı anda iki Reda önbelleğine yazma

Verileri doğrudan önbellekler arasında taşımak yerine, hem mevcut bir önbelleğe hem de ayarladığınız yeni bir önbelleğe veri yazmak için uygulamanızı kullanabilirsiniz. Uygulama, başlangıçta mevcut önbellekteki verileri okumaya devam eder. Yeni önbellekte gerekli veriler olduğunda, uygulamayı o önbelleğe geçirin ve eski olanı devre dışı bırakın. Örneğin, redin 'u oturum deposu olarak kullandığınızda ve uygulama oturumları yedi gün boyunca geçerli olduğundan, diyelim. Haftalık iki önbelleğe yazıldıktan sonra, yeni önbelleğin, tüm süre dolmayan oturum bilgilerini içermesi gerekir. Veri kaybına neden olmadan bu noktadan itibaren güvenle güvenebilirsiniz.

Bu seçeneği uygulamak için genel adımlar şunlardır:

1. Premium katmanda var olan önbellekten aynı boyutta (veya ondan büyük) bir Redsıs örneği için yeni bir Azure önbelleği oluşturun.

2. Uygulama kodunu hem yeni hem de özgün örneklere yazacak şekilde değiştirin.

3. Yeni örnek verilerle yeterince dolduruluncaya kadar özgün örnekten veri okumaya devam edin.

4. Uygulama kodunu yalnızca yeni örnekten okumak ve yazmak için güncelleştirin.

5. Özgün örneği silin.

### <a name="migrate-programmatically"></a>Programlı olarak geçir

Varolan bir önbellekten programlı bir şekilde veri okuyarak ve Redsıs için Azure önbelleğine yazarak özel bir geçiş işlemi oluşturabilirsiniz. Bu [Açık kaynaklı araç](https://github.com/deepakverma/redis-copy) , redsıs örneği Için bir Azure önbelleğinden diğerine veri kopyalamak için kullanılabilir. Bu araç, verileri farklı Azure önbellek bölgelerinde bulunan önbellek örnekleri arasında taşımak için faydalıdır. [Derlenmiş bir sürüm](https://github.com/deepakverma/redis-copy/releases/download/alpha/Release.zip) de mevcuttur. Kaynak kodunu, kendi geçiş aracınızı yazmak için yararlı bir kılavuz olacak şekilde de bulabilirsiniz.

> [!NOTE]
> Bu araç, Microsoft tarafından resmi olarak desteklenmez. 
>

Bu seçeneği uygulamak için genel adımlar şunlardır:

1. Mevcut önbelleğin bulunduğu bölgede bir VM oluşturun. Veri kümeniz büyükse, kopyalama süresini azaltmak için görece güçlü bir VM seçin.

2. Redsıs örneği için yeni bir Azure önbelleği oluşturun.

3. Boş olduğundan emin olmak için yeni önbellekteki verileri temizler. Bu adım, kopyalama aracının kendisi hedef önbellekteki mevcut bir anahtarın üzerine yazmadığı için gereklidir.

    > [!IMPORTANT]
    > Kaynak önbellekten temizlediğinizden emin olun.
    >

4. Kaynak önbellekten hedefe veri kopyalamayı otomatik hale getirmek için yukarıdaki açık kaynak aracı gibi bir uygulama kullanın. Veri kümenizin boyutuna bağlı olarak, kopyalama işleminin tamamlanması biraz zaman alabilir.

## <a name="next-steps"></a>Sonraki adımlar
Redsıs özellikleri için Azure önbelleği hakkında daha fazla bilgi edinin.

* [Redsıs hizmet katmanları için Azure önbelleği](cache-overview.md#service-tiers)
* [Verileri içeri aktar](cache-how-to-import-export-data.md#import)
