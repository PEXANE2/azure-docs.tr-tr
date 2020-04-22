---
title: Azure Key Vault sırları hakkında - Azure Key Vault
description: Azure Key Vault REST arabirimine ve sırlar için geliştirici ayrıntılarına genel bakış.
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: secrets
ms.topic: overview
ms.date: 09/04/2019
ms.author: mbaldwin
ms.openlocfilehash: eabfa03aa70f54a967fe256f694ef59ad0fe7ebe
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81685446"
---
# <a name="about-azure-key-vault-secrets"></a>Azure Key Vault sırları hakkında

Key Vault, parolalar ve veritabanı bağlantı dizeleri gibi sırların güvenli bir şekilde depolanmasını sağlar.

Bir geliştiricinin bakış açısından, Key Vault API'leri gizli değerleri dizeleri olarak kabul eder ve döndürür. Dahili olarak, Key Vault, her biri 25k bayt maksimum boyutuyla sırları sekizli (8 bit bayt) dizileri olarak saklar ve yönetir. Key Vault hizmeti sırlar için anlamsallık sağlamaz. Yalnızca verileri kabul eder, şifreler, saklar ve gizli bir tanımlayıcıyı ("id") döndürür. Tanımlayıcı daha sonra sırrı almak için kullanılabilir.  

Son derece hassas veriler söz konusu olduğunda müşterilerin veriler için ek koruma katmanlarını göz önünde bulundurması gerekir. Anahtarı Anahtar Kasasında depolamadan önce ayrı bir koruma anahtarı kullanarak verileri şifrelemek bunun bir örneğidir.  

Key Vault da sırlar için bir contentType alanı destekler. İstemciler, gizli verilerin alındığı zaman yorumlanmasına yardımcı olmak için bir sırrın içerik türünü belirtebilir. Bu alanın maksimum uzunluğu 255 karakterdir. Önceden tanımlanmış değerler yoktur. Önerilen kullanım, gizli verileri yorumlamak için bir ipucudur. Örneğin, bir uygulama hem parolaları hem de sertifikaları sır olarak depolayabilir ve sonra bu alanı ayırt etmek için kullanabilir. Önceden tanımlanmış değer yok.  

## <a name="secret-attributes"></a>Gizli öznitelikler

Gizli verilere ek olarak, aşağıdaki öznitelikler belirtilebilir:  

- *exp*: IntDate, isteğe bağlı, varsayılan **sonsuza kadar**. *Exp* (son kullanma süresi) özniteliği, [belirli durumlar](#date-time-controlled-operations)dışında gizli verilerin alınmaması gereken son kullanma süresini tanımlar. Bu alan, yalnızca anahtar kasa hizmetinin kullanıcılarına belirli bir sırrın kullanılmayabileceğini bildirdiği için **bilgilendirme** amaçlıdır. Değeri IntDate değeri içeren bir sayı olmalıdır.   
- *nbf*: IntDate, isteğe bağlı, varsayılan **şimdi**. *NBF* (daha önce değil) [özniteliği, belirli durumlar](#date-time-controlled-operations)dışında gizli verilerin alınmaması gereken zamanı tanımlar. Bu alan yalnızca **bilgilendirme** amaçlıdır. Değeri IntDate değeri içeren bir sayı olmalıdır. 
- *etkin*: boolean, isteğe bağlı, varsayılan **doğrudur**. Bu öznitelik, gizli verilerin alınıp alınamayacağını belirtir. Etkin öznitelik *nbf* ve *exp*arasında bir işlem oluştuğunda *nbf* ve *exp* ile birlikte kullanılır, sadece etkin **gerçek**ayarlanırsa izin verilecektir. *Nbf* ve *exp* penceresi dışındaki [işlemlere belirli durumlar](#date-time-controlled-operations)dışında otomatik olarak izin verilmez.  

Gizli öznitelikleri içeren herhangi bir yanıtta yer alan salt okunur öznitelikleri vardır:  

- *created*: IntDate, isteğe bağlı. Oluşturulan öznitelik, sırrın bu sürümünün ne zaman oluşturulduğunu gösterir. Bu değer, bu öznitelik eklenmesinden önce oluşturulan sırlar için null. Değeri, IntDate değeri içeren bir sayı olmalıdır.  
- *güncelleme*: IntDate, isteğe bağlı. Güncelleştirilmiş öznitelik, sırrın bu sürümünün ne zaman güncelleştirileni gösterir. Bu değer, bu öznitelik eklenmeden önce en son güncelleştirilen sırlar için null' dur. Değeri, IntDate değeri içeren bir sayı olmalıdır.

### <a name="date-time-controlled-operations"></a>Tarih-zaman kontrollü işlemler

Bir sır **almak** operasyon *nbf* / *exp* penceresi dışında, henüz geçerli olmayan ve süresi dolmuş sırları için çalışacaktır. Henüz geçerli olmayan bir sır için bir sırrın **get** operasyonunu aramak, test amacıyla kullanılabilir. Alma **(ting ting)** süresi dolmuş bir sır, kurtarma işlemleri için kullanılabilir.

## <a name="secret-access-control"></a>Parola erişim denetimi

Key Vault'ta yönetilen sırlar için Erişim Kontrolü, bu sırları içeren Key Vault düzeyinde sağlanır. Sırlar için erişim denetimi ilkesi, aynı Key Vault'taki anahtarlar için erişim denetimi ilkesinden farklıdır. Kullanıcılar sır tutmak için bir veya daha fazla kasa oluşturabilir ve senaryoya uygun bölümlemesi ve sırların yönetimini sürdürmeleri gerekir.   

Aşağıdaki izinler, her temel olarak, bir kasaüzerinde kontrol girişine erişen sırlarda kullanılabilir ve gizli bir nesneüzerinde izin verilen işlemleri yakından yansıtabilir:  

- Gizli yönetim işlemleri için izinler
  - *get*: Bir sır okuyun  
  - *liste*: Key Vault'ta saklanan bir sırrın sırlarını veya sürümlerini listele  
  - *set*: Bir sır oluşturma  
  - *delete*: Bir sırrı silme  
  - *recover*: Silinmiş bir sırrı kurtarma
  - *yedekleme*: Anahtar kasasında ki bir sırrı yedekle
  - *restore*: Yedeklenmiş bir sırrı anahtar kasasına geri yükleme

- Ayrıcalıklı işlemler için izinler
  - *tasfiye*: Silinmiş bir sırrı temizleme (kalıcı olarak silme)

Sırlar ile çalışma hakkında daha fazla bilgi [için, Key Vault REST API referans Gizli işlemleri](/rest/api/keyvault)bakın. İzinlerin oluşturulması hakkında daha fazla bilgi için [Vaults - Update Access Policy](/rest/api/keyvault/vaults/updateaccesspolicy) [bkz.](/rest/api/keyvault/vaults/createorupdate) 

## <a name="secret-tags"></a>Gizli etiketler  
Etiketler biçiminde uygulamaya özgü ek meta verileri belirtebilirsiniz. Key Vault, her biri 256 karakter adı ve 256 karakter değerine sahip olabilecek en fazla 15 etiketi destekler.  

>[!Note]
>Etiketler, *liste* varsa veya izin *alıyorsa,* arayan kişi tarafından okunabilir.

## <a name="azure-storage-account-key-management"></a>Azure Depolama hesap anahtar yönetimi

Key Vault, Azure depolama hesabı anahtarlarını yönetebilir:

- Dahili olarak, Key Vault tuşlarını Bir Azure depolama hesabıyla listeleyebilir (eşitleyebilir). 
- Key Vault tuşları periyodik olarak yeniden üretir (döndürür).
- Anahtar değerler arayana yanıt olarak asla döndürülür.
- Key Vault, hem depolama hesaplarının hem de klasik depolama hesaplarının anahtarlarını yönetir.

Daha fazla bilgi için Bkz. [Azure Anahtar Kasası Depolama Hesap Anahtarları](../secrets/overview-storage-keys.md))

## <a name="storage-account-access-control"></a>Depolama hesabı erişim kontrolü

Aşağıdaki izinler, bir kullanıcıya veya uygulama sorumlusuna yönetilen bir depolama hesabında işlemleri gerçekleştirmesi için yetki verirken kullanılabilir:  

- Yönetilen depolama hesabı ve SaS tanımlı işlemler için izinler
  - *get*: Depolama hesabı hakkında bilgi alır 
  - *liste*: Key Vault tarafından yönetilen depolama hesaplarını listele
  - *update*: Depolama hesabını güncelleştir
  - *delete*: Depolama hesabını silme  
  - *recover*: Silinen bir depolama hesabını kurtarma
  - *yedekleme*: Depolama hesabını yedekleme
  - *geri yükleme*: Yedeklenmiş bir depolama hesabını Key Vault'a geri yükleme
  - *set*: Bir depolama hesabı oluşturma veya güncelleme
  - *regeneratekey*: Depolama hesabı için belirtilen anahtar değerini yeniden oluşturma
  - *getsas*: Depolama hesabı için SAS tanımı hakkında bilgi alın
  - *listsas*: Depolama hesabı için depolama SAS tanımlarını listele
  - *deletesas*: Bir depolama hesabından SAS tanımını silme
  - *setsas*: Bir depolama hesabı için yeni bir SAS tanımı/öznitelikleri oluşturma veya güncelleştirme

- Ayrıcalıklı işlemler için izinler
  - *temizleme*: Yönetilen bir depolama hesabını temizleme (kalıcı olarak silme)

Daha fazla bilgi için [Key Vault REST API başvurusundaki Depolama hesabı işlemlerine](/rest/api/keyvault)bakın. İzinlerin oluşturulması hakkında daha fazla bilgi için [Vaults - Update Access Policy](/rest/api/keyvault/vaults/updateaccesspolicy) [bkz.](/rest/api/keyvault/vaults/createorupdate)

## <a name="next-steps"></a>Sonraki adımlar

- [Anahtar Kasası Hakkında](../general/overview.md)
- [Anahtarlar, sırlar ve sertifikalar hakkında](../general/about-keys-secrets-certificates.md)
- [Anahtarlar hakkında](../keys/about-keys.md)
- [Sertifikalar hakkında](../certificates/about-certificates.md)
- [Kimlik doğrulama, istekler ve yanıtlar](../general/authentication-requests-and-responses.md)
- [Key Vault Geliştirici Kılavuzu](../general/developers-guide.md)
