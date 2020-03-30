---
title: Azure'da SQL Server için Güvenlik Hususları | Microsoft Dokümanlar
description: Bu konu, Bir Azure Sanal Makine'de çalışan SQL Server güvenliğini sağlamak için genel kılavuz sağlar.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
manager: craigg
editor: ''
tags: azure-service-management
ms.assetid: d710c296-e490-43e7-8ca9-8932586b71da
ms.service: virtual-machines-sql
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 03/23/2018
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: f5ea0ddff38532b119d8d984f2dabd6d898b44a5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77031365"
---
# <a name="security-considerations-for-sql-server-in-azure-virtual-machines"></a>Azure Sanal Makineler'de SQL Server için Güvenlikle İlgili Dikkat Edilmesi Gerekenler

Bu konu, bir Azure sanal makinesinde (VM) SQL Server örneklerine güvenli erişim kurulmasına yardımcı olan genel güvenlik yönergelerini içerir.

Azure, sanal bir makinede çalışan SQL Server ile uyumlu bir çözüm oluşturmanıza olanak tanıyan çeşitli endüstri yönetmeliklerine ve standartlarına uygundur. Azure ile mevzuata uygunluk hakkında bilgi için [Azure Güven Merkezi'ne](https://azure.microsoft.com/support/trust-center/)bakın.

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="control-access-to-the-sql-vm"></a>SQL VM'ye erişimi kontrol etme

SQL Server sanal makinenizi oluştururken, makineye ve SQL Server'a kimlerin erişebildiğini nasıl dikkatle kontrol ettiğinizi düşünün. Genel olarak, aşağıdakileri yapmalısınız:

- SQL Server'a erişimi yalnızca gereksinim duyulan uygulama ve istemcilerle sınırlandırın.
- Kullanıcı hesaplarını ve parolaları yönetmek için en iyi uygulamaları izleyin.

Aşağıdaki bölümlerde bu noktalar üzerinden düşünme önerileri sağlar.

## <a name="secure-connections"></a>Güvenli bağlantılar

Bir galeri görüntüsü içeren bir SQL Server sanal makine oluşturduğunuzda, **SQL Server Bağlantı** seçeneği size Yerel **(VM içinde)**, **Özel (Sanal Ağ içinde)** veya **Genel (Internet)** seçeneğini sunar.

![SQL Server bağlantısı](./media/virtual-machines-windows-sql-security/sql-vm-connectivity-option.png)

En iyi güvenlik için senaryonuz için en kısıtlayıcı seçeneği seçin. Örneğin, aynı VM'de SQL Server'a erişen bir uygulama çalıştırıyorsanız, **Yerel** en güvenli seçimdir. SQL Server'a erişim gerektiren bir Azure uygulaması çalıştırıyorsanız, **Private** yalnızca belirtilen [Azure Sanal Ağı](../../../virtual-network/virtual-networks-overview.md)içinde SQL Server'a iletişimi güvence altına alar. SQL Server VM'ye **Ortak** (internet) erişimine ihtiyacınız varsa, saldırı yüzey alanınızı azaltmak için bu konudaki diğer en iyi uygulamaları takip ettiğinizden emin olun.

Portaldaki seçili seçenekler, sanal makinenize ağ trafiğine izin vermek veya reddetmek için VM'nin [ağ güvenlik grubunda](../../../virtual-network/security-overview.md) (NSG) gelen güvenlik kurallarını kullanır. SQL Server bağlantı noktasına (varsayılan 1433) trafik izni vermek için yeni gelen NSG kurallarını değiştirebilir veya oluşturabilirsiniz. Ayrıca, bu bağlantı noktası üzerinden iletişim kurmasına izin verilen belirli IP adreslerini de belirtebilirsiniz.

![Ağ güvenlik grubu kuralları](./media/virtual-machines-windows-sql-security/sql-vm-network-security-group-rules.png)

Ağ trafiğini kısıtlamak için NSG kurallarına ek olarak, sanal makinede Windows Güvenlik Duvarı'nı da kullanabilirsiniz.

Klasik dağıtım modeliyle uç noktaları kullanıyorsanız, kullanmazsanız sanal makinedeki uç noktaları kaldırın. Uç noktaları olan ACL'leri kullanma yla ilgili talimatlar için, [acl'yi bitiş noktasında yönet'e](/previous-versions/azure/virtual-machines/windows/classic/setup-endpoints#manage-the-acl-on-an-endpoint)bakın. Bu, Kaynak Yöneticisi'ni kullanan VM'ler için gerekli değildir.

Son olarak, Azure sanal makinenizde SQL Server Veritabanı Altyapısı örneğine göre şifreli bağlantıları etkinleştirmeyi düşünün. SQL sunucu örneğini imzalı bir sertifikayla yapılandırın. Daha fazla bilgi için veritabanı altyapısı ve [Bağlantı Dize Sözdizimine](https://msdn.microsoft.com/library/ms254500.aspx) [Şifreli Bağlantıları Etkinleştir'e](https://docs.microsoft.com/sql/database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine) bakın.

## <a name="encryption"></a>Şifreleme

Yönetilen diskler Sunucu Tarafı Şifrelemeve Azure Disk Şifrelemesi sunar. [Sunucu Tarafı](/azure/virtual-machines/windows/disk-encryption) Şifrelemesi, kuruluş güvenliği ve uyumluluk taahhütlerinizi karşılamak için verilerinizi sağlar ve verilerinizi korur. [Azure Disk Şifreleme,](/azure/security/fundamentals/azure-disk-encryption-vms-vmss) Bitlocker veya DM-Crypt teknolojisini kullanır ve hem işletim sistemi hem de veri disklerini şifrelemek için Azure Key Vault ile tümleşir. 

## <a name="use-a-non-default-port"></a>Varsayılan olmayan bir bağlantı noktası kullanma

Varsayılan olarak, SQL Server 1433 gibi iyi bilinen bir bağlantı noktasını dinler. Artırılmış güvenlik için SQL Server'ı 1401 gibi varsayılan olmayan bir bağlantı noktasında dinleyecek şekilde yapılandırın. Azure portalında bir SQL Server galeri görüntüsü sağlarsanız, bu bağlantı noktasını **SQL Server ayarları** bıçağında belirtebilirsiniz.

[!INCLUDE [windows-virtual-machines-sql-use-new-management-blade](../../../../includes/windows-virtual-machines-sql-new-resource.md)]

Bunu sağlamadan sonra yapılandırmak için iki seçeneğiniz var:

- Kaynak Yöneticisi VM'leri için [SQL sanal makineler kaynağından](virtual-machines-windows-sql-manage-portal.md#access-the-sql-virtual-machines-resource) **Güvenlik'i** seçebilirsiniz. Bu, bağlantı noktasını değiştirmek için bir seçenek sağlar.

  ![Portalda TCP bağlantı noktası değişikliği](./media/virtual-machines-windows-sql-security/sql-vm-change-tcp-port.png)

- Klasik VM'ler veya portalla birlikte sağlanmamış SQL Server VM'leri için, VM'ye uzaktan bağlanarak bağlantı noktasını el ile yapılandırabilirsiniz. Yapılandırma adımları için [bkz.](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-server-to-listen-on-a-specific-tcp-port) Bu el tekniğini kullanıyorsanız, bu TCP bağlantı noktasında gelen trafiğe izin vermek için bir Windows Güvenlik Duvarı kuralı eklemeniz gerekir.

> [!IMPORTANT]
> SQL Server bağlantı bağlantı bağlantı bağlantınızın genel internet bağlantılarına açık olup olmadığının varsayılan olmayan bir bağlantı noktası belirtilmesi iyi bir fikirdir.

SQL Server varsayılan olmayan bir bağlantı noktasında dinliyorsa, bağlandığınızda bağlantı noktasını belirtmeniz gerekir. Örneğin, sunucu IP adresinin 13.55.255.255 olduğu ve SQL Server'ın 1401 no'cbağlantı noktasında dinlediği bir senaryo düşünün. SQL Server'a bağlanmak için `13.55.255.255,1401` bağlantı dizesinde belirtirsiniz.

## <a name="manage-accounts"></a>Hesapları yönetme

Saldırganların hesap adlarını veya parolaları kolayca tahmin etmesini istemezsin. Yardım etmek için aşağıdaki ipuçlarını kullanın:

- **Yönetici**adında olmayan benzersiz bir yerel yönetici hesabı oluşturun.

- Tüm hesaplarınız için karmaşık güçlü parolalar kullanın. Güçlü bir parola oluşturma hakkında daha fazla bilgi için [bkz.](https://support.microsoft.com/instantanswers/9bd5223b-efbe-aa95-b15a-2fb37bef637d/create-a-strong-password)

- Varsayılan olarak Azure, SQL Server Virtual Machine kurulumu sırasında Windows Kimlik Doğrulaması'nı seçer. Bu nedenle, **SA** giriş devre dışı bırakılır ve bir parola kurulum tarafından atanır. **SA** girişinin kullanılmamasını veya etkinleştirilmemesini öneririz. SQL oturum açmanız gerekiyorsa, aşağıdaki stratejilerden birini kullanın:

  - **Sysadmin** üyeliği olan benzersiz bir ada sahip bir SQL hesabı oluşturun. Bunu, sağlama sırasında **SQL Kimlik Doğrulaması'nı** etkinleştirerek portaldan yapabilirsiniz.

    > [!TIP] 
    > Sağlama sırasında SQL Kimlik Doğrulamasını etkinleştirmezseniz, kimlik doğrulama modunu el ile **SQL Server ve Windows Kimlik Doğrulama Modu**olarak değiştirmeniz gerekir. Daha fazla bilgi için [Sunucu Kimlik Doğrulama Modunu Değiştir'e](https://docs.microsoft.com/sql/database-engine/configure-windows/change-server-authentication-mode)bakın.

  - **SA** oturum açma yı kullanmanız gerekiyorsa, sağlama dan sonra girişi etkinleştirin ve yeni bir güçlü parola atayın.

## <a name="additional-best-practices"></a>Diğer en iyi uygulamalar

Bu konuda açıklanan uygulamalara ek olarak, hem geleneksel şirket içi güvenlik uygulamalarının hem de sanal makine güvenliği en iyi uygulamalarının güvenlik en iyi uygulamalarını gözden geçirmenizi ve uygulamanızı öneririz. 

Şirket içi güvenlik uygulamaları hakkında daha fazla bilgi için, [SQL Server Yüklemesi](/sql/sql-server/install/security-considerations-for-a-sql-server-installation) ve [Güvenlik merkezi](/sql/relational-databases/security/security-center-for-sql-server-database-engine-and-azure-sql-database)için Güvenlik Hususları'na bakın. 

Sanal makine güvenliği hakkında daha fazla bilgi için [sanal makineler güvenlik genel bakış](/azure/security/fundamentals/virtual-machines-overview)bakın.


## <a name="next-steps"></a>Sonraki Adımlar

Performans la ilgili en iyi uygulamalarla da ilgileniyorsanız, [Azure Sanal Makinelerde SQL Server için En İyi Performans Uygulamaları'na](virtual-machines-windows-sql-performance.md)bakın.

Azure VM'lerde SQL Server'ı çalıştırmakla ilgili diğer konular için [Azure Sanal Makinelere genel bakışta SQL Server'a](virtual-machines-windows-sql-server-iaas-overview.md)bakın. SQL Server sanal makineleri hakkında sorularınız olursa [Sık Sorulan Sorular](virtual-machines-windows-sql-server-iaas-faq.md) bölümüne bakın.

