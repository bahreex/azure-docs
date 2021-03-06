---
title: Azure SQL Database dynamic data masking | Microsoft docs
description: SQL Database dynamic data masking limits sensitive data exposure by masking it to non-privileged users
services: sql-database
documentationcenter: ''
author: ronitr
manager: jhubbard
editor: ''

ms.assetid: 4b36d78e-7749-4f26-9774-eed1120a9182
ms.service: sql-database
ms.custom: security
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: "On Demand"
ms.date: 03/09/2017
ms.author: ronitr; ronmat

---
# SQL Database dynamic data masking

SQL Database dynamic data masking limits sensitive data exposure by masking it to non-privileged users. 

Dynamic data masking helps prevent unauthorized access to sensitive data by enabling customers to designate how much of the sensitive data to reveal with minimal impact on the application layer. It’s a policy-based security feature that hides the sensitive data in the result set of a query over designated database fields, while the data in the database is not changed.

For example, a service representative at a call center may identify callers by several digits of their credit card number, but those data items should not be fully exposed to the service representative. A masking rule can be defined that masks all but the last four digits of any credit card number in the result set of any query. As another example, an appropriate data mask can be defined to protect personally identifiable information (PII) data, so that a developer can query production environments for troubleshooting purposes without violating compliance regulations.

## SQL Database dynamic data masking basics
You set up a dynamic data masking policy in the Azure portal by selecting the dynamic data masking operation in your SQL Database configuration blade or settings blade.

### Dynamic data masking permissions
Dynamic data masking can be configured by the Azure Database admin, server admin, or security officer roles.

### Dynamic data masking policy
* **SQL users excluded from masking** - A set of SQL users or AAD identities that get unmasked data in the SQL query results. Users with administrator privileges are always excluded from masking, and see the original data without any mask.
* **Masking rules** - A set of rules that define the designated fields to be masked and the masking function that is used. The designated fields can be defined using a database schema name, table name, and column name.
* **Masking functions** - A set of methods that control the exposure of data for different scenarios.

| Masking Function | Masking Logic |
| --- | --- |
| **Default** |**Full masking according to the data types of the designated fields**<br/><br/>• Use XXXX or fewer Xs if the size of the field is less than 4 characters for string data types (nchar, ntext, nvarchar).<br/>• Use a zero value for numeric data types (bigint, bit, decimal, int, money, numeric, smallint, smallmoney, tinyint, float, real).<br/>• Use 01-01-1900 for date/time data types (date, datetime2, datetime, datetimeoffset, smalldatetime, time).<br/>• For SQL variant, the default value of the current type is used.<br/>• For XML the document <masked/> is used.<br/>• Use an empty value for special data types (timestamp table, hierarchyid, GUID, binary, image, varbinary spatial types). |
| **Credit card** |**Masking method, which exposes the last four digits of the designated fields** and adds a constant string as a prefix in the form of a credit card.<br/><br/>XXXX-XXXX-XXXX-1234 |
| **Email** |**Masking method, which exposes the first letter and replaces the domain with XXX.com** using a constant string prefix in the form of an email address.<br/><br/>aXX@XXXX.com |
| **Random number** |**Masking method, which generates a random number** according to the selected boundaries and actual data types. If the designated boundaries are equal, then the masking function is a constant number.<br/><br/>![Navigation pane](./media/sql-database-dynamic-data-masking-get-started/1_DDM_Random_number.png) |
| **Custom text** |**Masking method, which exposes the first and last characters** and adds a custom padding string in the middle. If the original string is shorter than the exposed prefix and suffix, only the padding string is used. <br/>prefix[padding]suffix<br/><br/>![Navigation pane](./media/sql-database-dynamic-data-masking-get-started/2_DDM_Custom_text.png) |

<a name="Anchor1"></a>

### Recommended fields to mask
The DDM recommendations engine, flags certain fields from your database as potentially sensitive fields, which may be good candidates for masking. In the Dynamic Data Masking blade in the portal, you will see the recommended columns for your database. All you need to do is click **Add Mask** for one or more columns and then **Save** to apply a mask for these fields.

## Set up dynamic data masking for your database using Powershell cmdlets
See [Azure SQL Database Cmdlets](https://msdn.microsoft.com/library/azure/mt574084.aspx).

## Set up dynamic data masking for your database using REST API
See [Operations for Azure SQL Databases](https://msdn.microsoft.com/library/dn505719.aspx).

