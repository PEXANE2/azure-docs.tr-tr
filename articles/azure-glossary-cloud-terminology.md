---
title: Azure sözlüğü - Azure sözlüğü | Microsoft Dokümanlar
description: Azure platformundaki bulut terminolojisini anlamak için Azure sözlüğünü kullanın. Bu kısa Azure sözlüğü, Azure için ortak bulut terimleri için tanımlar sağlar.
keywords: Azure sözlüğü, bulut terminolojisi, Azure sözlüğü, terminoloji tanımları, bulut terimleri
services: na
documentationcenter: na
author: MonicaRush
manager: jhubbard
editor: ''
ms.assetid: d7ac12f7-24b5-4bcd-9e4d-3d76fbd8d297
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/16/2017
ms.author: monicar
ms.openlocfilehash: 7c80ba6b15cec08da9bcefea243493a824ef66bd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79276484"
---
# <a name="microsoft-azure-glossary-a-dictionary-of-cloud-terminology-on-the-azure-platform"></a>Microsoft Azure sözlüğü: Azure platformunda bulut terminolojisi sözlüğü

Microsoft Azure sözlüğü, Azure platformu için bulut terminolojisinin kısa bir sözlüğüdür. Ayrıca bkz:

* [Microsoft Azure ve Amazon Web Hizmetleri](https://azure.microsoft.com/campaigns/azure-vs-aws/mapping/) - Azure hizmetlerinin ve bunların AWS benzerlerinin tanımları.<!-- I propose to link to https://azure.microsoft.com/services/ instead of this -->
* [Bulut bilgi işlem terimleri](https://azure.microsoft.com/overview/cloud-computing-dictionary/) - Genel endüstri bulut terimleri.

## <a name="account"></a>account
Azure aboneliğine erişmek ve yönetmek için kullanılan bir hesap. Bir hesap bunlardan biri olsa da genellikle Azure hesabı olarak adlandırılır: varolan bir çalışma, okul veya kişisel Microsoft hesabı veya Office 365 kullanıcı adı ve parola. [Ayrıca, ücretsiz deneme](https://azure.microsoft.com)sürümüne kaydolduğunuzda Azure aboneliğini yönetmek için bir hesap da oluşturabilirsiniz.  
Bkz. Office 365 hesabınız ve oturum açmak için [kullanabileceğiniz Hesaplar](active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md)ile Azure [aboneliğine kaydolun.](cost-management-billing/manage/office-365-account-for-azure-subscription.md)

## <a name="api-app"></a>API uygulaması
[App Service uygulaması](#app-service-app)için başka bir ad .

## <a name="app-service-app"></a>App Service uygulaması
[Azure Uygulama Hizmeti'nin](app-service/overview.md) bir web sitesi veya web uygulaması, web API veya [mobil uygulama arka ucunu](app-service-mobile/app-service-mobile-value-prop.md)barındırmak için sağladığı bilgi işlem kaynakları. App Service uygulamaları da *App Services,* *web uygulamaları,* *API uygulamaları*ve *mobil uygulamalar*olarak adlandırılır.

## <a name="availability-set"></a>kullanılabilirlik seti
Uygulama artıklığı ve güvenilirliği sağlamak için birlikte yönetilen sanal makineler topluluğu. Kullanılabilirlik kümesinin kullanımı, planlı veya planlanmamış bir bakım olayı sırasında en az bir sanal makinenin kullanılabilir olmasını sağlar.  
Bkz. [Windows sanal makinelerin kullanılabilirliğini yönetin](virtual-machines/windows/manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) ve [Linux sanal makinelerinin kullanılabilirliğini yönetin](virtual-machines/linux/manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="azure-classic-deployment-model"></a><a name="classic-model"></a>Azure klasik dağıtım modeli
Azure'da kaynakları dağıtmak için kullanılan iki [dağıtım modelinden](resource-manager-deployment-model.md) biri (yeni model Azure Kaynak Yöneticisi'dir). Bazı Azure hizmetleri yalnızca Kaynak Yöneticisi dağıtım modelini, bazıları yalnızca klasik dağıtım modelini destekler ve bazıları her ikisini de destekler. Her Azure hizmeti için belgeler, hangi modeli desteklediklerini belirtir.

## <a name="azure-command-line-interface-cli"></a><a name="cli"></a>Azure komut satırı arabirimi (CLI)
Windows, macOS ve Linux'tan Azure hizmetlerini yönetmek için kullanılabilen komut satırı arabirimi.  Bazı hizmetler veya hizmet özellikleri yalnızca PowerShell veya CLI üzerinden yönetilebilir. Bkz. [Azure CLI](/cli/azure)

## <a name="azure-powershell"></a><a name="powershell"></a>Azure PowerShell
Windows bilgisayarlarınkomut satırı üzerinden Azure hizmetlerini yönetmek için komut satırı arabirimi. Bazı hizmetler veya hizmet özellikleri yalnızca PowerShell veya CLI üzerinden yönetilebilir.
[Azure PowerShell'i nasıl yükleyip yapılandırın](/powershell/azure/overview)

## <a name="azure-resource-manager-deployment-model"></a><a name="arm-model"></a>Azure Kaynak Yöneticisi dağıtım modeli
Microsoft Azure'da kaynakları dağıtmak için kullanılan iki [dağıtım modelinden](resource-manager-deployment-model.md) biri (diğeri klasik dağıtım modelidir). Bazı Azure hizmetleri yalnızca Kaynak Yöneticisi dağıtım modelini, bazıları yalnızca klasik dağıtım modelini destekler ve bazıları her ikisini de destekler. Her Azure hizmeti için belgeler, hangi modeli desteklediklerini belirtir.

## <a name="fault-domain"></a>hata etki alanı
Sanal makinelerin aynı anda başarısız olabilecek bir kullanılabilirlik kümesinde toplanması. Bir örnek, raftaki ortak bir güç kaynağını ve ağ anahtarını paylaşan bir grup makinedir. Azure'da, kullanılabilirlik kümesindeki sanal makineler otomatik olarak birden çok hata etki alanı arasında ayrılır.  
Bkz. [Windows sanal makinelerin kullanılabilirliğini yönetin](virtual-machines/windows/manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) veya [Linux sanal makinelerinin kullanılabilirliğini yönetin](virtual-machines/linux/manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)  

## <a name="geo"></a>coğrafi
Genellikle iki veya daha fazla bölge içeren veri ihtisas için tanımlanmış bir sınır. Sınırlar ulusal sınırların içinde veya dışında olabilir ve vergi düzenlemesinden etkilenir. Her coğrafyanın en az bir bölgesi vardır. Asya Pasifik ve Japonya'ya örnek olarak asya pasifik ve japonya verilebilir. *Coğrafya*olarak da adlandırılır.  
Azure [Bölgelerine](best-practices-availability-paired-regions.md) Bakın

## <a name="geo-replication"></a>coğrafi çoğaltma
Bölgesel bir çift içindeki blobs, tablolar ve kuyruklar gibi içeriği otomatik olarak çoğaltma işlemi.  
[Azure SQL Veritabanı için Etkin Coğrafi Çoğaltma'ya](sql-database/sql-database-geo-replication-overview.md) bakın
<!-- The meaning of "geo" in this term seems to be different than the meaning provided in the "geo" entry -->

## <a name="image"></a>image
Herhangi bir sayıda sanal makine oluşturmak için kullanılabilecek işletim sistemi ve uygulama yapılandırmasını içeren bir dosya. Azure'da iki tür görüntü vardır: VM görüntüsü ve işletim sistemi görüntüsü. VM görüntüsü, görüntü oluşturulduğunda bir işletim sistemi ve sanal makineye bağlı tüm diskler içerir. İşletim sistemi görüntüsü, yalnızca veri diski yapılandırmaları olmayan genelleştirilmiş bir işletim sistemi içerir.  
[PowerShell veya CLI ile Azure'da Gezinme ve Windows sanal makine resimlerini seçme](virtual-machines/windows/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="limits"></a>Sınır -ları
Oluşturulabilecek kaynak sayısı veya elde edilebilen performans ölçütü. Sınırlar genellikle abonelikler, hizmetler ve tekliflerle ilişkilidir.  
Bkz. [Azure abonelik ve hizmet sınırları, kotalar ve kısıtlamalar](azure-resource-manager/management/azure-subscription-service-limits.md)

## <a name="load-balancer"></a>yük dengeleyici
Ağdaki bilgisayarlar arasında gelen trafiği dağıtan bir kaynak. Azure'da yük dengeleyicisi trafiği yük dengeleyici kümesinde tanımlanan sanal makinelere dağıtır. Bir [yük dengeleyici](load-balancer/load-balancer-overview.md) internete bakan olabilir, ya da dahili olabilir.  

## <a name="mobile-app"></a>mobil uygulama
[App Service App](#app-service-app)için başka bir isim .

## <a name="offer"></a>teklif
Bir Azure aboneliği için geçerli olan fiyatlandırma, kredi ve ilgili koşullar.  
Azure [teklif ayrıntıları sayfasına](https://azure.microsoft.com/support/legal/offer-details/) bakın

## <a name="portal"></a>portal
Azure hizmetlerini dağıtmak ve yönetmek için kullanılan güvenli web portalı.

## <a name="region"></a>region
Ulusal sınırları geçmeyen ve bir veya daha fazla veri merkezi içeren bir jeo içindeki alan. Fiyatlandırma, bölgesel hizmetler ve teklif türleri bölge düzeyinde ortaya alınır. Bir bölge genellikle birkaç yüz mil uzakta olabilir başka bir bölge ile eşlenir. Bölgesel çiftler, olağanüstü durum kurtarma ve yüksek kullanılabilirlik senaryoları için bir mekanizma olarak kullanılabilir. *Ayrıca konum*olarak anılacaktır.  
Azure [Bölgelerine](best-practices-availability-paired-regions.md) Bakın

## <a name="resource"></a>kaynak
Azure çözümünüzün bir parçası olan bir öğe. Her Azure hizmeti, veritabanları veya sanal makineler gibi farklı kaynak türlerini dağıtmanıza olanak tanır.   
Bkz. [Azure Kaynak Yöneticisi'ne genel bakış](azure-resource-manager/management/overview.md)

## <a name="resource-group"></a>kaynak grubu
Kaynak Yöneticisi'nde, bir uygulama için ilgili kaynakları tutan bir kapsayıcı. Kaynak grubu, bir uygulama için tüm kaynakları veya yalnızca mantıksal olarak birlikte gruplandırılan kaynakları içerebilir. Kuruluş için önemli olan faktörleri temel alarak kaynakları kaynak gruplarına nasıl ayıracağınızı belirleyebilirsiniz.  
Bkz. [Azure Kaynak Yöneticisi'ne genel bakış](azure-resource-manager/management/overview.md)

## <a name="resource-manager-template"></a><a name="arm-template"></a>Kaynak Yöneticisi şablonu
Bir veya daha fazla Azure kaynağını bildirimsel olarak tanımlayan ve dağıtılan kaynaklar arasındaki bağımlılıkları tanımlayan bir JSON dosyası. Şablon, kaynakları tutarlı ve sürekli olarak dağıtmak için kullanılabilir.  
Bkz. [Azure Kaynak Yöneticisi şablonları yazma](resource-group-authoring-templates.md)

## <a name="resource-provider"></a>kaynak sağlayıcısı
Kaynak Yöneticisi aracılığıyla dağıtabileceğiniz ve yönetebileceğiniz kaynakları sağlayan bir hizmet. Her kaynak sağlayıcısı dağıtılan kaynaklarla çalışmaya yönelik işlemler sunar. Kaynak sağlayıcılarına Azure portalı, Azure PowerShell ve çeşitli programlama SDK'ları aracılığıyla erişilebilir.  
Bkz. [Azure Kaynak Yöneticisi'ne genel bakış](azure-resource-manager/management/overview.md)

## <a name="role"></a>rol
Kullanıcılara, gruplara ve hizmetlere atanabilecek erişimi denetleme aracı. Roller, Azure kaynakları oluşturma, yönetme ve okuma gibi eylemleri gerçekleştirebilir.  
Bkz. [RBAC: Yerleşik roller](role-based-access-control/built-in-roles.md)

## <a name="service-level-agreement-sla"></a><a name="sla"></a>hizmet düzeyi sözleşmesi (SLA)
Microsoft'un çalışma süresi ve bağlantı için taahhütlerini açıklayan anlaşma. Her Azure hizmetinin belirli bir SLA'sı vardır.  
[Hizmet Düzeyi Sözleşmelerine](https://azure.microsoft.com/support/legal/sla/) Bakın

## <a name="shared-access-signature-sas"></a><a name="sas"></a>paylaşılan erişim imzası (SAS)
Hesap anahtarınızı açığa çıkarmadan kaynağa sınırlı erişim hakkı tanıyan imza. Örneğin, Azure Depolama istemciye blobs gibi nesnelere erişim izni vermek için [SAS kullanır.](storage/common/storage-dotnet-shared-access-signature-part-1.md) [IoT Hub,](iot-hub/iot-hub-devguide-security.md#security-tokens) cihazlara telemetri gönderme izni vermek için SAS kullanır.

## <a name="storage-account"></a>depolama hesabı
Azure Depolama'da Azure Blob, Sıra, Tablo ve Dosya hizmetlerine erişmenizi sağlayan bir hesap. Depolama hesabı adı, Azure Depolama veri nesneleri için benzersiz ad alanını tanımlar.  
Azure [depolama hesapları hakkında bkz.](storage/common/storage-create-storage-account.md)

## <a name="subscription"></a>aboneliği
Bir müşterinin Microsoft ile Azure hizmetleri edinmelerini sağlayan anlaşması. Abonelik fiyatlandırması ve ilgili koşullar, abonelik için seçilen teklife tabidir.
[Bkz. Microsoft Çevrimiçi Abonelik Sözleşmesi](https://azure.microsoft.com/support/legal/subscription-agreement/) ve Azure [abonelikleri Azure Etkin Dizini ile nasıl ilişkilendirilir?](active-directory/fundamentals/active-directory-how-subscriptions-associated-directory.md)

## <a name="tag"></a>etiket
Kaynakları yönetme veya faturalandırma gereksinimlerinize göre kategorilere ayırmanızı sağlayan bir dizin oluşturma terimi. Karmaşık bir kaynak koleksiyonunuz olduğunda, etiketleri bu varlıkları en mantıklı şekilde görselleştirmek için kullanabilirsiniz. Örneğin, kuruluşunuzda benzer görevleri üstlenen veya aynı departmana ait olan kaynakları etiketleyebilirsiniz.  
Bkz. [Azure kaynaklarınızı düzenlemek için etiketleri](resource-group-using-tags.md) kullanma

## <a name="update-domain"></a>etki alanını güncelleştir
Sanal makinelerin aynı anda güncelleştirilen bir kullanılabilirlik kümesinde toplanması. Aynı güncelleştirme etki alanında sanal makineler, planlı bakım sırasında birlikte yeniden başlatılır. Azure, aynı anda birden fazla güncelleştirme etki alanını yeniden başlatmaz. Yükseltme etki alanı olarak da adlandırılır.  
Bkz. [Windows sanal makinelerin kullanılabilirliğini yönetin](virtual-machines/windows/manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) ve [Linux sanal makinelerinin kullanılabilirliğini yönetin](virtual-machines/linux/manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="virtual-machine"></a><a name="vm"></a>sanal makine
İşletim sistemini çalıştıran fiziksel bir bilgisayarın yazılım uygulaması. Aynı donanımda aynı anda birden çok sanal makine çalıştırılabilir. Azure'da sanal makineler çeşitli boyutlarda kullanılabilir.  
Bkz. [Sanal Makineler belgeleri](https://azure.microsoft.com/documentation/services/virtual-machines/)

## <a name="virtual-machine-extension"></a><a name="vm-extension"></a>sanal makine uzantısı
Diğer programların çalışmasına yardımcı olan veya çalışan bir bilgisayarla etkileşim kurmanıza yardımcı olan davranışlar veya özellikler uygulayan bir kaynak. Örneğin, Bir Azure sanal makinesindeki uzak erişim değerlerini sıfırlamak veya değiştirmek için VM Access uzantısını kullanabilirsiniz.
<!-- This definition seems obscure to me; maybe a list of examples would work better than a conceptual definition? -->
Bkz. [Sanal makine uzantıları ve özellikleri (Windows)](virtual-machines/windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) veya Sanal makine uzantıları ve özellikleri hakkında [(Linux)](virtual-machines/linux/extensions-features.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="virtual-network"></a><a name="vnet"></a>sanal ağ
Azure kaynaklarınız arasında diğer tüm Azure kiracılarından yalıtılmış bağlantı sağlayan bir ağ. [Azure VPN Ağ Geçidi,](vpn-gateway/vpn-gateway-about-vpngateways.md) sanal ağlar arasında ve sanal ağ ile şirket içi ağ arasında bağlantılar kurmanıza olanak tanır. Sanal ağdaki IP adres bloklarını, DNS ayarlarını, güvenlik ilkelerini ve rota tablolarını tam olarak denetleyebilirsiniz.  
[Bkz. Sanal Ağa Genel Bakış](virtual-network/virtual-networks-overview.md)  

## <a name="web-app"></a>Web uygulaması
[App Service App](#app-service-app)için başka bir isim .

## <a name="see-also"></a>Ayrıca bkz.

* [Azure’u kullanmaya başlama](https://azure.microsoft.com/get-started/)
* [Bulut kaynak merkezi](https://azure.microsoft.com/resources/)  
* [İş uygulamanız için Azure](https://azure.microsoft.com/overview/business-apps-on-azure/)
* [Veri merkezinizde Azure](https://azure.microsoft.com/overview/business-apps-on-azure/)

