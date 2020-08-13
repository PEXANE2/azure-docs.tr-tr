---
title: Azure Blob depolamada ağ dosya sistemi 3,0 desteği (Önizleme) | Microsoft Docs
description: Blob Storage artık ağ dosya sistemi (NFS) 3,0 protokolünü desteklemektedir. Bu destek, Windows ve Linux istemcilerinin bir Azure sanal makinesinden (VM) veya şirket içinde çalışan bir bilgisayardan BLOB depolama alanına bir kapsayıcı takmalarını sağlar.
author: normesta
ms.subservice: blobs
ms.service: storage
ms.topic: conceptual
ms.date: 08/04/2020
ms.author: normesta
ms.reviewer: yzheng
ms.custom: references_regions
ms.openlocfilehash: 41ad20de6b7a800ff1f97a7ff371c8e0012fed27
ms.sourcegitcommit: c28fc1ec7d90f7e8b2e8775f5a250dd14a1622a6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2020
ms.locfileid: "88166986"
---
# <a name="network-file-system-nfs-30-protocol-support-in-azure-blob-storage-preview"></a>Azure Blob depolamada ağ dosya sistemi (NFS) 3,0 protokol desteği (Önizleme)

Blob Storage artık ağ dosya sistemi (NFS) 3,0 protokolünü desteklemektedir. Bu destek, Windows veya Linux istemcilerinin bir Azure sanal makinesi (VM) veya şirket içi bir bilgisayardan BLOB depolama alanına bir kapsayıcı takmalarını sağlar. 

> [!NOTE]
> Azure Blob depolamada NFS 3,0 protokol desteği genel önizlemeye sunuldu ve şu bölgelerde kullanılabilir: ABD Doğu, ABD Orta, ABD Orta Batı, Avustralya Güneydoğu, Kuzey Avrupa, UK Batı, Kore Orta, Kore Güney ve Kanada Orta.

## <a name="general-workflow-mounting-a-storage-account-container"></a>Genel iş akışı: depolama hesabı kapsayıcısı bağlama

Bir depolama hesabı kapsayıcısını bağlamak için bu işlemleri yapmanız gerekir.

1. Aboneliğiniz ile NFS 3,0 protokol özelliğini kaydedin.

2. Özelliğin kayıtlı olduğunu doğrulayın.

3. Bir Azure sanal ağı (VNet) oluşturun.

4. Ağ güvenliğini yapılandırın.

5. Yalnızca VNet 'ten trafiği kabul eden depolama hesabı oluşturun ve yapılandırın.

6. Depolama hesabında bir kapsayıcı oluşturun.

7. Kapsayıcıyı bağlayın.

Adım adım yönergeler için bkz. [ağ dosya sistemi (NFS) 3,0 protokolünü (Önizleme) kullanarak blob depolamayı bağlama](network-file-system-protocol-support-how-to.md).

> [!IMPORTANT]
> Bu görevleri sırayla gerçekleştirmek önemlidir. Hesabınızda NFS 3,0 protokolünü etkinleştirmeden önce oluşturduğunuz kapsayıcıları bağlayamıyoruz. Ayrıca, hesabınızda NFS 3,0 protokolünü etkinleştirdikten sonra devre dışı bırakabilirsiniz.

## <a name="network-security"></a>Ağ güvenliği

Depolama hesabınız bir sanal ağ içinde yer almalıdır. VNet, istemcilerin depolama hesabınıza güvenli bir şekilde bağlanmasını sağlar. Hesabınızdaki verileri güvenli hale getirmenin tek yolu, VNet ve diğer ağ güvenlik ayarlarını kullanmaktır. Hesap anahtarı yetkilendirme, Azure Active Directory (AD) güvenliği ve erişim denetim listeleri (ACL 'Ler) dahil olmak üzere verilerin güvenliğini sağlamak için kullanılan diğer herhangi bir araç, NFS 3,0 protokol desteği etkinleştirilmiş hesaplarda henüz desteklenmemektedir. 

Daha fazla bilgi için bkz. [BLOB depolama Için ağ güvenlik önerileri](security-recommendations.md#networking).

## <a name="supported-network-connections"></a>Desteklenen ağ bağlantıları

Bir istemci ortak veya [özel bir uç nokta](../common/storage-private-endpoints.md)üzerinden bağlanabilir ve aşağıdaki ağ konumlarından herhangi birinden bağlanabilir:

- Depolama hesabınız için yapılandırdığınız VNet. 

  Bu makalenin amacı doğrultusunda bu sanal ağa *birincil VNET*olarak başvuracağız. Daha fazla bilgi için bkz. [bir sanal ağ üzerinden erişim verme](../common/storage-network-security.md#grant-access-from-a-virtual-network).

- Birincil VNet ile aynı bölgedeki eşlenmiş bir sanal ağ.

  Bu eşlenen VNet 'e erişime izin vermek için depolama hesabınızı yapılandırmanız gerekecektir. Daha fazla bilgi için bkz. [bir sanal ağ üzerinden erişim verme](../common/storage-network-security.md#grant-access-from-a-virtual-network).

- [VPN Gateway](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways) veya [ExpressRoute ağ geçidi](https://docs.microsoft.com/azure/expressroute/expressroute-howto-add-gateway-portal-resource-manager)kullanarak birincil VNET 'iniz ile bağlantılı bir şirket içi ağ. 

  Daha fazla bilgi edinmek için bkz. Şirket [içi ağlardan erişimi yapılandırma](../common/storage-network-security.md#configuring-access-from-on-premises-networks).

- Eşlenmiş ağa bağlı bir şirket içi ağ.

  Bu işlem, [ağ geçidi](https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/vnet-peering#gateway-transit)aktarımlarıyla birlikte [VPN Gateway](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways) veya [ExpressRoute ağ geçidi](https://docs.microsoft.com/azure/expressroute/expressroute-howto-add-gateway-portal-resource-manager) kullanılarak yapılabilir. 

> [!IMPORTANT]
> Şirket içi bir ağdan bağlanıyorsanız, istemcinizin 111 ve 2048 bağlantı noktaları üzerinden giden iletişime izin verdiğinden emin olun. NFS 3,0 protokolü bu bağlantı noktalarını kullanır.

## <a name="azure-storage-features-not-yet-supported"></a>Azure depolama özellikleri henüz desteklenmiyor

Hesabınızda NFS 3,0 protokolünü etkinleştirdiğinizde aşağıdaki Azure depolama özellikleri desteklenmez. 

- Azure Active Directory (AD) güvenlik

- POSIX benzeri erişim denetim listeleri (ACL 'Ler)

- Mevcut depolama hesaplarında NFS 3,0 desteğini etkinleştirebilme

- NFS 3,0 desteğini bir depolama hesabında devre dışı bırakma özelliği (etkinleştirildikten sonra)

- REST API 'Leri veya SDK 'ları kullanarak bloblara yazma özelliği. 
  
## <a name="nfs-30-features-not-yet-supported"></a>NFS 3,0 özellikleri henüz desteklenmiyor

Aşağıdaki NFS 3,0 özellikleri henüz Azure Data Lake Storage 2. desteklenmez.

- UDP üzerinden NFS 3,0. Yalnızca TCP üzerinden NFS 3,0 desteklenir.

- Ağ kilitleme Yöneticisi (NLM) ile dosyaları kilitleme. Mount komutları `-o nolock` parametresini içermelidir.

- Alt dizinleri bağlama. Kök dizinini (kapsayıcı) yalnızca bağlayabilirsiniz.

- Bağlama listeleme (örneğin: komutunu kullanarak `showmount -a` )

- Dışarı aktarmaları listeleme (örneğin: komutunu kullanarak `showmount -e` )

- Kapsayıcıyı salt okunurdur dışa aktarma

## <a name="pricing"></a>Fiyatlandırma

Önizleme sırasında, depolama hesabınızda depolanan veriler aylık GB başına blob depolama ücretlerine göre aynı kapasite fiyatı üzerinden faturalandırılır. 

Önizleme sırasında bir işlem ücretlendirilmez. İşlemlere yönelik fiyatlandırma değişikliğe tabidir ve genel kullanıma sunulduğunda belirlenir.

## <a name="next-steps"></a>Sonraki adımlar

Başlamak için bkz. [ağ dosya sistemi (NFS) 3,0 protokolünü (Önizleme) kullanarak blob depolamayı bağlama](network-file-system-protocol-support-how-to.md).





