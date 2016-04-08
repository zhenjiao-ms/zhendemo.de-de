---
title: Unterst&#252;tzte Datentypen
ms.custom: na
ms.reviewer: na
ms.service: powerbi
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7f2e7b32-4da8-4722-9b22-2940afd891c5
---
# Unterst&#252;tzte Datentypen
---

Die Power BI-REST-API verfügt über die folgenden unterstützten [EDM](http://msdn.microsoft.com/en-us/library/vstudio/ee382832.aspx)-Datentypen und Einschränkungen:

<table>
  <tr>
    <td>
      <b>Datentyp</b>
    </td>
    <td>
      <b>Einschränkungen</b>
    </td>
  </tr>
  <tr>
    <td>Int64</td>
    <td>Int64.MaxValue und Int64.MinValue sind nicht zulässig.</td>
  </tr>
  <tr>
    <td>Double</td>
    <td>Die Werte Double.MaxValue und Double.MinValue sind nicht zulässig. NaN wird nicht unterstützt. +Infinity und -Infinity werden bei einigen Funktionen (z. B. Min, Max) nicht unterstützt.</td>
  </tr>
  <tr>
    <td>Boolesch</td>
    <td> Keine</td>
  </tr>
  <tr>
    <td>Datetime</td>
    <td>Beim Laden von Daten werden Werte mit Bruchteilen von Tagen auf ganze Vielfache von 1/300 Sekunden (3,33 ms) quantisiert.</td>
  </tr>
  <tr>
    <td>Zeichenfolge</td>
    <td>Derzeit sind bis zu 128.000 Zeichen zulässig.</td>
  </tr>
</table>


