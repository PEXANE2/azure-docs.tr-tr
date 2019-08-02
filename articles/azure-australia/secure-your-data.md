---
title: Azure Avustralya 'da veri güvenliği
description: Avustralya Kamu politika, yönetmelikler ve yasalların belirli ihtiyaçlarını karşılamak üzere Avustralya bölgeleri içinde Azure 'u yapılandırma.
author: Galey801
ms.service: azure-australia
ms.topic: conceptual
ms.date: 07/22/2019
ms.author: grgale
ms.openlocfilehash: 0301d506fc5764dbfda496727da95b2a1f2e9c7e
ms.sourcegitcommit: 6cff17b02b65388ac90ef3757bf04c6d8ed3db03
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2019
ms.locfileid: "68608342"
---
# <a name="data-security-in-azure-australia"></a>Azure Avustralya 'da veri güvenliği

Müşteri verilerinin güvenliğini sağlamak için kapsamlı ilkeler:

* Şifreleme kullanarak verileri koruma
* Gizli anahtarları yönetme
* Veri erişimini kısıtlama

## <a name="encrypting-your-data"></a>Verilerinizi şifreleme

Verilerin şifrelenmesi disk düzeyinde (bekleyen), veritabanlarında (yerinde ve yolda), uygulamalarda (aktarım sırasında) ve ağ üzerinde (aktarım sırasında) uygulanabilir. Azure 'da şifrelemeyi sağlamanın birkaç yolu vardır:

|Hizmet/Özellik|Açıklama|
|---|---|
|Depolama Hizmeti Şifrelemesi|Azure Depolama Hizmeti Şifrelemesi, depolama hesabı düzeyinde etkinleştirilir, bu da blok Blobları ve sayfa Blobları, Azure depolama 'ya yazılırken otomatik olarak şifrelenir. Azure depolama 'dan verileri okurken, depolama hizmeti tarafından geri döndürülmeden önce şifresi çözülür. Herhangi bir uygulamayı değiştirmek veya kod eklemek zorunda kalmadan verilerinizi güvenli hale getirmek için SSE 'yi kullanın.|
|Azure Disk Şifrelemesi|Azure sanal makinesi tarafından kullanılan işletim sistemi disklerini ve veri disklerini şifrelemek için Azure disk şifrelemesi kullanın. Azure Key Vault ile tümleştirme, denetlemenizi sağlar ve disk şifreleme anahtarlarını yönetmenize yardımcı olur.|
|İstemci tarafı uygulama düzeyi şifreleme|İstemci tarafı şifreleme, Java ve .NET depolama istemci kitaplıklarında yerleşik olarak bulunur ve bu da Azure Key Vault API 'Leri kullanabilir ve bu sayede basit hale gelir. Azure Active Directory kullanarak belirli bireyler için Azure Key Vault gizli bilgilere erişim kazanmak için Azure Key Vault kullanın.|
|Aktarım sırasında şifreleme|Azure Avustralya ile bağlantı için kullanılabilen temel şifreleme, aktarım düzeyi güvenlik (TLS) 1,2 protokolünü ve X. 509.440 sertifikalarını destekler. Federal bilgi Işleme standardı (FIPS) 140-2 düzey 1 şifreleme algoritmaları, Azure Avustralya veri merkezleri arasındaki altyapı ağ bağlantıları için de kullanılır.  Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1 ve Azure dosya paylaşımları, sanal makine ve dosya paylaşımı arasındaki şifreleme için SMB 3,0 kullanabilir. Verileri bir istemci uygulamasında depolamaya aktarılmadan önce şifrelemek ve depolama alanı dışına aktarıldıktan sonra verilerin şifresini çözmek için Istemci tarafı şifrelemesini kullanın.|
|IaaS VM 'Leri|Azure disk şifrelemesi 'ni kullanın. Azure depolama 'da bu diskleri yedeklemek için kullanılan VHD dosyalarını şifrelemek için Depolama Hizmeti Şifrelemesi açın, ancak bu yalnızca yeni yazılan verileri şifreler. Yani, bir VM oluşturur ve ardından VHD dosyasını tutan depolama hesabında Depolama Hizmeti Şifrelemesi etkinleştirirseniz, özgün VHD dosyası değil yalnızca değişiklikler şifrelenir.|
|İstemci Tarafında Şifreleme|Bu, verilerinizi şifrelemek için en güvenli yöntemdir, çünkü iletim öncesinde şifreler ve bekleyen verileri şifreler. Bununla birlikte, depolama kullanarak uygulamalarınıza kod eklemenizi gerektirir, ancak bunu yapmak istemeyebilirsiniz. Bu gibi durumlarda, iletim sırasında verileriniz için HTTPS ve bekleyen verileri şifrelemek için Depolama Hizmeti Şifrelemesi kullanabilirsiniz. İstemci tarafı şifreleme de istemci üzerinde daha fazla yük içerir — özellikle büyük miktarlarda veri şifrelerken ve aktarıyorsanız, ölçeklenebilirlik planlarınızda bunu hesaba geçirmeniz gerekir.|
|

Azure 'daki şifreleme seçenekleri hakkında daha fazla bilgi için [Depolama Güvenliği Kılavuzu](https://docs.microsoft.com/azure/storage/storage-security-guide)'na bakın.

## <a name="protecting-data-by-managing-secrets"></a>Gizli dizileri yöneterek verileri koruma

Güvenli anahtar yönetimi buluttaki verileri korumak için gereklidir. Müşterilerin, anahtar yönetimini basitleştirmek ve verileri şifrelemek için bulut uygulamaları ve Hizmetleri tarafından kullanılan anahtarların denetimini sürdürmek için çaba gösterilmesi gerekir.

### <a name="managing-secrets"></a>Gizli anahtarları yönetme

* Parolaların, sabit kodlanmış yapılandırma dosyaları, betikler veya kaynak kodunda açığa çıkarılması riskini en aza indirmek için Key Vault kullanın. Azure Key Vault, anahtarları (Azure disk şifrelemesi için şifreleme anahtarları gibi) ve gizli dizileri (parolalar gibi), FIPS 140-2 düzey 2 tarafından doğrulanan donanım güvenlik modüllerinde (HSM 'ler) depolayarak şifreler. Ek güvence için, bu HSM 'lerde anahtarları içeri veya dışarı aktarabilirsiniz.
* Uygulama kodu ve şablonlar yalnızca gizli dizi URI başvurularını içermelidir (yani gerçek gizlilikler kod, yapılandırma veya kaynak kodu depolarında değil). Bu, GitHub 'daki toplama botları gibi iç veya dış depolarda anahtar sızdırma saldırılarına engel olur.
* Key Vault içindeki güçlü RBAC denetimlerini kullanın. Güvenilen bir operatör şirketten ayrıldığında veya Şirket içindeki yeni bir gruba aktardıysanız, gizli anahtarlara erişebilmeleri önlenir.  

Daha fazla bilgi için bkz. [Azure Key Vault](azure-key-vault.md)

## <a name="isolation-to-restrict-data-access"></a>Veri erişimini kısıtlamak için yalıtım

Yalıtım, yalnızca yetkili kullanıcılara, hizmetlere ve uygulamalara veri erişimini sınırlamak için sınırları, segmentleri ve kapsayıcıları kullanmayla ilgilidir. Örneğin kiracılar arasındaki ayrım, Microsoft Azure gibi çok kiracılı bulut platformları için önemli bir güvenlik mekanizmasıdır. Mantıksal yalıtım, bir kiracının diğer kiracının işlemlerine engel olmasını önlemeye yardımcı olur.

#### <a name="per-customer-isolation"></a>Müşteri başına yalıtım

Azure, katman 2 VLAN yalıtımı, erişim denetim listeleri, yük dengeleyiciler ve IP filtreleri aracılığıyla ağ erişim denetimi ve ayırma uygular.

Müşteriler, kaynaklarını abonelikler, kaynak grupları, sanal ağlar ve alt ağlar arasında daha fazla yalıtabilir.

Microsoft Azure yalıtım hakkında daha fazla bilgi için [Azure genel bulutundaki yalıtıma](../security/fundamentals/isolation-choices.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar

[Azure VPN Gateway](vpn-gateway.md) makalesini gözden geçirin