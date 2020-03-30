---
title: DNS takma adı
description: Uygulamalarınız Azure SQL Veritabanı sunucunuzun adı için bir takma ad bağlanabilir. Bu arada, sql veritabanı diğer ad puan her zaman, sınama kolaylaştırmak için ve benzeri değiştirebilirsiniz.
services: sql-database
ms.service: sql-database
ms.subservice: operations
ms.custom: seo-lt-2019
ms.devlang: ''
ms.topic: conceptual
author: rohitnayakmsft
ms.author: rohitna
ms.reviewer: genemi, jrasnick, vanto
ms.date: 06/26/2019
ms.openlocfilehash: 05fa542a0ad1c72f73148eefd304a9771798598d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73820625"
---
# <a name="dns-alias-for-azure-sql-database"></a>Azure SQL Veritabanı için DNS takma adı

Azure SQL Veritabanı'nda Bir Etki Alanı Adı Sistemi (DNS) sunucusu vardır. PowerShell ve REST API'leri, SQL Veritabanı sunucu adınız için [DNS diğer adlarını oluşturmak ve yönetmek](#anchor-powershell-code-62x) için yapılan çağrıları kabul eder.

Azure SQL Veritabanı sunucu adının yerine *Bir DNS takma adı* kullanılabilir. İstemci programları bağlantı dizelerinde diğer adı kullanabilir. DNS diğer adı, istemci programlarınızı farklı sunuculara yönlendirebilecek bir çeviri katmanı sağlar. Bu katman, tüm istemcileri ve bağlantı dizelerini bulmak ve bunları bulmak zorunda kalmanın zorluklarından sizi kurtarıyor.

DNS diğer adlarının yaygın kullanımalanları aşağıdaki durumları içerir:

- Azure SQL Server için hatırlanması kolay bir ad oluşturun.
- İlk geliştirme sırasında, diğer adınız bir test SQL Veritabanı sunucusuna başvurabilir. Uygulama yayına girdiğinde, diğer adı üretim sunucusuna başvurmak üzere değiştirebilirsiniz. Testten üretime geçiş, veritabanı sunucusuna bağlanan birkaç istemcinin yapılandırmalarında herhangi bir değişiklik gerektirmez.
- Uygulamanızdaki tek veritabanının başka bir SQL Veritabanı sunucusuna taşındığını varsayalım. Burada, birkaç istemcinin yapılandırmalarını değiştirmek zorunda kalmadan diğer adı değiştirebilirsiniz.
- Bölgesel bir kesinti sırasında veritabanınızı farklı bir sunucu ve bölgede kurtarmak için coğrafi geri yükleme kullanırsınız. Varolan istemci uygulamasının yeniden bağlanabilmesi için varolan diğer adınızı yeni sunucuyu işaret etmek için değiştirebilirsiniz. 

## <a name="domain-name-system-dns-of-the-internet"></a>İnternet alan adı sistemi (DNS)

Internet DNS dayanır. DNS, dost isimlerinizi Azure SQL Veritabanı sunucunuzun adına çevirir.

## <a name="scenarios-with-one-dns-alias"></a>Bir DNS diğer adı olan senaryolar

Sisteminizi yeni bir Azure SQL Veritabanı sunucusuna değiştirmeniz gerektiğini varsayalım. Geçmişte her istemci programındaki her bağlantı dizesini bulmanız ve güncelleştirmeniz gerekiyordu. Ancak şimdi, bağlantı dizeleri bir DNS takma adı kullanıyorsa, yalnızca bir diğer ad özelliği güncelleştirilmelidir.

Azure SQL Veritabanı'nın DNS diğer ad özelliği aşağıdaki senaryolarda yardımcı olabilir:

### <a name="test-to-production"></a>Üretim testi

İstemci programlarını geliştirmeye başladığınızda, bağlantı dizelerinde bir DNS diğer adı kullanmalarını söyle. Diğer adın özelliklerini Azure SQL Veritabanı sunucunuzun bir test sürümüne işaret eder.

Daha sonra yeni sistem üretimde yayına girdiğinde, üretim SQL Veritabanı sunucusuna işaret etmek için diğer adın özelliklerini güncelleştirebilirsiniz. İstemci programlarında değişiklik gerekmez.

### <a name="cross-region-support"></a>Bölgeler arası destek

Olağanüstü durum kurtarma, SQL Veritabanı sunucunuzu farklı bir coğrafi bölgeye kaydırabilir. DNS takma adını kullanan bir sistem için, tüm istemciler için tüm bağlantı dizelerini bulma ve güncelleştirme gereksinimi önlenebilir. Bunun yerine, veritabanınızı barındıran yeni SQL Veritabanı sunucusuna başvurmak için bir diğer adı güncelleştirebilirsiniz.

## <a name="properties-of-a-dns-alias"></a>Bir DNS diğer adının özellikleri

SQL Veritabanı sunucunuz için aşağıdaki özellikler her DNS diğer adı için geçerlidir:

- *Benzersiz ad:* Oluşturduğunuz her takma ad, sunucu adları gibi tüm Azure SQL Veritabanı sunucuları arasında benzersizdir.
- *Sunucu gereklidir:* Tam olarak bir sunucuya başvurulmadığı sürece bir DNS diğer adı oluşturulamaz ve sunucu zaten var olmalıdır. Güncelleştirilmiş bir diğer ad her zaman varolan sunuculardan birine başvurmalıdır.
  - Bir SQL Veritabanı sunucusu bıraktığınızda, Azure sistemi sunucuya başvuran tüm DNS diğer adlarını da düşürür.
- *Herhangi bir bölgeye bağlı değildir:* DNS diğer adları bir bölgeye bağlı değildir. Herhangi bir DNS diğer adları, herhangi bir coğrafi bölgede bulunan bir Azure SQL Veritabanı sunucusuna başvurmak üzere güncelleştirilebilir.
  - Ancak, başka bir sunucuya başvurmak üzere bir diğer adı güncellerken, her iki sunucunun da aynı Azure *aboneliğinde*bulunması gerekir.
- *İzinler:* Bir DNS diğer adını yönetmek için, kullanıcının *Sunucu Katılımcısı* izinlerine veya daha yüksek olması gerekir. Daha fazla bilgi için azure [portalında Rol Tabanlı Erişim Denetimi'ne başlayın'](../role-based-access-control/overview.md)a bakın.

## <a name="manage-your-dns-aliases"></a>DNS takma adlarınızı yönetme

DNS takma adlarınızı programlı bir şekilde yönetmeniz için hem REST API'leri hem de PowerShell cmdlet'leri mevcuttur.

### <a name="rest-apis-for-managing-your-dns-aliases"></a>DNS diğer adlarınızı yönetmek için REST API'leri

REST API'leri için belgeler aşağıdaki web konumuna yakın dır:

- [Azure SQL Veritabanı REST API](https://docs.microsoft.com/rest/api/sql/)

Ayrıca, REST API'leri GitHub'da şu anda görülebilir:

- [Azure SQL Veritabanı sunucusu, DNS diğer adı REST API'leri](https://github.com/Azure/azure-rest-api-specs/blob/master/specification/sql/resource-manager/Microsoft.Sql/preview/2017-03-01-preview/serverDnsAliases.json)

<a name="anchor-powershell-code-62x"/>

#### <a name="powershell-for-managing-your-dns-aliases"></a>DNS diğer adlarınızı yönetmek için PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> PowerShell Azure Kaynak Yöneticisi modülü hala Azure SQL Veritabanı tarafından desteklenir, ancak gelecekteki tüm geliştirme az.sql modülü içindir. Bu cmdlets için [AzureRM.Sql](https://docs.microsoft.com/powershell/module/AzureRM.Sql/)bakın. Az modülündeki ve AzureRm modüllerinde bulunan komutların bağımsız değişkenleri önemli ölçüde aynıdır.

PowerShell cmdlets REST API'ler arama mevcuttur.

DNS takma adlarını yönetmek için kullanılan PowerShell cmdlets kod örneği şu şekilde belgelenmiştir:

- [Azure SQL Veritabanına DNS Diğer Adları için PowerShell](dns-alias-powershell.md)

Kod örneğinde kullanılan cmdletler şunlardır:

- [New-AzSqlServerDNSAlias](https://docs.microsoft.com/powershell/module/az.Sql/New-azSqlServerDnsAlias): Azure SQL Veritabanı hizmet sisteminde yeni bir DNS takma adı oluşturur. Takma ad, Azure SQL Veritabanı sunucusu 1'e başvurur.
- [Get-AzSqlServerDNSAlias](https://docs.microsoft.com/powershell/module/az.Sql/Get-azSqlServerDnsAlias): SQL DB server 1'e atanan tüm DNS takma adlarını alın ve listeleyin.
- [Set-AzSqlServerDNSAlias](https://docs.microsoft.com/powershell/module/az.Sql/Set-azSqlServerDnsAlias): Diğer adın ifade etmek üzere yapılandırıldığı sunucu adını, sunucu 1'den SQL DB server 2'ye değiştirir.
- [Remove-AzSqlServerDNSAlias](https://docs.microsoft.com/powershell/module/az.Sql/Remove-azSqlServerDnsAlias): DNS diğer adını kullanarak SQL DB server 2'den kaldırın.

## <a name="limitations-during-preview"></a>Önizleme sırasında sınırlamalar

Şu anda, bir DNS diğer adı aşağıdaki sınırlamalara sahiptir:

- *2 dakikaya kadar gecikme:* Bir DNS diğer adıgüncelleştirme veya kaldırılması 2 dakika kadar sürer.
  - Kısa bir gecikme olursa olsun, diğer ad istemci bağlantılarını eski sunucuya yönlendirmeyi hemen durdurur.
- *DNS arama:* Şimdilik, belirli bir DNS diğer adı hangi sunucunun atıfta bulunduğunu denetlemenin tek yetkili yolu, bir [DNS araması](https://docs.microsoft.com/windows-server/administration/windows-commands/nslookup)gerçekleştirmektir.
- _Tablo denetimi desteklenmez:_ Veritabanında *tablo denetimi* etkinleştirilmiş bir Azure SQL Veritabanı sunucusunda DNS takma adını kullanamazsınız.
  - Tablo denetimi amortismana hazırdır.
  - [Blob Auditing'e](sql-database-auditing.md)geçmenizi öneririz.

## <a name="related-resources"></a>İlgili kaynaklar

- Olağanüstü durum kurtarma da dahil olmak üzere [Azure SQL Veritabanı ile iş sürekliliğine genel bakış.](sql-database-business-continuity.md)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure SQL Veritabanına DNS Diğer Adları için PowerShell](dns-alias-powershell.md)
