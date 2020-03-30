---
title: Azure Depolama'yı kullanarak PaaS uygulamalarını güvence altına alma | Microsoft Dokümanlar
description: PaaS web ve mobil uygulamalarınızı güvence altına almak için Azure Depolama güvenliği en iyi uygulamaları hakkında bilgi edinin.
services: security
documentationcenter: na
author: TomShinder
manager: barbkess
editor: ''
ms.assetid: ''
ms.service: security
ms.subservice: security-fundamentals
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/28/2018
ms.author: tomsh
ms.openlocfilehash: 675e10101d01d831aad7652c70cbfcf320085a3c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "70999163"
---
# <a name="best-practices-for-securing-paas-web-and-mobile-applications-using-azure-storage"></a>Azure Depolama'yı kullanarak PaaS web ve mobil uygulamalarını güvence altına almak için en iyi uygulamalar
Bu makalede, hizmet olarak platform (PaaS) web ve mobil uygulamalarınızı güvence altına almak için Azure Depolama güvenlik en iyi uygulamaları koleksiyonunu tartışıyoruz. Bu en iyi uygulamalar Azure ile olan deneyimlerimizden ve sizin gibi müşterilerin deneyimlerinden elde edilir.

Azure, depolama alanını şirket içinde kolayca ulaşılamayacak şekilde dağıtmayı ve kullanmayı mümkün kılar. Azure depolama ile, nispeten daha az çabayla yüksek düzeyde ölçeklenebilirlik ve kullanılabilirlik düzeyine ulaşabilirsiniz. Azure Depolama, Windows ve Linux Azure Sanal Makineleri'nin temeli olmasının aynı zamanda büyük dağıtılmış uygulamaları da destekleyebilir.

Azure Depolama aşağıdaki dört hizmeti sağlar: Blob depolama, Tablo depolama, Sıra depolama ve Dosya depolama. Daha fazla bilgi için microsoft [Azure Depolama'ya Giriş 'e](/azure/storage/common/storage-introduction)bakın.

[Azure Depolama güvenlik kılavuzu,](/azure/storage/common/storage-security-guide) Azure Depolama ve güvenlik hakkında ayrıntılı bilgi için harika bir kaynaktır. Bu en iyi uygulamaları makale, daha fazla bilgi için güvenlik kılavuzunda bulunan bazı kavramları ve güvenlik kılavuzuna bağlantılar, hem de diğer kaynaklara yüksek düzeyde gider.

Bu makalede, aşağıdaki en iyi uygulamalar ele:

- Paylaşılan erişim imzaları (SAS)
- Rol tabanlı erişim denetimi (RBAC)
- Yüksek değerli veriler için istemci tarafı şifrelemesi
- Depolama Hizmeti Şifrelemesi


## <a name="use-a-shared-access-signature-instead-of-a-storage-account-key"></a>Depolama hesabı anahtarı yerine paylaşılan erişim imzası kullanma
Erişim denetimi çok önemlidir. Azure Depolama'ya erişimi denetlemenize yardımcı olmak için Azure, bir depolama hesabı oluşturduğunuzda iki adet 512 bit lik depolama hesabı anahtarı (SAK) oluşturur. Anahtar artıklığı düzeyi, rutin anahtar döndürme sırasında hizmet kesintilerini önlemenizi mümkün kılar. 

Depolama erişim anahtarları yüksek öncelikli sırlardır ve yalnızca depolama erişim denetiminin sorumluları tarafından erişilebilir olmalıdır. Yanlış kişiler bu anahtarlara erişirse, depolama nın tam denetimine sahip olurlar ve depolama alanına dosya ları değiştirebilir, silebilir veya ekleyebilir. Buna, kuruluşunuz veya müşterilerinizi tehlikeye atabilecek kötü amaçlı yazılımlar ve diğer içerik türleri de dahildir.

Yine de depolama daki nesnelere erişim sağlamanın bir yolunu bulabilmeniz gerekir. Daha ayrıntılı erişim sağlamak için paylaşılan erişim imzası (SAS) avantajlarından yararlanabilirsiniz. SAS, önceden tanımlanmış bir zaman aralığı ve belirli izinlerle belirli nesneleri depolama alanında paylaşmanızı mümkün kılar. Paylaşılan erişim imzası şunları tanımlamanıza olanak tanır:

- Başlangıç saati ve son kullanma saati de dahil olmak üzere SAS'ın geçerli olduğu aralık.
- SAS tarafından verilen izinler. Örneğin, blob üzerindeki bir SAS, kullanıcıya bu blob için okuma ve yazma izinleri verebilir, ancak izinleri silmez.
- Azure Depolama'nın SAS'ı kabul ettiği isteğe bağlı BIR IP adresi veya IP adresi aralığı. Örneğin, kuruluşunuza ait bir ip adresi aralığı belirtebilirsiniz. Bu, SAS'ınız için başka bir güvenlik ölçüsü sağlar.
- Azure Depolama'nın SAS'ı kabul ettiği protokol. Bu isteğe bağlı parametreyi, HTTPS kullanarak istemcilere erişimi kısıtlamak için kullanabilirsiniz.

SAS, depolama hesabı anahtarlarınızı vermeden içeriği istediğiniz şekilde paylaşmanızı sağlar. Uygulamanızda her zaman SAS kullanmak, depolama hesabı anahtarlarınızı tehlikeye atmadan depolama kaynaklarınızı paylaşmanın güvenli bir yoludur.

Paylaşılan erişim imzası hakkında daha fazla bilgi edinmek için [bkz.](/azure/storage/common/storage-dotnet-shared-access-signature-part-1) 

## <a name="use-role-based-access-control"></a>Rol tabanlı erişim denetimi kullanma
Erişimi yönetmenin başka bir yolu [da rol tabanlı erişim denetimi](/azure/role-based-access-control/overview) (RBAC) kullanmaktır. RBAC ile, çalışanlara, bilme ve en az ayrıcalık lı güvenlik ilkelerine dayalı olarak ihtiyaç duydukları tam izinleri vermeye odaklanırsınız. Çok fazla izin, bir hesabı saldırganlara maruz bırakabilir. Çok az izin, çalışanların işlerini verimli bir şekilde yapamayacakları anlamına gelir. RBAC, Azure için ince taneli erişim yönetimi sunarak bu sorunu gidermenize yardımcı olur. Bu, veri erişimi için güvenlik ilkelerini uygulamak isteyen kuruluşlar için zorunludur.

Kullanıcılara ayrıcalık lar atamak için Azure'da yerleşik RBAC rollerini kullanabilirsiniz. Örneğin, depolama hesaplarını yönetmesi gereken bulut operatörleri için Depolama Hesabı Katılımcısı'nı ve klasik depolama hesaplarını yönetmek için Klasik Depolama Hesabı Katılımcısı rolünü kullanın. VM'leri yönetmesi gereken ancak bağlı oldukları sanal ağ veya depolama hesabını yönetmesi gereken bulut operatörleri için bunları Sanal Makine Katılımcısı rolüne ekleyebilirsiniz.

RBAC gibi yetenekleri kullanarak veri erişim denetimini zorlamayan kuruluşlar, kullanıcıları için gerekenden daha fazla ayrıcalık veriyor olabilir. Bu, bazı kullanıcıların ilk başta sahip olmamaları gereken verilere erişmesine izin vererek veri güvenliğine yol açabilir.

RBAC hakkında daha fazla bilgi edinmek için bkz:

- [RBAC ve Azure portalı kullanarak erişimi yönetme](/azure/role-based-access-control/role-assignments-portal)
- [Azure kaynakları için yerleşik roller](/azure/role-based-access-control/built-in-roles)
- [Azure Depolama güvenlik kılavuzu](/azure/storage/common/storage-security-guide) 

## <a name="use-client-side-encryption-for-high-value-data"></a>Yüksek değerli veriler için istemci tarafı şifrelemeyi kullanma
İstemci tarafı şifreleme, Azure Depolama'ya yüklemeden önce aktarım sırasındaki verileri programlı olarak şifrelemenize ve verileri alırken programlı olarak çözmenizi sağlar. Bu, aktarım sırasında ki verilerin şifrelemesini sağlar, ancak aynı zamanda veri şifrelemesini de sağlar. İstemci tarafı şifreleme, verilerinizi şifrelemenin en güvenli yöntemidir, ancak uygulamanızda programlı değişiklikler yapmanızı ve önemli yönetim süreçlerini devreye sokmanızı gerektirir.

İstemci tarafı şifreleme, şifreleme anahtarlarınız üzerinde tek kontrole sahip olduğunuzu da sağlar. Kendi şifreleme anahtarlarınızı oluşturabilir ve yönetebilirsiniz. Azure depolama istemcikitaplığı, anahtar şifreleme anahtarı (KEK) kullanılarak sarılmış (şifrelenmiş) bir içerik şifreleme anahtarı (CEK) oluşturduğu bir zarf tekniği kullanır. KEK, önemli bir tanımlayıcı tarafından tanımlanır ve asimetrik anahtar çifti veya simetrik anahtar olabilir ve yerel olarak yönetilebilir veya [Azure Key Vault'ta](/azure/key-vault/key-vault-overview)depolanabilir.

İstemci tarafı şifreleme, Java ve .NET depolama istemcisi kitaplıklarına yerleşiktir. İstemci uygulamaları içindeki verileri şifreleme ve kendi şifreleme anahtarlarınızı oluşturma ve yönetme hakkında bilgi almak [için İstemci tarafı şifreleme](/azure/storage/common/storage-client-side-encryption) ve Microsoft Azure Depolama için Azure Anahtar Kasası'na bakın.

## <a name="enable-storage-service-encryption-for-data-at-rest"></a>Veriler için Depolama Hizmeti Şifrelemesini etkinleştirin
Dosya depolama için [Depolama Hizmeti Şifrelemesi](/azure/storage/common/storage-service-encryption) etkinleştirildiğinde, veriler AES-256 şifrelemesi kullanılarak otomatik olarak şifrelenir. Microsoft, tüm şifreleme, şifre çözme ve anahtar yönetimini işler. Bu özellik LRS ve GRS artıklık türleri için kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, PaaS web ve mobil uygulamalarınızı güvence altına almak için en iyi Azure Depolama güvenlik uygulamaları koleksiyonu tanıtıldı. PaaS dağıtımlarınızın güvenliğini sağlama hakkında daha fazla bilgi edinmek için bkz:

- [PaaS dağıtımlarının güvenliğini sağlama](paas-deployments.md)
- [Azure Uygulama Hizmetlerini Kullanarak PaaS web ve mobil uygulamaları güvence altına alma](paas-applications-using-app-services.md)
- [Azure'da PaaS veritabanlarının güvenliğini sağlama](paas-applications-using-sql.md)
