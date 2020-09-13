---
title: Azure NetApp Files kullanan çözüm mimarileri | Microsoft Docs
description: Azure NetApp Files kullanarak çözüm mimarileri için en iyi yöntemlere başvurular sağlar.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/08/2020
ms.author: b-juche
ms.openlocfilehash: ca17dccd8b17a2e0ff7e9b960c74b4c5a36018f5
ms.sourcegitcommit: d0541eccc35549db6381fa762cd17bc8e72b3423
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/09/2020
ms.locfileid: "89567328"
---
# <a name="solution-architectures-using-azure-netapp-files"></a>Azure NetApp Files kullanan çözüm mimarileri
Bu makalede, Azure NetApp Files kullanmaya yönelik çözüm mimarilerini anlamanıza yardımcı olabilecek en iyi yöntemlere başvurular sağlanmaktadır.  

Aşağıdaki diyagramda Azure NetApp Files sunduğu çözüm mimarilerinin kategorileri özetlenmektedir:

![Çözüm mimarisi kategorileri](../media/azure-netapp-files/solution-architecture-categories.png)

## <a name="linux-oss-apps-and-database-solutions"></a>Linux OSS uygulamaları ve veritabanı çözümleri

Bu bölüm, Linux OSS uygulamalarına ve veritabanlarına yönelik çözümler için başvurular sağlar. 

### <a name="oracle"></a>Oracle

* [Azure dağıtım Azure NetApp Files kullanarak en iyi Yöntem Kılavuzu](https://www.netapp.com/us/media/tr-4780.pdf)
* [Oracle VM görüntüleri ve bunların Microsoft Azure dağıtımı: paylaşılan depolama yapılandırma seçenekleri](https://docs.microsoft.com/azure/virtual-machines/workloads/oracle/oracle-vm-solutions#shared-storage-configuration-options)
* [Oracle Database ile Azure NetApp Files kullanmanın avantajları](solutions-benefits-azure-netapp-files-oracle-database.md)

## <a name="windows-apps-and-sql-server-solutions"></a>Windows Uygulamaları ve SQL Server çözümleri

Bu bölüm, Windows Uygulamaları ve SQL Server çözümleri için başvurular sağlar.

### <a name="file-sharing-and-global-file-caching"></a>Dosya paylaşımı ve genel dosya önbelleğe alma

* [Kendi Azure NFS 'Nizi oluşturun mi? Linux dosya paylaşımlarını buluta paketleyin](https://cloud.netapp.com/blog/ma-anf-blg-build-your-own-linux-nfs-file-shares)
* [Genel dosya önbelleği/Azure NetApp Files dağıtımı](https://youtu.be/91LKb1qsLIM)

### <a name="sql-server"></a>SQL Server

* [Azure NetApp Files SMB üzerinden SQL Server dağıtma](https://www.youtube.com/watch?v=x7udfcYbibs)
<!-- * [Deploy SQL Server Always-On Failover Cluster over SMB with Azure NetApp Files](https://www.youtube.com/watch?v=zuNJ5E07e8Q) --> 
<!-- * [Deploy Always-On Availability Groups with Azure NetApp Files](https://www.youtube.com/watch?v=y3VQmzzeyvc) --> 

## <a name="sap-on-azure-solutions"></a>Azure çözümlerinde SAP

Bu bölümde, Azure çözümlerinde SAP başvuruları sağlanmaktadır. 

### <a name="generic-sap-and-sap-netweaver"></a>Genel SAP ve SAP NetWeaver 

* [Azure NetApp Files kullanarak Microsoft Azure SAP uygulamaları](https://www.netapp.com/us/media/tr-4746.pdf)
* [SAP uygulamaları için Azure NetApp Files SUSE Linux Enterprise Server üzerindeki Azure VM 'lerinde SAP NetWeaver için yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-netapp-files)
* [SAP uygulamaları için Azure NetApp Files Red Hat Enterprise Linux üzerindeki Azure VM 'lerinde SAP NetWeaver için yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel-netapp-files)
* [SAP uygulamaları için Azure NetApp Files (SMB) ile Windows üzerinde Azure VM 'lerinde SAP NetWeaver için yüksek kullanılabilirlik](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-windows-netapp-files-smb)
* [SAP NetWeaver için Red Hat Enterprise Linux Azure VM 'lerde yüksek kullanılabilirlik çoklu SID Kılavuzu](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel-multi-sid)

### <a name="sap-hana"></a>SAP HANA 

* [SAP HANA Azure sanal makine depolama alanı yapılandırmaları](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-vm-operations-storage)
* [Red Hat Enterprise Linux Azure NetApp Files ile SAP HANA ölçeği yüksek kullanılabilirliği](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-high-availability-netapp-files-red-hat)
* [SUSE Linux Enterprise Server Azure NetApp Files ile Azure VM 'lerinde bekleme düğümüyle genişleme SAP HANA](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-scale-out-standby-netapp-files-suse)
* [Red Hat Enterprise Linux Azure NetApp Files ile Azure VM 'lerinde bekleme düğümüyle genişleme SAP HANA](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-scale-out-standby-netapp-files-rhel)

### <a name="sap-tech-community-and-blog-posts"></a>SAP teknoloji topluluğu ve blog gönderileri 

* [Azure NetApp Files – saniye cinsinden yedekleme SAP HANA](https://blog.netapp.com/azure-netapp-files-sap-hana-backup-in-seconds/)
* [Azure NetApp Files – HANA veritabanınızı bir anlık görüntü yedeklemesinden geri yükleme](https://blog.netapp.com/azure-netapp-files-backup-sap-hana)
* [Azure NetApp Files – yedeklemeyi bulut eşitlemeyle boşaltma SAP HANA](https://blog.netapp.com/azure-netapp-files-sap-hana)
* [Azure NetApp Files kullanarak SAP HANA sistem kopyalarınızı hızlandırın](https://blog.netapp.com/sap-hana-faster-using-azure-netapp-files/)
* [Bulut birimleri ONTAP ve Azure NetApp Files: SAP HANA sistem geçişi kolaylaştırıldı](https://blog.netapp.com/cloud-volumes-ontap-and-azure-netapp-files-sap-hana-system-migration-made-easy/)

## <a name="virtual-desktop-infrastructure-solutions"></a>Sanal masaüstü altyapısı çözümleri

Bu bölüm, sanal masaüstü altyapısı çözümlerine yönelik başvurular sağlar.

### <a name="windows-virtual-desktop"></a>Windows Sanal Masaüstü

* [Windows Sanal Masaüstü ile Azure NetApp Files kullanmanın avantajları](solutions-windows-virtual-desktop.md)
* [Windows sanal masaüstündeki FSLogix profil kapsayıcıları için depolama seçenekleri](https://docs.microsoft.com/azure/virtual-desktop/store-fslogix-profile#azure-platform-details)
* [Azure NetApp Files kullanarak bir konak havuzu için FSLogix profil kapsayıcısı oluşturma](https://docs.microsoft.com/azure/virtual-desktop/create-fslogix-profile-container)
* [Kurumsal ölçekte Windows Sanal Masaüstü](https://docs.microsoft.com/azure/architecture/example-scenario/wvd/windows-virtual-desktop)

## <a name="hpc-solutions"></a>HPC çözümleri

Bu bölümde, yüksek performanslı bilgi Işlem (HPC) çözümleri için başvurular sağlanmaktadır. 

### <a name="generic-hpc"></a>Genel HPC

* [Azure NetApp Files: bulut depolamalarınızdan en iyi şekilde yararlanmak](https://cloud.netapp.com/hubfs/Resources/ANF%20PERFORMANCE%20TESTING%20IN%20TEMPLATE.pdf)
* [Azure Batch ve Azure NetApp Files MPı iş yüklerini çalıştırma](https://azure.microsoft.com/resources/run-mpi-workloads-with-azure-batch-and-azure-netapp-files/)
* [Azure Cycle Cloud: Azure NetApp Files CycleCloud HPC ortamları](https://docs.microsoft.com/azure/cyclecloud/overview)

### <a name="oil-and-gas"></a>Petrol ve gaz

* [Yüksek performanslı bilgi işlem (HPC): Azure 'da yağ ve gaz](https://techcommunity.microsoft.com/t5/azure-global/high-performance-computing-hpc-oil-and-gas-in-azure/ba-p/824926)
* [Azure 'da rezervoır benzetim yazılımını çalıştırma](https://docs.microsoft.com/azure/architecture/example-scenario/infrastructure/reservoir-simulation)

### <a name="electronic-design-automation-eda"></a>Elektronik tasarım otomasyonu (EDA)

* [Elektronik tasarım otomasyonu için Azure NetApp Files kullanmanın avantajları](solutions-benefits-azure-netapp-files-electronic-design-automation.md)
* [Azure CycleCloud: Azure NetApp Files ile EDA HPC Laboratuvarı](https://github.com/Azure/cyclecloud-hands-on-labs/blob/master/EDA/README.md)

### <a name="analytics"></a>Analiz

* [Azure NetApp Files: Microsoft Azure SAS kılavuzuyla kullanılacak yeni bir paylaşılan dosya sistemi](https://communities.sas.com/t5/Architecture/Azure-NetApp-Files-A-new-shared-file-system-to-use-with-SAS-Grid/m-p/606978)

## <a name="azure-platform-services-solutions"></a>Azure platform hizmetleri çözümleri

Bu bölüm, Azure platform hizmetleri için çözümler sağlar. 

### <a name="azure-kubernetes-services-and-kubernetes"></a>Azure Kubernetes Hizmetleri ve Kubernetes

* [Azure NetApp Files Azure Kubernetes hizmeti ile tümleştirme](https://docs.microsoft.com/azure/aks/azure-netapp-files)
* [Azure NetApp Files ile Azure 'da bu dünya dışı Kubernetes performansı](https://cloud.netapp.com/blog/ma-anf-blg-configure-kubernetes-openshift)
* [Trident-kapsayıcılar için depolama Orchestrator](https://netapp-trident.readthedocs.io/en/stable-v20.04/kubernetes/operations/tasks/backends/anf.html)

### <a name="azure-batch"></a>Azure Batch

* [Azure Batch ve Azure NetApp Files MPı iş yüklerini çalıştırma](https://azure.microsoft.com/resources/run-mpi-workloads-with-azure-batch-and-azure-netapp-files/)
 
