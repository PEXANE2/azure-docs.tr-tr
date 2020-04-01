---
title: Eski Azure DNS Özel Bölgelerini yeni kaynak modeline geçirme
titleSuffix: Azure DNS
description: Bu kılavuz, eski özel DNS bölgelerinin en son kaynak modeline nasıl geçirilir hakkında adım adım yönerge sağlar
services: dns
author: rohinkoul
ms.service: dns
ms.topic: tutorial
ms.date: 06/18/2019
ms.author: rohink
ms.openlocfilehash: d29885104d6f39a17b5bdeb786cda8f56f58d987
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "76939360"
---
# <a name="migrating-legacy-azure-dns-private-zones-to-new-resource-model"></a>Eski Azure DNS özel bölgelerini yeni kaynak modeline geçirme

Genel önizleme sırasında, "zoneType" özelliği "Özel" olarak ayarlanmış "dnszones" kaynağı kullanılarak özel DNS bölgeleri oluşturuldu. Bu tür bölgeler 31 Aralık 2019'dan sonra desteklenmeyen ve "dnszones" yerine "privateDnsZones" kaynak türünü kullanan GA kaynak modeline geçirilmelidir. Geçiş işlemi basittir ve bu işlemi otomatikleştirmek için bir PowerShell komut dosyası sağladık. Bu kılavuz, Azure DNS özel bölgelerinizi yeni kaynak modeline geçirmek için adım adım yönerge sağlar.

Geçiş gerektiren dnszones kaynaklarını bulmak için; Azure CLI'de aşağıdaki komutu uygulayın.
```azurecli
az account set --subscription <SubscriptionId>
az network dns zone list --query "[?zoneType=='Private']"
```

## <a name="prerequisites"></a>Ön koşullar

Azure PowerShell'in en son sürümünü yüklediğinizden emin olun. Azure PowerShell (Az) hakkında daha fazla bilgi ve nasıl yüklenirhttps://docs.microsoft.com/powershell/azure/new-azureps-module-az

Azure PowerShell için Az.PrivateDns modülüne sahip olduğunuzdan emin olun. Bu modülü yüklemek için yükseltilmiş bir PowerShell penceresi (Yönetim modu) açın ve aşağıdaki komutu girin

```powershell
Install-Module -Name Az.PrivateDns
```

>[!IMPORTANT]
>Geçiş işlemi tam otomatiktir ve herhangi bir kapalı kalma süresine neden olması beklenmiyor. Ancak, kritik bir üretim ortamında Azure DNS özel bölgeleri (önizleme) kullanıyorsanız, planlanan bakım süresi penceresinde aşağıdaki geçiş işlemini gerçekleştirmeniz gerekir. Geçiş komut dosyasını çalıştırırken özel bir DNS bölgesinin yapılandırmasını veya kayıt kümelerini değiştirmediğinizden emin olun.

## <a name="installing-the-script"></a>Komut dosyasının yüklenmesi

Yükseltilmiş bir PowerShell penceresi açın (Yönetim modu) ve aşağıdaki komutu çalıştırın

```powershell
install-script PrivateDnsMigrationScript
```

Komut dosyasını yüklemesi istendiğinde "A" girin

![Komut dosyasının yüklenmesi](./media/private-dns-migration-guide/install-migration-script.png)

PowerShell komut dosyasının en son sürümünü dehttps://www.powershellgallery.com/packages/PrivateDnsMigrationScript

>[!IMPORTANT]
>Geçiş komut dosyası Azure bulut kabuğunda çalıştırılmamalı ve Internet'e bağlı bir VM veya yerel makinede çalıştırılmalıdır.

## <a name="running-the-script"></a>Komut dosyasını çalıştırma

Komut dosyasını çalıştırmak için aşağıdaki komutu yürütme

```powershell
PrivateDnsMigrationScript.ps1
```

![Komut dosyasını çalıştırma](./media/private-dns-migration-guide/running-migration-script.png)

### <a name="enter-the-subscription-id-and-sign-in-to-azure"></a>Abonelik kimliğini girin ve Azure'da oturum açın

Geçirmek istediğiniz özel DNS bölgelerini içeren abonelik kimliğini girmeniz istenir. Azure hesabınızda oturum açmanız istenir. Komut dosyasının abonelikteki özel DNS bölge kaynaklarına erişebilmeleri için oturum açma'yı tamamlayın.

![Azure'da oturum açma](./media/private-dns-migration-guide/login-migration-script.png)

### <a name="select-the-dns-zones-you-want-to-migrate"></a>Geçirmek istediğiniz DNS bölgelerini seçin

Abonelikteki tüm özel DNS bölgelerinin listesini alın ve hangilerini geçirmek istediğinizi onaylamanızı ister. Tüm özel DNS bölgelerini geçirmek için "A" girin. Bu adımı çalıştırdıktan sonra, komut dosyası yeni kaynak modelini kullanarak yeni özel DNS bölgeleri oluşturur ve verileri yeni DSN bölgesine kopyalar. Bu adım zaten mevcut özel DNS bölgeleri değiştirmez.

![DNS bölgelerini seçin](./media/private-dns-migration-guide/migratezone-migration-script.png)

### <a name="switching-dns-resolution-to-the-new-dns-zones"></a>DNS çözünürlüğü yeni DNS bölgelerine geçiş

Bölgeler ve kayıtlar yeni kaynak modeline kopyalandıktan sonra, komut dosyası DNS çözünürlüğünü yeni DNS bölgelerine değiştirmenizi ister. Bu adım, eski özel DNS bölgeleri ve sanal ağlarınız arasındaki ilişkiyi kaldırır. Eski bölge sanal ağlardan bağlantısız olduğunda, yukarıdaki adımda oluşturulan yeni DNS bölgeleri bu sanal ağlar için DNS çözünürlüğünü otomatik olarak devralır.

Tüm sanal ağlar için DNS çözünürlüğünü değiştirmek için 'A' seçeneğini belirleyin.

![Ad Çözümlemesi Arasında Geçiş](./media/private-dns-migration-guide/switchresolution-migration-script.png)

### <a name="verify-the-dns-resolution"></a>DNS çözünürlüğünü doğrulama

Daha fazla ilerlemeden önce, DNS bölgelerinizdeki DNS çözünürlüğünün beklendiği gibi çalıştığını doğrulayın. Azure VM'lerinizde oturum açabilir ve DNS çözümlemesi çalıştığını doğrulamak için geçirilen bölgelere karşı nslookup sorgusu düzenleyebilirsiniz.

![Ad Çözümünü Doğrula](./media/private-dns-migration-guide/verifyresolution-migration-script.png)

DNS sorgularının çözülmediğini fark ederseniz, birkaç dakika bekleyin ve sorguları yeniden deneyin. DNS sorguları beklendiği gibi çalışıyorsa, komut dosyası sanal ağı özel DNS bölgesinden kaldırmanızı istediğinde 'Y' girin.

![Ad Çözümünü Onayla](./media/private-dns-migration-guide/confirmresolution-migration-script.png)

>[!IMPORTANT]
>Herhangi bir nedenle geçirilen bölgelere karşı DNS çözünürlüğü beklendiği gibi çalışmıyorsa, yukarıdaki adıma 'N' girin ve komut dosyası DNS çözünürlüğünü eski bölgelere geri döndürür. Bir destek bileti oluşturun ve DNS bölgelerinizin geçişinde size yardımcı olabiliriz.

## <a name="cleanup"></a>Temizleme

Bu adım, eski DNS bölgelerini siler ve yalnızca DNS çözümlüçalışmanın beklendiği gibi çalıştığını doğruladıktan sonra yürütülmelidir. Her özel DNS bölgesini silmeniz istenir. Bu bölgeler için DNS çözünürlüğünün düzgün çalıştığını doğruladıktan sonra her istekte 'Y' girin.

![Temizleme](./media/private-dns-migration-guide/cleanup-migration-script.png)

## <a name="update-your-automation"></a>Otomasyonunuzu Güncelleyin

Şablonlar, PowerShell komut dosyaları veya SDK kullanılarak geliştirilen özel kod gibi otomasyon uyguluyorsanız, özel DNS bölgeleri için yeni kaynak modelini kullanmak için otomasyonunuzu güncelleştirmeniz gerekir. Aşağıda yeni özel DNS CLI/PS/SDK belgelerine bağlantılar verilmiştir.
* [Azure DNS özel bölgeler REST API](https://docs.microsoft.com/rest/api/dns/privatedns/privatezones)
* [Azure DNS özel bölgeler CLI](https://docs.microsoft.com/cli/azure/ext/privatedns/network/private-dns?view=azure-cli-latest)
* [Azure DNS özel bölgeler PowerShell](https://docs.microsoft.com/powershell/module/az.privatedns/?view=azps-2.3.2)
* [Azure DNS özel bölgeler SDK](https://docs.microsoft.com/dotnet/api/overview/azure/privatedns/management?view=azure-dotnet-preview)

## <a name="need-further-help"></a>Daha fazla yardıma ihtiyacınız var

Geçiş işlemi yle ilgili daha fazla yardıma ihtiyacınız varsa veya yukarıda listelenen adımların sizin için çalışmadığı herhangi bir nedenle bir destek bileti oluşturun. PowerShell komut dosyası tarafından oluşturulan transkript dosyasını destek biletinize ekleyin.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure PowerShell](./private-dns-getstarted-powershell.md) veya [Azure CLI'yi](./private-dns-getstarted-cli.md)kullanarak Azure DNS'de nasıl özel bir bölge oluşturabilirsiniz öğrenin.

* Azure DNS'deki özel bölgelerle gerçekleştirilebilecek bazı yaygın [özel bölge senaryoları](./private-dns-scenarios.md) hakkında bilgi edinin.

* Belirli işlem türleri için bekleyebileceğiniz belirli davranışlar da dahil olmak üzere Azure DNS'deki özel bölgelerle ilgili sık sorulan sorular ve yanıtlar için [Bkz.](./dns-faq-private.md)

* DNS bölgeleri ve kayıtları genel bakış ziyaret ederek [DNS bölgeleri ve](dns-zones-records.md)kayıtları hakkında bilgi edinin.

* Azure'un diğer önemli [ağ özelliklerinden](../networking/networking-overview.md) bazıları hakkında bilgi edinin.
