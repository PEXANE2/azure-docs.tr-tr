---
title: Azure'da Windows VM'lerde SQL Server ile Key Vault'u tümleştirin (Kaynak Yöneticisi) | Microsoft Dokümanlar
description: Azure Key Vault ile kullanılmak üzere SQL Server şifreleme yapılandırmasını nasıl otomatikleştirebilirsiniz öğrenin. Bu konu, Kaynak Yöneticisi ile oluşturulan SQL Server sanal makineleri ile Azure Key Vault Tümleştirmesi'nin nasıl kullanılacağını açıklar.
services: virtual-machines-windows
documentationcenter: ''
author: MashaMSFT
manager: craigg
editor: ''
tags: azure-service-management
ms.assetid: cd66dfb1-0e9b-4fb0-a471-9deaf4ab4ab8
ms.service: virtual-machines-sql
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 04/30/2018
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: cad70169e88e1fafa129c02f30d5288d39e30a9c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "70102144"
---
# <a name="configure-azure-key-vault-integration-for-sql-server-on-azure-virtual-machines-resource-manager"></a>Azure Sanal Makineler’de SQL Server için Azure Key Vault Tümleştirmesi Yapılandırma (Resource Manager)

> [!div class="op_single_selector"]
> * [Kaynak Yöneticisi](virtual-machines-windows-ps-sql-keyvault.md)
> * [Klasik](../sqlclassic/virtual-machines-windows-classic-ps-sql-keyvault.md)

## <a name="overview"></a>Genel Bakış
[Saydam veri şifreleme (TDE)](https://msdn.microsoft.com/library/bb934049.aspx), [sütun düzeyi şifreleme (CLE)](https://msdn.microsoft.com/library/ms173744.aspx)ve [yedekleme şifreleme](https://msdn.microsoft.com/library/dn449489.aspx)gibi birden çok SQL Server şifreleme özelliği vardır. Bu şifreleme biçimleri, şifreleme için kullandığınız şifreleme anahtarlarını yönetmenizi ve depolamanızı gerektirir. Azure Key Vault (AKV) hizmeti, bu anahtarların güvenliğini ve yönetimini güvenli ve yüksek kullanılabilir bir konumda geliştirmek için tasarlanmıştır. [SQL Server Bağlayıcısı,](https://www.microsoft.com/download/details.aspx?id=45344) SQL Server'ın bu anahtarları Azure Key Vault'tan kullanmasını sağlar.

Sql Server'ı şirket içi makinelerle çalıştırıyorsanız, [şirket içi SQL Server makinenizden Azure Key Vault'a erişmek için izleyebileceğiniz adımlar](https://msdn.microsoft.com/library/dn198405.aspx)vardır. Ancak Azure VM'lerde SQL Server için *Azure Anahtar Kasası Tümleştirme* özelliğini kullanarak zamandan tasarruf edebilirsiniz.

Bu özellik etkinleştirildiğinde, SQL Server Bağlayıcısı'nı otomatik olarak yükler, EKM sağlayıcısını Azure Key Vault'a erişmek üzere yapılandırır ve kasanıza erişmenizi sağlayacak kimlik bilgilerini oluşturur. Daha önce bahsedilen şirket içi belgelerdeki adımlara bakarsanız, bu özelliğin 2 ve 3 adımlarını otomatikleştirdiğini görebilirsiniz. Hala el ile yapmanız gereken tek şey anahtar tonoz ve anahtarları oluşturmaktır. Buradan, SQL VM'nizin tüm kurulumu otomatiktir. Bu özellik bu kurulumu tamamladıktan sonra, veritabanlarınızı veya yedeklemelerinizi normalde yaptığınız gibi şifrelemeye başlamak için T-SQL deyimlerini yürütebilirsiniz.

[!INCLUDE [AKV Integration Prepare](../../../../includes/virtual-machines-sql-server-akv-prepare.md)]

  >[!NOTE]
  > EKM Sağlayıcı sürüm 1.0.4.0 [SQL IaaS uzantısı](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-agent-extension)üzerinden SQL Server VM'de yüklenir. SQL IaaS Uzantısı yükseltme sağlayıcı sürümünü güncelleştirmez. Lütfen gerekirse EKM sağlayıcı sürümünü el ile yükseltmeyi düşünün (örneğin, SQL Yönetilen Örnek'e geçiş yaparken).


## <a name="enabling-and-configuring-akv-integration"></a>AKV tümleştirmesini etkinleştirme ve yapılandırma
Sağlama sırasında AKV tümleştirmesini etkinleştirebilir veya varolan VM'ler için yapılandırabilirsiniz.

### <a name="new-vms"></a>Yeni VM'ler
Kaynak Yöneticisi ile yeni bir SQL Server sanal makine sağlıyorsanız, Azure portalı Azure Anahtar Kasası tümleştirmesini etkinleştirmek için bir yol sağlar. Azure Key Vault özelliği yalnızca SQL Server'ın Kurumsal, Geliştirici ve Değerlendirme Sürümleri için kullanılabilir.

![SQL Azure Anahtar Kasası Tümleştirme](./media/virtual-machines-windows-ps-sql-keyvault/azure-sql-arm-akv.png)

Ayrıntılı bir sağlama bölümü için Azure [portalında SQL Server sanal makinesi sağlama bölümüne](virtual-machines-windows-portal-sql-server-provision.md)bakın.

### <a name="existing-vms"></a>Varolan VM'ler

[!INCLUDE [windows-virtual-machines-sql-use-new-management-blade](../../../../includes/windows-virtual-machines-sql-new-resource.md)]

Mevcut SQL Server sanal makineleri için [SQL sanal makine kaynağınızı](virtual-machines-windows-sql-manage-portal.md#access-the-sql-virtual-machines-resource) açın ve **Ayarlar**altında **Güvenlik'i** seçin. Azure Anahtar Kasası tümleştirmesini etkinleştirmek için **Etkinleştir'i** seçin. 

![Mevcut VM'ler için SQL AKV Tümleştirmesi](./media/virtual-machines-windows-ps-sql-keyvault/azure-sql-rm-akv-existing-vms.png)

Bittiğinde, değişikliklerinizi kaydetmek için **Güvenlik** sayfasının altındaki **Uygula** düğmesini seçin.

> [!NOTE]
> Burada oluşturduğumuz kimlik bilgisi adı daha sonra bir SQL girişine eşlenecektir. Bu, SQL girişinin anahtar kasasına erişmesini sağlar. 


> [!NOTE]
> Akv tümleştirmeyi bir şablon kullanarak da yapılandırabilirsiniz. Daha fazla bilgi için Azure [Anahtar Kasası tümleştirmesi için Azure quickstart şablonuna](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-sql-existing-keyvault-update)bakın.


[!INCLUDE [AKV Integration Next Steps](../../../../includes/virtual-machines-sql-server-akv-next-steps.md)]
