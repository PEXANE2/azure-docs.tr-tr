---
title: Arama dizin oluşturma için Azure SQL VM bağlantısı
titleSuffix: Azure Cognitive Search
description: Şifreli bağlantıları etkinleştirin ve Güvenlik duvarını Azure Bilişsel Arama'daki bir dizin leyiciden Bir Azure sanal makinesindeki (VM) SQL Server'a bağlantılara izin verecek şekilde yapılandırın.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 1ab2b7860e8a75da5f8acef2fc4fa54d4b73a30d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80256972"
---
# <a name="configure-a-connection-from-an-azure-cognitive-search-indexer-to-sql-server-on-an-azure-vm"></a>Azure VM'de Bir Azure Bilişsel Arama dizinleyicisinden SQL Server'a bağlantı yapılandırma

[Azure SQL Veritabanı'nı Azure Bilişsel Arama'ya bağlamak dizin oluşturarak,](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md#faq) **Azure VM'lerde (veya** kısaca **SQL Azure VM'ler)** SQL Server'a karşı dizin oluşturarak Azure Bilişsel Arama tarafından desteklenir, ancak önce güvenlikle ilgili birkaç ön koşul vardır. 

Azure Bilişsel Arama'dan Sql Server'a VM üzerinden bağlantılar herkese açık bir internet bağlantısıdır. Normalde bu bağlantılar için izleyeceğiniz tüm güvenlik önlemleri burada da geçerlidir:

+ Azure VM'deki SQL Server örneğinin tam nitelikli alan adı için bir [Sertifika Yetkilisi sağlayıcısından](https://en.wikipedia.org/wiki/Certificate_authority#Providers) sertifika alın.
+ Sertifikayı VM'ye yükleyin ve bu makaledeki yönergeleri kullanarak VM'deki şifreli bağlantıları etkinleştirin ve yapılandırın.

## <a name="enable-encrypted-connections"></a>Şifreli bağlantıları etkinleştirme
Azure Bilişsel Arama, genel internet bağlantısı üzerinden tüm dizinleyici istekleri için şifreli bir kanal gerektirir. Bu bölümde, bu işi yapmak için adımlar listelenin.

1. Konu adının Azure VM'nin tam nitelikli alan adı (FQDN) olduğunu doğrulamak için sertifikanın özelliklerini denetleyin. Özellikleri görüntülemek için CertUtils veya Sertifikalar gibi bir araç kullanabilirsiniz. FQDN'yi VM servis bıçağının Essentials bölümünden, [Azure portalında](https://portal.azure.com/) **Genel IP adresi/DNS ad etiketi** alanından alabilirsiniz.
   
   * Yeni **Kaynak Yöneticisi** şablonu kullanılarak oluşturulan VM'ler için FQDN`<your-VM-name>.<region>.cloudapp.azure.com`
   * **Klasik** VM olarak oluşturulan eski VM'ler için `<your-cloud-service-name.cloudapp.net>`FQDN .

2. SQL Server'ı, Kayıt Defteri Düzenleyicisi'ni (regedit) kullanarak sertifikayı kullanacak şekilde yapılandırın. 
   
    SQL Server Configuration Manager genellikle bu görev için kullanılsa da, bu senaryo için kullanamazsınız. Azure'daki VM'nin FQDN'si VM tarafından belirlenen FQDN ile eşleşmediği için içe aktarılan sertifikayı bulamaz (etki alanını yerel bilgisayar veya birleştiği ağ etki alanı olarak tanımlar). Adlar eşleşmediğinde, sertifikayı belirtmek için regedit'i kullanın.
   
   * Regedit'te, bu kayıt defteri `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\[MSSQL13.MSSQLSERVER]\MSSQLServer\SuperSocketNetLib\Certificate`anahtarına göz atın: .
     
     Parça `[MSSQL13.MSSQLSERVER]` sürüm ve örnek adına göre değişir. 
   * **Sertifika** anahtarının değerini VM'ye aldığınız TLS/SSL sertifikasının **parmak izine** ayarlayın.
     
     Parmak izi almak için çeşitli yollar vardır, bazı diğerlerinden daha iyi. MMC'deki **Sertifikalar'dan** kopyalarsanız, büyük olasılıkla [bu destek makalesinde açıklandığı gibi](https://support.microsoft.com/kb/2023869/)görünmez bir öncü karakter alırsınız ve bu da bağlantı girişiminde bulunulduğunda hataya neden olur. Bu sorunu düzeltmek için çeşitli geçici çözüm geçici çözüm vardır. En kolayı, regedit'teki anahtar değer alanındaki ana karakteri kaldırmak için parmak izinin ilk karakterini yeniden yazmak ve sonra arka adada yazmaktır. Alternatif olarak, parmak izini kopyalamak için farklı bir araç kullanabilirsiniz.

3. Hizmet hesabına izin ver. 
   
    TLS/SSL sertifikasının özel anahtarında SQL Server hizmet hesabına uygun izin verildiğinden emin olun. Bu adımı atlarsanız, SQL Server başlamaz. Bu görev için **Sertifikaları** snap-in veya **CertUtils** kullanabilirsiniz.
    
4. SQL Server hizmetini yeniden başlatın.

## <a name="configure-sql-server-connectivity-in-the-vm"></a>VM'de SQL Server bağlantısını yapılandırma
Azure Bilişsel Arama'nın gerektirdiği şifreli bağlantıyı ayarladıktan sonra, Azure VM'lerde SQL Server'a ek yapılandırma adımları gelir. Bunu zaten yapmadıysanız, bir sonraki adım yapılandırmayı şu makalelerden birini kullanarak bitirmektir:

* Kaynak **Yöneticisi** VM için, [Kaynak Yöneticisi'ni kullanarak Azure'daki SQL Server Virtual Machine'e bağlan'a](../virtual-machines/windows/sql/virtual-machines-windows-sql-connect.md)bakın. 
* **Klasik** VM için Azure [Classic'teki SQL Server Virtual Machine'e bağlan'a](../virtual-machines/windows/classic/sql-connect.md)bakın.

Özellikle, "internet üzerinden bağlanma" için her makalede bölümü gözden geçirin.

## <a name="configure-the-network-security-group-nsg"></a>Ağ Güvenlik Grubu'nu (NSG) yapılandırma
Azure VM'nizi diğer taraflarca erişilebilir hale getirmek için NSG ve ilgili Azure bitiş noktası veya Erişim Denetim Listesi'ni (ACL) yapılandırmak olağandışı bir durum değildir. Bunu, kendi uygulama mantığınızın SQL Azure VM'nize bağlanmasına izin vermek için daha önce yapmış olma olasılığınız yüksektir. SQL Azure VM'nize Azure Bilişsel Arama bağlantısı için de durum farklı değildir. 

Aşağıdaki bağlantılarVM dağıtımları için NSG yapılandırması hakkında yönergeler sağlar. Bu yönergeleri, IP adresine dayalı bir Azure Bilişsel Arama bitiş noktasını ACL olarak kullanın.

> [!NOTE]
> Arka plan için, [ağ güvenlik grubu nedir bakın?](../virtual-network/security-overview.md)
> 
> 

* Kaynak **Yöneticisi** VM için ARM [dağıtımları için NSG'ler nasıl oluşturulur'](../virtual-network/tutorial-filter-network-traffic.md)ı görün. 
* **Klasik** VM için, [Klasik dağıtımlar için NSG'ler nasıl oluşturulur'](../virtual-network/virtual-networks-create-nsg-classic-ps.md)a bakın.

IP adresi, sorunun ve olası geçici çözüm çözümünün farkındaysanız kolayca aşabileceğiniz birkaç sorun oluşturabilir. Geri kalan bölümler, ACL'deki IP adresleriyle ilgili sorunları işlemek için öneriler sağlar.

#### <a name="restrict-access-to-the-azure-cognitive-search"></a>Azure Bilişsel Arama'ya erişimi kısıtlama
SQL Azure VM'lerinizi tüm bağlantı isteklerine açık hale getirmek yerine, arama hizmetinizin IP adresine ve ACL'deki IP adresi `AzureCognitiveSearch` [etiketine](https://docs.microsoft.com/azure/virtual-network/service-tags-overview#available-service-tags) erişimi kısıtlamanızı önemle tavsiye ediyoruz.

Arama hizmetinizin FQDN'sini (örneğin) `<your-search-service-name>.search.windows.net`pingleyerek IP adresini öğrenebilirsiniz.

[Indirilebilir JSON dosyalarını](https://docs.microsoft.com/azure/virtual-network/service-tags-overview#discover-service-tags-by-using-downloadable-json-files) kullanarak `AzureCognitiveSearch` veya Service Tag Discovery [API](https://docs.microsoft.com/azure/virtual-network/service-tags-overview#use-the-service-tag-discovery-api-public-preview)aracılığıyla hizmet [etiketinin](https://docs.microsoft.com/azure/virtual-network/service-tags-overview#available-service-tags) IP adres aralığını öğrenebilirsiniz. IP adres aralığı haftalık olarak güncellenir.

#### <a name="managing-ip-address-fluctuations"></a>IP adresi dalgalanmalarını yönetme
Arama hizmetinizde yalnızca bir arama birimi (yani bir yineleme ve bir bölüm) varsa, IP adresi rutin hizmetyeniden başlatma sırasında değişir ve mevcut bir ACL'yi arama hizmetinizin IP adresiyle geçersiz kılınacaktır.

Sonraki bağlantı hatasını önlemenin bir yolu, Azure Bilişsel Arama'da birden fazla yineleme ve bir bölüm kullanmaktır. Bunu yapmak maliyeti artırır, ancak IP adresi sorununu da çözer. Azure Bilişsel Arama'da, birden fazla arama biriminiz olduğunda IP adresleri değişmez.

İkinci bir yaklaşım, bağlantının başarısız olmasını sağlamak ve NSG'deki ALA'ları yeniden yapılandırmaktır. Ortalama olarak, IP adreslerinin birkaç haftada bir değişmesini bekleyebilirsiniz. Denetimli dizin oluşturmayı seyrek olarak yapan müşteriler için bu yaklaşım uygulanabilir olabilir.

Üçüncü bir uygulanabilir (ancak özellikle güvenli olmayan) yaklaşım, arama hizmetinizin sağlandığı Azure bölgesinin IP adresi aralığını belirtmektir. Ortak IP adreslerinin Azure kaynaklarına tahsis edildiği IP aralıklarının listesi [Azure Veri Merkezi IP aralıklarında](https://www.microsoft.com/download/details.aspx?id=41653)yayımlanır. 

#### <a name="include-the-azure-cognitive-search-portal-ip-addresses"></a>Azure Bilişsel Arama portalı IP adreslerini ekleyin
Bir dizin oluşturabilirsiniz, Azure Bilişsel Arama portalı mantığının da oluşturma sırasında SQL Azure VM'nize erişmesi gerekir. Azure Bilişsel Arama portalı IP adresleri ping `stamp2.search.ext.azure.com`ile bulunabilir.

## <a name="next-steps"></a>Sonraki adımlar
Yapılandırmanın önüne çıkmasıyla, azure vm'de bir SQL Server'ı azure bilişsel arama dizinleyicisinin veri kaynağı olarak belirtebilirsiniz. Daha fazla bilgi için [dizin oluşturmayı kullanarak Azure SQL Veritabanını Azure Bilişsel Arama'ya bağlama](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md) ya da birleştirme bilgisine bakın.

