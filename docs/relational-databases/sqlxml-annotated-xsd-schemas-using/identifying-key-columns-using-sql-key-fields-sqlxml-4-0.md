---
title: 'Sql: キーフィールド (SQLXML) を使用してキー列を識別する'
description: 'キー列を識別するために XPath クエリで sql: キーフィールド注釈を指定することにより、SQLXML 4.0 クエリ結果に適切な入れ子を確実にする方法について説明します。'
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- nesting XML results
- proper nesting in results [SQLXML]
- sql:key-fields
- XSD schemas [SQLXML], key columns
- identifying key columns [SQLXML]
- annotated XSD schemas, key columns
- key columns [SQLXML]
- relationships [SQLXML], key columns
- hierarchical relationships [SQLXML]
- key-fields annotation
ms.assetid: 1a5ad868-8602-45c4-913d-6fbb837eebb0
author: MightyPen
ms.author: genemi
ms.reviewer: ''
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: fd8b23a2aaba27e166a3a13636e2362b54781704
ms.sourcegitcommit: da88320c474c1c9124574f90d549c50ee3387b4c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/01/2020
ms.locfileid: "85764938"
---
# <a name="identifying-key-columns-using-sqlkey-fields-sqlxml-40"></a>sql:key-fields を使用した、キー列の指定 (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  XSD スキーマに対して XPath クエリを指定する場合、結果内に適切な入れ子を生成するには、多くの場合キー情報が必要です。 **Sql: キーフィールド**の注釈を指定すると、適切な階層が生成されるようにすることができます。  
  
> [!NOTE]  
>  適切な入れ子を確保するために、テーブルにマップされる要素には**sql: キーフィールド**を指定することをお勧めします。 作成される XML は、基になる結果セットの順序指定に影響を受けます。 **Sql: キーフィールド**が指定されていない場合は、生成された XML が正しく作成されていない可能性があります。  
  
 **Sql: キーフィールド**の値は、リレーションシップ内の行を一意に識別する列を識別します。 行を一意に識別するために複数の列が必要な場合、列の値はスペースで区切られます。  
  
 要素に要素と子要素の間に定義されているが含まれていても、親要素で指定されているテーブルの主キーを提供しない場合は、 **sql: キーフィールド**の注釈を使用する必要があり **\<sql:relationship>** ます。  
  
## <a name="examples"></a>使用例  
 次の例を使用した実際のサンプルを作成するには、特定の条件を満たす必要があります。 詳細については、「 [SQLXML の例を実行するための要件](../../relational-databases/sqlxml/requirements-for-running-sqlxml-examples.md)」を参照してください。  
  
### <a name="a-producing-the-appropriate-nesting-when-sqlrelationship-does-not-provide-sufficient-information"></a>A: \<sql:relationship>が十分な情報を提供しない場合に適切な入れ子を生成する  
 次の例は **、sql: キーフィールド**を指定する必要がある場所を示しています。  
  
 次のスキーマについて考えてみます。 スキーマでは、要素と要素の間の階層を指定します。この要素は、 **\<Order>** **\<Customer>** **\<Order>** 要素が親で **\<Customer>** あり、要素が子です。  
  
 この **\<sql:relationship>** タグは、親子関係を指定するために使用されます。 このタグでは、Sales.SalesOrderHeader テーブルの CustomerID を親キーとして識別し、Sales.Customer テーブルの子キー CustomerID を参照します。 に指定された情報 **\<sql:relationship>** は、親テーブル (SalesOrderHeader) 内の行を一意に識別するのに十分ではありません。 このため、 **sql: キーフィールド**の注釈がないと、生成される階層が不正確になります。  
  
 Sql: で指定された**キーフィールド**では、 **\<Order>** 注釈は親 (SalesOrderHeader テーブル) 内の行を一意に識別し、その子要素はその親の下に表示されます。  
  
 スキーマは次のようになります。  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
            xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
<xsd:annotation>  
  <xsd:appinfo>  
    <sql:relationship name="OrdCust"  
        parent="Sales.SalesOrderHeader"  
        parent-key="CustomerID"  
        child="Sales.Customer"  
        child-key="CustomerID" />  
  </xsd:appinfo>  
</xsd:annotation>  
  <xsd:element name="Order" sql:relation="Sales.SalesOrderHeader"   
               sql:key-fields="SalesOrderID">  
   <xsd:complexType>  
     <xsd:sequence>  
     <xsd:element name="Customer" sql:relation="Sales.Customer"   
                       sql:relationship="OrdCust"  >  
       <xsd:complexType>  
         <xsd:attribute name="CustID" sql:field="CustomerID" />  
         <xsd:attribute name="SoldBy" sql:field="SalesPersonID" />  
       </xsd:complexType>  
     </xsd:element>  
     </xsd:sequence>  
     <xsd:attribute name="SalesOrderID" type="xsd:integer" />  
     <xsd:attribute name= "CustomerID" type="xsd:string" />  
    </xsd:complexType>  
  </xsd:element>  
</xsd:schema>  
```  
  
##### <a name="to-create-a-working-sample-of-this-schema"></a>このスキーマの実際のサンプルを作成するには  
  
1.  上のスキーマのコードをコピーして、テキスト ファイルに貼り付け、 KeyFields1.xml として保存します。  
  
2.  次のテンプレートをコピーして、テキスト ファイルに貼り付け、 KeyFields1.xml を保存したディレクトリに KeyFields1T.xml として保存します。 このテンプレートの XPath クエリでは、CustomerID が3未満のすべての要素が返され **\<Order>** ます。  
  
    ```  
    <ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
        <sql:xpath-query mapping-schema="KeyFields1.xml">  
            /Order[@CustomerID < 3]  
        </sql:xpath-query>  
    </ROOT>  
    ```  
  
     マッピング スキーマ (KeyFields1.xml) に指定するディレクトリ パスは、テンプレートを保存するディレクトリに対する相対パスです。 次のように、絶対パスを指定することもできます。  
  
    ```  
    mapping-schema="C:\MyDir\KeyFields1.xml"  
    ```  
  
3.  SQLXML 4.0 テスト スクリプト (sqlxml4test.vbs) を作成し、それを使用してテンプレートを実行します。  

     詳細については、「ADO を使用した[SQLXML クエリの実行](../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md)」を参照してください。  
  
 結果セットの一部を次に示します。  
  
```  
<ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
    <Order SalesOrderID="43860" CustomerID="1">  
       <Customer CustID="1" SoldBy="280"/>  
    </Order>  
    <Order SalesOrderID="44501" CustomerID="1">  
       <Customer CustID="1" SoldBy="280"/>  
    </Order>  
    <Order SalesOrderID="45283" CustomerID="1">  
       <Customer CustID="1" SoldBy="280"/>  
    </Order>  
    .....  
</ROOT>  
```  
  
### <a name="b-specifying-sqlkey-fields-to-produce-proper-nesting-in-the-result"></a>B: sql:key-fields を指定して結果内に適切な入れ子を生成する  
 次のスキーマでは、を使用して階層が指定されていません **\<sql:relationship>** 。 スキーマでは、Humanresources.employee テーブル内の従業員を一意に識別するために、 **sql: キーフィールド**の注釈を指定する必要があります。  
  
```  
<xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
            xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
  <xsd:element name="HumanResources.Employee" sql:key-fields="EmployeeID" >  
   <xsd:complexType>  
     <xsd:sequence>  
        <xsd:element name="Title">  
          <xsd:complexType>  
            <xsd:simpleContent>  
              <xsd:extension base="xsd:string">  
                 <xsd:attribute name="EmployeeID" type="xsd:integer" />  
              </xsd:extension>  
            </xsd:simpleContent>  
          </xsd:complexType>  
        </xsd:element>  
     </xsd:sequence>  
   </xsd:complexType>  
  </xsd:element>  
</xsd:schema>  
```  
  
##### <a name="to-create-a-working-sample-of-this-schema"></a>このスキーマの実際のサンプルを作成するには  
  
1.  上のスキーマのコードをコピーして、テキスト ファイルに貼り付け、 KeyFields2.xml として保存します。  
  
2.  次のテンプレートをコピーして、テキスト ファイルに貼り付け、 KeyFields2.xml を保存したディレクトリに KeyFields2T.xml として保存します。 このテンプレートの XPath クエリでは、すべての要素が返され **\<HumanResources.Employee>** ます。  
  
    ```  
    <ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
      <sql:xpath-query mapping-schema="KeyFields2.xml">  
        /HumanResources.Employee  
      </sql:xpath-query>  
    </ROOT>  
    ```  
  
     マッピング スキーマ (KeyFields2.xml) に指定するディレクトリ パスは、テンプレートを保存するディレクトリに対する相対パスです。 次のように、絶対パスを指定することもできます。  
  
    ```  
    mapping-schema="C:\MyDir\KeyFields2.xml"  
    ```  
  
3.  SQLXML 4.0 テスト スクリプト (sqlxml4test.vbs) を作成し、それを使用してテンプレートを実行します。  
  
     詳細については、「ADO を使用した[SQLXML クエリの実行](../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md)」を参照してください。  
  
 結果を次に示します。  
  
```  
<ROOT xmlns:sql="urn:schemas-microsoft-com:xml-sql">  
  <HumanResources.Employee>  
    <Title EmployeeID="1">Production Technician - WC60</Title>   
  </HumanResources.Employee>  
  <HumanResources.Employee>  
    <Title EmployeeID="2">Marketing Assistant</Title>   
  </HumanResources.Employee>  
  <HumanResources.Employee>  
    <Title EmployeeID="3">Engineering Manager</Title>   
  </HumanResources.Employee>  
  ...  
</ROOT>  
```  
  
  
