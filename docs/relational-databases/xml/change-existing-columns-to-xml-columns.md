---
title: "Change Existing Columns to XML Columns | Microsoft Docs"
description: Learn how to use the ALTER TABLE statement to change a string type column to an xml data type column.
ms.custom: "fresh2019may"
ms.date: "05/22/2019"
ms.prod: sql
ms.prod_service: "database-engine"
ms.reviewer: ""
ms.technology: xml
ms.topic: conceptual
helpviewer_keywords: 
  - "tables [XML]"
ms.assetid: 0d951424-9862-41fe-bd46-127f1c059bcb
author: MikeRayMSFT
ms.author: mikeray
---
# Change Existing Columns to XML Columns

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

The ALTER TABLE statement supports the **xml** data type. For example, you can alter any string type column to the **xml** data type. Note that in these cases, the documents contained in the column must be well formed. Also, if you are changing the type of the column from string to typed xml, the documents in the column are validated against the specified XSD schemas.  
  
```sql
CREATE TABLE T (Col1 int primary key, Col2 nvarchar(max));
GO  
INSERT INTO T   
  VALUES (1, '<Root><Product ProductID="1"/></Root>');
GO  
ALTER TABLE T   
  ALTER COLUMN Col2 xml;
```  
  
You can change an `xml` type column from untyped XML to typed XML. For example:  
  
```sql
CREATE TABLE T (Col1 int primary key, Col2 xml);
GO  
INSERT INTO T   
  values (1, '<p1:ProductDescription ProductModelID="1"   
xmlns:p1="https://schemas.microsoft.com/sqlserver/2004/07/adventure-works/ProductModelDescription">  
            </p1:ProductDescription>');
GO   
-- Make it a typed xml column by specifying a schema collection.  
ALTER TABLE T   
  ALTER COLUMN Col2 xml (Production.ProductDescriptionSchemaCollection);
```  
  
> [!NOTE]  
> The script will run against [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] database, because the XML schema collection, `Production.ProductDescriptionSchemaCollection`, is created as part of the [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] database.  
  
 In the previous example, all the instances stored in the column are validated and typed against the XSD schemas in the specified collection. If the column contains one or more XML instances that are invalid with regard to the specified schema, the `ALTER TABLE` statement will fail and you will not be able to change your untyped XML column into typed XML.  
  
> [!NOTE]  
> If a table is large, modifying an **xml** type column can be costly. This is because each document must be checked for being well formed and, for typed XML, must also be validated.  
  
For more information about typed XML, see [Compare Typed XML to Untyped XML](../../relational-databases/xml/compare-typed-xml-to-untyped-xml.md).  
