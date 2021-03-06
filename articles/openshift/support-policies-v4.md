---
title: Azure Red Hat OpenShift 4-kluster stöd princip
description: Förstå support policy krav för Red Hat OpenShift 4.
author: sakthi-vetrivel
ms.author: suvetriv
ms.service: container-service
ms.topic: conceptual
ms.date: 04/24/2020
ms.openlocfilehash: f496d0f38452fa7cf64bc9eef370bd0b2116cfa5
ms.sourcegitcommit: 8a7b82de18d8cba5c2cec078bc921da783a4710e
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/28/2020
ms.locfileid: "89049969"
---
# <a name="azure-red-hat-openshift-support-policy"></a>Support policy för Azure Red Hat OpenShift

Vissa konfigurationer för Azure Red Hat OpenShift 4-kluster kan påverka klustrets support. Azure Red Hat OpenShift 4 gör det möjligt för kluster administratörer att göra ändringar i interna kluster komponenter, men alla ändringar stöds inte. Support principen nedan delar vilka ändringar som strider mot principen och makulerar support från Microsoft och Red Hat.

> [!NOTE]
> Funktioner som har marker ATS som Technology Preview i OpenShift container Platform stöds inte i Azure Red Hat OpenShift.

## <a name="cluster-configuration-requirements"></a>Krav för kluster konfiguration

* Alla kluster operatörer för OpenShift måste vara i ett hanterat tillstånd. Listan över kluster operatörer kan returneras genom att köra `oc get clusteroperators` .
* Ta inte bort eller ändra klustrets Prometheus-och Alertmanager-tjänster.
* Ta inte bort Alertmanager-regler för tjänsten.
* Ta inte bort eller ändra Azure Red Hat OpenShift service Logging (mdsd poddar).
* Ta inte bort eller ändra pull-hemligheten för arosvc.azurecr.io-klustret.
* Alla virtuella kluster datorer måste ha direkt utgående Internet åtkomst, minst Azure Resource Manager (ARM) och service Logging (Genève).  Ingen form av HTTPS-proxyering stöds.
* Ändra inte DNS-konfigurationen för klustrets virtuella nätverk. Standard Azure DNS matchare måste användas.
* Åsidosätt inte något av klustrets MachineConfig-objekt (till exempel kubelet-konfigurationen) på något sätt.
* Tjänsten Azure Red Hat OpenShift har åtkomst till ditt kluster via tjänsten Private Link.  Ta inte bort eller ändra tjänst åtkomst.
* RHCOS Compute-noder stöds inte. Du kan till exempel inte använda en RHEL Compute-nod.

## <a name="supported-virtual-machine-sizes"></a>Storlekar för virtuella datorer som stöds

Azure Red Hat OpenShift 4 har stöd för Work Node-instanser på följande storlekar för virtuella datorer:

### <a name="general-purpose"></a>Generellt syfte

|Serie|Storlek|Virtuell processor|Minne: GiB|
|-|-|-|-|
|Dasv4|Standard_D4as_v4|4|16|
|Dasv4|Standard_D8as_v4|8|32|
|Dasv4|Standard_D16as_v4|16|64|
|Dasv4|Standard_D32as_v4|32|128|
|Dsv3|Standard_D4s_v3|4|16|
|Dsv3|Standard_D8s_v3|8|32|
|Dsv3|Standard_D16s_v3|16|64|
|Dsv3|Standard_D32s_v3|32|128|

### <a name="memory-optimized"></a>Minnesoptimerad

|Serie|Storlek|Virtuell processor|Minne: GiB|
|-|-|-|-|
|Esv3|Standard_E4s_v3|4|32|
|Esv3|Standard_E8s_v3|8|64|
|Esv3|Standard_E16s_v3|16|128|
|Esv3|Standard_E32s_v3|32|256|

### <a name="compute-optimized"></a>Beräkningsoptimerad

|Serie|Storlek|Virtuell processor|Minne: GiB|
|-|-|-|-|
|Fsv2|Standard_F4s_v2|4|8|
|Fsv2|Standard_F8s_v2|8|16|
|Fsv2|Standard_F16s_v2|16|32|
|Fsv2|Standard_F32s_v2|32|64|

### <a name="master-nodes"></a>Huvudnoder

|Serie|Storlek|Virtuell processor|Minne: GiB|
|-|-|-|-|
|Dsv3|Standard_D8s_v3|8|32|
|Dsv3|Standard_D16s_v3|16|64|
|Dsv3|Standard_D32s_v3|32|128|
