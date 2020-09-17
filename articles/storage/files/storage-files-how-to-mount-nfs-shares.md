---
title: Azure NFS dosya paylaşma-Azure dosyaları bağlama
description: Ağ dosya sistemi paylaşımının nasıl bağlanacağını öğrenin.
author: roygara
ms.service: storage
ms.topic: how-to
ms.date: 09/15/2020
ms.author: rogarana
ms.subservice: files
ms.custom: references_regions
ms.openlocfilehash: 530ae82720e6b4eb6a3e4d1021c0b37b9f4dbf5c
ms.sourcegitcommit: 7374b41bb1469f2e3ef119ffaf735f03f5fad484
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/16/2020
ms.locfileid: "90707450"
---
# <a name="how-to-mount-an-nfs-file-share"></a>NFS dosya paylaşımından bağlama

[Azure Dosyaları](storage-files-introduction.md), Windows'un kolay kullanılan bulut dosya sistemidir. Azure dosya paylaşımları, sunucu Ileti bloğu Protokolü (SMB) veya ağ dosya sistemi (NFS) protokolü kullanılarak Linux dağıtımları ile bağlanabilir. Bu makale, NFS ile bağlama ile, SMB ile bağlama hakkında daha fazla bilgi için bkz. [Linux Ile Azure dosyaları kullanma](storage-how-to-use-files-linux.md). Kullanılabilir protokollerin her biri hakkında ayrıntılı bilgi için bkz. [Azure dosya paylaşma protokolleri](storage-files-compare-protocols.md).

## <a name="limitations"></a>Sınırlamalar

[!INCLUDE [files-nfs-limitations](../../../includes/files-nfs-limitations.md)]

### <a name="regional-availability"></a>Bölgesel kullanılabilirlik

[!INCLUDE [files-nfs-regional-availability](../../../includes/files-nfs-regional-availability.md)]

## <a name="prerequisites"></a>Önkoşullar

- [BIR NFS paylaşma oluşturun](storage-files-how-to-create-nfs-shares.md).

    > [!IMPORTANT]
    > NFS paylaşımlarına yalnızca güvenilen ağlardan erişilebilir. NFS paylaşımınızla kurulan bağlantılar aşağıdaki kaynaklardan birinden kaynaklanmalıdır:

- Aşağıdaki ağ çözümlerinden birini kullanın:
    - [Özel uç nokta oluşturun](storage-files-networking-endpoints.md#create-a-private-endpoint) (önerilir) ya da [genel uç noktanıza erişimi kısıtlayın](storage-files-networking-endpoints.md#restrict-public-endpoint-access).
    - [Azure dosyaları ile kullanmak üzere Linux üzerinde Noktadan siteye (P2S) VPN yapılandırın](storage-files-configure-p2s-vpn-linux.md).
    - [Azure dosyaları ile kullanmak Için siteden sıteye VPN yapılandırın](storage-files-configure-s2s-vpn.md).
    - [ExpressRoute](../../expressroute/expressroute-introduction.md)'ı yapılandırın.

## <a name="disable-secure-transfer"></a>Güvenli aktarımı devre dışı bırak

1. Azure portal oturum açın ve oluşturduğunuz NFS paylaşımının bulunduğu depolama hesabına erişin.
1. **Yapılandırma**' yı seçin.
1. **Güvenli aktarım**Için **devre dışı** seçeneğini belirleyin.
1. **Kaydet**’i seçin.

    :::image type="content" source="media/storage-files-how-to-mount-nfs-shares/storage-account-disable-secure-transfer.png" alt-text="Güvenli aktarım devre dışı olan depolama hesabı yapılandırma ekranının ekran görüntüsü.":::

## <a name="mount-an-nfs-share"></a>NFS paylaşma bağlama

1. Dosya paylaşma oluşturulduktan sonra, paylaşma ' yı seçin ve **Linux 'Tan Bağlan**' ı seçin.
1. Kullanmak istediğiniz bağlama yolunu girip betiği kopyalayın.
1. İstemcinizin bağlantısını yapın ve belirtilen bağlama betiğini kullanın.

    :::image type="content" source="media/storage-files-how-to-create-mount-nfs-shares/mount-nfs-file-share-script.png" alt-text="Dosya paylaşma bağlantı dikey penceresinin ekran görüntüsü":::

Şimdi NFS paylaşımınızı taktıysanız.

## <a name="next-steps"></a>Sonraki adımlar

- Makalemize sahip Azure dosyaları hakkında daha fazla bilgi edinin ve [Azure dosyaları dağıtımı Için planlama](storage-files-planning.md)yapın.
- Herhangi bir sorunla karşılaşırsanız bkz. [Azure NFS dosya paylaşımları sorunlarını giderme](storage-troubleshooting-files-nfs.md).